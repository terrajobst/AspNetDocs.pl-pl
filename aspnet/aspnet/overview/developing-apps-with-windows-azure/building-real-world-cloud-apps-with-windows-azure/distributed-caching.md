---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/distributed-caching
title: Rozproszone buforowanie (Tworzenie aplikacji w chmurze w rzeczywistych warunkach na platformie Azure) | Microsoft Docs
author: MikeWasson
description: Tworzenie aplikacji w chmurze w świecie rzeczywistym za pomocą książki elektronicznej platformy Azure jest oparte na prezentacji opracowanej przez Scott Guthrie. Wyjaśniono 13 wzorców i praktyk, które mogą...
ms.author: riande
ms.date: 07/20/2015
ms.assetid: 406518e9-3817-49ce-8b90-e82bc461e2c0
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/distributed-caching
msc.type: authoredcontent
ms.openlocfilehash: c66187b990a828c53bd2f8115e3c9660fc6022ed
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74582811"
---
# <a name="distributed-caching-building-real-world-cloud-apps-with-azure"></a>Rozproszone buforowanie (Tworzenie aplikacji w chmurze w rzeczywistych warunkach na platformie Azure)

przez [Jan Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tomasz Dykstra](https://github.com/tdykstra)

[Pobierz poprawkę](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) lub [Pobierz książkę elektroniczną](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Tworzenie aplikacji w chmurze w świecie rzeczywistym za pomocą książki elektronicznej platformy Azure** jest oparte na prezentacji opracowanej przez Scott Guthrie. Wyjaśniono 13 wzorców i praktyk, które mogą pomóc w pomyślnym tworzeniu aplikacji sieci Web dla chmury. Aby uzyskać informacje na temat książki elektronicznej, zobacz [pierwszy rozdział](introduction.md).

W poprzednim rozdziale sprawdzono przejściową obsługę błędów i wymieniono buforowanie jako strategię wyłącznika. Ten rozdział zawiera więcej informacji o pamięci podręcznej, w tym o tym, kiedy używać jej, wspólnych wzorców do ich używania oraz jak wdrożyć ją na platformie Azure.

## <a name="what-is-distributed-caching"></a>Co to jest buforowanie rozproszone

Pamięć podręczna zapewnia wysoką przepływność, dostęp o małym opóźnieniu do często używanych danych aplikacji, przechowując dane w pamięci. W przypadku aplikacji w chmurze najbardziej przydatny typ pamięci podręcznej jest rozproszonej pamięci podręcznej, co oznacza, że dane nie są przechowywane w pamięci pojedynczego serwera sieci Web, ale w innych zasobach w chmurze, a dane w pamięci podręcznej są udostępniane wszystkim serwerom sieci Web aplikacji (lub innym maszynom wirtualnym w chmurze, które zostały AR używany przez aplikację).

![Diagram przedstawiający wiele serwerów sieci Web uzyskujących dostęp do tych samych serwerów pamięci podręcznej](distributed-caching/_static/image1.png)

Gdy aplikacja jest skalowana przez dodawanie lub usuwanie serwerów lub gdy serwery są zastępowane ze względu na uaktualnienia lub błędy, dane przechowywane w pamięci podręcznej są dostępne dla każdego serwera z uruchomioną aplikacją.

Unikanie dostępu do danych w trwałym magazynie danych o dużym opóźnieniu może znacznie zwiększyć czas odpowiedzi aplikacji. Na przykład pobieranie danych z pamięci podręcznej jest znacznie szybsze niż pobieranie jej z relacyjnej bazy danych.

Zaletą korzystania z pamięci podręcznej jest zredukowany ruch do trwałego magazynu danych, co może spowodować obniżenie kosztów w przypadku wyłączania danych dla trwałego magazynu danych.

## <a name="when-to-use-distributed-caching"></a>Kiedy należy używać rozproszonej pamięci podręcznej

Buforowanie działa najlepiej w przypadku obciążeń aplikacji, które zwiększają czytelność niż zapis danych i kiedy model danych obsługuje organizację klucz/wartość, która jest używana do przechowywania i pobierania danych w pamięci podręcznej. Jest to również przydatne, gdy użytkownicy aplikacji korzystają z wielu wspólnych danych. na przykład pamięć podręczna nie zapewni tylu korzyści, jeśli każdy użytkownik zwykle pobiera dane unikatowe dla danego użytkownika. Przykładem, gdzie buforowanie może być bardzo korzystne, jest katalogiem produktów, ponieważ dane nie ulegają częstym zmianom, a wszyscy klienci przeglądają te same dane.

Zaletą buforowania staje się coraz bardziej wymierną skalą aplikacji, ponieważ limity przepływności i opóźnienia opóźnień trwałego magazynu danych stają się bardziej ograniczeniem ogólnej wydajności aplikacji. Można jednak zaimplementować buforowanie z innych powodów niż wydajność. W przypadku danych, które nie muszą być idealnie aktualne, jeśli są widoczne dla użytkownika, dostęp do pamięci podręcznej może służyć jako wyłącznik, gdy trwały magazyn danych nie odpowiada lub jest niedostępny.

## <a name="popular-cache-population-strategies"></a>Popularne strategie wypełniania pamięci podręcznej

Aby można było pobrać dane z pamięci podręcznej, należy najpierw ją zapisać. Istnieje kilka strategii pobierania danych, które są potrzebne w pamięci podręcznej:

- Na żądanie/w pamięci podręcznej

    Aplikacja próbuje pobrać dane z pamięci podręcznej, a gdy pamięć podręczna nie ma danych ("chybień"), aplikacja przechowuje dane w pamięci podręcznej tak, aby były one dostępne następnym razem. Następnym razem, gdy aplikacja próbuje uzyskać te same dane, znajdzie to, czego szuka w pamięci podręcznej ("trafienie"). Aby uniemożliwić pobieranie danych z pamięci podręcznej, które uległy zmianie w bazie danych, należy unieważnić pamięć podręczną podczas wprowadzania zmian w magazynie danych.
- Wypychanie danych w tle

    Usługi w tle wypychanie danych do pamięci podręcznej zgodnie z regularnym harmonogramem, a aplikacja zawsze ściąga z pamięci podręcznej. Takie podejście doskonale sprawdza się w przypadku źródeł danych o dużym opóźnieniu, które nie wymagają, aby zawsze zwracały najnowsze dane.
- Wyłącznik

    Aplikacja zwykle komunikuje się bezpośrednio z trwałym magazynem danych, ale gdy trwały magazyn danych ma problemy z dostępnością, aplikacja pobiera dane z pamięci podręcznej. Dane mogły zostać umieszczone w pamięci podręcznej przy użyciu funkcji bezdyskowej lub strategii wypychania danych w tle. Jest to strategia obsługi błędów, a nie strategia ulepszania wydajności.

Aby zapewnić aktualność danych w pamięci podręcznej, można usunąć powiązane wpisy pamięci podręcznej, gdy aplikacja tworzy, aktualizuje lub usuwa dane. Jeśli jest porządku, aby aplikacja mogła czasami pobrać dane, które są nieaktualne, możesz polegać na konfigurowalnym czasie wygaśnięcia, aby ustawić limit, w jaki sposób mogą być używane stare dane pamięci podręcznej.

Można skonfigurować bezwzględne wygaśnięcie (ilość czasu od utworzenia elementu pamięci podręcznej) lub jego wygaśnięcie (ilość czasu od momentu ostatniego dostępu do elementu pamięci podręcznej). Bezwzględne wygaśnięcie jest używane w zależności od mechanizmu wygaśnięcia pamięci podręcznej, aby zapobiec utracie danych. W aplikacji Fix it ręcznie wykluczasz stare elementy pamięci podręcznej, a my użyjemy okresowego wygaśnięcia, aby zachować najnowsze dane w pamięci podręcznej. Niezależnie od wybranych zasad wygasania pamięć podręczna będzie automatycznie wykluczać najstarsze (ostatnio używane lub LRU) elementy, gdy zostanie osiągnięty limit pamięci pamięci podręcznej.

## <a name="sample-cache-aside-code-for-fix-it-app"></a>Przykładowy kod w pamięci podręcznej dla aplikacji poprawki IT

W poniższym przykładowym kodzie najpierw Sprawdzamy pamięć podręczną podczas pobierania zadania poprawki. Jeśli zadanie zostanie odnalezione w pamięci podręcznej, zostanie zwrócone. Jeśli nie zostanie znaleziona, pobieramy ją z bazy danych i zapisujemy ją w pamięci podręcznej. Zmiany wprowadzone w celu dodania buforowania do metody `FindTaskByIdAsync` są wyróżnione.

[!code-csharp[Main](distributed-caching/samples/sample1.cs?highlight=5,9-11,13-15,19)]

W przypadku aktualizowania lub usuwania tego zadania należy unieważnić (usunąć) buforowane zadanie. W przeciwnym razie przyszłe próby odczytania tego zadania będą nadal pobierać stare dane z pamięci podręcznej.

[!code-csharp[Main](distributed-caching/samples/sample2.cs?highlight=7)]

Są to przykłady umożliwiające zilustrowanie prostego kodu buforowania; buforowanie nie zostało zaimplementowane w projekcie poprawki do pobrania.

## <a name="azure-caching-services"></a>Usługi buforowania platformy Azure

Platforma Azure oferuje następujące usługi pamięci podręcznej: [Azure Redis Cache](https://msdn.microsoft.com/library/dn690523.aspx) i [Zarządzana pamięć podręczna platformy Azure](https://msdn.microsoft.com/library/dn386094.aspx). Pamięć podręczna Azure Redis jest oparta na popularnej [Redis Cache Open Source](http://redis.io/) i jest pierwszym wyborem w przypadku większości scenariuszy buforowania.

<a id="sessionstate"></a>
## <a name="aspnet-session-state-using-a-cache-provider"></a>ASP.NET stanu sesji przy użyciu dostawcy pamięci podręcznej

Jak wspomniano w [rozdziale najlepszych rozwiązań dotyczących projektowania sieci Web](web-development-best-practices.md), najlepszym rozwiązaniem jest unikanie korzystania z stanu sesji. Jeśli aplikacja wymaga stanu sesji, następnym najlepszym rozwiązaniem jest uniknięcie domyślnego dostawcy w pamięci, ponieważ nie jest włączony skalowanie w poziomie (wiele wystąpień serwera sieci Web). Dostawca stanu sesji ASP.NET SQL Server umożliwia lokację działającą na wielu serwerach sieci Web do korzystania z stanu sesji, ale wiąże się z wysokim kosztem opóźnienia w porównaniu z dostawcą w pamięci. Najlepszym rozwiązaniem w przypadku korzystania z stanu sesji jest użycie dostawcy pamięci podręcznej, takiego jak [dostawca stanu sesji dla pamięci podręcznej platformy Azure](https://msdn.microsoft.com/library/windowsazure/gg185668.aspx).

## <a name="summary"></a>Podsumowanie

Dowiesz się, jak poprawka aplikacji IT może zaimplementować buforowanie, aby zwiększyć czas reakcji i skalowalność oraz umożliwić, aby aplikacja kontynuowała reagowanie na operacje odczytu, gdy baza danych jest niedostępna. W [następnym rozdziale](queue-centric-work-pattern.md) pokazano, jak dodatkowo zwiększyć skalowalność i zapewnić, że aplikacja będzie nadal odpowiadać za operacje zapisu.

## <a name="resources"></a>Resources

Aby uzyskać więcej informacji na temat buforowania, zobacz następujące zasoby.

Dokumentacja

- [Pamięć podręczna platformy Azure](https://msdn.microsoft.com/library/gg278356.aspx). Oficjalna dokumentacja MSDN dotycząca buforowania na platformie Azure.
- [Wzorce i praktyki firmy Microsoft — wskazówki dotyczące platformy Azure](https://msdn.microsoft.com/library/dn568099.aspx). Zobacz wskazówki dotyczące buforowania i wzorzec z odkładaniem do pamięci podręcznej.
- [Failsafe: wskazówki dotyczące odpornych architektur chmurowych](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx). Oficjalny dokument według wytłoczyn Mercuri, Ulrich Homann i Andrew TOWNHILL. Zapoznaj się z sekcją w temacie buforowanie.
- [Najlepsze rozwiązania dotyczące projektowania usług na dużą skalę w usłudze Azure Cloud Services](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). K. Oficjalny dokument ze znakami SIMM i Michael Thomassy. Zapoznaj się z sekcją w rozproszonej pamięci podręcznej.
- [Rozproszone buforowanie na ścieżce do skalowalności](https://msdn.microsoft.com/magazine/dd942840.aspx). Starsza wersja artykułu usługi MSDN Magazine (2009), ale wyraźnie Zapisano wprowadzenie do rozproszonej pamięci podręcznej. Więcej informacji niż sekcje pamięci podręcznej FailSafe i najlepszych rozwiązań.

Wideo

- [Failsafe: kompilowanie skalowalnych, Odpornych Cloud Services](https://channel9.msdn.com/Series/FailSafe). Seria dziewięciu części przez Ulrich Homann, Marc Mercuri i marking SIMM. Przedstawia widok 400 na poziomie tworzenia architektury aplikacji w chmurze. Ta seria koncentruje się na teorii i przyczynach: Aby uzyskać więcej informacji na temat szczegółów, zobacz Tworzenie dużych serii przez znaczniki SIMM. Zobacz Omówienie buforowania w epizod 3, zaczynając od 1:24:14.
- [Tworzenie dużych: lekcje uzyskane od klientów platformy Azure — część I](https://channel9.msdn.com/Events/Build/2012/3-029). Simon Davies omawia rozproszone buforowanie, zaczynając od 46:00. Podobnie jak w przypadku serii failsafe, ale więcej szczegółów. Prezentacja została wydana 31 października 2012, dlatego nie obejmuje ona usługi buforowania Web Apps w Azure App Service, która została wprowadzona w 2013.

Przykładowy kod

- [Podstawy usługi w chmurze na platformie Azure](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Przykładowa aplikacja implementująca rozproszone buforowanie. Zapoznaj się z towarzyszącym wpisem w blogu [podstawowe usługi w chmurze — podstawowe informacje o buforowaniu](https://blogs.msdn.com/b/windowsazure/archive/2013/10/03/cloud-service-fundamentals-caching-basics.aspx).

> [!div class="step-by-step"]
> [Poprzednie](transient-fault-handling.md)
> [dalej](queue-centric-work-pattern.md)

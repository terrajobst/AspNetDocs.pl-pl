---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling
title: Obsługa błędów przejściowych (Tworzenie aplikacji w chmurze w rzeczywistych warunkach na platformie Azure) | Microsoft Docs
author: MikeWasson
description: Tworzenie aplikacji w chmurze w świecie rzeczywistym za pomocą książki elektronicznej platformy Azure jest oparte na prezentacji opracowanej przez Scott Guthrie. Wyjaśniono 13 wzorców i praktyk, które mogą...
ms.author: riande
ms.date: 11/03/2015
ms.assetid: 7ead83bc-c08c-4b26-8617-00e07292e35c
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling
msc.type: authoredcontent
ms.openlocfilehash: e798cb83cfb97db63fef6dc38c8f62804461d01b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78617328"
---
# <a name="transient-fault-handling-building-real-world-cloud-apps-with-azure"></a>Obsługa błędów przejściowych (Tworzenie aplikacji w chmurze w rzeczywistych warunkach na platformie Azure)

przez [Jan Wasson](https://github.com/MikeWasson), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tomasz Dykstra](https://github.com/tdykstra)

[Pobierz poprawkę](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) lub [Pobierz książkę elektroniczną](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Tworzenie aplikacji w chmurze w świecie rzeczywistym za pomocą książki elektronicznej platformy Azure** jest oparte na prezentacji opracowanej przez Scott Guthrie. Wyjaśniono 13 wzorców i praktyk, które mogą pomóc w pomyślnym tworzeniu aplikacji sieci Web dla chmury. Aby uzyskać informacje na temat książki elektronicznej, zobacz [pierwszy rozdział](introduction.md).

Podczas projektowania rzeczywistej aplikacji w chmurze świecie należy wziąć pod uwagę, jak obsługiwać tymczasowe przerwy w świadczeniu usług. Ten problem jest jednoznacznie ważny w aplikacjach w chmurze, ponieważ jest to zależne od połączeń sieciowych i usług zewnętrznych. Często można uzyskać niewielkie problemy, które są zwykle samonaprawiane, a jeśli nie są one przygotowane do ich inteligentnego obsłużenia, spowodują niewłaściwe środowisko dla klientów.

## <a name="causes-of-transient-failures"></a>Przyczyny błędów przejściowych

W środowisku chmury można okresowo wyszukiwać nieudane i opuszczone połączenia bazy danych. Jest to częściowo spowodowane tym, że przechodzą więcej modułów równoważenia obciążenia w porównaniu do środowiska lokalnego, w którym serwer sieci Web i serwer bazy danych mają bezpośrednie połączenie fizyczne. Ponadto czasami, gdy użytkownik korzysta z usługi wielodostępnej, zobaczysz, że wywołania usługi są wolniejsze lub przekroczą limit czasu, ponieważ ktoś inny korzystający z usługi jest w dużym stopniu. W innych przypadkach może być użytkownikiem, który jest zbyt często, a usługa zaświadomie ogranicza użytkownika — w celu uniemożliwienia niekorzystnego wpływu na innych dzierżawców usługi.

## <a name="use-smart-retryback-off-logic-to-mitigate-the-effect-of-transient-failures"></a>Użyj inteligentnej logiki ponawiania/wycofywania, aby zmniejszyć wpływ błędów przejściowych

Zamiast zgłaszać wyjątek i wyświetlać stronę niedostępną lub błędną dla klienta, można rozpoznawać błędy, które zwykle są przejściowe, i automatycznie ponawiać próbę wykonania operacji, która spowodowała błąd, w nadziei, który przed długim zakończy się pomyślnie. Większość czasu operacja zakończy się pomyślnie w drugim kroku, a następnie odzyskasz sprawność po błędzie, jeśli wystąpił problem.

Istnieje kilka sposobów implementacji logiki inteligentnego ponawiania.

- Usługa Microsoft Patterns &amp; Practices ma [blok aplikacji do obsługi błędów przejściowych](https://msdn.microsoft.com/library/dn440719(v=pandp.60).aspx) , który wykonuje wszystko, jeśli używasz ADO.NET do SQL Database dostępu (nie za pośrednictwem Entity Framework). Po prostu ustawisz zasady dla ponownych prób — liczbę ponownych prób wykonania zapytania lub polecenia oraz czas oczekiwania między próbami — zawiń kod SQL w bloku *using* .

    [!code-csharp[Main](transient-fault-handling/samples/sample1.cs)]

    TFH obsługuje również [pamięć podręczna oparta na roli platformy Azure](https://msdn.microsoft.com/library/windowsazure/dn386103.aspx) i [Service Bus](https://azure.microsoft.com/services/service-bus/).
- W przypadku korzystania z Entity Framework zwykle nie pracuje bezpośrednio z połączeniami SQL, dlatego nie można używać tych wzorców i pakietu rozwiązań, ale Entity Framework 6 konstruuje ten rodzaj logiki ponowienia próby w strukturze. Podobnie jak w przypadku określenia strategii ponawiania prób, a następnie podczas uzyskiwania dostępu do bazy danych EF używa tej strategii.

    Aby korzystać z tej funkcji w aplikacji Fix it, należy dodać klasę pochodzącą od *dbconfiguration* i włączyć logikę ponawiania.

    [!code-csharp[Main](transient-fault-handling/samples/sample2.cs)]

    W przypadku SQL Database wyjątków, które są identyfikowane jako zwykle błędy przejściowe, pokazany kod instruuje program Dr, aby ponowić próbę wykonania operacji do 3 razy, z opóźnieniem wycofywania z powrotem między ponownymi próbami i maksymalnym opóźnieniem wynoszącym 5 sekund. Wycofanie wykładnicze oznacza, że po każdym niepomyślnym ponowieniu próby zostanie poczekać dłuższy okres czasu przed ponowieniem próby. Jeśli trzy próby w wierszu zakończą się niepowodzeniem, zgłosi wyjątek. W poniższej sekcji dotyczącej wyłączników wyjaśniono, dlaczego należy wykonać wycofywanie wykładnicze i ograniczoną liczbę ponownych prób.

    Możesz mieć podobne problemy podczas korzystania z usługi Azure Storage, ponieważ poprawka aplikacji IT dla obiektów blob, a interfejs API klienta magazynu .NET już implementuje ten sam rodzaj logiki. Należy tylko określić zasady ponawiania lub nawet wtedy, gdy są one zadowolony z ustawień domyślnych.

<a id="circuitbreakers"></a>
## <a name="circuit-breakers"></a>Wyłączniki

Istnieje kilka przyczyn, dla których nie chcesz ponowić zbyt wiele razy w zbyt długim czasie:

- Zbyt wielu użytkowników trwale ponawianie próby nieudanych żądań może obniżyć wydajność innych użytkowników. Jeśli wszyscy użytkownicy zajmują się ponownymi żądaniami ponowień, można nawiązać połączenia z kolejkami wysyłania usług IIS i uniemożliwić aplikacji obsługę żądań, które w przeciwnym razie mogą pomyślnie obsłużyć.
- Jeśli wszyscy są ponawiane z powodu błędu usługi, może to oznaczać, że usługa zostanie umieszczona w kolejce do momentu odzyskania usługi po jej rozpoczęciu.
- Jeśli ten błąd jest spowodowany ograniczeniem i istnieje okno czasu używane przez usługę do ograniczania przepustowości, dalsze ponowne próby mogą przełączać to okno i spowodować, że ograniczanie przepływności będzie kontynuowane.
- Może się zdarzyć, że użytkownik oczekuje na renderowanie strony sieci Web. Zaczekaj, aż ludzie będą zbyt długie, mogą być bardziej irytujące, aby stosunkowo szybko polecić ich ponowienie próby później.

Wycofaj wycofanie niektórych z tych problemów, ograniczając częstotliwość ponawiania prób, które usługa może pobrać z aplikacji. Ale musisz *również mieć wyłączniki*: oznacza to, że przy określonym progu ponawiania prób aplikacja przestanie ponawiać próbę i wykonuje inne czynności, takie jak następujące:

- Alternatywa niestandardowa. Jeśli nie możesz uzyskać ceny giełdowej z firmy Reuters, być może możesz uzyskać ją z Bloomberg; lub jeśli nie można pobrać danych z bazy danych, może być możliwe uzyskanie jej z pamięci podręcznej.
- Niepowodzenie dyskretne. Jeśli potrzebujesz z usługi, nie dotyczy to wszystkich elementów aplikacji, po prostu zwróć wartość null, jeśli nie możesz uzyskać danych. Jeśli jest wyświetlane zadanie poprawki IT, a Blob service nie odpowiada, można wyświetlić szczegóły zadania bez obrazu.
- Szybka praca awaryjna. Wystąpił błąd użytkownika w celu uniknięcia zapełnienia usługi żądaniami ponowień, co może spowodować przerwanie działania usługi dla innych użytkowników lub zwiększenie okna ograniczenia przepustowości. Możesz wyświetlić przyjazny komunikat "Spróbuj ponownie później".

Nie ma żadnej zasady ponawiania, która nie pasuje do wszystkich. Możesz ponowić próbę więcej razy i zaczekać dłużej w asynchronicznym procesie roboczym, niż w przypadku synchronicznej aplikacji sieci Web, w której użytkownik czeka na odpowiedź. Możesz poczekać dłużej między ponownymi próbami usługi relacyjnej bazy danych niż usługa pamięci podręcznej. Poniżej przedstawiono kilka przykładowych zalecanych zasad ponawiania prób w celu uzyskania pomysłu, jak liczby mogą się różnić. ("Szybkie pierwsze" oznacza brak opóźnień przed pierwszym ponowieniem próby.

![Przykładowe zasady ponawiania prób](transient-fault-handling/_static/image1.png)

Aby uzyskać SQL Database wskazówki dotyczące ponowienia zasad, zobacz [Rozwiązywanie błędów przejściowych i błędy połączeń, aby SQL Database](https://azure.microsoft.com/documentation/articles/sql-database-connectivity-issues/).

## <a name="summary"></a>Podsumowanie

Strategia ponawiania/wycofywania może pomóc w wykonywaniu tymczasowych błędów niewidocznych dla klientów, a firma Microsoft oferuje platformy, których można użyć w celu zminimalizowania pracy, w której jest używana usługa ADO.NET, Entity Framework lub Azure Storage.

W [następnym rozdziale](distributed-caching.md)zawarto informacje na temat poprawy wydajności i niezawodności przy użyciu rozproszonej pamięci podręcznej.

## <a name="resources"></a>Zasoby

Więcej informacji można znaleźć w następujących zasobach:

Dokumentacja

- [Najlepsze rozwiązania dotyczące projektowania usług na dużą skalę w usłudze Azure Cloud Services](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). Oficjalny dokument ze znakami SIMM i Michael Thomassy. Podobnie jak w przypadku serii failsafe, ale więcej szczegółów. Zapoznaj się z sekcją telemetrię i diagnostyka.
- [Failsafe: wskazówki dotyczące odpornych architektur chmurowych](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx). Oficjalny dokument według wytłoczyn Mercuri, Ulrich Homann i Andrew TOWNHILL. Wersja strony sieci Web serii wideo FailSafe.
- [Wzorce i praktyki firmy Microsoft — wskazówki dotyczące platformy Azure](https://msdn.microsoft.com/library/dn568099.aspx). Zobacz wzorzec ponawiania, wzorzec inspektora agenta harmonogramu.
- [Odporność na uszkodzenia w Azure SQL Database](https://blogs.msdn.com/b/windowsazure/archive/2012/07/30/fault-tolerance-in-windows-azure-sql-database.aspx). Wpis w blogu według której należy Tony Petrossian.
- [Entity Framework — odporność połączenia/logika ponowień](https://msdn.microsoft.com/data/dn456835). Jak korzystać z funkcji obsługi błędów przejściowych w programie Entity Framework 6 i dostosowywać ją.
- [Odporność połączeń i przechwycenie poleceń z Entity Framework w aplikacji ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md). Czwarta część serii samouczków z dziewięcioma częścią pokazuje, jak skonfigurować funkcję odporności połączeń Dr 6 dla SQL Database.

Filmy wideo

- [Failsafe: kompilowanie skalowalnych, Odpornych Cloud Services](https://channel9.msdn.com/Series/FailSafe). Seria dziewięciu części przez Ulrich Homann, Marc Mercuri i marking SIMM. Prezentuje koncepcje wysokiego poziomu i zasady architektury w bardzo dostępnym i interesującym scenariuszu, w tym scenariusze opracowane przez firmę Microsoft Customer Advisory Team (CAT) z rzeczywistymi klientami. Zobacz Omówienie wyłączników w epizod 3, zaczynając od 40:55.
- [Tworzenie dużych: lekcje uzyskane od klientów platformy Azure — część II](https://channel9.msdn.com/Events/Build/2012/3-030). Oznacz moduły SIMM rozmowy dotyczące projektowania pod kątem awarii, przejściowej obsługi błędów i Instrumentacji wszystkich elementów.

Przykład kodu

- [Podstawy usługi w chmurze na platformie Azure](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Przykładowa aplikacja utworzona przez zespół doradców klientów Microsoft Azure, który pokazuje, jak korzystać z [bloku obsługi błędów przejściowych biblioteki przedsiębiorstwa](http://nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/) (TFH). Aby uzyskać więcej informacji, zobacz [Cloud Service Fundamentals Data Access Layer – Transient Fault Handling](https://social.technet.microsoft.com/wiki/contents/articles/18665.cloud-service-fundamentals-data-access-layer-transient-fault-handling.aspx) (Warstwa dostępu do danych w aplikacji Cloud Service Fundamentals — obsługa błędów przejściowych). TFH jest zalecana w przypadku dostępu do bazy danych za pomocą ADO.NET bezpośrednio (bez używania Entity Framework).

> [!div class="step-by-step"]
> [Poprzednie](monitoring-and-telemetry.md)
> [dalej](distributed-caching.md)

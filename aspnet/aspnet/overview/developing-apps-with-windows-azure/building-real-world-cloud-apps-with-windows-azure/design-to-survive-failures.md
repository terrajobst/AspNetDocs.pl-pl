---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/design-to-survive-failures
title: Projektowanie pod kątem niepowodzeń (Tworzenie rzeczywistych aplikacji w chmurze na platformie Azure) | Microsoft Docs
author: MikeWasson
description: Tworzenie aplikacji w chmurze w świecie rzeczywistym za pomocą książki elektronicznej platformy Azure jest oparte na prezentacji opracowanej przez Scott Guthrie. Wyjaśniono 13 wzorców i praktyk, które mogą...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 364ce84e-5af8-4e08-afc9-75a512b01f84
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/design-to-survive-failures
msc.type: authoredcontent
ms.openlocfilehash: 348232af531b5d53dc3cb46d6d2c7931d95a572d
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457131"
---
# <a name="design-to-survive-failures-building-real-world-cloud-apps-with-azure"></a>Projektowanie pod kątem niepowodzeń (Tworzenie aplikacji w chmurze w rzeczywistych warunkach na platformie Azure)

przez [Jan Wasson](https://github.com/MikeWasson), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tomasz Dykstra](https://github.com/tdykstra)

[Pobierz poprawkę](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) lub [Pobierz książkę elektroniczną](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Tworzenie aplikacji w chmurze w świecie rzeczywistym za pomocą książki elektronicznej platformy Azure** jest oparte na prezentacji opracowanej przez Scott Guthrie. Wyjaśniono 13 wzorców i praktyk, które mogą pomóc w pomyślnym tworzeniu aplikacji sieci Web dla chmury. Aby uzyskać informacje na temat książki elektronicznej, zobacz [pierwszy rozdział](introduction.md).

Jednym z rzeczy, które należy wziąć pod uwagę podczas tworzenia aplikacji dowolnego typu, ale szczególnie tych, które będą działać w chmurze, w których będą używane przez wiele osób, można zaprojektować aplikację tak, aby mogła ona bezpiecznie obsłużyć awarie i w dalszym ciągu dostarczać wartość najmniejszy. Mając wystarczająco dużo czasu, elementy są niewłaściwe w środowisku lub dowolnym systemie oprogramowania. Sposób, w jaki aplikacja obsługuje te sytuacje, decyduje o tym, jak zakłócają klienci i ile czasu trzeba poświęcać na analizowanie i rozwiązywanie problemów.

## <a name="types-of-failures"></a>Typy błędów

Istnieją dwie podstawowe kategorie niepowodzeń, które mają być obsługiwane inaczej:

- Przejściowe błędy samonaprawiania, takie jak sporadyczne problemy z łącznością sieciową.
- Trwałe błędy wymagające interwencji.

W przypadku błędów przejściowych można zaimplementować zasady ponawiania, aby upewnić się, że przez większość czasu aplikacja będzie szybko i automatycznie odzyskiwana. Klienci mogą zauważyć nieco dłuższy czas odpowiedzi, ale w przeciwnym razie nie będzie to miało wpływu. Pokażemy kilka sposobów obsługi tych błędów w [rozdziale obsługi błędów przejściowych](transient-fault-handling.md).

W przypadku trwałych błędów można zaimplementować funkcje monitorowania i rejestrowania, które powiadamiają natychmiast o problemach i ułatwiają analizę głównych przyczyn. Pokażemy kilka sposobów, aby pomóc Ci w powyższym rodzaju błędach w [rozdziale monitorowanie i telemetrię](monitoring-and-telemetry.md).

## <a name="failure-scope"></a>Zakres niepowodzeń

Należy również wziąć pod uwagę zakres błędów — niezależnie od tego, czy dotyczy to pojedynczej maszyny, cała usługa, taka jak SQL Database czy magazyn lub cały region.

![Zakres niepowodzeń](design-to-survive-failures/_static/image1.png)

### <a name="machine-failures"></a>Awarie maszyn

Na platformie Azure serwer, który uległ awarii, jest automatycznie zastępowany nowym, a dobrze zaprojektowana aplikacja w chmurze jest automatycznie i szybka. Wcześniej wprowadziliśmy zalety skalowalności warstwy sieci Web bezstanowej i łatwość odzyskiwania z serwera, który uległ awarii, jest kolejną zaletą bezstanową. Łatwość odzyskiwania to również jedna z zalet funkcji platformy jako usługi (PaaS, Platform-as-a-Service), takich jak SQL Database i Azure App Service Web Apps. Awarie sprzętu są rzadkie, ale gdy występują te usługi, obsługują je automatycznie; nie trzeba nawet pisać kodu, aby obsłużyć awarie maszyn w przypadku korzystania z jednej z tych usług.

### <a name="service-failures"></a>Błędy usługi

Aplikacje w chmurze zwykle korzystają z wielu usług. Na przykład aplikacja do rozwiązywania problemów używa usługi SQL Database, usługi magazynu i aplikacji internetowej wdrożonej do Azure App Service. Co aplikacja będzie działać, jeśli jedna z usług, z których korzystasz, zakończy się niepowodzeniem? W przypadku niektórych błędów usługi niezrozumiały komunikat "Niestety, spróbuj ponownie później" może być najlepszym rozwiązaniem. Jednak w wielu scenariuszach można to zrobić lepiej. Na przykład jeśli magazyn danych zaplecza nie działa, można zaakceptować dane wejściowe użytkownika, wyświetlić komunikat "żądanie zostało odebrane" i zapisać dane wejściowe zwrócono komunikatu inaczej. gdy potrzebna usługa będzie ponownie działać, możesz pobrać dane wejściowe i przetworzyć je.

Rozdział [wzorca pracy skoncentrowanego na kolejce](queue-centric-work-pattern.md) pokazuje jeden ze sposobów obsługi tego scenariusza. Aplikacja Fix it przechowuje zadania w SQL Database, ale nie musi ona zamykać pracy, gdy SQL Database nie działa. W tym rozdziale zobaczymy, jak przechowywać dane wejściowe użytkownika dla zadania w kolejce, a następnie użyć procesu roboczego, aby odczytać kolejkę i zaktualizować zadanie. Jeśli program SQL nie działa, można utworzyć poprawkę, której nie dotyczy to zadanie. proces roboczy może czekać i przetwarzać nowe zadania, gdy SQL Database jest dostępna.

### <a name="region-failures"></a>Awarie regionów

Całe regiony mogą zakończyć się niepowodzeniem. Klęska żywiołowa może zniszczyć centrum danych, ale może zostać spłaszczona przez platforma Meteor, a linia magistrali do centrum dane może zostać zacięta przez rolnika, Burying krowę backhoe itd. Jeśli aplikacja jest hostowana w Stricken centrum danych? Istnieje możliwość skonfigurowania aplikacji na platformie Azure tak, aby działała w wielu regionach jednocześnie, tak aby w przypadku awarii w jednym regionie nadal działała. Takie niepowodzenia są bardzo rzadkimi wystąpieniami, a większość aplikacji nie przeskakuje przez przeszkód niezbędne do zapewnienia nieprzerwanej usługi za pomocą błędów tego sortowania. Zapoznaj się z sekcją resources na końcu rozdziału, aby uzyskać informacje o tym, jak utrzymać dostęp do aplikacji nawet przez awarię regionu.

Celem systemu Azure jest zapewnienie, że wszystkie te rodzaje błędów są znacznie łatwiejsze, a zobaczysz kilka przykładów tego, jak robimy to w następujących działach.

## <a name="slas"></a>Umowy SLA

Ludzie często słuchają umów dotyczących poziomu usług (umowy SLA) w środowisku chmury. Zasadniczo są to niesie obietnice zwiększenia, że firmy sprawiają, że ich usługi są niezawodne. Umowa SLA na 99,9% oznacza, że usługa powinna działać prawidłowo 99,9% czasu. Jest to dość typowa wartość umowy SLA, która jest taka sama jak w przypadku bardzo dużej liczby, ale może nie zachodzić, ile czasu w dół w procentach rzeczywiście wynosi. Poniżej znajduje się tabela, która pokazuje, ile przestojów różnej wartości procentowej umowy SLA ma wartość w ciągu roku, miesiąca i tygodnia.

![Tabela umów SLA](design-to-survive-failures/_static/image2.png)

W związku z tym umowa SLA na 99,9% oznacza, że usługa może być 8,76 wyłączona o godz. w roku lub 43,2 minut w miesiącu. To więcej czasu niż większość użytkowników. W związku z tym deweloper powinien mieć świadomość, że jest możliwy pewien czas i obsłużyć go w sposób płynny. W pewnym momencie ktoś będzie korzystać z aplikacji, a usługa zostanie wyłączona i chcesz zminimalizować negatywny wpływ tego działania na klienta.

Jednym z nich należy wiedzieć o umowie SLA, do której odnosi się czas: czy zegar jest resetowany co tydzień, co miesiąc czy w każdym roku? Na platformie Azure zresetuję zegar co miesiąc, co jest lepsze od rocznej umowy SLA, ponieważ Roczna umowa SLA może ukryć niewłaściwe miesiące, przenosząc je na serię dobrych miesięcy.

Oczywiście zawsze zależy się lepiej niż umowa SLA; zwykle znacznie mniej niż to możliwe. Obietnica polega na tym, że jeśli kiedykolwiek wyjdziesz dłużej niż maksymalny czas, w którym możesz poprosił o zwrot pieniędzy. Kwota pieniędzy, którą powrócisz, prawdopodobnie nie może w pełni wynagradzać użytkownika w celu skorzystania z nadmiarowego czasu, ale ten aspekt umowy SLA działa jako zasady wymuszania i informuje o tym, że bardzo poważnie zajmiemy się tym.

### <a name="composite-slas"></a>Złożone umowy SLA

Ważną kwestią, którą należy wziąć pod uwagę podczas korzystania z usługi umowy SLA, jest wpływ używania wielu usług w aplikacji, a każda z nich ma odrębną umowę SLA. Na przykład aplikacja do rozwiązywania problemów używa Azure App Service Web Apps, Azure Storage i SQL Database. Poniżej przedstawiono numery umów SLA w dniu, w którym książka elektroniczna jest zapisywana w grudniu, 2013:

![Witryna sieci Web SLA, magazyn SQL Database](design-to-survive-failures/_static/image3.png)

Jaki jest maksymalny czas oczekiwania dla aplikacji na podstawie tej usługi umowy SLA? Może się zdarzyć, że w tym przypadku czas w dół będzie równy wartości procentowej najgorszej umowy SLA lub 99,9%. Może to być prawdziwe, jeśli wszystkie trzy usługi zawsze się nie powiodły, ale nie jest to konieczne w rzeczywistości. Każda usługa może się nie powieść niezależnie w różnym czasie, więc należy obliczyć umowną umowę SLA, mnożąc poszczególne numery umowy SLA.

![Złożona umowa SLA](design-to-survive-failures/_static/image4.png)

W związku z tym Twoja aplikacja może być wyłączona nie tylko przez 43,2 minut w miesiącu, ale 3 razy więcej, 108 minut w miesiącu i nadal mieści się w granicach umowy SLA platformy Azure.

Ten problem nie jest unikatowy dla platformy Azure. Firma Microsoft udostępnia obecnie najlepszą umowy slaą usługę w chmurze, a także podobne problemy, które należy wykonać w przypadku korzystania z usług w chmurze dowolnego dostawcy. To najważniejsze informacje dotyczą sposobu, w jaki można zaprojektować aplikację, aby bezpiecznie rozwiązywać błędy usługi, ponieważ mogą one być często wystarczająco duże, aby mieć wpływ na klientów lub użytkowników.

### <a name="cloud-slas-compared-to-enterprise-down-time-experience"></a>Cloud umowy SLA w porównaniu do korporacyjnego środowiska w czasie

Osoby czasami mówią, "w mojej aplikacji korporacyjnej nigdy nie mam tych problemów". Jeśli podasz informacje o tym, jak dużo czasu w miesiącu, w którym faktycznie się znajdują, zazwyczaj mówią, że jest to również sporadycznie wykonywane. W przypadku poproszenia o to, jak często zauważamy, że "Czasami trzeba utworzyć kopię zapasową lub zainstalować nowy serwer lub aktualizować oprogramowanie". Oczywiście, które liczą się w miarę upływu czasu. Większość aplikacji przedsiębiorstwa, o ile nie są szczególnie krytyczne, są w rzeczywistości wyłączone przez ponad ilość czasu dozwoloną przez naszą usługę umowy SLA. Ale jeśli jest to Twój serwer i Twoja infrastruktura, a ty ponosisz odpowiedzialność za nią i kontrolujesz ją, zamierzasz mniej Angst o czas nieokreślony. W środowisku chmury jest zależne od kogoś innego i nie wiesz, co się dzieje, więc możesz uzyskać więcej martwisz się na jego temat.

Gdy firma osiągnie większy procent czasu, niż wynika z umowy SLA w chmurze, realizuje ją, oferując dużo więcej pieniędzy na sprzęt. Usługa w chmurze mogła to zrobić, ale będzie musiała naliczać wiele innych usług. Zamiast tego można korzystać z ekonomicznej usługi i projektować swoje oprogramowanie, aby nieuniknione błędy powodowały minimalne zakłócenia dla klientów. Twoje zadanie jako projektant aplikacji w chmurze nie jest tak duże, aby uniknąć awarii, ponieważ nie jest to konieczne, aby uniknąć katastrofami, i można to zrobić, koncentrując się na oprogramowaniu, a nie na sprzęcie. Aplikacje dla przedsiębiorstw dążą do zmaksymalizowania średniego czasu między awariami, dzięki czemu aplikacje w chmurze dążą do zminimalizowania średniego czasu na odzyskanie.

### <a name="not-all-cloud-services-have-slas"></a>Nie wszystkie usługi w chmurze mają umowy SLA

Należy pamiętać, że nie każda usługa w chmurze ma nawet umowę SLA. Jeśli aplikacja jest zależna od usługi, która nie ma gwarancji w czasie, może być dłuższa niż największa. Jeśli na przykład zalogujesz się do swojej witryny przy użyciu dostawcy społecznościowego, takiego jak Facebook lub Twitter, skontaktuj się z dostawcą usług, aby dowiedzieć się, czy istnieje umowa SLA i czy nie jest to możliwe. Jeśli jednak usługa uwierzytelniania ulegnie awarii lub nie jest w stanie obsłużyć ilości zgłaszanych żądań, klienci są Zablokowani z Twojej aplikacji. Możesz być wyłączony przez dni lub dłużej. Twórcy jednej nowej aplikacji oczekują setki milionów plików do pobrania i brali zależność od uwierzytelniania w serwisie Facebook, ale nie uczestniczyły w serwisie Facebook przed kontynuowaniem i wykryciem zbyt późno, że nie istniała umowa SLA dla tej usługi.

### <a name="not-all-downtime-counts-toward-slas"></a>Brak wszystkich przestojów w kierunku umowy SLA

Niektóre usługi w chmurze mogą świadomie odmówić usługi, jeśli aplikacja przestanie korzystać z niej. Jest to tzw. *ograniczanie*. Jeśli usługa jest objęta umową SLA, powinna określać warunki, w których można ograniczyć ograniczenia, a projekt aplikacji powinien uniknąć tych warunków i odpowiednio reagować na ograniczanie przepustowości. Na przykład jeśli żądania do usługi kończą się niepowodzeniem, gdy przekroczy określoną liczbę na sekundę, chcesz upewnić się, że automatyczne ponowne próby nie nastąpiły tak szybko, że powodują, że ograniczanie przepływności będzie kontynuowane. Więcej informacji o ograniczaniu przepływności można znaleźć w [rozdziale obsługa błędów przejściowych](transient-fault-handling.md).

## <a name="summary"></a>Podsumowanie

W tym rozdziale podjęto próbę zapoznania się z tym, dlaczego prawdziwa aplikacja w chmurze musi być zaprojektowana w celu bezpiecznego przeżycia błędów. Począwszy od [następnego rozdziału](monitoring-and-telemetry.md), pozostałe wzorce w tej serii zawierają więcej szczegółów na temat niektórych strategii, których można użyć do wykonania tej czynności:

- Mają dobre [monitorowanie i telemetrię](monitoring-and-telemetry.md), dzięki czemu możesz szybko sprawdzić niepowodzenia, które wymagają interwencji, i masz wystarczające informacje, aby je rozwiązać.
- [Obsługa błędów przejściowych](transient-fault-handling.md) przez implementację logiki inteligentnego ponawiania, dzięki czemu aplikacja automatycznie odzyska dostęp do logiki [wyłącznika](transient-fault-handling.md#circuitbreakers) , gdy nie jest to możliwe.
- Użyj [rozproszonego buforowania](distributed-caching.md) , aby zminimalizować problemy dotyczące przepływności, opóźnienia i połączenia z dostępem do bazy danych.
- Zaimplementuj luźny sprzężenie za pośrednictwem [wzorca pracy skoncentrowanego na kolejkach](queue-centric-work-pattern.md), dzięki czemu fronton aplikacji może kontynuować pracę, gdy zaplecze nie działa.

## <a name="resources"></a>Zasoby

Aby uzyskać więcej informacji, zobacz dalsze rozdziały w tej książce elektronicznej oraz następujące zasoby.

Łączoną

- [Failsafe: wskazówki dotyczące odpornych architektur chmurowych](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx). Oficjalny dokument według wytłoczyn Mercuri, Ulrich Homann i Andrew TOWNHILL. Wersja strony sieci Web serii wideo FailSafe.
- [Najlepsze rozwiązania dotyczące projektowania usług na dużą skalę w usłudze Azure Cloud Services](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). Oficjalny dokument ze znakami SIMM i Michael Thomassy.
- [Wskazówki techniczne dotyczące ciągłości działania platformy Azure](https://msdn.microsoft.com/library/windowsazure/hh873027.aspx). Oficjalny dokument o Wickline i Jason Roth.
- [Odzyskiwanie po awarii i wysoka dostępność dla aplikacji platformy Azure](https://msdn.microsoft.com/library/windowsazure/dn251004.aspx). Oficjalny dokument przez Michael McKeown, hanu Kommalapati i Jason Roth.
- [Wzorce i praktyki firmy Microsoft — wskazówki dotyczące platformy Azure](https://msdn.microsoft.com/library/dn568099.aspx). Zobacz wskazówki dotyczące wdrażania w ramach programu wiele centrów danych, wzorzec wyłącznika.
- [Pomoc techniczna platformy Azure — umowy dotyczące poziomu usług](https://azure.microsoft.com/support/legal/sla/).
- [Ciągłość działania w Azure SQL Database](https://msdn.microsoft.com/library/windowsazure/hh852669.aspx). Dokumentacja dotycząca SQL Database funkcji wysokiej dostępności i odzyskiwania po awarii.
- [Wysoka dostępność i odzyskiwanie po awarii dla SQL Server na platformie Azure Virtual Machines](https://msdn.microsoft.com/library/windowsazure/jj870962.aspx).

Filmy wideo:

- [Failsafe: kompilowanie skalowalnych, Odpornych Cloud Services](https://channel9.msdn.com/Series/FailSafe). Seria dziewięciu części przez Ulrich Homann, Marc Mercuri i marking SIMM. Prezentuje koncepcje wysokiego poziomu i zasady architektury w bardzo dostępnym i interesującym scenariuszu, w tym scenariusze opracowane przez firmę Microsoft Customer Advisory Team (CAT) z rzeczywistymi klientami. Odcinki 1 i 8 umożliwiają projektowanie aplikacji w chmurze w celu przechodzenia do niepowodzeń. Zobacz również dyskusję na temat ograniczania przepustowości w epizod 2, zaczynając od 49:57, Omówienie punktów awarii i trybów niepowodzeń w epizod 2, rozpoczynając od 56:05, i Omówienie wyłączników w epizod 3, zaczynając od 40:55.
- [Tworzenie dużych: lekcje uzyskane od klientów platformy Azure — część II](https://channel9.msdn.com/Events/Build/2012/3-030). Oznacz moduły SIMM rozmowy dotyczące projektowania pod kątem awarii i Instrumentacji wszystkich elementów. Podobnie jak w przypadku serii failsafe, ale więcej szczegółów.

> [!div class="step-by-step"]
> [Poprzednie](unstructured-blob-storage.md)
> [dalej](monitoring-and-telemetry.md)

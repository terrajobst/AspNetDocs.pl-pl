---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/asp-net-hosting-options-cs
title: Opcje hostingu ASP.NET (C#) | Microsoft Docs
author: rick-anderson
description: Aplikacje sieci Web ASP.NET są zwykle zaprojektowane, tworzone i testowane w lokalnym środowisku programistycznym i muszą zostać wdrożone w środowisku produkcyjnym o...
ms.author: riande
ms.date: 04/01/2009
ms.assetid: 89a1d2bc-fdfd-4c5c-a3b0-49a08baaf63a
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/asp-net-hosting-options-cs
msc.type: authoredcontent
ms.openlocfilehash: 2eafa750167d89fa996a442633e79dce3d5b85bd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78637173"
---
# <a name="aspnet-hosting-options-c"></a>Opcje hostingu platformy ASP.NET (C#)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz plik PDF](https://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial01_Basics_cs.pdf)

> Aplikacje sieci Web ASP.NET są zwykle zaprojektowane, tworzone i testowane w lokalnym środowisku programistycznym i muszą zostać wdrożone w środowisku produkcyjnym, gdy będzie gotowe do wydania. Ten samouczek zawiera ogólne omówienie procesu wdrażania i służy jako wprowadzenie do tej serii samouczków.

## <a name="introduction"></a>Wprowadzenie

Aplikacje sieci Web są zwykle zaprojektowane, tworzone i testowane w środowisku programistycznym, które jest dostępne tylko dla programistów pracujących w tej witrynie. Gdy aplikacja będzie gotowa do zwolnienia, zostanie przeniesiona do środowiska produkcyjnego, w którym można uzyskać dostęp do witryny przez dowolnego użytkownika w Internecie. W tym procesie wdrażania wprowadzono kilka wyzwań:

- Środowisko produkcyjne musi istnieć i być prawidłowo skonfigurowane, aby można było wdrożyć aplikację ASP.NET; Ponadto środowisko produkcyjne musi być aktualizowane przy użyciu najnowszych poprawek zabezpieczeń.
- Prawidłowy zestaw plików znaczników, plików kodu i plików obsługi należy skopiować ze środowiska programistycznego do środowiska produkcyjnego. W przypadku aplikacji opartych na danych może to wymagać również skopiowania schematu bazy danych i/lub danych.
- Mogą istnieć różnice między tymi dwoma środowiskami. Parametry połączenia z bazą danych lub serwer poczty e-mail używane w środowisku programistycznym będą prawdopodobnie inne niż środowisko produkcyjne. Co więcej, zachowanie aplikacji może zależeć od środowiska. Na przykład w przypadku wystąpienia błędu podczas programowania szczegóły błędu mogą być wyświetlane na ekranie, ale w przypadku wystąpienia błędu w środowisku produkcyjnym powinna zostać wyświetlona strona błędu przyjazna dla użytkownika, a szczegóły błędu będą wysyłane do deweloperów.

Aby uniknąć pierwszego wyzwania — skonfigurowanie i utrzymywanie środowiska produkcyjnego — wiele osób i firm udostępnia swoje środowiska produkcyjne *dostawcom hostingu w sieci Web*. Dostawca hostingu w sieci Web to firma, która zarządza środowiskiem produkcyjnym w Twoim imieniu. Istnieją dostawcy hosta sieci Web niezliczone, każdy z różnymi cenami i poziomami usług. Zapoznaj się z sekcją "Znajdowanie dostawcy hosta sieci Web", aby uzyskać porady dotyczące lokalizowania tego dostawcy usług.

Jest to pierwsza z wielu samouczków, które zapoznają się z krokami związanymi z wdrażaniem aplikacji sieci Web ASP.NET w środowisku produkcyjnym zarządzanym przez dostawcę hosta sieci Web. W ramach tego samouczka będziemy analizować:

- Jakie pliki należy wdrożyć dla dostawcy hosta sieci Web.
- Narzędzia do usprawniania procesu wdrażania.
- Jak wdrożyć bazę danych.
- Wskazówki dotyczące wdrażania bazy danych korzystającej [z dostawcy członkostwa i ról opartego na języku SQL](../../older-versions-security/membership/creating-the-membership-schema-in-sql-server-cs.md), a także sposoby naśladowania narzędzia administracyjnego witryny sieci Web w środowisku produkcyjnym.
- Strategie umożliwiające bezproblemowe aktualizowanie bazy danych w środowisku produkcyjnym przy użyciu zmian wprowadzonych podczas opracowywania.
- Techniki rejestrowania błędów występujących w środowisku produkcyjnym oraz sposoby powiadamiania deweloperów o wystąpieniu błędu.

Te samouczki są zwięzłe i zawierają instrukcje krok po kroku dotyczące dużej ilości zrzutów ekranu, aby przeprowadzić Cię przez proces wizualnie. Ten samouczek Inaugural zawiera omówienie procesu wdrażania ASP.NET oraz porady dotyczące znajdowania dostawcy hostingu w sieci Web. Zacznijmy!

## <a name="an-overview-of-the-aspnet-deployment-process"></a>Przegląd procesu wdrażania ASP.NET

W Nutshell wdrażanie aplikacji ASP.NET obejmuje następujące trzy kroki:

1. Skonfiguruj aplikację sieci Web, serwer sieci Web i bazę danych w środowisku produkcyjnym.
2. Synchronizowanie stron ASP.NET, plików kodu, zestawów w folderze `Bin` i plików obsługi związanych z HTML, takich jak pliki CSS i JavaScript.
3. Zsynchronizuj schemat bazy danych i/lub dane.

Informacje o konfiguracji aplikacji sieci Web znajdują się zwykle w pliku `Web.config` i zawierają parametry połączenia bazy danych, kryteria obsługi błędów, reguły ponownego zapisywania adresów URL i informacje o serwerze poczty e-mail. Często te informacje różnią się w przypadku aplikacji w programowaniu i tej samej aplikacji w środowisku produkcyjnym. Na przykład podczas tworzenia aplikacji najlepiej używać programistycznej bazy danych, aby nie przeprowadzać testów względem produkcyjnej bazy danych. W związku z tym parametry połączenia bazy danych są zwykle różne dla aplikacji deweloperskich i produkcyjnych. Ze względu na te różnice część wdrożenia obejmuje wprowadzanie zmian w informacjach konfiguracyjnych aplikacji sieci Web.

Oprócz zmian konfiguracji aplikacji sieci Web, krok 1 może również pociągnąć za sobą konfigurację serwera sieci Web i bazy danych. Na przykład jeśli strona ASP.NET tworzy lub usuwa pliki z katalogu na serwerze sieci Web, należy skonfigurować serwer sieci Web tak, aby zezwalał na modyfikacje systemu plików. Podobnie mogą istnieć uprawnienia lub ustawienia uwierzytelniania, które należy wykonać w bazie danych.

Krok 2 obejmuje synchronizowanie zestawu podstawowych stron ASP.NET i obsługi plików między środowiskami deweloperskimi i produkcyjnymi. Określony zestaw plików powiązanych z ASP.NET, które muszą być synchronizowane między dwoma środowiskami, zależy od typu projektu utworzonego w programie Visual Studio, a także jest to dyskusja w następnym samouczku, [*określająca, które pliki muszą zostać wdrożone*](determining-what-files-need-to-be-deployed-cs.md). Trzeci i czwarty samouczki — [*wdrażanie witryny przy użyciu protokołu FTP*](deploying-your-site-using-an-ftp-client-cs.md) i [*wdrażanie witryny przy użyciu programu Visual Studio*](deploying-your-site-using-visual-studio-cs.md) — Sprawdź różne narzędzia i techniki do synchronizowania tych plików.

Podczas tworzenia aplikacji opartych na danych są zazwyczaj używane dwie bazy danych: jeden do programowania i jeden w środowisku produkcyjnym. Podczas opracowywania schemat bazy danych programistycznych może być modyfikowany w taki sposób, aby zawierał nowe tabele, kolumny, procedury składowane i wyzwalacze, lub być modyfikowane w celu usunięcia lub zmiany nazwy istniejących obiektów bazy danych. Między czasem wprowadzenia tych zmian i czasem wdrożenia aplikacji w środowisku produkcyjnym bazy danych programistycznych i produkcyjnych nie są zsynchronizowane. Ten asynchroniczności należy naprawić w procesie wdrażania. Te wyzwania będą badane w przyszłości samouczków.

## <a name="finding-a-web-host-provider"></a>Znajdowanie dostawcy hosta sieci Web

Aplikacje ASP.NET można wdrożyć na dowolnym serwerze sieci Web, na którym zainstalowano .NET Framework i Internet Information Services (IIS). Lokację można hostować z komputera osobistego, przy założeniu, że masz połączenie szerokopasmowe z Internetem i jak skonfigurować router tak, aby zezwalał na przychodzące żądania sieci Web. Lokację można również hostować z komputera w intranecie, tak jak wiele firm. Te samouczki udostępniają jednak witrynę internetową z dostawcą hosta sieci Web.

> [!NOTE]
> [Usługi IIS](https://www.iis.net/) to serwer sieci Web klasy korporacyjnej firmy Microsoft. Jest on dostarczany z wersjami systemu Windows, które nie są w wersji Home, takich jak Windows Server 2008 i niektóre wersje systemu Windows Vista. Nie trzeba instalować usług IIS, aby obsługiwały aplikacje ASP.NET w środowisku deweloperskim, ponieważ program Visual Studio zawiera serwer ASP.NET Development Web Server. Jednak serwer sieci Web ASP.NET Development akceptuje tylko połączenia lokalne i dlatego nie może być używany w środowisku produkcyjnym.

Aby można było wdrożyć lokację na potrzeby dostawcy hosta sieci Web, należy najpierw wybrać firmę, z którą firma ma wykonać działalność. W portalu Marketplace są niezliczone firmy hostingowe w sieci Web. wyszukiwanie "Firma hostingu internetowego" zwraca więcej niż 5 000 000 wyników. Jak znaleźć tę, która jest odpowiednia dla Ciebie? Ulubiony aparat wyszukiwania jest dobrym miejscem, podobnie jak w przypadku witryn internetowych, takich jak [TopHosts](http://www.tophosts.com/) i [HostCritique](http://www.hostcritique.net/), które porównują i różnią się w różnych usługach hostingowych. Zaleca się także zaproszenie współpracowników i współpracowników o wszelkie zalecenia; Możesz również zadawać zalecenia na [forum hostingu otwartym](https://forums.asp.net/158.aspx) tutaj na [forach ASP.NET](https://forums.asp.net/).

Firmy obsługujące hosting w sieci Web zazwyczaj oferują udostępnione plany hostingu i dedykowane plany hostingu. Dzięki udostępnianiu hostingu pojedynczy serwer sieci Web jest hostem dziesiątek, jeśli nie ma setek różnych witryn sieci Web. Dzięki dedykowanemu hostingowi możesz dzierżawić komputer od firmy, która obsługuje daną lokację i swoją lokację. Współużytkowany plan hostingu może obejmować obsługę stron ASP.NET, możliwość pracy z bazami danych programu Microsoft Access, 5 GB miejsca na dysku i 100 GB miesięcznego ruchu sieciowego dla $9,95 miesięcznie. Inny udostępniony plan hostingu może obejmować obsługę stron ASP.NET, dostęp do serwera bazy danych Microsoft SQL Server 2008, 10 GB miejsca na dysku i 250 GB miesięcznego ruchu sieciowego w przypadku $19,95 miesięcznie. Dedykowane plany hostingu są zwykle znacznie droższe, a koszty obejmują kilka stu dolarów miesięcznie, ale oferują lepszą wydajność i większą kontrolę niż w przypadku udostępnionych opcji hostingu. Wybór wybranego planu zależy od budżetu, ilości ruchu otrzymanego w witrynie sieci Web oraz przewidywanych funkcji.

W przypadku wybrania dostawcy hosta sieci Web należy wziąć pod uwagę dwa ważne zagadnienia. Jeśli masz pytania lub problemy z konfiguracją, jak długo trwa przesyłanie problemu do pomocy technicznej hosta sieci Web do momentu otrzymania odpowiedzi? Jak są niezawodne usługi firmy? Czy często występuje awaria bazy danych? Jak często serwer poczty e-mail przechodzi w tryb offline? Zawsze możesz pozyskać firmę, aby przedstawić szczegółowe informacje o ich okresie działania oraz uzyskać informacje o ich zasadach obsługi klienta, ale bardziej surefireą metodą jest zażądanie opinii o bieżących i wcześniejszych klientach, które można wykonać za pomocą forów online, grup dyskusyjnych i wiadomości e-mail listservs .

> [!NOTE]
> Niektóre firmy obsługujące sieci Web koncentrują się na konkretnym stosie technologii, takim jak .NET lub [Lampa](http://en.wikipedia.org/wiki/LAMP_stack) (**L** Inux **, Pache,** **M** ySQL i **P** HP), dlatego należy się upewnić, że wybrana Firma obsługuje aplikacje ASP.NET. Sprawdź również, czy obsługują one wersję ASP.NET używaną do kompilowania aplikacji. W przypadku tworzenia aplikacji opartej na danych upewnij się, że host sieci Web oferuje ten sam serwer bazy danych i wersję, której używasz.

## <a name="summary"></a>Podsumowanie

Aplikacje sieci Web ASP.NET są zwykle zaprojektowane, utworzone i przetestowane w lokalnym środowisku programistycznym. Gdy wersja jest gotowa do wydania, jest przenoszona do środowiska produkcyjnego. Chociaż istnieje możliwość hostowania witryn sieci Web ASP.NET na komputerze osobistym lub na serwerach w firmie, wiele firm i osób wybierają Źródło hostingu do dostawcy hosta sieci Web.

W tej serii samouczków przedstawiono kroki wdrażania aplikacji ASP.NET dla dostawcy hosta sieci Web, badając typowe wyzwania. Ten samouczek oferuje ogólne omówienie procesu wdrażania ASP.NET oraz porady dotyczące znajdowania odpowiedniego dostawcy hosta sieci Web. W następnym samouczku przedstawiono, jakie pliki związane z ASP.NETmi muszą zostać skopiowane do środowiska produkcyjnego podczas wdrażania witryny sieci Web.

Szczęśliwe programowanie!

### <a name="special-thanks-to"></a>Specjalne podziękowania dla...

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Recenzent potencjalnych klientów dla tego samouczka został Teresa Murphy. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Dalej](determining-what-files-need-to-be-deployed-cs.md)

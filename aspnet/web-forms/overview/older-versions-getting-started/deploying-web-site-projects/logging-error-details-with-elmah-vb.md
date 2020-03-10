---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-elmah-vb
title: Rejestrowanie szczegółów błędów za pomocą programu ELMAH (VB) | Microsoft Docs
author: rick-anderson
description: Rejestrowanie błędów modułów i programów obsługi (ELMAH) oferuje inne podejście do rejestrowania błędów czasu wykonywania w środowisku produkcyjnym. ELMAH to bezpłatny błąd typu open source...
ms.author: riande
ms.date: 06/09/2009
ms.assetid: a5f0439f-18b2-4c89-96ab-75b02c616f46
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-elmah-vb
msc.type: authoredcontent
ms.openlocfilehash: 46b7fc22807c8cb9f47ff035639815d7b6104735
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78525712"
---
# <a name="logging-error-details-with-elmah-vb"></a>Rejestrowanie szczegółów błędów za pomocą biblioteki ELMAH (VB)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz kod](https://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_14_VB.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial14_ELMAH_vb.pdf)

> Rejestrowanie błędów modułów i programów obsługi (ELMAH) oferuje inne podejście do rejestrowania błędów czasu wykonywania w środowisku produkcyjnym. ELMAH to bezpłatna Biblioteka rejestrowania błędów typu open source, która obejmuje funkcje takie jak filtrowanie błędów i możliwość wyświetlania dziennika błędów ze strony sieci Web jako źródła danych RSS lub pobierania ich jako pliku rozdzielanego przecinkami. Ten samouczek przeprowadzi Cię przez proces pobierania i konfigurowania ELMAH.

## <a name="introduction"></a>Wprowadzenie

[Poprzedni samouczek](logging-error-details-with-asp-net-health-monitoring-vb.md) zbadał ASP. System monitorowania kondycji sieci, który oferuje out z biblioteki Box do rejestrowania szerokiej gamy zdarzeń sieci Web. Wielu deweloperów korzysta z monitorowania kondycji, aby rejestrować i wysłać pocztą e-mail szczegóły nieobsłużonych wyjątków. Istnieje jednak kilka problemów w tym systemie. Pierwszy i najbardziej do przodu to brak żadnego rodzaju interfejsu użytkownika do wyświetlania informacji o rejestrowanych zdarzeniach. Jeśli chcesz zobaczyć podsumowanie 10 ostatnich błędów lub wyświetlić szczegóły błędu, który wystąpił w ubiegłym tygodniu, musisz albo poke za pośrednictwem bazy danych, skanować za pośrednictwem skrzynki odbiorczej poczty e-mail, albo utworzyć stronę sieci Web, która wyświetla informacje z tabeli `aspnet_WebEvent_Events`.

Inne bólu wskazują na złożoność monitorowania kondycji. Ze względu na to, że można użyć monitorowania kondycji w celu rejestrowania mnóstwo różnych zdarzeń i ponieważ istnieją różne opcje dotyczące sposobu, w jaki są rejestrowane zdarzenia, prawidłowe skonfigurowanie systemu monitorowania kondycji może być uciążliwym zadaniem. Na koniec występują problemy ze zgodnością. Ponieważ monitorowanie kondycji zostało po raz pierwszy dodane do .NET Framework w wersji 2,0, nie jest dostępne dla starszych aplikacji sieci Web utworzonych przy użyciu ASP.NET w wersji 1. x. Ponadto Klasa `SqlWebEventProvider`, która została użyta w poprzednim samouczku do rejestrowania szczegółów błędów w bazie danych, działa tylko z bazami danych Microsoft SQL Server. Należy utworzyć niestandardową klasę dostawcy dziennika, aby rejestrować błędy w alternatywnym magazynie danych, takim jak plik XML lub baza danych Oracle.

Alternatywą dla systemu monitorowania kondycji są błędy rejestrowania modułów i programów obsługi (ELMAH) — bezpłatny system rejestrowania błędów typu "open source" utworzony przez [Atif Aziz](http://www.raboof.com/). Najbardziej istotną różnicą między tymi dwoma systemami jest ELAMHa możliwość wyświetlania listy błędów i szczegółów konkretnego błędu ze strony sieci Web oraz źródła danych RSS. Program ELMAH jest łatwiejszy do skonfigurowania niż monitorowanie kondycji, ponieważ tylko rejestruje błędy. Ponadto program ELMAH obejmuje obsługę aplikacji ASP.NET 1. x, ASP.NET 2,0 i ASP.NET 3,5 oraz są dostarczane z różnymi dostawcami źródeł dzienników.

Ten samouczek przeprowadzi Cię przez kroki związane z dodawaniem programu ELMAH do aplikacji ASP.NET. Zacznijmy!

> [!NOTE]
> System monitorowania kondycji i program ELMAH mają własne zestawy informatyków i wad. Zachęcam do wypróbowania obu systemów i zdecydowania, co najlepiej odpowiada Twoim potrzebom.

## <a name="adding-elmah-to-an-aspnet-web-application"></a>Dodawanie programu ELMAH do aplikacji sieci Web ASP.NET

Integrowanie programu ELMAH z nową lub istniejącą aplikacją ASP.NET jest łatwym i prostym procesem, który jest wymagany w ciągu pięciu minut. W Nutshell, obejmuje cztery proste kroki:

1. Pobierz program ELMAH i Dodaj zestaw `Elmah.dll` do aplikacji sieci Web.
2. Rejestrowanie modułów i obsługi HTTP programu ELMAH w `Web.config`,
3. Określ opcje konfiguracji programu ELMAH i
4. Utwórz infrastrukturę źródłową dziennika błędów, w razie konieczności.

Przechodźmy po jednym z tych czterech kroków, po jednej naraz.

### <a name="step-1-downloading-the-elmah-project-files-and-addingelmahdllto-your-web-application"></a>Krok 1. Pobieranie plików projektu ELMAH i Dodawanie`Elmah.dll`do aplikacji sieci Web

Program ELMAH 1,0 BETA 3 (kompilacja 10617), Najnowsza wersja w momencie pisania, jest uwzględniony w pobieraniu dostępnym w tym samouczku. Alternatywnie możesz odwiedzić [witrynę internetową programu ELMAH](https://code.google.com/p/elmah/) , aby uzyskać najnowszą wersję lub pobrać kod źródłowy. Wyodrębnij pobieranie pliku ELMAH do folderu na pulpicie i zlokalizuj plik zestawu ELMAH (`Elmah.dll`).

> [!NOTE]
> Plik `Elmah.dll` znajduje się w folderze `Bin` pobierania, który ma podfoldery dla różnych wersji .NET Framework oraz dla kompilacji wydań i debugowania. Użyj kompilacji wydania dla odpowiedniej wersji platformy. Na przykład jeśli tworzysz aplikację sieci Web ASP.NET 3,5, skopiuj plik `Elmah.dll` z folderu `Bin\net-3.5\Release`.

Następnie otwórz program Visual Studio i Dodaj zestaw do projektu, klikając prawym przyciskiem myszy nazwę witryny sieci Web w Eksplorator rozwiązań i wybierając pozycję Dodaj odwołanie z menu kontekstowego. Spowoduje to wyświetlenie okna dialogowego Dodawanie odwołania. Przejdź do karty Przeglądaj i wybierz plik `Elmah.dll`. Ta akcja powoduje dodanie pliku `Elmah.dll` do folderu `Bin` aplikacji sieci Web.

> [!NOTE]
> Typ projektu aplikacji sieci Web (WAP) nie pokazuje folderu `Bin` w Eksplorator rozwiązań. Zamiast tego wyświetla listę tych elementów w folderze References.

Zestaw `Elmah.dll` obejmuje klasy używane przez system ELMAH. Te klasy należą do jednej z trzech kategorii:

- **Moduły HTTP** — moduł http to Klasa, która definiuje programy obsługi zdarzeń dla zdarzeń `HttpApplication`, takich jak zdarzenie `Error`. ELMAH zawiera wiele modułów HTTP, trzy najbardziej niemieckie: 

    - `ErrorLogModule` — rejestruje Nieobsłużone wyjątki w źródle dziennika.
    - `ErrorMailModule` — wysyła szczegóły nieobsłużonego wyjątku w wiadomości e-mail.
    - `ErrorFilterModule` — stosuje filtry określone przez dewelopera, aby określić, które wyjątki są rejestrowane i jakie są ignorowane.
- **Obsługa protokołu HTTP** — obsługa protokołu HTTP jest klasą odpowiedzialną za generowanie znaczników dla określonego typu żądania. ELMAH obejmuje procedury obsługi protokołu HTTP, które renderują szczegóły błędów jako stronę sieci Web, jako źródło danych RSS lub plik rozdzielany przecinkami (CSV).
- **Źródła dzienników błędów** — w polu ELMAH można rejestrować błędy do pamięci, Microsoft SQL Server do bazy danych programu Microsoft Access, do bazy danych w bazie danych firmy Oracle, do pliku XML, do bazy danych oprogramowania SQLite lub bazy danych systemu Vista DB. Podobnie jak w przypadku systemu monitorowania kondycji, architektura programu ELMAH została skompilowana przy użyciu modelu dostawcy, co oznacza, że można utworzyć i bezproblemowo zintegrować własnych niestandardowych dostawców źródeł dzienników, jeśli jest to konieczne.

### <a name="step-2-registering-elmahs-http-module-and-handler"></a>Krok 2. Rejestrowanie modułu i obsługi HTTP programu ELMAH

Plik `Elmah.dll` zawiera moduły HTTP i program obsługi, które są niezbędne do automatycznego rejestrowania nieobsłużonych wyjątków i wyświetlania szczegółów błędów ze strony sieci Web, muszą być jawnie zarejestrowane w konfiguracji aplikacji sieci Web. `ErrorLogModule` moduł HTTP, po zarejestrowaniu, subskrybuje zdarzenie `Error` `HttpApplication`. Każde zdarzenie zostanie zgłoszone `ErrorLogModule` rejestruje szczegóły wyjątku w określonym źródle dziennika. Zobaczymy, jak zdefiniować dostawcę źródła dziennika w następnej sekcji "Konfigurowanie programu ELMAH". Fabryka obsługi HTTP `ErrorLogPageFactory` jest odpowiedzialna za generowanie znaczników podczas wyświetlania dziennika błędów ze strony sieci Web.

Określona Składnia służąca do rejestrowania modułów i obsługi HTTP zależy od serwera sieci Web, na którym jest włączana lokacja. W przypadku serwera ASP.NET Development i usług IIS firmy Microsoft w wersji 6,0 lub starszej moduły HTTP i programy obsługi są zarejestrowane w sekcjach `<httpModules>` i `<httpHandlers>`, które znajdują się w `<system.web>` elemencie. Jeśli używasz usług IIS 7,0, muszą one być zarejestrowane w sekcjach `<modules>` i `<handlers>` elementu `<system.webServer>`. Na szczęście można zdefiniować moduły i programy obsługi HTTP w *obu* miejscach niezależnie od używanego serwera sieci Web. Ta opcja jest najbardziej przenośnym serwerem, ponieważ umożliwia korzystanie z tej samej konfiguracji w środowiskach deweloperskich i produkcyjnych niezależnie od używanego serwera sieci Web.

Zacznij od zarejestrowania modułu `ErrorLogModule` HTTP i procedury obsługi protokołu HTTP `ErrorLogPageFactory` w sekcji `<httpModules>` i `<httpHandlers>` w `<system.web>`. Jeśli konfiguracja już definiuje te dwa elementy, wystarczy po prostu dodać element `<add>` dla modułu HTTP i procedury obsługi.

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample1.xml)]

Następnie zarejestruj moduł i program obsługi protokołu HTTP programu ELMAH w elemencie `<system.webServer>`. Tak jak wcześniej, jeśli ten element nie jest jeszcze obecny w konfiguracji, Dodaj go.

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample2.xml)]

Domyślnie usługi IIS 7 zgłaszają skargę, jeśli moduły i programy obsługi HTTP są zarejestrowane w sekcji `<system.web>`. Atrybut `validateIntegratedModeConfiguration` w elemencie `<validation>` instruuje usługi IIS 7, aby pominąć takie komunikaty o błędach.

Należy zauważyć, że Składnia służąca do rejestrowania programu obsługi protokołu HTTP `ErrorLogPageFactory` zawiera atrybut `path`, który jest ustawiony na `elmah.axd`. Ten atrybut informuje aplikację sieci Web, że jeśli żądanie dotarło do strony o nazwie `elmah.axd`, żądanie powinno być przetwarzane przez `ErrorLogPageFactory` procedurę obsługi protokołu HTTP. W dalszej części tego samouczka zobaczysz procedurę obsługi protokołu HTTP `ErrorLogPageFactory` w działaniu.

### <a name="step-3-configuring-elmah"></a>Krok 3. Konfigurowanie ELMAH

Program ELMAH szuka opcji konfiguracji w pliku `Web.config` witryny sieci Web w sekcji konfiguracji niestandardowej o nazwie `<elmah>`. Aby można było użyć sekcji niestandardowej w `Web.config`, musi ona zostać najpierw zdefiniowana w elemencie `<configSections>`. Otwórz plik `Web.config` i Dodaj następujący znacznik do `<configSections>`:

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample3.xml)]

> [!NOTE]
> Jeśli konfigurujesz program ELMAH dla aplikacji ASP.NET 1. x, Usuń atrybut `requirePermission="false"` z powyższych elementów `<section>`.

Powyższa składnia rejestruje sekcję `<elmah>` niestandardowego i jej podsekcje: `<security>`, `<errorLog>`, `<errorMail>`i `<errorFilter>`.

Następnie Dodaj sekcję `<elmah>`, aby `Web.config`. Ta sekcja powinna pojawić się na tym samym poziomie co element `<system.web>`. W sekcji `<elmah>` Dodaj `<security>` i `<errorLog>` w taki sposób:

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample4.xml)]

Atrybut `allowRemoteAccess` `<security>` sekcji wskazuje, czy dostęp zdalny jest dozwolony. Jeśli ta wartość jest równa 0, Strona sieci Web dziennika błędów może być wyświetlana tylko lokalnie. Jeśli ten atrybut jest ustawiony na 1, Strona sieci Web dziennika błędów jest włączona zarówno dla odwiedzających zdalnego, jak i lokalnych. Na razie wyłączmy stronę sieci Web dziennika błędów dla zdalnych osób odwiedzających. Zezwolimy na dostęp zdalny później, gdy będziemy mogli omówić problemy związane z bezpieczeństwem.

Sekcja `<errorLog>` definiuje Źródło dziennika błędów, które określa, gdzie są rejestrowane szczegóły błędu; jest podobna do sekcji `<providers>` w systemie monitorowania kondycji. Powyższa składnia określa klasę `SqlErrorLog` jako źródło dziennika błędów, która rejestruje błędy w bazie danych Microsoft SQL Server określonej przez `connectionStringName` wartość atrybutu.

> [!NOTE]
> Program ELMAH jest dostarczany z dodatkowymi dostawcami dzienników błędów, których można użyć do rejestrowania błędów w pliku XML, bazy danych programu Microsoft Access, bazy danych Oracle i innych magazynach danych. Zapoznaj się z przykładowym plikiem `Web.config`, który jest dołączony do pobierania programu ELMAH, aby uzyskać informacje na temat korzystania z tych alternatywnych dostawców dzienników błędów.

### <a name="step-4-creating-the-error-log-source-infrastructure"></a>Krok 4. Tworzenie infrastruktury źródłowej dziennika błędów

Dostawca `SqlErrorLog` ELMAH rejestruje szczegóły błędu w określonej Microsoft SQL Server bazie danych. Dostawca `SqlErrorLog` oczekuje, że ta baza danych ma tabelę o nazwie `ELMAH_Error` i trzy procedury składowane: `ELMAH_GetErrorsXml`, `ELMAH_GetErrorXml`i `ELMAH_LogError`. Pobieranie pliku ELMAH obejmuje plik o nazwie `SQLServer.sql` w folderze `db` zawierającym język T-SQL do tworzenia tej tabeli i tych procedur składowanych. Musisz uruchomić te instrukcje w bazie danych, aby użyć dostawcy `SqlErrorLog`.

**Rysunki 1** i **2** pokazują Eksplorator bazy danych w programie Visual Studio po dodaniu obiektów bazy danych wymaganych przez dostawcę `SqlErrorLog`.

[![](logging-error-details-with-elmah-vb/_static/image2.png)](logging-error-details-with-elmah-vb/_static/image1.png)

**Rysunek 1**: dostawca `SqlErrorLog` rejestruje błędy w tabeli `ELMAH_Error`

[![](logging-error-details-with-elmah-vb/_static/image4.png)](logging-error-details-with-elmah-vb/_static/image3.png)

**Rysunek 2**: dostawca `SqlErrorLog` używa trzech procedur składowanych

## <a name="elmah-in-action"></a>ELMAH w akcji

W tym momencie dodaliśmy ELMAH do aplikacji sieci Web, zarejestrowano moduł `ErrorLogModule` HTTP i `ErrorLogPageFactory` obsługi protokołu HTTP, określono opcje konfiguracji programu ELMAH w `Web.config`i dodaliśmy odpowiednie obiekty bazy danych dla dostawcy dziennika błędów `SqlErrorLog`. Teraz można przystąpić do wyświetlania obiektu ELMAH w akcji. Odwiedź witrynę sieci Web przeglądający książki i odwiedź stronę, która generuje błąd czasu wykonywania, taką jak `Genre.aspx?ID=foo`lub nieistniejąca Strona, taka jak `NoSuchPage.aspx`. Elementy wyświetlane podczas odwiedzania tych stron są zależne od konfiguracji `<customErrors>` i tego, czy odwiedzasz lokalnie czy zdalnie. (Zapoznaj się z powrotem z samouczkiem dotyczącym [ *wyświetlania strony błędu niestandardowego* ](displaying-a-custom-error-page-vb.md) dla odświeżacza w tym temacie).

Obiekt ELMAH nie ma wpływu na zawartość pokazywaną użytkownikowi, gdy wystąpi nieobsługiwany wyjątek; po prostu rejestruje szczegółowe informacje. Ten dziennik błędów jest dostępny ze strony sieci Web `elmah.axd` z katalogu głównego witryny sieci Web, np. `http://localhost/BookReviews/elmah.axd`. (Ten plik nie istnieje fizycznie w projekcie, ale gdy żądanie jest wysyłane do `elmah.axd` środowisko uruchomieniowe wysyła je do `ErrorLogPageFactory` obsługi protokołu HTTP, która generuje znaczniki wysłane z powrotem do przeglądarki).

> [!NOTE]
> Możesz również użyć strony `elmah.axd`, aby nakazać programowi ELMAH wygenerowanie błędu testu. Odwiedzenie `elmah.axd/test` (jak w `http://localhost/BookReviews/elmah.axd/test`) powoduje zgłoszenie wyjątku typu `Elmah.TestException`, który zawiera komunikat o błędzie: "to jest wyjątek testu, który można bezpiecznie zignorować".

**Rysunek 3** przedstawia dziennik błędów podczas odwiedzania `elmah.axd` ze środowiska deweloperskiego.

[![](logging-error-details-with-elmah-vb/_static/image6.png)](logging-error-details-with-elmah-vb/_static/image5.png)

**Rysunek 3**. `Elmah.axd` wyświetla dziennik błędów ze strony sieci Web  
([Kliknij, aby wyświetlić obraz o pełnym rozmiarze](logging-error-details-with-elmah-vb/_static/image7.png))

Dziennik błędów na **rysunku 3** zawiera sześć wpisów błędów. Każdy wpis zawiera kod stanu HTTP (404 lub 500, dla tych błędów), typ, opis, nazwę zalogowanego użytkownika, gdy wystąpił błąd, oraz datę i godzinę. Kliknięcie linku szczegóły powoduje wyświetlenie strony zawierającej ten sam komunikat o błędzie wyświetlany w oknie Szczegóły błędu żółtego ekranu o śmierci (zobacz **rysunek 4**) wraz z wartościami zmiennych serwera w czasie błędu (patrz **rysunek 5**). Możesz również wyświetlić nieprzetworzony kod XML, w którym zapisywane są szczegóły błędu, w tym dodatkowe informacje, takie jak wartości w nagłówku POST protokołu HTTP.

[![](logging-error-details-with-elmah-vb/_static/image9.png)](logging-error-details-with-elmah-vb/_static/image8.png)

**Rysunek 4**. Wyświetl szczegóły błędu YSOD  
([Kliknij, aby wyświetlić obraz o pełnym rozmiarze](logging-error-details-with-elmah-vb/_static/image10.png))

[![](logging-error-details-with-elmah-vb/_static/image12.png)](logging-error-details-with-elmah-vb/_static/image11.png)

**Rysunek 5**. Eksplorowanie wartości kolekcji zmiennych serwera w czasie błędu  
([Kliknij, aby wyświetlić obraz o pełnym rozmiarze](logging-error-details-with-elmah-vb/_static/image13.png))

Wdrożenie programu ELMAH do produkcyjnej witryny sieci Web wiąże się z:

- Kopiowanie pliku `Elmah.dll` do folderu `Bin` w środowisku produkcyjnym,
- Kopiowanie ustawień konfiguracji specyficznych dla programu ELMAH do pliku `Web.config` używanego w środowisku produkcyjnym i
- Dodanie infrastruktury źródłowej dziennika błędów do produkcyjnej bazy danych.

W poprzednich samouczkach zostały omówione techniki kopiowania plików z programowania do produkcji. Prawdopodobnie najprostszym sposobem pobrania infrastruktury źródłowej dziennika błędów w produkcyjnej bazie danych jest użycie SQL Server Management Studio do nawiązania połączenia z produkcyjną bazą danych, a następnie wykonanie pliku skryptu `SqlServer.sql`, co spowoduje utworzenie wymaganej tabeli i procedur składowanych.

### <a name="viewing-the-error-details-page-on-production"></a>Wyświetlanie strony Szczegóły błędu w środowisku produkcyjnym

Po wdrożeniu lokacji w środowisku produkcyjnym odwiedź witrynę produkcyjną i Wygeneruj nieobsługiwany wyjątek. Podobnie jak w środowisku programistycznym, ELMAH nie ma wpływu na stronę błędu wyświetlaną w przypadku wystąpienia nieobsługiwanego wyjątku; Zamiast tego tylko rejestruje błąd. Jeśli spróbujesz odwiedzić stronę dziennika błędów (`elmah.axd`) ze środowiska produkcyjnego, pozostanie powitanie za pomocą strony zabronionej pokazanej na **rysunku 6**.

[![](logging-error-details-with-elmah-vb/_static/image15.png)](logging-error-details-with-elmah-vb/_static/image14.png)

**Ilustracja 6**. Domyślnie zdalne osoby odwiedzające nie mogą wyświetlić strony sieci Web dziennika błędów  
([Kliknij, aby wyświetlić obraz o pełnym rozmiarze](logging-error-details-with-elmah-vb/_static/image16.png))

Odwołaj się do sekcji `<security>` konfiguracji programu ELMAH, ustawiamy atrybut `allowRemoteAccess` na 0, co uniemożliwia użytkownikom zdalnym Wyświetlanie dziennika błędów. Ważne jest, aby uniemożliwić anonimowym osobom odwiedzającym wyświetlanie dziennika błędów, ponieważ szczegóły błędu mogą ujawnić luki w zabezpieczeniach lub inne poufne informacje. Jeśli zdecydujesz się ustawić ten atrybut na 1 i włączyć dostęp zdalny do dziennika błędów, należy zablokować ścieżkę `elmah.axd` tak, aby tylko autoryzowani osoby odwiedzające mogli uzyskać do niej dostęp. Można to osiągnąć przez dodanie elementu `<location>` do pliku `Web.config`.

Następująca konfiguracja zezwala na dostęp do strony sieci Web dziennika błędów tylko użytkownikom z roli administratora:

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample5.xml)]

> [!NOTE]
> Rola administratora i trzech użytkowników w systemie — Scott, Jisun i Alicja — zostały dodane do [ *konfigurowania witryny sieci Web korzystającej z usługi aplikacji* samouczka](configuring-a-website-that-uses-application-services-vb.md). Użytkownicy Scott i Jisun są członkami roli administratora. Aby uzyskać więcej informacji na temat uwierzytelniania i autoryzacji, zapoznaj się z [samouczkami dotyczącymi zabezpieczeń w witrynie sieci Web](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).

Dziennik błędów w środowisku produkcyjnym może teraz być wyświetlany przez użytkowników zdalnych; Zapoznaj się z powrotem z **ilustracjami 3**, **4**i **5** na potrzeby zrzutów ekranu na stronie sieci Web dziennik błędów. Jeśli jednak użytkownik anonimowy lub niebędący administratorem próbuje wyświetlić stronę dziennika błędów, automatycznie przekierowywać do strony logowania (`Login.aspx`), jak pokazano na **rysunku 7** .

[![](logging-error-details-with-elmah-vb/_static/image18.png)](logging-error-details-with-elmah-vb/_static/image17.png)

**Rysunek 7**: nieautoryzowani użytkownicy są automatycznie przekierowywani do strony logowania  
([Kliknij, aby wyświetlić obraz o pełnym rozmiarze](logging-error-details-with-elmah-vb/_static/image19.png))

### <a name="programmatically-logging-errors"></a>Programowe rejestrowanie błędów

Moduł protokołu HTTP `ErrorLogModule` ELMAH automatycznie rejestruje Nieobsłużone wyjątki do określonego źródła dziennika. Alternatywnie można rejestrować błąd bez konieczności zgłaszania nieobsłużonego wyjątku przy użyciu klasy `ErrorSignal` i jej metody `Raise`. Metoda `Raise` jest przenoszona do obiektu `Exception` i rejestruje je tak, jakby ten wyjątek został zgłoszony i osiągnął środowisko uruchomieniowe ASP.NET bez obsługi. Jednak różnica polega na tym, że żądanie nadal wykonuje się normalnie po wywołaniu metody `Raise`, podczas gdy zgłoszono, nieobsłużony wyjątek przerywa normalne wykonywanie żądania i powoduje, że środowisko uruchomieniowe ASP.NET wyświetla skonfigurowaną stronę błędu.

Klasa `ErrorSignal` jest przydatna w sytuacjach, gdy istnieje pewna akcja, która może zakończyć się niepowodzeniem, ale jej awaria nie jest krytyczna dla wykonywanej operacji. Na przykład witryna sieci Web może zawierać formularz, który pobiera dane wejściowe użytkownika, zapisuje je w bazie danych, a następnie wysyła użytkownikowi wiadomość e-mail informującą o tym, że informacje zostały przetworzone. Co powinno się zdarzyć, jeśli informacje zostały pomyślnie zapisane w bazie danych, ale podczas wysyłania wiadomości e-mail Wystąpił błąd? Jedną z opcji jest zgłoszenie wyjątku i wysłanie go do strony błędu. Jednak może to spowodować, że użytkownik zauważa, że wprowadzone informacje nie zostały zapisane. Innym podejściem jest zarejestrowanie błędu związanego z wiadomościami e-mail, ale nie zmiana środowiska użytkownika w jakikolwiek sposób. Jest to miejsce, w którym Klasa `ErrorSignal` jest przydatna.

[!code-vb[Main](logging-error-details-with-elmah-vb/samples/sample6.vb)]

## <a name="error-notification-via-email"></a>Powiadomienie o błędzie za pośrednictwem poczty E-mail

Wraz z błędami rejestrowania w bazie danych, można również skonfigurować w taki sposób, aby szczegółowe informacje o błędach były wysyłane do określonego adresata. Ta funkcja jest udostępniana przez moduł `ErrorMailModule` HTTP; w związku z tym należy zarejestrować ten moduł HTTP w `Web.config`, aby można było wysłać szczegóły błędu za pośrednictwem poczty e-mail.

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample7.xml)]

Następnie podaj informacje o błędzie wiadomości e-mail w sekcji `<errorMail>` elementu `<elmah>`, wskazujące nadawcę i odbiorcę wiadomości e-mail, podmiot oraz informację o tym, czy wiadomość e-mail jest wysyłana asynchronicznie.

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample8.xml)]

Za każdym razem, gdy wystąpi błąd w czasie wykonywania, w przypadku wystąpienia błędu programu ELMAH wysyła wiadomość e-mail w celu support@example.com ze szczegółowymi informacjami o błędzie. Wiadomość e-mail dotycząca błędu programu ELMAH zawiera te same informacje, które są wyświetlane na stronie sieci Web szczegóły błędu, mianowicie komunikat o błędzie, ślad stosu i zmienne serwerowe (patrz z powrotem do **ilustracji 4** i **5**). Wiadomość e-mail z błędami zawiera również żółty ekran z informacjami o śmierci w postaci załącznika (`YSOD.html`).

Na **rysunku nr 8** przedstawiono wiadomość e-mail z BŁĘDem ELMAH wygenerowanego przez odwiedzenie `Genre.aspx?ID=foo`. Na **rysunku 8** przedstawiono tylko komunikat o błędzie i ślad stosu, natomiast Zmienne serwera są uwzględniane w treści wiadomości e-mail.

[![](logging-error-details-with-elmah-vb/_static/image21.png)](logging-error-details-with-elmah-vb/_static/image20.png)

**Ilustracja 8**. możesz skonfigurować ELMAH, aby wysyłał szczegóły błędu pocztą e-mail  
([Kliknij, aby wyświetlić obraz o pełnym rozmiarze](logging-error-details-with-elmah-vb/_static/image22.png))

## <a name="only-logging-errors-of-interest"></a>Rejestrowane są tylko błędy związane z zainteresowaniami

Domyślnie program ELMAH rejestruje szczegóły każdego nieobsługiwanego wyjątku, w tym 404 i inne błędy HTTP. Można wydać polecenie ELMAH, aby zignorować te lub inne typy błędów przy użyciu filtrowania błędów. Logika filtrowania jest wykonywana przez moduł HTTP `ErrorFilterModule`, który należy zarejestrować w `Web.config`, aby można było użyć logiki filtrowania. Reguły filtrowania są określone w sekcji `<errorFilter>`.

Poniższe znaczniki instruują ELMAH, aby nie rejestrował błędów 404.

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample9.xml)]

> [!NOTE]
> Nie zapomnij, aby użyć filtrowania błędów, należy zarejestrować moduł `ErrorFilterModule` HTTP.

Element `<equal>` wewnątrz sekcji `<test>` jest określany jako potwierdzenie. Jeśli potwierdzenie zwróci wartość true, błąd jest filtrowany z dziennika programu ELMAH. Dostępne są inne potwierdzenia, w tym: `<greater>`, `<greater-or-equal>`, `<not-equal>`, `<lesser>`, `<lesser-or-equal>`itd. Można również łączyć potwierdzenia przy użyciu `<and>` i `<or>` operatory logiczne. Co więcej, możesz nawet uwzględnić proste wyrażenie JavaScript jako potwierdzenie lub napisać własne potwierdzenia w C# lub Visual Basic.

Aby uzyskać więcej informacji na temat możliwości filtrowania błędów w programie ELMAH, zapoznaj się z [sekcją filtrowanie błędów](https://code.google.com/p/elmah/wiki/ErrorFiltering) w [witrynie typu wiki programu ELMAH](https://code.google.com/p/elmah/w/list).

## <a name="summary"></a>Podsumowanie

Program ELMAH oferuje prosty, ale wydajny mechanizm rejestrowania błędów w aplikacji sieci Web ASP.NET. Podobnie jak w przypadku systemu monitorowania kondycji firmy Microsoft, ELMAH może rejestrować błędy w bazie danych i wysyłać szczegóły błędu do dewelopera za pośrednictwem poczty e-mail. W przeciwieństwie do systemu monitorowania kondycji, program ELMAH obejmuje obsługę usługi Box dla szerszego zakresu magazynów danych dzienników błędów, w tym: Microsoft SQL Server, Microsoft Access, Oracle, plików XML i kilku innych. Ponadto program ELMAH oferuje wbudowany mechanizm wyświetlania dziennika błędów i szczegółowych informacji o konkretnym błędzie ze strony sieci Web, `elmah.axd`. Na stronie `elmah.axd` można także renderować informacje o błędach jako źródło danych RSS lub plik z wartościami rozdzielanymi przecinkami (CSV), który można odczytać przy użyciu programu Microsoft Excel. Można również nakazać programowi ELMAH filtrowanie błędów z dziennika przy użyciu deklaratywnych lub programistycznych zatwierdzeń. I programu ELMAH można używać z aplikacjami ASP.NET w wersji 1. x.

Każda wdrożona aplikacja powinna mieć jakiś mechanizm automatycznego rejestrowania nieobsłużonych wyjątków i wysyłania powiadomień do zespołu programistycznego. Niezależnie od tego, czy jest to realizowane przy użyciu monitorowania kondycji, czy ELMAH jest pomocniczy. Innymi słowy, nie ma znaczenia, czy używasz monitorowania kondycji, czy ELMAH; Oceń oba systemy, a następnie wybierz te, które najlepiej pasują do Twoich potrzeb. Podstawowe znaczenie polega na tym, że w środowisku produkcyjnym można rejestrować Nieobsłużone wyjątki.

Szczęśliwe programowanie!

### <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat tematów omówionych w tym samouczku, zapoznaj się z następującymi zasobami:

- [ELMAH — rejestrowanie błędów modułów i programów obsługi](http://dotnetslackers.com/articles/aspnet/ErrorLoggingModulesAndHandlers.aspx)
- [Strona projektu ELMAH](https://code.google.com/p/elmah/) (kod źródłowy, przykłady, wiki)
- [Podłączanie programu ELMAH do aplikacji sieci Web w celu przechwycenia nieobsłużonych wyjątków](http://screencastaday.com/ScreenCasts/43_Plugging_Elmah_into_Web_Application_to_Catch_Unhandled_Exceptions.aspx) (wideo)
- [Strony dziennika błędów zabezpieczeń](https://code.google.com/p/elmah/wiki/SecuringErrorLogPages)
- [Tworzenie podłączanych składników ASP.NET za pomocą modułów i programów HTTP](https://msdn.microsoft.com/library/aa479332.aspx)
- [Samouczki dotyczące zabezpieczeń witryny sieci Web](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)

> [!div class="step-by-step"]
> [Poprzednie](logging-error-details-with-asp-net-health-monitoring-vb.md)
> [dalej](precompiling-your-website-vb.md)

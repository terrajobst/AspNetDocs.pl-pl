---
uid: visual-studio/overview/2013/aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes
title: ASP.NET and Web Tools Tools 2013.2 dla programu Visual Studio 2013 Release Notes | Dokumentacja firmy Microsoft
author: microsoft
description: ''
ms.author: riande
ms.date: 03/06/2014
ms.assetid: 7ef5f73c-ca60-43c1-bdb2-702800347e7e
msc.legacyurl: /visual-studio/overview/2013/aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes
msc.type: authoredcontent
ms.openlocfilehash: 22d4d4afd6963f23d6cfef1745a859c20b69d599
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/17/2019
ms.locfileid: "59422996"
---
# <a name="aspnet-and-web-tools-20132--for-visual-studio-2013-release-notes"></a>Rozszerzenie ASP.NET and Web Tools 2013.2 dla programu Visual Studio 2013 — informacje o wersji

przez [firmy Microsoft](https://github.com/microsoft)

## <a name="installation-notes"></a>Uwagi dotyczące instalacji

ASP.NET and Web Tools dla programu Visual Studio Tools 2013.2 są powiązane w Instalatorze głównym i można je pobrać jako część [Visual Studio 2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521).

## <a name="documentation"></a>Dokumentacja

Samouczki i inne informacje o ASP.NET and Web Tools dla programu Visual Studio Tools 2013.2 są dostępne z [witryny sieci web platformy ASP.NET](https://www.asp.net/).

## <a name="software-requirements"></a>Wymagania programowe

ASP.NET and Web Tools for Visual Studio 2013.2 requires Visual Studio 2013.

## <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-20132"></a>Nowe funkcje w ASP.NET and Web Tools dla programu Visual Studio Tools 2013.2

W poniższych sekcjach opisano funkcje, które zostały wprowadzone w wersji.

- [One ASP.NET Project Templates](#oneaspnet)
- [Obsługa protokołu SSL, podczas uruchamiania aplikacji sieci Web w usługach IIS Express](#ssl)
- [Ulepszenia edytora sieci Web programu Visual Studio](#vswebeditor)
- [Łączność z przeglądarkami](#browserlink)
- [Obsługa usługi Azure App Service Web Apps w programie Visual Studio](#waws)
- [Utwórz zasoby zdalne platformy Azure, podczas tworzenia nowego projektu sieci Web](#AzureResources)
- [Web Publish ulepszenia](#webpublish)
- [ASP.NET Scaffolding](#scaffolding)
- [NuGet 2.8.1](#nuget)
- [ASP.NET Web Forms](#webforms)
- [ASP.NET MVC 5.1.2](#mvc)
- [ASP.NET Web API 2.1.2](#webapi)
- [ASP.NET Web Pages 3.1.2](#webpages)
- [Entity Framework 6.1](#ef)
- [ASP.NET Identity 2.0.0](#identity)
- [Składniki Microsoft OWIN](#owin)
- [ASP.NET SignalR 2.0.2](#signalr)

<a id="oneaspnet"></a>
### <a name="one-aspnet-project-templates"></a>One ASP.NET Project Templates

- Aktualizacje szablony projektów programu ASP.NET do obsługi potwierdzenie konta i resetowania hasła.
- Aktualizowanie szablonu ASP.NET Web API, aby zapewnić obsługę uwierzytelniania za pomocą na lokalne konta organizacji.
- Szablon ASP.NET SPA zawiera teraz uwierzytelniania, która opiera się na widoku strony MVC i serwera. Szablon zawiera kontrolkę webapi, który może zostać oceniony jedynie przez uwierzytelnionych użytkowników.

<a id="ssl"></a>
### <a name="support-ssl-when-launching-web-applications-on-iis-express"></a>Obsługa protokołu SSL, podczas uruchamiania aplikacji sieci Web w usługach IIS Express

Aby wyeliminować ostrzeżenie o zabezpieczeniach podczas przeglądania i debugowania HTTPS na hoście lokalnym, dodaliśmy okna dialogowego do zezwalania na program Internet Explorer i Chrome zaufania z podpisem własnym usług IIS express certyfikatu SSL.

Na przykład można ustawić właściwość projekt sieci web do używania protokołu SSL. Kliknij przycisk F4, aby wyświetlić okno dialogowe właściwości. Zmiana **włączony protokół SSL** na wartość true. Skopiuj adres URL protokołu SSL.

![Właściwość włączony protokół SSL](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image1.png)

Ustaw kartę sieci web strony właściwości projektu sieci web do używania protokołu HTTPS na podstawie adresu URL (adres URL SSL będzie `https://localhost:44300/` , chyba że wcześniej utworzono witryn sieci Web protokołu SSL.)

![Ustaw adres URL projektu (HTTPS)](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image2.png)

Naciśnij klawisze CTRL + F5, aby uruchomić aplikację. Postępuj zgodnie z instrukcjami na ufanie który usługi IIS Express został wygenerowany certyfikat z podpisem własnym.

![Ostrzeżenie protokołu SSL](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image3.png)

Odczyt **ostrzeżenie o zabezpieczeniach** okna dialogowego, a następnie kliknij przycisk **tak** Jeśli chcesz zainstalować certyfikat reprezentujący localhost.

![Ostrzeżenie o zabezpieczeniach](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image4.png)

Witryna będzie widoczny w programu Internet Explorer lub Chrome bez ostrzeżenia dotyczącego certyfikatu w przeglądarce.

![Strona HTTPS bez ostrzeżenia](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image5.png)

Firefox używa magazynu certyfikatów, dzięki czemu zostanie wyświetlone ostrzeżenie.

<a id="vswebeditor"></a>
### <a name="visual-studio-web-editor-enhancements"></a>Ulepszenia edytora sieci Web programu Visual Studio

- **Nowy element projektu w formacie JSON i edytor**: Dodaliśmy element projektu w formacie JSON i Edytor programu Visual Studio. Bieżące funkcje edytora JSON obejmują kolorowanie, składnia sprawdzania poprawności, uzupełnianie nawiasów, tworzenie konspektu, ustawienia opcji narzędzi i innych.

    ![Edytor JSON](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image6.png)

    Technologia IntelliSense obsługuje teraz [schematu JSON](http://json-schema.org/) w wersji 3 i 4. Brak schematu pola kombi, aby wybrać istniejące schematy, Edytuj ścieżkę lokalnego schematu lub po prostu przeciągnij upuść plik projektu w formacie JSON do niej, aby uzyskać ścieżkę względną.

    ![Funkcja Intellisense JSON](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image7.png)    ![Edytor schematów JSON](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image8.png)
- **Nowy edytor Sass (SCSS)**: Dodaliśmy mniej w programie VS2013 RTM, a w efekcie powstał elementu projektu Sass i edytor. Edytor sass funkcje są porównywalne z edytora LESS i obejmują kolorowanie, zmienną i funkcja IntelliSense domieszki, komentarz lub usuń znaczniki komentarza, szybkie informacje, formatowanie, sprawdzanie poprawności składni, tworzenie konspektu, przejdź do definicji, selektor kolorów narzędzia do opcji ustawienia itp.

    ![Dodaj nowy element: Arkusz stylów języka SCSS](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image9.png)    ![Edytor arkusza stylów](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image10.png)
- **Nowy selektor adresu URL w pliku HTML, Razor, CSS LESS i Sass dokumentów:** VS 2013 są dostarczane z programem nie Wybór adresu URL poza stron formularzy sieci Web. Nowy selektor adres URL dla HTML, Razor, CSS, LESS i Sass edytory jest bezpłatne okna dialogowego, fluent selektor Pisownia, który rozumie ".." i pliku filtrów odpowiednio listy tagów img lub łącza.

    ![Wybór adresu URL dla tagu obrazu](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image11.png)    ![Wybór adresu URL dla widoków](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image12.png)    ![Adres URL selektora CSS](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image13.png)
- **Aktualizacje do edytora LESS, dodając więcej funkcji**
- **Uaktualnienie Intellisense knockout**: Dodaliśmy niestandardowa składnia KnockOut dla intelliSense programu VS "viewModel ko vs edytora:" składni. Może służyć do powiązania do wielu modeli widok na stronie przy użyciu komentarzy w postaci:

    ![Knockout Intellisense](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image14.png)

    Dodaliśmy również obsługę zagnieżdżonych ViewModel funkcja IntelliSense, dzięki czemu użytkownik może przejść do głęboko zagnieżdżonych obiektów na ViewModel.

    `<div data-bind="text: foo.bar.baz.etc" />`

    Funkcja IntelliSense wyświetlana jest pełną obsługą technologii IntelliSense obiekt JavaScript.

    ![Pełny obiekt JavaScript IntelliSense przedstawiający](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image15.png)
- **Nowy selektor adresu URL w pliku HTML, Razor, CSS LESS i Sass dokumentów**: VS 2013 są dostarczane z programem nie Wybór adresu URL poza stron formularzy sieci Web. Nowy selektor adres URL dla HTML, Razor, CSS, LESS i Sass edytory jest bezpłatne okna dialogowego, fluent selektor Pisownia, który rozumie ".." i pliku filtrów odpowiednio listy tagów img lub łącza.

    ![Wybór adresu URL dla tagu obrazu](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image16.png)    ![Wybór adresu URL dla widoków](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image17.png)    ![Adres URL selektora CSS](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image18.png)

<a id="browserlink"></a>
### <a name="browser-link"></a>Łączność z przeglądarkami

- Łączność z przeglądarkami teraz obsługuje połączenia HTTPS i wyświetli listę, na pulpicie nawigacyjnym za pomocą innych połączeń tak długo, jak certyfikat jest zaufany przez przeglądarkę.
- Statyczne mapowanie źródła HTML
- SPA obsługuje mapowanie danych
- Automatyczna aktualizacja mapowania danych

<a id="waws"></a>
### <a name="support-for-azure-app-service-web-apps-in-visual-studio"></a>Obsługa usługi Azure App Service Web Apps w programie Visual Studio

- **Zaloguj się pomocy technicznej systemu Azure.**
- **Zdalne debugowanie i zdalne dla aplikacji sieci web**: Obsługujemy teraz [zdalne debugowanie aplikacji sieci web w usłudze Azure App Service](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio) i zdalne wyświetlanie plików zawartości aplikacji sieci web w Eksploratorze serwera.

<a id="AzureResources"></a>
### <a name="create-remote-azure-resources-when-creating-a-new-web-project"></a>Utwórz zasoby zdalne platformy Azure, podczas tworzenia nowego projektu sieci Web

Dodaliśmy Azure ["Utwórz zasoby zdalne"](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet) pola wyboru w oknie dialogowym nowej aplikacji sieci web. Wybierając je, można zintegrować środowisko tworzenia nowej aplikacji sieci web, skonfigurowanie usługi Azure site publikowania, testowanie i tworzenie profilu publikowania w kilku prostych krokach.

![Nowy projekt z zasobami platformy Azure](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image19.png)![Publikowanie na platformie Azure](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image20.png)

<a id="webpublish"></a>
### <a name="web-publish-enhancements"></a>Web Publish ulepszenia

- Ulepszone środowisko użytkownika do publikacji.

<a id="scaffolding"></a>
### <a name="aspnet-scaffolding"></a>Funkcja tworzenia szkieletu ASP.NET

- **Obsługa typu wyliczeniowego:** Jeśli model używa typów wyliczeniowych, generator szkieletu MVC spowoduje wygenerowanie listy rozwijanej dla wyliczenia. Ta metoda korzysta pomocników wyliczenia w MVC.
- **Uruchomienie pomocy technicznej**: Aktualizowana szablony EditorFor w MVC Scaffolding, dlatego używają ładowania klasy.
- **Pakiet pomocy technicznej**: MVC i Web API generatory doda 5.1 pakietów dla platformy MVC i interfejs API sieci Web

Poniższe zrzuty ekranu pokazują tworzenie szkieletów modeli.

- Kod modelu:

     ![Model kodu](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image21.png)
- Kompilowanie kodu modelu, kliknij prawym przyciskiem myszy i wybierz **Dodaj**, **nowy element szkieletu**.

     ![Dodaj nowy element szkieletowy](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image22.png)
- Wybierz **kontroler MVC5 z widokami używający narzędzia Entity Framework**:

     ![Dodaj nowy kontroler MVC5 z widokami](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image23.png)
- **Dodawanie kontrolera** przy użyciu modelu:

    ![](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image24.png)
- Sprawdź kod generowany, na przykład zawiera Views/WeekdayModels/Edit.cshtml `@Html.EnumDropDownListFor`: ![Widok zawierający EnumDropDownListFor](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image25.png)
- Uruchom stronę, aby zobaczyć combobox wyliczenia wygenerowane, zwróć uwagę, że jeśli może mieć wartości null, ciągiem pustym można wybrać pola kombi. Na przykład **Utwórz** strona zawiera następujące czynności:

    ![Pole kombi, umożliwiając pusty ciąg](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image26.png)

<a id="nuget"></a>
### <a name="nuget-281"></a>NuGet 2.8.1

NuGet 2.8.1, który RTM został wydany w kwietnia 2014 r. Poniżej przedstawiono najważniejsze punkty w informacjach o wersji, ale Sprawdź, czy [pełne uwagi do wersji](http://docs.nuget.org/docs/release-notes/nuget-2.8) Aby uzyskać więcej informacji na temat tych zmian.

- **TARGET Windows Phone 8.1 aplikacji**: NuGet 2.8.1 teraz wspiera przy użyciu monikerów target framework "WindowsPhoneApp", "WPA", "WindowsPhoneApp81" i "WPA81" w aplikacji programu Windows Phone 8.1.

- **Stosowanie poprawek do rozpoznawania zależności**: Podczas rozpoznawania zależności pakietów, NuGet w przeszłości została zaimplementowana strategii wybierania Najniższa wersja pakietu głównych i pomocniczych, spełniające zależności w pakiecie. W przeciwieństwie do wersji głównych i pomocniczych, wersja poprawki zabezpieczeń był zawsze rozpoznać najwyższa wersja. Chociaż zachowanie było intencjami, utworzyć braku determinizm dla instalowanie pakietów z zależnościami.
- **Przełącznik DependencyVersion**: Chociaż zmienia NuGet 2.8 *domyślne* zachowanie dla rozpoznawania zależności, dodaje także bardziej precyzyjną kontrolę nad procesem rozpoznawania zależności za pośrednictwem przełącznika - DependencyVersion w konsoli Menedżera pakietów. Przełącznik umożliwia rozpoznawania zależności, aby Najniższa wersja możliwe (zachowanie domyślne), najnowsza wersja to możliwe, lub najwyższy drobnych lub wersja poprawki zabezpieczeń. Ta opcja działa tylko dla install-package polecenia programu powershell.
- **Atrybut DependencyVersion**: Oprócz przełącznika - DependencyVersion szczegóły przedstawiono powyżej, NuGet ma również dozwolone dla możliwości można ustawić nowy atrybut w pliku nuget.config Definiowanie co to jest wartość domyślna, jeśli nie określono przełącznika - DependencyVersion w wywołania Install-package. Ta wartość obowiązują również w oknie dialogowym Menedżer pakietów NuGet dla wszystkich operacji pakietu instalacji. Aby ustawić tę wartość, należy dodać atrybut poniżej do pliku nuget.config:

    `<config> <add key="dependencyversion" value="Highest" /> </config>`
- **Podgląd operacji NuGet za pomocą - WhatIf**: Niektóre pakiety NuGet może mieć wykresy zależności szczegółowe i jako takie on być przydatne podczas instalacji, odinstalowywania i zaktualizować operację, aby najpierw sprawdzić, co się stanie. NuGet 2.8 dodaje standardowy programu PowerShell — co zrobić, jeśli przełączyć się do polecenia install-package, odinstaluj pakiet i pakiet aktualizacji, umożliwiające wizualizowanie całego zamknięcia pakietów, do których zostanie zastosowana polecenia.
- **Obniżyć wersję pakietu**: Nie jest niczym niezwykłym, aby zainstalować wstępną wersję pakietu w celu zbadania nowe funkcje i zdecydować wycofać do ostatniego stabilnej wersji. Przed NuGet 2.8 to wieloetapowy proces odinstalowywania wstępną wersję pakietu oraz jego zależności, a następnie zainstaluj starszą wersję. Za pomocą NuGet 2.8, pakiet aktualizacji będzie teraz wycofać zamknięcia cały pakiet (np. drzewo zależności pakietu) do poprzedniej wersji.
- **Tworzenie zależności**: Wiele różnych typów funkcji mogą być dostarczane jako pakietów NuGet — w tym narzędzia, które są używane do optymalizacji procesu rozwoju. Te składniki mogą być zarejestrowana w tworzeniu nowego pakietu, należy rozważyć nie opublikowany zależność nowy pakiet, gdy jest ona nowsza. NuGet 2.8 umożliwia pakietu do identyfikacji w pliku .nuspec developmentDependency. Po zainstalowaniu tych metadanych również dodane do pliku packages.config projektu, do którego pakiet został zainstalowany. Podczas tego pliku packages.config później jest analizowana pod kątem zależności NuGet podczas pakowania nuget.exe, wykluczy tych zależności, oznaczone jako zależności rozwoju.
- **Pliki poszczególnych packages.config na różnych platformach**: Podczas opracowywania aplikacji dla wielu platform docelowych jest często mają różne pliki projektów dla poszczególnych środowisk odpowiednich kompilacji. Jest również typowe korzystanie z różnych pakietach NuGet w plikach inny projekt, pakiety są dostępne dla różnych poziomów pomocy technicznej dla różnych platform. NuGet 2.8 zapewnia ulepszoną obsługę tego scenariusza, tworząc packages.config różnych plików dla plików do innego projektu specyficznego dla platformy.
- **Powrót do lokalnej pamięci podręcznej**: Chociaż pakiety NuGet są zwykle używane z galerii zdalnego takich jak [galerii pakietów NuGet](http://www.nuget.org) połączenia z siecią, istnieje wiele scenariuszy, w których klient nie jest połączony. Bez połączenia sieciowego klienta programu NuGet nie mógł pomyślnie zainstalować pakiety — nawet wtedy, gdy te pakiety zostały już na komputerze klienckim, w lokalnej pamięci podręcznej narzędzia NuGet. NuGet 2.8 dodaje automatyczne rezerwowego pamięci podręcznej do konsoli Menedżera pakietów.

    Funkcja rezerwowego pamięci podręcznej nie wymaga żadnych argumentów. Ponadto pamięć podręczna rezerwowego obecnie działa tylko w konsoli Menedżera pakietów — zachowanie aktualnie nie działa w oknie dialogowym Menedżer pakietów.
- **Poprawki błędów**: Jedną z głównych poprawki wprowadzone był poprawę wydajności w pakiecie aktualizacji-Zainstaluj ponownie polecenie.

    Oprócz te funkcje i poprawki wymienione wyżej wydajności ta wersja programu NuGet obejmuje wiele poprawek błędów. Wystąpiły 181 Suma problemów, które zostały rozwiązane w wydaniu. Aby uzyskać pełną listę prac elementy rozwiązane w NuGet 2.8, widok [narzędzie do śledzenia problemów NuGet](https://nuget.codeplex.com/workitem/list/advanced?release=NuGet%202.8&status=all) w tej wersji.

<a id="webforms"></a>
### <a name="aspnet-web-forms"></a>Formularze sieci Web ASP.NET

- Szablony formularzy sieci Web teraz pokazują, jak zrobić potwierdzenie konta i resetowania hasła dla produktu ASP.NET Identity.
- Formant źródła danych jednostki i dynamiczne dostawca danych dla platformy Entity Framework 6. Aby uzyskać więcej informacji, zobacz następujący wpis w blogu MSDN: [Dostawca danych dynamicznych i kontrola EntityDataSource Entity Framework 6](https://blogs.msdn.com/b/webdev/archive/2014/01/30/announcing-preview-of-dynamic-data-provider-and-entitydatasource-control-for-entity-framework-6.aspx).

<a id="mvc"></a>
### <a name="aspnet-mvc-512"></a>ASP.NET MVC 5.1.2

- [Atrybut ulepszenia routingu](../../../mvc/overview/releases/mvc51-release-notes.md#AttributeRouting)
- [Obsługa ładowania początkowego szablony Edytora](../../../mvc/overview/releases/mvc51-release-notes.md#Bootstrap)
- [Obsługa typu wyliczeniowego w widokach](../../../mvc/overview/releases/mvc51-release-notes.md#Enum)
- [Dyskretny kod obsługi MinLength / MaxLength atrybutów](../../../mvc/overview/releases/mvc51-release-notes.md#Unobtrusive)
- [Obsługa kontekstu "this" w dyskretnego kodu Ajax](../../../mvc/overview/releases/mvc51-release-notes.md#thisContext)
- Różne [poprawki błędów](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=MVC&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="webapi"></a>
### <a name="aspnet-web-api-212"></a>ASP.NET Web API 2.1.2

- [Obsługa błędów globalne](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#global-error)
- [Atrybut rozszerzenia routingu](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#attribute-routing)
- [Ulepszenia strony pomocy](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#help-page)
- [Obsługa IgnoreRoute](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#ignoreroute)
- [Element formatujący typu nośnika BSON](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#bson)
- [Lepsza obsługa async filtry](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#async-filters)
- [Zapytanie analizy dla klienta, formatowanie biblioteki](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#query-parsing)
- Różne [poprawki błędów](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=Web%20API%7cWeb%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="webpages"></a>
### <a name="aspnet-web-pages-312"></a>ASP.NET Web Pages 3.1.2

- Różne [poprawki błędów](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=Web%20Pages/Razor&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="ef"></a>
### <a name="entity-framework-61"></a>Entity Framework 6.1

Entity Framework został zaktualizowany do wersji 6.1 dla środowiska uruchomieniowego i narzędzi. Entity Framework (EF) 6.1 jest aktualizację pomocniczą do platformy Entity Framework 6 i obejmują szereg poprawek usterek i nowe funkcje. Szczegółowe informacje na temat EF6.1, w tym łącza do dokumentacji dla nowych funkcji, zobacz [Historia wersji programu Entity Framework](https://msdn.microsoft.com/data/jj574253). Nowe funkcje w tej wersji:

- **Narzędzia konsolidacji** zapewnia spójny sposób do utworzenia nowego modelu platformy EF. Ta funkcja rozszerza Kreator modelu danych jednostki ADO.NET do obsługi tworzenia modeli Code First, w tym odtwarzania z istniejącej bazy danych. Funkcje te wcześniej były dostępne w wersji Beta jakość EF Power Tools.
- **Obsługa błędów zatwierdzania transakcji** udostępnia nowe [System.Data.Entity.Infrastructure.CommitFailureHandler](https://msdn.microsoft.com/library/system.data.entity.infrastructure.commitfailurehandler(v=vs.113).aspx) co sprawia, że korzystanie z możliwości nowo wprowadzonych w celu przechwycenia operacji transakcji. **CommitFailureHandler** umożliwia automatyczne odzyskiwanie po awarii połączenia, przy jednoczesnym Zatwierdzanie transakcji.
- **IndexAttribute** umożliwia indeksy określoną przez umieszczenie atrybutu na właściwości (lub właściwości) w modelu Code First. Kod najpierw następnie utworzy odpowiedni indeks w bazie danych.
- **Mapowania publicznego interfejsu API** zapewnia dostęp do informacji EF ma na sposobie mapowania właściwości i typy kolumn i tabel w bazie danych. W poprzednich wersjach tego interfejsu API były wewnętrznego.
- **Możliwość konfigurowania interceptory za pomocą pliku App/Web.config**(umożliwiając, interceptory do dodania bez konieczności ponownego kompilowania aplikacji).
- **DatabaseLogger** jest nowy interceptor, która ułatwia wszystkie operacje bazy danych w pliku dziennika. W połączeniu z poprzedniej funkcji dzięki temu można łatwo przełączać rejestrowanie operacji bazy danych dla wdrożonej aplikacji bez konieczności ponownej kompilacji.
- **Wykrywanie zmiany modelu migracje** został ulepszony, aby były szkieletu migracje bardziej precyzyjne; wydajności procesu wykrywania zmian również znacznie rozszerzono.
- **Ulepszenia wydajności** w tym operacje niższych bazy danych podczas inicjowania, optymalizacje dla porównania równości wartości null w zapytaniach LINQ szybciej wyświetlać generowania (Tworzenie modelu) w dalszych scenariuszach i bardziej wydajne materializacja śledzonych jednostek z wielu skojarzeń.

<a id="identity"></a>
### <a name="aspnet-identity-200"></a>ASP.NET Identity 2.0.0

- **Uwierzytelnianie dwuskładnikowe**: Tożsamości ASP.NET obsługuje teraz uwierzytelnianie dwuskładnikowe. Uwierzytelnianie dwuskładnikowe zapewnia dodatkową warstwę zabezpieczeń do kont użytkowników w przypadku, gdy hasło zostanie naruszone. Istnieje również ochronę ataków siłowych na kody dwuskładnikowe.
- **Blokada konta:** Zapewnia sposób zablokowania użytkownika, jeśli użytkownik wprowadzi nieprawidłowe ich haseł lub kodów two-Factor Authentication. Liczba nieudanych prób podania i zakres czasu dla użytkowników będą zablokowani można skonfigurować. Deweloper możliwość wyłączenia blokady konta dla określonych kont użytkownika powinno być są konieczne.
- **Potwierdzenie konta:** System tożsamości ASP.NET obsługuje teraz potwierdzenie konta. Jest to dość typowy scenariusz w większości witryn sieci Web już dziś, where, po zarejestrowaniu nowego konta w witrynie internetowej, należy potwierdzić swój adres e-mail, przed rozpoczęciem żadnych czynności w witrynie sieci Web. Potwierdzenie adresu e-mail jest przydatne, ponieważ uniemożliwia fikcyjne kont tworzona. Jest to bardzo przydatne w przypadku korzystania z poczty e-mail jako metody komunikacji z użytkownikami witryny sieci Web, takimi jak Forum witryny, bankowość, handlu elektronicznego i społecznościowych witryn sieci web.
- **Resetowanie hasła:** Hasło jest to funkcja, której użytkownik mogą resetować swoje hasła, gdy zapomniane hasła.
- **Sygnatura bezpieczeństwa (Wyloguj się wszędzie):** Obsługująca sposób ponownego generowania tokenu zabezpieczeń dla użytkownika w przypadku, gdy użytkownik zmieni swoje hasło lub innych zabezpieczeń powiązane informacje, takie jak usuwanie skojarzone logowania (np. Facebook, Google, Microsoft Account i tak dalej). Jest to niezbędne, aby upewnić się, że wszystkie tokeny generowane przy użyciu starego hasła nie są unieważniane. W przykładowym projekcie Jeśli zmienisz hasło użytkownika następnie wygenerowaniu nowy token dla użytkownika i wszystkie poprzednie tokeny są unieważniane. Funkcja ta zapewnia dodatkową warstwę zabezpieczeń do aplikacji, ponieważ po zmianie hasła, użytkownik zostanie wylogowany z dowolnego miejsca (dla innych przeglądarek) gdzie użytkownik zalogował się do tej aplikacji.
- **Zmień typ klucza podstawowego można rozszerzyć o użytkownikami i rolami**: W wersji 1.0 tożsamości ASP.NET typ klucza podstawowego dla tabeli, użytkownikami i rolami był ciągów. Oznacza to, gdy system tożsamości ASP.NET został zachowany w programie SQL Server przy użyciu platformy Entity Framework, firma Microsoft była używana nvarchar. Wystąpiły wiele rozważania tej domyślnej implementacji w witrynie Stack Overflow i na podstawie przychodzącego opinii. Udostępniliśmy hook rozszerzalności, w której można określić, jakie powinny być klucza podstawowego tabeli użytkownikami i rolami. Tego punktu zaczepienia rozszerzalności jest szczególnie przydatne, jeśli jest przeprowadzana migracja aplikacji, a aplikacja została UserId przechowywania są identyfikatory GUID lub liczby całkowite.
- **Obsługuje interfejs IQueryable użytkownikami i rolami**: Dodano obsługę dla elementu IQueryable na UsersStore i RolesStore, można łatwo uzyskać listę użytkowników i ról.
- **Operacja usunięcia pomocy technicznej za pomocą Menedżera UserManager**
- **Indeksowanie UserName**: W implementacji programu ASP.NET Identity Entity Framework dodaliśmy unikatowy indeks o nazwę użytkownika przy użyciu nowego IndexAttribute w programie EF 6.1.0. To sprawia, że się, że nazwy użytkowników są unikatowe i nie było żadnych wyścigu, w którym użytkownik może wystąpić zduplikowanych nazw użytkowników.
- **Modułu weryfikacji hasła rozszerzone:** Modułu weryfikacji hasła, która została wydana w wersji 1.0 tożsamości ASP.NET został sprawdzania poprawności hasła dosyć została tylko weryfikacji minimalnej długości. Brak nowego modułu weryfikacji hasła, który zapewnia większą kontrolę nad złożoności hasła. Należy pamiętać, że nawet wtedy, gdy włączysz wszystkie ustawienia w to hasło, zachęcamy do włączenia uwierzytelniania dwuskładnikowego dla kont użytkowników.
- **Oprogramowanie pośredniczące IdentityFactory / CreatePerOwinContext**:

    - **Menedżer użytkowników**: Wdrożenie fabryki umożliwia uzyskanie wystąpienia menedżera UserManager z kontekstu OWIN. Ten wzorzec jest podobne do używamy w celu uzyskania słowniku z kontekstu OWIN, logowanie i wylogowanie. Jest to zalecany sposób uzyskania wystąpienia menedżera UserManager na żądanie dla aplikacji.
    - **DbContextFactory**: Tożsamości ASP.NET używa platformy Entity Framework utrwalanie systemu tożsamości w programie SQL Server. W tym celu System tożsamości odwołuje się do ApplicationDbContext. Oprogramowanie pośredniczące DbContextFactory Zwraca wystąpienie klasy ApplicationDbContext na żądanie, które można użyć w aplikacji.
- **Pakiet NuGet przykłady tożsamości ASP.NET**: Pakiet NuGet przykłady można ułatwić Instalowanie i uruchamianie przykładów dla produktu ASP.NET Identity i stosuj najlepsze rozwiązania. To jest przykład aplikacji platformy ASP.NET MVC. Zmodyfikuj kod do własnych aplikacji przed wdrożeniem to w środowisku produkcyjnym. Plik powinien być zainstalowany w pustej aplikacji ASP.NET. Aby uzyskać więcej informacji o pakiecie przejdź do następujący wpis w blogu: [Ogłoszenie RTM produktu ASP.NET Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx)

<a id="owin"></a>
### <a name="microsoft-owin-components"></a>Składniki Microsoft OWIN

Wystąpiło wiele błędów, które zostały rozwiązane w tej wersji. Zobacz [informacje o wersji programu 2.1.0 wersji](https://katanaproject.codeplex.com/releases/view/113281) więcej szczegółowych informacji.

<a id="signalr"></a>
### <a name="aspnet-signalr-202"></a>ASP.NET SignalR 2.0.2

Wystąpiło wiele błędów, które zostały rozwiązane w tej wersji. Zobacz [informacje o wersji programu pkt 2.0.2 wersji](https://github.com/SignalR/SignalR/releases/tag/2.0.2) więcej szczegółowych informacji.

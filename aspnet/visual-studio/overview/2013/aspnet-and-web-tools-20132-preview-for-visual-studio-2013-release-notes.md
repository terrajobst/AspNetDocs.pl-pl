---
uid: visual-studio/overview/2013/aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes
title: ASP.NET and Web Tools 2013,2 dla Visual Studio 2013 informacji o wersji | Microsoft Docs
author: microsoft
description: ''
ms.author: riande
ms.date: 03/06/2014
ms.assetid: 7ef5f73c-ca60-43c1-bdb2-702800347e7e
msc.legacyurl: /visual-studio/overview/2013/aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes
msc.type: authoredcontent
ms.openlocfilehash: 22d4d4afd6963f23d6cfef1745a859c20b69d599
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78623194"
---
# <a name="aspnet-and-web-tools-20132--for-visual-studio-2013-release-notes"></a>Rozszerzenie ASP.NET and Web Tools 2013.2 dla programu Visual Studio 2013 — informacje o wersji

przez [firmę Microsoft](https://github.com/microsoft)

## <a name="installation-notes"></a>Uwagi dotyczące instalacji

ASP.NET and Web Tools programu Visual Studio 2013,2 są powiązane z instalatorem głównym i można je pobrać w ramach [Visual Studio 2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521).

## <a name="documentation"></a>Dokumentacja

Samouczki i inne informacje o ASP.NET and Web Tools dla programu Visual Studio 2013,2 są dostępne w [witrynie sieci Web ASP.NET](https://www.asp.net/).

## <a name="software-requirements"></a>Wymagania programowe

ASP.NET and Web Tools dla programu Visual Studio 2013,2 wymaga Visual Studio 2013.

## <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-20132"></a>Nowe funkcje w ASP.NET and Web Tools dla programu Visual Studio 2013,2

W poniższych sekcjach opisano funkcje wprowadzone w wersji.

- [Jeden szablon projektu ASP.NET](#oneaspnet)
- [Obsługa protokołu SSL podczas uruchamiania aplikacji sieci Web na IIS Express](#ssl)
- [Udoskonalenia edytora sieci Web programu Visual Studio](#vswebeditor)
- [Łączność z przeglądarkami](#browserlink)
- [Obsługa Web Apps Azure App Service w programie Visual Studio](#waws)
- [Tworzenie zdalnych zasobów platformy Azure podczas tworzenia nowego projektu sieci Web](#AzureResources)
- [Udoskonalenia publikowania w sieci Web](#webpublish)
- [Tworzenie szkieletu ASP.NET](#scaffolding)
- [NuGet 2.8.1](#nuget)
- [ASP.NET Web Forms](#webforms)
- [ASP.NET MVC 5.1.2](#mvc)
- [ASP.NET Web API 2.1.2](#webapi)
- [ASP.NET strony sieci Web 3.1.2](#webpages)
- [Entity Framework 6,1](#ef)
- [ASP.NET Identity 2.0.0](#identity)
- [Składniki OWIN firmy Microsoft](#owin)
- [ASP.NET sygnalizujący 2.0.2](#signalr)

<a id="oneaspnet"></a>
### <a name="one-aspnet-project-templates"></a>Jeden szablon projektu ASP.NET

- Aktualizacje szablonów projektów ASP.NET do obsługi potwierdzenia konta i resetowania hasła.
- Aktualizowanie szablonu interfejsu API sieci Web ASP.NET w celu obsługi uwierzytelniania przy użyciu lokalnych kont organizacji.
- Szablon ASP.NET SPA zawiera teraz uwierzytelnianie oparte na widokach po stronie MVC i serwerze. Szablon zawiera kontroler WebAPI, do którego można uzyskać dostęp tylko dla uwierzytelnionych użytkowników.

<a id="ssl"></a>
### <a name="support-ssl-when-launching-web-applications-on-iis-express"></a>Obsługa protokołu SSL podczas uruchamiania aplikacji sieci Web na IIS Express

Aby wyeliminować ostrzeżenie o zabezpieczeniach podczas przeglądania i debugowania protokołu HTTPS na hoście lokalnym, dodaliśmy okno dialogowe umożliwiające PROGRAMowi Internet Explorer i przeglądarce Chrome zaufać certyfikatowi SSL z podpisem własnym.

Na przykład właściwość projektu sieci Web można ustawić tak, aby korzystała z protokołu SSL. Kliknij przycisk F4, aby wyświetlić okno dialogowe właściwości. Zmień wartość **SSL z włączony** na true. Skopiuj adres URL protokołu SSL.

![Właściwość z włączonym protokołem SSL](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image1.png)

Ustaw kartę sieci Web strony właściwości projektu sieci Web na adres URL oparty na protokole HTTPS (adres URL protokołu SSL zostanie `https://localhost:44300/`, chyba że wcześniej utworzono witryny sieci Web SSL).

![Ustaw adres URL projektu (HTTPS)](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image2.png)

Naciśnij klawisze CTRL+F5, aby uruchomić aplikację. Postępuj zgodnie z instrukcjami, aby zaufać certyfikatowi z podpisem własnym, który IIS Express wygenerowany.

![Ostrzeżenie SSL](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image3.png)

Przeczytaj okno dialogowe **Ostrzeżenie o zabezpieczeniach** , a następnie kliknij przycisk **tak** , jeśli chcesz zainstalować certyfikat reprezentujący localhost.

![Ostrzeżenie o zabezpieczeniach](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image4.png)

Lokacja będzie wyświetlana w programie IE lub Chrome bez ostrzeżenia dotyczącego certyfikatu w przeglądarce.

![Strona HTTPS bez ostrzeżeń](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image5.png)

Program Firefox używa własnego magazynu certyfikatów, więc wyświetli ostrzeżenie.

<a id="vswebeditor"></a>
### <a name="visual-studio-web-editor-enhancements"></a>Udoskonalenia edytora sieci Web programu Visual Studio

- **Nowy element projektu JSON i Edytor**: dodaliśmy element projektu JSON i edytor do programu Visual Studio. Bieżące funkcje edytora JSON obejmują kolorowanie, walidację składni, uzupełnianie nawiasów klamrowych, konspekt, Ustawianie opcji narzędzi i nie tylko.

    ![Edytor JSON](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image6.png)

    Technologia IntelliSense obsługuje teraz [schemat JSON](http://json-schema.org/) v3 i v4. Istnieje pole kombi schematu, w którym można wybrać istniejące schematy, edytować ścieżkę do lokalnego schematu lub po prostu przeciągnąć do niego plik JSON w celu uzyskania ścieżki względnej.

    ![Funkcja IntelliSense JSON](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image7.png)    ![Edytor schematu JSON](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image8.png)
- **Nowy edytor Sass (SCSS)** : dodaliśmy mniej do VS2013 RTM i mamy teraz element projektu Sass i edytor. Funkcje edytora Sass są porównywalne z edytorem LESS i obejmują kolorowanie, zmienne i domieszki IntelliSense, komentarz/komentarz, szybkie informacje, formatowanie, sprawdzanie poprawności składni, tworzenie konspektu, edytowanie definicji, selektor kolorów, ustawienia opcji narzędzi itp.

    ![Dodaj nowy element: arkusz stylów SCSS](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image9.png)    ![Edytor arkusza stylów](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image10.png)
- **Nowy selektor adresów URL w dokumentach HTML, Razor, CSS, less i Sass:** Program VS 2013 dostarczany bez selektora adresów URL poza stronami formularzy sieci Web. Nowy selektor adresów URL dla edytorów HTML, Razor, CSS, LESS i Sass jest bezpłatnym selektorem tekstu, który ma świadomość ".." i filtruje listy plików odpowiednio do IMG tagów i linków.

    ![Selektor adresów URL dla tagu obrazu](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image11.png)    ![Selektor adresów URL dla widoków](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image12.png)    ![Selektor adresów URL dla CSS](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image13.png)
- **Aktualizacje w edytorze LESS przez dodanie większej liczby funkcji**
- **Odcinanie funkcji IntelliSense**: dodaliśmy niestandardową składnię odcinania dla programu vs IntelliSense, "ko-vs-Editor viewModel:" Syntax. Może służyć do powiązania z wieloma modelami widoku na stronie przy użyciu komentarzy w formularzu:

    ![Odcinanie funkcji IntelliSense](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image14.png)

    Dodano również obsługę zagnieżdżonej technologii IntelliSense ViewModel, dzięki czemu możesz przechodzić do głęboko zagnieżdżonych obiektów w ViewModel.

    `<div data-bind="text: foo.bar.baz.etc" />`

    Wyświetlana technologia IntelliSense to pełna funkcja IntelliSense obiektu JavaScript.

    ![Technologia IntelliSense pokazująca pełny obiekt JavaScript](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image15.png)
- **Nowy selektor adresów URL w formacie HTML, Razor, CSS, less i Sass dokumenty**: vs 2013 dostarczone bez SELEKTORA adresów URL poza stronami formularzy sieci Web. Nowy selektor adresów URL dla edytorów HTML, Razor, CSS, LESS i Sass jest bezpłatnym selektorem tekstu, który ma świadomość ".." i filtruje listy plików odpowiednio do IMG tagów i linków.

    ![Selektor adresów URL dla tagu obrazu](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image16.png)    ![Selektor adresów URL dla widoków](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image17.png)    ![Selektor adresów URL dla CSS](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image18.png)

<a id="browserlink"></a>
### <a name="browser-link"></a>Łącze przeglądarki

- Link przeglądarki obsługuje teraz połączenia HTTPS i zostanie wystawiony na pulpicie nawigacyjnym z innymi połączeniami, o ile certyfikat jest zaufany przez przeglądarkę.
- Mapowanie statycznego źródła HTML
- Obsługa SPA dla danych mapowania
- Autoaktualizacja danych mapowania

<a id="waws"></a>
### <a name="support-for-azure-app-service-web-apps-in-visual-studio"></a>Obsługa Web Apps Azure App Service w programie Visual Studio

- **Obsługa logowania na platformie Azure.**
- **Debugowanie zdalne i widok zdalny dla aplikacji sieci Web**: obsługujemy teraz [debugowanie zdalne dla aplikacji sieci Web w Azure App Service](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio) i widoku zdalnego plików zawartości aplikacji sieci Web w Eksploratorze serwera.

<a id="AzureResources"></a>
### <a name="create-remote-azure-resources-when-creating-a-new-web-project"></a>Tworzenie zdalnych zasobów platformy Azure podczas tworzenia nowego projektu sieci Web

Dodaliśmy pole wyboru platformy Azure ["Tworzenie zasobów zdalnych"](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet) w oknie dialogowym Nowa aplikacja sieci Web. Po wybraniu tej opcji będzie można zintegrować środowisko tworzenia nowej aplikacji sieci Web, skonfigurować witrynę publikowania platformy Azure do testowania i utworzyć profil publikowania w kilku prostych krokach.

![Nowy projekt z zasobami platformy Azure](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image19.png)![Publikowanie na platformie Azure](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image20.png)

<a id="webpublish"></a>
### <a name="web-publish-enhancements"></a>Udoskonalenia publikowania w sieci Web

- Ulepsz środowisko użytkownika do opublikowania.

<a id="scaffolding"></a>
### <a name="aspnet-scaffolding"></a>Tworzenie szkieletu ASP.NET

- **Obsługa wyliczenia:** Jeśli model używa typów wyliczeniowych, wówczas szkielet MVC będzie generował listę rozwijaną dla wyliczenia. Spowoduje to użycie pomocników wyliczenia w MVC.
- **Obsługa ładowania początkowego**: Zaktualizowano szablony EditorFor w obszarze tworzenia szkieletów MVC, aby używały klas Bootstrap.
- **Obsługa pakietów**: program MVC i szkielety interfejsu API sieci Web będą dodawać pakiety 5,1 dla MVC i interfejsu API sieci Web

Poniższe zrzuty ekranu przedstawiają modele szkieletowe.

- Kod modelu:

     ![Kod modelu](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image21.png)
- Skompiluj kod modelu, kliknij prawym przyciskiem myszy, a następnie wybierz polecenie **Dodaj**, **nowy element szkieletowy**.

     ![Dodaj nowy element szkieletowy](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image22.png)
- Wybierz **kontroler MVC5 z widokami, używając Entity Framework**:

     ![Dodaj nowy kontroler MVC5 z widokami](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image23.png)
- **Dodaj kontroler** przy użyciu modelu:

    ![](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image24.png)
- Sprawdź wygenerowany kod, na przykład widoki/WeekdayModels/Edit. cshtml zawiera `@Html.EnumDropDownListFor`: ![View zawierającego EnumDropDownListFor](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image25.png)
- Uruchom stronę, aby wyświetlić wygenerowane pole ComboBox wyliczenia. Zauważ, że jeśli wartość może być równa null, dla pola kombi można wybrać pusty ciąg. Na przykład na stronie **Tworzenie** są wyświetlane następujące elementy:

    ![Pole kombi zezwalające na pusty ciąg](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image26.png)

<a id="nuget"></a>
### <a name="nuget-281"></a>NuGet 2.8.1

Pakiet NuGet 2.8.1 RTM zostanie opublikowany w kwietniu 2014. Oto punkty najważniejsze z informacji o wersji, ale zapoznaj się z [pełnymi](http://docs.nuget.org/docs/release-notes/nuget-2.8) informacjami o wersji, aby uzyskać więcej informacji o tych zmianach.

- **Docelowa aplikacja Windows Phone 8,1**: 2.8.1 NuGet obsługuje teraz aplikacje przeznaczone do Windows Phone 8,1, używając monikerów platformy docelowej "WindowsPhoneApp", "WPA", "WindowsPhoneApp81" i "WPA81".

- **Rozpoznawanie poprawek dla zależności**: podczas rozpoznawania zależności pakietów pakiet NuGet ma historyczną strategię wybierania najmniejszej wersji głównej i pomocniczej, która spełnia zależności pakietu. W przeciwieństwie do wersji głównej i pomocniczej, wersja poprawki była zawsze rozwiązywana do najwyższej wersji. Mimo że zachowanie zostało prawidłowo zamierzone, utworzono brakujące ustalenia dotyczące instalowania pakietów z zależnościami.
- **Przełącznik DependencyVersion**: Chociaż program NuGet 2,8 zmienia *domyślne* zachowanie podczas rozpoznawania zależności, dodatkowo dodaje dokładniejszą kontrolę nad procesem rozpoznawania zależności za pośrednictwem przełącznika-DependencyVersion w konsoli Menedżera pakietów. Przełącznik włącza rozpoznawanie zależności do najmniejszej możliwej wersji (domyślnego zachowania), najwyższej możliwej wersji lub najwyższej wersji pomocniczej lub poprawki. Ten przełącznik działa tylko w przypadku instalacji pakietu w poleceniu programu PowerShell.
- **Atrybut DependencyVersion**: oprócz przełącznika-DependencyVersion podanego powyżej, pakiet NuGet może również ustawić nowy atrybut w pliku NuGet. config definiujący wartość domyślną, jeśli przełącznik-DependencyVersion nie jest określony w wywołaniu pakietu install-package. Ta wartość będzie również przestrzegana przez okno dialogowe Menedżera pakietów NuGet dla wszystkich operacji instalacji pakietu. Aby ustawić tę wartość, Dodaj poniższy atrybut do pliku NuGet. config:

    `<config> <add key="dependencyversion" value="Highest" /> </config>`
- **Podgląd operacji NuGet za pomocą-whatIf**: Niektóre pakiety NuGet mogą mieć szczegółowe wykresy zależności i w związku z tym mogą być przydatne podczas operacji instalowania, odinstalowywania lub aktualizacji, aby najpierw zobaczyć, co się stanie. Program NuGet 2,8 dodaje standardowy program PowerShell — co należy zrobić, aby przełączyć się do poleceń install-package, Uninstall-Package i Update-Package, aby umożliwić wizualizowanie całego zamknięcia pakietów, do których zostanie zastosowane polecenie.
- **Pakiet obniżania**poziomu: Instalowanie wersji wstępnej pakietu nie jest nietypowo w celu zbadania nowych funkcji, a następnie podjęcie decyzji o wycofaniu do ostatniej stabilnej wersji. Przed pakietem NuGet 2,8 jest to proces wieloetapowy odinstalowywania pakietu wstępnego wraz z jego zależnościami, a następnie instalowania wcześniejszej wersji. W przypadku programu NuGet 2,8 pakiet aktualizacji spowoduje teraz wycofanie całego zamknięcia pakietu (np. drzewa zależności pakietu) do poprzedniej wersji.
- **Zależności programistyczne**: wiele różnych typów możliwości może być dostarczanych jako pakiety NuGet — w tym narzędzia służące do optymalizowania procesu tworzenia oprogramowania. Te składniki, chociaż mogą być instrumentalem podczas tworzenia nowego pakietu, nie powinny być uważane za zależność od nowego pakietu podczas jego późniejszej publikacji. Program NuGet 2,8 umożliwia pakietowi identyfikację w pliku. nuspec jako developmentDependency. Po zainstalowaniu te metadane również zostaną dodane do pliku Packages. config projektu, w którym zainstalowano pakiet. Gdy plik Packages. config zostanie później przeanalizowany pod kątem zależności NuGet w pakiecie NuGet. exe, spowoduje to wykluczenie tych zależności jako zależności programistycznych.
- **Poszczególne pliki Packages. config dla różnych platform**: podczas tworzenia aplikacji dla wielu platform docelowych często istnieją różne pliki projektu dla każdego z odpowiednich środowisk kompilacji. Często używa się również różnych pakietów NuGet w różnych plikach projektu, ponieważ pakiety mają różne poziomy obsługi dla różnych platform. Program NuGet 2,8 zapewnia ulepszoną obsługę tego scenariusza, tworząc różne pliki Packages. config dla różnych plików projektu specyficznych dla platformy.
- **Powrót do lokalnej pamięci podręcznej**: Chociaż pakiety NuGet są zwykle używane z galerii zdalnej, takiej jak [Galeria NuGet](http://www.nuget.org) przy użyciu połączenia sieciowego, istnieje wiele scenariuszy, w których klient nie jest połączony. Bez połączenia sieciowego klient NuGet nie mógł pomyślnie zainstalować pakietów — nawet wtedy, gdy te pakiety znajdowały się już na komputerze klienta w lokalnej pamięci podręcznej NuGet. Pakiet NuGet 2,8 dodaje automatyczną rezerwę pamięci podręcznej do konsoli Menedżera pakietów.

    Funkcja rezerwy pamięci podręcznej nie wymaga żadnych argumentów polecenia. Ponadto rezerwa pamięci podręcznej obecnie działa tylko w konsoli Menedżera pakietów — zachowanie aktualnie nie działa w oknie dialogowym Menedżera pakietów.
- **Poprawki błędów**: jeden z najważniejszych poprawek błędów został ulepszony w poleceniu Update-Package-install.

    Oprócz tych funkcji i wyżej wymienionej poprawki wydajności ta wersja programu NuGet zawiera również wiele innych poprawek błędów. W wydaniu wystąpiło 181 całkowite problemy rozwiązane. Aby zapoznać się z pełną listą elementów roboczych ustalonych w programie NuGet 2,8, zobacz [Śledzenie problemów NuGet](https://nuget.codeplex.com/workitem/list/advanced?release=NuGet%202.8&status=all) dla tej wersji.

<a id="webforms"></a>
### <a name="aspnet-web-forms"></a>Formularze sieci Web ASP.NET

- Szablony formularzy sieci Web przedstawiają teraz sposób potwierdzenia konta i resetowania hasła dla ASP.NET Identity.
- Kontrola źródła danych jednostki i Dostawca danych dynamiczny dla Entity Framework 6. Aby uzyskać więcej informacji, zobacz następujący blog MSDN: [dostawca danych dynamicznych i Formant EntityDataSource dla Entity Framework 6](https://blogs.msdn.com/b/webdev/archive/2014/01/30/announcing-preview-of-dynamic-data-provider-and-entitydatasource-control-for-entity-framework-6.aspx).

<a id="mvc"></a>
### <a name="aspnet-mvc-512"></a>ASP.NET MVC 5.1.2

- [Udoskonalenia routingu atrybutów](../../../mvc/overview/releases/mvc51-release-notes.md#AttributeRouting)
- [Obsługa ładowania początkowego szablonów edytora](../../../mvc/overview/releases/mvc51-release-notes.md#Bootstrap)
- [Obsługa wyliczenia w widokach](../../../mvc/overview/releases/mvc51-release-notes.md#Enum)
- [Niewygodna obsługa atrybutów MinLength/MaxLength](../../../mvc/overview/releases/mvc51-release-notes.md#Unobtrusive)
- [Obsługa kontekstu "This" w niedyskretnym AJAX](../../../mvc/overview/releases/mvc51-release-notes.md#thisContext)
- Różne [poprawki błędów](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=MVC&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="webapi"></a>
### <a name="aspnet-web-api-212"></a>ASP.NET Web API 2.1.2

- [Globalna obsługa błędów](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#global-error)
- [Udoskonalenia routingu atrybutów](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#attribute-routing)
- [Ulepszenia stron pomocy](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#help-page)
- [Obsługa IgnoreRoute](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#ignoreroute)
- [BSON typ nośnika](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#bson)
- [Lepsza obsługa filtrów asynchronicznych](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#async-filters)
- [Analizowanie zapytań dla biblioteki formatowania klienta](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#query-parsing)
- Różne [poprawki błędów](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=Web%20API%7cWeb%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="webpages"></a>
### <a name="aspnet-web-pages-312"></a>ASP.NET strony sieci Web 3.1.2

- Różne [poprawki błędów](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=Web%20Pages/Razor&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="ef"></a>
### <a name="entity-framework-61"></a>Entity Framework 6,1

Entity Framework został zaktualizowany do wersji 6,1 dla środowiska uruchomieniowego i narzędzi. Entity Framework (EF) 6,1 to drobna aktualizacja Entity Framework 6 i zawiera szereg poprawek usterek i nowych funkcji. Aby uzyskać szczegółowe informacje na temat EF 6.1, w tym linki do dokumentacji nowych funkcji, zobacz [Entity Framework historię wersji](https://msdn.microsoft.com/data/jj574253). Nowe funkcje w tej wersji obejmują:

- **Konsolidacja narzędzi** zapewnia spójny sposób tworzenia nowego modelu EF. Ta funkcja rozszerza kreatora Entity Data Model ADO.NET, aby obsługiwał tworzenie modeli Code First, w tym odtwarzanie z istniejącej bazy danych. Te funkcje były wcześniej dostępne w jakości beta w narzędziach EF.
- **Obsługa niepowodzeń zatwierdzania transakcji** zapewnia nową funkcję [System. Data. Entity. Infrastructure. CommitFailureHandler](https://msdn.microsoft.com/library/system.data.entity.infrastructure.commitfailurehandler(v=vs.113).aspx) , która umożliwia przechwycenie operacji transakcji. **CommitFailureHandler** umożliwia automatyczne odzyskiwanie z błędów połączeń podczas zatwierdzania transakcji.
- **Indexattribute** umożliwia określenie indeksów przez umieszczenie atrybutu we właściwości (lub właściwości) w modelu Code First. Code First następnie utworzy odpowiedni indeks w bazie danych.
- **Interfejs API mapowania publicznego** zapewnia dostęp do informacji EF na temat tego, jak właściwości i typy są mapowane na kolumny i tabele w bazie danych. W poprzednich wersjach ten interfejs API był wewnętrzny.
- **Możliwość konfigurowania interceptorów za pośrednictwem pliku App/Web. config**(umożliwienie dodania interceptorów bez ponownego kompilowania aplikacji).
- **DatabaseLogger** jest nowym interceptorem, dzięki czemu można łatwo rejestrować wszystkie operacje bazy danych do pliku. W połączeniu z poprzednią funkcją umożliwia to łatwe przełączenie na rejestrowanie operacji bazy danych dla wdrożonej aplikacji bez konieczności ponownego kompilowania.
- Udoskonalono **wykrywanie zmian modelu migracji** , dzięki czemu migracja szkieletowa jest bardziej dokładna. znacznie Ulepszono wydajność procesu wykrywania zmian.
- **Ulepszenia wydajności** , w tym mniejsze operacje bazy danych podczas inicjowania, optymalizacje dla porównania równości o wartości null w zapytaniach LINQ, szybsze generowanie widoku (Tworzenie modelu) w większej liczbie scenariuszy i wydajniejsze materializację śledzonych jednostek z wieloma skojarzeniami.

<a id="identity"></a>
### <a name="aspnet-identity-200"></a>ASP.NET Identity 2.0.0

- **Uwierzytelnianie dwuskładnikowe**: ASP.NET Identity obsługuje teraz uwierzytelnianie dwuskładnikowe. Uwierzytelnianie dwuskładnikowe zapewnia dodatkową warstwę zabezpieczeń dla kont użytkowników w przypadku naruszenia bezpieczeństwa hasła. Istnieje również ochrona przed atakami na ataki w postaci odnoszące się do kodów dwóch czynników.
- **Blokada konta:** Umożliwia zablokowanie użytkownika, jeśli użytkownik wpisze nieprawidłowe hasło lub kody dwuskładnikowe. Można skonfigurować liczbę nieprawidłowych prób i przedział czasu dla użytkowników. Deweloper może opcjonalnie wyłączyć blokadę konta dla określonych kont użytkowników, jeśli są one potrzebne.
- **Potwierdzenie konta:** System ASP.NET Identity obsługuje teraz potwierdzenie konta. Jest to dość typowy scenariusz w większości witryn sieci Web dzisiaj, gdy zarejestrujesz się w celu utworzenia nowego konta w witrynie sieci Web, przed wykonaniem jakichkolwiek czynności w witrynie sieci Web musisz potwierdzić swój adres e-mail. Potwierdzenie adresu e-mail jest przydatne, ponieważ uniemożliwia tworzenie nieprawdziwych kont. Jest to bardzo przydatne, jeśli używasz poczty e-mail jako metody komunikowania się z użytkownikami witryny sieci Web, na przykład witrynami forum, bankowością, handlu elektronicznego lub społecznościowymi.
- **Resetowanie hasła:** Resetowanie hasła to funkcja, w której użytkownik może zresetować swoje hasła, jeśli zapomniano hasła.
- **Sygnatura zabezpieczeń (wylogowanie wszędzie):** Umożliwia ponowne wygenerowanie tokenu zabezpieczającego dla użytkownika w przypadkach, gdy użytkownik zmieni hasło lub wszelkie inne informacje związane z zabezpieczeniami, takie jak usunięcie skojarzonej nazwy logowania (np. Facebook, Google, konto Microsoft itd.). Jest to konieczne, aby upewnić się, że wszystkie tokeny wygenerowane ze starym hasłem są unieważnione. W przykładowym projekcie, jeśli zmienisz hasło użytkownika, zostanie wygenerowany nowy token dla użytkownika, a wszystkie poprzednie tokeny są unieważnione. Ta funkcja zapewnia dodatkową warstwę zabezpieczeń aplikacji od momentu zmiany hasła. użytkownik zostanie wylogowany z dowolnego miejsca (wszystkie inne przeglądarki), w którym użytkownik zalogował się do tej aplikacji.
- **Zmień typ klucza podstawowego na rozszerzalny dla użytkowników i ról**: w ASP.NET Identity 1,0 typ klucza podstawowego dla użytkowników tabeli i ról to ciągi. Oznacza to, że jeśli system ASP.NET Identity został utrwalony w SQL Server przy użyciu Entity Framework, użyto metody nvarchar. Wystąpiło wiele dyskusji na temat tej domyślnej implementacji na Stack Overflow i w oparciu o przychodzące informacje zwrotne. Podano punkt zaczepienia rozszerzalności, w którym można określić, co powinno być kluczem podstawowym tabeli użytkowników i ról. Ten hak rozszerzalności jest szczególnie przydatny w przypadku migrowania aplikacji, a aplikacja miała identyfikatory GUID lub liczby całkowite.
- **Obsługa platformy IQueryable dla użytkowników i ról**: Dodano obsługę platformy IQueryable w systemach UsersStore i RolesStore, można łatwo uzyskać listę użytkowników i ról.
- **Obsługa operacji usuwania przez użytkownika**
- **Indeksowanie na nazwie użytkownika**: w ASP.NET Identity Entity Framework implementacji dodaliśmy unikatowy indeks na nazwie użytkownika przy użyciu nowego elementu indexattribute w Ef 6.1.0. Daje to pewność, że nazwy użytkowników są zawsze unikatowe i nie istniały sytuacje, w których można było zakończyć Duplikowanie nazw użytkowników.
- **Ulepszone modułu sprawdzania haseł:** Moduł sprawdzania poprawności hasła, który został dostarczony w ASP.NET Identity 1,0 był dość podstawowym modułem sprawdzania haseł, który sprawdzał poprawność długości minimalnej. Istnieje nowy moduł sprawdzania poprawności haseł, który zapewnia większą kontrolę złożoności hasła. Należy pamiętać, że nawet jeśli włączysz wszystkie ustawienia w tym haśle, zachęcamy do włączenia uwierzytelniania dwuskładnikowego dla kont użytkowników.
- **IdentityFactory oprogramowanie pośredniczące/CreatePerOwinContext**:

    - **Menedżer użytkowników**: możesz użyć implementacji fabrycznej, aby uzyskać wystąpienie elementu usermanager z kontekstu Owin. Ten wzorzec jest podobny do tego, czego używamy do uzyskiwania elementu AuthenticationManager z kontekstu OWIN na potrzeby logowania i wylogowaniu. Jest to zalecany sposób uzyskiwania wystąpienia elementu Usermanager na żądanie dla aplikacji.
    - **DbContextFactory**: ASP.NET Identity używa Entity Framework do utrwalania systemu tożsamości w SQL Server. Aby to zrobić, system tożsamości ma odwołanie do ApplicationDbContext. Oprogramowanie pośredniczące DbContextFactory zwraca wystąpienie ApplicationDbContext na żądanie, którego można użyć w aplikacji.
- **Pakiet NuGet przykładów ASP.NET Identity**: pakiet NuGet przykładów może ułatwić instalowanie i uruchamianie przykładów dla ASP.NET Identity i korzystać z najlepszych rozwiązań. To jest przykładowa aplikacja ASP.NET MVC. Przed wdrożeniem w środowisku produkcyjnym należy zmodyfikować kod odpowiadający aplikacji. Przykład należy zainstalować w pustej aplikacji ASP.NET. Aby uzyskać więcej informacji na temat pakietu, przejdź do następującego wpisu w blogu: [ogłaszanie RTM ASP.NET Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx)

<a id="owin"></a>
### <a name="microsoft-owin-components"></a>Składniki Microsoft OWIN

Wystąpiło wiele błędów, które zostały rozwiązane w tej wersji. Aby uzyskać bardziej szczegółowe informacje, zobacz [Informacje o wersji dla wydania 2.1.0](https://katanaproject.codeplex.com/releases/view/113281) .

<a id="signalr"></a>
### <a name="aspnet-signalr-202"></a>ASP.NET SignalR 2.0.2

Wystąpiło wiele błędów, które zostały rozwiązane w tej wersji. Aby uzyskać bardziej szczegółowe informacje, zobacz [Informacje o wersji dla wydania 2.0.2](https://github.com/SignalR/SignalR/releases/tag/2.0.2) .

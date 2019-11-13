---
uid: whitepapers/mvc3-release-notes
title: ASP.NET MVC 3 | Microsoft Docs
author: rick-anderson
description: ''
ms.author: riande
ms.date: 10/06/2010
ms.assetid: f44c166e-7e91-48a0-a6f8-d9285f3594e5
msc.legacyurl: /whitepapers/mvc3-release-notes
msc.type: content
ms.openlocfilehash: 46d051a5eba6501cf36910b7674ce6400597de8a
ms.sourcegitcommit: 295cf898a4c87e264b0c35c7254b0fa4169f2278
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/13/2019
ms.locfileid: "74057015"
---
# <a name="aspnet-mvc-3"></a>ASP.NET MVC 3

- [Omówienie](#overview)
- [Uwagi dotyczące instalacji](#installation-notes)
- [Wymagania dotyczące oprogramowania](#software-requirements)
- [Dokumentacja](#documentation)
- [Pomocy](#support)
- [Uaktualnianie projektu ASP.NET MVC 2 do ASP.NET aktualizacji narzędzi MVC 3](#upgrading)
- [Aktualizacja narzędzi ASP.NET MVC 3 (12 kwietnia 2011)](#tu-changes)

    - [Okno dialogowe "Dodawanie kontrolera" umożliwia teraz tworzenie szkieletu kontrolerów z widokami i kodem dostępu do danych](#tu-AddControllerDialog)
    - [Ulepszenia okna dialogowego "ASP.NET MVC 3 New Project"](#tu-ImprovementsNewDialogBox)
    - [Szablony projektów zawierają teraz modernizację 1,7](#tu-Modernizr)
    - [Szablony projektu obejmują zaktualizowane wersje interfejsu użytkownika jQuery, jQuery i weryfikacji jQuery](#tu-UpdatedJQuery)
    - [Szablony projektów zawierają teraz ADO.NET Entity Framework 4,1 jako wstępnie zainstalowany pakiet NuGet](#tu-EF)
    - [Szablony projektu obejmują biblioteki JavaScript jako wstępnie zainstalowane pakiety NuGet](#tu-JavaScriptLibsNuget)
    - [Znane problemy](#tu-KI)
- [ASP.NET MVC 3 RTM (13 stycznia 2011)](#MVC3RTM)

    - [Zmiana: Zaktualizowano wersję interfejsu użytkownika jQuery do 1.8.7](#RTM-1)
    - [Zmiana: zmieniono domyślną ModelMetadataProvider z powrotem na DataAnnotationsModelMetadataProvider](#RTM-2)
    - [Naprawiono: wklejenie części wyrażenia Razor zawierającego odstępy powoduje ich odwrócenia](#RTM-3)
    - [Naprawiono: zmiana nazwy pliku Razor, który jest otwarty w edytorze wyłącza kolorowanie składni i IntelliSense](#RTM-4)
    - [Znane problemy](#RTM-KI)
    - [Istotne zmiany](#RTM-BC)
- [ASP.NET MVC 3 Release Candidate 2 (10 grudnia 2010)](#_Toc2)

    - [Szablony projektów zmienione w celu uwzględnienia jQuery 1.4.4, jQuery Validation 1,7 i jQuery UI 1.8.6 t interfejsu użytkownika 1.8.6](#_Toc2_1)
    - [Dodano klasę "AdditionalMetadataAttribute"](#_Toc2_2)
    - [Ulepszona funkcja tworzenia szkieletów widoków](#_Toc2_3)
    - [Dodano metodę html. Raw](#_Toc2_3)
    - [Zmieniono nazwę właściwości "Controller. ViewModel" i właściwość "View" na "ViewBag"](#_Toc2_4)
    - [Zmieniono nazwę klasy "ControllerSessionStateAttribute" na "SessionStateAttribute"](#_Toc2_5)
    - [Zmieniono nazwę właściwości Remoteattribute "Fields" na "AdditionalFields"](#_Toc2_6)
    - [Zmieniono nazwę "SkipRequestValidationAttribute" na "AllowHtmlAttribute"](#_Toc2_7)
    - [Zmieniono metodę "html. ValidationMessage", aby wyświetlić pierwszy przydatny komunikat o błędzie](#_Toc2_8)
    - [Stała deklaracja @model, aby nie dodawać odstępów do dokumentu](#_Toc2_9)
    - [Dodano Właściwość "FileExtensions", aby wyświetlić aparaty obsługujące nazwy plików specyficzne dla aparatu](#_Toc2_10)
    - [Naprawiono pomocnika "LabelFor", aby emitować poprawną wartość atrybutu "for".](#_Toc2_11)
    - [Rozwiązano metodę "RenderAction" w celu uzyskania pierwszeństwa jawnych wartości podczas wiązania modelu](#_Toc2_12)
    - [Istotne zmiany](#_Toc2_BC)
    - [Znane problemy](#_Toc2_KI)
- [ASP.NET MVC 3 Release Candidate (LIS 9, 2010)](#TOC_ASP_NET_3_RC)

    - [Nowe funkcje w ASP.NET MVC 3 RC](#_Toc276711785)
    - [Menedżer pakietów NuGet](#_Toc276711786)
    - [Ulepszone okno dialogowe "nowy projekt"](#_Toc276711787)
    - [Kontrolery Bezsesyjne](#_Toc276711788)
    - [Nowe atrybuty walidacji](#_Toc276711789)
    - [Nowe przeciążenia metod "LabelFor" i "LabelForModel"](#_Toc276711790)
    - [Buforowanie danych wyjściowych akcji podrzędnej](#_Toc276711791)
    - [Ulepszenia okna dialogowego Dodawanie widoku](#_Toc276711792)
    - [Szczegółowa Weryfikacja żądania](#_Toc276711793)
    - [Istotne zmiany](#_Toc276711794)
    - [Znane problemy](#_Toc276711795)
- [ASP.NET. Uwagi MVC 3 beta (2010 października)](#TOC_ASP_NET_3_Beta)

    - [Nowe funkcje w programie ASP.NET MVC 3 beta](#0.1__Toc274034215)
    - [Menedżer pakietów NuPack](#0.1__Toc274034216)
    - [Ulepszone okno dialogowe Nowy projekt](#0.1__Toc274034217)
    - [Uproszczony sposób określania modeli silnie wpisanych w widokach Razor](#0.1__Toc274034218)
    - [Obsługa nowych metod pomocnika stron sieci Web ASP.NET](#0.1__Toc274034219)
    - [Dodatkowa obsługa iniekcji zależności](#0.1__Toc274034220)
    - [Nowe wsparcie dla niezauważalnego opartego na technologii jQuery](#0.1__Toc274034221)
    - [Nowe wsparcie dla niedyskretnej weryfikacji jQuery](#0.1__Toc274034222)
    - [Nowe flagi dla całej aplikacji na potrzeby sprawdzania poprawności klienta i niedyskretnego języka JavaScript](#0.1__Toc274034223)
    - [Nowa obsługa kodu, który jest uruchamiany przed uruchomieniem widoków](#0.1__Toc274034224)
    - [Nowa Obsługa składni języka Razor języka VBHTML](#0.1__Toc274034225)
    - [Większa kontrola nad ValidateInputAttribute](#0.1__Toc274034226)
    - [Pomocnicy konwertują znaki podkreślenia na łączniki dla nazw atrybutów HTML określonych przy użyciu obiektów anonimowych](#0.1__Toc274034227)
    - [Poprawki błędów](#0.1__Toc274034228)
    - [Istotne zmiany](#0.1__Toc274034229)
    - [Znane problemy](#0.1__Toc274034230)
- [Zastrzeżenie](#0.1__Toc274034231)

<a id="overview"></a>
## <a name="overview"></a>Omówienie

W tym dokumencie opisano wydanie ASP.NET MVC 3 RTM dla programu Visual Studio 2010. ASP.NET MVC to struktura służąca do opracowywania aplikacji sieci Web, które używają wzorca Model-View-Controller (MVC). Instalator ASP.NET MVC 3 zawiera następujące składniki:

- Składniki środowiska uruchomieniowego ASP.NET MVC 3
- Narzędzia ASP.NET MVC 3 Visual Studio 2010
- Składniki czasu wykonywania stron sieci Web ASP.NET
- Narzędzia ASP.NET Web Pages programu Visual Studio 2010
- Microsoft Package Manager dla platformy .NET (NuGet)
- Aktualizacja programu Visual Studio 2010, która umożliwia obsługę składnia Razor. (Aby uzyskać szczegółowe informacje, zobacz artykuł w bazie wiedzy 2483190).

Pełny zestaw informacji o wersji dla każdej wersji wstępnej programu ASP.NET MVC 3 można znaleźć w witrynie sieci Web ASP.NET pod następującym adresem URL:

https://www.asp.net/learn/whitepapers/mvc3-release-notes

<a id="installation-notes"></a>
## <a name="installation-notes"></a>Uwagi dotyczące instalacji

Aby zainstalować ASP.NET MVC 3 RTM przy użyciu Instalatora platformy sieci Web (Web PI), odwiedź następującą stronę:

[https://www.microsoft.com/web/gallery/install.aspx?appid=MVC3](https://www.microsoft.com/web/gallery/install.aspx?appid=MVC3)

Alternatywnie można pobrać Instalatora programu ASP.NET MVC 3 RTM for Visual Studio 2010 z następującej strony:

https://go.microsoft.com/fwlink/?LinkID=208140

ASP.NET MVC 3 można zainstalować i uruchomić go równolegle z ASP.NET MVC 2.

<a id="software-requirements"></a>
## <a name="software-requirements"></a>Wymagania programowe

Składniki uruchomieniowe ASP.NET MVC 3 wymagają następującego oprogramowania:

- .NET Framework w wersji 4. 

    Narzędzia ASP.NET MVC 3 Visual Studio 2010 wymagają następującego oprogramowania:
- Visual Studio 2010 lub Visual Web Developer 2010 Express.

<a id="documentation"></a>
## <a name="documentation"></a>Dokumentacja

Dokumentacja usługi ASP.NET MVC jest dostępna w witrynie MSDN w sieci Web pod następującym adresem URL:

[https://go.microsoft.com/fwlink/?LinkId=205717](https://go.microsoft.com/fwlink/?LinkId=205717)

Samouczki i inne informacje dotyczące ASP.NET MVC są dostępne na stronie MVC witryny sieci Web ASP.NET pod następującym adresem URL:

[https://www.asp.net/mvc/](../mvc/index.md)

<a id="support"></a>
## <a name="support"></a>Obsługa

Jest to w pełni obsługiwana wersja. Informacje o uzyskiwaniu pomocy technicznej można znaleźć w [witrynie sieci web pomoc techniczna firmy Microsoft](https://support.microsoft.com/).

Możesz również bezpłatnie publikować pytania dotyczące tej wersji na forum ASP.NET MVC, gdzie członkowie społeczności ASP.NET są często w stanie zapewnić nieformalne wsparcie:

[https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)

<a id="upgrading"></a>
## <a name="upgrading-an-aspnet-mvc-2-project-to-aspnet-mvc-3-tools-update"></a>Uaktualnianie projektu ASP.NET MVC 2 do ASP.NET aktualizacji narzędzi MVC 3

ASP.NET MVC 3 można zainstalować równolegle z ASP.NET MVC 2 na tym samym komputerze, co zapewnia elastyczność w wyborze, kiedy uaktualnić aplikację ASP.NET MVC 2 do ASP.NET MVC 3.

Aby ręcznie uaktualnić istniejącą aplikację ASP.NET MVC 2 do wersji 3, wykonaj następujące czynności:

1. Utwórz nowy, pusty projekt ASP.NET MVC 3 na komputerze. Ten projekt będzie zawierać niektóre pliki, które są wymagane do uaktualnienia.
2. Skopiuj następujące pliki z projektu ASP.NET MVC 3 do odpowiedniej lokalizacji projektu ASP.NET MVC 2. Musisz zaktualizować wszystkie odwołania do biblioteki jQuery, aby uwzględnić nową nazwę pliku (jQuery-1.5.1. js): 

    - /Views/Web.config
    - /packages.config
    - /scripts/\*. js
    - /Content/themes/\*.\*
3. Skopiuj folder *Packages* z katalogu głównego pustego rozwiązania projektu ASP.NET MVC 3 do katalogu głównego rozwiązania, który znajduje się w katalogu, w którym znajduje się plik. sln rozwiązania.
4. Jeśli projekt ASP.NET MVC 2 zawiera jakieś obszary, skopiuj plik/Views/Web.config do folderu *widoki* każdego obszaru.
5. W obu plikach Web. config w projekcie ASP.NET MVC 2 globalne wyszukiwanie i zastępowanie wersji ASP.NET MVC. Znajdź następujące elementy: 

    [!code-console[Main](mvc3-release-notes/samples/sample1.cmd)]

    Zastąp go następującym:

    [!code-console[Main](mvc3-release-notes/samples/sample2.cmd)]
6. W Eksplorator rozwiązań usuń odwołanie do elementu *System. Web. MVC* (które wskazuje bibliotekę DLL w wersji 2), a następnie Dodaj odwołanie do elementu *System. Web. MVC* (v 3.0.0.0).
7. Dodaj odwołanie do elementu System. Web. Webpages. dll i system. Web. helps. dll. Te zestawy znajdują się w następujących folderach: 

    - % ProgramFiles% \ Microsoft ASP. NET\ASP.NET MVC 3 \ zestawy
    - % ProgramFiles% \ Microsoft ASP. NET\ASP.NET Web Pages\v1.0\Assemblies
8. W Eksplorator rozwiązań kliknij prawym przyciskiem myszy nazwę projektu i wybierz polecenie Zwolnij projekt. Następnie ponownie kliknij prawym przyciskiem myszy nazwę projektu i wybierz polecenie Edytuj *ProjectName*. csproj.
9. Znajdź element *ProjectTypeGuids* i Zastąp ciąg {F85E285D-A4E0-4152-9332-AB1D724D3325} atrybutem {E53F8FEA-EAE0-44A6-8774-FFD645390401}.
10. Zapisz zmiany, kliknij prawym przyciskiem myszy projekt, a następnie wybierz polecenie Załaduj ponownie projekt.
11. W głównym pliku Web. config aplikacji Dodaj następujące ustawienia do sekcji *zestawy* . 

    [!code-xml[Main](mvc3-release-notes/samples/sample3.xml)]
12. Jeśli projekt odwołuje się do wszystkich bibliotek innych firm, które są kompilowane przy użyciu ASP.NET MVC 2, Dodaj następujący wyróżniony element *bindingRedirect* do pliku Web. config w katalogu głównym aplikacji w sekcji *Konfiguracja* : 

    [!code-xml[Main](mvc3-release-notes/samples/sample4.xml)]

<a id="tu-changes"></a>
## <a name="changes-in-aspnet-mvc-3-tools-update"></a>Zmiany w narzędziu ASP.NET MVC 3 Updates

Ta sekcja zawiera opis zmian wprowadzonych w wersji aktualizacji narzędzi ASP.NET MVC 3 od wersji ASP.NET MVC 3 RTM.

<a id="tu-AddControllerDialog"></a>
### <a name="add-controller-dialog-box-can-now-scaffold-controllers-with-views-and-data-access-code"></a>Okno dialogowe "Dodawanie kontrolera" umożliwia teraz tworzenie szkieletu kontrolerów z widokami i kodem dostępu do danych

Tworzenie szkieletów jest sposobem na szybkie generowanie kontrolera i widoków dla aplikacji. Po wygenerowaniu kodu można go edytować, aby spełniał wymagania projektu.

Aby uruchomić okno dialogowe *Dodawanie kontrolera* w ASP.NET MVC 3, kliknij prawym przyciskiem myszy folder *Kontrolery* w *Eksplorator rozwiązań*, kliknij przycisk *Dodaj*, a następnie kliknij przycisk *kontroler*. Okno dialogowe zostało udoskonalone w celu zaoferowania dodatkowych opcji tworzenia szkieletów.

![](mvc3-release-notes/_static/image1.png)

Domyślnie dostępne są trzy szablony tworzenia szkieletów.

#### <a name="empty-controller"></a>Pusty kontroler

Ten szablon generuje pusty plik kontrolera. Ten szablon jest równoznaczny z niesprawdzaniem *dodawania akcji do tworzenia, edytowania, szczegółów i usuwania scenariuszy* we wcześniejszych wersjach ASP.NET MVC. W przypadku wybrania tej opcji nie będą dostępne żadne dalsze opcje.

#### <a name="controller-with-empty-readwrite-actions"></a>Kontroler z pustymi akcjami odczytu/zapisu

Ten szablon generuje plik kontrolera, który ma wszystkie wymagane metody akcji, ale nie kod implementacji w metodach. Ten szablon jest równoważny do sprawdzania *dodawania akcji do tworzenia, edytowania, szczegółów i usuwania scenariuszy* w poprzednich wersjach ASP.NET MVC. W przypadku wybrania tej opcji nie będą dostępne żadne dalsze opcje.

#### <a name="controller-with-readwrite-actions-and-views-using-entity-framework"></a>Kontroler z akcjami odczytu/zapisu i widokami korzystającymi z Entity Framework

Ten szablon umożliwia szybkie tworzenie działającego interfejsu użytkownika do wprowadzania danych. Generuje on kod, który obsługuje szereg typowych wymagań i scenariuszy, takich jak następujące:

- *Dostęp do danych*. Wygenerowany kod odczytuje i zapisuje jednostki w bazie danych. Działa z podejściem Entity Framework Code First w przypadku wybrania istniejącej klasy kontekstu danych lub Jeśli zezwolisz szablonowi na wygenerowanie nowej klasy *DbContext* . Działa ona również z Database First Entity Framework lub Model First, jeśli wybierzesz istniejącą klasę *ObjectContext* .
- *Sprawdzanie poprawności*. Wygenerowany kod używa funkcji powiązania i metadanych modelu MVC ASP.NET, dzięki czemu przesłania formularzy są weryfikowane zgodnie z regułami zadeklarowanymi w klasie modelu. Obejmuje to wbudowane reguły sprawdzania poprawności, takie jak atrybuty *wymagane* i *StringLength* oraz niestandardowe reguły walidacji.
- *Relacje jeden do wielu*. W przypadku zdefiniowania relacji jeden-do-wielu kluczy obcych między klasami modelu wygenerowany kod spowoduje utworzenie list rozwijanych w celu wybrania powiązanych jednostek. Na przykład można zdefiniować następujące klasy modelu, które są następujące Entity Framework Konwencji Code First: 

    [!code-csharp[Main](mvc3-release-notes/samples/sample5.cs)]

    Po utworzeniu szkieletu kontrolera klasy *produktu* jego widoki umożliwią użytkownikom wybranie obiektu *kategorii* dla każdego wystąpienia *produktu* .

    Ten szablon umożliwia korzystanie z dodatkowych opcji w oknie dialogowym *Dodawanie kontrolera* . Dla *klasy model*można wybrać dowolną klasę modelu w rozwiązaniu, która określa typ danych, które użytkownicy będą mogli tworzyć lub edytować:
- Jeśli chcesz użyć Code First Entity Framework, możesz wybrać dowolną klasę modelu.
- Jeśli używasz Database First Entity Framework lub Entity Framework Model First, pamiętaj o wybraniu klasy Entity zdefiniowanej w modelu koncepcyjnym.

W przypadku *klasy kontekstu danych*można wybrać następujące opcje:

- Jeśli chcesz użyć Code First i nie mieć istniejącej klasy kontekstu danych, wybierz * * nowy kontekst danych * *. Następnie zostanie wygenerowana Klasa kontekstu danych.
- Jeśli chcesz używać Code First i mieć istniejącą klasę kontekstu danych, wybierz ją tutaj. Zostanie ona zaktualizowana w celu utrwalenia wybranej klasy modelu.
- Jeśli używasz Database First lub Model First, w tym miejscu wybierz klasę kontekstu obiektu.

W obszarze widoki wybierz aparat widoku, którego chcesz użyć, lub wybierz opcję Brak, jeśli nie chcesz utworzyć szkieletu żadnych widoków.

Możesz wybrać pozycję Zaawansowane Optionsto Określ dalsze opcje dla wygenerowanych widoków. Na przykład możesz wybrać układ lub stronę wzorcową, która ma być używana.

<a id="tu-ImprovementsNewDialogBox"></a>
### <a name="improvements-to-the-aspnet-mvc-3-new-project-dialog-box"></a>Ulepszenia okna dialogowego "ASP.NET MVC 3 New Project"

Okno dialogowe, które służy do tworzenia nowych projektów ASP.NET MVC 3, zawiera wiele ulepszeń, jak wymieniono poniżej.

![](mvc3-release-notes/_static/image2.png)

#### <a name="new-intranet-project-template"></a>Nowy szablon "intranet projektu"

Lista szablonów projektu zawiera nowy szablon aplikacji intranetowej. Ten szablon zawiera ustawienia służące do kompilowania aplikacji sieci Web przy użyciu uwierzytelniania systemu Windows zamiast uwierzytelniania opartego na formularzach. Ponieważ aplikacja intranetowa wymaga niektórych ustawień usług IIS, których nie można hermetyzować w szablonie projektu, szablon zawiera plik Readme z instrukcjami dotyczącymi sposobu, w jaki szablon projektu działa w usługach IIS. Dokumentacja nowego szablonu aplikacji intranetowych jest dostępna w witrynie MSDN pod następującym adresem URL:

[https://msdn.microsoft.com/library/gg703322(VS.98).aspx](https://msdn.microsoft.com/library/gg703322(VS.98).aspx)

#### <a name="project-templates-are-now-html5-enabled"></a>Szablony projektów są teraz obsługiwane w języku HTML5

Okno dialogowe Nowy projekt zawiera teraz opcję dodawania funkcji specyficznych dla języka HTML5 do szablonów projektu. Wybranie opcji powoduje wygenerowanie widoków, które zawierają nowe elementy `<header>`HTML5, `<footer>`i `<navigation>`.

Należy pamiętać, że wcześniejsze wersje przeglądarek nie obsługują tagów specyficznych dla języka HTML5. Aby rozwiązać ten limit, szablony projektów HTML5 zawierają odwołanie do biblioteki programu modernizacji. (Zobacz następną sekcję).

<a id="tu-Modernizr"></a>
### <a name="project-templates-now-include-modernizr-17"></a>Szablony projektów zawierają teraz modernizację 1,7

Modernizacja to biblioteka języka JavaScript, która umożliwia obsługę stylów CSS 3 i HTML5 w przeglądarkach, które jeszcze nie obsługują tych funkcji. Ta biblioteka jest dołączana jako wstępnie zainstalowany pakiet NuGet w szablonach dla projektów ASP.NET MVC 3. Aby uzyskać więcej informacji na temat programu unowocześnienie, zobacz [http://www.modernizr.com/](http://www.modernizr.com/).

<a id="tu-UpdatedJQuery"></a>
### <a name="project-templates-include-updated-versions-of-jquery-jquery-ui-and-jquery-validation"></a>Szablony projektu obejmują zaktualizowane wersje interfejsu użytkownika jQuery, jQuery i weryfikacji jQuery

Szablony projektu zawierają teraz następujące wersje skryptów jQuery:

- jQuery 1.5.1
- Weryfikacja jQuery 1,8
- Interfejs użytkownika jQuery 1.8.11

Te biblioteki są dołączone jako wstępnie zainstalowane pakiety NuGet.

<a id="tu-EF"></a>
### <a name="project-templates-now-include-adonet-entity-framework-41-as-a-pre-installed-nuget-package"></a>Szablony projektów zawierają teraz ADO.NET Entity Framework 4,1 jako wstępnie zainstalowany pakiet NuGet

ADO.NET Entity Framework 4,1 zawiera funkcję Code First. Code First to nowy wzorzec programistyczny dla Entity Framework ADO.NET, który zapewnia alternatywę dla istniejących wzorców Database First i Model First.

Code First koncentruje się na definiowaniu modelu przy użyciu klas POCO ("zwykłych starych obiektów CLR") zapisanych C#w Visual Basic lub. Klasy te można następnie mapować do istniejącej bazy danych lub użyć do wygenerowania schematu bazy danych. Dodatkową konfigurację można dostarczyć przy użyciu atrybutów *DataAnnotations* lub interfejsów API Fluent.

Dokumentacja dotycząca używania kodu Firstwith ASP.NET MVC jest dostępna w witrynie sieci Web ASP.NET pod następującymi adresami URL:

[https://www.asp.net/mvc/tutorials/getting-started-with-mvc3-part1-cs](../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) [https://www.asp.net/entity-framework/tutorials/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)

<a id="tu-JavaScriptLibsNuget"></a>
### <a name="project-templates-include-javascript-libraries-as-pre-installed-nuget-packages"></a>Szablony projektu obejmują biblioteki JavaScript jako wstępnie zainstalowane pakiety NuGet

Podczas tworzenia nowego projektu ASP.NET MVC 3, projekt zawiera wymienione wcześniej pliki języka JavaScript (na przykład biblioteki programu modernizacji) przez zainstalowanie ich przy użyciu narzędzia NuGet zamiast bezpośredniego dodawania skryptów do folderu Scripts w szablonie projektu Contents. Dzięki temu można używać narzędzia NuGet do aktualizowania skryptów do najnowszej wersji, gdy zostaną wydane nowe wersje skryptów.

Na przykład, uwzględniając częstotliwość nowych wydań jQuery, wersja jQuery dołączona do szablonu projektu będzie nieaktualna. Jednak ponieważ jQuery jest dołączony jako zainstalowany pakiet NuGet, w oknie dialogowym NuGet zostanie wyświetlone powiadomienie, gdy dostępne są nowsze wersje platformy jQuery.

Ponieważ jQuery zawiera numer wersji w nazwie pliku, aktualizacja jQuery do najnowszej wersji wymaga także zaktualizowania znacznika `<script>`, który odwołuje się do pliku jQuery, aby użyć nowej nazwy pliku. Inne dołączone biblioteki skryptów nie zawierają numeru wersji w nazwie skryptu, dzięki czemu można je łatwiej aktualizować do najnowszych wersji.

<a id="tu-KI"></a>
## <a name="known-issues"></a>Znane problemy

- W niektórych przypadkach instalacja może zakończyć się niepowodzeniem z komunikatem o błędzie "instalacja nie powiodła się. kod błędu (0x80070643)". Aby uzyskać informacje o tym, jak obejść ten problem, zobacz artykuł w bazie [wiedzy 2531566](https://support.microsoft.com/kb/2531566).
- Tworzenie szkieletu w celu dodania kontrolera nie tworzy szkieletu jednostek, które wykorzystują obsługę dziedziczenia jednostki w ramach Entity Framework. Na przykład dana Klasa *osoby* podstawowej, która jest dziedziczona przez klasę *ucznia* , tworzenie szkieletu klasy *uczniów* spowoduje wygenerowanie kodu, który nie kompiluje się.
- Utworzenie nowego projektu ASP.NET MVC 3 w folderze rozwiązania spowoduje wystąpienie błędu *NullReferenceException* . Obejście polega na utworzeniu projektu ASP.NET MVC 3 w katalogu głównym rozwiązania, a następnie przeniesieniu go do folderu rozwiązania.
- Funkcja IntelliSense dla składnia Razor nie działa, gdy jest zainstalowany program Resharper. Jeśli zainstalowano program do desharpnia i chcesz skorzystać z obsługi technologii Razor IntelliSense w ASP.NET MVC 3, zobacz wpis [Razor IntelliSense i Resharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) na blogu Hadi Hariri, który omawia sposoby używania ich razem.
- Podczas instalacji okno dialogowe akceptacji umowy EULA wyświetla postanowienia licencyjne w oknie, które jest mniejsze niż zamierzone.
- Podczas edytowania widoku Razor (. cshtml lub. *plik VBHTML* ), widoki. ASP.NET MVC 3 nie zawiera żadnych fragmentów kodu dla widoków Razor. aspxselecting fragment kodu dla ASP.NET MVC wyświetli fragmenty dla
- W przypadku instalowania ASP.NET MVC 3 dla programu Visual Web Developer Express na komputerze, na którym nie jest zainstalowany program Visual Studio, a następnie instalowania programu Visual Studio, należy ponownie zainstalować ASP.NET MVC 3. Programy Visual Studio i Visual Web Developer Express Share Components, które są uaktualniane przez Instalatora ASP.NET MVC 3. Ten sam problem występuje, jeśli zainstalujesz ASP.NET MVC 3 dla programu Visual Studio na komputerze, na którym nie jest zainstalowany program Visual Web Developer Express, a następnie zainstalujesz program Visual Web Developer Express.

<a id="MVC3RTM"></a>
## <a name="changes-in-aspnet-mvc-3-rtm"></a>Zmiany w ASP.NET MVC 3 RTM

W tej sekcji opisano zmiany i poprawki błędów wprowadzone w wersji ASP.NET MVC 3 RTM od momentu opublikowania wersji RC2.

<a id="RTM-1"></a>
### <a name="change-updated-the-version-of-jquery-ui-to-187"></a>Zmiana: Zaktualizowano wersję interfejsu użytkownika jQuery do 1.8.7

Szablony projektów ASP.NET MVC dla programu Visual Studio zostały zaktualizowane w celu uwzględnienia najnowszej wersji biblioteki interfejsu użytkownika jQuery. Szablony obejmują również minimalny zestaw plików zasobów wymaganych przez interfejs użytkownika jQuery, taki jak skojarzone z nimi pliki CSS i Image.

<a id="RTM-2"></a>
### <a name="change-changed-the-default-modelmetadataprovider-back-to-dataannotationsmodelmetadataprovider"></a>Zmiana: zmieniono domyślną ModelMetadataProvider z powrotem na DataAnnotationsModelMetadataProvider

W wersji RC2 ASP.NET MVC 3 wprowadzono klasę *CachedDataAnnotationsMetadataProvider* , która zapewnia buforowanie na podstawie istniejącej klasy *DataAnnotationsModelMetadataProvider* w miarę poprawy wydajności. Jednak niektóre usterki zostały zgłoszone z tą implementacją, więc zmiana została wycofana i przeniesiona do projektu Futures MVC, który jest dostępny w [ASP.NET webstack](https://github.com/aspnet/AspNetWebStack).

<a id="RTM-3"></a>
### <a name="fixed-pasting-part-of-a-razor-expression-that-contains-whitespace-results-in-it-being-reversed"></a>Naprawiono: wklejenie części wyrażenia Razor zawierającego odstępy powoduje ich odwrócenia

W wersjach wstępnych programu ASP.NET MVC 3, gdy wklejasz część wyrażenia Razor, które zawiera odstępy w pliku Razor, wyrażenie wyniku zostanie cofnięte. Rozważmy na przykład następujący blok kodu Razor:

[!code-cshtml[Main](mvc3-release-notes/samples/sample6.cshtml)]

Jeśli wybierzesz tekst "pierwszy param" w pierwszej metodzie i wkleisz go jako argument do drugiej metody, wynik jest następujący:

[!code-cshtml[Main](mvc3-release-notes/samples/sample7.cshtml)]

Poprawne zachowanie polega na tym, że operacja wklejania powinna powodować następujące czynności:

[!code-cshtml[Main](mvc3-release-notes/samples/sample8.cshtml)]

Ten problem został rozwiązany w wersji RTM, dzięki czemu wyrażenie jest prawidłowo zachowywane podczas operacji wklejania.

<a id="RTM-4"></a>
### <a name="fixed-renaming-a-razor-file-that-is-opened-in-the-editor-disables-syntax-colorization-and-intellisense"></a>Naprawiono: zmiana nazwy pliku Razor, który jest otwarty w edytorze wyłącza kolorowanie składni i IntelliSense

Zmiana nazwy pliku Razor przy użyciu Eksplorator rozwiązań, gdy plik zostanie otwarty w oknie edytora powoduje wyróżnianie składni i użycie funkcji IntelliSense, aby przestać działać dla tego pliku. Ten problem został rozwiązany w taki sposób, aby wyróżnianie i IntelliSense były utrzymywane po zmianie nazwy.

<a id="RTM-KI"></a>
## <a name="known-issues"></a>Znane problemy

- Jeśli zamkniesz program Visual Studio 2010 SP1 Beta, gdy konsola Menedżera pakietów NuGet jest otwarta, program Visual Studio ulegnie awarii i podejmie próbę ponownego uruchomienia. Ten problem zostanie rozwiązany w wersji RTM programu Visual Studio 2010 z dodatkiem SP1.
- Instalator ASP.NET MVC 3 może tylko zainstalować początkową wersję Menedżera pakietów NuGet. Po zainstalowaniu początkowej wersji programu NuGet można zainstalować i zaktualizować za pomocą Menedżera rozszerzeń programu Visual Studio. Jeśli masz już zainstalowany pakiet NuGet, przejdź do galerii rozszerzeń programu Visual Studio, aby przeprowadzić aktualizację do najnowszej wersji programu NuGet.
- Utworzenie nowego projektu ASP.NET MVC 3 w folderze rozwiązania spowoduje wystąpienie błędu *NullReferenceException* . Obejście polega na utworzeniu projektu ASP.NET MVC 3 w katalogu głównym rozwiązania, a następnie przeniesieniu go do folderu rozwiązania.
- Instalator może zająć dużo dłużej niż wcześniejsze wersje ASP.NET MVC. Dzieje się tak, ponieważ aktualizuje składniki programu Visual Studio 2010.
- Funkcja IntelliSense dla składnia Razor nie działa, gdy jest zainstalowany program Resharper. Jeśli zainstalowano program do desharpnia i chcesz skorzystać z obsługi technologii Razor IntelliSense w ASP.NET MVC 3, zobacz wpis [Razor IntelliSense i Resharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) na blogu Hadi Hariri, który omawia sposoby używania ich razem.
- Widoki CCSHTML i VBHTML utworzone przy użyciu wersji beta ASP.NET MVC 3 nie mają ustawionej prawidłowej akcji kompilacji, z wynikiem pominięcia tych typów widoków podczas publikowania projektu. Wartość akcji kompilacji dla tych plików powinna być równa "Content" (zawartość). ASP.NET MVC 3 RTM rozwiązuje ten problem dla nowych plików, ale nie poprawia ustawienia dla istniejących plików dla projektu utworzonego z wersjami wstępnymi.
- ![](mvc3-release-notes/_static/image3.png)
- Podczas instalacji okno dialogowe akceptacji umowy EULA wyświetla postanowienia licencyjne w oknie, które jest mniejsze niż zamierzone.
- Podczas edytowania widoku Razor (plik. cshtml) element menu Przejdź do kontrolera w programie Visual Studio nie będzie dostępny i nie ma fragmentów kodu.
- W przypadku instalowania ASP.NET MVC 3 dla programu Visual Web Developer Express na komputerze, na którym nie jest zainstalowany program Visual Studio, a następnie instalowania programu Visual Studio, należy ponownie zainstalować ASP.NET MVC 3. Programy Visual Studio i Visual Web Developer Express Share Components, które są uaktualniane przez Instalatora ASP.NET MVC 3. Ten sam problem występuje, jeśli zainstalujesz ASP.NET MVC 3 dla programu Visual Studio na komputerze, na którym nie jest zainstalowany program Visual Web Developer Express, a następnie zainstalujesz program Visual Web Developer Express.

<a id="RTM-BC"></a>
## <a name="breaking-changes"></a>Fundamentalne zmiany

- W poprzednich wersjach ASP.NET MVC filtry akcji są tworzone na żądanie z wyjątkiem kilku przypadków. Takie zachowanie nigdy nie było gwarantowane, ale tylko szczegóły implementacji i kontrakt dla filtrów były traktowane jako bezstanowe. W ASP.NET MVC 3 filtry są buforowane bardziej agresywnie. W związku z tym wszystkie niestandardowe filtry akcji, które nieprawidłowo przechowują stan wystąpienia, mogą być uszkodzone.
- Kolejność wykonywania filtrów wyjątków została zmieniona dla filtrów wyjątków, które mają taką samą wartość *kolejności* . W ASP.NET MVC 2 i starszych, filtry wyjątków na kontrolerze, które mają taką samą wartość *kolejności* jak te w metodzie akcji są wykonywane przed filtrami wyjątków dla metody akcji. Zwykle jest to przypadek, gdy filtry wyjątków są stosowane bez określonej wartości *kolejności* . W ASP.NET MVC 3 Ta kolejność została odwrócona, tak aby najbardziej specyficzna procedura obsługi wyjątków była wykonywana w pierwszej kolejności. Tak jak w starszych wersjach, jeśli właściwość *Order* jest jawnie określona, filtry są uruchamiane w określonej kolejności.
- Do klasy podstawowej *VirtualPathProviderViewEngine* dodano nową właściwość o nazwie *FileExtensions* . Gdy ASP.NET wyszukuje widok według ścieżki (nie według nazwy), uwzględniane są tylko widoki z rozszerzeniem pliku znajdującym się na liście określonej przez tę nową właściwość. Jest to istotna zmiana w aplikacjach, w których zarejestrowano niestandardowego dostawcę kompilacji w celu włączenia niestandardowego rozszerzenia pliku dla widoków formularzy sieci Web i lokalizacji dostawcy odwołującego się do tych widoków przy użyciu pełnej ścieżki, a nie nazwy. Obejście polega na zmodyfikowaniu wartości właściwości *FileExtensions* w celu uwzględnienia niestandardowego rozszerzenia pliku.
- Implementacje fabryczne kontrolera niestandardowego, które bezpośrednio implementują interfejs *IControllerFactory* , muszą zapewniać implementację nowej metody *GetControllerSessionBehavior* , która została dodana do interfejsu w tej wersji. Ogólnie rzecz biorąc, zaleca się, aby nie wdrażać tego interfejsu bezpośrednio, a zamiast tego utworzyć klasę z *DefaultControllerFactory*.

<a id="_Toc2"></a>
## <a name="changes-in-aspnet-mvc-3-rc2"></a>Zmiany w ASP.NET MVC 3 RC2

W tej sekcji opisano zmiany (nowe funkcje i poprawki błędów) wprowadzone w wersji ASP.NET MVC 3 RC2 od momentu wydania wersji RC.

<a id="_Toc2_1"></a>
### <a name="project-templates-changed-to-include-jquery-144-jquery-validation-17-and-jquery-ui-186"></a>Szablony projektów zmienione w celu uwzględnienia jQuery 1.4.4, jQuery Validation 1,7 i jQuery UI 1.8.6

Szablony projektów dla ASP.NET MVC 3 zawierają teraz najnowsze wersje interfejsu użytkownika programu jQuery, narzędzia sprawdzania poprawności jQuery i jQuery. Interfejs użytkownika jQuery jest nowym uzupełnieniem szablonów projektu i udostępnia przydatne widżety interfejsu użytkownika. Aby uzyskać więcej informacji na temat interfejsu użytkownika jQuery, odwiedź stronę główną: [http://jqueryui.com/](http://jqueryui.com/).

<a id="_Toc2_2"></a>
### <a name="added-additionalmetadataattribute-class"></a>Dodano klasę "AdditionalMetadataAttribute"

Można użyć klasy *AdditionalMetadataAttribute* , aby wypełnić słownik *ModelMetadata. AdditionalValues* dla właściwości model.

Załóżmy na przykład, że model widoku ma właściwości, które powinny być wyświetlane tylko dla administratora. Ten model może mieć adnotację z nowym atrybutem przy użyciu AdminOnly jako klucza i prawdziwe jako wartość, jak w poniższym przykładzie:

[!code-csharp[Main](mvc3-release-notes/samples/sample9.cs)]

Te metadane są udostępniane w dowolnym szablonie wyświetlania lub edytora, gdy jest renderowany model widoku produktu. Jest to aplikacja Deweloper aplikacji, która interpretuje informacje o metadanych.

<a id="_Toc2_3"></a>
### <a name="improved-view-scaffolding"></a>Ulepszona funkcja tworzenia szkieletów widoków

Szablony T4 używane do tworzenia szkieletów w widokach teraz generują wywołania metod pomocników szablonu, takich jak *EditorFor* , zamiast pomocników, takich jak *TextBoxFor*. Ta zmiana usprawnia obsługę metadanych w modelu w postaci atrybutów adnotacji danych, gdy okno dialogowe Dodawanie widoku generuje widok.

Tworzenie szkieletu widoku Dodawanie obejmuje również ulepszone wykrywanie i użycie informacji o kluczu podstawowym w modelu na podstawie Konwencji. Na przykład okno dialogowe Dodawanie widoku używa tych informacji, aby upewnić się, że wartość klucza podstawowego nie jest szkieletowa jako pole formularza do edycji.

Domyślne edytowanie i tworzenie szablonów zawierają odwołania do skryptów jQuery wymaganych do weryfikacji klienta.

<a id="_Toc2_4"></a>
### <a name="added-htmlraw-method"></a>Dodano metodę html. Raw

Domyślnie kod HTML aparatu widoku Razor — koduje wszystkie wartości. Na przykład poniższy fragment kodu koduje kod HTML wewnątrz zmiennej Greeting, tak aby był wyświetlany na stronie jako `<strong>Hello World!</strong>`.

[!code-cshtml[Main](mvc3-release-notes/samples/sample10.cshtml)]

Nowa metoda *HTML. Raw* zapewnia prosty sposób wyświetlania niekodowanego kodu HTML, gdy zawartość jest znana jako bezpieczna. Poniższy przykład wyświetla ten sam ciąg, ale ciąg jest renderowany jako znacznik:

[!code-cshtml[Main](mvc3-release-notes/samples/sample11.cshtml)]

<a id="_Toc2_5"></a>
### <a name="renamed-controllerviewmodel-property-and-the-view-property-to-viewbag"></a>Zmieniono nazwę właściwości "Controller. ViewModel" i właściwość "View" na "ViewBag"

Wcześniej Właściwość *ViewModel* *kontrolera* odpowiada właściwości *View* widoku. Obie te właściwości umożliwiają dostęp do wartości obiektu *ViewDataDictionary* za pomocą składni dynamicznej metody dostępu do właściwości. Zmieniono nazwę obu właściwości na taki sam, aby uniknąć pomyłek i zapewnienia bardziej spójnej spójności.

<a id="_Toc2_6"></a>
### <a name="renamed-controllersessionstateattribute-class-to-sessionstateattribute"></a>Zmieniono nazwę klasy "ControllerSessionStateAttribute" na "SessionStateAttribute"

Klasa *ControllerSessionStateAttribute* została wprowadzona w wersji RC ASP.NET MVC 3. Nazwa właściwości została zmieniona na bardziej zwięzłe.

<a id="_Toc2_7"></a>
### <a name="renamed-remoteattribute-fields-property-to-additionalfields"></a>Zmieniono nazwę właściwości Remoteattribute "Fields" na "AdditionalFields"

Właściwość *pól* klasy *remoteattribute* spowodowała kilka pomyłek między użytkownikami. Zmiana nazwy tej właściwości na *AdditionalFields* objaśnia jej zamiar.

<a id="_Toc2_8"></a>
### <a name="renamed-skiprequestvalidationattribute-to-allowhtmlattribute"></a>Zmieniono nazwę "SkipRequestValidationAttribute" na "AllowHtmlAttribute"

Nazwa atrybutu *SkipRequestValidationAttribute* została zmieniona na *AllowHtmlAttribute* w celu lepszego reprezentowania zamierzonego użycia.

<a id="_Toc2_9"></a>
### <a name="changed-htmlvalidationmessage-method-to-display-the-first-useful-error-message"></a>Zmieniono metodę "html. ValidationMessage", aby wyświetlić pierwszy przydatny komunikat o błędzie

Metoda *HTML. ValidationMessage* została naprawiona, aby pokazać pierwszy użyteczny komunikat o błędzie zamiast prostego wyświetlania pierwszego błędu.

Podczas wiązania modelu słownik *ModelState* można wypełniać z wielu źródeł przy użyciu komunikatów o błędach, w tym od samego modelu (jeśli implementuje *IValidatableObject*), od atrybutów walidacji zastosowanych do właściwości oraz od wyjątków zgłaszanych podczas uzyskiwania dostępu do właściwości.

Gdy metoda *HTML. ValidationMessage* wyświetla komunikat weryfikacyjny, pomija wpisy stanu modelu, które obejmują wyjątek, ponieważ zazwyczaj nie są przeznaczone dla użytkownika końcowego. Zamiast tego metoda szuka pierwszego komunikatu weryfikacyjnego, który nie jest skojarzony z wyjątkiem i wyświetla ten komunikat. Jeśli taki komunikat nie zostanie znaleziony, domyślnie zostanie wyświetlony ogólny komunikat o błędzie skojarzony z pierwszym wyjątkiem.

<a id="_Toc2_10"></a>
### <a name="fixed-model-declaration-to-not-add-whitespace-to-the-document"></a>Stała deklaracja @model, aby nie dodawać odstępów do dokumentu

We wcześniejszych wersjach deklaracja `@model` w górnej części widoku dodała pusty wiersz do renderowanego wyjścia HTML. Ten problem został rozwiązany w taki sposób, że deklaracja nie wprowadza odstępu.

<a id="_Toc2_11"></a>
### <a name="added-fileextensions-property-to-view-engines-to-support-engine-specific-file-names"></a>Dodano Właściwość "FileExtensions", aby wyświetlić aparaty obsługujące nazwy plików specyficzne dla aparatu

Aparat widoku może zwrócić widok przy użyciu jawnej ścieżki widoku, jak w poniższym przykładzie:

[!code-csharp[Main](mvc3-release-notes/samples/sample12.cs)]

Pierwszy aparat widoku zawsze próbuje renderować widok. Domyślnie aparat widoku formularzy sieci Web jest pierwszym aparatem widoku; ponieważ aparat formularzy sieci Web nie może renderować widoku Razor, wystąpi błąd. Wyświetl aparaty mają teraz Właściwość *FileExtensions* , która jest używana do określania, które rozszerzenia plików obsługują. Ta właściwość jest sprawdzana, gdy ASP.NET określa, czy aparat widoku może renderować plik. Jest to istotna zmiana i więcej szczegółów znajduje się w sekcji istotne [zmiany](#_Toc2_BC) w tym dokumencie.

<a id="_Toc2_12"></a>
### <a name="fixed-labelfor-helper-to-emit-the-correct-value-for-the-for-attribute"></a>Naprawiono pomocnika "LabelFor", aby emitować poprawną wartość atrybutu "for".

Rozwiązano problem polegający na tym, że metoda *LabelFor* renderowana *dla* atrybutu, który pasuje do atrybutu *nazwy* elementu *wejściowego* zamiast jego identyfikatora. Zgodnie z W3C, atrybut *for* powinien być zgodny z identyfikatorem elementu *wejściowego* .

<a id="_Toc2_13"></a>
### <a name="fixed-renderaction-method-to-give-explicit-values-precedence-during-model-binding"></a>Rozwiązano metodę "RenderAction" w celu uzyskania pierwszeństwa jawnych wartości podczas wiązania modelu

We wcześniejszych wersjach jawne wartości, które zostały przesłane do metody *RenderAction* , zostały zignorowane na rzecz bieżących wartości formularza podczas wiązania modelu wewnątrz akcji podrzędnej. Poprawka gwarantuje, że jawne wartości mają pierwszeństwo podczas wiązania modelu.

<a id="_Toc2_BC"></a>
## <a name="breaking-changes"></a>Fundamentalne zmiany

- W poprzednich wersjach ASP.NET MVC filtry akcji zostały utworzone dla każdego żądania, z wyjątkiem kilku przypadków. Takie zachowanie nigdy nie było gwarantowane, ale tylko szczegóły implementacji i kontrakt dla filtrów były traktowane jako bezstanowe. W ASP.NET MVC 3 filtry są buforowane bardziej agresywnie. W związku z tym wszystkie niestandardowe filtry akcji, które nieprawidłowo przechowują stan wystąpienia, mogą być uszkodzone.
- Kolejność wykonywania filtrów wyjątków została zmieniona dla filtrów wyjątków, które mają taką samą wartość *kolejności* . W ASP.NET MVC 2 i starszych, filtry wyjątków na kontrolerze, który ma taką samą wartość *kolejności* jak te w metodzie akcji, zostały wykonane przed filtrami wyjątku w metodzie akcji. Zwykle zdarza się to w przypadku zastosowania filtrów wyjątków bez określonej wartości *kolejności* . W ASP.NET MVC 3 Ta kolejność została odwrócona, tak aby najbardziej specyficzna procedura obsługi wyjątków była wykonywana w pierwszej kolejności. Tak jak w starszych wersjach, jeśli właściwość *Order* jest jawnie określona, filtry są uruchamiane w określonej kolejności.
- Do klasy podstawowej *VirtualPathProviderViewEngine* dodano nową właściwość o nazwie *FileExtensions* . Gdy ASP.NET wyszukuje widok według ścieżki (nie według nazwy), uwzględniane są tylko widoki z rozszerzeniem pliku znajdującym się na liście określonej przez tę nową właściwość. Jest to istotna zmiana w aplikacjach, w których zarejestrowano niestandardowego dostawcę kompilacji w celu włączenia niestandardowego rozszerzenia pliku dla widoków formularzy sieci Web i lokalizacji dostawcy odwołującego się do tych widoków przy użyciu pełnej ścieżki, a nie nazwy. Obejście polega na zmodyfikowaniu wartości właściwości *FileExtensions* w celu uwzględnienia niestandardowego rozszerzenia pliku.
- Implementacje fabryczne kontrolera niestandardowego, które bezpośrednio implementują interfejs *IControllerFactory* , muszą zapewniać implementację nowej metody *GetControllerSessionBehavior* , która została dodana do interfejsu w tej wersji. Ogólnie rzecz biorąc, zaleca się, aby nie wdrażać tego interfejsu bezpośrednio, a zamiast tego utworzyć klasę z *DefaultControllerFactory*.

<a id="_Toc2_KI"></a>
## <a name="known-issues"></a>Znane problemy

- Instalator ASP.NET MVC 3 może tylko zainstalować początkową wersję Menedżera pakietów NuGet. Po zainstalowaniu początkowej wersji programu NuGet można zainstalować i zaktualizować za pomocą Menedżera rozszerzeń programu Visual Studio. Jeśli masz już zainstalowany pakiet NuGet, przejdź do galerii rozszerzeń programu Visual Studio, aby przeprowadzić aktualizację do najnowszej wersji programu NuGet.
- Utworzenie nowego projektu ASP.NET MVC 3 w folderze rozwiązania spowoduje wystąpienie błędu *NullReferenceException* . Obejście polega na utworzeniu projektu ASP.NET MVC 3 w katalogu głównym rozwiązania, a następnie przeniesieniu go do folderu rozwiązania.
- Instalator może zająć dużo dłużej niż wcześniejsze wersje ASP.NET MVC. Dzieje się tak, ponieważ aktualizuje składniki programu Visual Studio 2010.
- Funkcja IntelliSense dla składnia Razor nie działa, gdy jest zainstalowany program Resharper. Jeśli zainstalowano program do desharpnia i chcesz skorzystać z obsługi technologii Razor IntelliSense w ASP.NET MVC 3 RC2, zobacz wpis [Razor IntelliSense i Resharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) na blogu Hadi Hariri, który omawia sposoby używania ich razem.
- Widoki CSHTML i VBHTML utworzone przy użyciu wersji beta ASP.NET MVC 3 nie mają ustawionej prawidłowej akcji kompilacji, z wynikiem pominięcia tych typów widoków podczas publikowania projektu. Wartość *akcji kompilacji* dla tych plików powinna być równa "Content". ASP.NET MVC 3 RC2 rozwiązuje ten problem dla nowych plików, ale nie poprawia ustawienia dla istniejących plików dla projektu utworzonego w wersji beta.![](mvc3-release-notes/_static/image4.png)
- Podczas instalacji okno dialogowe akceptacji umowy EULA wyświetla postanowienia licencyjne w oknie, które jest mniejsze niż zamierzone.
- Podczas edytowania widoku Razor (plik. cshtml) element menu Przejdź do kontrolera w programie Visual Studio nie będzie dostępny i nie ma fragmentów kodu.
- W przypadku instalowania ASP.NET MVC 3 dla programu Visual Web Developer Express na komputerze, na którym nie jest zainstalowany program Visual Studio, a następnie instalowania programu Visual Studio, należy ponownie zainstalować ASP.NET MVC 3. Programy Visual Studio i Visual Web Developer Express Share Components, które są uaktualniane przez Instalatora ASP.NET MVC 3. Ten sam problem występuje, jeśli zainstalujesz ASP.NET MVC 3 dla programu Visual Studio na komputerze, na którym nie jest zainstalowany program Visual Web Developer Express, a następnie zainstalujesz program Visual Web Developer Express.
- Instalowanie ASP.NET MVC 3 RC 2 nie aktualizuje NuGet, jeśli jest już zainstalowany. Aby uaktualnić pakiet NuGet, przejdź do Menedżera rozszerzeń programu Visual Studio, który powinien zostać wyświetlony jako dostępna aktualizacja. W tym miejscu możesz uaktualnić pakiet NuGet do najnowszej wersji.

<a id="TOC_ASP_NET_3_RC"></a>
## <a name="aspnet-mvc-3-release-candidate"></a>ASP.NET MVC 3 Release Candidate

ASP.NET MVC Release Candidate został opublikowany w dniu 9 listopada 2010.

<a id="_Toc276711785"></a>
## <a name="new-features-in-aspnet-mvc-3-rc"></a>Nowe funkcje w ASP.NET MVC 3 RC

Ta sekcja zawiera opis funkcji wprowadzonych w wersji ASP.NET MVC 3 RC od momentu wydania beta.

<a id="_Toc276711786"></a>
### <a name="nuget-package-manager"></a>Menedżer pakietów NuGet

ASP.NET MVC 3 zawiera Menedżera pakietów NuGet (dawniej znany jako NuPack), czyli zintegrowanego narzędzia do zarządzania pakietami służącego do dodawania bibliotek i narzędzi do projektów programu Visual Studio. To narzędzie automatyzuje kroki, które deweloperzy wykorzystują dzisiaj, aby uzyskać bibliotekę w ich drzewie źródłowym.

Możesz współpracować z pakietem NuGet jako narzędziem wiersza polecenia, jako zintegrowane okno konsoli w programie Visual Studio 2010, z menu kontekstowego programu Visual Studio i jako zestaw poleceń cmdlet programu PowerShell.

Aby uzyskać więcej informacji na temat narzędzia NuGet, zapoznaj się z [dokumentacją programu NuGet](https://docs.microsoft.com/nuget/).

<a id="_Toc276711787"></a>
### <a name="improved-new-project-dialog-box"></a>Ulepszone okno dialogowe "nowy projekt"

Podczas tworzenia nowego projektu okna dialogowego Nowy projekt umożliwiają teraz określenie aparatu widoku oraz typu projektu ASP.NET MVC.

![](mvc3-release-notes/_static/image5.png)

W tej wersji dołączono obsługę modyfikowania listy szablonów i aparatów widoków wymienionych w oknie dialogowym.

Domyślne szablony są następujące:

Ciągiem. Zawiera minimalny zestaw plików dla projektu ASP.NET MVC, łącznie z domyślną strukturą katalogów dla projektów ASP.NET MVC, plik site. css zawierający domyślne style ASP.NET MVC oraz katalog skryptów zawierający domyślne pliki JavaScript.

Aplikacja internetowa. Zawiera przykładowe funkcje, które pokazują, jak używać dostawcy członkostwa z ASP.NET MVC.

Lista szablonów projektu, które są wyświetlane w oknie dialogowym, jest określona w rejestrze systemu Windows.

<a id="_Toc276711788"></a>
### <a name="sessionless-controllers"></a>Kontrolery Bezsesyjne

Nowy *ControllerSessionStateAttribute* daje większą kontrolę nad zachowaniem stanu sesji dla kontrolerów przez określenie wartości wyliczenia [System. Web. SessionState. SessionStateBehavior](https://msdn.microsoft.com/library/system.web.sessionstate.sessionstatebehavior.aspx) .

Poniższy przykład pokazuje, jak wyłączyć stan sesji dla wszystkich żądań do kontrolera.

[!code-csharp[Main](mvc3-release-notes/samples/sample13.cs)]

Poniższy przykład pokazuje, jak ustawić stan sesji tylko do odczytu dla wszystkich żądań do kontrolera.

[!code-csharp[Main](mvc3-release-notes/samples/sample14.cs)]

<a id="_Toc276711789"></a>
### <a name="new-validation-attributes"></a>Nowe atrybuty walidacji

#### <a name="compareattribute"></a>CompareAttribute

Nowy atrybut walidacji programu *CompareAttribute* umożliwia porównanie wartości dwóch różnych właściwości modelu. W poniższym przykładzie właściwość *ComparePassword* musi być zgodna z *hasłem* pola, aby było prawidłowe.

[!code-csharp[Main](mvc3-release-notes/samples/sample15.cs)]

#### <a name="remoteattribute"></a>Zdalnyattribute

Nowy atrybut *remoteattribute* Validation ma zalety zdalnego modułu sprawdzania poprawności wtyczki walidacji jQuery, który umożliwia walidację po stronie klienta wywoływanie metody na serwerze, który wykonuje rzeczywistą logikę walidacji.

W poniższym przykładzie właściwość *username* ma zastosowany atrybut *remoteattribute* . Podczas edytowania tej właściwości w widoku edycji Weryfikacja klienta wywoła akcję o nazwie *UserNameAvailable* w klasie *UsersController* w celu sprawdzenia poprawności tego pola.

[!code-csharp[Main](mvc3-release-notes/samples/sample16.cs)]

Poniższy przykład pokazuje odpowiedni kontroler.

[!code-csharp[Main](mvc3-release-notes/samples/sample17.cs)]

Domyślnie nazwa właściwości, do której zastosowano atrybut, jest wysyłana do metody Action jako parametr ciągu zapytania.

<a id="_Toc276711790"></a>
### <a name="new-overloads-for-labelfor-and-labelformodel-methods"></a>Nowe przeciążenia metod "LabelFor" i "LabelForModel"

Dodano nowe przeciążenia dla metod *LabelFor* i *LabelForModel* , które umożliwiają określenie tekstu etykiety. Poniższy przykład pokazuje, jak używać tych przeciążeń.

[!code-cshtml[Main](mvc3-release-notes/samples/sample18.cshtml)]

<a id="_Toc276711791"></a>
### <a name="child-action-output-caching"></a>Buforowanie danych wyjściowych akcji podrzędnej

*OutputCacheAttribute* obsługuje wyjściowe buforowanie akcji podrzędnych, które są wywoływane przy użyciu metod pomocnika *HTML. RenderAction* lub *HTML. Action* . Poniższy przykład przedstawia widok, który wywołuje inną akcję.

[!code-cshtml[Main](mvc3-release-notes/samples/sample19.cshtml)]

Akcja *GETDATE* ma adnotację z *OutputCacheAttribute*:

[!code-csharp[Main](mvc3-release-notes/samples/sample20.cs)]

Gdy ten kod jest uruchamiany, wynik wywołania do HTML. Action ("GetDate") jest buforowany przez 100 sekund.

<a id="_Toc276711792"></a>
### <a name="add-view-dialog-box-improvements"></a>Ulepszenia okna dialogowego Dodawanie widoku

Po dodaniu widoku o jednoznacznie określonym typie okno dialogowe Dodaj widok filtruje teraz więcej nieodpowiednich typów niż w poprzednich wersjach, takich jak wiele typów .NET Framework podstawowych. Ponadto lista jest teraz sortowana według nazwy klasy, a nie za pomocą w pełni kwalifikowanej nazwy typu, co ułatwia wyszukiwanie typów. Na przykład nazwa typu jest teraz wyświetlana jak w poniższym przykładzie:

ClassName (przestrzeń nazw)

W starszych wersjach będzie on wyświetlany w następujący sposób:

Przestrzeń nazw. ClassName

<a id="_Toc276711793"></a>
### <a name="granular-request-validation"></a>Szczegółowa Weryfikacja żądania

Właściwość *exclude* elementu *ValidateInputAttribute* już nie istnieje. Zamiast tego w celu pominięcia weryfikacji żądań dla określonych właściwości modelu podczas powiązania modelu należy użyć nowego *SkipRequestValidationAttribute*.

Na przykład załóżmy, że metoda akcji jest używana do edytowania wpisu w blogu:

[!code-csharp[Main](mvc3-release-notes/samples/sample21.cs)]

Poniższy przykład pokazuje model widoku dla wpisu w blogu.

[!code-csharp[Main](mvc3-release-notes/samples/sample22.cs)]

Gdy użytkownik przesyła jakiś znacznik właściwości Description, powiązanie modelu zakończy się niepowodzeniem z powodu walidacji żądania. Aby wyłączyć weryfikację żądań podczas powiązania modelu dla opisu wpisu w blogu, Zastosuj *SkipRequpestValidationAttribute* do właściwości, jak pokazano w tym przykładzie:.

[!code-csharp[Main](mvc3-release-notes/samples/sample23.cs)]

Alternatywnie, aby wyłączyć weryfikację żądań dla każdej właściwości modelu, Zastosuj *ValidateInputAttribute* z wartością *false* do metody akcji:

[!code-csharp[Main](mvc3-release-notes/samples/sample24.cs)]

<a id="_Toc276711794"></a>
## <a name="breaking-changes"></a>Fundamentalne zmiany

- Kolejność wykonywania filtrów wyjątków została zmieniona dla filtrów wyjątków, które mają taką samą wartość *kolejności* . W ASP.NET MVC 2 i starszych, filtry wyjątków na kontrolerze, które miały takie samo *Zamówienie* jak te na metodzie akcji, zostały wykonane przed filtrami wyjątku w metodzie akcji. Zwykle zdarza się to w przypadku zastosowania filtrów wyjątków bez określonej wartości *kolejności* . W ASP.NET MVC 3 Ta kolejność została odwrócona, tak aby najbardziej specyficzna procedura obsługi wyjątków była wykonywana w pierwszej kolejności. Tak jak w starszych wersjach, jeśli właściwość *Order* jest jawnie określona, filtry są uruchamiane w określonej kolejności.
- Dodano nową właściwość o nazwie *FileExtensions* do klasy podstawowej *VirtualPathProviderViewEngine* . Podczas wyszukiwania widoku według ścieżki (a nie według nazwy) uwzględniane są tylko widoki z rozszerzeniem pliku znajdującym się na liście określonej przez tę nową właściwość. Jest to istotna zmiana dla tych, którzy rejestrują niestandardowego dostawcę kompilacji w celu włączenia niestandardowego rozszerzenia pliku dla widoków formularzy sieci Web i odwołują się do tych widoków przy użyciu pełnej ścieżki, a nie nazwy. Obejście polega na zmodyfikowaniu wartości właściwości *FileExtensions* w celu uwzględnienia niestandardowego rozszerzenia pliku.

<a id="_Toc276711795"></a>
## <a name="known-issues"></a>Znane problemy

- Instalator może zająć dużo dłużej niż wcześniejsze wersje ASP.NET MVC, ponieważ aktualizuje składniki programu Visual Studio 2010.
- Dodawanie szkieletu widoku w przypadku wybrania właściwości astrongly widoku szkieletów. Te powinny być zawsze ignorowane przez tworzenie szkieletów. Okno dialogowe Dodawanie widoku tworzy również szkielet właściwości tylko do odczytu podczas generowania widoku "Edytuj" lub "Utwórz". Właściwości tylko do odczytu powinny być szkieletowe tylko dla widoków wyświetlania i listy.
- Debugowanie nie działa, gdy ASP.NET MVC 3 jest zainstalowany obok asynchronicznej CTP. ASP.NET MVC 3 nie można zainstalować równolegle z Async CTP. Odinstaluj asynchroniczne CTP, aby naprawić debugowanie. Aby uzyskać więcej informacji, Przeczytaj [ten wpis w blogu](http://drew-prog.blogspot.com/2010/11/how-to-uninstall-microsoft-aspnet-mvc-3.html) dotyczący odinstalowywania wszystkich fragmentów ASP.NET MVC 3 RC.
- Funkcja IntelliSense Razor nie działa, gdy zostanie zainstalowany program Resharper. Jeśli zainstalowano program w celu skorzystania z obsługi technologii Razor IntelliSense w ASP.NET MVC 3 RC, Przeczytaj [ten wpis w blogu](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) z usługi JetBrains, w którym omówiono sposoby używania ich razem.
- Widoki CSHTML i VBHTML utworzone przy użyciu wersji beta ASP.NET MVC 3 nie mają poprawnego działania kompilacji, co pomija ich publikowanie. *Akcja kompilacji* dla tych plików powinna mieć wartość "Content" (zawartość). ASP.NET MVC 3 RC rozwiązuje ten problem dla nowych plików, ale nie poprawia ustawienia dla istniejących plików dla projektu utworzonego w wersji beta.
- Instalator może zająć dużo dłużej niż wcześniejsze wersje ASP.NET MVC, ponieważ aktualizuje składniki programu Visual Studio 2010.
- Dodawanie szkieletu widoku w przypadku wybrania opcji "Edytuj", które są wyświetlane jako właściwości tylko do odczytu. Podobnie właściwości tylko do zapisu są szkieletowe dla widoków "Display".
- Podczas instalacji okno dialogowe akceptacji umowy EULA wyświetla postanowienia licencyjne w oknie, które jest mniejsze niż zamierzone.
- Zainstalowanie programu Visual Studio Async CTP powoduje konflikt z wersją Razor zawartą w ramach instalacji narzędziowej ASP.NET MVC 3. Upewnij się, że nie próbuj zainstalować zarówno programu Visual Studio Async CTP, jak i wydania Razor na tym samym komputerze.
- Podczas edytowania widoku Razor (plik. cshtml) element menu Przejdź do kontrolera w programie Visual Studio nie będzie dostępny i nie ma fragmentów kodu.

<a id="TOC_ASP_NET_3_Beta"></a>
## <a name="aspnet-mvc-3-beta"></a>ASP.NET MVC 3 beta

ASP.NET MVC 3 beta wydano 6 października 2010. Poniższe uwagi dotyczą wersji beta i podlegają wszelkim aktualizacjom lub zmianom przywoływanym w powyższej sekcji ASP.NET MVC 3 Release Candidate.

## <a id="0.1__Toc274034215"></a>Nowe funkcje w ASP.NET MVC 3 beta

<a id="0.1__Default_validation_system"></a>Ta sekcja zawiera opis funkcji wprowadzonych w wersji ASP.NET MVC 3 beta.

### <a id="0.1__Toc274034216"></a>Menedżer pakietów NuGet

ASP.NET MVC 3 zawiera Menedżera pakietów NuGet, czyli zintegrowanego narzędzia do zarządzania pakietami służącego do dodawania bibliotek i narzędzi do projektów programu Visual Studio. W większości, automatyzuje kroki, które deweloperzy wykorzystują dzisiaj, aby uzyskać bibliotekę w ich drzewie źródłowym.

Możesz współpracować z pakietem NuGet jako narzędziem wiersza polecenia, jako zintegrowane okno konsoli w programie Visual Studio 2010, z menu kontekstowego programu Visual Studio i jako zestaw poleceń cmdlet programu PowerShell.

Aby uzyskać więcej informacji na temat narzędzia NuGet, zapoznaj się z [dokumentacją programu NuGet](https://docs.microsoft.com/nuget/).

### <a id="0.1__Toc274034217"></a>Ulepszone okno dialogowe Nowy projekt

Podczas tworzenia nowego projektu okna dialogowego Nowy projekt umożliwiają teraz określenie aparatu widoku oraz typu projektu ASP.NET MVC.

![](mvc3-release-notes/_static/image6.png)

W tej wersji nie dołączono obsługi modyfikowania listy szablonów i aparatów widoków wymienionych w oknie dialogowym.

Domyślne szablony są następujące:

Ciągiem. Zawiera minimalny zestaw plików dla projektu ASP.NET MVC, łącznie z domyślną strukturą katalogów dla projektów ASP.NET MVC, małym pliku. CSS, który zawiera domyślne style ASP.NET MVC oraz katalog skryptów zawierający domyślne pliki JavaScript.

Aplikacja internetowa. Zawiera przykładowe funkcje, które pokazują, jak używać dostawcy członkostwa w ramach ASP.NET MVC.

### <a id="0.1__Toc274034218"></a>Uproszczony sposób określania modeli silnie wpisanych w widokach Razor

Sposób określenia typu modelu dla niejednoznacznie wpisanych widoków Razor został uproszczony przy użyciu nowej dyrektywy @model dla widoków CSHTML i @ModelType dyrektywie dla widoków VBHTML. We wcześniejszych wersjach ASP.NET MVC należy określić model o jednoznacznie określonym typie dla widoków Razor w następujący sposób:

[!code-cshtml[Main](mvc3-release-notes/samples/sample25.cshtml)]

W tej wersji można użyć następującej składni:

[!code-cshtml[Main](mvc3-release-notes/samples/sample26.cshtml)]

### <a id="0.1__Toc274034219"></a>Obsługa nowych metod pomocnika stron sieci Web ASP.NET

Nowe technologie ASP.NET Web Pages zawierają zestaw metod pomocniczych, które są przydatne do dodawania często używanych funkcji do widoków i kontrolerów. ASP.NET MVC 3 obsługuje używanie tych metod pomocnika w kontrolerach i widokach (w stosownych przypadkach). Te metody są zawarte w zestawie System. Web. Pomocnicys. Poniższa tabela zawiera kilka metod pomocnika stron sieci Web ASP.NET.

| **Pomagając** | **Opis** |
| --- | --- |
| Wykres | Renderuje wykres w widoku. Zawiera metody, takie jak Chart. ToWebImage, Chart. Save i Chart. Write. |
| Modułu | Używa algorytmów wyznaczania wartości skrótu do prawidłowego tworzenia haseł solonych i skrótów. |
| Siatka sieci | Renderuje kolekcję obiektów (zazwyczaj dane z bazy danych) jako siatkę. Obsługuje stronicowanie i sortowanie. |
| Obraz webimage | Renderuje obraz. |
| Poczty internetowej | Wysyła wiadomość e-mail. |

Krótki temat zawierający listę pomocników i składnię podstawową jest dostępny jako część dokumentacji ASP.NET składnia Razor pod następującym adresem URL:

[https://www.asp.net/webmatrix/tutorials/asp-net-web-pages-api-reference](../web-pages/overview/api-reference/asp-net-web-pages-api-reference.md)

### <a id="0.1__Toc274034220"></a>Dodatkowa obsługa iniekcji zależności

W przypadku tworzenia wersji ASP.NET MVC 3 wersja zapoznawcza 1 bieżąca wersja obejmuje dodatkową obsługę dwóch nowych usług i czterech istniejących usług oraz ulepszoną obsługę rozpoznawania zależności i wspólnego lokalizatora usługi.

#### <a name="new-icontrolleractivator-interface-for-fine-grained-controller-instantiation"></a>Nowy interfejs IControllerActivator dla tworzenia wystąpienia kontrolera szczegółowego

Nowy interfejs IControllerActivator zapewnia bardziej szczegółową kontrolę nad sposobem tworzenia wystąpień kontrolerów przy użyciu iniekcji zależności. Poniższy przykład pokazuje interfejs:

[!code-csharp[Main](mvc3-release-notes/samples/sample27.cs)]

W przeciwieństwie do roli fabryki kontrolerów. Fabryka kontrolerów jest implementacją interfejsu IControllerFactory, która jest odpowiedzialna za lokalizowanie typu kontrolera i tworzenie wystąpienia tego typu kontrolera.

Aktywatory kontrolera są odpowiedzialne za utworzenie wystąpienia typu kontrolera. Nie wykonują wyszukiwania typu kontrolera. Po znalezieniu właściwego typu kontrolera fabryki kontrolerów powinny delegować do wystąpienia IControllerActivator, aby obsłużyć rzeczywiste Tworzenie wystąpienia kontrolera.

Klasa DefaultControllerFactory ma nowy Konstruktor, który akceptuje wystąpienie IControllerFactory. Dzięki temu można zastosować iniekcję zależności w celu zarządzania tym aspektem tworzenia kontrolera bez konieczności przesłonięcia domyślnego zachowania wyszukiwania typu Controller.

#### <a name="iservicelocator-interface-replaced-with-idependencyresolver"></a>Interfejs IServiceLocator został zastąpiony IDependencyResolver

W oparciu o opinie społecznościowe wersja ASP.NET MVC 3 beta zastąpiła korzystanie z interfejsu IServiceLocator z interfejsem slimmed-Down IDependencyResolver specyficznym dla potrzeb ASP.NET MVC. Poniższy przykład pokazuje nowy interfejs:

[!code-csharp[Main](mvc3-release-notes/samples/sample28.cs)]

W ramach tej zmiany Klasa ServiceLocator została również zastąpiona klasą DependencyResolver. Rejestracja programu rozpoznawania zależności jest podobna do wcześniejszych wersji ASP.NET MVC:

[!code-csharp[Main](mvc3-release-notes/samples/sample29.cs)]

Implementacje tego interfejsu powinny po prostu delegować do źródłowego kontenera iniekcji zależności w celu zapewnienia zarejestrowanej usługi dla żądanego typu.

Jeśli nie ma żadnych zarejestrowanych usług dla żądanego typu, ASP.NET MVC oczekuje implementacji tego interfejsu, aby zwrócić wartość null z elementu GetService i zwrócić pustą kolekcję z usługi GetServices.

Nowa Klasa DependencyResolver umożliwia rejestrowanie klas implementujących nowy interfejs IDependencyResolver lub wspólny interfejs lokalizatora usługi (IServiceLocator). Aby uzyskać więcej informacji na temat typowego lokalizatora usługi, zobacz [CommonServiceLocator w witrynie GitHub](https://github.com/unitycontainer/commonservicelocator).

<a id="0.1__Breaking_Changes"></a>

#### <a name="new-iviewactivator-interface-for-fine-grained-view-page-instantiation"></a>Nowy interfejs IViewActivator dla tworzenia wystąpienia szczegółowych stron widoku

Nowy interfejs IViewPageActivator zapewnia bardziej precyzyjną kontrolę nad sposobem tworzenia wystąpień stron widoku przy użyciu iniekcji zależności. Dotyczy to zarówno wystąpień webformview, jak i wystąpień RazorView. Poniższy przykład pokazuje nowy interfejs:

[!code-csharp[Main](mvc3-release-notes/samples/sample30.cs)]

Klasy te teraz akceptują argument konstruktora IViewPageActivator, który pozwala na kontrolowanie sposobu tworzenia wystąpień ViewPage, ViewUserControl i WebViewPage.

#### <a name="new-dependency-resolver-support-for-existing-services"></a>Nowa obsługa rozpoznawania zależności dla istniejących usług

Nowe wydanie obejmuje obsługę rozpoznawania zależności dla następujących usług:

- Dostawcy walidacji modelu. Klasy implementujące ModelValidatorProvider mogą być rejestrowane w programie rozpoznawania zależności, a system będzie używać ich do obsługi weryfikacji po stronie klienta i serwera.
- Dostawca metadanych modelu. Pojedyncza Klasa implementująca ModelMetadataProvider może być zarejestrowana w programie rozpoznawania zależności, a system będzie używać jej do dostarczania metadanych dla systemów tworzenia szablonów i sprawdzania poprawności.
- Dostawcy wartości. Klasy implementujące ValueProviderFactory mogą być rejestrowane w programie rozpoznawania zależności, a system będzie używać ich do tworzenia dostawców wartości, które są używane przez kontroler i podczas wiązania modelu.
- Powiązania modeli. Klasy implementujące IModelBinderProvider mogą być rejestrowane w programie rozpoznawania zależności, a system będzie używać ich do tworzenia powiązań modelu, które są używane przez system powiązań modelu.

### <a id="0.1__Toc274034221"></a>Nowe wsparcie dla niezauważalnego opartego na technologii jQuery

ASP.NET MVC zawiera metody pomocnika AJAX, takie jak:

- AJAX. ActionLink
- AJAX. RouteLink
- AJAX. BeginForm
- AJAX. BeginRouteForm

Te metody używają języka JavaScript do wywołania metody akcji na serwerze zamiast korzystania z pełnego ogłaszania zwrotnego. Ta funkcja została zaktualizowana tak, aby korzystać z platformy jQuery w sposób niedyskretny. Zamiast w sposób nieinwazyjny emitować wbudowane skrypty klienta, te metody pomocnika oddzielą zachowanie od znaczników przez emitowanie atrybutów HTML5 przy użyciu prefiksu *Data-AJAX* . Zachowanie jest następnie stosowane do znaczników przez odwołanie do odpowiednich plików JavaScript. Upewnij się, że istnieją następujące pliki JavaScript:

- jQuery-1.4.1. js
- jQuery. niezauważalny. AJAX. js

Ta funkcja jest domyślnie włączona w pliku Web. config w szablonach nowych projektów ASP.NET MVC 3, ale jest domyślnie wyłączona dla istniejących projektów. Aby uzyskać więcej informacji, zobacz [Dodano flagi dla całej aplikacji na potrzeby sprawdzania poprawności klienta i](#0.1_AddedApplicationWideFlagsForClientValida) nieosobnego kodu JavaScript w dalszej części tego dokumentu.

### <a id="0.1__Toc274034222"></a>Nowe wsparcie dla niedyskretnej weryfikacji jQuery

Domyślnie ASP.NET MVC 3 beta używa weryfikacji jQuery w sposób niedyskretny w celu przeprowadzenia walidacji po stronie klienta. Aby włączyć niewygodną weryfikację klienta, wykonaj wywołanie podobne do następujących w widoku:

[!code-csharp[Main](mvc3-release-notes/samples/sample31.cs)]

Wymaga to, aby Właściwość ViewContext. UnobtrusiveJavaScriptEnabled została ustawiona na wartość true, którą można wykonać przy użyciu następującego wywołania:

[!code-csharp[Main](mvc3-release-notes/samples/sample32.cs)]

Upewnij się również, że istnieją odwołania do następujących plików JavaScript.

- jQuery-1.4.1. js
- jQuery. Validate. js
- jQuery. Validate. niezauważalne. js

Ta funkcja jest domyślnie włączona w pliku Web. config w szablonach nowych projektów ASP.NET MVC 3, ale jest domyślnie wyłączona dla istniejących projektów. Aby uzyskać więcej informacji, zobacz [nowe flagi dla całej aplikacji dotyczące sprawdzania poprawności klienta i](#0.1_AddedApplicationWideFlagsForClientValida) nieosobnego kodu JavaScript w dalszej części tego dokumentu.

<a id="0.1__Toc274034223"></a>

### <a id="0.1_AddedApplicationWideFlagsForClientValida"></a>Nowe flagi dla całej aplikacji na potrzeby sprawdzania poprawności klienta i niedyskretnego języka JavaScript

Można włączyć lub wyłączyć weryfikację klienta i niezależny kod JavaScript przy użyciu statycznych elementów członkowskich klasy HtmlHelper, jak w poniższym przykładzie:

[!code-csharp[Main](mvc3-release-notes/samples/sample33.cs)]

Domyślne szablony projektów domyślnie włączają niewygodną obsługę języka JavaScript. Możesz również włączyć lub wyłączyć te funkcje w głównym pliku Web. config aplikacji przy użyciu następujących ustawień:

[!code-xml[Main](mvc3-release-notes/samples/sample34.xml)]

Ponieważ te funkcje można włączyć domyślnie, nowe przeciążenia zostały wprowadzone do klasy HtmlHelper, która umożliwia zastąpienie ustawień domyślnych, jak pokazano w następujących przykładach:

[!code-csharp[Main](mvc3-release-notes/samples/sample35.cs)]

W celu zapewnienia zgodności z poprzednimi wersjami obie te funkcje są domyślnie wyłączone.

### <a id="0.1__Toc274034224"></a>Nowa obsługa kodu, który jest uruchamiany przed uruchomieniem widoków

Teraz można umieścić plik o nazwie \_viewstart. cshtml (lub \_viewstart. vbhtml) w katalogu views i dodać do niego kod, który będzie współużytkowany przez wiele widoków w tym katalogu i jego podkatalogach. Na przykład można umieścić Poniższy kod na stronie \_viewstart. cshtml w folderze ~/views:

[!code-cshtml[Main](mvc3-release-notes/samples/sample36.cshtml)]

Powoduje to ustawienie strony układu dla każdego widoku w folderze widoki i wszystkich jego podfolderów cyklicznie. Gdy widok jest renderowany, kod w pliku \_viewstart. cshtml zostanie uruchomiony przed uruchomieniem kodu widoku. Kod \_viewstart. cshtml ma zastosowanie do każdego widoku w tym folderze.

Domyślnie kod w pliku \_viewstart. cshtml ma również zastosowanie do widoków w dowolnym podfolderze. Jednak poszczególne podfoldery mogą mieć własną wersję pliku \_viewstart. cshtml; w takim przypadku wersja lokalna ma pierwszeństwo. Na przykład aby uruchomić kod, który jest wspólny dla wszystkich widoków dla HomeController, należy umieścić plik \_viewstart. cshtml w folderze ~/Views/Home.

### <a id="0.1__Toc274034225"></a>Nowa Obsługa składni języka Razor języka VBHTML

Poprzednia wersja zapoznawcza ASP.NET MVC obsługiwała widoki wykorzystujące składnia Razor w oparciu o C#. Te widoki używają rozszerzenia pliku. cshtml. W ramach trwającej pracy z obsługą Razor, ASP.NET MVC 3 beta wprowadza obsługę składnia Razor w Visual Basic, która używa rozszerzenia pliku. vbhtml.

Aby zapoznać się z wprowadzeniem do używania składni Visual Basic na stronach VBHTML, zobacz samouczek pod następującym adresem URL:

[https://www.asp.net/webmatrix/tutorials/asp-net-web-pages-visual-basic](../web-pages/overview/getting-started/introducing-razor-syntax-vb.md)

### <a id="0.1__Toc274034226"></a>Większa kontrola nad ValidateInputAttribute

ASP.NET MVC zawsze zawierał klasę ValidateInputAttribute, która wywołuje podstawową infrastrukturę weryfikacji żądań ASP.NET, aby upewnić się, że żądanie przychodzące nie zawiera potencjalnie złośliwych danych wejściowych. Domyślnie sprawdzanie poprawności danych wejściowych jest włączone. Można wyłączyć weryfikację żądań za pomocą atrybutu ValidateInputAttribute, jak w poniższym przykładzie:

[!code-csharp[Main](mvc3-release-notes/samples/sample37.cs)]

Jednak wiele aplikacji sieci Web ma pojedyncze pola formularza, które muszą zezwalać na HTML, natomiast pozostałe pola nie powinny. Klasa ValidateInputAttribute teraz pozwala określić listę pól, które nie powinny być uwzględniane w walidacji żądania.

Na przykład jeśli tworzysz aparat blogów, możesz chcieć zezwolić na użycie znaczników w polach treść i podsumowanie. Te pola mogą być reprezentowane przez dwa elementy wejściowe, z których każdy ma atrybut name odpowiadający nazwie właściwości ("treść" i "Podsumowanie"). Aby wyłączyć weryfikację żądań dla tych pól, określ nazwy (rozdzielone przecinkami) we właściwości Exclude klasy metodzie ValidateInput, jak w poniższym przykładzie:

[!code-csharp[Main](mvc3-release-notes/samples/sample38.cs)]

### <a id="0.1__Toc274034227"></a>Pomocnicy konwertują znaki podkreślenia na łączniki dla nazw atrybutów HTML określonych przy użyciu obiektów anonimowych

Metody pomocnika umożliwiają określanie par nazwa/wartość atrybutu przy użyciu obiektu anonimowego, jak w poniższym przykładzie:

[!code-csharp[Main](mvc3-release-notes/samples/sample39.cs)]

Takie podejście nie pozwala na używanie łączników w nazwie atrybutu, ponieważ łącznik nie może być używany dla nazwy właściwości w ASP.NET. Łączniki są jednak istotne dla niestandardowych atrybutów HTML5; na przykład HTML5 używa prefiksu "Data-".

W tym samym czasie znaki podkreślenia nie mogą być używane dla nazw atrybutów w kodzie HTML, ale są prawidłowe w obrębie nazw właściwości. W związku z tym, jeśli określisz atrybuty przy użyciu obiektu anonimowego, a nazwa atrybutu zawiera podkreślenie, metody pomocnika przekonwertują znaki podkreślenia na łączniki. Na przykład następująca składnia pomocnika używa znaku podkreślenia:

[!code-csharp[Main](mvc3-release-notes/samples/sample40.cs)]

Poprzedni przykład renderuje następujące znaczniki podczas uruchamiania pomocnika:

[!code-html[Main](mvc3-release-notes/samples/sample41.html)]

## <a id="0.1__Toc274034228"></a>Poprawki błędów

Domyślny szablon obiektu dla pomocników szablonu EditorFor i DisplayFor obsługuje teraz kolejność określoną we właściwości unplayattribute. Order. (W poprzednich wersjach ustawienie kolejności nie zostało użyte).

Sprawdzanie poprawności klienta obsługuje teraz weryfikację zastąpionych właściwości, które mają zastosowane atrybuty walidacji.

JsonValueProviderFactory jest teraz domyślnie zarejestrowany.

## <a id="0.1__Toc274034229"></a>Istotne zmiany

Kolejność wykonywania filtrów wyjątków została zmieniona dla filtrów wyjątków, które mają taką samą wartość kolejności. W ASP.NET MVC 2 i starszych, filtry wyjątków na kontrolerze z taką samą kolejnością jak w przypadku metody akcji zostały wykonane przed filtrami wyjątku w metodzie akcji. Zwykle zdarza się to w przypadku zastosowania filtrów wyjątków bez określonej wartości kolejności. W ASP.NET MVC 3 Ta kolejność została odwrócona, tak aby najbardziej specyficzna procedura obsługi wyjątków była wykonywana w pierwszej kolejności. Tak jak w starszych wersjach, jeśli Właściwość Order jest jawnie określona, filtry są uruchamiane w określonej kolejności.

## <a id="0.1__Toc274034230"></a>Znane problemy

Podczas instalacji okno dialogowe akceptacji umowy EULA wyświetla postanowienia licencyjne w oknie, które jest mniejsze niż zamierzone.

Widoki Razor nie obsługują funkcji IntelliSense ani wyróżniania składni. Przewiduje się, że obsługa składnia Razor w programie Visual Studio zostanie uwzględniona w ramach nowszej wersji.

Podczas edytowania widoku Razor (plik cshtml) <a id="0.1__Toc224729061"></a> <a id="0.1__Toc238051347"></a> element menu Przejdź do kontrolera w programie Visual Studio nie będzie dostępny i nie ma fragmentów kodu.

W przypadku używania składni @model do określenia widoku z jednoznacznie określonym typem, skróty specyficzne dla języka dla typów nie są rozpoznawane. Na przykład @model int nie będzie działała, ale @model Int32 będzie działała. Obejście tego błędu polega na użyciu rzeczywistej nazwy typu podczas określania typu modelu.

W przypadku używania składni @model do określenia widoku o jednoznacznie określonym typie CSHTML (lub @ModelType w celu określenia widoku typu "silna wartośćowa"), Typy dopuszczające wartość null i deklaracje tablic nie są obsługiwane. Na przykład @model int? nie jest obsługiwana. Zamiast tego należy użyć `@model Nullable<Int32>`. Składnia @model String [] nie jest również obsługiwana; Zamiast tego należy użyć `@model IList<string>`.

Podczas uaktualniania projektu ASP.NET MVC 2 do ASP.NET MVC 3 należy dodać następujące elementy do sekcji appSettings w pliku Web. config:

[!code-xml[Main](mvc3-release-notes/samples/sample42.xml)]

Istnieje znany problem, który powoduje, że uwierzytelnianie formularzy zawsze przekierowuje nieuwierzytelnionych użytkowników do ~/Account/Login, ignorując ustawienie uwierzytelniania formularzy używane w pliku Web. config. Obejście polega na dodaniu następującego ustawienia aplikacji.

[!code-xml[Main](mvc3-release-notes/samples/sample43.xml)]

## <a id="0.1__Toc274034231"></a>Zastrzeżenie

© 2011 Microsoft Corporation. Wszelkie prawa zastrzeżone. Ten dokument jest dostarczany "w takiej postaci, w jakim jest". Informacje i poglądy wyrażone w tym dokumencie, w tym adresy URL i inne odwołania do witryn internetowych, mogą ulec zmianie bez powiadomienia. Użytkownik ponosi ryzyko związane z korzystaniem z niego.

Niniejszy dokument nie udostępnia żadnych praw do jakiejkolwiek własności intelektualnej w jakimkolwiek produkcie firmy Microsoft. Licencjobiorca może kopiować i używać tego dokumentu do wewnętrznych celów referencyjnych.

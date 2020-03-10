---
uid: mvc/mvc3
title: ASP.NET MVC 3
author: rick-anderson
description: (obejmuje aktualizację narzędzi 2011 kwietnia) ASP.NET MVC 3 to platforma do tworzenia skalowalnych, opartych na standardach aplikacji sieci Web korzystających z dobrze ustanowionego wzorca projektowego...
ms.author: riande
ms.date: 10/05/2010
ms.assetid: dddc8812-a0bc-49f9-aafb-caf2064c2b8c
msc.legacyurl: /mvc/mvc3
msc.type: content
ms.openlocfilehash: 421a06c89d4dbcb05d4080033813cc6558b7c698
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78616411"
---
# <a name="aspnet-mvc-3"></a>ASP.NET MVC 3

> *(obejmuje aktualizację narzędzi 2011 kwietnia)*
> 
> ASP.NET MVC 3 to platforma służąca do tworzenia skalowalnych, opartych na standardach aplikacji sieci Web przy użyciu dobrze ustanowionych wzorców projektowych oraz możliwości ASP.NET i .NET Framework.
> 
> Jest on instalowany równolegle z ASP.NET MVC 2, więc Zacznij korzystać z niego już dziś!
> 
> Pobierz [Instalatora tutaj](https://go.microsoft.com/fwlink/?LinkID=208140)

## <a name="top-features"></a>Najważniejsze funkcje

- Zintegrowany system tworzenia szkieletów rozszerzalny za pomocą narzędzia NuGet
- Szablony projektów z włączoną obsługą języka HTML 5
- Widoki ekspresowe, w tym nowy aparat widoku Razor
- Zaawansowane punkty zaczepienia z iniekcją zależności i globalnymi filtrami akcji
- Rozbudowana obsługa języka JavaScript z niewygodnym kodem JavaScript, walidacją jQuery i powiązaniem JSON
- *Przeczytaj pełną listę funkcji [poniżej](#overview)*

## <a name="top-links"></a>Górne linki

Co nowego w ASP.NET MVC 3

- Krzysztof Haack: [wydano ASP.NET MVC 3](http://haacked.com/archive/2011/01/13/aspnetmvc3-released.aspx)
- Scott Hanselman: [ASP.NET MvC3, WebMatrix, NuGet, IIS Express i sadu wydana — wydanie sieci Web Microsoft styczeń w kontekście](http://www.hanselman.com/blog/ASPNETMVC3WebMatrixNuGetIISExpressAndOrchardReleasedTheMicrosoftJanuaryWebReleaseInContext.aspx)
- Scott Guthrie: [ogłasza wydanie ASP.NET MVC 3, IIS Express, SQL CE 4, Web Farm Framework, sadu, WebMatrix](https://weblogs.asp.net/scottgu/archive/2011/01/13/announcing-release-of-asp-net-mvc-3-iis-express-sql-ce-4-web-farm-framework-orchard-webmatrix.aspx)
- [Informacje o wersji dla ASP.NET MVC 3](../whitepapers/mvc3-release-notes.md)

Instalacja i pomoc

- Zainstaluj ASP.NET MVC 3 przy użyciu [Instalatora platformy sieci Web (zalecane)](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MVC3)
- Zainstaluj ASP.NET MVC 3 przy użyciu [pliku wykonywalnego instalatora](https://go.microsoft.com/fwlink/?LinkID=208140)
- Zainstaluj [ASP.NET MVC 3 dla programu Visual Studio 11 Developer Preview](https://go.microsoft.com/fwlink/?LinkID=208140)
- Przeczytaj [wprowadzenie do ASP.NET MVC 3 — samouczek](overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)
- Uzyskaj pomoc i omów ASP.NET MVC 3 na [forach](https://forums.asp.net/1146.aspx)

<a id="overview"></a>
## <a name="aspnet-mvc-3-overview"></a>ASP.NET MVC 3 — Omówienie

ASP.NET MVC 3 korzysta z ASP.NET MVC 1 i 2, dodając doskonałe funkcje, które upraszczają kod i umożliwiają dokładniejsze rozszerzanie. Ten temat zawiera omówienie wielu nowych funkcji uwzględnionych w tej wersji, zorganizowanych w następujące sekcje:

- [Rozszerzalne tworzenie szkieletów z integracją MvcScaffold](#BM_MvcScaffolding)
- [Szablony projektów z włączoną obsługą języka HTML 5](#BM_HTML5)
- [Aparat widoku Razor](#BM_TheRazorViewEngine)
- [Obsługa wielu aparatów widoku](#BM_Support_for_Multiple_View_Engines)
- [Ulepszenia kontrolera](#BM_Controller_Improvements)
- [JavaScript i AJAX](#BM_JavaScript_and_Ajax_Improvements)
- [Ulepszenia weryfikacji modelu](#BM_Model_Validation_Improvements)
- [Ulepszenia iniekcji zależności](#BM_Dependency_Injection_Improvements)
- [Inne nowe funkcje](#BM_Other_New_Features)

<a id="BM_MvcScaffolding"></a>

## <a name="extensible-scaffolding-with-mvcscaffold-integration"></a>Rozszerzalne tworzenie szkieletów z integracją MvcScaffold

Nowy system tworzenia szkieletów ułatwia szybkie i szybkie rozpoczęcie pracy w przypadku, gdy jest to zupełnie nowe środowisko i automatyzuje typowe zadania programistyczne, jeśli masz doświadczenie i już wiesz, co robisz.

Jest to obsługiwane przez nowy pakiet *szkieletu* NuGet o nazwie **MvcScaffolding**. Termin "rusztowania" jest używany przez wiele technologii oprogramowania do oznaczania "szybkiego generowania podstawowego konspektu oprogramowania, które można następnie edytować i dostosowywać". Pakiet szkieletu tworzony przez nas dla ASP.NET MVC jest znacznie korzystny w kilku scenariuszach:

- **Jeśli ASP.NET MVC po raz pierwszy**, ponieważ umożliwia szybkie uzyskanie przydatnego, działającego kodu, który można następnie edytować i dostosować zgodnie z potrzebami. Zapisuje Cię z Trauma na pustej stronie i nie ma pomysłu, gdzie zacząć!
- **Jeśli wiesz, że ASP.NET MVC i teraz eksplorujemy kilka nowych technologii** , takich jak mapowanie relacyjne obiektów, aparat widoku, biblioteka testowa, itp., ponieważ twórca tej technologii mógł także utworzyć dla niego pakiet szkieletu.
- **Jeśli prace wymagają wielokrotnego tworzenia podobnych klas lub plików z pewnej kolejności**, ponieważ można utworzyć niestandardowe generatory, które umożliwiają testowanie armatury, skryptów wdrażania lub innych potrzeb. Wszyscy członkowie zespołu mogą również używać niestandardowych szkieletów.

Inne funkcje w programie MvcScaffolding obejmują:

- Obsługa i C# projekty w języku VB
- Obsługa aparatów widoku Razor i ASPX
- Obsługuje tworzenie szkieletów w obszarach ASP.NET MVC i używanie układów widoków niestandardowych/wzorców
- Możesz łatwo dostosować dane wyjściowe, edytując szablony T4
- Można dodać zupełnie nowych szkieletów przy użyciu niestandardowych logiki programu PowerShell i niestandardowych szablonów T4. Te (i wszystkie podane przez Ciebie parametry niestandardowe) są automatycznie wyświetlane na liście uzupełniania na karcie konsoli.
- Pakiety NuGet zawierające dodatkowe rusztowania mogą być dostępne dla różnych technologii (np. istnieje Weryfikacja koncepcji jeden dla LINQ to SQL teraz) i mieszanie ich razem

Aktualizacja narzędzi ASP.NET MVC 3 obejmuje doskonałą obsługę programu Visual Studio dla tego systemu szkieletowego, na przykład:

- Okno dialogowe Dodawanie kontrolera obsługuje teraz pełne automatyczne tworzenie szkieletu akcji tworzenia, odczytywania, aktualizowania i usuwania kontrolerów oraz odpowiednich widoków. Domyślnie to szkielety kodu uzyskują dostęp do danych przy użyciu EF Code First.
- Okno dialogowe Dodawanie kontrolera obsługuje *rozszerzalne szkielety* za pośrednictwem pakietów NuGet, takich jak *MvcScaffolding*. Dzięki temu można podłączać się do okna dialogowego, które umożliwią tworzenie szkieletów dla innych technologii dostępu do danych, takich jak NHibernate, lub nawet JET z ODBCDirect, jeśli są one nachylone!

Aby uzyskać więcej informacji na temat tworzenia szkieletów w ASP.NET MVC 3, zobacz następujące zasoby:

- Steve Sanderson, w tym: 

    1. [Wprowadzenie: tworzenie szkieletu projektu ASP.NET MVC 3 z pakietem MvcScaffolding](http://blog.stevensanderson.com/2011/01/13/scaffold-your-aspnet-mvc-3-project-with-the-mvcscaffolding-package/)
    2. [Standardowe użycie: typowe przypadki użycia i opcje](http://blog.stevensanderson.com/2011/01/13/mvcscaffolding-standard-usage/)
    3. [Relacje jeden do wielu](http://blog.stevensanderson.com/2011/01/28/mvcscaffolding-one-to-many-relationships/)
    4. [Akcje tworzenia szkieletu i testy jednostkowe](http://blog.stevensanderson.com/2011/03/28/scaffolding-actions-and-unit-tests-with-mvcscaffolding/)
    5. [Zastępowanie szablonów T4](http://blog.stevensanderson.com/2011/04/06/mvcscaffolding-overriding-the-t4-templates/)
    6. [Ten wpis: Tworzenie niestandardowych szkieletów](http://blog.stevensanderson.com/2011/04/07/mvcscaffolding-creating-custom-scaffolders/)
- Wpis Scott Hanselman z własnej sesji kontrolera PDC 2010 [Tworzenie blogu z nienazwanym pakietem Webmiłości firmy Microsoft "](http://www.hanselman.com/blog/PDC10BuildingABlogWithMicrosoftUnnamedPackageOfWebLove.aspx)
- [Informacje o wersji MVC 3](../whitepapers/mvc3-release-notes.md)

<a id="BM_HTML5"></a>

## <a name="html-5-project-templates"></a>Szablony projektów HTML 5

Okno dialogowe Nowy projekt zawiera pole wyboru Włącz wersje HTML 5 szablonów projektu. Te szablony wykorzystują modernizację 1,7, aby zapewnić obsługę zgodności dla języków HTML 5 i CSS 3 w przeglądarkach niskiego poziomu.

<a id="BM_TheRazorViewEngine"></a>

## <a name="the-razor-view-engine"></a>Aparat widoku Razor

ASP.NET MVC 3 jest dostarczany z nowym aparatem widoku o nazwie Razor, który oferuje następujące korzyści:

- Składnia Razor jest czysty i zwięzły, co wymaga minimalnej liczby naciśnięć klawiszy.
- Razor można łatwo dowiedzieć się, w części, ponieważ jest ona oparta na istniejących C# językach, takich jak i Visual Basic.
- Program Visual Studio zawiera funkcję IntelliSense i kolorowanie kodu dla składnia Razor.
- Widoki Razor mogą być badane jednostkowo bez konieczności uruchamiania aplikacji lub uruchamiania serwera sieci Web.

Dostępne są następujące nowe funkcje Razor:

- Składnia `@model` do określania typu, który jest przesyłany do widoku.
- Składnia komentarza `@* *@`.
- Możliwość określenia wartości domyślnych (takich jak `layoutpage`) jednokrotnie dla całej lokacji.
- Metoda `Html.Raw` do wyświetlania tekstu bez kodowania kodu HTML.
- Obsługa udostępniania kodu w wielu widokach ( *\_viewstart. cshtml* lub *\_viewstart. vbhtml* ).

Razor również zawiera nowych pomocników HTML, takich jak:

- `Chart`. Renderuje wykres, oferując te same funkcje co formant wykresu w ASP.NET 4.
- `WebGrid`. Renderuje siatkę danych, dokończ przy użyciu funkcji stronicowania i sortowania.
- `Crypto`. Używa algorytmów wyznaczania wartości skrótu do prawidłowego tworzenia haseł solonych i skrótów.
- `WebImage`. Renderuje obraz.
- `WebMail`. Wysyła wiadomość e-mail.

Aby uzyskać więcej informacji na temat Razor, zobacz następujące zasoby:

- [Wpis w blogu Scott Guthrie wprowadzający Razor](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx)
- [Wpis w blogu Scott Guthrie wprowadzający słowo kluczowe @model](https://weblogs.asp.net/scottgu/archive/2010/10/19/asp-net-mvc-3-new-model-directive-support-in-razor.aspx)
- [Wpis w blogu Scott Guthrie wprowadzający układy Razor](https://weblogs.asp.net/scottgu/archive/2010/10/22/asp-net-mvc-3-layouts.aspx)
- [Krótki przewodnik po interfejsie API Razor](../web-pages/overview/api-reference/asp-net-web-pages-api-reference.md)
- [Informacje o wersji MVC 3](../whitepapers/mvc3-release-notes.md)

<a id="BM_Support_for_Multiple_View_Engines"></a>

## <a name="support-for-multiple-view-engines"></a>Obsługa wielu aparatów widoku

Okno dialogowe **Dodawanie widoku** w ASP.NET MVC 3 umożliwia wybranie aparatu widoku, z którym chcesz współpracować, a okno dialogowe **Nowy projekt** umożliwia określenie domyślnego aparatu widoku dla projektu. Można wybrać aparat widoku formularzy sieci Web (ASPX), Razor lub aparatu widoku typu open source, taki jak [Spark](http://sparkviewengine.com/), [NHaml](https://code.google.com/p/nhaml/)lub [NDjango](http://ndjango.org/).

<a id="BM_Controller_Improvements"></a>

## <a name="controller-improvements"></a>Ulepszenia kontrolera

### <a name="global-action-filters"></a>Globalne filtry akcji

Czasami chcesz przeprowadzić logikę przed uruchomieniem metody akcji lub po uruchomieniu metody akcji. Aby można było go obsługiwać, dostępne są filtry akcji ASP.NET MVC 2. Filtry akcji są atrybutami niestandardowymi, które zapewniają deklaratywny sposób dodawania działań przed akcjami i po akcji do określonych metod akcji kontrolera. Jednak w niektórych przypadkach może być konieczne określenie zachowania przed akcją lub po akcji, które ma zastosowanie do wszystkich metod akcji. MVC 3 pozwala określić filtry globalne, dodając je do kolekcji `GlobalFilters`. Aby uzyskać więcej informacji na temat globalnych filtrów akcji, zobacz następujące zasoby:

- [Blog Scott Guthrie w wersji zapoznawczej MVC 3](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx)
- [Filtrowanie w ASP.NET MVC](https://msdn.microsoft.com/library/gg416513(VS.98).aspx)

### <a name="new-viewbag-property"></a>Nowa właściwość "ViewBag"

Kontrolery MVC 2 obsługują Właściwość `ViewData`, która umożliwia przekazywanie danych do szablonu widoku przy użyciu interfejsu API z późnym wiązaniem. W MVC 3 można również użyć nieco prostszej składni z właściwością `ViewBag`, aby wykonać to samo przeznaczenie. Na przykład zamiast pisania `ViewData["Message"]="text"`można pisać `ViewBag.Message="text"`. Nie trzeba definiować żadnych klas o jednoznacznie określonym typie, aby użyć właściwości `ViewBag`. Ponieważ jest to właściwość dynamiczna, można w zamian pobrać lub ustawić właściwości, co spowoduje ich dynamiczne rozwiązanie w czasie wykonywania. Wewnętrznie właściwości `ViewBag` są przechowywane jako pary nazwa/wartość w słowniku `ViewData`. (Uwaga: w większości wstępnych wersji MVC 3 Właściwość `ViewBag` miała nazwę właściwości `ViewModel`.)

### <a name="new-actionresult-types"></a>Nowe typy "ActionResult"

Następujące typy `ActionResult` i odpowiednie metody pomocnika są nowe lub udoskonalone w MVC 3:

- [HttpNotFoundResult](https://msdn.microsoft.com/library/system.web.mvc.httpnotfoundresult(v=vs.98).aspx). Zwraca kod stanu HTTP 404 dla klienta.
- [RedirectResult](https://msdn.microsoft.com/library/system.web.mvc.redirectresult(v=VS.98).aspx). Zwraca tymczasowe przekierowanie (kod stanu HTTP 302) lub trwałe przekierowanie (kod stanu HTTP 301), w zależności od parametru logicznego. W połączeniu z tą zmianą Klasa [kontrolera](https://msdn.microsoft.com/library/system.web.mvc.controller(v=VS.98).aspx) ma teraz trzy metody wykonywania trwałych przekierowań: `RedirectPermanent`, `RedirectToRoutePermanent`i `RedirectToActionPermanent`. Te metody zwracają wystąpienie `RedirectResult` z właściwością `Permanent` ustawioną na `true`.
- [HttpStatusCodeResult](https://msdn.microsoft.com/library/system.web.mvc.httpstatuscoderesult(v=VS.98).aspx). Zwraca kod stanu HTTP określony przez użytkownika.

<a id="BM_JavaScript_and_Ajax_Improvements"></a>

## <a name="javascript-and-ajax-improvements"></a>Ulepszenia języka JavaScript i AJAX

Domyślnie pomocnicy technologii AJAX i walidacji w MVC 3 używają niezauważalnego podejścia JavaScript. Niezauważalne środowisko JavaScript zapobiega wprowadzaniu w kodzie HTML wbudowanego języka JavaScript. Dzięki temu kod HTML jest mniejszy i mniej czytelny oraz ułatwia wymianę lub dostosowywanie bibliotek języka JavaScript. Pomocnicy weryfikacji w MVC 3 również domyślnie używają wtyczki `jQueryValidate`. Jeśli chcesz zachowania MVC 2, możesz wyłączyć oddyskretny kod JavaScript przy użyciu ustawienia pliku *Web. config* . Aby uzyskać więcej informacji na temat ulepszeń JavaScript i AJAX, zobacz następujące zasoby:

- [Podstawowe wprowadzenie do niewygodnego języka JavaScript w witrynie Wikipedia](http://en.wikipedia.org/wiki/Unobtrusive_JavaScript)
- [Niewygodny wpis języka JavaScript w programie Brad Wilson](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-ajax.html)
- [Wpis do sprawdzania poprawności kodu JavaScript w programie Brad Wilson](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html)
- [Tworzenie aplikacji MVC 3 z użyciem Razor i niewygodnego języka JavaScript](overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript.md) (samouczek w witrynie ASP.NET)
- [Informacje o wersji MVC 3](../whitepapers/mvc3-release-notes.md)

### <a name="client-side-validation-enabled-by-default"></a>Sprawdzanie poprawności po stronie klienta jest domyślnie włączone

We wcześniejszych wersjach składnika MVC należy jawnie wywołać metodę `Html.EnableClientValidation` z widoku, aby umożliwić weryfikację po stronie klienta. W MVC 3 nie jest to już wymagane, ponieważ Walidacja po stronie klienta jest domyślnie włączona. (Można wyłączyć tę funkcję przy użyciu ustawienia w pliku *Web. config* ).

Aby Walidacja po stronie klienta działała, nadal trzeba odwoływać się do odpowiednich bibliotek walidacji jQuery i jQuery w Twojej lokacji. Te biblioteki można hostować na własnym serwerze lub odwoływać się do nich za pośrednictwem usługi Content Delivery Network (CDN), takiej jak sieci CDN firmy Microsoft lub Google.

### <a name="remote-validator"></a>Zdalny moduł sprawdzania poprawności

ASP.NET MVC 3 obsługuje nową klasę [remoteattribute](https://msdn.microsoft.com/library/system.web.mvc.remoteattribute(v=VS.98).aspx) , która pozwala korzystać z obsługi zdalnego modułu sprawdzania poprawności dla wtyczki. Dzięki temu Biblioteka walidacji po stronie klienta może automatycznie wywołać metodę niestandardową, która została zdefiniowana na serwerze w celu wykonania logiki walidacji, którą można wykonać tylko po stronie serwera.

W poniższym przykładzie atrybut `Remote` określa, że Walidacja klienta wywoła akcję o nazwie `UserNameAvailable` na klasie `UsersController` w celu sprawdzenia poprawności pola `UserName`.

[!code-csharp[Main](mvc3/samples/sample1.cs)]

Poniższy przykład pokazuje odpowiedni kontroler.

[!code-csharp[Main](mvc3/samples/sample2.cs)]

Aby uzyskać więcej informacji na temat korzystania z atrybutu `Remote`, zobacz [How to: Implementuj zdalne sprawdzanie poprawności w ASP.NET MVC](https://msdn.microsoft.com/library/gg508808(VS.98).aspx) w bibliotece MSDN.

### <a name="json-binding-support"></a>Obsługa powiązań JSON

ASP.NET MVC 3 zawiera wbudowaną obsługę powiązań JSON, która umożliwia metodom działania otrzymywanie danych i modelu z kodowaniem JSON — powiązanie ich z parametrami akcji-metody. Ta funkcja jest przydatna w scenariuszach obejmujących szablony klientów i powiązania danych. (Szablony klienta umożliwiają formatowanie i wyświetlanie pojedynczego elementu danych lub zestawu elementów danych przy użyciu szablonów, które są wykonywane na kliencie). MVC 3 umożliwia łatwe łączenie szablonów klientów z metodami akcji na serwerze, który wysyła i odbiera dane JSON. Aby uzyskać więcej informacji na temat obsługi powiązań JSON, zobacz sekcję **ulepszenia języka JavaScript i technologii AJAX** w [blogu Guthrie MVC 3 w wersji zapoznawczej](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx).

<a id="BM_Model_Validation_Improvements"></a>

## <a name="model-validation-improvements"></a>Ulepszenia weryfikacji modelu

### <a name="dataannotations-metadata-attributes"></a>Atrybuty metadanych "DataAnnotations"

ASP.NET MVC 3 obsługuje atrybuty metadanych `DataAnnotations`, takie jak `DisplayAttribute`.

### <a name="validationattribute-class"></a>Klasa "ValidationAttribute"

Ulepszona Klasa `ValidationAttribute` w .NET Framework 4, aby obsługiwała nowe Przeciążenie `IsValid`, które zawiera więcej informacji na temat bieżącego kontekstu walidacji, takich jak sprawdzanie poprawności obiektu. Umożliwia to bogatsze scenariusze, w których można sprawdzić bieżącą wartość w oparciu o inną właściwość modelu. Na przykład nowy atrybut `CompareAttribute` umożliwia porównanie wartości dwóch właściwości modelu. W poniższym przykładzie właściwość `ComparePassword` musi być zgodna z polem `Password`, aby było prawidłowe.

[!code-csharp[Main](mvc3/samples/sample3.cs)]

### <a name="validation-interfaces"></a>Interfejsy walidacji

Interfejs [IValidatableObject](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.ivalidatableobject.aspx) umożliwia wykonywanie walidacji na poziomie modelu i umożliwia podanie komunikatów o błędach walidacji, które są specyficzne dla stanu ogólnego modelu lub między dwiema właściwościami w modelu. MVC 3 teraz pobiera błędy z interfejsu `IValidatableObject` podczas powiązania modelu i automatycznie flaguje lub wyróżnia pola, których dotyczy ten widok, przy użyciu wbudowanych pomocników formularzy HTML.

Interfejs [IClientValidatable](https://msdn.microsoft.com/library/system.web.mvc.iclientvalidatable(v=VS.98).aspx) umożliwia ASP.NET MVC odnajdywanie w czasie wykonywania, niezależnie od tego, czy moduł sprawdzania poprawności obsługuje walidację klienta. Ten interfejs został zaprojektowany tak, aby można było zintegrować go z różnymi platformami sprawdzania poprawności.

Aby uzyskać więcej informacji na temat interfejsów walidacji, zobacz sekcję **ulepszenia walidacji modelu** w [blogu Scott Guthrie w wersji zapoznawczej](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx). Należy jednak zauważyć, że odwołanie do "IValidateObject" w blogu powinno mieć wartość "IValidatableObject".

<a id="BM_Dependency_Injection_Improvements"></a>

## <a name="dependency-injection-improvements"></a>Ulepszenia iniekcji zależności

ASP.NET MVC 3 oferuje lepszą obsługę stosowania iniekcji zależności (DI) oraz do integrowania przy użyciu iniekcji zależności lub kontenerów kontroli wersji (IOC). Obsługa funkcji DI została dodana w następujących obszarach:

- Kontrolery (rejestrowanie i wtryskiwanie fabryk kontrolerów, iniekcja kontrolerów).
- Widoki (rejestrowanie i wtryskiwanie aparatów wyświetlania, wprowadzanie zależności do stron widoku).
- Filtry akcji (lokalizowanie i wtryskiwanie filtrów).
- Powiązania modelu (rejestrowanie i wtryskiwanie).
- Dostawcy walidacji modelu (rejestrowanie i iniekcja).
- Dostawcy metadanych modelu (rejestrowanie i wtryskiwanie).
- Dostawcy wartości (rejestrowanie i wtryskiwanie).

MVC 3 obsługuje bibliotekę [Common Service Locator](https://github.com/unitycontainer/commonservicelocator) i wszystkie di kontener obsługujące interfejs `IServiceLocator` tej biblioteki. Obsługuje również nowy interfejs `IDependencyResolver`, który ułatwia integrację DI Frameworks.

Aby uzyskać więcej informacji na temat DI w MVC 3, zobacz następujące zasoby:

- [Seria wpisów w blogu Brada Wilson w lokalizacji usługi](http://bradwilson.typepad.com/blog/2010/07/service-location-pt1-introduction.html)
- [Informacje o wersji MVC 3](../whitepapers/mvc3-release-notes.md)

<a id="BM_Other_New_Features"></a>

## <a name="other-new-features"></a>Inne nowe funkcje

### <a name="nuget-integration"></a>Integracja z pakietem NuGet

ASP.NET MVC 3 automatycznie instaluje i włącza pakiet NuGet w ramach instalacji. NuGet to bezpłatny Menedżer pakietów typu "open source", który ułatwia znajdowanie, Instalowanie i używanie bibliotek i narzędzi platformy .NET w projektach. Działa ze wszystkimi typami projektów programu Visual Studio (w tym ASP.NET Web Forms i ASP.NET MVC).

Narzędzia NuGet umożliwiają deweloperom, którzy utrzymują projekty typu Open Source (na przykład projekty, takie jak MOQ, NHibernate, Ninject, StructureMap, NUnit, Windsor, RhinoMocks i ELMAH), w celu spakowania ich bibliotek i zarejestrowania ich w galerii w trybie online. Jest to łatwe dla deweloperów platformy .NET, którzy chcą korzystać z jednej z tych bibliotek, aby znaleźć pakiet i zainstalować go w projektach, nad którymi pracują.

W przypadku aktualizacji narzędzi ASP.NET 3 szablony projektów obejmują wstępnie zainstalowane pakiety NuGet w bibliotekach języka JavaScript, dzięki czemu są one aktualizowalne za pośrednictwem programu NuGet. Code First Entity Framework jest również wstępnie instalowany jako pakiet NuGet.

Więcej informacji o narzędziu NuGet można znaleźć w [dokumentacji programu NuGet](https://docs.microsoft.com/nuget/).

### <a name="partial-page-output-caching"></a>Częściowe buforowanie danych wyjściowych strony

ASP.NET MVC obsługuje buforowanie danych wyjściowych odpowiedzi pełnej strony od wersji 1. MVC 3 obsługuje również buforowanie danych wyjściowych częściowej strony, co pozwala łatwo buforować regiony lub fragmenty odpowiedzi. Aby uzyskać więcej informacji na temat buforowania, zobacz sekcję **częściowe buforowanie danych wyjściowych** [w blogu Scott GUTHRIE w sekcji MVC 3 Release Candidate](https://weblogs.asp.net/scottgu/archive/2010/11/09/announcing-the-asp-net-mvc-3-release-candidate.aspx) i **podrzędnej pamięci podręcznej akcji** w [informacjach o wersji MVC 3](../whitepapers/mvc3-release-notes.md).

### <a name="granular-control-over-request-validation"></a>Szczegółową kontrolę nad sprawdzaniem poprawności żądania

ASP.NET MVC ma wbudowaną weryfikację żądań, która automatycznie pomaga chronić przed atakami typu XSS i kodu HTML. Jednak czasami chcesz jawnie wyłączyć weryfikację żądań, na przykład jeśli chcesz zezwolić użytkownikom na publikowanie zawartości HTML (na przykład wpisów w blogu lub zawartości CMS). Teraz można dodać atrybut [AllowHtml](https://msdn.microsoft.com/library/system.web.mvc.allowhtmlattribute(v=VS.98).aspx) do modeli lub wyświetlić modele, aby wyłączyć weryfikację żądań dla poszczególnych właściwości podczas powiązania modelu. Aby uzyskać więcej informacji na temat weryfikacji żądań, zobacz następujące zasoby:

- **Niewygodna sekcja JavaScript i walidacja** w [blogu Scotta GUTHRIE w witrynie MVC 3 Release Candidate](https://weblogs.asp.net/scottgu/archive/2010/11/09/announcing-the-asp-net-mvc-3-release-candidate.aspx).
- [Informacje o wersji MVC 3](../whitepapers/mvc3-release-notes.md)

### <a name="extensible-new-project-dialog-box"></a>Rozszerzalne okno dialogowe "nowy projekt"

W ASP.NET MVC 3 można dodawać szablony projektów, wyświetlać aparaty i struktury projektu testów jednostkowych do okna dialogowego **Nowy projekt** .

### <a name="template-scaffolding-improvements"></a>Ulepszenia tworzenia szkieletów szablonów

Szablony szkieletowe ASP.NET MVC 3 to lepsze zadanie identyfikacji głównych właściwości klucza dla modeli i obsługiwania ich odpowiednio w starszych wersjach składnika MVC. (Na przykład szablony tworzenia szkieletów powinny teraz upewnić się, że klucz podstawowy nie jest szkieletem jako pole formularza do edycji).

Domyślnie szkielety tworzenia i edytowania używają teraz pomocnika `Html.EditorFor` zamiast pomocnika `Html.TextBoxFor`. Poprawia to obsługę metadanych w modelu w postaci atrybutów adnotacji danych, gdy okno dialogowe **Dodawanie widoku** generuje widok.

### <a name="new-overloads-for-htmllabelfor-and-htmllabelformodel"></a>Nowe przeciążenia dla "html. LabelFor" i "html. LabelForModel"

Dodano nowe przeciążenia metody dla `LabelFor` i `LabelForModel` metod pomocniczych. Nowe przeciążenia umożliwiają określanie lub zastępowanie tekstu etykiety.

### <a name="sessionless-controller-support"></a>Obsługa kontrolera bezsesyjnego

W ASP.NET MVC 3 możesz wskazać, czy chcesz, aby Klasa kontrolera korzystała z stanu sesji, a jeśli tak, to czy stan sesji powinien mieć wartość odczyt/zapis lub tylko do odczytu. Aby uzyskać więcej informacji na temat obsługi kontrolera bezsesyjnego, zobacz [Informacje o wersji MVC 3](../whitepapers/mvc3-release-notes.md).

### <a name="new-additionalmetadataattribute-class"></a>Nowa klasa "AdditionalMetadataAttribute"

Można użyć atrybutu [AdditionalMetadata](https://msdn.microsoft.com/library/system.web.mvc.additionalmetadataattribute(v=VS.98).aspx) do wypełniania słownika `ModelMetadata.AdditionalValues` dla właściwości modelu. Na przykład jeśli model widoku ma właściwość, która powinna być wyświetlana tylko administrator, można dodać adnotacje do tej właściwości, jak pokazano w następującym przykładzie:

[!code-csharp[Main](mvc3/samples/sample4.cs)]

Te metadane są udostępniane w dowolnym szablonie wyświetlania lub edytora, gdy jest renderowany model widoku produktu. Można interpretować informacje o metadanych.

### <a name="accountcontroller-improvements"></a>Udoskonalenia elementu AccountController

Elementu AccountController w szablonie projektu Internet został znacznie ulepszony.

### <a name="new-intranet-project-template"></a>Nowy szablon projektu intranetowego

Dołączany jest nowy szablon projektu intranetowego, który umożliwia uwierzytelnianie systemu Windows i usuwa elementu AccountController.

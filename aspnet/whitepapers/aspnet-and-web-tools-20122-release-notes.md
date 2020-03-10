---
uid: whitepapers/aspnet-and-web-tools-20122-release-notes
title: Informacje o wersji ASP.NET and Web Tools 2012,2 | Microsoft Docs
author: rick-anderson
description: Informacje o wersji dla ASP.NET and Web Tools 2012,2.
ms.author: riande
ms.date: 02/14/2013
ms.assetid: bdb18d02-9f61-4676-836d-6fdea94f9282
msc.legacyurl: /whitepapers/aspnet-and-web-tools-20122-release-notes
msc.type: content
ms.openlocfilehash: a4ea1d7c146309e1d5e8be944d496e9fd87bca3e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78523780"
---
# <a name="aspnet-and-web-tools-20122-release-notes"></a>Informacje o wersji rozszerzenia ASP.NET and Web Tools 2012.2

> W tym dokumencie opisano wydanie ASP.NET and Web Tools 2012,2. Jest to aktualizacja narzędzi internetowych programu Visual Studio i ASP.NET.

- [Uwagi dotyczące instalacji](#_Installation)
- [Dokumentacja](#_Documentation)
- [Pomoc techniczna](#_Support)
- [Wymagania dotyczące oprogramowania](#_Software_Requirements)
- [Nowe funkcje w ASP.NET and Web Tools 2012,2](#_New_Features_in)

    - [Narzędzi](#_Tooling)
    - [Publikowanie w sieci Web](#_Web_Publishing)
    - [Szablony ASP.NET MVC](#_Templates)
    - [ASP.NET Web API](#_ASP.NET_Web_API)

    - [ASP.NET sygnalizujący](#_ASP.NET_SignalR)
    - [Przyjazne adresy URL ASP.NET](#_ASP.NET_Friendly_URLs)
- [Znane problemy i istotne zmiany](#_Known_Issues_and)

<a id="_Installation"></a>
## <a name="installation-notes"></a>Uwagi dotyczące instalacji

ASP.NET and Web Tools 2012,2 dla programu Visual Studio 2012 można zainstalować przy użyciu [Instalatora platformy sieci Web](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=ASPDOTNETandWebTools2012_2). Jest to aktualizacja programu Visual Studio 2012 lub Visual Studio Express 2012 dla sieci Web, która jest wymagana. Jeśli nie masz zainstalowanego programu Visual Studio, zostanie zainstalowana Visual Studio Express 2012 dla sieci Web.

Możesz również ręcznie zainstalować ASP.NET and Web Tools 2012,2. Musisz mieć zainstalowany program Visual Studio 2012 lub Visual Studio Express 2012 dla sieci Web. Następnie użyj następujących instrukcji: 

1. Pobierz instalatora [ASP.NET i Web frameworks 2012,2](https://download.microsoft.com/download/6/5/6/6562AFBE-9503-4E64-970C-1427133FCD73/AspNetWebTools2012Setup.exe) z centrum pobierania.
2. Po wyświetleniu monitu kliknij przycisk Uruchom. Możesz również zapisać plik, aby uruchomić go później.
3. Sprawdź wersję programu Visual Studio, którą zaktualizujesz. Możesz to zrobić, uruchamiając program Visual Studio, który chcesz zaktualizować. Następnie kliknij element menu Pomoc.   
    ![](aspnet-and-web-tools-20122-release-notes/_static/image1.jpg)
4. Jeśli zobaczysz element menu &quot;o Microsoft Visual Studio 2012 dla sieci Web&quot; następnie Pobierz [sieć web Narzędzia deweloperskie 2012,2-Visual Studio Express 2012 for Web](https://go.microsoft.com/fwlink/?LinkID=282228). W przeciwnym razie Pobierz [Sieć Web Narzędzia deweloperskie 2012,2 — Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282228).
5. Po wyświetleniu monitu kliknij przycisk Uruchom. Możesz również zapisać plik, aby uruchomić go później.

> [!NOTE]
> Wersja 2012,2 ASP.NET and Web Tools nie obejmuje SQL Server narzędzi danych. SQL Server i baza danych SQL platformy Microsoft Azure udostępniają bogatszy zestaw narzędzi do obsługi baz danych, w tym tworzenie projektów w trybie offline, porównywanie schematów i rozszerzanie możliwości wdrażania bazy danych. Aby uzyskać więcej informacji lub zainstalować narzędzia SQL Server Data Tools, odwiedź [https://go.microsoft.com/fwlink/?LinkID=237127](https://go.microsoft.com/fwlink/?LinkID=237127).

<a id="_Documentation"></a>
## <a name="documentation"></a>Dokumentacja

Samouczki i inne informacje o ASP.NET and Web Tools 2012,2 są dostępne w witrynie sieci Web ASP.NET (https://www.asp.net).

<a id="_Support"></a>
## <a name="support"></a>Pomoc techniczna

ASP.NET and Web Tools 2012,2 jest oficjalnie wydane i obsługiwane. Możesz użyć standardowego kanału pomocy technicznej. Możesz również ogłosić pytania na forach ASP.NET ([https://forums.asp.net/](https://forums.asp.net/)), w których członkowie społeczności ASP.NET są często w stanie zapewnić nieformalne wsparcie.

<a id="_Software_Requirements"></a>
## <a name="software-requirements"></a>Wymagania programowe

ASP.NET and Web Tools 2012,2 wymaga programu Visual Studio 2012 lub Visual Studio Express 2012 dla sieci Web.

<a id="_New_Features_in"></a>
## <a name="new-features-in-aspnet-and-web-tools-20122"></a>Nowe funkcje w ASP.NET and Web Tools 2012,2

W tej sekcji opisano funkcje, które zostały wprowadzone w wersji 2012,2 ASP.NET and Web Tools.

<a id="_Tooling"></a>
### <a name="tooling"></a>Narzędzia

- {1&gt;Inspektor strony&lt;1} 

    - Obsługa mapowania wyboru języka JavaScript, dzięki czemu Inspektor strony może mapować elementy, które zostały dynamicznie dodane do strony z powrotem do odpowiedniego kodu JavaScript.
    - Możliwość wyświetlania aktualizacji CSS w czasie rzeczywistym.
    - Aby uzyskać więcej informacji, przeczytaj temat [Mapowanie autosynchronizacji CSS i wybór JavaScript w Inspektorze strony](https://blogs.msdn.com/b/webdev/archive/2012/12/14/css-auto-sync-and-javascript-selection-mapping-in-page-inspector.aspx).
- Edytor 

    - Obsługa wyróżniania składni dla CoffeeScript, Mustache, kierownicy i JsRender.
    - Edytor HTML udostępnia funkcję IntelliSense dla powiązań odcinania.
    - MNIEJSZA Edycja i obsługa kompilatora, aby umożliwić tworzenie dynamicznych arkuszy CSS przy użyciu mniej.
    - Wklej kod JSON jako klasę .NET. Użycie tego specjalnego polecenia wklejania do wklejenia kodu JSON do C# pliku VB.NET lub programu Visual Studio spowoduje automatyczne wygenerowanie klas platformy .NET wywnioskowanych na podstawie JSON.
- Obsługa emulatorów mobilnych dodaje punkty zaczepienia rozszerzalności, dzięki czemu emulatory innych firm można instalować jako VSIX. Zainstalowane emulatory zostaną wyświetlone na liście rozwijanej F5, aby deweloperzy mogli wyświetlać podgląd swoich witryn sieci Web na różnych urządzeniach przenośnych. Przeczytaj więcej na temat tej funkcji w blogu Scott Hanselman na [nowej integracji BrowserStack z programem Visual Studio](http://www.hanselman.com/blog/CrossBrowserDebuggingIntegratedIntoVisualStudioWithBrowserStack.aspx).

<a id="_Web_Publishing"></a>
### <a name="web-publishing"></a>Publikowanie w sieci Web

- Projekty witryn sieci Web mają teraz takie samo środowisko publikowania jak projekty aplikacji sieci Web, w tym publikowanie w witrynach sieci Web systemu Windows Azure.
- Publikowanie &#8211; selektywne dla co najmniej jednego pliku można wykonać następujące akcje (po opublikowaniu w Web Deploy punkcie końcowym): 

    - Opublikuj wybrane pliki.
    - Zapoznaj się z różnicą między plikiem lokalnym a plikiem zdalnym.
    - Zaktualizuj plik lokalny za pomocą pliku zdalnego lub zaktualizuj plik zdalny przy użyciu pliku lokalnego.

<a id="_Templates"></a>
### <a name="aspnet-mvc-templates"></a>Szablony ASP.NET MVC

- Nowy szablon aplikacji usługi Facebook ułatwia pisanie aplikacji usługi Facebook. W kilku prostych krokach można utworzyć aplikację usługi Facebook, która uzyskuje dane od zalogowanego użytkownika i integruje się z jego znajomymi. Szablon zawiera nową bibliotekę automatyzującą wszystkie żmudne procesy związanie z tworzeniem aplikacji usługi Facebook, łącznie z uwierzytelnianiem, uprawnieniami i uzyskiwaniem dostępu do danych usługi Facebook. Aby uzyskać więcej informacji na temat korzystania z szablonu aplikacji Facebook, zobacz [https://go.microsoft.com/fwlink/?LinkID=269921](https://go.microsoft.com/fwlink/?LinkID=269921).
- Nowy szablon aplikacji jednostronicowej platformy MVC umożliwia deweloperom tworzenie interakcyjnych aplikacji internetowych po stronie klienta przy użyciu języków HTML 5 i CSS 3 oraz popularnych bibliotek Knockout i jQuery JavaScript w składniku Web API platformy ASP.NET. Szablon zawiera aplikację listy "do zrobienia", w której przedstawiono typowe rozwiązania dotyczące tworzenia aplikacji HTML5 języka JavaScript korzystającej z interfejsu API serwera RESTful. Więcej informacji można znaleźć pod adresem [https://www.asp.net/single-page-application](../single-page-application/index.md).
- Teraz można utworzyć VSIX, który dodaje nowe szablony do okna dialogowego Nowy projekt ASP.NET MVC. Dowiedz się, jak tutaj: [https://go.microsoft.com/fwlink/?LinkId=275019](https://go.microsoft.com/fwlink/?LinkId=275019)
- Szablony projektu &#8211; MVC pakietu FixedDisplayModes zostały zaktualizowane w taki sposób, aby obejmowały nowy pakiet NuGet "FixedDisplayModes", który zawiera obejście błędu w MVC 4. Aby uzyskać więcej informacji na temat poprawki zawartej w pakiecie, zapoznaj się z tym wpisem w blogu ([https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx)) z zespołu MVC.

<a id="_ASP.NET_Web_API"></a>
### <a name="aspnet-web-api"></a>Składnik Web API platformy ASP.NET

Interfejs API sieci Web ASP.NET został rozszerzony o kilka nowych funkcji:

- ASP.NET internetowego interfejsu API OData
- Śledzenie interfejsu API sieci Web ASP.NET
- Strona pomocy interfejsu API sieci Web ASP.NET

#### <a name="aspnet-web-api-odata"></a>ASP.NET internetowego interfejsu API OData

Usługa ASP.NET Web API OData zapewnia elastyczność potrzebną do tworzenia punktów końcowych OData z rozbudowaną logiką biznesową dla każdego źródła danych. Za pomocą usługi ASP.NET Web API OData kontrolujesz ilość semantyki OData, którą chcesz uwidocznić. Usługa ASP.NET Web API OData jest dołączona do szablonów projektów ASP.NET MVC 4 i jest również dostępna z narzędzia NuGet ([http://www.nuget.org/packages/microsoft.aspnet.webapi.odata](http://www.nuget.org/packages/microsoft.aspnet.webapi.odata)).

Usługa ASP.NET Web API OData obsługuje obecnie następujące funkcje:

- Włącz semantykę zapytań OData, stosując atrybut [Queryable].
- Łatwo Weryfikuj zapytania OData i ograniczaj zestaw obsługiwanych opcji zapytania, operatorów i funkcji.
- Powiązanie parametru z ODataQueryOptions bezpośrednio w celu uzyskania abstrakcyjnej postaci drzewa składni zapytania, które można następnie sprawdzić i zastosować do interfejsu IQueryable lub IEnumerable.
- Włącz stronicowanie oparte na usługach i generowanie linku następnej strony, określając limity wyników dla atrybutu [Queryable].
- Zażądaj nieliniowej liczby łącznej liczby pasujących zasobów przy użyciu $inlinecount.
- Sterowanie propagacją wartości null.
- Wszystkie operatory w $filter.
- Wywnioskowanie modelu danych jednostki według Konwencji lub jawne dostosowanie modelu w sposób podobny do kodu Entity Framework — jako pierwszy.
- Uwidocznij zestawy jednostek, wyprowadzając je z EntitySetController.
- Proste, dostosowywalne konwencje umożliwiające udostępnianie właściwości nawigacji, manipulowanie łączami i implementowanie działań OData.
- Uproszczone Routing przy użyciu metody rozszerzenia MapODataRoute.
- Obsługa wersji przez udostępnienie wielu modeli modelu EDM.
- Uwidocznij dokument usługi i $metadata, aby można było generować klientów (.NET, Windows Phone, Sklep Windows itp.) dla internetowego interfejsu API.
- Obsługa formatu OData Atom, JSON i JSON.
- Tworzenie, aktualizowanie, częściowe aktualizowanie (poprawka) i usuwanie jednostek.
- Wykonywanie zapytań i manipulowanie relacjami między jednostkami.
- Utwórz linki relacji łączące się z trasami.
- Typy złożone.
- Dziedziczenie typu jednostki.
- Właściwości kolekcji.
- Wyliczenia.
- Akcje OData.
- Oparta na tej samej podstawie co Usługi danych programu WCF, mianowicie ODataLib ([http://www.nuget.org/packages/microsoft.data.odata](http://www.nuget.org/packages/microsoft.data.odata)).

Aby uzyskać więcej informacji na temat usługi ASP.NET Web API OData, zobacz [https://go.microsoft.com/fwlink/?LinkId=271141](https://go.microsoft.com/fwlink/?LinkId=271141).

#### <a name="aspnet-web-api-tracing"></a>Śledzenie interfejsu API sieci Web ASP.NET

Śledzenie interfejsu API sieci Web ASP.NET integruje dane śledzenia z interfejsów API sieci Web przy użyciu śledzenia .NET. Jest teraz włączona domyślnie w szablonie projektu interfejsu API sieci Web. Dane śledzenia dla interfejsów API sieci Web są wysyłane do okna danych wyjściowych i udostępniane za pomocą IntelliTrace. Śledzenie interfejsu API sieci Web ASP.NET umożliwia śledzenie informacji o interfejsie API sieci Web, które są hostowane w systemie Windows Azure za pomocą integracji z [systemem windows Diagnostyka Azure](https://msdn.microsoft.com/library/windowsazure/hh411529.aspx). Możesz również zainstalować i włączyć śledzenie interfejsu API sieci Web ASP.NET w dowolnej aplikacji przy użyciu pakietu NuGet ASP.NET Web API śledzącego ([http://www.nuget.org/packages/microsoft.aspnet.webapi.tracing](http://www.nuget.org/packages/microsoft.aspnet.webapi.tracing)).

Aby uzyskać więcej informacji na temat konfigurowania i korzystania ze śledzenia interfejsu Web API ASP.NET, zobacz [https://go.microsoft.com/fwlink/?LinkID=269874](https://go.microsoft.com/fwlink/?LinkID=269874).

#### <a name="aspnet-web-api-help-page"></a>Strona pomocy interfejsu API sieci Web ASP.NET

Strona pomocy interfejsu API sieci Web ASP.NET jest teraz domyślnie dołączona do szablonu projektu interfejsu API sieci Web. Strona pomocy interfejsu API sieci Web ASP.NET automatycznie generuje dokumentację dla interfejsów API sieci Web, w tym punkty końcowe HTTP, obsługiwane metody HTTP, parametry i przykładowe żądania i komunikaty odpowiedzi. Dokumentacja jest automatycznie pobierana z komentarzy w kodzie. Stronę pomocy interfejsu API sieci Web ASP.NET można także dodać do dowolnej aplikacji przy użyciu pakietu NuGet strony pomocy interfejsu API sieci Web ASP.NET ([http://www.nuget.org/packages/microsoft.aspnet.webapi.helppage](http://www.nuget.org/packages/microsoft.aspnet.webapi.helppage)).

Aby uzyskać więcej informacji o konfigurowaniu i dostosowywaniu strony pomocy interfejsu API sieci Web ASP.NET, zobacz [https://go.microsoft.com/fwlink/?LinkId=271140](https://go.microsoft.com/fwlink/?LinkId=271140).

<a id="_ASP.NET_SignalR"></a>
### <a name="aspnet-signalr"></a>Biblioteka SignalR platformy ASP.NET

ASP.NET sygnalizujący ułatwia dodawanie funkcji sieci Web w czasie rzeczywistym do aplikacji ASP.NET przy użyciu funkcji WebSockets, jeśli są dostępne i automatycznie wraca do innych technik, gdy nie jest.

Aby uzyskać więcej informacji o korzystaniu z ASP.NET sygnalizującego, zobacz [https://go.microsoft.com/fwlink/?LinkId=271271](https://go.microsoft.com/fwlink/?LinkId=271271).

<a id="_ASP.NET_Friendly_URLs"></a>
### <a name="aspnet-friendly-urls"></a>Przyjazne adresy URL platformy ASP.NET

ASP.NET FriendlyURLs ułatwia deweloperom formularzy sieci Web generowanie bardziej czytelnych adresów URL (bez rozszerzenia. aspx). Nie wymaga to żadnej konfiguracji i mogą być używane z istniejącymi aplikacjami ASP.NET v 4.0. Funkcja FriendlyURLs ułatwia deweloperom Dodawanie do aplikacji obsługi urządzeń przenośnych przez obsługę przełączania między pulpitami i widokami mobilnymi.

Aby uzyskać więcej informacji na temat instalowania i używania przyjaznych adresów URL ASP.NET, zobacz [http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx](http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx).

<a id="_Known_Issues_and"></a>
## <a name="known-issues-and-breaking-changes"></a>Znane problemy i istotne zmiany

W tej sekcji opisano znane problemy i istotne zmiany, które znajdują się w wersji 2012,2 ASP.NET and Web Tools.

### <a name="installation-issues"></a>Problemy z instalacją

#### <a name="out-of-order-installs-of-visual-studio-2012"></a>Instalacje poza kolejnością programu Visual Studio 2012

Zainstalowanie dodatkowej jednostki SKU programu Visual Studio 2012 po zainstalowaniu ASP.NET and Web Tools 2012,2 będzie wymagało wykonania operacji naprawczej. Rozważ poniższą sekwencję:

1. Install Visual Studio 2012 Express for Web
2. Zainstaluj ASP.NET and Web Tools 2012,2
3. Zainstaluj program Visual Studio 2012 Professional, Premium lub Ultimate

Krok 2 spowoduje jedynie zainstalowanie aktualizacji programu Express for Web. Aby zapewnić, że dodatkowa jednostka SKU zainstalowana w kroku 3 zawiera aktualizację, należy naprawić ASP.NET and Web Tools 2012,2, aby zainstalować aktualizacje dla ostatniej zainstalowanej jednostki SKU. Dotyczy to również sytuacji, gdy jednostki SKU w krokach 1 i 3 zostaną cofnięte.

#### <a name="installing-microsoft-aspnet-and-web-tools-20122-when-visual-studio-is-open"></a>Instalowanie Microsoft ASP.NET and Web Tools 2012,2, gdy jest otwarty program Visual Studio

Jeśli program VS jest otwarty podczas instalacji Microsoft ASP.NET and Web Tools 2012,2, Visual Studio może zakończyć się nieprawidłowym stanem. Zaleca się, aby użytkownicy zamykali wszystkie wystąpienia programu Visual Studio przed kontynuowaniem instalacji.

#### <a name="canceling-aspnet-and-web-tools-20122-setup-in-the-middle-of-installation"></a>Anulowanie konfiguracji ASP.NET and Web Tools 2012,2 w trakcie instalacji

Anulowanie konfiguracji ASP.NET and Web Tools 2012,2 w trakcie instalacji pozostawi nieprawidłowy stan programu Visual Studio. Aby rozwiązać ten problem, wykonaj następujące kroki: 

- Przejdź do apletu Dodaj/Usuń programy
- Odinstaluj Microsoft ASP.NET and Web Tools 2012,2, jeśli istnieje.
- Zainstaluj ponownie Microsoft ASP.NET and Web Tools 2012,2

#### <a name="after-uninstalling-aspnet-and-web-tools-20122-the-aspnet-mvc-4-templates-and-razor-v2-web-site-templates-are-missing"></a>Po odinstalowaniu ASP.NET and Web Tools 2012,2 brakuje szablonów witryny sieci Web ASP.NET MVC 4 i Razor v2

Odinstalowanie ASP.NET and Web Tools 2012,2 spowoduje również odinstalowanie wszystkich szablonów witryn sieci Web ASP.NET MVC 4 i Razor v2 z programu Visual Studio 2012.

Obejście polega na naprawie instalacji programu Visual Studio 2012 w celu ponownego zainstalowania szablonów witryn sieci Web ASP.NET MVC 4 i Razor v2.

### <a name="tooling-issues"></a>Problemy dotyczące narzędzi

#### <a name="nuget-error-reported-during-project-creation"></a>Zgłoszono błąd NuGet podczas tworzenia projektu

Po zainstalowaniu ASP.NET and Web Tools 2012,2 podczas tworzenia projektu MVC 4 może zostać wyświetlony następujący błąd

![](aspnet-and-web-tools-20122-release-notes/_static/image1.png)

ASP.NET and Web Tools 2012,2 dostarcza pakiet NuGet 2,1 i uaktualni rozszerzenie w programie Visual Studio 2012. W niektórych przypadkach Instalator VSIX nie będzie poprawnie aktualizować VSIX. Poniższe kroki pozwolą rozwiązać ten problem:

1. Uruchom program Visual Studio 2012 jako administrator
2. Przejdź do pozycji narzędzia-&gt;rozszerzenia i aktualizacje i Odinstaluj pakiet NuGet.
3. Zamknij program Visual Studio
4. Przejdź do folderu instalacji ASP.NET and Web Tools 2012,2:

    1. Dla programu Visual Studio 2012: **Program Files\Microsoft ASP. NET\ASP.NET Web Stack\Visual Studio 2012**
    2. Dla programu Visual Studio 2012 Express for Web: **Program Files\Microsoft ASP. NET\ASP.NET Web Stack\Visual Studio Express 2012 for Web**
5. Kliknij dwukrotnie plik NuGet. Tools. vsix, aby ponownie zainstalować pakiet NuGet

### <a name="web-api-issues"></a>Problemy z interfejsem API sieci Web

#### <a name="parsing-issues-in-filter-and-datetime-literals"></a>Analizowanie problemów w $filter i literałach DateTime

Analizator identyfikatora URI OData nie może prawidłowo przeanalizować częściowych literałów DateTime. Na przykład $filter = Start EQ DateTime "2012-12-31T12:00" nie można prawidłowo przeanalizować. Obejście polega na użyciu pełnego literału, $filter = Start EQ DateTime "2012-12-31T12:00:00".

#### <a name="odata-doesnt-support-case-insensitive-property-names"></a>Usługa OData nie obsługuje nazw właściwości bez uwzględniania wielkości liter.

Usługa OData nie obsługuje nazw właściwości bez uwzględniania wielkości liter w zapytaniach OData i ścieżce OData. Zobacz elementy robocze:

- [http://aspnetwebstack.codeplex.com/workitem/366](http://aspnetwebstack.codeplex.com/workitem/366)
- [http://aspnetwebstack.codeplex.com/workitem/704](http://aspnetwebstack.codeplex.com/workitem/704)

Jeśli użytkownicy mają różne wielkości liter po stronie klienta i po stronie serwera w języku JavaScript, prawdopodobnie wystąpi ten problem. Ten problem jest zaprojektowany w protokole OData. Jednak wielu użytkowników zgłasza ten problem. Aby obejść ten sposób, użytkownicy muszą skorygować ich przypadki w adres URL.

#### <a name="default-odata-routing-conventions-doesnt-support-postput-on-navigation-property"></a>Domyślne konwencje routingu OData nie obsługują właściwości POST/PUT dla nawigacji.

Domyślne konwencje routingu OData nie obsługują właściwości POST/PUT dla nawigacji. Zobacz [http://aspnetwebstack.codeplex.com/workitem/366](http://aspnetwebstack.codeplex.com/workitem/366)elementu roboczego. Brak tej powszechnie używanej Konwencji w konwencjach domyślnych.

Aby obejść ten sposób, użytkownicy muszą rozciągnąć nową Konwencję routingu w celu jej obsługi.

### <a name="facebook-template-issues"></a>Problemy dotyczące szablonów w usłudze Facebook

#### <a name="facebook-application-template-only-works-using-net-45"></a>Szablon aplikacji Facebook działa tylko z platformą .NET 4,5

Musisz wybrać pozycję .NET 4,5 na liście rozwijanej struktury w oknie dialogowym Nowy projekt, aby wyświetlić szablon aplikacji Facebook w ASP.NET MVC 4.

#### <a name="real-time-update-controller"></a>Kontroler aktualizacji w czasie rzeczywistym

Szablon aplikacji w serwisie Facebook umożliwia użytkownikom łatwe tworzenie kontrolera internetowego interfejsu API do obsługi aktualizacji w czasie rzeczywistym z serwisu Facebook. Jeśli komputer programistyczny znajduje się za translatorem adresów sieciowych, kontroler może nie działa bez dalszej konfiguracji sieci. Zobacz tutaj, aby uzyskać szczegółowe informacje: [http://facebook.stackoverflow.com/questions/5259467/can-a-computer-behind-a-nat-router-receive-realtime-updates-from-facebook](http://facebook.stackoverflow.com/questions/5259467/can-a-computer-behind-a-nat-router-receive-realtime-updates-from-facebook)

#### <a name="query-string-values-conflict-with-facebook-oauth-parameters"></a>Wartości ciągu zapytania powodują konflikt z parametrami OAuth protokołu Facebook

Następujące pola powodują konflikt z zwrotnym adresem URL wywołania w serwisie Facebook. Nie należy dodawać własnych wartości ciągu zapytania z następującymi nazwami: Code, Error, Error\_Description, Error\_przyczyn.

#### <a name="using-page-inspector-with-facebook-template"></a>Korzystanie z narzędzia Page Inspector z szablonem Facebook

Nie można używać funkcji Page Inspector w programie Visual Studio 2012 podczas debugowania aplikacji w serwisie Facebook. Inspektor strony nie obsługuje obecnie elementów iframe.

### <a name="single-page-application-template-issues"></a>Problemy związane z szablonem aplikacji jednostronicowej

#### <a name="with-jquery-19knockout-221-update-when-running-default-mvc-spa-project-new-todo-item-edit-enter-focus-event-is-not-handled-properly"></a>W przypadku aktualizacji JQuery 1.9/odcinania 2.2.1, gdy uruchomiony jest domyślny, nowy element do wykonania Edytuj wydarzenie fokusu nie jest prawidłowo obsługiwane.

W przypadku aktualizacji JQuery 1.9/odcinania 2.2.1, gdy uruchamiany jest domyślny projekt MVC SPA, nowy element do wykonania Edytuj nie jest już fokusem z powrotem do pola edycji nowego elementu do wykonania po wprowadzeniu nowego elementu do wykonania na listę zadań do zrobienia.

Aby obejść [http://knockoutjs.com/documentation/hasfocus-binding.html](http://knockoutjs.com/documentation/hasfocus-binding.html)odwołanie i wprowadzić podobną poprawkę do następującego przykładowego kodu:

Plik do zrobienia. model. js  
 Funkcja todolist (dane), Dodaj następujące elementy:  
 **Auto. isselectd = ko. dostrzegalne (false);**

Funkcja todoList. prototype. adddo zrobienia, Dodaj następujący tekst w kolorze czarnym:  
 **Auto. isselectd (true);**  
 sam. newTodoTitle (&quot;&quot;);

Plik index. cshtml, Dodaj następujący tekst:  
 dane formularza &lt;— bind =&quot;Prześlij: adddo&quot;&gt;  
 &lt;Input Class =&quot;Add&quot; typ =&quot;Text&quot; dane-bind =&quot;wartość: newTodoTitle, symbol zastępczy: "Wpisz tutaj, aby dodać", blurOnEnter: true, **hasFocus: Isselectd**, Event: {Blur: adddo}&quot; /&gt;  
 &lt;formularzy&gt;

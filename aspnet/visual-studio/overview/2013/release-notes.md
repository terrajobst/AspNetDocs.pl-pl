---
uid: visual-studio/overview/2013/release-notes
title: ASP.NET and Web Tools do Visual Studio 2013 informacji o wersji | Microsoft Docs
author: microsoft
description: W tym dokumencie opisano wydanie ASP.NET and Web Tools Visual Studio 2013.
ms.author: riande
ms.date: 10/17/2013
ms.assetid: 08815768-2702-42ae-ae85-0a59934a11d1
msc.legacyurl: /visual-studio/overview/2013/release-notes
msc.type: authoredcontent
ms.openlocfilehash: d8af9c8e7ee1316a5eac90c5959d07c628154e09
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600436"
---
# <a name="aspnet-and-web-tools-for-visual-studio-2013-release-notes"></a>Rozszerzenie ASP.NET and Web Tools dla programu Visual Studio 2013 — informacje o wersji

przez [firmę Microsoft](https://github.com/microsoft)

> W tym dokumencie opisano wydanie ASP.NET and Web Tools Visual Studio 2013.

## <a name="contents"></a>Spis treści

- [Uwagi dotyczące instalacji](#TOC1)
- [Dokumentacja](#TOC2)
- [Wymagania dotyczące oprogramowania](#TOC4)

### <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-2013"></a>Nowe funkcje w ASP.NET and Web Tools Visual Studio 2013

- [Jeden ASP.NET](#TOC6)
- [Nowe środowisko projektu sieci Web](#newproj)
- [Tworzenie szkieletu ASP.NET](#scaffold)
- [Łączność z przeglądarkami](#browser-link)
- [Udoskonalenia edytora sieci Web programu Visual Studio](#web-editor)
- [Obsługa Web Apps Azure App Service w programie Visual Studio](#waws)
- [Udoskonalenia publikowania w sieci Web](#publish)
- [NuGet 2.7](#nuget)
- [ASP.NET Web Forms](#TOC9)
- [ASP.NET MVC 5](#TOC10)
- [ASP.NET Web API 2](#TOC11)
- [ASP.NET sygnalizujący](#TOC13)
- [ASP.NET Identity](#TOC8)
- [Składniki OWIN firmy Microsoft](#TOC7)
- [Entity Framework 6](#ef6)
- [ASP.NET Razor 3](#TOC14)
- [Wstrzymywanie aplikacji ASP.NET](#TOC15)
- [Znane problemy i istotne zmiany](#knownissues)

<a id="TOC1"></a>
## <a name="installation-notes"></a>Uwagi dotyczące instalacji

ASP.NET and Web Tools dla Visual Studio 2013 są powiązane z instalatorem głównym i można je pobrać w [tym miejscu](https://www.asp.net/downloads).

<a id="TOC2"></a>
## <a name="documentation"></a>Dokumentacja

Samouczki i inne informacje dotyczące ASP.NET and Web Tools dla Visual Studio 2013 są dostępne w [witrynie sieci Web ASP.NET](https://www.asp.net/).

<a id="TOC4"></a>
## <a name="software-requirements"></a>Wymagania programowe

ASP.NET and Web Tools wymaga Visual Studio 2013.

<a id="TOC5"></a>
## <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-2013"></a>Nowe funkcje w ASP.NET and Web Tools Visual Studio 2013

W poniższych sekcjach opisano funkcje wprowadzone w wersji.

<a id="TOC6"></a>
## <a name="one-aspnet"></a>Jeden ASP.NET

Dzięki wykorzystaniu Visual Studio 2013 podjąłmy krok w kierunku ujednolicenia doświadczenia związanego z korzystaniem z technologii ASP.NET, dzięki czemu możesz łatwo mieszać i dopasowywać te, które chcesz. Na przykład można rozpocząć projekt przy użyciu MVC i łatwo dodać strony formularzy sieci Web do projektu później lub utworzyć szkielet sieci Web API w projekcie formularzy sieci Web. Jednym z ASP.NET jest to, aby ułatwić deweloperom tworzenie rzeczy, które lubisz w ASP.NET. Niezależnie od tego, którą technologię wybierasz, możesz mieć pewność, że tworzysz w oparciu o zaufaną podstawową strukturę jednego ASP.NET.

<a id="newproj"></a>
## <a name="new-web-project-experience"></a>Nowe środowisko projektu sieci Web

Ulepszono środowisko tworzenia nowych projektów sieci Web w Visual Studio 2013. W oknie dialogowym **Nowy projekt sieci Web ASP.NET** możesz wybrać odpowiedni typ projektu, skonfigurować dowolną kombinację technologii (formularzy sieci Web, MVC, Web API), skonfigurować opcje uwierzytelniania i dodać projekt testu jednostkowego.

![Nowy projekt ASP.NET](release-notes/_static/image1.png)

Nowe okno dialogowe umożliwia zmianę domyślnych opcji uwierzytelniania dla wielu szablonów. Na przykład podczas tworzenia projektu formularzy sieci Web ASP.NET można wybrać jedną z następujących opcji:

- Bez uwierzytelniania
- Indywidualne konta użytkowników (ASP.NET członkostwo lub logowanie dostawcy społecznego)
- Konta organizacji (Active Directory w aplikacji internetowej)
- Uwierzytelnianie systemu Windows (Active Directory w aplikacji intranetowej)

![Opcje uwierzytelniania](release-notes/_static/image2.png)

Aby uzyskać więcej informacji na temat nowego procesu tworzenia projektów sieci Web, zobacz [Tworzenie projektów sieci web ASP.NET w Visual Studio 2013](creating-web-projects-in-visual-studio.md). Aby uzyskać więcej informacji na temat nowych opcji uwierzytelniania, zobacz [ASP.NET Identity](#TOC8) w dalszej części tego dokumentu.

<a id="scaffold"></a>
## <a name="aspnet-scaffolding"></a>Tworzenie szkieletu ASP.NET

Tworzenie szkieletów ASP.NET jest strukturą generowania kodu dla aplikacji sieci Web ASP.NET. Ułatwia to dodawanie kodu standardowego do projektu, który współdziała z modelem danych.

W poprzednich wersjach programu Visual Studio funkcja tworzenia szkieletów została ograniczona do projektów ASP.NET MVC. Za pomocą Visual Studio 2013 można teraz używać szkieletów dla dowolnego projektu ASP.NET, w tym formularzy sieci Web. Visual Studio 2013 nie obsługuje obecnie generowania stron dla projektu formularzy sieci Web, ale nadal można używać szkieletów z formularzami sieci Web, dodając zależności MVC do projektu. Obsługa generowania stron dla formularzy sieci Web zostanie dodana w przyszłej aktualizacji.

W przypadku korzystania z szkieletów upewnij się, że wszystkie wymagane zależności są zainstalowane w projekcie. Na przykład, jeśli zaczniesz od projektu ASP.NET Web Forms, a następnie użyjesz szkieletu do dodania kontrolera interfejsu API sieci Web, wymagane pakiety i odwołania NuGet są dodawane do projektu automatycznie.

Aby dodać szkielet MVC do projektu formularzy sieci Web, Dodaj **nowy element szkieletowy** i wybierz **zależności MVC 5** w oknie dialogowym. Dostępne są dwie opcje tworzenia szkieletów MVC; Minimalny i pełny. W przypadku wybrania opcji minimalny tylko pakiety NuGet i odwołania dla ASP.NET MVC zostaną dodane do projektu. W przypadku wybrania opcji Pełna są dodawane minimalne zależności, a także wymagane pliki zawartości dla projektu MVC.

Obsługa tworzenia szkieletów kontrolerów asynchronicznych używa nowych funkcji asynchronicznych programu Entity Framework 6.

Aby uzyskać więcej informacji i samouczków, zobacz [Omówienie tworzenia szkieletów ASP.NET](aspnet-scaffolding-overview.md).

<a id="browser-link"></a>
## <a name="browser-link--signalr-channel-between-browser-and-visual-studio"></a>Link do przeglądarki — kanał sygnalizujący między przeglądarką a programem Visual Studio

Nowa funkcja [linku do przeglądarki](using-browser-link.md) umożliwia łączenie wielu przeglądarek z programem Visual Studio i odświeżanie ich wszystkich przez kliknięcie przycisku na pasku narzędzi. Możesz połączyć wiele przeglądarek z witryną programistyczną, w tym emulatorów mobilnych, a następnie kliknąć przycisk Odśwież, aby odświeżyć wszystkie przeglądarki w tym samym czasie. Link do przeglądarki udostępnia również interfejs API umożliwiający deweloperom pisanie rozszerzeń linków przeglądarki.

![](release-notes/_static/image3.png)

Dzięki umożliwieniu deweloperom korzystania z interfejsu API linków przeglądarki można utworzyć bardzo zaawansowane scenariusze, które przecinają granice między programem Visual Studio i dołączoną przeglądarką. Dzięki interfejsowi API usługi Web Essentials można utworzyć zintegrowane środowisko między programem Visual Studio i narzędziami deweloperskimi przeglądarki, zdalnie kontrolującymi emulatory mobilne i wiele innych.

<a id="web-editor"></a>
## <a name="visual-studio-web-editor-enhancements"></a>Udoskonalenia edytora sieci Web programu Visual Studio

Visual Studio 2013 obejmuje nowy edytor HTML dla plików Razor i plików HTML w aplikacjach sieci Web. Nowy edytor HTML zapewnia jednolity ujednolicony schemat oparty na języku HTML5. Jest to automatyczne uzupełnianie nawiasów klamrowych, interfejs użytkownika jQuery i funkcja IntelliSense atrybutów AngularJS, grupowanie funkcji IntelliSense atrybutów, identyfikator i nazwa klasy IntelliSense oraz inne udoskonalenia, w tym lepsza wydajność, formatowanie i SmartTags.

Poniższy zrzut ekranu ilustruje użycie funkcji IntelliSense atrybutów Bootstrap w edytorze HTML.

![IntelliSense w edytorze HTML](release-notes/_static/image4.png)

Visual Studio 2013 jest również z wbudowanymi edytorami CoffeeScript i LESS. Edytor LESS zawiera wszystkie chłodne funkcje z edytora CSS i ma konkretną funkcję IntelliSense dla zmiennych i domieszki na wszystkich mniej dokumentów w łańcuchu @import.

<a id="waws"></a>
## <a name="azure-app-service-web-apps-support-in-visual-studio"></a>Obsługa Web Apps Azure App Service w programie Visual Studio

W Visual Studio 2013 z zestawem Azure SDK dla programu .NET 2,2 można używać **Eksplorator serwera** do bezpośredniej współpracy ze zdalnymi aplikacjami sieci Web. Możesz zalogować się do konta platformy Azure, utworzyć nowe aplikacje sieci Web, skonfigurować aplikacje, wyświetlać dzienniki w czasie rzeczywistym i nie tylko. Wkrótce po udostępnieniu zestawu SDK 2,2 będzie można zdalnie uruchomić program w trybie debugowania na platformie Azure. Większość nowych funkcji Azure App Service Web Apps również działały w programie Visual Studio 2012 po zainstalowaniu bieżącej wersji zestawu Azure SDK dla platformy .NET.

Więcej informacji można znaleźć w następujących zasobach:

- [Tworzenie aplikacji sieci Web ASP.NET w Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/)
- [Rozwiązywanie problemów z aplikacją sieci Web w Azure App Service przy użyciu programu Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/)

<a id="publish"></a>
## <a name="web-publish-enhancements"></a>Udoskonalenia publikowania w sieci Web

Visual Studio 2013 obejmuje nowe i udoskonalone funkcje publikowania w sieci Web. Oto kilka z nich:

- Łatwa [Automatyzacja szyfrowania plików Web. config](https://go.microsoft.com/fwlink/?LinkId=325529). (Ten link i dwa następujące punkty dotyczą dokumentacji w witrynie MSDN, które mogą nie być dostępne do końca dnia w dniu 10/17).
- Łatwe [Automatyzowanie przełączania aplikacji w tryb offline podczas wdrażania](https://go.microsoft.com/fwlink/?LinkId=325530).
- Skonfiguruj Web Deploy, aby [użyć sumy kontrolnej plików zamiast daty ostatniej zmiany](https://go.microsoft.com/fwlink/?LinkId=325531) , aby określić, które pliki mają być kopiowane na serwer.
- Szybkie publikowanie pojedynczych wybranych plików (w tym Web. config) w przypadku używania metod publikowania FTP lub systemu plików, jak również z Web Deploy.

Więcej informacji o wdrażaniu w sieci Web ASP.NET można znaleźć [w witrynie ASP.NET](https://go.microsoft.com/fwlink/?LinkId=322027).

<a id="nuget"></a>
## <a name="nuget-27"></a>NuGet 2.7

Pakiet NuGet 2,7 zawiera rozbudowany zestaw nowych funkcji, które są szczegółowo opisane w [informacjach o wersji programu nuget 2,7](http://docs.nuget.org/docs/release-notes/nuget-2.7).

Ta wersja programu NuGet również eliminuje konieczność zapewnienia jawnej zgody na pobieranie pakietów przez funkcję przywracania pakietów NuGet. Poprawność (i skojarzone pole wyboru w oknie dialogowym preferencji narzędzia NuGet) jest teraz udzielane przez zainstalowanie programu NuGet. Teraz przywracanie pakietu jest domyślnie obsługiwane.

<a id="TOC9"></a>
## <a name="aspnet-web-forms"></a>Formularze sieci Web ASP.NET

### <a name="one-aspnet"></a>Jeden ASP.NET

Szablony projektów formularzy sieci Web są bezproblemowo integrowane z nowym jednym ASP.NET doświadczeniem. Można dodać obsługę MVC i interfejsu API sieci Web do projektu formularzy sieci Web, a uwierzytelnianie można skonfigurować przy użyciu jednego Kreatora tworzenia projektu ASP.NET. Aby uzyskać więcej informacji, zobacz [Tworzenie projektów sieci Web ASP.NET w Visual Studio 2013](creating-web-projects-in-visual-studio.md).

### <a name="aspnet-identity"></a>ASP.NET Identity

Szablony projektów formularzy sieci Web obsługują nową strukturę ASP.NET Identity. Ponadto szablony obsługują teraz Tworzenie projektu intranetu formularzy sieci Web. Aby uzyskać więcej informacji, zobacz [metody uwierzytelniania](creating-web-projects-in-visual-studio.md#auth) w temacie **Tworzenie projektów sieci Web ASP.NET w Visual Studio 2013**.

### <a name="bootstrap"></a>Bootstrap

Szablony formularzy sieci Web używają [ładowania początkowego](http://twitter.github.io/bootstrap/) , aby zapewnić elegancki i odpowiadający wygląd i wygodę, którą można łatwo dostosować. Aby uzyskać więcej informacji, zobacz [Bootstrap w szablonach projektu sieci web Visual Studio 2013](creating-web-projects-in-visual-studio.md#bootstrap).

<a id="TOC10"></a>
## <a name="aspnet-mvc-5"></a>ASP.NET MVC 5

### <a name="one-aspnet"></a>Jeden ASP.NET

Szablony projektów Web MVC są bezproblemowo integrowane z nowym jednym ASP.NET doświadczeniem. Możesz dostosować projekt MVC i skonfigurować uwierzytelnianie przy użyciu jednego Kreatora tworzenia projektu ASP.NET. Samouczek wprowadzający do ASP.NET MVC 5 można znaleźć w [wprowadzenie z ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).

Aby uzyskać informacje na temat uaktualniania projektów MVC 4 do MVC 5, zobacz [jak uaktualnić projekt ASP.NET MVC 4 i interfejs API sieci Web do ASP.NET MVC 5 i Web API 2](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).

### <a name="aspnet-identity"></a>ASP.NET Identity

Szablony projektu MVC zostały zaktualizowane, aby można było używać ASP.NET Identity do uwierzytelniania i zarządzania tożsamościami. Samouczek dotyczący usługi Facebook i usługi Google Authentication oraz nowy interfejs API członkostwa można znaleźć w witrynie [tworzenia aplikacji ASP.NET MVC 5 z usługami Facebook i Google OAuth2 i OpenID Connect](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) , a także [utworzyć aplikację ASP.NET MVC z UWIERZYTELNIANIEM i bazą danych SQL, a następnie wdrożyć ją w Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/).

### <a name="bootstrap"></a>Bootstrap

Szablon projektu MVC został zaktualizowany tak, aby można było używać [ładowania początkowego](http://getbootstrap.com/) , aby zapewnić elegancki i odpowiadający wygląd i wygodę, aby móc je łatwo dostosować. Aby uzyskać więcej informacji, zobacz [Bootstrap w szablonach projektu sieci web Visual Studio 2013](creating-web-projects-in-visual-studio.md#bootstrap).

### <a name="authentication-filters"></a>Filtry uwierzytelniania

Filtry uwierzytelniania są nowym rodzajem filtrów w ASP.NET MVC, które są uruchamiane przed filtrami autoryzacji w potoku ASP.NET MVC i umożliwiają określenie logiki uwierzytelniania dla każdej akcji, na kontroler lub globalnie dla wszystkich kontrolerów. Filtry uwierzytelniania przetwarzają poświadczenia w żądaniu i dostarczają odpowiednie podmioty zabezpieczeń. Filtry uwierzytelniania mogą także dodawać wyzwania związane z uwierzytelnianiem w odpowiedzi na nieautoryzowane żądania.

### <a name="filter-overrides"></a>Filtry przesłonięć

Teraz można przesłonić filtry stosowane do danej metody lub kontrolera akcji przez określenie filtru przesłonięcia. Filtry zastąpień określają zestaw typów filtrów, które nie powinny być uruchamiane dla danego zakresu (Akcja lub kontroler). Pozwala to na skonfigurowanie filtrów, które są stosowane globalnie, a następnie wykluczenie niektórych filtrów globalnych z zastosowania do określonych akcji lub kontrolerów.

### <a name="attribute-routing"></a>Routing atrybutów

ASP.NET MVC obsługuje teraz Routing atrybutów, dzięki czemu autor [http://attributerouting.net](http://attributerouting.net)ma udział w Tim McCall. Za pomocą routingu atrybutu można określić trasy, dodając adnotacje do działań i kontrolerów.

<a id="TOC11"></a>
## <a name="aspnet-web-api-2"></a>ASP.NET Web API 2

### <a name="attribute-routing"></a>Routing atrybutów

Interfejs API sieci Web ASP.NET obsługuje teraz Routing atrybutów, dzięki czemu autor [http://attributerouting.net](http://attributerouting.net)ma udział w Tim McCall. Za pomocą routingu atrybutu można określić trasy interfejsu API sieci Web, dodając adnotacje do działań i kontrolerów w następujący sposób:

[!code-csharp[Main](release-notes/samples/sample1.cs)]

Routing atrybutu daje większą kontrolę nad identyfikatorami URI w interfejsie API sieci Web. Można na przykład łatwo zdefiniować hierarchię zasobów przy użyciu jednego kontrolera interfejsu API:

[!code-csharp[Main](release-notes/samples/sample2.cs)]

Routing atrybutu udostępnia również wygodną składnię określającą parametry opcjonalne, wartości domyślne i ograniczenia trasy:

[!code-csharp[Main](release-notes/samples/sample3.cs)]

Aby uzyskać więcej informacji na temat routingu atrybutów, zobacz [Routing atrybutów w interfejsie Web API 2](../../../web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2.md).

### <a name="oauth-20"></a>OAuth 2,0

Szablony projektów internetowego interfejsu API i aplikacji jednostronicowych obsługują teraz autoryzację przy użyciu protokołu OAuth 2,0. Uwierzytelnianie OAuth 2,0 to platforma umożliwiająca autoryzowanie dostępu klientów do chronionych zasobów. Działa ona w przypadku wielu klientów, w tym przeglądarek i urządzeń przenośnych.

Obsługa protokołu OAuth 2,0 jest oparta na nowym oprogramowaniu pośredniczącym zabezpieczeń udostępnianym przez składniki OWIN firmy Microsoft do uwierzytelniania okaziciela i implementowania roli serwera autoryzacji. Alternatywnie klienci mogą być autoryzowani przy użyciu serwera autoryzacji organizacji, takiego jak Azure Active Directory lub ADFS w systemie Windows Server 2012 R2.

### <a name="odata-improvements"></a>Udoskonalenia OData

**Obsługa $select, $expand, $batch i $value**

Usługa ASP.NET Web API OData ma teraz pełną obsługę $select, $expand i $value. Można również użyć $batch do tworzenia wsadowych żądań i przetwarzania zestawów zmian.

Opcje $select i $expand umożliwiają zmianę kształtu danych zwracanych z punktu końcowego OData. Aby uzyskać więcej informacji, zobacz [wprowadzenie $SELECT i $expand obsługi w protokole OData interfejsu API sieci Web](../../../web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value.md).

**Ulepszone rozszerzalność**

Elementy formatujące OData są teraz rozszerzalne. Można dodawać metadane wpisu Atom, obsługiwać nazwanych wpisów strumienia i multimediów, dodawać adnotacje wystąpień i dostosowywać sposób generowania linków.

**Obsługa typu less**

Teraz można tworzyć usługi OData bez konieczności definiowania typów CLR dla typów jednostek. Zamiast tego kontrolery OData mogą przyjmować lub zwracać wystąpienia **IEdmObject**, które są deserializacji/deserializacji OData.

**Ponowne użycie istniejącego modelu**

Jeśli masz już istniejący model danych jednostki (EDM), możesz teraz ponownie użyć go bezpośrednio, zamiast tworzyć nowe. Na przykład jeśli używasz Entity Framework, możesz użyć modelu EDM, który kompiluje dla Ciebie.

### <a name="request-batching"></a>Przetwarzanie wsadowe żądań

Żądania wsadowe polegają na łączeniu wielu operacji w pojedyncze żądanie HTTP POST, aby zmniejszyć ruch sieciowy i zapewnić gładszy, mniej rozmawiający interfejs użytkownika. Interfejs API sieci Web ASP.NET obsługuje teraz kilka strategii tworzenia wsadowych żądań:

- Użyj punktu końcowego $batch usługi OData.
- Pakowanie wielu żądań w jedno żądanie MIME wieloczęściowego.
- Użyj niestandardowego formatu wsadowego.

Aby włączyć przetwarzanie wsadowe żądań, wystarczy dodać trasę z obsługą wsadową do konfiguracji internetowego interfejsu API:

[!code-csharp[Main](release-notes/samples/sample4.cs)]

Można również kontrolować, czy żądania mają być wykonywane sekwencyjnie, czy też w dowolnej kolejności.

### <a name="portable-aspnet-web-api-client"></a>Klient przenośnego interfejsu API sieci Web ASP.NET

Za pomocą klienta interfejsu API sieci Web ASP.NET można teraz tworzyć przenośne biblioteki klas, które działają w Sklepie Windows i Windows Phone 8 aplikacji. Można również tworzyć przenośne elementy formatujące, które mogą być współużytkowane przez klienta i serwer.

### <a name="improved-testability"></a>Ulepszona możliwości testowania

Interfejs API sieci Web 2 ułatwia testowanie jednostkowe kontrolerów interfejsu API. Po prostu Utwórz wystąpienie kontrolera API przy użyciu komunikatu żądania i konfiguracji, a następnie Wywołaj metodę akcji, która ma zostać przetestowana. Można również łatwo zasymulować klasę **UrlHelper** w przypadku metod akcji, które wykonują generowanie linków.

### <a name="ihttpactionresult"></a>IHttpActionResult

Teraz można zaimplementować IHttpActionResult, aby hermetyzować wynik metod akcji interfejsu API sieci Web. IHttpActionResult zwrócony z metody akcji interfejsu API sieci Web jest wykonywany przez środowisko uruchomieniowe internetowego interfejsu API ASP.NET w celu utworzenia wynikowego komunikatu odpowiedzi. IHttpActionResult można zwrócić z dowolnej akcji internetowego interfejsu API, aby uprościć testy jednostkowe implementacji interfejsu API sieci Web. Dla wygody są dostępne pewne implementacje IHttpActionResult, w tym wyniki dotyczące zwracania określonych kodów stanu, sformatowanej zawartości lub odpowiedzi negocjowanych przez zawartość.

### <a name="httprequestcontext"></a>HttpRequestContext

Nowy **HttpRequestContext** śledzi każdy stan, który jest powiązany z żądaniem, ale nie jest natychmiast dostępny w żądaniu. Na przykład można użyć **HttpRequestContext** , aby uzyskać dane trasy, podmiot zabezpieczeń skojarzony z żądaniem, certyfikat klienta, **UrlHelper** i katalog główny ścieżki wirtualnej. Można łatwo utworzyć **HttpRequestContext** do celów testowania jednostkowego.

Ponieważ podmiot zabezpieczeń żądania jest przepływem żądania, a nie polega na **wątku. CurrentPrincipal**, podmiot zabezpieczeń jest teraz dostępny przez cały okres istnienia żądania, gdy znajduje się w potoku interfejsu API sieci Web.

### <a name="cors"></a>SPECYFIKACJI

Dzięki innemu doskonałemu wpływowi z usługi Brock, ASP.NET teraz w pełni obsługuje udostępnianie żądania krzyżowego (CORS).

Zabezpieczenia przeglądarki uniemożliwiają stronom sieci Web wykonywanie żądań AJAX do innej domeny. [CORS](http://www.w3.org/TR/cors/) jest standardem W3C, który umożliwia serwerowi złagodzenie zasad tego samego źródła. Przy użyciu mechanizmu CORS serwer może jawnie zezwolić na niektóre żądania między źródłami podczas odrzucania innych.

Interfejs Web API 2 obsługuje teraz mechanizm CORS, w tym automatyczną obsługę żądań inspekcji wstępnej. Aby uzyskać więcej informacji, zobacz [Włączanie żądań między źródłami w interfejsie API sieci Web ASP.NET](../../../web-api/overview/security/enabling-cross-origin-requests-in-web-api.md).

### <a name="authentication-filters"></a>Filtry uwierzytelniania

Filtry uwierzytelniania są nowym rodzajem filtrów w interfejsie API sieci Web ASP.NET, które są uruchamiane przed filtrami autoryzacji w potoku interfejsu API sieci Web ASP.NET i umożliwiają określenie logiki uwierzytelniania dla każdej akcji, na kontroler lub globalnie dla wszystkich kontrolerów. Filtry uwierzytelniania przetwarzają poświadczenia w żądaniu i dostarczają odpowiednie podmioty zabezpieczeń. Filtry uwierzytelniania mogą także dodawać wyzwania związane z uwierzytelnianiem w odpowiedzi na nieautoryzowane żądania.

### <a name="filter-overrides"></a>Filtry przesłonięć

Teraz można przesłonić filtry stosowane do danej metody lub kontrolera akcji, określając filtr przesłonięcia. Filtry zastąpień określają zestaw typów filtrów, które nie powinny być uruchamiane dla danego zakresu (Akcja lub kontroler). Pozwala to dodać filtry globalne, ale następnie wykluczyć niektóre z określonych akcji lub kontrolerów.

### <a name="owin-integration"></a>Integracja OWIN

Interfejs API sieci Web ASP.NET teraz w pełni obsługuje OWIN i może być uruchamiany na dowolnym hoście obsługującym OWIN. Uwzględniono również **HostAuthenticationFilter** , który zapewnia integrację z systemem uwierzytelniania Owin.

Dzięki integracji z usługą OWIN można samodzielnie hostować internetowy interfejs API we własnym procesie wraz z innymi firmowymi narzędziami OWIN, takimi jak sygnalizujący. Aby uzyskać więcej informacji, zobacz [Używanie Owin do samoobsługowego hostowania interfejsu API sieci Web ASP.NET](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

<a id="TOC13"></a>
## <a name="aspnet-signalr-20"></a>ASP.NET sygnalizujący 2,0

W poniższych sekcjach opisano funkcje sygnalizujące 2,0.

- [Oparta na OWIN](#builtonowin)
- [MapHubs i MapConnection są teraz MapSignalR](#MapSignalR)
- [Obsługa wielu domen](#crossdomain)
- [wsparcie dla systemów iOS i Android za pośrednictwem funkcji wielodotykowych i plik monodroid](#mobile)
- [Przenośny klient platformy .NET](#portable)
- [Nowy pakiet z własnym hostem](#selfhost)
- [Obsługa serwera zgodna z poprzednimi wersjami](#backwardcompat)
- [Usunięto obsługę serwera dla programu .NET 4,0](#remove40)
- [Wysyłanie komunikatu do listy klientów i grup](#messagelist)
- [Wysyłanie komunikatu do określonego użytkownika](#sendtouser)
- [Lepsza obsługa błędów](#errorhandling)
- [Łatwiejsze testowanie jednostkowe centrów](#unittesting)
- [Obsługa błędów języka JavaScript](#javascripterror)

Aby zapoznać się z przykładem uaktualniania istniejącego projektu 1. x do usługi Sygnalizującer 2,0, zobacz [uaktualnianie projektu sygnalizującego 1. x](../../../signalr/overview/releases/upgrading-signalr-1x-projects-to-20.md).

<a id="builtonowin"></a>
### <a name="built-on-owin"></a>Oparta na OWIN

Sygnał 2,0 jest całkowicie zbudowany w [Owin (otwarty interfejs sieci Web dla platformy .NET)](http://owin.org/). Ta zmiana powoduje, że proces instalacji dla sygnalizującego jest znacznie bardziej spójny między aplikacjami sieci Web i samoobsługowym programem sygnalizującym, ale wymagało również wielu zmian interfejsu API.

<a id="MapSignalR"></a>

### <a name="maphubs-and-mapconnection-are-now-mapsignalr"></a>MapHubs i MapConnection są teraz MapSignalR

W celu zapewnienia zgodności ze standardami OWIN te metody zostały zmienione na `MapSignalR`. `MapSignalR` wywołana bez parametrów będzie mapować wszystkie centra (jako `MapHubs` w wersji 1. x); Aby zmapować poszczególne obiekty **PersistentConnection** , określ typ połączenia jako parametr typu oraz rozszerzenie adresu URL dla połączenia jako pierwszy argument.

Metoda `MapSignalR` jest wywoływana w klasie uruchomieniowej Owin. Visual Studio 2013 zawiera nowy szablon klasy uruchomieniowej Owin; Aby użyć tego szablonu, wykonaj następujące czynności:

1. Kliknij prawym przyciskiem myszy projekt
2. Wybierz pozycję **Dodaj**, **nowy element...**
3. Wybierz **klasę uruchomieniową Owin**. Nadaj nowej klasie nazwę **Startup.cs**.

W **aplikacji sieci Web** Klasa uruchomieniowa Owin zawierająca metodę `MapSignalR` jest następnie dodawana do procesu uruchamiania Owin przy użyciu wpisu w węźle Ustawienia aplikacji w pliku Web. config, jak pokazano poniżej.

W **aplikacji samohostowanej**Klasa uruchomieniowa jest przenoszona jako parametr typu metody `WebApp.Start`.

**Mapowanie centrów i połączeń w sygnale 1. x (z pliku aplikacji globalnej w aplikacji sieci Web):** 

[!code-csharp[Main](release-notes/samples/sample5.cs)]

**Mapowanie centrów i połączeń w Sygnalizującer 2,0 (z pliku klasy startowej Owin):** 

[!code-csharp[Main](release-notes/samples/sample6.cs)]

W **aplikacji samohostowanej**Klasa uruchomieniowa jest przekazana jako parametr typu dla metody `WebApp.Start`, jak pokazano poniżej.

[!code-csharp[Main](release-notes/samples/sample7.cs)]

<a id="crossdomain"></a>

### <a name="cross-domain-support"></a>Obsługa wielu domen

W sygnale 1. x żądania między domenami były kontrolowane przez jedną flagę EnableCrossDomain. Ta flaga steruje żądaniami JSONP i CORS. Aby zapewnić większą elastyczność, cała obsługa mechanizmu CORS została usunięta ze składnika serwerowego sygnalizującego (klienci języka JavaScript nadal używają mechanizmu CORS normalnie, jeśli wykryje, że jest on obsługiwany przez przeglądarkę), a nowe oprogramowanie pośredniczące OWIN zostało udostępnione do obsługi tych scenariuszy.

Jeśli na kliencie JSONP jest wymagany program (do obsługi żądań międzydomenowych w starszych przeglądarkach), musi on zostać włączony jawnie przez ustawienie `EnableJSONP` w obiekcie `HubConfiguration` do `true`, jak pokazano poniżej. 2,0 JSONP jest domyślnie wyłączona, ponieważ jest mniej bezpieczny niż mechanizm CORS.

Aby dodać nowe oprogramowanie CORS do programu sygnalizującego 2,0, dodaj bibliotekę `Microsoft.Owin.Cors` do projektu i Wywołaj `UseCors` przed programem sygnalizującym, jak pokazano w poniższej sekcji.

**Dodawanie Microsoft. Owin. CORS do projektu**: Aby zainstalować tę bibliotekę, uruchom następujące polecenie w konsoli Menedżera pakietów:

[!code-powershell[Main](release-notes/samples/sample8.ps1)]

To polecenie spowoduje dodanie wersji 2.0.0 pakietu do projektu.

**Wywoływanie UseCors**

Poniższe fragmenty kodu przedstawiają sposób implementacji połączeń między domenami w sygnalizacji 1. x i 2,0.

**Implementowanie żądań międzydomenowych w sygnale 1. x (z pliku aplikacji globalnej)**

[!code-csharp[Main](release-notes/samples/sample9.cs)]

**Implementowanie żądań międzydomenowych w Sygnalizującer 2,0 (z C# pliku kodu)**

Poniższy kod ilustruje sposób włączania funkcji CORS lub JSONP w projekcie sygnalizującym 2,0. Ten przykładowy kod używa `Map` i `RunSignalR` zamiast `MapSignalR`, tak aby oprogramowanie pośredniczące CORS było uruchamiane tylko dla żądań sygnalizujących, które wymagają obsługi mechanizmu CORS (a nie dla całego ruchu w ścieżce określonej w `MapSignalR`). `Map` mogą być również używane dla innych programów pośredniczących, które muszą być uruchamiane dla określonego prefiksu adresu URL, a nie dla całej aplikacji.

[!code-csharp[Main](release-notes/samples/sample10.cs)]

<a id="mobile"></a>

### <a name="ios-and-android-support-via-monotouch-and-monodroid"></a>wsparcie dla systemów iOS i Android za pośrednictwem funkcji wielodotykowych i plik monodroid

Dodano pomoc techniczną dla klientów z systemami iOS i Android przy użyciu plik monodroid składników z [biblioteki Xamarin](https://xamarin.com/). Aby uzyskać więcej informacji na temat korzystania z nich, zobacz [Używanie składników platformy Xamarin](https://github.com/SignalR/SignalR/wiki/Building-Mono.Mobile.sln). Te składniki będą dostępne w [sklepie Xamarin Store](https://store.xamarin.com/) , gdy dostępna jest wersja RTW.

<a id="portable"></a># # # Przenośny klient platformy .NET

Aby ulepszyć Programowanie dla wielu platform, klienci korzystający z technologii Silverlight, WinRT i Windows Phone zostali zainstalowani za pomocą jednego przenośnego klienta platformy .NET, który obsługuje następujące platformy:

- SIEĆ 4,5
- Program Silverlight 5
- WinRT (.NET dla aplikacji do sklepu Windows)
- Windows Phone 8

<a id="selfhost"></a>

### <a name="new-self-host-package"></a>Nowy pakiet z własnym hostem

Teraz pakiet NuGet ułatwia rozpoczęcie pracy z usługą sygnalizującego samodzielnego hostowania (aplikacje sygnalizujące, które są hostowane w procesie lub innej aplikacji, a nie są hostowane na serwerze sieci Web). Aby uaktualnić projekt samoobsługowy utworzony przy użyciu sygnalizującego 1. x, Usuń pakiet Microsoft. AspNet. Signal. Owin i Dodaj pakiet Microsoft. AspNet. Signaler. SelfHost. Aby uzyskać więcej informacji na temat rozpoczynania pracy z pakietem samoobsługi, zobacz [Samouczek: program do](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)samoobsługowego hostowania.

<a id="backwardcompat"></a>

### <a name="backward-compatible-server-support"></a>Obsługa serwera zgodna z poprzednimi wersjami

W poprzednich wersjach programu sygnalizujący wersje pakietu sygnalizującego użytego w kliencie i serwerze muszą być identyczne. W celu obsługi aplikacji, które byłyby trudne do zaktualizowania, system sygnalizujący 2,0 obsługuje teraz nowszą wersję serwerową z starszym klientem. **Uwaga: sygnał 2,0 nie obsługuje serwerów utworzonych przy użyciu starszych wersji z nowszymi klientami.**

<a id="remove40"></a>

### <a name="removed-server-support-for-net-40"></a>Usunięto obsługę serwera dla programu .NET 4,0

Sygnał 2,0 został porzucony jako wsparcie dla współdziałania serwera z platformą .NET 4,0. Program .NET 4,5 musi być używany z serwerami sygnalizującymi 2,0. Nadal istnieje klient .NET 4,0 dla sygnalizującego 2,0.

<a id="messagelist"></a>

### <a name="sending-a-message-to-a-list-of-clients-and-groups"></a>Wysyłanie komunikatu do listy klientów i grup

W przypadku sygnalizującego 2,0 można wysłać komunikat przy użyciu listy identyfikatorów klientów i grup. Poniższe fragmenty kodu pokazują, jak to zrobić.

**Wysyłanie komunikatu do listy klientów i grup przy użyciu PersistentConnection**

[!code-csharp[Main](release-notes/samples/sample11.cs)]

**Wysyłanie komunikatu do listy klientów i grup przy użyciu centrów**

[!code-csharp[Main](release-notes/samples/sample12.cs)]

<a id="sendtouser"></a>

### <a name="sending-a-message-to-a-specific-user"></a>Wysyłanie komunikatu do określonego użytkownika

Ta funkcja umożliwia użytkownikom określenie, jakie elementy userId bazują na IRequest za pośrednictwem nowego interfejsu IUserIdProvider:

**Interfejs IUserIdProvider**

[!code-csharp[Main](release-notes/samples/sample13.cs)]

Domyślnie zostanie zaimplementowana implementacja, która używa IPrincipal.Identity.Name użytkownika jako nazwy użytkownika.

W centrach można wysyłać komunikaty do tych użytkowników za pośrednictwem nowego interfejsu API:

**Korzystanie z interfejsu API clients. User**

[!code-csharp[Main](release-notes/samples/sample14.cs)]

<a id="errorhandling"></a>

### <a name="better-error-handling-support"></a>Lepsza obsługa błędów

Użytkownicy mogą teraz zgłosić **HubException** z dowolnego wywołania centrum. Konstruktor elementu **HubException** może przyjmować komunikat ciągu i dodatkowe dane błędu obiektu. Sygnalizujący będzie autoserializować wyjątek i wyśle go do klienta, gdzie zostanie użyty do odrzucania/niepowodzenia wywołania metody centrum.

Ustawienie **Pokaż szczegółowe wyjątki centrum** nie ma wpływu na **HubException** z powrotem do klienta programu; jest on zawsze wysyłany.

**Kod po stronie serwera pokazujący wysyłanie HubException do klienta**

[!code-csharp[Main](release-notes/samples/sample15.cs)]

**Kod klienta JavaScript pokazujący odpowiedź na HubException wysyłane z serwera**

[!code-html[Main](release-notes/samples/sample16.html)]

**Kod klienta platformy .NET ukazujący odpowiedź na HubException wysyłane z serwera**

[!code-csharp[Main](release-notes/samples/sample17.cs)]

<a id="unittesting"></a>

### <a name="easier-unit-testing-of-hubs"></a>Łatwiejsze testowanie jednostkowe centrów

Sygnał 2,0 zawiera interfejs o nazwie `IHubCallerConnectionContext` w centrach, które ułatwiają tworzenie makietów po stronie klienta. Poniższe fragmenty kodu przedstawiają korzystanie z tego interfejsu z popularnymi [xUnit.NET](https://github.com/xunit/xunit) i [MOQ](https://code.google.com/p/moq/).

**Sygnalizujący testy jednostkowe z xUnit.net**

[!code-csharp[Main](release-notes/samples/sample18.cs)]

**Sygnalizujący testy jednostkowe z MOQ**

[!code-csharp[Main](release-notes/samples/sample19.cs)]

<a id="javascripterror"></a>

### <a name="javascript-error-handling"></a>Obsługa błędów języka JavaScript

W programie Sygnalizującer 2,0 wszystkie wywołania zwrotne obsługi błędów języka JavaScript zwracają obiekty błędów języka JavaScript zamiast ciągów nieprzetworzonych. Dzięki temu sygnał może przepływać bardziej szczegółowe informacje do obsługi błędów. Wewnętrzny wyjątek można pobrać z właściwości `source` błędu.

**Kod klienta JavaScript obsługujący wyjątek uruchamiania. Niepowodzenie**

[!code-javascript[Main](release-notes/samples/sample20.js)]

<a id="TOC8"></a>
## <a name="aspnet-identity"></a>ASP.NET Identity

### <a name="new-aspnet-membership-system"></a>Nowy system członkostwa ASP.NET

ASP.NET Identity to nowy system członkostwa dla aplikacji ASP.NET. ASP.NET Identity ułatwia integrację danych profilu specyficznych dla użytkownika z danymi aplikacji. ASP.NET Identity umożliwia również wybranie modelu trwałości dla profilów użytkowników w aplikacji. Dane można przechowywać w bazie danych SQL Server lub w innym magazynie danych, w tym z magazynami danych NoSQL, takimi jak tabele usługi Azure Storage. Aby uzyskać więcej informacji, zobacz [poszczególne konta użytkowników](creating-web-projects-in-visual-studio.md#indauth) w temacie **Tworzenie projektów sieci Web ASP.NET w Visual Studio 2013**.

### <a name="claims-based-authentication"></a>Uwierzytelnianie oparte na oświadczeniach

ASP.NET obsługuje teraz uwierzytelnianie oparte na oświadczeniach, gdzie tożsamość użytkownika jest reprezentowana jako zestaw oświadczeń od zaufanego wystawcy. Użytkownicy mogą być uwierzytelniani przy użyciu nazwy użytkownika i hasła przechowywanego w bazie danych aplikacji lub przy użyciu dostawców tożsamości społecznościowych (na przykład: konta Microsoft, Facebook, Google, Twitter) lub przy użyciu kont organizacji za pośrednictwem Azure Active Directory lub Active Directory Federation Services (AD FS).

### <a name="integration-with-azure-active-directory-and-windows-server-active-directory"></a>Integracja z usługami Azure Active Directory i Windows Server Active Directory

Teraz można tworzyć projekty ASP.NET, które używają Azure Active Directory lub Active Directory systemu Windows Server (AD) do uwierzytelniania. Aby uzyskać więcej informacji, zobacz [konta organizacyjne](creating-web-projects-in-visual-studio.md#orgauth) w temacie **Tworzenie projektów sieci Web ASP.NET w Visual Studio 2013**.

### <a name="owin-integration"></a>Integracja OWIN

Uwierzytelnianie ASP.NET jest teraz oparte na oprogramowaniu pośredniczącym OWIN, które może być używane na dowolnym hoście opartym na OWIN. Aby uzyskać więcej informacji na temat OWIN, zobacz następującą sekcję [składniki programu Microsoft Owin](#TOC7) .

<a id="TOC7"></a>
## <a name="microsoft-owin-components"></a>Składniki Microsoft OWIN

[Otwórz interfejs sieci Web dla platformy .NET](http://owin.org/) (Owin) definiuje abstrakcję między serwerami sieci Web i aplikacjami sieci Web programu .NET. OWIN oddziela aplikację sieci Web od serwera, udostępniając aplikacje sieci Web Host-niezależny od. Na przykład można hostować aplikację sieci Web opartą na OWIN w usługach IIS lub na własnym hoście w procesie niestandardowym.

Zmiany wprowadzone w składnikach programu Microsoft OWIN (znanym także jako projekt Katana) obejmują nowe składniki serwera i hosta, nowe biblioteki pomocników i oprogramowanie pośredniczące oraz nowe oprogramowanie pośredniczące uwierzytelniania.

Aby uzyskać więcej informacji na temat OWIN i Katana, zobacz [co nowego w Owin i Katana](../../../aspnet/overview/owin-and-katana/index.md).

**Uwaga: aplikacje [Owin](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) nie mogą być uruchamiane w trybie klasycznym usług IIS; muszą być uruchamiane w trybie zintegrowanym.**

**Uwaga: aplikacje [Owin](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) muszą być uruchamiane w trybie pełnego zaufania.**

### <a name="new-servers-and-hosts"></a>Nowe serwery i hosty

W tej wersji dodano nowe składniki umożliwiające obsługę scenariuszy samoobsługi. Te składniki obejmują następujące pakiety NuGet:

- **Microsoft. Owin. host. odbiornika HttpListener**. Udostępnia serwer OWIN, który używa **odbiornika HttpListener** do nasłuchiwania żądań HTTP i kierowania ich do potoku Owin.
- **Microsoft. Owin. hosting** udostępnia bibliotekę dla deweloperów, którzy chcą sam hostować potok Owin w procesie niestandardowym, takim jak Aplikacja konsolowa lub usługa systemu Windows.
- **OwinHost**. Udostępnia autonomiczny plik wykonywalny, który zawija `Microsoft.Owin.Hosting` i umożliwia samodzielne hostowanie potoku OWIN bez konieczności pisania niestandardowej aplikacji hosta.

Ponadto pakiet `Microsoft.Owin.Host.SystemWeb` umożliwia teraz oprogramowanie pośredniczące zapewniające wskazówki do serwera **SystemWeb** , co oznacza, że oprogramowanie pośredniczące powinno być wywoływane podczas określonego etapu potoku ASP.NET. Ta funkcja jest szczególnie przydatna w przypadku uwierzytelniania pośredniczącego, który powinien być uruchamiany wcześnie w potoku ASP.NET.

### <a name="helper-libraries-and-middleware"></a>Biblioteki pomocników i oprogramowanie pośredniczące

Chociaż można pisać składniki OWIN przy użyciu tylko definicji funkcji i typów ze specyfikacji OWIN, nowy pakiet `Microsoft.Owin` zapewnia bardziej przyjazny dla użytkownika zestaw abstrakcji. Ten pakiet łączy kilka wcześniejszych pakietów (np. `Owin.Extensions`, `Owin.Types`) w jeden, dobrze skonstruowany model obiektów, który może być łatwo używany przez inne składniki OWIN. W rzeczywistości większość składników OWIN firmy Microsoft używa teraz tego pakietu.

> [!NOTE]
> Nie można uruchamiać aplikacji [Owin](http://www.owin.org) w trybie klasycznym usług IIS; muszą być uruchamiane w trybie zintegrowanym.

> [!NOTE]
> Aplikacje [Owin](http://www.owin.org) muszą być uruchamiane w trybie pełnego zaufania.

Ta wersja zawiera również pakiet Microsoft. Owin. Diagnostics, który obejmuje oprogramowanie pośredniczące do weryfikowania działającej aplikacji OWIN oraz oprogramowania pośredniczącego stronicowego, aby pomóc w zbadaniu błędów.

### <a name="authentication-components"></a>Składniki uwierzytelniania

Dostępne są następujące składniki uwierzytelniania.

- **Microsoft. Owin. Security. ActiveDirectory**. Włącza uwierzytelnianie przy użyciu lokalnych lub opartych na chmurze usług katalogowych.
- Plik **Microsoft. Owin. Security. cookies** umożliwia uwierzytelnianie przy użyciu plików cookie. Ten pakiet miał wcześniej nazwę `Microsoft.Owin.Security.Forms`.
- **Microsoft. Owin. Security. Facebook** umożliwia uwierzytelnianie za pomocą usługi w usłudze Facebook.
- **Microsoft. Owin. Security. Google** umożliwia uwierzytelnianie przy użyciu usługi Google OpenID Connect.
- **Microsoft. Owin. Security. JWT** umożliwia uwierzytelnianie przy użyciu tokenów JWT.
- **Microsoft. Owin. Security. MicrosoftAccount** umożliwia uwierzytelnianie przy użyciu kont Microsoft.
- **Microsoft. Owin. Security. OAuth**. Udostępnia serwer autoryzacji OAuth oraz oprogramowanie pośredniczące do uwierzytelniania tokenów okaziciela.
- Plik **Microsoft. Owin. Security. Twitter** umożliwia uwierzytelnianie przy użyciu usługi w usłudze Twitter dla systemu.

Ta wersja zawiera również pakiet `Microsoft.Owin.Cors`, który zawiera oprogramowanie pośredniczące do przetwarzania żądań HTTP między źródłami.

> [!NOTE]
> Obsługa podpisywania tokenu JWT została usunięta w końcowej wersji Visual Studio 2013.

<a id="ef6"></a>
## <a name="entity-framework-6"></a>Entity Framework 6

Aby zapoznać się z listą nowych funkcji i innych zmian w Entity Framework 6, zobacz [Entity Framework historię wersji](https://msdn.com/data/jj574253).

<a id="TOC14"></a>
## <a name="aspnet-razor-3"></a>ASP.NET Razor 3

ASP.NET Razor 3 obejmuje następujące nowe funkcje:

- Obsługa edycji karty. Wcześniej polecenie **Formatuj dokument** , automatyczne wcinanie i automatyczne formatowanie w programie Visual Studio nie działało prawidłowo w przypadku używania opcji **Zachowaj karty** . Ta zmiana poprawia formatowanie programu Visual Studio dla kodu Razor na potrzeby formatowania tabulatorów.
- Obsługa reguł ponownego zapisywania adresów URL podczas generowania linków.
- Usuwanie atrybutu przezroczystego zabezpieczeń.
  > [!NOTE]
  > Jest to istotna zmiana i powoduje, że Razor 3 jest niezgodna z MVC4 i wcześniejszym, natomiast Razor 2 jest niezgodne z MVC5 lub zestawami skompilowanymi dla MVC5.

Problemy z Razor 3 rozwiązane w Visual Studio 2013 z wersji wstępnych można znaleźć [tutaj](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Resolved%7cClosed&type=All&priority=All&release=All%7cv5.0%2bPreview%7cv5.0%2bRC%7cv5.0%2bRTM&assignedTo=All&component=Web%2bPages%252fRazor&reasonClosed=Fixed&sortField=LastUpdatedDate&sortDirection=Descending&page=0).

<a id="TOC15"></a>
## <a name="aspnet-app-suspend"></a>Wstrzymywanie aplikacji ASP.NET

Wstrzymywanie aplikacji ASP.NET to funkcja zmiany gier w .NET Framework 4.5.1, która radykalnie zmienia środowisko użytkownika i model gospodarczy do hostowania dużej liczby witryn ASP.NET na jednym komputerze. Aby uzyskać więcej informacji, zobacz [ASP.NET App Suspend — współdzielona obsługa sieci Web platformy .NET](https://blogs.msdn.com/b/dotnet/archive/2013/10/09/asp-net-app-suspend-responsive-shared-net-web-hosting.aspx).

<a id="knownissues"></a>
## <a name="known-issues-and-breaking-changes"></a>Znane problemy i istotne zmiany

W tej sekcji opisano znane problemy i istotne zmiany w ASP.NET and Web Tools Visual Studio 2013.

### <a name="nuget"></a>NuGet

- [Przywracanie nowego pakietu nie działa na platformie mono podczas korzystania z pliku SLN](https://nuget.codeplex.com/workitem/3596) — zostanie naprawione w nadchodzącym pobieraniu NuGet. exe i [pakiecie NuGet. Zaktualizuj pakiet](http://www.nuget.org/packages/NuGet.CommandLine/) .
- [Nowe przywracanie pakietu nie działa z projektami WIX](https://nuget.codeplex.com/workitem/3598) — zostanie naprawione w nadchodzącym pliku NuGet. exe i [pakiecie NuGet. Zaktualizuj pakiet](http://www.nuget.org/packages/NuGet.CommandLine/) .
- [Automatyczne przywracanie pakietów nie działa w przypadku projektów w folderze rozwiązania](https://nuget.codeplex.com/workitem/3625) — zostanie ono naprawione w pakiecie NuGet 2,8.

### <a name="aspnet-web-api"></a>Składnik Web API platformy ASP.NET

1. `ODataQueryOptions<T>.ApplyTo(IQueryable)` nie zwraca `IQueryable<T>` zawsze, ponieważ dodaliśmy obsługę `$select` i `$expand`.

    Nasze poprzednie przykłady dla `ODataQueryOptions<T>` zawsze przelane wartość zwracaną z `ApplyTo` do `IQueryable<T>`. Ten problem działał wcześniej, ponieważ opcje zapytania, które zostały wcześniej obsługiwane (`$filter`, `$orderby`, `$skip``$top`) nie zmieniają kształtu zapytania. Teraz, gdy obsługujemy `$select` i `$expand` wartość zwracana przez `ApplyTo` nie będzie `IQueryable<T>` zawsze.

    [!code-csharp[Main](release-notes/samples/sample21.cs)]

    Jeśli używasz przykładowego kodu z wcześniejszych wersji, będzie on kontynuował pracę, jeśli klient nie wyśle `$select` i `$expand`. Jeśli jednak chcesz obsługiwać `$select` i `$expand` musisz zmienić ten kod na ten.

    [!code-csharp[Main](release-notes/samples/sample22.cs)]
2. **Element Request. URL lub RequestContext. URL ma wartość null w trakcie żądania wsadowego**

    W scenariuszu wsadowym **UrlHelper** ma wartość null, gdy uzyskuje się dostęp z elementu **Request. URL** lub **RequestContext. URL**.

    Ten problem jest obecnie śledzony w tym miejscu: [BatchRequestContext. URL ma wartość null dla żądania wsadowego](http://aspnetwebstack.codeplex.com/workitem/1301).

    Obejście tego problemu polega na utworzeniu nowego wystąpienia **UrlHelper**, jak w poniższym przykładzie:

    **Tworzenie nowego wystąpienia elementu UrlHelper**

    [!code-csharp[Main](release-notes/samples/sample23.cs)]

### <a name="aspnet-mvc"></a>ASP.NET MVC

1. W przypadku korzystania z MVC5 i OrgAuth, jeśli masz widoki, które wykonują weryfikację AntiForgerToken, podczas publikowania danych w widoku może wystąpić następujący błąd:

    **Błąd**:

    *Błąd serwera w aplikacji "/".*

    <em>Nie znaleziono typu "<http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier>" lub "<https://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider>" w podanym identyfikator oświadczenia. Aby włączyć obsługę uwierzytelniania przy użyciu oświadczeń na podstawie opartych na oświadczeniach, należy sprawdzić, czy skonfigurowany dostawca oświadczeń udostępnia oba te oświadczenia na wystąpieniach identyfikator oświadczenia, które generuje. Jeśli skonfigurowany dostawca oświadczeń używa zamiast tego innego typu oświadczenia jako unikatowego identyfikatora, można go skonfigurować przez ustawienie właściwości statycznej AntiForgeryConfig. UniqueClaimTypeIdentifier.</em>

    **Obejście problemu**:

    Aby rozwiązać ten problem, Dodaj następujący wiersz w elemencie Global. asax:

    `AntiForgeryConfig.UniqueClaimTypeIdentifier = ClaimTypes.Name;`

    Ten problem zostanie rozwiązany w następnej wersji.
2. Po uaktualnieniu aplikacji MVC4 do MVC5, skompiluj rozwiązanie i uruchom je. Powinien zostać wyświetlony następujący błąd:

    Z Nie można rzutować programu System. Web. Webpages. Razor. Configuration. HostSection na [B] system. Web. Webpages. Razor. Configuration. HostSection. Typ A pochodzi od "System. Web. Webpages. Razor, Version = 2.0.0.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35" w kontekście "default" w lokalizacji "C:\windows\Microsoft.Net\assembly\GAC\_MSIL\System.Web.WebPages.Razor\v4.0\_2.0.0.0\_\_31bf3856ad364e35 \ System. Web. Webpages. Razor. dll". Typ B pochodzi od elementu "System. Web. Webpages. Razor, Version = 3.0.0.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35" w kontekście "default" w lokalizacji "C:\Windows\Microsoft.NET\Framework\v4.0.30319\Temporary ASP.NET Files\root\6d05bbd0\e8b5908e\assembly\dl3\c9cbca63\f8910382\_6273ce01 \ System. Web. Webpages. Razor. dll".

    Aby naprawić powyższy błąd, Otwórz *wszystkie* pliki Web. config (w tym foldery w folderze widoki) w projekcie i wykonaj następujące czynności:

   1. Zaktualizuj wszystkie wystąpienia wersji "4.0.0.0" elementu "System. Web. MVC" do "5.0.0.0".
   2. Zaktualizuj wszystkie wystąpienia wersji "2.0.0.0" "System. Web. Pomocnicys", &quot;system. Web. Webpages&quot; i &quot;system. Web. Webpages. Razor&quot; do "3.0.0.0"

      Na przykład po wprowadzeniu powyższych zmian powiązania zestawów powinny wyglądać następująco:

      [!code-xml[Main](release-notes/samples/sample24.xml)]

      Aby uzyskać informacje na temat uaktualniania projektów MVC 4 do MVC 5, zobacz [jak uaktualnić projekt ASP.NET MVC 4 i interfejs API sieci Web do ASP.NET MVC 5 i Web API 2](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).
3. W przypadku korzystania z walidacji po stronie klienta z niedyskretną weryfikacją jQuery komunikat weryfikacyjny jest czasami nieprawidłowy dla elementu wejściowego HTML z typem = "number". Błąd walidacji dla wymaganej wartości ("pole wiek jest wymagane") jest wyświetlany, gdy zostanie wprowadzony nieprawidłowy numer zamiast poprawnego komunikatu, że wymagana jest prawidłowa liczba.

    Ten problem często znajduje się w przypadku kodu szkieletowego dla modelu z właściwością Integer w widokach tworzenie i edytowanie.

    Aby obejść ten problem, Zmień pomocnika edytora z:

    `@Html.EditorFor(person => person.Age)`

    Do:

    `@Html.TextBoxFor(person => person.Age)`
4. ASP.NET MVC 5 nie obsługuje już częściowej relacji zaufania. Projekty łączące się z danymi binarnymi MVC lub WebAPI powinny usunąć atrybut [SecurityTransparent](https://msdn.microsoft.com/library/system.security.securitytransparentattribute.aspx) i atrybut [AllowPartiallyTrustedCallers](https://msdn.microsoft.com/library/system.security.allowpartiallytrustedcallersattribute.aspx) . Usunięcie tych atrybutów spowoduje usunięcie błędów kompilatora, takich jak następujące.

    `Attempt by security transparent method ‘MyComponent' to access security critical type 'System.Web.Mvc.MvcHtmlString' failed. Assembly 'PagedList.Mvc, Version=4.3.0.0, Culture=neutral, PublicKeyToken=abbb863e9397c5e1' is marked with the AllowPartiallyTrustedCallersAttribute, and uses the level 2 security transparency model. Level 2 transparency causes all methods in AllowPartiallyTrustedCallers assemblies to become security transparent by default, which may be the cause of this exception.`

    > Zwróć uwagę, że jako efekt uboczny tego nie można używać zestawów 4,0 i 5,0 w tej samej aplikacji. Wszystkie te elementy należy zaktualizować do 5,0.

### <a name="spa-template-with-facebook-authorization-may-cause-instability-in-ie-while-the-web-site-is-hosted-in-intranet-zone"></a>Szablon SPA z autoryzacją w serwisie Facebook może spowodować niestabilność w programie IE, gdy witryna sieci Web jest hostowana w strefie intranetowej

Szablon SPA zawiera zewnętrzne logowanie w usłudze Facebook. Gdy projekt utworzony za pomocą szablonu jest uruchamiany lokalnie, logowanie może spowodować awarię programu IE.

Rozwiązanie:

1. Hostowanie witryny sieci Web w strefie Internet; oraz

2. Przetestuj scenariusz w przeglądarce innej niż IE.

### <a name="web-forms-scaffolding"></a>Tworzenie szkieletu formularzy sieci Web

Tworzenie szkieletu formularzy sieci Web zostało usunięte z VS2013 i będzie dostępne w przyszłej aktualizacji programu Visual Studio. Można jednak nadal używać szkieletów w projekcie formularzy sieci Web, dodając zależności MVC i generując szkielet dla MVC. Projekt będzie zawierać kombinację formularzy sieci Web i MVC.

Aby dodać MVC do projektu formularzy sieci Web, Dodaj nowy element szkieletowy i wybierz **zależności MVC 5**. Wybierz opcję minimalny lub pełny w zależności od tego, czy potrzebne są wszystkie pliki zawartości, takie jak skrypty. Następnie Dodaj element szkieletowy dla MVC, który utworzy widoki i kontroler w projekcie.

### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a>Tworzenie szkieletu kodu MVC i interfejsu API sieci Web-HTTP 404, nie znaleziono błędu

Jeśli wystąpi błąd podczas dodawania elementu szkieletowego do projektu, istnieje możliwość, że projekt zostanie pozostawiony w stanie niespójnym. Niektóre zmiany zostaną wycofane, ale inne zmiany, takie jak zainstalowane pakiety NuGet, nie zostaną wycofane. Jeśli zmiany konfiguracji routingu są wycofywane, użytkownicy otrzymają błąd HTTP 404 podczas przechodzenia do elementów szkieletowych.

Obejście problemu:

- Aby naprawić ten błąd dla składnika MVC, Dodaj nowy element szkieletowy i wybierz zależności MVC 5 (minimalnie lub pełne). Ten proces spowoduje dodanie wszystkich wymaganych zmian do projektu.
- Aby naprawić ten błąd dla interfejsu API sieci Web:

  1. Dodaj klasę WebApiConfig do projektu.

      [!code-csharp[Main](release-notes/samples/sample25.cs)]

      [!code-vb[Main](release-notes/samples/sample26.vb)]
  2. Skonfiguruj WebApiConfig. Register w aplikacji\_Start w programie Global. asax w następujący sposób:

      [!code-csharp[Main](release-notes/samples/sample27.cs)]

      [!code-vb[Main](release-notes/samples/sample28.vb)]

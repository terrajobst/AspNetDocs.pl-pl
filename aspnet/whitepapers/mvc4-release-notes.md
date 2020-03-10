---
uid: whitepapers/mvc4-release-notes
title: ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: W tym dokumencie opisano wydanie ASP.NET MVC 4.
ms.author: riande
ms.date: 09/09/2011
ms.assetid: f014524f-25c0-4094-b8e1-886d99536f00
msc.legacyurl: /whitepapers/mvc4-release-notes
msc.type: content
ms.openlocfilehash: b57480bd0274fbb76c600dfb0dd09037bdcbf1e7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78563428"
---
# <a name="aspnet-mvc-4"></a>ASP.NET MVC w wersji 4

> W tym dokumencie opisano wydanie ASP.NET MVC 4.

- [Uwagi dotyczące instalacji](#_Toc303253802)
- [Dokumentacja](#_Toc303253803)
- [Pomoc techniczna](#_Toc303253804)
- [Wymagania dotyczące oprogramowania](#_Toc303253805)
- [Nowe funkcje w programie ASP.NET MVC 4](#_Toc303253807)

    - [ASP.NET Web API](#_Toc317096197)
    - [Ulepszenia domyślnych szablonów projektów](#_Toc303253808)
    - [Szablon projektu mobilnego](#_Toc303253809)
    - [Tryby wyświetlania](#_Toc303253810)
    - [Aplikacje jQuery Mobile, przełącznik widoku i zastępowanie przeglądarki](#_Toc303253811)
    - [Obsługa zadań dla kontrolerów asynchronicznych](#_Toc303253813)
    - [Zestaw Azure SDK](#_Toc303253814)
    - [Migracje baz danych](#_Toc303253818)
    - [Pusty szablon projektu](#_Toc303253819)
    - [Dodaj kontroler do dowolnego folderu projektu](#_Toc303253820)
    - [Tworzenie pakietów i minifikacja](#_Toc303253821)
    - [Włączanie logowania z serwisu Facebook i innych witryn przy użyciu protokołu OAuth i OpenID Connect](#_Toc303253822)
- [Uaktualnianie projektu ASP.NET MVC 3 do ASP.NET MVC 4](#_Toc303253806)
- [Zmiany z wersji ASP.NET MVC 4 Release Candidate](#_Toc303253817)
- [Znane problemy i istotne zmiany](#_Toc303253815)

<a id="_Toc303253802"></a>
## <a name="installation-notes"></a>Uwagi dotyczące instalacji

ASP.NET MVC 4 for Visual Studio 2010 można zainstalować ze [strony głównej ASP.NET MVC 4](../mvc/mvc4.md) przy użyciu Instalatora platformy sieci Web.

Zalecamy odinstalowanie wszystkich wcześniej zainstalowanych wersji zapoznawczych ASP.NET MVC 4 przed instalacją ASP.NET MVC 4. Możesz uaktualnić ASP.NET MVC 4 beta i Release Candidate do ASP.NET MVC 4 bez odinstalowywania.

Ta wersja nie jest zgodna z żadną wersją zapoznawczą .NET Framework 4,5. Przed zainstalowaniem ASP.NET MVC 4 należy oddzielnie uaktualnić wszystkie zainstalowane wersje zapoznawcze programu .NET Framework 4,5 do wersji ostatecznej.

ASP.NET MVC 4 można zainstalować i uruchomić obok siebie przy użyciu ASP.NET MVC 3.

<a id="_Toc303253803"></a>
## <a name="documentation"></a>Dokumentacja

Dokumentacja usługi ASP.NET MVC jest dostępna w witrynie MSDN pod następującym adresem URL:

[https://go.microsoft.com/fwlink/?LinkID=243043](https://go.microsoft.com/fwlink/?LinkID=243043)

Samouczki i inne informacje dotyczące ASP.NET MVC są dostępne na stronie MVC 4 witryny sieci Web ASP.NET ([https://www.asp.net/mvc/mvc4](../mvc/mvc4.md)).

<a id="_Toc303253804"></a>
## <a name="support"></a>Pomoc techniczna

ASP.NET MVC 4 jest w pełni obsługiwany. Jeśli masz pytania dotyczące pracy z tą wersją, możesz również ogłosić je na forum ASP.NET MVC ([https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)), gdzie członkowie społeczności ASP.NET często mogą zapewnić nieformalne wsparcie.

<a id="_Toc303253805"></a>
## <a name="software-requirements"></a>Wymagania programowe

Składniki ASP.NET MVC 4 dla programu Visual Studio wymagają programu PowerShell 2,0 i programu Visual Studio 2010 z dodatkiem Service Pack 1 lub Visual Web Developer Express 2010 z dodatkiem Service Pack 1.

<a id="_Toc303253807"></a>
## <a name="new-features-in-aspnet-mvc-4"></a>Nowe funkcje w programie ASP.NET MVC 4

W tej sekcji opisano funkcje wprowadzone w wersji ASP.NET MVC 4.

<a id="_Toc317096197"></a>
### <a name="aspnet-web-api"></a>Składnik Web API platformy ASP.NET

ASP.NET MVC 4 zawiera internetowy interfejs API ASP.NET, nową strukturę tworzenia usług HTTP, która może dotrzeć do szerokiego zakresu klientów, w tym przeglądarek i urządzeń przenośnych. Interfejs API sieci Web ASP.NET jest również idealnym platformą do kompilowania usług RESTful.

Interfejs API sieci Web ASP.NET zapewnia obsługę następujących funkcji:

- **Nowoczesny model programowania http:** Bezpośredni dostęp do żądań i odpowiedzi HTTP w interfejsach API sieci Web oraz manipulowanie nimi przy użyciu nowego, silnie określonego modelu obiektów HTTP. Ten sam model programowania i potok HTTP są symetrycznie dostępne na kliencie za pośrednictwem nowego typu *HttpClient* .
- **Pełna obsługa tras:** Interfejs API sieci Web ASP.NET obsługuje pełny zestaw możliwości routingu usługi ASP.NET routing, w tym parametrów tras i ograniczeń. Ponadto należy używać prostych Konwencji do mapowania akcji na metody HTTP.
- **Negocjowanie zawartości:** Klient i serwer mogą współdziałać ze sobą, aby określić właściwy format danych zwracanych z internetowego interfejsu API. Interfejs API sieci Web ASP.NET zapewnia obsługę domyślną dla formatów XML, JSON i formacie adresów URL i można ją rozciągnąć przez dodanie własnych elementów formatujących, a nawet zastąpienie domyślnej strategii negocjowania zawartości.
- **Powiązanie i walidacja modelu:** Powiązania modelu zapewniają łatwy sposób wyodrębniania danych z różnych części żądania HTTP i przekształcania tych części komunikatów na obiekty .NET, które mogą być używane przez akcje interfejsu API sieci Web. Sprawdzanie poprawności jest wykonywane również w przypadku parametrów akcji opartych na adnotacjach danych.
- **Filtry:** Interfejs API sieci Web ASP.NET obsługuje filtry z uwzględnieniem dobrze znanych filtrów, takich jak atrybut *[autoryzuje]* . Można tworzyć i dołączać własne filtry do akcji, autoryzacji i obsługi wyjątków.
- **Kompozycja zapytania:** Użyj atrybutu filtru *[Queryable]* dla akcji zwracającej Interfejs *IQueryable* , aby włączyć obsługę zapytań do internetowego interfejsu API za pośrednictwem Konwencji zapytania OData.
- **Ulepszone możliwości testowania:** Zamiast ustawiania szczegółów protokołu HTTP w statycznych obiektach kontekstu, akcje internetowego interfejsu API działają z wystąpieniami *HttpRequestMessage* i *HttpResponseMessage*. Utwórz projekt testu jednostkowego wraz z projektem interfejsu API sieci Web, aby zacząć szybko pisać testy jednostkowe dla funkcjonalności internetowego interfejsu API.
- **Konfiguracja oparta na kodzie:** Konfiguracja interfejsu API sieci Web ASP.NET jest realizowana wyłącznie za poorednictwem kodu, pozostawiając czyszczenie plików konfiguracji. Aby skonfigurować punkty rozszerzalności, należy użyć podanego wzorca lokalizatora usługi.
- **Ulepszona obsługa kontenerów z niewersjami formantów (IOC):** Interfejs API sieci Web ASP.NET zapewnia doskonałą obsługę kontenerów IoC za pomocą ulepszonego abstrakcji programu rozpoznawania zależności
- **Samodzielne hostowanie:** Interfejsy API sieci Web mogą być hostowane we własnym procesie oprócz usług IIS i nadal korzystają z pełnych możliwości tras i innych funkcji interfejsu API sieci Web.
- **Utwórz niestandardową stronę pomocy i testów:** Teraz można łatwo tworzyć niestandardowe strony pomocy i testów dla interfejsów API sieci Web za pomocą nowej usługi *IApiExplorer* , aby uzyskać pełny opis środowiska uruchomieniowego interfejsów API sieci Web.
- **Monitorowanie i Diagnostyka:** Interfejs API sieci Web ASP.NET teraz udostępnia infrastrukturę śledzenia wagi uproszczonej, która ułatwia integrację z istniejącymi rozwiązaniami rejestrowania, takimi jak system. Diagnostics, ETW i platformy rejestrowania innych firm. Śledzenie można włączyć, dostarczając implementację *ITraceWriter* i dodając ją do konfiguracji internetowego interfejsu API.
- **Generowanie linku:** Użyj *UrlHelper* internetowego interfejsu API ASP.NET, aby wygenerować linki do powiązanych zasobów w tej samej aplikacji.
- **Szablon projektu interfejsu API sieci Web:** Wybierz nowy projekt interfejsu API sieci Web Kreator nowego projektu MVC 4, aby szybko rozpocząć pracę z interfejsem API sieci Web ASP.NET.
- Tworzenie **szkieletu:** Za pomocą okna dialogowego **Dodawanie kontrolera** można szybko przetworzyć kontroler interfejsu API sieci Web na podstawie typu modelu opartego na Entity Framework.

Aby uzyskać więcej informacji na temat interfejsu Web API ASP.NET, odwiedź stronę [https://www.asp.net/web-api](../web-api/index.md).

<a id="_Toc303253808"></a>
### <a name="enhancements-to-default-project-templates"></a>Ulepszenia domyślnych szablonów projektów

Szablon służący do tworzenia nowych projektów ASP.NET MVC 4 został zaktualizowany w celu utworzenia bardziej nowoczesnej witryny sieci Web:

![](mvc4-release-notes/_static/image1.png)

Oprócz ulepszeń kosmetycznych udoskonalono funkcjonalność nowego szablonu. Szablon wykorzystuje technikę o nazwie renderowanie adaptacyjne, aby wyglądać dobrze w przeglądarkach klasycznych i przeglądarkach dla urządzeń przenośnych bez żadnego dostosowania.

![](mvc4-release-notes/_static/image2.png)

Aby zobaczyć adaptacyjne renderowanie w akcji, można użyć emulatora urządzenia przenośnego lub po prostu spróbować zmienić rozmiar okna przeglądarki pulpitu, aby było mniejsze. Gdy okno przeglądarki jest wystarczająco małe, układ strony zmieni się.

<a id="_Toc303253809"></a>
### <a name="mobile-project-template"></a>Szablon projektu mobilnego

Jeśli uruchamiasz nowy projekt i chcesz utworzyć witrynę specyficzną dla przeglądarek mobilnych i tabletów, możesz użyć nowego szablonu projektu aplikacji mobilnej. Jest to oparte na technologii jQuery Mobile, biblioteki Open Source do tworzenia interfejsu użytkownika zoptymalizowanego pod kątem obsługi dotykowej:

![](mvc4-release-notes/_static/image3.png)

Ten szablon zawiera tę samą strukturę aplikacji, co szablon aplikacji internetowej (a kod kontrolera jest praktycznie identyczny), ale jest on w stylu przy użyciu technologii jQuery Mobile, aby uzyskać dobre i dobrze działać na urządzeniach przenośnych opartych na dotykach. Aby dowiedzieć się więcej na temat struktury i stylu mobilnego interfejsu użytkownika, zobacz [witrynę sieci Web programu jQuery Mobile](http://jquerymobile.com/).

Jeśli masz już lokację zorientowaną na komputery stacjonarne, do której chcesz dodać widoki zoptymalizowane pod kątem urządzeń przenośnych, lub jeśli chcesz utworzyć pojedynczą lokację, która obsługuje widoki w różnych stylach, można użyć nowych trybów wyświetlania. (Zobacz następną sekcję).

<a id="_Toc303253810"></a>
### <a name="display-modes"></a>Tryby wyświetlania

Nowa funkcja trybów wyświetlania umożliwia aplikacji Wybieranie widoków w zależności od przeglądarki, która żąda żądania. Na przykład jeśli przeglądarka pulpitu żąda strony głównej, aplikacja może używać szablonu Views\Home\Index.cshtml. Jeśli przeglądarka mobilna żąda strony głównej, aplikacja może zwrócić szablon Views\Home\Index.mobile.cshtml.

Układy i częściowe mogą być również zastępowane dla określonych typów przeglądarek. Na przykład:

- Jeśli folder Views\Shared zawiera zarówno szablon \_Layout. cshtml, jak i \_Layout. Mobile. cshtml, domyślnie aplikacja będzie używać \_układ. Mobile. cshtml podczas żądań z przeglądarek mobilnych i \_Layout. cshtml podczas innych żądań.
- Jeśli folder zawiera zarówno \_. cshtml, jak i \_. Mobile. cshtml, instrukcja @Html.Partial("\_") będzie renderowana \_. Mobile. cshtml w trakcie żądań z przeglądarek mobilnych i \_część. cshtml podczas innych żądań.

Jeśli chcesz utworzyć bardziej szczegółowe widoki, układy lub częściowe widoki dla innych urządzeń, możesz zarejestrować nowe wystąpienie usługi *DefaultDisplayMode* , aby określić, która nazwa ma być wyszukiwana, gdy żądanie spełnia określone warunki. Na przykład można dodać następujący kod do *aplikacji\_Start* metody w pliku Global. asax, aby zarejestrować ciąg "iPhone" jako tryb wyświetlania, który ma zastosowanie, gdy przeglądarka telefonu iPhone wysyła żądanie:

[!code-csharp[Main](mvc4-release-notes/samples/sample1.cs)]

Po uruchomieniu tego kodu, gdy przeglądarka telefonów iPhone firmy Apple wyśle żądanie, aplikacja użyje układu Views\Shared\\_Layout. iPhone. cshtml (jeśli istnieje). Aby uzyskać więcej informacji o trybie wyświetlania, zobacz [ASP.NET MVC 4 Mobile Features](../mvc/overview/older-versions/aspnet-mvc-4-mobile-features.md). Aplikacje korzystające z DisplayModeProvider powinny instalować naprawione pakiety NuGet [DisplayModes](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) . [Aktualizacja ASP.NET 2012](https://go.microsoft.com/fwlink/?LinkID=271322) obejmuje naprawione pakiety NuGet [DisplayModes](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) w nowych szablonach projektów. Aby uzyskać szczegółowe informacje na temat poprawki, zobacz [ASP.NET MVC 4 Mobile buforowania usterki](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx) .

<a id="_Toc303253811"></a>
### <a name="jquery-mobile-and-mobile-features"></a>Funkcje mobilne i mobilne jQuery

Aby uzyskać informacje na temat tworzenia aplikacji mobilnych za pomocą ASP.NET MVC 4 przy użyciu technologii jQuery Mobile, zobacz samouczek [ASP.NET MVC 4 Mobile Features](../mvc/overview/older-versions/aspnet-mvc-4-mobile-features.md).

<a id="_Toc303253813"></a>
### <a name="task-support-for-asynchronous-controllers"></a>Obsługa zadań dla kontrolerów asynchronicznych

Teraz można pisać metody akcji asynchronicznych jako pojedyncze metody, które zwracają obiekt typu *zadanie* lub *zadanie&lt;ActionResult&gt;* .

 Aby uzyskać więcej informacji, zobacz [Używanie metod asynchronicznych w ASP.NET MVC 4](../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).

<a id="_Toc303253814"></a>
### <a name="azure-sdk"></a>Azure SDK

ASP.NET MVC 4 obsługuje 1,6 i nowsze wersje zestawu Windows Azure SDK.

<a id="_Toc303253818"></a>
### <a name="database-migrations"></a>Migracje baz danych

Projekty ASP.NET MVC 4 zawierają teraz Entity Framework 5. Jedną z doskonałych funkcji programu Entity Framework 5 jest obsługa migracji baz danych. Ta funkcja pozwala łatwo rozwijać schemat bazy danych przy użyciu migracji ukierunkowanej na kod podczas zachowywania danych w bazie danych. Aby uzyskać więcej informacji na temat migracji bazy danych, zobacz [Dodawanie nowego pola do modelu i tabeli filmów](../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table.md) w [samouczku wprowadzenie do ASP.NET MVC 4](../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md).

<a id="_Toc303253819"></a>
### <a name="empty-project-template"></a>Pusty szablon projektu

Szablon pustego projektu MVC jest teraz całkowicie pusty, aby można było zacząć od całkiem czystego zamknięcia. Nazwa wcześniejszej wersji pustego szablonu projektu została zmieniona na podstawowa.

<a id="_Toc303253820"></a>
### <a name="add-controller-to-any-project-folder"></a>Dodaj kontroler do dowolnego folderu projektu

Teraz możesz kliknąć prawym przyciskiem myszy i wybrać pozycję **Dodaj kontroler** z dowolnego folderu w projekcie MVC. Zapewnia to większą elastyczność organizowania kontrolerów, w tym utrzymywanie kontrolerów MVC i interfejsów API sieci Web w oddzielnych folderach.

<a id="_Toc303253821"></a>
### <a name="bundling-and-minification"></a>Tworzenie pakietów i minimalizowanie

Struktura i minifikacja Framework umożliwiają zmniejszenie liczby żądań HTTP wymaganych przez stronę sieci Web przez połączenie pojedynczych plików w jeden plik z pakietem dla skryptów i CSS. Następnie może zmniejszyć ogólny rozmiar tych żądań, minifikacja zawartość pakietu. Minifikacja może zawierać działania, takie jak eliminowanie spacji w celu skrócenia nazw zmiennych, aby nawet zwijać selektory CSS na podstawie ich semantyki. Zbiory są zadeklarowane i konfigurowane w kodzie i można je łatwo odwoływać w widokach za pośrednictwem metod pomocnika, które mogą generować jedno łącze do pakietu lub, w przypadku debugowania, wiele linków do poszczególnych zawartości pakietu. Aby uzyskać więcej informacji, zobacz artykułing [and minifikacja](../mvc/overview/performance/bundling-and-minification.md).

<a id="_Toc303253822"></a>
### <a name="enabling-logins-from-facebook-and-other-sites-using-oauth-and-openid"></a>Włączanie logowania z serwisu Facebook i innych witryn przy użyciu protokołu OAuth i OpenID Connect

Szablony domyślne w szablonach projektów internetowych w programie ASP.NET MVC 4 obejmują teraz obsługę logowania OAuth i OpenID Connect przy użyciu biblioteki DotNetOpenAuth. Informacje dotyczące konfigurowania dostawcy OAuth lub OpenID Connect można znaleźć w temacie [Obsługa OAuth/OpenID Connect dla formularzy WebForms, MVC i Webpages](https://blogs.msdn.com/b/webdev/archive/2012/08/15/oauth-openid-support-for-webforms-mvc-and-webpages.aspx) oraz [Dokumentacja funkcji OAuth i OpenID Connect na stronach sieci Web ASP.NET](../web-pages/overview/releases/top-features-in-web-pages-2.md#oauthsetup).

<a id="_Toc303253806"></a>
## <a name="upgrading-an-aspnet-mvc-3-project-to-aspnet-mvc-4"></a>Uaktualnianie projektu ASP.NET MVC 3 do ASP.NET MVC 4

ASP.NET MVC 4 można zainstalować równolegle z ASP.NET MVC 3 na tym samym komputerze, co zapewnia elastyczność w wyborze czasu uaktualnienia aplikacji ASP.NET MVC 3 do ASP.NET MVC 4.

Najprostszym sposobem na uaktualnienie jest utworzenie nowego projektu ASP.NET MVC 4 i skopiowanie wszystkich widoków, kontrolerów, kodu i plików zawartości z istniejącego projektu MVC 3 do nowego projektu, a następnie zaktualizowanie odwołań do zestawów w nowym projekcie w celu dopasowania ich do szablonu innego niż MVC w cluded Assembiles. Jeśli wprowadzono zmiany w pliku Web. config w projekcie MVC 3, należy również scalić te zmiany w pliku Web. config w projekcie MVC 4.

Aby ręcznie uaktualnić istniejącą aplikację ASP.NET MVC 3 do wersji 4, wykonaj następujące czynności:

1. We wszystkich plikach Web. config w projekcie (istnieje jeden w katalogu głównym projektu, jeden w folderze widoki i jeden w folderze widoki dla każdego obszaru w projekcie), Zastąp każde wystąpienie następującego tekstu (Uwaga: System. Web. Webpages, Version = 1.0.0.0 nie został znaleziony w projekty utworzone za pomocą programu Visual Studio 2012): 

    [!code-console[Main](mvc4-release-notes/samples/sample2.cmd)]

    z następującym odpowiednim tekstem:

    [!code-console[Main](mvc4-release-notes/samples/sample3.cmd)]
2. W głównym pliku Web. config zaktualizuj elementy *Webpages: Version* do "2.0.0.0" i Dodaj nowy klucz *PreserveLoginUrl* , który ma wartość "true": 

    [!code-xml[Main](mvc4-release-notes/samples/sample4.xml)]
3. W Eksplorator rozwiązań kliknij prawym przyciskiem myszy odwołania i wybierz pozycję Zarządzaj pakietami NuGet. W lewym okienku wybierz pozycję **Online\NuGet oficjalne źródło pakietu**, a następnie zaktualizuj następujące elementy:

    - ASP.NET MVC w wersji 4
    - (Opcjonalnie) program jQuery, walidacja jQuery i interfejs użytkownika jQuery
    - Obowiązkowe Entity Framework
    - (Optonal) Modernizacja
4. W Eksplorator rozwiązań kliknij prawym przyciskiem myszy nazwę projektu, a następnie wybierz polecenie Zwolnij projekt. Następnie ponownie kliknij prawym przyciskiem myszy nazwę i wybierz polecenie Edytuj *ProjectName*. csproj.
5. Znajdź element *ProjectTypeGuids* i Zastąp ciąg {E53F8FEA-EAE0-44A6-8774-FFD645390401} atrybutem {E3E379DF-F4C6-4180-9B81-6769533ABE47}.
6. Zapisz zmiany, Zamknij edytowany plik projektu (. csproj), kliknij prawym przyciskiem myszy projekt, a następnie wybierz polecenie Załaduj ponownie projekt.
7. Jeśli projekt odwołuje się do wszystkich bibliotek innych firm, które są kompilowane przy użyciu poprzednich wersji ASP.NET MVC, Otwórz główny plik Web. config i Dodaj następujące trzy elementy *bindingRedirect* w sekcji *konfiguracji* : 

    [!code-xml[Main](mvc4-release-notes/samples/sample5.xml)]

<a id="_Toc303253817"></a>
## <a name="changes-from-aspnet-mvc-4-release-candidate"></a>Zmiany z wersji ASP.NET MVC 4 Release Candidate

Informacje o wersji dla ASP.NET MVC 4 Release Candidate można znaleźć tutaj:

Najważniejsze zmiany z programu ASP.NET MVC 4 Release Candidate w tej wersji zostały przedstawione poniżej:

- **Konfiguracja na kontroler:** Kontrolery interfejsu API sieci Web ASP.NET można przypisaną atrybutem niestandardowym, który implementuje *IControllerConfiguration* , aby skonfigurować własne elementy formatujące, selektor akcji i powiązania parametrów. *HttpControllerConfigurationAttribute* został usunięty.
- **Procedury obsługi komunikatów na trasie:** Teraz można określić program obsługi komunikatów końcowych w łańcuchu żądań dla danej trasy. Dzięki temu można obsługiwać platformy nadające się do pracy przy użyciu routingu do wysyłania do ich własnych punktów końcowych (innych niż*IHttpController*).
- **Powiadomienia o postępie:** *ProgressMessageHandler* generuje powiadomienie o postępie dla załadowanej jednostki żądania i pobieranych jednostek odpowiedzi. Przy użyciu tej procedury obsługi można śledzić, jak daleko przekazujesz treść żądania lub pobierając treść odpowiedzi.
- **Wypchnij zawartość:** Klasa *PushStreamContent* umożliwia scenariusze, w których producent danych chce pisać bezpośrednio do żądania lub odpowiedzi (synchronicznie lub asynchronicznie) przy użyciu strumienia. Gdy *PushStreamContent* jest gotowa do akceptowania danych wywoływanych przez obiekt delegowany akcji przy użyciu strumienia wyjściowego. Deweloper może następnie zapisywać w strumieniu tak długo, jak to konieczne, i zamknąć strumień po zakończeniu zapisywania. *PushStreamContent* wykrywa zamknięcie strumienia i kończy bazowe *zadanie* asynchroniczne do zapisywania zawartości.
- **Tworzenie odpowiedzi na błędy:** Użyj typu *HttpError* , aby spójnie reprezentować informacje o błędach, takie jak błędy i wyjątki walidacji, przy jednoczesnym zachowaniu *IncludeErrorDetailPolicy*. Nowe metody rozszerzenia *CreateErrorResponse* umożliwiają łatwe tworzenie odpowiedzi błędów z *HttpError* jako zawartość. Zawartość *HttpError* jest w pełni negocjowana.
- **MediaRangeMapping usunięte:** Zakresy typów multimediów są teraz obsługiwane przez negocjowanie zawartości domyślnej.
- **Domyślne powiązanie parametrów dla parametrów typu prostego to teraz [FromUri]:** W poprzednich wersjach interfejsu API sieci Web ASP.NET domyślne powiązanie parametrów dla prostych parametrów typu używało powiązania modelu. Domyślne powiązanie parametrów dla parametrów typu prostego to teraz *[FromUri]* .
- **Wybór akcji uwzględnia wymagane parametry:** Wybór akcji w interfejsie API sieci Web ASP.NET spowoduje teraz wybranie akcji tylko wtedy, gdy podano wszystkie wymagane parametry, które pochodzą z identyfikatora URI. Parametr może być określony jako opcjonalny przez podanie wartości domyślnej argumentu w podpisie metody akcji.
- **Dostosowywanie powiązań parametrów http:** Użyj *ParameterBindingAttribute* , aby dostosować powiązanie parametrów dla określonego parametru akcji lub użyć *ParameterBindingRules* na *HttpConfiguration* , aby lepiej dostosować powiązania parametrów.
- **Udoskonalenia MediaTypeFormatter:** Elementy formatujące mają teraz dostęp do pełnego wystąpienia *HttpContent* .
- **Wybór zasad buforowania hosta:** Zaimplementuj i skonfiguruj usługę *IHostBufferPolicySelector* w interfejsie API sieci Web ASP.NET, aby umożliwić hostom określenie zasad dotyczących używania buforowania.
- **Dostęp do certyfikatów klienta w niezależny od hosta:** Użyj metody rozszerzenia *GetClientCertificate* , aby uzyskać dostarczony certyfikat klienta z komunikatu żądania.
- **Rozszerzalność negocjacji zawartości:** Dostosuj negocjowanie zawartości, wynosząc z *DefaultContentNegotiator* i zastępując dowolny aspekt negocjacji zawartości, którą chcesz.
- **Obsługa powrotu do 406 niedopuszczalnych odpowiedzi:** Teraz można zwrócić 406 niedopuszczalne odpowiedzi w interfejsie API sieci Web ASP.NET, jeśli nie można odnaleźć odpowiedniego programu formatującego przez utworzenie *DefaultContentNegotiator* z parametrem *excludeMatchOnTypeOnly* ustawionym na *wartość true*.
- **Odczytaj dane formularza jako NameValueCollection lub JToken:** Dane formularza można odczytywać w ciągu zapytania identyfikatora URI lub w treści żądania jako *NameValueCollection* przy użyciu odpowiednio metod rozszerzenia *ParseQueryString* i *ReadAsFormDataAsync* . Podobnie można odczytywać dane formularza w ciągu zapytania identyfikatora URI lub w treści żądania jako *JToken* przy użyciu odpowiednio metod rozszerzenia *TryReadQueryAsJson* i *ReadAsAsync*&lt;&gt; t.
- **Ulepszenia wieloczęściowe:** Teraz można napisać *MultipartStreamProvider* , który jest całkowicie dostosowany do typu wieloczęściowych danych MIME, które może odczytać i przedstawić wynik w optymalny sposób dla użytkownika. Możesz również podpiąć krok przetwarzania po przetworzeniu na *MultipartStreamProvider* , który umożliwia implementację niezależnie od tego, czy będzie ona przetwarzać elementy treści wieloczęściowej MIME. Na przykład implementacja *MultipartFormDataStreamProvider* odczytuje elementy danych formularza HTML i dodaje je do *NameValueCollection* , dzięki czemu można je łatwo uzyskać od obiektu wywołującego.
- **Ulepszenia generowania linków:** *UrlHelper* nie zależy już od *HttpControllerContext*. Teraz możesz uzyskiwać dostęp do *UrlHelper* z dowolnego kontekstu, w którym *HttpRequestMessage* jest dostępny.
- **Zmiana kolejności wykonywania obsługi komunikatów:** Procedury obsługi komunikatów są teraz wykonywane w kolejności, w jakiej są skonfigurowane, a nie w odwrotnej kolejności.
- **Pomocnik dla okablowania obsługi komunikatów:** Nowy *HttpClientFactory* , który może nawiązać połączenie *DelegatingHandlers* i utworzyć *HttpClient* z żądanym potokiem gotowym do użycia. Zapewnia również funkcjonalność dla okablowania z alternatywnymi wewnętrznymi obsłudze (wartość domyślna to *HttpClientHandler*), a także wykonuje okablowanie w przypadku używania *HttpMessageInvoker* lub innego *DelegatingHandler* zamiast *HttpClient* jako Top-źródło.
- **Obsługa sieci CDN w optymalizacji sieci Web ASP.NET:** Optymalizacja sieci Web ASP.NET teraz zapewnia obsługę alternatywnych ścieżek usługi CDN, co pozwala określić dla każdego pakietu dodatkowy adres URL wskazujący ten sam zasób w sieci dostarczania zawartości. Obsługa sieci CDN umożliwia uzyskanie bardziej zbliżonego do użytkowników końcowych elementów w postaci skryptów i stylów w oddzielnym znaczeniu w stosunku do konsumentów aplikacji sieci Web. Aplikacje produkcyjne powinny implementować rezerwę, gdy sieć CDN jest niedostępna. Przetestuj rezerwę.
- **ASP.NET trasy i konfigurację interfejsu API sieci Web są przenoszone do *WebApiConfig. Register* static Metoda, którą można ponownie przystąpić w kodzie testowym.** Trasy interfejsu API sieci Web ASP.NET zostały wcześniej dodane w *RouteConfig. RegisterRoutes* wraz ze standardowymi trasami MVC. Domyślne trasy i konfiguracje interfejsu API sieci Web ASP.NET są teraz obsługiwane w oddzielnym metodzie *WebApiConfig. Register* w celu ułatwienia testowania.

<a id="_Toc303253815"></a>
## <a name="known-issues-and-breaking-changes"></a>Znane problemy i istotne zmiany

- **Wersje RC i RTM ASP.NET MVC 4 niepoprawnie zwracały zbuforowane widoki pulpitu, gdy należy zwrócić widoki mobilne.**

    - Aby uzyskać szczegółowe informacje na temat poprawki, zobacz [ASP.NET MVC 4 Mobile buforowania](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx) . Poprawkę można zainstalować ze stałym pakietem NuGet [DisplayModes](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) .
- Istotne **zmiany w aparacie widoku Razor**. Następujące typy zostały usunięte z *System. Web. MVC. Razor*: 

    - *ModelSpan*
    - *MvcVBRazorCodeGenerator*
    - *MvcCSharpRazorCodeGenerator*
    - *MvcVBRazorCodeParser*

  Zostały również usunięte następujące metody: 

    - *MvcCSharpRazorCodeParser. ParseInheritsStatement (System. Web. Razor. parser. CodeBlockInfo)*
    - *MvcWebPageRazorHost. DecorateCodeGenerator (System. Web. Razor. Generator. RazorCodeGenerator)*
    - *MvcVBRazorCodeParser. ParseInheritsStatement (System. Web. Razor. parser. CodeBlockInfo)*
- **Gdy WebMatrix. webdata. dll znajduje się w katalogu/bin. aplikacji ASP.NET MVC 4, przejmuje adres URL uwierzytelniania formularzy.** Dodawanie zestawu WebMatrix. webdata. dll do aplikacji (na przykład przez wybranie opcji "ASP.NET strony sieci Web ze składnią Razor" w przypadku użycia okna dialogowego Dodawanie zależności do wdrożenia) spowoduje przesłonięcie przekierowania logowania uwierzytelniania do/Account/Logon, a nie/Account/Login zgodnie z oczekiwaniami przez domyślny kontroler kont ASP.NET MVC. Aby uniknąć tego zachowania i używać adresu URL określonego już w sekcji uwierzytelnianie pliku Web. config, można dodać obiekt element appSetting o nazwie PreserveLoginUrl i ustawić dla niego wartość true: 

    [!code-xml[Main](mvc4-release-notes/samples/sample6.xml)]
- **Nie można zainstalować Menedżera pakietów NuGet podczas próby zainstalowania ASP.NET MVC 4 dla instalacji równoległych programów Visual Studio 2010 i Visual Web Developer 2010.** Aby uruchomić program Visual Studio 2010 i Visual Web Developer 2010 obok ASP.NET MVC 4, należy zainstalować ASP.NET MVC 4 po zainstalowaniu obu wersji programu Visual Studio.
- **Odinstalowywanie ASP.NET MVC 4 kończy się niepowodzeniem, jeśli wstępnie wymagane składniki zostały już odinstalowane.** Aby oczyścić program ASP.NET MVC 4You, należy odinstalować ASP.NET MVC 4 przed odinstalowaniem programu Visual Studio.
- **Instalowanie ASP.NET MVC 4 ASP.NET aplikacji MVC 3 RTM.** Aplikacje ASP.NET MVC 3, które zostały utworzone przy użyciu wydania RTM (nie z [aktualizacją ASP.NET MVC 3 Tools](https://www.microsoft.com/download/details.aspx?id=1491) Release), wymagają następujących zmian, aby działały równolegle z ASP.NET MVC 4. Kompilowanie projektu bez wprowadzania tych aktualizacji powoduje błędy kompilacji. 

    **Wymagane aktualizacje**

  1. W głównym pliku Web. config Dodaj nowy *&lt;appSettings&gt;* wpis przy użyciu najważniejszych *stron sieci Web: wersja* i wartość *1.0.0.0*. 

      [!code-xml[Main](mvc4-release-notes/samples/sample7.xml)]
  2. W Eksplorator rozwiązań kliknij prawym przyciskiem myszy nazwę projektu, a następnie wybierz polecenie Zwolnij projekt. Następnie ponownie kliknij prawym przyciskiem myszy nazwę i wybierz polecenie Edytuj *ProjectName*. csproj.
  3. Znajdź następujące odwołania do zestawu: 

      [!code-xml[Main](mvc4-release-notes/samples/sample8.xml)]

      Zamień je na następujące elementy:

      [!code-xml[Main](mvc4-release-notes/samples/sample9.xml)]
  4. Zapisz zmiany, Zamknij edytowany plik projektu (. csproj), a następnie kliknij prawym przyciskiem myszy projekt i wybierz pozycję Załaduj ponownie.

- **Zmiana projektu ASP.NET MVC 4 na wartość docelową 4,0 z 4,5 nie aktualizuje odwołania do zestawu EntityFramework:** Jeśli zmienisz projekt ASP.NET MVC 4 na wartość Target 4,0 po odwołujących 4,5, odwołanie do zestawu EntityFramework będzie nadal wskazywało na wersję 4,5. Aby rozwiązać ten problem, Odinstaluj i ponownie zainstaluj pakiet NuGet EntityFramework.
- **403 zabronione w przypadku uruchamiania aplikacji ASP.NET MVC 4 na platformie Azure po zmianie na element docelowy 4,0 z 4,5:** Jeśli zmienisz projekt ASP.NET MVC 4 na wartość Target 4,0 po odwołujących 4,5, a następnie wdrożono go na platformie Azure, w czasie wykonywania może zostać wyświetlony błąd 403 zabroniony. Aby obejść ten problem, Dodaj następujący plik do pliku Web. config: `<modules runAllManagedModulesForAllRequests="true" />`
- **Program Visual Studio 2012 ulega awarii po wpisaniu "\' w literale ciągu w pliku Razor.** Aby obejść ten problem, wprowadź najpierw cudzysłów zamykający literału ciągu.
- <strong>Przechodzenie do &quot;konta/Zarządzanie&quot; w szablonie internetowym skutkuje błędem środowiska uruchomieniowego dla języków CHS, ZMN i CHT.</strong> Aby rozwiązać ten problem, zmodyfikuj stronę, aby oddzielić <em>@User.Identity.Name</em> przez umieszczenie jej jako jedynej zawartości w tagu <em>&lt;silnego&gt;</em> .
- **Dostawcy usług Google i LinkedIn nie są obsługiwani w witrynach sieci Web systemu Azure.** W przypadku wdrażania w usłudze witryny sieci Web systemu Azure należy używać alternatywnych dostawców uwierzytelniania.
- **W przypadku korzystania z UriPathExtensionMapping z usługami IIS 8 Express/IIS w przypadku próby użycia rozszerzenia nie znaleziono 404 błędów.** Procedura obsługi plików statycznych będzie zakłócać żądania do interfejsów API sieci Web, które używają *UriPathExtensionMappings*. Aby obejść ten problem, ustaw wartość *runAllManagedModulesForAllRequests = true* w pliku Web. config.
- **Metoda Controller. Execute nie jest już wywoływana.** Wszystkie kontrolery MVC są teraz zawsze wykonywane asynchronicznie.

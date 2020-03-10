---
uid: whitepapers/mvc4-beta-release-notes
title: ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: W tym dokumencie opisano wydanie ASP.NET MVC 4 beta dla programu Visual Studio 2010.
ms.author: riande
ms.date: 09/09/2011
ms.assetid: 666407bb-81de-4319-89ba-0302c382a208
msc.legacyurl: /whitepapers/mvc4-beta-release-notes
msc.type: content
ms.openlocfilehash: 17800dfe091bbb7afb25f7f41e3bd885b882edb0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78523304"
---
# <a name="aspnet-mvc-4"></a>ASP.NET MVC w wersji 4

> W tym dokumencie opisano wydanie ASP.NET MVC 4 beta dla programu Visual Studio 2010.
> 
> > [!NOTE]
> > To nie jest Najnowsza wersja. Informacje o wersji ASP.NET MVC 4 RC są dostępne [tutaj](mvc4-release-notes.md).

- [Uwagi dotyczące instalacji](#_Toc303253802)
- [Dokumentacja](#_Toc303253803)
- [Pomoc techniczna](#_Toc303253804)
- [Wymagania dotyczące oprogramowania](#_Toc303253805)
- [Uaktualnianie projektu ASP.NET MVC 3 do ASP.NET MVC 4](#_Toc303253806)
- [Nowe funkcje w programie ASP.NET MVC 4 beta](#_Toc303253807)

    - [ASP.NET Web API](#_Toc317096197)
    - [ASP.NET aplikacji jednostronicowej](#_Toc317096198)
    - [Ulepszenia domyślnych szablonów projektów](#_Toc303253808)
    - [Szablon projektu mobilnego](#_Toc303253809)
    - [Tryby wyświetlania](#_Toc303253810)
    - [Aplikacje jQuery Mobile, przełącznik widoku i zastępowanie przeglądarki](#_Toc303253811)
    - [Przepisy dotyczące generowania kodu w programie Visual Studio](#_Toc303253812)
    - [Obsługa zadań dla kontrolerów asynchronicznych](#_Toc303253813)
    - [Zestaw Azure SDK](#_Toc303253814)
    - [Znane problemy i istotne zmiany](#_Toc303253815)

<a id="_Toc303253802"></a>
## <a name="installation-notes"></a>Uwagi dotyczące instalacji

ASP.NET MVC 4 beta for Visual Studio 2010 można zainstalować ze [strony głównej ASP.NET MVC 4](../mvc/mvc4.md) przy użyciu Instalatora platformy sieci Web.

Przed zainstalowaniem ASP.NET MVC 4 beta należy odinstalować wszystkie wcześniej zainstalowane wersje zapoznawcze programu ASP.NET MVC 4.

Ta wersja nie jest zgodna z .NET Framework 4,5 Developer Preview. Przed zainstalowaniem programu ASP.NET MVC 4 beta należy odinstalować program .NET 4,5 Developer Preview.

ASP.NET MVC 4 można zainstalować i uruchomić go równolegle z ASP.NET MVC 3.

<a id="_Toc303253803"></a>
## <a name="documentation"></a>Dokumentacja

Dokumentacja usługi ASP.NET MVC jest dostępna w witrynie MSDN pod następującym adresem URL:

[https://go.microsoft.com/fwlink/?LinkID=243043](https://go.microsoft.com/fwlink/?LinkID=243043)

Samouczki i inne informacje dotyczące ASP.NET MVC są dostępne na stronie MVC 4 witryny sieci Web ASP.NET ([https://www.asp.net/mvc/mvc4](../mvc/mvc4.md)).

<a id="_Toc303253804"></a>
## <a name="support"></a>Pomoc techniczna

To jest wersja zapoznawcza i nie jest oficjalnie obsługiwana. Jeśli masz pytania dotyczące pracy z tą wersją, Opublikuj je na forum ASP.NET MVC ([https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)), gdzie członkowie społeczności ASP.NET często mogą zapewnić nieformalne wsparcie.

<a id="_Toc303253805"></a>
## <a name="software-requirements"></a>Wymagania programowe

Składniki ASP.NET MVC 4 dla programu Visual Studio wymagają programu PowerShell 2,0 i programu Visual Studio 2010 z dodatkiem Service Pack 1 lub Visual Web Developer Express 2010 z dodatkiem Service Pack 1.

<a id="_Toc303253806"></a>
## <a name="upgrading-an-aspnet-mvc-3-project-to-aspnet-mvc-4"></a>Uaktualnianie projektu ASP.NET MVC 3 do ASP.NET MVC 4

ASP.NET MVC 4 można zainstalować równolegle z ASP.NET MVC 3 na tym samym komputerze, co zapewnia elastyczność w wyborze czasu uaktualnienia aplikacji ASP.NET MVC 3 do ASP.NET MVC 4.

Najprostszym sposobem uaktualnienia jest utworzenie nowego projektu ASP.NET MVC 4 i skopiowanie wszystkich widoków, kontrolerów, kodu i plików zawartości z istniejącego projektu MVC 3 do nowego projektu, a następnie zaktualizowanie odwołań do zestawu w nowym projekcie w celu dopasowania go do starego projektu. Jeśli wprowadzono zmiany w pliku Web. config w projekcie MVC 3, należy również scalić te zmiany w pliku Web. config w projekcie MVC 4.

Aby ręcznie uaktualnić istniejącą aplikację ASP.NET MVC 3 do wersji 4, wykonaj następujące czynności:

1. We wszystkich plikach Web. config w projekcie (istnieje jeden w katalogu głównym projektu, jeden w folderze widoki i jeden w folderze widoki dla każdego obszaru w projekcie), Zastąp każde wystąpienie następującego tekstu:

    [!code-console[Main](mvc4-beta-release-notes/samples/sample1.cmd)]

    z następującym odpowiednim tekstem:

    [!code-console[Main](mvc4-beta-release-notes/samples/sample2.cmd)]
2. W głównym pliku Web. config zaktualizuj elementy *Webpages: Version* do "2.0.0.0" i Dodaj nowy klucz *PreserveLoginUrl* , który ma wartość "true":

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample3.xml)]
3. W Eksplorator rozwiązań usuń odwołanie do *System. Web. MVC* (które wskazuje na bibliotekę DLL w wersji 3). Następnie Dodaj odwołanie do elementu *System. Web. MVC* (v 4.0.0.0). W szczególności wprowadź następujące zmiany, aby zaktualizować odwołania do zestawu. Oto szczegółowe informacje:

    1. W Eksplorator rozwiązań Usuń odwołania do następujących zestawów: 

        - *System. Web. MVC*(v 3.0.0.0)
        - *System. Web. Webpages*(v 1.0.0.0)
        - *System. Web. Razor*(v 1.0.0.0)
        - *System. Web. Webpages. Deployment*(v 1.0.0.0)
        - *System. Web. Webpages. Razor*(v 1.0.0.0)
    2. Dodaj odwołania do następujących zestawów: 

        - *System. Web. MVC*(v 4.0.0.0)
        - *System. Web. Webpages*(v 2.0.0.0)
        - *System. Web. Razor*(v 2.0.0.0)
        - *System. Web. Webpages. Deployment*(v 2.0.0.0)
        - *System. Web. Webpages. Razor*(v 2.0.0.0)
4. W Eksplorator rozwiązań kliknij prawym przyciskiem myszy nazwę projektu, a następnie wybierz polecenie Zwolnij projekt. Następnie ponownie kliknij prawym przyciskiem myszy nazwę i wybierz polecenie Edytuj *ProjectName*. csproj.
5. Znajdź element *ProjectTypeGuids* i Zastąp ciąg {E53F8FEA-EAE0-44A6-8774-FFD645390401} atrybutem {E3E379DF-F4C6-4180-9B81-6769533ABE47}.
6. Zapisz zmiany, Zamknij edytowany plik projektu (. csproj), kliknij prawym przyciskiem myszy projekt, a następnie wybierz polecenie Załaduj ponownie projekt.
7. Jeśli projekt odwołuje się do wszystkich bibliotek innych firm, które są kompilowane przy użyciu poprzednich wersji ASP.NET MVC, Otwórz główny plik Web. config i Dodaj następujące trzy elementy *bindingRedirect* w sekcji *konfiguracji* : 

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample4.xml)]

<a id="_Toc303253807"></a>
## <a name="new-features-in-aspnet-mvc-4-beta"></a>Nowe funkcje w programie ASP.NET MVC 4 beta

Ta sekcja zawiera opis funkcji wprowadzonych w wersji ASP.NET MVC 4 beta.

<a id="_Toc317096197"></a>
### <a name="aspnet-web-api"></a>Składnik Web API platformy ASP.NET

ASP.NET MVC 4 zawiera teraz internetowy interfejs API ASP.NET, nową strukturę tworzenia usług HTTP, która może dotrzeć do szerokiego zakresu klientów, w tym przeglądarek i urządzeń przenośnych. Interfejs API sieci Web ASP.NET jest również idealnym platformą do kompilowania usług RESTful.

Interfejs API sieci Web ASP.NET zapewnia obsługę następujących funkcji:

- **Nowoczesny model programowania http:** Bezpośredni dostęp do żądań i odpowiedzi HTTP w interfejsach API sieci Web oraz manipulowanie nimi przy użyciu nowego, silnie określonego modelu obiektów HTTP. Ten sam model programowania i potok HTTP są symetrycznie dostępne na kliencie za pośrednictwem nowego typu HttpClient.
- **Pełna obsługa tras**: interfejsy API sieci Web obsługują teraz pełen zestaw możliwości trasy, które były zawsze częścią stosu sieci Web, w tym parametry tras i ograniczenia. Ponadto mapowanie do akcji ma pełną obsługę Konwencji, więc nie trzeba już stosować atrybutów takich jak [HttpPost] do klas i metod.
- **Negocjowanie zawartości**: klient i serwer mogą współdziałać ze sobą, aby określić właściwy format danych zwracanych z interfejsu API. Zapewniamy domyślną obsługę formatów XML, JSON i notacji z adresami URL, a także możesz ją rozciągnąć przez dodanie własnych elementów formatujących, a nawet zastąpienie domyślnej strategii negocjowania zawartości.
- **Powiązanie i walidacja modelu:** Powiązania modelu zapewniają łatwy sposób wyodrębniania danych z różnych części żądania HTTP i przekształcania tych części komunikatów na obiekty .NET, które mogą być używane przez akcje interfejsu API sieci Web.
- **Filtry:** Interfejsy API sieci Web obsługują teraz filtry, w tym dobrze znane filtry, takie jak atrybut [autoryzuje]. Można tworzyć i dołączać własne filtry do akcji, autoryzacji i obsługi wyjątków.
- **Kompozycja zapytania:** Po prostu zwracając wartość IQueryable&lt;T&gt;, internetowy interfejs API będzie obsługiwał zapytania za pośrednictwem Konwencji adresów URL OData.
- **Ulepszono testowanie szczegółów protokołu http:** Zamiast ustawiania szczegółów protokołu HTTP w statycznych obiektach kontekstu, akcje interfejsu API sieci Web mogą teraz współdziałać z wystąpieniami HttpRequestMessage i HttpResponseMessage. Ogólne wersje tych obiektów także istnieją, aby umożliwić współpracę z typami niestandardowymi w dodatku do typów HTTP.
- **Ulepszona wersja kontroli (IOC) za pośrednictwem DependencyResolver:** Interfejs API sieci Web używa teraz wzorca lokalizatora usługi zaimplementowanego przez program rozpoznawania zależności MVC do uzyskiwania wystąpień dla wielu różnych obiektów.
- **Konfiguracja oparta na kodzie:** Konfiguracja interfejsu API sieci Web jest realizowana wyłącznie za poorednictwem kodu, pozostawiając czyszczenie plików konfiguracji.
- **Samodzielne hostowanie:** Interfejsy API sieci Web mogą być hostowane we własnym procesie oprócz usług IIS i nadal korzystają z pełnych możliwości tras i innych funkcji interfejsu API sieci Web.

Aby uzyskać więcej informacji na temat interfejsu Web API ASP.NET, odwiedź stronę [https://www.asp.net/web-api](../web-api/index.md).

<a id="_Toc317096198"></a>
### <a name="aspnet-single-page-application"></a>ASP.NET aplikacji jednostronicowej

ASP.NET MVC 4 zawiera teraz wczesną wersję zapoznawczą środowiska tworzenia aplikacji jednostronicowych z znaczącymi interakcjami po stronie klienta przy użyciu języka JavaScript i interfejsów API sieci Web. Ta obsługa obejmuje:

- Zestaw bibliotek języka JavaScript na potrzeby bogatszych lokalnych interakcji z danymi buforowanymi
- Dodatkowe składniki internetowego interfejsu API do obsługi jednostek pracy i DAL
- Szablon projektu MVC z szkieletem umożliwiającym szybkie rozpoczynanie pracy

Aby uzyskać więcej informacji na temat obsługi aplikacji jednostronicowych w ASP.NET MVC 4, odwiedź stronę [https://www.asp.net/single-page-application](../single-page-application/index.md).

<a id="_Toc303253808"></a>
### <a name="enhancements-to-default-project-templates"></a>Ulepszenia domyślnych szablonów projektów

Szablon służący do tworzenia nowych projektów ASP.NET MVC 4 został zaktualizowany w celu utworzenia bardziej nowoczesnej witryny sieci Web:

![](mvc4-beta-release-notes/_static/image1.png)

Oprócz ulepszeń kosmetycznych udoskonalono funkcjonalność nowego szablonu. Szablon wykorzystuje technikę o nazwie renderowanie adaptacyjne, aby wyglądać dobrze w przeglądarkach klasycznych i przeglądarkach dla urządzeń przenośnych bez żadnego dostosowania.

![](mvc4-beta-release-notes/_static/image2.png)

Aby zobaczyć adaptacyjne renderowanie w akcji, można użyć emulatora urządzenia przenośnego lub po prostu spróbować zmienić rozmiar okna przeglądarki pulpitu, aby było mniejsze. Gdy okno przeglądarki jest wystarczająco małe, układ strony zmieni się.

Innym ulepszeniem szablonu projektu domyślnego jest użycie języka JavaScript w celu zapewnienia bogatszego interfejsu użytkownika. Linki logowania i rejestrowania, które są używane w szablonie, to przykłady użycia okna dialogowego interfejsu użytkownika jQuery do zaprezentowania rozbudowanego ekranu logowania:

![](mvc4-beta-release-notes/_static/image3.png)

<a id="_Toc303253809"></a>
### <a name="mobile-project-template"></a>Szablon projektu mobilnego

Jeśli uruchamiasz nowy projekt i chcesz utworzyć witrynę specyficzną dla przeglądarek mobilnych i tabletów, możesz użyć nowego szablonu projektu aplikacji mobilnej. Jest to oparte na technologii jQuery Mobile, biblioteki Open Source do tworzenia interfejsu użytkownika zoptymalizowanego pod kątem obsługi dotykowej:

![](mvc4-beta-release-notes/_static/image4.png)

Ten szablon zawiera tę samą strukturę aplikacji, co szablon aplikacji internetowej (a kod kontrolera jest praktycznie identyczny), ale jest on w stylu przy użyciu technologii jQuery Mobile, aby uzyskać dobre i dobrze działać na urządzeniach przenośnych opartych na dotykach. Aby dowiedzieć się więcej na temat struktury i stylu mobilnego interfejsu użytkownika, zobacz [witrynę sieci Web programu jQuery Mobile](http://jquerymobile.com/).

Jeśli masz już lokację zorientowaną na komputery stacjonarne, do której chcesz dodać widoki zoptymalizowane pod kątem urządzeń przenośnych, lub jeśli chcesz utworzyć pojedynczą lokację, która obsługuje widoki w różnych stylach, można użyć nowych trybów wyświetlania. (Zobacz następną sekcję).

<a id="_Toc303253810"></a>
### <a name="display-modes"></a>Tryby wyświetlania

Nowa funkcja trybów wyświetlania umożliwia aplikacji Wybieranie widoków w zależności od przeglądarki, która żąda żądania. Na przykład jeśli przeglądarka pulpitu żąda strony głównej, aplikacja może używać szablonu Views\Home\Index.cshtml. Jeśli przeglądarka mobilna żąda strony głównej, aplikacja może zwrócić szablon Views\Home\Index.mobile.cshtml.

Układy i częściowe mogą być również zastępowane dla określonych typów przeglądarek. Na przykład:

- Jeśli folder Views\Shared zawiera zarówno szablon \_Layout. cshtml, jak i \_Layout. Mobile. cshtml, domyślnie aplikacja będzie używać \_układ. Mobile. cshtml podczas żądań z przeglądarek mobilnych i \_Layout. cshtml podczas innych żądań.
- Jeśli folder zawiera zarówno \_. cshtml, jak i \_. Mobile. cshtml, instrukcja @Html.Partial("\_") będzie renderowana \_. Mobile. cshtml w trakcie żądań z przeglądarek mobilnych i \_część. cshtml podczas innych żądań.

Jeśli chcesz utworzyć bardziej szczegółowe widoki, układy lub częściowe widoki dla innych urządzeń, możesz zarejestrować nowe wystąpienie usługi *DefaultDisplayMode* , aby określić, która nazwa ma być wyszukiwana, gdy żądanie spełnia określone warunki. Na przykład można dodać następujący kod do *aplikacji\_Start* metody w pliku Global. asax, aby zarejestrować ciąg "iPhone" jako tryb wyświetlania, który ma zastosowanie, gdy przeglądarka telefonu iPhone wysyła żądanie:

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample5.cs)]

Po uruchomieniu tego kodu, gdy przeglądarka telefonów iPhone firmy Apple wyśle żądanie, aplikacja użyje układu Views\Shared\\_Layout. iPhone. cshtml (jeśli istnieje).

<a id="_Toc303253811"></a>
### <a name="jquery-mobile-the-view-switcher-and-browser-overriding"></a>Aplikacje jQuery Mobile, przełącznik widoku i zastępowanie przeglądarki

jQuery Mobile to Biblioteka open source służąca do tworzenia interfejsu użytkownika sieci Web zoptymalizowanej pod kątem dotyku. Jeśli chcesz używać platformy jQuery Mobile z aplikacją ASP.NET MVC 4, możesz pobrać i zainstalować pakiet NuGet, który pomoże Ci rozpocząć pracę. Aby zainstalować ją z poziomu konsoli Menedżera pakietów programu Visual Studio, wpisz następujące polecenie:

[!code-powershell[Main](mvc4-beta-release-notes/samples/sample6.ps1)]

Spowoduje to zainstalowanie oprogramowania jQuery Mobile i niektórych plików pomocnika, w tym następujących:

- Widoki/Shared/\_Layout. Mobile. cshtml, który jest układem opartym na urządzeniach przenośnych jQuery.
- Składnik-przełącznik widoku, który składa się z widoku częściowego widoków/Shared/\_ViewSwitcher. cshtml i kontrolera ViewSwitcherController.cs.

Po zainstalowaniu pakietu Uruchom aplikację przy użyciu przeglądarki mobilnej (lub równoważnej, takiej jak dodatek [User Agent agenta](http://chrispederick.com/work/user-agent-switcher/) Firefox). Zobaczysz, że strony wyglądają zupełnie inaczej, ponieważ jQuery Mobile obsługuje układ i style. Aby skorzystać z tej możliwości, można wykonać następujące czynności:

- Tworzenie zastąpień widoku specyficznych dla urządzeń przenośnych zgodnie z opisem w obszarze [tryby wyświetlania](#_Toc303253810) wcześniej (na przykład utwórz Views\Home\Index.Mobile.cshtml, aby zastąpić Views\Home\Index.cshtml dla przeglądarek mobilnych).
- Zapoznaj się z [dokumentacją programu jQuery Mobile](http://jquerymobile.com/) , aby dowiedzieć się więcej na temat dodawania elementów interfejsu użytkownika zoptymalizowanych pod kątem technologii w widokach mobilnych.

Konwencja dla stron sieci Web zoptymalizowanych pod kątem urządzeń przenośnych polega na dodaniu linku, którego tekst jest taki sam jak widok pulpitu lub tryb pełnej witryny, który umożliwia użytkownikom przełączanie do wersji klasycznej strony. Pakiet jQuery. Mobile. MVC zawiera przykładowy składnik View-przełącznik. Jest on używany w domyślnym widoku Views\Shared\\_Layout. Mobile. cshtml i wygląda następująco, gdy strona jest renderowana:

![](mvc4-beta-release-notes/_static/image5.png)

Jeśli Goście kliknie link, zostaną przełączeni do wersji klasycznej tej samej strony.

Ponieważ układ pulpitu nie obejmuje domyślnie przełącznika widoku, odwiedzający nie będą mogli przejść do trybu mobilnego. Aby włączyć tę funkcję, Dodaj następujące odwołanie do *\_ViewSwitcher* do układu pulpitu, po prostu wewnątrz *&lt;Body&gt;* elementu:

[!code-cshtml[Main](mvc4-beta-release-notes/samples/sample7.cshtml)]

Przełącznik widoku używa nowej funkcji o nazwie "zastępowanie przeglądarki". Ta funkcja umożliwia aplikacji traktowanie żądań tak, jakby znajdowały się one z innej przeglądarki (agent użytkownika) niż ta, z której pochodzą. Poniższa tabela zawiera listę metod, które zastępują w przeglądarce.

| `HttpContext.SetOverriddenBrowser(userAgentString)` | Przesłania rzeczywistą wartość agenta użytkownika żądania przy użyciu określonego agenta użytkownika. |
| --- | --- |
| `HttpContext.GetOverriddenUserAgent()` | Zwraca wartość zastąpienia agenta użytkownika żądania lub faktyczny ciąg agenta użytkownika, jeśli nie określono przesłonięcia. |
| `HttpContext.GetOverriddenBrowser()` | Zwraca wystąpienie *HttpBrowserCapabilitiesBase* , które odnosi się do agenta użytkownika, który jest aktualnie ustawiony dla żądania (wartość rzeczywista lub zastąpiona). Możesz użyć tej wartości, aby uzyskać właściwości, takie jak *IsMobileDevice*. |
| `HttpContext.ClearOverriddenBrowser()` | Usuwa wszelkich przesłoniętych agentów użytkownika dla bieżącego żądania. |

Zastępowanie przeglądarki jest podstawową funkcją ASP.NET MVC 4 i jest dostępne nawet wtedy, gdy nie zostanie zainstalowany pakiet jQuery. Mobile. MVC. Dotyczy to jednak tylko opcji Widok, układ i widok częściowy — nie ma to wpływu na żadną inną funkcję ASP.NET, która zależy od obiektu *Request. Browser* .

Domyślnie przesłonięcie agenta użytkownika jest przechowywane przy użyciu pliku cookie. Jeśli chcesz przechowywać przesłonięcie w innym miejscu (na przykład w bazie danych), możesz zastąpić domyślnego dostawcę (*BrowserOverrideStores. Current*). Dokumentacja dla tego dostawcy będzie dostępna do nowszej wersji ASP.NET MVC.

<a id="_Toc303253812"></a>
### <a name="recipes-for-code-generation-in-visual-studio"></a>Przepisy dotyczące generowania kodu w programie Visual Studio

Nowa funkcja przepisów umożliwia programowi Visual Studio generowanie kodu specyficznego dla rozwiązania na podstawie pakietów, które można zainstalować za pomocą narzędzia NuGet. Przepisy ramowe ułatwiają deweloperom pisanie wtyczek do generowania kodu, których można również użyć do zastępowania wbudowanych generatorów kodu dla Dodaj obszar, Dodaj kontroler i Dodaj widok. Ponieważ przepisy są wdrażane jako pakiety NuGet, można je łatwo zaewidencjonować do kontroli źródła i udostępnić wszystkim deweloperom w projekcie automatycznie. Są one również dostępne dla poszczególnych rozwiązań.

<a id="_Toc303253813"></a>
### <a name="task-support-for-asynchronous-controllers"></a>Obsługa zadań dla kontrolerów asynchronicznych

Teraz można pisać metody akcji asynchronicznych jako pojedyncze metody, które zwracają obiekt typu *zadanie* lub *zadanie&lt;ActionResult&gt;* .

Na przykład jeśli używasz programu Visual C# 5 (lub za pomocą [Async CTP](https://msdn.microsoft.com/vstudio/async.aspx)), możesz utworzyć asynchroniczną metodę akcji, która wygląda następująco:

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample8.cs)]

W poprzedniej metodzie działania wywołania do *newsService. GetHeadlinesAsync* i *sportsService. GetScoresAsync* są nazywane asynchronicznie i nie blokują wątku z puli wątków.

Asynchroniczne metody akcji, które zwracają wystąpienia *zadań* , mogą również obsługiwać limity czasu. Aby można było anulować metodę akcji, Dodaj parametr typu *CancellationToken* do sygnatury metody akcji. W poniższym przykładzie przedstawiono metodę akcji asynchronicznej, która ma limit czasu 2500 milisekund, który wyświetla widok *TimedOut* na kliencie w przypadku wystąpienia limitu czasu.

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample9.cs)]

<a id="_Toc303253814"></a>
### <a name="azure-sdk"></a>Azure SDK

ASP.NET MVC 4 beta obsługuje wydanie zestawu Windows Azure SDK z września 2011 1,5.

<a id="_Toc303253815"></a>
## <a name="known-issues-and-breaking-changes"></a>Znane problemy i istotne zmiany

- **Po zainstalowaniu programu ASP.NET MVC 4 beta Edytor CSHTML/VBHTML w programie Visual Studio 2010 z dodatkiem Service Pack 1 lub Edytor VBHTML może wstrzymywać się przez długi czas po wpisaniu fragmentu kodu lub JavaScript w plikach cshtml lub VBHTML.** Dzieje się tak tylko w przypadku aplikacji ASP.NET MVC 4, które zostały już utworzone i nie zostały jeszcze skompilowane.

    Obejście polega na skompilowaniu projektu w celu pobrania zestawów w folderze bin. Zwróć uwagę, że jeśli wyczyścisz projekt, który usuwa zestawy z folderu bin, problem z edytorem zostanie przywrócony.

    Ta poprawka zostanie poprawiona w następnej wersji.
- **C#Szablony projektów dla programu Visual Studio 11 Beta zawierają nieprawidłowe parametry połączenia w Global.asax.cs.** Domyślne połączenie określone w\_metodzie uruchamiania aplikacji dla projektów utworzonych w programie Visual Studio 11 Beta zawiera LocalDB parametry połączenia, które zawierają niezmieniony ukośnik odwrotny (\) znak. Powoduje to błąd połączenia przy próbie uzyskania dostępu do Entity Framework DbContext, który generuje SqlException.

    Aby rozwiązać ten problem, należy użyć znaku ucieczki odwróconej kreski ułamkowej w aplikacji\_Metoda startowa Global.asax.cs, tak aby odczytana w następujący sposób:

    [!code-csharp[Main](mvc4-beta-release-notes/samples/sample10.cs)]
- **Aplikacje ASP.NET MVC 4, które są przeznaczone dla platformy .NET 4,5, zgłaszają FileLoadException przy próbie uzyskania dostępu do zestawu System. NET. http. dll, gdy jest uruchamiany w ramach platformy .NET 4,0.** Aplikacje ASP.NET MVC 4 utworzone w ramach platformy .NET 4,5 zawierają przekierowanie powiązania, co spowoduje, że FileLoadException, którego stan "nie można załadować pliku lub zestawu" System .NET. http "lub jeden z jego zależności." gdy aplikacja jest wykonywana w systemie z zainstalowanym programem .NET 4,0. Aby rozwiązać ten problem, Usuń następujące przekierowanie powiązania z pliku Web. config:

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample11.xml)]

    Element powiązania zestawu w zmodyfikowanym pliku Web. config powinien wyglądać następująco:

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample12.xml)]
- <strong>Szablon elementu "Dodaj kontroler" w projektach Visual Basic generuje nieprawidłową przestrzeń nazw w przypadku wywołania</strong><strong>z wnętrza obszaru.</strong> Po dodaniu kontrolera do obszaru w projekcie ASP.NET MVC, który używa Visual Basic, szablon elementu wstawia nieprawidłową przestrzeń nazw do kontrolera. W wyniku przechodzenia do dowolnej akcji w kontrolerze Wystąpił błąd "nie znaleziono pliku".  
  
  Wygenerowana przestrzeń nazw pomija wszystkie elementy po głównej przestrzeni nazw. Na przykład wygenerowana przestrzeń nazw to *RootNamespace* , ale powinna mieć wartość *RootNamespace. Areas. Areaname. controllers* .
- **Istotne zmiany w aparacie widoku Razor.** W ramach ponownego zapisu analizatora składni Razor następujące typy zostały usunięte z elementu *System. Web. MVC. Razor*: 

    - *ModelSpan*
    - *MvcVBRazorCodeGenerator*
    - *MvcCSharpRazorCodeGenerator*
    - *MvcVBRazorCodeParser*

  Zostały również usunięte następujące metody: 

    - *MvcCSharpRazorCodeParser. ParseInheritsStatement (System. Web. Razor. parser. CodeBlockInfo)*
    - *MvcWebPageRazorHost. DecorateCodeGenerator (System. Web. Razor. Generator. RazorCodeGenerator)*
    - *MvcVBRazorCodeParser. ParseInheritsStatement (System. Web. Razor. parser. CodeBlockInfo)*
- **Gdy WebMatrix. webdata. dll znajduje się w katalogu/bin. aplikacji ASP.NET MVC 4, przejmuje adres URL uwierzytelniania formularzy.** Dodawanie zestawu WebMatrix. webdata. dll do aplikacji (na przykład przez wybranie opcji "ASP.NET strony sieci Web ze składnią Razor" w przypadku użycia okna dialogowego Dodawanie zależności do wdrożenia) spowoduje przesłonięcie przekierowania logowania uwierzytelniania do/Account/Logon, a nie/Account/Login zgodnie z oczekiwaniami przez domyślny kontroler kont ASP.NET MVC. Aby uniknąć tego zachowania i używać adresu URL określonego już w sekcji uwierzytelnianie pliku Web. config, można dodać obiekt element appSetting o nazwie PreserveLoginUrl i ustawić dla niego wartość true: 

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample13.xml)]
- **Nie można zainstalować Menedżera pakietów NuGet podczas próby zainstalowania ASP.NET MVC 4 dla instalacji równoległych programów Visual Studio 2010 i Visual Web Developer 2010.** Aby uruchomić program Visual Studio 2010 i Visual Web Developer 2010 obok ASP.NET MVC 4, należy zainstalować ASP.NET MVC 4 po zainstalowaniu obu wersji programu Visual Studio.
- **Odinstalowywanie ASP.NET MVC 4 kończy się niepowodzeniem, jeśli wstępnie wymagane składniki zostały już odinstalowane.** Aby oczyścić program ASP.NET MVC 4You, należy odinstalować ASP.NET MVC 4 przed odinstalowaniem programu Visual Studio.
- **Uruchamianie domyślnego projektu internetowego interfejsu API zawiera instrukcje, które niepoprawnie kierują użytkownika do dodawania tras przy użyciu metody RegisterApis, która nie istnieje.** Trasy należy dodać w metodzie RegisterRoutes przy użyciu tabeli tras ASP.NET.
- **Instalowanie programu ASP.NET MVC 4 beta ASP.NET aplikacje MVC 3 RTM.** Aplikacje ASP.NET MVC 3, które zostały utworzone przy użyciu wersji RTM (nie z aktualizacją ASP.NET MVC 3 Tools Release), wymagają następujących zmian, aby działały równolegle z ASP.NET MVC 4 beta. Kompilowanie projektu bez wprowadzania tych aktualizacji powoduje błędy kompilacji. 

    **Wymagane aktualizacje**

  1. W głównym pliku Web. config Dodaj nowy *&lt;appSettings&gt;* wpis przy użyciu najważniejszych *stron sieci Web: wersja* i wartość *1.0.0.0*.

      [!code-xml[Main](mvc4-beta-release-notes/samples/sample14.xml)]
  2. W Eksplorator rozwiązań kliknij prawym przyciskiem myszy nazwę projektu, a następnie wybierz polecenie Zwolnij projekt. Następnie ponownie kliknij prawym przyciskiem myszy nazwę i wybierz polecenie Edytuj *ProjectName*. csproj.
  3. Znajdź następujące odwołania do zestawu: 

      [!code-xml[Main](mvc4-beta-release-notes/samples/sample15.xml)]

      Zamień je na następujące elementy:

      [!code-xml[Main](mvc4-beta-release-notes/samples/sample16.xml)]
  4. Zapisz zmiany, Zamknij edytowany plik projektu (. csproj), a następnie kliknij prawym przyciskiem myszy projekt i wybierz pozycję Załaduj ponownie.

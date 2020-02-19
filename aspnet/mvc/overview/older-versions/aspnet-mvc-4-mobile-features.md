---
uid: mvc/overview/older-versions/aspnet-mvc-4-mobile-features
title: Funkcje mobilne ASP.NET MVC 4 | Microsoft Docs
author: Rick-Anderson
description: Dostępna jest teraz wersja MVC 5 tego samouczka z przykładami kodu w ramach wdrażania aplikacji sieci Web ASP.NET MVC 5 Mobile w witrynach sieci Web systemu Azure.
ms.author: riande
ms.date: 08/15/2012
ms.assetid: 27dc4fc8-1b51-43b0-933f-fc1b52476523
msc.legacyurl: /mvc/overview/older-versions/aspnet-mvc-4-mobile-features
msc.type: authoredcontent
ms.openlocfilehash: 9716def069ca9f7115af32e16381f41bd4d13342
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457651"
---
# <a name="aspnet-mvc-4-mobile-features"></a>Funkcje mobilne platformy ASP.NET MVC 4

Autor [Rick Anderson](https://twitter.com/RickAndMSFT)

> Dostępna jest teraz wersja MVC 5 tego samouczka z przykładami kodu w ramach [wdrażania aplikacji sieci web ASP.NET MVC 5 Mobile w witrynach sieci Web systemu Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/).

Ten samouczek zawiera informacje na temat sposobu pracy z funkcjami mobilnymi w aplikacji sieci Web ASP.NET MVC 4. W tym samouczku można użyć [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) lub Visual Web Developer 2010 Express Service Pack 1 (&quot;Visual Web Developer lub pliku VWD&quot;). Możesz użyć profesjonalnej wersji programu Visual Studio, jeśli masz już tę.

Przed rozpoczęciem upewnij się, że zainstalowano wymagania wstępne wymienione poniżej.

- [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) (zalecane) lub Visual Studio Web Developer Express SP1. Visual Studio 2012 contains ASP.NET MVC 4. W przypadku korzystania z programu Visual Web Developer 2010 należy zainstalować [ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392).

Potrzebny będzie również emulator przeglądarki mobilnej. Zostaną wykonane następujące czynności:

- [Emulator telefonu systemu Windows 7](https://msdn.microsoft.com/library/ff402563(VS.92).aspx). (Jest to emulator używany w większości zrzutów ekranu w tym samouczku).
- Zmień ciąg agenta użytkownika w celu emulowania telefonu iPhone. Zobacz [ten](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/) wpis w blogu.
- [Emulator aplikacji mobilnych w programie Opera](http://www.opera.com/developer/tools/mobile/)
- [Apple Safari](http://www.apple.com/safari/download/) z agentem użytkownika ustawionym na telefon iPhone. Instrukcje dotyczące sposobu ustawiania agenta użytkownika w przeglądarce Safari na "iPhone" znajdują się w temacie [How to let Safari poudawać The IE](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) w blogu David Alison.

Projekty programu Visual Studio C# z kodem źródłowym są dostępne do dołączenia do tego tematu:

- [Pobieranie projektu początkowego](https://go.microsoft.com/fwlink/?linkid=228307&amp;clcid=0x409)
- [Ukończono pobieranie projektu](https://go.microsoft.com/fwlink/?linkid=228306&amp;clcid=0x409)

### <a name="what-youll-build"></a>Co będziesz kompilować

Na potrzeby tego samouczka dodasz funkcje mobilne do prostej aplikacji z listą konferencji, która jest dostępna w [projekcie początkowym](https://go.microsoft.com/fwlink/?LinkId=228307). Poniższy zrzut ekranu przedstawia stronę Tagi ukończonej aplikacji, jak pokazano w [emulatorze telefonu systemu Windows 7](https://msdn.microsoft.com/library/ff402563(VS.92).aspx). Zobacz [Mapowanie klawiatury dla emulatora Windows Phone](https://msdn.microsoft.com/library/ff754352(v=vs.92).aspx) , aby uprościć wprowadzanie danych z klawiatury.

[![p1_Tags_CompletedProj](aspnet-mvc-4-mobile-features/_static/image2.png)](aspnet-mvc-4-mobile-features/_static/image1.png)

Możesz użyć programu Internet Explorer w wersji 9 lub 10, FireFox lub Chrome do opracowania aplikacji mobilnej, ustawiając [ciąg agenta użytkownika](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/). Na poniższej ilustracji przedstawiono ukończony samouczek przy użyciu programu Internet Explorer, który emuluje telefon iPhone. Aby ułatwić debugowanie aplikacji, można użyć narzędzi deweloperskich programu Internet Explorer F-12 i [Narzędzia programu Fiddler](http://www.fiddler2.com/fiddler2/) .

![](aspnet-mvc-4-mobile-features/_static/image3.png)

### <a name="skills-youll-learn"></a>Posiadane umiejętności

Oto, co uzyskasz:

- Jak szablony ASP.NET MVC 4 używają atrybutu HTML5 `viewport` i renderowania adaptacyjnego w celu usprawnienia wyświetlania na urządzeniach przenośnych.
- Jak utworzyć widoki specyficzne dla urządzeń przenośnych.
- Jak utworzyć przełącznik widoku, który umożliwia użytkownikom przełączanie się między widokiem mobilnym a widokiem pulpitu aplikacji.

### <a name="getting-started"></a>Wprowadzenie

Pobierz aplikację z listą konferencji dla początkowego projektu, korzystając z następującego linku: [Pobierz](https://go.microsoft.com/fwlink/?LinkId=228307). Następnie w Eksploratorze Windows kliknij prawym przyciskiem myszy plik *MvcMobile. zip* i wybierz polecenie **Właściwości**. W oknie dialogowym **Właściwości MvcMobile. zip** wybierz przycisk **odblokowywanie** . (Odblokowywanie zapobiega ostrzeżeniu zabezpieczeń, które występuje podczas próby użycia pliku *zip* pobranego z sieci Web).

![p1_unBlock](aspnet-mvc-4-mobile-features/_static/image4.png)

Kliknij prawym przyciskiem myszy plik *MvcMobile. zip* i wybierz polecenie **Wyodrębnij wszystko** , aby rozpakować plik. W programie Visual Studio Otwórz plik *MvcMobile. sln* .

Naciśnij klawisze CTRL + F5, aby uruchomić aplikację, co spowoduje wyświetlenie jej w przeglądarce klasycznej. Uruchom emulator przeglądarki mobilnej, skopiuj adres URL aplikacji konferencyjnej do emulatora, a następnie kliknij link **Przeglądaj przez tag** . Jeśli używasz emulatora Windows Phone, kliknij przycisk na pasku adresu URL i naciśnij klawisz pauzy, aby uzyskać dostęp do klawiatury. Na poniższym obrazie przedstawiono widok *AllTags* (wybierając pozycję **Przeglądaj według tagu**).

[![p1_browseTag](aspnet-mvc-4-mobile-features/_static/image6.png)](aspnet-mvc-4-mobile-features/_static/image5.png)

Ekran jest bardzo czytelny na urządzeniu przenośnym. Wybierz łącze ASP.NET.

[![p1_tagged_ASPNET](aspnet-mvc-4-mobile-features/_static/image8.png)](aspnet-mvc-4-mobile-features/_static/image7.png)

Widok tagów ASP.NET jest bardzo czytelny. Na przykład kolumna **Date** jest bardzo trudna do odczytania. W dalszej części tego samouczka utworzysz wersję widoku *AllTags* , która jest przeznaczona dla przeglądarek dla urządzeń przenośnych, dzięki czemu będzie można ją odczytać.

Uwaga: obecnie istnieje usterka w aparacie buforowania mobilnego. W przypadku aplikacji produkcyjnych należy zainstalować stały pakiet [DisplayModes](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) część. Aby uzyskać szczegółowe informacje na temat poprawki, zobacz [ASP.NET MVC 4 Mobile buforowania](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx) .

## <a name="css-media-queries"></a>Zapytania dotyczące multimediów CSS

[Zapytania o multimedia CSS](http://www.w3.org/TR/css3-mediaqueries/) są rozszerzeniem CSS dla typów multimediów. Umożliwiają one tworzenie reguł, które zastępują domyślne reguły CSS dla określonych przeglądarek (agenci użytkownika). Typową regułą dla CSS, która jest przeznaczona dla przeglądarek mobilnych, jest definiowanie maksymalnego rozmiaru ekranu. Plik *Content\Site.css* , który jest tworzony podczas tworzenia nowego projektu internetowego ASP.NET MVC 4, zawiera następujące zapytanie o Multimedia:

[!code-css[Main](aspnet-mvc-4-mobile-features/samples/sample1.css)]

Jeśli okno przeglądarki ma 850 pikseli szerokości lub mniej, użyje reguł CSS wewnątrz tego bloku nośnika. Możesz użyć zapytań o multimediach CSS, takich jak ten, aby zapewnić lepszy wyświetlacz zawartości HTML w małych przeglądarkach (np. przeglądarkach mobilnych) niż domyślne reguły CSS, które są przeznaczone do szerszego wyświetlania przeglądarek klasycznych.

## <a name="the-viewport-meta-tag"></a>Tag meta okienka ekranu

Większość przeglądarek dla urządzeń przenośnych definiuje szerokość okna przeglądarki wirtualnej ( *okienko ekranu*) o rozmiarze znacznie większym niż rzeczywista szerokość urządzenia przenośnego. Dzięki temu przeglądarki mobilne można dopasować do całej strony sieci Web wewnątrz ekranu wirtualnego. Użytkownicy mogą następnie powiększyć atrakcyjną zawartość. Jeśli jednak ustawisz szerokość okienka ekranu jako rzeczywistą szerokość urządzenia, powiększanie nie jest wymagane, ponieważ zawartość pasuje do przeglądarki mobilnej.

Znacznik `<meta>` okienka ekranu w pliku układu ASP.NET MVC 4 ustawia szerokość urządzenia jako okienko ekranu. Poniższy wiersz przedstawia znacznik okienka ekranu `<meta>` w pliku układu ASP.NET MVC 4.

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample2.html)]

## <a name="examining-the-effect-of-css-media-queries-and-the-viewport-meta-tag"></a>Badanie efektu zapytań o multimediach CSS i okienka ekranu wziernika

Otwórz plik *Views\Shared\\_Layout. cshtml* w edytorze i Skomentuj tag okienka ekranu `<meta>`. Poniższy znacznik przedstawia wiersz z komentarzem.

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample3.cshtml)]

Otwórz plik *MvcMobile\Content\Site.css* w edytorze i Zmień maksymalną szerokość w kwerendzie multimediów na zero pikseli. Uniemożliwi to używanie reguł CSS w przeglądarkach dla urządzeń przenośnych. Poniższy wiersz przedstawia zmodyfikowaną kwerendę multimediów:

[!code-css[Main](aspnet-mvc-4-mobile-features/samples/sample4.css)]

Zapisz zmiany i przejdź do aplikacji konferencyjnej w emulatorze przeglądarki mobilnej. Niewielki tekst na poniższej ilustracji jest wynikiem usunięcia znacznika `<meta>` okienka ekranu. Bez `<meta>` okienka ekranu, przeglądarka powiększa się do domyślnej szerokości okienka ekranu (850 pikseli lub szerszych dla większości przeglądarek dla urządzeń przenośnych).

[![p1_noViewPort](aspnet-mvc-4-mobile-features/_static/image10.png)](aspnet-mvc-4-mobile-features/_static/image9.png)

Cofnij zmiany — Usuń komentarz z okienka ekranu `<meta>` w pliku układu i Przywróć zapytanie o multimedia do 850 pikseli w pliku *site. css* . Zapisz zmiany i Odśwież przeglądarkę aplikacji mobilnych, aby sprawdzić, czy został przywrócony ekran przyjazny dla urządzeń przenośnych.

Tagi `<meta>` okienka ekranu i zapytanie o multimedia CSS nie są specyficzne dla ASP.NET MVC 4 i można korzystać z tych funkcji w dowolnej aplikacji sieci Web. Ale są one teraz wbudowane w pliki, które są generowane podczas tworzenia nowego projektu ASP.NET MVC 4.

Aby uzyskać więcej informacji na temat znacznika `<meta>` okienka ekranu, zobacz [jeden z dwóch okienka ekranu — druga część](http://www.quirksmode.org/mobile/viewports2.html).

W następnej sekcji zobaczysz, jak udostępnić widoki dla przeglądarki mobilnej.

## <a name="overriding-views-layouts-and-partial-views"></a>Zastępowanie widoków, układów i częściowych widoków

Istotną nową funkcją w ASP.NET MVC 4 jest prosty mechanizm, który umożliwia przesłonięcie dowolnego widoku (w tym układów i częściowych widoków) dla przeglądarek mobilnych w ogólności dla pojedynczej przeglądarki dla urządzeń przenośnych lub dowolnej konkretnej przeglądarki. Aby zapewnić widok specyficzny dla urządzeń przenośnych, można skopiować plik widoku i dodać *.* Nazwa pliku. Na przykład, aby utworzyć widok *indeksu* mobilnego, skopiuj *Views\Home\Index.cshtml* do *Views\Home\Index.Mobile.cshtml*.

W tej sekcji utworzysz plik układu specyficzny dla urządzeń przenośnych.

Aby rozpocząć, skopiuj *Views\Shared\\_Layout. cshtml* do *Views\Shared\\_Layout. Mobile. cshtml*. Otwórz *\_Layout. Mobile. cshtml* i zmień tytuł z **konferencji MVC4** na **konferencję (mobilną)** .

W każdym wywołaniu `Html.ActionLink` Usuń opcję "Przeglądaj według" w każdym linku *ActionLink*. Poniższy kod przedstawia sekcję ukończona treść pliku układu mobilnego.

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample5.cshtml)]

Skopiuj plik *Views\Home\AllTags.cshtml* do *Views\Home\AllTags.Mobile.cshtml*. Otwórz nowy plik i Zmień element `<h2>` z "Tags" na "Tags (M)":

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample6.html)]

Przejdź do strony Tagi przy użyciu przeglądarki pulpitu i emulatora programu Mobile Browser. Emulator przeglądarki mobilnej przedstawia dwa dokonane zmiany.

[![p2m_layoutTags. Mobile](aspnet-mvc-4-mobile-features/_static/image12.png)](aspnet-mvc-4-mobile-features/_static/image11.png)

W przeciwieństwie do ekranu pulpitu nie została zmieniona.

[![p2_layoutTagsDesktop](aspnet-mvc-4-mobile-features/_static/image14.png)](aspnet-mvc-4-mobile-features/_static/image13.png)

## <a name="browser-specific-views"></a>Widoki specyficzne dla przeglądarki

Oprócz widoków specyficznych dla urządzeń przenośnych i pulpitów można tworzyć widoki dla poszczególnych przeglądarek. Można na przykład tworzyć widoki, które są przeznaczone dla przeglądarki iPhone. W tej sekcji utworzysz układ dla przeglądarki iPhone i wersja telefonu iPhone widoku *AllTags* .

Otwórz plik *Global. asax* i Dodaj następujący kod do metody `Application_Start`.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample7.cs)]

Ten kod definiuje nowy tryb wyświetlania o nazwie "iPhone", który zostanie dopasowany do każdego żądania przychodzącego. Jeśli żądanie przychodzące pasuje do zdefiniowanego warunku (czyli jeśli agent użytkownika zawiera ciąg "iPhone"), ASP.NET MVC będzie szukać widoków, których nazwa zawiera sufiks "iPhone".

W kodzie kliknij prawym przyciskiem myszy pozycję `DefaultDisplayMode`, wybierz polecenie **Rozwiąż**, a następnie wybierz pozycję `using System.Web.WebPages;`. Spowoduje to dodanie odwołania do przestrzeni nazw `System.Web.WebPages`, w którym są zdefiniowane typy `DisplayModes` i `DefaultDisplayMode`.

[![p2_resolve](aspnet-mvc-4-mobile-features/_static/image16.png)](aspnet-mvc-4-mobile-features/_static/image15.png)

Alternatywnie możesz ręcznie dodać następujący wiersz do sekcji `using` pliku.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample8.cs)]

Pełna zawartość pliku *Global. asax* jest pokazana poniżej.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample9.cs)]

Zapisz zmiany. Skopiuj plik *MvcMobile\Views\Shared\\_Layout. Mobile. cshtml* do *MvcMobile\Views\Shared\\_Layout. iPhone. cshtml*. Otwórz nowy plik, a następnie zmień nagłówek `h1` z `Conference (Mobile)` na `Conference (iPhone)`.

Skopiuj plik *MvcMobile\Views\Home\AllTags.Mobile.cshtml* do *MvcMobile\Views\Home\AllTags.iPhone.cshtml*. W nowym pliku Zmień element `<h2>` z "Tagi (M)" na "Tagi (iPhone)".

Uruchom aplikację. Uruchom emulator przeglądarki mobilnej, upewnij się, że jego agent użytkownika jest ustawiony na "iPhone", i przejdź do widoku *AllTags* . Poniższy zrzut ekranu przedstawia widok *AllTags* renderowany w przeglądarce [Safari](http://www.apple.com/safari/download/) . Przeglądarkę Safari dla systemu Windows można pobrać [tutaj](https://support.apple.com/kb/DL1531).

[![p2_iphoneView](aspnet-mvc-4-mobile-features/_static/image18.png)](aspnet-mvc-4-mobile-features/_static/image17.png)

W tej sekcji przedstawiono sposób tworzenia układów i widoków urządzeń przenośnych oraz tworzenia układów i widoków dla konkretnych urządzeń, takich jak telefon iPhone. W następnej sekcji zobaczysz, jak korzystać z platformy jQuery Mobile w celu uzyskania bardziej atrakcyjnych widoków dla urządzeń przenośnych.

## <a name="using-jquery-mobile"></a>Korzystanie z programu jQuery Mobile

Biblioteka [jQuery Mobile](http://jquerymobile.com/demos/1.0b3/#/demos/1.0b3/docs/about/intro.html) udostępnia strukturę interfejsu użytkownika, która działa na wszystkich głównych przeglądarkach dla urządzeń przenośnych. jQuery Mobile stosuje *udoskonalenia progresywne* dla przeglądarek mobilnych, które obsługują CSS i JavaScript. Rozszerzenie progresywne umożliwia wszystkim przeglądarkom wyświetlanie podstawowej zawartości strony sieci Web, a jednocześnie zapewnia bardziej zaawansowane przeglądarki i urządzenia. Pliki JavaScript i CSS dołączone do stylu jQuery Mobile wiele elementów do dopasowania do przeglądarek mobilnych bez wprowadzania zmian w znacznikach.

W tej sekcji zainstalujemy pakiet NuGet *jQuery. Mobile. MVC* , który instaluje widżet jQuery Mobile i widok-przełącznik View.

Aby rozpocząć, Usuń udostępnione wcześniej pliki *\\_Layout. Mobile. cshtml* i *Shared\\_Layout. iPhone. cshtml* .

Zmień nazwy plików *Views\Home\AllTags.Mobile.cshtml* i *Views\Home\AllTags.iPhone.cshtml* na *Views\Home\AllTags.iPhone.cshtml.Hide* i *Views\Home\AllTags.Mobile.cshtml.Hide*. Ponieważ pliki nie mają już rozszerzenia *. cshtml* , nie będą używane przez środowisko uruchomieniowe ASP.NET MVC do renderowania widoku *AllTags* .

Zainstaluj pakiet NuGet *jQuery. Mobile. MVC* , wykonując następujące czynności:

1. W menu **Narzędzia** wybierz pozycję **Menedżer pakietów NuGet**, a następnie wybierz pozycję **konsola Menedżera pakietów**.

    [![p3_packageMgr](aspnet-mvc-4-mobile-features/_static/image20.png)](aspnet-mvc-4-mobile-features/_static/image19.png)
2. W **konsoli Menedżera pakietów**wprowadź `Install-Package jQuery.Mobile.MVC -version 1.0.0`

Na poniższej ilustracji przedstawiono pliki dodane i zmienione do projektu MvcMobile przez pakiet NuGet jQuery. Mobile. MVC. Dodawane pliki mają [Add] dołączane po nazwie pliku. Obraz nie pokazuje plików GIF i PNG dodanych do folderu *Content\images* .

![](aspnet-mvc-4-mobile-features/_static/image21.png)

Pakiet NuGet programu jQuery. Mobile. MVC instaluje następujące elementy:

- *Aplikacja\_plik Start\BundleMobileConfig.cs* , który jest wymagany do odwoływania się do dodanych plików JavaScript i CSS. Należy postępować zgodnie z poniższymi instrukcjami i odwoływać się do pakietu mobilnego zdefiniowanego w tym pliku.
- pliki CSS dla urządzeń przenośnych.
- Widżet kontrolera `ViewSwitcher` (*Controllers\ViewSwitcherController.cs*).
- pliki JavaScript w usłudze jQuery Mobile.
- Plik układu jQuery Mobile-Style (*Views\Shared\\_Layout. Mobile. cshtml*).
- Widok częściowy przełączania widoku *(MvcMobile\Views\Shared\\_ViewSwitcher. cshtml*), który zawiera link w górnej części każdej strony, aby przełączyć się z widoku pulpitu do widoku Mobile i na odwrót.
- Kilka plików obrazów<em>PNG</em> i <em>GIF</em> w folderze <em>Content\images</em> .

Otwórz plik *Global. asax* i Dodaj następujący kod jako ostatni wiersz metody `Application_Start`.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample10.cs)]

Poniższy kod przedstawia kompletny plik *Global. asax* .

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample11.cs?highlight=26)]

> [!NOTE]
> Jeśli używasz programu Internet Explorer 9 i nie widzisz podanej linii `BundleMobileConfig` powyżej żółtego wyróżnienia, kliknij przycisk [Widok zgodności](https://windows.microsoft.com/windows7/How-to-use-Compatibility-View-in-Internet-Explorer-9)obraz przycisku widok zgodności![(wyłączone)](https://res2.windows.microsoft.com/resbox/en/Windows 7/main/f080e77f-9b66-4ac8-9af0-803c4f8a859c_15.jpg "Obraz przycisku widok zgodności (wyłączony)") w programie IE, aby zmienić ikonę z obrazu konturowego ![przycisku widok zgodności (wyłączone)](https://res2.windows.microsoft.com/resbox/en/Windows 7/main/f080e77f-9b66-4ac8-9af0-803c4f8a859c_15.jpg "Obraz przycisku widok zgodności (wyłączony)") na obraz pełnego koloru ![przycisku widok zgodności (włączony)](https://res1.windows.microsoft.com/resbox/en/Windows 7/main/156805ff-3130-481b-a12d-4d3a96470f36_14.jpg "Obraz przycisku widok zgodności (włączony)"). Możesz również wyświetlić ten samouczek w przeglądarce FireFox lub Chrome.

Otwórz plik *MvcMobile\Views\Shared\\_Layout. Mobile. cshtml* i Dodaj następujący znacznik bezpośrednio po wywołaniu `Html.Partial`:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample12.cshtml)]

Poniżej przedstawiono kompletny plik *MvcMobile\Views\Shared\\_Layout. Mobile. cshtml* :

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample13.cshtml)]

Skompiluj aplikację, a następnie w emulatorze przeglądarki mobilnej przejdź do widoku *AllTags* . Zobaczysz następujące elementy:

[![p3_afterNuGet](aspnet-mvc-4-mobile-features/_static/image23.png)](aspnet-mvc-4-mobile-features/_static/image22.png)

> [!NOTE]
> Aby debugować konkretny kod, należy [ustawić ciąg agenta użytkownika](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/) dla przeglądarki IE lub Chrome na telefon iPhone, a następnie użyć narzędzi programistycznych F-12. Jeśli w przeglądarce mobilnej nie są wyświetlane łącza **Strona główna**, **prelegent**, **tag**i **Data** jako przyciski, odwołania do skryptów komórkowych jQuery i plików CSS prawdopodobnie nie są poprawne.

Oprócz zmian stylu zobaczysz **Widok Mobile** i link umożliwiający przełączenie się z widoku Mobile do widoku pulpitu. Wybierz łącze **widok pulpitu** , aby wyświetlić widok pulpitu.

[![p3_desktopView](aspnet-mvc-4-mobile-features/_static/image25.png)](aspnet-mvc-4-mobile-features/_static/image24.png)

Widok pulpitu nie umożliwia bezpośredniego przejścia do widoku mobilnego. Naprawimy to teraz. Otwórz plik *Views\Shared\\_Layout. cshtml* . Bezpośrednio pod elementem `body` strony Dodaj następujący kod, który renderuje widżet View-przełącznik:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample14.cshtml)]

Odśwież widok *AllTags* w przeglądarce dla urządzeń przenośnych. Teraz można przechodzić między widokami pulpitu i urządzeń przenośnych.

[![p3_desktopViewWithMobileLink](aspnet-mvc-4-mobile-features/_static/image27.png)](aspnet-mvc-4-mobile-features/_static/image26.png)

> [!NOTE]
> Uwaga dotycząca debugowania: można dodać poniższy kod na końcu Views\Shared\\_ViewSwitcher. cshtml, aby ułatwić debugowanie widoków w przypadku używania przeglądarki jako ciągu agenta użytkownika ustawionej na urządzenie przenośne.
>
> [!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample15.cs)]
>
> i dodanie następującego nagłówka do pliku *Views\Shared\\_Layout. cshtml* .
>
> [!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample16.html)]

Przejdź do strony *AllTags* w przeglądarce klasycznej. Widżet View-przełącznik nie jest wyświetlany w przeglądarce klasycznej, ponieważ jest dodany tylko do strony układu mobilnego. W dalszej części tego samouczka zobaczysz, jak dodać widżet View-przełącznik do widoku pulpitu.

[![p3_desktopBrowser](aspnet-mvc-4-mobile-features/_static/image29.png)](aspnet-mvc-4-mobile-features/_static/image28.png)

## <a name="improving-the-speakers-list"></a>Ulepszanie listy głośników

W przeglądarce mobilnej wybierz link **głośników** . Ponieważ nie ma widoku mobilnego (*AllSpeakers. Mobile. cshtml*), domyślny wyświetlacz głośników (*AllSpeakers. cshtml*) jest renderowany przy użyciu widoku układu mobilnego ( *\_Layout. Mobile. cshtml*).

[![p3_speakersDeskTop](aspnet-mvc-4-mobile-features/_static/image31.png)](aspnet-mvc-4-mobile-features/_static/image30.png)

Można globalnie wyłączyć domyślny widok (inny niż mobilny) z renderowania wewnątrz układu mobilnego, ustawiając `RequireConsistentDisplayMode`, aby `true` w *widokach\\_ViewStart. cshtml* , jak to:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample17.cshtml)]

Gdy `RequireConsistentDisplayMode` jest ustawiona na `true`, układ mobilny (<em>\_Layout. Mobile. cshtml</em>) jest używany tylko w przypadku widoków mobilnych. (Oznacza to, że plik widoku ma postać <em>* * viewName</em><em>. Mobile. cshtml</em>.) możesz chcieć ustawić `RequireConsistentDisplayMode`, aby `true`, jeśli układ mobilności nie działa dobrze z widokami nieprzenośnymi. Poniższy zrzut ekranu pokazuje, jak strona <em>głośników</em> jest renderowana, gdy `RequireConsistentDisplayMode` jest ustawiona na `true`.

[![p3_speakersConsistent](aspnet-mvc-4-mobile-features/_static/image33.png)](aspnet-mvc-4-mobile-features/_static/image32.png)

Można wyłączyć spójny tryb wyświetlania w widoku przez ustawienie `RequireConsistentDisplayMode`, aby `false` w pliku widoku. Następujące znaczniki w pliku *Views\Home\AllSpeakers.cshtml* są ustawiane `RequireConsistentDisplayMode` `false`:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample18.cshtml)]

## <a name="creating-a-mobile-speakers-view"></a>Tworzenie widoku głośników mobilnych

Jak już wydano, widok *głośników* jest możliwy do odczytania, ale linki są małe i trudno naciskać na urządzenie przenośne. W tej sekcji utworzysz widok *głośników* specyficznych dla urządzeń przenośnych, który wygląda podobnie do nowoczesnej aplikacji mobilnej — wyświetla duże i łatwe do wybrania linki i zawiera pole wyszukiwania umożliwiające szybkie znajdowanie głośników.

Skopiuj *AllSpeakers. cshtml* do *AllSpeakers. Mobile. cshtml*. Otwórz plik *AllSpeakers. Mobile. cshtml* i Usuń `<h2>` nagłówka elementu.

W tagu `<ul>` Dodaj atrybut `data-role` i ustaw jego wartość na `listview`. Podobnie jak inne [atrybuty`data-*`](http://html5doctor.com/html5-custom-data-attributes/), `data-role="listview"` ułatwiają naciskanie dużych elementów listy. To jest wygląd ukończonej adjustacji:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample19.cshtml)]

Odśwież przeglądarkę mobilną. Zaktualizowany widok wygląda następująco:

[![p3_updatedSpeakerView1](aspnet-mvc-4-mobile-features/_static/image35.png)](aspnet-mvc-4-mobile-features/_static/image34.png)

Mimo że widok Mobile został ulepszony, trudno jest poruszać się długą listą głośników. Aby rozwiązać ten problem, w tagu `<ul>` Dodaj atrybut `data-filter` i ustaw go na `true`. Poniższy kod pokazuje `ul` znaczników.

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample20.html)]

Na poniższej ilustracji przedstawiono pole filtru wyszukiwania w górnej części strony, które wynika z atrybutu `data-filter`.

[![ps_Data_Filter](aspnet-mvc-4-mobile-features/_static/image37.png)](aspnet-mvc-4-mobile-features/_static/image36.png)

Podczas wpisywania każdej litery w polu wyszukiwania jQuery Mobile filtruje wyświetlaną listę, jak pokazano na poniższej ilustracji.

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image39.png)](aspnet-mvc-4-mobile-features/_static/image38.png)

## <a name="improving-the-tags-list"></a>Ulepszanie listy tagów

Podobnie jak w przypadku domyślnego widoku *głośników* , widok *tagów* jest czytelny, ale linki są małe i trudne do naciśnięcia na urządzeniu przenośnym. W tej sekcji nastąpi poprawienie widoku *tagów* w taki sam sposób, w jaki Naprawiono widok *głośników* .

Usuń &quot;Ukryj sufiks&quot; do pliku *Views\Home\AllTags.Mobile.cshtml.Hide* , tak aby nazwa była *Views\Home\AllTags.Mobile.cshtml*. Otwórz plik o zmienionej nazwie i Usuń element `<h2>`.

Dodaj atrybuty `data-role` i `data-filter` do tagu `<ul>`, jak pokazano poniżej:

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample21.html)]

Poniższy obraz przedstawia filtrowanie stron tagów na liście `J`.

[![p3_tags_J](aspnet-mvc-4-mobile-features/_static/image41.png)](aspnet-mvc-4-mobile-features/_static/image40.png)

## <a name="improving-the-dates-list"></a>Ulepszanie listy dat

Możesz ulepszyć widok *dat* , tak jak udoskonalono widoki *głośników* i *tagów* , dzięki czemu łatwiej jest używać na urządzeniu przenośnym.

Skopiuj plik *Views\Home\AllDates.cshtml* do *Views\Home\AllDates.Mobile.cshtml*. Otwórz nowy plik i Usuń element `<h2>`.

Dodaj `data-role="listview"` do tagu `<ul>`, takich jak:

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample22.html)]

Poniższy obraz pokazuje, jak wygląda strona **daty** z atrybutem `data-role`.

[![p3_dates1](aspnet-mvc-4-mobile-features/_static/image43.png)](aspnet-mvc-4-mobile-features/_static/image42.png) Zastąp zawartość pliku *Views\Home\AllDates.Mobile.cshtml* następującym kodem:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample23.cshtml)]

Ten kod grupuje wszystkie sesje według dni. Tworzy rozdzielacz listy dla każdego nowego dnia i wyświetla wszystkie sesje dla każdego dnia w ramach rozdzielacza. Oto, co wygląda jak w przypadku uruchomienia tego kodu:

[![p3_dates2](aspnet-mvc-4-mobile-features/_static/image45.png)](aspnet-mvc-4-mobile-features/_static/image44.png)

## <a name="improving-the-sessionstable-view"></a>Poprawianie widoku sesji

W tej sekcji utworzysz widok sesji dla urządzeń przenośnych. Wprowadzone zmiany będą bardziej obszerne niż w innych utworzonych widokach.

W przeglądarce mobilnej naciśnij przycisk **głośnik** , a następnie wprowadź `Sc` w polu wyszukiwania.

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image47.png)](aspnet-mvc-4-mobile-features/_static/image46.png)

Naciśnij link **Scott Hanselman** .

[![p3_scottHa](aspnet-mvc-4-mobile-features/_static/image49.png)](aspnet-mvc-4-mobile-features/_static/image48.png)

Jak widać, ekran jest trudny do odczytania w przeglądarce dla urządzeń przenośnych. Kolumna Date jest trudna do odczytania, a kolumna Tagi jest poza widokiem. Aby rozwiązać ten problem, skopiuj *Views\Home\SessionsTable.cshtml* do *Views\Home\SessionsTable.Mobile.cshtml*, a następnie zastąp zawartość pliku następującym kodem:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample24.cshtml)]

Kod usuwa kolumny Room i Tagi oraz formatuje tytuł, prelegent i datę w pionie, dzięki czemu wszystkie te informacje są odczytywane w przeglądarce mobilnej. Poniższy obraz odzwierciedla zmiany w kodzie.

[![ps_SessionsByScottHa](aspnet-mvc-4-mobile-features/_static/image51.png)](aspnet-mvc-4-mobile-features/_static/image50.png)

## <a name="improving-the-sessionbycode-view"></a>Ulepszanie widoku SessionByCode

Na koniec utworzysz widok *SessionByCode* na konkretnym urządzeniu mobilnym. W przeglądarce mobilnej naciśnij przycisk **głośnik** , a następnie wprowadź `Sc` w polu wyszukiwania.

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image53.png)](aspnet-mvc-4-mobile-features/_static/image52.png)

Naciśnij link **Scott Hanselman** . Wyświetlane są sesje Scott Hanselman.

[![ps_SessionsByScottHa](aspnet-mvc-4-mobile-features/_static/image55.png)](aspnet-mvc-4-mobile-features/_static/image54.png)

Wybierz **Przegląd usługi Microsoft Web Stack** linku miłości.

[![ps_love](aspnet-mvc-4-mobile-features/_static/image57.png)](aspnet-mvc-4-mobile-features/_static/image56.png)

Domyślny widok pulpitu jest prawidłowo, ale można go ulepszyć.

Skopiuj *Views\Home\SessionByCode.cshtml* do *Views\Home\SessionByCode.Mobile.cshtml* i Zastąp zawartość pliku *Views\Home\SessionByCode.Mobile.cshtml* następującym znacznikiem:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample25.cshtml)]

Nowe oznakowanie używa atrybutu `data-role`, aby poprawić układ widoku.

Odśwież przeglądarkę mobilną. Poniższy obraz odzwierciedla wprowadzone zmiany kodu:

[![p3_love2](aspnet-mvc-4-mobile-features/_static/image59.png)](aspnet-mvc-4-mobile-features/_static/image58.png)

## <a name="wrapup-and-review"></a>Wrapup i przejrzyj

W tym samouczku wprowadzono nowe funkcje mobilne programu ASP.NET MVC 4 Developer Preview. Funkcje mobilne obejmują:

- Możliwość przesłonięcia widoków układu, widoków i częściowych, zarówno globalnie, jak i dla poszczególnych widoków.
- Kontrola nad układem i częściowe zastąpienie wymuszania przy użyciu właściwości `RequireConsistentDisplayMode`.
- Widżet View-przełącznik widoku dla widoków mobilnych, który może być również wyświetlany w widokach pulpitu.
- Obsługa obsługi określonych przeglądarek, takich jak przeglądarka telefonu iPhone.

## <a name="see-also"></a>Zobacz też

- Witryna sieci [komórkowej jQuery](http://jquerymobile.com) .
- [Omówienie technologii jQuery Mobile](http://jquerymobile.com/demos/1.0b3/docs/about/intro.html)
- [Najlepsze rozwiązania dotyczące aplikacji sieci Web dla urządzeń przenośnych z rekomendacją W3C](http://www.w3.org/TR/mwabp/)
- [Zalecenie dotyczące kandydatów W3C dla zapytań dotyczących multimediów](http://www.w3.org/TR/css3-mediaqueries/)

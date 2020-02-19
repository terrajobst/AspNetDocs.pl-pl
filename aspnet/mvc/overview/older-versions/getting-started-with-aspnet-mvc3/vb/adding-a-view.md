---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-view
title: Dodawanie widoku (VB) | Microsoft Docs
author: Rick-Anderson
description: Ten samouczek zawiera informacje na temat tworzenia aplikacji sieci Web ASP.NET MVC przy użyciu programu Microsoft Visual Web Developer 2010 Express z dodatkiem Service Pack 1, który jest...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: d3633f64-5d3c-45c9-ae4b-cb1563e3739f
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: fa200935d83bb26c07b302449a6eba6fd67b5322
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457357"
---
# <a name="adding-a-view-vb"></a>Dodawanie widoku (VB)

Autor [Rick Anderson](https://twitter.com/RickAndMSFT)

> Ten samouczek zawiera informacje na temat tworzenia aplikacji sieci Web ASP.NET MVC przy użyciu programu Microsoft Visual Web Developer 2010 Express z dodatkiem Service Pack 1, który jest bezpłatną wersją Microsoft Visual Studio. Przed rozpoczęciem upewnij się, że zainstalowano wymagania wstępne wymienione poniżej. Wszystkie z nich można zainstalować, klikając następujące łącze: [Instalator platformy sieci Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternatywnie możesz zainstalować wstępnie wymagane składniki, korzystając z następujących linków:
> 
> - [Wymagania wstępne programu Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Aktualizacja narzędzi ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(obsługa środowiska uruchomieniowego + narzędzia)
> 
> Jeśli używasz programu Visual Studio 2010 zamiast programu Visual Web Developer 2010, Zainstaluj wymagania wstępne, klikając następujące łącze: [wymagania wstępne programu Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Projekt Visual Web Developer z kodem źródłowym VB.NET jest dostępny do tego tematu. [Pobierz wersję VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Jeśli wolisz C#, przejdź do [ C# wersji](../cs/adding-a-view.md) tego samouczka.

W tej sekcji zamierzamy zmodyfikować klasę `HelloWorldController`, aby użyć pliku szablonu widoku do czystego hermetyzowania procesu generowania odpowiedzi HTML na klienta.

Zacznijmy od użycia szablonu widoku z metodą `Index` w klasie `HelloWorldController`. Obecnie Metoda `Index` zwraca ciąg z komunikatem, który jest zakodowany w obrębie klasy kontrolera. Zmień metodę `Index`, aby zwracała obiekt `View`, jak pokazano na poniższej liście:

[!code-vb[Main](adding-a-view/samples/sample1.vb)]

Teraz dodamy szablon widoku do naszego projektu, który możemy wywołać za pomocą metody `Index`. Aby to zrobić, kliknij prawym przyciskiem myszy wewnątrz metody `Index` i kliknij polecenie **Dodaj widok**.

[![IndexAddView](adding-a-view/_static/image2.png "IndexAddView")](adding-a-view/_static/image1.png)

Pojawi się okno dialogowe **Dodaj widok** . Pozostaw wpisy domyślne i kliknij przycisk **Dodaj** .

[![3addView](adding-a-view/_static/image4.png "3addView")](adding-a-view/_static/image3.png)

Zostanie utworzony folder *MvcMovie\Views\HelloWorld* i plik *MvcMovie\Views\HelloWorld\Index.vbhtml* . Można je zobaczyć w **Eksplorator rozwiązań**:

[![SolnExpHelloWorldIndx](adding-a-view/_static/image6.png "SolnExpHelloWorldIndx")](adding-a-view/_static/image5.png)

Dodaj kod HTML pod tagiem `<h2>`. Zmodyfikowany plik *MvcMovie\Views\HelloWorld\Index.vbhtml* jest przedstawiony poniżej.

[!code-vbhtml[Main](adding-a-view/samples/sample2.vbhtml)]

Uruchom aplikację i przejdź do kontrolera &quot;Hello World&quot; Controller (`http://localhost:xxxx/HelloWorld`). Metoda `Index` w kontrolerze nie została znacznie zadziałała. po prostu uruchomiono instrukcję `return View()`, co oznacza, że chcemy użyć pliku szablonu widoku do renderowania odpowiedzi dla klienta. Ponieważ nie określono jawnie nazwy pliku szablonu widoku do użycia, ASP.NET MVC domyślnie używa pliku widoku *index. vbhtml* w folderze *\Views\HelloWorld* . Na poniższym obrazie przedstawiono zakodowany ciąg w widoku.

[![3HelloWorld](adding-a-view/_static/image8.png "3HelloWorld")](adding-a-view/_static/image7.png)

Wyglądają dość dobre. Należy jednak zauważyć, że na pasku tytułu przeglądarki widnieje &quot;indeks&quot; i duży tytuł na stronie jest wyświetlany &quot;mojej aplikacji MVC.&quot; zmienić te zmiany.

## <a name="changing-views-and-layout-pages"></a>Zmienianie widoków i stron układu

Najpierw Zmieńmy tekst &quot;aplikacji MVC.&quot;, że tekst jest udostępniony i pojawia się na każdej stronie. Faktycznie występuje tylko w jednym miejscu w naszym projekcie, nawet jeśli znajduje się na każdej stronie w naszej aplikacji. Przejdź do folderu */views/Shared* w **Eksplorator rozwiązań** i Otwórz plik *\_Layout. vbhtml* . Ten plik jest nazywany stroną układu i jest współdzieloną &quot;Shell&quot;, że wszystkie inne strony będą używane.

Zanotuj `@RenderBody()` wiersz kodu w dolnej części pliku. `RenderBody` jest symbolem zastępczym, w którym wszystkie utworzone strony są wyświetlane, &quot;opakowane&quot; na stronie układ. Zmień nagłówek `<h1>` z **&quot;** aplikacji MVC&quot; na &quot;aplikacji filmowej MVC&quot;.

[!code-html[Main](adding-a-view/samples/sample3.html)]

Uruchom aplikację i zanotuj teraz komunikat &quot;&quot;aplikacji filmowej MVC. Kliknij link **informacje** , a na tej stronie jest również wyświetlana &quot;aplikacja Movie&quot;MVC.

Pełny plik *\_Layout. vbhtml* jest przedstawiony poniżej:

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

Teraz Zmieńmy tytuł strony indeksu (widok).

[!code-vbhtml[Main](adding-a-view/samples/sample5.vbhtml)]

Otwórz *MvcMovie\Views\HelloWorld\Index.vbhtml*. Istnieją dwa miejsca, w których należy wprowadzić zmianę: pierwszy, tekst wyświetlany w tytule przeglądarki, a następnie w pomocniczym nagłówku (`<h2>` elementu). Postanowimy nieco inaczej, aby zobaczyć, który bit kodu zmienia się w ramach aplikacji.

Uruchom aplikację i przejdź do`http://localhost:xx/HelloWorld`. Zwróć uwagę, że tytuł przeglądarki, nagłówek podstawowy i pomocnicze nagłówki zostały zmienione. W aplikacji można łatwo wprowadzać duże zmiany, korzystając z małych zmian w widoku. (Jeśli w przeglądarce nie są widoczne zmiany, może być wyświetlana zawartość z pamięci podręcznej. Naciśnij kombinację klawiszy CTRL + F5 w przeglądarce, aby wymusić załadowanie odpowiedzi z serwera.

[![3_MyMovieList](adding-a-view/_static/image10.png "3_MyMovieList")](adding-a-view/_static/image9.png)

Nasz mały bit &quot;danych&quot; (w tym przypadku &quot;Hello world! komunikat&quot;) jest zakodowany na stałe. Nasza aplikacja MVC ma w wersji V (views), a mamy C (kontrolery), ale nie ma jeszcze M (model). Wkrótce przeprowadzimy Cię przez proces tworzenia bazy danych i pobierania z niej danych modelu.

## <a name="passing-data-from-the-controller-to-the-view"></a>Przekazywanie danych z kontrolera do widoku

Zanim przejdziemy do bazy danych i porozmawiamy nad modelami, najpierw porozmawiamy o przekazywaniu informacji z kontrolera do widoku. Chcemy przekazać szablon widoku, który jest wymagany w celu renderowania odpowiedzi HTML na klienta. Te obiekty są zwykle tworzone i przesyłane przez klasę kontrolera do szablonu widoku i powinny zawierać tylko te dane, których wymaga szablon widoku — i nie tylko.

Wcześniej z klasą `HelloWorldController`, Metoda akcji `Welcome` wykonała `name` i `numTimes` parametr, a następnie wyprowadza wartości parametrów do przeglądarki. Zamiast tego kontroler nie kontynuuje bezpośredniego renderowania tej odpowiedzi, zamiast tego umieścimy te dane w zbiorze dla widoku. Kontrolery i widoki mogą używać obiektu `ViewBag` do przechowywania tych danych. Ta funkcja zostanie przeniesiona do szablonu widoku automatycznie i użyta do renderowania odpowiedzi HTML przy użyciu zawartości Work jako danych. Taki sposób, aby kontroler został objęty jednym elementem i szablonem widoku innym, umożliwiając nam zachowanie czystego &quot;separacji&quot; w aplikacji.

Alternatywnie możemy zdefiniować klasę niestandardową, a następnie utworzyć wystąpienie tego obiektu samodzielnie, wypełnić je danymi i przekazać je do widoku. Jest to często nazywane ViewModel, ponieważ jest modelem niestandardowym dla widoku. W przypadku małych ilości danych ViewBag działa doskonale.

Wróć do pliku *HelloWorldController. vb* zmień metodę `Welcome` wewnątrz kontrolera, aby umieścić komunikat i NumTimes w ViewBag. ViewBag jest obiektem dynamicznym. Oznacza to, że możesz umieścić dowolne z nich. ViewBag nie ma zdefiniowanych właściwości, dopóki nie umieścisz w niej elementu.

Kompletny `HelloWorldController.vb` z nową klasą w tym samym pliku.

[!code-vb[Main](adding-a-view/samples/sample6.vb)]

Teraz nasza ViewBag zawiera dane, które zostaną automatycznie przesłane do widoku. W przeciwnym razie możemy przekazywać swój własny obiekt podobny do tego, jeśli lubimy:

[!code-csharp[Main](adding-a-view/samples/sample7.cs)]

Teraz potrzebujemy szablonu `WelcomeView`! Uruchom aplikację, aby nowy kod został skompilowany. Zamknij przeglądarkę, kliknij prawym przyciskiem myszy wewnątrz metody `Welcome`, a następnie kliknij polecenie **Dodaj widok**.

Oto, jak wygląda okno dialogowe **Dodaj widok** .

[![3AddWelcomeView](adding-a-view/_static/image12.png "3AddWelcomeView")](adding-a-view/_static/image11.png)

Dodaj następujący kod do elementu `<h2>` w nowym <em>WITAMU.</em> plik VBHTML. Utworzymy pętlę i powiedzmy, &quot;Hello&quot; tyle razy, ile jest to konieczne!

[!code-vbhtml[Main](adding-a-view/samples/sample8.vbhtml)]

Uruchom aplikację i przejdź do `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

Teraz dane są pobierane z adresu URL i automatycznie przesyłane do kontrolera. Kontroler pakuje dane do obiektu `Model` i przekazuje ten obiekt do widoku. Widok niż wyświetla dane w formacie HTML dla użytkownika.

[![3Hello_Scott_4](adding-a-view/_static/image14.png "3Hello_Scott_4")](adding-a-view/_static/image13.png)

Jest to również typ &quot;M&quot; dla modelu, ale nie do rodzaju bazy danych. Zapoznaj się z informacjami i Utwórz bazę danych dla filmów.

> [!div class="step-by-step"]
> [Poprzednie](adding-a-controller.md)
> [dalej](adding-a-model.md)

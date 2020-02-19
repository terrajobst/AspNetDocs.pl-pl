---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-view
title: Dodawanie widoku | Microsoft Docs
author: Rick-Anderson
description: 'Uwaga: zaktualizowana wersja tego samouczka jest dostępna w tym miejscu, w którym są używane ASP.NET MVC 5 i Visual Studio 2013. Jest to bezpieczniejsze i łatwiejsze w obserwowanie...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: dde851d7-882e-4d99-9b96-cf96daed81cc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: 81c2e1f46b08cbc9b5aa5d6c1b36d9d8dc2ba581
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457638"
---
# <a name="adding-a-view"></a>Dodawanie widoku

Autor [Rick Anderson](https://twitter.com/RickAndMSFT)

> > [!NOTE]
> > Zaktualizowana wersja tego samouczka jest dostępna w [tym miejscu](../../getting-started/introduction/getting-started.md) , w którym są używane ASP.NET MVC 5 i Visual Studio 2013. Jest to bezpieczniejsze i łatwiejsze w obserwowanie i zademonstrowanie większej liczby funkcji.

W tej sekcji zamierzasz zmodyfikować klasę `HelloWorldController`, aby używać plików szablonów widoku do czystego hermetyzowania procesu generowania odpowiedzi HTML na klienta.

Utworzysz plik szablonu widoku przy użyciu [aparatu widoku Razor](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx) wprowadzonego z ASP.NET MVC 3. Szablony widoków oparte na składni Razor mają rozszerzenie pliku *. cshtml* i umożliwiają elegancki sposób tworzenia danych wyjściowych HTML przy użyciu C#. Razor minimalizuje liczbę znaków i naciśnięć klawiszy wymaganych podczas pisania szablonu widoku i umożliwia szybkie i płynne przepływy kodowania.

Obecnie Metoda `Index` zwraca ciąg z komunikatem, który jest zakodowany w klasie kontrolera. Zmień metodę `Index`, aby zwracała obiekt `View`, jak pokazano w poniższym kodzie:

[!code-csharp[Main](adding-a-view/samples/sample1.cs)]

Powyższa metoda `Index` używa szablonu widoku do wygenerowania odpowiedzi HTML do przeglądarki. Metody kontrolera (znane również jako [metody akcji](http://rachelappel.com/asp.net-mvc-actionresults-explained)), takie jak powyższa metoda `Index`, zazwyczaj zwracają [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx) (lub klasę pochodną [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx)), a nie typy pierwotne, takie jak ciąg.

W projekcie Dodaj szablon widoku, którego można użyć z metodą `Index`. Aby to zrobić, kliknij prawym przyciskiem myszy wewnątrz metody `Index` i kliknij polecenie **Dodaj widok**.

![](adding-a-view/_static/image1.png)

Pojawi się okno dialogowe **Dodaj widok** . Pozostaw wartości domyślne w taki sposób, aby były wyświetlane, a następnie kliknij przycisk **Dodaj** :

![](adding-a-view/_static/image2.png)

Zostanie utworzony folder *MvcMovie\Views\HelloWorld* i plik *MvcMovie\Views\HelloWorld\Index.cshtml* . Można je zobaczyć w **Eksplorator rozwiązań**:

![](adding-a-view/_static/image3.png)

Poniżej przedstawiono plik *index. cshtml* , który został utworzony:

![HelloWorldIndex](adding-a-view/_static/image4.png)

Dodaj następujący kod HTML pod tagiem `<h2>`.

[!code-html[Main](adding-a-view/samples/sample2.html)]

Pełny plik *MvcMovie\Views\HelloWorld\Index.cshtml* jest przedstawiony poniżej.

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml?highlight=7-8)]

Jeśli używasz programu Visual Studio 2012, w Eksploratorze rozwiązań kliknij prawym przyciskiem myszy plik *index. cshtml* i wybierz pozycję **Widok w Inspektorze strony**.

![PI](adding-a-view/_static/image5.png)

[Samouczek inspektora stron](../../views/using-page-inspector-in-aspnet-mvc.md) zawiera więcej informacji o tym nowym narzędziu.

Alternatywnie Uruchom aplikację i przejdź do kontrolera `HelloWorld` (`http://localhost:xxxx/HelloWorld`). Metoda `Index` w kontrolerze nie została znacznie zadziałała. po prostu uruchomiono instrukcję `return View()`, która określa, że metoda powinna używać pliku szablonu widoku, aby renderować odpowiedź do przeglądarki. Ponieważ nie określono jawnie nazwy pliku szablonu widoku, ASP.NET MVC domyślnie przy użyciu pliku widoku *index. cshtml* w folderze *\Views\HelloWorld* . Na poniższym obrazie przedstawiono ciąg &quot;Hello z naszego szablonu widoku!&quot; zakodowana w widoku.

![](adding-a-view/_static/image6.png)

Wyglądają dość dobre. Należy jednak zauważyć, że pasek tytułu przeglądarki pokazuje, &quot;indeks my ASP.NET&quot;, a w górnej części strony znajduje się tekst &quot;logo. Poniżej &quot;logo poniżej.&quot; Link&quot; to Rejestracja i logowanie w linki, a poniżej znajdują się linki do stron domowych, informacji i kontaktów. Zmieńmy niektóre z tych elementów.

## <a name="changing-views-and-layout-pages"></a>Zmienianie widoków i stron układu

Najpierw należy zmienić &quot;logo tutaj.&quot; tytuł u góry strony. Ten tekst jest typowy dla każdej strony. Jest on faktycznie zaimplementowany w tylko jednym miejscu w projekcie, nawet jeśli pojawia się na każdej stronie w aplikacji. Przejdź do folderu */views/Shared* w **Eksplorator rozwiązań** i Otwórz plik *\_Layout. cshtml* . Ten plik jest nazywany *stroną układu* i jest współdzieloną &quot;Shell&quot;, że wszystkie inne strony będą używane.

![_LayoutCshtml](adding-a-view/_static/image7.png)

Szablony układów umożliwiają określenie układu kontenera HTML witryny w jednym miejscu, a następnie zastosowanie go na wielu stronach w witrynie. Znajdź wiersz `@RenderBody()`. `RenderBody` jest symbolem zastępczym, w którym są wyświetlane wszystkie strony dotyczące określonego widoku, &quot;opakowane&quot; na stronie układ. Na przykład w przypadku wybrania łącza informacje widok *Views\Home\About.cshtml* jest renderowany wewnątrz metody `RenderBody`.

Zmień Nagłówek tytułu witryny w szablonie układu, &quot;logo tutaj&quot;, aby &quot;&quot;filmu MVC.

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

Zastąp zawartość elementu title następującym znacznikiem:

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml)]

Uruchom aplikację i zwróć uwagę, że teraz jest ona wyświetlana &quot;&quot;filmu MVC. Kliknij link **About (informacje** ) i sprawdź, czy ta strona zawiera również &quot;&quot;filmu MVC. Udało nam się wprowadzić zmiany w szablonie układu i wszystkie strony w witrynie odzwierciedlają nowy tytuł.

![](adding-a-view/_static/image8.png)

Teraz Zmieńmy tytuł widoku indeksu.

Otwórz *MvcMovie\Views\HelloWorld\Index.cshtml*. Istnieją dwa miejsca, w których należy wprowadzić zmianę: pierwszy, tekst wyświetlany w tytule przeglądarki, a następnie w pomocniczym nagłówku (`<h2>` elementu). Postanowisz nieco inaczej, aby zobaczyć, który bit kodu zmienia się w ramach aplikacji.

[!code-cshtml[Main](adding-a-view/samples/sample6.cshtml)]

Aby wskazać tytuł HTML do wyświetlenia, powyższy kod ustawia właściwość `Title` obiektu `ViewBag` (który znajduje się w szablonie widoku *index. cshtml* ). Jeśli powracasz do kodu źródłowego szablonu układu, Zauważ, że szablon używa tej wartości w elemencie `<title>` jako część sekcji `<head>` w kodzie HTML, który został wcześniej zmodyfikowany. Korzystając z tego podejścia `ViewBag`, można łatwo przekazać inne parametry między szablonem widoku i plikiem układu.

Uruchom aplikację i przejdź do `http://localhost:xx/HelloWorld`. Zwróć uwagę, że tytuł przeglądarki, nagłówek podstawowy i pomocnicze nagłówki zostały zmienione. (Jeśli w przeglądarce nie są widoczne zmiany, może być wyświetlana zawartość z pamięci podręcznej. Naciśnij kombinację klawiszy CTRL + F5 w przeglądarce, aby wymusić załadowanie odpowiedzi z serwera. Tytuł przeglądarki jest tworzony przy użyciu `ViewBag.Title` ustawionej w szablonie widoku *index. cshtml* oraz dodatkowej aplikacji &quot;-Movie&quot; dodanej w pliku układu.

Zwróć uwagę na to, jak zawartość w szablonie widoku *index. cshtml* została scalona z szablonem widoku *\_Layout. cshtml* , a pojedyncza odpowiedź HTML została wysłana do przeglądarki. Szablony układów ułatwiają wprowadzanie zmian, które są stosowane do wszystkich stron w aplikacji.

![](adding-a-view/_static/image9.png)

Nasz mały bit &quot;danych&quot; (w tym przypadku &quot;Hello z naszego szablonu widoku! komunikat&quot;) jest zakodowany na stałe. Aplikacja MVC ma &quot;V&quot; (View) i masz&quot; &quot;C (kontroler), ale nie &quot;M&quot; (model). Wkrótce przeprowadzimy Cię przez proces tworzenia bazy danych i pobierania z niej danych modelu.

## <a name="passing-data-from-the-controller-to-the-view"></a>Przekazywanie danych z kontrolera do widoku

Zanim przejdziemy do bazy danych i porozmawiamy nad modelami, najpierw porozmawiamy o przekazywaniu informacji z kontrolera do widoku. Klasy kontrolerów są wywoływane w odpowiedzi na żądanie przychodzącego adresu URL. Klasa kontrolera to miejsce, w którym można napisać kod, który obsługuje żądania przeglądarki przychodzącej, pobiera dane z bazy danych i ostatecznie decyduje o typie odpowiedzi wysyłanej z powrotem do przeglądarki. Szablony widoków mogą być następnie używane z poziomu kontrolera do generowania i formatowania odpowiedzi HTML w przeglądarce.

Kontrolery są odpowiedzialne za dostarczanie wszelkich danych lub obiektów, które są wymagane, aby szablon widoku mógł renderować odpowiedź do przeglądarki. Najlepszym rozwiązaniem: **szablon widoku nigdy nie powinien wykonywać logiki biznesowej ani bezpośrednio korzystać z bazy danych**. Zamiast tego szablon widoku powinien współpracować tylko z danymi, które są do niego udostępniane przez kontroler. Utrzymywanie tego &quot;separacji zagadnień&quot; pomaga zapewnić, że kod jest czysty, weryfikowalne i bardziej konserwowany.

Obecnie Metoda akcji `Welcome` w klasie `HelloWorldController` przyjmuje `name` i `numTimes` parametr, a następnie wyprowadza wartości bezpośrednio do przeglądarki. Zamiast przetworzyć tę odpowiedź jako ciąg, należy zmienić kontroler tak, aby używał szablonu widoku. Szablon widoku generuje odpowiedź dynamiczną, co oznacza, że należy przekazać odpowiednie bity danych z kontrolera do widoku w celu wygenerowania odpowiedzi. Można to zrobić, korzystając z kontrolera, który umieści dane dynamiczne (parametry) wymagane przez szablon widoku w obiekcie `ViewBag`, do którego ten szablon widoku może uzyskać dostęp.

Wróć do pliku *HelloWorldController.cs* i zmień metodę `Welcome`, aby dodać `Message` i `NumTimes` wartość do obiektu `ViewBag`. `ViewBag` jest obiektem dynamicznym, co oznacza, że można w nim umieścić dowolne z nich. Obiekt `ViewBag` nie ma zdefiniowanych właściwości, dopóki nie umieścisz w nim elementu. [System powiązania modelu MVC ASP.NET](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) automatycznie mapuje nazwane parametry (`name` i `numTimes`) z ciągu zapytania na pasku adresu do parametrów w metodzie. Pełny plik *HelloWorldController.cs* wygląda następująco:

[!code-csharp[Main](adding-a-view/samples/sample7.cs)]

Teraz obiekt `ViewBag` zawiera dane, które zostaną automatycznie przesłane do widoku.

Następnie potrzebny będzie szablon widoku powitalnego. W menu **kompilacja** wybierz pozycję **Kompiluj MvcMovie** , aby upewnić się, że projekt został skompilowany.

Następnie kliknij prawym przyciskiem myszy wewnątrz metody `Welcome` i kliknij polecenie **Dodaj widok**.

![](adding-a-view/_static/image10.png)

Oto jak wygląda okno dialogowe **Dodaj widok** :

![](adding-a-view/_static/image11.png)

Kliknij przycisk **Dodaj**, a następnie Dodaj następujący kod poniżej elementu `<h2>` w nowym pliku *Welcome. cshtml* . Utworzysz pętlę, która wydaje się &quot;Hello&quot; tyle razy, ile powinien. Pełny plik *Welcome. cshtml* jest przedstawiony poniżej.

[!code-cshtml[Main](adding-a-view/samples/sample8.cshtml)]

Uruchom aplikację i przejdź do następującego adresu URL:

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

Teraz dane są pobierane z adresu URL i przesyłane do kontrolera przy użyciu [spinacza modelu](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx). Kontroler umieszcza dane w obiekcie `ViewBag` i przekazuje ten obiekt do widoku. W tym widoku dane są wyświetlane w formacie HTML dla użytkownika.

![](adding-a-view/_static/image12.png)

W powyższym przykładzie użyto obiektu `ViewBag`, aby przekazać dane z kontrolera do widoku. W tym samouczku będziemy używać modelu widoku do przekazywania danych z kontrolera do widoku. Podejście model widoku do przekazywania danych jest ogólnie preferowane w porównaniu z rozwiązaniem widoku. Aby uzyskać więcej informacji, zobacz wpis w blogu [Dynamic V o jednoznacznie określonym typie](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) .

Jest to również typ &quot;M&quot; dla modelu, ale nie do rodzaju bazy danych. Zapoznaj się z informacjami i Utwórz bazę danych dla filmów.

> [!div class="step-by-step"]
> [Poprzednie](adding-a-controller.md)
> [dalej](adding-a-model.md)

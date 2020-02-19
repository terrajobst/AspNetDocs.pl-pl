---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-view
title: Dodawanie widoku (C#) | Microsoft Docs
author: Rick-Anderson
description: Ten samouczek zawiera informacje na temat tworzenia aplikacji sieci Web ASP.NET MVC przy użyciu programu Microsoft Visual Web Developer 2010 Express z dodatkiem Service Pack 1, który jest...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: abc7c78d-cb09-4a4c-a887-61bc401d40e3
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: b4a1316feb8d9b7f3ef5ca4755bf1cc5b23e6ef9
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/19/2020
ms.locfileid: "77458214"
---
# <a name="adding-a-view-c"></a>Dodawanie widoku (C#)

Autor [Rick Anderson](https://twitter.com/RickAndMSFT)

> > [!NOTE]
> > Zaktualizowana wersja tego samouczka jest dostępna w [tym miejscu](../../../getting-started/introduction/getting-started.md) , w którym są używane ASP.NET MVC 5 i Visual Studio 2013. Jest to bezpieczniejsze i łatwiejsze w obserwowanie i zademonstrowanie większej liczby funkcji.
> 
> 
> Ten samouczek zawiera informacje na temat tworzenia aplikacji sieci Web ASP.NET MVC przy użyciu programu Microsoft Visual Web Developer 2010 Express z dodatkiem Service Pack 1, który jest bezpłatną wersją Microsoft Visual Studio. Przed rozpoczęciem upewnij się, że zainstalowano wymagania wstępne wymienione poniżej. Wszystkie z nich można zainstalować, klikając następujące łącze: [Instalator platformy sieci Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternatywnie możesz zainstalować wstępnie wymagane składniki, korzystając z następujących linków:
> 
> - [Wymagania wstępne programu Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Aktualizacja narzędzi ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(obsługa środowiska uruchomieniowego + narzędzia)
> 
> Jeśli używasz programu Visual Studio 2010 zamiast programu Visual Web Developer 2010, Zainstaluj wymagania wstępne, klikając następujące łącze: [wymagania wstępne programu Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Projekt programu Visual Web Developer z C# kodem źródłowym jest dostępny do załączenia do tego tematu. [Pobierz wersję C# programu](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Jeśli wolisz Visual Basic, przejdź do [wersji Visual Basic](../vb/intro-to-aspnet-mvc-3.md) tego samouczka.

W tej sekcji zamierzasz zmodyfikować klasę `HelloWorldController`, aby używać plików szablonów widoku do czystego hermetyzowania procesu generowania odpowiedzi HTML na klienta.

Utworzysz plik szablonu widoku przy użyciu nowego [aparatu widoku Razor](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx) wprowadzonego z ASP.NET MVC 3. Szablony widoków oparte na składni Razor mają rozszerzenie pliku *. cshtml* i umożliwiają elegancki sposób tworzenia danych wyjściowych HTML przy użyciu C#. Razor minimalizuje liczbę znaków i naciśnięć klawiszy wymaganych podczas pisania szablonu widoku i umożliwia szybkie i płynne przepływy kodowania.

Zacznij od użycia szablonu widoku z metodą `Index` w klasie `HelloWorldController`. Obecnie Metoda `Index` zwraca ciąg z komunikatem, który jest zakodowany w klasie kontrolera. Zmień metodę `Index`, aby zwracała obiekt `View`, jak pokazano na poniższej liście:

[!code-csharp[Main](adding-a-view/samples/sample1.cs)]

Ten kod używa szablonu widoku do wygenerowania odpowiedzi HTML do przeglądarki. W projekcie Dodaj szablon widoku, którego można użyć z metodą `Index`. Aby to zrobić, kliknij prawym przyciskiem myszy wewnątrz metody `Index` i kliknij polecenie **Dodaj widok**.

![](adding-a-view/_static/image1.png)

Pojawi się okno dialogowe **Dodaj widok** . Pozostaw wartości domyślne w taki sposób, aby były wyświetlane, a następnie kliknij przycisk **Dodaj** :

![](adding-a-view/_static/image2.png)

Zostanie utworzony folder *MvcMovie\Views\HelloWorld* i plik *MvcMovie\Views\HelloWorld\Index.cshtml* . Można je zobaczyć w **Eksplorator rozwiązań**:

![](adding-a-view/_static/image3.png)

Poniżej przedstawiono plik *index. cshtml* , który został utworzony:

[![HelloWorldIndex](adding-a-view/_static/image5.png)](adding-a-view/_static/image4.png)

Dodaj kod HTML pod tagiem `<h2>`. Zmodyfikowany plik *MvcMovie\Views\HelloWorld\Index.cshtml* jest przedstawiony poniżej.

[!code-cshtml[Main](adding-a-view/samples/sample2.cshtml)]

Uruchom aplikację i przejdź do kontrolera `HelloWorld` (`http://localhost:xxxx/HelloWorld`). Metoda `Index` w kontrolerze nie została znacznie zadziałała. po prostu uruchomiono instrukcję `return View()`, która określa, że metoda powinna używać pliku szablonu widoku, aby renderować odpowiedź do przeglądarki. Ponieważ nie określono jawnie nazwy pliku szablonu widoku, ASP.NET MVC domyślnie przy użyciu pliku widoku *index. cshtml* w folderze *\Views\HelloWorld* . Na poniższym obrazie przedstawiono zakodowany ciąg w widoku.

![](adding-a-view/_static/image6.png)

Wyglądają dość dobre. Należy jednak zauważyć, że na pasku tytułu przeglądarki widnieje napis "index", a w przypadku dużego tytułu na stronie jest wyświetlany komunikat "moja aplikacja MVC". Zmieńmy te.

## <a name="changing-views-and-layout-pages"></a>Zmienianie widoków i stron układu

Najpierw chcesz zmienić tytuł "moja aplikacja MVC" w górnej części strony. Ten tekst jest typowy dla każdej strony. W rzeczywistości jest zaimplementowana tylko w jednym miejscu w projekcie, mimo że pojawia się na każdej stronie w aplikacji. Przejdź do folderu */views/Shared* w **Eksplorator rozwiązań** i Otwórz plik *\_Layout. cshtml* . Ten plik jest nazywany *stroną układu* i jest udostępnianą "powłoką", która jest używana przez wszystkie inne strony.

[![_LayoutCshtml](adding-a-view/_static/image8.png)](adding-a-view/_static/image7.png)

Szablony układów umożliwiają określenie układu kontenera HTML witryny w jednym miejscu, a następnie zastosowanie go na wielu stronach w witrynie. Zwróć uwagę na `@RenderBody()` wiersz w dolnej części pliku. `RenderBody` jest symbolem zastępczym, w którym wszystkie utworzone strony specyficzne dla widoku są wyświetlane jako "opakowane" na stronie układu. Zmień Nagłówek tytułu w szablonie układu z "moja aplikacja MVC" na "aplikacja Movie MVC".

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml)]

Uruchom aplikację i zwróć uwagę, że teraz mówi "aplikacja" MVC Movie ". Kliknij link **informacje** , a zobaczysz, jak również ta strona pokazuje "aplikację Movie MVC". Udało nam się wprowadzić zmiany w szablonie układu i wszystkie strony w witrynie odzwierciedlają nowy tytuł.

![](adding-a-view/_static/image9.png)

Pełny plik *\_Layout. cshtml* jest przedstawiony poniżej:

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

Teraz Zmieńmy tytuł strony indeksu (widok).

Otwórz *MvcMovie\Views\HelloWorld\Index.cshtml*. Istnieją dwa miejsca, w których należy wprowadzić zmianę: pierwszy, tekst wyświetlany w tytule przeglądarki, a następnie w pomocniczym nagłówku (`<h2>` elementu). Postanowisz nieco inaczej, aby zobaczyć, który bit kodu zmienia się w ramach aplikacji.

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml)]

Aby wskazać tytuł HTML do wyświetlenia, powyższy kod ustawia właściwość `Title` obiektu `ViewBag` (który znajduje się w szablonie widoku *index. cshtml* ). Jeśli powrócisz do kodu źródłowego szablonu układu, Zauważ, że szablon używa tej wartości w elemencie `<title>` w sekcji `<head>` w kodzie HTML. Korzystając z tej metody, można łatwo przekazać inne parametry między szablonem widoku i plikiem układu.

Uruchom aplikację i przejdź do `http://localhost:xx/HelloWorld`. Zwróć uwagę, że tytuł przeglądarki, nagłówek podstawowy i pomocnicze nagłówki zostały zmienione. (Jeśli w przeglądarce nie są widoczne zmiany, może być wyświetlana zawartość z pamięci podręcznej. Naciśnij kombinację klawiszy CTRL + F5 w przeglądarce, aby wymusić załadowanie odpowiedzi z serwera.

Zwróć uwagę na to, jak zawartość w szablonie widoku *index. cshtml* została scalona z szablonem widoku *\_Layout. cshtml* , a pojedyncza odpowiedź HTML została wysłana do przeglądarki. Szablony układów ułatwiają wprowadzanie zmian, które są stosowane do wszystkich stron w aplikacji.

![](adding-a-view/_static/image10.png)

Nasz mały bit "Data" (w tym przypadku "Hello z naszego szablonu widoku!") komunikat) jest zakodowany na stałe. Aplikacja MVC ma "V" (widok) i masz "C" (kontroler), ale nie jest jeszcze "M" (model). Wkrótce przeprowadzimy Cię przez proces tworzenia bazy danych i pobierania z niej danych modelu.

## <a name="passing-data-from-the-controller-to-the-view"></a>Przekazywanie danych z kontrolera do widoku

Zanim przejdziemy do bazy danych i porozmawiamy nad modelami, najpierw porozmawiamy o przekazywaniu informacji z kontrolera do widoku. Klasy kontrolerów są wywoływane w odpowiedzi na żądanie przychodzącego adresu URL. Klasa kontrolera to miejsce, w którym można napisać kod, który obsługuje parametry przychodzące, pobiera dane z bazy danych i ostatecznie decyduje o typie odpowiedzi wysyłanej z powrotem do przeglądarki. Szablony widoków mogą być następnie używane z poziomu kontrolera do generowania i formatowania odpowiedzi HTML w przeglądarce.

Kontrolery są odpowiedzialne za dostarczanie wszelkich danych lub obiektów, które są wymagane, aby szablon widoku mógł renderować odpowiedź do przeglądarki. Szablon widoku nigdy nie powinien wykonywać logiki biznesowej ani bezpośrednio korzystać z bazy danych. Zamiast tego należy korzystać tylko z danych dostarczanych przez kontroler. Utrzymywanie tego "separacji zagadnień" pomaga zapewnić, że kod jest czysty i bardziej przejrzysty.

Obecnie Metoda akcji `Welcome` w klasie `HelloWorldController` przyjmuje `name` i `numTimes` parametr, a następnie wyprowadza wartości bezpośrednio do przeglądarki. Zamiast przetworzyć tę odpowiedź jako ciąg, należy zmienić kontroler tak, aby używał szablonu widoku. Szablon widoku generuje odpowiedź dynamiczną, co oznacza, że należy przekazać odpowiednie bity danych z kontrolera do widoku w celu wygenerowania odpowiedzi. Można to zrobić, korzystając z kontrolera, który umieści dane dynamiczne wymagane przez szablon widoku w obiekcie `ViewBag`, do którego będzie mógł uzyskać dostęp ten szablon.

Wróć do pliku *HelloWorldController.cs* i zmień metodę `Welcome`, aby dodać `Message` i `NumTimes` wartość do obiektu `ViewBag`. `ViewBag` jest obiektem dynamicznym, co oznacza, że można w nim umieścić dowolne z nich. Obiekt `ViewBag` nie ma zdefiniowanych właściwości, dopóki nie umieścisz w nim elementu. Pełny plik *HelloWorldController.cs* wygląda następująco:

[!code-csharp[Main](adding-a-view/samples/sample6.cs)]

Teraz obiekt `ViewBag` zawiera dane, które zostaną automatycznie przesłane do widoku.

Następnie potrzebny będzie szablon widoku powitalnego. W menu **Debuguj** wybierz pozycję **Kompiluj MvcMovie** , aby upewnić się, że projekt został skompilowany.

[![BuildHelloWorld](adding-a-view/_static/image12.png)](adding-a-view/_static/image11.png)

Następnie kliknij prawym przyciskiem myszy wewnątrz metody `Welcome` i kliknij polecenie **Dodaj widok**. Oto jak wygląda okno dialogowe **Dodaj widok** :

![](adding-a-view/_static/image13.png)

Kliknij przycisk **Dodaj**, a następnie Dodaj następujący kod poniżej elementu `<h2>` w nowym pliku *Welcome. cshtml* . Utworzysz pętlę "Hello" tyle razy, ile jest to konieczne. Pełny plik *Welcome. cshtml* jest przedstawiony poniżej.

[!code-cshtml[Main](adding-a-view/samples/sample7.cshtml)]

Uruchom aplikację i przejdź do następującego adresu URL:

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

Teraz dane są pobierane z adresu URL i automatycznie przesyłane do kontrolera. Kontroler umieszcza dane w obiekcie `ViewBag` i przekazuje ten obiekt do widoku. W tym widoku dane są wyświetlane w formacie HTML dla użytkownika.

![](adding-a-view/_static/image14.png)

Jest to również rodzaj "M" dla modelu, ale nie jako rodzaj bazy danych. Zapoznaj się z informacjami i Utwórz bazę danych dla filmów.

> [!div class="step-by-step"]
> [Poprzednie](adding-a-controller.md)
> [dalej](adding-a-model.md)

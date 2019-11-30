---
uid: aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
title: Tworzenie niestandardowych pomocników HTML (C#) | Microsoft Docs
author: microsoft
description: Celem tego samouczka jest zademonstrowanie sposobu tworzenia niestandardowych pomocników HTML, których można używać w widokach MVC. Dzięki wykorzystaniu pomocnika HTML...
ms.author: riande
ms.date: 10/07/2008
ms.assetid: e454c67d-a86e-4119-a858-eb04bbec2dff
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
msc.type: authoredcontent
ms.openlocfilehash: 264ff9850bad397826b45649d52fbfefafc53a01
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74594514"
---
# <a name="creating-custom-html-helpers-c"></a>Tworzenie niestandardowych pomocników HTML (C#)

przez [firmę Microsoft](https://github.com/microsoft)

[Pobierz plik PDF](https://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_9_CS.pdf)

> Celem tego samouczka jest zademonstrowanie sposobu tworzenia niestandardowych pomocników HTML, których można używać w widokach MVC. Korzystając z zalet pomocników HTML, można zmniejszyć liczbę żmudnymych tagów HTML, które należy wykonać, aby utworzyć standardową stronę HTML.

Celem tego samouczka jest zademonstrowanie sposobu tworzenia niestandardowych pomocników HTML, których można używać w widokach MVC. Korzystając z zalet pomocników HTML, można zmniejszyć liczbę żmudnymych tagów HTML, które należy wykonać, aby utworzyć standardową stronę HTML.

W pierwszej części tego samouczka opisano niektóre z istniejących pomocników HTML dostępnych w strukturze ASP.NET MVC. Następnie opisano dwie metody tworzenia niestandardowych pomocników HTML: wyjaśniono, jak utworzyć niestandardowe pomocniki HTML, tworząc metodę statyczną i tworząc metodę rozszerzenia.

## <a name="understanding-html-helpers"></a>Zrozumienie pomocników HTML

Pomocnik HTML to tylko Metoda zwracająca ciąg. Ciąg może reprezentować dowolny typ zawartości. Można na przykład użyć pomocników HTML do renderowania standardowych tagów HTML, takich jak tagi HTML `<input>` i `<img>`. Można również użyć pomocników HTML do renderowania bardziej złożonej zawartości, takiej jak pasek kart lub tabela HTML danych bazy danych.

Struktura ASP.NET MVC obejmuje następujący zestaw standardowych pomocników HTML (to nie jest kompletna lista):

- HTML. ActionLink ()
- HTML. BeginForm ()
- HTML. CheckBox ()
- HTML. DropDownList ()
- HTML. EndForm ()
- HTML. Hidden ()
- HTML. ListBox ()
- HTML. Password ()
- HTML. RadioButton ()
- HTML. TextArea ()
- HTML. TextBox ()

Rozważmy na przykład formularz z listą 1. Ten formularz jest renderowany przy użyciu dwóch standardowych pomocników HTML (patrz rysunek 1). Ten formularz używa metod pomocniczych `Html.BeginForm()` i `Html.TextBox()` do renderowania prostego formularza HTML.

[Strona ![renderowana z pomocnikami HTML](creating-custom-html-helpers-cs/_static/image2.png)](creating-custom-html-helpers-cs/_static/image1.png)

**Ilustracja 01**: Strona renderowana za pomocą pomocników HTML ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-custom-html-helpers-cs/_static/image3.png))

**Lista 1 — `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample1.aspx)]

Metoda pomocnika html. BeginForm () służy do tworzenia tagów otwierających i zamykających HTML `<form>`. Zauważ, że metoda `Html.BeginForm()` jest wywoływana w instrukcji using. Instrukcja using gwarantuje, że tag `<form>` zostanie zamknięty na końcu bloku using.

Jeśli wolisz, zamiast tworzyć blok using, możesz wywołać metodę pomocnika html. EndForm (), aby zamknąć tag `<form>`. Użyj niezależnie od tego, jak utworzysz otwierający i zamykający tag `<form>`, który jest najbardziej intuicyjny.

Metody pomocnika `Html.TextBox()` są używane na liście 1 w celu renderowania tagów `<input>` HTML. Jeśli wybierzesz opcję Wyświetl źródło w przeglądarce, zobaczysz źródło HTML na liście 2. Należy zauważyć, że źródło zawiera standardowe Tagi HTML.

> [!IMPORTANT]
> Zwróć uwagę, że pomocnik `Html.TextBox()`-HTML jest renderowany przy użyciu tagów `<%= %>` zamiast tagów `<% %>`. Jeśli nie dołączysz znaku równości, nic nie zostanie renderowane do przeglądarki.

Struktura ASP.NET MVC zawiera niewielki zestaw pomocników. Najprawdopodobniej musisz zwiększyć strukturę MVC przy użyciu niestandardowych pomocników HTML. W pozostałej części tego samouczka nauczysz się na dwie metody tworzenia niestandardowych pomocników HTML.

**Lista 2 — `Index.aspx Source`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample2.aspx)]

### <a name="creating-html-helpers-with-static-methods"></a>Tworzenie pomocników HTML z metodami statycznymi

Najprostszym sposobem utworzenia nowego pomocnika HTML jest utworzenie metody statycznej zwracającej ciąg. Załóżmy na przykład, że użytkownik zdecyduje się na utworzenie nowego pomocnika HTML, który renderuje tag HTML `<label>`. Aby renderować `<label>`, można użyć klasy z listy 2.

**Lista 2 — `Helpers\LabelHelper.cs`**

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample3.cs)]

Nie ma żadnych specjalnych informacji o klasie w liście 2. Metoda `Label()` po prostu zwraca ciąg.

Widok zmodyfikowany indeks w liście 3 używa `LabelHelper` do renderowania tagów `<label>` HTML. Należy zauważyć, że widok zawiera dyrektywę `<%@ imports %>`, która importuje `Application1.Helpers` przestrzeni nazw.

**Lista 2 — `Views\Home\Index2.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample4.aspx)]

### <a name="creating-html-helpers-with-extension-methods"></a>Tworzenie pomocników HTML przy użyciu metod rozszerzających

Jeśli chcesz utworzyć pomocników HTML, które działają podobnie jak standardowe pomocniki HTML zawarte w środowisku ASP.NET MVC, należy utworzyć metody rozszerzenia. Metody rozszerzające umożliwiają dodawanie nowych metod do istniejącej klasy. Podczas tworzenia metody pomocnika HTML należy dodać nowe metody do klasy HtmlHelper reprezentowanej przez właściwość HTML widoku.

Klasa na liście 3 dodaje metodę rozszerzenia do klasy `HtmlHelper` o nazwie `Label()`. Istnieje kilka rzeczy, które należy zauważyć w przypadku tej klasy. Najpierw należy zauważyć, że Klasa jest klasą statyczną. Należy zdefiniować metodę rozszerzenia z klasą statyczną.

Po drugie należy zauważyć, że pierwszy parametr metody `Label()` jest poprzedzony `this`em słowa kluczowego. Pierwszy parametr metody rozszerzenia wskazuje klasę, którą rozszerza Metoda rozszerzenia.

**Lista 3 — `Helpers\LabelExtensions.cs`**

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample5.cs)]

Po utworzeniu metody rozszerzenia i pomyślnym skompilowaniu aplikacji Metoda rozszerzenia jest wyświetlana w Visual Studio IntelliSense, podobnie jak wszystkie inne metody klasy (patrz rysunek 2). Jedyną różnicą jest to, że metody rozszerzające są wyświetlane z symbolem specjalnym obok nich (ikona strzałki w dół).

[![przy użyciu metody rozszerzenia html. Label ()](creating-custom-html-helpers-cs/_static/image5.png)](creating-custom-html-helpers-cs/_static/image4.png)

**Ilustracja 02**. użycie metody rozszerzenia html. Label () ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-custom-html-helpers-cs/_static/image6.png))

Zmodyfikowany widok indeksu w liście 4 używa metody rozszerzenia html. Label () w celu renderowania wszystkich tagów `<label>`.

**Lista 4 — `Views\Home\Index3.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample6.aspx)]

## <a name="summary"></a>Podsumowanie

W tym samouczku przedstawiono dwie metody tworzenia niestandardowych pomocników HTML. Najpierw wiesz już, jak utworzyć niestandardowy pomocnik HTML `Label()`, tworząc metodę statyczną zwracającą ciąg. Następnie dowiesz się, jak utworzyć niestandardową metodę pomocnika HTML `Label()`, tworząc metodę rozszerzenia klasy `HtmlHelper`.

W tym samouczku koncentrujemy się na tworzeniu bardzo prostej metody pomocnika HTML. Należy pamiętać, że pomocnik HTML może być tak skomplikowany, jak chcesz. Możesz tworzyć pomocników HTML, którzy renderują zawartość rozbudowaną, taką jak widoki drzewa, menu lub tabele danych bazy danych.

> [!div class="step-by-step"]
> [Poprzednie](asp-net-mvc-views-overview-cs.md)
> [dalej](using-the-tagbuilder-class-to-build-html-helpers-cs.md)

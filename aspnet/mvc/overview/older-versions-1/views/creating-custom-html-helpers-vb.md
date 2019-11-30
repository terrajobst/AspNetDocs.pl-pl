---
uid: mvc/overview/older-versions-1/views/creating-custom-html-helpers-vb
title: Tworzenie niestandardowych pomocników HTML (VB) | Microsoft Docs
author: microsoft
description: Celem tego samouczka jest zademonstrowanie sposobu tworzenia niestandardowych pomocników HTML, których można używać w widokach MVC. Dzięki wykorzystaniu pomocnika HTML...
ms.author: riande
ms.date: 10/07/2008
ms.assetid: f96f4800-19ef-44c0-b457-55e777eb5de8
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-custom-html-helpers-vb
msc.type: authoredcontent
ms.openlocfilehash: aaeadde258a2855343a5bfb1e5ee76000e04f6bd
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74593877"
---
# <a name="creating-custom-html-helpers-vb"></a>Tworzenie niestandardowych pomocników HTML (VB)

przez [firmę Microsoft](https://github.com/microsoft)

[Pobierz plik PDF](https://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_9_VB.pdf)

> Celem tego samouczka jest zademonstrowanie sposobu tworzenia niestandardowych pomocników HTML, których można używać w widokach MVC. Korzystając z zalet pomocników HTML, można zmniejszyć liczbę żmudnymych tagów HTML, które należy wykonać, aby utworzyć standardową stronę HTML.

Celem tego samouczka jest zademonstrowanie sposobu tworzenia niestandardowych pomocników HTML, których można używać w widokach MVC. Korzystając z zalet pomocników HTML, można zmniejszyć liczbę żmudnymych tagów HTML, które należy wykonać, aby utworzyć standardową stronę HTML.

W pierwszej części tego samouczka opisano niektóre z istniejących pomocników HTML dostępnych w strukturze ASP.NET MVC. Następnie opisano dwie metody tworzenia niestandardowych pomocników HTML: wyjaśniono sposób tworzenia niestandardowych pomocników HTML przez utworzenie metody udostępnionej i utworzenie metody rozszerzenia.

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

Rozważmy na przykład formularz z listą 1. Ten formularz jest renderowany przy użyciu dwóch standardowych pomocników HTML (patrz rysunek 1). Ten formularz używa metod pomocniczych `Html.BeginForm()` i `Html.TextBox()`.

[Strona ![renderowana z pomocnikami HTML](creating-custom-html-helpers-vb/_static/image2.png)](creating-custom-html-helpers-vb/_static/image1.png)

**Ilustracja 01**: Strona renderowana za pomocą pomocników HTML ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-custom-html-helpers-vb/_static/image3.png))

**Lista 1 — `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample1.aspx)]

Metoda pomocnika `Html.BeginForm()` służy do tworzenia tagów otwierającego i zamykającego `<form>` HTML. Zauważ, że metoda `Html.BeginForm()` jest wywoływana w instrukcji using. Instrukcja using gwarantuje, że tag `<form>` zostanie zamknięty na końcu bloku using.

Jeśli wolisz, zamiast tworzyć blok using, możesz wywołać metodę pomocnika html. EndForm (), aby zamknąć tag `<form>`. Użyj niezależnie od tego, jak utworzysz otwierający i zamykający tag `<form>`, który jest najbardziej intuicyjny.

Metody pomocnika `Html.TextBox()` są używane na liście 1 w celu renderowania tagów `<input>` HTML. Jeśli wybierzesz opcję Wyświetl źródło w przeglądarce, zobaczysz źródło HTML na liście 2. Należy zauważyć, że źródło zawiera standardowe Tagi HTML.

> [!IMPORTANT]
> Zwróć uwagę, że pomocnik `Html.TextBox()`-HTML jest renderowany przy użyciu tagów `<%= %>` zamiast tagów `<% %>`. Jeśli nie dołączysz znaku równości, nic nie zostanie renderowane do przeglądarki.

Struktura ASP.NET MVC zawiera niewielki zestaw pomocników. Najprawdopodobniej musisz zwiększyć strukturę MVC przy użyciu niestandardowych pomocników HTML. W pozostałej części tego samouczka nauczysz się na dwie metody tworzenia niestandardowych pomocników HTML.

**Lista 2 — `Index.aspx Source`**

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample2.aspx)]

### <a name="creating-html-helpers-with-shared-methods"></a>Tworzenie pomocników HTML przy użyciu metod wspólnych

Najprostszym sposobem utworzenia nowego pomocnika HTML jest utworzenie metody udostępnionej, która zwraca ciąg. Załóżmy na przykład, że użytkownik zdecyduje się na utworzenie nowego pomocnika HTML, który renderuje tag HTML `<label>`. Aby renderować `<label>`, można użyć klasy z listy 2.

**Lista 2 — `Helpers\LabelHelper.vb`**

[!code-vb[Main](creating-custom-html-helpers-vb/samples/sample3.vb)]

Nie ma żadnych specjalnych informacji o klasie w liście 2. Metoda `Label()` po prostu zwraca ciąg.

Widok zmodyfikowany indeks w liście 3 używa `LabelHelper` do renderowania tagów `<label>` HTML. Należy zauważyć, że widok zawiera dyrektywę `<%@ imports %>`, która importuje przestrzeń nazw Application1. helps.

**Lista 2 — `Views\Home\Index2.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample4.aspx)]

### <a name="creating-html-helpers-with-extension-methods"></a>Tworzenie pomocników HTML przy użyciu metod rozszerzających

Jeśli chcesz utworzyć pomocników HTML, które działają podobnie jak standardowe pomocniki HTML zawarte w środowisku ASP.NET MVC, należy utworzyć metody rozszerzenia. Metody rozszerzające umożliwiają dodawanie nowych metod do istniejącej klasy. Podczas tworzenia metody pomocnika HTML należy dodać nowe metody do klasy `HtmlHelper` reprezentowanej przez właściwość HTML widoku.

Moduł Visual Basic na liście 3 dodaje metodę rozszerzenia o nazwie `Label()` do klasy `HtmlHelper`. Istnieje kilka rzeczy, które należy zauważyć w przypadku tego modułu. Najpierw należy zauważyć, że moduł ma atrybut `<Extension()>`. Aby można było użyć tego atrybutu, należy zaimportować przestrzeń nazw `System.Runtime.CompilerServices`

Następnie należy zauważyć, że pierwszy parametr metody `Label()` reprezentuje klasę `HtmlHelper`. Pierwszy parametr metody rozszerzenia wskazuje klasę, którą rozszerza Metoda rozszerzenia.

**Lista 3 — `Helpers\LabelExtensions.vb`**

[!code-vb[Main](creating-custom-html-helpers-vb/samples/sample5.vb)]

Po utworzeniu metody rozszerzenia i pomyślnym skompilowaniu aplikacji Metoda rozszerzenia jest wyświetlana w Visual Studio IntelliSense, podobnie jak wszystkie inne metody klasy (patrz rysunek 2). Jedyną różnicą jest to, że metody rozszerzające są wyświetlane z symbolem specjalnym obok nich (ikona strzałki w dół).

[![przy użyciu metody rozszerzenia html. Label ()](creating-custom-html-helpers-vb/_static/image5.png)](creating-custom-html-helpers-vb/_static/image4.png)

**Ilustracja 02**. użycie metody rozszerzenia html. Label () ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-custom-html-helpers-vb/_static/image6.png))

Zmodyfikowany widok indeksu w liście 4 używa metody rozszerzenia html. Label () w celu renderowania całej etykiety &lt;&gt; tagów.

**Lista 4 — `Views\Home\Index3.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample6.aspx)]

## <a name="summary"></a>Podsumowanie

W tym samouczku przedstawiono dwie metody tworzenia niestandardowych pomocników HTML. Najpierw wiesz już, jak utworzyć niestandardowy pomocnik HTML `Label()`, tworząc metodę udostępnioną, która zwraca ciąg. Następnie dowiesz się, jak utworzyć niestandardową metodę pomocnika HTML `Label()`, tworząc metodę rozszerzenia klasy `HtmlHelper`.

W tym samouczku koncentrujemy się na tworzeniu bardzo prostej metody pomocnika HTML. Należy pamiętać, że pomocnik HTML może być tak skomplikowany, jak chcesz. Możesz tworzyć pomocników HTML, którzy renderują zawartość rozbudowaną, taką jak widoki drzewa, menu lub tabele danych bazy danych.

> [!div class="step-by-step"]
> [Poprzednie](asp-net-mvc-views-overview-vb.md)
> [dalej](using-the-tagbuilder-class-to-build-html-helpers-vb.md)

---
uid: mvc/overview/older-versions-1/views/creating-page-layouts-with-view-master-pages-vb
title: Tworzenie układów strony za pomocą stron wzorcowych widoku (VB) | Microsoft Docs
author: microsoft
description: W tym samouczku dowiesz się, jak utworzyć typowy układ strony dla wielu stron w aplikacji, wykorzystując zalety wyświetlania stron wzorcowych. Można użyć...
ms.author: riande
ms.date: 10/16/2008
ms.assetid: d34f90a1-6de3-482a-a326-f87fdcbaaaff
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-page-layouts-with-view-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: 97c0ecf1953cc54030656dd710a5150243877110
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78541224"
---
# <a name="creating-page-layouts-with-view-master-pages-vb"></a>Tworzenie układów stron za pomocą stron wzorcowych widoku (VB)

przez [firmę Microsoft](https://github.com/microsoft)

[Pobierz plik PDF](https://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_12_VB.pdf)

> W tym samouczku dowiesz się, jak utworzyć typowy układ strony dla wielu stron w aplikacji, wykorzystując zalety wyświetlania stron wzorcowych. Możesz na przykład użyć strony wzorca widoku, aby zdefiniować dwukolumnowy układ strony i zastosować układ dwukolumnowy dla wszystkich stron w aplikacji sieci Web.

## <a name="creating-page-layouts-with-view-master-pages"></a>Tworzenie układów strony za pomocą stron wzorcowych wyświetlania

W tym samouczku dowiesz się, jak utworzyć typowy układ strony dla wielu stron w aplikacji, wykorzystując zalety wyświetlania stron wzorcowych. Możesz na przykład użyć strony wzorca widoku, aby zdefiniować dwukolumnowy układ strony i zastosować układ dwukolumnowy dla wszystkich stron w aplikacji sieci Web.

Możesz również skorzystać z funkcji wyświetlania stron wzorcowych, aby udostępnić wspólną zawartość na wielu stronach w aplikacji. Na przykład możesz umieścić logo witryny sieci Web, linki nawigacyjne i reklamy na transparencie na stronie wzorca widoku. Dzięki temu każda Strona w aplikacji będzie wyświetlać tę zawartość automatycznie.

W tym samouczku dowiesz się, jak utworzyć nową stronę wzorcową widoku i utworzyć nową stronę zawartości widoku na podstawie strony głównej.

### <a name="creating-a-view-master-page"></a>Tworzenie strony wzorcowej widoku

Zacznijmy od utworzenia strony wzorcowej widoku, która definiuje układ dwóch kolumn. Aby dodać nową stronę wzorcową widoku do projektu MVC, kliknij prawym przyciskiem myszy folder Views\Shared, a następnie wybierz opcję menu **Dodaj, nowy element**i wybierz szablon Strona wzorcowa widoku MVC (patrz rysunek 1).

[![dodawania strony wzorcowej widoku](creating-page-layouts-with-view-master-pages-vb/_static/image2.png)](creating-page-layouts-with-view-master-pages-vb/_static/image1.png)

**Ilustracja 01**. Dodawanie strony wzorcowej widoku ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-page-layouts-with-view-master-pages-vb/_static/image3.png))

W aplikacji można utworzyć więcej niż jedną stronę wzorcową widoku. Każda Strona wzorcowa widoku może definiować inny układ strony. Na przykład niektóre strony mogą mieć układ dwukolumnowy i inne strony mające układ z trzema kolumnami.

Strona główna widoku wygląda bardzo podobnie do widoku standardowego ASP.NET MVC. Jednak, w przeciwieństwie do widoku normalnego, Strona wzorcowa widoku zawiera jeden lub więcej tagów `<asp:ContentPlaceHolder>`. Tagi `<contentplaceholder>` są używane do oznaczania obszarów strony wzorcowej, które można przesłonić na pojedynczej stronie zawartości.

Na przykład strona wzorca widoku na liście 1 definiuje układ dwóch kolumn. Zawiera dwa `<contentplaceholder>` Tagi. Jeden `<ContentPlaceHolder>` dla każdej kolumny.

**Lista 1 — `Views\Shared\Site.master`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample1.aspx)]

Treść strony wzorca widoku na liście 1 zawiera dwa `<div>` Tagi, które odpowiadają dwóm kolumnom. Klasa kolumn kaskadowego arkusza stylów jest stosowana do obu tagów `<div>`. Ta klasa jest definiowana w arkuszu stylów zadeklarowanym w górnej części strony wzorcowej. Możesz sprawdzić, jak strona wzorcowa widoku będzie renderowana, przełączając się do widok Projekt. Kliknij kartę projektowanie w lewym dolnym rogu edytora kodu źródłowego (patrz rysunek 2).

[![podglądu strony wzorcowej w projektancie](creating-page-layouts-with-view-master-pages-vb/_static/image5.png)](creating-page-layouts-with-view-master-pages-vb/_static/image4.png)

**Ilustracja 02**. Podgląd strony wzorcowej w projektancie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-page-layouts-with-view-master-pages-vb/_static/image6.png))

### <a name="creating-a-view-content-page"></a>Tworzenie strony zawartości widoku

Po utworzeniu strony wzorcowej widoku można utworzyć co najmniej jedną stronę zawartości widoku opartą na stronie wzorca widoku. Na przykład możesz utworzyć stronę zawartości widoku indeksu dla kontrolera macierzystego, klikając prawym przyciskiem myszy folder Views\Home, wybierając pozycję **Dodaj, nowy element**, wybierając szablon **strony zawartości widoku MVC** , wprowadzając nazwę index. aspx, a następnie klikając przycisk Dodaj (patrz rysunek 3).

[![dodawania strony zawartości widoku](creating-page-layouts-with-view-master-pages-vb/_static/image8.png)](creating-page-layouts-with-view-master-pages-vb/_static/image7.png)

**Ilustracja 03**: Dodawanie strony zawartości widoku ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-page-layouts-with-view-master-pages-vb/_static/image9.png))

Po kliknięciu przycisku Dodaj zostanie wyświetlone nowe okno dialogowe, które umożliwia wybranie strony wzorca widoku do skojarzenia z stroną wyświetlania zawartości (patrz rysunek 4). Możesz przejść do strony głównej widoku witryny programu site. Master utworzonej w poprzedniej sekcji.

[![Wybieranie strony wzorcowej](creating-page-layouts-with-view-master-pages-vb/_static/image11.png)](creating-page-layouts-with-view-master-pages-vb/_static/image10.png)

**Ilustracja 04**: Wybieranie strony wzorcowej ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-page-layouts-with-view-master-pages-vb/_static/image12.png))

Po utworzeniu nowej strony zawartości widoku opartej na głównej stronie wzorcowej witryny. wzorzec otrzymujesz plik z listą 2.

**Lista 2 — `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample2.aspx)]

Należy zauważyć, że ten widok zawiera tag `<asp:Content>`, który odnosi się do poszczególnych tagów `<asp:ContentPlaceHolder>` na stronie wzorca widoku. Każdy tag `<asp:Content>` zawiera atrybut ContentPlaceHolderID wskazujący konkretną `<asp:ContentPlaceHolder>` zastąpień.

Zwróć uwagę na to, że strona widok zawartości na liście 2 nie zawiera żadnych standardowych tagów otwierających i zamykających Tagi HTML. Na przykład nie zawiera tagu otwierającego i zamykającego `<html>` ani `<head>`. Wszystkie normalne Tagi otwierające i zamykające są zawarte na stronie wzorcowej widoku.

Każda zawartość, która ma być wyświetlana na stronie zawartości widoku, musi być umieszczona w tagu `<asp:Content>`. Jeśli umieścisz dowolny plik HTML lub inną zawartość poza tymi tagami, podczas próby wyświetlenia strony zostanie wyświetlony komunikat o błędzie.

Nie musisz przesłonić każdego znacznika `<asp:ContentPlaceHolder>` ze strony wzorcowej na stronie widok zawartości. Aby zamienić tag z określoną zawartością, wystarczy przesłonić tag `<asp:ContentPlaceHolder>`.

Na przykład zmodyfikowany widok indeksu na liście 3 zawiera tylko dwa Tagi `<asp:Content>`. Każdy tag `<asp:Content>` zawiera jakiś tekst.

**Lista 3 — `Views\Home\Index.aspx (modified)`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample3.aspx)]

Gdy żądasz widoku na liście 3, renderuje stronę na rysunku 5. Zauważ, że widok renderuje stronę z dwiema kolumnami. Zwróć uwagę na to, że zawartość ze strony Wyświetl zawartość jest scalana z zawartością na stronie widoku wzorca.

[![strony zawartości widoku indeksu](creating-page-layouts-with-view-master-pages-vb/_static/image14.png)](creating-page-layouts-with-view-master-pages-vb/_static/image13.png)

**Ilustracja 05**: Strona zawartości widoku indeksu ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-page-layouts-with-view-master-pages-vb/_static/image15.png))

### <a name="modifying-view-master-page-content"></a>Modyfikowanie zawartości strony wzorcowej widoku

Jednym z problemów, które można napotkać niemal natychmiast podczas pracy z widokiem strony wzorcowej, jest problem związany z modyfikacją zawartości strony głównej widoku, gdy wymagane są różne strony zawartości widoku. Na przykład każda Strona w aplikacji sieci Web ma unikatowy tytuł. Jednak tytuł jest zadeklarowany na stronie wzorca widoku, a nie na stronie wyświetlanie zawartości. Jak to zrobić, jak dostosować tytuł strony dla każdej strony widoku zawartości?

Istnieją dwa sposoby modyfikacji tytułu wyświetlanego na stronie Wyświetl zawartość. Najpierw można przypisać tytuł strony do atrybutu title dyrektywy `<%@ page %>` zadeklarowanej w górnej części strony zawartości widoku. Na przykład jeśli chcesz przypisać tytuł strony "Super doskonały witryna sieci Web" do widoku indeksu, możesz w górnej części widoku indeks dodać następującą dyrektywę:

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample4.aspx)]

Gdy widok indeksu jest renderowany w przeglądarce, żądany tytuł zostanie wyświetlony na pasku tytułu przeglądarki:

[pasek tytułu przeglądarki ![](creating-page-layouts-with-view-master-pages-vb/_static/image17.png)](creating-page-layouts-with-view-master-pages-vb/_static/image16.png)

Istnieje jedno ważne wymaganie, aby strona widoku głównego spełniała warunki, aby atrybut title działał. Na stronie wzorca widoku musi znajdować się tag `<head runat="server">` zamiast standardowego tagu `<head>` dla jego nagłówka. Jeśli tag `<head>` nie zawiera atrybutu runat = "Server", wówczas tytuł nie zostanie wyświetlony. Domyślna strona wzorcowa widoku zawiera tag wymagane `<head runat="server">`.

Alternatywnym podejściem do modyfikacji zawartości strony wzorcowej z pojedynczej strony zawartości widoku jest Zawijanie regionu, który chcesz zmodyfikować w tagu `<asp:ContentPlaceHolder>`. Załóżmy na przykład, że chcesz zmienić nie tylko tytuł, ale również Tagi meta, renderowane przez stronę widoku wzorca. Strona widoku wzorca na liście 4 zawiera tag `<asp:ContentPlaceHolder>` w obrębie tagu `<head>`.

**Lista 4 — `Views\Shared\Site2.master`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample5.aspx)]

Zwróć uwagę, że tag `<asp:ContentPlaceHolder>` na liście 4 zawiera zawartość domyślną: domyślny tytuł i domyślne Tagi meta. Jeśli ten tag `<asp:ContentPlaceHolder>` nie zostanie zastąpiony na stronie pojedynczej zawartości widoku, zostanie wyświetlona zawartość domyślna.

Strona widok zawartości na liście 5 zastępuje tag `<asp:ContentPlaceHolder>` w celu wyświetlenia niestandardowego tytułu i niestandardowych tagów Meta.

**Lista 5 — `Views\Home\Index2.aspx`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample6.aspx)]

### <a name="summary"></a>Podsumowanie

W tym samouczku przedstawiono podstawowe wprowadzenie do wyświetlania stron wzorcowych i wyświetlania stron zawartości. Zawarto informacje na temat tworzenia nowych stron wzorcowych widoku i tworzenia stron zawartości widoku na ich podstawie. Sprawdzono również, jak można zmodyfikować zawartość strony wzorcowej widoku z określonej strony zawartości widoku.

> [!div class="step-by-step"]
> [Poprzednie](using-the-tagbuilder-class-to-build-html-helpers-vb.md)
> [dalej](passing-data-to-view-master-pages-vb.md)

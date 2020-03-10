---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
title: Wprowadzenie do stron sieci Web ASP.NET — tworzenie spójnego układu | Microsoft Docs
author: Rick-Anderson
description: W tym samouczku pokazano, jak używać układów do tworzenia spójnego wyglądu stron w witrynie, która używa stron sieci Web ASP.NET. Przyjęto założenie, że ukończono...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: c85ec591-f8d7-4882-b763-de6ab9f3df7a
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
msc.type: authoredcontent
ms.openlocfilehash: 678eb7089e95e3d221d6b2d82034a62aefa75757
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78526692"
---
# <a name="introducing-aspnet-web-pages---creating-a-consistent-layout"></a>Wprowadzenie do stron sieci Web ASP.NET — tworzenie spójnego układu

Autor [FitzMacken](https://github.com/tfitzmac)

> W tym samouczku pokazano, jak używać *układów* do tworzenia spójnego wyglądu stron w witrynie, która używa stron sieci Web ASP.NET. Przyjęto założenie, że ukończono serię przez [usunięcie danych bazy danych na stronach sieci Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=251584).
> 
> Zawartość:
> 
> - Co to jest strona układu.
> - Jak połączyć strony układu z zawartością dynamiczną.
> - Jak przekazać wartości do strony układu.

## <a name="about-layouts"></a>Układy — informacje

Wszystkie utworzone przez siebie strony zostały ukończone, autonomiczne strony. Wszystkie należą do tej samej lokacji, ale nie mają żadnych wspólnych elementów ani standardowego wyglądu.

Większość lokacji ma spójny wygląd i układ. Na przykład jeśli przejdziesz do witryny [Microsoft.com/Web](https://www.microsoft.com/web/) i zobaczysz, że wszystkie strony są zgodne z ogólnym układem i motywem wizualnym:

![Strona witryny Microsoft.com/web przedstawiająca układ nagłówka, obszaru nawigacji, obszaru zawartości i stopki](layouts/_static/image1.png)

*Nieefektywnym* sposobem tworzenia tego układu będzie Definiowanie nagłówka, paska nawigacyjnego i stopki oddzielnie na każdej stronie. Za każdym razem duplikujesz te same znaczniki. Jeśli chcesz zmienić coś (na przykład zaktualizować stopkę), musisz oddzielnie zmienić każdą stronę.

Jest to miejsce, w którym są dostępne *strony układu* . Na stronach sieci Web ASP.NET można zdefiniować stronę układu, która zawiera ogólny kontener dla stron w witrynie. Na przykład strona układu może zawierać nagłówek, obszar nawigacji i stopkę. Strona układu zawiera symbol zastępczy, w którym znajduje się główna Zawartość.

Następnie można zdefiniować pojedyncze strony zawartości zawierające znaczniki i kod tylko dla tej strony. Strony zawartości nie muszą być kompletnymi stronami HTML; nie muszą oni nawet mieć elementu `<body>`. Mają także wiersz kodu, który informuje ASP.NET, na której stronie układu ma być wyświetlana zawartość. Oto ilustracja przedstawiająca sposób działania tej relacji:

![Diagram koncepcyjny pokazujący dwie strony zawartości i stronę układu, do których one pasują.](layouts/_static/image2.png)

Ta interakcja jest łatwa do zrozumienia, gdy zobaczysz ją w działaniu. W tym samouczku zmienisz strony filmów do korzystania z układu.

## <a name="adding-a-layout-page"></a>Dodawanie strony układu

Zacznij od utworzenia strony układu, która definiuje typowy układ strony z nagłówkiem, stopką i obszarem zawartości głównej. W witrynie WebPagesMovies Dodaj stronę CSHTML o nazwie *\_Layout. cshtml*.

Znak podkreślenia wiodącego (`_`) jest znaczący. Jeśli nazwa strony rozpoczyna się od znaku podkreślenia, ASP.NET nie będzie bezpośrednio wysyłać tej strony do przeglądarki. Ta konwencja umożliwia zdefiniowanie stron, które są wymagane dla danej witryny, ale nie ma możliwości bezpośredniego żądania.

Zastąp zawartość na stronie następującym:

[!code-html[Main](layouts/samples/sample1.html)]

Jak widać, ten znacznik jest tylko HTML, który używa elementów `<div>` do definiowania trzech sekcji na stronie oraz jednego elementu `<div>`, aby przechowywać trzy sekcje. Stopka zawiera bit kodu Razor: `@DateTime.Now.Year`, który będzie renderować bieżący rok w tej lokalizacji na stronie.

Zwróć uwagę, że istnieje link do arkusza stylów o nazwie *filmy. css*. Arkusz stylów to miejsce, w którym zostaną zdefiniowane szczegóły układu fizycznego elementów. W tej chwili utworzysz ten element.

Jedyną nietypową funkcją na tej *\_stronie Layout. cshtml* jest wiersz `@Render.Body()`. Jest to symbol zastępczy, w którym zostanie wystawiona zawartość, gdy ten układ zostanie scalony z inną stroną.

## <a name="adding-a-css-file"></a>Dodawanie pliku. CSS

Preferowanym sposobem zdefiniowania rzeczywistego układu (czyli wyglądu) elementów na stronie jest użycie reguł kaskadowego arkusza stylów (CSS). Utwórz plik *CSS* , który zawiera reguły dla nowego układu.

W programie WebMatrix wybierz katalog główny witryny. Następnie na karcie **pliki** wstążki kliknij strzałkę pod przyciskiem **Nowy** , a następnie kliknij pozycję **Nowy folder**.

![Opcja "nowy folder" w obszarze nowy na Wstążce.](layouts/_static/image3.png)

Nazwij nowe *Style*folderów.

![Nazywanie nowego folderu "Style"](layouts/_static/image4.png)

W folderze nowe *Style* Utwórz plik o nazwie *filmy. css*.

![Tworzenie nowych filmów plik CSS](layouts/_static/image5.png)

Zastąp zawartość nowego pliku *. css* następującym:

[!code-css[Main](layouts/samples/sample2.css)]

Nie będziemy mówić więcej o tych regułach CSS, z wyjątkiem dwóch rzeczy. Jest to, że oprócz ustawiania czcionek i rozmiarów, reguły używają pozycjonowania bezwzględnego do ustalenia lokalizacji nagłówka, stopki i głównego obszaru zawartości. Jeśli dopiero zaczynasz pozycjonować w CSS, możesz przeczytać samouczek dotyczący [pozycjonowania CSS](http://www.w3schools.com/css/css_positioning.asp) w witrynie w3schools.

Druga należy zwrócić uwagę na to, że na dole zostały skopiowane reguły stylu, które zostały pierwotnie zdefiniowane w pliku *Films. cshtml* . Te reguły zostały użyte we [wprowadzeniu do wyświetlania danych przy użyciu samouczka ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251580) , aby uczynić pomocnika `WebGrid` renderować znaczniki dodawane do tabeli. (Jeśli zamierzasz użyć pliku *. css* do definiowania definicji stylu, możesz również ustawić reguły stylu dla całej witryny).

## <a name="updating-the-movies-file-to-use-the-layout"></a>Aktualizowanie pliku filmów do korzystania z układu

Teraz można zaktualizować istniejące pliki w lokacji, aby używały nowego układu. Otwórz plik *Films. cshtml* . W górnej części, jako pierwszy wiersz kodu, Dodaj następujące elementy:

[!code-csharp[Main](layouts/samples/sample3.cs)]

Strona zostanie teraz uruchomiona w następujący sposób:

[!code-cshtml[Main](layouts/samples/sample4.cshtml?highlight=2)]

Ten wiersz kodu instruuje ASP.NET, że gdy strona *filmów* zostanie uruchomiona, należy ją scalić z plikiem *\_Layout. cshtml* .

Ponieważ plik *Films. cshtml* używa teraz strony układu, można usunąć znaczniki ze strony *filmy. cshtml* , która jest traktowana przez plik *\_Layout. cshtml* . Wypełnij `<!DOCTYPE>`, `<html>`i `<body>` otwierając i zamykając Tagi. Wykorzystaj cały `<head>` element i jego zawartość, w tym reguły stylu dla siatki, ponieważ teraz masz te reguły w pliku *. css* . Gdy skończysz, Zmień istniejący element `<h1>` na element `<h2>`; masz już element `<h1>` na stronie układu. Zmień tekst `<h2>` na "filmy lists".

Zwykle nie trzeba wprowadzać tych zmian na stronie zawartości. Po uruchomieniu witryny za pomocą strony układu utworzysz strony zawartości bez wszystkich tych elementów, które mają zaczynać się od. W tym przypadku w tym przypadku konwertujesz autonomiczną stronę na jedną, która korzysta z układu, więc istnieje trochę oczyszczania.

Po zakończeniu strona *filmy. cshtml* będzie wyglądać następująco:

[!code-cshtml[Main](layouts/samples/sample5.cshtml)]

### <a name="testing-the-layout"></a>Testowanie układu

Teraz można zobaczyć, jak wygląda układ. W programie WebMatrix kliknij prawym przyciskiem myszy stronę *filmy. cshtml* i wybierz polecenie **Uruchom w przeglądarce**. Gdy przeglądarka wyświetli stronę, będzie wyglądać następująco:

![Strona filmów renderowana przy użyciu układu](layouts/_static/image6.png)

ASP.NET scalono zawartość strony filmy. cshtml na stronie *\_Layout. cshtml* , w której znajduje się `RenderBody` Metoda. I oczywiście strona *\_Layout. cshtml* odwołuje się do pliku *. css* , który definiuje wygląd strony.

## <a name="updating-the-addmovie-page-to-use-the-layout"></a>Aktualizowanie strony addmovie do korzystania z układu

Rzeczywista korzyść dotycząca układów polega na tym, że można używać ich dla wszystkich stron w witrynie. Otwórz stronę *addmovie. cshtml* .

Można pamiętać, że na stronie *addmovie. cshtml* na początku znajdują się pewne reguły CSS umożliwiające Definiowanie wyglądu komunikatów o błędach walidacji. Ponieważ masz teraz plik *CSS* witryny, możesz przenieść te reguły do pliku *. css* . Usuń je z pliku *addmovie. cshtml* i Dodaj je do dolnej części pliku *film. css* . Przenosisz następujące reguły:

[!code-css[Main](layouts/samples/sample6.css)]

Teraz należy wprowadzić takie same zmiany w pliku *addmovie. cshtml* dla *filmów. cshtml* — Dodaj `Layout="~/_Layout.cshtml;` i Usuń znacznik HTML, który jest teraz nadmiarowy. Zmień element `<h1>`, aby `<h2>`. Gdy skończysz, strona będzie wyglądać podobnie do tego przykładu:

[!code-cshtml[Main](layouts/samples/sample7.cshtml)]

Uruchom stronę. Teraz wygląda na to ilustracja:

![Strona "Dodawanie filmów" renderowana przy użyciu układu](layouts/_static/image7.png)

Chcesz wprowadzić podobne zmiany na stronach w witrynie — *EditMovie. cshtml* i *DeleteMovie. cshtml*. Jednak przed rozpoczęciem możesz wprowadzić kolejną zmianę w układzie, która sprawia, że jest nieco bardziej elastyczna.

## <a name="passing-title-information-to-the-layout-page"></a>Przekazywanie informacji o tytule do strony układu

Utworzona strona *\_Layout. cshtml* ma element `<title>`, który jest ustawiony na "Moja witryna filmów". Większość przeglądarek wyświetla zawartość tego elementu jako tekst na karcie:

![&lt;tytuł strony&gt; element wyświetlany na karcie przeglądarki](layouts/_static/image8.png)

Informacje o tym tytule są ogólne. Załóżmy, że tekst tytułu ma być bardziej szczegółowy dla bieżącej strony. (Tekst tytułu jest również używany przez aparaty wyszukiwania, aby określić, do czego służy strona). Można przekazać informacje ze strony zawartości, takiej jak *filmy. cshtml* lub *addmovie. cshtml* , do strony układ, a następnie użyć tych informacji do dostosowania działania renderowania strony układu.

Otwórz ponownie stronę *filmy. cshtml* . W kodzie u góry Dodaj następujący wiersz:

[!code-csharp[Main](layouts/samples/sample8.cs)]

Obiekt `Page` jest dostępny na wszystkich stronach *. cshtml* i jest przeznaczony do tego celu, a mianowicie do udostępniania informacji między stroną a jej układem.

Otwórz stronę *\_Layout. cshtml* . Zmień element `<title>` tak, aby wyglądał następująco:

[!code-html[Main](layouts/samples/sample9.html)]

Ten kod renderuje wszystko, co znajduje się we właściwości `Page.Title` bezpośrednio w tej lokalizacji na stronie.

Uruchom stronę *filmy. cshtml* . Tym razem karta przeglądarki pokazuje przekazaną wartość `Page.Title`:

![Karta przeglądarki pokazująca tytuł utworzony dynamicznie](layouts/_static/image9.png)

Jeśli chcesz, Wyświetl źródło strony w przeglądarce. Można zobaczyć, że element `<title>` jest renderowany jako `<title>List Movies</title>`.

> [!TIP] 
> 
> **Obiekt strony**
> 
> Przydatną funkcją `Page` jest to, że jest to obiekt dynamiczny — Właściwość `Title` nie jest nazwą stałą lub zastrzeżoną. Możesz użyć *dowolnej* nazwy dla wartości `Page` obiektu. Na przykład można łatwo przekazywać tytuł przy użyciu właściwości o nazwie `Page.CurrentName` lub `Page.MyPage`. Jedynym ograniczeniem jest to, że nazwa musi być zgodna z normalnymi regułami dla właściwości, które można nazwać. (Na przykład nazwa nie może zawierać spacji).
> 
> Za pomocą obiektu `Page` można przekazać dowolną liczbę wartości. Jeśli chcesz przekazać informacje o filmie do strony układu, możesz przekazać wartości, używając takich elementów jak `Page.MovieTitle` i `Page.Genre` i `Page.MovieYear`. (Lub wszystkie inne nazwy, które zostały stworzone w celu przechowywania informacji). Jedyne wymaganie — co jest prawdopodobnie oczywiste — należy używać tych samych nazw na stronie zawartości i strony układu.
> 
> Informacje przekazywane przy użyciu obiektu `Page` nie są ograniczone do tekstu wyświetlanego na stronie układu. Można przekazać wartość do strony układ, a następnie kod na stronie układu może użyć wartości, aby zdecydować, czy ma być wyświetlana sekcja strony, co plik *CSS* ma być używany i tak dalej. Wartości przekazywane w obiekcie `Page` są podobne do innych wartości, które są używane w kodzie. Tylko te wartości pochodzą ze strony zawartości i są przesyłane do strony układu.

Otwórz stronę *addmovie. cshtml* i Dodaj wiersz na początku kodu, który zawiera tytuł dla strony *addmovie. cshtml* :

[!code-csharp[Main](layouts/samples/sample10.cs)]

Uruchom stronę *addmovie. cshtml* . Zobaczysz nowy tytuł:

![Karta przeglądarki pokazująca, że tytuł "Dodaj filmy" został utworzony dynamicznie](layouts/_static/image10.png)

## <a name="updating-the-remaining-pages-to-use-the-layout"></a>Aktualizowanie pozostałych stron w celu korzystania z układu

Teraz możesz zakończyć pozostałe strony w swojej lokacji, tak aby korzystały z nowego układu. Otwórz *EditMovie. cshtml* i *DeleteMovie. cshtml* z kolei i wprowadź te same zmiany w każdym z nich.

Dodaj wiersz kodu, który łączy się ze stroną układu:

[!code-csharp[Main](layouts/samples/sample11.cs)]

Dodaj wiersz, aby ustawić tytuł strony:

[!code-csharp[Main](layouts/samples/sample12.cs)]

lub:

[!code-csharp[Main](layouts/samples/sample13.cs)]

Usuń wszystkie nadmiarowe znaczniki HTML — w zasadzie pozostaw tylko bity znajdujące się wewnątrz elementu `<body>` (plus blok kodu u góry).

Zmień element `<h1>` na element `<h2>`.

Po wprowadzeniu tych zmian przetestuj każdy z nich i upewnij się, że jest on prawidłowo wyświetlany i że tytuł jest poprawny.

## <a name="parting-thoughts-about-layout-pages"></a>Częściowe pomysły dotyczące stron układu

W tym samouczku utworzono stronę *\_Layout. cshtml* i użyto metody `RenderBody`, aby scalić zawartość z innej strony. Jest to podstawowy wzorzec służący do używania układów na stronach sieci Web.

Strony układu mają dodatkowe funkcje, które nie zostały tutaj omówione. Na przykład można zagnieżdżać strony układu — jedną stronę układu można z kolei zmienić. Układy zagnieżdżone mogą być przydatne, jeśli pracujesz z podsekcjami lokacji, które wymagają różnych układów. Możesz również użyć dodatkowych metod (na przykład `RenderSection`), aby skonfigurować nazwane sekcje na stronie układu.

Kombinacja stron układu i plików *CSS* jest zaawansowana. Jak widać w następnej serii samouczków, w programie WebMatrix można utworzyć witrynę na podstawie *szablonu*, który udostępnia lokację ze wstępnie utworzoną funkcją. Szablony ułatwiają używanie stron układu i CSS do tworzenia witryn, które wyglądają doskonale i mają funkcje takie jak menu. Oto zrzut ekranu strony głównej z witryny na podstawie szablonu, który pokazuje funkcje korzystające ze stron układu i CSS:

![Układ utworzony przez szablon witryny WebMatrix pokazujący nagłówek, obszar nawigacji, obszar zawartości, sekcję opcjonalną i linki logowania](layouts/_static/image11.png)

## <a name="complete-listing-for-movie-page-updated-to-use-a-layout-page"></a>Ukończ listę dla strony filmu (Zaktualizowano do korzystania ze strony układu)

[!code-cshtml[Main](layouts/samples/sample14.cshtml)]

## <a name="complete-page-listing-for-add-movie-page-updated-for-layout"></a>Kompletna lista stron na potrzeby dodawania stron filmu (zaktualizowanych na potrzeby układu)

[!code-cshtml[Main](layouts/samples/sample15.cshtml)]

## <a name="complete-page-listing-for-delete-movie-page-updated-for-layout"></a>Pełna lista stron do usuwania filmów (zaktualizowanych dla układu)

[!code-cshtml[Main](layouts/samples/sample16.cshtml)]

## <a name="complete-page-listing-for-edit-movie-page-updated-for-layout"></a>Kompletna lista stron do edycji strony filmu (zaktualizowana do układu)

[!code-cshtml[Main](layouts/samples/sample17.cshtml)]

## <a name="coming-up-next"></a>Przyszłe przejście

W następnym samouczku dowiesz się, jak opublikować witrynę w Internecie, aby wszyscy mogli ją zobaczyć.

## <a name="additional-resources"></a>Dodatkowe materiały

- [Tworzenie spójnego wyglądu](https://go.microsoft.com/fwlink/?LinkID=202891) — artykułu, który zawiera bardziej szczegółowe informacje na temat pracy z układami. Opisano w nim również, jak przekazać wartość do strony układu, która pokazuje lub ukrywa część zawartości.
- [Zagnieżdżone strony układu z](http://www.mikesdotnetting.com/Article/164/Nested-Layout-Pages-with-Razor) tablicą Razor — na przykład, jak zagnieżdżać strony układu. (Obejmuje pobieranie stron).

> [!div class="step-by-step"]
> [Poprzednie](deleting-data.md)
> [dalej](publishing.md)

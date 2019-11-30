---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-cs
title: Wyświetlanie danych za pomocą kontrolek DataList i wzmacniakC#() | Microsoft Docs
author: rick-anderson
description: W poprzednich samouczkach do wyświetlania danych użyto kontrolki GridView. Począwszy od tego samouczka, przyjrzyjmy się tworzeniu wspólnych wzorców raportowania za pomocą...
ms.author: riande
ms.date: 09/13/2006
ms.assetid: 0591cacc-b34b-4cf6-885e-2c9953bb0946
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: 09d3faf811f21a66bb5c234f71d77b2552ae6516
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74623115"
---
# <a name="displaying-data-with-the-datalist-and-repeater-controls-c"></a>Wyświetlanie danych za pomocą kontrolek DataList i Repeater (C#)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_29_CS.exe) lub [Pobierz plik PDF](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/datatutorial29cs1.pdf)

> W poprzednich samouczkach do wyświetlania danych użyto kontrolki GridView. Począwszy od tego samouczka, przyjrzyjmy się tworzeniu wspólnych wzorców raportowania za pomocą kontrolek DataList i wzmacniak, rozpoczynając od podstaw wyświetlania danych za pomocą tych kontrolek.

## <a name="introduction"></a>Wprowadzenie

We wszystkich przykładach w ciągu ostatnich 28 samouczków, jeśli konieczne jest wyświetlenie wielu rekordów ze źródła danych, które włączyłem formant GridView. Widok GridView renderuje wiersz dla każdego rekordu w źródle danych, wyświetlając pola danych rekordu s w kolumnach. Gdy widok GridView sprawia, że jest to przyciąganie, Strona, sortowanie, edytowanie i usuwanie danych, jego wygląd jest bitowym boxy. Ponadto Naprawiono znaczniki dla struktury GridView s, która zawiera `<table>` HTML z wierszem tabeli (`<tr>`) dla każdego rekordu i komórki tabeli (`<td>`) dla każdego pola.

Aby zapewnić większy stopień dostosowania w wyglądzie i renderowane znaczniki podczas wyświetlania wielu rekordów, ASP.NET 2,0 oferuje kontrolki DataList i wzmacniak (oba są również dostępne w ASP.NET w wersji 1. x). Formanty DataList i wzmacniak renderują swoją zawartość przy użyciu szablonów, a nie BoundFields, CheckBoxFields, ButtonFields itd. Podobnie jak w widoku GridView, element DataList renderuje jako `<table>`HTML, ale umożliwia wyświetlanie wielu rekordów źródła danych dla każdego wiersza tabeli. Wzmacniak, z drugiej strony, nie renderuje żadnych dodatkowych oznaczeń niż to, które zostały jawnie określone, i jest idealnym kandydatem, gdy potrzebujesz precyzyjnej kontroli nad wyemitowanym znacznikiem.

Za pośrednictwem kolejnych dziesiątek lub samouczków będziemy oglądać typowe wzorce raportowania z kontrolkami DataList i wzmacniak, rozpoczynając od podstaw wyświetlania danych za pomocą tych szablonów formantów. Zobaczymy, jak sformatować te kontrolki, jak zmienić układ rekordów źródła danych w scenariuszach DataList, wspólnych wzorzec/szczegóły, sposobach edytowania i usuwania danych, sposobach tworzenia stron rekordów itd.

## <a name="step-1-adding-the-datalist-and-repeater-tutorial-web-pages"></a>Krok 1. Dodawanie stron sieci Web z samouczkiem DataList i wzmacniak

Przed rozpoczęciem tego samouczka Poświęć chwilę na dodanie ASP.NET stron, których potrzebujemy w tym samouczku, i następnych kilku samouczków związanych z wyświetlaniem danych za pomocą elementów DataList i wzmacniak. Zacznij od utworzenia nowego folderu w projekcie o nazwie `DataListRepeaterBasics`. Następnie Dodaj następujące pięć stron ASP.NET do tego folderu, z których wszystkie są skonfigurowane do korzystania z strony wzorcowej `Site.master`:

- `Default.aspx`
- `Basics.aspx`
- `Formatting.aspx`
- `RepeatColumnAndDirection.aspx`
- `NestedControls.aspx`

![Utwórz folder DataListRepeaterBasics i Dodaj samouczek ASP.NET stron](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image1.png)

**Rysunek 1**. tworzenie folderu `DataListRepeaterBasics` i dodawanie stron samouczka ASP.NET

Otwórz stronę `Default.aspx` i przeciągnij kontrolkę użytkownika `SectionLevelTutorialListing.ascx` z folderu `UserControls` na powierzchnię projektu. Ta kontrolka użytkownika, utworzona na [stronach wzorcowych i](../introduction/master-pages-and-site-navigation-cs.md) w samouczku nawigacji po witrynie, wylicza mapę witryny i wyświetla samouczki z bieżącej sekcji na liście punktowanej.

[![dodać kontrolkę użytkownika SectionLevelTutorialListing. ascx do default. aspx](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image3.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image2.png)

**Rysunek 2**. Dodawanie kontrolki użytkownika `SectionLevelTutorialListing.ascx` do `Default.aspx` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image4.png))

Aby Lista punktowana wyświetlała wyświetlane przez siebie samouczki DataList i wzmacniak, należy dodać je do mapy witryny. Otwórz plik `Web.sitemap` i Dodaj następujące znaczniki po dodaniu przycisków niestandardowych znaczniki węzła mapy witryny:

[!code-xml[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample1.xml)]

![Aktualizowanie mapy witryny w celu uwzględnienia nowych stron ASP.NET](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image5.png)

**Rysunek 3**. aktualizowanie mapy witryny w celu uwzględnienia nowych stron ASP.NET

## <a name="step-2-displaying-product-information-with-the-datalist"></a>Krok 2. Wyświetlanie informacji o produkcie z elementem DataList

Podobnie jak w widoku FormView, renderowane dane wyjściowe kontrolki DataList są zależne od szablonów, a nie BoundFields, CheckBoxFields i tak dalej. W przeciwieństwie do FormView, element DataList jest przeznaczony do wyświetlania zestawu rekordów, a nie Solitary. Pozwól, aby rozpocząć ten samouczek z przystąpieniem do powiązania informacji o produkcie z elementem DataList. Zacznij od otwarcia strony `Basics.aspx` w folderze `DataListRepeaterBasics`. Następnie przeciągnij element DataList z przybornika do projektanta. Jak pokazano na rysunku 4, przed określeniem szablonów DataList s, Projektant wyświetla go jako szare pole.

[![przeciąganie elementu DataList z przybornika do projektanta](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image7.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image6.png)

**Ilustracja 4**. Przeciągnij stronę DataList z przybornika do projektanta ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image8.png))

W tagu inteligentnym DataList s Dodaj nowy element ObjectDataSource i skonfiguruj go tak, aby korzystał z metody `GetProducts` `ProductsBLL` Class s. Ze względu na to, że w tym samouczku utworzysz ponownie element DataList z tylko odczytem, Ustaw listę rozwijaną na (brak) w Kreatorze wstawiania, aktualizacji i usuwania.

[![wybrać opcję tworzenia nowego elementu ObjectDataSource](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image10.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image9.png)

**Rysunek 5**. wybór opcji Utwórz nowy element ObjectDataSource ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image11.png))

[![skonfigurować element ObjectDataSource do używania klasy ProductsBLL](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image13.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image12.png)

**Ilustracja 6**. Konfigurowanie elementu ObjectDataSource do używania klasy `ProductsBLL` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image14.png))

[![pobrać informacji o wszystkich produktach przy użyciu metody getProducts](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image16.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image15.png)

**Rysunek 7**. Pobieranie informacji o wszystkich produktach przy użyciu metody `GetProducts` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image17.png))

Po skonfigurowaniu elementu ObjectDataSource i skojarzeniu go z elementem DataList za pomocą jego tagu inteligentnego program Visual Studio automatycznie utworzy `ItemTemplate` w elemencie DataList, który będzie wyświetlał nazwę i wartość każdego pola danych zwróconego przez źródło danych (zobacz znacznik poniżej). Ta wartość domyślna `ItemTemplate` s jest taka sama jak w przypadku szablonów tworzonych automatycznie podczas wiązania źródła danych do FormView przez projektanta.

[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample2.aspx)]

> [!NOTE]
> Odwołaj to, że podczas wiązania źródła danych z kontrolką FormView przez tag inteligentny FormView, program Visual Studio utworzył `ItemTemplate`, `InsertItemTemplate`i `EditItemTemplate`. Element DataList jest jednak tworzony tylko `ItemTemplate`. Dzieje się tak, ponieważ DataList nie ma tej samej wbudowanej edycji i wstawiania obsługi oferowanej przez FormView. Element DataList zawiera zdarzenia związane z edytowaniem i usuwaniem, a edytowanie i usuwanie pomocy technicznej może być dodawane z użyciem bitu kodu, ale nie jest to proste, niezwykłe wsparcie, jak w widoku FormView. Zobaczymy, jak włączyć obsługę edycji i usuwania przy użyciu elementu DataList w przyszłym samouczku.

Poświęć chwilę na poprawienie wyglądu tego szablonu. Zamiast wyświetlać wszystkie pola danych, niech s wyświetla tylko nazwę produktu, dostawcę, kategorię, ilość na jednostkę i cenę jednostkową. Ponadto, pozwól, aby wyświetlić nazwę w `<h4>` nagłówku i ułożyć pozostałe pola przy użyciu `<table>` poniżej nagłówka.

Aby wprowadzić te zmiany, można użyć funkcji edycji szablonów w Projektancie z taga inteligentnego DataList s kliknij link Edytuj szablony lub ręcznie zmodyfikować szablon za pośrednictwem składni deklaracyjnej strony. Jeśli używasz opcji Edytuj szablony w projektancie, wynikowe znaczniki mogą być niezgodne z poniższą adiustacją, ale gdy oglądasz za pomocą przeglądarki, powinien wyglądać podobnie do zrzutu ekranu pokazanego na rysunku 8.

[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample3.aspx)]

> [!NOTE]
> W powyższym przykładzie użyto kontrolek sieci Web etykieta, których właściwość `Text` ma przypisaną wartość składni wiązania danych. Alternatywnie możemy pominąć etykiety całkowicie, wpisując tylko składnię wiązania z danymi. Oznacza to, że zamiast używać `<asp:Label ID="CategoryNameLabel" runat="server" Text='<%# Eval("CategoryName") %>' />` możemy zamiast tego użyć składni deklaratywnej `<%# Eval("CategoryName") %>`.

Jednak po wyjściu z kontrolek sieci Web etykieta oferuje dwie zalety. Po pierwsze, zapewnia łatwiejszy sposób formatowania danych na podstawie danych, jak widać w następnym samouczku. Po drugie opcja Edytuj szablony w projektancie nie wyświetla instrukcji deklaracyjnego wiązania danych, która pojawia się poza częścią kontrolki sieci Web. Zamiast tego interfejs Edit templates został zaprojektowany w celu ułatwienia pracy ze statycznymi znacznikami i kontrolkami sieci Web. zakłada się, że każde powiązanie danych zostanie wykonane za pośrednictwem okna dialogowego Edytuj powiązania danych, które jest dostępne z poziomu tagów inteligentnych formantów sieci Web.

W związku z tym podczas pracy z elementem DataList, który udostępnia opcję edycji szablonów za pomocą projektanta, wolisz używać kontrolek sieci Web etykieta, aby zawartość była dostępna za pomocą interfejsu Edytuj szablony. Jak zobaczymy się wkrótce, wzmacniak wymaga, aby zawartość szablonu była edytowana z widoku źródła. W związku z tym podczas przedstawiania szablonów wzmacniak często pomijają kontrolki sieci Web etykiet, chyba że wiemy, że musimy sformatować wygląd tekstu powiązanego z danymi na podstawie logiki programowej.

[![wszystkie dane wyjściowe produktu są renderowane przy użyciu elementu DataList s ItemTemplate](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image19.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image18.png)

**Ilustracja 8**. Każdy produkt wyjściowy jest renderowany przy użyciu `ItemTemplate` DataList s ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image20.png))

## <a name="step-3-improving-the-appearance-of-the-datalist"></a>Krok 3: Poprawa wyglądu elementu DataList

Podobnie jak w widoku GridView, element DataList oferuje wiele właściwości związanych z stylem, takich jak `Font`, `ForeColor`, `BackColor`, `CssClass`, `ItemStyle`, `AlternatingItemStyle`, `SelectedItemStyle`i tak dalej. Podczas pracy z kontrolkami GridView i DetailsView tworzymy pliki skórki w motywie `DataWebControls`, który wstępnie zdefiniował właściwości `CssClass` tych dwóch kontrolek i Właściwość `CssClass` dla kilku ich właściwości (`RowStyle`, `HeaderStyle`itd.). Zezwól na to samo dla elementu DataList.

Zgodnie z opisem w sekcji [Wyświetlanie danych za pomocą](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) samouczka elementu ObjectDataSource, plik skórki określa domyślne właściwości dotyczące wyglądu dla kontrolki sieci Web; Motyw to kolekcja plików skórki, CSS, obrazów i JavaScript, które definiują konkretny wygląd i działanie witryny sieci Web. W samouczku *Wyświetlanie danych z elementem ObjectDataSource* został utworzony motyw `DataWebControls` (który jest zaimplementowany jako folder w folderze `App_Themes`), który ma, obecnie dwa pliki skórki — `GridView.skin` i `DetailsView.skin`. Pozwól s dodać trzeci plik skórki, aby określić wstępnie zdefiniowane ustawienia stylu dla elementu DataList.

Aby dodać plik skórki, kliknij prawym przyciskiem myszy folder `App_Themes/DataWebControls`, wybierz polecenie Dodaj nowy element, a następnie wybierz z listy opcję plik skórki. Nadaj plikowi nazwę `DataList.skin`.

[![utworzyć nowy plik skórki o nazwie DataList. Skin](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image22.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image21.png)

**Ilustracja 9**. Utwórz nowy plik skórki o nazwie `DataList.skin` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image23.png))

Użyj następującego znacznika dla pliku `DataList.skin`:

[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample4.aspx)]

Te ustawienia przypisują te same klasy CSS do odpowiednich właściwości DataList, które były używane z kontrolkami GridView i DetailsView. Klasy CSS używane tutaj `DataWebControlStyle`, `AlternatingRowStyle`, `RowStyle`i tak dalej są zdefiniowane w pliku `Styles.css` i zostały dodane do poprzednich samouczków.

Po dodaniu tego pliku skórki, wygląd DataList s jest aktualizowany w projektancie (może być konieczne odświeżenie widoku projektanta, aby zobaczyć efekty nowego pliku skórki; w menu Widok wybierz polecenie Odśwież). Jak pokazano na rysunku 10, każdy produkt przemienny ma jasny kolor tła.

[![utworzyć nowy plik skórki o nazwie DataList. Skin](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image25.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image24.png)

**Ilustracja 10**. Tworzenie nowego pliku skórki o nazwie `DataList.skin` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image26.png))

## <a name="step-4-exploring-the-datalist-s-other-templates"></a>Krok 4. Eksplorowanie innych szablonów DataList

Oprócz `ItemTemplate`, DataList może obsługiwać sześć innych opcjonalnych szablonów:

- `HeaderTemplate` jeśli jest podany, dodaje wiersz nagłówka do danych wyjściowych i służy do renderowania tego wiersza
- `AlternatingItemTemplate` używany do renderowania elementów zmiennych
- `SelectedItemTemplate` używany do renderowania wybranego elementu; wybrany element jest elementem, którego indeks odpowiada [właściwości`SelectedIndex`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.selectedindex.aspx) DataList s
- `EditItemTemplate` używany do renderowania edytowanego elementu
- `SeparatorTemplate` Jeśli podano, program dodaje separator między każdym elementem i służy do renderowania tego separatora
- `FooterTemplate` — jeśli jest podany, dodaje wiersz stopki do danych wyjściowych i służy do renderowania tego wiersza

Podczas określania `HeaderTemplate` lub `FooterTemplate`, element DataList dodaje dodatkowy wiersz nagłówka lub stopki do renderowanych danych wyjściowych. Podobnie jak w przypadku wierszy nagłówka i stopki GridView, nagłówek i stopka w elemencie DataList nie są powiązane z danymi. W związku z tym każda składnia DataBinding w `HeaderTemplate` lub `FooterTemplate`, która próbuje uzyskać dostęp do powiązanych danych zwróci pusty ciąg.

> [!NOTE]
> Jak widać w samouczku [Wyświetlanie informacji podsumowujących w widoku "GridView](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-cs.md) ", podczas gdy wiersze nagłówka i stopki nie obsługują składni DataBinding, informacje specyficzne dla danych można wstrzyknąć bezpośrednio do tych wierszy z programu obsługi zdarzeń `RowDataBound` GridView. Ta technika może służyć do obliczania sum bieżących lub innych informacji z danych powiązanych z kontrolką oraz do przypisywania tych informacji do stopki. Tę samą koncepcję można zastosować do kontrolek DataList i wzmacniak; Jedyną różnicą jest to, że dla elementu DataList i wzmacniak Utwórz procedurę obsługi zdarzeń dla zdarzenia `ItemDataBound` (zamiast zdarzenia `RowDataBound`).

W naszym przykładzie poinformuj o tym, że informacje o produkcie title są wyświetlane u góry elementu DataList s wyniki w `<h3>` nagłówku. Aby to osiągnąć, Dodaj `HeaderTemplate` z odpowiednim znacznikiem. Korzystając z projektanta, można to zrobić, klikając link Edytuj szablony w tagu inteligentnym DataList s, wybierając szablon nagłówka z listy rozwijanej i wpisując tekst po wybraniu opcji Nagłówek 3 z listy rozwijanej styl (patrz rysunek 11).

[![dodać HeaderTemplate z informacjami o produkcie tekstowym](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image28.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image27.png)

**Ilustracja 11**. Dodawanie `HeaderTemplate` z informacjami o produkcie tekstu ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image29.png))

Alternatywnie można to dodać deklaratywnie, wprowadzając następujące znaczniki w `<asp:DataList>` tagów:

[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample5.html)]

Aby dodać trochę miejsca między każdą listą produktów, Dodaj `SeparatorTemplate`, który zawiera wiersz między każdą sekcją. Znacznik reguły poziomej (`<hr>`), dodaje taki dzielnik. Utwórz `SeparatorTemplate` tak, aby miał następujące znaczniki:

[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample6.html)]

> [!NOTE]
> Podobnie jak `HeaderTemplate` i `FooterTemplates`, `SeparatorTemplate` nie jest powiązany z żadnym rekordem ze źródła danych i w związku z tym nie może bezpośrednio uzyskać dostępu do rekordów źródła danych powiązanych z elementem DataList.

Po wprowadzeniu tego dodatku podczas wyświetlania strony za pomocą przeglądarki powinien wyglądać podobnie do rysunku 12. Zanotuj wiersz nagłówka i wiersz między każdą listą produktów.

[![DataList zawiera wiersz nagłówka i regułę poziomą między każdą listą produktów](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image31.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image30.png)

**Ilustracja 12**. element DataList zawiera wiersz nagłówka i regułę poziomą między każdą listą produktów ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image32.png))

## <a name="step-5-rendering-specific-markup-with-the-repeater-control"></a>Krok 5. renderowanie określonego znacznika przy użyciu kontrolki Wzmacniakka

Jeśli tworzysz widok/źródło z przeglądarki podczas odwiedzania przykładu DataList z rysunku 12, zobaczysz, że element DataList emituje `<table>` HTML, który zawiera wiersz tabeli (`<tr>`) z pojedynczą komórką tabeli (`<td>`) dla każdego elementu powiązanego z elementem DataList. W rzeczywistości dane wyjściowe są identyczne z tym, co byłyby emitowane z widoku GridView przy użyciu jednego TemplateField. Jak zobaczymy w przyszłym samouczku, element DataList będzie zezwalał na dalsze dostosowanie danych wyjściowych, umożliwiając nam wyświetlanie wielu rekordów źródła danych na wiersz tabeli.

Co zrobić, jeśli nie chcesz emitować `<table>`HTML? Aby uzyskać całkowitą i pełną kontrolę nad adiustacją wygenerowaną przez formant sieci Web danych, należy użyć kontrolki wzmacniak. Podobnie jak dla elementu DataList, wzmacniak jest konstruowany na podstawie szablonów. Wzmacniak jednak oferuje tylko następujące pięć szablonów:

- `HeaderTemplate` jeśli jest podany, dodaje określoną adiustację przed elementami
- `ItemTemplate` używany do renderowania elementów
- `AlternatingItemTemplate`, jeśli jest podany, używany do renderowania elementów zmiennych
- `SeparatorTemplate`, jeśli jest podany, dodaje określoną adiustację między poszczególnymi elementami
- `FooterTemplate` — jeśli jest podany, dodaje określoną adiustację po elementach

W ASP.NET 1. x formant wzmacniak był często używany do wyświetlania listy punktowanej, której dane pochodzą z niektórych źródeł danych. W takim przypadku `HeaderTemplate` i `FooterTemplates` będą zawierać Tagi `<ul>` otwierające i zamykane, a `ItemTemplate` zawierać `<li>` elementy ze składnią DataBinding. Takie podejście nadal może być używane w ASP.NET 2,0, jak w dwóch przykładach na [stronach wzorcowych i](../introduction/master-pages-and-site-navigation-cs.md) w samouczku nawigacji po witrynie:

- Na stronie wzorca `Site.master` wzmacniak został użyty do wyświetlenia listy punktowanej zawartości mapy witryny najwyższego poziomu (raportowanie podstawowe, filtrowanie raportów, formatowanie dostosowane itd.); inny, zagnieżdżony wzmacniak został użyty do wyświetlenia części podrzędnych sekcji najwyższego poziomu
- W `SectionLevelTutorialListing.ascx`, wzmacniak został użyty do wyświetlenia listy punktowanej części elementów podrzędnych w bieżącej sekcji mapy witryny

> [!NOTE]
> ASP.NET 2,0 wprowadza nową [kontrolkę BulletedList](https://msdn.microsoft.com/library/ms228101.aspx), którą można powiązać z kontrolką źródła danych w celu wyświetlenia prostej listy punktowanej. Z kontrolką BulletedList nie musimy określać żadnego kodu HTML powiązanego z listą; Zamiast tego wskazujemy, że pole danych ma być wyświetlane jako tekst dla każdego elementu listy.

Wzmacniak służy jako przechwyć wszystkie dane w sieci Web. Jeśli nie ma istniejącej kontrolki generującej wymaganą adiustację, można użyć kontrolki wzmacniak. Aby zilustrować przy użyciu wzmacniania, poinformuj listę kategorii wyświetlanych powyżej elementu DataList informacji o produkcie utworzonej w kroku 2. W szczególności, niech s mają kategorie wyświetlane w jednowierszowym `<table>` HTML, z każdą kategorią wyświetlaną jako kolumna w tabeli.

Aby to osiągnąć, Zacznij od przeciągnięcia kontrolki wzmacniak z przybornika do projektanta powyżej informacji o produkcie DataList. Podobnie jak w przypadku elementu DataList, wzmacniak początkowo jest wyświetlany jako szare pole do momentu zdefiniowania jego szablonów.

[![dodać wzmacniak do projektanta](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image34.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image33.png)

**Ilustracja 13**. Dodawanie wzmacniania do projektanta ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image35.png))

W tagu inteligentnym wzmacniak s tylko jedną opcję: Wybierz źródło danych. Należy wybrać opcję Utwórz nowy element ObjectDataSource i skonfigurować go tak, aby korzystał z metody `GetCategories` `CategoriesBLL` Class s.

[![utworzyć nowego elementu ObjectDataSource](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image37.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image36.png)

**Ilustracja 14**. Tworzenie nowego elementu ObjectDataSource ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image38.png))

[![skonfigurować element ObjectDataSource do używania klasy CategoriesBLL](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image40.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image39.png)

**Ilustracja 15**. Konfigurowanie elementu ObjectDataSource do używania klasy `CategoriesBLL` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image41.png))

[![pobrać informacji o wszystkich kategoriach za pomocą metody GetCategories](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image43.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image42.png)

**Ilustracja 16**. Pobieranie informacji o wszystkich kategoriach za pomocą metody `GetCategories` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image44.png))

W przeciwieństwie do elementu DataList, program Visual Studio nie tworzy automatycznie ItemTemplate dla wzmacniania po powiązaniu go ze źródłem danych. Ponadto nie można skonfigurować szablonów wzmacniania za pomocą projektanta i muszą one zostać określone deklaratywnie.

Aby wyświetlić kategorie jako `<table>` jednowierszowe z kolumną dla każdej kategorii, potrzebny jest wzmacniak, który będzie emitować znaczniki podobne do następujących:

[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample7.html)]

Ponieważ `<td>Category X</td>` tekst jest częścią, która powtarza się, pojawi się w ItemTemplateach. Znacznik, który pojawia się przed-`<table><tr>`-zostanie umieszczony w `HeaderTemplate` podczas końcowego znacznika `</tr></table>`-zostanie umieszczony w `FooterTemplate`. Aby wprowadzić te ustawienia szablonu, przejdź do deklaratywnej części strony ASP.NET, klikając przycisk Source (Źródło) w lewym dolnym rogu, a następnie wpisz następującą składnię:

[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample8.aspx)]

Wzmacniak emituje precyzyjne znaczniki określone przez jego szablony, nic nie rób nic więcej. Rysunek 17 przedstawia dane wyjściowe przewodnika po otwarciu w przeglądarce.

[![tabelę &lt;HTML z pojedynczym wierszem,&gt; listę każdej kategorii w oddzielnej kolumnie](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image46.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image45.png)

**Ilustracja 17**. `<table>` HTML z pojedynczym wierszem zawiera listę każdej kategorii w oddzielnej kolumnie ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image47.png))

## <a name="step-6-improving-the-appearance-of-the-repeater"></a>Krok 6. ulepszanie wyglądu wzmacniaka

Ze względu na to, że wzmacniak emituje precyzyjnie adiustacje określone przez jego szablony, nie powinien on być niezależny, ponieważ nie ma właściwości związanych z stylem dla tego Wzmacniakka. Aby zmienić wygląd zawartości wygenerowanej przez wzmacniak, należy ręcznie dodać wymaganą zawartość HTML lub CSS bezpośrednio do szablonów wzmacniania.

W naszym przykładzie, niech s mają kolumny kategorii alternatywne kolory tła, takie jak z przemiennymi wierszami w elemencie DataList. Aby to osiągnąć, musimy przypisać klasę `RowStyle` CSS do każdego elementu wzmacniak i `AlternatingRowStyle`j klasy CSS do każdego naprzemiennego elementu wzmacniania za pomocą szablonów `ItemTemplate` i `AlternatingItemTemplate`, takich jak:

[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample9.aspx)]

Zezwól również na dodanie wiersza nagłówka do danych wyjściowych z kategoriami produktów tekstowych. Ponieważ nie wiemy, ile kolumn, z których wynika `<table>`, będzie się znajdować w najprostszy sposób na wygenerowanie wiersza nagłówka, który ma być podzielony na wszystkie kolumny, jest użycie *dwóch* `<table>` s. Pierwszy `<table>` będzie zawierać dwa wiersze wiersza nagłówka i wiersza, który będzie zawierać drugi `<table>` jednego wiersza, który zawiera kolumnę dla każdej kategorii w systemie. Oznacza to, że chcemy emitować następujące znaczniki:

[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample10.html)]

Następujące `HeaderTemplate` i `FooterTemplate` powodują pożądany znacznik:

[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample11.aspx)]

Ilustracja 18 pokazuje wzmacniak po wprowadzeniu tych zmian.

[![kolumny kategorii alternatywny w kolorze tła i zawiera wiersz nagłówka](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image49.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image48.png)

**Ilustracja 18**. kolumna kategorii alternatywna w kolorze tła i zawiera wiersz nagłówka ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image50.png))

## <a name="summary"></a>Podsumowanie

Chociaż formant GridView ułatwia wyświetlanie, edytowanie, usuwanie, sortowanie i stronicowe dane, wygląd jest bardzo boxyy i podobny do siatki. Aby uzyskać większą kontrolę nad wyglądem, musimy włączyć kontrolki DataList lub wzmacniak. Obie te kontrolki wyświetlają zestaw rekordów przy użyciu szablonów, a nie BoundFields, CheckBoxFields i tak dalej.

Element DataList renderuje jako `<table>` HTML, który domyślnie wyświetla każdy rekord źródła danych w pojedynczym wierszu tabeli, podobnie jak GridView z pojedynczym TemplateField. Mimo że zobaczymy w przyszłym samouczku, element DataList zezwala na wyświetlanie wielu rekordów dla każdego wiersza tabeli. Z drugiej strony wzmacniak, całkowicie emituje adiustacje określone w jego szablonach; nie dodaje żadnych dodatkowych znaczników i dlatego jest często używany do wyświetlania danych w elementach HTML innych niż `<table>` (na przykład na liście punktowanej).

Chociaż element DataList i wzmacniak oferują większą elastyczność w renderowanych danych wyjściowych, nie mają one wielu wbudowanych funkcji dostępnych w widoku GridView. Jak sprawdzimy się w kolejnych samouczkach, niektóre z tych funkcji można podłączyć ponownie bez zbyt dużej nakładu pracy, ale należy pamiętać, że użycie elementu DataList lub wzmacniak w miejsce GridView ogranicza funkcje, których można używać bez konieczności implementowania tych funkcji. samodzielnie.

Szczęśliwe programowanie!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Recenzenci liderzy dla tego samouczka to Yaakov Ellis, Liz Shulok, Randy Schmidt i Stacy Park. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Next](formatting-the-datalist-and-repeater-based-upon-data-cs.md)

---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/showing-multiple-records-per-row-with-the-datalist-control-cs
title: Wyświetlanie wielu rekordów na wiersz z kontrolką DataList (C#) | Microsoft Docs
author: rick-anderson
description: W tym krótkim samouczku dowiesz się, jak dostosować układ elementu DataList za pomocą jego właściwości RepeatColumns i RepeatDirection.
ms.author: riande
ms.date: 09/13/2006
ms.assetid: cf5acaf5-d4f6-4957-badc-b89956b285f3
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/showing-multiple-records-per-row-with-the-datalist-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 3280a7b5f28207d3e640a6480f47869ce19692bc
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74638576"
---
# <a name="showing-multiple-records-per-row-with-the-datalist-control-c"></a>Wyświetlanie wielu rekordów w wierszu za pomocą kontrolki DataList (C#)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_31_CS.exe) lub [Pobierz plik PDF](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/datatutorial31cs1.pdf)

> W tym krótkim samouczku dowiesz się, jak dostosować układ elementu DataList za pomocą jego właściwości RepeatColumns i RepeatDirection.

## <a name="introduction"></a>Wprowadzenie

Przykładowa wartość DataList pojawia się w ostatnich dwóch samouczkach, ponieważ każdy rekord ze źródła danych jest traktowany jako wiersz w jednokolumnowym `<table>`HTML. Chociaż jest to domyślne zachowanie DataList, można bardzo łatwo dostosować wyświetlanie elementu DataList, tak aby elementy źródła danych były rozłożone w wielokolumnowej tabeli wielowierszowej. Ponadto możliwe jest, aby wszystkie elementy źródła danych były wyświetlane w jednym wierszu, wielokolumnowej DataList.

Można dostosować układ DataList s za pomocą jego `RepeatColumns` i właściwości `RepeatDirection`, co oznacza, że liczba kolumn jest renderowana i czy te elementy są ułożone pionowo czy poziomo. Rysunek 1, na przykład, pokazuje element DataList, który wyświetla informacje o produkcie w tabeli z trzema kolumnami.

[![DataList pokazuje trzy produkty na wiersz](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image2.png)](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image1.png)

**Rysunek 1**: DataList pokazuje trzy produkty na wiersz ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image3.png))

Pokazując wiele elementów źródła danych na wiersz, element DataList może efektywnie wykorzystać poziome miejsce na ekranie. W tym krótkim samouczku będziemy eksplorować te dwie właściwości DataList.

## <a name="step-1-displaying-product-information-in-a-datalist"></a>Krok 1. Wyświetlanie informacji o produkcie w elemencie DataList

Przed przeprowadzeniem analizy właściwości `RepeatColumns` i `RepeatDirection`, pozwól, aby najpierw utworzyć element DataList na naszej stronie, który zawiera informacje o produkcie przy użyciu standardowego, wielowierszowego układu tabeli. Na potrzeby tego przykładu, aby wyświetlić nazwę produktu, kategorię i cenę, można użyć następujących znaczników:

[!code-html[Main](showing-multiple-records-per-row-with-the-datalist-control-cs/samples/sample1.html)]

Dowiesz się, jak powiązać dane z elementem DataList w poprzednich przykładach, aby szybko Przechodź między tymi krokami. Aby rozpocząć, Otwórz stronę `RepeatColumnAndDirection.aspx` w folderze `DataListRepeaterBasics` i przeciągnij element DataList z przybornika na projektanta. W tagu inteligentnym DataList s wybierz opcję Utwórz nowy element ObjectDataSource i skonfiguruj go do ściągania danych z metody `GetProducts` `ProductsBLL` klasy s, wybierając pozycję (brak) z kart kreatora Wstaw, Aktualizuj i Usuń.

Po utworzeniu i powiązaniu nowego elementu ObjectDataSource z obiektem DataList program Visual Studio automatycznie utworzy `ItemTemplate`, w którym zostanie wyświetlona nazwa i wartość każdego z pól danych produktu. Dostosuj `ItemTemplate` bezpośrednio za pośrednictwem znaczników deklaratywnych lub z opcji Edytuj szablony w tagu inteligentnym DataList s, tak aby używały oznakowanego powyżej, zastępując *nazwę produktu*, *nazwę kategorii*i tekst *ceny* z kontrolkami etykiet, które używają odpowiedniej składni wiązania danych do przypisywania wartości do ich `Text` właściwości. Po zaktualizowaniu `ItemTemplate`znaczniki deklaratywne strony powinny wyglądać podobnie do następujących:

[!code-aspx[Main](showing-multiple-records-per-row-with-the-datalist-control-cs/samples/sample2.aspx)]

Zwróć uwagę, że w składni powiązań danych `Eval` dla `UnitPrice`został umieszczony element specyfikator formatu, formatowanie zwracanej wartości jako waluty `Eval("UnitPrice", "{0:C}").`

Poświęć chwilę na odwiedzenie strony w przeglądarce. Jak pokazano na rysunku 2, element DataList renderuje jako jednokolumnową tabelę z pojedynczym wierszem produktów.

[Domyślnie ![element DataList renderuje jako jednokolumnową tabelę wielowierszową](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image5.png)](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image4.png)

**Rysunek 2**. Domyślnie element DataList renderuje jako jednokolumnową tabelę wielowierszową ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image6.png))

## <a name="step-2-changing-the-datalist-s-layout-direction"></a>Krok 2. zmiana kierunku układu DataList

Chociaż domyślnym zachowaniem elementu DataList jest układ elementów w pionie w jednokolumnowej tabeli wielowierszowej, to zachowanie można łatwo zmienić za pomocą [właściwości`RepeatDirection`](https://msdn.microsoft.com/system.web.ui.webcontrols.datalist.repeatdirection.aspx)DataList s. Właściwość `RepeatDirection` może przyjmować jedną z dwóch możliwych wartości: `Horizontal` lub `Vertical` (wartość domyślna).

Zmieniając właściwość `RepeatDirection` z `Vertical` na `Horizontal`, obiekt DataList renderuje swoje rekordy w pojedynczym wierszu, tworząc jedną kolumnę na element źródła danych. Aby zilustrować ten efekt, kliknij ikonę DataList w projektancie, a następnie w okno Właściwości zmień właściwość `RepeatDirection` z `Vertical` na `Horizontal`. Natychmiast po wykonaniu tej czynności Projektant dostosowuje układ DataList s, tworząc jednowierszowy, wielokolumnowy interfejs (patrz rysunek 3).

[![Właściwość RepeatDirection określa kierunek, w jakim są ułożone elementy DataList s](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image8.png)](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image7.png)

**Rysunek 3**. Właściwość `RepeatDirection` określa kierunek, w jaki elementy DataList s są układane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image9.png))

W przypadku wyświetlania małych ilości danych jednowierszowa tabela wielokolumnowa może być idealnym sposobem maksymalizowania nieruchomości ekranu. W przypadku większych ilości danych jeden wiersz będzie wymagał wielu kolumn, które wypychają te elementy, które mogą przyłączyć się na ekranie do prawej strony. Na rysunku 4 przedstawiono produkty, które są renderowane w pojedynczym wierszu DataList. Ponieważ istnieje wiele produktów (ponad 80), użytkownik będzie musiał przewinąć w prawo, aby wyświetlić informacje o każdym z tych produktów.

[![dla dostatecznie dużych źródeł danych, jedna kolumna DataList będzie wymagała przewijania w poziomie](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image11.png)](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image10.png)

**Rysunek 4**. w przypadku dostatecznie dużych źródeł danych jedna kolumna DataList będzie wymagała przewijania w poziomie ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image12.png))

## <a name="step-3-displaying-data-in-a-multi-column-multi-row-table"></a>Krok 3. Wyświetlanie danych w wielokolumnowej tabeli wielowierszowej

Aby utworzyć wielokolumnową kolumnę DataList wielowierszowego, musimy ustawić [właściwość`RepeatColumns`](https://msdn.microsoft.com/system.web.ui.webcontrols.datalist.repeatcolumns.aspx) na liczbę kolumn do wyświetlenia. Domyślnie właściwość `RepeatColumns` jest ustawiona na 0, co spowoduje wyświetlenie wszystkich elementów w pojedynczym wierszu lub kolumnie (w zależności od wartości właściwości `RepeatDirection`).

W naszym przykładzie pozwól, aby w wierszu tabeli były wyświetlane trzy produkty. W związku z tym ustaw właściwość `RepeatColumns` na 3. Po wprowadzeniu tej zmiany Poświęć chwilę na wyświetlenie wyników w przeglądarce. Jak pokazano na rysunku 5, produkty są teraz wyświetlane w trzech kolumnach tabeli wielowierszowej.

[![trzy produkty są wyświetlane na wiersz](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image14.png)](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image13.png)

**Rysunek 5**. trzy produkty są wyświetlane na wiersz ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image15.png))

Właściwość `RepeatDirection` wpływa na sposób, w jaki elementy w elemencie DataList są układane. Rysunek 5 przedstawia wyniki z właściwością `RepeatDirection` ustawioną na `Horizontal`. Należy zauważyć, że pierwsze trzy produkty Chai, zmian i anyżu syropu są określane od lewej do prawej, od góry do dołu. Kolejne trzy produkty (począwszy od Chef Anton s Cajun) pojawiają się w wierszu poniżej trzech pierwszych. Jednak zmiana właściwości `RepeatDirection` z powrotem na `Vertical`, powoduje, że te produkty są układane od góry do dołu, od lewej do prawej, jak pokazano na rysunku 6.

[![tym miejscu produkty są rozłożone w pionie](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image17.png)](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image16.png)

**Ilustracja 6**. w tym miejscu produkty są ułożone pionowo ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image18.png))

Liczba wierszy wyświetlanych w tabeli wyników zależy od liczby wszystkich rekordów powiązanych z elementem DataList. Dokładne, a to s górny limit łącznej liczby elementów źródła danych podzielony przez wartość właściwości `RepeatColumns`. Ponieważ tabela `Products` ma obecnie 84 produktów, które są podzielne przez 3, istnieją 28 wierszy. Jeśli liczba elementów w źródle danych i `RepeatColumns` wartość właściwości nie są widoczne, ostatni wiersz lub kolumna będzie zawierać puste komórki. Jeśli `RepeatDirection` jest ustawiona na `Vertical`, Ostatnia kolumna będzie miała puste komórki; Jeśli `RepeatDirection` jest `Horizontal`, ostatni wiersz będzie miał puste komórki.

## <a name="summary"></a>Podsumowanie

Domyślnie element DataList wyświetla listę elementów w jednokolumnowej tabeli wielowierszowej, która naśladuje układ elementu GridView jednym TemplateField. Gdy ten domyślny układ jest akceptowalny, możemy zmaksymalizować zawartość ekranu, wyświetlając wiele elementów źródła danych na wiersz. W tym celu wystarczy ustawić Właściwość DataList s `RepeatColumns` na liczbę kolumn do wyświetlenia w wierszu. Ponadto Właściwość `RepeatDirection` DataList s może służyć do wskazywania, czy zawartość wielokolumnowej tabeli wielowierszowej powinna być określona w poziomie od lewej do prawej, od góry do dołu lub w pionie od góry do dołu, od lewej do prawej.

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Recenzent potencjalnych klientów dla tego samouczka był John suru. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](formatting-the-datalist-and-repeater-based-upon-data-cs.md)
> [dalej](nested-data-web-controls-cs.md)

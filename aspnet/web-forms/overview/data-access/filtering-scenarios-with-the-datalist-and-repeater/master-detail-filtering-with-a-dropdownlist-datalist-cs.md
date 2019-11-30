---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-with-a-dropdownlist-datalist-cs
title: Filtrowanie wzorców/szczegółów przy użyciu elementu DropDownListC#() | Microsoft Docs
author: rick-anderson
description: W tym samouczku pokazano, jak wyświetlić raporty wzorzec/szczegóły na pojedynczej stronie sieci Web przy użyciu kontrolek DropDownList, aby wyświetlić rekordy "Master" i DataList do DISPL...
ms.author: riande
ms.date: 07/18/2007
ms.assetid: 07fa47ae-e491-4a2f-b265-d342b9ddef46
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-with-a-dropdownlist-datalist-cs
msc.type: authoredcontent
ms.openlocfilehash: 8289f46fd6d0143802269d5c6196a4c40db9378c
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74631113"
---
# <a name="masterdetail-filtering-with-a-dropdownlist-c"></a>Filtrowanie rekordu głównego/szczegółów przy użyciu kontrolki DropDownList (C#)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_33_CS.exe) lub [Pobierz plik PDF](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/datatutorial33cs1.pdf)

> W tym samouczku przedstawiono sposób wyświetlania raportów wzorzec/szczegóły na pojedynczej stronie sieci Web przy użyciu kontrolek DropDownList do wyświetlania "Master" rekordów i DataList, aby wyświetlić "Szczegóły".

## <a name="introduction"></a>Wprowadzenie

Raport główny/szczegółowy, który został najpierw utworzony przy użyciu widoku GridView w wcześniejszym [filtrowaniu wzorzec/szczegóły za pomocą](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) samouczka DropDownList, rozpoczyna się od wyświetlania pewnego zestawu rekordów "Master". Następnie użytkownik może przejść do szczegółów jednego z rekordów głównych, co spowoduje wyświetlenie "szczegółów" tego rekordu głównego. Raporty wzorzec/szczegóły są idealnym wyborem dla wizualizacji relacji jeden-do-wielu i wyświetlania szczegółowych informacji z szczególnie "szerokich" tabel (z których każda zawiera wiele kolumn). Poznasz sposób implementacji raportów master/detail przy użyciu kontrolek GridView i DetailsView w poprzednich samouczkach. W tym samouczku i następnych dwóch, będziemy ponownie poznać te koncepcje, ale zamiast tego skup się na nich przy użyciu kontrolek DataList i wzmacniak.

W tym samouczku Przyjrzyjmy się użyciu DropDownList, aby zawierała rekordy "Master" z rekordami "Details" wyświetlanymi w elemencie DataList.

## <a name="step-1-adding-the-masterdetail-tutorial-web-pages"></a>Krok 1. Dodawanie stron sieci Web samouczka głównego/szczegółów

Przed rozpoczęciem pracy z tym samouczkiem Poświęćmy chwilę na dodanie folderu i ASP.NET stron, które będziemy potrzebować dla tego samouczka, a następnie dwie instrukcje dotyczące raportów wzorzec/szczegóły przy użyciu kontrolek DataList i wzmacniak. Zacznij od utworzenia nowego folderu w projekcie o nazwie `DataListRepeaterFiltering`. Następnie Dodaj następujące pięć stron ASP.NET do tego folderu, z których wszystkie są skonfigurowane do korzystania z strony wzorcowej `Site.master`:

- `Default.aspx`
- `FilterByDropDownList.aspx`
- `CategoryListMaster.aspx`
- `ProductsForCategoryDetails.aspx`
- `CategoriesAndProducts.aspx`

![Utwórz folder DataListRepeaterFiltering i Dodaj samouczek ASP.NET stron](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image1.png)

**Rysunek 1**. tworzenie folderu `DataListRepeaterFiltering` i dodawanie stron samouczka ASP.NET

Następnie otwórz stronę `Default.aspx` i przeciągnij kontrolkę użytkownika `SectionLevelTutorialListing.ascx` z folderu `UserControls` na powierzchnię projektu. Ta kontrolka użytkownika, utworzona na [stronach wzorcowych i](../introduction/master-pages-and-site-navigation-cs.md) w samouczku nawigacji po witrynie, wylicza mapę witryny i wyświetla samouczki z bieżącej sekcji na liście punktowanej.

[![dodać kontrolkę użytkownika SectionLevelTutorialListing. ascx do default. aspx](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image3.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image2.png)

**Rysunek 2**. Dodawanie kontrolki użytkownika `SectionLevelTutorialListing.ascx` do `Default.aspx` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image4.png))

Aby na liście punktowanej były wyświetlane samouczki wzorca/szczegółów, które będą tworzone, należy dodać je do mapy witryny. Otwórz plik `Web.sitemap` i Dodaj następujący znacznik po "Wyświetlanie danych z elementem DataList i wzmacniak" znacznikiem węzła mapy witryny:

[!code-xml[Main](master-detail-filtering-with-a-dropdownlist-datalist-cs/samples/sample1.xml)]

![Aktualizowanie mapy witryny w celu uwzględnienia nowych stron ASP.NET](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image5.png)

**Rysunek 3**. aktualizowanie mapy witryny w celu uwzględnienia nowych stron ASP.NET

## <a name="step-2-displaying-the-categories-in-a-dropdownlist"></a>Krok 2. Wyświetlanie kategorii w DropDownList

Nasz Raport główny/szczegółowy wyświetli listę kategorii w DropDownList, z których produkty wybranego elementu listy są wyświetlane w dalszej części na stronie elementu DataList. Pierwsze zadanie przed nami, a następnie, ma mieć kategorie wyświetlane w DropDownList. Aby rozpocząć, Otwórz stronę `FilterByDropDownList.aspx` w folderze `DataListRepeaterFiltering` i przeciągnij DropDownList z przybornika na projektanta strony. Następnie ustaw właściwość `ID` DropDownList na `Categories`. Kliknij link wybierz źródło danych z tagu inteligentnego DropDownList i Utwórz nowy element ObjectDataSource o nazwie `CategoriesDataSource`.

[![dodać nowego elementu ObjectDataSource o nazwie CategoriesDataSource](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image7.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image6.png)

**Ilustracja 4**. Dodawanie nowego elementu ObjectDataSource o nazwie `CategoriesDataSource` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image8.png))

Skonfiguruj nowy element ObjectDataSource, który wywołuje metodę `GetCategories()` klasy `CategoriesBLL`. Po skonfigurowaniu elementu ObjectDataSource nadal musimy określić, jakie pole źródła danych ma być wyświetlane w DropDownList, a które należy skojarzyć z wartością dla każdego elementu listy. Pole `CategoryName`, które ma być wyświetlane i `CategoryID`, jako wartość dla każdego elementu listy.

[![wyświetlać DropDownList pole CategoryName i używać IDkategorii jako wartości](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image10.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image9.png)

**Rysunek 5**. DropDownList wyświetlić pola `CategoryName` i użyj `CategoryID` jako wartości ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image11.png))

W tym momencie mamy kontrolkę DropDownList, która jest wypełniana rekordami z tabeli `Categories` (wszystkie wykonane przez około 6 sekund). Rysunek 6 przedstawia postęp z tego względu, gdy jest wyświetlany za pomocą przeglądarki.

[![listy rozwijanej z bieżącymi kategoriami](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image13.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image12.png)

**Ilustracja 6**. Lista rozwijana z bieżącymi kategoriami ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image14.png))

## <a name="step-2-adding-the-products-datalist"></a>Krok 2. Dodawanie elementu DataList produktów

Ostatnim krokiem w naszym raporcie głównym/szczegółowym jest wyświetlenie listy produktów skojarzonych z wybraną kategorią. Aby to osiągnąć, Dodaj element DataList do strony i Utwórz nowy element ObjectDataSource o nazwie `ProductsByCategoryDataSource`. Czy formant `ProductsByCategoryDataSource` pobiera swoje dane z metody `GetProductsByCategoryID(categoryID)` klasy `ProductsBLL`. Ponieważ ten Raport główny/szczegółowy jest tylko do odczytu, wybierz opcję (brak) na kartach Wstawianie, aktualizowanie i usuwanie.

[![wybrać metodę GetProductsByCategoryID (IDKategorii)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image16.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image15.png)

**Rysunek 7**. wybierz metodę `GetProductsByCategoryID(categoryID)` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image17.png))

Po kliknięciu przycisku Dalej Kreator ObjectDataSource poprosi nas o źródło wartości parametru *`categoryID`* `GetProductsByCategoryID(categoryID)` metody. Aby użyć wartości wybranego `categories` elementu DropDownList Ustaw Źródło parametru na Control i ControlID, aby `Categories`.

[![ustawić dla parametru IDKategorii wartości kategorii DropDownList](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image19.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image18.png)

**Ilustracja 8**. ustaw parametr *`categoryID`* na wartość `Categories` DropDownList ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image20.png))

Po zakończeniu działania Kreatora konfiguracji źródła danych program Visual Studio automatycznie wygeneruje `ItemTemplate` dla elementu DataList, który wyświetla nazwę i wartość każdego pola danych. Aby zamiast tego użyć `ItemTemplate`, w którym jest wyświetlana tylko nazwa produktu, Kategoria, dostawca, ilość na jednostkę i cena oraz `SeparatorTemplate`, które wprowadzają element `<hr>` między każdym elementem, przystąpi do. Używam `ItemTemplate` z przykładu w [wyświetlanych danych za pomocą samouczka z kontrolkami DataList i wzmacniak](../displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-cs.md) , ale możesz korzystać z dowolnego znacznika szablonu, który znajdziesz najbardziej atrakcyjny wizualnie.

Po wprowadzeniu tych zmian element DataList i jego znaczniki elementu ObjectDataSource powinny wyglądać podobnie do następujących:

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-datalist-cs/samples/sample2.aspx)]

Poświęć chwilę, aby zaewidencjonować nasz postęp w przeglądarce. Podczas pierwszego odwiedzania strony są wyświetlane te produkty należące do wybranej kategorii (napoje) (jak pokazano na rysunku 9), ale zmiana DropDownList nie aktualizuje danych. Wynika to z faktu, że w celu zaktualizowania elementu DataList musi wystąpić ogłoszenie zwrotne. Aby to osiągnąć, można ustawić właściwość `AutoPostBack` DropDownList na `true` lub dodać formant sieci Web przycisku do strony. Na potrzeby tego samouczka ustawiłem Właściwość `AutoPostBack` DropDownList na `true`.

Ilustracje 9 i 10 ilustrują Raport główny/szczegółowy w działaniu.

[![podczas pierwszego odwiedzania strony zostaną wyświetlone produkty napoje](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image22.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image21.png)

**Rysunek 9**. podczas pierwszego odwiedzania strony są wyświetlane produkty napoje ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image23.png))

[![wybranie nowego produktu (produkt) automatycznie spowoduje odświeżenie, zaktualizowanie elementu DataList](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image25.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image24.png)

**Ilustracja 10**: wybranie nowego produktu (produkt) powoduje automatyczne wypróbowanie ogłaszania zwrotnego, aktualizowanie elementu DataList ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image26.png))

## <a name="adding-a----choose-a-category----list-item"></a>Dodawanie elementu listy "--Wybierz kategorię--"

Gdy po raz pierwszy zostanie odwiedzana strona `FilterByDropDownList.aspx` kategorii (napoje) pierwszego elementu listy DropDownList są domyślnie zaznaczone, pokazując produkty napoje w elemencie DataList. W przypadku *filtrowania wzorzec/szczegóły przy użyciu* samouczka DropDownList dodaliśmy opcję "--Wybierz kategorię--" do DropDownList, która została wybrana domyślnie i, po wybraniu, wyświetla *wszystkie* produkty w bazie danych. Takie podejście było zarządzane podczas tworzenia listy produktów w widoku GridView, ponieważ każdy wiersz produktu zajęł niewielką ilość ekranu. Z elementem DataList, jednak informacje każdego produktu zużywają znacznie większy fragment ekranu. Nadal dodamy opcję "--Wybierz kategorię--" i wybrano ją domyślnie, ale zamiast wyświetlać wszystkie produkty po zaznaczeniu, skonfiguruj ją tak, aby nie pokazywała żadnych produktów.

Aby dodać nowy element listy do DropDownList, przejdź do okno Właściwości i kliknij wielokropek we właściwości `Items`. Dodaj nowy element listy z `Text` "--Wybierz kategorię--" i `Value` `0`.

![Dodaj](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image27.png)

**Ilustracja 11**. Dodawanie elementu "--Wybierz kategorię--"

Alternatywnie możesz dodać element listy, dodając następujący znacznik do DropDownList:

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-datalist-cs/samples/sample3.aspx)]

Ponadto musimy ustawić `AppendDataBoundItems` kontrolki DropDownList na `true`, ponieważ jest ona ustawiona na `false` (wartość domyślna), gdy kategorie są powiązane z DropDownList z elementu ObjectDataSource, zastąpią wszystkie ręcznie dodane elementy listy.

![Ustaw właściwość AppendDataBoundItems na wartość true](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image28.png)

**Ilustracja 12**. ustaw właściwość `AppendDataBoundItems` na wartość true

Z powodu wybrania wartości `0` dla elementu "--Wybierz kategorię--" występuje, ponieważ w systemie nie ma kategorii z wartością `0`, dlatego nie zostanie zwrócony żaden rekord produktu, gdy zostanie wybrany element listy "--Wybierz kategorię--". Aby to potwierdzić, poświęć chwilę na odwiedzenie strony za pomocą przeglądarki. Jak pokazano na rysunku 13, podczas pierwszego wyświetlania strony jest zaznaczone pole wyboru "--Wybierz kategorię--" i nie są wyświetlane żadne produkty.

[![, gdy](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image30.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image29.png)

**Rysunek 13**. po wybraniu elementu listy "--Wybierz kategorię--" nie są wyświetlane żadne produkty ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image31.png))

Jeśli nie chcesz wyświetlać *wszystkich* produktów, gdy wybrana jest opcja "--Wybierz kategorię--", Użyj zamiast niej wartości `-1`. Czytnik uważni będzie odwoływać ten element z powrotem w przypadku *filtrowania wzorzec/szczegóły za pomocą DropDownListego* samouczka zaktualizowaliśmy metodę `GetProductsByCategoryID(categoryID)` klasy `ProductsBLL` tak, aby w przypadku przekazanie *`categoryID`* wartości `-1` wszystkie rekordy produktów zostały zwrócone.

## <a name="summary"></a>Podsumowanie

Podczas wyświetlania danych zależnych od hierarchii często pomocne jest prezentowanie danych przy użyciu raportów master/detail, z których użytkownik może rozpocząć Peru danych od góry hierarchii i przejść do szczegółów. W tym samouczku sprawdzono Tworzenie prostego raportu głównego/szczegółów pokazującego produkty wybranej kategorii. Zostało to osiągnięte przy użyciu DropDownList dla listy kategorii i elementu DataList dla produktów należących do wybranej kategorii.

W następnym samouczku powiesz się, jak oddzielić wzorzec i szczegółowe informacje na dwóch stronach. Na pierwszej stronie zostanie wyświetlona lista "Master" z linkiem umożliwiającym wyświetlenie szczegółów. Kliknięcie linku spowoduje wyróżnienie użytkownika na drugą stronę, co spowoduje wyświetlenie szczegółowych informacji o wybranym rekordzie głównym.

Szczęśliwe programowanie!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla...

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Recenzent potencjalnych klientów dla tego samouczka został Randy Schmidt. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Next](master-detail-filtering-acess-two-pages-datalist-cs.md)

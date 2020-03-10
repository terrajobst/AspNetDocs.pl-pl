---
uid: web-forms/overview/data-access/masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs
title: Wzorzec/Szczegóły przy użyciu wybieranego wzorca GridView ze szczegółowym kontrolką detailview (C#) | Microsoft Docs
author: rick-anderson
description: Ten samouczek będzie miał widok GridView, którego wiersze zawierają nazwę i cenę każdego produktu wraz z przyciskiem SELECT. Kliknięcie przycisku Wybierz dla elementu particu...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 0f982827-f8f9-420d-b36b-57b23f5aa519
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs
msc.type: authoredcontent
ms.openlocfilehash: 04c427341f063729bd23b416a96f5acb20702792
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78621066"
---
# <a name="masterdetail-using-a-selectable-master-gridview-with-a-details-detailview-c"></a>Formularz typu rekord główny/szczegóły korzystający z kontrolki GridView umożliwiającej wybór rekordu głównego z kontrolką DetailView szczegółów (C#)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](https://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_10_CS.exe) lub [Pobierz plik PDF](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/datatutorial10cs1.pdf)

> Ten samouczek będzie miał widok GridView, którego wiersze zawierają nazwę i cenę każdego produktu wraz z przyciskiem SELECT. Kliknięcie przycisku Wybierz dla określonego produktu spowoduje, że wszystkie szczegółowe informacje będą wyświetlane w kontrolce DetailsView na tej samej stronie.

## <a name="introduction"></a>Wprowadzenie

W [poprzednim samouczku](master-detail-filtering-across-two-pages-cs.md) pokazano, jak utworzyć Raport główny/szczegółowy przy użyciu dwóch stron sieci Web: strony sieci Web "Master", z której została wyświetlona lista dostawców. oraz strona sieci Web "Szczegóły", która zawiera te produkty dostarczone przez wybranego dostawcę. Ten dwustronicowy format raportu może być zagęszczony na jednej stronie. Ten samouczek będzie miał widok GridView, którego wiersze zawierają nazwę i cenę każdego produktu wraz z przyciskiem SELECT. Kliknięcie przycisku Wybierz dla określonego produktu spowoduje, że wszystkie szczegółowe informacje będą wyświetlane w kontrolce DetailsView na tej samej stronie.

[![kliknij przycisk Wybierz, aby wyświetlić szczegóły produktu](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image2.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image1.png)

**Rysunek 1**. kliknięcie przycisku Wybierz spowoduje wyświetlenie szczegółów produktu ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image3.png))

## <a name="step-1-creating-a-selectable-gridview"></a>Krok 1. Tworzenie widoku GridView do wyboru

Odwołaj ten element w raporcie wzorzec/szczegóły dwustronicowy, który każdy rekord główny zawiera hiperłącze, które po kliknięciu wysłało użytkownika do strony szczegółów, przekazując kliknięty `SupplierID` wartość w ciągu QueryString. Takie hiperłącze zostało dodane do każdego wiersza GridView przy użyciu HyperLinkField. W przypadku raportu wzorzec/szczegóły pojedynczej strony będziemy potrzebować przycisku dla każdego wiersza GridView, który po kliknięciu pokazuje szczegółowe informacje. Formant GridView można skonfigurować tak, aby zawierał przycisk wyboru dla każdego wiersza, który powoduje odświeżenie i oznacza, że ten wiersz jest [SelectedRowem](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedrow.aspx)GridView.

Zacznij od dodania kontrolki GridView do strony `DetailsBySelecting.aspx` w folderze `Filtering`, ustawiając jej Właściwość `ID` na `ProductsGrid`. Następnie Dodaj nowy element ObjectDataSource o nazwie `AllProductsDataSource`, który wywołuje metodę `GetProducts()` klasy `ProductsBLL`.

[![utworzyć elementu ObjectDataSource o nazwie AllProductsDataSource](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image5.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image4.png)

**Rysunek 2**. Tworzenie elementu ObjectDataSource o nazwie `AllProductsDataSource` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image6.png))

[![użyć klasy ProductsBLL](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image8.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image7.png)

**Rysunek 3**. Korzystanie z klasy `ProductsBLL` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image9.png))

[![skonfigurować element ObjectDataSource do wywoływania metody getProducts ()](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image11.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image10.png)

**Ilustracja 4**. Konfigurowanie elementu ObjectDataSource do wywoływania metody `GetProducts()` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image12.png))

Edytuj pola GridView, usuwając wszystkie z nich oprócz `ProductName` i `UnitPrice` BoundFields. Ponadto możesz dostosowywać te BoundFields w miarę potrzeby, jak formatowanie `UnitPrice` BoundField jako waluta i zmiana właściwości `HeaderText` BoundFields. Kroki te można wykonać graficznie, klikając łącze Edytuj kolumny w tagu inteligentnym GridView lub ręcznie konfigurując składnię deklaratywną.

[![usunąć wszystkie oprócz ProductName i CenaJednostkowa BoundFields](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image14.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image13.png)

**Rysunek 5**. Usuń wszystkie oprócz `ProductName` i `UnitPrice` BoundFields ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image15.png))

Końcowa wartość znacznika GridView to:

[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/samples/sample1.aspx)]

Następnie musimy oznaczyć widok GridView jako zaznaczony, co spowoduje dodanie przycisku wyboru do każdego wiersza. Aby to osiągnąć, po prostu zaznacz pole wyboru Włącz zaznaczenie w tagu inteligentnym GridView.

[![uczynić wierszy GridView jako zaznaczone](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image17.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image16.png)

**Ilustracja 6**. Przekształć wiersze GridView w wybrane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image18.png))

Zaznaczenie opcji włączania wyboru powoduje dodanie CommandField do `ProductsGrid` GridView z właściwością `ShowSelectButton` ustawioną na wartość true. Spowoduje to wybranie przycisku wyboru dla każdego wiersza GridView, jak pokazano na rysunku 6. Domyślnie przyciski SELECT są renderowane jako LinkButtons, ale można użyć przycisków lub ImageButtons zamiast właściwości `ButtonType` CommandField.

[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/samples/sample2.aspx)]

Gdy klikniesz przycisk wyboru wiersza GridView, nastąpi odświeżenie, a właściwość `SelectedRow` GridView zostanie zaktualizowana. Poza właściwością `SelectedRow` GridView zawiera właściwości [SelectedIndex](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedindex%28VS.80%29.aspx), [SelectedValue](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedvalue%28VS.80%29.aspx)i [SelectedDataKey](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selecteddatakey%28VS.80%29.aspx) . Właściwość `SelectedIndex` zwraca indeks wybranego wiersza, podczas gdy właściwości `SelectedValue` i `SelectedDataKey` zwracają wartości na podstawie [Właściwości DataKeyNames ustawionej](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.datakeynames%28VS.80%29.aspx)GridView.

Właściwość `DataKeyNames` służy do kojarzenia co najmniej jednej wartości pola danych z każdym wierszem i jest często używana do unikatowego identyfikowania informacji z danych źródłowych z każdym wierszem GridView. Właściwość `SelectedValue` zwraca wartość pierwszego pola danych `DataKeyNames` dla wybranego wiersza, gdzie jako właściwość `SelectedDataKey` zwraca obiekt `DataKey` wybranego wiersza, który zawiera wszystkie wartości dla określonych pól klucza danych dla tego wiersza.

Właściwość `DataKeyNames` jest automatycznie ustawiana na pola danych z unikatowymi tożsamościami podczas powiązania źródła danych z elementem GridView, DetailsView lub FormView za pomocą projektanta. Mimo że ta właściwość została automatycznie ustawiona dla nas w poprzednich samouczkach, te przykłady byłyby działały bez określonej właściwości `DataKeyNames`. Jednak dla elementu GridView z wybieraniem w tym samouczku, a także dla przyszłych samouczków, w których będziemy sprawdzać Wstawianie, aktualizowanie i usuwanie, właściwość `DataKeyNames` musi być ustawiona poprawnie. Poświęć chwilę, aby upewnić się, że właściwość `DataKeyNames` GridView jest ustawiona na `ProductID`.

Obejrzyjmy nasz postęp w tym zakresie za pomocą przeglądarki. Należy pamiętać, że w widoku GridView wyświetlana jest nazwa i cena wszystkich produktów wraz z SELECT element LinkButton. Kliknięcie przycisku Wybierz spowoduje odświeżenie. W kroku 2 zobaczymy, jak mieć element DetailsView odpowiada na to ogłaszanie zwrotne, wyświetlając szczegóły wybranego produktu.

[![każdy wiersz produktu zawiera element SELECT element LinkButton](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image20.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image19.png)

**Rysunek 7**: każdy wiersz produktu zawiera element LinkButton ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image21.png))

### <a name="highlighting-the-selected-row"></a>Wyróżnianie zaznaczonego wiersza

`ProductsGrid` GridView ma właściwość `SelectedRowStyle`, która może służyć do dyktowania stylu wizualizacji wybranego wiersza. Używane prawidłowo, dzięki czemu można poprawić środowisko użytkownika, dokładniej pokazując, który wiersz widoku GridView jest obecnie zaznaczony. Na potrzeby tego samouczka zostanie wyróżniony wybrany wiersz z żółtym tłem.

Podobnie jak w przypadku naszych wcześniejszych samouczków, warto zadbać o to, aby ustawienia estetyczne były zdefiniowane jako klasy CSS. W związku z tym Utwórz nową klasę CSS w `Styles.css` o nazwie `SelectedRowStyle`.

[!code-css[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/samples/sample3.css)]

Aby zastosować tę klasę CSS do właściwości `SelectedRowStyle` *wszystkich* elementów GridView w naszej serii samouczków, Edytuj skórkę `GridView.skin` w motywie `DataWebControls`, aby uwzględnić ustawienia `SelectedRowStyle`, jak pokazano poniżej:

[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/samples/sample4.aspx)]

W tym przypadku wybrany wiersz GridView jest teraz wyróżniony żółtym kolorem tła.

[![dostosować wygląd zaznaczonego wiersza przy użyciu właściwości SelectedRowStyle GridView](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image23.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image22.png)

**Ilustracja 8**. Dostosowywanie wyglądu zaznaczonego wiersza za pomocą właściwości `SelectedRowStyle` GridView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image24.png))

## <a name="step-2-displaying-the-selected-products-details-in-a-detailsview"></a>Krok 2. Wyświetlanie szczegółowych informacji o wybranym produkcie w widoku DetailsView

Po zakończeniu `ProductsGrid` widoku GridView wszystko to jest konieczne dodanie widoku DetailsView, który wyświetla informacje o wybranym produkcie. Dodaj kontrolkę DetailsView powyżej widoku GridView i Utwórz nowy element ObjectDataSource o nazwie `ProductDetailsDataSource`. Ponieważ chcemy, aby ten DetailsView wyświetlał określone informacje o wybranym produkcie, skonfiguruj `ProductDetailsDataSource` tak, aby korzystał z metody `GetProductByProductID(productID)` klasy `ProductsBLL`.

[![wywołać metodę GetProductByProductID (productID) klasy ProductsBLL](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image26.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image25.png)

**Rysunek 9**. Wywołaj metodę `GetProductByProductID(productID)` klasy `ProductsBLL` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image27.png))

Wartość parametru *`productID`* uzyskana z właściwości `SelectedValue` kontrolki GridView. Jak wspomniano wcześniej, właściwość `SelectedValue` GridView zwraca pierwszą wartość klucza danych dla wybranego wiersza. W związku z tym jest konieczne, aby Właściwość `DataKeyNames` GridView była ustawiona na `ProductID`, dzięki czemu `ProductID` wartość wybranego wiersza jest zwracana przez `SelectedValue`.

[![ustawić parametru productID na Właściwość SelectedValue GridView](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image29.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image28.png)

**Ilustracja 10**. ustaw parametr *`productID`* na Właściwość `SelectedValue` GridView ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image30.png))

Po poprawnym skonfigurowaniu elementu ObjectDataSource `productDetailsDataSource` i powiązaniu go z elementem DetailsView ten samouczek został ukończony. Gdy strona jest najpierw odwiedzona, nie jest zaznaczony żaden wiersz, więc Właściwość `SelectedValue` GridView zwraca `null`. Ponieważ nie ma żadnych produktów z `NULL` `ProductID` wartością, żadne rekordy nie są zwracane przez metodę `GetProductByProductID(productID)`, co oznacza, że element DetailsView nie jest wyświetlany (patrz rysunek 11). Po kliknięciu przycisku SELECT w wierszu GridView zostanie odświeżone odświeżenie i widok DetailsView. Tym razem Właściwość `SelectedValue` GridView zwraca `ProductID` zaznaczonego wiersza, Metoda `GetProductByProductID(productID)` zwróci `ProductsDataTable` z informacjami o tym konkretnym produkcie, a widok DetailsView wyświetli te szczegóły (Zobacz Rysunek 12).

[![podczas pierwszej wizyty zostanie wyświetlony tylko widok GridView](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image32.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image31.png)

**Rysunek 11**. podczas pierwszej wizyty wyświetlana jest tylko wartość GridView ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image33.png))

[![po zaznaczeniu wiersza są wyświetlane szczegóły produktu](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image35.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image34.png)

**Ilustracja 12**. po wybraniu wiersza są wyświetlane szczegóły produktu ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image36.png))

## <a name="summary"></a>Podsumowanie

W tym i powyższych trzech samouczkach zaobserwowano kilka technik wyświetlania raportów głównych/szczegółowych. W tym samouczku sprawdzono użycie widoku GridView do przechowywania rekordów głównych i widoku DetailsView w celu wyświetlenia szczegółowych informacji o wybranym rekordzie głównym na tej samej stronie. We wcześniejszych samouczkach oglądamy sposób wyświetlania raportów wzorzec/szczegóły przy użyciu kontrolek DropDownList i wyświetlania rekordów głównych na jednej stronie sieci Web i szczegółowych rekordów na innym.

W tym samouczku zakończymy badanie raportów master/detail. Począwszy od następnego samouczka, rozpocznie się Eksplorowanie dostosowanego formatowania za pomocą widoku GridView, DetailsView i FormView. Zobaczymy, jak dostosować wygląd tych kontrolek w oparciu o powiązane z nimi dane, jak podsumowywać dane w stopce GridView oraz jak korzystać z szablonów, aby uzyskać większą kontrolę nad układem.

Szczęśliwe programowanie!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Recenzent potencjalnych klientów dla tego samouczka został Hilton Giesenow. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](master-detail-filtering-across-two-pages-cs.md)
> [dalej](master-detail-filtering-with-a-dropdownlist-vb.md)

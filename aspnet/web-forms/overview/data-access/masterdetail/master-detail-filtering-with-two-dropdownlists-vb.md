---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-with-two-dropdownlists-vb
title: Filtrowanie rekordu głównego/szczegółów przy użyciu dwóch kontrolek DROPDOWNLIST (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym samouczku rozwija relacji wzorzec/szczegół, aby dodać warstwę trzeci, za pomocą dwóch kontrolek DropDownList wybrać żądaną recor nadrzędne i nadrzędnych...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 11ae4f64-01ba-4823-95f4-a2fe1f84f7d7
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-with-two-dropdownlists-vb
msc.type: authoredcontent
ms.openlocfilehash: 20df98f1aacb046bb9ec9fa5ad03e008dc234509
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57070673"
---
<a name="masterdetail-filtering-with-two-dropdownlists-vb"></a>Filtrowanie rekordu głównego/szczegółów przy użyciu dwóch kontrolek DropDownList (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_8_VB.exe) lub [Pobierz plik PDF](master-detail-filtering-with-two-dropdownlists-vb/_static/datatutorial08vb1.pdf)

> W tym samouczku rozwija relacji wzorzec/szczegół, aby dodać warstwę trzeci, za pomocą dwóch kontrolek DropDownList do wybierania żądanego rekordów nadrzędnych i nadrzędnych.


## <a name="introduction"></a>Wprowadzenie

W [poprzedniego samouczka](master-detail-filtering-with-a-dropdownlist-vb.md) zbadaliśmy sposób wyświetlania raportu proste wzorzec/szczegół za pomocą pojedynczego kontrolki DropDownList wypełniony kategorie i GridView, wyświetlanie tych produktów, które należą do wybranej kategorii. Ten wzorzec raportu dobrze działa w przypadku wyświetlania rekordów relacji jeden do wielu, które można łatwo rozszerzyć, aby działać w przypadku scenariuszy obejmujących wiele relacji jeden do wielu. Na przykład system wprowadzania zamówień musi tabel, które odpowiadają klienci, zamówienia i kolejności pozycji. Dany klient może mieć wiele zamówień za pomocą każdego zamówienia składający się z wielu elementów. Takie dane widoczne dla użytkownika za pomocą dwóch kontrolek DROPDOWNLIST i GridView. Pierwszy DropDownList miałby elementu listy dla każdego klienta w bazie danych, drugi jednostkowego zawartości jest zamówień dla wybranego odbiorcy. GridView zwróciłaby listę pozycji z wybranego zamówienia.

Chociaż bazy danych Northwind zawiera informacje canonical szczegóły klienta/zamówienia w jego `Customers`, `Orders`, i `Order Details` tabel, tabele te nie są rejestrowane w naszej architektury. Niemniej jednak firma Microsoft może nadal pokazują przy użyciu dwóch kontrolek DROPDOWNLIST zależnych. Pierwszy DropDownList wyświetli listę kategorii, a druga produktów należących do wybranej kategorii. Element DetailsView zostanie wyświetlona lista szczegółów wybranego produktu.

## <a name="step-1-creating-and-populating-the-categories-dropdownlist"></a>Krok 1. Tworzenie i wypełnianie DropDownList kategorii

Naszym pierwszym celem jest można dodać kontrolki DropDownList, który wyświetla listę kategorii. Te kroki zostały szczegółowo zbadane w poprzednim samouczku, ale są podsumowywane w tym miejscu, aby informacje były kompletne.

Otwórz `MasterDetailsDetails.aspx` strony w `Filtering` folderu, na stronie, Dodaj kontrolki DropDownList ustaw jego `ID` właściwości `Categories`, a następnie kliknij łącze Konfiguruj źródła danych w jego tagu inteligentnego. Z Kreatora konfiguracji źródła danych wybierz dodać nowe źródło danych.


[![Dodaj nowe źródło danych dla metody DropDownList](master-detail-filtering-with-two-dropdownlists-vb/_static/image2.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image1.png)

**Rysunek 1**: Dodaj nowe źródło danych dla metody DropDownList ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-two-dropdownlists-vb/_static/image3.png))


Nowe źródło danych należy oczywiście ObjectDataSource. Nazwa tej nowej kontrolki ObjectDataSource `CategoriesDataSource` i Wywołaj `CategoriesBLL` obiektu `GetCategories()` metody.


[![Wybierz użyć klasy CategoriesBLL](master-detail-filtering-with-two-dropdownlists-vb/_static/image5.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image4.png)

**Rysunek 2**: Możliwość użycia `CategoriesBLL` klasy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-two-dropdownlists-vb/_static/image6.png))


[![Konfigurowanie kontrolki ObjectDataSource przy użyciu metody GetCategories()](master-detail-filtering-with-two-dropdownlists-vb/_static/image8.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image7.png)

**Rysunek 3**: Konfigurowanie kontrolki ObjectDataSource do użycia `GetCategories()` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-two-dropdownlists-vb/_static/image9.png))


Po skonfigurowaniu kontrolki ObjectDataSource nadal trzeba określić które pole źródła danych powinny być wyświetlane w `Categories` DropDownList i który z nich powinna być skonfigurowana jako wartość dla elementu listy. Ustaw `CategoryName` pola jako ekran i `CategoryID` jako wartość dla każdego elementu listy.


[![Masz wyświetlana lista DropDownList na pole CategoryName i użyj CategoryID jako wartość](master-detail-filtering-with-two-dropdownlists-vb/_static/image11.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image10.png)

**Rysunek 4**: Ma ekran DropDownList `CategoryName` pola i użyj `CategoryID` jako wartość ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-two-dropdownlists-vb/_static/image12.png))


W tym momencie mamy kontrolki DropDownList (`Categories`), jest wypełniana przy użyciu rekordów z `Categories` tabeli. Gdy użytkownik wybierze nową kategorię z metody DropDownList chcemy zwrotu wystąpić, aby odświeżyć produktu DropDownList, której będziemy tworzyć w kroku 2. W związku z tym, zaznacz opcję włączenia automatycznego ogłaszania zwrotnego z `categories` DropDownList w tagu inteligentnego.


[![Povolit vlastnost AutoPostBack dla metody DropDownList kategorii](master-detail-filtering-with-two-dropdownlists-vb/_static/image14.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image13.png)

**Rysunek 5**: Povolit vlastnost AutoPostBack dla `Categories` DropDownList ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-two-dropdownlists-vb/_static/image15.png))


## <a name="step-2-displaying-the-selected-categorys-products-in-a-second-dropdownlist"></a>Krok 2. Wyświetlanie wybranej kategorii produktów w drugiej kontrolki DropDownList

Za pomocą `Categories` DropDownList zakończona, naszym kolejnym krokiem jest wyświetlenie kontrolki DropDownList produktów należących do wybranej kategorii. W tym celu na stronie o nazwie Dodaj inny DropDownList `ProductsByCategory`. Podobnie jak w przypadku `Categories` DropDownList, Utwórz nowe kontrolki ObjectDataSource dla `ProductsByCategory` DropDownList o nazwie `ProductsByCategoryDataSource`.


[![Dodaj nowe źródło danych dla kontrolki ProductsByCategory DropDownList](master-detail-filtering-with-two-dropdownlists-vb/_static/image17.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image16.png)

**Rysunek 6**: Dodaj nowe źródło danych dla `ProductsByCategory` DropDownList ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-two-dropdownlists-vb/_static/image18.png))


[![Tworzenie nowego elementu ObjectDataSource, o nazwie ProductsByCategoryDataSource](master-detail-filtering-with-two-dropdownlists-vb/_static/image20.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image19.png)

**Rysunek 7**: Utwórz nowy o nazwie elementu ObjectDataSource `ProductsByCategoryDataSource` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-two-dropdownlists-vb/_static/image21.png))


Ponieważ `ProductsByCategory` DropDownList potrzeb, aby wyświetlić tylko te produkty należące do wybranej kategorii mają ObjectDataSource wywołania `GetProductsByCategoryID(categoryID)` metody z `ProductsBLL` obiektu.


[![Wybierz użyć klasy ProductsBLL](master-detail-filtering-with-two-dropdownlists-vb/_static/image23.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image22.png)

**Rysunek 8**: Możliwość użycia `ProductsBLL` klasy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-two-dropdownlists-vb/_static/image24.png))


[![Konfigurowanie kontrolki ObjectDataSource przy użyciu metody GetProductsByCategoryID(categoryID)](master-detail-filtering-with-two-dropdownlists-vb/_static/image26.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image25.png)

**Rysunek 9**: Konfigurowanie kontrolki ObjectDataSource do użycia `GetProductsByCategoryID(categoryID)` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-two-dropdownlists-vb/_static/image27.png))


W ostatnim kroku kreatora należy określić wartość *`categoryID`* parametru. Przypisz ten parametr do wybranego elementu z `Categories` DropDownList.


[![Ściąganie categoryID wartości parametru z kontrolki DropDownList kategorii](master-detail-filtering-with-two-dropdownlists-vb/_static/image29.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image28.png)

**Na rysunku nr 10**: Ściągnij *`categoryID`* wartość parametru `Categories` DropDownList ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-two-dropdownlists-vb/_static/image30.png))


Za pomocą kontrolki ObjectDataSource skonfigurowane pozostaje tylko do określenia, jakie pola źródła danych są używane do wyświetlania i wartość DropDownList elementów. Wyświetlanie `ProductName` pola, a następnie użyć `ProductID` pola jako wartość.


[![Określ pola źródła danych, używany dla tekstu i wartości właściwości ListItems DropDownList](master-detail-filtering-with-two-dropdownlists-vb/_static/image32.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image31.png)

**Rysunek 11**: Określ użyć pola źródła danych dla kontrolki DropDownList `ListItem` s " `Text` i `Value` właściwości ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-two-dropdownlists-vb/_static/image33.png))


Za pomocą kontrolki ObjectDataSource i `ProductsByCategory` DropDownList skonfigurowane strony wyświetli dwóch kontrolek DROPDOWNLIST: pierwszy spowoduje wyświetlenie listy wszystkich kategorii podczas gdy druga Wyświetla listę tych produktów należących do wybranej kategorii. Gdy użytkownik wybierze nową kategorię z pierwszej metody DropDownList, nastąpi odświeżenie strony i być odbitych drugiej metody DropDownList, wyświetlanie tych produktów, które należą do nowo wybranej kategorii. Rysunki 12 i 13 Pokaż `MasterDetailsDetails.aspx` w akcji podczas wyświetlania za pośrednictwem przeglądarki.


[![Po pierwsze, odwiedzając stronę, wybranej kategorii Beverages](master-detail-filtering-with-two-dropdownlists-vb/_static/image35.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image34.png)

**Rysunek 12**: Po pierwsze, odwiedzając stronę, wybranej kategorii Beverages ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-two-dropdownlists-vb/_static/image36.png))


[![Wybierając inną kategorię Wyświetla nowej kategorii produktów](master-detail-filtering-with-two-dropdownlists-vb/_static/image38.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image37.png)

**Rysunek 13**: Wybieranie różnych ekranów kategorii produktów do nowej kategorii ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-two-dropdownlists-vb/_static/image39.png))


Obecnie `productsByCategory` jest DropDownList, po zmianie *nie* powoduje odświeżenie strony. Jednakże chcemy zwrotu nastąpi po dodamy DetailsView, aby wyświetlić szczegóły wybranego produktu (krok 3). W związku z tym, zaznacz pole wyboru Włącz automatycznego ogłaszania zwrotnego z `productsByCategory` DropDownList w tagu inteligentnego.


[![Włącz funkcję automatycznego ogłaszania zwrotnego productsByCategory DropDownList](master-detail-filtering-with-two-dropdownlists-vb/_static/image41.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image40.png)

**Rysunek 14**: Włącz funkcję automatycznego ogłaszania zwrotnego `productsByCategory` DropDownList ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-two-dropdownlists-vb/_static/image42.png))


## <a name="step-3-using-a-detailsview-to-display-details-for-the-selected-product"></a>Krok 3. Aby wyświetlić szczegóły dla wybranego produktu przy użyciu DetailsView

Ostatnim krokiem jest, aby wyświetlić szczegóły dotyczące wybranego produktu w DetailsView. Aby to osiągnąć, Dodaj element DetailsView do strony, należy ustawić jego `ID` właściwości `ProductDetails`i Utwórz nowe kontrolki ObjectDataSource dla niego. Konfigurowanie tej kontrolki ObjectDataSource swoich danych z `ProductsBLL` klasy `GetProductByProductID(productID)` metody przy użyciu wybranej wartości `ProductsByCategory` DropDownList dla wartości *`productID`* parametru.


[![Wybierz użyć klasy ProductsBLL](master-detail-filtering-with-two-dropdownlists-vb/_static/image44.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image43.png)

**Rysunek 15**: Możliwość użycia `ProductsBLL` klasy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-two-dropdownlists-vb/_static/image45.png))


[![Konfigurowanie kontrolki ObjectDataSource przy użyciu metody GetProductByProductID(productID)](master-detail-filtering-with-two-dropdownlists-vb/_static/image47.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image46.png)

**Rysunek 16**: Konfigurowanie kontrolki ObjectDataSource do użycia `GetProductByProductID(productID)` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-two-dropdownlists-vb/_static/image48.png))


[![Ściąganie productID wartości parametru z kontrolki ProductsByCategory DropDownList](master-detail-filtering-with-two-dropdownlists-vb/_static/image50.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image49.png)

**Rysunek 17**: Ściągnij *`productID`* wartość parametru `ProductsByCategory` DropDownList ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-two-dropdownlists-vb/_static/image51.png))


Istnieje możliwość wyświetlenia dostępnych polach w `ProductDetails` DetailsView. Czy mogę załączania do usunięcia `ProductID`, `SupplierID`, i `CategoryID` pól i kolejności i sformatowany pozostałych pól. Ponadto, wyczyszczone I DetailsView `Height` i `Width` właściwości, umożliwiając DetailsView rozszerzyć szerokość konieczna najlepszej jakości obrazu swoje dane, niż musieć go ograniczone do określonego rozmiaru. Pełne znaczników pojawia się poniżej:


[!code-aspx[Main](master-detail-filtering-with-two-dropdownlists-vb/samples/sample1.aspx)]

Poświęć chwilę, aby wypróbować `MasterDetailsDetails.aspx` strony w przeglądarce. Na pierwszy rzut oka może występować, że wszystko działa zgodnie z potrzebami, ale drobny problem. Po wybraniu nowej kategorii `ProductsByCategory` DropDownList została zaktualizowana do tych produktów dla wybranej kategorii, ale `ProductDetails` DetailsView w dalszym ciągu Pokaż poprzednie informacje o produkcie. DetailsView jest aktualizowana, wybierając inny produkt dla wybranej kategorii. Ponadto w przypadku testowania w tyle dokładnie znajdziesz, jeśli stale nowe kategorie (takie jak wybieranie Beverages z `Categories` DropDownList, a następnie Condiments, następnie Confections) każdy wybór kategorii spowoduje `ProductDetails`DetailsView odświeżenia.

Aby ułatwić skonkretyzować ten problem, Przyjrzyjmy się konkretnemu przykładowi. Po pierwsze odwiedź stronę do kategorii Beverages jest zaznaczone, a powiązane produkty są ładowane w `ProductsByCategory` DropDownList. Chai jest wybranego produktu i jego szczegóły są wyświetlane w `ProductDetails` DetailsView, jak pokazano na rysunku 18.


[![Szczegóły produktu wybrana są wyświetlane w widoku DetailsView](master-detail-filtering-with-two-dropdownlists-vb/_static/image53.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image52.png)

**Rysunek 18**: Szczegóły produktu wybrana są wyświetlane w widoku DetailsView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-two-dropdownlists-vb/_static/image54.png))


Jeśli zmienisz wybór kategorii z Beverages na Condiments występuje odświeżenie strony i `ProductsByCategory` DropDownList jest odpowiednio aktualizowana, ale Chai DetailsView nadal przedstawia szczegółowe informacje.


[![Szczegóły wcześniej wybrane w produkcie są nadal wyświetlane](master-detail-filtering-with-two-dropdownlists-vb/_static/image56.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image55.png)

**Rysunek 19**: Szczegóły wcześniej wybrane w produkcie są nadal wyświetlane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-two-dropdownlists-vb/_static/image57.png))


Nowy produkt z listy pobrania odświeża DetailsView, zgodnie z oczekiwaniami. W przypadku wybrania nową kategorię po zmianie produktu DetailsView ponownie nie odświeżyć. Jednak jeśli zamiast wybierać nowy produkt został wybrany nową kategorię, DetailsView będzie odświeżyć. Co w świecie dochodzi?

Problem jest błąd chronometrażu w cyklu życia strony. Zawsze, gdy strona jest zażądał przechodzi przez kilka kroków, jako jego renderowania. W jednym z następujących czynności kontrolki ObjectDataSource kontrolki Sprawdź, czy ich `SelectParameters` wartości zostały zmienione. Jeśli tak, kontrolki sieci Web dane powiązane z kontrolki ObjectDataSource wie, musi odświeżyć jego wyświetlania. Na przykład, gdy wybrano nową kategorię, `ProductsByCategoryDataSource` ObjectDataSource wykrywa, czy zmieniono jej wartości parametrów i `ProductsByCategory` DropDownList rebinds, wprowadzenie produktów dla wybranej kategorii.

Problem, który pojawia się w tej sytuacji jest występowanie punktu w cyklu życia strony, który ObjectDataSources sprawdzać zmiany parametrów *przed* ponowne wiązanie skojarzonych danych kontrolki sieci Web. W związku z tym wybierając nową kategorię `ProductsByCategoryDataSource` ObjectDataSource wykryje zmianę w wartość jego parametru. Używane przez kontrolki ObjectDataSource `ProductDetails` DetailsView, jednak nie należy pamiętać, wszelkie zmiany ponieważ `ProductsByCategory` DropDownList jeszcze nie być odbitych. Później w cyklu życia `ProductsByCategory` DropDownList rebinds do jego kontrolki ObjectDataSource Przechwytywanie produktów dla nowo wybranej kategorii. Gdy `ProductsByCategory` DropDownList jego wartość została zmieniona, `ProductDetails` DetailsView ObjectDataSource już wykonała wyboru wartość jego parametru; w związku z tym, DetailsView wyświetla jego poprzednie wyniki. Ta interakcja jest przedstawiona na rysunku 20.


[![Wartość kontrolki ProductsByCategory DropDownList zmienia się po ObjectDataSource ProductDetails DetailsView sprawdza obecność zmian](master-detail-filtering-with-two-dropdownlists-vb/_static/image59.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image58.png)

**Rysunek 20**: `ProductsByCategory` DropDownList wartość zmiany po `ProductDetails` DetailsView ObjectDataSource sprawdza obecność zmian ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-two-dropdownlists-vb/_static/image60.png))


Aby temu zapobiec potrzebujemy, aby jawnie ponownie powiązać `ProductDetails` DetailsView po `ProductsByCategory` DropDownList została powiązana. Możemy to zrobić, wywołując `ProductDetails` DetailsView `DataBind()` metody podczas `ProductsByCategory` firmy DropDownList `DataBound` generowane zdarzenie. Dodaj następujący kod procedury obsługi zdarzeń, aby `MasterDetailsDetails.aspx` strony osobna klasa kodu (odnoszą się do "[programowe Ustawianie wartości parametrów elementu ObjectDataSource](../basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-cs.md)" do dyskusji na temat dodawania programu obsługi zdarzeń):


[!code-vb[Main](master-detail-filtering-with-two-dropdownlists-vb/samples/sample2.vb)]

Po tym jawnym wywołaniem `ProductDetails` DetailsView `DataBind()` metoda został dodany, samouczek działa zgodnie z oczekiwaniami. Rysunek 21 najważniejsze funkcje, jak zmieniło się to zostaną usunięte wcześniej problemu.


[![ProductDetails DetailsView jest generowane zdarzenie z danymi jawnie odświeżone po ProductsByCategory DropDownList firmy](master-detail-filtering-with-two-dropdownlists-vb/_static/image62.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image61.png)

**Rysunek 21**: `ProductDetails` DetailsView jest jawnie odświeżone po `ProductsByCategory` firmy DropDownList `DataBound` generowane zdarzenie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-two-dropdownlists-vb/_static/image63.png))


## <a name="summary"></a>Podsumowanie

Metody DropDownList służy jako element interfejsu użytkownika idealne rozwiązanie dla rekordu głównego/szczegółów raportów w przypadku, gdy istnieje relacja jeden do wielu między rekordami głównego i szczegółów. Jak filtrować wyświetlane przez wybraną kategorię produkty za pomocą pojedynczej metody DropDownList widzieliśmy w poprzednim samouczku. W tym samouczku będziemy zastępowane GridView produktów przy użyciu kontrolki DropDownList, a używane DetailsView, aby wyświetlić szczegóły dotyczące wybranego produktu. Kwestie omówione w tym samouczku można z łatwością rozszerzyć do modeli danych obejmujących wiele relacji jeden do wielu, takich jak klienci i zamówienia, kolejność elementów. Ogólnie rzecz biorąc można dodać kontrolki DropDownList dla każdej jednostki "jeden" w relacji jeden do wielu.

Wszystkiego najlepszego programowania!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Weryfikacja potencjalnych klientów w ramach tego samouczka został Hilton Giesenow. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](master-detail-filtering-with-a-dropdownlist-vb.md)
> [dalej](master-detail-filtering-across-two-pages-vb.md)

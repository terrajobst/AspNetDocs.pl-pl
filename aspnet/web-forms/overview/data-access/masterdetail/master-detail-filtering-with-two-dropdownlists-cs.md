---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-with-two-dropdownlists-cs
title: Filtrowanie wzorców/szczegółów przy użyciu dwóch kontrolek DropDownListC#() | Microsoft Docs
author: rick-anderson
description: Ten samouczek rozszerza relację wzorzec/szczegóły w celu dodania trzeciej warstwy, przy użyciu dwóch kontrolek DropDownList do wybrania żądanych elementów nadrzędnych i nadrzędnego nag...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: ac4b0d77-4816-4ded-afd0-88dab667aedd
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-with-two-dropdownlists-cs
msc.type: authoredcontent
ms.openlocfilehash: e24b7f91d34fbce1676f7f28ebb7d23903157f7f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78528491"
---
# <a name="masterdetail-filtering-with-two-dropdownlists-c"></a>Filtrowanie rekordu głównego/szczegółów przy użyciu dwóch kontrolek DropDownList (C#)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](https://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_8_CS.exe) lub [Pobierz plik PDF](master-detail-filtering-with-two-dropdownlists-cs/_static/datatutorial08cs1.pdf)

> Ten samouczek rozszerza relację główną/szczegółową, aby dodać trzecią warstwę, przy użyciu dwóch kontrolek DropDownList do wybierania żądanych rekordów nadrzędnych i nadrzędnego.

## <a name="introduction"></a>Wprowadzenie

W [poprzednim samouczku](master-detail-filtering-with-a-dropdownlist-cs.md) sprawdzono sposób wyświetlania prostego raportu wzorzec/szczegóły przy użyciu jednego DropDownList wypełnionego kategoriami i GridView pokazujących te produkty należące do wybranej kategorii. Ten wzorzec raportu działa dobrze, gdy są wyświetlane rekordy, które mają relację "jeden do wielu" i można ją łatwo rozszerzyć na potrzeby scenariuszy obejmujących wiele relacji jeden-do-wielu. Na przykład system wprowadzania zamówień ma tabele, które odpowiadają klientom, zamówieniom i wierszom zamówienia. Dany klient może mieć wiele zamówień z każdą kolejnością składającą się z wielu elementów. Takie dane mogą być prezentowane użytkownikowi przy użyciu dwóch kontrolek DropDownList i GridView. Pierwszy DropDownList będzie miał element listy dla każdego klienta w bazie danych, a druga zawartość jest zamówieniami umieszczonymi przez wybranego klienta. W widoku GridView będą wystawiane elementy wiersza z wybranej kolejności.

Baza danych Northwind zawiera dane kanoniczne dotyczące klienta/zamówienia/zamówienia w tabelach `Customers`, `Orders`i `Order Details`, ale te tabele nie są przechwytywane w naszej architekturze. Mimo to nadal możemy zilustrować użycie dwóch zależnych kontrolek DropDownList. Pierwszy DropDownList wyświetli listę kategorii, a drugi produkty należące do wybranej kategorii. Następnie w widoku DetailsView zostaną wystawione szczegóły wybranego produktu.

## <a name="step-1-creating-and-populating-the-categories-dropdownlist"></a>Krok 1. Tworzenie i wypełnianie kategorii DropDownList

Pierwszym celem jest dodanie DropDownList, który zawiera listę kategorii. Te kroki zostały szczegółowo opisane w poprzednim samouczku, ale zostały podsumowane w tym miejscu pod kątem kompletności.

Otwórz stronę `MasterDetailsDetails.aspx` w folderze `Filtering`, Dodaj DropDownList do strony, ustaw jej Właściwość `ID` na `Categories`, a następnie kliknij link Konfiguruj źródło danych w swoim tagu inteligentnym. W Kreatorze konfiguracji źródła danych wybierz, aby dodać nowe źródło danych.

[![dodać nowego źródła danych dla DropDownList](master-detail-filtering-with-two-dropdownlists-cs/_static/image2.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image1.png)

**Rysunek 1**. Dodawanie nowego źródła danych dla DropDownList ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](master-detail-filtering-with-two-dropdownlists-cs/_static/image3.png))

Nowe źródło danych powinno, naturalnie, być obiektem ObjectDataSource. Nazwij ten nowy element ObjectDataSource `CategoriesDataSource` i Wywołaj metodę `GetCategories()` obiektu `CategoriesBLL`.

[![wybrać użycie klasy CategoriesBLL](master-detail-filtering-with-two-dropdownlists-cs/_static/image5.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image4.png)

**Rysunek 2**. wybór użycia klasy `CategoriesBLL` (kliknij,[Aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-two-dropdownlists-cs/_static/image6.png))

[![skonfigurować element ObjectDataSource do korzystania z metody GetCategories ()](master-detail-filtering-with-two-dropdownlists-cs/_static/image8.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image7.png)

**Rysunek 3**. Konfigurowanie elementu ObjectDataSource do używania metody `GetCategories()` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](master-detail-filtering-with-two-dropdownlists-cs/_static/image9.png))

Po skonfigurowaniu elementu ObjectDataSource nadal musimy określić, które pole źródła danych ma być wyświetlane w `Categories` DropDownList i który ma być skonfigurowany jako wartość dla elementu listy. Ustaw `CategoryName` pole jako wartość wyświetlaną i `CategoryID` jako wartości dla każdego elementu listy.

[![wyświetlać DropDownList pole CategoryName i używać IDkategorii jako wartości](master-detail-filtering-with-two-dropdownlists-cs/_static/image11.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image10.png)

**Rysunek 4**. DropDownList wyświetlić pola `CategoryName` i użyj `CategoryID` jako wartości ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-two-dropdownlists-cs/_static/image12.png))

W tym momencie mamy kontrolkę DropDownList (`Categories`), która jest wypełniana rekordami z tabeli `Categories`. Gdy użytkownik wybierze nową kategorię z DropDownList, będziemy mieć ogłaszanie zwrotne w celu odświeżenia DropDownList produktu, które zostaną utworzone w kroku 2. W związku z tym zaznacz opcję Włącz autoogłaszanie w tagu inteligentnym `categories` DropDownList.

[![włączyć funkcję autoogłaszania zwrotnego dla kategorii DropDownList](master-detail-filtering-with-two-dropdownlists-cs/_static/image14.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image13.png)

**Rysunek 5**. Włączanie autoogłaszania zwrotnego dla `Categories` DropDownList ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-two-dropdownlists-cs/_static/image15.png))

## <a name="step-2-displaying-the-selected-categorys-products-in-a-second-dropdownlist"></a>Krok 2. Wyświetlanie produktów wybranej kategorii w drugim DropDownList

Po zakończeniu `Categories` DropDownList, następnym krokiem jest wyświetlenie DropDownList produktów należących do wybranej kategorii. Aby to osiągnąć, Dodaj kolejną DropDownList do strony o nazwie `ProductsByCategory`. Podobnie jak w przypadku `Categories` DropDownList, Utwórz nowy element ObjectDataSource dla `ProductsByCategory` DropDownList o nazwie `ProductsByCategoryDataSource`.

[![dodać nowego źródła danych dla ProductsByCategory DropDownList](master-detail-filtering-with-two-dropdownlists-cs/_static/image17.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image16.png)

**Ilustracja 6**. Dodawanie nowego źródła danych dla `ProductsByCategory` DropDownList ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-two-dropdownlists-cs/_static/image18.png))

[![utworzyć nowego elementu ObjectDataSource o nazwie ProductsByCategoryDataSource](master-detail-filtering-with-two-dropdownlists-cs/_static/image20.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image19.png)

**Rysunek 7**. Tworzenie nowego elementu ObjectDataSource o nazwie `ProductsByCategoryDataSource` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](master-detail-filtering-with-two-dropdownlists-cs/_static/image21.png))

Ponieważ `ProductsByCategory` DropDownList musi wyświetlić tylko te produkty należące do wybranej kategorii, element ObjectDataSource wywołuje metodę `GetProductsByCategoryID(categoryID)` z obiektu `ProductsBLL`.

[![wybrać użycie klasy ProductsBLL](master-detail-filtering-with-two-dropdownlists-cs/_static/image23.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image22.png)

**Ilustracja 8**. wybór użycia klasy `ProductsBLL` (kliknij,[Aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-two-dropdownlists-cs/_static/image24.png))

[![skonfigurować element ObjectDataSource do używania metody GetProductsByCategoryID (IDKategorii)](master-detail-filtering-with-two-dropdownlists-cs/_static/image26.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image25.png)

**Ilustracja 9**. Konfigurowanie elementu ObjectDataSource do używania metody `GetProductsByCategoryID(categoryID)` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-two-dropdownlists-cs/_static/image27.png))

W ostatnim kroku kreatora musimy określić wartość parametru *`categoryID`* . Przypisz ten parametr do wybranego elementu z `Categories` DropDownList.

[![ściągnąć wartość parametru IDKategorii z kategorii DropDownList](master-detail-filtering-with-two-dropdownlists-cs/_static/image29.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image28.png)

**Ilustracja 10**. ściąganie wartości parametru *`categoryID`* z `Categories` DropDownList ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-two-dropdownlists-cs/_static/image30.png))

Po skonfigurowaniu elementu ObjectDataSource wszystkie pozostałe, które pozostają, określają, które pola źródła danych są używane do wyświetlania i wartości elementów DropDownList. Wyświetl `ProductName` pola i użyj pola `ProductID` jako wartości.

[![określić pola źródła danych używane dla właściwości "text i value" elementu "DropDownList".](master-detail-filtering-with-two-dropdownlists-cs/_static/image32.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image31.png)

**Ilustracja 11**. Określ pola źródła danych używane dla `Text` DropDownList `ListItem` s i właściwości `Value` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](master-detail-filtering-with-two-dropdownlists-cs/_static/image33.png))

Za pomocą elementów ObjectDataSource i `ProductsByCategory` skonfigurowanych na naszej stronie zostanie wyświetlonych dwa kontrolek DropDownList: pierwsza spowoduje wyświetlenie listy wszystkich kategorii, a druga spowoduje wyświetlenie listy tych produktów należących do wybranej kategorii. Gdy użytkownik wybierze nową kategorię z pierwszego DropDownListu, odświeżenie zostanie nastąpi, a druga DropDownList zostanie odwiązana, pokazując te produkty należące do nowo wybranej kategorii. Rysunki 12 i 13 pokazują `MasterDetailsDetails.aspx` w akcji podczas przeglądania za pomocą przeglądarki.

[![podczas pierwszego odwiedzania strony wybrano kategorię napoje](master-detail-filtering-with-two-dropdownlists-cs/_static/image35.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image34.png)

**Rysunek 12**. podczas pierwszego odwiedzania strony jest wybierana Kategoria napoje ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](master-detail-filtering-with-two-dropdownlists-cs/_static/image36.png))

[![wybranie innej kategorii spowoduje wyświetlenie produktów nowej kategorii](master-detail-filtering-with-two-dropdownlists-cs/_static/image38.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image37.png)

**Ilustracja 13**. wybranie innej kategorii powoduje wyświetlenie produktów nowej kategorii ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-two-dropdownlists-cs/_static/image39.png))

Obecnie `productsByCategory` DropDownList, gdy zmiana *nie powoduje* odświeżenia. Będziemy jednak chcieć ogłaszać zwrotne po dodaniu widoku DetailsView, aby wyświetlić szczegółowe informacje o wybranym produkcie (krok 3). W związku z tym zaznacz pole wyboru Włącz autoogłaszanie w tagu inteligentnym `productsByCategory` DropDownList.

[![włączyć funkcję autoogłaszania zwrotnego dla productsByCategory DropDownList](master-detail-filtering-with-two-dropdownlists-cs/_static/image41.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image40.png)

**Ilustracja 14**. Włączanie funkcji autoogłaszania dla `productsByCategory` DropDownList ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](master-detail-filtering-with-two-dropdownlists-cs/_static/image42.png))

## <a name="step-3-using-a-detailsview-to-display-details-for-the-selected-product"></a>Krok 3. Wyświetlanie szczegółowych informacji o wybranym produkcie przy użyciu widoku DetailsView

Ostatnim krokiem jest wyświetlenie szczegółowych informacji o wybranym produkcie w widoku DetailsView. Aby to osiągnąć, Dodaj do strony opcję DetailsView, ustaw jej Właściwość `ID` na `ProductDetails`i Utwórz dla niej nowy element ObjectDataSource. Skonfiguruj ten element ObjectDataSource do ściągania danych z metody `GetProductByProductID(productID)` klasy `ProductsBLL` przy użyciu wybranej wartości `ProductsByCategory` DropDownList dla wartości parametru *`productID`* .

[![wybrać użycie klasy ProductsBLL](master-detail-filtering-with-two-dropdownlists-cs/_static/image44.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image43.png)

**Ilustracja 15**. Wybierz, aby użyć klasy `ProductsBLL` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](master-detail-filtering-with-two-dropdownlists-cs/_static/image45.png))

[![skonfigurować element ObjectDataSource do korzystania z metody GetProductByProductID (productID)](master-detail-filtering-with-two-dropdownlists-cs/_static/image47.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image46.png)

**Ilustracja 16**. Konfigurowanie elementu ObjectDataSource do używania metody `GetProductByProductID(productID)` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-two-dropdownlists-cs/_static/image48.png))

[![ściągnąć wartość parametru productID z ProductsByCategory DropDownList](master-detail-filtering-with-two-dropdownlists-cs/_static/image50.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image49.png)

**Ilustracja 17**. ściąganie wartości parametru *`productID`* z `ProductsByCategory` DropDownList ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-two-dropdownlists-cs/_static/image51.png))

Możesz wybrać opcję wyświetlania dowolnego z dostępnych pól w widoku DetailsView. Wybrano, aby usunąć pola `ProductID`, `SupplierID`i `CategoryID` oraz zmienić ich kolejność i sformatować pozostałe pola. Dodatkowo wyczyściłem `Height` i `Width` właściwości, co pozwala na rozwijanie widoku DetailsView do szerokości wymaganej do najlepszego wyświetlenia danych, a nie ograniczenia do określonego rozmiaru. Pełny znacznik jest widoczny poniżej:

[!code-aspx[Main](master-detail-filtering-with-two-dropdownlists-cs/samples/sample1.aspx)]

Zapoznaj się z chwilą, aby wypróbować stronę `MasterDetailsDetails.aspx` w przeglądarce. Na pierwszy rzut oka może się wydawać, że wszystko działa zgodnie z potrzebami, ale wystąpił delikatny problem. Po wybraniu nowej kategorii `ProductsByCategory` DropDownList zostanie zaktualizowana w celu uwzględnienia tych produktów dla wybranej kategorii, ale `ProductDetails` DetailsView nadal będzie pokazywała poprzednie informacje o produkcie. W przypadku wybrania innego produktu dla wybranej kategorii jest aktualizowana wartość DetailsView. Ponadto, jeśli testujesz dokładnie tyle, że w przypadku ciągłego wybierania nowych kategorii (takich jak wybór napojów z `Categories` DropDownList, a następnie Przypraws, a następnie wyrobów cukierniczych) każdy inny wybór kategorii spowoduje odświeżenie `ProductDetails` DetailsView.

Aby pomóc w skonkretyzować tego problemu, przyjrzyjmy się konkretnemu przykładowi. Po pierwszej wizycie na stronie wybrano kategorię napoje i powiązane produkty są ładowane do `ProductsByCategory` DropDownList. Chai to wybrany produkt, a jego szczegóły są wyświetlane w `ProductDetails` DetailsView, jak pokazano na rysunku 18.

[![szczegóły wybranego produktu są wyświetlane w widoku DetailsView](master-detail-filtering-with-two-dropdownlists-cs/_static/image53.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image52.png)

**Ilustracja 18**. szczegóły wybranego produktu są wyświetlane w widoku DetailsView ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](master-detail-filtering-with-two-dropdownlists-cs/_static/image54.png))

Jeśli zmienisz wybór kategorii z napojów na przyprawy, nastąpi odświeżenie, a `ProductsByCategory` DropDownList zostanie odpowiednio zaktualizowany, ale w widoku DetailsView nadal będą wyświetlane szczegółowe informacje o Chai.

[![informacje o poprzednio wybranym produkcie są nadal wyświetlane](master-detail-filtering-with-two-dropdownlists-cs/_static/image56.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image55.png)

**Ilustracja 19**. szczegóły poprzednio wybranego produktu są nadal wyświetlane (kliknij,[Aby wyświetlić obraz o pełnym rozmiarze](master-detail-filtering-with-two-dropdownlists-cs/_static/image57.png))

Wybranie nowego produktu z listy spowoduje odświeżenie widoku DetailsView zgodnie z oczekiwaniami. W przypadku wybrania nowej kategorii po zmianie produktu widok DetailsView ponownie nie zostanie odświeżony. Jednak w przypadku wybrania nowego produktu, który został wybrany nowej kategorii, można odświeżyć widok DetailsView. Co na świecie odbywa się tutaj?

Problem dotyczy cyklu życia strony. Za każdym razem, gdy zażądano strony, przejdzie ona przez kilka kroków w miarę renderowania. W jednym z tych kroków kontrolki elementu ObjectDataSource sprawdzają, czy dowolna z wartości `SelectParameters` uległy zmianie. Jeśli tak, formant sieci Web danych powiązany z elementem ObjectDataSource wie, że musi odświeżyć jego ekran. Na przykład po wybraniu nowej kategorii element ObjectDataSource `ProductsByCategoryDataSource` wykrył, że jego wartości parametrów uległy zmianie, a `ProductsByCategory` DropDownList rebind same, pobierając produkty dla wybranej kategorii.

Problem występujący w tej sytuacji polega na tym, że punkt w cyklu życia strony, który sprawdza zmiany parametrów, występuje *przed ponownym* wiązaniem skojarzonych kontrolek sieci Web danych. W związku z tym podczas wybierania nowej kategorii element ObjectDataSource `ProductsByCategoryDataSource` wykrywa zmianę wartości parametru. Element ObjectDataSource używany przez `ProductDetails` DetailsView nie zanotuje żadnych zmian, ponieważ `ProductsByCategory` DropDownList nie zostały jeszcze oddzielone. W dalszej części cyklu życia `ProductsByCategory` DropDownList ponownie powiązania z jego elementem ObjectDataSource, co umożliwia przechwyconie produktów dla nowo wybranej kategorii. Gdy wartość `ProductsByCategory` DropDownList została zmieniona, element ObjectDataSource `ProductDetails` DetailsView już zrobił sprawdzanie wartości parametrów; w związku z tym, DetailsView wyświetla poprzednie wyniki. Ta interakcja jest przedstawiana na rysunku 20.

[![zmiany wartości ProductsByCategory DropDownList po przeprowadzeniu testów elementu ObjectDataSource ProductDetails DetailsView dla zmian](master-detail-filtering-with-two-dropdownlists-cs/_static/image59.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image58.png)

**Ilustracja 20**: wartość `ProductsByCategory` DropDownList ulega zmianie po `ProductDetails` sprawdzenia elementu ObjectDataSource w widoku DetailsView dla zmian ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-two-dropdownlists-cs/_static/image60.png))

Aby rozwiązać ten konieczność, należy jawnie ponownie powiązać `ProductDetails` DetailsView po powiązaniu `ProductsByCategory` DropDownList. Możemy to osiągnąć przez wywołanie metody `DataBind()` `ProductDetails` DetailsView, gdy zostanie wyzwolone zdarzenie `DataBound` `ProductsByCategory`. Dodaj następujący kod obsługi zdarzeń do klasy związanej z kodem `MasterDetailsDetails.aspx` strony (zobacz "[programowo Ustawianie wartości parametrów elementu ObjectDataSource](../basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-cs.md)" w przypadku dyskusji na temat dodawania obsługi zdarzeń):

[!code-csharp[Main](master-detail-filtering-with-two-dropdownlists-cs/samples/sample2.cs)]

Po dodaniu tego jawnego wywołania metody `DataBind()` `ProductDetails` DetailsView, samouczek działa zgodnie z oczekiwaniami. Rysunek 21 przedstawia, w jaki sposób zmiana ta została zmieniona z powodu wcześniejszego problemu.

[![ProductDetails DetailsView jest jawnie odświeżany, gdy wyzwalane jest zdarzenie powiązane z danymi DropDownList ProductsByCategory](master-detail-filtering-with-two-dropdownlists-cs/_static/image62.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image61.png)

**Rysunek 21**. `ProductDetails` DetailsView jest jawnie odświeżany po wypełnieniu zdarzenia `DataBound` `ProductsByCategory` DropDownList ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-two-dropdownlists-cs/_static/image63.png))

## <a name="summary"></a>Podsumowanie

DropDownList służy jako idealny element interfejsu użytkownika dla raportów wzorzec/szczegóły, gdy istnieje relacja jeden do wielu między rekordem głównym i szczegółowymi. W poprzednim samouczku przedstawiono sposób użycia jednego DropDownList do filtrowania produktów wyświetlanych przez wybraną kategorię. W tym samouczku zamieniłem GridView produktów na DropDownList i użyto widoku DetailsView, aby wyświetlić szczegóły wybranego produktu. Koncepcje omówione w tym samouczku można łatwo rozszerzyć do modeli danych obejmujących wiele relacji jeden-do-wielu, takich jak klienci, zamówienia i elementy zamówienia. Ogólnie rzecz biorąc, można zawsze dodać DropDownList dla każdej jednostki "jednego" w relacji jeden-do-wielu.

Szczęśliwe programowanie!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Recenzent potencjalnych klientów dla tego samouczka został Hilton Giesenow. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](master-detail-filtering-with-a-dropdownlist-cs.md)
> [dalej](master-detail-filtering-across-two-pages-cs.md)

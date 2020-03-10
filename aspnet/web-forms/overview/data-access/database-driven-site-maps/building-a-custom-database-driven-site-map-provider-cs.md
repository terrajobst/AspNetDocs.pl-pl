---
uid: web-forms/overview/data-access/database-driven-site-maps/building-a-custom-database-driven-site-map-provider-cs
title: Tworzenie niestandardowego dostawcy mapy witryny opartego na bazie danychC#() | Microsoft Docs
author: rick-anderson
description: Domyślny dostawca mapy witryny w programie ASP.NET 2,0 pobiera dane z statycznego pliku XML. Chociaż dostawca oparty na języku XML jest odpowiedni dla wielu małych i średnich SIZ...
ms.author: riande
ms.date: 06/26/2007
ms.assetid: 04b7591d-106f-4f05-87e9-d416cb65a8a6
msc.legacyurl: /web-forms/overview/data-access/database-driven-site-maps/building-a-custom-database-driven-site-map-provider-cs
msc.type: authoredcontent
ms.openlocfilehash: a3e27b37703b12c9796e8516f0d805aef1fdf8d8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78611987"
---
# <a name="building-a-custom-database-driven-site-map-provider-c"></a>Tworzenie niestandardowego dostawcy map witryn opartych na bazie danych (C#)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz kod](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_62_CS.zip) lub [Pobierz plik PDF](building-a-custom-database-driven-site-map-provider-cs/_static/datatutorial62cs1.pdf)

> Domyślny dostawca mapy witryny w programie ASP.NET 2,0 pobiera dane z statycznego pliku XML. Chociaż dostawca oparty na języku XML jest odpowiedni dla wielu małych i średnich witryn sieci Web, większe aplikacje sieci Web wymagają szerszej mapy witryn. W tym samouczku utworzymy niestandardowego dostawcę mapy witryny, który pobiera dane z warstwy logiki biznesowej, która z kolei pobiera dane z bazy danych.

## <a name="introduction"></a>Wprowadzenie

ASP.NET 2,0 s funkcja mapy witryny umożliwia deweloperom stron Definiowanie mapy witryny aplikacji sieci Web w pewnym trwałym nośniku, na przykład w pliku XML. Po zdefiniowaniu dane mapy lokacji można uzyskać programowo przy użyciu [klasy`SiteMap`](https://msdn.microsoft.com/library/system.web.sitemap.aspx) w [przestrzeni nazw`System.Web`](https://msdn.microsoft.com/library/system.web.aspx) lub za pomocą różnych kontrolek sieci Web nawigacji, takich jak ścieżki mapy witryny, menu i drzewa TreeView. System mapy lokacji korzysta z [modelu dostawcy](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx) , aby można było utworzyć różne implementacje serializacji mapy witryny i podłączyć je do aplikacji sieci Web. Domyślny dostawca mapy witryny dostarczany z ASP.NET 2,0 utrzymuje strukturę mapy witryny w pliku XML. Z powrotem na [stronach wzorcowych i](../introduction/master-pages-and-site-navigation-cs.md) w samouczku nawigowania po witrynie został utworzony plik o nazwie `Web.sitemap`, który zawierał tę strukturę i który ZAKTUALIZOWAŁ kod XML za pomocą każdej nowej sekcji samouczka.

Domyślny dostawca mapy witryny oparty na języku XML działa prawidłowo, jeśli struktura mapy witryny jest dość statyczna, na przykład w przypadku tych samouczków. Jednak w wielu scenariuszach jest wymagana bardziej dynamiczna mapa witryny. Rozważmy mapę witryny pokazaną na rysunku 1, gdzie każda kategoria i produkt są wyświetlane jako sekcje w strukturze witryny sieci Web. W przypadku tej mapy witryny odwiedzanie strony sieci Web odpowiadającej węzłowi głównemu może spowodować wyświetlenie wszystkich kategorii, a podczas odwiedzania określonej strony sieci Web w kategorii należy wyświetlić listę produktów kategorii i wyświetlić określoną stronę produktu w sieci Web.

[![kategorie i produkty korzeń strukturę mapy witryny](building-a-custom-database-driven-site-map-provider-cs/_static/image1.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image1.png)

**Rysunek 1**. kategorie i produkty korzeń strukturę mapy witryny ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](building-a-custom-database-driven-site-map-provider-cs/_static/image2.png))

Chociaż ta struktura kategorii i produktu może być zakodowana w pliku `Web.sitemap`, należy ją zaktualizować za każdym razem, gdy Kategoria lub produkt został dodany, usunięty lub zmieniono jego nazwę. W związku z tym, konserwacja mapy witryny będzie znacznie uproszczona, jeśli struktura została pobrana z bazy danych lub, najlepiej, z warstwy logiki biznesowej architektury aplikacji. Dzięki temu, jak produkty i kategorie zostały dodane, zmieniono nazwę lub usunięto, mapa witryny zostanie automatycznie zaktualizowana w celu odzwierciedlenia tych zmian.

Ponieważ ASP.NET 2,0 s Serializacja mapy witryny jest zbudowana korzystającego modelu dostawcy, możemy utworzyć własnego niestandardowego dostawcę mapy witryny, który będzie zbierał dane z alternatywnego magazynu danych, takiego jak baza danych lub architektura. W tym samouczku utworzysz dostawcę niestandardowego, który pobiera dane z LOGIKI biznesowej. Zacznij korzystać z aplikacji.

> [!NOTE]
> Niestandardowy dostawca mapy witryny utworzony w tym samouczku jest ściśle połączony z architekturą i modelem danych aplikacji. Marcin Prosise s [przechowujący mapy witryn w SQL Server](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/) i [dostawca mapy witryny SQL zaczekał na](https://msdn.microsoft.com/msdnmag/issues/06/02/wickedcode/default.aspx) artykuły badanie uogólnionego podejścia do przechowywania danych mapy lokacji w SQL Server.

## <a name="step-1-creating-the-custom-site-map-provider-web-pages"></a>Krok 1. Tworzenie niestandardowych stron sieci Web dostawcy mapy witryny

Przed rozpoczęciem tworzenia niestandardowego dostawcy mapy witryny należy najpierw dodać strony ASP.NET, które będą potrzebne w tym samouczku. Zacznij od dodania nowego folderu o nazwie `SiteMapProvider`. Następnie Dodaj następujące strony ASP.NET do tego folderu, aby upewnić się, że każda strona jest skojarzona z `Site.master` stroną wzorcową:

- `Default.aspx`
- `ProductsByCategory.aspx`
- `ProductDetails.aspx`

Dodaj także podfolder `CustomProviders` do folderu `App_Code`.

![Dodaj strony ASP.NET dla samouczków związanych z dostawcą mapy witryny](building-a-custom-database-driven-site-map-provider-cs/_static/image2.gif)

**Rysunek 2**. dodawanie stron ASP.NET dla samouczków związanych z dostawcą mapy witryny

Ponieważ dla tej sekcji istnieje tylko jeden samouczek, nie potrzebujemy `Default.aspx`, aby wyświetlić listę samouczków sekcji. Zamiast tego, `Default.aspx` będą wyświetlać kategorie w kontrolce GridView. Zajmiemy się tym w kroku 2.

Następnie zaktualizuj `Web.sitemap`, aby dołączyć odwołanie do strony `Default.aspx`. Należy dodać następujące znaczniki po `<siteMapNode>`buforowania:

[!code-xml[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample1.xml)]

Po aktualizacji `Web.sitemap`zapoznaj się z witryną internetową samouczków za pomocą przeglądarki. Menu po lewej stronie zawiera teraz element samouczka dla jedynego dostawcy mapy witryny.

![Mapa witryny zawiera teraz wpis do samouczka dotyczącego dostawcy mapy witryny](building-a-custom-database-driven-site-map-provider-cs/_static/image3.gif)

**Rysunek 3**. Mapa witryny zawiera teraz wpis do samouczka dotyczącego dostawcy mapy witryny

Ten samouczek przedstawia tworzenie niestandardowego dostawcy mapy witryny i Konfigurowanie aplikacji sieci Web do korzystania z tego dostawcy. W szczególności utworzymy dostawcę, który zwróci mapę witryny, która zawiera węzeł główny wraz z węzłem dla każdej kategorii i produktu, jak przedstawiono na rysunku 1. Ogólnie rzecz biorąc, każdy węzeł w mapie witryny może określać adres URL. W przypadku naszej mapy witryny główny adres URL węzła zostanie `~/SiteMapProvider/Default.aspx`, co spowoduje wyświetlenie listy wszystkich kategorii w bazie danych. Każdy węzeł kategorii na mapie witryny będzie miał adres URL wskazujący na `~/SiteMapProvider/ProductsByCategory.aspx?CategoryID=categoryID`, który będzie zawierać listę wszystkich produktów w określonym *IDkategorii*. Na koniec każdy węzeł mapy witryny produktu wskaże `~/SiteMapProvider/ProductDetails.aspx?ProductID=productID`, co spowoduje wyświetlenie szczegółowych informacji o produkcie.

Aby rozpocząć, należy utworzyć strony `Default.aspx`, `ProductsByCategory.aspx`i `ProductDetails.aspx`. Te strony są wykonywane odpowiednio w krokach 2, 3 i 4. Ponieważ nacisk tego samouczka znajduje się w dostawcach mapy witryny, a ponieważ przeszłe samouczki pomogły utworzyć różne raporty wzorzec/szczegóły wielostronicowego, będziemy pospiesz przez kroki od 2 do 4. Jeśli potrzebujesz odświeżacza przy tworzeniu raportów master/detail obejmujących wiele stron, zapoznaj się z artykułem dotyczącym [filtrowania wzorca/szczegółów na dwóch stronach](../masterdetail/master-detail-filtering-across-two-pages-cs.md) .

## <a name="step-2-displaying-a-list-of-categories"></a>Krok 2. Wyświetlanie listy kategorii

Otwórz stronę `Default.aspx` w folderze `SiteMapProvider` i przeciągnij widok GridView z przybornika do projektanta, ustawiając jego `ID` na `Categories`. Z taga inteligentnego w widoku GridView powiąż go z nowym elementem ObjectDataSource o nazwie `CategoriesDataSource` i skonfiguruj go tak, aby pobierał dane przy użyciu metody `CategoriesBLL` klasy s `GetCategories`. Ponieważ w tym widoku GridView są wyświetlane kategorie i nie są dostępne funkcje modyfikacji danych, Ustaw listę rozwijaną na kartach UPDATE, INSERT i DELETE na wartość (brak).

[![skonfigurować element ObjectDataSource do zwracania kategorii za pomocą metody GetCategories](building-a-custom-database-driven-site-map-provider-cs/_static/image4.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image3.png)

**Ilustracja 4**. Konfigurowanie elementu ObjectDataSource do zwracania kategorii za pomocą metody `GetCategories` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](building-a-custom-database-driven-site-map-provider-cs/_static/image4.png))

[![ustawić listy rozwijane na kartach Aktualizuj, Wstaw i Usuń do (brak).](building-a-custom-database-driven-site-map-provider-cs/_static/image5.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image5.png)

**Rysunek 5**. Ustawianie list rozwijanych w kartach Aktualizuj, Wstaw i Usuń do (brak) ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](building-a-custom-database-driven-site-map-provider-cs/_static/image6.png))

Po zakończeniu działania Kreatora konfiguracji źródła danych program Visual Studio doda BoundField do `CategoryID`, `CategoryName`, `Description`, `NumberOfProducts`i `BrochurePath`. Edytuj widok GridView, aby zawierał tylko `CategoryName` i `Description` BoundFields i zaktualizować Właściwość `CategoryName` BoundField s `HeaderText` do kategorii.

Następnie Dodaj element HyperLinkField i umieść go tak, aby pozostały do lewej strony. Ustaw właściwość `DataNavigateUrlFields` na `CategoryID` i Właściwość `DataNavigateUrlFormatString` na `~/SiteMapProvider/ProductsByCategory.aspx?CategoryID={0}`. Ustaw właściwość `Text`, aby wyświetlić produkty.

![Dodaj element HyperLinkField do kategorii GridView](building-a-custom-database-driven-site-map-provider-cs/_static/image6.gif)

**Ilustracja 6**. Dodawanie HyperLinkField do `Categories` GridView

Po utworzeniu elementu ObjectDataSource i dostosowaniu pól GridView, dwa formanty deklaratywne znaczniki będą wyglądać następująco:

[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample2.aspx)]

Rysunek 7 przedstawia `Default.aspx`, gdy jest wyświetlany za pomocą przeglądarki. Kliknięcie linku Pokaż produkty kategorii spowoduje przejście do `ProductsByCategory.aspx?CategoryID=categoryID`, który zostanie skompilowany w kroku 3.

[![każda kategoria jest wyświetlana razem z linkiem Wyświetl produkty](building-a-custom-database-driven-site-map-provider-cs/_static/image7.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image7.png)

**Rysunek 7**. Każda kategoria jest wyświetlana wraz z linkiem Wyświetl produkty ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](building-a-custom-database-driven-site-map-provider-cs/_static/image8.png))

## <a name="step-3-listing-the-selected-category-s-products"></a>Krok 3. Wyświetlanie listy wybranych produktów kategorii s

Otwórz stronę `ProductsByCategory.aspx` i Dodaj widok GridView, nadając jej nazwę `ProductsByCategory`. Ze znacznika inteligentnego Powiąż widok GridView z nowym elementem ObjectDataSource o nazwie `ProductsByCategoryDataSource`. Skonfiguruj element ObjectDataSource do używania metody `GetProductsByCategoryID(categoryID)` `ProductsBLL` klasy s i Ustaw listę rozwijaną na wartość (brak) na kartach Aktualizuj, Wstaw i Usuń.

[![użyć metody ProductsBLL Class s GetProductsByCategoryID (IDKategorii)](building-a-custom-database-driven-site-map-provider-cs/_static/image8.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image9.png)

**Ilustracja 8**. użyj metody `GetProductsByCategoryID(categoryID)` `ProductsBLL` Class (kliknij,[Aby wyświetlić obraz o pełnym rozmiarze](building-a-custom-database-driven-site-map-provider-cs/_static/image10.png))

Ostatnim krokiem w Kreatorze konfiguracji źródła danych jest wyświetlanie danych źródłowych parametrów dla *IDkategorii*. Ponieważ te informacje są przesyłane za pośrednictwem pola QueryString `CategoryID`, z listy rozwijanej wybierz pozycję QueryString i wprowadź wartość IDKategorii w polu tekstowym QueryStringField, jak pokazano na rysunku 9. Kliknij przycisk Zakończ, aby zakończyć pracę kreatora.

[![użyć pola IDKategorii QueryString dla parametru IDKategorii](building-a-custom-database-driven-site-map-provider-cs/_static/image9.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image11.png)

**Rysunek 9**. użyj pola `CategoryID` QueryString dla parametru *IDkategorii* (kliknij,[Aby wyświetlić obraz o pełnym rozmiarze](building-a-custom-database-driven-site-map-provider-cs/_static/image12.png))

Po ukończeniu działania kreatora program Visual Studio doda odpowiednie BoundFields oraz CheckBoxField do widoku GridView dla pól danych produktu. Usuń wszystkie oprócz `ProductName`, `UnitPrice`i `SupplierName` BoundFields. Dostosuj te trzy BoundFields `HeaderText` właściwości, aby odczytywać odpowiednio produkt, cenę i dostawcę. Sformatuj `UnitPrice` BoundField jako walutę.

Następnie Dodaj element HyperLinkField i przenieś go do pozycji z lewej strony. Ustaw jej Właściwość `Text`, aby wyświetlić szczegóły, jej Właściwość `DataNavigateUrlFields` do `ProductID`oraz jej Właściwość `DataNavigateUrlFormatString` do `~/SiteMapProvider/ProductDetails.aspx?ProductID={0}`.

![Dodaj szczegóły widoku HyperLinkField wskazujące ProductDetails. aspx](building-a-custom-database-driven-site-map-provider-cs/_static/image10.gif)

**Ilustracja 10**. Dodawanie szczegółów widoku HyperLinkField, które wskazują `ProductDetails.aspx`

Po wprowadzeniu tych dostosowań, znaczniki deklaratywne GridView i ObjectDataSource s powinny wyglądać podobnie do następujących:

[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample3.aspx)]

Wróć do wyświetlania `Default.aspx` za pomocą przeglądarki i kliknij link Wyświetl produkty dla napojów. Spowoduje to przejście do `ProductsByCategory.aspx?CategoryID=1`, wyświetlenie nazw, cen i dostawców produktów w bazie danych Northwind należących do kategorii napoje (patrz rysunek 11). Możesz bardziej ulepszyć Tę stronę, aby dołączyć łącze do strony z listą kategorii (`Default.aspx`) i formantem DetailsView lub FormView, który wyświetla wybraną nazwę kategorii i opis.

[![są wyświetlane nazwy napojów, ceny i dostawcy](building-a-custom-database-driven-site-map-provider-cs/_static/image11.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image13.png)

**Rysunek 11**. wyświetlane są nazwy napojów, ceny i dostawcy ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](building-a-custom-database-driven-site-map-provider-cs/_static/image14.png))

## <a name="step-4-showing-a-product-s-details"></a>Krok 4. Pokazywanie szczegółowych informacji o produkcie

Na ostatniej stronie `ProductDetails.aspx`wyświetlane są szczegóły wybranych produktów. Otwórz `ProductDetails.aspx` i przeciągnij element DetailsView z przybornika do projektanta. Ustaw właściwość DetailsView s `ID`, aby `ProductInfo` i wyczyścić `Height` i `Width` wartości właściwości. Z jego tagu inteligentnego Powiąż element DetailsView z nowym elementem ObjectDataSource o nazwie `ProductDataSource`, konfigurując element ObjectDataSource do ściągania danych z metody `GetProductByProductID(productID)` klasy `ProductsBLL`. Podobnie jak w przypadku wcześniejszych stron sieci Web utworzonych w krokach 2 i 3, Ustaw listę rozwijaną na kartach Aktualizuj, Wstaw i Usuń do (brak).

[![skonfigurować element ObjectDataSource do korzystania z metody GetProductByProductID (productID)](building-a-custom-database-driven-site-map-provider-cs/_static/image12.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image15.png)

**Ilustracja 12**. Konfigurowanie elementu ObjectDataSource do używania metody `GetProductByProductID(productID)` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](building-a-custom-database-driven-site-map-provider-cs/_static/image16.png))

Ostatnim krokiem w Kreatorze konfiguracji źródła danych jest wyświetlanie danych źródłowych parametru *ProductID* . Ponieważ te dane pochodzą z pola QueryString `ProductID`, Ustaw listę rozwijaną na QueryString i pole tekstowe QueryStringField na ProductID. Na koniec kliknij przycisk Zakończ, aby zakończyć pracę kreatora.

[![skonfigurować parametr productID, aby ściągnąć jego wartość z pola IDProduktu QueryString](building-a-custom-database-driven-site-map-provider-cs/_static/image13.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image17.png)

**Ilustracja 13**. Skonfiguruj parametr *ProductID* , aby ściągnąć jego wartość z pola `ProductID` QueryString ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](building-a-custom-database-driven-site-map-provider-cs/_static/image18.png))

Po zakończeniu działania Kreatora konfiguracji źródła danych program Visual Studio utworzy odpowiednie BoundFields i CheckBoxField w widoku DetailsView dla pól danych produktu. Usuń `ProductID`, `SupplierID`i `CategoryID` BoundFields i skonfiguruj pozostałe pola zgodnie z oczekiwaniami. Po kilkuniu konfiguracji estetycznych, moje znaczniki deklaratywne i ObjectDataSource języka, wyglądały następująco:

[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample4.aspx)]

Aby przetestować Tę stronę, Wróć do `Default.aspx` i kliknij pozycję Wyświetl produkty dla kategorii napoje. Z listy napojów produktów kliknij link Wyświetl szczegóły dla Chai herbata. Spowoduje to przejście do `ProductDetails.aspx?ProductID=1`, w którym są wyświetlane szczegółowe informacje o Chai Tea (zobacz rysunek 14).

[![Chai herbata, Kategoria, Cena i inne informacje są wyświetlane](building-a-custom-database-driven-site-map-provider-cs/_static/image14.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image19.png)

**Rysunek 14**: dostawca Chai herbata, Kategoria, Cena i inne informacje są wyświetlane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](building-a-custom-database-driven-site-map-provider-cs/_static/image20.png))

## <a name="step-5-understanding-the-inner-workings-of-a-site-map-provider"></a>Krok 5. zrozumienie wewnętrznego działania dostawcy mapy witryny

Mapa witryny jest reprezentowana w pamięci serwera sieci Web jako kolekcja wystąpień `SiteMapNode`, które tworzą hierarchię. Musi istnieć tylko jeden katalog główny, a wszystkie węzły inne niż główne muszą mieć dokładnie jeden węzeł nadrzędny, a wszystkie węzły mogą mieć dowolną liczbę elementów podrzędnych. Każdy obiekt `SiteMapNode` reprezentuje sekcję w strukturze witryny sieci Web; te sekcje często mają odpowiednią stronę sieci Web. W związku z tym [klasa`SiteMapNode`](https://msdn.microsoft.com/library/system.web.sitemapnode.aspx) ma właściwości, takie jak `Title`, `Url`i `Description`, które zawierają informacje dotyczące sekcji, którą reprezentuje `SiteMapNode`. Istnieje również właściwość `Key`, która jednoznacznie identyfikuje poszczególne `SiteMapNode` w hierarchii, a także właściwości służące do ustanowienia tej hierarchii `ChildNodes`, `ParentNode`, `NextSibling`, `PreviousSibling`i tak dalej.

Rysunek 15 przedstawia ogólną strukturę mapy witryny na rysunku 1, ale z szczegółowymi informacjami o implementacji.

[![każda SiteMapNode ma właściwości, takie jak tytuł, adres URL, klucz i tak dalej.](building-a-custom-database-driven-site-map-provider-cs/_static/image16.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image15.gif)

**Ilustracja 15**: Każda `SiteMapNode` ma właściwości, takie jak `Title`, `Url`, `Key`i tak dalej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](building-a-custom-database-driven-site-map-provider-cs/_static/image17.gif))

Mapa witryny jest dostępna za pomocą [klasy`SiteMap`](https://msdn.microsoft.com/library/system.web.sitemap.aspx) w [przestrzeni nazw`System.Web`](https://msdn.microsoft.com/library/system.web.aspx). Ta właściwość klasy s `RootNode` zwraca wystąpienie `SiteMapNode` głównej mapy witryny. `CurrentNode` zwraca `SiteMapNode`, którego właściwość `Url` pasuje do adresu URL aktualnie żądanej strony. Ta klasa jest używana wewnętrznie przez kontrolki sieci Web ASP.NET 2,0 s nawigacji.

Po uzyskaniu dostępu do właściwości `SiteMap` klasy s musi ona serializować strukturę mapy witryny z pewnego trwałego nośnika do pamięci. Jednak logika serializacji mapy witryny nie jest sztywno zakodowana w klasie `SiteMap`. Zamiast tego w czasie wykonywania Klasa `SiteMap` określa, który *dostawca* mapy lokacji ma być używany do serializacji. Domyślnie używana jest [klasa`XmlSiteMapProvider`](https://msdn.microsoft.com/library/system.web.xmlsitemapprovider.aspx) , która odczytuje strukturę mapy lokacji z poprawnie sformatowanego pliku XML. Jednak w przypadku małej ilości pracy możemy utworzyć własnego niestandardowego dostawcę mapy witryny.

Wszyscy dostawcy mapy witryny muszą pochodzić z [klasy`SiteMapProvider`](https://msdn.microsoft.com/library/system.web.sitemapprovider.aspx), która zawiera podstawowe metody i właściwości wymagane przez dostawców mapy witryny, ale pomija wiele szczegółów implementacji. Druga klasa, [`StaticSiteMapProvider`](https://msdn.microsoft.com/library/system.web.staticsitemapprovider.aspx), rozszerza klasę `SiteMapProvider` i zawiera bardziej niezawodną implementację wymaganej funkcjonalności. Wewnętrznie w `StaticSiteMapProvider` są przechowywane `SiteMapNode` wystąpienia mapy witryny w `Hashtable` i przedstawiono metody, takie jak `AddNode(child, parent)`, `RemoveNode(siteMapNode),` i `Clear()`, które dodają i usuwają `SiteMapNode` s do wewnętrznej `Hashtable`. `XmlSiteMapProvider` pochodzi od `StaticSiteMapProvider`.

Podczas tworzenia niestandardowego dostawcy mapy witryny, który rozszerza `StaticSiteMapProvider`, istnieją dwie metody abstrakcyjne, które muszą zostać zastąpione: [`BuildSiteMap`](https://msdn.microsoft.com/library/system.web.staticsitemapprovider.buildsitemap.aspx) i [`GetRootNodeCore`](https://msdn.microsoft.com/library/system.web.sitemapprovider.getrootnodecore.aspx). `BuildSiteMap`, zgodnie z jego nazwą, jest odpowiedzialny za ładowanie struktury mapy witryny z magazynu trwałego i konstruowanie jej w pamięci. `GetRootNodeCore` zwraca węzeł główny w mapie witryny.

Zanim aplikacja sieci Web będzie mogła korzystać z dostawcy mapy witryny, musi być zarejestrowana w konfiguracji aplikacji. Domyślnie Klasa `XmlSiteMapProvider` jest zarejestrowana przy użyciu nazwy `AspNetXmlSiteMapProvider`. Aby zarejestrować dodatkowych dostawców mapy witryny, Dodaj następujące znaczniki do `Web.config`:

[!code-xml[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample5.xml)]

Wartość *Nazwa* przypisuje do dostawcy nazwę, którą można odczytać przez człowieka, podczas gdy *Typ* określa w pełni kwalifikowaną nazwę typu dostawcy mapy witryny. Poszukamy konkretnych wartości *nazwy* i wartości *typów* w kroku 7, po utworzeniu niestandardowego dostawcy mapy witryny.

Klasa dostawcy mapy witryny jest tworzona przy pierwszym dostępie z klasy `SiteMap` i pozostaje w pamięci przez okres istnienia aplikacji sieci Web. Ponieważ istnieje tylko jedno wystąpienie dostawcy mapy witryny, które może zostać wywołane przez wielu osób odwiedzających witrynę sieci Web, konieczna jest *bezpieczna wątkowo*Metoda dostawcy.

Ze względu na wydajność i skalowalność należy pamiętać, że pamięć podręczna jest w pamięci podręcznej struktury mapy lokacji i zwrócić tę buforowaną strukturę zamiast tworzyć ją przy każdym wywołaniu metody `BuildSiteMap`. `BuildSiteMap` może być wywoływana kilka razy na żądanie strony dla użytkownika, w zależności od kontrolek nawigacyjnych używanych na stronie oraz głębokości struktury mapy witryny. W każdym przypadku, jeśli nie buforuje struktury mapy witryny w `BuildSiteMap` następnie przy każdym wywołaniu, należy ponownie pobrać informacje o produkcie i kategorii z architektury (co spowoduje wykonanie zapytania do bazy danych). Zgodnie z opisem w poprzednich samouczkach buforowania dane buforowane mogą stać się przestarzałe. Aby to wymusić, możemy użyć wygaśnięcia opartych na zależnościach czasu lub w pamięci podręcznej SQL.

> [!NOTE]
> Dostawca mapy witryny może opcjonalnie zastąpić [metodę`Initialize`](https://msdn.microsoft.com/library/system.web.sitemapprovider.initialize.aspx). `Initialize` jest wywoływany podczas pierwszego wystąpienia dostawcy mapy witryny i jest on przeszukiwany wszystkie atrybuty niestandardowe przypisane do dostawcy w `Web.config` w `<add>` elementu, takie jak: `<add name="name" type="type" customAttribute="value" />`. Jest to przydatne, jeśli chcesz zezwolić deweloperowi na określanie różnych ustawień związanych z dostawcą mapy witryny, bez konieczności modyfikowania kodu dostawcy. Na przykład w przypadku odczytywania danych kategorii i produktów bezpośrednio z bazy danych, w przeciwieństwie do architektury, firma Microsoft chce pozwolić, aby deweloper strony określił parametry połączenia bazy danych za pośrednictwem `Web.config` zamiast korzystać z zakodowanej wartości w kodzie dostawcy. Niestandardowy dostawca mapy witryny zostanie skompilowany w kroku 6 nie przesłania tej metody `Initialize`. Przykład użycia metody `Initialize` można znaleźć w artykule [Jan Prosise](http://www.wintellect.com/Weblogs/CategoryView,category,Jeff%20Prosise.aspx) s [przechowującym mapy witryn w programie SQL Server](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/) .

## <a name="step-6-creating-the-custom-site-map-provider"></a>Krok 6. Tworzenie niestandardowego dostawcy mapy witryny

Aby utworzyć niestandardowego dostawcę mapy witryny, który kompiluje mapę witryny z kategorii i produktów w bazie danych Northwind, musimy utworzyć klasę rozszerzającą `StaticSiteMapProvider`. W kroku 1 zażądano dodania folderu `CustomProviders` w folderze `App_Code` — Dodaj nową klasę do tego folderu o nazwie `NorthwindSiteMapProvider`. Dodaj następujący kod do klasy `NorthwindSiteMapProvider`:

[!code-csharp[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample6.cs)]

Niech Rozpocznij od eksplorowania tej klasy `BuildSiteMap` Metoda, która rozpoczyna się od [instrukcji`lock`](https://msdn.microsoft.com/library/c5kehkcz.aspx). Instrukcja `lock` zezwala tylko jednemu wątkowi na wprowadzenie, a tym samym serializacji dostęp do jego kodu i uniemożliwia dwa współbieżne wątki przed przechodzeniem na jeden inny wyglądają.

Zmienna `SiteMapNode` na poziomie klasy `root` służy do buforowania struktury mapy witryny. Gdy mapa witryny jest tworzona po raz pierwszy lub po raz pierwszy po zmodyfikowaniu danych źródłowych, `root` zostanie `null`, a struktura mapy witryny zostanie skonstruowana. Węzeł główny mapy witryny jest przypisywany do `root` podczas procesu konstruowania, dzięki czemu przy następnym wywołaniu tej metody `root` nie będzie `null`. W związku z tym, tak długo, jak `root` nie jest `null` struktura mapy witryny zostanie zwrócona do obiektu wywołującego bez konieczności ponownego tworzenia.

Jeśli katalog główny jest `null`, struktura mapy witryny jest tworzona na podstawie informacji o produkcie i kategorii. Mapę witryny można utworzyć, tworząc `SiteMapNode` wystąpienia, a następnie tworząc hierarchię za pomocą wywołań do metody `StaticSiteMapProvider` klasy s `AddNode`. `AddNode` wykonuje wewnętrzne Księgowość, przechowując wystąpienia `SiteMapNode` w `Hashtable`. Przed rozpoczęciem konstruowania hierarchii rozpoczynamy od wywołania metody `Clear`, która czyści elementy z `Hashtable`wewnętrznej. Następnie Metoda `ProductsBLL` klasy s `GetProducts` i `ProductsDataTable` wyniki są przechowywane w zmiennych lokalnych.

Konstrukcja mapy witryny rozpocznie się, tworząc węzeł główny i przypisując go do `root`. Przeciążanie [konstruktora`SiteMapNode` s](https://msdn.microsoft.com/library/system.web.sitemapnode.sitemapnode.aspx) użyte w tym miejscu i w tym `BuildSiteMap` jest przenoszone następujące informacje:

- Odwołanie do dostawcy mapy witryny (`this`).
- `Key``SiteMapNode`. Ta wymagana wartość musi być unikatowa dla każdego `SiteMapNode`.
- `Url``SiteMapNode`. `Url` jest opcjonalne, ale jeśli zostały podane, każda `SiteMapNode` `Url` wartość musi być unikatowa.
- Wymagany `Title``SiteMapNode` s.

Wywołanie metody `AddNode(root)` dodaje `root` `SiteMapNode` do mapy witryny jako element główny. Następnie zostaną wyliczone wszystkie `ProductRow` w `ProductsDataTable`. Jeśli istnieje już `SiteMapNode` dla kategorii Current Product, odwołanie jest przywoływane. W przeciwnym razie nowy `SiteMapNode` kategorii zostanie utworzony i dodany jako element podrzędny `SiteMapNode``root` za pomocą wywołania metody `AddNode(categoryNode, root)`. Po znalezieniu lub utworzeniu odpowiedniej kategorii `SiteMapNode` węźle zostanie utworzony `SiteMapNode` dla bieżącego produktu i dodany jako element podrzędny kategorii `SiteMapNode` za pośrednictwem `AddNode(productNode, categoryNode)`. Należy zauważyć, że Kategoria `SiteMapNode` s `Url` wartość właściwości jest `~/SiteMapProvider/ProductsByCategory.aspx?CategoryID=categoryID`, podczas gdy produktu `SiteMapNode` s `Url` zostanie przypisany `~/SiteMapNode/ProductDetails.aspx?ProductID=productID`.

> [!NOTE]
> Te produkty, które mają `NULL` wartość bazy danych dla ich `CategoryID` są pogrupowane w kategorii `SiteMapNode` której Właściwość `Title` ma wartość None, a właściwość `Url` jest ustawiona na pusty ciąg. Zdecydowałem się ustawić `Url` na pusty ciąg, ponieważ metoda `ProductBLL` klasy `GetProductsByCategory(categoryID)` s nie ma obecnie możliwości zwrócenia tylko tych produktów z `NULL` `CategoryID` wartością. Warto również zademonstrować, jak kontrolki nawigacji renderują `SiteMapNode`, które nie mają wartości właściwości `Url`. Zachęcam do rozbudowania tego samouczka, tak aby Właściwość none `SiteMapNode` s `Url` wskazywała na `ProductsByCategory.aspx`, ale tylko te produkty mające `NULL` `CategoryID` wartości.

Po skonstruowaniu mapy witryny do pamięci podręcznej danych zostanie dodany dowolny obiekt, przy użyciu zależności pamięci podręcznej SQL `Categories` i `Products` tabelach za pośrednictwem obiektu `AggregateCacheDependency`. Wiemy, jak korzystać z zależności w pamięci podręcznej SQL w poprzednim samouczku, *przy użyciu zależności pamięci podręcznej SQL*. Dostawca niestandardowego mapowania witryn używa jednak przeciążenia metody `Insert`ej pamięci podręcznej danych, która nie została jeszcze zaeksplorowania. To przeciążenie przyjmuje jako końcowy parametr wejściowy delegata, który jest wywoływany, gdy obiekt zostanie usunięty z pamięci podręcznej. W każdym przypadku przekazujemy nowe [`CacheItemRemovedCallback` delegata](https://msdn.microsoft.com/library/system.web.caching.cacheitemremovedcallback.aspx) wskazujące na metodę `OnSiteMapChanged` zdefiniowana w dalszej części klasy `NorthwindSiteMapProvider`.

> [!NOTE]
> Reprezentacja mapy witryny w pamięci jest buforowana przez zmienną na poziomie klasy `root`. Ponieważ istnieje tylko jedno wystąpienie niestandardowej klasy dostawcy mapy witryny i ponieważ to wystąpienie jest współużytkowane przez wszystkie wątki w aplikacji sieci Web, ta zmienna klasy służy jako pamięć podręczna. Metoda `BuildSiteMap` używa również pamięci podręcznej danych, ale tylko jako środek do odbierania powiadomień, gdy dane źródłowe bazy danych w `Categories` lub tabelach `Products` zmienią się. Należy pamiętać, że wartość wprowadzona w pamięci podręcznej danych jest tylko bieżącą datą i godziną. Rzeczywiste dane mapy witryny *nie* są umieszczane w pamięci podręcznej danych.

Metoda `BuildSiteMap` zostanie zakończona, zwracając węzeł główny mapy witryny.

Pozostałe metody są dość proste. `GetRootNodeCore` jest odpowiedzialny za zwracanie węzła głównego. Ponieważ `BuildSiteMap` zwraca element główny, `GetRootNodeCore` po prostu zwraca wartość zwracaną `BuildSiteMap` s. Metoda `OnSiteMapChanged` ustawia `root` z powrotem na `null` po usunięciu elementu pamięci podręcznej. Po powrocie do `null`przy następnym czasie `BuildSiteMap` jest wywoływana, struktura mapy witryny zostanie odbudowana. Na koniec Właściwość `CachedDate` zwraca wartość daty i godziny przechowywanej w pamięci podręcznej danych, jeśli taka wartość istnieje. Ta właściwość może być używana przez dewelopera strony do określania, kiedy dane mapy lokacji były ostatnio buforowane.

## <a name="step-7-registering-thenorthwindsitemapprovider"></a>Krok 7: rejestrowanie`NorthwindSiteMapProvider`

Aby nasza aplikacja sieci Web korzystała z dostawcy mapy witryny `NorthwindSiteMapProvider` utworzonego w kroku 6, należy zarejestrować go w sekcji `<siteMap>` w `Web.config`. W tym celu Dodaj następujące znaczniki w `<system.web>` elementu w `Web.config`:

[!code-xml[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample7.xml)]

Ten znacznik wykonuje dwie czynności: po pierwsze wskazuje, że wbudowana `AspNetXmlSiteMapProvider` jest domyślnym dostawcą mapy witryny; Po drugie rejestruje niestandardowego dostawcę mapy witryny utworzonego w kroku 6 z przyjazną dla człowieka nazwą Northwind.

> [!NOTE]
> W przypadku dostawców mapy witryny znajdujących się w folderze `App_Code` aplikacji wartość atrybutu `type` jest po prostu nazwą klasy. Alternatywnie można utworzyć niestandardowego dostawcę mapy witryny w osobnym projekcie biblioteki klas z skompilowanym zestawem umieszczonym w katalogu aplikacji sieci Web `/Bin`. W takim przypadku `type` wartością atrybutu będzie *przestrzeń nazw*. *ClassName*, *AssemblyName* .

Po aktualizacji `Web.config`Poświęć chwilę na wyświetlenie dowolnej strony z samouczków w przeglądarce. Należy pamiętać, że interfejs nawigacji po lewej stronie nadal pokazuje sekcje i samouczki zdefiniowane w `Web.sitemap`. Jest to spowodowane tym, że `AspNetXmlSiteMapProvider` jako domyślny dostawca. Aby utworzyć element interfejsu użytkownika nawigacji, który używa `NorthwindSiteMapProvider`, należy jawnie określić, że powinien być używany dostawca mapy witryny Northwind. Zobaczmy, jak to zrobić w kroku 8.

## <a name="step-8-displaying-site-map-information-using-the-custom-site-map-provider"></a>Krok 8. Wyświetlanie informacji o mapie witryny przy użyciu niestandardowego dostawcy mapy witryny

Po utworzeniu niestandardowego dostawcy mapy witryny i zarejestrowaniu go w `Web.config`ponownie przygotujemy do dodania kontrolek nawigacji do stron `Default.aspx`, `ProductsByCategory.aspx`i `ProductDetails.aspx` w folderze `SiteMapProvider`. Zacznij od otworzenia strony `Default.aspx` i przeciągnij `SiteMapPath` z przybornika na projektanta. Kontrolka ścieżki mapy witryny znajduje się w sekcji nawigacji w przyborniku.

[![dodać ścieżki mapy witryny do default. aspx](building-a-custom-database-driven-site-map-provider-cs/_static/image19.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image18.gif)

**Ilustracja 16**. Dodawanie ścieżki mapy witryny do `Default.aspx` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](building-a-custom-database-driven-site-map-provider-cs/_static/image20.gif))

Kontrolka ścieżki mapy witryny wyświetla pasek nawigacyjny wskazujący bieżącą lokalizację strony w ramach mapy witryny. Dodaliśmy ścieżki mapy witryny w górnej części strony wzorcowej z powrotem na *stronach wzorcowych i* w samouczku nawigacji po witrynie.

Poświęć chwilę na wyświetlenie tej strony za pomocą przeglądarki. Ścieżki mapy witryny dodany na rysunku 16 używa domyślnego dostawcy mapy witryny, pobierając jego dane z `Web.sitemap`. W związku z tym, w obszarze nawigacji znajdują się główne &gt; dostosowywania mapy witryny, podobnie jak w przypadku stron nadrzędnych w prawym górnym rogu.

[![pasek nawigacyjny używa domyślnego dostawcy mapy witryny](building-a-custom-database-driven-site-map-provider-cs/_static/image22.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image21.gif)

**Ilustracja 17**. pasek nawigacyjny używa domyślnego dostawcy mapy witryny ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](building-a-custom-database-driven-site-map-provider-cs/_static/image23.gif))

Aby dodać ścieżki mapy witryny (rysunek 16), Użyj niestandardowego dostawcy mapy witryny, który został utworzony w kroku 6, ustaw jego [właściwość`SiteMapProvider`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sitemappath.sitemapprovider.aspx) na Northwind, nazwę, którą przypisano do `NorthwindSiteMapProvider` w `Web.config`. Niestety, Projektant nadal korzysta z domyślnego dostawcy mapy witryny, ale jeśli zostanie wyświetlona strona za pośrednictwem przeglądarki po wprowadzeniu tej zmiany właściwości, zobaczysz, że na tym pasku nawigacyjnym jest teraz używany niestandardowy dostawca mapy witryny.

[![do stron nadrzędnych jest teraz stosowana niestandardowa NorthwindSiteMapProvider dostawcy mapy witryny](building-a-custom-database-driven-site-map-provider-cs/_static/image25.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image24.gif)

**Ilustracja 18**. obraz przedstawiający teraz używa niestandardowego dostawcy mapy witryny `NorthwindSiteMapProvider` ([kliknij, aby wyświetlić obrazy o pełnym rozmiarze](building-a-custom-database-driven-site-map-provider-cs/_static/image26.gif))

Kontrolka ścieżki mapy witryny wyświetla bardziej funkcjonalny interfejs użytkownika na stronach `ProductsByCategory.aspx` i `ProductDetails.aspx`. Dodaj ścieżki mapy witryny do tych stron, ustawiając właściwość `SiteMapProvider` w obu wartościach na Northwind. W `Default.aspx` kliknij link Wyświetl produkty dla napojów, a następnie na linku Wyświetl szczegóły dla Chai herbata. Jak pokazano na rysunku 19, pasek nawigacyjny zawiera bieżącą sekcję mapy witryny (Chai herbata) i jej elementy nadrzędne: napoje i wszystkie kategorie.

[![do stron nadrzędnych jest teraz stosowana niestandardowa NorthwindSiteMapProvider dostawcy mapy witryny](building-a-custom-database-driven-site-map-provider-cs/_static/image27.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image21.png)

**Ilustracja 19**. pasek nawigacyjny używa teraz niestandardowego dostawcy mapy witryny `NorthwindSiteMapProvider` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](building-a-custom-database-driven-site-map-provider-cs/_static/image22.png))

Inne elementy interfejsu użytkownika nawigacji mogą służyć oprócz ścieżki mapy witryny, takich jak kontrolki menu i TreeView. Strony `Default.aspx`, `ProductsByCategory.aspx`i `ProductDetails.aspx` w pobraniu dla tego samouczka, na przykład wszystkie kontrolki menu dołączania (Zobacz Rysunek 20). Zobacz temat [Badanie funkcji nawigacyjnych witryny ASP.NET 2,0 s](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx) i sekcji [Używanie kontrolek nawigacji](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/navigation/sitenavcontrols.aspx) w witrynie [przewodnika szybki start w programie ASP.NET 2,0](https://quickstarts.asp.net/QuickStartv20/aspnet/) , aby uzyskać bardziej szczegółowy wygląd kontrolek nawigacji i systemu mapy witryny w ASP.NET 2,0.

[![formant menu zawiera listę kategorii i produktów](building-a-custom-database-driven-site-map-provider-cs/_static/image29.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image28.gif)

**Ilustracja 20**. kontrolka menu zawiera listę kategorii i produktów ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](building-a-custom-database-driven-site-map-provider-cs/_static/image30.gif))

Jak wspomniano wcześniej w tym samouczku, można programowo uzyskać dostęp do struktury mapy witryny za pomocą klasy `SiteMap`. Poniższy kod zwraca `SiteMapNode` głównej domyślnego dostawcy:

[!code-csharp[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample8.cs)]

Ponieważ `AspNetXmlSiteMapProvider` jest domyślnym dostawcą aplikacji, powyższy kod zwróci węzeł główny zdefiniowany w `Web.sitemap`. Aby odwołać się do dostawcy mapy witryny innego niż domyślny, użyj właściwości `SiteMap` klasy s [`Providers`](https://msdn.microsoft.com/library/system.web.sitemap.providers.aspx) , takiej jak:

[!code-csharp[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample9.cs)]

Gdzie *name* to nazwa niestandardowego dostawcy mapy witryny (Northwind dla naszej aplikacji sieci Web).

Aby uzyskać dostęp do elementu członkowskiego określonego dla dostawcy mapy witryny, należy użyć `SiteMap.Providers["name"]` do pobrania wystąpienia dostawcy, a następnie rzutowania go na odpowiedni typ. Na przykład aby wyświetlić Właściwość `NorthwindSiteMapProvider` s `CachedDate` na stronie ASP.NET, użyj następującego kodu:

[!code-csharp[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample10.cs)]

> [!NOTE]
> Pamiętaj, aby przetestować funkcję zależności pamięci podręcznej SQL. Po odwiedzeniu stron `Default.aspx`, `ProductsByCategory.aspx`i `ProductDetails.aspx` przejdź do jednego z samouczków w sekcji Edytowanie, wstawianie i usuwanie, a następnie Edytuj nazwę kategorii lub produktu. Następnie wróć do jednej ze stron w folderze `SiteMapProvider`. Przy założeniu, że przed przekazaniem przez mechanizm sondowania zmian w źródłowej bazie danych zostanie zanotowany wystarczająco dużo czasu, należy zaktualizować mapę lokacji, aby pokazać nową nazwę produktu lub kategorii.

## <a name="summary"></a>Podsumowanie

ASP.NET 2,0 s funkcje mapy witryny obejmują klasę `SiteMap`, wiele wbudowanych kontrolek sieci Web nawigacji i domyślnego dostawcę mapy witryny, który oczekuje informacji o mapie lokacji utrwalonych w pliku XML. Aby można było korzystać z informacji o mapie witryny z innego źródła, takiego jak baza danych, Architektura aplikacji lub zdalna usługa sieci Web, należy utworzyć niestandardowego dostawcę mapy witryny. Obejmuje to Tworzenie klasy, która dziedziczy bezpośrednio lub pośrednio z klasy `SiteMapProvider`.

W tym samouczku przedstawiono sposób tworzenia niestandardowego dostawcy mapy witryny, który bazuje na mapie witryny na informacje o produkcie i kategorii, które pochodzą z architektury aplikacji. Nasz dostawca rozszerzył klasę `StaticSiteMapProvider` i wymagała utworzenia metody `BuildSiteMap`, która pobrała dane, konstruuje hierarchię mapy lokacji i buforuje strukturę uzyskaną w zmiennej na poziomie klasy. Użyto zależności pamięci podręcznej SQL z funkcją wywołania zwrotnego, aby unieważniać buforowaną strukturę w przypadku zmodyfikowania bazowej `Categories` lub `Products` danych.

Szczęśliwe programowanie!

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat tematów omówionych w tym samouczku, zapoznaj się z następującymi zasobami:

- [Przechowywanie map witryn w SQL Server](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/) i [dostawca mapy witryny SQL zaczekał](https://msdn.microsoft.com/msdnmag/issues/06/02/wickedcode/default.aspx)
- [Zapoznaj się z modelem dostawcy ASP.NET 2,0 s](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx)
- [Zestaw narzędzi dostawcy](https://msdn.microsoft.com/asp.net/aa336558.aspx)
- [Badanie funkcji nawigowania po witrynie ASP.NET 2,0 s](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Recenzenci liderzy dla tego samouczka to Dave Gardner, Zack Kowalski, Teresa Murphy i Bernadette Leigh. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Dalej](building-a-custom-database-driven-site-map-provider-vb.md)

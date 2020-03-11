---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-across-two-pages-vb
title: Filtrowanie wzorców/szczegółów na dwóch stronach (VB) | Microsoft Docs
author: rick-anderson
description: W tym samouczku zaimplementujemy ten wzorzec przy użyciu widoku GridView, aby wyświetlić listę dostawców w bazie danych. Każdy wiersz dostawcy w widoku GridView będzie zawierał widok...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 361d6a44-3f1f-4daf-85df-d4c2b8bf065d
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-across-two-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: 252c6d5e48aff0087e090e3ddc1f58c84a2c030e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78529065"
---
# <a name="masterdetail-filtering-across-two-pages-vb"></a>Filtrowanie rekordu głównego/szczegółów na dwóch stronach (VB)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](https://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_9_VB.exe) lub [Pobierz plik PDF](master-detail-filtering-across-two-pages-vb/_static/datatutorial09vb1.pdf)

> W tym samouczku zaimplementujemy ten wzorzec przy użyciu widoku GridView, aby wyświetlić listę dostawców w bazie danych. Każdy wiersz dostawcy w widoku GridView będzie zawierać łącze Wyświetl produkty, które po kliknięciu spowoduje przejście użytkownika do osobnej strony, która wyświetla listę tych produktów dla wybranego dostawcy.

## <a name="introduction"></a>Wprowadzenie

W poprzednich dwóch samouczkach pokazano, jak wyświetlić raporty wzorzec/szczegóły na pojedynczej stronie sieci Web przy użyciu kontrolek DropDownList, aby wyświetlić "główne" rekordy i formant [GridView](master-detail-filtering-with-a-dropdownlist-vb.md) lub [DetailsView](master-detail-filtering-with-two-dropdownlists-vb.md) , aby wyświetlić "Szczegóły". Innym typowym wzorcem używanym w raportach Master/Details jest posiadanie rekordów głównych na jednej stronie sieci Web i szczegółowych informacji wyświetlanych na innym. Witryna internetowa Forum, podobnie jak [fora ASP.NET](https://forums.asp.net/), to doskonały przykład tego wzorca. Fora ASP.NET składają się z różnych Wprowadzenie forów, formularzy sieci Web, kontrolek prezentacji danych i tak dalej. Każde Forum składa się z wielu wątków, a każdy wątek składa się z szeregu wpisów. Na stronie głównej forów ASP.NET są wyświetlane fora. Kliknięcie forum przeniesie Cię do strony `ShowForum.aspx`, która zawiera listę wątków dla tego forum. Podobnie kliknięcie wątku powoduje `ShowPost.aspx`, w którym są wyświetlane wpisy dla klikniętego wątku.

W tym samouczku zaimplementujemy ten wzorzec przy użyciu widoku GridView, aby wyświetlić listę dostawców w bazie danych. Każdy wiersz dostawcy w widoku GridView będzie zawierać łącze Wyświetl produkty, które po kliknięciu spowoduje przejście użytkownika do osobnej strony, która wyświetla listę tych produktów dla wybranego dostawcy.

## <a name="step-1-addingsupplierlistmasteraspxandproductsforsupplierdetailsaspxpages-to-thefilteringfolder"></a>Krok 1. Dodawanie`SupplierListMaster.aspx`i`ProductsForSupplierDetails.aspx`stron do folderu`Filtering`

Podczas definiowania układu strony w trzecim samouczku dodaliśmy kilka stron "Starter" w folderach `BasicReporting`, `Filtering`i `CustomFormatting`. Jednak w tej chwili nie dodano strony Starter dla tego samouczka, więc Dodaj dwie nowe strony do folderu `Filtering`: `SupplierListMaster.aspx` i `ProductsForSupplierDetails.aspx`. `SupplierListMaster.aspx` będzie zawierać listę "Master" (dostawcy), gdy `ProductsForSupplierDetails.aspx` będą wyświetlały produkty wybranego dostawcy.

Podczas tworzenia tych dwóch nowych stron należy mieć pewność, że zostaną one skojarzone ze stroną wzorcową `Site.master`.

![Dodawanie stron SupplierListMaster. aspx i ProductsForSupplierDetails. aspx do folderu filtrowania](master-detail-filtering-across-two-pages-vb/_static/image1.png)

**Rysunek 1**. Dodawanie `SupplierListMaster.aspx` i `ProductsForSupplierDetails.aspx` stron do folderu `Filtering`

Ponadto podczas dodawania nowych stron do projektu należy zaktualizować plik mapy witryny, `Web.sitemap`w odpowiedni sposób. W tym samouczku po prostu Dodaj stronę `SupplierListMaster.aspx` do mapy witryny, używając następującej zawartości XML jako elementu podrzędnego filtrowania raportów `<siteMapNode>`:

[!code-xml[Main](master-detail-filtering-across-two-pages-vb/samples/sample1.xml)]

> [!NOTE]
> Możesz pomóc zautomatyzować proces aktualizowania pliku mapy witryny podczas dodawania nowych stron ASP.NET przy użyciu języka [KB.](http://odetocode.com/Blogs/scott/)bezpłatne [makro mapy witryny](http://odetocode.com/Blogs/scott/archive/2005/11/29/2537.aspx)programu Visual Studio.

## <a name="step-2-displaying-the-supplier-list-insupplierlistmasteraspx"></a>Krok 2. Wyświetlanie listy dostawców w`SupplierListMaster.aspx`

Po utworzeniu `SupplierListMaster.aspx` i `ProductsForSupplierDetails.aspx` stronach, następnym krokiem jest utworzenie widoku GridView dostawców w `SupplierListMaster.aspx`. Dodaj widok GridView do strony i powiąż go z nowym elementem ObjectDataSource. Ten element ObjectDataSource powinien używać metody `GetSuppliers()` klasy `SuppliersBLL`, aby zwracać wszystkich dostawców.

[![wybrać klasy SuppliersBLL](master-detail-filtering-across-two-pages-vb/_static/image3.png)](master-detail-filtering-across-two-pages-vb/_static/image2.png)

**Rysunek 2**. wybieranie klasy `SuppliersBLL` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](master-detail-filtering-across-two-pages-vb/_static/image4.png))

[![skonfigurować element ObjectDataSource do korzystania z metody getdostawcs ()](master-detail-filtering-across-two-pages-vb/_static/image6.png)](master-detail-filtering-across-two-pages-vb/_static/image5.png)

**Rysunek 3**. Konfigurowanie elementu ObjectDataSource do używania metody `GetSuppliers()` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](master-detail-filtering-across-two-pages-vb/_static/image7.png))

Musimy dołączyć link zatytułowany Wyświetl produkty w każdym wierszu GridView, który po kliknięciu przybiera użytkownikowi `ProductsForSupplierDetails.aspx` przekazywanie wartości `SupplierID` wybranego wiersza za pomocą ciągu QueryString. Na przykład, jeśli użytkownik kliknie łącze Wyświetl produkty dla dostawcy Warszawy (ma `SupplierID` wartość 4), powinny być wysyłane do `ProductsForSupplierDetails.aspx?SupplierID=4`.

Aby to osiągnąć, Dodaj element [HyperLinkField](https://msdn.microsoft.com/library/system.web.ui.webcontrols.hyperlinkfield.aspx) do widoku GridView, który dodaje hiperłącze do każdego wiersza GridView. Zacznij od kliknięcia linku Edytuj kolumny w tagu inteligentnym GridView. Następnie wybierz HyperLinkField z listy w lewym górnym rogu, a następnie kliknij przycisk Dodaj, aby dołączyć HyperLinkField na liście pól GridView.

[![dodać HyperLinkField do widoku GridView](master-detail-filtering-across-two-pages-vb/_static/image9.png)](master-detail-filtering-across-two-pages-vb/_static/image8.png)

**Ilustracja 4**. Dodawanie HyperLinkField do widoku GridView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-across-two-pages-vb/_static/image10.png))

HyperLinkField można skonfigurować tak, aby używała tego samego tekstu lub wartości adresu URL łącza w każdym wierszu GridView lub można oprzeć te wartości na wartościach danych powiązanych z każdym określonym wierszem. Aby określić wartość statyczną we wszystkich wierszach, użyj właściwości `Text` lub `NavigateUrl` HyperLinkField. Ponieważ chcemy, aby tekst łącza był taki sam dla wszystkich wierszy, ustaw właściwość `Text` HyperLinkField, aby wyświetlić produkty.

[![ustawić właściwość Text elementu HyperLinkField, aby wyświetlić produkty](master-detail-filtering-across-two-pages-vb/_static/image12.png)](master-detail-filtering-across-two-pages-vb/_static/image11.png)

**Rysunek 5**. ustaw właściwość `Text` HyperLinkField, aby wyświetlić produkty ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](master-detail-filtering-across-two-pages-vb/_static/image13.png))

Aby ustawić tekst lub wartość adresu URL na podstawie danych źródłowych powiązanych z wierszem GridView, Określ pola danych, z których wartości tekstowe lub adresy URL mają być ściągane we właściwościach `DataTextField` lub `DataNavigateUrlFields`. `DataTextField` można ustawić tylko dla jednego pola danych. `DataNavigateUrlFields`jednak można ustawić na listę pól danych rozdzielaną przecinkami. Często musimy oprzeć tekst lub adres URL na kombinacji wartości pola danych bieżącego wiersza i niektórych znaczników statycznych. Na przykład w tym samouczku chcemy, aby adres URL linków HyperLinkField był `ProductsForSupplierDetails.aspx?SupplierID=supplierID`, gdzie *`supplierID`* jest wartością `SupplierID` Row's każdego GridView. Należy zauważyć, że w tym miejscu wymagane są zarówno wartości statyczne, jak i oparte na danych: część `ProductsForSupplierDetails.aspx?SupplierID=` adresu URL linku jest statyczna, podczas gdy część *`supplierID`a* jest oparta na danych, ponieważ jej wartość jest własną `SupplierID` wartością każdego wiersza.

Aby wskazać kombinację wartości statycznych i opartych na danych, użyj właściwości `DataTextFormatString` i `DataNavigateUrlFormatString`. W tych właściwościach Wprowadź znacznik statyczny w razie potrzeby, a następnie użyj znacznika `{0}`, w którym ma zostać wyświetlona wartość pola określonego we właściwościach `DataTextField` lub `DataNavigateUrlFields`. Jeśli dla właściwości `DataNavigateUrlFields` określono wiele pól, użyj `{0}` miejsce, w którym ma zostać wstawiona wartość pierwszego pola, `{1}` dla drugiej wartości pola itd.

Zastosowanie tego do naszego samouczka wymaga ustawienia właściwości `DataNavigateUrlFields` na `SupplierID`, ponieważ to jest pole danych, którego wartość musi zostać dostosowana dla poszczególnych wierszy, i Właściwość `DataNavigateUrlFormatString` do `ProductsForSupplierDetails.aspx?SupplierID={0}`.

[![skonfigurować HyperLinkField do uwzględnienia odpowiedniego adresu URL linku na podstawie IDDostawcy](master-detail-filtering-across-two-pages-vb/_static/image15.png)](master-detail-filtering-across-two-pages-vb/_static/image14.png)

**Ilustracja 6**. Konfigurowanie HyperLinkField w celu uwzględnienia odpowiedniego adresu URL linku na podstawie `SupplierID` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](master-detail-filtering-across-two-pages-vb/_static/image16.png))

Po dodaniu HyperLinkField możesz dostosowywać pola GridView i zmieniać ich kolejność. Poniższe znaczniki pokazują widok GridView po wprowadzeniu drobnych dostosowań na poziomie pola.

[!code-aspx[Main](master-detail-filtering-across-two-pages-vb/samples/sample2.aspx)]

Poświęć chwilę na wyświetlenie strony `SupplierListMaster.aspx` za pomocą przeglądarki. Jak pokazano na rysunku 7, na stronie są obecnie wyświetlane wszystkie dostawcy, w tym link Pokaż produkty. Kliknięcie linku Wyświetl produkty spowoduje przejście do `ProductsForSupplierDetails.aspx`, przekazanie do `SupplierID` dostawcy w ciągu QueryString.

[![każdy wiersz dostawcy zawiera link Wyświetl produkty](master-detail-filtering-across-two-pages-vb/_static/image18.png)](master-detail-filtering-across-two-pages-vb/_static/image17.png)

**Rysunek 7**: każdy wiersz dostawcy zawiera link Wyświetl produkty ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](master-detail-filtering-across-two-pages-vb/_static/image19.png))

## <a name="step-3-listing-the-suppliers-products-inproductsforsupplierdetailsaspx"></a>Krok 3. Tworzenie listy produktów dostawcy w`ProductsForSupplierDetails.aspx`

W tym momencie strona `SupplierListMaster.aspx` wysyła użytkowników do `ProductsForSupplierDetails.aspx`, przekazując wybrany `SupplierID` dostawcy w ciągu QueryString. Ostatnim krokiem tego samouczka jest wyświetlenie produktów w widoku GridView w `ProductsForSupplierDetails.aspx`, których `SupplierID` jest równa `SupplierID` przekazaniu za pomocą ciągu QueryString. Aby to zrobić, Dodaj widok GridView do strony `ProductsForSupplierDetails.aspx` przy użyciu nowej kontrolki ObjectDataSource o nazwie `ProductsBySupplierDataSource`, która wywołuje metodę `GetProductsBySupplierID(supplierID)` z klasy `ProductsBLL`.

[![dodać nowego elementu ObjectDataSource o nazwie ProductsBySupplierDataSource](master-detail-filtering-across-two-pages-vb/_static/image21.png)](master-detail-filtering-across-two-pages-vb/_static/image20.png)

**Ilustracja 8**. Dodawanie nowego elementu ObjectDataSource o nazwie `ProductsBySupplierDataSource` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-across-two-pages-vb/_static/image22.png))

[![wybrać klasy ProductsBLL](master-detail-filtering-across-two-pages-vb/_static/image24.png)](master-detail-filtering-across-two-pages-vb/_static/image23.png)

**Ilustracja 9**. wybieranie klasy `ProductsBLL` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](master-detail-filtering-across-two-pages-vb/_static/image25.png))

[![, że element ObjectDataSource wywołuje metodę GetProductsBySupplierID (IDDostawcy)](master-detail-filtering-across-two-pages-vb/_static/image27.png)](master-detail-filtering-across-two-pages-vb/_static/image26.png)

**Ilustracja 10**. czy element ObjectDataSource wywołuje metodę `GetProductsBySupplierID(supplierID)` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](master-detail-filtering-across-two-pages-vb/_static/image28.png))

Ostatni krok kreatora konfiguracji źródła danych prosi nas o podanie źródła *`supplierID`* parametru metody `GetProductsBySupplierID(supplierID)`. Aby użyć wartości QueryString, ustaw Źródło parametru na QueryString i wprowadź nazwę wartości QueryString do użycia w polu tekstowym QueryStringField (`SupplierID`).

[![wypełnić wartość parametru IDDostawcy z wartości QueryString IDDostawcy](master-detail-filtering-across-two-pages-vb/_static/image30.png)](master-detail-filtering-across-two-pages-vb/_static/image29.png)

**Ilustracja 11**. Wypełnij wartość parametru *`supplierID`* za pomocą wartości `SupplierID` QueryString ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-across-two-pages-vb/_static/image31.png))

To wszystko! Rysunek 12 przedstawia stronę `ProductsForSupplierDetails.aspx` po odwiedzeniu przez kliknięcie linku dla Warszawy Traders z `SupplierListMaster.aspx`.

[![produkty dostarczone przez handlowców z Warszawy są wyświetlane](master-detail-filtering-across-two-pages-vb/_static/image33.png)](master-detail-filtering-across-two-pages-vb/_static/image32.png)

**Ilustracja 12**. pokazywane są produkty dostarczone przez handlowców z Warszawy ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](master-detail-filtering-across-two-pages-vb/_static/image34.png))

## <a name="displaying-supplier-information-inproductsforsupplierdetailsaspx"></a>Wyświetlanie informacji o dostawcy w`ProductsForSupplierDetails.aspx`

Jak pokazano na rysunku 12, Strona `ProductsForSupplierDetails.aspx` po prostu wyświetla listę produktów dostarczanych przez `SupplierID` określonych w ciągu QueryString. Jednak ktoś wysyłany bezpośrednio do tej strony nie wie, że rysunek 12 pokazuje produkty handlowców. W celu rozwiązania tego problemu można także wyświetlić informacje o dostawcach na tej stronie.

Zacznij od dodania widoku FormView powyżej elementu GridView. Utwórz nową kontrolkę ObjectDataSource o nazwie `SuppliersDataSource`, która wywołuje metodę `GetSupplierBySupplierID(supplierID)` klasy `SuppliersBLL`.

[![wybrać klasy SuppliersBLL](master-detail-filtering-across-two-pages-vb/_static/image36.png)](master-detail-filtering-across-two-pages-vb/_static/image35.png)

**Ilustracja 13**. wybieranie klasy `SuppliersBLL` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](master-detail-filtering-across-two-pages-vb/_static/image37.png))

[![, że element ObjectDataSource wywołuje metodę GetSupplierBySupplierID (IDDostawcy)](master-detail-filtering-across-two-pages-vb/_static/image39.png)](master-detail-filtering-across-two-pages-vb/_static/image38.png)

**Ilustracja 14**. czy element ObjectDataSource wywołuje metodę `GetSupplierBySupplierID(supplierID)` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](master-detail-filtering-across-two-pages-vb/_static/image40.png))

Tak jak w przypadku `ProductsBySupplierDataSource`, należy mieć parametr *`supplierID`* przypisany do wartości `SupplierID` QueryString wartości.

[![wypełnić wartość parametru IDDostawcy z wartości QueryString IDDostawcy](master-detail-filtering-across-two-pages-vb/_static/image42.png)](master-detail-filtering-across-two-pages-vb/_static/image41.png)

**Ilustracja 15**. Wypełnij wartość parametru *`supplierID`* za pomocą wartości `SupplierID` QueryString ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-across-two-pages-vb/_static/image43.png))

Podczas wiązania FormView z elementem ObjectDataSource w widok Projekt, program Visual Studio automatycznie utworzy `ItemTemplate`, `InsertItemTemplate`i `EditItemTemplate` z formantami sieci Web etykiet i TextBox dla każdego z pól danych zwracanych przez element ObjectDataSource. Ponieważ chcemy wyświetlić informacje o dostawcach bezpłatnie, aby usunąć `InsertItemTemplate` i `EditItemTemplate`. Następnie należy edytować ItemTemplate w taki sposób, aby wyświetlał nazwę firmy dostawcy w `<h3>` elemencie oraz adres, miasto, kraj i numer telefonu poniżej nazwy firmy. Alternatywnie można ręcznie ustawić `DataSourceID` FormView i utworzyć znacznik `ItemTemplate`, jak powrócimy do samouczka "[Wyświetlanie danych za pomocą elementu ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md)".

Po edycji znaczniki deklaratywne FormView powinny wyglądać podobnie do następujących:

[!code-aspx[Main](master-detail-filtering-across-two-pages-vb/samples/sample3.aspx)]

Rysunek 16 przedstawia zrzut ekranu strony `ProductsForSupplierDetails.aspx` po uwzględnieniu informacji o dostawcy przedstawionych powyżej.

[![Lista produktów zawiera podsumowanie dotyczące dostawcy](master-detail-filtering-across-two-pages-vb/_static/image45.png)](master-detail-filtering-across-two-pages-vb/_static/image44.png)

**Ilustracja 16**. Lista produktów zawiera podsumowanie dotyczące dostawcy ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](master-detail-filtering-across-two-pages-vb/_static/image46.png))

## <a name="applying-the-final-touches-for-theproductsforsupplierdetailsaspxui"></a>Zastosowanie końcowego dotknięcia dla interfejsu użytkownika`ProductsForSupplierDetails.aspx`

Aby ulepszyć środowisko użytkownika dla tego raportu, należy wykonać kilka dodatkowych czynności na stronie `ProductsForSupplierDetails.aspx`. Obecnie jedynym sposobem, w jaki użytkownik może przejść ze strony `ProductsForSupplierDetails.aspx` z powrotem do listy dostawców, ma kliknąć przycisk Wstecz w przeglądarce. Dodajmy kontrolkę HyperLink do strony `ProductsForSupplierDetails.aspx`, która łączy się z powrotem do `SupplierListMaster.aspx`, umożliwiając użytkownikowi powrót do listy głównej.

[![dodać kontrolkę HyperLink, aby ponownie przełączyć użytkownika do SupplierListMaster. aspx](master-detail-filtering-across-two-pages-vb/_static/image48.png)](master-detail-filtering-across-two-pages-vb/_static/image47.png)

**Ilustracja 17**. Dodawanie kontrolki hiperłącze, aby wrócić do `SupplierListMaster.aspx` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-across-two-pages-vb/_static/image49.png))

Jeśli użytkownik kliknie link Wyświetl produkty dla dostawcy, który nie ma żadnych produktów, `ProductsBySupplierDataSource` element ObjectDataSource w `ProductsForSupplierDetails.aspx` nie zwróci żadnych wyników. Widok GridView powiązany z elementem ObjectDataSource nie będzie renderować żadnego znacznika, który prowadzi do pustego regionu na stronie w przeglądarce użytkownika. Aby dokładniej informować użytkownika o braku produktów skojarzonych z wybranym dostawcą, można ustawić właściwość `EmptyDataText` GridView na komunikat, który ma być wyświetlany w przypadku wystąpienia takiej sytuacji. Ta właściwość została ustawiona na "nie ma produktów dostarczonych przez tego dostawcę"

Domyślnie wszyscy dostawcy w bazie danych Northwinds zapewniają co najmniej jeden produkt. Jednak w tym samouczku ręcznie zmodyfikowano tabelę `Products`, dzięki czemu dostawca escargots Nouveaux nie jest już skojarzony z żadnymi produktami. Ilustracja 18 przedstawia stronę szczegółów escargots Nouveaux po wprowadzeniu tej zmiany.

[![użytkownicy są informowani, że dostawca nie udostępnia żadnych produktów](master-detail-filtering-across-two-pages-vb/_static/image51.png)](master-detail-filtering-across-two-pages-vb/_static/image50.png)

**Ilustracja 18**. Użytkownicy są informowani, że dostawca nie udostępnia żadnych produktów ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-across-two-pages-vb/_static/image52.png))

## <a name="summary"></a>Podsumowanie

Raporty master/detail mogą wyświetlać zarówno rekordy główne, jak i szczegółowe na jednej stronie, w wielu witrynach sieci Web, które są rozdzielane na dwie strony internetowe. W tym samouczku zawarto informacje na temat sposobu implementacji takiego raportu wzorzec/szczegóły przez posiadanie dostawców wymienionych w widoku GridView na stronie sieci Web "Master" i skojarzonych produktów wymienionych na stronie "Szczegóły". Każdy wiersz dostawcy na głównej stronie sieci Web zawiera link do strony szczegółów, która została przeniesiona wzdłuż wartości `SupplierID` wiersza. Takie linki specyficzne dla wiersza można łatwo dodać przy użyciu HyperLinkField GridView.

Na stronie szczegółów pobieranie tych produktów dla określonego dostawcy zostało osiągnięte przez wywołanie metody `GetProductsBySupplierID(supplierID)` klasy `ProductsBLL`. Wartość parametru *`supplierID`* została określona deklaratywnie przy użyciu ciągu QueryString jako źródła parametru. Przeglądamy również sposób wyświetlania szczegółów dostawcy na stronie szczegółów przy użyciu widoku FormView.

Nasz [następny samouczek](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md) jest ostatnim z nich w raportach master/detail. Zobaczmy, jak wyświetlić listę produktów w widoku GridView, gdzie każdy wiersz ma przycisk Wybierz. Kliknięcie przycisku Wybierz spowoduje wyświetlenie szczegółów tego produktu w kontrolce DetailsView na tej samej stronie.

Szczęśliwe programowanie!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Recenzent potencjalnych klientów dla tego samouczka został Hilton Giesenow. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](master-detail-filtering-with-two-dropdownlists-vb.md)
> [dalej](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md)

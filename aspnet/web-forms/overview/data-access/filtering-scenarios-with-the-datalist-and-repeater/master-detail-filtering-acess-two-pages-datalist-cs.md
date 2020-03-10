---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-acess-two-pages-datalist-cs
title: Filtrowanie wzorców/szczegółów na dwóch stronach (C#) | Microsoft Docs
author: rick-anderson
description: W tym samouczku pokazano, jak oddzielić Raport główny/szczegółowy na dwóch stronach. Na stronie "Master" jest używana kontrolka wzmacniak do renderowania listy CATEG...
ms.author: riande
ms.date: 10/30/2010
ms.assetid: 68b8c023-92fa-4df6-9563-1764e16e4b04
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-acess-two-pages-datalist-cs
msc.type: authoredcontent
ms.openlocfilehash: 545b24a66476c55aff88ac62d3a6528105fea6c0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78607080"
---
# <a name="masterdetail-filtering-across-two-pages-c"></a>Filtrowanie rekordu głównego/szczegółów na dwóch stronach (C#)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_34_CS.exe) lub [Pobierz plik PDF](master-detail-filtering-acess-two-pages-datalist-cs/_static/datatutorial34cs1.pdf)

> W tym samouczku pokazano, jak oddzielić Raport główny/szczegółowy na dwóch stronach. Na stronie "Master" jest używana kontrolka wzmacniak do renderowania listy kategorii, które po kliknięciu powodują przejście użytkownika do strony "Szczegóły", w której dwie kolumny DataList pokazują te produkty należące do wybranej kategorii.

## <a name="introduction"></a>Wprowadzenie

W [poprzednim samouczku](master-detail-filtering-with-a-dropdownlist-datalist-cs.md) przedstawiono sposób wyświetlania raportów wzorzec/szczegóły na pojedynczej stronie sieci Web przy użyciu kontrolek DropDownList, aby wyświetlić rekordy "Master" i DataList, aby wyświetlić "Szczegóły". Innym typowym wzorcem używanym w raportach master/detail jest posiadanie rekordów głównych na jednej stronie sieci Web i szczegółowych informacji na drugim. W samouczku wcześniejsze [/szczegółowe filtrowanie w dwóch stronach](../masterdetail/master-detail-filtering-across-two-pages-cs.md) zostanie sprawdzony ten wzorzec przy użyciu widoku GridView, aby wyświetlić wszystkich dostawców w systemie. Ten GridView zawiera element HyperLinkField, który jest renderowany jako link do drugiej strony, przekazując `SupplierID` w ciągu QueryString. Druga strona użyła widoku GridView, aby wyświetlić te produkty dostarczone przez wybranego dostawcę.

Takie raporty wzorzec/szczegóły dwustronicowy można również wykonywać przy użyciu kontrolek DataList i wzmacniak. Jedyną różnicą jest to, że żadna z elementów DataList i wzmacniak nie zapewnia obsługi formantu HyperLinkField. Zamiast tego należy dodać kontrolkę sieci Web hiperlinku lub zakotwiczenie elementu HTML (`<a>`) w `ItemTemplate`formantu. Właściwość `NavigateUrl` hiperłącza lub atrybut `href` zakotwiczenia można następnie dostosować przy użyciu deklaratywnych lub programistycznych metod.

W tym samouczku zapoznajemy przykład, który wyświetla listę kategorii listy punktowanej na jednej stronie przy użyciu kontrolki wzmacniak. Każdy element listy będzie zawierać nazwę i opis kategorii, z nazwą kategorii wyświetlaną jako link do drugiej strony. Kliknięcie tego linku spowoduje wyróżnienie użytkownika na drugiej stronie, gdzie element DataList będzie pokazywał te produkty należące do wybranej kategorii.

## <a name="step-1-displaying-the-categories-in-a-bulleted-list"></a>Krok 1. Wyświetlanie kategorii na liście punktowanej

Pierwszy krok tworzenia każdego raportu wzorzec/szczegół jest uruchamiany przez wyświetlenie rekordów "Master". W związku z tym pierwsze zadanie ma wyświetlać kategorie na stronie "Master". Otwórz stronę `CategoryListMaster.aspx` w folderze `DataListRepeaterFiltering`, Dodaj kontrolkę wzmacniak i z tagu inteligentnego Dodaj nowy element ObjectDataSource. Skonfiguruj nowy element ObjectDataSource, aby miał dostęp do jego danych z metody `GetCategories` klasy `CategoriesBLL` (patrz rysunek 1).

[![skonfigurować element ObjectDataSource do używania metody GetCategories klasy CategoriesBLL](master-detail-filtering-acess-two-pages-datalist-cs/_static/image2.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image1.png)

**Rysunek 1**. Konfigurowanie elementu ObjectDataSource do używania metody `GetCategories` klasy `CategoriesBLL` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-acess-two-pages-datalist-cs/_static/image3.png))

Następnie zdefiniuj szablony wzmacniania, tak aby wyświetlały nazwę kategorii i opis jako element na liście punktowanej. Jeszcze nie martw się o posiadanie łącza do strony szczegółów w poszczególnych kategoriach. Poniżej przedstawiono znaczniki deklaratywne dla elementu wzmacniak i ObjectDataSource:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample1.aspx)]

Po zakończeniu tego znacznika Poświęć chwilę na wyświetlenie postępu w przeglądarce. Jak pokazano na rysunku 2, wzmacniak renderuje jako listę punktowaną pokazującą nazwę i opis kategorii.

[![każda kategoria jest wyświetlana jako element listy punktowanej](master-detail-filtering-acess-two-pages-datalist-cs/_static/image5.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image4.png)

**Rysunek 2**. Każda kategoria jest wyświetlana jako element listy punktowanej ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](master-detail-filtering-acess-two-pages-datalist-cs/_static/image6.png))

## <a name="step-2-turning-the-category-name-into-a-link-to-the-details-page"></a>Krok 2. zmiana nazwy kategorii na link do strony szczegółów

Aby umożliwić użytkownikowi wyświetlanie informacji o szczegółach dla danej kategorii, należy dodać łącze do każdego elementu listy punktowanej, który po kliknięciu spowoduje przejście użytkownika do drugiej strony (`ProductsForCategoryDetails.aspx`). Na drugiej stronie zostaną wyświetlone produkty dla wybranej kategorii przy użyciu elementu DataList. Aby określić kategorię, której łącze zostało kliknięte, musimy przekazywać `CategoryID` klikniętej kategorii do drugiej strony za pomocą pewnego mechanizmu. Najprostszym, najbardziej prostym sposobem transferu danych skalarnych z jednej strony do innej jest za pośrednictwem ciągu QueryString, który jest opcją używaną w tym samouczku. W szczególności strona `ProductsForCategoryDetails.aspx` oczekuje, że wybrana wartość *`categoryID`* zostanie przeniesiona za pomocą pola QueryString o nazwie `CategoryID`. Na przykład, aby wyświetlić produkty dla kategorii napoje, która ma `CategoryID` 1, użytkownik zostanie odwiedzany `ProductsForCategoryDetails.aspx?CategoryID=1`.

Aby utworzyć hiperłącze dla każdego elementu listy punktowanej w Wzmacniake, musimy dodać do `ItemTemplate`formant sieci Web hiperlinków lub element zakotwiczenia HTML (`<a>`). W scenariuszach, w których hiperłącze jest wyświetlane tak samo dla każdego wiersza, każde podejście będzie wystarczające. Dla powtarzających się woli użyć elementu zakotwiczenia. Aby użyć elementu zakotwiczenia, zaktualizuj ItemTemplateka wzmacniak do:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample2.aspx)]

Należy zauważyć, że `CategoryID` można wstrzyknąć bezpośrednio w atrybucie `href` elementu zakotwiczenia. Jednak aby to zrobić, należy rozdzielić wartość atrybutu `href` ze znakami apostrofów (i zanotować cudzysłowy), ponieważ metoda `Eval` w atrybucie `href` ogranicza swój ciąg (`"CategoryID"`) ze znakami cudzysłowu. Alternatywnie można użyć kontrolki sieci Web hiperlinku:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample3.aspx)]

Zwróć uwagę na sposób, w jaki statyczna część adresu URL — `ProductsForCategoryDetails.aspx?CategoryID` — jest dołączana do wyniku `Eval("CategoryID")` bezpośrednio w składni wiązania danych przy użyciu łączenia ciągów.

Jedną z korzyści z używania kontrolki hiperłącze jest to, że można programowo uzyskać dostęp z programu obsługi zdarzeń `ItemDataBound` przez wzmacniak, jeśli jest to konieczne. Na przykład, możesz chcieć wyświetlić nazwę kategorii jako tekst, a nie jako łącze dla kategorii bez skojarzonych produktów. Takie sprawdzenie może być wykonywane programowo w obsłudze zdarzeń `ItemDataBound`. w przypadku kategorii bez skojarzonych produktów Właściwość `NavigateUrl` hiperłącza może być ustawiona na pusty ciąg, co oznacza, że określona nazwa kategorii jest renderowana jako zwykły tekst (a nie jako link). Zapoznaj się z artykułem [Formatowanie elementów DataList i wzmacniak w oparciu o dane](../displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-cs.md) , aby uzyskać więcej informacji na temat formatowania zawartości DataList i wzmacniania na podstawie logiki programowej za pośrednictwem programu obsługi zdarzeń `ItemDataBound`.

Jeśli używasz następujących elementów, możesz skorzystać z metody zakotwiczenia lub kontrolki hiperłącze na stronie. Bez względu na to, że podczas wyświetlania strony za pomocą przeglądarki każda nazwa kategorii powinna być renderowana jako link do `ProductsForCategoryDetails.aspx`, przechodząc do odpowiedniej wartości `CategoryID` (patrz rysunek 3).

[![teraz Połącz nazwy kategorii z ProductsForCategoryDetails. aspx](master-detail-filtering-acess-two-pages-datalist-cs/_static/image8.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image7.png)

**Rysunek 3**. Nazwa kategorii teraz Link do `ProductsForCategoryDetails.aspx` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](master-detail-filtering-acess-two-pages-datalist-cs/_static/image9.png))

## <a name="step-3-listing-the-products-that-belong-to-the-selected-category"></a>Krok 3. Wyświetlanie listy produktów należących do wybranej kategorii

Gdy strona `CategoryListMaster.aspx` zostanie ukończona, jesteśmy gotowi do zaimplementowania strony "Szczegóły", `ProductsForCategoryDetails.aspx`. Otwórz Tę stronę, przeciągnij element DataList z przybornika do projektanta i ustaw jego właściwość `ID` na `ProductsInCategory`. Następnie z tagu inteligentnego DataList wybierz, aby dodać nowy element ObjectDataSource do strony, nadając mu nazwę `ProductsInCategoryDataSource`. Skonfiguruj ją tak, aby wywołuje metodę `GetProductsByCategoryID(categoryID)` klasy `ProductsBLL`; Ustaw listę rozwijaną na kartach Wstawianie, aktualizowanie i usuwanie na (brak).

[![skonfigurować element ObjectDataSource do używania metody GetProductsByCategoryID (IDKategorii) klasy ProductsBLL](master-detail-filtering-acess-two-pages-datalist-cs/_static/image11.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image10.png)

**Rysunek 4**. Konfigurowanie elementu ObjectDataSource do używania metody `GetProductsByCategoryID(categoryID)` `ProductsBLL` klasy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-acess-two-pages-datalist-cs/_static/image12.png))

Ponieważ metoda `GetProductsByCategoryID(categoryID)` akceptuje parametr wejściowy ( *`categoryID`* ), Kreator wybierania źródła danych oferuje nam możliwość określenia źródła parametru. Ustaw Źródło parametru na QueryString przy użyciu `CategoryID`QueryStringField.

[![użyć IDKategorii pola QueryString jako źródła parametru](master-detail-filtering-acess-two-pages-datalist-cs/_static/image14.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image13.png)

**Rysunek 5**. Użyj pola QueryString `CategoryID` jako źródła parametru ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](master-detail-filtering-acess-two-pages-datalist-cs/_static/image15.png))

Jak widać w poprzednich samouczkach, po zakończeniu działania kreatora wybierania źródła danych program Visual Studio automatycznie tworzy `ItemTemplate` dla elementu DataList zawierającego listę poszczególnych nazw i wartości pól danych. Zastąp ten szablon, który zawiera tylko nazwę produktu, dostawcę i cenę. Ponadto ustaw właściwość `RepeatColumns` DataList na 2. Po wprowadzeniu tych zmian elementy DataList i znaczniki deklaratywne elementu ObjectDataSource powinny wyglądać podobnie do następujących:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample4.aspx)]

Aby wyświetlić tę stronę w akcji, Zacznij od strony `CategoryListMaster.aspx`. następnie kliknij link na liście Kategorie listy punktowanej. Wykonanie tej czynności spowoduje przejście do `ProductsForCategoryDetails.aspx`, przekazanie `CategoryID` przez ciąg QueryString. `ProductsInCategoryDataSource` element ObjectDataSource w `ProductsForCategoryDetails.aspx` następnie pobierze tylko te produkty dla określonej kategorii i wyświetli je w elemencie DataList, który renderuje dwa produkty na wiersz. Rysunek 6 przedstawia zrzut ekranu `ProductsForCategoryDetails.aspx` podczas wyświetlania napojów.

[![są wyświetlane napoje, dwa na wiersz](master-detail-filtering-acess-two-pages-datalist-cs/_static/image17.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image16.png)

**Ilustracja 6**. wszystkie napoje są wyświetlane, dwa na wiersz ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-acess-two-pages-datalist-cs/_static/image18.png))

## <a name="step-4-displaying-category-information-on-productsforcategorydetailsaspx"></a>Krok 4. Wyświetlanie informacji o kategorii w ProductsForCategoryDetails. aspx

Po kliknięciu przez użytkownika kategorii w `CategoryListMaster.aspx`są one brane pod `ProductsForCategoryDetails.aspx` i pokazują produkty należące do wybranej kategorii. Jednak w `ProductsForCategoryDetails.aspx` nie ma żadnych wskazówek wizualnych dotyczących wyboru kategorii. Użytkownik, który chce klikać napoje, ale przypadkowo kliknął przyprawy, nie ma żadnego wpływu na ich pomyłkę, gdy docierają do `ProductsForCategoryDetails.aspx`. Aby uniknąć tego potencjalnego problemu, można wyświetlić informacje o wybranej kategorii — jej nazwie i opisie — w górnej części strony `ProductsForCategoryDetails.aspx`.

Aby to osiągnąć, Dodaj kontrolkę FormView nad formantem wzmacniania w `ProductsForCategoryDetails.aspx`. Następnie Dodaj nowy element ObjectDataSource do strony z tagu inteligentnego FormView o nazwie `CategoryDataSource` i skonfiguruj go tak, aby korzystał z metody `GetCategoryByCategoryID(categoryID)` klasy `CategoriesBLL`.

[![dostęp do informacji o kategorii za pomocą metody GetCategoryByCategoryID (IDKategorii) klasy CategoriesBLL](master-detail-filtering-acess-two-pages-datalist-cs/_static/image20.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image19.png)

**Rysunek 7**. dostęp do informacji o kategorii za pomocą metody `GetCategoryByCategoryID(categoryID)` klasy `CategoriesBLL` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-acess-two-pages-datalist-cs/_static/image21.png))

Podobnie jak w przypadku `ProductsInCategoryDataSource` element ObjectDataSource dodany w kroku 3, Kreator konfiguracji źródła danych `CategoryDataSource`monitu o podanie źródła dla parametru wejściowego metody `GetCategoryByCategoryID(categoryID)`. Użyj dokładnie tych samych ustawień co poprzednio, ustawiając Źródło parametru na QueryString i wartość QueryStringField na `CategoryID` (zobacz rysunek 5).

Po zakończeniu działania kreatora program Visual Studio automatycznie tworzy `ItemTemplate`, `EditItemTemplate`i `InsertItemTemplate` dla widoku FormView. Ponieważ udostępniamy interfejs tylko do odczytu, możesz usunąć `EditItemTemplate` i `InsertItemTemplate`. Ponadto możesz dostosowywać `ItemTemplate`FormView. Po usunięciu zbędnych szablonów i dostosowaniu ItemTemplate, znaczniki deklaratywne FormView i ObjectDataSource powinny wyglądać podobnie do następujących:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample5.aspx)]

Rysunek 8 przedstawia zrzut ekranu podczas wyświetlania tej strony za pomocą przeglądarki.

> [!NOTE]
> Oprócz FormView Dodaliśmy także kontrolkę HyperLink nad FormView, która przeniesie użytkownika z powrotem do listy kategorii (`CategoryListMaster.aspx`). Możesz bezpłatnie umieścić ten link w innym miejscu lub całkowicie pominąć.

[Informacje o kategorii ![są teraz wyświetlane u góry strony](master-detail-filtering-acess-two-pages-datalist-cs/_static/image23.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image22.png)

**Ilustracja 8**. Informacje o kategorii są teraz wyświetlane w górnej części strony ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](master-detail-filtering-acess-two-pages-datalist-cs/_static/image24.png))

## <a name="step-5-displaying-a-message-if-no-products-belong-to-the-selected-category"></a>Krok 5. wyświetlanie komunikatu w przypadku braku produktów należących do wybranej kategorii

Na stronie `CategoryListMaster.aspx` są wyświetlane wszystkie kategorie w systemie, niezależnie od tego, czy istnieją powiązane produkty. Jeśli użytkownik kliknie kategorię bez skojarzonych produktów, element DataList w `ProductsForCategoryDetails.aspx` nie będzie renderowany, ponieważ jego źródło danych nie będzie zawierało żadnych elementów. Jak widać w poprzednich samouczkach, widok GridView zawiera właściwość `EmptyDataText`, której można użyć do określenia wiadomości tekstowej, która ma być wyświetlana w przypadku braku rekordów w źródle danych. Niestety, ani element DataList ani wzmacniak nie mają takiej właściwości.

Aby wyświetlić komunikat informujący użytkownika, że nie ma pasujących produktów dla wybranej kategorii, musimy dodać kontrolkę etykieta do strony, której Właściwość `Text` ma przypisany komunikat, który ma być wyświetlany w przypadku braku pasujących produktów. Następnie musimy programowo ustawić właściwość `Visible` w zależności od tego, czy element DataList zawiera elementy.

Aby to osiągnąć, Zacznij od dodania etykiety poniżej elementu DataList. Ustaw jej Właściwość `ID` na `NoProductsMessage` i jej Właściwość `Text` na "nie ma produktów dla wybranej kategorii..." Następnie musimy programowo ustawić właściwość `Visible` tej etykiety na podstawie tego, czy dane zostały powiązane z `ProductsInCategory` DataList. To przypisanie należy wykonać po powiązaniu danych z elementem DataList. Dla widoku GridView, DetailsView i FormView możemy utworzyć procedurę obsługi zdarzeń dla zdarzenia `DataBound` kontrolki, która jest uruchamiana po zakończeniu wiązania. Jednak ani element DataList ani wzmacniak nie mają dostępnego zdarzenia `DataBound`.

W tym konkretnym przykładzie można przypisać właściwość `Visible` etykiety w obsłudze zdarzeń `Page_Load`, ponieważ dane zostaną przypisane do elementu DataList przed zdarzeniem `Load` strony. Jednakże takie podejście nie będzie działać w ogólnym przypadku, ponieważ dane z elementu ObjectDataSource mogą być powiązane z elementem DataList później w cyklu życia strony. Na przykład, jeśli wyświetlane dane są oparte na wartości w innej kontrolce, takiej jak podczas wyświetlania raportu wzorzec/szczegóły przy użyciu DropDownList do przechowywania rekordów "Master", dane mogą nie być ponownie powiązane z formantem sieci Web danych, dopóki `PreRender` etap cyklu życia strony.

Jednym z rozwiązań, które będą działały we wszystkich przypadkach, jest przypisanie właściwości `Visible` do `False` w programie obsługi zdarzeń `ItemDataBound` (lub `ItemCreated`) elementu DataList w przypadku powiązania typu elementów `Item` lub `AlternatingItem`. W takim przypadku wiemy, że istnieje co najmniej jeden element danych w źródle danych i w związku z tym można ukryć etykietę `NoProductsMessage`. Oprócz tego programu obsługi zdarzeń potrzebujemy również obsługi zdarzeń dla zdarzenia `DataBinding` elementu DataList, w którym zostanie zainicjowana Właściwość `Visible` etykiety do `True`. Ponieważ zdarzenie `DataBinding` wyzwalane przed zdarzeniami `ItemDataBound`, właściwość `Visible` etykiety zostanie początkowo ustawiona na `True`; Jeśli jednak istnieją jakieś elementy danych, zostanie ona ustawiona na `False`. Poniższy kod implementuje tę logikę:

[!code-csharp[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample6.cs)]

Wszystkie kategorie w bazie danych Northwind są skojarzone z co najmniej jednym wyrobem. Aby przetestować tę funkcję, ręcznie dostosowano bazę danych Northwind dla tego samouczka, należy ponownie przypisać wszystkie produkty skojarzone z kategorią produktu (`CategoryID` = 7) do kategorii ryby (`CategoryID` = 8). Można to zrobić z poziomu Eksplorator serwera, wybierając pozycję nowe zapytanie i używając następującej instrukcji `UPDATE`:

[!code-sql[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample7.sql)]

Po odpowiednio zaktualizuj bazę danych Wróć do strony `CategoryListMaster.aspx` i kliknij link. Ponieważ nie istnieją już żadne produkty należące do kategorii produkcji, zobaczysz, że nie ma żadnych produktów dla wybranej kategorii... " komunikat, jak pokazano na rysunku 9.

[![komunikat jest wyświetlany, jeśli nie ma żadnych produktów należących do wybranej kategorii](master-detail-filtering-acess-two-pages-datalist-cs/_static/image26.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image25.png)

**Rysunek 9**. komunikat jest wyświetlany, jeśli nie ma żadnych produktów należących do wybranej kategorii ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-acess-two-pages-datalist-cs/_static/image27.png))

## <a name="summary"></a>Podsumowanie

Raporty master/detail mogą wyświetlać zarówno rekordy główne, jak i szczegółowe na jednej stronie, w wielu witrynach sieci Web, które są rozdzielane na dwie strony internetowe. W tym samouczku przedstawiono sposób implementacji takiego raportu wzorzec/szczegóły przez posiadanie kategorii wymienionych na liście punktowanej przy użyciu wzmacniak na stronie sieci Web "Master" i skojarzonych produktów wymienionych na stronie "Szczegóły". Każdy element listy na nadrzędnej stronie sieci Web zawiera link do strony szczegółów, która została przeniesiona wzdłuż wartości `CategoryID` wiersza.

Na stronie szczegółów pobieranie tych produktów dla określonego dostawcy zostało wykonane za pomocą metody `GetProductsByCategoryID(categoryID)` klasy `ProductsBLL`. Wartość parametru *`categoryID`* została określona deklaratywnie przy użyciu `CategoryID` wartości QueryString jako źródła parametru. Przeglądamy również sposób wyświetlania szczegółów kategorii na stronie szczegółów przy użyciu widoku FormView i sposobu wyświetlania komunikatu w przypadku braku produktów należących do wybranej kategorii.

Szczęśliwe programowanie!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla...

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Recenzenci liderzy dla tego samouczka to Zack Kowalski i Liz Shulok. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](master-detail-filtering-with-a-dropdownlist-datalist-cs.md)
> [dalej](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md)

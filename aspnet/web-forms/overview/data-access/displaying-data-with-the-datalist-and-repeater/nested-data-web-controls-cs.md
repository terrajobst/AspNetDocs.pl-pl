---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/nested-data-web-controls-cs
title: Zagnieżdżone kontrolki internetowe danychC#() | Microsoft Docs
author: rick-anderson
description: W tym samouczku dowiesz się, jak używać elementu wzmacniak zagnieżdżonego w innym Wzmacniake. Przykłady pokazują, jak wypełnić wewnętrzny wzmacniak zarówno d...
ms.author: riande
ms.date: 09/13/2006
ms.assetid: ad3cb0ec-26cf-42d7-b81b-184a34ec9f86
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/nested-data-web-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: 8ef15bebb2c29976274b0cca1d6ace434ccc55ce
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78594970"
---
# <a name="nested-data-web-controls-c"></a>Zagnieżdżone kontrolki internetowe danych (C#)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_32_CS.exe) lub [Pobierz plik PDF](nested-data-web-controls-cs/_static/datatutorial32cs1.pdf)

> W tym samouczku dowiesz się, jak używać elementu wzmacniak zagnieżdżonego w innym Wzmacniake. Przykłady pokazują, jak wypełnić wewnętrzny wzmacniak jednocześnie i programowo.

## <a name="introduction"></a>Wprowadzenie

Oprócz statycznej składni HTML i powiązania DataBinding szablony mogą również obejmować formanty sieci Web i kontrolki użytkownika. Te kontrolki sieci Web mogą mieć przypisane do nich właściwości za pośrednictwem instrukcji deklaracyjnej, wiązania z danymi lub można programistycznie uzyskiwać dostęp do odpowiednich programów obsługi zdarzeń po stronie serwera.

Osadzając kontrolki w szablonie, można dostosować i ulepszyć środowisko użytkownika. Na przykład w przypadku korzystania z używanie TemplateField w ramach samouczka [kontrolki GridView](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) dowiesz się, jak dostosować wyświetlanie widoku GridView, dodając kontrolkę Calendar w TemplateField, aby pokazać datę zatrudnienia pracownika. w obszarze [Dodawanie kontrolek weryfikacji do edycji i wstawiania interfejsów](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md) i dostosowywania samouczków dotyczących [interfejsu modyfikacji danych](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) przedstawiono sposób dostosowywania edycji i wstawiania interfejsów przez dodanie formantów sprawdzania poprawności, pól tekstowych, kontrolek DropDownList i innych kontrolek sieci Web.

Szablony mogą również zawierać inne kontrolki sieci Web danych. Oznacza to, że możemy mieć element DataList, który zawiera inną wartość DataList (lub wzmacniak lub GridView lub DetailsView itd.) w ramach swoich szablonów. Wyzwanie z takim interfejsem wiąże odpowiednie dane z wewnętrznym formantem sieci Web danych. Istnieje kilka różnych metod, od opcji deklaratywnych używających elementu ObjectDataSource do programowania.

W tym samouczku dowiesz się, jak używać elementu wzmacniak zagnieżdżonego w innym Wzmacniake. Wzmacniak zewnętrzny będzie zawierać element dla każdej kategorii w bazie danych, wyświetlając nazwę i opis kategorii. Każdy element kategorii-wzmacniak wewnętrzny będzie wyświetlał informacje dla każdego produktu należącego do tej kategorii (patrz rysunek 1) na liście punktowanej. Nasze przykłady pokazują, jak wypełnić wewnętrzny wzmacniak jednocześnie i programowo.

[![każdej kategorii wraz z jej produktami są wymienione](nested-data-web-controls-cs/_static/image2.png)](nested-data-web-controls-cs/_static/image1.png)

**Rysunek 1**. na liście są wyświetlane wszystkie kategorie wraz z produktami ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](nested-data-web-controls-cs/_static/image3.png))

## <a name="step-1-creating-the-category-listing"></a>Krok 1. Tworzenie listy kategorii

Podczas kompilowania strony korzystającej z zagnieżdżonych kontrolek sieci Web, łatwiej jest zaprojektować, utworzyć i przetestować zewnętrzną kontrolkę sieci Web, bez nawet martw się o wewnętrzną kontrolkę zagnieżdżoną. W związku z tym Zacznijmy od kroków niezbędnych do dodania do strony, która zawiera nazwę i opis każdej kategorii.

Aby rozpocząć, Otwórz stronę `NestedControls.aspx` w folderze `DataListRepeaterBasics` i Dodaj kontrolkę wzmacniak do strony, ustawiając jej Właściwość `ID` na `CategoryList`. Z tagu inteligentnego wzmacniaka wybierz, aby utworzyć nowy element ObjectDataSource o nazwie `CategoriesDataSource`.

[Nazwa ![nowego elementu ObjectDataSource CategoriesDataSource](nested-data-web-controls-cs/_static/image5.png)](nested-data-web-controls-cs/_static/image4.png)

**Rysunek 2**. nazwa nowego `CategoriesDataSource` elementu ObjectDataSource ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](nested-data-web-controls-cs/_static/image6.png))

Skonfiguruj element ObjectDataSource w taki sposób, aby pobierał swoje dane z metody `GetCategories` `CategoriesBLL` Class.

[![skonfigurować element ObjectDataSource do używania metody GetCategories klasy CategoriesBLL](nested-data-web-controls-cs/_static/image8.png)](nested-data-web-controls-cs/_static/image7.png)

**Rysunek 3**. Konfigurowanie elementu ObjectDataSource do używania metody `GetCategories` `CategoriesBLL` Class (kliknij,[Aby wyświetlić obraz w pełnym rozmiarze](nested-data-web-controls-cs/_static/image9.png))

Aby określić zawartość szablonu wzmacniaka, należy przejść do widoku źródła i ręcznie wprowadzić składnię deklaratywną. Dodaj `ItemTemplate`, który wyświetla nazwę kategorii w elemencie `<h4>` i opis kategorii s w elemencie Paragraph (`<p>`). Ponadto poinformuj każdą kategorię z regułą poziomą (`<hr>`). Po wprowadzeniu tych zmian strona powinna zawierać składnię deklaratywną dla elementu wzmacniak i ObjectDataSource, która jest podobna do następującej:

[!code-aspx[Main](nested-data-web-controls-cs/samples/sample1.aspx)]

Na rysunku 4 przedstawiono postęp wyświetlania w przeglądarce.

[![każda nazwa i opis kategorii s są wyświetlane według reguły poziomej](nested-data-web-controls-cs/_static/image11.png)](nested-data-web-controls-cs/_static/image10.png)

**Ilustracja 4**. nazwa i opis kategorii s są wyświetlane według reguły poziomej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](nested-data-web-controls-cs/_static/image12.png))

## <a name="step-2-adding-the-nested-product-repeater"></a>Krok 2. Dodawanie zagnieżdżonego wzmacniaka produktu

Po zakończeniu tworzenia listy kategorii następnym zadaniem jest dodanie wzmacniak do `CategoryList` s `ItemTemplate`, który wyświetla informacje o tych produktach należących do odpowiedniej kategorii. Istnieje kilka sposobów, dla których możemy pobrać dane dla tego dwukierunkowego wzmacniaka, z którego korzystamy wkrótce. Na razie pozwól, aby po prostu utworzyć Wzmacniakę produktów w ramach `ItemTemplate``CategoryList`. W szczególności poinformuj, że każdy produkt na liście punktowanej zawiera wzmacniak z każdym elementem listy, w tym nazwę produktu i cenę.

Aby utworzyć ten wzmacniak, należy ręcznie wprowadzić wewnętrzną składnię i szablony deklaracyjnego wzmacniania w `CategoryList` s `ItemTemplate`. Dodaj następującą adiustację w `ItemTemplate``CategoryList` wzmacniak:

[!code-aspx[Main](nested-data-web-controls-cs/samples/sample2.aspx)]

## <a name="step-3-binding-the-category-specific-products-to-the-productsbycategorylist-repeater"></a>Krok 3. powiązanie produktów specyficznych dla kategorii z ProductsByCategoryList

Jeśli w tym momencie odwiedzasz stronę za pomocą przeglądarki, Twój ekran będzie wyglądać tak samo jak na rysunku 4, ponieważ jeszcze tu powiążemy wszelkie dane z Wzmacniaką. Istnieje kilka sposobów, dzięki którym możemy pobrać odpowiednie rekordy produktu i powiązać je z Wzmacniaką, a bardziej wydajniej niż inne. Głównym wyzwaniem w tym miejscu jest przywrócenie odpowiednich produktów dla określonej kategorii.

Do danych, które mają być powiązane z wewnętrznym formantem wzmacniania, można uzyskać deklaratywnie dostęp za pośrednictwem elementu ObjectDataSource w `ItemTemplate``CategoryList`ego, lub programowo, na stronie ASP.NET strony z kodem. Podobnie te dane mogą być powiązane z wewnętrznym Wzmacniakem, w sposób deklaratywny przez wewnętrzną właściwość "Wzmacniake" `DataSourceID` lub za pomocą składniowej funkcji DataBinding lub programowo poprzez odwołanie do wewnętrznego wzmacniak w obsłudze zdarzeń `ItemDataBound` wzmacniak `CategoryList`, programowe Ustawianie jego właściwości `DataSource` i wywoływanie jego metody `DataBind()`. Zapoznaj się z każdą z tych metod.

## <a name="accessing-the-data-declaratively-with-an-objectdatasource-control-and-theitemdataboundevent-handler"></a>Uzyskiwanie dostępu do danych w sposób deklaratywny przy użyciu kontrolki ObjectDataSource i programu obsługi zdarzeń`ItemDataBound`

Ponieważ niedawno użyto elementu ObjectDataSource w całości w tej serii samouczków, najbardziej naturalną opcją uzyskiwania dostępu do danych na potrzeby tego przykładu jest naklejenie z elementem ObjectDataSource. Klasa `ProductsBLL` ma metodę `GetProductsByCategoryID(categoryID)`, która zwraca informacje o tych produktach, które należą do określonego *`categoryID`* . W związku z tym możemy dodać element ObjectDataSource do `ItemTemplate` `CategoryList` wzmacniak i skonfigurować go do uzyskiwania dostępu do danych z tej metody klasy s.

Niestety, wzmacniak nie zezwoli na edycję jego szablonów za pomocą widok Projekt, dlatego musimy ręcznie dodać składnię deklaratywną dla tej kontrolki ObjectDataSource. Poniższa składnia przedstawia `CategoryList` wzmacniak `ItemTemplate` po dodaniu nowego elementu ObjectDataSource (`ProductsByCategoryDataSource`):

[!code-aspx[Main](nested-data-web-controls-cs/samples/sample3.aspx)]

W przypadku korzystania z metody ObjectDataSource musimy ustawić właściwość `ProductsByCategoryList` wzmacniak `DataSourceID` do `ID` elementu ObjectDataSource (`ProductsByCategoryDataSource`). Należy również zauważyć, że nasz element ObjectDataSource ma `<asp:Parameter>` elementu, który określa *`categoryID`* wartość, która zostanie przeniesiona do metody `GetProductsByCategoryID(categoryID)`. Ale jak określamy tę wartość? W idealnym przypadku można po prostu ustawić właściwość `DefaultValue` elementu `<asp:Parameter>` przy użyciu składni DataBinding, na przykład:

[!code-aspx[Main](nested-data-web-controls-cs/samples/sample4.aspx)]

Niestety, składnia DataBinding jest prawidłowa tylko w kontrolkach, które mają zdarzenie `DataBinding`. Klasa `Parameter` nie ma takiego zdarzenia, w związku z czym powyższa składnia jest niedozwolona i spowoduje błąd w czasie wykonywania.

Aby ustawić tę wartość, musimy utworzyć procedurę obsługi zdarzeń dla zdarzenia `ItemDataBound` `CategoryList`owego. Odwołaj, że zdarzenie `ItemDataBound` wyzwalane jednokrotnie dla każdego elementu powiązanego z Wzmacniaką. W związku z tym, za każdym razem, gdy to zdarzenie jest wyzwalane przez wzmacniak zewnętrzny, możemy przypisać bieżącą wartość `CategoryID` do parametru `ProductsByCategoryDataSource` ObjectDataSource s `CategoryID`.

Utwórz procedurę obsługi zdarzeń dla zdarzenia `CategoryList` wzmacniak `ItemDataBound` za pomocą następującego kodu:

[!code-csharp[Main](nested-data-web-controls-cs/samples/sample5.cs)]

Ta procedura obsługi zdarzeń rozpoczyna się od zagwarantowania, że będziemy odprowadzić się do elementu danych, a nie nagłówka, stopki lub elementu separatora. Następnie odwołujemy się do rzeczywistego wystąpienia `CategoriesRow`, które zostało właśnie powiązane z bieżącym `RepeaterItem`. Na koniec odwołujemy się do elementu ObjectDataSource w `ItemTemplate` i przypisz jego wartość parametru `CategoryID` do `CategoryID` bieżącej `RepeaterItem`.

W przypadku tego programu obsługi zdarzeń `ProductsByCategoryList` wzmacniak w każdym `RepeaterItem` jest powiązany z tymi produktami w kategorii `RepeaterItem` s. Rysunek 5 pokazuje zrzut ekranu przedstawiający wynikowe dane wyjściowe.

[![zewnętrzny wzmacniak list każdej kategorii; Wewnętrzny element zawiera listę produktów dla tej kategorii](nested-data-web-controls-cs/_static/image14.png)](nested-data-web-controls-cs/_static/image13.png)

**Ilustracja 5**. wzmacniak zewnętrzny wymienia każdą kategorię; Wewnętrzny element zawiera listę produktów dla tej kategorii ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](nested-data-web-controls-cs/_static/image15.png))

## <a name="accessing-the-products-by-category-data-programmatically"></a>Programistyczne uzyskiwanie dostępu do produktów według kategorii

Zamiast używać elementu ObjectDataSource do pobrania produktów dla bieżącej kategorii, możemy utworzyć metodę w klasie ASP.NET strony powiązanej z kodem (lub w folderze `App_Code` lub w oddzielnym projekcie biblioteki klas), która zwraca odpowiedni zestaw produktów w przypadku przekazywania ich do `CategoryID`. Załóżmy, że mamy taką metodę w klasie z kodem związanym ze stroną ASP.NET i że została ona nazwana `GetProductsInCategory(categoryID)`. Przy użyciu tej metody można powiązać produkty z bieżącą kategorią z wewnętrznym Wzmacniakem przy użyciu następującej składni deklaracyjnej:

[!code-aspx[Main](nested-data-web-controls-cs/samples/sample6.aspx)]

Właściwość `DataSource` wzmacniak używa składni wiązania danych, aby wskazać, że jej dane pochodzą z metody `GetProductsInCategory(categoryID)`. Ponieważ `Eval("CategoryID")` zwraca wartość typu `Object`, rzutowanie obiektu na `Integer` przed przekazaniem go do metody `GetProductsInCategory(categoryID)`. Należy pamiętać, że `CategoryID` dostępnym w tym miejscu za pośrednictwem składni wiązania danych jest `CategoryID` w wzmacniake *zewnętrznym* (`CategoryList`), który jest powiązany z rekordami w tabeli `Categories`. W związku z tym wiemy, że `CategoryID` nie może być wartością `NULL` bazy danych, co oznacza, że możemy odrzucać metodę `Eval` bez sprawdzania, czy będziemy ponownie korzystać z `DBNull`.

W tym podejściu musimy utworzyć metodę `GetProductsInCategory(categoryID)` i pobrać odpowiedni zestaw produktów z uwzględnieniem podanej *`categoryID`* . Możemy to zrobić, po prostu zwracając `ProductsDataTable` zwrócone przez metodę `ProductsBLL` klasy s `GetProductsByCategoryID(categoryID)`. Niech s utworzy metodę `GetProductsInCategory(categoryID)` w klasie powiązanej z kodem dla naszej strony `NestedControls.aspx`. Należy to zrobić przy użyciu następującego kodu:

[!code-csharp[Main](nested-data-web-controls-cs/samples/sample7.cs)]

Ta metoda po prostu tworzy wystąpienie metody `ProductsBLL` i zwraca wyniki metody `GetProductsByCategoryID(categoryID)`. Należy zauważyć, że metoda musi być oznaczona `Public` lub `Protected`; Jeśli metoda jest oznaczona `Private`, nie będzie dostępna ze znaczników deklaratywnych ASP.NET strony.

Po wprowadzeniu tych zmian w celu skorzystania z tej nowej techniki Poświęć chwilę na wyświetlenie strony za pomocą przeglądarki. Dane wyjściowe powinny być identyczne z danymi wyjściowymi przy użyciu metody obsługi zdarzeń ObjectDataSource i `ItemDataBound` (patrz z powrotem do rysunku 5, aby wyświetlić zrzut ekranu).

> [!NOTE]
> Może się wydawać, że pracy utworzyć metodę `GetProductsInCategory(categoryID)` w klasie z kodem ASP.NET strony. Po wykonaniu tej metody po prostu tworzy wystąpienie klasy `ProductsBLL` i zwraca wyniki metody `GetProductsByCategoryID(categoryID)`. Dlaczego nie tylko Wywołaj tę metodę bezpośrednio ze składni wiązania danych w Wzmacniake wewnętrznym, np. `DataSource='<%# ProductsBLL.GetProductsByCategoryID((int)(Eval("CategoryID"))) %>'`? Mimo że ta składnia nie będzie współdziałać z bieżącą implementacją klasy `ProductsBLL` (ponieważ metoda `GetProductsByCategoryID(categoryID)` jest metodą wystąpienia), można zmodyfikować `ProductsBLL` w celu uwzględnienia statycznej metody `GetProductsByCategoryID(categoryID)` lub dodać do klasy statyczną metodę `Instance()`, aby zwrócić nowe wystąpienie klasy `ProductsBLL`.

Chociaż takie modyfikacje eliminują potrzebę `GetProductsInCategory(categoryID)` metody w klasie z kodem związanym ze stroną ASP.NET, metoda klasy związanej z kodem zapewnia większą elastyczność podczas pracy z pobranymi danymi, ponieważ wkrótce zobaczymy.

## <a name="retrieving-all-of-the-product-information-at-once"></a>Pobieranie wszystkich informacji o produkcie jednocześnie

Dwie metody poprzedniej zostały zbadane, aby pobrały te produkty dla bieżącej kategorii przez nadanie wywołania metody `GetProductsByCategoryID(categoryID)` klasy `ProductsBLL` (pierwsze podejście zostało wykonane przez element ObjectDataSource, drugi za pomocą metody `GetProductsInCategory(categoryID)` w klasie związanej z kodem). Za każdym razem, gdy ta metoda jest wywoływana, Warstwa logiki biznesowej wywołuje do warstwy dostępu do danych, która wysyła zapytanie do bazy danych za pomocą instrukcji SQL, która zwraca wiersze z tabeli `Products`, której pole `CategoryID` pasuje do podanego parametru wejściowego.

W systemie nie ma określonych kategorii *n* , tym podejście polega na tym, że połączenie *n* + 1 wywołuje do bazy danych jedno zapytanie bazy danych, aby uzyskać wszystkie kategorie, a następnie *N* wywołania, aby uzyskać odpowiednie produkty dla każdej kategorii. Firma Microsoft może jednak pobrać wszystkie potrzebne dane w zaledwie dwóch wywołaniach, aby pobrać wszystkie kategorie i inne, aby uzyskać wszystkie produkty. Gdy mamy wszystkie produkty, możemy odfiltrować te produkty, aby tylko produkty pasujące do bieżącego `CategoryID` były powiązane z tym Wzmacniakem wewnętrznym kategorii.

Aby zapewnić tę funkcjonalność, należy jedynie wprowadzić niewielką modyfikację metody `GetProductsInCategory(categoryID)` w naszej klasie z kodem ASP.NET Page s. Zamiast odwracania wyników metody `GetProductsByCategoryID(categoryID)` klasy `ProductsBLL`, można w pierwszej kolejności uzyskać dostęp do *wszystkich* produktów (jeśli nie były już dostępne), a następnie zwrócić tylko przefiltrowany widok produktów w oparciu o zatwierdzono `CategoryID`.

[!code-csharp[Main](nested-data-web-controls-cs/samples/sample8.cs)]

Zwróć uwagę na dodanie zmiennej poziomu strony, `allProducts`. Zawiera informacje o wszystkich produktach i jest wypełniane podczas pierwszego wywołania metody `GetProductsInCategory(categoryID)`. Po upewnieniu się, że obiekt `allProducts` został utworzony i wypełniony, Metoda filtruje wyniki tabeli DataTable, tak aby były dostępne tylko wiersze, których `CategoryID` odpowiadają określonym `CategoryID`. Takie podejście zmniejsza liczbę prób uzyskania dostępu do bazy danych z *N* + 1 do dwóch.

To ulepszenie nie wprowadza żadnych zmian do renderowanego oznakowania strony ani nie zmniejsza liczby rekordów niż inne podejście. Po prostu zmniejsza liczbę wywołań do bazy danych.

> [!NOTE]
> Jeden z nich może mieć intuicyjny powód, aby zmniejszyć liczbę dostępu do bazy danych. Jednak taka sytuacja może nie być taka sama. Jeśli masz dużą liczbę produktów, których `CategoryID` jest `NULL`, na przykład wywołanie metody `GetProducts` zwraca liczbę produktów, które nigdy nie są wyświetlane. Ponadto zwracanie wszystkich produktów może być wasteful, jeśli zostanie wyświetlony tylko podzbiór kategorii, co może być przypadekem, jeśli wdrożono stronicowanie.

Tak jak zawsze, gdy chodzi o analizowanie wydajności dwóch technik, jedyną miarą Surefire jest uruchamianie kontrolowanych testów, które są dostosowane do typowych scenariuszy przypadków aplikacji.

## <a name="summary"></a>Podsumowanie

W tym samouczku przedstawiono sposób zagnieżdżania jednej kontrolki sieci Web danych w innym, szczególnie badanie, w jaki sposób mamy zewnętrzny wzmacniak wyświetlić element dla każdej kategorii z wewnętrznym Wzmacniakem zawierającym listę produktów dla każdej kategorii na liście punktowanej. Głównym wyzwaniem w tworzeniu zagnieżdżonego interfejsu użytkownika jest uzyskiwanie dostępu do i powiązywanie prawidłowych danych w wewnętrznej kontrolce danych w sieci Web. Dostępne są różne techniki, z których każda została sprawdzona w tym samouczku. Pierwsze podejście zbadał użycie elementu ObjectDataSource w zewnętrznym formancie sieci Web `ItemTemplate`, który został powiązany z wewnętrzną kontrolką danych w sieci Web za pomocą właściwości `DataSourceID`. Druga technika uzyskała dostęp do danych za pośrednictwem metody w klasie z kodem ASP.NET Page s. Tę metodę można następnie powiązać z właściwością wewnętrznej formantu sieci Web danych `DataSource` za pomocą składni DataBinding.

Chociaż zagnieżdżony interfejs użytkownika, który został zbadany w tym samouczku, użył elementu wzmacniak zagnieżdżonego w obrębie wzmacniaka, techniki te można rozszerzyć na inne kontrolki sieci Web danych. Można zagnieżdżać wzmacniak w widoku GridView lub GridView w obrębie elementu DataList i tak dalej.

Szczęśliwe programowanie!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Recenzenci liderzy dla tego samouczka to Zack Kowalski i Liz Shulok. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](showing-multiple-records-per-row-with-the-datalist-control-cs.md)
> [dalej](displaying-data-with-the-datalist-and-repeater-controls-vb.md)

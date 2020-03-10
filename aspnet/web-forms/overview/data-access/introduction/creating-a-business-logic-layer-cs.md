---
uid: web-forms/overview/data-access/introduction/creating-a-business-logic-layer-cs
title: Tworzenie warstwy logiki biznesowej (C#) | Microsoft Docs
author: rick-anderson
description: W tym samouczku dowiesz się, jak scentralizować reguły biznesowe do warstwy logiki biznesowej (LOGIKI biznesowej), która służy jako pośrednik do wymiany danych między t...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 85554606-47cb-4e4f-9848-eed9da579056
msc.legacyurl: /web-forms/overview/data-access/introduction/creating-a-business-logic-layer-cs
msc.type: authoredcontent
ms.openlocfilehash: df96f3e7422a0537bf1b003a33fe8d71a671ac33
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78605624"
---
# <a name="creating-a-business-logic-layer-c"></a>Tworzenie warstwy logiki biznesowej (C#)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](https://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_2_CS.exe) lub [Pobierz plik PDF](creating-a-business-logic-layer-cs/_static/datatutorial02cs1.pdf)

> W tym samouczku dowiesz się, jak scentralizować reguły biznesowe do warstwy logiki biznesowej (LOGIKI biznesowej), która służy jako pośrednik do wymiany danych między warstwą prezentacji a DALą.

## <a name="introduction"></a>Wprowadzenie

Warstwa dostępu do danych (DAL) utworzona w [pierwszym samouczku](creating-a-data-access-layer-cs.md) czyści logikę dostępu do danych z logiki prezentacji. Jednak mimo że DAL czyści szczegóły dostępu do danych z warstwy prezentacji, nie wymusza żadnych reguł firmy, które mogą być stosowane. Na przykład w przypadku naszej aplikacji możemy chcieć zmodyfikować pola `CategoryID` lub `SupplierID` tabeli `Products`, gdy pole `Discontinued` jest ustawione na 1, lub możemy wymusić zasady starszeństwa, uniemożliwiając sytuacje, w których pracownik jest zarządzany przez kogoś, kto go zazatrudniał. Innym typowym scenariuszem jest autoryzacja, co może spowodować, że tylko użytkownicy w danej roli mogą usuwać produkty lub zmieniać wartość `UnitPrice`.

W tym samouczku dowiesz się, jak scentralizować te reguły biznesowe do warstwy logiki biznesowej (LOGIKI biznesowej), która służy jako pośrednik do wymiany danych między warstwą prezentacji a DALą. W świecie rzeczywistym LOGIKI biznesowej powinien być zaimplementowany jako oddzielny projekt biblioteki klas. Jednak dla tych samouczków będziemy implementować LOGIKI biznesowej jako serię klas w naszym folderze `App_Code`, aby uprościć strukturę projektu. Rysunek 1 ilustruje relacje architektury między warstwą prezentacji, LOGIKI biznesowej i DAL.

![LOGIKI biznesowej oddziela warstwę prezentacji od warstwy dostępu do danych i nakłada reguły biznesowe](creating-a-business-logic-layer-cs/_static/image1.png)

**Rysunek 1**. logiki biznesowej oddziela warstwę prezentacji od warstwy dostępu do danych i nakłada reguły biznesowe

## <a name="step-1-creating-the-bll-classes"></a>Krok 1. tworzenie klas LOGIKI biznesowej

Nasza LOGIKI biznesowej będzie złożona z czterech klas, jednej dla każdej TableAdapter w DAL; Każda z tych klas LOGIKI biznesowej będzie miała metody pobierania, wstawiania, aktualizowania i usuwania z odpowiednich TableAdapter w DAL, stosując odpowiednie reguły biznesowe.

Aby dokładniej oddzielić klasy powiązane z DAL i LOGIKI biznesowej, Utwórzmy dwa podfoldery w folderze `App_Code`, `DAL` i `BLL`. Po prostu kliknij prawym przyciskiem myszy folder `App_Code` w Eksplorator rozwiązań i wybierz pozycję Nowy folder. Po utworzeniu tych dwóch folderów Przenieś określony zestaw danych utworzony w pierwszym samouczku do podfolderu `DAL`.

Następnie utwórz cztery pliki klas LOGIKI biznesowej w podfolderze `BLL`. Aby to osiągnąć, kliknij prawym przyciskiem myszy podfolder `BLL`, wybierz polecenie Dodaj nowy element i wybierz szablon klasy. Nazwij cztery klasy `ProductsBLL`, `CategoriesBLL`, `SuppliersBLL`i `EmployeesBLL`.

![Dodaj cztery nowe klasy do folderu App_Code](creating-a-business-logic-layer-cs/_static/image2.png)

**Rysunek 2**. Dodawanie czterech nowych klas do folderu `App_Code`

Następnie Dodajmy metody do poszczególnych klas, aby po prostu otoczyć metody zdefiniowane dla TableAdapters z pierwszego samouczka. Teraz te metody przywołują się bezpośrednio do DAL; będziemy później wrócić do dodania dowolnej wymaganej logiki biznesowej.

> [!NOTE]
> Jeśli używasz programu Visual Studio w wersji Standard lub nowszej (to oznacza, że *nie* używasz programu Visual Web Developer), możesz opcjonalnie projektować klasy wizualnie przy użyciu [Projektant klas](https://msdn.microsoft.com/library/default.asp?url=/library/dv_vstechart/html/clssdsgnr.asp). Zapoznaj się z [blogiem Projektant klas](https://blogs.msdn.com/classdesigner/default.aspx) , aby uzyskać więcej informacji na temat tej nowej funkcji w programie Visual Studio.

Dla klasy `ProductsBLL` musimy dodać łącznie siedem metod:

- `GetProducts()` zwraca wszystkie produkty
- `GetProductByProductID(productID)` zwraca produkt o określonym IDENTYFIKATORze produktu
- `GetProductsByCategoryID(categoryID)` zwraca wszystkie produkty z określonej kategorii
- `GetProductsBySupplier(supplierID)` zwraca wszystkie produkty z określonego dostawcy
- `AddProduct(productName, supplierID, categoryID, quantityPerUnit, unitPrice, unitsInStock, unitsOnOrder, reorderLevel, discontinued)` wstawia nowy produkt do bazy danych przy użyciu wartości przekazywania; Zwraca wartość `ProductID` nowo wstawionego rekordu.
- `UpdateProduct(productName, supplierID, categoryID, quantityPerUnit, unitPrice, unitsInStock, unitsOnOrder, reorderLevel, discontinued, productID)` aktualizuje istniejący produkt w bazie danych przy użyciu wartości przekazywania; zwraca `true`, jeśli dokładnie jeden wiersz został zaktualizowany, `false` w przeciwnym razie
- `DeleteProduct(productID)` usuwa określony produkt z bazy danych

ProductsBLL.cs

[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample1.cs)]

Metody, które po prostu zwracają dane `GetProducts`, `GetProductByProductID`, `GetProductsByCategoryID`i `GetProductBySuppliersID` są dość proste, ponieważ po prostu wywołują się do DAL. W niektórych scenariuszach mogą istnieć reguły biznesowe, które muszą być wdrożone na tym poziomie (na przykład reguły autoryzacji oparte na aktualnie zalogowanym użytkowniku lub roli, do której należy użytkownik). po prostu te metody będą opuszczane. W przypadku tych metod LOGIKI biznesowej służy tylko jako serwer proxy, za pośrednictwem którego warstwa prezentacji uzyskuje dostęp do danych źródłowych z warstwy dostępu do danych.

Metody `AddProduct` i `UpdateProduct` przyjmują jako parametry wartości dla różnych pól produktu i dodają nowy produkt lub aktualizują odpowiednio istniejący. Ponieważ wiele kolumn tabeli `Product` może przyjmować wartości `NULL` (`CategoryID`, `SupplierID`i `UnitPrice`, aby nazwać kilka), te parametry wejściowe dla `AddProduct` i `UpdateProduct` mapowane na takie kolumny używają [typów dopuszczających wartość null](https://msdn.microsoft.com/library/1t3y8s4s(v=vs.80).aspx). Typy dopuszczające wartość null są nowe w programie .NET 2,0 i zapewniają technikę wskazującą, czy typ wartości powinien być `null`. W C# obszarze można oflagować typ wartości jako typ dopuszczający wartość null poprzez dodanie `?` po typie (na przykład `int? x;`). Aby uzyskać więcej informacji, zapoznaj się z sekcją [Typy dopuszczające wartość null](https://msdn.microsoft.com/library/1t3y8s4s.aspx) w [ C# przewodniku programowania](https://msdn.microsoft.com/library/67ef8sbd%28VS.80%29.aspx) .

Wszystkie trzy metody zwracają wartość logiczną wskazującą, czy wiersz został wstawiony, zaktualizowany lub usunięty, ponieważ operacja może nie skutkować odpowiednim wierszem. Na przykład, jeśli programista strony wywoła `DeleteProduct` przekazywanie `ProductID` dla nieistniejącego produktu, instrukcja `DELETE` wystawiona dla bazy danych nie będzie miała znaczenia i w związku z tym Metoda `DeleteProduct` zwróci `false`.

Należy pamiętać, że podczas dodawania nowego produktu lub aktualizowania istniejącej wartości pola nowe lub zmodyfikowane produkt jako listę skalarnych zamiast akceptować wystąpienie `ProductsRow`. Ta metoda została wybrana, ponieważ Klasa `ProductsRow` dziedziczy z klasy `DataRow` ADO.NET, która nie ma domyślnego konstruktora bez parametrów. Aby można było utworzyć nowe wystąpienie `ProductsRow`, należy najpierw utworzyć wystąpienie `ProductsDataTable`, a następnie wywołać jego metodę `NewProductRow()` (którą będziemy w `AddProduct`). To Nieto przede wszystkim, gdy będziemy wstawiać i aktualizować produkty przy użyciu elementu ObjectDataSource. W końcu element ObjectDataSource podejmie próbę utworzenia wystąpienia parametrów wejściowych. Jeśli metoda LOGIKI biznesowej oczekuje wystąpienia `ProductsRow`, element ObjectDataSource podejmie próbę utworzenia jednego, ale kończy się niepowodzeniem z powodu braku domyślnego konstruktora bez parametrów. Aby uzyskać więcej informacji na temat tego problemu, zapoznaj się z poniższymi wpisami forów ASP.NET: [aktualizowaniem obiektów ObjectDataSource z zestawami danych z jednoznacznie określonymi typami](https://forums.asp.net/1098630/ShowPost.aspx)i [problemem przy użyciu elementu ObjectDataSource i jednoznacznie określonego typu](https://forums.asp.net/1048212/ShowPost.aspx).

Następnie w obu `AddProduct` i `UpdateProduct`kod tworzy wystąpienie `ProductsRow` i wypełnia je przekazaniem wartości. Podczas przypisywania wartości do DataColumns obiektu DataRow mogą wystąpić różne sprawdzenia poprawności na poziomie pola. W związku z tym ręczne wprowadzanie wartości z powrotem do obiektu DataRow pozwala upewnić się, że dane są przesyłane do metody LOGIKI biznesowej. Niestety klasy DataRow o jednoznacznie określonym typie wygenerowane przez program Visual Studio nie używają typów dopuszczających wartość null. Zamiast tego, aby wskazać, że określona kolumna danych w elemencie DataRow powinna odpowiadać wartości `NULL` Database, należy użyć metody `SetColumnNameNull()`.

W `UpdateProduct` najpierw załadujemy produkt do aktualizacji przy użyciu `GetProductByProductID(productID)`. Chociaż może się to okazać niepotrzebnym wyjazdem bazy danych, ta dodatkowa podróż będzie wartościowa w przyszłych samouczkach, które eksplorują współbieżność optymistyczną. Optymistyczna współbieżność to technika, która zapewnia, że dwóch użytkowników, którzy jednocześnie pracują nad tymi samymi danymi, nie zastąpią przypadkowo zmian. Przechwycony cały rekord ułatwia również tworzenie metod aktualizacji w LOGIKI biznesowej, które modyfikują podzestaw kolumn elementu DataRow. Podczas eksplorowania klasy `SuppliersBLL` zobaczymy przykład.

Na koniec należy zauważyć, że Klasa `ProductsBLL` ma [przypisany atrybut DataObject](https://msdn.microsoft.com/library/system.componentmodel.dataobjectattribute.aspx) (`[System.ComponentModel.DataObject]` składni bezpośrednio przed instrukcją klasy w górnej części pliku), a metody mają [atrybuty DataObjectMethodAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataobjectmethodattribute.aspx). Atrybut `DataObject` oznacza klasę jako obiekt odpowiedni dla powiązania z [kontrolką ObjectDataSource](https://msdn.microsoft.com/library/9a4kyhcx.aspx), podczas gdy `DataObjectMethodAttribute` wskazuje przeznaczenie metody. Ponieważ zobaczymy w przyszłych samouczkach, element ObjectDataSource ASP.NET 2.0 ułatwia deklaratywne uzyskiwanie dostępu do danych z klasy. Aby można było filtrować listę możliwych klas do powiązania w Kreatorze elementu ObjectDataSource, domyślnie tylko te klasy oznaczone jako `DataObjects` są wyświetlane na liście rozwijanej kreatora. Klasa `ProductsBLL` będzie działać tylko wtedy, gdy nie są to atrybuty, ale ich dodanie ułatwia pracę z kreatorem elementu ObjectDataSource.

## <a name="adding-the-other-classes"></a>Dodawanie innych klas

Po ukończeniu klasy `ProductsBLL` nadal musimy dodać klasy do pracy z kategoriami, dostawcami i pracownikami. Poświęć chwilę na utworzenie następujących klas i metod przy użyciu koncepcji z powyższego przykładu:

- **CategoriesBLL.cs**

    - `GetCategories()`
    - `GetCategoryByCategoryID(categoryID)`
- **SuppliersBLL.cs**

    - `GetSuppliers()`
    - `GetSupplierBySupplierID(supplierID)`
    - `GetSuppliersByCountry(country)`
    - `UpdateSupplierAddress(supplierID, address, city, country)`
- **EmployeesBLL.cs**

    - `GetEmployees()`
    - `GetEmployeeByEmployeeID(employeeID)`
    - `GetEmployeesByManager(managerID)`

Jedną z metod należy zauważyć, że metoda `UpdateSupplierAddress` klasy `SuppliersBLL`. Ta metoda zapewnia interfejs do aktualizowania tylko informacji o adresie dostawcy. Wewnętrznie metoda ta odczytuje w obiekcie `SupplierDataRow` dla określonego `supplierID` (przy użyciu `GetSupplierBySupplierID`), ustawia jego właściwości powiązane z adresami, a następnie wywołuje do metody `Update` `SupplierDataTable`. Metoda `UpdateSupplierAddress` jest następująca:

[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample2.cs)]

Zapoznaj się z tym artykułem Pobierz, aby dokończyć implementację klas LOGIKI biznesowej.

## <a name="step-2-accessing-the-typed-datasets-through-the-bll-classes"></a>Krok 2. Uzyskiwanie dostępu do wpisanych zestawów danych za pomocą klas LOGIKI biznesowej

W pierwszym samouczku przedstawiono przykłady pracy bezpośrednio z określonym zestawem danych programowo, ale z dodaniem naszych klas LOGIKI biznesowej, warstwa prezentacji powinna działać na LOGIKI biznesowej. W `AllProducts.aspx` przykładzie z pierwszego samouczka `ProductsTableAdapter` został użyty do powiązania listy produktów z elementem GridView, jak pokazano w poniższym kodzie:

[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample3.cs)]

Aby korzystać z nowych klas LOGIKI biznesowej, wszystkie te, które muszą zostać zmienione, to pierwszy wiersz kodu po prostu Zastąp `ProductsTableAdapter` obiekt obiektem `ProductBLL`:

[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample4.cs)]

Klasy LOGIKI biznesowej można również uzyskać deklaratywnie (jako zestaw danych) za pomocą elementu ObjectDataSource. Więcej szczegółów znajduje się w poniższych samouczkach dotyczących programu ObjectDataSource.

[![Lista produktów zostanie wyświetlona w widoku GridView](creating-a-business-logic-layer-cs/_static/image4.png)](creating-a-business-logic-layer-cs/_static/image3.png)

**Rysunek 3**. Lista produktów jest wyświetlana w widoku GridView ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-a-business-logic-layer-cs/_static/image5.png))

## <a name="step-3-adding-field-level-validation-to-the-datarow-classes"></a>Krok 3. Dodawanie walidacji na poziomie pola do klas DataRow

Walidacja na poziomie pola jest sprawdzana względem wartości właściwości obiektów biznesowych podczas wstawiania lub aktualizowania. Niektóre reguły walidacji na poziomie pola dla produktów obejmują:

- Pole `ProductName` nie może zawierać więcej niż 40 znaków
- Długość pola `QuantityPerUnit` nie może przekraczać 20 znaków
- Pola `ProductID`, `ProductName`i `Discontinued` są wymagane, ale wszystkie inne pola są opcjonalne
- Pola `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`i `ReorderLevel` nie mogą być równe zeru ani mniejsze.

Te reguły mogą i powinny być wyrażone na poziomie bazy danych. Limit znaków w polach `ProductName` i `QuantityPerUnit` jest przechwytywany przez typy danych tych kolumn w tabeli `Products` (odpowiednio`nvarchar(40)` i `nvarchar(20)`). Czy pola są wymagane, a opcjonalne są wyrażane przez, jeśli kolumna tabeli bazy danych zezwala `NULL` s. Istnieją cztery [ograniczenia check](https://msdn.microsoft.com/library/ms188258.aspx) , które zapewniają, że tylko wartości większe niż lub równe zero mogą wprowadzać je do kolumn `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`lub `ReorderLevel`.

Oprócz wymuszania tych reguł w bazie danych należy również wymusić na poziomie zestawu danych. W rzeczywistości długość pola oraz informacja o tym, czy wartość jest wymagana, czy opcjonalnie, są już przechwytywane dla zestawu danych DataColumns. Aby zobaczyć istniejące automatyczne sprawdzanie poprawności na poziomie pola, przejdź do projektanta obiektów DataSet, wybierz pole z jednej z tabel danych, a następnie przejdź do okno Właściwości. Jak pokazano na rysunku 4, kolumna `QuantityPerUnit` DataColumn w `ProductsDataTable` ma maksymalną długość 20 znaków i zezwala na `NULL` wartości. Jeśli podjęto próbę ustawienia właściwości `QuantityPerUnit` `ProductsDataRow`na wartość ciągu dłuższą niż 20 znaków, zostanie zgłoszony `ArgumentException`.

[![kolumna DataColumn zapewnia podstawowe sprawdzanie poprawności na poziomie pola](creating-a-business-logic-layer-cs/_static/image7.png)](creating-a-business-logic-layer-cs/_static/image6.png)

**Rysunek 4**. kolumna DataColumn zawiera podstawowe sprawdzanie poprawności na poziomie pola ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-a-business-logic-layer-cs/_static/image8.png))

Niestety, nie możemy określić kontroli granic, takich jak wartość `UnitPrice` musi być większa lub równa zero, przez okno Właściwości. Aby zapewnić ten typ walidacji na poziomie pola, należy utworzyć procedurę obsługi zdarzeń dla zdarzenia [ColumnChanging](https://msdn.microsoft.com/library/system.data.datatable.columnchanging%28VS.80%29.aspx) tabeli DataTable. Jak wspomniano w [poprzednim samouczku](creating-a-data-access-layer-cs.md), elementy DataSet, DataTables i DataRow utworzone przez określony zestaw danych można rozszerzyć za pomocą klas częściowych. Za pomocą tej techniki możemy utworzyć procedurę obsługi zdarzeń `ColumnChanging` dla klasy `ProductsDataTable`. Zacznij od utworzenia klasy w folderze `App_Code` o nazwie `ProductsDataTable.ColumnChanging.cs`.

[![dodać nową klasę do folderu App_Code](creating-a-business-logic-layer-cs/_static/image10.png)](creating-a-business-logic-layer-cs/_static/image9.png)

**Rysunek 5**. Dodawanie nowej klasy do folderu `App_Code` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-a-business-logic-layer-cs/_static/image11.png))

Następnie Utwórz procedurę obsługi zdarzeń dla zdarzenia `ColumnChanging`, która zapewnia, że wartości kolumn `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`i `ReorderLevel` (jeśli nie `NULL`) są większe lub równe zero. Jeśli jakakolwiek taka kolumna jest poza zakresem, zgłoś `ArgumentException`.

ProductsDataTable.ColumnChanging.cs

[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample5.cs)]

## <a name="step-4-adding-custom-business-rules-to-the-blls-classes"></a>Krok 4. Dodawanie niestandardowych reguł biznesowych do klas LOGIKI biznesowej

Oprócz walidacji na poziomie pola mogą istnieć niestandardowe reguły biznesowe, które obejmują różne jednostki lub koncepcje, które nie można wyrazić elementu na poziomie pojedynczej kolumny, takie jak:

- W przypadku wycofania produktu nie można zaktualizować jego `UnitPrice`
- Kraj pobytu pracownika musi być taki sam jak kraj zamieszkania swojego kierownika
- Produkt nie może zostać wycofany, jeśli jest jedynym produktem dostarczonym przez dostawcę

Klasy LOGIKI biznesowej powinny zawierać kontrole w celu zapewnienia zgodności z regułami biznesowymi aplikacji. Te sprawdzenia można dodać bezpośrednio do metod, do których mają zastosowanie.

Załóżmy, że nasze reguły biznesowe określają, że produkt nie może zostać oznaczony jako nieobsługiwany, jeśli był to jedyny produkt od danego dostawcy. Oznacza to, że jeśli produkt *X* był jedynym produktem zakupionego od dostawcy *Y*, nie można oznaczyć *X* jako nieprzerwanego; Jeśli jednak dostawca *Y* podał nam trzy produkty, *a*, *B*i *C*, możemy oznaczyć wszystkie te i wszystkie z nich jako nieobsługiwane. Nieparzysta reguła biznesowa, ale reguły biznesowe i wspólny sens nie zawsze są wyrównane!

Aby wymusić tę regułę biznesową w `UpdateProducts` metodzie, należy sprawdzić, czy `Discontinued` została ustawiona na `true` i, jeśli tak, będziemy dzwonić w `GetProductsBySupplierID`, aby określić liczbę produktów zakupionych od dostawcy tego produktu. W przypadku zakupu tylko jednego produktu od tego dostawcy zgłaszamy `ApplicationException`.

[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample6.cs)]

## <a name="responding-to-validation-errors-in-the-presentation-tier"></a>Reagowanie na błędy walidacji w warstwie prezentacji

Podczas wywoływania LOGIKI biznesowej z poziomu warstwy prezentacji możemy zdecydować, czy próbować obsługiwać wszelkie wyjątki, które mogą zostać zgłoszone, lub pozwolić im na wystąpienie bąbelków w ASP.NET (które spowodują wygenerowanie zdarzenia `Error` `HttpApplication`). Aby obsłużyć wyjątek podczas programistycznego pracy z LOGIKI biznesowej, możemy użyć [try... Catch](https://msdn.microsoft.com/library/0yd65esw.aspx) , jak pokazano na poniższym przykładzie:

[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample7.cs)]

Jak zobaczymy w przyszłych samouczkach, obsługa wyjątków, które są zaznaczane na podstawie LOGIKI biznesowej przy użyciu kontrolki sieci Web danych do wstawiania, aktualizowania lub usuwania danych, może być obsłużona bezpośrednio w programie obsługi zdarzeń, zamiast konieczności zawijania kodu w blokach `try...catch`.

## <a name="summary"></a>Podsumowanie

Dobrze zaprojektowana aplikacja jest przygotowana do odrębnych warstw, z których każdy hermetyzuje określoną rolę. W pierwszym samouczku w tej serii artykułu została utworzona Warstwa dostępu do danych za pomocą zestawów DataSet z określonym typem; w tym samouczku utworzyliśmy warstwę logiki biznesowej jako serię klas w folderze `App_Code` aplikacji, który jest wywoływany w naszym DAL. LOGIKI biznesowej implementuje logikę na poziomie pola i na poziomie firmy dla naszej aplikacji. Oprócz tworzenia oddzielnych LOGIKI biznesowej, tak jak w tym samouczku, kolejną opcją jest poszerzenie metod TableAdapters "za pomocą klas częściowych. Jednak użycie tej techniki nie pozwala na zastąpienie istniejących metod ani oddzielenie naszych DAL i naszych LOGIKI biznesowej jako czystych, jak podejście podjęte w tym artykule.

Po ukończeniu DAL i LOGIKI biznesowej wszystko jest gotowe do rozpoczęcia pracy nad naszą warstwą prezentacji. W [następnym samouczku](master-pages-and-site-navigation-cs.md) zajmiemy się krótką prezentacją dotyczącą tematów dotyczących dostępu do danych i definiowania spójnego układu stron do użycia w całym samouczku.

Szczęśliwe programowanie!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Recenzenci liderzy dla tego samouczka to Liz Shulok, Dennis Patterson, Carlos Santos i Hilton Giesenow. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](creating-a-data-access-layer-cs.md)
> [dalej](master-pages-and-site-navigation-cs.md)

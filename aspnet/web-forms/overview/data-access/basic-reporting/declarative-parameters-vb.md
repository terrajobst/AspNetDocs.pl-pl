---
uid: web-forms/overview/data-access/basic-reporting/declarative-parameters-vb
title: Parametry deklaratywne (VB) | Microsoft Docs
author: rick-anderson
description: W tym samouczku pokazano, jak za pomocą zestawu parametrów ustawić zakodowaną wartość, aby wybrać dane do wyświetlenia w formancie DetailsView.
ms.author: riande
ms.date: 03/31/2010
ms.assetid: dc1234a3-114f-4c9a-8d25-50ca03cc8e8e
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/declarative-parameters-vb
msc.type: authoredcontent
ms.openlocfilehash: cdc42752fc78d18366af037a81fe4ebe5a1646ef
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78597140"
---
# <a name="declarative-parameters-vb"></a>Parametry deklaratywne (VB)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](https://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_5_VB.exe) lub [Pobierz plik PDF](declarative-parameters-vb/_static/datatutorial05vb1.pdf)

> W tym samouczku pokazano, jak za pomocą zestawu parametrów ustawić zakodowaną wartość, aby wybrać dane do wyświetlenia w formancie DetailsView.

## <a name="introduction"></a>Wprowadzenie

W [ostatnim samouczku](displaying-data-with-the-objectdatasource-vb.md) oglądamy wyświetlanie danych za pomocą kontrolek GridView, DetailsView i FormView powiązanych z kontrolką ObjectDataSource, która wywołała metodę `GetProducts()` z klasy `ProductsBLL`. Metoda `GetProducts()` zwraca element DataTable o jednoznacznie określonym typie, wypełniony ze wszystkimi rekordami z tabeli `Products` bazy danych Northwind. Klasa `ProductsBLL` zawiera dodatkowe metody zwracające tylko podzestawy produktów — `GetProductByProductID(productID)`, `GetProductsByCategoryID(categoryID)`i `GetProductsBySupplierID(supplierID)`. Te trzy metody oczekują parametru wejściowego wskazującego sposób filtrowania zwróconych informacji o produkcie.

Element ObjectDataSource może służyć do wywoływania metod, które oczekują parametrów wejściowych, ale w tym celu należy określić miejsce, z którego pochodzą wartości tych parametrów. Wartości parametrów mogą być zakodowane na stałe lub mogą pochodzić z różnych źródeł dynamicznych, w tym: wartości QueryString, zmienne sesji, wartość właściwości kontrolki sieci Web na stronie lub inne.

Na potrzeby tego samouczka Zacznijmy od zilustrowania sposobu używania parametru jako wartości zakodowanej. W szczególności Przyjrzyjmy się dodawaniu widoku DetailsView do strony zawierającej informacje dotyczące konkretnego produktu, a mianowicie Chef Anton Gumbo, który ma `ProductID` 5. Następnie zobaczymy, jak ustawić wartość parametru na podstawie kontrolki sieci Web. W szczególności użyjemy pola tekstowego, aby zezwolić na typ użytkownika w kraju, po którym można kliknąć przycisk, aby wyświetlić listę dostawców znajdujących się w tym kraju.

## <a name="using-a-hard-coded-parameter-value"></a>Używanie zakodowanej wartości parametru

Dla pierwszego przykładu Zacznij od dodania kontrolki DetailsView do strony `DeclarativeParams.aspx` w folderze `BasicReporting`. Z poziomu tagu inteligentnego DetailsView wybierz pozycję &lt;nowe źródło danych&gt; z listy rozwijanej i wybierz opcję dodania elementu ObjectDataSource.

[![dodać elementu ObjectDataSource do strony](declarative-parameters-vb/_static/image2.png)](declarative-parameters-vb/_static/image1.png)

**Rysunek 1**. Dodawanie elementu ObjectDataSource do strony ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](declarative-parameters-vb/_static/image3.png))

Spowoduje to automatyczne uruchomienie Kreatora wyboru źródła danych kontrolki ObjectDataSource. Wybierz klasę `ProductsBLL` z pierwszego ekranu kreatora.

[![wybrać klasy ProductsBLL](declarative-parameters-vb/_static/image5.png)](declarative-parameters-vb/_static/image4.png)

**Rysunek 2**. wybieranie klasy `ProductsBLL` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](declarative-parameters-vb/_static/image6.png))

Ponieważ chcemy wyświetlić informacje o konkretnym produkcie, chcemy użyć metody `GetProductByProductID(productID)`.

[![wybrać metodę GetProductByProductID (productID)](declarative-parameters-vb/_static/image8.png)](declarative-parameters-vb/_static/image7.png)

**Rysunek 3**. wybierz metodę `GetProductByProductID(productID)` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](declarative-parameters-vb/_static/image9.png))

Ponieważ wybrana metoda zawiera parametr, istnieje jeszcze jeden ekran dla kreatora, w którym zostanie wyświetlony monit o zdefiniowanie wartości do użycia dla parametru. Na liście po lewej stronie są wyświetlane wszystkie parametry wybranej metody. W przypadku `GetProductByProductID(productID)` istnieje tylko jeden `productID`. Po prawej stronie możemy określić wartość wybranego parametru. Lista rozwijana Źródło parametrów wylicza różne możliwe źródła dla wartości parametru. Ponieważ chcemy określić ustaloną wartość 5 dla parametru `productID`, pozostaw Źródło parametru jako brak i wprowadź wartość 5 w polu tekstowym DefaultValue.

[![ustalona wartość parametru 5 zostanie użyta dla parametru productID](declarative-parameters-vb/_static/image11.png)](declarative-parameters-vb/_static/image10.png)

**Ilustracja 4**. dla parametru `productID` zostanie użyta twarda wartość parametru 5 ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](declarative-parameters-vb/_static/image12.png))

Po zakończeniu działania Kreatora konfiguracji źródła danych, znaczniki deklaratywne kontrolki ObjectDataSource zawierają obiekt `Parameter` w kolekcji `SelectParameters` dla każdego z parametrów wejściowych oczekiwanych przez metodę zdefiniowaną we właściwości `SelectMethod`. Ponieważ metoda, której używamy w tym przykładzie, oczekuje tylko jednego parametru wejściowego, `parameterID`, w tym miejscu znajduje się tylko jeden wpis. Kolekcja `SelectParameters` może zawierać dowolną klasę, która dziedziczy z klasy `Parameter` w przestrzeni nazw `System.Web.UI.WebControls`. W przypadku zakodowanych wartości parametrów jest używana klasa bazowa `Parameter`, ale dla innych opcji źródła parametrów jest używana Klasa pochodna `Parameter`; w razie konieczności można także utworzyć własne [niestandardowe typy parametrów](http://www.leftslipper.com/ShowFaq.aspx?FaqId=11).

[!code-aspx[Main](declarative-parameters-vb/samples/sample1.aspx)]

> [!NOTE]
> Jeśli używasz na własnym komputerze, znaczniki deklaratywne widoczne w tym punkcie mogą zawierać wartości `InsertMethod`, `UpdateMethod`i `DeleteMethod` właściwości, a także `DeleteParameters`. Kreator wyboru źródła danych w elemencie ObjectDataSource automatycznie określa metody z `ProductBLL` do użycia podczas wstawiania, aktualizowania i usuwania, chyba że jawnie wyczyścisz te, ale zostaną one uwzględnione w powyższym znaczniku.

Podczas odwiedzania tej strony formant sieci Web danych wywoła metodę `Select` elementu ObjectDataSource, która wywoła metodę `GetProductByProductID(productID)` klasy `ProductsBLL` przy użyciu zakodowanej wartości 5 dla `productID` parametru wejściowego. Metoda zwróci silnie wpisaną obiekt `ProductDataTable`, który zawiera pojedynczy wiersz z informacjami o połączeniu Gumbo Anton (produkt z `ProductID` 5).

[są wyświetlane ![informacje dotyczące mieszania Gumbo Anton Chef](declarative-parameters-vb/_static/image14.png)](declarative-parameters-vb/_static/image13.png)

**Rysunek 5**. wyświetlane są informacje o kombinacji Gumbo Chef Anton ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](declarative-parameters-vb/_static/image15.png))

## <a name="setting-the-parameter-value-to-the-property-value-of-a-web-control"></a>Ustawianie wartości parametru na wartość właściwości kontrolki sieci Web

Wartości parametrów elementu ObjectDataSource można również ustawić na podstawie wartości kontrolki sieci Web na stronie. Aby to zilustrować, przyjrzyjmy się w widoku GridView, który zawiera listę wszystkich dostawców znajdujących się w kraju określonym przez użytkownika. Aby to zrobić, dodając pole tekstowe do strony, w której użytkownik może wprowadzić nazwę kraju. Ustaw właściwość `ID` formantu TextBox na `CountryName`. Dodaj również formant sieci Web przycisku.

[![dodać pola tekstowego do strony z IDENTYFIKATORem CountryName](declarative-parameters-vb/_static/image17.png)](declarative-parameters-vb/_static/image16.png)

**Ilustracja 6**. Dodawanie pola tekstowego do strony z `ID` `CountryName` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](declarative-parameters-vb/_static/image18.png))

Następnie Dodaj widok GridView do strony i, z tagu inteligentnego, wybierz opcję dodania nowego elementu ObjectDataSource. Ponieważ chcemy wyświetlić informacje o dostawcach, wybierz klasę `SuppliersBLL` z pierwszego ekranu kreatora. Na drugim ekranie Wybierz metodę `GetSuppliersByCountry(country)`.

[![wybrać metody GetSuppliersByCountry (kraj)](declarative-parameters-vb/_static/image20.png)](declarative-parameters-vb/_static/image19.png)

**Rysunek 7**. wybierz metodę `GetSuppliersByCountry(country)` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](declarative-parameters-vb/_static/image21.png))

Ponieważ metoda `GetSuppliersByCountry(country)` ma parametr wejściowy, Kreator ponownie zawiera końcowy ekran służący do wybierania wartości parametru. Tym razem ustaw Źródło parametru na kontrolkę. Spowoduje to wypełnienie listy rozwijanej ControlID nazwami kontrolek na stronie; Wybierz z listy formant `CountryName`. Gdy strona zostanie odwiedzana po raz pierwszy, `CountryName` pole tekstowe będzie puste, więc żadne wyniki nie są zwracane i nic nie zostanie wyświetlone. Jeśli chcesz, aby niektóre wyniki były domyślnie wyświetlane, Ustaw odpowiednio pole tekstowe DefaultValue.

[![ustawić wartości parametru CountryName](declarative-parameters-vb/_static/image23.png)](declarative-parameters-vb/_static/image22.png)

**Ilustracja 8**. Ustaw wartość parametru na wartość kontrolki `CountryName` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](declarative-parameters-vb/_static/image24.png))

Deklaratywne znaczniki elementu ObjectDataSource różnią się nieco od pierwszego przykładu, przy użyciu [parametrze ControlParameter](https://msdn.microsoft.com/library/system.web.ui.webcontrols.controlparameter.aspx) zamiast standardowego obiektu `Parameter`. `ControlParameter` ma dodatkowe właściwości do określenia `ID` kontrolki sieci Web oraz wartości właściwości, która ma być używana dla parametru (`PropertyName`). Kreator konfiguracji źródła danych był wystarczająco inteligentny, aby określić, że dla pola tekstowego prawdopodobnie zechcemy użyć właściwości `Text` dla wartości parametru. Jeśli jednak chcesz użyć innej wartości właściwości z formantu sieci Web, możesz zmienić wartość `PropertyName` w tym miejscu lub klikając łącze "Pokaż zaawansowane właściwości" w kreatorze.

[!code-aspx[Main](declarative-parameters-vb/samples/sample2.aspx)]

Podczas odwiedzin strony po raz pierwszy pole tekstowe `CountryName` jest puste. Metoda `Select` elementu ObjectDataSource jest nadal wywoływana przez element GridView, ale wartość `Nothing` jest przenoszona do metody `GetSuppliersByCountry(country)`. TableAdapter konwertuje `Nothing` do bazy danych `NULL` wartość (`DBNull.Value`), ale zapytanie używane przez metodę `GetSuppliersByCountry(country)` jest zapisywana w taki sposób, że nie zwraca żadnych wartości, gdy wartość `NULL` zostanie określona dla parametru `@CategoryID`. W skrócie nie są zwracane żadne dostawcy.

Gdy odwiedzający wprowadzi w danym kraju, a następnie kliknie przycisk Pokaż dostawców, aby spowodować odświeżenie, Metoda `Select` elementu ObjectDataSource zostanie ponownie zbadana, przekazując wartość `Text` kontrolki TextBox jako parametr `country`.

[![dostawców z Kanady są pokazani](declarative-parameters-vb/_static/image26.png)](declarative-parameters-vb/_static/image25.png)

**Rysunek 9**. pokazywane są te dostawcy z Kanady ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](declarative-parameters-vb/_static/image27.png))

## <a name="showing-all-suppliers-by-default"></a>Domyślnie pokazywanie wszystkich dostawców

Zamiast wyświetlać żadnego z dostawców podczas pierwszego wyświetlania strony możemy chcieć wyświetlić *wszystkich* dostawców na początku, umożliwiając użytkownikowi dostosowanie listy, wprowadzając nazwę kraju w polu tekstowym. Gdy pole tekstowe jest puste, Metoda `GetSuppliersByCountry(country)` klasy `SuppliersBLL` jest przenoszona `Nothing` dla *`country`* parametru wejściowego. Ta `Nothing` wartość jest następnie przenoszona do metody `GetSupplierByCountry(country)`owej DAL, w której zostanie przetłumaczona na wartość `NULL` bazy danych dla parametru `@Country` w następującej kwerendzie:

[!code-sql[Main](declarative-parameters-vb/samples/sample3.sql)]

Wyrażenie `Country = NULL` zawsze zwraca wartość false, nawet w przypadku rekordów, których kolumna `Country` ma wartość `NULL`; w związku z tym nie są zwracane żadne rekordy.

Aby zwrócić *wszystkich* dostawców, gdy pole tekstowe kraj jest puste, można rozszerzyć metodę `GetSuppliersByCountry(country)` w logiki biznesowej, aby wywołać metodę `GetSuppliers()`, gdy jej parametr country jest `Nothing` i wywołać metodę `GetSuppliersByCountry(country)` dal w inny sposób. Będzie to miało wpływ na zwracanie wszystkich dostawców, gdy nie określono żadnego kraju i odpowiedni podzbiór dostawców, gdy parametr Country jest uwzględniony.

Zmień metodę `GetSuppliersByCountry(country)` w klasie `SuppliersBLL` na następujące:

[!code-vb[Main](declarative-parameters-vb/samples/sample4.vb)]

Dzięki tej zmianie na stronie `DeclarativeParams.aspx` są wyświetlane wszyscy dostawcy podczas pierwszej wizyty (lub za każdym razem, gdy pole tekstowe `CountryName` jest puste).

[![wszyscy dostawcy są teraz domyślnie wyświetlani](declarative-parameters-vb/_static/image29.png)](declarative-parameters-vb/_static/image28.png)

**Rysunek 10**: Wszyscy dostawcy są teraz wyświetlani domyślnie ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](declarative-parameters-vb/_static/image30.png))

## <a name="summary"></a>Podsumowanie

Aby można było użyć metod z parametrami wejściowymi, musimy określić wartości parametrów w kolekcji `SelectParameters` elementu ObjectDataSource. Różne typy parametrów umożliwiają uzyskanie wartości parametru z różnych źródeł. Domyślny typ parametru używa zakodowanej wartości, ale równie łatwo (bez wiersza kodu) wartości parametrów mogą być uzyskiwane z ciągu QueryString, zmiennych sesji, plików cookie, a nawet wartości wprowadzonych przez użytkownika z formantów sieci Web na stronie.

Przykłady przedstawione w tym samouczku przedstawiają sposób używania wartości parametrów deklaratywnych. Mogą jednak wystąpić sytuacje, w których musimy używać źródła parametrów, które jest niedostępne, takiego jak bieżąca data i godzina, lub, jeśli nasza witryna korzystała z przynależności do użytkownika. Na potrzeby takich scenariuszy można ustawić wartości parametrów programowo przed wywołaniem metody obiektu bazowego przez element ObjectDataSource. Zobaczymy, jak to zrobić w [następnym samouczku](programmatically-setting-the-objectdatasource-s-parameter-values-vb.md).

Szczęśliwe programowanie!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Recenzent potencjalnych klientów dla tego samouczka został Hilton Giesenow. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](displaying-data-with-the-objectdatasource-vb.md)
> [dalej](programmatically-setting-the-objectdatasource-s-parameter-values-vb.md)

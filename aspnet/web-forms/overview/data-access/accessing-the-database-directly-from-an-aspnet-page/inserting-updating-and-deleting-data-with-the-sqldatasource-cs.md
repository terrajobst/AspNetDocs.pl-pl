---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/inserting-updating-and-deleting-data-with-the-sqldatasource-cs
title: Wstawianie, aktualizowanie i usuwanie danych za pomocą kontrolki SqlDataSource (C#) | Microsoft Docs
author: rick-anderson
description: W poprzednich samouczkach dowiesz się, jak kontrolka ObjectDataSource może wstawiać, aktualizować i usuwać dane. Kontrolka kontrolki SqlDataSource obsługuje t...
ms.author: riande
ms.date: 02/20/2007
ms.assetid: a526f0ec-779e-4a2b-a476-6604090d25ce
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/inserting-updating-and-deleting-data-with-the-sqldatasource-cs
msc.type: authoredcontent
ms.openlocfilehash: 495e0e81a67e6926e1c4fa92e29ebbda747cd418
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74610534"
---
# <a name="inserting-updating-and-deleting-data-with-the-sqldatasource-c"></a>Wstawianie, aktualizowanie i usuwanie danych przy użyciu kontrolki SqlDataSource (C#)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_49_CS.exe) lub [Pobierz plik PDF](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/datatutorial49cs1.pdf)

> W poprzednich samouczkach dowiesz się, jak kontrolka ObjectDataSource może wstawiać, aktualizować i usuwać dane. Kontrolka kontrolki SqlDataSource obsługuje te same operacje, ale podejście jest inne, a w tym samouczku pokazano, jak skonfigurować kontrolki SqlDataSource do wstawiania, aktualizowania i usuwania danych.

## <a name="introduction"></a>Wprowadzenie

Jak opisano w temacie [Omówienie wstawiania, aktualizowania i usuwania](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md), formant GridView udostępnia wbudowane funkcje aktualizowania i usuwania, natomiast formanty DetailsView i FormView obejmują Wstawianie obsługi oraz edytowanie i usuwanie funkcji. Te możliwości modyfikacji danych można podłączyć bezpośrednio do kontrolki źródła danych bez konieczności pisania wiersza kodu. [Omówienie wstawiania, aktualizowania i usuwania](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) zbadane za pomocą elementu ObjectDataSource, aby ułatwić Wstawianie, aktualizowanie i usuwanie za pomocą kontrolek GridView, DetailsView i FormView. Alternatywnie można użyć kontrolki SqlDataSource zamiast elementu ObjectDataSource.

Odwołaj się do programu, aby obsługiwać Wstawianie, aktualizowanie i usuwanie, z elementem ObjectDataSource, którego potrzebujemy, aby określić metody warstwy obiektu do wywołania, aby wykonać akcję Wstaw, zaktualizuj lub Usuń. Przy użyciu kontrolki SqlDataSource musimy podać `INSERT`, `UPDATE`i `DELETE` instrukcje SQL (lub procedury składowane) do wykonania. Jak zobaczymy w tym samouczku, te instrukcje można utworzyć ręcznie lub mogą być automatycznie generowane przez Kreatora konfiguracji źródła danych kontrolki SqlDataSource s.

> [!NOTE]
> Ponieważ już omówiono możliwości wstawiania, edytowania i usuwania formantów GridView, DetailsView i FormView, ten samouczek koncentruje się na konfigurowaniu kontrolki kontrolki SqlDataSource w celu obsługi tych operacji. Jeśli zachodzi potrzeba zaimplementowania tych funkcji w widoku GridView, DetailsView i FormView, Wróć do podręcznika edytowanie, wstawianie i usuwanie danych, rozpoczynając od [omówienia wstawiania, aktualizowania i usuwania](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md).

## <a name="step-1-specifyinginsertupdate-anddeletestatements"></a>Krok 1. Określanie`INSERT`,`UPDATE`i`DELETE`instrukcji

Jak widać w ostatnich dwóch samouczkach, aby pobrać dane z formantu kontrolki SqlDataSource, musimy ustawić dwie właściwości:

1. `ConnectionString`, która określa bazę danych, do której ma zostać wysłana kwerenda, i
2. `SelectCommand`, która określa instrukcję SQL ad hoc lub nazwę procedury składowanej do wykonania w celu zwrócenia wyników.

W przypadku `SelectCommand` wartości z parametrami wartości parametrów są określane za pośrednictwem kolekcji kontrolki SqlDataSource s `SelectParameters` i mogą zawierać zakodowane wartości, typowe wartości parametrów źródłowych (pola QueryString, zmienne sesji, wartości formantów sieci Web itd.) lub mogą być przypisywane programowo. Gdy metoda `Select()` sterowania kontrolki SqlDataSource jest wywoływana programowo lub automatycznie z kontrolki sieci Web danych, połączenie z bazą danych jest ustanawiane, wartości parametrów są przypisywane do zapytania, a polecenie jest bezczynne do bazy danych. Wyniki są następnie zwracane jako zestaw danych lub element DataReader, w zależności od wartości właściwości formantu s `DataSourceMode`.

Wraz z zaznaczaniem danych formant kontrolki SqlDataSource może służyć do wstawiania, aktualizowania i usuwania danych przez dostarczanie `INSERT`, `UPDATE`i `DELETE` instrukcji SQL w ten sam sposób. Po prostu Przypisz właściwości `InsertCommand`, `UpdateCommand`i `DeleteCommand` `INSERT`, `UPDATE`i `DELETE` instrukcje SQL do wykonania. Jeśli instrukcje mają parametry (jak zawsze są), Uwzględnij je w kolekcjach `InsertParameters`, `UpdateParameters`i `DeleteParameters`.

Po określeniu wartości `InsertCommand`, `UpdateCommand`lub `DeleteCommand`, opcja Włącz wstawianie, Włącz edytowanie lub Włącz usuwanie w odpowiednim tagu inteligentnym kontrolki sieci Web danych stanie się dostępna. Aby to zilustrować, podejmij przykładową stronę `Querying.aspx` utworzoną w [kwerendzie danych za pomocą samouczka kontrolki kontrolki SqlDataSource](querying-data-with-the-sqldatasource-control-cs.md) i uzupełnij ją w celu uwzględnienia funkcji usuwania.

Zacznij od otworzenia `InsertUpdateDelete.aspx` i `Querying.aspx` stron z folderu `SqlDataSource`. Z poziomu projektanta na stronie `Querying.aspx` wybierz kontrolki SqlDataSource i GridView z pierwszego przykładu (kontrolki `ProductsDataSource` i `GridView1`). Po wybraniu dwóch kontrolek przejdź do menu Edycja i wybierz polecenie Kopiuj (lub po prostu naciśnij klawisze CTRL + C). Następnie przejdź do projektanta `InsertUpdateDelete.aspx` i wklej w kontrolkach. Po przeniesieniu dwóch formantów do `InsertUpdateDelete.aspx`, Przetestuj stronę w przeglądarce. Powinny być widoczne wartości kolumn `ProductID`, `ProductName`i `UnitPrice` dla wszystkich rekordów w tabeli `Products` bazy danych.

[![wszystkie produkty są wymienione na liście uporządkowane według identyfikatora ProductID](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image1.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image1.png)

**Rysunek 1**. wszystkie produkty są wymienione na liście uporządkowane według `ProductID` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image2.png))

## <a name="adding-the-sqldatasource-sdeletecommandanddeleteparametersproperties"></a>Dodawanie`DeleteCommand`kontrolki SqlDataSource i`DeleteParameters`właściwości

W tym momencie mamy kontrolki SqlDataSource, która po prostu zwraca wszystkie rekordy z tabeli `Products` i GridView, który renderuje te dane. Naszym celem jest poszerzenie tego przykładu, aby umożliwić użytkownikowi usuwanie produktów za pośrednictwem widoku GridView. Aby to osiągnąć, musimy określić wartości `DeleteCommand` kontrolki SqlDataSource i `DeleteParameters` właściwości, a następnie skonfigurować widok GridView do obsługi usuwania.

Właściwości `DeleteCommand` i `DeleteParameters` można określić na kilka sposobów:

- Za pomocą składni deklaracyjnej
- Z okno Właściwości w projektancie
- Na ekranie określ niestandardową instrukcję SQL lub procedurę przechowywaną w Kreatorze konfiguracji źródła danych
- Za pośrednictwem przycisku zaawansowanego na ekranie Określanie kolumn z tabeli widok w Kreatorze konfiguracji źródła danych, który będzie w rzeczywistości automatycznie generować instrukcję SQL `DELETE` i kolekcję parametrów użytą we właściwościach `DeleteCommand` i `DeleteParameters`

Sprawdzimy, jak automatycznie utworzyć instrukcję `DELETE` utworzoną w kroku 2. Na razie użyj okno Właściwości w projektancie, Chociaż Kreator konfigurowania źródła danych lub opcja deklaracyjnej składni będzie działała prawidłowo.

W projektancie w `InsertUpdateDelete.aspx`kliknij `ProductsDataSource` kontrolki SqlDataSource, a następnie Wywołaj okno Właściwości (z menu Widok wybierz okno Właściwości lub po prostu naciśnij klawisz F4). Wybierz Właściwość DeleteQuery, która spowoduje wyświetlenie zestawu wielokropków.

![Wybierz Właściwość DeleteQuery w oknie właściwości](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image2.gif)

**Rysunek 2**. Wybieranie właściwości DeleteQuery z okna właściwości

> [!NOTE]
> Kontrolki SqlDataSource/t ma Właściwość DeleteQuery. DeleteQuery jest kombinacją właściwości `DeleteCommand` i `DeleteParameters` i są wyświetlane tylko w okno Właściwości podczas wyświetlania okna przez projektanta. Jeśli przeglądasz okno Właściwości w widoku źródła, zamiast tego znajdziesz Właściwość `DeleteCommand`.

Kliknij wielokropek we właściwości DeleteQuery, aby wyświetlić okno dialogowe Edytor parametrów i polecenie (patrz rysunek 3). W tym oknie dialogowym można określić instrukcję SQL `DELETE` i określić parametry. Wprowadź następujące zapytanie do pola tekstowego polecenia `DELETE` (ręcznie lub przy użyciu Konstruktor zapytań, jeśli wolisz):

[!code-sql[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/samples/sample1.sql)]

Następnie kliknij przycisk Odśwież parametry, aby dodać parametr `@ProductID` do listy parametrów poniżej.

[![zaznacz Właściwość DeleteQuery w oknie właściwości](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image3.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image3.png)

**Rysunek 3**. Wybierz Właściwość DeleteQuery w oknie właściwości ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image4.png))

*Nie* dostarczaj wartości dla tego parametru (pozostaw Źródło parametru brak). Po dodaniu obsługi usuwania do widoku GridView zobaczysz automatycznie tę wartość parametru, używając wartości kolekcji `DataKeys` dla wiersza, którego przycisk usuwania został kliknięty.

> [!NOTE]
> Nazwa parametru użyta w zapytaniu `DELETE` *musi* być taka sama jak nazwa `DataKeyNames` wartości w widoku GridView, DetailsView lub FormView. Oznacza to, że parametr w instrukcji `DELETE` jest celowo całkowicie o nazwie `@ProductID` (zamiast, powiedzieć, `@ID`), ponieważ nazwa kolumny klucza podstawowego w tabeli Products (i w związku z tym wartość DataKeyNames ustawionej w widoku GridView) jest `ProductID`.

Jeśli nazwa parametru i wartość `DataKeyNames` nie są zgodne, w widoku GridView nie można automatycznie przypisać parametru do wartości z kolekcji `DataKeys`.

Po wprowadzeniu informacji związanych z usuwaniem do okna dialogowego Edytor parametrów i poleceń kliknij przycisk OK i przejdź do widoku źródła, aby sprawdzić powstające znaczniki deklaratywne:

[!code-aspx[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/samples/sample2.aspx)]

Zwróć uwagę na dodanie `DeleteCommand` właściwości, a także sekcji `<DeleteParameters>` i obiektu parametru o nazwie `productID`.

## <a name="configuring-the-gridview-for-deleting"></a>Konfigurowanie widoku GridView do usuwania

Po dodaniu właściwości `DeleteCommand`, tag "GridView" w widoku inteligentnym zawiera teraz opcję Włącz usuwanie. Zaznacz to pole wyboru. Jak opisano w temacie [Omówienie wstawiania, aktualizowania i usuwania](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md), powoduje to, że element GridView dodaje CommandField z właściwością `ShowDeleteButton` ustawioną na `true`. Jak pokazano na rysunku 4, gdy strona zostanie odwiedzana za pomocą przeglądarki, zostanie dołączony przycisk Usuń. Przetestuj Tę stronę, usuwając niektóre produkty.

[![każdy wiersz GridView zawiera teraz przycisk Usuń](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image4.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image5.png)

**Ilustracja 4**. Każdy wiersz GridView zawiera teraz przycisk Usuń ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image6.png))

Po kliknięciu przycisku Usuń następuje odświeżenie, a w obszarze GridView zostanie przypisany parametr `ProductID` wartość wartości kolekcji `DataKeys` dla wiersza, którego przycisk Usuń został kliknięty, i wywołanie metody `Delete()` kontrolki SqlDataSource. Kontrolka kontrolki SqlDataSource następnie łączy się z bazą danych i wykonuje instrukcję `DELETE`. Widok GridView następnie tworzy powiązanie z kontrolki SqlDataSource, przywracając i wyświetlając bieżący zestaw produktów (które nie zawierają już rekordu, który został usunięty).

> [!NOTE]
> Ponieważ element GridView używa swojej kolekcji `DataKeys`, aby wypełnić parametry kontrolki SqlDataSource, nieważne jest, aby właściwość GridView s `DataKeyNames` została ustawiona na kolumny, które stanowią klucz podstawowy i że kontrolki SqlDataSource s `SelectCommand` zwróci te kolumny. Ponadto należy pamiętać, że nazwa parametru w kontrolki SqlDataSource s `DeleteCommand` jest ustawiona na `@ProductID`. Jeśli właściwość `DataKeyNames` nie jest ustawiona lub parametr nie ma nazwy `@ProductsID`, kliknięcie przycisku Usuń spowoduje odświeżenie, ale w rzeczywistości nie usunie żadnego rekordu.

Rysunek 5 przedstawia interakcję w formie graficznej. Zapoznaj się z artykułem [Sprawdzanie zdarzeń skojarzonych z wstawianiem, aktualizowaniem i usuwaniem](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs.md) samouczka, aby zapoznać się z bardziej szczegółowym omówieniem łańcucha zdarzeń związanych z wstawianiem, aktualizowaniem i usuwaniem danych z kontrolki sieci Web.

![Kliknięcie przycisku Usuń w widoku GridView wywołuje metodę kontrolki SqlDataSource s Delete ()](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image5.gif)

**Rysunek 5**. kliknięcie przycisku Usuń w widoku GridView wywołuje metodę `Delete()` kontrolki SqlDataSource s

## <a name="step-2-automatically-generating-theinsertupdate-anddeletestatements"></a>Krok 2. Automatyczne generowanie instrukcji`INSERT`,`UPDATE`i`DELETE`

Zgodnie z krokiem 1, `INSERT`, `UPDATE`i `DELETE` instrukcji SQL można określić za pomocą okno Właściwości lub składni deklaracyjnej sterowania. Jednak takie podejście wymaga ręcznego zapisania instrukcji SQL, które mogą być monotonouse i podatne na błędy. Na szczęście Kreator konfigurowania źródła danych udostępnia opcję automatycznego generowania instrukcji `INSERT`, `UPDATE`i `DELETE` przy użyciu kolumn Określ kolumny z tabeli.

Pozwól, aby poznać tę opcję automatycznego generowania. Dodaj element DetailsView do projektanta w `InsertUpdateDelete.aspx` i ustaw jego właściwość `ID` na `ManageProducts`. Następnie z tagu inteligentnego DetailsView s wybierz opcję utworzenia nowego źródła danych i utworzenia kontrolki SqlDataSource o nazwie `ManageProductsDataSource`.

[![utworzyć nowy kontrolki SqlDataSource o nazwie ManageProductsDataSource](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image6.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image7.png)

**Ilustracja 6**. Tworzenie nowego kontrolki SqlDataSource o nazwie `ManageProductsDataSource` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image8.png))

W Kreatorze konfiguracji źródła danych wybierz opcję Użyj `NORTHWINDConnectionString` parametry połączenia i kliknij przycisk Dalej. Na ekranie Konfiguruj instrukcję SELECT pozostaw zaznaczone pole wyboru Określ kolumny z tabeli lub widoku, a następnie wybierz tabelę `Products` z listy rozwijanej. Wybierz kolumny `ProductID`, `ProductName`, `UnitPrice`i `Discontinued` z listy pól wyboru.

[![przy użyciu tabeli Products (produkty) Zwróć kolumny ProductID, ProductName, CenaJednostkowa i uncontinued](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image7.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image9.png)

**Rysunek 7**. użycie tabeli `Products`, zwrócenie kolumn `ProductID`, `ProductName`, `UnitPrice`i `Discontinued` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image10.png))

Aby automatycznie generować instrukcje `INSERT`, `UPDATE`i `DELETE` na podstawie wybranej tabeli i kolumn, kliknij przycisk Zaawansowane i zaznacz pole wyboru Generuj `INSERT`, `UPDATE`i `DELETE` instrukcji.

![Zaznacz pole wyboru Generuj instrukcje INSERT, UPDATE i DELETE](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image8.gif)

**Ilustracja 8**. Zaznacz pole wyboru Generuj `INSERT`, `UPDATE`i `DELETE` instrukcji

Pole wyboru Generuj `INSERT`, `UPDATE`i `DELETE` będzie możliwe tylko do sprawdzenia, jeśli wybrana tabela ma klucz podstawowy, a kolumna klucza podstawowego (lub kolumny) znajduje się na liście zwracanych kolumn. Pole wyboru Użyj optymistycznej współbieżności, które jest wybierane po zaznaczeniu pola wyboru Generuj `INSERT`, `UPDATE`i `DELETE`, uzupełnia klauzule `WHERE` w wyrażeniach `UPDATE` i `DELETE` w celu zapewnienia optymistycznej kontroli współbieżności. Na razie pozostaw to pole wyboru niezaznaczone; sprawdzimy optymistyczną współbieżność z kontrolką kontrolki SqlDataSource w następnym samouczku.

Po zaznaczeniu pola wyboru Generuj `INSERT`, `UPDATE`i `DELETE`, kliknij przycisk OK, aby powrócić do ekranu Konfigurowanie instrukcji SELECT, a następnie kliknij przycisk Dalej, a następnie Zakończ, aby zakończyć pracę Kreatora konfiguracji źródła danych. Po ukończeniu działania kreatora program Visual Studio doda BoundFields do widoku DetailsView dla kolumn `ProductID`, `ProductName`i `UnitPrice` oraz CheckBoxField dla kolumny `Discontinued`. Z poziomu tagu inteligentnego DetailsView s zaznacz opcję Włącz stronicowanie, aby użytkownik odwiedzający Tę stronę mógł przejść przez produkty. Wyczyść również `Width` i właściwości `Height` DetailsView.

Należy zauważyć, że tag inteligentny ma dostępne opcje umożliwia wstawianie, Włączanie edytowania i włączania usuwania. Wynika to z faktu, że kontrolki SqlDataSource zawiera wartości dla `InsertCommand`, `UpdateCommand`i `DeleteCommand`, jak pokazano w następującej składni deklaracyjnej:

[!code-aspx[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/samples/sample3.aspx)]

Zwróć uwagę na to, w jaki sposób formant kontrolki SqlDataSource ma wartości ustawione automatycznie dla właściwości `InsertCommand`, `UpdateCommand`i `DeleteCommand`. Zestaw kolumn, do których odwołuje się `InsertCommand` i `UpdateCommand`, jest oparty na tych, które znajdują się w instrukcji `SELECT`. Oznacza to, że nie ma żadnych *kolumn produktów w* `InsertCommand` i `UpdateCommand`, istnieją tylko kolumny określone w `SelectCommand` (mniej `ProductID`, które zostały pominięte, [`IDENTITY`](http://www.sqlteam.com/item.asp?ItemID=102)ponieważ nie można zmienić wartości podczas edycji i są automatycznie przypisywane podczas wstawiania). Ponadto dla każdego parametru w `InsertCommand`, `UpdateCommand`i `DeleteCommand` właściwości są odpowiednie parametry w kolekcjach `InsertParameters`, `UpdateParameters`i `DeleteParameters`.

Aby włączyć funkcje modyfikacji danych DetailsView s, zaznacz opcje Włącz wstawianie, Włącz edytowanie i Włącz usuwanie w jego tagu inteligentnym. Spowoduje to dodanie CommandField z właściwościami `ShowInsertButton`, `ShowEditButton`i `ShowDeleteButton` ustawionymi na `true`.

Odwiedź stronę w przeglądarce i zanotuj przyciski Edytuj, Usuń i nowe zawarte w widoku DetailsView. Kliknięcie przycisku Edytuj powoduje przełączenie widoku DetailsView do trybu edycji, który wyświetla każdy BoundField, którego właściwość `ReadOnly` jest ustawiona na `false` (domyślnie) jako pole tekstowe i CheckBoxField jako pole wyboru.

[![domyślnego interfejsu edycji widoku DetailsView](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image9.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image11.png)

**Rysunek 9**: domyślny interfejs edycji widoku DetailsView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image12.png))

Analogicznie, można usunąć aktualnie wybrany produkt lub dodać nowy produkt do systemu. Ponieważ instrukcja `InsertCommand` działa tylko z kolumnami `ProductName`, `UnitPrice`i `Discontinued`, inne kolumny mają zarówno `NULL`, jak i ich domyślną wartość przypisaną przez bazę danych podczas wstawiania. Podobnie jak w przypadku elementu ObjectDataSource, jeśli `InsertCommand` brakuje żadnej kolumny tabeli bazy danych, która nie zezwala `NULL` s i nie ma wartości domyślnej, podczas próby wykonania instrukcji `INSERT` wystąpi błąd SQL.

> [!NOTE]
> Interfejsy wstawiania i edytowania widoku DetailsView nie mają żadnego sortowania ani walidacji. Aby dodać kontrolki walidacji lub dostosować interfejsy, należy skonwertować BoundFields na używanie TemplateField. Aby uzyskać więcej informacji, zapoznaj się z tematami [dodawania kontrolek weryfikacji do edycji i wstawiania interfejsów](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md) oraz dostosowywania samouczków dotyczących [interfejsu modyfikacji danych](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) .

Należy również pamiętać, że w przypadku aktualizowania i usuwania, w widoku DetailsView jest używany wartość bieżąca produkt s `DataKey`, która jest obecna tylko w przypadku skonfigurowania właściwości `DataKeyNames`. Jeśli edytowanie lub usuwanie nie ma żadnego efektu, upewnij się, że właściwość `DataKeyNames` jest ustawiona.

## <a name="limitations-of-automatically-generating-sql-statements"></a>Ograniczenia dotyczące automatycznego generowania instrukcji SQL

Ponieważ opcja Generuj `INSERT`, `UPDATE`i `DELETE` jest dostępna tylko podczas wybierania kolumn z tabeli, aby uzyskać bardziej skomplikowane zapytania, należy napisać własne instrukcje `INSERT`, `UPDATE`i `DELETE` jak w kroku 1. Często instrukcje SQL `SELECT` używają `JOIN` s do przenoszenia danych z co najmniej jednej tabeli odnośników na potrzeby wyświetlania (takie jak przywrócenie pola `CategoryName` `Categories` tabeli w przypadku wyświetlania informacji o produkcie). W tym samym czasie możemy chcieć zezwolić użytkownikowi na edytowanie, aktualizowanie lub wstawianie danych do tabeli podstawowej (`Products`w tym przypadku).

Mimo że instrukcje `INSERT`, `UPDATE`i `DELETE` mogą być wprowadzane ręcznie, należy wziąć pod uwagę następujące wskazówki dotyczące zapisywania czasu. Początkowo Skonfiguruj kontrolki SqlDataSource tak, aby ściągał dane bezpośrednio z tabeli `Products`. Użyj Kreatora konfiguracji źródła danych s Określ kolumny z ekranu tabeli lub widoku, aby można było automatycznie generować instrukcje `INSERT`, `UPDATE`i `DELETE`. Następnie po zakończeniu pracy kreatora wybierz opcję konfigurowania SelectQuery z okno Właściwości (lub, Alternatywnie, Wróć do Kreatora konfiguracji źródła danych, ale użyj opcji Określ niestandardową instrukcję SQL lub procedurę składowaną). Następnie zaktualizuj instrukcję `SELECT`, aby zawierała składnię `JOIN`. Ta technika oferuje korzyści wynikające z oszczędności czasu dla automatycznie generowanych instrukcji SQL i pozwala na bardziej dostosowane instrukcje `SELECT`.

Innym ograniczeniem automatycznego generowania instrukcji `INSERT`, `UPDATE`i `DELETE` jest to, że kolumny w instrukcjach `INSERT` i `UPDATE` są oparte na kolumnach zwracanych przez instrukcję `SELECT`. Może być jednak konieczne zaktualizowanie lub wstawienie więcej lub mniejszej liczby pól. Na przykład w przykładzie z kroku 2 może być konieczne, aby `UnitPrice` BoundField być tylko do odczytu. W takim przypadku nie powinien pojawić się w `UpdateCommand`. Możesz też chcieć ustawić wartość pola tabeli, która nie jest wyświetlana w widoku GridView. Na przykład podczas dodawania nowego rekordu możemy mieć ustawioną wartość `QuantityPerUnit`.

Jeśli takie dostosowania są wymagane, należy je wprowadzić ręcznie, przez okno Właściwości, określić niestandardową instrukcję SQL lub procedurę składowaną w Kreatorze albo za pomocą składni deklaratywnej.

> [!NOTE]
> Podczas dodawania parametrów, które nie mają odpowiednich pól w kontrolce sieci Web danych, należy pamiętać, że te wartości parametrów muszą być przypisane w określony sposób. Te wartości mogą być: stałe kodowane bezpośrednio w `InsertCommand` lub `UpdateCommand`; mogą pochodzić ze wstępnie zdefiniowanego źródła (QueryString, stan sesji, kontrolki sieci Web na stronie itd.); lub można je przypisać programowo, jak zostało to opisane w poprzednim samouczku.

## <a name="summary"></a>Podsumowanie

Aby formanty sieci Web danych mogły korzystać z wbudowanych funkcji wstawiania, edytowania i usuwania, kontrola źródła danych, z którą są powiązane, musi oferować takie funkcje. W przypadku kontrolki SqlDataSource oznacza to, że instrukcje `INSERT`, `UPDATE`i `DELETE` SQL muszą być przypisane do właściwości `InsertCommand`, `UpdateCommand`i `DeleteCommand`. Te właściwości i odpowiednie kolekcje parametrów mogą być dodawane ręcznie lub automatycznie za pomocą Kreatora konfiguracji źródła danych. W tym samouczku zbadamy obie techniki.

Sprawdzono użycie optymistycznej współbieżności z elementem ObjectDataSource w [implementacji optymistycznego samouczka współbieżności](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md) . Kontrolka kontrolki SqlDataSource zapewnia również optymistyczną obsługę współbieżności. Zgodnie z opisem w kroku 2, podczas automatycznego generowania instrukcji `INSERT`, `UPDATE`i `DELETE`, Kreator oferuje użycie optymistycznej opcji współbieżności. Jak zobaczymy w następnym samouczku, użycie optymistycznej współbieżności z kontrolki SqlDataSource modyfikuje klauzule `WHERE` w instrukcjach `UPDATE` i `DELETE`, aby upewnić się, że wartości innych kolumn nie uległy zmianie od momentu ostatniego wyświetlenia danych na stronie.

Szczęśliwe programowanie!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Poprzednie](using-parameterized-queries-with-the-sqldatasource-cs.md)
> [dalej](implementing-optimistic-concurrency-with-the-sqldatasource-cs.md)

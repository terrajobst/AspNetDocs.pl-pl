---
uid: web-forms/overview/moving-to-aspnet-20/data-source-controls
title: Kontrolki źródła danych | Microsoft Docs
author: microsoft
description: Kontrolka DataGrid w ASP.NET 1. x oznaczył doskonałe udoskonalenia dostępu do danych w aplikacjach sieci Web. Jednak nie jest to przyjazne dla użytkownika, ponieważ może to być...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 78fd0e92-f9c6-4e96-a5e9-0375b307a828
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/data-source-controls
msc.type: authoredcontent
ms.openlocfilehash: a2e2cfbec3e5aebf42a2de30bab7d45b4b610298
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78639413"
---
# <a name="data-source-controls"></a>Kontrolki źródła danych

przez [firmę Microsoft](https://github.com/microsoft)

> Kontrolka DataGrid w ASP.NET 1. x oznaczył doskonałe udoskonalenia dostępu do danych w aplikacjach sieci Web. Jednak nie jest to przyjazne dla użytkownika, ponieważ było to możliwe. Nadal wymagało dużej ilości kodu, aby uzyskać wiele przydatnych funkcji. Jest to model we wszystkich przedsięwzięciach dostępu do danych w 1. x.

Kontrolka DataGrid w ASP.NET 1. x oznaczył doskonałe udoskonalenia dostępu do danych w aplikacjach sieci Web. Jednak nie jest to przyjazne dla użytkownika, ponieważ było to możliwe. Nadal wymagało dużej ilości kodu, aby uzyskać wiele przydatnych funkcji. Jest to model we wszystkich przedsięwzięciach dostępu do danych w 1. x.

ASP.NET 2,0 dotyczy tego programu w części z kontrolkami źródła danych. Kontrolki źródła danych w ASP.NET 2,0 zapewniają deweloperom deklaratywny model służący do pobierania danych, wyświetlania danych i edytowania danych. Celem kontroli źródła danych jest zapewnienie spójnej reprezentacji danych do kontrolek powiązanych z danymi, niezależnie od źródła danych. W przypadku kontrolek źródła danych w ASP.NET 2,0 jest klasą abstrakcyjną DataSourceControl. Klasa DataSourceControl dostarcza podstawową implementację interfejsu IDataSource i interfejsu IListSource, a drugie, który umożliwia przypisanie kontroli źródła danych jako źródła danych kontrolki powiązanej z danymi (za pośrednictwem nowej właściwości DataSourceId omówione w dalszej części artykułu i uwidaczniaj dane w postaci listy. Każda lista danych z kontrolki źródła danych jest udostępniana jako obiekt elementu danych. Dostęp do wystąpień źródła danych jest udostępniany przez interfejs IDataSource. Na przykład Metoda GetViewNames zwraca nazwę ICollection, która pozwala na Wyliczenie danych o widokach skojarzonych z konkretną kontrolą źródła danych, a Metoda GetView pozwala uzyskać dostęp do konkretnego wystąpienia elementu według nazwy.

Kontrolki źródła danych nie mają interfejsu użytkownika. Są one implementowane jako kontrolki serwera, dzięki czemu mogą obsługiwać składnię deklaratywną i w razie potrzeby mają dostęp do stanu strony. Formanty źródła danych nie renderują żadnego znacznika HTML do klienta.

> [!NOTE]
> Jak zobaczysz później, można również korzystać z pamięci podręcznej uzyskanych przy użyciu kontroli źródła danych.

## <a name="storing-connection-strings"></a>Przechowywanie parametrów połączenia

Przed rozpoczęciem procesu konfigurowania kontroli źródła danych należy zakryć nową funkcję ASP.NET 2,0 dotyczącą parametrów połączenia. ASP.NET 2,0 wprowadza nową sekcję w pliku konfiguracji, która umożliwia łatwe przechowywanie parametrów połączenia, które mogą być odczytywane dynamicznie w czasie wykonywania. Sekcja &lt;connectionStrings&gt; ułatwia przechowywanie parametrów połączenia.

Poniższy fragment kodu dodaje nowe parametry połączenia.

[!code-xml[Main](data-source-controls/samples/sample1.xml)]

> [!NOTE]
> Podobnie jak w sekcji &lt;appSettings&gt; sekcja &lt;connectionStrings&gt; pojawia się poza sekcją &lt;system. Web&gt; w pliku konfiguracji.

Aby użyć tych parametrów połączenia, można użyć następującej składni podczas ustawiania atrybutu ConnectionString kontrolki serwer.

[!code-aspx[Main](data-source-controls/samples/sample2.aspx)]

Sekcję &lt;connectionStrings&gt; można również zaszyfrować, aby poufne informacje nie były ujawniane. Ta możliwość zostanie omówiona w późniejszym module.

## <a name="caching-data-sources"></a>Buforowanie źródeł danych

Każdy DataSourceControl zawiera cztery właściwości do konfigurowania buforowania; EnableCaching, CacheDuration, CacheExpirationPolicy i CacheKeyDependency.

## <a name="enablecaching"></a>EnableCaching

EnableCaching jest właściwością logiczną, która określa, czy buforowanie jest włączone dla kontroli źródła danych.

## <a name="cacheduration-property"></a>Właściwość CacheDuration

Właściwość CacheDuration ustawia liczbę sekund, przez które pozostała ważność pamięci podręcznej. Ustawienie tej właściwości na **0** powoduje, że pamięć podręczna będzie ważna, dopóki nie zostanie jawnie unieważniona.

## <a name="cacheexpirationpolicy-property"></a>CacheExpirationPolicy Property

Właściwość CacheExpirationPolicy można ustawić na wartość **bezwzględną** lub **przesuwaną**. Ustawienie wartości bezwzględnej oznacza, że maksymalny czas, w którym dane będą buforowane, to liczba sekund określona przez właściwość CacheDuration. Ustawiając je na przesuwanie, czas wygaśnięcia jest resetowany, gdy każda operacja zostanie wykonana.

## <a name="cachekeydependency-property"></a>Właściwość CacheKeyDependency

Jeśli wartość ciągu jest określona dla właściwości CacheKeyDependency, ASP.NET ustawi nową zależność pamięci podręcznej na podstawie tego ciągu. Dzięki temu można jawnie unieważnić pamięć podręczną przez zmianę lub usunięcie CacheKeyDependency.

**Ważne**: Jeśli Personifikacja jest włączona i dostęp do źródła danych i/lub zawartości danych opiera się na tożsamości klienta, zaleca się, aby buforowanie było wyłączone przez ustawienie EnableCaching na false. Jeśli buforowanie jest włączone w tym scenariuszu, a użytkownik inny niż użytkownik, który pierwotnie zażądał danych wystawia żądanie, autoryzacja do źródła danych nie zostanie wymuszona. Dane będą po prostu obsługiwane z pamięci podręcznej.

## <a name="the-sqldatasource-control"></a>Kontrolka kontrolki SqlDataSource

Formant kontrolki SqlDataSource umożliwia deweloperowi dostęp do danych przechowywanych w dowolnej relacyjnej bazie danych, która obsługuje ADO.NET. Za pomocą dostawcy system. Data. SqlClient można uzyskać dostęp do bazy danych SQL Server, dostawcy system. Data. OleDb, dostawcy system. Data. ODBC lub dostawcy system. Data. OracleClient w celu uzyskania dostępu do programu Oracle. W związku z tym kontrolki SqlDataSource jest na pewno używany do uzyskiwania dostępu do danych w bazie danych SQL Server.

Aby użyć kontrolki SqlDataSource, wystarczy podać wartość właściwości ConnectionString i określić polecenie SQL lub procedurę składowaną. Kontrolka kontrolki SqlDataSource zajmuje się pracą z podstawową architekturą ADO.NET. Spowoduje to otwarcie połączenia, wysłanie zapytania do źródła danych lub wykonanie procedury składowanej, zwrócenie danych, a następnie zamknięcie połączenia.

> [!NOTE]
> Ponieważ Klasa DataSourceControl automatycznie zamyka połączenie, powinno zmniejszyć liczbę wywołań klientów generowanych przez przeciek połączeń z bazą danych.

Poniższy fragment kodu wiąże formant DropDownList z kontrolką kontrolki SqlDataSource przy użyciu parametrów połączenia, które są przechowywane w pliku konfiguracji, jak pokazano powyżej.

[!code-aspx[Main](data-source-controls/samples/sample3.aspx)]

Jak pokazano powyżej, właściwość DataSourceMode elementu kontrolki SqlDataSource określa tryb dla źródła danych. W powyższym przykładzie element DataSourceMode ma wartość DataReader. W takim przypadku kontrolki SqlDataSource zwraca obiekt IDataReader przy użyciu kursora tylko do odczytu i dla elementu Read-Only. Określony typ zwracanego obiektu jest kontrolowany przez używanego dostawcę. W tym przypadku jest używany dostawca system. Data. SqlClient określony w sekcji &lt;connectionStrings&gt; pliku Web. config. W związku z tym zwracany obiekt będzie typem SqlDataReader. Określając wartość DataSourceMode zestawu danych, dane mogą być przechowywane w zestawie danych na serwerze. Ten tryb pozwala dodawać funkcje, takie jak sortowanie, stronicowanie itp. Jeśli został powiązany z danymi kontrolki SqlDataSource do kontrolki GridView, wybieram tryb zestawu danych. Jednak w przypadku DropDownList tryb elementu DataReader jest poprawnym wyborem.

> [!NOTE]
> W przypadku buforowania elementu kontrolki SqlDataSource lub elementu AccessDataSource Właściwość DataSourceMode musi być ustawiona na DataSet. Wystąpi wyjątek, jeśli włączysz buforowanie z elementem DataSourceMode elementu DataReader.

## <a name="sqldatasource-properties"></a>Właściwości kontrolki SqlDataSource

Poniżej wymieniono niektóre właściwości kontrolki kontrolki SqlDataSource.

### <a name="cancelselectonnullparameter"></a>CancelSelectOnNullParameter

Wartość logiczna określająca, czy polecenie select zostało anulowane, jeśli jeden z parametrów ma wartość null. Domyślnie wartość true.

### <a name="conflictdetection"></a>ConflictDetection

W sytuacji, gdy wielu użytkowników może aktualizować źródło danych w tym samym czasie, właściwość ConflictDetection określa zachowanie kontrolki kontrolki SqlDataSource. Ta właściwość zwraca jedną z wartości wyliczenia ConflictOptions. Te wartości to **CompareAllValues** i **OverwriteChanges**. W przypadku wybrania opcji OverwriteChanges Ostatnia osoba zapisu danych w źródle danych zastępuje wszystkie poprzednie zmiany. Jeśli jednak Właściwość ConflictDetection ma wartość CompareAllValues, parametry są tworzone dla kolumn zwracanych przez właściwość SelectCommand i parametry są również tworzone w celu przechowywania oryginalnych wartości w każdej z tych kolumn, co umożliwia kontrolki SqlDataSource Określ, czy wartości zostały zmienione od czasu wykonania elementu SelectCommand.

### <a name="deletecommand"></a>DeleteCommand

Ustawia lub pobiera ciąg SQL używany podczas usuwania wierszy z bazy danych. Może to być zapytanie SQL lub nazwa procedury składowanej.

### <a name="deletecommandtype"></a>DeleteCommandtype

Ustawia lub pobiera typ polecenia Delete, kwerendę SQL (tekst) lub procedurę przechowywaną (StoredProcedure).

### <a name="deleteparameters"></a>DeleteParameters

Zwraca parametry, które są używane przez DeleteCommand obiektu SqlDataSourceView skojarzonego z kontrolką kontrolki SqlDataSource.

### <a name="oldvaluesparameterformatstring"></a>OldValuesParameterFormatString

Ta właściwość służy do określania formatu oryginalnych parametrów wartości w przypadkach, gdy właściwość ConflictDetection jest ustawiona na CompareAllValues. Wartość domyślna to {0}, co oznacza, że oryginalne parametry wartości będą mieć taką samą nazwę jak oryginalny parametr. Innymi słowy, jeśli nazwa pola to IDPracownika, parametr pierwotnej wartości zostałby @EmployeeID.

### <a name="selectcommand"></a>Właściwość

Ustawia lub pobiera ciąg SQL, który jest używany do pobierania danych z bazy danych. Może to być zapytanie SQL lub nazwa procedury składowanej.

### <a name="selectcommandtype"></a>Właściwość SelectCommandtype

Ustawia lub pobiera typ polecenia SELECT, kwerendę SQL (tekst) lub procedurę przechowywaną (StoredProcedure).

### <a name="selectparameters"></a>SelectParameters

Zwraca parametry, które są używane przez właściwość SelectCommand obiektu SqlDataSourceView skojarzonego z kontrolką kontrolki SqlDataSource.

### <a name="sortparametername"></a>SortParameterName

Pobiera lub ustawia nazwę parametru procedury składowanej, który jest używany podczas sortowania danych pobranych przez formant źródła danych. Prawidłowy tylko wtedy, gdy właściwość SelectCommandtype ma wartość StoredProcedure.

### <a name="sqlcachedependency"></a>SqlCacheDependency

Rozdzielany średnikami ciąg określający bazy danych i tabele używane w zależności od pamięci podręcznej SQL Server. (Zależności w pamięci podręcznej SQL zostaną omówione w późniejszym module).

### <a name="updatecommand"></a>UpdateCommand

Ustawia lub pobiera ciąg SQL, który jest używany podczas aktualizowania danych w bazie danych. Może to być zapytanie SQL lub nazwa procedury składowanej.

### <a name="updatecommandtype"></a>UpdateCommandType

Ustawia lub pobiera typ polecenia aktualizacji, kwerendę SQL (tekst) lub procedurę przechowywaną (StoredProcedure).

### <a name="updateparameters"></a>UpdateParameters

Zwraca parametry, które są używane przez element UpdateCommand obiektu SqlDataSourceView skojarzonego z kontrolką kontrolki SqlDataSource.

## <a name="the-accessdatasource-control"></a>Formant AccessDataSource

Formant AccessDataSource pochodzi z klasy kontrolki SqlDataSource i służy do tworzenia powiązań danych z bazą danych programu Microsoft Access. Właściwość ConnectionString kontrolki AccessDataSource jest właściwością tylko do odczytu. Zamiast używać właściwości ConnectionString, właściwość datapliku jest używana do wskazywania bazy danych programu Access, jak pokazano poniżej.

[!code-aspx[Main](data-source-controls/samples/sample4.aspx)]

Wartość AccessDataSource zawsze ustawi wartość ProviderName podstawowego kontrolki SqlDataSource na system. Data. OleDb i nawiąże połączenie z bazą danych przy użyciu dostawcy OLE DB Microsoft. Jet. OLEDB. 4.0. Nie można użyć formantu AccessDataSource do nawiązania połączenia z bazą danych dostępu chronionego hasłem. Jeśli musisz nawiązać połączenie z bazą danych chronioną hasłem, użyj kontrolki kontrolki SqlDataSource.

> [!NOTE]
> Bazy danych programu Access przechowywane w witrynie sieci Web należy umieścić w katalogu\_aplikacji. ASP.NET nie zezwala na przeglądanie plików w tym katalogu. W przypadku korzystania z baz danych programu Access należy przyznać uprawnienia do odczytu i zapisu konta procesu dla aplikacji\_Directory.

## <a name="the-xmldatasource-control"></a>Formant XmlDataSource

XmlDataSource służy do powiązania danych XML z kontrolkami powiązanymi z danymi. Można powiązać z plikiem XML przy użyciu właściwości dataplików lub można powiązać z ciągiem XML przy użyciu właściwości data. XmlDataSource uwidacznia atrybuty XML jako pola możliwe do powiązania. W przypadkach, gdy należy powiązać z wartościami, które nie są reprezentowane jako atrybuty, należy użyć przekształcenia XSL. Możesz również użyć wyrażeń XPath do filtrowania danych XML.

Rozważmy następujący plik XML:

[!code-xml[Main](data-source-controls/samples/sample5.xml)]

Zwróć uwagę, że element XmlDataSource używa właściwości XPath *osób/osób* w celu filtrowania tylko dla &lt;osoby&gt; węzły. DropDownList następnie tworzy powiązania danych z atrybutem LastName przy użyciu właściwości DataTextField.

Chociaż formant XmlDataSource jest używany głównie do powiązania danych z danymi XML tylko do odczytu, możliwe jest edytowanie pliku danych XML. Należy zauważyć, że w takich przypadkach automatyczne wstawianie, aktualizowanie i usuwanie informacji w pliku XML nie odbywa się automatycznie, tak jak w przypadku innych kontrolek źródła danych. Zamiast tego należy napisać kod, aby ręcznie edytować dane przy użyciu następujących metod formantu XmlDataSource.

### <a name="getxmldocument"></a>GetXmlDocument

Pobiera obiekt XmlDocument zawierający kod XML pobrany przez element XmlDataSource.

### <a name="save"></a>Zapisz

Zapisuje w pamięci element XmlDocument z powrotem do źródła danych.

Należy pamiętać, że metoda Save będzie działała tylko wtedy, gdy spełnione są następujące dwa warunki:

1. Element XmlDataSource używa właściwości dataplików, aby powiązać z plikiem XML, a nie z właściwością danych, aby utworzyć powiązanie z danymi XML w pamięci.
2. Nie określono transformacji za pośrednictwem właściwości transform lub TransformFile.

Należy zauważyć, że metoda Save może dać nieoczekiwane wyniki, gdy jest wywoływana przez wielu użytkowników jednocześnie.

## <a name="the-objectdatasource-control"></a>Formant ObjectDataSource

Kontrolki źródła danych, które zostały omówione w tym punkcie, są doskonałymi opcjami dla aplikacji dwuwarstwowych, w których kontrola źródła danych komunikuje się bezpośrednio z magazynem danych. Jednak wiele aplikacji działających w świecie to aplikacje wielowarstwowe, w których kontrolka źródła danych może wymagać komunikacji z obiektem biznesowym, który z kolei komunikuje się z warstwą danych. W takich sytuacjach element ObjectDataSource wypełnia Bill dobrze. Element ObjectDataSource działa w połączeniu z obiektem źródłowym. Kontrolka ObjectDataSource utworzy wystąpienie obiektu źródłowego, wywołaj określoną metodę i Usuń wystąpienie obiektu wszystkie w zakresie pojedynczego żądania, jeśli obiekt ma metody instancji zamiast metod statycznych (udostępnione w Visual Basic). W związku z tym obiekt musi być bezstanowy. Oznacza to, że obiekt powinien uzyskać i zwolnić wszystkie wymagane zasoby w ramach pojedynczego żądania. Aby kontrolować sposób tworzenia obiektu źródłowego, należy obsłużyć zdarzenie ObjectCreating formantu ObjectDataSource. Można utworzyć wystąpienie obiektu źródłowego, a następnie ustawić właściwość obiektu ObjectInstance klasy ObjectDataSourceEventArgs na to wystąpienie. Kontrolka ObjectDataSource będzie używać wystąpienia, które jest tworzone w zdarzeniu ObjectCreating zamiast tworzenia własnego wystąpienia.

Jeśli obiekt źródłowy dla formantu ObjectDataSource udostępnia publiczne metody statyczne (udostępnione w Visual Basic), które mogą być wywoływane w celu pobierania i modyfikowania danych, kontrolka ObjectDataSource wywoła te metody bezpośrednio. Jeśli formant ObjectDataSource musi utworzyć wystąpienie obiektu źródłowego, aby wykonać wywołania metody, obiekt musi zawierać konstruktora publicznego, który nie przyjmuje żadnych parametrów. Kontrolka ObjectDataSource wywoła ten Konstruktor, gdy tworzy nowe wystąpienie obiektu źródłowego.

Jeśli obiekt źródłowy nie zawiera konstruktora publicznego bez parametrów, można utworzyć wystąpienie obiektu źródłowego, które będzie używane przez kontrolkę ObjectDataSource w zdarzeniu ObjectCreating.

## <a name="specifying-object-methods"></a>Określanie metod obiektów

Obiekt źródłowy formantu ObjectDataSource może zawierać dowolną liczbę metod, które są używane do wybierania, wstawiania, aktualizowania lub usuwania danych. Metody te są wywoływane przez kontrolkę ObjectDataSource na podstawie nazwy metody, jak określono za pomocą właściwości SelectMethod, InsertMethod, UpdateMethod lub DeleteMethod kontrolki ObjectDataSource. Obiekt źródłowy może również zawierać opcjonalną metodę SelectCount, która jest identyfikowana przez kontrolkę ObjectDataSource przy użyciu właściwości SelectCountMethod, która zwraca liczbę całkowitej liczby obiektów w źródle danych. Kontrolka ObjectDataSource wywoła metodę SelectCount po wywołaniu metody Select, aby pobrać łączną liczbę rekordów w źródle danych do użycia podczas stronicowania.

## <a name="lab-using-data-source-controls"></a>Laboratorium wykorzystujące kontrolki źródła danych

## <a name="exercise-1---displaying-data-with-the-sqldatasource-control"></a>Ćwiczenie 1 — Wyświetlanie danych za pomocą kontrolki kontrolki SqlDataSource

Następujące ćwiczenie nawiązują połączenie z bazą danych Northwind przy użyciu formantu kontrolki SqlDataSource. Przyjęto założenie, że masz dostęp do bazy danych Northwind w wystąpieniu SQL Server 2000.

1. Utwórz nową witrynę sieci Web ASP.NET.
2. Dodaj nowy plik Web. config.

    1. Kliknij prawym przyciskiem myszy projekt w Eksplorator rozwiązań a następnie kliknij pozycję Dodaj nowy element.
    2. Wybierz z listy szablonów pozycję plik konfiguracji sieci Web, a następnie kliknij przycisk Dodaj.
3. Edytuj connectionStrings&gt; &lt;w następujący sposób: 

    [!code-aspx[Main](data-source-controls/samples/sample6.aspx)]
4. Przełącz się do widoku kodu i Dodaj atrybut ConnectionString i atrybut SelectCommand do &lt;ASP: kontrolki SqlDataSource&gt; kontrolki w następujący sposób: 

    [!code-aspx[Main](data-source-controls/samples/sample7.aspx)]
5. Z widok Projekt Dodaj nową kontrolkę GridView.
6. Z listy rozwijanej wybierz źródło danych w menu zadania GridView wybierz pozycję SqlDataSource1.
7. Kliknij prawym przyciskiem myszy wartość default. aspx i wybierz polecenie Wyświetl w przeglądarce z menu. Kliknij przycisk tak po wyświetleniu monitu o zapisanie.
8. W widoku GridView są wyświetlane dane z tabeli Products.

## <a name="exercise-2---editing-data-with-the-sqldatasource-control"></a>Ćwiczenie 2 — Edytowanie danych za pomocą kontrolki kontrolki SqlDataSource

Poniższe ćwiczenie pokazują, jak dane powiązać formant DropDownList przy użyciu składni deklaratywnej i umożliwia edytowanie danych przedstawionych w kontrolce DropDownList.

1. W widok Projekt Usuń formant GridView z default. aspx. 

    **Ważne**: pozostaw formant kontrolki SqlDataSource na stronie.
2. Dodaj formant DropDownList do default. aspx.
3. Przejdź do widoku źródła.
4. Dodaj atrybut DataSourceId, DataTextField i DataValueField do formantu &lt;ASP: DropDownList&gt; w następujący sposób: 

    [!code-aspx[Main](data-source-controls/samples/sample8.aspx)]
5. Zapisz plik default. aspx i Wyświetl go w przeglądarce. Należy pamiętać, że DropDownList zawiera wszystkie produkty z bazy danych Northwind.
6. Zamknij okno przeglądarki.
7. W widoku źródła default. aspx Dodaj nową kontrolkę TextBox poniżej kontrolki DropDownList. Zmień Właściwość ID pola tekstowego na txtProductName.
8. Pod kontrolką TextBox Dodaj nową kontrolkę Button. Zmień Właściwość ID przycisku na btnUpdate oraz właściwość text, aby **zaktualizować nazwę produktu**.
9. W widoku źródła default. aspx Dodaj właściwość UpdateCommand i dwa nowe UpdateParameters do tagu kontrolki SqlDataSource w następujący sposób: 

    [!code-aspx[Main](data-source-controls/samples/sample9.aspx)]

    > [!NOTE]
    > Należy zauważyć, że w tym kodzie dodano dwa parametry aktualizacji (ProductName i ProductID). Te parametry są mapowane na właściwość Text pola tekstowego txtProductName oraz Właściwość SelectedValue DropDownList ddlProducts.
10. Przełącz się do widok Projekt i kliknij dwukrotnie kontrolkę przycisk, aby dodać procedurę obsługi zdarzeń.
11. Dodaj następujący kod do btnUpdate\_kliknij kod: 

    [!code-csharp[Main](data-source-controls/samples/sample10.cs)]
12. Kliknij prawym przyciskiem myszy plik default. aspx i wybierz opcję wyświetlania go w przeglądarce. Kliknij przycisk tak po wyświetleniu monitu, aby zapisać wszystkie zmiany.
13. Klasy częściowe ASP.NET 2,0 umożliwiają kompilowanie w czasie wykonywania. Nie jest konieczne Kompilowanie aplikacji, aby zobaczyć, że zmiany kodu zaczną obowiązywać.
14. Wybierz produkt z DropDownList.
15. Wprowadź nową nazwę wybranego produktu w polu tekstowym, a następnie kliknij przycisk Aktualizuj.
16. Nazwa produktu została zaktualizowana w bazie danych programu.

## <a name="exercise-3-using-the-objectdatasource-control"></a>Ćwiczenie 3 przy użyciu kontrolki ObjectDataSource

W tym ćwiczeniu pokazano, jak używać kontrolki ObjectDataSource i obiektu źródłowego do współdziałania z bazą danych Northwind.

1. Kliknij prawym przyciskiem myszy projekt w Eksplorator rozwiązań a następnie kliknij pozycję Dodaj nowy element.
2. Na liście szablony wybierz pozycję formularz sieci Web. Zmień nazwę na Object. aspx, a następnie kliknij przycisk Dodaj.
3. Kliknij prawym przyciskiem myszy projekt w Eksplorator rozwiązań a następnie kliknij pozycję Dodaj nowy element.
4. Na liście szablony wybierz pozycję Klasa. Zmień nazwę klasy na NorthwindData.cs, a następnie kliknij przycisk Dodaj.
5. Kliknij przycisk tak po wyświetleniu monitu o dodanie klasy do folderu\_aplikacji.
6. Dodaj następujący kod do pliku NorthwindData.cs: 

    [!code-csharp[Main](data-source-controls/samples/sample11.cs)]
7. Dodaj następujący kod do widoku źródła obiektu Object. aspx: 

    [!code-aspx[Main](data-source-controls/samples/sample12.aspx)]
8. Zapisz wszystkie pliki i Przeglądaj obiekt Object. aspx.
9. Korzystając z interfejsu, można wyświetlać szczegóły, edytować pracowników, dodawać pracowników i usuwać pracowników.

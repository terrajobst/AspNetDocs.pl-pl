---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application
title: Obsługa współbieżności z Entity Framework 4,0 w aplikacji sieci Web ASP.NET 4 | Microsoft Docs
author: tdykstra
description: Ta seria samouczków jest oparta na aplikacji sieci Web firmy Contoso University, utworzonej przez Wprowadzenie z serią samouczków Entity Framework 4,0. I...
ms.author: riande
ms.date: 01/26/2011
ms.assetid: a5aa22a6-fb7f-4f41-9c7f-addda151940b
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application
msc.type: authoredcontent
ms.openlocfilehash: 3df5f7d9c8fb22e1ea34fe16560bdb9a1309bb56
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78632588"
---
# <a name="handling-concurrency-with-the-entity-framework-40-in-an-aspnet-4-web-application"></a>Obsługa współbieżności z Entity Framework 4,0 w aplikacji sieci Web ASP.NET 4

Autor [Dykstra](https://github.com/tdykstra)

> Ta seria samouczków jest oparta na aplikacji sieci Web firmy Contoso University, utworzonej przez Wprowadzenie z serią samouczków [Entity Framework 4,0](https://asp.net/entity-framework/tutorials#Getting%20Started) . Jeśli wcześniej samouczki nie zostały wykonane, jako punkt wyjścia dla tego samouczka możesz [pobrać utworzoną aplikację](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) . Możesz również [pobrać aplikację](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) utworzoną przez kompletną serię samouczków. Jeśli masz pytania dotyczące samouczków, możesz je ogłosić na [forum ASP.NET Entity Framework](https://forums.asp.net/1227.aspx).

W poprzednim samouczku przedstawiono sposób sortowania i filtrowania danych przy użyciu kontrolki `ObjectDataSource` i Entity Framework. W tym samouczku przedstawiono opcje obsługi współbieżności w aplikacji sieci Web ASP.NET, która używa Entity Framework. Zostanie utworzona nowa strona sieci Web, która jest przeznaczona do aktualizowania przypisań biurowych. Na tej stronie można obsługiwać problemy współbieżności i na utworzonej wcześniej stronie działów.

[![Image06](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image2.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image1.png)

[![Image01](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image4.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image3.png)

## <a name="concurrency-conflicts"></a>Konflikty współbieżności

Konflikt współbieżności występuje, gdy jeden użytkownik edytuje rekord, a inny użytkownik edytuje ten sam rekord przed zapisaniem zmiany pierwszego użytkownika w bazie danych. Jeśli Entity Framework nie zostanie skonfigurowana w celu wykrywania takich konfliktów, osoba, która zaktualizuje bazę danych, zastępuje zmiany wprowadzone przez innych użytkowników. W wielu aplikacjach to ryzyko jest akceptowalne i nie trzeba konfigurować aplikacji tak, aby obsługiwała potencjalne konflikty współbieżności. (Jeśli istnieje kilku użytkowników lub kilka aktualizacji lub jeśli nie jest to naprawdę krytyczne, jeśli niektóre zmiany zostaną nadpisywane, koszty programowania współbieżności mogą być większe.) Jeśli nie musisz martwić się o konflikty współbieżności, możesz pominąć ten samouczek. Pozostałe dwa samouczki w serii nie zależą od elementów, które są kompilowane w tym elemencie.

### <a name="pessimistic-concurrency-locking"></a>Współbieżność pesymistyczna (blokowanie)

Jeśli aplikacja musi zapobiegać przypadkowej utracie danych w scenariuszach współbieżności, jeden ze sposobów jest używany do blokowania baz danych. Jest to nazywane *pesymistyczną współbieżnością*. Na przykład przed odczytaniem wiersza z bazy danych należy zażądać blokady dla dostępu tylko do odczytu lub do aktualizacji. Jeśli zablokujesz wiersz na potrzeby dostępu do aktualizacji, żaden inny użytkownik nie będzie mógł zablokować wiersza dla dostępu tylko do odczytu lub aktualizacji, ponieważ spowodowałoby to skopiowanie danych w procesie. Jeśli zablokujesz wiersz dla dostępu tylko do odczytu, inne osoby mogą także zablokować dostęp tylko do odczytu, ale nie dla aktualizacji.

Zarządzanie blokadami ma pewne wady. Może być skomplikowany dla programu. Wymaga to znaczących zasobów zarządzania bazami danych. może to spowodować problemy z wydajnością w miarę wzrostu liczby użytkowników aplikacji (oznacza to, że nie jest to dobrze skalowane). Z tego względu nie wszystkie systemy zarządzania bazami danych obsługują pesymistyczne współbieżności. Entity Framework nie zapewnia wbudowanej pomocy technicznej i ten samouczek nie pokazuje, jak go wdrożyć.

### <a name="optimistic-concurrency"></a>Optymistyczna współbieżność

Alternatywą dla pesymistycznej współbieżności jest *Optymistyczna współbieżność*. Współbieżność optymistyczna pozwala na wykonywanie konfliktów współbieżności, a następnie podejmowanie odpowiednich działań. Na przykład Jan uruchamia stronę *działu. aspx* , klika link **Edytuj** dla działu historia i zmniejsza kwotę **budżetu** od $1 000 000,00 do $125 000,00. (Jan administruje działem konkurencyjnym i chce zwolnić pieniądze za własny dział).

[![Image07](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image6.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image5.png)

Przed kliknięciem przycisku **Aktualizuj**w witrynie Janina zostanie uruchomiona ta sama strona, kliknięcie linku **Edytuj** dla działu historia, a następnie zmiana pola **data rozpoczęcia** z 1/10/2011 na 1/1/1999. (Janina zarządza działem historii i chce nadać mu wyższy staż).

[![Image08](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image8.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image7.png)

Jan klika pozycję **Aktualizuj** najpierw, a następnie kliknij przycisk **Aktualizuj**. Przeglądarka Janina wyświetla teraz kwotę **budżetu** jako $1 000 000,00, ale jest niepoprawna, ponieważ kwota została zmieniona przez jan na $125 000,00.

Poniżej wymieniono niektóre akcje, które można wykonać w tym scenariuszu:

- Można śledzić, która właściwość została zmodyfikowana przez użytkownika i zaktualizować tylko odpowiednie kolumny w bazie danych. W przykładowym scenariuszu żadne dane nie zostaną utracone, ponieważ różne właściwości zostały zaktualizowane przez dwóch użytkowników. Następnym razem, gdy ktoś przegląda dział historii, zobaczy 1/1/1999 i $125 000,00. 

    Jest to domyślne zachowanie w Entity Framework i może znacznie zmniejszyć liczbę konfliktów, które mogłyby spowodować utratę danych. Jednak takie zachowanie nie pozwala na utratę danych, jeśli konkurencyjne zmiany są wprowadzane do tej samej właściwości jednostki. Ponadto takie zachowanie nie zawsze jest możliwe; podczas mapowania procedur składowanych na typ jednostki wszystkie właściwości jednostki są aktualizowane po wprowadzeniu zmian w jednostce w bazie danych.
- Możesz pozwolić, aby zmiana została zastąpiona przez Jan. Po kliknięciu przycisku **Aktualizuj**kwota **budżetu** wraca do $1 000 000,00. Jest to tzw. *klient WINS* lub *ostatni w scenariuszu usługi WINS* . (Wartości klienta mają pierwszeństwo przed tym, co znajduje się w magazynie danych).
- Można zapobiec aktualizacji firmy Janina ze zmian w bazie danych. Zazwyczaj można wyświetlić komunikat o błędzie, wyświetlić jego bieżący stan i umożliwić jego ponowne wprowadzenie zmian, jeśli nadal chce je wprowadzić. Proces ten można zautomatyzować przez zapisanie jego danych wejściowych i umożliwienie mu ponownego zastosowania, bez konieczności jego ponownie wprowadzania. Jest to tzw. scenariusz *magazynu usługi WINS* . (Wartości magazynu danych pierwszeństwo wartości przesłany przez klienta.)

### <a name="detecting-concurrency-conflicts"></a>Wykrywanie konfliktów współbieżności

W Entity Framework można rozwiązać konflikty przez obsługę `OptimisticConcurrencyException` wyjątków zgłaszanych przez Entity Framework. Aby dowiedzieć się, kiedy należy zgłosić te wyjątki, Entity Framework musi mieć możliwość wykrywania konfliktów. W związku z tym należy odpowiednio skonfigurować bazę danych i model danych. Dostępne są następujące opcje włączania wykrywania konfliktów:

- W bazie danych programu Dołącz kolumnę tabeli, której można użyć do określenia, kiedy wiersz został zmieniony. Następnie można skonfigurować Entity Framework, aby uwzględnić tę kolumnę w klauzuli `Where` `Update` SQL lub `Delete` polecenia.

    Jest to cel kolumny `Timestamp` w tabeli `OfficeAssignment`.

    [![Image09](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image10.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image9.png)

    Typ danych kolumny `Timestamp` jest również nazywany `Timestamp`. Kolumna nie zawiera jednak wartości daty ani godziny. Zamiast tego wartość jest numerem sekwencyjnym, który jest zwiększany za każdym razem, gdy wiersz zostanie zaktualizowany. W `Update` lub `Delete`, klauzula `Where` zawiera oryginalną `Timestamp` wartość. Jeśli aktualizowany wiersz został zmieniony przez innego użytkownika, wartość w `Timestamp` jest różna od oryginalnej wartości, więc klauzula `Where` nie zwraca żadnego wiersza do zaktualizowania. Gdy Entity Framework stwierdzi, że żadne wiersze nie zostały zaktualizowane przez bieżącą `Update` lub polecenie `Delete` (oznacza to, że gdy liczba odnośnych wierszy wynosi zero), interpretuje to jako konflikt współbieżności.
- Skonfiguruj Entity Framework, aby uwzględnić oryginalne wartości każdej kolumny w tabeli w klauzuli `Where` poleceń `Update` i `Delete`.

    Jak w pierwszej opcji, jeśli coś w wierszu uległo zmianie od momentu pierwszego odczytu wiersza, klauzula `Where` nie zwróci wiersza do zaktualizowania, który Entity Framework interpretuje jako konflikt współbieżności. Ta metoda jest skuteczna jak użycie pola `Timestamp`, ale może być niewydajne. W przypadku tabel bazy danych z wieloma kolumnami może wystąpić bardzo duże klauzule `Where`, a w aplikacji sieci Web może być wymagane zachowanie dużej ilości danych. Utrzymywanie dużej ilości stanu może wpłynąć na wydajność aplikacji, ponieważ wymaga ona zasobów serwera (na przykład stanu sesji) lub musi być uwzględniona na stronie sieci Web (na przykład Wyświetl stan).

W tym samouczku zostanie dodana obsługa błędów optymistycznych konfliktów współbieżności dla jednostki, która nie ma właściwości śledzenia (jednostka `Department`) i dla jednostki, która ma właściwość śledzenia (`OfficeAssignment` jednostki).

## <a name="handling-optimistic-concurrency-without-a-tracking-property"></a>Obsługa optymistycznej współbieżności bez właściwości śledzenia

Aby zaimplementować optymistyczną współbieżność dla jednostki `Department`, która nie ma właściwości śledzenia (`Timestamp`), należy wykonać następujące zadania:

- Zmień model danych, aby włączyć śledzenie współbieżności dla jednostek `Department`.
- W klasie `SchoolRepository` obsługiwać wyjątki współbieżności w metodzie `SaveChanges`.
- Na stronie *działS. aspx* Obsługuj wyjątki współbieżności, wyświetlając komunikat ostrzegawczy informujący o niepomyślnych zmianach. Użytkownik może wyświetlić bieżące wartości i ponowić próbę zmiany, jeśli nadal są potrzebne.

### <a name="enabling-concurrency-tracking-in-the-data-model"></a>Włączanie śledzenia współbieżności w modelu danych

W programie Visual Studio Otwórz aplikację sieci Web Contoso University, z którą pracujesz w poprzednim samouczku w tej serii.

Otwórz *SchoolModel. edmx*i w projektancie modeli danych kliknij prawym przyciskiem myszy Właściwość `Name` w jednostce `Department`, a następnie kliknij polecenie **Właściwości**. W oknie **Właściwości** zmień właściwość `ConcurrencyMode` na `Fixed`.

[![Image16](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image12.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image11.png)

Wykonaj te same czynności dla innych niż podstawowe właściwości skalarnych (`Budget`, `StartDate`i `Administrator`). (Nie można tego zrobić dla właściwości nawigacji). Oznacza to, że za każdym razem, gdy Entity Framework generuje polecenie SQL `Update` lub `Delete` w celu zaktualizowania jednostki `Department` w bazie danych, te kolumny (z oryginalnymi wartościami) muszą być uwzględnione w klauzuli `Where`. Jeśli nie zostanie znaleziony żaden wiersz, gdy zostanie wykonane polecenie `Update` lub `Delete`, Entity Framework zgłosi wyjątek optymistycznej współbieżności.

Zapisz i Zamknij model danych.

### <a name="handling-concurrency-exceptions-in-the-dal"></a>Obsługa wyjątków współbieżności w DAL

Otwórz *SchoolRepository.cs* i Dodaj następującą instrukcję `using` dla przestrzeni nazw `System.Data`:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample1.cs)]

Dodaj następującą nową metodę `SaveChanges`, która obsługuje optymistyczne wyjątki współbieżności:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample2.cs)]

Jeśli wystąpi błąd współbieżności, gdy ta metoda jest wywoływana, wartości właściwości jednostki w pamięci są zastępowane wartościami znajdującymi się obecnie w bazie danych. Wyjątek współbieżności jest ponownie zgłaszany, dzięki czemu Strona sieci Web może je obsłużyć.

W metodach `DeleteDepartment` i `UpdateDepartment` Zastąp istniejące wywołanie `context.SaveChanges()` z wywołaniem `SaveChanges()`, aby wywołać nową metodę.

### <a name="handling-concurrency-exceptions-in-the-presentation-layer"></a>Obsługa wyjątków współbieżności w warstwie prezentacji

Otwórz *działS. aspx* i dodaj atrybut `OnDeleted="DepartmentsObjectDataSource_Deleted"` do kontrolki `DepartmentsObjectDataSource`. Tag otwierający dla kontrolki będzie teraz podobny do poniższego przykładu.

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample3.aspx)]

W kontrolce `DepartmentsGridView` Określ wszystkie kolumny tabeli w atrybucie `DataKeyNames`, jak pokazano w poniższym przykładzie. Należy zauważyć, że spowoduje to utworzenie bardzo dużych pól stanu widoku, co jest jednym z powodów, dla których użycie pola śledzenia jest ogólnie preferowanym sposobem śledzenia konfliktów współbieżności.

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample4.aspx)]

Otwórz *Departments.aspx.cs* i Dodaj następującą instrukcję `using` dla przestrzeni nazw `System.Data`:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample5.cs)]

Dodaj następującą nową metodę, która będzie wywoływana ze `Updated` kontroli źródła danych i `Deleted` obsługi zdarzeń w celu obsługi wyjątków współbieżności:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample6.cs)]

Ten kod sprawdza typ wyjątku i jeśli jest to wyjątek współbieżności, kod dynamicznie tworzy kontrolkę `CustomValidator`, która z kolei wyświetla komunikat w kontrolce `ValidationSummary`.

Wywołaj nową metodę z procedury obsługi zdarzeń `Updated`, która została dodana wcześniej. Ponadto Utwórz nową procedurę obsługi zdarzeń `Deleted`, która wywołuje tę samą metodę (ale nie wykonuje żadnych innych czynności):

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample7.cs)]

### <a name="testing-optimistic-concurrency-in-the-departments-page"></a>Testowanie optymistycznej współbieżności na stronie działy

Uruchom stronę *działS. aspx* .

[![Image17](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image14.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image13.png)

Kliknij przycisk **Edytuj** w wierszu i zmień wartość w kolumnie **budżet** . (Pamiętaj, że można edytować tylko te rekordy, które zostały utworzone dla tego samouczka, ponieważ istniejące `School` rekordy bazy danych zawierają nieprawidłowe dane. Rekord działu ekonomii jest bezpiecznym jednym do eksperymentowania z.)

[![Image18](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image16.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image15.png)

Otwórz nowe okno przeglądarki i ponownie uruchom stronę (Skopiuj adres URL z pola Adres pierwszego okna przeglądarki do drugiego okna przeglądarki).

[![Image17](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image18.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image17.png)

Kliknij przycisk **Edytuj** w tym samym wierszu, który został wcześniej zmodyfikowany, a następnie zmień wartość **budżetu** na inną.

[![Image19](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image20.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image19.png)

W drugim oknie przeglądarki kliknij pozycję **Aktualizuj**. Kwota **budżetu** została pomyślnie zmieniona na tę nową wartość.

[![Image20](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image22.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image21.png)

W pierwszym oknie przeglądarki kliknij pozycję **Aktualizuj**. Aktualizacja nie powiedzie się. Kwota **budżetu** jest ponownie wyświetlana przy użyciu wartości ustawionej w drugim oknie przeglądarki i zostanie wyświetlony komunikat o błędzie.

[![Image21](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image24.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image23.png)

## <a name="handling-optimistic-concurrency-using-a-tracking-property"></a>Obsługa optymistycznej współbieżności przy użyciu właściwości śledzenia

Aby obsłużyć optymistyczną współbieżność dla jednostki, która ma właściwość śledzenia, należy wykonać następujące zadania:

- Dodaj procedury składowane do modelu danych, aby zarządzać jednostkami `OfficeAssignment`. (Właściwości śledzenia i procedury składowane nie muszą być używane razem; są one po prostu pogrupowane w tym miejscu na potrzeby ilustracji).
- Dodaj metody CRUD do DAL i LOGIKI biznesowej dla jednostek `OfficeAssignment`, w tym kodu do obsługi optymistycznych wyjątków współbieżności w DAL.
- Utwórz stronę sieci Web przypisania pakietu Office.
- Przetestuj optymistyczną współbieżność na nowej stronie sieci Web.

### <a name="adding-officeassignment-stored-procedures-to-the-data-model"></a>Dodawanie OfficeAssignment procedur składowanych do modelu danych

Otwórz plik *SchoolModel. edmx* w projektancie modeli, kliknij prawym przyciskiem myszy powierzchnię projektu, a następnie kliknij pozycję **Aktualizuj model z bazy danych**. Na karcie **Dodaj** okna dialogowego **Wybierz obiekty bazy danych** rozwiń węzeł **procedury składowane** i wybierz trzy `OfficeAssignment` procedury składowane (zobacz poniższy zrzut ekranu), a następnie kliknij przycisk **Zakończ**. (Te procedury składowane znajdują się już w bazie danych podczas pobierania lub tworzenia przy użyciu skryptu).

[![Image02](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image26.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image25.png)

Kliknij prawym przyciskiem myszy jednostkę `OfficeAssignment` i wybierz pozycję **mapowanie procedury składowanej**.

[![Image03](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image28.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image27.png)

Ustaw funkcje **INSERT**, **Update**i **delete** w taki sposób, aby korzystały z odpowiednich procedur składowanych. Dla `OrigTimestamp` parametru funkcji `Update` Ustaw **Właściwość** na `Timestamp` i wybierz opcję **Użyj oryginalnej wartości** .

[![Image04](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image30.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image29.png)

Gdy Entity Framework wywoła procedurę składowaną `UpdateOfficeAssignment`, przekaże oryginalną wartość kolumny `Timestamp` w parametrze `OrigTimestamp`. Procedura składowana używa tego parametru w jego `Where` klauzuli:

[!code-sql[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample8.sql)]

Procedura składowana wybiera także nową wartość kolumny `Timestamp` po aktualizacji, dzięki czemu Entity Framework może utrzymywać `OfficeAssignment` jednostkę, która znajduje się w pamięci w synchronizacji z odpowiednim wierszem bazy danych.

(Należy zauważyć, że procedura składowana usuwania przypisania pakietu Office nie ma parametru `OrigTimestamp`. W związku z tym Entity Framework nie może zweryfikować, czy jednostka nie została zmieniona przed usunięciem.)

Zapisz i Zamknij model danych.

### <a name="adding-officeassignment-methods-to-the-dal"></a>Dodawanie metod OfficeAssignment do DAL

Otwórz *ISchoolRepository.cs* i Dodaj następujące metody CRUD dla zestawu jednostek `OfficeAssignment`:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample9.cs)]

Dodaj następujące nowe metody do *SchoolRepository.cs*. W metodzie `UpdateOfficeAssignment` należy wywołać metodę `SaveChanges` lokalnego zamiast `context.SaveChanges`.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample10.cs)]

W projekcie testowym Otwórz *MockSchoolRepository.cs* i Dodaj do niego następujące metody zbierania `OfficeAssignment` i CRUD. (Repozytorium makiety musi implementować interfejs repozytorium lub rozwiązanie nie zostanie skompilowane).

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample11.cs)]

### <a name="adding-officeassignment-methods-to-the-bll"></a>Dodawanie metod OfficeAssignment do LOGIKI biznesowej

W głównym projekcie Otwórz *SchoolBL.cs* i Dodaj następujące metody CRUD dla zestawu jednostek `OfficeAssignment`:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample12.cs)]

## <a name="creating-an-officeassignments-web-page"></a>Tworzenie strony sieci Web OfficeAssignments

Utwórz nową stronę sieci Web, która korzysta z *witryny. Master* Master i nadaj jej nazwę *OfficeAssignments. aspx*. Dodaj następujący znacznik do kontrolki `Content` o nazwie `Content2`:

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample13.aspx)]

Zauważ, że w atrybucie `DataKeyNames` znacznik określa właściwość `Timestamp`, a także klucz rekordu (`InstructorID`). Określanie właściwości w atrybucie `DataKeyNames` powoduje, że formant zapisuje je w stanie kontroli (co jest podobne do stanu widoku), dzięki czemu oryginalne wartości są dostępne podczas przetwarzania ogłaszania zwrotnego.

Jeśli nie zapisano wartości `Timestamp`, Entity Framework nie będzie miała dla klauzuli `Where` polecenia SQL `Update`. W związku z tym nic nie zostanie znalezione do zaktualizowania. W związku z tym Entity Framework może zgłosić wyjątek współbieżności optymistycznej za każdym razem, gdy jednostka `OfficeAssignment` zostanie zaktualizowana.

Otwórz *OfficeAssignments.aspx.cs* i Dodaj następującą instrukcję `using` dla warstwy dostępu do danych:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample14.cs)]

Dodaj następującą metodę `Page_Init`, która umożliwia dynamiczne działanie danych. Dodaj również następujące procedury obsługi dla zdarzenia `Updated` formantu `ObjectDataSource`, aby sprawdzić występowanie błędów współbieżności:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample15.cs)]

### <a name="testing-optimistic-concurrency-in-the-officeassignments-page"></a>Testowanie optymistycznej współbieżności na stronie OfficeAssignments

Uruchom stronę *OfficeAssignments. aspx* .

[![Image10](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image32.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image31.png)

Kliknij przycisk **Edytuj** w wierszu i zmień wartość w kolumnie **Lokalizacja** .

[![Image11](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image34.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image33.png)

Otwórz nowe okno przeglądarki i ponownie uruchom stronę (Skopiuj adres URL z pierwszego okna przeglądarki do drugiego okna przeglądarki).

[![Image10](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image36.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image35.png)

Kliknij przycisk **Edytuj** w tym samym wierszu, który był wcześniej edytowany, i zmień wartość **lokalizacji** na inną.

[![Image12](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image38.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image37.png)

W drugim oknie przeglądarki kliknij pozycję **Aktualizuj**.

[![Image13](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image40.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image39.png)

Przejdź do pierwszego okna przeglądarki, a następnie kliknij przycisk **Aktualizuj**.

[![Image15](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image42.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image41.png)

Zobaczysz komunikat o błędzie, a wartość **lokalizacji** została zaktualizowana, aby wyświetlić wartość, którą zmieniono w drugim oknie przeglądarki.

## <a name="handling-concurrency-with-the-entitydatasource-control"></a>Obsługa współbieżności przy użyciu kontrolki EntityDataSource

Formant `EntityDataSource` zawiera wbudowaną logikę, która rozpoznaje ustawienia współbieżności w modelu danych i odpowiednio obsługuje operacje aktualizowania i usuwania. Jednak podobnie jak w przypadku wszystkich wyjątków, należy samodzielnie obsłużyć `OptimisticConcurrencyException` wyjątków, aby zapewnić przyjazny dla użytkownika komunikat o błędzie.

Następnie skonfigurujesz stronę *kursy. aspx* (która używa kontrolki `EntityDataSource`) do zezwalania na operacje aktualizowania i usuwania oraz wyświetlania komunikatu o błędzie w przypadku wystąpienia konfliktu współbieżności. Jednostka `Course` nie ma kolumny śledzenia współbieżności, dlatego należy użyć tej samej metody, która została użyta w jednostce `Department`: Śledź wartości wszystkich właściwości niebędących kluczami.

Otwórz plik *SchoolModel. edmx* . Dla właściwości niebędących kluczami jednostki `Course` (`Title`, `Credits`i `DepartmentID`) ustaw właściwość **tryb współbieżności** na `Fixed`. Następnie Zapisz i Zamknij model danych.

Otwórz stronę *kursy. aspx* i wprowadź następujące zmiany:

- W kontrolce `CoursesEntityDataSource` Dodaj atrybuty `EnableUpdate="true"` i `EnableDelete="true"`. Tag otwierający dla tej kontrolki jest teraz podobny do następującego:

    [!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample16.aspx)]
- W kontrolce `CoursesGridView` Zmień wartość atrybutu `DataKeyNames` na `"CourseID,Title,Credits,DepartmentID"`. Następnie Dodaj element `CommandField` do elementu `Columns`, który zawiera przyciski **Edytuj** i **Usuń** (`<asp:CommandField ShowEditButton="True" ShowDeleteButton="True" />`). Kontrolka `GridView` jest teraz podobna do poniższego przykładu:

    [!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample17.aspx)]

Uruchom stronę i Utwórz sytuację powodującą konflikt tak jak wcześniej na stronie działy. Uruchom stronę w dwóch oknach przeglądarki, kliknij przycisk **Edytuj** w tym samym wierszu w każdym oknie i wprowadź inną zmianę w każdym z nich. Kliknij przycisk **Aktualizuj** w jednym oknie, a następnie kliknij przycisk **Aktualizuj** w drugim oknie. Po dwukrotnym kliknięciu przycisku **Aktualizuj** zostanie wyświetlona strona błędu z wynikiem nieobsłużonego wyjątku współbieżności.

[![Image22](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image44.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image43.png)

Ten błąd jest obsługiwany w sposób podobny do tego, jak został obsłużony dla formantu `ObjectDataSource`. Otwórz stronę *kursy. aspx* i w kontrolce `CoursesEntityDataSource` Określ programy obsługi dla zdarzeń `Deleted` i `Updated`. Tag otwierający kontrolki wygląda teraz podobnie jak w poniższym przykładzie:

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample18.aspx)]

Przed kontrolką `CoursesGridView` Dodaj następującą kontrolkę `ValidationSummary`:

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample19.aspx)]

W *courses.aspx.cs*dodaj instrukcję `using` dla przestrzeni nazw `System.Data`, Dodaj metodę, która sprawdza, czy są używane wyjątki współbieżności, i Dodaj programy obsługi dla `Updated` i `Deleted` formantów `EntityDataSource`. Kod będzie wyglądać następująco:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample20.cs)]

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample21.cs)]

Jedyną różnicą między tym kodem i przeznaczeniem dla kontrolki `ObjectDataSource` jest to, że w tym przypadku wyjątek współbieżności znajduje się we właściwości `Exception` obiektu argumenty zdarzenia, a nie w `InnerException` właściwości tego wyjątku.

Uruchom stronę i ponownie utwórz konflikt współbieżności. W tym momencie zostanie wyświetlony komunikat o błędzie:

[![Image23](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image46.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image45.png)

Kończy to wprowadzenie do obsługi konfliktów współbieżności. W następnym samouczku przedstawiono wskazówki dotyczące poprawy wydajności aplikacji sieci Web, która używa Entity Framework.

> [!div class="step-by-step"]
> [Poprzednie](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering.md)
> [dalej](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md)

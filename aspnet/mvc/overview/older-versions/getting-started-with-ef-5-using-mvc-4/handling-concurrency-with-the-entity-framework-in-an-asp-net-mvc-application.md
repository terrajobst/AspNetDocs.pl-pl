---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
title: Obsługa współbieżności przy użyciu Entity Framework w aplikacji ASP.NET MVC (7 z 10) | Microsoft Docs
author: tdykstra
description: Przykładowa aplikacja internetowa Contoso University pokazuje, jak tworzyć aplikacje ASP.NET MVC 4 przy użyciu Code First Entity Framework 5 i programu Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: b83f47c4-8521-4d0a-8644-e8f77e39733e
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 9800a313879477f36a730e6a70c79bc06d403ae3
ms.sourcegitcommit: e365196c75ce93cd8967412b1cfdc27121816110
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/07/2020
ms.locfileid: "77074959"
---
# <a name="handling-concurrency-with-the-entity-framework-in-an-aspnet-mvc-application-7-of-10"></a>Obsługa współbieżności przy użyciu Entity Framework w aplikacji ASP.NET MVC (7 z 10)

Autor [Dykstra](https://github.com/tdykstra)

[Pobierz ukończony projekt](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Przykładowa aplikacja internetowa Contoso University pokazuje, jak tworzyć aplikacje ASP.NET MVC 4 przy użyciu Entity Framework 5 Code First i programu Visual Studio 2012. Aby uzyskać informacje na temat serii samouczków, zobacz [pierwszy samouczek w serii](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Możesz uruchomić serię samouczków od początku lub [pobrać początkowy projekt dla tego rozdziału](building-the-ef5-mvc4-chapter-downloads.md) i Zacznij tutaj.
> 
> > [!NOTE] 
> > 
> > Jeśli wystąpi problem, którego nie można rozwiązać, [Pobierz ukończony rozdział](building-the-ef5-mvc4-chapter-downloads.md) i spróbuj odtworzyć problem. Ogólnie rzecz biorąc, można znaleźć rozwiązanie problemu, porównując kod z kompletnym kodem. Niektóre typowe błędy i sposoby ich rozwiązywania można znaleźć w temacie [błędy i obejścia.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)

W poprzednich dwóch samouczkach pracujesz z powiązanymi danymi. W tym samouczku przedstawiono sposób obsługi współbieżności. Utworzysz strony sieci Web, które współpracują z jednostką `Department`, a strony, które edytują i usuwają jednostki `Department`, będą obsługiwać błędy współbieżności. Na poniższych ilustracjach przedstawiono strony indeks i Usuń, w tym niektóre komunikaty, które są wyświetlane w przypadku wystąpienia konfliktu współbieżności.

![Department_Index_page_before_edits](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="concurrency-conflicts"></a>Konflikty współbieżności

Konflikt współbieżności występuje, gdy jeden użytkownik wyświetla dane jednostki w celu ich edycji, a następnie inny użytkownik aktualizuje dane tej samej jednostki przed zapisaniem w bazie danych pierwszego użytkownika. Jeśli nie włączysz wykrywania takich konfliktów, osoba, która aktualizuje bazę danych, ostatnio zastępuje zmiany wprowadzone przez innych użytkowników. W wielu aplikacjach to ryzyko jest akceptowalne: w przypadku kilku użytkowników lub kilku aktualizacji lub jeśli nie jest to naprawdę krytyczne, jeśli niektóre zmiany zostaną nadpisywane, koszt programowania współbieżności może wznieść korzyści. W takim przypadku nie trzeba konfigurować aplikacji do obsługi konfliktów współbieżności.

### <a name="pessimistic-concurrency-locking"></a>Współbieżność pesymistyczna (blokowanie)

Jeśli aplikacja musi zapobiegać przypadkowej utracie danych w scenariuszach współbieżności, jeden ze sposobów jest używany do blokowania baz danych. Jest to nazywane *pesymistyczną współbieżnością*. Na przykład przed odczytaniem wiersza z bazy danych należy zażądać blokady dla dostępu tylko do odczytu lub do aktualizacji. Jeśli zablokujesz wiersz na potrzeby dostępu do aktualizacji, żaden inny użytkownik nie będzie mógł zablokować wiersza dla dostępu tylko do odczytu lub aktualizacji, ponieważ spowodowałoby to skopiowanie danych w procesie. Jeśli zablokujesz wiersz dla dostępu tylko do odczytu, inne osoby mogą także zablokować dostęp tylko do odczytu, ale nie dla aktualizacji.

Zarządzanie blokadami ma wady. Może być skomplikowany dla programu. Wymaga to znaczących zasobów zarządzania bazami danych. może to spowodować problemy z wydajnością w miarę wzrostu liczby użytkowników aplikacji (oznacza to, że nie jest to dobrze skalowane). Z tego względu nie wszystkie systemy zarządzania bazami danych obsługują pesymistyczne współbieżności. Entity Framework nie zapewnia wbudowanej pomocy technicznej i ten samouczek nie pokazuje, jak go wdrożyć.

### <a name="optimistic-concurrency"></a>Optymistyczna współbieżność

Alternatywą dla pesymistycznej współbieżności jest *Optymistyczna współbieżność*. Współbieżność optymistyczna pozwala na wykonywanie konfliktów współbieżności, a następnie podejmowanie odpowiednich działań. Na przykład Jan uruchamia stronę Edytowanie działów, zmienia kwotę **budżetu** dla działu angielskiego z $350 000,00 na $0,00.

![Changing_English_dept_budget_to_100000](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Przed kliknięciem przycisku **Zapisz**, Janina uruchamia tę samą stronę i zmienia wartość pola **data rozpoczęcia** z 9/1/2007 na 8/8/2013.

![Changing_English_dept_start_date_to_1999](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Jan klika pozycję **Zapisz** jako pierwszy i widzi zmiany, gdy przeglądarka powróci do strony indeks, a następnie kliknij przycisk **Zapisz**. Co dzieje się potem określają sposób obsługi konfliktów współbieżności. Dostępne są następujące opcje:

- Można śledzić, która właściwość została zmodyfikowana przez użytkownika i zaktualizować tylko odpowiednie kolumny w bazie danych. W przykładowym scenariuszu żadne dane nie zostaną utracone, ponieważ różne właściwości zostały zaktualizowane przez dwóch użytkowników. Następnym razem, gdy ktoś przegląda dział w języku angielskim, zobaczy zmiany w wysokości Jan i Janina — datę początkową 8/8/2013 i budżet zerowych dolarów.

    Ta metoda aktualizacji może zmniejszyć liczbę konfliktów, które mogłyby spowodować utratę danych, ale nie może uniknąć utraty danych, jeśli wprowadzono konkurencyjne zmiany w tej samej właściwości jednostki. Czy Entity Framework działa w ten sposób, zależy od sposobu implementacji kodu aktualizacji. Często nie jest to praktyczne w aplikacji sieci Web, ponieważ może wymagać utrzymania dużej ilości danych w celu śledzenia wszystkich oryginalnych wartości właściwości dla jednostki, a także nowych wartości. Obsługa dużych ilości Stanów może wpłynąć na wydajność aplikacji, ponieważ wymaga zasobów serwera lub musi być uwzględniona na stronie sieci Web (na przykład w ukrytych polach).
- Możesz pozwolić, aby zmiana została zastąpiona przez Jan. Następnym razem, gdy ktoś przegląda dział w języku angielskim, zobaczy 8/8/2013 i przywrócona wartość $350 000,00. Jest to tzw. *klient WINS* lub *ostatni w scenariuszu usługi WINS* . (Wartości klienta mają pierwszeństwo przed tym, co znajduje się w magazynie danych). Jak zostało to opisane we wprowadzeniu do tej sekcji, jeśli nie wykonasz kodowania na potrzeby obsługi współbieżności, zostanie to wykonane automatycznie.
- Można zapobiec aktualizacji firmy Janina ze zmian w bazie danych. Zwykle zostanie wyświetlony komunikat o błędzie, pokazany bieżący stan danych i umożliwi to ponowne zastosowanie jego zmian, jeśli nadal chce je wprowadzić. Jest to tzw. scenariusz *magazynu usługi WINS* . (Wartości ze sklepu danych mają pierwszeństwo przed wartościami przesyłanymi przez klienta). W tym samouczku zostanie wdrożony scenariusz sklepu WINS. Ta metoda zapewnia, że żadne zmiany nie są zastępowane bez alertu użytkownika o tym, co się dzieje.

### <a name="detecting-concurrency-conflicts"></a>Wykrywanie konfliktów współbieżności

Konflikty można rozwiązać przez obsługę wyjątków [OptimisticConcurrencyException](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) zgłaszanych przez Entity Framework. Aby dowiedzieć się, kiedy należy zgłosić te wyjątki, Entity Framework musi mieć możliwość wykrywania konfliktów. W związku z tym należy odpowiednio skonfigurować bazę danych i model danych. Dostępne są następujące opcje włączania wykrywania konfliktów:

- W tabeli bazy danych Dołącz kolumnę śledzenia, której można użyć do określenia, kiedy wiersz został zmieniony. Następnie można skonfigurować Entity Framework, aby uwzględnić tę kolumnę w klauzuli `Where` `Update` SQL lub `Delete` polecenia.

    Typ danych kolumny śledzenia jest zazwyczaj [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx). Wartość [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) jest kolejnym numerem, który jest zwiększany za każdym razem, gdy wiersz zostanie zaktualizowany. W `Update` lub `Delete`, klauzula `Where` zawiera oryginalną wartość kolumny śledzenia (oryginalną wersję wiersza). Jeśli aktualizowany wiersz został zmieniony przez innego użytkownika, wartość w kolumnie `rowversion` różni się od oryginalnej wartości, dlatego instrukcja `Update` lub `Delete` nie może znaleźć wiersza do zaktualizowania z powodu klauzuli `Where`. Gdy Entity Framework stwierdzi, że żadne wiersze nie zostały zaktualizowane przez polecenie `Update` lub `Delete` (oznacza to, że gdy liczba odnośnych wierszy wynosi zero), interpretuje to jako konflikt współbieżności.
- Skonfiguruj Entity Framework, aby uwzględnić oryginalne wartości każdej kolumny w tabeli w klauzuli `Where` poleceń `Update` i `Delete`.

    Jak w pierwszej opcji, jeśli coś w wierszu uległo zmianie od momentu pierwszego odczytu wiersza, klauzula `Where` nie zwróci wiersza do zaktualizowania, który Entity Framework interpretuje jako konflikt współbieżności. W przypadku tabel bazy danych z wieloma kolumnami takie podejście może powodować bardzo duże `Where` klauzule i może wymagać utrzymania dużej ilości danych. Jak wspomniano wcześniej, utrzymanie dużej ilości stanu może wpłynąć na wydajność aplikacji, ponieważ wymaga ona zasobów serwera lub musi być uwzględniona na stronie sieci Web. W związku z tym takie podejście zwykle nie jest zalecane i nie jest to metoda używana w tym samouczku.

    Jeśli chcesz zaimplementować te podejście do współbieżności, musisz oznaczyć wszystkie właściwości klucza niepodstawowego w jednostce, dla której chcesz śledzić współbieżność, dodając do nich atrybut [ConcurrencyCheck](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.concurrencycheckattribute.aspx) . Ta zmiana umożliwia Entity Framework uwzględnienie wszystkich kolumn w klauzuli SQL `WHERE` instrukcji `UPDATE`.

W pozostałej części tego samouczka dodasz Właściwość śledzenia [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) do jednostki `Department`, utworzysz kontroler i widoki i testujesz, aby upewnić się, że wszystko działa prawidłowo.

## <a name="add-an-optimistic-concurrency-property-to-the-department-entity"></a>Dodawanie optymistycznej właściwości współbieżności do jednostki działu

W *Models\Department.cs*Dodaj właściwość śledzenia o nazwie `RowVersion`:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=18-19)]

Atrybut [timestamp](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx) określa, że ta kolumna zostanie uwzględniona w klauzuli `Where` `Update` i `Delete` polecenia wysyłane do bazy danych. Ten atrybut jest nazywany [sygnaturą czasową](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx) , ponieważ poprzednie wersje SQL Server używały typu danych [sygnatur czasowych](https://msdn.microsoft.com/library/ms182776(v=SQL.90).aspx) SQL przed zastąpieniem go przez [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) . Typ .NET dla `rowversion` jest tablicą bajtów. Jeśli wolisz używać interfejsu API Fluent, możesz użyć metody [IsConcurrencyToken](https://msdn.microsoft.com/library/gg679501(v=VS.103).aspx) , aby określić właściwość śledzenia, jak pokazano w następującym przykładzie:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Zobacz artykuł dotyczący usługi GitHub [Zamień IsConcurrencyToken na IsRowVersion](https://github.com/aspnet/AspNetDocs/issues/302).

Przez dodanie właściwości, która zmieniła model bazy danych, należy wykonać kolejną migrację. W konsoli Menedżera pakietów (PMC) wprowadź następujące polecenia:

[!code-console[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cmd)]

## <a name="create-a-department-controller"></a>Tworzenie kontrolera działu

Utwórz kontroler `Department` i Wyświetl w taki sam sposób jak inne kontrolery, używając następujących ustawień:

![Add_Controller_dialog_box_for_Department_controller](https://asp.net/media/2578041/Windows-Live-Writer_Handling-C.NET-MVC-Application-7-of-10h1_AFDC_Add_Controller_dialog_box_for_Department_controller_d1d9c788-f970-4d6a-9f5a-1eddc84330b7.png)

W *Controllers\DepartmentController.cs*dodaj instrukcję `using`:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

Zmień wartość "LastName" na "FullName" wszędzie w tym pliku (cztery wystąpienia), tak aby listy rozwijane administratorów działu zawierały pełną nazwę instruktora, a nie tylko nazwisko.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=1)]

Zastąp istniejący kod metody `HttpPost` `Edit` następującym kodem:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

W widoku będzie przechowywana oryginalna wartość `RowVersion` w ukrytym polu. Gdy spinacz modelu tworzy wystąpienie `department`, ten obiekt będzie miał oryginalną wartość właściwości `RowVersion` i nowe wartości dla innych właściwości, które zostały wprowadzone przez użytkownika na stronie Edycja. Następnie, gdy Entity Framework tworzy polecenie SQL `UPDATE`, to polecenie będzie zawierać klauzulę `WHERE`, która szuka wiersza, który ma oryginalną wartość `RowVersion`.

Jeśli nie ma żadnych wierszy, których dotyczy polecenie `UPDATE` (żadne wiersze nie mają oryginalnej wartości `RowVersion`), Entity Framework zgłasza wyjątek `DbUpdateConcurrencyException`, a kod w bloku `catch` pobiera jednostkę `Department`, której to dotyczy, z obiektu Exception. Ta jednostka ma zarówno wartości odczytane z bazy danych, jak i nowe wartości wprowadzone przez użytkownika:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Następnie kod dodaje niestandardowy komunikat o błędzie dla każdej kolumny, która ma wartości bazy danych inne niż wprowadzone przez użytkownika na stronie edytowania:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Dłuższy komunikat o błędzie wyjaśnia, co się stało i co należy zrobić:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Na koniec kod ustawia wartość `RowVersion` obiektu `Department` na nową wartość pobraną z bazy danych. Ta nowa `RowVersion` wartość będzie przechowywana w ukrytym polu po ponownym wyświetleniu strony edycji, a przy następnym kliknięciu przycisku **Zapisz**zostaną przechwycone tylko błędy współbieżności, które zachodzą od momentu wyświetlenia ekranu edycji.

W *Views\Department\Edit.cshtml*Dodaj pole ukryte, aby zapisać wartość właściwości `RowVersion`, bezpośrednio po ukrytym polu właściwości `DepartmentID`:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cshtml?highlight=17)]

W *Views\Department\Index.cshtml*Zastąp istniejący kod następującym kodem, aby przenieść linki do wierszy z lewej strony, a następnie zmień tytuł strony i nagłówki kolumn, aby były wyświetlane `FullName` zamiast `LastName` w kolumnie **administrator** :

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cshtml)]

## <a name="testing-optimistic-concurrency-handling"></a>Testowanie obsługi optymistycznej współbieżności

Uruchom lokację i kliknij pozycję **działy**:

![Department_Index_page_before_edits](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Kliknij prawym przyciskiem myszy pozycję **Edytuj** hiperłącze dla elementu Jan Abercrombie i wybierz polecenie **Otwórz na nowej karcie,** a następnie kliknij hiperlink **Edit** for Jan Abercrombie. Te dwa okna zawierają te same informacje.

![Department_Edit_page_before_changes](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Zmień pole w pierwszym oknie przeglądarki, a następnie kliknij przycisk **Zapisz**.

![Department_Edit_page_1_after_change](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

W przeglądarce zostanie wyświetlona strona indeks o zmienionej wartości.

![Departments_Index_page_after_first_budget_edit](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Zmień dowolne pole w drugim oknie przeglądarki, a następnie kliknij przycisk **Zapisz**.

![Department_Edit_page_2_after_change](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

Kliknij przycisk **Zapisz** w drugim oknie przeglądarki. Zostanie wyświetlony komunikat o błędzie:

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

Kliknij przycisk **Zapisz** ponownie. Wartość wprowadzona w drugiej przeglądarce jest zapisywana wraz z oryginalną wartością danych zmienionych w pierwszej przeglądarce. Zapisane wartości są wyświetlane, gdy zostanie wyświetlona strona indeks.

![Department_Index_page_with_change_from_second_browser](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)

## <a name="updating-the-delete-page"></a>Aktualizowanie strony usuwania

Na stronie Usuwanie Entity Framework wykrywa konflikty współbieżności spowodowane przez inną osobę edytującą dział w podobny sposób. Gdy metoda `HttpGet` `Delete` wyświetla widok potwierdzenia, widok zawiera oryginalną wartość `RowVersion` w ukrytym polu. Ta wartość jest następnie dostępna dla metody `Delete` `HttpPost`, która jest wywoływana, gdy użytkownik potwierdzi usunięcie. Gdy Entity Framework tworzy polecenie SQL `DELETE`, zawiera klauzulę `WHERE` z oryginalną wartością `RowVersion`. Jeśli w poleceniu wyniki są równe zero (oznacza to, że wiersz został zmieniony po wyświetleniu strony potwierdzenia usunięcia), zgłaszany jest wyjątek współbieżności, a metoda `HttpGet Delete` jest wywoływana z flagą błędu ustawioną na `true` w celu ponownego wyświetlenia strony potwierdzenia z komunikatem o błędzie. Istnieje również możliwość, że nie wpłynęły na wiersze, ponieważ wiersz został usunięty przez innego użytkownika, więc w takim przypadku zostanie wyświetlony inny komunikat o błędzie.

W *DepartmentController.cs*Zastąp metodę `Delete` `HttpGet` następującym kodem:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]

Metoda przyjmuje opcjonalny parametr, który wskazuje, czy strona jest ponownie wyświetlana po wystąpieniu błędu współbieżności. Jeśli ta flaga jest `true`, komunikat o błędzie jest wysyłany do widoku przy użyciu właściwości `ViewBag`.

Zastąp kod w metodzie `Delete` `HttpPost` (o nazwie `DeleteConfirmed`) następującym kodem:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

W kodzie szkieletowym, który właśnie został zastąpiony, ta metoda akceptuje tylko identyfikator rekordu:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

Ten parametr został zmieniony do wystąpienia jednostki `Department` utworzonego przez spinacz modelu. Zapewnia to dostęp do wartości właściwości `RowVersion` oprócz klucza rekordu.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

Zmieniono także nazwę metody akcji z `DeleteConfirmed` na `Delete`. Kod szkieletu o nazwie `HttpPost` `Delete` Metoda `DeleteConfirmed`, aby dać metodę `HttpPost` unikatową sygnaturę. (Środowisko CLR wymaga przeciążonych metod, aby mieć inne parametry metody). Teraz, gdy podpisy są unikatowe, można naklejić do Konwencji MVC i używać tej samej nazwy dla `HttpPost` i `HttpGet` metody Delete.

W przypadku przechwyconego błędu współbieżności kod ponownie wyświetla stronę potwierdzenia usuwania i zawiera flagę wskazującą, że powinien zostać wyświetlony komunikat o błędzie współbieżności.

W *Views\Department\Delete.cshtml*, Zastąp kod szkieletowy następującym kodem, który wprowadza zmiany formatowania i dodaje pole komunikatu o błędzie. Zmiany są wyróżnione.

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml?highlight=9,37,40,45-46)]

Ten kod dodaje komunikat o błędzie między nagłówkiem `h2` i `h3`:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]

Zastępuje `LastName` z `FullName` w `Administrator` polu:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml)]

Na koniec dodaje ukryte pola dla właściwości `DepartmentID` i `RowVersion` po instrukcji `Html.BeginForm`:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]

Uruchom stronę indeksu działów. Kliknij prawym przyciskiem myszy pozycję **Usuń** hiperłącze dla działu angielskiego i wybierz pozycję **Otwórz w nowym oknie,** a następnie w pierwszym oknie kliknij hiperlink **Edytuj** dla działu angielskiego.

W pierwszym oknie Zmień jedną z wartości, a następnie kliknij przycisk **Zapisz** :

![Department_Edit_page_after_change_before_delete](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

Strona indeks potwierdza zmianę.

![Departments_Index_page_after_budget_edit_before_delete](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image13.png)

W drugim oknie kliknij pozycję **Usuń**.

![Department_Delete_confirmation_page_before_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image14.png)

Zobaczysz komunikat o błędzie współbieżności, a wartości działu są odświeżane z aktualną wartością w bazie danych.

![Department_Delete_confirmation_page_with_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)

Jeśli klikniesz przycisk **Usuń** ponownie, nastąpi przekierowanie do strony indeks, która pokazuje, że dział został usunięty.

## <a name="summary"></a>Podsumowanie

Kończy to wprowadzenie do obsługi konfliktów współbieżności. Aby uzyskać informacje o innych sposobach obsługi różnych scenariuszy współbieżności, zobacz [optymistyczne wzorce współbieżności](https://blogs.msdn.com/b/adonet/archive/2011/02/03/using-dbcontext-in-ef-feature-ctp5-part-9-optimistic-concurrency-patterns.aspx) i [Praca z wartościami właściwości](https://blogs.msdn.com/b/adonet/archive/2011/01/30/using-dbcontext-in-ef-feature-ctp5-part-5-working-with-property-values.aspx) w blogu zespołu Entity Framework. W następnym samouczku pokazano, jak zaimplementować dziedziczenie na poziomie tabeli dla jednostek `Instructor` i `Student`.

Linki do innych zasobów Entity Framework można znaleźć w [mapie zawartości dostęp do danych ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Poprzednie](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [dalej](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)

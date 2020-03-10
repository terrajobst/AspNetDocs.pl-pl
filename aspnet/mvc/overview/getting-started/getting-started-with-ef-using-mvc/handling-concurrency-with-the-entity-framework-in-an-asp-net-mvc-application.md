---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
title: 'Samouczek: obsługa współbieżności z EF w aplikacji ASP.NET MVC 5'
description: W tym samouczku pokazano, jak używać optymistycznej współbieżności, aby obsłużyć konflikty, gdy wielu użytkowników jednocześnie zaktualizuje tę samą jednostkę.
author: tdykstra
ms.author: riande
ms.topic: tutorial
ms.date: 01/15/2019
ms.assetid: be0c098a-1fb2-457e-b815-ddca601afc65
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 43c5fdff5601c9bff32300d3460de0079a498d28
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78616110"
---
# <a name="tutorial-handle-concurrency-with-ef-in-an-aspnet-mvc-5-app"></a>Samouczek: obsługa współbieżności z EF w aplikacji ASP.NET MVC 5

We wcześniejszych samouczkach przedstawiono sposób aktualizowania danych. W tym samouczku pokazano, jak używać optymistycznej współbieżności, aby obsłużyć konflikty, gdy wielu użytkowników jednocześnie zaktualizuje tę samą jednostkę. Można zmienić strony sieci Web, które współpracują z jednostką `Department`, aby obsługiwały błędy współbieżności. Na poniższych ilustracjach przedstawiono strony edycji i usuwania, w tym niektóre komunikaty, które są wyświetlane w przypadku wystąpienia konfliktu współbieżności.

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)

W tym samouczku zostaną wykonane następujące czynności:

> [!div class="checklist"]
> * Informacje o konfliktach współbieżności
> * Dodawanie optymistycznej współbieżności
> * Modyfikowanie kontrolera działu
> * Obsługa współbieżności testów
> * Aktualizuj stronę Delete

## <a name="prerequisites"></a>Wymagania wstępne

* [Model asynchroniczny i procedury składowane](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="concurrency-conflicts"></a>Konfliktów współbieżności

Konflikt współbieżności występuje, gdy jeden użytkownik wyświetla dane jednostki w celu ich edycji, a następnie inny użytkownik aktualizuje dane tej samej jednostki przed zapisaniem w bazie danych pierwszego użytkownika. Jeśli nie włączysz wykrywania takich konfliktów, osoba, która aktualizuje bazę danych, ostatnio zastępuje zmiany wprowadzone przez innych użytkowników. W wielu aplikacjach to ryzyko jest akceptowalne: w przypadku kilku użytkowników lub kilku aktualizacji lub jeśli nie jest to naprawdę krytyczne, jeśli niektóre zmiany zostaną nadpisywane, koszt programowania współbieżności może wznieść korzyści. W takim przypadku nie trzeba konfigurować aplikacji do obsługi konfliktów współbieżności.

### <a name="pessimistic-concurrency-locking"></a>Współbieżność pesymistyczna (blokowanie)

Jeśli aplikacja musi zapobiegać przypadkowej utracie danych w scenariuszach współbieżności, jeden ze sposobów jest używany do blokowania baz danych. Jest to nazywane *pesymistyczną współbieżnością*. Na przykład przed odczytaniem wiersza z bazy danych należy zażądać blokady dla dostępu tylko do odczytu lub do aktualizacji. Jeśli zablokujesz wiersz na potrzeby dostępu do aktualizacji, żaden inny użytkownik nie będzie mógł zablokować wiersza dla dostępu tylko do odczytu lub aktualizacji, ponieważ spowodowałoby to skopiowanie danych w procesie. Jeśli zablokujesz wiersz dla dostępu tylko do odczytu, inne osoby mogą także zablokować dostęp tylko do odczytu, ale nie dla aktualizacji.

Zarządzanie blokadami ma wady. Może być skomplikowany dla programu. Wymaga to znaczących zasobów zarządzania bazami danych. może to spowodować problemy z wydajnością w miarę zwiększania się liczby użytkowników aplikacji. Z tego względu nie wszystkie systemy zarządzania bazami danych obsługują pesymistyczne współbieżności. Entity Framework nie zapewnia wbudowanej pomocy technicznej i ten samouczek nie pokazuje, jak go wdrożyć.

### <a name="optimistic-concurrency"></a>Optymistyczna współbieżność

Alternatywą dla pesymistycznej współbieżności jest *Optymistyczna współbieżność*. Współbieżność optymistyczna pozwala na wykonywanie konfliktów współbieżności, a następnie podejmowanie odpowiednich działań. Na przykład Jan uruchamia stronę Edytowanie działów, zmienia kwotę **budżetu** dla działu angielskiego z $350 000,00 na $0,00.

Przed kliknięciem przycisku **Zapisz**, Janina uruchamia tę samą stronę i zmienia wartość pola **data rozpoczęcia** z 9/1/2007 na 8/8/2013.

Jan klika pozycję **Zapisz** jako pierwszy i widzi zmiany, gdy przeglądarka powróci do strony indeks, a następnie kliknij przycisk **Zapisz**. Co dzieje się potem określają sposób obsługi konfliktów współbieżności. Dostępne są następujące opcje:

- Można śledzić, która właściwość została zmodyfikowana przez użytkownika i zaktualizować tylko odpowiednie kolumny w bazie danych. W przykładowym scenariuszu żadne dane nie zostaną utracone, ponieważ różne właściwości zostały zaktualizowane przez dwóch użytkowników. Następnym razem, gdy ktoś przegląda dział w języku angielskim, zobaczy zmiany w wysokości Jan i Janina — datę początkową 8/8/2013 i budżet zerowych dolarów.

    Ta metoda aktualizacji może zmniejszyć liczbę konfliktów, które mogłyby spowodować utratę danych, ale nie może uniknąć utraty danych, jeśli wprowadzono konkurencyjne zmiany w tej samej właściwości jednostki. Czy Entity Framework działa w ten sposób, zależy od sposobu implementacji kodu aktualizacji. Często nie jest to praktyczne w aplikacji sieci Web, ponieważ może wymagać utrzymania dużej ilości danych w celu śledzenia wszystkich oryginalnych wartości właściwości dla jednostki, a także nowych wartości. Obsługa dużych ilości Stanów może wpłynąć na wydajność aplikacji, ponieważ wymaga zasobów serwera lub musi być uwzględniona na stronie sieci Web (na przykład w ukrytych polach) lub w pliku cookie.
- Możesz pozwolić, aby zmiana została zastąpiona przez Jan. Następnym razem, gdy ktoś przegląda dział w języku angielskim, zobaczy 8/8/2013 i przywrócona wartość $350 000,00. Jest to tzw. *klient WINS* lub *ostatni w scenariuszu usługi WINS* . (Wszystkie wartości z klienta mają pierwszeństwo przed tym, co znajduje się w magazynie danych). Jak zostało to opisane we wprowadzeniu do tej sekcji, jeśli nie wykonasz kodowania na potrzeby obsługi współbieżności, zostanie to wykonane automatycznie.
- Można zapobiec aktualizacji firmy Janina ze zmian w bazie danych. Zwykle zostanie wyświetlony komunikat o błędzie, pokazany bieżący stan danych i umożliwi to ponowne zastosowanie jego zmian, jeśli nadal chce je wprowadzić. Jest to tzw. scenariusz *magazynu usługi WINS* . (Wartości ze sklepu danych mają pierwszeństwo przed wartościami przesyłanymi przez klienta). W tym samouczku zostanie wdrożony scenariusz sklepu WINS. Ta metoda zapewnia, że żadne zmiany nie są zastępowane bez alertu użytkownika o tym, co się dzieje.

### <a name="detecting-concurrency-conflicts"></a>Wykrywanie konfliktów współbieżności

Konflikty można rozwiązać przez obsługę wyjątków [OptimisticConcurrencyException](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) zgłaszanych przez Entity Framework. Aby dowiedzieć się, kiedy należy zgłosić te wyjątki, Entity Framework musi mieć możliwość wykrywania konfliktów. W związku z tym należy odpowiednio skonfigurować bazę danych i model danych. Dostępne są następujące opcje włączania wykrywania konfliktów:

- W tabeli bazy danych Dołącz kolumnę śledzenia, której można użyć do określenia, kiedy wiersz został zmieniony. Następnie można skonfigurować Entity Framework, aby uwzględnić tę kolumnę w klauzuli `Where` `Update` SQL lub `Delete` polecenia.

    Typ danych kolumny śledzenia jest zazwyczaj [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx). Wartość [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) jest kolejnym numerem, który jest zwiększany za każdym razem, gdy wiersz zostanie zaktualizowany. W `Update` lub `Delete`, klauzula `Where` zawiera oryginalną wartość kolumny śledzenia (oryginalną wersję wiersza). Jeśli aktualizowany wiersz został zmieniony przez innego użytkownika, wartość w kolumnie `rowversion` różni się od oryginalnej wartości, dlatego instrukcja `Update` lub `Delete` nie może znaleźć wiersza do zaktualizowania z powodu klauzuli `Where`. Gdy Entity Framework stwierdzi, że żadne wiersze nie zostały zaktualizowane przez polecenie `Update` lub `Delete` (oznacza to, że gdy liczba odnośnych wierszy wynosi zero), interpretuje to jako konflikt współbieżności.
- Skonfiguruj Entity Framework, aby uwzględnić oryginalne wartości każdej kolumny w tabeli w klauzuli `Where` poleceń `Update` i `Delete`.

    Jak w pierwszej opcji, jeśli coś w wierszu uległo zmianie od momentu pierwszego odczytu wiersza, klauzula `Where` nie zwróci wiersza do zaktualizowania, który Entity Framework interpretuje jako konflikt współbieżności. W przypadku tabel bazy danych z wieloma kolumnami takie podejście może powodować bardzo duże `Where` klauzule i może wymagać utrzymania dużej ilości danych. Jak wspomniano wcześniej, utrzymanie dużej ilości stanu może wpłynąć na wydajność aplikacji. W związku z tym takie podejście zwykle nie jest zalecane i nie jest to metoda używana w tym samouczku.

    Jeśli chcesz zaimplementować te podejście do współbieżności, musisz oznaczyć wszystkie właściwości klucza niepodstawowego w jednostce, dla której chcesz śledzić współbieżność, dodając do nich atrybut [ConcurrencyCheck](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.concurrencycheckattribute.aspx) . Ta zmiana umożliwia Entity Framework uwzględnienie wszystkich kolumn w klauzuli SQL `WHERE` instrukcji `UPDATE`.

W pozostałej części tego samouczka dodasz Właściwość śledzenia [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) do jednostki `Department`, utworzysz kontroler i widoki i testujesz, aby upewnić się, że wszystko działa prawidłowo.

## <a name="add-optimistic-concurrency"></a>Dodawanie optymistycznej współbieżności

W *Models\Department.cs*Dodaj właściwość śledzenia o nazwie `RowVersion`:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=20-22)]

Atrybut [timestamp](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx) określa, że ta kolumna zostanie uwzględniona w klauzuli `Where` `Update` i `Delete` polecenia wysyłane do bazy danych. Ten atrybut jest nazywany [sygnaturą czasową](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx) , ponieważ poprzednie wersje SQL Server używały typu danych [sygnatur czasowych](https://msdn.microsoft.com/library/ms182776(v=SQL.90).aspx) SQL przed zastąpieniem go przez [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) . Typem .NET dla [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) jest tablica bajtów.

Jeśli wolisz używać interfejsu API Fluent, możesz użyć metody [IsConcurrencyToken](https://msdn.microsoft.com/library/gg679501(v=VS.103).aspx) , aby określić właściwość śledzenia, jak pokazano w następującym przykładzie:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Przez dodanie właściwości, która zmieniła model bazy danych, należy wykonać kolejną migrację. W konsoli Menedżera pakietów (PMC) wprowadź następujące polecenia:

[!code-console[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cmd)]

## <a name="modify-department-controller"></a>Modyfikowanie kontrolera działu

W *Controllers\DepartmentController.cs*dodaj instrukcję `using`:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

W pliku *DepartmentController.cs* Zmień wszystkie cztery wystąpienia "LastName" na "FullName", tak aby listy rozwijane administratorów działu zawierały pełną nazwę instruktora, a nie tylko nazwisko.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=1)]

Zastąp istniejący kod metody `HttpPost` `Edit` następującym kodem:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

Jeśli metoda `FindAsync` zwraca wartość null, dział został usunięty przez innego użytkownika. Pokazany kod używa opublikowanych wartości formularzy do utworzenia jednostki działu, aby można było ponownie wyświetlić stronę edytowania z komunikatem o błędzie. Alternatywnie nie trzeba ponownie tworzyć jednostki działu, jeśli zostanie wyświetlony komunikat o błędzie bez ponownego wyświetlania pól działu.

Widok przechowuje oryginalną wartość `RowVersion` w ukrytym polu, a metoda otrzymuje ją w parametrze `rowVersion`. Przed wywołaniem `SaveChanges`należy umieścić tę oryginalną wartość właściwości `RowVersion` w kolekcji `OriginalValues` dla jednostki. Następnie, gdy Entity Framework tworzy polecenie SQL `UPDATE`, to polecenie będzie zawierać klauzulę `WHERE`, która szuka wiersza, który ma oryginalną wartość `RowVersion`.

Jeśli nie ma żadnych wierszy, których dotyczy polecenie `UPDATE` (żadne wiersze nie mają oryginalnej wartości `RowVersion`), Entity Framework zgłasza wyjątek `DbUpdateConcurrencyException`, a kod w bloku `catch` pobiera jednostkę `Department`, której to dotyczy, z obiektu Exception.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Ten obiekt ma nowe wartości wprowadzone przez użytkownika w jego właściwości `Entity` i można uzyskać wartości odczytane z bazy danych, wywołując metodę `GetDatabaseValues`.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Metoda `GetDatabaseValues` zwraca wartość null, jeśli ktoś usunął wiersz z bazy danych. w przeciwnym razie musisz rzutować zwracany obiekt na klasę `Department`, aby uzyskać dostęp do właściwości `Department`. (Ponieważ sprawdzono już do usunięcia, `databaseEntry` będzie miał wartość null tylko wtedy, gdy dział został usunięty po wykonaniu `FindAsync` i przed wykonaniem `SaveChanges`.)

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Następnie kod dodaje niestandardowy komunikat o błędzie dla każdej kolumny, która ma wartości bazy danych inne niż wprowadzone przez użytkownika na stronie edytowania:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

Dłuższy komunikat o błędzie wyjaśnia, co się stało i co należy zrobić:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

Na koniec kod ustawia wartość `RowVersion` obiektu `Department` na nową wartość pobraną z bazy danych. Ta nowa `RowVersion` wartość będzie przechowywana w ukrytym polu po ponownym wyświetleniu strony edycji, a przy następnym kliknięciu przycisku **Zapisz**zostaną przechwycone tylko błędy współbieżności, które zachodzą od momentu wyświetlenia ekranu edycji.

W *Views\Department\Edit.cshtml*Dodaj pole ukryte, aby zapisać wartość właściwości `RowVersion`, bezpośrednio po ukrytym polu właściwości `DepartmentID`:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=18)]

## <a name="test-concurrency-handling"></a>Obsługa współbieżności testów

Uruchom lokację i kliknij pozycję **działy**.

Kliknij prawym przyciskiem myszy pozycję **Edytuj** hiperlink dla działu angielskiego i wybierz polecenie **Otwórz na nowej karcie,** a następnie kliknij hiperłącze **Edytuj** dla działu angielskiego. Dwie karty zawierają te same informacje.

Zmień pole na pierwszej karcie przeglądarki, a następnie kliknij przycisk **Zapisz**.

W przeglądarce zostanie wyświetlona strona indeks o zmienionej wartości.

Zmień pole na drugiej karcie przeglądarki, a następnie kliknij przycisk **Zapisz**. Zostanie wyświetlony komunikat o błędzie:

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

Kliknij przycisk **Zapisz** ponownie. Wartość wprowadzona na drugiej karcie przeglądarki jest zapisywana wraz z oryginalną wartością danych, które zostały zmienione w pierwszej przeglądarce. Zapisane wartości są wyświetlane, gdy zostanie wyświetlona strona indeks.

## <a name="update-the-delete-page"></a>Aktualizuj stronę Delete

Na stronie Usuwanie Entity Framework wykrywa konflikty współbieżności spowodowane przez inną osobę edytującą dział w podobny sposób. Gdy metoda `HttpGet` `Delete` wyświetla widok potwierdzenia, widok zawiera oryginalną wartość `RowVersion` w ukrytym polu. Ta wartość jest następnie dostępna dla metody `Delete` `HttpPost`, która jest wywoływana, gdy użytkownik potwierdzi usunięcie. Gdy Entity Framework tworzy polecenie SQL `DELETE`, zawiera klauzulę `WHERE` z oryginalną wartością `RowVersion`. Jeśli w poleceniu wyniki są równe zero (oznacza to, że wiersz został zmieniony po wyświetleniu strony potwierdzenia usunięcia), zgłaszany jest wyjątek współbieżności, a metoda `HttpGet Delete` jest wywoływana z flagą błędu ustawioną na `true` w celu ponownego wyświetlenia strony potwierdzenia z komunikatem o błędzie. Istnieje również możliwość, że nie wpłynęły na wiersze, ponieważ wiersz został usunięty przez innego użytkownika, więc w takim przypadku zostanie wyświetlony inny komunikat o błędzie.

W *DepartmentController.cs*Zastąp metodę `Delete` `HttpGet` następującym kodem:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

Metoda przyjmuje opcjonalny parametr, który wskazuje, czy strona jest ponownie wyświetlana po wystąpieniu błędu współbieżności. Jeśli ta flaga jest `true`, komunikat o błędzie jest wysyłany do widoku przy użyciu właściwości `ViewBag`.

Zastąp kod w metodzie `Delete` `HttpPost` (o nazwie `DeleteConfirmed`) następującym kodem:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

W kodzie szkieletowym, który właśnie został zastąpiony, ta metoda akceptuje tylko identyfikator rekordu:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

Ten parametr został zmieniony do wystąpienia jednostki `Department` utworzonego przez spinacz modelu. Zapewnia to dostęp do wartości właściwości `RowVersion` oprócz klucza rekordu.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

Zmieniono także nazwę metody akcji z `DeleteConfirmed` na `Delete`. Kod szkieletu o nazwie `HttpPost` `Delete` Metoda `DeleteConfirmed`, aby dać metodę `HttpPost` unikatową sygnaturę. (Środowisko CLR wymaga przeciążonych metod, aby mieć inne parametry metody). Teraz, gdy podpisy są unikatowe, można naklejić do Konwencji MVC i używać tej samej nazwy dla `HttpPost` i `HttpGet` metody Delete.

W przypadku przechwyconego błędu współbieżności kod ponownie wyświetla stronę potwierdzenia usuwania i zawiera flagę wskazującą, że powinien zostać wyświetlony komunikat o błędzie współbieżności.

W *Views\Department\Delete.cshtml*, Zastąp kod szkieletowy następującym kodem, który dodaje pole komunikatu o błędzie i ukryte pola właściwości DepartmentID i rowversion. Zmiany są wyróżnione.

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml?highlight=9-10,21,52-54)]

Ten kod dodaje komunikat o błędzie między nagłówkiem `h2` i `h3`:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml)]

Zastępuje `LastName` z `FullName` w `Administrator` polu:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]

Na koniec dodaje ukryte pola dla właściwości `DepartmentID` i `RowVersion` po instrukcji `Html.BeginForm`:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cshtml)]

Uruchom stronę indeksu działów. Kliknij prawym przyciskiem myszy pozycję **Usuń** hiperłącze dla działu angielskiego i wybierz pozycję **Otwórz na nowej karcie,** a następnie na pierwszej karcie kliknij hiperłącze **Edytuj** dla działu angielskiego.

W pierwszym oknie Zmień jedną z wartości, a następnie kliknij przycisk **Zapisz**.

Strona indeks potwierdza zmianę.

Na drugiej karcie kliknij pozycję **Usuń**.

Zobaczysz komunikat o błędzie współbieżności, a wartości działu są odświeżane z aktualną wartością w bazie danych.

![Department_Delete_confirmation_page_with_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)

Jeśli klikniesz przycisk **Usuń** ponownie, nastąpi przekierowanie do strony indeks, która pokazuje, że dział został usunięty.

## <a name="get-the-code"></a>Uzyskiwanie kodu

[Pobierz ukończony projekt](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Dodatkowe zasoby

Linki do innych zasobów Entity Framework można znaleźć w [zasobach zalecanych przez dostęp do danych ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

Aby uzyskać informacje o innych sposobach obsługi różnych scenariuszy współbieżności, zobacz [optymistyczne wzorce współbieżności](https://msdn.microsoft.com/data/jj592904) i [Praca z wartościami właściwości](https://msdn.microsoft.com/data/jj592677) w witrynie MSDN. W następnym samouczku pokazano, jak zaimplementować dziedziczenie na poziomie tabeli dla jednostek `Instructor` i `Student`.

## <a name="next-steps"></a>Następne kroki

W tym samouczku zostaną wykonane następujące czynności:

> [!div class="checklist"]
> * Informacje o konfliktach współbieżności
> * Dodano optymistyczną współbieżność
> * Zmodyfikowany kontroler działu
> * Przetestowana obsługa współbieżności
> * Zaktualizowano stronę usuwania

Przejdź do następnego artykułu, aby dowiedzieć się, jak zaimplementować dziedziczenie w modelu danych.
> [!div class="nextstepaction"]
> [Implementowanie dziedziczenia w modelu danych](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)

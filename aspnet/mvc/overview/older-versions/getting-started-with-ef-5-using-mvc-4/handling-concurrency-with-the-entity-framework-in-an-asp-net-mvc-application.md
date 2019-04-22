---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
title: Obsługa współbieżności przy użyciu platformy Entity Framework w aplikacji ASP.NET MVC (7 10) | Dokumentacja firmy Microsoft
author: tdykstra
description: Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 4 przy użyciu Entity Framework 5 Code First i programu Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: b83f47c4-8521-4d0a-8644-e8f77e39733e
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: a79cca143df9a10b4255796a6d034688713e4e52
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/17/2019
ms.locfileid: "59379758"
---
# <a name="handling-concurrency-with-the-entity-framework-in-an-aspnet-mvc-application-7-of-10"></a>Obsługa współbieżności przy użyciu platformy Entity Framework w aplikacji ASP.NET MVC (7 10)

przez [Tom Dykstra](https://github.com/tdykstra)

[Pobierz ukończony projekt](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 4 przy użyciu Entity Framework 5 Code First i programu Visual Studio 2012. Aby uzyskać informacji na temat tej serii samouczka, zobacz [pierwszym samouczku tej serii](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Można uruchomić tej serii samouczka od początku lub [pobrać projekt startowy w tym rozdziale](building-the-ef5-mvc4-chapter-downloads.md) i zacznij tutaj.
> 
> > [!NOTE] 
> > 
> > Jeśli napotkasz problem, nie można rozpoznać [Pobieranie ukończone rozdział](building-the-ef5-mvc4-chapter-downloads.md) i spróbuj odtworzyć problem. Rozwiązanie tego problemu można znaleźć zwykle porównując swój kod, aby kompletny kod. Niektóre typowe błędy i sposobu rozwiązania tych problemów można znaleźć [błędów i rozwiązania problemu.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


W poprzednich samouczkach dwóch doświadczenie w pracy z powiązanych danych. W tym samouczku pokazano, jak obsługiwać współbieżności. Utworzysz stron sieci web, które działają z `Department` jednostki i stron, które edytowania i usuwania `Department` jednostek obsługi błędów współbieżności. Na poniższych ilustracjach przedstawiono indeks i usuwanie stron, tym niektóre komunikaty, które są wyświetlane, jeśli wystąpi konflikt współbieżności.

![Department_Index_page_before_edits](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="concurrency-conflicts"></a>Konfliktów współbieżności

Występuje konflikt współbieżności, gdy jeden użytkownik wyświetla dane jednostki w celu edycji, a następnie inny użytkownik aktualizuje dane w tej samej jednostki przed zapisaniem zmian pierwszego użytkownika do bazy danych. Jeśli nie włączysz wykrywania takie konflikty, ostatnia spojrzenia aktualizuje bazę danych zastępuje zmiany wprowadzone przez użytkownika. W wielu aplikacjach, to zagrożenie jest dopuszczalne: w przypadku kilku użytkowników lub kilka aktualizacji lub jeśli nie jest tak naprawdę krytycznego, jeśli niektóre zmiany są zastępowane, koszt programowania współbieżność może przeważają korzyści. W takiej sytuacji nie trzeba skonfigurować aplikację do obsługi konfliktów współbieżności.

### <a name="pessimistic-concurrency-locking"></a>Współbieżność pesymistyczna (blokowanie)

Jeśli Twoja aplikacja wymaga uniknąć przypadkowej utraty danych w scenariuszach współbieżności, używają blokady bazy danych jest jednym ze sposobów, aby to zrobić. Jest to nazywane *Współbieżność pesymistyczna*. Na przykład przed przeczytaniem wiersz z bazy danych, możesz zażądać blokady dla tylko do odczytu lub uzyskać dostęp do aktualizacji. Jeśli zablokujesz wiersza dla dostępu do aktualizacji, inni użytkownicy nie mogą zablokować wiersza dla tylko do odczytu lub zaktualizować dostępu, ponieważ jaką uzyskują kopii danych, która jest w trakcie zmieniany. Jeśli zablokujesz wiersz, aby uzyskać dostęp tylko do odczytu, inne można również zablokować go uzyskać dostęp tylko do odczytu, ale nie dla aktualizacji.

Zarządzanie blokady ma wady. Może być skomplikowane, aby program. Wymaga znaczących bazy danych zarządzania zasobami, a jako liczbę użytkowników aplikacji może spowodować problemy z wydajnością zwiększa (czyli go nie jest dobrze skalowalna). Z tego względu nie wszystkie systemy zarządzania bazami danych obsługuje pesymistycznej współbieżności. Entity Framework nie obsługuje wbudowanej go, a w tym samouczku nie pokazano, jak ją wdrożyć.

### <a name="optimistic-concurrency"></a>Optymistyczna współbieżność

Jest alternatywą do Współbieżność pesymistyczna *optymistycznej współbieżności*. Optymistyczna współbieżność oznacza, umożliwiając konfliktów współbieżności do wykonania, a następnie reagowaniu odpowiednio Jeśli tak jest. Na przykład Jan uruchamia działów stronie Edytowanie zmiany **budżetu** kwotę dla angielskiego dział z $350,000.00 0,00 USD.

![Changing_English_dept_budget_to_100000](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Zanim Jan kliknie **Zapisz**, Magdalena uruchamia tej samej stronie i zmiany **Data rozpoczęcia** pola z 1-9/2007 do 8/8/2013.

![Changing_English_dept_start_date_to_1999](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

John kliknie **Zapisz** pierwszy i widzi kliknie jego zmiana, gdy przeglądarka powróci do strony indeksu, a następnie Magdalena **Zapisz**. Co dzieje się potem określają sposób obsługi konfliktów współbieżności. Niektóre opcje obejmują następujące czynności:

- Można zachować informacje o właściwości, które użytkownik zmodyfikował i aktualizować tylko odpowiednie kolumny w bazie danych. W przykładowym scenariuszu żadne dane nie zostałyby utracone, ponieważ inne właściwości zostały zaktualizowane przez dwóch użytkowników. Przy następnym ktoś przegląda angielskiej działu, zobaczą zmiany nazwy i John's — Data rozpoczęcia 8/8/2013 oraz budżetu, zerowego dolarów.

    Ta metoda aktualizacji może zmniejszyć liczbę konfliktów, które może spowodować utratę danych, ale nie można uniknąć utraty danych, jeśli konkurencyjnych zmiany zostały wprowadzone w tej samej właściwości jednostki. Czy platformy Entity Framework działa w ten sposób zależy od tego, jak zaimplementować kod aktualizacji. Często nie jest praktyczne w aplikacji sieci web, ponieważ może wymagać, obsługa dużych ilości stanu w celu śledzenia oryginalnych wartości właściwości dla jednostki, a także nowe wartości. Obsługa dużych ilości stanu może wpłynąć na wydajność aplikacji, ponieważ jej wymaga zasobów serwera albo muszą być zawarte na stronie sieci web (na przykład w ukrytych polach).
- Można pozwolić, aby zmiana Joanny zastąpienie John's zmian. Przy następnym ktoś przegląda angielskiej działu, użytkownik zobaczy 8/8/2013 i przywrócone wartości $350,000.00. Jest to nazywane *Wins klienta* lub *ostatnie w usłudze Wins* scenariusza. (Wartości klienta pierwszeństwo co znajduje się w magazynie danych.) Jak wspomniano we wprowadzeniu do tej sekcji, w przeciwnym razie pisania obsługi współbieżności, nastąpi to automatycznie.
- Aby uniemożliwić zmiany Joanny aktualizowane w bazie danych. Zazwyczaj będzie wyświetlony komunikat o błędzie, pokazywać bieżący stan danych i umożliwienia jej uzyskania ponownie zastosować jej zmiany, jeśli chce nadal je. Jest to nazywane *Store Wins* scenariusza. (Wartości magazynu danych pierwszeństwo wartości przesłany przez klienta.) Będzie implementować scenariusza Store Wins w ramach tego samouczka. Ta metoda zapewnia, że żadne zmiany nie zostaną zastąpione bez użytkownika, w tym celu z wydarzeniami.

### <a name="detecting-concurrency-conflicts"></a>Wykrywanie konfliktów współbieżności

Należy rozwiązać konflikty, obsługując [OptimisticConcurrencyException](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) wyjątki, które zgłasza Entity Framework. Aby wiedzieć, kiedy trzeba generować te wyjątki, platformy Entity Framework musi umożliwiać wykrywanie konfliktów. W związku z tym należy skonfigurować bazy danych oraz model danych odpowiednio. Niektóre opcje umożliwiające wykrywanie konfliktów są następujące:

- W tabeli bazy danych zawierają kolumny śledzenia, który może służyć do określania, kiedy wiersz został zmieniony. Następnie można skonfigurować programu Entity Framework, aby uwzględnić tej kolumny w `Where` klauzuli SQL `Update` lub `Delete` poleceń.

    Typ danych kolumny śledzenia jest zazwyczaj [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx). [Rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) wartość numeru sekwencyjnego, który jest zwiększana za każdym razem, zaktualizować wiersza. W `Update` lub `Delete` polecenia `Where` klauzula zawiera oryginalna wartość kolumny śledzenia (oryginalna wersja wiersza). Jeśli aktualizacji wiersza został zmieniony przez innego użytkownika, wartość w `rowversion` kolumny różni się od oryginalnej wartości, więc `Update` lub `Delete` instrukcji nie można odnaleźć wiersza do zaktualizowania z powodu `Where` klauzuli. Gdy Entity Framework wykryje, czy żadne wiersze nie zostały zaktualizowane przez `Update` lub `Delete` polecenie (to znaczy, gdy liczba wierszy, których to dotyczy, jest równy zero), który interpretuje jako konflikt współbieżności.
- Konfigurowanie programu Entity Framework, aby uwzględnić oryginalnych wartości każdej kolumny w tabeli w `Where` klauzuli `Update` i `Delete` poleceń.

    Tak jak w pierwszej opcji, jeśli nic w wierszu zmieniła się od wiersza została najpierw przeczytać artykuł `Where` klauzuli nie zwraca wiersz, aby zaktualizować, której platformy Entity Framework interpretuje jako konflikt współbieżności. Dla tabel bazy danych, które mają wiele kolumn, to podejście może doprowadzić do bardzo dużych `Where` zdań i może wymagać obsługi dużych ilości stanu. Jak wspomniano wcześniej, obsługi dużych ilości stanu może wpłynąć na wydajność aplikacji, ponieważ jej wymaga zasobów serwera albo muszą być zawarte w stronie sieci web. W związku z tym to podejście zazwyczaj nie zaleca się i nie można go metodę używaną w ramach tego samouczka.

    Jeśli chcesz zaimplementować to podejście do współbieżności, musisz oznaczyć wszystkie właściwości bez klucza podstawowego w jednostce, które chcesz śledzić concurrency, dodając [ConcurrencyCheck](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.concurrencycheckattribute.aspx) atrybutu do nich. Czy zmiana umożliwia programu Entity Framework uwzględnić wszystkie kolumny w SQL `WHERE` klauzuli `UPDATE` instrukcji.

W pozostałej części tego samouczka dodasz [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) śledzenia właściwość `Department` jednostki, Utwórz kontrolera i widoki, a test, aby sprawdzić, czy wszystko działa poprawnie.

## <a name="add-an-optimistic-concurrency-property-to-the-department-entity"></a>Dodaj właściwość optymistycznej współbieżności do jednostki działu

W *Models\Department.cs*, dodawanie właściwości śledzenia o nazwie `RowVersion`:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=18-19)]

[Sygnatura czasowa](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx) atrybut określa, że ta kolumna będzie załączona `Where` klauzuli `Update` i `Delete` polecenia wysyłane do bazy danych. Ten atrybut jest wywoływana [sygnatura czasowa](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx) ponieważ poprzednie wersje programu SQL Server SQL [sygnatura czasowa](https://msdn.microsoft.com/library/ms182776(v=SQL.90).aspx) typu danych, zanim SQL [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) zastąpiono ją. Typ architektury .net dla `rowversion` jest tablicą bajtów. Jeśli wolisz używać interfejsu API fluent, możesz użyć [IsConcurrencyToken](https://msdn.microsoft.com/library/gg679501(v=VS.103).aspx) metodę, aby określić właściwości śledzenia, jak pokazano w poniższym przykładzie:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Przez dodanie właściwości po zmianie modelu bazy danych, więc należy przeprowadzić migrację z innej. W konsoli Menedżera pakietów (PMC) wprowadź następujące polecenia:

[!code-console[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cmd)]

## <a name="create-a-department-controller"></a>Tworzenie kontrolera działu

Utwórz `Department` kontrolera i widoki taki sam sposób czy innych kontrolerów, używając następujących ustawień:

![Add_Controller_dialog_box_for_Department_controller](https://asp.net/media/2578041/Windows-Live-Writer_Handling-C.NET-MVC-Application-7-of-10h1_AFDC_Add_Controller_dialog_box_for_Department_controller_d1d9c788-f970-4d6a-9f5a-1eddc84330b7.png)

W *Controllers\DepartmentController.cs*, Dodaj `using` instrukcji:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

Zmiana "LastName" do "Imię i nazwisko" wszędzie, gdzie w tym pliku (czterech wystąpień) tak, aby listy rozwijane działu administrator będzie zawierać imię i nazwisko instruktora, a nie po prostu nazwisko.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=1)]

Zastąp istniejący kod dla `HttpPost` `Edit` metoda następującym kodem:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

Widok będą przechowywane w oryginalnym `RowVersion` wartość w ukrytym polu. Gdy tworzy integratora modelu `department` wystąpienie tego obiektu będzie miał oryginalny `RowVersion` wartości właściwości i nowe wartości dla innych właściwości, tak jak zostały wprowadzone przez użytkownika na stronie edycji. Następnie, gdy platforma Entity Framework tworzy SQL `UPDATE` polecenia, czy polecenie będzie zawierać `WHERE` klauzula, która wyszukuje dla wiersza zawierającego oryginalne `RowVersion` wartość.

Jeśli żadne wiersze nie jest narażony na `UPDATE` polecenia (nie wiersze mają oryginalny `RowVersion` wartość), zgłasza Entity Framework `DbUpdateConcurrencyException` wyjątku i kod w `catch` bloku pobiera dotkniętych `Department` jednostki, z wyjątkiem obiekt. Ta jednostka ma zarówno wartość odczytu z bazy danych, jak i nowe wartości wprowadzonej przez użytkownika:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Następnie kod dodaje niestandardowy komunikat o błędzie dla każdej kolumny, która ma inny niż użytkownik wprowadzi na stronie edycji wartości bazy danych:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Dłużej komunikat o błędzie wyjaśnia, co się stało i co należy zrobić:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Na koniec kod ustawia `RowVersion` wartość `Department` pobrać obiektu nową wartość z bazy danych. Ta nowa `RowVersion` wartość zostanie zapisana w ukrytym polu podczas edycji strony zostanie wyświetlony ponownie, a następnie czasu użytkownik klika polecenie **Zapisz**, tylko błędy współbieżności, które się zdarzyć, ponieważ ponowne wyświetlanie strony edycji zostanie przechwycony.

W *Views\Department\Edit.cshtml*, Dodaj pole ukryte, aby zapisać `RowVersion` wartości właściwości, natychmiast po ukryte pole umożliwiające `DepartmentID` właściwości:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cshtml?highlight=17)]

W *Views\Department\Index.cshtml*, Zastąp istniejący kod następującym kodem przenosić łącza wiersza po lewej stronie i Zmień stronę tytuł i nagłówki kolumn do wyświetlenia `FullName` zamiast `LastName` w **Administratora** kolumny:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cshtml)]

## <a name="testing-optimistic-concurrency-handling"></a>Testowanie obsługi optymistycznej współbieżności

Uruchamianie witryny, a następnie kliknij przycisk **działów**:

![Department_Index_page_before_edits](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Kliknij prawym przyciskiem myszy **Edytuj** hiperlink do osoby Kim Abercrombie, a następnie wybierz pozycję **Otwórz na nowej karcie** kliknięcie **Edytuj** hiperlink do osoby Kim Abercrombie. Dwa okna wyświetlania tych samych informacji.

![Department_Edit_page_before_changes](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Zmienianie pola w pierwszym oknie przeglądarki, a następnie kliknij przycisk **Zapisz**.

![Department_Edit_page_1_after_change](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

Przeglądarka wyświetla stronę indeksu wartością zmienione.

![Departments_Index_page_after_first_budget_edit](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Zmiany w dowolnym polu w drugim oknie przeglądarki, a następnie kliknij przycisk **Zapisz**.

![Department_Edit_page_2_after_change](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

Kliknij przycisk **Zapisz** w drugim oknie przeglądarki. Zobaczysz komunikat o błędzie:

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

Kliknij przycisk **Zapisz** ponownie. Wprowadzona w przeglądarce druga wartość są zapisywane wraz z oryginalnej wartości danych, które możesz zmienić w przeglądarce pierwszy. Zobaczysz zapisane wartości, gdy zostanie wyświetlona strona indeksu.

![Department_Index_page_with_change_from_second_browser](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)

## <a name="updating-the-delete-page"></a>Aktualizowanie strony Delete

Na stronie usuwania programu Entity Framework wykrywa konfliktów współbieżności spowodowanych przez osoby z działu inne do edycji w podobny sposób. Gdy `HttpGet` `Delete` metoda Wyświetla widok potwierdzenie, widok zawiera oryginalny `RowVersion` wartość w ukrytym polu. Wartość jest następnie udostępniana `HttpPost` `Delete` metodę, która jest wywoływana, gdy użytkownik potwierdzi usunięcie. Gdy platforma Entity Framework tworzy SQL `DELETE` polecenia obejmuje `WHERE` klauzuli z oryginalnym `RowVersion` wartość. Jeśli wyniki polecenia zero wierszy dotyczy (wiersz został zmieniony po stronie potwierdzenia usunięcia został wyświetlony przesyłane), zwracany jest wyjątek współbieżności, a `HttpGet Delete` metoda jest wywoływana z flagą błąd równa `true` Aby ponownie wyświetlić Strona potwierdzenia komunikatu o błędzie. Istnieje również możliwość, że zero zmienionych wierszy, ponieważ wiersz został usunięty przez innego użytkownika, więc w takim przypadku jest wyświetlany komunikat o błędzie inny.

W *DepartmentController.cs*, Zastąp `HttpGet` `Delete` metoda następującym kodem:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]

Metoda przyjmuje opcjonalny parametr, który wskazuje, czy strona jest są wyświetlane ponownie po błędzie współbieżności. Jeśli ta flaga jest `true`, komunikat o błędzie jest wysyłany do widoku przy użyciu `ViewBag` właściwości.

Zastąp kod w `HttpPost` `Delete` — metoda (o nazwie `DeleteConfirmed`) z następującym kodem:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

W utworzony szkielet kodu, który został zastąpiony ta metoda akceptowane tylko identyfikator rekordu:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

Ten parametr, aby zmiany zostały wprowadzone `Department` wystąpienia jednostki utworzone przez integratora modelu. Daje to użytkownikowi dostęp do `RowVersion` wartość właściwości oprócz klucza rekordu.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

Również zostały zmienione nazwy metody akcji z `DeleteConfirmed` do `Delete`. Utworzony szkielet kodu o nazwie `HttpPost` `Delete` metoda `DeleteConfirmed` zapewnienie `HttpPost` unikatowy podpis metody. (Środowisko CLR wymaga przeciążone metody, aby miała parametry do innej metody). Skoro podpisy są unikatowe, możesz przestrzegaj Konwencji MVC i użyj takiej samej nazwie `HttpPost` i `HttpGet` usunięcie metod.

Jeżeli zostanie przechwycony błąd współbieżności, kod zostanie ponownie strona potwierdzenia usunięcia i zapewnia, że flagę, która wskazuje, że powinien być wyświetlany komunikat o błędzie współbieżności.

W *Views\Department\Delete.cshtml*, Zastąp utworzony szkielet kodu poniższym kodem, który sprawia, że niektóre elementy formatowania zmiany i dodaje pole komunikat o błędzie. Zmiany są wyróżnione.

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml?highlight=9,37,40,45-46)]

Ten kod dodaje komunikat o błędzie między `h2` i `h3` nagłówków:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]

Zastępuje on programy `LastName` z `FullName` w `Administrator` pola:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml)]

Na koniec dodaje ukryte pola dla `DepartmentID` i `RowVersion` właściwości po `Html.BeginForm` instrukcji:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]

Uruchom stronę indeksu działów. Kliknij prawym przyciskiem myszy **Usuń** hiperlink do działu w języku angielskim, a następnie wybierz pozycję **Otwórz w nowym oknie** następnie w pierwszym oknie kliknij **Edytuj** hiperłącza w języku angielskim Dział.

W pierwszym oknie zmienić jedną z wartości, a następnie kliknij przycisk **Zapisz** :

![Department_Edit_page_after_change_before_delete](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

Na stronie indeksu potwierdza zmianę.

![Departments_Index_page_after_budget_edit_before_delete](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image13.png)

W drugim oknie kliknij **Usuń**.

![Department_Delete_confirmation_page_before_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image14.png)

Zostanie wyświetlony komunikat o błędzie współbieżności, a wartości Dział zostaną odświeżone przy użyciu co to jest obecnie dostępna w bazie danych.

![Department_Delete_confirmation_page_with_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)

Jeśli klikniesz **Usuń** ponownie, użytkownik jest przekierowany do strony indeksu, który pokazuje, że dział został usunięty.

## <a name="summary"></a>Podsumowanie

Na tym kończy się wprowadzenie do obsługi konfliktów współbieżności. Aby uzyskać informacji na temat innych sposobów, aby obsłużyć różne scenariusze współbieżności, zobacz [wzorców optymistycznej współbieżności](https://blogs.msdn.com/b/adonet/archive/2011/02/03/using-dbcontext-in-ef-feature-ctp5-part-9-optimistic-concurrency-patterns.aspx) i [Praca z wartościami właściwości](https://blogs.msdn.com/b/adonet/archive/2011/01/30/using-dbcontext-in-ef-feature-ctp5-part-5-working-with-property-values.aspx) na blogu zespołu programu Entity Framework. Następny samouczek przedstawia sposób implementowania Tabela wg hierarchii dziedziczenia dla `Instructor` i `Student` jednostek.

Linki do innych zasobów platformy Entity Framework można znaleźć w [Mapa zawartości dostępu do danych ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Poprzednie](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [dalej](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)

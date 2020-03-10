---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
title: 'Samouczek: używanie procedur asynchronicznych i składowanych z Dr w aplikacji ASP.NET MVC'
description: W tym samouczku pokazano, jak zaimplementować model programowania asynchronicznego i dowiedzieć się, jak korzystać z procedur składowanych.
author: tdykstra
ms.author: riande
ms.date: 01/18/2019
ms.topic: tutorial
ms.assetid: 27d110fc-d1b7-4628-a763-26f1e6087549
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 5612f2f25d06feb904a205505ed8f048d2263266
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78583441"
---
# <a name="tutorial-use-async-and-stored-procedures-with-ef-in-an-aspnet-mvc-app"></a>Samouczek: używanie procedur asynchronicznych i składowanych z Dr w aplikacji ASP.NET MVC

We wcześniejszych samouczkach pokazano, jak odczytywać i aktualizować dane przy użyciu modelu programowania synchronicznego. W tym samouczku przedstawiono sposób implementacji asynchronicznego modelu programowania. Kod asynchroniczny może poprawić wydajność aplikacji, ponieważ ułatwia ona korzystanie z zasobów serwera.

W tym samouczku pokazano również, jak używać procedur składowanych do operacji INSERT, Update i DELETE w jednostce.

Na koniec należy ponownie wdrożyć aplikację na platformie Azure wraz ze wszystkimi zmianami bazy danych, które zostały zaimplementowane od czasu pierwszego wdrożenia.

Na poniższych ilustracjach przedstawiono niektóre ze stron, z którymi będziesz korzystać.

![Strona działy](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Utwórz dział](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

W tym samouczku zostaną wykonane następujące czynności:

> [!div class="checklist"]
> * Informacje o kodzie asynchronicznym
> * Tworzenie kontrolera działu
> * Korzystanie z procedur składowanych
> * Wdrażanie na platformie Azure

## <a name="prerequisites"></a>Wymagania wstępne

* [Aktualizowanie powiązanych danych](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="why-use-asynchronous-code"></a>Dlaczego warto używać kodu asynchronicznego

Serwer sieci web ma ograniczoną liczbę dostępnych wątków, a w sytuacjach, w dużym obciążeniem wszystkie dostępne wątki mogło zostać użyte. Jeśli tak się stanie, serwer nie może przetworzyć nowe żądania aż wątki są zwalniane. Przy użyciu kodu synchronicznego wiele wątków może powiązane, gdy nie są faktycznie czynności wykonują wszelkie prace ponieważ oczekują one operacji We/Wy zakończyć. Za pomocą kodu asynchronicznego podczas procesu jest oczekiwania na we/wy ukończyć, jego wątku jest zwalniana dla serwera na potrzeby przetwarzaniem innych żądań. W efekcie kod asynchroniczny umożliwia efektywniejsze korzystanie z zasobów serwera, a serwer jest włączony do obsługi większej ilości ruchu bez opóźnień.

We wcześniejszych wersjach programu .NET, pisanie i testowanie kodu asynchronicznego jest skomplikowane, podatne na błędy i trudne do debugowania. W przypadku programu .NET 4,5, pisania, testowania i debugowania kodu asynchronicznego jest to znacznie prostsze, dlatego należy zwykle pisać kod asynchroniczny, chyba że masz powód. Kod asynchroniczny wprowadza niewielką ilość narzutów, ale w przypadku niskiego natężenia ruchu zwiększenie wydajności jest mało znaczące

Aby uzyskać więcej informacji o programowaniu asynchronicznym, zobacz [Korzystanie z asynchronicznej obsługi programu .NET 4.5, aby uniknąć blokowania wywołań](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices.md#async).

## <a name="create-department-controller"></a>Utwórz kontroler działu

Utwórz kontroler działu w taki sam sposób, jak w przypadku wcześniejszych kontrolerów, z wyjątkiem tego, zaznacz pole wyboru **Użyj akcji kontrolera asynchronicznego** .

W poniższych podświetlech pokazano, co zostało dodane do kodu synchronicznego dla metody `Index`, aby uczynić ją asynchroniczną:

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=1,4)]

Zostały zastosowane cztery zmiany, aby włączyć asynchroniczne wykonywanie zapytania bazy danych Entity Framework:

- Metoda jest oznaczona za pomocą słowa kluczowego `async`, co informuje kompilator, aby wygenerował wywołania zwrotne dla części treści metody i automatycznie utworzyć obiekt `Task<ActionResult>`, który jest zwracany.
- Typ zwracany został zmieniony z `ActionResult` na `Task<ActionResult>`. Typ `Task<T>` reprezentuje bieżącą współpracę z wynikiem typu `T`.
- Słowo kluczowe `await` zostało zastosowane do wywołania usługi sieci Web. Gdy kompilator widzi to słowo kluczowe, w tle dzieli metodę na dwie części. Pierwsza część jest zakończona operacją, która jest uruchamiana asynchronicznie. Druga część jest umieszczana w metodzie wywołania zwrotnego, która jest wywoływana po zakończeniu operacji.
- Wywołano asynchroniczną wersję metody rozszerzenia `ToList`.

Dlaczego instrukcja `departments.ToList` została zmodyfikowana, ale nie instrukcją `departments = db.Departments`? Przyczyną jest to, że tylko instrukcje, które powodują, że zapytania lub polecenia wysyłane do bazy danych są wykonywane asynchronicznie. Instrukcja `departments = db.Departments` konfiguruje zapytanie, ale zapytanie nie jest wykonywane do momentu wywołania metody `ToList`. W związku z tym tylko Metoda `ToList` jest wykonywana asynchronicznie.

W metodzie `Details` i `HttpGet` `Edit` i `Delete` Metoda `Find` jest taka, która powoduje wysłanie zapytania do bazy danych, dzięki czemu jest to metoda, która jest wykonywana asynchronicznie:

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs?highlight=7)]

W metodach `Create`, `HttpPost Edit`i `DeleteConfirmed` jest to wywołanie metody `SaveChanges`, które powoduje wykonanie polecenia, nie instrukcje, takie jak `db.Departments.Add(department)`, które powodują, że tylko jednostki w pamięci mają być modyfikowane.

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs?highlight=6)]

Otwórz *Views\Department\Index.cshtml*i Zastąp kod szablonu następującym kodem:

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml?highlight=3,5,20-22,36-38)]

Ten kod zmienia tytuł z indeksu na działy, przenosi nazwę administratora z prawej strony i udostępnia pełną nazwę administratora.

W widokach tworzenie, usuwanie, szczegóły i edycja Zmień podpis pola `InstructorID` na "Administrator" tak samo jak w przypadku zmiany pola Nazwa działu na "Wydział" w widokach kursu.

W widokach tworzenie i edytowanie Użyj następującego kodu:

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml)]

W widokach Usuń i szczegóły Użyj następującego kodu:

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml)]

Uruchom aplikację, a następnie kliknij kartę **działy** .

Wszystko działa tak samo jak w przypadku innych kontrolerów, ale w tym kontrolerze wszystkie zapytania SQL są wykonywane asynchronicznie.

Niektóre kwestie, o których należy wiedzieć w przypadku używania programowania asynchronicznego z Entity Framework:

- Kod asynchroniczny nie jest bezpieczny wątkowo. Innymi słowy, innymi słowy, nie należy próbować wykonać wielu operacji równolegle przy użyciu tego samego wystąpienia kontekstu.
- Jeśli chcesz wykorzystać zalety wydajności w kodzie asynchronicznym, upewnij się, że wszystkie używane pakiety biblioteki (na przykład na potrzeby stronicowania) używają również metody asynchronicznej, jeśli wywołują wszelkie Entity Framework metod, które powodują wysłanie zapytań do bazy danych.

## <a name="use-stored-procedures"></a>Korzystanie z procedur składowanych

Niektórzy deweloperzy i przetwarzający wolą używać procedur składowanych do uzyskiwania dostępu do bazy danych. We wcześniejszych wersjach Entity Framework można pobrać dane przy użyciu procedury składowanej przez [wykonanie nieprzetworzonego zapytania SQL](advanced-entity-framework-scenarios-for-an-mvc-web-application.md), ale nie można wydać instrukcji EF używania procedur składowanych dla operacji aktualizacji. W programie Dr 6 można łatwo skonfigurować Code First do korzystania z procedur składowanych.

1. W *DAL\SchoolContext.cs*Dodaj wyróżniony kod do metody `OnModelCreating`.

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=9)]

    Ten kod instruuje Entity Framework, aby używać procedur składowanych do operacji INSERT, Update i DELETE w jednostce `Department`.
2. W konsoli Zarządzanie pakietami wprowadź następujące polecenie:

    `add-migration DepartmentSP`

    Otwórz *migracje\\&lt;sygnatury czasowej&gt;\_DepartmentSP.cs* , aby wyświetlić kod w metodzie `Up`, która tworzy procedury składowane INSERT, Update i DELETE:

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs?highlight=3-4,26-27,42-43)]
3. W konsoli Zarządzanie pakietami wprowadź następujące polecenie:

     `update-database`
4. Uruchom aplikację w trybie debugowania, kliknij kartę **działy** , a następnie kliknij przycisk **Utwórz nowy**.
5. Wprowadź dane dla nowego działu, a następnie kliknij przycisk **Utwórz**.

6. W programie Visual Studio zapoznaj się z dziennikami w oknie **danych wyjściowych** , aby zobaczyć, że procedura składowana została użyta do wstawienia nowego wiersza działu.

     ![Wstawianie działu SP](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Code First tworzy nazwy domyślnej procedury przechowywanej. W przypadku korzystania z istniejącej bazy danych może być konieczne dostosowanie nazw procedur składowanych, aby można było używać procedur składowanych zdefiniowanych już w bazie danych programu. Aby uzyskać informacje o tym, jak to zrobić, zobacz [Entity Framework Code First Wstawianie/aktualizowanie/usuwanie procedur składowanych](https://msdn.microsoft.com/data/dn468673).

Jeśli chcesz dostosować wygenerowane procedury składowane, możesz edytować kod szkieletowy dla metody `Up` migracji, która tworzy procedurę składowaną. W ten sposób zmiany zostaną odzwierciedlone za każdym razem, gdy migracja zostanie uruchomiona i zostanie zastosowana do produkcyjnej bazy danych, gdy migracja zostanie uruchomiona automatycznie w środowisku produkcyjnym po wdrożeniu.

Jeśli chcesz zmienić istniejącą procedurę składowaną, która została utworzona w poprzedniej migracji, możesz użyć polecenia Add-Migration do wygenerowania pustej migracji, a następnie ręcznie napisać kod, który wywołuje metodę [AlterStoredProcedure](https://msdn.microsoft.com/library/system.data.entity.migrations.dbmigration.alterstoredprocedure.aspx) .

## <a name="deploy-to-azure"></a>Wdrażanie na platformie Azure

Ta sekcja wymaga wykonania opcjonalnej sekcji **wdrażanie aplikacji do platformy Azure** w samouczku [migracja i wdrażanie](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md) tej serii. W przypadku migracji błędów, które zostały rozwiązane przez usunięcie bazy danych w projekcie lokalnym, Pomiń tę sekcję.

1. W programie Visual Studio kliknij prawym przyciskiem myszy projekt w **Eksplorator rozwiązań** i wybierz polecenie **Publikuj** z menu kontekstowego.
2. Kliknij przycisk **Opublikuj**.

    Program Visual Studio wdroży aplikację na platformie Azure, a aplikacja zostanie otwarta w domyślnej przeglądarce działającej na platformie Azure.
3. Przetestuj aplikację, aby upewnić się, że działa.

    Przy pierwszym uruchomieniu strony, która uzyskuje dostęp do bazy danych, Entity Framework uruchamia wszystkie migracje `Up` metody wymagane w celu przeprowadzenia Aktualności bazy danych z bieżącym modelem danych. Teraz możesz użyć wszystkich stron sieci Web, które zostały dodane od czasu ostatniego wdrożenia, łącznie ze stronami działu, które zostały dodane w tym samouczku.

## <a name="get-the-code"></a>Uzyskiwanie kodu

[Pobierz ukończony projekt](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Dodatkowe zasoby

Linki do innych zasobów Entity Framework można znaleźć w [zasobach zalecanych przez dostęp do danych ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

## <a name="next-steps"></a>Następne kroki

W tym samouczku zostaną wykonane następujące czynności:

> [!div class="checklist"]
> * Informacje o kodzie asynchronicznym
> * Utworzono kontroler działu
> * Używane procedury składowane
> * Wdrożone na platformie Azure

Przejdź do następnego artykułu, aby dowiedzieć się, jak obsłużyć konflikty, gdy wielu użytkowników aktualizuje tę samą jednostkę w tym samym czasie.
> [!div class="nextstepaction"]
> [Obsługa współbieżności](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)

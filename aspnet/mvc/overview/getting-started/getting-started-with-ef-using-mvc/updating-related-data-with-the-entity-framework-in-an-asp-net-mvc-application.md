---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: 'Samouczek: aktualizowanie powiązanych danych za pomocą EF w aplikacji ASP.NET MVC'
description: W tym samouczku opisano aktualizowanie powiązanych danych. W przypadku większości relacji można to zrobić przez zaktualizowanie pól klucza obcego lub właściwości nawigacji.
author: tdykstra
ms.author: riande
ms.date: 01/19/2019
ms.topic: tutorial
ms.assetid: 7ba88418-5d0a-437d-b6dc-7c3816d4ec07
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 4d5f6447fdccefdcdf9497a9e94f23243302a0e1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78616005"
---
# <a name="tutorial-update-related-data-with-ef-in-an-aspnet-mvc-app"></a>Samouczek: aktualizowanie powiązanych danych za pomocą EF w aplikacji ASP.NET MVC

W poprzednim samouczku Wyświetlono powiązane dane. W tym samouczku opisano aktualizowanie powiązanych danych. W przypadku większości relacji można to zrobić przez zaktualizowanie pól klucza obcego lub właściwości nawigacji. W przypadku relacji wiele-do-wielu Entity Framework nie uwidacznia tabeli sprzężenia bezpośrednio, dlatego można dodawać i usuwać jednostki do i z odpowiednich właściwości nawigacji.

Na poniższych ilustracjach przedstawiono niektóre ze stron, z którymi będziesz korzystać.

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

![Instruktor Edytuj kursy](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

W tym samouczku zostaną wykonane następujące czynności:

> [!div class="checklist"]
> * Dostosowywanie stron kursów
> * Dodaj pakiet Office do strony instruktorów
> * Dodawanie kursów do strony instruktorów
> * Aktualizacja DeleteConfirmed
> * Dodawanie lokalizacji i kursów biura do strony tworzenia

## <a name="prerequisites"></a>Wymagania wstępne

* [Odczytywanie powiązanych danych](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="customize-courses-pages"></a>Dostosowywanie stron kursów

Po utworzeniu nowej jednostki kursu musi ona mieć relację z istniejącym działem. Aby to ułatwić, kod szkieletowy obejmuje metody kontrolera oraz tworzenie i edytowanie widoków zawierających listę rozwijaną umożliwiającą wybranie działu. Lista rozwijana ustawia `Course.DepartmentID` właściwość klucza obcego i to wszystko Entity Framework potrzeby, aby załadować właściwość nawigacji `Department` z odpowiednią jednostką `Department`. Użyjesz kodu szkieletowego, ale nieco zmień go, aby dodać obsługę błędów i posortować listę rozwijaną.

W *CourseController.cs*usuń cztery `Create` i `Edit` metody i zastąp je następującym kodem:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,11-12,19-25,40,44,46,48-78)]

Dodaj następującą instrukcję `using` na początku pliku:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Metoda `PopulateDepartmentsDropDownList` pobiera listę wszystkich działów posortowanych według nazwy, tworzy kolekcję `SelectList` dla listy rozwijanej i przekazuje kolekcję do widoku we właściwości `ViewBag`. Metoda akceptuje opcjonalny parametr `selectedDepartment`, który umożliwia kod wywołujący, aby określić element, który zostanie wybrany, gdy zostanie wyrenderowana lista rozwijana. Widok przekaże nazwę `DepartmentID` do pomocnika [DropDownList](../../older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc.md) , a pomocnik wie, że szuka w obiekcie `ViewBag` `SelectList` o nazwie `DepartmentID`.

Metoda `HttpGet` `Create` wywołuje metodę `PopulateDepartmentsDropDownList` bez ustawiania wybranego elementu, ponieważ dla nowego kursu nie ustanowiono jeszcze działu:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

Metoda `HttpGet` `Edit` ustawia wybrany element na podstawie identyfikatora działu, który jest już przypisany do edytowanego kursu:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=12)]

Metody `HttpPost` dla obu `Create` i `Edit` zawierają również kod, który ustawia wybrany element, gdy ponownie wyświetla stronę po błędzie:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=6)]

Ten kod gwarantuje, że po wyświetleniu strony w celu wyświetlenia komunikatu o błędzie zostanie zaznaczona żadna z nich.

Widoki kursów są już szkieletem z listami rozwijanymi dla pola dział, ale nie chcesz podpisać DepartmentID dla tego pola, więc w celu zmiany podpisu należy wprowadzić następujące zmiany w pliku *Views\Course\Create.cshtml* .

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml?highlight=43)]

Wprowadź tę samą zmianę w *Views\Course\Edit.cshtml*.

Zwykle szkielet nie tworzy szkieletu klucza podstawowego, ponieważ wartość klucza jest generowana przez bazę danych i nie można jej zmienić i nie jest zrozumiałą wartością wyświetlaną dla użytkowników. W przypadku jednostek kursu szkielet zawiera pole tekstowe dla `CourseID` pola, ponieważ rozumie, że atrybut `DatabaseGeneratedOption.None` oznacza, że użytkownik powinien mieć możliwość wprowadzenia wartości klucza podstawowego. Ale nie rozumie to, że liczba jest istotna, którą chcesz zobaczyć w innych widokach, więc musisz ją dodać ręcznie.

W *Views\Course\Edit.cshtml*Dodaj pole numer kursu przed polem **title** . Ponieważ jest to klucz podstawowy, jest wyświetlany, ale nie można go zmienić.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cshtml)]

Istnieje już pole ukryte (Pomocnik`Html.HiddenFor`) dla numeru kursu w widoku edycji. Dodanie pomocnika *HTML. LabelFor* nie eliminuje potrzeby ukrytego pola, ponieważ nie powoduje, że numer kursu ma być uwzględniony w opublikowanych danych, gdy użytkownik kliknie przycisk **Zapisz** na stronie Edycja.

W *Views\Course\Delete.cshtml* i *Views\Course\Details.cshtml*Zmień napis Nazwa działu z "nazwa" na "Wydział" i Dodaj pole numer kursu przed polem **tytułu** .

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cshtml?highlight=2,9-15)]

Uruchom stronę **Tworzenie** (Wyświetl indeks kursu i kliknij pozycję **Utwórz nową**) i wprowadź dane, aby utworzyć nowy kurs:

| Wartość | Ustawienie |
| ----- | ------- |
| Liczba | Wprowadź *1000*. |
| Tytuł | Wprowadź *algebry*. |
| Środki | Wprowadź *4*. |
|Dział | Wybierz pozycję **matematyka**. |

Kliknij przycisk **Utwórz**. Zostanie wyświetlona strona indeks kursu z nowym kursem, który został dodany do listy. Nazwa działu na liście stron indeksu pochodzi z właściwości nawigacji, co oznacza, że relacja została prawidłowo ustanowiona.

Uruchom stronę **Edytowanie** (Wyświetl stronę indeks kursu i kliknij pozycję **Edytuj** na kursie).

Zmień dane na stronie i kliknij przycisk **Zapisz**. Zostanie wyświetlona strona indeks kursu z zaktualizowanymi danymi kursu.

## <a name="add-office-to-instructors-page"></a>Dodaj pakiet Office do strony instruktorów

Podczas edytowania rekordu instruktora chcesz mieć możliwość aktualizowania przypisania biura instruktora. Jednostka `Instructor` ma relację jeden do zera lub jeden z jednostką `OfficeAssignment`, co oznacza, że należy obsługiwać następujące sytuacje:

- Jeśli użytkownik wyczyści przypisanie pakietu Office i początkowo miał wartość, należy usunąć i usunąć jednostkę `OfficeAssignment`.
- Jeśli użytkownik wprowadzi wartość przypisania pakietu Office i początkowo była pusta, należy utworzyć nową jednostkę `OfficeAssignment`.
- Jeśli użytkownik zmieni wartość przypisania pakietu Office, należy zmienić wartość w istniejącej jednostce `OfficeAssignment`.

Otwórz *InstructorController.cs* i przyjrzyj się metodzie `Edit` `HttpGet`:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Kod szkieletowy nie jest tym, co chcesz. Konfiguruje dane dla listy rozwijanej, ale potrzebne jest pole tekstowe. Zastąp tę metodę następującym kodem:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs?highlight=7-10)]

Ten kod odrzuca instrukcję `ViewBag` i dodaje eager ładowania dla skojarzonej jednostki `OfficeAssignment`. Nie można wykonać ładowania eager z użyciem metody `Find`, więc zamiast tego są używane metody `Where` i `Single` w celu wybrania instruktora.

Zastąp metodę `Edit` `HttpPost` poniższym kodem. obsługujące aktualizacje przypisywania pakietu Office:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

Odwołanie do `RetryLimitExceededException` wymaga instrukcji `using`; Aby dodać go — Umieść wskaźnik myszy na `RetryLimitExceededException`. Zostanie wyświetlony następujący komunikat: ![ komunikat o wyjątku ponowienia próby](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image13.png)

Wybierz pozycję **Pokaż potencjalne poprawki**, a następnie **Użyj elementu System. Data. Entity. Infrastructure**

![Rozwiąż wyjątek ponownych prób](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image14.png)

Kod wykonuje następujące czynności:

- Zmienia nazwę metody na `EditPost`, ponieważ sygnatura jest teraz taka sama jak Metoda `HttpGet` (atrybut `ActionName` określa, że adres URL/Edit/jest nadal używany).
- Pobiera bieżącą jednostkę `Instructor` z bazy danych przy użyciu eager ładowania dla właściwości nawigacji `OfficeAssignment`. Jest to takie samo, jak w przypadku metody `Edit` `HttpGet`.
- Aktualizuje pobraną jednostkę `Instructor` przy użyciu wartości ze spinacza modelu. Używane Przeciążenie [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.108).aspx) umożliwia *dozwolonych* właściwości, które mają zostać uwzględnione. Pozwala to uniknąć nadmiernego księgowania, jak wyjaśniono w [drugim samouczku](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md).

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]
- Jeśli lokalizacja biura jest pusta, ustawia właściwość `Instructor.OfficeAssignment` na wartość null, aby pokrewny wiersz w tabeli `OfficeAssignment` zostanie usunięty.

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]
- Zapisuje zmiany w bazie danych.

W *Views\Instructor\Edit.cshtml*, po `div` elementów dla pola **Data zatrudnienia** Dodaj nowe pole do edycji lokalizacji biura:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml)]

Uruchom stronę (Wybierz kartę **Instruktorzy** , a następnie kliknij przycisk **Edytuj** w instruktorze). Zmień **lokalizację biura** , a następnie kliknij przycisk **Zapisz**.

## <a name="add-courses-to-instructors-page"></a>Dodawanie kursów do strony instruktorów

Instruktorzy mogą uczyć się dowolnej liczby kursów. Teraz poprawisz stronę Edytowanie instruktora, dodając możliwość zmiany przypisań kursu przy użyciu grupy pól wyboru.

Relacja między jednostkami `Course` i `Instructor` jest wiele-do-wielu, co oznacza, że nie masz bezpośredniego dostępu do właściwości klucza obcego, które znajdują się w tabeli sprzężenia. Zamiast tego należy dodawać i usuwać jednostki do i z właściwości nawigacji `Instructor.Courses`.

Interfejs użytkownika, który umożliwia zmianę kursów, do których jest przypisany instruktor, jest grupą pól wyboru. Zostanie wyświetlone pole wyboru dla każdego kursu w bazie danych, a są wybrane te, do których jest przypisany instruktor. Użytkownik może zaznaczyć lub wyczyścić pola wyboru, aby zmienić przypisania kursu. Jeśli liczba kursów była znacznie większa, prawdopodobnie chcesz użyć innej metody prezentacji danych w widoku, ale w celu utworzenia lub usunięcia relacji Użyj tej samej metody manipulowania właściwościami nawigacji.

Aby zapewnić dane do widoku listy pól wyboru, należy użyć klasy model widoku. Utwórz *AssignedCourseData.cs* w folderze *modele widoków* i Zastąp istniejący kod następującym kodem:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

W *InstructorController.cs*Zastąp metodę `Edit` `HttpGet` poniższym kodem. Zmiany są wyróżnione.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs?highlight=9,12,20-35)]

Kod dodaje eager ładowania dla właściwości nawigacji `Courses` i wywołuje nową metodę `PopulateAssignedCourseData`, aby podać informacje dla tablicy pól wyboru przy użyciu klasy modelu widoku `AssignedCourseData`.

Kod w metodzie `PopulateAssignedCourseData` odczytuje wszystkie jednostki `Course` w celu załadowania listy kursów przy użyciu klasy model widoku. Dla każdego kursu kod sprawdza, czy kurs istnieje we właściwości nawigacji `Courses` instruktora. Aby utworzyć efektywne wyszukiwanie podczas sprawdzania, czy kurs jest przypisany do instruktora, kursy przypisane do instruktora są umieszczane w kolekcji [HashSet —](https://msdn.microsoft.com/library/bb359438.aspx) . Właściwość `Assigned` jest ustawiona na `true` dla kursów, które są przypisywane przez instruktora. Widok użyje tej właściwości, aby określić, które pola wyboru muszą być wyświetlane jako wybrane. Na koniec lista jest przenoszona do widoku we właściwości `ViewBag`.

Następnie Dodaj kod, który jest wykonywany, gdy użytkownik kliknie przycisk **Zapisz**. Zastąp metodę `EditPost` poniższym kodem, który wywołuje nową metodę, która aktualizuje `Courses` właściwość nawigacji jednostki `Instructor`. Zmiany są wyróżnione.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs?highlight=3,11,25,37,40-68)]

Sygnatura metody różni się od metody `Edit` `HttpGet`, więc nazwa metody zmienia się z `EditPost` z powrotem na `Edit`.

Ponieważ widok nie zawiera kolekcji `Course` jednostek, segregator modelu nie może automatycznie zaktualizować właściwości nawigacji `Courses`. Zamiast używać spinacza modelu do aktualizowania właściwości nawigacji `Courses`, należy to zrobić w nowej `UpdateInstructorCourses` metodzie. W związku z tym należy wykluczyć Właściwość `Courses` z powiązania modelu. Nie wymaga to żadnych zmian w kodzie, który wywołuje [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.98).aspx) , ponieważ jest używane Przeciążenie *listy dozwolonych* i `Courses` nie znajduje się na liście dołączania.

Jeśli nie wybrano żadnych pól wyboru, kod w `UpdateInstructorCourses` inicjuje właściwość nawigacji `Courses` z pustą kolekcją:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

Kod następnie przechodzi między wszystkimi kursami w bazie danych i sprawdza każdy kurs w odniesieniu do tych, które są aktualnie przypisane do instruktora, a także do tych, które zostały wybrane w widoku. Aby ułatwić efektywne wyszukiwanie, te dwie kolekcje są przechowywane w `HashSet` obiektów.

Jeśli pole wyboru dla kursu zostało wybrane, ale kurs nie znajduje się w `Instructor.Courses` właściwości nawigacji, kurs zostanie dodany do kolekcji we właściwości nawigacji.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

Jeśli nie wybrano pola wyboru dla kursu, ale kurs jest we właściwości nawigacji `Instructor.Courses`, kurs zostanie usunięty z właściwości nawigacji.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs)]

W *Views\Instructor\Edit.cshtml*Dodaj pole **kursów** z tablicą pól wyboru, dodając następujący kod bezpośrednio po `div` elementów dla pola `OfficeAssignment` i przed elementem `div` dla przycisku **Zapisz** :

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml)]

Po wklejeniu kodu, jeśli podziały wierszy i wcięcia nie wyglądają jak tutaj, należy ręcznie naprawić wszystko tak, aby wyglądało na to, co widzisz w tym miejscu. Wcięcia nie muszą być doskonałe, ale `@</tr><tr>`, `@:<td>`, `@:</td>`i `@</tr>` muszą znajdować się w jednym wierszu, jak pokazano, lub wystąpi błąd w czasie wykonywania.

Ten kod tworzy tabelę HTML z trzema kolumnami. W każdej kolumnie jest pole wyboru z podpisem zawierającym numer i tytuł kursu. Wszystkie pola wyboru mają taką samą nazwę ("selectedCourses"), która informuje spinacz modelu, że mają być traktowane jako Grupa. Atrybut `value` każdego pola wyboru jest ustawiany na wartość `CourseID.` podczas ogłaszania strony, spinacz modelu przekazuje tablicę do kontrolera, który składa się z wartości `CourseID` tylko dla wybranych pól wyboru.

Gdy pola wyboru są wstępnie renderowane, te, które są przeznaczone dla kursów przypisanych do instruktora, mają `checked` atrybuty, które wybierają je (sprawdza zaznaczone).

Po zmianie przypisań kursu należy mieć możliwość zweryfikowania zmian wprowadzonych po powrocie lokacji na stronę `Index`. W związku z tym należy dodać kolumnę do tabeli na tej stronie. W takim przypadku nie trzeba używać obiektu `ViewBag`, ponieważ informacje, które mają być wyświetlane, są już właściwością nawigacji `Courses` jednostki `Instructor`, która jest przekazywana do strony jako model.

W *Views\Instructor\Index.cshtml*Dodaj nagłówek **kursów** bezpośrednio po nagłówku **biura** , jak pokazano w następującym przykładzie:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cshtml?highlight=6)]

Następnie Dodaj nową komórkę szczegółów bezpośrednio po komórce szczegółów lokalizacji pakietu Office:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml?highlight=7-14)]

Uruchom stronę **indeks instruktora** , aby zobaczyć kursy przypisane do każdego instruktora.

Kliknij przycisk **Edytuj** w instruktorze, aby wyświetlić stronę Edycja.

Zmień niektóre przypisania kursu, a następnie kliknij przycisk **Zapisz**. Wprowadzone zmiany zostaną odzwierciedlone na stronie indeksu.

 Uwaga: podejście podjęte tutaj do edytowania danych kursu instruktora działa dobrze, gdy istnieje ograniczona liczba kursów. W przypadku kolekcji, które są znacznie większe, będzie wymagane inne interfejs użytkownika i inna metoda aktualizacji.

## <a name="update-deleteconfirmed"></a>Aktualizacja DeleteConfirmed

W *InstructorController.cs*usuń metodę `DeleteConfirmed` i Wstaw w jej miejscu następujący kod.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample24.cs?highlight=5-8,12-18)]

Ten kod wprowadza następujące zmiany:

- Jeśli instruktor zostanie przypisany jako administrator dowolnego działu, program usunie przypisanie instruktora z tego działu. Bez tego kodu podczas próby usunięcia instruktora, który został przypisany jako administrator dla działu, wystąpi błąd integralności referencyjnej.

Ten kod nie obsługuje scenariusza jednego instruktora przypisanego jako administrator dla wielu działów. W ostatnim samouczku dodasz kod, który uniemożliwia wykonanie tego scenariusza.

## <a name="add-office-location-and-courses-to-the-create-page"></a>Dodawanie lokalizacji i kursów biura do strony tworzenia

W *InstructorController.cs*Usuń `HttpGet` i `HttpPost` `Create` metody, a następnie Dodaj następujący kod w ich miejscu:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample25.cs)]

Ten kod jest podobny do tego, co wydano dla metod edycji, z wyjątkiem tego, że nie wybrano kursów. Metoda `HttpGet` `Create` wywołuje metodę `PopulateAssignedCourseData` nie, ponieważ mogą istnieć wybrane kursy, ale w celu zapewnienia pustej kolekcji dla pętli `foreach` w widoku (w przeciwnym razie kod widoku zgłosi wyjątek odwołania o wartości null).

Metoda HttpPost Create dodaje każdy wybrany kurs do właściwości nawigacji kursy przed kodem szablonu sprawdzającym błędy walidacji i dodaje nowy instruktor do bazy danych. Kursy są dodawane nawet wtedy, gdy występują błędy modelu, aby w przypadku wystąpienia błędów modelu (na przykład użytkownik określił nieprawidłową datę), dzięki czemu gdy strona zostanie ponownie wyświetlona przy użyciu komunikatu o błędzie, wszystkie dokonane wybory kursów zostaną automatycznie przywrócone.

Zwróć uwagę, że w celu dodania kursów do właściwości nawigacji `Courses` musisz zainicjować właściwość jako pustą kolekcję:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample26.cs)]

Alternatywnie, aby to zrobić w kodzie kontrolera, można to zrobić w modelu instruktora poprzez zmianę metody pobierającej właściwości na automatyczne utworzenie kolekcji, jeśli nie istnieje, jak pokazano w następującym przykładzie:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample27.cs)]

Jeśli zmodyfikujesz Właściwość `Courses` w ten sposób, możesz usunąć jawny kod inicjalizacji właściwości w kontrolerze.

W *Views\Instructor\Create.cshtml*, Dodaj pola tekstowe lokalizacji biurowej i pola wyboru kursu po polu Data zatrudnienia i przed przyciskiem **Prześlij** .

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample28.cshtml)]

Po wklejeniu kodu Popraw podziały wierszy i wcięcie tak samo jak wcześniej dla strony edycji.

Uruchom stronę tworzenie i Dodaj instruktora.

<a id="transactions"></a>

## <a name="handling-transactions"></a>Obsługa transakcji

Zgodnie z opisem w [samouczku podstawowej funkcjonalności CRUD](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)Entity Framework domyślnie niejawnie implementują transakcje. W przypadku scenariuszy, w których potrzebna jest większa kontrola — na przykład jeśli chcesz uwzględnić operacje wykonywane poza Entity Framework w transakcji — zobacz [Praca z transakcjami](https://msdn.microsoft.com/data/dn456843) w witrynie MSDN.

## <a name="get-the-code"></a>Uzyskiwanie kodu

[Pobierz ukończony projekt](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Dodatkowe zasoby

Linki do innych zasobów Entity Framework można znaleźć w temacie [ASP.NET Data Access — zalecane zasoby](../../../../whitepapers/aspnet-data-access-content-map.md).

## <a name="next-step"></a>Następny krok

W tym samouczku zostaną wykonane następujące czynności:

> [!div class="checklist"]
> * Strony kursów niestandardowych
> * Dodano pakiet Office do strony instruktorów
> * Dodano kursy do strony instruktorów
> * Zaktualizowano DeleteConfirmed
> * Dodano lokalizację i kursy biura do strony tworzenia

Przejdź do następnego artykułu, aby dowiedzieć się, jak zaimplementować asynchroniczny model programowania.
> [!div class="nextstepaction"]
> [Asynchroniczny model programowania](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application.md)

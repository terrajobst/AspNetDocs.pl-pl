---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: Aktualizowanie powiązanych danych za pomocą Entity Framework w aplikacji ASP.NET MVC (6 z 10) | Microsoft Docs
author: tdykstra
description: Przykładowa aplikacja internetowa Contoso University pokazuje, jak tworzyć aplikacje ASP.NET MVC 4 przy użyciu Code First Entity Framework 5 i programu Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 7871dc05-2750-470f-8b4c-3a52511949bc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: d29cb172d642b67947b461d1a7e55d01872bb8c2
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74592440"
---
# <a name="updating-related-data-with-the-entity-framework-in-an-aspnet-mvc-application-6-of-10"></a>Aktualizowanie powiązanych danych za pomocą Entity Framework w aplikacji ASP.NET MVC (6 z 10)

Autor [Dykstra](https://github.com/tdykstra)

[Pobierz ukończony projekt](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Przykładowa aplikacja internetowa Contoso University pokazuje, jak tworzyć aplikacje ASP.NET MVC 4 przy użyciu Entity Framework 5 Code First i programu Visual Studio 2012. Aby uzyskać informacje na temat serii samouczków, zobacz [pierwszy samouczek w serii](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Możesz uruchomić serię samouczków od początku lub [pobrać początkowy projekt dla tego rozdziału](building-the-ef5-mvc4-chapter-downloads.md) i Zacznij tutaj.
> 
> > [!NOTE] 
> > 
> > Jeśli wystąpi problem, którego nie można rozwiązać, [Pobierz ukończony rozdział](building-the-ef5-mvc4-chapter-downloads.md) i spróbuj odtworzyć problem. Ogólnie rzecz biorąc, można znaleźć rozwiązanie problemu, porównując kod z kompletnym kodem. Niektóre typowe błędy i sposoby ich rozwiązywania można znaleźć w temacie [błędy i obejścia.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)

W poprzednim samouczku Wyświetlono powiązane dane; w tym samouczku opisano aktualizowanie powiązanych danych. W przypadku większości relacji można to zrobić, aktualizując odpowiednie pola klucza obcego. W przypadku relacji wiele-do-wielu Entity Framework nie uwidacznia tabeli sprzężenia bezpośrednio, dlatego należy jawnie dodawać i usuwać jednostki do i z odpowiednich właściwości nawigacji.

Na poniższych ilustracjach przedstawiono strony, z którymi będziesz korzystać.

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a>Dostosowywanie stron tworzenia i edytowania dla kursów

Po utworzeniu nowej jednostki kursu musi ona mieć relację z istniejącym działem. Aby to ułatwić, kod szkieletowy obejmuje metody kontrolera oraz tworzenie i edytowanie widoków zawierających listę rozwijaną umożliwiającą wybranie działu. Lista rozwijana ustawia `Course.DepartmentID` właściwość klucza obcego i to wszystko Entity Framework potrzeby, aby załadować właściwość nawigacji `Department` z odpowiednią jednostką `Department`. Użyjesz kodu szkieletowego, ale nieco zmień go, aby dodać obsługę błędów i posortować listę rozwijaną.

W *CourseController.cs*usuń cztery `Edit` i `Create` metody i zastąp je następującym kodem:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,10,13-14,21-27,34,41,44-45,52-58,62-68)]

Metoda `PopulateDepartmentsDropDownList` pobiera listę wszystkich działów posortowanych według nazwy, tworzy kolekcję `SelectList` dla listy rozwijanej i przekazuje kolekcję do widoku we właściwości `ViewBag`. Metoda akceptuje opcjonalny parametr `selectedDepartment`, który umożliwia kod wywołujący, aby określić element, który zostanie wybrany, gdy zostanie wyrenderowana lista rozwijana. Widok przekaże nazwę `DepartmentID` do [pomocnika `DropDownList`](../working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc.md), a pomocnik wie, że w obiekcie `ViewBag` `SelectList` o nazwie `DepartmentID`.

Metoda `HttpGet` `Create` wywołuje metodę `PopulateDepartmentsDropDownList` bez ustawiania wybranego elementu, ponieważ dla nowego kursu nie ustanowiono jeszcze działu:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Metoda `HttpGet` `Edit` ustawia wybrany element na podstawie identyfikatora działu, który jest już przypisany do edytowanego kursu:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

Metody `HttpPost` dla obu `Create` i `Edit` zawierają również kod, który ustawia wybrany element, gdy ponownie wyświetla stronę po błędzie:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

Ten kod gwarantuje, że po wyświetleniu strony w celu wyświetlenia komunikatu o błędzie zostanie zaznaczona żadna z nich.

W *Views\Course\Create.cshtml*, Dodaj wyróżniony kod, aby utworzyć nowe pole numeru kursu przed polem **tytułu** . Zgodnie z opisem we wcześniejszym samouczku pola klucza podstawowego nie są domyślnie szkieletowe, ale ten klucz podstawowy ma znaczenie, więc użytkownik powinien mieć możliwość wprowadzenia wartości klucza.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=17-23)]

W *Views\Course\Edit.cshtml*, *Views\Course\Delete.cshtml*i *Views\Course\Details.cshtml*Dodaj pole numer kursu przed polem **title** . Ponieważ jest to klucz podstawowy, jest wyświetlany, ale nie można go zmienić.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml)]

Uruchom stronę **Tworzenie** (Wyświetl indeks kursu i kliknij pozycję **Utwórz nową**) i wprowadź dane, aby utworzyć nowy kurs:

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Kliknij przycisk **Utwórz**. Zostanie wyświetlona strona indeks kursu z nowym kursem, który został dodany do listy. Nazwa działu na liście stron indeksu pochodzi z właściwości nawigacji, co oznacza, że relacja została prawidłowo ustanowiona.

![Course_Index_page_showing_new_course](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Uruchom stronę **Edytowanie** (Wyświetl stronę indeks kursu i kliknij pozycję **Edytuj** na kursie).

![Course_edit_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Zmień dane na stronie i kliknij przycisk **Zapisz**. Zostanie wyświetlona strona indeks kursu z zaktualizowanymi danymi kursu.

## <a name="adding-an-edit-page-for-instructors"></a>Dodawanie strony edycji dla instruktorów

Podczas edytowania rekordu instruktora chcesz mieć możliwość aktualizowania przypisania biura instruktora. Jednostka `Instructor` ma relację jeden do zera lub jeden z jednostką `OfficeAssignment`, co oznacza, że należy obsługiwać następujące sytuacje:

- Jeśli użytkownik wyczyści przypisanie pakietu Office i początkowo miał wartość, należy usunąć i usunąć jednostkę `OfficeAssignment`.
- Jeśli użytkownik wprowadzi wartość przypisania pakietu Office i początkowo była pusta, należy utworzyć nową jednostkę `OfficeAssignment`.
- Jeśli użytkownik zmieni wartość przypisania pakietu Office, należy zmienić wartość w istniejącej jednostce `OfficeAssignment`.

Otwórz *InstructorController.cs* i przyjrzyj się metodzie `Edit` `HttpGet`:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Kod szkieletowy nie jest tym, co chcesz. Konfiguruje dane dla listy rozwijanej, ale potrzebne jest pole tekstowe. Zastąp tę metodę następującym kodem:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Ten kod odrzuca instrukcję `ViewBag` i dodaje eager ładowania dla skojarzonej jednostki `OfficeAssignment`. Nie można wykonać ładowania eager z użyciem metody `Find`, więc zamiast tego są używane metody `Where` i `Single` w celu wybrania instruktora.

Zastąp metodę `Edit` `HttpPost` poniższym kodem. obsługujące aktualizacje przypisywania pakietu Office:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Kod wykonuje następujące czynności:

- Pobiera bieżącą jednostkę `Instructor` z bazy danych przy użyciu eager ładowania dla właściwości nawigacji `OfficeAssignment`. Jest to takie samo, jak w przypadku metody `Edit` `HttpGet`.
- Aktualizuje pobraną jednostkę `Instructor` przy użyciu wartości ze spinacza modelu. Używane Przeciążenie [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.108).aspx) umożliwia *dozwolonych* właściwości, które mają zostać uwzględnione. Pozwala to uniknąć nadmiernego księgowania, jak wyjaśniono w [drugim samouczku](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md).

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]
- Jeśli lokalizacja biura jest pusta, ustawia właściwość `Instructor.OfficeAssignment` na wartość null, aby pokrewny wiersz w tabeli `OfficeAssignment` zostanie usunięty.

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]
- Zapisuje zmiany w bazie danych.

W *Views\Instructor\Edit.cshtml*, po `div` elementów dla pola **Data zatrudnienia** Dodaj nowe pole do edycji lokalizacji biura:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml)]

Uruchom stronę (Wybierz kartę **Instruktorzy** , a następnie kliknij przycisk **Edytuj** w instruktorze). Zmień **lokalizację biura** , a następnie kliknij przycisk **Zapisz**.

![Changing_the_office_location](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

## <a name="adding-course-assignments-to-the-instructor-edit-page"></a>Dodawanie przypisań kursu do strony edytowania instruktora

Instruktorzy mogą uczyć się dowolnej liczby kursów. Teraz poprawisz stronę Edytowanie instruktora, dodając możliwość zmiany przypisań kursu przy użyciu grupy pól wyboru, jak pokazano na poniższym zrzucie ekranu:

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

Relacja między jednostkami `Course` i `Instructor` jest wiele-do-wielu, co oznacza, że nie masz bezpośredniego dostępu do tabeli sprzężenia. Zamiast tego należy dodawać i usuwać jednostki do i z właściwości nawigacji `Instructor.Courses`.

Interfejs użytkownika, który umożliwia zmianę kursów, do których jest przypisany instruktor, jest grupą pól wyboru. Zostanie wyświetlone pole wyboru dla każdego kursu w bazie danych, a są wybrane te, do których jest przypisany instruktor. Użytkownik może zaznaczyć lub wyczyścić pola wyboru, aby zmienić przypisania kursu. Jeśli liczba kursów była znacznie większa, prawdopodobnie chcesz użyć innej metody prezentacji danych w widoku, ale w celu utworzenia lub usunięcia relacji Użyj tej samej metody manipulowania właściwościami nawigacji.

Aby zapewnić dane do widoku listy pól wyboru, należy użyć klasy model widoku. Utwórz *AssignedCourseData.cs* w folderze *modele widoków* i Zastąp istniejący kod następującym kodem:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

W *InstructorController.cs*Zastąp metodę `Edit` `HttpGet` poniższym kodem. Zmiany są wyróżnione.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs?highlight=5,8,12-27)]

Kod dodaje eager ładowania dla właściwości nawigacji `Courses` i wywołuje nową metodę `PopulateAssignedCourseData`, aby podać informacje dla tablicy pól wyboru przy użyciu klasy modelu widoku `AssignedCourseData`.

Kod w metodzie `PopulateAssignedCourseData` odczytuje wszystkie jednostki `Course` w celu załadowania listy kursów przy użyciu klasy model widoku. Dla każdego kursu kod sprawdza, czy kurs istnieje we właściwości nawigacji `Courses` instruktora. Aby utworzyć efektywne wyszukiwanie podczas sprawdzania, czy kurs jest przypisany do instruktora, kursy przypisane do instruktora są umieszczane w kolekcji [HashSet —](https://msdn.microsoft.com/library/bb359438.aspx) . Właściwość `Assigned` jest ustawiona na `true` dla kursów, które są przypisywane przez instruktora. Widok użyje tej właściwości, aby określić, które pola wyboru muszą być wyświetlane jako wybrane. Na koniec lista jest przenoszona do widoku we właściwości `ViewBag`.

Następnie Dodaj kod, który jest wykonywany, gdy użytkownik kliknie przycisk **Zapisz**. Zastąp metodę `Edit` `HttpPost` poniższym kodem, który wywołuje nową metodę, która aktualizuje właściwość nawigacji `Courses` jednostki `Instructor`. Zmiany są wyróżnione.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs?highlight=3,7,20,33,37-65)]

Ponieważ widok nie zawiera kolekcji `Course` jednostek, segregator modelu nie może automatycznie zaktualizować właściwości nawigacji `Courses`. Zamiast używać spinacza modelu do aktualizowania właściwości nawigacji kursy, należy to zrobić w nowej metodzie `UpdateInstructorCourses`. W związku z tym należy wykluczyć Właściwość `Courses` z powiązania modelu. Nie wymaga to żadnych zmian w kodzie, który wywołuje [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.98).aspx) , ponieważ jest używane Przeciążenie *listy dozwolonych* i `Courses` nie znajduje się na liście dołączania.

Jeśli nie wybrano żadnych pól wyboru, kod w `UpdateInstructorCourses` inicjuje właściwość nawigacji `Courses` z pustą kolekcją:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

Kod następnie przechodzi między wszystkimi kursami w bazie danych i sprawdza każdy kurs w odniesieniu do tych, które są aktualnie przypisane do instruktora, a także do tych, które zostały wybrane w widoku. Aby ułatwić efektywne wyszukiwanie, te dwie kolekcje są przechowywane w `HashSet` obiektów.

Jeśli pole wyboru dla kursu zostało wybrane, ale kurs nie znajduje się w `Instructor.Courses` właściwości nawigacji, kurs zostanie dodany do kolekcji we właściwości nawigacji.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs)]

Jeśli nie wybrano pola wyboru dla kursu, ale kurs jest we właściwości nawigacji `Instructor.Courses`, kurs zostanie usunięty z właściwości nawigacji.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

W *Views\Instructor\Edit.cshtml*Dodaj pole **kursów** z tablicą pól wyboru, dodając następujący wyróżniony kod bezpośrednio po `div` elementach pola `OfficeAssignment`:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml?highlight=51-73)]

Ten kod tworzy tabelę HTML z trzema kolumnami. W każdej kolumnie jest pole wyboru z podpisem zawierającym numer i tytuł kursu. Wszystkie pola wyboru mają taką samą nazwę ("selectedCourses"), która informuje spinacz modelu, że mają być traktowane jako Grupa. Atrybut `value` każdego pola wyboru jest ustawiany na wartość `CourseID.` podczas ogłaszania strony, spinacz modelu przekazuje tablicę do kontrolera, który składa się z wartości `CourseID` tylko dla wybranych pól wyboru.

Gdy pola wyboru są wstępnie renderowane, te, które są przeznaczone dla kursów przypisanych do instruktora, mają `checked` atrybuty, które wybierają je (sprawdza zaznaczone).

Po zmianie przypisań kursu należy mieć możliwość zweryfikowania zmian wprowadzonych po powrocie lokacji na stronę `Index`. W związku z tym należy dodać kolumnę do tabeli na tej stronie. W takim przypadku nie trzeba używać obiektu `ViewBag`, ponieważ informacje, które mają być wyświetlane, są już właściwością nawigacji `Courses` jednostki `Instructor`, która jest przekazywana do strony jako model.

W *Views\Instructor\Index.cshtml*Dodaj nagłówek **kursów** bezpośrednio po nagłówku **biura** , jak pokazano w następującym przykładzie:

[!code-html[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.html?highlight=7)]

Następnie Dodaj nową komórkę szczegółów bezpośrednio po komórce szczegółów lokalizacji pakietu Office:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml?highlight=19,50-57)]

Uruchom stronę **indeks instruktora** , aby zobaczyć kursy przypisane do każdego instruktora:

![Instructor_index_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Kliknij przycisk **Edytuj** w instruktorze, aby wyświetlić stronę Edycja.

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

Zmień niektóre przypisania kursu, a następnie kliknij przycisk **Zapisz**. Wprowadzone zmiany zostaną odzwierciedlone na stronie indeksu.

 Uwaga: podejście podjęte do edytowania danych kursu instruktora działa dobrze, gdy istnieje ograniczona liczba kursów. W przypadku kolekcji, które są znacznie większe, będzie wymagane inne interfejs użytkownika i inna metoda aktualizacji.  

## <a name="update-the-delete-method"></a>Aktualizowanie metody Delete

Zmień kod w metodzie Delete HttpPost, aby rekord przypisania pakietu Office (jeśli istnieje) został usunięty po usunięciu instruktora:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs?highlight=6,10)]

Jeśli spróbujesz usunąć instruktora, który jest przypisany do działu jako administrator, zostanie wyświetlony błąd integralności referencyjnej. Zapoznaj [się z bieżącą wersją tego samouczka](../../getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) , aby uzyskać dodatkowy kod, który automatycznie usunie instruktora z dowolnego działu, w którym instruktor jest przypisany jako administrator.

## <a name="summary"></a>Podsumowanie

Wprowadzenie do pracy z danymi pokrewnymi zostało zakończone. Do tych samouczków zrobiono pełen zakres operacji CRUD, ale nie zawarto żadnych problemów dotyczących współbieżności. W następnym samouczku przedstawiono temat współbieżności, wyjaśnienie opcji obsługi oraz dodanie obsługi współbieżności do kodu CRUD, który został już zapisany dla jednego typu jednostki.

Linki do innych zasobów Entity Framework można znaleźć na końcu [ostatniego samouczka w tej serii](advanced-entity-framework-scenarios-for-an-mvc-web-application.md).

> [!div class="step-by-step"]
> [Poprzednie](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [dalej](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)

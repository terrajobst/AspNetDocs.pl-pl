---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
title: 'Samouczek: Implementowanie funkcji CRUD za pomocą Entity Framework w ASP.NET MVC | Microsoft Docs'
description: Przejrzyj i Dostosuj kod Create, Read, Update, Delete (CRUD), który tworzy szkielet MVC automatycznie w kontrolerach i widokach.
author: tdykstra
ms.author: riande
ms.date: 01/22/2019
ms.topic: tutorial
ms.assetid: a2f70ba4-83d1-4002-9255-24732726c4f2
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 9ed388543dd54d209ff2a0b92df4f7659962582c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78583161"
---
# <a name="tutorial-implement-crud-functionality-with-the-entity-framework-in-aspnet-mvc"></a>Samouczek: Implementowanie funkcji CRUD przy użyciu Entity Framework w ASP.NET MVC

W [poprzednim samouczku](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)utworzono aplikację MVC, która przechowuje i wyświetla dane przy użyciu Entity Framework (EF) 6 i SQL Server LocalDB. W tym samouczku pokazano, jak przeglądać i dostosowywać kod Create, Read, Update, Delete (CRUD), który automatycznie tworzy szkielet MVC dla Ciebie w kontrolerach i widokach.

> [!NOTE]
> Jest to typowa Metoda implementacji wzorca repozytorium w celu utworzenia warstwy abstrakcji między kontrolerem i warstwą dostępu do danych. Aby zachować te samouczki jako proste i ukierunkowane na uczenie się, jak korzystać z programu EF 6, nie korzystają z repozytoriów. Aby uzyskać informacje o sposobach implementowania repozytoriów, zobacz [mapa zawartości ASP.NET Data Access](../../../../whitepapers/aspnet-data-access-content-map.md).

Oto przykłady tworzonych stron sieci Web:

![Zrzut ekranu przedstawiający stronę szczegółów ucznia.](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image1.png)

![Zrzut ekranu przedstawiający stronę tworzenie ucznia.](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image2.png)

![Zrzut ekranu z stroną usuwania ucznia.](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image3.png)

W tym samouczku zostaną wykonane następujące czynności:

> [!div class="checklist"]
> * Utwórz stronę szczegółów
> * Aktualizowanie strony tworzenia
> * Aktualizowanie metody edycji HttpPost
> * Aktualizuj stronę Delete
> * Zamknij połączenia bazy danych
> * Obsługa transakcji

## <a name="prerequisites"></a>Wymagania wstępne

* [Tworzenie modelu danych Entity Framework](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)

## <a name="create-a-details-page"></a>Utwórz stronę szczegółów

Kod szkieletu dla uczniów `Index` stronie, który opuścił Właściwość `Enrollments`, ponieważ ta właściwość zawiera kolekcję. Na stronie `Details` zostanie wyświetlona zawartość kolekcji w tabeli HTML.

W *Controllers\StudentController.cs*Metoda akcji dla widoku `Details` używa metody [Find](https://msdn.microsoft.com/library/gg696418(v=VS.103).aspx) do pobrania pojedynczej jednostki `Student`.

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample1.cs)]

Wartość klucza jest przenoszona do metody jako parametr `id` i pochodzi z *danych trasy* w hiperłączu **szczegóły** na stronie indeks.

### <a name="tip-route-data"></a>Porada: **dane trasy**

Dane trasy to dane, które spinacz modelu znalazł w segmencie adresów URL określonym w tabeli routingu. Na przykład trasa domyślna określa `controller`, `action`i `id` segmentów:

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample2.cs?highlight=3)]

W poniższym adresie URL trasy domyślne są mapowane `Instructor` jako `controller`, `Index` jako `action` i 1 jako `id`; są to wartości danych tras.

`http://localhost:1230/Instructor/Index/1?courseID=2021`

`?courseID=2021` jest wartością ciągu zapytania. Model spinacza również będzie działał w przypadku przekazania `id` jako wartości ciągu zapytania:

`http://localhost:1230/Instructor/Index?id=1&CourseID=2021`

Adresy URL są tworzone przez instrukcje `ActionLink` w widoku Razor. W poniższym kodzie parametr `id` jest zgodny z domyślną trasą, więc `id` jest dodawany do danych trasy.

[!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample3.cshtml)]

W poniższym kodzie `courseID` nie jest zgodny z parametrem w domyślnej trasie, więc jest dodawana jako ciąg zapytania.

[!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample4.cshtml)]

### <a name="to-create-the-details-page"></a>Aby utworzyć stronę szczegółów

1. Otwórz *Views\Student\Details.cshtml*.

   Każde pole jest wyświetlane za pomocą pomocnika `DisplayFor`, jak pokazano w następującym przykładzie:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample5.cshtml)]

2. Po polu `EnrollmentDate` i bezpośrednio przed tagiem zamykającym `</dl>` Dodaj wyróżniony kod, aby wyświetlić listę rejestracji, jak pokazano w następującym przykładzie:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample6.cshtml?highlight=8-29)]

    Jeśli Wcięcie kodu jest nieprawidłowe po wklejeniu kodu, naciśnij klawisz **ctrl**+**K**, **Ctrl**+**D** , aby go sformatować.

    Ten kod pętle za pomocą jednostek we właściwości nawigacji `Enrollments`. Dla każdej jednostki `Enrollment` we właściwości wyświetla tytuł kursu i klasy. Tytuł kursu jest pobierany z jednostki `Course`, która jest przechowywana we właściwości nawigacji `Course` jednostki `Enrollments`. Wszystkie te dane są pobierane z bazy danych automatycznie, gdy jest to konieczne. Inaczej mówiąc, używasz ładowania z opóźnieniem tutaj. Nie określono *eager ładowania* dla właściwości nawigacji `Courses`, więc rejestracje nie zostały pobrane w tym samym zapytaniu, które otrzymało uczniów. Zamiast tego przy pierwszej próbie uzyskania dostępu do właściwości nawigacji `Enrollments` do bazy danych jest wysyłane nowe zapytanie w celu pobrania danych. Więcej informacji na temat ładowania z opóźnieniem i ładowania eager można znaleźć w artykule samouczek dotyczący [odczytywania danych pokrewnych](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) w dalszej części tej serii.

3. Otwórz stronę szczegółów, uruchamiając program (**Ctrl**+**F5**), wybierając kartę **studenci** , a następnie klikając link **szczegóły** dla Alexander Carson. (Jeśli naciśniesz klawisz **Ctrl**+**F5** , podczas gdy plik *details. cshtml* zostanie otwarty, zostanie wyświetlony błąd HTTP 400. Wynika to z faktu, że program Visual Studio próbuje uruchomić stronę szczegółów, ale nie został osiągnięty przy użyciu linku, który określa studenta do wyświetlenia. Jeśli tak się stanie, Usuń "student/Details" z adresu URL i spróbuj ponownie lub, zamknij przeglądarkę, kliknij prawym przyciskiem myszy projekt, a następnie kliknij pozycję **wyświetl** > **Widok w przeglądarce**.)

    Zostanie wyświetlona lista kursów i ocen dla wybranego ucznia.

4. Zamknij okno przeglądarki.

## <a name="update-the-create-page"></a>Aktualizowanie strony tworzenia

1. W *Controllers\StudentController.cs*zastąp `Create` metodę akcji <xref:System.Web.Mvc.HttpPostAttribute> poniższym kodem. Ten kod dodaje blok `try-catch` i usuwa `ID` z atrybutu <xref:System.Web.Mvc.BindAttribute> dla metody szkieletowej:

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample7.cs?highlight=3,5-6,13-18)]

    Ten kod dodaje jednostkę `Student` utworzoną przez spinacz modelu ASP.NET MVC do zestawu jednostek `Students`, a następnie zapisuje zmiany w bazie danych. *Model spinacza* odnosi się do funkcji ASP.NET MVC, która ułatwia pracę z danymi przesyłanymi przez formularz; Model segregatora konwertuje wartości ogłoszonych formularzy na typy CLR i przekazuje je do metody akcji w parametrach. W takim przypadku spinacz modelu tworzy wystąpienie `Student` jednostki za pomocą wartości właściwości z kolekcji `Form`.

    `ID` z atrybutu bind został usunięty, ponieważ `ID` jest wartością klucza podstawowego, która SQL Server zostanie ustawiona automatycznie podczas wstawiania wiersza. Dane wejściowe użytkownika nie ustawiają wartości `ID`.

    <a id="overpost"></a>

    ### <a name="security-warning---the-validateantiforgerytoken-attribute-helps-prevent-cross-site-request-forgery-attacks-it-requires-a-corresponding-htmlantiforgerytoken-statement-in-the-view-which-youll-see-later"></a>Ostrzeżenie o zabezpieczeniach — atrybut `ValidateAntiForgeryToken` pomaga zapobiegać atakom na [fałszerstwo żądań między lokacjami](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) . Wymaga odpowiedniej instrukcji `Html.AntiForgeryToken()` w widoku, która będzie widoczna później.

    Atrybut `Bind` jest jednym ze sposobów na ochronę przed *nadmiernym publikowaniem* w scenariuszach tworzenia. Załóżmy na przykład, że jednostka `Student` zawiera właściwość `Secret`, która nie powinna być ustawiona przez tę stronę sieci Web.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample8.cs?highlight=7)]

    Nawet jeśli nie masz pola `Secret` na stronie sieci Web, haker może użyć narzędzia takiego jak [programu Fiddler](http://fiddler2.com/home)lub napisać kod JavaScript, aby ogłosić `Secret` wartość formularza. Bez atrybutu <xref:System.Web.Mvc.BindAttribute> ograniczając pola, które są używane przez spinacz modelu podczas tworzenia wystąpienia `Student`<em>,</em> spinacz modelu utworzy `Secret` wartość formularza i użyje go do utworzenia wystąpienia jednostki `Student`. Następnie, niezależnie od wartości, haker określony dla pola formularza `Secret` zostanie zaktualizowany w bazie danych. Na poniższej ilustracji przedstawiono Narzędzie programu Fiddler, które dodaje pole `Secret` (z wartością "naddawaj") do wartości zaksięgowanych formularzy.

    ![](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image5.png)

    Wartość "przeznaczenie" zostanie następnie pomyślnie dodana do właściwości `Secret` wstawionego wiersza, mimo że nie ma możliwości ustawienia tej właściwości na stronie sieci Web.

    Najlepiej używać parametru `Include` z atrybutem `Bind` do *dozwolonych* pól. Istnieje również możliwość użycia parametru `Exclude` do pola listy *zablokowanych* , które mają zostać wykluczone. Przyczyna `Include` jest bezpieczniejsze, ponieważ po dodaniu nowej właściwości do jednostki nowe pole nie jest automatycznie chronione przez `Exclude` listę.

    Aby zapobiec nadpisywaniu w scenariuszach edycji, należy najpierw odczytać jednostkę z bazy danych, a następnie wywołać `TryUpdateModel`, przekazując listę jawnie dozwolonych właściwości. Jest to metoda używana w tych samouczkach.

    Alternatywny sposób zapobiegania przechodzeniu, który jest preferowany przez wielu deweloperów, polega na użyciu modeli widoku zamiast klas jednostek z powiązaniem modelu. Uwzględnij tylko właściwości, które chcesz zaktualizować w modelu widoku. Po zakończeniu tworzenia spinacza modelu MVC Skopiuj właściwości modelu widoku do wystąpienia jednostki, opcjonalnie używając narzędzia takiego jak [automapowanie](http://automapper.org/). Użyj bazy danych. Wpis w wystąpieniu jednostki, aby ustawić jego stan na niezmieniony, a następnie ustawić właściwość ("PropertyName"). Właściwość "IsModified" ma wartość true dla każdej właściwości jednostki, która jest uwzględniona w modelu widoku. Ta metoda działa zarówno w scenariuszach edycji, jak i tworzenia.

    Poza atrybutem `Bind`, blok `try-catch` jest jedyną zmianą dokonaną w kodzie szkieletowym. Jeśli wyjątek, który pochodzi z <xref:System.Data.DataException> jest przechwytywany podczas zapisywania zmian, zostanie wyświetlony ogólny komunikat o błędzie. wyjątki <xref:System.Data.DataException> są czasami spowodowane przez coś zewnętrznego dla aplikacji, a nie z powodu błędu programowania, dlatego należy spróbować ponownie. Chociaż nie jest zaimplementowany w tym przykładzie, aplikacja do jakości produkcyjnej mógłby rejestrować wyjątek. Aby uzyskać więcej informacji, zobacz sekcję **log for Insight** w temacie [monitorowanie i telemetrię (Tworzenie aplikacji w chmurze w rzeczywistości na platformie Azure)](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry.md#log).

    Kod w *Views\Student\Create.cshtml* jest podobny do wartości *wydanej w temacie details. cshtml*, z wyjątkiem tego, że `EditorFor` i pomocników `ValidationMessageFor` są używane dla każdego pola zamiast `DisplayFor`. Oto odpowiedni kod:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample9.cshtml)]

    Polecenie *Create. cshtml* zawiera również `@Html.AntiForgeryToken()`, które współdziałają z atrybutem `ValidateAntiForgeryToken` w kontrolerze w celu zapobiegania atakom na ataki [między lokacjami](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) .

    W elemencie *Create. cshtml*nie są wymagane żadne zmiany.

2. Uruchom stronę, uruchamiając program, wybierając kartę **studenci** , a następnie klikając pozycję **Utwórz nowy**.

3. Wprowadź nazwy i nieprawidłową datę, a następnie kliknij przycisk **Utwórz** , aby wyświetlić komunikat o błędzie.

    Jest to weryfikacja po stronie serwera, która jest domyślnie pobierana. W późniejszym samouczku zobaczysz, jak dodać atrybuty generujące kod dla walidacji po stronie klienta. Poniższy wyróżniony kod pokazuje sprawdzanie poprawności modelu w metodzie **Create** .

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample10.cs?highlight=1)]

4. Zmień datę na prawidłową wartość, a następnie kliknij pozycję **Utwórz** , aby zobaczyć, że nowy student zostanie wyświetlony na stronie **indeks** .

5. Zamknij okno przeglądarki.

## <a name="update-httppost-edit-method"></a>Aktualizowanie metody edycji HttpPost

1. Zastąp <xref:System.Web.Mvc.HttpPostAttribute> `Edit` metodę akcji następującym kodem:

   [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample11.cs)]

   > [!NOTE]
   > W *Controllers\StudentController.cs*Metoda `HttpGet Edit` (taka bez atrybutu `HttpPost`) używa metody `Find` do pobrania wybranej jednostki `Student`, jak pokazano w metodzie `Details`. Nie musisz zmieniać tej metody.

   Te zmiany implementują najlepsze [rozwiązanie w zakresie zabezpieczeń, aby](#overpost)zapobiec nadpisywaniu, ponieważ szkielet został wygenerowany jako atrybut `Bind` i dodano jednostkę utworzoną przez spinacz modelu do zestawu jednostek ze zmodyfikowaną flagą. Ten kod nie jest już zalecany, ponieważ atrybut `Bind` czyści wszystkie istniejące wcześniej dane w polach, które nie znajdują się w parametrze `Include`. W przyszłości program do tworzenia szkieletu kontrolera MVC zostanie zaktualizowany, aby nie generował `Bind` atrybutów dla metod edycji.

   Nowy kod odczytuje istniejącą jednostkę i wywołuje <xref:System.Web.Mvc.Controller.TryUpdateModel%2A>, aby aktualizować pola z danych wejściowych użytkownika w ogłoszonych formularzach. Automatyczne śledzenie zmian Entity Framework ustawia flagę [EntityState. Modified](<xref:System.Data.EntityState.Modified>) w jednostce. Gdy wywoływana jest metoda [metody SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) , flaga <xref:System.Data.EntityState.Modified> powoduje, że Entity Framework tworzenia instrukcji SQL w celu zaktualizowania wiersza bazy danych. [Konflikty współbieżności](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md) są ignorowane, a wszystkie kolumny wiersza bazy danych są aktualizowane, w tym te, które nie uległy zmianie przez użytkownika. (W dalszej części samouczka pokazano, jak obsłużyć konflikty współbieżności, a jeśli chcesz, aby tylko poszczególne pola były aktualizowane w bazie danych, można ustawić jednostkę na [EntityState. Unchanged](<xref:System.Data.EntityState.Unchanged>) i ustawić poszczególne pola na [EntityState. Modified](<xref:System.Data.EntityState.Modified>)).

   Aby zapobiec przepełnieniu, pola, które mają być aktualizowane przez stronę edycji, są listy dozwolonych w parametrach `TryUpdateModel`. Obecnie nie ma żadnych dodatkowych pól, które są chronione, ale lista pól, które mają być powiązane przez spinacz modelu, zapewnia, że jeśli dodasz pola do modelu danych w przyszłości, są one automatycznie chronione, dopóki nie zostaną jawnie dodane do nich w tym miejscu.

   W wyniku tych zmian podpis metody metody edycji HttpPost jest taki sam jak Metoda edycji narzędzia HttpGet; w związku z tym zmieniono nazwę metody EditPost.

   > [!TIP]
   >
   > **Stany jednostek i metody Attach i metody SaveChanges**
   >
   > Kontekst bazy danych śledzi, czy jednostki w pamięci są zsynchronizowane z odpowiadającymi im wierszami w bazie danych, a te informacje określają, co się stanie w przypadku wywołania metody `SaveChanges`. Na przykład w przypadku przekazania nowej jednostki do metody [Add](https://msdn.microsoft.com/library/system.data.entity.dbset.add(v=vs.103).aspx) stan tej jednostki jest ustawiony na `Added`. Następnie wywołując metodę [metody SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) , kontekst bazy danych wystawia polecenie SQL `INSERT`.
   >
   > Jednostka może być w jednym z następujących [Stanów](xref:System.Data.EntityState):
   >
   > - `Added`. Jednostka jeszcze nie istnieje w bazie danych. Metoda `SaveChanges` musi wydać instrukcję `INSERT`.
   > - `Unchanged`. Nie trzeba wykonywać żadnych czynności za pomocą tej jednostki za pomocą metody `SaveChanges`. Po odczytaniu jednostki z bazy danych jednostka zaczyna się od tego stanu.
   > - `Modified`. Niektóre lub wszystkie wartości właściwości jednostki zostały zmodyfikowane. Metoda `SaveChanges` musi wydać instrukcję `UPDATE`.
   > - `Deleted`. Jednostka została oznaczona do usunięcia. Metoda `SaveChanges` musi wydać instrukcję `DELETE`.
   > - `Detached`. Jednostka nie jest śledzona przez kontekst bazy danych.
   >
   > W aplikacji klasycznej zmiany stanu są zazwyczaj ustawiane automatycznie. W przypadku typu pulpitu aplikacji należy odczytać jednostkę i wprowadzić zmiany w niektórych wartościach właściwości. Powoduje to, że stan jednostki jest automatycznie zmieniany na `Modified`. Po wywołaniu `SaveChanges`, Entity Framework generuje instrukcję SQL `UPDATE`, która aktualizuje tylko rzeczywiste właściwości, które zostały zmienione.
   >
   > Rozłączona natura aplikacji sieci Web nie zezwala na tę ciągłą sekwencję. [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx) , który odczytuje jednostkę, jest usuwany po wyrenderowaniu strony. Gdy zostanie wywołana metoda działania `HttpPost` `Edit`, zostanie wykonane nowe żądanie i masz nowe wystąpienie [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx), więc musisz ręcznie ustawić stan jednostki, aby `Modified.` następnie podczas wywoływania `SaveChanges`, Entity Framework aktualizuje wszystkie kolumny wiersza bazy danych, ponieważ kontekst nie ma informacji o zmianach właściwości.
   >
   > Jeśli chcesz, aby instrukcja SQL `Update` zaktualizował tylko pola, które zostały zmienione przez użytkownika, możesz zapisać oryginalne wartości w jakiś sposób (na przykład ukryte pola), aby były dostępne po wywołaniu metody `HttpPost` `Edit`. Następnie można utworzyć jednostkę `Student` przy użyciu oryginalnych wartości, wywołać metodę `Attach` z tą oryginalną wersją jednostki, zaktualizować wartości jednostki do nowych wartości, a następnie wywołać `SaveChanges.` Aby uzyskać więcej informacji, zobacz temat [Stany jednostki i metody SaveChanges](/ef/ef6/saving/change-tracking/entity-state) i [dane lokalne](/ef/ef6/querying/local-data).

   Kod HTML i Razor w *Views\Student\Edit.cshtml* są podobne do przedstawiono w temacie *Create. cshtml*i nie są wymagane żadne zmiany.

2. Uruchom stronę, uruchamiając program, wybierając kartę **studenci** , a następnie klikając hiperłącze **Edytuj** .

3. Zmień niektóre dane i kliknij przycisk **Zapisz**. Na stronie indeks są widoczne zmienione dane.

4. Zamknij okno przeglądarki.

## <a name="update-the-delete-page"></a>Aktualizuj stronę Delete

W *Controllers\StudentController.cs*kod szablonu metody `Delete` <xref:System.Web.Mvc.HttpGetAttribute> używa metody `Find` do pobrania wybranej jednostki `Student`, jak pokazano w metodach `Details` i `Edit`. Jednak w celu zaimplementowania niestandardowego komunikatu o błędzie, gdy wywołanie `SaveChanges` zakończy się niepowodzeniem, należy dodać do tej metody pewne funkcje i odpowiedni widok.

Podczas operacji aktualizowania i tworzenia należy wykonać operacje usuwania, które wymagają dwóch metod akcji. Metoda wywoływana w odpowiedzi na żądanie GET wyświetla widok, który daje użytkownikowi możliwość zatwierdzenia lub anulowania operacji usuwania. Jeśli użytkownik zatwierdzi ten element, zostanie utworzone żądanie POST. Gdy tak się stanie, Metoda `HttpPost` `Delete` jest wywoływana, a następnie ta metoda faktycznie wykonuje operację usuwania.

Dodasz blok `try-catch` do metody <xref:System.Web.Mvc.HttpPostAttribute> `Delete`, aby obsłużyć wszelkie błędy, które mogą wystąpić podczas aktualizacji bazy danych. Jeśli wystąpi błąd, Metoda <xref:System.Web.Mvc.HttpPostAttribute> `Delete` wywołuje metodę <xref:System.Web.Mvc.HttpGetAttribute> `Delete`, przekazując do niej parametr wskazujący, że wystąpił błąd. Metoda <xref:System.Web.Mvc.HttpGetAttribute> `Delete` następnie ponownie wyświetla stronę potwierdzenia wraz z komunikatem o błędzie, dając użytkownikowi możliwość anulowania lub spróbuj ponownie.

1. Zastąp <xref:System.Web.Mvc.HttpGetAttribute> `Delete` metodę akcji następującym kodem, który zarządza raportowaniem błędów:

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample12.cs?highlight=1,7-10)]

    Ten kod akceptuje [opcjonalny parametr](https://msdn.microsoft.com/library/dd264739.aspx) , który wskazuje, czy metoda została wywołana po niepowodzeniu zapisania zmian. Ten parametr jest `false` w przypadku wywołania metody `HttpGet` `Delete` bez wcześniejszego błędu. Gdy jest wywoływana przez metodę `Delete` `HttpPost` w odpowiedzi na błąd aktualizacji bazy danych, parametr jest `true`, a komunikat o błędzie jest przesyłany do widoku.

2. Zastąp metodę Action `Delete` <xref:System.Web.Mvc.HttpPostAttribute> (o nazwie `DeleteConfirmed`) następującym kodem, który wykonuje rzeczywistą operację usuwania i przechwytuje wszystkie błędy aktualizacji bazy danych.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample13.cs)]

    Ten kod pobiera wybraną jednostkę, a następnie wywołuje metodę [Remove](https://msdn.microsoft.com/library/system.data.entity.dbset.remove(v=vs.103).aspx) w celu ustawienia stanu jednostki na `Deleted`. Po wywołaniu `SaveChanges` zostanie wygenerowane polecenie SQL `DELETE`. Zmieniono także nazwę metody akcji z `DeleteConfirmed` na `Delete`. Kod szkieletu o nazwie `HttpPost` `Delete` Metoda `DeleteConfirmed`, aby dać metodę `HttpPost` unikatową sygnaturę. (Środowisko CLR wymaga przeciążonych metod, aby mieć inne parametry metody). Teraz, gdy podpisy są unikatowe, można naklejić do Konwencji MVC i używać tej samej nazwy dla `HttpPost` i `HttpGet` metody Delete.

    Jeśli zwiększenie wydajności aplikacji o wysokim poziomie jest priorytetem, można uniknąć niepotrzebnego zapytania SQL, aby pobrać wiersz przez zastąpienie wierszy kodu, które wywołują `Find` i `Remove` metod, przy użyciu następującego kodu:

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample14.cs)]

    Ten kod tworzy wystąpienie `Student` jednostki przy użyciu tylko wartości klucza podstawowego, a następnie ustawia stan jednostki na `Deleted`. To wszystko, co Entity Framework potrzebuje, aby usunąć jednostkę.

    Jak wspomniano, Metoda `HttpGet` `Delete` nie usuwa danych. Wykonanie operacji usuwania w odpowiedzi na żądanie GET (lub w tym przypadku wykonanie jakiejkolwiek operacji edycji, operacji tworzenia lub jakiejkolwiek innej operacji, która zmienia dane), powoduje powstanie zagrożenia bezpieczeństwa. Aby uzyskać więcej informacji, zobacz [ASP.NET #46 MVC — nie używaj linków do usuwania, ponieważ tworzą one luki w zabezpieczeniach](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx) na blogu Stephen Walther.

3. W *Views\Student\Delete.cshtml*, Dodaj komunikat o błędzie między nagłówkiem `h2` i nagłówkiem `h3`, jak pokazano w następującym przykładzie:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample15.cshtml?highlight=2)]

4. Uruchom stronę, uruchamiając program, wybierając kartę **studenci** , a następnie klikając hiperłącze **Usuń** .

5. Wybierz pozycję **Usuń** na stronie, na której jest na pewno **chcesz usunąć ten komunikat?** .

    Strona indeks zostanie wyświetlona bez usuniętego ucznia. (W [samouczku współbieżności](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)zostanie wyświetlony przykładowy kod obsługi błędu).

## <a name="close-database-connections"></a>Zamknij połączenia bazy danych

W celu zamknięcia połączeń z bazą danych i zwolnienia zasobów, które znajdują się jak najszybciej, należy usunąć wystąpienie kontekstu po zakończeniu pracy z nim. To dlatego, że kod szkieletu udostępnia metodę [Dispose](https://msdn.microsoft.com/library/system.idisposable.dispose(v=vs.110).aspx) na końcu klasy `StudentController` w *StudentController.cs*, jak pokazano w następującym przykładzie:

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample16.cs)]

Klasa bazowa `Controller` już implementuje interfejs `IDisposable`, więc ten kod po prostu dodaje przesłonięcie do metody `Dispose(bool)`, aby jawnie usunąć wystąpienie kontekstu.

## <a name="handle-transactions"></a>Obsługa transakcji

Domyślnie Entity Framework niejawnie implementują transakcje. W scenariuszach, w których wprowadzane są zmiany w wielu wierszach lub tabelach, a następnie Wywołaj `SaveChanges`, Entity Framework automatycznie sprawdza, czy wszystkie zmiany zostały wykonane pomyślnie, czy też kończą się niepowodzeniem. Jeśli najpierw zostaną wykonane pewne zmiany, a następnie wystąpi błąd, te zmiany są automatycznie wycofywane. W scenariuszach, w których potrzebujesz więcej kontroli&mdash;na przykład, jeśli chcesz uwzględnić operacje wykonywane poza Entity Framework w transakcji&mdash;zobacz [Praca z transakcjami](/ef/ef6/saving/transactions).

## <a name="get-the-code"></a>Uzyskiwanie kodu

[Pobierz ukończony projekt](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Dodatkowe zasoby

Masz teraz pełny zestaw stron, które wykonują proste operacje CRUD dla jednostek `Student`. Użyto pomocników MVC do generowania elementów interfejsu użytkownika dla pól danych. Aby uzyskać więcej informacji na temat pomocników MVC, zobacz [renderowanie formularza za pomocą pomocników HTML](/previous-versions/aspnet/dd410596(v=vs.98)) (artykuł jest przeznaczony dla MVC 3, ale jest nadal istotny dla MVC 5).

Linki do innych zasobów EF 6 można znaleźć w temacie [ASP.NET Data Access — zalecane zasoby](../../../../whitepapers/aspnet-data-access-content-map.md).

## <a name="next-steps"></a>Następne kroki

W tym samouczku zostaną wykonane następujące czynności:

> [!div class="checklist"]
> * Utworzono stronę szczegółów
> * Zaktualizowano stronę tworzenia
> * Zaktualizowano metodę edycji HttpPost
> * Zaktualizowano stronę usuwania
> * Zamknięte połączenia bazy danych
> * Obsłużone transakcje

Przejdź do następnego artykułu, aby dowiedzieć się, jak dodać sortowanie, filtrowanie i stronicowanie do projektu.
> [!div class="nextstepaction"]
> [Sortowanie, filtrowanie i stronicowanie](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)

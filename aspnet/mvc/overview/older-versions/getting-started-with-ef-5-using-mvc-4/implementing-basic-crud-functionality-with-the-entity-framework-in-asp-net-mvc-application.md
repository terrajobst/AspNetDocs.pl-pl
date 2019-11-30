---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
title: Implementowanie podstawowej funkcjonalności CRUD z Entity Framework w aplikacji ASP.NET MVC (2 z 10) | Microsoft Docs
author: tdykstra
description: Przykładowa aplikacja internetowa Contoso University pokazuje, jak tworzyć aplikacje ASP.NET MVC 4 przy użyciu Code First Entity Framework 5 i programu Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: f7bace3f-b85a-47ff-b5fe-49e81441cdf9
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: eba146d2975e0dcf243facf7c205c4acfe40b6f6
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74595329"
---
# <a name="implementing-basic-crud-functionality-with-the-entity-framework-in-aspnet-mvc-application-2-of-10"></a>Implementowanie podstawowej funkcjonalności CRUD z Entity Framework w aplikacji ASP.NET MVC (2 z 10)

Autor [Dykstra](https://github.com/tdykstra)

[Pobierz ukończony projekt](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Przykładowa aplikacja internetowa Contoso University pokazuje, jak tworzyć aplikacje ASP.NET MVC 4 przy użyciu Entity Framework 5 Code First i programu Visual Studio 2012. Aby uzyskać informacje na temat serii samouczków, zobacz [pierwszy samouczek w serii](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Możesz uruchomić serię samouczków od początku lub [pobrać początkowy projekt dla tego rozdziału](building-the-ef5-mvc4-chapter-downloads.md) i Zacznij tutaj.
> 
> > [!NOTE] 
> > 
> > Jeśli wystąpi problem, którego nie można rozwiązać, [Pobierz ukończony rozdział](building-the-ef5-mvc4-chapter-downloads.md) i spróbuj odtworzyć problem. Ogólnie rzecz biorąc, można znaleźć rozwiązanie problemu, porównując kod z kompletnym kodem. Niektóre typowe błędy i sposoby ich rozwiązywania można znaleźć w temacie [błędy i obejścia.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)

W poprzednim samouczku utworzono aplikację MVC, która przechowuje i wyświetla dane przy użyciu Entity Framework i SQL Server LocalDB. W tym samouczku opisano, jak przeglądać i dostosowywać kod CRUD (tworzenie, odczytywanie, aktualizowanie, usuwanie), który jest automatycznie tworzony przez szkielet MVC dla Ciebie w kontrolerach i widokach.

> [!NOTE]
> Jest to typowa Metoda implementacji wzorca repozytorium w celu utworzenia warstwy abstrakcji między kontrolerem i warstwą dostępu do danych. Aby zachować te samouczki jako proste, nie należy implementować repozytorium do późniejszego samouczka w tej serii.

W tym samouczku utworzysz następujące strony sieci Web:

![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image1.png)

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image2.png)

![Student_Create_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image3.png)

![Student_delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image4.png)

## <a name="creating-a-details-page"></a>Tworzenie strony szczegółów

Kod szkieletu dla uczniów `Index` stronie, który opuścił Właściwość `Enrollments`, ponieważ ta właściwość zawiera kolekcję. Na stronie `Details` zostanie wyświetlona zawartość kolekcji w tabeli HTML.

 W *Controllers\StudentController.cs*Metoda akcji dla widoku `Details` używa metody `Find` do pobierania pojedynczej jednostki `Student`. 

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample1.cs)]

 Wartość klucza jest przenoszona do metody jako parametr `id` i pochodzi z danych trasy w hiperłączu **szczegóły** na stronie indeks. 

1. Otwórz *Views\Student\Details.cshtml*. Każde pole jest wyświetlane za pomocą pomocnika `DisplayFor`, jak pokazano w następującym przykładzie: 

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample2.cshtml)]
2. Po polu `EnrollmentDate` i bezpośrednio przed tagiem zamykającym `fieldset` Dodaj kod, aby wyświetlić listę rejestracji, jak pokazano w następującym przykładzie:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample3.cshtml?highlight=4-22)]

    Ten kod pętle za pomocą jednostek we właściwości nawigacji `Enrollments`. Dla każdej jednostki `Enrollment` we właściwości wyświetla tytuł kursu i klasy. Tytuł kursu jest pobierany z jednostki `Course`, która jest przechowywana we właściwości nawigacji `Course` jednostki `Enrollments`. Wszystkie te dane są pobierane z bazy danych automatycznie, gdy jest to konieczne. (Innymi słowy, używasz ładowania z opóźnieniem tutaj. Nie określono *eager ładowania* dla właściwości nawigacji `Courses`, więc przy pierwszym próbie uzyskania dostępu do tej właściwości zostanie wysłane zapytanie do bazy danych w celu pobrania danych. Więcej informacji na temat ładowania z opóźnieniem i ładowania eager można znaleźć w artykule samouczek dotyczący [odczytywania danych pokrewnych](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) w dalszej części tej serii.)
3. Uruchom stronę, wybierając kartę **studenci** i klikając link **szczegóły** dla Alexander Carson. Zostanie wyświetlona lista kursów i ocen dla wybranego ucznia:

    ![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image5.png)

## <a name="updating-the-create-page"></a>Aktualizowanie strony tworzenia

1. W *Controllers\StudentController.cs*Zastąp metodę akcji `HttpPost``Create` poniższym kodem, aby dodać blok `try-catch` i [atrybut bind](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx) do metody szkieletowej: 

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample4.cs?highlight=4,7-8,14-20)]

    Ten kod dodaje jednostkę `Student` utworzoną przez spinacz modelu ASP.NET MVC do zestawu jednostek `Students`, a następnie zapisuje zmiany w bazie danych. (*Model spinacza* odnosi się do funkcji ASP.NET MVC, która ułatwia pracę z danymi przesyłanymi przez formularz; segregator modelu konwertuje wartości formularza ogłoszonego na typy CLR i przekazuje je do metody akcji w parametrach. W takim przypadku spinacz modelu tworzy wystąpienie `Student` jednostki za pomocą wartości właściwości z kolekcji `Form`.)

    Atrybut `ValidateAntiForgeryToken` pomaga zapobiegać atakom na [fałszerstwo żądań między witrynami](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) .

<a id="overpost"></a>

    > [!WARNING]
    > Security - The `Bind` attribute is added to protect against *over-posting*. For example, suppose the `Student` entity includes a `Secret` property that you don't want this web page to update.
    > 
    > [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample5.cs?highlight=7)]
    > 
    > Even if you don't have a `Secret` field on the web page, a hacker could use a tool such as [fiddler](http://fiddler2.com/home), or write some JavaScript, to post a `Secret` form value. Without the [Bind](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx) attribute limiting the fields that the model binder uses when it creates a `Student` instance*,* the model binder would pick up that `Secret` form value and use it to update the `Student` entity instance. Then whatever value the hacker specified for the `Secret` form field would be updated in your database. The following image shows the fiddler tool adding the `Secret` field (with the value "OverPost") to the posted form values.
    > 
    > ![](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image6.png)  
    > 
    > The value "OverPost" would then be successfully added to the `Secret` property of the inserted row, although you never intended that the web page be able to update that property.
    > 
    > It's a security best practice to use the `Include` parameter with the `Bind` attribute to *whitelist* fields. It's also possible to use the `Exclude` parameter to *blacklist* fields you want to exclude. The reason `Include` is more secure is that when you add a new property to the entity, the new field is not automatically protected by an `Exclude` list.
    > 
    > Another alternative approach, and one preferred by many, is to use only view models with model binding. The view model contains only the properties you want to bind. Once the MVC model binder has finished, you copy the view model properties to the entity instance.

    Other than the `Bind` attribute, the `try-catch` block is the only change you've made to the scaffolded code. If an exception that derives from [DataException](https://msdn.microsoft.com/library/system.data.dataexception.aspx) is caught while the changes are being saved, a generic error message is displayed. [DataException](https://msdn.microsoft.com/library/system.data.dataexception.aspx) exceptions are sometimes caused by something external to the application rather than a programming error, so the user is advised to try again. Although not implemented in this sample, a production quality application would log the exception (and non-null inner exceptions ) with a logging mechanism such as [ELMAH](https://code.google.com/p/elmah/).

    The code in *Views\Student\Create.cshtml* is similar to what you saw in *Details.cshtml*, except that `EditorFor` and `ValidationMessageFor` helpers are used for each field instead of `DisplayFor`. The following example shows the relevant code:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample6.cshtml)]

    *Create.cshtml* also includes `@Html.AntiForgeryToken()`, which works with the `ValidateAntiForgeryToken` attribute in the controller to help prevent [cross-site request forgery](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) attacks.

    No changes are required in *Create.cshtml*.
2. Uruchom stronę, wybierając kartę **uczniowie** i klikając pozycję **Utwórz nową**.

    ![Student_Create_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image7.png)

    Niektóre sprawdzanie poprawności danych działa domyślnie. Wprowadź nazwy i nieprawidłową datę, a następnie kliknij przycisk **Utwórz** , aby wyświetlić komunikat o błędzie.

    ![Students_Create_page_error_message](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image8.png)

    Następujący wyróżniony kod pokazuje sprawdzanie poprawności modelu.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample7.cs?highlight=5)]

    Zmień datę na prawidłową wartość, na przykład 9/1/2005, a następnie kliknij pozycję **Utwórz** , aby zobaczyć, że nowy student zostanie wyświetlony na stronie **indeks** .

    ![Students_Index_page_with_new_student](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image9.png)

## <a name="updating-the-edit-post-page"></a>Aktualizowanie strony edycji wpisu

W *Controllers\StudentController.cs*, metoda `Edit` `HttpGet` (bez atrybutu `HttpPost`) używa metody `Find` do pobrania wybranej jednostki `Student`, jak pokazano w metodzie `Details`. Nie musisz zmieniać tej metody.

Należy jednak zastąpić `HttpPost` `Edit` metodę akcji następującym kodem, aby dodać blok `try-catch` i [atrybut bind](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx):

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample8.cs)]

Ten kod jest podobny do tego, co zostało nadane w `HttpPost` `Create` metodzie. Jednak zamiast dodawania jednostki utworzonej przez spinacz modelu do zestawu jednostek, ten kod ustawia flagę w jednostce wskazującej, że została zmieniona. Gdy metoda [metody SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) jest wywoływana, [modyfikowana](https://msdn.microsoft.com/library/system.data.entitystate.aspx) flaga powoduje, że Entity Framework tworzenia instrukcji SQL w celu zaktualizowania wiersza bazy danych. Wszystkie kolumny wiersza bazy danych zostaną zaktualizowane, łącznie z tymi, które nie uległy zmianie, i konflikty współbieżności są ignorowane. (Dowiesz się, jak obsługiwać współbieżność w kolejnym samouczku w tej serii).

### <a name="entity-states-and-the-attach-and-savechanges-methods"></a>Stany jednostek i metody Attach i metody SaveChanges

Kontekst bazy danych śledzi, czy jednostki w pamięci są zsynchronizowane z odpowiadającymi im wierszami w bazie danych, a te informacje określają, co się stanie w przypadku wywołania metody `SaveChanges`. Na przykład w przypadku przekazania nowej jednostki do metody [Add](https://msdn.microsoft.com/library/system.data.entity.dbset.add(v=vs.103).aspx) stan tej jednostki jest ustawiony na `Added`. Następnie wywołując metodę [metody SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) , kontekst bazy danych wystawia polecenie SQL `INSERT`.

Jednostka może być w jednym z[następujących stanów](https://msdn.microsoft.com/library/system.data.entitystate.aspx):

- `Added`. Jednostka jeszcze nie istnieje w bazie danych. Metoda `SaveChanges` musi wydać instrukcję `INSERT`.
- `Unchanged`. Nie trzeba wykonywać żadnych czynności za pomocą tej jednostki za pomocą metody `SaveChanges`. Po odczytaniu jednostki z bazy danych jednostka zaczyna się od tego stanu.
- `Modified`. Niektóre lub wszystkie wartości właściwości jednostki zostały zmodyfikowane. Metoda `SaveChanges` musi wydać instrukcję `UPDATE`.
- `Deleted`. Jednostka została oznaczona do usunięcia. Metoda `SaveChanges` musi wydać instrukcję `DELETE`.
- `Detached`. Jednostka nie jest śledzona przez kontekst bazy danych.

W aplikacji klasycznej zmiany stanu są zazwyczaj ustawiane automatycznie. W przypadku typu pulpitu aplikacji należy odczytać jednostkę i wprowadzić zmiany w niektórych wartościach właściwości. Powoduje to, że stan jednostki jest automatycznie zmieniany na `Modified`. Po wywołaniu `SaveChanges`, Entity Framework generuje instrukcję SQL `UPDATE`, która aktualizuje tylko rzeczywiste właściwości, które zostały zmienione.

Rozłączona natura aplikacji sieci Web nie zezwala na tę ciągłą sekwencję. [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx) , który odczytuje jednostkę, jest usuwany po wyrenderowaniu strony. Gdy zostanie wywołana metoda działania `HttpPost` `Edit`, zostanie wykonane nowe żądanie i masz nowe wystąpienie [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx), więc musisz ręcznie ustawić stan jednostki, aby `Modified.` następnie podczas wywoływania `SaveChanges`, Entity Framework aktualizuje wszystkie kolumny wiersza bazy danych, ponieważ kontekst nie ma informacji o zmianach właściwości.

Jeśli chcesz, aby instrukcja SQL `Update` zaktualizował tylko pola, które zostały zmienione przez użytkownika, możesz zapisać oryginalne wartości w jakiś sposób (na przykład ukryte pola), aby były dostępne po wywołaniu metody `HttpPost` `Edit`. Następnie można utworzyć jednostkę `Student` przy użyciu oryginalnych wartości, wywołać metodę `Attach` z tą oryginalną wersją jednostki, zaktualizować wartości jednostki do nowych wartości, a następnie wywoływać `SaveChanges.` Aby uzyskać więcej informacji, zobacz temat [Entity States and metody SaveChanges](https://msdn.microsoft.com/data/jj592676) and [local data](https://msdn.microsoft.com/data/jj592872) Center Centrum deweloperów danych MSDN.

Kod w *Views\Student\Edit.cshtml* jest podobny do tego, co zostało opisane w temacie *Create. cshtml*, i nie są wymagane żadne zmiany.

Uruchom stronę, wybierając kartę **studenci** , a następnie klikając hiperłącze **Edytuj** .

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image10.png)

Zmień niektóre dane i kliknij przycisk **Zapisz**. Na stronie indeks są widoczne zmienione dane.

![Students_Index_page_after_edit](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image11.png)

## <a name="updating-the-delete-page"></a>Aktualizowanie strony usuwania

W *Controllers\StudentController.cs*kod szablonu metody `Delete` `HttpGet` używa metody `Find` do pobrania wybranej jednostki `Student`, jak pokazano w metodach `Details` i `Edit`. Jednak w celu zaimplementowania niestandardowego komunikatu o błędzie, gdy wywołanie `SaveChanges` zakończy się niepowodzeniem, należy dodać do tej metody pewne funkcje i odpowiedni widok.

Podczas operacji aktualizowania i tworzenia należy wykonać operacje usuwania, które wymagają dwóch metod akcji. Metoda wywoływana w odpowiedzi na żądanie GET wyświetla widok, który daje użytkownikowi możliwość zatwierdzenia lub anulowania operacji usuwania. Jeśli użytkownik zatwierdzi ten element, zostanie utworzone żądanie POST. Gdy tak się stanie, Metoda `HttpPost` `Delete` jest wywoływana, a następnie ta metoda faktycznie wykonuje operację usuwania.

Dodasz blok `try-catch` do metody `HttpPost` `Delete`, aby obsłużyć wszelkie błędy, które mogą wystąpić podczas aktualizacji bazy danych. Jeśli wystąpi błąd, Metoda `HttpPost` `Delete` wywołuje metodę `HttpGet` `Delete`, przekazując do niej parametr wskazujący, że wystąpił błąd. Metoda `HttpGet Delete` następnie ponownie wyświetla stronę potwierdzenia wraz z komunikatem o błędzie, dając użytkownikowi możliwość anulowania lub spróbuj ponownie.

1. Zastąp `HttpGet` `Delete` metodę akcji następującym kodem, który zarządza raportowaniem błędów: 

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample9.cs)]

    Ten kod akceptuje [opcjonalny](https://msdn.microsoft.com/library/dd264739.aspx) parametr logiczny, który wskazuje, czy został wywołany po niepowodzeniu zapisania zmian. Ten parametr jest `false` w przypadku wywołania metody `HttpGet` `Delete` bez wcześniejszego błędu. Gdy jest wywoływana przez metodę `Delete` `HttpPost` w odpowiedzi na błąd aktualizacji bazy danych, parametr jest `true`, a komunikat o błędzie jest przesyłany do widoku.
2. Zastąp metodę Action `Delete` `HttpPost` (o nazwie `DeleteConfirmed`) następującym kodem, który wykonuje rzeczywistą operację usuwania i przechwytuje wszystkie błędy aktualizacji bazy danych.

     [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample10.cs)]

     Ten kod pobiera wybraną jednostkę, a następnie wywołuje metodę [Remove](https://msdn.microsoft.com/library/system.data.entity.dbset.remove(v=vs.103).aspx) w celu ustawienia stanu jednostki na `Deleted`. Po wywołaniu `SaveChanges` zostanie wygenerowane polecenie SQL `DELETE`. Zmieniono także nazwę metody akcji z `DeleteConfirmed` na `Delete`. Kod szkieletu o nazwie `HttpPost` `Delete` Metoda `DeleteConfirmed`, aby dać metodę `HttpPost` unikatową sygnaturę. (Środowisko CLR wymaga przeciążonych metod, aby mieć inne parametry metody). Teraz, gdy podpisy są unikatowe, można naklejić do Konwencji MVC i używać tej samej nazwy dla `HttpPost` i `HttpGet` metody Delete.

     Jeśli zwiększenie wydajności aplikacji o wysokim poziomie głośności jest priorytetem, można uniknąć niepotrzebnego zapytania SQL, aby pobrać wiersz przez zastąpienie wierszy kodu, które wywołują `Find` i `Remove` metody z poniższym kodem, jak pokazano w żółtym wyróżnieniu:

     [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample11.cs)]

     Ten kod tworzy wystąpienie `Student` jednostki przy użyciu tylko wartości klucza podstawowego, a następnie ustawia stan jednostki na `Deleted`. To wszystko, co Entity Framework potrzebuje, aby usunąć jednostkę.

     Jak wspomniano, Metoda `HttpGet` `Delete` nie usuwa danych. Wykonanie operacji usuwania w odpowiedzi na żądanie GET (lub w tym przypadku wykonanie jakiejkolwiek operacji edycji, operacji tworzenia lub jakiejkolwiek innej operacji, która zmienia dane), powoduje powstanie zagrożenia bezpieczeństwa. Aby uzyskać więcej informacji, zobacz [ASP.NET #46 MVC — nie używaj linków do usuwania, ponieważ tworzą one luki w zabezpieczeniach](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx) na blogu Stephen Walther.
3. W *Views\Student\Delete.cshtml*, Dodaj komunikat o błędzie między nagłówkiem `h2` i nagłówkiem `h3`, jak pokazano w następującym przykładzie:

     [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample12.cshtml?highlight=2)]

     Uruchom stronę, wybierając kartę **uczniów** i klikając hiperłącze **Usuń** :

     ![Student_Delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image12.png)
4. Kliknij przycisk **Usuń**. Strona indeks zostanie wyświetlona bez usuniętego ucznia. (W dalszej części tej serii zobaczysz przykład kodu obsługi błędu w akcji w samouczku [Obsługa współbieżności](../../getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md) ).

## <a name="ensuring-that-database-connections-are-not-left-open"></a>Upewnienie się, że połączenia bazy danych nie są otwarte

Aby upewnić się, że połączenia bazy danych są prawidłowo zamknięte, a zasoby, które są zwalniane, powinny być widoczne, aby wystąpienie kontekstu zostało usunięte. To dlatego, że kod szkieletu udostępnia metodę [Dispose](https://msdn.microsoft.com/library/system.idisposable.dispose(v=vs.110).aspx) na końcu klasy `StudentController` w *StudentController.cs*, jak pokazano w następującym przykładzie:

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample13.cs)]

Klasa bazowa `Controller` już implementuje interfejs `IDisposable`, więc ten kod po prostu dodaje przesłonięcie do metody `Dispose(bool)`, aby jawnie usunąć wystąpienie kontekstu.

## <a name="summary"></a>Podsumowanie

Masz teraz pełny zestaw stron, które wykonują proste operacje CRUD dla jednostek `Student`. Użyto pomocników MVC do generowania elementów interfejsu użytkownika dla pól danych. Aby uzyskać więcej informacji na temat pomocników MVC, zobacz [renderowanie formularza za pomocą pomocników HTML](https://msdn.microsoft.com/library/dd410596(v=VS.98).aspx) (strona dotyczy MVC 3, ale jest nadal istotna dla MVC 4).

W następnym samouczku poznasz funkcjonalność strony indeks poprzez dodanie sortowania i stronicowania.

Linki do innych zasobów Entity Framework można znaleźć w [mapie zawartości dostęp do danych ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Poprzednie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)
> [dalej](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)

---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: Odczytywanie powiązanych danych za pomocą Entity Framework w aplikacji ASP.NET MVC (5 z 10) | Microsoft Docs
author: tdykstra
description: Przykładowa aplikacja internetowa Contoso University pokazuje, jak tworzyć aplikacje ASP.NET MVC 4 przy użyciu Code First Entity Framework 5 i programu Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 0d6fb83b-71f7-425d-8dec-981197d7ec42
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: e1752022b66952783039fbea0c2880e2f9be6ef8
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74595210"
---
# <a name="reading-related-data-with-the-entity-framework-in-an-aspnet-mvc-application-5-of-10"></a>Odczytywanie powiązanych danych za pomocą Entity Framework w aplikacji ASP.NET MVC (5 z 10)

Autor [Dykstra](https://github.com/tdykstra)

[Pobierz ukończony projekt](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Przykładowa aplikacja internetowa Contoso University pokazuje, jak tworzyć aplikacje ASP.NET MVC 4 przy użyciu Entity Framework 5 Code First i programu Visual Studio 2012. Aby uzyskać informacje na temat serii samouczków, zobacz [pierwszy samouczek w serii](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Możesz uruchomić serię samouczków od początku lub [pobrać początkowy projekt dla tego rozdziału](building-the-ef5-mvc4-chapter-downloads.md) i Zacznij tutaj.
> 
> > [!NOTE] 
> > 
> > Jeśli wystąpi problem, którego nie można rozwiązać, [Pobierz ukończony rozdział](building-the-ef5-mvc4-chapter-downloads.md) i spróbuj odtworzyć problem. Ogólnie rzecz biorąc, można znaleźć rozwiązanie problemu, porównując kod z kompletnym kodem. Niektóre typowe błędy i sposoby ich rozwiązywania można znaleźć w temacie [błędy i obejścia.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)

W poprzednim samouczku został ukończony model danych szkoły. W tym samouczku pokazano, jak odczytywać i wyświetlać powiązane dane — czyli dane, które Entity Framework ładowane do właściwości nawigacji.

Na poniższych ilustracjach przedstawiono strony, z którymi będziesz korzystać.

![](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="lazy-eager-and-explicit-loading-of-related-data"></a>Opóźnione, Eager i jawne ładowanie powiązanych danych

Istnieje kilka Entity Framework sposobów załadowania powiązanych danych do właściwości nawigacji jednostki:

- *Ładowanie z opóźnieniem*. Gdy obiekt jest najpierw odczytywany, powiązane dane nie są pobierane. Jednak przy pierwszej próbie uzyskania dostępu do właściwości nawigacji dane wymagane dla tej właściwości nawigacji są pobierane automatycznie. Powoduje to, że wiele zapytań jest wysyłanych do bazy danych — jeden dla samej jednostki i po każdym każdej chwili, gdy powiązane dane dla jednostki muszą zostać pobrane. 

    ![Lazy_loading_example](https://asp.net/media/2577850/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Lazy_loading_example_2c44eabb-5fd3-485a-837d-8e3d053f2c0c.png)
- *Ładowanie eager*. Po odczytaniu jednostki są pobierane powiązane dane. Zwykle powoduje to pojedyncze zapytanie sprzężenia, które pobiera wszystkie dane, które są zbędne. Należy określić eager ładowania przy użyciu metody `Include`.

    ![Eager_loading_example](https://asp.net/media/2577856/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Eager_loading_example_33f907ff-f0b0-4057-8e75-05a8cacac807.png)
- *Jawne ładowanie*. Jest to podobne do ładowania z opóźnieniem, z tą różnicą, że jawnie pobierasz powiązane dane w kodzie; nie odbywa się to automatycznie podczas uzyskiwania dostępu do właściwości nawigacji. Powiązane dane można ładować ręcznie, pobierając wpis menedżera stanu obiektów dla jednostki i wywołując metodę `Collection.Load` dla kolekcji lub metodę `Reference.Load` dla właściwości, które zawierają pojedynczą jednostkę. (W poniższym przykładzie, jeśli chcesz załadować właściwość nawigacji administratora, Zastąp `Collection(x => x.Courses)` z `Reference(x => x.Administrator)`.)

    ![Explicit_loading_example](https://asp.net/media/2577862/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Explicit_loading_example_79d8c368-6d82-426f-be9a-2b443644ab15.png)

Ponieważ nie pobierają one natychmiast wartości właściwości, ładowanie z opóźnieniem i jawne ładowanie są również nazywane *opóźnionym ładowaniem*.

Ogólnie rzecz biorąc, Jeśli wiesz, że potrzebne są powiązane dane dla każdej pobranej jednostki, ładowanie eager oferuje najlepszą wydajność, ponieważ pojedyncze zapytanie wysyłane do bazy danych jest zwykle bardziej wydajne niż osobne zapytania dla każdej pobranej jednostki. Na przykład w powyższych przykładach Załóżmy, że każdy dział ma dziesięć powiązanych kursów. Przykład ładowania eager może skutkować pojedynczym zapytaniem (sprzężeniem) i pojedynczą rundą w bazie danych. Przykłady ładowania z opóźnieniem i jawnego ładowania powodują jedenaście zapytań i jedenaście rund do bazy danych. Dodatkowe podróże do bazy danych są szczególnie niekorzystne w przypadku opóźnień.

Z drugiej strony, w niektórych scenariuszach ładowanie z opóźnieniem jest bardziej wydajne. Ładowanie eager może spowodować wygenerowanie bardzo złożonej sprzężenia, które SQL Server nie może przetwarzać efektywnie. Lub jeśli musisz uzyskać dostęp do właściwości nawigacji jednostki tylko dla podzbioru zestawu jednostek, które są przetwarzane, ładowanie z opóźnieniem może być lepsze, ponieważ ładowanie eager spowodowałoby pobranie większej ilości danych niż jest to potrzebne. Jeśli wydajność ma kluczowe znaczenie, najlepszym rozwiązaniem jest przetestowanie wydajności obu metod w celu zapewnienia najlepszego wyboru.

Zazwyczaj użycie jawnego ładowania jest możliwe tylko wtedy, gdy włączono pobieranie z opóźnieniem. Jeden scenariusz, gdy należy wyłączyć pobieranie z opóźnieniem, jest w trakcie serializacji. Pobieranie z opóźnieniem i Serializacja nie są dobrze mieszane i jeśli nie jest to konieczne, możesz wykonać zapytania o znacznie większej ilości danych niż zamierzone, gdy jest włączone ładowanie z opóźnieniem. Serializacja zazwyczaj działa przez uzyskanie dostępu do każdej właściwości w wystąpieniu typu. Dostęp do właściwości wyzwala ładowanie z opóźnieniem, a te jednostki załadowane z opóźnieniem są serializowane. Następnie proces serializacji uzyskuje dostęp do każdej właściwości jednostek ładowanych z opóźnieniem, co może spowodować jeszcze większą liczbę operacji ładowania i serializacji z opóźnieniem. Aby zapobiec tej reakcji łańcucha uruchamiania, przed serializacji jednostki należy wyłączyć pobieranie z opóźnieniem.

Klasa kontekstu bazy danych domyślnie wykonuje ładowanie z opóźnieniem. Istnieją dwa sposoby wyłączenia ładowania z opóźnieniem:

- Dla określonych właściwości nawigacji, Pomiń słowo kluczowe `virtual` podczas deklarowania właściwości.
- Dla wszystkich właściwości nawigacji Ustaw `LazyLoadingEnabled` na `false`. Na przykład można umieścić następujący kod w konstruktorze klasy kontekstu: 

    [!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

Ładowanie z opóźnieniem może maskować kod, który powoduje problemy z wydajnością. Na przykład kod, który nie określa eager lub bezpośredniego ładowania, ale przetwarza dużą liczbę jednostek i używa kilku właściwości nawigacji w każdej iteracji, może być bardzo wydajny (ze względu na wiele podróży do bazy danych). Aplikacja, która działa dobrze podczas opracowywania przy użyciu lokalnego programu SQL Server, może mieć problemy z wydajnością podczas przenoszenia do Azure SQL Database z powodu zwiększonego opóźnienia i obciążenia z opóźnieniem. Profilowanie zapytań bazy danych przy użyciu realistycznego obciążenia testowego pomoże określić, czy ładowanie z opóźnieniem jest odpowiednie. Aby uzyskać więcej informacji, zobacz [sztuczna Entity Framework Strategies: ładowanie powiązanych danych](https://msdn.microsoft.com/magazine/hh205756.aspx) i [Korzystanie z Entity Framework, aby zmniejszyć opóźnienie sieci do usługi SQL Azure](https://msdn.microsoft.com/magazine/gg309181.aspx).

## <a name="create-a-courses-index-page-that-displays-department-name"></a>Utwórz stronę indeksu kursów, która wyświetla nazwę działu

Jednostka `Course` obejmuje właściwość nawigacji, która zawiera `Department` jednostkę działu, do której jest przypisany kurs. Aby wyświetlić nazwę przypisanego działu na liście kursów, należy uzyskać Właściwość `Name` z jednostki `Department`, która znajduje się we właściwości nawigacji `Course.Department`.

Utwórz kontroler o nazwie `CourseController` dla typu jednostki `Course`, używając tych samych opcji, które były wcześniej dostępne dla kontrolera `Student`, jak pokazano na poniższej ilustracji (z wyjątkiem tego, że w przeciwieństwie do obrazu, Klasa kontekstowa znajduje się w przestrzeni nazw DAL, a nie w przestrzeni nazw modeli):

![Add_Controller_dialog_box_for_Course_controller](https://asp.net/media/2577868/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Add_Controller_dialog_box_for_Course_controller_c167c11e-2d3e-4b64-a2b9-a0b368b8041d.png)

Otwórz *Controllers\CourseController.cs* i przyjrzyj się metodzie `Index`:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Funkcja automatycznego tworzenia szkieletów określiła eager ładowania dla właściwości nawigacji `Department` przy użyciu metody `Include`.

Otwórz *Views\Course\Index.cshtml* i Zastąp istniejący kod następującym kodem. Zmiany są wyróżnione:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=4,15,18,28-30)]

Do kodu szkieletowego wprowadzono następujące zmiany:

- Zmieniono nagłówek z **indeksu** na **kursy**.
- Przenosi linki wierszy z lewej strony.
- Dodano kolumnę pod **numerem** nagłówka, która zawiera `CourseID` wartość właściwości. (Domyślnie klucze podstawowe nie są szkieletowe, ponieważ zwykle nie są to użytkownicy końcowi. Jednak w tym przypadku klucz podstawowy ma znaczenie i chcesz go wyświetlić.
- Zmieniono nagłówek ostatniej kolumny z **DepartmentID** (nazwę klucza obcego na jednostkę `Department`) do **działu**.

Zwróć uwagę, że dla ostatniej kolumny kod szkieletu wyświetla Właściwość `Name` jednostki `Department`, która jest załadowana do właściwości nawigacji `Department`:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml)]

Uruchom stronę (Wybierz kartę **kursy** na stronie głównej programu Contoso University), aby wyświetlić listę z nazwami działów.

![Courses_index_page_with_department_names](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="create-an-instructors-index-page-that-shows-courses-and-enrollments"></a>Utwórz stronę indeksu instruktorów pokazującą kursy i rejestracje

W tej sekcji utworzysz kontroler i widok dla jednostki `Instructor`, aby wyświetlić stronę indeksu instruktorów:

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Ta strona odczytuje i wyświetla powiązane dane w następujący sposób:

- Lista instruktorów wyświetla powiązane dane z jednostki `OfficeAssignment`. Jednostki `Instructor` i `OfficeAssignment` znajdują się w relacji jeden-do-zero-lub-jednego. Użyjesz eager ładowania dla jednostek `OfficeAssignment`. Jak wyjaśniono wcześniej, ładowanie eager jest zwykle bardziej wydajne, gdy potrzebne są powiązane dane dla wszystkich pobranych wierszy tabeli podstawowej. W takim przypadku chcesz wyświetlić przypisania pakietu Office dla wszystkich wyświetlanych instruktorów.
- Gdy użytkownik wybierze instruktora, zostaną wyświetlone powiązane jednostki `Course`. Jednostki `Instructor` i `Course` znajdują się w relacji wiele-do-wielu. Będziesz używać eager ładowania dla jednostek `Course` i powiązanych z nimi jednostek `Department`. W takim przypadku ładowanie z opóźnieniem może być bardziej wydajne, ponieważ potrzebne są kursy tylko dla wybranego instruktora. Jednak w tym przykładzie pokazano, jak używać eager ładowania dla właściwości nawigacji w obrębie jednostek, które są same we właściwościach nawigacji.
- Gdy użytkownik wybierze kurs, zostanie wyświetlona powiązana dane z zestawu jednostek `Enrollments`. Jednostki `Course` i `Enrollment` znajdują się w relacji jeden do wielu. Należy dodać jawne ładowanie dla jednostek `Enrollment` i powiązanych z nimi jednostek `Student`. (Jawne ładowanie nie jest konieczne, ponieważ jest włączone ładowanie z opóźnieniem, ale pokazuje, jak przeprowadzić jawne ładowanie).

### <a name="create-a-view-model-for-the-instructor-index-view"></a>Utwórz model widoku dla widoku indeksu instruktora

Na stronie indeks instruktora są wyświetlane trzy różne tabele. W związku z tym utworzysz model widoku zawierający trzy właściwości, z których każda będzie zawierać dane dla jednej z tabel.

W folderze *modele widoków* Utwórz *InstructorIndexData.cs* i Zastąp istniejący kod następującym kodem:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

### <a name="adding-a-style-for-selected-rows"></a>Dodawanie stylu dla wybranych wierszy

Aby oznaczyć wybrane wiersze, potrzebny jest inny kolor tła. Aby podać styl dla tego interfejsu użytkownika, Dodaj następujący wyróżniony kod do sekcji `/* info and errors */` w *Content\Site.css*, jak pokazano poniżej:

[!code-css[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.css?highlight=2-5)]

### <a name="creating-the-instructor-controller-and-views"></a>Tworzenie kontrolera i widoków instruktora

Utwórz kontroler `InstructorController`, jak pokazano na poniższej ilustracji:

![Add_Controller_dialog_box_for_Instructor_controller](https://asp.net/media/2577909/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Add_Controller_dialog_box_for_Instructor_controller_f99c10aa-1efd-49d6-af1d-b00461616107.png)

Otwórz *Controllers\InstructorController.cs* i dodaj instrukcję `using` dla przestrzeni nazw `ViewModels`:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Kod szkieletowy w metodzie `Index` określa eager ładowania tylko dla właściwości nawigacji `OfficeAssignment`:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Zastąp metodę `Index` poniższym kodem, aby załadować dodatkowe powiązane dane i umieścić je w modelu widoku:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Metoda akceptuje opcjonalne dane trasy (`id`) i parametr ciągu zapytania (`courseID`), które zawierają wartości identyfikatora wybranego instruktora i wybranego kursu, i przekazuje wszystkie wymagane dane do widoku. Parametry są udostępniane przez **Wybieranie** hiperlinków na stronie.

> [!TIP]
> 
> **Dane trasy**
> 
> Dane trasy to dane, które spinacz modelu znalazł w segmencie adresów URL określonym w tabeli routingu. Na przykład trasa domyślna określa `controller`, `action`i `id` segmentów:
> 
> ```csharp
> routes.MapRoute(  
>  name: "Default",  
>  url: "{controller}/{action}/{id}",  
>  defaults: new { controller = "Home", action = "Index", id = UrlParameter.Optional }  
> );
> ```
> 
> W poniższym adresie URL trasy domyślne są mapowane `Instructor` jako `controller`, `Index` jako `action` i 1 jako `id`; są to wartości danych tras.
> 
> `http://localhost:1230/Instructor/Index/1?courseID=2021`
> 
> "? courseID = 2021" jest wartością ciągu zapytania. Model spinacza również będzie działał w przypadku przekazania `id` jako wartości ciągu zapytania:
> 
> `http://localhost:1230/Instructor/Index?id=1&CourseID=2021`
> 
> Adresy URL są tworzone przez instrukcje `ActionLink` w widoku Razor. W poniższym kodzie parametr `id` jest zgodny z domyślną trasą, więc `id` jest dodawany do danych trasy.
> 
> [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cshtml)]
> 
> W poniższym kodzie `courseID` nie jest zgodny z parametrem w domyślnej trasie, więc jest dodawana jako ciąg zapytania.
> 
> [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cshtml)]

Kod rozpoczyna się od utworzenia wystąpienia modelu widoku i umieszczenie go w liście instruktorów. Kod określa eager ładowania dla `Instructor.OfficeAssignment` i `Instructor.Courses` właściwość nawigacji.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs?highlight=3-4)]

Druga metoda `Include` ładuje kursy i dla każdego załadowanego kursu wykonuje eager ładowania dla właściwości nawigacji `Course.Department`.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

Jak wspomniano wcześniej, ładowanie eager nie jest wymagane, ale jest gotowe do zwiększenia wydajności. Ponieważ widok zawsze wymaga jednostki `OfficeAssignment`, można go pobrać w tym samym zapytaniu. jednostki `Course` są wymagane w przypadku wybrania na stronie sieci Web instruktora, więc ładowanie eager jest lepsze niż ładowanie z opóźnieniem, tylko wtedy, gdy strona jest wyświetlana częściej jako kurs wybrany niż bez.

W przypadku wybrania identyfikatora instruktora wybrany instruktor jest pobierany z listy instruktorów w modelu widoku. Właściwość `Courses` modelu widoku jest następnie ładowana z jednostkami `Course` z tej właściwości nawigacji tego `Courses` instruktora.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

Metoda `Where` zwraca kolekcję, ale w tym przypadku kryteria przesłane do tej metody powodują, że zwracana jest tylko jedna jednostka `Instructor`. Metoda `Single` konwertuje kolekcję na jedną jednostkę `Instructor`, która zapewnia dostęp do właściwości `Courses` tej jednostki.

Możesz użyć [pojedynczej](https://msdn.microsoft.com/library/system.linq.enumerable.single.aspx) metody w kolekcji, Jeśli wiesz, że kolekcja będzie zawierać tylko jeden element. Metoda `Single` zgłasza wyjątek, jeśli kolekcja została przeniesiona do niej jest pusta lub jeśli istnieje więcej niż jeden element. Alternatywą jest [SingleOrDefault](https://msdn.microsoft.com/library/bb342451.aspx), która zwraca wartość domyślną (`null` w tym przypadku), jeśli kolekcja jest pusta. Jednak w takim przypadku nadal może wystąpić wyjątek (od próby znalezienia właściwości `Courses` w odwołaniu `null`), a komunikat o wyjątku będzie mniej jasno wskazywał przyczynę problemu. Po wywołaniu metody `Single` można również przekazać warunek `Where` zamiast wywołania `Where` metody osobno:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

Zamiast:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

Następnie, jeśli wybrano kurs, wybrany kurs zostanie pobrany z listy kursów w modelu widoku. Następnie właściwość `Enrollments` modelu widoku jest ładowana z jednostkami `Enrollment` z `Enrollments` właściwości nawigacji tego kursu.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs)]

### <a name="modifying-the-instructor-index-view"></a>Modyfikowanie widoku indeksu instruktora

W *Views\Instructor\Index.cshtml*Zastąp istniejący kod następującym kodem. Zmiany są wyróżnione:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml?highlight=1,4,18,22-27,29,43-48)]

Wprowadzono następujące zmiany w istniejącym kodzie:

- Zmieniono klasę modelu na `InstructorIndexData`.
- Zmieniono tytuł strony z **indeksu** na **Instruktorzy**.
- Przenosi kolumny łączy wierszy z lewej strony.
- Usunięto kolumnę **FullName** .
- Dodano kolumnę **pakietu Office** , która wyświetla `item.OfficeAssignment.Location` tylko wtedy, gdy `item.OfficeAssignment` nie ma wartości null. (Ponieważ jest to relacja "jeden do zera" lub jeden-do-jednego, może nie być powiązana jednostka `OfficeAssignment`). 

    [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]
- Dodano kod, który dynamicznie dodaje `class="selectedrow"` do elementu `tr` wybranego instruktora. Spowoduje to ustawienie koloru tła dla wybranego wiersza przy użyciu utworzonej wcześniej klasy CSS. (Atrybut `valign` będzie przydatny w poniższym samouczku, gdy dodasz do tabeli kolumnę wielowierszową). 

    [!code-html[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.html)]
- Dodano nowe `ActionLink` z etykietą **Wybierz** bezpośrednio przed innymi łączami w każdym wierszu, co spowoduje wysłanie wybranego identyfikatora instruktora do metody `Index`.

Uruchom aplikację i wybierz kartę **Instruktorzy** . Na stronie jest wyświetlana właściwość `Location` powiązanych jednostek `OfficeAssignment` i pustej komórki tabeli, gdy nie ma żadnej powiązanej jednostki `OfficeAssignment`.

![Instructors_index_page_with_nothing_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

W pliku *Views\Instructor\Index.cshtml* po zamykającym elemencie `table` (na końcu pliku) Dodaj następujący wyróżniony kod. Spowoduje to wyświetlenie listy kursów związanych z instruktorem w przypadku wybrania instruktora.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml?highlight=11-46)]

Ten kod odczytuje Właściwość `Courses` modelu widoku w celu wyświetlenia listy kursów. Udostępnia również hiperłącze `Select`, które wysyła identyfikator wybranego kursu do metody akcji `Index`.

> [!NOTE]
> Plik *. css* jest buforowany przez przeglądarki. Jeśli nie widzisz zmian podczas uruchamiania aplikacji, wykonaj twarde odświeżanie (przytrzymaj klawisz CTRL podczas klikania przycisku **Odśwież** lub naciśnij klawisze CTRL + F5).

Uruchom stronę i wybierz instruktora. Teraz zobaczysz siatkę wyświetlającą kursy przypisane do wybranego instruktora, a dla każdego kursu zobaczysz nazwę przypisanego działu.

![Instructors_index_page_with_instructor_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Po dodaniu bloku kodu Dodaj następujący kod. Spowoduje to wyświetlenie listy studentów, którzy są rejestrowani w kursie po wybraniu tego kursu.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cshtml)]

Ten kod odczytuje Właściwość `Enrollments` modelu widoku w celu wyświetlenia listy uczniów zarejestrowanych w kursie.

Uruchom stronę i wybierz instruktora. Następnie wybierz kurs, aby zobaczyć listę zarejestrowanych studentów i ich klasy.

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

### <a name="adding-explicit-loading"></a>Dodawanie jawnego ładowania

Otwórz *InstructorController.cs* i sprawdź, jak Metoda `Index` pobiera listę rejestracji dla wybranego kursu:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cs)]

Po pobraniu listy instruktorów należy określić eager do załadowania dla właściwości nawigacji `Courses` i dla właściwości `Department` każdego kursu. Następnie należy umieścić kolekcję `Courses` w modelu widoku i teraz uzyskujesz dostęp do właściwości nawigacji `Enrollments` z jednej jednostki w tej kolekcji. Ponieważ nie określono eager ładowania dla właściwości nawigacji `Course.Enrollments`, dane z tej właściwości są wyświetlane na stronie jako wynik ładowania z opóźnieniem.

Jeśli ładowanie z opóźnieniem zostało wyłączone bez zmiany kodu w inny sposób, właściwość `Enrollments` będzie miała wartość null, niezależnie od tego, ile rejestracji rzeczywiście miał kurs. W takim przypadku w celu załadowania właściwości `Enrollments` należy określić ładowanie eager lub jawne. Zaobserwowano już, jak przeprowadzić ładowanie eager. Aby zobaczyć przykład jawnego ładowania, Zastąp metodę `Index` poniższym kodem, który jawnie załaduje Właściwość `Enrollments`. Kod zmieniony jest wyróżniony.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample24.cs?highlight=20-27)]

Po pobraniu wybranej jednostki `Course` nowy kod jawnie załaduje właściwość nawigacji `Enrollments` tego kursu:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample25.cs)]

Następnie jawnie wczytuje `Enrollment` jednostki `Student` powiązanej jednostki:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample26.cs)]

Zwróć uwagę, że używasz metody `Collection` do załadowania właściwości kolekcji, ale dla właściwości, która zawiera tylko jedną jednostkę, używasz metody `Reference`. Możesz teraz uruchomić stronę indeks instruktora i zobaczyć, że nie ma żadnych różnic w wyświetlanych na stronie, chociaż zmieniono sposób pobierania danych.

## <a name="summary"></a>Podsumowanie

Wszystkie trzy metody (z opóźnieniem, Eager i Explicit) zostały już użyte do załadowania powiązanych danych do właściwości nawigacji. W następnym samouczku dowiesz się, jak zaktualizować powiązane dane.

Linki do innych zasobów Entity Framework można znaleźć w [mapie zawartości dostęp do danych ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Poprzednie](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
> [dalej](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)

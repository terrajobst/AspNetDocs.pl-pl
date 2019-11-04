---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: 'Samouczek: odczytywanie powiązanych danych za pomocą EF w aplikacji ASP.NET MVC'
description: W tym samouczku pokazano, jak odczytywać i wyświetlać powiązane dane — czyli dane, które Entity Framework ładowane do właściwości nawigacji.
author: tdykstra
ms.author: riande
ms.date: 01/22/2019
ms.topic: tutorial
ms.assetid: 18cdd896-8ed9-4547-b143-114711e3eafb
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: d804c8dd45ad131949260c85d9d9c6683bfe9646
ms.sourcegitcommit: 84b1681d4e6253e30468c8df8a09fe03beea9309
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/02/2019
ms.locfileid: "73445654"
---
# <a name="tutorial-read-related-data-with-ef-in-an-aspnet-mvc-app"></a>Samouczek: odczytywanie powiązanych danych za pomocą EF w aplikacji ASP.NET MVC

W poprzednim samouczku został ukończony model danych szkoły. W tym samouczku pokazano, jak odczytywać i wyświetlać powiązane dane — czyli dane, które Entity Framework ładowane do właściwości nawigacji.

Na poniższych ilustracjach przedstawiono strony, z którymi będziesz korzystać.

![](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

[Pobierz ukończony projekt](https://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)

> Przykładowa aplikacja internetowa Contoso University pokazuje, jak tworzyć aplikacje ASP.NET MVC 5 za pomocą Code First Entity Framework 6 i programu Visual Studio. Aby uzyskać informacje na temat serii samouczków, zobacz [pierwszy samouczek w serii](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

W tym samouczku przedstawiono następujące instrukcje:

> [!div class="checklist"]
> * Dowiedz się, jak ładować powiązane dane
> * Utwórz stronę kursów
> * Tworzenie strony instruktorów

## <a name="prerequisites"></a>Wymagania wstępne

* [Tworzenie bardziej złożonego modelu danych](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)

## <a name="learn-how-to-load-related-data"></a>Dowiedz się, jak ładować powiązane dane

Istnieje kilka Entity Framework sposobów załadowania powiązanych danych do właściwości nawigacji jednostki:

- *Ładowanie z opóźnieniem*. Gdy obiekt jest najpierw odczytywany, powiązane dane nie są pobierane. Jednak przy pierwszej próbie uzyskania dostępu do właściwości nawigacji dane wymagane dla tej właściwości nawigacji są pobierane automatycznie. Powoduje to, że wiele zapytań jest wysyłanych do bazy danych — jeden dla samej jednostki i po każdym każdej chwili, gdy powiązane dane dla jednostki muszą zostać pobrane. Klasa `DbContext` domyślnie włącza ładowanie z opóźnieniem.

    ![Lazy_loading_example](https://asp.net/media/2577850/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Lazy_loading_example_2c44eabb-5fd3-485a-837d-8e3d053f2c0c.png)
- *Ładowanie eager*. Po odczytaniu jednostki są pobierane powiązane dane. Zwykle powoduje to pojedyncze zapytanie sprzężenia, które pobiera wszystkie dane, które są zbędne. Należy określić eager ładowania przy użyciu metody `Include`.

    ![Eager_loading_example](https://asp.net/media/2577856/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Eager_loading_example_33f907ff-f0b0-4057-8e75-05a8cacac807.png)
- *Jawne ładowanie*. Jest to podobne do ładowania z opóźnieniem, z tą różnicą, że jawnie pobierasz powiązane dane w kodzie; nie odbywa się to automatycznie podczas uzyskiwania dostępu do właściwości nawigacji. Powiązane dane są ładowane ręcznie poprzez pobranie wpisu menedżera stanu obiektów dla jednostki i wywołanie [kolekcji. Metoda Load](https://msdn.microsoft.com/library/gg696220(v=vs.103).aspx) dla kolekcji lub metoda [Reference. Load](https://msdn.microsoft.com/library/gg679166(v=vs.103).aspx) dla właściwości, które zawierają pojedynczą jednostkę. (W poniższym przykładzie, jeśli chcesz załadować właściwość nawigacji administratora, Zastąp `Collection(x => x.Courses)` z `Reference(x => x.Administrator)`.) Zazwyczaj użycie jawnego ładowania jest możliwe tylko wtedy, gdy włączono pobieranie z opóźnieniem.

    ![Explicit_loading_example](https://asp.net/media/2577862/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Explicit_loading_example_79d8c368-6d82-426f-be9a-2b443644ab15.png)

Ponieważ nie pobierają one natychmiast wartości właściwości, ładowanie z opóźnieniem i jawne ładowanie są również nazywane *opóźnionym ładowaniem*.

### <a name="performance-considerations"></a>Zagadnienia dotyczące wydajności

Jeśli wiesz, że potrzebujesz pokrewnych danych dla każdej pobranej jednostki, ładowanie eager często oferuje najlepszą wydajność, ponieważ pojedyncze zapytanie wysyłane do bazy danych jest zwykle bardziej wydajne niż osobne zapytania dla każdej pobranej jednostki. Na przykład w powyższych przykładach Załóżmy, że każdy dział ma dziesięć powiązanych kursów. Przykład ładowania eager może skutkować pojedynczym zapytaniem (sprzężeniem) i pojedynczą rundą w bazie danych. Przykłady ładowania z opóźnieniem i jawnego ładowania powodują jedenaście zapytań i jedenaście rund do bazy danych. Dodatkowe podróże do bazy danych są szczególnie niekorzystne w przypadku opóźnień.

Z drugiej strony, w niektórych scenariuszach ładowanie z opóźnieniem jest bardziej wydajne. Ładowanie eager może spowodować wygenerowanie bardzo złożonej sprzężenia, które SQL Server nie może przetwarzać efektywnie. Lub jeśli musisz uzyskać dostęp do właściwości nawigacji jednostki tylko dla podzbioru zestawu jednostek, które są przetwarzane, ładowanie z opóźnieniem może być lepsze, ponieważ ładowanie eager spowodowałoby pobranie większej ilości danych niż jest to potrzebne. Jeśli wydajność ma kluczowe znaczenie, najlepszym rozwiązaniem jest przetestowanie wydajności obu metod w celu zapewnienia najlepszego wyboru.

Ładowanie z opóźnieniem może maskować kod, który powoduje problemy z wydajnością. Na przykład kod, który nie określa eager lub bezpośredniego ładowania, ale przetwarza dużą liczbę jednostek i używa kilku właściwości nawigacji w każdej iteracji, może być bardzo wydajny (ze względu na wiele podróży do bazy danych). Aplikacja, która działa dobrze podczas opracowywania przy użyciu lokalnego programu SQL Server, może mieć problemy z wydajnością podczas przenoszenia do Azure SQL Database z powodu zwiększonego opóźnienia i obciążenia z opóźnieniem. Profilowanie zapytań bazy danych przy użyciu realistycznego obciążenia testowego pomoże określić, czy ładowanie z opóźnieniem jest odpowiednie. Aby uzyskać więcej informacji, zobacz [sztuczna Entity Framework Strategies: ładowanie powiązanych danych](https://msdn.microsoft.com/magazine/hh205756.aspx) i [Korzystanie z Entity Framework, aby zmniejszyć opóźnienie sieci do usługi SQL Azure](https://msdn.microsoft.com/magazine/gg309181.aspx).

### <a name="disable-lazy-loading-before-serialization"></a>Wyłącz ładowanie z opóźnieniem przed serializacją

W przypadku pozostawienia włączonego ładowania z opóźnieniem podczas serializacji można zakończyć wykonywanie zapytań znacznie większej ilości danych niż zamierzone. Serializacja zazwyczaj działa przez uzyskanie dostępu do każdej właściwości w wystąpieniu typu. Dostęp do właściwości wyzwala ładowanie z opóźnieniem, a te jednostki załadowane z opóźnieniem są serializowane. Następnie proces serializacji uzyskuje dostęp do każdej właściwości jednostek ładowanych z opóźnieniem, co może spowodować jeszcze większą liczbę operacji ładowania i serializacji z opóźnieniem. Aby zapobiec tej reakcji łańcucha uruchamiania, przed serializacji jednostki należy wyłączyć pobieranie z opóźnieniem.

Serializacja może być również skomplikowana przez klasy proxy używane przez Entity Framework, zgodnie z opisem w [samouczku scenariusze zaawansowane](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#proxies).

Jednym ze sposobów uniknięcia problemów serializacji jest Serializowanie obiektów transferu danych (DTO) zamiast obiektów jednostek, jak pokazano w samouczku [using Web API with Entity Framework](../../../../web-api/overview/data/using-web-api-with-entity-framework/part-5.md) .

Jeśli nie korzystasz z DTO, możesz wyłączyć ładowanie z opóźnieniem i uniknąć problemów z serwerem proxy, [wyłączając Tworzenie serwera proxy](https://msdn.microsoft.com/data/jj592886.aspx).

Oto kilka innych [sposobów wyłączenia ładowania z opóźnieniem](https://msdn.microsoft.com/data/jj574232):

- Dla określonych właściwości nawigacji, Pomiń słowo kluczowe `virtual` podczas deklarowania właściwości.
- Dla wszystkich właściwości nawigacji Ustaw `LazyLoadingEnabled` na `false`, umieść następujący kod w konstruktorze klasy kontekstu:

    [!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

## <a name="create-a-courses-page"></a>Utwórz stronę kursów

Jednostka `Course` obejmuje właściwość nawigacji, która zawiera `Department` jednostkę działu, do której jest przypisany kurs. Aby wyświetlić nazwę przypisanego działu na liście kursów, należy uzyskać Właściwość `Name` z jednostki `Department`, która znajduje się we właściwości nawigacji `Course.Department`.

Utwórz kontroler o nazwie `CourseController` (nie CoursesController) dla typu jednostki `Course`, używając tych samych opcji dla **kontrolera MVC 5 z widokami, używając narzędzia Entity Framework** szkieletem wcześniej dla kontrolera `Student`:

| Ustawienie | Wartość |
| ------- | ----- |
| Klasa modelu | Wybierz pozycję **kurs (ContosoUniversity. models)** . |
| Klasa kontekstu danych | Wybierz pozycję **SchoolContext (ContosoUniversity. dal)** . |
| Nazwa kontrolera | Wprowadź *CourseController*. Ponownie, nie *CoursesController* z *s*. W przypadku wybrania **kursu (ContosoUniversity. models)** wartość **nazwy kontrolera** została automatycznie wypełniona. Musisz zmienić wartość. |

Pozostaw pozostałe wartości domyślne i Dodaj kontroler.

Otwórz *Controllers\CourseController.cs* i przyjrzyj się metodzie `Index`:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Funkcja automatycznego tworzenia szkieletów określiła eager ładowania dla właściwości nawigacji `Department` przy użyciu metody `Include`.

Otwórz *Views\Course\Index.cshtml* i Zastąp kod szablonu poniższym kodem. Zmiany są wyróżnione:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=4,7,14-16,23-25,31-33,40-42)]

Do kodu szkieletowego wprowadzono następujące zmiany:

- Zmieniono nagłówek z **indeksu** na **kursy**.
- Dodano kolumnę **liczbową** , która wyświetla wartość właściwości `CourseID`. Domyślnie klucze podstawowe nie są szkieletowe, ponieważ zazwyczaj nie mają znaczenia dla użytkowników końcowych. Jednak w tym przypadku klucz podstawowy ma znaczenie i chcesz go wyświetlić.
- Przeniesienie kolumny **działu** z prawej strony i zmiana jej nagłówka. Program szkieletowy prawidłowo zdecydował się na wyświetlenie właściwości `Name` z jednostki `Department`, ale w tym miejscu na stronie kursu nagłówek kolumny powinien być **działem** , a nie **nazwą**.

Należy zauważyć, że dla kolumny dział, kod szkieletowy wyświetla Właściwość `Name` jednostki `Department`, która jest załadowana do właściwości nawigacji `Department`:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml?highlight=2)]

Uruchom stronę (Wybierz kartę **kursy** na stronie głównej programu Contoso University), aby wyświetlić listę z nazwami działów.

## <a name="create-an-instructors-page"></a>Tworzenie strony instruktorów

W tej sekcji utworzysz kontroler i widok dla jednostki `Instructor`, aby wyświetlić stronę instruktorów. Ta strona odczytuje i wyświetla powiązane dane w następujący sposób:

- Lista instruktorów wyświetla powiązane dane z jednostki `OfficeAssignment`. Jednostki `Instructor` i `OfficeAssignment` znajdują się w relacji jeden-do-zero-lub-jednego. Użyjesz eager ładowania dla jednostek `OfficeAssignment`. Jak wyjaśniono wcześniej, ładowanie eager jest zwykle bardziej wydajne, gdy potrzebne są powiązane dane dla wszystkich pobranych wierszy tabeli podstawowej. W takim przypadku chcesz wyświetlić przypisania pakietu Office dla wszystkich wyświetlanych instruktorów.
- Gdy użytkownik wybierze instruktora, zostaną wyświetlone powiązane jednostki `Course`. Jednostki `Instructor` i `Course` znajdują się w relacji wiele-do-wielu. Będziesz używać eager ładowania dla jednostek `Course` i powiązanych z nimi jednostek `Department`. W takim przypadku ładowanie z opóźnieniem może być bardziej wydajne, ponieważ potrzebne są kursy tylko dla wybranego instruktora. Jednak w tym przykładzie pokazano, jak używać eager ładowania dla właściwości nawigacji w obrębie jednostek, które są same we właściwościach nawigacji.
- Gdy użytkownik wybierze kurs, zostanie wyświetlona powiązana dane z zestawu jednostek `Enrollments`. Jednostki `Course` i `Enrollment` znajdują się w relacji jeden-do-wielu. Należy dodać jawne ładowanie dla jednostek `Enrollment` i powiązanych z nimi jednostek `Student`. (Jawne ładowanie nie jest konieczne, ponieważ jest włączone ładowanie z opóźnieniem, ale pokazuje, jak przeprowadzić jawne ładowanie).

### <a name="create-a-view-model-for-the-instructor-index-view"></a>Utwórz model widoku dla widoku indeksu instruktora

Na stronie instruktorzy są wyświetlane trzy różne tabele. W związku z tym utworzysz model widoku zawierający trzy właściwości, z których każda będzie zawierać dane dla jednej z tabel.

W folderze *modele widoków* Utwórz *InstructorIndexData.cs* i Zastąp istniejący kod następującym kodem:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

### <a name="create-the-instructor-controller-and-views"></a>Tworzenie kontrolera i widoków instruktora

Utwórz kontroler `InstructorController` (nie InstructorsController) z akcją odczytu/zapisu EF:

| Ustawienie | Wartość |
| ------- | ----- |
| Klasa modelu | Wybierz pozycję **instruktor (ContosoUniversity. models)** . |
| Klasa kontekstu danych | Wybierz pozycję **SchoolContext (ContosoUniversity. dal)** . |
| Nazwa kontrolera | Wprowadź *InstructorController*. Ponownie, nie *InstructorsController* z *s*. W przypadku wybrania **kursu (ContosoUniversity. models)** wartość **nazwy kontrolera** została automatycznie wypełniona. Musisz zmienić wartość. |

Pozostaw pozostałe wartości domyślne i Dodaj kontroler.

Otwórz *Controllers\InstructorController.cs* i dodaj instrukcję `using` dla przestrzeni nazw `ViewModels`:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

Kod szkieletowy w metodzie `Index` określa eager ładowania tylko dla właściwości nawigacji `OfficeAssignment`:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Zastąp metodę `Index` poniższym kodem, aby załadować dodatkowe powiązane dane i umieścić je w modelu widoku:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Metoda akceptuje opcjonalne dane trasy (`id`) i parametr ciągu zapytania (`courseID`), które zawierają wartości identyfikatora wybranego instruktora i wybranego kursu, i przekazuje wszystkie wymagane dane do widoku. Parametry są udostępniane przez **Wybieranie** hiperlinków na stronie.

Kod rozpoczyna się od utworzenia wystąpienia modelu widoku i umieszczenie go w liście instruktorów. Kod określa eager ładowania dla `Instructor.OfficeAssignment` i `Instructor.Courses` właściwość nawigacji.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs?highlight=3-4)]

Druga metoda `Include` ładuje kursy i dla każdego załadowanego kursu wykonuje eager ładowania dla właściwości nawigacji `Course.Department`.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

Jak wspomniano wcześniej, ładowanie eager nie jest wymagane, ale jest gotowe do zwiększenia wydajności. Ponieważ widok zawsze wymaga jednostki `OfficeAssignment`, można go pobrać w tym samym zapytaniu. jednostki `Course` są wymagane w przypadku wybrania na stronie sieci Web instruktora, więc ładowanie eager jest lepsze niż ładowanie z opóźnieniem, tylko wtedy, gdy strona jest wyświetlana częściej jako kurs wybrany niż bez.

W przypadku wybrania identyfikatora instruktora wybrany instruktor jest pobierany z listy instruktorów w modelu widoku. Właściwość `Courses` modelu widoku jest następnie ładowana z jednostkami `Course` z tej właściwości nawigacji tego `Courses` instruktora.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

Metoda `Where` zwraca kolekcję, ale w tym przypadku kryteria przesłane do tej metody powodują, że zwracana jest tylko jedna jednostka `Instructor`. Metoda `Single` konwertuje kolekcję na jedną jednostkę `Instructor`, która zapewnia dostęp do właściwości `Courses` tej jednostki.

Możesz użyć [pojedynczej](https://msdn.microsoft.com/library/system.linq.enumerable.single.aspx) metody w kolekcji, Jeśli wiesz, że kolekcja będzie zawierać tylko jeden element. Metoda `Single` zgłasza wyjątek, jeśli kolekcja została przeniesiona do niej jest pusta lub jeśli istnieje więcej niż jeden element. Alternatywą jest [SingleOrDefault](https://msdn.microsoft.com/library/bb342451.aspx), która zwraca wartość domyślną (`null` w tym przypadku), jeśli kolekcja jest pusta. Jednak w takim przypadku nadal może wystąpić wyjątek (od próby znalezienia właściwości `Courses` w odwołaniu `null`), a komunikat o wyjątku będzie mniej jasno wskazywał przyczynę problemu. Po wywołaniu metody `Single` można również przekazać warunek `Where` zamiast wywołania `Where` metody osobno:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]

Zamiast:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

Następnie, jeśli wybrano kurs, wybrany kurs zostanie pobrany z listy kursów w modelu widoku. Następnie właściwość `Enrollments` modelu widoku jest ładowana z jednostkami `Enrollment` z `Enrollments` właściwości nawigacji tego kursu.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

### <a name="modify-the-instructor-index-view"></a>Modyfikowanie widoku indeksu instruktora

W *Views\Instructor\Index.cshtml*Zastąp kod szablonu poniższym kodem. Zmiany są wyróżnione:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cshtml?highlight=1,4,14-18,21,23-28,38-43,45)]

Wprowadzono następujące zmiany w istniejącym kodzie:

- Zmieniono klasę modelu na `InstructorIndexData`.
- Zmieniono tytuł strony z **indeksu** na **Instruktorzy**.
- Dodano kolumnę **pakietu Office** , która wyświetla `item.OfficeAssignment.Location` tylko wtedy, gdy `item.OfficeAssignment` nie ma wartości null. (Ponieważ jest to relacja "jeden do zera" lub jeden-do-jednego, może nie być powiązana jednostka `OfficeAssignment`).

    [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml)]
- Dodano kod, który dynamicznie dodaje `class="success"` do elementu `tr` wybranego instruktora. Ustawia kolor tła dla wybranego wiersza przy użyciu klasy [Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap) .

    [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]
- Dodano nowe `ActionLink` z etykietą **Wybierz** bezpośrednio przed innymi łączami w każdym wierszu, co spowoduje wysłanie wybranego identyfikatora instruktora do metody `Index`.

Uruchom aplikację i wybierz kartę **Instruktorzy** . Na stronie jest wyświetlana właściwość `Location` powiązanych jednostek `OfficeAssignment` i pustej komórki tabeli, gdy nie ma żadnej powiązanej jednostki `OfficeAssignment`.

W pliku *Views\Instructor\Index.cshtml* po zamykającym elemencie `table` (na końcu pliku) Dodaj następujący kod. Ten kod wyświetla listę kursów związanych z instruktorem w przypadku wybrania instruktora.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml)]

Ten kod odczytuje Właściwość `Courses` modelu widoku, aby wyświetlić listę kursów. Udostępnia również hiperłącze `Select`, które wysyła identyfikator wybranego kursu do metody akcji `Index`.

Uruchom stronę i wybierz instruktora. Teraz zobaczysz siatkę wyświetlającą kursy przypisane do wybranego instruktora, a dla każdego kursu zobaczysz nazwę przypisanego działu.

Po dodaniu bloku kodu Dodaj następujący kod. Spowoduje to wyświetlenie listy studentów, którzy są rejestrowani w kursie po wybraniu tego kursu.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]

Ten kod odczytuje Właściwość `Enrollments` modelu widoku w celu wyświetlenia listy uczniów zarejestrowanych w kursie.

Uruchom stronę i wybierz instruktora. Następnie wybierz kurs, aby zobaczyć listę zarejestrowanych studentów i ich klasy.

### <a name="adding-explicit-loading"></a>Dodawanie jawnego ładowania

Otwórz *InstructorController.cs* i sprawdź, jak Metoda `Index` pobiera listę rejestracji dla wybranego kursu:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs)]

Po pobraniu listy instruktorów należy określić eager do załadowania dla właściwości nawigacji `Courses` i dla właściwości `Department` każdego kursu. Następnie należy umieścić kolekcję `Courses` w modelu widoku i teraz uzyskujesz dostęp do właściwości nawigacji `Enrollments` z jednej jednostki w tej kolekcji. Ponieważ nie określono eager ładowania dla właściwości nawigacji `Course.Enrollments`, dane z tej właściwości są wyświetlane na stronie jako wynik ładowania z opóźnieniem.

Jeśli ładowanie z opóźnieniem zostało wyłączone bez zmiany kodu w inny sposób, właściwość `Enrollments` będzie miała wartość null, niezależnie od tego, ile rejestracji rzeczywiście miał kurs. W takim przypadku w celu załadowania właściwości `Enrollments` należy określić ładowanie eager lub jawne. Zaobserwowano już, jak przeprowadzić ładowanie eager. Aby zobaczyć przykład jawnego ładowania, Zastąp metodę `Index` poniższym kodem, który jawnie załaduje Właściwość `Enrollments`. Kod zmieniony jest wyróżniony.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs?highlight=20-31)]

Po pobraniu wybranej jednostki `Course` nowy kod jawnie załaduje właściwość nawigacji `Enrollments` tego kursu:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

Następnie jawnie wczytuje `Enrollment` jednostki `Student` powiązanej jednostki:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cs)]

Zwróć uwagę, że używasz metody `Collection` do załadowania właściwości kolekcji, ale dla właściwości, która zawiera tylko jedną jednostkę, używasz metody `Reference`.

Uruchom teraz stronę indeks instruktora i nie zobaczysz żadnych różnic w wyświetlanych na stronie, chociaż zmieniono sposób pobierania danych.

## <a name="get-the-code"></a>Pobierz kod

[Pobierz ukończony projekt](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Dodatkowe zasoby

Linki do innych zasobów Entity Framework można znaleźć w [zasobach zalecanych przez dostęp do danych ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

## <a name="next-steps"></a>Następne kroki

W tym samouczku przedstawiono następujące instrukcje:

> [!div class="checklist"]
> * Dowiesz się, jak ładować powiązane dane
> * Utworzono stronę kursów
> * Utworzono stronę instruktorów

Przejdź do następnego artykułu, aby dowiedzieć się, jak zaktualizować powiązane dane.

> [!div class="nextstepaction"]
> [Aktualizowanie powiązanych danych](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)

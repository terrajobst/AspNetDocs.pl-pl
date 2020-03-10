---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-a-more-complex-data-model-for-an-asp-net-mvc-application
title: 'Samouczek: tworzenie bardziej złożonego modelu danych dla aplikacji ASP.NET MVC'
author: tdykstra
description: W tym samouczku dowiesz się więcej o jednostkach i relacjach, a następnie dostosuj model danych, określając formatowanie, walidację i reguły mapowania bazy danych.
ms.author: riande
ms.date: 01/22/2019
ms.topic: tutorial
ms.assetid: 46f7f3c9-274f-4649-811d-92222a9b27e2
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-a-more-complex-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 933354b09eb4674e6f523f8a65816410521f026f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78583329"
---
# <a name="tutorial-create-a-more-complex-data-model-for-an-aspnet-mvc-app"></a>Samouczek: tworzenie bardziej złożonego modelu danych dla aplikacji ASP.NET MVC

W poprzednich samouczkach pracowałeś z prostym modelem danych, który składa się z trzech jednostek. W tym samouczku dodasz więcej jednostek i relacji i dostosowasz model danych, określając formatowanie, walidację i reguły mapowania bazy danych. W tym artykule przedstawiono dwa sposoby dostosowywania modelu danych: przez dodanie atrybutów do klas jednostek i przez dodanie kodu do klasy kontekstu bazy danych.

Po zakończeniu klasy jednostek składają się na ukończony model danych przedstawiony na poniższej ilustracji:

![School_class_diagram](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image1.png)

W tym samouczku zostaną wykonane następujące czynności:

> [!div class="checklist"]
> * Dostosowywanie modelu danych
> * Aktualizuj jednostkę ucznia
> * Utwórz jednostkę instruktora
> * Tworzenie jednostki OfficeAssignment
> * Modyfikowanie jednostki kursu
> * Tworzenie jednostki działu
> * Modyfikowanie jednostki rejestracji
> * Dodaj kod do kontekstu bazy danych
> * Wypełnianie bazy danych z danymi testowymi
> * Dodawanie migracji
> * Aktualizowanie bazy danych

## <a name="prerequisites"></a>Wymagania wstępne

* [Code First migracji i wdrożenia](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="customize-the-data-model"></a>Dostosowywanie modelu danych

W tej sekcji dowiesz się, jak dostosować model danych przy użyciu atrybutów, które określają formatowanie, walidację i reguły mapowania bazy danych. Następnie w kilku poniższych sekcjach utworzysz kompletny `School` model danych, dodając atrybuty do klas, które zostały już utworzone, i tworząc nowe klasy dla pozostałych typów jednostek w modelu.

### <a name="the-datatype-attribute"></a>Atrybut DataType

W przypadku dat rejestracji uczniów na wszystkich stronach sieci Web jest obecnie wyświetlany czas wraz z datą, chociaż wszystko, co jest ważne dla tego pola, to Data. Używając atrybutów adnotacji danych, można wprowadzić jedną zmianę kodu, która naprawi format wyświetlania w każdym widoku, który wyświetla dane. Aby zapoznać się z przykładem, jak to zrobić, Dodaj atrybut do właściwości `EnrollmentDate` w klasie `Student`.

W *Models\Student.cs*dodaj instrukcję `using` dla przestrzeni nazw `System.ComponentModel.DataAnnotations` i dodaj atrybuty `DataType` i `DisplayFormat` do właściwości `EnrollmentDate`, jak pokazano w następującym przykładzie:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,12-13)]

Atrybut [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) służy do określania typu danych, który jest bardziej szczegółowy niż typ wewnętrzny bazy danych. W tym przypadku chcemy tylko śledzić datę, a nie datę i godzinę. [Wyliczenie DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) zawiera wiele typów danych, takich jak *Data, godzina, numer telefonu, waluta, EmailAddress* i inne. Atrybut `DataType` może również umożliwić aplikacji automatyczne udostępnianie funkcji specyficznych dla typu. Na przykład łącze `mailto:` można utworzyć dla elementu [DataType. EmailAddress](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx), a selektor daty można dostarczyć dla elementu [DataType. Date](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) w przeglądarkach, które obsługują [HTML5](http://html5.org/). Atrybuty [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) emitują [dane](http://ejohn.org/blog/html-5-data-attributes/) HTML 5 (wymawiane *kreski danych*), które mogą zrozumieć przeglądarki HTML 5. Atrybuty [typu danych](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) nie zapewniają żadnej weryfikacji.

`DataType.Date` nie określa formatu wyświetlanej daty. Domyślnie pole dane jest wyświetlane zgodnie z domyślnymi formatami opartymi na [CultureInfo](https://msdn.microsoft.com/library/vstudio/system.globalization.cultureinfo(v=vs.110).aspx)serwera.

Atrybut `DisplayFormat` jest używany do jawnego określania formatu daty:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample2.cs)]

Ustawienie `ApplyFormatInEditMode` określa, że należy również zastosować określone formatowanie, gdy wartość jest wyświetlana w polu tekstowym do edycji. (Możesz nie chcieć, aby dla niektórych pól — na przykład w przypadku wartości walutowych, można nie chcieć, aby symbol waluty w polu tekstowym do edycji.)

Możesz użyć atrybutu [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) przez samego siebie, ale zazwyczaj dobrym pomysłem jest użycie atrybutu [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) również. Atrybut `DataType` przekazuje *semantykę* danych w przeciwieństwie do sposobu renderowania na ekranie i zapewnia następujące korzyści, których nie można uzyskać za pomocą `DisplayFormat`:

- Przeglądarka może włączać funkcje HTML5 (na przykład w celu wyświetlenia kontrolki Calendar, symbolu waluty właściwej dla ustawień regionalnych, linków poczty e-mail, niektórych weryfikacji danych wejściowych po stronie klienta itp.).
- Domyślnie przeglądarka będzie renderować dane przy użyciu poprawnego formatu na podstawie [ustawień regionalnych](https://msdn.microsoft.com/library/vstudio/wyzd2bce.aspx).
- Atrybut [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) umożliwia wybranie odpowiedniego szablonu pola w celu renderowania danych przez MVC. ( [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) używa szablonu ciągu). Aby uzyskać więcej informacji, zobacz Wilson [ASP.NET MVC 2](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html). (W przypadku pisania dla MVC 2, ten artykuł nadal dotyczy bieżącej wersji ASP.NET MVC).

Jeśli używasz atrybutu `DataType` z polem Date, musisz określić atrybut `DisplayFormat` również, aby upewnić się, że pole jest renderowane prawidłowo w przeglądarkach Chrome. Aby uzyskać więcej informacji, zobacz [ten wątek StackOverflow](http://stackoverflow.com/questions/12633471/mvc4-datatype-date-editorfor-wont-display-date-value-in-chrome-fine-in-ie).

Aby uzyskać więcej informacji o sposobie obsługi innych formatów daty w MVC, przejdź do [wprowadzenia MVC 5: badanie metod edycji i widoku edycji](../introduction/examining-the-edit-methods-and-edit-view.md) oraz wyszukiwanie na stronie &quot;&quot;ach wielojęzycznych.

Uruchom ponownie stronę indeksu ucznia i Zauważ, że czasy nie są już wyświetlane dla dat rejestracji. Ta sama wartość będzie prawdziwa dla każdego widoku, który używa modelu `Student`.

![Students_index_page_with_formatted_date](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image2.png)

### <a name="the-stringlengthattribute"></a>StringLengthAttribute

Można również określić reguły walidacji danych i komunikaty o błędach walidacji przy użyciu atrybutów. [Atrybut StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) ustawia maksymalną długość w bazie danych i zapewnia weryfikację po stronie klienta i po stronie serwera dla ASP.NET MVC. Możesz również określić minimalną długość ciągu w tym atrybucie, ale wartość minimalna nie ma wpływu na schemat bazy danych.

Załóżmy, że chcesz upewnić się, że użytkownicy nie wprowadzają więcej niż 50 znaków. Aby dodać to ograniczenie, Dodaj atrybuty [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) do właściwości `LastName` i `FirstMidName`, jak pokazano w następującym przykładzie:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample3.cs?highlight=10,12)]

Atrybut [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) nie uniemożliwi użytkownikowi wprowadzania białych znaków w nazwie. Można użyć atrybutu [RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) do zastosowania ograniczeń do danych wejściowych. Na przykład poniższy kod wymaga, aby pierwszy znak był wielką literą, a pozostałe znaki są alfabetyczne:

`[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]`

Atrybut [MaxLength](https://msdn.microsoft.com/library/System.ComponentModel.DataAnnotations.MaxLengthAttribute.aspx) zapewnia podobną funkcjonalność atrybutu [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) , ale nie zapewnia weryfikacji po stronie klienta.

Uruchom aplikację i kliknij kartę **uczniowie** . Zostanie wyświetlony następujący błąd:

*Model z kopią zapasową kontekstu "SchoolContext" został zmieniony od czasu utworzenia bazy danych. Aby zaktualizować bazę danych ([https://go.microsoft.com/fwlink/?LinkId=238269](https://go.microsoft.com/fwlink/?LinkId=238269)), należy rozważyć użycie migracje Code First.*

Model bazy danych zmienił się w taki sposób, że wymaga zmiany w schemacie bazy danych, a Entity Framework wykrył, że. Aby zaktualizować schemat bez utraty danych, które zostały dodane do bazy danych za pomocą interfejsu użytkownika, będziesz używać migracji. Jeśli zmieniono dane, które zostały utworzone przez metodę `Seed`, które zostaną zmienione z powrotem do stanu pierwotnego z powodu metody [AddOrUpdate](https://msdn.microsoft.com/library/hh846520(v=vs.103).aspx) używanej w metodzie `Seed`. ([AddOrUpdate](https://msdn.microsoft.com/library/hh846520(v=vs.103).aspx) jest odpowiednikiem operacji "upsert" w terminologii bazy danych).

W konsoli Menedżera pakietów (PMC) wprowadź następujące polecenia:

[!code-console[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample4.cmd)]

`add-migration` polecenie tworzy plik o nazwie *&lt;sygnatura czasowa&gt;\_MaxLengthOnNames.cs*. Ten plik zawiera kod w metodzie `Up`, która zaktualizuje bazę danych w taki sposób, aby była zgodna z bieżącym modelem danych. `update-database` polecenie uruchomiło ten kod.

Sygnatura czasowa dołączona do nazwy pliku migracji jest używana przez Entity Framework do porządkowania migracji. Można utworzyć wiele migracji przed uruchomieniem `update-database` polecenie, a następnie wszystkie migracje zostaną zastosowane w kolejności, w której zostały utworzone.

Uruchom stronę **Tworzenie** i wprowadź nazwę o długości większej niż 50 znaków. Po kliknięciu przycisku **Utwórz**Walidacja po stronie klienta wyświetla komunikat o błędzie: *pole LastName musi być ciągiem o maksymalnej długości 50.*

### <a name="the-column-attribute"></a>Atrybut Column

Można również użyć atrybutów do kontrolowania sposobu, w jaki klasy i właściwości są mapowane do bazy danych. Załóżmy, że użyto nazwy `FirstMidName` dla pola pierwszej nazwy, ponieważ pole może zawierać również nazwę środkową. Ale chcesz, aby kolumna bazy danych była nazywana `FirstName`, ponieważ użytkownicy, którzy będą zapisywać zapytania ad hoc względem bazy danych, są przyzwyczajoni do tej nazwy. Aby utworzyć to mapowanie, można użyć atrybutu `Column`.

Atrybut `Column` określa, że podczas tworzenia bazy danych kolumna tabeli `Student`, która jest mapowana na Właściwość `FirstMidName` będzie nazywana `FirstName`. Innymi słowy, gdy kod odnosi się do `Student.FirstMidName`, dane będą pochodzić z lub zostaną zaktualizowane w kolumnie `FirstName` tabeli `Student`. Jeśli nie określisz nazw kolumn, mają one taką samą nazwę jak nazwa właściwości.

W pliku *student.cs* dodaj instrukcję `using` dla elementu [System. ComponentModel. DataAnnotations. Schema](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.aspx) i Dodaj atrybut nazwa kolumny do właściwości `FirstMidName`, jak pokazano w poniższym wyróżnionym kodzie:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample5.cs?highlight=4,14)]

Dodanie [atrybutu Column](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute.aspx) zmienia model z SchoolContext, więc nie jest on zgodny z bazą danych. Wprowadź następujące polecenia w obszarze PMC, aby utworzyć kolejną migrację:

[!code-console[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample6.cmd)]

W **Eksplorator serwera**Otwórz projektanta tabeli *uczniów* , klikając dwukrotnie tabelę *uczniów* .

Na poniższej ilustracji przedstawiono oryginalną nazwę kolumny, tak jak przed zastosowaniem pierwszych dwóch migracji. Oprócz nazwy kolumny zmieniającej się z `FirstMidName` na `FirstName`, dwie kolumny nazw zmieniły się od `MAX` długość do 50 znaków.

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image5.png)

Możesz również wprowadzić zmiany mapowania bazy danych przy użyciu [interfejsu API Fluent](https://msdn.microsoft.com/data/jj591617), jak widać w dalszej części tego samouczka.

> [!NOTE]
> Jeśli spróbujesz skompilować przed ukończeniem tworzenia wszystkich klas jednostek w poniższych sekcjach, mogą wystąpić błędy kompilatora.

## <a name="update-student-entity"></a>Aktualizuj jednostkę ucznia

W *Models\Student.cs*Zastąp kod, który został dodany wcześniej do poniższego kodu. Zmiany są wyróżnione.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample7.cs?highlight=11,13,15,18,22,25-32)]

### <a name="the-required-attribute"></a>Wymagany atrybut

[Wymagany atrybut](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) powoduje, że pola nazwy są wymagane. `Required attribute` nie jest wymagana w przypadku typów wartości takich jak DateTime, int, Double i float. Do typów wartości nie można przypisać wartości null, dlatego są one niezależnie traktowane jako wymagane pola. 

Atrybut `Required` musi być używany z `MinimumLength`, aby można było wymusić `MinimumLength`.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample8.cs?highlight=2)]

`MinimumLength` i `Required` Zezwalaj na odstępy, aby spełnić kryteria weryfikacji. Użyj atrybutu `RegularExpression`, aby uzyskać pełne kontrolę nad ciągiem.

### <a name="the-display-attribute"></a>Atrybut wyświetlania

Atrybut `Display` określa, że podpis pól tekstowych powinien mieć wartość "First Name", "nazwisko", "Full Name" i "Data rejestracji" zamiast nazwy właściwości w każdym wystąpieniu (które nie zawiera spacji dzielących wyrazy).

### <a name="the-fullname-calculated-property"></a>Właściwość obliczeniowa FullName

`FullName` jest właściwością obliczaną, która zwraca wartość utworzoną przez połączenie dwóch innych właściwości. W związku z tym ma tylko `get` akcesor i w bazie danych nie zostanie wygenerowana żadna kolumna `FullName`.

## <a name="create-instructor-entity"></a>Utwórz jednostkę instruktora

Utwórz *Models\Instructor.cs*, zastępując kod szablonu następującym kodem:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample9.cs)]

Zwróć uwagę, że kilka właściwości jest taka sama w jednostkach `Student` i `Instructor`. W samouczku [implementującym dziedziczenie](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md) w dalszej części tej serii powrócisz ten kod, aby wyeliminować nadmiarowość.

Można umieścić wiele atrybutów w jednym wierszu, aby można było również napisać klasę instruktora w następujący sposób:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample10.cs)]

### <a name="the-courses-and-officeassignment-navigation-properties"></a>Właściwości nawigacji kursów i OfficeAssignment

Właściwości `Courses` i `OfficeAssignment` są właściwościami nawigacji. Jak wyjaśniono wcześniej, są one zwykle zdefiniowane jako [wirtualne](https://msdn.microsoft.com/library/9fkccyh4(v=vs.110).aspx) , dzięki czemu mogą korzystać z funkcji Entity Framework o nazwie [ładowanie z opóźnieniem](https://msdn.microsoft.com/magazine/hh205756.aspx). Ponadto, jeśli właściwość nawigacji może zawierać wiele jednostek, jego typ musi implementować interfejs [ICollection&lt;t&gt;](https://msdn.microsoft.com/library/92t2ye13.aspx) interfejsu. Na przykład [IList&lt;t&gt;](https://msdn.microsoft.com/library/5y536ey6.aspx) uprawnia, ale nie [&gt;IEnumerable&lt;t](https://msdn.microsoft.com/library/9eekhta0.aspx) , ponieważ `IEnumerable<T>` nie implementuje interfejsu [Add](https://msdn.microsoft.com/library/63ywd54z.aspx).

Instruktor może wyuczyć dowolną liczbę kursów, więc `Courses` jest definiowana jako kolekcja `Course` jednostek.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

Nasz stan reguł firmy instruktor może mieć tylko co najwyżej jeden pakiet Office, więc `OfficeAssignment` jest zdefiniowany jako pojedyncze jednostki `OfficeAssignment` (które mogą być `null` w przypadku, gdy nie jest przypisany żaden pakiet Office).

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

## <a name="create-officeassignment-entity"></a>Tworzenie jednostki OfficeAssignment

Utwórz *Models\OfficeAssignment.cs* przy użyciu następującego kodu:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample13.cs)]

Skompiluj projekt, co spowoduje zapisanie zmian i sprawdzenie, czy nie wprowadzono żadnych kopii i wkleić błędów, które kompilator może przechwytywać.

### <a name="the-key-attribute"></a>Atrybut klucza

Istnieje relacja jeden do zera między `Instructor` i jednostkami `OfficeAssignment`. Przypisanie pakietu Office istnieje tylko w odniesieniu do instruktora, do którego jest przypisane, i dlatego jego klucz podstawowy jest również jego kluczem obcym dla jednostki `Instructor`. Jednak Entity Framework nie może automatycznie rozpoznać `InstructorID` jako klucza podstawowego tej jednostki, ponieważ jego nazwa nie jest zgodna z konwencją nazewnictwa `ID` lub *classname* `ID`. W związku z tym, atrybut `Key` jest używany do identyfikowania go jako klucz:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample14.cs)]

Można również użyć atrybutu `Key`, jeśli jednostka ma własny klucz podstawowy, ale chcesz nazwać Właściwość inną niż `classnameID` lub `ID`. Domyślnie EF traktuje klucz jako niewygenerowany poza bazą danych, ponieważ kolumna jest dla identyfikującej relacji.

### <a name="the-foreignkey-attribute"></a>Atrybut ForeignKey

Gdy istnieje relacja "jeden do zera" lub "jeden do jednego" między dwiema jednostkami (na przykład między `OfficeAssignment` i `Instructor`), EF nie może obejść, który koniec relacji jest podmiotem zabezpieczeń i który jest zależny od siebie. Relacje jeden do jednego mają właściwość nawigacji odwołania w każdej klasie do innej klasy. [Atrybut ForeignKey](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute.aspx) można zastosować do klasy zależnej, aby ustanowić relację. Jeśli pominięto [atrybut ForeignKey](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute.aspx), podczas próby utworzenia migracji zostanie wyświetlony następujący błąd:

*Nie można określić głównego końca skojarzenia między typami "ContosoUniversity. models. OfficeAssignment" i "ContosoUniversity. models. instruktor". Główny koniec tego skojarzenia musi być jawnie skonfigurowany przy użyciu interfejsu API Fluent relacji lub adnotacji danych.*

W dalszej części tego samouczka zobaczysz, jak skonfigurować tę relację przy użyciu interfejsu API Fluent.

### <a name="the-instructor-navigation-property"></a>Właściwość nawigacji instruktora

Jednostka `Instructor` ma właściwość nawigacji `OfficeAssignment` wartość null (ponieważ instruktor może nie mieć przypisania pakietu Office), a jednostka `OfficeAssignment` ma właściwość nawigacji `Instructor` niedopuszczające wartości null (ponieważ przypisanie pakietu Office nie może istnieć bez użycia instruktora--`InstructorID` nie dopuszcza wartości null). Gdy jednostka `Instructor` ma powiązaną jednostkę `OfficeAssignment`, każda jednostka będzie mieć odwołanie do drugiej z nich we właściwości nawigacji.

Można umieścić `[Required]` atrybutu we właściwości nawigacji instruktora, aby określić, że musi istnieć powiązany instruktor, ale nie trzeba tego robić, ponieważ klucz obcy InstructorID (który jest również kluczem do tej tabeli) nie dopuszcza wartości null.

## <a name="modify-the-course-entity"></a>Modyfikowanie jednostki kursu

W *Models\Course.cs*Zastąp kod, który został dodany wcześniej do następującego kodu:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample15.cs)]

Jednostka kursu ma właściwość klucza obcego, `DepartmentID` która wskazuje powiązaną jednostkę `Department` i ma `Department` właściwość nawigacji. Entity Framework nie wymaga dodawania właściwości klucza obcego do modelu danych, jeśli istnieje właściwość nawigacji dla powiązanej jednostki. EF automatycznie tworzy klucze obce w bazie danych wszędzie tam, gdzie są one zbędne. Jednak klucz obcy w modelu danych może sprawiać, że aktualizacje są prostsze i bardziej wydajne. Na przykład podczas pobierania jednostki kursu do edycji, jednostka `Department` ma wartość null, jeśli nie zostanie załadowana, więc w przypadku zaktualizowania jednostki kursu należy najpierw pobrać jednostkę `Department`. Gdy właściwość klucza obcego `DepartmentID` jest uwzględniona w modelu danych, nie musisz pobierać `Department` jednostki przed aktualizacją.

### <a name="the-databasegenerated-attribute"></a>Atrybut DatabaseGenerated

[Atrybut DatabaseGenerated](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute.aspx) z parametrem [None](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedoption(v=vs.110).aspx) we właściwości `CourseID` określa, że wartości klucza podstawowego są dostarczane przez użytkownika, a nie generowane przez bazę danych.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample16.cs)]

Domyślnie Entity Framework zakłada, że wartości klucza podstawowego są generowane przez bazę danych. To wszystko, czego potrzebujesz w większości scenariuszy. Jednak w przypadku `Course` jednostek używany jest numer kursu określony przez użytkownika, taki jak seria 1000 dla jednego działu, seria 2000 dla innego działu itd.

### <a name="foreign-key-and-navigation-properties"></a>Właściwości klucza obcego i nawigacji

Właściwości klucza obcego i właściwości nawigacji w jednostce `Course` odzwierciedlają następujące relacje:

- Kurs jest przypisywany do jednego działu, dlatego istnieje `DepartmentID` klucz obcy i właściwość nawigacji `Department` z przyczyn wymienionych powyżej.

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample17.cs)]
- W kursie może być zarejestrowana dowolna liczba uczniów, więc właściwość nawigacji `Enrollments` jest kolekcją:

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample18.cs)]
- Kurs może być nauczany przez wiele instruktorów, więc właściwość nawigacji `Instructors` jest kolekcją:

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample19.cs)]

## <a name="create-the-department-entity"></a>Tworzenie jednostki działu

Utwórz *Models\Department.cs* przy użyciu następującego kodu:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample20.cs)]

### <a name="the-column-attribute"></a>Atrybut Column

Wcześniej użyto [atrybutu Column](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute.aspx) do zmiany mapowania nazw kolumn. W kodzie jednostki `Department` atrybut `Column` jest używany do zmiany mapowania typu danych SQL, dzięki czemu kolumna zostanie zdefiniowana przy użyciu SQL Server typu [pieniędzy](https://msdn.microsoft.com/library/ms179882.aspx) w bazie danych:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample21.cs)]

Mapowanie kolumn zazwyczaj nie jest wymagane, ponieważ Entity Framework zwykle wybiera odpowiedni SQL Server typ danych oparty na typie CLR zdefiniowanym dla właściwości. Typ `decimal` CLR mapuje do typu `decimal` SQL Server. Ale w tym przypadku wiesz, że kolumna będzie zawierać kwoty walutowe, a typ danych [walutowych](https://msdn.microsoft.com/library/ms179882.aspx) jest bardziej odpowiedni dla tego. Aby uzyskać więcej informacji o typach danych CLR i sposobach ich dopasowania do SQL Server typów danych, zobacz [SqlClient for Entity Framework](https://msdn.microsoft.com/library/bb896344.aspx).

### <a name="foreign-key-and-navigation-properties"></a>Właściwości klucza obcego i nawigacji

Właściwości klucza obcego i nawigacji odzwierciedlają następujące relacje:

- Dział może lub nie ma uprawnienia administratora, a administrator jest zawsze instruktorem. W związku z tym Właściwość `InstructorID` jest dołączana jako klucz obcy do jednostki `Instructor`, a znak zapytania jest dodawany po wyznaczeniu typu `int`, aby oznaczyć właściwość jako wartość null. Właściwość nawigacji ma nazwę `Administrator`, ale zawiera jednostkę `Instructor`:

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample22.cs)]
- Dział może mieć wiele kursów, dlatego istnieje `Courses` właściwość nawigacji:

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample23.cs)]

  > [!NOTE]
  > Zgodnie z Konwencją Entity Framework włącza kaskadowe usuwanie kluczy obcych niedopuszczających wartości null oraz relacji wiele-do-wielu. Może to spowodować cykliczne reguły usuwania kaskadowego, co spowoduje wyjątek podczas próby dodania migracji. Jeśli na przykład właściwość `Department.InstructorID` nie została zdefiniowana jako wartość null, zostanie wyświetlony następujący komunikat o wyjątku: "relacja referencyjna spowoduje niedozwolone cykliczne odwołanie". Jeśli reguły biznesowe wymagają `InstructorID` właściwość, aby nie dopuszczać wartości null, należy użyć następującej instrukcji interfejsu API Fluent, aby wyłączyć usuwanie kaskadowe dla relacji:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample24.cs)]

## <a name="modify-the-enrollment-entity"></a>Modyfikowanie jednostki rejestracji

 W *Models\Enrollment.cs*Zastąp kod, który został dodany wcześniej do poniższego kodu

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample25.cs?highlight=1,15)]

### <a name="foreign-key-and-navigation-properties"></a>Właściwości klucza obcego i nawigacji

Właściwości klucza obcego i właściwości nawigacji odzwierciedlają następujące relacje:

- Rekord rejestracji jest przeznaczony dla jednego kursu, dlatego istnieje `CourseID` właściwość klucza obcego i właściwość nawigacji `Course`:

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample26.cs)]
- Rekord rejestracji dotyczy pojedynczego ucznia, dlatego istnieje `StudentID` właściwość klucza obcego i właściwość nawigacji `Student`:

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample27.cs)]

### <a name="many-to-many-relationships"></a>Relacje wiele do wielu

Istnieje relacja wiele-do-wielu między jednostkami `Student` i `Course`, a jednostka `Enrollment` działa jako tabela sprzężeń wiele-do-wielu *z ładunkiem* w bazie danych. Oznacza to, że tabela `Enrollment` zawiera dodatkowe dane, a nie klucze obce dla sprzężonych tabel (w tym przypadku klucz podstawowy i Właściwość `Grade`).

Na poniższej ilustracji pokazano, jak wyglądają te relacje w diagramie jednostek. (Ten diagram został wygenerowany przy użyciu [Entity Framework narzędzia do zarządzania](https://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d), a Tworzenie diagramu nie jest częścią tego samouczka. jest on właśnie używany tutaj jako ilustracja).

![Student — Course_many do many_relationship](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image12.png)

Każda linia relacji ma 1 na jednym końcu i gwiazdkę (\*), wskazując relację jeden do wielu.

Jeśli tabela `Enrollment` nie zawiera informacji o klasie, będzie musiała zawierać dwa klucze obce `CourseID` i `StudentID`. W takim przypadku będzie odpowiadać tabeli sprzężenia wiele-do-wielu *bez ładunku* (lub *czystej tabeli sprzężeń*) w bazie danych i nie trzeba będzie tworzyć dla niej klasy modelu. Jednostki `Instructor` i `Course` mają ten rodzaj relacji wiele-do-wielu, a jak widać, nie istnieje Klasa jednostki między nimi:

![Instructor-Course_many-to-many_relationship](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image13.png)

W bazie danych jest wymagana tabela sprzężenia, jak pokazano na poniższym diagramie bazy danych:

![Instructor-Course_many-to-many_relationship_tables](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image14.png)

Entity Framework automatycznie tworzy tabelę `CourseInstructor` i odczytuje i aktualizuje ją pośrednio, odczytując i aktualizując `Instructor.Courses` i `Course.Instructors` właściwości nawigacji.

## <a name="entity-relationship-diagram"></a>Diagram relacji jednostki

Na poniższej ilustracji przedstawiono diagram, który Entity Framework narzędzia do tworzenia dla gotowego modelu szkoły.

![School_data_model_diagram](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image1.png)

Oprócz linii relacji wiele-do-wielu (\* do \*) i linii relacji jeden-do-wielu (od 1 do \*) w tym miejscu możesz zobaczyć linię relacji jeden-do-zero-lub-jednego (od 1 do 0.. 1) między jednostkami `Instructor` i `OfficeAssignment` i wierszem relacji zero-lub-jeden-do-wielu (0.. 1 do \*) między instruktorem i jednostkami działu.

## <a name="add-code-to-database-context"></a>Dodaj kod do kontekstu bazy danych

Następnie dodasz nowe jednostki do klasy `SchoolContext` i dostosowasz niektóre mapowanie przy użyciu wywołań [interfejsu API Fluent](https://msdn.microsoft.com/data/jj591617) . Interfejs API to "Fluent", ponieważ jest często używany przez ciąg serii wywołań metod wraz z pojedynczą instrukcją, jak w poniższym przykładzie:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample28.cs)]

W tym samouczku użyjesz interfejsu API Fluent tylko dla mapowania bazy danych, której nie można wykonać za pomocą atrybutów. Można jednak również użyć interfejsu API Fluent, aby określić większość reguł formatowania, walidacji i mapowania, które można wykonać przy użyciu atrybutów. Niektóre atrybuty, takie jak `MinimumLength`, nie mogą być stosowane z interfejsem API Fluent. Jak wspomniano wcześniej, `MinimumLength` nie zmienia schematu, stosuje tylko regułę walidacji po stronie klienta i serwera

Niektórzy deweloperzy wolą korzystać z interfejsu API Fluent wyłącznie w taki sposób, aby mogli utrzymać czyste klasy jednostek. Jeśli chcesz, możesz mieszać atrybuty i interfejs API Fluent, a istnieje kilka dostosowań, które można wykonać tylko za pomocą interfejsu API Fluent, ale ogólnie zalecane jest, aby wybrać jeden z tych dwóch podejść i użyć go spójnie tak dużo, jak to możliwe.

Aby dodać nowe jednostki do modelu danych i wykonać mapowanie bazy danych, które nie było wykonywane przy użyciu atrybutów, Zastąp kod w *DAL\SchoolContext.cs* następującym kodem:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample29.cs)]

Nowa instrukcja w metodzie [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) konfiguruje tabelę sprzężenia wiele-do-wielu:

- Dla relacji wiele-do-wielu między jednostkami `Instructor` i `Course` kod określa nazwę tabeli i kolumny dla tabeli sprzężenia. Code First może skonfigurować relację wiele-do-wielu dla Ciebie bez tego kodu, ale jeśli nie zostanie ona wywołana, zostaną wyświetlone domyślne nazwy, takie jak `InstructorInstructorID` dla kolumny `InstructorID`.

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample30.cs)]

Poniższy kod przedstawia przykład sposobu użycia interfejsu API Fluent zamiast atrybutów do określenia relacji między `Instructor` i `OfficeAssignment` jednostek:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample31.cs)]

Aby uzyskać informacje o tym, co "interfejs API Fluent" działa w tle, zobacz wpis w blogu [interfejsu API Fluent](https://blogs.msdn.com/b/aspnetue/archive/2011/05/04/entity-framework-code-first-tutorial-supplement-what-is-going-on-in-a-fluent-api-call.aspx) .

## <a name="seed-database-with-test-data"></a>Wypełnianie bazy danych z danymi testowymi

Zastąp kod w pliku *Migrations\Configuration.cs* następującym kodem, aby podać dane dotyczące inicjatora dla nowo utworzonych jednostek.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample32.cs)]

Jak przedstawiono w pierwszym samouczku, większość tego kodu po prostu aktualizuje lub tworzy nowe obiekty Entity oraz ładuje przykładowe dane do właściwości, które są wymagane do testowania. Należy jednak zauważyć, że jednostka `Course`, która ma relację wiele-do-wielu z jednostką `Instructor`, jest obsługiwana:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample33.cs)]

Podczas tworzenia obiektu `Course` należy zainicjować właściwość nawigacji `Instructors` jako pustą kolekcję przy użyciu `Instructors = new List<Instructor>()`kodu. Dzięki temu można dodać jednostki `Instructor` powiązane z tym `Course` przy użyciu metody `Instructors.Add`. Jeśli nie utworzono pustej listy, nie można dodać tych relacji, ponieważ właściwość `Instructors` powinna mieć wartość null i nie mogła mieć metody `Add`. Można również dodać inicjalizację listy do konstruktora.

## <a name="add-a-migration"></a>Dodawanie migracji

W obszarze PMC wprowadź polecenie `add-migration` (nie wykonuj jeszcze `update-database` polecenia):

`add-Migration ComplexDataModel`

Jeśli próbowano uruchomić polecenie `update-database` w tym momencie (nie rób tego jeszcze), zostanie wyświetlony następujący błąd:

*Instrukcja ALTER TABLE spowodowała konflikt z ograniczeniem klucza obcego "FK\_dbo.\_a z kursu. Dział\_DepartmentID ". Konflikt wystąpił w bazie danych "ContosoUniversity", tabela "dbo. Dział ", kolumna" DepartmentID ".*

Czasami w przypadku wykonywania migracji z istniejącymi danymi, należy wstawić dane szczątkowe do bazy danych w celu spełnienia ograniczeń klucza obcego i to, co trzeba zrobić teraz. Wygenerowany kod w metodzie `Up` ComplexDataModel dodaje klucz obcy `DepartmentID` niedopuszczający wartości null do tabeli `Course`. Ponieważ istnieją już wiersze w tabeli `Course` podczas uruchamiania kodu, operacja `AddColumn` nie powiedzie się, ponieważ SQL Server nie wie, jaką wartość umieścić w kolumnie, która nie może mieć wartości null. W związku z tym należy zmienić kod w celu nadania nowej kolumnie wartości domyślnej, a także utworzyć Wydział zastępczy o nazwie "Temp", który będzie pełnił rolę działu domyślnego. W efekcie istniejące `Course` wiersze będą ze sobą powiązane z działem "Temp" po uruchomieniu metody `Up`. Można je powiązać z prawidłowymi departamentami w metodzie `Seed`.

Edytuj &lt;*znacznik czasu&gt;\_pliku ComplexDataModel.cs* , Dodaj komentarz do wiersza kodu, który dodaje kolumnę DepartmentID do tabeli kurs, a następnie dodasz następujący wyróżniony kod (wiersz z komentarzem jest również wyróżniony):

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample34.cs?highlight=14-18)]

Gdy metoda `Seed` zostanie uruchomiona, spowoduje wstawienie wierszy w tabeli `Department` i spowoduje powiązanie istniejących wierszy `Course` z nowymi wierszami `Department`. Jeśli nie dodano żadnych kursów w interfejsie użytkownika, nie potrzebujesz już "Temp" ani wartości domyślnej w kolumnie `Course.DepartmentID`. Aby umożliwić komuś dodanie kursów przy użyciu aplikacji, należy również zaktualizować kod metody `Seed`, aby upewnić się, że wszystkie wiersze `Course` (nie tylko te wstawione przez wcześniejsze uruchomienia metody `Seed`) mają prawidłowe wartości `DepartmentID` przed usunięciem z kolumny wartości domyślnej i usunięciu działu "Temp".

## <a name="update-the-database"></a>Aktualizowanie bazy danych

Po zakończeniu edycji &lt;*sygnatury czasowej&gt;\_pliku ComplexDataModel.cs* wprowadź polecenie `update-database` w obszarze PMC, aby przeprowadzić migrację.

[!code-powershell[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample35.ps1)]

> [!NOTE]
> Podczas migrowania danych i wprowadzania zmian w schemacie można uzyskać inne błędy. Jeśli wystąpią błędy migracji, nie można rozwiązać tego problemu, możesz zmienić nazwę bazy danych w parametrach połączenia lub usunąć bazę danych. Najprostszym podejściem jest zmiana nazwy bazy danych w pliku *Web. config* . Poniższy przykład pokazuje zmianę nazwy na CU\_test:
>
> [!code-xml[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample36.xml?highlight=1)]
>
> W przypadku nowej bazy danych nie ma żadnych danych do migrowania, a `update-database` polecenie jest znacznie bardziej prawdopodobnie wykonane bez błędów. Aby uzyskać instrukcje dotyczące sposobu usuwania bazy danych, zobacz [Jak usunąć bazę danych z programu Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/).
>
> Jeśli to się nie powiedzie, można spróbować ponownie zainicjować bazę danych, wprowadzając następujące polecenie w kryterium PMC:
>
> `update-database -TargetMigration:0`

Otwórz bazę danych w **Eksplorator serwera** tak jak wcześniej, a następnie rozwiń węzeł **tabele** , aby zobaczyć, że wszystkie tabele zostały utworzone. (Jeśli nadal masz otwarty **Eksplorator serwera** od momentu wcześniejszego, kliknij przycisk **Odśwież** .)

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image16.png)

Nie utworzono klasy modelu dla tabeli `CourseInstructor`. Jak wyjaśniono wcześniej, jest to tabela sprzężeń dla relacji wiele-do-wielu między jednostkami `Instructor` i `Course`.

Kliknij prawym przyciskiem myszy tabelę `CourseInstructor` i wybierz polecenie **Pokaż dane tabeli** , aby sprawdzić, czy zawiera ona dane w wyniku `Instructor` jednostek dodanych do właściwości nawigacji `Course.Instructors`.

![Table_data_in_CourseInstructor_table](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image17.png)

## <a name="get-the-code"></a>Uzyskiwanie kodu

[Pobierz ukończony projekt](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Dodatkowe zasoby

Linki do innych zasobów Entity Framework można znaleźć w [zasobach zalecanych przez dostęp do danych ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

## <a name="next-steps"></a>Następne kroki

W tym samouczku zostaną wykonane następujące czynności:

> [!div class="checklist"]
> * Dostosowany model danych
> * Zaktualizowana jednostka ucznia
> * Utworzono jednostkę instruktora
> * Utworzono jednostkę OfficeAssignment
> * Zmodyfikowano jednostkę kursu
> * Utworzono jednostkę działu
> * Zmodyfikowano jednostkę rejestracji
> * Dodano kod do kontekstu bazy danych
> * Wypełnianie bazy danych z danymi testowymi
> * Dodano migrację
> * Zaktualizowano bazę danych

Przejdź do następnego artykułu, aby dowiedzieć się, jak odczytywać i wyświetlać powiązane dane, które Entity Framework ładowane do właściwości nawigacji.

> [!div class="nextstepaction"]
> [Odczytywanie powiązanych danych](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)

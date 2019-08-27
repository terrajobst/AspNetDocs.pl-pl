---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-a-more-complex-data-model-for-an-asp-net-mvc-application
title: 'Samouczek: Tworzenie bardziej złożonego modelu danych dla aplikacji ASP.NET MVC'
author: tdykstra
description: W tym samouczku dowiesz się więcej o jednostkach i relacjach, a następnie dostosuj model danych, określając formatowanie, walidację i reguły mapowania bazy danych.
ms.author: riande
ms.date: 01/22/2019
ms.topic: tutorial
ms.assetid: 46f7f3c9-274f-4649-811d-92222a9b27e2
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-a-more-complex-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 933354b09eb4674e6f523f8a65816410521f026f
ms.sourcegitcommit: aa3c2efd56466fc6bdc387ee01ad6f50a261665b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/26/2019
ms.locfileid: "70020997"
---
# <a name="tutorial-create-a-more-complex-data-model-for-an-aspnet-mvc-app"></a>Samouczek: Tworzenie bardziej złożonego modelu danych dla aplikacji ASP.NET MVC

W poprzednich samouczkach pracowałeś z prostym modelem danych, który składa się z trzech jednostek. W tym samouczku dodasz więcej jednostek i relacji i dostosowasz model danych, określając formatowanie, walidację i reguły mapowania bazy danych. W tym artykule przedstawiono dwa sposoby dostosowywania modelu danych: przez dodanie atrybutów do klas jednostek i przez dodanie kodu do klasy kontekstu bazy danych.

Po zakończeniu klasy jednostek składają się na ukończony model danych przedstawiony na poniższej ilustracji:

![School_class_diagram](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image1.png)

W tym samouczku przedstawiono następujące instrukcje:

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

W tej sekcji dowiesz się, jak dostosować model danych przy użyciu atrybutów, które określają formatowanie, walidację i reguły mapowania bazy danych. Następnie w kilku poniższych sekcjach utworzysz cały `School` model danych, dodając do klas, które zostały już utworzone, i tworząc nowe klasy dla pozostałych typów jednostek w modelu.

### <a name="the-datatype-attribute"></a>Atrybut DataType

W przypadku dat rejestracji uczniów na wszystkich stronach sieci Web jest obecnie wyświetlany czas wraz z datą, chociaż wszystko, co jest ważne dla tego pola, to Data. Używając atrybutów adnotacji danych, można wprowadzić jedną zmianę kodu, która naprawi format wyświetlania w każdym widoku, który wyświetla dane. Aby zapoznać się z przykładem, jak to zrobić, Dodaj atrybut do `EnrollmentDate` właściwości `Student` w klasie.

W *Models\Student.cs*, Dodaj `using` instrukcję dla `DataType` `System.ComponentModel.DataAnnotations` przestrzeninazw`DisplayFormat` i Dodaj atrybuty i do właściwości,jakpokazanownastępującymprzykładzie:`EnrollmentDate`

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,12-13)]

Atrybut [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) służy do określania typu danych, który jest bardziej szczegółowy niż typ wewnętrzny bazy danych. W tym przypadku chcemy tylko śledzić datę, a nie datę i godzinę. [Wyliczenie DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) zawiera wiele typów danych, takich jak *Data, godzina, numer telefonu, waluta, EmailAddress* i inne. Ten `DataType` atrybut może również umożliwić aplikacji automatyczne udostępnianie funkcji specyficznych dla typu. Na `mailto:` przykład można utworzyć link dla elementu [DataType. EmailAddress](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx), a selektor daty można dostarczyć dla elementu [DataType. Date](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) w przeglądarkach, które obsługują [HTML5](http://html5.org/). Atrybuty [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) emitują [dane](http://ejohn.org/blog/html-5-data-attributes/) HTML 5 (wymawiane *kreski danych*), które mogą zrozumieć przeglądarki HTML 5. Atrybuty [typu danych](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) nie zapewniają żadnej weryfikacji.

`DataType.Date`nie określa formatu wyświetlanej daty. Domyślnie pole dane jest wyświetlane zgodnie z domyślnymi formatami opartymi na [CultureInfo](https://msdn.microsoft.com/library/vstudio/system.globalization.cultureinfo(v=vs.110).aspx)serwera.

Ten `DisplayFormat` atrybut służy do jawnego określenia formatu daty:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample2.cs)]

`ApplyFormatInEditMode` Ustawienie określa, że należy również zastosować określone formatowanie, gdy wartość jest wyświetlana w polu tekstowym do edycji. (Możesz nie chcieć, aby dla niektórych pól — na przykład w przypadku wartości walutowych, można nie chcieć, aby symbol waluty w polu tekstowym do edycji.)

Możesz użyć atrybutu [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) przez samego siebie, ale zazwyczaj dobrym pomysłem jest użycie atrybutu [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) również. Ten `DataType` atrybut przekazuje semantykę danych w przeciwieństwie do sposobu renderowania na ekranie i zapewnia następujące korzyści, których nie `DisplayFormat`można uzyskać:

- Przeglądarka może włączać funkcje HTML5 (na przykład w celu wyświetlenia kontrolki Calendar, symbolu waluty właściwej dla ustawień regionalnych, linków poczty e-mail, niektórych weryfikacji danych wejściowych po stronie klienta itp.).
- Domyślnie przeglądarka będzie renderować dane przy użyciu poprawnego formatu na podstawie [ustawień regionalnych](https://msdn.microsoft.com/library/vstudio/wyzd2bce.aspx).
- Atrybut [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) umożliwia wybranie odpowiedniego szablonu pola w celu renderowania danych przez MVC. ( [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) używa szablonu ciągu). Aby uzyskać więcej informacji, zobacz Wilson [ASP.NET MVC 2](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html). (W przypadku pisania dla MVC 2, ten artykuł nadal dotyczy bieżącej wersji ASP.NET MVC).

Jeśli używasz `DataType` atrybutu z polem Date, musisz `DisplayFormat` określić atrybut również w celu upewnienia się, że pole jest renderowane prawidłowo w przeglądarkach Chrome. Aby uzyskać więcej informacji, zobacz [ten wątek StackOverflow](http://stackoverflow.com/questions/12633471/mvc4-datatype-date-editorfor-wont-display-date-value-in-chrome-fine-in-ie).

Aby uzyskać więcej informacji o sposobie obsługi innych formatów daty w MVC, przejdź do [wprowadzenia MVC 5: Badanie metod edycji i widoku](../introduction/examining-the-edit-methods-and-edit-view.md) edycji oraz wyszukiwanie na stronie dla celów &quot;wielojęzycznych&quot;.

Uruchom ponownie stronę indeksu ucznia i Zauważ, że czasy nie są już wyświetlane dla dat rejestracji. Ta sama wartość będzie prawdziwa dla każdego widoku, który korzysta `Student` z modelu.

![Students_index_page_with_formatted_date](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image2.png)

### <a name="the-stringlengthattribute"></a>StringLengthAttribute

Można również określić reguły walidacji danych i komunikaty o błędach walidacji przy użyciu atrybutów. [Atrybut StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) ustawia maksymalną długość w bazie danych i zapewnia weryfikację po stronie klienta i po stronie serwera dla ASP.NET MVC. Możesz również określić minimalną długość ciągu w tym atrybucie, ale wartość minimalna nie ma wpływu na schemat bazy danych.

Załóżmy, że chcesz upewnić się, że użytkownicy nie wprowadzają więcej niż 50 znaków. Aby dodać to ograniczenie, Dodaj atrybuty [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) do `LastName` właściwości i `FirstMidName` , jak pokazano w następującym przykładzie:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample3.cs?highlight=10,12)]

Atrybut [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) nie uniemożliwi użytkownikowi wprowadzania białych znaków w nazwie. Można użyć atrybutu [RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) do zastosowania ograniczeń do danych wejściowych. Na przykład poniższy kod wymaga, aby pierwszy znak był wielką literą, a pozostałe znaki są alfabetyczne:

`[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]`

Atrybut [MaxLength](https://msdn.microsoft.com/library/System.ComponentModel.DataAnnotations.MaxLengthAttribute.aspx) zapewnia podobną funkcjonalność atrybutu [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) , ale nie zapewnia weryfikacji po stronie klienta.

Uruchom aplikację i kliknij kartę **uczniowie** . Zostanie wyświetlony następujący błąd:

*Model z kopią zapasową kontekstu "SchoolContext" został zmieniony od czasu utworzenia bazy danych. Rozważ użycie Migracje Code First do zaktualizowania bazy danych[https://go.microsoft.com/fwlink/?LinkId=238269](https://go.microsoft.com/fwlink/?LinkId=238269)().*

Model bazy danych zmienił się w taki sposób, że wymaga zmiany w schemacie bazy danych, a Entity Framework wykrył, że. Aby zaktualizować schemat bez utraty danych, które zostały dodane do bazy danych za pomocą interfejsu użytkownika, będziesz używać migracji. Jeśli zmieniono dane, które zostały utworzone przez `Seed` metodę, zmiany są z powrotem do stanu pierwotnego z powodu metody [AddOrUpdate](https://msdn.microsoft.com/library/hh846520(v=vs.103).aspx) , która `Seed` jest używana w metodzie. ([AddOrUpdate](https://msdn.microsoft.com/library/hh846520(v=vs.103).aspx) jest odpowiednikiem operacji "upsert" w terminologii bazy danych).

W konsoli Menedżera pakietów (PMC) wprowadź następujące polecenia:

[!code-console[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample4.cmd)]

Polecenie tworzy plik o nazwie  *&lt;&gt;timestamp\_MaxLengthOnNames.cs.* `add-migration` Ten plik zawiera kod w `Up` metodzie, która zaktualizuje bazę danych w taki sposób, aby była zgodna z bieżącym modelem danych. `update-database` Polecenie uruchomiło ten kod.

Sygnatura czasowa dołączona do nazwy pliku migracji jest używana przez Entity Framework do porządkowania migracji. Można utworzyć wiele migracji przed uruchomieniem `update-database` polecenia, a następnie wszystkie migracje zostaną zastosowane w kolejności, w której zostały utworzone.

Uruchom stronę **Tworzenie** i wprowadź nazwę o długości większej niż 50 znaków. Po kliknięciu przycisku **Utwórz**zostanie wyświetlony komunikat o błędzie: *Pole LastName musi być ciągiem o maksymalnej długości 50.*

### <a name="the-column-attribute"></a>Atrybut Column

Można również użyć atrybutów do kontrolowania sposobu, w jaki klasy i właściwości są mapowane do bazy danych. Załóżmy, że użyto nazwy `FirstMidName` dla pola pierwszej nazwy, ponieważ pole może zawierać również nazwę środkową. Ale chcesz, aby kolumna bazy danych była nazywana `FirstName`, ponieważ użytkownicy, którzy będą zapisywać zapytania ad hoc względem bazy danych, są przyzwyczajoni do tej nazwy. Aby to mapowanie było możliwe, można użyć `Column` atrybutu.

Ten `Column` atrybut określa, że podczas tworzenia bazy danych kolumna `Student` `FirstMidName` tabeli, która jest mapowana na właściwość, będzie nosiła nazwę `FirstName`. Innymi słowy, gdy Twój kod odwołuje się `Student.FirstMidName`do, dane będą pochodzić z lub zostaną zaktualizowane `FirstName` w kolumnie `Student` tabeli. Jeśli nie określisz nazw kolumn, mają one taką samą nazwę jak nazwa właściwości.

W pliku *student.cs* Dodaj `using` instrukcję dla elementu [System. ComponentModel. DataAnnotations. Schema](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.aspx) i Dodaj atrybut nazwy kolumny do `FirstMidName` właściwości, jak pokazano w następującym wyróżnionym kodzie:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample5.cs?highlight=4,14)]

Dodanie [atrybutu Column](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute.aspx) zmienia model z SchoolContext, więc nie jest on zgodny z bazą danych. Wprowadź następujące polecenia w obszarze PMC, aby utworzyć kolejną migrację:

[!code-console[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample6.cmd)]

W **Eksplorator serwera**Otwórz projektanta tabeli *uczniów* , klikając dwukrotnie tabelę *uczniów* .

Na poniższej ilustracji przedstawiono oryginalną nazwę kolumny, tak jak przed zastosowaniem pierwszych dwóch migracji. Oprócz nazw kolumn zmienianych z `FirstMidName` na `FirstName`, dwie nazwy kolumn zostały zmienione od `MAX` długości do 50 znaków.

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image5.png)

Możesz również wprowadzić zmiany mapowania bazy danych przy użyciu [interfejsu API Fluent](https://msdn.microsoft.com/data/jj591617), jak widać w dalszej części tego samouczka.

> [!NOTE]
> Jeśli spróbujesz skompilować przed ukończeniem tworzenia wszystkich klas jednostek w poniższych sekcjach, mogą wystąpić błędy kompilatora.

## <a name="update-student-entity"></a>Aktualizuj jednostkę ucznia

W *Models\Student.cs*Zastąp kod, który został dodany wcześniej do poniższego kodu. Zmiany są wyróżnione.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample7.cs?highlight=11,13,15,18,22,25-32)]

### <a name="the-required-attribute"></a>Wymagany atrybut

[Wymagany atrybut](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) powoduje, że pola nazwy są wymagane. Nie `Required attribute` jest wymagana w przypadku typów wartości takich jak DateTime, int, Double i float. Do typów wartości nie można przypisać wartości null, dlatego są one niezależnie traktowane jako wymagane pola. 

Atrybut musi być używany z `MinimumLength` , `MinimumLength` aby można było wymusić. `Required`

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample8.cs?highlight=2)]

`MinimumLength`i `Required` Zezwalaj na odstępy, aby spełnić kryteria weryfikacji. `RegularExpression` Użyj atrybutu w celu pełnego kontrolowania ciągu.

### <a name="the-display-attribute"></a>Atrybut wyświetlania

Ten `Display` atrybut określa, że podpis pól tekstowych powinien mieć wartość "First Name", "nazwisko", "Full Name" i "Data rejestracji" zamiast nazwy właściwości w każdym wystąpieniu (które nie zawiera spacji dzielących wyrazy).

### <a name="the-fullname-calculated-property"></a>Właściwość obliczeniowa FullName

`FullName`jest właściwością obliczaną, która zwraca wartość utworzoną przez połączenie dwóch innych właściwości. W związku z tym ma `get` tylko metodę dostępu i `FullName` żadna kolumna nie zostanie wygenerowana w bazie danych.

## <a name="create-instructor-entity"></a>Utwórz jednostkę instruktora

Utwórz *Models\Instructor.cs*, zastępując kod szablonu następującym kodem:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample9.cs)]

Zauważ, że kilka właściwości jest taka sama w `Student` jednostkach i `Instructor` . W samouczku [implementującym dziedziczenie](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md) w dalszej części tej serii powrócisz ten kod, aby wyeliminować nadmiarowość.

Można umieścić wiele atrybutów w jednym wierszu, aby można było również napisać klasę instruktora w następujący sposób:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample10.cs)]

### <a name="the-courses-and-officeassignment-navigation-properties"></a>Właściwości nawigacji kursów i OfficeAssignment

Właściwości `Courses` i`OfficeAssignment` są właściwościami nawigacji. Jak wyjaśniono wcześniej, są one zwykle zdefiniowane jako [wirtualne](https://msdn.microsoft.com/library/9fkccyh4(v=vs.110).aspx) , dzięki czemu mogą korzystać z funkcji Entity Framework o nazwie [ładowanie](https://msdn.microsoft.com/magazine/hh205756.aspx)z opóźnieniem. Ponadto, jeśli właściwość nawigacji może zawierać wiele jednostek, jego typ musi implementować interfejs [ICollection&lt;T&gt; ](https://msdn.microsoft.com/library/92t2ye13.aspx) . Na przykład [,&lt;IList&gt; t](https://msdn.microsoft.com/library/5y536ey6.aspx) kwalifikuje się, ale nie `IEnumerable<T>` jako [&lt;IEnumerable&gt; t](https://msdn.microsoft.com/library/9eekhta0.aspx) , ponieważ nie implementuje [dodawania](https://msdn.microsoft.com/library/63ywd54z.aspx).

Instruktor może uczyć się dowolnej liczby kursów, tak więc `Courses` jest zdefiniowany jako `Course` kolekcja jednostek.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

Nasz stan reguł firmy instruktor może mieć tylko co najwyżej jeden pakiet Office, więc `OfficeAssignment` jest zdefiniowany jako pojedynczy `OfficeAssignment` obiekt (może to być `null` brak przypisania pakietu Office).

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

## <a name="create-officeassignment-entity"></a>Tworzenie jednostki OfficeAssignment

Utwórz *Models\OfficeAssignment.cs* przy użyciu następującego kodu:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample13.cs)]

Skompiluj projekt, co spowoduje zapisanie zmian i sprawdzenie, czy nie wprowadzono żadnych kopii i wkleić błędów, które kompilator może przechwytywać.

### <a name="the-key-attribute"></a>Atrybut klucza

Istnieje relacja jeden do zera między `Instructor` `OfficeAssignment` i a jednostkami. Przypisanie pakietu Office istnieje tylko w odniesieniu do instruktora, do którego jest przypisane, i dlatego jego klucz podstawowy jest również jego kluczem obcym `Instructor` dla jednostki. Ale Entity Framework nie może automatycznie rozpoznać `InstructorID` klucza podstawowego tej jednostki, ponieważ jego nazwa nie jest zgodna z `ID` konwencją nazewnictwa lub *ClassName* `ID` . W związku z tym atrybutjestużywanydoidentyfikowaniagojakoklucz:`Key`

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample14.cs)]

Można również użyć atrybutu, `Key` Jeśli jednostka ma własny klucz podstawowy, ale chcesz nazwać Właściwość inną niż `classnameID` lub `ID`. Domyślnie EF traktuje klucz jako niewygenerowany poza bazą danych, ponieważ kolumna jest dla identyfikującej relacji.

### <a name="the-foreignkey-attribute"></a>Atrybut ForeignKey

Gdy istnieje relacja "jeden do zera" lub "jeden do jednego" między dwiema jednostkami (na przykład między `OfficeAssignment` i `Instructor`), EF nie może obejść tego, który koniec relacji jest podmiotem zabezpieczeń i który jest zależny od siebie. Relacje jeden do jednego mają właściwość nawigacji odwołania w każdej klasie do innej klasy. [Atrybut ForeignKey](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute.aspx) można zastosować do klasy zależnej, aby ustanowić relację. Jeśli pominięto [atrybut ForeignKey](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute.aspx), podczas próby utworzenia migracji zostanie wyświetlony następujący błąd:

*Nie można określić głównego końca skojarzenia między typami "ContosoUniversity. models. OfficeAssignment" i "ContosoUniversity. models. instruktor". Główny koniec tego skojarzenia musi być jawnie skonfigurowany przy użyciu interfejsu API Fluent relacji lub adnotacji danych.*

W dalszej części tego samouczka zobaczysz, jak skonfigurować tę relację przy użyciu interfejsu API Fluent.

### <a name="the-instructor-navigation-property"></a>Właściwość nawigacji instruktora

Jednostka ma właściwość nawigacji dopuszczającą wartość null `OfficeAssignment` (ponieważ instruktor może nie mieć `OfficeAssignment` przypisania pakietu Office), a jednostka ma właściwość nawigacji niedopuszczające `Instructor` wartości null (ponieważ przypisanie pakietu Office nie może `Instructor` istnieje bez instruktora — `InstructorID` nie dopuszcza wartości null. Gdy jednostka ma powiązaną `OfficeAssignment` jednostkę, każda jednostka będzie mieć odwołanie do drugiej z nich we właściwości nawigacji. `Instructor`

Można umieścić `[Required]` atrybut we właściwości nawigacji instruktora, aby określić, że musi istnieć powiązany instruktor, ale nie trzeba tego robić, ponieważ klucz obcy InstructorID (który jest również kluczem do tej tabeli) nie dopuszcza wartości null.

## <a name="modify-the-course-entity"></a>Modyfikowanie jednostki kursu

W *Models\Course.cs*Zastąp kod, który został dodany wcześniej do następującego kodu:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample15.cs)]

Jednostka kursu ma właściwość `DepartmentID` klucza obcego, która wskazuje `Department` jednostkę powiązaną `Department` i ma właściwość nawigacji. Entity Framework nie wymaga dodawania właściwości klucza obcego do modelu danych, jeśli istnieje właściwość nawigacji dla powiązanej jednostki. EF automatycznie tworzy klucze obce w bazie danych wszędzie tam, gdzie są one zbędne. Jednak klucz obcy w modelu danych może sprawiać, że aktualizacje są prostsze i bardziej wydajne. Na przykład podczas pobierania jednostki kursu do edycji, `Department` jednostka ma wartość null, jeśli nie zostanie załadowana, więc po zaktualizowaniu jednostki kursu należy najpierw `Department` pobrać jednostkę. Gdy właściwość `DepartmentID` klucza obcego jest uwzględniona w modelu danych, nie musisz `Department` pobierać jednostki przed aktualizacją.

### <a name="the-databasegenerated-attribute"></a>Atrybut DatabaseGenerated

[Atrybut DatabaseGenerated](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute.aspx) z `CourseID` parametrem [none](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedoption(v=vs.110).aspx) we właściwości określa, że wartości klucza podstawowego są dostarczane przez użytkownika, a nie generowane przez bazę danych.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample16.cs)]

Domyślnie Entity Framework zakłada, że wartości klucza podstawowego są generowane przez bazę danych. To wszystko, czego potrzebujesz w większości scenariuszy. Jednak w przypadku `Course` jednostek używany jest numer kursu określony przez użytkownika, taki jak seria 1000 dla jednego działu, seria 2000 dla innego działu itd.

### <a name="foreign-key-and-navigation-properties"></a>Właściwości klucza obcego i nawigacji

Właściwości klucza obcego i właściwości nawigacji w `Course` jednostce odzwierciedlają następujące relacje:

- Kurs jest przypisywany do jednego działu, dlatego istnieje `DepartmentID` klucz obcy `Department` i właściwość nawigacji dla powodów wymienionych powyżej.

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample17.cs)]
- Kurs może zawierać dowolną liczbę studentów, więc `Enrollments` właściwość nawigacji jest kolekcją:

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample18.cs)]
- Kurs może być nauczany przez wiele instruktorów, więc `Instructors` właściwość nawigacji jest kolekcją:

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample19.cs)]

## <a name="create-the-department-entity"></a>Tworzenie jednostki działu

Utwórz *Models\Department.cs* przy użyciu następującego kodu:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample20.cs)]

### <a name="the-column-attribute"></a>Atrybut Column

Wcześniej użyto [atrybutu Column](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute.aspx) do zmiany mapowania nazw kolumn. W kodzie dla `Department` jednostki `Column` atrybut jest używany do zmiany mapowania typu danych SQL, tak aby kolumna została zdefiniowana przy użyciu SQL Server typu [pieniędzy](https://msdn.microsoft.com/library/ms179882.aspx) w bazie danych:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample21.cs)]

Mapowanie kolumn zazwyczaj nie jest wymagane, ponieważ Entity Framework zwykle wybiera odpowiedni SQL Server typ danych oparty na typie CLR zdefiniowanym dla właściwości. Typ CLR `decimal` jest mapowany na typ SQL Server `decimal` . Ale w tym przypadku wiesz, że kolumna będzie zawierać kwoty walutowe, a typ danych [walutowych](https://msdn.microsoft.com/library/ms179882.aspx) jest bardziej odpowiedni dla tego. Aby uzyskać więcej informacji o typach danych CLR i sposobach ich dopasowania do SQL Server typów danych, zobacz [SqlClient for Entity Framework](https://msdn.microsoft.com/library/bb896344.aspx).

### <a name="foreign-key-and-navigation-properties"></a>Właściwości klucza obcego i nawigacji

Właściwości klucza obcego i nawigacji odzwierciedlają następujące relacje:

- Dział może lub nie ma uprawnienia administratora, a administrator jest zawsze instruktorem. W `InstructorID` związku z tym właściwość jest dołączana jako klucz `Instructor` obcy do jednostki, a `int` znak zapytania jest dodawany po wyznaczeniu typu, aby oznaczyć właściwość jako wartość null. Właściwość nawigacji ma nazwę `Administrator` , ale `Instructor` zawiera jednostkę:

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample22.cs)]
- Dział może mieć wiele kursów, dlatego istnieje `Courses` właściwość nawigacji:

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample23.cs)]

  > [!NOTE]
  > Zgodnie z Konwencją Entity Framework włącza kaskadowe usuwanie kluczy obcych niedopuszczających wartości null oraz relacji wiele-do-wielu. Może to spowodować cykliczne reguły usuwania kaskadowego, co spowoduje wyjątek podczas próby dodania migracji. Na przykład jeśli `Department.InstructorID` właściwość nie została zdefiniowana jako wartość null, zostanie wyświetlony następujący komunikat o wyjątku: "Relacja referencyjna spowoduje, że odwołanie cykliczne jest niedozwolone". Jeśli właściwość reguły biznesowe wymaga `InstructorID` , aby nie dopuszczać wartości null, należy użyć następującej instrukcji interfejsu API Fluent, aby wyłączyć usuwanie kaskadowe dla relacji:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample24.cs)]

## <a name="modify-the-enrollment-entity"></a>Modyfikowanie jednostki rejestracji

 W *Models\Enrollment.cs*Zastąp kod, który został dodany wcześniej do poniższego kodu

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample25.cs?highlight=1,15)]

### <a name="foreign-key-and-navigation-properties"></a>Właściwości klucza obcego i nawigacji

Właściwości klucza obcego i właściwości nawigacji odzwierciedlają następujące relacje:

- Rekord rejestracji dotyczy pojedynczego kursu, dlatego istnieje `CourseID` właściwość klucza obcego `Course` i właściwość nawigacji:

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample26.cs)]
- Rekord rejestracji dotyczy pojedynczego ucznia, dlatego istnieje `StudentID` właściwość klucza obcego `Student` i właściwość nawigacji:

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample27.cs)]

### <a name="many-to-many-relationships"></a>Relacje wiele do wielu

Istnieje `Student` relacja wiele-do-wielu między obiektami i `Course` i `Enrollment` jednostka działa jako tabela sprzężeń wiele-do-wielu *z ładunkiem* w bazie danych. Oznacza to, że `Enrollment` tabela zawiera dodatkowe dane, a nie klucze obce dla sprzężonych tabel (w tym przypadku klucz podstawowy `Grade` i właściwość).

Na poniższej ilustracji pokazano, jak wyglądają te relacje w diagramie jednostek. (Ten diagram został wygenerowany przy użyciu [Entity Framework narzędzia do zarządzania](https://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d), a Tworzenie diagramu nie jest częścią tego samouczka. jest on właśnie używany tutaj jako ilustracja).

![Student-Course_many-to-many_relationship](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image12.png)

Każda linia relacji ma 1 na jednym końcu i gwiazdkę (\*), co wskazuje na relację jeden do wielu.

Jeśli tabela nie zawiera informacji o klasie, musi zawierać tylko dwa klucze `CourseID` obce i `StudentID`. `Enrollment` W takim przypadku będzie odpowiadać tabeli sprzężenia wiele-do-wielu *bez ładunku* (lub *czystej tabeli sprzężeń*) w bazie danych i nie trzeba będzie tworzyć dla niej klasy modelu. Jednostki `Instructor` i`Course` mają ten rodzaj relacji wiele-do-wielu i jak widać, nie istnieje Klasa jednostki między nimi:

![Instructor-Course_many-to-many_relationship](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image13.png)

W bazie danych jest wymagana tabela sprzężenia, jak pokazano na poniższym diagramie bazy danych:

![Instructor-Course_many-to-many_relationship_tables](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image14.png)

Entity Framework automatycznie tworzy `CourseInstructor` tabelę i odczytuje i aktualizuje ją pośrednio przez odczytywanie i `Instructor.Courses` aktualizowanie właściwości nawigacji i `Course.Instructors` .

## <a name="entity-relationship-diagram"></a>Diagram relacji jednostki

Na poniższej ilustracji przedstawiono diagram, który Entity Framework narzędzia do tworzenia dla gotowego modelu szkoły.

![School_data_model_diagram](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image1.png)

Poza wierszami relacji wiele-do-wielu (\* do \*) i liniami jeden-do-wielu (od 1 do \*) można zobaczyć w tym miejscu linię relacji jeden-do-zero-lub-jednego (od 1 `Instructor` do 0.. 1) między i `OfficeAssignment` jednostki i linia relacji zero-lub-jeden-do-wielu (od 0.. 1 do \*) między instruktorem a jednostkami działu.

## <a name="add-code-to-database-context"></a>Dodaj kod do kontekstu bazy danych

Następnie dodasz nowe jednostki do `SchoolContext` klasy i dostosowasz niektóre mapowanie przy użyciu wywołań [interfejsu API Fluent](https://msdn.microsoft.com/data/jj591617) . Interfejs API to "Fluent", ponieważ jest często używany przez ciąg serii wywołań metod wraz z pojedynczą instrukcją, jak w poniższym przykładzie:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample28.cs)]

W tym samouczku użyjesz interfejsu API Fluent tylko dla mapowania bazy danych, której nie można wykonać za pomocą atrybutów. Można jednak również użyć interfejsu API Fluent, aby określić większość reguł formatowania, walidacji i mapowania, które można wykonać przy użyciu atrybutów. Niektórych atrybutów, takich `MinimumLength` jak nie można zastosować w interfejsie API Fluent. Jak wspomniano wcześniej `MinimumLength` , nie zmienia schematu, stosuje tylko regułę walidacji po stronie klienta i serwera

Niektórzy deweloperzy wolą korzystać z interfejsu API Fluent wyłącznie w taki sposób, aby mogli utrzymać czyste klasy jednostek. Jeśli chcesz, możesz mieszać atrybuty i interfejs API Fluent, a istnieje kilka dostosowań, które można wykonać tylko za pomocą interfejsu API Fluent, ale ogólnie zalecane jest, aby wybrać jeden z tych dwóch podejść i użyć go spójnie tak dużo, jak to możliwe.

Aby dodać nowe jednostki do modelu danych i wykonać mapowanie bazy danych, które nie było wykonywane przy użyciu atrybutów, Zastąp kod w *DAL\SchoolContext.cs* następującym kodem:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample29.cs)]

Nowa instrukcja w metodzie [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) konfiguruje tabelę sprzężenia wiele-do-wielu:

- Dla relacji wiele-do-wielu między `Instructor` obiektami i `Course` , kod określa nazwy tabeli i kolumn dla tabeli sprzężenia. Code First może skonfigurować relację wiele-do-wielu dla Ciebie bez tego kodu, ale jeśli nie zostanie ona wywołana, zostaną wyświetlone domyślne nazwy, takie jak `InstructorInstructorID` `InstructorID` dla kolumny.

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample30.cs)]

Poniższy kod przedstawia przykład sposobu użycia interfejsu API Fluent zamiast atrybutów do określenia relacji między `Instructor` obiektami i: `OfficeAssignment`

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample31.cs)]

Aby uzyskać informacje o tym, co "interfejs API Fluent" działa w tle, zobacz wpis w blogu [interfejsu API Fluent](https://blogs.msdn.com/b/aspnetue/archive/2011/05/04/entity-framework-code-first-tutorial-supplement-what-is-going-on-in-a-fluent-api-call.aspx) .

## <a name="seed-database-with-test-data"></a>Wypełnianie bazy danych z danymi testowymi

Zastąp kod w pliku *Migrations\Configuration.cs* następującym kodem, aby podać dane dotyczące inicjatora dla nowo utworzonych jednostek.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample32.cs)]

Jak przedstawiono w pierwszym samouczku, większość tego kodu po prostu aktualizuje lub tworzy nowe obiekty Entity oraz ładuje przykładowe dane do właściwości, które są wymagane do testowania. Należy jednak zauważyć, że `Course` jednostka, która ma relację wiele-do-wielu `Instructor` z jednostką, jest obsługiwana:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample33.cs)]

Podczas tworzenia `Course` obiektu, należy `Instructors` zainicjować właściwość nawigacji jako pustą kolekcję przy użyciu kodu `Instructors = new List<Instructor>()`. Dzięki temu można dodać `Instructor` jednostki, które są związane z tym `Course` przy użyciu `Instructors.Add` metody. Jeśli nie utworzono pustej listy, nie będzie można dodać tych relacji, ponieważ `Instructors` właściwość powinna mieć wartość null i nie może `Add` mieć metody. Można również dodać inicjalizację listy do konstruktora.

## <a name="add-a-migration"></a>Dodawanie migracji

Z kryterium PMC wprowadź `add-migration` polecenie (nie `update-database` wykonuj jeszcze polecenia):

`add-Migration ComplexDataModel`

Jeśli podjęto próbę uruchomienia `update-database` polecenia w tym punkcie (nie rób tego jeszcze), zostanie wyświetlony następujący błąd:

*Instrukcja ALTER TABLE spowodowała konflikt z ograniczeniem klucza obcego "FK\_dbo. Obiekt\_dbo kursu. Departament\_DepartmentID ". Konflikt wystąpił w bazie danych "ContosoUniversity", tabela "dbo. Dział ", kolumna" DepartmentID ".*

Czasami w przypadku wykonywania migracji z istniejącymi danymi, należy wstawić dane szczątkowe do bazy danych w celu spełnienia ograniczeń klucza obcego i to, co trzeba zrobić teraz. Wygenerowany kod w metodzie ComplexDataModel `Up` dodaje do `Course` tabeli klucz obcy, `DepartmentID` który nie dopuszcza wartości null. Ponieważ w `Course` tabeli istnieją już wiersze `AddColumn` , które są uruchamiane, operacja zakończy się niepowodzeniem, ponieważ SQL Server nie wie, jaką wartość należy umieścić w kolumnie, która nie może mieć wartości null. W związku z tym należy zmienić kod w celu nadania nowej kolumnie wartości domyślnej, a także utworzyć Wydział zastępczy o nazwie "Temp", który będzie pełnił rolę działu domyślnego. W związku z tym istniejące `Course` wiersze będą ze sobą powiązane z działem "Temp" `Up` po uruchomieniu metody. Można je powiązać z prawidłowymi departamentami w `Seed` metodzie.

Edytuj plik *ComplexDataModel.cs&gt;sygnatury\_czasowej* , Dodaj komentarz do wiersza kodu, który dodaje kolumnę DepartmentID do tabeli kursów, a następnie doda następujący wyróżniony kod (wiersz z komentarzem jest również &lt; wyróżnione):

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample34.cs?highlight=14-18)]

Gdy metoda zostanie uruchomiona, spowoduje wstawienie wierszy `Department` w tabeli i spowoduje powiązanie istniejących `Course` wierszy z tymi nowymi `Department` wierszami. `Seed` Jeśli nie dodano żadnych kursów w interfejsie użytkownika, nie potrzebujesz już "Temp" ani wartości domyślnej w `Course.DepartmentID` kolumnie. Aby umożliwić komuś dodanie kursów przy użyciu aplikacji, należy również zaktualizować `Seed` kod metody, aby upewnić się, że wszystkie `Course` wiersze (nie tylko te wstawione `Seed` przez wcześniejsze uruchomienia metody) prawidłowe `DepartmentID` wartości przed usunięciem wartości domyślnej z kolumny i usunięciem działu "Temp".

## <a name="update-the-database"></a>Aktualizowanie bazy danych

Po zakończeniu edycji &lt;pliku *ComplexDataModel.cs sygnatury&gt;\_czasowej* wprowadź `update-database` polecenie w obszarze PMC, aby wykonać migrację.

[!code-powershell[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample35.ps1)]

> [!NOTE]
> Podczas migrowania danych i wprowadzania zmian w schemacie można uzyskać inne błędy. Jeśli wystąpią błędy migracji, nie można rozwiązać tego problemu, możesz zmienić nazwę bazy danych w parametrach połączenia lub usunąć bazę danych. Najprostszym podejściem jest zmiana nazwy bazy danych w pliku *Web. config* . Poniższy przykład pokazuje zmianę nazwy na test cu\_:
>
> [!code-xml[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample36.xml?highlight=1)]
>
> W przypadku nowej bazy danych nie ma żadnych danych do migracji, a `update-database` wykonywanie polecenia jest znacznie większe. Aby uzyskać instrukcje dotyczące sposobu usuwania bazy danych, zobacz [Jak usunąć bazę danych z programu Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/).
>
> Jeśli to się nie powiedzie, można spróbować ponownie zainicjować bazę danych, wprowadzając następujące polecenie w kryterium PMC:
>
> `update-database -TargetMigration:0`

Otwórz bazę danych w **Eksplorator serwera** tak jak wcześniej, a następnie rozwiń węzeł **tabele** , aby zobaczyć, że wszystkie tabele zostały utworzone. (Jeśli nadal masz otwarty **Eksplorator serwera** od momentu wcześniejszego, kliknij przycisk **Odśwież** .)

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image16.png)

Nie utworzono klasy modelu dla `CourseInstructor` tabeli. Jak wyjaśniono wcześniej, jest to tabela sprzężenia dla relacji wiele-do-wielu między `Instructor` obiektami i. `Course`

Kliknij prawym przyciskiem `CourseInstructor` myszy tabelę i wybierz polecenie **Pokaż dane tabeli** , aby sprawdzić, czy zawiera on dane w wyniku `Instructor` jednostek dodanych do `Course.Instructors` właściwości nawigacji.

![Table_data_in_CourseInstructor_table](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image17.png)

## <a name="get-the-code"></a>Pobierz kod

[Pobierz ukończony projekt](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Dodatkowe zasoby

Linki do innych zasobów Entity Framework można znaleźć w zasobach [zalecanych przez dostęp do danych ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

## <a name="next-steps"></a>Następne kroki

W tym samouczku przedstawiono następujące instrukcje:

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

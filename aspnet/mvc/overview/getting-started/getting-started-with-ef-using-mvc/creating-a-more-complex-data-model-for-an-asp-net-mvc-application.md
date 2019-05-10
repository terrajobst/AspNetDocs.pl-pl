---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-a-more-complex-data-model-for-an-asp-net-mvc-application
title: 'Samouczek: Tworzenie bardziej złożonego modelu danych dla aplikacji ASP.NET MVC'
author: tdykstra
description: W tym samouczku dodasz więcej jednostek i relacji i będzie Dostosuj model danych, określając formatowania i sprawdzania poprawności i reguł mapowania bazy danych.
ms.author: riande
ms.date: 01/22/2019
ms.topic: tutorial
ms.assetid: 46f7f3c9-274f-4649-811d-92222a9b27e2
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-a-more-complex-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 5c27f6fe07856db2b2961abc8fa797343d361d97
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65120937"
---
# <a name="tutorial-create-a-more-complex-data-model-for-an-aspnet-mvc-app"></a>Samouczek: Tworzenie bardziej złożonego modelu danych dla aplikacji ASP.NET MVC

W poprzednich samouczkach doświadczenie w pracy z modelu prostego danych, który został składające się z trzech jednostek. W tym samouczku dodasz więcej jednostek i relacji, a następnie Dostosuj model danych, określając formatowania i sprawdzania poprawności i reguł mapowania bazy danych. W tym artykule przedstawiono Dostosuj model danych na dwa sposoby: przez dodanie atrybutów do klas jednostek i, dodając kod do klasy kontekstu bazy danych.

Gdy skończysz, klas jednostek tworzących model danych ukończone, który jest wyświetlany na poniższej ilustracji:

![School_class_diagram](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image1.png)

W ramach tego samouczka możesz:

> [!div class="checklist"]
> * Dostosuj model danych
> * Aktualizowanie jednostki dla uczniów
> * Tworzenie jednostki przez instruktorów
> * Tworzenie jednostki OfficeAssignment
> * Modyfikowanie jednostek kursu
> * Tworzenie jednostki działu
> * Modyfikowanie jednostek rejestracji
> * Dodaj kod do kontekstu bazy danych
> * Baza danych inicjatora, przy użyciu danych testowych
> * Dodaj migrację
> * Aktualizowanie bazy danych

## <a name="prerequisites"></a>Wymagania wstępne

* [Pierwszy migracje i wdrażanie kodu](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="customize-the-data-model"></a>Dostosuj model danych

W tej sekcji pokazano, jak dostosować model danych przy użyciu atrybutów, które określają, formatowanie, sprawdzanie poprawności i reguł mapowania bazy danych. A następnie w kilka z następujących sekcji utworzysz pełne `School` modelu danych, dodając atrybuty klasy już utworzone i tworzenia nowych klas pozostałe typy jednostek w modelu.

### <a name="the-datatype-attribute"></a>Atrybut typu danych

Dat rejestracji dla uczniów wszystkie strony sieci web obecnie wyświetlić czas wraz z datą, mimo że wszystkich interesujących Cię dla tego pola jest datą. Za pomocą atrybutów adnotacji danych, można go utworzyć kod zmian, który naprawi format wyświetlania w każdym widoku, który zawiera dane. Aby zobaczyć przykład sposobu wykonania, należy dodać atrybut `EnrollmentDate` właściwość `Student` klasy.

W *Models\Student.cs*, Dodaj `using` poufności informacji dotyczące `System.ComponentModel.DataAnnotations` przestrzeni nazw i Dodaj `DataType` i `DisplayFormat` atrybuty do `EnrollmentDate` właściwości, jak pokazano w poniższym przykładzie:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,12-13)]

[DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) atrybut jest używany do określenia typu danych, który jest bardziej szczegółowe niż typ wewnętrznej bazy danych. W tym przypadku ma być uruchamiany tylko do śledzenia daty, nie daty i godziny. [Wyliczenie typu danych](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) zawiera dla wielu typów danych, takich jak *daty, godziny, numer telefonu, waluty, EmailAddress* itd. `DataType` Atrybut można również włączyć automatyczne udostępnianie funkcji specyficznych dla typu aplikacji. Na przykład `mailto:` łącza mogą być tworzone dla [DataType.EmailAddress](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx), i można podać selektora daty [DataType.Date](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) w przeglądarkach obsługujących [HTML5](http://html5.org/). [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) atrybuty emituje HTML 5 [danych -](http://ejohn.org/blog/html-5-data-attributes/) (Wymowa *dash danych*) atrybutów, które może zrozumieć przeglądarki HTML 5. [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) atrybuty nie są oferowane wszystkich sprawdzania poprawności.

`DataType.Date` Określa format daty, która jest wyświetlana. Domyślnie pole danych są wyświetlane domyślne formaty oparte na tym serwerze [CultureInfo](https://msdn.microsoft.com/library/vstudio/system.globalization.cultureinfo(v=vs.110).aspx).

`DisplayFormat` Atrybut jest używany jawnie określić format daty:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample2.cs)]

`ApplyFormatInEditMode` Ustawienie określa, czy określony sposób formatowania powinien również będą stosowane, gdy wartość jest wyświetlana w polu tekstowym do edycji. (Nie może być, w przypadku niektórych pól — na przykład dla wartości waluty może nie ma symbolu waluty, w polu tekstowym do edycji.)

Możesz użyć [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) atrybutu przez sam, ale zazwyczaj jest dobry pomysł, aby użyć [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) również atrybutu. `DataType` Atrybutu powoduje *semantyki* danych tak jak w przeciwieństwie do sposobu renderować ją na ekranie i zapewnia następujące korzyści, które nie można uzyskać za pomocą `DisplayFormat`:

- Przeglądarka można włączyć funkcje HTML5 (na przykład pokazać kontrolki kalendarza, symbol waluty odpowiednich ustawień regionalnych, przesyłanie pocztą e-mail łączy kilka po stronie klienta. dane wejściowe sprawdzania poprawności, itp.).
- Domyślnie, przeglądarka wyświetli dane przy użyciu poprawny format, w oparciu o swoje [ustawień regionalnych](https://msdn.microsoft.com/library/vstudio/wyzd2bce.aspx).
- [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) atrybutu aby umożliwić MVC wybrać szablon po prawej stronie pola w celu przedstawienia tych danych ( [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) korzysta z szablonu ciągu). Aby uzyskać więcej informacji, zobacz Brad Wilson [szablony programu ASP.NET MVC 2](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html). (Chociaż napisane dla platformy MVC 2, w tym artykule nadal obowiązuje ograniczenie do bieżącej wersji platformy ASP.NET MVC.)

Jeśli używasz `DataType` atrybutu z polem daty należy określić `DisplayFormat` atrybut również w celu zapewnienia, że pole poprawnie renderowana w przeglądarkach Chrome. Aby uzyskać więcej informacji, zobacz [wątek w witrynie StackOverflow](http://stackoverflow.com/questions/12633471/mvc4-datatype-date-editorfor-wont-display-date-value-in-chrome-fine-in-ie).

Aby uzyskać więcej informacji na temat obsługi innych formatów daty, MVC, przejdź do [MVC 5 wprowadzenie: Badanie metod edycji i widoku Edycja](../introduction/examining-the-edit-methods-and-edit-view.md) i wyszukiwania na stronie &quot;internacjonalizacji&quot;.

Ponownie uruchom stronę indeksu dla uczniów i zwróć uwagę, nie są wyświetlane godziny dla daty rejestracji. Taki sam plik będzie znajdował się w każdym widoku, który używa `Student` modelu.

![Students_index_page_with_formatted_date](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image2.png)

### <a name="the-stringlengthattribute"></a>StringLengthAttribute

Można również określić reguły sprawdzania poprawności danych i komunikatów o błędach weryfikacji przy użyciu atrybutów. [Atrybut StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) Ustawia maksymalną długość w bazie danych i zapewnia po stronie klienta i po stronie serwera sprawdzania poprawności dla platformy ASP.NET MVC. Minimalna długość ciągu można również określić, w tym atrybucie, ale wartość minimalna nie ma wpływu na schemat bazy danych.

Załóżmy, że chcesz upewnić się, że użytkownicy nie wprowadzić więcej niż 50 znaków dla nazwy. Aby dodać to ograniczenie [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) atrybuty do `LastName` i `FirstMidName` właściwości, jak pokazano w poniższym przykładzie:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample3.cs?highlight=10,12)]

[StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) atrybutu nie uniemożliwić wprowadzanie białe miejsca dla nazwy użytkownika. Możesz użyć [wyrażenia regularnego](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) atrybutu, aby zastosować ograniczenia do danych wejściowych. Na przykład poniższy kod wymaga pierwszy znak na wielkie litery, a pozostałe znaki, aby być znakiem alfabetycznym:

`[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]`

[MaxLength](https://msdn.microsoft.com/library/System.ComponentModel.DataAnnotations.MaxLengthAttribute.aspx) atrybutu zapewnia funkcje podobne do [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) atrybutu, ale nie zapewnia po stronie klienta sprawdzania poprawności.

Uruchom aplikację, a następnie kliknij przycisk **studentów** kartę. Zostanie wyświetlony następujący błąd:

*Model kopii kontekstu "SchoolContext" została zmieniona od czasu utworzenia bazy danych. Należy rozważyć użycie migracje Code First w aktualizacji bazy danych ([https://go.microsoft.com/fwlink/?LinkId=238269](https://go.microsoft.com/fwlink/?LinkId=238269)).*

Model bazy danych została zmieniona w taki sposób, że wymaga zmiany w schemacie bazy danych i Entity Framework wykrył, że. Użyjesz migracji do zaktualizowania schematu bez utraty danych, który został dodany do bazy danych przy użyciu interfejsu użytkownika. Jeśli zmienisz danych, który został utworzony przy `Seed` metody, która ma zostać zmieniona do pierwotnego stanu z powodu [AddOrUpdate](https://msdn.microsoft.com/library/hh846520(v=vs.103).aspx) metodę, której używasz w `Seed` metody. ([AddOrUpdate](https://msdn.microsoft.com/library/hh846520(v=vs.103).aspx) jest równoważna z operacją "upsert" z bazy danych terminologii.)

W konsoli Menedżera pakietów (PMC) wprowadź następujące polecenia:

[!code-console[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample4.cmd)]

`add-migration` Polecenie tworzy plik o nazwie  *&lt;sygnatura czasowa&gt;\_MaxLengthOnNames.cs*. Ten plik zawiera kod w `Up` metodę, która spowoduje zaktualizowanie bazy danych, aby dopasować bieżącym modelem danych. `update-database` Polecenie zadziałało kod.

Sygnatura czasowa dołączony do nazwy pliku migracji umożliwia przez program Entity Framework kolejność migracji. Możesz utworzyć wiele migracji przed uruchomieniem `update-database` polecenia, a następnie wszystkie migracje są stosowane w kolejności, w którym zostały utworzone.

Uruchom **Utwórz** stronie, a następnie wprowadź albo nazwa jest dłuższa niż 50 znaków. Po kliknięciu **Utwórz**, weryfikacji po stronie klienta zawiera komunikat o błędzie: *Pola Nazwisko musi być ciągiem o maksymalnej długości 50.*

### <a name="the-column-attribute"></a>Atrybut kolumny

Umożliwia także atrybuty kontrolować, jak klas i właściwości są mapowane do bazy danych. Załóżmy, że używasz nazwy `FirstMidName` nazwę pierwszego pola, ponieważ pole może także zawierać imienia. Jednak kolumna bazy danych, można go nazwać `FirstName`, ponieważ użytkownicy, którzy będą Pisanie zapytań ad hoc w bazie danych są przyzwyczajeni do tej nazwy. Aby to mapowanie, można użyć `Column` atrybutu.

`Column` Atrybut określa, że po utworzeniu bazy danych, kolumna `Student` tabelę, która mapuje do `FirstMidName` właściwość o nazwie `FirstName`. Innymi słowy, gdy kod odwołuje się do `Student.FirstMidName`, dane będą pochodzić z lub aktualizowane w `FirstName` kolumny `Student` tabeli. Jeśli nie określisz nazwy kolumn, otrzymują one taką samą nazwę jak nazwa właściwości.

W *Student.cs* Dodaj `using` poufności informacji dotyczące [System.ComponentModel.DataAnnotations.Schema](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.aspx) i Dodaj atrybut nazwy kolumny, aby `FirstMidName` właściwości, jak pokazano w następujący wyróżniony kod:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample5.cs?highlight=4,14)]

Dodanie [atrybut kolumny](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute.aspx) zmiany modelu kopii SchoolContext, dzięki czemu nie będzie ona zgodna z bazą danych. Wprowadź następujące polecenia w konsoli zarządzania Pakietami, aby utworzyć inną migracji:

[!code-console[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample6.cmd)]

W **Eksploratora serwera**, otwórz *uczniów* projektanta tabel, klikając dwukrotnie plik *uczniów* tabeli.

Na poniższej ilustracji przedstawiono oryginalna nazwa kolumny w jakim był, zanim stosowane najpierw dwie migracje. Oprócz nazwy kolumn, zmiana z `FirstMidName` do `FirstName`, dwóch kolumn będących nazwy zostały zmienione od `MAX` długość to 50 znaków.

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image5.png)

Można również zmienić bazę danych mapowania przy użyciu [Fluent API](https://msdn.microsoft.com/data/jj591617), jak można zauważyć w dalszej części tego samouczka.

> [!NOTE]
> Jeśli spróbujesz skompilować przed zakończeniem, tworzenie wszystkich klas jednostek w poniższych sekcjach, możesz otrzymać błędy kompilatora.

## <a name="update-student-entity"></a>Aktualizowanie jednostki dla uczniów

W *Models\Student.cs*, Zastąp kod został dodany wcześniej następujący kod. Zmiany są wyróżnione.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample7.cs?highlight=11,13,15,18,22,25-32)]

### <a name="the-required-attribute"></a>Wymagany atrybut

[Wymaganego atrybutu](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) sprawia, że nazwa właściwości wymagane pola. `Required attribute` Jest zbędny w przypadku typów wartości, takie jak DateTime, int, double i float. Typy wartości nie można przypisać wartość null, więc natury są traktowane jako wymagane pola. Można usunąć [wymaganego atrybutu](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) i zastąp parametr długości minimalnej `StringLength` atrybutu:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample8.cs?highlight=2)]

### <a name="the-display-attribute"></a>Atrybut wyświetlania

`Display` Atrybut określa, czy podpis dla pól tekstowych powinny być "Imię", "Last Name", "Pełna nazwa" i "Data rejestracji" zamiast nazwy właściwości, w każdym wystąpieniu (który nie ma miejsca dzielenia wyrazów).

### <a name="the-fullname-calculated-property"></a>Imię i nazwisko obliczona właściwość

`FullName` jest właściwością obliczeniową, która zwraca wartość, która jest tworzona przez dołączenie dwóch innych właściwości. W związku z tym jest tylko `get` metody dostępu i nie `FullName` kolumny zostanie wygenerowany w bazie danych.

## <a name="create-instructor-entity"></a>Tworzenie jednostki przez instruktorów

Tworzenie *Models\Instructor.cs*, zastępując kod szablonu poniższym kodem:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample9.cs)]

Należy zauważyć, że kilka właściwości jest taka sama w `Student` i `Instructor` jednostek. W [Implementowanie dziedziczenia](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md) samouczek w dalszej części tej serii, będzie Refaktoryzuj ten kod w celu wyeliminowania nadmiarowości.

W jednej linii, można umieścić wiele atrybutów, dzięki czemu można także napisać instruktora klasy w następujący sposób:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample10.cs)]

### <a name="the-courses-and-officeassignment-navigation-properties"></a>Kursy i OfficeAssignment właściwości nawigacji

`Courses` i `OfficeAssignment` właściwości są właściwości nawigacji. Jak wyjaśniono wcześniej, są zazwyczaj definiowane jako [wirtualnego](https://msdn.microsoft.com/library/9fkccyh4(v=vs.110).aspx) tak, aby można było korzystać funkcję programu Entity Framework [powolne ładowanie](https://msdn.microsoft.com/magazine/hh205756.aspx). Ponadto, jeśli właściwość nawigacji może zawierać wiele jednostek, jego typ musi implementować [ICollection&lt;T&gt; ](https://msdn.microsoft.com/library/92t2ye13.aspx) interfejsu. Na przykład [IList&lt;T&gt; ](https://msdn.microsoft.com/library/5y536ey6.aspx) ale nie kwalifikuje się [IEnumerable&lt;T&gt; ](https://msdn.microsoft.com/library/9eekhta0.aspx) ponieważ `IEnumerable<T>` nie implementuje [Dodaj ](https://msdn.microsoft.com/library/63ywd54z.aspx).

Pod kierunkiem instruktora nauczyć dowolnej liczby kursów, więc `Courses` jest zdefiniowany jako kolekcja `Course` jednostek.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

Reguły biznesowe stanu pod kierunkiem instruktora może zawierać tylko co najwyżej jednego pakietu office, więc `OfficeAssignment` jest zdefiniowany jako pojedynczy `OfficeAssignment` jednostki (które mogą być `null` Jeśli nie przypisano żadnych pakietu office).

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

## <a name="create-officeassignment-entity"></a>Tworzenie jednostki OfficeAssignment

Tworzenie *Models\OfficeAssignment.cs* następującym kodem:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample13.cs)]

Skompiluj projekt, co spowoduje zapisanie zmian i sprawdza, czy nie utworzono żadnej kopii i wkleić błędy, które kompilator może przechwycić.

### <a name="the-key-attribute"></a>Atrybut klucza

Brak relacji jeden do zero lub jeden między `Instructor` i `OfficeAssignment` jednostek. Biuro istnieje tylko w odniesieniu do przez instruktorów, jest ona przypisana do, a zatem jej klucz podstawowy jest również klucz obcy, aby `Instructor` jednostki. Ale Entity Framework automatycznie nie może rozpoznać `InstructorID` jako podstawowy klucza tej jednostki, ponieważ jego nazwa nie postępuj zgodnie z `ID` lub *classname* `ID` konwencji nazewnictwa. W związku z tym `Key` atrybut jest używany do identyfikowania jej jako klucza:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample14.cs)]

Można również użyć `Key` atrybutu, jeśli jednostka ma swój własny klucz podstawowy, ale chcesz coś, co jest inna niż nazwa właściwości `classnameID` lub `ID`. Domyślnie EF traktuje klucza jako inne niż wygenerowane bazy danych, ponieważ kolumna jest przeznaczona dla relacji identyfikującej.

### <a name="the-foreignkey-attribute"></a>Atrybut klucza obcego

Gdy istnieje relacja jeden do zero lub jeden lub relacja jeden do jednego między dwiema jednostkami (takich jak między `OfficeAssignment` i `Instructor`), EF nie może działać, które end relacji jest podmiot zabezpieczeń i zakończenia, który jest zależny. Relacje jeden do jednego ma odwołanie do właściwości nawigacji w każdej klasie do innej klasy. [Atrybutu klucza obcego](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute.aspx) mogą być stosowane do klasy zależne do ustanawiania relacji. Jeżeli pominięto [atrybutu klucza obcego](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute.aspx), zostanie wyświetlony następujący błąd podczas próby utworzenia migracji:

*Nie można ustalić głównego końca skojarzenia między typami "ContosoUniversity.Models.OfficeAssignment" i "ContosoUniversity.Models.Instructor". Główny koniec tego skojarzenia muszą być jawnie skonfigurowane, przy użyciu interfejsu API fluent relacji lub adnotacji danych.*

W dalszej części samouczka pokazano, jak skonfigurować tę relację przy użyciu interfejsu API fluent.

### <a name="the-instructor-navigation-property"></a>Właściwość nawigacji przez instruktorów

`Instructor` Jednostka ma dopuszczający wartości null `OfficeAssignment` właściwości nawigacji (ponieważ pod kierunkiem instruktora, może nie mieć przypisania pakietu office) i `OfficeAssignment` jednostka ma wartość null `Instructor` właściwości nawigacji (ponieważ nie przypisania pakietu office istnieje bez pod kierunkiem instruktora — `InstructorID` nie dopuszcza wartości null). Gdy `Instructor` jednostka ma powiązane `OfficeAssignment` jednostki każda jednostka ma odwołania do innych niż w jego właściwości nawigacji.

Możesz umieścić `[Required]` atrybutu właściwość nawigacji przez instruktorów, aby określić, że muszą być powiązane przez instruktorów, ale nie trzeba tego robić, ponieważ nie dopuszczają wartości klucza obcego InstructorID (który również jest kluczem do tej tabeli).

## <a name="modify-the-course-entity"></a>Modyfikowanie jednostek kursu

W *Models\Course.cs*, Zastąp kod został dodany wcześniej następujący kod:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample15.cs)]

Jednostka kurs ma właściwość klucza obcego `DepartmentID` który wskazuje powiązane `Department` jednostki, ma `Department` właściwości nawigacji. Entity Framework nie wymaga dodanie właściwości klucza obcego w modelu danych mają właściwości nawigacji dla powiązanej jednostki. EF automatycznie tworzy klucze obce w bazie danych wszędzie tam, gdzie jest to konieczne. Ale o klucza obcego w modelu danych, że aktualizacje są prostszy i efektywniejszy. Na przykład podczas pobierania jednostki kurs, aby edytować, `Department` jednostki ma wartość null Jeśli nie zostanie załadowany, więc po zaktualizowaniu jednostki kurs, trzeba najpierw pobrać `Department` jednostki. Gdy właściwość klucza obcego `DepartmentID` znajduje się w modelu danych nie trzeba pobrać `Department` jednostki przed uaktualnieniem.

### <a name="the-databasegenerated-attribute"></a>Atrybut DatabaseGenerated

[Atrybut DatabaseGenerated](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute.aspx) z [Brak](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedoption(v=vs.110).aspx) parametru `CourseID` właściwość określa, że wartości klucza podstawowego są dostarczone przez użytkownika, a nie generowane przez bazę danych.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample16.cs)]

Domyślnie platforma Entity Framework przyjęto założenie, że wartości klucza podstawowego są generowane przez bazę danych. To, co ma w większości scenariuszy. Jednak w przypadku `Course` jednostki, będzie numer kurs określonych przez użytkownika, takich jak serie 1000 służy do jednego działu, serię 2000 do innego działu i tak dalej.

### <a name="foreign-key-and-navigation-properties"></a>Klucz obcy i właściwości nawigacji

Właściwości klucza obcego i właściwości nawigacji w `Course` jednostki odzwierciedlają się następująco:

- Kurs jest przypisany do jednego działu, więc ma `DepartmentID` klucza obcego i `Department` właściwość nawigacji z powodów wymienionych powyżej.

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample17.cs)]
- Kurs może mieć dowolną liczbę uczniów zarejestrowane, więc `Enrollments` właściwość nawigacji jest kolekcją:

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample18.cs)]
- Kurs może być prowadzone przez instruktorów wielu, więc `Instructors` właściwość nawigacji jest kolekcją:

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample19.cs)]

## <a name="create-the-department-entity"></a>Tworzenie jednostki działu

Tworzenie *Models\Department.cs* następującym kodem:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample20.cs)]

### <a name="the-column-attribute"></a>Atrybut kolumny

Wcześniej użyto [atrybut kolumny](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute.aspx) zmienić mapowanie nazwy kolumny. W kodzie dla `Department` jednostki `Column` atrybut jest używany do zmienić mapowanie typu danych SQL, tak aby kolumna zostanie zdefiniowana przy użyciu programu SQL Server [pieniądze](https://msdn.microsoft.com/library/ms179882.aspx) typu w bazie danych:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample21.cs)]

Mapowanie kolumn zazwyczaj nie jest wymagana, ponieważ Entity Framework zazwyczaj wybiera się odpowiedni typ danych programu SQL Server, na podstawie typu CLR, który zdefiniujesz dla właściwości. Środowisko CLR `decimal` typu map do programu SQL Server `decimal` typu. Ale w takim przypadku wiesz, że kolumna będzie zawierający kwoty waluty, a [pieniądze](https://msdn.microsoft.com/library/ms179882.aspx) typ danych jest bardziej odpowiednia dla tego. Aby uzyskać więcej informacji na temat typy danych CLR i jak pasują do typów danych programu SQL Server, zobacz [SqlClient dla typów programu Entity Framework](https://msdn.microsoft.com/library/bb896344.aspx).

### <a name="foreign-key-and-navigation-properties"></a>Klucz obcy i właściwości nawigacji

Obcy właściwości klucza i nawigacji odzwierciedla się następująco:

- Dział może lub nie ma uprawnienia administratora, a administrator zawsze ma pod kierunkiem instruktora. W związku z tym `InstructorID` właściwość jest dołączana jako kluczem obcym `Instructor` jednostki i znak zapytania, które są dodawane po `int` wpisz oznaczenia, aby oznaczyć właściwości jako dopuszczającego wartość null. Właściwość nawigacji jest o nazwie `Administrator` , ale przechowuje `Instructor` jednostki:

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample22.cs)]
- Dział może mieć wielu kursów, więc ma `Courses` właściwość nawigacji:

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample23.cs)]

  > [!NOTE]
  > Zgodnie z Konwencją platformy Entity Framework umożliwia usuwanie kaskadowe nieprzyjmujące wartości kluczy obcych i w przypadku relacji wiele do wielu. Może to spowodować cykliczne cascade delete reguł, które spowoduje wyjątek podczas próby dodania do migracji. Na przykład, jeśli nie zostały zdefiniowane `Department.InstructorID` właściwość jako dopuszczającego wartość null, otrzymamy następujący komunikat o wyjątku: "Więzy relacji spowoduje odwołanie cykliczne, w którym jest niedozwolone." W razie potrzeby własne reguły biznesowe `InstructorID` właściwość jako wartość null, trzeba wyłączyć przy użyciu następującej instrukcji interfejsu API fluent usuwanie kaskadowe relacji:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample24.cs)]

## <a name="modify-the-enrollment-entity"></a>Modyfikowanie jednostek rejestracji

 W *Models\Enrollment.cs*, Zastąp kod został dodany wcześniej następujący kod

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample25.cs?highlight=1,15)]

### <a name="foreign-key-and-navigation-properties"></a>Klucz obcy i właściwości nawigacji

Właściwości klucza obcego i właściwości nawigacji odzwierciedla się następująco:

- Rekord rejestracji jest jednego kursu, więc ma `CourseID` właściwość klucza obcego i `Course` właściwość nawigacji:

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample26.cs)]
- Rekord rejestracji jest dla uczniów lub studentów pojedynczego, więc ma `StudentID` właściwość klucza obcego i `Student` właściwość nawigacji:

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample27.cs)]

### <a name="many-to-many-relationships"></a>Relacje wiele do wielu

Istnieje relacja wiele do wielu między `Student` i `Course` jednostki, a `Enrollment` jednostki działa jako tabelę sprzężenia wiele do wielu *z ładunkiem* w bazie danych. Oznacza to, że `Enrollment` tabela zawiera dane dodatkowe, oprócz kluczy obcych do tabel sprzężonych (w tym przypadku kluczem podstawowym i `Grade` właściwości).

Poniższa ilustracja przedstawia, jak wyglądają te relacje w diagramie jednostki. (Ten diagram został wygenerowany za pomocą [Entity Framework Power Tools](https://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d); tworzenia diagramu nie należały do tego samouczka, po prostu jest on używany tutaj jako ilustrację.)

![Student Course_many do many_relationship](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image12.png)

Każdy wiersz relacji ma wartość 1 na jednym końcu i znak gwiazdki (\*) na drugim, wskazując relacji jeden do wielu.

Jeśli `Enrollment` tabeli nie włączono informacji klasy korporacyjnej, czy tylko musi on zawierać dwa klucze obce `CourseID` i `StudentID`. W takiej sytuacji będzie odpowiadać na tabelę sprzężenia wiele do wielu *bez ładunku* (lub *czystego sprzężenie tabeli*) w bazie danych i nie należy utworzyć klasę modelu dla niej w ogóle. `Instructor` i `Course` jednostki mają tego rodzaju relacji wiele do wielu i jak widać, między nimi jest Brak klasy jednostki:

![Instructor-Course_many-to-many_relationship](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image13.png)

Tabelę sprzężenia jest jednak wymagany w bazie danych, jak pokazano na poniższym diagramie bazy danych:

![Instructor-Course_many-to-many_relationship_tables](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image14.png)

Entity Framework automatycznie tworzy `CourseInstructor` tabeli, a Odczyt i zaktualizować go pośrednio za odczytywanie i aktualizowanie `Instructor.Courses` i `Course.Instructors` właściwości nawigacji.

## <a name="entity-relationship-diagram"></a>Diagram relacji jednostki

Poniższa ilustracja przedstawia diagram, który narzędzi Entity Framework Power Tools Tworzenie modelu ukończone szkoły.

![School_data_model_diagram](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image1.png)

Oprócz liniach relacji wiele do wielu (\* do \*) i liniach relacji jeden do wielu (1, aby \*), możesz teraz zobaczyć wiersz relacji jeden do zero lub jeden (1 się od 0 do 1) między `Instructor` i `OfficeAssignment` jednostki i wiersz relacji zero lub jeden do wielu (0.. 1 na \*) między jednostkami przez instruktorów i działu.

## <a name="add-code-to-database-context"></a>Dodaj kod do kontekstu bazy danych

Następnie dodasz nowe jednostki do `SchoolContext` klasy i dostosowywać niektóre przy użyciu mapowania [interfejsu API fluent](https://msdn.microsoft.com/data/jj591617) wywołania. Interfejs API jest "fluent", ponieważ jest ona często używana przez centrali szereg wywołań metod w pojedynczej instrukcji, jak w poniższym przykładzie:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample28.cs)]

W tym samouczku użyjesz interfejsu API fluent tylko dla mapowania bazy danych, nie można wykonać za pomocą atrybutów. Jednak umożliwia także wygodnego interfejsu API do określenia większość formatowania, sprawdzanie poprawności i reguł mapowania, które można wykonać przy użyciu atrybutów. Niektóre atrybuty, takie jak `MinimumLength` nie można zastosować za pomocą interfejsu API fluent. Jak wspomniano wcześniej, `MinimumLength` nie zmienił się schemat, ma zastosowanie tylko reguły walidacji po stronie klienta i serwera

Niektórzy deweloperzy wolą używać interfejsu API fluent, wyłącznie tak, aby ich zachowania ich klasami jednostki "Wyczyść". Jeśli chcesz, a kilka dostosowania, które jest możliwe tylko przy użyciu interfejsu API fluent można łączyć atrybutów i wygodnego interfejsu API, ale ogólnie rzecz biorąc zalecaną praktyką jest, wybierz jedną z tych dwóch metod i używać oznacza spójnie możliwie.

Aby dodać nowe jednostki do danych modelu i wykonać mapowanie bazy danych, które nie przy użyciu atrybutów, Zastąp kod w *DAL\SchoolContext.cs* następującym kodem:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample29.cs)]

Nowy raport w [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) metoda konfiguruje sprzężona wiele do wielu tabela:

- Dla relacji wiele do wielu między `Instructor` i `Course` jednostek, kod określa nazwy tabel i kolumn dla tabeli sprzężenia. Kod najpierw skonfigurować relacji wiele do wielu za Ciebie bez tego kodu, ale jeśli nie chcesz wywołać, otrzymasz domyślnych nazw takich jak `InstructorInstructorID` dla `InstructorID` kolumny.

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample30.cs)]

Poniższy kod zawiera przykład sposobu użycia możesz może mieć interfejs fluent API zamiast atrybutów do określania relacji między `Instructor` i `OfficeAssignment` jednostki:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample31.cs)]

Aby dowiedzieć się, jak instrukcji "interfejsu API fluent" działania w tle, zobacz [Fluent API](https://blogs.msdn.com/b/aspnetue/archive/2011/05/04/entity-framework-code-first-tutorial-supplement-what-is-going-on-in-a-fluent-api-call.aspx) wpis w blogu.

## <a name="seed-database-with-test-data"></a>Baza danych inicjatora, przy użyciu danych testowych

Zastąp kod w *Migrations\Configuration.cs* pliku następującym kodem w celu dostarczenia danych inicjatora dla nowych jednostek, które zostały utworzone.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample32.cs)]

Jak przedstawiono w pierwszym samouczku większość ten kod po prostu aktualizacji lub tworzy nowe obiekty jednostki i ładuje przykładowych danych do właściwości zgodnie z potrzebami testowania. Należy jednak zauważyć, jak `Course` jednostki, która jest w relacji wiele do wielu za pomocą `Instructor` odbywa się w jednostce:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample33.cs)]

Po utworzeniu `Course` obiektu, należy zainicjować `Instructors` właściwość nawigacji jako pustą kolekcję przy użyciu kodu `Instructors = new List<Instructor>()`. Dzięki temu można dodać `Instructor` jednostek, które są powiązane z tym `Course` przy użyciu `Instructors.Add` metody. Jeśli pusta lista nie jest utworzona, nie można dodać te relacje ponieważ `Instructors` właściwość może mieć wartości null i nie ma `Add` metody. Inicjalizacja listy można również dodać do konstruktora.

## <a name="add-a-migration"></a>Dodaj migrację

W konsoli zarządzania Pakietami, wprowadź `add-migration` polecenie (nie `update-database` polecenia jeszcze):

`add-Migration ComplexDataModel`

Jeśli próbowano uruchomić `update-database` polecenia w tym momencie (nie rób tego jeszcze), czy występuje następujący błąd:

*Instrukcja ALTER TABLE konflikt z ograniczeniem klucza OBCEGO "klucza Obcego\_dbo. Kurs\_dbo. Dział\_DepartmentID ". Konflikt wystąpił w bazie danych "ContosoUniversity" table "dbo. Dział", kolumna"DepartmentID".*

Czasami, podczas wykonywania migracji z istniejącymi danymi, należy wstawić dane do bazy danych, aby spełnić ograniczeń klucza obcego, a to, co musisz zrobić teraz. Kod wygenerowany w ComplexDataModel `Up` metoda dodaje dopuszcza `DepartmentID` klucz obcy, aby `Course` tabeli. Ponieważ istnieje już wierszy w `Course` tabeli, gdy kod działa, `AddColumn` operacja zakończy się niepowodzeniem, ponieważ program SQL Server nie może ustalić, jakie wartości do umieszczenia w kolumnie, która nie może mieć wartości null. W związku z tym konieczna zmiana kodu, aby nadać nową kolumnę wartości domyślnej i utworzyć dział szkieletu o nazwie "Temp" jako domyślnego działu. Co w efekcie istniejący `Course` wiersze zostaną wszystkie powiązane do działu "Temperatura", po `Up` metoda przebiegów. Można go powiązać poprawne działom `Seed` metody.

Edytuj &lt; *sygnatura czasowa&gt;\_ComplexDataModel.cs* , komentarz wiersz kodu, która doda kolumnę DepartmentID do tabeli Kurs i Dodaj następujący wyróżniony kod (komentarze wiersz jest również wyróżnione):

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample34.cs?highlight=14-18)]

Podczas `Seed` uruchamia metody zostaną wstawione wiersze w `Department` tabeli i będą dotyczyły istniejące `Course` wiersze te nowe `Department` wierszy. Jeśli jeszcze nie dodano żadnych kursów w interfejsie użytkownika, nie jest już należałoby wtedy dział "Temp" lub wartość domyślną w `Course.DepartmentID` kolumny. Aby zezwolić na możliwość, że ktoś może dodano kursy za pomocą aplikacji, będzie również chcesz zaktualizować `Seed` kod metody, aby upewnić się, że wszystkie `Course` wierszy (nie tylko tych, które zostały wstawione przez wcześniejszych przebiegów `Seed` metoda) mają Nieprawidłowa `DepartmentID` wartości przed usunięciem domyślną wartość z kolumny i Usuń dział "Temp".

## <a name="update-the-database"></a>Aktualizowanie bazy danych

Po zakończeniu edycji &lt; *sygnatura czasowa&gt;\_ComplexDataModel.cs* pliku, wprowadź `update-database` polecenia w konsoli zarządzania Pakietami można wykonać migracji.

[!code-powershell[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample35.ps1)]

> [!NOTE]
> Istnieje możliwość uzyskać inne błędy, podczas migracji danych i wprowadzania zmian schematu. Jeśli występują błędy migracji, których nie można rozwiązać, możesz zmienić nazwę bazy danych w parametrach połączenia lub usunąć bazy danych. Najprostszą metodą jest zmiana nazwy bazy danych w *Web.config* pliku. Poniższy przykład przedstawia nazwę zmieniono na CU\_testu:
>
> [!code-xml[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample36.xml?highlight=1)]
>
> Za pomocą nowej bazy danych, nie ma żadnych danych, aby przeprowadzić migrację oraz `update-database` polecenia jest znacznie bardziej prawdopodobne zakończyć bez błędów. Aby uzyskać instrukcje dotyczące sposobu usuwania z bazy danych, zobacz [jak usunąć bazę danych z programu Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/).
>
> W przypadku niepowodzenia inną rzeczą, którą możesz spróbować jest ponownie zainicjować bazy danych, wprowadzając następujące polecenie w konsoli zarządzania Pakietami:
>
> `update-database -TargetMigration:0`

Otwórz bazę danych w **Eksploratora serwera** były wykonywane wcześniej, i rozwiń **tabel** węzeł, aby zobaczyć, czy wszystkie tabele zostały utworzone. (Jeśli nadal masz **Eksploratora serwera** Otwórz od wcześniejszego stanu, kliknij pozycję **Odśwież** przycisku.)

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image16.png)

Nie utworzono klasę modelu `CourseInstructor` tabeli. Jak wyjaśniono wcześniej, jest to tabela sprzężenia dla relacji wiele do wielu między `Instructor` i `Course` jednostek.

Kliknij prawym przyciskiem myszy `CourseInstructor` tabeli, a następnie wybierz pozycję **Pokaż dane tabeli** Aby sprawdzić, czy zawiera dane w nim na `Instructor` jednostki został dodany do `Course.Instructors` właściwości nawigacji.

![Table_data_in_CourseInstructor_table](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image17.png)

## <a name="get-the-code"></a>Pobierz kod

[Pobierz ukończony projekt](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Dodatkowe zasoby

Linki do innych zasobów platformy Entity Framework można znaleźć w [dostęp do danych platformy ASP.NET — zalecane zasoby](../../../../whitepapers/aspnet-data-access-content-map.md).

## <a name="next-steps"></a>Następne kroki

W ramach tego samouczka możesz:

> [!div class="checklist"]
> * Dostosowany model danych
> * Zaktualizowana jednostka dla uczniów
> * Utworzonej jednostki przez instruktorów
> * Utworzonej jednostki OfficeAssignment
> * Zmodyfikować jednostki kursu
> * Utworzone jednostki działu
> * Zmodyfikować jednostki rejestracji
> * Dodano kod kontekst bazy danych
> * Wypełnionych bazy danych za pomocą danych testowych
> * Dodano migracji
> * Aktualizacji bazy danych

Przejdź do następnego artykułu, aby dowiedzieć się, jak czytać i wyświetlanie powiązanych danych Entity Framework ładującego do właściwości nawigacji.

> [!div class="nextstepaction"]
> [Odczytywanie powiązanych danych](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)

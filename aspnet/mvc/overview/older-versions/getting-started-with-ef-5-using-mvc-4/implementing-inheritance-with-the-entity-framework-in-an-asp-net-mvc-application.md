---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
title: Implementowanie dziedziczenia przy użyciu Entity Framework w aplikacji ASP.NET MVC (8 z 10) | Microsoft Docs
author: tdykstra
description: Przykładowa aplikacja internetowa Contoso University pokazuje, jak tworzyć aplikacje ASP.NET MVC 4 przy użyciu Code First Entity Framework 5 i programu Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: a5c3feff-5335-4cdd-a97d-f7a8785c2494
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 9507cba71b976825257cc9948d54f2651355959d
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74595320"
---
# <a name="implementing-inheritance-with-the-entity-framework-in-an-aspnet-mvc-application-8-of-10"></a>Implementowanie dziedziczenia przy użyciu Entity Framework w aplikacji ASP.NET MVC (8 z 10)

Autor [Dykstra](https://github.com/tdykstra)

[Pobierz ukończony projekt](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Przykładowa aplikacja internetowa Contoso University pokazuje, jak tworzyć aplikacje ASP.NET MVC 4 przy użyciu Entity Framework 5 Code First i programu Visual Studio 2012. Aby uzyskać informacje na temat serii samouczków, zobacz [pierwszy samouczek w serii](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Możesz uruchomić serię samouczków od początku lub [pobrać początkowy projekt dla tego rozdziału](building-the-ef5-mvc4-chapter-downloads.md) i Zacznij tutaj.
> 
> > [!NOTE] 
> > 
> > Jeśli wystąpi problem, którego nie można rozwiązać, [Pobierz ukończony rozdział](building-the-ef5-mvc4-chapter-downloads.md) i spróbuj odtworzyć problem. Ogólnie rzecz biorąc, można znaleźć rozwiązanie problemu, porównując kod z kompletnym kodem. Niektóre typowe błędy i sposoby ich rozwiązywania można znaleźć w temacie [błędy i obejścia.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)

W poprzednim samouczku zostały obsłużone wyjątki współbieżności. W tym samouczku pokazano, jak zaimplementować dziedziczenie w modelu danych.

W programowaniu zorientowanym obiektowo można użyć dziedziczenia, aby wyeliminować nadmiarowy kod. W tym samouczku zmienisz klasy `Instructor` i `Student` tak, aby znajdowały się one w `Person` klasie bazowej, która zawiera właściwości, takie jak `LastName`, które są wspólne dla instruktorów i studentów. Nie dodasz ani nie zmienisz żadnych stron sieci Web, ale zmienisz część kodu, a zmiany zostaną automatycznie odzwierciedlone w bazie danych.

## <a name="table-per-hierarchy-versus-table-per-type-inheritance"></a>Dziedziczenie na poziomie hierarchii w zależności od typu tabeli

W programowaniu zorientowanym obiektowo można użyć dziedziczenia, aby ułatwić pracę z powiązanymi klasami. Na przykład klasy `Instructor` i `Student` w modelu danych `School` współdzielą kilka właściwości, co powoduje nadmiarowy kod:

![Student_and_Instructor_classes](https://asp.net/media/2578113/Windows-Live-Writer_58f5a93579b2_CC7B_Student_and_Instructor_classes_e7a32f99-8bc4-48ce-aeaf-216a18071a8b.png)

Załóżmy, że chcesz wyeliminować nadmiarowy kod dla właściwości, które są współużytkowane przez `Instructor` i `Student` jednostek. Można utworzyć `Person` klasę bazową, która zawiera tylko te właściwości udostępnione, a następnie uczynić jednostki `Instructor` i `Student` dziedziczone z tej klasy bazowej, jak pokazano na poniższej ilustracji:

![Student_and_Instructor_classes_deriving_from_Person_class](https://asp.net/media/2578119/Windows-Live-Writer_58f5a93579b2_CC7B_Student_and_Instructor_classes_deriving_from_Person_class_671d708c-cbb8-454a-a8f8-c2d99439acd9.png)

Istnieje kilka sposobów reprezentowania tej struktury dziedziczenia w bazie danych. Możesz mieć `Person` tabelę zawierającą informacje o studentach i instruktorach w pojedynczej tabeli. Niektóre kolumny mogą dotyczyć tylko instruktorów (`HireDate`), tylko dla studentów (`EnrollmentDate`), niektórych do obu (`LastName`, `FirstName`). Zazwyczaj masz kolumnę *rozróżniacza* , aby wskazać, który typ reprezentuje każdy wiersz. Na przykład kolumna rozróżniacza może mieć "instruktor" dla instruktorów i "Student" dla studentów.

![Tabela na hierarchy_example](https://asp.net/media/2578125/Windows-Live-Writer_58f5a93579b2_CC7B_Table-per-hierarchy_example_244067cd-b451-4e9b-9595-793b9afca505.png)

Ten wzorzec generowania struktury dziedziczenia jednostek z pojedynczej tabeli bazy danych jest nazywany dziedziczeniem *na hierarchię* (TPH).

Alternatywą jest, aby baza danych wyglądała podobnie jak struktura dziedziczenia. Na przykład można mieć tylko pola nazw w tabeli `Person` i mieć osobne tabele `Instructor` i `Student` z polami daty.

![Tabela na type_inheritance](https://asp.net/media/2578131/Windows-Live-Writer_58f5a93579b2_CC7B_Table-per-type_inheritance.png)

Ten wzorzec tworzenia tabeli bazy danych dla każdej klasy jednostek jest nazywany dziedziczeniem *tabeli na typ* (TPT).

Wzorce dziedziczenia TPH ogólnie zapewniają lepszą wydajność w Entity Framework niż wzorce dziedziczenia TPT, ponieważ wzorce TPT mogą powodować złożone zapytania sprzężenia. W tym samouczku pokazano, jak wdrożyć dziedziczenie TPH. Można to zrobić, wykonując następujące czynności:

- Utwórz klasę `Person` i Zmień klasy `Instructor` i `Student` na pochodne od `Person`.
- Dodaj kod mapowania model-do-Database do klasy kontekstu bazy danych.
- Zmień `InstructorID` i `StudentID` odwołania w projekcie do `PersonID`.

## <a name="creating-the-person-class"></a>Tworzenie klasy Person

 Uwaga: nie będzie możliwe skompilowanie projektu po utworzeniu poniższych klas do momentu zaktualizowania kontrolerów, które korzystają z tych klas. 

W folderze *modele* Utwórz *Person.cs* i Zastąp kod szablonu następującym kodem:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

W *Instructor.cs*należy utworzyć klasę `Instructor` z klasy `Person` i usunąć pola klucza i nazwy. Kod będzie wyglądać podobnie do poniższego przykładu:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Wprowadź podobne zmiany w *student.cs*. Klasa `Student` będzie wyglądać podobnie do poniższego przykładu:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="adding-the-person-entity-type-to-the-model"></a>Dodawanie typu jednostki osoby do modelu

W *SchoolContext.cs*dodaj Właściwość `DbSet` dla typu jednostki `Person`:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

To wszystko, co Entity Framework potrzebuje w celu skonfigurowania dziedziczenia w hierarchii na poziomie tabeli. Jak zobaczysz, gdy baza danych zostanie utworzona, będzie miała `Person` tabeli zamiast tabel `Student` i `Instructor`.

## <a name="changing-instructorid-and-studentid-to-personid"></a>Zmiana InstructorID i StudentID na PersonID

W *SchoolContext.cs*, w instrukcji szkolenia instruktora, Zmień `MapRightKey("InstructorID")` na `MapRightKey("PersonID")`:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=4)]

Ta zmiana nie jest wymagana; po prostu zmienia nazwę kolumny InstructorID w tabeli sprzężeń wiele-do-wielu. Jeśli pozostawiłeś nazwę jako InstructorID, aplikacja nadal będzie działała poprawnie. Oto zakończono *SchoolContext.cs*:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs?highlight=15,24)]

Następnie należy zmienić `InstructorID` na `PersonID` i `StudentID` `PersonID` w całym projekcie, ***z wyjątkiem*** plików migracji sygnatur czasowych w folderze *migracji* . W tym celu można znaleźć i otworzyć tylko te pliki, które należy zmienić, a następnie wprowadzić globalną zmianę w otwartych plikach. Jedynym plikiem w folderze *migracji* , który należy zmienić, jest *Migrations\Configuration.cs.*

1. > [!IMPORTANT]
   > Zacznij od zamykania wszystkich otwartych plików w programie Visual Studio.
2. Kliknij przycisk **Znajdź i Zamień — Znajdź wszystkie pliki** w menu **Edycja** , a następnie wyszukaj wszystkie pliki w projekcie zawierającym `InstructorID`.  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)
3. Otwórz każdy plik w oknie **Znajdź wyniki** , ***z wyjątkiem*** &lt;sygnatury czasowej&gt;plików migracji *CS\_* w folderze *migracje* , klikając dwukrotnie jeden wiersz dla każdego pliku.  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)
4. Otwórz okno dialogowe **Zastąp w plikach** i Zmień **wygląd** na **wszystkie otwarte dokumenty**.
5. Użyj okna dialogowego **Zastąp w plikach** , aby zmienić wszystkie `InstructorID` na `PersonID.`  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)
6. Znajdź wszystkie pliki w projekcie, które zawierają `StudentID`.
7. Otwórz każdy plik w oknie **Znajdź wyniki** , ***z wyjątkiem*** &lt;sygnatury czasowej&gt; *\_\*. cs* plików migracji w folderze *migracji* , klikając dwukrotnie jeden wiersz dla każdego pliku.  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)
8. Otwórz okno dialogowe **Zastąp w plikach** i Zmień **wygląd** na **wszystkie otwarte dokumenty**.
9. Użyj okna dialogowego **Zastąp w plikach** , aby zmienić wszystkie `StudentID` `PersonID`.   
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)
10. Skompiluj projekt.

(Należy zauważyć, że ilustruje to *niekorzyść* wzorca `classnameID` do nazewnictwa kluczy podstawowych. Jeśli masz nazwane identyfikatory kluczy podstawowych bez poprzedzania nazwy klasy, *nie* trzeba teraz zmieniać nazwy.)

## <a name="create-and-update-a-migrations-file"></a>Tworzenie i aktualizowanie pliku migracji

W konsoli Menedżera pakietów (PMC) wprowadź następujące polecenie:

`Add-Migration Inheritance`

Uruchom `Update-Database` polecenie w obszarze PMC. Polecenie zakończy się niepowodzeniem w tym momencie, ponieważ mamy istniejące dane, które nie wiedzą, jak obsłużyć migracje. Zostanie wyświetlony następujący błąd:

*Instrukcja ALTER TABLE spowodowała konflikt z ograniczeniem klucza obcego "FK\_dbo. Dział\_dbo. Osoba\_PersonID ". Konflikt wystąpił w bazie danych "ContosoUniversity", tabela "dbo. Osoba ", kolumna" PersonID ".*

Otwórz *migracje\&lt; sygnatura czasowa&gt;\_Inheritance.cs* i Zastąp metodę `Up` następującym kodem:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=25,29-40)]

Uruchom ponownie polecenie `update-database`.

> [!NOTE]
> Podczas migrowania danych i wprowadzania zmian w schemacie można uzyskać inne błędy. Jeśli wystąpią błędy migracji, nie można rozwiązać tego problemu, możesz kontynuować pracę z samouczkiem, zmieniając parametry połączenia w pliku *Web. config* lub usuwając bazę danych. Najprostszym podejściem jest zmiana nazwy bazy danych w pliku *Web. config* . Na przykład zmień nazwę bazy danych na CU\_test, jak pokazano w następującym przykładzie:
> 
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.xml?highlight=1-2)]
> 
> W przypadku nowej bazy danych nie ma żadnych danych do migrowania, a `update-database` polecenie jest znacznie bardziej prawdopodobnie wykonane bez błędów. Aby uzyskać instrukcje dotyczące sposobu usuwania bazy danych, zobacz [Jak usunąć bazę danych z programu Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/). Jeśli ta metoda zostanie zastosowana w celu kontynuowania samouczka, pomiń krok wdrożenia na końcu tego samouczka, ponieważ wdrożona witryna uzyska ten sam błąd podczas automatycznego uruchamiania migracji. Jeśli chcesz rozwiązać problem z błędami migracji, najlepszym zasobem jest jedno z forów Entity Framework lub StackOverflow.com.

## <a name="testing"></a>Testowanie

Uruchom witrynę i wypróbuj różne strony. Wszystko działa tak samo jak wcześniej.

W **Eksplorator serwera** rozwiń węzeł **SchoolContext** , a następnie **tabele**i sprawdź, czy tabele **student** i **instruktor** zostały zastąpione przez tabelę **Person** . Rozwiń tabelę **Person** i zobaczysz, że zawiera ona wszystkie kolumny, które były używane do znajdowania w tabelach **studenta** i **instruktora** .

![Server_Explorer_showing_Person_table](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Kliknij prawym przyciskiem myszy tabelę osoba, a następnie kliknij polecenie **Pokaż dane tabeli** , aby wyświetlić kolumnę rozróżniacza.

![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

Na poniższym diagramie przedstawiono strukturę nowej szkolnej bazy danych:

![School_database_diagram](https://asp.net/media/2578143/Windows-Live-Writer_58f5a93579b2_CC7B_School_database_diagram_6350a801-7199-413f-bbac-4a2009ed19d7.png)

## <a name="summary"></a>Podsumowanie

Dziedziczenie na poziomie tabeli jest teraz zaimplementowane dla klas `Person`, `Student`i `Instructor`. Aby uzyskać więcej informacji na temat tej i innych struktur dziedziczenia, zobacz [dziedziczenie strategii mapowania](https://weblogs.asp.net/manavi/archive/2010/12/24/inheritance-mapping-strategies-with-entity-framework-code-first-ctp5-part-1-table-per-hierarchy-tph.aspx) na blogu Morteza Manavi. W następnym samouczku zobaczysz kilka sposobów implementacji wzorców repozytorium i jednostki pracy.

Linki do innych zasobów Entity Framework można znaleźć w [mapie zawartości dostęp do danych ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Poprzednie](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [dalej](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)

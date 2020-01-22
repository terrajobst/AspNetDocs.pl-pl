---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
title: 'Samouczek: implementowanie dziedziczenia z EF w aplikacji ASP.NET MVC 5'
description: W tym samouczku pokazano, jak zaimplementować dziedziczenie w modelu danych.
author: tdykstra
ms.author: riande
ms.date: 01/21/2019
ms.topic: tutorial
ms.assetid: 08834147-77ec-454a-bb7a-d931d2a40dab
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 73a01ed47b0935a1a9734c197377470defb1fe36
ms.sourcegitcommit: 88fc80e3f65aebdf61ec9414810ddbc31c543f04
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/22/2020
ms.locfileid: "76519391"
---
# <a name="tutorial-implement-inheritance-with-ef-in-an-aspnet-mvc-5-app"></a>Samouczek: implementowanie dziedziczenia z EF w aplikacji ASP.NET MVC 5

W poprzednim samouczku zostały obsłużone wyjątki współbieżności. W tym samouczku pokazano, jak zaimplementować dziedziczenie w modelu danych.

W programowaniu zorientowanym obiektowo można użyć [dziedziczenia](http://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming)) , aby ułatwić [ponowne użycie kodu](http://en.wikipedia.org/wiki/Code_reuse). W tym samouczku zmienisz klasy `Instructor` i `Student` tak, aby znajdowały się one w `Person` klasie bazowej, która zawiera właściwości, takie jak `LastName`, które są wspólne dla instruktorów i studentów. Nie dodasz ani nie zmienisz żadnych stron sieci Web, ale zmienisz część kodu, a zmiany zostaną automatycznie odzwierciedlone w bazie danych.

W tym samouczku zostały wykonane następujące czynności:

> [!div class="checklist"]
> * Informacje na temat mapowania dziedziczenia do bazy danych
> * Tworzenie klasy Person
> * Aktualizuj instruktora i uczniów
> * Dodawanie osoby do modelu
> * Tworzenie i aktualizowanie migracji
> * Testowanie implementacji
> * Wdrażanie na platformie Azure

## <a name="prerequisites"></a>Wymagania wstępne

* [Obsługa współbieżności](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="map-inheritance-to-database"></a>Dziedziczenie mapowania do bazy danych

Klasy `Instructor` i `Student` w modelu danych `School` mają różne właściwości, które są identyczne:

![Student_and_Instructor_classes](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

Załóżmy, że chcesz wyeliminować nadmiarowy kod dla właściwości, które są współużytkowane przez `Instructor` i `Student` jednostek. Lub chcesz napisać usługę, która może formatować nazwy bez Caring, niezależnie od tego, czy nazwa pochodzi od instruktora, czy studenta. Można utworzyć `Person` klasę bazową, która zawiera tylko te właściwości udostępnione, a następnie uczynić jednostki `Instructor` i `Student` dziedziczone z tej klasy bazowej, jak pokazano na poniższej ilustracji:

![Student_and_Instructor_classes_deriving_from_Person_class](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

Istnieje kilka sposobów reprezentowania tej struktury dziedziczenia w bazie danych. Możesz mieć `Person` tabelę zawierającą informacje o studentach i instruktorach w pojedynczej tabeli. Niektóre kolumny mogą dotyczyć tylko instruktorów (`HireDate`), tylko dla studentów (`EnrollmentDate`), niektórych do obu (`LastName`, `FirstName`). Zazwyczaj masz kolumnę *rozróżniacza* , aby wskazać, który typ reprezentuje każdy wiersz. Na przykład kolumna rozróżniacza może mieć "instruktor" dla instruktorów i "Student" dla studentów.

![Tabela na hierarchy_example](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Ten wzorzec generowania struktury dziedziczenia jednostek z pojedynczej tabeli bazy danych jest nazywany dziedziczeniem *na hierarchię* (TPH).

Alternatywą jest, aby baza danych wyglądała podobnie jak struktura dziedziczenia. Na przykład można mieć tylko pola nazw w tabeli `Person` i mieć osobne tabele `Instructor` i `Student` z polami daty.

![Tabela na type_inheritance](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Ten wzorzec tworzenia tabeli bazy danych dla każdej klasy jednostek jest nazywany dziedziczeniem *tabeli na typ* (TPT).

Jeszcze kolejną opcją jest zamapowanie wszystkich typów nieabstrakcyjnych na poszczególne tabele. Wszystkie właściwości klasy, łącznie z dziedziczonymi właściwościami, mapują do kolumn odpowiedniej tabeli. Ten wzorzec jest nazywany dziedziczeniem klasy opartej na tabeli (TPC). W przypadku zaimplementowania dziedziczenia TPC dla klas `Person`, `Student`i `Instructor` jak pokazano wcześniej, tabele `Student` i `Instructor` nie będą wyglądały inaczej po wdrożeniu dziedziczenia niż wcześniej.

Wzorce TPHu i dziedziczenia są ogólnie zapewniające lepszą wydajność w Entity Framework niż wzorce dziedziczenia TPT, ponieważ wzorce TPT mogą powodować złożone zapytania sprzężenia.

W tym samouczku pokazano, jak wdrożyć dziedziczenie TPH. TPH jest domyślnym wzorcem dziedziczenia w Entity Framework, więc wszystko, co trzeba zrobić, to utworzenie klasy `Person`, zmiana klas `Instructor` i `Student` na podstawie `Person`, dodanie nowej klasy do `DbContext`i utworzenie migracji. (Aby uzyskać informacje na temat implementowania innych wzorców dziedziczenia, zobacz [Mapowanie dziedziczenia według typu tabeli (TPT)](https://msdn.microsoft.com/data/jj591617#2.5) i [Mapowanie dziedziczenia klasy opartej na tabeli (TPC)](https://msdn.microsoft.com/data/jj591617#2.6) w dokumentacji MSDN Entity Framework.)

## <a name="create-the-person-class"></a>Tworzenie klasy Person

W folderze *modele* Utwórz *Person.cs* i Zastąp kod szablonu następującym kodem:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

## <a name="update-instructor-and-student"></a>Aktualizuj instruktora i uczniów

Teraz zaktualizuj *Instructor.cs* i *student.cs* , aby dziedziczyły wartości z *Person.SC*.

W *Instructor.cs*należy utworzyć klasę `Instructor` z klasy `Person` i usunąć pola klucza i nazwy. Kod będzie wyglądać podobnie do poniższego przykładu:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Wprowadź podobne zmiany w *student.cs*. Klasa `Student` będzie wyglądać podobnie do poniższego przykładu:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="add-person-to-the-model"></a>Dodawanie osoby do modelu

W *SchoolContext.cs*dodaj Właściwość `DbSet` dla typu jednostki `Person`:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

To wszystko, co Entity Framework potrzebuje w celu skonfigurowania dziedziczenia w hierarchii na poziomie tabeli. Jak zobaczysz, gdy baza danych zostanie zaktualizowana, będzie miała `Person` tabeli zamiast tabel `Student` i `Instructor`.

## <a name="create-and-update-migrations"></a>Tworzenie i aktualizowanie migracji

W konsoli Menedżera pakietów (PMC) wprowadź następujące polecenie:

`Add-Migration Inheritance`

Uruchom `Update-Database` polecenie w obszarze PMC. Polecenie zakończy się niepowodzeniem w tym momencie, ponieważ mamy istniejące dane, które nie wiedzą, jak obsłużyć migracje. Zostanie wyświetlony komunikat o błędzie podobny do następującego:

> *Nie można porzucić obiektu "dbo. Instruktora, ponieważ odwołuje się do niego ograniczenie FOREIGN KEY.*

Otwórz *migracje\&lt; sygnatura czasowa&gt;\_Inheritance.cs* i Zastąp metodę `Up` następującym kodem:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

Ten kod obejmuje następujące zadania aktualizacji bazy danych:

- Usuwa ograniczenia klucza obcego i indeksy wskazujące na tabelę uczniów.
- Zmienia nazwę tabeli instruktora jako osobę i wprowadza zmiany, które są niezbędne do przechowywania danych ucznia:

    - Dodaje wartość null EnrollmentDate dla studentów.
    - Dodaje kolumnę rozróżniacza, aby wskazać, czy wiersz dotyczy studenta, czy instruktora.
    - Sprawia, że HireDate dopuszcza wartość null, ponieważ wiersze uczniów nie mają dat zatrudnienia.
    - Dodaje pole tymczasowe, które będzie używane do aktualizacji kluczy obcych, które wskazują uczniów. Podczas kopiowania uczniów do tabeli osób otrzymają nowe wartości klucza podstawowego.
- Kopiuje dane z tabeli uczniów do tabeli Person. Powoduje to, że uczniowie mogą uzyskać przypisane nowe wartości klucza podstawowego.
- Naprawia wartości kluczy obcych, które wskazują uczniów.
- Ponownie tworzy ograniczenia klucza obcego i indeksy, teraz wskazując je do tabeli Person.

(Jeśli jako typ klucza podstawowego użyto identyfikatora GUID zamiast Integer, wartości klucza podstawowego studenta nie będą musiały ulec zmianie, a niektóre z tych kroków mogły zostać pominięte).

Uruchom ponownie polecenie `update-database`.

(W systemie produkcyjnym należy wprowadzić odpowiednie zmiany w metodzie Down w przypadku, gdy kiedykolwiek było konieczne użycie tego programu w celu powrotu do poprzedniej wersji bazy danych. W tym samouczku nie będziesz używać metody Down.)

> [!NOTE]
> Podczas migrowania danych i wprowadzania zmian w schemacie można uzyskać inne błędy. Jeśli wystąpią błędy migracji, nie można rozwiązać tego problemu, możesz kontynuować pracę z samouczkiem, zmieniając parametry połączenia w pliku *Web. config* lub usuwając bazę danych. Najprostszym podejściem jest zmiana nazwy bazy danych w pliku *Web. config* . Na przykład zmień nazwę bazy danych na ContosoUniversity2, jak pokazano w następującym przykładzie:
>
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.xml?highlight=2)]
>
> W przypadku nowej bazy danych nie ma żadnych danych do migrowania, a `update-database` polecenie jest znacznie bardziej prawdopodobnie wykonane bez błędów. Aby uzyskać instrukcje dotyczące sposobu usuwania bazy danych, zobacz [Jak usunąć bazę danych z programu Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/). Jeśli zostanie to zastosowane w celu kontynuowania samouczka, pomiń krok wdrożenia na końcu tego samouczka lub Wdróż go w nowej lokacji i bazie danych. Jeśli aktualizacja została wdrożona w tej samej lokacji, która została już wdrożona, program Dr pobierze ten sam błąd podczas automatycznego uruchamiania migracji. Jeśli chcesz rozwiązać problem z błędami migracji, najlepszym zasobem jest jedno z forów Entity Framework lub StackOverflow.com.

## <a name="test-the-implementation"></a>Testowanie implementacji

Uruchom witrynę i wypróbuj różne strony. Wszystko działa tak samo jak wcześniej.

W **Eksplorator serwera** rozwiń węzeł **Data Connections\SchoolContext** , a następnie pozycję **tabele**i sprawdź, czy tabele **student** i **instruktor** zostały zastąpione przez tabelę **Person** . Rozwiń tabelę **Person** i zobaczysz, że zawiera ona wszystkie kolumny, które były używane do znajdowania w tabelach **studenta** i **instruktora** .

Kliknij prawym przyciskiem myszy tabelę osoba, a następnie kliknij polecenie **Pokaż dane tabeli** , aby wyświetlić kolumnę rozróżniacza.

Na poniższym diagramie przedstawiono strukturę nowej szkolnej bazy danych:

![School_database_diagram](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

## <a name="deploy-to-azure"></a>Wdrażanie na platformie Azure

Ta sekcja wymaga wykonania opcjonalnej sekcji **wdrażanie aplikacji do platformy Azure** w [części 3, sortowanie, filtrowanie i stronicowanie](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md) tej serii samouczków. W przypadku migracji błędów, które zostały rozwiązane przez usunięcie bazy danych w projekcie lokalnym, Pomiń ten krok; Utwórz nową witrynę i bazę danych i Wdróż ją w nowym środowisku.

1. W programie Visual Studio kliknij prawym przyciskiem myszy projekt w **Eksplorator rozwiązań** i wybierz polecenie **Publikuj** z menu kontekstowego.

2. Kliknij przycisk **publikowania**.

    Aplikacja sieci Web zostanie otwarta w domyślnej przeglądarce.

3. Przetestuj aplikację, aby upewnić się, że działa.

    Przy pierwszym uruchomieniu strony, która uzyskuje dostęp do bazy danych, Entity Framework uruchamia wszystkie migracje `Up` metody wymagane w celu przeprowadzenia Aktualności bazy danych z bieżącym modelem danych.

## <a name="get-the-code"></a>Uzyskaj kod

[Pobierz ukończony projekt](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Dodatkowe zasoby

Linki do innych zasobów Entity Framework można znaleźć w [zasobach zalecanych przez dostęp do danych ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

Aby uzyskać więcej informacji na temat tej i innych struktur dziedziczenia, zobacz [wzorzec dziedziczenia TPT](https://msdn.microsoft.com/data/jj618293) oraz [wzorzec dziedziczenia TPH](https://msdn.microsoft.com/data/jj618292) w witrynie MSDN. W następnym samouczku zobaczysz, jak obsłużyć wiele relatywnie zaawansowanych scenariuszy Entity Framework.

## <a name="next-steps"></a>Następne kroki

W tym samouczku zostały wykonane następujące czynności:

> [!div class="checklist"]
> * Zapoznaj się z mapowaniem dziedziczenia do bazy danych
> * Utworzono klasę Person
> * Zaktualizowany instruktor i student
> * Dodano osobę do modelu
> * Tworzenie i aktualizowanie migracji
> * Przetestowano implementację
> * Wdrożone na platformie Azure

Przejdź do następnego artykułu, aby dowiedzieć się więcej na temat tematów, które są przydatne, gdy przejdziesz poza podstawowe informacje na temat tworzenia aplikacji sieci Web ASP.NET korzystających z Entity Framework Code First.
> [!div class="nextstepaction"]
> [Zaawansowane scenariusze platformy Entity Framework](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)

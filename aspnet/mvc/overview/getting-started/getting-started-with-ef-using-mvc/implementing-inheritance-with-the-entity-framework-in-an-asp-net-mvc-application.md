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
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78583063"
---
# <a name="tutorial-implement-inheritance-with-ef-in-an-aspnet-mvc-5-app"></a><span data-ttu-id="c39a9-103">Samouczek: implementowanie dziedziczenia z EF w aplikacji ASP.NET MVC 5</span><span class="sxs-lookup"><span data-stu-id="c39a9-103">Tutorial: Implement Inheritance with EF in an ASP.NET MVC 5 app</span></span>

<span data-ttu-id="c39a9-104">W poprzednim samouczku zostały obsłużone wyjątki współbieżności.</span><span class="sxs-lookup"><span data-stu-id="c39a9-104">In the previous tutorial you handled concurrency exceptions.</span></span> <span data-ttu-id="c39a9-105">W tym samouczku pokazano, jak zaimplementować dziedziczenie w modelu danych.</span><span class="sxs-lookup"><span data-stu-id="c39a9-105">This tutorial will show you how to implement inheritance in the data model.</span></span>

<span data-ttu-id="c39a9-106">W programowaniu zorientowanym obiektowo można użyć [dziedziczenia](http://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming)) , aby ułatwić [ponowne użycie kodu](http://en.wikipedia.org/wiki/Code_reuse).</span><span class="sxs-lookup"><span data-stu-id="c39a9-106">In object-oriented programming, you can use [inheritance](http://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming)) to facilitate [code reuse](http://en.wikipedia.org/wiki/Code_reuse).</span></span> <span data-ttu-id="c39a9-107">W tym samouczku zmienisz klasy `Instructor` i `Student` tak, aby znajdowały się one w `Person` klasie bazowej, która zawiera właściwości, takie jak `LastName`, które są wspólne dla instruktorów i studentów.</span><span class="sxs-lookup"><span data-stu-id="c39a9-107">In this tutorial, you'll change the `Instructor` and `Student` classes so that they derive from a `Person` base class which contains properties such as `LastName` that are common to both instructors and students.</span></span> <span data-ttu-id="c39a9-108">Nie dodasz ani nie zmienisz żadnych stron sieci Web, ale zmienisz część kodu, a zmiany zostaną automatycznie odzwierciedlone w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="c39a9-108">You won't add or change any web pages, but you'll change some of the code and those changes will be automatically reflected in the database.</span></span>

<span data-ttu-id="c39a9-109">W tym samouczku zostaną wykonane następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="c39a9-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c39a9-110">Informacje na temat mapowania dziedziczenia do bazy danych</span><span class="sxs-lookup"><span data-stu-id="c39a9-110">Learn to map inheritance to database</span></span>
> * <span data-ttu-id="c39a9-111">Tworzenie klasy Person</span><span class="sxs-lookup"><span data-stu-id="c39a9-111">Create the Person class</span></span>
> * <span data-ttu-id="c39a9-112">Aktualizuj instruktora i uczniów</span><span class="sxs-lookup"><span data-stu-id="c39a9-112">Update Instructor and Student</span></span>
> * <span data-ttu-id="c39a9-113">Dodawanie osoby do modelu</span><span class="sxs-lookup"><span data-stu-id="c39a9-113">Add Person to the Model</span></span>
> * <span data-ttu-id="c39a9-114">Tworzenie i aktualizowanie migracji</span><span class="sxs-lookup"><span data-stu-id="c39a9-114">Create and Update Migrations</span></span>
> * <span data-ttu-id="c39a9-115">Testowanie implementacji</span><span class="sxs-lookup"><span data-stu-id="c39a9-115">Test the implementation</span></span>
> * <span data-ttu-id="c39a9-116">Wdrażanie na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="c39a9-116">Deploy to Azure</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c39a9-117">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="c39a9-117">Prerequisites</span></span>

* [<span data-ttu-id="c39a9-118">Obsługa współbieżności</span><span class="sxs-lookup"><span data-stu-id="c39a9-118">Handling Concurrency</span></span>](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="map-inheritance-to-database"></a><span data-ttu-id="c39a9-119">Dziedziczenie mapowania do bazy danych</span><span class="sxs-lookup"><span data-stu-id="c39a9-119">Map inheritance to database</span></span>

<span data-ttu-id="c39a9-120">Klasy `Instructor` i `Student` w modelu danych `School` mają różne właściwości, które są identyczne:</span><span class="sxs-lookup"><span data-stu-id="c39a9-120">The `Instructor` and `Student` classes in the `School` data model have several properties that are identical:</span></span>

![Student_and_Instructor_classes](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

<span data-ttu-id="c39a9-122">Załóżmy, że chcesz wyeliminować nadmiarowy kod dla właściwości, które są współużytkowane przez `Instructor` i `Student` jednostek.</span><span class="sxs-lookup"><span data-stu-id="c39a9-122">Suppose you want to eliminate the redundant code for the properties that are shared by the `Instructor` and `Student` entities.</span></span> <span data-ttu-id="c39a9-123">Lub chcesz napisać usługę, która może formatować nazwy bez Caring, niezależnie od tego, czy nazwa pochodzi od instruktora, czy studenta.</span><span class="sxs-lookup"><span data-stu-id="c39a9-123">Or you want to write a service that can format names without caring whether the name came from an instructor or a student.</span></span> <span data-ttu-id="c39a9-124">Można utworzyć `Person` klasę bazową, która zawiera tylko te właściwości udostępnione, a następnie uczynić jednostki `Instructor` i `Student` dziedziczone z tej klasy bazowej, jak pokazano na poniższej ilustracji:</span><span class="sxs-lookup"><span data-stu-id="c39a9-124">You could create a `Person` base class which contains only those shared properties, then make the `Instructor` and `Student` entities inherit from that base class, as shown in the following illustration:</span></span>

![Student_and_Instructor_classes_deriving_from_Person_class](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

<span data-ttu-id="c39a9-126">Istnieje kilka sposobów reprezentowania tej struktury dziedziczenia w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="c39a9-126">There are several ways this inheritance structure could be represented in the database.</span></span> <span data-ttu-id="c39a9-127">Możesz mieć `Person` tabelę zawierającą informacje o studentach i instruktorach w pojedynczej tabeli.</span><span class="sxs-lookup"><span data-stu-id="c39a9-127">You could have a `Person` table that includes information about both students and instructors in a single table.</span></span> <span data-ttu-id="c39a9-128">Niektóre kolumny mogą dotyczyć tylko instruktorów (`HireDate`), tylko dla studentów (`EnrollmentDate`), niektórych do obu (`LastName`, `FirstName`).</span><span class="sxs-lookup"><span data-stu-id="c39a9-128">Some of the columns could apply only to instructors (`HireDate`), some only to students (`EnrollmentDate`), some to both (`LastName`, `FirstName`).</span></span> <span data-ttu-id="c39a9-129">Zazwyczaj masz kolumnę *rozróżniacza* , aby wskazać, który typ reprezentuje każdy wiersz.</span><span class="sxs-lookup"><span data-stu-id="c39a9-129">Typically, you'd have a *discriminator* column to indicate which type each row represents.</span></span> <span data-ttu-id="c39a9-130">Na przykład kolumna rozróżniacza może mieć "instruktor" dla instruktorów i "Student" dla studentów.</span><span class="sxs-lookup"><span data-stu-id="c39a9-130">For example, the discriminator column might have "Instructor" for instructors and "Student" for students.</span></span>

![Tabela na hierarchy_example](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

<span data-ttu-id="c39a9-132">Ten wzorzec generowania struktury dziedziczenia jednostek z pojedynczej tabeli bazy danych jest nazywany dziedziczeniem *na hierarchię* (TPH).</span><span class="sxs-lookup"><span data-stu-id="c39a9-132">This pattern of generating an entity inheritance structure from a single database table is called *table-per-hierarchy* (TPH) inheritance.</span></span>

<span data-ttu-id="c39a9-133">Alternatywą jest, aby baza danych wyglądała podobnie jak struktura dziedziczenia.</span><span class="sxs-lookup"><span data-stu-id="c39a9-133">An alternative is to make the database look more like the inheritance structure.</span></span> <span data-ttu-id="c39a9-134">Na przykład można mieć tylko pola nazw w tabeli `Person` i mieć osobne tabele `Instructor` i `Student` z polami daty.</span><span class="sxs-lookup"><span data-stu-id="c39a9-134">For example, you could have only the name fields in the `Person` table and have separate `Instructor` and `Student` tables with the date fields.</span></span>

![Tabela na type_inheritance](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

<span data-ttu-id="c39a9-136">Ten wzorzec tworzenia tabeli bazy danych dla każdej klasy jednostek jest nazywany dziedziczeniem *tabeli na typ* (TPT).</span><span class="sxs-lookup"><span data-stu-id="c39a9-136">This pattern of making a database table for each entity class is called *table per type* (TPT) inheritance.</span></span>

<span data-ttu-id="c39a9-137">Jeszcze kolejną opcją jest zamapowanie wszystkich typów nieabstrakcyjnych na poszczególne tabele.</span><span class="sxs-lookup"><span data-stu-id="c39a9-137">Yet another option is to map all non-abstract types to individual tables.</span></span> <span data-ttu-id="c39a9-138">Wszystkie właściwości klasy, łącznie z dziedziczonymi właściwościami, mapują do kolumn odpowiedniej tabeli.</span><span class="sxs-lookup"><span data-stu-id="c39a9-138">All properties of a class, including inherited properties, map to columns of the corresponding table.</span></span> <span data-ttu-id="c39a9-139">Ten wzorzec jest nazywany dziedziczeniem klasy opartej na tabeli (TPC).</span><span class="sxs-lookup"><span data-stu-id="c39a9-139">This pattern is called Table-per-Concrete Class (TPC) inheritance.</span></span> <span data-ttu-id="c39a9-140">W przypadku zaimplementowania dziedziczenia TPC dla klas `Person`, `Student`i `Instructor` jak pokazano wcześniej, tabele `Student` i `Instructor` nie będą wyglądały inaczej po wdrożeniu dziedziczenia niż wcześniej.</span><span class="sxs-lookup"><span data-stu-id="c39a9-140">If you implemented TPC inheritance for the `Person`, `Student`, and `Instructor` classes as shown earlier, the `Student` and `Instructor` tables would look no different after implementing inheritance than they did before.</span></span>

<span data-ttu-id="c39a9-141">Wzorce TPHu i dziedziczenia są ogólnie zapewniające lepszą wydajność w Entity Framework niż wzorce dziedziczenia TPT, ponieważ wzorce TPT mogą powodować złożone zapytania sprzężenia.</span><span class="sxs-lookup"><span data-stu-id="c39a9-141">TPC and TPH inheritance patterns generally deliver better performance in the Entity Framework than TPT inheritance patterns, because TPT patterns can result in complex join queries.</span></span>

<span data-ttu-id="c39a9-142">W tym samouczku pokazano, jak wdrożyć dziedziczenie TPH.</span><span class="sxs-lookup"><span data-stu-id="c39a9-142">This tutorial demonstrates how to implement TPH inheritance.</span></span> <span data-ttu-id="c39a9-143">TPH jest domyślnym wzorcem dziedziczenia w Entity Framework, więc wszystko, co trzeba zrobić, to utworzenie klasy `Person`, zmiana klas `Instructor` i `Student` na podstawie `Person`, dodanie nowej klasy do `DbContext`i utworzenie migracji.</span><span class="sxs-lookup"><span data-stu-id="c39a9-143">TPH is the default inheritance pattern in the Entity Framework, so all you have to do is create a `Person` class, change the `Instructor` and `Student` classes to derive from `Person`, add the new class to the `DbContext`, and create a migration.</span></span> <span data-ttu-id="c39a9-144">(Aby uzyskać informacje na temat implementowania innych wzorców dziedziczenia, zobacz [Mapowanie dziedziczenia według typu tabeli (TPT)](https://msdn.microsoft.com/data/jj591617#2.5) i [Mapowanie dziedziczenia klasy opartej na tabeli (TPC)](https://msdn.microsoft.com/data/jj591617#2.6) w dokumentacji MSDN Entity Framework.)</span><span class="sxs-lookup"><span data-stu-id="c39a9-144">(For information about how to implement the other inheritance patterns, see [Mapping the Table-Per-Type (TPT) Inheritance](https://msdn.microsoft.com/data/jj591617#2.5) and [Mapping the Table-Per-Concrete Class (TPC) Inheritance](https://msdn.microsoft.com/data/jj591617#2.6) in the MSDN Entity Framework documentation.)</span></span>

## <a name="create-the-person-class"></a><span data-ttu-id="c39a9-145">Tworzenie klasy Person</span><span class="sxs-lookup"><span data-stu-id="c39a9-145">Create the Person class</span></span>

<span data-ttu-id="c39a9-146">W folderze *modele* Utwórz *Person.cs* i Zastąp kod szablonu następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="c39a9-146">In the *Models* folder, create *Person.cs* and replace the template code with the following code:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

## <a name="update-instructor-and-student"></a><span data-ttu-id="c39a9-147">Aktualizuj instruktora i uczniów</span><span class="sxs-lookup"><span data-stu-id="c39a9-147">Update Instructor and Student</span></span>

<span data-ttu-id="c39a9-148">Teraz zaktualizuj *Instructor.cs* i *student.cs* , aby dziedziczyły wartości z *Person.SC*.</span><span class="sxs-lookup"><span data-stu-id="c39a9-148">Now update the *Instructor.cs* and *Student.cs* to inherit values from the *Person.sc*.</span></span>

<span data-ttu-id="c39a9-149">W *Instructor.cs*należy utworzyć klasę `Instructor` z klasy `Person` i usunąć pola klucza i nazwy.</span><span class="sxs-lookup"><span data-stu-id="c39a9-149">In *Instructor.cs*, derive the `Instructor` class from the `Person` class and remove the key and name fields.</span></span> <span data-ttu-id="c39a9-150">Kod będzie wyglądać podobnie do poniższego przykładu:</span><span class="sxs-lookup"><span data-stu-id="c39a9-150">The code will look like the following example:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

<span data-ttu-id="c39a9-151">Wprowadź podobne zmiany w *student.cs*.</span><span class="sxs-lookup"><span data-stu-id="c39a9-151">Make similar changes to *Student.cs*.</span></span> <span data-ttu-id="c39a9-152">Klasa `Student` będzie wyglądać podobnie do poniższego przykładu:</span><span class="sxs-lookup"><span data-stu-id="c39a9-152">The `Student` class will look like the following example:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="add-person-to-the-model"></a><span data-ttu-id="c39a9-153">Dodawanie osoby do modelu</span><span class="sxs-lookup"><span data-stu-id="c39a9-153">Add Person to the Model</span></span>

<span data-ttu-id="c39a9-154">W *SchoolContext.cs*dodaj Właściwość `DbSet` dla typu jednostki `Person`:</span><span class="sxs-lookup"><span data-stu-id="c39a9-154">In *SchoolContext.cs*, add a `DbSet` property for the `Person` entity type:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

<span data-ttu-id="c39a9-155">To wszystko, co Entity Framework potrzebuje w celu skonfigurowania dziedziczenia w hierarchii na poziomie tabeli.</span><span class="sxs-lookup"><span data-stu-id="c39a9-155">This is all that the Entity Framework needs in order to configure table-per-hierarchy inheritance.</span></span> <span data-ttu-id="c39a9-156">Jak zobaczysz, gdy baza danych zostanie zaktualizowana, będzie miała `Person` tabeli zamiast tabel `Student` i `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="c39a9-156">As you'll see, when the database is updated, it will have a `Person` table in place of the `Student` and `Instructor` tables.</span></span>

## <a name="create-and-update-migrations"></a><span data-ttu-id="c39a9-157">Tworzenie i aktualizowanie migracji</span><span class="sxs-lookup"><span data-stu-id="c39a9-157">Create and Update Migrations</span></span>

<span data-ttu-id="c39a9-158">W konsoli Menedżera pakietów (PMC) wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="c39a9-158">In the Package Manager Console (PMC), enter the following command:</span></span>

`Add-Migration Inheritance`

<span data-ttu-id="c39a9-159">Uruchom `Update-Database` polecenie w obszarze PMC.</span><span class="sxs-lookup"><span data-stu-id="c39a9-159">Run the `Update-Database` command in the PMC.</span></span> <span data-ttu-id="c39a9-160">Polecenie zakończy się niepowodzeniem w tym momencie, ponieważ mamy istniejące dane, które nie wiedzą, jak obsłużyć migracje.</span><span class="sxs-lookup"><span data-stu-id="c39a9-160">The command will fail at this point because we have existing data that migrations doesn't know how to handle.</span></span> <span data-ttu-id="c39a9-161">Zostanie wyświetlony komunikat o błędzie podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="c39a9-161">You get an error message like the following one:</span></span>

> <span data-ttu-id="c39a9-162">*Nie można porzucić obiektu "dbo. Instruktora, ponieważ odwołuje się do niego ograniczenie FOREIGN KEY.*</span><span class="sxs-lookup"><span data-stu-id="c39a9-162">*Could not drop object 'dbo.Instructor' because it is referenced by a FOREIGN KEY constraint.*</span></span>

<span data-ttu-id="c39a9-163">Otwórz *migracje\&lt; sygnatura czasowa&gt;\_Inheritance.cs* i Zastąp metodę `Up` następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="c39a9-163">Open *Migrations\&lt;timestamp&gt;\_Inheritance.cs* and replace the `Up` method with the following code:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

<span data-ttu-id="c39a9-164">Ten kod obejmuje następujące zadania aktualizacji bazy danych:</span><span class="sxs-lookup"><span data-stu-id="c39a9-164">This code takes care of the following database update tasks:</span></span>

- <span data-ttu-id="c39a9-165">Usuwa ograniczenia klucza obcego i indeksy wskazujące na tabelę uczniów.</span><span class="sxs-lookup"><span data-stu-id="c39a9-165">Removes foreign key constraints and indexes that point to the Student table.</span></span>
- <span data-ttu-id="c39a9-166">Zmienia nazwę tabeli instruktora jako osobę i wprowadza zmiany, które są niezbędne do przechowywania danych ucznia:</span><span class="sxs-lookup"><span data-stu-id="c39a9-166">Renames the Instructor table as Person and makes changes needed for it to store Student data:</span></span>

    - <span data-ttu-id="c39a9-167">Dodaje wartość null EnrollmentDate dla studentów.</span><span class="sxs-lookup"><span data-stu-id="c39a9-167">Adds nullable EnrollmentDate for students.</span></span>
    - <span data-ttu-id="c39a9-168">Dodaje kolumnę rozróżniacza, aby wskazać, czy wiersz dotyczy studenta, czy instruktora.</span><span class="sxs-lookup"><span data-stu-id="c39a9-168">Adds Discriminator column to indicate whether a row is for a student or an instructor.</span></span>
    - <span data-ttu-id="c39a9-169">Sprawia, że HireDate dopuszcza wartość null, ponieważ wiersze uczniów nie mają dat zatrudnienia.</span><span class="sxs-lookup"><span data-stu-id="c39a9-169">Makes HireDate nullable since student rows won't have hire dates.</span></span>
    - <span data-ttu-id="c39a9-170">Dodaje pole tymczasowe, które będzie używane do aktualizacji kluczy obcych, które wskazują uczniów.</span><span class="sxs-lookup"><span data-stu-id="c39a9-170">Adds a temporary field that will be used to update foreign keys that point to students.</span></span> <span data-ttu-id="c39a9-171">Podczas kopiowania uczniów do tabeli osób otrzymają nowe wartości klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="c39a9-171">When you copy students into the Person table they'll get new primary key values.</span></span>
- <span data-ttu-id="c39a9-172">Kopiuje dane z tabeli uczniów do tabeli Person.</span><span class="sxs-lookup"><span data-stu-id="c39a9-172">Copies data from the Student table into the Person table.</span></span> <span data-ttu-id="c39a9-173">Powoduje to, że uczniowie mogą uzyskać przypisane nowe wartości klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="c39a9-173">This causes students to get assigned new primary key values.</span></span>
- <span data-ttu-id="c39a9-174">Naprawia wartości kluczy obcych, które wskazują uczniów.</span><span class="sxs-lookup"><span data-stu-id="c39a9-174">Fixes foreign key values that point to students.</span></span>
- <span data-ttu-id="c39a9-175">Ponownie tworzy ograniczenia klucza obcego i indeksy, teraz wskazując je do tabeli Person.</span><span class="sxs-lookup"><span data-stu-id="c39a9-175">Re-creates foreign key constraints and indexes, now pointing them to the Person table.</span></span>

<span data-ttu-id="c39a9-176">(Jeśli jako typ klucza podstawowego użyto identyfikatora GUID zamiast Integer, wartości klucza podstawowego studenta nie będą musiały ulec zmianie, a niektóre z tych kroków mogły zostać pominięte).</span><span class="sxs-lookup"><span data-stu-id="c39a9-176">(If you had used GUID instead of integer as the primary key type, the student primary key values wouldn't have to change, and several of these steps could have been omitted.)</span></span>

<span data-ttu-id="c39a9-177">Uruchom ponownie polecenie `update-database`.</span><span class="sxs-lookup"><span data-stu-id="c39a9-177">Run the `update-database` command again.</span></span>

<span data-ttu-id="c39a9-178">(W systemie produkcyjnym należy wprowadzić odpowiednie zmiany w metodzie Down w przypadku, gdy kiedykolwiek było konieczne użycie tego programu w celu powrotu do poprzedniej wersji bazy danych.</span><span class="sxs-lookup"><span data-stu-id="c39a9-178">(In a production system you would make corresponding changes to the Down method in case you ever had to use that to go back to the previous database version.</span></span> <span data-ttu-id="c39a9-179">W tym samouczku nie będziesz używać metody Down.)</span><span class="sxs-lookup"><span data-stu-id="c39a9-179">For this tutorial you won't be using the Down method.)</span></span>

> [!NOTE]
> <span data-ttu-id="c39a9-180">Podczas migrowania danych i wprowadzania zmian w schemacie można uzyskać inne błędy.</span><span class="sxs-lookup"><span data-stu-id="c39a9-180">It's possible to get other errors when migrating data and making schema changes.</span></span> <span data-ttu-id="c39a9-181">Jeśli wystąpią błędy migracji, nie można rozwiązać tego problemu, możesz kontynuować pracę z samouczkiem, zmieniając parametry połączenia w pliku *Web. config* lub usuwając bazę danych.</span><span class="sxs-lookup"><span data-stu-id="c39a9-181">If you get migration errors you can't resolve, you can continue with the tutorial by changing the connection string in the *Web.config* file or by deleting the database.</span></span> <span data-ttu-id="c39a9-182">Najprostszym podejściem jest zmiana nazwy bazy danych w pliku *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="c39a9-182">The simplest approach is to rename the database in the *Web.config* file.</span></span> <span data-ttu-id="c39a9-183">Na przykład zmień nazwę bazy danych na ContosoUniversity2, jak pokazano w następującym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="c39a9-183">For example, change the database name to ContosoUniversity2 as shown in the following example:</span></span>
>
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.xml?highlight=2)]
>
> <span data-ttu-id="c39a9-184">W przypadku nowej bazy danych nie ma żadnych danych do migrowania, a `update-database` polecenie jest znacznie bardziej prawdopodobnie wykonane bez błędów.</span><span class="sxs-lookup"><span data-stu-id="c39a9-184">With a new database, there is no data to migrate, and the `update-database` command is much more likely to complete without errors.</span></span> <span data-ttu-id="c39a9-185">Aby uzyskać instrukcje dotyczące sposobu usuwania bazy danych, zobacz [Jak usunąć bazę danych z programu Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/).</span><span class="sxs-lookup"><span data-stu-id="c39a9-185">For instructions on how to delete the database, see [How to Drop a Database from Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/).</span></span> <span data-ttu-id="c39a9-186">Jeśli zostanie to zastosowane w celu kontynuowania samouczka, pomiń krok wdrożenia na końcu tego samouczka lub Wdróż go w nowej lokacji i bazie danych.</span><span class="sxs-lookup"><span data-stu-id="c39a9-186">If you take this approach in order to continue with the tutorial, skip the deployment step at the end of this tutorial or deploy to a new site and database.</span></span> <span data-ttu-id="c39a9-187">Jeśli aktualizacja została wdrożona w tej samej lokacji, która została już wdrożona, program Dr pobierze ten sam błąd podczas automatycznego uruchamiania migracji.</span><span class="sxs-lookup"><span data-stu-id="c39a9-187">If you deploy an update to the same site you've been deploying to already, EF will get the same error there when it runs migrations automatically.</span></span> <span data-ttu-id="c39a9-188">Jeśli chcesz rozwiązać problem z błędami migracji, najlepszym zasobem jest jedno z forów Entity Framework lub StackOverflow.com.</span><span class="sxs-lookup"><span data-stu-id="c39a9-188">If you want to troubleshoot a migrations error, the best resource is one of the Entity Framework forums or StackOverflow.com.</span></span>

## <a name="test-the-implementation"></a><span data-ttu-id="c39a9-189">Testowanie implementacji</span><span class="sxs-lookup"><span data-stu-id="c39a9-189">Test the implementation</span></span>

<span data-ttu-id="c39a9-190">Uruchom witrynę i wypróbuj różne strony.</span><span class="sxs-lookup"><span data-stu-id="c39a9-190">Run the site and try various pages.</span></span> <span data-ttu-id="c39a9-191">Wszystko działa tak samo jak wcześniej.</span><span class="sxs-lookup"><span data-stu-id="c39a9-191">Everything works the same as it did before.</span></span>

<span data-ttu-id="c39a9-192">W **Eksplorator serwera** rozwiń węzeł **Data Connections\SchoolContext** , a następnie pozycję **tabele**i sprawdź, czy tabele **student** i **instruktor** zostały zastąpione przez tabelę **Person** .</span><span class="sxs-lookup"><span data-stu-id="c39a9-192">In **Server Explorer,** expand **Data Connections\SchoolContext** and then **Tables**, and you see that the **Student** and **Instructor** tables have been replaced by a **Person** table.</span></span> <span data-ttu-id="c39a9-193">Rozwiń tabelę **Person** i zobaczysz, że zawiera ona wszystkie kolumny, które były używane do znajdowania w tabelach **studenta** i **instruktora** .</span><span class="sxs-lookup"><span data-stu-id="c39a9-193">Expand the **Person** table and you see that it has all of the columns that used to be in the **Student** and **Instructor** tables.</span></span>

<span data-ttu-id="c39a9-194">Kliknij prawym przyciskiem myszy tabelę osoba, a następnie kliknij polecenie **Pokaż dane tabeli** , aby wyświetlić kolumnę rozróżniacza.</span><span class="sxs-lookup"><span data-stu-id="c39a9-194">Right-click the Person table, and then click **Show Table Data** to see the discriminator column.</span></span>

<span data-ttu-id="c39a9-195">Na poniższym diagramie przedstawiono strukturę nowej szkolnej bazy danych:</span><span class="sxs-lookup"><span data-stu-id="c39a9-195">The following diagram illustrates the structure of the new School database:</span></span>

![School_database_diagram](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

## <a name="deploy-to-azure"></a><span data-ttu-id="c39a9-197">Wdrażanie na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="c39a9-197">Deploy to Azure</span></span>

<span data-ttu-id="c39a9-198">Ta sekcja wymaga wykonania opcjonalnej sekcji **wdrażanie aplikacji do platformy Azure** w [części 3, sortowanie, filtrowanie i stronicowanie](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md) tej serii samouczków.</span><span class="sxs-lookup"><span data-stu-id="c39a9-198">This section requires you to have completed the optional **Deploying the app to Azure** section in [Part 3, Sorting, Filtering, and Paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md) of this tutorial series.</span></span> <span data-ttu-id="c39a9-199">W przypadku migracji błędów, które zostały rozwiązane przez usunięcie bazy danych w projekcie lokalnym, Pomiń ten krok; Utwórz nową witrynę i bazę danych i Wdróż ją w nowym środowisku.</span><span class="sxs-lookup"><span data-stu-id="c39a9-199">If you had migrations errors that you resolved by deleting the database in your local project, skip this step; or create a new site and database, and deploy to the new environment.</span></span>

1. <span data-ttu-id="c39a9-200">W programie Visual Studio kliknij prawym przyciskiem myszy projekt w **Eksplorator rozwiązań** i wybierz polecenie **Publikuj** z menu kontekstowego.</span><span class="sxs-lookup"><span data-stu-id="c39a9-200">In Visual Studio, right-click the project in **Solution Explorer** and select **Publish** from the context menu.</span></span>

2. <span data-ttu-id="c39a9-201">Kliknij przycisk **Opublikuj**.</span><span class="sxs-lookup"><span data-stu-id="c39a9-201">Click **Publish**.</span></span>

    <span data-ttu-id="c39a9-202">Aplikacja sieci Web zostanie otwarta w domyślnej przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="c39a9-202">The Web app opens in your default browser.</span></span>

3. <span data-ttu-id="c39a9-203">Przetestuj aplikację, aby upewnić się, że działa.</span><span class="sxs-lookup"><span data-stu-id="c39a9-203">Test the application to verify it's working.</span></span>

    <span data-ttu-id="c39a9-204">Przy pierwszym uruchomieniu strony, która uzyskuje dostęp do bazy danych, Entity Framework uruchamia wszystkie migracje `Up` metody wymagane w celu przeprowadzenia Aktualności bazy danych z bieżącym modelem danych.</span><span class="sxs-lookup"><span data-stu-id="c39a9-204">The first time you run a page that accesses the database, the Entity Framework runs all of the migrations `Up` methods required to bring the database up to date with the current data model.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="c39a9-205">Uzyskiwanie kodu</span><span class="sxs-lookup"><span data-stu-id="c39a9-205">Get the code</span></span>

[<span data-ttu-id="c39a9-206">Pobierz ukończony projekt</span><span class="sxs-lookup"><span data-stu-id="c39a9-206">Download Completed Project</span></span>](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a><span data-ttu-id="c39a9-207">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="c39a9-207">Additional resources</span></span>

<span data-ttu-id="c39a9-208">Linki do innych zasobów Entity Framework można znaleźć w [zasobach zalecanych przez dostęp do danych ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="c39a9-208">Links to other Entity Framework resources can be found in the [ASP.NET Data Access - Recommended Resources](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

<span data-ttu-id="c39a9-209">Aby uzyskać więcej informacji na temat tej i innych struktur dziedziczenia, zobacz [wzorzec dziedziczenia TPT](https://msdn.microsoft.com/data/jj618293) oraz [wzorzec dziedziczenia TPH](https://msdn.microsoft.com/data/jj618292) w witrynie MSDN.</span><span class="sxs-lookup"><span data-stu-id="c39a9-209">For more information about this and other inheritance structures, see [TPT Inheritance Pattern](https://msdn.microsoft.com/data/jj618293) and [TPH Inheritance Pattern](https://msdn.microsoft.com/data/jj618292) on MSDN.</span></span> <span data-ttu-id="c39a9-210">W następnym samouczku zobaczysz, jak obsłużyć wiele relatywnie zaawansowanych scenariuszy Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="c39a9-210">In the next tutorial you'll see how to handle a variety of relatively advanced Entity Framework scenarios.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c39a9-211">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="c39a9-211">Next steps</span></span>

<span data-ttu-id="c39a9-212">W tym samouczku zostaną wykonane następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="c39a9-212">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c39a9-213">Zapoznaj się z mapowaniem dziedziczenia do bazy danych</span><span class="sxs-lookup"><span data-stu-id="c39a9-213">Learned to map inheritance to database</span></span>
> * <span data-ttu-id="c39a9-214">Utworzono klasę Person</span><span class="sxs-lookup"><span data-stu-id="c39a9-214">Created the Person class</span></span>
> * <span data-ttu-id="c39a9-215">Zaktualizowany instruktor i student</span><span class="sxs-lookup"><span data-stu-id="c39a9-215">Updated Instructor and Student</span></span>
> * <span data-ttu-id="c39a9-216">Dodano osobę do modelu</span><span class="sxs-lookup"><span data-stu-id="c39a9-216">Added Person to the Model</span></span>
> * <span data-ttu-id="c39a9-217">Tworzenie i aktualizowanie migracji</span><span class="sxs-lookup"><span data-stu-id="c39a9-217">Created and Update Migrations</span></span>
> * <span data-ttu-id="c39a9-218">Przetestowano implementację</span><span class="sxs-lookup"><span data-stu-id="c39a9-218">Tested the implementation</span></span>
> * <span data-ttu-id="c39a9-219">Wdrożone na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="c39a9-219">Deployed to Azure</span></span>

<span data-ttu-id="c39a9-220">Przejdź do następnego artykułu, aby dowiedzieć się więcej na temat tematów, które są przydatne, gdy przejdziesz poza podstawowe informacje na temat tworzenia aplikacji sieci Web ASP.NET korzystających z Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="c39a9-220">Advance to the next article to learn about topics that are useful to be aware of when you go beyond the basics of developing ASP.NET web applications that use Entity Framework Code First.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="c39a9-221">Zaawansowane scenariusze platformy Entity Framework</span><span class="sxs-lookup"><span data-stu-id="c39a9-221">Advanced Entity Framework Scenarios</span></span>](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)

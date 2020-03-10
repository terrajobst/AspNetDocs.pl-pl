---
uid: mvc/overview/getting-started/database-first-development/changing-the-database
title: 'Samouczek: zmiana bazy danych dla Database First EF z aplikacją ASP.NET MVC'
description: Ten samouczek koncentruje się na wprowadzaniu aktualizacji struktury bazy danych i propagowaniu zmian w całej aplikacji sieci Web.
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: cfd5c083-a319-482e-8f25-5b38caa93954
msc.legacyurl: /mvc/overview/getting-started/database-first-development/changing-the-database
msc.type: authoredcontent
ms.openlocfilehash: 52cad1120908cf0d4f85770f8e2690f9415c5f56
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78616264"
---
# <a name="tutorial-change-the-database-for-ef-database-first-with-aspnet-mvc-app"></a><span data-ttu-id="955a4-103">Samouczek: zmiana bazy danych dla Database First EF z aplikacją ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="955a4-103">Tutorial: Change the database for EF Database First with ASP.NET MVC app</span></span>

<span data-ttu-id="955a4-104">Korzystając z szkieletów MVC, Entity Framework i ASP.NET, można utworzyć aplikację sieci Web, która udostępnia interfejs istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="955a4-104">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="955a4-105">W tej serii samouczków pokazano, jak automatycznie generować kod, który umożliwia użytkownikom wyświetlanie, edytowanie, tworzenie i usuwanie danych znajdujących się w tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="955a4-105">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="955a4-106">Wygenerowany kod odpowiada kolumnom w tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="955a4-106">The generated code corresponds to the columns in the database table.</span></span>

<span data-ttu-id="955a4-107">Ten samouczek koncentruje się na wprowadzaniu aktualizacji struktury bazy danych i propagowaniu zmian w całej aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="955a4-107">This tutorial focuses on making an update to the database structure and propagating that change throughout the web application.</span></span>

<span data-ttu-id="955a4-108">W tym samouczku zostaną wykonane następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="955a4-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="955a4-109">Dodawanie kolumny</span><span class="sxs-lookup"><span data-stu-id="955a4-109">Add a column</span></span>
> * <span data-ttu-id="955a4-110">Dodawanie właściwości do widoków</span><span class="sxs-lookup"><span data-stu-id="955a4-110">Add the property to the views</span></span>

## <a name="prerequisites"></a><span data-ttu-id="955a4-111">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="955a4-111">Prerequisites</span></span>

* [<span data-ttu-id="955a4-112">Generowanie widoków</span><span class="sxs-lookup"><span data-stu-id="955a4-112">Generating views</span></span>](generating-views.md)

## <a name="add-a-column"></a><span data-ttu-id="955a4-113">Dodawanie kolumny</span><span class="sxs-lookup"><span data-stu-id="955a4-113">Add a column</span></span>

<span data-ttu-id="955a4-114">W przypadku aktualizowania struktury tabeli w bazie danych należy upewnić się, że zmiana jest propagowana do modelu danych, widoków i kontrolera.</span><span class="sxs-lookup"><span data-stu-id="955a4-114">If you update the structure of a table in your database, you need to ensure that your change is propagated to the data model, views, and controller.</span></span>

<span data-ttu-id="955a4-115">W tym samouczku dodasz nową kolumnę do tabeli Students, aby zarejestrować nazwę środkową ucznia.</span><span class="sxs-lookup"><span data-stu-id="955a4-115">For this tutorial, you will add a new column to the Student table to record the middle name of the student.</span></span> <span data-ttu-id="955a4-116">Aby dodać tę kolumnę, Otwórz projekt bazy danych i Otwórz plik student. SQL.</span><span class="sxs-lookup"><span data-stu-id="955a4-116">To add this column, open the database project, and open the Student.sql file.</span></span> <span data-ttu-id="955a4-117">Za pomocą projektanta lub kodu T-SQL Dodaj kolumnę o nazwie **MiddleName** , która jest nvarchar (50) i dopuszcza wartości null.</span><span class="sxs-lookup"><span data-stu-id="955a4-117">Through either the designer or the T-SQL code, add a column named **MiddleName** that is an NVARCHAR(50) and allows NULL values.</span></span>

<span data-ttu-id="955a4-118">Wdróż tę zmianę w lokalnej bazie danych, uruchamiając projekt bazy danych (lub F5).</span><span class="sxs-lookup"><span data-stu-id="955a4-118">Deploy this change to your local database by starting your database project (or F5).</span></span> <span data-ttu-id="955a4-119">Nowe pole zostanie dodane do tabeli.</span><span class="sxs-lookup"><span data-stu-id="955a4-119">The new field is added to the table.</span></span> <span data-ttu-id="955a4-120">Jeśli nie widzisz go w Eksplorator obiektów SQL Server, kliknij przycisk Odśwież w okienku.</span><span class="sxs-lookup"><span data-stu-id="955a4-120">If you do not see it in the SQL Server Object Explorer, click the Refresh button in the pane.</span></span>

![Pokaż nową kolumnę](changing-the-database/_static/image2.png)

<span data-ttu-id="955a4-122">Nowa kolumna istnieje w tabeli bazy danych, ale nie istnieje obecnie w klasie modelu danych.</span><span class="sxs-lookup"><span data-stu-id="955a4-122">The new column exists in the database table, but it does not currently exist in the data model class.</span></span> <span data-ttu-id="955a4-123">Musisz zaktualizować model, aby dołączyć nową kolumnę.</span><span class="sxs-lookup"><span data-stu-id="955a4-123">You must update the model to include your new column.</span></span> <span data-ttu-id="955a4-124">W folderze **modele** Otwórz plik **ContosoModel. edmx** , aby wyświetlić Diagram modelu.</span><span class="sxs-lookup"><span data-stu-id="955a4-124">In the **Models** folder, open the **ContosoModel.edmx** file to display the model diagram.</span></span> <span data-ttu-id="955a4-125">Zwróć uwagę, że model ucznia nie zawiera właściwości MiddleName.</span><span class="sxs-lookup"><span data-stu-id="955a4-125">Notice that the Student model does not contain the MiddleName property.</span></span> <span data-ttu-id="955a4-126">Kliknij prawym przyciskiem myszy w dowolnym miejscu na powierzchni projektowej, a następnie wybierz polecenie **Aktualizuj model z bazy danych**.</span><span class="sxs-lookup"><span data-stu-id="955a4-126">Right-click anywhere on the design surface, and select **Update Model from Database**.</span></span>

<span data-ttu-id="955a4-127">W Kreatorze aktualizacji wybierz kartę **Odśwież** , a następnie wybierz pozycję **tabele** > **dbo** > **student**.</span><span class="sxs-lookup"><span data-stu-id="955a4-127">In the Update Wizard, select the **Refresh** tab and then select **Tables** > **dbo** > **Student**.</span></span> <span data-ttu-id="955a4-128">Kliknij przycisk **Finish** (Zakończ).</span><span class="sxs-lookup"><span data-stu-id="955a4-128">Click **Finish**.</span></span>

<span data-ttu-id="955a4-129">Po zakończeniu procesu aktualizacji diagram bazy danych zawiera nową właściwość **MiddleName** .</span><span class="sxs-lookup"><span data-stu-id="955a4-129">After the update process is finished, the database diagram includes the new **MiddleName** property.</span></span> <span data-ttu-id="955a4-130">Zapisz plik **ContosoModel. edmx** .</span><span class="sxs-lookup"><span data-stu-id="955a4-130">Save the **ContosoModel.edmx** file.</span></span> <span data-ttu-id="955a4-131">Musisz zapisać ten plik, aby uzyskać nową właściwość do propagowania do klasy **student.cs** .</span><span class="sxs-lookup"><span data-stu-id="955a4-131">You must save this file for the new property to be propagated to the **Student.cs** class.</span></span> <span data-ttu-id="955a4-132">Baza danych i model zostały zaktualizowane.</span><span class="sxs-lookup"><span data-stu-id="955a4-132">You have now updated the database and the model.</span></span>

<span data-ttu-id="955a4-133">Skompiluj rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="955a4-133">Build the solution.</span></span>

## <a name="add-the-property-to-the-views"></a><span data-ttu-id="955a4-134">Dodawanie właściwości do widoków</span><span class="sxs-lookup"><span data-stu-id="955a4-134">Add the property to the views</span></span>

<span data-ttu-id="955a4-135">Niestety, widoki nadal nie zawierają nowej właściwości.</span><span class="sxs-lookup"><span data-stu-id="955a4-135">Unfortunately, the views still do not contain the new property.</span></span> <span data-ttu-id="955a4-136">Aby zaktualizować widoki, które są dostępne, możesz ponownie wygenerować widoki przez ponowne dodanie szkieletu dla klasy uczniów lub można ręcznie dodać nową właściwość do istniejących widoków.</span><span class="sxs-lookup"><span data-stu-id="955a4-136">To update the views you have two options - you can either re-generate the views by once again adding scaffolding for the Student class, or you can manually add the new property to your existing views.</span></span> <span data-ttu-id="955a4-137">W tym samouczku dodasz szkielet ponownie, ponieważ nie wprowadzono żadnych niestandardowych zmian w widokach generowanych automatycznie.</span><span class="sxs-lookup"><span data-stu-id="955a4-137">In this tutorial, you will add the scaffolding again because you have not made any customized changes to the automatically-generated views.</span></span> <span data-ttu-id="955a4-138">Możesz rozważyć ręczne dodanie właściwości po wprowadzeniu zmian w widokach i nie chcesz utracić tych zmian.</span><span class="sxs-lookup"><span data-stu-id="955a4-138">You might consider manually adding the property when you have made changes to the views and do not want to lose those changes.</span></span>

<span data-ttu-id="955a4-139">Aby upewnić się, że widoki są odtwarzane, Usuń folder **uczniów** w obszarze **widoki**i Usuń **StudentsController**.</span><span class="sxs-lookup"><span data-stu-id="955a4-139">To ensure the views are re-created, delete the **Students** folder under **Views**, and delete the **StudentsController**.</span></span> <span data-ttu-id="955a4-140">Następnie kliknij prawym przyciskiem myszy folder **controllers** , a następnie Dodaj szkielet dla modelu **ucznia** .</span><span class="sxs-lookup"><span data-stu-id="955a4-140">Then, right-click the **Controllers** folder and add scaffolding for the **Student** model.</span></span> <span data-ttu-id="955a4-141">Ponownie Nazwij kontroler **StudentsController**.</span><span class="sxs-lookup"><span data-stu-id="955a4-141">Again, name the controller **StudentsController**.</span></span> <span data-ttu-id="955a4-142">Wybierz pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="955a4-142">Select **Add**.</span></span>

<span data-ttu-id="955a4-143">Ponownie skompiluj rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="955a4-143">Build the solution again.</span></span> <span data-ttu-id="955a4-144">Widoki zawierają teraz Właściwość MiddleName.</span><span class="sxs-lookup"><span data-stu-id="955a4-144">The views now contain the MiddleName property.</span></span>

![Pokaż drugie imię](changing-the-database/_static/image5.png)

## <a name="next-steps"></a><span data-ttu-id="955a4-146">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="955a4-146">Next steps</span></span>

<span data-ttu-id="955a4-147">W tym samouczku zostaną wykonane następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="955a4-147">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="955a4-148">Dodano kolumnę</span><span class="sxs-lookup"><span data-stu-id="955a4-148">Added a column</span></span>
> * <span data-ttu-id="955a4-149">Dodano właściwość do widoków</span><span class="sxs-lookup"><span data-stu-id="955a4-149">Added the property to the views</span></span>

<span data-ttu-id="955a4-150">Przejdź do następnego samouczka, aby dowiedzieć się, jak dostosować widok na potrzeby wyświetlania szczegółowych informacji o rekordzie ucznia.</span><span class="sxs-lookup"><span data-stu-id="955a4-150">Advance to the next tutorial to learn how to customize the view for showing details about a student record.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="955a4-151">Dostosowywanie widoku</span><span class="sxs-lookup"><span data-stu-id="955a4-151">Customize a view</span></span>](customizing-a-view.md)
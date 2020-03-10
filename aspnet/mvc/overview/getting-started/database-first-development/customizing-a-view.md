---
uid: mvc/overview/getting-started/database-first-development/customizing-a-view
title: 'Samouczek: Dostosowywanie widoku dla Database First EF z aplikacją ASP.NET MVC'
description: Ten samouczek koncentruje się na zmianie automatycznie generowanych widoków w celu ulepszenia prezentacji.
author: Rick-Anderson
ms.author: riande
ms.date: 01/24/2019
ms.topic: tutorial
ms.assetid: 269380ff-d7e1-4035-8ad1-fe1316a25f76
msc.legacyurl: /mvc/overview/getting-started/database-first-development/customizing-a-view
msc.type: authoredcontent
ms.openlocfilehash: 89b8a0eb84b6e287c45bc141c68a2c76e63b0e41
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78583595"
---
# <a name="tutorial-customize-view-for-ef-database-first-with-aspnet-mvc-app"></a><span data-ttu-id="90c85-103">Samouczek: Dostosowywanie widoku dla Database First EF z aplikacją ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="90c85-103">Tutorial: Customize view for EF Database First with ASP.NET MVC app</span></span>

<span data-ttu-id="90c85-104">Korzystając z szkieletów MVC, Entity Framework i ASP.NET, można utworzyć aplikację sieci Web, która udostępnia interfejs istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="90c85-104">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="90c85-105">W tej serii samouczków pokazano, jak automatycznie generować kod, który umożliwia użytkownikom wyświetlanie, edytowanie, tworzenie i usuwanie danych znajdujących się w tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="90c85-105">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="90c85-106">Wygenerowany kod odpowiada kolumnom w tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="90c85-106">The generated code corresponds to the columns in the database table.</span></span>

<span data-ttu-id="90c85-107">Ten samouczek koncentruje się na zmianie automatycznie generowanych widoków w celu ulepszenia prezentacji.</span><span class="sxs-lookup"><span data-stu-id="90c85-107">This tutorial focuses on changing the automatically-generated views to enhance the presentation.</span></span>

<span data-ttu-id="90c85-108">W tym samouczku zostaną wykonane następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="90c85-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="90c85-109">Dodawanie kursów do strony szczegółów ucznia</span><span class="sxs-lookup"><span data-stu-id="90c85-109">Add courses to the student detail page</span></span>
> * <span data-ttu-id="90c85-110">Potwierdź, że kursy są dodawane do strony</span><span class="sxs-lookup"><span data-stu-id="90c85-110">Confirm that the courses are added to the page</span></span>

## <a name="prerequisites"></a><span data-ttu-id="90c85-111">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="90c85-111">Prerequisites</span></span>

* [<span data-ttu-id="90c85-112">Zmiana bazy danych</span><span class="sxs-lookup"><span data-stu-id="90c85-112">Change the database</span></span>](changing-the-database.md)

## <a name="add-courses-to-student-detail"></a><span data-ttu-id="90c85-113">Dodawanie kursów do szczegółów ucznia</span><span class="sxs-lookup"><span data-stu-id="90c85-113">Add courses to student detail</span></span>

<span data-ttu-id="90c85-114">Wygenerowany kod stanowi dobry punkt wyjścia dla aplikacji, ale nie musi zapewniać wszystkich funkcji potrzebnych w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="90c85-114">The generated code provides a good starting point for your application but it does not necessarily provide all of the functionality that you need in your application.</span></span> <span data-ttu-id="90c85-115">Można dostosować kod w celu spełnienia określonych wymagań aplikacji.</span><span class="sxs-lookup"><span data-stu-id="90c85-115">You can customize the code to meet the particular requirements of your application.</span></span> <span data-ttu-id="90c85-116">Obecnie aplikacja nie wyświetla zarejestrowanych kursów dla wybranego ucznia.</span><span class="sxs-lookup"><span data-stu-id="90c85-116">Currently, your application does not display the enrolled courses for the selected student.</span></span> <span data-ttu-id="90c85-117">W tej sekcji dodasz zarejestrowane kursy dla każdego ucznia do widoku **szczegółów** dla ucznia.</span><span class="sxs-lookup"><span data-stu-id="90c85-117">In this section, you will add the enrolled courses for each student to the **Details** view for the student.</span></span>

<span data-ttu-id="90c85-118">Otwórz **widoki** > **uczniów** > *szczegóły. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="90c85-118">Open **Views** > **Students** > *Details.cshtml*.</span></span> <span data-ttu-id="90c85-119">Poniżej ostatniego znacznika &lt;/DL&gt;, ale przed zamykającym tagiem &lt;/DIV&gt; Dodaj następujący kod.</span><span class="sxs-lookup"><span data-stu-id="90c85-119">Below the last &lt;/dl&gt; tag, but before the closing &lt;/div&gt; tag, add the following code.</span></span>

[!code-cshtml[Main](customizing-a-view/samples/sample1.cshtml)]

<span data-ttu-id="90c85-120">Ten kod tworzy tabelę, która wyświetla wiersz dla każdego rekordu w tabeli rejestracji dla wybranego ucznia.</span><span class="sxs-lookup"><span data-stu-id="90c85-120">This code creates a table that displays a row for each record in the Enrollment table for the selected student.</span></span> <span data-ttu-id="90c85-121">Metoda **Display** renderuje HTML dla obiektu (ModelItem), który reprezentuje wyrażenie.</span><span class="sxs-lookup"><span data-stu-id="90c85-121">The **Display** method renders HTML for the object (modelItem) that represents the expression.</span></span> <span data-ttu-id="90c85-122">Aby upewnić się, że wartość jest poprawnie sformatowana w oparciu o typ i szablon tego typu, należy użyć metody wyświetlania (zamiast osadzania wartości właściwości w kodzie).</span><span class="sxs-lookup"><span data-stu-id="90c85-122">You use the Display method (rather than simply embedding the property value in the code) to make sure the value is formatted correctly based on its type and the template for that type.</span></span> <span data-ttu-id="90c85-123">W tym przykładzie każde wyrażenie zwraca pojedynczą właściwość z bieżącego rekordu w pętli, a wartości są typami pierwotnymi, które są renderowane jako tekst.</span><span class="sxs-lookup"><span data-stu-id="90c85-123">In this example, each expression returns a single property from the current record in the loop, and the values are primitive types which are rendered as text.</span></span>

## <a name="confirm-courses-are-added"></a><span data-ttu-id="90c85-124">Dodano kursy potwierdzające</span><span class="sxs-lookup"><span data-stu-id="90c85-124">Confirm courses are added</span></span>

<span data-ttu-id="90c85-125">Uruchom rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="90c85-125">Run the solution.</span></span> <span data-ttu-id="90c85-126">Kliknij **listę studentów** i wybierz **szczegóły** dla jednego z uczniów.</span><span class="sxs-lookup"><span data-stu-id="90c85-126">Click **List of students** and select **Details** for one of the students.</span></span> <span data-ttu-id="90c85-127">W widoku zostaną wyświetlone zarejestrowane kursy.</span><span class="sxs-lookup"><span data-stu-id="90c85-127">You will see the enrolled courses have been included in the view.</span></span>

![student z rejestracją](customizing-a-view/_static/image1.png)

## <a name="next-steps"></a><span data-ttu-id="90c85-129">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="90c85-129">Next steps</span></span>
<span data-ttu-id="90c85-130">W tym samouczku zostaną wykonane następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="90c85-130">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="90c85-131">Dodano kursy do strony szczegółów ucznia</span><span class="sxs-lookup"><span data-stu-id="90c85-131">Added courses to the student detail page</span></span>
> * <span data-ttu-id="90c85-132">Potwierdzenie, że kursy są dodawane do strony</span><span class="sxs-lookup"><span data-stu-id="90c85-132">Confirmed that the courses are added to the page</span></span>

<span data-ttu-id="90c85-133">Przejdź do następnego samouczka, aby dowiedzieć się, jak dodać adnotacje do danych, aby określić wymagania dotyczące weryfikacji i formatowanie wyświetlania.</span><span class="sxs-lookup"><span data-stu-id="90c85-133">Advance to the next tutorial to learn how to add data annotations to specify validation requirements and display formatting.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="90c85-134">Ulepszanie walidacji danych</span><span class="sxs-lookup"><span data-stu-id="90c85-134">Enhance data validation</span></span>](enhancing-data-validation.md)

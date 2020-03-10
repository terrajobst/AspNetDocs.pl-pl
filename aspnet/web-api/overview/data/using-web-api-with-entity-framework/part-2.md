---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-2
title: Dodawanie modeli i kontrolerów | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 88908ff8-51a9-40eb-931c-a8139128b680
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-2
msc.type: authoredcontent
ms.openlocfilehash: 57dacda421968f341284d89c9a3ad80040c16e25
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557541"
---
# <a name="add-models-and-controllers"></a><span data-ttu-id="8d00c-102">Dodawanie modeli i kontrolerów</span><span class="sxs-lookup"><span data-stu-id="8d00c-102">Add Models and Controllers</span></span>

<span data-ttu-id="8d00c-103">według [Jan Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="8d00c-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="8d00c-104">Pobierz ukończony projekt</span><span class="sxs-lookup"><span data-stu-id="8d00c-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="8d00c-105">W tej sekcji dodasz klasy modelu, które definiują jednostki bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8d00c-105">In this section, you will add model classes that define the database entities.</span></span> <span data-ttu-id="8d00c-106">Następnie dodasz Kontrolery interfejsu API sieci Web, które wykonują operacje CRUD na tych jednostkach.</span><span class="sxs-lookup"><span data-stu-id="8d00c-106">Then you will add Web API controllers that perform CRUD operations on those entities.</span></span>

## <a name="add-model-classes"></a><span data-ttu-id="8d00c-107">Dodaj klasy modelu</span><span class="sxs-lookup"><span data-stu-id="8d00c-107">Add Model Classes</span></span>

<span data-ttu-id="8d00c-108">W tym samouczku utworzymy bazę danych przy użyciu podejścia "Code First" do Entity Framework (EF).</span><span class="sxs-lookup"><span data-stu-id="8d00c-108">In this tutorial, we'll create the database by using the "Code First" approach to Entity Framework (EF).</span></span> <span data-ttu-id="8d00c-109">Za pomocą Code First można pisać C# klasy, które odpowiadają tabelom bazy danych, a EF tworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="8d00c-109">With Code First, you write C# classes that correspond to database tables, and EF creates the database.</span></span> <span data-ttu-id="8d00c-110">(Aby uzyskać więcej informacji, zobacz [Entity Framework podejścia programistyczne](https://msdn.microsoft.com/library/ms178359%28v=vs.110%29.aspx#dbfmfcf).)</span><span class="sxs-lookup"><span data-stu-id="8d00c-110">(For more information, see [Entity Framework Development Approaches](https://msdn.microsoft.com/library/ms178359%28v=vs.110%29.aspx#dbfmfcf).)</span></span>

<span data-ttu-id="8d00c-111">Zaczynamy od definiowania obiektów domeny jako POCOs (zwykłych obiektów CLR).</span><span class="sxs-lookup"><span data-stu-id="8d00c-111">We start by defining our domain objects as POCOs (plain-old CLR objects).</span></span> <span data-ttu-id="8d00c-112">Utworzymy następujący POCOs:</span><span class="sxs-lookup"><span data-stu-id="8d00c-112">We will create the following POCOs:</span></span>

- <span data-ttu-id="8d00c-113">Autor</span><span class="sxs-lookup"><span data-stu-id="8d00c-113">Author</span></span>
- <span data-ttu-id="8d00c-114">Book (Książka)</span><span class="sxs-lookup"><span data-stu-id="8d00c-114">Book</span></span>

<span data-ttu-id="8d00c-115">W Eksplorator rozwiązań kliknij prawym przyciskiem myszy folder modele.</span><span class="sxs-lookup"><span data-stu-id="8d00c-115">In Solution Explorer, right click the Models folder.</span></span> <span data-ttu-id="8d00c-116">Wybierz pozycję **Dodaj**, a następnie wybierz pozycję **Klasa**.</span><span class="sxs-lookup"><span data-stu-id="8d00c-116">Select **Add**, then select **Class**.</span></span> <span data-ttu-id="8d00c-117">Nadaj klasie nazwę `Author`.</span><span class="sxs-lookup"><span data-stu-id="8d00c-117">Name the class `Author`.</span></span>

![](part-2/_static/image1.png)

<span data-ttu-id="8d00c-118">Zastąp cały kod standardowy w Author.cs następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="8d00c-118">Replace all of the boilerplate code in Author.cs with the following code.</span></span>

[!code-csharp[Main](part-2/samples/sample1.cs)]

<span data-ttu-id="8d00c-119">Dodaj inną klasę o nazwie `Book`, używając poniższego kodu.</span><span class="sxs-lookup"><span data-stu-id="8d00c-119">Add another class named `Book`, with the following code.</span></span>

[!code-csharp[Main](part-2/samples/sample2.cs)]

<span data-ttu-id="8d00c-120">Entity Framework będą używać tych modeli do tworzenia tabel bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8d00c-120">Entity Framework will use these models to create database tables.</span></span> <span data-ttu-id="8d00c-121">Dla każdego modelu Właściwość `Id` stanie się kolumną klucza podstawowego w tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8d00c-121">For each model, the `Id` property will become the primary key column of the database table.</span></span>

<span data-ttu-id="8d00c-122">W klasie Book `AuthorId` definiuje klucz obcy do tabeli `Author`.</span><span class="sxs-lookup"><span data-stu-id="8d00c-122">In the Book class, the `AuthorId` defines a foreign key into the `Author` table.</span></span> <span data-ttu-id="8d00c-123">(Dla uproszczenia przyjęto, że każda książka ma jednego autora). Klasa Book zawiera również właściwość nawigacji do powiązanej `Author`.</span><span class="sxs-lookup"><span data-stu-id="8d00c-123">(For simplicity, I'm assuming that each book has a single author.) The book class also contains a navigation property to the related `Author`.</span></span> <span data-ttu-id="8d00c-124">Możesz użyć właściwości nawigacji, aby uzyskać dostęp do powiązanego `Author` w kodzie.</span><span class="sxs-lookup"><span data-stu-id="8d00c-124">You can use the navigation property to access the related `Author` in code.</span></span> <span data-ttu-id="8d00c-125">Dowiesz się więcej o właściwościach nawigacji w części 4, [Obsługa relacji jednostek](part-4.md).</span><span class="sxs-lookup"><span data-stu-id="8d00c-125">I say more about navigation properties in part 4, [Handling Entity Relations](part-4.md).</span></span>

## <a name="add-web-api-controllers"></a><span data-ttu-id="8d00c-126">Dodawanie kontrolerów interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="8d00c-126">Add Web API Controllers</span></span>

<span data-ttu-id="8d00c-127">W tej sekcji dodamy Kontrolery interfejsu API sieci Web, które obsługują operacje CRUD (tworzenie, odczytywanie, aktualizowanie i usuwanie).</span><span class="sxs-lookup"><span data-stu-id="8d00c-127">In this section, we'll add Web API controllers that support CRUD operations (create, read, update, and delete).</span></span> <span data-ttu-id="8d00c-128">Kontrolery będą używać Entity Framework do komunikowania się z warstwą bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8d00c-128">The controllers will use Entity Framework to communicate with the database layer.</span></span>

<span data-ttu-id="8d00c-129">Najpierw można usunąć kontrolery plików/ValuesController. cs.</span><span class="sxs-lookup"><span data-stu-id="8d00c-129">First, you can delete the file Controllers/ValuesController.cs.</span></span> <span data-ttu-id="8d00c-130">Ten plik zawiera przykładowy kontroler internetowego interfejsu API, ale nie jest potrzebny do tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="8d00c-130">This file contains an example Web API controller, but you don't need it for this tutorial.</span></span>

![](part-2/_static/image2.png)

<span data-ttu-id="8d00c-131">Następnie Skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="8d00c-131">Next, build the project.</span></span> <span data-ttu-id="8d00c-132">Tworzenie szkieletu interfejsu API sieci Web używa odbicia w celu znalezienia klas modelu, więc wymaga skompilowanego zestawu.</span><span class="sxs-lookup"><span data-stu-id="8d00c-132">The Web API scaffolding uses reflection to find the model classes, so it needs the compiled assembly.</span></span>

<span data-ttu-id="8d00c-133">W Eksplorator rozwiązań kliknij prawym przyciskiem myszy folder controllers.</span><span class="sxs-lookup"><span data-stu-id="8d00c-133">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="8d00c-134">Wybierz pozycję **Dodaj**, a następnie pozycję **kontroler**.</span><span class="sxs-lookup"><span data-stu-id="8d00c-134">Select **Add**, then select **Controller**.</span></span>

![](part-2/_static/image3.png)

<span data-ttu-id="8d00c-135">W oknie dialogowym **Dodawanie szkieletu** wybierz opcję "kontroler Web API 2 z akcjami przy użyciu Entity Framework".</span><span class="sxs-lookup"><span data-stu-id="8d00c-135">In the **Add Scaffold** dialog, select "Web API 2 Controller with actions, using Entity Framework".</span></span> <span data-ttu-id="8d00c-136">Kliknij pozycję **Add** (Dodaj).</span><span class="sxs-lookup"><span data-stu-id="8d00c-136">Click **Add**.</span></span>

![](part-2/_static/image4.png)

<span data-ttu-id="8d00c-137">W oknie dialogowym **Dodawanie kontrolera** wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="8d00c-137">In the **Add Controller** dialog, do the following:</span></span>

1. <span data-ttu-id="8d00c-138">Z listy rozwijanej **Klasa modelu** wybierz klasę `Author`.</span><span class="sxs-lookup"><span data-stu-id="8d00c-138">In the **Model class** dropdown, select the `Author` class.</span></span> <span data-ttu-id="8d00c-139">(Jeśli nie widzisz go na liście rozwijanej, upewnij się, że został skompilowany projekt).</span><span class="sxs-lookup"><span data-stu-id="8d00c-139">(If you don't see it listed in the dropdown, make sure that you built the project.)</span></span>
2. <span data-ttu-id="8d00c-140">Zaznacz opcję "Użyj akcji kontrolera asynchronicznego".</span><span class="sxs-lookup"><span data-stu-id="8d00c-140">Check "Use async controller actions".</span></span>
3. <span data-ttu-id="8d00c-141">Pozostaw nazwę kontrolera jako &quot;AuthorsController&quot;.</span><span class="sxs-lookup"><span data-stu-id="8d00c-141">Leave the controller name as &quot;AuthorsController&quot;.</span></span>
4. <span data-ttu-id="8d00c-142">Kliknij przycisk Plus (+) obok **klasy kontekstu danych**.</span><span class="sxs-lookup"><span data-stu-id="8d00c-142">Click plus (+) button next to **Data Context Class**.</span></span>

![](part-2/_static/image5.png)

<span data-ttu-id="8d00c-143">W oknie dialogowym **nowy kontekst danych** pozostaw nazwę domyślną, a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="8d00c-143">In the **New Data Context** dialog, leave the default name and click **Add**.</span></span>

![](part-2/_static/image6.png)

<span data-ttu-id="8d00c-144">Kliknij przycisk **Dodaj** , aby zakończyć okno dialogowe **Dodawanie kontrolera** .</span><span class="sxs-lookup"><span data-stu-id="8d00c-144">Click **Add** to complete the **Add Controller** dialog.</span></span> <span data-ttu-id="8d00c-145">To okno dialogowe dodaje dwie klasy do projektu:</span><span class="sxs-lookup"><span data-stu-id="8d00c-145">The dialog adds two classes to your project:</span></span>

- <span data-ttu-id="8d00c-146">`AuthorsController` definiuje kontroler interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="8d00c-146">`AuthorsController` defines a Web API controller.</span></span> <span data-ttu-id="8d00c-147">Kontroler implementuje interfejs API REST używany przez klientów do wykonywania operacji CRUD na liście autorów.</span><span class="sxs-lookup"><span data-stu-id="8d00c-147">The controller implements the REST API that clients use to perform CRUD operations on the list of authors.</span></span>
- <span data-ttu-id="8d00c-148">`BookServiceContext` zarządza obiektami jednostek w czasie wykonywania, co obejmuje wypełnianie obiektów danymi z bazy danych, śledzenie zmian i utrwalanie danych w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="8d00c-148">`BookServiceContext` manages entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span> <span data-ttu-id="8d00c-149">Dziedziczy po `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="8d00c-149">It inherits from `DbContext`.</span></span>

![](part-2/_static/image7.png)

<span data-ttu-id="8d00c-150">W tym momencie Skompiluj projekt ponownie.</span><span class="sxs-lookup"><span data-stu-id="8d00c-150">At this point, build the project again.</span></span> <span data-ttu-id="8d00c-151">Teraz wykonaj te same kroki, aby dodać kontroler interfejsu API dla jednostek `Book`.</span><span class="sxs-lookup"><span data-stu-id="8d00c-151">Now go through the same steps to add an API controller for `Book` entities.</span></span> <span data-ttu-id="8d00c-152">Tym razem wybierz `Book` klasy model i wybierz istniejącą klasę `BookServiceContext` dla klasy kontekstu danych.</span><span class="sxs-lookup"><span data-stu-id="8d00c-152">This time, select `Book` for the model class, and select the existing `BookServiceContext` class for the data context class.</span></span> <span data-ttu-id="8d00c-153">(Nie twórz nowego kontekstu danych). Kliknij przycisk **Dodaj** , aby dodać kontroler.</span><span class="sxs-lookup"><span data-stu-id="8d00c-153">(Don't create a new data context.) Click **Add** to add the controller.</span></span>

![](part-2/_static/image8.png)

> [!div class="step-by-step"]
> <span data-ttu-id="8d00c-154">[Poprzednie](part-1.md)
> [dalej](part-3.md)</span><span class="sxs-lookup"><span data-stu-id="8d00c-154">[Previous](part-1.md)
[Next](part-3.md)</span></span>

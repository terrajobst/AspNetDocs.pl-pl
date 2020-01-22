---
uid: mvc/overview/getting-started/introduction/adding-a-model
title: Dodawanie modelu | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 05/28/2015
ms.assetid: 276552b5-f349-4fcf-8f40-6d042f7aa88e
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: 0d926c7a8bd99c56820208921c10e609da56d236
ms.sourcegitcommit: 88fc80e3f65aebdf61ec9414810ddbc31c543f04
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/22/2020
ms.locfileid: "76519040"
---
# <a name="adding-a-model"></a><span data-ttu-id="85935-102">Dodawanie modelu</span><span class="sxs-lookup"><span data-stu-id="85935-102">Adding a Model</span></span>

<span data-ttu-id="85935-103">Autor [Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="85935-103">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

[!INCLUDE [Tutorial Note](index.md)]

<span data-ttu-id="85935-104">W tej sekcji dodasz kilka klas do zarządzania filmami w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="85935-104">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="85935-105">Te klasy będą modelem &quot;&quot; częścią aplikacji ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="85935-105">These classes will be the &quot;model&quot; part of the ASP.NET MVC app.</span></span>

<span data-ttu-id="85935-106">Będziesz używać .NET Framework technologii dostępu do danych, znanej jako [Entity Framework](https://docs.microsoft.com/ef/) do definiowania i pracy z tymi klasami modelu.</span><span class="sxs-lookup"><span data-stu-id="85935-106">You'll use a .NET Framework data-access technology known as the [Entity Framework](https://docs.microsoft.com/ef/) to define and work with these model classes.</span></span> <span data-ttu-id="85935-107">Entity Framework (często określana jako EF) obsługuje model programistyczny o nazwie *Code First*.</span><span class="sxs-lookup"><span data-stu-id="85935-107">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="85935-108">Code First umożliwia tworzenie obiektów modelu przez pisanie prostych klas.</span><span class="sxs-lookup"><span data-stu-id="85935-108">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="85935-109">(Są one również znane jako klasy POCO, od &quot;zwykłych obiektów CLR.&quot;) Następnie można utworzyć bazę danych na bieżąco od klas, co umożliwia bardzo czysty i szybki przepływ pracy programistycznej.</span><span class="sxs-lookup"><span data-stu-id="85935-109">(These are also known as POCO classes, from &quot;plain-old CLR objects.&quot;) You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span> <span data-ttu-id="85935-110">Jeśli musisz najpierw utworzyć bazę danych, możesz skorzystać z tego samouczka, aby dowiedzieć się więcej na temat programowania aplikacji MVC i EF.</span><span class="sxs-lookup"><span data-stu-id="85935-110">If you are required to create the database first, you can still follow this tutorial to learn about MVC and EF app development.</span></span> <span data-ttu-id="85935-111">Następnie można postępować zgodnie z samouczkiem tworzenie [szkieletu Fizmakens ASP.NET](xref:visual-studio/overview/2013/aspnet-scaffolding-overview) , który obejmuje pierwsze podejście do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="85935-111">You can then follow Tom Fizmakens [ASP.NET Scaffolding](xref:visual-studio/overview/2013/aspnet-scaffolding-overview) tutorial, which covers the database first approach.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="85935-112">Dodawanie klas modelu</span><span class="sxs-lookup"><span data-stu-id="85935-112">Adding Model Classes</span></span>

<span data-ttu-id="85935-113">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy folder *modele* , wybierz polecenie **Dodaj**, a następnie wybierz pozycję **Klasa**.</span><span class="sxs-lookup"><span data-stu-id="85935-113">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="85935-114">Wprowadź nazwę *klasy* &quot;filmu&quot;.</span><span class="sxs-lookup"><span data-stu-id="85935-114">Enter the *class* name &quot;Movie&quot;.</span></span>

<span data-ttu-id="85935-115">Dodaj następujące pięć właściwości do klasy `Movie`:</span><span class="sxs-lookup"><span data-stu-id="85935-115">Add the following five properties to the `Movie` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

<span data-ttu-id="85935-116">Użyjemy klasy `Movie` do reprezentowania filmów w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="85935-116">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="85935-117">Każde wystąpienie obiektu `Movie` będzie odpowiadać wierszowi w tabeli bazy danych, a każda właściwość klasy `Movie` zostanie zamapowana na kolumnę w tabeli.</span><span class="sxs-lookup"><span data-stu-id="85935-117">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="85935-118">Uwaga: aby można było użyć klasy System. Data. Entity i powiązanej, należy zainstalować [pakiet NuGet Entity Framework](https://www.nuget.org/packages/EntityFramework/).</span><span class="sxs-lookup"><span data-stu-id="85935-118">Note: In order to use System.Data.Entity, and the related class, you need to install the [Entity Framework NuGet Package](https://www.nuget.org/packages/EntityFramework/).</span></span> <span data-ttu-id="85935-119">Aby uzyskać dalsze instrukcje, postępuj zgodnie z linkiem.</span><span class="sxs-lookup"><span data-stu-id="85935-119">Follow the link for further instructions.</span></span>

<span data-ttu-id="85935-120">W tym samym pliku Dodaj następującą `MovieDBContext` klasy:</span><span class="sxs-lookup"><span data-stu-id="85935-120">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample2.cs?highlight=2,15-18)]

<span data-ttu-id="85935-121">Klasa `MovieDBContext` reprezentuje kontekst bazy danych filmów Entity Framework, który obsługuje pobieranie, przechowywanie i aktualizowanie wystąpień klasy `Movie` w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="85935-121">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="85935-122">`MovieDBContext` pochodzi od klasy bazowej `DbContext` dostarczonej przez Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="85935-122">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span>

<span data-ttu-id="85935-123">Aby można było odwoływać się do `DbContext` i `DbSet`, należy dodać następującą instrukcję `using` na początku pliku:</span><span class="sxs-lookup"><span data-stu-id="85935-123">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `using` statement at the top of the file:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

<span data-ttu-id="85935-124">Można to zrobić przez ręczne dodanie instrukcji using lub umieszczenie wskaźnika myszy na czerwono falistej linii, kliknij przycisk `Show potential fixes` i kliknij przycisk `using System.Data.Entity;`</span><span class="sxs-lookup"><span data-stu-id="85935-124">You can do this by manually adding the using statement, or you can hover over the red squiggly lines, click `Show potential fixes` and click `using System.Data.Entity;`</span></span>

![](adding-a-model/_static/image2.png)

<span data-ttu-id="85935-125">Uwaga: Usunięto kilka nieużywanych instrukcji `using`.</span><span class="sxs-lookup"><span data-stu-id="85935-125">Note: Several unused `using` statements have been removed.</span></span> <span data-ttu-id="85935-126">Program Visual Studio będzie wyświetlał nieużywane zależności jako szare.</span><span class="sxs-lookup"><span data-stu-id="85935-126">Visual Studio will show unused dependencies as gray.</span></span> <span data-ttu-id="85935-127">Nieużywane zależności można usunąć, umieszczając kursor na szarych zależnościach, klikając `Show potential fixes` i klikając polecenie **Usuń nieużywane użycie.**</span><span class="sxs-lookup"><span data-stu-id="85935-127">You can remove unused dependencies by hovering over the gray dependencies, click `Show potential fixes` and click **Remove Unused Usings.**</span></span>

![](adding-a-model/_static/image3.png)

<span data-ttu-id="85935-128">Ostatecznie dodaliśmy model (M w MVC).</span><span class="sxs-lookup"><span data-stu-id="85935-128">We've finally added a model (the M in MVC).</span></span> <span data-ttu-id="85935-129">W następnej sekcji będziesz korzystać z parametrów połączenia z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="85935-129">In the next section you'll work with the database connection string.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="85935-130">[Poprzedni](adding-a-view.md)
> [Następny](creating-a-connection-string.md)</span><span class="sxs-lookup"><span data-stu-id="85935-130">[Previous](adding-a-view.md)
[Next](creating-a-connection-string.md)</span></span>

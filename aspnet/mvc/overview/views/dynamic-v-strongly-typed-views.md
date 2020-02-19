---
uid: mvc/overview/views/dynamic-v-strongly-typed-views
title: Dynamiczne a Widoki o jednoznacznie określonym typie | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/27/2011
ms.assetid: 0cbd88da-0da6-4605-b222-2835c6478304
msc.legacyurl: /mvc/overview/views/dynamic-v-strongly-typed-views
msc.type: authoredcontent
ms.openlocfilehash: 3e81c6381b1e280e3b74cb7eb6ea6e6c3224e655
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/19/2020
ms.locfileid: "77455676"
---
# <a name="dynamic-v-strongly-typed-views"></a><span data-ttu-id="17f94-103">Dynamiczne a</span><span class="sxs-lookup"><span data-stu-id="17f94-103">Dynamic v.</span></span> <span data-ttu-id="17f94-104">silnie typizowane widoki</span><span class="sxs-lookup"><span data-stu-id="17f94-104">Strongly Typed Views</span></span>

<span data-ttu-id="17f94-105">Autor [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="17f94-105">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="17f94-106">Istnieją trzy sposoby przekazywania informacji z kontrolera do widoku w ASP.NET MVC 3:</span><span class="sxs-lookup"><span data-stu-id="17f94-106">There are three ways to pass information from a controller to a view in ASP.NET MVC 3:</span></span>

1. <span data-ttu-id="17f94-107">Jako obiekt modelu o jednoznacznie określonym typie.</span><span class="sxs-lookup"><span data-stu-id="17f94-107">As a strongly typed model object.</span></span>
2. <span data-ttu-id="17f94-108">Jako typ dynamiczny (przy użyciu @model dynamiczny)</span><span class="sxs-lookup"><span data-stu-id="17f94-108">As a dynamic type (using @model dynamic)</span></span>
3. <span data-ttu-id="17f94-109">Korzystanie z ViewBag</span><span class="sxs-lookup"><span data-stu-id="17f94-109">Using the ViewBag</span></span>

<span data-ttu-id="17f94-110">Zapisałem prostą aplikację blogową MVC 3, która umożliwia porównanie i porównywanie dynamicznych i silnie wpisanych widoków.</span><span class="sxs-lookup"><span data-stu-id="17f94-110">I've written a simple MVC 3 Top Blog application to compare and contrast dynamic and strongly typed views.</span></span> <span data-ttu-id="17f94-111">Kontroler jest uruchamiany z prostą listą blogów:</span><span class="sxs-lookup"><span data-stu-id="17f94-111">The controller starts out with a simple list of blogs:</span></span>

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample1.cs)]

<span data-ttu-id="17f94-112">Kliknij prawym przyciskiem myszy metodę IndexNotStonglyTyped () i Dodaj widok Razor.</span><span class="sxs-lookup"><span data-stu-id="17f94-112">Right click in the IndexNotStonglyTyped() method and add a Razor view.</span></span>

<span data-ttu-id="17f94-113">[![8475. NotStronglyTypedView [1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="17f94-113">[![8475.NotStronglyTypedView[1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)</span></span>

<span data-ttu-id="17f94-114">Upewnij się, że pole wyboru **Utwórz jednoznacznie wpisaną** nie jest zaznaczone.</span><span class="sxs-lookup"><span data-stu-id="17f94-114">Make sure the **Create a strongly-typed view** box is not checked.</span></span> <span data-ttu-id="17f94-115">Widok wyników nie zawiera wiele:</span><span class="sxs-lookup"><span data-stu-id="17f94-115">The resulting view doesn't contain much:</span></span>

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample2.cshtml)]

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample3.cshtml)]

<span data-ttu-id="17f94-116">Ze względu na to, że używamy dynamicznego i nie widoku o jednoznacznie określonym typie, technologia IntelliSense nie pomoże nam.</span><span class="sxs-lookup"><span data-stu-id="17f94-116">Because we're using a dynamic and not a strongly typed view, intellisense doesn't help us.</span></span> <span data-ttu-id="17f94-117">Ukończony kod jest przedstawiony poniżej:</span><span class="sxs-lookup"><span data-stu-id="17f94-117">The completed code is shown below:</span></span>

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample4.cshtml)]

<span data-ttu-id="17f94-118">[![6646. NotStronglyTypedView_5F00_IE [1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="17f94-118">[![6646.NotStronglyTypedView_5F00_IE[1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)</span></span>

<span data-ttu-id="17f94-119">Teraz dodamy widok o jednoznacznie określonym typie.</span><span class="sxs-lookup"><span data-stu-id="17f94-119">Now we'll add a strongly typed view.</span></span> <span data-ttu-id="17f94-120">Dodaj następujący kod do kontrolera:</span><span class="sxs-lookup"><span data-stu-id="17f94-120">Add the following code to the controller:</span></span>

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample5.cs)]

<span data-ttu-id="17f94-121">Zwróć uwagę, że jest to dokładnie ten sam widok zwracany (topBlogs); Wywołaj jako widok niesilnie określony.</span><span class="sxs-lookup"><span data-stu-id="17f94-121">Notice it's exactly the same return View(topBlogs); call as the non-strongly typed view.</span></span> <span data-ttu-id="17f94-122">Kliknij prawym przyciskiem myszy wewnątrz *StonglyTypedIndex ()* i wybierz polecenie **Dodaj widok**.</span><span class="sxs-lookup"><span data-stu-id="17f94-122">Right click inside of *StonglyTypedIndex()* and select **Add View**.</span></span> <span data-ttu-id="17f94-123">Tym razem wybierz klasę model **bloga** i wybierz pozycję **Lista** jako szablon szkieletu.</span><span class="sxs-lookup"><span data-stu-id="17f94-123">This time select the **Blog** Model class and select **List** as the Scaffold template.</span></span>

<span data-ttu-id="17f94-124">[![5658. StrongView [1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="17f94-124">[![5658.StrongView[1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)</span></span>

<span data-ttu-id="17f94-125">W nowym szablonie widoku pobieramy obsługę technologii IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="17f94-125">Inside the new view template we get intellisense support.</span></span>

<span data-ttu-id="17f94-126">[![7002. IntelliSense [1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="17f94-126">[![7002.IntelliSense[1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)</span></span>

<span data-ttu-id="17f94-127">Projekt c# można pobrać [tutaj](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip).</span><span class="sxs-lookup"><span data-stu-id="17f94-127">The c# project can be downloaded [here](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip).</span></span>

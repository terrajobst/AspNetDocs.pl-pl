---
uid: mvc/overview/views/dynamic-v-strongly-typed-views
title: Dynamiczne a Silnie Typizowane widoki | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/27/2011
ms.assetid: 0cbd88da-0da6-4605-b222-2835c6478304
msc.legacyurl: /mvc/overview/views/dynamic-v-strongly-typed-views
msc.type: authoredcontent
ms.openlocfilehash: 3235fc58fbf93cb87946f8ebd4a478eff7ce80e3
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/09/2019
ms.locfileid: "59386141"
---
# <a name="dynamic-v-strongly-typed-views"></a><span data-ttu-id="23cbe-103">Dynamiczne a</span><span class="sxs-lookup"><span data-stu-id="23cbe-103">Dynamic v.</span></span> <span data-ttu-id="23cbe-104">silnie typizowane widoki</span><span class="sxs-lookup"><span data-stu-id="23cbe-104">Strongly Typed Views</span></span>

<span data-ttu-id="23cbe-105">Przez [Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="23cbe-105">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

<span data-ttu-id="23cbe-106">Istnieją trzy sposoby przekazywania informacji z kontrolera do widoku w ASP.NET MVC 3:</span><span class="sxs-lookup"><span data-stu-id="23cbe-106">There are three ways to pass information from a controller to a view in ASP.NET MVC 3:</span></span>

1. <span data-ttu-id="23cbe-107">Jako obiekt silnie typizowany model.</span><span class="sxs-lookup"><span data-stu-id="23cbe-107">As a strongly typed model object.</span></span>
2. <span data-ttu-id="23cbe-108">Jako typu dynamicznego (przy użyciu @model dynamiczny)</span><span class="sxs-lookup"><span data-stu-id="23cbe-108">As a dynamic type (using @model dynamic)</span></span>
3. <span data-ttu-id="23cbe-109">Za pomocą obiekt ViewBag</span><span class="sxs-lookup"><span data-stu-id="23cbe-109">Using the ViewBag</span></span>

<span data-ttu-id="23cbe-110">Moje recenzje prostą aplikację MVC 3 pierwszych blogu do Porównaj i zestaw widoki dynamiczne i silnie typizowane.</span><span class="sxs-lookup"><span data-stu-id="23cbe-110">I've written a simple MVC 3 Top Blog application to compare and contrast dynamic and strongly typed views.</span></span> <span data-ttu-id="23cbe-111">Kontroler rozpoczyna się od prostego listę blogów:</span><span class="sxs-lookup"><span data-stu-id="23cbe-111">The controller starts out with a simple list of blogs:</span></span>

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample1.cs)]

<span data-ttu-id="23cbe-112">Kliknij prawym przyciskiem myszy w metodzie IndexNotStonglyTyped() i Dodawanie widoku Razor.</span><span class="sxs-lookup"><span data-stu-id="23cbe-112">Right click in the IndexNotStonglyTyped() method and add a Razor view.</span></span>

[![8<span data-ttu-id="23cbe-113">475.NotStronglyTypedView[1]]</span><span class="sxs-lookup"><span data-stu-id="23cbe-113">475.NotStronglyTypedView[1]]</span></span>(dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)

<span data-ttu-id="23cbe-114">Upewnij się, że **utworzyć widok silnie typizowane** pole nie jest zaznaczone.</span><span class="sxs-lookup"><span data-stu-id="23cbe-114">Make sure the **Create a strongly-typed view** box is not checked.</span></span> <span data-ttu-id="23cbe-115">Wynikowym widoku nie zawiera wiele:</span><span class="sxs-lookup"><span data-stu-id="23cbe-115">The resulting view doesn't contain much:</span></span>

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample2.cshtml)]

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample3.cshtml)]

<span data-ttu-id="23cbe-116">Ponieważ używamy dynamiczne i silnie typizowanego widoku, funkcja intellisense nie pomagają nam.</span><span class="sxs-lookup"><span data-stu-id="23cbe-116">Because we're using a dynamic and not a strongly typed view, intellisense doesn't help us.</span></span> <span data-ttu-id="23cbe-117">Kompletny kod jest pokazany poniżej:</span><span class="sxs-lookup"><span data-stu-id="23cbe-117">The completed code is shown below:</span></span>

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample4.cshtml)]

[![6<span data-ttu-id="23cbe-118">646.NotStronglyTypedView_5F00_IE[1]]</span><span class="sxs-lookup"><span data-stu-id="23cbe-118">646.NotStronglyTypedView_5F00_IE[1]]</span></span>(dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)

<span data-ttu-id="23cbe-119">Teraz dodamy silnie typizowanego widoku.</span><span class="sxs-lookup"><span data-stu-id="23cbe-119">Now we'll add a strongly typed view.</span></span> <span data-ttu-id="23cbe-120">Dodaj następujący kod na kontrolerze:</span><span class="sxs-lookup"><span data-stu-id="23cbe-120">Add the following code to the controller:</span></span>

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample5.cs)]


<span data-ttu-id="23cbe-121">Zwróć uwagę, że jest dokładnie ten sam zwracany View(topBlogs); Wywołaj jako — silnie typizowanego widoku.</span><span class="sxs-lookup"><span data-stu-id="23cbe-121">Notice it's exactly the same return View(topBlogs); call as the non-strongly typed view.</span></span> <span data-ttu-id="23cbe-122">Kliknij prawym przyciskiem myszy wewnątrz *StonglyTypedIndex()* i wybierz **Dodaj widok**.</span><span class="sxs-lookup"><span data-stu-id="23cbe-122">Right click inside of *StonglyTypedIndex()* and select **Add View**.</span></span> <span data-ttu-id="23cbe-123">Tym razem wybierz pozycję **Blog** klasa modelu, a następnie wybierz pozycję **listy** jako szablon szkieletu.</span><span class="sxs-lookup"><span data-stu-id="23cbe-123">This time select the **Blog** Model class and select **List** as the Scaffold template.</span></span>

[![5<span data-ttu-id="23cbe-124">658.StrongView[1]]</span><span class="sxs-lookup"><span data-stu-id="23cbe-124">658.StrongView[1]]</span></span>(dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)

<span data-ttu-id="23cbe-125">Wewnątrz szablonu widoku uzyskujemy obsługę funkcji intellisense.</span><span class="sxs-lookup"><span data-stu-id="23cbe-125">Inside the new view template we get intellisense support.</span></span>

[![7<span data-ttu-id="23cbe-126">002.IntelliSense[1]]</span><span class="sxs-lookup"><span data-stu-id="23cbe-126">002.IntelliSense[1]]</span></span>(dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)

<span data-ttu-id="23cbe-127">Projekt c# można pobrać [tutaj](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip).</span><span class="sxs-lookup"><span data-stu-id="23cbe-127">The c# project can be downloaded [here](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip).</span></span>

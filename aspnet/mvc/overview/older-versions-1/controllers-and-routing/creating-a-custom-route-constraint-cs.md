---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-cs
title: Tworzenie niestandardowego ograniczenia trasy (C#) | Microsoft Docs
author: StephenWalther
description: Stephen Walther demonstruje, jak można utworzyć niestandardowe ograniczenie trasy. Implementujemy proste ograniczenie niestandardowe, które uniemożliwia dopasowanie trasy w...
ms.author: riande
ms.date: 02/16/2009
ms.assetid: a4f4bf4e-abcc-4650-8f43-527e48b52fe6
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-cs
msc.type: authoredcontent
ms.openlocfilehash: 98d5839e3d2623665770ccc5689c28f9eb29c8f6
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78601452"
---
# <a name="creating-a-custom-route-constraint-c"></a><span data-ttu-id="010f3-104">Tworzenie niestandardowego ograniczenia trasy (C#)</span><span class="sxs-lookup"><span data-stu-id="010f3-104">Creating a Custom Route Constraint (C#)</span></span>

<span data-ttu-id="010f3-105">Autor [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="010f3-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="010f3-106">Stephen Walther demonstruje, jak można utworzyć niestandardowe ograniczenie trasy.</span><span class="sxs-lookup"><span data-stu-id="010f3-106">Stephen Walther demonstrates how you can create a custom route constraint.</span></span> <span data-ttu-id="010f3-107">Implementujemy proste ograniczenie niestandardowe, które uniemożliwia dopasowanie trasy w przypadku żądania przeglądarki z komputera zdalnego.</span><span class="sxs-lookup"><span data-stu-id="010f3-107">We implement a simple custom constraint that prevents a route from being matched when a browser request is made from a remote computer.</span></span>

<span data-ttu-id="010f3-108">Celem tego samouczka jest zademonstrowanie sposobu tworzenia niestandardowego ograniczenia trasy.</span><span class="sxs-lookup"><span data-stu-id="010f3-108">The goal of this tutorial is to demonstrate how you can create a custom route constraint.</span></span> <span data-ttu-id="010f3-109">Ograniczenie trasy niestandardowej umożliwia uniemożliwienie dopasowania trasy, chyba że jest dopasowywany warunek niestandardowy.</span><span class="sxs-lookup"><span data-stu-id="010f3-109">A custom route constraint enables you to prevent a route from being matched unless some custom condition is matched.</span></span>

<span data-ttu-id="010f3-110">W tym samouczku utworzysz ograniczenie trasy hosta lokalnego.</span><span class="sxs-lookup"><span data-stu-id="010f3-110">In this tutorial, we create a Localhost route constraint.</span></span> <span data-ttu-id="010f3-111">Ograniczenie trasy hosta lokalnego odpowiada tylko na żądania wysyłane z komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="010f3-111">The Localhost route constraint only matches requests made from the local computer.</span></span> <span data-ttu-id="010f3-112">Żądania zdalne przesyłane przez Internet nie są zgodne.</span><span class="sxs-lookup"><span data-stu-id="010f3-112">Remote requests from across the Internet are not matched.</span></span>

<span data-ttu-id="010f3-113">Implementowanie niestandardowego ograniczenia trasy przez implementację interfejsu IRouteConstraint.</span><span class="sxs-lookup"><span data-stu-id="010f3-113">You implement a custom route constraint by implementing the IRouteConstraint interface.</span></span> <span data-ttu-id="010f3-114">Jest to bardzo prosty interfejs, który opisuje jedną metodę:</span><span class="sxs-lookup"><span data-stu-id="010f3-114">This is an extremely simple interface which describes a single method:</span></span>

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample1.cs)]

<span data-ttu-id="010f3-115">Metoda zwraca wartość logiczną.</span><span class="sxs-lookup"><span data-stu-id="010f3-115">The method returns a Boolean value.</span></span> <span data-ttu-id="010f3-116">Jeśli zwracasz wartość false, trasa skojarzona z ograniczeniem nie będzie zgodna z żądaniem przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="010f3-116">If you return false, the route associated with the constraint won't match the browser request.</span></span>

<span data-ttu-id="010f3-117">Ograniczenie localhost jest zawarte w liście 1.</span><span class="sxs-lookup"><span data-stu-id="010f3-117">The Localhost constraint is contained in Listing 1.</span></span>

<span data-ttu-id="010f3-118">**Lista 1 — LocalhostConstraint.cs**</span><span class="sxs-lookup"><span data-stu-id="010f3-118">**Listing 1 - LocalhostConstraint.cs**</span></span>

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample2.cs)]

<span data-ttu-id="010f3-119">Ograniczenie dotyczące listy 1 wykorzystuje Właściwość IsLocal uwidocznioną przez klasę HttpRequest.</span><span class="sxs-lookup"><span data-stu-id="010f3-119">The constraint in Listing 1 takes advantage of the IsLocal property exposed by the HttpRequest class.</span></span> <span data-ttu-id="010f3-120">Ta właściwość zwraca wartość PRAWDA, jeśli adres IP żądania to 127.0.0.1 lub gdy adres IP żądania jest taki sam, jak w przypadku adresu IP serwera.</span><span class="sxs-lookup"><span data-stu-id="010f3-120">This property returns true when the IP address of the request is either 127.0.0.1 or when the IP of the request is the same as the server's IP address.</span></span>

<span data-ttu-id="010f3-121">Używasz ograniczenia niestandardowego w ramach trasy zdefiniowanej w pliku Global. asax.</span><span class="sxs-lookup"><span data-stu-id="010f3-121">You use a custom constraint within a route defined in the Global.asax file.</span></span> <span data-ttu-id="010f3-122">Plik Global. asax na liście 2 używa ograniczenia localhost, aby uniemożliwić wszystkim użytkownikom żądanie strony administratora, chyba że zgłasza żądanie z serwera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="010f3-122">The Global.asax file in Listing 2 uses the Localhost constraint to prevent anyone from requesting an Admin page unless they make the request from the local server.</span></span> <span data-ttu-id="010f3-123">Na przykład żądanie/Admin/DeleteAll zakończy się niepowodzeniem, gdy zostanie wykonane z serwera zdalnego.</span><span class="sxs-lookup"><span data-stu-id="010f3-123">For example, a request for /Admin/DeleteAll will fail when made from a remote server.</span></span>

<span data-ttu-id="010f3-124">**Lista 2 — Global. asax**</span><span class="sxs-lookup"><span data-stu-id="010f3-124">**Listing 2 - Global.asax**</span></span>

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample3.cs)]

<span data-ttu-id="010f3-125">Ograniczenie localhost jest używane w definicji trasy administratora.</span><span class="sxs-lookup"><span data-stu-id="010f3-125">The Localhost constraint is used in the definition of the Admin route.</span></span> <span data-ttu-id="010f3-126">Ta trasa nie będzie zgodna z żądaniem przeglądarki zdalnej.</span><span class="sxs-lookup"><span data-stu-id="010f3-126">This route won't be matched by a remote browser request.</span></span> <span data-ttu-id="010f3-127">Należy jednak pamiętać, że inne trasy zdefiniowane w Global. asax mogą być zgodne z tym samym żądaniem.</span><span class="sxs-lookup"><span data-stu-id="010f3-127">Realize, however, that other routes defined in Global.asax might match the same request.</span></span> <span data-ttu-id="010f3-128">Ważne jest, aby zrozumieć, że ograniczenie uniemożliwia określonej trasie dopasowanie żądania i nie wszystkie trasy zdefiniowane w pliku Global. asax.</span><span class="sxs-lookup"><span data-stu-id="010f3-128">It is important to understand that a constraint prevents a particular route from matching a request and not all routes defined in the Global.asax file.</span></span>

<span data-ttu-id="010f3-129">Zwróć uwagę, że trasa domyślna została oznaczona jako komentarz z pliku Global. asax na liście 2.</span><span class="sxs-lookup"><span data-stu-id="010f3-129">Notice that the Default route has been commented out from the Global.asax file in Listing 2.</span></span> <span data-ttu-id="010f3-130">W przypadku uwzględnienia trasy domyślnej trasy domyślnej byłyby zgodne z żądaniami kontrolera administratora.</span><span class="sxs-lookup"><span data-stu-id="010f3-130">If you include the Default route, then the Default route would match requests for the Admin controller.</span></span> <span data-ttu-id="010f3-131">W takim przypadku użytkownicy zdalni nadal mogą wywoływać akcje kontrolera administratora, mimo że ich żądania nie są zgodne z trasą administracyjną.</span><span class="sxs-lookup"><span data-stu-id="010f3-131">In that case, remote users could still invoke actions of the Admin controller even though their requests wouldn't match the Admin route.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="010f3-132">[Poprzednie](creating-a-route-constraint-cs.md)
> [dalej](asp-net-mvc-controller-overview-vb.md)</span><span class="sxs-lookup"><span data-stu-id="010f3-132">[Previous](creating-a-route-constraint-cs.md)
[Next](asp-net-mvc-controller-overview-vb.md)</span></span>

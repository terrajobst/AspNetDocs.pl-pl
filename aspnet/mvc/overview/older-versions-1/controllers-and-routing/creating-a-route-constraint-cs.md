---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-cs
title: Tworzenie ograniczenia trasy (C#) | Microsoft Docs
author: StephenWalther
description: W tym samouczku Stephen Walther demonstruje, jak można kontrolować, jak żądania przeglądarki są zgodne z trasami, tworząc ograniczenia trasy z wyrażeniami regularnymi.
ms.author: riande
ms.date: 02/16/2009
ms.assetid: 0bfd06b1-12d3-4fbb-9779-a82e5eb7fe7d
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-cs
msc.type: authoredcontent
ms.openlocfilehash: 51ef859287b3424faf85f4a3606a220ab48a9466
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78582013"
---
# <a name="creating-a-route-constraint-c"></a><span data-ttu-id="a844e-103">Tworzenie ograniczenia trasy (C#)</span><span class="sxs-lookup"><span data-stu-id="a844e-103">Creating a Route Constraint (C#)</span></span>

<span data-ttu-id="a844e-104">Autor [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="a844e-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="a844e-105">W tym samouczku Stephen Walther demonstruje, jak można kontrolować, jak żądania przeglądarki są zgodne z trasami, tworząc ograniczenia trasy z wyrażeniami regularnymi.</span><span class="sxs-lookup"><span data-stu-id="a844e-105">In this tutorial, Stephen Walther demonstrates how you can control how browser requests match routes by creating route constraints with regular expressions.</span></span>

<span data-ttu-id="a844e-106">Ograniczenia trasy służą do ograniczania żądań przeglądarki, które pasują do określonej trasy.</span><span class="sxs-lookup"><span data-stu-id="a844e-106">You use route constraints to restrict the browser requests that match a particular route.</span></span> <span data-ttu-id="a844e-107">Aby określić ograniczenie trasy, można użyć wyrażenia regularnego.</span><span class="sxs-lookup"><span data-stu-id="a844e-107">You can use a regular expression to specify a route constraint.</span></span>

<span data-ttu-id="a844e-108">Załóżmy na przykład, że w pliku Global. asax została zdefiniowana trasa z listy 1.</span><span class="sxs-lookup"><span data-stu-id="a844e-108">For example, imagine that you have defined the route in Listing 1 in your Global.asax file.</span></span>

<span data-ttu-id="a844e-109">**Lista 1 — Global.asax.cs**</span><span class="sxs-lookup"><span data-stu-id="a844e-109">**Listing 1 - Global.asax.cs**</span></span>

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample1.cs)]

<span data-ttu-id="a844e-110">Lista 1 zawiera trasę o nazwie Product.</span><span class="sxs-lookup"><span data-stu-id="a844e-110">Listing 1 contains a route named Product.</span></span> <span data-ttu-id="a844e-111">Trasy produktu można użyć do mapowania żądań przeglądarki do ProductController zawartych w liście 2.</span><span class="sxs-lookup"><span data-stu-id="a844e-111">You can use the Product route to map browser requests to the ProductController contained in Listing 2.</span></span>

<span data-ttu-id="a844e-112">**Lista 2 — Controllers\ProductController.cs**</span><span class="sxs-lookup"><span data-stu-id="a844e-112">**Listing 2 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample2.cs)]

<span data-ttu-id="a844e-113">Zwróć uwagę, że akcja szczegóły () udostępniona przez kontroler produktu akceptuje pojedynczy parametr o nazwie productId.</span><span class="sxs-lookup"><span data-stu-id="a844e-113">Notice that the Details() action exposed by the Product controller accepts a single parameter named productId.</span></span> <span data-ttu-id="a844e-114">Ten parametr jest parametrem liczby całkowitej.</span><span class="sxs-lookup"><span data-stu-id="a844e-114">This parameter is an integer parameter.</span></span>

<span data-ttu-id="a844e-115">Trasa zdefiniowana na liście 1 będzie pasować do dowolnych z następujących adresów URL:</span><span class="sxs-lookup"><span data-stu-id="a844e-115">The route defined in Listing 1 will match any of the following URLs:</span></span>

- <span data-ttu-id="a844e-116">/Product/23</span><span class="sxs-lookup"><span data-stu-id="a844e-116">/Product/23</span></span>
- <span data-ttu-id="a844e-117">/Product/7</span><span class="sxs-lookup"><span data-stu-id="a844e-117">/Product/7</span></span>

<span data-ttu-id="a844e-118">Niestety trasa będzie również zgodna z następującymi adresami URL:</span><span class="sxs-lookup"><span data-stu-id="a844e-118">Unfortunately, the route will also match the following URLs:</span></span>

- <span data-ttu-id="a844e-119">/Product/blah</span><span class="sxs-lookup"><span data-stu-id="a844e-119">/Product/blah</span></span>
- <span data-ttu-id="a844e-120">/Product/apple</span><span class="sxs-lookup"><span data-stu-id="a844e-120">/Product/apple</span></span>

<span data-ttu-id="a844e-121">Ponieważ akcja Details () oczekuje parametru Integer, wykonanie żądania zawierającego coś innego niż wartość całkowita spowoduje wystąpienie błędu.</span><span class="sxs-lookup"><span data-stu-id="a844e-121">Because the Details() action expects an integer parameter, making a request that contains something other than an integer value will cause an error.</span></span> <span data-ttu-id="a844e-122">Jeśli na przykład wpiszesz adres URL/Product/Apple w przeglądarce, zobaczysz stronę błędu na rysunku 1.</span><span class="sxs-lookup"><span data-stu-id="a844e-122">For example, if you type the URL /Product/apple into your browser then you will get the error page in Figure 1.</span></span>

<span data-ttu-id="a844e-123">[![okno dialogowe Nowy projekt](creating-a-route-constraint-cs/_static/image1.jpg)](creating-a-route-constraint-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a844e-123">[![The New Project dialog box](creating-a-route-constraint-cs/_static/image1.jpg)](creating-a-route-constraint-cs/_static/image1.png)</span></span>

<span data-ttu-id="a844e-124">**Ilustracja 01**: wyświetlanie rozwinięcia strony ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-a-route-constraint-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="a844e-124">**Figure 01**: Seeing a page explode ([Click to view full-size image](creating-a-route-constraint-cs/_static/image2.png))</span></span>

<span data-ttu-id="a844e-125">To, co naprawdę chcesz zrobić, jest zgodne tylko z adresami URL, które zawierają poprawną liczbę całkowitą productId.</span><span class="sxs-lookup"><span data-stu-id="a844e-125">What you really want to do is only match URLs that contain a proper integer productId.</span></span> <span data-ttu-id="a844e-126">Można użyć ograniczenia podczas definiowania trasy, aby ograniczyć adresy URL, które pasują do trasy.</span><span class="sxs-lookup"><span data-stu-id="a844e-126">You can use a constraint when defining a route to restrict the URLs that match the route.</span></span> <span data-ttu-id="a844e-127">Zmodyfikowana trasa produktu na liście 3 zawiera ograniczenie wyrażenia regularnego, które jest zgodne tylko z liczbami całkowitymi.</span><span class="sxs-lookup"><span data-stu-id="a844e-127">The modified Product route in Listing 3 contains a regular expression constraint that only matches integers.</span></span>

<span data-ttu-id="a844e-128">**Lista 3 — Global.asax.cs**</span><span class="sxs-lookup"><span data-stu-id="a844e-128">**Listing 3 - Global.asax.cs**</span></span>

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample3.cs)]

<span data-ttu-id="a844e-129">Wyrażenie regularne \d + dopasowuje co najmniej jedną liczbę całkowitą.</span><span class="sxs-lookup"><span data-stu-id="a844e-129">The regular expression \d+ matches one or more integers.</span></span> <span data-ttu-id="a844e-130">To ograniczenie powoduje, że trasa produktu jest zgodna z następującymi adresami URL:</span><span class="sxs-lookup"><span data-stu-id="a844e-130">This constraint causes the Product route to match the following URLs:</span></span>

- <span data-ttu-id="a844e-131">/Product/3</span><span class="sxs-lookup"><span data-stu-id="a844e-131">/Product/3</span></span>
- <span data-ttu-id="a844e-132">/Product/8999</span><span class="sxs-lookup"><span data-stu-id="a844e-132">/Product/8999</span></span>

<span data-ttu-id="a844e-133">Ale nie następujące adresy URL:</span><span class="sxs-lookup"><span data-stu-id="a844e-133">But not the following URLs:</span></span>

- <span data-ttu-id="a844e-134">/Product/apple</span><span class="sxs-lookup"><span data-stu-id="a844e-134">/Product/apple</span></span>
- <span data-ttu-id="a844e-135">/Product</span><span class="sxs-lookup"><span data-stu-id="a844e-135">/Product</span></span>

- <span data-ttu-id="a844e-136">Te żądania przeglądarki będą obsługiwane przez inną trasę lub, jeśli nie ma pasujących tras, zostanie zwrócony *błąd.*</span><span class="sxs-lookup"><span data-stu-id="a844e-136">These browser requests will be handled by another route or, if there are no matching routes, a *The resource could not be found* error will be returned.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a844e-137">[Poprzednie](creating-custom-routes-cs.md)
> [dalej](creating-a-custom-route-constraint-cs.md)</span><span class="sxs-lookup"><span data-stu-id="a844e-137">[Previous](creating-custom-routes-cs.md)
[Next](creating-a-custom-route-constraint-cs.md)</span></span>

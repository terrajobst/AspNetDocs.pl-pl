---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-cs
title: Tworzenie tras niestandardowych (C#) | Microsoft Docs
author: microsoft
description: Dowiedz się, jak dodać niestandardowe trasy do aplikacji ASP.NET MVC. W tym samouczku dowiesz się, jak zmodyfikować domyślną tabelę tras w pliku Global. asax.
ms.author: riande
ms.date: 02/16/2009
ms.assetid: 3cd08f02-8763-490a-b625-2ac96a24b73f
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-cs
msc.type: authoredcontent
ms.openlocfilehash: 58f72e390f0053d136ef00ddbda0b071ba225d98
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78601333"
---
# <a name="creating-custom-routes-c"></a><span data-ttu-id="ee62a-104">Tworzenie tras niestandardowych (C#)</span><span class="sxs-lookup"><span data-stu-id="ee62a-104">Creating Custom Routes (C#)</span></span>

<span data-ttu-id="ee62a-105">przez [firmę Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="ee62a-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="ee62a-106">Dowiedz się, jak dodać niestandardowe trasy do aplikacji ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="ee62a-106">Learn how to add custom routes to an ASP.NET MVC application.</span></span> <span data-ttu-id="ee62a-107">W tym samouczku dowiesz się, jak zmodyfikować domyślną tabelę tras w pliku Global. asax.</span><span class="sxs-lookup"><span data-stu-id="ee62a-107">In this tutorial, you learn how to modify the default route table in the Global.asax file.</span></span>

<span data-ttu-id="ee62a-108">W tym samouczku dowiesz się, jak dodać trasę niestandardową do aplikacji ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="ee62a-108">In this tutorial, you learn how to add a custom route to an ASP.NET MVC application.</span></span> <span data-ttu-id="ee62a-109">Dowiesz się, jak zmodyfikować domyślną tabelę tras w pliku Global. asax przy użyciu trasy niestandardowej.</span><span class="sxs-lookup"><span data-stu-id="ee62a-109">You learn how to modify the default route table in the Global.asax file with a custom route.</span></span>

<span data-ttu-id="ee62a-110">W przypadku wielu prostych aplikacji ASP.NET MVC domyślna tabela tras będzie działać prawidłowo.</span><span class="sxs-lookup"><span data-stu-id="ee62a-110">For many simple ASP.NET MVC applications, the default route table will work just fine.</span></span> <span data-ttu-id="ee62a-111">Można jednak stwierdzić, że masz specjalne potrzeby routingu.</span><span class="sxs-lookup"><span data-stu-id="ee62a-111">However, you might discover that you have specialized routing needs.</span></span> <span data-ttu-id="ee62a-112">W takim przypadku można utworzyć trasę niestandardową.</span><span class="sxs-lookup"><span data-stu-id="ee62a-112">In that case, you can create a custom route.</span></span>

<span data-ttu-id="ee62a-113">Załóżmy na przykład, że tworzysz aplikację w blogu.</span><span class="sxs-lookup"><span data-stu-id="ee62a-113">Imagine, for example, that you are building a blog application.</span></span> <span data-ttu-id="ee62a-114">Możesz chcieć obsługiwać przychodzące żądania, które wyglądają następująco:</span><span class="sxs-lookup"><span data-stu-id="ee62a-114">You might want to handle incoming requests that look like this:</span></span>

<span data-ttu-id="ee62a-115">/Archive/12-25-2009</span><span class="sxs-lookup"><span data-stu-id="ee62a-115">/Archive/12-25-2009</span></span>

<span data-ttu-id="ee62a-116">Gdy użytkownik wprowadzi to żądanie, chce zwrócić wpis w blogu odpowiadający dacie 12/25/2009.</span><span class="sxs-lookup"><span data-stu-id="ee62a-116">When a user enters this request, you want to return the blog entry that corresponds to the date 12/25/2009.</span></span> <span data-ttu-id="ee62a-117">Aby można było obsłużyć ten typ żądania, należy utworzyć trasę niestandardową.</span><span class="sxs-lookup"><span data-stu-id="ee62a-117">In order to handle this type of request, you need to create a custom route.</span></span>

<span data-ttu-id="ee62a-118">Plik Global. asax znajdujący się na liście 1 zawiera nową trasę niestandardową o nazwie blog, która obsługuje żądania, które wyglądają jak*Data entry*/Archive/.</span><span class="sxs-lookup"><span data-stu-id="ee62a-118">The Global.asax file in Listing 1 contains a new custom route, named Blog, which handles requests that look like /Archive/*entry date*.</span></span>

<span data-ttu-id="ee62a-119">**Lista 1-Global. asax (z trasą niestandardową)**</span><span class="sxs-lookup"><span data-stu-id="ee62a-119">**Listing 1 - Global.asax (with custom route)**</span></span>

[!code-csharp[Main](creating-custom-routes-cs/samples/sample1.cs)]

<span data-ttu-id="ee62a-120">Kolejność tras dodawanych do tabeli tras jest ważna.</span><span class="sxs-lookup"><span data-stu-id="ee62a-120">The order of the routes that you add to the route table is important.</span></span> <span data-ttu-id="ee62a-121">Nasza nowa niestandardowa trasa blogu jest dodawana przed istniejącą domyślną trasą.</span><span class="sxs-lookup"><span data-stu-id="ee62a-121">Our new custom Blog route is added before the existing Default route.</span></span> <span data-ttu-id="ee62a-122">Jeśli zamówienie zostało cofnięte, trasa domyślna zawsze zostanie wywołana zamiast trasy niestandardowej.</span><span class="sxs-lookup"><span data-stu-id="ee62a-122">If you reversed the order, then the Default route always will get called instead of the custom route.</span></span>

<span data-ttu-id="ee62a-123">Niestandardowa trasa blogu dopasowuje wszystkie żądania, które zaczynają się od/Archive/.</span><span class="sxs-lookup"><span data-stu-id="ee62a-123">The custom Blog route matches any request that starts with /Archive/.</span></span> <span data-ttu-id="ee62a-124">Tak więc pasuje do wszystkich następujących adresów URL:</span><span class="sxs-lookup"><span data-stu-id="ee62a-124">So, it matches all of the following URLs:</span></span>

- <span data-ttu-id="ee62a-125">/Archive/12-25-2009</span><span class="sxs-lookup"><span data-stu-id="ee62a-125">/Archive/12-25-2009</span></span>

- <span data-ttu-id="ee62a-126">/Archive/10-6-2004</span><span class="sxs-lookup"><span data-stu-id="ee62a-126">/Archive/10-6-2004</span></span>

- <span data-ttu-id="ee62a-127">/Archive/apple</span><span class="sxs-lookup"><span data-stu-id="ee62a-127">/Archive/apple</span></span>

<span data-ttu-id="ee62a-128">Trasa niestandardowa mapuje żądanie przychodzące do kontrolera o nazwie archiwalne i wywołuje akcję wejścia ().</span><span class="sxs-lookup"><span data-stu-id="ee62a-128">The custom route maps the incoming request to a controller named Archive and invokes the Entry() action.</span></span> <span data-ttu-id="ee62a-129">Gdy wywoływana jest metoda entry (), Data wejścia jest przenoszona jako parametr o nazwie entryDate.</span><span class="sxs-lookup"><span data-stu-id="ee62a-129">When the Entry() method is called, the entry date is passed as a parameter named entryDate.</span></span>

<span data-ttu-id="ee62a-130">Możesz użyć trasy niestandardowej blogu z kontrolerem na liście 2.</span><span class="sxs-lookup"><span data-stu-id="ee62a-130">You can use the Blog custom route with the controller in Listing 2.</span></span>

<span data-ttu-id="ee62a-131">**Lista 2 — ArchiveController.cs**</span><span class="sxs-lookup"><span data-stu-id="ee62a-131">**Listing 2 - ArchiveController.cs**</span></span>

[!code-csharp[Main](creating-custom-routes-cs/samples/sample2.cs)]

<span data-ttu-id="ee62a-132">Zwróć uwagę, że metoda entry () w liście 2 akceptuje parametr typu DateTime.</span><span class="sxs-lookup"><span data-stu-id="ee62a-132">Notice that the Entry() method in Listing 2 accepts a parameter of type DateTime.</span></span> <span data-ttu-id="ee62a-133">Struktura MVC jest wystarczająco inteligentna, aby przekonwertować datę wejścia z adresu URL na wartość typu DateTime.</span><span class="sxs-lookup"><span data-stu-id="ee62a-133">The MVC framework is smart enough to convert the entry date from the URL into a DateTime value automatically.</span></span> <span data-ttu-id="ee62a-134">Jeśli nie można przekonwertować parametru daty wpisu z adresu URL na DateTime, zostanie zgłoszony błąd (patrz rysunek 1).</span><span class="sxs-lookup"><span data-stu-id="ee62a-134">If the entry date parameter from the URL cannot be converted to a DateTime, an error is raised (see Figure 1).</span></span>

<span data-ttu-id="ee62a-135">**Rysunek 1. Błąd konwersji parametru**</span><span class="sxs-lookup"><span data-stu-id="ee62a-135">**Figure 1 - Error from converting parameter**</span></span>

<span data-ttu-id="ee62a-136">[![okno dialogowe Nowy projekt](creating-custom-routes-cs/_static/image1.jpg)](creating-custom-routes-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="ee62a-136">[![The New Project dialog box](creating-custom-routes-cs/_static/image1.jpg)](creating-custom-routes-cs/_static/image1.png)</span></span>

<span data-ttu-id="ee62a-137">**Ilustracja 01**. błąd podczas konwertowania parametru ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-custom-routes-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="ee62a-137">**Figure 01**: Error from converting parameter ([Click to view full-size image](creating-custom-routes-cs/_static/image2.png))</span></span>

## <a name="summary"></a><span data-ttu-id="ee62a-138">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="ee62a-138">Summary</span></span>

<span data-ttu-id="ee62a-139">Celem tego samouczka było zademonstrowanie sposobu tworzenia trasy niestandardowej.</span><span class="sxs-lookup"><span data-stu-id="ee62a-139">The goal of this tutorial was to demonstrate how you can create a custom route.</span></span> <span data-ttu-id="ee62a-140">Dowiesz się, jak dodać trasę niestandardową do tabeli tras w pliku Global. asax, która reprezentuje wpisy w blogu.</span><span class="sxs-lookup"><span data-stu-id="ee62a-140">You learned how to add a custom route to the route table in the Global.asax file that represents blog entries.</span></span> <span data-ttu-id="ee62a-141">Omawiamy sposób mapowania żądań wpisów blogu do kontrolera o nazwie ArchiveController i akcji kontrolera o nazwie entry ().</span><span class="sxs-lookup"><span data-stu-id="ee62a-141">We discussed how to map requests for blog entries to a controller named ArchiveController and a controller action named Entry().</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ee62a-142">[Poprzednie](aspnet-mvc-controllers-overview-cs.md)
> [dalej](creating-a-route-constraint-cs.md)</span><span class="sxs-lookup"><span data-stu-id="ee62a-142">[Previous](aspnet-mvc-controllers-overview-cs.md)
[Next](creating-a-route-constraint-cs.md)</span></span>

---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-vb
title: Tworzenie ograniczenia trasy (VB) | Dokumentacja firmy Microsoft
author: StephenWalther
description: 'W tym samouczku Walther Autor: Stephen pokazuje, jak kontrolować, jak przeglądarka żąda dopasowanie tras przez tworzenie ograniczenia trasy z wyrażeniami regularnymi.'
ms.author: riande
ms.date: 02/16/2009
ms.assetid: b7cce113-c82c-45bf-b97b-357e5d9f7f56
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-vb
msc.type: authoredcontent
ms.openlocfilehash: 8c7b2274ff396f222382488ed877599e86ae5b99
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/09/2019
ms.locfileid: "59412687"
---
# <a name="creating-a-route-constraint-vb"></a><span data-ttu-id="34cae-103">Tworzenie ograniczenia trasy (VB)</span><span class="sxs-lookup"><span data-stu-id="34cae-103">Creating a Route Constraint (VB)</span></span>

<span data-ttu-id="34cae-104">przez [Walther Autor: Stephen](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="34cae-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="34cae-105">W tym samouczku Walther Autor: Stephen pokazuje, jak kontrolować, jak przeglądarka żąda dopasowanie tras przez tworzenie ograniczenia trasy z wyrażeniami regularnymi.</span><span class="sxs-lookup"><span data-stu-id="34cae-105">In this tutorial, Stephen Walther demonstrates how you can control how browser requests match routes by creating route constraints with regular expressions.</span></span>


<span data-ttu-id="34cae-106">Ograniczenia trasy umożliwia ograniczanie żądań przeglądarki, które pasuje do określonej trasy.</span><span class="sxs-lookup"><span data-stu-id="34cae-106">You use route constraints to restrict the browser requests that match a particular route.</span></span> <span data-ttu-id="34cae-107">Aby określić ograniczenia trasy, można użyć wyrażenia regularnego.</span><span class="sxs-lookup"><span data-stu-id="34cae-107">You can use a regular expression to specify a route constraint.</span></span>

<span data-ttu-id="34cae-108">Na przykład Wyobraź sobie, że zdefiniowano trasy w ofercie 1 w pliku Global.asax.</span><span class="sxs-lookup"><span data-stu-id="34cae-108">For example, imagine that you have defined the route in Listing 1 in your Global.asax file.</span></span>

**<span data-ttu-id="34cae-109">Wyświetlanie listy 1 - Global.asax.vb</span><span class="sxs-lookup"><span data-stu-id="34cae-109">Listing 1 - Global.asax.vb</span></span>**

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample1.vb)]

<span data-ttu-id="34cae-110">Wyświetlanie listy 1 zawiera trasę o nazwie produktu.</span><span class="sxs-lookup"><span data-stu-id="34cae-110">Listing 1 contains a route named Product.</span></span> <span data-ttu-id="34cae-111">Trasy produktu służy do mapowania żądania przeglądarki ProductController zawarte w ofercie 2.</span><span class="sxs-lookup"><span data-stu-id="34cae-111">You can use the Product route to map browser requests to the ProductController contained in Listing 2.</span></span>

**<span data-ttu-id="34cae-112">Wyświetlanie listy 2 - Controllers\ProductController.vb</span><span class="sxs-lookup"><span data-stu-id="34cae-112">Listing 2 - Controllers\ProductController.vb</span></span>**

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample2.vb)]

<span data-ttu-id="34cae-113">Należy zauważyć, że akcja Details() udostępnianych przez kontroler produktu przyjmuje jeden parametr o nazwie productId.</span><span class="sxs-lookup"><span data-stu-id="34cae-113">Notice that the Details() action exposed by the Product controller accepts a single parameter named productId.</span></span> <span data-ttu-id="34cae-114">Ten parametr jest parametrem liczby całkowitej.</span><span class="sxs-lookup"><span data-stu-id="34cae-114">This parameter is an integer parameter.</span></span>

<span data-ttu-id="34cae-115">Trasy zdefiniowane w ofercie 1 będzie pasuje do żadnego z następujących adresów URL:</span><span class="sxs-lookup"><span data-stu-id="34cae-115">The route defined in Listing 1 will match any of the following URLs:</span></span>

- <span data-ttu-id="34cae-116">/ Produktu/23</span><span class="sxs-lookup"><span data-stu-id="34cae-116">/Product/23</span></span>
- <span data-ttu-id="34cae-117">Produkt/7 dni w tygodniu</span><span class="sxs-lookup"><span data-stu-id="34cae-117">/Product/7</span></span>

<span data-ttu-id="34cae-118">Niestety trasy się zgadzać następujące adresy URL:</span><span class="sxs-lookup"><span data-stu-id="34cae-118">Unfortunately, the route will also match the following URLs:</span></span>

- <span data-ttu-id="34cae-119">/ Produktu/blah</span><span class="sxs-lookup"><span data-stu-id="34cae-119">/Product/blah</span></span>
- <span data-ttu-id="34cae-120">/Product/apple</span><span class="sxs-lookup"><span data-stu-id="34cae-120">/Product/apple</span></span>

<span data-ttu-id="34cae-121">Ponieważ akcji Details() oczekuje parametru liczby całkowitej, żądania zawiera coś innego niż wartość całkowitą spowoduje błąd.</span><span class="sxs-lookup"><span data-stu-id="34cae-121">Because the Details() action expects an integer parameter, making a request that contains something other than an integer value will cause an error.</span></span> <span data-ttu-id="34cae-122">Na przykład jeśli wpiszesz /Product/apple adresu URL w przeglądarce następnie otrzymasz strony błędu na rysunku 1.</span><span class="sxs-lookup"><span data-stu-id="34cae-122">For example, if you type the URL /Product/apple into your browser then you will get the error page in Figure 1.</span></span>


[![T<span data-ttu-id="34cae-123">okno dialogowe Nowy projekt HE]</span><span class="sxs-lookup"><span data-stu-id="34cae-123">he New Project dialog box]</span></span>(creating-a-route-constraint-vb/_static/image1.jpg)](creating-a-route-constraint-vb/_static/image1.png)

<span data-ttu-id="34cae-124">**Rysunek 01**: Wyświetlanie strony explode ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-route-constraint-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="34cae-124">**Figure 01**: Seeing a page explode ([Click to view full-size image](creating-a-route-constraint-vb/_static/image2.png))</span></span>


<span data-ttu-id="34cae-125">Co tak naprawdę chcesz zrobić to dopasowanie tylko adresów URL zawierających productId właściwej liczby całkowitej.</span><span class="sxs-lookup"><span data-stu-id="34cae-125">What you really want to do is only match URLs that contain a proper integer productId.</span></span> <span data-ttu-id="34cae-126">Można użyć ograniczenie podczas definiowania trasy, aby ograniczyć adresy URL, które odpowiadają trasy.</span><span class="sxs-lookup"><span data-stu-id="34cae-126">You can use a constraint when defining a route to restrict the URLs that match the route.</span></span> <span data-ttu-id="34cae-127">Zmodyfikowane trasy produktu w ofercie 3 zawiera ograniczenia wyrażenia regularnego, który jest zgodny tylko liczby całkowite.</span><span class="sxs-lookup"><span data-stu-id="34cae-127">The modified Product route in Listing 3 contains a regular expression constraint that only matches integers.</span></span>

**<span data-ttu-id="34cae-128">Wyświetlanie listy 3 - Global.asax.vb</span><span class="sxs-lookup"><span data-stu-id="34cae-128">Listing 3 - Global.asax.vb</span></span>**

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample3.vb)]

<span data-ttu-id="34cae-129">\D+ wyrażenia regularnego dopasowuje jeden lub więcej liczb całkowitych.</span><span class="sxs-lookup"><span data-stu-id="34cae-129">The regular expression \d+ matches one or more integers.</span></span> <span data-ttu-id="34cae-130">To ograniczenie powoduje, że trasy produktu dopasować następujące adresy URL:</span><span class="sxs-lookup"><span data-stu-id="34cae-130">This constraint causes the Product route to match the following URLs:</span></span>

- <span data-ttu-id="34cae-131">/ Produkt/3</span><span class="sxs-lookup"><span data-stu-id="34cae-131">/Product/3</span></span>
- <span data-ttu-id="34cae-132">/Product/8999</span><span class="sxs-lookup"><span data-stu-id="34cae-132">/Product/8999</span></span>

<span data-ttu-id="34cae-133">Ale nie następujące adresy URL:</span><span class="sxs-lookup"><span data-stu-id="34cae-133">But not the following URLs:</span></span>

- <span data-ttu-id="34cae-134">/Product/apple</span><span class="sxs-lookup"><span data-stu-id="34cae-134">/Product/apple</span></span>
- <span data-ttu-id="34cae-135">/ Produktu</span><span class="sxs-lookup"><span data-stu-id="34cae-135">/Product</span></span>

<span data-ttu-id="34cae-136">Te żądania przeglądarki będzie obsługiwany przez innej trasy lub, jeśli istnieje nie zgodnych tras *nie można odnaleźć zasobu* zostanie zwrócony błąd.</span><span class="sxs-lookup"><span data-stu-id="34cae-136">These browser requests will be handled by another route or, if there are no matching routes, a *The resource could not be found* error will be returned.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="34cae-137">[Poprzednie](creating-custom-routes-vb.md)
> [dalej](creating-a-custom-route-constraint-vb.md)</span><span class="sxs-lookup"><span data-stu-id="34cae-137">[Previous](creating-custom-routes-vb.md)
[Next](creating-a-custom-route-constraint-vb.md)</span></span>

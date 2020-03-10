---
uid: web-forms/videos/net-4/routing/how-do-i-work-with-urls-in-aspnet-routing
title: Jak korzystać z adresów URL w routingu ASP.NET? | Microsoft Docs
author: rick-anderson
description: W tym filmie wideo Krzysztof pikseli pokazuje, jak określić adresy URL w witrynie sieci Web, która korzysta z routingu ASP.NET. Najpierw zostanie utworzona witryna sieci Web, a w polu gł...
ms.author: riande
ms.date: 10/15/2010
ms.assetid: 08f9d0a7-cfa0-4914-a672-8a64295d7ba8
msc.legacyurl: /web-forms/videos/net-4/routing/how-do-i-work-with-urls-in-aspnet-routing
msc.type: video
ms.openlocfilehash: eb1060fd9cc9469dc2b1d2e918823316c36840cb
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78565129"
---
# <a name="how-do-i-work-with-urls-in-aspnet-routing"></a><span data-ttu-id="7fcdd-105">Jak korzystać z adresów URL w routingu ASP.NET?</span><span class="sxs-lookup"><span data-stu-id="7fcdd-105">How Do I: Work with URLs in ASP.NET Routing?</span></span>

<span data-ttu-id="7fcdd-106">[Krzysztof pikseli](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="7fcdd-106">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="7fcdd-107">W tym filmie wideo Krzysztof pikseli pokazuje, jak określić adresy URL w witrynie sieci Web, która korzysta z routingu ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="7fcdd-107">In this video Chris Pels shows how to specify URLs in a web site that utilizes ASP.NET routing.</span></span> <span data-ttu-id="7fcdd-108">Najpierw zostanie utworzona witryna sieci Web, a w globalnej klasie aplikacji (. asax) jest definiowana Routing.</span><span class="sxs-lookup"><span data-stu-id="7fcdd-108">First, a web site is created and routing is defined in the Global Application Class (.asax).</span></span> <span data-ttu-id="7fcdd-109">Następnie zostanie utworzona Przykładowa strona sieci Web, a na stronie zostanie dodany adres URL oparty na zdefiniowanej trasie przy użyciu standardowego podejścia "kodowanego", np. "~/Stats/Visitors".</span><span class="sxs-lookup"><span data-stu-id="7fcdd-109">Next, a sample web page is created and a URL based upon a defined route is added to the page using the standard "hard coded" approach, e.g., "~/Stats/Visitors".</span></span> <span data-ttu-id="7fcdd-110">Następnie do strony zostanie dodany inny link, który dynamicznie generuje ten sam adres URL w znaczniku przy użyciu metody RouteValue, która akceptuje nazwę trasy i parametry.</span><span class="sxs-lookup"><span data-stu-id="7fcdd-110">Another link is then added to the page which dynamically generates the same URL in markup using the RouteValue method which accepts the route name and parameters.</span></span> <span data-ttu-id="7fcdd-111">Ten sam adres URL jest następnie implementowany przy użyciu kodu zamiast znacznika bezpośrednio na stronie.</span><span class="sxs-lookup"><span data-stu-id="7fcdd-111">The same URL is then implemented using code rather than markup directly in the page.</span></span> <span data-ttu-id="7fcdd-112">Oryginalna trasa i fizyczna lokalizacja strony są następnie zmieniane, co spowoduje, że link twardy zakodowany nie działa prawidłowo, a obie automatycznie generowane łącza działają poprawnie.</span><span class="sxs-lookup"><span data-stu-id="7fcdd-112">The original route and physical page location are then changed, resulting in the hard coded link no longer working whereas both dynamically generated links function properly.</span></span> <span data-ttu-id="7fcdd-113">Na koniec zostanie omówiona wartość linków dynamicznie generowanych.</span><span class="sxs-lookup"><span data-stu-id="7fcdd-113">Finally, the value of dynamically generated links is then discussed.</span></span>

[<span data-ttu-id="7fcdd-114">&#9654;Obejrzyj wideo (20 minut)</span><span class="sxs-lookup"><span data-stu-id="7fcdd-114">&#9654; Watch video (20 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-work-with-urls-in-aspnet-routing)

> [!div class="step-by-step"]
> [<span data-ttu-id="7fcdd-115">Wstecz</span><span class="sxs-lookup"><span data-stu-id="7fcdd-115">Previous</span></span>](how-do-i-use-routing-with-aspnet-web-forms.md)

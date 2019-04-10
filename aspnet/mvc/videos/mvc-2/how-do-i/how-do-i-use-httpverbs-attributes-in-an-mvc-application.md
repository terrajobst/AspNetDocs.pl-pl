---
uid: mvc/videos/mvc-2/how-do-i/how-do-i-use-httpverbs-attributes-in-an-mvc-application
title: Jak mogę Używanie atrybutów HttpVerbs w aplikacji MVC? | Microsoft Docs
author: rick-anderson
description: W tym wideo pikseli Chris pokazuje, jak używanie atrybutów HttpVerbs w celu kontrolowania dostępu do akcji MVC. Po pierwsze Przykładowa aplikacja jest tworzony z ko domyślny...
ms.author: riande
ms.date: 12/30/2009
ms.assetid: d2488a1d-0f3f-4994-8fbe-4f59b8c9503e
msc.legacyurl: /mvc/videos/mvc-2/how-do-i/how-do-i-use-httpverbs-attributes-in-an-mvc-application
msc.type: video
ms.openlocfilehash: bda3b122aaf2970b9238d7120ad15fb06672c85b
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/09/2019
ms.locfileid: "59378081"
---
# <a name="how-do-i-use-httpverbs-attributes-in-an-mvc-application"></a><span data-ttu-id="8db87-105">Jak mogę Używanie atrybutów HttpVerbs w aplikacji MVC?</span><span class="sxs-lookup"><span data-stu-id="8db87-105">How Do I: Use HttpVerbs Attributes in an MVC Application?</span></span>

<span data-ttu-id="8db87-106">przez [Chris pikseli](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="8db87-106">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="8db87-107">W tym wideo pikseli Chris pokazuje, jak używanie atrybutów HttpVerbs w celu kontrolowania dostępu do akcji MVC.</span><span class="sxs-lookup"><span data-stu-id="8db87-107">In this video Chris Pels shows how to use the HttpVerbs attributes to control access to MVC actions.</span></span> <span data-ttu-id="8db87-108">Po pierwsze przykładowej aplikacji tworzona jest domyślny kontroler i widok do edycji informacji.</span><span class="sxs-lookup"><span data-stu-id="8db87-108">First, a sample application is created with a default controller and view for editing the information.</span></span> <span data-ttu-id="8db87-109">Następnie drugiej akcji indeksu jest dodawany do kontrolera, który ma atrybut HttpPost, który ogranicza wywoływana tylko wtedy, gdy jest używana przez metodę POST protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="8db87-109">Next, a second Index action is added to the controller which has an HttpPost attribute which restricts it to being called only when an HTTP POST is used.</span></span> <span data-ttu-id="8db87-110">Jako kontynuację atrybut AcceptVerbs() jest implementowany jako alternatywnej składni dla programu Visual Studio 2008.</span><span class="sxs-lookup"><span data-stu-id="8db87-110">As a follow-up, the AcceptVerbs() attribute is implemented as an alternative syntax for Visual Studio 2008.</span></span> <span data-ttu-id="8db87-111">Następnie omówiono użytkowania HttpVerbs w celu zapobiegania zagrożenie bezpieczeństwa związanych z użyciem żądania HTTP GET do wykonania usuwania z łącza.</span><span class="sxs-lookup"><span data-stu-id="8db87-111">A use of the HttpVerbs for preventing the security risk associated with using an HTTP GET to perform a delete from a link is then discussed.</span></span>

[<span data-ttu-id="8db87-112">&#9654;Obejrzyj film wideo (16 w minutach)</span><span class="sxs-lookup"><span data-stu-id="8db87-112">&#9654; Watch video (16 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-use-httpverbs-attributes-in-an-mvc-application)

> [!div class="step-by-step"]
> <span data-ttu-id="8db87-113">[Poprzednie](how-do-i-work-with-model-binders-in-an-mvc-application.md)
> [dalej](mvc2-html-encoding.md)</span><span class="sxs-lookup"><span data-stu-id="8db87-113">[Previous](how-do-i-work-with-model-binders-in-an-mvc-application.md)
[Next](mvc2-html-encoding.md)</span></span>

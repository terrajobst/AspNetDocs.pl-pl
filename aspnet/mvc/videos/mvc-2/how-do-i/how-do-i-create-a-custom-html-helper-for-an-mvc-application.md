---
uid: mvc/videos/mvc-2/how-do-i/how-do-i-create-a-custom-html-helper-for-an-mvc-application
title: 'Jak: Tworzenie niestandardowego pomocnika HTML dla aplikacji MVC? | Microsoft Docs'
author: rick-anderson
description: W tym filmie wideo Krzysztof pikseli pokazuje, jak utworzyć niestandardowy HtmlHelper, który nie jest dostępny w standardowym zestawie w aplikacji MVC. Najpierw przykładowy zastoso MVC...
ms.author: riande
ms.date: 12/11/2009
ms.assetid: 58b5eb15-4160-4ce2-ae70-6ba94262ea73
msc.legacyurl: /mvc/videos/mvc-2/how-do-i/how-do-i-create-a-custom-html-helper-for-an-mvc-application
msc.type: video
ms.openlocfilehash: 60953243d3038667e4f729b1394e68f0c9d7c178
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78559046"
---
# <a name="how-do-i-create-a-custom-html-helper-for-an-mvc-application"></a><span data-ttu-id="d81d5-105">Jak: Tworzenie niestandardowego pomocnika HTML dla aplikacji MVC?</span><span class="sxs-lookup"><span data-stu-id="d81d5-105">How Do I: Create a Custom HTML Helper for an MVC Application?</span></span>

<span data-ttu-id="d81d5-106">[Krzysztof pikseli](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="d81d5-106">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="d81d5-107">W tym filmie wideo Krzysztof pikseli pokazuje, jak utworzyć niestandardowy HtmlHelper, który nie jest dostępny w standardowym zestawie w aplikacji MVC.</span><span class="sxs-lookup"><span data-stu-id="d81d5-107">In this video Chris Pels shows how to create a custom HtmlHelper that is not available in the standard set in an MVC application.</span></span> <span data-ttu-id="d81d5-108">Najpierw zostanie utworzona Przykładowa aplikacja MVC z kontrolerem demonstracyjnym i widok służący do testowania niestandardowego HtmlHelper.</span><span class="sxs-lookup"><span data-stu-id="d81d5-108">First, a sample MVC application is created with a demo controller and view to test the custom HtmlHelper.</span></span> <span data-ttu-id="d81d5-109">Następnie moduł jest tworzony przy użyciu funkcji publicznej, która jest metodą rozszerzenia, która reprezentuje implementację niestandardowego HtmlHelper.</span><span class="sxs-lookup"><span data-stu-id="d81d5-109">Next, a module is created with a public function that is an extension method which represents the implementation of the custom HtmlHelper.</span></span> <span data-ttu-id="d81d5-110">Pomocnik niestandardowy służy do tworzenia tagów `<img>` na stronie i otrzymywania kilku parametrów przychodzących, takich jak identyfikator, adres URL i tekst alternatywny dla tagu obrazu.</span><span class="sxs-lookup"><span data-stu-id="d81d5-110">The custom helper is for creating `<img>` tags in a page and receives several inbound parameters including the id, url, and alt text for the image tag.</span></span> <span data-ttu-id="d81d5-111">Następnie logika jest dodawana do funkcji w celu zwrócenia znacznika ukończony `<img>` z określonymi informacjami.</span><span class="sxs-lookup"><span data-stu-id="d81d5-111">The logic is then added to the function for returning the completed `<img>` tag with the specified information.</span></span> <span data-ttu-id="d81d5-112">Następnie niestandardowy HtmlHelper jest używany na stronie demonstracyjnej do wyświetlania obrazu.</span><span class="sxs-lookup"><span data-stu-id="d81d5-112">Then the custom HtmlHelper is used on the demo page to display an image.</span></span> <span data-ttu-id="d81d5-113">Na koniec niestandardowy HtmlHelper jest rozwinięty, aby obejmował wiele zastąpień konstruktora, które zapewniają elastyczność umożliwiającą łatwiejsze tworzenie różnych tagów `<img>`.</span><span class="sxs-lookup"><span data-stu-id="d81d5-113">Finally, the custom HtmlHelper is expanded to include multiple constructor overrides which provide flexibility for more easily creating different `<img>` tags.</span></span>

[<span data-ttu-id="d81d5-114">&#9654;Obejrzyj wideo (18 minut)</span><span class="sxs-lookup"><span data-stu-id="d81d5-114">&#9654; Watch video (18 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-create-a-custom-html-helper-for-an-mvc-application)

> [!div class="step-by-step"]
> <span data-ttu-id="d81d5-115">[Poprzednie](how-do-i-implement-view-models-to-manage-data-for-aspnet-mvc-views.md)
> [dalej](how-do-i-work-with-model-binders-in-an-mvc-application.md)</span><span class="sxs-lookup"><span data-stu-id="d81d5-115">[Previous](how-do-i-implement-view-models-to-manage-data-for-aspnet-mvc-views.md)
[Next](how-do-i-work-with-model-binders-in-an-mvc-application.md)</span></span>

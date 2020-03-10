---
uid: web-forms/videos/aspnet-ajax/how-do-i-implement-the-persistent-communications-pattern-using-web-services
title: '[Jak:] Zaimplementować wzorzec komunikacji trwałej przy użyciu usług sieci Web? | Microsoft Docs'
author: JoeStagner
description: W tradycyjnej witrynie sieci Web przeglądarka i serwer nie utrzymują bieżąco komunikacji, ale komunikują się tylko z odpowiedzią na użytkownika wykonującego działania...
ms.author: riande
ms.date: 08/22/2007
ms.assetid: 424c06cd-6d61-43cd-a1f2-d1a6b62e47b1
msc.legacyurl: /web-forms/videos/aspnet-ajax/how-do-i-implement-the-persistent-communications-pattern-using-web-services
msc.type: video
ms.openlocfilehash: de2eb281cd4bab46635af480ac2e8f07f60f1591
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78628752"
---
# <a name="how-do-i-implement-the-persistent-communications-pattern-using-web-services"></a><span data-ttu-id="09eaf-104">[Jak:] Zaimplementować wzorzec komunikacji trwałej przy użyciu usług sieci Web?</span><span class="sxs-lookup"><span data-stu-id="09eaf-104">[How Do I:] Implement the Persistent Communications Pattern using Web Services?</span></span>

<span data-ttu-id="09eaf-105">Jan [Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="09eaf-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

<span data-ttu-id="09eaf-106">W tradycyjnej witrynie sieci Web przeglądarka i serwer nie utrzymują bieżąco komunikacji, ale komunikują się tylko z odpowiedzią na użytkownika wykonującego akcję.</span><span class="sxs-lookup"><span data-stu-id="09eaf-106">In a traditional Web site the browser and the server do not maintain an ongoing communication, but communicate only in response to the user performing an action.</span></span> <span data-ttu-id="09eaf-107">W nowoczesnej witrynie sieci Web, gdzie strona stanie się kontenerem aplikacji, może być korzystne dla przeglądarki i serwera do obsługi trwającej komunikacji, dzięki czemu aktualizacje stron mogą wystąpić bez wykonywania akcji przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="09eaf-107">In a modern Web site where the page becomes an application container, it can be advantageous for the browser and the server to maintain an ongoing communication so that page updates can occur without the user performing an action.</span></span> <span data-ttu-id="09eaf-108">Jest to nazywane wzorcem komunikacji trwałej dla technologii AJAX.</span><span class="sxs-lookup"><span data-stu-id="09eaf-108">This is known as the Persistent Communications Pattern for AJAX.</span></span> <span data-ttu-id="09eaf-109">ASP.NET AJAX oferuje dwa główne sposoby wdrażania wzorca komunikacji trwałej przez deweloperów sieci Web.</span><span class="sxs-lookup"><span data-stu-id="09eaf-109">ASP.NET AJAX provides two main ways for Web developers to implement the Persistent Communications Pattern.</span></span> <span data-ttu-id="09eaf-110">Na wcześniejszym filmie wideo znaleźliśmy, jak używać elementu UpdatePanel ASP.NET AJAX jako podstawy implementacji.</span><span class="sxs-lookup"><span data-stu-id="09eaf-110">In an earlier video we saw how to use the ASP.NET AJAX UpdatePanel as the basis of the implementation.</span></span> <span data-ttu-id="09eaf-111">W tym filmie wideo dowiesz się, jak zaimplementować ten sam wzorzec przy użyciu wywołania JavaScrpt do usługi sieci Web, która eliminuje potrzebę ASP.NET AJAX UpdatePanel.</span><span class="sxs-lookup"><span data-stu-id="09eaf-111">In this video we learn how to implement the same pattern using a JavaScrpt call to a Web service, which removes the need for an ASP.NET AJAX UpdatePanel.</span></span>

[<span data-ttu-id="09eaf-112">&#9654;Obejrzyj wideo (16 minut)</span><span class="sxs-lookup"><span data-stu-id="09eaf-112">&#9654; Watch video (16 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-implement-the-persistent-communications-pattern-using-web-services)

> [!div class="step-by-step"]
> <span data-ttu-id="09eaf-113">[Poprzednie](how-do-i-localize-an-aspnet-ajax-application.md)
> [dalej](how-do-i-trigger-an-updatepanel-refresh-from-a-dropdownlist-control.md)</span><span class="sxs-lookup"><span data-stu-id="09eaf-113">[Previous](how-do-i-localize-an-aspnet-ajax-application.md)
[Next](how-do-i-trigger-an-updatepanel-refresh-from-a-dropdownlist-control.md)</span></span>

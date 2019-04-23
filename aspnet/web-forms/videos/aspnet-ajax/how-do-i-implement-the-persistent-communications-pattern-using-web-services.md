---
uid: web-forms/videos/aspnet-ajax/how-do-i-implement-the-persistent-communications-pattern-using-web-services
title: '[Jak mogę] Implementowanie wzorca trwałej komunikacji przy użyciu usługi sieci Web? | Microsoft Docs'
author: JoeStagner
description: W witrynie sieci Web tradycyjnych przeglądarką a serwerem nie obsługują utrzymują stałą łączność, ale komunikują się tylko w odpowiedzi na użytkownika wykonującego działanie...
ms.author: riande
ms.date: 08/22/2007
ms.assetid: 424c06cd-6d61-43cd-a1f2-d1a6b62e47b1
msc.legacyurl: /web-forms/videos/aspnet-ajax/how-do-i-implement-the-persistent-communications-pattern-using-web-services
msc.type: video
ms.openlocfilehash: de2eb281cd4bab46635af480ac2e8f07f60f1591
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/17/2019
ms.locfileid: "59408111"
---
# <a name="how-do-i-implement-the-persistent-communications-pattern-using-web-services"></a><span data-ttu-id="c3aab-104">[Jak mogę] Implementowanie wzorca trwałej komunikacji przy użyciu usługi sieci Web?</span><span class="sxs-lookup"><span data-stu-id="c3aab-104">[How Do I:] Implement the Persistent Communications Pattern using Web Services?</span></span>

<span data-ttu-id="c3aab-105">przez [Stagner Jan](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="c3aab-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

<span data-ttu-id="c3aab-106">W witrynie sieci Web tradycyjnych przeglądarką a serwerem nie obsługują utrzymują stałą łączność, ale komunikują się tylko w odpowiedzi na użytkownika wykonującego akcję.</span><span class="sxs-lookup"><span data-stu-id="c3aab-106">In a traditional Web site the browser and the server do not maintain an ongoing communication, but communicate only in response to the user performing an action.</span></span> <span data-ttu-id="c3aab-107">W nowoczesnych witryny sieci Web, gdy strona staje się kontener aplikacji może być korzystne dla przeglądarki i serwera, aby zachować utrzymują stałą łączność aktualizacji stron mogła zaistnieć bez użytkownika wykonującego akcję.</span><span class="sxs-lookup"><span data-stu-id="c3aab-107">In a modern Web site where the page becomes an application container, it can be advantageous for the browser and the server to maintain an ongoing communication so that page updates can occur without the user performing an action.</span></span> <span data-ttu-id="c3aab-108">Jest to nazywane wzorca trwałej komunikacji technologii AJAX.</span><span class="sxs-lookup"><span data-stu-id="c3aab-108">This is known as the Persistent Communications Pattern for AJAX.</span></span> <span data-ttu-id="c3aab-109">ASP.NET AJAX zawiera dwa główne sposoby dla deweloperów sieci Web w celu zaimplementowania wzorca trwałej komunikacji.</span><span class="sxs-lookup"><span data-stu-id="c3aab-109">ASP.NET AJAX provides two main ways for Web developers to implement the Persistent Communications Pattern.</span></span> <span data-ttu-id="c3aab-110">W filmie wcześniej widzieliśmy jak używać kontrolki UpdatePanel ASP.NET AJAX jako podstawa wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="c3aab-110">In an earlier video we saw how to use the ASP.NET AJAX UpdatePanel as the basis of the implementation.</span></span> <span data-ttu-id="c3aab-111">W tym filmie pokazano możemy informacje o sposobie implementacji tego samego wzorca przy użyciu JavaScrpt wywołania usługi sieci Web, co eliminuje potrzebę ASP.NET AJAX UpdatePanel.</span><span class="sxs-lookup"><span data-stu-id="c3aab-111">In this video we learn how to implement the same pattern using a JavaScrpt call to a Web service, which removes the need for an ASP.NET AJAX UpdatePanel.</span></span>

[<span data-ttu-id="c3aab-112">&#9654;Obejrzyj film wideo (16 w minutach)</span><span class="sxs-lookup"><span data-stu-id="c3aab-112">&#9654; Watch video (16 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-implement-the-persistent-communications-pattern-using-web-services)

> [!div class="step-by-step"]
> <span data-ttu-id="c3aab-113">[Poprzednie](how-do-i-localize-an-aspnet-ajax-application.md)
> [dalej](how-do-i-trigger-an-updatepanel-refresh-from-a-dropdownlist-control.md)</span><span class="sxs-lookup"><span data-stu-id="c3aab-113">[Previous](how-do-i-localize-an-aspnet-ajax-application.md)
[Next](how-do-i-trigger-an-updatepanel-refresh-from-a-dropdownlist-control.md)</span></span>

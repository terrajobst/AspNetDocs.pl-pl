---
uid: web-forms/videos/how-do-i/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information
title: '[Jak:] Kontroluj buforowanie strony ASP.NET na podstawie informacji niestandardowych | Microsoft Docs'
author: rick-anderson
description: W tym filmie wideo Krzysztof pikseli pokazuje, jak kontrolować kryteria buforowania strony ASP.NET na podstawie informacji niestandardowych. Zostanie utworzona Przykładowa strona, a następnie polecenie O...
ms.author: riande
ms.date: 02/19/2009
ms.assetid: f230c316-1313-4b8f-967c-62f9684fe378
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information
msc.type: video
ms.openlocfilehash: 676bc8f234a6e517104d07fd58a0ff14aec73e69
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78624734"
---
# <a name="how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information"></a><span data-ttu-id="4a258-104">[Jak:] Kontrolowanie buforowania strony ASP.NET na podstawie informacji niestandardowych</span><span class="sxs-lookup"><span data-stu-id="4a258-104">[How Do I:] Control the Caching of an ASP.NET Page Based Upon Custom Information</span></span>

<span data-ttu-id="4a258-105">[Krzysztof pikseli](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="4a258-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="4a258-106">W tym filmie wideo Krzysztof pikseli pokazuje, jak kontrolować kryteria buforowania strony ASP.NET na podstawie informacji niestandardowych.</span><span class="sxs-lookup"><span data-stu-id="4a258-106">In this video Chris Pels shows how to control the criteria for caching an ASP.NET page based upon custom information.</span></span> <span data-ttu-id="4a258-107">Zostanie utworzona Przykładowa strona, a następnie dyrektywa OutputCache jest używana z atrybutem VaryByCustom, który zawiera wartość niestandardową.</span><span class="sxs-lookup"><span data-stu-id="4a258-107">A sample page is created and then the OutputCache directive is used with the VaryByCustom attribute which contains a custom value.</span></span> <span data-ttu-id="4a258-108">Następnie Metoda GetVaryCustomByString () zostanie zastąpiona w module Global. asax, który zapewnia obsługę atrybutu niestandardowego.</span><span class="sxs-lookup"><span data-stu-id="4a258-108">Next, the GetVaryCustomByString() method is overridden in the global.asax module which provides the handling of the custom attribute.</span></span> <span data-ttu-id="4a258-109">W tej metodzie zwracany jest ciąg, który jednoznacznie identyfikuje buforowaną wersję strony.</span><span class="sxs-lookup"><span data-stu-id="4a258-109">In that method a string is returned that uniquely identifies the cached version of the page.</span></span> <span data-ttu-id="4a258-110">Na koniec istnieje dyskusja dotycząca sposobu buforowania przy użyciu wartości niestandardowej na kilka sposobów dla witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="4a258-110">Finally, there is a discussion about how caching using a custom value can be used in several ways for a web site.</span></span>

[<span data-ttu-id="4a258-111">&#9654;Obejrzyj wideo (12 minut)</span><span class="sxs-lookup"><span data-stu-id="4a258-111">&#9654; Watch video (12 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information)

---
uid: web-forms/videos/how-do-i/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header
title: '[Jak:]  Buforowanie strony ASP.NET na podstawie informacji z nagłówka HTTP | Microsoft Docs'
author: rick-anderson
description: W tym filmie wideo Krzysztof pikseli pokazuje, jak zachować stronę w wyjściowej pamięci podręcznej ASP.NET na podstawie informacji w nagłówku HTTP strony. Najpierw potencjalny HEA HTTP...
ms.author: riande
ms.date: 02/26/2009
ms.assetid: 0f8df1bd-080a-4eeb-980c-c2fbb05d30c2
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header
msc.type: video
ms.openlocfilehash: 79c27f39793a4a3a94ea412838fb3844579e874d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78624972"
---
# <a name="how-do-i--cache-an-aspnet-page-based-upon-information-in-the-http-header"></a><span data-ttu-id="ff5bb-104">[Jak:]  Buforowanie strony ASP.NET na podstawie informacji w nagłówku HTTP</span><span class="sxs-lookup"><span data-stu-id="ff5bb-104">[How Do I:]  Cache an ASP.NET Page Based Upon Information in the HTTP Header</span></span>

<span data-ttu-id="ff5bb-105">[Krzysztof pikseli](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="ff5bb-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="ff5bb-106">W tym filmie wideo Krzysztof pikseli pokazuje, jak zachować stronę w wyjściowej pamięci podręcznej ASP.NET na podstawie informacji w nagłówku HTTP strony.</span><span class="sxs-lookup"><span data-stu-id="ff5bb-106">In this video Chris Pels shows how to keep a page in the ASP.NET output cache based upon information in the page's HTTP header.</span></span> <span data-ttu-id="ff5bb-107">Najpierw są analizowane potencjalne wartości nagłówka HTTP.</span><span class="sxs-lookup"><span data-stu-id="ff5bb-107">First, the potential HTTP header values are reviewed.</span></span> <span data-ttu-id="ff5bb-108">Następnie zostanie utworzona Przykładowa strona, a następnie dyrektywa OutputCache jest używana z atrybutem VaryByHeader, który zawiera wartość "Accept-Language", nagłówka HTTP, aby kontrolować buforowanie na podstawie języka przeglądarki użytkownika.</span><span class="sxs-lookup"><span data-stu-id="ff5bb-108">Then, a sample page is created and then the OutputCache directive is used with the VaryByHeader attribute which contains a value "accept-language", an HTTP header, to control caching based upon the language of the user's browser.</span></span> <span data-ttu-id="ff5bb-109">Przykładowa strona jest wyświetlana w programie IE, która jest ustawiona na język angielski, a następnie w przeglądarce FireFox, która jest ustawiona do korzystania z języka francuskiego.</span><span class="sxs-lookup"><span data-stu-id="ff5bb-109">The sample page is viewed in IE which is set to English and then in FireFox which is set to use French.</span></span> <span data-ttu-id="ff5bb-110">Na koniec należy omówić opcję przenoszenia definicji pamięci podręcznej do CacheProfile w pliku Web. config.</span><span class="sxs-lookup"><span data-stu-id="ff5bb-110">Finally, the option to move the cache definition to a CacheProfile in the web.config file is discussed.</span></span>

[<span data-ttu-id="ff5bb-111">&#9654;Obejrzyj wideo (12 minut)</span><span class="sxs-lookup"><span data-stu-id="ff5bb-111">&#9654; Watch video (12 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header)

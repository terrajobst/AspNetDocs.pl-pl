---
uid: web-forms/videos/how-do-i/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header
title: '[Jak mogę]  Strony ASP.NET na podstawie informacji w nagłówku HTTP w pamięci podręcznej | Dokumentacja firmy Microsoft'
author: rick-anderson
description: W tym wideo pikseli Chris pokazuje, jak zachować strony w pamięci podręcznej danych wyjściowych platformy ASP.NET na podstawie informacji w nagłówku HTTP strony. Pierwszy, potencjalne nagłówków HTTP...
ms.author: riande
ms.date: 02/26/2009
ms.assetid: 0f8df1bd-080a-4eeb-980c-c2fbb05d30c2
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header
msc.type: video
ms.openlocfilehash: 79c27f39793a4a3a94ea412838fb3844579e874d
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/17/2019
ms.locfileid: "59404147"
---
# <a name="how-do-i--cache-an-aspnet-page-based-upon-information-in-the-http-header"></a><span data-ttu-id="e1cd4-104">[Jak mogę]  Strony ASP.NET na podstawie informacji w nagłówku HTTP w pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="e1cd4-104">[How Do I:]  Cache an ASP.NET Page Based Upon Information in the HTTP Header</span></span>

<span data-ttu-id="e1cd4-105">przez [Chris pikseli](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="e1cd4-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="e1cd4-106">W tym wideo pikseli Chris pokazuje, jak zachować strony w pamięci podręcznej danych wyjściowych platformy ASP.NET na podstawie informacji w nagłówku HTTP strony.</span><span class="sxs-lookup"><span data-stu-id="e1cd4-106">In this video Chris Pels shows how to keep a page in the ASP.NET output cache based upon information in the page's HTTP header.</span></span> <span data-ttu-id="e1cd4-107">Najpierw są przeglądane potencjalnych wartości nagłówka HTTP.</span><span class="sxs-lookup"><span data-stu-id="e1cd4-107">First, the potential HTTP header values are reviewed.</span></span> <span data-ttu-id="e1cd4-108">Następnie przykładowa strona jest tworzony i następnie dyrektywy OutputCache jest używana z atrybutu VaryByHeader, który zawiera wartość "Zaakceptuj — język", nagłówek HTTP do kontrolowania, buforowanie, oparte na język przeglądarki użytkownika.</span><span class="sxs-lookup"><span data-stu-id="e1cd4-108">Then, a sample page is created and then the OutputCache directive is used with the VaryByHeader attribute which contains a value "accept-language", an HTTP header, to control caching based upon the language of the user's browser.</span></span> <span data-ttu-id="e1cd4-109">Strona przykładu zostanie wyświetlony w programie Internet Explorer, która została ustawiona na angielski, a następnie w przeglądarce FireFox, które jest ustawione na język francuski.</span><span class="sxs-lookup"><span data-stu-id="e1cd4-109">The sample page is viewed in IE which is set to English and then in FireFox which is set to use French.</span></span> <span data-ttu-id="e1cd4-110">Na koniec omówiono możliwość skorzystania z definicją w pamięci podręcznej z CacheProfile w pliku web.config.</span><span class="sxs-lookup"><span data-stu-id="e1cd4-110">Finally, the option to move the cache definition to a CacheProfile in the web.config file is discussed.</span></span>

[<span data-ttu-id="e1cd4-111">&#9654;Obejrzyj film wideo (12 minut)</span><span class="sxs-lookup"><span data-stu-id="e1cd4-111">&#9654; Watch video (12 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header)

---
uid: web-pages/overview/performance-and-traffic/bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site
title: Dzielenie i Minifikacja zasobów w witrynie ASP.NET Web Pages (Razor) | Microsoft Docs
author: microsoft
description: Tworzenie i minifikacjanie jest sposobem na szybsze Udostępnianie witryny. Zgrupowanie umożliwia łączenie wielu plików JavaScript (. js) lub wielu kaskadowych arkuszy stylów (...
ms.author: riande
ms.date: 06/21/2012
ms.assetid: 8906f1e9-4b66-4a03-8e8a-9e9debf8ed91
msc.legacyurl: /web-pages/overview/performance-and-traffic/bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site
msc.type: authoredcontent
ms.openlocfilehash: 5e42111ad71ec65581e56c73822e23ecd5fcbd58
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78635710"
---
# <a name="bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="bb8d9-104">Tworzenie pakietów i minifikacja zasobów w witrynie ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="bb8d9-104">Bundling and Minifying Assets in an ASP.NET Web Pages (Razor) Site</span></span>

<span data-ttu-id="bb8d9-105">przez [firmę Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="bb8d9-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="bb8d9-106">Tworzenie i minifikacjanie jest sposobem na szybsze Udostępnianie witryny.</span><span class="sxs-lookup"><span data-stu-id="bb8d9-106">Bundling and minification are ways to make your site faster.</span></span> <span data-ttu-id="bb8d9-107">Zgrupowanie umożliwia łączenie wielu plików JavaScript ( *. js*) lub wielu plików kaskadowych arkuszy stylów (*CSS*), dzięki czemu można je pobrać jako jednostkę, a nie pojedynczo.</span><span class="sxs-lookup"><span data-stu-id="bb8d9-107">Bundling lets you combine multiple JavaScript (*.js*) files or multiple cascading style sheet (*.css*) files so that they can be downloaded as a unit, rather than one at a time.</span></span> <span data-ttu-id="bb8d9-108">Minifikacja dzieli biały znak i wykonuje inne typy kompresji, aby pobrane pliki były możliwie małe.</span><span class="sxs-lookup"><span data-stu-id="bb8d9-108">Minification squeezes out whitespace and performs other types of compression to make the downloaded files as small a possible.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="bb8d9-109">Wydanie RC ASP.NET Web Pages 2 nie obsługuje grupowania i minifikacja, ponieważ pakiet zawierający wymagane elementy nie jest jeszcze dostępny w programie Microsoft WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="bb8d9-109">The RC release of ASP.NET Web Pages 2 does not support bundling and minification because the package that contains the required elements is not yet available in Microsoft WebMatrix.</span></span> <span data-ttu-id="bb8d9-110">Przepraszamy za te niedogodności.</span><span class="sxs-lookup"><span data-stu-id="bb8d9-110">We apologize for this inconvenience.</span></span> <span data-ttu-id="bb8d9-111">Oczekuje się, że pakiet jest dostępny w ostatecznej wersji ASP.NET Web Pages 2 i WebMatrix 2.</span><span class="sxs-lookup"><span data-stu-id="bb8d9-111">The package is expected to be available in the final release of ASP.NET Web Pages 2 and WebMatrix 2.</span></span>

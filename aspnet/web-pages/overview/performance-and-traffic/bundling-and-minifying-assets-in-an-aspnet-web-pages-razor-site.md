---
uid: web-pages/overview/performance-and-traffic/bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site
title: Tworzenie pakietów i Minifikacja zasobów we wzorcu ASP.NET Web Pages (Razor) lokacji | Dokumentacja firmy Microsoft
author: microsoft
description: Tworzenie pakietów i minimalizowanie są sposoby szybciej witryny. Tworzenie pakietów pozwala możesz połączyć wiele plików JavaScript (js) lub wielu kaskadowy arkusz stylów (...)
ms.author: riande
ms.date: 06/21/2012
ms.assetid: 8906f1e9-4b66-4a03-8e8a-9e9debf8ed91
msc.legacyurl: /web-pages/overview/performance-and-traffic/bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site
msc.type: authoredcontent
ms.openlocfilehash: 5e42111ad71ec65581e56c73822e23ecd5fcbd58
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/09/2019
ms.locfileid: "59400454"
---
# <a name="bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="57794-104">Tworzenie pakietów i minifikacja zasobów w witrynie ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="57794-104">Bundling and Minifying Assets in an ASP.NET Web Pages (Razor) Site</span></span>

<span data-ttu-id="57794-105">przez [firmy Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="57794-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="57794-106">Tworzenie pakietów i minimalizowanie są sposoby szybciej witryny.</span><span class="sxs-lookup"><span data-stu-id="57794-106">Bundling and minification are ways to make your site faster.</span></span> <span data-ttu-id="57794-107">Tworzenie pakietów pozwala łączyć wiele JavaScript (*js*) plików lub wielu kaskadowy arkusz stylów (*.css*) plików, dzięki czemu mogą być pobierane jako jednostkę, a nie po kolei.</span><span class="sxs-lookup"><span data-stu-id="57794-107">Bundling lets you combine multiple JavaScript (*.js*) files or multiple cascading style sheet (*.css*) files so that they can be downloaded as a unit, rather than one at a time.</span></span> <span data-ttu-id="57794-108">Minimalizowanie powstaną się odstępów i wykonuje inne typy kompresji możliwe zapewnienie pobranych plików niewielkie.</span><span class="sxs-lookup"><span data-stu-id="57794-108">Minification squeezes out whitespace and performs other types of compression to make the downloaded files as small a possible.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="57794-109">Wersji RC programu ASP.NET Web Pages 2 nie obsługuje tworzenie pakietów i minimalizowanie, ponieważ pakiet, który zawiera wymagane elementy nie jest jeszcze dostępna w Microsoft WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="57794-109">The RC release of ASP.NET Web Pages 2 does not support bundling and minification because the package that contains the required elements is not yet available in Microsoft WebMatrix.</span></span> <span data-ttu-id="57794-110">Przepraszamy za niedogodności.</span><span class="sxs-lookup"><span data-stu-id="57794-110">We apologize for this inconvenience.</span></span> <span data-ttu-id="57794-111">Pakiet powinien być dostępny w ostatecznej wersji programu ASP.NET Web Pages 2 i programu WebMatrix 2.</span><span class="sxs-lookup"><span data-stu-id="57794-111">The package is expected to be available in the final release of ASP.NET Web Pages 2 and WebMatrix 2.</span></span>

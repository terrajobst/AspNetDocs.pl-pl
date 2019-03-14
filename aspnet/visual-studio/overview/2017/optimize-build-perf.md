---
uid: visual-studio/overview/2017/optimize-build-perf
title: Optymalizacja wydajności kompilacji dla rozwiązania
author: AngelosP
description: Optymalizacja wydajności kompilacji dla rozwiązania
ms.author: riande
ms.date: 08/29/2018
msc.type: authoredcontent
ms.openlocfilehash: c1a5cf5e59374b4c0dd7150c5dd62fbde42af555
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57073802"
---
# <a name="optimize-build-performance-for-solution"></a><span data-ttu-id="e5dfc-103">Optymalizacja wydajności kompilacji dla rozwiązania</span><span class="sxs-lookup"><span data-stu-id="e5dfc-103">Optimize build performance for solution</span></span>

<span data-ttu-id="e5dfc-104">Visual Studio 2017 15.8 lub nowszych obejmują element menu: **Tworzenie** > **kompilacji platformy ASP.NET** > **zoptymalizować wydajność kompilacji dla rozwiązania**.</span><span class="sxs-lookup"><span data-stu-id="e5dfc-104">Visual Studio 2017 15.8 or later include a menu item: **Build** > **ASP.NET Compilation** > **Optimize Build Performance for Solution**.</span></span>

![Zrzut ekranu przedstawiający nowy element menu](optimize-build-perf/_static/optimize-build-performance-for-solution.png)

<span data-ttu-id="e5dfc-106">Program ASP.NET kompiluje swoją opinię w czasie wykonywania, co oznacza, że projektu programu ASP.NET niesie ze sobą kopię kompilator.</span><span class="sxs-lookup"><span data-stu-id="e5dfc-106">ASP.NET compiles its views at runtime, which means that an ASP.NET project carries with it a copy of the compiler.</span></span> <span data-ttu-id="e5dfc-107">Jednak na komputerze dewelopera podczas kopiowania kompilatora nie jest zgodna kopia programu Visual Studio, kompilacja jest wpływ na wydajność rzędu kilku 1 – 3 sekundy na kompilacji przyrostowej.</span><span class="sxs-lookup"><span data-stu-id="e5dfc-107">However on a developer machine when the copy of the compiler doesn't match Visual Studio's copy, build performance is impacted on the order of 1-3 seconds per incremental build.</span></span> <span data-ttu-id="e5dfc-108">Ta funkcja aktualizacji projektu kopię kompilator, aby dopasować programu Visual Studio, która zwykle przyspiesza kompilacje przyrostowe.</span><span class="sxs-lookup"><span data-stu-id="e5dfc-108">This feature updates your project's copy of the compiler to match Visual Studio's, which usually speeds up incremental builds.</span></span>

<span data-ttu-id="e5dfc-109">**To ma zastosowanie do środowiska ASP.NET Framework 4.7.1 lub później wyłącznie dla projektów, natomiast nie odnoszą się do platformy ASP.NET Core.**</span><span class="sxs-lookup"><span data-stu-id="e5dfc-109">**This is applicable to ASP.NET Framework 4.7.1 or later projects only, it does not apply to ASP.NET Core.**</span></span>

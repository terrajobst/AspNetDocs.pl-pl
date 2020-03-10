---
uid: visual-studio/overview/2017/optimize-build-perf
title: Optymalizacja wydajności kompilacji dla rozwiązania
author: AngelosP
description: Optymalizacja wydajności kompilacji dla rozwiązania
ms.author: riande
ms.date: 08/29/2018
msc.type: authoredcontent
ms.openlocfilehash: c1a5cf5e59374b4c0dd7150c5dd62fbde42af555
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622641"
---
# <a name="optimize-build-performance-for-solution"></a><span data-ttu-id="f9a44-103">Optymalizacja wydajności kompilacji dla rozwiązania</span><span class="sxs-lookup"><span data-stu-id="f9a44-103">Optimize build performance for solution</span></span>

<span data-ttu-id="f9a44-104">Program Visual Studio 2017 15,8 lub nowszy zawiera element menu: **Build** > **kompilacja ASP.NET** > **Optymalizuj wydajność kompilacji dla rozwiązania**.</span><span class="sxs-lookup"><span data-stu-id="f9a44-104">Visual Studio 2017 15.8 or later include a menu item: **Build** > **ASP.NET Compilation** > **Optimize Build Performance for Solution**.</span></span>

![Zrzut ekranu przedstawiający nowy element menu](optimize-build-perf/_static/optimize-build-performance-for-solution.png)

<span data-ttu-id="f9a44-106">ASP.NET kompiluje swoje widoki w czasie wykonywania, co oznacza, że projekt ASP.NET przeprowadzi z niego kopię kompilatora.</span><span class="sxs-lookup"><span data-stu-id="f9a44-106">ASP.NET compiles its views at runtime, which means that an ASP.NET project carries with it a copy of the compiler.</span></span> <span data-ttu-id="f9a44-107">Jednak na komputerze dewelopera, gdy kopia kompilatora nie jest zgodna z kopią programu Visual Studio, wydajność kompilacji ma wpływ na 1-3 sekund na kompilację przyrostową.</span><span class="sxs-lookup"><span data-stu-id="f9a44-107">However on a developer machine when the copy of the compiler doesn't match Visual Studio's copy, build performance is impacted on the order of 1-3 seconds per incremental build.</span></span> <span data-ttu-id="f9a44-108">Ta funkcja aktualizuje kopię kompilatora projektu, tak aby pasowała do programu Visual Studio, co zazwyczaj przyspiesza Kompilacje przyrostowe.</span><span class="sxs-lookup"><span data-stu-id="f9a44-108">This feature updates your project's copy of the compiler to match Visual Studio's, which usually speeds up incremental builds.</span></span>

<span data-ttu-id="f9a44-109">**Ma to zastosowanie tylko do ASP.NET Framework 4.7.1 lub nowszego, nie ma zastosowania do ASP.NET Core.**</span><span class="sxs-lookup"><span data-stu-id="f9a44-109">**This is applicable to ASP.NET Framework 4.7.1 or later projects only, it does not apply to ASP.NET Core.**</span></span>

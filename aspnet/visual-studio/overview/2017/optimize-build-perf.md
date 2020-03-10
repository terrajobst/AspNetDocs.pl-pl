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
# <a name="optimize-build-performance-for-solution"></a>Optymalizacja wydajności kompilacji dla rozwiązania

Program Visual Studio 2017 15,8 lub nowszy zawiera element menu: **Build** > **kompilacja ASP.NET** > **Optymalizuj wydajność kompilacji dla rozwiązania**.

![Zrzut ekranu przedstawiający nowy element menu](optimize-build-perf/_static/optimize-build-performance-for-solution.png)

ASP.NET kompiluje swoje widoki w czasie wykonywania, co oznacza, że projekt ASP.NET przeprowadzi z niego kopię kompilatora. Jednak na komputerze dewelopera, gdy kopia kompilatora nie jest zgodna z kopią programu Visual Studio, wydajność kompilacji ma wpływ na 1-3 sekund na kompilację przyrostową. Ta funkcja aktualizuje kopię kompilatora projektu, tak aby pasowała do programu Visual Studio, co zazwyczaj przyspiesza Kompilacje przyrostowe.

**Ma to zastosowanie tylko do ASP.NET Framework 4.7.1 lub nowszego, nie ma zastosowania do ASP.NET Core.**

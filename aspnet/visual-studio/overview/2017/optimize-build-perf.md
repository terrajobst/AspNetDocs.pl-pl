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
# <a name="optimize-build-performance-for-solution"></a>Optymalizacja wydajności kompilacji dla rozwiązania

Visual Studio 2017 15.8 lub nowszych obejmują element menu: **Tworzenie** > **kompilacji platformy ASP.NET** > **zoptymalizować wydajność kompilacji dla rozwiązania**.

![Zrzut ekranu przedstawiający nowy element menu](optimize-build-perf/_static/optimize-build-performance-for-solution.png)

Program ASP.NET kompiluje swoją opinię w czasie wykonywania, co oznacza, że projektu programu ASP.NET niesie ze sobą kopię kompilator. Jednak na komputerze dewelopera podczas kopiowania kompilatora nie jest zgodna kopia programu Visual Studio, kompilacja jest wpływ na wydajność rzędu kilku 1 – 3 sekundy na kompilacji przyrostowej. Ta funkcja aktualizacji projektu kopię kompilator, aby dopasować programu Visual Studio, która zwykle przyspiesza kompilacje przyrostowe.

**To ma zastosowanie do środowiska ASP.NET Framework 4.7.1 lub później wyłącznie dla projektów, natomiast nie odnoszą się do platformy ASP.NET Core.**

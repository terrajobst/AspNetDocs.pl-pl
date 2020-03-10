---
uid: mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-vb
title: Omówienie filtrów akcji (VB) | Microsoft Docs
author: microsoft
description: Celem tego samouczka jest wyjaśnienie filtrów akcji. Filtr akcji jest atrybutem, który można zastosować do akcji kontrolera — lub całego kontrolera...
ms.author: riande
ms.date: 10/16/2008
ms.assetid: e83812f2-c53e-4a43-a7c1-d64c59ecf694
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-vb
msc.type: authoredcontent
ms.openlocfilehash: 263658231ccaa7863508c691a3570bc00b9e8039
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78601172"
---
# <a name="understanding-action-filters-vb"></a>Objaśnienie filtrów akcji (VB)

przez [firmę Microsoft](https://github.com/microsoft)

[Pobierz plik PDF](https://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_14_VB.pdf)

> Celem tego samouczka jest wyjaśnienie filtrów akcji. Filtr akcji jest atrybutem, który można zastosować do akcji kontrolera--lub całego kontrolera — który modyfikuje sposób wykonywania akcji.

## <a name="understanding-action-filters"></a>Omówienie filtrów akcji

Celem tego samouczka jest wyjaśnienie filtrów akcji. Filtr akcji jest atrybutem, który można zastosować do akcji kontrolera--lub całego kontrolera — który modyfikuje sposób wykonywania akcji. Struktura ASP.NET MVC zawiera kilka filtrów akcji:

- OutputCache — ta akcja filtr umożliwia buforowanie danych wyjściowych akcji kontrolera przez określony czas.
- HandleError — ten filtr akcji obsługuje błędy wywoływane, gdy akcja kontrolera zostanie wykonana.
- Autoryzuj — ten filtr akcji umożliwia ograniczenie dostępu do określonego użytkownika lub roli.

Możesz również utworzyć własne niestandardowe filtry akcji. Na przykład można utworzyć niestandardowy filtr akcji w celu zaimplementowania niestandardowego systemu uwierzytelniania. Można też utworzyć filtr akcji, który modyfikuje dane widoku zwrócone przez akcję kontrolera.

W tym samouczku dowiesz się, jak skompilować filtr akcji od podstaw. Tworzymy filtr akcji dziennika, który rejestruje różne etapy przetwarzania akcji w oknie danych wyjściowych programu Visual Studio.

### <a name="using-an-action-filter"></a>Korzystanie z filtru akcji

Filtr akcji jest atrybutem. Większość filtrów akcji można zastosować do poszczególnych akcji kontrolera lub całego kontrolera.

Na przykład kontroler danych na liście 1 uwidacznia akcję o nazwie `Index()`, która zwraca bieżący czas. Ta akcja ma `OutputCache` filtr akcji. Ten filtr powoduje, że wartość zwracana przez akcję jest buforowana przez 10 sekund.

**Lista 1 — `Controllers\DataController.vb`**

[!code-vb[Main](understanding-action-filters-vb/samples/sample1.vb)]

Jeśli akcja `Index()` zostanie wielokrotnie wywołana przez wprowadzenie adresu URL/Data/Index na pasku adresu przeglądarki i naciśnięcie przycisku odświeżania wiele razy, zobaczysz ten sam czas przez 10 sekund. Dane wyjściowe akcji `Index()` są buforowane przez 10 sekund (patrz rysunek 1).

[![pamięci podręcznej](understanding-action-filters-vb/_static/image2.png)](understanding-action-filters-vb/_static/image1.png)

**Ilustracja 01**: buforowany czas ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-action-filters-vb/_static/image3.png))

W przypadku wybrania jednego filtru akcji w ramach listy 1 do metody `Index()` jest stosowany filtr akcji `OutputCache`. W razie potrzeby można zastosować wiele filtrów akcji do tej samej akcji. Na przykład możesz chcieć zastosować filtry akcji `OutputCache` i `HandleError` do tej samej akcji.

Na liście 1 Filtr akcji `OutputCache` jest stosowany do akcji `Index()`. Ten atrybut można również zastosować do samej klasy `DataController`. W takim przypadku wynik zwrócony przez wszystkie akcje uwidocznione przez kontroler jest buforowany przez 10 sekund.

### <a name="the-different-types-of-filters"></a>Różne typy filtrów

Platforma ASP.NET MVC obsługuje cztery różne typy filtrów:

1. Filtry autoryzacji — implementuje atrybut `IAuthorizationFilter`.
2. Filtry akcji — implementuje atrybut `IActionFilter`.
3. Filtry wyników — implementuje atrybut `IResultFilter`.
4. Filtry wyjątków — implementuje atrybut `IExceptionFilter`.

Filtry są wykonywane w podanej kolejności. Na przykład filtry autoryzacji są zawsze wykonywane, zanim filtry akcji i filtry wyjątków są zawsze wykonywane po każdym innym typie filtru.

Filtry autoryzacji służą do implementowania uwierzytelniania i autoryzacji dla akcji kontrolera. Na przykład filtr Autoryzuj jest przykładem filtru autoryzacji.

Filtry akcji zawierają logikę wykonywaną przed wykonaniem akcji kontrolera i po niej. Możesz użyć filtru akcji, na przykład, aby zmodyfikować dane widoku, które zwraca akcja kontrolera.

Filtry wyników zawierają logikę wykonywaną przed i po wykonaniu wyniku widoku. Na przykład możesz chcieć zmodyfikować wynik widoku bezpośrednio przed renderowaniem widoku w przeglądarce.

Filtry wyjątków to ostatni typ filtru do uruchomienia. Możesz użyć filtru wyjątków, aby obsłużyć błędy wywoływane przez akcje kontrolera lub wyniki akcji kontrolera. Można także użyć filtrów wyjątków, aby rejestrować błędy.

Każdy inny typ filtru jest wykonywany w określonej kolejności. Jeśli chcesz kontrolować kolejność, w której są wykonywane filtry tego samego typu, można ustawić właściwość kolejność filtru.

Klasa bazowa dla wszystkich filtrów akcji jest klasą `System.Web.Mvc.FilterAttribute`. Jeśli chcesz zaimplementować określony typ filtru, należy utworzyć klasę, która dziedziczy z klasy bazowego filtru i implementuje co najmniej jeden interfejs IAuthorizationFilter, IActionFilter, IResultFilter lub ExceptionFilter.

### <a name="the-base-actionfilterattribute-class"></a>Podstawowa klasa ActionFilterAttribute

Aby ułatwić implementowanie niestandardowego filtru akcji, struktura ASP.NET MVC zawiera podstawową klasę `ActionFilterAttribute`. Ta klasa implementuje zarówno interfejsy `IActionFilter`, jak i `IResultFilter` i dziedziczy z klasy `Filter`.

Terminologia nie jest całkowicie spójna. Technicznie Klasa, która dziedziczy z klasy ActionFilterAttribute, jest zarówno filtrem akcji, jak i filtrem wynikowym. Jednak w tym sensie filtr akcji programu Word służy do odwoływania się do dowolnego typu filtru w strukturze ASP.NET MVC.

Podstawowa klasa ActionFilterAttribute ma następujące metody, które można przesłonić:

- OnActionExecuting — ta metoda jest wywoływana przed wykonaniem akcji kontrolera.
- OnActionExecuted — ta metoda jest wywoływana po wykonaniu akcji kontrolera.
- OnResultExecuting — ta metoda jest wywoływana przed wykonaniem wyniku akcji kontrolera.
- OnResultExecuted — ta metoda jest wywoływana po wykonaniu wyniku akcji kontrolera.

W następnej sekcji zobaczymy, jak można zaimplementować każdą z tych różnych metod.

### <a name="creating-a-log-action-filter"></a>Tworzenie filtru akcji dziennika

Aby zilustrować, jak można skompilować filtr akcji niestandardowych, utworzymy niestandardowy filtr akcji, który rejestruje etapy przetwarzania akcji kontrolera w oknie danych wyjściowych programu Visual Studio. Nasze `LogActionFilter` znajdują się na liście 2.

**Lista 2 — `ActionFilters\LogActionFilter.vb`**

[!code-vb[Main](understanding-action-filters-vb/samples/sample2.vb)]

Na liście 2 metody `OnActionExecuting()`, `OnActionExecuted()`, `OnResultExecuting()`i `OnResultExecuted()` wywołują metodę `Log()`. Nazwa metody i dane bieżącej trasy są przesyłane do metody `Log()`. Metoda `Log()` zapisuje komunikat do okna danych wyjściowych programu Visual Studio (patrz rysunek 2).

[![zapisywania do okna danych wyjściowych programu Visual Studio](understanding-action-filters-vb/_static/image5.png)](understanding-action-filters-vb/_static/image4.png)

**Ilustracja 02**. zapisywanie do okna danych wyjściowych programu Visual Studio ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](understanding-action-filters-vb/_static/image6.png))

Kontroler Home na liście 3 ilustruje, jak można zastosować filtr akcji dziennika do całej klasy kontrolera. Za każdym razem, gdy wszystkie akcje uwidocznione przez kontroler macierzysty są wywoływane — Metoda `Index()` lub metoda `About()` — etapy przetwarzania akcji są rejestrowane w oknie danych wyjściowych programu Visual Studio.

**Lista 3 — `Controllers\HomeController.vb`**

[!code-vb[Main](understanding-action-filters-vb/samples/sample3.vb)]

### <a name="summary"></a>Podsumowanie

W tym samouczku wprowadzono do ASP.NET filtrów akcji MVC. Zapoznaj się z czterema różnymi typami filtrów: filtry autoryzacji, filtry akcji, filtry wynikowe i filtry wyjątków. Zapoznaj się również z klasą `ActionFilterAttribute` podstawowej.

Wreszcie zawiesz się, jak zaimplementować prosty filtr akcji. Utworzyliśmy filtr akcji dziennika, który rejestruje etapy przetwarzania akcji kontrolera w oknie danych wyjściowych programu Visual Studio.

> [!div class="step-by-step"]
> [Poprzednie](asp-net-mvc-routing-overview-vb.md)
> [dalej](improving-performance-with-output-caching-vb.md)

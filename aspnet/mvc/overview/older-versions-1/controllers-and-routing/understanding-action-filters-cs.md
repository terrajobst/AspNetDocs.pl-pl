---
uid: mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-cs
title: Objaśnienie filtrów akcji (C#) | Dokumentacja firmy Microsoft
author: microsoft
description: Celem tego samouczka jest wyjaśnić filtrów akcji. Filtr akcji jest atrybut, który można zastosować do akcji kontrolera — lub całego kontrolera...
ms.author: riande
ms.date: 10/16/2008
ms.assetid: a94e4e81-40c1-47b7-8613-126a1a6cc93d
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-cs
msc.type: authoredcontent
ms.openlocfilehash: 6c7706d8252d5a0271f1b9243fa8eb282f722654
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57076388"
---
<a name="understanding-action-filters-c"></a>Objaśnienie filtrów akcji (C#)
====================
przez [firmy Microsoft](https://github.com/microsoft)

[Pobierz plik PDF](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_14_CS.pdf)

> Celem tego samouczka jest wyjaśnić filtrów akcji. Filtr akcji jest atrybut, który można zastosować do akcji kontrolera — lub cały kontroler — która modyfikuje sposób, w którym akcja jest wykonywana.


## <a name="understanding-action-filters"></a>Objaśnienie filtrów akcji

Celem tego samouczka jest wyjaśnić filtrów akcji. Filtr akcji jest atrybut, który można zastosować do akcji kontrolera — lub cały kontroler — która modyfikuje sposób, w którym akcja jest wykonywana. Platforma ASP.NET MVC zawiera wiele filtrów akcji:

- OutputCache — ten filtr akcji buforuje dane wyjściowe akcji kontrolera dla określonego przedziału czasu.
- HandleError — ten filtr akcji obsługuje błędy zgłaszane w przypadku wykonuje akcji kontrolera.
- Autoryzowanie — ten filtr akcji pozwala ograniczyć dostęp do określonego użytkownika lub roli.

Możesz również utworzyć własne filtry akcji niestandardowej. Na przykład można utworzyć filtr akcji niestandardowej, aby zaimplementować niestandardowy system uwierzytelniania. Możesz też utworzyć filtr akcji, która modyfikuje dane widoku, zwrócone przez akcji kontrolera.

W tym samouczku dowiesz się, jak do utworzenia filtru akcji od podstaw. Możemy utworzyć filtr akcji dziennika, która rejestruje różne etapy przetwarzania akcji w oknie programu Visual Studio danych wyjściowych.

### <a name="using-an-action-filter"></a>Przy użyciu filtru akcji

Filtr akcji jest atrybutem. Większość filtrów akcji można zastosować do akcji kontrolera indywidualnych lub całego kontrolera.

Na przykład kontroler danych w ofercie 1 udostępnia akcję o nazwie `Index()` , zwraca bieżącą godzinę. Ta akcja zostanie nadany `OutputCache` filtru akcji. Ten filtr powoduje, że wartość zwracana przez akcję przechowywanie w pamięci podręcznej przez 10 sekund.

**1 — Lista `Controllers\DataController.cs`**

[!code-csharp[Main](understanding-action-filters-cs/samples/sample1.cs)]

Jeśli wywołujesz wielokrotnie `Index()` akcję, wprowadzając adres URL/Data/indeksu w pasku adresu przeglądarki i naciskanie klawisza odświeżania przycisk wiele razy, w tym samym czasie będzie widocznych 10 sekund. Dane wyjściowe `Index()` akcji jest buforowana przez 10 sekund (patrz rysunek 1).


[![Czas pamięci podręcznej](understanding-action-filters-cs/_static/image2.png)](understanding-action-filters-cs/_static/image1.png)

**Rysunek 01**: Buforowane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-action-filters-cs/_static/image3.png))


W ofercie 1 filtr jednej akcji — `OutputCache` filtr akcji — jest stosowany do `Index()` metody. Jeśli potrzebujesz, można zastosować wiele filtrów akcji do tego samego działania. Na przykład możesz chcieć zastosowanie zarówno `OutputCache` i `HandleError` filtry akcji do tego samego działania.

W ofercie 1 `OutputCache` akcji filtr jest stosowany do `Index()` akcji. Można również zastosować ten atrybut `DataController` samej klasy. W takim przypadku wynik zwracany przez działanie udostępnianych przez kontroler będzie buforowana przez 10 sekund.

### <a name="the-different-types-of-filters"></a>Różne typy filtrów

Platforma ASP.NET MVC obsługuje cztery typy filtrów:

1. Filtry autoryzacji — implementuje `IAuthorizationFilter` atrybutu.
2. Filtry działań — implementuje `IActionFilter` atrybutu.
3. Wynik filtry — implementuje `IResultFilter` atrybutu.
4. Filtry wyjątków — implementuje `IExceptionFilter` atrybutu.

Filtry są wykonywane w kolejności podanej powyżej. Na przykład filtry autoryzacji są zawsze wykonywane przed filtry akcji, a filtry wyjątków są zawsze wykonywane po każdego typu filtru.

Filtry autoryzacji są używane do implementowania uwierzytelniania i autoryzacji dla akcji kontrolera. Na przykład filtr autoryzacji jest przykładem filtru autoryzacji.

Filtry akcji zawierają logikę, który jest wykonywany przed i po wykonaniu akcji kontrolera. Na przykład filtr akcji można użyć do modyfikowania danych widoku, która zwraca akcji kontrolera.

Filtry wyników zawiera logikę, który jest wykonywany przed i po wykonaniu wyniku widoku. Na przykład możesz chcieć zmodyfikować wynik widoku kliknij prawym przyciskiem myszy przed wyświetleniem tego widoku w przeglądarce.

Filtry wyjątków są ostatni typ filtru do uruchomienia. Można użyć filtru wyjątków do obsługi błędów zgłaszane przez akcji kontrolera lub wyników akcji kontrolera. Możesz również użyć filtry wyjątków rejestrować błędy.

Każdy typ filtru jest wykonywane w określonej kolejności. Jeśli chcesz kontrolować kolejność wykonywania filtrów tego samego typu można ustawić filtr właściwości Order.

Klasa bazowa dla wszystkich filtrów działania jest `System.Web.Mvc.FilterAttribute` klasy. Jeśli chcesz zaimplementować określonego typu filtr, a następnie należy utworzyć klasę, która dziedziczy z klasy bazowej filtru i implementuje jedną lub więcej `IAuthorizationFilter`, `IActionFilter`, `IResultFilter`, lub `IExceptionFilter` interfejsów.

### <a name="the-base-actionfilterattribute-class"></a>Klasa bazowa ActionFilterAttribute

Aby ułatwić zaimplementować filtr akcji niestandardowej, platforma ASP.NET MVC zawiera podstawy `ActionFilterAttribute` klasy. Ta klasa implementuje interfejsy `IActionFilter` i `IResultFilter` interfejsy i dziedziczy `Filter` klasy.

Terminologia, w tym miejscu nie jest całkowicie zgodne. Technicznie rzecz biorąc klasy, która dziedziczy z klasy ActionFilterAttribute jest zarówno filtr akcji, jak i filtru wyników. Jednak w tym sensie, luźne, filtru akcji programu word jest używana do odwoływania się do dowolnego typu filtru w platformę ASP.NET MVC.

Podstawa `ActionFilterAttribute` klasa ma następujących metod, które można przesłonić:

- OnActionExecuting — ta metoda jest wywoływana przed wykonaniem akcji kontrolera.
- OnActionExecuted — ta metoda jest wywoływana po wykonaniu akcji kontrolera.
- OnResultExecuting — ta metoda jest wywoływana przed wykonaniem wyniku akcji kontrolera.
- OnResultExecuted — ta metoda jest wywoływana po wykonaniu wyniku akcji kontrolera.

W następnej sekcji zobaczymy, jak można implementować każda z tych różnych metod.

### <a name="creating-a-log-action-filter"></a>Utworzenie filtru akcji dziennika

Aby zilustrować, jak tworzyć filtru akcji niestandardowej, utworzymy filtru akcji niestandardowej, która rejestruje etapy przetwarzania akcji kontrolera, w oknie programu Visual Studio danych wyjściowych. Nasze `LogActionFilter` znajduje się w ofercie 2.

**2 — Lista `ActionFilters\LogActionFilter.cs`**

[!code-csharp[Main](understanding-action-filters-cs/samples/sample2.cs)]

W ofercie 2 `OnActionExecuting()`, `OnActionExecuted()`, `OnResultExecuting()`, i `OnResultExecuted()` wywołania metody `Log()` metody. Nazwa metody i bieżące dane trasy jest przekazywany do `Log()` metody. `Log()` Metoda zapisuje komunikat w oknie programu Visual Studio danych wyjściowych (patrz rysunek 2).


[![Zapisywanie w oknie programu Visual Studio danych wyjściowych.](understanding-action-filters-cs/_static/image5.png)](understanding-action-filters-cs/_static/image4.png)

**Rysunek 02**: Zapisywanie w oknie programu Visual Studio danych wyjściowych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-action-filters-cs/_static/image6.png))


Kontrolera głównego w ofercie 3 ilustruje, jak można zastosować filtr akcji dziennika do klasy całego kontrolera. Zawsze, gdy wszystkie akcje, udostępnianych przez kontrolera głównego są wywoływane — albo `Index()` metody lub `About()` metodą — etapy przetwarzania działania są rejestrowane w oknie programu Visual Studio danych wyjściowych.

**3 — lista `Controllers\HomeController.cs`**

[!code-csharp[Main](understanding-action-filters-cs/samples/sample3.cs)]

### <a name="summary"></a>Podsumowanie

W tym samouczku zostały wprowadzone do platformy ASP.NET MVC filtry akcji. Przedstawia informacje na temat cztery typy filtrów: filtry autoryzacji, filtry akcji, filtry wyników i filtry wyjątków. Wiesz już o base `ActionFilterAttribute` klasy.

Na koniec pokazaliśmy ci, jak zaimplementować prostego filtru akcji. Utworzyliśmy dziennika filtr akcji, która rejestruje etapy przetwarzania akcji kontrolera, w oknie programu Visual Studio danych wyjściowych.

> [!div class="step-by-step"]
> [Poprzednie](asp-net-mvc-routing-overview-cs.md)
> [dalej](improving-performance-with-output-caching-cs.md)

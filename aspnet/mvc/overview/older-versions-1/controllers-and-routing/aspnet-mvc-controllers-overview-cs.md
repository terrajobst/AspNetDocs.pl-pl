---
uid: mvc/overview/older-versions-1/controllers-and-routing/aspnet-mvc-controllers-overview-cs
title: Omówienie kontrolera MVC ASP.NET (C#) | Microsoft Docs
author: StephenWalther
description: W tym samouczku Stephen Walther wprowadza do ASP.NET kontrolerów MVC. Dowiesz się, jak tworzyć nowe kontrolery i zwracać różne typy zasobów akcji...
ms.author: riande
ms.date: 02/16/2008
ms.assetid: b985c49a-3668-455c-a366-f85f6bc64b12
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/aspnet-mvc-controllers-overview-cs
msc.type: authoredcontent
ms.openlocfilehash: 1a287b37742400a17c2ed53cfd00bfb053b4f3d2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78544115"
---
# <a name="aspnet-mvc-controller-overview-c"></a>Omówienie kontrolera ASP.NET MVC (C#)

Autor [Stephen Walther](https://github.com/StephenWalther)

> W tym samouczku Stephen Walther wprowadza do ASP.NET kontrolerów MVC. Dowiesz się, jak tworzyć nowe kontrolery i zwracać różne typy wyników akcji.

W tym samouczku przedstawiono temat ASP.NET kontrolery MVC, akcje kontrolera i wyniki akcji. Po ukończeniu tego samouczka dowiesz się, w jaki sposób kontrolery są używane do kontrolowania sposobu, w jaki Goście współdziałają z witryną sieci Web ASP.NET MVC.

## <a name="understanding-controllers"></a>Omówienie kontrolerów

Kontrolery MVC są odpowiedzialne za odpowiadanie na żądania wysyłane do witryny sieci Web ASP.NET MVC. Każde żądanie przeglądarki jest zamapowane na określony kontroler. Załóżmy na przykład, że wprowadzasz następujący adres URL na pasku adresu przeglądarki:

`http://localhost/Product/Index/3`

W takim przypadku jest wywoływany kontroler o nazwie ProductController. ProductController jest odpowiedzialny za generowanie odpowiedzi na żądanie przeglądarki. Na przykład kontroler może zwrócić określony widok z powrotem do przeglądarki lub kontroler może przekierować użytkownika do innego kontrolera.

Lista 1 zawiera prosty kontroler o nazwie ProductController.

**Listing1 - Controllers\ProductController.cs**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample1.cs)]

Jak widać na liście 1, kontroler jest tylko klasą (Visual Basic .NET lub C# klasą). Kontroler jest klasą pochodzącą od podstawowej klasy System. Web. MVC. Controller. Ponieważ kontroler dziedziczy z tej klasy bazowej, kontroler dziedziczy kilka przydatnych metod bezpłatnie (omawiamy te metody w chwilę).

## <a name="understanding-controller-actions"></a>Informacje o akcjach kontrolera

Kontroler ujawnia akcje kontrolera. Akcja to metoda na kontrolerze, który jest wywoływany po wprowadzeniu określonego adresu URL na pasku adresu przeglądarki. Załóżmy na przykład, że tworzysz żądanie dla następującego adresu URL:

`http://localhost/Product/Index/3`

W tym przypadku Metoda index () jest wywoływana w klasie ProductController. Metoda index () jest przykładem akcji kontrolera.

Akcja kontrolera musi być publiczną metodą klasy kontrolera. C#Metody domyślnie są metodami prywatnymi. Należy pamiętać, że każda metoda publiczna dodawana do klasy kontrolera jest automatycznie uwidaczniana jako akcja kontrolera (należy zachować ostrożność, ponieważ działanie kontrolera może być wywoływane przez każdą z nich, wpisując prawidłowy adres URL na pasku adresu przeglądarki).

Istnieją pewne dodatkowe wymagania, które muszą być spełnione przez akcję kontrolera. Metoda używana jako akcja kontrolera nie może być przeciążona. Ponadto akcja kontrolera nie może być metodą statyczną. Oprócz tego, można użyć tylko dowolnej metody jako akcji kontrolera.

## <a name="understanding-action-results"></a>Informacje o wynikach akcji

Akcja kontrolera zwraca coś o nazwie " *wynik akcji*". Wynikiem akcji jest to, co akcja kontrolera zwraca w odpowiedzi na żądanie przeglądarki.

Platforma ASP.NET MVC obsługuje kilka typów wyników akcji, takich jak:

1. ViewResult — reprezentuje kod HTML i znacznik.
2. EmptyResult — reprezentuje brak wyniku.
3. RedirectResult — reprezentuje przekierowanie do nowego adresu URL.
4. JsonResult — reprezentuje wynik JavaScript Object Notation, który może być używany w aplikacji AJAX.
5. JavaScriptResult — reprezentuje skrypt JavaScript.
6. ContentResult — reprezentuje wynik tekstowy.
7. FileContentResult — reprezentuje plik do pobrania (z zawartością binarną).
8. FilePathResult — reprezentuje plik do pobrania (ze ścieżką).
9. FileStreamResult — reprezentuje plik do pobrania (ze strumieniem pliku).

Wszystkie te wyniki akcji dziedziczą z podstawowej klasy ActionResult.

W większości przypadków akcja kontrolera zwraca ViewResult. Na przykład akcja kontrolera indeksu w liście 2 zwraca ViewResult.

**Lista 2 — Controllers\BookController.cs**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample2.cs)]

Gdy akcja zwraca ViewResult, kod HTML jest zwracany do przeglądarki. Metoda index () w liście 2 zwraca widok o nazwie index do przeglądarki.

Zwróć uwagę, że akcja index () na liście 2 nie zwraca elementu ViewResult (). Zamiast tego Metoda View () klasy bazowej kontrolera jest wywoływana. Zwykle nie zwracasz wyniku działania bezpośrednio. Zamiast tego należy wywołać jedną z następujących metod klasy podstawowej kontrolera:

1. Widok — zwraca wynik akcji ViewResult.
2. Redirect — zwraca wynik akcji RedirectResult.
3. RedirectToAction — zwraca wynik akcji RedirectToRouteResult.
4. RedirectToRoute — zwraca wynik akcji RedirectToRouteResult.
5. JSON — zwraca wynik akcji JsonResult.
6. JavaScriptResult — zwraca JavaScriptResult.
7. Content — zwraca wynik akcji ContentResult.
8. Plik — zwraca FileContentResult, FilePathResult lub FileStreamResult w zależności od parametrów przekazaną do metody.

Tak więc, jeśli chcesz zwrócić widok do przeglądarki, wywoływana jest metoda View (). Jeśli chcesz przekierować użytkownika z jednej akcji kontrolera do innej, należy wywołać metodę RedirectToAction (). Na przykład akcja szczegóły () na liście 3 wyświetla widok lub przekierowuje użytkownika do akcji index () w zależności od tego, czy parametr ID ma wartość.

**Lista 3 — CustomerController.cs**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample3.cs)]

Wynik akcji ContentResult jest specjalny. Możesz użyć wyniku działania ContentResult, aby zwrócić wynik akcji w postaci zwykłego tekstu. Na przykład Metoda index () w liście 4 zwraca komunikat w postaci zwykłego tekstu, a nie jako HTML.

**Lista 4 — Controllers\StatusController.cs**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample4.cs)]

Po wywołaniu akcji StatusController. index () widok nie jest zwracany. Zamiast tego tekst nieprzetworzony "Hello world!" jest zwracany do przeglądarki.

Jeśli akcja kontrolera zwraca wynik, który nie jest wynikiem akcji — na przykład datę lub liczbę całkowitą, wynik jest zawijany automatycznie w ContentResult. Na przykład po wywołaniu akcji index () elementu WorkController na liście 5 jest to data, która jest automatycznie zwracana jako ContentResult.

**Lista 5 — WorkController.cs**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample5.cs)]

Akcja index () w liście 5 zwraca obiekt DateTime. Struktura ASP.NET MVC Konwertuje obiekt DateTime na ciąg i automatycznie zawija wartość DateTime w ContentResult. Przeglądarka otrzymuje datę i godzinę w postaci zwykłego tekstu.

## <a name="summary"></a>Podsumowanie

Celem tego samouczka było wprowadzenie do koncepcji kontrolerów MVC ASP.NET, akcji kontrolera i wyników akcji kontrolera. W pierwszej sekcji przedstawiono sposób dodawania nowych kontrolerów do projektu ASP.NET MVC. Następnie wiesz, jak publiczne metody kontrolera są ujawniane w programie Universe jako akcje kontrolera. Na koniec omawiamy różne typy wyników akcji, które mogą być zwracane z akcji kontrolera. W szczególności omówione zostało zwrócenie elementu ViewResult, RedirectToActionResult i ContentResult z akcji kontrolera.

> [!div class="step-by-step"]
> [Poprzednie](creating-an-action-vb.md)
> [dalej](creating-custom-routes-cs.md)

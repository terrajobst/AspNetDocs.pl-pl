---
uid: mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
title: Objaśnienie procesu wykonywania platformy ASP.NET MVC | Dokumentacja firmy Microsoft
author: microsoft
description: Dowiedz się, jak platforma ASP.NET MVC przetwarza żądanie przeglądarkę krok po kroku.
ms.author: riande
ms.date: 01/27/2009
ms.assetid: d1608db3-660d-4079-8c15-f452ff01f1db
msc.legacyurl: /mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
msc.type: authoredcontent
ms.openlocfilehash: 3a3bcf4b78e3b19fb4d293f67b68c3a790d05221
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57072215"
---
<a name="understanding-the-aspnet-mvc-execution-process"></a>Objaśnienie procesu wykonywania we wzorcu ASP.NET MVC
====================
przez [firmy Microsoft](https://github.com/microsoft)

> Dowiedz się, jak platforma ASP.NET MVC przetwarza żądanie przeglądarkę krok po kroku.


Najpierw przekazywania żądań do aplikacji sieci Web opartych na programie ASP.NET MVC **UrlRoutingModule** obiektu, który jest moduł protokołu HTTP. Ten moduł analizuje żądania i wykonuje wybór trasy. **UrlRoutingModule** obiektu wybiera pierwszy obiekt trasy, który pasuje do bieżącego żądania. (Obiekt trasy jest klasa, która implementuje **RouteBase**, i zazwyczaj to wystąpienie **trasy** klasy.) Jeśli brak tras zgodnych, **UrlRoutingModule** obiektu nic nie robi i umożliwia żądanie wracać do regularnego żądania programu ASP.NET lub IIS przetwarzania.

Z wybranego **trasy** obiektu **UrlRoutingModule** uzyskuje obiekt **IRouteHandler** obiekt, który jest skojarzony z **trasy**obiektu. Zazwyczaj w aplikacji MVC, będzie to wystąpienie **MvcRouteHandler**. **IRouteHandler** tworzy wystąpienie **IHttpHandler** obiektu i przekazuje je **IHttpContext** obiektu. Domyślnie **IHttpHandler** wystąpienia dla platformy MVC jest **MvcHandler** obiektu. **MvcHandler** obiektu następnie wybiera kontrolera, który będzie ostatecznie obsługiwać żądania.

> [!NOTE]
> Po uruchomieniu aplikacji sieci Web platformy ASP.NET MVC w usługach IIS 7.0, bez rozszerzenia nazwy pliku jest wymagana dla projektów MVC. W usługach IIS 6.0, program obsługi wymaga jednak mapowania rozszerzenia nazwy pliku MVC do biblioteki DLL ISAPI programu ASP.NET.


Moduł i procedury obsługi są punktami wejściowymi umożliwiającymi platformę ASP.NET MVC. Wykonują następujące czynności:

- Wybierz odpowiedni kontroler w aplikacji sieci Web MVC.
- Uzyskaj wystąpienia określonego kontrolera.
- Wywołaj kontrolera **Execute** metody.

Poniższa lista zawiera etapy wykonywania dla projektu sieci Web MVC:

- Odbieranie pierwszego żądania aplikacji 

    - W pliku Global.asax **trasy** obiekty są dodawane do **RouteTable** obiektu.
- Wykonaj routingu 

    - **UrlRoutingModule** modułu używa pierwszego dopasowania **trasy** obiektu **RouteTable** kolekcji, aby utworzyć **RouteData** obiekt, który następnie używane w celu utworzenia **RequestContext** (**IHttpContext**) obiektu.
- Utwórz procedurę obsługi żądania MVC 

    - **MvcRouteHandler** obiektu tworzy wystąpienie **MvcHandler** klasy i przekazuje je **RequestContext** wystąpienia.
- Tworzenie kontrolera 

    - **MvcHandler** obiektu używa **RequestContext** wystąpienia, aby zidentyfikować **IControllerFactory** obiektu (zazwyczaj wystąpienie  **DefaultControllerFactory** klasy) do tworzenia wystąpienia kontrolera za pomocą.
- Wykonaj controller — **MvcHandler** wystąpienia wywołuje kontroler s **Execute** metody. |
- Wywołaj akcję 

    - Większość kontrolerów dziedziczyć **kontrolera** klasy bazowej. Dla kontrolerów, które to robi **ControllerActionInvoker** określa obiekt, który jest skojarzony z kontrolerem, metody akcji, które klasy kontrolera do wywołania, a następnie wywołuje tę metodę.
- Wynik wykonania 

    - Metoda typowych akcji może odbierać dane wejściowe użytkownika przygotowywania danych właściwą odpowiedź i wykonujących wynik, zwracając typ wyniku. Typy wbudowane wyników, które mogą być wykonywane są następujące: **ViewResult** (który renderuje widok, to typ wyniku większość często używanych), **RedirectToRouteResult**, **RedirectResult**, **ContentResult**,  **JsonResult**, i **EmptyResult**.

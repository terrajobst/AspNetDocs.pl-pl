---
uid: mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
title: Informacje o procesie wykonywania ASP.NET MVC | Microsoft Docs
author: microsoft
description: Dowiedz się, w jaki sposób platforma MVC ASP.NET przetwarza żądanie przeglądarki, krok po kroku.
ms.author: riande
ms.date: 01/27/2009
ms.assetid: d1608db3-660d-4079-8c15-f452ff01f1db
msc.legacyurl: /mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
msc.type: authoredcontent
ms.openlocfilehash: 28940947253e0af43886cf1231f8aaf4615526cc
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78541518"
---
# <a name="understanding-the-aspnet-mvc-execution-process"></a>Objaśnienie procesu wykonywania we wzorcu ASP.NET MVC

przez [firmę Microsoft](https://github.com/microsoft)

> Dowiedz się, w jaki sposób platforma MVC ASP.NET przetwarza żądanie przeglądarki, krok po kroku.

Żądania do aplikacji sieci Web opartej na ASP.NET MVC najpierw przekazywane przez obiekt **UrlRoutingModule** , który jest modułem http. Ten moduł analizuje żądanie i wykonuje wybór trasy. Obiekt **UrlRoutingModule** wybiera pierwszy obiekt trasy, który jest zgodny z bieżącym żądaniem. (Obiekt trasy jest klasą, która implementuje **RouteBase**, i jest zwykle wystąpieniem klasy **trasy** ). Jeśli żadne trasy nie pasują do siebie, obiekt **UrlRoutingModule** nie robi nic i pozwala na żądanie powraca do regularnego przetwarzania żądań ASP.NET lub IIS.

Z wybranego obiektu **trasy** obiekt **UrlRoutingModule** uzyskuje obiekt **IRouteHandler** , który jest skojarzony z obiektem **Route** . Zazwyczaj w aplikacji MVC będzie to wystąpienie **MvcRouteHandler**. Wystąpienie **IRouteHandler** tworzy obiekt **IHttpHandler** i przekazuje go do obiektu **IHttpContext** . Domyślnie wystąpienie **IHttpHandler** dla MVC jest obiektem **MvcHandler** . Obiekt **MvcHandler** wybiera kontroler, który ostatecznie obsłuży żądanie.

> [!NOTE]
> Gdy aplikacja sieci Web ASP.NET MVC jest uruchamiana w usługach IIS 7,0, dla projektów MVC nie jest wymagane rozszerzenie nazwy pliku. Jednak w usługach IIS 6,0 program obsługi wymaga zamapowania rozszerzenia nazwy pliku MVC na ASP.NET ISAPI DLL.

Moduł i program obsługi są punktami wejścia do struktury ASP.NET MVC. Wykonują następujące czynności:

- Wybierz odpowiedni kontroler w aplikacji sieci Web MVC.
- Uzyskanie określonego wystąpienia kontrolera.
- Wywołaj metodę **Execute** kontrolera.

Poniżej przedstawiono listę etapów wykonywania dla projektu sieci Web MVC:

- Odbieraj pierwsze żądanie dla aplikacji 

    - W pliku Global. asax obiekty **tras** są dodawane do obiektu **Route** .
- Wykonaj Routing 

    - Moduł **UrlRoutingModule** używa pierwszego dopasowanego obiektu **trasy** w kolekcji **Routes** w celu utworzenia obiektu **RouteData** , który następnie używa do tworzenia obiektu **RequestContext** (**IHttpContext**).
- Utwórz procedurę obsługi żądania MVC 

    - Obiekt **MvcRouteHandler** tworzy wystąpienie klasy **MvcHandler** i przekazuje je do wystąpienia **RequestContext** .
- Utwórz kontroler 

    - Obiekt **MvcHandler** używa wystąpienia **RequestContext** do identyfikowania obiektu **IControllerFactory** (zwykle wystąpienie klasy **DefaultControllerFactory** ) do utworzenia wystąpienia kontrolera za pomocą.
- Wykonaj kontroler — wystąpienie **MvcHandler** wywołuje metodę **Execute** sterownika. |
- Akcja wywołania 

    - Większość kontrolerów dziedziczy z klasy podstawowej **kontrolera** . Dla kontrolerów, które to powodują, obiekt **ControllerActionInvoker** , który jest skojarzony z kontrolerem określa metodę akcji klasy kontrolera do wywołania, a następnie wywołuje tę metodę.
- Wykonaj wynik 

    - Typową metodą akcji może być otrzymanie danych wejściowych użytkownika, przygotowanie odpowiednich danych odpowiedzi, a następnie wykonanie wyniku zwracając typ wyniku. Wbudowane typy wyników, które mogą być wykonywane, obejmują: **ViewResult** (który renderuje widok i jest najczęściej używanym typem wyniku), **RedirectToRouteResult**, **RedirectResult**, **ContentResult**, **JsonResult**i **EmptyResult**.

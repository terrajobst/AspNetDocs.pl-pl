---
uid: mvc/overview/views/dynamic-v-strongly-typed-views
title: Dynamiczne a Silnie Typizowane widoki | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/27/2011
ms.assetid: 0cbd88da-0da6-4605-b222-2835c6478304
msc.legacyurl: /mvc/overview/views/dynamic-v-strongly-typed-views
msc.type: authoredcontent
ms.openlocfilehash: 3235fc58fbf93cb87946f8ebd4a478eff7ce80e3
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/17/2019
ms.locfileid: "59386141"
---
# <a name="dynamic-v-strongly-typed-views"></a>Dynamiczne a silnie typizowane widoki

Przez [Rick Anderson]((https://twitter.com/RickAndMSFT))

Istnieją trzy sposoby przekazywania informacji z kontrolera do widoku w ASP.NET MVC 3:

1. Jako obiekt silnie typizowany model.
2. Jako typu dynamicznego (przy użyciu @model dynamiczny)
3. Za pomocą obiekt ViewBag

Moje recenzje prostą aplikację MVC 3 pierwszych blogu do Porównaj i zestaw widoki dynamiczne i silnie typizowane. Kontroler rozpoczyna się od prostego listę blogów:

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample1.cs)]

Kliknij prawym przyciskiem myszy w metodzie IndexNotStonglyTyped() i Dodawanie widoku Razor.

[![8475.NotStronglyTypedView[1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)

Upewnij się, że **utworzyć widok silnie typizowane** pole nie jest zaznaczone. Wynikowym widoku nie zawiera wiele:

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample2.cshtml)]

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample3.cshtml)]

Ponieważ używamy dynamiczne i silnie typizowanego widoku, funkcja intellisense nie pomagają nam. Kompletny kod jest pokazany poniżej:

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample4.cshtml)]

[![6646.NotStronglyTypedView_5F00_IE[1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)

Teraz dodamy silnie typizowanego widoku. Dodaj następujący kod na kontrolerze:

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample5.cs)]


Zwróć uwagę, że jest dokładnie ten sam zwracany View(topBlogs); Wywołaj jako — silnie typizowanego widoku. Kliknij prawym przyciskiem myszy wewnątrz *StonglyTypedIndex()* i wybierz **Dodaj widok**. Tym razem wybierz pozycję **Blog** klasa modelu, a następnie wybierz pozycję **listy** jako szablon szkieletu.

[![5658.StrongView[1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)

Wewnątrz szablonu widoku uzyskujemy obsługę funkcji intellisense.

[![7002.IntelliSense[1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)

Projekt c# można pobrać [tutaj](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip).

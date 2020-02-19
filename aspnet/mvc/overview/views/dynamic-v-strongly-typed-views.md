---
uid: mvc/overview/views/dynamic-v-strongly-typed-views
title: Dynamiczne a Widoki o jednoznacznie określonym typie | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/27/2011
ms.assetid: 0cbd88da-0da6-4605-b222-2835c6478304
msc.legacyurl: /mvc/overview/views/dynamic-v-strongly-typed-views
msc.type: authoredcontent
ms.openlocfilehash: 3e81c6381b1e280e3b74cb7eb6ea6e6c3224e655
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/19/2020
ms.locfileid: "77455676"
---
# <a name="dynamic-v-strongly-typed-views"></a>Dynamiczne a silnie typizowane widoki

Autor [Rick Anderson](https://twitter.com/RickAndMSFT)

Istnieją trzy sposoby przekazywania informacji z kontrolera do widoku w ASP.NET MVC 3:

1. Jako obiekt modelu o jednoznacznie określonym typie.
2. Jako typ dynamiczny (przy użyciu @model dynamiczny)
3. Korzystanie z ViewBag

Zapisałem prostą aplikację blogową MVC 3, która umożliwia porównanie i porównywanie dynamicznych i silnie wpisanych widoków. Kontroler jest uruchamiany z prostą listą blogów:

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample1.cs)]

Kliknij prawym przyciskiem myszy metodę IndexNotStonglyTyped () i Dodaj widok Razor.

[![8475. NotStronglyTypedView [1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)

Upewnij się, że pole wyboru **Utwórz jednoznacznie wpisaną** nie jest zaznaczone. Widok wyników nie zawiera wiele:

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample2.cshtml)]

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample3.cshtml)]

Ze względu na to, że używamy dynamicznego i nie widoku o jednoznacznie określonym typie, technologia IntelliSense nie pomoże nam. Ukończony kod jest przedstawiony poniżej:

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample4.cshtml)]

[![6646. NotStronglyTypedView_5F00_IE [1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)

Teraz dodamy widok o jednoznacznie określonym typie. Dodaj następujący kod do kontrolera:

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample5.cs)]

Zwróć uwagę, że jest to dokładnie ten sam widok zwracany (topBlogs); Wywołaj jako widok niesilnie określony. Kliknij prawym przyciskiem myszy wewnątrz *StonglyTypedIndex ()* i wybierz polecenie **Dodaj widok**. Tym razem wybierz klasę model **bloga** i wybierz pozycję **Lista** jako szablon szkieletu.

[![5658. StrongView [1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)

W nowym szablonie widoku pobieramy obsługę technologii IntelliSense.

[![7002. IntelliSense [1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)

Projekt c# można pobrać [tutaj](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip).

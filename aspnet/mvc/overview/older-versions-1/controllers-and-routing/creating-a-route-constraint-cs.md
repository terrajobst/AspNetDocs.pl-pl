---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-cs
title: Tworzenie ograniczenia trasy (C#) | Microsoft Docs
author: StephenWalther
description: W tym samouczku Stephen Walther demonstruje, jak można kontrolować, jak żądania przeglądarki są zgodne z trasami, tworząc ograniczenia trasy z wyrażeniami regularnymi.
ms.author: riande
ms.date: 02/16/2009
ms.assetid: 0bfd06b1-12d3-4fbb-9779-a82e5eb7fe7d
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-cs
msc.type: authoredcontent
ms.openlocfilehash: 51ef859287b3424faf85f4a3606a220ab48a9466
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78582013"
---
# <a name="creating-a-route-constraint-c"></a>Tworzenie ograniczenia trasy (C#)

Autor [Stephen Walther](https://github.com/StephenWalther)

> W tym samouczku Stephen Walther demonstruje, jak można kontrolować, jak żądania przeglądarki są zgodne z trasami, tworząc ograniczenia trasy z wyrażeniami regularnymi.

Ograniczenia trasy służą do ograniczania żądań przeglądarki, które pasują do określonej trasy. Aby określić ograniczenie trasy, można użyć wyrażenia regularnego.

Załóżmy na przykład, że w pliku Global. asax została zdefiniowana trasa z listy 1.

**Lista 1 — Global.asax.cs**

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample1.cs)]

Lista 1 zawiera trasę o nazwie Product. Trasy produktu można użyć do mapowania żądań przeglądarki do ProductController zawartych w liście 2.

**Lista 2 — Controllers\ProductController.cs**

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample2.cs)]

Zwróć uwagę, że akcja szczegóły () udostępniona przez kontroler produktu akceptuje pojedynczy parametr o nazwie productId. Ten parametr jest parametrem liczby całkowitej.

Trasa zdefiniowana na liście 1 będzie pasować do dowolnych z następujących adresów URL:

- /Product/23
- /Product/7

Niestety trasa będzie również zgodna z następującymi adresami URL:

- /Product/blah
- /Product/apple

Ponieważ akcja Details () oczekuje parametru Integer, wykonanie żądania zawierającego coś innego niż wartość całkowita spowoduje wystąpienie błędu. Jeśli na przykład wpiszesz adres URL/Product/Apple w przeglądarce, zobaczysz stronę błędu na rysunku 1.

[![okno dialogowe Nowy projekt](creating-a-route-constraint-cs/_static/image1.jpg)](creating-a-route-constraint-cs/_static/image1.png)

**Ilustracja 01**: wyświetlanie rozwinięcia strony ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-a-route-constraint-cs/_static/image2.png))

To, co naprawdę chcesz zrobić, jest zgodne tylko z adresami URL, które zawierają poprawną liczbę całkowitą productId. Można użyć ograniczenia podczas definiowania trasy, aby ograniczyć adresy URL, które pasują do trasy. Zmodyfikowana trasa produktu na liście 3 zawiera ograniczenie wyrażenia regularnego, które jest zgodne tylko z liczbami całkowitymi.

**Lista 3 — Global.asax.cs**

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample3.cs)]

Wyrażenie regularne \d + dopasowuje co najmniej jedną liczbę całkowitą. To ograniczenie powoduje, że trasa produktu jest zgodna z następującymi adresami URL:

- /Product/3
- /Product/8999

Ale nie następujące adresy URL:

- /Product/apple
- /Product

- Te żądania przeglądarki będą obsługiwane przez inną trasę lub, jeśli nie ma pasujących tras, zostanie zwrócony *błąd.*

> [!div class="step-by-step"]
> [Poprzednie](creating-custom-routes-cs.md)
> [dalej](creating-a-custom-route-constraint-cs.md)

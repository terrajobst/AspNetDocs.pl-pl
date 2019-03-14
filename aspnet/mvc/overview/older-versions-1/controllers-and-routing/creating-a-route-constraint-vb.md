---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-vb
title: Tworzenie ograniczenia trasy (VB) | Dokumentacja firmy Microsoft
author: StephenWalther
description: 'W tym samouczku Walther Autor: Stephen pokazuje, jak kontrolować, jak przeglądarka żąda dopasowanie tras przez tworzenie ograniczenia trasy z wyrażeniami regularnymi.'
ms.author: riande
ms.date: 02/16/2009
ms.assetid: b7cce113-c82c-45bf-b97b-357e5d9f7f56
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-vb
msc.type: authoredcontent
ms.openlocfilehash: 0a8e540a00d852d5b710bfdbf63a68f6e6d280ee
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57068270"
---
<a name="creating-a-route-constraint-vb"></a>Tworzenie ograniczenia trasy (VB)
====================
przez [Walther Autor: Stephen](https://github.com/StephenWalther)

> W tym samouczku Walther Autor: Stephen pokazuje, jak kontrolować, jak przeglądarka żąda dopasowanie tras przez tworzenie ograniczenia trasy z wyrażeniami regularnymi.


Ograniczenia trasy umożliwia ograniczanie żądań przeglądarki, które pasuje do określonej trasy. Aby określić ograniczenia trasy, można użyć wyrażenia regularnego.

Na przykład Wyobraź sobie, że zdefiniowano trasy w ofercie 1 w pliku Global.asax.

**Wyświetlanie listy 1 - Global.asax.vb**

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample1.vb)]

Wyświetlanie listy 1 zawiera trasę o nazwie produktu. Trasy produktu służy do mapowania żądania przeglądarki ProductController zawarte w ofercie 2.

**Wyświetlanie listy 2 - Controllers\ProductController.vb**

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample2.vb)]

Należy zauważyć, że akcja Details() udostępnianych przez kontroler produktu przyjmuje jeden parametr o nazwie productId. Ten parametr jest parametrem liczby całkowitej.

Trasy zdefiniowane w ofercie 1 będzie pasuje do żadnego z następujących adresów URL:

- / Produktu/23
- Produkt/7 dni w tygodniu

Niestety trasy się zgadzać następujące adresy URL:

- / Produktu/blah
- /Product/apple

Ponieważ akcji Details() oczekuje parametru liczby całkowitej, żądania zawiera coś innego niż wartość całkowitą spowoduje błąd. Na przykład jeśli wpiszesz /Product/apple adresu URL w przeglądarce następnie otrzymasz strony błędu na rysunku 1.


[![Okno dialogowe Nowy projekt](creating-a-route-constraint-vb/_static/image1.jpg)](creating-a-route-constraint-vb/_static/image1.png)

**Rysunek 01**: Wyświetlanie strony explode ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-route-constraint-vb/_static/image2.png))


Co tak naprawdę chcesz zrobić to dopasowanie tylko adresów URL zawierających productId właściwej liczby całkowitej. Można użyć ograniczenie podczas definiowania trasy, aby ograniczyć adresy URL, które odpowiadają trasy. Zmodyfikowane trasy produktu w ofercie 3 zawiera ograniczenia wyrażenia regularnego, który jest zgodny tylko liczby całkowite.

**Wyświetlanie listy 3 - Global.asax.vb**

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample3.vb)]

\D+ wyrażenia regularnego dopasowuje jeden lub więcej liczb całkowitych. To ograniczenie powoduje, że trasy produktu dopasować następujące adresy URL:

- / Produkt/3
- /Product/8999

Ale nie następujące adresy URL:

- /Product/apple
- / Produktu

Te żądania przeglądarki będzie obsługiwany przez innej trasy lub, jeśli istnieje nie zgodnych tras *nie można odnaleźć zasobu* zostanie zwrócony błąd.

> [!div class="step-by-step"]
> [Poprzednie](creating-custom-routes-vb.md)
> [dalej](creating-a-custom-route-constraint-vb.md)

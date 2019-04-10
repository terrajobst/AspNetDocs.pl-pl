---
uid: web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-cs
title: Wybieranie jednej animacji spoza listy (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki. Struktura również zez...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 06a776fe-7c73-4ca7-8e02-5260a86edc03
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-cs
msc.type: authoredcontent
ms.openlocfilehash: 1cbb60431824ce642625c06cba6b5194aa547b1b
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/09/2019
ms.locfileid: "59419707"
---
# <a name="picking-one-animation-out-of-a-list-c"></a>Wybieranie jednej animacji z listy (C#)

przez [Christian Wenz](https://github.com/wenz)

[Pobierz program Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation5.cs.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation5CS.pdf)

> Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki. Struktura umożliwia także programisty do pobrania jednej animacji spoza listy animacji, w zależności od oceny kodu JavaScript.


## <a name="overview"></a>Omówienie

Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki. Struktura umożliwia także programisty do pobrania jednej animacji spoza listy animacji, w zależności od oceny kodu JavaScript.

## <a name="steps"></a>Kroki

Po pierwsze, obejmują `ScriptManager` w strony, a następnie biblioteki ASP.NET AJAX jest ładowany, dzięki czemu można użyć zestawu narzędzi kontroli:

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample1.aspx)]

Animacja zostanie zastosowana do panelu tekstu, który wygląda w następujący sposób:

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample2.aspx)]

W skojarzone klasy CSS do obsługi panelu zdefiniowany jako kolor tła dobre rozwiązanie, a także ustawić stała szerokość panelu:

[!code-css[Main](picking-one-animation-out-of-a-list-cs/samples/sample3.css)]

Następnie należy dodać `AnimationExtender` do strony, zapewniając `ID`, `TargetControlID` atrybut i obowiązkowe `runat="server":`

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample4.aspx)]

W ramach `<Animations>` węzła, użyj `<OnLoad>` do uruchamiania animacji, po całkowitym załadowaniu strony. Zamiast jednej z regularnych animacji `<Case>` element, który jest dostarczany do gry. Wartość jego atrybutu SelectScript jest oceniany; zwracana wartość musi być numeryczny. W zależności od tego numeru, jeden subanimations w ramach &lt;przypadek&gt; jest wykonywany. Na przykład jeśli SelectScript ma wartość 2, zestaw narzędzi kontroli uruchamia trzeci animacji w ramach &lt;przypadek&gt; (zliczanie rozpoczyna się od 0).

Następujące znaczniki definiuje trzy subanimations: Zmiana rozmiaru szerokość, wysokość rozmiaru i wygaszanie. Kod JavaScript (`Math.floor(3 * Math.random())`) następnie wybiera liczbą z zakresu od 0 do 2, tak, aby jeden z trzech animacji uruchomieniu:

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample5.aspx)]


[![One animacji trzy możliwe: Panel pobiera szersze](picking-one-animation-out-of-a-list-cs/_static/image2.png)](picking-one-animation-out-of-a-list-cs/_static/image1.png)

Jeden z trzech możliwych animacji: Panel poszerzania ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](picking-one-animation-out-of-a-list-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](animation-depending-on-a-condition-cs.md)
> [dalej](animating-in-response-to-user-interaction-cs.md)

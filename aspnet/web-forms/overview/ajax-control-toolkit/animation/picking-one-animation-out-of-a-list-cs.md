---
uid: web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-cs
title: Wybieranie jednej animacji z listy (C#) | Microsoft Docs
author: wenz
description: Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. Platforma nie umożliwia także...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 06a776fe-7c73-4ca7-8e02-5260a86edc03
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-cs
msc.type: authoredcontent
ms.openlocfilehash: 2bc203d694792716bbf4f7d7b8d152c589bf99b1
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74575111"
---
# <a name="picking-one-animation-out-of-a-list-c"></a>Wybieranie jednej animacji z listy (C#)

Autor [Christian Wenz](https://github.com/wenz)

[Pobierz kod](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation5.cs.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation5CS.pdf)

> Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. Struktura umożliwia również programistom wybranie jednej animacji z listy animacji, w zależności od oceny kodu JavaScript.

## <a name="overview"></a>Omówienie

Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. Struktura umożliwia również programistom wybranie jednej animacji z listy animacji, w zależności od oceny kodu JavaScript.

## <a name="steps"></a>Kroki

Po pierwsze, Uwzględnij `ScriptManager` na stronie; następnie załadowana zostanie Biblioteka ASP.NET AJAX, co umożliwia korzystanie z zestawu narzędzi kontroli:

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample1.aspx)]

Animacja zostanie zastosowana do panelu tekstu, który wygląda następująco:

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample2.aspx)]

W skojarzonej klasie CSS panelu, zdefiniuj kolor tła całkiem, a także Ustaw stałą szerokość panelu:

[!code-css[Main](picking-one-animation-out-of-a-list-cs/samples/sample3.css)]

Następnie Dodaj `AnimationExtender` do strony, podając `ID`, atrybut `TargetControlID` i obowiązkowe `runat="server":`

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample4.aspx)]

W węźle `<Animations>` Użyj `<OnLoad>`, aby uruchomić animacje po całkowitym załadowaniu strony. Zamiast jednego ze zwykłych animacji, element `<Case>` jest dostępny do odtworzenia. Wartość atrybutu SelectScript jest oceniana; zwracana wartość musi być wartością numeryczną. W zależności od tego numeru jest wykonywana jedna z animacji w &lt;&gt; przypadku. Na przykład jeśli SelectScript oblicza wartość 2, zestaw narzędzi sterowania uruchamia trzecią animację w &lt;&gt; przypadku (zliczanie zaczyna się od 0).

Następujące oznakowanie definiuje trzy animacje: zmienianie rozmiarów szerokości, zmianę rozmiarów i stopniowe Rozjaśnianie. Kod JavaScript (`Math.floor(3 * Math.random())`) następnie wybiera liczbę z zakresu od 0 do 2, aby jeden z trzech animacji był uruchamiany:

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample5.aspx)]

[![jedną z możliwych trzech animacji: Panel jest szerszy](picking-one-animation-out-of-a-list-cs/_static/image2.png)](picking-one-animation-out-of-a-list-cs/_static/image1.png)

Jedna z możliwych trzech animacji: Panel jest szerszy (kliknij,[Aby wyświetlić obraz w pełnym rozmiarze](picking-one-animation-out-of-a-list-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](animation-depending-on-a-condition-cs.md)
> [dalej](animating-in-response-to-user-interaction-cs.md)

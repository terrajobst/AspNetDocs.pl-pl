---
uid: web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-vb
title: Animacja w zależności od warunku (VB) | Microsoft Docs
author: wenz
description: Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. Czy animacja jest...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 1b87d8d6-b3f7-4126-b51c-d41442fbf947
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-vb
msc.type: authoredcontent
ms.openlocfilehash: 583ebdbf109beb74b9a425020477183067bbb79a
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74607010"
---
# <a name="animation-depending-on-a-condition-vb"></a>Animacja w zależności od warunku (VB)

Autor [Christian Wenz](https://github.com/wenz)

[Pobierz kod](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.vb.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4VB.pdf)

> Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. Czy animacja jest uruchamiana, czy nie może również zależeć od warunku w postaci niektórych kodów JavaScript.

## <a name="overview"></a>Omówienie

Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. Czy animacja jest uruchamiana, czy nie może również zależeć od warunku w postaci niektórych kodów JavaScript.

## <a name="steps"></a>Kroki

Po pierwsze, Uwzględnij `ScriptManager` na stronie; następnie załadowana zostanie Biblioteka ASP.NET AJAX, co umożliwia korzystanie z zestawu narzędzi kontroli:

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample1.aspx)]

Animacja zostanie zastosowana do panelu tekstu, który wygląda następująco:

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample2.aspx)]

W skojarzonej klasie CSS panelu, zdefiniuj kolor tła całkiem, a także Ustaw stałą szerokość panelu:

[!code-css[Main](animation-depending-on-a-condition-vb/samples/sample3.css)]

Następnie Dodaj `AnimationExtender` do strony, podając `ID`, atrybut `TargetControlID` i obowiązkowe `runat="server":`

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample4.aspx)]

W węźle `<Animations>` Użyj `<OnLoad>`, aby uruchomić animacje po całkowitym załadowaniu strony. Zamiast jednego ze zwykłych animacji, element `<Condition>` jest dostępny do odtworzenia. Kod JavaScript podany jako wartość atrybutu `ConditionScript` jest wykonywany w czasie wykonywania. Jeśli wartość jest równa true, animacja jest wykonywana, w przeciwnym razie. Poniższe znaczniki zawierają dwie animacje, z których każdy jest wykonywany w 50% przypadków losowo. Ponieważ w `<OnLoad>`może istnieć tylko jedna animacja, dwa `<Condition>` animacje są sprzężone ze sobą przy użyciu elementu `<Sequence>`:

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample5.aspx)]

Należy zauważyć, że znak mniejszości (`<`) w atrybucie `ConditionScript` musi mieć wartość ucieczki (). Po uruchomieniu tego skryptu nie są wykonywane żadne animacje ani jeden z dwóch elementów lub.

[![panel jest znikający bez zmiany rozmiarów, więc drugie uruchomienie animacji spowoduje, że pierwszy z nich nie](animation-depending-on-a-condition-vb/_static/image2.png)](animation-depending-on-a-condition-vb/_static/image1.png)

Panel jest odtwarzany bez zmiany rozmiaru, więc drugie uruchomienie animacji, pierwsze, nie ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](animation-depending-on-a-condition-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](executing-several-animations-after-each-other-vb.md)
> [dalej](picking-one-animation-out-of-a-list-vb.md)

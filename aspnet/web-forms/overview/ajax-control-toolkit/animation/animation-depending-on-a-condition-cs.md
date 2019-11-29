---
uid: web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-cs
title: Animacja w zależności od warunku (C#) | Microsoft Docs
author: wenz
description: Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. Czy animacja jest...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: b7a28c0d-efb9-443a-80a4-1a5ee54671cd
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-cs
msc.type: authoredcontent
ms.openlocfilehash: 476b0cf80fa7c04cd8b8f9a92060ddabb9d14c13
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599854"
---
# <a name="animation-depending-on-a-condition-c"></a>Animacja w zależności od warunku (C#)

Autor [Christian Wenz](https://github.com/wenz)

[Pobierz kod](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.cs.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4CS.pdf)

> Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. Czy animacja jest uruchamiana, czy nie może również zależeć od warunku w postaci niektórych kodów JavaScript.

## <a name="overview"></a>Omówienie

Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. Czy animacja jest uruchamiana, czy nie może również zależeć od warunku w postaci niektórych kodów JavaScript.

## <a name="steps"></a>Kroki

Po pierwsze, Uwzględnij `ScriptManager` na stronie; następnie załadowana zostanie Biblioteka ASP.NET AJAX, co umożliwia korzystanie z zestawu narzędzi kontroli:

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample1.aspx)]

Animacja zostanie zastosowana do panelu tekstu, który wygląda następująco:

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample2.aspx)]

W skojarzonej klasie CSS panelu, zdefiniuj kolor tła całkiem, a także Ustaw stałą szerokość panelu:

[!code-css[Main](animation-depending-on-a-condition-cs/samples/sample3.css)]

Następnie Dodaj `AnimationExtender` do strony, podając `ID`, atrybut `TargetControlID` i obowiązkowe `runat="server":`

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample4.aspx)]

W węźle `<Animations>` Użyj `<OnLoad>`, aby uruchomić animacje po całkowitym załadowaniu strony. Zamiast jednego ze zwykłych animacji, element `<Condition>` jest dostępny do odtworzenia. Kod JavaScript podany jako wartość atrybutu `ConditionScript` jest wykonywany w czasie wykonywania. Jeśli wartość jest równa true, animacja jest wykonywana, w przeciwnym razie. Poniższe znaczniki zawierają dwie animacje, z których każdy jest wykonywany w 50% przypadków losowo. Ponieważ w `<OnLoad>`może istnieć tylko jedna animacja, dwa `<Condition>` animacje są sprzężone ze sobą przy użyciu elementu `<Sequence>`:

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample5.aspx)]

Należy zauważyć, że znak mniejszości (`<`) w atrybucie `ConditionScript` musi mieć wartość ucieczki (). Po uruchomieniu tego skryptu nie są wykonywane żadne animacje ani jeden z dwóch elementów lub.

[![panel jest znikający bez zmiany rozmiarów, więc drugie uruchomienie animacji spowoduje, że pierwszy z nich nie](animation-depending-on-a-condition-cs/_static/image2.png)](animation-depending-on-a-condition-cs/_static/image1.png)

Panel jest odtwarzany bez zmiany rozmiaru, więc drugie uruchomienie animacji, pierwsze, nie ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](animation-depending-on-a-condition-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](executing-several-animations-after-each-other-cs.md)
> [dalej](picking-one-animation-out-of-a-list-cs.md)

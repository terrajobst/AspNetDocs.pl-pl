---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-cs
title: Animowanie w odpowiedzi na interakcję użytkownikaC#() | Microsoft Docs
author: wenz
description: Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. Animacje mogą być gwiazdką...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: ea26549d-fbbf-4973-a108-b14cd1d6de26
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-cs
msc.type: authoredcontent
ms.openlocfilehash: d04fa680d0cd4f7fb54521ac6fbb47a2cf9a83cf
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599879"
---
# <a name="animating-in-response-to-user-interaction-c"></a>Wykonywanie animacji w odpowiedzi na interakcję z użytkownikiem (C#)

Autor [Christian Wenz](https://github.com/wenz)

[Pobierz kod](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.cs.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6CS.pdf)

> Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. Animacje mogą być uruchamiane automatycznie lub mogą być wyzwalane przez interakcję z użytkownikiem, np. klikając myszą.

## <a name="overview"></a>Omówienie

Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. Animacje mogą być uruchamiane automatycznie lub mogą być wyzwalane przez interakcję z użytkownikiem, np. klikając myszą.

## <a name="steps"></a>Kroki

Po pierwsze, Uwzględnij `ScriptManager` na stronie; następnie załadowana zostanie Biblioteka ASP.NET AJAX, co umożliwia korzystanie z zestawu narzędzi kontroli:

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample1.aspx)]

Animacja zostanie zastosowana do panelu tekstu, który wygląda następująco:

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample2.aspx)]

W skojarzonej klasie CSS panelu, zdefiniuj kolor tła całkiem, a także Ustaw stałą szerokość panelu:

[!code-css[Main](animating-in-response-to-user-interaction-cs/samples/sample3.css)]

Następnie Dodaj `AnimationExtender` do strony, podając `ID`, atrybut `TargetControlID` i obowiązkowe `runat="server"`:

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample4.aspx)]

W węźle `<Animations>` istnieje pięć sposobów uruchamiania animacji przez interakcję z użytkownikiem (brak elementu jest `<OnLoad>`, który jest wykonywany po całkowitym załadowaniu całej strony):

- `<OnClick>` (kliknięcie kontrolki myszy)
- `<OnHoverOut>` (mysz opuszcza formant)
- `<OnHoverOver>` (przesuwanie myszą nad kontrolką, zatrzymywanie animacji `<OnHoverOut>`)
- `<OnMouseOut>` (mysz opuszcza kontrolkę)
- `<OnMouseOver>` (przesuwanie myszą nad kontrolką, nie zatrzymywanie animacji `<OnMouseOut>`)

W tym scenariuszu jest używana `<OnClick>`. Gdy użytkownik kliknie panel, zmieniany jest rozmiar i zanika w tym samym czasie.

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample5.aspx)]

[![kliknięciu myszą uruchamia animację](animating-in-response-to-user-interaction-cs/_static/image2.png)](animating-in-response-to-user-interaction-cs/_static/image1.png)

Kliknięcie przycisku powoduje uruchomienie animacji ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](animating-in-response-to-user-interaction-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](picking-one-animation-out-of-a-list-cs.md)
> [dalej](disabling-actions-during-animation-cs.md)

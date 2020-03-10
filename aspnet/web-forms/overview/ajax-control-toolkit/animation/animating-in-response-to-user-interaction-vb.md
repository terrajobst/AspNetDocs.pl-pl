---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-vb
title: Animowanie w odpowiedzi na interakcję użytkownika (VB) | Microsoft Docs
author: wenz
description: Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. Animacje mogą być gwiazdką...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: c8204c05-ec27-40fe-933d-88e4e727a482
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-vb
msc.type: authoredcontent
ms.openlocfilehash: 629d79505cebd49c2f05333bfbf78166f80fc6cb
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78614402"
---
# <a name="animating-in-response-to-user-interaction-vb"></a>Wykonywanie animacji w odpowiedzi na interakcję z użytkownikiem (VB)

Autor [Christian Wenz](https://github.com/wenz)

[Pobierz kod](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.vb.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6VB.pdf)

> Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. Animacje mogą być uruchamiane automatycznie lub mogą być wyzwalane przez interakcję z użytkownikiem, np. klikając myszą.

## <a name="overview"></a>Omówienie

Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. Animacje mogą być uruchamiane automatycznie lub mogą być wyzwalane przez interakcję z użytkownikiem, np. klikając myszą.

## <a name="steps"></a>Kroki

Po pierwsze, Uwzględnij `ScriptManager` na stronie; następnie załadowana zostanie Biblioteka ASP.NET AJAX, co umożliwia korzystanie z zestawu narzędzi kontroli:

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample1.aspx)]

Animacja zostanie zastosowana do panelu tekstu, który wygląda następująco:

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample2.aspx)]

W skojarzonej klasie CSS panelu, zdefiniuj kolor tła całkiem, a także Ustaw stałą szerokość panelu:

[!code-css[Main](animating-in-response-to-user-interaction-vb/samples/sample3.css)]

Następnie Dodaj `AnimationExtender` do strony, podając `ID`, atrybut `TargetControlID` i obowiązkowe `runat="server"`:

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample4.aspx)]

W węźle `<Animations>` istnieje pięć sposobów uruchamiania animacji przez interakcję z użytkownikiem (brak elementu jest `<OnLoad>`, który jest wykonywany po całkowitym załadowaniu całej strony):

- `<OnClick>` (kliknięcie kontrolki myszy)
- `<OnHoverOut>` (mysz opuszcza formant)
- `<OnHoverOver>` (przesuwanie myszą nad kontrolką, zatrzymywanie animacji `<OnHoverOut>`)
- `<OnMouseOut>` (mysz opuszcza kontrolkę)
- `<OnMouseOver>` (przesuwanie myszą nad kontrolką, nie zatrzymywanie animacji `<OnMouseOut>`)

W tym scenariuszu jest używana `<OnClick>`. Gdy użytkownik kliknie panel, zmieniany jest rozmiar i zanika w tym samym czasie.

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample5.aspx)]

[![kliknięciu myszą uruchamia animację](animating-in-response-to-user-interaction-vb/_static/image2.png)](animating-in-response-to-user-interaction-vb/_static/image1.png)

Kliknięcie przycisku powoduje uruchomienie animacji ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](animating-in-response-to-user-interaction-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](picking-one-animation-out-of-a-list-vb.md)
> [dalej](disabling-actions-during-animation-vb.md)

---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-cs
title: Animowanie kontrolki UpdatePanel (C#) | Microsoft Docs
author: wenz
description: Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. Dla zawartości...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e57f8c7c-3940-4bc0-9468-3a0ca69158ea
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 8875d750d57c5f4e362bdf461826130a881ab1d4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536380"
---
# <a name="animating-an-updatepanel-control-c"></a>Animowanie kontrolki UpdatePanel (C#)

Autor [Christian Wenz](https://github.com/wenz)

[Pobierz kod](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation1.cs.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation1CS.pdf)

> Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. W przypadku zawartości elementu UpdatePanel istnieje specjalne rozszerzenie, które opiera się na środowisku animacji: UpdatePanelAnimation. W tym samouczku pokazano, jak skonfigurować takie animacje dla elementu UpdatePanel.

## <a name="overview"></a>Omówienie

Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. W przypadku zawartości `UpdatePanel`istnieje specjalne rozszerzenie, które opiera się na środowisku animacji: `UpdatePanelAnimation`. W tym samouczku pokazano, jak skonfigurować taką animację dla `UpdatePanel`.

## <a name="steps"></a>Kroki

Pierwszym krokiem jest zwykle uwzględnienie `ScriptManager` na stronie, tak aby Biblioteka ASP.NET AJAX została załadowana i można było użyć zestawu narzędzi do sterowania:

[!code-aspx[Main](animating-an-updatepanel-control-cs/samples/sample1.aspx)]

Animacja w tym scenariuszu zostanie zastosowana do kontrolki sieci Web ASP.NET `Wizard` znajdującej się w `UpdatePanel`. Trzy (dowolne) kroki zapewniają wystarczającą ilość opcji wyzwalających ogłaszanie zwrotne:

[!code-aspx[Main](animating-an-updatepanel-control-cs/samples/sample2.aspx)]

Znaczniki wymagane dla formantu `UpdatePanelAnimationExtender` są bardzo podobne do znaczników użytych dla `AnimationExtender`. W `TargetControlID` atrybucie udostępniamy `ID` `UpdatePanel` do animacji; w kontrolce `UpdatePanelAnimationExtender` element `<Animations>` przechowuje znaczniki XML dla animacji. Istnieje jednak jedna różnica: ilość zdarzeń i obsługi zdarzeń jest ograniczona w porównaniu do `AnimationExtender`. W przypadku `UpdatePanels`istnieją tylko dwa z nich:

- `<OnUpdated>`, gdy element UpdatePanel został zaktualizowany
- `<OnUpdating>`, gdy element UpdatePanel rozpocznie aktualizowanie

W tym scenariuszu Nowa zawartość `UpdatePanel` (po odświeżeniu zwrotnego) zmienia się w programie. Jest to wymagane oznaczenie:

[!code-aspx[Main](animating-an-updatepanel-control-cs/samples/sample3.aspx)]

Teraz za każdym razem, gdy w elemencie UpdatePanel zostanie przeprowadzone ogłaszanie zwrotne, Nowa zawartość panelu jest płynnie zmieniana.

[![następnym kroku kreatora zostanie zanikanie](animating-an-updatepanel-control-cs/_static/image2.png)](animating-an-updatepanel-control-cs/_static/image1.png)

Następnym krokiem kreatora jest zanikanie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](animating-an-updatepanel-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](changing-an-animation-using-client-side-code-cs.md)
> [dalej](dynamically-controlling-updatepanel-animations-cs.md)

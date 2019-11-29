---
uid: web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
title: Wyłączanie akcji podczas animacji (C#) | Microsoft Docs
author: wenz
description: Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. Obsługuje również akcję...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 918026b4-2f63-421d-8546-df12856960a8
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
msc.type: authoredcontent
ms.openlocfilehash: e91205ad2f9e6ee1fdd869ceb7587c3a82754772
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599731"
---
# <a name="disabling-actions-during-animation-c"></a>Wyłączanie akcji podczas animacji (C#)

Autor [Christian Wenz](https://github.com/wenz)

[Pobierz kod](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.cs.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7CS.pdf)

> Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. Obsługuje także akcje, takie jak kliknięcia myszą. Jednak po kliknięciu przycisku myszy uruchamia animację, pożądane jest wyłączenie kliknięć myszą podczas animacji.

## <a name="overview"></a>Omówienie

Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. Obsługuje także akcje, takie jak kliknięcia myszą. Jednak po kliknięciu przycisku myszy uruchamia animację, pożądane jest wyłączenie kliknięć myszą podczas animacji.

## <a name="steps"></a>Kroki

Po pierwsze, Uwzględnij `ScriptManager` na stronie; następnie załadowana zostanie Biblioteka ASP.NET AJAX, co umożliwia korzystanie z zestawu narzędzi kontroli:

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample1.aspx)]

Animacja zostanie zastosowana do przycisku HTML w następujący sposób:

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample2.aspx)]

Należy zauważyć, że kontrolka HTML jest używana zamiast kontrolki sieci Web, ponieważ nie chcę, aby przycisk utworzył ogłaszanie zwrotne; po prostu uruchamia dla nas animację po stronie klienta.

Następnie Dodaj `AnimationExtender` do strony, podając `ID`, atrybut `TargetControlID` i obowiązkowe `runat="server"`:

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample3.aspx)]

W węźle `<Animations>` `<OnClick>` jest prawym elementem, aby obsłużyć kliknięcie myszą. Jednak przycisk można kliknąć również podczas animacji. Element `<EnableAction>` może zadbać o to. Ustawienie `Enabled="false"` wyłącza przycisk w ramach animacji. Ponieważ korzystamy z kilku indywidualnych animacji (wyłączenie przycisku i rzeczywistych animacji), element `<Parallel>` jest wymagany do przyklejania pojedynczych animacji w jeden. Oto kompletny znacznik dla `AnimationExtender`:

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample4.aspx)]

Możliwe jest również ponowne włączenie do przycisku po animacji, przy użyciu następującego elementu XML na końcu listy:

[!code-xml[Main](disabling-actions-during-animation-cs/samples/sample5.xml)]

Jednak w danym scenariuszu może być bezużyteczny, ponieważ przycisk znika i nie jest widoczny na końcu animacji.

[![przycisk jest wyłączony zaraz po uruchomieniu animacji](disabling-actions-during-animation-cs/_static/image2.png)](disabling-actions-during-animation-cs/_static/image1.png)

Przycisk jest wyłączony zaraz po uruchomieniu animacji ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](disabling-actions-during-animation-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](animating-in-response-to-user-interaction-cs.md)
> [dalej](triggering-an-animation-in-another-control-cs.md)

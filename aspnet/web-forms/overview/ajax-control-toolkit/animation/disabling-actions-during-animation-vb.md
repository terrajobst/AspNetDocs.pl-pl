---
uid: web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-vb
title: Wyłączanie akcji podczas animacji (VB) | Microsoft Docs
author: wenz
description: Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. Obsługuje również akcję...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: a86c0276-6481-46ee-8b4f-8c2009399ee9
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-vb
msc.type: authoredcontent
ms.openlocfilehash: 4924d4f70099255b930d53f6a72e810be7a47485
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74606832"
---
# <a name="disabling-actions-during-animation-vb"></a>Wyłączanie akcji podczas animacji (VB)

Autor [Christian Wenz](https://github.com/wenz)

[Pobierz kod](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.vb.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7VB.pdf)

> Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. Obsługuje także akcje, takie jak kliknięcia myszą. Jednak po kliknięciu przycisku myszy uruchamia animację, pożądane jest wyłączenie kliknięć myszą podczas animacji.

## <a name="overview"></a>Omówienie

Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. Obsługuje także akcje, takie jak kliknięcia myszą. Jednak po kliknięciu przycisku myszy uruchamia animację, pożądane jest wyłączenie kliknięć myszą podczas animacji.

## <a name="steps"></a>Kroki

Po pierwsze, Uwzględnij `ScriptManager` na stronie; następnie załadowana zostanie Biblioteka ASP.NET AJAX, co umożliwia korzystanie z zestawu narzędzi kontroli:

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample1.aspx)]

Animacja zostanie zastosowana do przycisku HTML w następujący sposób:

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample2.aspx)]

Należy zauważyć, że kontrolka HTML jest używana zamiast kontrolki sieci Web, ponieważ nie chcę, aby przycisk utworzył ogłaszanie zwrotne; po prostu uruchamia dla nas animację po stronie klienta.

Następnie Dodaj `AnimationExtender` do strony, podając `ID`, atrybut `TargetControlID` i obowiązkowe `runat="server"`:

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample3.aspx)]

W węźle `<Animations>` `<OnClick>` jest prawym elementem, aby obsłużyć kliknięcie myszą. Jednak przycisk można kliknąć również podczas animacji. Element `<EnableAction>` może zadbać o to. Ustawienie `Enabled="false"` wyłącza przycisk w ramach animacji. Ponieważ korzystamy z kilku indywidualnych animacji (wyłączenie przycisku i rzeczywistych animacji), element `<Parallel>` jest wymagany do przyklejania pojedynczych animacji w jeden. Oto kompletny znacznik dla `AnimationExtender`:

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample4.aspx)]

Możliwe jest również ponowne włączenie do przycisku po animacji, przy użyciu następującego elementu XML na końcu listy:

[!code-xml[Main](disabling-actions-during-animation-vb/samples/sample5.xml)]

Jednak w danym scenariuszu może być bezużyteczny, ponieważ przycisk znika i nie jest widoczny na końcu animacji.

[![przycisk jest wyłączony zaraz po uruchomieniu animacji](disabling-actions-during-animation-vb/_static/image2.png)](disabling-actions-during-animation-vb/_static/image1.png)

Przycisk jest wyłączony zaraz po uruchomieniu animacji ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](disabling-actions-during-animation-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](animating-in-response-to-user-interaction-vb.md)
> [dalej](triggering-an-animation-in-another-control-vb.md)

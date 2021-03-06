---
uid: web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-vb
title: Wyzwalanie animacji w innej kontrolce (VB) | Microsoft Docs
author: wenz
description: Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. Ogólnie uruchamianie...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 25ebaf1f-5a9f-423d-98c7-1d694e93664f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 6a4af2324afab7519170c123b6ea7c57ab3e03fb
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536072"
---
# <a name="triggering-an-animation-in-another-control-vb"></a>Wyzwalanie animacji w innej kontrolce (VB)

Autor [Christian Wenz](https://github.com/wenz)

[Pobierz kod](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.vb.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8VB.pdf)

> Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. Ogólnie uruchamianie animacji jest wyzwalane przez interakcję użytkownika z tą samą kontrolką. Możliwe jest również współdziałanie z jednym formantem, a następnie przeprowadzenie animacji innej kontrolki.

## <a name="overview"></a>Omówienie

Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. Ogólnie uruchamianie animacji jest wyzwalane przez interakcję użytkownika z tą samą kontrolką. Możliwe jest również współdziałanie z jednym formantem, a następnie przeprowadzenie animacji innej kontrolki.

## <a name="steps"></a>Kroki

Po pierwsze, Uwzględnij `ScriptManager` na stronie; następnie załadowana zostanie Biblioteka ASP.NET AJAX, co umożliwia korzystanie z zestawu narzędzi kontroli:

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample1.aspx)]

Animacja zostanie zastosowana do panelu tekstu, który wygląda następująco:

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample2.aspx)]

W skojarzonej klasie CSS panelu, zdefiniuj kolor tła całkiem, a także Ustaw stałą szerokość panelu:

[!code-css[Main](triggering-an-animation-in-another-control-vb/samples/sample3.css)]

Aby rozpocząć animację panelu, używany jest przycisk HTML. Należy pamiętać, że `<input type="button" />` jest korzystne dla `<asp:Button />`, ponieważ nie ma potrzeby ogłaszania zwrotnego po kliknięciu tego przycisku przez użytkownika.

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample4.aspx)]

Następnie Dodaj `AnimationExtender` do strony, podając `ID`, atrybut `TargetControlID` i obowiązkowe `runat="server"`. Ważne jest, aby ustawić `TargetControlID` na identyfikator przycisku (element wyzwalający animację), a nie na identyfikator panelu (animowany element).

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample5.aspx)]

W węźle `<Animations>` Umieść animacje w zwykły sposób. Aby zmienić panel, a nie przycisk, należy ustawić atrybut `AnimationTarget` dla każdego elementu animacji w `AnimationExtender`. Wartość `AnimationTarget` jest IDENTYFIKATORem panelu. W ten sposób animacje są wykonywane z panelem, a nie z przyciskiem wyzwalającym. Oto `AnimationExtender` znaczników w tym scenariuszu:

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample6.aspx)]

Zwróć uwagę na kolejność specjalną, w której pojawiają się pojedyncze animacje. Po pierwsze, przycisk zostaje zdezaktywowany po uruchomieniu animacji. Ponieważ nie ma `AnimationTarget` atrybutu w elemencie `<EnableAction>`, ta animacja jest stosowana do formantu źródłowego: przycisk. Następne dwa kroki animacji są wykonywane równolegle (`<Parallel>` elementu). Oba mają atrybuty `AnimationTarget` ustawione na `"Panel1"`, a tym samym animowanie panelu, a nie przycisku.

[![kliknięciu przycisku myszy spowoduje uruchomienie animacji panelu.](triggering-an-animation-in-another-control-vb/_static/image2.png)](triggering-an-animation-in-another-control-vb/_static/image1.png)

Kliknięcie przycisku powoduje uruchomienie animacji panelu ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](triggering-an-animation-in-another-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](disabling-actions-during-animation-vb.md)
> [dalej](modifying-animations-from-the-server-side-vb.md)

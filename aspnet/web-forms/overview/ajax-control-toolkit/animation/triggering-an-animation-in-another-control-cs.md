---
uid: web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-cs
title: Wyzwalanie animacji w innej kontrolce (C#) | Microsoft Docs
author: wenz
description: Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. Ogólnie uruchamianie...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e5d99c2b-d8ee-413c-80d5-c120cffb0a4c
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-cs
msc.type: authoredcontent
ms.openlocfilehash: e0a1f8986047da04db6fde8e54b6fd0ac6ee2603
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536079"
---
# <a name="triggering-an-animation-in-another-control-c"></a>Wyzwalanie animacji w innej kontrolce (C#)

Autor [Christian Wenz](https://github.com/wenz)

[Pobierz kod](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.cs.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8CS.pdf)

> Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. Ogólnie uruchamianie animacji jest wyzwalane przez interakcję użytkownika z tą samą kontrolką. Możliwe jest również współdziałanie z jednym formantem, a następnie przeprowadzenie animacji innej kontrolki.

## <a name="overview"></a>Omówienie

Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. Ogólnie uruchamianie animacji jest wyzwalane przez interakcję użytkownika z tą samą kontrolką. Możliwe jest również współdziałanie z jednym formantem, a następnie przeprowadzenie animacji innej kontrolki.

## <a name="steps"></a>Kroki

Po pierwsze, Uwzględnij `ScriptManager` na stronie; następnie załadowana zostanie Biblioteka ASP.NET AJAX, co umożliwia korzystanie z zestawu narzędzi kontroli:

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample1.aspx)]

Animacja zostanie zastosowana do panelu tekstu, który wygląda następująco:

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample2.aspx)]

W skojarzonej klasie CSS panelu, zdefiniuj kolor tła całkiem, a także Ustaw stałą szerokość panelu:

[!code-css[Main](triggering-an-animation-in-another-control-cs/samples/sample3.css)]

Aby rozpocząć animację panelu, używany jest przycisk HTML. Należy pamiętać, że `<input type="button" />` jest korzystne dla `<asp:Button />`, ponieważ nie ma potrzeby ogłaszania zwrotnego po kliknięciu tego przycisku przez użytkownika.

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample4.aspx)]

Następnie Dodaj `AnimationExtender` do strony, podając `ID`, atrybut `TargetControlID` i obowiązkowe `runat="server"`. Ważne jest, aby ustawić `TargetControlID` na identyfikator przycisku (element wyzwalający animację), a nie na identyfikator panelu (animowany element).

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample5.aspx)]

W węźle `<Animations>` Umieść animacje w zwykły sposób. Aby zmienić panel, a nie przycisk, należy ustawić atrybut `AnimationTarget` dla każdego elementu animacji w `AnimationExtender`. Wartość `AnimationTarget` jest IDENTYFIKATORem panelu. W ten sposób animacje są wykonywane z panelem, a nie z przyciskiem wyzwalającym. Oto `AnimationExtender` znaczników w tym scenariuszu:

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample6.aspx)]

Zwróć uwagę na kolejność specjalną, w której pojawiają się pojedyncze animacje. Po pierwsze, przycisk zostaje zdezaktywowany po uruchomieniu animacji. Ponieważ nie ma `AnimationTarget` atrybutu w elemencie `<EnableAction>`, ta animacja jest stosowana do formantu źródłowego: przycisk. Następne dwa kroki animacji są wykonywane równolegle (`<Parallel>` elementu). Oba mają atrybuty `AnimationTarget` ustawione na `"Panel1"`, a tym samym animowanie panelu, a nie przycisku.

[![kliknięciu przycisku myszy spowoduje uruchomienie animacji panelu.](triggering-an-animation-in-another-control-cs/_static/image2.png)](triggering-an-animation-in-another-control-cs/_static/image1.png)

Kliknięcie przycisku powoduje uruchomienie animacji panelu ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](triggering-an-animation-in-another-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](disabling-actions-during-animation-cs.md)
> [dalej](modifying-animations-from-the-server-side-cs.md)

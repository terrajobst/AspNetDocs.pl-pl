---
uid: web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-vb
title: Wyzwalanie animacji w innej kontrolce (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki. Ogólnie rzecz biorąc, uruchamianie...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 25ebaf1f-5a9f-423d-98c7-1d694e93664f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 132f9f85eccabc890308984b9e78ed1d2212c57a
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57072701"
---
<a name="triggering-an-animation-in-another-control-vb"></a>Wyzwalanie animacji w innej kontrolce (VB)
====================
przez [Christian Wenz](https://github.com/wenz)

[Pobierz program Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.vb.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8VB.pdf)

> Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki. Ogólnie rzecz biorąc uruchamianie animacji jest wyzwalana przez interakcji użytkownika z tej samej kontrolki. Jest jednak również możliwość oddziałują na jeden formant, a następnie animacji innej kontrolki.


## <a name="overview"></a>Omówienie

Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki. Ogólnie rzecz biorąc uruchamianie animacji jest wyzwalana przez interakcji użytkownika z tej samej kontrolki. Jest jednak również możliwość oddziałują na jeden formant, a następnie animacji innej kontrolki.

## <a name="steps"></a>Kroki

Po pierwsze, obejmują `ScriptManager` w strony, a następnie biblioteki ASP.NET AJAX jest ładowany, dzięki czemu można użyć zestawu narzędzi kontroli:

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample1.aspx)]

Animacja zostanie zastosowana do panelu tekstu, który wygląda w następujący sposób:

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample2.aspx)]

W skojarzone klasy CSS do obsługi panelu zdefiniowany jako kolor tła dobre rozwiązanie, a także ustawić stała szerokość panelu:

[!code-css[Main](triggering-an-animation-in-another-control-vb/samples/sample3.css)]

Aby rozpocząć, animowanie panelu, przycisk HTML jest używany. Należy pamiętać, że `<input type="button" />` jest uprzywilejowanym za pośrednictwem `<asp:Button />` ponieważ nie chcemy ogłaszania zwrotnego po kliknięciu tego przycisku przez użytkownika.

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample4.aspx)]

Następnie należy dodać `AnimationExtender` do strony, zapewniając `ID`, `TargetControlID` atrybut i obowiązkowe `runat="server"`. Jest ważne, aby ustawić `TargetControlID` identyfikator przycisku (element wyzwalanie animacji), nie identyfikatorowi panelu (elementu, jest animowany)

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample5.aspx)]

W ramach `<Animations>` węzła, animacje miejsce w zwykły sposób. Aby stały się zmienić w panelu, ustaw nie przycisk `AnimationTarget` atrybutu dla każdego elementu animacji w ramach `AnimationExtender`. Wartość `AnimationTarget` oczywiście jest to identyfikator panelu. W ten sposób animacji się zdarzyć, za pomocą panelu, nie za pomocą przycisku wyzwalania. Oto `AnimationExtender` znaczników dla tego scenariusza:

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample6.aspx)]

Należy zwrócić uwagę specjalne kolejność, w jakiej są wyświetlane poszczególne animacji. Po pierwsze przycisk pobiera dezaktywowane, po uruchomieniu animacji. Ponieważ ma nie `AnimationTarget` atrybutu w `<EnableAction>` elementu, ta animacja jest stosowana do sterowania źródłowy: przycisku. Kroki opisane w dwóch następnych animacji są przeprowadzane parallelly (`<Parallel>` elementu). Zachowują się ich `AnimationTarget` Ustaw atrybuty `"Panel1"`, dlatego animowanie panelu nie przycisk.


[![Kliknięcie na przycisk uruchamiania animacji panelu](triggering-an-animation-in-another-control-vb/_static/image2.png)](triggering-an-animation-in-another-control-vb/_static/image1.png)

Kliknięcie na przycisk uruchamia panel animacji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](triggering-an-animation-in-another-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](disabling-actions-during-animation-vb.md)
> [dalej](modifying-animations-from-the-server-side-vb.md)

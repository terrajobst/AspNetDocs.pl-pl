---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-vb
title: Wykonywanie kilku animacji jedna po drugiej (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki. Umożliwia uruchamianie severa...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 21ece509-79cc-4d9d-892d-7b6e9c4d3502
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-vb
msc.type: authoredcontent
ms.openlocfilehash: 89412c078bbe40f06d31327d0a17bf3ea8bc8314
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57074423"
---
<a name="executing-several-animations-after-each-other-vb"></a>Wykonywanie kilku animacji jedna po drugiej (VB)
====================
przez [Christian Wenz](https://github.com/wenz)

[Pobierz program Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.vb.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3VB.pdf)

> Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki. Umożliwia uruchamianie kilku animacji jedna po drugiej.


## <a name="overview"></a>Omówienie

Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki. Umożliwia uruchamianie kilku animacji jedna po drugiej.

## <a name="steps"></a>Kroki

Po pierwsze, obejmują `ScriptManager` w strony, a następnie biblioteki ASP.NET AJAX jest ładowany, dzięki czemu można użyć zestawu narzędzi kontroli:

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample1.aspx)]

Animacja zostanie zastosowana do panelu tekstu, który wygląda w następujący sposób:

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample2.aspx)]

W skojarzone klasy CSS do obsługi panelu zdefiniowany jako kolor tła dobre rozwiązanie, a także ustawić stała szerokość panelu:

[!code-css[Main](executing-several-animations-after-each-other-vb/samples/sample3.css)]

Następnie należy dodać `AnimationExtender` do strony, zapewniając `ID`, `TargetControlID` atrybut i obowiązkowe `runat="server":`

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample4.aspx)]

W ramach `<Animations>` węzła, użyj `<OnLoad>` do uruchamiania animacji, po całkowitym załadowaniu strony. Ogólnie rzecz biorąc `<OnLoad>` akceptuje tylko jednej animacji. Framework animacji umożliwia Cię do dołączenia do kilku animacji w ją przy użyciu `<Sequence>` elementu. Wszystkie animacje w ramach `<Sequence>` są wykonane jeden po drugim. Poniżej przedstawiono możliwe kod znaczników dla `AnimationExtender` kontrolki, najpierw dokonując panelu szerszy i następnie zmniejszenie jego wysokość:

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample5.aspx)]

Po uruchomieniu tego skryptu panelu otrzymuje szerszy i następnie mniejsze.


[![Najpierw jest zwiększana szerokość](executing-several-animations-after-each-other-vb/_static/image2.png)](executing-several-animations-after-each-other-vb/_static/image1.png)

Najpierw jest zwiększana szerokość ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](executing-several-animations-after-each-other-vb/_static/image3.png))


[![Zmniejszy wysokość](executing-several-animations-after-each-other-vb/_static/image5.png)](executing-several-animations-after-each-other-vb/_static/image4.png)

Następnie zmniejszyła się wysokość ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](executing-several-animations-after-each-other-vb/_static/image6.png))

> [!div class="step-by-step"]
> [Poprzednie](executing-several-animations-at-the-same-time-vb.md)
> [dalej](animation-depending-on-a-condition-vb.md)

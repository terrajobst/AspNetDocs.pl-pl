---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-vb
title: Wykonywanie kilku animacji w tym samym czasie (VB) | Microsoft Docs
author: wenz
description: Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. Umożliwia uruchomienie serwera Sever...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 2469f7ea-1489-42fb-a8e1-414c90141ce9
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-vb
msc.type: authoredcontent
ms.openlocfilehash: fc11debd06c897c29db56d42167d483f5608cdf8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78614255"
---
# <a name="executing-several-animations-at-the-same-time-vb"></a>Wykonywanie kilku animacji w tym samym czasie (VB)

Autor [Christian Wenz](https://github.com/wenz)

[Pobierz kod](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation2.vb.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation2VB.pdf)

> Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. Umożliwia równoległe uruchamianie kilku animacji.

## <a name="overview"></a>Omówienie

Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. Umożliwia równoległe uruchamianie kilku animacji.

## <a name="steps"></a>Kroki

Po pierwsze, Uwzględnij `ScriptManager` na stronie; następnie załadowana zostanie Biblioteka ASP.NET AJAX, co umożliwia korzystanie z zestawu narzędzi kontroli:

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample1.aspx)]

Animacja zostanie zastosowana do panelu tekstu, który wygląda następująco:

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample2.aspx)]

W skojarzonej klasie CSS panelu, zdefiniuj kolor tła całkiem, a także Ustaw stałą szerokość panelu:

[!code-css[Main](executing-several-animations-at-the-same-time-vb/samples/sample3.css)]

Następnie Dodaj `AnimationExtender` do strony, podając `ID`, atrybut `TargetControlID` i obowiązkowe `runat="server"`:

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample4.aspx)]

W węźle `<Animations>` Użyj `<OnLoad>`, aby uruchomić animacje po całkowitym załadowaniu strony. Ogólnie rzecz biorąc, `<OnLoad>` akceptuje tylko jedną animację. Struktura animacji umożliwia sprzęganie kilku animacji w jeden przy użyciu elementu `<Parallel>`. Wszystkie animacje w `<Parallel>` są wykonywane w tym samym czasie.

Oto możliwe znaczniki dla kontrolki `AnimationExtender`, Znikające i zmieniające rozmiar panelu w tym samym czasie:

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample5.aspx)]

W rzeczywistości: po uruchomieniu tego skryptu panel zostanie wyświetlony, a następnie zmienia rozmiar (więcej niż podróżuje jego szerokość i halving jego wysokość) i stopniowo znika.

[![panel jest znikający i zmienia rozmiar (łącznie z jego zawartością, dzięki czemu aparat renderowania przeglądarki)](executing-several-animations-at-the-same-time-vb/_static/image2.png)](executing-several-animations-at-the-same-time-vb/_static/image1.png)

Panel jest odtwarzany i zmienia rozmiar (w tym jego zawartość, Dzięki aparatowi renderowania przeglądarki) ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](executing-several-animations-at-the-same-time-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](adding-animation-to-a-control-vb.md)
> [dalej](executing-several-animations-after-each-other-vb.md)

---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-vb
title: Wykonywanie kilku animacji między sobą (VB) | Microsoft Docs
author: wenz
description: Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. Umożliwia uruchomienie serwera Sever...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 21ece509-79cc-4d9d-892d-7b6e9c4d3502
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-vb
msc.type: authoredcontent
ms.openlocfilehash: 5034fe905299d4559b713dd841cb166886179c39
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598134"
---
# <a name="executing-several-animations-after-each-other-vb"></a>Wykonywanie kilku animacji jedna po drugiej (VB)

Autor [Christian Wenz](https://github.com/wenz)

[Pobierz kod](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.vb.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3VB.pdf)

> Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. Umożliwia uruchamianie kilku animacji jeden po drugim.

## <a name="overview"></a>Omówienie

Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. Umożliwia uruchamianie kilku animacji jeden po drugim.

## <a name="steps"></a>Kroki

Po pierwsze, Uwzględnij `ScriptManager` na stronie; następnie załadowana zostanie Biblioteka ASP.NET AJAX, co umożliwia korzystanie z zestawu narzędzi kontroli:

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample1.aspx)]

Animacja zostanie zastosowana do panelu tekstu, który wygląda następująco:

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample2.aspx)]

W skojarzonej klasie CSS panelu, zdefiniuj kolor tła całkiem, a także Ustaw stałą szerokość panelu:

[!code-css[Main](executing-several-animations-after-each-other-vb/samples/sample3.css)]

Następnie Dodaj `AnimationExtender` do strony, podając `ID`, atrybut `TargetControlID` i obowiązkowe `runat="server":`

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample4.aspx)]

W węźle `<Animations>` Użyj `<OnLoad>`, aby uruchomić animacje po całkowitym załadowaniu strony. Ogólnie rzecz biorąc, `<OnLoad>` akceptuje tylko jedną animację. Struktura animacji umożliwia sprzęganie kilku animacji w jeden przy użyciu elementu `<Sequence>`. Wszystkie animacje w `<Sequence>` są wykonywane jeden po drugim. Oto możliwe znaczniki dla kontrolki `AnimationExtender`, najpierw czynijąc panel, a następnie zmniejszając jego wysokość:

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample5.aspx)]

Po uruchomieniu tego skryptu panel najpierw uzyskuje szerszy rozmiar, a następnie mniejszy.

[![pierwsze zwiększenie szerokości](executing-several-animations-after-each-other-vb/_static/image2.png)](executing-several-animations-after-each-other-vb/_static/image1.png)

Najpierw Zwiększ szerokość (kliknij,[Aby wyświetlić obraz w pełnym rozmiarze](executing-several-animations-after-each-other-vb/_static/image3.png))

[![następnie wysokość zostanie zmniejszona](executing-several-animations-after-each-other-vb/_static/image5.png)](executing-several-animations-after-each-other-vb/_static/image4.png)

Następnie wysokość zostanie zmniejszona ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](executing-several-animations-after-each-other-vb/_static/image6.png))

> [!div class="step-by-step"]
> [Poprzednie](executing-several-animations-at-the-same-time-vb.md)
> [dalej](animation-depending-on-a-condition-vb.md)

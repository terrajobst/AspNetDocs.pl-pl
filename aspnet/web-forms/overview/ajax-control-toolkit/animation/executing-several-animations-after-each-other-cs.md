---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-cs
title: Wykonywanie kilku animacji między sobą (C#) | Microsoft Docs
author: wenz
description: Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. Umożliwia uruchomienie serwera Sever...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 7dc02b18-2b5d-4844-b7c5-cbd818477163
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-cs
msc.type: authoredcontent
ms.openlocfilehash: e378ac038796dbd44e89d099532b9e186dcf673f
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74575454"
---
# <a name="executing-several-animations-after-each-other-c"></a>Wykonywanie kilku animacji jedna po drugiej (C#)

Autor [Christian Wenz](https://github.com/wenz)

[Pobierz kod](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.cs.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3CS.pdf)

> Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. Umożliwia uruchamianie kilku animacji jeden po drugim.

Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. Umożliwia uruchamianie kilku animacji jeden po drugim.

## <a name="steps"></a>Kroki

Po pierwsze, Uwzględnij `ScriptManager` na stronie; następnie załadowana zostanie Biblioteka ASP.NET AJAX, co umożliwia korzystanie z zestawu narzędzi kontroli:

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample1.aspx)]

Animacja zostanie zastosowana do panelu tekstu, który wygląda następująco:

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample2.aspx)]

W skojarzonej klasie CSS panelu, zdefiniuj kolor tła całkiem, a także Ustaw stałą szerokość panelu:

[!code-css[Main](executing-several-animations-after-each-other-cs/samples/sample3.css)]

Następnie Dodaj `AnimationExtender` do strony, podając `ID`, atrybut `TargetControlID` i obowiązkowe `runat="server":`

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample4.aspx)]

W węźle `<Animations>` Użyj `<OnLoad>`, aby uruchomić animacje po całkowitym załadowaniu strony. Ogólnie rzecz biorąc, `<OnLoad>` akceptuje tylko jedną animację. Struktura animacji umożliwia sprzęganie kilku animacji w jeden przy użyciu elementu `<Sequence>`. Wszystkie animacje w `<Sequence>` są wykonywane jeden po drugim. Oto możliwe znaczniki dla kontrolki `AnimationExtender`, najpierw czynijąc panel, a następnie zmniejszając jego wysokość:

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample5.aspx)]

Po uruchomieniu tego skryptu panel najpierw uzyskuje szerszy rozmiar, a następnie mniejszy.

[![pierwsze zwiększenie szerokości](executing-several-animations-after-each-other-cs/_static/image2.png)](executing-several-animations-after-each-other-cs/_static/image1.png)

Najpierw Zwiększ szerokość (kliknij,[Aby wyświetlić obraz w pełnym rozmiarze](executing-several-animations-after-each-other-cs/_static/image3.png))

[![następnie wysokość zostanie zmniejszona](executing-several-animations-after-each-other-cs/_static/image5.png)](executing-several-animations-after-each-other-cs/_static/image4.png)

Następnie wysokość zostanie zmniejszona ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](executing-several-animations-after-each-other-cs/_static/image6.png))

> [!div class="step-by-step"]
> [Poprzednie](executing-several-animations-at-the-same-time-cs.md)
> [dalej](animation-depending-on-a-condition-cs.md)

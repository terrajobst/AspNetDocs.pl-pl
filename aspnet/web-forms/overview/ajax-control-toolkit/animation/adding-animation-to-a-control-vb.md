---
uid: web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-vb
title: Dodawanie animacji do kontrolki (VB) | Microsoft Docs
author: wenz
description: Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. W tym samouczku pokazano, jak...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: c120187e-963e-4439-bb85-32771bc7f1f4
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-vb
msc.type: authoredcontent
ms.openlocfilehash: efaee9c1665d795dc1a889b9ac9f25dd1c08f4e2
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74607180"
---
# <a name="adding-animation-to-a-control-vb"></a>Dodawanie animacji do kontrolki (VB)

Autor [Christian Wenz](https://github.com/wenz)

[Pobierz kod](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.vb.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1VB.pdf)

> Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. W tym samouczku pokazano, jak skonfigurować taką animację.

## <a name="overview"></a>Omówienie

Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. W tym samouczku pokazano, jak skonfigurować taką animację.

## <a name="steps"></a>Kroki

Pierwszym krokiem jest zwykle uwzględnienie `ScriptManager` na stronie, tak aby Biblioteka ASP.NET AJAX została załadowana i można było użyć zestawu narzędzi do sterowania:

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample1.aspx)]

Animacja w tym scenariuszu zostanie zastosowana do panelu tekstu, który wygląda następująco:

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample2.aspx)]

Skojarzona Klasa CSS panelu definiuje kolor tła i Szerokość:

[!code-css[Main](adding-animation-to-a-control-vb/samples/sample3.css)]

Następnie potrzebujemy `AnimationExtender`. Po podania `ID` i zwykłych `runat="server"`, atrybut `TargetControlID` musi być ustawiony na kontrolkę do animacji w naszym przypadku, panel:

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample4.aspx)]

Cała animacja jest stosowana deklaratywnie, przy użyciu składni XML, niestety obecnie nie jest w pełni obsługiwana przez funkcję IntelliSense programu Visual Studio. Węzeł główny jest `<Animations>;` w tym węźle, ale jest dozwolonych kilka zdarzeń, które określają, kiedy wykonane są następujące animacje:

- `OnClick` (kliknięcie myszą)
- `OnHoverOut` (gdy mysz opuści formant)
- `OnHoverOver` (gdy wskaźnik myszy jest przesuwany nad kontrolką, zatrzymywanie animacji `OnHoverOut`)
- `OnLoad` (gdy strona została załadowana)
- `OnMouseOut` (gdy mysz opuści formant)
- `OnMouseOver` (gdy wskaźnik myszy znajduje się nad kontrolką, nie zatrzyma animacji `OnMouseOut`)

Struktura zawiera zestaw animacji, z których każdy jest reprezentowany przez własny element XML. Oto wybór:

- `<Color>` (zmiana koloru)
- `<FadeIn>` (Zanikanie w)
- `<FadeOut>` (Zanikanie)
- `<Property>` (zmiana właściwości kontrolki)
- `<Pulse>` (pulsating)
- `<Resize>` (zmiana rozmiaru)
- `<Scale>` (proporcjonalnie do zmiany rozmiaru)

W tym przykładzie panel stopniowo rozjaśnia się. Animacja przyjmuje 1,5 sekund (`Duration` atrybutu), wyświetlając 24 klatki (kroki animacji) na sekundę (atrybut`Fps`). Oto kompletny znacznik dla kontrolki `AnimationExtender`:

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample5.aspx)]

Po uruchomieniu tego skryptu panel zostanie wyświetlony i zanika w ciągu jednej i pół sekundy.

[![panel jest znikający](adding-animation-to-a-control-vb/_static/image2.png)](adding-animation-to-a-control-vb/_static/image1.png)

Panel powoduje zanikanie ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](adding-animation-to-a-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](dynamically-controlling-updatepanel-animations-cs.md)
> [dalej](executing-several-animations-at-the-same-time-vb.md)

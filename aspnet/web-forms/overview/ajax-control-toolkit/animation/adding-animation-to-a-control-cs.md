---
uid: web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-cs
title: Dodawanie animacji do kontrolki (C#) | Microsoft Docs
author: wenz
description: Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. W tym samouczku pokazano, jak...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 0f1fc1f5-9dbd-44e7-931e-387d42f0342b
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-cs
msc.type: authoredcontent
ms.openlocfilehash: dd63157fe616c5f6874b7cca11f4ede15018df04
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78614451"
---
# <a name="adding-animation-to-a-control-c"></a>Dodawanie animacji do kontrolki (C#)

Autor [Christian Wenz](https://github.com/wenz)

[Pobierz kod](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.cs.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1CS.pdf)

> Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. W tym samouczku pokazano, jak skonfigurować taką animację.

## <a name="overview"></a>Omówienie

Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. W tym samouczku pokazano, jak skonfigurować taką animację.

## <a name="steps"></a>Kroki

Pierwszym krokiem jest zwykle uwzględnienie `ScriptManager` na stronie, tak aby Biblioteka ASP.NET AJAX została załadowana i można było użyć zestawu narzędzi do sterowania:

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample1.aspx)]

Animacja w tym scenariuszu zostanie zastosowana do panelu tekstu, który wygląda następująco:

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample2.aspx)]

Skojarzona Klasa CSS panelu definiuje kolor tła i Szerokość:

[!code-css[Main](adding-animation-to-a-control-cs/samples/sample3.css)]

Następnie potrzebujemy `AnimationExtender`. Po podania `ID` i zwykłych `runat="server"`, atrybut `TargetControlID` musi być ustawiony na kontrolkę do animacji w naszym przypadku, panel:

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample4.aspx)]

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

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample5.aspx)]

Po uruchomieniu tego skryptu panel zostanie wyświetlony i zanika w ciągu jednej i pół sekundy.

[![panel jest znikający](adding-animation-to-a-control-cs/_static/image2.png)](adding-animation-to-a-control-cs/_static/image1.png)

Panel powoduje zanikanie ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](adding-animation-to-a-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Dalej](executing-several-animations-at-the-same-time-cs.md)

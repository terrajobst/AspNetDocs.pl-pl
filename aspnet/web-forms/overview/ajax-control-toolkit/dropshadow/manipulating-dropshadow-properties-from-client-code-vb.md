---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
title: Manipulowanie właściwościami DropShadow z poziomu kodu klienta (VB) | Microsoft Docs
author: wenz
description: Kontrolka DropShadow w zestawie narzędzi AJAX Control rozszerza panel z cieniem. Właściwości tego rozszerzenia można także zmienić za pomocą JavaScrip klienta...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 11be4211-2fb9-4e15-b6d4-2aa623d81f3e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
msc.type: authoredcontent
ms.openlocfilehash: a39adb9c06819f6f828add7d762effad430b8570
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78613800"
---
# <a name="manipulating-dropshadow-properties-from-client-code-vb"></a>Manipulowanie właściwościami kontrolki DropShadow z poziomu kodu klienta (VB)

Autor [Christian Wenz](https://github.com/wenz)

[Pobierz kod](https://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.vb.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2VB.pdf)

> Kontrolka DropShadow w zestawie narzędzi AJAX Control rozszerza panel z cieniem. Właściwości tego rozszerzenia można także zmienić przy użyciu kodu JavaScript klienta.

## <a name="overview"></a>Omówienie

Kontrolka DropShadow w zestawie narzędzi AJAX Control rozszerza panel z cieniem. Właściwości tego rozszerzenia można także zmienić przy użyciu kodu JavaScript klienta.

## <a name="steps"></a>Kroki

Kod jest uruchamiany z panelem zawierającym kilka wierszy tekstu:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample1.aspx)]

Skojarzona Klasa CSS daje panelowi całkiem kolor tła:

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample2.css)]

`DropShadowExtender` zostanie dodany, aby zwiększyć panel z efektem cienia, nieprzezroczystość ustawiona na 50%:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample3.aspx)]

Następnie formant `ScriptManager` AJAX ASP.NET umożliwia działanie zestawu narzędzi sterowania:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample4.aspx)]

Inny panel zawiera dwa linki JavaScript do ustawiania nieprzezroczystości cieniowania: łącze minus zmniejsza nieprzezroczystość w tle, a link Plus go zwiększa.

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample5.aspx)]

Funkcja JavaScript `changeOpacity()` musi najpierw znaleźć kontrolkę `DropShadowExtender` na stronie. ASP.NET AJAX definiuje metodę `$find()` dla tego zadania. Następnie Metoda `get_Opacity()` pobiera bieżącą nieprzezroczystość, Metoda `set_Opacity()` ustawia ją. Kod JavaScript umieszcza bieżącą wartość nieprzezroczystości w `<label>` elemencie:

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample6.html)]

[![na stronie klienta zmieniono nieprzezroczystość](manipulating-dropshadow-properties-from-client-code-vb/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-vb/_static/image1.png)

Nieprzezroczystość jest zmieniana po stronie klienta ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](manipulating-dropshadow-properties-from-client-code-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Wstecz](adjusting-the-z-index-of-a-dropshadow-vb.md)

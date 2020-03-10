---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
title: Manipulowanie właściwościami DropShadow z poziomu koduC#klienta () | Microsoft Docs
author: wenz
description: Dostosowywanie interfejsu edycji elementu DataList
ms.author: riande
ms.date: 06/02/2008
ms.assetid: c83ca3e6-c0bf-4158-a166-40c1ab0f33da
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 790f0d881e43518600968d6c175d4eaa53d0e5f9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78613821"
---
# <a name="manipulating-dropshadow-properties-from-client-code-c"></a>Manipulowanie właściwościami kontrolki DropShadow z poziomu kodu klienta (C#)

Autor [Christian Wenz](https://github.com/wenz)

[Pobierz kod](https://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.cs.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2CS.pdf)

> Kontrolka DropShadow w zestawie narzędzi AJAX Control rozszerza panel z cieniem. Właściwości tego rozszerzenia można także zmienić przy użyciu kodu JavaScript klienta.

## <a name="overview"></a>Omówienie

Kontrolka DropShadow w zestawie narzędzi AJAX Control rozszerza panel z cieniem. Właściwości tego rozszerzenia można także zmienić przy użyciu kodu JavaScript klienta.

## <a name="steps"></a>Kroki

Kod jest uruchamiany z panelem zawierającym kilka wierszy tekstu:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample1.aspx)]

Skojarzona Klasa CSS daje panelowi całkiem kolor tła:

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample2.css)]

`DropShadowExtender` zostanie dodany, aby zwiększyć panel z efektem cienia, nieprzezroczystość ustawiona na 50%:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample3.aspx)]

Następnie formant `ScriptManager` AJAX ASP.NET umożliwia działanie zestawu narzędzi sterowania:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample4.aspx)]

Inny panel zawiera dwa linki JavaScript do ustawiania nieprzezroczystości cieniowania: łącze minus zmniejsza nieprzezroczystość w tle, a link Plus go zwiększa.

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample5.aspx)]

Funkcja JavaScript `changeOpacity()` musi najpierw znaleźć kontrolkę `DropShadowExtender` na stronie. ASP.NET AJAX definiuje metodę `$find()` dla tego zadania. Następnie Metoda `get_Opacity()` pobiera bieżącą nieprzezroczystość, Metoda `set_Opacity()` ustawia ją. Kod JavaScript umieszcza bieżącą wartość nieprzezroczystości w `<label>` elemencie:

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample6.html)]

[![na stronie klienta zmieniono nieprzezroczystość](manipulating-dropshadow-properties-from-client-code-cs/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-cs/_static/image1.png)

Nieprzezroczystość jest zmieniana po stronie klienta ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](manipulating-dropshadow-properties-from-client-code-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](adjusting-the-z-index-of-a-dropshadow-cs.md)
> [dalej](adjusting-the-z-index-of-a-dropshadow-vb.md)

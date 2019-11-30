---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-cs
title: Dostosowywanie indeksu Z DropShadow (C#) | Microsoft Docs
author: wenz
description: Kontrolka DropShadow w zestawie narzędzi AJAX Control rozszerza panel z cieniem. Jednak ten cień czasami powoduje konflikt z innymi kontrolkami, dla insta...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 14133833-e518-4347-87b9-6b6f71f14a77
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-cs
msc.type: authoredcontent
ms.openlocfilehash: 12bc7f0430f1f30ff964cd9547ee1e9b0aa7423c
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74574301"
---
# <a name="adjusting-the-z-index-of-a-dropshadow-c"></a>Dostosowywanie indeksu Z kontrolki DropShadow (C#)

Autor [Christian Wenz](https://github.com/wenz)

[Pobierz kod](https://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.cs.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1CS.pdf)

> Kontrolka DropShadow w zestawie narzędzi AJAX Control rozszerza panel z cieniem. Jednak ten cień czasami powoduje konflikt z innymi kontrolkami, na przykład formant menu ASP.NET. Gdy pojawi się pozycja menu, zostanie wyświetlona w tle.

## <a name="overview"></a>Omówienie

Kontrolka DropShadow w zestawie narzędzi AJAX Control rozszerza panel z cieniem. Jednak ten cień czasami powoduje konflikt z innymi kontrolkami, na przykład formant menu ASP.NET. Gdy pojawi się pozycja menu, zostanie wyświetlona w tle.

## <a name="steps"></a>Kroki

Kod rozpoczyna się od samego panelu, zawierający wystarczająco dużo tekstu, aby panel zawiera wystarczającą ilość tekstu, aby efekt był widoczny:

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample1.aspx)]

Inny panel jest umieszczany bezpośrednio przed panelem `panelShadow`. Zawiera menu z orientacją poziomą, tak aby wpisy menu były widoczne (lub zamiast: w obszarze) panelu `dropShadow`):

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample2.aspx)]

Następnie `DropShadowExtender` zostanie dodany w celu rozwinięcia panelu `panelShadow` z efektem cienia:

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample3.aspx)]

Na koniec formant `ScriptManager` AJAX ASP.NET umożliwia działanie zestawu narzędzi sterowania:

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample4.aspx)]

Po uruchomieniu tego skryptu pozycje menu są wyświetlane poniżej panelu. Jednak menu używa klasy CSS `panel`, gdzie wystarczy zdefiniować dwie rzeczy, aby elementy były wyświetlane przed drugim panelem:

- Pozycjonowanie względne
- Pozytywny indeks z

[!code-css[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample5.css)]

Następnie formant `DropShadowExtender` nie powoduje konfliktu z kontrolką menu.

[![przed: element menu nie jest widoczny](adjusting-the-z-index-of-a-dropshadow-cs/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image1.png)

Przed: element menu nie jest widoczny ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](adjusting-the-z-index-of-a-dropshadow-cs/_static/image3.png))

[![po: pojawia się pozycja menu](adjusting-the-z-index-of-a-dropshadow-cs/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image4.png)

Po: pojawia się pozycja menu ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](adjusting-the-z-index-of-a-dropshadow-cs/_static/image6.png))

> [!div class="step-by-step"]
> [Next](manipulating-dropshadow-properties-from-client-code-cs.md)

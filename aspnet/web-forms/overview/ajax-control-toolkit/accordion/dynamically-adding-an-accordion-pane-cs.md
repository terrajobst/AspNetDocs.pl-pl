---
uid: web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-cs
title: Dynamiczne dodawanie okienka kontrolki Accordion (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolki właściwości Accordion na zestawu narzędzi AJAX Control Toolkit zawiera wiele okienek i umożliwia użytkownikowi wyświetlanie jednego z nich jednocześnie. Panele zwykle są deklarowane w...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 66d88cfa-f26f-46b1-ad52-1c9e03c04a48
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-cs
msc.type: authoredcontent
ms.openlocfilehash: 7134c95845ec7f22b5216e10b50ab8f81cd24806
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65131251"
---
# <a name="dynamically-adding-an-accordion-pane-c"></a>Dynamiczne dodawanie okienka kontrolki Accordion (C#)

przez [Christian Wenz](https://github.com/wenz)

[Pobierz program Code](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.cs.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2CS.pdf)

> Kontrolki właściwości Accordion na zestawu narzędzi AJAX Control Toolkit zawiera wiele okienek i umożliwia użytkownikowi wyświetlanie jednego z nich jednocześnie. Panele zwykle są zadeklarowane w obrębie sama strona, ale kod po stronie serwera może służyć do ten sam efekt.

## <a name="overview"></a>Omówienie

Kontrolki właściwości Accordion na zestawu narzędzi AJAX Control Toolkit zawiera wiele okienek i umożliwia użytkownikowi wyświetlanie jednego z nich jednocześnie. Panele zwykle są zadeklarowane w obrębie sama strona, ale kod po stronie serwera może służyć do ten sam efekt.

## <a name="steps"></a>Kroki

Kontrolka właściwości Accordion uwidacznia wszystkie ważne właściwości do kodu po stronie serwera. Między innymi `Panes` właściwość udziela dostępu do kolekcja okienek, które tworzą właściwości Accordion. Okienka, co jest typu `AccordionPane`. W związku z tym jest prosta, aby utworzyć takie okienka:

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample1.cs)]

`HeaderContainer` Właściwość `AccordionPane` zapewnia dostęp do kontrolek ASP.NET, w ramach sekcji nagłówka okienka; `ContentContainer` właściwość `AccordionPane` działa tak samo dla zawartości sekcji okienka. Dzięki temu kod platformy ASP.NET do dodawania zawartości do okienka:

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample2.cs)]

Na koniec pane(s) muszą zostać dodane do `Panes` zbiór właściwości Accordion:

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample3.cs)]

Oto kompletny kod po stronie serwera, który dodaje dwa okienka do kontrolki właściwości Accordion:

[!code-aspx[Main](dynamically-adding-an-accordion-pane-cs/samples/sample4.aspx)]

Jedynym elementem brak jest właściwości Accordion, która zależy od obecności ASP.NET `ScriptManager` sterowania:

[!code-aspx[Main](dynamically-adding-an-accordion-pane-cs/samples/sample5.aspx)]

Aby zakończyć w przykładzie, dwóch klas CSS, do którego odwołuje się kontroli właściwości Accordion zawierają informacje dotyczące stylu przeglądarki:

[!code-css[Main](dynamically-adding-an-accordion-pane-cs/samples/sample6.css)]

[![Dane w właściwości accordion dynamicznie został dodany przez kod po stronie serwera](dynamically-adding-an-accordion-pane-cs/_static/image2.png)](dynamically-adding-an-accordion-pane-cs/_static/image1.png)

Dane w właściwości accordion dynamicznie został dodany przez kod po stronie serwera ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](dynamically-adding-an-accordion-pane-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](databinding-to-an-accordion-cs.md)
> [dalej](databinding-to-an-accordion-vb.md)

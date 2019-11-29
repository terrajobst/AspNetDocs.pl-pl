---
uid: web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-vb
title: Dynamiczne dodawanie okienka zgodnie z opisem (VB) | Microsoft Docs
author: wenz
description: Kontrola zgodnie z zestawem narzędzi AJAX Control Toolkit udostępnia wiele okienek i umożliwia użytkownikowi wyświetlanie jednego z nich naraz. Panele są zwykle zadeklarowane w...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: fae968c9-1902-487d-b053-86a46dd52c3f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-vb
msc.type: authoredcontent
ms.openlocfilehash: be48db5ea3de4af46b0f864cc9e73d2f518294a4
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74607201"
---
# <a name="dynamically-adding-an-accordion-pane-vb"></a>Dynamiczne dodawanie okienka zgodnie z przepisami (VB)

Autor [Christian Wenz](https://github.com/wenz)

[Pobierz kod](https://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.vb.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2VB.pdf)

> Kontrola zgodnie z zestawem narzędzi AJAX Control Toolkit udostępnia wiele okienek i umożliwia użytkownikowi wyświetlanie jednego z nich naraz. Panele są zwykle deklarowane na samej stronie, ale kod po stronie serwera może być używany do osiągnięcia tego samego wyniku.

## <a name="overview"></a>Omówienie

Kontrola zgodnie z zestawem narzędzi AJAX Control Toolkit udostępnia wiele okienek i umożliwia użytkownikowi wyświetlanie jednego z nich naraz. Panele są zwykle deklarowane na samej stronie, ale kod po stronie serwera może być używany do osiągnięcia tego samego wyniku.

## <a name="steps"></a>Kroki

Kontrola wystawcy uwidacznia wszystkie ważne właściwości kodu po stronie serwera. W innych przypadkach Właściwość `Panes` udziela dostępu do kolekcji okienek, które składają się na siebie zgodnie z zasadami. Każde okienko jest typu `AccordionPane`. W związku z tym nie można utworzyć takiego okienka:

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample1.vb)]

Właściwość `HeaderContainer` `AccordionPane` zapewnia dostęp do kontrolek ASP.NET w sekcji nagłówka okienka; Właściwość `ContentContainer` `AccordionPane` jest taka sama dla sekcji zawartość okienka. Dzięki temu kod ASP.NET może dodawać zawartość do okienek:

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample2.vb)]

Na koniec należy dodać okienka (y) do `Panes` kolekcji:

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample3.vb)]

Oto kompletny kod po stronie serwera, który dodaje dwa okienka do kontrolki przydzielenia:

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample4.aspx)]

Jedyny brakujący element to samo samo, co zależy od obecności kontrolki `ScriptManager` ASP.NET:

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample5.aspx)]

Aby zakończyć ten przykład, dwie klasy CSS, do których odwołuje się kontrolka, udostępniają informacje o stylu dla przeglądarki:

[!code-css[Main](dynamically-adding-an-accordion-pane-vb/samples/sample6.css)]

[![dane w ramach traktowania zostały dynamicznie dodane przez kod po stronie serwera](dynamically-adding-an-accordion-pane-vb/_static/image2.png)](dynamically-adding-an-accordion-pane-vb/_static/image1.png)

Dane w postawce zostały dynamicznie dodane przez kod po stronie serwera ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](dynamically-adding-an-accordion-pane-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Ubiegł](databinding-to-an-accordion-vb.md)

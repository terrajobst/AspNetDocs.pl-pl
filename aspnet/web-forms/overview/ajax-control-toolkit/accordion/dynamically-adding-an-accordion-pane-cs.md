---
uid: web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-cs
title: Dynamiczne dodawanie okienka zgodnie z (C#) | Microsoft Docs
author: wenz
description: Kontrola zgodnie z zestawem narzędzi AJAX Control Toolkit udostępnia wiele okienek i umożliwia użytkownikowi wyświetlanie jednego z nich naraz. Panele są zwykle zadeklarowane w...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 66d88cfa-f26f-46b1-ad52-1c9e03c04a48
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-cs
msc.type: authoredcontent
ms.openlocfilehash: 2834f56bd77c412923f4a8f382e670727f70eae4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78614465"
---
# <a name="dynamically-adding-an-accordion-pane-c"></a>Dynamiczne dodawanie okienka zgodnie z przepisami (C#)

Autor [Christian Wenz](https://github.com/wenz)

[Pobierz kod](https://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.cs.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2CS.pdf)

> Kontrola zgodnie z zestawem narzędzi AJAX Control Toolkit udostępnia wiele okienek i umożliwia użytkownikowi wyświetlanie jednego z nich naraz. Panele są zwykle deklarowane na samej stronie, ale kod po stronie serwera może być używany do osiągnięcia tego samego wyniku.

## <a name="overview"></a>Omówienie

Kontrola zgodnie z zestawem narzędzi AJAX Control Toolkit udostępnia wiele okienek i umożliwia użytkownikowi wyświetlanie jednego z nich naraz. Panele są zwykle deklarowane na samej stronie, ale kod po stronie serwera może być używany do osiągnięcia tego samego wyniku.

## <a name="steps"></a>Kroki

Kontrola wystawcy uwidacznia wszystkie ważne właściwości kodu po stronie serwera. W innych przypadkach Właściwość `Panes` udziela dostępu do kolekcji okienek, które składają się na siebie zgodnie z zasadami. Każde okienko jest typu `AccordionPane`. W związku z tym nie można utworzyć takiego okienka:

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample1.cs)]

Właściwość `HeaderContainer` `AccordionPane` zapewnia dostęp do kontrolek ASP.NET w sekcji nagłówka okienka; Właściwość `ContentContainer` `AccordionPane` jest taka sama dla sekcji zawartość okienka. Dzięki temu kod ASP.NET może dodawać zawartość do okienek:

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample2.cs)]

Na koniec należy dodać okienka (y) do `Panes` kolekcji:

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample3.cs)]

Oto kompletny kod po stronie serwera, który dodaje dwa okienka do kontrolki przydzielenia:

[!code-aspx[Main](dynamically-adding-an-accordion-pane-cs/samples/sample4.aspx)]

Jedyny brakujący element to samo samo, co zależy od obecności kontrolki `ScriptManager` ASP.NET:

[!code-aspx[Main](dynamically-adding-an-accordion-pane-cs/samples/sample5.aspx)]

Aby zakończyć ten przykład, dwie klasy CSS, do których odwołuje się kontrolka, udostępniają informacje o stylu dla przeglądarki:

[!code-css[Main](dynamically-adding-an-accordion-pane-cs/samples/sample6.css)]

[![dane w ramach traktowania zostały dynamicznie dodane przez kod po stronie serwera](dynamically-adding-an-accordion-pane-cs/_static/image2.png)](dynamically-adding-an-accordion-pane-cs/_static/image1.png)

Dane w postawce zostały dynamicznie dodane przez kod po stronie serwera ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](dynamically-adding-an-accordion-pane-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](databinding-to-an-accordion-cs.md)
> [dalej](databinding-to-an-accordion-vb.md)

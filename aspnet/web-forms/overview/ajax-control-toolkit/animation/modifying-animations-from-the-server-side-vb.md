---
uid: web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-vb
title: Modyfikowanie animacji po stronie serwera (VB) | Microsoft Docs
author: wenz
description: Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. Animacje mogą również...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: addcf4aa-340a-460b-9c64-506424a1f725
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-vb
msc.type: authoredcontent
ms.openlocfilehash: ebc311d1a931ad611d9556799c94440d41a9cf49
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598001"
---
# <a name="modifying-animations-from-the-server-side-vb"></a>Modyfikowanie animacji po stronie serwera (VB)

Autor [Christian Wenz](https://github.com/wenz)

[Pobierz kod](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.vb.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9VB.pdf)

> Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. Animacje można także zmienić po stronie serwera

## <a name="overview"></a>Omówienie

Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. Animacje można także zmienić po stronie serwera

## <a name="steps"></a>Kroki

Po pierwsze, Uwzględnij `ScriptManager` na stronie; następnie załadowana zostanie Biblioteka ASP.NET AJAX, co umożliwia korzystanie z zestawu narzędzi kontroli:

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample1.aspx)]

Animacja zostanie zastosowana do panelu tekstu, który wygląda następująco:

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample2.aspx)]

W skojarzonej klasie CSS panelu, zdefiniuj kolor tła całkiem, a także Ustaw stałą szerokość panelu:

[!code-css[Main](modifying-animations-from-the-server-side-vb/samples/sample3.css)]

Pozostała część kodu jest uruchamiana po stronie serwera i nie używa znaczników; Zamiast tego używa kodu do utworzenia formantu `AnimationExtender`:

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample4.aspx)]

Jednak zestaw narzędzi kontroli nie zapewnia obecnie dostępu do interfejsu API w celu utworzenia pojedynczych animacji. Można jednak ustawić właściwość animacje `AnimationExtender`na ciąg zawierający znacznik XML, używany podczas bardziej przypisywania animacji. W celu utworzenia pliku XML, który nie może zawierać elementu `<Animations>`, można użyć obsługi XML .NET Framework lub, jak w poniższym kodzie, wystarczy podać ciąg:

[!code-vb[Main](modifying-animations-from-the-server-side-vb/samples/sample5.vb)]

Na koniec Dodaj formant `AnimationExtender` do bieżącej strony w obrębie elementu `<form runat="server">`, aby upewnić się, że animacja jest dołączona i uruchomiona:

[!code-vb[Main](modifying-animations-from-the-server-side-vb/samples/sample6.vb)]

[![animacja jest tworzona przy użyciu kodu/VB po C#stronie serwera](modifying-animations-from-the-server-side-vb/_static/image2.png)](modifying-animations-from-the-server-side-vb/_static/image1.png)

Animacja jest tworzona przy użyciu kodu/VB po C#stronie serwera ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](modifying-animations-from-the-server-side-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](triggering-an-animation-in-another-control-vb.md)
> [dalej](executing-animations-using-client-side-code-vb.md)

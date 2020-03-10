---
uid: web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-vb
title: Zmiana animacji przy użyciu kodu po stronie klienta (VB) | Microsoft Docs
author: wenz
description: Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. Animacja może również...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: a7fe5de5-a964-4780-ae5e-70821dfb50a0
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-vb
msc.type: authoredcontent
ms.openlocfilehash: cce0a5a901f71edd40eada59ac7eeba93222e2b3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536310"
---
# <a name="changing-an-animation-using-client-side-code-vb"></a>Zmienianie animacji przy użyciu kodu po stronie klienta (VB)

Autor [Christian Wenz](https://github.com/wenz)

[Pobierz kod](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.vb.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11VB.pdf)

> Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. Animację można także zmienić przy użyciu niestandardowego kodu JavaScript po stronie klienta.

## <a name="overview"></a>Omówienie

Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. Animację można także zmienić przy użyciu niestandardowego kodu JavaScript po stronie klienta.

## <a name="steps"></a>Kroki

Po pierwsze, Uwzględnij `ScriptManager` na stronie; następnie załadowana zostanie Biblioteka ASP.NET AJAX, co umożliwia korzystanie z zestawu narzędzi kontroli:

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample1.aspx)]

Animacja zostanie zastosowana do panelu tekstu, który wygląda następująco:

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample2.aspx)]

W skojarzonej klasie CSS panelu, zdefiniuj kolor tła całkiem, a także Ustaw stałą szerokość panelu:

[!code-css[Main](changing-an-animation-using-client-side-code-vb/samples/sample3.css)]

Rzeczywista animacja jest uruchamiana przez przycisk HTML:

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample4.aspx)]

Następnie Dodaj `AnimationExtender` do strony, podając `ID`, atrybut `TargetControlID` i obowiązkowe `runat="server"`:

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample5.aspx)]

Należy zauważyć, że w kontrolce `AnimationExtender` nie ma węzła `<Animations>`. Niestandardowy kod JavaScript służy do zapewnienia animacji, które mają być używane z kontrolką.

Podobnie jak w przypadku interfejsu API serwera `AnimationExtender`, nie istnieje łatwy sposób przypisywania jeszcze animacji do urządzenia Extender. Jednak rozszerzenie udostępnia kilka metod odczytywania i zapisywania animacji zarejestrowanych w różnych zdarzeniach (`OnClick`, `OnLoad`itd.). Oto kilka przykładów:

- `get_OnClick()`
- `set_OnClick()`
- `get_OnLoad()`
- `set_OnLoad()`
- `...`

Format wartości zwracanej przez funkcje `get_*()` i Format argumentu dla funkcji `set_*()` jest ciągiem JSON, zapewniającym reprezentację obiektu w postaci tablicy XML. Obecnie nie ma możliwości przekazania obiektu w, ale istnieje możliwość odczytania obiektu z danej animacji (`get_OnXXXBehavior()` metod).

Poniżej znajduje się ciąg JSON (bez cudzysłowów ograniczających i sformatowanych dobrze) reprezentujący animację wyzwalaną przez przycisk, ale animowanie panelu przez zmianę jego rozmiaru i zanikanie go w tym samym czasie:

[!code-json[Main](changing-an-animation-using-client-side-code-vb/samples/sample6.json)]

Poniższy kod JavaScript przypisuje ten skrypt JSON do `OnClick` animacji bieżącego rozszerzenia i uruchamia go:

[!code-html[Main](changing-an-animation-using-client-side-code-vb/samples/sample7.html)]

[![animacje są uruchamiane natychmiast, bez kliknięcia przycisku myszy (i z bardzo małą adiustacją)](changing-an-animation-using-client-side-code-vb/_static/image2.png)](changing-an-animation-using-client-side-code-vb/_static/image1.png)

Animacja zostanie uruchomiona natychmiast, bez kliknięcia przycisku myszy (i z bardzo małą adiustacją) ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](changing-an-animation-using-client-side-code-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](executing-animations-using-client-side-code-vb.md)
> [dalej](animating-an-updatepanel-control-vb.md)

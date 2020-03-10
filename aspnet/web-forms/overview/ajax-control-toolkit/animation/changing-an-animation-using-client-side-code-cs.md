---
uid: web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-cs
title: Zmiana animacji przy użyciu kodu po stronie klienta (C#) | Microsoft Docs
author: wenz
description: Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. Animacja może również...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 2bfbc5cc-f942-44b7-a62d-a29520f1da9a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 84fc2d6646b89cfabb2193cdfca59462d6d7ef16
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598218"
---
# <a name="changing-an-animation-using-client-side-code-c"></a>Zmienianie animacji przy użyciu kodu po stronie klienta (C#)

Autor [Christian Wenz](https://github.com/wenz)

[Pobierz kod](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.cs.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11CS.pdf)

> Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. Animację można także zmienić przy użyciu niestandardowego kodu JavaScript po stronie klienta.

## <a name="overview"></a>Omówienie

Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. Animację można także zmienić przy użyciu niestandardowego kodu JavaScript po stronie klienta.

## <a name="steps"></a>Kroki

Po pierwsze, Uwzględnij `ScriptManager` na stronie; następnie załadowana zostanie Biblioteka ASP.NET AJAX, co umożliwia korzystanie z zestawu narzędzi kontroli:

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample1.aspx)]

Animacja zostanie zastosowana do panelu tekstu, który wygląda następująco:

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample2.aspx)]

W skojarzonej klasie CSS panelu, zdefiniuj kolor tła całkiem, a także Ustaw stałą szerokość panelu:

[!code-css[Main](changing-an-animation-using-client-side-code-cs/samples/sample3.css)]

Rzeczywista animacja jest uruchamiana przez przycisk HTML:

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample4.aspx)]

Następnie Dodaj `AnimationExtender` do strony, podając `ID`, atrybut `TargetControlID` i obowiązkowe `runat="server"`:

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample5.aspx)]

Należy zauważyć, że w kontrolce `AnimationExtender` nie ma węzła `<Animations>`. Niestandardowy kod JavaScript służy do zapewnienia animacji, które mają być używane z kontrolką.

Podobnie jak w przypadku interfejsu API serwera `AnimationExtender`, nie istnieje łatwy sposób przypisywania jeszcze animacji do urządzenia Extender. Jednak rozszerzenie udostępnia kilka metod odczytywania i zapisywania animacji zarejestrowanych w różnych zdarzeniach (`OnClick`, `OnLoad`itd.). Oto kilka przykładów:

- `get_OnClick()`
- `set_OnClick()`
- `get_OnLoad()`
- `set_OnLoad()`
- `...`

Format wartości zwracanej przez funkcje `get_*()` i Format argumentu dla funkcji `set_*()` jest ciągiem JSON, zapewniającym reprezentację obiektu w postaci tablicy XML. Obecnie nie ma możliwości przekazania obiektu w, ale istnieje możliwość odczytania obiektu z danej animacji (`get_OnXXXBehavior()` metod).

Poniżej znajduje się ciąg JSON (bez cudzysłowów ograniczających i sformatowanych dobrze) reprezentujący animację wyzwalaną przez przycisk, ale animowanie panelu przez zmianę jego rozmiaru i zanikanie go w tym samym czasie:

[!code-json[Main](changing-an-animation-using-client-side-code-cs/samples/sample6.json)]

Poniższy kod JavaScript przypisuje ten skrypt JSON do `OnClick` animacji bieżącego rozszerzenia i uruchamia go:

[!code-html[Main](changing-an-animation-using-client-side-code-cs/samples/sample7.html)]

[![animacje są uruchamiane natychmiast, bez kliknięcia przycisku myszy (i z bardzo małą adiustacją)](changing-an-animation-using-client-side-code-cs/_static/image2.png)](changing-an-animation-using-client-side-code-cs/_static/image1.png)

Animacja zostanie uruchomiona natychmiast, bez kliknięcia przycisku myszy (i z bardzo małą adiustacją) ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](changing-an-animation-using-client-side-code-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](executing-animations-using-client-side-code-cs.md)
> [dalej](animating-an-updatepanel-control-cs.md)

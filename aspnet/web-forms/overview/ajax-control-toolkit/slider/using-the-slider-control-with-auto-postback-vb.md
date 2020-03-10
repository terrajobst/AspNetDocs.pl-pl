---
uid: web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-vb
title: Korzystanie z kontrolki suwaka z funkcją autoogłaszania zwrotnego (VB) | Microsoft Docs
author: wenz
description: Kontrolka suwaka w zestawie narzędzi AJAX Control zawiera graficzny suwak, który można kontrolować za pomocą myszy. Możliwe jest, aby suwak był Autopost...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 41d1abba-97a5-4a45-9b44-d05624c19777
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-vb
msc.type: authoredcontent
ms.openlocfilehash: e7a3286bcf7ca844f5dcfa4848c15e0bd4767c0f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78553565"
---
# <a name="using-the-slider-control-with-auto-postback-vb"></a>Korzystanie z kontrolki suwaka z funkcją autoogłaszania zwrotnego (VB)

Autor [Christian Wenz](https://github.com/wenz)

[Pobierz kod](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.vb.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1VB.pdf)

> Kontrolka suwaka w zestawie narzędzi AJAX Control zawiera graficzny suwak, który można kontrolować za pomocą myszy. Po zmianie wartości na suwaku jest możliwe przeprowadzenie autoogłaszania.

## <a name="overview"></a>Omówienie

Kontrolka suwaka w zestawie narzędzi AJAX Control zawiera graficzny suwak, który można kontrolować za pomocą myszy. Po zmianie wartości na suwaku jest możliwe przeprowadzenie autoogłaszania.

## <a name="steps"></a>Kroki

Aby suwak automatycznie ogłaszał zwrotnie po zmianie, oba pola tekstowe muszą mieć atrybut `AutoPostBack="true"`: pole tekstowe, które zostanie przesunięte do samego suwaka, oraz pole tekstowe, które zawiera położenie suwaka. Oto wymagane znaczniki dla tego:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample1.aspx)]

Formant `SliderExtender` z zestawu narzędzi ASP.NET AJAX Control przypisuje funkcje suwaka do dwóch pól tekstowych:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample2.aspx)]

Dodatkowy element etykiety będzie później używany do informowania użytkownika o ogłaszaniu zwrotnym:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample3.aspx)]

Na koniec kontrola `ScriptManager` ASP.NET AJAX ładuje kod JavaScript wymagany do działania zestawu narzędzi sterowania:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample4.aspx)]

Teraz suwak jest ogłaszany ponownie; po stronie serwera to zdarzenie może być przechwytywane i podejmowane działania:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample5.aspx)]

[![Przesuwanie suwaka wyzwala ogłaszanie zwrotne](using-the-slider-control-with-auto-postback-vb/_static/image2.png)](using-the-slider-control-with-auto-postback-vb/_static/image1.png)

Przesuwanie suwaka wyzwala ogłaszanie zwrotne ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-the-slider-control-with-auto-postback-vb/_static/image3.png))

[![później, Data tej zmiany jest zapisywana w etykiecie](using-the-slider-control-with-auto-postback-vb/_static/image5.png)](using-the-slider-control-with-auto-postback-vb/_static/image4.png)

Następnie Data tej zmiany jest zapisywana w etykiecie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-the-slider-control-with-auto-postback-vb/_static/image6.png))

> [!div class="step-by-step"]
> [Poprzednie](databinding-the-slider-control-cs.md)
> [dalej](databinding-the-slider-control-vb.md)

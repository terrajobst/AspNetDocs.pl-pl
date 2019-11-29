---
uid: web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-cs
title: Przy użyciu kontrolki suwaka z funkcją autoogłaszania zwrotnego (C#) | Microsoft Docs
author: wenz
description: Kontrolka suwaka w zestawie narzędzi AJAX Control zawiera graficzny suwak, który można kontrolować za pomocą myszy. Możliwe jest, aby suwak był Autopost...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 4d85e9fb-91e6-41f2-9c13-754549b19c27
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-cs
msc.type: authoredcontent
ms.openlocfilehash: 785d62108667fddac42994344cde265e82aca8f4
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74598391"
---
# <a name="using-the-slider-control-with-auto-postback-c"></a>Używanie kontrolki suwaka z funkcją autoogłaszania zwrotnego (C#)

Autor [Christian Wenz](https://github.com/wenz)

[Pobierz kod](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.cs.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1CS.pdf)

> Kontrolka suwaka w zestawie narzędzi AJAX Control zawiera graficzny suwak, który można kontrolować za pomocą myszy. Po zmianie wartości na suwaku jest możliwe przeprowadzenie autoogłaszania.

## <a name="overview"></a>Omówienie

Kontrolka suwaka w zestawie narzędzi AJAX Control zawiera graficzny suwak, który można kontrolować za pomocą myszy. Po zmianie wartości na suwaku jest możliwe przeprowadzenie autoogłaszania.

## <a name="steps"></a>Kroki

Aby suwak automatycznie ogłaszał zwrotnie po zmianie, oba pola tekstowe muszą mieć atrybut `AutoPostBack="true"`: pole tekstowe, które zostanie przesunięte do samego suwaka, oraz pole tekstowe, które zawiera położenie suwaka. Oto wymagane znaczniki dla tego:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample1.aspx)]

Formant `SliderExtender` z zestawu narzędzi ASP.NET AJAX Control przypisuje funkcje suwaka do dwóch pól tekstowych:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample2.aspx)]

Dodatkowy element etykiety będzie później używany do informowania użytkownika o ogłaszaniu zwrotnym:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample3.aspx)]

Na koniec kontrola `ScriptManager` ASP.NET AJAX ładuje kod JavaScript wymagany do działania zestawu narzędzi sterowania:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample4.aspx)]

Teraz suwak jest ogłaszany ponownie; po stronie serwera to zdarzenie może być przechwytywane i podejmowane działania:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample5.aspx)]

[![Przesuwanie suwaka wyzwala ogłaszanie zwrotne](using-the-slider-control-with-auto-postback-cs/_static/image2.png)](using-the-slider-control-with-auto-postback-cs/_static/image1.png)

Przesuwanie suwaka wyzwala ogłaszanie zwrotne ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-the-slider-control-with-auto-postback-cs/_static/image3.png))

[![później, Data tej zmiany jest zapisywana w etykiecie](using-the-slider-control-with-auto-postback-cs/_static/image5.png)](using-the-slider-control-with-auto-postback-cs/_static/image4.png)

Następnie Data tej zmiany jest zapisywana w etykiecie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-the-slider-control-with-auto-postback-cs/_static/image6.png))

> [!div class="step-by-step"]
> [Next](databinding-the-slider-control-cs.md)

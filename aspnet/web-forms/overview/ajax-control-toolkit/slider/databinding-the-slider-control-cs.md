---
uid: web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-cs
title: Powiązanie kontrolki suwakaC#() | Microsoft Docs
author: wenz
description: Kontrolka suwaka w zestawie narzędzi AJAX Control zawiera graficzny suwak, który można kontrolować za pomocą myszy. Istnieje możliwość powiązania bieżącego positio...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: b7f77869-aa1d-4025-924f-622c57112db6
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-cs
msc.type: authoredcontent
ms.openlocfilehash: ef547573f17f3265ad72717d3d3bbc622fd6894e
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74598577"
---
# <a name="databinding-the-slider-control-c"></a>Powiązanie danych kontrolki Slider (C#)

Autor [Christian Wenz](https://github.com/wenz)

[Pobierz kod](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.cs.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0CS.pdf)

> Kontrolka suwaka w zestawie narzędzi AJAX Control zawiera graficzny suwak, który można kontrolować za pomocą myszy. Można powiązać bieżącą pozycję suwaka z inną kontrolką ASP.NET.

## <a name="overview"></a>Omówienie

Kontrolka suwaka w zestawie narzędzi AJAX Control zawiera graficzny suwak, który można kontrolować za pomocą myszy. Można powiązać bieżącą pozycję suwaka z inną kontrolką ASP.NET.

## <a name="steps"></a>Kroki

Aby aktywować funkcje ASP.NET AJAX i zestawu narzędzi sterowania, formant `ScriptManager` musi być umieszczony w dowolnym miejscu na stronie (ale w obrębie elementu `<form>`):

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample1.aspx)]

Następnie Dodaj dwie `TextBox` kontrolki do strony. Jeden z nich zostanie przekształcony w suwak graficzny, a drugi będzie pomieścić położenie suwaka.

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample2.aspx)]

Następnym krokiem jest już ostatni krok. Kontrolka `SliderExtender` z zestawu narzędzi ASP.NET AJAX Control tworzy suwak z pierwszego pola tekstowego i automatycznie aktualizuje drugie pole tekstowe po zmianie położenia suwaka. Aby to działało, atrybut `TargetControlID` `SliderExtender`musi być ustawiony na identyfikator pierwszego pola tekstowego; atrybut `BoundControlID` musi być ustawiony na identyfikator drugiego pola tekstowego.

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample3.aspx)]

Jak widać w przeglądarce, powiązanie danych działa w obu kierunkach: wprowadzenie nowej wartości w polu tekstowym aktualizuje pozycję suwaka. Jeśli nastąpi odczytanie drugiego pola tekstowego, możesz dodać słabą ochronę do pola tekstowego, aby utrudnić Użytkownikowi ręczne zaktualizowanie wartości w tym miejscu.

[Suwak ![i pole tekstowe są zsynchronizowane](databinding-the-slider-control-cs/_static/image2.png)](databinding-the-slider-control-cs/_static/image1.png)

Suwak i pole tekstowe są zsynchronizowane ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](databinding-the-slider-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](using-the-slider-control-with-auto-postback-cs.md)
> [dalej](using-the-slider-control-with-auto-postback-vb.md)

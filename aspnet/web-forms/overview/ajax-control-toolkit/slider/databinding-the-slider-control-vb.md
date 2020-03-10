---
uid: web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-vb
title: Powiązanie kontrolki suwaka (VB) | Microsoft Docs
author: wenz
description: Kontrolka suwaka w zestawie narzędzi AJAX Control zawiera graficzny suwak, który można kontrolować za pomocą myszy. Istnieje możliwość powiązania bieżącego positio...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 4f3ba53f-d166-422d-b29c-403348057836
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 9c14373bdfdead9916950b8a1cf61f427f7ba50b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78627219"
---
# <a name="databinding-the-slider-control-vb"></a>Powiązanie danych kontrolki Slider (VB)

Autor [Christian Wenz](https://github.com/wenz)

[Pobierz kod](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.vb.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0VB.pdf)

> Kontrolka suwaka w zestawie narzędzi AJAX Control zawiera graficzny suwak, który można kontrolować za pomocą myszy. Można powiązać bieżącą pozycję suwaka z inną kontrolką ASP.NET.

## <a name="overview"></a>Omówienie

Kontrolka suwaka w zestawie narzędzi AJAX Control zawiera graficzny suwak, który można kontrolować za pomocą myszy. Można powiązać bieżącą pozycję suwaka z inną kontrolką ASP.NET.

## <a name="steps"></a>Kroki

Aby aktywować funkcje ASP.NET AJAX i zestawu narzędzi sterowania, formant `ScriptManager` musi być umieszczony w dowolnym miejscu na stronie (ale w obrębie elementu `<form>`):

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample1.aspx)]

Następnie Dodaj dwie `TextBox` kontrolki do strony. Jeden z nich zostanie przekształcony w suwak graficzny, a drugi będzie pomieścić położenie suwaka.

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample2.aspx)]

Następnym krokiem jest już ostatni krok. Kontrolka `SliderExtender` z zestawu narzędzi ASP.NET AJAX Control tworzy suwak z pierwszego pola tekstowego i automatycznie aktualizuje drugie pole tekstowe po zmianie położenia suwaka. Aby to działało, atrybut `TargetControlID` `SliderExtender`musi być ustawiony na identyfikator pierwszego pola tekstowego; atrybut `BoundControlID` musi być ustawiony na identyfikator drugiego pola tekstowego.

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample3.aspx)]

Jak widać w przeglądarce, powiązanie danych działa w obu kierunkach: wprowadzenie nowej wartości w polu tekstowym aktualizuje pozycję suwaka. Jeśli nastąpi odczytanie drugiego pola tekstowego, możesz dodać słabą ochronę do pola tekstowego, aby utrudnić Użytkownikowi ręczne zaktualizowanie wartości w tym miejscu.

[Suwak ![i pole tekstowe są zsynchronizowane](databinding-the-slider-control-vb/_static/image2.png)](databinding-the-slider-control-vb/_static/image1.png)

Suwak i pole tekstowe są zsynchronizowane ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](databinding-the-slider-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Wstecz](using-the-slider-control-with-auto-postback-vb.md)

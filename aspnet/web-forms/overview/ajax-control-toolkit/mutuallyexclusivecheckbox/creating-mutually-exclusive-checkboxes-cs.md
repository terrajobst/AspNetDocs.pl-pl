---
uid: web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-cs
title: Tworzenie wzajemnie wykluczających sięC#pól wyboru () | Microsoft Docs
author: wenz
description: 'Gdy można wybrać tylko jeden z zestawów opcji, są zwykle używane przyciski radiowe. Istnieje zwrot, chociaż: wybrano jeden przycisk radiowy w grupie,...'
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 8e11b813-ba0d-4c29-b0f8-f65db6dbef1e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-cs
msc.type: authoredcontent
ms.openlocfilehash: ddc154601752cc856f00dd4f3207952ab7e0e3e0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78613072"
---
# <a name="creating-mutually-exclusive-checkboxes-c"></a>Tworzenie wzajemnie wykluczających się pól wyboru (C#)

Autor [Christian Wenz](https://github.com/wenz)

[Pobierz kod](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.cs.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0CS.pdf)

> Gdy można wybrać tylko jeden z zestawów opcji, są zwykle używane przyciski radiowe. Istnieje zwrot, chociaż: po wybraniu jednego przycisku radiowego w grupie nie można usunąć zaznaczenia wszystkich przycisków radiowych. Pola wyboru można zaznaczać w dowolnym momencie, ale nie wykluczają się wzajemnie. Ten samouczek zawiera najlepsze podejścia: pola wyboru, które wzajemnie się wykluczają.

## <a name="overview"></a>Omówienie

Gdy można wybrać tylko jeden z zestawów opcji, są zwykle używane przyciski radiowe. Istnieje zwrot, chociaż: po wybraniu jednego przycisku radiowego w grupie nie można usunąć zaznaczenia wszystkich przycisków radiowych. Pola wyboru można zaznaczać w dowolnym momencie, ale nie wykluczają się wzajemnie. Ten samouczek zawiera najlepsze podejścia: pola wyboru, które wzajemnie się wykluczają.

## <a name="steps"></a>Kroki

Zestaw narzędzi ASP.NET AJAX Control Toolkit zawiera rozszerzenie MutuallyExclusiveCheckBox. Dzięki temu programiści mogą przypisywać dowolne pola wyboru do nazwy grupy (`Key` atrybutu). Ze wszystkich pól wyboru w tej samej grupie można wybrać tylko jedną z nich.

Zacznijmy od umieszczenia dwóch pól wyboru na nowej stronie ASP.NET. Może być więcej, ale dwa z nich wystarczą do zademonstrowania zasady:

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample1.aspx)]

Dla obu pól wyboru formant MutuallyExclusiveCheckBoxExtender musi być umieszczony na stronie. Oba atrybuty klucza muszą mieć tę samą wartość, podobnie jak atrybuty wartości elementów przycisku radiowego HTML muszą być identyczne, aby można było zauważyć, do której grupy należą. Właściwość TargetControlID elementu Extender wskazuje identyfikator pola wyboru.

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample2.aspx)]

Na koniec Uwzględnij ASP.NET AJAX `ScriptManager`, który jest wymagany przez wszystkie elementy zestawu narzędzi ASP.NET AJAX Control Toolkit:

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample3.aspx)]

Zapisz i Uruchom stronę: można zaznaczyć i usunąć zaznaczenie obu pól wyboru, jednak w żadnym momencie nie można sprawdzić obu pól wyboru.

[![można sprawdzić tylko jedno pole wyboru w czasie](creating-mutually-exclusive-checkboxes-cs/_static/image2.png)](creating-mutually-exclusive-checkboxes-cs/_static/image1.png)

Tylko jedno pole wyboru może być zaznaczone naraz ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-mutually-exclusive-checkboxes-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Dalej](creating-mutually-exclusive-checkboxes-vb.md)

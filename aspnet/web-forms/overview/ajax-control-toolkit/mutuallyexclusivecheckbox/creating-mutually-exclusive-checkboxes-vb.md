---
uid: web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-vb
title: Tworzenie wzajemnie wykluczających się pól wyboru (VB) | Microsoft Docs
author: wenz
description: 'Gdy można wybrać tylko jeden z zestawów opcji, są zwykle używane przyciski radiowe. Istnieje zwrot, chociaż: wybrano jeden przycisk radiowy w grupie,...'
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e9dd1d5a-a1db-4114-981d-6a91acb1d709
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-vb
msc.type: authoredcontent
ms.openlocfilehash: f33936dd4d71f6bbf08f02966eefe44c8c152eba
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74606455"
---
# <a name="creating-mutually-exclusive-checkboxes-vb"></a>Tworzenie wzajemnie wykluczających się pól wyboru (VB)

Autor [Christian Wenz](https://github.com/wenz)

[Pobierz kod](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.vb.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0VB.pdf)

> Gdy można wybrać tylko jeden z zestawów opcji, są zwykle używane przyciski radiowe. Istnieje zwrot, chociaż: po wybraniu jednego przycisku radiowego w grupie nie można usunąć zaznaczenia wszystkich przycisków radiowych. Pola wyboru można zaznaczać w dowolnym momencie, ale nie wykluczają się wzajemnie. Ten samouczek zawiera najlepsze podejścia: pola wyboru, które wzajemnie się wykluczają.

## <a name="overview"></a>Omówienie

Gdy można wybrać tylko jeden z zestawów opcji, są zwykle używane przyciski radiowe. Istnieje zwrot, chociaż: po wybraniu jednego przycisku radiowego w grupie nie można usunąć zaznaczenia wszystkich przycisków radiowych. Pola wyboru można zaznaczać w dowolnym momencie, ale nie wykluczają się wzajemnie. Ten samouczek zawiera najlepsze podejścia: pola wyboru, które wzajemnie się wykluczają.

## <a name="steps"></a>Kroki

Zestaw narzędzi ASP.NET AJAX Control Toolkit zawiera rozszerzenie MutuallyExclusiveCheckBox. Dzięki temu programiści mogą przypisywać dowolne pola wyboru do nazwy grupy (`Key` atrybutu). Ze wszystkich pól wyboru w tej samej grupie można wybrać tylko jedną z nich.

Zacznijmy od umieszczenia dwóch pól wyboru na nowej stronie ASP.NET. Może być więcej, ale dwa z nich wystarczą do zademonstrowania zasady:

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample1.aspx)]

Dla obu pól wyboru formant MutuallyExclusiveCheckBoxExtender musi być umieszczony na stronie. Oba atrybuty klucza muszą mieć tę samą wartość, podobnie jak atrybuty wartości elementów przycisku radiowego HTML muszą być identyczne, aby można było zauważyć, do której grupy należą. Właściwość TargetControlID elementu Extender wskazuje identyfikator pola wyboru.

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample2.aspx)]

Na koniec Uwzględnij ASP.NET AJAX `ScriptManager`, który jest wymagany przez wszystkie elementy zestawu narzędzi ASP.NET AJAX Control Toolkit:

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample3.aspx)]

Zapisz i Uruchom stronę: można zaznaczyć i usunąć zaznaczenie obu pól wyboru, jednak w żadnym momencie nie można sprawdzić obu pól wyboru.

[![można sprawdzić tylko jedno pole wyboru w czasie](creating-mutually-exclusive-checkboxes-vb/_static/image2.png)](creating-mutually-exclusive-checkboxes-vb/_static/image1.png)

Tylko jedno pole wyboru może być zaznaczone naraz ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-mutually-exclusive-checkboxes-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Ubiegł](creating-mutually-exclusive-checkboxes-cs.md)

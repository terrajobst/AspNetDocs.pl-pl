---
uid: web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-cs
title: Używanie wielu kontrolek popupC#() | Microsoft Docs
author: wenz
description: Rozszerzenie PopupControl w zestawie narzędzi AJAX Control oferuje łatwy sposób wyzwalania okna podręcznego w przypadku aktywowania innej kontrolki. Istnieje również możliwość użycia m...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 91511b0b-311d-481f-9e7c-73f07b813b79
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: 8700fe89af591e8b481e853580b0efa0cddbf1bc
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74611652"
---
# <a name="using-multiple-popup-controls-c"></a>Używanie wielu kontrolek Popup (C#)

Autor [Christian Wenz](https://github.com/wenz)

[Pobierz kod](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.cs.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1CS.pdf)

> Rozszerzenie PopupControl w zestawie narzędzi AJAX Control oferuje łatwy sposób wyzwalania okna podręcznego w przypadku aktywowania innej kontrolki. Istnieje również możliwość użycia więcej niż jednej kontrolki popup na jednej stronie.

## <a name="overview"></a>Omówienie

Rozszerzenie PopupControl w zestawie narzędzi AJAX Control oferuje łatwy sposób wyzwalania okna podręcznego w przypadku aktywowania innej kontrolki. Istnieje również możliwość użycia więcej niż jednej kontrolki popup na jednej stronie.

## <a name="steps"></a>Kroki

Aby aktywować funkcje ASP.NET AJAX i zestawu narzędzi sterowania, formant `ScriptManager` musi być umieszczony w dowolnym miejscu na stronie (ale w obrębie elementu `<form>`):

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample1.aspx)]

Następnie Dodaj panel, który służy jako okno podręczne. W bieżącym scenariuszu panel zawiera kontrolkę `Calendar`. Aby uniknąć odświeżenia stron spowodowanych przez ogłaszanie zwrotne w kalendarzu, panel jest umieszczany w kontrolce `UpdatePanel`:

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample2.aspx)]

Strona zawiera również dwa pola tekstowe. Dla każdego pola tekstowego okno podręczne zostanie wyświetlone po aktywowaniu pola tekstowego.

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample3.aspx)]

Teraz możesz rozłożyć wszystkie dwa pola tekstowe z `PopupControlExtender`. Atrybut `TargetControlID` zawiera identyfikator formantu powiązanego z przedłużaczem. Atrybut `PopupControlID` zawiera identyfikator panelu podręcznego. W takim przypadku obydwa elementy wykraczają na ten sam panel, ale również są dostępne różne panele.

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample4.aspx)]

Teraz za każdym razem, gdy klikniesz wewnątrz pola tekstowego, Kalendarz zostanie wyświetlony poniżej pola, co umożliwi wybranie daty. (Pobranie wybranej daty z powrotem do pól tekstowych zostanie omówione w innym samouczku).

[![kalendarz pojawia się, gdy użytkownik kliknie w polu tekstowym](using-multiple-popup-controls-cs/_static/image2.png)](using-multiple-popup-controls-cs/_static/image1.png)

Kalendarz pojawia się, gdy użytkownik kliknie w polu tekstowym ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](using-multiple-popup-controls-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Next](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)

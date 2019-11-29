---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-vb
title: Obsługa ogłaszania zwrotnego w kontrolce popup przy użyciu elementu UpdatePanel (VB) | Microsoft Docs
author: wenz
description: Rozszerzenie PopupControl w zestawie narzędzi AJAX Control oferuje łatwy sposób wyzwalania okna podręcznego w przypadku aktywowania innej kontrolki. Należy zwrócić szczególną uwagę...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: ec9db57c-9f68-402a-bf4c-0d63d5f6908e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-vb
msc.type: authoredcontent
ms.openlocfilehash: dd045ae56696c7944df98cf805ba812fde1bb4ff
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74598826"
---
# <a name="handling-postbacks-from-a-popup-control-with-an-updatepanel-vb"></a>Obsługa ogłaszania zwrotnego w kontrolce Popup z kontrolką UpdatePanel (VB)

Autor [Christian Wenz](https://github.com/wenz)

[Pobierz kod](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.vb.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2VB.pdf)

> Rozszerzenie PopupControl w zestawie narzędzi AJAX Control oferuje łatwy sposób wyzwalania okna podręcznego w przypadku aktywowania innej kontrolki. Należy zwrócić szczególną uwagę, gdy ogłaszanie zwrotne odbywa się w tym menu podręcznym.

## <a name="overview"></a>Omówienie

Rozszerzenie PopupControl w zestawie narzędzi AJAX Control oferuje łatwy sposób wyzwalania okna podręcznego w przypadku aktywowania innej kontrolki. Należy zwrócić szczególną uwagę, gdy ogłaszanie zwrotne odbywa się w tym menu podręcznym.

## <a name="steps"></a>Kroki

W przypadku korzystania z `PopupControl` z ogłaszaniem zwrotnym `UpdatePanel` może uniemożliwić odświeżenie strony spowodowane przez ogłaszanie zwrotne. Następujące znaczniki definiują kilka ważnych elementów:

- Kontrolka `ScriptManager`, tak aby zestaw narzędzi ASP.NET AJAX Control działał
- Dwie `TextBox` kontrolki, które będą wyzwalać menu podręczne
- Formant `Panel`, który będzie używany jako podręczny
- W panelu formant `Calendar` jest osadzony w kontrolce `UpdatePanel`
- Dwie `PopupControlExtender` kontrolki, które przypisują panel do pól tekstowych

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/samples/sample1.aspx)]

Należy zauważyć, że atrybut `OnSelectionChanged` kontrolki `Calendar` jest ustawiony. Tak więc, gdy użytkownik wybierze datę w kalendarzu, następuje ogłaszanie zwrotne i zostanie uruchomiona metoda po stronie serwera `c1_SelectionChanged()`. W ramach tej metody bieżąca data musi być pobrana i zapisywana z powrotem do pola tekstowego.

Składnia dla programu jest następująca: najpierw należy wygenerować obiekt serwera proxy dla `PopupControlExtender` na stronie. Zestaw narzędzi ASP.NET AJAX Control Toolkit oferuje metodę `GetProxyForCurrentPopup()`. Obiekt zwracany przez tę metodę obsługuje metodę `Commit()`, która wysyła wartość z powrotem do kontrolki, która wyzwoliła wyskakujące okienko, a nie kontrolka, która wyzwoliła wywołanie metody!). Poniższy kod przedstawia wybraną datę jako argument metody `Commit()`, powodując, że kod zapiszą wybraną datę z powrotem do pola tekstowego:

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/samples/sample2.aspx)]

Teraz za każdym razem, gdy klikniesz datę kalendarzową, wybrana data zostanie wyświetlona w skojarzonym polu tekstowym i zostanie utworzona kontrolka selektora daty, która jest obecnie dostępna w wielu witrynach sieci Web.

[![kalendarz pojawia się, gdy użytkownik kliknie w polu tekstowym](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image1.png)

Kalendarz pojawia się, gdy użytkownik kliknie w polu tekstowym ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image3.png))

[![kliknięcie daty spowoduje jej umieszczenie w polu tekstowym](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image4.png)

Kliknięcie daty spowoduje jej umieszczenie w polu tekstowym ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image6.png))

> [!div class="step-by-step"]
> [Poprzednie](using-multiple-popup-controls-vb.md)
> [dalej](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb.md)

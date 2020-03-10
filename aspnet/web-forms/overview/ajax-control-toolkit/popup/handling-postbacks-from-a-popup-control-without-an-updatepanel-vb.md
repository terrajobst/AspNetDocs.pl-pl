---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-vb
title: Obsługa ogłaszania zwrotnego w kontrolce popup bez elementu UpdatePanel (VB) | Microsoft Docs
author: wenz
description: Rozszerzenie PopupControl w zestawie narzędzi AJAX Control oferuje łatwy sposób wyzwalania okna podręcznego w przypadku aktywowania innej kontrolki. Gdy ogłaszanie zwrotne odbywa się w funkcji su...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: a0b9186c-0912-4fff-916a-6d17e696a50b
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-vb
msc.type: authoredcontent
ms.openlocfilehash: aaecf77c1d25f2c99ef4e9948d79fc01b837169b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78553985"
---
# <a name="handling-postbacks-from-a-popup-control-without-an-updatepanel-vb"></a>Obsługa ogłaszania zwrotnego w kontrolce Popup bez kontrolki UpdatePanel (VB)

Autor [Christian Wenz](https://github.com/wenz)

[Pobierz kod](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.vb.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3VB.pdf)

> Rozszerzenie PopupControl w zestawie narzędzi AJAX Control oferuje łatwy sposób wyzwalania okna podręcznego w przypadku aktywowania innej kontrolki. Gdy ogłaszanie zwrotne odbywa się w takim panelu, a na stronie jest dostępnych kilka paneli, trudno ustalić, który panel został kliknięty.

## <a name="overview"></a>Omówienie

Rozszerzenie PopupControl w zestawie narzędzi AJAX Control oferuje łatwy sposób wyzwalania okna podręcznego w przypadku aktywowania innej kontrolki. Gdy ogłaszanie zwrotne odbywa się w takim panelu, a na stronie jest dostępnych kilka paneli, trudno ustalić, który panel został kliknięty.

## <a name="steps"></a>Kroki

W przypadku korzystania z `PopupControl` z ogłaszaniem zwrotnym, ale bez konieczności `UpdatePanel` na stronie, zestaw narzędzi kontroli nie oferuje metody określania, który element klienta wyzwolił okno podręczne, które z kolei spowodowało odświeżenie. Jednak niewielka lewę oferuje obejście tego scenariusza.

Najpierw jest to podstawowa konfiguracja: dwa pola tekstowe, które wyzwalają to samo menu podręczne, kalendarz. Dwa `PopupControlExtenders` przełączenia pól tekstowych i podręcznych.

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample1.aspx)]

Podstawowym pomysłem jest dodanie ukrytego pola formularza w &lt;`form`&gt; elementu, który zawiera pole tekstowe, które uruchomiło menu podręczne:

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample2.aspx)]

Po załadowaniu strony kod JavaScript dodaje procedurę obsługi zdarzeń do obu pól tekstowych: zawsze, gdy użytkownik kliknie pole tekstowe, jego nazwa jest zapisywana w ukrytym polu formularza:

[!code-html[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample3.html)]

W kodzie po stronie serwera wartość pola ukryte musi być odczytana. Ponieważ ukryte pola formularzy są proste do manipulowania, wymagana jest dozwolonych podejście do zweryfikowania wartości ukrytej. Po zidentyfikowaniu poprawnego pola tekstowego Data w kalendarzu zostanie zapisywana.

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample4.aspx)]

[![kalendarz pojawia się, gdy użytkownik kliknie w polu tekstowym](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image1.png)

Kalendarz pojawia się, gdy użytkownik kliknie w polu tekstowym ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image3.png))

[![kliknięcie daty spowoduje jej umieszczenie w polu tekstowym](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image4.png)

Kliknięcie daty spowoduje jej umieszczenie w polu tekstowym ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image6.png))

> [!div class="step-by-step"]
> [Wstecz](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)

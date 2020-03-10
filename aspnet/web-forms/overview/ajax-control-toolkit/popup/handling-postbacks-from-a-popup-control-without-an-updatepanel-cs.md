---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-cs
title: Obsługa ogłaszania zwrotnego w kontrolce popup bez elementuC#UpdatePanel () | Microsoft Docs
author: wenz
description: Rozszerzenie PopupControl w zestawie narzędzi AJAX Control oferuje łatwy sposób wyzwalania okna podręcznego w przypadku aktywowania innej kontrolki. Gdy ogłaszanie zwrotne odbywa się w funkcji su...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 25444121-5a72-4dac-8e50-ad2b7ac667af
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-cs
msc.type: authoredcontent
ms.openlocfilehash: 9c4c59bb9dbd3e2ba2b3b81ecf76271f21673bce
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78612743"
---
# <a name="handling-postbacks-from-a-popup-control-without-an-updatepanel-c"></a>Obsługa ogłaszania zwrotnego w kontrolce Popup bez kontrolki UpdatePanel (C#)

Autor [Christian Wenz](https://github.com/wenz)

[Pobierz kod](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.cs.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3CS.pdf)

> Rozszerzenie PopupControl w zestawie narzędzi AJAX Control oferuje łatwy sposób wyzwalania okna podręcznego w przypadku aktywowania innej kontrolki. Gdy ogłaszanie zwrotne odbywa się w takim panelu, a na stronie jest dostępnych kilka paneli, trudno ustalić, który panel został kliknięty.

## <a name="overview"></a>Omówienie

Rozszerzenie PopupControl w zestawie narzędzi AJAX Control oferuje łatwy sposób wyzwalania okna podręcznego w przypadku aktywowania innej kontrolki. Gdy ogłaszanie zwrotne odbywa się w takim panelu, a na stronie jest dostępnych kilka paneli, trudno ustalić, który panel został kliknięty.

## <a name="steps"></a>Kroki

W przypadku korzystania z `PopupControl` z ogłaszaniem zwrotnym, ale bez konieczności `UpdatePanel` na stronie, zestaw narzędzi kontroli nie oferuje metody określania, który element klienta wyzwolił okno podręczne, które z kolei spowodowało odświeżenie. Jednak niewielka lewę oferuje obejście tego scenariusza.

Najpierw jest to podstawowa konfiguracja: dwa pola tekstowe, które wyzwalają to samo menu podręczne, kalendarz. Dwa `PopupControlExtenders` przełączenia pól tekstowych i podręcznych.

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample1.aspx)]

Podstawowym pomysłem jest dodanie ukrytego pola formularza w &lt;`form`&gt; elementu, który zawiera pole tekstowe, które uruchomiło menu podręczne:

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample2.aspx)]

Po załadowaniu strony kod JavaScript dodaje procedurę obsługi zdarzeń do obu pól tekstowych: zawsze, gdy użytkownik kliknie pole tekstowe, jego nazwa jest zapisywana w ukrytym polu formularza:

[!code-html[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample3.html)]

W kodzie po stronie serwera wartość pola ukryte musi być odczytana. Ponieważ ukryte pola formularzy są proste do manipulowania, wymagana jest dozwolonych podejście do zweryfikowania wartości ukrytej. Po zidentyfikowaniu poprawnego pola tekstowego Data w kalendarzu zostanie zapisywana.

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample4.aspx)]

[![kalendarz pojawia się, gdy użytkownik kliknie w polu tekstowym](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image1.png)

Kalendarz pojawia się, gdy użytkownik kliknie w polu tekstowym ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image3.png))

[![kliknięcie daty spowoduje jej umieszczenie w polu tekstowym](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image4.png)

Kliknięcie daty spowoduje jej umieszczenie w polu tekstowym ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image6.png))

> [!div class="step-by-step"]
> [Poprzednie](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
> [dalej](using-multiple-popup-controls-vb.md)

---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-vb
title: Obsługa ogłaszania zwrotnego w kontrolce Popup z kontrolką UpdatePanel (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Urządzenia extender PopupControl zestawu narzędzi AJAX Control Toolkit oferuje prosty sposób wyzwalania okna podręcznego, po aktywowaniu wszelkimi innymi kontrolkami. Szczególną uwagę należy zwrócić...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: ec9db57c-9f68-402a-bf4c-0d63d5f6908e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-vb
msc.type: authoredcontent
ms.openlocfilehash: 3c4b58e864ca99aa30444fbb3244bfa4ffb4c336
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/09/2019
ms.locfileid: "59379043"
---
# <a name="handling-postbacks-from-a-popup-control-with-an-updatepanel-vb"></a>Obsługa ogłaszania zwrotnego w kontrolce Popup z kontrolką UpdatePanel (VB)

przez [Christian Wenz](https://github.com/wenz)

[Pobierz program Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.vb.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2VB.pdf)

> Urządzenia extender PopupControl zestawu narzędzi AJAX Control Toolkit oferuje prosty sposób wyzwalania okna podręcznego, po aktywowaniu wszelkimi innymi kontrolkami. Szczególną uwagę należy podjąć w przypadku ogłaszania zwrotnego w obrębie okna podręcznego.


## <a name="overview"></a>Omówienie

Urządzenia extender PopupControl zestawu narzędzi AJAX Control Toolkit oferuje prosty sposób wyzwalania okna podręcznego, po aktywowaniu wszelkimi innymi kontrolkami. Szczególną uwagę należy podjąć w przypadku ogłaszania zwrotnego w obrębie okna podręcznego.

## <a name="steps"></a>Kroki

Korzystając z `PopupControl` z zwrotu `UpdatePanel` może uniemożliwić odświeżania strony spowodowane zwrotu. Następujące znaczniki definiuje kilka ważnych elementów:

- A `ScriptManager` kontrolki ASP.NET AJAX Control Toolkit działa
- Dwa `TextBox` formanty, które wyzwolą oba okna podręcznego
- A `Panel` formant, który będzie służyć jako menu podręcznego.
- W panelu `Calendar` kontroli jest osadzony w ramach `UpdatePanel` kontroli
- Dwa `PopupControlExtender` mechanizmy przypisać panelu do pól tekstowych

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/samples/sample1.aspx)]

Należy pamiętać, że `OnSelectionChanged` atrybutu `Calendar` kontrolki jest ustawiona. Tak, gdy użytkownik wybierze datę w kalendarzu, występuje odświeżenie strony, a także metoda po stronie serwera `c1_SelectionChanged()` jest wykonywany. W ramach tej metody należy pobrać i zapisywane z powrotem do pola tekstowego bieżącą datę.

Składnia, jest następujący: Po pierwsze, serwer proxy dla obiektu `PopupControlExtender` na stronie musi zostać wygenerowany. ASP.NET AJAX Control Toolkit oferuje `GetProxyForCurrentPopup()` metody. Obiekt, Metoda ta zwraca obsługuje `Commit()` metodę, która wysyła wartość do formantu, który wyzwolił menu podręczne (nie formant który wyzwolił wywołanie metody!). Poniższy kod zawiera wybranej daty jako argument dla `Commit()` metody, powodując kod w celu zapisania wybranej daty z powrotem do pola tekstowego:

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/samples/sample2.aspx)]

Teraz po każdym kliknięciu datę w kalendarzu wybrana data pojawia się w polu tekstowym skojarzone tworzenie kontrolki selektora daty, które aktualnie znajdują się na wiele witryn sieci Web.


[![Tjego kalendarz jest wyświetlany, gdy użytkownik klika polecenie w polu tekstowym](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image1.png)

Kalendarz jest wyświetlany, gdy użytkownik kliknie w polu tekstowym ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image3.png))


[![Clicking w dniu umieszcza go w polu tekstowym](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image4.png)

Klikając pozycję na wartość typu date umieszcza go w polu tekstowym ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image6.png))

> [!div class="step-by-step"]
> [Poprzednie](using-multiple-popup-controls-vb.md)
> [dalej](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb.md)

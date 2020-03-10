---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-cs
title: Używanie używanie textboxwatermark z kontrolkami walidacji (C#) | Microsoft Docs
author: wenz
description: Kontrolka używanie textboxwatermark w zestawie narzędzi AJAX Control rozszerza pole tekstowe tak, aby tekst był wyświetlany w polu. Gdy użytkownik kliknie w polu i...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: d49940cb-d38c-456a-b800-5f0eb705d09f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: bc9498b1c5ba2f38b90706c9200ffa813a945fa9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78577841"
---
# <a name="using-textboxwatermark-with-validation-controls-c"></a>Używanie kontrolki TextBoxWatermark z kontrolkami walidacji (C#)

Autor [Christian Wenz](https://github.com/wenz)

[Pobierz kod](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.cs.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2CS.pdf)

> Kontrolka używanie textboxwatermark w zestawie narzędzi AJAX Control rozszerza pole tekstowe tak, aby tekst był wyświetlany w polu. Gdy użytkownik kliknie pole, zostanie opróżnione. Jeśli użytkownik opuści pole bez wprowadzania tekstu, zostanie wyświetlony tekst wstępnie wypełniony. Może to spowodować konflikt z kontrolkami sprawdzania poprawności ASP.NET na tej samej stronie, ale te problemy mogą zostać przezwyciężyne.

## <a name="overview"></a>Omówienie

Kontrolka `TextBoxWatermark` w zestawie narzędzi AJAX Control rozszerza pole tekstowe tak, aby tekst był wyświetlany w polu. Gdy użytkownik kliknie pole, zostanie opróżnione. Jeśli użytkownik opuści pole bez wprowadzania tekstu, zostanie wyświetlony tekst wstępnie wypełniony. Może to spowodować konflikt z kontrolkami sprawdzania poprawności ASP.NET na tej samej stronie, ale te problemy mogą zostać przezwyciężyne.

## <a name="steps"></a>Kroki

Podstawowa konfiguracja przykładu jest następująca: formant `TextBox` jest używany jako znak wodny przy użyciu kontrolki `TextBoxWatermarkExtender`. Przycisk wyzwala ogłaszanie zwrotne i będzie później używany do wyzwalania kontrolek weryfikacji na stronie. Ponadto kontrolka `ScriptManager` jest wymagana do zainicjowania ASP.NET AJAX:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample1.aspx)]

Teraz Dodaj kontrolkę `RequiredFieldValidator`, która sprawdza, czy w polu występuje tekst w czasie przesyłania formularza. Właściwość `InitialValue` modułu walidacji musi być ustawiona na taką samą wartość, która jest używana w kontrolce `TextBoxWatermarkExtender`: gdy formularz zostanie przesłany, wartość niezmienionego pola tekstowego jest wartością limitu dolnego:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample2.aspx)]

Istnieje jednak jeden problem z tym podejściem: Jeśli klient wyłączy kod JavaScript, pole tekstowe nie jest wypełniane tekstem znaku wodnego, w związku z czym `RequiredFieldValidator` nie wyzwala komunikatu o błędzie. W związku z tym, wymagana jest druga kontrolka `RequiredFieldValidator`, która sprawdza puste pole tekstowe (Pomijanie atrybutu `InitialValue`).

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample3.aspx)]

Ponieważ zarówno moduł sprawdzania poprawności używa `Display`=`"Dynamic"`, użytkownik końcowy nie może odróżnić od wyglądu wizualizacji, który został wyzwolony przez dwa moduły walidacji; Zamiast tego wygląda na to, że wystąpiły tylko jeden z nich.

Na koniec Dodaj kod po stronie serwera, aby wyprowadzić tekst w polu, jeśli żaden moduł sprawdzania poprawności nie wygenerował komunikatu o błędzie:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample4.aspx)]

[![moduł sprawdzania poprawności zgłasza, że w polu nie ma tekstu](using-textboxwatermark-with-validation-controls-cs/_static/image2.png)](using-textboxwatermark-with-validation-controls-cs/_static/image1.png)

Moduł sprawdzania poprawności zgłasza, że w polu nie ma tekstu ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](using-textboxwatermark-with-validation-controls-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](using-textboxwatermark-in-a-formview-cs.md)
> [dalej](using-textboxwatermark-in-a-formview-vb.md)

---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-cs
title: Wykonywanie animacji przy użyciu kodu po stronie klientaC#() | Microsoft Docs
author: wenz
description: Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. Wykonanie animacji...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 0270e0df-6fde-4a8f-a2cb-2cacc55143f2
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-cs
msc.type: authoredcontent
ms.openlocfilehash: b6ba1553b9c8c51d5d6ae1679e53f9cc1d17b769
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598183"
---
# <a name="executing-animations-using-client-side-code-c"></a>Wykonywanie animacji przy użyciu kodu po stronie klienta (C#)

Autor [Christian Wenz](https://github.com/wenz)

[Pobierz kod](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.cs.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10CS.pdf)

> Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. Wykonanie animacji może być również wyzwalane przy użyciu niestandardowego kodu JavaScript po stronie klienta.

## <a name="overview"></a>Omówienie

Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. Wykonanie animacji może być również wyzwalane przy użyciu niestandardowego kodu JavaScript po stronie klienta.

## <a name="steps"></a>Kroki

Po pierwsze, Uwzględnij `ScriptManager` na stronie; następnie załadowana zostanie Biblioteka ASP.NET AJAX, co umożliwia korzystanie z zestawu narzędzi kontroli:

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample1.aspx)]

Animacja zostanie zastosowana do panelu tekstu, który wygląda następująco:

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample2.aspx)]

W skojarzonej klasie CSS panelu, zdefiniuj kolor tła całkiem, a także Ustaw stałą szerokość panelu:

[!code-css[Main](executing-animations-using-client-side-code-cs/samples/sample3.css)]

Następnie Dodaj `AnimationExtender` do strony, podając `ID`, atrybut `TargetControlID` i obowiązkowe `runat="server"`:

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample4.aspx)]

W węźle `<Animations>` Użyj `<OnClick>`, aby uruchomić animacje, gdy użytkownik kliknie panel. Dodaj dwie animacje do wykonania równolegle:

[!code-xml[Main](executing-animations-using-client-side-code-cs/samples/sample5.xml)]

Ze względu na demonstrację ta animacja (i wszelkie inne animacje utworzone przy użyciu zestawu narzędzi sterowania) są wykonywane przy użyciu kodu JavaScript, gdy strona zostanie uruchomiona. Najpierw musimy uzyskać dostęp do kontrolki `AnimationExtender`. Biblioteka ASP.NET AJAX zawiera funkcję `$find()` dla tego zadania:

[!code-csharp[Main](executing-animations-using-client-side-code-cs/samples/sample6.cs)]

Formant `AnimationExtender` uwidacznia bogaty interfejs API, w tym metody o nazwach identycznych z programami obsługi zdarzeń używanymi w znacznikach XML: `OnClick()`, `OnLoad()`i tak dalej. Na przykład wywołanie metody `OnClick()` wykonuje animację wewnątrz elementu `<OnClick>` kontrolki `AnimationExtender`:

[!code-javascript[Main](executing-animations-using-client-side-code-cs/samples/sample7.js)]

Poniżej znajduje się kompletny kod JavaScript po stronie klienta, który emuluje kliknięcie panelu po całkowitym załadowaniu strony, należy zauważyć, że nazwa funkcji `pageLoad()` jest używana, która jest wywoływana przez ASP.NET AJAX po załadowaniu strony i wszystkich dołączonych bibliotek języka JavaScript.

[!code-html[Main](executing-animations-using-client-side-code-cs/samples/sample8.html)]

[![animacje są uruchamiane natychmiast, bez kliknięcia przycisku myszy](executing-animations-using-client-side-code-cs/_static/image2.png)](executing-animations-using-client-side-code-cs/_static/image1.png)

Animacja zostanie uruchomiona natychmiast, bez kliknięcia przycisku myszy ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](executing-animations-using-client-side-code-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](modifying-animations-from-the-server-side-cs.md)
> [dalej](changing-an-animation-using-client-side-code-cs.md)

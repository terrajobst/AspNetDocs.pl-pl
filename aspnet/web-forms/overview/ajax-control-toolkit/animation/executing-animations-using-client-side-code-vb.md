---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-vb
title: Wykonywanie animacji przy użyciu kodu po stronie klienta (VB) | Microsoft Docs
author: wenz
description: Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. Wykonanie animacji...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: f7073f50-d765-456d-9957-926ce60f35f6
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 7ef36900d20d8d07c3c6f3b63ce96568a377a0ed
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74575472"
---
# <a name="executing-animations-using-client-side-code-vb"></a>Wykonywanie animacji przy użyciu kodu po stronie klienta (VB)

Autor [Christian Wenz](https://github.com/wenz)

[Pobierz kod](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.vb.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10VB.pdf)

> Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. Wykonanie animacji może być również wyzwalane przy użyciu niestandardowego kodu JavaScript po stronie klienta.

## <a name="overview"></a>Omówienie

Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. Wykonanie animacji może być również wyzwalane przy użyciu niestandardowego kodu JavaScript po stronie klienta.

## <a name="steps"></a>Kroki

Po pierwsze, Uwzględnij `ScriptManager` na stronie; następnie załadowana zostanie Biblioteka ASP.NET AJAX, co umożliwia korzystanie z zestawu narzędzi kontroli:

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample1.aspx)]

Animacja zostanie zastosowana do panelu tekstu, który wygląda następująco:

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample2.aspx)]

W skojarzonej klasie CSS panelu, zdefiniuj kolor tła całkiem, a także Ustaw stałą szerokość panelu:

[!code-css[Main](executing-animations-using-client-side-code-vb/samples/sample3.css)]

Następnie Dodaj `AnimationExtender` do strony, podając `ID`, atrybut `TargetControlID` i obowiązkowe `runat="server"`:

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample4.aspx)]

W węźle `<Animations>` Użyj `<OnClick>`, aby uruchomić animacje, gdy użytkownik kliknie panel. Dodaj dwie animacje do wykonania równolegle:

[!code-xml[Main](executing-animations-using-client-side-code-vb/samples/sample5.xml)]

Ze względu na demonstrację ta animacja (i wszelkie inne animacje utworzone przy użyciu zestawu narzędzi sterowania) są wykonywane przy użyciu kodu JavaScript, gdy strona zostanie uruchomiona. Najpierw musimy uzyskać dostęp do kontrolki `AnimationExtender`. Biblioteka ASP.NET AJAX zawiera funkcję `$find()` dla tego zadania:

[!code-csharp[Main](executing-animations-using-client-side-code-vb/samples/sample6.cs)]

Formant `AnimationExtender` uwidacznia bogaty interfejs API, w tym metody o nazwach identycznych z programami obsługi zdarzeń używanymi w znacznikach XML: `OnClick()`, `OnLoad()`i tak dalej. Na przykład wywołanie metody `OnClick()` wykonuje animację wewnątrz elementu `<OnClick>` kontrolki `AnimationExtender`:

[!code-javascript[Main](executing-animations-using-client-side-code-vb/samples/sample7.js)]

Poniżej znajduje się kompletny kod JavaScript po stronie klienta, który emuluje kliknięcie panelu po całkowitym załadowaniu strony, należy zauważyć, że nazwa funkcji `pageLoad()` jest używana, która jest wywoływana przez ASP.NET AJAX po załadowaniu strony i wszystkich dołączonych bibliotek języka JavaScript.

[!code-html[Main](executing-animations-using-client-side-code-vb/samples/sample8.html)]

[![animacje są uruchamiane natychmiast, bez kliknięcia przycisku myszy](executing-animations-using-client-side-code-vb/_static/image2.png)](executing-animations-using-client-side-code-vb/_static/image1.png)

Animacja zostanie uruchomiona natychmiast, bez kliknięcia przycisku myszy ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](executing-animations-using-client-side-code-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](modifying-animations-from-the-server-side-vb.md)
> [dalej](changing-an-animation-using-client-side-code-vb.md)

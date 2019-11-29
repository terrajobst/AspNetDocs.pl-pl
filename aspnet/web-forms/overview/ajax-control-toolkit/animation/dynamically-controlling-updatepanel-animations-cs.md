---
uid: web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-cs
title: Dynamiczne kontrolowanie animacji UpdatePanelC#() | Microsoft Docs
author: wenz
description: Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. Dla zawartości...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 5138b8fe-98ff-4e73-a00b-e263fc3ff11d
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-cs
msc.type: authoredcontent
ms.openlocfilehash: 183974564764aab9c0d8a4e577995f3c444bf2d3
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74606795"
---
# <a name="dynamically-controlling-updatepanel-animations-c"></a>Dynamiczne kontrolowanie animacji kontrolki UpdatePanel (C#)

Autor [Christian Wenz](https://github.com/wenz)

[Pobierz kod](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.cs.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2CS.pdf)

> Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. W przypadku zawartości elementu UpdatePanel istnieje specjalne rozszerzenie, które opiera się na środowisku animacji: UpdatePanelAnimation. Może również współdziałać z wyzwalaczami UpdatePanel.

## <a name="overview"></a>Omówienie

Kontrolka animacji w narzędziu ASP.NET AJAX Control Toolkit nie jest tylko kontrolką, ale całą strukturą służącą do dodawania animacji do kontrolki. W przypadku zawartości `UpdatePanel`istnieje specjalne rozszerzenie, które opiera się na środowisku animacji: `UpdatePanelAnimation`. Może również współdziałać z wyzwalaczami `UpdatePanel`.

## <a name="steps"></a>Kroki

Pierwszym krokiem jest zwykle uwzględnienie `ScriptManager` na stronie, tak aby Biblioteka ASP.NET AJAX została załadowana i można było użyć zestawu narzędzi do sterowania:

[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample1.aspx)]

Animacja w tym scenariuszu zostanie zastosowana do wyświetlania bieżącego czasu. Te informacje mogą być zapisywane w etykiecie przy użyciu metody `Page_Load()` lub (dla uproszczenia) użyto następującego kodu wbudowanego:

[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample2.aspx)]

Ponadto zostanie utworzony przycisk służący do wyzwalania aktualizacji czasu:

[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample3.aspx)]

Ten kod jest następnie umieszczany w sekcji `<ContentTemplate>` elementu `UpdatePanel`. Atrybut `UpdateMode` panelu musi być ustawiony na `"Conditional"`, ponieważ tylko Wyzwalacze mogą zaktualizować zawartość panelu. W `<Triggers>` sekcji `UpdatePanel`zostanie utworzony asynchroniczny wyzwalacz ogłaszania zwrotnego i powiązany z zdarzeniem `Click` przycisku. W takim przypadku, jeśli użytkownik kliknie przycisk, `UpdatePanel` jest odświeżane. Oto znaczniki dla kontrolki `UpdatePanel`:

[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample4.aspx)]

Na koniec należy skonfigurować `UpdatePanelAnimationExtender`: ustaw atrybut `TargetControlID` na identyfikator panelu i zdefiniuj animację w ramach rozszerzenia. Zanikanie w systemie ma sens, co sprawia, że jest to bardzo atrakcyjny wizualny nacisk na zaktualizowany czas. Znaczniki rozszerzające mogą następnie wyglądać następująco:

[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample5.aspx)]

Uruchom plik w przeglądarce. Za każdym razem, gdy klikniesz przycisk, bieżący czas jest pokazywany w panelu, zawsze zanika w czasie trwania jednej sekundy.

[![bieżący czas zanika w](dynamically-controlling-updatepanel-animations-cs/_static/image2.png)](dynamically-controlling-updatepanel-animations-cs/_static/image1.png)

Bieżąca godzina powoduje zanikanie ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](dynamically-controlling-updatepanel-animations-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](animating-an-updatepanel-control-cs.md)
> [dalej](adding-animation-to-a-control-vb.md)

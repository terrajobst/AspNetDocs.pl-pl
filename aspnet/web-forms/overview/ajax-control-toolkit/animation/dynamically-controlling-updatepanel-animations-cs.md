---
uid: web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-cs
title: Dynamiczne kontrolowanie animacji kontrolki UpdatePanel (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki. Dla zawartości...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 5138b8fe-98ff-4e73-a00b-e263fc3ff11d
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-cs
msc.type: authoredcontent
ms.openlocfilehash: 0767b66a035069629c15e658c1e75ea78a7bd07b
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/09/2019
ms.locfileid: "59407656"
---
# <a name="dynamically-controlling-updatepanel-animations-c"></a>Dynamiczne kontrolowanie animacji kontrolki UpdatePanel (C#)

przez [Christian Wenz](https://github.com/wenz)

[Pobierz program Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.cs.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2CS.pdf)

> Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki. Dla zawartości UpdatePanel specjalne rozszerzenia istnieje już intensywnie korzystającej z framework animacji: UpdatePanelAnimation. Może również współdziałać, wraz z wyzwalaczy UpdatePanel.


## <a name="overview"></a>Omówienie

Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki. Dla zawartości `UpdatePanel`, specjalne urządzenia extender istnieje która intensywnie korzysta z framework animacji: `UpdatePanelAnimation`. Może również współpracować wraz z `UpdatePanel` wyzwalaczy.

## <a name="steps"></a>Kroki

Pierwszym krokiem jest jak zwykle obejmują `ScriptManager` na stronie, aby biblioteka ASP.NET AJAX jest ładowany i można go używać razem sterowania:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample1.aspx)]

Animacja w tym scenariuszu zostaną zastosowane do wyświetlenia bieżącej godziny. Te informacje mogą być zapisywane w etykiecie, używając `Page_Load()` metodę, lub (dla uproszczenia należy) jest używany następujący kod wbudowany:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample2.aspx)]

Ponadto jest tworzony przycisk do wyzwalania aktualizacji w czasie:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample3.aspx)]

Następnie umieść ten kod `<ContentTemplate>` części `UpdatePanel` elementu. Panel `UpdateMode` atrybutu musi być równa `"Conditional"`, ponieważ tylko wyzwalaczy może aktualizować zawartość panelu. W `<Triggers>` części `UpdatePanel`, asynchronicznego zwrotu wyzwalacza jest tworzony i powiązane `Click` zdarzenia przycisku. Dlatego, jeśli użytkownik kliknie przycisk, `UpdatePanel` jest odświeżane. Oto znaczniki dla `UpdatePanel` sterowania:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample4.aspx)]

Na koniec `UpdatePanelAnimationExtender` muszą być skonfigurowane: Ustaw `TargetControlID` atrybutu ID panelu i zdefiniuj animacji w ramach urządzenia extender. Rozjaśnianie sprawia, że sens, co powoduje utworzenie nieuprzywilejowany visual większym naciskiem na czas aktualizacji. Rozszerzenia znaczników może następnie wyglądać następująco:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample5.aspx)]

Uruchom plik w przeglądarce. Zawsze, gdy klikniesz przycisk, bieżący czas jest wyświetlany w panelu zawsze zanikania przy czasie trwania jednej sekundy.


[![TADAM bieżący czas jest stopniowe](dynamically-controlling-updatepanel-animations-cs/_static/image2.png)](dynamically-controlling-updatepanel-animations-cs/_static/image1.png)

Bieżący czas jest stopniowe ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](dynamically-controlling-updatepanel-animations-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](animating-an-updatepanel-control-cs.md)
> [dalej](adding-animation-to-a-control-vb.md)

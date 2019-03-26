---
uid: web-forms/videos/aspnet-ajax/how-do-i-use-the-conditional-updatemode-of-the-updatepanel
title: '[Jak mogę] Użyj warunkowego trybu UpdateMode kontrolki UpdatePanel? | Microsoft Docs'
author: JoeStagner
description: Kontrolki UpdatePanel ASP.NET AJAX zawiera właściwość UpdateMode, która może być ustawiona na "Always" lub "Warunkowe". Wartość domyślna to zawsze, w którym to przypadku UpdatePan...
ms.author: riande
ms.date: 08/01/2007
ms.assetid: 10b5bad3-4c18-464f-9454-0b3e60b7b8be
msc.legacyurl: /web-forms/videos/aspnet-ajax/how-do-i-use-the-conditional-updatemode-of-the-updatepanel
msc.type: video
ms.openlocfilehash: d1d407863340ad143c9859263ff66d538ca00335
ms.sourcegitcommit: 289e051cc8a90e8f7127e239fda73047bde4de12
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/25/2019
ms.locfileid: "58423835"
---
<a name="how-do-i-use-the-conditional-updatemode-of-the-updatepanel"></a><span data-ttu-id="70be4-105">[Jak mogę] Użyj warunkowego trybu UpdateMode kontrolki UpdatePanel?</span><span class="sxs-lookup"><span data-stu-id="70be4-105">[How Do I:] Use the Conditional UpdateMode of the UpdatePanel?</span></span>
====================
<span data-ttu-id="70be4-106">przez [Stagner Jan](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="70be4-106">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

<span data-ttu-id="70be4-107">Kontrolki UpdatePanel ASP.NET AJAX zawiera właściwość UpdateMode, która może być ustawiona na "Always" lub "Warunkowe".</span><span class="sxs-lookup"><span data-stu-id="70be4-107">The ASP.NET AJAX UpdatePanel includes an UpdateMode property that may be set to 'Always' or 'Conditional'.</span></span> <span data-ttu-id="70be4-108">Wartość domyślna to zawsze, w którym to przypadku UpdatePanel zawsze zaktualizuje jego zawartości podczas odświeżania asynchronicznego.</span><span class="sxs-lookup"><span data-stu-id="70be4-108">The default is Always, in which case the UpdatePanel will always update its content during an asynchronous postback.</span></span> <span data-ttu-id="70be4-109">W tym filmie pokazano dowie się, jak możemy ustawić UpdateMode na warunkowego, w którym przypadku UpdatePanel obejmie tylko jego zawartość po jej metodę aktualizacji naszego kodu po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="70be4-109">In this video we learn how we can set the UpdateMode to Conditional, in which case the UpdatePanel will only update its content when our server-side code calls its Update method.</span></span> <span data-ttu-id="70be4-110">Dzięki temu można używać logikę warunkową w języku C# lub kod języka Visual Basic do określania, czy kontrolki UpdatePanel zaktualizuje jego zawartość w ciągu bieżącego asynchronicznego zwrotu.</span><span class="sxs-lookup"><span data-stu-id="70be4-110">This allows you to use conditional logic in your C# or Visual Basic code to determine whether the UpdatePanel will update its content during the current asynchronous postback.</span></span>

[<span data-ttu-id="70be4-111">&#9654;Obejrzyj film wideo (minuty 13)</span><span class="sxs-lookup"><span data-stu-id="70be4-111">&#9654; Watch video (13 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-use-the-conditional-updatemode-of-the-updatepanel)

> [!div class="step-by-step"]
> <span data-ttu-id="70be4-112">[Poprzednie](how-do-i-determine-whether-an-asynchronous-postback-has-occurred.md)
> [dalej](how-do-i-implement-the-persistent-communications-pattern-with-the-updatepanel.md)</span><span class="sxs-lookup"><span data-stu-id="70be4-112">[Previous](how-do-i-determine-whether-an-asynchronous-postback-has-occurred.md)
[Next](how-do-i-implement-the-persistent-communications-pattern-with-the-updatepanel.md)</span></span>

---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-8
title: Wyświetl szczegóły elementu | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 75ef94b1-bbec-4681-9210-452dba816144
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-8
msc.type: authoredcontent
ms.openlocfilehash: 3f48c5ad73ceb9a4873fbbb621b518553a498966
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557324"
---
# <a name="display-item-details"></a><span data-ttu-id="e79db-102">Wyświetlanie szczegółów elementu</span><span class="sxs-lookup"><span data-stu-id="e79db-102">Display Item Details</span></span>

<span data-ttu-id="e79db-103">według [Jan Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="e79db-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="e79db-104">Pobierz ukończony projekt</span><span class="sxs-lookup"><span data-stu-id="e79db-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="e79db-105">W tej sekcji dodasz możliwość wyświetlania szczegółów dla każdej książki.</span><span class="sxs-lookup"><span data-stu-id="e79db-105">In this section, you will add the ability to view details for each book.</span></span> <span data-ttu-id="e79db-106">W aplikacji App. js Dodaj do modelu widoku następujący kod:</span><span class="sxs-lookup"><span data-stu-id="e79db-106">In app.js, add to the following code to the view model:</span></span>

[!code-javascript[Main](part-8/samples/sample1.js)]

<span data-ttu-id="e79db-107">W widokach/Home/index. cshtml Dodaj element powiązania danych do linku szczegóły:</span><span class="sxs-lookup"><span data-stu-id="e79db-107">In Views/Home/Index.cshtml, add a data-bind element to the Details link:</span></span>

[!code-html[Main](part-8/samples/sample2.html?highlight=5)]

<span data-ttu-id="e79db-108">To wiąże procedurę obsługi kliknięcia dla &lt;elementu&gt; do funkcji `getBookDetail` w modelu widoku.</span><span class="sxs-lookup"><span data-stu-id="e79db-108">This binds the click handler for the &lt;a&gt; element to the `getBookDetail` function on the view model.</span></span>

<span data-ttu-id="e79db-109">W tym samym pliku Zastąp następujące oznakowanie:</span><span class="sxs-lookup"><span data-stu-id="e79db-109">In the same file, replace the following mark-up:</span></span>

[!code-html[Main](part-8/samples/sample3.html)]

<span data-ttu-id="e79db-110">na kod:</span><span class="sxs-lookup"><span data-stu-id="e79db-110">with this:</span></span>

[!code-html[Main](part-8/samples/sample4.html)]

<span data-ttu-id="e79db-111">Ten znacznik tworzy tabelę zawierającą dane powiązane z właściwościami `detail` widocznymi w modelu widoku.</span><span class="sxs-lookup"><span data-stu-id="e79db-111">This markup creates a table that is data-bound to the properties of the `detail` observable in the view model.</span></span>

<span data-ttu-id="e79db-112">Składnia "&lt;!--ko-&quot;&gt;umożliwia uwzględnienie powiązania odcinania poza elementem modelu DOM.</span><span class="sxs-lookup"><span data-stu-id="e79db-112">The "&lt;!-- ko --&gt;&quot; syntax lets you include a Knockout binding outside of a DOM element.</span></span> <span data-ttu-id="e79db-113">W takim przypadku powiązanie `if` powoduje, że ta sekcja znaczników będzie wyświetlana tylko wtedy, gdy `details` ma wartość różną od null.</span><span class="sxs-lookup"><span data-stu-id="e79db-113">In this case, the `if` binding causes this section of markup to be displayed only when `details` is non-null.</span></span>

[!code-html[Main](part-8/samples/sample5.html)]

<span data-ttu-id="e79db-114">Teraz, jeśli uruchomisz aplikację i klikniesz jeden z &quot;szczegóły&quot; linki, aplikacja wyświetli Szczegóły książki.</span><span class="sxs-lookup"><span data-stu-id="e79db-114">Now if you run the app and click one of the &quot;Detail&quot; links, the app will display the book details.</span></span>

[![](part-8/_static/image2.png)](part-8/_static/image1.png)

> [!div class="step-by-step"]
> <span data-ttu-id="e79db-115">[Poprzednie](part-7.md)
> [dalej](part-9.md)</span><span class="sxs-lookup"><span data-stu-id="e79db-115">[Previous](part-7.md)
[Next](part-9.md)</span></span>

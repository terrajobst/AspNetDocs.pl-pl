---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-8
title: Wyświetl szczegóły elementu | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 75ef94b1-bbec-4681-9210-452dba816144
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-8
msc.type: authoredcontent
ms.openlocfilehash: 83f291326307a4964afdd5e8b50f2c375348ed0e
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57078275"
---
<a name="display-item-details"></a><span data-ttu-id="45c24-102">Wyświetlanie szczegółów elementu</span><span class="sxs-lookup"><span data-stu-id="45c24-102">Display Item Details</span></span>
====================
<span data-ttu-id="45c24-103">przez [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="45c24-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="45c24-104">Pobierz ukończony projekt</span><span class="sxs-lookup"><span data-stu-id="45c24-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="45c24-105">W tej sekcji dodasz możliwości, aby wyświetlić szczegóły dotyczące każdej książki.</span><span class="sxs-lookup"><span data-stu-id="45c24-105">In this section, you will add the ability to view details for each book.</span></span> <span data-ttu-id="45c24-106">W app.js Dodaj poniższy kod do modelu widoku:</span><span class="sxs-lookup"><span data-stu-id="45c24-106">In app.js, add to the following code to the view model:</span></span>

[!code-javascript[Main](part-8/samples/sample1.js)]

<span data-ttu-id="45c24-107">W Views/Home/Index.cshtml Dodaj element wiązania danych do łącza Szczegóły:</span><span class="sxs-lookup"><span data-stu-id="45c24-107">In Views/Home/Index.cshtml, add a data-bind element to the Details link:</span></span>

[!code-html[Main](part-8/samples/sample2.html?highlight=5)]

<span data-ttu-id="45c24-108">Powoduje to powiązanie programu obsługi kliknij &lt;&gt; elementu `getBookDetail` funkcji na model widoku.</span><span class="sxs-lookup"><span data-stu-id="45c24-108">This binds the click handler for the &lt;a&gt; element to the `getBookDetail` function on the view model.</span></span>

<span data-ttu-id="45c24-109">W tym samym pliku Zamień następujące wycinka:</span><span class="sxs-lookup"><span data-stu-id="45c24-109">In the same file, replace the following mark-up:</span></span>

[!code-html[Main](part-8/samples/sample3.html)]

<span data-ttu-id="45c24-110">na kod:</span><span class="sxs-lookup"><span data-stu-id="45c24-110">with this:</span></span>

[!code-html[Main](part-8/samples/sample4.html)]

<span data-ttu-id="45c24-111">Ten kod znaczników tworzy tabelę, która jest powiązany z danymi właściwości `detail` zauważalne w modelu widoku.</span><span class="sxs-lookup"><span data-stu-id="45c24-111">This markup creates a table that is data-bound to the properties of the `detail` observable in the view model.</span></span>

<span data-ttu-id="45c24-112">"&lt;!--Ko —&gt; &quot; składni pozwala objąć powiązanie Knockout poza elementu DOM.</span><span class="sxs-lookup"><span data-stu-id="45c24-112">The "&lt;!-- ko --&gt;&quot; syntax lets you include a Knockout binding outside of a DOM element.</span></span> <span data-ttu-id="45c24-113">W tym przypadku `if` powiązania powoduje, że ta część znaczników, które mają być wyświetlane tylko wtedy, gdy `details` jest różna od null.</span><span class="sxs-lookup"><span data-stu-id="45c24-113">In this case, the `if` binding causes this section of markup to be displayed only when `details` is non-null.</span></span>

[!code-html[Main](part-8/samples/sample5.html)]

<span data-ttu-id="45c24-114">Teraz uruchom aplikację i kliknij jeden z &quot;szczegółów&quot; łącza, aplikacji spowoduje wyświetlenie szczegółów książki.</span><span class="sxs-lookup"><span data-stu-id="45c24-114">Now if you run the app and click one of the &quot;Detail&quot; links, the app will display the book details.</span></span>

[![](part-8/_static/image2.png)](part-8/_static/image1.png)

> [!div class="step-by-step"]
> <span data-ttu-id="45c24-115">[Poprzednie](part-7.md)
> [dalej](part-9.md)</span><span class="sxs-lookup"><span data-stu-id="45c24-115">[Previous](part-7.md)
[Next](part-9.md)</span></span>
---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-9
title: Dodaj nowy element do bazy danych | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 0967c29e-e124-4db0-a788-c45d0ff5aff2
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-9
msc.type: authoredcontent
ms.openlocfilehash: 692269a2c11e529af78f24feca74bba704b5b54b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557289"
---
# <a name="add-a-new-item-to-the-database"></a><span data-ttu-id="1e02f-102">Dodawanie nowego elementu do bazy danych</span><span class="sxs-lookup"><span data-stu-id="1e02f-102">Add a New Item to the Database</span></span>

<span data-ttu-id="1e02f-103">według [Jan Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="1e02f-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="1e02f-104">Pobierz ukończony projekt</span><span class="sxs-lookup"><span data-stu-id="1e02f-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="1e02f-105">W tej sekcji dodasz możliwość tworzenia nowej książki przez użytkowników.</span><span class="sxs-lookup"><span data-stu-id="1e02f-105">In this section, you will add the ability for users to create a new book.</span></span> <span data-ttu-id="1e02f-106">W aplikacji App. js Dodaj następujący kod do modelu widoku:</span><span class="sxs-lookup"><span data-stu-id="1e02f-106">In app.js, add the following code to the view model:</span></span>

[!code-javascript[Main](part-9/samples/sample1.js)]

<span data-ttu-id="1e02f-107">W index. cshtml Zastąp następujące znaczniki:</span><span class="sxs-lookup"><span data-stu-id="1e02f-107">In Index.cshtml, replace the following markup:</span></span>

[!code-html[Main](part-9/samples/sample2.html)]

<span data-ttu-id="1e02f-108">Tym:</span><span class="sxs-lookup"><span data-stu-id="1e02f-108">With:</span></span>

[!code-html[Main](part-9/samples/sample3.html)]

<span data-ttu-id="1e02f-109">Ten znacznik tworzy formularz do przesyłania nowego autora.</span><span class="sxs-lookup"><span data-stu-id="1e02f-109">This markup creates a form for submitting a new author.</span></span> <span data-ttu-id="1e02f-110">Wartości dla listy rozwijanej autor są powiązane z `authors` widocznymi w modelu widoku.</span><span class="sxs-lookup"><span data-stu-id="1e02f-110">The values for the author drop-down list are data-bound to the `authors` observable in the view model.</span></span> <span data-ttu-id="1e02f-111">W przypadku innych danych wejściowych formularza wartości są powiązane z danymi właściwością `newBook` modelu widoku.</span><span class="sxs-lookup"><span data-stu-id="1e02f-111">For the other form inputs, the values are data-bound to the `newBook` property of the view model.</span></span>

<span data-ttu-id="1e02f-112">Procedura obsługi przesyłania w formularzu jest powiązana z funkcją `addBook`:</span><span class="sxs-lookup"><span data-stu-id="1e02f-112">The submit handler on the form is bound to the `addBook` function:</span></span>

[!code-html[Main](part-9/samples/sample4.html)]

<span data-ttu-id="1e02f-113">Funkcja `addBook` odczytuje bieżące wartości danych wejściowych formularza powiązanych z danymi, aby utworzyć obiekt JSON.</span><span class="sxs-lookup"><span data-stu-id="1e02f-113">The `addBook` function reads the current values of the data-bound form inputs to create a JSON object.</span></span> <span data-ttu-id="1e02f-114">Następnie zapisuje obiekt JSON w `/api/books`.</span><span class="sxs-lookup"><span data-stu-id="1e02f-114">Then it POSTs the JSON object to `/api/books`.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="1e02f-115">[Poprzednie](part-8.md)
> [dalej](part-10.md)</span><span class="sxs-lookup"><span data-stu-id="1e02f-115">[Previous](part-8.md)
[Next](part-10.md)</span></span>

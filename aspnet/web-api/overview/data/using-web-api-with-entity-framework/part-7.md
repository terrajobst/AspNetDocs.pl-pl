---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-7
title: Utwórz widok (interfejs użytkownika) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: b2445062-a1fe-4133-8994-f510280f6d9a
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-7
msc.type: authoredcontent
ms.openlocfilehash: 62c4523c2c6fb399cfbc3716309a1379996d601c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557303"
---
# <a name="create-the-view-ui"></a><span data-ttu-id="36345-102">Tworzenie widoku (interfejs użytkownika)</span><span class="sxs-lookup"><span data-stu-id="36345-102">Create the View (UI)</span></span>

<span data-ttu-id="36345-103">według [Jan Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="36345-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="36345-104">Pobierz ukończony projekt</span><span class="sxs-lookup"><span data-stu-id="36345-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="36345-105">W tej sekcji rozpocznie się Definiowanie kodu HTML dla aplikacji i Dodawanie powiązania danych między kodem HTML i modelem widoku.</span><span class="sxs-lookup"><span data-stu-id="36345-105">In this section, you will start to define the HTML for the app, and add data binding between the HTML and the view model.</span></span>

<span data-ttu-id="36345-106">Otwórz plik widoki/Home/index. cshtml.</span><span class="sxs-lookup"><span data-stu-id="36345-106">Open the file Views/Home/Index.cshtml.</span></span> <span data-ttu-id="36345-107">Zastąp całą zawartość tego pliku następującym.</span><span class="sxs-lookup"><span data-stu-id="36345-107">Replace the entire contents of that file with the following.</span></span>

[!code-cshtml[Main](part-7/samples/sample1.cshtml)]

<span data-ttu-id="36345-108">Większość `div` elementów ma dla stylów [ładowania początkowego](http://getbootstrap.com/) .</span><span class="sxs-lookup"><span data-stu-id="36345-108">Most of the `div` elements are there for [Bootstrap](http://getbootstrap.com/) styling.</span></span> <span data-ttu-id="36345-109">Ważne elementy są tymi, których atrybuty `data-bind`.</span><span class="sxs-lookup"><span data-stu-id="36345-109">The important elements are the ones with `data-bind` attributes.</span></span> <span data-ttu-id="36345-110">Ten atrybut łączy kod HTML z modelem widoku.</span><span class="sxs-lookup"><span data-stu-id="36345-110">This attribute links the HTML to the view model.</span></span>

<span data-ttu-id="36345-111">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="36345-111">For example:</span></span>

[!code-html[Main](part-7/samples/sample2.html)]

<span data-ttu-id="36345-112">W tym przykładzie &quot;`text`powiązania &quot; powoduje, że element `<p>` wyświetla wartość właściwości `error` z modelu widoku.</span><span class="sxs-lookup"><span data-stu-id="36345-112">In this example, the &quot;`text`&quot; binding causes the `<p>` element to show the value of the `error` property from the view model.</span></span> <span data-ttu-id="36345-113">Odwołaj ten `error` został zadeklarowany jako `ko.observable`:</span><span class="sxs-lookup"><span data-stu-id="36345-113">Recall that `error` was declared as a `ko.observable`:</span></span>

[!code-javascript[Main](part-7/samples/sample3.js)]

<span data-ttu-id="36345-114">Za każdym razem, gdy nowa wartość jest przypisana do `error`, odcinanie aktualizuje tekst w elemencie `<p>`.</span><span class="sxs-lookup"><span data-stu-id="36345-114">Whenever a new value is assigned to `error`, Knockout updates the text in the `<p>` element.</span></span>

<span data-ttu-id="36345-115">Powiązanie `foreach` informuje odcinanie pętli przez zawartość tablicy `books`.</span><span class="sxs-lookup"><span data-stu-id="36345-115">The `foreach` binding tells Knockout to loop through the contents of the `books` array.</span></span> <span data-ttu-id="36345-116">Dla każdego elementu w tablicy odcinanie tworzy nowy element &lt;li&gt;.</span><span class="sxs-lookup"><span data-stu-id="36345-116">For each item in the array, Knockout creates a new &lt;li&gt; element.</span></span> <span data-ttu-id="36345-117">Powiązania wewnątrz kontekstu `foreach` odnoszą się do właściwości elementu tablicy.</span><span class="sxs-lookup"><span data-stu-id="36345-117">Bindings inside the context of the `foreach` refer to properties on the array item.</span></span> <span data-ttu-id="36345-118">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="36345-118">For example:</span></span>

[!code-html[Main](part-7/samples/sample4.html)]

<span data-ttu-id="36345-119">W tym miejscu powiązanie `text` odczytuje właściwość Autor każdej książki.</span><span class="sxs-lookup"><span data-stu-id="36345-119">Here the `text` binding reads the Author property of each book.</span></span>

<span data-ttu-id="36345-120">Jeśli aplikacja zostanie uruchomiona teraz, powinna wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="36345-120">If you run the application now, it should look like this:</span></span>

![](part-7/_static/image1.png)

<span data-ttu-id="36345-121">Lista książek ładuje się asynchronicznie, po załadowaniu strony.</span><span class="sxs-lookup"><span data-stu-id="36345-121">The list of books loads asynchronously, after the page loads.</span></span> <span data-ttu-id="36345-122">Teraz linki &quot;szczegóły&quot; nie działają.</span><span class="sxs-lookup"><span data-stu-id="36345-122">Right now, the &quot;Details&quot; links are not functional.</span></span> <span data-ttu-id="36345-123">Ta funkcja zostanie dodana w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="36345-123">We'll add this functionality in the next section.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="36345-124">[Poprzednie](part-6.md)
> [dalej](part-8.md)</span><span class="sxs-lookup"><span data-stu-id="36345-124">[Previous](part-6.md)
[Next](part-8.md)</span></span>

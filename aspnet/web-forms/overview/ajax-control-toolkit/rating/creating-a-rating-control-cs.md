---
uid: web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-cs
title: Tworzenie kontrolki oceny (C#) | Microsoft Docs
author: wenz
description: Wiele witryn sieci Web — od handlu elektronicznego do witryn społecznościowych oferuje użytkownikom możliwość oceniania artykułów lub przedmiotów. Zwykle wymaga to pewnego nakładu na kodowanie, ale mamy...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 969fb28f-2bff-4fc4-b24a-27f5e2534a37
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-cs
msc.type: authoredcontent
ms.openlocfilehash: c0bf793406e378321f54f0460031c526a0b41a02
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74611571"
---
# <a name="creating-a-rating-control-c"></a><span data-ttu-id="4255a-104">Tworzenie kontrolki Rating (C#)</span><span class="sxs-lookup"><span data-stu-id="4255a-104">Creating a Rating Control (C#)</span></span>

<span data-ttu-id="4255a-105">Autor [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="4255a-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="4255a-106">[Pobierz kod](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/rating0.cs.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/rating0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="4255a-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/rating0.cs.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/rating0CS.pdf)</span></span>

> <span data-ttu-id="4255a-107">Wiele witryn sieci Web — od handlu elektronicznego do witryn społecznościowych oferuje użytkownikom możliwość oceniania artykułów lub przedmiotów.</span><span class="sxs-lookup"><span data-stu-id="4255a-107">Many websites, from e-commerce to community sites, offer their users to rate articles or items.</span></span> <span data-ttu-id="4255a-108">Zwykle wymaga to pewnego wysiłku związanego z kodowaniem, ale mamy do dyspozycji zestaw narzędzi kontroli.</span><span class="sxs-lookup"><span data-stu-id="4255a-108">This usually requires some coding effort, but we do have the Control Toolkit to our disposal.</span></span>

## <a name="overview"></a><span data-ttu-id="4255a-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="4255a-109">Overview</span></span>

<span data-ttu-id="4255a-110">Wiele witryn sieci Web — od handlu elektronicznego do witryn społecznościowych oferuje użytkownikom możliwość oceniania artykułów lub przedmiotów.</span><span class="sxs-lookup"><span data-stu-id="4255a-110">Many websites, from e-commerce to community sites, offer their users to rate articles or items.</span></span> <span data-ttu-id="4255a-111">Zwykle wymaga to pewnego wysiłku związanego z kodowaniem, ale mamy do dyspozycji zestaw narzędzi kontroli.</span><span class="sxs-lookup"><span data-stu-id="4255a-111">This usually requires some coding effort, but we do have the Control Toolkit to our disposal.</span></span>

## <a name="steps"></a><span data-ttu-id="4255a-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="4255a-112">Steps</span></span>

<span data-ttu-id="4255a-113">Najpierw wystarczy (co najmniej) dwa rodzaje obrazów: jeden dla wypełnionego elementu oceny i jeden dla pustego elementu oceny.</span><span class="sxs-lookup"><span data-stu-id="4255a-113">First of all, you need (at least) two kinds of images: one for a filled out rating item, and one for an empty rating item.</span></span> <span data-ttu-id="4255a-114">Element oceny jest zwykle gwiazdką lub uśmiechem.</span><span class="sxs-lookup"><span data-stu-id="4255a-114">A rating item is usually a star or a smiley.</span></span> <span data-ttu-id="4255a-115">W tym scenariuszu znajdziesz trzy pliki, buźki. png i Empty. png i smiley-done. png w ramach pobierania kodu źródłowego dla tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="4255a-115">For this scenario, you find three files, smiley.png and empty.png and smiley-done.png as part of the source code downloads for this tutorial.</span></span>

<span data-ttu-id="4255a-116">Następnie utwórz nowy plik ASP.NET i zacznij od dodania do niego formantu `ScriptManager`:</span><span class="sxs-lookup"><span data-stu-id="4255a-116">Then, create a new ASP.NET file and start with adding a `ScriptManager` control to it:</span></span>

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample1.aspx)]

<span data-ttu-id="4255a-117">Następnie Dodaj formant `Rating` z zestawu narzędzi ASP.NET AJAX Control Toolkit.</span><span class="sxs-lookup"><span data-stu-id="4255a-117">Then, add the `Rating` control from the ASP.NET AJAX Control Toolkit.</span></span> <span data-ttu-id="4255a-118">Następujące atrybuty muszą być ustawione na potrzeby tego przykładu:</span><span class="sxs-lookup"><span data-stu-id="4255a-118">The following attributes need to be set for this example:</span></span>

- <span data-ttu-id="4255a-119">`CurrentRating` klasyfikacji początkowej, która ma zostać użyta</span><span class="sxs-lookup"><span data-stu-id="4255a-119">`CurrentRating` the initial rating to be used</span></span>
- <span data-ttu-id="4255a-120">`MaxRating` maksymalną klasyfikację</span><span class="sxs-lookup"><span data-stu-id="4255a-120">`MaxRating` the maximum rating</span></span>
- <span data-ttu-id="4255a-121">`EmptyStarCssClass` klasy CSS, która ma być używana, gdy element klasyfikacji (gwiazdka) jest pusty</span><span class="sxs-lookup"><span data-stu-id="4255a-121">`EmptyStarCssClass` the CSS class to use when a rating item ( star ) is empty</span></span>
- <span data-ttu-id="4255a-122">`FilledStarCssClass` klasy CSS, która ma być używana w przypadku wypełnienia elementu klasyfikacji (gwiazdka)</span><span class="sxs-lookup"><span data-stu-id="4255a-122">`FilledStarCssClass` the CSS class to use when a rating item ( star ) is filled out</span></span>
- <span data-ttu-id="4255a-123">`StarCssClass` klasy CSS do użycia dla widocznego statu</span><span class="sxs-lookup"><span data-stu-id="4255a-123">`StarCssClass` the CSS class to use for a visible stat</span></span>
- <span data-ttu-id="4255a-124">`WaitingStarCssClass` klasy CSS, która ma być używana podczas wysyłania do serwera klasyfikacji gwiazdek</span><span class="sxs-lookup"><span data-stu-id="4255a-124">`WaitingStarCssClass` the CSS class to use while a star rating is sent back to the server</span></span>

<span data-ttu-id="4255a-125">A oto znacznik, który tworzy kontrolkę klasyfikacji z pięcioma elementami (uśmiechy), dla których żadna wartość nie jest początkowo wypełniana:</span><span class="sxs-lookup"><span data-stu-id="4255a-125">And here is the markup which creates a rating control with five items (smileys) of which none is filled out initially:</span></span>

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample2.aspx)]

<span data-ttu-id="4255a-126">Trzy klasy CSS, do których istnieją odwołania, teraz muszą wyświetlać odpowiednie pliki obrazów, co jest łatwe do użycia w CSS:</span><span class="sxs-lookup"><span data-stu-id="4255a-126">The three referenced CSS classes now need to show the appropriate image files, which is easy to do using CSS:</span></span>

[!code-css[Main](creating-a-rating-control-cs/samples/sample3.css)]

<span data-ttu-id="4255a-127">Upewnij się, że podajesz szerokość i wysokość trzech obrazów, w przeciwnym razie może wyglądać bit popełniliśmy up.</span><span class="sxs-lookup"><span data-stu-id="4255a-127">Make sure that you provide the width and height of the three images, otherwise the display may look a bit messed up.</span></span>

<span data-ttu-id="4255a-128">Na koniec wynik oceny powinien zostać wyświetlony użytkownikowi (lub, co najmniej zapisany w bazie danych).</span><span class="sxs-lookup"><span data-stu-id="4255a-128">Finally, the result of the rating should be displayed to the user (or, at least saved in a database).</span></span> <span data-ttu-id="4255a-129">Dodaj etykietę dla danych wyjściowych wiadomości tekstowej i przycisk Prześlij, aby opublikować formularz oceny na serwerze:</span><span class="sxs-lookup"><span data-stu-id="4255a-129">So add a label for the output of a text message and a submit button to post back the rating form to the server:</span></span>

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample4.aspx)]

<span data-ttu-id="4255a-130">W kodzie po stronie serwera, uzyskaj dostęp do kontrolki oceny za pośrednictwem jej `ID` a następnie uzyskaj dostęp do jej właściwości `CurrentRating`, która jest liczbą wybranych elementów rankingu, w naszym przykładzie wartość z przedziału od 0 do 5.</span><span class="sxs-lookup"><span data-stu-id="4255a-130">In the server-side code, access the Rating control via its `ID` and then access its `CurrentRating` property which is the number of the selected rating items, in our example a value between 0 and 5.</span></span>

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample5.aspx)]

<span data-ttu-id="4255a-131">Zapisz stronę i Załaduj ją do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="4255a-131">Save the page and load it into your browser.</span></span> <span data-ttu-id="4255a-132">Po umieszczeniu wskaźnika myszy nad elementami klasyfikacji (początkowo puste) występuje efekt JavaScript: Klasyfikacja zmienia się.</span><span class="sxs-lookup"><span data-stu-id="4255a-132">When you hover over the (initially empty) rating items, a JavaScript effect occurs: The rating changes.</span></span> <span data-ttu-id="4255a-133">Po kliknięciu zestawu gwiazdek bieżąca klasyfikacja zostanie zachowana.</span><span class="sxs-lookup"><span data-stu-id="4255a-133">When you click on the set of stars, the current rating is retained.</span></span> <span data-ttu-id="4255a-134">Na koniec po przesłaniu formularza kod po stronie serwera wyprowadza wybraną klasyfikację.</span><span class="sxs-lookup"><span data-stu-id="4255a-134">Finally, when you submit the form, the server-side code outputs the selected rating.</span></span>

<span data-ttu-id="4255a-135">[![tworzenia systemu klasyfikacji z minimalnym kodem](creating-a-rating-control-cs/_static/image2.png)](creating-a-rating-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="4255a-135">[![Creating a rating system with minimal code](creating-a-rating-control-cs/_static/image2.png)](creating-a-rating-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="4255a-136">Tworzenie systemu klasyfikacji z minimalnym kodem ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-rating-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="4255a-136">Creating a rating system with minimal code ([Click to view full-size image](creating-a-rating-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="4255a-137">Next</span><span class="sxs-lookup"><span data-stu-id="4255a-137">Next</span></span>](creating-a-rating-control-vb.md)

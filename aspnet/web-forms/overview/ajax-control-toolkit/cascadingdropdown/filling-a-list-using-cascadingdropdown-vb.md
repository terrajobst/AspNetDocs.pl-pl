---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-vb
title: Wypełnianie listy przy użyciu kontrolki CascadingDropDown (VB) | Microsoft Docs
author: wenz
description: Kontrolka kontrolki CascadingDropDown w zestawie narzędzi AJAX Control rozszerza kontrolkę DropDownList, tak aby zmiany w jednej DropDownList ładowały skojarzone wartości w anoth...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 5236695e-5c70-4887-baee-0bfb0afb3448
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: 8dd9ef8a4bdf705ba4451b7fd240e4de8618221c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536009"
---
# <a name="filling-a-list-using-cascadingdropdown-vb"></a><span data-ttu-id="9436a-103">Wypełnianie listy przy użyciu kontrolki CascadingDropDown (VB)</span><span class="sxs-lookup"><span data-stu-id="9436a-103">Filling a List Using CascadingDropDown (VB)</span></span>

<span data-ttu-id="9436a-104">Autor [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="9436a-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="9436a-105">[Pobierz kod](https://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.vb.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="9436a-105">[Download Code](https://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.vb.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0VB.pdf)</span></span>

> <span data-ttu-id="9436a-106">Kontrolka kontrolki CascadingDropDown w zestawie narzędzi AJAX Control rozszerza kontrolkę DropDownList, tak aby zmiany w jednej DropDownList ładowały skojarzone wartości w innym DropDownList.</span><span class="sxs-lookup"><span data-stu-id="9436a-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="9436a-107">(Na przykład jedna lista zawiera listę stanów USA, a następna lista jest wypełniana głównymi miastami tego stanu). Pierwszym wyzwaniem do rozwiązania jest rzeczywiste wypełnienie listy rozwijanej przy użyciu tej kontrolki.</span><span class="sxs-lookup"><span data-stu-id="9436a-107">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) The first challenge to solve is to actually fill a dropdown list using this control.</span></span>

## <a name="overview"></a><span data-ttu-id="9436a-108">Omówienie</span><span class="sxs-lookup"><span data-stu-id="9436a-108">Overview</span></span>

<span data-ttu-id="9436a-109">Kontrolka kontrolki CascadingDropDown w zestawie narzędzi AJAX Control rozszerza kontrolkę DropDownList, tak aby zmiany w jednej DropDownList ładowały skojarzone wartości w innym DropDownList.</span><span class="sxs-lookup"><span data-stu-id="9436a-109">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="9436a-110">(Na przykład jedna lista zawiera listę stanów USA, a następna lista jest wypełniana głównymi miastami tego stanu). Pierwszym wyzwaniem do rozwiązania jest rzeczywiste wypełnienie listy rozwijanej przy użyciu tej kontrolki.</span><span class="sxs-lookup"><span data-stu-id="9436a-110">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) The first challenge to solve is to actually fill a dropdown list using this control.</span></span>

## <a name="steps"></a><span data-ttu-id="9436a-111">Kroki</span><span class="sxs-lookup"><span data-stu-id="9436a-111">Steps</span></span>

<span data-ttu-id="9436a-112">Aby aktywować funkcje ASP.NET AJAX i zestawu narzędzi sterowania, formant `ScriptManager` musi być umieszczony w dowolnym miejscu na stronie (ale w obrębie elementu `<form>`):</span><span class="sxs-lookup"><span data-stu-id="9436a-112">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample1.aspx)]

<span data-ttu-id="9436a-113">Następnie wymagany jest formant DropDownList:</span><span class="sxs-lookup"><span data-stu-id="9436a-113">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample2.aspx)]

<span data-ttu-id="9436a-114">Dla tej listy zostanie dodany kontrolki CascadingDropDown.</span><span class="sxs-lookup"><span data-stu-id="9436a-114">For this list, a CascadingDropDown extender is added.</span></span> <span data-ttu-id="9436a-115">Wyśle żądanie asynchroniczne do usługi sieci Web, która następnie zwróci listę wpisów do wyświetlenia na liście.</span><span class="sxs-lookup"><span data-stu-id="9436a-115">It will send an asynchronous request to a web service which will then return a list of entries to be displayed in the list.</span></span> <span data-ttu-id="9436a-116">Aby to działało, należy ustawić następujące atrybuty kontrolki CascadingDropDown:</span><span class="sxs-lookup"><span data-stu-id="9436a-116">For this to work, the following CascadingDropDown attributes need to be set:</span></span>

- <span data-ttu-id="9436a-117">`ServicePath`: adres URL usługi sieci Web dostarczającej wpisy listy</span><span class="sxs-lookup"><span data-stu-id="9436a-117">`ServicePath`: URL of a web service delivering the list entries</span></span>
- <span data-ttu-id="9436a-118">`ServiceMethod`: Metoda sieci Web dostarczająca wpisy listy</span><span class="sxs-lookup"><span data-stu-id="9436a-118">`ServiceMethod`: Web method delivering the list entries</span></span>
- <span data-ttu-id="9436a-119">`TargetControlID`: Identyfikator listy rozwijanej</span><span class="sxs-lookup"><span data-stu-id="9436a-119">`TargetControlID`: ID of the dropdown list</span></span>
- <span data-ttu-id="9436a-120">`Category`: informacje o kategorii, które są przesyłane do metody sieci Web w przypadku wywołania</span><span class="sxs-lookup"><span data-stu-id="9436a-120">`Category`: Category information that is submitted to the web method when called</span></span>
- <span data-ttu-id="9436a-121">`PromptText`: tekst wyświetlany podczas asynchronicznego ładowania danych listy z serwera</span><span class="sxs-lookup"><span data-stu-id="9436a-121">`PromptText`: Text displayed when asynchronously loading list data from the server</span></span>

<span data-ttu-id="9436a-122">Poniżej znajduje się znacznik elementu `CascadingDropDown`.</span><span class="sxs-lookup"><span data-stu-id="9436a-122">Here is the markup for the `CascadingDropDown` element.</span></span> <span data-ttu-id="9436a-123">Jedyną różnicą między C# i VB jest nazwa skojarzonej usługi sieci Web:</span><span class="sxs-lookup"><span data-stu-id="9436a-123">The only difference between C# and VB is the name of the associated web service:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample3.aspx)]

<span data-ttu-id="9436a-124">Kod JavaScript pochodzący z rozszerzenia `CascadingDropDown` rozszerza wywołanie metody usługi sieci Web z następującym podpisem:</span><span class="sxs-lookup"><span data-stu-id="9436a-124">The JavaScript code coming from the `CascadingDropDown` extender calls a web service method with the following signature:</span></span>

[!code-vb[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample4.vb)]

<span data-ttu-id="9436a-125">Dlatego ważne jest, aby Metoda zwracała tablicę typu `CascadingDropDownNameValue` (zdefiniowana przez zestaw narzędzi ASP.NET AJAX Control Toolkit).</span><span class="sxs-lookup"><span data-stu-id="9436a-125">So the important aspect is that the method needs to return an array of type `CascadingDropDownNameValue` (defined by the ASP.NET AJAX Control Toolkit).</span></span> <span data-ttu-id="9436a-126">W konstruktorze `CascadingDropDownNameValue` najpierw tekst wpisu listy, a następnie należy podać jego wartość, podobnie jak w przypadku `<option value="VALUE">NAME</option>` w kodzie HTML.</span><span class="sxs-lookup"><span data-stu-id="9436a-126">In the `CascadingDropDownNameValue` constructor, first the list entry's text and then its value must be provided, just as `<option value="VALUE">NAME</option>` would do in HTML.</span></span> <span data-ttu-id="9436a-127">Oto kilka przykładowych danych:</span><span class="sxs-lookup"><span data-stu-id="9436a-127">Here is some sample data:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample5.aspx)]

<span data-ttu-id="9436a-128">Załadowanie strony w przeglądarce spowoduje wyzwolenie listy dla trzech dostawców.</span><span class="sxs-lookup"><span data-stu-id="9436a-128">Loading the page in the browser will trigger the list to be filled with three vendors.</span></span>

<span data-ttu-id="9436a-129">[![lista jest wypełniana automatycznie](filling-a-list-using-cascadingdropdown-vb/_static/image2.png)](filling-a-list-using-cascadingdropdown-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="9436a-129">[![The list is filled automatically](filling-a-list-using-cascadingdropdown-vb/_static/image2.png)](filling-a-list-using-cascadingdropdown-vb/_static/image1.png)</span></span>

<span data-ttu-id="9436a-130">Lista jest wypełniana automatycznie ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](filling-a-list-using-cascadingdropdown-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="9436a-130">The list is filled automatically ([Click to view full-size image](filling-a-list-using-cascadingdropdown-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9436a-131">[Poprzednie](using-auto-postback-with-cascadingdropdown-cs.md)
> [dalej](using-cascadingdropdown-with-a-database-vb.md)</span><span class="sxs-lookup"><span data-stu-id="9436a-131">[Previous](using-auto-postback-with-cascadingdropdown-cs.md)
[Next](using-cascadingdropdown-with-a-database-vb.md)</span></span>

---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-cs
title: Wypełnianie listy przy użyciu kontrolki CascadingDropDown (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolki CascadingDropDown w zestawu narzędzi AJAX Control Toolkit rozszerza formant DropDownList, tak aby zmiany w jednej metody DropDownList ładowania skojarzone wartości w anoth...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: f949aafa-fe57-43b0-b722-f0dd33a900be
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: a9a3bf12b721c8f5eec21f3090142e40e74b0b9c
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/09/2019
ms.locfileid: "59395649"
---
# <a name="filling-a-list-using-cascadingdropdown-c"></a><span data-ttu-id="4a30a-103">Wypełnianie listy przy użyciu kontrolki CascadingDropDown (C#)</span><span class="sxs-lookup"><span data-stu-id="4a30a-103">Filling a List Using CascadingDropDown (C#)</span></span>

<span data-ttu-id="4a30a-104">przez [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="4a30a-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="4a30a-105">[Pobierz program Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.cs.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="4a30a-105">[Download Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0CS.pdf)</span></span>

> <span data-ttu-id="4a30a-106">Kontrolki CascadingDropDown w zestawu narzędzi AJAX Control Toolkit rozszerza formant DropDownList tak, aby zmiany w jednej metody DropDownList ładowania skojarzone wartości w innej metody DropDownList.</span><span class="sxs-lookup"><span data-stu-id="4a30a-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="4a30a-107">(Na przykład jedna lista zawiera listę stanów USA oraz następnej liście następnie jest wypełniany głównych miast, w tym stanie.) Pierwszy wyzwaniem wymagającym rozwiązania jest faktycznie wypełnienia listy rozwijanej, za pomocą tego formantu.</span><span class="sxs-lookup"><span data-stu-id="4a30a-107">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) The first challenge to solve is to actually fill a dropdown list using this control.</span></span>


## <a name="overview"></a><span data-ttu-id="4a30a-108">Omówienie</span><span class="sxs-lookup"><span data-stu-id="4a30a-108">Overview</span></span>

<span data-ttu-id="4a30a-109">Kontrolki CascadingDropDown w zestawu narzędzi AJAX Control Toolkit rozszerza formant DropDownList tak, aby zmiany w jednej metody DropDownList ładowania skojarzone wartości w innej metody DropDownList.</span><span class="sxs-lookup"><span data-stu-id="4a30a-109">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="4a30a-110">(Na przykład jedna lista zawiera listę stanów USA oraz następnej liście następnie jest wypełniany głównych miast, w tym stanie.) Pierwszy wyzwaniem wymagającym rozwiązania jest faktycznie wypełnienia listy rozwijanej, za pomocą tego formantu.</span><span class="sxs-lookup"><span data-stu-id="4a30a-110">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) The first challenge to solve is to actually fill a dropdown list using this control.</span></span>

## <a name="steps"></a><span data-ttu-id="4a30a-111">Kroki</span><span class="sxs-lookup"><span data-stu-id="4a30a-111">Steps</span></span>

<span data-ttu-id="4a30a-112">W celu włączenia funkcji ASP.NET AJAX i zestaw narzędzi do sterowania `ScriptManager` kontroli muszą znajdować się gdziekolwiek na stronie (ale poziomu `<form>` elementu):</span><span class="sxs-lookup"><span data-stu-id="4a30a-112">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample1.aspx)]

<span data-ttu-id="4a30a-113">Następnie formant DropDownList jest wymagane:</span><span class="sxs-lookup"><span data-stu-id="4a30a-113">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample2.aspx)]

<span data-ttu-id="4a30a-114">Dla tej listy rozszerzeń kontrolki CascadingDropDown zostanie dodany.</span><span class="sxs-lookup"><span data-stu-id="4a30a-114">For this list, a CascadingDropDown extender is added.</span></span> <span data-ttu-id="4a30a-115">Asynchroniczne żądanie zostanie wysłane do usługi sieci web, które są następnie przekazywane ponownie listę wpisów do wyświetlenia na liście.</span><span class="sxs-lookup"><span data-stu-id="4a30a-115">It will send an asynchronous request to a web service which will then return a list of entries to be displayed in the list.</span></span> <span data-ttu-id="4a30a-116">Aby to zrobić, należy skonfigurować następujące atrybuty kontrolki CascadingDropDown:</span><span class="sxs-lookup"><span data-stu-id="4a30a-116">For this to work, the following CascadingDropDown attributes need to be set:</span></span>

- `ServicePath`<span data-ttu-id="4a30a-117">: Adres URL usługi sieci web, zapewniając pozycji na liście</span><span class="sxs-lookup"><span data-stu-id="4a30a-117">: URL of a web service delivering the list entries</span></span>
- `ServiceMethod`<span data-ttu-id="4a30a-118">: Metoda sieci Web, zapewniając pozycji na liście</span><span class="sxs-lookup"><span data-stu-id="4a30a-118">: Web method delivering the list entries</span></span>
- `TargetControlID`<span data-ttu-id="4a30a-119">: Identyfikator listy rozwijanej</span><span class="sxs-lookup"><span data-stu-id="4a30a-119">: ID of the dropdown list</span></span>
- `Category`<span data-ttu-id="4a30a-120">: Informacje o kategorii, przesłane do metody sieci web, gdy zostanie wywołana</span><span class="sxs-lookup"><span data-stu-id="4a30a-120">: Category information that is submitted to the web method when called</span></span>
- `PromptText`<span data-ttu-id="4a30a-121">: Tekst wyświetlany, gdy asynchronicznie ładowania listy danych z serwera</span><span class="sxs-lookup"><span data-stu-id="4a30a-121">: Text displayed when asynchronously loading list data from the server</span></span>

<span data-ttu-id="4a30a-122">Oto znaczniki dla `CascadingDropDown` elementu.</span><span class="sxs-lookup"><span data-stu-id="4a30a-122">Here is the markup for the `CascadingDropDown` element.</span></span> <span data-ttu-id="4a30a-123">Jedyną różnicą między C# i VB jest nazwą usługi skojarzone sieci web:</span><span class="sxs-lookup"><span data-stu-id="4a30a-123">The only difference between C# and VB is the name of the associated web service:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample3.aspx)]

<span data-ttu-id="4a30a-124">Kod JavaScript pochodzący z `CascadingDropDown` extender wywołań metody usługi sieci web z następującą sygnaturą:</span><span class="sxs-lookup"><span data-stu-id="4a30a-124">The JavaScript code coming from the `CascadingDropDown` extender calls a web service method with the following signature:</span></span>

[!code-csharp[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample4.cs)]

<span data-ttu-id="4a30a-125">Dlatego metoda musi zwracać tablicę typu ważnym aspektem `CascadingDropDownNameValue` (zdefiniowanej przez program ASP.NET AJAX Control Toolkit).</span><span class="sxs-lookup"><span data-stu-id="4a30a-125">So the important aspect is that the method needs to return an array of type `CascadingDropDownNameValue` (defined by the ASP.NET AJAX Control Toolkit).</span></span> <span data-ttu-id="4a30a-126">W `CascadingDropDownNameValue` konstruktora, pierwszy tekst wpisu listy, a następnie jego wartość musi być podana, podobnie jak `<option value="VALUE">NAME</option>` sposób analogiczny jak w formacie HTML.</span><span class="sxs-lookup"><span data-stu-id="4a30a-126">In the `CascadingDropDownNameValue` constructor, first the list entry's text and then its value must be provided, just as `<option value="VALUE">NAME</option>` would do in HTML.</span></span> <span data-ttu-id="4a30a-127">Poniżej przedstawiono przykładowe dane:</span><span class="sxs-lookup"><span data-stu-id="4a30a-127">Here is some sample data:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample5.aspx)]

<span data-ttu-id="4a30a-128">Ładowanie strony w przeglądarce wyzwoli listy ma zostać wypełniony przy użyciu trzech dostawców.</span><span class="sxs-lookup"><span data-stu-id="4a30a-128">Loading the page in the browser will trigger the list to be filled with three vendors.</span></span>


[![T<span data-ttu-id="4a30a-129">Lista he jest wypełniane automatycznie]</span><span class="sxs-lookup"><span data-stu-id="4a30a-129">he list is filled automatically]</span></span>(filling-a-list-using-cascadingdropdown-cs/_static/image2.png)](filling-a-list-using-cascadingdropdown-cs/_static/image1.png)

<span data-ttu-id="4a30a-130">Lista jest wypełniana automatycznie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](filling-a-list-using-cascadingdropdown-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="4a30a-130">The list is filled automatically ([Click to view full-size image](filling-a-list-using-cascadingdropdown-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="4a30a-131">Next</span><span class="sxs-lookup"><span data-stu-id="4a30a-131">Next</span></span>](using-cascadingdropdown-with-a-database-cs.md)

---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-vb
title: Wypełnianie listy przy użyciu kontrolki CascadingDropDown (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolki CascadingDropDown w zestawu narzędzi AJAX Control Toolkit rozszerza formant DropDownList, tak aby zmiany w jednej metody DropDownList ładowania skojarzone wartości w anoth...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 5236695e-5c70-4887-baee-0bfb0afb3448
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: b4bbe5c120a6f17a7dca08e6fc855018a2e23797
ms.sourcegitcommit: 289e051cc8a90e8f7127e239fda73047bde4de12
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/25/2019
ms.locfileid: "58421963"
---
<a name="filling-a-list-using-cascadingdropdown-vb"></a><span data-ttu-id="0a786-103">Wypełnianie listy przy użyciu kontrolki CascadingDropDown (VB)</span><span class="sxs-lookup"><span data-stu-id="0a786-103">Filling a List Using CascadingDropDown (VB)</span></span>
====================
<span data-ttu-id="0a786-104">przez [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="0a786-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="0a786-105">[Pobierz program Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.vb.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="0a786-105">[Download Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0VB.pdf)</span></span>

> <span data-ttu-id="0a786-106">Kontrolki CascadingDropDown w zestawu narzędzi AJAX Control Toolkit rozszerza formant DropDownList tak, aby zmiany w jednej metody DropDownList ładowania skojarzone wartości w innej metody DropDownList.</span><span class="sxs-lookup"><span data-stu-id="0a786-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="0a786-107">(Na przykład jedna lista zawiera listę stanów USA oraz następnej liście następnie jest wypełniany głównych miast, w tym stanie.) Pierwszy wyzwaniem wymagającym rozwiązania jest faktycznie wypełnienia listy rozwijanej, za pomocą tego formantu.</span><span class="sxs-lookup"><span data-stu-id="0a786-107">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) The first challenge to solve is to actually fill a dropdown list using this control.</span></span>


## <a name="overview"></a><span data-ttu-id="0a786-108">Omówienie</span><span class="sxs-lookup"><span data-stu-id="0a786-108">Overview</span></span>

<span data-ttu-id="0a786-109">Kontrolki CascadingDropDown w zestawu narzędzi AJAX Control Toolkit rozszerza formant DropDownList tak, aby zmiany w jednej metody DropDownList ładowania skojarzone wartości w innej metody DropDownList.</span><span class="sxs-lookup"><span data-stu-id="0a786-109">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="0a786-110">(Na przykład jedna lista zawiera listę stanów USA oraz następnej liście następnie jest wypełniany głównych miast, w tym stanie.) Pierwszy wyzwaniem wymagającym rozwiązania jest faktycznie wypełnienia listy rozwijanej, za pomocą tego formantu.</span><span class="sxs-lookup"><span data-stu-id="0a786-110">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) The first challenge to solve is to actually fill a dropdown list using this control.</span></span>

## <a name="steps"></a><span data-ttu-id="0a786-111">Kroki</span><span class="sxs-lookup"><span data-stu-id="0a786-111">Steps</span></span>

<span data-ttu-id="0a786-112">W celu włączenia funkcji ASP.NET AJAX i zestaw narzędzi do sterowania `ScriptManager` kontroli muszą znajdować się gdziekolwiek na stronie (ale poziomu `<form>` elementu):</span><span class="sxs-lookup"><span data-stu-id="0a786-112">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample1.aspx)]

<span data-ttu-id="0a786-113">Następnie formant DropDownList jest wymagane:</span><span class="sxs-lookup"><span data-stu-id="0a786-113">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample2.aspx)]

<span data-ttu-id="0a786-114">Dla tej listy rozszerzeń kontrolki CascadingDropDown zostanie dodany.</span><span class="sxs-lookup"><span data-stu-id="0a786-114">For this list, a CascadingDropDown extender is added.</span></span> <span data-ttu-id="0a786-115">Asynchroniczne żądanie zostanie wysłane do usługi sieci web, które są następnie przekazywane ponownie listę wpisów do wyświetlenia na liście.</span><span class="sxs-lookup"><span data-stu-id="0a786-115">It will send an asynchronous request to a web service which will then return a list of entries to be displayed in the list.</span></span> <span data-ttu-id="0a786-116">Aby to zrobić, należy skonfigurować następujące atrybuty kontrolki CascadingDropDown:</span><span class="sxs-lookup"><span data-stu-id="0a786-116">For this to work, the following CascadingDropDown attributes need to be set:</span></span>

- <span data-ttu-id="0a786-117">`ServicePath`: Adres URL usługi sieci web, zapewniając pozycji na liście</span><span class="sxs-lookup"><span data-stu-id="0a786-117">`ServicePath`: URL of a web service delivering the list entries</span></span>
- <span data-ttu-id="0a786-118">`ServiceMethod`: Metoda sieci Web, zapewniając pozycji na liście</span><span class="sxs-lookup"><span data-stu-id="0a786-118">`ServiceMethod`: Web method delivering the list entries</span></span>
- <span data-ttu-id="0a786-119">`TargetControlID`: Identyfikator listy rozwijanej</span><span class="sxs-lookup"><span data-stu-id="0a786-119">`TargetControlID`: ID of the dropdown list</span></span>
- <span data-ttu-id="0a786-120">`Category`: Informacje o kategorii, przesłane do metody sieci web, gdy zostanie wywołana</span><span class="sxs-lookup"><span data-stu-id="0a786-120">`Category`: Category information that is submitted to the web method when called</span></span>
- <span data-ttu-id="0a786-121">`PromptText`: Tekst wyświetlany, gdy asynchronicznie ładowania listy danych z serwera</span><span class="sxs-lookup"><span data-stu-id="0a786-121">`PromptText`: Text displayed when asynchronously loading list data from the server</span></span>

<span data-ttu-id="0a786-122">Oto znaczniki dla `CascadingDropDown` elementu.</span><span class="sxs-lookup"><span data-stu-id="0a786-122">Here is the markup for the `CascadingDropDown` element.</span></span> <span data-ttu-id="0a786-123">Jedyną różnicą między C# i VB jest nazwą usługi skojarzone sieci web:</span><span class="sxs-lookup"><span data-stu-id="0a786-123">The only difference between C# and VB is the name of the associated web service:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample3.aspx)]

<span data-ttu-id="0a786-124">Kod JavaScript pochodzący z `CascadingDropDown` extender wywołań metody usługi sieci web z następującą sygnaturą:</span><span class="sxs-lookup"><span data-stu-id="0a786-124">The JavaScript code coming from the `CascadingDropDown` extender calls a web service method with the following signature:</span></span>

[!code-vb[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample4.vb)]

<span data-ttu-id="0a786-125">Dlatego metoda musi zwracać tablicę typu ważnym aspektem `CascadingDropDownNameValue` (zdefiniowanej przez program ASP.NET AJAX Control Toolkit).</span><span class="sxs-lookup"><span data-stu-id="0a786-125">So the important aspect is that the method needs to return an array of type `CascadingDropDownNameValue` (defined by the ASP.NET AJAX Control Toolkit).</span></span> <span data-ttu-id="0a786-126">W `CascadingDropDownNameValue` konstruktora, pierwszy tekst wpisu listy, a następnie jego wartość musi być podana, podobnie jak `<option value="VALUE">NAME</option>` sposób analogiczny jak w formacie HTML.</span><span class="sxs-lookup"><span data-stu-id="0a786-126">In the `CascadingDropDownNameValue` constructor, first the list entry's text and then its value must be provided, just as `<option value="VALUE">NAME</option>` would do in HTML.</span></span> <span data-ttu-id="0a786-127">Poniżej przedstawiono przykładowe dane:</span><span class="sxs-lookup"><span data-stu-id="0a786-127">Here is some sample data:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample5.aspx)]

<span data-ttu-id="0a786-128">Ładowanie strony w przeglądarce wyzwoli listy ma zostać wypełniony przy użyciu trzech dostawców.</span><span class="sxs-lookup"><span data-stu-id="0a786-128">Loading the page in the browser will trigger the list to be filled with three vendors.</span></span>


<span data-ttu-id="0a786-129">[![Lista jest wypełniana automatycznie](filling-a-list-using-cascadingdropdown-vb/_static/image2.png)](filling-a-list-using-cascadingdropdown-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="0a786-129">[![The list is filled automatically](filling-a-list-using-cascadingdropdown-vb/_static/image2.png)](filling-a-list-using-cascadingdropdown-vb/_static/image1.png)</span></span>

<span data-ttu-id="0a786-130">Lista jest wypełniana automatycznie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](filling-a-list-using-cascadingdropdown-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="0a786-130">The list is filled automatically ([Click to view full-size image](filling-a-list-using-cascadingdropdown-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0a786-131">[Poprzednie](using-auto-postback-with-cascadingdropdown-cs.md)
> [dalej](using-cascadingdropdown-with-a-database-vb.md)</span><span class="sxs-lookup"><span data-stu-id="0a786-131">[Previous](using-auto-postback-with-cascadingdropdown-cs.md)
[Next](using-cascadingdropdown-with-a-database-vb.md)</span></span>

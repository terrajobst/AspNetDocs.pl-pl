---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-cs
title: Korzystanie z funkcji autoogłaszania zwrotnego z kontrolki CascadingDropDown (C#) | Microsoft Docs
author: wenz
description: Kontrolka kontrolki CascadingDropDown w zestawie narzędzi AJAX Control rozszerza kontrolkę DropDownList, tak aby zmiany w jednej DropDownList ładowały skojarzone wartości w anoth...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 6755d8d9-14be-4a1d-86e5-1a6110f3dea8
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: 8bccd716814e7de544798010cecbc148ec50b5cd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78535953"
---
# <a name="using-auto-postback-with-cascadingdropdown-c"></a><span data-ttu-id="866d8-103">Używanie automatycznego ogłaszania zwrotnego za pomocą kontrolki CascadingDropDown (C#)</span><span class="sxs-lookup"><span data-stu-id="866d8-103">Using Auto-Postback with CascadingDropDown (C#)</span></span>

<span data-ttu-id="866d8-104">Autor [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="866d8-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="866d8-105">[Pobierz kod](https://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.cs.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="866d8-105">[Download Code](https://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.cs.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3CS.pdf)</span></span>

> <span data-ttu-id="866d8-106">Kontrolka kontrolki CascadingDropDown w zestawie narzędzi AJAX Control rozszerza kontrolkę DropDownList, tak aby zmiany w jednej DropDownList ładowały skojarzone wartości w innym DropDownList.</span><span class="sxs-lookup"><span data-stu-id="866d8-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="866d8-107">Jednak w przypadku korzystania z formantu kontrolki CascadingDropDown, ASP. Funkcja autoogłaszania DropDownList w sieci nie działa, ponieważ asynchroniczne ładowanie danych do listy generuje (niepotrzebne) ogłaszanie zwrotne.</span><span class="sxs-lookup"><span data-stu-id="866d8-107">However when using the CascadingDropDown control, ASP.NET's DropDownList control's AutoPostBack feature does not work, since asynchronously loading data into the list generates an (unnecessary) postback itself.</span></span> <span data-ttu-id="866d8-108">W przypadku niektórych kodów JavaScript ten efekt można uniknąć.</span><span class="sxs-lookup"><span data-stu-id="866d8-108">With some JavaScript code, this effect can be avoided.</span></span>

## <a name="overview"></a><span data-ttu-id="866d8-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="866d8-109">Overview</span></span>

<span data-ttu-id="866d8-110">Kontrolka kontrolki CascadingDropDown w zestawie narzędzi AJAX Control rozszerza kontrolkę DropDownList, tak aby zmiany w jednej DropDownList ładowały skojarzone wartości w innym DropDownList.</span><span class="sxs-lookup"><span data-stu-id="866d8-110">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="866d8-111">(Na przykład jedna lista zawiera listę stanów USA, a następna lista jest wypełniana głównymi miastami tego stanu). Jednak w przypadku korzystania z formantu kontrolki CascadingDropDown, ASP. Funkcja autoogłaszania DropDownList w sieci nie działa, ponieważ asynchroniczne ładowanie danych do listy generuje (niepotrzebne) ogłaszanie zwrotne.</span><span class="sxs-lookup"><span data-stu-id="866d8-111">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) However when using the CascadingDropDown control, ASP.NET's DropDownList control's AutoPostBack feature does not work, since asynchronously loading data into the list generates an (unnecessary) postback itself.</span></span> <span data-ttu-id="866d8-112">W przypadku niektórych kodów JavaScript ten efekt można uniknąć.</span><span class="sxs-lookup"><span data-stu-id="866d8-112">With some JavaScript code, this effect can be avoided.</span></span>

## <a name="steps"></a><span data-ttu-id="866d8-113">Kroki</span><span class="sxs-lookup"><span data-stu-id="866d8-113">Steps</span></span>

<span data-ttu-id="866d8-114">Aby uaktywnić funkcje ASP.NET AJAX i zestaw narzędzi sterowania, formant `ScriptManager` musi być umieszczony w dowolnym miejscu na stronie (ale w &lt;`form`&gt; elementu):</span><span class="sxs-lookup"><span data-stu-id="866d8-114">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the &lt;`form`&gt; element):</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample1.aspx)]

<span data-ttu-id="866d8-115">Następnie wymagany jest formant DropDownList:</span><span class="sxs-lookup"><span data-stu-id="866d8-115">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample2.aspx)]

<span data-ttu-id="866d8-116">Dla tej listy dodano rozszerzenie kontrolki CascadingDropDown, podając adres URL i informacje o metodzie usługi sieci Web:</span><span class="sxs-lookup"><span data-stu-id="866d8-116">For this list, a CascadingDropDown extender is added, providing web service URL and method information:</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample3.aspx)]

<span data-ttu-id="866d8-117">Rozszerzenie kontrolki CascadingDropDown następnie asynchronicznie wywołuje usługę sieci Web przy użyciu następującej sygnatury metody:</span><span class="sxs-lookup"><span data-stu-id="866d8-117">The CascadingDropDown extender then asynchronously calls a web service with the following method signature:</span></span>

[!code-csharp[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample4.cs)]

<span data-ttu-id="866d8-118">Metoda zwraca tablicę typu kontrolki CascadingDropDown wartość.</span><span class="sxs-lookup"><span data-stu-id="866d8-118">The method returns an array of type CascadingDropDown value.</span></span> <span data-ttu-id="866d8-119">Konstruktor typu oczekuje najpierw podpisu wpisu listy, a następnie wartość (HTML `value` atrybutu).</span><span class="sxs-lookup"><span data-stu-id="866d8-119">The type's constructor expects first the list entry's caption and then the value (HTML `value` attribute).</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample5.aspx)]

<span data-ttu-id="866d8-120">Załadowanie strony w przeglądarce spowoduje wypełnienie listy rozwijanej trzema dostawcami, a drugi jest wybierany w pierwszej postaci.</span><span class="sxs-lookup"><span data-stu-id="866d8-120">Loading the page in the browser will fill the dropdown list with three vendors, the second one being preselected.</span></span> <span data-ttu-id="866d8-121">Ponadto ASP.NET definiuje metodę `__doPostBack()` JavaScript.</span><span class="sxs-lookup"><span data-stu-id="866d8-121">Also, ASP.NET defines the `__doPostBack()` JavaScript method.</span></span> <span data-ttu-id="866d8-122">Po załadowaniu strony to wywołanie języka JavaScript jest dodawane do listy rozwijanej, ale tylko wtedy, gdy znajdują się w niej elementy.</span><span class="sxs-lookup"><span data-stu-id="866d8-122">Once the page has been loaded, this JavaScript call is added to the dropdown list, but only if there are elements in it.</span></span> <span data-ttu-id="866d8-123">Jeśli na liście nie ma elementów, zestaw narzędzi do sterowania jest obecnie ładowany, więc kod JavaScript używa limitu czasu i próbuje ponownie za pół sekundy.</span><span class="sxs-lookup"><span data-stu-id="866d8-123">If there are no elements in the list, the Control Toolkit is currently loading them, so the JavaScript code uses a timeout and tries again in a half second.</span></span>

[!code-html[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample6.html)]

<span data-ttu-id="866d8-124">W ten sposób ogłaszanie zwrotne jest wykonywane tylko wtedy, gdy na liście znajdują się elementy, a użytkownik wybierze wpis.</span><span class="sxs-lookup"><span data-stu-id="866d8-124">This way, a postback is only executed when there are actually elements in the list and the user selects an entry.</span></span>

<span data-ttu-id="866d8-125">[![wybranie elementu listy spowoduje odświeżenie](using-auto-postback-with-cascadingdropdown-cs/_static/image2.png)](using-auto-postback-with-cascadingdropdown-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="866d8-125">[![Selecting a list element causes a postback](using-auto-postback-with-cascadingdropdown-cs/_static/image2.png)](using-auto-postback-with-cascadingdropdown-cs/_static/image1.png)</span></span>

<span data-ttu-id="866d8-126">Wybranie elementu listy powoduje ogłoszenie zwrotne ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-auto-postback-with-cascadingdropdown-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="866d8-126">Selecting a list element causes a postback ([Click to view full-size image](using-auto-postback-with-cascadingdropdown-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="866d8-127">[Poprzednie](presetting-list-entries-with-cascadingdropdown-cs.md)
> [dalej](filling-a-list-using-cascadingdropdown-vb.md)</span><span class="sxs-lookup"><span data-stu-id="866d8-127">[Previous](presetting-list-entries-with-cascadingdropdown-cs.md)
[Next](filling-a-list-using-cascadingdropdown-vb.md)</span></span>

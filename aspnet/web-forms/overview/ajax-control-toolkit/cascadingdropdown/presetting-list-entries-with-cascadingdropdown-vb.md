---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-vb
title: Ustawianie wpisów listy z kontrolki CascadingDropDown (VB) | Microsoft Docs
author: wenz
description: Kontrolka kontrolki CascadingDropDown w zestawie narzędzi AJAX Control rozszerza kontrolkę DropDownList, tak aby zmiany w jednej DropDownList ładowały skojarzone wartości w anoth...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: ec61ced7-bbca-4bdd-aa3b-80878f295181
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: 58d675993777f9dcbe0ce1890a60046c91ee8907
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599546"
---
# <a name="presetting-list-entries-with-cascadingdropdown-vb"></a><span data-ttu-id="cccfd-103">Wstępne ustawianie pozycji listy przy użyciu kontrolki CascadingDropDown (VB)</span><span class="sxs-lookup"><span data-stu-id="cccfd-103">Presetting List Entries with CascadingDropDown (VB)</span></span>

<span data-ttu-id="cccfd-104">Autor [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="cccfd-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="cccfd-105">[Pobierz kod](https://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown2.vb.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/CascadingDropDown2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="cccfd-105">[Download Code](https://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown2.vb.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/CascadingDropDown2VB.pdf)</span></span>

> <span data-ttu-id="cccfd-106">Kontrolka kontrolki CascadingDropDown w zestawie narzędzi AJAX Control rozszerza kontrolkę DropDownList, tak aby zmiany w jednej DropDownList ładowały skojarzone wartości w innym DropDownList.</span><span class="sxs-lookup"><span data-stu-id="cccfd-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="cccfd-107">W przypadku niewielkiej ilości kodu jest możliwe, że element listy jest wybierany po załadowaniu danych dynamicznie.</span><span class="sxs-lookup"><span data-stu-id="cccfd-107">With a little bit of code it is possible that a list element is preselected once the data has been dynamically loaded.</span></span>

## <a name="overview"></a><span data-ttu-id="cccfd-108">Omówienie</span><span class="sxs-lookup"><span data-stu-id="cccfd-108">Overview</span></span>

<span data-ttu-id="cccfd-109">Kontrolka kontrolki CascadingDropDown w zestawie narzędzi AJAX Control rozszerza kontrolkę DropDownList, tak aby zmiany w jednej DropDownList ładowały skojarzone wartości w innym DropDownList.</span><span class="sxs-lookup"><span data-stu-id="cccfd-109">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="cccfd-110">(Na przykład jedna lista zawiera listę stanów USA, a następna lista jest wypełniana głównymi miastami tego stanu). W przypadku niewielkiej ilości kodu jest możliwe, że element listy jest wybierany po załadowaniu danych dynamicznie.</span><span class="sxs-lookup"><span data-stu-id="cccfd-110">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) With a little bit of code it is possible that a list element is preselected once the data has been dynamically loaded.</span></span>

## <a name="steps"></a><span data-ttu-id="cccfd-111">Kroki</span><span class="sxs-lookup"><span data-stu-id="cccfd-111">Steps</span></span>

<span data-ttu-id="cccfd-112">Aby aktywować funkcje ASP.NET AJAX i zestawu narzędzi sterowania, formant `ScriptManager` musi być umieszczony w dowolnym miejscu na stronie (ale w obrębie elementu `<form>`):</span><span class="sxs-lookup"><span data-stu-id="cccfd-112">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample1.aspx)]

<span data-ttu-id="cccfd-113">Następnie wymagany jest formant DropDownList:</span><span class="sxs-lookup"><span data-stu-id="cccfd-113">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample2.aspx)]

<span data-ttu-id="cccfd-114">Dla tej listy dodano rozszerzenie kontrolki CascadingDropDown, podając adres URL i informacje o metodzie usługi sieci Web:</span><span class="sxs-lookup"><span data-stu-id="cccfd-114">For this list, a CascadingDropDown extender is added, providing web service URL and method information:</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample3.aspx)]

<span data-ttu-id="cccfd-115">Rozszerzenie kontrolki CascadingDropDown następnie asynchronicznie wywołuje usługę sieci Web przy użyciu następującej sygnatury metody:</span><span class="sxs-lookup"><span data-stu-id="cccfd-115">The CascadingDropDown extender then asynchronously calls a web service with the following method signature:</span></span>

[!code-vb[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample4.vb)]

<span data-ttu-id="cccfd-116">Metoda zwraca tablicę typu kontrolki CascadingDropDown wartość.</span><span class="sxs-lookup"><span data-stu-id="cccfd-116">The method returns an array of type CascadingDropDown value.</span></span> <span data-ttu-id="cccfd-117">Konstruktor typu oczekuje najpierw podpisu wpisu listy, a następnie wartość (HTML `value` atrybutu).</span><span class="sxs-lookup"><span data-stu-id="cccfd-117">The type's constructor expects first the list entry's caption and then the value (HTML `value` attribute).</span></span> <span data-ttu-id="cccfd-118">Jeśli trzeci argument ma wartość true, element list zostanie automatycznie wybrany w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="cccfd-118">If the third argument is set to true, the list element is automatically selected in the browser.</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample5.aspx)]

<span data-ttu-id="cccfd-119">Załadowanie strony w przeglądarce spowoduje wypełnienie listy rozwijanej trzema dostawcami, a drugi jest wybierany w pierwszej postaci.</span><span class="sxs-lookup"><span data-stu-id="cccfd-119">Loading the page in the browser will fill the dropdown list with three vendors, the second one being preselected.</span></span>

<span data-ttu-id="cccfd-120">[![lista jest wypełniana i wybierana automatycznie](presetting-list-entries-with-cascadingdropdown-vb/_static/image2.png)](presetting-list-entries-with-cascadingdropdown-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="cccfd-120">[![The list is filled and preselected automatically](presetting-list-entries-with-cascadingdropdown-vb/_static/image2.png)](presetting-list-entries-with-cascadingdropdown-vb/_static/image1.png)</span></span>

<span data-ttu-id="cccfd-121">Lista jest wypełniana i wybierana automatycznie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](presetting-list-entries-with-cascadingdropdown-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="cccfd-121">The list is filled and preselected automatically ([Click to view full-size image](presetting-list-entries-with-cascadingdropdown-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="cccfd-122">[Poprzednie](using-cascadingdropdown-with-a-database-vb.md)
> [dalej](using-auto-postback-with-cascadingdropdown-vb.md)</span><span class="sxs-lookup"><span data-stu-id="cccfd-122">[Previous](using-cascadingdropdown-with-a-database-vb.md)
[Next](using-auto-postback-with-cascadingdropdown-vb.md)</span></span>

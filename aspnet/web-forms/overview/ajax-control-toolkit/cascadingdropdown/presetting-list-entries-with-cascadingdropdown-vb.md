---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-vb
title: Wstępne Ustawianie pozycji listy przy użyciu kontrolki CascadingDropDown (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolki CascadingDropDown w zestawu narzędzi AJAX Control Toolkit rozszerza formant DropDownList, tak aby zmiany w jednej metody DropDownList ładowania skojarzone wartości w anoth...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: ec61ced7-bbca-4bdd-aa3b-80878f295181
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: b943569f0f10d7f680954e100297973255aab51e
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65128747"
---
# <a name="presetting-list-entries-with-cascadingdropdown-vb"></a><span data-ttu-id="afcf4-103">Wstępne ustawianie pozycji listy przy użyciu kontrolki CascadingDropDown (VB)</span><span class="sxs-lookup"><span data-stu-id="afcf4-103">Presetting List Entries with CascadingDropDown (VB)</span></span>

<span data-ttu-id="afcf4-104">przez [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="afcf4-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="afcf4-105">[Pobierz program Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown2.vb.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/CascadingDropDown2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="afcf4-105">[Download Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown2.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/CascadingDropDown2VB.pdf)</span></span>

> <span data-ttu-id="afcf4-106">Kontrolki CascadingDropDown w zestawu narzędzi AJAX Control Toolkit rozszerza formant DropDownList tak, aby zmiany w jednej metody DropDownList ładowania skojarzone wartości w innej metody DropDownList.</span><span class="sxs-lookup"><span data-stu-id="afcf4-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="afcf4-107">Za pomocą niewielki kod jest możliwe, że element listy wyświetlana jest wartość po dynamicznie załadowane dane.</span><span class="sxs-lookup"><span data-stu-id="afcf4-107">With a little bit of code it is possible that a list element is preselected once the data has been dynamically loaded.</span></span>

## <a name="overview"></a><span data-ttu-id="afcf4-108">Omówienie</span><span class="sxs-lookup"><span data-stu-id="afcf4-108">Overview</span></span>

<span data-ttu-id="afcf4-109">Kontrolki CascadingDropDown w zestawu narzędzi AJAX Control Toolkit rozszerza formant DropDownList tak, aby zmiany w jednej metody DropDownList ładowania skojarzone wartości w innej metody DropDownList.</span><span class="sxs-lookup"><span data-stu-id="afcf4-109">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="afcf4-110">(Na przykład jedna lista zawiera listę stanów USA oraz następnej liście następnie jest wypełniany głównych miast, w tym stanie.) Za pomocą niewielki kod jest możliwe, że element listy wyświetlana jest wartość po dynamicznie załadowane dane.</span><span class="sxs-lookup"><span data-stu-id="afcf4-110">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) With a little bit of code it is possible that a list element is preselected once the data has been dynamically loaded.</span></span>

## <a name="steps"></a><span data-ttu-id="afcf4-111">Kroki</span><span class="sxs-lookup"><span data-stu-id="afcf4-111">Steps</span></span>

<span data-ttu-id="afcf4-112">W celu włączenia funkcji ASP.NET AJAX i zestaw narzędzi do sterowania `ScriptManager` kontroli muszą znajdować się gdziekolwiek na stronie (ale poziomu `<form>` elementu):</span><span class="sxs-lookup"><span data-stu-id="afcf4-112">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample1.aspx)]

<span data-ttu-id="afcf4-113">Następnie formant DropDownList jest wymagane:</span><span class="sxs-lookup"><span data-stu-id="afcf4-113">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample2.aspx)]

<span data-ttu-id="afcf4-114">Dla tej listy rozszerzeń kontrolki CascadingDropDown jest dodawany, dostarczania informacji adresu URL i metody usługi sieci web:</span><span class="sxs-lookup"><span data-stu-id="afcf4-114">For this list, a CascadingDropDown extender is added, providing web service URL and method information:</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample3.aspx)]

<span data-ttu-id="afcf4-115">Rozszerzenie kontrolki CascadingDropDown asynchronicznie wywołuje usługę sieci web za pomocą następujący podpis metody:</span><span class="sxs-lookup"><span data-stu-id="afcf4-115">The CascadingDropDown extender then asynchronously calls a web service with the following method signature:</span></span>

[!code-vb[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample4.vb)]

<span data-ttu-id="afcf4-116">Metoda zwraca tablicę typu kontrolki CascadingDropDown wartość.</span><span class="sxs-lookup"><span data-stu-id="afcf4-116">The method returns an array of type CascadingDropDown value.</span></span> <span data-ttu-id="afcf4-117">Konstruktor typu oczekuje, że najpierw podpis wpisu listy, a następnie wartość (HTML `value` atrybutu).</span><span class="sxs-lookup"><span data-stu-id="afcf4-117">The type's constructor expects first the list entry's caption and then the value (HTML `value` attribute).</span></span> <span data-ttu-id="afcf4-118">Jeśli trzeci argument jest ustawiona na wartość true, na liście elementu jest automatycznie wybierany w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="afcf4-118">If the third argument is set to true, the list element is automatically selected in the browser.</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample5.aspx)]

<span data-ttu-id="afcf4-119">Ładowanie strony w przeglądarce spowoduje wypełnienie listy rozwijanej przy użyciu trzech dostawców drugi są wstępnie wybrane.</span><span class="sxs-lookup"><span data-stu-id="afcf4-119">Loading the page in the browser will fill the dropdown list with three vendors, the second one being preselected.</span></span>

<span data-ttu-id="afcf4-120">[![Lista jest wypełniana i wstępnie wybrane automatycznie](presetting-list-entries-with-cascadingdropdown-vb/_static/image2.png)](presetting-list-entries-with-cascadingdropdown-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="afcf4-120">[![The list is filled and preselected automatically](presetting-list-entries-with-cascadingdropdown-vb/_static/image2.png)](presetting-list-entries-with-cascadingdropdown-vb/_static/image1.png)</span></span>

<span data-ttu-id="afcf4-121">Lista jest wypełniana i wstępnie wybrane automatycznie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](presetting-list-entries-with-cascadingdropdown-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="afcf4-121">The list is filled and preselected automatically ([Click to view full-size image](presetting-list-entries-with-cascadingdropdown-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="afcf4-122">[Poprzednie](using-cascadingdropdown-with-a-database-vb.md)
> [dalej](using-auto-postback-with-cascadingdropdown-vb.md)</span><span class="sxs-lookup"><span data-stu-id="afcf4-122">[Previous](using-cascadingdropdown-with-a-database-vb.md)
[Next](using-auto-postback-with-cascadingdropdown-vb.md)</span></span>

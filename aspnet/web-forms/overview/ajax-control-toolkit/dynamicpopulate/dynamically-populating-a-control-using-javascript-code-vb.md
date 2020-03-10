---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-vb
title: Dynamiczne wypełnianie kontrolki przy użyciu kodu JavaScript (VB) | Microsoft Docs
author: wenz
description: Kontrolka używanie dynamicpopulate w zestawie narzędzi ASP.NET AJAX Control wywołuje usługę sieci Web (lub metodę strony) i wypełnia wartość wyniki do kontrolki docelowej na t...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 90582e54-3e90-432a-9da5-689fb39ed56b
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-vb
msc.type: authoredcontent
ms.openlocfilehash: b2bd5b1571ccebc9baa501b29743aecdb4543fb2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78613765"
---
# <a name="dynamically-populating-a-control-using-javascript-code-vb"></a><span data-ttu-id="df6a4-103">Dynamiczne wypełnianie kontrolki przy użyciu kodu JavaScript (VB)</span><span class="sxs-lookup"><span data-stu-id="df6a4-103">Dynamically Populating a Control Using JavaScript Code (VB)</span></span>

<span data-ttu-id="df6a4-104">Autor [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="df6a4-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="df6a4-105">[Pobierz kod](https://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.vb.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="df6a4-105">[Download Code](https://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.vb.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1VB.pdf)</span></span>

> <span data-ttu-id="df6a4-106">Kontrolka używanie dynamicpopulate w zestawie narzędzi ASP.NET AJAX Control wywołuje usługę sieci Web (lub metodę strony) i wypełnia wartość wyniki do kontrolki docelowej na stronie bez odświeżania strony.</span><span class="sxs-lookup"><span data-stu-id="df6a4-106">The DynamicPopulate control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="df6a4-107">Możliwe jest również wyzwolenie populacji przy użyciu niestandardowego kodu JavaScript po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="df6a4-107">It is also possible to trigger the population using custom client-side JavaScript code.</span></span>

## <a name="overview"></a><span data-ttu-id="df6a4-108">Omówienie</span><span class="sxs-lookup"><span data-stu-id="df6a4-108">Overview</span></span>

<span data-ttu-id="df6a4-109">Formant `DynamicPopulate` w zestawie narzędzi ASP.NET AJAX Control wywołuje usługę sieci Web (lub metodę strony) i wypełnia wartość wyniki do kontrolki docelowej na stronie bez odświeżania strony.</span><span class="sxs-lookup"><span data-stu-id="df6a4-109">The `DynamicPopulate` control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="df6a4-110">Możliwe jest również wyzwolenie populacji przy użyciu niestandardowego kodu JavaScript po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="df6a4-110">It is also possible to trigger the population using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="df6a4-111">Kroki</span><span class="sxs-lookup"><span data-stu-id="df6a4-111">Steps</span></span>

<span data-ttu-id="df6a4-112">Najpierw potrzebna jest usługa sieci Web ASP.NET, która implementuje metodę, która ma zostać wywołana przez formant `DynamicPopulateExtender`.</span><span class="sxs-lookup"><span data-stu-id="df6a4-112">First of all, you need an ASP.NET Web Service which implements the method to be called by the `DynamicPopulateExtender` control.</span></span> <span data-ttu-id="df6a4-113">Usługa sieci Web implementuje metodę `getDate()`, która oczekuje jednego argumentu typu ciąg o nazwie `contextKey`, ponieważ kontrolka `DynamicPopulate` wysyła jedną część informacji o kontekście do każdego wywołania usługi sieci Web.</span><span class="sxs-lookup"><span data-stu-id="df6a4-113">The web service implements the method `getDate()` that expects one argument of type string, called `contextKey`, since the `DynamicPopulate` control sends one piece of context information with each web service call.</span></span> <span data-ttu-id="df6a4-114">Oto kod (plik `DynamicPopulate.vb.asmx`), który pobiera bieżącą datę w jednym z trzech formatów:</span><span class="sxs-lookup"><span data-stu-id="df6a4-114">Here is the code (file `DynamicPopulate.vb.asmx`) which retrieves the current date in one of three formats:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample1.aspx)]

<span data-ttu-id="df6a4-115">W następnym kroku Utwórz nową witrynę ASP.NET i Rozpocznij pracę z kontrolką ScriptManager ASP.NET AJAX:</span><span class="sxs-lookup"><span data-stu-id="df6a4-115">In the next step, create a new ASP.NET site and start with the ASP.NET AJAX ScriptManager control:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample2.aspx)]

<span data-ttu-id="df6a4-116">Następnie Dodaj kontrolkę etykieta (na przykład za pomocą kontrolki HTML o tej samej nazwie lub kontrolki sieci Web `<asp:Label />`), która będzie później wyświetlać wynik wywołania usługi sieci Web.</span><span class="sxs-lookup"><span data-stu-id="df6a4-116">Then, add a label control (for instance using the HTML control of the same name, or the `<asp:Label />` web control) which will later show the result of the web service call.</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample3.aspx)]

<span data-ttu-id="df6a4-117">Następnie Uwzględnij formant `DynamicPopulateExtender` i podaj informacje o usłudze sieci Web, kontrolce docelowej, ale nie nazwę kontrolki, która wyzwala populację, którą zostanie później przeprowadzona, przy użyciu niestandardowego języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="df6a4-117">Next, include a `DynamicPopulateExtender` control and provide web service information, target control, but not the name of the control which triggers the population this will be done later on, using custom JavaScript!</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample4.aspx)]

<span data-ttu-id="df6a4-118">Teraz ze składnikiem JavaScript.</span><span class="sxs-lookup"><span data-stu-id="df6a4-118">Now to the JavaScript part.</span></span> <span data-ttu-id="df6a4-119">Funkcja `$find()` zdefiniowana przez ASP.NET AJAX Library zwraca odwołanie do obiektów po stronie serwera zestawu narzędzi ASP.NET AJAX Control Toolkit, takich jak `DynamicPopulateExtender`.</span><span class="sxs-lookup"><span data-stu-id="df6a4-119">The `$find()` function, defined by the ASP.NET AJAX library, returns a reference to server-side objects of the ASP.NET AJAX Control Toolkit such as `DynamicPopulateExtender`.</span></span> <span data-ttu-id="df6a4-120">W bieżącym pliku `$find("dpe")` zwraca odwołanie do kontrolki `DynamicPopulateExtender` na stronie.</span><span class="sxs-lookup"><span data-stu-id="df6a4-120">In the current file, `$find("dpe")` returns a reference to the one `DynamicPopulateExtender` control in the page.</span></span> <span data-ttu-id="df6a4-121">Uwidacznia metodę o nazwie `populate()`, która wyzwala proces wypełniania dynamicznego.</span><span class="sxs-lookup"><span data-stu-id="df6a4-121">It exposes a method called `populate()` which triggers the dynamic population process.</span></span> <span data-ttu-id="df6a4-122">Metoda `populate()` wymaga jednego argumentu: klucza kontekstu, który będzie używany jako argument dla metody `getDate()` sieci Web.</span><span class="sxs-lookup"><span data-stu-id="df6a4-122">The `populate()` method requires one argument: the context key which will serve as argument to the `getDate()` web method.</span></span> <span data-ttu-id="df6a4-123">Na przykład `$find("dpe").populate("format1")` zapełnił etykietę bieżącą datą w formacie dnia miesiąca.</span><span class="sxs-lookup"><span data-stu-id="df6a4-123">So for instance, `$find("dpe").populate("format1")` would populate the label with the current date in month-day-year format.</span></span>

<span data-ttu-id="df6a4-124">Aby zwiększyć elastyczność przykładu, użytkownik może teraz wybrać kilka formatów daty.</span><span class="sxs-lookup"><span data-stu-id="df6a4-124">In order to make the sample a bit more flexible, the user may now choose between several date formats.</span></span> <span data-ttu-id="df6a4-125">Dla każdej z nich zostanie wyświetlony przycisk radiowy.</span><span class="sxs-lookup"><span data-stu-id="df6a4-125">For each one of them, a radio button is displayed.</span></span> <span data-ttu-id="df6a4-126">Gdy użytkownik kliknie przycisk radiowy, kod JavaScript dynamicznie wypełnia etykietę wybranym formatem daty.</span><span class="sxs-lookup"><span data-stu-id="df6a4-126">Once the user clicks on a radio button, JavaScript code dynamically populates the label with the selected date format.</span></span> <span data-ttu-id="df6a4-127">Poniżej znajdują się następujące przyciski radiowe:</span><span class="sxs-lookup"><span data-stu-id="df6a4-127">Here are those radio buttons:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample5.aspx)]

<span data-ttu-id="df6a4-128">Należy zauważyć, że w kontekście przycisku radiowego wyrażenie JavaScript `this.value` odnosi się do wartości bieżącego przycisku, co ma na celu dokładnie te same informacje, z którymi może współdziałać `getDate()` Metoda.</span><span class="sxs-lookup"><span data-stu-id="df6a4-128">Note that within the context of a radio button, the JavaScript expression `this.value` refers to the value of the current button, which happens to be exactly the same information the `getDate()` method can work with.</span></span>

<span data-ttu-id="df6a4-129">[![kliknięcie przycisku pobiera datę z serwera w określonym formacie](dynamically-populating-a-control-using-javascript-code-vb/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="df6a4-129">[![A click on the button retrieves the date from the server, in the format specified](dynamically-populating-a-control-using-javascript-code-vb/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="df6a4-130">Kliknięcie przycisku pobiera datę z serwera w określonym formacie ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](dynamically-populating-a-control-using-javascript-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="df6a4-130">A click on the button retrieves the date from the server, in the format specified ([Click to view full-size image](dynamically-populating-a-control-using-javascript-code-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="df6a4-131">[Poprzednie](dynamically-populating-a-control-vb.md)
> [dalej](using-dynamicpopulate-with-a-user-control-and-javascript-vb.md)</span><span class="sxs-lookup"><span data-stu-id="df6a4-131">[Previous](dynamically-populating-a-control-vb.md)
[Next](using-dynamicpopulate-with-a-user-control-and-javascript-vb.md)</span></span>

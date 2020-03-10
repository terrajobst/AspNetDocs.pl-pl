---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-vb
title: Dynamiczne wypełnianie kontrolki (VB) | Microsoft Docs
author: wenz
description: Kontrolka używanie dynamicpopulate w zestawie narzędzi ASP.NET AJAX Control wywołuje usługę sieci Web (lub metodę strony) i wypełnia wartość wyniki do kontrolki docelowej na t...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 27305347-7b5d-4519-97b7-197a357e7f6e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-vb
msc.type: authoredcontent
ms.openlocfilehash: d11320f1f89bb69afe5f62751574079716124da0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78535687"
---
# <a name="dynamically-populating-a-control-vb"></a><span data-ttu-id="acfad-103">Dynamiczne wypełnianie kontrolki (VB)</span><span class="sxs-lookup"><span data-stu-id="acfad-103">Dynamically Populating a Control (VB)</span></span>

<span data-ttu-id="acfad-104">Autor [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="acfad-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="acfad-105">[Pobierz kod](https://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate0.vb.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="acfad-105">[Download Code](https://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate0.vb.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate0VB.pdf)</span></span>

> <span data-ttu-id="acfad-106">Kontrolka używanie dynamicpopulate w zestawie narzędzi ASP.NET AJAX Control wywołuje usługę sieci Web (lub metodę strony) i wypełnia wartość wyniki do kontrolki docelowej na stronie bez odświeżania strony.</span><span class="sxs-lookup"><span data-stu-id="acfad-106">The DynamicPopulate control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span>

## <a name="overview"></a><span data-ttu-id="acfad-107">Omówienie</span><span class="sxs-lookup"><span data-stu-id="acfad-107">Overview</span></span>

<span data-ttu-id="acfad-108">Formant `DynamicPopulate` w zestawie narzędzi ASP.NET AJAX Control wywołuje usługę sieci Web (lub metodę strony) i wypełnia wartość wyniki do kontrolki docelowej na stronie bez odświeżania strony.</span><span class="sxs-lookup"><span data-stu-id="acfad-108">The `DynamicPopulate` control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="acfad-109">W tym samouczku pokazano, jak to skonfigurować.</span><span class="sxs-lookup"><span data-stu-id="acfad-109">This tutorial shows how to set this up.</span></span>

## <a name="steps"></a><span data-ttu-id="acfad-110">Kroki</span><span class="sxs-lookup"><span data-stu-id="acfad-110">Steps</span></span>

<span data-ttu-id="acfad-111">Najpierw potrzebna jest usługa sieci Web ASP.NET, która implementuje metodę, która ma zostać wywołana przez `DynamicPopulate`.</span><span class="sxs-lookup"><span data-stu-id="acfad-111">First of all, you need an ASP.NET Web Service which implements the method to be called by `DynamicPopulate`.</span></span> <span data-ttu-id="acfad-112">Klasa usługi sieci Web wymaga atrybutu `ScriptService`, który jest zdefiniowany w ramach `Microsoft.Web.Script.Services`; w przeciwnym razie ASP.NET AJAX nie może utworzyć serwera proxy JavaScript po stronie klienta dla usługi sieci Web, która z kolei jest wymagana przez `DynamicPopulate`.</span><span class="sxs-lookup"><span data-stu-id="acfad-112">The web service class requires the `ScriptService` attribute which is defined within `Microsoft.Web.Script.Services`; otherwise ASP.NET AJAX cannot create the client-side JavaScript proxy for the web service which in turn is required by `DynamicPopulate`.</span></span>

<span data-ttu-id="acfad-113">Metoda sieci Web musi oczekiwać jednego argumentu typu String o nazwie `contextKey`, ponieważ kontrolka `DynamicPopulate` wysyła jedną część informacji o kontekście do każdego wywołania usługi sieci Web.</span><span class="sxs-lookup"><span data-stu-id="acfad-113">The web method must expect one argument of type string, called `contextKey`, since the `DynamicPopulate` control sends one piece of context information with each web service call.</span></span> <span data-ttu-id="acfad-114">Poniższa usługa sieci Web zwraca bieżącą datę w formacie reprezentowanym przez argument `contextKey`:</span><span class="sxs-lookup"><span data-stu-id="acfad-114">The following web service returns the current date in a format represented by the `contextKey` argument:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample1.aspx)]

<span data-ttu-id="acfad-115">Usługa sieci Web jest następnie zapisywana jako `DynamicPopulate.vb.asmx`.</span><span class="sxs-lookup"><span data-stu-id="acfad-115">The web service is then saved as `DynamicPopulate.vb.asmx`.</span></span> <span data-ttu-id="acfad-116">Alternatywnie można zaimplementować metodę `getDate()` jako metodę strony na rzeczywistej stronie ASP.NET z kontrolką `DynamicPopulate`.</span><span class="sxs-lookup"><span data-stu-id="acfad-116">Alternatively, you could implement the `getDate()` method as a page method within the actual ASP.NET page with the `DynamicPopulate` control.</span></span>

<span data-ttu-id="acfad-117">W następnym kroku Utwórz nowy plik ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="acfad-117">In the next step, create a new ASP.NET file.</span></span> <span data-ttu-id="acfad-118">Jak zawsze, pierwszym krokiem jest uwzględnienie `ScriptManager` na bieżącej stronie, aby załadować bibliotekę AJAX ASP.NET i aby działała zestaw narzędzi sterowania:</span><span class="sxs-lookup"><span data-stu-id="acfad-118">As always, the first step is to include the `ScriptManager` in the current page to load the ASP.NET AJAX library and to make the Control Toolkit work:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample2.aspx)]

<span data-ttu-id="acfad-119">Następnie Dodaj kontrolkę etykieta (na przykład przy użyciu kontrolki HTML o tej samej nazwie lub &lt;`asp:Label` /&gt; formant sieci Web), która będzie później wyświetlać wynik wywołania usługi sieci Web.</span><span class="sxs-lookup"><span data-stu-id="acfad-119">Then, add a label control (for instance using the HTML control of the same name, or the &lt;`asp:Label` /&gt; web control) which will later show the result of the web service call.</span></span>

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample3.aspx)]

<span data-ttu-id="acfad-120">Przycisk HTML (jako kontrolka HTML, ponieważ nie wymaga ogłaszania zwrotnego na serwerze) będzie używany do wyzwalania populacji dynamicznej:</span><span class="sxs-lookup"><span data-stu-id="acfad-120">An HTML button (as an HTML control, since we do not require a postback to the server) will then be used to trigger the dynamic population:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample4.aspx)]

<span data-ttu-id="acfad-121">Na koniec potrzebujemy formantu `DynamicPopulateExtender`, aby obprowadzić działania.</span><span class="sxs-lookup"><span data-stu-id="acfad-121">Finally, we need the `DynamicPopulateExtender` control to wire things up.</span></span> <span data-ttu-id="acfad-122">Następujące atrybuty zostaną ustawione (poza oczywistymi, `ID` i `runat`=`"server"`):</span><span class="sxs-lookup"><span data-stu-id="acfad-122">The following attributes will be set (apart from the obvious ones, `ID` and `runat`=`"server"`):</span></span>

- <span data-ttu-id="acfad-123">`TargetControlID` miejsca, w którym ma zostać umieszczony wynik wywołania usługi sieci Web</span><span class="sxs-lookup"><span data-stu-id="acfad-123">`TargetControlID` where to put the result from the web service call</span></span>
- <span data-ttu-id="acfad-124">`ServicePath` ścieżkę do usługi sieci Web (Pomiń, jeśli chcesz użyć metody strony)</span><span class="sxs-lookup"><span data-stu-id="acfad-124">`ServicePath` path to the web service (omit if you want to use a page method)</span></span>
- <span data-ttu-id="acfad-125">`ServiceMethod` nazwę metody sieci Web lub metody strony</span><span class="sxs-lookup"><span data-stu-id="acfad-125">`ServiceMethod` name of the web method or page method</span></span>
- <span data-ttu-id="acfad-126">`ContextKey` informacje kontekstu do wysłania do usługi sieci Web</span><span class="sxs-lookup"><span data-stu-id="acfad-126">`ContextKey` context information to be sent to the web service</span></span>
- <span data-ttu-id="acfad-127">`PopulateTriggerControlID` element wyzwalający wywołanie usługi sieci Web</span><span class="sxs-lookup"><span data-stu-id="acfad-127">`PopulateTriggerControlID` element which triggers the web service call</span></span>
- <span data-ttu-id="acfad-128">`ClearContentsDuringUpdate`, czy element docelowy ma być opróżniany podczas wywołania usługi sieci Web</span><span class="sxs-lookup"><span data-stu-id="acfad-128">`ClearContentsDuringUpdate` whether to empty the target element during the web service call</span></span>

<span data-ttu-id="acfad-129">Jak widać, formant wymaga pewnych informacji, ale umieszczenie wszystkiego w miejscu jest bardzo proste.</span><span class="sxs-lookup"><span data-stu-id="acfad-129">As you can see, the control requires some information but putting everything into place is quite straight-forward.</span></span> <span data-ttu-id="acfad-130">Poniżej znajduje się znacznik kontrolki `DynamicPopulateExtender` w bieżącym scenariuszu:</span><span class="sxs-lookup"><span data-stu-id="acfad-130">Here is the markup for the `DynamicPopulateExtender` control in the current scenario:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample5.aspx)]

<span data-ttu-id="acfad-131">Uruchom stronę ASP.NET w przeglądarce i kliknij przycisk; Bieżąca data zostanie wyświetlona w formacie miesiąca dziennego.</span><span class="sxs-lookup"><span data-stu-id="acfad-131">Run the ASP.NET page in the browser and click on the button; you will receive the current date in month-day-year format.</span></span>

<span data-ttu-id="acfad-132">[![kliknięcie przycisku pobiera datę z serwera](dynamically-populating-a-control-vb/_static/image2.png)](dynamically-populating-a-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="acfad-132">[![A click on the button retrieves the date from the server](dynamically-populating-a-control-vb/_static/image2.png)](dynamically-populating-a-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="acfad-133">Kliknięcie przycisku pobiera datę z serwera ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](dynamically-populating-a-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="acfad-133">A click on the button retrieves the date from the server ([Click to view full-size image](dynamically-populating-a-control-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="acfad-134">[Poprzednie](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)
> [dalej](dynamically-populating-a-control-using-javascript-code-vb.md)</span><span class="sxs-lookup"><span data-stu-id="acfad-134">[Previous](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)
[Next](dynamically-populating-a-control-using-javascript-code-vb.md)</span></span>

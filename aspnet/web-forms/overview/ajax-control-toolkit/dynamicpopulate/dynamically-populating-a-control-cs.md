---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-cs
title: Dynamiczne wypełnianie kontrolki (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolki DynamicPopulate w ASP.NET AJAX Control Toolkit wywołuje usługę sieci web (lub metody korzystającej ze strony) i wypełnia wynikowej wartości do formantu docelowego t...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e1fec43e-1daf-49d2-b0c7-7f1b930455cc
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 42c1cd684196c026f1435cba289fc2535187087c
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/09/2019
ms.locfileid: "59417302"
---
# <a name="dynamically-populating-a-control-c"></a><span data-ttu-id="ba3aa-103">Dynamiczne wypełnianie kontrolki (C#)</span><span class="sxs-lookup"><span data-stu-id="ba3aa-103">Dynamically Populating a Control (C#)</span></span>

<span data-ttu-id="ba3aa-104">przez [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="ba3aa-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="ba3aa-105">[Pobierz program Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate0.cs.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="ba3aa-105">[Download Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate0.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate0CS.pdf)</span></span>

> <span data-ttu-id="ba3aa-106">Kontrolki DynamicPopulate w ASP.NET AJAX Control Toolkit wywołuje usługę sieci web (lub metody korzystającej ze strony) i wprowadza wartość wynikową do formantu docelowego na stronie bez odświeżania strony.</span><span class="sxs-lookup"><span data-stu-id="ba3aa-106">The DynamicPopulate control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span>


## <a name="overview"></a><span data-ttu-id="ba3aa-107">Omówienie</span><span class="sxs-lookup"><span data-stu-id="ba3aa-107">Overview</span></span>

<span data-ttu-id="ba3aa-108">`DynamicPopulate` Kontrola w ASP.NET AJAX Control Toolkit wywołuje usługę sieci web (lub metody korzystającej ze strony) i wprowadza wartość wynikową do formantu docelowego na stronie bez odświeżania strony.</span><span class="sxs-lookup"><span data-stu-id="ba3aa-108">The `DynamicPopulate` control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="ba3aa-109">W tym samouczku pokazano, jak to skonfigurować.</span><span class="sxs-lookup"><span data-stu-id="ba3aa-109">This tutorial shows how to set this up.</span></span>

## <a name="steps"></a><span data-ttu-id="ba3aa-110">Kroki</span><span class="sxs-lookup"><span data-stu-id="ba3aa-110">Steps</span></span>

<span data-ttu-id="ba3aa-111">Po pierwsze, potrzebujesz usługi sieci Web ASP.NET, która implementuje metodę można wywoływać za pomocą `DynamicPopulate`.</span><span class="sxs-lookup"><span data-stu-id="ba3aa-111">First of all, you need an ASP.NET Web Service which implements the method to be called by `DynamicPopulate`.</span></span> <span data-ttu-id="ba3aa-112">Klasa usługi sieci web wymaga `ScriptService` atrybutu, która jest zdefiniowana w ramach `Microsoft.Web.Script.Services`; w przeciwnym razie ASP.NET AJAX, nie można utworzyć serwera proxy JavaScript po stronie klienta dla usługi sieci web, która z kolei jest wymagana przez `DynamicPopulate`.</span><span class="sxs-lookup"><span data-stu-id="ba3aa-112">The web service class requires the `ScriptService` attribute which is defined within `Microsoft.Web.Script.Services`; otherwise ASP.NET AJAX cannot create the client-side JavaScript proxy for the web service which in turn is required by `DynamicPopulate`.</span></span>

<span data-ttu-id="ba3aa-113">Metoda sieci web należy oczekiwać jeden argument typu String, o nazwie `contextKey`, ponieważ `DynamicPopulate` kontroli wysyła jeden fragment informacje o kontekście przy każdym wywołaniem usługi sieci web.</span><span class="sxs-lookup"><span data-stu-id="ba3aa-113">The web method must expect one argument of type string, called `contextKey`, since the `DynamicPopulate` control sends one piece of context information with each web service call.</span></span> <span data-ttu-id="ba3aa-114">Następująca usługa sieci web zwraca bieżącą datę w formacie, reprezentowane przez `contextKey` argumentu:</span><span class="sxs-lookup"><span data-stu-id="ba3aa-114">The following web service returns the current date in a format represented by the `contextKey` argument:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample1.aspx)]

<span data-ttu-id="ba3aa-115">Usługa sieci web, następnie jest zapisywany jako `DynamicPopulate.cs.asmx`.</span><span class="sxs-lookup"><span data-stu-id="ba3aa-115">The web service is then saved as `DynamicPopulate.cs.asmx`.</span></span> <span data-ttu-id="ba3aa-116">Alternatywnie można zaimplementować `getDate()` metodę jako metodę stron w ciągu rzeczywistą stronę ASP.NET za pomocą `DynamicPopulate` kontroli.</span><span class="sxs-lookup"><span data-stu-id="ba3aa-116">Alternatively, you could implement the `getDate()` method as a page method within the actual ASP.NET page with the `DynamicPopulate` control.</span></span>

<span data-ttu-id="ba3aa-117">W następnym kroku utwórz nowy plik platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ba3aa-117">In the next step, create a new ASP.NET file.</span></span> <span data-ttu-id="ba3aa-118">Jak zawsze, pierwszym krokiem jest uwzględnienie `ScriptManager` na bieżącej stronie, można załadować biblioteki ASP.NET AJAX i pracy zestaw narzędzi do sterowania:</span><span class="sxs-lookup"><span data-stu-id="ba3aa-118">As always, the first step is to include the `ScriptManager` in the current page to load the ASP.NET AJAX library and to make the Control Toolkit work:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample2.aspx)]

<span data-ttu-id="ba3aa-119">Następnie dodaj kontrolkę typu etykieta (na przykład za pomocą kontrolki HTML o takiej samej nazwie lub &lt; `asp:Label`  / &gt; formant sieci web) będzie później przedstawiono w nich wynik wywołania usługi sieci web.</span><span class="sxs-lookup"><span data-stu-id="ba3aa-119">Then, add a label control (for instance using the HTML control of the same name, or the &lt;`asp:Label` /&gt; web control) which will later show the result of the web service call.</span></span>

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample3.aspx)]

<span data-ttu-id="ba3aa-120">Przycisk HTML (jako kontrolkę HTML, ponieważ nie wymaga zwrot do serwera), następnie będzie służyć do wyzwolenia dynamiczne wypełnianie:</span><span class="sxs-lookup"><span data-stu-id="ba3aa-120">An HTML button (as an HTML control, since we do not require a postback to the server) will then be used to trigger the dynamic population:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample4.aspx)]

<span data-ttu-id="ba3aa-121">Na koniec musimy `DynamicPopulateExtender` kontroli coś o komunikacji sieciowej w górę.</span><span class="sxs-lookup"><span data-stu-id="ba3aa-121">Finally, we need the `DynamicPopulateExtender` control to wire things up.</span></span> <span data-ttu-id="ba3aa-122">Następujące atrybuty zostaną ustawione (oprócz widocznych te `ID` i `runat` = `"server"`):</span><span class="sxs-lookup"><span data-stu-id="ba3aa-122">The following attributes will be set (apart from the obvious ones, `ID` and `runat`=`"server"`):</span></span>

- `TargetControlID` <span data-ttu-id="ba3aa-123">gdzie umieścić wyniki z wywołaniem usługi sieci web</span><span class="sxs-lookup"><span data-stu-id="ba3aa-123">where to put the result from the web service call</span></span>
- `ServicePath` <span data-ttu-id="ba3aa-124">Ścieżka do usługi sieci web (pominąć, jeśli chcesz użyć metody strony)</span><span class="sxs-lookup"><span data-stu-id="ba3aa-124">path to the web service (omit if you want to use a page method)</span></span>
- `ServiceMethod` <span data-ttu-id="ba3aa-125">Nazwa metody sieci web lub metody korzystającej ze strony</span><span class="sxs-lookup"><span data-stu-id="ba3aa-125">name of the web method or page method</span></span>
- `ContextKey` <span data-ttu-id="ba3aa-126">informacje o kontekście do wysłania do usługi sieci web</span><span class="sxs-lookup"><span data-stu-id="ba3aa-126">context information to be sent to the web service</span></span>
- `PopulateTriggerControlID` <span data-ttu-id="ba3aa-127">element, co powoduje wyzwolenie wywołania usługi sieci web</span><span class="sxs-lookup"><span data-stu-id="ba3aa-127">element which triggers the web service call</span></span>
- `ClearContentsDuringUpdate` <span data-ttu-id="ba3aa-128">Czy chcesz opróżnić elementu docelowego podczas wywołania usługi sieci web</span><span class="sxs-lookup"><span data-stu-id="ba3aa-128">whether to empty the target element during the web service call</span></span>

<span data-ttu-id="ba3aa-129">Jak widać, którego wymaga formant pewne informacje, ale umieszczenia wszystkiego w miejscu jest bardzo proste.</span><span class="sxs-lookup"><span data-stu-id="ba3aa-129">As you can see, the control requires some information but putting everything into place is quite straight-forward.</span></span> <span data-ttu-id="ba3aa-130">Oto znaczniki dla `DynamicPopulateExtender` kontrolki, w tym scenariuszu:</span><span class="sxs-lookup"><span data-stu-id="ba3aa-130">Here is the markup for the `DynamicPopulateExtender` control in the current scenario:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample5.aspx)]

<span data-ttu-id="ba3aa-131">Uruchamianie jej w przeglądarce strony ASP.NET, a następnie kliknij przycisk; otrzymasz bieżącą datę w formacie dzień miesiąc rok.</span><span class="sxs-lookup"><span data-stu-id="ba3aa-131">Run the ASP.NET page in the browser and click on the button; you will receive the current date in month-day-year format.</span></span>


[![A <span data-ttu-id="ba3aa-132">Kliknij przycisk pobiera datę z serwera]</span><span class="sxs-lookup"><span data-stu-id="ba3aa-132">click on the button retrieves the date from the server]</span></span>(dynamically-populating-a-control-cs/_static/image2.png)](dynamically-populating-a-control-cs/_static/image1.png)

<span data-ttu-id="ba3aa-133">Kliknięcie przycisku pobiera datę z serwera ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](dynamically-populating-a-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="ba3aa-133">A click on the button retrieves the date from the server ([Click to view full-size image](dynamically-populating-a-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="ba3aa-134">Next</span><span class="sxs-lookup"><span data-stu-id="ba3aa-134">Next</span></span>](dynamically-populating-a-control-using-javascript-code-cs.md)

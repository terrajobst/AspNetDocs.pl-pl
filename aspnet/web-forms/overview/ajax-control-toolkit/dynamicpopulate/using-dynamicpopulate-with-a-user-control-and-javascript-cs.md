---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-cs
title: Używanie używanie dynamicpopulate z kontrolką użytkownika i JavaScript (C#) | Microsoft Docs
author: wenz
description: Kontrolka używanie dynamicpopulate w zestawie narzędzi ASP.NET AJAX Control wywołuje usługę sieci Web (lub metodę strony) i wypełnia wartość wyniki do kontrolki docelowej na t...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 38ac8250-8854-444c-b9ab-8998faa41c5a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-cs
msc.type: authoredcontent
ms.openlocfilehash: a0e6d04a5f62ab558aceb8302d94d3bf2dc8a39f
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599154"
---
# <a name="using-dynamicpopulate-with-a-user-control-and-javascript-c"></a><span data-ttu-id="4a283-103">Używanie kontrolki DynamicPopulate z kontrolką użytkownika i kodem JavaScript (C#)</span><span class="sxs-lookup"><span data-stu-id="4a283-103">Using DynamicPopulate with a User Control And JavaScript (C#)</span></span>

<span data-ttu-id="4a283-104">Autor [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="4a283-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="4a283-105">[Pobierz kod](https://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.cs.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="4a283-105">[Download Code](https://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.cs.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2CS.pdf)</span></span>

> <span data-ttu-id="4a283-106">Kontrolka używanie dynamicpopulate w zestawie narzędzi ASP.NET AJAX Control wywołuje usługę sieci Web (lub metodę strony) i wypełnia wartość wyniki do kontrolki docelowej na stronie bez odświeżania strony.</span><span class="sxs-lookup"><span data-stu-id="4a283-106">The DynamicPopulate control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="4a283-107">Możliwe jest również wyzwolenie populacji przy użyciu niestandardowego kodu JavaScript po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="4a283-107">It is also possible to trigger the population using custom client-side JavaScript code.</span></span> <span data-ttu-id="4a283-108">Należy jednak zwrócić szczególną uwagę, gdy rozszerzenie znajduje się w kontrolce użytkownika.</span><span class="sxs-lookup"><span data-stu-id="4a283-108">However special care has to be taken when the extender resides in a user control.</span></span>

## <a name="overview"></a><span data-ttu-id="4a283-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="4a283-109">Overview</span></span>

<span data-ttu-id="4a283-110">Formant `DynamicPopulate` w zestawie narzędzi ASP.NET AJAX Control wywołuje usługę sieci Web (lub metodę strony) i wypełnia wartość wyniki do kontrolki docelowej na stronie bez odświeżania strony.</span><span class="sxs-lookup"><span data-stu-id="4a283-110">The `DynamicPopulate` control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="4a283-111">Możliwe jest również wyzwolenie populacji przy użyciu niestandardowego kodu JavaScript po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="4a283-111">It is also possible to trigger the population using custom client-side JavaScript code.</span></span> <span data-ttu-id="4a283-112">Należy jednak zwrócić szczególną uwagę, gdy rozszerzenie znajduje się w kontrolce użytkownika.</span><span class="sxs-lookup"><span data-stu-id="4a283-112">However special care has to be taken when the extender resides in a user control.</span></span>

## <a name="steps"></a><span data-ttu-id="4a283-113">Kroki</span><span class="sxs-lookup"><span data-stu-id="4a283-113">Steps</span></span>

<span data-ttu-id="4a283-114">Najpierw potrzebna jest usługa sieci Web ASP.NET, która implementuje metodę, która ma zostać wywołana przez formant `DynamicPopulateExtender`.</span><span class="sxs-lookup"><span data-stu-id="4a283-114">First of all, you need an ASP.NET Web Service which implements the method to be called by the `DynamicPopulateExtender` control.</span></span> <span data-ttu-id="4a283-115">Usługa sieci Web implementuje metodę `getDate()`, która oczekuje jednego argumentu typu ciąg o nazwie `contextKey`, ponieważ kontrolka `DynamicPopulate` wysyła jedną część informacji o kontekście do każdego wywołania usługi sieci Web.</span><span class="sxs-lookup"><span data-stu-id="4a283-115">The web service implements the method `getDate()` that expects one argument of type string, called `contextKey`, since the `DynamicPopulate` control sends one piece of context information with each web service call.</span></span> <span data-ttu-id="4a283-116">Oto kod (plik `DynamicPopulate.cs.asmx`), który pobiera bieżącą datę w jednym z trzech formatów:</span><span class="sxs-lookup"><span data-stu-id="4a283-116">Here is the code (file `DynamicPopulate.cs.asmx`) which retrieves the current date in one of three formats:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample1.aspx)]

<span data-ttu-id="4a283-117">W następnym kroku Utwórz nową kontrolkę użytkownika (plik`.ascx`), która jest oznaczona przez następującą deklarację w pierwszym wierszu:</span><span class="sxs-lookup"><span data-stu-id="4a283-117">In the next step, create a new user control (`.ascx` file), denoted by the following declaration in its first line:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample2.aspx)]

<span data-ttu-id="4a283-118">Element &lt;`label`&gt; będzie używany do wyświetlania danych pochodzących z serwera.</span><span class="sxs-lookup"><span data-stu-id="4a283-118">A &lt;`label`&gt; element will be used to display the data coming from the server.</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample3.aspx)]

<span data-ttu-id="4a283-119">Ponadto w pliku kontrolnym użytkownika będziemy używać trzech przycisków radiowych, z których każdy reprezentuje jeden z trzech możliwych formatów daty obsługiwanych przez usługę sieci Web.</span><span class="sxs-lookup"><span data-stu-id="4a283-119">Also in the user control file, we will use three radio buttons, each one representing one of the three possible date formats supported by the web service.</span></span> <span data-ttu-id="4a283-120">Gdy użytkownik kliknie jeden z przycisków radiowych, przeglądarka wykona kod JavaScript, który wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="4a283-120">When the user clicks on one of the radio buttons, the browser will execute JavaScript code which looks like this:</span></span>

[!code-powershell[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample4.ps1)]

<span data-ttu-id="4a283-121">Ten kod uzyskuje dostęp do `DynamicPopulateExtender` (nie martw się o nietypowego identyfikatora, zostanie on uwzględniony w dalszej części) i wyzwala dynamiczną populację z danymi.</span><span class="sxs-lookup"><span data-stu-id="4a283-121">This code accesses the `DynamicPopulateExtender` (do not worry about the strange ID yet, this will be covered later on) and triggers the dynamic population with data.</span></span> <span data-ttu-id="4a283-122">W kontekście bieżącego przycisku radiowego `this.value` odnosi się do jego wartości `format1`, `format2` lub `format3` dokładnie tak, jak oczekuje Metoda sieci Web.</span><span class="sxs-lookup"><span data-stu-id="4a283-122">In the context of the current radio button, `this.value` refers to its value which is `format1`, `format2` or `format3` exactly what the web method expects.</span></span>

<span data-ttu-id="4a283-123">Jedyną kwestią w kontrolce użytkownika jest jeszcze formant `DynamicPopulateExtender`, który łączy przyciski radiowe z usługą sieci Web.</span><span class="sxs-lookup"><span data-stu-id="4a283-123">The only thing missing in the user control yet is the `DynamicPopulateExtender` control which links the radio buttons to the web service.</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample5.aspx)]

<span data-ttu-id="4a283-124">Ponownie można zauważyć, że jest to dziwny identyfikator używany w kontrolce: `mcd1$myDate`, a nie `myDate`.</span><span class="sxs-lookup"><span data-stu-id="4a283-124">Again you may note the strange ID used in the control: `mcd1$myDate` instead of `myDate`.</span></span> <span data-ttu-id="4a283-125">Wcześniej kod JavaScript używany `mcd1_dpe1`, aby uzyskać dostęp do `DynamicPopulateExtender` zamiast `dpe1`. Ta strategia nazewnictwa jest specjalnym wymaganiem w przypadku używania `DynamicPopulateExtender` w ramach kontrolki użytkownika.</span><span class="sxs-lookup"><span data-stu-id="4a283-125">Previously, the JavaScript code used `mcd1_dpe1` to access the `DynamicPopulateExtender` instead of `dpe1`.This naming strategy is a special requirement when using `DynamicPopulateExtender` within a user control.</span></span> <span data-ttu-id="4a283-126">Ponadto należy osadzić kontrolkę użytkownika w określony sposób, aby wszystko działało.</span><span class="sxs-lookup"><span data-stu-id="4a283-126">Furthermore, you have to embed the user control in a specific way to make it all work.</span></span> <span data-ttu-id="4a283-127">Utwórz nową stronę ASP.NET i zarejestruj Prefiks tagu dla właśnie zaimplementowanej kontrolki użytkownika:</span><span class="sxs-lookup"><span data-stu-id="4a283-127">Create a new ASP.NET page and register a tag prefix for the user control you have just implemented:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample6.aspx)]

<span data-ttu-id="4a283-128">Następnie na nowej stronie Uwzględnij formant `ScriptManager` AJAX ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="4a283-128">Then, include the ASP.NET AJAX `ScriptManager` control on the new page:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample7.aspx)]

<span data-ttu-id="4a283-129">Na koniec Dodaj kontrolkę użytkownika do strony.</span><span class="sxs-lookup"><span data-stu-id="4a283-129">Finally, add the user control to the page.</span></span> <span data-ttu-id="4a283-130">Musisz tylko ustawić jego atrybut `ID` (i `runat="server"`, oczywiście), ale trzeba również ustawić go na określoną nazwę: `mcd1` ponieważ jest to prefiks używany w kontrolce użytkownika do uzyskiwania dostępu do niego przy użyciu języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="4a283-130">You only have to set its `ID` attribute (and `runat="server"`, of course), but you also have to set it to a specific name: `mcd1` since this is the prefix used within the user control to access it using JavaScript.</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample8.aspx)]

<span data-ttu-id="4a283-131">I to wszystko!</span><span class="sxs-lookup"><span data-stu-id="4a283-131">And that's it!</span></span> <span data-ttu-id="4a283-132">Strona działa zgodnie z oczekiwaniami: użytkownik klika jeden z przycisków radiowych, formant w zestawie narzędzi wywołuje usługę sieci Web i wyświetla bieżącą datę w żądanym formacie.</span><span class="sxs-lookup"><span data-stu-id="4a283-132">The page behaves as expected: A user clicks on one of the radio buttons, the control in the Toolkit calls the web service and displays the current date in the desired format.</span></span>

<span data-ttu-id="4a283-133">[![przyciski radiowe znajdują się w kontrolce użytkownika](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="4a283-133">[![The radio buttons reside in a user control](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image1.png)</span></span>

<span data-ttu-id="4a283-134">Przyciski radiowe znajdują się w kontrolce użytkownika ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="4a283-134">The radio buttons reside in a user control ([Click to view full-size image](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="4a283-135">[Poprzednie](dynamically-populating-a-control-using-javascript-code-cs.md)
> [dalej](dynamically-populating-a-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="4a283-135">[Previous](dynamically-populating-a-control-using-javascript-code-cs.md)
[Next](dynamically-populating-a-control-vb.md)</span></span>

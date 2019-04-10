---
uid: web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-cs
title: Tworzenie Numeric w górę/dół kontrolki z zapleczem usługi internetowej (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Zamiast umożliwienie użytkownikowi wpisz wartość w polu wyboru, kontrolki numeric up/down (znajdującą się na Windows i innych systemów operacyjnych) może okazać się c ponieważ coraz więcej...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: c99bbc72-d4de-41ed-92a4-9a4632368363
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-cs
msc.type: authoredcontent
ms.openlocfilehash: b8160c6f5ac090e120e86f4273749b756857967e
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/09/2019
ms.locfileid: "59385712"
---
# <a name="creating-a-numeric-updown-control-with-a-web-service-backend-c"></a><span data-ttu-id="26681-103">Tworzenie kontrolki Numeric Up/Down z zapleczem usługi internetowej (C#)</span><span class="sxs-lookup"><span data-stu-id="26681-103">Creating a Numeric Up/Down Control with a Web Service Backend (C#)</span></span>

<span data-ttu-id="26681-104">przez [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="26681-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="26681-105">[Pobierz program Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/numericupdown1.cs.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/numericupdown1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="26681-105">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/numericupdown1.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/numericupdown1CS.pdf)</span></span>

> <span data-ttu-id="26681-106">Zamiast umożliwienie użytkownikowi wpisz wartość w polu wyboru, liczbowych w górę/dół kontroli (co oznacza, że istnieje na Windows i innych systemów operacyjnych) można udowodnić, że ponieważ coraz więcej doświadczenia.</span><span class="sxs-lookup"><span data-stu-id="26681-106">Instead of letting a user type a value into a check box, a numeric up/down control (that exists on Windows and other operating systems) could prove as more comfortable.</span></span> <span data-ttu-id="26681-107">Domyślnie formant NumericUpDown zawsze zwiększa lub zmniejsza wartość 1, ale usługi sieci web okaże bardziej elastyczne.</span><span class="sxs-lookup"><span data-stu-id="26681-107">By default, the NumericUpDown control always increases or decreases a value by 1, but a web service proves more flexibility.</span></span>


## <a name="overview"></a><span data-ttu-id="26681-108">Omówienie</span><span class="sxs-lookup"><span data-stu-id="26681-108">Overview</span></span>

<span data-ttu-id="26681-109">Zamiast umożliwienie użytkownikowi wpisz wartość w polu wyboru, liczbowych w górę/dół kontroli (co oznacza, że istnieje na Windows i innych systemów operacyjnych) można udowodnić, że ponieważ coraz więcej doświadczenia.</span><span class="sxs-lookup"><span data-stu-id="26681-109">Instead of letting a user type a value into a check box, a numeric up/down control (that exists on Windows and other operating systems) could prove as more comfortable.</span></span> <span data-ttu-id="26681-110">Domyślnie `NumericUpDown` kontroli zawsze zwiększa lub zmniejsza wartość 1, ale usługi sieci web okaże bardziej elastyczne.</span><span class="sxs-lookup"><span data-stu-id="26681-110">By default, the `NumericUpDown` control always increases or decreases a value by 1, but a web service proves more flexibility.</span></span>

## <a name="steps"></a><span data-ttu-id="26681-111">Kroki</span><span class="sxs-lookup"><span data-stu-id="26681-111">Steps</span></span>

<span data-ttu-id="26681-112">ASP.NET AJAX Control Toolkit zawiera `NumericUpDown` rozszerzeń, który automatycznie dodaje dwa przyciski do pola tekstowego: Jedną zwiększenia jego wartość, jedną dla zmniejsza się go.</span><span class="sxs-lookup"><span data-stu-id="26681-112">The ASP.NET AJAX Control Toolkit contains the `NumericUpDown` extender which automatically adds two buttons to a text box: One for increasing its value, one for decreasing it.</span></span> <span data-ttu-id="26681-113">Jednak formant obsługuje również wywołanie usługi sieci web (lub wywołanie metody strony).</span><span class="sxs-lookup"><span data-stu-id="26681-113">However the control also supports a web service call (or page method call).</span></span> <span data-ttu-id="26681-114">Zawsze, gdy w górę lub dół przycisk po kliknięciu JavaScript kod łączy się z serwerem sieci web i wykonuje metodę istnieje.</span><span class="sxs-lookup"><span data-stu-id="26681-114">Whenever the up or down button is clicked, the JavaScript code connects to the web server and executes a method there.</span></span> <span data-ttu-id="26681-115">Podpis metody jest następujące:</span><span class="sxs-lookup"><span data-stu-id="26681-115">The method signature is the following one:</span></span>

[!code-csharp[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/samples/sample1.cs)]

<span data-ttu-id="26681-116">`current` Argument jest bieżąca wartość w polu tekstowym; `tag` atrybut jest dodatkowy kontekst danych, którą można określić jako właściwość `NumericUpDown` rozszerzeń (ale nie jest wymagane).</span><span class="sxs-lookup"><span data-stu-id="26681-116">The `current` argument is the current value in the text box; the `tag` attribute is additional context data that can be set as a property of the `NumericUpDown` extender (but is not required).</span></span>

<span data-ttu-id="26681-117">W tym przykładzie kontrolki numeric up/down tylko Zezwalaj na wartości, które są dwa uprawnienia: 1, 2, 4, 8, 16, 32, 64 i tak dalej.</span><span class="sxs-lookup"><span data-stu-id="26681-117">For this sample, the numeric up/down control shall only allow values that are powers of two: 1, 2, 4, 8, 16, 32, 64, and so on.</span></span> <span data-ttu-id="26681-118">W związku z tym metoda wykonania, gdy użytkownik chce, aby zwiększyć wartość należy double stara wartość; inna metoda należy podzielić wartość przez dwa.</span><span class="sxs-lookup"><span data-stu-id="26681-118">Therefore, the method executed when the user wants to increase the value must double the old value; the other method must divide value by two.</span></span> <span data-ttu-id="26681-119">Oto więc usługę sieci web zakończenie:</span><span class="sxs-lookup"><span data-stu-id="26681-119">So here is the complete web service:</span></span>

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/samples/sample2.aspx)]

<span data-ttu-id="26681-120">Na koniec Utwórz nową stronę programu ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="26681-120">Finally, create a new ASP.NET page.</span></span> <span data-ttu-id="26681-121">Należy w zwykły sposób, `ScriptManager` kontroli `TextBox` kontroli i `NumericUpDownExtender` kontroli.</span><span class="sxs-lookup"><span data-stu-id="26681-121">As usual, you need a `ScriptManager` control, a `TextBox` control and a `NumericUpDownExtender` control.</span></span> <span data-ttu-id="26681-122">W przypadku drugiego nagłówka musisz podać informacje o usłudze sieci web:</span><span class="sxs-lookup"><span data-stu-id="26681-122">For the latter, you have to provide the web service information:</span></span>

- `ServiceDownMethod` <span data-ttu-id="26681-123">Nazwa w dół metodę sieci web lub strony — metoda</span><span class="sxs-lookup"><span data-stu-id="26681-123">name of the down web method or page method</span></span>
- `ServiceDownPath` <span data-ttu-id="26681-124">Ścieżka do usługi sieci web za pomocą metody dół usługi; pominąć, jeśli używana jest metoda strony</span><span class="sxs-lookup"><span data-stu-id="26681-124">path to the web service with the down service method; omit if you are using a page method</span></span>
- `ServiceUpMethod` <span data-ttu-id="26681-125">Nazwa pracy metodę sieci web lub strony — metoda</span><span class="sxs-lookup"><span data-stu-id="26681-125">name of the up web method or page method</span></span>
- `ServiceUpPath` <span data-ttu-id="26681-126">Ścieżka do usługi sieci web za pomocą metody pracy usługi; pominąć, jeśli używana jest metoda strony</span><span class="sxs-lookup"><span data-stu-id="26681-126">path to the web service with the up service method; omit if you are using a page method</span></span>

<span data-ttu-id="26681-127">Oto kompletny kod znaczników dla strony:</span><span class="sxs-lookup"><span data-stu-id="26681-127">Here is the complete markup for the page:</span></span>

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/samples/sample3.aspx)]

<span data-ttu-id="26681-128">Po uruchomieniu strony, zwróć uwagę, jak wartość w polu tekstowym zawsze podwaja się po kliknięciu przycisk w prawym górnym i filtrach, po kliknięciu przycisku niższe.</span><span class="sxs-lookup"><span data-stu-id="26681-128">If you run the page, notice how the value in the text box always doubles when you click on the upper button, and is halved when you click on the lower button.</span></span>


[![O<span data-ttu-id="26681-129">tylko do odczytu, które są wartością potęgi liczby 2 pojawiają się]</span><span class="sxs-lookup"><span data-stu-id="26681-129">nly numbers that are a power of 2 appear]</span></span>(creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image2.png)](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image1.png)

<span data-ttu-id="26681-130">Są wyświetlane tylko cyfry, które są wartością potęgi liczby 2 ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="26681-130">Only numbers that are a power of 2 appear ([Click to view full-size image](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="26681-131">Next</span><span class="sxs-lookup"><span data-stu-id="26681-131">Next</span></span>](creating-a-numeric-up-down-control-with-a-web-service-backend-vb.md)

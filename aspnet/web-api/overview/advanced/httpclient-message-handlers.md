---
uid: web-api/overview/advanced/httpclient-message-handlers
title: Programy obsługi komunikatów HttpClient w ASP.NET Web API-ASP.NET 4. x
author: MikeWasson
description: Tworzenie niestandardowych programów obsługi komunikatów dla interfejsu API sieci Web ASP.NET w ASP.NET 4. x
ms.author: riande
ms.date: 10/01/2012
ms.custom: seoapril2019
ms.assetid: 5a4b6c80-b2e9-4710-8969-d5076f7f82b8
msc.legacyurl: /web-api/overview/advanced/httpclient-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: 265bd9b2f48ed7d1e955f3c4947d10fd589b3e17
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557646"
---
# <a name="httpclient-message-handlers-in-aspnet-web-api"></a><span data-ttu-id="146a3-103">Programy obsługi komunikatów HttpClient w interfejsie API sieci Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="146a3-103">HttpClient Message Handlers in ASP.NET Web API</span></span>

<span data-ttu-id="146a3-104">według [Jan Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="146a3-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="146a3-105">*Program obsługi komunikatów* jest klasą, która odbiera żądanie HTTP i zwraca odpowiedź HTTP.</span><span class="sxs-lookup"><span data-stu-id="146a3-105">A *message handler* is a class that receives an HTTP request and returns an HTTP response.</span></span>

<span data-ttu-id="146a3-106">Zwykle seria programów obsługi komunikatów jest łańcuchowo.</span><span class="sxs-lookup"><span data-stu-id="146a3-106">Typically, a series of message handlers are chained together.</span></span> <span data-ttu-id="146a3-107">Pierwsza procedura obsługi odbiera żądanie HTTP, wykonuje pewne przetwarzanie i przekazuje żądanie do kolejnej procedury obsługi.</span><span class="sxs-lookup"><span data-stu-id="146a3-107">The first handler receives an HTTP request, does some processing, and gives the request to the next handler.</span></span> <span data-ttu-id="146a3-108">W pewnym momencie zostanie utworzona odpowiedź i przejdzie tworzenie kopii zapasowej łańcucha.</span><span class="sxs-lookup"><span data-stu-id="146a3-108">At some point, the response is created and goes back up the chain.</span></span> <span data-ttu-id="146a3-109">Ten wzorzec jest nazywany programem obsługi *delegowania* .</span><span class="sxs-lookup"><span data-stu-id="146a3-109">This pattern is called a *delegating* handler.</span></span>

![](httpclient-message-handlers/_static/image1.png)

<span data-ttu-id="146a3-110">Po stronie klienta Klasa **HttpClient** używa obsługi komunikatów do przetwarzania żądań.</span><span class="sxs-lookup"><span data-stu-id="146a3-110">On the client side, the **HttpClient** class uses a message handler to process requests.</span></span> <span data-ttu-id="146a3-111">Domyślna procedura obsługi to **HttpClientHandler**, która wysyła żądanie przez sieć i pobiera odpowiedź z serwera.</span><span class="sxs-lookup"><span data-stu-id="146a3-111">The default handler is **HttpClientHandler**, which sends the request over the network and gets the response from the server.</span></span> <span data-ttu-id="146a3-112">Do potoku klienta można wstawić niestandardowe programy obsługi komunikatów:</span><span class="sxs-lookup"><span data-stu-id="146a3-112">You can insert custom message handlers into the client pipeline:</span></span>

![](httpclient-message-handlers/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="146a3-113">Interfejs API sieci Web ASP.NET również używa programów obsługi komunikatów po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="146a3-113">ASP.NET Web API also uses message handlers on the server side.</span></span> <span data-ttu-id="146a3-114">Aby uzyskać więcej informacji, zobacz [procedury obsługi komunikatów http](http-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="146a3-114">For more information, see [HTTP Message Handlers](http-message-handlers.md).</span></span>

## <a name="custom-message-handlers"></a><span data-ttu-id="146a3-115">Niestandardowe programy obsługi komunikatów</span><span class="sxs-lookup"><span data-stu-id="146a3-115">Custom Message Handlers</span></span>

<span data-ttu-id="146a3-116">Aby napisać niestandardową procedurę obsługi komunikatów, pochodzi od **systemu .NET. http. DelegatingHandler** i przesłania metodę **SendAsync** .</span><span class="sxs-lookup"><span data-stu-id="146a3-116">To write a custom message handler, derive from **System.Net.Http.DelegatingHandler** and override the **SendAsync** method.</span></span> <span data-ttu-id="146a3-117">W tym miejscu znajduje się sygnatura metody:</span><span class="sxs-lookup"><span data-stu-id="146a3-117">Here is the method signature:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample1.cs)]

<span data-ttu-id="146a3-118">Metoda przyjmuje **HttpRequestMessage** jako dane wejściowe i asynchronicznie zwraca **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="146a3-118">The method takes an **HttpRequestMessage** as input and asynchronously returns an **HttpResponseMessage**.</span></span> <span data-ttu-id="146a3-119">Typowa implementacja wykonuje następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="146a3-119">A typical implementation does the following:</span></span>

1. <span data-ttu-id="146a3-120">Przetwórz komunikat żądania.</span><span class="sxs-lookup"><span data-stu-id="146a3-120">Process the request message.</span></span>
2. <span data-ttu-id="146a3-121">Wywołaj `base.SendAsync`, aby wysłać żądanie do wewnętrznej procedury obsługi.</span><span class="sxs-lookup"><span data-stu-id="146a3-121">Call `base.SendAsync` to send the request to the inner handler.</span></span>
3. <span data-ttu-id="146a3-122">Procedura obsługi wewnętrznej zwraca komunikat odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="146a3-122">The inner handler returns a response message.</span></span> <span data-ttu-id="146a3-123">(Ten krok jest asynchroniczny).</span><span class="sxs-lookup"><span data-stu-id="146a3-123">(This step is asynchronous.)</span></span>
4. <span data-ttu-id="146a3-124">Przetwórz odpowiedź i zwróć ją do obiektu wywołującego.</span><span class="sxs-lookup"><span data-stu-id="146a3-124">Process the response and return it to the caller.</span></span>

<span data-ttu-id="146a3-125">Poniższy przykład pokazuje procedurę obsługi komunikatów, która dodaje niestandardowy nagłówek do żądania wychodzącego:</span><span class="sxs-lookup"><span data-stu-id="146a3-125">The following example shows a message handler that adds a custom header to the outgoing request:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample2.cs)]

<span data-ttu-id="146a3-126">Wywołanie `base.SendAsync` jest asynchroniczne.</span><span class="sxs-lookup"><span data-stu-id="146a3-126">The call to `base.SendAsync` is asynchronous.</span></span> <span data-ttu-id="146a3-127">Jeśli program obsługi wykonuje wszelką pracę po tym wywołaniu, użyj słowa kluczowego **await** , aby wznowić wykonywanie po zakończeniu metody.</span><span class="sxs-lookup"><span data-stu-id="146a3-127">If the handler does any work after this call, use the **await** keyword to resume execution after the method completes.</span></span> <span data-ttu-id="146a3-128">Poniższy przykład pokazuje procedurę obsługi, która rejestruje kody błędów.</span><span class="sxs-lookup"><span data-stu-id="146a3-128">The following example shows a handler that logs error codes.</span></span> <span data-ttu-id="146a3-129">Samo rejestrowanie nie jest bardzo interesujące, ale przykład pokazuje, jak uzyskać odpowiedzi wewnątrz procedury obsługi.</span><span class="sxs-lookup"><span data-stu-id="146a3-129">The logging itself is not very interesting, but the example shows how to get at the response inside the handler.</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample3.cs?highlight=10,13)]

## <a name="adding-message-handlers-to-the-client-pipeline"></a><span data-ttu-id="146a3-130">Dodawanie programów obsługi komunikatów do potoku klienta</span><span class="sxs-lookup"><span data-stu-id="146a3-130">Adding Message Handlers to the Client Pipeline</span></span>

<span data-ttu-id="146a3-131">Aby dodać niestandardowe programy obsługi do **HttpClient**, użyj metody **HttpClientFactory. Create** :</span><span class="sxs-lookup"><span data-stu-id="146a3-131">To add custom handlers to **HttpClient**, use the **HttpClientFactory.Create** method:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample4.cs)]

<span data-ttu-id="146a3-132">Procedury obsługi komunikatów są wywoływane w kolejności, w jakiej zostały przekazane do metody **Create** .</span><span class="sxs-lookup"><span data-stu-id="146a3-132">Message handlers are called in the order that you pass them into the **Create** method.</span></span> <span data-ttu-id="146a3-133">Ponieważ procedury obsługi są zagnieżdżone, komunikat odpowiedzi jest przesyłany w innym kierunku.</span><span class="sxs-lookup"><span data-stu-id="146a3-133">Because handlers are nested, the response message travels in the other direction.</span></span> <span data-ttu-id="146a3-134">Oznacza to, że ostatnim programem obsługi jest pierwszy, aby uzyskać komunikat odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="146a3-134">That is, the last handler is the first to get the response message.</span></span>

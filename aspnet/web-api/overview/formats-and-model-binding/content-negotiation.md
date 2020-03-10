---
uid: web-api/overview/formats-and-model-binding/content-negotiation
title: Negocjowanie zawartości w interfejsie Web API ASP.NET — ASP.NET 4. x
author: MikeWasson
description: Opisuje, w jaki sposób interfejs API sieci Web ASP.NET implementuje negocjowanie zawartości HTTP dla ASP.NET 4. x.
ms.author: riande
ms.date: 05/20/2012
ms.custom: seoapril2019
ms.assetid: 0dd51b30-bf5a-419f-a1b7-2817ccca3c7d
msc.legacyurl: /web-api/overview/formats-and-model-binding/content-negotiation
msc.type: authoredcontent
ms.openlocfilehash: cb6668ff6de276d3778ce11f27ce597d8bf1f9c7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622270"
---
# <a name="content-negotiation-in-aspnet-web-api"></a><span data-ttu-id="1890d-103">Negocjowanie zawartości w interfejsie API sieci Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="1890d-103">Content Negotiation in ASP.NET Web API</span></span>

<span data-ttu-id="1890d-104">według [Jan Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="1890d-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="1890d-105">W tym artykule opisano sposób, w jaki interfejs API sieci Web ASP.NET implementuje negocjowanie zawartości dla ASP.NET 4. x.</span><span class="sxs-lookup"><span data-stu-id="1890d-105">This article describes how ASP.NET Web API implements content negotiation for ASP.NET 4.x.</span></span>

<span data-ttu-id="1890d-106">Specyfikacja protokołu HTTP (RFC 2616) definiuje negocjowanie zawartości jako "proces wybierania najlepszej reprezentacji dla danej odpowiedzi w przypadku, gdy jest dostępnych wiele prezentacji".</span><span class="sxs-lookup"><span data-stu-id="1890d-106">The HTTP specification (RFC 2616) defines content negotiation as "the process of selecting the best representation for a given response when there are multiple representations available."</span></span> <span data-ttu-id="1890d-107">Podstawowym mechanizmem negocjacji zawartości w protokole HTTP są następujące nagłówki żądania:</span><span class="sxs-lookup"><span data-stu-id="1890d-107">The primary mechanism for content negotiation in HTTP are these request headers:</span></span>

- <span data-ttu-id="1890d-108">**Zaakceptuj:** Typy nośników akceptowalne dla odpowiedzi, takie jak "Application/JSON", "Application/XML" lub niestandardowy typ nośnika, taki jak &quot;application/vnd. example + XML&quot;</span><span class="sxs-lookup"><span data-stu-id="1890d-108">**Accept:** Which media types are acceptable for the response, such as "application/json," "application/xml," or a custom media type such as &quot;application/vnd.example+xml&quot;</span></span>
- <span data-ttu-id="1890d-109">**Akceptuj-charset:** Które zestawy znaków są akceptowalne, takie jak UTF-8 lub ISO 8859-1.</span><span class="sxs-lookup"><span data-stu-id="1890d-109">**Accept-Charset:** Which character sets are acceptable, such as UTF-8 or ISO 8859-1.</span></span>
- <span data-ttu-id="1890d-110">**Akceptuj-Encoding:** Jakie kodowanie zawartości jest akceptowalne, na przykład gzip.</span><span class="sxs-lookup"><span data-stu-id="1890d-110">**Accept-Encoding:** Which content encodings are acceptable, such as gzip.</span></span>
- <span data-ttu-id="1890d-111">**Akceptuj — język:** Preferowany język naturalny, taki jak "en-us".</span><span class="sxs-lookup"><span data-stu-id="1890d-111">**Accept-Language:** The preferred natural language, such as "en-us".</span></span>

<span data-ttu-id="1890d-112">Serwer może również przeglądać inne fragmenty żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="1890d-112">The server can also look at other portions of the HTTP request.</span></span> <span data-ttu-id="1890d-113">Jeśli na przykład żądanie zawiera nagłówek X-Request-with wskazujący żądanie AJAX, serwer może domyślnie mieć wartość JSON, jeśli nie ma nagłówka Accept.</span><span class="sxs-lookup"><span data-stu-id="1890d-113">For example, if the request contains an X-Requested-With header, indicating an AJAX request, the server might default to JSON if there is no Accept header.</span></span>

<span data-ttu-id="1890d-114">W tym artykule dowiesz się, jak interfejs API sieci Web używa nagłówków Accept i Accept-Charset.</span><span class="sxs-lookup"><span data-stu-id="1890d-114">In this article, we'll look at how Web API uses the Accept and Accept-Charset headers.</span></span> <span data-ttu-id="1890d-115">(W tej chwili nie ma wbudowanego wsparcia dla akceptowania kodowania lub akceptowania języka).</span><span class="sxs-lookup"><span data-stu-id="1890d-115">(At this time, there is no built-in support for Accept-Encoding or Accept-Language.)</span></span>

## <a name="serialization"></a><span data-ttu-id="1890d-116">Serializacja</span><span class="sxs-lookup"><span data-stu-id="1890d-116">Serialization</span></span>

<span data-ttu-id="1890d-117">Jeśli kontroler internetowego interfejsu API zwraca zasób jako typ CLR, potok serializować wartość zwracaną i zapisuje ją w treści odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="1890d-117">If a Web API controller returns a resource as CLR type, the pipeline serializes the return value and writes it into the HTTP response body.</span></span>

<span data-ttu-id="1890d-118">Rozważmy na przykład następujące działanie kontrolera:</span><span class="sxs-lookup"><span data-stu-id="1890d-118">For example, consider the following controller action:</span></span>

[!code-csharp[Main](content-negotiation/samples/sample1.cs)]

<span data-ttu-id="1890d-119">Klient może wysłać to żądanie HTTP:</span><span class="sxs-lookup"><span data-stu-id="1890d-119">A client might send this HTTP request:</span></span>

[!code-console[Main](content-negotiation/samples/sample2.cmd)]

<span data-ttu-id="1890d-120">W odpowiedzi serwer może wysłać:</span><span class="sxs-lookup"><span data-stu-id="1890d-120">In response, the server might send:</span></span>

[!code-console[Main](content-negotiation/samples/sample3.cmd)]

<span data-ttu-id="1890d-121">W tym przykładzie klient zażądał elementu JSON, JavaScript lub "cokolwiek" (\*/\*).</span><span class="sxs-lookup"><span data-stu-id="1890d-121">In this example, the client requested either JSON, Javascript, or "anything" (\*/\*).</span></span> <span data-ttu-id="1890d-122">Serwer odpowiedział na reprezentację obiektu `Product` w formacie JSON.</span><span class="sxs-lookup"><span data-stu-id="1890d-122">The server responded with a JSON representation of the `Product` object.</span></span> <span data-ttu-id="1890d-123">Zwróć uwagę, że w nagłówku Content-Type w odpowiedzi jest ustawiona wartość &quot;Application/JSON&quot;.</span><span class="sxs-lookup"><span data-stu-id="1890d-123">Notice that the Content-Type header in the response is set to &quot;application/json&quot;.</span></span>

<span data-ttu-id="1890d-124">Kontroler może również zwracać obiekt **HttpResponseMessage** .</span><span class="sxs-lookup"><span data-stu-id="1890d-124">A controller can also return an **HttpResponseMessage** object.</span></span> <span data-ttu-id="1890d-125">Aby określić obiekt CLR dla treści odpowiedzi, wywołaj **metodę rozszerzającą** metody:</span><span class="sxs-lookup"><span data-stu-id="1890d-125">To specify a CLR object for the response body, call the **CreateResponse** extension method:</span></span>

[!code-csharp[Main](content-negotiation/samples/sample4.cs)]

<span data-ttu-id="1890d-126">Ta opcja zapewnia większą kontrolę nad szczegółami odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="1890d-126">This option gives you more control over the details of the response.</span></span> <span data-ttu-id="1890d-127">Można ustawić kod stanu, dodać nagłówki HTTP i tak dalej.</span><span class="sxs-lookup"><span data-stu-id="1890d-127">You can set the status code, add HTTP headers, and so forth.</span></span>

<span data-ttu-id="1890d-128">Obiekt, który serializować zasób jest nazywany elementem *formatującego multimediów*.</span><span class="sxs-lookup"><span data-stu-id="1890d-128">The object that serializes the resource is called a *media formatter*.</span></span> <span data-ttu-id="1890d-129">Elementy formatujące multimedia pochodzą z klasy **MediaTypeFormatter** .</span><span class="sxs-lookup"><span data-stu-id="1890d-129">Media formatters derive from the **MediaTypeFormatter** class.</span></span> <span data-ttu-id="1890d-130">Interfejs API sieci Web udostępnia elementy formatujące multimediów dla plików XML i JSON oraz można tworzyć niestandardowe elementy formatujące do obsługi innych typów multimediów.</span><span class="sxs-lookup"><span data-stu-id="1890d-130">Web API provides media formatters for XML and JSON, and you can create custom formatters to support other media types.</span></span> <span data-ttu-id="1890d-131">Aby uzyskać informacje na temat pisania niestandardowego programu formatującego, zobacz elementy [formatujące multimedia](media-formatters.md).</span><span class="sxs-lookup"><span data-stu-id="1890d-131">For information about writing a custom formatter, see [Media Formatters](media-formatters.md).</span></span>

## <a name="how-content-negotiation-works"></a><span data-ttu-id="1890d-132">Jak działa negocjowanie zawartości</span><span class="sxs-lookup"><span data-stu-id="1890d-132">How Content Negotiation Works</span></span>

<span data-ttu-id="1890d-133">Najpierw potok pobiera usługę **IContentNegotiator** z obiektu **HttpConfiguration** .</span><span class="sxs-lookup"><span data-stu-id="1890d-133">First, the pipeline gets the **IContentNegotiator** service from the **HttpConfiguration** object.</span></span> <span data-ttu-id="1890d-134">Pobiera również listę elementów formatujących multimedia z kolekcji **HttpConfiguration. formatującegos** .</span><span class="sxs-lookup"><span data-stu-id="1890d-134">It also gets the list of media formatters from the **HttpConfiguration.Formatters** collection.</span></span>

<span data-ttu-id="1890d-135">Następnie potok wywołuje **IContentNegotiator. Negotiate**, przekazując:</span><span class="sxs-lookup"><span data-stu-id="1890d-135">Next, the pipeline calls **IContentNegotiator.Negotiate**, passing in:</span></span>

- <span data-ttu-id="1890d-136">Typ obiektu do serializacji</span><span class="sxs-lookup"><span data-stu-id="1890d-136">The type of object to serialize</span></span>
- <span data-ttu-id="1890d-137">Kolekcja elementów formatujących multimedia</span><span class="sxs-lookup"><span data-stu-id="1890d-137">The collection of media formatters</span></span>
- <span data-ttu-id="1890d-138">Żądanie HTTP</span><span class="sxs-lookup"><span data-stu-id="1890d-138">The HTTP request</span></span>

<span data-ttu-id="1890d-139">Metoda **Negotiate** zwraca dwie części informacji:</span><span class="sxs-lookup"><span data-stu-id="1890d-139">The **Negotiate** method returns two pieces of information:</span></span>

- <span data-ttu-id="1890d-140">Którego programu formatującego użyć</span><span class="sxs-lookup"><span data-stu-id="1890d-140">Which formatter to use</span></span>
- <span data-ttu-id="1890d-141">Typ nośnika dla odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="1890d-141">The media type for the response</span></span>

<span data-ttu-id="1890d-142">Jeśli nie zostanie znaleziony żaden program formatujący, Metoda **Negotiate** zwraca **wartość null**, a klient odbiera błąd HTTP 406 (nieakceptowalny).</span><span class="sxs-lookup"><span data-stu-id="1890d-142">If no formatter is found, the **Negotiate** method returns **null**, and the client receives HTTP error 406 (Not Acceptable).</span></span>

<span data-ttu-id="1890d-143">Poniższy kod pokazuje, jak kontroler może bezpośrednio wywoływać negocjowanie zawartości:</span><span class="sxs-lookup"><span data-stu-id="1890d-143">The following code shows how a controller can directly invoke content negotiation:</span></span>

[!code-csharp[Main](content-negotiation/samples/sample5.cs)]

<span data-ttu-id="1890d-144">Ten kod jest odpowiednikiem działania potoku automatycznie.</span><span class="sxs-lookup"><span data-stu-id="1890d-144">This code is equivalent to the what the pipeline does automatically.</span></span>

## <a name="default-content-negotiator"></a><span data-ttu-id="1890d-145">Domyślny negocjuje zawartość</span><span class="sxs-lookup"><span data-stu-id="1890d-145">Default Content Negotiator</span></span>

<span data-ttu-id="1890d-146">Klasa **DefaultContentNegotiator** zapewnia domyślną implementację **IContentNegotiator**.</span><span class="sxs-lookup"><span data-stu-id="1890d-146">The **DefaultContentNegotiator** class provides the default implementation of **IContentNegotiator**.</span></span> <span data-ttu-id="1890d-147">Do wybrania programu formatującego są stosowane kilka kryteriów.</span><span class="sxs-lookup"><span data-stu-id="1890d-147">It uses several criteria to select a formatter.</span></span>

<span data-ttu-id="1890d-148">Najpierw program formatujący musi być w stanie serializować typ.</span><span class="sxs-lookup"><span data-stu-id="1890d-148">First, the formatter must be able to serialize the type.</span></span> <span data-ttu-id="1890d-149">Jest to weryfikowane przez wywołanie metody **MediaTypeFormatter. Unwritetype**.</span><span class="sxs-lookup"><span data-stu-id="1890d-149">This is verified by calling **MediaTypeFormatter.CanWriteType**.</span></span>

<span data-ttu-id="1890d-150">Następnie negocjuje zawartość analizuje każdy program formatujący i szacuje, jak dobrze pasuje do żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="1890d-150">Next, the content negotiator looks at each formatter and evaluates how well it matches the HTTP request.</span></span> <span data-ttu-id="1890d-151">Aby oszacować dopasowanie, negocjuje zawartość analizuje dwie rzeczy w programie formatującego:</span><span class="sxs-lookup"><span data-stu-id="1890d-151">To evaluate the match, the content negotiator looks at two things on the formatter:</span></span>

- <span data-ttu-id="1890d-152">Kolekcja **SupportedMediaTypes** , która zawiera listę obsługiwanych typów nośników.</span><span class="sxs-lookup"><span data-stu-id="1890d-152">The **SupportedMediaTypes** collection, which contains a list of supported media types.</span></span> <span data-ttu-id="1890d-153">Negocjowanie zawartości próbuje dopasować tę listę do nagłówka akceptowania żądań.</span><span class="sxs-lookup"><span data-stu-id="1890d-153">The content negotiator tries to match this list against the request Accept header.</span></span> <span data-ttu-id="1890d-154">Należy pamiętać, że nagłówek Accept może zawierać zakresy.</span><span class="sxs-lookup"><span data-stu-id="1890d-154">Note that the Accept header can include ranges.</span></span> <span data-ttu-id="1890d-155">Na przykład "tekst/zwykły" to dopasowanie do tekstu/\* lub \*/\*.</span><span class="sxs-lookup"><span data-stu-id="1890d-155">For example, "text/plain" is a match for text/\* or \*/\*.</span></span>
- <span data-ttu-id="1890d-156">Kolekcja **MediaTypeMappings** , która zawiera listę obiektów **MediaTypeMapping** .</span><span class="sxs-lookup"><span data-stu-id="1890d-156">The **MediaTypeMappings** collection, which contains a list of **MediaTypeMapping** objects.</span></span> <span data-ttu-id="1890d-157">Klasa **MediaTypeMapping** zapewnia ogólny sposób dopasowywania żądań HTTP z typami nośników.</span><span class="sxs-lookup"><span data-stu-id="1890d-157">The **MediaTypeMapping** class provides a generic way to match HTTP requests with media types.</span></span> <span data-ttu-id="1890d-158">Można na przykład zamapować niestandardowy nagłówek HTTP na określony typ nośnika.</span><span class="sxs-lookup"><span data-stu-id="1890d-158">For example, it could map a custom HTTP header to a particular media type.</span></span>

<span data-ttu-id="1890d-159">Jeśli istnieje wiele dopasowań, dopasowanie do najwyższego współczynnika jakości WINS.</span><span class="sxs-lookup"><span data-stu-id="1890d-159">If there are multiple matches, the match with the highest quality factor wins.</span></span> <span data-ttu-id="1890d-160">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="1890d-160">For example:</span></span>

[!code-console[Main](content-negotiation/samples/sample6.cmd)]

<span data-ttu-id="1890d-161">W tym przykładzie Application/JSON ma implikowany współczynnik jakości 1,0, więc jest preferowany w przypadku aplikacji/XML.</span><span class="sxs-lookup"><span data-stu-id="1890d-161">In this example, application/json has an implied quality factor of 1.0, so it is preferred over application/xml.</span></span>

<span data-ttu-id="1890d-162">Jeśli nie zostaną znalezione żadne dopasowania, wynegocjowanie zawartości próbuje dopasować typ nośnika treści żądania (jeśli istnieje).</span><span class="sxs-lookup"><span data-stu-id="1890d-162">If no matches are found, the content negotiator tries to match on the media type of the request body, if any.</span></span> <span data-ttu-id="1890d-163">Na przykład, jeśli żądanie zawiera dane JSON, negocjuje zawartość będzie szukać programu formatującego w formacie JSON.</span><span class="sxs-lookup"><span data-stu-id="1890d-163">For example, if the request contains JSON data, the content negotiator looks for a JSON formatter.</span></span>

<span data-ttu-id="1890d-164">Jeśli nadal nie ma dopasowania, negocjuje zawartość po prostu wybiera pierwszy program formatujący, który może serializować typ.</span><span class="sxs-lookup"><span data-stu-id="1890d-164">If there are still no matches, the content negotiator simply picks the first formatter that can serialize the type.</span></span>

## <a name="selecting-a-character-encoding"></a><span data-ttu-id="1890d-165">Wybieranie kodowania znaków</span><span class="sxs-lookup"><span data-stu-id="1890d-165">Selecting a Character Encoding</span></span>

<span data-ttu-id="1890d-166">Po wybraniu programu formatującego zawartość jest wybierana przy użyciu właściwości **SupportedEncodings** w programie formatującego i pasuje do nagłówka Accept-Charset w żądaniu (jeśli istnieje).</span><span class="sxs-lookup"><span data-stu-id="1890d-166">After a formatter is selected, the content negotiator chooses the best character encoding by looking at the **SupportedEncodings** property on the formatter, and matching it against the Accept-Charset header in the request (if any).</span></span>

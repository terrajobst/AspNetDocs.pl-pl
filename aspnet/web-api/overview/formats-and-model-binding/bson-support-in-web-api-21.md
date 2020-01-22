---
uid: web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
title: Obsługa BSON w ASP.NET Web API 2,1-ASP.NET 4. x
author: MikeWasson
description: pokazuje, jak używać BSON w kontrolerze internetowego interfejsu API (po stronie serwera) i w aplikacji klienckiej platformy .NET dla ASP.NET 4. x.
ms.author: riande
ms.date: 01/20/2014
ms.custom: seoapril2019
ms.assetid: ce11b017-0ca6-4376-aa9d-a7f3288101de
msc.legacyurl: /web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: ccbc0372120301b1cd8d4cdc86bd9fba9404d8ae
ms.sourcegitcommit: 88fc80e3f65aebdf61ec9414810ddbc31c543f04
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/22/2020
ms.locfileid: "76519336"
---
# <a name="bson-support-in-aspnet-web-api-21"></a><span data-ttu-id="c1f35-103">Obsługa BSON w ASP.NET Web API 2,1</span><span class="sxs-lookup"><span data-stu-id="c1f35-103">BSON Support in ASP.NET Web API 2.1</span></span>

<span data-ttu-id="c1f35-104">według [Jan Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="c1f35-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="c1f35-105">W tym temacie pokazano, jak używać BSON na kontrolerze internetowego interfejsu API (po stronie serwera) i w aplikacji klienckiej platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="c1f35-105">This topic shows how to use BSON in your Web API controller (server side) and in a .NET client app.</span></span> <span data-ttu-id="c1f35-106">Internetowy interfejs API 2,1 wprowadza obsługę BSON.</span><span class="sxs-lookup"><span data-stu-id="c1f35-106">Web API 2.1 introduces support for BSON.</span></span> 

## <a name="what-is-bson"></a><span data-ttu-id="c1f35-107">Co to jest BSON?</span><span class="sxs-lookup"><span data-stu-id="c1f35-107">What is BSON?</span></span>

<span data-ttu-id="c1f35-108">[BSON](http://bsonspec.org/) to binarny format serializacji.</span><span class="sxs-lookup"><span data-stu-id="c1f35-108">[BSON](http://bsonspec.org/) is a binary serialization format.</span></span> <span data-ttu-id="c1f35-109">"BSON" oznacza "binarny kod JSON", ale BSON i JSON są serializowane bardzo inaczej.</span><span class="sxs-lookup"><span data-stu-id="c1f35-109">"BSON" stands for "Binary JSON", but BSON and JSON are serialized very differently.</span></span> <span data-ttu-id="c1f35-110">BSON jest "notacją JSON", ponieważ obiekty są reprezentowane jako pary nazwa-wartość, podobnie jak JSON.</span><span class="sxs-lookup"><span data-stu-id="c1f35-110">BSON is "JSON-like", because objects are represented as name-value pairs, similar to JSON.</span></span> <span data-ttu-id="c1f35-111">W przeciwieństwie do formatu JSON, liczbowe typy danych są przechowywane jako bajty, a nie ciągi</span><span class="sxs-lookup"><span data-stu-id="c1f35-111">Unlike JSON, numeric data types are stored as bytes, not strings</span></span>

<span data-ttu-id="c1f35-112">BSON została zaprojektowana jako uproszczona, łatwa do skanowania i szybka do kodowania/dekodowania.</span><span class="sxs-lookup"><span data-stu-id="c1f35-112">BSON was designed to be lightweight, easy to scan, and fast to encode/decode.</span></span>

- <span data-ttu-id="c1f35-113">BSON jest porównywalny w rozmiarze do formatu JSON.</span><span class="sxs-lookup"><span data-stu-id="c1f35-113">BSON is comparable in size to JSON.</span></span> <span data-ttu-id="c1f35-114">W zależności od danych ładunek BSON może być mniejszy lub większy od ładunku JSON.</span><span class="sxs-lookup"><span data-stu-id="c1f35-114">Depending on the data, a BSON payload may be smaller or larger than a JSON payload.</span></span> <span data-ttu-id="c1f35-115">W przypadku serializowania danych binarnych, takich jak plik obrazu, BSON jest mniejszy niż format JSON, ponieważ dane binarne nie są zakodowane w formacie base64.</span><span class="sxs-lookup"><span data-stu-id="c1f35-115">For serializing binary data, such as an image file, BSON is smaller than JSON, because the binary data is not base64-encoded.</span></span>
- <span data-ttu-id="c1f35-116">BSON dokumenty są łatwe do skanowania, ponieważ elementy są poprzedzone prefiksem pola długości, dzięki czemu Analizator może pominąć elementy bez dekodowania.</span><span class="sxs-lookup"><span data-stu-id="c1f35-116">BSON documents are easy to scan because elements are prefixed with a length field, so a parser can skip elements without decoding them.</span></span>
- <span data-ttu-id="c1f35-117">Kodowanie i dekodowanie są wydajne, ponieważ typy danych liczbowych są przechowywane jako liczby, a nie ciągi.</span><span class="sxs-lookup"><span data-stu-id="c1f35-117">Encoding and decoding are efficient, because numeric data types are stored as numbers, not strings.</span></span>

<span data-ttu-id="c1f35-118">Natywni Klienci, na przykład aplikacje klienckie platformy .NET, mogą korzystać z BSON zamiast formatów tekstowych, takich jak JSON lub XML.</span><span class="sxs-lookup"><span data-stu-id="c1f35-118">Native clients, such as .NET client apps, can benefit from using BSON in place of text-based formats such as JSON or XML.</span></span> <span data-ttu-id="c1f35-119">W przypadku klientów korzystających z przeglądarki prawdopodobnie warto naklejić za pomocą formatu JSON, ponieważ kod JavaScript może bezpośrednio przekonwertować ładunek JSON.</span><span class="sxs-lookup"><span data-stu-id="c1f35-119">For browser clients, you will probably want to stick with JSON, because JavaScript can directly convert the JSON payload.</span></span>

<span data-ttu-id="c1f35-120">Na szczęście interfejs API sieci Web używa [negocjacji zawartości](content-negotiation.md), więc interfejs API może obsługiwać oba formaty i pozwolić na wybór klienta.</span><span class="sxs-lookup"><span data-stu-id="c1f35-120">Fortunately, Web API uses [content negotiation](content-negotiation.md), so your API can support both formats and let the client choose.</span></span>

## <a name="enabling-bson-on-the-server"></a><span data-ttu-id="c1f35-121">Włączanie BSON na serwerze</span><span class="sxs-lookup"><span data-stu-id="c1f35-121">Enabling BSON on the Server</span></span>

<span data-ttu-id="c1f35-122">W konfiguracji internetowego interfejsu API Dodaj **BsonMediaTypeFormatter** do kolekcji programu formatującego.</span><span class="sxs-lookup"><span data-stu-id="c1f35-122">In your Web API configuration, add the **BsonMediaTypeFormatter** to the formatters collection.</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample1.cs)]

<span data-ttu-id="c1f35-123">Teraz jeśli klient żąda aplikacji "Application/BSON", internetowy interfejs API będzie używać programu formatującego BSON.</span><span class="sxs-lookup"><span data-stu-id="c1f35-123">Now if the client requests "application/bson", Web API will use the BSON formatter.</span></span>

<span data-ttu-id="c1f35-124">Aby skojarzyć BSON z innymi typami nośników, Dodaj je do kolekcji SupportedMediaTypes.</span><span class="sxs-lookup"><span data-stu-id="c1f35-124">To associate BSON with other media types, add them to the SupportedMediaTypes collection.</span></span> <span data-ttu-id="c1f35-125">Poniższy kod dodaje "application/vnd. contoso" do obsługiwanych typów multimediów:</span><span class="sxs-lookup"><span data-stu-id="c1f35-125">The following code adds "application/vnd.contoso" to the supported media types:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample2.cs)]

## <a name="example-http-session"></a><span data-ttu-id="c1f35-126">Przykładowa sesja HTTP</span><span class="sxs-lookup"><span data-stu-id="c1f35-126">Example HTTP Session</span></span>

<span data-ttu-id="c1f35-127">W tym przykładzie użyjemy następującej klasy modelu i prostego kontrolera interfejsu API sieci Web:</span><span class="sxs-lookup"><span data-stu-id="c1f35-127">For this example, we'll use the following model class plus a simple Web API controller:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample3.cs)]

<span data-ttu-id="c1f35-128">Klient może wysłać następujące żądanie HTTP:</span><span class="sxs-lookup"><span data-stu-id="c1f35-128">A client might send the following HTTP request:</span></span>

[!code-console[Main](bson-support-in-web-api-21/samples/sample4.cmd)]

<span data-ttu-id="c1f35-129">Oto odpowiedź:</span><span class="sxs-lookup"><span data-stu-id="c1f35-129">Here is the response:</span></span>

[!code-console[Main](bson-support-in-web-api-21/samples/sample5.cmd)]

<span data-ttu-id="c1f35-130">W tym miejscu zamieniono dane binarne na &quot;.&quot; znaków.</span><span class="sxs-lookup"><span data-stu-id="c1f35-130">Here I've replaced the binary data with &quot;.&quot; characters.</span></span> <span data-ttu-id="c1f35-131">Poniższy zrzut ekranu z programu Fiddler pokazuje pierwotne wartości szesnastkowe.</span><span class="sxs-lookup"><span data-stu-id="c1f35-131">The following screen shot from Fiddler shows the raw hex values.</span></span>

[![](bson-support-in-web-api-21/_static/image2.png)](bson-support-in-web-api-21/_static/image1.png)

## <a name="using-bson-with-httpclient"></a><span data-ttu-id="c1f35-132">Używanie BSON z HttpClient</span><span class="sxs-lookup"><span data-stu-id="c1f35-132">Using BSON with HttpClient</span></span>

<span data-ttu-id="c1f35-133">Aplikacje klienckie platformy .NET mogą używać programu formatującego BSON z **HttpClient**.</span><span class="sxs-lookup"><span data-stu-id="c1f35-133">.NET clients apps can use the BSON formatter with **HttpClient**.</span></span> <span data-ttu-id="c1f35-134">Aby uzyskać więcej informacji na temat **HttpClient**, zobacz [wywoływanie interfejsu API sieci Web z klienta platformy .NET](../advanced/calling-a-web-api-from-a-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="c1f35-134">For more information about **HttpClient**, see [Calling a Web API From a .NET Client](../advanced/calling-a-web-api-from-a-net-client.md).</span></span>

<span data-ttu-id="c1f35-135">Poniższy kod wysyła żądanie GET, które akceptuje BSON, a następnie deserializacji ładunku BSON w odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="c1f35-135">The following code sends a GET request that accepts BSON, and then deserializes the BSON payload in the response.</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample6.cs)]

<span data-ttu-id="c1f35-136">Aby zażądać BSON z serwera, ustaw nagłówek Accept na "Application/BSON":</span><span class="sxs-lookup"><span data-stu-id="c1f35-136">To request BSON from the server, set the Accept header to "application/bson":</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample7.cs)]

<span data-ttu-id="c1f35-137">Aby zdeserializować treści odpowiedzi, użyj **BsonMediaTypeFormatter**.</span><span class="sxs-lookup"><span data-stu-id="c1f35-137">To deserialize the response body, use the **BsonMediaTypeFormatter**.</span></span> <span data-ttu-id="c1f35-138">Ten program formatujący nie znajduje się w domyślnej kolekcji programu formatującego, więc musisz go określić podczas odczytywania treści odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="c1f35-138">This formatter is not in the default formatters collection, so you have to specify it when you read the response body:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample8.cs)]

<span data-ttu-id="c1f35-139">W następnym przykładzie pokazano, jak wysłać żądanie POST zawierające BSON.</span><span class="sxs-lookup"><span data-stu-id="c1f35-139">The next example shows how to send a POST request that contains BSON.</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample9.cs)]

<span data-ttu-id="c1f35-140">Większość tego kodu jest taka sama jak w poprzednim przykładzie.</span><span class="sxs-lookup"><span data-stu-id="c1f35-140">Much of this code is the same as the previous example.</span></span> <span data-ttu-id="c1f35-141">Ale w metodzie **PostAsync** należy określić **BsonMediaTypeFormatter** jako program formatujący:</span><span class="sxs-lookup"><span data-stu-id="c1f35-141">But in the **PostAsync** method, specify **BsonMediaTypeFormatter** as the formatter:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample10.cs)]

## <a name="serializing-top-level-primitive-types"></a><span data-ttu-id="c1f35-142">Serializowanie typów pierwotnych najwyższego poziomu</span><span class="sxs-lookup"><span data-stu-id="c1f35-142">Serializing Top-Level Primitive Types</span></span>

<span data-ttu-id="c1f35-143">Każdy dokument BSON jest listą par klucz/wartość. Specyfikacja BSON nie definiuje składni do serializacji pojedynczej nieprzetworzonej wartości, takiej jak liczba całkowita lub ciąg.</span><span class="sxs-lookup"><span data-stu-id="c1f35-143">Every BSON document is a list of key/value pairs.The BSON specification does not define a syntax for serializing a single raw value, such as an integer or string.</span></span>

<span data-ttu-id="c1f35-144">Aby obejść to ograniczenie, **BsonMediaTypeFormatter** traktuje pierwotne typy jako przypadek specjalny.</span><span class="sxs-lookup"><span data-stu-id="c1f35-144">To work around this limitation, the **BsonMediaTypeFormatter** treats primitive types as a special case.</span></span> <span data-ttu-id="c1f35-145">Przed serializowaniem, konwertuje wartość na parę klucz/wartość z kluczem "value".</span><span class="sxs-lookup"><span data-stu-id="c1f35-145">Before serializing, it converts the value into a key/value pair with the key "Value".</span></span> <span data-ttu-id="c1f35-146">Załóżmy na przykład, że kontroler interfejsu API zwraca liczbę całkowitą:</span><span class="sxs-lookup"><span data-stu-id="c1f35-146">For example, suppose your API controller returns an integer:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample11.cs)]

<span data-ttu-id="c1f35-147">Przed serializowaniem, program formatujący BSON konwertuje ten format na następującą parę klucz/wartość:</span><span class="sxs-lookup"><span data-stu-id="c1f35-147">Before serializing, the BSON formatter converts this to the following key/value pair:</span></span>

[!code-json[Main](bson-support-in-web-api-21/samples/sample12.json)]

<span data-ttu-id="c1f35-148">Podczas deserializacji, program formatujący konwertuje dane z powrotem na pierwotną wartość.</span><span class="sxs-lookup"><span data-stu-id="c1f35-148">When you deserialize, the formatter converts the data back to the original value.</span></span> <span data-ttu-id="c1f35-149">Jednak klienci korzystający z innego parsera BSON będą musieli obsłużyć ten przypadek, jeśli interfejs API sieci Web zwróci nieprzetworzone wartości.</span><span class="sxs-lookup"><span data-stu-id="c1f35-149">However, clients using a different BSON parser will need to handle this case, if your web API returns raw values.</span></span> <span data-ttu-id="c1f35-150">Ogólnie rzecz biorąc, należy rozważyć zwrócenie danych strukturalnych, a nie nieprzetworzonych wartości.</span><span class="sxs-lookup"><span data-stu-id="c1f35-150">In general, you should consider returning structured data, rather than raw values.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c1f35-151">Dodatkowe materiały</span><span class="sxs-lookup"><span data-stu-id="c1f35-151">Additional Resources</span></span>

[<span data-ttu-id="c1f35-152">Przykład BSON internetowego interfejsu API</span><span class="sxs-lookup"><span data-stu-id="c1f35-152">Web API BSON Sample</span></span>](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/BSONSample/)

[<span data-ttu-id="c1f35-153">Elementy formatujące multimedia</span><span class="sxs-lookup"><span data-stu-id="c1f35-153">Media Formatters</span></span>](media-formatters.md)

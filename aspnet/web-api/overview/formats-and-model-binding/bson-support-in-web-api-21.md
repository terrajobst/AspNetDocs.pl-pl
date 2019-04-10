---
uid: web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
title: Obsługa formatu BSON we wzorcu ASP.NET Web API 2.1 — ASP.NET 4.x
author: MikeWasson
description: ilustruje sposób używania formatu BSON w kontrolerze interfejsu API sieci Web (po stronie serwera) i w aplikacji klienta .NET dla platformy ASP.NET 4.x.
ms.author: riande
ms.date: 01/20/2014
ms.custom: seoapril2019
ms.assetid: ce11b017-0ca6-4376-aa9d-a7f3288101de
msc.legacyurl: /web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: 911e2abcfd277075b3cba71e624ec6390b99a15e
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/09/2019
ms.locfileid: "59382235"
---
# <a name="bson-support-in-aspnet-web-api-21"></a><span data-ttu-id="25e85-103">Obsługa formatu BSON we wzorcu ASP.NET Web API 2.1</span><span class="sxs-lookup"><span data-stu-id="25e85-103">BSON Support in ASP.NET Web API 2.1</span></span>

<span data-ttu-id="25e85-104">przez [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="25e85-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="25e85-105">W tym temacie pokazano, jak używać formatu BSON w kontrolerze interfejsu API sieci Web (po stronie serwera) i w aplikacji klienckiej .NET.</span><span class="sxs-lookup"><span data-stu-id="25e85-105">This topic shows how to use BSON in your Web API controller (server side) and in a .NET client app.</span></span> <span data-ttu-id="25e85-106">Składnik Web API 2.1 wprowadzono obsługę formatu BSON.</span><span class="sxs-lookup"><span data-stu-id="25e85-106">Web API 2.1 introduces support for BSON.</span></span> 

## <a name="what-is-bson"></a><span data-ttu-id="25e85-107">Co to jest BSON?</span><span class="sxs-lookup"><span data-stu-id="25e85-107">What is BSON?</span></span>

<span data-ttu-id="25e85-108">[BSON](http://bsonspec.org/) to binarny format serializacji.</span><span class="sxs-lookup"><span data-stu-id="25e85-108">[BSON](http://bsonspec.org/) is a binary serialization format.</span></span> <span data-ttu-id="25e85-109">"BSON" oznacza "Binarny JSON", ale są bardzo różny sposób serializacji formatu BSON i JSON.</span><span class="sxs-lookup"><span data-stu-id="25e85-109">"BSON" stands for "Binary JSON", but BSON and JSON are serialized very differently.</span></span> <span data-ttu-id="25e85-110">Jest BSON "Notacji JSON", ponieważ obiekty są reprezentowane jako pary nazwa wartość, podobny do formatu JSON.</span><span class="sxs-lookup"><span data-stu-id="25e85-110">BSON is "JSON-like", because objects are represented as name-value pairs, similar to JSON.</span></span> <span data-ttu-id="25e85-111">W przeciwieństwie do formatu JSON numeryczne typy danych są przechowywane w postaci bajtów, a nie ciągi</span><span class="sxs-lookup"><span data-stu-id="25e85-111">Unlike JSON, numeric data types are stored as bytes, not strings</span></span>

<span data-ttu-id="25e85-112">BSON został zaprojektowany tak, to lekki, łatwość skanowania oraz szybkość kodowania dekodowania.</span><span class="sxs-lookup"><span data-stu-id="25e85-112">BSON was designed to be lightweight, easy to scan, and fast to encode/decode.</span></span>

- <span data-ttu-id="25e85-113">BSON jest porównywalny rozmiar do formatu JSON.</span><span class="sxs-lookup"><span data-stu-id="25e85-113">BSON is comparable in size to JSON.</span></span> <span data-ttu-id="25e85-114">W zależności od danych ładunek BSON może być mniejszy lub większy od ładunku JSON.</span><span class="sxs-lookup"><span data-stu-id="25e85-114">Depending on the data, a BSON payload may be smaller or larger than a JSON payload.</span></span> <span data-ttu-id="25e85-115">Dla serializacji danych binarnych, takich jak plik obrazu BSON jest mniejszy niż JSON, dane, ponieważ dane binarne nie jest zakodowany w formacie base64.</span><span class="sxs-lookup"><span data-stu-id="25e85-115">For serializing binary data, such as an image file, BSON is smaller than JSON, because the binary data is not base64-encoded.</span></span>
- <span data-ttu-id="25e85-116">BSON dokumenty są łatwe do skanowania, ponieważ elementy mają prefiks pola długości, dlatego analizatorem przejść elementów bez ich dekodowania.</span><span class="sxs-lookup"><span data-stu-id="25e85-116">BSON documents are easy to scan because elements are prefixed with a length field, so a parser can skip elements without decoding them.</span></span>
- <span data-ttu-id="25e85-117">Kodowania i dekodowania są wydajne, ponieważ typy danych liczbowych są przechowywane jako liczby, ciągi nie.</span><span class="sxs-lookup"><span data-stu-id="25e85-117">Encoding and decoding are efficient, because numeric data types are stored as numbers, not strings.</span></span>

<span data-ttu-id="25e85-118">Natywne klientów, takich jak aplikacje klienckie .NET, mogą korzystać z formatu BSON zamiast oparte na tekście formatów, takich jak JSON lub XML.</span><span class="sxs-lookup"><span data-stu-id="25e85-118">Native clients, such as .NET client apps, can benefit from using BSON in place of text-based formats such as JSON or XML.</span></span> <span data-ttu-id="25e85-119">Dla klientów w przeglądarkach prawdopodobnie można działać zgodnie z formatu JSON, ponieważ usługa języka JavaScript bezpośrednio można przekonwertować ładunek JSON.</span><span class="sxs-lookup"><span data-stu-id="25e85-119">For browser clients, you will probably want to stick with JSON, because JavaScript can directly convert the JSON payload.</span></span>

<span data-ttu-id="25e85-120">Na szczęście usługa korzysta z interfejsu API sieci Web [negocjacji zawartości](content-negotiation.md), więc może obsługiwać obu formatów i zezwala komputerom, wybierz interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="25e85-120">Fortunately, Web API uses [content negotiation](content-negotiation.md), so your API can support both formats and let the client choose.</span></span>

## <a name="enabling-bson-on-the-server"></a><span data-ttu-id="25e85-121">Włączanie BSON na serwerze</span><span class="sxs-lookup"><span data-stu-id="25e85-121">Enabling BSON on the Server</span></span>

<span data-ttu-id="25e85-122">W konfiguracji interfejsu API sieci Web, Dodaj **BsonMediaTypeFormatter** do kolekcji elementów formatujących.</span><span class="sxs-lookup"><span data-stu-id="25e85-122">In your Web API configuration, add the **BsonMediaTypeFormatter** to the formatters collection.</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample1.cs)]

<span data-ttu-id="25e85-123">Teraz jeśli zażąda "application/bson" interfejsu API sieci Web zostanie użyty program formatujący formatu BSON.</span><span class="sxs-lookup"><span data-stu-id="25e85-123">Now if the client requests "application/bson", Web API will use the BSON formatter.</span></span>

<span data-ttu-id="25e85-124">Aby skojarzyć BSON z innych typów nośników, należy dodać je do kolekcji SupportedMediaTypes.</span><span class="sxs-lookup"><span data-stu-id="25e85-124">To associate BSON with other media types, add them to the SupportedMediaTypes collection.</span></span> <span data-ttu-id="25e85-125">Poniższy kod dodaje "application/vnd.contoso" do typów nośników obsługiwanych:</span><span class="sxs-lookup"><span data-stu-id="25e85-125">The following code adds "application/vnd.contoso" to the supported media types:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample2.cs)]

## <a name="example-http-session"></a><span data-ttu-id="25e85-126">Przykład sesji HTTP</span><span class="sxs-lookup"><span data-stu-id="25e85-126">Example HTTP Session</span></span>

<span data-ttu-id="25e85-127">W tym przykładzie użyjemy następujące klasy modelu, a także proste kontrolera interfejsu API sieci Web:</span><span class="sxs-lookup"><span data-stu-id="25e85-127">For this example, we'll use the following model class plus a simple Web API controller:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample3.cs)]

<span data-ttu-id="25e85-128">Klient może wysłać poniższe żądanie HTTP:</span><span class="sxs-lookup"><span data-stu-id="25e85-128">A client might send the following HTTP request:</span></span>

[!code-console[Main](bson-support-in-web-api-21/samples/sample4.cmd)]

<span data-ttu-id="25e85-129">Poniżej przedstawiono odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="25e85-129">Here is the response:</span></span>

[!code-console[Main](bson-support-in-web-api-21/samples/sample5.cmd)]

<span data-ttu-id="25e85-130">W tym miejscu został zastąpiony danymi binarnymi o &quot;.&quot; znaków.</span><span class="sxs-lookup"><span data-stu-id="25e85-130">Here I've replaced the binary data with &quot;.&quot; characters.</span></span> <span data-ttu-id="25e85-131">Zrzut ekranu z następujących z programu Fiddler pokazuje pierwotne wartości szesnastkowych.</span><span class="sxs-lookup"><span data-stu-id="25e85-131">The following screen shot from Fiddler shows the raw hex values.</span></span>

[![](bson-support-in-web-api-21/_static/image2.png)](bson-support-in-web-api-21/_static/image1.png)

## <a name="using-bson-with-httpclient"></a><span data-ttu-id="25e85-132">Przy użyciu formatu BSON z HttpClient</span><span class="sxs-lookup"><span data-stu-id="25e85-132">Using BSON with HttpClient</span></span>

<span data-ttu-id="25e85-133">Aplikacje klientów platformy .NET mogą używać formatu BSON element formatujący **HttpClient**.</span><span class="sxs-lookup"><span data-stu-id="25e85-133">.NET clients apps can use the BSON formatter with **HttpClient**.</span></span> <span data-ttu-id="25e85-134">Aby uzyskać więcej informacji na temat **HttpClient**, zobacz [wywołania sieci Web interfejsu API z klienta programu .NET](../advanced/calling-a-web-api-from-a-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="25e85-134">For more information about **HttpClient**, see [Calling a Web API From a .NET Client](../advanced/calling-a-web-api-from-a-net-client.md).</span></span>

<span data-ttu-id="25e85-135">Poniższy kod wysyła żądanie GET, które akceptuje formatu BSON i deserializuje ładunek BSON w odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="25e85-135">The following code sends a GET request that accepts BSON, and then deserializes the BSON payload in the response.</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample6.cs)]

<span data-ttu-id="25e85-136">Aby zażądać BSON z serwera, ustawić nagłówek Accept do "application/bson":</span><span class="sxs-lookup"><span data-stu-id="25e85-136">To request BSON from the server, set the Accept header to "application/bson":</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample7.cs)]

<span data-ttu-id="25e85-137">Do deserializacji treści odpowiedzi, należy użyć **BsonMediaTypeFormatter**.</span><span class="sxs-lookup"><span data-stu-id="25e85-137">To deserialize the response body, use the **BsonMediaTypeFormatter**.</span></span> <span data-ttu-id="25e85-138">Ten program formatujący nie jest w domyślnej kolekcji elementów formatujących, więc trzeba je wprowadzić podczas odczytu treści odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="25e85-138">This formatter is not in the default formatters collection, so you have to specify it when you read the response body:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample8.cs)]

<span data-ttu-id="25e85-139">Następny przykład przedstawia sposób wysłania żądania POST, która zawiera formatu BSON.</span><span class="sxs-lookup"><span data-stu-id="25e85-139">The next example shows how to send a POST request that contains BSON.</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample9.cs)]

<span data-ttu-id="25e85-140">Większość tego kodu jest taka sama, jak w poprzednim przykładzie.</span><span class="sxs-lookup"><span data-stu-id="25e85-140">Much of this code is the same as the previous example.</span></span> <span data-ttu-id="25e85-141">Ale w **PostAsync** metody, określ **BsonMediaTypeFormatter** jako element formatujący:</span><span class="sxs-lookup"><span data-stu-id="25e85-141">But in the **PostAsync** method, specify **BsonMediaTypeFormatter** as the formatter:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample10.cs)]

## <a name="serializing-top-level-primitive-types"></a><span data-ttu-id="25e85-142">Serializacja typów pierwotnych najwyższego poziomu</span><span class="sxs-lookup"><span data-stu-id="25e85-142">Serializing Top-Level Primitive Types</span></span>

<span data-ttu-id="25e85-143">Każdy dokument BSON jest listą par klucz/wartość. Specyfikacja formatu BSON nie definiuje składni dla serializacji pojedynczej wartości pierwotnych, takich jak typu integer lub string.</span><span class="sxs-lookup"><span data-stu-id="25e85-143">Every BSON document is a list of key/value pairs.The BSON specification does not define a syntax for serializing a single raw value, such as an integer or string.</span></span>

<span data-ttu-id="25e85-144">Aby obejść to ograniczenie **BsonMediaTypeFormatter** traktuje typów pierwotnych w szczególnych przypadkach.</span><span class="sxs-lookup"><span data-stu-id="25e85-144">To work around this limitation, the **BsonMediaTypeFormatter** treats primitive types as a special case.</span></span> <span data-ttu-id="25e85-145">Przed rozpoczęciem serializacji, konwertuje wartość w parę klucza i wartości z kluczem "Value".</span><span class="sxs-lookup"><span data-stu-id="25e85-145">Before serializing, it converts the value into a key/value pair with the key "Value".</span></span> <span data-ttu-id="25e85-146">Na przykład załóżmy, że dany kontroler interfejsu API zwraca liczbę całkowitą:</span><span class="sxs-lookup"><span data-stu-id="25e85-146">For example, suppose your API controller returns an integer:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample11.cs)]

<span data-ttu-id="25e85-147">Przed rozpoczęciem serializacji, element formatujący BSON konwertuje to następujące pary klucz/wartość:</span><span class="sxs-lookup"><span data-stu-id="25e85-147">Before serializing, the BSON formatter converts this to the following key/value pair:</span></span>

[!code-json[Main](bson-support-in-web-api-21/samples/sample12.json)]

<span data-ttu-id="25e85-148">Podczas deserializacji, element formatujący konwertuje je do oryginalnej wartości.</span><span class="sxs-lookup"><span data-stu-id="25e85-148">When you deserialize, the formatter converts the data back to the original value.</span></span> <span data-ttu-id="25e85-149">Jednak jeśli internetowy interfejs API zwraca wartości w wierszach klientów przy użyciu innego analizatora BSON należy się do obsługi tej sprawy.</span><span class="sxs-lookup"><span data-stu-id="25e85-149">However, clients using a different BSON parser will need to handle this case, if your web API returns raw values.</span></span> <span data-ttu-id="25e85-150">Ogólnie rzecz biorąc należy rozważyć, zwracając ustrukturyzowanych danych, a nie wartości w wierszach.</span><span class="sxs-lookup"><span data-stu-id="25e85-150">In general, you should consider returning structured data, rather than raw values.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="25e85-151">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="25e85-151">Additional Resources</span></span>

[<span data-ttu-id="25e85-152">Przykład formatu BSON interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="25e85-152">Web API BSON Sample</span></span>](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/)

[<span data-ttu-id="25e85-153">Programy formatujące multimedia</span><span class="sxs-lookup"><span data-stu-id="25e85-153">Media Formatters</span></span>](media-formatters.md)

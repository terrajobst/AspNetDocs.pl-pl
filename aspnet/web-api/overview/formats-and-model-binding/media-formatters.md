---
uid: web-api/overview/formats-and-model-binding/media-formatters
title: Elementy formatujące multimedia w ASP.NET Web API 2-ASP.NET 4. x
author: MikeWasson
description: Pokazuje, jak obsługiwać dodatkowe formaty multimediów w ASP.NET Web API for ASP.NET 4. x.
ms.author: riande
ms.date: 01/20/2014
ms.custom: seoapril2019
ms.assetid: 4c56f64a-086a-44ce-99c2-4c69604cd7fd
msc.legacyurl: /web-api/overview/formats-and-model-binding/media-formatters
msc.type: authoredcontent
ms.openlocfilehash: da0c566dad302054d7d0a6435e4c6df178c64772
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557254"
---
# <a name="media-formatters-in-aspnet-web-api-2"></a><span data-ttu-id="dcc2d-103">Elementy formatujące multimedia w ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="dcc2d-103">Media Formatters in ASP.NET Web API 2</span></span>

<span data-ttu-id="dcc2d-104">według [Jan Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="dcc2d-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="dcc2d-105">W tym samouczku pokazano, jak obsługiwać dodatkowe formaty multimediów w interfejsie Web API ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="dcc2d-105">This tutorial shows how to support additional media formats in ASP.NET Web API.</span></span>

## <a name="internet-media-types"></a><span data-ttu-id="dcc2d-106">Typy multimediów internetowych</span><span class="sxs-lookup"><span data-stu-id="dcc2d-106">Internet Media Types</span></span>

<span data-ttu-id="dcc2d-107">Typ nośnika, nazywany również typem MIME, identyfikuje format fragmentu danych.</span><span class="sxs-lookup"><span data-stu-id="dcc2d-107">A media type, also called a MIME type, identifies the format of a piece of data.</span></span> <span data-ttu-id="dcc2d-108">W przypadku protokołu HTTP typy nośników opisują format treści wiadomości.</span><span class="sxs-lookup"><span data-stu-id="dcc2d-108">In HTTP, media types describe the format of the message body.</span></span> <span data-ttu-id="dcc2d-109">Typ nośnika składa się z dwóch ciągów, typu i podtypu.</span><span class="sxs-lookup"><span data-stu-id="dcc2d-109">A media type consists of two strings, a type and a subtype.</span></span> <span data-ttu-id="dcc2d-110">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="dcc2d-110">For example:</span></span>

- <span data-ttu-id="dcc2d-111">tekst/HTML</span><span class="sxs-lookup"><span data-stu-id="dcc2d-111">text/html</span></span>
- <span data-ttu-id="dcc2d-112">image/png</span><span class="sxs-lookup"><span data-stu-id="dcc2d-112">image/png</span></span>
- <span data-ttu-id="dcc2d-113">application/json</span><span class="sxs-lookup"><span data-stu-id="dcc2d-113">application/json</span></span>

<span data-ttu-id="dcc2d-114">Gdy komunikat HTTP zawiera treść jednostki, nagłówek Content-Type określa format treści komunikatu.</span><span class="sxs-lookup"><span data-stu-id="dcc2d-114">When an HTTP message contains an entity-body, the Content-Type header specifies the format of the message body.</span></span> <span data-ttu-id="dcc2d-115">To nakazuje odbiorcy analizowanie zawartości treści wiadomości.</span><span class="sxs-lookup"><span data-stu-id="dcc2d-115">This tells the receiver how to parse the contents of the message body.</span></span>

<span data-ttu-id="dcc2d-116">Na przykład, jeśli odpowiedź HTTP zawiera obraz PNG, odpowiedzi mogą mieć następujące nagłówki.</span><span class="sxs-lookup"><span data-stu-id="dcc2d-116">For example, if an HTTP response contains a PNG image, the response might have the following headers.</span></span>

[!code-console[Main](media-formatters/samples/sample1.cmd)]

<span data-ttu-id="dcc2d-117">Gdy klient wysyła komunikat żądania, może zawierać nagłówek Accept.</span><span class="sxs-lookup"><span data-stu-id="dcc2d-117">When the client sends a request message, it can include an Accept header.</span></span> <span data-ttu-id="dcc2d-118">Nagłówek Accept informuje serwer, które typy nośników, których klient chce wykonać z serwera.</span><span class="sxs-lookup"><span data-stu-id="dcc2d-118">The Accept header tells the server which media type(s) the client wants from the server.</span></span> <span data-ttu-id="dcc2d-119">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="dcc2d-119">For example:</span></span>

[!code-console[Main](media-formatters/samples/sample2.cmd)]

<span data-ttu-id="dcc2d-120">Ten nagłówek nakazuje serwerowi, że klient chce wykonać kod HTML, XHTML lub XML.</span><span class="sxs-lookup"><span data-stu-id="dcc2d-120">This header tells the server that the client wants either HTML, XHTML, or XML.</span></span>

<span data-ttu-id="dcc2d-121">Typ nośnika określa, jak interfejs API sieci Web serializować i deserializacji Treść wiadomości HTTP.</span><span class="sxs-lookup"><span data-stu-id="dcc2d-121">The media type determines how Web API serializes and deserializes the HTTP message body.</span></span> <span data-ttu-id="dcc2d-122">Interfejs API sieci Web ma wbudowaną obsługę danych XML, JSON, BSON i form-urlencoded, a także obsługuje dodatkowe typy nośników, pisząc program *formatujący multimedia*.</span><span class="sxs-lookup"><span data-stu-id="dcc2d-122">Web API has built-in support for XML, JSON, BSON, and form-urlencoded data, and you can support additional media types by writing a *media formatter*.</span></span>

<span data-ttu-id="dcc2d-123">Aby utworzyć program formatujący multimedia, należy uzyskać od jednej z następujących klas:</span><span class="sxs-lookup"><span data-stu-id="dcc2d-123">To create a media formatter, derive from one of these classes:</span></span>

- <span data-ttu-id="dcc2d-124">[MediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx).</span><span class="sxs-lookup"><span data-stu-id="dcc2d-124">[MediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx).</span></span> <span data-ttu-id="dcc2d-125">Ta klasa używa asynchronicznych metod odczytu i zapisu.</span><span class="sxs-lookup"><span data-stu-id="dcc2d-125">This class uses asynchronous read and write methods.</span></span>
- <span data-ttu-id="dcc2d-126">[BufferedMediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx).</span><span class="sxs-lookup"><span data-stu-id="dcc2d-126">[BufferedMediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx).</span></span> <span data-ttu-id="dcc2d-127">Ta klasa pochodzi z **MediaTypeFormatter** , ale używa synchronicznych metod odczytu/zapisu.</span><span class="sxs-lookup"><span data-stu-id="dcc2d-127">This class derives from **MediaTypeFormatter** but uses synchronous read/write methods.</span></span>

<span data-ttu-id="dcc2d-128">Wyprowadzanie z **BufferedMediaTypeFormatter** jest prostsze, ponieważ nie istnieje kod asynchroniczny, ale oznacza to również, że wątek wywołujący może blokować podczas operacji we/wy.</span><span class="sxs-lookup"><span data-stu-id="dcc2d-128">Deriving from **BufferedMediaTypeFormatter** is simpler, because there is no asynchronous code, but it also means the calling thread can block during I/O.</span></span>

## <a name="example-creating-a-csv-media-formatter"></a><span data-ttu-id="dcc2d-129">Przykład: Tworzenie programu formatującego multimedia w formacie CSV</span><span class="sxs-lookup"><span data-stu-id="dcc2d-129">Example: Creating a CSV Media Formatter</span></span>

<span data-ttu-id="dcc2d-130">Poniższy przykład przedstawia program formatujący typ nośnika, który może serializować obiekt produktu do formatu wartości rozdzielanych przecinkami (CSV).</span><span class="sxs-lookup"><span data-stu-id="dcc2d-130">The following example shows a media type formatter that can serialize a Product object to a comma-separated values (CSV) format.</span></span> <span data-ttu-id="dcc2d-131">W tym przykładzie zastosowano typ produktu zdefiniowany w samouczku [Tworzenie interfejsu API sieci Web obsługującego operacje CRUD](../older-versions/creating-a-web-api-that-supports-crud-operations.md).</span><span class="sxs-lookup"><span data-stu-id="dcc2d-131">This example uses the Product type defined in the tutorial [Creating a Web API that Supports CRUD Operations](../older-versions/creating-a-web-api-that-supports-crud-operations.md).</span></span> <span data-ttu-id="dcc2d-132">Oto Definicja obiektu produkt:</span><span class="sxs-lookup"><span data-stu-id="dcc2d-132">Here is the definition of the Product object:</span></span>

[!code-csharp[Main](media-formatters/samples/sample3.cs)]

<span data-ttu-id="dcc2d-133">Aby zaimplementować program formatujący CSV, zdefiniuj klasę, która pochodzi od **BufferedMediaTypeFormatter**:</span><span class="sxs-lookup"><span data-stu-id="dcc2d-133">To implement a CSV formatter, define a class that derives from **BufferedMediaTypeFormatter**:</span></span>

[!code-csharp[Main](media-formatters/samples/sample4.cs)]

<span data-ttu-id="dcc2d-134">W konstruktorze Dodaj typy nośników obsługiwane przez program formatujący.</span><span class="sxs-lookup"><span data-stu-id="dcc2d-134">In the constructor, add the media types that the formatter supports.</span></span> <span data-ttu-id="dcc2d-135">W tym przykładzie program formatujący obsługuje pojedynczy typ nośnika, &quot;&quot;tekstu/CSV:</span><span class="sxs-lookup"><span data-stu-id="dcc2d-135">In this example, the formatter supports a single media type, &quot;text/csv&quot;:</span></span>

[!code-csharp[Main](media-formatters/samples/sample5.cs)]

<span data-ttu-id="dcc2d-136">Zastąp metodę **Nowritetype** , aby wskazać, które typy mogą serializować elementy formatującego:</span><span class="sxs-lookup"><span data-stu-id="dcc2d-136">Override the **CanWriteType** method to indicate which types the formatter can serialize:</span></span>

[!code-csharp[Main](media-formatters/samples/sample6.cs)]

<span data-ttu-id="dcc2d-137">W tym przykładzie program formatujący może serializować pojedyncze `Product` obiekty, a także kolekcje obiektów `Product`.</span><span class="sxs-lookup"><span data-stu-id="dcc2d-137">In this example, the formatter can serialize single `Product` objects as well as collections of `Product` objects.</span></span>

<span data-ttu-id="dcc2d-138">Analogicznie Zastąp metodę **Unreadtype** , aby wskazać, które typy mogą deserializować program formatujący.</span><span class="sxs-lookup"><span data-stu-id="dcc2d-138">Similarly, override the **CanReadType** method to indicate which types the formatter can deserialize.</span></span> <span data-ttu-id="dcc2d-139">W tym przykładzie program formatujący nie obsługuje deserializacji, dlatego metoda po prostu zwraca **wartość false**.</span><span class="sxs-lookup"><span data-stu-id="dcc2d-139">In this example, the formatter does not support deserialization, so the method simply returns **false**.</span></span>

[!code-csharp[Main](media-formatters/samples/sample7.cs)]

<span data-ttu-id="dcc2d-140">Na koniec Zastąp metodę **WriteToStream** .</span><span class="sxs-lookup"><span data-stu-id="dcc2d-140">Finally, override the **WriteToStream** method.</span></span> <span data-ttu-id="dcc2d-141">Ta metoda serializacji typ przez zapisanie go do strumienia.</span><span class="sxs-lookup"><span data-stu-id="dcc2d-141">This method serializes a type by writing it to a stream.</span></span> <span data-ttu-id="dcc2d-142">Jeśli program formatujący obsługuje deserializacji, należy również zastąpić metodę **readFromStream** .</span><span class="sxs-lookup"><span data-stu-id="dcc2d-142">If your formatter supports deserialization, also override the **ReadFromStream** method.</span></span>

[!code-csharp[Main](media-formatters/samples/sample8.cs)]

## <a name="adding-a-media-formatter-to-the-web-api-pipeline"></a><span data-ttu-id="dcc2d-143">Dodawanie programu formatującego multimedia do potoku interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="dcc2d-143">Adding a Media Formatter to the Web API Pipeline</span></span>

<span data-ttu-id="dcc2d-144">Aby dodać program formatujący typ nośnika do potoku interfejsu API sieci Web, należy użyć właściwości program **formatującegos** w obiekcie **HttpConfiguration** .</span><span class="sxs-lookup"><span data-stu-id="dcc2d-144">To add a media type formatter to the Web API pipeline, use the **Formatters** property on the **HttpConfiguration** object.</span></span>

[!code-csharp[Main](media-formatters/samples/sample9.cs)]

## <a name="character-encodings"></a><span data-ttu-id="dcc2d-145">Kodowania znaków</span><span class="sxs-lookup"><span data-stu-id="dcc2d-145">Character Encodings</span></span>

<span data-ttu-id="dcc2d-146">Opcjonalnie program formatujący multimedia może obsługiwać wiele kodowań znaków, takich jak UTF-8 lub ISO 8859-1.</span><span class="sxs-lookup"><span data-stu-id="dcc2d-146">Optionally, a media formatter can support multiple character encodings, such as UTF-8 or ISO 8859-1.</span></span>

<span data-ttu-id="dcc2d-147">W konstruktorze Dodaj co najmniej jeden typ [System. Text. Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx) do kolekcji **SupportedEncodings** .</span><span class="sxs-lookup"><span data-stu-id="dcc2d-147">In the constructor, add one or more [System.Text.Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx) types to the **SupportedEncodings** collection.</span></span> <span data-ttu-id="dcc2d-148">Najpierw umieść kodowanie domyślne.</span><span class="sxs-lookup"><span data-stu-id="dcc2d-148">Put the default encoding first.</span></span>

[!code-csharp[Main](media-formatters/samples/sample10.cs?highlight=6-7)]

<span data-ttu-id="dcc2d-149">W metodach **WriteToStream** i **readFromStream** Wywołaj [MediaTypeFormatter. SelectCharacterEncoding](https://msdn.microsoft.com/library/hh969054.aspx) , aby wybrać preferowane kodowanie znaków.</span><span class="sxs-lookup"><span data-stu-id="dcc2d-149">In the **WriteToStream** and **ReadFromStream** methods, call [MediaTypeFormatter.SelectCharacterEncoding](https://msdn.microsoft.com/library/hh969054.aspx) to select the preferred character encoding.</span></span> <span data-ttu-id="dcc2d-150">Ta metoda dopasowuje nagłówki żądania do listy obsługiwanych kodowań.</span><span class="sxs-lookup"><span data-stu-id="dcc2d-150">This method matches the request headers against the list of supported encodings.</span></span> <span data-ttu-id="dcc2d-151">Podczas odczytu lub zapisu ze strumienia używaj zwracanego **kodowania** :</span><span class="sxs-lookup"><span data-stu-id="dcc2d-151">Use the returned **Encoding** when you read or write from the stream:</span></span>

[!code-csharp[Main](media-formatters/samples/sample11.cs?highlight=3,5)]

---
uid: web-api/overview/formats-and-model-binding/json-and-xml-serialization
title: JSON i serializacji XML we wzorcu ASP.NET Web API — ASP.NET 4.x
author: MikeWasson
description: W tym artykule opisano elementy formatujące formatami JSON i XML w interfejsie API sieci Web platformy ASP.NET dla aplikacji ASP.NET 4.x.
ms.author: riande
ms.date: 05/30/2012
ms.custom: seoapril2019
ms.assetid: 1cd7525d-de5e-4ab6-94f0-51480d3255d1
msc.legacyurl: /web-api/overview/formats-and-model-binding/json-and-xml-serialization
msc.type: authoredcontent
ms.openlocfilehash: 00fa07f00eabf7e6c883c5e9ceaf9a38a8f49605
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65126167"
---
# <a name="json-and-xml-serialization-in-aspnet-web-api"></a><span data-ttu-id="0efd2-103">JSON i serializacji XML we wzorcu ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="0efd2-103">JSON and XML Serialization in ASP.NET Web API</span></span>

<span data-ttu-id="0efd2-104">przez [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="0efd2-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="0efd2-105">W tym artykule opisano elementy formatujące formatami JSON i XML w interfejsie API sieci Web platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="0efd2-105">This article describes the JSON and XML formatters in ASP.NET Web API.</span></span>

<span data-ttu-id="0efd2-106">W programie ASP.NET Web API *program formatujący typ nośnika* jest obiektem, który można:</span><span class="sxs-lookup"><span data-stu-id="0efd2-106">In ASP.NET Web API, a *media-type formatter* is an object that can:</span></span>

- <span data-ttu-id="0efd2-107">Treść komunikatu obiektów CLR odczytu z HTTP</span><span class="sxs-lookup"><span data-stu-id="0efd2-107">Read CLR objects from an HTTP message body</span></span>
- <span data-ttu-id="0efd2-108">Zapisywanie obiektów CLR w treści komunikatu HTTP</span><span class="sxs-lookup"><span data-stu-id="0efd2-108">Write CLR objects into an HTTP message body</span></span>

<span data-ttu-id="0efd2-109">Internetowy interfejs API udostępnia programy formatujące typy nośnika dla formatu JSON i XML.</span><span class="sxs-lookup"><span data-stu-id="0efd2-109">Web API provides media-type formatters for both JSON and XML.</span></span> <span data-ttu-id="0efd2-110">Struktura wstawia te elementy formatujące do potoku domyślnie.</span><span class="sxs-lookup"><span data-stu-id="0efd2-110">The framework inserts these formatters into the pipeline by default.</span></span> <span data-ttu-id="0efd2-111">Klienci mogą żądać JSON lub XML w nagłówku Accept żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="0efd2-111">Clients can request either JSON or XML in the Accept header of the HTTP request.</span></span>

## <a name="contents"></a><span data-ttu-id="0efd2-112">Spis treści</span><span class="sxs-lookup"><span data-stu-id="0efd2-112">Contents</span></span>

- [<span data-ttu-id="0efd2-113">Element formatujący typu nośnika JSON</span><span class="sxs-lookup"><span data-stu-id="0efd2-113">JSON Media-Type Formatter</span></span>](#json_media_type_formatter)

    - [<span data-ttu-id="0efd2-114">Właściwości tylko do odczytu</span><span class="sxs-lookup"><span data-stu-id="0efd2-114">Read-Only Properties</span></span>](#json_readonly)
    - [<span data-ttu-id="0efd2-115">Daty</span><span class="sxs-lookup"><span data-stu-id="0efd2-115">Dates</span></span>](#json_dates)
    - [<span data-ttu-id="0efd2-116">Wcięcia</span><span class="sxs-lookup"><span data-stu-id="0efd2-116">Indenting</span></span>](#json_indenting)
    - [<span data-ttu-id="0efd2-117">Camelcase</span><span class="sxs-lookup"><span data-stu-id="0efd2-117">Camel Casing</span></span>](#json_camelcasing)
    - [<span data-ttu-id="0efd2-118">Anonimowy i ze słabą kontrolą typów obiektów</span><span class="sxs-lookup"><span data-stu-id="0efd2-118">Anonymous and Weakly-Typed Objects</span></span>](#json_anon)
- [<span data-ttu-id="0efd2-119">XML Media-Type Formatter</span><span class="sxs-lookup"><span data-stu-id="0efd2-119">XML Media-Type Formatter</span></span>](#xml_media_type_formatter)

    - [<span data-ttu-id="0efd2-120">Właściwości tylko do odczytu</span><span class="sxs-lookup"><span data-stu-id="0efd2-120">Read-Only Properties</span></span>](#xml_readonly)
    - [<span data-ttu-id="0efd2-121">Daty</span><span class="sxs-lookup"><span data-stu-id="0efd2-121">Dates</span></span>](#xml_dates)
    - [<span data-ttu-id="0efd2-122">Wcięcia</span><span class="sxs-lookup"><span data-stu-id="0efd2-122">Indenting</span></span>](#xml_indenting)
    - [<span data-ttu-id="0efd2-123">Ustawienie na typ XML serializatorów</span><span class="sxs-lookup"><span data-stu-id="0efd2-123">Setting Per-Type XML Serializers</span></span>](#xml_pertype)
- [<span data-ttu-id="0efd2-124">Usuwanie formatu JSON czy element formatujący XML</span><span class="sxs-lookup"><span data-stu-id="0efd2-124">Removing the JSON or XML Formatter</span></span>](#removing_the_json_or_xml_formatter)
- [<span data-ttu-id="0efd2-125">Obsługa odwołania do obiektu cykliczne</span><span class="sxs-lookup"><span data-stu-id="0efd2-125">Handling Circular Object References</span></span>](#handling_circular_object_references)
- [<span data-ttu-id="0efd2-126">Testowanie odpowiedzialność za serializację obiektu</span><span class="sxs-lookup"><span data-stu-id="0efd2-126">Testing Object Serialization</span></span>](#testing_object_serialization)

<a id="json_media_type_formatter"></a>
## <a name="json-media-type-formatter"></a><span data-ttu-id="0efd2-127">Element formatujący typu nośnika JSON</span><span class="sxs-lookup"><span data-stu-id="0efd2-127">JSON Media-Type Formatter</span></span>

<span data-ttu-id="0efd2-128">Formatowanie JSON jest udostępniany przez **JsonMediaTypeFormatter** klasy.</span><span class="sxs-lookup"><span data-stu-id="0efd2-128">JSON formatting is provided by the **JsonMediaTypeFormatter** class.</span></span> <span data-ttu-id="0efd2-129">Domyślnie **JsonMediaTypeFormatter** używa [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) biblioteki do wykonywania serializacji.</span><span class="sxs-lookup"><span data-stu-id="0efd2-129">By default, **JsonMediaTypeFormatter** uses the [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) library to perform serialization.</span></span> <span data-ttu-id="0efd2-130">Na składnik Json.NET to projekt typu open source innych firm.</span><span class="sxs-lookup"><span data-stu-id="0efd2-130">Json.NET is a third-party open source project.</span></span>

<span data-ttu-id="0efd2-131">Jeśli wolisz, możesz skonfigurować **JsonMediaTypeFormatter** klasy **klasa DataContractJsonSerializer** zamiast struktury Json.NET.</span><span class="sxs-lookup"><span data-stu-id="0efd2-131">If you prefer, you can configure the **JsonMediaTypeFormatter** class to use the **DataContractJsonSerializer** instead of Json.NET.</span></span> <span data-ttu-id="0efd2-132">Aby to zrobić, należy ustawić **UseDataContractJsonSerializer** właściwości **true**:</span><span class="sxs-lookup"><span data-stu-id="0efd2-132">To do so, set the **UseDataContractJsonSerializer** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample1.cs)]

### <a name="json-serialization"></a><span data-ttu-id="0efd2-133">Serializacja JSON</span><span class="sxs-lookup"><span data-stu-id="0efd2-133">JSON Serialization</span></span>

<span data-ttu-id="0efd2-134">W tej sekcji opisano niektóre zachowania określonego programu formatującego JSON, przy użyciu domyślnego [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) serializatora.</span><span class="sxs-lookup"><span data-stu-id="0efd2-134">This section describes some specific behaviors of the JSON formatter, using the default [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) serializer.</span></span> <span data-ttu-id="0efd2-135">To jest nie należy traktować jako pełną dokumentację biblioteki na składnik Json.NET; Aby uzyskać więcej informacji, zobacz [dokumentacji na składnik Json.NET](http://james.newtonking.com/projects/json/help/).</span><span class="sxs-lookup"><span data-stu-id="0efd2-135">This is not meant to be comprehensive documentation of the Json.NET library; for more information, see the [Json.NET Documentation](http://james.newtonking.com/projects/json/help/).</span></span>

#### <a name="what-gets-serialized"></a><span data-ttu-id="0efd2-136">Co to jest serializowana z jego?</span><span class="sxs-lookup"><span data-stu-id="0efd2-136">What Gets Serialized?</span></span>

<span data-ttu-id="0efd2-137">Domyślnie wszystkie właściwości publiczne i pola są uwzględnione w serializacji JSON.</span><span class="sxs-lookup"><span data-stu-id="0efd2-137">By default, all public properties and fields are included in the serialized JSON.</span></span> <span data-ttu-id="0efd2-138">Aby pominąć właściwość lub pole, dekoracji ją za pomocą **JsonIgnore** atrybutu.</span><span class="sxs-lookup"><span data-stu-id="0efd2-138">To omit a property or field, decorate it with the **JsonIgnore** attribute.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample2.cs)]

<span data-ttu-id="0efd2-139">Jeśli użytkownik sobie tego życzy &quot;uczestnictwo&quot; podejście, dekoracji klasy za pomocą **DataContract** atrybutu.</span><span class="sxs-lookup"><span data-stu-id="0efd2-139">If you prefer an &quot;opt-in&quot; approach, decorate the class with the **DataContract** attribute.</span></span> <span data-ttu-id="0efd2-140">Jeśli ten atrybut jest obecny, elementy członkowskie są ignorowane, chyba że mają **DataMember**.</span><span class="sxs-lookup"><span data-stu-id="0efd2-140">If this attribute is present, members are ignored unless they have the **DataMember**.</span></span> <span data-ttu-id="0efd2-141">Można również użyć **DataMember** serializować prywatnych elementów członkowskich.</span><span class="sxs-lookup"><span data-stu-id="0efd2-141">You can also use **DataMember** to serialize private members.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample3.cs)]

<a id="json_readonly"></a>
### <a name="read-only-properties"></a><span data-ttu-id="0efd2-142">Właściwości tylko do odczytu</span><span class="sxs-lookup"><span data-stu-id="0efd2-142">Read-Only Properties</span></span>

<span data-ttu-id="0efd2-143">Właściwości tylko do odczytu są serializowane domyślnie.</span><span class="sxs-lookup"><span data-stu-id="0efd2-143">Read-only properties are serialized by default.</span></span>

<a id="json_dates"></a>
### <a name="dates"></a><span data-ttu-id="0efd2-144">Daty</span><span class="sxs-lookup"><span data-stu-id="0efd2-144">Dates</span></span>

<span data-ttu-id="0efd2-145">Domyślnie program Json.NET zapisuje dat [ISO 8601](http://www.w3.org/TR/NOTE-datetime) formatu.</span><span class="sxs-lookup"><span data-stu-id="0efd2-145">By default, Json.NET writes dates in [ISO 8601](http://www.w3.org/TR/NOTE-datetime) format.</span></span> <span data-ttu-id="0efd2-146">Daty w formacie UTC (Coordinated Universal Time) są zapisywane z sufiksem "Z".</span><span class="sxs-lookup"><span data-stu-id="0efd2-146">Dates in UTC (Coordinated Universal Time) are written with a "Z" suffix.</span></span> <span data-ttu-id="0efd2-147">Daty w czasie lokalnym obejmują przesunięcia strefy czasowej.</span><span class="sxs-lookup"><span data-stu-id="0efd2-147">Dates in local time include a time-zone offset.</span></span> <span data-ttu-id="0efd2-148">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="0efd2-148">For example:</span></span>

[!code-console[Main](json-and-xml-serialization/samples/sample4.cmd)]

<span data-ttu-id="0efd2-149">Domyślnie program Json.NET zachowuje strefy czasowej.</span><span class="sxs-lookup"><span data-stu-id="0efd2-149">By default, Json.NET preserves the time zone.</span></span> <span data-ttu-id="0efd2-150">Możesz przesłonić to przez ustawienie właściwości DateTimeZoneHandling:</span><span class="sxs-lookup"><span data-stu-id="0efd2-150">You can override this by setting the DateTimeZoneHandling property:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample5.cs)]

<span data-ttu-id="0efd2-151">Jeśli wolisz używać [format daty Microsoft JSON](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) zamiast ISO 8601, ustaw **DateFormatHandling** właściwości ustawienia serializatora:</span><span class="sxs-lookup"><span data-stu-id="0efd2-151">If you prefer to use [Microsoft JSON date format](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) instead of ISO 8601, set the **DateFormatHandling** property on the serializer settings:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample6.cs)]

<a id="json_indenting"></a>
### <a name="indenting"></a><span data-ttu-id="0efd2-152">Wcięcia</span><span class="sxs-lookup"><span data-stu-id="0efd2-152">Indenting</span></span>

<span data-ttu-id="0efd2-153">Aby napisać JSON z wcięciami, należy ustawić **formatowanie** ustawienie **Formatting.Indented**:</span><span class="sxs-lookup"><span data-stu-id="0efd2-153">To write indented JSON, set the **Formatting** setting to **Formatting.Indented**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample7.cs)]

<a id="json_camelcasing"></a>
### <a name="camel-casing"></a><span data-ttu-id="0efd2-154">Camelcase</span><span class="sxs-lookup"><span data-stu-id="0efd2-154">Camel Casing</span></span>

<span data-ttu-id="0efd2-155">Aby zapisać nazwy właściwości JSON zawierające camelcase, bez wprowadzania zmian w modelu danych, należy ustawić **CamelCasePropertyNamesContractResolver** na element serializujący:</span><span class="sxs-lookup"><span data-stu-id="0efd2-155">To write JSON property names with camel casing, without changing your data model, set the **CamelCasePropertyNamesContractResolver** on the serializer:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample8.cs)]

<a id="json_anon"></a>
### <a name="anonymous-and-weakly-typed-objects"></a><span data-ttu-id="0efd2-156">Anonimowy i ze słabą kontrolą typów obiektów</span><span class="sxs-lookup"><span data-stu-id="0efd2-156">Anonymous and Weakly-Typed Objects</span></span>

<span data-ttu-id="0efd2-157">Metody akcji można zwrócić obiektu anonimowego i serializować go do formatu JSON.</span><span class="sxs-lookup"><span data-stu-id="0efd2-157">An action method can return an anonymous object and serialize it to JSON.</span></span> <span data-ttu-id="0efd2-158">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="0efd2-158">For example:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample9.cs)]

<span data-ttu-id="0efd2-159">Treści komunikatu odpowiedzi zawiera następujące dane JSON:</span><span class="sxs-lookup"><span data-stu-id="0efd2-159">The response message body will contain the following JSON:</span></span>

[!code-json[Main](json-and-xml-serialization/samples/sample10.json)]

<span data-ttu-id="0efd2-160">Jeśli internetowy interfejs API odbiera luźno struktura obiektów JSON od klientów, może wykonywać deserializację treści żądania, aby **Newtonsoft.Json.Linq.JObject** typu.</span><span class="sxs-lookup"><span data-stu-id="0efd2-160">If your web API receives loosely structured JSON objects from clients, you can deserialize the request body to a **Newtonsoft.Json.Linq.JObject** type.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample11.cs)]

<span data-ttu-id="0efd2-161">Jednak zazwyczaj lepiej jest używać obiektów silnie typizowanych danych.</span><span class="sxs-lookup"><span data-stu-id="0efd2-161">However, it is usually better to use strongly typed data objects.</span></span> <span data-ttu-id="0efd2-162">Nie jest potrzebny do analizowania danych, użytkownika i uzyskać korzyści wynikające z weryfikacją modelu.</span><span class="sxs-lookup"><span data-stu-id="0efd2-162">Then you don't need to parse the data yourself, and you get the benefits of model validation.</span></span>

<span data-ttu-id="0efd2-163">Serializator XML nie obsługuje typów anonimowych lub **JObject** wystąpień.</span><span class="sxs-lookup"><span data-stu-id="0efd2-163">The XML serializer does not support anonymous types or **JObject** instances.</span></span> <span data-ttu-id="0efd2-164">Jeśli używasz tych funkcji dla danych JSON, należy usunąć program formatujący kod XML z potoku, zgodnie z opisem w dalszej części tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="0efd2-164">If you use these features for your JSON data, you should remove the XML formatter from the pipeline, as described later in this article.</span></span>

<a id="xml_media_type_formatter"></a>
## <a name="xml-media-type-formatter"></a><span data-ttu-id="0efd2-165">Element formatujący typu nośnika XML</span><span class="sxs-lookup"><span data-stu-id="0efd2-165">XML Media-Type Formatter</span></span>

<span data-ttu-id="0efd2-166">Formatowanie XML są dostarczane przez **XmlMediaTypeFormatter** klasy.</span><span class="sxs-lookup"><span data-stu-id="0efd2-166">XML formatting is provided by the **XmlMediaTypeFormatter** class.</span></span> <span data-ttu-id="0efd2-167">Domyślnie **XmlMediaTypeFormatter** używa **DataContractSerializer** klasy do wykonywania serializacji.</span><span class="sxs-lookup"><span data-stu-id="0efd2-167">By default, **XmlMediaTypeFormatter** uses the **DataContractSerializer** class to perform serialization.</span></span>

<span data-ttu-id="0efd2-168">Jeśli wolisz, możesz skonfigurować **XmlMediaTypeFormatter** używać **XmlSerializer** zamiast **DataContractSerializer**.</span><span class="sxs-lookup"><span data-stu-id="0efd2-168">If you prefer, you can configure the **XmlMediaTypeFormatter** to use the **XmlSerializer** instead of the **DataContractSerializer**.</span></span> <span data-ttu-id="0efd2-169">Aby to zrobić, należy ustawić **UseXmlSerializer** właściwości **true**:</span><span class="sxs-lookup"><span data-stu-id="0efd2-169">To do so, set the **UseXmlSerializer** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample12.cs)]

<span data-ttu-id="0efd2-170">**XmlSerializer** klasa obsługuje mniejszą niż zestaw typów niż **DataContractSerializer**, ale zapewnia większą kontrolę nad wynikowy kod XML.</span><span class="sxs-lookup"><span data-stu-id="0efd2-170">The **XmlSerializer** class supports a narrower set of types than **DataContractSerializer**, but gives more control over the resulting XML.</span></span> <span data-ttu-id="0efd2-171">Należy rozważyć użycie **XmlSerializer** Jeśli potrzebujesz do dopasowania istniejącego schematu XML.</span><span class="sxs-lookup"><span data-stu-id="0efd2-171">Consider using **XmlSerializer** if you need to match an existing XML schema.</span></span>

### <a name="xml-serialization"></a><span data-ttu-id="0efd2-172">Serializacji XML</span><span class="sxs-lookup"><span data-stu-id="0efd2-172">XML Serialization</span></span>

<span data-ttu-id="0efd2-173">W tej sekcji opisano niektóre zachowania określonego programu formatującego XML, przy użyciu domyślnego **DataContractSerializer**.</span><span class="sxs-lookup"><span data-stu-id="0efd2-173">This section describes some specific behaviors of the XML formatter, using the default **DataContractSerializer**.</span></span>

<span data-ttu-id="0efd2-174">Domyślnie DataContractSerializer zachowuje się w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="0efd2-174">By default, the DataContractSerializer behaves as follows:</span></span>

- <span data-ttu-id="0efd2-175">Wszystkie właściwości publiczne odczytu/zapisu i pola są serializowane.</span><span class="sxs-lookup"><span data-stu-id="0efd2-175">All public read/write properties and fields are serialized.</span></span> <span data-ttu-id="0efd2-176">Aby pominąć właściwość lub pole, dekoracji ją za pomocą **IgnoreDataMember** atrybutu.</span><span class="sxs-lookup"><span data-stu-id="0efd2-176">To omit a property or field, decorate it with the **IgnoreDataMember** attribute.</span></span>
- <span data-ttu-id="0efd2-177">Prywatnych i chronionych składowych nie są serializowane.</span><span class="sxs-lookup"><span data-stu-id="0efd2-177">Private and protected members are not serialized.</span></span>
- <span data-ttu-id="0efd2-178">Właściwości tylko do odczytu nie są serializowane.</span><span class="sxs-lookup"><span data-stu-id="0efd2-178">Read-only properties are not serialized.</span></span> <span data-ttu-id="0efd2-179">(Jednak zawartość właściwości kolekcji tylko do odczytu są serializowane.)</span><span class="sxs-lookup"><span data-stu-id="0efd2-179">(However, the contents of a read-only collection property are serialized.)</span></span>
- <span data-ttu-id="0efd2-180">Nazwy klas i składowych są zapisywane w pliku XML, dokładnie tak, jak pojawiają się w deklaracji klasy.</span><span class="sxs-lookup"><span data-stu-id="0efd2-180">Class and member names are written in the XML exactly as they appear in the class declaration.</span></span>
- <span data-ttu-id="0efd2-181">Domyślny obszar nazw XML jest używany.</span><span class="sxs-lookup"><span data-stu-id="0efd2-181">A default XML namespace is used.</span></span>

<span data-ttu-id="0efd2-182">Jeśli potrzebujesz większej kontroli nad serializacji, można dekoracji klasy za pomocą **DataContract** atrybutu.</span><span class="sxs-lookup"><span data-stu-id="0efd2-182">If you need more control over the serialization, you can decorate the class with the **DataContract** attribute.</span></span> <span data-ttu-id="0efd2-183">Jeśli ten atrybut jest obecny, klasa jest serializowana w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="0efd2-183">When this attribute is present, the class is serialized as follows:</span></span>

- <span data-ttu-id="0efd2-184">&quot;Zgoda&quot; podejścia: Właściwości i pola nie są szeregowane domyślnie.</span><span class="sxs-lookup"><span data-stu-id="0efd2-184">&quot;Opt in&quot; approach: Properties and fields are not serialized by default.</span></span> <span data-ttu-id="0efd2-185">Do serializacji, właściwość lub pole, dekoracji ją za pomocą **DataMember** atrybutu.</span><span class="sxs-lookup"><span data-stu-id="0efd2-185">To serialize a property or field, decorate it with the **DataMember** attribute.</span></span>
- <span data-ttu-id="0efd2-186">Do serializacji Członkowskich prywatnych ani chronionych, dekoracji ją za pomocą **DataMember** atrybutu.</span><span class="sxs-lookup"><span data-stu-id="0efd2-186">To serialize a private or protected member, decorate it with the **DataMember** attribute.</span></span>
- <span data-ttu-id="0efd2-187">Właściwości tylko do odczytu nie są serializowane.</span><span class="sxs-lookup"><span data-stu-id="0efd2-187">Read-only properties are not serialized.</span></span>
- <span data-ttu-id="0efd2-188">Aby zmienić sposób wyświetlania nazwy klasy w pliku XML, należy ustawić *nazwa* parametru w **DataContract** atrybutu.</span><span class="sxs-lookup"><span data-stu-id="0efd2-188">To change how the class name appears in the XML, set the *Name* parameter in the **DataContract** attribute.</span></span>
- <span data-ttu-id="0efd2-189">Aby zmienić sposób wyświetlania nazwę elementu członkowskiego w pliku XML, należy ustawić *nazwa* parametru w **DataMember** atrybutu.</span><span class="sxs-lookup"><span data-stu-id="0efd2-189">To change how a member name appears in the XML, set the *Name* parameter in the **DataMember** attribute.</span></span>
- <span data-ttu-id="0efd2-190">Aby zmienić obszar nazw XML, należy ustawić *Namespace* parametru w **DataContract** klasy.</span><span class="sxs-lookup"><span data-stu-id="0efd2-190">To change the XML namespace, set the *Namespace* parameter in the **DataContract** class.</span></span>

<a id="xml_readonly"></a>
### <a name="read-only-properties"></a><span data-ttu-id="0efd2-191">Właściwości tylko do odczytu</span><span class="sxs-lookup"><span data-stu-id="0efd2-191">Read-Only Properties</span></span>

<span data-ttu-id="0efd2-192">Właściwości tylko do odczytu nie są serializowane.</span><span class="sxs-lookup"><span data-stu-id="0efd2-192">Read-only properties are not serialized.</span></span> <span data-ttu-id="0efd2-193">Jeśli właściwość jest tylko do odczytu zapasowego pola prywatnego, można oznaczyć pola prywatnego przy użyciu **DataMember** atrybutu.</span><span class="sxs-lookup"><span data-stu-id="0efd2-193">If a read-only property has a backing private field, you can mark the private field with the **DataMember** attribute.</span></span> <span data-ttu-id="0efd2-194">Takie podejście wymaga **DataContract** atrybut w klasie.</span><span class="sxs-lookup"><span data-stu-id="0efd2-194">This approach requires the **DataContract** attribute on the class.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample13.cs)]

<a id="xml_dates"></a>
### <a name="dates"></a><span data-ttu-id="0efd2-195">Daty</span><span class="sxs-lookup"><span data-stu-id="0efd2-195">Dates</span></span>

<span data-ttu-id="0efd2-196">Daty są zapisywane w formacie ISO 8601.</span><span class="sxs-lookup"><span data-stu-id="0efd2-196">Dates are written in ISO 8601 format.</span></span> <span data-ttu-id="0efd2-197">Na przykład &quot;2012-05-23T20:21:37.9116538Z&quot;.</span><span class="sxs-lookup"><span data-stu-id="0efd2-197">For example, &quot;2012-05-23T20:21:37.9116538Z&quot;.</span></span>

<a id="xml_indenting"></a>
### <a name="indenting"></a><span data-ttu-id="0efd2-198">Wcięcia</span><span class="sxs-lookup"><span data-stu-id="0efd2-198">Indenting</span></span>

<span data-ttu-id="0efd2-199">Aby zapisać XML z wcięciami, należy ustawić **wcięcie** właściwości **true**:</span><span class="sxs-lookup"><span data-stu-id="0efd2-199">To write indented XML, set the **Indent** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample14.cs)]

<a id="xml_pertype"></a>
## <a name="setting-per-type-xml-serializers"></a><span data-ttu-id="0efd2-200">Ustawienie na typ XML serializatorów</span><span class="sxs-lookup"><span data-stu-id="0efd2-200">Setting Per-Type XML Serializers</span></span>

<span data-ttu-id="0efd2-201">Możesz ustawić różne serializatory XML dla różnych typów CLR.</span><span class="sxs-lookup"><span data-stu-id="0efd2-201">You can set different XML serializers for different CLR types.</span></span> <span data-ttu-id="0efd2-202">Na przykład, może mieć obiektu danych, który wymaga **XmlSerializer** zgodności z poprzednimi wersjami.</span><span class="sxs-lookup"><span data-stu-id="0efd2-202">For example, you might have a particular data object that requires **XmlSerializer** for backward compatibility.</span></span> <span data-ttu-id="0efd2-203">Możesz użyć **XmlSerializer** dla tego obiektu i dalej używaj **DataContractSerializer** dla innych typów.</span><span class="sxs-lookup"><span data-stu-id="0efd2-203">You can use **XmlSerializer** for this object and continue to use **DataContractSerializer** for other types.</span></span>

<span data-ttu-id="0efd2-204">Aby ustawić element serializujący XML dla określonego typu, należy wywołać **SetSerializer**.</span><span class="sxs-lookup"><span data-stu-id="0efd2-204">To set an XML serializer for a particular type, call **SetSerializer**.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample15.cs)]

<span data-ttu-id="0efd2-205">Można określić **XmlSerializer** lub dowolnego obiektu, który pochodzi od klasy **XmlObjectSerializer**.</span><span class="sxs-lookup"><span data-stu-id="0efd2-205">You can specify an **XmlSerializer** or any object that derives from **XmlObjectSerializer**.</span></span>

<a id="removing_the_json_or_xml_formatter"></a>
## <a name="removing-the-json-or-xml-formatter"></a><span data-ttu-id="0efd2-206">Usuwanie formatu JSON czy element formatujący XML</span><span class="sxs-lookup"><span data-stu-id="0efd2-206">Removing the JSON or XML Formatter</span></span>

<span data-ttu-id="0efd2-207">Program formatujący JSON lub element formatujący XML można usunąć z listy elementów formatujących, jeśli nie chcesz ich używać.</span><span class="sxs-lookup"><span data-stu-id="0efd2-207">You can remove the JSON formatter or the XML formatter from the list of formatters, if you do not want to use them.</span></span> <span data-ttu-id="0efd2-208">Główne przyczyny, w tym celu są następujące:</span><span class="sxs-lookup"><span data-stu-id="0efd2-208">The main reasons to do this are:</span></span>

- <span data-ttu-id="0efd2-209">Aby ograniczyć odpowiedzi interfejsu API sieci web na określonego typu nośnika.</span><span class="sxs-lookup"><span data-stu-id="0efd2-209">To restrict your web API responses to a particular media type.</span></span> <span data-ttu-id="0efd2-210">Na przykład można zdecydować, do obsługi tylko odpowiedzi JSON i Usuń program formatujący kod XML.</span><span class="sxs-lookup"><span data-stu-id="0efd2-210">For example, you might decide to support only JSON responses, and remove the XML formatter.</span></span>
- <span data-ttu-id="0efd2-211">Aby zastąpić domyślny element formatujący niestandardowego elementu formatującego.</span><span class="sxs-lookup"><span data-stu-id="0efd2-211">To replace the default formatter with a custom formatter.</span></span> <span data-ttu-id="0efd2-212">Na przykład można zastąpić elementu formatującego JSON, za pomocą niestandardowych implementacji elementu formatującego JSON.</span><span class="sxs-lookup"><span data-stu-id="0efd2-212">For example, you could replace the JSON formatter with your own custom implementation of a JSON formatter.</span></span>

<span data-ttu-id="0efd2-213">Poniższy kod przedstawia sposób usuwania domyślne elementy formatujące.</span><span class="sxs-lookup"><span data-stu-id="0efd2-213">The following code shows how to remove the default formatters.</span></span> <span data-ttu-id="0efd2-214">Wywołania od usługi **aplikacji\_Start** metoda, zdefiniowana w pliku Global.asax.</span><span class="sxs-lookup"><span data-stu-id="0efd2-214">Call this from your **Application\_Start** method, defined in Global.asax.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample16.cs)]

<a id="handling_circular_object_references"></a>
## <a name="handling-circular-object-references"></a><span data-ttu-id="0efd2-215">Obsługa odwołania do obiektu cykliczne</span><span class="sxs-lookup"><span data-stu-id="0efd2-215">Handling Circular Object References</span></span>

<span data-ttu-id="0efd2-216">Domyślnie elementy formatujące formatami JSON i XML zapisania wszystkich obiektów jako wartości.</span><span class="sxs-lookup"><span data-stu-id="0efd2-216">By default, the JSON and XML formatters write all objects as values.</span></span> <span data-ttu-id="0efd2-217">Jeśli dwie właściwości odnoszą się do tego samego obiektu lub tego samego obiektu, który pojawia się dwa razy w kolekcji, program formatujący będzie dwa razy serializacji obiektu.</span><span class="sxs-lookup"><span data-stu-id="0efd2-217">If two properties refer to the same object, or if the same object appears twice in a collection, the formatter will serialize the object twice.</span></span> <span data-ttu-id="0efd2-218">Jest to konkretnych problemów Jeśli grafu obiektów zawiera cykle, ponieważ element serializujący spowoduje zgłoszenie wyjątku w przypadku wykrycia pętli na wykresie.</span><span class="sxs-lookup"><span data-stu-id="0efd2-218">This is a particular problem if your object graph contains cycles, because the serializer will throw an exception when it detects a loop in the graph.</span></span>

<span data-ttu-id="0efd2-219">Należy wziąć pod uwagę następujące modele obiektów i kontrolera.</span><span class="sxs-lookup"><span data-stu-id="0efd2-219">Consider the following object models and controller.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample17.cs)]

<span data-ttu-id="0efd2-220">Wywoływanie tej akcji spowoduje, że element formatujący zgłoszony wyjątek, co przekłada się na stan kodu 500 (wewnętrzny błąd serwera) odpowiedź do klienta.</span><span class="sxs-lookup"><span data-stu-id="0efd2-220">Invoking this action will cause the formatter to thrown an exception, which translates to a status code 500 (Internal Server Error) response to the client.</span></span>

<span data-ttu-id="0efd2-221">Aby zachować odwołania do obiektów w formacie JSON, Dodaj następujący kod do **aplikacji\_Start** metody w pliku Global.asax:</span><span class="sxs-lookup"><span data-stu-id="0efd2-221">To preserve object references in JSON, add the following code to **Application\_Start** method in the Global.asax file:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample18.cs)]

<span data-ttu-id="0efd2-222">Teraz akcji kontrolera zwróci JSON, który wygląda w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="0efd2-222">Now the controller action will return JSON that looks like this:</span></span>

[!code-json[Main](json-and-xml-serialization/samples/sample19.json)]

<span data-ttu-id="0efd2-223">Należy zauważyć, że serializator dodaje &quot;$id&quot; właściwości do obu obiektów.</span><span class="sxs-lookup"><span data-stu-id="0efd2-223">Notice that the serializer adds an &quot;$id&quot; property to both objects.</span></span> <span data-ttu-id="0efd2-224">Ponadto wykryje, czy właściwość Employee.Department tworzy pętli, a więc zastępuje wartość odwołania do obiektu: {&quot;$ref&quot;:&quot;1&quot;}.</span><span class="sxs-lookup"><span data-stu-id="0efd2-224">Also, it detects that the Employee.Department property creates a loop, so it replaces the value with an object reference: {&quot;$ref&quot;:&quot;1&quot;}.</span></span>

> [!NOTE]
> <span data-ttu-id="0efd2-225">Odwołania do obiektu nie są standardowe w formacie JSON.</span><span class="sxs-lookup"><span data-stu-id="0efd2-225">Object references are not standard in JSON.</span></span> <span data-ttu-id="0efd2-226">Przed użyciem tej funkcji, należy rozważyć, czy klienci będą mogli przeanalizować wyniki.</span><span class="sxs-lookup"><span data-stu-id="0efd2-226">Before using this feature, consider whether your clients will be able to parse the results.</span></span> <span data-ttu-id="0efd2-227">Może być lepiej po prostu usunąć cykle z wykresu.</span><span class="sxs-lookup"><span data-stu-id="0efd2-227">It might be better simply to remove cycles from the graph.</span></span> <span data-ttu-id="0efd2-228">Na przykład łącze z pracowników do działu nie jest naprawdę potrzebne w tym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="0efd2-228">For example, the link from Employee back to Department is not really needed in this example.</span></span>

<span data-ttu-id="0efd2-229">Aby zachować odwołania do obiektów w formacie XML, masz dwie opcje.</span><span class="sxs-lookup"><span data-stu-id="0efd2-229">To preserve object references in XML, you have two options.</span></span> <span data-ttu-id="0efd2-230">Prostsze opcją jest dodanie `[DataContract(IsReference=true)]` do klasy modelu.</span><span class="sxs-lookup"><span data-stu-id="0efd2-230">The simpler option is to add `[DataContract(IsReference=true)]` to your model class.</span></span> <span data-ttu-id="0efd2-231">*IsReference* parametr umożliwia odwołania do obiektu.</span><span class="sxs-lookup"><span data-stu-id="0efd2-231">The *IsReference* parameter enables object references.</span></span> <span data-ttu-id="0efd2-232">Należy pamiętać, że **DataContract** sprawia, że serializacji uczestnictwo, więc należy również dodać **DataMember** atrybuty do właściwości:</span><span class="sxs-lookup"><span data-stu-id="0efd2-232">Remember that **DataContract** makes serialization opt-in, so you will also need to add **DataMember** attributes to the properties:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample20.cs)]

<span data-ttu-id="0efd2-233">Teraz program formatujący dadzą XML podobne do następujących:</span><span class="sxs-lookup"><span data-stu-id="0efd2-233">Now the formatter will produce XML similar to following:</span></span>

[!code-xml[Main](json-and-xml-serialization/samples/sample21.xml)]

<span data-ttu-id="0efd2-234">Jeśli chcesz uniknąć atrybuty w klasie modelu, drugą opcją jest: Tworzenie nowego typu **DataContractSerializer** wystąpienia i ustaw *preserveObjectReferences* do **true** w konstruktorze.</span><span class="sxs-lookup"><span data-stu-id="0efd2-234">If you want to avoid attributes on your model class, there is another option: Create a new type-specific **DataContractSerializer** instance and set *preserveObjectReferences* to **true** in the constructor.</span></span> <span data-ttu-id="0efd2-235">Następnie ustaw tego wystąpienia jako element serializujący-type na typ nośnika elementu formatującego XML.</span><span class="sxs-lookup"><span data-stu-id="0efd2-235">Then set this instance as a per-type serializer on the XML media-type formatter.</span></span> <span data-ttu-id="0efd2-236">Następujący kod pokazuje, jak to zrobić:</span><span class="sxs-lookup"><span data-stu-id="0efd2-236">The following code show how to do this:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample22.cs?highlight=3)]

<a id="testing_object_serialization"></a>
## <a name="testing-object-serialization"></a><span data-ttu-id="0efd2-237">Testowanie odpowiedzialność za serializację obiektu</span><span class="sxs-lookup"><span data-stu-id="0efd2-237">Testing Object Serialization</span></span>

<span data-ttu-id="0efd2-238">Podczas projektowania internetowego interfejsu API jest przydatne do testowania, jak można serializować obiektów danych.</span><span class="sxs-lookup"><span data-stu-id="0efd2-238">As you design your web API, it is useful to test how your data objects will be serialized.</span></span> <span data-ttu-id="0efd2-239">Można to zrobić bez tworzenia kontrolera lub wywołanie akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="0efd2-239">You can do this without creating a controller or invoking a controller action.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample23.cs)]

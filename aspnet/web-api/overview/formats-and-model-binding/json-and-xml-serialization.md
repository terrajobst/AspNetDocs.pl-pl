---
uid: web-api/overview/formats-and-model-binding/json-and-xml-serialization
title: Serializacja JSON i XML w ASP.NET Web API-ASP.NET 4. x
author: MikeWasson
description: Opisuje elementy programu formatującego JSON i XML w interfejsie Web API ASP.NET dla ASP.NET 4. x.
ms.author: riande
ms.date: 05/30/2012
ms.custom: seoapril2019
ms.assetid: 1cd7525d-de5e-4ab6-94f0-51480d3255d1
msc.legacyurl: /web-api/overview/formats-and-model-binding/json-and-xml-serialization
msc.type: authoredcontent
ms.openlocfilehash: 00fa07f00eabf7e6c883c5e9ceaf9a38a8f49605
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557471"
---
# <a name="json-and-xml-serialization-in-aspnet-web-api"></a><span data-ttu-id="38b53-103">Serializacja JSON i XML w interfejsie API sieci Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="38b53-103">JSON and XML Serialization in ASP.NET Web API</span></span>

<span data-ttu-id="38b53-104">według [Jan Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="38b53-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="38b53-105">W tym artykule opisano elementy formatujące JSON i XML w interfejsie Web API ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="38b53-105">This article describes the JSON and XML formatters in ASP.NET Web API.</span></span>

<span data-ttu-id="38b53-106">W interfejsie API sieci Web *ASP.NET jest to* obiekt, który może:</span><span class="sxs-lookup"><span data-stu-id="38b53-106">In ASP.NET Web API, a *media-type formatter* is an object that can:</span></span>

- <span data-ttu-id="38b53-107">Odczytywanie obiektów CLR z treści wiadomości HTTP</span><span class="sxs-lookup"><span data-stu-id="38b53-107">Read CLR objects from an HTTP message body</span></span>
- <span data-ttu-id="38b53-108">Pisanie obiektów CLR w treści wiadomości HTTP</span><span class="sxs-lookup"><span data-stu-id="38b53-108">Write CLR objects into an HTTP message body</span></span>

<span data-ttu-id="38b53-109">Interfejs API sieci Web udostępnia elementy formatujące typy multimediów dla danych JSON i XML.</span><span class="sxs-lookup"><span data-stu-id="38b53-109">Web API provides media-type formatters for both JSON and XML.</span></span> <span data-ttu-id="38b53-110">Struktura wstawia domyślnie te elementy formatujące do potoku.</span><span class="sxs-lookup"><span data-stu-id="38b53-110">The framework inserts these formatters into the pipeline by default.</span></span> <span data-ttu-id="38b53-111">Klienci mogą żądać elementu JSON lub XML w nagłówku Accept żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="38b53-111">Clients can request either JSON or XML in the Accept header of the HTTP request.</span></span>

## <a name="contents"></a><span data-ttu-id="38b53-112">Spis treści</span><span class="sxs-lookup"><span data-stu-id="38b53-112">Contents</span></span>

- [<span data-ttu-id="38b53-113">Program formatujący typ multimediów JSON</span><span class="sxs-lookup"><span data-stu-id="38b53-113">JSON Media-Type Formatter</span></span>](#json_media_type_formatter)

    - [<span data-ttu-id="38b53-114">Właściwości tylko do odczytu</span><span class="sxs-lookup"><span data-stu-id="38b53-114">Read-Only Properties</span></span>](#json_readonly)
    - [<span data-ttu-id="38b53-115">Okres</span><span class="sxs-lookup"><span data-stu-id="38b53-115">Dates</span></span>](#json_dates)
    - [<span data-ttu-id="38b53-116">Wcięcia</span><span class="sxs-lookup"><span data-stu-id="38b53-116">Indenting</span></span>](#json_indenting)
    - [<span data-ttu-id="38b53-117">Notacji CamelCase</span><span class="sxs-lookup"><span data-stu-id="38b53-117">Camel Casing</span></span>](#json_camelcasing)
    - [<span data-ttu-id="38b53-118">Obiekty anonimowe i słabo wpisane</span><span class="sxs-lookup"><span data-stu-id="38b53-118">Anonymous and Weakly-Typed Objects</span></span>](#json_anon)
- [<span data-ttu-id="38b53-119">Program formatujący typ multimediów XML</span><span class="sxs-lookup"><span data-stu-id="38b53-119">XML Media-Type Formatter</span></span>](#xml_media_type_formatter)

    - [<span data-ttu-id="38b53-120">Właściwości tylko do odczytu</span><span class="sxs-lookup"><span data-stu-id="38b53-120">Read-Only Properties</span></span>](#xml_readonly)
    - [<span data-ttu-id="38b53-121">Okres</span><span class="sxs-lookup"><span data-stu-id="38b53-121">Dates</span></span>](#xml_dates)
    - [<span data-ttu-id="38b53-122">Wcięcia</span><span class="sxs-lookup"><span data-stu-id="38b53-122">Indenting</span></span>](#xml_indenting)
    - [<span data-ttu-id="38b53-123">Ustawianie serializatorów XML dla poszczególnych typów</span><span class="sxs-lookup"><span data-stu-id="38b53-123">Setting Per-Type XML Serializers</span></span>](#xml_pertype)
- [<span data-ttu-id="38b53-124">Usuwanie elementu formatującego JSON lub XML</span><span class="sxs-lookup"><span data-stu-id="38b53-124">Removing the JSON or XML Formatter</span></span>](#removing_the_json_or_xml_formatter)
- [<span data-ttu-id="38b53-125">Obsługa cyklicznych odwołań do obiektów</span><span class="sxs-lookup"><span data-stu-id="38b53-125">Handling Circular Object References</span></span>](#handling_circular_object_references)
- [<span data-ttu-id="38b53-126">Testowanie serializacji obiektu</span><span class="sxs-lookup"><span data-stu-id="38b53-126">Testing Object Serialization</span></span>](#testing_object_serialization)

<a id="json_media_type_formatter"></a>
## <a name="json-media-type-formatter"></a><span data-ttu-id="38b53-127">Program formatujący typ multimediów JSON</span><span class="sxs-lookup"><span data-stu-id="38b53-127">JSON Media-Type Formatter</span></span>

<span data-ttu-id="38b53-128">Formatowanie JSON jest dostarczane przez klasę **JsonMediaTypeFormatter** .</span><span class="sxs-lookup"><span data-stu-id="38b53-128">JSON formatting is provided by the **JsonMediaTypeFormatter** class.</span></span> <span data-ttu-id="38b53-129">Domyślnie **JsonMediaTypeFormatter** używa biblioteki [JSON.NET](https://github.com/JamesNK/Newtonsoft.Json) do wykonywania serializacji.</span><span class="sxs-lookup"><span data-stu-id="38b53-129">By default, **JsonMediaTypeFormatter** uses the [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) library to perform serialization.</span></span> <span data-ttu-id="38b53-130">Json.NET to projekt typu "open source" innej firmy.</span><span class="sxs-lookup"><span data-stu-id="38b53-130">Json.NET is a third-party open source project.</span></span>

<span data-ttu-id="38b53-131">Jeśli wolisz, możesz skonfigurować klasę **JsonMediaTypeFormatter** , aby korzystała z **Klasa DataContractJsonSerializer** zamiast JSON.NET.</span><span class="sxs-lookup"><span data-stu-id="38b53-131">If you prefer, you can configure the **JsonMediaTypeFormatter** class to use the **DataContractJsonSerializer** instead of Json.NET.</span></span> <span data-ttu-id="38b53-132">Aby to zrobić, ustaw właściwość **UseDataContractJsonSerializer** na **wartość true**:</span><span class="sxs-lookup"><span data-stu-id="38b53-132">To do so, set the **UseDataContractJsonSerializer** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample1.cs)]

### <a name="json-serialization"></a><span data-ttu-id="38b53-133">Serializacja JSON</span><span class="sxs-lookup"><span data-stu-id="38b53-133">JSON Serialization</span></span>

<span data-ttu-id="38b53-134">W tej sekcji opisano niektóre specyficzne zachowania programu formatującego JSON przy użyciu domyślnego serializatora [JSON.NET](https://github.com/JamesNK/Newtonsoft.Json) .</span><span class="sxs-lookup"><span data-stu-id="38b53-134">This section describes some specific behaviors of the JSON formatter, using the default [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) serializer.</span></span> <span data-ttu-id="38b53-135">Nie jest to przeznaczone do kompleksowej dokumentacji biblioteki Json.NET. Aby uzyskać więcej informacji, zapoznaj się z [dokumentacją JSON.NET](http://james.newtonking.com/projects/json/help/).</span><span class="sxs-lookup"><span data-stu-id="38b53-135">This is not meant to be comprehensive documentation of the Json.NET library; for more information, see the [Json.NET Documentation](http://james.newtonking.com/projects/json/help/).</span></span>

#### <a name="what-gets-serialized"></a><span data-ttu-id="38b53-136">Co jest serializowane?</span><span class="sxs-lookup"><span data-stu-id="38b53-136">What Gets Serialized?</span></span>

<span data-ttu-id="38b53-137">Domyślnie wszystkie właściwości publiczne i pola są zawarte w serializowanym kodzie JSON.</span><span class="sxs-lookup"><span data-stu-id="38b53-137">By default, all public properties and fields are included in the serialized JSON.</span></span> <span data-ttu-id="38b53-138">Aby pominąć właściwość lub pole, dekorować je z atrybutem **JsonIgnore** .</span><span class="sxs-lookup"><span data-stu-id="38b53-138">To omit a property or field, decorate it with the **JsonIgnore** attribute.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample2.cs)]

<span data-ttu-id="38b53-139">Jeśli wolisz &quot;podejście&quot;, dekorować klasę z atrybutem **DataContract** .</span><span class="sxs-lookup"><span data-stu-id="38b53-139">If you prefer an &quot;opt-in&quot; approach, decorate the class with the **DataContract** attribute.</span></span> <span data-ttu-id="38b53-140">Jeśli ten atrybut jest obecny, elementy członkowskie są ignorowane, chyba że mają **element członkowski elementu członkowskiego**.</span><span class="sxs-lookup"><span data-stu-id="38b53-140">If this attribute is present, members are ignored unless they have the **DataMember**.</span></span> <span data-ttu-id="38b53-141">Można również użyć **elementu DataMember** do serializacji prywatnych członków.</span><span class="sxs-lookup"><span data-stu-id="38b53-141">You can also use **DataMember** to serialize private members.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample3.cs)]

<a id="json_readonly"></a>
### <a name="read-only-properties"></a><span data-ttu-id="38b53-142">Właściwości tylko do odczytu</span><span class="sxs-lookup"><span data-stu-id="38b53-142">Read-Only Properties</span></span>

<span data-ttu-id="38b53-143">Właściwości tylko do odczytu są domyślnie serializowane.</span><span class="sxs-lookup"><span data-stu-id="38b53-143">Read-only properties are serialized by default.</span></span>

<a id="json_dates"></a>
### <a name="dates"></a><span data-ttu-id="38b53-144">Daty</span><span class="sxs-lookup"><span data-stu-id="38b53-144">Dates</span></span>

<span data-ttu-id="38b53-145">Domyślnie Json.NET zapisuje daty w formacie [ISO 8601](http://www.w3.org/TR/NOTE-datetime) .</span><span class="sxs-lookup"><span data-stu-id="38b53-145">By default, Json.NET writes dates in [ISO 8601](http://www.w3.org/TR/NOTE-datetime) format.</span></span> <span data-ttu-id="38b53-146">Daty w formacie UTC (uniwersalny czas koordynowany) są zapisywane przy użyciu sufiksu "Z".</span><span class="sxs-lookup"><span data-stu-id="38b53-146">Dates in UTC (Coordinated Universal Time) are written with a "Z" suffix.</span></span> <span data-ttu-id="38b53-147">Daty w lokalnym czasie obejmują przesunięcie strefy czasowej.</span><span class="sxs-lookup"><span data-stu-id="38b53-147">Dates in local time include a time-zone offset.</span></span> <span data-ttu-id="38b53-148">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="38b53-148">For example:</span></span>

[!code-console[Main](json-and-xml-serialization/samples/sample4.cmd)]

<span data-ttu-id="38b53-149">Domyślnie Json.NET zachowuje strefę czasową.</span><span class="sxs-lookup"><span data-stu-id="38b53-149">By default, Json.NET preserves the time zone.</span></span> <span data-ttu-id="38b53-150">Można zastąpić to ustawienie właściwości DateTimeZoneHandling:</span><span class="sxs-lookup"><span data-stu-id="38b53-150">You can override this by setting the DateTimeZoneHandling property:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample5.cs)]

<span data-ttu-id="38b53-151">Jeśli wolisz używać [formatu daty JSON firmy Microsoft](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) zamiast ISO 8601, ustaw właściwość **DateFormatHandling** w ustawieniach serializatora:</span><span class="sxs-lookup"><span data-stu-id="38b53-151">If you prefer to use [Microsoft JSON date format](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) instead of ISO 8601, set the **DateFormatHandling** property on the serializer settings:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample6.cs)]

<a id="json_indenting"></a>
### <a name="indenting"></a><span data-ttu-id="38b53-152">Wcięcia</span><span class="sxs-lookup"><span data-stu-id="38b53-152">Indenting</span></span>

<span data-ttu-id="38b53-153">Aby zapisać **w formacie JSON** z wcięciem, ustaw formatowanie formatowania **. wcięcia**:</span><span class="sxs-lookup"><span data-stu-id="38b53-153">To write indented JSON, set the **Formatting** setting to **Formatting.Indented**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample7.cs)]

<a id="json_camelcasing"></a>
### <a name="camel-casing"></a><span data-ttu-id="38b53-154">Notacji CamelCase</span><span class="sxs-lookup"><span data-stu-id="38b53-154">Camel Casing</span></span>

<span data-ttu-id="38b53-155">Aby zapisać nazwy właściwości JSON z notacji CamelCase wielkością liter, bez zmiany modelu danych, ustaw **CamelCasePropertyNamesContractResolver** w serializatorze:</span><span class="sxs-lookup"><span data-stu-id="38b53-155">To write JSON property names with camel casing, without changing your data model, set the **CamelCasePropertyNamesContractResolver** on the serializer:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample8.cs)]

<a id="json_anon"></a>
### <a name="anonymous-and-weakly-typed-objects"></a><span data-ttu-id="38b53-156">Obiekty anonimowe i słabo wpisane</span><span class="sxs-lookup"><span data-stu-id="38b53-156">Anonymous and Weakly-Typed Objects</span></span>

<span data-ttu-id="38b53-157">Metoda akcji może zwracać obiekt anonimowy i serializować go do formatu JSON.</span><span class="sxs-lookup"><span data-stu-id="38b53-157">An action method can return an anonymous object and serialize it to JSON.</span></span> <span data-ttu-id="38b53-158">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="38b53-158">For example:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample9.cs)]

<span data-ttu-id="38b53-159">Treść komunikatu odpowiedzi będzie zawierać następujący kod JSON:</span><span class="sxs-lookup"><span data-stu-id="38b53-159">The response message body will contain the following JSON:</span></span>

[!code-json[Main](json-and-xml-serialization/samples/sample10.json)]

<span data-ttu-id="38b53-160">Jeśli internetowy interfejs API odbiera luźno strukturalne obiekty JSON od klientów, można zdeserializować treści żądania do typu **Newtonsoft. JSON. LINQ. JObject** .</span><span class="sxs-lookup"><span data-stu-id="38b53-160">If your web API receives loosely structured JSON objects from clients, you can deserialize the request body to a **Newtonsoft.Json.Linq.JObject** type.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample11.cs)]

<span data-ttu-id="38b53-161">Jednak zazwyczaj lepiej jest używać obiektów danych o jednoznacznie określonym typie.</span><span class="sxs-lookup"><span data-stu-id="38b53-161">However, it is usually better to use strongly typed data objects.</span></span> <span data-ttu-id="38b53-162">Następnie nie musisz samodzielnie analizować danych i uzyskać korzyści związane z walidacją modelu.</span><span class="sxs-lookup"><span data-stu-id="38b53-162">Then you don't need to parse the data yourself, and you get the benefits of model validation.</span></span>

<span data-ttu-id="38b53-163">Serializator XML nie obsługuje typów anonimowych ani wystąpień **JObject** .</span><span class="sxs-lookup"><span data-stu-id="38b53-163">The XML serializer does not support anonymous types or **JObject** instances.</span></span> <span data-ttu-id="38b53-164">Jeśli używasz tych funkcji dla danych JSON, Usuń program formatujący XML z potoku, zgodnie z opisem w dalszej części tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="38b53-164">If you use these features for your JSON data, you should remove the XML formatter from the pipeline, as described later in this article.</span></span>

<a id="xml_media_type_formatter"></a>
## <a name="xml-media-type-formatter"></a><span data-ttu-id="38b53-165">Program formatujący typ multimediów XML</span><span class="sxs-lookup"><span data-stu-id="38b53-165">XML Media-Type Formatter</span></span>

<span data-ttu-id="38b53-166">Formatowanie XML jest dostarczane przez klasę **XmlMediaTypeFormatter** .</span><span class="sxs-lookup"><span data-stu-id="38b53-166">XML formatting is provided by the **XmlMediaTypeFormatter** class.</span></span> <span data-ttu-id="38b53-167">Domyślnie **XmlMediaTypeFormatter** używa klasy **DataContractSerializer** do wykonywania serializacji.</span><span class="sxs-lookup"><span data-stu-id="38b53-167">By default, **XmlMediaTypeFormatter** uses the **DataContractSerializer** class to perform serialization.</span></span>

<span data-ttu-id="38b53-168">Jeśli wolisz, możesz skonfigurować **XmlMediaTypeFormatter** do korzystania z **XmlSerializer** zamiast **DataContractSerializer**.</span><span class="sxs-lookup"><span data-stu-id="38b53-168">If you prefer, you can configure the **XmlMediaTypeFormatter** to use the **XmlSerializer** instead of the **DataContractSerializer**.</span></span> <span data-ttu-id="38b53-169">Aby to zrobić, ustaw właściwość **UseXmlSerializer** na **wartość true**:</span><span class="sxs-lookup"><span data-stu-id="38b53-169">To do so, set the **UseXmlSerializer** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample12.cs)]

<span data-ttu-id="38b53-170">Klasa **XmlSerializer** obsługuje węższy zestaw typów niż element **DataContractSerializer**, ale daje większą kontrolę nad wynikiem XML.</span><span class="sxs-lookup"><span data-stu-id="38b53-170">The **XmlSerializer** class supports a narrower set of types than **DataContractSerializer**, but gives more control over the resulting XML.</span></span> <span data-ttu-id="38b53-171">Jeśli musisz dopasować istniejący schemat XML, rozważ użycie **elementu XmlSerializer** .</span><span class="sxs-lookup"><span data-stu-id="38b53-171">Consider using **XmlSerializer** if you need to match an existing XML schema.</span></span>

### <a name="xml-serialization"></a><span data-ttu-id="38b53-172">Serializacji XML</span><span class="sxs-lookup"><span data-stu-id="38b53-172">XML Serialization</span></span>

<span data-ttu-id="38b53-173">W tej sekcji opisano niektóre specyficzne zachowania programu formatującego XML przy użyciu domyślnego obiektu **DataContractSerializer**.</span><span class="sxs-lookup"><span data-stu-id="38b53-173">This section describes some specific behaviors of the XML formatter, using the default **DataContractSerializer**.</span></span>

<span data-ttu-id="38b53-174">Domyślnie DataContractSerializer zachowuje się w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="38b53-174">By default, the DataContractSerializer behaves as follows:</span></span>

- <span data-ttu-id="38b53-175">Wszystkie publiczne właściwości odczytu/zapisu i pola są serializowane.</span><span class="sxs-lookup"><span data-stu-id="38b53-175">All public read/write properties and fields are serialized.</span></span> <span data-ttu-id="38b53-176">Aby pominąć właściwość lub pole, dekorować je z atrybutem **IgnoreDataMember** .</span><span class="sxs-lookup"><span data-stu-id="38b53-176">To omit a property or field, decorate it with the **IgnoreDataMember** attribute.</span></span>
- <span data-ttu-id="38b53-177">Prywatne i chronione składowe nie są serializowane.</span><span class="sxs-lookup"><span data-stu-id="38b53-177">Private and protected members are not serialized.</span></span>
- <span data-ttu-id="38b53-178">Właściwości tylko do odczytu nie są serializowane.</span><span class="sxs-lookup"><span data-stu-id="38b53-178">Read-only properties are not serialized.</span></span> <span data-ttu-id="38b53-179">(Jednak zawartość właściwości kolekcji tylko do odczytu jest serializowana).</span><span class="sxs-lookup"><span data-stu-id="38b53-179">(However, the contents of a read-only collection property are serialized.)</span></span>
- <span data-ttu-id="38b53-180">Nazwy klas i elementów członkowskich są zapisywane w kodzie XML dokładnie tak, jak pojawiają się one w deklaracji klasy.</span><span class="sxs-lookup"><span data-stu-id="38b53-180">Class and member names are written in the XML exactly as they appear in the class declaration.</span></span>
- <span data-ttu-id="38b53-181">Zostanie użyta domyślna przestrzeń nazw XML.</span><span class="sxs-lookup"><span data-stu-id="38b53-181">A default XML namespace is used.</span></span>

<span data-ttu-id="38b53-182">Jeśli potrzebujesz większej kontroli nad serializacji, możesz dekorować klasę przy użyciu atrybutu **DataContract** .</span><span class="sxs-lookup"><span data-stu-id="38b53-182">If you need more control over the serialization, you can decorate the class with the **DataContract** attribute.</span></span> <span data-ttu-id="38b53-183">Gdy ten atrybut jest obecny, Klasa jest serializowana w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="38b53-183">When this attribute is present, the class is serialized as follows:</span></span>

- <span data-ttu-id="38b53-184">&quot;opt&quot; podejście: właściwości i pola nie są domyślnie serializowane.</span><span class="sxs-lookup"><span data-stu-id="38b53-184">&quot;Opt in&quot; approach: Properties and fields are not serialized by default.</span></span> <span data-ttu-id="38b53-185">Aby serializować właściwość lub pole, dekorować je z atrybutem **DataMember** .</span><span class="sxs-lookup"><span data-stu-id="38b53-185">To serialize a property or field, decorate it with the **DataMember** attribute.</span></span>
- <span data-ttu-id="38b53-186">Aby serializować prywatną lub chronioną składową, dekorować ją z atrybutem **DataMember** .</span><span class="sxs-lookup"><span data-stu-id="38b53-186">To serialize a private or protected member, decorate it with the **DataMember** attribute.</span></span>
- <span data-ttu-id="38b53-187">Właściwości tylko do odczytu nie są serializowane.</span><span class="sxs-lookup"><span data-stu-id="38b53-187">Read-only properties are not serialized.</span></span>
- <span data-ttu-id="38b53-188">Aby zmienić sposób wyświetlania nazwy klasy w kodzie XML, należy ustawić parametr *name* w atrybucie **DataContract** .</span><span class="sxs-lookup"><span data-stu-id="38b53-188">To change how the class name appears in the XML, set the *Name* parameter in the **DataContract** attribute.</span></span>
- <span data-ttu-id="38b53-189">Aby zmienić sposób wyświetlania nazwy elementu członkowskiego w kodzie XML, należy ustawić parametr *name* w atrybucie **DataMember** .</span><span class="sxs-lookup"><span data-stu-id="38b53-189">To change how a member name appears in the XML, set the *Name* parameter in the **DataMember** attribute.</span></span>
- <span data-ttu-id="38b53-190">Aby zmienić przestrzeń nazw XML, ustaw parametr *Namespace* w klasie **DataContract** .</span><span class="sxs-lookup"><span data-stu-id="38b53-190">To change the XML namespace, set the *Namespace* parameter in the **DataContract** class.</span></span>

<a id="xml_readonly"></a>
### <a name="read-only-properties"></a><span data-ttu-id="38b53-191">Właściwości tylko do odczytu</span><span class="sxs-lookup"><span data-stu-id="38b53-191">Read-Only Properties</span></span>

<span data-ttu-id="38b53-192">Właściwości tylko do odczytu nie są serializowane.</span><span class="sxs-lookup"><span data-stu-id="38b53-192">Read-only properties are not serialized.</span></span> <span data-ttu-id="38b53-193">Jeśli właściwość tylko do odczytu ma prywatne pole zapasowe, można oznaczyć pole prywatne z atrybutem **DataMember** .</span><span class="sxs-lookup"><span data-stu-id="38b53-193">If a read-only property has a backing private field, you can mark the private field with the **DataMember** attribute.</span></span> <span data-ttu-id="38b53-194">To podejście wymaga atrybutu **DataContract** dla klasy.</span><span class="sxs-lookup"><span data-stu-id="38b53-194">This approach requires the **DataContract** attribute on the class.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample13.cs)]

<a id="xml_dates"></a>
### <a name="dates"></a><span data-ttu-id="38b53-195">Daty</span><span class="sxs-lookup"><span data-stu-id="38b53-195">Dates</span></span>

<span data-ttu-id="38b53-196">Daty są zapisywane w formacie ISO 8601.</span><span class="sxs-lookup"><span data-stu-id="38b53-196">Dates are written in ISO 8601 format.</span></span> <span data-ttu-id="38b53-197">Na przykład &quot;2012-05-23T20:21:37.9116538 Z&quot;.</span><span class="sxs-lookup"><span data-stu-id="38b53-197">For example, &quot;2012-05-23T20:21:37.9116538Z&quot;.</span></span>

<a id="xml_indenting"></a>
### <a name="indenting"></a><span data-ttu-id="38b53-198">Wcięcia</span><span class="sxs-lookup"><span data-stu-id="38b53-198">Indenting</span></span>

<span data-ttu-id="38b53-199">Aby napisać plik XML z wcięciem, ustaw właściwość **wcięcie** na **true**:</span><span class="sxs-lookup"><span data-stu-id="38b53-199">To write indented XML, set the **Indent** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample14.cs)]

<a id="xml_pertype"></a>
## <a name="setting-per-type-xml-serializers"></a><span data-ttu-id="38b53-200">Ustawianie serializatorów XML dla poszczególnych typów</span><span class="sxs-lookup"><span data-stu-id="38b53-200">Setting Per-Type XML Serializers</span></span>

<span data-ttu-id="38b53-201">Można ustawić różne serializacji XML dla różnych typów CLR.</span><span class="sxs-lookup"><span data-stu-id="38b53-201">You can set different XML serializers for different CLR types.</span></span> <span data-ttu-id="38b53-202">Na przykład może istnieć konkretny obiekt danych, który wymaga **XmlSerializer** w celu zapewnienia zgodności z poprzednimi wersjami.</span><span class="sxs-lookup"><span data-stu-id="38b53-202">For example, you might have a particular data object that requires **XmlSerializer** for backward compatibility.</span></span> <span data-ttu-id="38b53-203">Można użyć **elementu XmlSerializer** dla tego obiektu i nadal używać klasy **DataContractSerializer** dla innych typów.</span><span class="sxs-lookup"><span data-stu-id="38b53-203">You can use **XmlSerializer** for this object and continue to use **DataContractSerializer** for other types.</span></span>

<span data-ttu-id="38b53-204">Aby ustawić Serializator XML dla określonego typu, wywołaj metodę **setserializer**.</span><span class="sxs-lookup"><span data-stu-id="38b53-204">To set an XML serializer for a particular type, call **SetSerializer**.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample15.cs)]

<span data-ttu-id="38b53-205">Można określić element **XmlSerializer** lub obiekt, który pochodzi od **XmlObjectSerializer**.</span><span class="sxs-lookup"><span data-stu-id="38b53-205">You can specify an **XmlSerializer** or any object that derives from **XmlObjectSerializer**.</span></span>

<a id="removing_the_json_or_xml_formatter"></a>
## <a name="removing-the-json-or-xml-formatter"></a><span data-ttu-id="38b53-206">Usuwanie elementu formatującego JSON lub XML</span><span class="sxs-lookup"><span data-stu-id="38b53-206">Removing the JSON or XML Formatter</span></span>

<span data-ttu-id="38b53-207">Można usunąć program formatujący JSON lub program formatujący XML z listy elementów formatujących, jeśli nie chcesz ich używać.</span><span class="sxs-lookup"><span data-stu-id="38b53-207">You can remove the JSON formatter or the XML formatter from the list of formatters, if you do not want to use them.</span></span> <span data-ttu-id="38b53-208">Oto najważniejsze przyczyny tego:</span><span class="sxs-lookup"><span data-stu-id="38b53-208">The main reasons to do this are:</span></span>

- <span data-ttu-id="38b53-209">Aby ograniczyć odpowiedzi interfejsu API sieci Web do określonego typu nośnika.</span><span class="sxs-lookup"><span data-stu-id="38b53-209">To restrict your web API responses to a particular media type.</span></span> <span data-ttu-id="38b53-210">Na przykład możesz zdecydować się na obsługę tylko odpowiedzi JSON i usunąć program formatujący XML.</span><span class="sxs-lookup"><span data-stu-id="38b53-210">For example, you might decide to support only JSON responses, and remove the XML formatter.</span></span>
- <span data-ttu-id="38b53-211">Zastępowanie domyślnego programu formatującego przy użyciu niestandardowego programu formatującego.</span><span class="sxs-lookup"><span data-stu-id="38b53-211">To replace the default formatter with a custom formatter.</span></span> <span data-ttu-id="38b53-212">Na przykład można zastąpić program formatujący JSON własną niestandardową implementacją programu formatującego JSON.</span><span class="sxs-lookup"><span data-stu-id="38b53-212">For example, you could replace the JSON formatter with your own custom implementation of a JSON formatter.</span></span>

<span data-ttu-id="38b53-213">Poniższy kod przedstawia sposób usuwania domyślnych programów formatującego.</span><span class="sxs-lookup"><span data-stu-id="38b53-213">The following code shows how to remove the default formatters.</span></span> <span data-ttu-id="38b53-214">Wywołaj ten program z poziomu **aplikacji,\_Metoda startowa** zdefiniowana w elemencie Global. asax.</span><span class="sxs-lookup"><span data-stu-id="38b53-214">Call this from your **Application\_Start** method, defined in Global.asax.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample16.cs)]

<a id="handling_circular_object_references"></a>
## <a name="handling-circular-object-references"></a><span data-ttu-id="38b53-215">Obsługa cyklicznych odwołań do obiektów</span><span class="sxs-lookup"><span data-stu-id="38b53-215">Handling Circular Object References</span></span>

<span data-ttu-id="38b53-216">Domyślnie elementy formatujące JSON i XML zapisują wszystkie obiekty jako wartości.</span><span class="sxs-lookup"><span data-stu-id="38b53-216">By default, the JSON and XML formatters write all objects as values.</span></span> <span data-ttu-id="38b53-217">Jeśli dwie właściwości odwołują się do tego samego obiektu, lub jeśli ten sam obiekt występuje dwa razy w kolekcji, program formatujący będzie serializować obiekt dwa razy.</span><span class="sxs-lookup"><span data-stu-id="38b53-217">If two properties refer to the same object, or if the same object appears twice in a collection, the formatter will serialize the object twice.</span></span> <span data-ttu-id="38b53-218">Jest to konkretny problem, jeśli wykres obiektu zawiera cykle, ponieważ serializator zgłosi wyjątek, gdy wykryje pętlę na grafie.</span><span class="sxs-lookup"><span data-stu-id="38b53-218">This is a particular problem if your object graph contains cycles, because the serializer will throw an exception when it detects a loop in the graph.</span></span>

<span data-ttu-id="38b53-219">Należy wziąć pod uwagę następujące modele obiektów i kontroler.</span><span class="sxs-lookup"><span data-stu-id="38b53-219">Consider the following object models and controller.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample17.cs)]

<span data-ttu-id="38b53-220">Wywołanie tej akcji spowoduje, że program formatujący wymusił wyjątek, który zostanie przetłumaczony na kod stanu 500 (wewnętrzny błąd serwera) odpowiedzi na klienta.</span><span class="sxs-lookup"><span data-stu-id="38b53-220">Invoking this action will cause the formatter to thrown an exception, which translates to a status code 500 (Internal Server Error) response to the client.</span></span>

<span data-ttu-id="38b53-221">Aby zachować odwołania do obiektów w formacie JSON, Dodaj następujący kod do metody **Start\_aplikacji** w pliku Global. asax:</span><span class="sxs-lookup"><span data-stu-id="38b53-221">To preserve object references in JSON, add the following code to **Application\_Start** method in the Global.asax file:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample18.cs)]

<span data-ttu-id="38b53-222">Teraz akcja kontrolera zwróci kod JSON, który będzie wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="38b53-222">Now the controller action will return JSON that looks like this:</span></span>

[!code-json[Main](json-and-xml-serialization/samples/sample19.json)]

<span data-ttu-id="38b53-223">Należy zauważyć, że serializator dodaje właściwość &quot;$id&quot; do obydwu obiektów.</span><span class="sxs-lookup"><span data-stu-id="38b53-223">Notice that the serializer adds an &quot;$id&quot; property to both objects.</span></span> <span data-ttu-id="38b53-224">Wykrywa także, że właściwość Employee. Department tworzy pętlę, więc zastępuje ją odwołaniem do obiektu: {&quot;$ref&quot;:&quot;1&quot;}.</span><span class="sxs-lookup"><span data-stu-id="38b53-224">Also, it detects that the Employee.Department property creates a loop, so it replaces the value with an object reference: {&quot;$ref&quot;:&quot;1&quot;}.</span></span>

> [!NOTE]
> <span data-ttu-id="38b53-225">Odwołania do obiektów nie są standardowe w formacie JSON.</span><span class="sxs-lookup"><span data-stu-id="38b53-225">Object references are not standard in JSON.</span></span> <span data-ttu-id="38b53-226">Przed użyciem tej funkcji należy rozważyć, czy klienci będą mogli analizować wyniki.</span><span class="sxs-lookup"><span data-stu-id="38b53-226">Before using this feature, consider whether your clients will be able to parse the results.</span></span> <span data-ttu-id="38b53-227">Może być lepiej po prostu usunąć cykle z grafu.</span><span class="sxs-lookup"><span data-stu-id="38b53-227">It might be better simply to remove cycles from the graph.</span></span> <span data-ttu-id="38b53-228">Na przykład łącze od pracownika z powrotem do działu nie jest naprawdę potrzebne w tym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="38b53-228">For example, the link from Employee back to Department is not really needed in this example.</span></span>

<span data-ttu-id="38b53-229">Aby zachować odwołania do obiektów w kodzie XML, dostępne są dwie opcje.</span><span class="sxs-lookup"><span data-stu-id="38b53-229">To preserve object references in XML, you have two options.</span></span> <span data-ttu-id="38b53-230">Łatwiejszym rozwiązaniem jest dodanie `[DataContract(IsReference=true)]` do klasy modelu.</span><span class="sxs-lookup"><span data-stu-id="38b53-230">The simpler option is to add `[DataContract(IsReference=true)]` to your model class.</span></span> <span data-ttu-id="38b53-231">Parametr *IsReference* służy do włączania odwołań do obiektów.</span><span class="sxs-lookup"><span data-stu-id="38b53-231">The *IsReference* parameter enables object references.</span></span> <span data-ttu-id="38b53-232">Należy pamiętać, że element **DataContract** sprawia, że Serializacja jest konieczna, więc należy również dodać atrybuty **elementu członkowskiego** do właściwości:</span><span class="sxs-lookup"><span data-stu-id="38b53-232">Remember that **DataContract** makes serialization opt-in, so you will also need to add **DataMember** attributes to the properties:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample20.cs)]

<span data-ttu-id="38b53-233">Teraz w programie formatującego zostanie wygenerowane XML podobne do następującego:</span><span class="sxs-lookup"><span data-stu-id="38b53-233">Now the formatter will produce XML similar to following:</span></span>

[!code-xml[Main](json-and-xml-serialization/samples/sample21.xml)]

<span data-ttu-id="38b53-234">Jeśli chcesz uniknąć atrybutów klasy modelu, istnieje inna opcja: Utwórz nowe wystąpienie **DataContractSerializer** specyficzne dla typu i ustaw *preserveObjectReferences* na **true** w konstruktorze.</span><span class="sxs-lookup"><span data-stu-id="38b53-234">If you want to avoid attributes on your model class, there is another option: Create a new type-specific **DataContractSerializer** instance and set *preserveObjectReferences* to **true** in the constructor.</span></span> <span data-ttu-id="38b53-235">Następnie ustaw to wystąpienie jako serializator dla poszczególnych typów w programie formatującego typu multimediów XML.</span><span class="sxs-lookup"><span data-stu-id="38b53-235">Then set this instance as a per-type serializer on the XML media-type formatter.</span></span> <span data-ttu-id="38b53-236">Poniższy kod pokazuje, jak to zrobić:</span><span class="sxs-lookup"><span data-stu-id="38b53-236">The following code show how to do this:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample22.cs?highlight=3)]

<a id="testing_object_serialization"></a>
## <a name="testing-object-serialization"></a><span data-ttu-id="38b53-237">Testowanie serializacji obiektu</span><span class="sxs-lookup"><span data-stu-id="38b53-237">Testing Object Serialization</span></span>

<span data-ttu-id="38b53-238">Podczas projektowania interfejsu API sieci Web warto przetestować sposób serializacji obiektów danych.</span><span class="sxs-lookup"><span data-stu-id="38b53-238">As you design your web API, it is useful to test how your data objects will be serialized.</span></span> <span data-ttu-id="38b53-239">Można to zrobić bez tworzenia kontrolera ani wywoływania akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="38b53-239">You can do this without creating a controller or invoking a controller action.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample23.cs)]

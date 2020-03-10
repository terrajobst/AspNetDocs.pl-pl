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
# <a name="json-and-xml-serialization-in-aspnet-web-api"></a>Serializacja JSON i XML w interfejsie API sieci Web ASP.NET

według [Jan Wasson](https://github.com/MikeWasson)

W tym artykule opisano elementy formatujące JSON i XML w interfejsie Web API ASP.NET.

W interfejsie API sieci Web *ASP.NET jest to* obiekt, który może:

- Odczytywanie obiektów CLR z treści wiadomości HTTP
- Pisanie obiektów CLR w treści wiadomości HTTP

Interfejs API sieci Web udostępnia elementy formatujące typy multimediów dla danych JSON i XML. Struktura wstawia domyślnie te elementy formatujące do potoku. Klienci mogą żądać elementu JSON lub XML w nagłówku Accept żądania HTTP.

## <a name="contents"></a>Spis treści

- [Program formatujący typ multimediów JSON](#json_media_type_formatter)

    - [Właściwości tylko do odczytu](#json_readonly)
    - [Okres](#json_dates)
    - [Wcięcia](#json_indenting)
    - [Notacji CamelCase](#json_camelcasing)
    - [Obiekty anonimowe i słabo wpisane](#json_anon)
- [Program formatujący typ multimediów XML](#xml_media_type_formatter)

    - [Właściwości tylko do odczytu](#xml_readonly)
    - [Okres](#xml_dates)
    - [Wcięcia](#xml_indenting)
    - [Ustawianie serializatorów XML dla poszczególnych typów](#xml_pertype)
- [Usuwanie elementu formatującego JSON lub XML](#removing_the_json_or_xml_formatter)
- [Obsługa cyklicznych odwołań do obiektów](#handling_circular_object_references)
- [Testowanie serializacji obiektu](#testing_object_serialization)

<a id="json_media_type_formatter"></a>
## <a name="json-media-type-formatter"></a>Program formatujący typ multimediów JSON

Formatowanie JSON jest dostarczane przez klasę **JsonMediaTypeFormatter** . Domyślnie **JsonMediaTypeFormatter** używa biblioteki [JSON.NET](https://github.com/JamesNK/Newtonsoft.Json) do wykonywania serializacji. Json.NET to projekt typu "open source" innej firmy.

Jeśli wolisz, możesz skonfigurować klasę **JsonMediaTypeFormatter** , aby korzystała z **Klasa DataContractJsonSerializer** zamiast JSON.NET. Aby to zrobić, ustaw właściwość **UseDataContractJsonSerializer** na **wartość true**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample1.cs)]

### <a name="json-serialization"></a>Serializacja JSON

W tej sekcji opisano niektóre specyficzne zachowania programu formatującego JSON przy użyciu domyślnego serializatora [JSON.NET](https://github.com/JamesNK/Newtonsoft.Json) . Nie jest to przeznaczone do kompleksowej dokumentacji biblioteki Json.NET. Aby uzyskać więcej informacji, zapoznaj się z [dokumentacją JSON.NET](http://james.newtonking.com/projects/json/help/).

#### <a name="what-gets-serialized"></a>Co jest serializowane?

Domyślnie wszystkie właściwości publiczne i pola są zawarte w serializowanym kodzie JSON. Aby pominąć właściwość lub pole, dekorować je z atrybutem **JsonIgnore** .

[!code-csharp[Main](json-and-xml-serialization/samples/sample2.cs)]

Jeśli wolisz &quot;podejście&quot;, dekorować klasę z atrybutem **DataContract** . Jeśli ten atrybut jest obecny, elementy członkowskie są ignorowane, chyba że mają **element członkowski elementu członkowskiego**. Można również użyć **elementu DataMember** do serializacji prywatnych członków.

[!code-csharp[Main](json-and-xml-serialization/samples/sample3.cs)]

<a id="json_readonly"></a>
### <a name="read-only-properties"></a>Właściwości tylko do odczytu

Właściwości tylko do odczytu są domyślnie serializowane.

<a id="json_dates"></a>
### <a name="dates"></a>Daty

Domyślnie Json.NET zapisuje daty w formacie [ISO 8601](http://www.w3.org/TR/NOTE-datetime) . Daty w formacie UTC (uniwersalny czas koordynowany) są zapisywane przy użyciu sufiksu "Z". Daty w lokalnym czasie obejmują przesunięcie strefy czasowej. Na przykład:

[!code-console[Main](json-and-xml-serialization/samples/sample4.cmd)]

Domyślnie Json.NET zachowuje strefę czasową. Można zastąpić to ustawienie właściwości DateTimeZoneHandling:

[!code-csharp[Main](json-and-xml-serialization/samples/sample5.cs)]

Jeśli wolisz używać [formatu daty JSON firmy Microsoft](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) zamiast ISO 8601, ustaw właściwość **DateFormatHandling** w ustawieniach serializatora:

[!code-csharp[Main](json-and-xml-serialization/samples/sample6.cs)]

<a id="json_indenting"></a>
### <a name="indenting"></a>Wcięcia

Aby zapisać **w formacie JSON** z wcięciem, ustaw formatowanie formatowania **. wcięcia**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample7.cs)]

<a id="json_camelcasing"></a>
### <a name="camel-casing"></a>Notacji CamelCase

Aby zapisać nazwy właściwości JSON z notacji CamelCase wielkością liter, bez zmiany modelu danych, ustaw **CamelCasePropertyNamesContractResolver** w serializatorze:

[!code-csharp[Main](json-and-xml-serialization/samples/sample8.cs)]

<a id="json_anon"></a>
### <a name="anonymous-and-weakly-typed-objects"></a>Obiekty anonimowe i słabo wpisane

Metoda akcji może zwracać obiekt anonimowy i serializować go do formatu JSON. Na przykład:

[!code-csharp[Main](json-and-xml-serialization/samples/sample9.cs)]

Treść komunikatu odpowiedzi będzie zawierać następujący kod JSON:

[!code-json[Main](json-and-xml-serialization/samples/sample10.json)]

Jeśli internetowy interfejs API odbiera luźno strukturalne obiekty JSON od klientów, można zdeserializować treści żądania do typu **Newtonsoft. JSON. LINQ. JObject** .

[!code-csharp[Main](json-and-xml-serialization/samples/sample11.cs)]

Jednak zazwyczaj lepiej jest używać obiektów danych o jednoznacznie określonym typie. Następnie nie musisz samodzielnie analizować danych i uzyskać korzyści związane z walidacją modelu.

Serializator XML nie obsługuje typów anonimowych ani wystąpień **JObject** . Jeśli używasz tych funkcji dla danych JSON, Usuń program formatujący XML z potoku, zgodnie z opisem w dalszej części tego artykułu.

<a id="xml_media_type_formatter"></a>
## <a name="xml-media-type-formatter"></a>Program formatujący typ multimediów XML

Formatowanie XML jest dostarczane przez klasę **XmlMediaTypeFormatter** . Domyślnie **XmlMediaTypeFormatter** używa klasy **DataContractSerializer** do wykonywania serializacji.

Jeśli wolisz, możesz skonfigurować **XmlMediaTypeFormatter** do korzystania z **XmlSerializer** zamiast **DataContractSerializer**. Aby to zrobić, ustaw właściwość **UseXmlSerializer** na **wartość true**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample12.cs)]

Klasa **XmlSerializer** obsługuje węższy zestaw typów niż element **DataContractSerializer**, ale daje większą kontrolę nad wynikiem XML. Jeśli musisz dopasować istniejący schemat XML, rozważ użycie **elementu XmlSerializer** .

### <a name="xml-serialization"></a>Serializacji XML

W tej sekcji opisano niektóre specyficzne zachowania programu formatującego XML przy użyciu domyślnego obiektu **DataContractSerializer**.

Domyślnie DataContractSerializer zachowuje się w następujący sposób:

- Wszystkie publiczne właściwości odczytu/zapisu i pola są serializowane. Aby pominąć właściwość lub pole, dekorować je z atrybutem **IgnoreDataMember** .
- Prywatne i chronione składowe nie są serializowane.
- Właściwości tylko do odczytu nie są serializowane. (Jednak zawartość właściwości kolekcji tylko do odczytu jest serializowana).
- Nazwy klas i elementów członkowskich są zapisywane w kodzie XML dokładnie tak, jak pojawiają się one w deklaracji klasy.
- Zostanie użyta domyślna przestrzeń nazw XML.

Jeśli potrzebujesz większej kontroli nad serializacji, możesz dekorować klasę przy użyciu atrybutu **DataContract** . Gdy ten atrybut jest obecny, Klasa jest serializowana w następujący sposób:

- &quot;opt&quot; podejście: właściwości i pola nie są domyślnie serializowane. Aby serializować właściwość lub pole, dekorować je z atrybutem **DataMember** .
- Aby serializować prywatną lub chronioną składową, dekorować ją z atrybutem **DataMember** .
- Właściwości tylko do odczytu nie są serializowane.
- Aby zmienić sposób wyświetlania nazwy klasy w kodzie XML, należy ustawić parametr *name* w atrybucie **DataContract** .
- Aby zmienić sposób wyświetlania nazwy elementu członkowskiego w kodzie XML, należy ustawić parametr *name* w atrybucie **DataMember** .
- Aby zmienić przestrzeń nazw XML, ustaw parametr *Namespace* w klasie **DataContract** .

<a id="xml_readonly"></a>
### <a name="read-only-properties"></a>Właściwości tylko do odczytu

Właściwości tylko do odczytu nie są serializowane. Jeśli właściwość tylko do odczytu ma prywatne pole zapasowe, można oznaczyć pole prywatne z atrybutem **DataMember** . To podejście wymaga atrybutu **DataContract** dla klasy.

[!code-csharp[Main](json-and-xml-serialization/samples/sample13.cs)]

<a id="xml_dates"></a>
### <a name="dates"></a>Daty

Daty są zapisywane w formacie ISO 8601. Na przykład &quot;2012-05-23T20:21:37.9116538 Z&quot;.

<a id="xml_indenting"></a>
### <a name="indenting"></a>Wcięcia

Aby napisać plik XML z wcięciem, ustaw właściwość **wcięcie** na **true**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample14.cs)]

<a id="xml_pertype"></a>
## <a name="setting-per-type-xml-serializers"></a>Ustawianie serializatorów XML dla poszczególnych typów

Można ustawić różne serializacji XML dla różnych typów CLR. Na przykład może istnieć konkretny obiekt danych, który wymaga **XmlSerializer** w celu zapewnienia zgodności z poprzednimi wersjami. Można użyć **elementu XmlSerializer** dla tego obiektu i nadal używać klasy **DataContractSerializer** dla innych typów.

Aby ustawić Serializator XML dla określonego typu, wywołaj metodę **setserializer**.

[!code-csharp[Main](json-and-xml-serialization/samples/sample15.cs)]

Można określić element **XmlSerializer** lub obiekt, który pochodzi od **XmlObjectSerializer**.

<a id="removing_the_json_or_xml_formatter"></a>
## <a name="removing-the-json-or-xml-formatter"></a>Usuwanie elementu formatującego JSON lub XML

Można usunąć program formatujący JSON lub program formatujący XML z listy elementów formatujących, jeśli nie chcesz ich używać. Oto najważniejsze przyczyny tego:

- Aby ograniczyć odpowiedzi interfejsu API sieci Web do określonego typu nośnika. Na przykład możesz zdecydować się na obsługę tylko odpowiedzi JSON i usunąć program formatujący XML.
- Zastępowanie domyślnego programu formatującego przy użyciu niestandardowego programu formatującego. Na przykład można zastąpić program formatujący JSON własną niestandardową implementacją programu formatującego JSON.

Poniższy kod przedstawia sposób usuwania domyślnych programów formatującego. Wywołaj ten program z poziomu **aplikacji,\_Metoda startowa** zdefiniowana w elemencie Global. asax.

[!code-csharp[Main](json-and-xml-serialization/samples/sample16.cs)]

<a id="handling_circular_object_references"></a>
## <a name="handling-circular-object-references"></a>Obsługa cyklicznych odwołań do obiektów

Domyślnie elementy formatujące JSON i XML zapisują wszystkie obiekty jako wartości. Jeśli dwie właściwości odwołują się do tego samego obiektu, lub jeśli ten sam obiekt występuje dwa razy w kolekcji, program formatujący będzie serializować obiekt dwa razy. Jest to konkretny problem, jeśli wykres obiektu zawiera cykle, ponieważ serializator zgłosi wyjątek, gdy wykryje pętlę na grafie.

Należy wziąć pod uwagę następujące modele obiektów i kontroler.

[!code-csharp[Main](json-and-xml-serialization/samples/sample17.cs)]

Wywołanie tej akcji spowoduje, że program formatujący wymusił wyjątek, który zostanie przetłumaczony na kod stanu 500 (wewnętrzny błąd serwera) odpowiedzi na klienta.

Aby zachować odwołania do obiektów w formacie JSON, Dodaj następujący kod do metody **Start\_aplikacji** w pliku Global. asax:

[!code-csharp[Main](json-and-xml-serialization/samples/sample18.cs)]

Teraz akcja kontrolera zwróci kod JSON, który będzie wyglądać następująco:

[!code-json[Main](json-and-xml-serialization/samples/sample19.json)]

Należy zauważyć, że serializator dodaje właściwość &quot;$id&quot; do obydwu obiektów. Wykrywa także, że właściwość Employee. Department tworzy pętlę, więc zastępuje ją odwołaniem do obiektu: {&quot;$ref&quot;:&quot;1&quot;}.

> [!NOTE]
> Odwołania do obiektów nie są standardowe w formacie JSON. Przed użyciem tej funkcji należy rozważyć, czy klienci będą mogli analizować wyniki. Może być lepiej po prostu usunąć cykle z grafu. Na przykład łącze od pracownika z powrotem do działu nie jest naprawdę potrzebne w tym przykładzie.

Aby zachować odwołania do obiektów w kodzie XML, dostępne są dwie opcje. Łatwiejszym rozwiązaniem jest dodanie `[DataContract(IsReference=true)]` do klasy modelu. Parametr *IsReference* służy do włączania odwołań do obiektów. Należy pamiętać, że element **DataContract** sprawia, że Serializacja jest konieczna, więc należy również dodać atrybuty **elementu członkowskiego** do właściwości:

[!code-csharp[Main](json-and-xml-serialization/samples/sample20.cs)]

Teraz w programie formatującego zostanie wygenerowane XML podobne do następującego:

[!code-xml[Main](json-and-xml-serialization/samples/sample21.xml)]

Jeśli chcesz uniknąć atrybutów klasy modelu, istnieje inna opcja: Utwórz nowe wystąpienie **DataContractSerializer** specyficzne dla typu i ustaw *preserveObjectReferences* na **true** w konstruktorze. Następnie ustaw to wystąpienie jako serializator dla poszczególnych typów w programie formatującego typu multimediów XML. Poniższy kod pokazuje, jak to zrobić:

[!code-csharp[Main](json-and-xml-serialization/samples/sample22.cs?highlight=3)]

<a id="testing_object_serialization"></a>
## <a name="testing-object-serialization"></a>Testowanie serializacji obiektu

Podczas projektowania interfejsu API sieci Web warto przetestować sposób serializacji obiektów danych. Można to zrobić bez tworzenia kontrolera ani wywoływania akcji kontrolera.

[!code-csharp[Main](json-and-xml-serialization/samples/sample23.cs)]

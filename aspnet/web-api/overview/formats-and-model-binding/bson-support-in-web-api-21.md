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
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622298"
---
# <a name="bson-support-in-aspnet-web-api-21"></a>Obsługa BSON w ASP.NET Web API 2,1

według [Jan Wasson](https://github.com/MikeWasson)

W tym temacie pokazano, jak używać BSON na kontrolerze internetowego interfejsu API (po stronie serwera) i w aplikacji klienckiej platformy .NET. Internetowy interfejs API 2,1 wprowadza obsługę BSON. 

## <a name="what-is-bson"></a>Co to jest BSON?

[BSON](http://bsonspec.org/) to binarny format serializacji. "BSON" oznacza "binarny kod JSON", ale BSON i JSON są serializowane bardzo inaczej. BSON jest "notacją JSON", ponieważ obiekty są reprezentowane jako pary nazwa-wartość, podobnie jak JSON. W przeciwieństwie do formatu JSON, liczbowe typy danych są przechowywane jako bajty, a nie ciągi

BSON została zaprojektowana jako uproszczona, łatwa do skanowania i szybka do kodowania/dekodowania.

- BSON jest porównywalny w rozmiarze do formatu JSON. W zależności od danych ładunek BSON może być mniejszy lub większy od ładunku JSON. W przypadku serializowania danych binarnych, takich jak plik obrazu, BSON jest mniejszy niż format JSON, ponieważ dane binarne nie są zakodowane w formacie base64.
- BSON dokumenty są łatwe do skanowania, ponieważ elementy są poprzedzone prefiksem pola długości, dzięki czemu Analizator może pominąć elementy bez dekodowania.
- Kodowanie i dekodowanie są wydajne, ponieważ typy danych liczbowych są przechowywane jako liczby, a nie ciągi.

Natywni Klienci, na przykład aplikacje klienckie platformy .NET, mogą korzystać z BSON zamiast formatów tekstowych, takich jak JSON lub XML. W przypadku klientów korzystających z przeglądarki prawdopodobnie warto naklejić za pomocą formatu JSON, ponieważ kod JavaScript może bezpośrednio przekonwertować ładunek JSON.

Na szczęście interfejs API sieci Web używa [negocjacji zawartości](content-negotiation.md), więc interfejs API może obsługiwać oba formaty i pozwolić na wybór klienta.

## <a name="enabling-bson-on-the-server"></a>Włączanie BSON na serwerze

W konfiguracji internetowego interfejsu API Dodaj **BsonMediaTypeFormatter** do kolekcji programu formatującego.

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample1.cs)]

Teraz jeśli klient żąda aplikacji "Application/BSON", internetowy interfejs API będzie używać programu formatującego BSON.

Aby skojarzyć BSON z innymi typami nośników, Dodaj je do kolekcji SupportedMediaTypes. Poniższy kod dodaje "application/vnd. contoso" do obsługiwanych typów multimediów:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample2.cs)]

## <a name="example-http-session"></a>Przykładowa sesja HTTP

W tym przykładzie użyjemy następującej klasy modelu i prostego kontrolera interfejsu API sieci Web:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample3.cs)]

Klient może wysłać następujące żądanie HTTP:

[!code-console[Main](bson-support-in-web-api-21/samples/sample4.cmd)]

Oto odpowiedź:

[!code-console[Main](bson-support-in-web-api-21/samples/sample5.cmd)]

W tym miejscu zamieniono dane binarne na &quot;.&quot; znaków. Poniższy zrzut ekranu z programu Fiddler pokazuje pierwotne wartości szesnastkowe.

[![](bson-support-in-web-api-21/_static/image2.png)](bson-support-in-web-api-21/_static/image1.png)

## <a name="using-bson-with-httpclient"></a>Używanie BSON z HttpClient

Aplikacje klienckie platformy .NET mogą używać programu formatującego BSON z **HttpClient**. Aby uzyskać więcej informacji na temat **HttpClient**, zobacz [wywoływanie interfejsu API sieci Web z klienta platformy .NET](../advanced/calling-a-web-api-from-a-net-client.md).

Poniższy kod wysyła żądanie GET, które akceptuje BSON, a następnie deserializacji ładunku BSON w odpowiedzi.

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample6.cs)]

Aby zażądać BSON z serwera, ustaw nagłówek Accept na "Application/BSON":

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample7.cs)]

Aby zdeserializować treści odpowiedzi, użyj **BsonMediaTypeFormatter**. Ten program formatujący nie znajduje się w domyślnej kolekcji programu formatującego, więc musisz go określić podczas odczytywania treści odpowiedzi:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample8.cs)]

W następnym przykładzie pokazano, jak wysłać żądanie POST zawierające BSON.

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample9.cs)]

Większość tego kodu jest taka sama jak w poprzednim przykładzie. Ale w metodzie **PostAsync** należy określić **BsonMediaTypeFormatter** jako program formatujący:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample10.cs)]

## <a name="serializing-top-level-primitive-types"></a>Serializowanie typów pierwotnych najwyższego poziomu

Każdy dokument BSON jest listą par klucz/wartość. Specyfikacja BSON nie definiuje składni do serializacji pojedynczej nieprzetworzonej wartości, takiej jak liczba całkowita lub ciąg.

Aby obejść to ograniczenie, **BsonMediaTypeFormatter** traktuje pierwotne typy jako przypadek specjalny. Przed serializowaniem, konwertuje wartość na parę klucz/wartość z kluczem "value". Załóżmy na przykład, że kontroler interfejsu API zwraca liczbę całkowitą:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample11.cs)]

Przed serializowaniem, program formatujący BSON konwertuje ten format na następującą parę klucz/wartość:

[!code-json[Main](bson-support-in-web-api-21/samples/sample12.json)]

Podczas deserializacji, program formatujący konwertuje dane z powrotem na pierwotną wartość. Jednak klienci korzystający z innego parsera BSON będą musieli obsłużyć ten przypadek, jeśli interfejs API sieci Web zwróci nieprzetworzone wartości. Ogólnie rzecz biorąc, należy rozważyć zwrócenie danych strukturalnych, a nie nieprzetworzonych wartości.

## <a name="additional-resources"></a>Dodatkowe materiały

[Przykład BSON internetowego interfejsu API](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/BSONSample/)

[Elementy formatujące multimedia](media-formatters.md)

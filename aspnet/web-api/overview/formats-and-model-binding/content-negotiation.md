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
# <a name="content-negotiation-in-aspnet-web-api"></a>Negocjowanie zawartości w interfejsie API sieci Web ASP.NET

według [Jan Wasson](https://github.com/MikeWasson)

W tym artykule opisano sposób, w jaki interfejs API sieci Web ASP.NET implementuje negocjowanie zawartości dla ASP.NET 4. x.

Specyfikacja protokołu HTTP (RFC 2616) definiuje negocjowanie zawartości jako "proces wybierania najlepszej reprezentacji dla danej odpowiedzi w przypadku, gdy jest dostępnych wiele prezentacji". Podstawowym mechanizmem negocjacji zawartości w protokole HTTP są następujące nagłówki żądania:

- **Zaakceptuj:** Typy nośników akceptowalne dla odpowiedzi, takie jak "Application/JSON", "Application/XML" lub niestandardowy typ nośnika, taki jak &quot;application/vnd. example + XML&quot;
- **Akceptuj-charset:** Które zestawy znaków są akceptowalne, takie jak UTF-8 lub ISO 8859-1.
- **Akceptuj-Encoding:** Jakie kodowanie zawartości jest akceptowalne, na przykład gzip.
- **Akceptuj — język:** Preferowany język naturalny, taki jak "en-us".

Serwer może również przeglądać inne fragmenty żądania HTTP. Jeśli na przykład żądanie zawiera nagłówek X-Request-with wskazujący żądanie AJAX, serwer może domyślnie mieć wartość JSON, jeśli nie ma nagłówka Accept.

W tym artykule dowiesz się, jak interfejs API sieci Web używa nagłówków Accept i Accept-Charset. (W tej chwili nie ma wbudowanego wsparcia dla akceptowania kodowania lub akceptowania języka).

## <a name="serialization"></a>Serializacja

Jeśli kontroler internetowego interfejsu API zwraca zasób jako typ CLR, potok serializować wartość zwracaną i zapisuje ją w treści odpowiedzi HTTP.

Rozważmy na przykład następujące działanie kontrolera:

[!code-csharp[Main](content-negotiation/samples/sample1.cs)]

Klient może wysłać to żądanie HTTP:

[!code-console[Main](content-negotiation/samples/sample2.cmd)]

W odpowiedzi serwer może wysłać:

[!code-console[Main](content-negotiation/samples/sample3.cmd)]

W tym przykładzie klient zażądał elementu JSON, JavaScript lub "cokolwiek" (\*/\*). Serwer odpowiedział na reprezentację obiektu `Product` w formacie JSON. Zwróć uwagę, że w nagłówku Content-Type w odpowiedzi jest ustawiona wartość &quot;Application/JSON&quot;.

Kontroler może również zwracać obiekt **HttpResponseMessage** . Aby określić obiekt CLR dla treści odpowiedzi, wywołaj **metodę rozszerzającą** metody:

[!code-csharp[Main](content-negotiation/samples/sample4.cs)]

Ta opcja zapewnia większą kontrolę nad szczegółami odpowiedzi. Można ustawić kod stanu, dodać nagłówki HTTP i tak dalej.

Obiekt, który serializować zasób jest nazywany elementem *formatującego multimediów*. Elementy formatujące multimedia pochodzą z klasy **MediaTypeFormatter** . Interfejs API sieci Web udostępnia elementy formatujące multimediów dla plików XML i JSON oraz można tworzyć niestandardowe elementy formatujące do obsługi innych typów multimediów. Aby uzyskać informacje na temat pisania niestandardowego programu formatującego, zobacz elementy [formatujące multimedia](media-formatters.md).

## <a name="how-content-negotiation-works"></a>Jak działa negocjowanie zawartości

Najpierw potok pobiera usługę **IContentNegotiator** z obiektu **HttpConfiguration** . Pobiera również listę elementów formatujących multimedia z kolekcji **HttpConfiguration. formatującegos** .

Następnie potok wywołuje **IContentNegotiator. Negotiate**, przekazując:

- Typ obiektu do serializacji
- Kolekcja elementów formatujących multimedia
- Żądanie HTTP

Metoda **Negotiate** zwraca dwie części informacji:

- Którego programu formatującego użyć
- Typ nośnika dla odpowiedzi

Jeśli nie zostanie znaleziony żaden program formatujący, Metoda **Negotiate** zwraca **wartość null**, a klient odbiera błąd HTTP 406 (nieakceptowalny).

Poniższy kod pokazuje, jak kontroler może bezpośrednio wywoływać negocjowanie zawartości:

[!code-csharp[Main](content-negotiation/samples/sample5.cs)]

Ten kod jest odpowiednikiem działania potoku automatycznie.

## <a name="default-content-negotiator"></a>Domyślny negocjuje zawartość

Klasa **DefaultContentNegotiator** zapewnia domyślną implementację **IContentNegotiator**. Do wybrania programu formatującego są stosowane kilka kryteriów.

Najpierw program formatujący musi być w stanie serializować typ. Jest to weryfikowane przez wywołanie metody **MediaTypeFormatter. Unwritetype**.

Następnie negocjuje zawartość analizuje każdy program formatujący i szacuje, jak dobrze pasuje do żądania HTTP. Aby oszacować dopasowanie, negocjuje zawartość analizuje dwie rzeczy w programie formatującego:

- Kolekcja **SupportedMediaTypes** , która zawiera listę obsługiwanych typów nośników. Negocjowanie zawartości próbuje dopasować tę listę do nagłówka akceptowania żądań. Należy pamiętać, że nagłówek Accept może zawierać zakresy. Na przykład "tekst/zwykły" to dopasowanie do tekstu/\* lub \*/\*.
- Kolekcja **MediaTypeMappings** , która zawiera listę obiektów **MediaTypeMapping** . Klasa **MediaTypeMapping** zapewnia ogólny sposób dopasowywania żądań HTTP z typami nośników. Można na przykład zamapować niestandardowy nagłówek HTTP na określony typ nośnika.

Jeśli istnieje wiele dopasowań, dopasowanie do najwyższego współczynnika jakości WINS. Na przykład:

[!code-console[Main](content-negotiation/samples/sample6.cmd)]

W tym przykładzie Application/JSON ma implikowany współczynnik jakości 1,0, więc jest preferowany w przypadku aplikacji/XML.

Jeśli nie zostaną znalezione żadne dopasowania, wynegocjowanie zawartości próbuje dopasować typ nośnika treści żądania (jeśli istnieje). Na przykład, jeśli żądanie zawiera dane JSON, negocjuje zawartość będzie szukać programu formatującego w formacie JSON.

Jeśli nadal nie ma dopasowania, negocjuje zawartość po prostu wybiera pierwszy program formatujący, który może serializować typ.

## <a name="selecting-a-character-encoding"></a>Wybieranie kodowania znaków

Po wybraniu programu formatującego zawartość jest wybierana przy użyciu właściwości **SupportedEncodings** w programie formatującego i pasuje do nagłówka Accept-Charset w żądaniu (jeśli istnieje).

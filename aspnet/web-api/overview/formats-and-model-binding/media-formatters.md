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
# <a name="media-formatters-in-aspnet-web-api-2"></a>Elementy formatujące multimedia w ASP.NET Web API 2

według [Jan Wasson](https://github.com/MikeWasson)

W tym samouczku pokazano, jak obsługiwać dodatkowe formaty multimediów w interfejsie Web API ASP.NET.

## <a name="internet-media-types"></a>Typy multimediów internetowych

Typ nośnika, nazywany również typem MIME, identyfikuje format fragmentu danych. W przypadku protokołu HTTP typy nośników opisują format treści wiadomości. Typ nośnika składa się z dwóch ciągów, typu i podtypu. Na przykład:

- tekst/HTML
- image/png
- application/json

Gdy komunikat HTTP zawiera treść jednostki, nagłówek Content-Type określa format treści komunikatu. To nakazuje odbiorcy analizowanie zawartości treści wiadomości.

Na przykład, jeśli odpowiedź HTTP zawiera obraz PNG, odpowiedzi mogą mieć następujące nagłówki.

[!code-console[Main](media-formatters/samples/sample1.cmd)]

Gdy klient wysyła komunikat żądania, może zawierać nagłówek Accept. Nagłówek Accept informuje serwer, które typy nośników, których klient chce wykonać z serwera. Na przykład:

[!code-console[Main](media-formatters/samples/sample2.cmd)]

Ten nagłówek nakazuje serwerowi, że klient chce wykonać kod HTML, XHTML lub XML.

Typ nośnika określa, jak interfejs API sieci Web serializować i deserializacji Treść wiadomości HTTP. Interfejs API sieci Web ma wbudowaną obsługę danych XML, JSON, BSON i form-urlencoded, a także obsługuje dodatkowe typy nośników, pisząc program *formatujący multimedia*.

Aby utworzyć program formatujący multimedia, należy uzyskać od jednej z następujących klas:

- [MediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx). Ta klasa używa asynchronicznych metod odczytu i zapisu.
- [BufferedMediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx). Ta klasa pochodzi z **MediaTypeFormatter** , ale używa synchronicznych metod odczytu/zapisu.

Wyprowadzanie z **BufferedMediaTypeFormatter** jest prostsze, ponieważ nie istnieje kod asynchroniczny, ale oznacza to również, że wątek wywołujący może blokować podczas operacji we/wy.

## <a name="example-creating-a-csv-media-formatter"></a>Przykład: Tworzenie programu formatującego multimedia w formacie CSV

Poniższy przykład przedstawia program formatujący typ nośnika, który może serializować obiekt produktu do formatu wartości rozdzielanych przecinkami (CSV). W tym przykładzie zastosowano typ produktu zdefiniowany w samouczku [Tworzenie interfejsu API sieci Web obsługującego operacje CRUD](../older-versions/creating-a-web-api-that-supports-crud-operations.md). Oto Definicja obiektu produkt:

[!code-csharp[Main](media-formatters/samples/sample3.cs)]

Aby zaimplementować program formatujący CSV, zdefiniuj klasę, która pochodzi od **BufferedMediaTypeFormatter**:

[!code-csharp[Main](media-formatters/samples/sample4.cs)]

W konstruktorze Dodaj typy nośników obsługiwane przez program formatujący. W tym przykładzie program formatujący obsługuje pojedynczy typ nośnika, &quot;&quot;tekstu/CSV:

[!code-csharp[Main](media-formatters/samples/sample5.cs)]

Zastąp metodę **Nowritetype** , aby wskazać, które typy mogą serializować elementy formatującego:

[!code-csharp[Main](media-formatters/samples/sample6.cs)]

W tym przykładzie program formatujący może serializować pojedyncze `Product` obiekty, a także kolekcje obiektów `Product`.

Analogicznie Zastąp metodę **Unreadtype** , aby wskazać, które typy mogą deserializować program formatujący. W tym przykładzie program formatujący nie obsługuje deserializacji, dlatego metoda po prostu zwraca **wartość false**.

[!code-csharp[Main](media-formatters/samples/sample7.cs)]

Na koniec Zastąp metodę **WriteToStream** . Ta metoda serializacji typ przez zapisanie go do strumienia. Jeśli program formatujący obsługuje deserializacji, należy również zastąpić metodę **readFromStream** .

[!code-csharp[Main](media-formatters/samples/sample8.cs)]

## <a name="adding-a-media-formatter-to-the-web-api-pipeline"></a>Dodawanie programu formatującego multimedia do potoku interfejsu API sieci Web

Aby dodać program formatujący typ nośnika do potoku interfejsu API sieci Web, należy użyć właściwości program **formatującegos** w obiekcie **HttpConfiguration** .

[!code-csharp[Main](media-formatters/samples/sample9.cs)]

## <a name="character-encodings"></a>Kodowania znaków

Opcjonalnie program formatujący multimedia może obsługiwać wiele kodowań znaków, takich jak UTF-8 lub ISO 8859-1.

W konstruktorze Dodaj co najmniej jeden typ [System. Text. Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx) do kolekcji **SupportedEncodings** . Najpierw umieść kodowanie domyślne.

[!code-csharp[Main](media-formatters/samples/sample10.cs?highlight=6-7)]

W metodach **WriteToStream** i **readFromStream** Wywołaj [MediaTypeFormatter. SelectCharacterEncoding](https://msdn.microsoft.com/library/hh969054.aspx) , aby wybrać preferowane kodowanie znaków. Ta metoda dopasowuje nagłówki żądania do listy obsługiwanych kodowań. Podczas odczytu lub zapisu ze strumienia używaj zwracanego **kodowania** :

[!code-csharp[Main](media-formatters/samples/sample11.cs?highlight=3,5)]

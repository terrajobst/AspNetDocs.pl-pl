---
uid: web-api/overview/advanced/httpclient-message-handlers
title: Programy obsługi komunikatów HttpClient w ASP.NET Web API-ASP.NET 4. x
author: MikeWasson
description: Tworzenie niestandardowych programów obsługi komunikatów dla interfejsu API sieci Web ASP.NET w ASP.NET 4. x
ms.author: riande
ms.date: 10/01/2012
ms.custom: seoapril2019
ms.assetid: 5a4b6c80-b2e9-4710-8969-d5076f7f82b8
msc.legacyurl: /web-api/overview/advanced/httpclient-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: 265bd9b2f48ed7d1e955f3c4947d10fd589b3e17
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557646"
---
# <a name="httpclient-message-handlers-in-aspnet-web-api"></a>Programy obsługi komunikatów HttpClient w interfejsie API sieci Web ASP.NET

według [Jan Wasson](https://github.com/MikeWasson)

*Program obsługi komunikatów* jest klasą, która odbiera żądanie HTTP i zwraca odpowiedź HTTP.

Zwykle seria programów obsługi komunikatów jest łańcuchowo. Pierwsza procedura obsługi odbiera żądanie HTTP, wykonuje pewne przetwarzanie i przekazuje żądanie do kolejnej procedury obsługi. W pewnym momencie zostanie utworzona odpowiedź i przejdzie tworzenie kopii zapasowej łańcucha. Ten wzorzec jest nazywany programem obsługi *delegowania* .

![](httpclient-message-handlers/_static/image1.png)

Po stronie klienta Klasa **HttpClient** używa obsługi komunikatów do przetwarzania żądań. Domyślna procedura obsługi to **HttpClientHandler**, która wysyła żądanie przez sieć i pobiera odpowiedź z serwera. Do potoku klienta można wstawić niestandardowe programy obsługi komunikatów:

![](httpclient-message-handlers/_static/image2.png)

> [!NOTE]
> Interfejs API sieci Web ASP.NET również używa programów obsługi komunikatów po stronie serwera. Aby uzyskać więcej informacji, zobacz [procedury obsługi komunikatów http](http-message-handlers.md).

## <a name="custom-message-handlers"></a>Niestandardowe programy obsługi komunikatów

Aby napisać niestandardową procedurę obsługi komunikatów, pochodzi od **systemu .NET. http. DelegatingHandler** i przesłania metodę **SendAsync** . W tym miejscu znajduje się sygnatura metody:

[!code-csharp[Main](httpclient-message-handlers/samples/sample1.cs)]

Metoda przyjmuje **HttpRequestMessage** jako dane wejściowe i asynchronicznie zwraca **HttpResponseMessage**. Typowa implementacja wykonuje następujące czynności:

1. Przetwórz komunikat żądania.
2. Wywołaj `base.SendAsync`, aby wysłać żądanie do wewnętrznej procedury obsługi.
3. Procedura obsługi wewnętrznej zwraca komunikat odpowiedzi. (Ten krok jest asynchroniczny).
4. Przetwórz odpowiedź i zwróć ją do obiektu wywołującego.

Poniższy przykład pokazuje procedurę obsługi komunikatów, która dodaje niestandardowy nagłówek do żądania wychodzącego:

[!code-csharp[Main](httpclient-message-handlers/samples/sample2.cs)]

Wywołanie `base.SendAsync` jest asynchroniczne. Jeśli program obsługi wykonuje wszelką pracę po tym wywołaniu, użyj słowa kluczowego **await** , aby wznowić wykonywanie po zakończeniu metody. Poniższy przykład pokazuje procedurę obsługi, która rejestruje kody błędów. Samo rejestrowanie nie jest bardzo interesujące, ale przykład pokazuje, jak uzyskać odpowiedzi wewnątrz procedury obsługi.

[!code-csharp[Main](httpclient-message-handlers/samples/sample3.cs?highlight=10,13)]

## <a name="adding-message-handlers-to-the-client-pipeline"></a>Dodawanie programów obsługi komunikatów do potoku klienta

Aby dodać niestandardowe programy obsługi do **HttpClient**, użyj metody **HttpClientFactory. Create** :

[!code-csharp[Main](httpclient-message-handlers/samples/sample4.cs)]

Procedury obsługi komunikatów są wywoływane w kolejności, w jakiej zostały przekazane do metody **Create** . Ponieważ procedury obsługi są zagnieżdżone, komunikat odpowiedzi jest przesyłany w innym kierunku. Oznacza to, że ostatnim programem obsługi jest pierwszy, aby uzyskać komunikat odpowiedzi.

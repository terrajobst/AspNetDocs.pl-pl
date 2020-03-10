---
uid: web-api/overview/advanced/http-message-handlers
title: Procedury obsługi komunikatów HTTP w interfejsie Web API ASP.NET — ASP.NET 4. x
author: MikeWasson
description: Omówienie obsługi komunikatów HTTP w interfejsie Web API ASP.NET dla ASP.NET 4. x
ms.author: riande
ms.date: 02/13/2012
ms.custom: seoapril2019
ms.assetid: 9002018b-3aa3-4358-bb1c-fbb5bc751d01
msc.legacyurl: /web-api/overview/advanced/http-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: a8e6f1da8df4802e1acf7779a2fc75bfe8ab876f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622585"
---
# <a name="http-message-handlers-in-aspnet-web-api"></a>Procedury obsługi komunikatów HTTP w interfejsie API sieci Web ASP.NET

według [Jan Wasson](https://github.com/MikeWasson)

*Program obsługi komunikatów* jest klasą, która odbiera żądanie HTTP i zwraca odpowiedź HTTP. Procedury obsługi komunikatów pochodzą od abstrakcyjnej klasy **HttpMessageHandler** .

Zwykle seria programów obsługi komunikatów jest łańcuchowo. Pierwsza procedura obsługi odbiera żądanie HTTP, wykonuje pewne przetwarzanie i przekazuje żądanie do kolejnej procedury obsługi. W pewnym momencie zostanie utworzona odpowiedź i przejdzie tworzenie kopii zapasowej łańcucha. Ten wzorzec jest nazywany programem obsługi *delegowania* .

![](http-message-handlers/_static/image1.png)

## <a name="server-side-message-handlers"></a>Procedury obsługi komunikatów po stronie serwera

Po stronie serwera potok interfejsu API sieci Web używa wbudowanych programów obsługi komunikatów:

- **HttpServer** pobiera żądanie od hosta.
- **HttpRoutingDispatcher** wysyła żądanie na podstawie trasy.
- **HttpControllerDispatcher** wysyła żądanie do kontrolera interfejsu API sieci Web.

Możesz dodać niestandardowe programy obsługi do potoku. Procedury obsługi komunikatów są przydatne w przypadku zagadnień związanych z wycinaniem, które działają na poziomie komunikatów HTTP (a nie w ramach akcji kontrolera). Na przykład program obsługi komunikatów może:

- Odczytaj lub zmodyfikuj nagłówki żądania.
- Dodaj nagłówek odpowiedzi do odpowiedzi.
- Weryfikuj żądania przed osiągnięciem kontrolera.

Ten diagram przedstawia dwa niestandardowe programy obsługi wstawione do potoku:

![](http-message-handlers/_static/image2.png)

> [!NOTE]
> Po stronie klienta program HttpClient również używa programów obsługi komunikatów. Aby uzyskać więcej informacji, zobacz [procedury obsługi komunikatów HttpClient](httpclient-message-handlers.md).

## <a name="custom-message-handlers"></a>Niestandardowe programy obsługi komunikatów

Aby napisać niestandardową procedurę obsługi komunikatów, pochodzi od **systemu .NET. http. DelegatingHandler** i przesłania metodę **SendAsync** . Ta metoda ma następujący podpis:

[!code-csharp[Main](http-message-handlers/samples/sample1.cs)]

Metoda przyjmuje **HttpRequestMessage** jako dane wejściowe i asynchronicznie zwraca **HttpResponseMessage**. Typowa implementacja wykonuje następujące czynności:

1. Przetwórz komunikat żądania.
2. Wywołaj `base.SendAsync`, aby wysłać żądanie do wewnętrznej procedury obsługi.
3. Procedura obsługi wewnętrznej zwraca komunikat odpowiedzi. (Ten krok jest asynchroniczny).
4. Przetwórz odpowiedź i zwróć ją do obiektu wywołującego.

Oto prosty przykład:

[!code-csharp[Main](http-message-handlers/samples/sample2.cs)]

> [!NOTE]
> Wywołanie `base.SendAsync` jest asynchroniczne. Jeśli program obsługi wykonuje wszelkie działania po tym wywołaniu, użyj słowa kluczowego **await** , jak pokazano.

Procedura delegowania może również pominąć wewnętrzną procedurę obsługi i bezpośrednio utworzyć odpowiedź:

[!code-csharp[Main](http-message-handlers/samples/sample3.cs)]

Jeśli program obsługi delegowania Tworzy odpowiedź bez wywoływania `base.SendAsync`, żądanie pomija resztę potoku. Może to być przydatne w przypadku programu obsługi, który sprawdza poprawność żądania (Tworzenie odpowiedzi na błąd).

![](http-message-handlers/_static/image3.png)

## <a name="adding-a-handler-to-the-pipeline"></a>Dodawanie programu obsługi do potoku

Aby dodać program obsługi komunikatów po stronie serwera, Dodaj program obsługi do kolekcji **HttpConfiguration. MessageHandlers** . Jeśli do utworzenia projektu użyto szablonu "aplikacja sieci Web ASP.NET MVC 4", można to zrobić wewnątrz klasy **WebApiConfig** :

[!code-csharp[Main](http-message-handlers/samples/sample4.cs)]

Procedury obsługi komunikatów są wywoływane w takiej samej kolejności, w jakiej występują w kolekcji **MessageHandlers** . Ponieważ są one zagnieżdżone, komunikat odpowiedzi jest przesyłany w innym kierunku. Oznacza to, że ostatnim programem obsługi jest pierwszy, aby uzyskać komunikat odpowiedzi.

Zwróć uwagę, że nie musisz ustawiać wewnętrznych programów obsługi; Struktura internetowego interfejsu API automatycznie łączy programy obsługi komunikatów.

W przypadku samoobsługowego [udostępniania](../older-versions/self-host-a-web-api.md)Utwórz wystąpienie klasy **HttpSelfHostConfiguration** i Dodaj programy obsługi do kolekcji **MessageHandlers** .

[!code-csharp[Main](http-message-handlers/samples/sample5.cs)]

Teraz przyjrzyjmy się kilku przykładom niestandardowych programów obsługi komunikatów.

## <a name="example-x-http-method-override"></a>Przykład: X-HTTP-Method-override

Metoda X-HTTP-Method-override jest niestandardowym nagłówkiem HTTP. Jest ona przeznaczona dla klientów, którzy nie mogą wysyłać określonych typów żądań HTTP, takich jak PUT lub DELETE. Zamiast tego klient wysyła żądanie POST i ustawia do odpowiedniej metody nagłówek X-HTTP-Method-override. Na przykład:

[!code-console[Main](http-message-handlers/samples/sample6.cmd)]

Oto procedura obsługi komunikatów, która dodaje obsługę funkcji X-HTTP-Method-override:

[!code-csharp[Main](http-message-handlers/samples/sample7.cs)]

W metodzie **SendAsync** program obsługi sprawdza, czy komunikat żądania jest żądaniem post i czy zawiera nagłówek X-HTTP-Method-override. Jeśli tak, sprawdza poprawność wartości nagłówka, a następnie modyfikuje metodę żądania. Na koniec program obsługi wywołuje `base.SendAsync`, aby przekazać komunikat do kolejnej procedury obsługi.

Gdy żądanie osiągnie klasę **HttpControllerDispatcher** , **HttpControllerDispatcher** kieruje żądanie na podstawie zaktualizowanej metody żądania.

## <a name="example-adding-a-custom-response-header"></a>Przykład: Dodawanie niestandardowego nagłówka odpowiedzi

Oto procedura obsługi komunikatów, która dodaje niestandardowy nagłówek do każdego komunikatu odpowiedzi:

[!code-csharp[Main](http-message-handlers/samples/sample8.cs)]

Najpierw program obsługi wywołuje `base.SendAsync`, aby przekazać żądanie do wewnętrznego programu obsługi komunikatów. Procedura obsługi wewnętrznej zwraca komunikat odpowiedzi, ale robi to asynchronicznie przy użyciu **&lt;zadania t&gt;** obiektu. Komunikat odpowiedzi nie jest dostępny, dopóki `base.SendAsync` nie zostanie ukończona asynchronicznie.

W tym przykładzie za pomocą słowa kluczowego **await** można wykonać pracę asynchroniczną po zakończeniu `SendAsync`. Jeśli jesteś celem .NET Framework 4,0, użyj **zadania**&lt;t&gt; **. ContinueWith** :

[!code-csharp[Main](http-message-handlers/samples/sample9.cs)]

## <a name="example-checking-for-an-api-key"></a>Przykład: sprawdzanie klucza interfejsu API

Niektóre usługi sieci Web wymagają od klientów dołączenia klucza interfejsu API do żądania. Poniższy przykład pokazuje, jak program obsługi komunikatów może sprawdzić żądania dla prawidłowego klucza interfejsu API:

[!code-csharp[Main](http-message-handlers/samples/sample10.cs)]

Ta procedura obsługi wyszukuje klucz interfejsu API w ciągu zapytania identyfikatora URI. (W tym przykładzie Załóżmy, że klucz jest ciągiem statycznym. Rzeczywista implementacja prawdopodobnie użyje bardziej złożonej walidacji.) Jeśli ciąg zapytania zawiera klucz, program obsługi przekaże żądanie do wewnętrznej procedury obsługi.

Jeśli żądanie nie ma prawidłowego klucza, program obsługi tworzy komunikat odpowiedzi ze stanem 403, dostęp zabroniony. W takim przypadku program obsługi nie wywołuje `base.SendAsync`, więc wewnętrzna procedura obsługi nigdy nie otrzymuje żądania ani nie jest kontrolerem. W związku z tym kontroler może założyć, że wszystkie żądania przychodzące mają prawidłowy klucz interfejsu API.

> [!NOTE]
> Jeśli klucz interfejsu API dotyczy tylko niektórych akcji kontrolera, należy rozważyć użycie filtru akcji zamiast procedury obsługi komunikatów. Filtry akcji są uruchamiane po wykonaniu routingu URI.

## <a name="per-route-message-handlers"></a>Procedury obsługi komunikatów na trasie

Procedury obsługi w kolekcji **HttpConfiguration. MessageHandlers** mają zastosowanie globalnie.

Alternatywnie można dodać program obsługi komunikatów do określonej trasy podczas definiowania trasy:

[!code-csharp[Main](http-message-handlers/samples/sample11.cs?highlight=16)]

W tym przykładzie, jeśli identyfikator URI żądania pasuje do "Route2", żądanie jest wysyłane do `MessageHandler2`. Na poniższym diagramie przedstawiono potok dla tych dwóch tras:

![](http-message-handlers/_static/image4.png)

Zwróć uwagę, że `MessageHandler2` zastępuje domyślny **HttpControllerDispatcher**. W tym przykładzie `MessageHandler2` tworzy odpowiedź i żądania, które pasują do "Route2", nigdy nie przechodzą do kontrolera. Dzięki temu można zamienić cały mechanizm kontrolera internetowego interfejsu API na własny niestandardowy punkt końcowy.

Alternatywnie program obsługi komunikatów na trasie może delegować do **HttpControllerDispatcher**, który następnie wysyła do kontrolera.

![](http-message-handlers/_static/image5.png)

Poniższy kod przedstawia sposób konfigurowania tej trasy:

[!code-csharp[Main](http-message-handlers/samples/sample12.cs)]

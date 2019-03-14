---
uid: web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
title: Routingu we wzorcu ASP.NET Web API | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: riande
ms.date: 10/29/2018
ms.assetid: 0675bdc7-282f-4f47-b7f3-7e02133940ca
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: a7bc998fc23c0453fc9cd6ac1e7b9af7bd516225
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57077066"
---
<a name="routing-in-aspnet-web-api"></a>Routingu we wzorcu ASP.NET Web API
====================
przez [Mike Wasson](https://github.com/MikeWasson)

W tym artykule opisano, jak ASP.NET Web API kieruje żądania HTTP do kontrolerów.

> [!NOTE]
> Osoby zaznajomione z platformą ASP.NET MVC routingu internetowego interfejsu API jest bardzo podobny do routingu MVC. Główną różnicą jest to, że interfejsu API sieci Web użyto zlecenie HTTP, nie ścieżka identyfikatora URI, aby wybrać akcję. Umożliwia także MVC stylu routing w interfejsie API sieci Web. W tym artykule nie zakłada żadnej wiedzy platformy ASP.NET MVC.

## <a name="routing-tables"></a>Tabele routingu

W programie ASP.NET Web API *kontrolera* to klasa, która obsługuje żądania HTTP. Metody publiczne kontrolera są nazywane *metod akcji* lub po prostu *akcje*. Gdy struktury Web API odbiera żądanie, kieruje żądanie do operacji.

Aby ustalić, jaka akcja do wywołania, środowisko wykorzystuje *tabeli routingu*. Szablon projektu Visual Studio dla interfejsu API sieci Web tworzy trasy domyślne:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample1.cs)]

Ta trasa jest zdefiniowana w *WebApiConfig.cs* pliku, który jest umieszczony w *aplikacji\_Start* katalogu:

![](routing-in-aspnet-web-api/_static/image1.png)

Aby uzyskać więcej informacji na temat `WebApiConfig` klasy, zobacz [Konfigurowanie ASP.NET Web API](../advanced/configuring-aspnet-web-api.md).

Jeśli samodzielnego hostowania interfejsu API sieci Web, musisz ustawić w tabeli routingu bezpośrednio `HttpSelfHostConfiguration` obiektu. Aby uzyskać więcej informacji, zobacz [samodzielnego hostowania interfejsu Web API](../older-versions/self-host-a-web-api.md).

Każdy wpis w tabeli routingu zawiera *szablon trasy*. Domyślny szablon trasy dla interfejsu API sieci Web jest &quot;interfejsu api / {controller} / {id}&quot;. W tym szablonie &quot;api&quot; jest segmentu ścieżki literału, a {controller} i {id} są zmiennymi symbol zastępczy.

Gdy struktury Web API odbiera żądanie HTTP, próbuje dopasować identyfikatora URI na jednym z szablonów tras w tabeli routingu. Jeśli żadna trasa nie pasuje, klient odbiera błąd 404. Na przykład następujące identyfikatory URI zgodna trasy domyślnej:

- / api/contacts
- /API/Contacts/1
- /API/Products/gizmo1

Jednak następujący identyfikator URI nie jest zgodny, ponieważ brakuje &quot;api&quot; segmencie:

- / contacts/1

> [!NOTE]
> Przyczyna przy użyciu "interfejs api" dla trasy jest uniknąć kolizji z routing na platformie ASP.NET MVC. Dzięki temu można mieć &quot;/kontaktuje się&quot; przejdź do kontrolera MVC i &quot;/api/contacts&quot; przejdź do kontrolera internetowego interfejsu API. Oczywiście jeśli ta Konwencja nie lubisz, możesz zmienić tabela routingu domyślnego.

Po znalezieniu pasującego trasy interfejsu API sieci Web wybiera kontroler i akcję:

- Aby odnaleźć kontrolera, dodaje interfejsu API sieci Web &quot;kontrolera&quot; wartość *{controller}* zmiennej.
- Aby znaleźć akcję, internetowy interfejs API patrzy na zlecenie HTTP, a następnie szuka akcji, których nazwy zaczynają się o tej nazwie zlecenie HTTP. Na przykład żądanie GET, internetowy interfejs API wyszukuje prefiksem akcję &quot;uzyskać&quot;, takich jak &quot;GetContact&quot; lub &quot;GetAllContacts&quot;. Ta Konwencja dotyczy tylko GET, POST, PUT, DELETE, HEAD, opcje i stosowanie poprawek do zlecenia. Pozostałe zlecenia HTTP można włączyć za pomocą atrybutów kontrolera. Później zobaczymy przykład.
- Inne zmienne symbol zastępczy w szablonie trasy, takich jak *{id}* są mapowane do parametrów akcji.

Przyjrzyjmy się przykładowi. Załóżmy, że należy zdefiniować następujący kontroler:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample2.cs)]

Poniżej przedstawiono niektóre możliwe żądania HTTP wraz z akcji, która zostanie wywołana dla każdego:

| Czasownik HTTP | Ścieżka identyfikatora URI | Akcja | Parametr |
| --- | --- | --- | --- |
| GET | Interfejs API/produktów | GetAllProducts | *(Brak)* |
| GET | Interfejs API/produkty/4 | GetProductById | 4 |
| DELETE | Interfejs API/produkty/4 | DeleteProduct | 4 |
| POST | Interfejs API/produktów | *(Brak dopasowań)* |  |

Należy zauważyć, że *{id}* segmencie identyfikatora URI, jeśli jest obecny, jest mapowany na *identyfikator* parametru akcji. W tym przykładzie kontrolera definiuje dwie metody GET, jeden z *identyfikator* parametrów i bez parametrów.

Ponadto dokonany wybór żądanie POST zakończy się niepowodzeniem, ponieważ kontroler nie definiują &quot;wpis... &quot; metody.

## <a name="routing-variations"></a>Zmiany routingu

W poprzedniej sekcji opisano podstawowy mechanizm routingu ASP.NET Web API. W tej sekcji opisano pewne różnice.

### <a name="http-verbs"></a>Zlecenia HTTP

Zamiast używania konwencji nazewnictwa dla zleceń HTTP, możesz jawnie określić czasownik HTTP dla akcji przez urządzanie metodę akcji przy użyciu jednego z następujących atrybutów:

- `[HttpGet]`
- `[HttpPut]`
- `[HttpPost]`
- `[HttpDelete]`
- `[HttpHead]`
- `[HttpOptions]`
- `[HttpPatch]`

W poniższym przykładzie `FindProduct` metody jest mapowany na żądania pobierania:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample3.cs)]

Aby umożliwić wielu poleceń HTTP dla akcji lub Zezwalaj na zlecenia HTTP inne niż GET, PUT, POST, DELETE, HEAD, opcji i poprawek, użyj `[AcceptVerbs]` atrybut, który przyjmuje listę zleceń HTTP.

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample4.cs)]

<a id="routing_by_action_name"></a>
### <a name="routing-by-action-name"></a>Routing, dbając o Nazwa akcji

Szablon routingu domyślnego interfejsu API sieci Web używa zlecenie HTTP, aby wybrać akcję. Jednak można również utworzyć trasę, gdy nazwa akcji znajduje się w identyfikatorze URI:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample5.cs)]

W tym szablonie trasy *{action}* nazwy parametrów metody akcji kontrolera. Z tym stylem routingu umożliwia określanie dozwolonych poleceń HTTP atrybutów. Na przykład załóżmy, że dany kontroler ma następującą metodę:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample6.cs)]

W tym przypadku żądania GET dla "interfejsu api/produkty/szczegóły/1" mapującej do `Details` metody. Ten styl routingu jest podobny do wzorca ASP.NET MVC i może być odpowiedni dla stylu RPC interfejsu API.

Nazwa akcji można zastąpić za pomocą `[ActionName]` atrybutu. W poniższym przykładzie, istnieją dwie akcje, które mapują &quot;produkty/api/miniatur/*identyfikator*. Obsługuje jeden GET oraz drugi WPIS:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample7.cs)]

### <a name="non-actions"></a>Bez akcji

Aby zapobiec sytuacji, w której metody pobierania wywołana jako akcję, należy użyć `[NonAction]` atrybutu. To sygnalizuje Framework, metoda nie jest działanie, nawet wtedy, gdy w przeciwnym razie dopasowanie nastąpi reguły routingu.

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample8.cs)]

## <a name="further-reading"></a>Dalsze informacje

W tym temacie podano ogólny widok routingu. Aby uzyskać więcej informacji, zobacz [Routing i wybieranie akcji](routing-and-action-selection.md), która opisuje dokładnie tak jak struktura pasuje do identyfikatora URI do trasy, wybiera kontrolera i następnie wybiera akcję do wywołania.

---
uid: web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
title: Routing w interfejsie API sieci Web ASP.NET | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 10/29/2018
ms.assetid: 0675bdc7-282f-4f47-b7f3-7e02133940ca
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 85862c094cc54365267b1f21e68d235a15519cda
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557611"
---
# <a name="routing-in-aspnet-web-api"></a>Routing w składniku Web API platformy ASP.NET

według [Jan Wasson](https://github.com/MikeWasson)

W tym artykule opisano sposób, w jaki interfejs API sieci Web ASP.NET kieruje żądania HTTP do kontrolerów.

> [!NOTE]
> Jeśli znasz ASP.NET MVC, routing interfejsu API sieci Web jest bardzo podobny do routingu MVC. Główną różnicą jest to, że interfejs API sieci Web używa zlecenia HTTP, a nie ścieżki identyfikatora URI, aby wybrać akcję. W interfejsie API sieci Web można również używać routingu w stylu MVC. W tym artykule nie założono żadnej znajomości ASP.NET MVC.

## <a name="routing-tables"></a>Tabele routingu

W interfejsie API sieci Web ASP.NET *kontroler* jest klasą, która obsługuje żądania HTTP. Metody publiczne kontrolera są nazywane *metodami akcji* lub po prostu *akcjami*. Gdy struktura internetowego interfejsu API odbiera żądanie, kieruje żądanie do akcji.

Aby określić akcję do wywołania, struktura używa *tabeli routingu*. Szablon projektu programu Visual Studio dla interfejsu API sieci Web tworzy trasę domyślną:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample1.cs)]

Ta trasa jest zdefiniowana w pliku *WebApiConfig.cs* , który znajduje się w *aplikacji\_Start* Directory:

![](routing-in-aspnet-web-api/_static/image1.png)

Aby uzyskać więcej informacji na temat klasy `WebApiConfig`, zobacz [Konfigurowanie interfejsu API sieci Web ASP.NET](../advanced/configuring-aspnet-web-api.md).

W przypadku samoobsługowego interfejsu API sieci Web należy ustawić tabelę routingu bezpośrednio na obiekcie `HttpSelfHostConfiguration`. Aby uzyskać więcej informacji, zobacz samoobsługowy [interfejs API sieci Web](../older-versions/self-host-a-web-api.md).

Każdy wpis w tabeli routingu zawiera *szablon trasy*. Domyślnym szablonem trasy dla interfejsu API sieci Web jest &quot;API/{Controller}/{ID}&quot;. W tym szablonie &quot;&quot; API jest segmentem ścieżki literału, a {Controller} i {ID} są zmiennymi symbolami zastępczymi.

Gdy struktura internetowego interfejsu API odbiera żądanie HTTP, próbuje dopasować identyfikator URI do jednego z szablonów tras w tabeli routingu. W przypadku braku dopasowania trasy klient odbiera błąd 404. Na przykład następujące identyfikatory URI pasują do trasy domyślnej:

- /api/contacts
- /api/contacts/1
- /api/products/gizmo1

Jednak następujący identyfikator URI nie jest zgodny, ponieważ brakuje segmentu &quot;interfejsu API&quot;:

- /contacts/1

> [!NOTE]
> Powodem korzystania z funkcji "API" w marszrucie jest uniknięcie kolizji z routingiem ASP.NET MVC. Dzięki temu można &quot;/Contacts&quot; przejść do kontrolera MVC i &quot;/API/Contacts&quot; przejdź do kontrolera interfejsu API sieci Web. Oczywiście, jeśli nie podoba Ci się ta konwencja, możesz zmienić domyślną tabelę tras.

Po znalezieniu pasującej trasy interfejs API sieci Web wybiera kontroler i akcję:

- Aby znaleźć kontroler, interfejs API sieci Web dodaje&quot; kontrolera &quot;do wartości zmiennej *{Controller}* .
- Aby znaleźć akcję, interfejs API sieci Web przegląda czasownik HTTP, a następnie szuka akcji, której nazwa zaczyna się od tej nazwy zlecenia HTTP. Na przykład przy użyciu żądania GET interfejs API sieci Web szuka akcji poprzedzonej &quot;Get&quot;, takiej jak &quot;GetContact&quot; lub &quot;&quot;. Ta konwencja ma zastosowanie tylko do czasowników GET, POST, PUT, DELETE, szef, OPTIONS i PATCH. Inne zlecenia HTTP można włączyć przy użyciu atrybutów na kontrolerze. Zobaczymy przykład tej późniejszej.
- Inne zmienne zastępcze w szablonie trasy, takie jak *{ID},* są mapowane na parametry akcji.

Spójrzmy na przykład. Załóżmy, że definiujesz następujący kontroler:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample2.cs)]

Poniżej przedstawiono niektóre możliwe żądania HTTP wraz z akcją, która jest wywoływana dla każdego z nich:

| Czasownik HTTP | Ścieżka identyfikatora URI | Akcja | Parametr |
| --- | --- | --- | --- |
| GET | interfejsy API/produkty | GetAllProducts | *dawaj* |
| GET | interfejsy API/produkty/4 | GetProductById | 4 |
| DELETE | interfejsy API/produkty/4 | DeleteProduct | 4 |
| POST | interfejsy API/produkty | *(brak dopasowania)* |  |

Zwróć uwagę, że segment *{ID}* identyfikatora URI (jeśli istnieje) jest mapowany na parametr *ID* akcji. W tym przykładzie kontroler definiuje dwie metody GET, jeden z parametrem *ID* i jeden bez parametrów.

Należy również pamiętać, że żądanie POST zakończy się niepowodzeniem, ponieważ kontroler nie definiuje metody &quot;post...&quot;.

## <a name="routing-variations"></a>Różnice routingu

W poprzedniej sekcji opisano mechanizm routingu podstawowego dla interfejsu API sieci Web ASP.NET. W tej sekcji opisano niektóre odmiany.

### <a name="http-verbs"></a>czasowniki HTTP

Zamiast używać konwencji nazewnictwa dla czasowników HTTP, można jawnie określić czasownik HTTP dla akcji, dekorowania nazwy metodę akcji z jednym z następujących atrybutów:

- `[HttpGet]`
- `[HttpPut]`
- `[HttpPost]`
- `[HttpDelete]`
- `[HttpHead]`
- `[HttpOptions]`
- `[HttpPatch]`

W poniższym przykładzie metoda `FindProduct` jest mapowana na żądania GET:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample3.cs)]

Aby zezwolić na wiele czasowników HTTP dla akcji lub zezwolić na zlecenia HTTP inne niż GET, PUT, POST, DELETE, NAGŁÓWKowy, OPTIONS i PATCH, Użyj atrybutu `[AcceptVerbs]`, który pobiera listę zleceń HTTP.

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample4.cs)]

<a id="routing_by_action_name"></a>
### <a name="routing-by-action-name"></a>Routing według nazwy akcji

W przypadku domyślnego szablonu routingu interfejs API sieci Web używa zlecenia HTTP do wybrania akcji. Można jednak również utworzyć trasę, w której nazwa akcji jest uwzględniona w identyfikatorze URI:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample5.cs)]

W tym szablonie trasy parametr *{Action}* nazywa metodę Action na kontrolerze. Przy użyciu tego stylu routingu użyj atrybutów, aby określić dozwolone zlecenia HTTP. Załóżmy na przykład, że kontroler ma następującą metodę:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample6.cs)]

W takim przypadku żądanie GET dla "API/Products/Details" zostanie zmapowane na metodę `Details`. Ten styl routingu jest podobny do ASP.NET MVC i może być odpowiedni dla interfejsu API w stylu wywołania RPC.

Można zastąpić nazwę akcji przy użyciu atrybutu `[ActionName]`. W poniższym przykładzie istnieją dwie akcje, które mapują na &quot;API/produkty/miniaturę/*Identyfikator*. Jeden obsługuje funkcję GET, a druga obsługuje wpis:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample7.cs)]

### <a name="non-actions"></a>Akcje niedziałające

Aby zapobiec wywołaniu metody jako akcji, Użyj atrybutu `[NonAction]`. Sygnalizuje to platformę, która nie jest akcją, nawet jeśli w przeciwnym razie będzie zgodna z regułami routingu.

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample8.cs)]

## <a name="further-reading"></a>Dalsze informacje

W tym temacie przedstawiono ogólny widok routingu. Aby uzyskać więcej szczegółów, zobacz [Routing i wybór akcji](routing-and-action-selection.md), który opisuje dokładnie, w jaki sposób architektura dopasowuje identyfikator URI do trasy, wybiera kontroler, a następnie wybiera akcję do wywołania.

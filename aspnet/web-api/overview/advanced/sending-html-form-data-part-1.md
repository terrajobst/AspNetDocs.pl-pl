---
uid: web-api/overview/advanced/sending-html-form-data-part-1
title: 'Wysyłanie danych formularza HTML w interfejsie Web API ASP.NET: form-urlencoded Data-ASP.NET 4. x'
author: MikeWasson
description: W tym artykule przedstawiono sposób publikowania danych formularza urlencoded na kontrolerze interfejsu API sieci Web przy użyciu ASP.NET 4. x
ms.author: riande
ms.date: 06/15/2012
ms.custom: seoapril2019
ms.assetid: 585351c4-809a-4bf5-bcbe-35d624f565fe
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-1
msc.type: authoredcontent
ms.openlocfilehash: 7243069dbd8051b1374ed6e0112c273b8fe26f61
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557604"
---
# <a name="sending-html-form-data-in-aspnet-web-api-form-urlencoded-data"></a>Wysyłanie danych formularza HTML w interfejsie Web API ASP.NET: form-urlencoded Data

według [Jan Wasson](https://github.com/MikeWasson)

## <a name="part-1-form-urlencoded-data"></a>Część 1: formularz-urlencoded danych

W tym artykule przedstawiono sposób publikowania danych formularza urlencoded na kontrolerze interfejsu API sieci Web.

- [Przegląd formularzy HTML](#overview_of_html_forms)
- [Wysyłanie typów złożonych](#sending_complex_types)
- [Wysyłanie danych formularza za pośrednictwem technologii AJAX](#sending_form_data_via_ajax)
- [Wysyłanie typów prostych](#sending_simple_types)

> [!NOTE]
> [Pobierz ukończony projekt](https://code.msdn.microsoft.com/ASPNET-Web-API-Sending-a6f9d007).

<a id="overview_of_html_forms"></a>
## <a name="overview-of-html-forms"></a>Przegląd formularzy HTML

Formularze HTML używają elementu GET lub POST do wysyłania danych do serwera. Atrybut **Method** elementu **form** daje metodę http:

[!code-html[Main](sending-html-form-data-part-1/samples/sample1.html)]

Metoda domyślna to GET. Jeśli formularz używa GET, dane formularza są kodowane w identyfikatorze URI jako ciąg zapytania. Jeśli formularz używa wpisu POST, dane formularza są umieszczane w treści żądania. W przypadku opublikowanych danych atrybut **Enctype** określa format treści żądania:

| enctype | Opis |
| --- | --- |
| application/x-www-form-urlencoded | Dane formularza są kodowane jako pary nazwa/wartość, podobnie jak ciąg zapytania identyfikatora URI. Jest to domyślny format dla wpisu POST. |
| wieloczęściowe/formularz-dane | Dane formularza są kodowane jako wieloczęściowy komunikat MIME. Użyj tego formatu, jeśli przekazujesz plik na serwer. |

Część 1 tego artykułu wygląda w formacie x-www-form-urlencoded. [Część 2](sending-html-form-data-part-2.md) zawiera opis wieloczęściowego MIME.

<a id="sending_complex_types"></a>
## <a name="sending-complex-types"></a>Wysyłanie typów złożonych

Zwykle wysyłany jest typ złożony składający się z wartości pobranych z kilku kontrolek formularza. Rozważmy następujący model, który reprezentuje aktualizację stanu:

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample2.cs)]

Oto kontroler interfejsu API sieci Web, który akceptuje obiekt `Update` za pośrednictwem wpisu POST.

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample3.cs)]

> [!NOTE]
> Ten kontroler korzysta z [routingu opartego na akcjach](../web-api-routing-and-actions/routing-in-aspnet-web-api.md#routing_by_action_name), więc szablon trasy to &quot;API/{Controller}/{Action}/{id}&quot;. Klient będzie ogłaszał dane w &quot;/API/Updates/Complex&quot;.

Teraz Napiszmy formularz HTML dla użytkowników w celu przesłania aktualizacji stanu.

[!code-html[Main](sending-html-form-data-part-1/samples/sample4.html)]

Zwróć uwagę, że atrybut **Action** w formularzu jest identyfikatorem URI naszej akcji kontrolera. Oto formularz z wprowadzonymi wartościami:

![](sending-html-form-data-part-1/_static/image1.png)

Gdy użytkownik kliknie przycisk Prześlij, przeglądarka wyśle żądanie HTTP podobne do następujących:

[!code-console[Main](sending-html-form-data-part-1/samples/sample5.cmd)]

Należy zauważyć, że treść żądania zawiera dane formularza sformatowane jako pary nazwa/wartość. Interfejs API sieci Web automatycznie konwertuje pary nazwa/wartość na wystąpienie klasy `Update`.

<a id="sending_form_data_via_ajax"></a>
## <a name="sending-form-data-via-ajax"></a>Wysyłanie danych formularza za pośrednictwem technologii AJAX

Gdy użytkownik przesyła formularz, przeglądarka nawiguje poza bieżącą stronę i renderuje treść komunikatu odpowiedzi. Jest to prawidłowe, gdy odpowiedź jest stroną HTML. W przypadku interfejsu API sieci Web treść odpowiedzi jest zwykle pusta lub zawiera dane strukturalne, takie jak JSON. W takim przypadku lepiej jest wysyłać dane formularza przy użyciu żądania AJAX, aby strona mogła przetworzyć odpowiedź.

Poniższy kod przedstawia sposób publikowania danych formularza przy użyciu jQuery.

[!code-html[Main](sending-html-form-data-part-1/samples/sample6.html)]

Funkcja **przesyłania** jQuery zastępuje akcję formularza nową funkcją. Spowoduje to zastąpienie domyślnego zachowania przycisku Prześlij. Funkcja **serializacji** serializować dane formularza do par nazwa/wartość. Aby wysłać dane formularza do serwera, wywołaj `$.post()`.

Po zakończeniu żądania `.success()` lub procedura obsługi `.error()` wyświetla odpowiedni komunikat dla użytkownika.

![](sending-html-form-data-part-1/_static/image2.png)

<a id="sending_simple_types"></a>
## <a name="sending-simple-types"></a>Wysyłanie typów prostych

W poprzednich sekcjach wysłaliśmy typ złożony, który jest deserializowany przez internetowy interfejs API do wystąpienia klasy modelu. Możesz również wysyłać typy proste, takie jak ciąg.

> [!NOTE]
> Przed wysłaniem typu prostego Rozważ zamiast tego zapakowanie wartości w typie złożonym. Dzięki temu można sprawdzić poprawność modelu po stronie serwera i ułatwić rozbudowanie modelu w razie potrzeby.

Podstawowe kroki umożliwiające wysłanie typu prostego są takie same, ale istnieją dwie delikatne różnice. Najpierw w kontrolerze należy dekorować nazwę parametru z atrybutem **FromBody** .

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample7.cs?highlight=3)]

Domyślnie interfejs API sieci Web próbuje uzyskać proste typy z identyfikatora URI żądania. Atrybut **FromBody** informuje interfejs API sieci Web o odczytaniu wartości z treści żądania.

> [!NOTE]
> Interfejs API sieci Web odczytuje treść odpowiedzi najwyżej raz, więc tylko jeden parametr akcji może pochodzić z treści żądania. Jeśli musisz uzyskać wiele wartości z treści żądania, zdefiniuj typ złożony.

Po drugie klient musi wysłać wartość w następującym formacie:

[!code-xml[Main](sending-html-form-data-part-1/samples/sample8.xml)]

W odniesieniu do typu prostego część nazwy pary nazwa/wartość musi być pusta. Nie wszystkie przeglądarki obsługują tę procedurę dla formularzy HTML, ale ten format można utworzyć w skrypcie w następujący sposób:

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample9.js)]

Oto przykładowa postać:

[!code-html[Main](sending-html-form-data-part-1/samples/sample10.html)]

A Oto skrypt do przesyłania wartości formularza. Jedyną różnicą od poprzedniego skryptu jest argument przekazaną do funkcji **post** .

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample11.js?highlight=2)]

Możesz użyć tego samego podejścia, aby wysłać tablicę typów prostych:

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample12.js)]

## <a name="additional-resources"></a>Dodatkowe materiały

[Część 2: przekazywanie plików i wieloczęściowe MIME](sending-html-form-data-part-2.md)

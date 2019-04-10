---
uid: web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
title: Model sprawdzania poprawności we wzorcu ASP.NET Web API — ASP.NET 4.x
author: MikeWasson
description: Omówienie sprawdzania poprawności modelu programu ASP.NET Web API dla platformy ASP.NET 4.x.
ms.author: riande
ms.date: 07/20/2012
ms.custom: seoapril2019
ms.assetid: 7d061207-22b8-4883-bafa-e89b1e7749ca
msc.legacyurl: /web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: d4e792f8cc2f79c2ab82c5a74fd50f49475fac4f
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/09/2019
ms.locfileid: "59404575"
---
# <a name="model-validation-in-aspnet-web-api"></a>Weryfikacja modelu we wzorcu ASP.NET Web API

przez [Mike Wasson](https://github.com/MikeWasson)

W tym artykule pokazano, jak dodawać adnotacje do modeli, korzystanie z adnotacji do sprawdzania poprawności danych i obsługi błędów sprawdzania poprawności w interfejsie API sieci web. Gdy klient wysyła dane do interfejsu API sieci web, często chcesz sprawdzić poprawność danych przed wykonaniem jakiegokolwiek przetwarzania. 

## <a name="data-annotations"></a>Adnotacje danych

ASP.NET Web API, można użyć atrybuty z [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) przestrzeni nazw, aby ustawić reguły sprawdzania poprawności dla właściwości modelu. Należy wziąć pod uwagę następujący wzór:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample1.cs)]

Sprawdzanie poprawności modelu użycie we wzorcu ASP.NET MVC to powinna wyglądać znajomo. **Wymagane** atrybutu jest wyświetlany komunikat, który `Name` właściwość nie może mieć wartości null. **Zakres** atrybutu jest wyświetlany komunikat, który `Weight` musi należeć do zakresu od 0 do 999.

Załóżmy, że klient wysyła żądanie POST z następującą reprezentację JSON:

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample2.json)]

Widać, że klient nie zawiera `Name` właściwość, która jest oznaczona jako wymagane. Kiedy internetowy interfejs API konwertuje dane JSON do `Product` wystąpienia, weryfikuje `Product` względem atrybutów sprawdzania poprawności. W akcji kontrolera można sprawdzić, czy model jest prawidłowy:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample3.cs)]

Sprawdzanie poprawności modelu nie gwarantuje, że dane klienta są bezpieczne. Mogą być wymagane dodatkowe sprawdzenie poprawności w innych warstwach aplikacji. (Na przykład warstwa danych może wymuszać ograniczenia klucza obcego). Samouczek [przy użyciu interfejsu API sieci Web za pomocą platformy Entity Framework](../data/using-web-api-with-entity-framework/part-1.md) analizuje niektóre z tych problemów.

**"Niepełną ogłaszając"**: Publikowanie niepełną występuje, gdy klient powoduje, że niektóre właściwości. Na przykład załóżmy, że klient wysyła następujące czynności:

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample4.json)]

W tym miejscu klienta nie określono wartości dla `Price` lub `Weight`. Element formatujący JSON przypisuje wartość domyślną równą zero, aby brakuje właściwości.

![](model-validation-in-aspnet-web-api/_static/image1.png)

Stan modelu jest prawidłowa, ponieważ zero jest prawidłową wartością dla tych właściwości. Czy jest to problem, zależy od danego scenariusza. Na przykład w operacji aktualizacji, możesz chcieć rozróżnienia między "zero" i "nenastaveno." Wymuś na klientach, aby ustawić wartość, ustawić właściwość jako dopuszczającego wartość null i ustaw **wymagane** atrybutu:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample5.cs?highlight=1-2)]

**"Polegającymi"**: Klient może także wysłać *więcej* danych niż oczekiwano. Na przykład:

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample6.json)]

W tym miejscu za pomocą pliku JSON zawiera właściwość ("Color"), która nie istnieje w `Product` modelu. W takim przypadku program formatujący JSON po prostu ignoruje tę wartość. (Element formatujący XML działa tak samo). Publikowanie nadmiernego powoduje występowanie problemów, jeśli model ma właściwości, które mają być tylko do odczytu. Na przykład:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample7.cs)]

Nie chcesz, aby użytkownikom aktualizowanie `IsAdmin` właściwości i podnieść samodzielnie administratorom! Najbezpieczniejszym strategii jest użycie klasy modelu, który dokładnie pasuje, klient może wysyłać:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample8.cs)]

> [!NOTE]
> Wpis w blogu Brad Wilson "[vs sprawdzania poprawności danych wejściowych. Walidacja we wzorcu ASP.NET MVC modelu](http://bradwilson.typepad.com/blog/2010/01/input-validation-vs-model-validation-in-aspnet-mvc.html)"jest dobre omówienie niepełną ogłaszania i nadmiernego ogłaszania. Mimo że ogłoszenie dotyczące platformy ASP.NET MVC 2 problemy dotyczą nadal interfejsu API sieci Web.


## <a name="handling-validation-errors"></a>Obsługa błędów sprawdzania poprawności

Interfejs API sieci Web nie automatycznie zwraca błąd do klienta podczas sprawdzania poprawności zakończy się niepowodzeniem. Jest akcji kontrolera, aby sprawdzić stan modelu i odpowiednio reagować.

Można również utworzyć filtr akcji, aby sprawdzić stan modelu przed wywołaniem akcji kontrolera. Poniżej znajduje się kod przykładowy:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample9.cs)]

Jeśli sprawdzanie poprawności modelu nie powiedzie się, filtr zwróci odpowiedź HTTP, który zawiera błędy weryfikacji. W takim przypadku nie jest wywoływany akcji kontrolera.

[!code-console[Main](model-validation-in-aspnet-web-api/samples/sample10.cmd)]

Aby zastosować filtr do wszystkich kontrolerów składnika Web API, należy dodać wystąpienia filtru w celu **HttpConfiguration.Filters** kolekcji podczas konfiguracji:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample11.cs)]

Innym rozwiązaniem jest, aby ustawić filtr jako atrybut na poszczególnych kontrolerach lub akcji kontrolera:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample12.cs)]

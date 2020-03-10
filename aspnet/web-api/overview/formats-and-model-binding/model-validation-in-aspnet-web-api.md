---
uid: web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
title: Walidacja modelu w ASP.NET Web API-ASP.NET 4. x
author: MikeWasson
description: Przegląd weryfikacji modelu w interfejsie Web API ASP.NET dla ASP.NET 4. x.
ms.author: riande
ms.date: 07/20/2012
ms.custom: seoapril2019
ms.assetid: 7d061207-22b8-4883-bafa-e89b1e7749ca
msc.legacyurl: /web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 531a66b7ab642bd012663517640f2766f1917f25
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557240"
---
# <a name="model-validation-in-aspnet-web-api"></a>Walidacja modelu w interfejsie API sieci Web ASP.NET

według [Jan Wasson](https://github.com/MikeWasson)

W tym artykule przedstawiono sposób dodawania adnotacji do modeli, używania adnotacji do sprawdzania poprawności danych i obsługi błędów walidacji w interfejsie API sieci Web. Gdy klient wysyła dane do internetowego interfejsu API, często chcesz sprawdzić poprawność danych przed wykonaniem jakiegokolwiek przetwarzania. 

## <a name="data-annotations"></a>Adnotacje danych

W ASP.NET Web API można używać atrybutów z przestrzeni nazw [System. ComponentModel. DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) do ustawiania reguł walidacji dla właściwości w modelu. Rozważmy następujący model:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample1.cs)]

Jeśli używasz walidacji modelu w ASP.NET MVC, powinno to wyglądać znajomo. **Wymagany** atrybut wskazuje, że właściwość `Name` nie może mieć wartości null. Atrybut **zakresu** wskazuje, że `Weight` musi należeć do zakresu od zero do 999.

Załóżmy, że klient wysyła żądanie POST z następującą reprezentacją JSON:

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample2.json)]

Można zobaczyć, że klient nie zawiera właściwości `Name`, która jest oznaczona jako wymagana. Gdy interfejs API sieci Web konwertuje kod JSON na wystąpienie `Product`, sprawdza poprawność `Product` względem atrybutów walidacji. W akcji kontrolera możesz sprawdzić, czy model jest prawidłowy:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample3.cs)]

Walidacja modelu nie gwarantuje, że dane klienta są bezpieczne. W innych warstwach aplikacji może być wymagana dodatkowa weryfikacja. (Na przykład warstwa danych może wymusić ograniczenia klucza obcego). Samouczek [korzystający z interfejsu API sieci Web i Entity Framework](../data/using-web-api-with-entity-framework/part-1.md) Eksplorowanie niektórych z tych problemów.

**"W obszarze Księgowanie"** : w obszarze Księgowanie występuje, gdy klient opuści pewne właściwości. Załóżmy na przykład, że klient wysyła następujące elementy:

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample4.json)]

W tym miejscu klient nie określił wartości dla `Price` lub `Weight`. Program formatujący JSON przypisuje wartość domyślną zero do brakujących właściwości.

![](model-validation-in-aspnet-web-api/_static/image1.png)

Stan modelu jest prawidłowy, ponieważ zero jest prawidłową wartością dla tych właściwości. To, czy jest to problem, zależy od danego scenariusza. Na przykład w operacji aktualizacji można chcieć rozróżnić wartości "zero" i "nie ustawiono". Aby wymusić ustawienie wartości przez klientów, należy wprowadzić wartość null i ustawić **wymagany** atrybut:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample5.cs?highlight=1-2)]

**"Za pośrednictwem publikowania"** : Klient może również wysyłać *więcej* danych niż oczekiwano. Na przykład:

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample6.json)]

W tym miejscu plik JSON zawiera właściwość ("Color"), która nie istnieje w modelu `Product`. W takim przypadku program formatujący JSON po prostu ignoruje tę wartość. (Program formatujący XML robi to tak samo). Nadmierne Księgowanie powoduje problemy, jeśli model ma właściwości, które są przeznaczone tylko do odczytu. Na przykład:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample7.cs)]

Nie chcesz, aby użytkownicy mogli aktualizować Właściwość `IsAdmin` i Podnieś poziom uprawnień do administratorów. Najbezpieczniejsza strategia polega na użyciu klasy modelu, która dokładnie pasuje do tego, co klient może wysłać:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample8.cs)]

> [!NOTE]
> Wpis w blogu z programu Brada Wilson "[weryfikacja danych wejściowych a Walidacja modelu w ASP.NET MVC](http://bradwilson.typepad.com/blog/2010/01/input-validation-vs-model-validation-in-aspnet-mvc.html)" ma dobrą dyskusję na temat księgowania i nadmiernego księgowania. Mimo że wpis dotyczy ASP.NET MVC 2, problemy nadal dotyczą interfejsu API sieci Web.

## <a name="handling-validation-errors"></a>Obsługa błędów walidacji

Gdy Walidacja nie powiedzie się, internetowy interfejs API nie zwróci automatycznie błędu do klienta. Jest to akcja kontrolera, aby sprawdzić stan modelu i odpowiednio odpowiedzieć.

Możesz również utworzyć filtr akcji, aby sprawdzić stan modelu przed wywołaniem akcji kontrolera. Poniżej znajduje się kod przykładowy:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample9.cs)]

Jeśli walidacja modelu zakończy się niepowodzeniem, ten filtr zwróci odpowiedź HTTP zawierającą błędy walidacji. W takim przypadku akcja kontrolera nie zostanie wywołana.

[!code-console[Main](model-validation-in-aspnet-web-api/samples/sample10.cmd)]

Aby zastosować ten filtr do wszystkich kontrolerów internetowego interfejsu API, Dodaj wystąpienie filtru do kolekcji **HttpConfiguration. filters** podczas konfiguracji:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample11.cs)]

Innym rozwiązaniem jest ustawienie filtru jako atrybutu na poszczególnych kontrolerach lub akcjach kontrolera:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample12.cs)]

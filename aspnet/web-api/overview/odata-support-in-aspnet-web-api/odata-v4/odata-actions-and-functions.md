---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
title: Akcje i funkcje w protokole OData v4 przy użyciu ASP.NET Web API 2,2 | Microsoft Docs
author: MikeWasson
description: W przypadku protokołu OData akcje i funkcje są sposobem dodawania zachowań po stronie serwera, które nie są łatwo zdefiniowane jako operacje CRUD na jednostkach. W tym samouczku pokazano, jak to zrobić...
ms.author: riande
ms.date: 06/27/2014
ms.assetid: 0e6fb03c-b16d-4bb0-ab0b-552bd2b6ece1
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
msc.type: authoredcontent
ms.openlocfilehash: f5af94e93e5b7f2351d40febbf1a468d635c9db1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556225"
---
# <a name="actions-and-functions-in-odata-v4-using-aspnet-web-api-22"></a>Akcje i funkcje w protokole OData v4 przy użyciu ASP.NET Web API 2,2

według [Jan Wasson](https://github.com/MikeWasson)

> W przypadku protokołu OData akcje i funkcje są sposobem dodawania zachowań po stronie serwera, które nie są łatwo zdefiniowane jako operacje CRUD na jednostkach. W tym samouczku pokazano, jak dodać akcje i funkcje do punktu końcowego protokołu OData v4 przy użyciu interfejsu Web API 2,2. Samouczek kompiluje się w samouczku [Tworzenie punktu końcowego OData v4 przy użyciu ASP.NET Web API 2](create-an-odata-v4-endpoint.md)
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używane w samouczku
>
> - Internetowy interfejs API 2,2
> - OData 4
> - Visual Studio 2013 (Pobierz program Visual Studio 2017 [tutaj](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))
> - .NET 4.5
>
> ## <a name="tutorial-versions"></a>Wersje samouczka
>
> W przypadku usługi OData w wersji 3 zobacz [Akcje OData w ASP.NET Web API 2](../odata-v3/odata-actions.md).

Różnica między *akcjami* i *funkcjami* polega na tym, że akcje mogą mieć efekty uboczne, a funkcje nie. Obie akcje i funkcje mogą zwracać dane. Niektóre zastosowania do akcji obejmują:

- Złożone transakcje.
- Jednoczesne manipulowanie kilkoma jednostkami.
- Zezwalanie na aktualizacje tylko do określonych właściwości jednostki.
- Wysyłanie danych, które nie są jednostką.

Funkcje są przydatne do zwracania informacji, które nie są bezpośrednio powiązane z jednostką lub kolekcją.

Akcja (lub funkcja) może być celem pojedynczej jednostki lub kolekcji. W terminologii OData jest to *powiązanie*. Możesz również mieć &quot;niepowiązane&quot; akcje/funkcje, które są wywoływane jako operacje statyczne w usłudze.

## <a name="example-adding-an-action"></a>Przykład: Dodawanie akcji

Zdefiniujmy akcję do oceniania produktu.

> [!NOTE]
> Ten samouczek kompiluje się w samouczku [Tworzenie punktu końcowego OData v4 przy użyciu interfejsu ASP.NET Web API 2](create-an-odata-v4-endpoint.md)

Najpierw Dodaj model `ProductRating`, aby reprezentować klasyfikacje.

[!code-csharp[Main](odata-actions-and-functions/samples/sample1.cs)]

Dodaj także **nieogólnymi** do klasy `ProductsContext`, tak aby EF utworzy tabelę klasyfikacji w bazie danych.

[!code-csharp[Main](odata-actions-and-functions/samples/sample2.cs)]

### <a name="add-the-action-to-the-edm"></a>Dodawanie akcji do modelu EDM

W WebApiConfig.cs Dodaj następujący kod:

[!code-csharp[Main](odata-actions-and-functions/samples/sample3.cs)]

Metoda **EntityTypeConfiguration. Action** dodaje akcję do modelu Entity Data Model (EDM). Metoda **parametru** określa typ parametru dla akcji.

Ten kod ustawia również przestrzeń nazw dla modelu EDM. Przestrzeń nazw ma znaczenie, ponieważ identyfikator URI akcji obejmuje w pełni kwalifikowaną nazwę akcji:

[!code-console[Main](odata-actions-and-functions/samples/sample4.cmd)]

> [!NOTE]
> W typowej konfiguracji usług IIS, kropka w tym adresie URL spowoduje zwrócenie błędu 404. Aby rozwiązać ten problem, Dodaj następującą sekcję do pliku Web. config:

[!code-xml[Main](odata-actions-and-functions/samples/sample5.xml)]

### <a name="add-a-controller-method-for-the-action"></a>Dodaj metodę kontrolera dla akcji

Aby włączyć akcję &quot;rate&quot;, Dodaj następującą metodę do `ProductsController`:

[!code-csharp[Main](odata-actions-and-functions/samples/sample6.cs)]

Zwróć uwagę, że nazwa metody jest zgodna z nazwą akcji. Atrybut **[HTTPPOST]** określa, że metoda jest metodą POST protokołu HTTP.

Aby wywołać akcję, klient wysyła żądanie HTTP POST w następujący sposób:

[!code-console[Main](odata-actions-and-functions/samples/sample7.cmd)]

Akcja &quot;rate&quot; jest powiązana z wystąpieniami produktu, więc identyfikator URI dla akcji to w pełni kwalifikowana nazwa działania dołączona do identyfikatora URI jednostki. (Odwołaj, że ustawimy przestrzeń nazw EDM na &quot;ProductService&quot;, więc w pełni kwalifikowana nazwa akcji to &quot;ProductService. rate&quot;).

Treść żądania zawiera parametry akcji jako ładunek JSON. Interfejs API sieci Web automatycznie konwertuje ładunek JSON na obiekt **ODataActionParameters** , który jest tylko słownikiem wartości parametrów. Użyj tego słownika, aby uzyskać dostęp do parametrów w metodzie kontrolera.

Jeśli klient wysyła parametry akcji w niewłaściwym formacie, wartość **ModelState. IsValid** jest równa false. Zaznacz tę flagę w metodzie kontrolera i zwróć błąd, jeśli **IsValid** ma wartość false.

[!code-csharp[Main](odata-actions-and-functions/samples/sample8.cs)]

## <a name="example-adding-a-function"></a>Przykład: Dodawanie funkcji

Teraz Dodajmy funkcję OData, która zwraca najbardziej kosztowny produkt. Jak poprzednio, pierwszym krokiem jest dodanie funkcji do modelu EDM. W WebApiConfig.cs Dodaj następujący kod.

[!code-csharp[Main](odata-actions-and-functions/samples/sample9.cs)]

W takim przypadku funkcja jest powiązana z kolekcją produkty, a nie z wystąpieniami poszczególnych produktów. Klienci wywołują funkcję, wysyłając żądanie GET:

[!code-console[Main](odata-actions-and-functions/samples/sample10.cmd)]

Oto Metoda kontrolera dla tej funkcji:

[!code-csharp[Main](odata-actions-and-functions/samples/sample11.cs)]

Zwróć uwagę, że nazwa metody jest zgodna z nazwą funkcji. Atrybut **[narzędzia HttpGet]** określa, że metoda jest metodą HTTP GET.

Oto odpowiedź HTTP:

[!code-console[Main](odata-actions-and-functions/samples/sample12.cmd)]

## <a name="example-adding-an-unbound-function"></a>Przykład: Dodawanie funkcji niepowiązanej

Poprzedni przykład to funkcja powiązana z kolekcją. W tym następnym przykładzie utworzymy *niepowiązaną* funkcję. Niepowiązane funkcje są wywoływane jako operacje statyczne w usłudze. Funkcja w tym przykładzie zwróci podatek sprzedaży dla danego kodu pocztowego.

W pliku WebApiConfig Dodaj funkcję do modelu EDM:

[!code-csharp[Main](odata-actions-and-functions/samples/sample13.cs)]

Zwróć uwagę, że wywołujemy **funkcję** bezpośrednio w **ODataModelBuilder**, a nie na typie lub kolekcji jednostek. Oznacza to, że funkcja jest niezwiązana z konstruktorem modelu.

Oto Metoda kontrolera implementująca funkcję:

[!code-csharp[Main](odata-actions-and-functions/samples/sample14.cs)]

Nie ma znaczenia kontrolera interfejsu API sieci Web, w którym jest umieszczana ta metoda. Można ją umieścić w `ProductsController`lub zdefiniować oddzielny kontroler. Atrybut **[ODataRoute]** definiuje szablon identyfikatora URI dla funkcji.

Oto przykładowe żądanie klienta:

[!code-console[Main](odata-actions-and-functions/samples/sample15.cmd)]

Odpowiedź HTTP:

[!code-console[Main](odata-actions-and-functions/samples/sample16.cmd)]

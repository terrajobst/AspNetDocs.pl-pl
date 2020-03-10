---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
title: Relacje jednostek w protokole OData v4 przy użyciu ASP.NET Web API 2,2 | Microsoft Docs
author: MikeWasson
description: 'Większość zestawów danych definiuje relacje między jednostkami: klienci mają zamówienia; książki mają autorów; produkty mają dostawców. Korzystając z protokołu OData, klienci mogą przechodzić...'
ms.author: riande
ms.date: 06/26/2014
ms.assetid: 72657550-ec09-4779-9bfc-2fb15ecd51c7
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: fbafb2b2346689271905db5790cdddeeb809b070
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598694"
---
# <a name="entity-relations-in-odata-v4-using-aspnet-web-api-22"></a>Relacje jednostek w protokole OData v4 przy użyciu ASP.NET Web API 2,2

według [Jan Wasson](https://github.com/MikeWasson)

> Większość zestawów danych definiuje relacje między jednostkami: klienci mają zamówienia; książki mają autorów; produkty mają dostawców. Korzystając z protokołu OData, klienci mogą nawigować po relacjach jednostek. W danym produkcie można znaleźć dostawcę. Można również tworzyć i usuwać relacje. Na przykład można ustawić dostawcę dla produktu.
>
> W tym samouczku pokazano, jak obsługiwać te operacje w protokole OData v4 przy użyciu interfejsu API sieci Web ASP.NET. Samouczek jest oparty na samouczku [Tworzenie punktu końcowego OData v4 przy użyciu ASP.NET Web API 2](create-an-odata-v4-endpoint.md).
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używane w samouczku
>
> - Internetowy interfejs API 2,1
> - OData 4
> - Visual Studio 2013 (Pobierz program Visual Studio 2017 [tutaj](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))
> - Entity Framework 6
> - .NET 4.5
>
> ## <a name="tutorial-versions"></a>Wersje samouczka
>
> W przypadku protokołu OData w wersji 3 zobacz temat [Obsługa relacji jednostek w protokole OData V3](https://asp.net/web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations).

## <a name="add-a-supplier-entity"></a>Dodawanie jednostki dostawcy

> [!NOTE]
> Samouczek jest oparty na samouczku [Tworzenie punktu końcowego OData v4 przy użyciu ASP.NET Web API 2](create-an-odata-v4-endpoint.md).

Najpierw potrzebujemy powiązanej jednostki. Dodaj klasę o nazwie `Supplier` w folderze modele.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample1.cs)]

Dodaj właściwość nawigacji do klasy `Product`:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample2.cs?highlight=13-15)]

Dodaj nowy **nieogólnymi** do klasy `ProductsContext`, tak aby Entity Framework obejmował tabelę dostawców w bazie danych.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample3.cs?highlight=10)]

W programie WebApiConfig.cs Dodaj &quot;dostawców&quot; do modelu danych jednostki:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample4.cs?highlight=6)]

## <a name="add-a-suppliers-controller"></a>Dodawanie kontrolera dostawcy

Dodaj klasę `SuppliersController` do folderu controllers.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample5.cs)]

Nie pokazuje, jak dodać operacje CRUD dla tego kontrolera. Kroki są takie same jak dla kontrolera produktów (zobacz [Tworzenie punktu końcowego OData v4](create-an-odata-v4-endpoint.md)).

## <a name="getting-related-entities"></a>Pobieranie powiązanych jednostek

Aby uzyskać dostawcę produktu, klient wysyła żądanie GET:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample6.cmd)]

Aby obsłużyć to żądanie, Dodaj następującą metodę do klasy `ProductsController`:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample7.cs)]

Ta metoda używa domyślnej konwencji nazewnictwa

- Nazwa metody: GetX, gdzie X jest właściwością nawigacji.
- Nazwa parametru: *klucz*

Jeśli przestrzegasz tej konwencji nazewnictwa, interfejs API sieci Web automatycznie mapuje żądanie HTTP do metody kontrolera.

Przykładowe żądanie HTTP:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample8.cmd)]

Przykładowa odpowiedź HTTP:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample9.cmd)]

### <a name="getting-a-related-collection"></a>Pobieranie powiązanej kolekcji

W poprzednim przykładzie produkt ma jednego dostawcę. Właściwość nawigacji może również zwracać kolekcję. Poniższy kod pobiera produkty dla dostawcy:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample10.cs)]

W tym przypadku metoda zwraca interfejs **IQueryable** zamiast **SingleResult&lt;t&gt;**

Przykładowe żądanie HTTP:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample11.cmd)]

Przykładowa odpowiedź HTTP:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample12.cmd)]

## <a name="creating-a-relationship-between-entities"></a>Tworzenie relacji między jednostkami

Usługa OData obsługuje tworzenie lub usuwanie relacji między dwoma istniejącymi jednostkami. W terminologii OData w wersji v4 relacja jest &quot;&quot;odwołania. (W protokole OData V3 relacja została wywołana jako *link*. Różnice między protokołami nie mają znaczenia dla tego samouczka.)

Odwołanie ma własny identyfikator URI z formularzem `/Entity/NavigationProperty/$ref`. Na przykład poniżej znajduje się identyfikator URI służący do adresowania odwołania między produktem a jego dostawcą:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample13.cmd)]

Aby dodać relację, klient wysyła żądanie POST lub PUT na ten adres.

- Umieść, jeśli właściwość nawigacji jest pojedynczą jednostką, taką jak `Product.Supplier`.
- Opublikuj, jeśli właściwość nawigacji jest kolekcją, taką jak `Supplier.Products`.

Treść żądania zawiera identyfikator URI innej jednostki w relacji. Oto przykładowe żądanie:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample14.cmd)]

W tym przykładzie klient wysyła żądanie PUT do `/Products(6)/Supplier/$ref`, czyli $ref URI dla `Supplier` produktu o IDENTYFIKATORze 6. Jeśli żądanie zakończy się powodzeniem, serwer wysyła odpowiedź 204 (brak zawartości):

[!code-console[Main](entity-relations-in-odata-v4/samples/sample15.cmd)]

Poniżej przedstawiono metodę kontrolera umożliwiającą dodanie relacji do `Product`:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample16.cs)]

Parametr *navigationProperty* określa relację do ustawienia. (Jeśli istnieje więcej niż jedna właściwość nawigacji w jednostce, można dodać więcej instrukcji `case`.)

Parametr *linku* zawiera identyfikator URI dostawcy. Interfejs API sieci Web automatycznie analizuje treść żądania, aby uzyskać wartość dla tego parametru.

Aby wyszukać dostawcę, potrzebujemy identyfikatora (lub klucza), który jest częścią parametru *linku* . W tym celu należy użyć następującej metody pomocnika:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample17.cs)]

Zasadniczo ta metoda używa biblioteki OData do dzielenia ścieżki URI na segmenty, wyszukiwanie segmentu, który zawiera klucz, i konwertowanie klucza na poprawny typ.

## <a name="deleting-a-relationship-between-entities"></a>Usuwanie relacji między jednostkami

Aby usunąć relację, klient wysyła żądanie HTTP DELETE do $ref URI:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample18.cmd)]

Oto Metoda kontrolera, aby usunąć relację między produktem a dostawcą:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample19.cs)]

W tym przypadku `Product.Supplier` jest &quot;1&quot; końcu relacji 1-do-wielu, więc można usunąć relację tylko przez ustawienie `Product.Supplier` na `null`.

Na &quot;wielu&quot; końcu relacji klient musi określić powiązaną jednostkę do usunięcia. W tym celu klient wysyła identyfikator URI powiązanej jednostki w ciągu zapytania żądania. Na przykład, aby usunąć "produkt 1" z "dostawca 1":

[!code-console[Main](entity-relations-in-odata-v4/samples/sample20.cmd?highlight=1)]

Aby można było obsługiwać ten interfejs API sieci Web, musimy uwzględnić dodatkowy parametr w metodzie `DeleteRef`. Poniżej przedstawiono metodę kontrolera do usunięcia produktu z relacji `Supplier.Products`.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample21.cs)]

Parametr *klucza* jest kluczem dostawcy, a parametr *relatedKey* jest kluczem produktu do usunięcia z relacji `Products`. Należy zauważyć, że interfejs API sieci Web automatycznie pobiera klucz z ciągu zapytania.

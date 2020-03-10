---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-routing-conventions
title: Konwencje routingu w programie ASP.NET Web API 2 OData-ASP.NET 4. x
author: MikeWasson
description: Opisuje konwencje routingu, które są używane przez interfejs Web API 2 w ASP.NET 4. x dla punktów końcowych OData.
ms.author: riande
ms.date: 07/31/2013
ms.custom: seoapril2019
ms.assetid: adbc175a-14eb-4ab2-a441-d056ffa8266f
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-routing-conventions
msc.type: authoredcontent
ms.openlocfilehash: 63df4a82cd8df92631485b2544117844cfd0ca56
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78614717"
---
# <a name="routing-conventions-in-aspnet-web-api-2-odata"></a>Konwencje routingu w protokole OData ASP.NET Web API 2

według [Jan Wasson](https://github.com/MikeWasson)

> W tym artykule opisano konwencje routingu, które są używane przez interfejs Web API 2 w ASP.NET 4. x dla punktów końcowych OData.

Gdy interfejs API sieci Web pobiera żądanie OData, mapuje żądanie na nazwę kontrolera i nazwę akcji. Mapowanie jest oparte na metodzie HTTP i identyfikatorze URI. Na przykład `GET /odata/Products(1)` Maps `ProductsController.GetProduct`.

W części 1 tego artykułu opisano wbudowane konwencje routingu OData. Te konwencje są przeznaczone specjalnie dla punktów końcowych OData i zastępują domyślny system routingu interfejsu API sieci Web. (Zastępowanie następuje po wywołaniu **MapODataRoute**).

W części 2 przedstawiono sposób dodawania niestandardowych Konwencji routingu. Obecnie wbudowane konwencje nie obejmują całego zakresu identyfikatorów URI OData, ale można je zwiększyć, aby obsługiwać dodatkowe przypadki.

- [Wbudowane konwencje routingu](#conventions)
- [Niestandardowe Konwencje routingu](#custom)

<a id="conventions"></a>
## <a name="built-in-routing-conventions"></a>Wbudowane konwencje routingu

Przed zdefiniowaniem Konwencji routingu OData w interfejsie API sieci Web warto zrozumieć identyfikatory URI OData. [Identyfikator URI OData](http://www.odata.org/documentation/odata-v3-documentation/url-conventions/) składa się z:

- Katalog główny usługi
- Ścieżka zasobu
- Opcje zapytań

![](odata-routing-conventions/_static/image1.png)

W przypadku routingu ważna część to ścieżka zasobu. Ścieżka zasobu jest podzielona na segmenty. Na przykład `/Products(1)/Supplier` ma trzy segmenty:

- `Products` odwołuje się do zestawu jednostek o nazwie "Products".
- `1` jest kluczem jednostki, wybierając jedną jednostkę z zestawu.
- `Supplier` jest właściwością nawigacji, która wybiera powiązaną jednostkę.

Dlatego ta ścieżka wybiera dostawcę produktu 1.

> [!NOTE]
> Segmenty ścieżki OData nie zawsze odpowiadają segmentom URI. Na przykład "1" jest uznawany za segment ścieżki.

**Nazwy kontrolerów.** Nazwa kontrolera jest zawsze tworzona na podstawie jednostki ustawionej w katalogu głównym ścieżki zasobu. Na przykład jeśli ścieżka zasobu jest `/Products(1)/Supplier`, interfejs API sieci Web szuka kontrolera o nazwie `ProductsController`.

**Nazwy akcji.** Nazwy akcji są wyprowadzane z segmentów ścieżki i modelu Entity Data Model (EDM), jak wymieniono w poniższych tabelach. W niektórych przypadkach dla nazwy akcji dostępne są dwie opcje. Na przykład "Get" lub &quot;getproductss&quot;.

**Wykonywanie zapytań względem jednostek**

| Żądanie | Przykładowy identyfikator URI | Nazwa akcji | Przykład akcji |
| --- | --- | --- | --- |
| Pobierz/EntitySet | /Products | Getentityset lub Get | GetProducts |
| Pobierz/EntitySet (klucz) | /Products (1) | GetEntityType lub Get | Getproduct |
| Pobierz/EntitySet (Key)/Cast | /Products(1)/Models.Book | GetEntityType lub Get | GetBook |

Aby uzyskać więcej informacji, zobacz [Tworzenie punktu końcowego OData tylko do odczytu](odata-v3/creating-an-odata-endpoint.md).

**Tworzenie, aktualizowanie i usuwanie jednostek**

| Żądanie | Przykładowy identyfikator URI | Nazwa akcji | Przykład akcji |
| --- | --- | --- | --- |
| Opublikuj/EntitySet | /Products | PostEntityType lub post | PostProduct |
| PUT/EntitySet (klucz) | /Products (1) | PutEntityType lub Put | PutProduct |
| PUT/EntitySet (Key)/Cast | /Products(1)/Models.Book | PutEntityType lub Put | PutBook |
| Poprawka/EntitySet (klucz) | /Products (1) | PatchEntityType lub poprawka | PatchProduct |
| Poprawka/EntitySet (Key)/Cast | /Products(1)/Models.Book | PatchEntityType lub poprawka | PatchBook |
| Usuń/EntitySet (klucz) | /Products (1) | DeleteEntityType lub Usuń | DeleteProduct |
| Usuń/EntitySet (Key)/Cast | /Products(1)/Models.Book | DeleteEntityType lub Usuń | DeleteBook |

**Wykonywanie zapytania dotyczącego właściwości nawigacji**

| Żądanie | Przykładowy identyfikator URI | Nazwa akcji | Przykład akcji |
| --- | --- | --- | --- |
| Pobierz/EntitySet (Key)/Navigation | /Products (1)/Supplier | GetNavigationFromEntityType lub getnawigacji | GetSupplierFromProduct |
| Pobierz/EntitySet (Key)/Cast/Navigation | /Products(1)/Models.Book/Author | GetNavigationFromEntityType lub getnawigacji | GetAuthorFromBook |

Aby uzyskać więcej informacji, zobacz [Praca z relacjami jednostek](odata-v3/working-with-entity-relations.md).

**Tworzenie i usuwanie linków**

| Żądanie | Przykładowy identyfikator URI | Nazwa akcji |
| --- | --- | --- |
| Ogłoś/EntitySet (Key)/$links/Navigation | /Products (1)/$links/Supplier | CreateLink |
| Umieść/EntitySet (Key)/$links/Navigation | /Products (1)/$links/Supplier | CreateLink |
| Usuń/EntitySet (Key)/$links/Navigation | /Products (1)/$links/Supplier | DeleteLink |
| Usuń/EntitySet (Key)/$links/Navigation (relatedKey) | /Products/(1)/$links/Suppliers (1) | DeleteLink |

Aby uzyskać więcej informacji, zobacz [Praca z relacjami jednostek](odata-v3/working-with-entity-relations.md).

**Właściwości**

*Wymaga interfejsu Web API 2*

| Żądanie | Przykładowy identyfikator URI | Nazwa akcji | Przykład akcji |
| --- | --- | --- | --- |
| Pobierz/EntitySet (Key)/Property | /Products(1)/Name | GetPropertyFromEntityType lub GetProperty | GetNameFromProduct |
| Pobierz/EntitySet (Key)/Cast/Property | /Products(1)/Models.Book/Author | GetPropertyFromEntityType lub GetProperty | GetTitleFromBook |

**Akcje**

| Żądanie | Przykładowy identyfikator URI | Nazwa akcji | Przykład akcji |
| --- | --- | --- | --- |
| Ogłoś/EntitySet (Key)/Action | /Products (1)/rate | ActionNameOnEntityType lub ActionName | RateOnProduct |
| Ogłoś/EntitySet (Key)/Cast/Action | /Products(1)/Models.Book/CheckOut | ActionNameOnEntityType lub ActionName | CheckOutOnBook |

Aby uzyskać więcej informacji, zobacz [Akcje OData](odata-v3/odata-actions.md).

**Sygnatury metod**

Oto kilka reguł dla sygnatur metod:

- Jeśli ścieżka zawiera klucz, Akcja powinna mieć parametr o nazwie *Key*.
- Jeśli ścieżka zawiera klucz do właściwości nawigacji, Akcja powinna mieć parametr o nazwie *relatedKey*.
- Parametry *klucza* dekorować i *relatedKey* z parametrem **[FromODataUri]** .
- Żądania POST i PUT przyjmują parametr typu Entity.
- Żądanie PATCH przyjmuje parametr typu **Delta&lt;t&gt;** , gdzie *t* jest typem jednostki.

Poniżej przedstawiono przykład przedstawiający sygnatury metod dla każdej wbudowanej Konwencji routingu OData.

[!code-csharp[Main](odata-routing-conventions/samples/sample1.cs)]

<a id="custom"></a>
## <a name="custom-routing-conventions"></a>Niestandardowe Konwencje routingu

Obecnie wbudowane konwencje nie obejmują wszystkich możliwych identyfikatorów URI OData. Nowe konwencje można dodać, implementując Interfejs **IODataRoutingConvention** . Ten interfejs ma dwie metody:

[!code-csharp[Main](odata-routing-conventions/samples/sample2.cs)]

- **SelectController** zwraca nazwę kontrolera.
- **SelectAction** zwraca nazwę akcji.

W przypadku obu metod, jeśli Konwencja nie ma zastosowania do tego żądania, metoda powinna zwrócić wartość null.

**ODataPath** parametr reprezentuje ścieżkę do przeanalizowanego zasobu OData. Zawiera listę wystąpień **[ODataPathSegment](https://msdn.microsoft.com/library/system.web.http.odata.routing.odatapathsegment.aspx)** , po jednym dla każdego segmentu ścieżki zasobu. **ODataPathSegment** jest klasą abstrakcyjną; Każdy typ segmentu jest reprezentowany przez klasę, która pochodzi od **ODataPathSegment**.

Właściwość **ODataPath. TemplatePath** to ciąg, który reprezentuje wszystkie segmenty ścieżki. Na przykład jeśli identyfikator URI jest `/Products(1)/Supplier`, szablon ścieżki jest &quot;~/EntitySet/Key/Navigation&quot;. Należy zauważyć, że segmenty nie odpowiadają bezpośrednio na segmenty identyfikatora URI. Na przykład klucz jednostki (1) jest reprezentowany jako własny **ODataPathSegment**.

Zwykle implementacja **IODataRoutingConvention** wykonuje następujące czynności:

1. Porównaj szablon ścieżki, aby sprawdzić, czy ta Konwencja dotyczy bieżącego żądania. Jeśli nie ma zastosowania, zwraca wartość null.
2. Jeśli stosuje się Konwencję, należy użyć właściwości wystąpień **ODataPathSegment** do wyprowadzania nazw kontrolerów i akcji.
3. W przypadku akcji Dodaj dowolne wartości do słownika tras, które powinny być powiązane z parametrami akcji (zazwyczaj klucze jednostek).

Przyjrzyjmy się konkretnemu przykładowi. Wbudowane konwencje routingu nie obsługują indeksowania w kolekcji nawigacji. Innymi słowy, nie istnieje konwencja dla identyfikatorów URI, takich jak następujące:

[!code-javascript[Main](odata-routing-conventions/samples/sample3.js)]

Oto niestandardowa Konwencja routingu do obsługi tego typu zapytania.

[!code-csharp[Main](odata-routing-conventions/samples/sample4.cs)]

Uwagi:

1. Pochodzi z **EntitySetRoutingConvention**, ponieważ metoda **SelectController** w tej klasie jest odpowiednia dla tej nowej konwencji routingu. Oznacza to, że nie trzeba ponownie wdrażać **SelectController**.
2. Konwencja dotyczy tylko żądań GET i tylko wtedy, gdy szablon ścieżki jest &quot;~/EntitySet/Key/Navigation/Key&quot;.
3. Nazwa akcji to &quot;Get {EntityType}&quot;, gdzie *{EntityType}* jest typem kolekcji nawigacji. Na przykład &quot;Get&quot;. Możesz użyć dowolnej konwencji nazewnictwa, aby upewnić się, &#8212; że wszystkie akcje Twojego kontrolera są zgodne.
4. Akcja przyjmuje dwa parametry o nazwach *Key* i *relatedKey*. (Aby zapoznać się z listą niektórych wstępnie zdefiniowanych nazw parametrów, zobacz [ODataRouteConstants](https://msdn.microsoft.com/library/system.web.http.odata.routing.odatarouteconstants.aspx)).

Następnym krokiem jest dodanie nowej konwencji do listy Konwencji routingu. Dzieje się tak podczas konfiguracji, jak pokazano w poniższym kodzie:

[!code-csharp[Main](odata-routing-conventions/samples/sample5.cs)]

Oto kilka innych konwencji przykładowych routingu, które są przydatne do badania:

- [CompositeKeyRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataCompositeKeySample/ODataCompositeKeySample/Extensions/CompositeKeyRoutingConvention.cs)
- [CustomNavigationRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataServiceSample/ODataService/Extensions/CustomNavigationRoutingConvention.cs)
- [NonBindableActionRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataActionsSample/ODataActionsSample/NonBindableActionRoutingConvention.cs)
- [ODataVersionRouteConstraint](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataVersioningSample/ODataVersioningSample/Extensions/ODataVersionRouteConstraint.cs)

Sam interfejs API sieci Web jest to open source, więc można zobaczyć [kod źródłowy](http://aspnetwebstack.codeplex.com/) wbudowanych Konwencji routingu. Są one zdefiniowane w przestrzeni nazw **System. Web. http. OData. Routing. Conventions** .

---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
title: Obsługa relacji jednostek w protokole OData v3 z interfejsu Web API 2 | Dokumentacja firmy Microsoft
author: MikeWasson
description: 'Większość zestawów danych należy zdefiniować relacje między jednostkami: Klienci mają zamówienia; książki mają autorzy; Jeśli te produkty mają dostawców. Przy użyciu protokołu OData, klienci mogą przejść za pośrednictwem...'
ms.author: riande
ms.date: 02/26/2014
ms.assetid: 1e4c2eb4-b6cf-42ff-8a65-4d71ddca0394
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
msc.type: authoredcontent
ms.openlocfilehash: dc80a984911a7f000edc7974992a1ed936ebb348
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65131472"
---
# <a name="supporting-entity-relations-in-odata-v3-with-web-api-2"></a>Obsługa relacji jednostek w protokole OData v3 z interfejsu Web API 2

przez [Mike Wasson](https://github.com/MikeWasson)

[Pobierz ukończony projekt](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> Większość zestawów danych należy zdefiniować relacje między jednostkami: Klienci mają zamówienia; książki mają autorzy; Jeśli te produkty mają dostawców. Używanie protokołu OData, klienci mogą przejść za pośrednictwem relacji jednostek. Biorąc pod uwagę produktu, można znaleźć dostawcy. Można również tworzyć i usuwać relacje. Na przykład można ustawić dostawcę dla produktu.
> 
> W tym samouczku pokazano, jak obsługują te operacje w interfejsie API sieci Web platformy ASP.NET. Samouczek opiera się na samouczka [Tworzenie punktu końcowego OData v3 z sieci Web API 2](creating-an-odata-endpoint.md).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używanego w tym samouczku
> 
> 
> - Internetowy interfejs API 2
> - OData w wersji 3
> - Entity Framework 6

## <a name="add-a-supplier-entity"></a>Dodaj jednostkę dostawcy

Najpierw musimy dodać nowy typ jednostek do naszych źródła danych OData. Dodamy `Supplier` klasy.

[!code-csharp[Main](working-with-entity-relations/samples/sample1.cs)]

Ta klasa używa ciągu klucza jednostki. W praktyce, które mogą okazać się rzadziej niż przy użyciu klucza liczby całkowitej. Jednak warto widoczne, jak OData obsługuje inne typy kluczy, oprócz liczby całkowite.

Następnie utworzymy relację, dodając `Supplier` właściwość `Product` klasy:

[!code-csharp[Main](working-with-entity-relations/samples/sample2.cs)]

Dodaj nową **DbSet** do `ProductServiceContext` klasy tak, aby Entity Framework będzie zawierać `Supplier` tabeli w bazie danych.

[!code-csharp[Main](working-with-entity-relations/samples/sample3.cs?highlight=9)]

W WebApiConfig.cs należy dodać jednostki "Dostawcy" model EDM:

[!code-csharp[Main](working-with-entity-relations/samples/sample4.cs?highlight=4)]

## <a name="navigation-properties"></a>Właściwości nawigacji

Aby uzyskać dostawcy, produktu, klient wysyła żądanie pobrania:

[!code-console[Main](working-with-entity-relations/samples/sample5.cmd)]

Poniżej przedstawiono właściwości nawigacji "Dostawca" na `Product` typu. W tym przypadku `Supplier` odnosi się do jednego elementu, ale nawigacji właściwość może również zwracać kolekcję (relacji jeden do wielu lub wiele do wielu).

Aby zapewnić obsługę tego żądania, dodaj następującą metodę do `ProductsController` klasy:

[!code-csharp[Main](working-with-entity-relations/samples/sample6.cs)]

*Klucz* parametr jest klucz produktu. Metoda ta zwraca obiekt pokrewny&#8212;w tym przypadku `Supplier` wystąpienia. Nazwa metody i nazwa parametru są ważne. Ogólnie rzecz biorąc Jeśli właściwość nawigacji nosi nazwę "X", należy dodać metodę o nazwie "Metody GetX". Metoda przyjmuje parametr o nazwie "*klucz*" który jest zgodny z typem danych klucza nadrzędnego.

Warto również uwzględnić **[FromOdataUri]** atrybutu w *klucz* parametru. Ten atrybut informuje reguły składni OData używane, gdy jej analizuje klucza z identyfikatora URI żądania interfejsu API sieci Web.

## <a name="creating-and-deleting-links"></a>Tworzenie i usuwanie łącza

OData obsługuje tworzenie lub usuwanie relacji między dwiema jednostkami. W terminologii OData relacja jest "Połącz". Każde połączenie ma identyfikator URI z formularzem *jednostki*/$links /*jednostki*. Na przykład łącze z produktu dostawcy wygląda następująco:

[!code-console[Main](working-with-entity-relations/samples/sample7.cmd)]

Aby utworzyć nowy link, klient wysyła żądanie POST do identyfikator URI linku. Treść żądania jest identyfikator URI obiektu docelowego. Na przykład załóżmy, że istnieje dostawcy z kluczem "CTSO". Aby utworzyć łącze z "Product(1)" do "Supplier('CTSO')", klient wysyła żądanie, jak pokazano poniżej:

[!code-console[Main](working-with-entity-relations/samples/sample8.cmd)]

Aby usunąć łącze, klient wysyła żądanie usunięcia łącza identyfikatora URI.

**Tworzenie łączy główny**

Aby włączyć klienta utworzyć łącza dostawcy produktu, Dodaj następujący kod, aby `ProductsController` klasy:

[!code-csharp[Main](working-with-entity-relations/samples/sample9.cs)]

Ta metoda przyjmuje trzy parametry:

- *Klucz*: Kluczem do obiektu nadrzędnego (produkt)
- *Element navigationProperty*: Nazwa właściwości nawigacji. W tym przykładzie właściwość nawigacji z jedynymi prawidłowymi jest "Dostawca".
- *Link*: Powiązana jednostka identyfikatora URI OData. Ta wartość jest pobierana z treści żądania. Na przykład łącze identyfikatora URI może być "`http://localhost/odata/Suppliers('CTSO')`, co oznacza dostawcy o identyfikatorze ="CTSO".

Metoda używa łącze do wyszukiwania dostawcy. Jeśli zostanie znalezione pasujące dostawcy, metoda ustawia `Product.Supplier` właściwości i zapisuje wynik w bazie danych.

Część najtrudniejsze jest analizowanie identyfikator URI linku. Po prostu musisz symulować wynik wysyłającego żądanie GET do tego identyfikatora URI. Następującą metodę pomocnika pokazuje, jak to zrobić. Metoda wywołuje proces routingu internetowego interfejsu API i otrzymuje **element ODataPath** wystąpienia, która reprezentuje przeanalizowany ścieżki OData. Identyfikator URI linku jednego z segmentów powinna być kluczem jednostki. (Jeśli nie, klient wysłał nieprawidłowy identyfikator URI.)

[!code-csharp[Main](working-with-entity-relations/samples/sample10.cs)]

**Usuwanie łącza**

Aby usunąć łącze, Dodaj następujący kod, aby `ProductsController` klasy:

[!code-csharp[Main](working-with-entity-relations/samples/sample11.cs)]

W tym przykładzie właściwość nawigacji jest pojedynczym `Supplier` jednostki. Jeśli właściwość nawigacji jest kolekcją, identyfikator URI do usunięcia łącza musi zawierać klucz dla obiektu pokrewnego. Na przykład:

[!code-console[Main](working-with-entity-relations/samples/sample12.cmd)]

To żądanie usuwa zamówienie 1 z klienta customer 1. W tym przypadku metoda DeleteLink mają następujący podpis:

[!code-csharp[Main](working-with-entity-relations/samples/sample13.cs)]

*RelatedKey* parametru udostępnia klucz dla obiektu pokrewnego. Tak w Twojej `DeleteLink` metody odnośnika podstawowej jednostki przez *klucz* parametru, Znajdź powiązanej jednostki przez *relatedKey* parametru, a następnie Usuń skojarzenie. W zależności od modelu danych, czasami trzeba zaimplementować obie wersje `DeleteLink`. Interfejs API sieci Web będzie wywoływać poprawnej wersji oparte na żądaniu identyfikatora URI.

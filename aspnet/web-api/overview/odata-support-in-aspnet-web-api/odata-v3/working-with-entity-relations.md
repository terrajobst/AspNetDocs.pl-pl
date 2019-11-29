---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
title: Obsługa relacji jednostek w protokole OData V3 przy użyciu interfejsu Web API 2 | Microsoft Docs
author: MikeWasson
description: 'Większość zestawów danych definiuje relacje między jednostkami: klienci mają zamówienia; książki mają autorów; produkty mają dostawców. Korzystając z protokołu OData, klienci mogą przechodzić...'
ms.author: riande
ms.date: 02/26/2014
ms.assetid: 1e4c2eb4-b6cf-42ff-8a65-4d71ddca0394
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
msc.type: authoredcontent
ms.openlocfilehash: 726a7d51123805e05f6831ef9cd7eaa84b6c44bd
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600312"
---
# <a name="supporting-entity-relations-in-odata-v3-with-web-api-2"></a>Obsługa relacji jednostek w protokole OData V3 przy użyciu interfejsu Web API 2

według [Jan Wasson](https://github.com/MikeWasson)

[Pobierz ukończony projekt](https://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> Większość zestawów danych definiuje relacje między jednostkami: klienci mają zamówienia; książki mają autorów; produkty mają dostawców. Korzystając z protokołu OData, klienci mogą nawigować po relacjach jednostek. W danym produkcie można znaleźć dostawcę. Można również tworzyć i usuwać relacje. Na przykład można ustawić dostawcę dla produktu.
> 
> W tym samouczku pokazano, jak obsługiwać te operacje w interfejsie API sieci Web ASP.NET. Samouczek jest oparty na samouczku [Tworzenie punktu końcowego OData V3 przy użyciu interfejsu Web API 2](creating-an-odata-endpoint.md).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używane w samouczku
> 
> 
> - Web API 2
> - Usługa OData w wersji 3
> - Entity Framework 6

## <a name="add-a-supplier-entity"></a>Dodawanie jednostki dostawcy

Najpierw musimy dodać nowy typ jednostki do naszego źródła danych OData. Dodamy klasę `Supplier`.

[!code-csharp[Main](working-with-entity-relations/samples/sample1.cs)]

Ta klasa używa ciągu dla klucza jednostki. W przypadku, które mogą być mniej typowe niż użycie klucza liczb całkowitych. Jednak warto zobaczyć, jak usługa OData obsługuje inne typy kluczy niż liczby całkowite.

Następnie utworzymy relację poprzez dodanie właściwości `Supplier` do klasy `Product`:

[!code-csharp[Main](working-with-entity-relations/samples/sample2.cs)]

Dodaj nowy **nieogólnymi** do klasy `ProductServiceContext`, tak aby Entity Framework obejmował tabelę `Supplier` w bazie danych.

[!code-csharp[Main](working-with-entity-relations/samples/sample3.cs?highlight=9)]

W programie WebApiConfig.cs Dodaj jednostkę "dostawcy" do modelu modelu EDM:

[!code-csharp[Main](working-with-entity-relations/samples/sample4.cs?highlight=4)]

## <a name="navigation-properties"></a>Właściwości nawigacji

Aby uzyskać dostawcę produktu, klient wysyła żądanie GET:

[!code-console[Main](working-with-entity-relations/samples/sample5.cmd)]

Tutaj "dostawca" jest właściwością nawigacji dla typu `Product`. W tym przypadku `Supplier` odnosi się do pojedynczego elementu, ale właściwość nawigacji może również zwracać kolekcję (relacja jeden-do-wielu lub wiele-do-wielu).

Aby obsłużyć to żądanie, Dodaj następującą metodę do klasy `ProductsController`:

[!code-csharp[Main](working-with-entity-relations/samples/sample6.cs)]

Parametr *klucza* jest kluczem produktu. Metoda zwraca pokrewną jednostkę&#8212;w tym przypadku wystąpienie `Supplier`. Nazwa metody i nazwa parametru są oba ważne. Ogólnie rzecz biorąc, jeśli właściwość nawigacji ma nazwę "X", należy dodać metodę o nazwie "GetX". Metoda musi przyjmować parametr o nazwie "*Key*", który jest zgodny z typem danych klucza nadrzędnego.

Ważne jest również uwzględnienie atrybutu **[FromOdataUri]** w parametrze *klucza* . Ten atrybut wskazuje, że interfejs API sieci Web ma używać reguł składni OData podczas analizowania klucza z żądania URI.

## <a name="creating-and-deleting-links"></a>Tworzenie i usuwanie linków

Usługa OData obsługuje tworzenie lub usuwanie relacji między dwoma jednostkami. W terminologii OData relacja jest "link". Każdy link ma identyfikator URI z *jednostką*/$Links/*jednostką*. Na przykład łącze od produktu do dostawcy wygląda następująco:

[!code-console[Main](working-with-entity-relations/samples/sample7.cmd)]

Aby utworzyć nowe łącze, klient wysyła żądanie POST do identyfikatora URI linku. Treść żądania jest identyfikatorem URI jednostki docelowej. Załóżmy na przykład, że istnieje dostawca z kluczem "CTSO". Aby utworzyć łącze "Product (1)" do "dostawca (" CTSO "), klient wysyła żądanie podobne do następujących:

[!code-console[Main](working-with-entity-relations/samples/sample8.cmd)]

Aby usunąć łącze, klient wysyła żądanie usunięcia do identyfikatora URI linku.

**Tworzenie linków**

Aby umożliwić klientowi tworzenie linków dostawcy produktu, Dodaj następujący kod do klasy `ProductsController`:

[!code-csharp[Main](working-with-entity-relations/samples/sample9.cs)]

Ta metoda przyjmuje trzy parametry:

- *klucz*: klucz do jednostki nadrzędnej (produkt)
- *navigationProperty*: Nazwa właściwości nawigacji. W tym przykładzie jedyną prawidłową właściwością nawigacji jest "dostawca".
- *link*: identyfikator URI OData jednostki powiązanej. Ta wartość jest pobierana z treści żądania. Na przykład identyfikator URI linku może mieć wartość "`http://localhost/odata/Suppliers('CTSO')`, co oznacza dostawcę o IDENTYFIKATORze" CTSO ".

Metoda używa linku, aby wyszukać dostawcę. Jeśli zostanie znaleziony pasujący dostawca, Metoda ustawia właściwość `Product.Supplier` i zapisuje wynik do bazy danych.

Najtrudniejszą częścią jest analiza identyfikatora URI linku. Zasadniczo należy zasymulować wynik wysłania żądania GET do tego identyfikatora URI. W poniższej metodzie pomocnika pokazano, jak to zrobić. Metoda wywołuje proces routingu interfejsu API sieci Web i przywraca wystąpienie **ODataPath** reprezentujące przeanalizowane ścieżki OData. Dla identyfikatora URI łącza jeden z segmentów powinien być kluczem jednostki. (Jeśli nie, klient wysłał nieprawidłowy identyfikator URI).

[!code-csharp[Main](working-with-entity-relations/samples/sample10.cs)]

**Usuwanie linków**

Aby usunąć łącze, Dodaj następujący kod do klasy `ProductsController`:

[!code-csharp[Main](working-with-entity-relations/samples/sample11.cs)]

W tym przykładzie właściwość nawigacji jest pojedynczą jednostką `Supplier`. Jeśli właściwość nawigacji jest kolekcją, identyfikator URI służący do usunięcia linku musi zawierać klucz powiązanej jednostki. Na przykład:

[!code-console[Main](working-with-entity-relations/samples/sample12.cmd)]

To żądanie usuwa zamówienie 1 od klienta 1. W takim przypadku Metoda DeleteLink będzie miała następujący podpis:

[!code-csharp[Main](working-with-entity-relations/samples/sample13.cs)]

Parametr *relatedKey* zwraca klucz jednostki powiązanej. Tak więc w metodzie `DeleteLink` należy odszukać jednostkę podstawową według parametru *klucza* , znaleźć powiązaną jednostkę przez parametr *relatedKey* , a następnie usunąć skojarzenie. W zależności od modelu danych może być konieczne zaimplementowanie obu wersji `DeleteLink`. Interfejs API sieci Web będzie wywoływał poprawną wersję na podstawie identyfikatora URI żądania.

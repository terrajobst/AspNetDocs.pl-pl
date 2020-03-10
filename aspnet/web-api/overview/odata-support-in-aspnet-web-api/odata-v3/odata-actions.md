---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
title: Obsługa akcji OData w programie ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: 'W protokole OData akcje są sposobem dodawania zachowań po stronie serwera, które nie są łatwo zdefiniowane jako operacje CRUD na jednostkach. Niektóre zastosowania do akcji obejmują: Implementuj...'
ms.author: riande
ms.date: 02/25/2014
ms.assetid: 2d7b3aa2-aa47-4e6e-b0ce-3d65a1c6fe02
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
msc.type: authoredcontent
ms.openlocfilehash: ae8b23f0868f992cb2bbbf14ee3f7ac848501515
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556358"
---
# <a name="supporting-odata-actions-in-aspnet-web-api-2"></a>Obsługa działań OData w programie ASP.NET Web API 2

według [Jan Wasson](https://github.com/MikeWasson)

[Pobierz ukończony projekt](https://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> W protokole OData *Akcje* są sposobem dodawania zachowań po stronie serwera, które nie są łatwo zdefiniowane jako operacje CRUD na jednostkach. Niektóre zastosowania do akcji obejmują:
> 
> - Implementowanie złożonych transakcji.
> - Jednoczesne manipulowanie kilkoma jednostkami.
> - Zezwalanie na aktualizacje tylko do określonych właściwości jednostki.
> - Wysyłanie informacji do serwera, który nie jest zdefiniowany w jednostce.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używane w samouczku
> 
> 
> - Internetowy interfejs API 2
> - Usługa OData w wersji 3
> - Entity Framework 6

## <a name="example-rating-a-product"></a>Przykład: Ocena produktu

W tym przykładzie chcemy umożliwić użytkownikom ocenianie produktów, a następnie uwidocznienie średniej klasyfikacji dla każdego produktu. W bazie danych zostanie przechowana lista ocen, które zostały dostosowane do produktów.

Oto model, którego możemy użyć do reprezentowania klasyfikacji w Entity Framework:

[!code-csharp[Main](odata-actions/samples/sample1.cs)]

Ale nie chcemy, aby klienci OGŁASZAli obiekt `ProductRating` w kolekcji "ratings". Intuicyjnie, ocena jest skojarzona z kolekcją produkty, a klient powinien potrzebować tylko do opublikowania wartości klasyfikacji.

W związku z tym zamiast używania normalnych operacji CRUD definiujemy akcję, którą klient może wywołać w produkcie. W terminologii OData akcja jest *powiązana* z jednostkami produktu.

>Akcje mają efekty uboczne na serwerze. Z tego powodu są wywoływane przy użyciu żądań HTTP POST. Akcje mogą zawierać parametry i typy zwracane, które są opisane w metadanych usługi. Klient wysyła parametry w treści żądania, a serwer wysyła wartość zwracaną w treści odpowiedzi. Aby wywołać akcję "Oceń produkt", klient wysyła wpis do identyfikatora URI w następujący sposób:

[!code-console[Main](odata-actions/samples/sample2.cmd)]

Dane w żądaniu POST są po prostu klasyfikacją produktu:

[!code-console[Main](odata-actions/samples/sample3.cmd)]

## <a name="declare-the-action-in-the-entity-data-model"></a>Zadeklaruj akcję w Entity Data Model

W konfiguracji internetowego interfejsu API Dodaj akcję do modelu Entity Data Model (EDM):

[!code-csharp[Main](odata-actions/samples/sample4.cs)]

Ten kod definiuje "RateProduct" jako akcję, którą można wykonać w jednostkach produktu. Deklaruje także, że akcja przyjmuje parametr **int** o nazwie "Rating" i zwraca wartość **int** .

## <a name="add-the-action-to-the-controller"></a>Dodaj akcję do kontrolera

Akcja "RateProduct" jest powiązana z jednostkami produktu. Aby zaimplementować akcję, Dodaj metodę o nazwie `RateProduct` do kontrolera produktów:

[!code-csharp[Main](odata-actions/samples/sample5.cs)]

Zwróć uwagę, że nazwa metody jest zgodna z nazwą akcji w modelu EDM. Metoda ma dwa parametry:

- *klucz*: klucz produktu do oceniania.
- *Parametry*: słownik wartości parametrów akcji.

Jeśli używasz domyślnych Konwencji routingu, parametr klucza musi mieć nazwę "klucz". Ważne jest również uwzględnienie atrybutu **[FromOdataUri]** , jak pokazano. Ten atrybut wskazuje, że interfejs API sieci Web ma używać reguł składni OData podczas analizowania klucza z żądania URI.

Użyj słownika *parametrów* , aby uzyskać parametry akcji:

[!code-csharp[Main](odata-actions/samples/sample6.cs)]

Jeśli klient wysyła parametry akcji w prawidłowym formacie, wartość **ModelState. IsValid** ma wartość true. W takim przypadku można pobrać wartości parametrów przy użyciu słownika **ODataActionParameters** . W tym przykładzie akcja `RateProduct` przyjmuje jeden parametr o nazwie "Rating".

## <a name="action-metadata"></a>Metadane akcji

Aby wyświetlić metadane usługi, Wyślij żądanie GET do/OData/$metadata. Poniżej znajduje się część metadanych, która deklaruje `RateProduct` akcję:

[!code-xml[Main](odata-actions/samples/sample7.xml)]

Element **FunctionImport** deklaruje akcję. Większość pól nie ma wyjaśnień, ale dwie są następujące:

- **Isbindd** oznacza, że akcja może być wywoływana w jednostce docelowej, co najmniej jakiś czas.
- **IsAlwaysBindable** oznacza, że akcja zawsze może być wywoływana w jednostce docelowej.

Różnica polega na tym, że niektóre akcje są zawsze dostępne dla klientów, ale inne akcje mogą zależeć od stanu jednostki. Załóżmy na przykład, że zdefiniujesz akcję "Kup". Można kupić tylko element, który znajduje się w magazynie. Jeśli element jest poza magazynem, klient nie może wywołać tej akcji.

Podczas definiowania modelu EDM, Metoda **akcji** tworzy zawsze powiązane akcje:

[!code-csharp[Main](odata-actions/samples/sample8.cs?highlight=1)]

Porozmawiam w dalszej części tego tematu o akcjach, które nie są zawsze wiązane (nazywane również akcjami *przejściowymi* ).

## <a name="invoking-the-action"></a>Wywoływanie akcji

Teraz zobaczmy, jak klient wywoła tę akcję. Załóżmy, że klient chce przekazać klasyfikację 2 do produktu o IDENTYFIKATORze = 4. Oto przykładowy komunikat żądania, który używa formatu JSON dla treści żądania:

[!code-console[Main](odata-actions/samples/sample9.cmd)]

Oto komunikat odpowiedzi:

[!code-console[Main](odata-actions/samples/sample10.cmd)]

## <a name="binding-an-action-to-an-entity-set"></a>Wiązanie akcji z zestawem jednostek

W poprzednim przykładzie akcja jest powiązana z pojedynczą jednostką: klient klasyfikuje pojedynczy produkt. Można również powiązać akcję z kolekcją jednostek. Po prostu wprowadź następujące zmiany:

W modelu EDM Dodaj akcję do właściwości **kolekcji** jednostki.

[!code-csharp[Main](odata-actions/samples/sample11.cs?highlight=1)]

W metodzie kontrolera Pomiń parametr *klucza* .

[!code-csharp[Main](odata-actions/samples/sample12.cs)]

Teraz klient wywołuje akcję dla zestawu jednostek Products:

[!code-console[Main](odata-actions/samples/sample13.cmd)]

## <a name="actions-with-collection-parameters"></a>Akcje z parametrami kolekcji

Akcje mogą mieć parametry, które pobierają kolekcję wartości. W modelu EDM Użyj **CollectionParameter&lt;t&gt;** , aby zadeklarować parametr.

[!code-csharp[Main](odata-actions/samples/sample14.cs)]

Deklaruje on parametr o nazwie "ratings", który pobiera kolekcję wartości **int** . W metodzie kontrolera nadal otrzymujesz wartość parametru z obiektu **ODataActionParameters** , ale teraz wartość jest typu **ICollection&lt;int&gt;** wartość:

[!code-csharp[Main](odata-actions/samples/sample15.cs)]

## <a name="transient-actions"></a>Akcje przejściowe

W przykładzie "RateProduct" użytkownicy zawsze mogą ocenić produkt, więc akcja jest zawsze dostępna. Jednak niektóre akcje zależą od stanu jednostki. Na przykład w usłudze najmu wideo akcja "wyewidencjonowywanie" nie zawsze jest dostępna. (Jest to zależne od tego, czy kopia tego wideo jest dostępna). Akcja tego typu jest nazywana akcją *przejściową* .

W metadanych usługi przejściowa akcja ma **IsAlwaysBindable** równa false. Jest to rzeczywista wartość domyślna, więc metadane będą wyglądać następująco:

[!code-xml[Main](odata-actions/samples/sample16.xml)]

Oto dlaczego te kwestie: Jeśli akcja jest przejściowa, serwer musi poinformować klienta, gdy akcja jest dostępna. Robi to poprzez dołączenie linku do akcji w jednostce. Oto przykład dla jednostki filmu:

[!code-console[Main](odata-actions/samples/sample17.cmd)]

Właściwość "#CheckOut" zawiera link do akcji wyewidencjonowywania. Jeśli akcja jest niedostępna, serwer pomija link.

Aby zadeklarować przejściową akcję w modelu EDM, wywołaj metodę **TransientAction** :

[!code-csharp[Main](odata-actions/samples/sample18.cs)]

Ponadto należy podać funkcję, która zwraca link akcji dla danej jednostki. Ustaw tę funkcję, wywołując **HasActionLink**. Funkcję można napisać jako wyrażenie lambda:

[!code-csharp[Main](odata-actions/samples/sample19.cs)]

Jeśli akcja jest dostępna, wyrażenie lambda zwraca link do akcji. Serializator OData zawiera ten link, gdy serializować jednostkę. Gdy akcja jest niedostępna, funkcja zwraca `null`.

## <a name="additional-resources"></a>Dodatkowe materiały

[Przykład akcji OData](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v3/ODataActionsSample/)

---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
title: 'Część 2: Tworzenie modeli domeny | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/03/2012
ms.assetid: fe3ef85f-bdc6-4e10-9768-25aa565c01d0
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
msc.type: authoredcontent
ms.openlocfilehash: 7c5ed1bdb4b390c94907b14e208231f16ad42d96
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600360"
---
# <a name="part-2-creating-the-domain-models"></a>Część 2: Tworzenie modeli domeny

według [Jan Wasson](https://github.com/MikeWasson)

[Pobierz ukończony projekt](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-models"></a>Dodaj modele

Istnieją trzy sposoby podejścia Entity Framework:

- Baza danych — pierwsza: rozpoczyna się od bazy danych, a Entity Framework generuje kod.
- Model — pierwszy: rozpoczyna się od modelu wizualnego, a Entity Framework generuje zarówno bazę danych, jak i kod.
- Kod — pierwszy: rozpoczyna się od kodu, a Entity Framework generuje bazę danych.

Korzystamy z metody pierwszego kodu, więc zacznijmy od definiowania naszych obiektów domeny jako POCOs (zwykłych obiektów CLR). W przypadku pierwszego podejścia do kodu obiekty domeny nie potrzebują dodatkowego kodu do obsługi warstwy bazy danych, takich jak transakcje lub trwałość. (W przeciwnym razie nie muszą dziedziczyć z klasy [EntityObject](https://msdn.microsoft.com/library/system.data.objects.dataclasses.entityobject.aspx) ). Można nadal używać adnotacji danych do kontrolowania sposobu, w jaki Entity Framework tworzy schemat bazy danych.

Ponieważ POCOs nie posiadają żadnych dodatkowych właściwości, które opisują [stan bazy danych](https://msdn.microsoft.com/library/system.data.entitystate.aspx), można je łatwo serializować do formatu JSON lub XML. Nie oznacza to jednak, że zawsze należy uwidocznić modele Entity Framework bezpośrednio dla klientów, ponieważ zostaną one wyświetlone w dalszej części tego samouczka.

Utworzymy następujący POCOs:

- Produkt
- Porządek
- OrderDetail

Aby utworzyć każdą klasę, kliknij prawym przyciskiem myszy folder modele w Eksplorator rozwiązań. Z menu kontekstowego wybierz pozycję **Dodaj** , a następnie wybierz pozycję **Klasa.**

![](using-web-api-with-entity-framework-part-2/_static/image1.png)

Dodaj klasę `Product` przy użyciu następującej implementacji:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample1.cs)]

Zgodnie z Konwencją Entity Framework używa właściwości `Id` jako klucza podstawowego i mapuje ją do kolumny tożsamości w tabeli bazy danych. Gdy tworzysz nowe wystąpienie `Product`, nie ustawisz wartości dla `Id`, ponieważ baza danych generuje wartość.

Atrybut **ScaffoldColumn** informuje ASP.NET MVC, aby pominąć Właściwość `Id` podczas generowania formularza edytora. **Wymagany** atrybut jest używany do walidacji modelu. Określa, że właściwość `Name` nie może być pustym ciągiem.

Dodaj klasę `Order`:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample2.cs)]

Dodaj klasę `OrderDetail`:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample3.cs)]

## <a name="foreign-key-relations"></a>Relacje klucza obcego

Zamówienie zawiera wiele szczegółów zamówienia, a wszystkie szczegóły zamówienia odnoszą się do jednego produktu. Aby przedstawić te relacje, Klasa `OrderDetail` definiuje właściwości o nazwie `OrderId` i `ProductId`. Entity Framework będzie wnioskować, że te właściwości reprezentują klucze obce i doda do bazy danych ograniczenia klucza obcego.

![](using-web-api-with-entity-framework-part-2/_static/image2.png)

Klasy `Order` i `OrderDetail` obejmują również właściwości "Nawigacja", które zawierają odwołania do obiektów pokrewnych. Mając zamówienie, można przejść do produktów w kolejności, postępując zgodnie z właściwościami nawigacji.

Kompiluj projekt teraz. Entity Framework używa odbicia w celu odnajdywania właściwości modeli, dlatego wymaga skompilowanego zestawu do utworzenia schematu bazy danych.

## <a name="configure-the-media-type-formatters"></a>Konfigurowanie elementów formatujących typ nośnika

Program [formatujący typ nośnika](../../formats-and-model-binding/media-formatters.md) jest obiektem, który serializować dane, gdy interfejs API sieci Web zapisuje treść odpowiedzi HTTP. Wbudowane elementy formatujące obsługują dane JSON i XML. Domyślnie oba te programy formatujące są serializowane wszystkie obiekty według wartości.

Serializacja przez wartość tworzy problem, jeśli Graf obiektu zawiera odwołania cykliczne. Jest to dokładnie przypadek z klasami `Order` i `OrderDetail`, ponieważ każdy z nich przechowuje odwołanie do drugiego. Program formatujący będzie podążał za odwołaniami, pisząc każdy obiekt według wartości i przejść w okręgach. W związku z tym musimy zmienić zachowanie domyślne.

W Eksplorator rozwiązań rozwiń węzeł aplikacja\_folder Start i Otwórz plik o nazwie WebApiConfig.cs. Dodaj następujący kod do klasy `WebApiConfig`:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample4.cs?highlight=11)]

Ten kod ustawia program formatujący JSON w celu zachowania odwołań do obiektów i całkowicie usuwa plik XML z potoku. (Można skonfigurować program formatujący XML, aby zachować odwołania do obiektów, ale jest nieco bardziej wydajny i potrzebujemy tylko formatu JSON dla tej aplikacji. Aby uzyskać więcej informacji, zobacz [Obsługa cyklicznych odwołań do obiektów](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).)

> [!div class="step-by-step"]
> [Poprzednie](using-web-api-with-entity-framework-part-1.md)
> [dalej](using-web-api-with-entity-framework-part-3.md)

---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
title: Część 6. Tworzenie kontrolerów produktu i kolejności | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 91ee29ee-0689-40ee-914a-e7dd733b6622
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
msc.type: authoredcontent
ms.openlocfilehash: e0bf88e3477acbde910cde956042449bc86ce79a
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600029"
---
# <a name="part-6-creating-product-and-order-controllers"></a>Część 6. Tworzenie kontrolerów produktu i kolejności

według [Jan Wasson](https://github.com/MikeWasson)

[Pobierz ukończony projekt](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-a-products-controller"></a>Dodawanie kontrolera produktów

Kontroler administracyjny jest przeznaczony dla użytkowników, którzy mają uprawnienia administratora. Z drugiej strony klienci mogą wyświetlać produkty, ale nie mogą ich tworzyć, aktualizować ani usuwać.

Można łatwo ograniczyć dostęp do metod post, PUT i DELETE, pozostawiając otwarte metody get. Sprawdź dane, które są zwracane dla produktu:

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample1.json?highlight=1)]

Właściwość `ActualCost` nie powinna być widoczna dla klientów! Rozwiązaniem jest zdefiniowanie *obiektu transferu danych* (DTO), który zawiera podzbiór właściwości, które powinny być widoczne dla klientów. Do `ProductDTO` wystąpień będziemy używać wystąpień LINQ to Project `Product`.

Dodaj klasę o nazwie `ProductDTO` do folderu modele.

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample2.cs)]

Teraz Dodaj kontroler. W Eksplorator rozwiązań kliknij prawym przyciskiem myszy folder controllers. Wybierz pozycję **Dodaj**, a następnie pozycję **kontroler**. W oknie dialogowym **Dodawanie kontrolera** nazwij kontroler &quot;ProductsController&quot;. W obszarze **szablon**wybierz pozycję **pusty kontroler interfejsu API**.

![](using-web-api-with-entity-framework-part-6/_static/image1.png)

Zastąp wszystko w pliku źródłowym następującym kodem:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample3.cs)]

Kontroler nadal używa `OrdersContext` do wykonywania zapytań w bazie danych. Jednak zamiast zwracać `Product` wystąpienia bezpośrednio, wywołamy `MapProducts`, aby zaprojektować je na wystąpieniach `ProductDTO`:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample4.cs?highlight=1)]

Metoda `MapProducts` zwraca interfejs **IQueryable**, więc możemy złożyć wynik z innymi parametrami zapytania. Można to zobaczyć w metodzie `GetProduct`, która dodaje klauzulę **WHERE** do zapytania:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample5.cs?highlight=2)]

## <a name="add-an-orders-controller"></a>Dodawanie kontrolera zamówień

Następnie Dodaj kontroler, który umożliwia użytkownikom tworzenie i wyświetlanie zamówień.

Zaczniemy od innego DTO. W Eksplorator rozwiązań kliknij prawym przyciskiem myszy folder modele i Dodaj klasę o nazwie `OrderDTO` Użyj następującej implementacji:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample6.cs)]

Teraz Dodaj kontroler. W Eksplorator rozwiązań kliknij prawym przyciskiem myszy folder controllers. Wybierz pozycję **Dodaj**, a następnie pozycję **kontroler**. W oknie dialogowym **Dodawanie kontrolera** ustaw następujące opcje:

- W obszarze **nazwa kontrolera**wprowadź wartość "OrdersController".
- W obszarze **szablon**wybierz pozycję "kontroler interfejsu API z akcjami odczytu/zapisu, używając Entity Framework".
- W obszarze **Klasa modelu**wybierz&quot;&quot;zamówienie (ProductStore. models).
- W obszarze **Klasa kontekstu danych**wybierz&quot;&quot;OrdersContext (ProductStore. models).

![](using-web-api-with-entity-framework-part-6/_static/image2.png)

Kliknij przycisk **Dodaj**. Spowoduje to dodanie pliku o nazwie OrdersController.cs. Następnie musimy zmodyfikować domyślną implementację kontrolera.

Najpierw usuń `PutOrder` i `DeleteOrder` metody. W tym przykładzie klienci nie mogą modyfikować ani usuwać istniejących zamówień. W rzeczywistej aplikacji potrzebna jest duża logika zaplecza, która będzie obsługiwać te przypadki. (Na przykład czy zamówienie zostało już wysłane?)

Zmień metodę `GetOrders`, aby zwracała tylko zamówienia należące do użytkownika:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample7.cs)]

Zmień metodę `GetOrder` w następujący sposób:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample8.cs)]

Poniżej przedstawiono zmiany wprowadzone w metodzie:

- Wartość zwracana jest wystąpieniem `OrderDTO`, a nie `Order`.
- Gdy wysyłamy zapytanie do bazy danych dla zamówienia, użyjemy metody [DBQuery. include](https://msdn.microsoft.com/library/gg696395) , aby pobrać powiązane `OrderDetail` i `Product` jednostki.
- Wynik zostanie spłaszczony przy użyciu projekcji.

Odpowiedź HTTP będzie zawierać tablicę produktów z ilościami:

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample9.json)]

Ten format jest łatwiejszy w przypadku klientów korzystających z grafu oryginalnego obiektu, który zawiera zagnieżdżone jednostki (zamówienie, szczegóły i produkty).

Ostatnia metoda do uwzględnienia `PostOrder`. Teraz ta metoda przyjmuje wystąpienie `Order`. Należy jednak wziąć pod uwagę to, co się stanie, jeśli klient wyśle treść żądania w następujący sposób:

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample10.json)]

Jest to dobrze skonstruowana kolejność, a Entity Framework będzie Happily Wstawianie go do bazy danych. Ale zawiera jednostkę produktu, która nie istniała wcześniej. Klient właśnie utworzył nowy produkt w naszej bazie danych. Jest to nieoczekiwane dla działu realizacji zamówień, gdy zobaczymy zamówienie dla Koala. Dobrym wymaganiem jest, aby zachować ostrożność w przypadku danych akceptowanych w żądaniu POST lub PUT.

Aby uniknąć tego problemu, Zmień metodę `PostOrder`, aby wykonać `OrderDTO` wystąpienie. Użyj `OrderDTO`, aby utworzyć `Order`.

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample11.cs)]

Zwróć uwagę, że używamy właściwości `ProductID` i `Quantity` i zignorujemy wszelkie wartości, które klient wysłał jako nazwę produktu lub cenę. Jeśli identyfikator produktu jest nieprawidłowy, nastąpi naruszenie ograniczenia klucza obcego w bazie danych, a wstawienie nie powiedzie się.

Oto pełna Metoda `PostOrder`:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample12.cs)]

Na koniec Dodaj atrybut **Autoryzuj** do kontrolera:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample13.cs)]

Teraz tylko zarejestrowani użytkownicy mogą tworzyć i wyświetlać zamówienia.

> [!div class="step-by-step"]
> [Poprzednie](using-web-api-with-entity-framework-part-5.md)
> [dalej](using-web-api-with-entity-framework-part-7.md)

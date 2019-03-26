---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
title: Część 7. Tworzenie głównej strony | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: eb32a17b-626c-4373-9a7d-3387992f3c04
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
msc.type: authoredcontent
ms.openlocfilehash: 91a6496e2640668c58ec0493d47d909e2de67367
ms.sourcegitcommit: 289e051cc8a90e8f7127e239fda73047bde4de12
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/25/2019
ms.locfileid: "58421353"
---
<a name="part-7-creating-the-main-page"></a>Część 7. Tworzenie strony głównej
====================
przez [Mike Wasson](https://github.com/MikeWasson)

[Pobierz ukończony projekt](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-the-main-page"></a>Tworzenie strony głównej

W tej sekcji utworzysz strony głównej aplikacji. Ta strona będzie bardziej skomplikowane niż strony administratora, co firma Microsoft będzie podejście w kilku krokach. Po drodze zobaczysz niektóre bardziej zaawansowane techniki struktura Knockout.js. Poniżej przedstawiono podstawowy układ strony:

![](using-web-api-with-entity-framework-part-7/_static/image1.png)

- "Produkty" zawiera szereg produktów.
- "Koszyka" przechowywać tablicę produkty z ilości. Klikając pozycję "Dodaj do koszyka" aktualizuje koszyka.
- "Orders" zawiera tablicę identyfikatorów zamówień.
- "Szczegóły" zawiera szczegóły zamówień, który jest tablicą elementów (produkty z ilościami)

Zaczniemy, definiując niektóre podstawowy układ w języku HTML, bez powiązania danych lub skryptu. Otwórz plik Views/Home/Index.cshtml i Zastąp całą zawartość następujących czynności:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample1.html)]

Następnie dodaj sekcję skrypty i utworzyć pusty model widoku:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-7/samples/sample2.cshtml)]

Na podstawie projektu rysowane wcześniej, nasz model view potrzebuje dostrzegalne elementy produktów, koszyk, zamówień i szczegółowe informacje. Dodaj następujące zmienne `AppViewModel` obiektu:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample3.js)]

Użytkownicy mogą dodać elementy z listy produktów do koszyka i usuwanie elementów z koszyka. Do hermetyzacji tych funkcji, utworzymy innej klasy model widoku, który reprezentuje produkt. Dodaj następujący kod do `AppViewModel`:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample4.js?highlight=4)]

`ProductViewModel` Klasa zawiera dwie funkcje, które są używane do przenoszenia produktu do i z koszyka: `addItemToCart` dodaje jedną jednostkę produktu do koszyka, a `removeAllFromCart` spowoduje usunięcie wszystkich ilości produktu.

Użytkownicy mogą wybrać istniejące zamówienie i Pobierz szczegóły zamówienia. Umieściliśmy tej funkcji do innego modelu widoku:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample5.js?highlight=4)]

`OrderDetailsViewModel` Jest inicjowany za pomocą zamówienia, a jej pobiera szczegóły zamówienia, wysyłając żądanie AJAX do serwera.

Zauważ również, `total` właściwość `OrderDetailsViewModel`. Ta właściwość jest specjalnym rodzajem obserwowalnymi o nazwie [obliczane możliwość obserwowania](http://knockoutjs.com/documentation/computedObservables.html). Jak wskazuje nazwa, obliczana możliwość obserwowania umożliwia powiązanie danych z obliczoną wartością&#8212;w tym przypadku całkowity koszt zamówienia.

Następnie dodaj te funkcje, aby `AppViewModel`:

- `resetCart` Usuwa wszystkie elementy z koszyka.
- `getDetails` pobiera szczegóły zamówienia (wypychając nową `OrderDetailsViewModel` na `details` listy).
- `createOrder` Tworzy nowe zamówienie i usuwa zawartość koszyka.


[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample6.js?highlight=4)]

Na koniec można zainicjować modelu widoku, wprowadzając wysyłanie żądań AJAX dla produktów i zamówień:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample7.js)]

Dobrze, to duża ilość kodu, ale dostosowaliśmy ją się krok po kroku, więc miejmy nadzieję projektu jest wyczyszczone. Teraz możemy dodać niektóre wiązania struktura Knockout.js w formacie HTML.

**Produkty**

Poniżej przedstawiono powiązania dla listy produktów:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample8.html)]

Wykonuje iterację przez tablicę produktów i wyświetla nazwę i ceny. Przycisk "Dodaj do Order" jest widoczny tylko wtedy, gdy użytkownik jest zalogowany.

Wywołuje przycisk "Dodaj do Order" `addItemToCart` na `ProductViewModel` wystąpienia produktu. W tym przykładzie pokazano, jak funkcja dobre rozwiązanie z użyciem Knockout.js: Jeśli model widoku zawiera innych modeli widoków, można zastosować powiązania do wewnętrznego modelu. W tym przykładzie powiązania w ramach `foreach` są stosowane do wszystkich `ProductViewModel` wystąpień. To podejście jest znacznie bardziej przejrzyste niż umieszczenie wszystkich funkcji w pojedynczy model widoku.

**Koszyk**

Poniżej przedstawiono wiązania koszyka:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample9.html)]

Wykonuje iterację przez tablicę koszyka i wyświetla nazwę, ceny i ilości. Należy pamiętać o tym, czy link "Usuń" oraz przycisk "Utwórz zamówienie" są powiązane z funkcji model widoku.

**Zamówienia**

Poniżej przedstawiono powiązania dla listy zamówień:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample10.html)]

To wykonuje iterację na zamówień i zawiera identyfikator zamówienia. Zdarzenie kliknięcia łącze jest powiązany z `getDetails` funkcji.

**Szczegóły zamówienia**

Poniżej przedstawiono wiązania szczegóły zamówienia:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample11.html)]

Iteruje przez elementy w kolejności i wyświetla produktu, ceny i ilości. Div otaczającego jest widoczne tylko wtedy, gdy tablica szczegółowe informacje zawiera jeden lub więcej elementów.

## <a name="conclusion"></a>Wniosek

W tym samouczku utworzono aplikację, która korzysta z programu Entity Framework do komunikacji z bazą danych i Web API platformy ASP.NET, aby udostępnić interfejs publicznie dostępnych na podstawie warstwy danych. ASP.NET MVC 4 są używane do renderowania stron HTML i struktura Knockout.js oraz jQuery zapewniają dynamiczne interakcje bez ładuje ponownie stronę.

Dodatkowe zasoby:

- [ASP.NET Data Access Content Map](https://msdn.microsoft.com/library/6759sth4.aspx)
- [Entity Framework Developer Center](https://msdn.microsoft.com/data/ef)

> [!div class="step-by-step"]
> [Poprzednie](using-web-api-with-entity-framework-part-6.md)

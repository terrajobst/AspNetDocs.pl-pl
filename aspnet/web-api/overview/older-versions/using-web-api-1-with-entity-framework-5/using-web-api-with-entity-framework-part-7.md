---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
title: Część 7. Tworzenie strony głównej | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: eb32a17b-626c-4373-9a7d-3387992f3c04
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
msc.type: authoredcontent
ms.openlocfilehash: fe4074c701159a137be3644d65ca844f160c2399
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599988"
---
# <a name="part-7-creating-the-main-page"></a>Część 7. Tworzenie strony głównej

według [Jan Wasson](https://github.com/MikeWasson)

[Pobierz ukończony projekt](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-the-main-page"></a>Tworzenie strony głównej

W tej sekcji utworzysz główną stronę aplikacji. Ta strona będzie bardziej złożona niż strona administracyjna, dlatego będziemy rozmieścić ją w kilku krokach. W ten sposób zobaczysz bardziej zaawansowane techniki odcinania. js. Oto podstawowy układ strony:

![](using-web-api-with-entity-framework-part-7/_static/image1.png)

- "Produkty" przechowuje tablicę produktów.
- "Koszyk" zawiera tablicę produktów z ilościami. Kliknięcie pozycji "Dodaj do koszyka" powoduje aktualizację koszyka.
- "Zamówienia" przechowuje tablicę identyfikatorów zamówień.
- "Szczegóły" zawiera szczegółowe informacje o zamówieniach, czyli tablicę elementów (produkty z ilościami)

Zaczniemy od definiowania podstawowego układu w języku HTML, bez powiązania danych ani skryptu. Otwórz widok plików/home/index. cshtml i Zastąp całą zawartość następującym:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample1.html)]

Następnie Dodaj sekcję skrypty i Utwórz pusty model widoku:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-7/samples/sample2.cshtml)]

W oparciu o przedstawiony wcześniej projekt nasz model widoku wymaga observables dla produktów, koszyków, zamówień i szczegółów. Dodaj następujące zmienne do obiektu `AppViewModel`:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample3.js)]

Użytkownicy mogą dodawać elementy z listy produktów do koszyka i usuwać elementy z koszyka. Aby hermetyzować te funkcje, utworzymy kolejną klasę View-model, która reprezentuje produkt. Dodaj następujący kod do `AppViewModel`:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample4.js?highlight=4)]

Klasa `ProductViewModel` zawiera dwie funkcje, które są używane do przenoszenia produktu do i z koszyka: `addItemToCart` dodaje jedną jednostkę produktu do koszyka, a `removeAllFromCart` usuwa wszystkie ilości produktu.

Użytkownicy mogą wybrać istniejące zamówienie i uzyskać szczegółowe informacje o zamówieniu. Ta funkcja zostanie hermetyzowana do innego widoku — model:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample5.js?highlight=4)]

`OrderDetailsViewModel` jest inicjowana z kolejnością i pobiera szczegóły zamówienia przez wysłanie żądania AJAX do serwera.

Zauważ również, że właściwość `total` w `OrderDetailsViewModel`. Ta właściwość jest specjalnym rodzajem dostrzegalnych elementów o nazwie [obliczone zauważalne](http://knockoutjs.com/documentation/computedObservables.html). Jak nazywa się, obliczone zauważalne umożliwia powiązanie danych z wartością&#8212;obliczaną w tym przypadku, łączny koszt zamówienia.

Następnie Dodaj następujące funkcje do `AppViewModel`:

- `resetCart` usuwa wszystkie elementy z koszyka.
- `getDetails` pobiera szczegóły dla zamówienia (przez wypchnięcie nowej `OrderDetailsViewModel` na listę `details`).
- `createOrder` tworzy nowe zamówienie i opróżnia koszyk.

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample6.js?highlight=4)]

Na koniec zainicjuj model widoku, wykonując żądania AJAX dotyczące produktów i zamówień:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample7.js)]

Jest to bardzo dużo kodu, ale tworzymymy ją krok po kroku, więc miejmy nadzieję projekt jest przejrzysty. Teraz możemy dodać do kodu HTML pewne powiązania odcinania. js.

**Produkty**

Oto powiązania z listą produktów:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample8.html)]

To iteruje tablicę produkty i wyświetla nazwę i cenę. Przycisk "Dodaj do zamówienia" jest widoczny tylko wtedy, gdy użytkownik jest zalogowany.

Przycisk "Dodaj do zamówienia" wywołuje `addItemToCart` w wystąpieniu `ProductViewModel` dla produktu. Przedstawia to całkiem funkcję odcinania. js: Gdy model widoku zawiera inne modele widoku, można zastosować powiązania z modelem wewnętrznym. W tym przykładzie powiązania w `foreach` są stosowane do każdego wystąpienia `ProductViewModel`. To podejście jest znacznie bardziej przejrzyste niż umieszczenie wszystkich funkcji w jednym modelu widoku.

**Koszyk**

Oto powiązania dla koszyka:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample9.html)]

To iteruje tablicę koszyka i wyświetla nazwę, cenę i liczbę. Zwróć uwagę, że łącze "Usuń" i przycisk "Utwórz zamówienie" są powiązane z funkcjami widoku-model.

**Zamówienie**

Oto powiązania z listą Orders (zamówienia):

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample10.html)]

Powoduje to iterację zamówień i pokazuje identyfikator zamówienia. Zdarzenie kliknięcia w łączu jest powiązane z funkcją `getDetails`.

**Szczegóły zamówienia**

Oto powiązania dla szczegółów zamówienia:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample11.html)]

To iteruje elementy w kolejności i wyświetla produkt, cenę i ilość. Otaczający blok DIV jest widoczny tylko wtedy, gdy tablica szczegółów zawiera jeden lub więcej elementów.

## <a name="conclusion"></a>Podsumowanie

W tym samouczku utworzysz aplikację, która używa Entity Framework do komunikowania się z bazą danych, i ASP.NET interfejs API sieci Web, aby zapewnić publiczny interfejs na podstawie warstwy danych. Korzystamy z ASP.NET MVC 4 do renderowania stron HTML oraz odcinania. js Plus jQuery, aby zapewnić dynamiczne interakcje bez ponownego ładowania stron.

Dodatkowe zasoby:

- [Mapa zawartości dostępu do danych ASP.NET](https://msdn.microsoft.com/library/6759sth4.aspx)
- [Centrum deweloperów Entity Framework](https://msdn.microsoft.com/data/ef)

> [!div class="step-by-step"]
> [Poprzednie](using-web-api-with-entity-framework-part-6.md)

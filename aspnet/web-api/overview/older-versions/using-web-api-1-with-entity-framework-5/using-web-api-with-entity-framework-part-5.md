---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
title: 'Część 5: Tworzenie dynamicznego interfejsu użytkownika z odcinaniem. js | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 9d9cb3b0-f4a7-434e-a508-9fc0ad0eb813
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
msc.type: authoredcontent
ms.openlocfilehash: bbdeba756de7986cfeb92aa10a57f4f101382f99
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599995"
---
# <a name="part-5-creating-a-dynamic-ui-with-knockoutjs"></a>Część 5. Tworzenie dynamicznego interfejsu użytkownika przy użyciu narzędzia separowania. js

według [Jan Wasson](https://github.com/MikeWasson)

[Pobierz ukończony projekt](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-a-dynamic-ui-with-knockoutjs"></a>Tworzenie dynamicznego interfejsu użytkownika z użyciem biblioteki Knockout.js

W tej sekcji będziemy używać odcinania. js do dodawania funkcji do widoku administratora.

[Separowanie. js](http://knockoutjs.com/) to biblioteka języka JavaScript, która ułatwia powiązanie formantów HTML z danymi. Separowanie. js używa wzorca Model-View-ViewModel (MVVM).

- *Model* jest reprezentacją po stronie serwera danych w domenie biznesowej (w naszym przypadku, produkty i zamówienia).
- *Widok* jest warstwą prezentacji (html).
- *Model widoku* to obiekt JavaScript, który zawiera dane modelu. Model widoku jest abstrakcją kodu interfejsu użytkownika. Nie ma informacji o reprezentacji HTML. Zamiast tego reprezentuje funkcje abstrakcyjne widoku, takie jak "Lista elementów".

Widok jest powiązany z danymi z modelem widoku. Aktualizacje widoku-model są automatycznie odzwierciedlane w widoku. Model widoku również pobiera zdarzenia z widoku, takie jak kliknięcie przycisku i wykonuje operacje na modelu, takie jak tworzenie zamówienia.

![](using-web-api-with-entity-framework-part-5/_static/image1.png)

Najpierw zdefiniujemy model widoku. Następnie powiążemy znacznik HTML z modelem widoku.

Dodaj następującą sekcję Razor do admin. cshtml:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample1.cshtml)]

Tę sekcję można dodać w dowolnym miejscu w pliku. Gdy widok jest renderowany, sekcja pojawia się u dołu strony HTML bezpośrednio przed tagiem zamykającym &lt;/Body&gt;.

Cały skrypt dla tej strony zostanie umieszczony wewnątrz tagu skryptu wskazywanym przez komentarz:

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample2.html)]

Najpierw Zdefiniuj klasę widoku-model:

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample3.js)]

**ko. observableArray** jest specjalnym rodzajem obiektu w odróżnieniu, nazywanym *zauważalnością*. Z [dokumentacji odcinania. js](http://knockoutjs.com/documentation/observables.html): zauważalny jest "obiekt JavaScript, który może powiadamiać subskrybentów o zmianach". Gdy zawartość zauważalnej zmiany, widok zostanie automatycznie zaktualizowany do dopasowania.

Aby wypełnić tablicę `products`, Utwórz żądanie AJAX do internetowego interfejsu API. Odwołaj, że został zapisany podstawowy identyfikator URI dla interfejsu API w worku widoku (zobacz [część 4](using-web-api-with-entity-framework-part-4.md) samouczka).

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample4.js?highlight=5)]

Następnie Dodaj funkcje do modelu widoku, aby tworzyć, aktualizować i usuwać produkty. Te funkcje przesyłają wywołania AJAX do internetowego interfejsu API i używają wyników do aktualizowania modelu widoku.

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample5.js?highlight=7)]

Teraz najważniejszym elementem: gdy DOM jest zapełniony, wywołaj funkcję **ko. applyBindings** i przekaż nowe wystąpienie `ProductsViewModel`:

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample6.js)]

Metoda **ko. applyBindings** umożliwia aktywowanie odcinania i nacinanie modelu widoku do widoku.

Teraz, gdy mamy już model widoku, możemy utworzyć powiązania. W pliku separowania. js można to zrobić, dodając atrybuty `data-bind` do elementów HTML. Na przykład, aby powiązać listę HTML z tablicą, użyj powiązania `foreach`:

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample7.html?highlight=1)]

Powiązanie `foreach` wykonuje iterację przez tablicę i tworzy elementy podrzędne dla każdego obiektu w tablicy. Powiązania elementów podrzędnych mogą odwoływać się do właściwości obiektów array.

Dodaj następujące powiązania do listy "aktualizacje produktów":

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample8.html)]

Element `<li>` występuje w zakresie powiązania **foreach** . Oznacza to, że odcinanie będzie renderować element jeden raz dla każdego produktu w tablicy `products`. Wszystkie powiązania w ramach elementu `<li>` odwołują się do tego wystąpienia produktu. Na przykład `$data.Name` odwołuje się do właściwości `Name` produktu.

Aby ustawić wartości danych wejściowych tekstu, użyj powiązania `value`. Przyciski są powiązane z funkcjami w widoku model, przy użyciu powiązania `click`. Wystąpienie produktu jest przesyłane jako parametr do każdej funkcji. Aby uzyskać więcej informacji, [Dokumentacja dotycząca odcinania. js](http://knockoutjs.com/documentation/observables.html) ma dobre opisy różnych powiązań.

Następnie Dodaj powiązanie dla zdarzenia **Prześlij** na formularzu Dodaj produkt:

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample9.html)]

To powiązanie wywołuje funkcję `create` w modelu widoku, aby utworzyć nowy produkt.

Oto pełen kod widoku administratora:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample10.cshtml)]

Uruchom aplikację, zaloguj się przy użyciu konta administratora, a następnie kliknij link "Administrator". Powinna zostać wyświetlona lista produktów i będzie można tworzyć, aktualizować lub usuwać produkty.

> [!div class="step-by-step"]
> [Poprzednie](using-web-api-with-entity-framework-part-4.md)
> [dalej](using-web-api-with-entity-framework-part-6.md)

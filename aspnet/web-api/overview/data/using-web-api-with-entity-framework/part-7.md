---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-7
title: Tworzenie widoku (UI) | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: b2445062-a1fe-4133-8994-f510280f6d9a
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-7
msc.type: authoredcontent
ms.openlocfilehash: 62c4523c2c6fb399cfbc3716309a1379996d601c
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/17/2019
ms.locfileid: "59408241"
---
# <a name="create-the-view-ui"></a>Tworzenie widoku (interfejs użytkownika)

przez [Mike Wasson](https://github.com/MikeWasson)

[Pobierz ukończony projekt](https://github.com/MikeWasson/BookService)

W tej sekcji powoduje rozpoczęcie zdefiniować kod HTML dla aplikacji, a następnie dodać powiązanie danych pomiędzy HTML a model widoku.

Otwórz plik Views/Home/Index.cshtml. Zamień całą zawartość tego pliku poniżej.

[!code-cshtml[Main](part-7/samples/sample1.cshtml)]

Większość `div` elementy są [Bootstrap](http://getbootstrap.com/) style. Ważne elementy są tymi, które z `data-bind` atrybutów. Ten atrybut łączy HTML w modelu widoku.

Na przykład:

[!code-html[Main](part-7/samples/sample2.html)]

W tym przykładzie &quot; `text` &quot; powoduje, że powiązanie `<p>` elementu, aby wyświetlić wartość `error` właściwości z modelu widoku. Pamiętamy `error` został zadeklarowany jako `ko.observable`:

[!code-javascript[Main](part-7/samples/sample3.js)]

Zawsze, gdy nowa wartość jest przypisany do `error`, Knockout aktualizuje tekstu w `<p>` elementu.

`foreach` Powiązania informuje Knockout pętli zawartość `books` tablicy. Dla każdego elementu w tablicy, tworzy nową Knockout &lt;li&gt; elementu. Powiązania w kontekście `foreach` odnoszą się do właściwości dla elementu tablicy. Na przykład:

[!code-html[Main](part-7/samples/sample4.html)]

W tym miejscu `text` powiązania odczytuje właściwość Autor poszczególne książki.

Jeśli uruchomisz aplikację teraz, jego powinien wyglądać następująco:

![](part-7/_static/image1.png)

Listy książek ładuje asynchronicznie, po załadowaniu strony. Od razu &quot;szczegóły&quot; nie działają łącza. Ta funkcja zostanie dodany w następnej sekcji.

> [!div class="step-by-step"]
> [Poprzednie](part-6.md)
> [dalej](part-8.md)

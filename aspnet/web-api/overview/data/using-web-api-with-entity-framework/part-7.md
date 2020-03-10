---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-7
title: Utwórz widok (interfejs użytkownika) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: b2445062-a1fe-4133-8994-f510280f6d9a
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-7
msc.type: authoredcontent
ms.openlocfilehash: 62c4523c2c6fb399cfbc3716309a1379996d601c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557303"
---
# <a name="create-the-view-ui"></a>Tworzenie widoku (interfejs użytkownika)

według [Jan Wasson](https://github.com/MikeWasson)

[Pobierz ukończony projekt](https://github.com/MikeWasson/BookService)

W tej sekcji rozpocznie się Definiowanie kodu HTML dla aplikacji i Dodawanie powiązania danych między kodem HTML i modelem widoku.

Otwórz plik widoki/Home/index. cshtml. Zastąp całą zawartość tego pliku następującym.

[!code-cshtml[Main](part-7/samples/sample1.cshtml)]

Większość `div` elementów ma dla stylów [ładowania początkowego](http://getbootstrap.com/) . Ważne elementy są tymi, których atrybuty `data-bind`. Ten atrybut łączy kod HTML z modelem widoku.

Na przykład:

[!code-html[Main](part-7/samples/sample2.html)]

W tym przykładzie &quot;`text`powiązania &quot; powoduje, że element `<p>` wyświetla wartość właściwości `error` z modelu widoku. Odwołaj ten `error` został zadeklarowany jako `ko.observable`:

[!code-javascript[Main](part-7/samples/sample3.js)]

Za każdym razem, gdy nowa wartość jest przypisana do `error`, odcinanie aktualizuje tekst w elemencie `<p>`.

Powiązanie `foreach` informuje odcinanie pętli przez zawartość tablicy `books`. Dla każdego elementu w tablicy odcinanie tworzy nowy element &lt;li&gt;. Powiązania wewnątrz kontekstu `foreach` odnoszą się do właściwości elementu tablicy. Na przykład:

[!code-html[Main](part-7/samples/sample4.html)]

W tym miejscu powiązanie `text` odczytuje właściwość Autor każdej książki.

Jeśli aplikacja zostanie uruchomiona teraz, powinna wyglądać następująco:

![](part-7/_static/image1.png)

Lista książek ładuje się asynchronicznie, po załadowaniu strony. Teraz linki &quot;szczegóły&quot; nie działają. Ta funkcja zostanie dodana w następnej sekcji.

> [!div class="step-by-step"]
> [Poprzednie](part-6.md)
> [dalej](part-8.md)

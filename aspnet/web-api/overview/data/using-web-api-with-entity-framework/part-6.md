---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-6
title: Tworzenie klienta JavaScript | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 20360326-b123-4b1e-abae-1d350edf4ce4
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-6
msc.type: authoredcontent
ms.openlocfilehash: 74f2cc4e5e401d690042b05b028dfc0c46ae282a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622347"
---
# <a name="create-the-javascript-client"></a>Tworzenie klienta JavaScript

według [Jan Wasson](https://github.com/MikeWasson)

[Pobierz ukończony projekt](https://github.com/MikeWasson/BookService)

W tej sekcji utworzysz klienta dla aplikacji, używając języka HTML, JavaScript i biblioteki [separowania. js](http://knockoutjs.com/) . Utworzymy aplikację kliencką na etapach:

- Wyświetlanie listy książek.
- Wyświetlanie szczegółów książki.
- Dodawanie nowej książki.

Biblioteka odcinania używa wzorca Model-View-ViewModel (MVVM):

- **Model** jest reprezentacją po stronie serwera danych w domenie biznesowej (w naszym przypadku, książki i autorzy).
- **Widok** jest warstwą prezentacji (html).
- **Model widoku** to obiekt JavaScript, który zawiera modele. Model widoku jest abstrakcją kodu interfejsu użytkownika. Nie ma informacji o reprezentacji HTML. Zamiast tego reprezentuje funkcje abstrakcyjne widoku, takie jak &quot;listy książek&quot;.

Widok jest powiązany z danymi z modelem widoku. Aktualizacje modelu widoku są automatycznie odzwierciedlane w widoku. Model widoku pobiera również zdarzenia z widoku, takie jak kliknięcia przycisku.

![](part-6/_static/image1.png)

Takie podejście ułatwia zmianę układu i interfejsu użytkownika aplikacji, ponieważ można zmienić powiązania, bez konieczności ponownego pisania kodu. Na przykład można wyświetlić listę elementów jako `<ul>`, a następnie zmienić ją później na tabelę.

## <a name="add-the-knockout-library"></a>Dodawanie biblioteki odcinania

W programie Visual Studio w menu **Narzędzia** wybierz pozycję **Menedżer pakietów NuGet**. Następnie wybierz pozycję **konsola Menedżera pakietów**. W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:

[!code-console[Main](part-6/samples/sample1.cmd)]

To polecenie dodaje pliki odcinania do folderu skrypty.

## <a name="create-the-view-model"></a>Tworzenie modelu widoku

Dodaj plik języka JavaScript o nazwie App. js do folderu Scripts. (W Eksplorator rozwiązań kliknij prawym przyciskiem myszy folder skrypty, wybierz polecenie **Dodaj**, a następnie wybierz pozycję **plik JavaScript**). Wklej następujący kod:

[!code-javascript[Main](part-6/samples/sample2.js)]

W odcinania Klasa `observable` włącza powiązanie danych. Gdy zawartość zauważalnej zmiany, zauważalnie powiadamia wszystkie kontrolki powiązane z danymi, aby mogli ją zaktualizować. (Klasa `observableArray` to wersja tablicy, która jest *zauważalna*). Aby zacząć od, nasz model widoku ma dwa observables:

- `books` zawiera listę książek.
- `error` zawiera komunikat o błędzie, jeśli wywołanie AJAX zakończy się niepowodzeniem.

Metoda `getAllBooks` umożliwia wywołanie AJAX w celu uzyskania listy książek. Następnie wypycha wynik do tablicy `books`.

Metoda `ko.applyBindings` jest częścią biblioteki odcinania. Przyjmuje model widoku jako parametr i konfiguruje powiązanie danych.

## <a name="add-a-script-bundle"></a>Dodawanie pakietu skryptu

Zgrupowanie to funkcja w programie ASP.NET 4,5, która ułatwia łączenie wielu plików w jeden plik i tworzenie ich. Zgrupowanie zmniejsza liczbę żądań do serwera, co może poprawić czas ładowania strony.

Otwórz aplikację pliku\_Start/BundleConfig. cs. Dodaj następujący kod do metody RegisterBundles.

[!code-csharp[Main](part-6/samples/sample3.cs)]

> [!div class="step-by-step"]
> [Poprzednie](part-5.md)
> [dalej](part-7.md)

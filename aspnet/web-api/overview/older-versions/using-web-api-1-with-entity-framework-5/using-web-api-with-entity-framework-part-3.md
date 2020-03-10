---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
title: 'Część 3: Tworzenie kontrolera administratora | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 6b9ae3c4-0274-4170-a1bb-9df9c546b2a9
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
msc.type: authoredcontent
ms.openlocfilehash: f39be7a84e85db93487d246e9f8cb59c401fe5ce
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556050"
---
# <a name="part-3-creating-an-admin-controller"></a>Część 3: Tworzenie kontrolera administratora

według [Jan Wasson](https://github.com/MikeWasson)

[Pobierz ukończony projekt](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-controller"></a>Dodawanie kontrolera administratora

W tej sekcji dodamy kontroler interfejsu API sieci Web, który obsługuje operacje CRUD (tworzenie, odczytywanie, aktualizowanie i usuwanie) dla produktów. Kontroler będzie używać Entity Framework do komunikowania się z warstwą bazy danych. Tylko administratorzy będą mogli korzystać z tego kontrolera. Klienci będą uzyskiwać dostęp do produktów za pomocą innego kontrolera.

W Eksplorator rozwiązań kliknij prawym przyciskiem myszy folder controllers. Wybierz opcję **Dodaj** , a następnie pozycję **kontroler**.

![](using-web-api-with-entity-framework-part-3/_static/image1.png)

W oknie dialogowym **Dodawanie kontrolera** Nadaj nazwę kontrolerowi `AdminController`. W obszarze **szablon**wybierz pozycję &quot;kontroler interfejsu API z akcjami odczytu/zapisu, używając Entity Framework&quot;. W obszarze **Klasa modelu**wybierz pozycję "produkt (ProductStore. models)". W obszarze **kontekst danych**wybierz opcję "&lt;nowym&gt;kontekstu danych".

![](using-web-api-with-entity-framework-part-3/_static/image2.png)

> [!NOTE]
> Jeśli lista rozwijana **Klasa modelu** nie zawiera żadnych klas modelu, należy upewnić się, że projekt został skompilowany. Entity Framework używa odbicia, dlatego wymaga skompilowanego zestawu.

Wybranie opcji "&lt;nowego kontekstu danych&gt;" spowoduje otwarcie okna dialogowego **nowego kontekstu danych** . Nazwij `ProductStore.Models.OrdersContext`kontekstu danych.

![](using-web-api-with-entity-framework-part-3/_static/image3.png)

Kliknij przycisk **OK** , aby odrzucić okno dialogowe **nowego kontekstu danych** . W oknie dialogowym **Dodawanie kontrolera** kliknij przycisk **Dodaj**.

Oto, co zostało dodane do projektu:

- Klasa o nazwie `OrdersContext`, która pochodzi od **DbContext**. Ta klasa zapewnia Glue między modelami POCO i bazą danych.
- Kontroler interfejsu API sieci Web o nazwie `AdminController`. Ten kontroler obsługuje operacje CRUD na wystąpieniach `Product`. Używa klasy `OrdersContext` do komunikowania się z Entity Framework.
- Nowe parametry połączenia z bazą danych w pliku Web. config.

![](using-web-api-with-entity-framework-part-3/_static/image4.png)

Otwórz plik OrdersContext.cs. Zauważ, że Konstruktor Określa nazwę parametrów połączenia bazy danych. Ta nazwa odwołuje się do parametrów połączenia, które zostały dodane do pliku Web. config.

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample1.cs)]

Dodaj następujące właściwości do klasy `OrdersContext`:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample2.cs)]

**Nieogólnymi** reprezentuje zestaw jednostek, które mogą być zapytania. Oto kompletna lista dla klasy `OrdersContext`:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample3.cs)]

Klasa `AdminController` definiuje pięć metod implementujących podstawowe funkcje CRUD. Każda metoda odpowiada identyfikatorowi URI, który może być wywoływany przez klienta:

| Controller — Metoda | Opis | Identyfikator URI | Metoda HTTP |
| --- | --- | --- | --- |
| GetProducts | Pobiera wszystkie produkty. | interfejsy API/produkty | GET |
| Getproduct | Znajduje produkt według identyfikatora. | Interfejs API/produkty/*Identyfikator* | GET |
| PutProduct | Aktualizuje produkt. | Interfejs API/produkty/*Identyfikator* | PUT |
| PostProduct | Tworzy nowy produkt. | interfejsy API/produkty | POST |
| DeleteProduct | Usuwa produkt. | Interfejs API/produkty/*Identyfikator* | DELETE |

Każda metoda wywołuje do `OrdersContext`, aby wykonać zapytanie względem bazy danych. Metody modyfikujące wywołanie kolekcji (PUT, POST i DELETE) `db.SaveChanges`, aby zachować zmiany w bazie danych. Kontrolery są tworzone na żądanie HTTP, a następnie usuwane, dlatego należy zachować zmiany przed zwróceniem metody.

## <a name="add-a-database-initializer"></a>Dodawanie inicjatora bazy danych

Entity Framework ma świetną funkcję, która umożliwia wypełnienie bazy danych przy uruchamianiu i automatyczne odtworzenie bazy danych przy każdym zmianie modeli. Ta funkcja jest przydatna podczas opracowywania, ponieważ zawsze masz pewne dane testowe, nawet jeśli zmienisz modele.

W Eksplorator rozwiązań kliknij prawym przyciskiem myszy folder modele i Utwórz nową klasę o nazwie `OrdersContextInitializer`. Wklej w następującej implementacji:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample4.cs)]

Przez dziedziczenie z klasy **DropCreateDatabaseIfModelChanges** , poinformujemy Entity Framework o porzucenia bazy danych przy każdej modyfikacji klas modelu. Gdy Entity Framework tworzy (lub ponownie tworzy) bazę danych, wywołuje metodę **inicjatora** , aby wypełnić tabele. Używamy metody **inicjatora** , aby dodać przykładowe produkty i przykładową kolejność.

Ta funkcja doskonale nadaje się do testowania, ale nie używa klasy **DropCreateDatabaseIfModelChanges** w środowisku produkcyjnym, ponieważ można utracić dane, jeśli ktoś zmieni klasę modelu.

Następnie otwórz Global. asax i Dodaj następujący kod do **aplikacji\_Start** :

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample5.cs)]

## <a name="send-a-request-to-the-controller"></a>Wyślij żądanie do kontrolera

W tym momencie nie zapisano kodu klienta, ale można wywołać internetowy interfejs API przy użyciu przeglądarki sieci Web lub narzędzia debugowania HTTP, takiego jak [programu Fiddler](http://www.fiddler2.com/fiddler2/). W programie Visual Studio naciśnij klawisz F5, aby rozpocząć debugowanie. Przeglądarka sieci Web zostanie otwarta w `http://localhost:*portnum*/`, gdzie *portnum* jest numerem portu.

Wyślij żądanie HTTP do "`http://localhost:*portnum*/api/admin`. Pierwsze żądanie może być wykonane wolno, ponieważ Entity Framework musi utworzyć i wypełniać bazę danych. Odpowiedź powinna wyglądać podobnie do poniższego:

[!code-console[Main](using-web-api-with-entity-framework-part-3/samples/sample6.cmd)]

> [!div class="step-by-step"]
> [Poprzednie](using-web-api-with-entity-framework-part-2.md)
> [dalej](using-web-api-with-entity-framework-part-4.md)

---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-2
title: Dodawanie modeli i kontrolerów | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 88908ff8-51a9-40eb-931c-a8139128b680
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-2
msc.type: authoredcontent
ms.openlocfilehash: 57dacda421968f341284d89c9a3ad80040c16e25
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557541"
---
# <a name="add-models-and-controllers"></a>Dodawanie modeli i kontrolerów

według [Jan Wasson](https://github.com/MikeWasson)

[Pobierz ukończony projekt](https://github.com/MikeWasson/BookService)

W tej sekcji dodasz klasy modelu, które definiują jednostki bazy danych. Następnie dodasz Kontrolery interfejsu API sieci Web, które wykonują operacje CRUD na tych jednostkach.

## <a name="add-model-classes"></a>Dodaj klasy modelu

W tym samouczku utworzymy bazę danych przy użyciu podejścia "Code First" do Entity Framework (EF). Za pomocą Code First można pisać C# klasy, które odpowiadają tabelom bazy danych, a EF tworzy bazę danych. (Aby uzyskać więcej informacji, zobacz [Entity Framework podejścia programistyczne](https://msdn.microsoft.com/library/ms178359%28v=vs.110%29.aspx#dbfmfcf).)

Zaczynamy od definiowania obiektów domeny jako POCOs (zwykłych obiektów CLR). Utworzymy następujący POCOs:

- Autor
- Book (Książka)

W Eksplorator rozwiązań kliknij prawym przyciskiem myszy folder modele. Wybierz pozycję **Dodaj**, a następnie wybierz pozycję **Klasa**. Nadaj klasie nazwę `Author`.

![](part-2/_static/image1.png)

Zastąp cały kod standardowy w Author.cs następującym kodem.

[!code-csharp[Main](part-2/samples/sample1.cs)]

Dodaj inną klasę o nazwie `Book`, używając poniższego kodu.

[!code-csharp[Main](part-2/samples/sample2.cs)]

Entity Framework będą używać tych modeli do tworzenia tabel bazy danych. Dla każdego modelu Właściwość `Id` stanie się kolumną klucza podstawowego w tabeli bazy danych.

W klasie Book `AuthorId` definiuje klucz obcy do tabeli `Author`. (Dla uproszczenia przyjęto, że każda książka ma jednego autora). Klasa Book zawiera również właściwość nawigacji do powiązanej `Author`. Możesz użyć właściwości nawigacji, aby uzyskać dostęp do powiązanego `Author` w kodzie. Dowiesz się więcej o właściwościach nawigacji w części 4, [Obsługa relacji jednostek](part-4.md).

## <a name="add-web-api-controllers"></a>Dodawanie kontrolerów interfejsu API sieci Web

W tej sekcji dodamy Kontrolery interfejsu API sieci Web, które obsługują operacje CRUD (tworzenie, odczytywanie, aktualizowanie i usuwanie). Kontrolery będą używać Entity Framework do komunikowania się z warstwą bazy danych.

Najpierw można usunąć kontrolery plików/ValuesController. cs. Ten plik zawiera przykładowy kontroler internetowego interfejsu API, ale nie jest potrzebny do tego samouczka.

![](part-2/_static/image2.png)

Następnie Skompiluj projekt. Tworzenie szkieletu interfejsu API sieci Web używa odbicia w celu znalezienia klas modelu, więc wymaga skompilowanego zestawu.

W Eksplorator rozwiązań kliknij prawym przyciskiem myszy folder controllers. Wybierz pozycję **Dodaj**, a następnie pozycję **kontroler**.

![](part-2/_static/image3.png)

W oknie dialogowym **Dodawanie szkieletu** wybierz opcję "kontroler Web API 2 z akcjami przy użyciu Entity Framework". Kliknij pozycję **Add** (Dodaj).

![](part-2/_static/image4.png)

W oknie dialogowym **Dodawanie kontrolera** wykonaj następujące czynności:

1. Z listy rozwijanej **Klasa modelu** wybierz klasę `Author`. (Jeśli nie widzisz go na liście rozwijanej, upewnij się, że został skompilowany projekt).
2. Zaznacz opcję "Użyj akcji kontrolera asynchronicznego".
3. Pozostaw nazwę kontrolera jako &quot;AuthorsController&quot;.
4. Kliknij przycisk Plus (+) obok **klasy kontekstu danych**.

![](part-2/_static/image5.png)

W oknie dialogowym **nowy kontekst danych** pozostaw nazwę domyślną, a następnie kliknij przycisk **Dodaj**.

![](part-2/_static/image6.png)

Kliknij przycisk **Dodaj** , aby zakończyć okno dialogowe **Dodawanie kontrolera** . To okno dialogowe dodaje dwie klasy do projektu:

- `AuthorsController` definiuje kontroler interfejsu API sieci Web. Kontroler implementuje interfejs API REST używany przez klientów do wykonywania operacji CRUD na liście autorów.
- `BookServiceContext` zarządza obiektami jednostek w czasie wykonywania, co obejmuje wypełnianie obiektów danymi z bazy danych, śledzenie zmian i utrwalanie danych w bazie danych. Dziedziczy po `DbContext`.

![](part-2/_static/image7.png)

W tym momencie Skompiluj projekt ponownie. Teraz wykonaj te same kroki, aby dodać kontroler interfejsu API dla jednostek `Book`. Tym razem wybierz `Book` klasy model i wybierz istniejącą klasę `BookServiceContext` dla klasy kontekstu danych. (Nie twórz nowego kontekstu danych). Kliknij przycisk **Dodaj** , aby dodać kontroler.

![](part-2/_static/image8.png)

> [!div class="step-by-step"]
> [Poprzednie](part-1.md)
> [dalej](part-3.md)

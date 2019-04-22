---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-5
title: Tworzenie obiektów transferu danych (dto) | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 0fd07176-b74b-48f0-9fac-0f02e3ffa213
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-5
msc.type: authoredcontent
ms.openlocfilehash: 1af29955e8040c34840d4c77fc2006f59d2324dd
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/17/2019
ms.locfileid: "59395280"
---
# <a name="create-data-transfer-objects-dtos"></a>Tworzenie obiektów transferu danych (DTO)

przez [Mike Wasson](https://github.com/MikeWasson)

[Pobierz ukończony projekt](https://github.com/MikeWasson/BookService)

Po prawej stronie teraz nasz internetowy interfejs API jednostek bazy danych do klienta. Klient odbiera dane, które mapuje bezpośrednio na tabelach bazy danych. Jednakże, które nie zawsze jest dobrym pomysłem. Czasami trzeba zmienić kształt danych, który można wysłać do klienta. Na przykład możesz chcieć:

- Usuń odwołania cykliczne (zobacz poprzednią sekcję).
- Ukryj konkretnej właściwości, które klienci nie powinni wyświetlić.
- Pominięto niektóre właściwości, aby zmniejszyć rozmiar ładunku.
- Spłaszczanie wykresów obiektów, które zawierają obiektów zagnieżdżonych, aby były bardziej wygodne do klientów.
- Należy unikać "polegającymi" luki w zabezpieczeniach. (Zobacz [sprawdzania poprawności modelu](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md) dyskusję na temat nadmiernego ogłaszania.)
- Rozdzielenie swojej warstwy usług z warstwą bazy danych.

Aby to osiągnąć, można zdefiniować *obiekt transferu danych* (DTO). Obiekt DTO jest obiektem, który definiuje, jak dane zostaną wysłane za pośrednictwem sieci. Zobaczmy, jak to działa z jednostką książki. W folderze modeli należy dodać dwie klasy DTO:

[!code-csharp[Main](part-5/samples/sample1.cs)]

`BookDetailDTO` Klasa zawiera wszystkie właściwości modelu książki, chyba że `AuthorName` jest ciągiem, który będzie przechowywać imię i nazwisko autora. `BookDTO` Klasy zawiera podzbiór właściwości z `BookDetailDTO`.

Następnie zastąp te dwie metody GET w `BooksController` klasy z wersjami, które zwracają dto. Użyjemy LINQ **wybierz** instrukcję, aby konwertować z książki jednostek dto.

[!code-csharp[Main](part-5/samples/sample2.cs)]

Oto SQL generowane przez nowy `GetBooks` metody. Widać, że EF tłumaczy LINQ **wybierz** w instrukcji SQL SELECT.

[!code-sql[Main](part-5/samples/sample3.sql)]

Na koniec zmodyfikuj `PostBook` metodę, aby zwrócić obiekt DTO.

[!code-csharp[Main](part-5/samples/sample4.cs)]

> [!NOTE]
> W tym samouczku konwertujemy na dto ręcznie w kodzie. Innym rozwiązaniem jest korzystanie z biblioteki, takich jak [AutoMapper](http://automapper.org/) automatycznie obsługująca konwersji.
> 
> [!div class="step-by-step"]
> [Poprzednie](part-4.md)
> [dalej](part-6.md)

---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-5
title: Tworzenie Transfer danych obiektów (DTO) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 0fd07176-b74b-48f0-9fac-0f02e3ffa213
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-5
msc.type: authoredcontent
ms.openlocfilehash: fc0463420207eba764014b8ec7123c5150e38247
ms.sourcegitcommit: 84b1681d4e6253e30468c8df8a09fe03beea9309
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/02/2019
ms.locfileid: "73445764"
---
# <a name="create-data-transfer-objects-dtos"></a>Tworzenie obiektów transferu danych (DTO)

według [Jan Wasson](https://github.com/MikeWasson)

[Pobierz ukończony projekt](https://github.com/MikeWasson/BookService)

Teraz nasz interfejs API sieci Web uwidacznia jednostki bazy danych klientowi. Klient odbiera dane, które są mapowane bezpośrednio do tabel bazy danych. Jednak nie zawsze jest dobrym pomysłem. Czasami chcesz zmienić kształt danych wysyłanych na klienta. Na przykład możesz chcieć:

- Usuń odwołania cykliczne (zobacz poprzednią sekcję).
- Ukryj konkretne właściwości, które nie powinny być wyświetlane dla klientów.
- Pomiń niektóre właściwości, aby zmniejszyć rozmiar ładunku.
- Spłaszcz wykresy obiektów zawierające obiekty zagnieżdżone, aby zwiększyć ich wygodę dla klientów.
- Unikaj luk w zabezpieczeniach. (Zobacz [Walidacja modelu](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md) w przypadku dyskusji o nadmiernym księgowaniu).
- Oddziel warstwę usługi od warstwy bazy danych.

Aby to osiągnąć, można zdefiniować *obiekt transferu danych* (DTO). DTO jest obiektem, który definiuje sposób przesyłania danych przez sieć. Zobaczmy, jak działa z jednostką książki. W folderze modele Dodaj dwie klasy DTO:

[!code-csharp[Main](part-5/samples/sample1.cs)]

Klasa `BookDetailDto` zawiera wszystkie właściwości modelu książki, z tą różnicą, że `AuthorName` jest ciągiem, który będzie zawierać nazwę autora. Klasa `BookDto` zawiera podzbiór właściwości z `BookDetailDto`.

Następnie zastąp dwie metody GET w klasie `BooksController` z wersjami, które zwracają DTO. Użyjemy instrukcji **SELECT** języka LINQ do konwersji z jednostek książki na DTO.

[!code-csharp[Main](part-5/samples/sample2.cs)]

Oto kod SQL generowany przez nową metodę `GetBooks`. Można zobaczyć, że EF tłumaczy element LINQ **SELECT** na instrukcję SELECT języka SQL.

[!code-sql[Main](part-5/samples/sample3.sql)]

Na koniec zmodyfikuj metodę `PostBook`, aby zwracała DTO.

[!code-csharp[Main](part-5/samples/sample4.cs)]

> [!NOTE]
> W tym samouczku Konwertujemy się na DTO ręcznie w kodzie. Inną opcją jest użycie biblioteki, takiej jak [automapowanie](http://automapper.org/) , która obsługuje konwersję automatycznie.
> 
> [!div class="step-by-step"]
> [Poprzedni](part-4.md)
> [Następny](part-6.md)

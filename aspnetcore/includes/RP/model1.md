---
ms.openlocfilehash: 934db70fe49ba9a5330b25596dc694e0ac50c04e
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57069995"
---
Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

W tej sekcji dodasz klasy zarządzania filmów w bazie danych. Użyj tych klas z [Entity Framework Core](/ef/core) (EF Core) do pracy z bazą danych. EF Core to platforma mapowania obiektowo relacyjny (ORM), która upraszcza kod dostępu do danych, który trzeba napisać.

Klasy modeli, które możesz utworzyć są znane jako klasy POCO (od "old zwykły CLR obiekty"), ponieważ nie mają żadnych zależności programu EF Core. Mogą określać właściwości danych, które są przechowywane w bazie danych.

W tym samouczku pisania klasy modeli i programem EF Core tworzy bazę danych. Alternatywnym podejściu, nieuwzględnione w tym miejscu jest [wygenerowane klasy modelu z istniejącej bazy danych](/ef/core/get-started/aspnetcore/existing-db).

[Wyświetlanie lub pobieranie](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) próbki.

---
ms.openlocfilehash: 2481b10abae9aefce2d5173d8071e4c2236db928
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57073109"
---

> [!NOTE]
> Wiele operacji zmiany schematu nie są obsługiwane przez dostawcę programu EF Core bazy danych SQLite. Na przykład dodawanie kolumny jest obsługiwane, ale usuwanie kolumny nie jest obsługiwane. Jeśli migracji została utworzona, aby usunąć kolumnę, `ef migrations add` polecenie zakończy się pomyślnie, ale `ef database update` polecenie kończy się niepowodzeniem. Ręczne pisanie kodu migracji przeprowadzić odbudowania indeksu tabeli można rozwiązać niektóre z tych ograniczeń. Wymaga odbudowania indeksu tabeli:

>* Zmiana nazwy istniejącej tabeli.
>* Trwa tworzenie nowej tabeli.
>* Kopiowanie danych z tabeli starej do nowej tabeli.
>* Usunięcie starych tabeli.

Aby uzyskać więcej informacji, zobacz następujące zasoby:
> * [SQLite EF Core Database Provider Limitations](/ef/core/providers/sqlite/limitations)
> * [Dostosowywanie kodu migracji](/ef/core/managing-schemas/migrations/#customize-migration-code)
> * [Wstępne wypełnianie danych](/ef/core/modeling/data-seeding)
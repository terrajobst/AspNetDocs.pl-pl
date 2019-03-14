---
ms.openlocfilehash: 2481b10abae9aefce2d5173d8071e4c2236db928
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57073109"
---

> [!NOTE]
> <span data-ttu-id="e3ded-101">Wiele operacji zmiany schematu nie są obsługiwane przez dostawcę programu EF Core bazy danych SQLite.</span><span class="sxs-lookup"><span data-stu-id="e3ded-101">Many schema change operations are not supported by the EF Core SQLite provider.</span></span> <span data-ttu-id="e3ded-102">Na przykład dodawanie kolumny jest obsługiwane, ale usuwanie kolumny nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="e3ded-102">For example, adding a column is supported, but removing a column is not supported.</span></span> <span data-ttu-id="e3ded-103">Jeśli migracji została utworzona, aby usunąć kolumnę, `ef migrations add` polecenie zakończy się pomyślnie, ale `ef database update` polecenie kończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="e3ded-103">If a migration is created to remove a column, the `ef migrations add` command succeeds but the `ef database update` command fails.</span></span> <span data-ttu-id="e3ded-104">Ręczne pisanie kodu migracji przeprowadzić odbudowania indeksu tabeli można rozwiązać niektóre z tych ograniczeń.</span><span class="sxs-lookup"><span data-stu-id="e3ded-104">Some of these limitations can be overcome by manually writing migrations code to perform a table rebuild.</span></span> <span data-ttu-id="e3ded-105">Wymaga odbudowania indeksu tabeli:</span><span class="sxs-lookup"><span data-stu-id="e3ded-105">A table rebuild involves:</span></span>

>* <span data-ttu-id="e3ded-106">Zmiana nazwy istniejącej tabeli.</span><span class="sxs-lookup"><span data-stu-id="e3ded-106">Renaming the existing table.</span></span>
>* <span data-ttu-id="e3ded-107">Trwa tworzenie nowej tabeli.</span><span class="sxs-lookup"><span data-stu-id="e3ded-107">Creating a new table.</span></span>
>* <span data-ttu-id="e3ded-108">Kopiowanie danych z tabeli starej do nowej tabeli.</span><span class="sxs-lookup"><span data-stu-id="e3ded-108">Copying data from the old table to the new table.</span></span>
>* <span data-ttu-id="e3ded-109">Usunięcie starych tabeli.</span><span class="sxs-lookup"><span data-stu-id="e3ded-109">Dropping the old table.</span></span>

<span data-ttu-id="e3ded-110">Aby uzyskać więcej informacji, zobacz następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="e3ded-110">For more information, see the following resources:</span></span>
> * [<span data-ttu-id="e3ded-111">SQLite EF Core Database Provider Limitations</span><span class="sxs-lookup"><span data-stu-id="e3ded-111">SQLite EF Core Database Provider Limitations</span></span>](/ef/core/providers/sqlite/limitations)
> * [<span data-ttu-id="e3ded-112">Dostosowywanie kodu migracji</span><span class="sxs-lookup"><span data-stu-id="e3ded-112">Customize migration code</span></span>](/ef/core/managing-schemas/migrations/#customize-migration-code)
> * [<span data-ttu-id="e3ded-113">Wstępne wypełnianie danych</span><span class="sxs-lookup"><span data-stu-id="e3ded-113">Data seeding</span></span>](/ef/core/modeling/data-seeding)
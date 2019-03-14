---
ms.openlocfilehash: 4d6b6309e6e15c364542b5d3ae296d53c6ea4358
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57069839"
---
<span data-ttu-id="59c1c-101">Wygenerowany kod tożsamości w bazie danych wymaga [migracje Entity Framework Core](/ef/core/managing-schemas/migrations/).</span><span class="sxs-lookup"><span data-stu-id="59c1c-101">The generated Identity database code requires [Entity Framework Core Migrations](/ef/core/managing-schemas/migrations/).</span></span> <span data-ttu-id="59c1c-102">Utwórz migracji i aktualizują bazę danych.</span><span class="sxs-lookup"><span data-stu-id="59c1c-102">Create a migration and update the database.</span></span> <span data-ttu-id="59c1c-103">Na przykład uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="59c1c-103">For example, run the following commands:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="59c1c-104">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="59c1c-104">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="59c1c-105">W programie Visual Studio **Konsola Menedżera pakietów**:</span><span class="sxs-lookup"><span data-stu-id="59c1c-105">In the Visual Studio **Package Manager Console**:</span></span>

```PMC
Add-Migration CreateIdentitySchema
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="59c1c-106">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="59c1c-106">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
```

------

<span data-ttu-id="59c1c-107">Parametr name "CreateIdentitySchema" `Add-Migration` polecenia jest określana.</span><span class="sxs-lookup"><span data-stu-id="59c1c-107">The "CreateIdentitySchema" name parameter for the `Add-Migration` command is arbitrary.</span></span> <span data-ttu-id="59c1c-108">`"CreateIdentitySchema"` w tym artykule opisano migracji.</span><span class="sxs-lookup"><span data-stu-id="59c1c-108">`"CreateIdentitySchema"` describes the migration.</span></span>

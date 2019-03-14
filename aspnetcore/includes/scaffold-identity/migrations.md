---
ms.openlocfilehash: 4d6b6309e6e15c364542b5d3ae296d53c6ea4358
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57069839"
---
Wygenerowany kod tożsamości w bazie danych wymaga [migracje Entity Framework Core](/ef/core/managing-schemas/migrations/). Utwórz migracji i aktualizują bazę danych. Na przykład uruchom następujące polecenia:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

W programie Visual Studio **Konsola Menedżera pakietów**:

```PMC
Add-Migration CreateIdentitySchema
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```cli
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
```

------

Parametr name "CreateIdentitySchema" `Add-Migration` polecenia jest określana. `"CreateIdentitySchema"` w tym artykule opisano migracji.

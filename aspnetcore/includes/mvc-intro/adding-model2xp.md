---
ms.openlocfilehash: 0084d8c6bc5d16eaa1c3fa36d8aabb2aa31e1283
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57067454"
---
<a name="cli"></a>
## <a name="perform-initial-migration"></a>Przeprowadzenie początkowej migracji

W wierszu polecenia Uruchom następujące polecenia interfejsu wiersza polecenia platformy .NET Core:

```console
dotnet ef migrations add InitialCreate
dotnet ef database update
```

`dotnet ef migrations add InitialCreate` Polecenie generuje kod, aby utworzyć schemat początkowej bazy danych. Schemat jest oparta na modelu, określone w `DbContext` (w *Models/MvcMovieContext.cs* pliku). `Initial` Argument jest używany do nazywania migracje. Można użyć dowolnej nazwy, ale zgodnie z Konwencją wybierz nazwę, która opisuje migracji. Zobacz [wprowadzenie do migracji](xref:data/ef-mvc/migrations#introduction-to-migrations) Aby uzyskać więcej informacji.

`dotnet ef database update` Polecenia `Up` method in Class metoda *migracje /\<sygnatura czasowa > _InitialCreate.cs* pliku, który tworzy bazę danych.

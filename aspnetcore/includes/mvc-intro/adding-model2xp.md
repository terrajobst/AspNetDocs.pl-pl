---
ms.openlocfilehash: 0084d8c6bc5d16eaa1c3fa36d8aabb2aa31e1283
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57067454"
---
<a name="cli"></a>
## <a name="perform-initial-migration"></a><span data-ttu-id="e158d-101">Przeprowadzenie początkowej migracji</span><span class="sxs-lookup"><span data-stu-id="e158d-101">Perform initial migration</span></span>

<span data-ttu-id="e158d-102">W wierszu polecenia Uruchom następujące polecenia interfejsu wiersza polecenia platformy .NET Core:</span><span class="sxs-lookup"><span data-stu-id="e158d-102">From the command line, run the following .NET Core CLI commands:</span></span>

```console
dotnet ef migrations add InitialCreate
dotnet ef database update
```

<span data-ttu-id="e158d-103">`dotnet ef migrations add InitialCreate` Polecenie generuje kod, aby utworzyć schemat początkowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="e158d-103">The `dotnet ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span> <span data-ttu-id="e158d-104">Schemat jest oparta na modelu, określone w `DbContext` (w *Models/MvcMovieContext.cs* pliku).</span><span class="sxs-lookup"><span data-stu-id="e158d-104">The schema is based on the model specified in the `DbContext` (In the *Models/MvcMovieContext.cs* file).</span></span> <span data-ttu-id="e158d-105">`Initial` Argument jest używany do nazywania migracje.</span><span class="sxs-lookup"><span data-stu-id="e158d-105">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="e158d-106">Można użyć dowolnej nazwy, ale zgodnie z Konwencją wybierz nazwę, która opisuje migracji.</span><span class="sxs-lookup"><span data-stu-id="e158d-106">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="e158d-107">Zobacz [wprowadzenie do migracji](xref:data/ef-mvc/migrations#introduction-to-migrations) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="e158d-107">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="e158d-108">`dotnet ef database update` Polecenia `Up` method in Class metoda *migracje /\<sygnatura czasowa > _InitialCreate.cs* pliku, który tworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="e158d-108">The `dotnet ef database update` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>

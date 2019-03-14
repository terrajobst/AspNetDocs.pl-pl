---
ms.openlocfilehash: f323482d6f8bfaebf7bf6673d5fb91608430760a
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57071867"
---
## <a name="add-initial-migration-and-update-the-database"></a><span data-ttu-id="b746c-101">Dodawanie początkowej migracji i aktualizowanie bazy danych</span><span class="sxs-lookup"><span data-stu-id="b746c-101">Add initial migration and update the database</span></span>

* <span data-ttu-id="b746c-102">Otwórz wiersz polecenia i przejdź do katalogu projektu.</span><span class="sxs-lookup"><span data-stu-id="b746c-102">Open a command prompt and navigate to the project directory.</span></span> <span data-ttu-id="b746c-103">(Katalogu zawierającego *Startup.cs* pliku).</span><span class="sxs-lookup"><span data-stu-id="b746c-103">(The directory containing the *Startup.cs* file).</span></span>

* <span data-ttu-id="b746c-104">W wierszu polecenia wpisz następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="b746c-104">Run the following commands in the command prompt:</span></span>

  ```console
  dotnet restore
  dotnet ef migrations add Initial
  dotnet ef database update
  ```
  
  <span data-ttu-id="b746c-105">[.NET core](/dotnet/core/tools/index) jest implementacją dla wielu platform .NET.</span><span class="sxs-lookup"><span data-stu-id="b746c-105">[.NET Core](/dotnet/core/tools/index) is a cross-platform implementation of .NET.</span></span> <span data-ttu-id="b746c-106">Oto, wykonaj następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="b746c-106">Here is what these commands do:</span></span>

  * <span data-ttu-id="b746c-107">[DotNet restore](/dotnet/core/tools/dotnet-restore): Pobiera pakiety NuGet, określone w *.csproj* pliku.</span><span class="sxs-lookup"><span data-stu-id="b746c-107">[dotnet restore](/dotnet/core/tools/dotnet-restore): Downloads the NuGet packages specified in the *.csproj* file.</span></span>
  * <span data-ttu-id="b746c-108">`dotnet ef migrations add Initial` Uruchamia polecenie migracje Entity Framework .NET Core interfejsu wiersza polecenia i tworzy początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="b746c-108">`dotnet ef migrations add Initial` Runs the Entity Framework .NET Core CLI migrations command and creates the initial migration.</span></span> <span data-ttu-id="b746c-109">Parametr po "add" to nazwa, który przypiszesz do migracji.</span><span class="sxs-lookup"><span data-stu-id="b746c-109">The parameter after "add" is a name that you assign to the migration.</span></span> <span data-ttu-id="b746c-110">W tym miejscu możesz teraz nazewnictwa migracji "Początkowego" ponieważ jest on początkowej migracji bazy danych.</span><span class="sxs-lookup"><span data-stu-id="b746c-110">Here you're naming the migration "Initial" because it's the initial database migration.</span></span> <span data-ttu-id="b746c-111">Ta operacja tworzy *danych/Migrations/\<daty / godziny > _Initial.cs* plik zawierający polecenia migracji, aby dodać *filmu* tabeli w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="b746c-111">This operation creates the *Data/Migrations/\<date-time>_Initial.cs* file containing the migration commands to add the *Movie* table to the database.</span></span>
  * <span data-ttu-id="b746c-112">`dotnet ef database update`  Aktualizuje bazę danych do migracji, którą właśnie utworzyliśmy.</span><span class="sxs-lookup"><span data-stu-id="b746c-112">`dotnet ef database update`  Updates the database with the migration we just created.</span></span>

<span data-ttu-id="b746c-113">W następnym samouczku dowiesz się o bazy danych i parametry połączenia.</span><span class="sxs-lookup"><span data-stu-id="b746c-113">You'll learn about the database and connection string in the next tutorial.</span></span> <span data-ttu-id="b746c-114">Dowiesz się o zmianach w modelu danych [dodać pole](xref:tutorials/first-mvc-app/new-field) samouczka.</span><span class="sxs-lookup"><span data-stu-id="b746c-114">You'll learn about data model changes in the [Add a field](xref:tutorials/first-mvc-app/new-field) tutorial.</span></span>

---
ms.openlocfilehash: 85fa9f8b18dc7dfb3e9d37703549dc5c28dc7a4a
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57077441"
---
<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="185b0-101">Tworzenie szkieletu modelu Movie</span><span class="sxs-lookup"><span data-stu-id="185b0-101">Scaffold the Movie model</span></span>

* <span data-ttu-id="185b0-102">Uruchom następujące polecenie w wierszu polecenia (w katalogu projektu, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików):</span><span class="sxs-lookup"><span data-stu-id="185b0-102">Run the following from the command line (in the project directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files):</span></span>

  ```console
  dotnet restore
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

<span data-ttu-id="185b0-103">Jeśli zostanie wyświetlony błąd:</span><span class="sxs-lookup"><span data-stu-id="185b0-103">If you get the error:</span></span>
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

<span data-ttu-id="185b0-104">Błąd poprzedzający występuje, gdy jesteś w niewłaściwej katalogu.</span><span class="sxs-lookup"><span data-stu-id="185b0-104">The preceeding error happens when you are in the wrong directory.</span></span> <span data-ttu-id="185b0-105">Otwórz powłokę wiersza polecenia do katalogu projektu (katalog, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików), a następnie uruchom polecenie poprzedzający.</span><span class="sxs-lookup"><span data-stu-id="185b0-105">Open a command shell to the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files), and then run the preceeding command.</span></span>

<span data-ttu-id="185b0-106">Jeśli zostanie wyświetlony błąd:</span><span class="sxs-lookup"><span data-stu-id="185b0-106">If you get the error:</span></span>
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

<span data-ttu-id="185b0-107">Zamknij program Visual Studio i ponownie uruchom polecenie.</span><span class="sxs-lookup"><span data-stu-id="185b0-107">Exit Visual Studio and run the command again.</span></span>

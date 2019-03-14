---
ms.openlocfilehash: cb9041a466309a400b19cdecd76f653499c6ac4b
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57074846"
---
<span data-ttu-id="ea935-101">Uruchom Generator szkieletu tożsamości:</span><span class="sxs-lookup"><span data-stu-id="ea935-101">Run the Identity scaffolder:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ea935-102">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ea935-102">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="ea935-103">Z **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy na Projekt > **Dodaj** > **nowy element szkieletu**.</span><span class="sxs-lookup"><span data-stu-id="ea935-103">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="ea935-104">W okienku po lewej stronie **Dodawanie szkieletu** okno dialogowe, wybierz opcję **tożsamości** > **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="ea935-104">From the left pane of the **Add Scaffold** dialog, select **Identity** > **ADD**.</span></span>
* <span data-ttu-id="ea935-105">W **tożsamość usługi ADD** okno dialogowe, wybierz odpowiednie opcje.</span><span class="sxs-lookup"><span data-stu-id="ea935-105">In the **ADD Identity** dialog, select the options you want.</span></span>
  * <span data-ttu-id="ea935-106">Wybierz istniejący stronę układu lub plik układu zostanie zastąpiony niepoprawny kod znaczników.</span><span class="sxs-lookup"><span data-stu-id="ea935-106">Select your existing layout page, or your layout file will be overwritten with incorrect markup.</span></span> <span data-ttu-id="ea935-107">Kiedy istniejący  *\_Layout.cshtml* plików jest zaznaczona, jest **nie** zastąpione.</span><span class="sxs-lookup"><span data-stu-id="ea935-107">When an existing *\_Layout.cshtml* file is selected, it is **not** overwritten.</span></span>

 <span data-ttu-id="ea935-108">Na przykład `~/Pages/Shared/_Layout.cshtml` dla stron Razor `~/Views/Shared/_Layout.cshtml` dla projektów MVC</span><span class="sxs-lookup"><span data-stu-id="ea935-108">For example `~/Pages/Shared/_Layout.cshtml` for Razor Pages `~/Views/Shared/_Layout.cshtml` for MVC projects</span></span>
* <span data-ttu-id="ea935-109">Aby użyć istniejącego kontekstu danych, wybierz co najmniej jeden plik do zastąpienia.</span><span class="sxs-lookup"><span data-stu-id="ea935-109">To use your existing data context, select at least one file to override.</span></span> <span data-ttu-id="ea935-110">Musisz wybrać co najmniej jeden plik, aby dodać kontekstu danych.</span><span class="sxs-lookup"><span data-stu-id="ea935-110">You must select at least one file to add your data context.</span></span>
  * <span data-ttu-id="ea935-111">Wybierz klasy kontekstu danych.</span><span class="sxs-lookup"><span data-stu-id="ea935-111">Select your data context class.</span></span>
  * <span data-ttu-id="ea935-112">Wybierz **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="ea935-112">Select **ADD**.</span></span>
* <span data-ttu-id="ea935-113">Aby utworzyć nowy kontekst użytkownika i potencjalnie tworzenia klasy użytkownika niestandardowego dla tożsamości:</span><span class="sxs-lookup"><span data-stu-id="ea935-113">To create a new user context and possibly create a custom user class for Identity:</span></span>
  * <span data-ttu-id="ea935-114">Wybierz **+** przycisk, aby utworzyć nową **klasa kontekstu danych**.</span><span class="sxs-lookup"><span data-stu-id="ea935-114">Select the **+** button to create a new **Data context class**.</span></span>
  * <span data-ttu-id="ea935-115">Wybierz **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="ea935-115">Select **ADD**.</span></span>

<span data-ttu-id="ea935-116">Uwaga: Jeśli tworzysz nowy kontekst użytkownika, nie trzeba wybrać plik do zastąpienia.</span><span class="sxs-lookup"><span data-stu-id="ea935-116">Note: If you're creating a new user context, you don't have to select a file to override.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="ea935-117">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="ea935-117">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="ea935-118">Jeśli nie zainstalowano wcześniej Generator szkieletu ASP.NET Core, zainstalowanie go teraz:</span><span class="sxs-lookup"><span data-stu-id="ea935-118">If you have not previously installed the ASP.NET Core scaffolder, install it now:</span></span>

```cli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="ea935-119">Dodaj odwołanie do pakietu [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) do projektu (\*.csproj) pliku.</span><span class="sxs-lookup"><span data-stu-id="ea935-119">Add a package reference to [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) to the project (\*.csproj) file.</span></span> <span data-ttu-id="ea935-120">Uruchom następujące polecenie w katalogu projektu:</span><span class="sxs-lookup"><span data-stu-id="ea935-120">Run the following command in the project directory:</span></span>

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

<span data-ttu-id="ea935-121">Uruchom następujące polecenie, aby wyświetlić listę opcji Generator szkieletu tożsamości:</span><span class="sxs-lookup"><span data-stu-id="ea935-121">Run the following command to list the Identity scaffolder options:</span></span>

```cli
dotnet aspnet-codegenerator identity -h
```

<span data-ttu-id="ea935-122">W folderze projektu należy uruchomić Generator szkieletu tożsamości z wybranymi opcjami.</span><span class="sxs-lookup"><span data-stu-id="ea935-122">In the project folder, run the Identity scaffolder with the options you want.</span></span> <span data-ttu-id="ea935-123">Na przykład aby skonfigurować tożsamość przy użyciu domyślnego interfejsu użytkownika i minimalną liczbę plików, uruchom następujące polecenie.</span><span class="sxs-lookup"><span data-stu-id="ea935-123">For example, to setup identity with the default UI and the minimum number of files, run the following command.</span></span> <span data-ttu-id="ea935-124">Użyj poprawnej nazwy FQDN kontekst bazy danych:</span><span class="sxs-lookup"><span data-stu-id="ea935-124">Use the correct fully qualified name for your DB context:</span></span>

```cli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files Account.Register
```

<span data-ttu-id="ea935-125">PowerShell używa średnika jako separatora polecenia.</span><span class="sxs-lookup"><span data-stu-id="ea935-125">Powershell uses semicolon as a command separator.</span></span> <span data-ttu-id="ea935-126">Przy użyciu programu powershell, ucieczki średnikami, na liście plików lub umieścić na liście plików w cudzysłów.</span><span class="sxs-lookup"><span data-stu-id="ea935-126">When using powershell, escape the semi-colons in the file list or put the file list in double quotes.</span></span> <span data-ttu-id="ea935-127">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="ea935-127">For example:</span></span>

```cli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="ea935-128">Jeśli Generator szkieletu tożsamości jest uruchomiony bez określenia `--files` flagi lub `--useDefaultUI` Flaga wszystkie dostępne strony tożsamości interfejsu użytkownika zostanie utworzona w projekcie.</span><span class="sxs-lookup"><span data-stu-id="ea935-128">If you run the Identity scaffolder without specifying the `--files` flag or the `--useDefaultUI` flag, all the available Identity UI pages will be created in your project.</span></span>

-------------

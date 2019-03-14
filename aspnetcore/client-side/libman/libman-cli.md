---
title: LibMan interfejsu wiersza polecenia (CLI) za pomocą platformy ASP.NET Core
author: scottaddie
description: Dowiedz się, jak używać interfejsu wiersza polecenia (CLI) LibMan w projektach programu ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 08/30/2018
uid: client-side/libman/libman-cli
ms.openlocfilehash: 5667f79648a60b8fd9496f8041ef08891ab766af
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57073652"
---
# <a name="use-the-libman-command-line-interface-cli-with-aspnet-core"></a><span data-ttu-id="3c57b-103">LibMan interfejsu wiersza polecenia (CLI) za pomocą platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3c57b-103">Use the LibMan command-line interface (CLI) with ASP.NET Core</span></span>

<span data-ttu-id="3c57b-104">Przez [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="3c57b-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="3c57b-105">[LibMan](xref:client-side/libman/index) interfejs wiersza polecenia to narzędzie dla wielu platform, które ma obsługiwane wszędzie platformy .NET Core jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="3c57b-105">The [LibMan](xref:client-side/libman/index) CLI is a cross-platform tool that's supported everywhere .NET Core is supported.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3c57b-106">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="3c57b-106">Prerequisites</span></span>

* [!INCLUDE [2.1-SDK](../../includes/2.1-SDK.md)]

## <a name="installation"></a><span data-ttu-id="3c57b-107">Instalacja</span><span class="sxs-lookup"><span data-stu-id="3c57b-107">Installation</span></span>

<span data-ttu-id="3c57b-108">Aby zainstalować interfejs wiersza polecenia LibMan:</span><span class="sxs-lookup"><span data-stu-id="3c57b-108">To install the LibMan CLI:</span></span>

```console
dotnet tool install -g Microsoft.Web.LibraryManager.Cli
```

<span data-ttu-id="3c57b-109">A [narzędzia programu .NET Core globalnego](/dotnet/core/tools/global-tools#install-a-global-tool) jest instalowany z [Microsoft.Web.LibraryManager.Cli](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Cli/) pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="3c57b-109">A [.NET Core Global Tool](/dotnet/core/tools/global-tools#install-a-global-tool) is installed from the [Microsoft.Web.LibraryManager.Cli](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Cli/) NuGet package.</span></span>

<span data-ttu-id="3c57b-110">Aby zainstalować interfejs wiersza polecenia LibMan z określonego źródła pakietu NuGet:</span><span class="sxs-lookup"><span data-stu-id="3c57b-110">To install the LibMan CLI from a specific NuGet package source:</span></span>

```console
dotnet tool install -g Microsoft.Web.LibraryManager.Cli --version 1.0.94-g606058a278 --add-source C:\Temp\
```

<span data-ttu-id="3c57b-111">W powyższym przykładzie narzędzie globalne Core .NET jest zainstalowany na lokalnym komputerze Windows *C:\Temp\Microsoft.Web.LibraryManager.Cli.1.0.94-g606058a278.nupkg* pliku.</span><span class="sxs-lookup"><span data-stu-id="3c57b-111">In the preceding example, a .NET Core Global Tool is installed from the local Windows machine's *C:\Temp\Microsoft.Web.LibraryManager.Cli.1.0.94-g606058a278.nupkg* file.</span></span>

## <a name="usage"></a><span data-ttu-id="3c57b-112">Użycie</span><span class="sxs-lookup"><span data-stu-id="3c57b-112">Usage</span></span>

<span data-ttu-id="3c57b-113">Po pomyślnej instalacji interfejsu wiersza polecenia służy następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="3c57b-113">After successful installation of the CLI, the following command can be used:</span></span>

```console
libman
```

<span data-ttu-id="3c57b-114">Aby wyświetlić zainstalowanej wersji interfejsu wiersza polecenia:</span><span class="sxs-lookup"><span data-stu-id="3c57b-114">To view the installed CLI version:</span></span>

```console
libman --version
```

<span data-ttu-id="3c57b-115">Aby wyświetlić dostępne polecenia interfejsu wiersza polecenia:</span><span class="sxs-lookup"><span data-stu-id="3c57b-115">To view the available CLI commands:</span></span>

```console
libman --help
```

<span data-ttu-id="3c57b-116">Poprzednie polecenie wyświetli dane wyjściowe podobne do następujących:</span><span class="sxs-lookup"><span data-stu-id="3c57b-116">The preceding command displays output similar to the following:</span></span>

```console
 1.0.163+g45474d37ed

Usage: libman [options] [command]

Options:
  --help|-h  Show help information
  --version  Show version information

Commands:
  cache      List or clean libman cache contents
  clean      Deletes all library files defined in libman.json from the project
  init       Create a new libman.json
  install    Add a library definition to the libman.json file, and download the 
             library to the specified location
  restore    Downloads all files from provider and saves them to specified 
             destination
  uninstall  Deletes all files for the specified library from their specified 
             destination, then removes the specified library definition from 
             libman.json
  update     Updates the specified library

Use "libman [command] --help" for more information about a command.
```

<span data-ttu-id="3c57b-117">W poniższych sekcjach opisano dostępne polecenia interfejsu wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="3c57b-117">The following sections outline the available CLI commands.</span></span>

## <a name="initialize-libman-in-the-project"></a><span data-ttu-id="3c57b-118">Inicjowanie LibMan w projekcie</span><span class="sxs-lookup"><span data-stu-id="3c57b-118">Initialize LibMan in the project</span></span>

<span data-ttu-id="3c57b-119">`libman init` Polecenie tworzy *libman.json* pliku, jeśli nie istnieje.</span><span class="sxs-lookup"><span data-stu-id="3c57b-119">The `libman init` command creates a *libman.json* file if one doesn't exist.</span></span> <span data-ttu-id="3c57b-120">Plik jest tworzony przy użyciu domyślnej zawartości szablonu elementu.</span><span class="sxs-lookup"><span data-stu-id="3c57b-120">The file is created with the default item template content.</span></span>

### <a name="synopsis"></a><span data-ttu-id="3c57b-121">Streszczenie</span><span class="sxs-lookup"><span data-stu-id="3c57b-121">Synopsis</span></span>

```console
libman init [-d|--default-destination] [-p|--default-provider] [--verbosity]
libman init [-h|--help]
```

### <a name="options"></a><span data-ttu-id="3c57b-122">Opcje</span><span class="sxs-lookup"><span data-stu-id="3c57b-122">Options</span></span>

<span data-ttu-id="3c57b-123">Poniższe opcje są dostępne dla `libman init` polecenia:</span><span class="sxs-lookup"><span data-stu-id="3c57b-123">The following options are available for the `libman init` command:</span></span>

* `-d|--default-destination <PATH>`

  <span data-ttu-id="3c57b-124">Ścieżka względną do bieżącego folderu.</span><span class="sxs-lookup"><span data-stu-id="3c57b-124">A path relative to the current folder.</span></span> <span data-ttu-id="3c57b-125">Pliki bibliotek są instalowane w tej lokalizacji, jeśli nie `destination` właściwość jest zdefiniowana dla biblioteki w *libman.json*.</span><span class="sxs-lookup"><span data-stu-id="3c57b-125">Library files are installed in this location if no `destination` property is defined for a library in *libman.json*.</span></span> <span data-ttu-id="3c57b-126">`<PATH>` Wartości są zapisywane w `defaultDestination` właściwość *libman.json*.</span><span class="sxs-lookup"><span data-stu-id="3c57b-126">The `<PATH>` value is written to the `defaultDestination` property of *libman.json*.</span></span>

* `-p|--default-provider <PROVIDER>`

  <span data-ttu-id="3c57b-127">Dostawcę, który ma być używany, jeśli nie zdefiniowano żadnego dostawcy dla danej biblioteki.</span><span class="sxs-lookup"><span data-stu-id="3c57b-127">The provider to use if no provider is defined for a given library.</span></span> <span data-ttu-id="3c57b-128">`<PROVIDER>` Wartości są zapisywane w `defaultProvider` właściwość *libman.json*.</span><span class="sxs-lookup"><span data-stu-id="3c57b-128">The `<PROVIDER>` value is written to the `defaultProvider` property of *libman.json*.</span></span> <span data-ttu-id="3c57b-129">Zastąp `<PROVIDER>` przy użyciu jednego z następujących wartości:</span><span class="sxs-lookup"><span data-stu-id="3c57b-129">Replace `<PROVIDER>` with one of the following values:</span></span>

  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="3c57b-130">Przykłady</span><span class="sxs-lookup"><span data-stu-id="3c57b-130">Examples</span></span>

<span data-ttu-id="3c57b-131">Aby utworzyć *libman.json* pliku w projektach programu ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="3c57b-131">To create a *libman.json* file in an ASP.NET Core project:</span></span>

* <span data-ttu-id="3c57b-132">Przejdź do katalogu głównego projektu.</span><span class="sxs-lookup"><span data-stu-id="3c57b-132">Navigate to the project root.</span></span>
* <span data-ttu-id="3c57b-133">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="3c57b-133">Run the following command:</span></span>

  ```console
  libman init
  ```

* <span data-ttu-id="3c57b-134">Wpisz nazwę domyślnego dostawcę lub naciśnij klawisz `Enter` używać domyślnego dostawcę CDNJS.</span><span class="sxs-lookup"><span data-stu-id="3c57b-134">Type the name of the default provider, or press `Enter` to use the default CDNJS provider.</span></span> <span data-ttu-id="3c57b-135">Prawidłowe wartości to:</span><span class="sxs-lookup"><span data-stu-id="3c57b-135">Valid values include:</span></span>

  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

  ![polecenie init libman — domyślny dostawca](_static/libman-init-provider.png)

<span data-ttu-id="3c57b-137">A *libman.json* plik zostanie dodany do katalogu głównego projektu o następującej zawartości:</span><span class="sxs-lookup"><span data-stu-id="3c57b-137">A *libman.json* file is added to the project root with the following content:</span></span>

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": []
}
```

## <a name="add-library-files"></a><span data-ttu-id="3c57b-138">Dodaj pliki bibliotek</span><span class="sxs-lookup"><span data-stu-id="3c57b-138">Add library files</span></span>

<span data-ttu-id="3c57b-139">`libman install` Polecenie pobiera i instaluje pliki biblioteki do projektu.</span><span class="sxs-lookup"><span data-stu-id="3c57b-139">The `libman install` command downloads and installs library files into the project.</span></span> <span data-ttu-id="3c57b-140">A *libman.json* plik zostanie dodany, jeśli nie istnieje.</span><span class="sxs-lookup"><span data-stu-id="3c57b-140">A *libman.json* file is added if one doesn't exist.</span></span> <span data-ttu-id="3c57b-141">*Libman.json* plik zostanie zmodyfikowany na potrzeby przechowywania szczegóły konfiguracji dla plików biblioteki.</span><span class="sxs-lookup"><span data-stu-id="3c57b-141">The *libman.json* file is modified to store configuration details for the library files.</span></span>

### <a name="synopsis"></a><span data-ttu-id="3c57b-142">Streszczenie</span><span class="sxs-lookup"><span data-stu-id="3c57b-142">Synopsis</span></span>

```console
libman install <LIBRARY> [-d|--destination] [--files] [-p|--provider] [--verbosity]
libman install [-h|--help]
```

### <a name="arguments"></a><span data-ttu-id="3c57b-143">Argumenty</span><span class="sxs-lookup"><span data-stu-id="3c57b-143">Arguments</span></span>

`LIBRARY`

<span data-ttu-id="3c57b-144">Nazwa biblioteki do zainstalowania.</span><span class="sxs-lookup"><span data-stu-id="3c57b-144">The name of the library to install.</span></span> <span data-ttu-id="3c57b-145">Ta nazwa może zawierać notacji numeru wersji (na przykład `@1.2.0`).</span><span class="sxs-lookup"><span data-stu-id="3c57b-145">This name may include version number notation (for example, `@1.2.0`).</span></span>

### <a name="options"></a><span data-ttu-id="3c57b-146">Opcje</span><span class="sxs-lookup"><span data-stu-id="3c57b-146">Options</span></span>

<span data-ttu-id="3c57b-147">Poniższe opcje są dostępne dla `libman install` polecenia:</span><span class="sxs-lookup"><span data-stu-id="3c57b-147">The following options are available for the `libman install` command:</span></span>

* `-d|--destination <PATH>`

  <span data-ttu-id="3c57b-148">Lokalizacja, aby zainstalować bibliotekę.</span><span class="sxs-lookup"><span data-stu-id="3c57b-148">The location to install the library.</span></span> <span data-ttu-id="3c57b-149">Jeśli nie zostanie określony, domyślna lokalizacja jest używana.</span><span class="sxs-lookup"><span data-stu-id="3c57b-149">If not specified, the default location is used.</span></span> <span data-ttu-id="3c57b-150">Jeśli nie `defaultDestination` właściwość jest określona w *libman.json*, ta opcja jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="3c57b-150">If no `defaultDestination` property is specified in *libman.json*, this option is required.</span></span>

* `--files <FILE>`

  <span data-ttu-id="3c57b-151">Określ nazwę pliku do zainstalowania z biblioteki.</span><span class="sxs-lookup"><span data-stu-id="3c57b-151">Specify the name of the file to install from the library.</span></span> <span data-ttu-id="3c57b-152">Jeśli nie zostanie określony, wszystkie pliki z biblioteki są instalowane.</span><span class="sxs-lookup"><span data-stu-id="3c57b-152">If not specified, all files from the library are installed.</span></span> <span data-ttu-id="3c57b-153">Podaj jeden `--files` opcji na plik do zainstalowania.</span><span class="sxs-lookup"><span data-stu-id="3c57b-153">Provide one `--files` option per file to be installed.</span></span> <span data-ttu-id="3c57b-154">Obsługiwane są zbyt ścieżek względnych.</span><span class="sxs-lookup"><span data-stu-id="3c57b-154">Relative paths are supported too.</span></span> <span data-ttu-id="3c57b-155">Na przykład: `--files dist/browser/signalr.js`.</span><span class="sxs-lookup"><span data-stu-id="3c57b-155">For example: `--files dist/browser/signalr.js`.</span></span>

* `-p|--provider <PROVIDER>`

  <span data-ttu-id="3c57b-156">Nazwa dostawcy na potrzeby przejęcia biblioteki.</span><span class="sxs-lookup"><span data-stu-id="3c57b-156">The name of the provider to use for the library acquisition.</span></span> <span data-ttu-id="3c57b-157">Zastąp `<PROVIDER>` przy użyciu jednego z następujących wartości:</span><span class="sxs-lookup"><span data-stu-id="3c57b-157">Replace `<PROVIDER>` with one of the following values:</span></span>
  
  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

  <span data-ttu-id="3c57b-158">Jeśli nie zostanie określony, `defaultProvider` właściwość *libman.json* jest używany.</span><span class="sxs-lookup"><span data-stu-id="3c57b-158">If not specified, the `defaultProvider` property in *libman.json* is used.</span></span> <span data-ttu-id="3c57b-159">Jeśli nie `defaultProvider` właściwość jest określona w *libman.json*, ta opcja jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="3c57b-159">If no `defaultProvider` property is specified in *libman.json*, this option is required.</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="3c57b-160">Przykłady</span><span class="sxs-lookup"><span data-stu-id="3c57b-160">Examples</span></span>

<span data-ttu-id="3c57b-161">Należy wziąć pod uwagę następujące *libman.json* pliku:</span><span class="sxs-lookup"><span data-stu-id="3c57b-161">Consider the following *libman.json* file:</span></span>

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": []
}
```

<span data-ttu-id="3c57b-162">Aby zainstalować wersję jQuery 3.2.1 *jquery.min.js* plik *skryptów/wwwroot/jquery* folder przy użyciu dostawcy CDNJS:</span><span class="sxs-lookup"><span data-stu-id="3c57b-162">To install the jQuery version 3.2.1 *jquery.min.js* file to the *wwwroot/scripts/jquery* folder using the CDNJS provider:</span></span>

```console
libman install jquery@3.2.1 --provider cdnjs --destination wwwroot/scripts/jquery --files jquery.min.js
```

<span data-ttu-id="3c57b-163">*Libman.json* pliku jest podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="3c57b-163">The *libman.json* file resembles the following:</span></span>

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": [
    {
      "library": "jquery@3.2.1",
      "destination": "wwwroot/scripts/jquery",
      "files": [
        "jquery.min.js"
      ]
    }
  ]
}
```

<span data-ttu-id="3c57b-164">Aby zainstalować *calendar.js* i *calendar.css* plików ze *C:\\temp\\contosoCalendar\\*  przy użyciu systemu plików Dostawca:</span><span class="sxs-lookup"><span data-stu-id="3c57b-164">To install the *calendar.js* and *calendar.css* files from *C:\\temp\\contosoCalendar\\* using the file system provider:</span></span>

  ```console
  libman install C:\temp\contosoCalendar\ --provider filesystem --files calendar.js --files calendar.css
  ```

<span data-ttu-id="3c57b-165">Następujący monit pojawia się z dwóch przyczyn:</span><span class="sxs-lookup"><span data-stu-id="3c57b-165">The following prompt appears for two reasons:</span></span>

* <span data-ttu-id="3c57b-166">*Libman.json* plik nie zawiera `defaultDestination` właściwości.</span><span class="sxs-lookup"><span data-stu-id="3c57b-166">The *libman.json* file doesn't contain a `defaultDestination` property.</span></span>
* <span data-ttu-id="3c57b-167">`libman install` Nie zawiera polecenia `-d|--destination` opcji.</span><span class="sxs-lookup"><span data-stu-id="3c57b-167">The `libman install` command doesn't contain the `-d|--destination` option.</span></span>

![polecenie - instalacji libman docelowego](_static/libman-install-destination.png)

<span data-ttu-id="3c57b-169">Po zaakceptowaniu domyślnej lokalizacji docelowej *libman.json* pliku jest podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="3c57b-169">After accepting the default destination, the *libman.json* file resembles the following:</span></span>

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": [
    {
      "library": "jquery@3.2.1",
      "destination": "wwwroot/scripts/jquery",
      "files": [
        "jquery.min.js"
      ]
    },
    {
      "library": "C:\\temp\\contosoCalendar\\",
      "provider": "filesystem",
      "destination": "wwwroot/lib/contosoCalendar",
      "files": [
        "calendar.js",
        "calendar.css"
      ]
    }
  ]
}
```

## <a name="restore-library-files"></a><span data-ttu-id="3c57b-170">Przywróć pliki bibliotek</span><span class="sxs-lookup"><span data-stu-id="3c57b-170">Restore library files</span></span>

<span data-ttu-id="3c57b-171">`libman restore` Polecenie powoduje zainstalowanie plików biblioteki zdefiniowanych w *libman.json*.</span><span class="sxs-lookup"><span data-stu-id="3c57b-171">The `libman restore` command installs library files defined in *libman.json*.</span></span> <span data-ttu-id="3c57b-172">Mają zastosowanie następujące zasady:</span><span class="sxs-lookup"><span data-stu-id="3c57b-172">The following rules apply:</span></span>

* <span data-ttu-id="3c57b-173">Jeśli nie *libman.json* plik istnieje w katalogu głównym projektu, zostanie zwrócony błąd.</span><span class="sxs-lookup"><span data-stu-id="3c57b-173">If no *libman.json* file exists in the project root, an error is returned.</span></span>
* <span data-ttu-id="3c57b-174">Jeśli biblioteka określa dostawcę, `defaultProvider` właściwość *libman.json* jest ignorowana.</span><span class="sxs-lookup"><span data-stu-id="3c57b-174">If a library specifies a provider, the `defaultProvider` property in *libman.json* is ignored.</span></span>
* <span data-ttu-id="3c57b-175">Jeśli biblioteka określa miejsce docelowe `defaultDestination` właściwość *libman.json* jest ignorowana.</span><span class="sxs-lookup"><span data-stu-id="3c57b-175">If a library specifies a destination, the `defaultDestination` property in *libman.json* is ignored.</span></span>

### <a name="synopsis"></a><span data-ttu-id="3c57b-176">Streszczenie</span><span class="sxs-lookup"><span data-stu-id="3c57b-176">Synopsis</span></span>

```console
libman restore [--verbosity]
libman restore [-h|--help]
```

### <a name="options"></a><span data-ttu-id="3c57b-177">Opcje</span><span class="sxs-lookup"><span data-stu-id="3c57b-177">Options</span></span>

<span data-ttu-id="3c57b-178">Poniższe opcje są dostępne dla `libman restore` polecenia:</span><span class="sxs-lookup"><span data-stu-id="3c57b-178">The following options are available for the `libman restore` command:</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="3c57b-179">Przykłady</span><span class="sxs-lookup"><span data-stu-id="3c57b-179">Examples</span></span>

<span data-ttu-id="3c57b-180">Do przywrócenia plików biblioteki zdefiniowanych w *libman.json*:</span><span class="sxs-lookup"><span data-stu-id="3c57b-180">To restore the library files defined in *libman.json*:</span></span>

```console
libman restore
```

## <a name="delete-library-files"></a><span data-ttu-id="3c57b-181">Usuń pliki bibliotek</span><span class="sxs-lookup"><span data-stu-id="3c57b-181">Delete library files</span></span>

<span data-ttu-id="3c57b-182">`libman clean` Polecenie usuwa wcześniej przywrócić za pośrednictwem LibMan pliki biblioteki.</span><span class="sxs-lookup"><span data-stu-id="3c57b-182">The `libman clean` command deletes library files previously restored via LibMan.</span></span> <span data-ttu-id="3c57b-183">Foldery, które stać się puste po wykonaniu tej czynności zostaną usunięte.</span><span class="sxs-lookup"><span data-stu-id="3c57b-183">Folders that become empty after this operation are deleted.</span></span> <span data-ttu-id="3c57b-184">Plików bibliotek skojarzone konfiguracje w `libraries` właściwość *libman.json* nie zostaną usunięte.</span><span class="sxs-lookup"><span data-stu-id="3c57b-184">The library files' associated configurations in the `libraries` property of *libman.json* aren't removed.</span></span>

### <a name="synopsis"></a><span data-ttu-id="3c57b-185">Streszczenie</span><span class="sxs-lookup"><span data-stu-id="3c57b-185">Synopsis</span></span>

```console
libman clean [--verbosity]
libman clean [-h|--help]
```

### <a name="options"></a><span data-ttu-id="3c57b-186">Opcje</span><span class="sxs-lookup"><span data-stu-id="3c57b-186">Options</span></span>

<span data-ttu-id="3c57b-187">Poniższe opcje są dostępne dla `libman clean` polecenia:</span><span class="sxs-lookup"><span data-stu-id="3c57b-187">The following options are available for the `libman clean` command:</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="3c57b-188">Przykłady</span><span class="sxs-lookup"><span data-stu-id="3c57b-188">Examples</span></span>

<span data-ttu-id="3c57b-189">Aby usunąć pliki biblioteki zainstalowanych za pomocą LibMan:</span><span class="sxs-lookup"><span data-stu-id="3c57b-189">To delete library files installed via LibMan:</span></span>

```console
libman clean
```

## <a name="uninstall-library-files"></a><span data-ttu-id="3c57b-190">Odinstaluj pliki biblioteki</span><span class="sxs-lookup"><span data-stu-id="3c57b-190">Uninstall library files</span></span>

<span data-ttu-id="3c57b-191">`libman uninstall` Polecenia:</span><span class="sxs-lookup"><span data-stu-id="3c57b-191">The `libman uninstall` command:</span></span>

* <span data-ttu-id="3c57b-192">Usuwa wszystkie pliki skojarzone z określonej biblioteki z miejsca docelowego w *libman.json*.</span><span class="sxs-lookup"><span data-stu-id="3c57b-192">Deletes all files associated with the specified library from the destination in *libman.json*.</span></span>
* <span data-ttu-id="3c57b-193">Usuwa konfigurację biblioteki skojarzony z *libman.json*.</span><span class="sxs-lookup"><span data-stu-id="3c57b-193">Removes the associated library configuration from *libman.json*.</span></span>

<span data-ttu-id="3c57b-194">Błąd występuje, gdy:</span><span class="sxs-lookup"><span data-stu-id="3c57b-194">An error occurs when:</span></span>

* <span data-ttu-id="3c57b-195">Nie *libman.json* plik istnieje w katalogu głównym projektu.</span><span class="sxs-lookup"><span data-stu-id="3c57b-195">No *libman.json* file exists in the project root.</span></span>
* <span data-ttu-id="3c57b-196">Określonej biblioteki nie istnieje.</span><span class="sxs-lookup"><span data-stu-id="3c57b-196">The specified library doesn't exist.</span></span>

<span data-ttu-id="3c57b-197">Jeśli jest zainstalowana więcej niż jedna biblioteka o tej samej nazwie, zostanie wyświetlony monit wybierz jedną.</span><span class="sxs-lookup"><span data-stu-id="3c57b-197">If more than one library with the same name is installed, you're prompted to choose one.</span></span>

### <a name="synopsis"></a><span data-ttu-id="3c57b-198">Streszczenie</span><span class="sxs-lookup"><span data-stu-id="3c57b-198">Synopsis</span></span>

```console
libman uninstall <LIBRARY> [--verbosity]
libman uninstall [-h|--help]
```

### <a name="arguments"></a><span data-ttu-id="3c57b-199">Argumenty</span><span class="sxs-lookup"><span data-stu-id="3c57b-199">Arguments</span></span>

`LIBRARY`

<span data-ttu-id="3c57b-200">Nazwa biblioteki do odinstalowania.</span><span class="sxs-lookup"><span data-stu-id="3c57b-200">The name of the library to uninstall.</span></span> <span data-ttu-id="3c57b-201">Ta nazwa może zawierać notacji numeru wersji (na przykład `@1.2.0`).</span><span class="sxs-lookup"><span data-stu-id="3c57b-201">This name may include version number notation (for example, `@1.2.0`).</span></span>

### <a name="options"></a><span data-ttu-id="3c57b-202">Opcje</span><span class="sxs-lookup"><span data-stu-id="3c57b-202">Options</span></span>

<span data-ttu-id="3c57b-203">Poniższe opcje są dostępne dla `libman uninstall` polecenia:</span><span class="sxs-lookup"><span data-stu-id="3c57b-203">The following options are available for the `libman uninstall` command:</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="3c57b-204">Przykłady</span><span class="sxs-lookup"><span data-stu-id="3c57b-204">Examples</span></span>

<span data-ttu-id="3c57b-205">Należy wziąć pod uwagę następujące *libman.json* pliku:</span><span class="sxs-lookup"><span data-stu-id="3c57b-205">Consider the following *libman.json* file:</span></span>

[!code-json[](samples/LibManSample/libman.json)]

* <span data-ttu-id="3c57b-206">Aby odinstalować jQuery, powiedzie się jedną z następujących poleceń:</span><span class="sxs-lookup"><span data-stu-id="3c57b-206">To uninstall jQuery, either of the following commands succeed:</span></span>

  ```console
  libman uninstall jquery
  ```

  ```console
  libman uninstall jquery@3.3.1
  ```

* <span data-ttu-id="3c57b-207">Aby odinstalować plików Lodash zainstalowanych za pomocą `filesystem` dostawcy:</span><span class="sxs-lookup"><span data-stu-id="3c57b-207">To uninstall the Lodash files installed via the `filesystem` provider:</span></span>

  ```console
  libman uninstall C:\temp\lodash\
  ```

## <a name="update-library-version"></a><span data-ttu-id="3c57b-208">Zaktualizuj wersję biblioteki</span><span class="sxs-lookup"><span data-stu-id="3c57b-208">Update library version</span></span>

<span data-ttu-id="3c57b-209">`libman update` Polecenie aktualizuje bibliotekę zainstalowanych za pomocą LibMan do wybranej wersji.</span><span class="sxs-lookup"><span data-stu-id="3c57b-209">The `libman update` command updates a library installed via LibMan to the specified version.</span></span>

<span data-ttu-id="3c57b-210">Błąd występuje, gdy:</span><span class="sxs-lookup"><span data-stu-id="3c57b-210">An error occurs when:</span></span>

* <span data-ttu-id="3c57b-211">Nie *libman.json* plik istnieje w katalogu głównym projektu.</span><span class="sxs-lookup"><span data-stu-id="3c57b-211">No *libman.json* file exists in the project root.</span></span>
* <span data-ttu-id="3c57b-212">Określonej biblioteki nie istnieje.</span><span class="sxs-lookup"><span data-stu-id="3c57b-212">The specified library doesn't exist.</span></span>

<span data-ttu-id="3c57b-213">Jeśli jest zainstalowana więcej niż jedna biblioteka o tej samej nazwie, zostanie wyświetlony monit wybierz jedną.</span><span class="sxs-lookup"><span data-stu-id="3c57b-213">If more than one library with the same name is installed, you're prompted to choose one.</span></span>

### <a name="synopsis"></a><span data-ttu-id="3c57b-214">Streszczenie</span><span class="sxs-lookup"><span data-stu-id="3c57b-214">Synopsis</span></span>

```console
libman update <LIBRARY> [-pre] [--to] [--verbosity]
libman update [-h|--help]
```

### <a name="arguments"></a><span data-ttu-id="3c57b-215">Argumenty</span><span class="sxs-lookup"><span data-stu-id="3c57b-215">Arguments</span></span>

`LIBRARY`

<span data-ttu-id="3c57b-216">Nazwa biblioteki, aby zaktualizować.</span><span class="sxs-lookup"><span data-stu-id="3c57b-216">The name of the library to update.</span></span>

### <a name="options"></a><span data-ttu-id="3c57b-217">Opcje</span><span class="sxs-lookup"><span data-stu-id="3c57b-217">Options</span></span>

<span data-ttu-id="3c57b-218">Poniższe opcje są dostępne dla `libman update` polecenia:</span><span class="sxs-lookup"><span data-stu-id="3c57b-218">The following options are available for the `libman update` command:</span></span>

* `-pre`

  <span data-ttu-id="3c57b-219">Uzyskaj najnowszą wersję wstępną wersję biblioteki.</span><span class="sxs-lookup"><span data-stu-id="3c57b-219">Obtain the latest prerelease version of the library.</span></span>

* `--to <VERSION>`

  <span data-ttu-id="3c57b-220">Uzyskać określoną wersję biblioteki.</span><span class="sxs-lookup"><span data-stu-id="3c57b-220">Obtain a specific version of the library.</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="3c57b-221">Przykłady</span><span class="sxs-lookup"><span data-stu-id="3c57b-221">Examples</span></span>

* <span data-ttu-id="3c57b-222">Aby zaktualizować jQuery do najnowszej wersji:</span><span class="sxs-lookup"><span data-stu-id="3c57b-222">To update jQuery to the latest version:</span></span>

  ```console
  libman update jquery
  ```

* <span data-ttu-id="3c57b-223">Aby zaktualizować jQuery do wersji 3.3.1:</span><span class="sxs-lookup"><span data-stu-id="3c57b-223">To update jQuery to version 3.3.1:</span></span>

  ```console
  libman update jquery --to 3.3.1
  ```

* <span data-ttu-id="3c57b-224">Aby zaktualizować jQuery do najnowszej wersji wstępnej:</span><span class="sxs-lookup"><span data-stu-id="3c57b-224">To update jQuery to the latest prerelease version:</span></span>

  ```console
  libman update jquery -pre
  ```

## <a name="manage-library-cache"></a><span data-ttu-id="3c57b-225">Zarządzaj pamięcią podręczną biblioteki</span><span class="sxs-lookup"><span data-stu-id="3c57b-225">Manage library cache</span></span>

<span data-ttu-id="3c57b-226">`libman cache` Polecenia zarządza w pamięci podręcznej biblioteki LibMan.</span><span class="sxs-lookup"><span data-stu-id="3c57b-226">The `libman cache` command manages the LibMan library cache.</span></span> <span data-ttu-id="3c57b-227">`filesystem` Dostawcy nie korzysta z pamięci podręcznej biblioteki.</span><span class="sxs-lookup"><span data-stu-id="3c57b-227">The `filesystem` provider doesn't use the library cache.</span></span>

### <a name="synopsis"></a><span data-ttu-id="3c57b-228">Streszczenie</span><span class="sxs-lookup"><span data-stu-id="3c57b-228">Synopsis</span></span>

```console
libman cache clean [<PROVIDER>] [--verbosity]
libman cache list [--files] [--libraries] [--verbosity]
libman cache [-h|--help]
```

### <a name="arguments"></a><span data-ttu-id="3c57b-229">Argumenty</span><span class="sxs-lookup"><span data-stu-id="3c57b-229">Arguments</span></span>

`PROVIDER`

<span data-ttu-id="3c57b-230">Używana tylko z `clean` polecenia.</span><span class="sxs-lookup"><span data-stu-id="3c57b-230">Only used with the `clean` command.</span></span> <span data-ttu-id="3c57b-231">Określa pamięci podręcznej dostawcy, aby wyczyścić.</span><span class="sxs-lookup"><span data-stu-id="3c57b-231">Specifies the provider cache to clean.</span></span> <span data-ttu-id="3c57b-232">Prawidłowe wartości to:</span><span class="sxs-lookup"><span data-stu-id="3c57b-232">Valid values include:</span></span>

[!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

### <a name="options"></a><span data-ttu-id="3c57b-233">Opcje</span><span class="sxs-lookup"><span data-stu-id="3c57b-233">Options</span></span>

<span data-ttu-id="3c57b-234">Poniższe opcje są dostępne dla `libman cache` polecenia:</span><span class="sxs-lookup"><span data-stu-id="3c57b-234">The following options are available for the `libman cache` command:</span></span>

* `--files`

  <span data-ttu-id="3c57b-235">Lista nazw plików, które są buforowane.</span><span class="sxs-lookup"><span data-stu-id="3c57b-235">List the names of files that are cached.</span></span>

* `--libraries`

  <span data-ttu-id="3c57b-236">Wyświetl listę nazw bibliotek, które są buforowane.</span><span class="sxs-lookup"><span data-stu-id="3c57b-236">List the names of libraries that are cached.</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="3c57b-237">Przykłady</span><span class="sxs-lookup"><span data-stu-id="3c57b-237">Examples</span></span>

* <span data-ttu-id="3c57b-238">Aby wyświetlić nazwy pamięci podręcznej bibliotek na dostawcy, użyj jednej z następujących poleceń:</span><span class="sxs-lookup"><span data-stu-id="3c57b-238">To view the names of cached libraries per provider, use one of the following commands:</span></span>

  ```console
  libman cache list
  ```

  ```console
  libman cache list --libraries
  ```

  <span data-ttu-id="3c57b-239">Wyświetlone dane wyjściowe podobne do następujących:</span><span class="sxs-lookup"><span data-stu-id="3c57b-239">Output similar to the following is displayed:</span></span>

  ```console
  Cache contents:
  ---------------
  unpkg:
      knockout
      react
      vue
  cdnjs:
      font-awesome
      jquery
      knockout
      lodash.js
      react
  ```

* <span data-ttu-id="3c57b-240">Aby wyświetlić nazwy plików w pamięci podręcznej biblioteki na dostawcy:</span><span class="sxs-lookup"><span data-stu-id="3c57b-240">To view the names of cached library files per provider:</span></span>

  ```console
  libman cache list --files
  ```

  <span data-ttu-id="3c57b-241">Wyświetlone dane wyjściowe podobne do następujących:</span><span class="sxs-lookup"><span data-stu-id="3c57b-241">Output similar to the following is displayed:</span></span>

  ```console
  Cache contents:
  ---------------
  unpkg:
      knockout:
          <list omitted for brevity>
      react:
          <list omitted for brevity>
      vue:
          <list omitted for brevity>
  cdnjs:
      font-awesome
          metadata.json
      jquery
          metadata.json
          3.2.1\core.js
          3.2.1\jquery.js
          3.2.1\jquery.min.js
          3.2.1\jquery.min.map
          3.2.1\jquery.slim.js
          3.2.1\jquery.slim.min.js
          3.2.1\jquery.slim.min.map
          3.3.1\core.js
          3.3.1\jquery.js
          3.3.1\jquery.min.js
          3.3.1\jquery.min.map
          3.3.1\jquery.slim.js
          3.3.1\jquery.slim.min.js
          3.3.1\jquery.slim.min.map
      knockout
          metadata.json
          3.4.2\knockout-debug.js
          3.4.2\knockout-min.js
      lodash.js
          metadata.json
          4.17.10\lodash.js
          4.17.10\lodash.min.js
      react
          metadata.json
  ```

  <span data-ttu-id="3c57b-242">Powiadomienia poprzedzającego dane wyjściowe pokazuje tej wersji 3.2.1 i 3.3.1 są buforowane w ramach dostawcy CDNJS jQuery.</span><span class="sxs-lookup"><span data-stu-id="3c57b-242">Notice the preceding output shows that jQuery versions 3.2.1 and 3.3.1 are cached under the CDNJS provider.</span></span>

* <span data-ttu-id="3c57b-243">Aby opróżnić pamięć podręczną biblioteki dostawcy CDNJS:</span><span class="sxs-lookup"><span data-stu-id="3c57b-243">To empty the library cache for the CDNJS provider:</span></span>

  ```console
  libman cache clean cdnjs
  ```

  <span data-ttu-id="3c57b-244">Po opróżnienia pamięci podręcznej dostawcy CDNJS `libman cache list` polecenie wyświetla następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="3c57b-244">After emptying the CDNJS provider cache, the `libman cache list` command displays the following:</span></span>

  ```console
  Cache contents:
  ---------------
  unpkg:
      knockout
      react
      vue
  cdnjs:
      (empty)
  ```

* <span data-ttu-id="3c57b-245">Opróżnienia pamięci podręcznej dla wszystkich obsługiwanych dostawców:</span><span class="sxs-lookup"><span data-stu-id="3c57b-245">To empty the cache for all supported providers:</span></span>

  ```console
  libman cache clean
  ```

  <span data-ttu-id="3c57b-246">Po opróżnieniu wszystkich dostawcy pamięci podręcznych, `libman cache list` polecenie wyświetla następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="3c57b-246">After emptying all provider caches, the `libman cache list` command displays the following:</span></span>

  ```console
  Cache contents:
  ---------------
  unpkg:
      (empty)
  cdnjs:
      (empty)
  ```

## <a name="additional-resources"></a><span data-ttu-id="3c57b-247">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="3c57b-247">Additional resources</span></span>

* [<span data-ttu-id="3c57b-248">Zainstaluj narzędzie globalne</span><span class="sxs-lookup"><span data-stu-id="3c57b-248">Install a Global Tool</span></span>](/dotnet/core/tools/global-tools#install-a-global-tool)
* <xref:client-side/libman/libman-vs>
* [<span data-ttu-id="3c57b-249">Repozytorium LibMan GitHub</span><span class="sxs-lookup"><span data-stu-id="3c57b-249">LibMan GitHub repository</span></span>](https://github.com/aspnet/LibraryManager)

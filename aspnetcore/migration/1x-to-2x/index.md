---
title: Migracja z programu ASP.NET Core 1.x do 2.0
author: scottaddie
description: W tym artykule omówiono wymagania wstępne oraz najbardziej typowe etapy migracji projektu ASP.NET Core 1.x do ASP.NET Core 2.0.
ms.author: scaddie
ms.custom: mvc
ms.date: 10/24/2018
uid: migration/1x-to-2x/index
---
# <a name="migrate-from-aspnet-core-1x-to-20"></a><span data-ttu-id="7bad8-103">Migracja z programu ASP.NET Core 1.x do 2.0</span><span class="sxs-lookup"><span data-stu-id="7bad8-103">Migrate from ASP.NET Core 1.x to 2.0</span></span>

<span data-ttu-id="7bad8-104">Przez [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="7bad8-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="7bad8-105">W tym artykule przedstawiono aktualizowania istniejącego projektu platformy ASP.NET Core 1.x do ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="7bad8-105">In this article, we walk you through updating an existing ASP.NET Core 1.x project to ASP.NET Core 2.0.</span></span> <span data-ttu-id="7bad8-106">Migrowanie aplikacji ASP.NET Core 2.0 umożliwia korzystanie z zalet [wiele nowych funkcji i ulepszeń wydajności](xref:aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="7bad8-106">Migrating your application to ASP.NET Core 2.0 enables you to take advantage of [many new features and performance improvements](xref:aspnetcore-2.0).</span></span>

<span data-ttu-id="7bad8-107">Istniejące aplikacje programu ASP.NET Core 1.x opierają się poza szablony projektów specyficznych dla wersji.</span><span class="sxs-lookup"><span data-stu-id="7bad8-107">Existing ASP.NET Core 1.x applications are based off of version-specific project templates.</span></span> <span data-ttu-id="7bad8-108">Zgodnie z rozwojem platformę ASP.NET Core tak zrobić, szablony projektów i kod startowy w nich zawarte.</span><span class="sxs-lookup"><span data-stu-id="7bad8-108">As the ASP.NET Core framework evolves, so do the project templates and the starter code contained within them.</span></span> <span data-ttu-id="7bad8-109">Oprócz aktualizowanie platformę ASP.NET Core, musisz zaktualizować kod aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7bad8-109">In addition to updating the ASP.NET Core framework, you need to update the code for your application.</span></span>

<a name="prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="7bad8-110">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="7bad8-110">Prerequisites</span></span>

<span data-ttu-id="7bad8-111">Zobacz [Rozpoczynanie pracy z platformą ASP.NET Core](xref:getting-started).</span><span class="sxs-lookup"><span data-stu-id="7bad8-111">See [Get Started with ASP.NET Core](xref:getting-started).</span></span>

<a name="tfm"></a>

## <a name="update-target-framework-moniker-tfm"></a><span data-ttu-id="7bad8-112">Aktualizuj Moniker platformy docelowej (TFM)</span><span class="sxs-lookup"><span data-stu-id="7bad8-112">Update Target Framework Moniker (TFM)</span></span>

<span data-ttu-id="7bad8-113">Projekty przeznaczone dla platformy .NET Core, należy użyć [TFM](/dotnet/standard/frameworks#referring-to-frameworks) wersji większa lub równa .NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="7bad8-113">Projects targeting .NET Core should use the [TFM](/dotnet/standard/frameworks#referring-to-frameworks) of a version greater than or equal to .NET Core 2.0.</span></span> <span data-ttu-id="7bad8-114">Wyszukaj `<TargetFramework>` w węźle *.csproj* plik i zastąp jego tekst wewnętrzny z `netcoreapp2.0`:</span><span class="sxs-lookup"><span data-stu-id="7bad8-114">Search for the `<TargetFramework>` node in the *.csproj* file, and replace its inner text with `netcoreapp2.0`:</span></span>

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=3)]

<span data-ttu-id="7bad8-115">Projekty przeznaczone dla .NET Framework, należy użyć elementu TFM wersji większy lub równy .NET Framework 4.6.1.</span><span class="sxs-lookup"><span data-stu-id="7bad8-115">Projects targeting .NET Framework should use the TFM of a version greater than or equal to .NET Framework 4.6.1.</span></span> <span data-ttu-id="7bad8-116">Wyszukaj `<TargetFramework>` w węźle *.csproj* plik i zastąp jego tekst wewnętrzny z `net461`:</span><span class="sxs-lookup"><span data-stu-id="7bad8-116">Search for the `<TargetFramework>` node in the *.csproj* file, and replace its inner text with `net461`:</span></span>

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=4)]

> [!NOTE]
> <span data-ttu-id="7bad8-117">.NET core 2.0 oferuje znacznie większy obszar powierzchni niż .NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="7bad8-117">.NET Core 2.0 offers a much larger surface area than .NET Core 1.x.</span></span> <span data-ttu-id="7bad8-118">Jeśli jest przeznaczony dla .NET Framework, wyłącznie ze względu na Brak interfejsów API na platformie .NET Core 1.x, przeznaczonych dla platformy .NET Core 2.0 jest zwykle działa.</span><span class="sxs-lookup"><span data-stu-id="7bad8-118">If you're targeting .NET Framework solely because of missing APIs in .NET Core 1.x, targeting .NET Core 2.0 is likely to work.</span></span>

<a name="global-json"></a>

## <a name="update-net-core-sdk-version-in-globaljson"></a><span data-ttu-id="7bad8-119">Aktualizacja wersji zestawu SDK programu .NET Core w global.json</span><span class="sxs-lookup"><span data-stu-id="7bad8-119">Update .NET Core SDK version in global.json</span></span>

<span data-ttu-id="7bad8-120">Jeśli rozwiązanie opiera się na [ *global.json* ](/dotnet/core/tools/global-json) plik, aby odwoływać się do określonej wersji zestawu .NET Core SDK, zaktualizuj jego `version` właściwości do korzystania z wersji 2.0 zainstalowany na komputerze:</span><span class="sxs-lookup"><span data-stu-id="7bad8-120">If your solution relies upon a [*global.json*](/dotnet/core/tools/global-json) file to target a specific .NET Core SDK version, update its `version` property to use the 2.0 version installed on your machine:</span></span>

[!code-json[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/global.json?highlight=3)]

<a name="package-reference"></a>

## <a name="update-package-references"></a><span data-ttu-id="7bad8-121">Aktualizuj odwołania do pakietu</span><span class="sxs-lookup"><span data-stu-id="7bad8-121">Update package references</span></span>

<span data-ttu-id="7bad8-122">*.Csproj* pliku w projekcie 1.x wyświetla każdy pakiet NuGet, używanych przez projekt.</span><span class="sxs-lookup"><span data-stu-id="7bad8-122">The *.csproj* file in a 1.x project lists each NuGet package used by the project.</span></span>

<span data-ttu-id="7bad8-123">W projektach programu ASP.NET Core 2.0 przeznaczonych dla platformy .NET Core 2.0, pojedynczy [meta Microsoft.aspnetcore.all](xref:fundamentals/metapackage) odwoływać w *.csproj* plik zastępuje kolekcję pakietów:</span><span class="sxs-lookup"><span data-stu-id="7bad8-123">In an ASP.NET Core 2.0 project targeting .NET Core 2.0, a single [metapackage](xref:fundamentals/metapackage) reference in the *.csproj* file replaces the collection of packages:</span></span>

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=8-10)]

<span data-ttu-id="7bad8-124">Wszystkie funkcje platformy ASP.NET Core 2.0 i Entity Framework Core 2.0 są uwzględnione w meta Microsoft.aspnetcore.all.</span><span class="sxs-lookup"><span data-stu-id="7bad8-124">All the features of ASP.NET Core 2.0 and Entity Framework Core 2.0 are included in the metapackage.</span></span>

<span data-ttu-id="7bad8-125">Projekty programu ASP.NET Core 2.0, przeznaczone dla .NET Framework powinny nadal odwoływać się do poszczególnych pakietów NuGet.</span><span class="sxs-lookup"><span data-stu-id="7bad8-125">ASP.NET Core 2.0 projects targeting .NET Framework should continue to reference individual NuGet packages.</span></span> <span data-ttu-id="7bad8-126">Aktualizacja `Version` atrybut każdego `<PackageReference />` węzeł 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="7bad8-126">Update the `Version` attribute of each `<PackageReference />` node to 2.0.0.</span></span>

<span data-ttu-id="7bad8-127">Na przykład poniżej przedstawiono listę `<PackageReference />` węzłów użytych w typowym projekcie platformy ASP.NET Core 2.0, przeznaczonych dla platformy .NET Framework:</span><span class="sxs-lookup"><span data-stu-id="7bad8-127">For example, here's the list of `<PackageReference />` nodes used in a typical ASP.NET Core 2.0 project targeting .NET Framework:</span></span>

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=9-22)]

<a name="dot-net-cli-tool-reference"></a>

## <a name="update-net-core-cli-tools"></a><span data-ttu-id="7bad8-128">Aktualizowanie narzędzi interfejsu wiersza polecenia platformy .NET Core</span><span class="sxs-lookup"><span data-stu-id="7bad8-128">Update .NET Core CLI tools</span></span>

<span data-ttu-id="7bad8-129">W *.csproj* pliku, zaktualizuj `Version` atrybut każdego `<DotNetCliToolReference />` węzeł 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="7bad8-129">In the *.csproj* file, update the `Version` attribute of each `<DotNetCliToolReference />` node to 2.0.0.</span></span>

<span data-ttu-id="7bad8-130">Na przykład poniżej przedstawiono listę narzędzi interfejsu wiersza polecenia używane w typowym projekcie platformy ASP.NET Core 2.0, przeznaczonych dla platformy .NET Core 2.0:</span><span class="sxs-lookup"><span data-stu-id="7bad8-130">For example, here's the list of CLI tools used in a typical ASP.NET Core 2.0 project targeting .NET Core 2.0:</span></span>

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=12-16)]

<a name="package-target-fallback"></a>

## <a name="rename-package-target-fallback-property"></a><span data-ttu-id="7bad8-131">Zmień nazwę właściwości rezerwowe docelowego pakietu</span><span class="sxs-lookup"><span data-stu-id="7bad8-131">Rename Package Target Fallback property</span></span>

<span data-ttu-id="7bad8-132">*.Csproj* pliku projektu 1.x używane `PackageTargetFallback` węzła i zmiennej:</span><span class="sxs-lookup"><span data-stu-id="7bad8-132">The *.csproj* file of a 1.x project used a `PackageTargetFallback` node and variable:</span></span>

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App.csproj?range=5)]

<span data-ttu-id="7bad8-133">Zmień nazwę węzła i zmienną `AssetTargetFallback`:</span><span class="sxs-lookup"><span data-stu-id="7bad8-133">Rename both the node and variable to `AssetTargetFallback`:</span></span>

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=4)]

<a name="program-cs"></a>

## <a name="update-main-method-in-programcs"></a><span data-ttu-id="7bad8-134">Zaktualizuj metodę Main w pliku Program.cs</span><span class="sxs-lookup"><span data-stu-id="7bad8-134">Update Main method in Program.cs</span></span>

<span data-ttu-id="7bad8-135">W projektach 1.x `Main` metody *Program.cs* zapoznaniu się następująco:</span><span class="sxs-lookup"><span data-stu-id="7bad8-135">In 1.x projects, the `Main` method of *Program.cs* looked like this:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Program.cs?name=snippet_ProgramCs&highlight=8-19)]

<span data-ttu-id="7bad8-136">W projektach 2.0 `Main` metody *Program.cs* został uproszczony:</span><span class="sxs-lookup"><span data-stu-id="7bad8-136">In 2.0 projects, the `Main` method of *Program.cs* has been simplified:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Program.cs?highlight=8-11)]

<span data-ttu-id="7bad8-137">Wdrożenie tego wzorca nowe 2.0 zdecydowanie zaleca się i jest wymagana dla funkcji produktów, takich jak [migracje Core Entity Framework (EF)](xref:data/ef-mvc/migrations) do pracy.</span><span class="sxs-lookup"><span data-stu-id="7bad8-137">The adoption of this new 2.0 pattern is highly recommended and is required for product features like [Entity Framework (EF) Core Migrations](xref:data/ef-mvc/migrations) to work.</span></span> <span data-ttu-id="7bad8-138">Aby na przykład uruchomić `Update-Database` z okna konsoli Menedżera pakietów lub `dotnet ef database update` polecenia wiersza (na projekty na platformie ASP.NET Core 2.0) generuje następujący błąd:</span><span class="sxs-lookup"><span data-stu-id="7bad8-138">For example, running `Update-Database` from the Package Manager Console window or `dotnet ef database update` from the command line (on projects converted to ASP.NET Core 2.0) generates the following error:</span></span>

```
Unable to create an object of type '<Context>'. Add an implementation of 'IDesignTimeDbContextFactory<Context>' to the project, or see https://go.microsoft.com/fwlink/?linkid=851728 for additional patterns supported at design time.
```

<a name="add-modify-configuration"></a>

## <a name="add-configuration-providers"></a><span data-ttu-id="7bad8-139">Dodawanie dostawcy konfiguracji</span><span class="sxs-lookup"><span data-stu-id="7bad8-139">Add configuration providers</span></span>

<span data-ttu-id="7bad8-140">W projektach 1.x Dodawanie dostawcy konfiguracji aplikacji było wykonywane za pośrednictwem `Startup` konstruktora.</span><span class="sxs-lookup"><span data-stu-id="7bad8-140">In 1.x projects, adding configuration providers to an app was accomplished via the `Startup` constructor.</span></span> <span data-ttu-id="7bad8-141">Kroki trzeba tworzenia wystąpienia obiektu `ConfigurationBuilder`, ładowania odpowiednich dostawców (zmienne środowiskowe, ustawienia aplikacji itp.) i Inicjowanie członkiem `IConfigurationRoot`.</span><span class="sxs-lookup"><span data-stu-id="7bad8-141">The steps involved creating an instance of `ConfigurationBuilder`, loading applicable providers (environment variables, app settings, etc.), and initializing a member of `IConfigurationRoot`.</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Startup.cs?name=snippet_1xStartup)]

<span data-ttu-id="7bad8-142">Poprzedni przykład ładuje `Configuration` Członkowskie przy użyciu ustawień konfiguracyjnych z *appsettings.json* oraz dowolne *appsettings.\< EnvironmentName\>.json* pliku dopasowania `IHostingEnvironment.EnvironmentName` właściwości.</span><span class="sxs-lookup"><span data-stu-id="7bad8-142">The preceding example loads the `Configuration` member with configuration settings from *appsettings.json* as well as any *appsettings.\<EnvironmentName\>.json* file matching the `IHostingEnvironment.EnvironmentName` property.</span></span> <span data-ttu-id="7bad8-143">Lokalizacja tych plików znajduje się w tej samej ścieżce co *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="7bad8-143">The location of these files is at the same path as *Startup.cs*.</span></span>

<span data-ttu-id="7bad8-144">W 2.0 projektów standardowy kod konfiguracji zarezerwowanymi 1.x projektów działa w tle.</span><span class="sxs-lookup"><span data-stu-id="7bad8-144">In 2.0 projects, the boilerplate configuration code inherent to 1.x projects runs behind-the-scenes.</span></span> <span data-ttu-id="7bad8-145">Na przykład zmienne środowiskowe i ustawienia aplikacji są ładowane podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="7bad8-145">For example, environment variables and app settings are loaded at startup.</span></span> <span data-ttu-id="7bad8-146">Odpowiednik *Startup.cs* kodu jest ograniczona do `IConfiguration` inicjowania za pomocą wprowadzonego wystąpienie:</span><span class="sxs-lookup"><span data-stu-id="7bad8-146">The equivalent *Startup.cs* code is reduced to `IConfiguration` initialization with the injected instance:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/Startup.cs?name=snippet_2xStartup)]

<span data-ttu-id="7bad8-147">Można usunąć domyślnych dostawców dodane przez `WebHostBuilder.CreateDefaultBuilder`, wywołaj `Clear` metody `IConfigurationBuilder.Sources` właściwości wewnątrz `ConfigureAppConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="7bad8-147">To remove the default providers added by `WebHostBuilder.CreateDefaultBuilder`, invoke the `Clear` method on the `IConfigurationBuilder.Sources` property inside of `ConfigureAppConfiguration`.</span></span> <span data-ttu-id="7bad8-148">Aby dodać ponownie dostawców, korzystanie z `ConfigureAppConfiguration` method in Class metoda *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="7bad8-148">To add providers back, utilize the `ConfigureAppConfiguration` method in *Program.cs*:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/Program.cs?name=snippet_ProgramMainConfigProviders&highlight=9-14)]

<span data-ttu-id="7bad8-149">Konfiguracja używana przez `CreateDefaultBuilder` metody w poprzednim fragmencie kodu widać [tutaj](https://github.com/aspnet/MetaPackages/blob/rel/2.0.0/src/Microsoft.AspNetCore/WebHost.cs#L152).</span><span class="sxs-lookup"><span data-stu-id="7bad8-149">The configuration used by the `CreateDefaultBuilder` method in the preceding code snippet can be seen [here](https://github.com/aspnet/MetaPackages/blob/rel/2.0.0/src/Microsoft.AspNetCore/WebHost.cs#L152).</span></span>

<span data-ttu-id="7bad8-150">Aby uzyskać więcej informacji, zobacz [konfiguracji w programie ASP.NET Core](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="7bad8-150">For more information, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index).</span></span>

<a name="db-init-code"></a>

## <a name="move-database-initialization-code"></a><span data-ttu-id="7bad8-151">Przenieś kod inicjowania bazy danych</span><span class="sxs-lookup"><span data-stu-id="7bad8-151">Move database initialization code</span></span>

<span data-ttu-id="7bad8-152">W projektach 1.x przy użyciu programu EF Core 1.x, polecenia takie jak `dotnet ef migrations add` wykonuje następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="7bad8-152">In 1.x projects using EF Core 1.x, a command such as `dotnet ef migrations add` does the following:</span></span>

1. <span data-ttu-id="7bad8-153">Tworzy wystąpienie `Startup` wystąpienia</span><span class="sxs-lookup"><span data-stu-id="7bad8-153">Instantiates a `Startup` instance</span></span>
1. <span data-ttu-id="7bad8-154">Wywołuje `ConfigureServices` metody do rejestrowania wszystkich usług przy użyciu iniekcji zależności (w tym `DbContext` typów)</span><span class="sxs-lookup"><span data-stu-id="7bad8-154">Invokes the `ConfigureServices` method to register all services with dependency injection (including `DbContext` types)</span></span>
1. <span data-ttu-id="7bad8-155">Wykonuje jego wymagane zadania</span><span class="sxs-lookup"><span data-stu-id="7bad8-155">Performs its requisite tasks</span></span>

<span data-ttu-id="7bad8-156">W projektach 2.0 przy użyciu programu EF Core 2.0 `Program.BuildWebHost` wywoływana w celu uzyskania usług aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7bad8-156">In 2.0 projects using EF Core 2.0, `Program.BuildWebHost` is invoked to obtain the application services.</span></span> <span data-ttu-id="7bad8-157">W przeciwieństwie do 1.x, to ma dodatkowe efekt uboczny wywoływania `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="7bad8-157">Unlike 1.x, this has the additional side effect of invoking `Startup.Configure`.</span></span> <span data-ttu-id="7bad8-158">Jeśli aplikacja 1.x wywoływany kod inicjowania bazy danych w jego `Configure` metody, mogą wystąpić nieoczekiwane problemy.</span><span class="sxs-lookup"><span data-stu-id="7bad8-158">If your 1.x app invoked database initialization code in its `Configure` method, unexpected problems can occur.</span></span> <span data-ttu-id="7bad8-159">Na przykład jeśli jeszcze nie istnieje baza danych, rozmieszczania kod jest uruchamiany przed wykonaniem polecenia migracji programu EF Core.</span><span class="sxs-lookup"><span data-stu-id="7bad8-159">For example, if the database doesn't yet exist, the seeding code runs before the EF Core Migrations command execution.</span></span> <span data-ttu-id="7bad8-160">Ten problem powoduje, że `dotnet ef migrations list` polecenie, aby zakończyć się niepowodzeniem, jeśli baza danych jeszcze nie istnieje.</span><span class="sxs-lookup"><span data-stu-id="7bad8-160">This problem causes a `dotnet ef migrations list` command to fail if the database doesn't yet exist.</span></span>

<span data-ttu-id="7bad8-161">Rozważmy poniższy kod inicjowania inicjatora 1.x w `Configure` metody *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="7bad8-161">Consider the following 1.x seed initialization code in the `Configure` method of *Startup.cs*:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Startup.cs?name=snippet_ConfigureSeedData&highlight=8)]

<span data-ttu-id="7bad8-162">W projektach 2.0, należy przenieść `SeedData.Initialize` wywołanie `Main` metody *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="7bad8-162">In 2.0 projects, move the `SeedData.Initialize` call to the `Main` method of *Program.cs*:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Program2.cs?name=snippet_Main2Code&highlight=10)]

<span data-ttu-id="7bad8-163">Począwszy od wersji 2.0, to nic robić złym zwyczajem `BuildWebHost` z wyjątkiem kompilacji i skonfigurować hosta sieci web.</span><span class="sxs-lookup"><span data-stu-id="7bad8-163">As of 2.0, it's bad practice to do anything in `BuildWebHost` except build and configure the web host.</span></span> <span data-ttu-id="7bad8-164">Wszystko, co jest o uruchamianiu aplikacji powinno zostać obsłużone poza `BuildWebHost` &mdash; zwykle w `Main` metody *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="7bad8-164">Anything that's about running the application should be handled outside of `BuildWebHost` &mdash; typically in the `Main` method of *Program.cs*.</span></span>

<a name="view-compilation"></a>

## <a name="review-razor-view-compilation-setting"></a><span data-ttu-id="7bad8-165">Sprawdź ustawienie kompilacja widoku Razor</span><span class="sxs-lookup"><span data-stu-id="7bad8-165">Review Razor view compilation setting</span></span>

<span data-ttu-id="7bad8-166">Krótszy czas uruchamiania aplikacji i mniejsze pakiety opublikowane ma priorytetowe znaczenie dla Ciebie.</span><span class="sxs-lookup"><span data-stu-id="7bad8-166">Faster application startup time and smaller published bundles are of utmost importance to you.</span></span> <span data-ttu-id="7bad8-167">Z tego względu [kompilacja widoku Razor](xref:mvc/views/view-compilation) jest domyślnie włączona w programie ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="7bad8-167">For these reasons, [Razor view compilation](xref:mvc/views/view-compilation) is enabled by default in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="7bad8-168">Ustawienie `MvcRazorCompileOnPublish` właściwości na wartość true nie jest już wymagane.</span><span class="sxs-lookup"><span data-stu-id="7bad8-168">Setting the `MvcRazorCompileOnPublish` property to true is no longer required.</span></span> <span data-ttu-id="7bad8-169">O ile nie jest wyłączenie kompilacja widoku, właściwości mogą zostać usunięte z *.csproj* pliku.</span><span class="sxs-lookup"><span data-stu-id="7bad8-169">Unless you're disabling view compilation, the property may be removed from the *.csproj* file.</span></span>

<span data-ttu-id="7bad8-170">Gdy przeznaczonych dla platformy .NET Framework, nadal należy jawnie odwoływać się do [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation) pakietu NuGet w swojej *.csproj* pliku:</span><span class="sxs-lookup"><span data-stu-id="7bad8-170">When targeting .NET Framework, you still need to explicitly reference the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation) NuGet package in your *.csproj* file:</span></span>

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=15)]

<a name="app-insights"></a>

## <a name="rely-on-application-insights-light-up-features"></a><span data-ttu-id="7bad8-171">Zależą od funkcji "światła w górę" usługi Application Insights</span><span class="sxs-lookup"><span data-stu-id="7bad8-171">Rely on Application Insights "light-up" features</span></span>

<span data-ttu-id="7bad8-172">Ważne jest bezproblemowa ustawień Instrumentacji wydajności aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7bad8-172">Effortless setup of application performance instrumentation is important.</span></span> <span data-ttu-id="7bad8-173">Teraz możesz polegać na nowym [usługi Application Insights](/azure/application-insights/app-insights-overview) "światła w górę" Funkcje dostępne w narzędzia programu Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="7bad8-173">You can now rely on the new [Application Insights](/azure/application-insights/app-insights-overview) "light-up" features available in the Visual Studio 2017 tooling.</span></span>

<span data-ttu-id="7bad8-174">Domyślnie projekty ASP.NET Core 1.1, utworzone w programie Visual Studio 2017 dodany usługi Application Insights.</span><span class="sxs-lookup"><span data-stu-id="7bad8-174">ASP.NET Core 1.1 projects created in Visual Studio 2017 added Application Insights by default.</span></span> <span data-ttu-id="7bad8-175">Jeśli nie używasz zestawu SDK usługi Application Insights bezpośrednio, poza *Program.cs* i *Startup.cs*, wykonaj następujące kroki:</span><span class="sxs-lookup"><span data-stu-id="7bad8-175">If you're not using the Application Insights SDK directly, outside of *Program.cs* and *Startup.cs*, follow these steps:</span></span>

1. <span data-ttu-id="7bad8-176">Jeśli przeznaczony dla platformy .NET Core, należy usunąć następujące `<PackageReference />` węzła z *.csproj* pliku:</span><span class="sxs-lookup"><span data-stu-id="7bad8-176">If targeting .NET Core, remove the following `<PackageReference />` node from the *.csproj* file:</span></span>

    [!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App.csproj?range=10)]

2. <span data-ttu-id="7bad8-177">Jeśli przeznaczony dla platformy .NET Core, Usuń `UseApplicationInsights` wywołanie metody rozszerzenia z *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="7bad8-177">If targeting .NET Core, remove the `UseApplicationInsights` extension method invocation from *Program.cs*:</span></span>

    [!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Program.cs?name=snippet_ProgramCsMain&highlight=8)]

3. <span data-ttu-id="7bad8-178">Usuń wywołanie interfejsu API usługi Application Insights po stronie klienta z *_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="7bad8-178">Remove the Application Insights client-side API call from *_Layout.cshtml*.</span></span> <span data-ttu-id="7bad8-179">Obejmuje on następujące dwa wiersze kodu:</span><span class="sxs-lookup"><span data-stu-id="7bad8-179">It comprises the following two lines of code:</span></span>

    [!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Shared/_Layout.cshtml?range=1,19&dedent=4)]

<span data-ttu-id="7bad8-180">Jeśli używasz zestawu SDK usługi Application Insights bezpośrednio, nadal można to zrobić.</span><span class="sxs-lookup"><span data-stu-id="7bad8-180">If you are using the Application Insights SDK directly, continue to do so.</span></span> <span data-ttu-id="7bad8-181">W wersji 2.0 [meta Microsoft.aspnetcore.all](xref:fundamentals/metapackage) zawiera najnowszą wersję usługi Application Insights, więc pojawia się błąd starszą wersję pakietu, jeśli masz odwołujące się do starszej wersji.</span><span class="sxs-lookup"><span data-stu-id="7bad8-181">The 2.0 [metapackage](xref:fundamentals/metapackage) includes the latest version of Application Insights, so a package downgrade error appears if you're referencing an older version.</span></span>

<a name="auth-and-identity"></a>

## <a name="adopt-authenticationidentity-improvements"></a><span data-ttu-id="7bad8-182">Przyjęcie ulepszenia uwierzytelniania/tożsamości</span><span class="sxs-lookup"><span data-stu-id="7bad8-182">Adopt authentication/Identity improvements</span></span>

<span data-ttu-id="7bad8-183">Platforma ASP.NET Core 2.0 ma nowy model uwierzytelniania i Liczba znaczących zmian w tożsamości platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7bad8-183">ASP.NET Core 2.0 has a new authentication model and a number of significant changes to ASP.NET Core Identity.</span></span> <span data-ttu-id="7bad8-184">Jeśli projekt utworzony z indywidualnymi kontami użytkowników, włączona lub jeśli ręcznie dodano uwierzytelniania lub tożsamości, zobacz [migracji uwierzytelnianie i tożsamość do ASP.NET Core 2.0](xref:migration/1x-to-2x/identity-2x).</span><span class="sxs-lookup"><span data-stu-id="7bad8-184">If you created your project with Individual User Accounts enabled, or if you have manually added authentication or Identity, see [Migrate Authentication and Identity to ASP.NET Core 2.0](xref:migration/1x-to-2x/identity-2x).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7bad8-185">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="7bad8-185">Additional resources</span></span>

* [<span data-ttu-id="7bad8-186">Istotne zmiany w programie ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="7bad8-186">Breaking Changes in ASP.NET Core 2.0</span></span>](https://github.com/aspnet/announcements/issues?page=1&q=is%3Aissue+is%3Aopen+label%3A2.0.0+label%3A%22Breaking+change%22&utf8=%E2%9C%93)

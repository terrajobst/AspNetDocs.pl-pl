---
title: Szablon projektu platformy React za pomocą platformy ASP.NET Core
author: SteveSandersonMS
description: Dowiedz się, jak rozpocząć pracę przy użyciu szablonu projektu ASP.NET Core jednej strony aplikacji (SPA) dla platformy React i utworzyć react aplikacji.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 02/13/2019
uid: spa/react
ms.openlocfilehash: 3b2b2e67b5d577872bafefef5624a13ca1a22449
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57066668"
---
# <a name="use-the-react-project-template-with-aspnet-core"></a><span data-ttu-id="42e36-103">Szablon projektu platformy React za pomocą platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="42e36-103">Use the React project template with ASP.NET Core</span></span>

<span data-ttu-id="42e36-104">Zaktualizowany szablon projektu platformy React udostępnia dogodny punkt początkowy dla platformy ASP.NET Core z aplikacji przy użyciu platformy React i [tworzenie platformy react aplikacji](https://github.com/facebookincubator/create-react-app) konwencje (CRA), aby zaimplementować interfejs rozbudowane, po stronie klienta użytkownika (UI).</span><span class="sxs-lookup"><span data-stu-id="42e36-104">The updated React project template provides a convenient starting point for ASP.NET Core apps using React and [create-react-app](https://github.com/facebookincubator/create-react-app) (CRA) conventions to implement a rich, client-side user interface (UI).</span></span>

<span data-ttu-id="42e36-105">Szablon jest odpowiednikiem Tworzenie projektu ASP.NET Core jako zaplecza interfejsu API i standardowy projekt kanadyjskiej administracji podatkowej reagować na działanie jako interfejsu użytkownika, ale zapewniające wygodę hostingu, zarówno w projekcie pojedynczej aplikacji, który może być skompilowane i opublikowane jako pojedyncza jednostka.</span><span class="sxs-lookup"><span data-stu-id="42e36-105">The template is equivalent to creating both an ASP.NET Core project to act as an API backend, and a standard CRA React project to act as a UI, but with the convenience of hosting both in a single app project that can be built and published as a single unit.</span></span>

## <a name="create-a-new-app"></a><span data-ttu-id="42e36-106">Tworzenie nowej aplikacji</span><span class="sxs-lookup"><span data-stu-id="42e36-106">Create a new app</span></span>

<span data-ttu-id="42e36-107">Jeśli masz zainstalowany program ASP.NET Core 2.1, nie ma potrzeby instalowania szablon projektu platformy React.</span><span class="sxs-lookup"><span data-stu-id="42e36-107">If you have ASP.NET Core 2.1 installed, there's no need to install the React project template.</span></span>

<span data-ttu-id="42e36-108">Utwórz nowy projekt z wiersza polecenia za pomocą polecenia `dotnet new react` w pustym katalogu.</span><span class="sxs-lookup"><span data-stu-id="42e36-108">Create a new project from a command prompt using the command `dotnet new react` in an empty directory.</span></span> <span data-ttu-id="42e36-109">Na przykład, następujące polecenia tworzą aplikację w *Moje aplikacyjnego nowe* katalogu i przełącz się do tego katalogu:</span><span class="sxs-lookup"><span data-stu-id="42e36-109">For example, the following commands create the app in a *my-new-app* directory and switch to that directory:</span></span>

```console
dotnet new react -o my-new-app
cd my-new-app
```

<span data-ttu-id="42e36-110">Uruchom aplikację z programu Visual Studio lub platformy .NET Core interfejsu wiersza polecenia:</span><span class="sxs-lookup"><span data-stu-id="42e36-110">Run the app from either Visual Studio or the .NET Core CLI:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="42e36-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="42e36-111">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="42e36-112">Otwórz wygenerowany *.csproj* pliku i uruchom aplikację w zwykły z tego miejsca.</span><span class="sxs-lookup"><span data-stu-id="42e36-112">Open the generated *.csproj* file, and run the app as normal from there.</span></span>

<span data-ttu-id="42e36-113">Proces kompilacji przywraca zależności rozwiązania npm przy pierwszym uruchomieniu może potrwać kilka minut.</span><span class="sxs-lookup"><span data-stu-id="42e36-113">The build process restores npm dependencies on the first run, which can take several minutes.</span></span> <span data-ttu-id="42e36-114">Kolejne kompilacje są znacznie szybciej.</span><span class="sxs-lookup"><span data-stu-id="42e36-114">Subsequent builds are much faster.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="42e36-115">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="42e36-115">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="42e36-116">Upewnij się, że zmienna środowiskowa o nazwie `ASPNETCORE_Environment` wartością `Development`.</span><span class="sxs-lookup"><span data-stu-id="42e36-116">Ensure you have an environment variable called `ASPNETCORE_Environment` with value of `Development`.</span></span> <span data-ttu-id="42e36-117">Na Windows (w monity-PowerShell), uruchom `SET ASPNETCORE_Environment=Development`.</span><span class="sxs-lookup"><span data-stu-id="42e36-117">On Windows (in non-PowerShell prompts), run `SET ASPNETCORE_Environment=Development`.</span></span> <span data-ttu-id="42e36-118">W systemie Linux lub macOS Uruchom `export ASPNETCORE_Environment=Development`.</span><span class="sxs-lookup"><span data-stu-id="42e36-118">On Linux or macOS, run `export ASPNETCORE_Environment=Development`.</span></span>

<span data-ttu-id="42e36-119">Uruchom [kompilacji dotnet](/dotnet/core/tools/dotnet-build) Aby sprawdzić, aplikacja poprawnie się kompiluje.</span><span class="sxs-lookup"><span data-stu-id="42e36-119">Run [dotnet build](/dotnet/core/tools/dotnet-build) to verify your app builds correctly.</span></span> <span data-ttu-id="42e36-120">Przy pierwszym uruchomieniu procesu kompilacji przywraca zależności rozwiązania npm, co może zająć kilka minut.</span><span class="sxs-lookup"><span data-stu-id="42e36-120">On the first run, the build process restores npm dependencies, which can take several minutes.</span></span> <span data-ttu-id="42e36-121">Kolejne kompilacje są znacznie szybciej.</span><span class="sxs-lookup"><span data-stu-id="42e36-121">Subsequent builds are much faster.</span></span>

<span data-ttu-id="42e36-122">Uruchom [dotnet, uruchom](/dotnet/core/tools/dotnet-run) Aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="42e36-122">Run [dotnet run](/dotnet/core/tools/dotnet-run) to start the app.</span></span>

---

<span data-ttu-id="42e36-123">Szablon projektu umożliwia utworzenie aplikacji platformy ASP.NET Core i aplikacji w języku React.</span><span class="sxs-lookup"><span data-stu-id="42e36-123">The project template creates an ASP.NET Core app and a React app.</span></span> <span data-ttu-id="42e36-124">Aplikacja platformy ASP.NET Core jest przeznaczona do użytku z dostępem do danych, autoryzację i inne problemy po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="42e36-124">The ASP.NET Core app is intended to be used for data access, authorization, and other server-side concerns.</span></span> <span data-ttu-id="42e36-125">Aplikacja platformy React, znajdującej się w *ClientApp* podkatalogu, jest przeznaczona do użycia dla wszystkich obaw interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="42e36-125">The React app, residing in the *ClientApp* subdirectory, is intended to be used for all UI concerns.</span></span>

## <a name="add-pages-images-styles-modules-etc"></a><span data-ttu-id="42e36-126">Dodawanie stron, obrazów, style, moduły, itp.</span><span class="sxs-lookup"><span data-stu-id="42e36-126">Add pages, images, styles, modules, etc.</span></span>

<span data-ttu-id="42e36-127">*ClientApp* katalog jest standardowa aplikacja kanadyjskiej administracji podatkowej reagować.</span><span class="sxs-lookup"><span data-stu-id="42e36-127">The *ClientApp* directory is a standard CRA React app.</span></span> <span data-ttu-id="42e36-128">Można znaleźć w oficjalnej [dokumentacji kanadyjskiej administracji podatkowej](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="42e36-128">See the official [CRA documentation](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md) for more information.</span></span>

<span data-ttu-id="42e36-129">Istnieją drobne różnice między aplikacją platformy React, utworzony za pomocą tego szablonu i utworzonym przez kanadyjskiej administracji podatkowej. jednak możliwości aplikacji nie uległy zmianie.</span><span class="sxs-lookup"><span data-stu-id="42e36-129">There are slight differences between the React app created by this template and the one created by CRA itself; however, the app's capabilities are unchanged.</span></span> <span data-ttu-id="42e36-130">Aplikacja utworzona przez szablon zawiera [Bootstrap](https://getbootstrap.com/)— na podstawie układu oraz przykład podstawowej routingu.</span><span class="sxs-lookup"><span data-stu-id="42e36-130">The app created by the template contains a [Bootstrap](https://getbootstrap.com/)-based layout and a basic routing example.</span></span>

## <a name="install-npm-packages"></a><span data-ttu-id="42e36-131">Instalowanie pakietów npm</span><span class="sxs-lookup"><span data-stu-id="42e36-131">Install npm packages</span></span>

<span data-ttu-id="42e36-132">Aby zainstalować pakiety npm innych firm, należy użyć wiersza polecenia w *ClientApp* podkatalogu.</span><span class="sxs-lookup"><span data-stu-id="42e36-132">To install third-party npm packages, use a command prompt in the *ClientApp* subdirectory.</span></span> <span data-ttu-id="42e36-133">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="42e36-133">For example:</span></span>

```console
cd ClientApp
npm install --save <package_name>
```

## <a name="publish-and-deploy"></a><span data-ttu-id="42e36-134">Publikowanie i wdrażanie</span><span class="sxs-lookup"><span data-stu-id="42e36-134">Publish and deploy</span></span>

<span data-ttu-id="42e36-135">Na etapie opracowywania aplikacji jest uruchamiany w trybie zoptymalizowane pod kątem dla wygody deweloperów.</span><span class="sxs-lookup"><span data-stu-id="42e36-135">In development, the app runs in a mode optimized for developer convenience.</span></span> <span data-ttu-id="42e36-136">Na przykład pakiety języka JavaScript obejmują map źródeł (tak, aby podczas debugowania, możesz zobaczyć oryginalnego kodu źródłowego).</span><span class="sxs-lookup"><span data-stu-id="42e36-136">For example, JavaScript bundles include source maps (so that when debugging, you can see your original source code).</span></span> <span data-ttu-id="42e36-137">Aplikacja oczekuje JavaScript, HTML i CSS, zmiany plików na dysku i automatycznie następuje rekompilacja i ponowne załadowanie po wykryciu tych plików, zmiany.</span><span class="sxs-lookup"><span data-stu-id="42e36-137">The app watches JavaScript, HTML, and CSS file changes on disk and automatically recompiles and reloads when it sees those files change.</span></span>

<span data-ttu-id="42e36-138">W środowisku produkcyjnym obsługiwać wersję aplikacji, która jest zoptymalizowana pod kątem wydajności.</span><span class="sxs-lookup"><span data-stu-id="42e36-138">In production, serve a version of your app that's optimized for performance.</span></span> <span data-ttu-id="42e36-139">Jest ona konfigurowana automatycznie.</span><span class="sxs-lookup"><span data-stu-id="42e36-139">This is configured to happen automatically.</span></span> <span data-ttu-id="42e36-140">Podczas publikowania, konfigurację kompilacji emituje zminimalizowany, kompilacja transpiled kodu po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="42e36-140">When you publish, the build configuration emits a minified, transpiled build of your client-side code.</span></span> <span data-ttu-id="42e36-141">W przeciwieństwie do tworzenia kompilacji kompilacja w produkcji nie wymaga środowiska Node.js można zainstalować na serwerze.</span><span class="sxs-lookup"><span data-stu-id="42e36-141">Unlike the development build, the production build doesn't require Node.js to be installed on the server.</span></span>

<span data-ttu-id="42e36-142">Można użyć standardowego [metody hosting i wdrażanie platformy ASP.NET Core](xref:host-and-deploy/index).</span><span class="sxs-lookup"><span data-stu-id="42e36-142">You can use standard [ASP.NET Core hosting and deployment methods](xref:host-and-deploy/index).</span></span>

## <a name="run-the-cra-server-independently"></a><span data-ttu-id="42e36-143">Uruchom serwer kanadyjskiej administracji podatkowej niezależnie</span><span class="sxs-lookup"><span data-stu-id="42e36-143">Run the CRA server independently</span></span>

<span data-ttu-id="42e36-144">Projekt jest skonfigurowana do uruchamiania wystąpienia serwera wdrożeniowego programu kanadyjskiej administracji podatkowej w tle, gdy aplikacja platformy ASP.NET Core jest uruchamiany w trybie projektowania.</span><span class="sxs-lookup"><span data-stu-id="42e36-144">The project is configured to start its own instance of the CRA development server in the background when the ASP.NET Core app starts in development mode.</span></span> <span data-ttu-id="42e36-145">Jest to wygodne, ponieważ oznacza to, że nie trzeba ręcznie uruchomić oddzielny serwer.</span><span class="sxs-lookup"><span data-stu-id="42e36-145">This is convenient because it means you don't have to run a separate server manually.</span></span>

<span data-ttu-id="42e36-146">Brak wadą tego ustawienia domyślnego.</span><span class="sxs-lookup"><span data-stu-id="42e36-146">There's a drawback to this default setup.</span></span> <span data-ttu-id="42e36-147">Po każdej zmianie kodu C# i ASP.NET Core aplikacja wymaga ponownego uruchomienia, serwer kanadyjskiej administracji podatkowej zostanie ponownie uruchomiony.</span><span class="sxs-lookup"><span data-stu-id="42e36-147">Each time you modify your C# code and your ASP.NET Core app needs to restart, the CRA server restarts.</span></span> <span data-ttu-id="42e36-148">Kilka sekund, należy uruchomić tworzenie kopii zapasowej.</span><span class="sxs-lookup"><span data-stu-id="42e36-148">A few seconds are required to start back up.</span></span> <span data-ttu-id="42e36-149">Jeśli wprowadzasz częste C# kodu edycji i nie chcesz czekać, aż serwer kanadyjskiej administracji podatkowej ponownie uruchomić, uruchom serwer kanadyjskiej administracji podatkowej zewnętrznie, niezależnie od procesu, ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="42e36-149">If you're making frequent C# code edits and don't want to wait for the CRA server to restart, run the CRA server externally, independently of the ASP.NET Core process.</span></span> <span data-ttu-id="42e36-150">Aby to zrobić:</span><span class="sxs-lookup"><span data-stu-id="42e36-150">To do so:</span></span>

1. <span data-ttu-id="42e36-151">Dodaj *ENV* plik *ClientApp* podkatalogu z następującymi ustawieniami:</span><span class="sxs-lookup"><span data-stu-id="42e36-151">Add a *.env* file to the *ClientApp* subdirectory with the following setting:</span></span>

    ```
    BROWSER=none
    ```
    
    <span data-ttu-id="42e36-152">Uniemożliwi to przeglądarki sieci web otwierania podczas uruchamiania serwera kanadyjskiej administracji podatkowej zewnętrznie.</span><span class="sxs-lookup"><span data-stu-id="42e36-152">This will prevent your web browser from opening when starting the CRA server externally.</span></span>

2. <span data-ttu-id="42e36-153">W wierszu polecenia przejdź do *ClientApp* podkatalogu, a następnie uruchom serwera wdrożeniowego programu kanadyjskiej administracji podatkowej:</span><span class="sxs-lookup"><span data-stu-id="42e36-153">In a command prompt, switch to the *ClientApp* subdirectory, and launch the CRA development server:</span></span>

    ```console
    cd ClientApp
    npm start
    ```

3. <span data-ttu-id="42e36-154">Zmodyfikować aplikację platformy ASP.NET Core w taki sposób, aby użyć wystąpienia zewnętrznego serwera kanadyjskiej administracji podatkowej zamiast uruchamiania jednego z własnych.</span><span class="sxs-lookup"><span data-stu-id="42e36-154">Modify your ASP.NET Core app to use the external CRA server instance instead of launching one of its own.</span></span> <span data-ttu-id="42e36-155">W swojej *uruchamiania* klasy, Zastąp `spa.UseReactDevelopmentServer` wywołania następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="42e36-155">In your *Startup* class, replace the `spa.UseReactDevelopmentServer` invocation with the following:</span></span>

    ```csharp
    spa.UseProxyToSpaDevelopmentServer("http://localhost:3000");
    ```

<span data-ttu-id="42e36-156">Po uruchomieniu aplikacji platformy ASP.NET Core, nie będzie on uruchomienia serwera kanadyjskiej administracji podatkowej.</span><span class="sxs-lookup"><span data-stu-id="42e36-156">When you start your ASP.NET Core app, it won't launch a CRA server.</span></span> <span data-ttu-id="42e36-157">Wystąpienie, które można uruchomić ręcznie jest używana zamiast tego.</span><span class="sxs-lookup"><span data-stu-id="42e36-157">The instance you started manually is used instead.</span></span> <span data-ttu-id="42e36-158">To umożliwia uruchamianie i ponowne uruchamianie szybciej.</span><span class="sxs-lookup"><span data-stu-id="42e36-158">This enables it to start and restart faster.</span></span> <span data-ttu-id="42e36-159">Oczekuje się już na aplikację platformy React, aby odbudować każdorazowo.</span><span class="sxs-lookup"><span data-stu-id="42e36-159">It's no longer waiting for your React app to rebuild each time.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="42e36-160">"Renderowania po stronie serwera" nie jest obsługiwaną funkcją tego szablonu.</span><span class="sxs-lookup"><span data-stu-id="42e36-160">"Server-side rendering" is not a supported feature of this template.</span></span> <span data-ttu-id="42e36-161">Naszym celem, korzystając z tego szablonu jest spełniają zgodność z "Utwórz react-app".</span><span class="sxs-lookup"><span data-stu-id="42e36-161">Our goal with this template is to meet parity with "create-react-app".</span></span> <span data-ttu-id="42e36-162">Jako takie scenariusze i funkcje, które nie są uwzględnione w projekcie "Utwórz react-app" (na przykład SSR) nie są obsługiwane i są pozostawiane w charakterze ćwiczenia dla użytkownika.</span><span class="sxs-lookup"><span data-stu-id="42e36-162">As such, scenarios and features not included in a "create-react-app" project (such as SSR) are not supported and are left as an exercise for the user.</span></span>

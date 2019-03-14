---
title: What's new in ASP.NET Core 2.0
author: rick-anderson
description: Dowiedz się więcej o nowych funkcjach w programie ASP.NET Core 2.0.
ms.author: riande
ms.date: 07/10/2017
uid: aspnetcore-2.0
ms.openlocfilehash: a6d3179c84bfef0b15c2772e696466b88d228de5
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57075014"
---
# <a name="whats-new-in-aspnet-core-20"></a><span data-ttu-id="9a497-103">What's new in ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="9a497-103">What's new in ASP.NET Core 2.0</span></span>

<span data-ttu-id="9a497-104">W tym artykule przedstawiono najbardziej znaczących zmian w programie ASP.NET Core 2.0 wraz z łączami do odpowiedniej dokumentacji.</span><span class="sxs-lookup"><span data-stu-id="9a497-104">This article highlights the most significant changes in ASP.NET Core 2.0, with links to relevant documentation.</span></span>

## <a name="razor-pages"></a><span data-ttu-id="9a497-105">Razor Pages</span><span class="sxs-lookup"><span data-stu-id="9a497-105">Razor Pages</span></span>

<span data-ttu-id="9a497-106">Strony razor jest to nowa funkcja usługi ASP.NET Core MVC, która sprawia, że kodowania skoncentrowane na stronie scenariuszy łatwiejsze i bardziej wydajne.</span><span class="sxs-lookup"><span data-stu-id="9a497-106">Razor Pages is a new feature of ASP.NET Core MVC that makes coding page-focused scenarios easier and more productive.</span></span>

<span data-ttu-id="9a497-107">Aby uzyskać więcej informacji zobacz wprowadzenie i samouczków:</span><span class="sxs-lookup"><span data-stu-id="9a497-107">For more information, see the introduction and tutorial:</span></span>

* [<span data-ttu-id="9a497-108">Wprowadzenie do produktu Razor Pages</span><span class="sxs-lookup"><span data-stu-id="9a497-108">Introduction to Razor Pages</span></span>](xref:razor-pages/index)
* [<span data-ttu-id="9a497-109">Wprowadzenie do korzystania ze stron Razor</span><span class="sxs-lookup"><span data-stu-id="9a497-109">Get started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)

## <a name="aspnet-core-metapackage"></a><span data-ttu-id="9a497-110">Meta Microsoft.aspnetcore.all platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9a497-110">ASP.NET Core metapackage</span></span>

<span data-ttu-id="9a497-111">Nowe meta Microsoft.aspnetcore.all platformy ASP.NET Core obejmuje wszystkie pakiety wprowadzone i obsługiwane przez zespoły platformy ASP.NET Core i Entity Framework Core, wraz z ich zależnościami wewnętrznych i innych firm 3.</span><span class="sxs-lookup"><span data-stu-id="9a497-111">A new ASP.NET Core metapackage includes all of the packages made and supported by the ASP.NET Core and Entity Framework Core teams, along with their internal and 3rd-party dependencies.</span></span> <span data-ttu-id="9a497-112">Nie trzeba wybrać poszczególne platformy ASP.NET Core funkcje przez pakiet.</span><span class="sxs-lookup"><span data-stu-id="9a497-112">You no longer need to choose individual ASP.NET Core features by package.</span></span> <span data-ttu-id="9a497-113">Wszystkie funkcje są uwzględnione w [pakiet](https://www.nuget.org/packages/Microsoft.AspNetCore.All) pakietu.</span><span class="sxs-lookup"><span data-stu-id="9a497-113">All features are included in the [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) package.</span></span> <span data-ttu-id="9a497-114">Szablony domyślne używają tego pakietu.</span><span class="sxs-lookup"><span data-stu-id="9a497-114">The default templates use this package.</span></span>

<span data-ttu-id="9a497-115">Aby uzyskać więcej informacji, zobacz [pakiet meta Microsoft.aspnetcore.all dla programu ASP.NET Core 2.0](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="9a497-115">For more information, see [Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.0](xref:fundamentals/metapackage).</span></span>

## <a name="runtime-store"></a><span data-ttu-id="9a497-116">Środowisko uruchomieniowe Store</span><span class="sxs-lookup"><span data-stu-id="9a497-116">Runtime Store</span></span>

<span data-ttu-id="9a497-117">Aplikacje, które używają `Microsoft.AspNetCore.All` meta Microsoft.aspnetcore.all automatycznie wykorzystywać nowe Store środowiska uruchomieniowego programu .NET Core.</span><span class="sxs-lookup"><span data-stu-id="9a497-117">Applications that use the `Microsoft.AspNetCore.All` metapackage automatically take advantage of the new .NET Core Runtime Store.</span></span> <span data-ttu-id="9a497-118">Store zawiera wszystkie zasoby środowiska uruchomieniowego, wymagane do uruchamiania aplikacji ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="9a497-118">The Store contains all the runtime assets needed to run ASP.NET Core 2.0 applications.</span></span> <span data-ttu-id="9a497-119">Kiedy używasz `Microsoft.AspNetCore.All` meta Microsoft.aspnetcore.all, żadne zasoby z przywoływanych pakietów dla platformy ASP.NET Core NuGet są wdrażane przy użyciu aplikacji, ponieważ są one już przechowywane w systemie docelowym.</span><span class="sxs-lookup"><span data-stu-id="9a497-119">When you use the `Microsoft.AspNetCore.All` metapackage, no assets from the referenced ASP.NET Core NuGet packages are deployed with the application because they already reside on the target system.</span></span> <span data-ttu-id="9a497-120">Zasoby w Store środowiska uruchomieniowego są również wstępnie skompilowanych, aby poprawić czas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9a497-120">The assets in the Runtime Store are also precompiled to improve application startup time.</span></span>

<span data-ttu-id="9a497-121">Aby uzyskać więcej informacji, zobacz [magazynu środowiska uruchomieniowego](/dotnet/core/deploying/runtime-store)</span><span class="sxs-lookup"><span data-stu-id="9a497-121">For more information, see [Runtime store](/dotnet/core/deploying/runtime-store)</span></span>

## <a name="net-standard-20"></a><span data-ttu-id="9a497-122">.NET Standard 2.0</span><span class="sxs-lookup"><span data-stu-id="9a497-122">.NET Standard 2.0</span></span>

<span data-ttu-id="9a497-123">Pakiety platformy ASP.NET Core 2.0 docelowych .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="9a497-123">The ASP.NET Core 2.0 packages target .NET Standard 2.0.</span></span> <span data-ttu-id="9a497-124">Pakiety mogą być przywoływane przez inne biblioteki .NET Standard 2.0 i mogą być uruchamiane na .NET Standard 2.0 zgodne implementacji .NET, w tym .NET Core 2.0 i .NET Framework 4.6.1.</span><span class="sxs-lookup"><span data-stu-id="9a497-124">The packages can be referenced by other .NET Standard 2.0 libraries, and they can run on .NET Standard 2.0-compliant implementations of .NET, including .NET Core 2.0 and .NET Framework 4.6.1.</span></span> 

<span data-ttu-id="9a497-125">`Microsoft.AspNetCore.All` Meta Microsoft.aspnetcore.all jest przeznaczony dla platformy .NET Core 2.0, ponieważ jest ono przeznaczone do użycia z Store środowiska uruchomieniowego programu .NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="9a497-125">The `Microsoft.AspNetCore.All` metapackage targets .NET Core 2.0 only, because it's intended to be used with the .NET Core 2.0 Runtime Store.</span></span>

## <a name="configuration-update"></a><span data-ttu-id="9a497-126">Aktualizacja konfiguracji</span><span class="sxs-lookup"><span data-stu-id="9a497-126">Configuration update</span></span>

<span data-ttu-id="9a497-127">`IConfiguration` Wystąpienia zostanie dodany do kontenera usług, domyślnie w programie ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="9a497-127">An `IConfiguration` instance is added to the services container by default in ASP.NET Core 2.0.</span></span> <span data-ttu-id="9a497-128">`IConfiguration` w usługach kontenerów ułatwia dla aplikacji, które można pobrać wartości konfiguracji z kontenera.</span><span class="sxs-lookup"><span data-stu-id="9a497-128">`IConfiguration` in the services container makes it easier for applications to retrieve configuration values from the container.</span></span>

<span data-ttu-id="9a497-129">Aby uzyskać informacje o stanie planowanej dokumentacji, zobacz [problem w usłudze GitHub](https://github.com/aspnet/Docs/issues/3387).</span><span class="sxs-lookup"><span data-stu-id="9a497-129">For information about the status of planned documentation, see the [GitHub issue](https://github.com/aspnet/Docs/issues/3387).</span></span>

## <a name="logging-update"></a><span data-ttu-id="9a497-130">Rejestrowanie aktualizacji</span><span class="sxs-lookup"><span data-stu-id="9a497-130">Logging update</span></span>

<span data-ttu-id="9a497-131">W programie ASP.NET Core 2.0 rejestrowania jest włączona w systemie (DI) iniekcji zależności domyślnie.</span><span class="sxs-lookup"><span data-stu-id="9a497-131">In ASP.NET Core 2.0, logging is incorporated into the dependency injection (DI) system by default.</span></span> <span data-ttu-id="9a497-132">Dodawanie dostawcy i Konfigurowanie filtrowania w *Program.cs* zamiast w pliku *Startup.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="9a497-132">You add providers and configure filtering in the *Program.cs* file instead of in the *Startup.cs* file.</span></span> <span data-ttu-id="9a497-133">I domyślnego `ILoggerFactory` obsługuje filtrowanie w sposób, który pozwala na użytek jeden elastyczne podejście filtrowanie krzyżowe dostawcy i filtrowanie określonego dostawcy.</span><span class="sxs-lookup"><span data-stu-id="9a497-133">And the default `ILoggerFactory` supports filtering in a way that lets you use one flexible approach for both cross-provider filtering and specific-provider filtering.</span></span>

<span data-ttu-id="9a497-134">Aby uzyskać więcej informacji, zobacz [wprowadzenie do rejestrowania](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="9a497-134">For more information, see [Introduction to Logging](xref:fundamentals/logging/index).</span></span>

## <a name="authentication-update"></a><span data-ttu-id="9a497-135">Aktualizacja uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="9a497-135">Authentication update</span></span>

<span data-ttu-id="9a497-136">Nowy model uwierzytelniania ułatwia skonfigurowanie uwierzytelniania dla aplikacji przy użyciu DI.</span><span class="sxs-lookup"><span data-stu-id="9a497-136">A new authentication model makes it easier to configure authentication for an application using DI.</span></span>

<span data-ttu-id="9a497-137">Nowe szablony są dostępne do konfigurowania uwierzytelniania dla aplikacji sieci web i interfejsów API za pomocą [Azure AD B2C] (https://azure.microsoft.com/services/active-directory-b2c/).</span><span class="sxs-lookup"><span data-stu-id="9a497-137">New templates are available for configuring authentication for web apps and web APIs using [Azure AD B2C] (https://azure.microsoft.com/services/active-directory-b2c/).</span></span>

<span data-ttu-id="9a497-138">Aby uzyskać informacje o stanie planowanej dokumentacji, zobacz [problem w usłudze GitHub](https://github.com/aspnet/Docs/issues/3054).</span><span class="sxs-lookup"><span data-stu-id="9a497-138">For information about the status of planned documentation, see the [GitHub issue](https://github.com/aspnet/Docs/issues/3054).</span></span>

## <a name="identity-update"></a><span data-ttu-id="9a497-139">Aktualizacja tożsamości</span><span class="sxs-lookup"><span data-stu-id="9a497-139">Identity update</span></span>

<span data-ttu-id="9a497-140">Wprowadziliśmy je łatwiej tworzyć bezpieczne internetowych interfejsów API przy użyciu tożsamości w programie ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="9a497-140">We've made it easier to build secure web APIs using Identity in ASP.NET Core 2.0.</span></span> <span data-ttu-id="9a497-141">Możesz uzyskać tokenów dostępu do uzyskiwania dostępu do sieci web API za pomocą [Microsoft Authentication Library (MSAL)](https://www.nuget.org/packages/Microsoft.Identity.Client).</span><span class="sxs-lookup"><span data-stu-id="9a497-141">You can acquire access tokens for accessing your web APIs using the [Microsoft Authentication Library (MSAL)](https://www.nuget.org/packages/Microsoft.Identity.Client).</span></span>

<span data-ttu-id="9a497-142">Aby uzyskać więcej informacji na temat zmian uwierzytelniania w wersji 2.0 zobacz następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="9a497-142">For more information on authentication changes in 2.0, see the following resources:</span></span>

* [<span data-ttu-id="9a497-143">Potwierdzenie konta i odzyskiwanie hasła w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9a497-143">Account confirmation and password recovery in ASP.NET Core</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="9a497-144">Włączanie generowania kodu QR dla wystawcy uwierzytelnienia aplikacji w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9a497-144">Enable QR Code generation for authenticator apps in ASP.NET Core</span></span>](xref:security/authentication/identity-enable-qrcodes)
* [<span data-ttu-id="9a497-145">Migrowanie do platformy ASP.NET Core 2.0 uwierzytelniania i tożsamości</span><span class="sxs-lookup"><span data-stu-id="9a497-145">Migrate Authentication and Identity to ASP.NET Core 2.0</span></span>](xref:migration/1x-to-2x/identity-2x)

## <a name="spa-templates"></a><span data-ttu-id="9a497-146">Szablonów aplikacji JEDNOSTRONICOWYCH</span><span class="sxs-lookup"><span data-stu-id="9a497-146">SPA templates</span></span>

<span data-ttu-id="9a497-147">Pojedynczy szablony projektu strona aplikacji (SPA) szablonów Angular, Aurelia, struktura Knockout.js, React.js i React.js z kontenera Redux są dostępne.</span><span class="sxs-lookup"><span data-stu-id="9a497-147">Single Page Application (SPA) project templates for Angular, Aurelia, Knockout.js, React.js, and React.js with Redux are available.</span></span> <span data-ttu-id="9a497-148">Platformy Angular szablon został uaktualniony do Angular 4.</span><span class="sxs-lookup"><span data-stu-id="9a497-148">The Angular template has been updated to Angular 4.</span></span> <span data-ttu-id="9a497-149">Szablony usługi Angular i języka React domyślnie są dostępne; Aby dowiedzieć się, jak uzyskać innych szablonów, zobacz [Utwórz nowy projekt SPA](xref:client-side/spa-services#creating-a-new-project).</span><span class="sxs-lookup"><span data-stu-id="9a497-149">The Angular and React templates are available by default; for information about how to get the other templates, see [Create a new SPA project](xref:client-side/spa-services#creating-a-new-project).</span></span> <span data-ttu-id="9a497-150">Aby uzyskać informacje dotyczące sposobu tworzenia SPA w programie ASP.NET Core, zobacz [Użyj użyciem technologii JavaScriptServices dla tworzenia aplikacje jednostronicowe](xref:client-side/spa-services).</span><span class="sxs-lookup"><span data-stu-id="9a497-150">For information about how to build a SPA in ASP.NET Core, see [Use JavaScriptServices for Creating Single Page Applications](xref:client-side/spa-services).</span></span>

## <a name="kestrel-improvements"></a><span data-ttu-id="9a497-151">Ulepszenia kestrel</span><span class="sxs-lookup"><span data-stu-id="9a497-151">Kestrel improvements</span></span>

<span data-ttu-id="9a497-152">Serwer sieci web Kestrel ma nowe funkcje, które lepiej przystosować na serwerze dostępnym z Internetu.</span><span class="sxs-lookup"><span data-stu-id="9a497-152">The Kestrel web server has new features that make it more suitable as an Internet-facing server.</span></span> <span data-ttu-id="9a497-153">Liczba opcji konfiguracji ograniczenia serwera zostały dodane w `KestrelServerOptions` klasy przez nowe `Limits` właściwości.</span><span class="sxs-lookup"><span data-stu-id="9a497-153">A number of server constraint configuration options are added in the `KestrelServerOptions` class's new `Limits` property.</span></span> <span data-ttu-id="9a497-154">Dodaj następujące limity:</span><span class="sxs-lookup"><span data-stu-id="9a497-154">Add limits for the following:</span></span>

- <span data-ttu-id="9a497-155">Maksymalna liczba połączeń klientów</span><span class="sxs-lookup"><span data-stu-id="9a497-155">Maximum client connections</span></span>
- <span data-ttu-id="9a497-156">Rozmiar treści żądania maksymalna</span><span class="sxs-lookup"><span data-stu-id="9a497-156">Maximum request body size</span></span>
- <span data-ttu-id="9a497-157">Szybkość danych treści żądania minimalne</span><span class="sxs-lookup"><span data-stu-id="9a497-157">Minimum request body data rate</span></span>

<span data-ttu-id="9a497-158">Aby uzyskać więcej informacji, zobacz [implementacji serwera sieci web Kestrel w programie ASP.NET Core](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="9a497-158">For more information, see [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel).</span></span>

## <a name="weblistener-renamed-to-httpsys"></a><span data-ttu-id="9a497-159">Zmieniono nazwę pliku HTTP.sys WebListener</span><span class="sxs-lookup"><span data-stu-id="9a497-159">WebListener renamed to HTTP.sys</span></span>

<span data-ttu-id="9a497-160">Pakiety `Microsoft.AspNetCore.Server.WebListener` i `Microsoft.Net.Http.Server` została scalona nowy pakiet `Microsoft.AspNetCore.Server.HttpSys`.</span><span class="sxs-lookup"><span data-stu-id="9a497-160">The packages `Microsoft.AspNetCore.Server.WebListener` and `Microsoft.Net.Http.Server` have been merged into a new package `Microsoft.AspNetCore.Server.HttpSys`.</span></span> <span data-ttu-id="9a497-161">Przestrzenie nazw zostały zaktualizowane do dopasowania.</span><span class="sxs-lookup"><span data-stu-id="9a497-161">The namespaces have been updated to match.</span></span>

<span data-ttu-id="9a497-162">Aby uzyskać więcej informacji, zobacz [implementacji serwera sieci web HTTP.sys, w programie ASP.NET Core](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="9a497-162">For more information, see [HTTP.sys web server implementation in ASP.NET Core](xref:fundamentals/servers/httpsys).</span></span>

## <a name="enhanced-http-header-support"></a><span data-ttu-id="9a497-163">Ulepszona obsługa nagłówków HTTP</span><span class="sxs-lookup"><span data-stu-id="9a497-163">Enhanced HTTP header support</span></span>

<span data-ttu-id="9a497-164">Podczas przesyłania przy użyciu platformy MVC `FileStreamResult` lub `FileContentResult`, masz teraz możliwość ustawienia `ETag` lub `LastModified` daty dla zawartości, przesyłania.</span><span class="sxs-lookup"><span data-stu-id="9a497-164">When using MVC to transmit a `FileStreamResult` or a `FileContentResult`, you now have the option to set an `ETag` or a `LastModified` date on the content you transmit.</span></span> <span data-ttu-id="9a497-165">Można ustawić następujące wartości w zwracanej zawartości przy użyciu kodu podobne do następujących:</span><span class="sxs-lookup"><span data-stu-id="9a497-165">You can set these values on the returned content with code similar to the following:</span></span>

```csharp
var data = Encoding.UTF8.GetBytes("This is a sample text from a binary array");
var entityTag = new EntityTagHeaderValue("\"MyCalculatedEtagValue\"");
return File(data, "text/plain", "downloadName.txt", lastModified: DateTime.UtcNow.AddSeconds(-5), entityTag: entityTag);
```

<span data-ttu-id="9a497-166">Pliku zwrócone do odwiedzających będzie posiadać odpowiednie nagłówki HTTP dla `ETag` i `LastModified` wartości.</span><span class="sxs-lookup"><span data-stu-id="9a497-166">The file returned to your visitors will be decorated with the appropriate HTTP headers for the `ETag` and `LastModified` values.</span></span>

<span data-ttu-id="9a497-167">Jeśli aplikacja obiektu odwiedzającego zażąda zawartości z nagłówkiem żądania zakresu, ASP.NET Core rozpoznaje żądania i obsługuje nagłówka.</span><span class="sxs-lookup"><span data-stu-id="9a497-167">If an application visitor requests content with a Range Request header, ASP.NET Core recognizes the request and handles the header.</span></span> <span data-ttu-id="9a497-168">Żądanej zawartości mogą być dostarczane częściowo, ASP.NET Core odpowiednio pomija i zwraca tylko żądany zestaw bajtów.</span><span class="sxs-lookup"><span data-stu-id="9a497-168">If the requested content can be partially delivered, ASP.NET Core appropriately skips and returns just the requested set of bytes.</span></span> <span data-ttu-id="9a497-169">Nie musisz tworzyć żadnych specjalnych obsługi do Twojej metody, aby dostosować lub obsługi tej funkcji; odbywa się automatycznie dla Ciebie.</span><span class="sxs-lookup"><span data-stu-id="9a497-169">You don't need to write any special handlers into your methods to adapt or handle this feature; it's automatically handled for you.</span></span>

## <a name="hosting-startup-and-application-insights"></a><span data-ttu-id="9a497-170">Uruchamianie hostingu i usługi Application Insights</span><span class="sxs-lookup"><span data-stu-id="9a497-170">Hosting startup and Application Insights</span></span>

<span data-ttu-id="9a497-171">Środowisk hostingowych teraz wstrzykiwanie zależności dodatkowych pakietów i wykonanie kodu podczas uruchamiania aplikacji bez zastosowania konieczności jawnego wiedziały lub wywołanie dowolnej metody.</span><span class="sxs-lookup"><span data-stu-id="9a497-171">Hosting environments can now inject extra package dependencies and execute code during application startup, without the application needing to explicitly take a dependency or call any methods.</span></span> <span data-ttu-id="9a497-172">Tę funkcję można włączyć niektórych środowisk "światła w górę" funkcji unikatowych dla danego środowiska bez aplikacji musi wiedzieć z wyprzedzeniem.</span><span class="sxs-lookup"><span data-stu-id="9a497-172">This feature can be used to enable certain environments to "light-up" features unique to that environment without the application needing to know ahead of time.</span></span> 

<span data-ttu-id="9a497-173">W programie ASP.NET Core 2.0, ta funkcja jest używana, można automatycznie włączyć diagnostykę usługi Application Insights podczas debugowania w programie Visual Studio i (po zgodzie na rozwiązanie) podczas uruchamiania w usłudze Azure App Services.</span><span class="sxs-lookup"><span data-stu-id="9a497-173">In ASP.NET Core 2.0, this feature is used to automatically enable Application Insights diagnostics when debugging in Visual Studio and (after opting in) when running in Azure App Services.</span></span> <span data-ttu-id="9a497-174">W rezultacie szablony projektów nie jest już dodać pakiety usługi Application Insights i kod domyślnie.</span><span class="sxs-lookup"><span data-stu-id="9a497-174">As a result, the project templates no longer add Application Insights packages and code by default.</span></span>

<span data-ttu-id="9a497-175">Aby uzyskać informacje o stanie planowanej dokumentacji, zobacz [problem w usłudze GitHub](https://github.com/aspnet/Docs/issues/3389).</span><span class="sxs-lookup"><span data-stu-id="9a497-175">For information about the status of planned documentation, see the [GitHub issue](https://github.com/aspnet/Docs/issues/3389).</span></span>

## <a name="automatic-use-of-anti-forgery-tokens"></a><span data-ttu-id="9a497-176">Automatyczne użycie tokeny zabezpieczające przed fałszerstwem</span><span class="sxs-lookup"><span data-stu-id="9a497-176">Automatic use of anti-forgery tokens</span></span>

<span data-ttu-id="9a497-177">Platforma ASP.NET Core zawsze pomogła kodowanie HTML zawartości domyślnie, ale z nową wersją dodatkowego kroku jest podjęte w celu pomocy atakami fałszerstwo żądania międzywitrynowego (XSRF).</span><span class="sxs-lookup"><span data-stu-id="9a497-177">ASP.NET Core has always helped HTML-encode content by default, but with the new version an extra step is taken to help prevent cross-site request forgery (XSRF) attacks.</span></span> <span data-ttu-id="9a497-178">Platforma ASP.NET Core teraz emitować tokeny zabezpieczające przed fałszerstwem domyślnie i sprawdzić ich poprawność akcje POST formularza i stron bez dodatkowej konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="9a497-178">ASP.NET Core will now emit anti-forgery tokens by default and validate them on form POST actions and pages without extra configuration.</span></span>

<span data-ttu-id="9a497-179">Aby uzyskać więcej informacji, zobacz [zapobiec Cross-Site Request Forgery (XSRF/CSRF) ataki](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="9a497-179">For more information, see [Prevent Cross-Site Request Forgery (XSRF/CSRF) attacks](xref:security/anti-request-forgery).</span></span>

## <a name="automatic-precompilation"></a><span data-ttu-id="9a497-180">Automatyczne wstępnej kompilacji</span><span class="sxs-lookup"><span data-stu-id="9a497-180">Automatic precompilation</span></span>

<span data-ttu-id="9a497-181">Wstępna kompilacja widoku razor jest domyślnie podczas publikowania, zmniejszając rozmiar danych wyjściowych publikowania i aplikacji czas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="9a497-181">Razor view pre-compilation is enabled during publish by default, reducing the publish output size and application startup time.</span></span>

<span data-ttu-id="9a497-182">Aby uzyskać więcej informacji, zobacz [kompilacja widoku Razor i wstępnej kompilacji w programie ASP.NET Core](xref:mvc/views/view-compilation).</span><span class="sxs-lookup"><span data-stu-id="9a497-182">For more information, see [Razor view compilation and precompilation in ASP.NET Core](xref:mvc/views/view-compilation).</span></span>

## <a name="razor-support-for-c-71"></a><span data-ttu-id="9a497-183">Obsługa razor dla języka C# 7.1</span><span class="sxs-lookup"><span data-stu-id="9a497-183">Razor support for C# 7.1</span></span>

<span data-ttu-id="9a497-184">Aparat widoku Razor został zaktualizowany do pracy z nowy kompilator Roslyn.</span><span class="sxs-lookup"><span data-stu-id="9a497-184">The Razor view engine has been updated to work with the new Roslyn compiler.</span></span> <span data-ttu-id="9a497-185">Obejmuje to obsługę funkcji języka C# 7.1 jak domyślne wyrażeń, wywnioskowane nazwy krotki i dopasowania do wzorca za pomocą typów ogólnych.</span><span class="sxs-lookup"><span data-stu-id="9a497-185">That includes support for C# 7.1 features like Default Expressions, Inferred Tuple Names, and Pattern-Matching with Generics.</span></span> <span data-ttu-id="9a497-186">Aby korzystać z języka C# 7.1 w projekcie, dodaj następującą właściwość w pliku projektu, a następnie załaduj ponownie rozwiązanie:</span><span class="sxs-lookup"><span data-stu-id="9a497-186">To use C# 7.1 in your project, add the following property in your project file and then reload the solution:</span></span>

```xml
<LangVersion>latest</LangVersion>
```

<span data-ttu-id="9a497-187">Aby uzyskać informacje o stanie funkcji języka C# 7.1 zobacz [repozytorium GitHub platformy Roslyn](https://github.com/dotnet/roslyn/blob/master/docs/Language%20Feature%20Status.md).</span><span class="sxs-lookup"><span data-stu-id="9a497-187">For information about the status of C# 7.1 features, see [the Roslyn GitHub repository](https://github.com/dotnet/roslyn/blob/master/docs/Language%20Feature%20Status.md).</span></span>

## <a name="other-documentation-updates-for-20"></a><span data-ttu-id="9a497-188">Inne aktualizacje dokumentacji dla wersji 2.0</span><span class="sxs-lookup"><span data-stu-id="9a497-188">Other documentation updates for 2.0</span></span>

* [<span data-ttu-id="9a497-189">Program Visual Studio publikowania profile na potrzeby wdrażania aplikacji platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9a497-189">Visual Studio publish profiles for ASP.NET Core app deployment</span></span>](xref:host-and-deploy/visual-studio-publish-profiles)
* [<span data-ttu-id="9a497-190">Zarządzanie kluczami</span><span class="sxs-lookup"><span data-stu-id="9a497-190">Key Management</span></span>](xref:security/data-protection/implementation/key-management)
* [<span data-ttu-id="9a497-191">Konfigurowanie uwierzytelniania serwisu Facebook</span><span class="sxs-lookup"><span data-stu-id="9a497-191">Configure Facebook authentication</span></span>](xref:security/authentication/facebook-logins)
* [<span data-ttu-id="9a497-192">Konfigurowanie uwierzytelniania usługi Twitter</span><span class="sxs-lookup"><span data-stu-id="9a497-192">Configure Twitter authentication</span></span>](xref:security/authentication/twitter-logins)
* [<span data-ttu-id="9a497-193">Konfigurowanie uwierzytelniania serwisu Google</span><span class="sxs-lookup"><span data-stu-id="9a497-193">Configure Google authentication</span></span>](xref:security/authentication/google-logins)
* [<span data-ttu-id="9a497-194">Konfigurowanie uwierzytelniania Account firmy Microsoft</span><span class="sxs-lookup"><span data-stu-id="9a497-194">Configure Microsoft Account authentication</span></span>](xref:security/authentication/microsoft-logins)

## <a name="migration-guidance"></a><span data-ttu-id="9a497-195">Wskazówki dotyczące migracji</span><span class="sxs-lookup"><span data-stu-id="9a497-195">Migration guidance</span></span>

<span data-ttu-id="9a497-196">Aby uzyskać wskazówki dotyczące sposobu przeprowadzenia migracji platformy ASP.NET Core 1.x aplikacji ASP.NET Core 2.0 zobacz następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="9a497-196">For guidance on how to migrate ASP.NET Core 1.x applications to ASP.NET Core 2.0, see the following resources:</span></span>

* [<span data-ttu-id="9a497-197">Migracja z programu ASP.NET Core 1.x do platformy ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="9a497-197">Migrate from ASP.NET Core 1.x to ASP.NET Core 2.0</span></span>](xref:migration/1x-to-2x/index)
* [<span data-ttu-id="9a497-198">Migrowanie do platformy ASP.NET Core 2.0 uwierzytelniania i tożsamości</span><span class="sxs-lookup"><span data-stu-id="9a497-198">Migrate Authentication and Identity to ASP.NET Core 2.0</span></span>](xref:migration/1x-to-2x/identity-2x)

## <a name="additional-information"></a><span data-ttu-id="9a497-199">Dodatkowe informacje</span><span class="sxs-lookup"><span data-stu-id="9a497-199">Additional Information</span></span>

<span data-ttu-id="9a497-200">Aby uzyskać pełną listę zmian, zobacz [platformy ASP.NET Core w wersji 2.0 — informacje o wersji](https://github.com/aspnet/Home/releases/tag/2.0.0).</span><span class="sxs-lookup"><span data-stu-id="9a497-200">For the complete list of changes, see the [ASP.NET Core 2.0 Release Notes](https://github.com/aspnet/Home/releases/tag/2.0.0).</span></span>

<span data-ttu-id="9a497-201">Aby połączyć się z postępach i planach zespołu projektowego ASP.NET Core, obejrzyj Konferencję [ASP.NET Community Standup](https://live.asp.net/).</span><span class="sxs-lookup"><span data-stu-id="9a497-201">To connect with the ASP.NET Core development team's progress and plans, tune in to the [ASP.NET Community Standup](https://live.asp.net/).</span></span>

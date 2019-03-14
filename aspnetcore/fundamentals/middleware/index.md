---
title: ASP.NET Core Middleware
author: rick-anderson
description: Więcej informacji na temat oprogramowania pośredniczącego platformy ASP.NET Core i potok żądań.
ms.author: riande
ms.custom: mvc
ms.date: 02/17/2019
uid: fundamentals/middleware/index
---
# <a name="aspnet-core-middleware"></a><span data-ttu-id="9947d-103">ASP.NET Core Middleware</span><span class="sxs-lookup"><span data-stu-id="9947d-103">ASP.NET Core Middleware</span></span>

<span data-ttu-id="9947d-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="9947d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="9947d-105">Oprogramowanie pośredniczące to oprogramowanie, które jest umieszczone w potoku aplikacji do obsługi żądań i odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="9947d-105">Middleware is software that's assembled into an app pipeline to handle requests and responses.</span></span> <span data-ttu-id="9947d-106">Poszczególne składniki:</span><span class="sxs-lookup"><span data-stu-id="9947d-106">Each component:</span></span>

* <span data-ttu-id="9947d-107">Pozwala wybrać, czy przekazywać żądania do następnego składnika w potoku.</span><span class="sxs-lookup"><span data-stu-id="9947d-107">Chooses whether to pass the request to the next component in the pipeline.</span></span>
* <span data-ttu-id="9947d-108">Można wykonać pracy przed i po nim następny składnik w potoku.</span><span class="sxs-lookup"><span data-stu-id="9947d-108">Can perform work before and after the next component in the pipeline.</span></span>

<span data-ttu-id="9947d-109">Delegaty żądania są używane do tworzenia potoku żądania.</span><span class="sxs-lookup"><span data-stu-id="9947d-109">Request delegates are used to build the request pipeline.</span></span> <span data-ttu-id="9947d-110">Delegaty żądania obsługi danego żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="9947d-110">The request delegates handle each HTTP request.</span></span>

<span data-ttu-id="9947d-111">Żądania, obiekty delegowane są skonfigurowane przy użyciu <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>, <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*>, i <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*> metody rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="9947d-111">Request delegates are configured using <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>, <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*>, and <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*> extension methods.</span></span> <span data-ttu-id="9947d-112">Delegat pojedynczego żądania może być określony w tekście jako metodę anonimową (nazywane w tekście oprogramowania pośredniczącego), lub można zdefiniować klasy wielokrotnego użytku.</span><span class="sxs-lookup"><span data-stu-id="9947d-112">An individual request delegate can be specified in-line as an anonymous method (called in-line middleware), or it can be defined in a reusable class.</span></span> <span data-ttu-id="9947d-113">Te klasy wielokrotnego użytku i metod anonimowych w tekście są *oprogramowania pośredniczącego*, nazywane również *składników oprogramowania pośredniczącego*.</span><span class="sxs-lookup"><span data-stu-id="9947d-113">These reusable classes and in-line anonymous methods are *middleware*, also called *middleware components*.</span></span> <span data-ttu-id="9947d-114">Każdy składnik oprogramowania pośredniczącego w potoku żądania jest odpowiedzialny za wywoływanie następny składnik w potoku lub zwarcie potoku.</span><span class="sxs-lookup"><span data-stu-id="9947d-114">Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline or short-circuiting the pipeline.</span></span> <span data-ttu-id="9947d-115">Gdy short-circuits oprogramowanie pośredniczące, jest on nazywany *terminalu oprogramowania pośredniczącego* ponieważ uniemożliwia dalsze oprogramowania pośredniczącego przetwarzania żądania.</span><span class="sxs-lookup"><span data-stu-id="9947d-115">When a middleware short-circuits, it's called a *terminal middleware* because it prevents further middleware from processing the request.</span></span>

<span data-ttu-id="9947d-116"><xref:migration/http-modules> wyjaśnia różnicę pomiędzy potoki żądania w programie ASP.NET Core i ASP.NET 4.x i udostępnia więcej przykładów oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="9947d-116"><xref:migration/http-modules> explains the difference between request pipelines in ASP.NET Core and ASP.NET 4.x and provides more middleware samples.</span></span>

## <a name="create-a-middleware-pipeline-with-iapplicationbuilder"></a><span data-ttu-id="9947d-117">Tworzenie potoku oprogramowania pośredniczącego z IApplicationBuilder</span><span class="sxs-lookup"><span data-stu-id="9947d-117">Create a middleware pipeline with IApplicationBuilder</span></span>

<span data-ttu-id="9947d-118">Potok żądań ASP.NET Core składa się z sekwencji obiektów delegowanych żądania, o nazwie jedna po drugiej.</span><span class="sxs-lookup"><span data-stu-id="9947d-118">The ASP.NET Core request pipeline consists of a sequence of request delegates, called one after the other.</span></span> <span data-ttu-id="9947d-119">Poniższy diagram ilustruje pojęcia.</span><span class="sxs-lookup"><span data-stu-id="9947d-119">The following diagram demonstrates the concept.</span></span> <span data-ttu-id="9947d-120">Wątek wykonywania następuje zamiast czarnych strzałek.</span><span class="sxs-lookup"><span data-stu-id="9947d-120">The thread of execution follows the black arrows.</span></span>

![Wyświetlanie żądań przychodzących, przetwarzania za pomocą trzech middlewares i odpowiedzi, wychodzenia z aplikacji wzorca przetwarzania żądania.](index/_static/request-delegate-pipeline.png)

<span data-ttu-id="9947d-124">Każdy delegat mogą wykonywać operacje, przed i po następnym delegata.</span><span class="sxs-lookup"><span data-stu-id="9947d-124">Each delegate can perform operations before and after the next delegate.</span></span> <span data-ttu-id="9947d-125">Delegatów obsługi wyjątków powinna być wywoływana na wczesnym etapie potoku, dlatego ich może przechwytywać wyjątki, które występują w późniejszym etapie w potoku.</span><span class="sxs-lookup"><span data-stu-id="9947d-125">Exception-handling delegates should be called early in the pipeline, so they can catch exceptions that occur in later stages of the pipeline.</span></span>

<span data-ttu-id="9947d-126">Najprostsza możliwa aplikacji ASP.NET Core ustawia delegat pojedynczego żądania, który obsługuje wszystkie żądania.</span><span class="sxs-lookup"><span data-stu-id="9947d-126">The simplest possible ASP.NET Core app sets up a single request delegate that handles all requests.</span></span> <span data-ttu-id="9947d-127">Ten przypadek nie zawiera rzeczywistego żądania potoku.</span><span class="sxs-lookup"><span data-stu-id="9947d-127">This case doesn't include an actual request pipeline.</span></span> <span data-ttu-id="9947d-128">Zamiast tego jednego funkcja anonimowa jest wywoływana w odpowiedzi na każde żądanie HTTP.</span><span class="sxs-lookup"><span data-stu-id="9947d-128">Instead, a single anonymous function is called in response to every HTTP request.</span></span>

[!code-csharp[](index/snapshot/Middleware/Startup.cs?name=snippet1)]

<span data-ttu-id="9947d-129">Pierwszy <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> delegata kończy potoku.</span><span class="sxs-lookup"><span data-stu-id="9947d-129">The first <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> delegate terminates the pipeline.</span></span>

<span data-ttu-id="9947d-130">Utworzyć łańcuch wielu delegatów żądanie, wraz z <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>.</span><span class="sxs-lookup"><span data-stu-id="9947d-130">Chain multiple request delegates together with <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>.</span></span> <span data-ttu-id="9947d-131">`next` Parametr reprezentuje dalej delegata w potoku.</span><span class="sxs-lookup"><span data-stu-id="9947d-131">The `next` parameter represents the next delegate in the pipeline.</span></span> <span data-ttu-id="9947d-132">Można zwarcie potok według *nie* wywoływania *dalej* parametru.</span><span class="sxs-lookup"><span data-stu-id="9947d-132">You can short-circuit the pipeline by *not* calling the *next* parameter.</span></span> <span data-ttu-id="9947d-133">Zazwyczaj można wykonywać akcje przed i po następnym delegata, tak jak pokazano w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="9947d-133">You can typically perform actions both before and after the next delegate, as the following example demonstrates:</span></span>

[!code-csharp[](index/snapshot/Chain/Startup.cs?name=snippet1)]

<span data-ttu-id="9947d-134">Delegat nie przeszło żądania do następnej delegata, jest nazywany *zwarcie Potok żądań*.</span><span class="sxs-lookup"><span data-stu-id="9947d-134">When a delegate doesn't pass a request to the next delegate, it's called *short-circuiting the request pipeline*.</span></span> <span data-ttu-id="9947d-135">Zwarcie jest często pożądane, ponieważ takie rozwiązanie pomaga uniknąć niepotrzebnych pracy.</span><span class="sxs-lookup"><span data-stu-id="9947d-135">Short-circuiting is often desirable because it avoids unnecessary work.</span></span> <span data-ttu-id="9947d-136">Na przykład [oprogramowanie pośredniczące plików statycznych](xref:fundamentals/static-files) może działać jako *terminalu oprogramowania pośredniczącego* przetwarzania żądania pliku statycznego i zwarcie pozostałego potoku.</span><span class="sxs-lookup"><span data-stu-id="9947d-136">For example, [Static File Middleware](xref:fundamentals/static-files) can act as a *terminal middleware* by processing a request for a static file and short-circuiting the rest of the pipeline.</span></span> <span data-ttu-id="9947d-137">Oprogramowanie pośredniczące było należy dodać do potoku oprogramowania pośredniczącego, które kończy się dalsze przetwarzanie nadal przetwarza kod po ich `next.Invoke` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="9947d-137">Middleware added to the pipeline before the middleware that terminates further processing still processes code after their `next.Invoke` statements.</span></span> <span data-ttu-id="9947d-138">Jednakże zostanie wyświetlone następujące ostrzeżenie o podjęto próbę zapisania odpowiedzi, który już został wysłany.</span><span class="sxs-lookup"><span data-stu-id="9947d-138">However, see the following warning about attempting to write to a response that has already been sent.</span></span>

> [!WARNING]
> <span data-ttu-id="9947d-139">Nie wywołuj `next.Invoke` po wysłaniu odpowiedzi do klienta.</span><span class="sxs-lookup"><span data-stu-id="9947d-139">Don't call `next.Invoke` after the response has been sent to the client.</span></span> <span data-ttu-id="9947d-140">Zmienia się na <xref:Microsoft.AspNetCore.Http.HttpResponse> po rozpoczęciu odpowiedzi zgłoszenie wyjątku.</span><span class="sxs-lookup"><span data-stu-id="9947d-140">Changes to <xref:Microsoft.AspNetCore.Http.HttpResponse> after the response has started throw an exception.</span></span> <span data-ttu-id="9947d-141">Na przykład zmiany, takie jak ustawianie nagłówków i kod stanu zgłosić wyjątek.</span><span class="sxs-lookup"><span data-stu-id="9947d-141">For example, changes such as setting headers and a status code throw an exception.</span></span> <span data-ttu-id="9947d-142">Zapisywanie w treści odpowiedzi po wywołaniu `next`:</span><span class="sxs-lookup"><span data-stu-id="9947d-142">Writing to the response body after calling `next`:</span></span>
>
> * <span data-ttu-id="9947d-143">Może to spowodować naruszenie protokołu.</span><span class="sxs-lookup"><span data-stu-id="9947d-143">May cause a protocol violation.</span></span> <span data-ttu-id="9947d-144">Na przykład zapisywanie ponad podanej `Content-Length`.</span><span class="sxs-lookup"><span data-stu-id="9947d-144">For example, writing more than the stated `Content-Length`.</span></span>
> * <span data-ttu-id="9947d-145">Może spowodować uszkodzenie format treści.</span><span class="sxs-lookup"><span data-stu-id="9947d-145">May corrupt the body format.</span></span> <span data-ttu-id="9947d-146">Na przykład zapis stopki HTML do pliku CSS.</span><span class="sxs-lookup"><span data-stu-id="9947d-146">For example, writing an HTML footer to a CSS file.</span></span>
>
> <span data-ttu-id="9947d-147"><xref:Microsoft.AspNetCore.Http.HttpResponse.HasStarted*> jest to przydatne wskazówka, aby wskazać, czy nagłówki zostały wysłane, czy jednostka została zapisana.</span><span class="sxs-lookup"><span data-stu-id="9947d-147"><xref:Microsoft.AspNetCore.Http.HttpResponse.HasStarted*> is a useful hint to indicate if headers have been sent or the body has been written to.</span></span>

## <a name="order"></a><span data-ttu-id="9947d-148">Zamówienie</span><span class="sxs-lookup"><span data-stu-id="9947d-148">Order</span></span>

<span data-ttu-id="9947d-149">Kolejność dodaną składników oprogramowania pośredniczącego w `Startup.Configure` metoda definiuje kolejność, w którym są wywoływane składników oprogramowania pośredniczącego na żądania i odwrotnej kolejności dla odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="9947d-149">The order that middleware components are added in the `Startup.Configure` method defines the order in which the middleware components are invoked on requests and the reverse order for the response.</span></span> <span data-ttu-id="9947d-150">Kolejność jest krytyczny dla bezpieczeństwa, wydajności i funkcjonalności.</span><span class="sxs-lookup"><span data-stu-id="9947d-150">The order is critical for security, performance, and functionality.</span></span>

<span data-ttu-id="9947d-151">Następujące `Startup.Configure` metoda dodaje składników oprogramowania pośredniczącego dla typowych scenariuszy aplikacji:</span><span class="sxs-lookup"><span data-stu-id="9947d-151">The following `Startup.Configure` method adds middleware components for common app scenarios:</span></span>

::: moniker range=">= aspnetcore-2.0"

1. <span data-ttu-id="9947d-152">Obsługa wyjątku/błędów</span><span class="sxs-lookup"><span data-stu-id="9947d-152">Exception/error handling</span></span>
1. <span data-ttu-id="9947d-153">Protokół zabezpieczeń Strict transportu HTTP</span><span class="sxs-lookup"><span data-stu-id="9947d-153">HTTP Strict Transport Security Protocol</span></span>
1. <span data-ttu-id="9947d-154">Przekierowania protokołu HTTPS</span><span class="sxs-lookup"><span data-stu-id="9947d-154">HTTPS redirection</span></span>
1. <span data-ttu-id="9947d-155">Serwer plików statycznych</span><span class="sxs-lookup"><span data-stu-id="9947d-155">Static file server</span></span>
1. <span data-ttu-id="9947d-156">Wymuszanie zasad plików cookie</span><span class="sxs-lookup"><span data-stu-id="9947d-156">Cookie policy enforcement</span></span>
1. <span data-ttu-id="9947d-157">Uwierzytelnianie</span><span class="sxs-lookup"><span data-stu-id="9947d-157">Authentication</span></span>
1. <span data-ttu-id="9947d-158">Sesja</span><span class="sxs-lookup"><span data-stu-id="9947d-158">Session</span></span>
1. <span data-ttu-id="9947d-159">MVC</span><span class="sxs-lookup"><span data-stu-id="9947d-159">MVC</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    if (env.IsDevelopment())
    {
        // When the app runs in the Development environment:
        //   Use the Developer Exception Page to report app runtime errors.
        //   Use the Database Error Page to report database runtime errors.
        app.UseDeveloperExceptionPage();
        app.UseDatabaseErrorPage();
    }
    else
    {
        // When the app doesn't run in the Development environment:
        //   Enable the Exception Handler Middleware to catch exceptions
        //     thrown in the following middlewares.
        //   Use the HTTP Strict Transport Security Protocol (HSTS)
        //     Middleware.
        app.UseExceptionHandler("/Error");
        app.UseHsts();
    }

    // Use HTTPS Redirection Middleware to redirect HTTP requests to HTTPS.
    app.UseHttpsRedirection();

    // Return static files and end the pipeline.
    app.UseStaticFiles();

    // Use Cookie Policy Middleware to conform to EU General Data 
    // Protection Regulation (GDPR) regulations.
    app.UseCookiePolicy();

    // Authenticate before the user accesses secure resources.
    app.UseAuthentication();

    // If the app uses session state, call Session Middleware after Cookie 
    // Policy Middleware and before MVC Middleware.
    app.UseSession();

    // Add MVC to the request pipeline.
    app.UseMvc();
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

1. <span data-ttu-id="9947d-160">Obsługa wyjątku/błędów</span><span class="sxs-lookup"><span data-stu-id="9947d-160">Exception/error handling</span></span>
1. <span data-ttu-id="9947d-161">Pliki statyczne</span><span class="sxs-lookup"><span data-stu-id="9947d-161">Static files</span></span>
1. <span data-ttu-id="9947d-162">Uwierzytelnianie</span><span class="sxs-lookup"><span data-stu-id="9947d-162">Authentication</span></span>
1. <span data-ttu-id="9947d-163">Sesja</span><span class="sxs-lookup"><span data-stu-id="9947d-163">Session</span></span>
1. <span data-ttu-id="9947d-164">MVC</span><span class="sxs-lookup"><span data-stu-id="9947d-164">MVC</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    // Enable the Exception Handler Middleware to catch exceptions
    //   thrown in the following middlewares.
    app.UseExceptionHandler("/Home/Error");

    // Return static files and end the pipeline.
    app.UseStaticFiles();

    // Authenticate before you access secure resources.
    app.UseIdentity();

    // If the app uses session state, call UseSession before 
    // MVC Middleware.
    app.UseSession();

    // Add MVC to the request pipeline.
    app.UseMvcWithDefaultRoute();
}
```

::: moniker-end

<span data-ttu-id="9947d-165">W poprzednim kodzie przykład, każda metoda rozszerzenia oprogramowanie pośredniczące jest uwidaczniany w <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> za pośrednictwem <xref:Microsoft.AspNetCore.Builder?displayProperty=fullName> przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="9947d-165">In the preceding example code, each middleware extension method is exposed on <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> through the <xref:Microsoft.AspNetCore.Builder?displayProperty=fullName> namespace.</span></span>

<span data-ttu-id="9947d-166"><xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> Pierwszy składnik oprogramowania pośredniczącego jest dodawany do potoku.</span><span class="sxs-lookup"><span data-stu-id="9947d-166"><xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> is the first middleware component added to the pipeline.</span></span> <span data-ttu-id="9947d-167">W związku z tym oprogramowanie pośredniczące programu obsługi wyjątków przechwytuje wszystkie wyjątki, które występują w nowszym wywołuje.</span><span class="sxs-lookup"><span data-stu-id="9947d-167">Therefore, the Exception Handler Middleware catches any exceptions that occur in later calls.</span></span>

<span data-ttu-id="9947d-168">Oprogramowanie pośredniczące plików statycznych jest wywoływana na wczesnym etapie potoku, dzięki czemu mogą obsługiwać żądania i zwarcie bez pośrednictwa pozostałe składniki.</span><span class="sxs-lookup"><span data-stu-id="9947d-168">Static File Middleware is called early in the pipeline so that it can handle requests and short-circuit without going through the remaining components.</span></span> <span data-ttu-id="9947d-169">Dostarcza statycznej oprogramowanie pośredniczące plików **nie** sprawdzeń autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="9947d-169">The Static File Middleware provides **no** authorization checks.</span></span> <span data-ttu-id="9947d-170">Wszystkich plików obsługiwanych przez, łącznie z tymi w ramach *wwwroot*, są publicznie dostępne.</span><span class="sxs-lookup"><span data-stu-id="9947d-170">Any files served by it, including those under *wwwroot*, are publicly available.</span></span> <span data-ttu-id="9947d-171">Podejścia do zabezpieczania plików statycznych, zobacz <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="9947d-171">For an approach to secure static files, see <xref:fundamentals/static-files>.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="9947d-172">Jeśli żądanie nie jest obsługiwany przez statyczne oprogramowanie pośredniczące plików, będzie przekazany z oprogramowaniem pośredniczącym uwierzytelniania (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>), który przeprowadza uwierzytelnianie.</span><span class="sxs-lookup"><span data-stu-id="9947d-172">If the request isn't handled by the Static File Middleware, it's passed on to the Authentication Middleware (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>), which performs authentication.</span></span> <span data-ttu-id="9947d-173">Uwierzytelnianie nie powodują pominięcie nieuwierzytelnione żądania.</span><span class="sxs-lookup"><span data-stu-id="9947d-173">Authentication doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="9947d-174">Mimo że uwierzytelniania oprogramowania pośredniczącego uwierzytelniania na podstawie żądań autoryzacji (i odrzucenia) występuje tylko wtedy, gdy MVC wybiera określonych akcji i kontrolerów MVC lub strony Razor.</span><span class="sxs-lookup"><span data-stu-id="9947d-174">Although Authentication Middleware authenticates requests, authorization (and rejection) occurs only after MVC selects a specific Razor Page or MVC controller and action.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="9947d-175">Jeśli żądanie nie jest obsługiwany przez oprogramowanie pośredniczące plików statycznych, przekazaniem do oprogramowania pośredniczącego tożsamości (<xref:Microsoft.AspNetCore.Builder.BuilderExtensions.UseIdentity*>), który przeprowadza uwierzytelnianie.</span><span class="sxs-lookup"><span data-stu-id="9947d-175">If the request isn't handled by Static File Middleware, it's passed on to the Identity Middleware (<xref:Microsoft.AspNetCore.Builder.BuilderExtensions.UseIdentity*>), which performs authentication.</span></span> <span data-ttu-id="9947d-176">Tożsamość nie powodują pominięcie nieuwierzytelnione żądania.</span><span class="sxs-lookup"><span data-stu-id="9947d-176">Identity doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="9947d-177">Mimo że tożsamość uwierzytelniania na podstawie żądań autoryzacji (i odrzucenia) występuje tylko wtedy, gdy MVC wybiera określonego kontrolera i akcji.</span><span class="sxs-lookup"><span data-stu-id="9947d-177">Although Identity authenticates requests, authorization (and rejection) occurs only after MVC selects a specific controller and action.</span></span>

::: moniker-end

<span data-ttu-id="9947d-178">Poniższy przykład pokazuje kolejność oprogramowania pośredniczącego, w którym żądań dotyczących plików statycznych są obsługiwane przez oprogramowanie pośredniczące plików statycznych przed oprogramowanie pośredniczące kompresji odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="9947d-178">The following example demonstrates a middleware order where requests for static files are handled by Static File Middleware before Response Compression Middleware.</span></span> <span data-ttu-id="9947d-179">Pliki statyczne nie są kompresowane z tym zamówieniem oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="9947d-179">Static files aren't compressed with this middleware order.</span></span> <span data-ttu-id="9947d-180">Odpowiedzi MVC z <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> można skompresować.</span><span class="sxs-lookup"><span data-stu-id="9947d-180">The MVC responses from <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> can be compressed.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    // Static files not compressed by Static File Middleware.
    app.UseStaticFiles();
    app.UseResponseCompression();
    app.UseMvcWithDefaultRoute();
}
```

### <a name="use-run-and-map"></a><span data-ttu-id="9947d-181">Użyj, uruchom i mapy</span><span class="sxs-lookup"><span data-stu-id="9947d-181">Use, Run, and Map</span></span>

<span data-ttu-id="9947d-182">Konfigurowanie przy użyciu potoku HTTP `Use`, `Run`, i `Map`.</span><span class="sxs-lookup"><span data-stu-id="9947d-182">Configure the HTTP pipeline using `Use`, `Run`, and `Map`.</span></span> <span data-ttu-id="9947d-183">`Use` Metoda może zwarcie potoku (to znaczy, jeśli on nie wywołać `next` delegata żądania).</span><span class="sxs-lookup"><span data-stu-id="9947d-183">The `Use` method can short-circuit the pipeline (that is, if it doesn't call a `next` request delegate).</span></span> <span data-ttu-id="9947d-184">`Run` jest to Konwencja i niektórych składników oprogramowania pośredniczącego może narazić `Run[Middleware]` metod, które są uruchamiane na końcu potoku.</span><span class="sxs-lookup"><span data-stu-id="9947d-184">`Run` is a convention, and some middleware components may expose `Run[Middleware]` methods that run at the end of the pipeline.</span></span>

<span data-ttu-id="9947d-185"><xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> rozszerzenia są używane jako Konwencję do tworzenia odgałęzień potoku.</span><span class="sxs-lookup"><span data-stu-id="9947d-185"><xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> extensions are used as a convention for branching the pipeline.</span></span> <span data-ttu-id="9947d-186">`Map*` gałęzie potoku żądania na podstawie dopasowań podanej ścieżki żądania.</span><span class="sxs-lookup"><span data-stu-id="9947d-186">`Map*` branches the request pipeline based on matches of the given request path.</span></span> <span data-ttu-id="9947d-187">Jeśli ścieżka żądania rozpoczyna się od podanej ścieżce, jest wykonywana gałąź.</span><span class="sxs-lookup"><span data-stu-id="9947d-187">If the request path starts with the given path, the branch is executed.</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMap.cs?name=snippet1)]

<span data-ttu-id="9947d-188">W poniższej tabeli przedstawiono żądań i odpowiedzi z `http://localhost:1234` przy użyciu poprzedniego kodu.</span><span class="sxs-lookup"><span data-stu-id="9947d-188">The following table shows the requests and responses from `http://localhost:1234` using the previous code.</span></span>

| <span data-ttu-id="9947d-189">Żądanie</span><span class="sxs-lookup"><span data-stu-id="9947d-189">Request</span></span>             | <span data-ttu-id="9947d-190">Odpowiedź</span><span class="sxs-lookup"><span data-stu-id="9947d-190">Response</span></span>                     |
| ------------------- | ---------------------------- |
| <span data-ttu-id="9947d-191">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="9947d-191">localhost:1234</span></span>      | <span data-ttu-id="9947d-192">Pozdrowienia ze-Map delegata.</span><span class="sxs-lookup"><span data-stu-id="9947d-192">Hello from non-Map delegate.</span></span> |
| <span data-ttu-id="9947d-193">localhost:1234/map1</span><span class="sxs-lookup"><span data-stu-id="9947d-193">localhost:1234/map1</span></span> | <span data-ttu-id="9947d-194">Mapa badania 1</span><span class="sxs-lookup"><span data-stu-id="9947d-194">Map Test 1</span></span>                   |
| <span data-ttu-id="9947d-195">localhost:1234/map2</span><span class="sxs-lookup"><span data-stu-id="9947d-195">localhost:1234/map2</span></span> | <span data-ttu-id="9947d-196">Mapa badania 2</span><span class="sxs-lookup"><span data-stu-id="9947d-196">Map Test 2</span></span>                   |
| <span data-ttu-id="9947d-197">localhost:1234/map3</span><span class="sxs-lookup"><span data-stu-id="9947d-197">localhost:1234/map3</span></span> | <span data-ttu-id="9947d-198">Pozdrowienia ze-Map delegata.</span><span class="sxs-lookup"><span data-stu-id="9947d-198">Hello from non-Map delegate.</span></span> |

<span data-ttu-id="9947d-199">Gdy `Map` jest używane, ścieżka dopasowane jej ewentualny związek są usuwane z `HttpRequest.Path` i dołączonych do `HttpRequest.PathBase` dla każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="9947d-199">When `Map` is used, the matched path segment(s) are removed from `HttpRequest.Path` and appended to `HttpRequest.PathBase` for each request.</span></span>

<span data-ttu-id="9947d-200">[MapWhen](/dotnet/api/microsoft.aspnetcore.builder.mapwhenextensions) odgałęzień potoku żądania na podstawie wyniku danego predykatu.</span><span class="sxs-lookup"><span data-stu-id="9947d-200">[MapWhen](/dotnet/api/microsoft.aspnetcore.builder.mapwhenextensions) branches the request pipeline based on the result of the given predicate.</span></span> <span data-ttu-id="9947d-201">Wszelkie predykatu typu `Func<HttpContext, bool>` może być używany do mapowania żądania do nowej gałęzi w potoku.</span><span class="sxs-lookup"><span data-stu-id="9947d-201">Any predicate of type `Func<HttpContext, bool>` can be used to map requests to a new branch of the pipeline.</span></span> <span data-ttu-id="9947d-202">W poniższym przykładzie, predykat jest używany do wykrywania obecności zmiennej ciągu zapytania `branch`:</span><span class="sxs-lookup"><span data-stu-id="9947d-202">In the following example, a predicate is used to detect the presence of a query string variable `branch`:</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMapWhen.cs?name=snippet1)]

<span data-ttu-id="9947d-203">W poniższej tabeli przedstawiono żądań i odpowiedzi z `http://localhost:1234` przy użyciu poprzedniego kodu.</span><span class="sxs-lookup"><span data-stu-id="9947d-203">The following table shows the requests and responses from `http://localhost:1234` using the previous code.</span></span>

| <span data-ttu-id="9947d-204">Żądanie</span><span class="sxs-lookup"><span data-stu-id="9947d-204">Request</span></span>                       | <span data-ttu-id="9947d-205">Odpowiedź</span><span class="sxs-lookup"><span data-stu-id="9947d-205">Response</span></span>                     |
| ----------------------------- | ---------------------------- |
| <span data-ttu-id="9947d-206">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="9947d-206">localhost:1234</span></span>                | <span data-ttu-id="9947d-207">Pozdrowienia ze-Map delegata.</span><span class="sxs-lookup"><span data-stu-id="9947d-207">Hello from non-Map delegate.</span></span> |
| <span data-ttu-id="9947d-208">localhost:1234/?branch=master</span><span class="sxs-lookup"><span data-stu-id="9947d-208">localhost:1234/?branch=master</span></span> | <span data-ttu-id="9947d-209">Używana gałąź = master</span><span class="sxs-lookup"><span data-stu-id="9947d-209">Branch used = master</span></span>         |

<span data-ttu-id="9947d-210">`Map` obsługuje zagnieżdżania, na przykład:</span><span class="sxs-lookup"><span data-stu-id="9947d-210">`Map` supports nesting, for example:</span></span>

```csharp
app.Map("/level1", level1App => {
    level1App.Map("/level2a", level2AApp => {
        // "/level1/level2a" processing
    });
    level1App.Map("/level2b", level2BApp => {
        // "/level1/level2b" processing
    });
});
   ```

<span data-ttu-id="9947d-211">`Map` można również przeprowadzać dopasowywanie wiele segmentów jednocześnie:</span><span class="sxs-lookup"><span data-stu-id="9947d-211">`Map` can also match multiple segments at once:</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMultiSeg.cs?name=snippet1&highlight=13)]

## <a name="built-in-middleware"></a><span data-ttu-id="9947d-212">Wbudowane oprogramowania pośredniczącego</span><span class="sxs-lookup"><span data-stu-id="9947d-212">Built-in middleware</span></span>

<span data-ttu-id="9947d-213">Platforma ASP.NET Core jest dostarczany z następujących składników oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="9947d-213">ASP.NET Core ships with the following middleware components.</span></span> <span data-ttu-id="9947d-214">*Kolejności* kolumna zawiera uwagi dotyczące umieszczenia oprogramowanie pośredniczące w potoku przetwarzania żądań i na jakich warunkach oprogramowanie pośredniczące może rozwiązać niniejszą przetwarzania żądania.</span><span class="sxs-lookup"><span data-stu-id="9947d-214">The *Order* column provides notes on middleware placement in the request processing pipeline and under what conditions the middleware may terminate request processing.</span></span> <span data-ttu-id="9947d-215">Oprogramowanie pośredniczące short-circuits potoku przetwarzania żądań i uniemożliwia dalsze podrzędnego oprogramowanie pośredniczące przetwarza żądanie, jest nazywany *terminalu oprogramowania pośredniczącego*.</span><span class="sxs-lookup"><span data-stu-id="9947d-215">When a middleware short-circuits the request processing pipeline and prevents further downstream middleware from processing a request, it's called a *terminal middleware*.</span></span> <span data-ttu-id="9947d-216">Aby uzyskać więcej informacji na temat zwarcie, zobacz [tworzenie potoku oprogramowania pośredniczącego z IApplicationBuilder](#create-a-middleware-pipeline-with-iapplicationbuilder) sekcji.</span><span class="sxs-lookup"><span data-stu-id="9947d-216">For more information on short-circuiting, see the [Create a middleware pipeline with IApplicationBuilder](#create-a-middleware-pipeline-with-iapplicationbuilder) section.</span></span>

| <span data-ttu-id="9947d-217">Oprogramowanie pośredniczące</span><span class="sxs-lookup"><span data-stu-id="9947d-217">Middleware</span></span> | <span data-ttu-id="9947d-218">Opis</span><span class="sxs-lookup"><span data-stu-id="9947d-218">Description</span></span> | <span data-ttu-id="9947d-219">Zamówienie</span><span class="sxs-lookup"><span data-stu-id="9947d-219">Order</span></span> |
| ---------- | ----------- | ----- |
| [<span data-ttu-id="9947d-220">Uwierzytelnianie</span><span class="sxs-lookup"><span data-stu-id="9947d-220">Authentication</span></span>](xref:security/authentication/identity) | <span data-ttu-id="9947d-221">Zapewnia obsługę uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="9947d-221">Provides authentication support.</span></span> | <span data-ttu-id="9947d-222">Przed `HttpContext.User` jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="9947d-222">Before `HttpContext.User` is needed.</span></span> <span data-ttu-id="9947d-223">Terminala dla wywołania zwrotne OAuth.</span><span class="sxs-lookup"><span data-stu-id="9947d-223">Terminal for OAuth callbacks.</span></span> |
| [<span data-ttu-id="9947d-224">Zasady plików cookie</span><span class="sxs-lookup"><span data-stu-id="9947d-224">Cookie Policy</span></span>](xref:security/gdpr) | <span data-ttu-id="9947d-225">Śledzi zgody od użytkowników do przechowywania informacji osobistych i wymusza standardy minimalne dla pliku cookie pól, takich jak `secure` i `SameSite`.</span><span class="sxs-lookup"><span data-stu-id="9947d-225">Tracks consent from users for storing personal information and enforces minimum standards for cookie fields, such as `secure` and `SameSite`.</span></span> | <span data-ttu-id="9947d-226">Zanim oprogramowanie pośredniczące, która wystawia pliki cookie.</span><span class="sxs-lookup"><span data-stu-id="9947d-226">Before middleware that issues cookies.</span></span> <span data-ttu-id="9947d-227">Przykłady: Uwierzytelnianie, sesji, MVC (TempData).</span><span class="sxs-lookup"><span data-stu-id="9947d-227">Examples: Authentication, Session, MVC (TempData).</span></span> |
| [<span data-ttu-id="9947d-228">CORS</span><span class="sxs-lookup"><span data-stu-id="9947d-228">CORS</span></span>](xref:security/cors) | <span data-ttu-id="9947d-229">Konfiguruje, Cross-Origin Resource Sharing.</span><span class="sxs-lookup"><span data-stu-id="9947d-229">Configures Cross-Origin Resource Sharing.</span></span> | <span data-ttu-id="9947d-230">Przed składników, które korzystają z mechanizmu CORS.</span><span class="sxs-lookup"><span data-stu-id="9947d-230">Before components that use CORS.</span></span> |
| [<span data-ttu-id="9947d-231">Diagnostyka</span><span class="sxs-lookup"><span data-stu-id="9947d-231">Diagnostics</span></span>](xref:fundamentals/error-handling) | <span data-ttu-id="9947d-232">Konfiguruje diagnostyki.</span><span class="sxs-lookup"><span data-stu-id="9947d-232">Configures diagnostics.</span></span> | <span data-ttu-id="9947d-233">Przed składniki, które generują błędy.</span><span class="sxs-lookup"><span data-stu-id="9947d-233">Before components that generate errors.</span></span> |
| [<span data-ttu-id="9947d-234">Nagłówki przekazywane</span><span class="sxs-lookup"><span data-stu-id="9947d-234">Forwarded Headers</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions) | <span data-ttu-id="9947d-235">Przekazuje nagłówki przekazywane do bieżącego żądania.</span><span class="sxs-lookup"><span data-stu-id="9947d-235">Forwards proxied headers onto the current request.</span></span> | <span data-ttu-id="9947d-236">Przed składniki, które zużywają zaktualizowanymi polami.</span><span class="sxs-lookup"><span data-stu-id="9947d-236">Before components that consume the updated fields.</span></span> <span data-ttu-id="9947d-237">Przykłady: schematu, hosta, adres IP klienta, metoda.</span><span class="sxs-lookup"><span data-stu-id="9947d-237">Examples: scheme, host, client IP, method.</span></span> |
| [<span data-ttu-id="9947d-238">Kontrola kondycji</span><span class="sxs-lookup"><span data-stu-id="9947d-238">Health Check</span></span>](xref:host-and-deploy/health-checks) | <span data-ttu-id="9947d-239">Sprawdza kondycję aplikacji ASP.NET Core oraz jego zależności, takich jak sprawdzanie dostępności bazy danych.</span><span class="sxs-lookup"><span data-stu-id="9947d-239">Checks the health of an ASP.NET Core app and its dependencies, such as checking database availability.</span></span> | <span data-ttu-id="9947d-240">Terminal, gdy żądanie pasuje do endpoint sprawdzania kondycji.</span><span class="sxs-lookup"><span data-stu-id="9947d-240">Terminal if a request matches a health check endpoint.</span></span> |
| [<span data-ttu-id="9947d-241">Zastąpienie metody HTTP</span><span class="sxs-lookup"><span data-stu-id="9947d-241">HTTP Method Override</span></span>](/dotnet/api/microsoft.aspnetcore.builder.httpmethodoverrideextensions) | <span data-ttu-id="9947d-242">Zezwala na przychodzące żądania POST przesłonić metodę.</span><span class="sxs-lookup"><span data-stu-id="9947d-242">Allows an incoming POST request to override the method.</span></span> | <span data-ttu-id="9947d-243">Przed składniki, które zużywają zaktualizowana metoda.</span><span class="sxs-lookup"><span data-stu-id="9947d-243">Before components that consume the updated method.</span></span> |
| [<span data-ttu-id="9947d-244">HTTPS Redirection</span><span class="sxs-lookup"><span data-stu-id="9947d-244">HTTPS Redirection</span></span>](xref:security/enforcing-ssl#require-https) | <span data-ttu-id="9947d-245">Przekieruj wszystkie żądania HTTP do HTTPS (platformy ASP.NET Core 2.1 lub nowszej).</span><span class="sxs-lookup"><span data-stu-id="9947d-245">Redirect all HTTP requests to HTTPS (ASP.NET Core 2.1 or later).</span></span> | <span data-ttu-id="9947d-246">Przed składniki, których wartość użycia adresu URL.</span><span class="sxs-lookup"><span data-stu-id="9947d-246">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="9947d-247">Zabezpieczenia transportu Strict HTTP (HSTS)</span><span class="sxs-lookup"><span data-stu-id="9947d-247">HTTP Strict Transport Security (HSTS)</span></span>](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts) | <span data-ttu-id="9947d-248">Oprogramowanie ulepszenie pośredniczące zabezpieczeń, który dodaje nagłówek odpowiedzi specjalne (platformy ASP.NET Core 2.1 lub nowszej).</span><span class="sxs-lookup"><span data-stu-id="9947d-248">Security enhancement middleware that adds a special response header (ASP.NET Core 2.1 or later).</span></span> | <span data-ttu-id="9947d-249">Przed wysłaniem odpowiedzi i po składniki, które modyfikują żądań.</span><span class="sxs-lookup"><span data-stu-id="9947d-249">Before responses are sent and after components that modify requests.</span></span> <span data-ttu-id="9947d-250">Przykłady: Nagłówki przekazywane ponownego zapisywania adresów URL.</span><span class="sxs-lookup"><span data-stu-id="9947d-250">Examples: Forwarded Headers, URL Rewriting.</span></span> |
| [<span data-ttu-id="9947d-251">MVC</span><span class="sxs-lookup"><span data-stu-id="9947d-251">MVC</span></span>](xref:mvc/overview) | <span data-ttu-id="9947d-252">Przetwarza żądania przy użyciu stron MVC i Razor (platformy ASP.NET Core w wersji 2.0 lub nowszej).</span><span class="sxs-lookup"><span data-stu-id="9947d-252">Processes requests with MVC/Razor Pages (ASP.NET Core 2.0 or later).</span></span> | <span data-ttu-id="9947d-253">Terminal, jeśli żądanie jest zgodny z trasą.</span><span class="sxs-lookup"><span data-stu-id="9947d-253">Terminal if a request matches a route.</span></span> |
| [<span data-ttu-id="9947d-254">OWIN</span><span class="sxs-lookup"><span data-stu-id="9947d-254">OWIN</span></span>](xref:fundamentals/owin) | <span data-ttu-id="9947d-255">Współdziałanie z aplikacji opartych na OWIN, serwerów i oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="9947d-255">Interop with OWIN-based apps, servers, and middleware.</span></span> | <span data-ttu-id="9947d-256">Terminal, jeśli oprogramowanie pośredniczące OWIN w pełni przetwarza żądanie.</span><span class="sxs-lookup"><span data-stu-id="9947d-256">Terminal if the OWIN Middleware fully processes the request.</span></span> |
| [<span data-ttu-id="9947d-257">Buforowanie odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="9947d-257">Response Caching</span></span>](xref:performance/caching/middleware) | <span data-ttu-id="9947d-258">Zapewnia obsługę buforowania odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="9947d-258">Provides support for caching responses.</span></span> | <span data-ttu-id="9947d-259">Przed składniki, które wymagają buforowania.</span><span class="sxs-lookup"><span data-stu-id="9947d-259">Before components that require caching.</span></span> |
| [<span data-ttu-id="9947d-260">Kompresja odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="9947d-260">Response Compression</span></span>](xref:performance/response-compression) | <span data-ttu-id="9947d-261">Zapewnia obsługę kompresowania odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="9947d-261">Provides support for compressing responses.</span></span> | <span data-ttu-id="9947d-262">Przed składniki, które wymagają kompresji.</span><span class="sxs-lookup"><span data-stu-id="9947d-262">Before components that require compression.</span></span> |
| [<span data-ttu-id="9947d-263">Lokalizacja żądania</span><span class="sxs-lookup"><span data-stu-id="9947d-263">Request Localization</span></span>](xref:fundamentals/localization) | <span data-ttu-id="9947d-264">Zapewnia obsługę lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="9947d-264">Provides localization support.</span></span> | <span data-ttu-id="9947d-265">Przed składniki poufnych lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="9947d-265">Before localization sensitive components.</span></span> |
| [<span data-ttu-id="9947d-266">Routing</span><span class="sxs-lookup"><span data-stu-id="9947d-266">Routing</span></span>](xref:fundamentals/routing) | <span data-ttu-id="9947d-267">Definiuje i ogranicza trasy żądania.</span><span class="sxs-lookup"><span data-stu-id="9947d-267">Defines and constrains request routes.</span></span> | <span data-ttu-id="9947d-268">Terminalu zgodnych tras.</span><span class="sxs-lookup"><span data-stu-id="9947d-268">Terminal for matching routes.</span></span> |
| [<span data-ttu-id="9947d-269">Sesja</span><span class="sxs-lookup"><span data-stu-id="9947d-269">Session</span></span>](xref:fundamentals/app-state) | <span data-ttu-id="9947d-270">Zapewnia obsługę zarządzania sesjami użytkownika.</span><span class="sxs-lookup"><span data-stu-id="9947d-270">Provides support for managing user sessions.</span></span> | <span data-ttu-id="9947d-271">Przed składniki, które wymagają sesji.</span><span class="sxs-lookup"><span data-stu-id="9947d-271">Before components that require Session.</span></span> |
| [<span data-ttu-id="9947d-272">Pliki statyczne</span><span class="sxs-lookup"><span data-stu-id="9947d-272">Static Files</span></span>](xref:fundamentals/static-files) | <span data-ttu-id="9947d-273">Zapewnia obsługę obsługujących pliki statyczne oraz przeglądanie katalogów.</span><span class="sxs-lookup"><span data-stu-id="9947d-273">Provides support for serving static files and directory browsing.</span></span> | <span data-ttu-id="9947d-274">Terminal, gdy żądanie pasuje do pliku.</span><span class="sxs-lookup"><span data-stu-id="9947d-274">Terminal if a request matches a file.</span></span> |
| [<span data-ttu-id="9947d-275">Ponownego zapisywania adresów URL</span><span class="sxs-lookup"><span data-stu-id="9947d-275">URL Rewriting</span></span>](xref:fundamentals/url-rewriting) | <span data-ttu-id="9947d-276">Zapewnia obsługę ponownego zapisywania adresów URL przekierowywania żądań.</span><span class="sxs-lookup"><span data-stu-id="9947d-276">Provides support for rewriting URLs and redirecting requests.</span></span> | <span data-ttu-id="9947d-277">Przed składniki, których wartość użycia adresu URL.</span><span class="sxs-lookup"><span data-stu-id="9947d-277">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="9947d-278">Obiekty WebSocket</span><span class="sxs-lookup"><span data-stu-id="9947d-278">WebSockets</span></span>](xref:fundamentals/websockets) | <span data-ttu-id="9947d-279">Włącza protokół Websocket.</span><span class="sxs-lookup"><span data-stu-id="9947d-279">Enables the WebSockets protocol.</span></span> | <span data-ttu-id="9947d-280">Przed składniki, które są wymagane w celu umożliwienia akceptowania żądań protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="9947d-280">Before components that are required to accept WebSocket requests.</span></span> |

## <a name="additional-resources"></a><span data-ttu-id="9947d-281">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="9947d-281">Additional resources</span></span>

* <xref:fundamentals/middleware/write>
* <xref:migration/http-modules>
* <xref:fundamentals/startup>
* <xref:fundamentals/request-features>
* <xref:fundamentals/middleware/extensibility>
* <xref:fundamentals/middleware/extensibility-third-party-container>

---
title: Ataki zapobiec Cross-Site Request Forgery (XSRF/CSRF) w programie ASP.NET Core
author: steve-smith
description: Dowiedz się, jak zapobiegać atakom aplikacji sieci web, w którym złośliwą witrynę sieci Web mogą mieć wpływ na interakcję między przeglądarką klienta i aplikacji.
ms.author: riande
ms.custom: mvc
ms.date: 10/11/2018
uid: security/anti-request-forgery
ms.openlocfilehash: 6e140717834b901e12ef7863fd07b983b0c55107
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57066530"
---
# <a name="prevent-cross-site-request-forgery-xsrfcsrf-attacks-in-aspnet-core"></a><span data-ttu-id="942c5-103">Ataki zapobiec Cross-Site Request Forgery (XSRF/CSRF) w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="942c5-103">Prevent Cross-Site Request Forgery (XSRF/CSRF) attacks in ASP.NET Core</span></span>

<span data-ttu-id="942c5-104">Przez [Steve Smith](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="942c5-104">By [Steve Smith](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="942c5-105">Fałszowanie żądań między witrynami (znany także jako Wymowa XSRF lub CSRF, *szkl zobacz*) jest ataku na aplikacje hostowane w sieci web, zgodnie z którą aplikacja sieci web złośliwego mogą mieć wpływ na interakcję między przeglądarką klienta i aplikacji sieci web, która ufa Przeglądarka.</span><span class="sxs-lookup"><span data-stu-id="942c5-105">Cross-site request forgery (also known as XSRF or CSRF, pronounced *see-surf*) is an attack against web-hosted apps whereby a malicious web app can influence the interaction between a client browser and a web app that trusts that browser.</span></span> <span data-ttu-id="942c5-106">Te ataki są możliwe, ponieważ przeglądarki sieci web wysyłać niektóre typy tokenów uwierzytelniania automatycznie za pomocą każdego żądania do witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="942c5-106">These attacks are possible because web browsers send some types of authentication tokens automatically with every request to a website.</span></span> <span data-ttu-id="942c5-107">Ta forma wykorzystać jest także znana jako *ataku jednym kliknięciem* lub *sesji jazda* ponieważ ataku korzysta z zalet użytkownik wcześniej uwierzytelniona sesji.</span><span class="sxs-lookup"><span data-stu-id="942c5-107">This form of exploit is also known as a *one-click attack* or *session riding* because the attack takes advantage of the user's previously authenticated session.</span></span>

<span data-ttu-id="942c5-108">Przykład ataku typu CSRF:</span><span class="sxs-lookup"><span data-stu-id="942c5-108">An example of a CSRF attack:</span></span>

1. <span data-ttu-id="942c5-109">Użytkownik zaloguje się do `www.good-banking-site.com` za pomocą uwierzytelniania formularzy.</span><span class="sxs-lookup"><span data-stu-id="942c5-109">A user signs into `www.good-banking-site.com` using forms authentication.</span></span> <span data-ttu-id="942c5-110">Serwer uwierzytelnia użytkownika i wysyła odpowiedź, który zawiera plik cookie uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="942c5-110">The server authenticates the user and issues a response that includes an authentication cookie.</span></span> <span data-ttu-id="942c5-111">Witryna jest narażony na ataki, ponieważ Magazyn uzna każde żądanie, które otrzymuje z prawidłowy plik cookie.</span><span class="sxs-lookup"><span data-stu-id="942c5-111">The site is vulnerable to attack because it trusts any request that it receives with a valid authentication cookie.</span></span>
1. <span data-ttu-id="942c5-112">Użytkownik odwiedzi złośliwą witrynę, `www.bad-crook-site.com`.</span><span class="sxs-lookup"><span data-stu-id="942c5-112">The user visits a malicious site, `www.bad-crook-site.com`.</span></span>

   <span data-ttu-id="942c5-113">Złośliwa witryna `www.bad-crook-site.com`, podobny do następującego formularza HTML zawiera:</span><span class="sxs-lookup"><span data-stu-id="942c5-113">The malicious site, `www.bad-crook-site.com`, contains an HTML form similar to the following:</span></span>

   ```html
   <h1>Congratulations! You're a Winner!</h1>
   <form action="http://good-banking-site.com/api/account" method="post">
       <input type="hidden" name="Transaction" value="withdraw">
       <input type="hidden" name="Amount" value="1000000">
       <input type="submit" value="Click to collect your prize!">
   </form>
   ```

   <span data-ttu-id="942c5-114">Należy zauważyć, że formularz `action` wpisy do lokacji narażony, a nie do złośliwych witryn.</span><span class="sxs-lookup"><span data-stu-id="942c5-114">Notice that the form's `action` posts to the vulnerable site, not to the malicious site.</span></span> <span data-ttu-id="942c5-115">Jest to część "cross-site" CSRF.</span><span class="sxs-lookup"><span data-stu-id="942c5-115">This is the "cross-site" part of CSRF.</span></span>

1. <span data-ttu-id="942c5-116">Gdy użytkownik wybierze przycisk Prześlij.</span><span class="sxs-lookup"><span data-stu-id="942c5-116">The user selects the submit button.</span></span> <span data-ttu-id="942c5-117">Przeglądarka sprawia, że żądanie i automatycznie dodaje plik cookie uwierzytelniania dla Żądana domena `www.good-banking-site.com`.</span><span class="sxs-lookup"><span data-stu-id="942c5-117">The browser makes the request and automatically includes the authentication cookie for the requested domain, `www.good-banking-site.com`.</span></span>
1. <span data-ttu-id="942c5-118">Żądanie jest uruchamiany na `www.good-banking-site.com` serwera przy użyciu kontekstu uwierzytelniania użytkownika i mogą wykonywać żadnych akcji, która może wykonywać uwierzytelnionego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="942c5-118">The request runs on the `www.good-banking-site.com` server with the user's authentication context and can perform any action that an authenticated user is allowed to perform.</span></span>

<span data-ttu-id="942c5-119">Oprócz tego scenariusza, gdy użytkownik wybierze przycisk, który można przesłać formularza złośliwych witryn wykonać następujące akcje:</span><span class="sxs-lookup"><span data-stu-id="942c5-119">In addition to the scenario where the user selects the button to submit the form, the malicious site could:</span></span>

* <span data-ttu-id="942c5-120">Uruchom skrypt, który automatycznie przesyła formularz.</span><span class="sxs-lookup"><span data-stu-id="942c5-120">Run a script that automatically submits the form.</span></span>
* <span data-ttu-id="942c5-121">Wyślij przesyłania formularza jako żądaniem AJAX.</span><span class="sxs-lookup"><span data-stu-id="942c5-121">Send the form submission as an AJAX request.</span></span>
* <span data-ttu-id="942c5-122">Ukryj formularza przy użyciu CSS.</span><span class="sxs-lookup"><span data-stu-id="942c5-122">Hide the form using CSS.</span></span>

<span data-ttu-id="942c5-123">Te scenariusze alternatywnych nie wymagają żadnych akcji lub danych wejściowych od użytkownika innego niż początkowo odwiedzający złośliwych witryn.</span><span class="sxs-lookup"><span data-stu-id="942c5-123">These alternative scenarios don't require any action or input from the user other than initially visiting the malicious site.</span></span>

<span data-ttu-id="942c5-124">Przy użyciu protokołu HTTPS nie uniemożliwia ataku CSRF.</span><span class="sxs-lookup"><span data-stu-id="942c5-124">Using HTTPS doesn't prevent a CSRF attack.</span></span> <span data-ttu-id="942c5-125">Złośliwa witryna może wysyłać `https://www.good-banking-site.com/` równie łatwo, jak może wysłać żądanie niezabezpieczone żądania.</span><span class="sxs-lookup"><span data-stu-id="942c5-125">The malicious site can send an `https://www.good-banking-site.com/` request just as easily as it can send an insecure request.</span></span>

<span data-ttu-id="942c5-126">Niektóre ataki docelowych punktów końcowych, które odpowiadają na żądania GET, w którym to przypadku tag obrazu może służyć do wykonania akcji.</span><span class="sxs-lookup"><span data-stu-id="942c5-126">Some attacks target endpoints that respond to GET requests, in which case an image tag can be used to perform the action.</span></span> <span data-ttu-id="942c5-127">Ta forma ataku jest typowe w witrynach forum, które zezwala na obrazach, ale Blokuj kod JavaScript.</span><span class="sxs-lookup"><span data-stu-id="942c5-127">This form of attack is common on forum sites that permit images but block JavaScript.</span></span> <span data-ttu-id="942c5-128">Aplikacje, które zmieniają stan dla żądań GET, gdzie zmiennych lub zasoby zostały zmienione, są podatne na złośliwe ataki.</span><span class="sxs-lookup"><span data-stu-id="942c5-128">Apps that change state on GET requests, where variables or resources are altered, are vulnerable to malicious attacks.</span></span> <span data-ttu-id="942c5-129">**Żądania GET, które zmiany stanu jest niebezpieczne. Najlepszym rozwiązaniem jest nigdy nie ulegną zmianie stanu na żądanie GET.**</span><span class="sxs-lookup"><span data-stu-id="942c5-129">**GET requests that change state are insecure. A best practice is to never change state on a GET request.**</span></span>

<span data-ttu-id="942c5-130">Możliwe dla aplikacji sieci web, które używać plików cookie uwierzytelniania, ponieważ są CSRF ataków:</span><span class="sxs-lookup"><span data-stu-id="942c5-130">CSRF attacks are possible against web apps that use cookies for authentication because:</span></span>

* <span data-ttu-id="942c5-131">Przeglądarki przechowywania plików cookie, wystawiony przez aplikację sieci web.</span><span class="sxs-lookup"><span data-stu-id="942c5-131">Browsers store cookies issued by a web app.</span></span>
* <span data-ttu-id="942c5-132">Przechowywane pliki cookie obejmują pliki cookie z sesji uwierzytelnionych użytkowników.</span><span class="sxs-lookup"><span data-stu-id="942c5-132">Stored cookies include session cookies for authenticated users.</span></span>
* <span data-ttu-id="942c5-133">Przeglądarki Wyślij wszystkie pliki cookie skojarzone z domeną, do aplikacji sieci web każde żądanie, niezależnie od tego, jak żądania do aplikacji został wygenerowany w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="942c5-133">Browsers send all of the cookies associated with a domain to the web app every request regardless of how the request to app was generated within the browser.</span></span>

<span data-ttu-id="942c5-134">Jednak ataków CSRF nie są ograniczone do wykorzystania plików cookie.</span><span class="sxs-lookup"><span data-stu-id="942c5-134">However, CSRF attacks aren't limited to exploiting cookies.</span></span> <span data-ttu-id="942c5-135">Na przykład uwierzytelnianie podstawowe i szyfrowane są również narażone.</span><span class="sxs-lookup"><span data-stu-id="942c5-135">For example, Basic and Digest authentication are also vulnerable.</span></span> <span data-ttu-id="942c5-136">Po zalogowaniu się przy użyciu uwierzytelniania podstawowe lub szyfrowane, przeglądarka automatycznie wysyła poświadczenia do sesji&dagger; kończy.</span><span class="sxs-lookup"><span data-stu-id="942c5-136">After a user signs in with Basic or Digest authentication, the browser automatically sends the credentials until the session&dagger; ends.</span></span>

<span data-ttu-id="942c5-137">&dagger;W tym kontekście *sesji* odwołuje się do sesji po stronie klienta, w którym użytkownik jest uwierzytelniony.</span><span class="sxs-lookup"><span data-stu-id="942c5-137">&dagger;In this context, *session* refers to the client-side session during which the user is authenticated.</span></span> <span data-ttu-id="942c5-138">Jest niezwiązanych ze sobą do sesji po stronie serwera lub [platformy ASP.NET Core sesji w oprogramowaniu pośredniczącym](xref:fundamentals/app-state).</span><span class="sxs-lookup"><span data-stu-id="942c5-138">It's unrelated to server-side sessions or [ASP.NET Core Session Middleware](xref:fundamentals/app-state).</span></span>

<span data-ttu-id="942c5-139">Użytkownicy mogą zabezpieczyć się przed CSRF luk w zabezpieczeniach, wykonując różne środki ostrożności:</span><span class="sxs-lookup"><span data-stu-id="942c5-139">Users can guard against CSRF vulnerabilities by taking precautions:</span></span>

* <span data-ttu-id="942c5-140">Zaloguj się na zniżki w stosunku do aplikacji sieci web po zakończeniu korzystania z nich.</span><span class="sxs-lookup"><span data-stu-id="942c5-140">Sign off of web apps when finished using them.</span></span>
* <span data-ttu-id="942c5-141">Wyczyść pliki cookie przeglądarki okresowo.</span><span class="sxs-lookup"><span data-stu-id="942c5-141">Clear browser cookies periodically.</span></span>

<span data-ttu-id="942c5-142">Luki w zabezpieczeniach CSRF są jednak zasadniczo problem z aplikacją sieci web, a nie użytkownika końcowego.</span><span class="sxs-lookup"><span data-stu-id="942c5-142">However, CSRF vulnerabilities are fundamentally a problem with the web app, not the end user.</span></span>

## <a name="authentication-fundamentals"></a><span data-ttu-id="942c5-143">Podstawowe informacje dotyczące uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="942c5-143">Authentication fundamentals</span></span>

<span data-ttu-id="942c5-144">Uwierzytelnianie na podstawie plików cookie jest popularnych formy uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="942c5-144">Cookie-based authentication is a popular form of authentication.</span></span> <span data-ttu-id="942c5-145">Systemy uwierzytelniania opartego na tokenach rosnące popularność, szczególnie w przypadku aplikacje jednostronicowe (źródła).</span><span class="sxs-lookup"><span data-stu-id="942c5-145">Token-based authentication systems are growing in popularity, especially for Single Page Applications (SPAs).</span></span>

### <a name="cookie-based-authentication"></a><span data-ttu-id="942c5-146">Na podstawie plików cookie uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="942c5-146">Cookie-based authentication</span></span>

<span data-ttu-id="942c5-147">Gdy użytkownik jest uwierzytelniany przy użyciu nazwy użytkownika i hasła, one wystawiony token token zawierający bilet uwierzytelniania, który może służyć do uwierzytelniania i autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="942c5-147">When a user authenticates using their username and password, they're issued a token, containing an authentication ticket that can be used for authentication and authorization.</span></span> <span data-ttu-id="942c5-148">Token jest przechowywany jako sprawia, że plik cookie, który towarzyszy każde żądanie klienta.</span><span class="sxs-lookup"><span data-stu-id="942c5-148">The token is stored as a cookie that accompanies every request the client makes.</span></span> <span data-ttu-id="942c5-149">Generowanie i weryfikowania tego pliku cookie odbywa się przez oprogramowanie pośredniczące uwierzytelniania plików Cookie.</span><span class="sxs-lookup"><span data-stu-id="942c5-149">Generating and validating this cookie is performed by the Cookie Authentication Middleware.</span></span> <span data-ttu-id="942c5-150">[Oprogramowania pośredniczącego](xref:fundamentals/middleware/index) serializuje głównej nazwy użytkownika w zaszyfrowanym pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="942c5-150">The [middleware](xref:fundamentals/middleware/index) serializes a user principal into an encrypted cookie.</span></span> <span data-ttu-id="942c5-151">Kolejne żądania oprogramowania pośredniczącego sprawdza poprawność pliku cookie, ponownie tworzy nazwę główną i przypisuje podmiotowi zabezpieczeń na [użytkownika](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user) właściwość [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext).</span><span class="sxs-lookup"><span data-stu-id="942c5-151">On subsequent requests, the middleware validates the cookie, recreates the principal, and assigns the principal to the [User](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user) property of [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext).</span></span>

### <a name="token-based-authentication"></a><span data-ttu-id="942c5-152">Uwierzytelnianie oparte na tokenie</span><span class="sxs-lookup"><span data-stu-id="942c5-152">Token-based authentication</span></span>

<span data-ttu-id="942c5-153">Gdy użytkownik jest uwierzytelniany, one wystawiony token token (nie antiforgery token).</span><span class="sxs-lookup"><span data-stu-id="942c5-153">When a user is authenticated, they're issued a token (not an antiforgery token).</span></span> <span data-ttu-id="942c5-154">Token zawiera informacje o użytkowniku w formie [oświadczeń](/dotnet/framework/security/claims-based-identity-model) lub tokenu odwołania, który wskazuje aplikację do stanu użytkownika w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="942c5-154">The token contains user information in the form of [claims](/dotnet/framework/security/claims-based-identity-model) or a reference token that points the app to user state maintained in the app.</span></span> <span data-ttu-id="942c5-155">Gdy użytkownik próbuje uzyskać dostęp do zasobu wymaga uwierzytelnienia, token jest wysyłany do aplikacji przy użyciu nagłówka autoryzacji dodatkowe w postaci tokenu elementu nośnego.</span><span class="sxs-lookup"><span data-stu-id="942c5-155">When a user attempts to access a resource requiring authentication, the token is sent to the app with an additional authorization header in form of Bearer token.</span></span> <span data-ttu-id="942c5-156">To sprawia, że aplikacja bezstanowe.</span><span class="sxs-lookup"><span data-stu-id="942c5-156">This makes the app stateless.</span></span> <span data-ttu-id="942c5-157">W kolejnych żądań token jest przekazywany w żądaniu weryfikacji po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="942c5-157">In each subsequent request, the token is passed in the request for server-side validation.</span></span> <span data-ttu-id="942c5-158">Ten token nie jest *zaszyfrowanych*; ma ona *zakodowane*.</span><span class="sxs-lookup"><span data-stu-id="942c5-158">This token isn't *encrypted*; it's *encoded*.</span></span> <span data-ttu-id="942c5-159">Na serwerze token jest dekodowana uzyskiwać dostęp do swoich informacji.</span><span class="sxs-lookup"><span data-stu-id="942c5-159">On the server, the token is decoded to access its information.</span></span> <span data-ttu-id="942c5-160">Aby wysłać token dla kolejnych żądań, należy przechowywać token w magazynie lokalnym w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="942c5-160">To send the token on subsequent requests, store the token in the browser's local storage.</span></span> <span data-ttu-id="942c5-161">Nie można zajmującym się ochroną CSRF luk w zabezpieczeniach, jeśli token jest przechowywany w magazynie lokalnym w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="942c5-161">Don't be concerned about CSRF vulnerability if the token is stored in the browser's local storage.</span></span> <span data-ttu-id="942c5-162">CSRF jest istotny, gdy token jest przechowywany w pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="942c5-162">CSRF is a concern when the token is stored in a cookie.</span></span>

### <a name="multiple-apps-hosted-at-one-domain"></a><span data-ttu-id="942c5-163">Wiele aplikacji hostowanej w jednej domenie</span><span class="sxs-lookup"><span data-stu-id="942c5-163">Multiple apps hosted at one domain</span></span>

<span data-ttu-id="942c5-164">Udostępnione środowiska hostingu są podatne na przejęcie kontroli sesji logowania CSRF i inne ataki.</span><span class="sxs-lookup"><span data-stu-id="942c5-164">Shared hosting environments are vulnerable to session hijacking, login CSRF, and other attacks.</span></span>

<span data-ttu-id="942c5-165">Mimo że `example1.contoso.net` i `example2.contoso.net` różnych hostów istnieje relacja nawiązywanie niejawnych relacji zaufania między hostami w ramach `*.contoso.net` domeny.</span><span class="sxs-lookup"><span data-stu-id="942c5-165">Although `example1.contoso.net` and `example2.contoso.net` are different hosts, there's an implicit trust relationship between hosts under the `*.contoso.net` domain.</span></span> <span data-ttu-id="942c5-166">Ta relacja niejawne zaufanie umożliwia potencjalnie niezaufane hosty wpływać na siebie nawzajem pliki cookie (zasady tego samego źródła, które określają sposób wysyłanie żądań AJAX nie zawsze dotyczą plików cookie protokołu HTTP).</span><span class="sxs-lookup"><span data-stu-id="942c5-166">This implicit trust relationship allows potentially untrusted hosts to affect each other's cookies (the same-origin policies that govern AJAX requests don't necessarily apply to HTTP cookies).</span></span>

<span data-ttu-id="942c5-167">Można zapobiec ataków wykorzystujących zaufanych plików cookie między aplikacjami hostowanych na tej samej domenie, nie udostępniając domen.</span><span class="sxs-lookup"><span data-stu-id="942c5-167">Attacks that exploit trusted cookies between apps hosted on the same domain can be prevented by not sharing domains.</span></span> <span data-ttu-id="942c5-168">Gdy każda aplikacja jest hostowana na własnej domeny, istnieje relacja zaufania niejawnych plików cookie w celu wykorzystania.</span><span class="sxs-lookup"><span data-stu-id="942c5-168">When each app is hosted on its own domain, there is no implicit cookie trust relationship to exploit.</span></span>

## <a name="aspnet-core-antiforgery-configuration"></a><span data-ttu-id="942c5-169">Konfiguracja antiforgery platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="942c5-169">ASP.NET Core antiforgery configuration</span></span>

> [!WARNING]
> <span data-ttu-id="942c5-170">Platforma ASP.NET Core implementuje antiforgery przy użyciu [ochrony danych programu ASP.NET Core](xref:security/data-protection/introduction).</span><span class="sxs-lookup"><span data-stu-id="942c5-170">ASP.NET Core implements antiforgery using [ASP.NET Core Data Protection](xref:security/data-protection/introduction).</span></span> <span data-ttu-id="942c5-171">Stos ochrony danych musi być skonfigurowany do pracy w farmie serwerów.</span><span class="sxs-lookup"><span data-stu-id="942c5-171">The data protection stack must be configured to work in a server farm.</span></span> <span data-ttu-id="942c5-172">Zobacz [Konfigurowanie ochrony danych](xref:security/data-protection/configuration/overview) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="942c5-172">See [Configuring data protection](xref:security/data-protection/configuration/overview) for more information.</span></span>

<span data-ttu-id="942c5-173">W programie ASP.NET Core 2.0 lub nowszej [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) wprowadza antiforgery tokenów do elementów formularza HTML.</span><span class="sxs-lookup"><span data-stu-id="942c5-173">In ASP.NET Core 2.0 or later, the [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) injects antiforgery tokens into HTML form elements.</span></span> <span data-ttu-id="942c5-174">Następujący kod w pliku Razor automatycznie generuje antiforgery tokenów:</span><span class="sxs-lookup"><span data-stu-id="942c5-174">The following markup in a Razor file automatically generates antiforgery tokens:</span></span>

```cshtml
<form method="post">
    ...
</form>
```

<span data-ttu-id="942c5-175">Similarily, [IHtmlHelper.BeginForm](/dotnet/api/microsoft.aspnetcore.mvc.rendering.ihtmlhelper.beginform) generuje tokeny antiforgery domyślnie, jeśli metoda formularza nie GET.</span><span class="sxs-lookup"><span data-stu-id="942c5-175">Similarily, [IHtmlHelper.BeginForm](/dotnet/api/microsoft.aspnetcore.mvc.rendering.ihtmlhelper.beginform) generates antiforgery tokens by default if the form's method isn't GET.</span></span>

<span data-ttu-id="942c5-176">Automatyczne generowanie tokenów antiforgery dla elementów formularza HTML się dzieje po `<form>` tag zawiera `method="post"` atrybut i jednej z następujących mają wartość true:</span><span class="sxs-lookup"><span data-stu-id="942c5-176">The automatic generation of antiforgery tokens for HTML form elements happens when the `<form>` tag contains the `method="post"` attribute and either of the following are true:</span></span>

  * <span data-ttu-id="942c5-177">Atrybut akcji jest pusty (`action=""`).</span><span class="sxs-lookup"><span data-stu-id="942c5-177">The action attribute is empty (`action=""`).</span></span>
  * <span data-ttu-id="942c5-178">Nie został dostarczony atrybut akcji (`<form method="post">`).</span><span class="sxs-lookup"><span data-stu-id="942c5-178">The action attribute isn't supplied (`<form method="post">`).</span></span>

<span data-ttu-id="942c5-179">Można wyłączyć automatyczne generowanie tokenów antiforgery dla elementów formularza HTML:</span><span class="sxs-lookup"><span data-stu-id="942c5-179">Automatic generation of antiforgery tokens for HTML form elements can be disabled:</span></span>

* <span data-ttu-id="942c5-180">Jawnie wyłączyć antiforgery tokenów z informacjami o `asp-antiforgery` atrybutu:</span><span class="sxs-lookup"><span data-stu-id="942c5-180">Explicitly disable antiforgery tokens with the `asp-antiforgery` attribute:</span></span>

  ```cshtml
  <form method="post" asp-antiforgery="false">
      ...
  </form>
  ```

* <span data-ttu-id="942c5-181">Element formularza jest wyłączony w poziomie dla pomocników tagów przy użyciu Pomocnika tagów [! symbol rezygnacji](xref:mvc/views/tag-helpers/intro#opt-out):</span><span class="sxs-lookup"><span data-stu-id="942c5-181">The form element is opted-out of Tag Helpers by using the Tag Helper [! opt-out symbol](xref:mvc/views/tag-helpers/intro#opt-out):</span></span>

  ```cshtml
  <!form method="post">
      ...
  </!form>
  ```

* <span data-ttu-id="942c5-182">Usuń `FormTagHelper` z widoku.</span><span class="sxs-lookup"><span data-stu-id="942c5-182">Remove the `FormTagHelper` from the view.</span></span> <span data-ttu-id="942c5-183">`FormTagHelper` Może zostać usunięty z widokiem, dodając następujące dyrektywy do widoku Razor:</span><span class="sxs-lookup"><span data-stu-id="942c5-183">The `FormTagHelper` can be removed from a view by adding the following directive to the Razor view:</span></span>

  ```cshtml
  @removeTagHelper Microsoft.AspNetCore.Mvc.TagHelpers.FormTagHelper, Microsoft.AspNetCore.Mvc.TagHelpers
  ```

> [!NOTE]
> <span data-ttu-id="942c5-184">[Strony razor](xref:razor-pages/index) są automatycznie chronione przed XSRF/CSRF.</span><span class="sxs-lookup"><span data-stu-id="942c5-184">[Razor Pages](xref:razor-pages/index) are automatically protected from XSRF/CSRF.</span></span> <span data-ttu-id="942c5-185">Aby uzyskać więcej informacji, zobacz [XSRF/CSRF i stron Razor](xref:razor-pages/index#xsrf).</span><span class="sxs-lookup"><span data-stu-id="942c5-185">For more information, see [XSRF/CSRF and Razor Pages](xref:razor-pages/index#xsrf).</span></span>

<span data-ttu-id="942c5-186">Najbardziej typowe podejście do obrony przed atakami CSRF jest użycie *wzorzec tokenu Synchronizator* (STP).</span><span class="sxs-lookup"><span data-stu-id="942c5-186">The most common approach to defending against CSRF attacks is to use the *Synchronizer Token Pattern* (STP).</span></span> <span data-ttu-id="942c5-187">STP jest używany, gdy użytkownik zażąda strony z danymi formularza:</span><span class="sxs-lookup"><span data-stu-id="942c5-187">STP is used when the user requests a page with form data:</span></span>

1. <span data-ttu-id="942c5-188">Serwer wysyła token skojarzone z tożsamością bieżącego użytkownika do klienta.</span><span class="sxs-lookup"><span data-stu-id="942c5-188">The server sends a token associated with the current user's identity to the client.</span></span>
1. <span data-ttu-id="942c5-189">Klient wysyła z powrotem token do serwera w celu weryfikacji.</span><span class="sxs-lookup"><span data-stu-id="942c5-189">The client sends back the token to the server for verification.</span></span>
1. <span data-ttu-id="942c5-190">Jeśli serwer odbiera token, który nie jest zgodna tożsamość uwierzytelnionego użytkownika, żądanie zostanie odrzucone.</span><span class="sxs-lookup"><span data-stu-id="942c5-190">If the server receives a token that doesn't match the authenticated user's identity, the request is rejected.</span></span>

<span data-ttu-id="942c5-191">Token jest unikatowy i nieprzewidywalne.</span><span class="sxs-lookup"><span data-stu-id="942c5-191">The token is unique and unpredictable.</span></span> <span data-ttu-id="942c5-192">Token, również może służyć do zapewnienia prawidłowego sekwencjonowania serii żądań (na przykład, zapewniając sekwencji żądanie: stronie 1 &ndash; stronie 2 &ndash; strony 3).</span><span class="sxs-lookup"><span data-stu-id="942c5-192">The token can also be used to ensure proper sequencing of a series of requests (for example, ensuring the request sequence of: page 1 &ndash; page 2 &ndash; page 3).</span></span> <span data-ttu-id="942c5-193">Wszystkie formularze w szablonach ASP.NET Core MVC i stron Razor generowania antiforgery tokenów.</span><span class="sxs-lookup"><span data-stu-id="942c5-193">All of the forms in ASP.NET Core MVC and Razor Pages templates generate antiforgery tokens.</span></span> <span data-ttu-id="942c5-194">Następujące pary Wyświetl przykłady generowania antiforgery tokenów:</span><span class="sxs-lookup"><span data-stu-id="942c5-194">The following pair of view examples generate antiforgery tokens:</span></span>

```cshtml
<form asp-controller="Manage" asp-action="ChangePassword" method="post">
    ...
</form>

@using (Html.BeginForm("ChangePassword", "Manage"))
{
    ...
}
```

<span data-ttu-id="942c5-195">Jawnie dodać token antiforgery na `<form>` elementu bez używanie pomocników tagów przy użyciu Pomocnika kodu HTML [ @Html.AntiForgeryToken ](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.htmlhelper.antiforgerytoken):</span><span class="sxs-lookup"><span data-stu-id="942c5-195">Explicitly add an antiforgery token to a `<form>` element without using Tag Helpers with the HTML helper [@Html.AntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.htmlhelper.antiforgerytoken):</span></span>

```cshtml
<form action="/" method="post">
    @Html.AntiForgeryToken()
</form>
```

<span data-ttu-id="942c5-196">We wszystkich powyższych przypadkach ASP.NET Core dodaje ukryte pole formularza podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="942c5-196">In each of the preceding cases, ASP.NET Core adds a hidden form field similar to the following:</span></span>

```cshtml
<input name="__RequestVerificationToken" type="hidden" value="CfDJ8NrAkS ... s2-m9Yw">
```

<span data-ttu-id="942c5-197">Platforma ASP.NET Core obejmuje trzy [filtry](xref:mvc/controllers/filters) do pracy z tokenami antiforgery:</span><span class="sxs-lookup"><span data-stu-id="942c5-197">ASP.NET Core includes three [filters](xref:mvc/controllers/filters) for working with antiforgery tokens:</span></span>

* [<span data-ttu-id="942c5-198">ValidateAntiForgeryToken</span><span class="sxs-lookup"><span data-stu-id="942c5-198">ValidateAntiForgeryToken</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute)
* [<span data-ttu-id="942c5-199">AutoValidateAntiforgeryToken</span><span class="sxs-lookup"><span data-stu-id="942c5-199">AutoValidateAntiforgeryToken</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute)
* [<span data-ttu-id="942c5-200">IgnoreAntiforgeryToken</span><span class="sxs-lookup"><span data-stu-id="942c5-200">IgnoreAntiforgeryToken</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute)

## <a name="antiforgery-options"></a><span data-ttu-id="942c5-201">Opcje antiforgery</span><span class="sxs-lookup"><span data-stu-id="942c5-201">Antiforgery options</span></span>

<span data-ttu-id="942c5-202">Dostosowywanie [antiforgery opcje](/dotnet/api/Microsoft.AspNetCore.Antiforgery.AntiforgeryOptions) w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="942c5-202">Customize [antiforgery options](/dotnet/api/Microsoft.AspNetCore.Antiforgery.AntiforgeryOptions) in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
services.AddAntiforgery(options => 
{
    // Set Cookie properties using CookieBuilder properties†.
    options.FormFieldName = "AntiforgeryFieldname";
    options.HeaderName = "X-CSRF-TOKEN-HEADERNAME";
    options.SuppressXFrameOptionsHeader = false;
});
```

<span data-ttu-id="942c5-203">&dagger;Ustaw antiforgery `Cookie` właściwości za pomocą właściwości [CookieBuilder](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder) klasy.</span><span class="sxs-lookup"><span data-stu-id="942c5-203">&dagger;Set the antiforgery `Cookie` properties using the properties of the [CookieBuilder](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder) class.</span></span>

| <span data-ttu-id="942c5-204">Opcja</span><span class="sxs-lookup"><span data-stu-id="942c5-204">Option</span></span> | <span data-ttu-id="942c5-205">Opis</span><span class="sxs-lookup"><span data-stu-id="942c5-205">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="942c5-206">Cookie</span><span class="sxs-lookup"><span data-stu-id="942c5-206">Cookie</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookie) | <span data-ttu-id="942c5-207">Określa ustawienia używane do tworzenia antiforgery plików cookie.</span><span class="sxs-lookup"><span data-stu-id="942c5-207">Determines the settings used to create the antiforgery cookies.</span></span> |
| [<span data-ttu-id="942c5-208">FormFieldName</span><span class="sxs-lookup"><span data-stu-id="942c5-208">FormFieldName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.formfieldname) | <span data-ttu-id="942c5-209">Nazwa pola formularza, używany przez antiforgery system do renderowania antiforgery tokenów w widokach.</span><span class="sxs-lookup"><span data-stu-id="942c5-209">The name of the hidden form field used by the antiforgery system to render antiforgery tokens in views.</span></span> |
| [<span data-ttu-id="942c5-210">HeaderName</span><span class="sxs-lookup"><span data-stu-id="942c5-210">HeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.headername) | <span data-ttu-id="942c5-211">Nazwa nagłówka używany przez antiforgery systemu.</span><span class="sxs-lookup"><span data-stu-id="942c5-211">The name of the header used by the antiforgery system.</span></span> <span data-ttu-id="942c5-212">Jeśli `null`, system uwzględnia tylko dane formularza.</span><span class="sxs-lookup"><span data-stu-id="942c5-212">If `null`, the system considers only form data.</span></span> |
| [<span data-ttu-id="942c5-213">SuppressXFrameOptionsHeader</span><span class="sxs-lookup"><span data-stu-id="942c5-213">SuppressXFrameOptionsHeader</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.suppressxframeoptionsheader) | <span data-ttu-id="942c5-214">Określa, czy pominąć Generowanie `X-Frame-Options` nagłówka.</span><span class="sxs-lookup"><span data-stu-id="942c5-214">Specifies whether to suppress generation of the `X-Frame-Options` header.</span></span> <span data-ttu-id="942c5-215">Domyślnie nagłówek jest generowany z wartością "SAMEORIGIN".</span><span class="sxs-lookup"><span data-stu-id="942c5-215">By default, the header is generated with a value of "SAMEORIGIN".</span></span> <span data-ttu-id="942c5-216">Wartość domyślna to `false`.</span><span class="sxs-lookup"><span data-stu-id="942c5-216">Defaults to `false`.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
services.AddAntiforgery(options => 
{
    options.CookieDomain = "contoso.com";
    options.CookieName = "X-CSRF-TOKEN-COOKIENAME";
    options.CookiePath = "Path";
    options.FormFieldName = "AntiforgeryFieldname";
    options.HeaderName = "X-CSRF-TOKEN-HEADERNAME";
    options.RequireSsl = false;
    options.SuppressXFrameOptionsHeader = false;
});
```

| <span data-ttu-id="942c5-217">Opcja</span><span class="sxs-lookup"><span data-stu-id="942c5-217">Option</span></span> | <span data-ttu-id="942c5-218">Opis</span><span class="sxs-lookup"><span data-stu-id="942c5-218">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="942c5-219">Cookie</span><span class="sxs-lookup"><span data-stu-id="942c5-219">Cookie</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookie) | <span data-ttu-id="942c5-220">Określa ustawienia używane do tworzenia antiforgery plików cookie.</span><span class="sxs-lookup"><span data-stu-id="942c5-220">Determines the settings used to create the antiforgery cookies.</span></span> |
| [<span data-ttu-id="942c5-221">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="942c5-221">CookieDomain</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiedomain) | <span data-ttu-id="942c5-222">Domena pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="942c5-222">The domain of the cookie.</span></span> <span data-ttu-id="942c5-223">Wartość domyślna to `null`.</span><span class="sxs-lookup"><span data-stu-id="942c5-223">Defaults to `null`.</span></span> <span data-ttu-id="942c5-224">Ta właściwość jest przestarzała i zostanie usunięta w przyszłej wersji.</span><span class="sxs-lookup"><span data-stu-id="942c5-224">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="942c5-225">Zalecaną alternatywą jest Cookie.Domain.</span><span class="sxs-lookup"><span data-stu-id="942c5-225">The recommended alternative is Cookie.Domain.</span></span> |
| [<span data-ttu-id="942c5-226">CookieName</span><span class="sxs-lookup"><span data-stu-id="942c5-226">CookieName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiename) | <span data-ttu-id="942c5-227">Nazwa pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="942c5-227">The name of the cookie.</span></span> <span data-ttu-id="942c5-228">Jeśli nie jest ustawiona, system generuje unikatowa nazwa zaczyna się od [DefaultCookiePrefix](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.defaultcookieprefix) (". AspNetCore.Antiforgery.").</span><span class="sxs-lookup"><span data-stu-id="942c5-228">If not set, the system generates a unique name beginning with the [DefaultCookiePrefix](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.defaultcookieprefix) (".AspNetCore.Antiforgery.").</span></span> <span data-ttu-id="942c5-229">Ta właściwość jest przestarzała i zostanie usunięta w przyszłej wersji.</span><span class="sxs-lookup"><span data-stu-id="942c5-229">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="942c5-230">Zalecaną alternatywą jest Cookie.Name.</span><span class="sxs-lookup"><span data-stu-id="942c5-230">The recommended alternative is Cookie.Name.</span></span> |
| [<span data-ttu-id="942c5-231">CookiePath</span><span class="sxs-lookup"><span data-stu-id="942c5-231">CookiePath</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiepath) | <span data-ttu-id="942c5-232">Ścieżka zestawu w pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="942c5-232">The path set on the cookie.</span></span> <span data-ttu-id="942c5-233">Ta właściwość jest przestarzała i zostanie usunięta w przyszłej wersji.</span><span class="sxs-lookup"><span data-stu-id="942c5-233">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="942c5-234">Zalecaną alternatywą jest Cookie.Path.</span><span class="sxs-lookup"><span data-stu-id="942c5-234">The recommended alternative is Cookie.Path.</span></span> |
| [<span data-ttu-id="942c5-235">FormFieldName</span><span class="sxs-lookup"><span data-stu-id="942c5-235">FormFieldName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.formfieldname) | <span data-ttu-id="942c5-236">Nazwa pola formularza, używany przez antiforgery system do renderowania antiforgery tokenów w widokach.</span><span class="sxs-lookup"><span data-stu-id="942c5-236">The name of the hidden form field used by the antiforgery system to render antiforgery tokens in views.</span></span> |
| [<span data-ttu-id="942c5-237">HeaderName</span><span class="sxs-lookup"><span data-stu-id="942c5-237">HeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.headername) | <span data-ttu-id="942c5-238">Nazwa nagłówka używany przez antiforgery systemu.</span><span class="sxs-lookup"><span data-stu-id="942c5-238">The name of the header used by the antiforgery system.</span></span> <span data-ttu-id="942c5-239">Jeśli `null`, system uwzględnia tylko dane formularza.</span><span class="sxs-lookup"><span data-stu-id="942c5-239">If `null`, the system considers only form data.</span></span> |
| [<span data-ttu-id="942c5-240">RequireSsl</span><span class="sxs-lookup"><span data-stu-id="942c5-240">RequireSsl</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.requiressl) | <span data-ttu-id="942c5-241">Określa, czy protokół HTTPS jest wymagany przez antiforgery system.</span><span class="sxs-lookup"><span data-stu-id="942c5-241">Specifies whether HTTPS is required by the antiforgery system.</span></span> <span data-ttu-id="942c5-242">Jeśli `true`, Niepowodzenie żądania innego niż HTTPS.</span><span class="sxs-lookup"><span data-stu-id="942c5-242">If `true`, non-HTTPS requests fail.</span></span> <span data-ttu-id="942c5-243">Wartość domyślna to `false`.</span><span class="sxs-lookup"><span data-stu-id="942c5-243">Defaults to `false`.</span></span> <span data-ttu-id="942c5-244">Ta właściwość jest przestarzała i zostanie usunięta w przyszłej wersji.</span><span class="sxs-lookup"><span data-stu-id="942c5-244">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="942c5-245">Zalecaną alternatywą jest Cookie.SecurePolicy.</span><span class="sxs-lookup"><span data-stu-id="942c5-245">The recommended alternative is to set Cookie.SecurePolicy.</span></span> |
| [<span data-ttu-id="942c5-246">SuppressXFrameOptionsHeader</span><span class="sxs-lookup"><span data-stu-id="942c5-246">SuppressXFrameOptionsHeader</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.suppressxframeoptionsheader) | <span data-ttu-id="942c5-247">Określa, czy pominąć Generowanie `X-Frame-Options` nagłówka.</span><span class="sxs-lookup"><span data-stu-id="942c5-247">Specifies whether to suppress generation of the `X-Frame-Options` header.</span></span> <span data-ttu-id="942c5-248">Domyślnie nagłówek jest generowany z wartością "SAMEORIGIN".</span><span class="sxs-lookup"><span data-stu-id="942c5-248">By default, the header is generated with a value of "SAMEORIGIN".</span></span> <span data-ttu-id="942c5-249">Wartość domyślna to `false`.</span><span class="sxs-lookup"><span data-stu-id="942c5-249">Defaults to `false`.</span></span> |

::: moniker-end

<span data-ttu-id="942c5-250">Aby uzyskać więcej informacji, zobacz [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions).</span><span class="sxs-lookup"><span data-stu-id="942c5-250">For more information, see [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions).</span></span>

## <a name="configure-antiforgery-features-with-iantiforgery"></a><span data-ttu-id="942c5-251">Konfigurowanie funkcji antiforgery z IAntiforgery</span><span class="sxs-lookup"><span data-stu-id="942c5-251">Configure antiforgery features with IAntiforgery</span></span>

<span data-ttu-id="942c5-252">[IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) udostępnia interfejs API w celu skonfigurowania funkcji antiforgery.</span><span class="sxs-lookup"><span data-stu-id="942c5-252">[IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) provides the API to configure antiforgery features.</span></span> <span data-ttu-id="942c5-253">`IAntiforgery` można zażądać w `Configure` metody `Startup` klasy.</span><span class="sxs-lookup"><span data-stu-id="942c5-253">`IAntiforgery` can be requested in the `Configure` method of the `Startup` class.</span></span> <span data-ttu-id="942c5-254">W poniższym przykładzie użyto oprogramowania pośredniczącego ze strony głównej aplikacji do wygenerowania tokenu antiforgery i wysłać w odpowiedzi jako plik cookie (przy użyciu domyślnego Angular konwencji nazewnictwa opisane w dalszej części tego tematu):</span><span class="sxs-lookup"><span data-stu-id="942c5-254">The following example uses middleware from the app's home page to generate an antiforgery token and send it in the response as a cookie (using the default Angular naming convention described later in this topic):</span></span>

```csharp
public void Configure(IApplicationBuilder app, IAntiforgery antiforgery)
{
    app.Use(next => context =>
    {
        string path = context.Request.Path.Value;

        if (
            string.Equals(path, "/", StringComparison.OrdinalIgnoreCase) ||
            string.Equals(path, "/index.html", StringComparison.OrdinalIgnoreCase))
        {
            // The request token can be sent as a JavaScript-readable cookie, 
            // and Angular uses it by default.
            var tokens = antiforgery.GetAndStoreTokens(context);
            context.Response.Cookies.Append("XSRF-TOKEN", tokens.RequestToken, 
                new CookieOptions() { HttpOnly = false });
        }

        return next(context);
    });
}
```

### <a name="require-antiforgery-validation"></a><span data-ttu-id="942c5-255">Wymagaj weryfikacji antiforgery</span><span class="sxs-lookup"><span data-stu-id="942c5-255">Require antiforgery validation</span></span>

<span data-ttu-id="942c5-256">[ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute) jest filtr akcji, które mogą być stosowane do poszczególnych akcji, kontrolera, lub globalnie.</span><span class="sxs-lookup"><span data-stu-id="942c5-256">[ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute) is an action filter that can be applied to an individual action, a controller, or globally.</span></span> <span data-ttu-id="942c5-257">Żądania kierowane do akcji, dla których zastosowano ten filtr są blokowane, chyba że żądanie zawiera prawidłowy token antiforgery.</span><span class="sxs-lookup"><span data-stu-id="942c5-257">Requests made to actions that have this filter applied are blocked unless the request includes a valid antiforgery token.</span></span>

```csharp
[HttpPost]
[ValidateAntiForgeryToken]
public async Task<IActionResult> RemoveLogin(RemoveLoginViewModel account)
{
    ManageMessageId? message = ManageMessageId.Error;
    var user = await GetCurrentUserAsync();

    if (user != null)
    {
        var result = 
            await _userManager.RemoveLoginAsync(
                user, account.LoginProvider, account.ProviderKey);

        if (result.Succeeded)
        {
            await _signInManager.SignInAsync(user, isPersistent: false);
            message = ManageMessageId.RemoveLoginSuccess;
        }
    }

    return RedirectToAction(nameof(ManageLogins), new { Message = message });
}
```

<span data-ttu-id="942c5-258">`ValidateAntiForgeryToken` Atrybut wymaga tokenu dla żądań kierowanych do metody akcji, rozszerza, łącznie z żądania HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="942c5-258">The `ValidateAntiForgeryToken` attribute requires a token for requests to the action methods it decorates, including HTTP GET requests.</span></span> <span data-ttu-id="942c5-259">Jeśli `ValidateAntiForgeryToken` atrybut jest stosowany w kontrolerach aplikacji, może zostać zastąpiona przez `IgnoreAntiforgeryToken` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="942c5-259">If the `ValidateAntiForgeryToken` attribute is applied across the app's controllers, it can be overridden with the `IgnoreAntiforgeryToken` attribute.</span></span>

> [!NOTE]
> <span data-ttu-id="942c5-260">Platforma ASP.NET Core nie obsługuje automatyczne dodawanie tokenów antiforgery żądania GET.</span><span class="sxs-lookup"><span data-stu-id="942c5-260">ASP.NET Core doesn't support adding antiforgery tokens to GET requests automatically.</span></span>

### <a name="automatically-validate-antiforgery-tokens-for-unsafe-http-methods-only"></a><span data-ttu-id="942c5-261">Automatycznie weryfikować tokeny antiforgery niebezpieczne metod HTTP</span><span class="sxs-lookup"><span data-stu-id="942c5-261">Automatically validate antiforgery tokens for unsafe HTTP methods only</span></span>

<span data-ttu-id="942c5-262">Aplikacje platformy ASP.NET Core nie Generuj antiforgery tokenów dla bezpiecznych metod HTTP (GET, HEAD, opcji i śledzenia).</span><span class="sxs-lookup"><span data-stu-id="942c5-262">ASP.NET Core apps don't generate antiforgery tokens for safe HTTP methods (GET, HEAD, OPTIONS, and TRACE).</span></span> <span data-ttu-id="942c5-263">Zamiast stosowania szeroko `ValidateAntiForgeryToken` atrybutu, a następnie przesłanianie go za pomocą `IgnoreAntiforgeryToken` atrybutów, [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute) atrybut może być używany.</span><span class="sxs-lookup"><span data-stu-id="942c5-263">Instead of broadly applying the `ValidateAntiForgeryToken` attribute and then overriding it with `IgnoreAntiforgeryToken` attributes, the [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute) attribute can be used.</span></span> <span data-ttu-id="942c5-264">Ten atrybut działa identycznie do `ValidateAntiForgeryToken` atrybutów, z tą różnicą, że nie wymaga tokeny żądań zostało nawiązane przy użyciu następujących metod HTTP:</span><span class="sxs-lookup"><span data-stu-id="942c5-264">This attribute works identically to the `ValidateAntiForgeryToken` attribute, except that it doesn't require tokens for requests made using the following HTTP methods:</span></span>

* <span data-ttu-id="942c5-265">GET</span><span class="sxs-lookup"><span data-stu-id="942c5-265">GET</span></span>
* <span data-ttu-id="942c5-266">GŁÓWNY</span><span class="sxs-lookup"><span data-stu-id="942c5-266">HEAD</span></span>
* <span data-ttu-id="942c5-267">OPCJE</span><span class="sxs-lookup"><span data-stu-id="942c5-267">OPTIONS</span></span>
* <span data-ttu-id="942c5-268">TRACE</span><span class="sxs-lookup"><span data-stu-id="942c5-268">TRACE</span></span>

<span data-ttu-id="942c5-269">Firma Microsoft zaleca użycie `AutoValidateAntiforgeryToken` szeroko w scenariuszach bez interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="942c5-269">We recommend use of `AutoValidateAntiforgeryToken` broadly for non-API scenarios.</span></span> <span data-ttu-id="942c5-270">Gwarantuje to, że akcja POST są chronione domyślnie.</span><span class="sxs-lookup"><span data-stu-id="942c5-270">This ensures POST actions are protected by default.</span></span> <span data-ttu-id="942c5-271">Alternatywą jest Ignoruj antiforgery tokenów domyślnie, chyba że `ValidateAntiForgeryToken` jest stosowany do poszczególnych metod akcji.</span><span class="sxs-lookup"><span data-stu-id="942c5-271">The alternative is to ignore antiforgery tokens by default, unless `ValidateAntiForgeryToken` is applied to individual action methods.</span></span> <span data-ttu-id="942c5-272">Je bardziej prawdopodobne w tym scenariuszu dla metody akcji POST pozostać niechronionej przez pomyłkę, wychodzenia z aplikacji jest narażony na ataki CSRF.</span><span class="sxs-lookup"><span data-stu-id="942c5-272">It's more likely in this scenario for a POST action method to be left unprotected by mistake, leaving the app vulnerable to CSRF attacks.</span></span> <span data-ttu-id="942c5-273">Wszystkie wpisy, należy wysłać antiforgery tokenu.</span><span class="sxs-lookup"><span data-stu-id="942c5-273">All POSTs should send the antiforgery token.</span></span>

<span data-ttu-id="942c5-274">Interfejsy API nie ma mechanizmu automatycznego wysyłania-cookie część tokenu.</span><span class="sxs-lookup"><span data-stu-id="942c5-274">APIs don't have an automatic mechanism for sending the non-cookie part of the token.</span></span> <span data-ttu-id="942c5-275">Implementacja prawdopodobnie zależy od implementacji kodu klienta.</span><span class="sxs-lookup"><span data-stu-id="942c5-275">The implementation probably depends on the client code implementation.</span></span> <span data-ttu-id="942c5-276">Poniżej przedstawiono kilka przykładów:</span><span class="sxs-lookup"><span data-stu-id="942c5-276">Some examples are shown below:</span></span>

<span data-ttu-id="942c5-277">Przykład na poziomie klasy:</span><span class="sxs-lookup"><span data-stu-id="942c5-277">Class-level example:</span></span>

```csharp
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
```

<span data-ttu-id="942c5-278">Przykład globalne:</span><span class="sxs-lookup"><span data-stu-id="942c5-278">Global example:</span></span>

```csharp
services.AddMvc(options => 
    options.Filters.Add(new AutoValidateAntiforgeryTokenAttribute()));
```

### <a name="override-global-or-controller-antiforgery-attributes"></a><span data-ttu-id="942c5-279">Zastąpienie globalny lub kontroler antiforgery atrybutów</span><span class="sxs-lookup"><span data-stu-id="942c5-279">Override global or controller antiforgery attributes</span></span>

<span data-ttu-id="942c5-280">[IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute) filtr jest używany, aby wyeliminować konieczność stosowania antiforgery token dla danej akcji (lub kontroler).</span><span class="sxs-lookup"><span data-stu-id="942c5-280">The [IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute) filter is used to eliminate the need for an antiforgery token for a given action (or controller).</span></span> <span data-ttu-id="942c5-281">Po zastosowaniu tego filtru zastępuje `ValidateAntiForgeryToken` i `AutoValidateAntiforgeryToken` filtry określone na wyższym poziomie (globalnie lub na innym kontrolerze).</span><span class="sxs-lookup"><span data-stu-id="942c5-281">When applied, this filter overrides `ValidateAntiForgeryToken` and `AutoValidateAntiforgeryToken` filters specified at a higher level (globally or on a controller).</span></span>

```csharp
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
    [HttpPost]
    [IgnoreAntiforgeryToken]
    public async Task<IActionResult> DoSomethingSafe(SomeViewModel model)
    {
        // no antiforgery token required
    }
}
```

## <a name="refresh-tokens-after-authentication"></a><span data-ttu-id="942c5-282">Tokenów odświeżania po uwierzytelnieniu</span><span class="sxs-lookup"><span data-stu-id="942c5-282">Refresh tokens after authentication</span></span>

<span data-ttu-id="942c5-283">Tokeny powinny być odświeżane po użytkownik jest uwierzytelniany przez przekierowanie użytkownika do widoku lub strony stron Razor.</span><span class="sxs-lookup"><span data-stu-id="942c5-283">Tokens should be refreshed after the user is authenticated by redirecting the user to a view or Razor Pages page.</span></span>

## <a name="javascript-ajax-and-spas"></a><span data-ttu-id="942c5-284">JavaScript, wywołań AJAX i aplikacji jednostronicowych</span><span class="sxs-lookup"><span data-stu-id="942c5-284">JavaScript, AJAX, and SPAs</span></span>

<span data-ttu-id="942c5-285">W tradycyjnych aplikacji opartych na języku HTML antiforgery tokeny są przekazywane do serwera przy użyciu pól formularza.</span><span class="sxs-lookup"><span data-stu-id="942c5-285">In traditional HTML-based apps, antiforgery tokens are passed to the server using hidden form fields.</span></span> <span data-ttu-id="942c5-286">Nowoczesne aplikacje oparte na języku JavaScript i aplikacji jednostronicowych wielu żądań programowo.</span><span class="sxs-lookup"><span data-stu-id="942c5-286">In modern JavaScript-based apps and SPAs, many requests are made programmatically.</span></span> <span data-ttu-id="942c5-287">Te żądania AJAX może używać innych technik (takie jak nagłówki żądania i pliki cookie) w celu wysyłania tokenu.</span><span class="sxs-lookup"><span data-stu-id="942c5-287">These AJAX requests may use other techniques (such as request headers or cookies) to send the token.</span></span>

<span data-ttu-id="942c5-288">Pliki cookie są używane do przechowywania tokenów uwierzytelniania i do uwierzytelniania żądań interfejsu API na serwerze, CSRF czy potencjalny problem.</span><span class="sxs-lookup"><span data-stu-id="942c5-288">If cookies are used to store authentication tokens and to authenticate API requests on the server, CSRF is a potential problem.</span></span> <span data-ttu-id="942c5-289">Jeśli Magazyn lokalny jest używany do przechowywania tokenu, CSRF luk w zabezpieczeniach może zminimalizować, ponieważ wartości z magazynu lokalnego nie są automatycznie przesyłane do serwera z każdym żądaniem.</span><span class="sxs-lookup"><span data-stu-id="942c5-289">If local storage is used to store the token, CSRF vulnerability might be mitigated because values from local storage aren't sent automatically to the server with every request.</span></span> <span data-ttu-id="942c5-290">W związku z tym przy użyciu lokalnego magazynu do przechowywania antiforgery token na kliencie oraz wysyłania tokenu, ponieważ nagłówek żądania jest zalecanym podejściem.</span><span class="sxs-lookup"><span data-stu-id="942c5-290">Thus, using local storage to store the antiforgery token on the client and sending the token as a request header is a recommended approach.</span></span>

### <a name="javascript"></a><span data-ttu-id="942c5-291">JavaScript</span><span class="sxs-lookup"><span data-stu-id="942c5-291">JavaScript</span></span>

<span data-ttu-id="942c5-292">Przy użyciu języka JavaScript za pomocą widoków, token mogą być tworzone przy użyciu usługi z w widoku.</span><span class="sxs-lookup"><span data-stu-id="942c5-292">Using JavaScript with views, the token can be created using a service from within the view.</span></span> <span data-ttu-id="942c5-293">Wstrzykiwanie [Microsoft.AspNetCore.Antiforgery.IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) usługi do wyświetlać i wywoływać [GetAndStoreTokens](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery.getandstoretokens):</span><span class="sxs-lookup"><span data-stu-id="942c5-293">Inject the [Microsoft.AspNetCore.Antiforgery.IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) service into the view and call [GetAndStoreTokens](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery.getandstoretokens):</span></span>

[!code-csharp[](anti-request-forgery/sample/MvcSample/Views/Home/Ajax.cshtml?highlight=4-10,12-13,35-36)]

<span data-ttu-id="942c5-294">To podejście eliminuje konieczność łączenia się bezpośrednio z ustawienia plików cookie z serwera lub ich odczytywania od klienta.</span><span class="sxs-lookup"><span data-stu-id="942c5-294">This approach eliminates the need to deal directly with setting cookies from the server or reading them from the client.</span></span>

<span data-ttu-id="942c5-295">Poprzedni przykład wykorzystuje JavaScript ma zostać odczytana wartość ukrytego pola nagłówka AJAX żądań POST.</span><span class="sxs-lookup"><span data-stu-id="942c5-295">The preceding example uses JavaScript to read the hidden field value for the AJAX POST header.</span></span>

<span data-ttu-id="942c5-296">JavaScript można również dostęp do tokenów w plikach cookie i użyj zawartość pliku cookie, aby utworzyć nagłówek, wartością tokenu.</span><span class="sxs-lookup"><span data-stu-id="942c5-296">JavaScript can also access tokens in cookies and use the cookie's contents to create a header with the token's value.</span></span>

```csharp
context.Response.Cookies.Append("CSRF-TOKEN", tokens.RequestToken, 
    new Microsoft.AspNetCore.Http.CookieOptions { HttpOnly = false });
```

<span data-ttu-id="942c5-297">Zakładając, że skrypt żądań wysyłania tokenu w zawiera nagłówek o nazwie `X-CSRF-TOKEN`, skonfigurować usługę antiforgery do wyszukania `X-CSRF-TOKEN` nagłówka:</span><span class="sxs-lookup"><span data-stu-id="942c5-297">Assuming the script requests to send the token in a header called `X-CSRF-TOKEN`, configure the antiforgery service to look for the `X-CSRF-TOKEN` header:</span></span>

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-CSRF-TOKEN");
```

<span data-ttu-id="942c5-298">W poniższym przykładzie użyto języka JavaScript, aby utworzyć żądanie AJAX przy użyciu odpowiedniego nagłówka:</span><span class="sxs-lookup"><span data-stu-id="942c5-298">The following example uses JavaScript to make an AJAX request with the appropriate header:</span></span>

```javascript
function getCookie(cname) {
    var name = cname + "=";
    var decodedCookie = decodeURIComponent(document.cookie);
    var ca = decodedCookie.split(';');
    for(var i = 0; i <ca.length; i++) {
        var c = ca[i];
        while (c.charAt(0) == ' ') {
            c = c.substring(1);
        }
        if (c.indexOf(name) == 0) {
            return c.substring(name.length, c.length);
        }
    }
    return "";
}

var csrfToken = getCookie("CSRF-TOKEN");

var xhttp = new XMLHttpRequest();
xhttp.onreadystatechange = function() {
    if (xhttp.readyState == XMLHttpRequest.DONE) {
        if (xhttp.status == 200) {
            alert(xhttp.responseText);
        } else {
            alert('There was an error processing the AJAX request.');
        }
    }
};
xhttp.open('POST', '/api/password/changepassword', true);
xhttp.setRequestHeader("Content-type", "application/json");
xhttp.setRequestHeader("X-CSRF-TOKEN", csrfToken);
xhttp.send(JSON.stringify({ "newPassword": "ReallySecurePassword999$$$" }));
```

### <a name="angularjs"></a><span data-ttu-id="942c5-299">AngularJS</span><span class="sxs-lookup"><span data-stu-id="942c5-299">AngularJS</span></span>

<span data-ttu-id="942c5-300">Moduł AngularJS używa konwencji adres CSRF.</span><span class="sxs-lookup"><span data-stu-id="942c5-300">AngularJS uses a convention to address CSRF.</span></span> <span data-ttu-id="942c5-301">Jeśli serwer wysyła plik cookie o nazwie `XSRF-TOKEN`, AngularJS `$http` usługa dodaje wartość pliku cookie do nagłówka, gdy wysyła żądanie do serwera.</span><span class="sxs-lookup"><span data-stu-id="942c5-301">If the server sends a cookie with the name `XSRF-TOKEN`, the AngularJS `$http` service adds the cookie value to a header when it sends a request to the server.</span></span> <span data-ttu-id="942c5-302">Ten proces odbywa się automatycznie.</span><span class="sxs-lookup"><span data-stu-id="942c5-302">This process is automatic.</span></span> <span data-ttu-id="942c5-303">Nagłówek nie musi jawnie ustawiona w kliencie.</span><span class="sxs-lookup"><span data-stu-id="942c5-303">The header doesn't need to be set in the client explicitly.</span></span> <span data-ttu-id="942c5-304">Nazwa nagłówka jest `X-XSRF-TOKEN`.</span><span class="sxs-lookup"><span data-stu-id="942c5-304">The header name is `X-XSRF-TOKEN`.</span></span> <span data-ttu-id="942c5-305">Serwer powinna wykryć ten nagłówek i zweryfikować jego zawartość.</span><span class="sxs-lookup"><span data-stu-id="942c5-305">The server should detect this header and validate its contents.</span></span>

<span data-ttu-id="942c5-306">Dla interfejsu API platformy ASP.NET Core pracować z niniejszej Konwencji, w przypadku uruchamiania aplikacji:</span><span class="sxs-lookup"><span data-stu-id="942c5-306">For ASP.NET Core API to work with this convention in your application startup:</span></span>

* <span data-ttu-id="942c5-307">Skonfiguruj aplikację, aby dostarczyć token w pliku cookie o nazwie `XSRF-TOKEN`.</span><span class="sxs-lookup"><span data-stu-id="942c5-307">Configure your app to provide a token in a cookie called `XSRF-TOKEN`.</span></span>
* <span data-ttu-id="942c5-308">Konfigurowanie usługi antiforgery do wyszukania nagłówka o nazwie `X-XSRF-TOKEN`.</span><span class="sxs-lookup"><span data-stu-id="942c5-308">Configure the antiforgery service to look for a header named `X-XSRF-TOKEN`.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IAntiforgery antiforgery)
{
    app.Use(next => context =>
    {
        string path = context.Request.Path.Value;

        if (
            string.Equals(path, "/", StringComparison.OrdinalIgnoreCase) ||
            string.Equals(path, "/index.html", StringComparison.OrdinalIgnoreCase))
        {
            // The request token can be sent as a JavaScript-readable cookie, 
            // and Angular uses it by default.
            var tokens = antiforgery.GetAndStoreTokens(context);
            context.Response.Cookies.Append("XSRF-TOKEN", tokens.RequestToken, 
                new CookieOptions() { HttpOnly = false });
        }

        return next(context);
    });
}

public void ConfigureServices(IServiceCollection services)
{
    // Angular's default header name for sending the XSRF token.
    services.AddAntiforgery(options => options.HeaderName = "X-XSRF-TOKEN");
}
```

<span data-ttu-id="942c5-309">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="942c5-309">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="extend-antiforgery"></a><span data-ttu-id="942c5-310">Rozszerzanie antiforgery</span><span class="sxs-lookup"><span data-stu-id="942c5-310">Extend antiforgery</span></span>

<span data-ttu-id="942c5-311">[IAntiForgeryAdditionalDataProvider](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) typu umożliwia deweloperom rozszerzania działania systemu anti-CSRF przez obustronne konwertowanie w każdy token dodatkowe dane.</span><span class="sxs-lookup"><span data-stu-id="942c5-311">The [IAntiForgeryAdditionalDataProvider](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) type allows developers to extend the behavior of the anti-CSRF system by round-tripping additional data in each token.</span></span> <span data-ttu-id="942c5-312">[GetAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.getadditionaldata) metoda jest wywoływana za każdym razem generowania tokenu pola i wartość zwracana jest osadzony w generowanych token.</span><span class="sxs-lookup"><span data-stu-id="942c5-312">The [GetAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.getadditionaldata) method is called each time a field token is generated, and the return value is embedded within the generated token.</span></span> <span data-ttu-id="942c5-313">Implementujący może zwrócić sygnaturę czasową, identyfikatora jednorazowego lub dowolna inna wartość, a następnie wywołać [ValidateAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.validateadditionaldata) do sprawdzania poprawności danych podczas weryfikowania tokenu.</span><span class="sxs-lookup"><span data-stu-id="942c5-313">An implementer could return a timestamp, a nonce, or any other value and then call [ValidateAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.validateadditionaldata) to validate this data when the token is validated.</span></span> <span data-ttu-id="942c5-314">Nazwa użytkownika klienta już jest osadzony w generowanych tokenach, więc nie trzeba wpisywać te informacje.</span><span class="sxs-lookup"><span data-stu-id="942c5-314">The client's username is already embedded in the generated tokens, so there's no need to include this information.</span></span> <span data-ttu-id="942c5-315">Jeśli token zawiera dane dodatkowe, ale nie `IAntiForgeryAdditionalDataProvider` jest skonfigurowany, dane dodatkowe nie jest zweryfikowany.</span><span class="sxs-lookup"><span data-stu-id="942c5-315">If a token includes supplemental data but no `IAntiForgeryAdditionalDataProvider` is configured, the supplemental data isn't validated.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="942c5-316">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="942c5-316">Additional resources</span></span>

* <span data-ttu-id="942c5-317">[CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) na [Otwórz projekt zabezpieczeń aplikacji sieci Web](https://www.owasp.org/index.php/Main_Page) (OWASP).</span><span class="sxs-lookup"><span data-stu-id="942c5-317">[CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) on [Open Web Application Security Project](https://www.owasp.org/index.php/Main_Page) (OWASP).</span></span>
* <xref:host-and-deploy/web-farm>

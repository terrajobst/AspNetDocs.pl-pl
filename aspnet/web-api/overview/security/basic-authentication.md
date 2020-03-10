---
uid: web-api/overview/security/basic-authentication
title: Uwierzytelnianie podstawowe w interfejsie API sieci Web ASP.NET | Microsoft Docs
author: MikeWasson
description: Opisuje używanie uwierzytelniania podstawowego w interfejsie Web API ASP.NET.
ms.author: riande
ms.date: 10/02/2014
ms.assetid: 41423767-0021-47c3-9e53-0021b457c39f
msc.legacyurl: /web-api/overview/security/basic-authentication
msc.type: authoredcontent
ms.openlocfilehash: 1470bd4b5abd5199b9a5105973b053812d643351
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78555728"
---
# <a name="basic-authentication-in-aspnet-web-api"></a><span data-ttu-id="97e66-103">Uwierzytelnianie podstawowe w interfejsie API sieci Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="97e66-103">Basic Authentication in ASP.NET Web API</span></span>

<span data-ttu-id="97e66-104">według [Jan Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="97e66-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="97e66-105">Uwierzytelnianie podstawowe jest zdefiniowane w [dokumencie RFC 2617, uwierzytelnianie http: uwierzytelnianie Basic i szyfrowanie dostępu szyfrowanego](http://www.ietf.org/rfc/rfc2617.txt).</span><span class="sxs-lookup"><span data-stu-id="97e66-105">Basic authentication is defined in [RFC 2617, HTTP Authentication: Basic and Digest Access Authentication](http://www.ietf.org/rfc/rfc2617.txt).</span></span>

<span data-ttu-id="97e66-106">Wady</span><span class="sxs-lookup"><span data-stu-id="97e66-106">Disadvantages</span></span>

- <span data-ttu-id="97e66-107">Poświadczenia użytkownika są wysyłane w żądaniu.</span><span class="sxs-lookup"><span data-stu-id="97e66-107">User credentials are sent in the request.</span></span>
- <span data-ttu-id="97e66-108">Poświadczenia są wysyłane w postaci zwykłego tekstu.</span><span class="sxs-lookup"><span data-stu-id="97e66-108">Credentials are sent as plaintext.</span></span>
- <span data-ttu-id="97e66-109">Poświadczenia są wysyłane przy użyciu każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="97e66-109">Credentials are sent with every request.</span></span>
- <span data-ttu-id="97e66-110">Nie ma możliwości wylogowania, z wyjątkiem zakończenia sesji przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="97e66-110">No way to log out, except by ending the browser session.</span></span>
- <span data-ttu-id="97e66-111">Podatne na fałszerstwo żądania między lokacjami (CSRF); wymaga środków antyCSRFowych.</span><span class="sxs-lookup"><span data-stu-id="97e66-111">Vulnerable to cross-site request forgery (CSRF); requires anti-CSRF measures.</span></span>

<span data-ttu-id="97e66-112">Zalety</span><span class="sxs-lookup"><span data-stu-id="97e66-112">Advantages</span></span>

- <span data-ttu-id="97e66-113">Internet Standard.</span><span class="sxs-lookup"><span data-stu-id="97e66-113">Internet standard.</span></span>
- <span data-ttu-id="97e66-114">Obsługiwane przez wszystkie główne przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="97e66-114">Supported by all major browsers.</span></span>
- <span data-ttu-id="97e66-115">Stosunkowo prosty protokół.</span><span class="sxs-lookup"><span data-stu-id="97e66-115">Relatively simple protocol.</span></span>

<span data-ttu-id="97e66-116">Uwierzytelnianie podstawowe działa w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="97e66-116">Basic authentication works as follows:</span></span>

1. <span data-ttu-id="97e66-117">Jeśli żądanie wymaga uwierzytelnienia, serwer zwraca 401 (bez autoryzacji).</span><span class="sxs-lookup"><span data-stu-id="97e66-117">If a request requires authentication, the server returns 401 (Unauthorized).</span></span> <span data-ttu-id="97e66-118">Odpowiedź zawiera nagłówek WWW-Authenticate wskazujący, że serwer obsługuje uwierzytelnianie podstawowe.</span><span class="sxs-lookup"><span data-stu-id="97e66-118">The response includes a WWW-Authenticate header, indicating the server supports Basic authentication.</span></span>
2. <span data-ttu-id="97e66-119">Klient wysyła kolejne żądanie z poświadczeniami klienta w nagłówku autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="97e66-119">The client sends another request, with the client credentials in the Authorization header.</span></span> <span data-ttu-id="97e66-120">Poświadczenia są formatowane jako ciąg "Name: Password", kodowane algorytmem Base64.</span><span class="sxs-lookup"><span data-stu-id="97e66-120">The credentials are formatted as the string "name:password", base64-encoded.</span></span> <span data-ttu-id="97e66-121">Poświadczenia nie są szyfrowane.</span><span class="sxs-lookup"><span data-stu-id="97e66-121">The credentials are not encrypted.</span></span>

<span data-ttu-id="97e66-122">Uwierzytelnianie podstawowe jest wykonywane w kontekście "obszar".</span><span class="sxs-lookup"><span data-stu-id="97e66-122">Basic authentication is performed within the context of a "realm."</span></span> <span data-ttu-id="97e66-123">Serwer zawiera nazwę obszaru w nagłówku WWW-Authenticate.</span><span class="sxs-lookup"><span data-stu-id="97e66-123">The server includes the name of the realm in the WWW-Authenticate header.</span></span> <span data-ttu-id="97e66-124">Poświadczenia użytkownika są prawidłowe w tym obszarze.</span><span class="sxs-lookup"><span data-stu-id="97e66-124">The user's credentials are valid within that realm.</span></span> <span data-ttu-id="97e66-125">Dokładny zakres obszaru jest definiowany przez serwer.</span><span class="sxs-lookup"><span data-stu-id="97e66-125">The exact scope of a realm is defined by the server.</span></span> <span data-ttu-id="97e66-126">Na przykład można zdefiniować kilka obszarów, aby podzielić zasoby na partycje.</span><span class="sxs-lookup"><span data-stu-id="97e66-126">For example, you might define several realms in order to partition resources.</span></span>

![](basic-authentication/_static/image1.png)

<span data-ttu-id="97e66-127">Ponieważ poświadczenia są wysyłane jako nieszyfrowane, uwierzytelnianie podstawowe jest zabezpieczane tylko za pośrednictwem protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="97e66-127">Because the credentials are sent unencrypted, Basic authentication is only secure over HTTPS.</span></span> <span data-ttu-id="97e66-128">Zobacz [Praca z protokołem SSL w internetowym interfejsie API](working-with-ssl-in-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="97e66-128">See [Working with SSL in Web API](working-with-ssl-in-web-api.md).</span></span>

<span data-ttu-id="97e66-129">Uwierzytelnianie podstawowe jest również podatne na ataki CSRF.</span><span class="sxs-lookup"><span data-stu-id="97e66-129">Basic authentication is also vulnerable to CSRF attacks.</span></span> <span data-ttu-id="97e66-130">Po wprowadzeniu poświadczeń przez użytkownika przeglądarka automatycznie wysyła je na kolejne żądania do tej samej domeny, na czas trwania sesji.</span><span class="sxs-lookup"><span data-stu-id="97e66-130">After the user enters credentials, the browser automatically sends them on subsequent requests to the same domain, for the duration of the session.</span></span> <span data-ttu-id="97e66-131">Obejmuje to żądania AJAX.</span><span class="sxs-lookup"><span data-stu-id="97e66-131">This includes AJAX requests.</span></span> <span data-ttu-id="97e66-132">Zapoznaj się z artykułem [zapobieganie atakom CSRFym między witrynami](preventing-cross-site-request-forgery-csrf-attacks.md).</span><span class="sxs-lookup"><span data-stu-id="97e66-132">See [Preventing Cross-Site Request Forgery (CSRF) Attacks](preventing-cross-site-request-forgery-csrf-attacks.md).</span></span>

## <a name="basic-authentication-with-iis"></a><span data-ttu-id="97e66-133">Uwierzytelnianie podstawowe przy użyciu usług IIS</span><span class="sxs-lookup"><span data-stu-id="97e66-133">Basic Authentication with IIS</span></span>

<span data-ttu-id="97e66-134">Usługi IIS obsługują uwierzytelnianie podstawowe, ale istnieje zastrzeżenie: użytkownik jest uwierzytelniany przy użyciu poświadczeń systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="97e66-134">IIS supports Basic authentication, but there is a caveat: The user is authenticated against their Windows credentials.</span></span> <span data-ttu-id="97e66-135">Oznacza to, że użytkownik musi mieć konto w domenie serwera.</span><span class="sxs-lookup"><span data-stu-id="97e66-135">That means the user must have an account on the server's domain.</span></span> <span data-ttu-id="97e66-136">W przypadku publicznej witryny sieci Web zwykle jest wymagane uwierzytelnienie względem dostawcy członkostwa ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="97e66-136">For a public-facing web site, you typically want to authenticate against an ASP.NET membership provider.</span></span>

<span data-ttu-id="97e66-137">Aby włączyć uwierzytelnianie podstawowe przy użyciu usług IIS, ustaw tryb uwierzytelniania na "Windows" w pliku Web. config projektu ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="97e66-137">To enable Basic authentication using IIS, set the authentication mode to "Windows" in the Web.config of your ASP.NET project:</span></span>

[!code-xml[Main](basic-authentication/samples/sample1.xml)]

<span data-ttu-id="97e66-138">W tym trybie usługi IIS uwierzytelniają się przy użyciu poświadczeń systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="97e66-138">In this mode, IIS uses Windows credentials to authenticate.</span></span> <span data-ttu-id="97e66-139">Ponadto należy włączyć uwierzytelnianie podstawowe w usługach IIS.</span><span class="sxs-lookup"><span data-stu-id="97e66-139">In addition, you must enable Basic authentication in IIS.</span></span> <span data-ttu-id="97e66-140">W Menedżerze usług IIS przejdź do opcji Widok funkcji, wybierz pozycję Uwierzytelnianie i Włącz uwierzytelnianie podstawowe.</span><span class="sxs-lookup"><span data-stu-id="97e66-140">In IIS Manager, go to Features View, select Authentication, and enable Basic authentication.</span></span>

![](basic-authentication/_static/image2.png)

<span data-ttu-id="97e66-141">W projekcie interfejsu API sieci Web Dodaj atrybut `[Authorize]` dla wszystkich akcji kontrolera, które wymagają uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="97e66-141">In your Web API project, add the `[Authorize]` attribute for any controller actions that need authentication.</span></span>

<span data-ttu-id="97e66-142">Klient uwierzytelnia się za pomocą ustawienia nagłówka autoryzacji w żądaniu.</span><span class="sxs-lookup"><span data-stu-id="97e66-142">A client authenticates itself by setting the Authorization header in the request.</span></span> <span data-ttu-id="97e66-143">Klienci przeglądarki wykonują ten krok automatycznie.</span><span class="sxs-lookup"><span data-stu-id="97e66-143">Browser clients perform this step automatically.</span></span> <span data-ttu-id="97e66-144">Klienci niebędący przeglądarką muszą ustawić nagłówek.</span><span class="sxs-lookup"><span data-stu-id="97e66-144">Nonbrowser clients will need to set the header.</span></span>

## <a name="basic-authentication-with-custom-membership"></a><span data-ttu-id="97e66-145">Uwierzytelnianie podstawowe z członkostwem niestandardowym</span><span class="sxs-lookup"><span data-stu-id="97e66-145">Basic Authentication with Custom Membership</span></span>

<span data-ttu-id="97e66-146">Jak wspomniano, uwierzytelnianie podstawowe wbudowane w usługi IIS używa poświadczeń systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="97e66-146">As mentioned, the Basic Authentication built into IIS uses Windows credentials.</span></span> <span data-ttu-id="97e66-147">Oznacza to, że należy utworzyć konta dla użytkowników na serwerze hostingu.</span><span class="sxs-lookup"><span data-stu-id="97e66-147">That means you need to create accounts for your users on the hosting server.</span></span> <span data-ttu-id="97e66-148">Jednak w przypadku aplikacji internetowej konta użytkowników są zwykle przechowywane w zewnętrznej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="97e66-148">But for an internet application, user accounts are typically stored in an external database.</span></span>

<span data-ttu-id="97e66-149">Poniższy kod, jak moduł HTTP wykonujący uwierzytelnianie podstawowe.</span><span class="sxs-lookup"><span data-stu-id="97e66-149">The following code how an HTTP module that performs Basic Authentication.</span></span> <span data-ttu-id="97e66-150">Możesz łatwo podłączyć dostawcę członkostwa ASP.NET, zastępując metodę `CheckPassword`, która jest metodą fikcyjną w tym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="97e66-150">You can easily plug in an ASP.NET membership provider by replacing the `CheckPassword` method, which is a dummy method in this example.</span></span>

<span data-ttu-id="97e66-151">W przypadku interfejsu Web API 2 należy rozważyć zapisanie [filtru uwierzytelniania](authentication-filters.md) lub [oprogramowania pośredniczącego Owin](../../../aspnet/overview/owin-and-katana/index.md)zamiast modułu http.</span><span class="sxs-lookup"><span data-stu-id="97e66-151">In Web API 2, you should consider writing an [authentication filter](authentication-filters.md) or [OWIN middleware](../../../aspnet/overview/owin-and-katana/index.md), instead of an HTTP module.</span></span>

[!code-csharp[Main](basic-authentication/samples/sample2.cs)]

<span data-ttu-id="97e66-152">Aby włączyć moduł HTTP, Dodaj następujący plik do pliku Web. config w sekcji **System. WebServer** :</span><span class="sxs-lookup"><span data-stu-id="97e66-152">To enable the HTTP module, add the following to your web.config file in the **system.webServer** section:</span></span>

[!code-xml[Main](basic-authentication/samples/sample3.xml?highlight=4)]

<span data-ttu-id="97e66-153">Zastąp ciąg "YourAssemblyName" nazwą zestawu (bez rozszerzenia "dll").</span><span class="sxs-lookup"><span data-stu-id="97e66-153">Replace "YourAssemblyName" with the name of the assembly (not including the "dll" extension).</span></span>

<span data-ttu-id="97e66-154">Należy wyłączyć inne schematy uwierzytelniania, takie jak formularze lub uwierzytelnianie systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="97e66-154">You should disable other authentication schemes, such as Forms or Windows auth.</span></span>

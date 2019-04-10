---
uid: web-api/overview/security/basic-authentication
title: Uwierzytelnianie podstawowe w interfejsie Web API platformy ASP.NET | Dokumentacja firmy Microsoft
author: MikeWasson
description: W tym artykule opisano, w interfejsie API sieci Web platformy ASP.NET przy użyciu uwierzytelniania podstawowego.
ms.author: riande
ms.date: 10/02/2014
ms.assetid: 41423767-0021-47c3-9e53-0021b457c39f
msc.legacyurl: /web-api/overview/security/basic-authentication
msc.type: authoredcontent
ms.openlocfilehash: 1470bd4b5abd5199b9a5105973b053812d643351
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/09/2019
ms.locfileid: "59412557"
---
# <a name="basic-authentication-in-aspnet-web-api"></a><span data-ttu-id="f6ca0-103">Uwierzytelnianie podstawowe we wzorcu ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="f6ca0-103">Basic Authentication in ASP.NET Web API</span></span>

<span data-ttu-id="f6ca0-104">przez [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="f6ca0-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="f6ca0-105">Uwierzytelnianie podstawowe jest zdefiniowany w [RFC 2617, uwierzytelnianie HTTP: Podstawowe i szyfrowane uwierzytelniania dostępu do](http://www.ietf.org/rfc/rfc2617.txt).</span><span class="sxs-lookup"><span data-stu-id="f6ca0-105">Basic authentication is defined in [RFC 2617, HTTP Authentication: Basic and Digest Access Authentication](http://www.ietf.org/rfc/rfc2617.txt).</span></span>

<span data-ttu-id="f6ca0-106">Wady</span><span class="sxs-lookup"><span data-stu-id="f6ca0-106">Disadvantages</span></span>

- <span data-ttu-id="f6ca0-107">Poświadczenia użytkownika są wysyłane w żądaniu.</span><span class="sxs-lookup"><span data-stu-id="f6ca0-107">User credentials are sent in the request.</span></span>
- <span data-ttu-id="f6ca0-108">Poświadczenia są wysyłane jako zwykły tekst.</span><span class="sxs-lookup"><span data-stu-id="f6ca0-108">Credentials are sent as plaintext.</span></span>
- <span data-ttu-id="f6ca0-109">Poświadczenia są wysyłane z każdym żądaniem.</span><span class="sxs-lookup"><span data-stu-id="f6ca0-109">Credentials are sent with every request.</span></span>
- <span data-ttu-id="f6ca0-110">Sposób logowania, z wyjątkiem poprzez zakończenie sesji przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="f6ca0-110">No way to log out, except by ending the browser session.</span></span>
- <span data-ttu-id="f6ca0-111">Narażone na fałszerstwo żądania międzywitrynowego (CSRF); wymaga anti-CSRF miar bez ograniczeń.</span><span class="sxs-lookup"><span data-stu-id="f6ca0-111">Vulnerable to cross-site request forgery (CSRF); requires anti-CSRF measures.</span></span>

<span data-ttu-id="f6ca0-112">Zalety</span><span class="sxs-lookup"><span data-stu-id="f6ca0-112">Advantages</span></span>

- <span data-ttu-id="f6ca0-113">Standardy internetowe.</span><span class="sxs-lookup"><span data-stu-id="f6ca0-113">Internet standard.</span></span>
- <span data-ttu-id="f6ca0-114">Obsługiwane przez wszystkie popularne przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="f6ca0-114">Supported by all major browsers.</span></span>
- <span data-ttu-id="f6ca0-115">Protokół stosunkowo proste.</span><span class="sxs-lookup"><span data-stu-id="f6ca0-115">Relatively simple protocol.</span></span>

<span data-ttu-id="f6ca0-116">Uwierzytelnianie podstawowe działa w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="f6ca0-116">Basic authentication works as follows:</span></span>

1. <span data-ttu-id="f6ca0-117">Jeśli żądanie wymaga uwierzytelnienia, serwer zwraca wartość 401 (bez autoryzacji).</span><span class="sxs-lookup"><span data-stu-id="f6ca0-117">If a request requires authentication, the server returns 401 (Unauthorized).</span></span> <span data-ttu-id="f6ca0-118">Odpowiedź zawiera nagłówka WWW-Authenticate, wskazujący, że serwer obsługuje uwierzytelnianie podstawowe.</span><span class="sxs-lookup"><span data-stu-id="f6ca0-118">The response includes a WWW-Authenticate header, indicating the server supports Basic authentication.</span></span>
2. <span data-ttu-id="f6ca0-119">Klient wysyła kolejne żądanie przy użyciu poświadczeń klienta w nagłówku autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="f6ca0-119">The client sends another request, with the client credentials in the Authorization header.</span></span> <span data-ttu-id="f6ca0-120">Poświadczenia są sformatowane jako ciąg "Nazwa: hasło", algorytmem Base64.</span><span class="sxs-lookup"><span data-stu-id="f6ca0-120">The credentials are formatted as the string "name:password", base64-encoded.</span></span> <span data-ttu-id="f6ca0-121">Poświadczenia nie są szyfrowane.</span><span class="sxs-lookup"><span data-stu-id="f6ca0-121">The credentials are not encrypted.</span></span>

<span data-ttu-id="f6ca0-122">Uwierzytelnianie podstawowe jest wykonywane w kontekście parametru "realm."</span><span class="sxs-lookup"><span data-stu-id="f6ca0-122">Basic authentication is performed within the context of a "realm."</span></span> <span data-ttu-id="f6ca0-123">Serwer zawiera nazwę obszaru nagłówka WWW-Authenticate.</span><span class="sxs-lookup"><span data-stu-id="f6ca0-123">The server includes the name of the realm in the WWW-Authenticate header.</span></span> <span data-ttu-id="f6ca0-124">Poświadczenia użytkownika są prawidłowe w ramach tego obszaru.</span><span class="sxs-lookup"><span data-stu-id="f6ca0-124">The user's credentials are valid within that realm.</span></span> <span data-ttu-id="f6ca0-125">Dokładny zakres obszaru jest definiowany przez serwer.</span><span class="sxs-lookup"><span data-stu-id="f6ca0-125">The exact scope of a realm is defined by the server.</span></span> <span data-ttu-id="f6ca0-126">Na przykład można zdefiniować kilka obszarów w kolejności do partycji zasobów.</span><span class="sxs-lookup"><span data-stu-id="f6ca0-126">For example, you might define several realms in order to partition resources.</span></span>

![](basic-authentication/_static/image1.png)

<span data-ttu-id="f6ca0-127">Ponieważ poświadczenia są wysyłane niezaszyfrowane, uwierzytelnianie podstawowe jest tylko bezpieczne za pośrednictwem protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="f6ca0-127">Because the credentials are sent unencrypted, Basic authentication is only secure over HTTPS.</span></span> <span data-ttu-id="f6ca0-128">Zobacz [Praca z protokołem SSL w składniku Web API](working-with-ssl-in-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="f6ca0-128">See [Working with SSL in Web API](working-with-ssl-in-web-api.md).</span></span>

<span data-ttu-id="f6ca0-129">Uwierzytelnianie podstawowe jest również narażone na ataki CSRF.</span><span class="sxs-lookup"><span data-stu-id="f6ca0-129">Basic authentication is also vulnerable to CSRF attacks.</span></span> <span data-ttu-id="f6ca0-130">Po użytkownik wprowadza poświadczenia, przeglądarka automatycznie wysyła je dla kolejnych żądań do tej samej domeny, na czas trwania sesji.</span><span class="sxs-lookup"><span data-stu-id="f6ca0-130">After the user enters credentials, the browser automatically sends them on subsequent requests to the same domain, for the duration of the session.</span></span> <span data-ttu-id="f6ca0-131">W tym wysyłanie żądań AJAX.</span><span class="sxs-lookup"><span data-stu-id="f6ca0-131">This includes AJAX requests.</span></span> <span data-ttu-id="f6ca0-132">Zobacz [zapobieganie atakom na fałszerstwo żądania Międzywitrynowego (CSRF)](preventing-cross-site-request-forgery-csrf-attacks.md).</span><span class="sxs-lookup"><span data-stu-id="f6ca0-132">See [Preventing Cross-Site Request Forgery (CSRF) Attacks](preventing-cross-site-request-forgery-csrf-attacks.md).</span></span>

## <a name="basic-authentication-with-iis"></a><span data-ttu-id="f6ca0-133">Uwierzytelnianie podstawowe w usługach IIS</span><span class="sxs-lookup"><span data-stu-id="f6ca0-133">Basic Authentication with IIS</span></span>

<span data-ttu-id="f6ca0-134">Usługi IIS obsługuje uwierzytelnianie podstawowe, ale istnieje Ostrzeżenie: Użytkownik jest uwierzytelniany względem poświadczeń Windows.</span><span class="sxs-lookup"><span data-stu-id="f6ca0-134">IIS supports Basic authentication, but there is a caveat: The user is authenticated against their Windows credentials.</span></span> <span data-ttu-id="f6ca0-135">Oznacza to, że użytkownik musi mieć konto w domenie serwera.</span><span class="sxs-lookup"><span data-stu-id="f6ca0-135">That means the user must have an account on the server's domain.</span></span> <span data-ttu-id="f6ca0-136">Dla witryny sieci web publicznego zazwyczaj chcesz uwierzytelniać dostawcy członkostwa platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f6ca0-136">For a public-facing web site, you typically want to authenticate against an ASP.NET membership provider.</span></span>

<span data-ttu-id="f6ca0-137">Aby włączyć uwierzytelnianie podstawowe, za pomocą usług IIS, ustaw tryb uwierzytelniania "Windows" w pliku Web.config projektu programu ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="f6ca0-137">To enable Basic authentication using IIS, set the authentication mode to "Windows" in the Web.config of your ASP.NET project:</span></span>

[!code-xml[Main](basic-authentication/samples/sample1.xml)]

<span data-ttu-id="f6ca0-138">W tym trybie usług IIS przy użyciu poświadczeń Windows do uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="f6ca0-138">In this mode, IIS uses Windows credentials to authenticate.</span></span> <span data-ttu-id="f6ca0-139">Ponadto należy włączyć uwierzytelnianie podstawowe w usługach IIS.</span><span class="sxs-lookup"><span data-stu-id="f6ca0-139">In addition, you must enable Basic authentication in IIS.</span></span> <span data-ttu-id="f6ca0-140">W Menedżerze usług IIS, przejdź do widoku funkcji, wybierz opcję Uwierzytelnianie i włączyć uwierzytelnianie podstawowe.</span><span class="sxs-lookup"><span data-stu-id="f6ca0-140">In IIS Manager, go to Features View, select Authentication, and enable Basic authentication.</span></span>

![](basic-authentication/_static/image2.png)

<span data-ttu-id="f6ca0-141">W projekcie interfejsu API sieci Web Dodaj `[Authorize]` atrybutu dla wszystkich działań kontrolera, które wymagają uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="f6ca0-141">In your Web API project, add the `[Authorize]` attribute for any controller actions that need authentication.</span></span>

<span data-ttu-id="f6ca0-142">Klient uwierzytelnia, ustawiając nagłówek autoryzacji w żądaniu.</span><span class="sxs-lookup"><span data-stu-id="f6ca0-142">A client authenticates itself by setting the Authorization header in the request.</span></span> <span data-ttu-id="f6ca0-143">Klientów w przeglądarkach, które automatycznie wykonać ten krok.</span><span class="sxs-lookup"><span data-stu-id="f6ca0-143">Browser clients perform this step automatically.</span></span> <span data-ttu-id="f6ca0-144">Klienci nonbrowser należy ustawić nagłówek.</span><span class="sxs-lookup"><span data-stu-id="f6ca0-144">Nonbrowser clients will need to set the header.</span></span>

## <a name="basic-authentication-with-custom-membership"></a><span data-ttu-id="f6ca0-145">Uwierzytelnianie podstawowe z członkostwem niestandardowe</span><span class="sxs-lookup"><span data-stu-id="f6ca0-145">Basic Authentication with Custom Membership</span></span>

<span data-ttu-id="f6ca0-146">Jak wspomniano wcześniej, uwierzytelnianie podstawowe, wbudowanych usług IIS przy użyciu poświadczeń Windows.</span><span class="sxs-lookup"><span data-stu-id="f6ca0-146">As mentioned, the Basic Authentication built into IIS uses Windows credentials.</span></span> <span data-ttu-id="f6ca0-147">Oznacza to, że należy utworzyć konta dla użytkowników na serwerze hostingu.</span><span class="sxs-lookup"><span data-stu-id="f6ca0-147">That means you need to create accounts for your users on the hosting server.</span></span> <span data-ttu-id="f6ca0-148">Jednak dla aplikacji internetowych, konta użytkowników są zazwyczaj przechowywane w zewnętrznej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="f6ca0-148">But for an internet application, user accounts are typically stored in an external database.</span></span>

<span data-ttu-id="f6ca0-149">Poniższy kod, w jaki sposób moduł HTTP, który wykonuje uwierzytelnianie podstawowe.</span><span class="sxs-lookup"><span data-stu-id="f6ca0-149">The following code how an HTTP module that performs Basic Authentication.</span></span> <span data-ttu-id="f6ca0-150">Możesz łatwo podłączyć dostawcy członkostwa platformy ASP.NET, zastępując `CheckPassword` metody, która jest metodą fikcyjnego z rolą w tym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="f6ca0-150">You can easily plug in an ASP.NET membership provider by replacing the `CheckPassword` method, which is a dummy method in this example.</span></span>

<span data-ttu-id="f6ca0-151">W sieci Web API 2, należy rozważyć pisania [filtr uwierzytelniania](authentication-filters.md) lub [oprogramowania pośredniczącego OWIN](../../../aspnet/overview/owin-and-katana/index.md), zamiast moduł protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="f6ca0-151">In Web API 2, you should consider writing an [authentication filter](authentication-filters.md) or [OWIN middleware](../../../aspnet/overview/owin-and-katana/index.md), instead of an HTTP module.</span></span>

[!code-csharp[Main](basic-authentication/samples/sample2.cs)]

<span data-ttu-id="f6ca0-152">Aby włączyć moduł HTTP, Dodaj następujący element do pliku web.config w **system.webServer** sekcji:</span><span class="sxs-lookup"><span data-stu-id="f6ca0-152">To enable the HTTP module, add the following to your web.config file in the **system.webServer** section:</span></span>

[!code-xml[Main](basic-authentication/samples/sample3.xml?highlight=4)]

<span data-ttu-id="f6ca0-153">Zastąp "YourAssemblyName" Nazwa zestawu (z wykluczeniem rozszerzenie "dll").</span><span class="sxs-lookup"><span data-stu-id="f6ca0-153">Replace "YourAssemblyName" with the name of the assembly (not including the "dll" extension).</span></span>

<span data-ttu-id="f6ca0-154">Należy wyłączyć inne schematy uwierzytelniania, takich jak uwierzytelniania formularzy lub Windows</span><span class="sxs-lookup"><span data-stu-id="f6ca0-154">You should disable other authentication schemes, such as Forms or Windows auth.</span></span>

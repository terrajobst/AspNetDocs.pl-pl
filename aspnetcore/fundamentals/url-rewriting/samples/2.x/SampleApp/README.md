---
ms.openlocfilehash: 4a4ad30846d1993b3e561c96c00fbf9a0c7f3929
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57070880"
---
# <a name="aspnet-core-url-rewriting-sample-aspnet-core-2x"></a><span data-ttu-id="f0afd-101">Adresu URL platformy ASP.NET Core ponowne napisanie przykładowe (ASP.NET Core 2.x)</span><span class="sxs-lookup"><span data-stu-id="f0afd-101">ASP.NET Core URL Rewriting Sample (ASP.NET Core 2.x)</span></span>

<span data-ttu-id="f0afd-102">W tym przykładzie pokazano sposób użycia platformy ASP.NET Core 2.x oprogramowanie pośredniczące ponownego zapisywania adresów URL.</span><span class="sxs-lookup"><span data-stu-id="f0afd-102">This sample illustrates usage of ASP.NET Core 2.x URL Rewriting Middleware.</span></span> <span data-ttu-id="f0afd-103">Aplikacja pokazuje adres URL przekierowania i adres URL ponowne napisanie opcje.</span><span class="sxs-lookup"><span data-stu-id="f0afd-103">The app demonstrates URL redirect and URL rewriting options.</span></span>

<span data-ttu-id="f0afd-104">Podczas uruchamiania przykładu, innego niż plik odpowiedzi zwraca nowych lub przekierowanego adresu URL po zastosowaniu jednej reguły do adresu URL żądania.</span><span class="sxs-lookup"><span data-stu-id="f0afd-104">When running the sample, non-file responses return the rewritten or redirected URL when one of the rules is applied to a request URL.</span></span> <span data-ttu-id="f0afd-105">Aby uzyskać przykłady plików XML i tekst oprogramowanie pośredniczące plików statycznych służy pliku po adresie URL żądania jest przepisany przez oprogramowanie pośredniczące.</span><span class="sxs-lookup"><span data-stu-id="f0afd-105">For the XML and text file examples, Static File Middleware serves the file after the request URL is rewritten by the middleware.</span></span>

## <a name="examples-in-this-sample"></a><span data-ttu-id="f0afd-106">Przykłady w tym przykładzie</span><span class="sxs-lookup"><span data-stu-id="f0afd-106">Examples in this sample</span></span>

* `AddRedirect("redirect-rule/(.*)", "redirected/$1")`
  - <span data-ttu-id="f0afd-107">Kod stanu powodzenia: 302 (Found)</span><span class="sxs-lookup"><span data-stu-id="f0afd-107">Success status code: 302 (Found)</span></span>
  - <span data-ttu-id="f0afd-108">Przykład (przekierowanie): **/redirect-rule / {capture_group}** do **/redirected/ {capture_group}**</span><span class="sxs-lookup"><span data-stu-id="f0afd-108">Example (redirect): **/redirect-rule/{capture_group}** to **/redirected/{capture_group}**</span></span>
* `AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", skipRemainingRules: true)`
  - <span data-ttu-id="f0afd-109">Kod stanu powodzenia: 200 (OK)</span><span class="sxs-lookup"><span data-stu-id="f0afd-109">Success status code: 200 (OK)</span></span>
  - <span data-ttu-id="f0afd-110">Przykład (Edycja): **/rewrite-rule / {capture_group_1} / {capture_group_2}** do **/ przepisany? var1 = {capture_group_1} & var2 = {capture_group_2}**</span><span class="sxs-lookup"><span data-stu-id="f0afd-110">Example (rewrite): **/rewrite-rule/{capture_group_1}/{capture_group_2}** to **/rewritten?var1={capture_group_1}&var2={capture_group_2}**</span></span>
* `AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt")`
  - <span data-ttu-id="f0afd-111">Kod stanu powodzenia: 302 (Found)</span><span class="sxs-lookup"><span data-stu-id="f0afd-111">Success status code: 302 (Found)</span></span>
  - <span data-ttu-id="f0afd-112">Przykład (przekierowanie): **/apache-mod-rules-redirect / {capture_group}** do **/ przekierowanie? id = {capture_group}**</span><span class="sxs-lookup"><span data-stu-id="f0afd-112">Example (redirect): **/apache-mod-rules-redirect/{capture_group}** to **/redirected?id={capture_group}**</span></span>
* `AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml")`
  - <span data-ttu-id="f0afd-113">Kod stanu powodzenia: 200 (OK)</span><span class="sxs-lookup"><span data-stu-id="f0afd-113">Success status code: 200 (OK)</span></span>
  - <span data-ttu-id="f0afd-114">Przykład (Edycja): **/iis-rules-rewrite / {capture_group}** do **/ przepisany? id = {capture_group}**</span><span class="sxs-lookup"><span data-stu-id="f0afd-114">Example (rewrite): **/iis-rules-rewrite/{capture_group}** to **/rewritten?id={capture_group}**</span></span>
* `Add(RedirectXmlFileRequests)`
  - <span data-ttu-id="f0afd-115">Kod stanu powodzenia: 301 (trwale przeniesiona)</span><span class="sxs-lookup"><span data-stu-id="f0afd-115">Success status code: 301 (Moved Permanently)</span></span>
  - <span data-ttu-id="f0afd-116">Przykład (przekierowanie): **/file.xml** do **/xmlfiles/file.xml**</span><span class="sxs-lookup"><span data-stu-id="f0afd-116">Example (redirect): **/file.xml** to **/xmlfiles/file.xml**</span></span>
* `Add(RewriteTextFileRequests)`
  - <span data-ttu-id="f0afd-117">Kod stanu powodzenia: 200 (OK)</span><span class="sxs-lookup"><span data-stu-id="f0afd-117">Success status code: 200 (OK)</span></span>
  - <span data-ttu-id="f0afd-118">Przykład (Edycja): **/some_file.txt** do **/file.txt**</span><span class="sxs-lookup"><span data-stu-id="f0afd-118">Example (rewrite): **/some_file.txt** to **/file.txt**</span></span>
* `Add(new RedirectImageRequests(".png", "/png-images")))`<br>`Add(new RedirectImageRequests(".jpg", "/jpg-images")))`
  - <span data-ttu-id="f0afd-119">Kod stanu powodzenia: 301 (trwale przeniesiona)</span><span class="sxs-lookup"><span data-stu-id="f0afd-119">Success status code: 301 (Moved Permanently)</span></span>
  - <span data-ttu-id="f0afd-120">Przykład (przekierowanie): **/image.png** do **/png-images/image.png**</span><span class="sxs-lookup"><span data-stu-id="f0afd-120">Example (redirect): **/image.png** to **/png-images/image.png**</span></span>
  - <span data-ttu-id="f0afd-121">Przykład (przekierowanie): **/image.jpg** do **/jpg-images/image.jpg**</span><span class="sxs-lookup"><span data-stu-id="f0afd-121">Example (redirect): **/image.jpg** to **/jpg-images/image.jpg**</span></span>

## <a name="use-a-physicalfileprovider"></a><span data-ttu-id="f0afd-122">Użyj PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="f0afd-122">Use a PhysicalFileProvider</span></span>

<span data-ttu-id="f0afd-123">Możesz również uzyskać `IFileProvider` , tworząc `PhysicalFileProvider` do przekazania do `AddApacheModRewrite()` i `AddIISUrlRewrite()` metody:</span><span class="sxs-lookup"><span data-stu-id="f0afd-123">You can also obtain an `IFileProvider` by creating a `PhysicalFileProvider` to pass into the `AddApacheModRewrite()` and `AddIISUrlRewrite()` methods:</span></span>

```csharp
using Microsoft.Extensions.FileProviders;
PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
```

## <a name="secure-redirection-extensions"></a><span data-ttu-id="f0afd-124">Zabezpieczanie rozszerzenia przekierowania</span><span class="sxs-lookup"><span data-stu-id="f0afd-124">Secure redirection extensions</span></span>

<span data-ttu-id="f0afd-125">Ten przykład obejmuje `WebHostBuilder` konfiguracji dla aplikacji, które chcesz użyć adresów URL (`https://localhost:5001`, `https://localhost`) i certyfikat testowy (*testCert.pfx*) ułatwiają Eksplorowanie metod bezpiecznego przekierowania.</span><span class="sxs-lookup"><span data-stu-id="f0afd-125">This sample includes `WebHostBuilder` configuration for the app to use URLs (`https://localhost:5001`, `https://localhost`) and a test certificate (*testCert.pfx*) to assist in exploring the secure redirect methods.</span></span> <span data-ttu-id="f0afd-126">Jeśli serwer ma już portu 443 przypisane lub jest w użyciu, `https://localhost` przykład nie zadziała&mdash;Usuń `ListenOptions` dla portu 443 w `CreateWebHostBuilder` metody *Program.cs* pliku lub Usuń powiązanie portu 443 na serwer, tak aby usługa Kestrel można użyć portu.</span><span class="sxs-lookup"><span data-stu-id="f0afd-126">If the server already has port 443 assigned or in use, the `https://localhost` example doesn't work&mdash;remove the `ListenOptions` for port 443 in the `CreateWebHostBuilder` method of the *Program.cs* file or unbind port 443 on the server so that Kestrel can use the port.</span></span>

| <span data-ttu-id="f0afd-127">Metoda</span><span class="sxs-lookup"><span data-stu-id="f0afd-127">Method</span></span>                           | <span data-ttu-id="f0afd-128">Kod stanu:</span><span class="sxs-lookup"><span data-stu-id="f0afd-128">Status Code</span></span> |    <span data-ttu-id="f0afd-129">Port</span><span class="sxs-lookup"><span data-stu-id="f0afd-129">Port</span></span>    |
| -------------------------------- | :---------: | :--------: |
| `.AddRedirectToHttpsPermanent()` |     <span data-ttu-id="f0afd-130">301</span><span class="sxs-lookup"><span data-stu-id="f0afd-130">301</span></span>     | <span data-ttu-id="f0afd-131">null (465)</span><span class="sxs-lookup"><span data-stu-id="f0afd-131">null (465)</span></span> |
| `.AddRedirectToHttps()`          |     <span data-ttu-id="f0afd-132">302</span><span class="sxs-lookup"><span data-stu-id="f0afd-132">302</span></span>     | <span data-ttu-id="f0afd-133">null (465)</span><span class="sxs-lookup"><span data-stu-id="f0afd-133">null (465)</span></span> |
| `.AddRedirectToHttps(301)`       |     <span data-ttu-id="f0afd-134">301</span><span class="sxs-lookup"><span data-stu-id="f0afd-134">301</span></span>     | <span data-ttu-id="f0afd-135">null (465)</span><span class="sxs-lookup"><span data-stu-id="f0afd-135">null (465)</span></span> |
| `.AddRedirectToHttps(301, 5001)` |     <span data-ttu-id="f0afd-136">301</span><span class="sxs-lookup"><span data-stu-id="f0afd-136">301</span></span>     |    <span data-ttu-id="f0afd-137">5001</span><span class="sxs-lookup"><span data-stu-id="f0afd-137">5001</span></span>    |

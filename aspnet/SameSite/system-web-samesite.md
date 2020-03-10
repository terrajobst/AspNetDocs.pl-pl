---
title: Pracuj z plikami cookie SameSite w ASP.NET
author: rick-anderson
description: Dowiedz się, jak używać programu do SameSite plików cookie w ASP.NET
ms.author: riande
ms.date: 2/15/2019
uid: samesite/system-web-samesite
ms.openlocfilehash: 7987a5d6c9b3a82679d42a2d381d471d56f495c2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78546747"
---
# <a name="work-with-samesite-cookies-in-aspnet"></a><span data-ttu-id="eedfa-103">Pracuj z plikami cookie SameSite w ASP.NET</span><span class="sxs-lookup"><span data-stu-id="eedfa-103">Work with SameSite cookies in ASP.NET</span></span>

<span data-ttu-id="eedfa-104">Autor: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="eedfa-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="eedfa-105">SameSite jest projektem standardowym [IETF](https://ietf.org/about/) opracowanym w celu zapewnienia pewnej ochrony przed atakami polegającymi na translokacjach (CSRF).</span><span class="sxs-lookup"><span data-stu-id="eedfa-105">SameSite is an [IETF](https://ietf.org/about/) draft standard designed to provide some protection against cross-site request forgery (CSRF) attacks.</span></span> <span data-ttu-id="eedfa-106">Pierwotnie Sporządzono w [2016](https://tools.ietf.org/html/draft-west-first-party-cookies-07), projekt Standard został zaktualizowany w [2019](https://tools.ietf.org/html/draft-west-cookie-incrementalism-00).</span><span class="sxs-lookup"><span data-stu-id="eedfa-106">Originally drafted in [2016](https://tools.ietf.org/html/draft-west-first-party-cookies-07), the draft standard was updated in [2019](https://tools.ietf.org/html/draft-west-cookie-incrementalism-00).</span></span> <span data-ttu-id="eedfa-107">Zaktualizowany standard nie jest zgodny z poprzednią wersją standardową, co poniżej przedstawiono najbardziej zauważalne różnice:</span><span class="sxs-lookup"><span data-stu-id="eedfa-107">The updated standard is not backward compatible with the previous standard, with the following being the most noticeable differences:</span></span>

* <span data-ttu-id="eedfa-108">Pliki cookie bez nagłówka SameSite są domyślnie traktowane jako `SameSite=Lax`.</span><span class="sxs-lookup"><span data-stu-id="eedfa-108">Cookies without SameSite header are treated as `SameSite=Lax` by default.</span></span>
* <span data-ttu-id="eedfa-109">`SameSite=None` należy użyć, aby zezwolić na użycie plików cookie między lokacjami.</span><span class="sxs-lookup"><span data-stu-id="eedfa-109">`SameSite=None` must be used to allow cross-site cookie use.</span></span>
* <span data-ttu-id="eedfa-110">Pliki cookie, które potwierdzają `SameSite=None` muszą być również oznaczone jako `Secure`.</span><span class="sxs-lookup"><span data-stu-id="eedfa-110">Cookies that assert `SameSite=None` must also be marked as `Secure`.</span></span>
* <span data-ttu-id="eedfa-111">Aplikacje korzystające z [`<iframe>`](https://developer.mozilla.org/docs/Web/HTML/Element/iframe) mogą napotkać problemy `sameSite=Lax` lub `sameSite=Strict` plików cookie, ponieważ `<iframe>` jest traktowana jako scenariusze między lokacjami.</span><span class="sxs-lookup"><span data-stu-id="eedfa-111">Applications that use [`<iframe>`](https://developer.mozilla.org/docs/Web/HTML/Element/iframe) may experience issues with `sameSite=Lax` or `sameSite=Strict` cookies because `<iframe>` is treated as cross-site scenarios.</span></span>
* <span data-ttu-id="eedfa-112">Wartość `SameSite=None` nie jest dozwolona przez [standard 2016](https://tools.ietf.org/html/draft-west-first-party-cookies-07) i powoduje, że niektóre implementacje traktują takie pliki cookie jako `SameSite=Strict`.</span><span class="sxs-lookup"><span data-stu-id="eedfa-112">The value `SameSite=None` is not allowed by the [2016 standard](https://tools.ietf.org/html/draft-west-first-party-cookies-07) and causes some implementations to treat such cookies as `SameSite=Strict`.</span></span> <span data-ttu-id="eedfa-113">Zobacz [Obsługa starszych przeglądarek](#sob) w tym dokumencie.</span><span class="sxs-lookup"><span data-stu-id="eedfa-113">See [Supporting older browsers](#sob) in this document.</span></span>

<span data-ttu-id="eedfa-114">Ustawienie `SameSite=Lax` działa w przypadku większości plików cookie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="eedfa-114">The `SameSite=Lax` setting works for most application cookies.</span></span> <span data-ttu-id="eedfa-115">Niektóre formy uwierzytelniania, takie jak [OpenID Connect Connect](https://openid.net/connect/) (OIDC) i [WS-Federation](https://auth0.com/docs/protocols/ws-fed) , domyślnie mogą publikować przekierowania na podstawie.</span><span class="sxs-lookup"><span data-stu-id="eedfa-115">Some forms of authentication like [OpenID Connect](https://openid.net/connect/) (OIDC) and [WS-Federation](https://auth0.com/docs/protocols/ws-fed) default to POST based redirects.</span></span> <span data-ttu-id="eedfa-116">Przekierowania na podstawie wpisu wyzwalają ochronę za pomocą przeglądarki SameSite, więc SameSite jest wyłączona dla tych składników.</span><span class="sxs-lookup"><span data-stu-id="eedfa-116">The POST based redirects trigger the SameSite browser protections, so SameSite is disabled for these components.</span></span> <span data-ttu-id="eedfa-117">Nie ma to wpływ na większość nazw logowania [OAuth](https://oauth.net/) z powodu różnic w sposobie przepływu żądania.</span><span class="sxs-lookup"><span data-stu-id="eedfa-117">Most [OAuth](https://oauth.net/) logins are not affected due to differences in how the request flows.</span></span>

<span data-ttu-id="eedfa-118">Każdy składnik ASP.NET, który emituje pliki cookie, musi zdecydować, czy SameSite jest odpowiedni.</span><span class="sxs-lookup"><span data-stu-id="eedfa-118">Each ASP.NET component that emits cookies needs to decide if SameSite is appropriate.</span></span>

<span data-ttu-id="eedfa-119">Zobacz [znane problemy](#known) dotyczące problemów z aplikacjami po 2019 zainstalowaniu aktualizacji SameSite .NET.</span><span class="sxs-lookup"><span data-stu-id="eedfa-119">See [Known Issues](#known) for problems with applications after installing the 2019 .Net SameSite updates.</span></span>

## <a name="using-samesite-in-aspnet-472-and-48"></a><span data-ttu-id="eedfa-120">Używanie SameSite w ASP.NET 4.7.2 i 4,8</span><span class="sxs-lookup"><span data-stu-id="eedfa-120">Using SameSite in ASP.NET 4.7.2 and 4.8</span></span>

<span data-ttu-id="eedfa-121">Program .NET 4.7.2 i 4,8 obsługuje [wersję standard 2019](https://tools.ietf.org/html/draft-west-cookie-incrementalism-00) dla SameSite od czasu wydania aktualizacji w grudniu 2019.</span><span class="sxs-lookup"><span data-stu-id="eedfa-121">.Net 4.7.2 and 4.8 supports the [2019 draft standard](https://tools.ietf.org/html/draft-west-cookie-incrementalism-00) for SameSite since the release of updates in December 2019.</span></span> <span data-ttu-id="eedfa-122">Deweloperzy mogą programowo sterować wartością nagłówka SameSite przy użyciu [Właściwości HttpCookie. SameSite](/dotnet/api/system.web.httpcookie.samesite#System_Web_HttpCookie_SameSite).</span><span class="sxs-lookup"><span data-stu-id="eedfa-122">Developers are able to programmatically control the value of the SameSite header using the [HttpCookie.SameSite property](/dotnet/api/system.web.httpcookie.samesite#System_Web_HttpCookie_SameSite).</span></span> <span data-ttu-id="eedfa-123">Ustawienie właściwości `SameSite` na `Strict`, `Lax`lub `None` powoduje, że te wartości są zapisywane w sieci przy użyciu pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="eedfa-123">Setting the `SameSite` property to `Strict`, `Lax`, or `None` results in those values being written on the network with the cookie.</span></span> <span data-ttu-id="eedfa-124">Ustawienie równe `(SameSiteMode)(-1)` wskazuje, że w sieci nie powinien znajdować się nagłówek SameSite.</span><span class="sxs-lookup"><span data-stu-id="eedfa-124">Setting it equal to `(SameSiteMode)(-1)` indicates that no SameSite header should be included on the network with the cookie.</span></span> <span data-ttu-id="eedfa-125">[Właściwość HttpCookie. Secure](/dotnet/api/system.web.httpcookie.secure)lub "RequireSSL" w plikach konfiguracji może służyć do oznaczania pliku cookie jako `Secure` lub nie.</span><span class="sxs-lookup"><span data-stu-id="eedfa-125">The [HttpCookie.Secure Property](/dotnet/api/system.web.httpcookie.secure), or 'requireSSL' in config files, can be used to mark the cookie as `Secure` or not.</span></span>

<span data-ttu-id="eedfa-126">Nowe wystąpienia `HttpCookie` będą domyślnie `SameSite=(SameSiteMode)(-1)` i `Secure=false`.</span><span class="sxs-lookup"><span data-stu-id="eedfa-126">New `HttpCookie` instances will default to `SameSite=(SameSiteMode)(-1)` and `Secure=false`.</span></span> <span data-ttu-id="eedfa-127">Te wartości domyślne można przesłonić w sekcji konfiguracji `system.web/httpCookies`, gdzie `"Unspecified"` ciąg jest przyjazną składnią tylko do konfiguracji dla `(SameSiteMode)(-1)`:</span><span class="sxs-lookup"><span data-stu-id="eedfa-127">These defaults can be overridden in the `system.web/httpCookies` configuration section, where the string `"Unspecified"` is a friendly configuration-only syntax for `(SameSiteMode)(-1)`:</span></span>

```xml
<configuration>
 <system.web>
  <httpCookies sameSite="[Strict|Lax|None|Unspecified]" requireSSL="[true|false]" />
 <system.web>
<configuration>
```

<span data-ttu-id="eedfa-128">Program ASP.Net również zawiera cztery określone pliki cookie dla tych funkcji: uwierzytelnianie anonimowe, uwierzytelnianie formularzy, stan sesji i zarządzanie rolami.</span><span class="sxs-lookup"><span data-stu-id="eedfa-128">ASP.Net also issues four specific cookies of its own for these features: Anonymous Authentication, Forms Authentication, Session State, and Role Management.</span></span> <span data-ttu-id="eedfa-129">Wystąpienia tych plików cookie uzyskanych w środowisku uruchomieniowym można manipulować przy użyciu właściwości `SameSite` i `Secure`, podobnie jak każde inne wystąpienie HttpCookie.</span><span class="sxs-lookup"><span data-stu-id="eedfa-129">Instances of these cookies obtained in runtime can be manipulated using the `SameSite` and `Secure` properties just like any other HttpCookie instance.</span></span> <span data-ttu-id="eedfa-130">Jednak ze względu na zbieraninyą w standardzie SameSite opcje konfiguracji tych czterech plików cookie są niespójne.</span><span class="sxs-lookup"><span data-stu-id="eedfa-130">However, due to the patchwork emergence of the SameSite standard, configuration options for these four features cookies is inconsistent.</span></span> <span data-ttu-id="eedfa-131">Poniżej przedstawiono odpowiednie sekcje i atrybuty konfiguracji z wartościami domyślnymi.</span><span class="sxs-lookup"><span data-stu-id="eedfa-131">The relevant configuration sections and attributes, with defaults, are shown below.</span></span> <span data-ttu-id="eedfa-132">Jeśli dla funkcji nie ma `SameSite` ani `Secure` powiązany atrybut, funkcja zostanie przywrócona zgodnie z ustawieniami domyślnymi skonfigurowanymi w sekcji `system.web/httpCookies` opisanej powyżej.</span><span class="sxs-lookup"><span data-stu-id="eedfa-132">If there is no `SameSite` or `Secure` related attribute for a feature, then the feature will fall back on the defaults configured in the `system.web/httpCookies` section discussed above.</span></span>

```xml
<configuration>
 <system.web>
  <anonymousIdentification cookieRequireSSL="false" /> <!-- No config attribute for SameSite -->
  <authentication>
   <forms cookieSameSite="Lax" requireSSL="false" />
  </authentication>
  <sessionState cookieSameSite="Lax" /> <!-- No config attribute for Secure -->
  <roleManager cookieRequireSSL="false" /> <!-- No config attribute for SameSite -->
 <system.web>
<configuration>
```  

<span data-ttu-id="eedfa-133">**Uwaga**: element "unonly" jest dostępny tylko do `system.web/httpCookies@sameSite` w tym momencie.</span><span class="sxs-lookup"><span data-stu-id="eedfa-133">**Note**: 'Unspecified' is only available to `system.web/httpCookies@sameSite` at the moment.</span></span> <span data-ttu-id="eedfa-134">Mamy nadzieję, że dodamy podobną składnię do wcześniej pokazanych atrybutów cookieSameSite w przyszłych aktualizacjach.</span><span class="sxs-lookup"><span data-stu-id="eedfa-134">We hope to add similar syntax to the previously shown cookieSameSite attributes in future updates.</span></span> <span data-ttu-id="eedfa-135">Ustawienie `(SameSiteMode)(-1)` w kodzie nadal działa w wystąpieniach tych plików cookie. \*</span><span class="sxs-lookup"><span data-stu-id="eedfa-135">Setting `(SameSiteMode)(-1)` in code still works on instances of these cookies.\*</span></span>

[!INCLUDE[](~/includes/MTcomments.md)]

<a name="retargeting"></a>

### <a name="retarget-net-apps"></a><span data-ttu-id="eedfa-136">Przekieruj aplikacje .NET</span><span class="sxs-lookup"><span data-stu-id="eedfa-136">Retarget .NET apps</span></span>

<span data-ttu-id="eedfa-137">Aby określić element docelowy .NET 4.7.2 lub nowszy:</span><span class="sxs-lookup"><span data-stu-id="eedfa-137">To target .NET 4.7.2 or later:</span></span>

* <span data-ttu-id="eedfa-138">Upewnij się, że *plik Web. config* zawiera następujące elementy:</span><span class="sxs-lookup"><span data-stu-id="eedfa-138">Ensure *web.config* contains the following:</span></span>  <!-- review, I removed `debug="true"` -->

  ```xml
  <system.web>
    <compilation targetFramework="4.7.2"/>
    <httpRuntime targetFramework="4.7.2"/>
  </system.web>

* Verify the project file contains the correct [TargetFrameworkVersion](/visualstudio/msbuild/msbuild-target-framework-and-target-platform?view=vs-2019):

  ```xml
  <TargetFrameworkVersion>v4.7.2</TargetFrameworkVersion>
  ```

  <span data-ttu-id="eedfa-139">[Przewodnik migracji platformy .NET](/dotnet/framework/migration-guide/) zawiera więcej szczegółów.</span><span class="sxs-lookup"><span data-stu-id="eedfa-139">The [.NET Migration Guide](/dotnet/framework/migration-guide/) has more details.</span></span>

* <span data-ttu-id="eedfa-140">Sprawdź, czy pakiety NuGet w projekcie są zgodne z prawidłową wersją platformy.</span><span class="sxs-lookup"><span data-stu-id="eedfa-140">Verify NuGet packages in the project are targeted at the correct framework version.</span></span> <span data-ttu-id="eedfa-141">Aby sprawdzić poprawność wersji platformy, zbadaj plik *Packages. config* , na przykład:</span><span class="sxs-lookup"><span data-stu-id="eedfa-141">You can verify the correct framework version by examining the *packages.config* file, for example:</span></span>

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <packages>
    <package id="Microsoft.AspNet.Mvc" version="5.2.7" targetFramework="net472" />
    <package id="Microsoft.ApplicationInsights" version="2.4.0" targetFramework="net451" />
  </packages>
  ```

  <span data-ttu-id="eedfa-142">W poprzednim pliku *Packages. config* pakiet `Microsoft.ApplicationInsights`:</span><span class="sxs-lookup"><span data-stu-id="eedfa-142">In the preceding *packages.config* file, the `Microsoft.ApplicationInsights` package:</span></span>
    * <span data-ttu-id="eedfa-143">Jest przeznaczony dla programu .NET 4.5.1.</span><span class="sxs-lookup"><span data-stu-id="eedfa-143">Is  targeted against .NET 4.5.1.</span></span>
    * <span data-ttu-id="eedfa-144">Powinien mieć zaktualizowany atrybut `targetFramework`, aby `net472`, jeśli istnieje zaktualizowany pakiet przeznaczony dla obiektu docelowego struktury.</span><span class="sxs-lookup"><span data-stu-id="eedfa-144">Should have its `targetFramework` attribute updated to `net472` if an updated package targeting your framework target exists.</span></span>

<a name="nope"></a>

### <a name="net-versions-earlier-than-472"></a><span data-ttu-id="eedfa-145">.NET w wersji wcześniejszej niż 4.7.2</span><span class="sxs-lookup"><span data-stu-id="eedfa-145">.NET versions earlier than 4.7.2</span></span>

<span data-ttu-id="eedfa-146">Firma Microsoft nie obsługuje programu .NET w wersji niższej niż 4.7.2 do zapisania atrybutu cookie tego samego pliku.</span><span class="sxs-lookup"><span data-stu-id="eedfa-146">Microsoft does not support .NET versions lower that 4.7.2 for writing the same-site cookie attribute.</span></span> <span data-ttu-id="eedfa-147">Nie znaleźliśmy niezawodnej metody:</span><span class="sxs-lookup"><span data-stu-id="eedfa-147">We have not found a reliable way to:</span></span>

* <span data-ttu-id="eedfa-148">Upewnij się, że atrybut jest poprawnie zapisany na podstawie wersji przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="eedfa-148">Ensure the attribute is written correctly based on browser version.</span></span>
* <span data-ttu-id="eedfa-149">Przechwycenie i dostosowanie plików cookie dotyczących uwierzytelniania i sesji w starszych wersjach platformy.</span><span class="sxs-lookup"><span data-stu-id="eedfa-149">Intercept and adjust authentication and session cookies on older framework versions.</span></span>

### <a name="december-patch-behavior-changes"></a><span data-ttu-id="eedfa-150">Zmiany zachowania poprawek w grudniu</span><span class="sxs-lookup"><span data-stu-id="eedfa-150">December patch behavior changes</span></span>

<span data-ttu-id="eedfa-151">W przypadku .NET Framework Właściwość `SameSite` interpretuje wartość `None`:</span><span class="sxs-lookup"><span data-stu-id="eedfa-151">The specific behavior change for .NET Framework is how the `SameSite` property interprets the `None` value:</span></span>

* <span data-ttu-id="eedfa-152">Przed poprawką wartość `None` to:</span><span class="sxs-lookup"><span data-stu-id="eedfa-152">Before the patch a value of `None` meant:</span></span>
  * <span data-ttu-id="eedfa-153">Nie emituj atrybutu wcale.</span><span class="sxs-lookup"><span data-stu-id="eedfa-153">Do not emit the attribute at all.</span></span>
* <span data-ttu-id="eedfa-154">Po zastosowaniu poprawki:</span><span class="sxs-lookup"><span data-stu-id="eedfa-154">After the patch:</span></span>
  * <span data-ttu-id="eedfa-155">Wartość `None`oznacza "Emituj atrybut z wartością `None`".</span><span class="sxs-lookup"><span data-stu-id="eedfa-155">A value of `None`it means "Emit the attribute with a value of `None`".</span></span>
  * <span data-ttu-id="eedfa-156">`SameSite` wartość `(SameSiteMode)(-1)` powoduje, że atrybut nie jest emitowany.</span><span class="sxs-lookup"><span data-stu-id="eedfa-156">A `SameSite` value of `(SameSiteMode)(-1)` causes the attribute not to be emitted.</span></span>

<span data-ttu-id="eedfa-157">Domyślna wartość SameSite dla plików cookie związanych z uwierzytelnianiem formularzy i stanem sesji została zmieniona z `None` na `Lax`.</span><span class="sxs-lookup"><span data-stu-id="eedfa-157">The default SameSite value for forms authentication and session state cookies was changed from `None` to `Lax`.</span></span>

### <a name="summary-of-change-impact-on-browsers"></a><span data-ttu-id="eedfa-158">Podsumowanie wpływu zmiany na przeglądarki</span><span class="sxs-lookup"><span data-stu-id="eedfa-158">Summary of change impact on browsers</span></span>

<span data-ttu-id="eedfa-159">Jeśli zainstalujesz poprawkę i wygenerujesz plik cookie z `SameSite.None`, zostanie wykorzystana jedna z dwóch sytuacji:</span><span class="sxs-lookup"><span data-stu-id="eedfa-159">If you install the patch and issue a cookie with `SameSite.None`, one of two things will happen:</span></span>
* <span data-ttu-id="eedfa-160">Program Chrome V80 traktuje ten plik cookie zgodnie z nową implementacją, a nie wymusić tych samych ograniczeń lokacji w pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="eedfa-160">Chrome v80 will treat this cookie according to the new implementation, and not enforce same site restrictions on the cookie.</span></span>
* <span data-ttu-id="eedfa-161">Każda przeglądarka, która nie została zaktualizowana w celu obsługi nowej implementacji, będzie zgodna ze starą implementacją.</span><span class="sxs-lookup"><span data-stu-id="eedfa-161">Any browser that has not been updated to support the new implementation will follow the old implementation.</span></span> <span data-ttu-id="eedfa-162">Stara implementacja brzmi:</span><span class="sxs-lookup"><span data-stu-id="eedfa-162">The old implementation says:</span></span>
  * <span data-ttu-id="eedfa-163">Jeśli zobaczysz niezrozumiałą wartość, zignoruj ją i Przełącz na rygorystyczne ograniczenia lokacji.</span><span class="sxs-lookup"><span data-stu-id="eedfa-163">If you see a value you don't understand, ignore it and switch to strict same site restrictions.</span></span>

<span data-ttu-id="eedfa-164">Aplikacja jest przerywana w przeglądarce Chrome lub może zostać przerwana w wielu innych miejscach.</span><span class="sxs-lookup"><span data-stu-id="eedfa-164">So either the app breaks in Chrome, or you break in numerous other places.</span></span>

## <a name="history-and-changes"></a><span data-ttu-id="eedfa-165">Historia i zmiany</span><span class="sxs-lookup"><span data-stu-id="eedfa-165">History and changes</span></span>

<span data-ttu-id="eedfa-166">Obsługa SameSite została najpierw zaimplementowana w programie .NET 4.7.2 przy użyciu [standardowego standardu 2016](https://tools.ietf.org/html/draft-west-first-party-cookies-07#section-4.1).</span><span class="sxs-lookup"><span data-stu-id="eedfa-166">SameSite support was first implemented in .NET 4.7.2 using the [2016 draft standard](https://tools.ietf.org/html/draft-west-first-party-cookies-07#section-4.1).</span></span>

<span data-ttu-id="eedfa-167">19 listopada 2019 aktualizacje dla systemu Windows Zaktualizowano .NET 4.7.2 + ze standardu 2016 do standardu 2019.</span><span class="sxs-lookup"><span data-stu-id="eedfa-167">The November 19, 2019 updates for Windows updated .NET 4.7.2+ from the 2016 standard to the 2019 standard.</span></span> <span data-ttu-id="eedfa-168">Dodatkowe aktualizacje są nachodzące dla innych wersji systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="eedfa-168">Additional updates are forthcoming for other versions of Windows.</span></span> <span data-ttu-id="eedfa-169">Aby uzyskać więcej informacji, zobacz temat <xref:samesite/kbs-samesite>.</span><span class="sxs-lookup"><span data-stu-id="eedfa-169">For more information, see <xref:samesite/kbs-samesite>.</span></span>

 <span data-ttu-id="eedfa-170">Wersja robocza 2019 specyfikacji SameSite:</span><span class="sxs-lookup"><span data-stu-id="eedfa-170">The 2019 draft of the SameSite specification:</span></span>

* <span data-ttu-id="eedfa-171">**Nie** jest wstecznie zgodne z wersją roboczą 2016.</span><span class="sxs-lookup"><span data-stu-id="eedfa-171">Is **not** backwards compatible with the 2016 draft.</span></span> <span data-ttu-id="eedfa-172">Aby uzyskać więcej informacji, zobacz [Obsługa starszych przeglądarek](#sob) w tym dokumencie.</span><span class="sxs-lookup"><span data-stu-id="eedfa-172">For more information, see [Supporting older browsers](#sob) in this document.</span></span>
* <span data-ttu-id="eedfa-173">Określa, że pliki cookie są domyślnie traktowane jako `SameSite=Lax`.</span><span class="sxs-lookup"><span data-stu-id="eedfa-173">Specifies cookies are treated as `SameSite=Lax` by default.</span></span>
* <span data-ttu-id="eedfa-174">Określa pliki cookie, które jawnie porzucają `SameSite=None` w celu włączenia dostarczania między lokacjami należy również oznaczyć jako `Secure`.</span><span class="sxs-lookup"><span data-stu-id="eedfa-174">Specifies cookies that explicitly assert `SameSite=None` in order to enable cross-site delivery should also be marked as `Secure`.</span></span>
* <span data-ttu-id="eedfa-175">Jest obsługiwane przez poprawki wydane zgodnie z opisem w powyższej bazie wiedzy.</span><span class="sxs-lookup"><span data-stu-id="eedfa-175">Is supported by patches issued as described in the KB's listed above.</span></span>
* <span data-ttu-id="eedfa-176">Zaplanowano włączenie programu [Chrome](https://chromestatus.com/feature/5088147346030592) domyślnie w [lutym 2020](https://blog.chromium.org/2019/10/developers-get-ready-for-new.html).</span><span class="sxs-lookup"><span data-stu-id="eedfa-176">Is scheduled to be enabled by [Chrome](https://chromestatus.com/feature/5088147346030592) by default in [Feb 2020](https://blog.chromium.org/2019/10/developers-get-ready-for-new.html).</span></span> <span data-ttu-id="eedfa-177">Przeglądarki zaczynają przechodzenie do tego standardu w 2019.</span><span class="sxs-lookup"><span data-stu-id="eedfa-177">Browsers started moving to this standard in 2019.</span></span>

<a name="known"><a/>

## <a name="known-issues"></a><span data-ttu-id="eedfa-178">Znane problemy</span><span class="sxs-lookup"><span data-stu-id="eedfa-178">Known Issues</span></span>

<span data-ttu-id="eedfa-179">Ze względu na to, że specyfikacje robocze 2016 i 2019 nie są zgodne, Aktualizacja programu .NET Framework w listopadzie 2019 wprowadza pewne zmiany, które mogą zostać zerwane.</span><span class="sxs-lookup"><span data-stu-id="eedfa-179">Because the 2016 and 2019 draft specifications are not compatible, the November 2019 .Net Framework update introduces some changes that may be breaking.</span></span>

* <span data-ttu-id="eedfa-180">Pliki cookie stanu sesji i uwierzytelniania formularzy są teraz zapisywane w sieci jako `Lax`, a nie nieokreślone.</span><span class="sxs-lookup"><span data-stu-id="eedfa-180">Session State and Forms Authentication cookies are now written to the network as `Lax` instead of unspecified.</span></span>
  * <span data-ttu-id="eedfa-181">Chociaż większość aplikacji pracuje z `SameSite=Lax` plikami cookie, aplikacje, które PUBLIKUJą w witrynach lub aplikacjach korzystających z `iframe` mogą stwierdzić, że ich stan sesji lub pliki cookie autoryzacji formularzy nie są używane zgodnie z oczekiwaniami.</span><span class="sxs-lookup"><span data-stu-id="eedfa-181">While most apps work with `SameSite=Lax` cookies, apps that POST across sites or applications that make use of `iframe` may find that their session state or forms authorization cookies aren't being used as expected.</span></span> <span data-ttu-id="eedfa-182">Aby rozwiązać ten konieczność, należy zmienić wartość `cookieSameSite` w odpowiedniej sekcji konfiguracji, jak opisano wcześniej.</span><span class="sxs-lookup"><span data-stu-id="eedfa-182">To remedy this, change the `cookieSameSite` value in the appropriate configuration section as discussed previously.</span></span>
* <span data-ttu-id="eedfa-183">HttpCookies, który jawnie ustawił `SameSite=None` w kodzie lub konfiguracji, ma teraz tę wartość, która została wcześniej pominięta.</span><span class="sxs-lookup"><span data-stu-id="eedfa-183">HttpCookies that explicitly set `SameSite=None` in code or configuration now have that value written with the cookie, whereas it was previously omitted.</span></span> <span data-ttu-id="eedfa-184">Może to powodować problemy ze starszymi przeglądarkami, które obsługują tylko wersję standardową 2016.</span><span class="sxs-lookup"><span data-stu-id="eedfa-184">This may cause issues with older browsers that only support the 2016 draft standard.</span></span>
  * <span data-ttu-id="eedfa-185">W przypadku przeglądarek obsługujących wersję standardową 2019 w programie `SameSite=None` pliki cookie Pamiętaj, aby oznaczyć je także `Secure` lub nie można ich rozpoznać.</span><span class="sxs-lookup"><span data-stu-id="eedfa-185">When targeting browsers supporting the 2019 draft standard with `SameSite=None` cookies, remember to also mark them `Secure` or they may not be recognized.</span></span>
  * <span data-ttu-id="eedfa-186">Aby przywrócić zachowanie 2016 niepisania `SameSite=None`, użyj ustawienia aplikacji `aspnet:SupressSameSiteNone=true`.</span><span class="sxs-lookup"><span data-stu-id="eedfa-186">To revert to the 2016 behavior of not writing `SameSite=None`, use the app setting `aspnet:SupressSameSiteNone=true`.</span></span> <span data-ttu-id="eedfa-187">Należy pamiętać, że dotyczy to wszystkich HttpCookies w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="eedfa-187">Note that this will apply to all HttpCookies in the app.</span></span>

### <a name="azure-app-servicesamesite-cookie-handling"></a><span data-ttu-id="eedfa-188">Azure App Service — obsługa plików cookie SameSite</span><span class="sxs-lookup"><span data-stu-id="eedfa-188">Azure App Service—SameSite cookie handling</span></span>

<span data-ttu-id="eedfa-189">Zobacz [Azure App Service — obsługa plików cookie SameSite i .NET Framework Poprawka 4.7.2,](https://azure.microsoft.com/updates/app-service-samesite-cookie-update/) Aby uzyskać informacje na temat Azure App Service sposobu konfigurowania zachowań SameSite w aplikacjach .NET 4.7.2.</span><span class="sxs-lookup"><span data-stu-id="eedfa-189">See [Azure App Service—SameSite cookie handling and .NET Framework 4.7.2 patch](https://azure.microsoft.com/updates/app-service-samesite-cookie-update/) for information about how Azure App Service is configuring SameSite behaviors in .Net 4.7.2 apps.</span></span>

<a name="sob"></a>

## <a name="supporting-older-browsers"></a><span data-ttu-id="eedfa-190">Obsługa starszych przeglądarek</span><span class="sxs-lookup"><span data-stu-id="eedfa-190">Supporting older browsers</span></span>

<span data-ttu-id="eedfa-191">Standard 2016 SameSite, że nieznane wartości muszą być traktowane jako wartości `SameSite=Strict`.</span><span class="sxs-lookup"><span data-stu-id="eedfa-191">The 2016 SameSite standard mandated that unknown values must be treated as `SameSite=Strict` values.</span></span> <span data-ttu-id="eedfa-192">Aplikacje dostępne ze starszych przeglądarek, które obsługują Standard 2016 SameSite, mogą ulec przerwie, gdy pobierają Właściwość SameSite o wartości `None`.</span><span class="sxs-lookup"><span data-stu-id="eedfa-192">Apps accessed from older browsers which support the 2016 SameSite standard may break when they get a SameSite property with a value of `None`.</span></span> <span data-ttu-id="eedfa-193">Aplikacje sieci Web muszą implementować wykrywanie przeglądarki, jeśli chcą obsługiwać starsze przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="eedfa-193">Web apps must implement browser detection if they intend to support older browsers.</span></span> <span data-ttu-id="eedfa-194">ASP.NET nie implementuje wykrywania przeglądarki, ponieważ wartości agentów użytkownika są bardzo nietrwałe i często zmieniają się.</span><span class="sxs-lookup"><span data-stu-id="eedfa-194">ASP.NET doesn't implement browser detection because User-Agents values are highly volatile and change frequently.</span></span>

<span data-ttu-id="eedfa-195">Podejście firmy Microsoft do rozwiązania problemu polega na zaimplementowaniu składników wykrywania przeglądarki w celu rozdzielenia atrybutu `sameSite=None` z plików cookie, jeśli jest on znany jako nieobsługiwany przez przeglądarkę.</span><span class="sxs-lookup"><span data-stu-id="eedfa-195">Microsoft's approach to fixing the problem is to help you implement browser detection components to strip the `sameSite=None` attribute from cookies if a browser is known to not support it.</span></span> <span data-ttu-id="eedfa-196">Firma Google otrzymała instrukcje dotyczące wydawania podwójnych plików cookie, jednego z nowym atrybutem i jednego bez atrybutu.</span><span class="sxs-lookup"><span data-stu-id="eedfa-196">Google's advice was to issue double cookies, one with the new attribute, and one without the attribute at all.</span></span> <span data-ttu-id="eedfa-197">Należy jednak wziąć pod uwagę ograniczoną opinię firmy Google.</span><span class="sxs-lookup"><span data-stu-id="eedfa-197">However we consider Google's advice limited.</span></span> <span data-ttu-id="eedfa-198">Niektóre przeglądarki, w szczególności przeglądarki mobilne, mają bardzo małe limity liczby plików cookie, które mogą być wysyłane przez lokację lub nazwę domeny.</span><span class="sxs-lookup"><span data-stu-id="eedfa-198">Some browsers, especially mobile browsers have very small limits on the number of cookies a site, or a domain name can send.</span></span> <span data-ttu-id="eedfa-199">Wysyłanie wielu plików cookie, szczególnie dużych plików cookie, takich jak pliki cookie uwierzytelniania, mogą szybko dotrzeć do przeglądarki mobilnej, co sprawia, że błędy aplikacji są trudne do zdiagnozowania i naprawienia.</span><span class="sxs-lookup"><span data-stu-id="eedfa-199">Sending multiple cookies, especially large cookies like authentication cookies can reach the mobile browser limit very quickly, causing app failures that are hard to diagnose and fix.</span></span> <span data-ttu-id="eedfa-200">Ponadto jako struktura istnieje duży ekosystem kodu i składników innych firm, które mogą nie zostać zaktualizowane, aby można było korzystać z metody podwójnego pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="eedfa-200">Furthermore as a framework there is a large ecosystem of third party code and components that may not be updated to use a double cookie approach.</span></span>

<span data-ttu-id="eedfa-201">Kod wykrywania przeglądarki używany w przykładowych projektach w [tym repozytorium GitHub]() znajduje się w dwóch plikach</span><span class="sxs-lookup"><span data-stu-id="eedfa-201">The browser detection code used in the sample projects in [this GitHub repository]() is contained in two files</span></span>

* [<span data-ttu-id="eedfa-202">C#SameSiteSupport.cs</span><span class="sxs-lookup"><span data-stu-id="eedfa-202">C# SameSiteSupport.cs</span></span>](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/SameSiteSupport.cs)
* [<span data-ttu-id="eedfa-203">VB SameSiteSupport. vb</span><span class="sxs-lookup"><span data-stu-id="eedfa-203">VB SameSiteSupport.vb</span></span>](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/SameSiteSupport.vb)

<span data-ttu-id="eedfa-204">Te wykrywania są najczęściej spotykanymi agentami przeglądarki, którzy obsługują Standard 2016 i dla których atrybut musi zostać całkowicie usunięty.</span><span class="sxs-lookup"><span data-stu-id="eedfa-204">These detections are the most common browser agents we have seen that support the 2016 standard and for which the attribute needs to be completely removed.</span></span> <span data-ttu-id="eedfa-205">Nie jest to kompletna implementacja:</span><span class="sxs-lookup"><span data-stu-id="eedfa-205">It isn't meant as a complete implementation:</span></span>

* <span data-ttu-id="eedfa-206">Twoja aplikacja może zobaczyć przeglądarki, których nie ma w naszej witrynie testowej.</span><span class="sxs-lookup"><span data-stu-id="eedfa-206">Your app may see browsers that our test sites do not.</span></span>
* <span data-ttu-id="eedfa-207">Należy przystąpić do dodawania wykrywania zależnie od potrzeb danego środowiska.</span><span class="sxs-lookup"><span data-stu-id="eedfa-207">You should be prepared to add detections as necessary for your environment.</span></span>

<span data-ttu-id="eedfa-208">Sposób działania wykrywania zależy od używanej wersji platformy .NET i platformy sieci Web.</span><span class="sxs-lookup"><span data-stu-id="eedfa-208">How you wire up the detection varies according the version of .NET and the web framework that you are using.</span></span> <span data-ttu-id="eedfa-209">Następujący kod można wywołać w witrynie wywołania [HttpCookie](/dotnet/api/system.web.httpcookie) :</span><span class="sxs-lookup"><span data-stu-id="eedfa-209">The following code can be called at the [HttpCookie](/dotnet/api/system.web.httpcookie) call site:</span></span>

[!code-csharp[](sample/SameSiteCheck.cs?name=snippet)]

<span data-ttu-id="eedfa-210">Zobacz następujące tematy dotyczące plików cookie ASP.NET 4.7.2 SameSite:</span><span class="sxs-lookup"><span data-stu-id="eedfa-210">See the following ASP.NET 4.7.2 SameSite cookie topics:</span></span>

* [<span data-ttu-id="eedfa-211">C#Standard</span><span class="sxs-lookup"><span data-stu-id="eedfa-211">C# MVC</span></span>](xref:samesite/csMVC)
* [<span data-ttu-id="eedfa-212">C#Formularzy WebForms</span><span class="sxs-lookup"><span data-stu-id="eedfa-212">C# WebForms</span></span>](xref:samesite/CSharpWebForms)
* [<span data-ttu-id="eedfa-213">Formularze sieci Web VB</span><span class="sxs-lookup"><span data-stu-id="eedfa-213">VB WebForms</span></span>](xref:samesite/vbWF)
* [<span data-ttu-id="eedfa-214">VB MVC</span><span class="sxs-lookup"><span data-stu-id="eedfa-214">VB MVC</span></span>](xref:samesite/vbMVC)
<!--
* <xref:samesite/csMVC>
* <xref:samesite/CSharpWebForms>
* <xref:samesite/vbWF>
* <xref:samesite/vbMVC>
-->

### <a name="ensuring-your-site-redirects-to-https"></a><span data-ttu-id="eedfa-215">Upewnienie się, że witryna przekierowuje do protokołu HTTPS</span><span class="sxs-lookup"><span data-stu-id="eedfa-215">Ensuring your site redirects to HTTPS</span></span>

<span data-ttu-id="eedfa-216">W przypadku ASP.NET 4. x, WebForms i MVC funkcja [ponownego zapisywania adresów URL programu IIS](/iis/extensions/url-rewrite-module/creating-rewrite-rules-for-the-url-rewrite-module) może służyć do przekierowywania wszystkich żądań do protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="eedfa-216">For ASP.NET 4.x, WebForms and MVC, [IIS's URL Rewrite](/iis/extensions/url-rewrite-module/creating-rewrite-rules-for-the-url-rewrite-module) feature can be used to redirect all requests to HTTPS.</span></span> <span data-ttu-id="eedfa-217">W poniższym kodzie XML przedstawiono przykładową regułę:</span><span class="sxs-lookup"><span data-stu-id="eedfa-217">The following XML shows a sample rule:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <system.webServer>
    <rewrite>
      <rules>
        <rule name="Redirect to https" stopProcessing="true">
          <match url="(.*)"/>
          <conditions>
            <add input="{HTTPS}" pattern="Off"/>
            <add input="{REQUEST_METHOD}" pattern="^get$|^head$" />
          </conditions>
          <action type="Redirect" url="https://{HTTP_HOST}/{R:1}" redirectType="Permanent"/>
        </rule>
      </rules>
    </rewrite>
  </system.webServer>
</configuration>
```

<span data-ttu-id="eedfa-218">W przypadku instalacji lokalnych [adresów URL usług IIS](https://www.iis.net/downloads/microsoft/url-rewrite) jest to opcjonalna funkcja, która może wymagać instalacji.</span><span class="sxs-lookup"><span data-stu-id="eedfa-218">In on-premises installations of [IIS URL Rewrite](https://www.iis.net/downloads/microsoft/url-rewrite) is an optional feature that may need installing.</span></span>

## <a name="test-apps-for-samesite-problems"></a><span data-ttu-id="eedfa-219">Testowanie aplikacji pod kątem problemów z SameSite</span><span class="sxs-lookup"><span data-stu-id="eedfa-219">Test apps for SameSite problems</span></span>

<span data-ttu-id="eedfa-220">Musisz przetestować swoją aplikację za pomocą obsługiwanych przeglądarek i przejść przez scenariusze, które obejmują pliki cookie.</span><span class="sxs-lookup"><span data-stu-id="eedfa-220">You must test your app with the browsers you support and go through your scenarios that involve cookies.</span></span> <span data-ttu-id="eedfa-221">Scenariusze plików cookie zwykle obejmują</span><span class="sxs-lookup"><span data-stu-id="eedfa-221">Cookie scenarios typically involve</span></span>

* <span data-ttu-id="eedfa-222">Formularze logowania</span><span class="sxs-lookup"><span data-stu-id="eedfa-222">Login forms</span></span>
* <span data-ttu-id="eedfa-223">Zewnętrzne mechanizmy logowania, takie jak Facebook, Azure AD, OAuth i OIDC</span><span class="sxs-lookup"><span data-stu-id="eedfa-223">External login mechanisms such as Facebook, Azure AD, OAuth and OIDC</span></span>
* <span data-ttu-id="eedfa-224">Strony akceptujące żądania z innych lokacji</span><span class="sxs-lookup"><span data-stu-id="eedfa-224">Pages that accept requests from other sites</span></span>
* <span data-ttu-id="eedfa-225">Strony w aplikacji zaprojektowane do osadzenia w elementach iframe</span><span class="sxs-lookup"><span data-stu-id="eedfa-225">Pages in your app designed to be embedded in iframes</span></span>

<span data-ttu-id="eedfa-226">Należy sprawdzić, czy pliki cookie są tworzone, utrwalane i usuwane prawidłowo w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="eedfa-226">You should check that cookies are created, persisted and deleted correctly in your app.</span></span>

<span data-ttu-id="eedfa-227">Aplikacje, które współdziałają z witrynami zdalnymi, takie jak logowanie innych firm, muszą:</span><span class="sxs-lookup"><span data-stu-id="eedfa-227">Apps that interact with remote sites such as through third-party login need to:</span></span>

* <span data-ttu-id="eedfa-228">Przetestuj interakcję w wielu przeglądarkach.</span><span class="sxs-lookup"><span data-stu-id="eedfa-228">Test the interaction on multiple browsers.</span></span>
* <span data-ttu-id="eedfa-229">Zastosuj [wykrywanie przeglądarki i środki zaradcze](#sob) omówione w tym dokumencie.</span><span class="sxs-lookup"><span data-stu-id="eedfa-229">Apply the [browser detection and mitigation](#sob) discussed in this document.</span></span>

<span data-ttu-id="eedfa-230">Przetestuj aplikacje sieci Web przy użyciu wersji klienta, która może być zgodą na nowe zachowanie SameSite.</span><span class="sxs-lookup"><span data-stu-id="eedfa-230">Test web apps using a client version that can opt-in to the new SameSite behavior.</span></span> <span data-ttu-id="eedfa-231">Przeglądarki Chrome, Firefox i chrom Edge mają nowe flagi funkcji opt, których można użyć do testowania.</span><span class="sxs-lookup"><span data-stu-id="eedfa-231">Chrome, Firefox, and Chromium Edge all have new opt-in feature flags that can be used for testing.</span></span> <span data-ttu-id="eedfa-232">Po zastosowaniu przez aplikację SameSite poprawek przetestuj ją ze starszymi wersjami klienta, szczególnie Safari.</span><span class="sxs-lookup"><span data-stu-id="eedfa-232">After your app applies the SameSite patches, test it with older client versions, especially Safari.</span></span> <span data-ttu-id="eedfa-233">Aby uzyskać więcej informacji, zobacz [Obsługa starszych przeglądarek](#sob) w tym dokumencie.</span><span class="sxs-lookup"><span data-stu-id="eedfa-233">For more information, see [Supporting older browsers](#sob) in this document.</span></span>

### <a name="test-with-chrome"></a><span data-ttu-id="eedfa-234">Testowanie za pomocą przeglądarki Chrome</span><span class="sxs-lookup"><span data-stu-id="eedfa-234">Test with Chrome</span></span>

<span data-ttu-id="eedfa-235">Chrome 78 + daje mylące wyniki, ponieważ ma tymczasowe środki zaradcze.</span><span class="sxs-lookup"><span data-stu-id="eedfa-235">Chrome 78+ gives misleading results because it has a temporary mitigation in place.</span></span> <span data-ttu-id="eedfa-236">Chrom 78 + tymczasowe środki zaradcze dopuszczają pliki cookie mniejsze niż dwie minuty.</span><span class="sxs-lookup"><span data-stu-id="eedfa-236">The Chrome 78+ temporary mitigation allows cookies less than two minutes old.</span></span> <span data-ttu-id="eedfa-237">Program Chrome 76 lub 77 z włączonymi odpowiednimi flagami testu zapewnia dokładniejsze wyniki.</span><span class="sxs-lookup"><span data-stu-id="eedfa-237">Chrome 76 or 77 with the appropriate test flags enabled provides more accurate results.</span></span> <span data-ttu-id="eedfa-238">Aby przetestować nowe zachowanie SameSite, przełącz `chrome://flags/#same-site-by-default-cookies` na **włączone**.</span><span class="sxs-lookup"><span data-stu-id="eedfa-238">To test the new SameSite behavior toggle `chrome://flags/#same-site-by-default-cookies` to **Enabled**.</span></span> <span data-ttu-id="eedfa-239">Starsze wersje programu Chrome (75 i poniżej) zostały zgłoszone w celu niepowodzenia z nowym ustawieniem `None`.</span><span class="sxs-lookup"><span data-stu-id="eedfa-239">Older versions of Chrome (75 and below) are reported to fail with the new `None` setting.</span></span> <span data-ttu-id="eedfa-240">Zobacz [Obsługa starszych przeglądarek](#sob) w tym dokumencie.</span><span class="sxs-lookup"><span data-stu-id="eedfa-240">See [Supporting older browsers](#sob) in this document.</span></span>

<span data-ttu-id="eedfa-241">Firma Google nie udostępnia starszych wersji programu Chrome.</span><span class="sxs-lookup"><span data-stu-id="eedfa-241">Google does not make older chrome versions available.</span></span> <span data-ttu-id="eedfa-242">Postępuj zgodnie z instrukcjami podanymi w części [pobieranie chromu](https://www.chromium.org/getting-involved/download-chromium) , aby przetestować starsze wersje programu Chrome.</span><span class="sxs-lookup"><span data-stu-id="eedfa-242">Follow the instructions at [Download Chromium](https://www.chromium.org/getting-involved/download-chromium) to test older versions of Chrome.</span></span> <span data-ttu-id="eedfa-243">**Nie** Pobieraj programu Chrome z linków dostarczonych przez wyszukiwanie starszych wersji programu Chrome.</span><span class="sxs-lookup"><span data-stu-id="eedfa-243">Do **not** download Chrome from links provided by searching for older versions of chrome.</span></span>

* [<span data-ttu-id="eedfa-244">Chrom 76 Win64</span><span class="sxs-lookup"><span data-stu-id="eedfa-244">Chromium 76 Win64</span></span>](https://commondatastorage.googleapis.com/chromium-browser-snapshots/index.html?prefix=Win_x64/664998/)
* [<span data-ttu-id="eedfa-245">Chrom 74 Win64</span><span class="sxs-lookup"><span data-stu-id="eedfa-245">Chromium 74 Win64</span></span>](https://commondatastorage.googleapis.com/chromium-browser-snapshots/index.html?prefix=Win_x64/638880/)
* <span data-ttu-id="eedfa-246">Jeśli nie używasz 64-bitowej wersji systemu Windows, możesz użyć [przeglądarki OmahaProxy](https://omahaproxy.appspot.com/) do wyszukania, która gałąź chromu odnosi się do programu Chrome 74 (v 74.0.3729.108), korzystając z [instrukcji dostarczonych przez chrom](https://www.chromium.org/getting-involved/download-chromium).</span><span class="sxs-lookup"><span data-stu-id="eedfa-246">If you're not using a 64bit version of Windows you can use the [OmahaProxy viewer](https://omahaproxy.appspot.com/) to look up which Chromium branch corresponds to Chrome 74 (v74.0.3729.108) using the [instructions provided by Chromium](https://www.chromium.org/getting-involved/download-chromium).</span></span>

<span data-ttu-id="eedfa-247">Począwszy od wersji `80.0.3975.0`Kanaryjskich, można wyłączyć swobodny + POST tymczasowych środków zaradczych na potrzeby testowania przy użyciu nowej flagi `--enable-features=SameSiteDefaultChecksMethodRigorously`, aby umożliwić Testowanie witryn i usług w stanie końcowym funkcji, w której zostało usunięte.</span><span class="sxs-lookup"><span data-stu-id="eedfa-247">Starting in Canary version `80.0.3975.0`, the Lax+POST temporary mitigation can be disabled for testing purposes using the new flag `--enable-features=SameSiteDefaultChecksMethodRigorously` to allow testing of sites and services in the eventual end state of the feature where the mitigation has been removed.</span></span> <span data-ttu-id="eedfa-248">Aby uzyskać więcej informacji, zobacz artykuł dotyczący projektów chrom [SameSite Updates](https://www.chromium.org/updates/same-site)</span><span class="sxs-lookup"><span data-stu-id="eedfa-248">For more information, see The Chromium Projects [SameSite Updates](https://www.chromium.org/updates/same-site)</span></span>

#### <a name="test-with-chrome-80"></a><span data-ttu-id="eedfa-249">Testowanie przy użyciu przeglądarki Chrome 80 +</span><span class="sxs-lookup"><span data-stu-id="eedfa-249">Test with Chrome 80+</span></span>

<span data-ttu-id="eedfa-250">[Pobierz](https://www.google.com/chrome/) wersję programu Chrome, która obsługuje nowy atrybut.</span><span class="sxs-lookup"><span data-stu-id="eedfa-250">[Download](https://www.google.com/chrome/) a version of Chrome that supports their new attribute.</span></span> <span data-ttu-id="eedfa-251">W chwili pisania bieżąca wersja to Chrome 80.</span><span class="sxs-lookup"><span data-stu-id="eedfa-251">At the time of writing, the current version is Chrome 80.</span></span> <span data-ttu-id="eedfa-252">Program Chrome 80 wymaga, aby flaga `chrome://flags/#same-site-by-default-cookies` mogła korzystać z nowego zachowania.</span><span class="sxs-lookup"><span data-stu-id="eedfa-252">Chrome 80 needs the flag `chrome://flags/#same-site-by-default-cookies` enabled to use the new behavior.</span></span> <span data-ttu-id="eedfa-253">Należy również włączyć (`chrome://flags/#cookies-without-same-site-must-be-secure`), aby przetestować nadchodzące zachowanie plików cookie, dla których nie włączono atrybutu sameSite.</span><span class="sxs-lookup"><span data-stu-id="eedfa-253">You should also enable (`chrome://flags/#cookies-without-same-site-must-be-secure`) to test the upcoming behavior for cookies which have no sameSite attribute enabled.</span></span> <span data-ttu-id="eedfa-254">Program Chrome 80 jest włączony, aby przełączyć przełącznik do traktowania plików cookie bez atrybutu jako `SameSite=Lax`, aczkolwiek z upływem czasu prolongaty dla określonych żądań.</span><span class="sxs-lookup"><span data-stu-id="eedfa-254">Chrome 80 is on target to make the switch to treat cookies without the attribute as `SameSite=Lax`, albeit with a timed grace period for certain requests.</span></span> <span data-ttu-id="eedfa-255">Aby wyłączyć okres prolongaty dla programu Chrome 80, można uruchomić przy użyciu następującego argumentu wiersza polecenia:</span><span class="sxs-lookup"><span data-stu-id="eedfa-255">To disable the timed grace period Chrome 80 can be launched with the following command line argument:</span></span>

`--enable-features=SameSiteDefaultChecksMethodRigorously`

<span data-ttu-id="eedfa-256">Chrome 80 zawiera komunikaty ostrzegawcze w konsoli przeglądarki dotyczące brakujących atrybutów sameSite.</span><span class="sxs-lookup"><span data-stu-id="eedfa-256">Chrome 80 has warning messages in the browser console about missing sameSite attributes.</span></span> <span data-ttu-id="eedfa-257">Użyj klawisza F12, aby otworzyć konsolę przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="eedfa-257">Use F12 to open the browser console.</span></span>

### <a name="test-with-safari"></a><span data-ttu-id="eedfa-258">Testowanie przy użyciu przeglądarki Safari</span><span class="sxs-lookup"><span data-stu-id="eedfa-258">Test with Safari</span></span>

<span data-ttu-id="eedfa-259">Przeglądarka Safari 12 ściśle wdrożyła poprzednią wersję roboczą i kończy się niepowodzeniem, gdy nowa wartość `None` jest w pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="eedfa-259">Safari 12 strictly implemented the prior draft and fails when the new `None` value is in a cookie.</span></span> <span data-ttu-id="eedfa-260">`None` jest możliwe za pośrednictwem kodu wykrywania przeglądarki [obsługującego starsze przeglądarki](#sob) w tym dokumencie.</span><span class="sxs-lookup"><span data-stu-id="eedfa-260">`None` is avoided via the browser detection code [Supporting older browsers](#sob) in this document.</span></span> <span data-ttu-id="eedfa-261">Przetestuj logowanie za pomocą przeglądarki Safari 12, Safari 13 i WebKit opartej na programie MSAL, ADAL lub niezależnie od używanej biblioteki.</span><span class="sxs-lookup"><span data-stu-id="eedfa-261">Test Safari 12, Safari 13, and WebKit based OS style logins using MSAL, ADAL or whatever library you are using.</span></span> <span data-ttu-id="eedfa-262">Problem jest zależny od podstawowej wersji systemu operacyjnego.</span><span class="sxs-lookup"><span data-stu-id="eedfa-262">The problem is dependent on the underlying OS version.</span></span> <span data-ttu-id="eedfa-263">OSX Mojave (10,14) i iOS 12 są znane jako problemy ze zgodnością z nowym zachowaniem SameSite.</span><span class="sxs-lookup"><span data-stu-id="eedfa-263">OSX Mojave (10.14) and iOS 12 are known to have compatibility problems with the new SameSite behavior.</span></span> <span data-ttu-id="eedfa-264">Uaktualnienie systemu operacyjnego do wersji OSX Catalina (10,15) lub iOS 13 rozwiązuje problem.</span><span class="sxs-lookup"><span data-stu-id="eedfa-264">Upgrading the OS to OSX Catalina (10.15) or iOS 13 fixes the problem.</span></span> <span data-ttu-id="eedfa-265">Przeglądarka Safari nie ma obecnie flagi zgody na testowanie nowych zachowań specyfikacji.</span><span class="sxs-lookup"><span data-stu-id="eedfa-265">Safari does not currently have an opt-in flag for testing the new spec behavior.</span></span>

### <a name="test-with-firefox"></a><span data-ttu-id="eedfa-266">Testowanie za pomocą przeglądarki Firefox</span><span class="sxs-lookup"><span data-stu-id="eedfa-266">Test with Firefox</span></span>

<span data-ttu-id="eedfa-267">Obsługę programu Firefox dla nowego standardu można przetestować w wersji 68 + przez wypróbowanie na stronie `about:config` z flagą funkcji `network.cookie.sameSite.laxByDefault`.</span><span class="sxs-lookup"><span data-stu-id="eedfa-267">Firefox support for the new standard can be tested on version 68+ by opting in on the `about:config` page with the feature flag `network.cookie.sameSite.laxByDefault`.</span></span> <span data-ttu-id="eedfa-268">Nie zgłoszono problemów ze zgodnością ze starszymi wersjami programu Firefox.</span><span class="sxs-lookup"><span data-stu-id="eedfa-268">There haven't been reports of compatibility issues with older versions of Firefox.</span></span>

### <a name="test-with-edge-legacy-browser"></a><span data-ttu-id="eedfa-269">Testowanie przy użyciu przeglądarki Edge (starsza wersja)</span><span class="sxs-lookup"><span data-stu-id="eedfa-269">Test with Edge (Legacy) browser</span></span>

<span data-ttu-id="eedfa-270">Program Edge obsługuje stary Standard SameSite.</span><span class="sxs-lookup"><span data-stu-id="eedfa-270">Edge supports the old SameSite standard.</span></span> <span data-ttu-id="eedfa-271">W wersji brzegowej 44 + nie ma żadnych znanych problemów ze zgodnością z nowym standardem.</span><span class="sxs-lookup"><span data-stu-id="eedfa-271">Edge version 44+ doesn't have any known compatibility problems with the new standard.</span></span>

### <a name="test-with-edge-chromium"></a><span data-ttu-id="eedfa-272">Testowanie przy użyciu krawędzi (chrom)</span><span class="sxs-lookup"><span data-stu-id="eedfa-272">Test with Edge (Chromium)</span></span>

<span data-ttu-id="eedfa-273">Flagi SameSite są ustawiane na stronie `edge://flags/#same-site-by-default-cookies`.</span><span class="sxs-lookup"><span data-stu-id="eedfa-273">SameSite flags are set on the `edge://flags/#same-site-by-default-cookies` page.</span></span> <span data-ttu-id="eedfa-274">Nie odnaleziono problemów ze zgodnością z funkcją chromu.</span><span class="sxs-lookup"><span data-stu-id="eedfa-274">No compatibility issues were discovered with Edge Chromium.</span></span>

### <a name="test-with-electron"></a><span data-ttu-id="eedfa-275">Testuj przy użyciu elektronu</span><span class="sxs-lookup"><span data-stu-id="eedfa-275">Test with Electron</span></span>

<span data-ttu-id="eedfa-276">Wersje elektronów obejmują starsze wersje chromu.</span><span class="sxs-lookup"><span data-stu-id="eedfa-276">Versions of Electron include older versions of Chromium.</span></span> <span data-ttu-id="eedfa-277">Na przykład wersja elektronów używana przez zespoły to chrom 66, który wykazuje starsze zachowanie.</span><span class="sxs-lookup"><span data-stu-id="eedfa-277">For example, the version of Electron used by Teams is Chromium 66, which exhibits the older behavior.</span></span> <span data-ttu-id="eedfa-278">Należy przeprowadzić własne testy zgodności z wersją elektronów używaną przez produkt.</span><span class="sxs-lookup"><span data-stu-id="eedfa-278">You must perform your own compatibility testing with the version of Electron your product uses.</span></span> <span data-ttu-id="eedfa-279">Zobacz [Obsługa starszych przeglądarek](#sob).</span><span class="sxs-lookup"><span data-stu-id="eedfa-279">See [Supporting older browsers](#sob).</span></span>

## <a name="reverting-samesite-patches"></a><span data-ttu-id="eedfa-280">Cofanie poprawek SameSite</span><span class="sxs-lookup"><span data-stu-id="eedfa-280">Reverting SameSite patches</span></span>

<span data-ttu-id="eedfa-281">Można przywrócić zaktualizowane zachowanie sameSite w aplikacjach .NET Framework do poprzedniego zachowania, gdzie atrybut sameSite nie jest emitowany dla wartości `None`, i przywraca pliki cookie uwierzytelniania i sesji w celu nieemitowania wartości.</span><span class="sxs-lookup"><span data-stu-id="eedfa-281">You can revert the updated sameSite behavior in .NET Framework apps to its previous behavior where the sameSite attribute is not emitted for a value of `None`, and revert the authentication and session cookies to not emit the value.</span></span> <span data-ttu-id="eedfa-282">Ta wartość powinna zostać wyświetlona jako *niezwykle tymczasowe rozwiązanie*, ponieważ zmiany w programie Chrome spowodują przerwanie wszelkich zewnętrznych żądań post lub uwierzytelnienie dla użytkowników przy użyciu przeglądarek, które obsługują zmiany w standardowym.</span><span class="sxs-lookup"><span data-stu-id="eedfa-282">This should be viewed as an *extremely temporary fix*, as the Chrome changes will break any external POST requests or authentication for users using browsers which support the changes to the standard.</span></span>

### <a name="reverting-net-472-behavior"></a><span data-ttu-id="eedfa-283">Przywracanie zachowania 4.7.2 .NET</span><span class="sxs-lookup"><span data-stu-id="eedfa-283">Reverting .NET 4.7.2 behavior</span></span>

<span data-ttu-id="eedfa-284">Zaktualizuj *plik Web. config* , aby uwzględnić następujące ustawienia konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="eedfa-284">Update *web.config* to include the following configuration settings:</span></span>

```xml
<configuration> 
  <appSettings>
    <add key="aspnet:SuppressSameSiteNone" value="true" />
  </appSettings>
 
  <system.web> 
    <authentication> 
      <forms cookieSameSite="None" /> 
    </authentication> 
    <sessionState cookieSameSite="None" /> 
  </system.web> 
</configuration>
```

## <a name="additional-resources"></a><span data-ttu-id="eedfa-285">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="eedfa-285">Additional resources</span></span>

* [<span data-ttu-id="eedfa-286">Nadchodzące zmiany plików cookie SameSite w ASP.NET i ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="eedfa-286">Upcoming SameSite Cookie Changes in ASP.NET and ASP.NET Core</span></span>](https://devblogs.microsoft.com/aspnet/upcoming-samesite-cookie-changes-in-asp-net-and-asp-net-core/)
* [<span data-ttu-id="eedfa-287">Wskazówki dotyczące testowania i debugowania SameSite-by-default i "SameSite = none; Bezpieczne "pliki cookie</span><span class="sxs-lookup"><span data-stu-id="eedfa-287">Tips for testing and debugging SameSite-by-default and “SameSite=None; Secure” cookies</span></span>](https://www.chromium.org/updates/same-site/test-debug)
* [<span data-ttu-id="eedfa-288">Blog chromu: deweloperzy: przygotowanie do nowego SameSite = none; Ustawienia bezpiecznego pliku cookie</span><span class="sxs-lookup"><span data-stu-id="eedfa-288">Chromium Blog:Developers: Get Ready for New SameSite=None; Secure Cookie Settings</span></span>](https://blog.chromium.org/2019/10/developers-get-ready-for-new.html)
* [<span data-ttu-id="eedfa-289">Wyjaśniono pliki cookie SameSite</span><span class="sxs-lookup"><span data-stu-id="eedfa-289">SameSite cookies explained</span></span>](https://web.dev/samesite-cookies-explained/)
* [<span data-ttu-id="eedfa-290">Aktualizacje programu Chrome</span><span class="sxs-lookup"><span data-stu-id="eedfa-290">Chrome Updates</span></span>](https://www.chromium.org/updates/same-site)
* [<span data-ttu-id="eedfa-291">Poprawki programu .NET SameSite</span><span class="sxs-lookup"><span data-stu-id="eedfa-291">.NET SameSite Patches</span></span>](/aspnet/samesite/kbs-samesite)
* [<span data-ttu-id="eedfa-292">Informacje o tej samej lokacji dla aplikacji sieci Web platformy Azure</span><span class="sxs-lookup"><span data-stu-id="eedfa-292">Azure Web Applications Same Site Information</span></span>](https://azure.microsoft.com/updates/app-service-samesite-cookie-update/)
* [<span data-ttu-id="eedfa-293">Informacje o tej samej lokacji w usłudze Azure ActiveDirectory</span><span class="sxs-lookup"><span data-stu-id="eedfa-293">Azure ActiveDirectory Same Site Information</span></span>](/azure/active-directory/develop/howto-handle-samesite-cookie-changes-chrome-browser)

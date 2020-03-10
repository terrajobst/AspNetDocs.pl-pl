---
title: Przykład pliku cookie SameSite dla ASP.NET C# 4.7.2 MVC
author: blowdart
description: Przykład pliku cookie SameSite dla ASP.NET C# 4.7.2 MVC
ms.author: riande
ms.date: 2/15/2019
uid: samesite/csMVC
ms.openlocfilehash: dcbd0bee009669fb747d74e6ccef07fbae70a236
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78544724"
---
# <a name="samesite-cookie-sample-for-aspnet-472-c-mvc"></a><span data-ttu-id="8ac66-103">Przykład pliku cookie SameSite dla ASP.NET C# 4.7.2 MVC</span><span class="sxs-lookup"><span data-stu-id="8ac66-103">SameSite cookie sample for ASP.NET 4.7.2 C# MVC</span></span>

<span data-ttu-id="8ac66-104">.NET Framework 4,7 ma wbudowaną obsługę atrybutu [SameSite](https://www.owasp.org/index.php/SameSite) , ale jest zgodna z oryginalnym standardem.</span><span class="sxs-lookup"><span data-stu-id="8ac66-104">.NET Framework 4.7 has built-in support for the [SameSite](https://www.owasp.org/index.php/SameSite) attribute, but it adheres to the original standard.</span></span>
<span data-ttu-id="8ac66-105">Zachowanie poprawek zostało zmienione znaczenia `SameSite.None`, aby emitować atrybut o wartości `None`, zamiast wyemitować wartość w ogóle.</span><span class="sxs-lookup"><span data-stu-id="8ac66-105">The patched behavior changed the meaning of `SameSite.None` to emit the attribute with a value of `None`, rather than not emit the value at all.</span></span> <span data-ttu-id="8ac66-106">Jeśli nie chcesz emitować wartości, możesz ustawić właściwość `SameSite` w pliku cookie na wartość-1.</span><span class="sxs-lookup"><span data-stu-id="8ac66-106">If you want to not emit the value you can set the `SameSite` property on a cookie to -1.</span></span>

## <a name="sampleCode"></a><span data-ttu-id="8ac66-107">Pisanie atrybutu SameSite</span><span class="sxs-lookup"><span data-stu-id="8ac66-107">Writing the SameSite attribute</span></span>

<span data-ttu-id="8ac66-108">Poniżej przedstawiono przykład sposobu pisania atrybutu SameSite w pliku cookie;</span><span class="sxs-lookup"><span data-stu-id="8ac66-108">Following is an example of how to write a SameSite attribute on a cookie;</span></span>

```c#
// Create the cookie
HttpCookie sameSiteCookie = new HttpCookie("SameSiteSample");

// Set a value for the cookieSite none.
// Note this will also require you to be running on HTTPS
sameSiteCookie.Value = "sample";

// Set the secure flag, which Chrome's changes will require for Same
sameSiteCookie.Secure = true;

// Set the cookie to HTTP only which is good practice unless you really do need
// to access it client side in scripts.
sameSiteCookie.HttpOnly = true;

// Add the SameSite attribute, this will emit the attribute with a value of none.
// To not emit the attribute at all set the SameSite property to -1.
sameSiteCookie.SameSite = SameSiteMode.None;

// Add the cookie to the response cookie collection
Response.Cookies.Add(sameSiteCookie);
```

[!INCLUDE[](~/includes/MTcomments.md)]

<span data-ttu-id="8ac66-109">Domyślny atrybut sameSite stanu sesji jest ustawiany w parametrze "cookieSameSite" ustawień sesji w `web.config`</span><span class="sxs-lookup"><span data-stu-id="8ac66-109">The default sameSite attribute for session state is set in the 'cookieSameSite' parameter of the session settings in `web.config`</span></span>

```xml
<system.web>
  <sessionState cookieSameSite="None">     
  </sessionState>
</system.web>
```

## <a name="mvc-authentication"></a><span data-ttu-id="8ac66-110">Uwierzytelnianie MVC</span><span class="sxs-lookup"><span data-stu-id="8ac66-110">MVC Authentication</span></span>

<span data-ttu-id="8ac66-111">Uwierzytelnianie oparte na plikach cookie OWIN MVC korzysta z Menedżera plików cookie, aby umożliwić zmianę atrybutów plików cookie.</span><span class="sxs-lookup"><span data-stu-id="8ac66-111">OWIN MVC cookie based authentication uses a cookie manager to enable the changing of cookie attributes.</span></span> <span data-ttu-id="8ac66-112">[SameSiteCookieManager.cs](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472CSharpMVC5/SameSiteCookieManager.cs) to implementacja klasy, którą można skopiować do własnych projektów.</span><span class="sxs-lookup"><span data-stu-id="8ac66-112">The [SameSiteCookieManager.cs](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472CSharpMVC5/SameSiteCookieManager.cs) is an implementation of such a class which you can copy into your own projects.</span></span> 

<span data-ttu-id="8ac66-113">Należy upewnić się, że składniki Microsoft. Owin są uaktualnione do wersji 4.1.0 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="8ac66-113">You must ensure your Microsoft.Owin components are all upgraded to version 4.1.0 or greater.</span></span> <span data-ttu-id="8ac66-114">Sprawdź plik `packages.config`, aby upewnić się, że wszystkie numery wersji są zgodne, na przykład.</span><span class="sxs-lookup"><span data-stu-id="8ac66-114">Check your `packages.config` file to ensure all the version numbers match, for example.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<packages>
  <!-- other packages -->
  <package id="Microsoft.Owin.Host.SystemWeb" version="4.1.0" targetFramework="net472" />
  <package id="Microsoft.Owin.Security" version="4.1.0" targetFramework="net472" />
  <package id="Microsoft.Owin.Security.Cookies" version="4.1.0" targetFramework="net472" />
  <package id="Microsoft.Web.Infrastructure" version="1.0.0.0" targetFramework="net472" />
  <package id="Owin" version="1.0" targetFramework="net472" />
</packages>
```

<span data-ttu-id="8ac66-115">Składniki uwierzytelniania należy następnie skonfigurować tak, aby używały pliku Cookiemanager w klasie startowej;</span><span class="sxs-lookup"><span data-stu-id="8ac66-115">The authentication components must then be configured to use the CookieManager in your startup class;</span></span>

```c#
public void Configuration(IAppBuilder app)
{
    app.UseCookieAuthentication(new CookieAuthenticationOptions
    {
        CookieSameSite = SameSiteMode.None,
        CookieHttpOnly = true,
        CookieSecure = CookieSecureOption.Always,
        CookieManager = new SameSiteCookieManager(new SystemWebCookieManager())
    });
}
```

<span data-ttu-id="8ac66-116">Należy ustawić Menedżera plików cookie dla *każdego* składnika, który go obsługuje, obejmuje to CookieAuthentication i OpenIdConnectAuthentication.</span><span class="sxs-lookup"><span data-stu-id="8ac66-116">A cookie manager must be set on *each* component that supports it, this includes CookieAuthentication and OpenIdConnectAuthentication.</span></span>

<span data-ttu-id="8ac66-117">SystemWebCookieManager jest używany w celu uniknięcia [znanych problemów](https://github.com/aspnet/AspNetKatana/wiki/System.Web-response-cookie-integration-issues) z integracją plików cookie odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="8ac66-117">The SystemWebCookieManager is used to avoid [known issues](https://github.com/aspnet/AspNetKatana/wiki/System.Web-response-cookie-integration-issues) with response cookie integration.</span></span>

### <a name="running-the-sample"></a><span data-ttu-id="8ac66-118">Uruchamianie przykładowej aplikacji</span><span class="sxs-lookup"><span data-stu-id="8ac66-118">Running the sample</span></span>

<span data-ttu-id="8ac66-119">Po uruchomieniu przykładowego projektu Załaduj debuger przeglądarki na stronie początkowej i użyj go do wyświetlenia kolekcji plików cookie dla witryny.</span><span class="sxs-lookup"><span data-stu-id="8ac66-119">If you run the sample project  load your browser debugger on the initial page and use it to view the cookie collection for the site.</span></span>
<span data-ttu-id="8ac66-120">Aby to zrobić, w przeglądarce Edge i Chrome naciśnij `F12` następnie wybierz kartę `Application` i kliknij adres URL witryny poniżej opcji `Cookies` w sekcji `Storage`.</span><span class="sxs-lookup"><span data-stu-id="8ac66-120">To do so in Edge and Chrome press `F12` then select the `Application` tab and click the site URL under the `Cookies` option in the `Storage` section.</span></span>

![Lista plików cookie debugera przeglądarki](sample/img/BrowserDebugger.png)

<span data-ttu-id="8ac66-122">Po kliknięciu przycisku "Utwórz pliki cookie" można zobaczyć, że w powyższym obrazie plik cookie został utworzony przez próbkę `Lax`SameSite, dopasowując wartość ustawioną w [przykładowym kodzie](#sampleCode).</span><span class="sxs-lookup"><span data-stu-id="8ac66-122">You can see from the image above that the cookie created by the sample when you click the "Create Cookies" button has a SameSite attribute value of `Lax`, matching the value set in the [sample code](#sampleCode).</span></span>

## <a name="interception"></a><span data-ttu-id="8ac66-123">Przechwytywanie plików cookie, które nie są kontrolowane</span><span class="sxs-lookup"><span data-stu-id="8ac66-123">Intercepting cookies you do not control</span></span>

<span data-ttu-id="8ac66-124">W programie .NET 4.5.2 wprowadzono nowe zdarzenie dotyczące przechwytywania zapisu nagłówków, `Response.AddOnSendingHeaders`.</span><span class="sxs-lookup"><span data-stu-id="8ac66-124">.NET 4.5.2 introduced a new event for intercepting the writing of headers, `Response.AddOnSendingHeaders`.</span></span> <span data-ttu-id="8ac66-125">Może to służyć do przechwytywania plików cookie przed ich zwróceniem do komputera klienckiego.</span><span class="sxs-lookup"><span data-stu-id="8ac66-125">This can be used to intercept cookies before they are returned to the client machine.</span></span> <span data-ttu-id="8ac66-126">W przykładzie tworzymy zdarzenie do metody statycznej, która sprawdza, czy przeglądarka obsługuje nowe zmiany sameSite, a jeśli nie, zmienia pliki cookie, aby nie wyemitować atrybutu, jeśli ustawiono nową wartość `None`.</span><span class="sxs-lookup"><span data-stu-id="8ac66-126">In the sample we wire up the event to a static method which checks whether the browser supports the new sameSite changes, and if not, changes the cookies to not emit the attribute if the new `None` value has been set.</span></span>

<span data-ttu-id="8ac66-127">Zapoznaj się z plikiem [Global. asax](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472CSharpMVC5/Global.asax.cs) przykładem podłączania zdarzenia i [SameSiteCookieRewriter.cs](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472CSharpMVC5/SameSiteCookieRewriter.cs) , aby zapoznać się z przykładem obsługi zdarzenia i dostosowując plik cookie `sameSite` atrybut, który można skopiować do własnego kodu.</span><span class="sxs-lookup"><span data-stu-id="8ac66-127">See [global.asax](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472CSharpMVC5/Global.asax.cs) for an example of hooking up the event and [SameSiteCookieRewriter.cs](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472CSharpMVC5/SameSiteCookieRewriter.cs) for an example of handling the event and adjusting the cookie `sameSite` attribute which you can copy into your own code.</span></span>

```c#
public static void FilterSameSiteNoneForIncompatibleUserAgents(object sender)
{
    HttpApplication application = sender as HttpApplication;
    if (application != null)
    {
        var userAgent = application.Context.Request.UserAgent;
        if (SameSite.BrowserDetection.DisallowsSameSiteNone(userAgent))
        {
            HttpContext.Current.Response.AddOnSendingHeaders(context =>
            {
                var cookies = context.Response.Cookies;
                for (var i = 0; i < cookies.Count; i++)
                {
                    var cookie = cookies[i];
                    if (cookie.SameSite == SameSiteMode.None)
                    {
                        cookie.SameSite = (SameSiteMode)(-1); // Unspecified
                    }
                }
            });
        }
    }
}
```

<span data-ttu-id="8ac66-128">W ten sam sposób można zmienić określone zachowanie nazwanego pliku cookie. Poniższy przykład dostosowuje domyślny plik cookie uwierzytelniania z `Lax` do `None` w przeglądarkach obsługujących wartość `None` lub usuwa atrybut sameSite w przeglądarkach, które nie obsługują `None`.</span><span class="sxs-lookup"><span data-stu-id="8ac66-128">You can change specific named cookie behavior in much the same way; the sample below adjust the default authentication cookie from `Lax` to `None` on browsers which support the `None` value, or removes the sameSite attribute on browsers which do not support `None`.</span></span>

```c#
public static void AdjustSpecificCookieSettings()
{
    HttpContext.Current.Response.AddOnSendingHeaders(context =>
    {
        var cookies = context.Response.Cookies;
        for (var i = 0; i < cookies.Count; i++)
        {
            var cookie = cookies[i]; 
            // Forms auth: ".ASPXAUTH"
            // Session: "ASP.NET_SessionId"
            if (string.Equals(".ASPXAUTH", cookie.Name, StringComparison.Ordinal))
            { 
                if (SameSite.BrowserDetection.DisallowsSameSiteNone(userAgent))
                {
                    cookie.SameSite = -1;
                }
                else
                {
                    cookie.SameSite = SameSiteMode.None;
                }
                cookie.Secure = true;
            }
        }
    });
}
```

### <a name="more-information"></a><span data-ttu-id="8ac66-129">Więcej informacji</span><span class="sxs-lookup"><span data-stu-id="8ac66-129">More Information</span></span>
 
[<span data-ttu-id="8ac66-130">Aktualizacje programu Chrome</span><span class="sxs-lookup"><span data-stu-id="8ac66-130">Chrome Updates</span></span>](https://www.chromium.org/updates/same-site)

[<span data-ttu-id="8ac66-131">Dokumentacja OWIN SameSite</span><span class="sxs-lookup"><span data-stu-id="8ac66-131">OWIN SameSite Documentation</span></span>](/aspnet/samesite/owin-samesite)

[<span data-ttu-id="8ac66-132">Dokumentacja ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8ac66-132">ASP.NET Documentation</span></span>](/aspnet/samesite/system-web-samesite)

[<span data-ttu-id="8ac66-133">Poprawki programu .NET SameSite</span><span class="sxs-lookup"><span data-stu-id="8ac66-133">.NET SameSite Patches</span></span>](/aspnet/samesite/kbs-samesite)
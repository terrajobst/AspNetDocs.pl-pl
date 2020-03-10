---
title: Przykład pliku cookie SameSite dla ASP.NET C# 4.7.2 WebForms
author: blowdart
description: Przykład pliku cookie SameSite dla ASP.NET C# 4.7.2 WebForms
ms.author: riande
ms.date: 2/15/2019
uid: samesite/CSharpWebForms
ms.openlocfilehash: 50d4745eca5954275abaa59dab726e7cf7ea193f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78526118"
---
# <a name="samesite-cookie-sample-for-aspnet-472-c-webforms"></a><span data-ttu-id="11b8c-103">Przykład pliku cookie SameSite dla ASP.NET C# 4.7.2 WebForms</span><span class="sxs-lookup"><span data-stu-id="11b8c-103">SameSite cookie sample for ASP.NET 4.7.2 C# WebForms</span></span>

<span data-ttu-id="11b8c-104">.NET Framework 4,7 ma wbudowaną obsługę atrybutu [SameSite](https://www.owasp.org/index.php/SameSite) , ale jest zgodna z oryginalnym standardem.</span><span class="sxs-lookup"><span data-stu-id="11b8c-104">.NET Framework 4.7 has built-in support for the [SameSite](https://www.owasp.org/index.php/SameSite) attribute, but it adheres to the original standard.</span></span>
<span data-ttu-id="11b8c-105">Zachowanie poprawek zostało zmienione znaczenia `SameSite.None`, aby emitować atrybut o wartości `None`, zamiast wyemitować wartość w ogóle.</span><span class="sxs-lookup"><span data-stu-id="11b8c-105">The patched behavior changed the meaning of `SameSite.None` to emit the attribute with a value of `None`, rather than not emit the value at all.</span></span> <span data-ttu-id="11b8c-106">Jeśli nie chcesz emitować wartości, możesz ustawić właściwość `SameSite` w pliku cookie na wartość-1.</span><span class="sxs-lookup"><span data-stu-id="11b8c-106">If you want to not emit the value you can set the `SameSite` property on a cookie to -1.</span></span>

## <a name="sampleCode"></a><span data-ttu-id="11b8c-107">Pisanie atrybutu SameSite</span><span class="sxs-lookup"><span data-stu-id="11b8c-107">Writing the SameSite attribute</span></span>

<span data-ttu-id="11b8c-108">Poniżej przedstawiono przykład sposobu pisania atrybutu SameSite w pliku cookie;</span><span class="sxs-lookup"><span data-stu-id="11b8c-108">Following is an example of how to write a SameSite attribute on a cookie;</span></span>

```c#
// Create the cookie
HttpCookie sameSiteCookie = new HttpCookie("SameSiteSample");

// Set a value for the cookie
sameSiteCookie.Value = "sample";

// Set the secure flag, which Chrome's changes will require for SameSite none.
// Note this will also require you to be running on HTTPS
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

<span data-ttu-id="11b8c-109">Domyślny atrybut sameSite dla pliku cookie uwierzytelniania formularzy jest ustawiany w `cookieSameSite` parametr ustawień uwierzytelniania formularzy w `web.config`</span><span class="sxs-lookup"><span data-stu-id="11b8c-109">The default sameSite attribute for a forms authentication cookie is set in the `cookieSameSite` parameter of the forms authentication settings in `web.config`</span></span> 

```xml
<system.web>
  <authentication mode="Forms">
    <forms name=".ASPXAUTH" loginUrl="~/" cookieSameSite="None" requireSSL="true">
    </forms>
  </authentication>
</system.web>
```

<span data-ttu-id="11b8c-110">Domyślny atrybut sameSite stanu sesji jest również ustawiany w parametrze "cookieSameSite" ustawień sesji w `web.config`</span><span class="sxs-lookup"><span data-stu-id="11b8c-110">The default sameSite attribute for session state is also set in the 'cookieSameSite' parameter of the session settings in `web.config`</span></span>

```xml
<system.web>
  <sessionState cookieSameSite="None">     
  </sessionState>
</system.web>
```

<span data-ttu-id="11b8c-111">Aktualizacja z listopada 2019 do platformy .NET zmieniła domyślne ustawienia uwierzytelniania formularzy i sesji na `lax` jak jest to najbezpieczniejsze ustawienie, jednak jeśli osadzasz strony w iframes, może być konieczne przywrócenie tego ustawienia do wartości none, a następnie dodanie kodu [przechwycenia](#interception) pokazanego poniżej w celu dostosowania zachowania `none` w zależności od możliwości przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="11b8c-111">The November 2019 update to .NET changed the default settings for Forms Authentication and Session to `lax` as is the most compatible setting, however if you embed pages into iframes you may need to revert this setting to None, and then add the [interception](#interception) code shown below to adjust the `none` behavior depending on browser capability.</span></span>

### <a name="running-the-sample"></a><span data-ttu-id="11b8c-112">Uruchamianie przykładowej aplikacji</span><span class="sxs-lookup"><span data-stu-id="11b8c-112">Running the sample</span></span>

<span data-ttu-id="11b8c-113">Po uruchomieniu przykładowego projektu Załaduj debuger przeglądarki na stronie początkowej i użyj go do wyświetlenia kolekcji plików cookie dla witryny.</span><span class="sxs-lookup"><span data-stu-id="11b8c-113">If you run the sample project  load your browser debugger on the initial page and use it to view the cookie collection for the site.</span></span>
<span data-ttu-id="11b8c-114">Aby to zrobić, w przeglądarce Edge i Chrome naciśnij `F12` następnie wybierz kartę `Application` i kliknij adres URL witryny poniżej opcji `Cookies` w sekcji `Storage`.</span><span class="sxs-lookup"><span data-stu-id="11b8c-114">To do so in Edge and Chrome press `F12` then select the `Application` tab and click the site URL under the `Cookies` option in the `Storage` section.</span></span>

![Lista plików cookie debugera przeglądarki](sample/img/BrowserDebugger.png)

<span data-ttu-id="11b8c-116">Po kliknięciu przycisku "Utwórz pliki cookie" można zobaczyć, że w powyższym obrazie plik cookie został utworzony przez próbkę `Lax`SameSite, dopasowując wartość ustawioną w [przykładowym kodzie](#sampleCode).</span><span class="sxs-lookup"><span data-stu-id="11b8c-116">You can see from the image above that the cookie created by the sample when you click the "Create Cookies" button has a SameSite attribute value of `Lax`, matching the value set in the [sample code](#sampleCode).</span></span>

## <a name="interception"></a><span data-ttu-id="11b8c-117">Przechwytywanie plików cookie, które nie są kontrolowane</span><span class="sxs-lookup"><span data-stu-id="11b8c-117">Intercepting cookies you do not control</span></span>

<span data-ttu-id="11b8c-118">W programie .NET 4.5.2 wprowadzono nowe zdarzenie dotyczące przechwytywania zapisu nagłówków, `Response.AddOnSendingHeaders`.</span><span class="sxs-lookup"><span data-stu-id="11b8c-118">.NET 4.5.2 introduced a new event for intercepting the writing of headers, `Response.AddOnSendingHeaders`.</span></span> <span data-ttu-id="11b8c-119">Może to służyć do przechwytywania plików cookie przed ich zwróceniem do komputera klienckiego.</span><span class="sxs-lookup"><span data-stu-id="11b8c-119">This can be used to intercept cookies before they are returned to the client machine.</span></span> <span data-ttu-id="11b8c-120">W przykładzie tworzymy zdarzenie do metody statycznej, która sprawdza, czy przeglądarka obsługuje nowe zmiany sameSite, a jeśli nie, zmienia pliki cookie, aby nie wyemitować atrybutu, jeśli ustawiono nową wartość `None`.</span><span class="sxs-lookup"><span data-stu-id="11b8c-120">In the sample we wire up the event to a static method which checks whether the browser supports the new sameSite changes, and if not, changes the cookies to not emit the attribute if the new `None` value has been set.</span></span>

<span data-ttu-id="11b8c-121">Zapoznaj się z plikiem [Global. asax](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472CSharpWebForms/Global.asax.cs) przykładem, aby dowiedzieć się, jak podłączyć zdarzenie i [SameSiteCookieRewriter.cs](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472CSharpWebForms/SameSiteCookieRewriter.cs) na przykład obsłużyć zdarzenie i dostosować atrybut `sameSite` plików cookie.</span><span class="sxs-lookup"><span data-stu-id="11b8c-121">See [global.asax](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472CSharpWebForms/Global.asax.cs) for an example of hooking up the event and [SameSiteCookieRewriter.cs](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472CSharpWebForms/SameSiteCookieRewriter.cs) for an example of handling the event and adjusting the cookie `sameSite` attribute.</span></span>

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

<span data-ttu-id="11b8c-122">W ten sam sposób można zmienić określone zachowanie nazwanego pliku cookie. Poniższy przykład dostosowuje domyślny plik cookie uwierzytelniania z `Lax` do `None` w przeglądarkach obsługujących wartość `None` lub usuwa atrybut sameSite w przeglądarkach, które nie obsługują `None`.</span><span class="sxs-lookup"><span data-stu-id="11b8c-122">You can change specific named cookie behavior in much the same way; the sample below adjust the default authentication cookie from `Lax` to `None` on browsers which support the `None` value, or removes the sameSite attribute on browsers which do not support `None`.</span></span>

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

## <a name="more-information"></a><span data-ttu-id="11b8c-123">Więcej informacji</span><span class="sxs-lookup"><span data-stu-id="11b8c-123">More Information</span></span>

[<span data-ttu-id="11b8c-124">Aktualizacje programu Chrome</span><span class="sxs-lookup"><span data-stu-id="11b8c-124">Chrome Updates</span></span>](https://www.chromium.org/updates/same-site)

[<span data-ttu-id="11b8c-125">Dokumentacja ASP.NET</span><span class="sxs-lookup"><span data-stu-id="11b8c-125">ASP.NET Documentation</span></span>](/aspnet/samesite/system-web-samesite)

[<span data-ttu-id="11b8c-126">Poprawki programu .NET SameSite</span><span class="sxs-lookup"><span data-stu-id="11b8c-126">.NET SameSite Patches</span></span>](/aspnet/samesite/kbs-samesite)
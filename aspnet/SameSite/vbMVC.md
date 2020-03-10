---
title: Przykład pliku cookie SameSite dla ASP.NET 4.7.2 VB MVC
author: blowdart
description: Przykład pliku cookie SameSite dla ASP.NET 4.7.2 VB MVC
ms.author: riande
ms.date: 2/15/2019
uid: samesite/vbMVC
ms.openlocfilehash: f6effce6075f94fb58ce10ec08bf010fab8b4b56
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78547811"
---
# <a name="samesite-cookie-sample-for-aspnet-472-vb-mvc"></a><span data-ttu-id="ca9b0-103">Przykład pliku cookie SameSite dla ASP.NET 4.7.2 VB MVC</span><span class="sxs-lookup"><span data-stu-id="ca9b0-103">SameSite cookie sample for ASP.NET 4.7.2 VB MVC</span></span>

<span data-ttu-id="ca9b0-104">.NET Framework 4,7 ma wbudowaną obsługę atrybutu [SameSite](https://www.owasp.org/index.php/SameSite) , ale jest zgodna z oryginalnym standardem.</span><span class="sxs-lookup"><span data-stu-id="ca9b0-104">.NET Framework 4.7 has built-in support for the [SameSite](https://www.owasp.org/index.php/SameSite) attribute, but it adheres to the original standard.</span></span>
<span data-ttu-id="ca9b0-105">Zachowanie poprawek zostało zmienione znaczenia `SameSite.None`, aby emitować atrybut o wartości `None`, zamiast wyemitować wartość w ogóle.</span><span class="sxs-lookup"><span data-stu-id="ca9b0-105">The patched behavior changed the meaning of `SameSite.None` to emit the attribute with a value of `None`, rather than not emit the value at all.</span></span> <span data-ttu-id="ca9b0-106">Jeśli nie chcesz emitować wartości, możesz ustawić właściwość `SameSite` w pliku cookie na wartość-1.</span><span class="sxs-lookup"><span data-stu-id="ca9b0-106">If you want to not emit the value you can set the `SameSite` property on a cookie to -1.</span></span>

## <a name="sampleCode"></a><span data-ttu-id="ca9b0-107">Pisanie atrybutu SameSite</span><span class="sxs-lookup"><span data-stu-id="ca9b0-107">Writing the SameSite attribute</span></span>

<span data-ttu-id="ca9b0-108">Poniżej przedstawiono przykład sposobu pisania atrybutu SameSite w pliku cookie;</span><span class="sxs-lookup"><span data-stu-id="ca9b0-108">Following is an example of how to write a SameSite attribute on a cookie;</span></span>

```vb
' Create the cookie
Dim sameSiteCookie As New HttpCookie("sameSiteSample")

' Set a value for the cookie
sameSiteCookie.Value = "sample"

' Set the secure flag, which Chrome's changes will require for SameSite none.
' Note this will also require you to be running on HTTPS
sameSiteCookie.Secure = True

' Set the cookie to HTTP only which is good practice unless you really do need
' to access it client side in scripts.
sameSiteCookie.HttpOnly = True

' Expire the cookie in 1 minute
sameSiteCookie.Expires = Date.Now.AddMinutes(1)

' Add the SameSite attribute, this will emit the attribute with a value of none.
' To Not emit the attribute at all set the SameSite property to -1.
sameSiteCookie.SameSite = SameSiteMode.None

' Add the cookie to the response cookie collection
Response.Cookies.Add(sameSiteCookie)
```

[!INCLUDE[](~/includes/MTcomments.md)]

<span data-ttu-id="ca9b0-109">Domyślny atrybut sameSite stanu sesji jest ustawiany w parametrze "cookieSameSite" ustawień sesji w `web.config`</span><span class="sxs-lookup"><span data-stu-id="ca9b0-109">The default sameSite attribute for session state is set in the 'cookieSameSite' parameter of the session settings in `web.config`</span></span>

```xml
<system.web>
  <sessionState cookieSameSite="None">     
  </sessionState>
</system.web>
```

## <a name="mvc-authentication"></a><span data-ttu-id="ca9b0-110">Uwierzytelnianie MVC</span><span class="sxs-lookup"><span data-stu-id="ca9b0-110">MVC Authentication</span></span>

<span data-ttu-id="ca9b0-111">Uwierzytelnianie oparte na plikach cookie OWIN MVC korzysta z Menedżera plików cookie, aby umożliwić zmianę atrybutów plików cookie.</span><span class="sxs-lookup"><span data-stu-id="ca9b0-111">OWIN MVC cookie based authentication uses a cookie manager to enable the changing of cookie attributes.</span></span> <span data-ttu-id="ca9b0-112">[SameSiteCookieManager. vb](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472VisualBasicMVC5/SameSiteCookieManager.vb) to implementacja klasy, którą można skopiować do własnych projektów.</span><span class="sxs-lookup"><span data-stu-id="ca9b0-112">The [SameSiteCookieManager.vb](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472VisualBasicMVC5/SameSiteCookieManager.vb) is an implementation of such a class which you can copy into your own projects.</span></span> 

<span data-ttu-id="ca9b0-113">Należy upewnić się, że składniki Microsoft. Owin są uaktualnione do wersji 4.1.0 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="ca9b0-113">You must ensure your Microsoft.Owin components are all upgraded to version 4.1.0 or greater.</span></span> <span data-ttu-id="ca9b0-114">Sprawdź plik `packages.config`, aby upewnić się, że wszystkie numery wersji są zgodne, na przykład.</span><span class="sxs-lookup"><span data-stu-id="ca9b0-114">Check your `packages.config` file to ensure all the version numbers match, for example.</span></span>

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

<span data-ttu-id="ca9b0-115">Składniki uwierzytelniania muszą być skonfigurowane do używania pliku Cookiemanager w klasie startowej.</span><span class="sxs-lookup"><span data-stu-id="ca9b0-115">The authentication components must be configured to use the CookieManager in your startup class;</span></span>

```vb
Public Sub Configuration(app As IAppBuilder)
    app.UseCookieAuthentication(New CookieAuthenticationOptions() With {
        .CookieSameSite = SameSiteMode.None,
        .CookieHttpOnly = True,
        .CookieSecure = CookieSecureOption.Always,
        .CookieManager = New SameSiteCookieManager(New SystemWebCookieManager())
    })
End Sub
```

<span data-ttu-id="ca9b0-116">Należy ustawić Menedżera plików cookie dla *każdego* składnika, który go obsługuje, obejmuje to CookieAuthentication i OpenIdConnectAuthentication.</span><span class="sxs-lookup"><span data-stu-id="ca9b0-116">A cookie manager must be set on *each* component that supports it, this includes CookieAuthentication and OpenIdConnectAuthentication.</span></span>

<span data-ttu-id="ca9b0-117">SystemWebCookieManager jest używany w celu uniknięcia [znanych problemów](https://github.com/aspnet/AspNetKatana/wiki/System.Web-response-cookie-integration-issues) z integracją plików cookie odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="ca9b0-117">The SystemWebCookieManager is used to avoid [known issues](https://github.com/aspnet/AspNetKatana/wiki/System.Web-response-cookie-integration-issues) with response cookie integration.</span></span>

### <a name="running-the-sample"></a><span data-ttu-id="ca9b0-118">Uruchamianie przykładowej aplikacji</span><span class="sxs-lookup"><span data-stu-id="ca9b0-118">Running the sample</span></span>

<span data-ttu-id="ca9b0-119">W przypadku uruchomienia przykładowego projektu Załaduj debuger przeglądarki na stronie początkowej i użyj go, aby wyświetlić kolekcję plików cookie dla tej witryny.</span><span class="sxs-lookup"><span data-stu-id="ca9b0-119">If you run the sample project please load your browser debugger on the initial page and use it to view the cookie collection for the site.</span></span>
<span data-ttu-id="ca9b0-120">Aby to zrobić, w przeglądarce Edge i Chrome naciśnij `F12` następnie wybierz kartę `Application` i kliknij adres URL witryny poniżej opcji `Cookies` w sekcji `Storage`.</span><span class="sxs-lookup"><span data-stu-id="ca9b0-120">To do so in Edge and Chrome press `F12` then select the `Application` tab and click the site URL under the `Cookies` option in the `Storage` section.</span></span>

![Lista plików cookie debugera przeglądarki](sample/img/BrowserDebugger.png)

<span data-ttu-id="ca9b0-122">Po kliknięciu przycisku "Utwórz SameSite cookie" można zobaczyć z obrazu powyżej, że plik cookie utworzony przez próbkę ma wartość `Lax`, dopasowując wartość ustawioną w [przykładowym kodzie](#sampleCode).</span><span class="sxs-lookup"><span data-stu-id="ca9b0-122">You can see from the image above that the cookie created by the sample when you click the "Create SameSite Cookie" button has a SameSite attribute value of `Lax`, matching the value set in the [sample code](#sampleCode).</span></span>

## <a name="interception"></a><span data-ttu-id="ca9b0-123">Przechwytywanie plików cookie, które nie są kontrolowane</span><span class="sxs-lookup"><span data-stu-id="ca9b0-123">Intercepting cookies you do not control</span></span>

<span data-ttu-id="ca9b0-124">W programie .NET 4.5.2 wprowadzono nowe zdarzenie dotyczące przechwytywania zapisu nagłówków, `Response.AddOnSendingHeaders`.</span><span class="sxs-lookup"><span data-stu-id="ca9b0-124">.NET 4.5.2 introduced a new event for intercepting the writing of headers, `Response.AddOnSendingHeaders`.</span></span> <span data-ttu-id="ca9b0-125">Może to służyć do przechwytywania plików cookie przed ich zwróceniem do komputera klienckiego.</span><span class="sxs-lookup"><span data-stu-id="ca9b0-125">This can be used to intercept cookies before they are returned to the client machine.</span></span> <span data-ttu-id="ca9b0-126">W przykładzie tworzymy zdarzenie do metody statycznej, która sprawdza, czy przeglądarka obsługuje nowe zmiany sameSite, a jeśli nie, zmienia pliki cookie, aby nie wyemitować atrybutu, jeśli ustawiono nową wartość `None`.</span><span class="sxs-lookup"><span data-stu-id="ca9b0-126">In the sample we wire up the event to a static method which checks whether the browser supports the new sameSite changes, and if not, changes the cookies to not emit the attribute if the new `None` value has been set.</span></span>

<span data-ttu-id="ca9b0-127">Zapoznaj się z plikiem [Global. asax](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472VisualBasicMVC5/Global.asax.vb) przykładem dotyczącym podłączania zdarzenia i [SameSiteCookieRewriter. vb](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472VisualBasicMVC5/SameSiteCookieRewriter.vb) , aby zapoznać się z przykładem obsługi zdarzenia i dostosowując plik cookie `sameSite` atrybutu, który można skopiować do własnego kodu.</span><span class="sxs-lookup"><span data-stu-id="ca9b0-127">See [global.asax](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472VisualBasicMVC5/Global.asax.vb) for an example of hooking up the event and [SameSiteCookieRewriter.vb](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472VisualBasicMVC5/SameSiteCookieRewriter.vb) for an example of handling the event and adjusting the cookie `sameSite` attribute which you can copy into your own code.</span></span>

```vb
Sub FilterSameSiteNoneForIncompatibleUserAgents(ByVal sender As Object)
    Dim application As HttpApplication = TryCast(sender, HttpApplication)

    If application IsNot Nothing Then
        Dim userAgent = application.Context.Request.UserAgent

        If SameSite.DisallowsSameSiteNone(userAgent) Then
            application.Response.AddOnSendingHeaders(
                Function(context)
                    Dim cookies = context.Response.Cookies

                    For i = 0 To cookies.Count - 1
                        Dim cookie = cookies(i)

                        If cookie.SameSite = SameSiteMode.None Then
                            cookie.SameSite = CType((-1), SameSiteMode)
                        End If
                    Next
                End Function)
        End If
    End If
End Sub
```

<span data-ttu-id="ca9b0-128">W ten sam sposób można zmienić określone zachowanie nazwanego pliku cookie. Poniższy przykład dostosowuje domyślny plik cookie uwierzytelniania z `Lax` do `None` w przeglądarkach obsługujących wartość `None` lub usuwa atrybut sameSite w przeglądarkach, które nie obsługują `None`.</span><span class="sxs-lookup"><span data-stu-id="ca9b0-128">You can change specific named cookie behavior in much the same way; the sample below adjust the default authentication cookie from `Lax` to `None` on browsers which support the `None` value, or removes the sameSite attribute on browsers which do not support `None`.</span></span>

```vb
Public Shared Sub AdjustSpecificCookieSettings()
    HttpContext.Current.Response.AddOnSendingHeaders(Function(context)
            Dim cookies = context.Response.Cookies

            For i = 0 To cookies.Count - 1
            Dim cookie = cookies(i)

            If String.Equals(".ASPXAUTH", cookie.Name, StringComparison.Ordinal) Then

                If SameSite.BrowserDetection.DisallowsSameSiteNone(userAgent) Then
                    cookie.SameSite = -1
                Else
                    cookie.SameSite = SameSiteMode.None
                End If

                cookie.Secure = True
            End If
            Next
        End Function)
End Sub
```

## <a name="more-information"></a><span data-ttu-id="ca9b0-129">Więcej informacji</span><span class="sxs-lookup"><span data-stu-id="ca9b0-129">More Information</span></span>
 
[<span data-ttu-id="ca9b0-130">Aktualizacje programu Chrome</span><span class="sxs-lookup"><span data-stu-id="ca9b0-130">Chrome Updates</span></span>](https://www.chromium.org/updates/same-site)

[<span data-ttu-id="ca9b0-131">Dokumentacja OWIN SameSite</span><span class="sxs-lookup"><span data-stu-id="ca9b0-131">OWIN SameSite Documentation</span></span>](/aspnet/samesite/owin-samesite)

[<span data-ttu-id="ca9b0-132">Dokumentacja ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ca9b0-132">ASP.NET Documentation</span></span>](/aspnet/samesite/system-web-samesite)

[<span data-ttu-id="ca9b0-133">Poprawki programu .NET SameSite</span><span class="sxs-lookup"><span data-stu-id="ca9b0-133">.NET SameSite Patches</span></span>](/aspnet/samesite/kbs-samesite)
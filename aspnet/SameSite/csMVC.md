---
title: Przykład pliku cookie SameSite dla ASP.NET C# 4.7.2 MVC
author: blowdart
description: Przykład pliku cookie SameSite dla ASP.NET C# 4.7.2 MVC
ms.author: riande
ms.date: 2/15/2019
uid: samesite/csMVC
ms.openlocfilehash: dcbd0bee009669fb747d74e6ccef07fbae70a236
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/19/2020
ms.locfileid: "77458456"
---
# <a name="samesite-cookie-sample-for-aspnet-472-c-mvc"></a>Przykład pliku cookie SameSite dla ASP.NET C# 4.7.2 MVC

.NET Framework 4,7 ma wbudowaną obsługę atrybutu [SameSite](https://www.owasp.org/index.php/SameSite) , ale jest zgodna z oryginalnym standardem.
Zachowanie poprawek zostało zmienione znaczenia `SameSite.None`, aby emitować atrybut o wartości `None`, zamiast wyemitować wartość w ogóle. Jeśli nie chcesz emitować wartości, możesz ustawić właściwość `SameSite` w pliku cookie na wartość-1.

## <a name="sampleCode"></a>Pisanie atrybutu SameSite

Poniżej przedstawiono przykład sposobu pisania atrybutu SameSite w pliku cookie;

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

Domyślny atrybut sameSite stanu sesji jest ustawiany w parametrze "cookieSameSite" ustawień sesji w `web.config`

```xml
<system.web>
  <sessionState cookieSameSite="None">     
  </sessionState>
</system.web>
```

## <a name="mvc-authentication"></a>Uwierzytelnianie MVC

Uwierzytelnianie oparte na plikach cookie OWIN MVC korzysta z Menedżera plików cookie, aby umożliwić zmianę atrybutów plików cookie. [SameSiteCookieManager.cs](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472CSharpMVC5/SameSiteCookieManager.cs) to implementacja klasy, którą można skopiować do własnych projektów. 

Należy upewnić się, że składniki Microsoft. Owin są uaktualnione do wersji 4.1.0 lub nowszej. Sprawdź plik `packages.config`, aby upewnić się, że wszystkie numery wersji są zgodne, na przykład.

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

Składniki uwierzytelniania należy następnie skonfigurować tak, aby używały pliku Cookiemanager w klasie startowej;

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

Należy ustawić Menedżera plików cookie dla *każdego* składnika, który go obsługuje, obejmuje to CookieAuthentication i OpenIdConnectAuthentication.

SystemWebCookieManager jest używany w celu uniknięcia [znanych problemów](https://github.com/aspnet/AspNetKatana/wiki/System.Web-response-cookie-integration-issues) z integracją plików cookie odpowiedzi.

### <a name="running-the-sample"></a>Uruchamianie przykładowej aplikacji

Po uruchomieniu przykładowego projektu Załaduj debuger przeglądarki na stronie początkowej i użyj go do wyświetlenia kolekcji plików cookie dla witryny.
Aby to zrobić, w przeglądarce Edge i Chrome naciśnij `F12` następnie wybierz kartę `Application` i kliknij adres URL witryny poniżej opcji `Cookies` w sekcji `Storage`.

![Lista plików cookie debugera przeglądarki](sample/img/BrowserDebugger.png)

Po kliknięciu przycisku "Utwórz pliki cookie" można zobaczyć, że w powyższym obrazie plik cookie został utworzony przez próbkę `Lax`SameSite, dopasowując wartość ustawioną w [przykładowym kodzie](#sampleCode).

## <a name="interception"></a>Przechwytywanie plików cookie, które nie są kontrolowane

W programie .NET 4.5.2 wprowadzono nowe zdarzenie dotyczące przechwytywania zapisu nagłówków, `Response.AddOnSendingHeaders`. Może to służyć do przechwytywania plików cookie przed ich zwróceniem do komputera klienckiego. W przykładzie tworzymy zdarzenie do metody statycznej, która sprawdza, czy przeglądarka obsługuje nowe zmiany sameSite, a jeśli nie, zmienia pliki cookie, aby nie wyemitować atrybutu, jeśli ustawiono nową wartość `None`.

Zapoznaj się z plikiem [Global. asax](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472CSharpMVC5/Global.asax.cs) przykładem podłączania zdarzenia i [SameSiteCookieRewriter.cs](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472CSharpMVC5/SameSiteCookieRewriter.cs) , aby zapoznać się z przykładem obsługi zdarzenia i dostosowując plik cookie `sameSite` atrybut, który można skopiować do własnego kodu.

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

W ten sam sposób można zmienić określone zachowanie nazwanego pliku cookie. Poniższy przykład dostosowuje domyślny plik cookie uwierzytelniania z `Lax` do `None` w przeglądarkach obsługujących wartość `None` lub usuwa atrybut sameSite w przeglądarkach, które nie obsługują `None`.

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

### <a name="more-information"></a>Więcej informacji
 
[Aktualizacje programu Chrome](https://www.chromium.org/updates/same-site)

[Dokumentacja OWIN SameSite](/aspnet/samesite/owin-samesite)

[Dokumentacja ASP.NET](/aspnet/samesite/system-web-samesite)

[Poprawki programu .NET SameSite](/aspnet/samesite/kbs-samesite)
---
title: Przykład pliku cookie SameSite dla ASP.NET C# 4.7.2 WebForms
author: blowdart
description: Przykład pliku cookie SameSite dla ASP.NET C# 4.7.2 WebForms
ms.author: riande
ms.date: 2/15/2019
uid: samesite/CSharpWebForms
ms.openlocfilehash: 50d4745eca5954275abaa59dab726e7cf7ea193f
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/19/2020
ms.locfileid: "77458484"
---
# <a name="samesite-cookie-sample-for-aspnet-472-c-webforms"></a>Przykład pliku cookie SameSite dla ASP.NET C# 4.7.2 WebForms

.NET Framework 4,7 ma wbudowaną obsługę atrybutu [SameSite](https://www.owasp.org/index.php/SameSite) , ale jest zgodna z oryginalnym standardem.
Zachowanie poprawek zostało zmienione znaczenia `SameSite.None`, aby emitować atrybut o wartości `None`, zamiast wyemitować wartość w ogóle. Jeśli nie chcesz emitować wartości, możesz ustawić właściwość `SameSite` w pliku cookie na wartość-1.

## <a name="sampleCode"></a>Pisanie atrybutu SameSite

Poniżej przedstawiono przykład sposobu pisania atrybutu SameSite w pliku cookie;

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

Domyślny atrybut sameSite dla pliku cookie uwierzytelniania formularzy jest ustawiany w `cookieSameSite` parametr ustawień uwierzytelniania formularzy w `web.config` 

```xml
<system.web>
  <authentication mode="Forms">
    <forms name=".ASPXAUTH" loginUrl="~/" cookieSameSite="None" requireSSL="true">
    </forms>
  </authentication>
</system.web>
```

Domyślny atrybut sameSite stanu sesji jest również ustawiany w parametrze "cookieSameSite" ustawień sesji w `web.config`

```xml
<system.web>
  <sessionState cookieSameSite="None">     
  </sessionState>
</system.web>
```

Aktualizacja z listopada 2019 do platformy .NET zmieniła domyślne ustawienia uwierzytelniania formularzy i sesji na `lax` jak jest to najbezpieczniejsze ustawienie, jednak jeśli osadzasz strony w iframes, może być konieczne przywrócenie tego ustawienia do wartości none, a następnie dodanie kodu [przechwycenia](#interception) pokazanego poniżej w celu dostosowania zachowania `none` w zależności od możliwości przeglądarki.

### <a name="running-the-sample"></a>Uruchamianie przykładowej aplikacji

Po uruchomieniu przykładowego projektu Załaduj debuger przeglądarki na stronie początkowej i użyj go do wyświetlenia kolekcji plików cookie dla witryny.
Aby to zrobić, w przeglądarce Edge i Chrome naciśnij `F12` następnie wybierz kartę `Application` i kliknij adres URL witryny poniżej opcji `Cookies` w sekcji `Storage`.

![Lista plików cookie debugera przeglądarki](sample/img/BrowserDebugger.png)

Po kliknięciu przycisku "Utwórz pliki cookie" można zobaczyć, że w powyższym obrazie plik cookie został utworzony przez próbkę `Lax`SameSite, dopasowując wartość ustawioną w [przykładowym kodzie](#sampleCode).

## <a name="interception"></a>Przechwytywanie plików cookie, które nie są kontrolowane

W programie .NET 4.5.2 wprowadzono nowe zdarzenie dotyczące przechwytywania zapisu nagłówków, `Response.AddOnSendingHeaders`. Może to służyć do przechwytywania plików cookie przed ich zwróceniem do komputera klienckiego. W przykładzie tworzymy zdarzenie do metody statycznej, która sprawdza, czy przeglądarka obsługuje nowe zmiany sameSite, a jeśli nie, zmienia pliki cookie, aby nie wyemitować atrybutu, jeśli ustawiono nową wartość `None`.

Zapoznaj się z plikiem [Global. asax](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472CSharpWebForms/Global.asax.cs) przykładem, aby dowiedzieć się, jak podłączyć zdarzenie i [SameSiteCookieRewriter.cs](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472CSharpWebForms/SameSiteCookieRewriter.cs) na przykład obsłużyć zdarzenie i dostosować atrybut `sameSite` plików cookie.

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

## <a name="more-information"></a>Więcej informacji

[Aktualizacje programu Chrome](https://www.chromium.org/updates/same-site)

[Dokumentacja ASP.NET](/aspnet/samesite/system-web-samesite)

[Poprawki programu .NET SameSite](/aspnet/samesite/kbs-samesite)
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
# <a name="samesite-cookie-sample-for-aspnet-472-vb-mvc"></a>Przykład pliku cookie SameSite dla ASP.NET 4.7.2 VB MVC

.NET Framework 4,7 ma wbudowaną obsługę atrybutu [SameSite](https://www.owasp.org/index.php/SameSite) , ale jest zgodna z oryginalnym standardem.
Zachowanie poprawek zostało zmienione znaczenia `SameSite.None`, aby emitować atrybut o wartości `None`, zamiast wyemitować wartość w ogóle. Jeśli nie chcesz emitować wartości, możesz ustawić właściwość `SameSite` w pliku cookie na wartość-1.

## <a name="sampleCode"></a>Pisanie atrybutu SameSite

Poniżej przedstawiono przykład sposobu pisania atrybutu SameSite w pliku cookie;

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

Domyślny atrybut sameSite stanu sesji jest ustawiany w parametrze "cookieSameSite" ustawień sesji w `web.config`

```xml
<system.web>
  <sessionState cookieSameSite="None">     
  </sessionState>
</system.web>
```

## <a name="mvc-authentication"></a>Uwierzytelnianie MVC

Uwierzytelnianie oparte na plikach cookie OWIN MVC korzysta z Menedżera plików cookie, aby umożliwić zmianę atrybutów plików cookie. [SameSiteCookieManager. vb](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472VisualBasicMVC5/SameSiteCookieManager.vb) to implementacja klasy, którą można skopiować do własnych projektów. 

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

Składniki uwierzytelniania muszą być skonfigurowane do używania pliku Cookiemanager w klasie startowej.

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

Należy ustawić Menedżera plików cookie dla *każdego* składnika, który go obsługuje, obejmuje to CookieAuthentication i OpenIdConnectAuthentication.

SystemWebCookieManager jest używany w celu uniknięcia [znanych problemów](https://github.com/aspnet/AspNetKatana/wiki/System.Web-response-cookie-integration-issues) z integracją plików cookie odpowiedzi.

### <a name="running-the-sample"></a>Uruchamianie przykładowej aplikacji

W przypadku uruchomienia przykładowego projektu Załaduj debuger przeglądarki na stronie początkowej i użyj go, aby wyświetlić kolekcję plików cookie dla tej witryny.
Aby to zrobić, w przeglądarce Edge i Chrome naciśnij `F12` następnie wybierz kartę `Application` i kliknij adres URL witryny poniżej opcji `Cookies` w sekcji `Storage`.

![Lista plików cookie debugera przeglądarki](sample/img/BrowserDebugger.png)

Po kliknięciu przycisku "Utwórz SameSite cookie" można zobaczyć z obrazu powyżej, że plik cookie utworzony przez próbkę ma wartość `Lax`, dopasowując wartość ustawioną w [przykładowym kodzie](#sampleCode).

## <a name="interception"></a>Przechwytywanie plików cookie, które nie są kontrolowane

W programie .NET 4.5.2 wprowadzono nowe zdarzenie dotyczące przechwytywania zapisu nagłówków, `Response.AddOnSendingHeaders`. Może to służyć do przechwytywania plików cookie przed ich zwróceniem do komputera klienckiego. W przykładzie tworzymy zdarzenie do metody statycznej, która sprawdza, czy przeglądarka obsługuje nowe zmiany sameSite, a jeśli nie, zmienia pliki cookie, aby nie wyemitować atrybutu, jeśli ustawiono nową wartość `None`.

Zapoznaj się z plikiem [Global. asax](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472VisualBasicMVC5/Global.asax.vb) przykładem dotyczącym podłączania zdarzenia i [SameSiteCookieRewriter. vb](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472VisualBasicMVC5/SameSiteCookieRewriter.vb) , aby zapoznać się z przykładem obsługi zdarzenia i dostosowując plik cookie `sameSite` atrybutu, który można skopiować do własnego kodu.

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

W ten sam sposób można zmienić określone zachowanie nazwanego pliku cookie. Poniższy przykład dostosowuje domyślny plik cookie uwierzytelniania z `Lax` do `None` w przeglądarkach obsługujących wartość `None` lub usuwa atrybut sameSite w przeglądarkach, które nie obsługują `None`.

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

## <a name="more-information"></a>Więcej informacji
 
[Aktualizacje programu Chrome](https://www.chromium.org/updates/same-site)

[Dokumentacja OWIN SameSite](/aspnet/samesite/owin-samesite)

[Dokumentacja ASP.NET](/aspnet/samesite/system-web-samesite)

[Poprawki programu .NET SameSite](/aspnet/samesite/kbs-samesite)
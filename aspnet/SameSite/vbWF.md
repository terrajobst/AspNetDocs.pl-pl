---
title: Przykład pliku cookie SameSite dla formularzy ASP.NET 4.7.2 VB WebForms
author: blowdart
description: Przykład pliku cookie SameSite dla formularzy ASP.NET 4.7.2 VB WebForms
ms.author: riande
ms.date: 2/15/2019
uid: samesite/vbWF
ms.openlocfilehash: 8979edecc5acf7dac81b9f53d31af00389f4727c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78544997"
---
# <a name="samesite-cookie-sample-for-aspnet-472-vb-webforms"></a>Przykład pliku cookie SameSite dla formularzy ASP.NET 4.7.2 VB WebForms
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

Zapoznaj się z plikiem [Global. asax](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472VisualBasicWebForms/Global.asax.vb) przykładem, aby dowiedzieć się, jak podłączyć zdarzenie i [SameSiteCookieRewriter.cs](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472VisualBasicWebForms/SameSiteCookieRewriter.vb) na przykład obsłużyć zdarzenie i dostosować atrybut `sameSite` plików cookie.


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

[Dokumentacja ASP.NET](/aspnet/samesite/system-web-samesite)

[Poprawki programu .NET SameSite](/aspnet/samesite/kbs-samesite)
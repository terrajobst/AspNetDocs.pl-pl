---
uid: web-api/overview/security/forms-authentication
title: Uwierzytelnianie formularzy w interfejsie API sieci Web ASP.NET | Microsoft Docs
author: MikeWasson
description: Opisuje używanie uwierzytelniania formularzy w interfejsie Web API ASP.NET.
ms.author: riande
ms.date: 12/12/2012
ms.assetid: 9f06c1f2-ffaa-4831-94a0-2e4a3befdf07
msc.legacyurl: /web-api/overview/security/forms-authentication
msc.type: authoredcontent
ms.openlocfilehash: 147bfab76e48497f35a72b28cd935f40ec4193bf
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598596"
---
# <a name="forms-authentication-in-aspnet-web-api"></a>Uwierzytelnianie formularzy w interfejsie API sieci Web ASP.NET

według [Jan Wasson](https://github.com/MikeWasson)

Uwierzytelnianie formularzy używa formularza HTML do wysyłania poświadczeń użytkownika do serwera. Nie jest standardem internetowym. Uwierzytelnianie formularzy jest odpowiednie tylko w przypadku interfejsów API sieci Web, które są wywoływane z aplikacji sieci Web, dzięki czemu użytkownik może korzystać z formularza HTML.

| Zalety | Wady |
| --- | --- |
| — Łatwa implementacja: wbudowana w ASP.NET. -Korzysta z dostawcy członkostwa ASP.NET, co ułatwia zarządzanie kontami użytkowników. | -Nie jest standardowym mechanizmem uwierzytelniania HTTP; używa plików cookie protokołu HTTP zamiast standardowego nagłówka autoryzacji. -Wymaga klienta przeglądarki. -Poświadczenia są wysyłane w postaci zwykłego tekstu. -Narażony na fałszerstwo żądania między lokacjami (CSRF); wymaga środków antyCSRFowych. — Trudno korzystać z klientów nieobsługujących przeglądarki. Logowanie wymaga przeglądarki. — Poświadczenia użytkownika są wysyłane w żądaniu. — Niektórzy użytkownicy nie wyłączają plików cookie. |

Krótko, uwierzytelnianie formularzy w ASP.NET działa następująco:

1. Klient żąda zasobu wymagającego uwierzytelnienia.
2. Jeśli użytkownik nie jest uwierzytelniony, serwer zwróci HTTP 302 (znaleziono) i przekierowuje do strony logowania.
3. Użytkownik wprowadza poświadczenia i przesyła formularz.
4. Serwer zwraca kolejną wartość HTTP 302, która przekierowuje z powrotem do oryginalnego identyfikatora URI. Ta odpowiedź zawiera plik cookie uwierzytelniania.
5. Klient ponownie żąda zasobu. Żądanie zawiera plik cookie uwierzytelniania, aby serwer przyznał żądanie.

![](forms-authentication/_static/image1.png)

Aby uzyskać więcej informacji, zobacz [Omówienie uwierzytelniania formularzy.](../../../web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-cs.md)

## <a name="using-forms-authentication-with-web-api"></a>Korzystanie z uwierzytelniania formularzy przy użyciu interfejsu API sieci Web

Aby utworzyć aplikację, która korzysta z uwierzytelniania formularzy, wybierz szablon "aplikacja internetowa" w Kreatorze projektu MVC 4. Ten szablon tworzy kontrolery MVC do zarządzania kontami. Można również użyć szablonu "aplikacja jednostronicowa", dostępnego w ASP.NET 2012 Update.

W kontrolerach interfejsu API sieci Web można ograniczyć dostęp przy użyciu atrybutu `[Authorize]`, zgodnie z opisem w temacie [using the [autoryzuje] atrybutu](authentication-and-authorization-in-aspnet-web-api.md#auth3).

Formularze — uwierzytelnianie używa pliku cookie sesji do uwierzytelniania żądań. Przeglądarki automatycznie wysyłają wszystkie odpowiednie pliki cookie do docelowej witryny sieci Web. Ta funkcja sprawia, że uwierzytelnianie formularzy jest potencjalnie podatne na ataki z wykorzystaniem fałszerstwa żądań między lokacjami (CSRF [).](preventing-cross-site-request-forgery-csrf-attacks.md)

Uwierzytelnianie formularzy nie szyfruje poświadczeń użytkownika. W związku z tym uwierzytelnianie formularzy nie jest bezpieczne, chyba że jest używany z protokołem SSL. Zobacz [Praca z protokołem SSL w internetowym interfejsie API](working-with-ssl-in-web-api.md).

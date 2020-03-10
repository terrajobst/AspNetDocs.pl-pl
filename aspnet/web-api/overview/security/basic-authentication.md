---
uid: web-api/overview/security/basic-authentication
title: Uwierzytelnianie podstawowe w interfejsie API sieci Web ASP.NET | Microsoft Docs
author: MikeWasson
description: Opisuje używanie uwierzytelniania podstawowego w interfejsie Web API ASP.NET.
ms.author: riande
ms.date: 10/02/2014
ms.assetid: 41423767-0021-47c3-9e53-0021b457c39f
msc.legacyurl: /web-api/overview/security/basic-authentication
msc.type: authoredcontent
ms.openlocfilehash: 1470bd4b5abd5199b9a5105973b053812d643351
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78555728"
---
# <a name="basic-authentication-in-aspnet-web-api"></a>Uwierzytelnianie podstawowe w interfejsie API sieci Web ASP.NET

według [Jan Wasson](https://github.com/MikeWasson)

Uwierzytelnianie podstawowe jest zdefiniowane w [dokumencie RFC 2617, uwierzytelnianie http: uwierzytelnianie Basic i szyfrowanie dostępu szyfrowanego](http://www.ietf.org/rfc/rfc2617.txt).

Wady

- Poświadczenia użytkownika są wysyłane w żądaniu.
- Poświadczenia są wysyłane w postaci zwykłego tekstu.
- Poświadczenia są wysyłane przy użyciu każdego żądania.
- Nie ma możliwości wylogowania, z wyjątkiem zakończenia sesji przeglądarki.
- Podatne na fałszerstwo żądania między lokacjami (CSRF); wymaga środków antyCSRFowych.

Zalety

- Internet Standard.
- Obsługiwane przez wszystkie główne przeglądarki.
- Stosunkowo prosty protokół.

Uwierzytelnianie podstawowe działa w następujący sposób:

1. Jeśli żądanie wymaga uwierzytelnienia, serwer zwraca 401 (bez autoryzacji). Odpowiedź zawiera nagłówek WWW-Authenticate wskazujący, że serwer obsługuje uwierzytelnianie podstawowe.
2. Klient wysyła kolejne żądanie z poświadczeniami klienta w nagłówku autoryzacji. Poświadczenia są formatowane jako ciąg "Name: Password", kodowane algorytmem Base64. Poświadczenia nie są szyfrowane.

Uwierzytelnianie podstawowe jest wykonywane w kontekście "obszar". Serwer zawiera nazwę obszaru w nagłówku WWW-Authenticate. Poświadczenia użytkownika są prawidłowe w tym obszarze. Dokładny zakres obszaru jest definiowany przez serwer. Na przykład można zdefiniować kilka obszarów, aby podzielić zasoby na partycje.

![](basic-authentication/_static/image1.png)

Ponieważ poświadczenia są wysyłane jako nieszyfrowane, uwierzytelnianie podstawowe jest zabezpieczane tylko za pośrednictwem protokołu HTTPS. Zobacz [Praca z protokołem SSL w internetowym interfejsie API](working-with-ssl-in-web-api.md).

Uwierzytelnianie podstawowe jest również podatne na ataki CSRF. Po wprowadzeniu poświadczeń przez użytkownika przeglądarka automatycznie wysyła je na kolejne żądania do tej samej domeny, na czas trwania sesji. Obejmuje to żądania AJAX. Zapoznaj się z artykułem [zapobieganie atakom CSRFym między witrynami](preventing-cross-site-request-forgery-csrf-attacks.md).

## <a name="basic-authentication-with-iis"></a>Uwierzytelnianie podstawowe przy użyciu usług IIS

Usługi IIS obsługują uwierzytelnianie podstawowe, ale istnieje zastrzeżenie: użytkownik jest uwierzytelniany przy użyciu poświadczeń systemu Windows. Oznacza to, że użytkownik musi mieć konto w domenie serwera. W przypadku publicznej witryny sieci Web zwykle jest wymagane uwierzytelnienie względem dostawcy członkostwa ASP.NET.

Aby włączyć uwierzytelnianie podstawowe przy użyciu usług IIS, ustaw tryb uwierzytelniania na "Windows" w pliku Web. config projektu ASP.NET:

[!code-xml[Main](basic-authentication/samples/sample1.xml)]

W tym trybie usługi IIS uwierzytelniają się przy użyciu poświadczeń systemu Windows. Ponadto należy włączyć uwierzytelnianie podstawowe w usługach IIS. W Menedżerze usług IIS przejdź do opcji Widok funkcji, wybierz pozycję Uwierzytelnianie i Włącz uwierzytelnianie podstawowe.

![](basic-authentication/_static/image2.png)

W projekcie interfejsu API sieci Web Dodaj atrybut `[Authorize]` dla wszystkich akcji kontrolera, które wymagają uwierzytelniania.

Klient uwierzytelnia się za pomocą ustawienia nagłówka autoryzacji w żądaniu. Klienci przeglądarki wykonują ten krok automatycznie. Klienci niebędący przeglądarką muszą ustawić nagłówek.

## <a name="basic-authentication-with-custom-membership"></a>Uwierzytelnianie podstawowe z członkostwem niestandardowym

Jak wspomniano, uwierzytelnianie podstawowe wbudowane w usługi IIS używa poświadczeń systemu Windows. Oznacza to, że należy utworzyć konta dla użytkowników na serwerze hostingu. Jednak w przypadku aplikacji internetowej konta użytkowników są zwykle przechowywane w zewnętrznej bazie danych.

Poniższy kod, jak moduł HTTP wykonujący uwierzytelnianie podstawowe. Możesz łatwo podłączyć dostawcę członkostwa ASP.NET, zastępując metodę `CheckPassword`, która jest metodą fikcyjną w tym przykładzie.

W przypadku interfejsu Web API 2 należy rozważyć zapisanie [filtru uwierzytelniania](authentication-filters.md) lub [oprogramowania pośredniczącego Owin](../../../aspnet/overview/owin-and-katana/index.md)zamiast modułu http.

[!code-csharp[Main](basic-authentication/samples/sample2.cs)]

Aby włączyć moduł HTTP, Dodaj następujący plik do pliku Web. config w sekcji **System. WebServer** :

[!code-xml[Main](basic-authentication/samples/sample3.xml?highlight=4)]

Zastąp ciąg "YourAssemblyName" nazwą zestawu (bez rozszerzenia "dll").

Należy wyłączyć inne schematy uwierzytelniania, takie jak formularze lub uwierzytelnianie systemu Windows.

---
uid: web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks
title: Zapobieganie atakom w ramach żądania krzyżowego (CSRF) w ASP.NET MVC
author: MikeWasson
description: Opisuje atak między lokacjami (CSRF) i sposób wdrażania miar antyCSRFowych w ASP.NET Web MVC.
ms.author: riande
ms.date: 12/12/2012
ms.assetid: 81d46f14-8f48-4d8c-830d-cc8d594dc11b
msc.legacyurl: /web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks
msc.type: authoredcontent
ms.openlocfilehash: 5fb0f8bcc9e587ba4fbbf2b857d3bf7adcaafb94
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78555119"
---
# <a name="preventing-cross-site-request-forgery-csrf-attacks-in-aspnet-mvc-application"></a>Zapobieganie atakom przez wiele witryn (CSRF) w aplikacji ASP.NET MVC

według [Jan Wasson](https://github.com/MikeWasson)

Fałszerstwo żądania między lokacjami (CSRF) to atak polegający na tym, że złośliwa witryna wysyła żądanie do zagrożonej lokacji, w której użytkownik jest aktualnie zalogowany.

Oto przykład ataku CSRF:

1. Użytkownik loguje się do `www.example.com` przy użyciu uwierzytelniania formularzy.
2. Serwer uwierzytelnia użytkownika. Odpowiedź z serwera zawiera plik cookie uwierzytelniania.
3. Bez wylogowywania użytkownik odwiedza złośliwą witrynę sieci Web. Ta złośliwa witryna zawiera następującą postać HTML: 

    [!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample1.html)]

    Należy zauważyć, że w ramach akcji formularza są umieszczane wpisy w zagrożonej witrynie, a nie w złośliwej witrynie. To jest część "wiele witryn" z CSRF.
4. Użytkownik klika przycisk Prześlij. Przeglądarka zawiera plik cookie uwierzytelniania z żądaniem.
5. Żądanie jest uruchamiane na serwerze z kontekstem uwierzytelniania użytkownika i może wykonywać wszystkie czynności, które mogą wykonać uwierzytelnionego użytkownika.

Mimo że ten przykład wymaga, aby użytkownik klikał przycisk formularza, złośliwa strona może tak jak łatwo uruchomić skrypt, który przesyła formularz automatycznie. Ponadto użycie protokołu SSL nie uniemożliwia ataku CSRF, ponieważ złośliwa witryna może wysłać żądanie "https://".

Zazwyczaj ataki CSRF są dostępne dla witryn sieci Web, które używają plików cookie do uwierzytelniania, ponieważ przeglądarki wysyłają wszystkie odpowiednie pliki cookie do docelowej witryny sieci Web. Jednak ataki CSRF nie ograniczają się do korzystania z plików cookie. Na przykład uwierzytelnianie podstawowe i szyfrowane jest również zagrożone. Po zalogowaniu się użytkownika przy użyciu uwierzytelniania podstawowego lub szyfrowanego. Przeglądarka automatycznie wysyła poświadczenia do momentu zakończenia sesji.

## <a name="anti-forgery-tokens"></a>Tokeny zabezpieczające przed fałszerstwem

Aby zapobiec atakom CSRF, ASP.NET MVC używa tokenów chroniących przed fałszerstwem, nazywanych również *tokenami weryfikacji żądań*.

1. Klient żąda strony HTML zawierającej formularz.
2. Serwer zawiera dwa tokeny w odpowiedzi. Jeden token jest wysyłany jako plik cookie. Druga zostanie umieszczona w ukrytym polu formularza. Tokeny są generowane losowo, dzięki czemu element atakującej nie może odgadnąć wartości.
3. Gdy klient prześle formularz, musi wysłać oba tokeny z powrotem do serwera. Klient wysyła token cookie jako plik cookie i wysyła token formularza wewnątrz danych formularza. (Klient przeglądarki automatycznie robi to podczas przesyłania formularza przez użytkownika).
4. Jeśli żądanie nie zawiera obu tokenów, serwer nie zezwala na żądanie.

Oto przykład formularza HTML z tokenem ukrytego formularza:

[!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample2.html)]

Tokeny chroniące przed fałszerstwem działają, ponieważ złośliwa strona nie może odczytać tokenów użytkownika ze względu na zasady tego samego źródła. ([Zasady tego samego pochodzenia](http://www.w3.org/Security/wiki/Same_Origin_Policy) uniemożliwiają dokumentom hostowanym w dwóch różnych lokacjach dostęp do każdej z nich. Tak więc w poprzednim przykładzie złośliwa strona może wysyłać żądania do example.com, ale nie może odczytać odpowiedzi.

Aby zapobiec atakom CSRF, należy użyć tokenów chroniących przed fałszerstwem przy użyciu dowolnego protokołu uwierzytelniania, w którym przeglądarka w trybie dyskretnym wysyła poświadczenia po zalogowaniu się użytkownika. Dotyczy to protokołów uwierzytelniania opartych na plikach cookie, takich jak uwierzytelnianie formularzy, a także protokołów takich jak uwierzytelnianie podstawowe i szyfrowane.

Należy wymagać, aby tokeny zabezpieczające przed fałszowaniem były stosowane do wszelkich metod niebezpiecznych (POST, PUT i DELETE). Upewnij się również, że metody bezpieczne (GET, główna) nie mają żadnych efektów ubocznych. Ponadto, jeśli włączysz obsługę międzydomenową, taką jak CORS lub JSONP, to nawet bezpieczne metody, takie jak GET, potencjalnie narażony na ataki CSRF, umożliwiając atakującemu odczytywanie potencjalnie poufnych danych.

## <a name="anti-forgery-tokens-in-aspnet-mvc"></a>Tokeny chroniące przed fałszowaniem w ASP.NET MVC

Aby dodać tokeny chroniące przed fałszerstwem do strony Razor, użyj metody pomocnika **HtmlHelper. AntiForgeryToken** :

[!code-cshtml[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample3.cshtml)]

Ta metoda dodaje ukryte pole formularza, a także ustawia token cookie.

## <a name="anti-csrf-and-ajax"></a>Anti-CSRF i AJAX

Token formularza może stanowić problem z żądaniami AJAX, ponieważ żądanie AJAX może wysyłać dane JSON, a nie dane formularza HTML. Jednym z rozwiązań jest wysłanie tokenów w niestandardowym nagłówku HTTP. Poniższy kod używa składnia Razor do wygenerowania tokenów, a następnie dodaje tokeny do żądania AJAX. Tokeny są generowane na serwerze przez wywołanie funkcji " **Gettokens**".

[!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample4.html)]

Podczas przetwarzania żądania Wyodrębnij tokeny z nagłówka żądania. Następnie Wywołaj metodę **Unfałszerstwa. Validate** , aby sprawdzić poprawność tokenów. Metoda **Validate** zgłasza wyjątek, jeśli tokeny są nieprawidłowe.

[!code-csharp[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample5.cs)]

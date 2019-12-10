---
title: Pracuj z plikami cookie SameSite i otwartym interfejsem sieci Web dla platformy .NET (OWIN)
author: rick-anderson
description: Pracuj z plikami cookie SameSite i otwartym interfejsem sieci Web dla platformy .NET (OWIN)
ms.author: riande
ms.date: 12/6/2019
uid: owin-samesite
ms.openlocfilehash: ac5ae24eeb9e8e1cc6296667a4bebef72c3eb62c
ms.sourcegitcommit: 7b1e1784213dd4c301635f9e181764f3e2f94162
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/10/2019
ms.locfileid: "74993078"
---
# <a name="samesite-cookies-and-the-open-web-interface-for-net-owin"></a>Pliki cookie SameSite i otwarty interfejs sieci Web dla platformy .NET (OWIN)

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

`SameSite` jest projektem [IETF](https://ietf.org/about/) opracowaną w celu zapewnienia pewnej ochrony przed ATAKami CSRFymi między lokacjami. [Wersja robocza SameSite 2019](https://tools.ietf.org/html/draft-west-cookie-incrementalism-00):

* Domyślnie traktuje pliki cookie jako `SameSite=Lax`.
* Stany plików cookie, które jawnie potwierdzają `SameSite=None` w celu włączenia dostarczania między lokacjami, powinny być oznaczone jako `Secure`.

`Lax` działa w przypadku większości plików cookie aplikacji. Niektóre formy uwierzytelniania, takie jak [OpenID Connect Connect](https://openid.net/connect/) (OIDC) i [WS-Federation](https://auth0.com/docs/protocols/ws-fed) , domyślnie mogą publikować przekierowania na podstawie. Przekierowania oparte na WPISach wyzwalają ochronę za pomocą przeglądarki `SameSite`, więc `SameSite` jest wyłączone dla tych składników. Większość logowań [uwierzytelniania OAuth](https://oauth.net/) nie ma wpływu z powodu różnic między przepływem żądania. Wszystkie inne składniki **nie** są domyślnie ustawiane `SameSite` i używają domyślnego zachowania klientów (stary lub nowy).

`None` parametr powoduje problemy ze zgodnością z klientami, którzy zaimplementowali poprzednią [wersję standard 2016](https://tools.ietf.org/html/draft-west-first-party-cookies-07) (na przykład system iOS 12). Zobacz [Obsługa starszych przeglądarek](#sob) w tym dokumencie.

Każdy składnik OWIN, który emituje pliki cookie, musi zdecydować, czy `SameSite` jest odpowiednie.

Aby uzyskać wersję ASP.NET 4. x tego artykułu, zobacz <xref:samesite/system-web-samesite>.

## <a name="api-usage-with-samesite"></a>Użycie interfejsu API z SameSite

`Microsoft.Owin` ma własną `SameSite` implementację:

* To nie zależy bezpośrednio od tego, w `System.Web`.
* `SameSite` działa we wszystkich wersjach docelowych pakietów `Microsoft.Owin`, .NET 4,5 i nowszych.
* Tylko składnik [SystemWebCookieManager](https://github.com/aspnet/AspNetKatana/blob/dev/src/Microsoft.Owin.Host.SystemWeb/SystemWebCookieManager.cs) bezpośrednio współdziała z klasą `HttpCookie` `System.Web`.

`SystemWebCookieManager` zależy od interfejsów API programu .NET 4.7.2 `System.Web` w celu włączenia obsługi `SameSite` oraz poprawek w celu zmiany zachowania.

Przyczyny używania `SystemWebCookieManager` są opisane w temacie [problemy z integracją plików cookie z odpowiedzią na Owin i system. Web](https://github.com/aspnet/AspNetKatana/wiki/System.Web-response-cookie-integration-issues). `SystemWebCookieManager` jest zalecana w przypadku uruchamiania na `System.Web`.

Poniższy kod ustawia `SameSite`, aby `Lax`:

```csharp
owinContext.Response.Cookies.Append("My Key", "My Value", new CookieOptions()
{
    SameSite = SameSiteMode.Lax
});
```

Poniższe interfejsy API używają `SameSite`:

* [Microsoft. Owin. SameSiteMode](https://github.com/aspnet/AspNetKatana/blob/dev/src/Microsoft.Owin/SameSiteMode.cs)
* [CookieOptions.SameSite](xref:Microsoft.AspNetCore.Http.CookieOptions.SameSite)
* [Klasa CookieAuthenticationOptions](/previous-versions/aspnet/dn385599(v%3Dvs.113)) <!-- CookieAuthenticationOptions.CookieSameSite not published -->
* [CookieAuthenticationOptions.CookieSameSite](https://github.com/aspnet/AspNetKatana/blob/dev/src/Microsoft.Owin.Security.Cookies/CookieAuthenticationOptions.cs#L68-#L72)
* [ICookieManager](/previous-versions/aspnet/dn800238(v%3Dvs.113))
* [SystemWebCookieManager](https://github.com/aspnet/AspNetKatana/blob/dev/src/Microsoft.Owin.Host.SystemWeb/SystemWebCookieManager.cs)
* [SystemWebChunkingCookieManager](https://github.com/aspnet/AspNetKatana/blob/dev/src/Microsoft.Owin.Host.SystemWeb/SystemWebChunkingCookieManager.cs)
* [CookieAuthenticationOptions. Cookiemanager](https://github.com/aspnet/AspNetKatana/blob/dev/src/Microsoft.Owin.Security.Cookies/CookieAuthenticationOptions.cs#L143-#AL148)
* [OpenIdConnectAuthenticationOptions. Cookiemanager](https://github.com/aspnet/AspNetKatana/blob/dev/src/Microsoft.Owin.Security.OpenIdConnect/OpenIdConnectAuthenticationOptions.cs#L315-#L318)

## <a name="history-and-changes"></a>Historia i zmiany

[Pakiet Microsoft. Owin](https://www.nuget.org/packages/Microsoft.Owin/) nigdy nie obsługuje [wersji standardu`SameSite` 2016](https://tools.ietf.org/html/draft-west-first-party-cookies-07#section-4.1).

Obsługa [wersji roboczej SameSite 2019](https://tools.ietf.org/html/draft-west-cookie-incrementalism-00) jest dostępna tylko w `Microsoft.Owin` 4.1.0 i nowszych. Brak poprawek dla wcześniejszych wersji.

Wersja robocza 2019 specyfikacji `SameSite`:

* **Nie** jest wstecznie zgodne z wersją roboczą 2016. Aby uzyskać więcej informacji, zobacz [Obsługa starszych przeglądarek](#sob) w tym dokumencie.
* Określa, że pliki cookie są domyślnie traktowane jako `SameSite=Lax`.
* Określa pliki cookie, które jawnie potwierdzają `SameSite=None` w celu włączenia dostarczania między lokacjami, powinny być oznaczone jako `Secure`. `None` to nowy wpis do rezygnacji.
* Zaplanowano włączenie programu [Chrome](https://chromestatus.com/feature/5088147346030592) domyślnie w [lutym 2020](https://blog.chromium.org/2019/10/developers-get-ready-for-new.html). Przeglądarki zaczynają przechodzenie do tego standardu w 2019.
* Jest obsługiwane przez poprawki wydane zgodnie z opisem w artykule bazy wiedzy. Aby uzyskać więcej informacji, zobacz temat <xref:samesite/kbs-samesite>.

<a name="sob"></a>

## <a name="supporting-older-browsers"></a>Obsługa starszych przeglądarek

W standardzie 2016 `SameSite` jest przyznany, że nieznane wartości muszą być traktowane jako `SameSite=Strict` wartości. Aplikacje dostępne ze starszych przeglądarek, które obsługują Standard 2016 `SameSite`, mogą ulec przerwie, gdy pobierają Właściwość `SameSite` o wartości `None`. Aplikacje sieci Web muszą implementować wykrywanie przeglądarki, jeśli chcą obsługiwać starsze przeglądarki. ASP.NET nie implementuje wykrywania przeglądarki, ponieważ wartości agentów użytkownika są bardzo nietrwałe i często zmieniają się. Punkt rozszerzenia w [ICookieManager](/previous-versions/aspnet/dn800238(v%3Dvs.113)) umożliwia podłączanie w ramach logiki specyficznej dla agenta użytkownika.
<!-- https://docs.microsoft.com/en-us/previous-versions/aspnet/dn800238(v%3Dvs.113) -->

W `Startup.Configuration`Dodaj kod podobny do poniższego:

[!code-csharp[](sample/Startup1.cs?name=snippet)]

Poprzedzający kod wymaga programu .NET 4.7.2 lub nowszego `SameSite` poprawka.

Poniższy kod przedstawia przykład implementacji `SameSiteCookieManager`:

[!code-csharp[](sample/SameSiteCookieManager.cs?name=snippet)]

W poprzednim przykładzie `DisallowsSameSiteNone` jest wywoływana w `CheckSameSite` metodzie. `DisallowsSameSiteNone` to metoda użytkownika, która wykrywa, czy agent użytkownika nie obsługuje `SameSite` `None`:

[!code-csharp[](sample/SameSiteCookieManager.cs?name=snippet3&highlight=4)]

Poniższy kod przedstawia przykładową metodę `DisallowsSameSiteNone`:

> [!WARNING]
> Poniższy kod służy tylko do celów demonstracyjnych:
> * Nie powinna być uważana za ukończoną.
> * Nie jest on obsługiwany ani wspierany.

[!code-csharp[](sample/SameSiteCookieManager.cs?name=snippet2)]

## <a name="test-apps-for-samesite-problems"></a>Testowanie aplikacji pod kątem problemów z SameSite

Aplikacje, które współdziałają z witrynami zdalnymi, takie jak logowanie innych firm, muszą:

* Przetestuj interakcję w wielu przeglądarkach.
* Zastosuj [wykrywanie przeglądarki i środki zaradcze](#sob) omówione w tym dokumencie.

Przetestuj aplikacje sieci Web przy użyciu wersji klienta, która może nawiązać zgodę na nowe zachowanie `SameSite`. Przeglądarki Chrome, Firefox i chrom Edge mają nowe flagi funkcji opt, których można użyć do testowania. Gdy aplikacja zastosuje `SameSite` poprawki, przetestuj ją ze starszymi wersjami klienta, szczególnie Safari. Aby uzyskać więcej informacji, zobacz [Obsługa starszych przeglądarek](#sob) w tym dokumencie.

### <a name="test-with-chrome"></a>Testowanie za pomocą przeglądarki Chrome

Chrome 78 + daje mylące wyniki, ponieważ ma tymczasowe środki zaradcze. Chrom 78 + tymczasowe środki zaradcze dopuszczają pliki cookie mniejsze niż dwie minuty. Program Chrome 76 lub 77 z włączonymi odpowiednimi flagami testu zapewnia dokładniejsze wyniki. Aby przetestować nowe zachowanie `SameSite` Przełącz `chrome://flags/#same-site-by-default-cookies` na **włączone**. Starsze wersje programu Chrome (75 i poniżej) zostały zgłoszone w celu niepowodzenia z nowym ustawieniem `None`. Zobacz [Obsługa starszych przeglądarek](#sob) w tym dokumencie.

Firma Google nie udostępnia starszych wersji programu Chrome. Postępuj zgodnie z instrukcjami podanymi w części [pobieranie chromu](https://www.chromium.org/getting-involved/download-chromium) , aby przetestować starsze wersje programu Chrome. **Nie** Pobieraj programu Chrome z linków dostarczonych przez wyszukiwanie starszych wersji programu Chrome.

* [Chrom 76 Win64](https://commondatastorage.googleapis.com/chromium-browser-snapshots/index.html?prefix=Win_x64/664998/)
* [Chrom 74 Win64](https://commondatastorage.googleapis.com/chromium-browser-snapshots/index.html?prefix=Win_x64/638880/)

### <a name="test-with-safari"></a>Testowanie przy użyciu przeglądarki Safari

Przeglądarka Safari 12 ściśle wdrożyła poprzednią wersję roboczą i kończy się niepowodzeniem, gdy nowa wartość `None` jest w pliku cookie. `None` jest możliwe za pośrednictwem kodu wykrywania przeglądarki [obsługującego starsze przeglądarki](#sob) w tym dokumencie. Przetestuj logowanie za pomocą przeglądarki Safari 12, Safari 13 i WebKit opartej na programie MSAL, ADAL lub niezależnie od używanej biblioteki. Problem jest zależny od podstawowej wersji systemu operacyjnego. OSX Mojave (10,14) i iOS 12 są znane jako problemy ze zgodnością z nowym zachowaniem `SameSite`. Uaktualnienie systemu operacyjnego do wersji OSX Catalina (10,15) lub iOS 13 rozwiązuje problem. Przeglądarka Safari nie ma obecnie flagi zgody na testowanie nowych zachowań specyfikacji.

### <a name="test-with-firefox"></a>Testowanie za pomocą przeglądarki Firefox

Obsługę programu Firefox dla nowego standardu można przetestować w wersji 68 + przez wypróbowanie na stronie `about:config` z flagą funkcji `network.cookie.sameSite.laxByDefault`. Nie zgłoszono problemów ze zgodnością ze starszymi wersjami programu Firefox.

### <a name="test-with-edge-browser"></a>Testowanie przy użyciu przeglądarki Edge

Program Edge obsługuje stary Standard `SameSite`. Wersja brzegowa 44 nie ma żadnych znanych problemów ze zgodnością z nowym standardem.

### <a name="test-with-edge-chromium"></a>Testowanie przy użyciu krawędzi (chrom)

na stronie `edge://flags/#same-site-by-default-cookies` są ustawione flagi `SameSite`. Nie odnaleziono problemów ze zgodnością z funkcją chromu.

### <a name="test-with-electron"></a>Testuj przy użyciu elektronu

Wersje elektronów obejmują starsze wersje chromu. Na przykład wersja elektronów używana przez zespoły to chrom 66, który wykazuje starsze zachowanie. Należy przeprowadzić własne testy zgodności z wersją elektronów używaną przez produkt. Zapoznaj się z tematem [Obsługa starszych przeglądarek](#sob) w następnej sekcji.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Blog chromu: deweloperzy: przygotowanie do nowego SameSite = none; Ustawienia bezpiecznego pliku cookie](https://blog.chromium.org/2019/10/developers-get-ready-for-new.html)
* [Wyjaśniono pliki cookie SameSite](https://web.dev/samesite-cookies-explained/)
* [Problemy z integracją plików cookie OWIN i system. Web Response](https://github.com/aspnet/AspNetKatana/wiki/System.Web-response-cookie-integration-issues)

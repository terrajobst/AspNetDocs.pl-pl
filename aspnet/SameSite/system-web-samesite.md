---
title: Pracuj z plikami cookie SameSite w ASP.NET
author: rick-anderson
description: Dowiedz się, jak używać programu do SameSite plików cookie w ASP.NET
ms.author: riande
ms.date: 1/22/2019
uid: samesite/system-web-samesite
ms.openlocfilehash: d2160bd9aeb93398b49b3a0e5e7a8a4404a5bc63
ms.sourcegitcommit: 88fc80e3f65aebdf61ec9414810ddbc31c543f04
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/22/2020
ms.locfileid: "76519196"
---
# <a name="work-with-samesite-cookies-in-aspnet"></a>Pracuj z plikami cookie SameSite w ASP.NET

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

SameSite jest projektem [IETF](https://ietf.org/about/) opracowanym w celu zapewnienia pewnej ochrony przed atakami na żądania bezlokacjowe (CSRF). [Wersja robocza SameSite 2019](https://tools.ietf.org/html/draft-west-cookie-incrementalism-00):

* Domyślnie traktuje pliki cookie jako `SameSite=Lax`.
* Stany plików cookie, które jawnie potwierdzają `SameSite=None` w celu włączenia dostarczania między lokacjami, powinny być oznaczone jako `Secure`.

`Lax` działa w przypadku większości plików cookie aplikacji. Niektóre formy uwierzytelniania, takie jak [OpenID Connect Connect](https://openid.net/connect/) (OIDC) i [WS-Federation](https://auth0.com/docs/protocols/ws-fed) , domyślnie mogą publikować przekierowania na podstawie. Przekierowania na podstawie wpisu wyzwalają ochronę za pomocą przeglądarki SameSite, więc SameSite jest wyłączona dla tych składników. Nie ma to wpływ na większość nazw logowania [OAuth](https://oauth.net/) z powodu różnic w sposobie przepływu żądania.

`None` parametr powoduje problemy ze zgodnością z klientami, którzy zaimplementowali poprzednią [wersję standard 2016](https://tools.ietf.org/html/draft-west-first-party-cookies-07) (na przykład system iOS 12). Zobacz [Obsługa starszych przeglądarek](#sob) w tym dokumencie.

Każdy składnik ASP.NET, który emituje pliki cookie, musi zdecydować, czy SameSite jest odpowiedni.

## <a name="api-usage-with-samesite"></a>Użycie interfejsu API z SameSite

Zobacz [Właściwość HttpCookie. SameSite](/dotnet/api/system.web.httpcookie.samesite#System_Web_HttpCookie_SameSite)

## <a name="history-and-changes"></a>Historia i zmiany

Obsługa SameSite została najpierw zaimplementowana w programie .NET 4.7.2 przy użyciu [standardowego standardu 2016](https://tools.ietf.org/html/draft-west-first-party-cookies-07#section-4.1).

19 listopada 2019 aktualizacje dla systemu Windows Zaktualizowano .NET 4.7.2 + ze standardu 2016 do standardu 2019. Dodatkowe aktualizacje są nachodzące dla innych wersji systemu Windows. Aby uzyskać więcej informacji, zobacz temat <xref:samesite/kbs-samesite>.

 Wersja robocza 2019 specyfikacji SameSite:

* **Nie** jest wstecznie zgodne z wersją roboczą 2016. Aby uzyskać więcej informacji, zobacz [Obsługa starszych przeglądarek](#sob) w tym dokumencie.
* Określa, że pliki cookie są domyślnie traktowane jako `SameSite=Lax`.
* Określa pliki cookie, które jawnie potwierdzają `SameSite=None` w celu włączenia dostarczania między lokacjami, powinny być oznaczone jako `Secure`. `None` to nowy wpis do rezygnacji.
* Jest obsługiwane przez poprawki wydane zgodnie z opisem w powyższej bazie wiedzy.
* Zaplanowano włączenie programu [Chrome](https://chromestatus.com/feature/5088147346030592) domyślnie w [lutym 2020](https://blog.chromium.org/2019/10/developers-get-ready-for-new.html). Przeglądarki zaczynają przechodzenie do tego standardu w 2019.

### <a name="azure-app-servicesamesite-cookie-handling"></a>Azure App Service — obsługa plików cookie SameSite

Aby uzyskać więcej informacji, zobacz [Azure App Service — obsługa plików cookie SameSite i .NET Framework Poprawka 4.7.2](https://azure.microsoft.com/updates/app-service-samesite-cookie-update/) .

<a name="sob"></a>

## <a name="supporting-older-browsers"></a>Obsługa starszych przeglądarek

Standard 2016 SameSite, że nieznane wartości muszą być traktowane jako wartości `SameSite=Strict`. Aplikacje dostępne ze starszych przeglądarek, które obsługują Standard 2016 SameSite, mogą ulec przerwie, gdy pobierają Właściwość SameSite o wartości `None`. Aplikacje sieci Web muszą implementować wykrywanie przeglądarki, jeśli chcą obsługiwać starsze przeglądarki. ASP.NET nie implementuje wykrywania przeglądarki, ponieważ wartości agentów użytkownika są bardzo nietrwałe i często zmieniają się. Następujący kod można wywołać w witrynie wywołania <xref:HTTP.HttpCookie>:

[!code-csharp[](sample/SameSiteCheck.cs?name=snippet)]

W powyższym przykładzie `MyUserAgentDetectionLib.DisallowsSameSiteNone` jest biblioteką dostarczoną przez użytkownika, która wykrywa, czy agent użytkownika nie obsługuje SameSite `None`. Poniższy kod przedstawia przykładową metodę `DisallowsSameSiteNone`:

> [!WARNING]
> Poniższy kod służy tylko do celów demonstracyjnych:
> * Nie powinna być uważana za ukończoną.
> * Nie jest on obsługiwany ani wspierany.

[!code-csharp[](sample/SameSiteCheck.cs?name=snippet2)]

## <a name="test-apps-for-samesite-problems"></a>Testowanie aplikacji pod kątem problemów z SameSite

Aplikacje, które współdziałają z witrynami zdalnymi, takie jak logowanie innych firm, muszą:

* Przetestuj interakcję w wielu przeglądarkach.
* Zastosuj [wykrywanie przeglądarki i środki zaradcze](#sob) omówione w tym dokumencie.

Przetestuj aplikacje sieci Web przy użyciu wersji klienta, która może być zgodą na nowe zachowanie SameSite. Przeglądarki Chrome, Firefox i chrom Edge mają nowe flagi funkcji opt, których można użyć do testowania. Po zastosowaniu przez aplikację SameSite poprawek przetestuj ją ze starszymi wersjami klienta, szczególnie Safari. Aby uzyskać więcej informacji, zobacz [Obsługa starszych przeglądarek](#sob) w tym dokumencie.

### <a name="test-with-chrome"></a>Testowanie za pomocą przeglądarki Chrome

Chrome 78 + daje mylące wyniki, ponieważ ma tymczasowe środki zaradcze. Chrom 78 + tymczasowe środki zaradcze dopuszczają pliki cookie mniejsze niż dwie minuty. Program Chrome 76 lub 77 z włączonymi odpowiednimi flagami testu zapewnia dokładniejsze wyniki. Aby przetestować nowe zachowanie SameSite, przełącz `chrome://flags/#same-site-by-default-cookies` na **włączone**. Starsze wersje programu Chrome (75 i poniżej) zostały zgłoszone w celu niepowodzenia z nowym ustawieniem `None`. Zobacz [Obsługa starszych przeglądarek](#sob) w tym dokumencie.

Firma Google nie udostępnia starszych wersji programu Chrome. Postępuj zgodnie z instrukcjami podanymi w części [pobieranie chromu](https://www.chromium.org/getting-involved/download-chromium) , aby przetestować starsze wersje programu Chrome. **Nie** Pobieraj programu Chrome z linków dostarczonych przez wyszukiwanie starszych wersji programu Chrome.

* [Chrom 76 Win64](https://commondatastorage.googleapis.com/chromium-browser-snapshots/index.html?prefix=Win_x64/664998/)
* [Chrom 74 Win64](https://commondatastorage.googleapis.com/chromium-browser-snapshots/index.html?prefix=Win_x64/638880/)

### <a name="test-with-safari"></a>Testowanie przy użyciu przeglądarki Safari

Przeglądarka Safari 12 ściśle wdrożyła poprzednią wersję roboczą i kończy się niepowodzeniem, gdy nowa wartość `None` jest w pliku cookie. `None` jest możliwe za pośrednictwem kodu wykrywania przeglądarki [obsługującego starsze przeglądarki](#sob) w tym dokumencie. Przetestuj logowanie za pomocą przeglądarki Safari 12, Safari 13 i WebKit opartej na programie MSAL, ADAL lub niezależnie od używanej biblioteki. Problem jest zależny od podstawowej wersji systemu operacyjnego. OSX Mojave (10,14) i iOS 12 są znane jako problemy ze zgodnością z nowym zachowaniem SameSite. Uaktualnienie systemu operacyjnego do wersji OSX Catalina (10,15) lub iOS 13 rozwiązuje problem. Przeglądarka Safari nie ma obecnie flagi zgody na testowanie nowych zachowań specyfikacji.

### <a name="test-with-firefox"></a>Testowanie za pomocą przeglądarki Firefox

Obsługę programu Firefox dla nowego standardu można przetestować w wersji 68 + przez wypróbowanie na stronie `about:config` z flagą funkcji `network.cookie.sameSite.laxByDefault`. Nie zgłoszono problemów ze zgodnością ze starszymi wersjami programu Firefox.

### <a name="test-with-edge-browser"></a>Testowanie przy użyciu przeglądarki Edge

Program Edge obsługuje stary Standard SameSite. Wersja brzegowa 44 nie ma żadnych znanych problemów ze zgodnością z nowym standardem.

### <a name="test-with-edge-chromium"></a>Testowanie przy użyciu krawędzi (chrom)

Flagi SameSite są ustawiane na stronie `edge://flags/#same-site-by-default-cookies`. Nie odnaleziono problemów ze zgodnością z funkcją chromu.

### <a name="test-with-electron"></a>Testuj przy użyciu elektronu

Wersje elektronów obejmują starsze wersje chromu. Na przykład wersja elektronów używana przez zespoły to chrom 66, który wykazuje starsze zachowanie. Należy przeprowadzić własne testy zgodności z wersją elektronów używaną przez produkt. Zapoznaj się z tematem [Obsługa starszych przeglądarek](#sob) w następnej sekcji.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Blog chromu: deweloperzy: przygotowanie do nowego SameSite = none; Ustawienia bezpiecznego pliku cookie](https://blog.chromium.org/2019/10/developers-get-ready-for-new.html)
* [Wyjaśniono pliki cookie SameSite](https://web.dev/samesite-cookies-explained/)

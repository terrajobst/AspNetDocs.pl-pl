---
title: Pracuj z plikami cookie SameSite w ASP.NET
author: rick-anderson
description: Dowiedz się, jak używać programu do SameSite plików cookie w ASP.NET
ms.author: riande
ms.date: 1/22/2019
uid: samesite/system-web-samesite
ms.openlocfilehash: c262e300361f33621e8bd126a34b251c23f56e1a
ms.sourcegitcommit: 6bd0d7581ec36dc32cb85d0d5fc0e51068dd4423
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/14/2020
ms.locfileid: "77234765"
---
# <a name="work-with-samesite-cookies-in-aspnet"></a>Pracuj z plikami cookie SameSite w ASP.NET

Autor: [Rick Anderson](https://twitter.com/RickAndMSFT)

SameSite jest projektem standardowym [IETF](https://ietf.org/about/) opracowanym w celu zapewnienia pewnej ochrony przed atakami polegającymi na translokacjach (CSRF). Pierwotnie Sporządzono w [2016](https://tools.ietf.org/html/draft-west-first-party-cookies-07), projekt Standard został zaktualizowany w [2019](https://tools.ietf.org/html/draft-west-cookie-incrementalism-00). Zaktualizowany standard nie jest zgodny z poprzednią wersją standardową, co poniżej przedstawiono najbardziej zauważalne różnice:

* Pliki cookie bez nagłówka SameSite są domyślnie traktowane jako `SameSite=Lax`.
* `SameSite=None` należy użyć, aby zezwolić na użycie plików cookie między lokacjami.
* Pliki cookie, które potwierdzają `SameSite=None` muszą być również oznaczone jako `Secure`.
* Wartość SameSite = None nie jest dozwolona przez [standard 2016](https://tools.ietf.org/html/draft-west-first-party-cookies-07) i powoduje, że niektóre implementacje traktują takie pliki cookie jako SameSite = Strict. Zobacz [Obsługa starszych przeglądarek](#sob) w tym dokumencie.

Ustawienie `SameSite=Lax` działa w przypadku większości plików cookie aplikacji. Niektóre formy uwierzytelniania, takie jak [OpenID Connect Connect](https://openid.net/connect/) (OIDC) i [WS-Federation](https://auth0.com/docs/protocols/ws-fed) , domyślnie mogą publikować przekierowania na podstawie. Przekierowania na podstawie wpisu wyzwalają ochronę za pomocą przeglądarki SameSite, więc SameSite jest wyłączona dla tych składników. Nie ma to wpływ na większość nazw logowania [OAuth](https://oauth.net/) z powodu różnic w sposobie przepływu żądania.

Aplikacje korzystające z `iframe` mogą napotkać problemy `SameSite=Lax` lub `SameSite=Strict` plików cookie, ponieważ elementy iframe są traktowane jako scenariusze między lokacjami.

Każdy składnik ASP.NET, który emituje pliki cookie, musi zdecydować, czy SameSite jest odpowiedni.

Zobacz [znane problemy](#known) dotyczące problemów z aplikacjami po 2019 zainstalowaniu aktualizacji SameSite .NET.

## <a name="using-samesite-in-aspnet-472-and-48"></a>Używanie SameSite w ASP.NET 4.7.2 i 4,8

Program .NET 4.7.2 i 4,8 obsługuje [wersję standard 2019](https://tools.ietf.org/html/draft-west-cookie-incrementalism-00) dla SameSite od czasu wydania aktualizacji w grudniu 2019. Deweloperzy mogą programowo sterować wartością nagłówka SameSite przy użyciu [Właściwości HttpCookie. SameSite](/dotnet/api/system.web.httpcookie.samesite#System_Web_HttpCookie_SameSite). Ustawienie właściwości `SameSite` na `Strict`, `Lax`lub `None` powoduje, że te wartości są zapisywane w sieci przy użyciu pliku cookie. Ustawienie równe `(SameSiteMode)(-1)` wskazuje, że w sieci nie powinien znajdować się nagłówek SameSite. [Właściwość HttpCookie. Secure](/dotnet/api/system.web.httpcookie.secure)lub "RequireSSL" w plikach konfiguracji może służyć do oznaczania pliku cookie jako `Secure` lub nie.

Nowe wystąpienia `HttpCookie` będą domyślnie `SameSite=(SameSiteMode)(-1)` i `Secure=false`. Te wartości domyślne można przesłonić w sekcji konfiguracji `system.web/httpCookies`, gdzie `"Unspecified"` ciąg jest przyjazną składnią tylko do konfiguracji dla `(SameSiteMode)(-1)`:

```xml
<configuration>
 <system.web>
  <httpCookies sameSite="[Strict|Lax|None|Unspecified]" requireSSL="[true|false]" />
 <system.web>
<configuration>
```

Program ASP.Net również zawiera cztery określone pliki cookie dla tych funkcji: uwierzytelnianie anonimowe, uwierzytelnianie formularzy, stan sesji i zarządzanie rolami. Wystąpienia tych plików cookie uzyskanych w środowisku uruchomieniowym można manipulować przy użyciu właściwości `SameSite` i `Secure`, podobnie jak każde inne wystąpienie HttpCookie. Jednak ze względu na zbieraninyą w standardzie SameSite opcje konfiguracji tych czterech plików cookie są niespójne. Poniżej przedstawiono odpowiednie sekcje i atrybuty konfiguracji z wartościami domyślnymi. Jeśli dla funkcji nie ma `SameSite` ani `Secure` powiązany atrybut, funkcja zostanie przywrócona zgodnie z ustawieniami domyślnymi skonfigurowanymi w sekcji `system.web/httpCookies` opisanej powyżej.

```xml
<configuration>
 <system.web>
  <anonymousIdentification cookieRequireSSL="false" /> <!-- No config attribute for SameSite -->
  <authentication>
   <forms cookieSameSite="Lax" requireSSL="false" />
  </authentication>
  <sessionState cookieSameSite="Lax" /> <!-- No config attribute for Secure -->
  <roleManager cookieRequireSSL="false" /> <!-- No config attribute for SameSite -->
 <system.web>
<configuration>
```  

**Uwaga**: element "unonly" jest dostępny tylko do `system.web/httpCookies@sameSite` w tym momencie. Mamy nadzieję, że dodamy podobną składnię do wcześniej pokazanych atrybutów cookieSameSite w przyszłych aktualizacjach. Ustawienie `(SameSiteMode)(-1)` w kodzie nadal działa w wystąpieniach tych plików cookie. *

## <a name="history-and-changes"></a>Historia i zmiany

Obsługa SameSite została najpierw zaimplementowana w programie .NET 4.7.2 przy użyciu [standardowego standardu 2016](https://tools.ietf.org/html/draft-west-first-party-cookies-07#section-4.1).

19 listopada 2019 aktualizacje dla systemu Windows Zaktualizowano .NET 4.7.2 + ze standardu 2016 do standardu 2019. Dodatkowe aktualizacje są nachodzące dla innych wersji systemu Windows. Aby uzyskać więcej informacji, zobacz <xref:samesite/kbs-samesite>.

 Wersja robocza 2019 specyfikacji SameSite:

* **Nie** jest wstecznie zgodne z wersją roboczą 2016. Aby uzyskać więcej informacji, zobacz [Obsługa starszych przeglądarek](#sob) w tym dokumencie.
* Określa, że pliki cookie są domyślnie traktowane jako `SameSite=Lax`.
* Określa pliki cookie, które jawnie porzucają `SameSite=None` w celu włączenia dostarczania między lokacjami należy również oznaczyć jako `Secure`.
* Jest obsługiwane przez poprawki wydane zgodnie z opisem w powyższej bazie wiedzy.
* Zaplanowano włączenie programu [Chrome](https://chromestatus.com/feature/5088147346030592) domyślnie w [lutym 2020](https://blog.chromium.org/2019/10/developers-get-ready-for-new.html). Przeglądarki zaczynają przechodzenie do tego standardu w 2019.

<a name="known"><a/>

## <a name="known-issues"></a>Znane problemy

Ze względu na to, że specyfikacje robocze 2016 i 2019 nie są zgodne, Aktualizacja programu .NET Framework w listopadzie 2019 wprowadza pewne zmiany, które mogą zostać zerwane.

* Pliki cookie stanu sesji i uwierzytelniania formularzy są teraz zapisywane w sieci jako `Lax`, a nie nieokreślone.
  * Chociaż większość aplikacji pracuje z `SameSite=Lax` plikami cookie, aplikacje, które PUBLIKUJą w witrynach lub aplikacjach korzystających z `iframe` mogą stwierdzić, że ich stan sesji lub pliki cookie autoryzacji formularzy nie są używane zgodnie z oczekiwaniami. Aby rozwiązać ten konieczność, należy zmienić wartość `cookieSameSite` w odpowiedniej sekcji konfiguracji, jak opisano wcześniej.
* HttpCookies, który jawnie ustawił `SameSite=None` w kodzie lub konfiguracji, ma teraz tę wartość, która została wcześniej pominięta. Może to powodować problemy ze starszymi przeglądarkami, które obsługują tylko wersję standardową 2016.
  * W przypadku przeglądarek obsługujących wersję standardową 2019 w programie `SameSite=None` pliki cookie Pamiętaj, aby oznaczyć je także `Secure` lub nie można ich rozpoznać.
  * Aby przywrócić zachowanie 2016 niepisania `SameSite=None`, użyj ustawienia aplikacji `aspnet:SupressSameSiteNone=true`. Należy pamiętać, że dotyczy to wszystkich HttpCookies w aplikacji.

### <a name="azure-app-servicesamesite-cookie-handling"></a>Azure App Service — obsługa plików cookie SameSite

Zobacz [Azure App Service — obsługa plików cookie SameSite i .NET Framework Poprawka 4.7.2,](https://azure.microsoft.com/updates/app-service-samesite-cookie-update/) Aby uzyskać informacje na temat Azure App Service sposobu konfigurowania zachowań SameSite w aplikacjach .NET 4.7.2.

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

## <a name="additional-resources"></a>Zasoby dodatkowe

* [Nadchodzące zmiany plików cookie SameSite w ASP.NET i ASP.NET Core](https://devblogs.microsoft.com/aspnet/upcoming-samesite-cookie-changes-in-asp-net-and-asp-net-core/)
* [Blog chromu: deweloperzy: przygotowanie do nowego SameSite = none; Ustawienia bezpiecznego pliku cookie](https://blog.chromium.org/2019/10/developers-get-ready-for-new.html)
* [Wyjaśniono pliki cookie SameSite](https://web.dev/samesite-cookies-explained/)

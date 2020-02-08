---
uid: web-api/overview/security/authentication-filters
title: Filtry uwierzytelniania w programie ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: Filtr uwierzytelniania to składnik, który uwierzytelnia żądanie HTTP. Interfejsy Web API 2 i MVC 5 obsługują filtry uwierzytelniania, ale różnią się nieznacznie.
ms.author: riande
ms.date: 09/25/2014
ms.assetid: b9882e53-b3ca-4def-89b0-322846973ccb
msc.legacyurl: /web-api/overview/security/authentication-filters
msc.type: authoredcontent
ms.openlocfilehash: 2ef9e62a6c634237e920b6d7aba2127b835f959d
ms.sourcegitcommit: e365196c75ce93cd8967412b1cfdc27121816110
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/07/2020
ms.locfileid: "77075076"
---
# <a name="authentication-filters-in-aspnet-web-api-2"></a>Filtry uwierzytelniania w programie ASP.NET Web API 2

według [Jan Wasson](https://github.com/MikeWasson)

> Filtr uwierzytelniania to składnik, który uwierzytelnia żądanie HTTP. Interfejsy Web API 2 i MVC 5 obsługują filtry uwierzytelniania, ale różnią się one nieznacznie w odniesieniu do konwencji nazewnictwa dla interfejsu filtru. W tym temacie opisano filtry uwierzytelniania interfejsu API sieci Web.

Filtry uwierzytelniania umożliwiają ustawienie schematu uwierzytelniania dla poszczególnych kontrolerów lub akcji. Dzięki temu aplikacja może obsługiwać różne mechanizmy uwierzytelniania dla różnych zasobów HTTP.

W tym artykule pokazywać kod z przykładu [uwierzytelniania podstawowego](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/BasicAuthentication) na [https://github.com/aspnet/samples](https://github.com/aspnet/samples). Przykład pokazuje filtr uwierzytelniania, który implementuje schemat uwierzytelniania dostępu podstawowego protokołu HTTP (RFC 2617). Filtr jest zaimplementowany w klasie o nazwie `IdentityBasicAuthenticationAttribute`. Nie pokazujemy całego kodu z próbki, tylko części, które ilustrują sposób pisania filtru uwierzytelniania.

## <a name="setting-an-authentication-filter"></a>Ustawianie filtru uwierzytelniania

Podobnie jak w przypadku innych filtrów, filtry uwierzytelniania mogą być stosowane do poszczególnych kontrolerów interfejsu API sieci Web.

Aby zastosować filtr uwierzytelniania do kontrolera, dekorować klasę kontrolera z atrybutem Filter. Poniższy kod ustawia filtr `[IdentityBasicAuthentication]` dla klasy kontrolera, co umożliwia uwierzytelnianie podstawowe dla wszystkich akcji kontrolera.

[!code-csharp[Main](authentication-filters/samples/sample1.cs)]

Aby zastosować filtr do jednej akcji, dekorować akcję z filtrem. Poniższy kod ustawia filtr `[IdentityBasicAuthentication]` na metodzie `Post` kontrolera.

[!code-csharp[Main](authentication-filters/samples/sample2.cs)]

Aby zastosować filtr do wszystkich kontrolerów internetowego interfejsu API, Dodaj go do **GlobalConfiguration. filters**.

[!code-csharp[Main](authentication-filters/samples/sample3.cs)]

## <a name="implementing-a-web-api-authentication-filter"></a>Implementowanie filtru uwierzytelniania interfejsu API sieci Web

W interfejsie API sieci Web filtry uwierzytelniania implementują interfejs [System. Web. http. filters. IAuthenticationFilter](https://msdn.microsoft.com/library/system.web.http.filters.iauthenticationfilter.aspx) . Powinny one również dziedziczyć z **System. Attribute**, aby można było je zastosować jako atrybuty.

Interfejs **IAuthenticationFilter** ma dwie metody:

- **AuthenticateAsync** uwierzytelnia żądanie, sprawdzając poświadczenia w żądaniu, jeśli są obecne.
- **ChallengeAsync** dodaje wyzwanie uwierzytelniania do odpowiedzi HTTP, w razie potrzeby.

Metody te odnoszą się do przepływu uwierzytelniania zdefiniowanego w [rfc 2612](http://tools.ietf.org/html/rfc2616) i [RFC 2617](http://tools.ietf.org/html/rfc2617):

1. Klient wysyła poświadczenia w nagłówku autoryzacji. Zwykle dzieje się tak, gdy klient otrzymuje odpowiedź 401 (nieautoryzowaną) z serwera. Klient może jednak wysyłać poświadczenia z dowolnym żądaniem, a nie zaraz po otrzymaniu 401.
2. Jeśli serwer nie akceptuje poświadczeń, zwraca odpowiedź 401 (bez autoryzacji). Odpowiedź zawiera nagłówek WWW-Authenticate, który zawiera jedno lub kilka wyzwań. Każde wyzwanie określa schemat uwierzytelniania rozpoznany przez serwer.

Serwer może również zwrócić 401 z anonimowego żądania. W rzeczywistości zazwyczaj jest inicjowany proces uwierzytelniania:

1. Klient wysyła żądanie anonimowe.
2. Serwer zwraca 401.
3. Klienci ponownie wysyłają żądanie z poświadczeniami.

Ten przepływ obejmuje procedurę *uwierzytelniania* i *autoryzacji* .

- Uwierzytelnianie udowadnia tożsamość klienta.
- Autoryzacja określa, czy klient może uzyskać dostęp do określonego zasobu.

W interfejsie API sieci Web filtry uwierzytelniania obsługują uwierzytelnianie, ale nie autoryzację. Autoryzacja powinna odbywać się przez filtr autoryzacji lub wewnątrz akcji kontrolera.

Oto przepływ w potoku Web API 2:

1. Przed wywołaniem akcji interfejs API sieci Web tworzy listę filtrów uwierzytelniania dla tej akcji. Obejmuje to filtry z zakresem akcji, zakresem kontrolera i zakresem globalnym.
2. Interfejs API sieci Web wywołuje **AuthenticateAsync** na każdym filtrze na liście. Każdy filtr może sprawdzać poprawność poświadczeń w żądaniu. Jeśli dowolny filtr pomyślnie sprawdza poprawność poświadczeń, filtr tworzy **IPrincipal** i dołącza go do żądania. Filtr może również wyzwolić błąd w tym momencie. Jeśli tak, pozostała część potoku nie zostanie uruchomiona.
3. Przy założeniu, że nie ma żadnych błędów, żądanie przepływa przez resztę potoku.
4. Na koniec interfejs API sieci Web wywołuje metodę **ChallengeAsync** każdego filtru uwierzytelniania. Filtry używają tej metody w celu dodania wyzwania do odpowiedzi, w razie potrzeby. Zwykle (ale nie zawsze), co się stanie w odpowiedzi na błąd 401.

Na poniższych diagramach przedstawiono dwa możliwe przypadki. W pierwszej kolejności filtr uwierzytelniania pomyślnie uwierzytelnia żądanie, filtr autoryzacji autoryzuje żądanie, a akcja kontrolera zwraca 200 (OK).

![](authentication-filters/_static/image1.png)

W drugim przykładzie filtr uwierzytelniania uwierzytelnia żądanie, ale filtr autoryzacji zwraca 401 (bez autoryzacji). W takim przypadku akcja kontrolera nie zostanie wywołana. Filtr uwierzytelniania dodaje nagłówek WWW-Authenticate do odpowiedzi.

![](authentication-filters/_static/image2.png)

Inne kombinacje są możliwe&mdash;na przykład, jeśli akcja kontrolera zezwala na żądania anonimowe, może istnieć filtr uwierzytelniania, ale brak autoryzacji.

## <a name="implementing-the-authenticateasync-method"></a>Implementowanie metody AuthenticateAsync

Metoda **AuthenticateAsync** próbuje uwierzytelnić żądanie. W tym miejscu znajduje się sygnatura metody:

[!code-csharp[Main](authentication-filters/samples/sample4.cs)]

Metoda **AuthenticateAsync** musi wykonać jedną z następujących czynności:

1. Nic (No-op).
2. Utwórz element **IPrincipal** i ustaw go na żądanie.
3. Ustaw wynik błędu.

Opcja (1) oznacza, że żądanie nie miało żadnych poświadczeń, które rozpoznaje filtr. Opcja (2) oznacza, że filtr pomyślnie uwierzytelnił żądanie. Opcja (3) oznacza, że żądanie miało nieprawidłowe poświadczenia (takie jak nieprawidłowe hasło), które wyzwala odpowiedź na błąd.

Oto ogólny zarys implementacji **AuthenticateAsync**.

1. Wyszukaj poświadczenia w żądaniu.
2. Jeśli nie ma żadnych poświadczeń, nic nie rób i zwróć (No-op).
3. Jeśli istnieją poświadczenia, ale filtr nie rozpoznaje schematu uwierzytelniania, nic nie rób ani nie zwraca (No-op). Inny filtr w potoku może zrozumieć schemat.
4. Jeśli istnieją poświadczenia zrozumiałe dla filtru, spróbuj je uwierzytelnić.
5. Jeśli poświadczenia są nieprawidłowe, zwróć 401 przez ustawienie `context.ErrorResult`.
6. Jeśli poświadczenia są prawidłowe, Utwórz **IPrincipal** i ustaw `context.Principal`.

W poniższym kodzie przedstawiono metodę **AuthenticateAsync** z przykładu [podstawowego uwierzytelniania](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/BasicAuthentication) . Komentarze wskazują każdy krok. Kod pokazuje kilka typów błędu: nagłówek autoryzacji bez poświadczeń, źle sformułowane poświadczenia i zła nazwa użytkownika/hasło.

[!code-csharp[Main](authentication-filters/samples/sample5.cs)]

## <a name="setting-an-error-result"></a>Ustawianie wyniku błędu

Jeśli poświadczenia są nieprawidłowe, filtr musi ustawić `context.ErrorResult` na **IHttpActionResult** , który tworzy odpowiedź na błąd. Aby uzyskać więcej informacji na temat **IHttpActionResult**, zobacz [wyniki akcji w interfejsie Web API 2](../getting-started-with-aspnet-web-api/action-results.md).

Przykład uwierzytelniania podstawowego zawiera klasę `AuthenticationFailureResult`, która jest odpowiednia do tego celu.

[!code-csharp[Main](authentication-filters/samples/sample6.cs)]

## <a name="implementing-challengeasync"></a>Implementowanie ChallengeAsync

Celem metody **ChallengeAsync** jest dodanie wyzwań uwierzytelniania do odpowiedzi, w razie potrzeby. W tym miejscu znajduje się sygnatura metody:

[!code-csharp[Main](authentication-filters/samples/sample7.cs)]

Metoda jest wywoływana dla każdego filtru uwierzytelniania w potoku żądania.

Ważne jest, aby zrozumieć, że **ChallengeAsync** jest wywoływana *przed utworzeniem* odpowiedzi HTTP i prawdopodobnie nawet przed uruchomieniem akcji kontrolera. Gdy **ChallengeAsync** jest wywoływana, `context.Result` zawiera **IHttpActionResult**, który jest używany później do utworzenia odpowiedzi HTTP. Więc gdy **ChallengeAsync** jest wywoływana, nie wiesz jeszcze niczego o odpowiedzi HTTP. Metoda **ChallengeAsync** powinna zastąpić oryginalną wartość `context.Result` nową **IHttpActionResult**. Ta **IHttpActionResult** musi otaczać oryginalny `context.Result`.

![](authentication-filters/_static/image3.png)

Wywołaję oryginalny **IHttpActionResult** *wyniku wewnętrznego*i nowy **IHttpActionResult** *wynik zewnętrzny*. Zewnętrzny wynik musi wykonać następujące czynności:

1. Wywołaj wynik wewnętrzny, aby utworzyć odpowiedź HTTP.
2. Badanie odpowiedzi.
3. W razie potrzeby Dodaj wyzwanie uwierzytelniania do odpowiedzi.

Poniższy przykład jest pobierany z przykładu podstawowego uwierzytelniania. Definiuje **IHttpActionResult** dla zewnętrznego wyniku.

[!code-csharp[Main](authentication-filters/samples/sample8.cs)]

Właściwość `InnerResult` zawiera wewnętrzny **IHttpActionResult**. Właściwość `Challenge` reprezentuje nagłówek uwierzytelniania www. Zwróć uwagę, że **wywoływanie ExecuteAsync** najpierw wywołuje `InnerResult.ExecuteAsync` w celu utworzenia odpowiedzi HTTP, a następnie dodaje wyzwanie w razie potrzeby.

Przed dodaniem wyzwania Sprawdź kod odpowiedzi. Większość schematów uwierzytelniania dodaje wyzwanie tylko wtedy, gdy odpowiedź to 401, jak pokazano poniżej. Niektóre schematy uwierzytelniania umożliwiają jednak dodanie wyzwania do odpowiedzi o powodzeniu. Na przykład zobacz [Negotiate](http://tools.ietf.org/html/rfc4559#section-5) (RFC 4559).

W przypadku klasy `AddChallengeOnUnauthorizedResult` rzeczywista wartość kodu w **ChallengeAsync** jest prosta. Wystarczy utworzyć wynik i dołączyć go do `context.Result`.

[!code-csharp[Main](authentication-filters/samples/sample9.cs)]

Uwaga: przykład uwierzytelniania podstawowego jest abstrakcyjny dla tej logiki jako bit, umieszczając ją w metodzie rozszerzenia.

## <a name="combining-authentication-filters-with-host-level-authentication"></a>Łączenie filtrów uwierzytelniania z uwierzytelnianiem na poziomie hosta

"Uwierzytelnianie na poziomie hosta" to uwierzytelnianie wykonywane przez hosta (na przykład IIS), zanim żądanie osiągnie strukturę internetowego interfejsu API.

Często można włączyć uwierzytelnianie na poziomie hosta dla pozostałej części aplikacji, ale wyłączyć je dla kontrolerów interfejsu API sieci Web. Typowym scenariuszem jest na przykład włączenie uwierzytelniania formularzy na poziomie hosta, ale użycie uwierzytelniania opartego na tokenach dla internetowego interfejsu API.

Aby wyłączyć uwierzytelnianie na poziomie hosta w potoku interfejsu API sieci Web, wywołaj `config.SuppressHostPrincipal()` w konfiguracji. Powoduje to, że interfejs API sieci Web usuwa **IPrincipal** z dowolnych żądań, które wprowadzają potok interfejsu API sieci Web. Skutecznie &quot;nie uwierzytelnia&quot; żądania.

[!code-csharp[Main](authentication-filters/samples/sample10.cs)]

## <a name="additional-resources"></a>Dodatkowe zasoby

[Filtry zabezpieczeń interfejsu API sieci Web ASP.NET](https://msdn.microsoft.com/magazine/dn781361.aspx) (Magazyn MSDN)

---
uid: signalr/overview/security/introduction-to-security
title: Wprowadzenie do zabezpieczeń sygnalizujących | Microsoft Docs
author: bradygaster
description: Opisuje problemy z zabezpieczeniami, które należy wziąć pod uwagę podczas tworzenia aplikacji sygnalizującej.
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: ed562717-8591-4936-8e10-c7e63dcb570a
msc.legacyurl: /signalr/overview/security/introduction-to-security
msc.type: authoredcontent
ms.openlocfilehash: 24ce20b45543468de28ad017ba62d2f6e5a00f3b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78558577"
---
# <a name="introduction-to-signalr-security"></a>Wprowadzenie do zabezpieczeń usługi SignalR

[Fletcher Patryk](https://github.com/pfletcher), [Tomasz FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> W tym artykule opisano problemy związane z zabezpieczeniami, które należy wziąć pod uwagę podczas tworzenia aplikacji sygnalizującej.
>
> ## <a name="software-versions-used-in-this-topic"></a>Wersje oprogramowania używane w tym temacie
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - Sygnalizujący wersja 2
>
>
>
> ## <a name="previous-versions-of-this-topic"></a>Poprzednie wersje tego tematu
>
> Aby uzyskać informacje o wcześniejszych wersjach programu sygnalizującego, zobacz sekcję [sygnalizujące starsze wersje](../older-versions/index.md).
>
> ## <a name="questions-and-comments"></a>Pytania i Komentarze
>
> Prosimy o opinię na temat sposobu, w jaki lubię ten samouczek, i co możemy ulepszyć w komentarzach w dolnej części strony. Jeśli masz pytania, które nie są bezpośrednio związane z samouczkiem, możesz je ogłosić na [forum ASP.NET](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).

## <a name="overview"></a>Omówienie

Ten dokument zawiera następujące sekcje:

- [Pojęcia dotyczące zabezpieczeń sygnałów](#concepts)

    - [Uwierzytelnianie i autoryzacja](#authentication)
    - [Token połączenia](#connectiontoken)
    - [Ponowne łączenie grup po ponownym połączeniu](#rejoingroup)
- [Jak sygnalizujący zapobiega podrabianiu żądań między lokacjami](#csrf)
- [Zalecenia dotyczące zabezpieczeń sygnałów](#recommendations)

    - [Protokół SSL (Secure Sockets)](#ssl)
    - [Nie używaj grup jako mechanizmu zabezpieczeń](#groupsecurity)
    - [Bezpieczne obsługiwanie danych wejściowych z klientów](#input)
    - [Uzgadnianie zmiany stanu użytkownika z aktywnym połączeniem](#reconcile)
    - [Automatycznie generowane pliki proxy JavaScript](#autogen)
    - [Wyjątki](#exceptions)

<a id="concepts"></a>

## <a name="signalr-security-concepts"></a>Pojęcia dotyczące zabezpieczeń sygnałów

<a id="authentication"></a>

### <a name="authentication-and-authorization"></a>Uwierzytelnianie i autoryzacja

Usługa sygnalizująca nie udostępnia żadnych funkcji uwierzytelniania użytkowników. Zamiast tego należy zintegrować funkcje sygnalizujące z istniejącą strukturą uwierzytelniania dla aplikacji. Użytkownicy są uwierzytelniani jako zwykle w aplikacji i pracują z wynikami uwierzytelniania w kodzie sygnalizującym. Można na przykład uwierzytelniać użytkowników za pomocą uwierzytelniania ASP.NET Forms, a następnie w centrum, wymusić, którzy użytkownicy lub które role mają autoryzację, aby wywołać metodę. W centrum można także przekazać informacje uwierzytelniania, takie jak nazwa użytkownika lub czy użytkownik należy do roli, do klienta programu.

Sygnalizujący udostępnia atrybut [Autoryzuj](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) , aby określić, którzy użytkownicy mają dostęp do centrum lub metody. Należy zastosować atrybut Autoryzuj do koncentratora lub określonych metod w centrum. Bez atrybutu Autoryzuj wszystkie metody publiczne w centrum są dostępne dla klienta, który jest podłączony do centrum. Aby uzyskać więcej informacji na temat centrów, zobacz [uwierzytelnianie i autoryzacja dla centrów sygnałów](hub-authorization.md).

Należy zastosować atrybut `Authorize` do centrów, ale nie połączeń trwałych. Aby wymusić reguły autoryzacji przy użyciu `PersistentConnection` należy zastąpić metodę `AuthorizeRequest`. Aby uzyskać więcej informacji na temat połączeń trwałych, zobacz [uwierzytelnianie i autoryzacja dla połączeń trwałych](persistent-connection-authorization.md).

<a id="connectiontoken"></a>

### <a name="connection-token"></a>Token połączenia

Program sygnalizujący zmniejsza ryzyko wykonania złośliwych poleceń przez zweryfikowanie tożsamości nadawcy. Dla każdego żądania klient i serwer przekażą token połączenia, który zawiera identyfikator połączenia i nazwę użytkownika dla uwierzytelnionych użytkowników. Identyfikator połączenia jednoznacznie identyfikuje każdego połączonego klienta. Serwer losowo generuje identyfikator połączenia podczas tworzenia nowego połączenia i utrzymuje ten identyfikator na czas trwania połączenia. Mechanizm uwierzytelniania dla aplikacji sieci Web zapewnia nazwę użytkownika. W celu ochrony tokenu połączenia program sygnalizujący używa szyfrowania i podpisu cyfrowego.

![](introduction-to-security/_static/image2.png)

Dla każdego żądania serwer sprawdza zawartość tokenu, aby upewnić się, że żądanie pochodzi od określonego użytkownika. Nazwa użytkownika musi odpowiadać identyfikatorowi połączenia. Sprawdzając poprawność zarówno identyfikatora połączenia, jak i nazwy użytkownika, sygnalizujący uniemożliwia złośliwemu użytkownikowi łatwe personifikowanie innego użytkownika. Jeśli serwer nie może zweryfikować tokenu połączenia, żądanie nie powiedzie się.

![](introduction-to-security/_static/image4.png)

Ponieważ identyfikator połączenia jest częścią procesu weryfikacji, nie należy ujawniać identyfikatora połączenia jednego użytkownika innym użytkownikom ani przechowywać wartości na komputerze klienckim, na przykład w pliku cookie.

#### <a name="connection-tokens-vs-other-token-types"></a>Tokeny połączenia a inne typy tokenów

Tokeny połączenia są czasami oflagowane przez narzędzia zabezpieczeń, ponieważ pojawiają się one jako tokeny sesji lub tokeny uwierzytelniania, które stanowią zagrożenie, jeśli są uwidocznione.

Token połączenia sygnalizującego nie jest tokenem uwierzytelniania. Służy do potwierdzenia, że użytkownik wykonujący to żądanie jest tym samym, który utworzył połączenie. Token połączenia jest niezbędny, ponieważ sygnał ASP.NET umożliwia nawiązywanie połączeń między serwerami. Token kojarzy połączenie z określonym użytkownikiem, ale nie potwierdza tożsamości użytkownika zgłaszającego żądanie. Aby żądanie sygnału zostało prawidłowo uwierzytelnione, musi mieć inny token, który potwierdza tożsamość użytkownika, na przykład plik cookie lub token okaziciela. Jednak sam token połączenia nie zgłasza żadnego żądania przez tego użytkownika, tylko że identyfikator połączenia zawarty w tokenie jest skojarzony z tym użytkownikiem.

Ponieważ token połączenia nie zapewnia własnego oświadczenia uwierzytelniania, nie jest traktowany jako token "Session" lub "Authentication". Pobranie tokenu połączenia danego użytkownika i jego odtworzenie w żądaniu uwierzytelnianym jako inny użytkownik (lub nieuwierzytelnione żądanie) zakończy się niepowodzeniem, ponieważ tożsamość użytkownika żądania i tożsamość przechowywana w tokenie nie będą zgodne.

<a id="rejoingroup"></a>

### <a name="rejoining-groups-when-reconnecting"></a>Ponowne łączenie grup po ponownym połączeniu

Domyślnie aplikacja sygnalizująca automatycznie ponownie przypisze użytkownika do odpowiednich grup podczas ponownego nawiązywania połączenia po tymczasowym przerwaniu, na przykład w przypadku porzucenia i ponownego ustanowienia połączenia przed upływem limitu czasu połączenia. Po ponownym nawiązaniu połączenia klient przekazuje token grupy zawierający identyfikator połączenia i przypisane grupy. Token grupy jest podpisany cyfrowo i szyfrowany. Klient zachowuje ten sam identyfikator połączenia po ponownej nawiązaniu połączenia; w związku z tym identyfikator połączenia przekazaną przez ponownie podłączony klient musi być zgodny z poprzednim identyfikatorem połączenia używanym przez klienta. Ta weryfikacja uniemożliwia złośliwemu użytkownikowi przekazanie żądań dołączenia do nieautoryzowanych grup podczas ponownego nawiązywania połączenia.

Należy jednak pamiętać, że token grupy nie wygasa. Jeśli użytkownik należy do grupy w przeszłości, ale został zabroniony z tej grupy, może być w stanie naśladować token grupy zawierający zabronioną grupę. Jeśli musisz bezpiecznie zarządzać użytkownikami należącymi do grup, musisz przechowywać te dane na serwerze, na przykład w bazie danych. Następnie należy dodać logikę do aplikacji, która weryfikuje na serwerze, czy użytkownik należy do grupy. Aby zapoznać się z przykładem weryfikowania członkostwa w grupie, zobacz [Praca z grupami](../guide-to-the-api/working-with-groups.md).

Automatyczne Ponowne przyłączanie grup ma zastosowanie tylko wtedy, gdy połączenie jest ponownie nawiązywane po tymczasowym przerwie. Jeśli użytkownik odłączy się przez przejście do aplikacji lub ponowne uruchomienie aplikacji, aplikacja musi obsłużyć procedurę dodawania tego użytkownika do odpowiednich grup. Aby uzyskać więcej informacji, zobacz [Praca z grupami](../guide-to-the-api/working-with-groups.md).

<a id="csrf"></a>

## <a name="how-signalr-prevents-cross-site-request-forgery"></a>Jak sygnalizujący zapobiega podrabianiu żądań między lokacjami

Fałszerstwo żądania między lokacjami (CSRF) to atak polegający na tym, że złośliwa witryna wysyła żądanie do zagrożonej lokacji, w której użytkownik jest aktualnie zalogowany. Program sygnalizujący uniemożliwia CSRF, uniemożliwiając złośliwej lokacji utworzenie prawidłowego żądania dla aplikacji sygnalizującej.

### <a name="description-of-csrf-attack"></a>Opis ataku CSRF

Oto przykład ataku CSRF:

1. Użytkownik loguje się do www.example.com przy użyciu uwierzytelniania formularzy.
2. Serwer uwierzytelnia użytkownika. Odpowiedź z serwera zawiera plik cookie uwierzytelniania.
3. Bez wylogowywania użytkownik odwiedza złośliwą witrynę sieci Web. Ta złośliwa witryna zawiera następującą postać HTML:

    [!code-html[Main](introduction-to-security/samples/sample1.html)]

   Należy zauważyć, że w ramach akcji formularza są umieszczane wpisy w zagrożonej witrynie, a nie w złośliwej witrynie. To jest część "wiele witryn" z CSRF.
4. Użytkownik klika przycisk Prześlij. Przeglądarka zawiera plik cookie uwierzytelniania z żądaniem.
5. Żądanie jest uruchamiane na serwerze example.com z kontekstem uwierzytelniania użytkownika i może wykonywać wszystkie czynności, które mogą wykonać uwierzytelnionego użytkownika.

Mimo że ten przykład wymaga, aby użytkownik klikał przycisk formularza, złośliwa strona może jak równie łatwo uruchomić skrypt wysyłający żądanie AJAX do aplikacji sygnalizującej. Ponadto użycie protokołu SSL nie uniemożliwia ataku CSRF, ponieważ złośliwa witryna może wysłać żądanie "https://".

Zazwyczaj ataki CSRF są dostępne dla witryn sieci Web, które używają plików cookie do uwierzytelniania, ponieważ przeglądarki wysyłają wszystkie odpowiednie pliki cookie do docelowej witryny sieci Web. Jednak ataki CSRF nie ograniczają się do korzystania z plików cookie. Na przykład uwierzytelnianie podstawowe i szyfrowane jest również zagrożone. Gdy użytkownik zaloguje się przy użyciu uwierzytelniania podstawowego lub szyfrowanego, przeglądarka automatycznie wyśle poświadczenia do momentu zakończenia sesji.

### <a name="csrf-mitigations-taken-by-signalr"></a>CSRF środki zaradcze wykonywane przez sygnalizującego

Program sygnalizujący wykonuje poniższe kroki, aby uniemożliwić złośliwym lokacjom Tworzenie prawidłowych żądań do aplikacji. Program sygnalizujący domyślnie wykonuje te kroki, nie trzeba podejmować żadnych działań w kodzie.

- **Wyłącz żądania między domenami** Sygnalizujący wyłącza żądania między domenami, aby uniemożliwić użytkownikom wywoływanie punktu końcowego sygnalizującego z domeny zewnętrznej. Sygnał traktuje wszystkie żądania z domeny zewnętrznej jako nieprawidłowe i blokuje żądanie. Zalecamy zachowanie tego zachowania domyślnego; w przeciwnym razie złośliwa witryna może dowolnych użytkowników do wysyłania poleceń do witryny. Jeśli konieczne jest użycie żądań między domenami, zobacz [jak ustanowić połączenie między domenami](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain) .
- **Przekaż token połączenia w ciągu zapytania, a nie w pliku cookie** Sygnał przekazuje token połączenia jako wartość ciągu zapytania, a nie jako plik cookie. Przechowywanie tokenu połączenia w pliku cookie jest niebezpieczne, ponieważ przeglądarka może przypadkowo przesłać token połączenia w przypadku napotkania złośliwego kodu. Ponadto przekazywanie tokenu połączenia w ciągu zapytania uniemożliwia utrzymywanie tokenu połączenia poza bieżącym połączeniem. W związku z tym złośliwy użytkownik nie może wykonać żądania w ramach poświadczeń uwierzytelniania innego użytkownika.
- **Weryfikuj token połączenia** Zgodnie z opisem w sekcji [token połączenia](#connectiontoken) serwer wie, który identyfikator połączenia jest skojarzony z każdym uwierzytelnionym użytkownikiem. Serwer nie przetwarza żadnych żądań z identyfikatora połączenia, który nie jest zgodny z nazwą użytkownika. Jest mało prawdopodobne, że złośliwy użytkownik może odgadnąć prawidłowe żądanie, ponieważ złośliwy użytkownik musi znać nazwę użytkownika i bieżący wygenerowany losowo identyfikator połączenia. Ten identyfikator połączenia będzie nieprawidłowy, gdy tylko zakończy się połączenie. Użytkownicy anonimowi nie powinni mieć dostępu do żadnych poufnych informacji.

<a id="recommendations"></a>

## <a name="signalr-security-recommendations"></a>Zalecenia dotyczące zabezpieczeń sygnałów

<a id="ssl"></a>

### <a name="secure-socket-layers-ssl-protocol"></a>Protokół SSL (Secure Sockets)

Protokół SSL używa szyfrowania w celu zabezpieczenia transportu danych między klientem i serwerem. Jeśli aplikacja sygnalizująca przesyła poufne informacje między klientem a serwerem, należy użyć protokołu SSL do transportu. Aby uzyskać więcej informacji o konfigurowaniu protokołu SSL, zobacz [jak skonfigurować protokół SSL w usługach IIS 7](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).

<a id="groupsecurity"></a>

### <a name="do-not-use-groups-as-a-security-mechanism"></a>Nie używaj grup jako mechanizmu zabezpieczeń

Grupy to wygodny sposób zbierania powiązanych użytkowników, ale nie są bezpiecznym mechanizmem ograniczania dostępu do poufnych informacji. Jest to szczególnie prawdziwe, gdy użytkownicy mogą automatycznie odłączać grupy podczas ponownego nawiązywania połączenia. Zamiast tego należy rozważyć dodanie uprzywilejowanych użytkowników do roli i ograniczenie dostępu do metody centrum tylko do członków tej roli. Aby uzyskać przykład ograniczenia dostępu na podstawie roli, zobacz [uwierzytelnianie i autoryzacja dla centrów sygnałów](hub-authorization.md). Aby zapoznać się z przykładem sprawdzania dostępu użytkowników do grup podczas ponownego nawiązywania połączenia, zobacz [Praca z grupami](../guide-to-the-api/working-with-groups.md).

<a id="input"></a>

### <a name="safely-handling-input-from-clients"></a>Bezpieczne obsługiwanie danych wejściowych z klientów

Aby upewnić się, że złośliwy użytkownik nie wyśle skryptu do innych użytkowników, należy zakodować wszystkie dane wejściowe od klientów przeznaczonych do emisji do innych klientów. Należy kodować komunikaty na klientach otrzymujących zamiast na serwerze, ponieważ aplikacja sygnalizująca może mieć wiele różnych typów klientów. W związku z tym kodowanie HTML działa dla klienta sieci Web, ale nie dla innych typów klientów. Na przykład Metoda klienta sieci Web, aby wyświetlić komunikat rozmowy, bezpiecznie obsłużyć nazwę użytkownika i komunikat, wywołując funkcję `html()`.

[!code-html[Main](introduction-to-security/samples/sample2.html?highlight=3-4)]

<a id="reconcile"></a>

### <a name="reconciling-a-change-in-user-status-with-an-active-connection"></a>Uzgadnianie zmiany stanu użytkownika z aktywnym połączeniem

Jeśli stan uwierzytelnienia użytkownika zostanie zmieniony, gdy istnieje aktywne połączenie, użytkownik otrzyma komunikat o błędzie z informacją "tożsamość użytkownika nie może ulec zmianie podczas aktywnego połączenia sygnalizującego". W takim przypadku aplikacja powinna ponownie połączyć się z serwerem, aby upewnić się, że identyfikator połączenia i nazwa użytkownika są skoordynowane. Na przykład jeśli aplikacja zezwala użytkownikowi na wylogowanie się, gdy istnieje aktywne połączenie, nazwa użytkownika dla połączenia nie będzie już zgodna z nazwą przekazaną dla następnego żądania. Należy zatrzymać połączenie przed wylogowaniem użytkownika, a następnie uruchomić go ponownie.

Należy jednak pamiętać, że większość aplikacji nie będzie musiała ręcznie zatrzymać i uruchomić połączenia. Jeśli aplikacja przekierowuje użytkowników do osobnej strony po wylogowaniu, takiej jak domyślne zachowanie w aplikacji formularzy sieci Web lub aplikacji MVC, lub odświeża bieżącą stronę po wylogowaniu, aktywne połączenie zostanie automatycznie rozłączone i nie Wymagaj wszelkich dodatkowych akcji.

Poniższy przykład pokazuje, jak zatrzymać i uruchomić połączenie, gdy stan użytkownika został zmieniony.

[!code-html[Main](introduction-to-security/samples/sample3.html)]

Można również zmienić stan uwierzytelniania użytkownika, jeśli w lokacji jest używane okresowe wygaśnięcie z uwierzytelnianiem formularzy i nie ma aktywności, aby zachować prawidłowy plik cookie uwierzytelniania. W takim przypadku użytkownik zostanie wylogowany, a nazwa użytkownika nie będzie już zgodna z nazwą użytkownika w tokenie połączenia. Ten problem można rozwiązać, dodając jakiś skrypt, który okresowo żąda zasobu na serwerze sieci Web, aby zachować prawidłowy plik cookie uwierzytelniania. Poniższy przykład pokazuje, jak zażądać zasobu co 30 minut.

[!code-javascript[Main](introduction-to-security/samples/sample4.js)]

<a id="autogen"></a>

### <a name="automatically-generated-javascript-proxy-files"></a>Automatycznie generowane pliki proxy JavaScript

Jeśli nie chcesz uwzględniać wszystkich centrów i metod w pliku proxy JavaScript dla każdego użytkownika, możesz wyłączyć automatyczne generowanie pliku. Możesz wybrać tę opcję, jeśli masz wiele centrów i metod, ale nie chcesz, aby każdy użytkownik miał świadomość wszystkich metod. Aby wyłączyć generowanie automatyczne, należy ustawić **EnableJavaScriptProxies** na **false**.

[!code-csharp[Main](introduction-to-security/samples/sample5.cs)]

Aby uzyskać więcej informacji na temat plików serwera proxy języka JavaScript, zobacz [wygenerowany serwer proxy i jego zawartość](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy). <a id="exceptions"></a>

### <a name="exceptions"></a>Wyjątki

Należy unikać przekazywania obiektów wyjątków do klientów, ponieważ obiekty mogą uwidaczniać klientom poufne informacje. Zamiast tego należy wywołać metodę na kliencie, który wyświetla odpowiedni komunikat o błędzie.

[!code-csharp[Main](introduction-to-security/samples/sample6.cs)]

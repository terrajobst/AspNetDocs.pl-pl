---
uid: signalr/overview/older-versions/introduction-to-security
title: Wprowadzenie do zabezpieczeń sygnalizujących (sygnał 1. x) | Microsoft Docs
author: bradygaster
description: Opisuje problemy z zabezpieczeniami, które należy wziąć pod uwagę podczas tworzenia aplikacji sygnalizującej.
ms.author: bradyg
ms.date: 10/17/2013
ms.assetid: 715a4059-d307-4631-abbb-c789c95d6eb4
msc.legacyurl: /signalr/overview/older-versions/introduction-to-security
msc.type: authoredcontent
ms.openlocfilehash: 34172c0a2a15a7ab0d782704d5831ce236f5c989
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536730"
---
# <a name="introduction-to-signalr-security-signalr-1x"></a>Wprowadzenie do zabezpieczeń usługi SignalR (SignalR 1.x)

[Fletcher Patryk](https://github.com/pfletcher), [Tomasz FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> W tym artykule opisano problemy związane z zabezpieczeniami, które należy wziąć pod uwagę podczas tworzenia aplikacji sygnalizującej.

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

Sygnał został zaprojektowany do zintegrowania z istniejącą strukturą uwierzytelniania dla aplikacji. Nie udostępnia żadnych funkcji uwierzytelniania użytkowników. Zamiast tego uwierzytelniasz użytkowników jako normalnie w aplikacji, a następnie pracujesz z wynikami uwierzytelniania w kodzie sygnalizującym. Można na przykład uwierzytelniać użytkowników za pomocą uwierzytelniania ASP.NET Forms, a następnie w centrum, wymusić, którzy użytkownicy lub które role mają autoryzację, aby wywołać metodę. W centrum można także przekazać informacje uwierzytelniania, takie jak nazwa użytkownika lub czy użytkownik należy do roli, do klienta programu.

Sygnalizujący udostępnia atrybut [Autoryzuj](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) , aby określić, którzy użytkownicy mają dostęp do centrum lub metody. Należy zastosować atrybut Autoryzuj do koncentratora lub określonych metod w centrum. Bez atrybutu Autoryzuj wszystkie metody publiczne w centrum są dostępne dla klienta, który jest podłączony do centrum. Aby uzyskać więcej informacji na temat centrów, zobacz [uwierzytelnianie i autoryzacja dla centrów sygnałów](../security/hub-authorization.md).

Atrybut `Authorize` jest używany tylko z koncentratorami. Aby wymusić reguły autoryzacji przy użyciu `PersistentConnection` należy zastąpić metodę `AuthorizeRequest`. Aby uzyskać więcej informacji na temat połączeń trwałych, zobacz [uwierzytelnianie i autoryzacja dla połączeń trwałych](../security/persistent-connection-authorization.md).

<a id="connectiontoken"></a>

### <a name="connection-token"></a>Token połączenia

Program sygnalizujący zmniejsza ryzyko wykonania złośliwych poleceń przez zweryfikowanie tożsamości nadawcy. Token połączenia zawierający identyfikator i nazwę użytkownika dla uwierzytelnionych użytkowników jest przesyłany między klientem i serwerem dla każdego żądania. Identyfikator połączenia jest unikatowym identyfikatorem, który jest generowana losowo przez serwer w przypadku utworzenia nowego połączenia i jest utrwalany na czas trwania połączenia. Nazwa użytkownika jest udostępniana przez mechanizm uwierzytelniania dla aplikacji sieci Web. Token połączenia jest chroniony przy użyciu szyfrowania i podpisu cyfrowego.

![](introduction-to-security/_static/image2.png)

Dla każdego żądania serwer sprawdza zawartość tokenu, aby upewnić się, że żądanie pochodzi od określonego użytkownika. Nazwa użytkownika musi odpowiadać identyfikatorowi połączenia. Sprawdzając poprawność zarówno identyfikatora połączenia, jak i nazwy użytkownika, sygnalizujący uniemożliwia złośliwemu użytkownikowi łatwe personifikowanie innego użytkownika. Jeśli serwer nie może zweryfikować tokenu połączenia, żądanie nie powiedzie się.

![](introduction-to-security/_static/image4.png)

Ponieważ identyfikator połączenia jest częścią procesu weryfikacji, nie należy ujawniać identyfikatora połączenia jednego użytkownika innym użytkownikom ani przechowywać wartości na komputerze klienckim, na przykład w pliku cookie.

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

1. Użytkownik loguje się do `www.example.com`przy użyciu uwierzytelniania formularzy.
2. Serwer uwierzytelnia użytkownika. Odpowiedź z serwera zawiera plik cookie uwierzytelniania.
3. Bez wylogowywania użytkownik odwiedza złośliwą witrynę sieci Web. Ta złośliwa witryna zawiera następującą postać HTML: 

    [!code-html[Main](introduction-to-security/samples/sample1.html)]

   Należy zauważyć, że w ramach akcji formularza są umieszczane wpisy w zagrożonej witrynie, a nie w złośliwej witrynie. To jest część "wiele witryn" z CSRF.
4. Użytkownik klika przycisk Prześlij. Przeglądarka zawiera plik cookie uwierzytelniania z żądaniem.
5. Żądanie jest uruchamiane na serwerze example.com z kontekstem uwierzytelniania użytkownika i może wykonywać wszystkie czynności, które mogą wykonać uwierzytelnionego użytkownika.

Mimo że ten przykład wymaga, aby użytkownik klikał przycisk formularza, złośliwa strona może jak równie łatwo uruchomić skrypt wysyłający żądanie AJAX do aplikacji sygnalizującej. Ponadto użycie protokołu SSL nie uniemożliwia ataku CSRF, ponieważ złośliwa witryna może wysłać żądanie "https://".

Zazwyczaj ataki CSRF są dostępne dla witryn sieci Web, które używają plików cookie do uwierzytelniania, ponieważ przeglądarki wysyłają wszystkie odpowiednie pliki cookie do docelowej witryny sieci Web. Jednak ataki CSRF nie ograniczają się do korzystania z plików cookie. Na przykład uwierzytelnianie podstawowe i szyfrowane jest również zagrożone. Gdy użytkownik zaloguje się przy użyciu uwierzytelniania podstawowego lub szyfrowanego, przeglądarka automatycznie wyśle poświadczenia do momentu zakończenia sesji.

### <a name="csrf-mitigations-taken-by-signalr"></a>CSRF środki zaradcze wykonywane przez sygnalizującego

Program sygnalizujący wykonuje poniższe kroki, aby uniemożliwić złośliwym lokacjom Tworzenie prawidłowych żądań do aplikacji sygnalizującej. Te kroki są domyślnie podejmowane i nie wymagają żadnych akcji w kodzie.

- **Wyłącz żądania między domenami**  
 Domyślnie żądania między domenami są wyłączone w aplikacji sygnalizującej, aby uniemożliwić użytkownikom wywoływanie punktu końcowego sygnalizującego z domeny zewnętrznej. Wszystkie żądania, które pochodzą z domeny zewnętrznej, są automatycznie traktowane jako nieprawidłowe i zablokowane. Zalecane jest zachowanie tego zachowania domyślnego; w przeciwnym razie złośliwa witryna może dowolnych użytkowników do wysyłania poleceń do witryny. Jeśli konieczne jest użycie żądań między domenami, zobacz [jak ustanowić połączenie między domenami](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain) .
- **Przekaż token połączenia w ciągu zapytania, a nie w pliku cookie**  
 Sygnał przekazuje token połączenia jako wartość ciągu zapytania, a nie jako plik cookie. W przypadku braku przechowywania tokenu połączenia jako pliku cookie token połączenia nie jest przypadkowo przekazywany przez przeglądarkę w przypadku napotkania złośliwego kodu. Ponadto token połączenia nie jest utrwalany poza bieżącym połączeniem. W związku z tym złośliwy użytkownik nie może wykonać żądania w ramach poświadczeń uwierzytelniania innego użytkownika.
- **Weryfikuj token połączenia**  
 Zgodnie z opisem w sekcji [token połączenia](#connectiontoken) serwer wie, który identyfikator połączenia jest skojarzony z każdym uwierzytelnionym użytkownikiem. Serwer nie przetwarza żadnych żądań z identyfikatora połączenia, który nie jest zgodny z nazwą użytkownika. Jest mało prawdopodobne, że złośliwy użytkownik może odgadnąć prawidłowe żądanie, ponieważ złośliwy użytkownik musi znać nazwę użytkownika i bieżący wygenerowany losowo identyfikator połączenia. Ten identyfikator połączenia będzie nieprawidłowy, gdy tylko zakończy się połączenie. Użytkownicy anonimowi nie powinni mieć dostępu do żadnych poufnych informacji.

<a id="recommendations"></a>

## <a name="signalr-security-recommendations"></a>Zalecenia dotyczące zabezpieczeń sygnałów

<a id="ssl"></a>

### <a name="secure-socket-layers-ssl-protocol"></a>Protokół SSL (Secure Sockets)

Protokół SSL używa szyfrowania w celu zabezpieczenia transportu danych między klientem i serwerem. Jeśli aplikacja sygnalizująca przesyła poufne informacje między klientem a serwerem, należy użyć protokołu SSL do transportu. Aby uzyskać więcej informacji o konfigurowaniu protokołu SSL, zobacz [jak skonfigurować protokół SSL w usługach IIS 7](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).

<a id="groupsecurity"></a>

### <a name="do-not-use-groups-as-a-security-mechanism"></a>Nie używaj grup jako mechanizmu zabezpieczeń

Grupy to wygodny sposób zbierania powiązanych użytkowników, ale nie są bezpiecznym mechanizmem ograniczania dostępu do poufnych informacji. Jest to szczególnie prawdziwe, gdy użytkownicy mogą automatycznie odłączać grupy podczas ponownego nawiązywania połączenia. Zamiast tego należy rozważyć dodanie uprzywilejowanych użytkowników do roli i ograniczenie dostępu do metody centrum tylko do członków tej roli. Aby uzyskać przykład ograniczenia dostępu na podstawie roli, zobacz [uwierzytelnianie i autoryzacja dla centrów sygnałów](../security/hub-authorization.md). Aby zapoznać się z przykładem sprawdzania dostępu użytkowników do grup podczas ponownego nawiązywania połączenia, zobacz [Praca z grupami](../guide-to-the-api/working-with-groups.md).

<a id="input"></a>

### <a name="safely-handling-input-from-clients"></a>Bezpieczne obsługiwanie danych wejściowych z klientów

Wszystkie dane wejściowe z klientów, które są przeznaczone do emisji do innych klientów, muszą zostać zakodowane w celu zapewnienia, że złośliwy użytkownik nie wyśle skryptu do innych użytkowników. Najlepszym rozwiązaniem jest kodowanie komunikatów na klientach otrzymujących zamiast na serwerze, ponieważ aplikacja sygnalizująca może mieć wiele różnych typów klientów. W związku z tym kodowanie HTML działa dla klienta sieci Web, ale nie dla innych typów klientów. Na przykład Metoda klienta sieci Web, aby wyświetlić komunikat rozmowy, bezpiecznie obsłużyć nazwę użytkownika i komunikat, wywołując funkcję `html()`.

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

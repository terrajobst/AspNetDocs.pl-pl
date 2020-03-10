---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-server
title: Przewodnik interfejsu API centrów sygnałów ASP.NET — serwerC#() | Microsoft Docs
author: bradygaster
description: Ten dokument zawiera wprowadzenie do programowania po stronie serwera interfejsu API centrów ASP.NETer dla usługi sygnalizującego w wersji 2, z przykładami kodu...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: b19913e5-cd8a-4e4b-a872-5ac7a858a934
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-server
msc.type: authoredcontent
ms.openlocfilehash: c681b104b15bfc4a04587c7abf685dcf20def2ca
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536751"
---
# <a name="aspnet-signalr-hubs-api-guide---server-c"></a>Przewodnik interfejsu API centrów sygnałów ASP.NET — serwerC#()

[Fletcher Patryk](https://github.com/pfletcher), [Tomasz Dykstra](https://github.com/tdykstra)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Ten dokument zawiera wprowadzenie do programowania po stronie serwera interfejsu API centrów ASP.NETer dla usługi sygnalizującego w wersji 2, z przykładami kodu pokazującymi typowe opcje.
> 
> Interfejs API centrów sygnałów umożliwia wykonywanie zdalnych wywołań procedur (RPC) z serwera do podłączonych klientów i od klientów do serwera programu. W polu kod serwera można zdefiniować metody, które mogą być wywoływane przez klientów, i wywoływanie metod uruchamianych na kliencie. W kodzie klienta należy zdefiniować metody, które mogą być wywoływane z serwera programu, i wywoływanie metod, które są uruchamiane na serwerze. Sygnalizujący, że wszystkie instalacje z klientem do serwera są obsługiwane.
> 
> Sygnalizujący oferuje również interfejs API niższego poziomu o nazwie połączeń trwałych. Aby zapoznać się z wprowadzeniem do sygnałów, centrów i połączeń trwałych, zobacz [wprowadzenie do sygnalizującego 2](../getting-started/introduction-to-signalr.md).
> 
> ## <a name="software-versions-used-in-this-topic"></a>Wersje oprogramowania używane w tym temacie
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - Sygnalizujący wersja 2
>   
> 
> 
> ## <a name="topic-versions"></a>Wersje tematów
> 
> Aby uzyskać informacje o wcześniejszych wersjach programu sygnalizującego, zobacz sekcję [sygnalizujące starsze wersje](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Pytania i Komentarze
> 
> Prosimy o opinię na temat sposobu, w jaki lubię ten samouczek, i co możemy ulepszyć w komentarzach w dolnej części strony. Jeśli masz pytania, które nie są bezpośrednio związane z samouczkiem, możesz je ogłosić na [forum ASP.NET](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).

## <a name="overview"></a>Omówienie

Ten dokument zawiera następujące sekcje:

- [Jak zarejestrować oprogramowanie pośredniczące sygnalizujące](#route)

    - [Adres URL/SignalR](#signalrurl)
    - [Konfigurowanie opcji sygnalizujących](#options)
- [Tworzenie i używanie klas centrów](#hubclass)

    - [Okres istnienia obiektu centrum](#transience)
    - [Notacji CamelCase — wielkość liter w nazwach centrów w klientach JavaScript](#hubnames)
    - [Wiele centrów](#multiplehubs)
    - [Centra o jednoznacznie określonym typie](#stronglytypedhubs)
- [Jak zdefiniować metody w klasie centrów, które mogą być wywoływane przez klientów](#hubmethods)

    - [Notacji CamelCase — wielkość liter w nazwach metod w klientach JavaScript](#methodnames)
    - [Kiedy wykonać asynchronicznie](#asyncmethods)
    - [Definiowanie przeciążeń](#overloads)
    - [Raportowanie postępu z wywołań metod centrów](#progress)
- [Jak wywołać metody klienta z klasy Hub](#callfromhub)

    - [Wybieranie klientów, którzy będą otrzymywać wywołania RPC](#selectingclients)
    - [Brak weryfikacji w czasie kompilacji dla nazw metod](#dynamicmethodnames)
    - [Dopasowanie nazw metod bez uwzględniania wielkości liter](#caseinsensitive)
    - [Wykonywanie asynchroniczne](#asyncclient)
- [Zarządzanie członkostwem w grupie z klasy centrów](#groupsfromhub)

    - [Asynchroniczne wykonywanie metod Add i Remove](#asyncgroupmethods)
    - [Trwałość członkostwa w grupie](#grouppersistence)
    - [Grupy pojedynczego użytkownika](#singleusergroups)
- [Jak obsłużyć zdarzenia okresu istnienia połączenia w klasie centrów](#connectionlifetime)

    - [Gdy są wywoływane metody OnConnected, ondisconnected i OnReconnected](#onreconnected)
    - [Stan wywołującego nie został wypełniony](#nocallerstate)
- [Jak uzyskać informacje o kliencie z właściwości kontekstu](#contextproperty)
- [Jak przekazać stan między klientami a klasą centrum](#passstate)
- [Jak obsłużyć błędy w klasie centrów](#handleErrors)
- [Jak wywoływać metody klienta i zarządzać grupami spoza klasy Hub](#callfromoutsidehub)

    - [Wywoływanie metod klienta](#callingclientsoutsidehub)
    - [Zarządzanie członkostwem w grupie](#managinggroupsoutsidehub)
- [Jak włączyć śledzenie](#tracing)
- [Jak dostosować potok centrów](#hubpipeline)

Aby uzyskać dokumentację dotyczącą sposobu programowania klientów, zobacz następujące zasoby:

- [Przewodnik interfejsu API centrów sygnałów — klient JavaScript](hubs-api-guide-javascript-client.md)
- [Przewodnik interfejsu API centrów sygnałów — klient platformy .NET](hubs-api-guide-net-client.md)

Składniki serwera dla sygnalizującego 2 są dostępne tylko w programie .NET 4,5. Na serwerach z programem .NET 4,0 musi być używany program sygnalizujący v1. x.

<a id="route"></a>

## <a name="how-to-register-signalr-middleware"></a>Jak zarejestrować oprogramowanie pośredniczące sygnalizujące

Aby zdefiniować trasę, która będzie używana przez klientów do łączenia się z centrum, wywołaj metodę `MapSignalR` podczas uruchamiania aplikacji. `MapSignalR` jest [metodą rozszerzającą](https://msdn.microsoft.com/library/vstudio/bb383977.aspx) dla klasy `OwinExtensions`. Poniższy przykład pokazuje, jak zdefiniować trasę centrów sygnałów przy użyciu klasy startowej OWIN.

[!code-csharp[Main](hubs-api-guide-server/samples/sample1.cs)]

Jeśli dodajesz funkcję sygnalizującą do aplikacji ASP.NET MVC, upewnij się, że trasa sygnalizująca jest dodawana przed innymi trasami. Aby uzyskać więcej informacji, zobacz [Samouczek: wprowadzenie z sygnałami 2 i MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).

<a id="signalrurl"></a>

### <a name="the-signalr-url"></a>Adres URL/SignalR

Domyślnie adres URL trasy używany przez klientów do łączenia się z centrum to "/SignalR". (Nie należy mylić tego adresu URL z adresem URL "/SignalR/Hubs", który jest przeznaczony dla automatycznie generowanego pliku języka JavaScript. Aby uzyskać więcej informacji na temat wygenerowanego serwera proxy, zobacz [Przewodnik interfejsu API centrów sygnałów — klient JavaScript — wygenerowany serwer proxy i jego zawartość](hubs-api-guide-javascript-client.md#genproxy).

Mogą występować nadzwyczajne sytuacje, w których ten podstawowy adres URL nie może być użyteczny dla sygnalizującego; na przykład w projekcie znajduje się folder o nazwie *sygnalizujący* i nie chcesz zmieniać nazwy. W takim przypadku można zmienić podstawowy adres URL, jak pokazano w poniższych przykładach (Zastąp "/SignalR" w przykładowym kodzie z żądanym adresem URL).

**Kod serwera, który określa adres URL**

[!code-csharp[Main](hubs-api-guide-server/samples/sample2.cs?highlight=1)]

**Kod klienta JavaScript, który określa adres URL (z wygenerowanym serwerem proxy)**

[!code-javascript[Main](hubs-api-guide-server/samples/sample3.js?highlight=1)]

**Kod klienta JavaScript, który określa adres URL (bez wygenerowanego serwera proxy)**

[!code-javascript[Main](hubs-api-guide-server/samples/sample4.js?highlight=1)]

**Kod klienta .NET, który określa adres URL**

[!code-csharp[Main](hubs-api-guide-server/samples/sample5.cs?highlight=1)]

<a id="options"></a>

### <a name="configuring-signalr-options"></a>Konfigurowanie opcji sygnalizujących

Przeciążenia metody `MapSignalR` umożliwiają określenie niestandardowego adresu URL, niestandardowego programu rozpoznawania zależności oraz następujących opcji:

- Włącz wywołania międzydomenowe za pomocą mechanizmu CORS lub JSONP z klientów przeglądarek.

    Zazwyczaj Jeśli przeglądarka ładuje stronę `http://contoso.com`, połączenie sygnalizujące znajduje się w tej samej domenie, w `http://contoso.com/signalr`. Jeśli strona z `http://contoso.com` nawiązać połączenie z `http://fabrikam.com/signalr`, to jest połączenie między domenami. Ze względów bezpieczeństwa połączenia między domenami są domyślnie wyłączone. Aby uzyskać więcej informacji, zobacz [Przewodnik dotyczący interfejsu API centrów ASP.NET Signals — JavaScript Client — jak ustanowić połączenie między domenami](hubs-api-guide-javascript-client.md#crossdomain).
- Włącz szczegółowe komunikaty o błędach.

    Gdy wystąpią błędy, domyślne zachowanie sygnalizującego jest wysyłane do klientów komunikat powiadomienia bez szczegółowych informacji o tym, co się stało. Wysyłanie szczegółowych informacji o błędach do klientów nie jest zalecane w środowisku produkcyjnym, ponieważ Złośliwi użytkownicy mogą korzystać z informacji w przypadku ataków na aplikację. W przypadku rozwiązywania problemów można użyć tej opcji, aby tymczasowo włączyć bardziej informacyjne raportowanie błędów.
- Wyłącz automatycznie generowane pliki proxy JavaScript.

    Domyślnie plik JavaScript z serwerami proxy dla klas centrów jest generowany w odpowiedzi na adres URL "/SignalR/Hubs". Jeśli nie chcesz używać serwerów proxy języka JavaScript lub chcesz wygenerować ten plik ręcznie i zapoznaj się z plikiem fizycznym na klientach, możesz użyć tej opcji, aby wyłączyć generowanie serwera proxy. Aby uzyskać więcej informacji, zobacz [Przewodnik interfejsu API centrów sygnałów — JavaScript Client — jak utworzyć plik fizyczny dla generowanego przez program sygnalizującego serwera proxy](hubs-api-guide-javascript-client.md#manualproxy).

Poniższy przykład pokazuje, jak określić adres URL połączenia sygnalizującego i te opcje w wywołaniu metody `MapSignalR`. Aby określić niestandardowy adres URL, Zastąp ciąg "/SignalR" w przykładzie adresem URL, którego chcesz użyć.

[!code-csharp[Main](hubs-api-guide-server/samples/sample6.cs)]

<a id="hubclass"></a>

## <a name="how-to-create-and-use-hub-classes"></a>Tworzenie i używanie klas centrów

Aby utworzyć centrum, należy utworzyć klasę, która pochodzi od elementu [Microsoft. ASPNET. signaler. Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx). W poniższym przykładzie przedstawiono prostą klasę centrów dla aplikacji czatu.

[!code-csharp[Main](hubs-api-guide-server/samples/sample7.cs)]

W tym przykładzie połączony klient może wywoływać metodę `NewContosoChatMessage`, a w przypadku, gdy dane są wysyłane do wszystkich połączonych klientów.

<a id="transience"></a>

### <a name="hub-object-lifetime"></a>Okres istnienia obiektu centrum

Nie można utworzyć wystąpienia klasy centrum ani wywołać jej metod z własnego kodu na serwerze; wszystko to wykonywane przez potok centrów sygnałów. Program sygnalizujący tworzy nowe wystąpienie klasy centrów za każdym razem, gdy musi obsłużyć operację centrum, taką jak gdy klient łączy się, rozłącza lub wywołuje metodę wywołania do serwera.

Ponieważ wystąpienia klasy centrów są przejściowe, nie można ich używać do utrzymania stanu z jednego wywołania metody do następnego. Za każdym razem, gdy serwer odbiera wywołanie metody z klienta, nowe wystąpienie klasy Hub przetwarza komunikat. Aby zachować stan za pośrednictwem wielu połączeń i wywołań metod, należy użyć innej metody, takiej jak baza danych lub zmienna statyczna w klasie Hub, lub innej klasy, która nie pochodzi od `Hub`. W przypadku utrwalania danych w pamięci przy użyciu metody takiej jak zmienna statyczna w klasie centrów dane zostaną utracone podczas odtwarzania domeny aplikacji.

Jeśli chcesz wysyłać komunikaty do klientów z własnego kodu, który jest uruchamiany poza klasą Hub, nie można tego zrobić przez utworzenie wystąpienia klasy centrum, ale można to zrobić, pobierając odwołanie do obiektu kontekstu sygnalizującego dla klasy centrum. Aby uzyskać więcej informacji, zobacz [jak wywoływać metody klienta i zarządzać grupami spoza klasy Hub](#callfromoutsidehub) w dalszej części tego tematu.

<a id="hubnames"></a>

### <a name="camel-casing-of-hub-names-in-javascript-clients"></a>Notacji CamelCase — wielkość liter w nazwach centrów w klientach JavaScript

Domyślnie klienci języka JavaScript odwołują się do centrów przy użyciu notacji camelcaseej wersji klasy. Program sygnalizujący automatycznie wprowadza tę zmianę, aby kod JavaScript mógł być zgodny z konwencjami języka JavaScript. Poprzedni przykład jest określany jako `contosoChatHub` w kodzie JavaScript.

**Serwer**

[!code-csharp[Main](hubs-api-guide-server/samples/sample8.cs?highlight=1)]

**Klient JavaScript korzystający z wygenerowanego serwera proxy**

[!code-javascript[Main](hubs-api-guide-server/samples/sample9.js?highlight=1)]

Jeśli chcesz określić inną nazwę dla klientów do użycia, Dodaj atrybut `HubName`. W przypadku korzystania z atrybutu `HubName` nie jest zmieniana nazwa notacji CamelCase przypadku na klientach JavaScript.

**Serwer**

[!code-csharp[Main](hubs-api-guide-server/samples/sample10.cs?highlight=1)]

**Klient JavaScript korzystający z wygenerowanego serwera proxy**

[!code-javascript[Main](hubs-api-guide-server/samples/sample11.js?highlight=1)]

<a id="multiplehubs"></a>

### <a name="multiple-hubs"></a>Multiple Hubs

W aplikacji można definiować wiele klas centrów. Po wykonaniu tej czynności połączenie jest udostępniane, ale grupy są osobne:

- Wszyscy klienci będą używać tego samego adresu URL, aby nawiązać połączenie sygnalizujące z usługą ("/SignalR" lub niestandardowy adres URL, jeśli został określony) i że połączenie jest używane dla wszystkich centrów zdefiniowanych przez usługę.

    Nie ma różnicy wydajności dla wielu centrów w porównaniu do definiowania wszystkich funkcji centrum w jednej klasie.
- Wszystkie centra uzyskują te same informacje o żądaniu HTTP.

    Ponieważ wszystkie centra korzystają z tego samego połączenia, jedyną informacją o żądaniu HTTP, którą pobiera serwer, jest to, co znajduje się w oryginalnym żądaniu HTTP, które nawiązuje połączenie sygnalizujące. W przypadku użycia żądania połączenia w celu przekazania informacji z klienta do serwera przez określenie ciągu zapytania nie można podać różnych parametrów zapytania do różnych centrów. Wszystkie centra będą otrzymywać te same informacje.
- Wygenerowany plik proxy języka JavaScript będzie zawierać serwery proxy dla wszystkich centrów w jednym pliku.

    Aby uzyskać informacje na temat serwerów proxy języka JavaScript, zobacz [Przewodnik interfejsu API centrów sygnałów-JavaScript Client-wygenerowany serwer proxy i jego zawartość](hubs-api-guide-javascript-client.md#genproxy).
- Grupy są zdefiniowane w centrach.

    W programie Sygnalizującer można zdefiniować nazwane grupy do emisji do podzestawów połączonych klientów. Grupy są utrzymywane osobno dla każdego centrum. Na przykład grupa o nazwie "Administratorzy" będzie zawierać jeden zestaw klientów dla klasy `ContosoChatHub`, a ta sama nazwa grupy odwołuje się do innego zestawu klientów dla klasy `StockTickerHub`.

<a id="stronglytypedhubs"></a>
### <a name="strongly-typed-hubs"></a>Centra o jednoznacznie określonym typie

Aby zdefiniować interfejs dla metod centrów, do których klient może się odwoływać (i włączyć funkcję IntelliSense w metodach centrum), należy utworzyć centrum z `Hub<T>` (wprowadzone w sygnalizacji 2,1), a nie `Hub`:

[!code-csharp[Main](hubs-api-guide-server/samples/sample12.cs)]

<a id="hubmethods"></a>

## <a name="how-to-define-methods-in-the-hub-class-that-clients-can-call"></a>Jak zdefiniować metody w klasie centrów, które mogą być wywoływane przez klientów

Aby uwidocznić metodę w centrum, która ma być wywoływana z klienta, zadeklaruj metodę publiczną, jak pokazano w poniższych przykładach.

[!code-csharp[Main](hubs-api-guide-server/samples/sample13.cs?highlight=3)]

[!code-csharp[Main](hubs-api-guide-server/samples/sample14.cs?highlight=3)]

Można określić typ zwracany i parametry, w tym typy złożone i tablice, tak jak w przypadku dowolnej C# metody. Wszystkie dane, które są odbierane w parametrach lub zwracane do obiektu wywołującego, są przekazywane między klientem a serwerem przy użyciu JSON, a sygnalizujący obsługuje powiązanie obiektów złożonych i tablic obiektów automatycznie.

<a id="methodnames"></a>

### <a name="camel-casing-of-method-names-in-javascript-clients"></a>Notacji CamelCase — wielkość liter w nazwach metod w klientach JavaScript

Domyślnie klienci języka JavaScript odwołują się do metod centrów przy użyciu notacji camelcaseej wersji metody. Program sygnalizujący automatycznie wprowadza tę zmianę, aby kod JavaScript mógł być zgodny z konwencjami języka JavaScript.

**Serwer**

[!code-csharp[Main](hubs-api-guide-server/samples/sample15.cs?highlight=1)]

**Klient JavaScript korzystający z wygenerowanego serwera proxy**

[!code-javascript[Main](hubs-api-guide-server/samples/sample16.js?highlight=1)]

Jeśli chcesz określić inną nazwę dla klientów do użycia, Dodaj atrybut `HubMethodName`.

**Serwer**

[!code-csharp[Main](hubs-api-guide-server/samples/sample17.cs?highlight=1)]

**Klient JavaScript korzystający z wygenerowanego serwera proxy**

[!code-javascript[Main](hubs-api-guide-server/samples/sample18.js?highlight=1)]

<a id="asyncmethods"></a>

### <a name="when-to-execute-asynchronously"></a>Kiedy wykonać asynchronicznie

Jeśli metoda będzie długotrwała lub musi wykonywać zadania, które mogą być zależne, takie jak wyszukiwanie bazy danych lub wywołanie usługi sieci Web, ustaw metodę Hub asynchronicznie, zwracając [zadanie](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) (zamiast elementu `void` Return) lub [zadania&lt;t&gt;](https://msdn.microsoft.com/library/dd321424.aspx) obiektu (zamiast `T` zwracanego typu). Po powrocie obiektu `Task` z metody, sygnalizujący czeka na ukończenie `Task`, a następnie wysyła nieopakowany wynik z powrotem do klienta, więc nie ma żadnych różnic w sposobie kodowania wywołania metody na kliencie.

Stosowanie metody centrum asynchronicznie zapobiega blokowaniu połączenia w przypadku korzystania z transportu WebSocket. Gdy metoda centrum jest wykonywana synchronicznie, a transport jest w trybie WebSocket, kolejne wywołania metod znajdujących się w koncentratorze z tego samego klienta są blokowane do momentu zakończenia metody centrum.

Poniższy przykład pokazuje tę samą metodę, która jest zakodowana do uruchamiania synchronicznie lub asynchronicznie, po którym następuje kod klienta JavaScript, który działa w przypadku wywoływania jednej wersji.

**Wykonywane**

[!code-csharp[Main](hubs-api-guide-server/samples/sample19.cs)]

**Komunikacji**

[!code-csharp[Main](hubs-api-guide-server/samples/sample20.cs?highlight=1,7-8)]

**Klient JavaScript korzystający z wygenerowanego serwera proxy**

[!code-javascript[Main](hubs-api-guide-server/samples/sample21.js)]

Aby uzyskać więcej informacji o sposobach używania metod asynchronicznych w ASP.NET 4,5, zobacz [Używanie metod asynchronicznych w ASP.NET MVC 4](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).

<a id="overloads"></a>

### <a name="defining-overloads"></a>Definiowanie przeciążeń

Jeśli chcesz zdefiniować przeciążenia dla metody, liczba parametrów w każdym przeciążeniu musi się różnić. W przypadku odróżnienia przeciążenia po określeniu różnych typów parametrów, Klasa centrum zostanie skompilowana, ale usługa sygnalizująca zgłosi wyjątek w czasie wykonywania, gdy klienci spróbują wywołać jedno z przeciążeń.

<a id="progress"></a>
### <a name="reporting-progress-from-hub-method-invocations"></a>Raportowanie postępu z wywołań metod centrów

Sygnał 2,1 dodaje obsługę [wzorca raportowania postępu](https://blogs.msdn.com/b/dotnet/archive/2012/06/06/async-in-4-5-enabling-progress-and-cancellation-in-async-apis.aspx) wprowadzonego w programie .NET 4,5. Aby zaimplementować Raportowanie postępu, zdefiniuj `IProgress<T>` parametr dla metody centrum, do której Twój klient ma dostęp:

[!code-csharp[Main](hubs-api-guide-server/samples/sample22.cs)]

Podczas pisania długotrwałej metody serwera należy użyć asynchronicznego wzorca programowania, takiego jak Async/await, zamiast blokować wątek centrum.

<a id="callfromhub"></a>

## <a name="how-to-call-client-methods-from-the-hub-class"></a>Jak wywołać metody klienta z klasy Hub

Aby wywołać metody klienta z serwera, użyj właściwości `Clients` w metodzie klasy Hub. Poniższy przykład przedstawia kod serwera, który wywołuje `addNewMessageToPage` na wszystkich połączonych klientach i kod klienta, który definiuje metodę w kliencie JavaScript.

**Serwer**

[!code-csharp[Main](hubs-api-guide-server/samples/sample23.cs?highlight=5)]

Wywołanie metody klienta jest operacją asynchroniczną i zwraca `Task`. Użyj `await`:

* , Aby upewnić się, że komunikat jest wysyłany bez błędu. 
* Aby włączyć przechwytywanie i obsłużyć błędy w bloku try-catch.

**Klient JavaScript korzystający z wygenerowanego serwera proxy**

[!code-html[Main](hubs-api-guide-server/samples/sample24.html?highlight=1)]

Nie można uzyskać wartości zwracanej z metody klienta; Składnia, taka jak `int x = Clients.All.add(1,1)`, nie działa.

Można określić typy złożone i tablice dla parametrów. Poniższy przykład przekazuje typ złożony do klienta w parametrze metody.

**Kod serwera, który wywołuje metodę klienta przy użyciu obiektu złożonego**

[!code-csharp[Main](hubs-api-guide-server/samples/sample25.cs?highlight=3)]

**Kod serwera, który definiuje obiekt złożony**

[!code-csharp[Main](hubs-api-guide-server/samples/sample26.cs?highlight=1)]

**Klient JavaScript korzystający z wygenerowanego serwera proxy**

[!code-javascript[Main](hubs-api-guide-server/samples/sample27.js?highlight=2-3)]

<a id="selectingclients"></a>

### <a name="selecting-which-clients-will-receive-the-rpc"></a>Wybieranie klientów, którzy będą otrzymywać wywołania RPC

Właściwość klienci zwraca obiekt [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) , który udostępnia kilka opcji określania klientów, którzy będą otrzymywać wywołania RPC:

- Wszyscy połączeni klienci.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample28.cs)]
- Tylko klient wywołujący.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample29.cs)]
- Wszyscy klienci z wyjątkiem klienta wywołującego.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample30.cs)]
- Określony klient identyfikowany przez identyfikator połączenia.

    [!code-css[Main](hubs-api-guide-server/samples/sample31.css)]

    Ten przykład wywołuje `addContosoChatMessageToPage` na kliencie wywołującym i ma ten sam efekt, co użycie `Clients.Caller`.
- Wszyscy połączeni klienci z wyjątkiem określonych klientów identyfikowane przez identyfikator połączenia.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample32.cs)]
- Wszyscy połączeni klienci w określonej grupie.

    [!code-css[Main](hubs-api-guide-server/samples/sample33.css)]
- Wszyscy połączeni klienci w określonej grupie z wyjątkiem określonych klientów identyfikowane przez identyfikator połączenia.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample34.cs)]
- Wszyscy połączeni klienci w określonej grupie z wyjątkiem klienta wywołującego.

    [!code-css[Main](hubs-api-guide-server/samples/sample35.css)]
- Określony użytkownik identyfikowany według identyfikatora użytkownika.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample36.cs)]

    Domyślnie jest to `IPrincipal.Identity.Name`, ale można to zmienić, [rejestrując implementację IUserIdProvider z hostem globalnym](mapping-users-to-connections.md#IUserIdProvider).
- Wszyscy klienci i grupy na liście identyfikatorów połączeń.

    [!code-css[Main](hubs-api-guide-server/samples/sample37.css)]
- Lista grup.

    [!code-css[Main](hubs-api-guide-server/samples/sample38.css)]
- Użytkownik według nazwy.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample39.cs)]
- Lista nazw użytkowników (wprowadzona w sygnalizacji 2,1).

    [!code-csharp[Main](hubs-api-guide-server/samples/sample40.cs)]

<a id="dynamicmethodnames"></a>

### <a name="no-compile-time-validation-for-method-names"></a>Brak weryfikacji w czasie kompilacji dla nazw metod

Określona nazwa metody jest interpretowana jako obiekt dynamiczny, co oznacza, że nie istnieje funkcja IntelliSense ani weryfikacja w czasie kompilacji. Wyrażenie jest oceniane w czasie wykonywania. Gdy wywołanie metody jest wykonywane, sygnalizujący wysyła nazwę metody i wartości parametrów do klienta, a jeśli klient ma metodę zgodną z nazwą, ta metoda jest wywoływana i są do niej przenoszone wartości parametrów. Jeśli na kliencie nie zostanie znaleziona zgodna Metoda, żaden błąd nie zostanie zgłoszony. Aby uzyskać informacje na temat formatu danych przesyłanych do klienta przez program sygnalizujący w tle podczas wywoływania metody klienta, zobacz [wprowadzenie do usługi sygnalizującej](../getting-started/introduction-to-signalr.md).

<a id="caseinsensitive"></a>

### <a name="case-insensitive-method-name-matching"></a>Dopasowanie nazw metod bez uwzględniania wielkości liter

W dopasowaniu nazw metod nie jest rozróżniana wielkość liter. Na przykład `Clients.All.addContosoChatMessageToPage` na serwerze wykona `AddContosoChatMessageToPage`, `addcontosochatmessagetopage`lub `addContosoChatMessageToPage` na kliencie.

<a id="asyncclient"></a>

### <a name="asynchronous-execution"></a>Wykonywanie asynchroniczne

Wywoływana metoda jest wykonywana asynchronicznie. Każdy kod, który jest wywoływany po wywołaniu metody do klienta, będzie wykonywany natychmiast bez oczekiwania na zakończenie przesyłania danych do klientów przez program sygnalizujący, chyba że określisz, że kolejne wiersze kodu powinny czekać na ukończenie metody. Poniższy przykład kodu pokazuje, jak wykonać dwie metody klienta sekwencyjnie.

**Używanie oczekiwania (.NET 4,5)**

[!code-csharp[Main](hubs-api-guide-server/samples/sample41.cs?highlight=1,3)]

Jeśli używasz `await`, aby poczekać, aż Metoda klienta zakończy się przed wykonaniem kolejnego wiersza kodu, nie oznacza to, że klienci otrzymają komunikat przed wykonaniem kolejnego wiersza kodu. "Uzupełnianie" wywołania metody klienta oznacza tylko, że sygnalizujący zrobił wszystko niezbędne do wysłania wiadomości. Jeśli potrzebujesz weryfikacji, że klienci otrzymali komunikat, musisz samodzielnie programować ten mechanizm. Można na przykład zakodować metodę `MessageReceived` w centrum, a w metodzie `addContosoChatMessageToPage` na kliencie można wywołać `MessageReceived` po wykonaniu jakiejkolwiek czynności, którą trzeba wykonać na kliencie. W `MessageReceived` w centrum można wykonać dowolną czynność, która zależy od rzeczywistego odbioru klienta i przetwarzania oryginalnego wywołania metody.

### <a name="how-to-use-a-string-variable-as-the-method-name"></a>Jak używać zmiennej ciągu jako nazwy metody

Jeśli chcesz wywołać metodę klienta przy użyciu zmiennej ciągu jako nazwy metody, Cast `Clients.All` (lub `Clients.Others`, `Clients.Caller`itd.), aby `IClientProxy`, a następnie wywołać wywołania [(MethodName, args...)](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).

[!code-csharp[Main](hubs-api-guide-server/samples/sample42.cs)]

<a id="groupsfromhub"></a>

## <a name="how-to-manage-group-membership-from-the-hub-class"></a>Zarządzanie członkostwem w grupie z klasy centrów

Grupy w sygnalizacji zapewniają metodę rozgłaszania komunikatów do określonych podzestawów połączonych klientów. Grupa może mieć dowolną liczbę klientów, a klient może być członkiem dowolnej liczby grup.

Aby zarządzać członkostwem w grupie, użyj metod [Add](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) i [Remove](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) dostarczonych przez właściwość `Groups` klasy Hub. W poniższym przykładzie przedstawiono metody `Groups.Add` i `Groups.Remove` używane w metodach centralnych, które są wywoływane przez kod klienta, a następnie kod klienta JavaScript, który je wywołuje.

**Serwer**

[!code-csharp[Main](hubs-api-guide-server/samples/sample43.cs?highlight=5,10)]

**Klient JavaScript korzystający z wygenerowanego serwera proxy**

[!code-javascript[Main](hubs-api-guide-server/samples/sample44.js)]

[!code-javascript[Main](hubs-api-guide-server/samples/sample45.js)]

Nie musisz jawnie tworzyć grup. W efekcie Grupa jest tworzona automatycznie przy pierwszym określeniu jej nazwy w wywołaniu `Groups.Add`i zostanie usunięta po usunięciu ostatniego połączenia z członkostwa w nim.

Brak interfejsu API do uzyskiwania listy członkostwa w grupie lub listy grup. Program sygnalizujący wysyła komunikaty do klientów i grup na podstawie [modelu pub/sub](http://en.wikipedia.org/wiki/Publish/subscribe), a serwer nie zachowuje list grup ani członkostw w grupach. Pozwala to zmaksymalizować skalowalność, ponieważ po dodaniu węzła do kolektywu serwerów sieci Web, każdy stan, który utrzymuje usługa sygnalizująca, musi być propagowany do nowego węzła.

<a id="asyncgroupmethods"></a>

### <a name="asynchronous-execution-of-add-and-remove-methods"></a>Asynchroniczne wykonywanie metod Add i Remove

Metody `Groups.Add` i `Groups.Remove` wykonują asynchronicznie. Jeśli chcesz dodać klienta do grupy i natychmiast wysłać komunikat do klienta przy użyciu grupy, musisz upewnić się, że metoda `Groups.Add` została najpierw ukończona. Poniższy przykład kodu pokazuje, jak to zrobić.

**Dodawanie klienta do grupy, a następnie wysyłanie komunikatów do klienta**

[!code-csharp[Main](hubs-api-guide-server/samples/sample46.cs?highlight=1,3)]

<a id="grouppersistence"></a>

### <a name="group-membership-persistence"></a>Trwałość członkostwa w grupie

Program sygnalizujący śledzi połączenia, a nie użytkowników, dlatego jeśli chcesz, aby użytkownik znajdował się w tej samej grupie za każdym razem, gdy użytkownik nawiąże połączenie, należy wywołać `Groups.Add` za każdym razem, gdy użytkownik ustanowi nowe połączenie.

Po tymczasowej utracie łączności czasami program sygnalizujący może automatycznie przywrócić połączenie. W takim przypadku sygnalizujący przywraca to samo połączenie, nie ustanawia nowego połączenia, a więc członkostwo w grupie klienta zostanie automatycznie przywrócone. Jest to możliwe nawet wtedy, gdy tymczasowy przerwanie jest wynikiem ponownego uruchomienia serwera lub błędu, ponieważ stan połączenia dla każdego klienta, w tym członkostwa w grupie, jest przełączany do klienta. Jeśli serwer ulegnie awarii i zostanie zastąpiony przez nowy serwer przed przekroczeniem limitu czasu, klient będzie mógł automatycznie połączyć się z nowym serwerem i ponownie zarejestrować w grupach, które jest członkiem.

Gdy połączenie nie może zostać przywrócone automatycznie po utracie łączności lub gdy upłynął limit czasu połączenia lub gdy klient odłączy się (na przykład gdy przeglądarka nawiguje do nowej strony), członkostwo w grupach zostanie utracone. Następnym razem, gdy użytkownik łączy się z nowym połączeniem. Aby zachować członkostwo w grupach, gdy ten sam użytkownik ustanowi nowe połączenie, aplikacja musi śledzić skojarzenia między użytkownikami i grupami oraz przywracać członkostwa w grupach za każdym razem, gdy użytkownik ustanowi nowe połączenie.

Więcej informacji o połączeniach i repołączeniach znajduje się [w temacie jak obsłużyć zdarzenia okresu istnienia połączenia w klasie centrów](#connectionlifetime) w dalszej części tego tematu.

<a id="singleusergroups"></a>

### <a name="single-user-groups"></a>Grupy pojedynczego użytkownika

Aplikacje korzystające z programu sygnalizującego zwykle muszą śledzić skojarzenia między użytkownikami i połączeniami, aby wiedzieć, który użytkownik wysłał komunikat i które użytkownicy powinni otrzymywać wiadomości. Grupy są używane w jednym z dwóch często używanych wzorców w celu wykonania tej operacji.

- Grupy pojedynczego użytkownika.

    Możesz określić nazwę użytkownika jako nazwę grupy i dodać bieżący identyfikator połączenia do grupy za każdym razem, gdy użytkownik łączy się lub ponownie nawiązuje połączenie. Wysyłanie komunikatów do użytkownika, który jest wysyłany do grupy. Wadą tej metody jest to, że grupa nie pozwala na ustalenie, czy użytkownik jest w trybie online, czy offline.
- Śledź skojarzenia między nazwami użytkowników i identyfikatorami połączeń.

    Można przechowywać skojarzenie między nazwami użytkowników i co najmniej jednym identyfikatorem połączenia w słowniku lub bazie danych i aktualizować przechowywane dane za każdym razem, gdy użytkownik nawiązuje połączenie lub rozłącza. Aby wysyłać komunikaty do użytkownika, należy określić identyfikatory połączeń. Wadą tej metody jest to, że zajmuje więcej pamięci.

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events-in-the-hub-class"></a>Jak obsłużyć zdarzenia okresu istnienia połączenia w klasie centrów

Typowymi przyczynami obsługi zdarzeń okresu istnienia połączenia są śledzenie tego, czy użytkownik jest połączony, i śledzenie skojarzenia między nazwami użytkowników i identyfikatorami połączeń. Aby uruchomić własny kod, gdy klienci nawiązują połączenie lub rozłączają, Zastąp metody wirtualne `OnConnected`, `OnDisconnected`i `OnReconnected` klasy Hub, jak pokazano w poniższym przykładzie.

[!code-csharp[Main](hubs-api-guide-server/samples/sample47.cs?highlight=3,14,22)]

<a id="onreconnected"></a>

### <a name="when-onconnected-ondisconnected-and-onreconnected-are-called"></a>Gdy są wywoływane metody OnConnected, ondisconnected i OnReconnected

Za każdym razem, gdy przeglądarka nawiguje do nowej strony, należy nawiązać nowe połączenie, co oznacza, że sygnalizujący wykona metodę `OnDisconnected`, a następnie metodę `OnConnected`. Sygnalizujący zawsze tworzy nowy identyfikator połączenia po nawiązaniu nowego połączenia.

Metoda `OnReconnected` jest wywoływana, gdy nastąpiło tymczasowe przerwanie łączności, z której sygnalizujący mogą automatycznie odzyskiwać dane, na przykład gdy kabel jest tymczasowo odłączony i ponownie połączony przed upływem limitu czasu połączenia. Metoda `OnDisconnected` jest wywoływana, gdy klient zostanie odłączony, a sygnalizujący nie może automatycznie ponownie nawiązać połączenia, na przykład gdy przeglądarka nawiguje do nowej strony. W związku z tym możliwe jest `OnConnected`, `OnReconnected`, `OnDisconnected`; lub `OnConnected`, `OnDisconnected`. Nie zobaczysz sekwencji `OnConnected`, `OnDisconnected``OnReconnected` dla danego połączenia.

Metoda `OnDisconnected` nie jest wywoływana w niektórych scenariuszach, na przykład w przypadku awarii serwera lub odtworzenia domeny aplikacji. Gdy inny serwer jest w trybie online lub domena aplikacji zakończy jego odtwarzanie, niektórzy klienci mogą być w stanie ponownie nawiązać połączenie i uruchomić zdarzenie `OnReconnected`.

Aby uzyskać więcej informacji, zobacz [Omówienie i obsługa zdarzeń okresu istnienia połączenia w programie sygnalizującym](handling-connection-lifetime-events.md).

<a id="nocallerstate"></a>

### <a name="caller-state-not-populated"></a>Stan wywołującego nie został wypełniony

Metody obsługi zdarzeń okresu istnienia połączenia są wywoływane z serwera, co oznacza, że każdy stan umieszczony w obiekcie `state` na kliencie nie zostanie wypełniony we właściwości `Caller` na serwerze. Aby uzyskać informacje na temat obiektu `state` i właściwości `Caller`, zobacz [jak przekazać stan między klientami a klasą centrum](#passstate) w dalszej części tego tematu.

<a id="contextproperty"></a>

## <a name="how-to-get-information-about-the-client-from-the-context-property"></a>Jak uzyskać informacje o kliencie z właściwości kontekstu

Aby uzyskać informacje o kliencie, użyj właściwości `Context` klasy Hub. Właściwość `Context` zwraca obiekt [HubCallerContext](https://msdn.microsoft.com/library/jj890883(v=vs.111).aspx) , który zapewnia dostęp do następujących informacji:

- Identyfikator połączenia klienta wywołującego.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample48.cs?highlight=1)]

    Identyfikator połączenia jest identyfikatorem GUID, który jest przypisany przez sygnalizujący (nie można określić wartości w własnym kodzie). Istnieje jeden identyfikator połączenia dla każdego połączenia, a ten sam identyfikator połączenia jest używany przez wszystkie centra, jeśli w aplikacji znajduje się wiele centrów.
- Dane nagłówka HTTP.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample49.cs?highlight=1)]

    Można również uzyskać nagłówki HTTP z `Context.Headers`. Przyczyną wielu odwołań do tego samego elementu jest to, że `Context.Headers` został utworzony jako pierwszy, właściwość `Context.Request` została dodana później i `Context.Headers` została zachowana w celu zapewnienia zgodności z poprzednimi wersjami.
- Dane ciągu zapytania.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample50.cs?highlight=1)]

    Możesz również uzyskać dane ciągu zapytania z `Context.QueryString`.

    Ciąg zapytania, który jest pobierany w tej właściwości, jest używany z żądaniem HTTP, które ustanowiło połączenie sygnalizujące. Parametry ciągu zapytania można dodać do klienta przez skonfigurowanie połączenia, które jest wygodnym sposobem przekazywania danych dotyczących klienta z klienta do serwera programu. W poniższym przykładzie pokazano jeden ze sposobów dodawania ciągu zapytania do klienta JavaScript w przypadku korzystania z wygenerowanego serwera proxy.

    [!code-javascript[Main](hubs-api-guide-server/samples/sample51.js?highlight=1)]

    Aby uzyskać więcej informacji na temat ustawiania parametrów ciągu zapytania, zobacz Przewodniki interfejsu API dla klientów [JavaScript](hubs-api-guide-javascript-client.md) i [.NET](hubs-api-guide-net-client.md) .

    Metodę transportu używaną w połączeniu można znaleźć w danych ciągu zapytania, wraz z innymi wartościami używanymi wewnętrznie przez program sygnalizujący:

    [!code-csharp[Main](hubs-api-guide-server/samples/sample52.cs)]

    Wartość `transportMethod` będzie równa "WebSockets", "serverSentEvents", "foreverFrame" lub "longPolling". Należy pamiętać, że jeśli ta wartość zostanie sprawdzona w metodzie obsługi zdarzeń `OnConnected`, w niektórych scenariuszach można początkowo uzyskać wartość transportu, która nie jest ostateczną negocjowaną metodą transportu dla połączenia. W takim przypadku Metoda zgłosi wyjątek i zostanie wywołana ponownie później podczas ustanowienia końcowej metody transportu.
- Cookie.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample53.cs?highlight=1)]

    Możesz również pobrać pliki cookie z `Context.RequestCookies`.
- Informacje o użytkowniku.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample54.cs?highlight=1)]
- Obiekt HttpContext dla żądania:

    [!code-csharp[Main](hubs-api-guide-server/samples/sample55.cs?highlight=1)]

    Użyj tej metody zamiast uzyskać `HttpContext.Current`, aby uzyskać obiekt `HttpContext` dla połączenia sygnalizującego.

<a id="passstate"></a>

## <a name="how-to-pass-state-between-clients-and-the-hub-class"></a>Jak przekazać stan między klientami a klasą centrum

Serwer proxy klienta udostępnia obiekt `state`, w którym można przechowywać dane, które mają być przesyłane do serwera przy użyciu każdego wywołania metody. Na serwerze można uzyskać dostęp do tych danych we właściwości `Clients.Caller` w metodach centralnych wywoływanych przez klientów. Właściwość `Clients.Caller` nie jest wypełniona dla metod obsługi zdarzeń okresu istnienia połączenia `OnConnected`, `OnDisconnected`i `OnReconnected`.

Tworzenie lub aktualizowanie danych w obiekcie `state` i Właściwość `Clients.Caller` działa w obu kierunkach. Można aktualizować wartości na serwerze i są one przesyłane z powrotem do klienta programu.

Poniższy przykład przedstawia kod klienta JavaScript, który przechowuje stan przesyłania do serwera przy każdym wywołaniu metody.

[!code-javascript[Main](hubs-api-guide-server/samples/sample56.js?highlight=1-2)]

Poniższy przykład pokazuje odpowiedni kod w kliencie .NET.

[!code-csharp[Main](hubs-api-guide-server/samples/sample57.cs?highlight=1-2)]

W klasie centrów możesz uzyskać dostęp do tych danych we właściwości `Clients.Caller`. Poniższy przykład pokazuje kod, który pobiera stan, do którego odwołuje się w poprzednim przykładzie.

[!code-csharp[Main](hubs-api-guide-server/samples/sample58.cs?highlight=3-4)]

> [!NOTE]
> Ten mechanizm dla stanu utrwalania nie jest przeznaczony dla dużych ilości danych, ponieważ wszystkie elementy, które zostały umieszczone w `state` lub właściwości `Clients.Caller`, są wyłączane z każdym wywołaniem metody. Jest to przydatne w przypadku mniejszych elementów, takich jak nazwy użytkowników lub liczniki.

W VB.NET lub w koncentratorze z jednoznacznie określonym typem nie można uzyskać dostępu do obiektu stanu wywołującego za pomocą `Clients.Caller`; Zamiast tego należy użyć `Clients.CallerState` (wprowadzona w sygnalizacji 2,1):

**Używanie CallerState wC#**

[!code-csharp[Main](hubs-api-guide-server/samples/sample59.cs?highlight=3-4)]

**Używanie CallerState w Visual Basic**

[!code-vb[Main](hubs-api-guide-server/samples/sample60.vb)]

<a id="handleErrors"></a>

## <a name="how-to-handle-errors-in-the-hub-class"></a>Jak obsłużyć błędy w klasie centrów

Aby obsłużyć błędy występujące w metodach klasy centrum, należy najpierw upewnić się, że wszystkie wyjątki od operacji asynchronicznych (takich jak wywoływanie metod klienta) za pomocą `await`. Następnie użyj co najmniej jednej z następujących metod:

- Zawiń kod metody w blokach try-catch i zarejestruj obiekt wyjątku. Na potrzeby debugowania można wysłać wyjątek do klienta, ale ze względów bezpieczeństwa nie zaleca się wysyłania szczegółowych informacji do klientów w środowisku produkcyjnym.
- Utwórz moduł potoków centrów, który obsługuje metodę [OnIncomingError](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx) . W poniższym przykładzie przedstawiono moduł potoku, który rejestruje błędy, a następnie kod w Startup.cs, który wprowadza moduł do potoku centrów.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample61.cs)]

    [!code-csharp[Main](hubs-api-guide-server/samples/sample62.cs?highlight=4)]
- Użyj klasy `HubException` (wprowadzona w sygnalizacji 2). Ten błąd może zostać zgłoszony z dowolnego wywołania piasty. Konstruktor `HubError` przyjmuje komunikat ciągu i obiekt do przechowywania dodatkowych danych o błędach. Sygnalizujący będzie autoserializować wyjątek i wyśle go do klienta, gdzie zostanie użyty do odrzucenia lub niepowodzenia wywołania metody centrum.

    Poniższy przykład kodu pokazuje, jak zgłosić `HubException` podczas wywołania centrum oraz jak obsłużyć wyjątek na klientach JavaScript i .NET.

    **Kod serwera pokazujący klasę HubException**

    [!code-csharp[Main](hubs-api-guide-server/samples/sample63.cs)]

    **Kod klienta JavaScript pokazujący odpowiedź na wyrzucanie HubException w centrum**

    [!code-html[Main](hubs-api-guide-server/samples/sample64.html)]

    **Kod klienta platformy .NET pokazujący odpowiedź na wyrzucanie HubException w centrum**

    [!code-csharp[Main](hubs-api-guide-server/samples/sample65.cs)]

Więcej informacji o modułach potoku centrów znajduje się w sekcji [jak dostosować potok centrów](#hubpipeline) w dalszej części tego tematu.

<a id="tracing"></a>

## <a name="how-to-enable-tracing"></a>Jak włączyć śledzenie

Aby włączyć śledzenie po stronie serwera, Dodaj element System. Diagnostics do pliku Web. config, jak pokazano w tym przykładzie:

[!code-html[Main](hubs-api-guide-server/samples/sample66.html?highlight=17-72)]

Po uruchomieniu aplikacji w programie Visual Studio można wyświetlić dzienniki w oknie **danych wyjściowych** .

<a id="callfromoutsidehub"></a>

## <a name="how-to-call-client-methods-and-manage-groups-from-outside-the-hub-class"></a>Jak wywoływać metody klienta i zarządzać grupami spoza klasy Hub

Aby wywołać metody klienta z innej klasy niż Klasa Hub, Pobierz odwołanie do obiektu kontekstu sygnalizującego dla centrum i użyj go do wywołania metod na kliencie lub zarządzania grupami.

Poniższa przykładowa Klasa `StockTicker` pobiera obiekt kontekstu, zapisuje go w wystąpieniu klasy, zapisuje wystąpienie klasy w statycznej właściwości i używa kontekstu z wystąpienia klasy pojedynczej w celu wywołania metody `updateStockPrice` na klientach podłączonych do centrum o nazwie `StockTickerHub`.

[!code-csharp[Main](hubs-api-guide-server/samples/sample67.cs?highlight=8,24)]

Jeśli konieczne jest wielokrotne użycie kontekstu w obiekcie długim czasie, Pobierz odwołanie raz i Zapisz je, a nie ponownie za każdym razem. Uzyskanie kontekstu polega na tym, że sygnalizujący wyśle komunikaty do klientów w tej samej kolejności, w której metody centrum umożliwiają wywoływanie metody klienta. Aby zapoznać się z samouczkiem, który pokazuje, jak używać kontekstu sygnalizującego dla centrum, zobacz [serwer Broadcast with ASP.NET signaler](../getting-started/tutorial-server-broadcast-with-signalr.md).

<a id="callingclientsoutsidehub"></a>

### <a name="calling-client-methods"></a>Wywoływanie metod klienta

Można określić, którzy klienci będą otrzymywać wywołania RPC, ale jest mniej opcji niż w przypadku wywołania z klasy centrum. Przyczyną tego jest to, że kontekst nie jest skojarzony z konkretnym wywołaniem z klienta, dlatego wszelkie metody, które wymagają znajomości bieżącego identyfikatora połączenia, takiego jak `Clients.Others`, `Clients.Caller`lub `Clients.OthersInGroup`, nie są dostępne. Dostępne są następujące opcje:

- Wszyscy połączeni klienci.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample68.cs)]
- Określony klient identyfikowany przez identyfikator połączenia.

    [!code-css[Main](hubs-api-guide-server/samples/sample69.css)]
- Wszyscy połączeni klienci z wyjątkiem określonych klientów identyfikowane przez identyfikator połączenia.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample70.cs)]
- Wszyscy połączeni klienci w określonej grupie.

    [!code-css[Main](hubs-api-guide-server/samples/sample71.css)]
- Wszyscy połączeni klienci w określonej grupie z wyjątkiem określonych klientów identyfikowane przez identyfikator połączenia.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample72.cs)]

Jeśli wywołujesz klasę niebędącą centrami z metod w klasie centrów, możesz przekazać bieżący identyfikator połączenia i użyć go z `Clients.Client`, `Clients.AllExcept`lub `Clients.Group`, aby symulować `Clients.Caller`, `Clients.Others`lub `Clients.OthersInGroup`. W poniższym przykładzie Klasa `MoveShapeHub` przekazuje identyfikator połączenia do klasy `Broadcaster`, dzięki czemu Klasa `Broadcaster` może symulować `Clients.Others`.

[!code-csharp[Main](hubs-api-guide-server/samples/sample73.cs?highlight=12,36)]

<a id="managinggroupsoutsidehub"></a>

### <a name="managing-group-membership"></a>Zarządzanie członkostwem w grupie

W przypadku zarządzania grupami te same opcje są takie same jak w klasie centrów.

- Dodawanie klienta do grupy

    [!code-csharp[Main](hubs-api-guide-server/samples/sample74.cs)]
- Usuwanie klienta z grupy

    [!code-css[Main](hubs-api-guide-server/samples/sample75.css)]

<a id="hubpipeline"></a>

## <a name="how-to-customize-the-hubs-pipeline"></a>Jak dostosować potok centrów

Sygnalizujący umożliwia wprowadzanie własnego kodu do potoku centrum. W poniższym przykładzie przedstawiono niestandardowy moduł potoku, który rejestruje każde wywołanie metody przychodzącej odebrane od klienta i wywołania metody wychodzącej wywołanego na kliencie:

[!code-csharp[Main](hubs-api-guide-server/samples/sample76.cs)]

Następujący kod w pliku *Startup.cs* rejestruje moduł do uruchomienia w potoku centrum:

[!code-csharp[Main](hubs-api-guide-server/samples/sample77.cs?highlight=3)]

Istnieje wiele różnych metod, które można przesłonić. Aby uzyskać pełną listę, zobacz [metody HubPipelineModule](https://msdn.microsoft.com/library/jj918633(v=vs.111).aspx).

---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-net-client
title: Przewodnik interfejsu API centrów sygnałów ASP.NET — klient platformyC#.NET () | Microsoft Docs
author: bradygaster
description: Ten dokument zawiera wprowadzenie do korzystania z interfejsu API centrów dla programu sygnalizującego w wersji 2 na klientach platformy .NET, takich jak Windows Store (WinRT), WPF, Silverlight i wady...
ms.author: bradyg
ms.date: 01/15/2019
ms.assetid: 6d02d9f7-94e5-4140-9f51-5a6040f274f6
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-net-client
msc.type: authoredcontent
ms.openlocfilehash: d3536f1c15cd7dad7cd660becf0577e5c131f707
ms.sourcegitcommit: 295cf898a4c87e264b0c35c7254b0fa4169f2278
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/13/2019
ms.locfileid: "74057010"
---
# <a name="aspnet-signalr-hubs-api-guide---net-client-c"></a>Przewodnik interfejsu API centrów sygnałów ASP.NET — klient platformyC#.NET ()

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Ten dokument zawiera wprowadzenie do korzystania z interfejsu API centrów dla programu sygnalizującego w wersji 2 na klientach platformy .NET, takich jak Windows Store (WinRT), WPF, Silverlight i aplikacje konsolowe.
>
> Interfejs API centrów sygnałów umożliwia wykonywanie zdalnych wywołań procedur (RPC) z serwera do podłączonych klientów i od klientów do serwera programu. W polu kod serwera można zdefiniować metody, które mogą być wywoływane przez klientów, i wywoływanie metod uruchamianych na kliencie. W kodzie klienta należy zdefiniować metody, które mogą być wywoływane z serwera programu, i wywoływanie metod, które są uruchamiane na serwerze. Sygnalizujący, że wszystkie instalacje z klientem do serwera są obsługiwane.
>
> Sygnalizujący oferuje również interfejs API niższego poziomu o nazwie połączeń trwałych. Aby zapoznać się z wprowadzeniem do sygnałów, centrów i połączeń trwałych lub samouczka, w którym pokazano, jak utworzyć kompletną aplikację sygnalizującą, zobacz [sygnalizującer-wprowadzenie](../getting-started/index.md).
>
> ## <a name="software-versions-used-in-this-topic"></a>Wersje oprogramowania używane w tym temacie
>
>
> - [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)
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

- [Konfiguracja klienta](#clientsetup)
- [Jak nawiązać połączenie](#establishconnection)

    - [Połączenia między domenami z klientów Silverlight](#slcrossdomain)
- [Jak skonfigurować połączenie](#configureconnection)

    - [Jak ustawić maksymalną liczbę jednoczesnych połączeń w klientach WPF](#maxconnections)
    - [Jak określić parametry ciągu zapytania](#querystring)
    - [Jak określić metodę transportu](#transport)
    - [Jak określić nagłówki HTTP](#httpheaders)
    - [Jak określić certyfikaty klienta](#clientcertificate)
- [Jak utworzyć serwer proxy centrum](#proxy)
- [Jak zdefiniować metody na kliencie, które mogą być wywoływane przez serwer](#callclient)

    - [Metody bez parametrów](#clientmethodswithoutparms)
    - [Metody z parametrami, Określanie typów parametrów](#clientmethodswithparmtypes)
    - [Metody z parametrami, określając dynamiczne obiekty parametrów](#clientmethodswithdynamparms)
    - [Jak usunąć procedurę obsługi](#removehandler)
- [Jak wywoływać metody serwera z klienta](#callserver)
- [Jak obsługiwać zdarzenia okresu istnienia połączenia](#connectionlifetime)
- [Jak obsłużyć błędy](#handleerrors)
- [Jak włączyć rejestrowanie po stronie klienta](#logging)
- [Przykłady kodu aplikacji WPF, Silverlight i konsoli dla metod klienta wywoływanych przez serwer](#wpfsl)

Przykładowe projekty klienta platformy .NET można znaleźć w następujących zasobach:

- [Gustavo-Armenta/sygnalizujący — przykłady](https://github.com/gustavo-armenta/SignalR-Samples) w GitHub.com (WinRT, Silverlight, przykłady aplikacji konsolowych).
- [DamianEdwards/Signal-MoveShapeDemo/MoveShape. Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) on GitHub.com (przykład WPF).
- [Signaler/Microsoft. ASPNET. Signal. Client. Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) in GitHub.com (przykładowa aplikacja konsoli).

Aby uzyskać dokumentację dotyczącą sposobu programowania serwera lub klientów JavaScript, zobacz następujące zasoby:

- [Przewodnik interfejsu API centrów sygnałów — serwer](hubs-api-guide-server.md)
- [Przewodnik interfejsu API centrów sygnałów — klient JavaScript](hubs-api-guide-javascript-client.md)

Łącza do tematów dotyczących odwołań do interfejsów API znajdują się w wersji programu .NET 4,5 interfejsu API. Jeśli używasz programu .NET 4, zobacz [temat wersja interfejsu API w wersji .NET 4](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).

<a id="clientsetup"></a>

## <a name="client-setup"></a>Konfiguracja klienta

Zainstaluj pakiet NuGet [Microsoft. ASPNET. Signal. Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) (nie pakiet [Microsoft. ASPNET. signaler](http://nuget.org/packages/microsoft.aspnet.signalr) ). Ten pakiet obsługuje klientów WinRT, Silverlight, WPF, aplikacji konsolowych i Windows Phone dla programów .NET 4 i .NET 4,5.

Jeśli wersja usługi sygnalizująca, która jest zainstalowana na kliencie, różni się od wersji programu znajdującej się na serwerze, sygnał jest często możliwy do dostosowania do różnic. Na przykład serwer z uruchomionym programem sygnalizującym w wersji 2 będzie obsługiwał klientów programu, na których zainstalowano 1.1. x, a także klientów z zainstalowaną wersją 2. Jeśli różnica między wersją na serwerze i wersją na kliencie jest zbyt duża lub jeśli klient jest nowszy niż serwer, sygnalizujący zgłasza wyjątek `InvalidOperationException`, gdy klient próbuje nawiązać połączenie. Komunikat o błędzie to "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`".

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a>Jak nawiązać połączenie

Aby można było nawiązać połączenie, należy utworzyć obiekt `HubConnection` i utworzyć serwer proxy. Aby nawiązać połączenie, wywołaj metodę `Start` na obiekcie `HubConnection`.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample1.cs?highlight=1,5)]

> [!NOTE]
> W przypadku klientów JavaScript należy zarejestrować co najmniej jeden program obsługi zdarzeń przed wywołaniem metody `Start` w celu nawiązania połączenia. Nie jest to konieczne w przypadku klientów platformy .NET. W przypadku klientów JavaScript wygenerowany kod serwera proxy automatycznie tworzy serwery proxy dla wszystkich centrów istniejących na serwerze, a procedura rejestrowania programu obsługi wskazuje, które centra, których Klient zamierza używać. Jednak dla klienta platformy .NET można ręcznie utworzyć centra proxy, dlatego moduł sygnalizujący zakłada, że będziesz używać dowolnego centrum, dla którego tworzysz serwer proxy.

Przykładowy kod używa domyślnego adresu URL "/SignalR", aby nawiązać połączenie z usługą sygnalizującej. Aby uzyskać informacje na temat sposobu określania innego podstawowego adresu URL, zobacz [Przewodnik interfejsu API centrów ASP.NET Signals-Server-The/SIGNALR URL](hubs-api-guide-server.md#signalrurl).

Metoda `Start` jest wykonywana asynchronicznie. Aby upewnić się, że kolejne wiersze kodu nie są wykonywane, dopóki połączenie nie zostanie nawiązane, użyj `await` w metodzie asynchronicznej ASP.NET 4,5 lub `.Wait()` w metodzie synchronicznej. Nie należy używać `.Wait()` w kliencie WinRT.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample2.cs?highlight=1)]

[!code-css[Main](hubs-api-guide-net-client/samples/sample3.css?highlight=1)]

<a id="slcrossdomain"></a>

### <a name="cross-domain-connections-from-silverlight-clients"></a>Połączenia między domenami z klientów Silverlight

Informacje o sposobie włączania połączeń między domenami z klientów Silverlight można znaleźć w temacie [udostępnianie usługi w granicach domen](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx).

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a>Jak skonfigurować połączenie

Przed nawiązaniem połączenia można określić jedną z następujących opcji:

- Limit jednoczesnych połączeń.
- Parametry ciągu zapytania.
- Metoda transportu.
- Nagłówki HTTP.
- Certyfikaty klienta.

<a id="maxconnections"></a>

### <a name="how-to-set-the-maximum-number-of-concurrent-connections-in-wpf-clients"></a>Jak ustawić maksymalną liczbę jednoczesnych połączeń w klientach WPF

W przypadku klientów WPF może zajść potrzeba zwiększenia maksymalnej liczby jednoczesnych połączeń z wartością domyślną 2. Zalecana wartość to 10.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample4.cs?highlight=5)]

Aby uzyskać więcej informacji, zobacz [ServicePointManager. DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a>Jak określić parametry ciągu zapytania

Jeśli chcesz wysłać dane do serwera podczas łączenia się z klientem, możesz dodać parametry ciągu zapytania do obiektu połączenia. Poniższy przykład pokazuje, jak ustawić parametr ciągu zapytania w kodzie klienta.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample5.cs)]

Poniższy przykład pokazuje, jak odczytać parametr ciągu zapytania w kodzie serwera.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample6.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a>Jak określić metodę transportu

W ramach procesu łączenia klient sygnalizujący zwykle negocjuje z serwerem, aby określić najlepszy transport obsługiwany przez serwer i klienta. Jeśli wiesz już, którego transportu chcesz użyć, możesz pominąć ten proces negocjacji. Aby określić metodę transportu, przekaż obiekt transportu do metody startowej. Poniższy przykład pokazuje, jak określić metodę transportu w kodzie klienta.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample7.cs?highlight=5)]

Przestrzeń nazw [Microsoft. ASPNET. Signal. Client. Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) zawiera następujące klasy, których można użyć do określenia transportu.

- [LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)
- [ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)
- [WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (dostępne tylko wtedy, gdy serwer i klient używają platformy .NET 4,5).
- [Autotransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (automatycznie wybiera najlepszą transport, który jest obsługiwany przez klienta i serwer. Jest to domyślny transport. Przekazanie tego w `Start` metodzie ma ten sam skutek, co nie przechodzą w żadnej operacji.

Transport ForeverFrame nie jest uwzględniony na tej liście, ponieważ jest używany tylko przez przeglądarki.

Aby uzyskać informacje na temat sprawdzania metody transportu w kodzie serwera, zobacz [Przewodnik interfejsu API centrów ASP.NET Signals-Server — jak uzyskać informacje o kliencie z właściwości Context](hubs-api-guide-server.md#contextproperty). Aby uzyskać więcej informacji na temat transportów i rezerw, zobacz [wprowadzenie do sygnałów-Transports and fallbacks](../getting-started/introduction-to-signalr.md#transports).

<a id="httpheaders"></a>

### <a name="how-to-specify-http-headers"></a>Jak określić nagłówki HTTP

Aby ustawić nagłówki HTTP, użyj właściwości `Headers` w obiekcie Connection. Poniższy przykład pokazuje, jak dodać nagłówek HTTP.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample8.cs?highlight=2)]

<a id="clientcertificate"></a>

### <a name="how-to-specify-client-certificates"></a>Jak określić certyfikaty klienta

Aby dodać certyfikaty klienta, użyj metody `AddClientCertificate` w obiekcie Connection.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample9.cs?highlight=2)]

<a id="proxy"></a>

## <a name="how-to-create-the-hub-proxy"></a>Jak utworzyć serwer proxy centrum

W celu zdefiniowania metod na kliencie, które mogą być wywoływane przez koncentrator z serwera, i aby wywołać metody w centrum na serwerze, Utwórz serwer proxy dla centrum, wywołując `CreateHubProxy` w obiekcie Connection. Ciąg przekazany do `CreateHubProxy` jest nazwą klasy centrum lub nazwą określoną przez atrybut `HubName`, jeśli został on użyty na serwerze. W dopasowaniu nazw nie jest rozróżniana wielkość liter.

**Klasa centrum na serwerze**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample10.cs?highlight=1)]

**Utwórz serwer proxy klienta dla klasy centrum**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample11.cs?highlight=3)]

Jeśli dekorować klasę centrów z atrybutem `HubName`, Użyj tej nazwy.

**Klasa centrum na serwerze**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample12.cs)]

**Utwórz serwer proxy klienta dla klasy centrum**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample13.cs?highlight=3)]

W przypadku wywołania `HubConnection.CreateHubProxy` wiele razy z tą samą `hubName`można uzyskać ten sam buforowany obiekt `IHubProxy`.

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a>Jak zdefiniować metody na kliencie, które mogą być wywoływane przez serwer

Aby zdefiniować metodę, którą może wywołać serwer, użyj metody `On` serwera proxy, aby zarejestrować procedurę obsługi zdarzeń.

W dopasowaniu nazw metod nie jest rozróżniana wielkość liter. Na przykład `Clients.All.UpdateStockPrice` na serwerze wykona `updateStockPrice`, `updatestockprice`lub `UpdateStockPrice` na kliencie.

Różne platformy klienckie mają różne wymagania dotyczące sposobu pisania kodu metody w celu zaktualizowania interfejsu użytkownika. Przykłady przedstawiono dla klientów WinRT (Windows Store .NET). Przykłady aplikacji WPF, Silverlight i konsoli znajdują się w [osobnej sekcji w dalszej części tego tematu](#wpfsl).

<a id="clientmethodswithoutparms"></a>

### <a name="methods-without-parameters"></a>Metody bez parametrów

Jeśli obsługiwana metoda nie ma parametrów, użyj nieogólnego przeciążenia metody `On`:

**Kod serwera wywołujący metodę klienta bez parametrów**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample14.cs?highlight=5)]

**Kod klienta WinRT dla metody wywoływanej z serwera bez parametrów ([Zobacz przykłady WPF i Silverlight w dalszej części tego tematu](#wpfsl))**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample15.cs)]

<a id="clientmethodswithparmtypes"></a>

### <a name="methods-with-parameters-specifying-the-parameter-types"></a>Metody z parametrami, Określanie typów parametrów

Jeśli obsługiwana metoda ma parametry, określ typy parametrów jako typy ogólne metody `On`. Istnieją ogólne przeciążenia metody `On`, aby umożliwić określenie maksymalnie 8 parametrów (4 w Windows Phone 7). W poniższym przykładzie jeden parametr jest wysyłany do metody `UpdateStockPrice`.

**Kod serwera wywołujący metodę klienta z parametrem**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample16.cs?highlight=3)]

**Klasa giełdowa użyta dla parametru**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample17.cs)]

**Kod klienta WinRT dla metody wywoływanej z serwera z parametrem ([Zobacz przykłady WPF i Silverlight w dalszej części tego tematu](#wpfsl))**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample18.cs?highlight=1,5)]

<a id="clientmethodswithdynamparms"></a>

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a>Metody z parametrami, określając dynamiczne obiekty parametrów

Alternatywą dla określania parametrów jako typów ogólnych metody `On` można określić parametry jako obiekty dynamiczne:

**Kod serwera wywołujący metodę klienta z parametrem**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample19.cs?highlight=3)]

**Klasa giełdowa użyta dla parametru**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample20.cs)]

**Kod klienta WinRT dla metody wywoływanej z serwera z parametrem przy użyciu obiektu dynamicznego dla parametru ([Zobacz przykłady WPF i Silverlight w dalszej części tego tematu](#wpfsl))**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample21.cs?highlight=1,5)]

<a id="removehandler"></a>

### <a name="how-to-remove-a-handler"></a>Jak usunąć procedurę obsługi

Aby usunąć procedurę obsługi, wywołaj jej metodę `Dispose`.

**Kod klienta dla metody wywoływanej z serwera**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample22.cs?highlight=1)]

**Kod klienta do usunięcia programu obsługi**

[!code-css[Main](hubs-api-guide-net-client/samples/sample23.css?highlight=1)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a>Jak wywoływać metody serwera z klienta

Aby wywołać metodę na serwerze, użyj metody `Invoke` w serwerze proxy centrum.

Jeśli metoda serwera nie ma zwracanej wartości, użyj nieogólnego przeciążenia metody `Invoke`.

**Kod serwera dla metody, która nie ma wartości zwracanej**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample24.cs?highlight=3)]

**Kod klienta wywołujący metodę, która nie ma wartości zwracanej**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample25.cs?highlight=1)]

Jeśli metoda serwera ma wartość zwracaną, określ zwracany typ jako typ ogólny metody `Invoke`.

**Kod serwera dla metody, która ma wartość zwracaną i przyjmuje parametr typu złożonego**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample26.cs?highlight=1)]

**Klasa giełdowa użyta dla parametru i wartości zwracanej**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample27.cs)]

**Kod klienta wywołujący metodę, która ma wartość zwracaną i przyjmuje parametr typu złożonego, w metodzie asynchronicznej ASP.NET 4,5**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample28.cs?highlight=1-2)]

**Kod klienta wywołujący metodę, która ma wartość zwracaną i pobiera parametr typu złożonego, w metodzie synchronicznej**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample29.cs?highlight=1-2)]

Metoda `Invoke` wykonuje asynchroniczne i zwraca obiekt `Task`. Jeśli nie określisz `await` ani `.Wait()`, następnym wierszem kodu zostanie wykonane przed wykonaniem metody wywoływanej.

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a>Jak obsługiwać zdarzenia okresu istnienia połączenia

Program sygnalizujący udostępnia następujące zdarzenia okresu istnienia połączenia, które można obsłużyć:

- `Received`: wywoływane po odebraniu dowolnego danych w połączeniu. Dostarcza odebrane dane.
- `ConnectionSlow`: wywoływane, gdy klient wykrywa powolne lub często porzucane połączenie.
- `Reconnecting`: uruchamiany, gdy podstawowy transport zacznie ponownie nawiązać połączenie.
- `Reconnected`: wywoływane, gdy podstawowy transport został połączony ponownie.
- `StateChanged`: występuje, gdy zmieni się stan połączenia. Zapewnia poprzedni stan i nowy stan. Aby uzyskać informacje o wartościach stanu połączenia, zobacz [Wyliczenie ConnectionState](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).
- `Closed`: wywoływane, gdy połączenie zostało rozłączone.

Na przykład, jeśli chcesz wyświetlać komunikaty ostrzegawcze dla błędów, które nie są krytyczne, ale powodują sporadyczne problemy z połączeniem, takie jak spowolnienie lub częste usuwanie połączenia, obsłuż zdarzenie `ConnectionSlow`.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample30.cs)]

Aby uzyskać więcej informacji, zobacz [Omówienie i obsługa zdarzeń okresu istnienia połączenia w programie sygnalizującym](handling-connection-lifetime-events.md).

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a>Jak obsłużyć błędy

Jeśli nie włączysz jawnie szczegółowych komunikatów o błędach na serwerze, obiekt wyjątku zwracany przez sygnalizujący po wystąpieniu błędu zawiera minimalne informacje o błędzie. Na przykład jeśli wywołanie `newContosoChatMessage` nie powiedzie się, komunikat o błędzie w obiekcie Error zawiera "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" wysyłanie szczegółowych komunikatów o błędach do klientów w środowisku produkcyjnym nie jest zalecane ze względów bezpieczeństwa, ale jeśli chcesz włączyć szczegółowe komunikaty o błędach w celu rozwiązywania problemów, użyj następującego kodu na serwerze.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample31.cs?highlight=2)]

<a id="handleerrors"></a>

Aby obsłużyć błędy, które wywołuje program sygnalizujący, można dodać procedurę obsługi dla zdarzenia `Error` w obiekcie Connection.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample32.cs)]

Aby obsłużyć błędy wywołań metod, zawiń kod w bloku try-catch.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample33.cs)]

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a>Jak włączyć rejestrowanie po stronie klienta

Aby włączyć rejestrowanie po stronie klienta, należy ustawić właściwości `TraceLevel` i `TraceWriter` w obiekcie Connection.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample34.cs?highlight=3-4)]

<a id="wpfsl"></a>

## <a name="wpf-silverlight-and-console-application-code-samples-for-client-methods-that-the-server-can-call"></a>Przykłady kodu aplikacji WPF, Silverlight i konsoli dla metod klienta wywoływanych przez serwer

Przykłady kodu pokazane wcześniej w celu zdefiniowania metod klienta, które mogą być wywoływane przez serwer, mają zastosowanie do klientów WinRT. W poniższych przykładach przedstawiono odpowiedni kod dla klientów WPF, Silverlight i aplikacji konsolowych.

### <a name="methods-without-parameters"></a>Metody bez parametrów

**Kod klienta WPF dla metody wywoływanej z serwera bez parametrów**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample35.cs?highlight=1)]

**Kod klienta Silverlight dla metody wywoływanej z serwera bez parametrów**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample36.cs?highlight=1)]

**Kod klienta aplikacji konsolowej dla metody wywoływanej z serwera bez parametrów**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample37.cs?highlight=1)]

### <a name="methods-with-parameters-specifying-the-parameter-types"></a>Metody z parametrami, Określanie typów parametrów

**Kod klienta WPF dla metody wywoływanej z serwera z parametrem**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample38.cs?highlight=1,4)]

**Kod klienta Silverlight dla metody wywoływanej z serwera z parametrem**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample39.cs?highlight=1,5)]

**Kod klienta aplikacji konsolowej dla metody wywoływanej z serwera z parametrem**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample40.cs?highlight=1-2)]

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a>Metody z parametrami, określając dynamiczne obiekty parametrów

**Kod klienta WPF dla metody wywoływanej z serwera z parametrem przy użyciu obiektu dynamicznego dla parametru**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample41.cs?highlight=1,4)]

**Kod klienta Silverlight dla metody wywoływanej z serwera z parametrem przy użyciu obiektu dynamicznego dla parametru**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample42.cs?highlight=1,5)]

**Kod klienta aplikacji konsolowej dla metody wywoływanej z serwera z parametrem przy użyciu obiektu dynamicznego dla parametru**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample43.cs?highlight=1-2)]

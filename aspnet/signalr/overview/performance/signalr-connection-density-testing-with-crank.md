---
uid: signalr/overview/performance/signalr-connection-density-testing-with-crank
title: Testowanie gęstości połączeń przy użyciu sygnału korbowego | Microsoft Docs
author: bradygaster
description: Testowanie gęstości połączenia usługi SignalR za pomocą funkcji Crank
ms.author: bradyg
ms.date: 02/22/2015
ms.assetid: 148d9ca7-1af1-44b6-a9fb-91e261b9b463
msc.legacyurl: /signalr/overview/performance/signalr-connection-density-testing-with-crank
msc.type: authoredcontent
ms.openlocfilehash: 901e039fbb81651ed18d560c99745b7e7f716e01
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78558339"
---
# <a name="signalr-connection-density-testing-with-crank"></a>Testowanie gęstości połączenia usługi SignalR za pomocą funkcji Crank

Autor [FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> W tym artykule opisano sposób użycia narzędzia korb do testowania aplikacji z wieloma symulowanymi klientami.

Gdy aplikacja jest uruchomiona w środowisku hostingu (rola sieci Web platformy Azure, usługi IIS lub własne hosty za pomocą Owin), można testować odpowiedzi aplikacji na wysoki poziom gęstości połączeń przy użyciu narzędzia. Środowisko hostingu może być serwerem Internet Information Services (IIS), hostem Owin lub rolą sieci Web platformy Azure. (Uwaga: Liczniki wydajności nie są dostępne w Azure App Service Web Apps, więc nie będzie można uzyskać danych dotyczących wydajności z testu gęstości połączenia).

Gęstość połączenia odnosi się do liczby jednoczesnych połączeń TCP, które można ustalić na serwerze. Każde połączenie TCP wiąże się z własnym obciążeniem i otwarcie dużej liczby nieczynnych połączeń spowoduje powstanie wąskiego gardła pamięci.

[Baza kodu sygnalizująca](https://github.com/signalr/signalr) zawiera narzędzie do testowania obciążenia o nazwie **korb**. Najnowszą wersję platformy 4.0 można znaleźć w [gałęzi dev](https://github.com/SignalR/signalr/tree/dev) w witrynie GitHub. W [tym miejscu](https://github.com/SignalR/SignalR/archive/dev.zip)możesz pobrać archiwum zip gałęzi dev dla sygnalizującej.

System korb może służyć do całkowitego nasycenia pamięci serwera, aby można było obliczyć łączną liczbę bezczynnych połączeń na sprzęcie serwera. Alternatywnie, można również użyć opcji korb do załadowania serwera z określoną ilością pamięci, aby obciążać połączenia do momentu osiągnięcia określonej liczby lub określonego progu pamięci.

Podczas testowania należy używać zdalnych klientów, aby uniknąć wszelkich konkursów zasobów (tj. połączeń TCP i pamięci). Monitoruj klientów, aby upewnić się, że nie powodują one wąskich gardeł, które mogą uniemożliwić serwerowi osiągnięcie pełnej pojemności (pamięci lub procesora CPU). Może być konieczne zwiększenie liczby klientów w celu całkowitego załadowania serwera.

### <a name="running-a-connection-density-test"></a>Uruchamianie testu gęstości połączenia

W tej sekcji opisano kroki wymagane do uruchomienia testu gęstości połączenia w aplikacji sygnalizującej.

1. Pobierz i skompiluj [gałąź Dev dla sygnalizującego kodu](https://github.com/SignalR/SignalR/archive/dev.zip). W wierszu polecenia przejdź do &lt;katalogu projektu&gt;\src\Microsoft.AspNet.SignalR.Crank\bin\debug.
2. Wdróż aplikację w zamierzonym środowisku hostingu. Zanotuj punkt końcowy używany przez aplikację; na przykład w aplikacji utworzonej w [samouczku wprowadzenie](../getting-started/tutorial-getting-started-with-signalr.md)punkt końcowy jest `http://<yourhost>:8080/signalr`.
3. [Liczniki wydajności instalacji sygnałów](signalr-performance.md#perfcounters) na serwerze. Jeśli aplikacja działa na platformie Azure, zobacz [Używanie liczników wydajności sygnałów w roli sieci Web platformy Azure](using-signalr-performance-counters-in-an-azure-web-role.md).

Po pobraniu i skompilowaniu bazy kodu i zainstalowaniu liczników wydajności na hoście można znaleźć w folderze `src\Microsoft.AspNet.SignalR.Crank\bin\Debug`.

Dostępne opcje narzędzia korb to:

- **/?** : Wyświetla ekran pomoc. Dostępne opcje są również wyświetlane w przypadku pominięcia parametru **adresu URL** .
- **/URL**: adres URL dla połączeń sygnału. Ten parametr jest wymagany. W przypadku aplikacji sygnalizującej przy użyciu domyślnego mapowania ścieżka zostanie zakończona "/SignalR".
- **/Transport**: Nazwa używanej transportu. Wartość domyślna to `auto`, co spowoduje wybranie najlepszego dostępnego protokołu. Opcje obejmują `WebSockets`, `ServerSentEvents`i `LongPolling` (`ForeverFrame` nie jest opcją dla systemu korbowego, ponieważ jest używany klient .NET zamiast programu Internet Explorer). Aby uzyskać więcej informacji o tym, jak program sygnalizujący wybiera transporty, zobacz [Transports and fallbacks](../getting-started/introduction-to-signalr.md#transports).
- **/BatchSize**: liczba klientów dodanych do każdej partii. Wartość domyślna to 50.
- **/ConnectInterval**: interwał w milisekundach między dodawaniem połączeń. Wartość domyślna to 500.
- **/Connections**: liczba połączeń używanych do załadowania i testowania aplikacji. Wartość domyślna to 100 000.
- **/ConnectTimeout**: limit czasu (w sekundach), po upływie którego test zostanie przerwany. Wartość domyślna to 300.
- **MinServerMBytes**: minimalny dostęp do serwera (w megabajtach). Wartość domyślna to 500.
- **SendBytes**: rozmiar ładunku wysyłanego do serwera w bajtach. Wartość domyślna to 0.
- **SendInterval**: opóźnienie (w milisekundach) między komunikatami a serwerem. Wartość domyślna to 500.
- **Właściwości SendTimeout**: limit czasu (w milisekundach) dla komunikatów wysyłanych do serwera. Wartość domyślna to 300.
- **ControllerUrl**: adres URL, pod którym jeden klient będzie hostować koncentrator kontrolera. Wartość domyślna to null (brak koncentratora kontrolera). Koncentrator kontrolera jest uruchamiany po rozpoczęciu sesji. nie przeprowadzono dalszej komunikacji między koncentratorem kontrolera i etapem.
- **NumClients**: liczba symulowanych klientów do połączenia z aplikacją. Wartość domyślna to 1.
- **Plik_dziennika**: nazwa pliku dziennika dla przebiegu testu. Wartość domyślna to `crank.csv`.
- **SampleInterval**: czas w milisekundach między próbkami licznika wydajności. Wartość domyślna to 1000.
- **SignalRInstance**: nazwa wystąpienia liczników wydajności na serwerze. Wartość domyślna to użycie stanu połączenia klienta.

### <a name="example"></a>Przykład

Następujące polecenie przetestuje lokację o nazwie `pfsignalr` na platformie Azure, która hostuje aplikację na porcie 8080 z centrum o nazwie "ControllerHub", przy użyciu połączeń 100.

`crank /Connections:100 /Url:http://pfsignalr.cloudapp.net:8080/signalr`

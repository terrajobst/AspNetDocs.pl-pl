---
uid: signalr/overview/older-versions/scaleout-with-redis
title: Skalowania sygnalizujący z Redis (Signaler 1. x) | Microsoft Docs
author: bradygaster
description: ''
ms.author: bradyg
ms.date: 05/01/2013
ms.assetid: 6abecf80-8ffa-41ba-b0d9-1d9edbe7687b
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-redis
msc.type: authoredcontent
ms.openlocfilehash: 5079cf3fa773faeac911eddc75dc66e94ad98bb0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536562"
---
# <a name="signalr-scaleout-with-redis-signalr-1x"></a>SignalR — skalowanie w poziomie z użyciem pamięci podręcznej Redis (SignalR 1.x)

według [Jan Wasson](https://github.com/MikeWasson), [Patryk Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

W tym samouczku będziesz używać [Redis](http://redis.io/) do dystrybuowania komunikatów w aplikacji sygnalizującej wdrożonej na dwóch oddzielnych WYSTĄPIENIACH usług IIS.

Redis jest magazynem wartości w pamięci. Obsługuje również system obsługi komunikatów z modelem publikowania/subskrybowania. Funkcja Rediser programu sygnalizującego używa funkcji pub/sub do przesyłania dalej komunikatów do innych serwerów.

![](scaleout-with-redis/_static/image1.png)

W tym samouczku zostaną użyte trzy serwery:

- Dwa serwery z systemem Windows, które będą używane do wdrażania aplikacji sygnalizującej.
- Jeden serwer z systemem Linux, który będzie używany do uruchamiania Redis. W przypadku zrzutów ekranu w tym samouczku użyto Ubuntu 12,04 TLS.

Jeśli nie masz trzech serwerów fizycznych do użycia, możesz utworzyć maszyny wirtualne w funkcji Hyper-V. Innym rozwiązaniem jest utworzenie maszyn wirtualnych na platformie Azure.

Chociaż w tym samouczku jest stosowana Oficjalna implementacja Redis, istnieje również [port systemu Windows Redis](https://github.com/MSOpenTech/redis) z MSOpenTech. Konfiguracja i konfiguracja są różne, ale w przeciwnym razie kroki są takie same.

> [!NOTE] 
> 
> Usługa sygnalizująca skalowania z Redis nie obsługuje klastrów Redis.

## <a name="overview"></a>Omówienie

Zanim przejdziemy do szczegółowego samouczka, poniżej przedstawiono krótkie omówienie tego, co robisz.

1. Zainstaluj program Redis i uruchom serwer Redis.
2. Dodaj te pakiety NuGet do aplikacji: 

    - [Microsoft. AspNet. Signal](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft. AspNet. Signal. Redis](http://nuget.org/packages/Microsoft.AspNet.SignalR.Redis)
3. Tworzenie aplikacji sygnalizującej.
4. Dodaj następujący kod do szablonu Global. asax, aby skonfigurować plan: 

    [!code-csharp[Main](scaleout-with-redis/samples/sample1.cs)]

## <a name="ubuntu-on-hyper-v"></a>Ubuntu na funkcji Hyper-V

Korzystając z funkcji Windows Hyper-V, można łatwo utworzyć maszynę wirtualną Ubuntu w systemie Windows Server.

Pobierz plik ISO Ubuntu z [http://www.ubuntu.com](http://www.ubuntu.com/).

W funkcji Hyper-V Dodaj nową maszynę wirtualną. W kroku **Podłączanie wirtualnego dysku twardego** wybierz pozycję **Utwórz wirtualny dysk twardy**.

![](scaleout-with-redis/_static/image2.png)

W kroku **Opcje instalacji** wybierz pozycję **plik obrazu (. ISO)** , kliknij przycisk **Przeglądaj**, a następnie przejdź do Ubuntu instalacji ISO.

![](scaleout-with-redis/_static/image3.png)

## <a name="install-redis"></a>Zainstaluj Redis

Postępuj zgodnie z instrukcjami w [http://redis.io/download](http://redis.io/download) , aby pobrać i skompilować Redis.

[!code-console[Main](scaleout-with-redis/samples/sample2.cmd)]

Spowoduje to skompilowanie plików binarnych Redis w katalogu `src`.

Domyślnie Redis nie wymaga hasła. Aby ustawić hasło, edytuj plik `redis.conf`, który znajduje się w katalogu głównym kodu źródłowego. (Utwórz kopię zapasową pliku przed edycją!) Dodaj następującą dyrektywę do `redis.conf`:

[!code-powershell[Main](scaleout-with-redis/samples/sample3.ps1)]

Teraz uruchom serwer Redis:

[!code-css[Main](scaleout-with-redis/samples/sample4.css)]

![](scaleout-with-redis/_static/image4.png)

Otwórz port 6379, który jest domyślnym portem, który Redis nasłuchuje. (Numer portu można zmienić w pliku konfiguracyjnym).

## <a name="create-the-signalr-application"></a>Tworzenie aplikacji sygnalizującej

Utwórz aplikację sygnalizującą, wykonując jeden z następujących samouczków:

- [Wprowadzenie z sygnałem](../getting-started/tutorial-getting-started-with-signalr.md)
- [Wprowadzenie z sygnalizacyjnym i MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md)

Następnie zmodyfikujemy aplikację czatu, aby obsługiwała skalowania z Redis. Najpierw Dodaj pakiet NuGet Rediser do projektu. W programie Visual Studio w menu **Narzędzia** wybierz pozycję **Menedżer pakietów NuGet**, a następnie wybierz pozycję **konsola Menedżera pakietów**. W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:

[!code-powershell[Main](scaleout-with-redis/samples/sample5.ps1)]

Następnie otwórz plik Global. asax. Dodaj następujący kod do **aplikacji\_Start** :

[!code-csharp[Main](scaleout-with-redis/samples/sample6.cs)]

- "serwer" jest nazwą serwera, na którym działa Redis.
- *port* jest numerem portu
- "hasło" to hasło zdefiniowane w pliku Redis. conf.
- "Nazwa_aplikacji" to dowolny ciąg. Program sygnalizujący tworzy kanał Redis pub/Sub o tej nazwie.

Na przykład:

[!code-csharp[Main](scaleout-with-redis/samples/sample7.cs)]

## <a name="deploy-and-run-the-application"></a>Wdrażanie i uruchamianie aplikacji

Przygotuj wystąpienia systemu Windows Server, aby wdrożyć aplikację sygnalizującą.

Dodaj rolę usług IIS. Uwzględnij funkcje "projektowanie aplikacji", w tym protokół WebSocket.

![](scaleout-with-redis/_static/image5.png)

Dołącz także usługę zarządzania (wymienioną w obszarze "narzędzia do zarządzania").

![](scaleout-with-redis/_static/image6.png)

**Zainstaluj Web Deploy 3,0.** Po uruchomieniu Menedżera usług IIS zostanie wyświetlony monit o zainstalowanie platformy sieci Web firmy Microsoft lub [pobranie instalatora](https://go.microsoft.com/fwlink/?LinkId=255386). W Instalatorze platformy Wyszukaj Web Deploy i zainstaluj Web Deploy 3,0

![](scaleout-with-redis/_static/image7.png)

Sprawdź, czy usługa zarządzania siecią Web jest uruchomiona. Jeśli nie, uruchom usługę. (Jeśli nie widzisz usługi zarządzania siecią Web na liście usług systemu Windows, upewnij się, że usługa zarządzania została zainstalowana po dodaniu roli usług IIS.)

Domyślnie usługa zarządzania siecią Web nasłuchuje na porcie TCP 8172. W zaporze systemu Windows utwórz nową regułę ruchu przychodzącego zezwalającą na ruch TCP na porcie 8172. Aby uzyskać więcej informacji, zobacz [Konfigurowanie reguł zapory](https://technet.microsoft.com/library/dd448559(WS.10).aspx). (Jeśli przechowujesz maszyny wirtualne na platformie Azure, możesz to zrobić bezpośrednio w Azure Portal. Zobacz [, jak skonfigurować punkty końcowe na maszynie wirtualnej](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/).

Teraz możesz przystąpić do wdrażania projektu programu Visual Studio z komputera deweloperskiego na serwerze. W Eksplorator rozwiązań kliknij prawym przyciskiem myszy rozwiązanie, a następnie kliknij pozycję **Publikuj**.

Aby uzyskać bardziej szczegółową dokumentację dotyczącą wdrażania w sieci Web, zobacz [Web Deployment Content Map for Visual Studio i ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).

Jeśli aplikacja zostanie wdrożona na dwóch serwerach, można otworzyć każde wystąpienie w osobnym oknie przeglądarki i zobaczyć, że każdy z nich odbiera komunikaty sygnalizujące. (Oczywiście w środowisku produkcyjnym dwa serwery byłyby za modułem równoważenia obciążenia).

![](scaleout-with-redis/_static/image8.png)

Jeśli chcesz wiedzieć chcesz zobaczyć komunikaty wysyłane do Redis, możesz użyć klienta **Redis-CLI** , który jest instalowany z Redis.

[!code-xml[Main](scaleout-with-redis/samples/sample8.xml)]

![](scaleout-with-redis/_static/image9.png)

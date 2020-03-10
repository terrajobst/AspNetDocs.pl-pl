---
uid: signalr/overview/performance/scaleout-with-redis
title: Skalowania sygnalizujący Redis | Microsoft Docs
author: bradygaster
description: Wersje oprogramowania używane w tym temacie Visual Studio 2013 programu .NET 4,5 sygnalizującego w wersji 2 poprzednie wersje tego tematu, aby uzyskać informacje o wcześniejszych wersjach programu...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: 6ecd08c1-e364-4cd7-ad4c-806521911585
msc.legacyurl: /signalr/overview/performance/scaleout-with-redis
msc.type: authoredcontent
ms.openlocfilehash: 58a7affa1769523955adc76455a1c33be6f49751
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78579227"
---
# <a name="signalr-scaleout-with-redis"></a>SignalR — skalowanie w poziomie z użyciem pamięci podręcznej Redis

według [Jan Wasson](https://github.com/MikeWasson)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> ## <a name="software-versions-used-in-this-topic"></a>Wersje oprogramowania używane w tym temacie
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - Sygnalizacja w wersji 2,4
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
    - [Microsoft. AspNet. Signal. StackExchangeRedis](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.StackExchangeRedis)
    
3. Tworzenie aplikacji sygnalizującej.
4. Dodaj następujący kod do Startup.cs w celu skonfigurowania planu:

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

- [Wprowadzenie z sygnałem 2,0](../getting-started/tutorial-getting-started-with-signalr.md)
- [Wprowadzenie z sygnałami 2,0 i MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)

Następnie zmodyfikujemy aplikację czatu, aby obsługiwała skalowania z Redis. Najpierw Dodaj pakiet NuGet `Microsoft.AspNet.SignalR.StackExchangeRedis` do projektu. W programie Visual Studio w menu **Narzędzia** wybierz pozycję **Menedżer pakietów NuGet**, a następnie wybierz pozycję **konsola Menedżera pakietów**. W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:

[!code-powershell[Main](scaleout-with-redis/samples/sample5.ps1)]

Następnie otwórz plik Startup.cs. Dodaj następujący kod do metody **konfiguracji** :

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

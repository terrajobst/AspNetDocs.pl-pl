---
uid: signalr/overview/older-versions/scaleout-with-sql-server
title: Skalowania sygnalizujący z SQL Server (Signaler 1. x) | Microsoft Docs
author: bradygaster
description: ''
ms.author: bradyg
ms.date: 05/01/2013
ms.assetid: 1dca7967-8296-444a-9533-837eb284e78c
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-sql-server
msc.type: authoredcontent
ms.openlocfilehash: 8674b06a2a751c9a7b1c6c9b19782974d0602a62
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536471"
---
# <a name="signalr-scaleout-with-sql-server-signalr-1x"></a>SignalR — skalowanie w poziomie z użyciem programu SQL Server (SignalR 1.x)

według [Jan Wasson](https://github.com/MikeWasson), [Patryk Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

W tym samouczku będziesz używać SQL Server do dystrybuowania komunikatów w aplikacji sygnalizującej wdrożonej w dwóch oddzielnych wystąpieniach usług IIS. Możesz również uruchomić ten samouczek na pojedynczym komputerze testowym, ale aby uzyskać pełen efekt, należy wdrożyć aplikację sygnalizującą na co najmniej dwóch serwerach. Należy również zainstalować SQL Server na jednym z serwerów lub na osobnym dedykowanym serwerze. Innym rozwiązaniem jest uruchomienie samouczka przy użyciu maszyn wirtualnych na platformie Azure.

![](scaleout-with-sql-server/_static/image1.png)

## <a name="prerequisites"></a>Wymagania wstępne

Microsoft SQL Server 2005 lub nowszy. Plan dla programu jest obsługiwany zarówno przez program, jak i wersje systemu SQL Server. Nie obsługuje wersji SQL Server Compact ani Azure SQL Database. (Jeśli aplikacja jest hostowana na platformie Azure, rozważ użycie zamiast niej planu Service Bus.)

## <a name="overview"></a>Omówienie

Zanim przejdziemy do szczegółowego samouczka, poniżej przedstawiono krótkie omówienie tego, co robisz.

1. Utwórz nową pustą bazę danych. Plan nie utworzy niezbędnych tabel w tej bazie danych.
2. Dodaj te pakiety NuGet do aplikacji: 

    - [Microsoft. AspNet. Signal](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft. AspNet. Signaler. SqlServer](http://nuget.org/packages/Microsoft.AspNet.SignalR.SqlServer)
3. Tworzenie aplikacji sygnalizującej.
4. Dodaj następujący kod do szablonu Global. asax, aby skonfigurować plan: 

    [!code-csharp[Main](scaleout-with-sql-server/samples/sample1.cs)]

## <a name="configure-the-database"></a>Konfigurowanie bazy danych

Zdecyduj, czy aplikacja będzie używać uwierzytelniania systemu Windows, czy SQL Server uwierzytelniania w celu uzyskania dostępu do bazy danych. Bez względu na to upewnij się, że użytkownik bazy danych ma uprawnienia do logowania, tworzenia schematów i tworzenia tabel.

Utwórz nową bazę danych do użycia w celu utworzenia planu. Można nadać bazie danych dowolną nazwę. Nie musisz tworzyć żadnych tabel w bazie danych; plan nie utworzy niezbędnych tabel.

![](scaleout-with-sql-server/_static/image2.png)

## <a name="enable-service-broker"></a>Włącz Service Broker

Zaleca się włączenie Service Broker dla bazy danych programu planowanie. Service Broker zapewnia natywną obsługę komunikatów i kolejkowania w SQL Server, co pozwala na efektywniejsze otrzymywanie aktualizacji. (Jednak plan nie działa również bez Service Broker).

Aby sprawdzić, czy Service Broker jest włączona, zbadaj kolumnę **is\_Broker\_Enabled** w widoku wykazu **sys. databases** .

[!code-sql[Main](scaleout-with-sql-server/samples/sample2.sql)]

![](scaleout-with-sql-server/_static/image3.png)

Aby włączyć Service Broker, użyj następującego zapytania SQL:

[!code-sql[Main](scaleout-with-sql-server/samples/sample3.sql)]

> [!NOTE]
> Jeśli ta kwerenda wydaje się być zakleszczeniem, upewnij się, że żadne aplikacje nie są połączone z bazą danych.

Jeśli włączono śledzenie, ślady będą również wskazywać, czy Service Broker jest włączona.

## <a name="create-a-signalr-application"></a>Tworzenie aplikacji sygnalizującej

Utwórz aplikację sygnalizującą, wykonując jeden z następujących samouczków:

- [Wprowadzenie z sygnałem](../getting-started/tutorial-getting-started-with-signalr.md)
- [Wprowadzenie z sygnalizacyjnym i MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md)

Następnie zmodyfikujemy aplikację czatu, aby obsługiwała skalowania z SQL Server. Najpierw Dodaj pakiet NuGet programu Signaler do projektu. W programie Visual Studio w menu **Narzędzia** wybierz pozycję **Menedżer pakietów NuGet**, a następnie wybierz pozycję **konsola Menedżera pakietów**. W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:

[!code-powershell[Main](scaleout-with-sql-server/samples/sample4.ps1)]

Następnie otwórz plik Global. asax. Dodaj następujący kod do **aplikacji\_Start** :

[!code-csharp[Main](scaleout-with-sql-server/samples/sample5.cs)]

## <a name="deploy-and-run-the-application"></a>Wdrażanie i uruchamianie aplikacji

Przygotuj wystąpienia systemu Windows Server, aby wdrożyć aplikację sygnalizującą.

Dodaj rolę usług IIS. Uwzględnij funkcje "projektowanie aplikacji", w tym protokół WebSocket.

![](scaleout-with-sql-server/_static/image4.png)

Dołącz także usługę zarządzania (wymienioną w obszarze "narzędzia do zarządzania").

![](scaleout-with-sql-server/_static/image5.png)

**Zainstaluj Web Deploy 3,0.** Po uruchomieniu Menedżera usług IIS zostanie wyświetlony monit o zainstalowanie platformy sieci Web firmy Microsoft lub [pobranie instalatora](https://go.microsoft.com/fwlink/?LinkId=255386). W Instalatorze platformy Wyszukaj Web Deploy i zainstaluj Web Deploy 3,0

![](scaleout-with-sql-server/_static/image6.png)

Sprawdź, czy usługa zarządzania siecią Web jest uruchomiona. Jeśli nie, uruchom usługę. (Jeśli nie widzisz usługi zarządzania siecią Web na liście usług systemu Windows, upewnij się, że usługa zarządzania została zainstalowana po dodaniu roli usług IIS.)

Na koniec Otwórz port 8172 dla protokołu TCP. Jest to port, którego używa narzędzie Web Deploy.

Teraz możesz przystąpić do wdrażania projektu programu Visual Studio z komputera deweloperskiego na serwerze. W Eksplorator rozwiązań kliknij prawym przyciskiem myszy rozwiązanie, a następnie kliknij pozycję **Publikuj**.

Aby uzyskać bardziej szczegółową dokumentację dotyczącą wdrażania w sieci Web, zobacz [Web Deployment Content Map for Visual Studio i ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).

Jeśli aplikacja zostanie wdrożona na dwóch serwerach, można otworzyć każde wystąpienie w osobnym oknie przeglądarki i zobaczyć, że każdy z nich odbiera komunikaty sygnalizujące. (Oczywiście w środowisku produkcyjnym dwa serwery byłyby za modułem równoważenia obciążenia).

![](scaleout-with-sql-server/_static/image7.png)

Po uruchomieniu aplikacji można zobaczyć, że sygnalizujący automatycznie utworzy tabele w bazie danych:

![](scaleout-with-sql-server/_static/image8.png)

Program sygnalizujący zarządza tabelami. Dopóki aplikacja jest wdrożona, nie usuwaj wierszy, Modyfikuj tabelę i tak dalej.

---
uid: signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
title: Korzystanie z liczników wydajności sygnalizujących w roli sieci Web platformy Azure | Microsoft Docs
author: guardrex
description: Jak zainstalować liczniki wydajności sygnalizujące i korzystać z nich w roli sieci Web platformy Azure.
keywords: ASP. NET, sygnalizujący, licznik wydajności, rola sieci Web platformy Azure
ms.author: bradyg
ms.date: 10/03/2018
ms.assetid: 2a127d3b-21ed-4cc9-bec0-cdab4e742a25
msc.legacyurl: /signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
msc.type: authoredcontent
ms.openlocfilehash: 969a2ce43a7cb8d649555daf282f900401c0c914
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78578982"
---
# <a name="using-signalr-performance-counters-in-an-azure-web-role"></a>Korzystanie z liczników wydajności sygnałów w roli sieci Web platformy Azure

Autor [Luke Latham](https://github.com/guardrex)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

Liczniki wydajności sygnałów są używane do monitorowania wydajności aplikacji w roli sieci Web platformy Azure. Liczniki są przechwytywane przez Diagnostyka Microsoft Azure. Liczniki wydajności dla sygnałów są instalowane na platformie Azure przy użyciu programu *sygnalizującego. exe*, tego samego narzędzia, które jest używane w przypadku aplikacji autonomicznych lub lokalnych. Ponieważ Role platformy Azure są przejściowe, należy skonfigurować aplikację do instalowania i rejestrowania liczników wydajności sygnałów przy uruchamianiu.

## <a name="prerequisites"></a>Wymagania wstępne

* Visual Studio 2015 lub [2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
* [Microsoft Azure SDK dla programu Visual Studio](https://azure.microsoft.com/downloads/) **Uwaga: Uruchom ponownie komputer po zainstalowaniu zestawu SDK.**
* Subskrypcja Microsoft Azure: Aby utworzyć bezpłatne konto wersji próbnej platformy Azure, zobacz [bezpłatna wersja próbna platformy Azure](https://azure.microsoft.com/free/).

## <a name="creating-an-azure-web-role-application-that-exposes-signalr-performance-counters"></a>Tworzenie aplikacji roli sieci Web platformy Azure, która uwidacznia liczniki wydajności sygnałów

1. Otwórz program Visual Studio.

2. W programie Visual Studio wybierz pozycje **Plik** > **Nowy** > **Projekt**.

3. W oknie dialogowym **Nowy projekt** wybierz kategorię **Visual C#**  > **Cloud** z lewej strony, a następnie wybierz szablon **usługi w chmurze platformy Azure** . Nadaj aplikacji nazwę **SignalRPerfCounters** i wybierz **przycisk OK**.

   ![Nowa aplikacja w chmurze](using-signalr-performance-counters-in-an-azure-web-role/_static/image1.png)

   > [!NOTE]
   > Jeśli nie widzisz kategorii szablonów w **chmurze** lub szablonu **usługi w chmurze Azure** , musisz zainstalować obciążenie Programowanie na **platformie azure** dla programu Visual Studio 2017. Wybierz łącze **otwórz Instalator programu Visual Studio** w lewej dolnej części okna dialogowego **Nowy projekt** , aby otworzyć Instalator programu Visual Studio. Wybierz obciążenie **Programowanie na platformie Azure** , a następnie wybierz pozycję **Modyfikuj** , aby rozpocząć instalowanie obciążenia.
   >
   > ![Obciążenie Programowanie na platformie Azure w Instalator programu Visual Studio](using-signalr-performance-counters-in-an-azure-web-role/_static/azure-development-workload.png)

4. W oknie dialogowym **nowa Microsoft Azure usługa w chmurze** wybierz pozycję **rola sieci Web ASP.NET** i wybierz przycisk >, aby dodać rolę do projektu. Kliknij przycisk **OK**.

   ![Dodaj rolę sieci Web ASP.NET](using-signalr-performance-counters-in-an-azure-web-role/_static/image2.png)

5. W oknie dialogowym **Nowa aplikacja sieci Web ASP.NET — WebRole1** wybierz szablon **MVC** , a następnie wybierz przycisk **OK**.

   ![Dodawanie MVC i interfejsu API sieci Web](using-signalr-performance-counters-in-an-azure-web-role/_static/image3.png)

6. W **Eksplorator rozwiązań**Otwórz plik *Diagnostics. Wadcfgx* w obszarze **WebRole1**.

   ![Eksplorator rozwiązań Diagnostics. wadcfgx](using-signalr-performance-counters-in-an-azure-web-role/_static/image4.png)

7. Zastąp zawartość pliku następującą konfiguracją i Zapisz plik:

   [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample1.xml)]

8. Otwórz **konsolę Menedżera pakietów** z **narzędzi** > **Menedżera pakietów NuGet**. Wprowadź następujące polecenia, aby zainstalować najnowszą wersję programu sygnalizującego i pakiet narzędzi sygnalizujących:

   [!code-powershell[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample2.ps1)]

9. Skonfiguruj aplikację, aby zainstalować liczniki wydajności sygnalizujące w wystąpieniu roli podczas jego uruchamiania lub odtwarzania. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt **WebRole1** i wybierz pozycję **Dodaj** > **Nowy folder**. Nazwij nowy folder *startowy*.

   ![Dodaj folder startowy](using-signalr-performance-counters-in-an-azure-web-role/_static/image5.png)

10. Skopiuj plik *sygnalizującer. exe* (dodany z pakietem **Microsoft. ASPNET. signaler.** /SignalRPerfCounters/Packages/Microsoft.ASPNET.SignalR.utils.) z folderu projektu \<>\<wersja >/Tools do folderu *Start* utworzonego w poprzednim kroku.

11. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy folder *startowy* i wybierz polecenie **Dodaj** > **istniejący element**. W wyświetlonym oknie dialogowym wybierz pozycję *sygnalizujący. exe* i wybierz pozycję **Dodaj**.

    ![Dodaj program signaler. exe do projektu](using-signalr-performance-counters-in-an-azure-web-role/_static/image6.png)

12. Kliknij prawym przyciskiem myszy utworzony folder *startowy* . Wybierz pozycję **dodaj** > **nowy element**. Wybierz węzeł **Ogólne** , wybierz pozycję **plik tekstowy**i Nadaj nowemu elementowi *SignalRPerfCounterInstall. cmd*. Ten plik polecenia spowoduje zainstalowanie liczników wydajności sygnalizujących w roli sieci Web.

    ![Utwórz plik wsadowy instalacji licznika wydajności sygnalizujący](using-signalr-performance-counters-in-an-azure-web-role/_static/image7.png)

13. Gdy program Visual Studio tworzy plik *SignalRPerfCounterInstall. cmd* , zostanie automatycznie otwarty w oknie głównym. Zastąp zawartość pliku następującym skryptem, a następnie Zapisz i zamknij plik. Ten skrypt wykonuje program *sygnalizując. exe*, który dodaje liczniki wydajności sygnalizujące do wystąpienia roli.

    [!code-console[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample3.cmd)]

14. Wybierz plik *sygnalizującer. exe* w **Eksplorator rozwiązań**. We **właściwościach**pliku ustaw opcję **Kopiuj do katalogu wyjściowego** na **zawsze Kopiuj**.

    ![Ustaw wartość Kopiuj do katalogu wyjściowego, aby zawsze była kopiowana](using-signalr-performance-counters-in-an-azure-web-role/_static/image8.png)

15. Powtórz poprzedni krok dla pliku *SignalRPerfCounterInstall. cmd* .

16. Kliknij prawym przyciskiem myszy plik *SignalRPerfCounterInstall. cmd* i wybierz polecenie **Otwórz za pomocą**. W wyświetlonym oknie dialogowym wybierz pozycję **Edytor binarny** , a następnie wybierz **przycisk OK**.

    ![Otwórz za pomocą edytora binarnego](using-signalr-performance-counters-in-an-azure-web-role/_static/image9.png)

17. W edytorze binarnym zaznacz wszystkie wiodące bajty w pliku i usuń je. Zapisz i zamknij plik.

    ![Usuń wiodące bajty](using-signalr-performance-counters-in-an-azure-web-role/_static/image10.png)

18. Otwórz plik *ServiceDefinition. csdef* i Dodaj zadanie uruchamiania, które powoduje wykonanie pliku *SignalrPerfCounterInstall. cmd* podczas uruchamiania usługi:

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample4.xml?highlight=4-7)]

19. Otwórz `Views/Shared/_Layout.cshtml` i Usuń skrypt pakietu jQuery z końca pliku.

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample5.cshtml)]

20. Dodaj klienta JavaScript, który stale wywołuje metodę `increment` na serwerze. Otwórz `Views/Home/Index.cshtml` i Zastąp zawartość następującym kodem:

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample6.cshtml)]

21. Utwórz nowy folder w projekcie **WebRole1** o nazwie *Hubs*. Kliknij prawym przyciskiem myszy folder *Hubs* w **Eksplorator rozwiązań** a następnie wybierz pozycję **Dodaj** > **nowy element**. W oknie dialogowym **Dodaj nowy element** wybierz kategorię moduł **sygnalizujący** > **sieci Web** , a następnie wybierz szablon elementu **Klasa centrum sygnałów (v2)** . Nazwij nowe centrum *MyHub.cs* i wybierz pozycję **Dodaj**.

    ![Dodawanie klasy centrum sygnałów do folderu Hubs w oknie dialogowym Dodaj nowy element](using-signalr-performance-counters-in-an-azure-web-role/_static/image13.png)

22. *MyHub.cs* zostanie automatycznie otwarta w oknie głównym. Zastąp zawartość następującym kodem, a następnie Zapisz i zamknij plik:

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample7.cs)]

23. *[Plik. exe](signalr-connection-density-testing-with-crank.md)* jest narzędziem do testowania gęstości połączenia z bazą kodu sygnalizującej. Ponieważ podpory wymaga połączenia trwałego, należy dodać je do lokacji do użycia podczas testowania. Dodaj nowy folder do projektu **WebRole1** o nazwie *PersistentConnections*. Kliknij prawym przyciskiem myszy ten folder i wybierz polecenie **Dodaj** **klasę** > . Nazwij nowy plik klasy *MyPersistentConnections.cs* a następnie wybierz pozycję **Dodaj**.

24. Program Visual Studio otworzy plik *MyPersistentConnections.cs* w oknie głównym. Zastąp zawartość następującym kodem, a następnie Zapisz i zamknij plik:

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample8.cs)]

25. Przy użyciu klasy `Startup` obiekty sygnalizujące są uruchamiane po rozpoczęciu OWIN. Otwórz lub Utwórz *Startup.cs* i Zastąp zawartość następującym kodem:

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample9.cs)]

    W powyższym kodzie atrybut `OwinStartup` oznacza tę klasę do uruchomienia OWIN. Metoda `Configuration` uruchamia sygnał.

26. Przetestuj aplikację w Emulator Microsoft Azure, naciskając klawisz **F5**.

    > [!NOTE]
    > Jeśli napotkasz **FileLoadException** o **MapSignalR**, Zmień przekierowania powiązań w *pliku Web. config* w następujący sposób:

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample12.xml?highlight=3,7)]

27. Odczekaj około jednej minuty. Otwórz okno narzędzia Cloud Explorer w programie Visual Studio (**wyświetl** > **Cloud Explorer**) i rozwiń `(Local)/Storage Accounts/(Development)/Tables`ścieżki. Kliknij dwukrotnie pozycję **WADPerformanceCountersTable**. W danych tabeli powinny być widoczne liczniki sygnalizujące. Jeśli nie widzisz tabeli, może być konieczne ponowne wprowadzenie poświadczeń usługi Azure Storage. Może być konieczne wybranie przycisku **Odśwież** w celu wyświetlenia tabeli w programie **Cloud Explorer** lub wybranie przycisku **Odśwież** w oknie Otwórz tabelę, aby wyświetlić dane w tabeli.

    ![Wybieranie tabeli liczników wydajności funkcji wad w programie Visual Studio Cloud Explorer](using-signalr-performance-counters-in-an-azure-web-role/_static/image11.png)

    ![Wyświetlanie liczników zebranych w tabeli liczników wydajności funkcji wad](using-signalr-performance-counters-in-an-azure-web-role/_static/image12.png)

28. Aby przetestować aplikację w chmurze, zaktualizuj plik **ServiceConfiguration. Cloud. cscfg** i ustaw `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` na prawidłowe parametry połączenia z kontem usługi Azure Storage.

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample10.xml)]

29. Wdróż aplikację w subskrypcji platformy Azure. Aby uzyskać szczegółowe informacje na temat sposobu wdrażania aplikacji na platformie Azure, zobacz [jak utworzyć i wdrożyć usługę w chmurze](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).

30. Zaczekaj kilka minut. W programie **Cloud Explorer**Znajdź konto magazynu skonfigurowane powyżej i Znajdź w nim tabelę `WADPerformanceCountersTable`. W danych tabeli powinny być widoczne liczniki sygnalizujące. Jeśli nie widzisz tabeli, może być konieczne ponowne wprowadzenie poświadczeń usługi Azure Storage. Może być konieczne wybranie przycisku **Odśwież** w celu wyświetlenia tabeli w programie **Cloud Explorer** lub wybranie przycisku **Odśwież** w oknie Otwórz tabelę, aby wyświetlić dane w tabeli.

Specjalne poświęcenie na [Martin Richard](https://social.msdn.microsoft.com/profile/Martin+Richard) dla oryginalnej zawartości używanej w tym samouczku.

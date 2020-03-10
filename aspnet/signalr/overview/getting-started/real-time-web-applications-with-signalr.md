---
uid: signalr/overview/getting-started/real-time-web-applications-with-signalr
title: 'Ćwiczenia: aplikacje sieci Web w czasie rzeczywistym za pomocą sygnalizującego | Microsoft Docs'
author: bradygaster
description: Aplikacje sieci Web w czasie rzeczywistym umożliwiają wypychanie zawartości po stronie serwera do podłączonych klientów w czasie rzeczywistym. Dla deweloperów ASP.NET, ASP...
ms.author: bradyg
ms.date: 07/16/2014
ms.assetid: ba07958c-42e1-4da0-81db-ba6925ed6db0
msc.legacyurl: /signalr/overview/getting-started/real-time-web-applications-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 9e39fd3f2fc9d4e791002450085215096c222fcd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78537101"
---
# <a name="hands-on-lab-real-time-web-applications-with-signalr"></a>Ćwiczenia praktyczne: aplikacje internetowe czasu rzeczywistego z wykorzystaniem usługi SignalR

przez [zespół Camp sieci Web](https://twitter.com/webcamps)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

[Pobierz zestaw szkoleniowy dla sieci Web Camp, wydanie z października 2015](https://github.com/Microsoft-Web/WebCampTrainingKit/releases/tag/v2015.10.13b)

> Aplikacje sieci Web w czasie rzeczywistym umożliwiają wypychanie zawartości po stronie serwera do podłączonych klientów w czasie rzeczywistym. W przypadku deweloperów ASP.NET, **sygnalizujący ASP.NET** to biblioteka umożliwiająca dodawanie funkcji sieci Web w czasie rzeczywistym do aplikacji. Korzysta ona z kilku transportów, automatycznie wybierając najlepszą dostępną transport, z uwzględnieniem najlepszego dostępnego transportu klienta i serwera. Korzysta ona z **protokołu WebSocket**, interfejsu API HTML5, który umożliwia komunikację dwukierunkową między przeglądarką a serwerem.
> 
> **Sygnalizuje** także prosty, wysoki poziom interfejsu API służący do wykonywania wywołań RPC serwera na klientach (wywoływanie funkcji JavaScript w przeglądarkach klientów z kodu .NET po stronie serwera) w aplikacji ASP.NET, a także dodawanie przydatnych punktów zaczepienia do zarządzania połączeniami, takich jak zdarzenia łączenia/rozłączania, grupowanie połączeń i autoryzację.
> 
> **Sygnalizujący** jest abstrakcją dla niektórych transportów, które są wymagane do pracy w czasie rzeczywistym między klientem i serwerem. Połączenie **sygnalizujące** zaczyna się od protokołu HTTP i jest następnie podwyższane do połączenia **WebSocket** , jeśli jest dostępne. Protokół **WebSocket** jest idealnym transportem dla **sygnalizującego**, ponieważ sprawia, że jest to najbardziej wydajne wykorzystanie pamięci serwera, ma najniższe opóźnienie i ma najbardziej podstawowe funkcje (takie jak komunikacja pełnego dupleksu między klientem i serwerem), ale również ma najbardziej rygorystyczne wymagania: protokół **WebSocket** wymaga, aby serwer używał **systemu Windows server 2012** lub **Windows 8**wraz z **.NET Framework 4,5**. Jeśli te wymagania nie są spełnione, program **sygnalizujący** podejmie próbę użycia innych transportów do nawiązywania połączeń (takich jak *AJAX Longing*).
> 
> Interfejs API **sygnalizującego** zawiera dwa modele komunikacji między klientami i serwerami: **trwałe połączenia** i **centra**. **Połączenie** reprezentuje prosty punkt końcowy do wysyłania komunikatów o pojedynczym odbiorcy, pogrupowanych lub emisji. **Koncentrator** to bardziej wysoki poziom potoku oparty na interfejsie API połączenia, który umożliwia klientowi i serwerowi bezpośrednie wywoływanie metod.
> 
> ![Architektura sygnałów](real-time-web-applications-with-signalr/_static/image1.png)
> 
> Wszystkie przykładowe kod i fragmenty kodu są zawarte w zestawie szkoleniowym dla sieci Web Camp, Wydanie października 2015, dostępne w [https://github.com/Microsoft-Web/WebCampTrainingKit/releases/tag/v2015.10.13b](https://github.com/Microsoft-Web/WebCampTrainingKit/releases/tag/v2015.10.13b).  Należy pamiętać, że link Instalatora na tej stronie już nie działa; Zamiast tego użyj jednego z linków w sekcji Assets.

<a id="Overview"></a>
## <a name="overview"></a>Omówienie

<a id="Objectives"></a>
### <a name="objectives"></a>Cele

W tym ćwiczeniu dowiesz się, jak:

- Wysyłanie powiadomień z serwera do klienta przy użyciu usługi sygnalizującej.
- Skalowanie aplikacji sygnalizującej przy użyciu **SQL Server**.

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Wymagania wstępne

Następujące czynności są wymagane do wykonania tego laboratorium praktycznego:

- [Visual Studio Express 2013 dla sieci Web](https://www.microsoft.com/visualstudio/) lub nowszego

<a id="Setup"></a>
### <a name="setup"></a>Konfigurowanie

Aby można było uruchomić ćwiczenia w tym ćwiczeniu, należy najpierw skonfigurować środowisko.

1. Otwórz okno Eksploratora Windows i przejdź do folderu **źródłowego** laboratorium.
2. Kliknij prawym przyciskiem myszy **Setup. cmd** i wybierz polecenie **Uruchom jako administrator** , aby uruchomić proces instalacji, który skonfiguruje środowisko i zainstaluje fragmenty kodu programu Visual Studio dla tego laboratorium.
3. Jeśli zostanie wyświetlone okno dialogowe Kontrola konta użytkownika, potwierdź akcję, aby wykonać operację.

> [!NOTE]
> Upewnij się, że wszystkie zależności dla tego laboratorium zostały sprawdzone przed uruchomieniem Instalatora.

<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>Używanie fragmentów kodu

W całym dokumencie laboratoryjnym pojawi się monit o wstawienie bloków kodu. Dla wygody większość tego kodu jest udostępniana jako fragmenty Visual Studio Code, do których można uzyskać dostęp z poziomu Visual Studio 2013, aby uniknąć konieczności ręcznego dodawania go.

> [!NOTE]
> Każdemu z nich towarzyszy rozpoczęcie rozwiązanie znajdujące się w folderze **BEGIN** w ćwiczeniu, który umożliwia wykonywanie poszczególnych czynności niezależnie od innych. Należy pamiętać, że fragmenty kodu dodawane podczas wykonywania nie są dostępne w tych rozwiązaniach i mogą nie działać do czasu ukończenia ćwiczenia. Wewnątrz kodu źródłowego dla ćwiczenia znajdziesz również folder **końcowy** zawierający rozwiązanie programu Visual Studio z kodem, który wynika z wykonania czynności w odpowiednim ćwiczeniu. Te rozwiązania można wykorzystać jako wskazówkę, jeśli potrzebujesz dodatkowej pomocy podczas pracy w ramach tego praktycznego laboratorium.

---

<a id="Exercises"></a>
## <a name="exercises"></a>Symulacyjn

To laboratorium praktyczne obejmuje następujące ćwiczenia:

1. [Praca z danymi w czasie rzeczywistym przy użyciu usługi sygnalizującej](#Exercise1)
2. [Skalowanie w dół przy użyciu SQL Server](#Exercise2)

Szacowany czas wykonywania tego laboratorium: **60 minut**

> [!NOTE]
> Po pierwszym uruchomieniu programu Visual Studio należy wybrać jedną z wstępnie zdefiniowanych kolekcji ustawień. Każda wstępnie zdefiniowana kolekcja jest zaprojektowana tak, aby była zgodna z konkretnym stylem deweloperskim i określa układy okien, zachowanie edytora, fragmenty kodu IntelliSense i opcje okna dialogowego. Procedury przedstawione w tym laboratorium opisują akcje niezbędne do wykonania danego zadania w programie Visual Studio, gdy jest używana **Ogólna kolekcja ustawień deweloperskich** . W przypadku wybrania innej kolekcji ustawień dla środowiska programistycznego mogą wystąpić różnice w czynnościach, które należy wziąć pod uwagę.

<a id="Exercise1"></a>
### <a name="exercise-1-working-with-real-time-data-using-signalr"></a>Ćwiczenie 1: Praca z danymi w czasie rzeczywistym przy użyciu usługi sygnalizującej

Podczas gdy rozmowa jest często używana na przykład, można wykonać całą wiele czynności dzięki funkcjom sieci Web w czasie rzeczywistym. Za każdym razem, gdy użytkownik Odświeża stronę sieci Web, aby zobaczyć nowe dane, lub strona implementuje długotrwałe sondowanie AJAX, aby pobrać nowe dane, można użyć sygnalizującego.

Sygnalizujący obsługuje funkcje **wypychania** lub **rozgłaszania** serwera; obsługuje ona zarządzanie połączeniami automatycznie. W przypadku klasycznych połączeń HTTP dla komunikacji klient-serwer połączenie jest ponownie ustanawiane dla każdego żądania, ale sygnalizujący zapewnia trwałe połączenie między klientem a serwerem. W programie sygnalizujący kod serwera wywołuje kod klienta w przeglądarce przy użyciu zdalnych wywołań procedur (RPC), a nie modelu odpowiedzi Request-Response już dzisiaj.

W tym ćwiczeniu skonfigurujesz aplikację **quizu pojedynek maniaków komputerowych** , która będzie używać programu sygnalizującego do wyświetlania pulpitu nawigacyjnego statystyk z zaktualizowanymi metrykami bez konieczności odświeżania całej strony.

<a id="Ex1Task1"></a>
#### <a name="task-1--exploring-the-geek-quiz-statistics-page"></a>Zadanie 1 — Eksplorowanie strony statystyki quizu pojedynek maniaków komputerowych

W tym zadaniu przejdziesz przez aplikację i sprawdzisz, jak wyświetlana jest strona Statystyka oraz jak można poprawić sposób aktualizowania informacji.

1. Otwórz **Visual Studio Express 2013 dla sieci Web** i Otwórz rozwiązanie **GeekQuiz. sln** znajdujące się w folderze **Source\Ex1-WorkingWithRealTimeData\Begin** .
2. Naciśnij klawisz **F5** , aby uruchomić rozwiązanie. Strona **logowania** powinna zostać wyświetlona w przeglądarce.

    ![Uruchamianie rozwiązania](real-time-web-applications-with-signalr/_static/image2.png "Uruchamianie rozwiązania")

    *Uruchamianie rozwiązania*
3. Kliknij pozycję **zarejestruj** w prawym górnym rogu strony, aby utworzyć nowego użytkownika w aplikacji.

    ![Zarejestruj łącze](real-time-web-applications-with-signalr/_static/image3.png "Zarejestruj łącze")

    *Zarejestruj łącze*
4. Na stronie **Rejestr** wprowadź **nazwę użytkownika** i **hasło**, a następnie kliknij pozycję **zarejestruj**.

    ![Rejestrowanie użytkownika](real-time-web-applications-with-signalr/_static/image4.png "Rejestrowanie użytkownika")

    *Rejestrowanie użytkownika*
5. Aplikacja rejestruje nowe konto, a użytkownik zostaje uwierzytelniony i przekierowany z powrotem na stronę główną przedstawiającą pierwsze pytanie quizu.
6. Otwórz stronę **Statystyka** w nowym oknie i umieść ją **obok strony** **głównej** .

    ![Okna równoległe](real-time-web-applications-with-signalr/_static/image5.png "Okna sąsiadujące")

    *Okna równoległe*
7. Na stronie **głównej** Odpowiedz na pytanie, klikając jedną z opcji.

    ![Odpowiadanie na pytanie](real-time-web-applications-with-signalr/_static/image6.png "Odpowiadanie na pytanie")

    *Odpowiadanie na pytanie*
8. Po kliknięciu jednego z przycisków powinna zostać wyświetlona odpowiedź.

    ![Odpowiedź na pytanie poprawna](real-time-web-applications-with-signalr/_static/image7.png "Odpowiedź na pytanie poprawna")

    *Odpowiedź na pytanie poprawna*
9. Zwróć uwagę, że informacje podane na stronie Statystyka są nieaktualne. Odśwież stronę, aby zobaczyć zaktualizowane wyniki.

    ![Strona statystyki](real-time-web-applications-with-signalr/_static/image8.png "Strona statystyki")

    *Strona statystyki*
10. Wróć do programu Visual Studio i Zatrzymaj debugowanie.

<a id="Ex1Task2"></a>
#### <a name="task-2--adding-signalr-to-geek-quiz-to-show-online-charts"></a>Zadanie 2 — Dodawanie sygnału do pojedynek maniaków komputerowych quizu w celu pokazania wykresów online

W tym zadaniu dodasz sygnał do rozwiązania i wyślesz aktualizacje do klientów automatycznie po wysłaniu nowej odpowiedzi na serwer.

1. W menu **Narzędzia** w programie Visual Studio wybierz pozycję **Menedżer pakietów NuGet**, a następnie kliknij pozycję **konsola Menedżera pakietów**.
2. W oknie **konsola Menedżera pakietów** wykonaj następujące polecenie:

    [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample1.ps1)]

    ![Instalacja pakietu sygnalizującego](real-time-web-applications-with-signalr/_static/image9.png "Instalacja pakietu sygnalizującego")

    *Instalacja pakietu sygnalizującego*

   > [!NOTE]
   > Podczas instalowania pakietów NuGet usługi **sygnalizującego** w wersji 2.0.2 z zupełnie nowej aplikacji MVC 5 należy ręcznie zaktualizować pakiety **Owin** do wersji 2.0.1 (lub nowszej) przed zainstalowaniem programu sygnalizującego. W tym celu można wykonać następujący skrypt w **konsoli Menedżera pakietów**:
   > 
   > [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample2.ps1)]
   > 
   > W przyszłych wydaniach sygnału OWIN zależności zostaną automatycznie zaktualizowane.
3. W **Eksplorator rozwiązań**rozwiń folder **skrypty** i zwróć uwagę, że pliki *js* programu sygnalizujące zostały dodane do rozwiązania.

    ![Dokumentacja języka JavaScript dla sygnałów](real-time-web-applications-with-signalr/_static/image10.png "Dokumentacja języka JavaScript dla sygnałów")

    *Dokumentacja języka JavaScript dla sygnałów*
4. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt **GeekQuiz** , wybierz pozycję **Dodaj** | **Nowy folder**i nadaj nazwę **centram**IT.
5. Kliknij prawym przyciskiem myszy folder **Hubs** i wybierz polecenie **Dodaj | Nowy element**.

    ![Dodaj nowy element](real-time-web-applications-with-signalr/_static/image11.png "Dodaj nowy element")

    *Dodaj nowy element*
6. W oknie dialogowym **Dodaj nowy element** wybierz  **C# wizualizację | Sieć Web | Węzeł sygnalizujący** w lewym okienku, wybierz pozycję **Klasa centrum sygnałów (v2)** w środkowym okienku, Nazwij plik **StatisticsHub.cs** i kliknij przycisk **Dodaj**.

    ![Okno dialogowe Dodawanie nowego elementu](real-time-web-applications-with-signalr/_static/image12.png "Okno dialogowe Dodawanie nowego elementu")

    *Okno dialogowe Dodawanie nowego elementu*
7. Zastąp kod w klasie **StatisticsHub** następującym kodem.

    (Fragment kodu- *RealTimeSignalR-Ex1-StatisticsHubClass*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample3.cs)]
8. Otwórz **Startup.cs** i Dodaj następujący wiersz na końcu metody **konfiguracji** .

    (Fragment kodu- *RealTimeSignalR-Ex1-MapSignalR*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample4.cs)]
9. Otwórz stronę **StatisticsService.cs** wewnątrz folderu **Services** i Dodaj następujące dyrektywy using.

    (Fragment kodu- *RealTimeSignalR-Ex1-UsingDirectives*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample5.cs)]
10. Aby powiadomić podłączonych klientów o aktualizacjach, należy najpierw pobrać obiekt **kontekstu** dla bieżącego połączenia. Obiekt **Hub** zawiera metody wysyłania komunikatów do pojedynczego klienta lub emisji do wszystkich podłączonych klientów. Dodaj następującą metodę do klasy **StatisticsService** , aby emitować dane statystyczne.

    (Fragment kodu- *RealTimeSignalR-Ex1-NotifyUpdatesMethod*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample6.cs)]

    > [!NOTE]
    > W powyższym kodzie używasz dowolnej nazwy metody do wywołania funkcji na kliencie (tj.: *updateStatistics*). Określona nazwa metody jest interpretowana jako obiekt dynamiczny, co oznacza, że nie istnieje funkcja IntelliSense ani weryfikacja w czasie kompilacji. Wyrażenie jest oceniane w czasie wykonywania. Gdy wywołanie metody jest wykonywane, sygnalizujący wysyła nazwę metody i wartości parametrów do klienta. Jeśli klient ma metodę zgodną z nazwą, ta metoda jest wywoływana i wartości parametrów są do niego przenoszone. Jeśli na kliencie nie zostanie znaleziona zgodna Metoda, żaden błąd nie zostanie zgłoszony. Aby uzyskać więcej informacji, zobacz [Przewodnik interfejsu API centrów sygnałów ASP.NET](../guide-to-the-api/hubs-api-guide-server.md).
11. Otwórz stronę **TriviaController.cs** wewnątrz folderu **controllers** i Dodaj następujące dyrektywy using.

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample7.cs)]
12. Dodaj następujący wyróżniony kod do metody akcji **post** .

    (Fragment kodu- *RealTimeSignalR-Ex1-NotifyUpdatesCall*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample8.cs)]
13. Otwórz stronę **Statistics. cshtml** w **widokach | Folder macierzysty** . Znajdź sekcję **skrypty** i Dodaj następujące odwołania do skryptów na początku sekcji.

    (Fragment kodu- *RealTimeSignalR-Ex1-SignalRScriptReferences*)

    [!code-cshtml[Main](real-time-web-applications-with-signalr/samples/sample9.cshtml)]

    > [!NOTE]
    > Po dodaniu sygnalizującego i innych bibliotek skryptów do projektu programu Visual Studio Menedżer pakietów może zainstalować wersję pliku skryptu sygnalizującego, która jest nowsza niż wersja pokazana w tym temacie. Upewnij się, że odwołanie do skryptu w kodzie jest zgodne z wersją biblioteki skryptów zainstalowaną w projekcie.
14. Dodaj następujący wyróżniony kod, aby połączyć klienta z centrum sygnałów i zaktualizować dane statystyczne po odebraniu nowej wiadomości z centrum.

    (Fragment kodu- *RealTimeSignalR-Ex1-SignalRClientCode*)

    [!code-cshtml[Main](real-time-web-applications-with-signalr/samples/sample10.cshtml)]

    W tym kodzie tworzysz serwer proxy centrum i zarejestrowano procedurę obsługi zdarzeń w celu nasłuchiwania komunikatów wysyłanych przez serwer. W tym przypadku użytkownik nasłuchuje komunikatów wysyłanych za pomocą metody *updateStatistics* .

<a id="Ex1Task3"></a>
#### <a name="task-3--running-the-solution"></a>Zadanie 3 — Uruchamianie rozwiązania

W tym zadaniu zostanie uruchomione rozwiązanie, aby sprawdzić, czy widok statystyki jest aktualizowany automatycznie przy użyciu programu sygnalizującego po odebraniu nowego pytania.

1. Naciśnij klawisz **F5** , aby uruchomić rozwiązanie.

    > [!NOTE]
    > Jeśli nie zostało to jeszcze zalogowane w aplikacji, zaloguj się przy użyciu użytkownika utworzonego w ramach zadania 1.
2. Otwórz stronę **statystyki** w nowym oknie i umieść stronę **główną** i strony **statystyk** obok siebie jak w zadaniu 1.
3. Na stronie **głównej** Odpowiedz na pytanie, klikając jedną z opcji.

    ![Odpowiadanie na inne pytanie](real-time-web-applications-with-signalr/_static/image13.png "Odpowiadanie na inne pytanie")

    *Odpowiadanie na inne pytanie*
4. Po kliknięciu jednego z przycisków powinna zostać wyświetlona odpowiedź. Zwróć uwagę, że informacje statystyczne na stronie są aktualizowane automatycznie po odebraniu pytania z zaktualizowanymi informacjami bez konieczności odświeżania całej strony.

    ![Strona statystyki została odświeżona po odebraniu odpowiedzi](real-time-web-applications-with-signalr/_static/image14.png "Strona statystyki została odświeżona po odebraniu odpowiedzi")

    *Strona statystyki została odświeżona po odebraniu odpowiedzi*

<a id="Exercise2"></a>
### <a name="exercise-2-scaling-out-using-sql-server"></a>Ćwiczenie 2: skalowanie w dół przy użyciu SQL Server

W przypadku skalowania aplikacji sieci Web można ogólnie wybrać opcję *skalowanie w górę* i *skalowanie* w dół. *Skalowanie w górę* oznacza użycie większego serwera, z większą ilością zasobów (procesor CPU, pamięć RAM itp.) podczas *skalowania w poziomie* oznacza dodanie większej liczby serwerów do obsługi obciążenia. Problem z tym drugim polega na tym, że klienci mogą być kierowani do różnych serwerów. Klient połączony z jednym serwerem nie będzie odbierać komunikatów wysyłanych z innego serwera.

Te problemy można rozwiązać przy użyciu składnika o nazwie " *Plan*", aby przekazywać komunikaty między serwerami. Po włączeniu obsługi planów, każde wystąpienie aplikacji wysyła komunikaty do planu i planuje je do innych wystąpień aplikacji.

Obecnie istnieją trzy typy planów dla sygnalizującego:

- **Azure Service Bus systemu Windows**. Service Bus to infrastruktura obsługi komunikatów, która umożliwia składnikom wysyłanie luźno sprzężonych komunikatów.
- **SQL Server**. SQL Server planuje zapisywanie komunikatów w tabelach SQL. Plan nie używa Service Broker do wydajnej obsługi komunikatów. Jednak działa również wtedy, gdy Service Broker nie jest włączona.
- **Redis**. Redis jest magazynem wartości w pamięci. Redis obsługuje wzorzec publikowania/subskrybowania ("pub/Sub") do wysyłania wiadomości.

Każdy komunikat jest wysyłany przez magistralę komunikatów. Magistrala komunikatów implementuje interfejs [IMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.imessagebus(v=vs.100).aspx) , który zapewnia abstrakcję publikowania/subskrybowania. Planuje działanie, zastępując domyślne **IMessageBus** magistralą zaprojektowaną dla tego planu.

Każde wystąpienie serwera nawiązuje połączenie z planem w celu przechodzenia przez magistralę. Po wysłaniu wiadomości zostanie ona zaplanowana, a plan nie wysyła go do każdego serwera. Gdy serwer odbiera komunikat z planu, przechowuje komunikat w lokalnej pamięci podręcznej. Serwer następnie dostarcza komunikaty do klientów ze swojej lokalnej pamięci podręcznej.

Aby uzyskać więcej informacji na temat sposobu działania programu sygnalizującego, przeczytaj ten [artykuł](../performance/scaleout-in-signalr.md).

> [!NOTE]
> Istnieje kilka scenariuszy, w których plan może stać się wąskim gardłem. Poniżej przedstawiono kilka typowych scenariuszy sygnalizujących:
> 
> - [Emisja serwera](tutorial-server-broadcast-with-signalr.md) (np. giełdowego): plany pracy dla tego scenariusza są odpowiednie, ponieważ serwer kontroluje szybkość, z jaką wysyłane są komunikaty.
> - [Klient-Klient](tutorial-getting-started-with-signalr.md) (np. rozmowa): w tym scenariuszu plan może być wąskim gardłem, jeśli liczba komunikatów skaluje się do liczby klientów. oznacza to, że jeśli szybkość komunikatów rośnie proporcjonalnie do przyłączenia większej liczby klientów.
> - [Wysoka częstotliwość](tutorial-high-frequency-realtime-with-signalr.md) w czasie rzeczywistym (np. w grach czasu rzeczywistego): w tym scenariuszu nie zaleca się planowania.

W tym ćwiczeniu będziesz używać **SQL Server** do dystrybuowania komunikatów w aplikacji **quizu pojedynek maniaków komputerowych** . Te zadania zostaną uruchomione na pojedynczym komputerze testowym, aby dowiedzieć się, jak skonfigurować konfigurację, ale w celu uzyskania pełnego efektu należy wdrożyć aplikację sygnalizującą na co najmniej dwóch serwerach. Należy również zainstalować SQL Server na jednym z serwerów lub na osobnym dedykowanym serwerze.

![Skalowanie w poziomie przy użyciu diagramu SQL Server](real-time-web-applications-with-signalr/_static/image15.png)

<a id="Ex2Task1"></a>
#### <a name="task-1---understanding-the-scenario"></a>Zadanie 1 — zrozumienie scenariusza

W tym zadaniu zostanie uruchomione 2 wystąpienia **quizu pojedynek maniaków komputerowych** , które symulują wiele wystąpień usług IIS na komputerze lokalnym. W tym scenariuszu podczas odpowiadania na pytania kwizy w jednej aplikacji aktualizacja nie będzie powiadamiana na stronie statystyk drugiego wystąpienia. Ta symulacja przypomina środowisko, w którym aplikacja jest wdrażana w wielu wystąpieniach i używa modułu równoważenia obciążenia do komunikowania się z nimi.

1. Otwórz rozwiązanie **BEGIN. sln** znajdujące się w folderze **Source/Ex2-ScalingOutWithSQLServer/BEGIN** . Po załadowaniu należy **Eksplorator serwera** zauważyć, że rozwiązanie ma dwa projekty z identycznymi strukturami, ale o różnych nazwach. Spowoduje to zasymulowanie uruchamiania dwóch wystąpień tej samej aplikacji na komputerze lokalnym.

    ![Rozpocznij rozwiązanie symuluje 2 wystąpienia quizu pojedynek maniaków komputerowych](real-time-web-applications-with-signalr/_static/image16.png "Rozpocznij rozwiązanie symuluje 2 wystąpienia quizu pojedynek maniaków komputerowych")

    *Rozpocznij rozwiązanie symuluje 2 wystąpienia quizu pojedynek maniaków komputerowych*
2. Otwórz stronę właściwości rozwiązania, klikając prawym przyciskiem myszy węzeł rozwiązania i wybierając polecenie **Właściwości**. W obszarze **projekt startowy**wybierz opcję **wiele projektów startowych** , a następnie zmień wartość **akcji** dla obu projektów na *Rozpocznij*.

    ![Uruchamianie wielu projektów](real-time-web-applications-with-signalr/_static/image17.png "Uruchamianie wielu projektów")

    *Uruchamianie wielu projektów*
3. Naciśnij klawisz **F5** , aby uruchomić rozwiązanie. Aplikacja uruchomi dwa wystąpienia **quizu pojedynek maniaków komputerowych** w różnych portach, symulując wiele wystąpień tej samej aplikacji. Przypnij jedną z przeglądarek po lewej stronie i drugą na prawo od ekranu. Zaloguj się przy użyciu swoich poświadczeń lub Zarejestruj nowego użytkownika. Po zalogowaniu Zachowaj stronę kwizy po lewej stronie i przejdź do strony **Statystyka** w przeglądarce po prawej.

    ![Pojedynek maniaków komputerowych Quiz obok siebie](real-time-web-applications-with-signalr/_static/image18.png)

    *Pojedynek maniaków komputerowych Quiz obok siebie*

    ![Quiz pojedynek maniaków komputerowych w różnych portach](real-time-web-applications-with-signalr/_static/image19.png)

    *Quiz pojedynek maniaków komputerowych w różnych portach*
4. Zacznij odpowiadać na pytania w lewej przeglądarce, aby zauważyć, że strona **statystyk** w odpowiedniej przeglądarce nie jest aktualizowana. Wynika to z faktu, że usługa **sygnalizująca** korzysta z lokalnej pamięci podręcznej do dystrybuowania komunikatów na klientach, a ten scenariusz symuluje wiele wystąpień, dlatego pamięć podręczna nie jest udostępniana między nimi. Możesz sprawdzić, czy program **sygnalizujący** działa, testując te same kroki, ale używając jednej aplikacji. Poniższe zadania umożliwiają skonfigurowanie planu w celu replikowania komunikatów między wystąpieniami.
5. Wróć do programu Visual Studio i Zatrzymaj debugowanie.

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-the-sql-server-backplane"></a>Zadanie 2 — Tworzenie planu SQL Server

W tym zadaniu utworzysz bazę danych, która będzie działać jako plan dla aplikacji **quizu pojedynek maniaków komputerowych** . Będziesz używać **Eksplorator obiektów SQL Server** , aby przeglądać serwer i inicjować bazę danych. Ponadto zostanie włączona **Service Broker**.

1. W programie **Visual Studio**Otwórz **Widok** menu i wybierz **Eksplorator obiektów SQL Server**.
2. Połącz się z wystąpieniem LocalDB, klikając prawym przyciskiem myszy węzeł **SQL Server** i wybierając polecenie **Dodaj SQL Server...** .

    ![Dodawanie wystąpienia SQL Server](real-time-web-applications-with-signalr/_static/image20.png "Dodawanie wystąpienia SQL Server")

    *Dodawanie wystąpienia SQL Server do Eksplorator obiektów SQL Server*
3. Ustaw **nazwę serwera** na *(LocalDB) \V11.0* i pozostaw **uwierzytelnianie systemu Windows** jako tryb uwierzytelniania. Kliknij pozycję **Połącz**, aby kontynuować.

    ![Nawiązywanie połączenia z usługą LocalDB](real-time-web-applications-with-signalr/_static/image21.png "Nawiązywanie połączenia z usługą LocalDB")

    *Nawiązywanie połączenia z usługą LocalDB*
4. Teraz, gdy masz połączenie z wystąpieniem usługi LocalDB, musisz utworzyć bazę danych, która będzie reprezentować SQL Server plan dla sygnalizującego. Aby to zrobić, kliknij prawym przyciskiem myszy węzeł **bazy danych** i wybierz polecenie **Dodaj nową bazę danych**.

    ![Dodawanie nowej bazy danych](real-time-web-applications-with-signalr/_static/image22.png "Dodawanie nowej bazy danych")

    *Dodawanie nowej bazy danych*
5. Ustaw nazwę bazy danych na *sygnalizacja* i kliknij przycisk **OK** , aby ją utworzyć.

    ![Tworzenie bazy danych sygnalizującego](real-time-web-applications-with-signalr/_static/image23.png "Tworzenie bazy danych sygnalizującego")

    *Tworzenie bazy danych sygnalizującego*

    > [!NOTE]
    > Możesz wybrać dowolną nazwę bazy danych.
6. Aby szybciej otrzymywać aktualizacje z planu, zaleca się włączenie Service Broker bazy danych. Service Broker zapewnia natywną obsługę komunikatów i kolejkowania w programie SQL Server. Plan nie działa również bez Service Broker. Otwórz nowe zapytanie, klikając prawym przyciskiem myszy bazę danych i wybierz polecenie **nowe zapytanie**.

    ![Otwieranie nowego zapytania](real-time-web-applications-with-signalr/_static/image24.png "Otwieranie nowego zapytania")

    *Otwieranie nowego zapytania*
7. Aby sprawdzić, czy Service Broker jest włączona, zbadaj kolumnę **is\_Broker\_Enabled** w widoku wykazu **sys. databases** . Wykonaj następujący skrypt w ostatnio otwartym oknie zapytania.

    [!code-sql[Main](real-time-web-applications-with-signalr/samples/sample11.sql)]

    ![Wykonywanie zapytania dotyczącego stanu Service Broker](real-time-web-applications-with-signalr/_static/image25.png "Wykonywanie zapytania dotyczącego stanu Service Broker")

    *Wykonywanie zapytania dotyczącego stanu Service Broker*
8. Jeśli wartość kolumny " **\_broker\_włączona** w bazie danych jest &quot;0&quot;, użyj następującego polecenia, aby je włączyć. Zastąp **&lt;&gt;bazy danych** nazwą ustawioną podczas tworzenia bazy danych (np.: sygnalizującer).

    [!code-sql[Main](real-time-web-applications-with-signalr/samples/sample12.sql)]

    ![Włączanie Service Broker](real-time-web-applications-with-signalr/_static/image26.png "Włączanie Service Broker")

    *Włączanie Service Broker*

    > [!NOTE]
    > Jeśli ta kwerenda wydaje się być zakleszczeniem, upewnij się, że żadne aplikacje nie są połączone z bazą danych.

<a id="Ex2Task3"></a>
#### <a name="task-3--configuring-the-signalr-application"></a>Zadanie 3 — Konfigurowanie aplikacji sygnalizującej

W tym zadaniu skonfigurujesz **Quiz pojedynek maniaków komputerowych** , aby połączyć się z planem SQL Server. Najpierw dodamy pakiet NuGet programu **signaler. SqlServer** i ustawimy parametry połączenia z bazą danych.

1. Otwórz **konsolę Menedżera pakietów** z **narzędzi** > **Menedżera pakietów NuGet**. Upewnij się, że na liście rozwijanej **domyślny projekt** jest wybrany projekt **GeekQuiz** . Wpisz następujące polecenie, aby zainstalować pakiet NuGet **Microsoft. ASPNET. Signal. SqlServer** .

    [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample13.ps1)]
2. Powtórz poprzedni krok, ale ten czas dla projektu **GeekQuiz2**.
3. Aby skonfigurować plan SQL Server, Otwórz plik **Startup.cs** projektu **GeekQuiz** i Dodaj następujący kod do metody **Configure** . Zastąp **&lt;&gt;bazy** danych nazwą swojej bazy danych, która została użyta podczas tworzenia planu SQL Server. Powtórz ten krok dla projektu **GeekQuiz2** .

    (Fragment kodu- *RealTimeSignalR-Ex2-StartupConfiguration*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample14.cs)]
4. Teraz, gdy oba projekty są skonfigurowane do używania planu SQL Server, naciśnij klawisz **F5** , aby uruchomić je jednocześnie.
5. Ponownie program **Visual Studio** uruchomi dwa wystąpienia **quizu pojedynek maniaków komputerowych** na różnych portach. Przypnij jedną z przeglądarek po lewej stronie, a drugą z prawej strony ekranu i zaloguj się przy użyciu swoich poświadczeń. Pozostaw stronę kwizy po lewej stronie i przejdź do strony **statystyk** w odpowiedniej przeglądarce.
6. Zacznij odpowiadać na pytania w przeglądarce po lewej stronie. Tym razem strona **statystyki** zostanie zaktualizowana w ramach planu pracy z planem. Przełączaj między aplikacjami (**statystyki** są teraz po lewej stronie, a **kwizy** znajduje się po prawej) i Powtarzaj test, aby sprawdzić, czy działa on w obu wystąpieniach. Plan jest używany jako *udostępniona pamięć podręczna* komunikatów dla każdego połączonego serwera, a każdy serwer będzie przechowywał komunikaty w swojej lokalnej pamięci podręcznej w celu dystrybucji do podłączonych klientów.
7. Wróć do programu Visual Studio i Zatrzymaj debugowanie.
8. Składnik SQL Servera planu automatycznego generuje niezbędne tabele w określonej bazie danych. W panelu **Eksplorator obiektów SQL Server** Otwórz bazę danych utworzoną dla planu (np.: sygnalizującer) i rozwiń jej tabele. Powinny zostać wyświetlone następujące tabele:

    ![Tabele wygenerowane przez plan](real-time-web-applications-with-signalr/_static/image27.png)

    *Tabele wygenerowane przez plan*
9. Kliknij prawym przyciskiem myszy składnik **sygnalizujący. messages\_0** Table i wybierz polecenie **Wyświetl dane**.

    ![Tabela komunikatów planu](real-time-web-applications-with-signalr/_static/image28.png)

    *Tabela komunikatów planu*
10. Podczas odpowiadania na pytania kwizy można zobaczyć różne komunikaty wysyłane do **centrum** . Plan moje rozesłać te komunikaty do dowolnego połączonego wystąpienia.

    ![Tabela nieplanowanych komunikatów](real-time-web-applications-with-signalr/_static/image29.png)

    *Tabela nieplanowanych komunikatów*

---

<a id="Summary"></a>
## <a name="summary"></a>Podsumowanie

W tym ćwiczeniu dowiesz się, jak dodać **sygnał** do aplikacji i wysyłać powiadomienia z serwera do podłączonych klientów przy użyciu **centrów**. Ponadto wiesz już, jak skalować aplikację przy użyciu składnika dla *planu* , gdy aplikacja jest wdrażana w wielu WYSTĄPIENIACH usług IIS.

---
uid: signalr/overview/older-versions/tutorial-server-broadcast-with-aspnet-signalr
title: 'Samouczek: emisja serwera z sygnałem ASP.NET 1. x | Microsoft Docs'
author: bradygaster
description: W tym samouczku pokazano, jak utworzyć aplikację sieci Web korzystającą z sygnalizującego ASP.NET, aby zapewnić funkcję emisji serwera. Emisja serwera oznacza, że gmina...
ms.author: bradyg
ms.date: 04/10/2013
ms.assetid: ab7b2554-956a-4f6d-b2a0-4ae0c62e8580
msc.legacyurl: /signalr/overview/older-versions/tutorial-server-broadcast-with-aspnet-signalr
msc.type: authoredcontent
ms.openlocfilehash: 68908be34f6b010e512677fe5f5e31bfdefab592
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78579409"
---
# <a name="tutorial-server-broadcast-with-aspnet-signalr-1x"></a>Samouczek: emisje serwera z użyciem wzorca ASP.NET SignalR 1.x

[Fletcher Patryk](https://github.com/pfletcher), [Tomasz Dykstra](https://github.com/tdykstra)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> W tym samouczku pokazano, jak utworzyć aplikację sieci Web korzystającą z sygnalizującego ASP.NET, aby zapewnić funkcję emisji serwera. Emisja serwera oznacza, że komunikacja wysyłana do klientów jest inicjowana przez serwer. W tym scenariuszu wymagane jest inne podejście programistyczne niż scenariusze komunikacji równorzędnej, takie jak aplikacje czatu, w którym komunikacja wysyłana do klientów jest inicjowana przez co najmniej jednego klienta.
> 
> Aplikacja, którą utworzysz w tym samouczku, symuluje taktowanie giełdowe, typowy scenariusz dla funkcji emisji serwera.
> 
> Komentarze w samouczku są powitane. Jeśli masz pytania, które nie są bezpośrednio związane z samouczkiem, możesz je ogłosić na [forum ASP.NET](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com).

## <a name="overview"></a>Omówienie

[Microsoft. ASPNET. signaler. przykładowy](http://nuget.org/packages/microsoft.aspnet.signalr.sample) pakiet NuGet instaluje przykład symulowanej aplikacji giełdowej w projekcie programu Visual Studio. W pierwszej części tego samouczka utworzysz uproszczoną wersję tej aplikacji od podstaw. W pozostałej części tego samouczka zainstalujesz pakiet NuGet i zapoznajesz dodatkowe funkcje i kod, który tworzy.

Aplikacja ticker jest reprezentatywna dla rodzaju aplikacji w czasie rzeczywistym, w której chcesz okresowo wypchnięciować lub emitować powiadomienia z serwera do wszystkich połączonych klientów.

Aplikacja, która zostanie utworzona w pierwszej części tego samouczka, wyświetli siatkę z danymi giełdowymi.

![Wersja wstępna StockTicker](tutorial-server-broadcast-with-aspnet-signalr/_static/image1.png)

Okresowo aktualizuje ceny giełdowe serwera i wypychanie aktualizacji do wszystkich połączonych klientów. W przeglądarce numery i symbole w kolumnach **Zmień** i **%** zmieniają się dynamicznie w odpowiedzi na powiadomienia z serwera. Jeśli otworzysz dodatkowe przeglądarki pod tym samym adresem URL, wszystkie będą wyświetlać te same dane i te same zmiany w danych jednocześnie.

Ten samouczek zawiera następujące sekcje:

- [Wymagania wstępne](#prerequisites)
- [Tworzenie projektu](#createproject)
- [Dodaj pakiety NuGet sygnalizującego](#nugetpackages)
- [Konfigurowanie kodu serwera](#server)
- [Konfigurowanie kodu klienta](#client)
- [Testowanie aplikacji](#test)
- [Włączanie rejestrowania](#enablelogging)
- [Instalowanie i przeglądanie pełnego przykładu StockTicker](#fullsample)
- [Następne kroki](#nextsteps)

> [!NOTE]
> Jeśli nie chcesz wykonać kroków tworzenia aplikacji, możesz zainstalować program sygnalizujący. przykładowy pakiet w nowym, **pustym projekcie aplikacji sieci Web ASP.NET** , i przeczytaj te kroki, aby uzyskać wyjaśnienia dotyczące kodu. Pierwsza część samouczka obejmuje podzestaw próbnika. przykładowy kod, a druga część objaśnia najważniejsze funkcje dodatkowych funkcji w programie Sygnalizującer. przykładowego pakietu.

<a id="prerequisites"></a>

## <a name="prerequisites"></a>Wymagania wstępne

Przed rozpoczęciem upewnij się, że na komputerze jest zainstalowany program Visual Studio 2012 lub 2010 z dodatkiem SP1. Jeśli nie masz programu Visual Studio, zobacz [ASP.NET downloads](https://www.asp.net/downloads) , aby uzyskać bezpłatny program Visual Studio 2012 Express for Web.

Jeśli masz program Visual Studio 2010, upewnij się, że jest zainstalowany pakiet [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) .

<a id="createproject"></a>

## <a name="create-the-project"></a>Tworzenie projektu

1. W menu **plik** kliknij pozycję **Nowy projekt**.
2. W oknie dialogowym **Nowy projekt** rozwiń węzeł **C#** w obszarze **Szablony** i wybierz pozycję **Sieć Web**.
3. Wybierz szablon **pustej aplikacji sieci Web ASP.NET** , nadaj nazwę *sygnalizującemu projektowi. StockTicker*, a następnie kliknij przycisk **OK**.

    ![Okno dialogowe Nowy projekt](tutorial-server-broadcast-with-aspnet-signalr/_static/image2.png)

<a id="nugetpackages"></a>

## <a name="add-the-signalr-nuget-packages"></a>Dodaj pakiety NuGet sygnalizującego

### <a name="add-the-signalr-and-jquery-nuget-packages"></a>Dodaj pakiet sygnalizujący i pakiety NuGet programu JQuery

Do projektu można dodać funkcję sygnalizującą, instalując pakiet NuGet.

1. Kliknij pozycję **Narzędzia | Menedżer pakietów NuGet | Konsola Menedżera pakietów**.
2. W Menedżerze pakietów wprowadź następujące polecenie.

    [!code-powershell[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample1.ps1)]

    Pakiet sygnalizujący instaluje wiele innych pakietów NuGet jako zależności. Po zakończeniu instalacji masz wszystkie składniki serwera i klienta wymagane do użycia programu sygnalizującego w aplikacji ASP.NET.

<a id="server"></a>

## <a name="set-up-the-server-code"></a>Konfigurowanie kodu serwera

W tej sekcji skonfigurujesz kod, który jest uruchamiany na serwerze programu.

### <a name="create-the-stock-class"></a>Tworzenie klasy giełdowej

Zacznij od utworzenia klasy modelu giełdowego, która będzie używana do przechowywania i przesyłania informacji o magazynie.

1. Utwórz nowy plik klasy w folderze projektu, nadaj mu nazwę *Stock.cs*, a następnie zastąp kod szablonu następującym kodem:

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample2.cs)]

    Dwie właściwości, które zostaną ustawione podczas tworzenia zasobów, to symbol (na przykład MSFT dla firmy Microsoft) i cena. Inne właściwości zależą od tego, jak i kiedy ustawiasz cenę. Przy pierwszym ustawianiu ceny wartość jest przekazywana do DayOpen. Po wybraniu ceny wartości właściwości Change i PercentChange są obliczane na podstawie różnicy między cenami i DayOpen.

### <a name="create-the-stockticker-and-stocktickerhub-classes"></a>Tworzenie klas StockTicker i StockTickerHub

Użyjesz interfejsu API centrum sygnału do obsługi interakcji między serwerem a klientem. Klasa StockTickerHub, która pochodzi od klasy Hub sygnalizującej, będzie obsługiwać odbierające połączenia i wywołania metod od klientów. Należy również zachować dane giełdowe i uruchomić obiekt Timer, aby okresowo wyzwalać aktualizacje cenowe niezależnie od połączeń klientów. Nie można umieścić tych funkcji w klasie centrów, ponieważ wystąpienia centrów są przejściowe. Dla każdej operacji w centrum jest tworzone wystąpienie klasy centrum, takie jak połączenia i wywołania z klienta do serwera. Dlatego mechanizm, który przechowuje dane giełdowe, aktualizuje ceny i emituje aktualizacje cen, musi działać w oddzielnym klasie, o nazwie StockTicker.

![Emitowanie z StockTicker](tutorial-server-broadcast-with-aspnet-signalr/_static/image4.png)

Tylko jedno wystąpienie klasy StockTicker ma działać na serwerze, dlatego należy skonfigurować odwołanie z każdego wystąpienia StockTickerHub do wystąpienia singleton StockTicker. Klasa StockTicker musi być w stanie emitować do klientów, ponieważ ma dane podstawowe i wyzwala aktualizacje, ale StockTicker nie jest klasą centrum. W związku z tym Klasa StockTicker musi uzyskać odwołanie do obiektu kontekstu połączenia centrum sygnału. Następnie może użyć obiektu kontekstu połączenia sygnalizującego do emisji do klientów.

1. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt, a następnie kliknij pozycję **Dodaj nowy element**.
2. Jeśli masz program Visual Studio 2012 z [aktualizacją ASP.NET and Web Tools 2012,2](https://go.microsoft.com/fwlink/?LinkId=279941), kliknij pozycję **Sieć Web** w obszarze **Wizualizacja C#**  i wybierz szablon element **klasy centrum sygnałów** . W przeciwnym razie wybierz szablon **klasy** .
3. Nazwij nową klasę *StockTickerHub.cs*, a następnie kliknij przycisk **Dodaj**.

    ![Add StockTickerHub.cs](tutorial-server-broadcast-with-aspnet-signalr/_static/image5.png)
4. Zastąp kod szablonu poniższym kodem:

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample3.cs)]

    Klasa [Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx) służy do definiowania metod, które mogą być wywoływane przez klientów na serwerze. Definiujesz jedną metodę: `GetAllStocks()`. Gdy klient początkowo nawiązuje połączenie z serwerem, wywoła tę metodę, aby uzyskać listę wszystkich zasobów z ich bieżącymi cenami. Metoda może wykonywać synchronicznie i zwracać `IEnumerable<Stock>`, ponieważ zwraca dane z pamięci. Jeśli metoda musiała pobrać dane, wykonując czynności, które mogą być zależne, takie jak wyszukiwanie bazy danych lub wywołanie usługi sieci Web, należy określić `Task<IEnumerable<Stock>>` jako wartość zwrotną, aby włączyć asynchroniczne przetwarzanie. Aby uzyskać więcej informacji, zobacz [Podręcznik interfejsu API centrów ASP.NETer — serwer — Kiedy należy wykonać asynchronicznie](index.md).

    Atrybut HubName określa, w jaki sposób do centrum będzie odwoływać się kod JavaScript na kliencie. Domyślna nazwa na kliencie, jeśli nie jest używany ten atrybut jest notacji CamelCase wersja klasy, która w tym przypadku powinna być stockTickerHub.

    Jak zobaczysz później podczas tworzenia klasy StockTicker, pojedyncze wystąpienie tej klasy zostanie utworzone we właściwości wystąpienia statycznego. To pojedyncze wystąpienie StockTicker pozostaje w pamięci niezależnie od tego, ile klientów nawiązuje połączenie lub rozłączenie, i że to wystąpienie jest używane przez metodę GetAllStocks do zwracania bieżących informacji o zapasach.
5. Utwórz nowy plik klasy w folderze projektu, nadaj mu nazwę *StockTicker.cs*, a następnie zastąp kod szablonu następującym kodem:

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample4.cs)]

    Ponieważ wiele wątków będzie działać w tym samym wystąpieniu kodu StockTicker, Klasa StockTicker musi być threadsafe.

    ### <a name="storing-the-singleton-instance-in-a-static-field"></a>Przechowywanie pojedynczego wystąpienia w polu statycznym

    Kod inicjuje pole wystąpienia elementu statycznego \_, które wykonuje kopię zapasową właściwości wystąpienia z wystąpieniem klasy, i jest to jedyne wystąpienie klasy, którą można utworzyć, ponieważ Konstruktor jest oznaczony jako prywatny. [Inicjalizacja z opóźnieniem](https://msdn.microsoft.com/library/dd997286.aspx) jest używana dla pola wystąpienia \_, a nie ze względu na wydajność, ale aby upewnić się, że tworzenie wystąpienia jest threadsafe.

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample5.cs)]

    Za każdym razem, gdy klient nawiązuje połączenie z serwerem, nowe wystąpienie klasy StockTickerHub uruchomione w osobnym wątku Pobiera pojedyncze wystąpienie StockTicker z właściwości statycznej StockTicker. Instance, jak pokazano wcześniej w klasie StockTickerHub.

    ### <a name="storing-stock-data-in-a-concurrentdictionary"></a>Przechowywanie danych giełdowych w ConcurrentDictionary

    Konstruktor inicjuje zbieranie \_ych zapasów z niektórymi przykładowymi danymi zapasowymi, a GetAllStocks zwraca te zasoby. Jak widać wcześniej, ta kolekcja zasobów jest z kolei zwracana przez StockTickerHub. GetAllStocks, która jest metodą serwera w klasie centrów, którą klienci mogą wywoływać.

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample6.cs)]

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample7.cs)]

    Kolekcja magazynów jest definiowana jako typ [ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx) dla bezpieczeństwa wątków. Alternatywnie, można użyć obiektu [dictionary](https://msdn.microsoft.com/library/xfhwa508.aspx) i jawnie zablokować słownik po wprowadzeniu w nim zmian.

    W przypadku tej aplikacji przykładowej można przechowywać dane aplikacji w pamięci i utracić dane po usunięciu wystąpienia usługi StockTicker. W rzeczywistej aplikacji należy współpracować z magazynem danych zaplecza, takim jak baza danych.

    ### <a name="periodically-updating-stock-prices"></a>Okresowe aktualizowanie cen akcji

    Konstruktor uruchamia obiekt Timer, który okresowo wywołuje metody, które codziennie aktualizują ceny giełdowe.

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample8.cs)]

    UpdateStockPrices jest wywoływana przez czasomierz, który przekazuje wartość null w parametrze State. Przed aktualizacją cen jest wykonywana blokada \_obiektu updateStockPricesLock. Kod sprawdza, czy inny wątek już aktualizuje ceny, a następnie wywołuje TryUpdateStockPrice na każdym spisie na liście. Metoda TryUpdateStockPrice decyduje o tym, czy należy zmienić cenę giełdową, i ile jej zmienić. W przypadku zmiany ceny giełdowej BroadcastStockPrice jest wywoływana, aby emitować zmianę cen giełdowych do wszystkich połączonych klientów.

    Flaga updatingStockPrices \_jest oznaczona jako [nietrwała](https://msdn.microsoft.com/library/x13ttww7.aspx) , aby upewnić się, że dostęp do niego jest threadsafe.

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample9.cs)]

    W rzeczywistej aplikacji Metoda TryUpdateStockPrice wywoła usługę sieci Web w celu wyszukania ceny. w tym kodzie używa generatora liczb losowych, aby wprowadzać zmiany losowo.

    ### <a name="getting-the-signalr-context-so-that-the-stockticker-class-can-broadcast-to-clients"></a>Pobieranie kontekstu sygnalizującego, aby Klasa StockTicker mogła emitować się do klientów

    Ponieważ zmiany cen pochodzą z tego miejsca w obiekcie StockTicker, jest to obiekt, który musi wywołać metodę updateStockPrice na wszystkich połączonych klientach. W klasie centrum masz interfejs API do wywoływania metod klienta, ale StockTicker nie pochodzi od klasy Hub i nie ma odwołań do żadnego obiektu Hub. W związku z tym, aby emitować do podłączonych klientów, Klasa StockTicker musi uzyskać wystąpienie kontekstu sygnalizującego dla klasy StockTickerHub i używać go do wywoływania metod na klientach.

    Kod pobiera odwołanie do kontekstu sygnalizującego, gdy tworzy wystąpienie klasy pojedynczej, przekazuje odwołanie do konstruktora, a Konstruktor umieszcza go we właściwości clients.

    Istnieją dwa powody, dla których chcesz uzyskać kontekst tylko raz: pobieranie kontekstu jest kosztowną operacją i zapewnianie, że zaplanowana kolejność komunikatów wysyłanych do klientów zostanie zachowana.

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample10.cs)]

    Pobranie właściwości clients kontekstu i umieszczenie jej we właściwości StockTickerClient umożliwia pisanie kodu w celu wywołania metod klienta, które wyglądają tak samo jak w klasie centrów. Na przykład, aby emitować do wszystkich klientów, można napisać klienta. ALL. updateStockPrice (stock).

    Metoda updateStockPrice wywoływana w BroadcastStockPrice nie istnieje jeszcze. dodasz go później podczas pisania kodu, który jest uruchamiany na kliencie. Możesz odwołać się do updateStockPrice tutaj, ponieważ klienci. All są dynamicznymi, co oznacza, że wyrażenie będzie oceniane w czasie wykonywania. Gdy to wywołanie metody jest wykonywane, sygnalizujący wyśle do klienta nazwę metody i wartość parametru, a jeśli klient ma metodę o nazwie updateStockPrice, ta metoda zostanie wywołana i zostanie do niej przeniesiona wartość parametru.

    Klienci. wszystkie oznaczają wysyłanie do wszystkich klientów. Sygnalizujący oferuje inne opcje umożliwiające określenie klientów lub grup klientów, do których mają być wysyłane. Aby uzyskać więcej informacji, zobacz [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx).

### <a name="register-the-signalr-route"></a>Rejestrowanie trasy sygnalizującej

Serwer musi wiedzieć, który adres URL przechwycić i bezpośrednio do sygnalizującego. W tym celu należy dodać kod do pliku *Global. asax* .

1. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt, a następnie kliknij pozycję **Dodaj nowy element**.
2. Wybierz szablon **globalny element klasy aplikacji** , a następnie kliknij przycisk **Dodaj**.

    ![Dodaj Global. asax](tutorial-server-broadcast-with-aspnet-signalr/_static/image6.png)
3. Dodaj kod rejestracji trasy sygnału do metody startowej\_aplikacji:

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample11.cs)]

    Domyślnie podstawowy adres URL dla całego ruchu sygnalizującego to "/SignalR", a "/SignalR/Hubs" służy do pobierania dynamicznie generowanego pliku języka JavaScript, który definiuje serwery proxy dla wszystkich centrów w aplikacji. Metoda MapHubs zawiera przeciążenia umożliwiające określenie innego podstawowego adresu URL i niektórych opcji sygnalizujących w wystąpieniu klasy [HubConfiguration](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubconfiguration(v=vs.111).aspx) .
4. Dodaj instrukcję using na początku pliku:

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample12.cs)]
5. Zapisz i zamknij plik *Global. asax* i skompiluj projekt.

Konfiguracja kodu serwera została ukończona. W następnej sekcji skonfigurujesz klienta.

<a id="client"></a>

## <a name="set-up-the-client-code"></a>Konfigurowanie kodu klienta

1. Utwórz nowy plik HTML w folderze projektu i nadaj mu nazwę *StockTicker. html*.
2. Zastąp kod szablonu poniższym kodem:

    [!code-html[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample13.html)]

    KOD HTML tworzy tabelę z 5 kolumnami, wierszem nagłówka i wierszem danych z pojedynczą komórką obejmującą wszystkie 5 kolumn. W wierszu danych zostanie wyświetlona wartość "ładowanie..." i będą wyświetlane tylko chwilę po uruchomieniu aplikacji. Kod JavaScript spowoduje usunięcie tego wiersza i dodanie go do swoich wierszy z danymi podstawowymi pobranymi z serwera.

    Tagi skryptu określają plik skryptu jQuery, podstawowy plik skryptu sygnalizującego, plik skryptu dla serwerów proxy sygnalizującego i plik skryptu StockTicker, który zostanie utworzony później. Plik skryptu dla serwerów proxy sygnalizujący, który określa adres URL "/SignalR/Hubs", jest generowana dynamicznie i definiuje metody proxy dla metod w klasie centrów, w tym przypadku StockTickerHub. GetAllStocks. Jeśli wolisz, możesz wygenerować ten plik JavaScript ręcznie przy użyciu [narzędzi sygnalizujących](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) i wyłączyć dynamiczne tworzenie plików w wywołaniu metody MapHubs.
3. > [!IMPORTANT]
   > Upewnij się, że odwołania do pliku JavaScript w *StockTicker. html* są poprawne. Oznacza to, że wersja jQuery w tagu skryptu (1.8.2 w przykładzie) jest taka sama jak wersja jQuery w folderze *skryptów* projektu i upewnij się, że wersja sygnalizująca w tagu skryptu jest taka sama jak wersja sygnalizująca w folderze *scripts* projektu. W razie potrzeby zmień nazwy plików w tagach skryptów.
4. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy pozycję *StockTicker. html*, a następnie kliknij pozycję **Ustaw jako stronę początkową**.
5. Utwórz nowy plik JavaScript w folderze projektu i nadaj mu nazwę *StockTicker. js*..
6. Zastąp kod szablonu poniższym kodem:

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample14.js)]

    $. Connection odwołuje się do serwerów proxy sygnalizujących. Kod pobiera odwołanie do serwera proxy dla klasy StockTickerHub i umieszcza je w zmiennej ticker. Nazwa serwera proxy jest nazwą ustawioną przez atrybut [HubName]:

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample15.js)]

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample16.cs)]

    Po zdefiniowaniu wszystkich zmiennych i funkcji, ostatni wiersz kodu w pliku inicjuje połączenie sygnalizujące przez wywołanie funkcji uruchomienia sygnału. Funkcja Start wykonuje asynchroniczne i zwraca [odroczony obiekt jQuery](http://api.jquery.com/category/deferred-object/), co oznacza, że można wywołać funkcję gotowe, aby określić funkcję do wywołania po zakończeniu operacji asynchronicznej.

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample17.js)]

    Funkcja init wywołuje funkcję getAllStocks na serwerze i używa informacji zwracanych przez serwer w celu zaktualizowania tabeli giełdowej. Należy zauważyć, że domyślnie trzeba używać notacji CamelCase wielkości liter na kliencie, chociaż nazwa metody jest w języku Pascal-wielkość liter na serwerze. Reguła "notacji CamelCase" dotyczy tylko metod, a nie obiektów. Na przykład odwołujesz się do zasobów. Symbol i magazyn. Cena, niemagazynowa. symbol lub cena giełdowa.

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample18.js)]

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample19.cs)]

    Jeśli chcesz użyć obudowy Pascala na kliencie lub jeśli chcesz użyć zupełnie innej nazwy metody, możesz dekorować metodę Hub z atrybutem HubMethodName w taki sam sposób jak w przypadku klasy Hub z atrybutem HubName.

    W metodzie init kod HTML dla wiersza tabeli jest tworzony dla każdego obiektu giełdowego otrzymanego z serwera przez wywołanie formatStock do formatowania właściwości obiektu giełdowego, a następnie wywołanie supplant (który jest zdefiniowany w górnej części *StockTicker. js*), aby zastąpić symbole zastępcze w zmiennej rowTemplate wartością właściwości obiektu giełdowego. Otrzymany kod HTML jest następnie dołączany do tabeli giełdowej.

    Wywoływanie init przez przekazanie go jako funkcji wywołania zwrotnego, która jest wykonywana po zakończeniu asynchronicznej funkcji uruchomienia. Jeśli wywołano init jako oddzielną instrukcję JavaScript po wywołaniu metody Start, funkcja nie powiedzie się, ponieważ zostanie wykonana natychmiast bez oczekiwania na zakończenie tworzenia połączenia przez funkcję startową. W takim przypadku funkcja init spróbuje wywołać funkcję getAllStocks przed nawiązaniem połączenia z serwerem.

    Gdy serwer zmieni cenę giełdową, wywoła updateStockPrice na podłączonych klientach. Funkcja jest dodawana do właściwości client serwera proxy stockTicker, aby była dostępna dla wywołań z serwera.

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample20.js)]

    Funkcja updateStockPrice formatuje obiekt giełdowy otrzymany z serwera w wierszu tabeli w taki sam sposób jak w funkcji init. Jednak zamiast dołączania wiersza do tabeli Znajdź bieżący wiersz zapasu w tabeli i zastępuje ten wiersz nowym.

<a id="test"></a>

## <a name="test-the-application"></a>Testowanie aplikacji

1. Naciśnij klawisz F5, aby uruchomić aplikację w trybie debugowania.

    W tabeli giełdowej początkowo jest wyświetlana wartość "ładowanie..." wiersz po krótkim opóźnieniu wyświetlane są początkowe dane podstawowe, a następnie ceny giełdowe zaczynają ulec zmianie.

    ![Ładowaniu](tutorial-server-broadcast-with-aspnet-signalr/_static/image7.png)

    ![Początkowa tabela giełdowa](tutorial-server-broadcast-with-aspnet-signalr/_static/image8.png)

    ![Tabela giełdowa otrzymuje zmiany z serwera](tutorial-server-broadcast-with-aspnet-signalr/_static/image9.png)
2. Skopiuj adres URL z paska adresu przeglądarki i wklej go do co najmniej jednego nowego okna przeglądarki.

    Początkowy wyświetlacz giełdowy jest taki sam jak pierwsza przeglądarka i zmiany są wykonywane jednocześnie.
3. Zamknij wszystkie przeglądarki i Otwórz nową przeglądarkę, a następnie przejdź do tego samego adresu URL.

    Obiekt StockTicker singleton był nadal uruchamiany na serwerze, dlatego w tabeli giełdowej wyświetlane są wszystkie zasoby, które nadal zmieniają się. (Nie widzisz tabeli początkowej ze zmianami, które mają wartość zero).
4. Zamknij okno przeglądarki.

<a id="enablelogging"></a>

## <a name="enable-logging"></a>Włączanie rejestrowania

Sygnalizujący ma wbudowaną funkcję rejestrowania, którą można włączyć na kliencie, aby pomóc w rozwiązywaniu problemów. W tej części należy włączyć rejestrowanie i zobaczyć przykłady pokazujące, w jaki sposób dzienniki poinformują o następujących metodach transportu:

- Obiekty [WebSockets](http://en.wikipedia.org/wiki/WebSocket)obsługiwane przez usługi IIS 8 i bieżące przeglądarki.
- [Zdarzenia wysyłane przez serwer](http://en.wikipedia.org/wiki/Server-sent_events)obsługiwane przez przeglądarki inne niż Internet Explorer.
- [Ramka bez ograniczeń](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe)obsługiwana przez program Internet Explorer.
- [Długotrwałe sondowanie AJAX](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling)obsługiwane przez wszystkie przeglądarki.

Dla dowolnego połączenia sygnalizujący wybiera najlepszą metodę transportu, którą obsługuje serwer i klient.

1. Otwórz *StockTicker. js* i Dodaj wiersz kodu, aby włączyć rejestrowanie bezpośrednio przed kodem, który inicjuje połączenie na końcu pliku:

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample21.js)]
2. Naciśnij klawisz F5, aby uruchomić projekt.
3. Otwórz okno narzędzia deweloperskie w przeglądarce i wybierz konsolę, aby wyświetlić dzienniki. Może być konieczne odświeżenie strony, aby wyświetlić dzienniki sygnalizujące transportowanie metody transportu dla nowego połączenia.

    Jeśli używasz programu Internet Explorer 10 w systemie Windows 8 (IIS 8), Metoda transportowa to WebSockets.

    ![Konsola programu IE 10 IIS 8](tutorial-server-broadcast-with-aspnet-signalr/_static/image10.png)

    Jeśli używasz programu Internet Explorer 10 w systemie Windows 7 (IIS 7,5), Metoda transportu jest iframe.

    ![IE 10 Console, IIS 7.5](tutorial-server-broadcast-with-aspnet-signalr/_static/image11.png)

    W programie Firefox zainstaluj dodatek Firebug, aby uzyskać okno konsoli. W przypadku korzystania z programu Firefox 19 w systemie Windows 8 (IIS 8) Metoda transportu jest funkcją WebSockets.

    ![Firefox 19 IIS 8 obiektów WebSockets](tutorial-server-broadcast-with-aspnet-signalr/_static/image12.png)

    W przypadku korzystania z programu Firefox 19 w systemie Windows 7 (IIS 7,5) Metoda transportu to zdarzenia wysłane przez serwer.

    ![Konsola programu Firefox 19 usług IIS 7,5](tutorial-server-broadcast-with-aspnet-signalr/_static/image13.png)

<a id="fullsample"></a>

## <a name="install-and-review-the-full-stockticker-sample"></a>Instalowanie i przeglądanie pełnego przykładu StockTicker

Aplikacja StockTicker zainstalowana przez [program Microsoft. ASPNET. Signal. przykładowy](http://nuget.org/packages/microsoft.aspnet.signalr.sample) pakiet NuGet zawiera więcej funkcji niż wersja uproszczona, która właśnie została utworzona od podstaw. W tej części samouczka zainstalujesz pakiet NuGet i zapoznajesz się z nowymi funkcjami oraz kodem, który je implementuje.

### <a name="install-the-signalrsample-nuget-package"></a>Zainstaluj program sygnalizujący. przykładowy pakiet NuGet

1. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt, a następnie kliknij pozycję **Zarządzaj pakietami NuGet**.
2. W oknie dialogowym **Zarządzanie pakietami NuGet** kliknij pozycję **online**, wprowadź *próbnik. przykładowo* w polu **Wyszukaj online** , a następnie kliknij pozycję **Zainstaluj** w pakiecie **sygnalizującym. przykładowy** pakiet.

    ![Zainstaluj program sygnalizujący. przykładowy pakiet](tutorial-server-broadcast-with-aspnet-signalr/_static/image14.png)
3. W pliku *Global. asax* Dodaj komentarz do metody Route. routes. MapHubs (); wiersz, który został dodany wcześniej w aplikacji\_Metoda Start.

    Kod w *Global. asax* nie jest już wymagany, ponieważ sygnalizujący. przykładowy pakiet rejestruje trasę sygnalizującą w *aplikacji\_Start/RegisterHubs. cs* pliku:

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample22.cs)]

    Klasa webaktywatora, do której odwołuje się atrybut zestawu, jest zawarta w pakiecie NuGet WebActivatorEx, który jest instalowany jako zależność dla sygnalizującego. przykładowego pakietu.
4. W **Eksplorator rozwiązań**rozwiń węzeł *sygnalizujący. sample* , który został utworzony przez zainstalowanie próbnika. przykładowy pakiet.
5. W folderze *sygnalizującym. przykład* kliknij prawym przyciskiem myszy *plik StockTicker. html*, a następnie kliknij pozycję **Ustaw jako stronę początkową**.

    > [!NOTE]
    > Instalowanie programu sygnalizującego. przykładowy pakiet NuGet może zmienić wersję platformy jQuery, która znajduje się w folderze *skryptów* . Nowy plik *StockTicker. html* instalowany przez pakiet w *sygnale. przykładowy* folder będzie synchronizowany z wersją jQuery, którą instaluje pakiet, ale jeśli chcesz ponownie uruchomić oryginalny plik *StockTicker. html* , być może trzeba będzie najpierw zaktualizować odwołanie do jQuery w tagu skryptu.

### <a name="run-the-application"></a>Uruchamianie aplikacji

1. Naciśnij klawisz F5, aby uruchomić aplikację.

    Oprócz siatki, która została wcześniej wyświetlona, cała aplikacja taktowania w poziomie obejmuje przewijane okno, które wyświetla te same dane giełdowe. Po uruchomieniu aplikacji po raz pierwszy "rynek" jest "zamknięty" i widoczna jest siatka statyczna i okno taktowania, które nie jest przewijane.

    ![StockTicker ekranu startowego](tutorial-server-broadcast-with-aspnet-signalr/_static/image15.png)

    Po kliknięciu przycisku **Otwórz rynek**zostanie wyświetlone okno wydarzenie na **żywo** , które będzie przewinąć w poziomie, a serwer rozpocznie okresowe emitowanie zmian cen giełdowych. Za każdym razem, gdy cena giełdowa ulegnie zmianie, zostanie zaktualizowana siatka siatki **spisu** i **dynamiczny magazyn** . Gdy zmiana ceny giełdy jest dodatnia, jest ona pokazywana z zielonym tłem, a gdy zmiana jest ujemna, zapasy są wyświetlane z czerwonym tłem.

    ![Aplikacja StockTicker, otwarta na rynku](tutorial-server-broadcast-with-aspnet-signalr/_static/image16.png)

    Przycisk **Zamknij rynek** powoduje zatrzymanie zmian i zatrzymanie przewijania znacznika, a przycisk **Resetuj** resetuje wszystkie dane giełdowe do stanu początkowego przed rozpoczęciem zmian cen. Jeśli otworzysz więcej okien przeglądarki i przejdziesz do tego samego adresu URL, zobaczysz te same dane dynamicznie aktualizowane w każdej przeglądarce. Po kliknięciu jednego z przycisków wszystkie przeglądarki reagują w ten sam sposób w tym samym czasie.

### <a name="live-stock-ticker-display"></a>Wyświetlanie grafu giełdowego na żywo

Wyświetlacz **giełdowy na żywo** jest nieuporządkowaną listą w elemencie DIV, który jest sformatowany w jednym wierszu według stylów CSS. Znacznik jest inicjowany i aktualizowany w taki sam sposób, jak w tabeli: przez zastąpienie symboli zastępczych w ciągu szablonu &lt;li&gt; i dynamiczne dodanie elementów &lt;li&gt; do &lt;go elementu ul&gt;. Przewijanie jest wykonywane przy użyciu funkcji "Animuj animacje" w celu zaróżnienia lewego marginesu listy nieuporządkowanej w bloku DIV.

KOD HTML znacznika giełdowego:

[!code-html[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample23.html)]

KOD CSS znacznika giełdowego:

[!code-html[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample24.html)]

Kod jQuery, który umożliwia przewinięcie:

[!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample25.js)]

### <a name="additional-methods-on-the-server-that-the-client-can-call"></a>Dodatkowe metody na serwerze, z którym klient może wywoływać

Klasa StockTickerHub definiuje cztery dodatkowe metody, które klient może wywołać:

[!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample26.cs)]

OpenMarket, CloseMarket i Reset są wywoływane w odpowiedzi na przyciski w górnej części strony. Pokazują one wzorzec jednego klienta wyzwalającego zmianę stanu, który jest natychmiast propagowany do wszystkich klientów. Każda z tych metod wywołuje metodę w klasie StockTicker, która wpływa na zmianę stanu rynku, a następnie emituje nowy stan.

W klasie StockTicker stan rynku jest obsługiwany przez właściwość MarketState, która zwraca wartość wyliczenia MarketState:

[!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample27.cs)]

Każda z metod, które zmieniają stan rynku, to w bloku blokady, ponieważ Klasa StockTicker musi być threadsafe:

[!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample28.cs)]

Aby upewnić się, że ten kod jest threadsafe, pole \_marketState, które wykonuje kopię zapasową właściwości MarketState, jest oznaczone jako nietrwałe.

[!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample29.cs)]

Metody BroadcastMarketStateChange i BroadcastMarketReset są podobne do metody BroadcastStockPrice, która została już wykorzystana, z tą różnicą, że wywołują różne metody zdefiniowane na kliencie:

[!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample30.cs)]

### <a name="additional-functions-on-the-client-that-the-server-can-call"></a>Dodatkowe funkcje na kliencie, które może wywoływać serwer

Funkcja updateStockPrice obsługuje teraz zarówno siatkę, jak i wyświetlanie znaczników, a następnie używa jQuery. Color do Flash Red i Green Color.

Nowe funkcje w programie *sygnalizującer. StockTicker. js* włącza i wyłącza przyciski na podstawie stanu na rynku, a także zatrzymuje lub uruchamiają przewijanie w poziomie okna znaczników. Ponieważ wiele funkcji jest dodawanych do programu ticker. Client, [Funkcja rozszerzająca jQuery](http://api.jquery.com/jQuery.extend/) służy do ich dodawania.

[!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample31.js)]

### <a name="additional-client-setup-after-establishing-the-connection"></a>Dodatkowa konfiguracja klienta po ustanowieniu połączenia

Po nawiązaniu połączenia klient ma kilka dodatkowych czynności do wykonania: Sprawdź, czy rynek jest otwarty lub zamknięty w celu wywołania odpowiedniej funkcji marketOpened lub marketClosed, i Dołącz wywołania metody serwera do przycisków.

[!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample32.js)]

Metody serwera nie są połączone z przyciskami aż po nawiązaniu połączenia, dzięki czemu kod nie może próbować wywołać metody serwera przed udostępnieniem.

<a id="nextsteps"></a>

## <a name="next-steps"></a>Następne kroki

W tym samouczku przedstawiono sposób programowania aplikacji sygnalizującej, która emituje komunikaty z serwera do wszystkich połączonych klientów, zarówno okresowo, jak i w odpowiedzi na powiadomienia z dowolnego klienta. Wzorzec korzystania z wielowątkowego wystąpienia pojedynczego do obsługi stanu serwera może również być używany w scenariuszach gier online obejmujących wiele graczy. Aby zapoznać się z przykładem, zapoznaj się [z grą korzystającą z usługi sygnalizującej](https://github.com/NTaylorMullen/ShootR).

Aby zapoznać się z samouczkami pokazującymi scenariusze komunikacji równorzędnej, zobacz [wprowadzenie z sygnalizacją](index.md) i [aktualizacją w czasie rzeczywistym za pomocą usługi sygnalizującego](index.md).

Aby dowiedzieć się więcej na temat zaawansowanych koncepcji związanych z programowaniem sygnałów, odwiedź następujące witryny dotyczące kodu źródłowego i zasobów usługi sygnalizującego:

- [ASP.NET sygnalizujący](https://asp.net/signalr/)
- [Projekt sygnalizującego](http://signalr.net/)
- [Usługi GitHub i przykłady dla sygnałów](https://github.com/SignalR/SignalR)
- [Strona typu wiki sygnalizująca](https://github.com/SignalR/SignalR/wiki)

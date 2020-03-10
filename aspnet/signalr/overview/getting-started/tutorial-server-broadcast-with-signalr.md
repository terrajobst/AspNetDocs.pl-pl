---
uid: signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
title: 'Samouczek: emisja serwera z sygnałem 2 | Microsoft Docs'
author: tdykstra
description: W tym samouczku pokazano, jak utworzyć aplikację sieci Web korzystającą z ASP.NET sygnalizującego 2, aby zapewnić funkcję emisji serwera.
ms.author: bradyg
ms.date: 01/02/2019
ms.topic: tutorial
ms.assetid: 1568247f-60b5-4eca-96e0-e661fbb2b273
msc.legacyurl: /signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 14924109fff8db3e537e6bc08b6dc868792ee660
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536604"
---
# <a name="tutorial-server-broadcast-with-signalr-2"></a>Samouczek: emisja serwera z sygnałem 2

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

W tym samouczku pokazano, jak utworzyć aplikację sieci Web korzystającą z ASP.NET sygnalizującego 2, aby zapewnić funkcję emisji serwera. Emisja serwera oznacza, że serwer uruchamia komunikację wysyłaną do klientów.

Aplikacja, którą utworzysz w tym samouczku, symuluje taktowanie giełdowe, typowy scenariusz dla funkcji emisji serwera. Okresowo serwer losowo aktualizuje ceny akcji i emituje aktualizacje do wszystkich podłączonych klientów. W przeglądarce numery i symbole w kolumnach **Zmień** i **%** zmieniają się dynamicznie w odpowiedzi na powiadomienia z serwera. Jeśli otworzysz dodatkowe przeglądarki pod tym samym adresem URL, wszystkie będą wyświetlać te same dane i te same zmiany w danych jednocześnie.

![Tworzenie sieci Web](tutorial-server-broadcast-with-signalr/_static/image1.png)

W tym samouczku zostaną wykonane następujące czynności:

> [!div class="checklist"]
> * Tworzenie projektu
> * Konfigurowanie kodu serwera
> * Sprawdzanie kodu serwera
> * Konfigurowanie kodu klienta
> * Sprawdzanie kodu klienta
> * Testowanie aplikacji
> * Włączanie rejestrowania

> [!IMPORTANT]
> Jeśli nie chcesz wykonać kroków tworzenia aplikacji, możesz zainstalować program sygnalizujący. przykładowego pakietu w nowym pustym projekcie aplikacji sieci Web ASP.NET. W przypadku zainstalowania pakietu NuGet bez wykonywania czynności opisanych w tym samouczku należy wykonać instrukcje zawarte w pliku *README. txt* . Aby uruchomić pakiet, należy dodać klasę uruchomieniową OWIN, która wywołuje metodę `ConfigureSignalR` w zainstalowanym pakiecie. Jeśli nie dodasz klasy uruchomieniowej OWIN, zostanie wyświetlony komunikat o błędzie. Zobacz sekcję [Instalowanie przykładu StockTicker](#install-the-stockticker-sample) w tym artykule.

## <a name="prerequisites"></a>Wymagania wstępne

* Program [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) z pakietem roboczym **Tworzenie aplikacji na platformie ASP.NET i aplikacji internetowych**.

## <a name="create-the-project"></a>Tworzenie projektu

W tej sekcji pokazano, jak utworzyć pustą aplikację sieci Web ASP.NET za pomocą programu Visual Studio 2017.

1. W programie Visual Studio Utwórz aplikację sieci Web ASP.NET.

    ![Tworzenie sieci Web](tutorial-server-broadcast-with-signalr/_static/image2.png)

1. W oknie **nowy ASP.NET Web Application-sygnalizującer. StockTicker** pozostaw **puste** zaznaczone i wybierz **przycisk OK**.

## <a name="set-up-the-server-code"></a>Konfigurowanie kodu serwera

W tej sekcji skonfigurujesz kod, który jest uruchamiany na serwerze.

### <a name="create-the-stock-class"></a>Tworzenie klasy giełdowej

Zacznij od utworzenia klasy modelu *giełdowego* , która będzie używana do przechowywania i przesyłania informacji o magazynie.

1. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt i wybierz polecenie Dodaj **klasę** > .

1. Nadaj *klasie nazwę* i Dodaj ją do projektu.

1. Zastąp kod w pliku *Stock.cs* tym kodem:

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample1.cs)]

    Dwie właściwości, które zostaną ustawione podczas tworzenia zasobów, są `Symbol` (na przykład MSFT dla firmy Microsoft) i `Price`. Inne właściwości zależą od tego, jak i kiedy ustawiasz `Price`. Przy pierwszym ustawianiu `Price`wartość jest przekazywana do `DayOpen`. Po ustawieniu `Price`aplikacja oblicza wartości właściwości `Change` i `PercentChange` na podstawie różnicy między `Price` i `DayOpen`.

### <a name="create-the-stocktickerhub-and-stockticker-classes"></a>Tworzenie klas StockTickerHub i StockTicker

Użyjesz interfejsu API centrum sygnału do obsługi interakcji między serwerem a klientem. Klasa `StockTickerHub`, która pochodzi od klasy sygnalizującej `Hub`, będzie obsługiwać odbierające połączenia i wywołania metod od klientów. Należy również zachować dane giełdowe i uruchomić `Timer` obiektu. Obiekt `Timer` będzie okresowo wyzwalać aktualizacje cen niezależnie od połączeń klientów. Nie można umieścić tych funkcji w klasie `Hub`, ponieważ centra są przejściowe. Aplikacja tworzy wystąpienie klasy `Hub` dla każdego zadania w centrum, takie jak połączenia i wywołania z klienta do serwera. Dlatego mechanizm, który przechowuje dane giełdowe, aktualizuje ceny i emituje aktualizacje cen, musi działać w oddzielnym klasie. Nazwa klasy `StockTicker`.

![Emitowanie z StockTicker](tutorial-server-broadcast-with-signalr/_static/image3.png)

Tylko jedno wystąpienie klasy `StockTicker` może być uruchomione na serwerze, dlatego należy skonfigurować odwołanie z każdego wystąpienia `StockTickerHub` do wystąpienia pojedynczego `StockTicker`. Klasa `StockTicker` musi emitować do klientów, ponieważ ma dane podstawowe i wyzwala aktualizacje, ale `StockTicker` nie jest klasą `Hub`. Klasa `StockTicker` musi uzyskać odwołanie do obiektu kontekstu połączenia centrum sygnału. Następnie może użyć obiektu kontekstu połączenia sygnalizującego do emisji do klientów.

#### <a name="create-stocktickerhubcs"></a>Create StockTickerHub.cs

1. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt, a następnie wybierz pozycję **Dodaj** > **nowy element**.

1. W obszarze **Dodaj nowy element — sygnalizującer. StockTicker**wybierz **pozycję zainstalowane** > **Visual C#**  > **sieci Web** > **sygnalizujący** , a następnie wybierz pozycję **Klasa centrum sygnałów (v2)** .

1. Nadaj klasie nazwę *StockTickerHub* i Dodaj ją do projektu.

    Ten krok powoduje utworzenie pliku klasy *StockTickerHub.cs* . Jednocześnie dodaje zestaw plików skryptów i odwołań do zestawów, które obsługują program sygnalizujący do projektu.

1. Zastąp kod w pliku *StockTickerHub.cs* tym kodem:

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample2.cs)]

1. Zapisz plik.

Aplikacja używa klasy [Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx) do definiowania metod, które klienci mogą wywoływać na serwerze. Definiujesz jedną metodę: `GetAllStocks()`. Gdy klient początkowo nawiązuje połączenie z serwerem, wywoła tę metodę, aby uzyskać listę wszystkich zasobów z ich bieżącymi cenami. Metoda może być uruchomiona synchronicznie i zwracać `IEnumerable<Stock>`, ponieważ zwraca dane z pamięci.

Jeśli metoda musiała pobrać dane, wykonując coś, co będzie wymagało oczekiwania, takiego jak wyszukiwanie bazy danych lub wywołanie usługi sieci Web, należy określić `Task<IEnumerable<Stock>>` jako wartość zwrotną, aby włączyć asynchroniczne przetwarzanie. Aby uzyskać więcej informacji, zobacz [Podręcznik interfejsu API centrów ASP.NETer — serwer — Kiedy należy wykonać asynchronicznie](../guide-to-the-api/hubs-api-guide-server.md#asyncmethods).

Atrybut `HubName` określa, w jaki sposób aplikacja będzie odwoływać się do centrum w kodzie JavaScript na kliencie. Nazwa domyślna na kliencie, jeśli ten atrybut nie jest używany, to camelCase wersja klasy, która w tym przypadku byłaby `stockTickerHub`.

Jak zobaczysz później podczas tworzenia klasy `StockTicker`, aplikacja utworzy pojedyncze wystąpienie tej klasy w swojej statycznej `Instance` właściwości. To pojedyncze wystąpienie `StockTicker` znajduje się w pamięci, niezależnie od tego, ile klientów nawiązuje połączenie lub rozłączanie. To wystąpienie jest używane przez metodę `GetAllStocks()` do zwracania bieżących informacji o zapasach.

#### <a name="create-stocktickercs"></a>Utwórz StockTicker.cs

1. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt i wybierz polecenie Dodaj **klasę** > .

1. Nadaj klasie nazwę *StockTicker* i Dodaj ją do projektu.

1. Zastąp kod w pliku *StockTicker.cs* tym kodem:

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample3.cs)]

Ponieważ wszystkie wątki będą działać w tym samym wystąpieniu kodu StockTicker, Klasa StockTicker musi być bezpieczna wątkowo.

### <a name="examine-the-server-code"></a>Sprawdzanie kodu serwera

Jeśli sprawdzisz kod serwera, pomoże Ci zrozumieć, jak działa aplikacja.

#### <a name="storing-the-singleton-instance-in-a-static-field"></a>Przechowywanie pojedynczego wystąpienia w polu statycznym

Kod inicjuje statyczne `_instance` pole, które wykonuje kopię zapasową właściwości `Instance` z wystąpieniem klasy. Ponieważ Konstruktor jest prywatny, jest to jedyne wystąpienie klasy, które może utworzyć aplikacja. Aplikacja używa [inicjalizacji z opóźnieniem](/dotnet/framework/performance/lazy-initialization) dla pola `_instance`. Nie jest ze względu na wydajność. Należy upewnić się, że tworzenie wystąpienia jest bezpieczne wątkowo.

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample4.cs)]

Za każdym razem, gdy klient nawiązuje połączenie z serwerem, nowe wystąpienie klasy StockTickerHub uruchomione w osobnym wątku Pobiera pojedyncze wystąpienie StockTicker z właściwości statycznej `StockTicker.Instance`, jak pokazano wcześniej w klasie `StockTickerHub`.

#### <a name="storing-stock-data-in-a-concurrentdictionary"></a>Przechowywanie danych giełdowych w ConcurrentDictionary

Konstruktor inicjuje kolekcję `_stocks` z niektórymi przykładowymi danymi zapasowymi, a `GetAllStocks` zwraca te zasoby. Jak widać wcześniej, ta kolekcja zapasów jest zwracana przez `StockTickerHub.GetAllStocks`, czyli metodę serwera w klasie `Hub`, którą klienci mogą wywoływać.

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample5.cs)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample6.cs)]

Kolekcja magazynów jest definiowana jako typ [ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx) dla bezpieczeństwa wątków. Alternatywnie, można użyć obiektu [dictionary](https://msdn.microsoft.com/library/xfhwa508.aspx) i jawnie zablokować słownik po wprowadzeniu w nim zmian.

W przypadku tej aplikacji przykładowej można przechowywać dane aplikacji w pamięci i utracić dane, gdy aplikacja zostanie oddysponowana wystąpieniem `StockTicker`. W rzeczywistej aplikacji można korzystać z magazynu danych zaplecza, takiego jak baza danych.

#### <a name="periodically-updating-stock-prices"></a>Okresowe aktualizowanie cen akcji

Konstruktor uruchamia obiekt `Timer`, który okresowo wywołuje metody, które codziennie aktualizują ceny giełdowe.

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample7.cs)]

 `Timer` wywołań `UpdateStockPrices`, które są przekazywane do wartości null w parametrze stanu. Przed aktualizacją cen aplikacja wykonuje blokadę na obiekcie `_updateStockPricesLock`. Kod sprawdza, czy inny wątek już aktualizuje ceny, a następnie wywołuje `TryUpdateStockPrice` na każdym spisie na liście. Metoda `TryUpdateStockPrice` decyduje o tym, czy należy zmienić cenę giełdową, i ile jej zmienić. W przypadku zmiany ceny giełdowej aplikacja wywoła `BroadcastStockPrice`, aby emitować zmianę cen giełdowych do wszystkich połączonych klientów.

Flaga `_updatingStockPrices` oznaczona jako [nietrwała](https://msdn.microsoft.com/library/x13ttww7.aspx) , aby upewnić się, że jest bezpieczna wątkowo.

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample8.cs)]

W rzeczywistej aplikacji Metoda `TryUpdateStockPrice` wywoła usługę sieci Web w celu wyszukania ceny. W tym kodzie aplikacja używa generatora liczb losowych, aby wprowadzać zmiany losowo.

#### <a name="getting-the-signalr-context-so-that-the-stockticker-class-can-broadcast-to-clients"></a>Pobieranie kontekstu sygnalizującego, aby Klasa StockTicker mogła emitować się do klientów

Ponieważ zmiany cen pochodzą z tego miejsca w obiekcie `StockTicker`, jest to obiekt, który musi wywołać metodę `updateStockPrice` na wszystkich połączonych klientach. W klasie `Hub` masz interfejs API do wywoływania metod klienta, ale `StockTicker` nie pochodzi od klasy `Hub` i nie zawiera odwołania do żadnego obiektu `Hub`. Aby emitować do podłączonych klientów, Klasa `StockTicker` musi uzyskać wystąpienie kontekstu sygnalizującego dla klasy `StockTickerHub` i używać go do wywoływania metod na klientach.

Kod pobiera odwołanie do kontekstu sygnalizującego, gdy tworzy wystąpienie klasy pojedynczej, przekazuje odwołanie do konstruktora, a Konstruktor umieszcza go we właściwości `Clients`.

Istnieją dwa powody, dla których chcesz uzyskać kontekst tylko raz: uzyskanie kontekstu jest kosztownym zadaniem i wprowadzenie go, aby zachować zaplanowaną kolejność komunikatów wysyłanych do klientów.

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample9.cs)]

Pobranie właściwości `Clients` kontekstu i umieszczenie jej we właściwości `StockTickerClient` umożliwia pisanie kodu w celu wywołania metod klienta, które wyglądają tak samo jak w klasie `Hub`. Na przykład do emisji do wszystkich klientów można pisać `Clients.All.updateStockPrice(stock)`.

Metoda `updateStockPrice` wywoływana w `BroadcastStockPrice` nie istnieje jeszcze. Dodasz go później podczas pisania kodu, który jest uruchamiany na kliencie. Możesz odwołać się do `updateStockPrice` tutaj, ponieważ `Clients.All` jest dynamiczna, co oznacza, że aplikacja będzie szacować wyrażenie w czasie wykonywania. Gdy to wywołanie metody jest wykonywane, sygnalizujący wyśle do klienta nazwę metody i wartość parametru, a jeśli klient ma metodę o nazwie `updateStockPrice`, aplikacja wywoła tę metodę i przekaże do niej wartość parametru.

`Clients.All` oznacza wysyłanie do wszystkich klientów. Sygnalizujący oferuje inne opcje umożliwiające określenie klientów lub grup klientów, do których mają być wysyłane. Aby uzyskać więcej informacji, zobacz [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx).

### <a name="register-the-signalr-route"></a>Rejestrowanie trasy sygnalizującej

Serwer musi wiedzieć, który adres URL przechwycić i bezpośrednio do sygnalizującego. Aby to zrobić, Dodaj klasę uruchomieniową OWIN:

1. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt, a następnie wybierz pozycję **Dodaj** > **nowy element**.

1. W obszarze **Dodaj nowy element — sygnalizujący. StockTicker** wybierz pozycję **zainstalowane** > **Visual C#**  > **Web** , a następnie wybierz pozycję **Owin klasy startowej**.

1. Nazwij klasę *uruchamiania* i wybierz **przycisk OK**.

1. Zastąp domyślny kod w pliku *Startup.cs* tym kodem:

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample10.cs)]

Konfiguracja kodu serwera została zakończona. W następnej sekcji zostanie skonfigurowany klient.

## <a name="set-up-the-client-code"></a>Konfigurowanie kodu klienta

W tej sekcji skonfigurujesz kod, który jest uruchamiany na kliencie programu.

### <a name="create-the-html-page-and-javascript-file"></a>Tworzenie strony HTML i pliku JavaScript

Na stronie HTML zostaną wyświetlone dane, a plik JavaScript będzie organizować dane.

#### <a name="create-stocktickerhtml"></a>Create StockTicker.html

Najpierw należy dodać klienta HTML.

1. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt i wybierz polecenie **Dodaj** > **stronę HTML**.

1. Nazwij plik *StockTicker* i wybierz **przycisk OK**.

1. Zastąp domyślny kod w pliku *StockTicker. html* tym kodem:

    [!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample11.html?highlight=40-43)]

    KOD HTML tworzy tabelę z pięcioma kolumnami, wierszem nagłówka i wierszem danych z pojedynczą komórką obejmującą wszystkie pięć kolumn. W wierszu danych zostanie wyświetlona wartość "ładowanie..." chwilę po uruchomieniu aplikacji. Kod JavaScript spowoduje usunięcie tego wiersza i dodanie go do swoich wierszy z danymi podstawowymi pobranymi z serwera.

    Tagi skryptu określają:

    * Plik skryptu jQuery.

    * Podstawowy plik skryptu sygnalizującego.

    * Plik skryptu dla serwerów proxy sygnałów.

    * Plik skryptu StockTicker, który utworzysz później.

    Aplikacja dynamicznie generuje plik skryptu dla serwerów proxy sygnalizującego. Określa adres URL "/SignalR/Hubs" i definiuje metody serwera proxy dla metod klasy Hub, w tym przypadku `StockTickerHub.GetAllStocks`. Jeśli wolisz, możesz wygenerować ten plik JavaScript ręcznie przy użyciu [narzędzi sygnalizujących](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/). Nie zapomnij wyłączyć tworzenia pliku dynamicznego w wywołaniu metody `MapHubs`.

1. W **Eksplorator rozwiązań**rozwiń węzeł **skrypty**.

    Biblioteki skryptów dla jQuery i sygnalizujących są widoczne w projekcie.

    > [!IMPORTANT]
    > Menedżer pakietów zainstaluje nowszą wersję skryptów sygnalizujących.

1. Zaktualizuj odwołania do skryptu w bloku kodu, aby odpowiadały wersji plików skryptów w projekcie.

1. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy pozycję *StockTicker. html*, a następnie wybierz pozycję **Ustaw jako stronę początkową**.

#### <a name="create-stocktickerjs"></a>Create StockTicker.js

Teraz Utwórz plik JavaScript.

1. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt i wybierz polecenie **Dodaj** > **plik JavaScript**.

1. Nazwij plik *StockTicker* i wybierz **przycisk OK**.

1. Dodaj ten kod do pliku *StockTicker. js* :

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample12.js)]

### <a name="examine-the-client-code"></a>Sprawdzanie kodu klienta

Po sprawdzeniu kodu klienta pomoże Ci dowiedzieć się, jak kod klienta współdziała z kodem serwera, aby umożliwić działanie aplikacji.

#### <a name="starting-the-connection"></a>Uruchamianie połączenia

`$.connection` odnosi się do serwerów proxy sygnalizujących. Kod pobiera odwołanie do serwera proxy dla klasy `StockTickerHub` i umieszcza je w zmiennej `ticker`. Nazwa serwera proxy jest nazwą ustawioną przez atrybut `HubName`:

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample13.js)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample14.cs)]

Po zdefiniowaniu wszystkich zmiennych i funkcji ostatni wiersz kodu w pliku inicjuje połączenie sygnalizujące przez wywołanie funkcji `start` sygnałów. Funkcja `start` wykonuje asynchroniczne i zwraca [odroczony obiekt jQuery](http://api.jquery.com/category/deferred-object/). Można wywołać funkcję gotowe, aby określić funkcję do wywołania, gdy aplikacja zakończy akcję asynchroniczną.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample15.js)]

#### <a name="getting-all-the-stocks"></a>Pobieranie wszystkich zasobów

Funkcja `init` wywołuje funkcję `getAllStocks` na serwerze i używa informacji zwracanych przez serwer w celu zaktualizowania tabeli giełdowej. Należy zauważyć, że domyślnie należy używać camelCasing na kliencie, nawet jeśli nazwa metody jest w języku Pascal-wielkość liter na serwerze. Reguła camelCasing dotyczy tylko metod, a nie obiektów. Załóżmy na przykład, że odwołujesz się do `stock.Symbol` i `stock.Price`, a nie `stock.symbol` lub `stock.price`.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample16.js)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample17.cs)]

W metodzie `init` aplikacja tworzy kod HTML dla wiersza tabeli dla każdego obiektu giełdowego otrzymanego z serwera, wywołując `formatStock` do formatowania właściwości obiektu `stock`, a następnie wywołując `supplant`, aby zamienić symbole zastępcze w zmiennej `rowTemplate` na wartości właściwości obiektu `stock`. Otrzymany kod HTML jest następnie dołączany do tabeli giełdowej.

> [!NOTE]
> `init` przez przekazanie go jako funkcji `callback`, która jest wykonywana po zakończeniu asynchronicznej funkcji `start`. Jeśli wywołano `init` jako oddzielną instrukcję JavaScript po wywołaniu `start`, funkcja nie powiedzie się, ponieważ zostanie uruchomiona natychmiast bez oczekiwania na zakończenie tworzenia połączenia przez funkcję startową. W takim przypadku funkcja `init` spróbuje wywołać funkcję `getAllStocks`, zanim aplikacja nawiąże połączenie z serwerem.

#### <a name="getting-updated-stock-prices"></a>Pobieranie zaktualizowanych cen giełdowych

Gdy serwer zmieni cenę giełdową, wywoła `updateStockPrice` na podłączonych klientach. Aplikacja dodaje funkcję do właściwości client serwera proxy `stockTicker`, aby była dostępna dla wywołań z serwera.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample18.js)]

Funkcja `updateStockPrice` formatuje obiekt giełdowy otrzymany z serwera w wierszu tabeli w taki sam sposób jak w funkcji `init`. Zamiast dołączyć wiersz do tabeli, znajduje bieżący wiersz zapasu w tabeli i zamienia ten wiersz na nowy.

## <a name="test-the-application"></a>Testowanie aplikacji

Możesz przetestować aplikację, aby upewnić się, że działa. Zobaczysz wszystkie okna przeglądarki wyświetlają dynamiczną tabelę giełdową z wahaniami cen giełdowych.

1. Na pasku narzędzi Włącz **debugowanie skryptów** , a następnie wybierz przycisk Odtwórz, aby uruchomić aplikację w trybie debugowania.

    ![Zrzut ekranu przedstawiający Włączanie trybu debugowania przez użytkownika i wybieranie opcji Odtwórz.](tutorial-server-broadcast-with-signalr/_static/image4.png)

    Zostanie otwarte okno przeglądarki z wyświetlaniem **tabeli giełdowej na żywo**. W tabeli giełdowej początkowo jest wyświetlana wartość "ładowanie..." Po krótkim czasie aplikacja wyświetli początkowe dane giełdowe, a następnie ceny giełdowe zaczynają się zmienić.

1. Skopiuj adres URL z przeglądarki, Otwórz dwie inne przeglądarki i wklej adresy URL do pasków adresów.

    Początkowy wyświetlacz giełdowy jest taki sam jak pierwsza przeglądarka i zmiany są wykonywane jednocześnie.

1. Zamknij wszystkie przeglądarki, Otwórz nową przeglądarkę i przejdź do tego samego adresu URL.

    Obiekt StockTicker singleton nadal działa na serwerze. W **tabeli magazynu na żywo** widać, że zasoby nadal się zmieniają. Nie widzisz tabeli początkowej ze zmianami o wartości zero.

1. Zamknij okno przeglądarki.

## <a name="enable-logging"></a>Włączanie rejestrowania

Sygnalizujący ma wbudowaną funkcję rejestrowania, którą można włączyć na kliencie, aby pomóc w rozwiązywaniu problemów. W tej części należy włączyć rejestrowanie i zobaczyć przykłady pokazujące, w jaki sposób dzienniki poinformują o następujących metodach transportu:

* Obiekty [WebSockets](http://en.wikipedia.org/wiki/WebSocket)obsługiwane przez usługi IIS 8 i bieżące przeglądarki.

* [Zdarzenia wysyłane przez serwer](http://en.wikipedia.org/wiki/Server-sent_events)obsługiwane przez przeglądarki inne niż Internet Explorer.

* [Ramka bez ograniczeń](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe)obsługiwana przez program Internet Explorer.

* [Długotrwałe sondowanie AJAX](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling)obsługiwane przez wszystkie przeglądarki.

Dla dowolnego połączenia sygnalizujący wybiera najlepszą metodę transportu, którą obsługuje serwer i klient.

1. Otwórz *StockTicker. js*.

1. Dodaj ten wyróżniony wiersz kodu, aby włączyć rejestrowanie bezpośrednio przed kodem, który inicjuje połączenie na końcu pliku:

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample19.js?highlight=2)]

1. Naciśnij klawisz **F5** , aby uruchomić projekt.

1. Otwórz okno narzędzia deweloperskie w przeglądarce i wybierz konsolę, aby wyświetlić dzienniki. Może być konieczne odświeżenie strony, aby wyświetlić dzienniki sygnalizujące transportowanie metody transportu dla nowego połączenia.

    * Jeśli używasz programu Internet Explorer 10 w systemie Windows 8 (IIS 8), Metoda transportowa to **WebSockets**.

    * Jeśli używasz programu Internet Explorer 10 w systemie Windows 7 (IIS 7,5), Metoda transportu jest **iframe**.

    * Jeśli korzystasz z programu Firefox 19 w systemie Windows 8 (IIS 8), Metoda transportu jest funkcją **WebSockets**.

        > [!TIP]
        > W programie Firefox zainstaluj dodatek Firebug, aby uzyskać okno konsoli.

    * W przypadku korzystania z programu Firefox 19 w systemie Windows 7 (IIS 7,5) Metoda transportu to zdarzenia **wysłane przez serwer** .

## <a name="install-the-stockticker-sample"></a>Instalowanie przykładu StockTicker

[Microsoft. ASPNET. Signal. przykład](http://nuget.org/packages/microsoft.aspnet.signalr.sample) instaluje aplikację StockTicker. Pakiet NuGet zawiera więcej funkcji niż wersja uproszczona utworzona od podstaw. W tej części samouczka zainstalujesz pakiet NuGet i zapoznajesz się z nowymi funkcjami oraz kodem, który je implementuje.

> [!IMPORTANT]
> Jeśli pakiet zostanie zainstalowany bez wykonywania wcześniejszych kroków tego samouczka, należy dodać do projektu klasę uruchomieniową OWIN. Ten krok zawiera opis tego pliku Readme. txt dla pakietu NuGet.

### <a name="install-the-signalrsample-nuget-package"></a>Zainstaluj program sygnalizujący. przykładowy pakiet NuGet

1. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt i wybierz polecenie **Zarządzaj pakietami NuGet**.

1. W **Menedżerze pakietów NuGet: signaler. StockTicker**, wybierz pozycję **Przeglądaj**.

1. W obszarze **Źródło pakietu**wybierz pozycję **NuGet.org**.

1. Wprowadź *sygnalizowanie. przykład* w polu wyszukiwania i wybierz pozycję **Microsoft. ASPNET. signal. sample** > **Install**.

1. W **Eksplorator rozwiązań**rozwiń folder *sygnalizujący. sample* .

    Instalowanie programu sygnalizującego. przykładowy pakiet utworzył folder i jego zawartość.

1. W folderze *sygnalizującym. sample* kliknij prawym przyciskiem myszy *plik StockTicker. html*, a następnie wybierz pozycję **Ustaw jako stronę początkową**.

    > [!NOTE]
    > Instalowanie programu sygnalizującego. przykładowy pakiet NuGet może zmienić wersję platformy jQuery, która znajduje się w folderze *skryptów* . Nowy plik *StockTicker. html* instalowany przez pakiet w *sygnale. przykładowy* folder będzie synchronizowany z wersją jQuery, którą instaluje pakiet, ale jeśli chcesz ponownie uruchomić oryginalny plik *StockTicker. html* , być może trzeba będzie najpierw zaktualizować odwołanie do jQuery w tagu skryptu.

### <a name="run-the-application"></a>Uruchamianie aplikacji

 Tabela, która została zaprojektowana w pierwszej aplikacji, miała przydatne funkcje. W przypadku pełnego spisu aplikacji są widoczne nowe funkcje: okno przewijania w poziomie, które pokazuje dane giełdowe i zasoby, które zmieniają kolor w miarę wzrostu i spadku.

1. Naciśnij klawisz **F5** , aby uruchomić aplikację.

     Po uruchomieniu aplikacji po raz pierwszy "rynek" jest "zamknięty" i zobaczysz tabelę statyczną i okno znaczników, które nie jest przewijane.

1. Wybierz pozycję **Otwórz rynek**.

    ![Zrzut ekranu dynamicznego znacznika.](tutorial-server-broadcast-with-signalr/_static/image5.png)

    * Skrzynka **giełdowa na żywo** zostanie przesunięta w poziomie, a serwer rozpocznie okresowe emitowanie zmian cen giełdowych.

    * Za każdym razem, gdy zmieni się cena giełdowa, aplikacja aktualizuje zarówno **dynamiczną** , jak i giełdę na **żywo**.

    * Gdy zmiana ceny zapasów jest dodatnia, aplikacja pokazuje magazyn z zielonym tłem.

    * Gdy zmiana jest ujemna, aplikacja pokazuje zapas z czerwonym tłem.

1. Wybierz pozycję **Zamknij rynek**.

    * Aktualizacja tabeli zatrzymuje się.

    * Znacznik przestaje przewijać.

1. Wybierz pozycję **Zresetuj**.

    * Wszystkie dane giełdowe są resetowane.

    * Aplikacja przywraca stan początkowy przed rozpoczęciem zmian cen.

1. Skopiuj adres URL z przeglądarki, Otwórz dwie inne przeglądarki i wklej adresy URL do pasków adresów.

1. Te same dane są automatycznie aktualizowane w każdej przeglądarce.

1. Po wybraniu dowolnej z tych kontrolek wszystkie przeglądarki reagują w ten sam sposób w tym samym czasie.

### <a name="live-stock-ticker-display"></a>Wyświetlanie grafu giełdowego na żywo

Wyświetlacz **giełdowy na żywo** jest nieuporządkowaną listą w elemencie `<div>` sformatowanym w jednym wierszu według stylów CSS. Aplikacja inicjuje i aktualizuje znaczniki w taki sam sposób jak tabela: przez zastąpienie symboli zastępczych w ciągu szablonu `<li>` i dynamiczne dodanie elementów `<li>` do elementu `<ul>`. Aplikacja obejmuje przewijanie przy użyciu funkcji jQuery `animate` w celu zaróżnienia lewego marginesu listy nieuporządkowanej w `<div>`.

#### <a name="signalrsample-stocktickerhtml"></a>Sygnalizujący. przykład StockTicker. html

Kod HTML znacznika giełdowego:

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample20.html)]

#### <a name="signalrsample-stocktickercss"></a>Sygnalizujący. przykład StockTicker. CSS

Kod CSS znacznika giełdowego:

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample21.html)]

#### <a name="signalrsample-signalrstocktickerjs"></a>Sygnalizujący. przykładowy sygnał. StockTicker. js

Kod jQuery, który umożliwia przewinięcie:

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample22.js)]

### <a name="additional-methods-on-the-server-that-the-client-can-call"></a>Dodatkowe metody na serwerze, z którym klient może wywoływać

Aby dodać elastyczność do aplikacji, istnieją dodatkowe metody, które może wywoływać aplikacja.

#### <a name="signalrsample-stocktickerhubcs"></a>SignalR.Sample StockTickerHub.cs

Klasa `StockTickerHub` definiuje cztery dodatkowe metody, które klient może wywoływać:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample23.cs)]

Aplikacja wywołuje `OpenMarket`, `CloseMarket`i `Reset` w odpowiedzi na przyciski w górnej części strony. Pokazują one wzorzec jednego klienta wyzwalającego zmianę stanu natychmiast propagowany do wszystkich klientów. Każda z tych metod wywołuje metodę w klasie `StockTicker`, która powoduje zmianę stanu rynku, a następnie emituje nowy stan.

#### <a name="signalrsample-stocktickercs"></a>Sygnalizujący. przykład StockTicker.cs

W klasie `StockTicker` aplikacja zachowuje stan rynku z właściwością `MarketState`, która zwraca `MarketState` wartość wyliczenia:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample24.cs)]

Każda z metod, które zmieniają stan rynku, to w bloku blokady, ponieważ Klasa `StockTicker` musi być bezpieczna wątkowo:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample25.cs)]

Aby upewnić się, że ten kod jest bezpieczny dla wątków, pole `_marketState`, które wykonuje kopię zapasową właściwości `MarketState` wyznaczono `volatile`:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample26.cs)]

Metody `BroadcastMarketStateChange` i `BroadcastMarketReset` są podobne do metody BroadcastStockPrice, która została już wykorzystana, z tą różnicą, że wywołują różne metody zdefiniowane na kliencie:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample27.cs)]

### <a name="additional-functions-on-the-client-that-the-server-can-call"></a>Dodatkowe funkcje na kliencie, które może wywoływać serwer

Funkcja `updateStockPrice` obsługuje teraz zarówno tabelę, jak i wyświetlanie znaczników i używa `jQuery.Color` do Flash Red i Green Color.

Nowe funkcje w programie *sygnalizującer. StockTicker. js* włącza i wyłącza przyciski na podstawie stanu na rynku. Zatrzymają one również lub uruchamiają przewijanie w poziomie giełdy na **żywo** . Ponieważ wiele funkcji jest dodawanych do `ticker.client`, aplikacja używa [funkcji rozszerzającej jQuery](http://api.jquery.com/jQuery.extend/) , aby je dodać.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample28.js)]

### <a name="additional-client-setup-after-establishing-the-connection"></a>Dodatkowa konfiguracja klienta po ustanowieniu połączenia

Po nawiązaniu połączenia klient ma kilka dodatkowych czynności do wykonania:

* Sprawdź, czy rynek jest otwarty lub zamknięty w celu wywołania odpowiedniej funkcji `marketOpened` lub `marketClosed`.

* Dołącz wywołania metody serwera do przycisków.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample29.js)]

Metody serwera nie są połączone z przyciskami do momentu, gdy aplikacja nawiąże połączenie. Kod nie może wywoływać metod serwera przed ich udostępnieniem.

## <a name="additional-resources"></a>Dodatkowe zasoby

W tym samouczku przedstawiono sposób programowania aplikacji sygnalizującej, która emituje komunikaty z serwera do wszystkich połączonych klientów. Teraz można emitować wiadomości okresowo i w odpowiedzi na powiadomienia z dowolnego klienta. Można użyć koncepcji wielowątkowego wystąpienia pojedynczego, aby zachować stan serwera w scenariuszach gier online obejmujących wiele graczy. Aby zapoznać się z przykładem, zapoznaj [się z grą korzystającą z usługi sygnalizującej](https://github.com/NTaylorMullen/ShootR).

Aby zapoznać się z samouczkami pokazującymi scenariusze komunikacji równorzędnej, zobacz [wprowadzenie z sygnalizacją](introduction-to-signalr.md) i [aktualizacją w czasie rzeczywistym za pomocą usługi sygnalizującego](tutorial-high-frequency-realtime-with-signalr.md).

Aby uzyskać więcej informacji o sygnalizacji, zobacz następujące zasoby:

* [ASP.NET sygnalizujący](../../index.md)
* [Projekt sygnalizującego](http://signalr.net/)
* [Usługi GitHub i przykłady dla sygnałów](https://github.com/SignalR/SignalR)
* [Strona typu wiki sygnalizująca](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a>Następne kroki

W tym samouczku zostaną wykonane następujące czynności:

> [!div class="checklist"]
> * Utworzono projekt
> * Konfigurowanie kodu serwera
> * Zbadano kod serwera
> * Konfigurowanie kodu klienta
> * Zbadano kod klienta
> * Testowanie aplikacji
> * Włączone rejestrowanie

Przejdź do następnego artykułu, aby dowiedzieć się, jak utworzyć aplikację sieci Web w czasie rzeczywistym korzystającą z ASP.NET sygnalizującego 2.
> [!div class="nextstepaction"]
> [Tworzenie aplikacji sieci Web w czasie rzeczywistym za pomocą sygnalizującego](real-time-web-applications-with-signalr.md)

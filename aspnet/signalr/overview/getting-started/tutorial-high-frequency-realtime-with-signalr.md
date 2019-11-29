---
uid: signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
title: 'Samouczek: Tworzenie aplikacji w czasie rzeczywistym o wysokiej częstotliwości z sygnałem 2 | Microsoft Docs'
author: bradygaster
description: W tym samouczku pokazano, jak utworzyć aplikację sieci Web korzystającą z sygnalizującego ASP.NET, aby zapewnić funkcję obsługi komunikatów o wysokiej częstotliwości.
ms.author: bradyg
ms.date: 01/22/2019
ms.assetid: 9f969dda-78ea-4329-b1e3-e51c02210a2b
msc.legacyurl: /signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: 2503e90735d6cfa445ee08c9e43f8443aa106096
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600451"
---
# <a name="tutorial-create-high-frequency-real-time-app-with-signalr-2"></a>Samouczek: Tworzenie aplikacji w czasie rzeczywistym o wysokiej częstotliwości z sygnałem 2

W tym samouczku pokazano, jak utworzyć aplikację sieci Web korzystającą z ASP.NET sygnalizującego 2, aby zapewnić funkcję obsługi komunikatów o wysokiej częstotliwości. W takim przypadku "Obsługa komunikatów o wysokiej częstotliwości" oznacza, że serwer wysyła aktualizacje ze stałą stawką. Wysyłasz do 10 wiadomości w drugim.

Utworzona aplikacja wyświetla kształt, który użytkownicy mogą przeciągać. Serwer aktualizuje położenie kształtu we wszystkich połączonych przeglądarkach, aby dopasować położenie przeciąganego kształtu przy użyciu aktualizacji z limitem czasu.

Koncepcje wprowadzone w tym samouczku zawierają aplikacje w czasie rzeczywistym oraz inne aplikacje symulacyjne.

W tym samouczku przedstawiono następujące instrukcje:

> [!div class="checklist"]
> * Konfigurowanie projektu
> * Tworzenie aplikacji podstawowej
> * Mapuj do centrum podczas uruchamiania aplikacji
> * Dodawanie klienta
> * Uruchamianie aplikacji
> * Dodaj pętlę klienta
> * Dodaj pętlę serwera
> * Dodaj gładką animację

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a>Wymagania wstępne

* [Program Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) z **ASP.NET i programowaniem w sieci Web** .

## <a name="set-up-the-project"></a>Konfigurowanie projektu

W tej sekcji utworzysz projekt w programie Visual Studio 2017.

W tej sekcji pokazano, jak za pomocą programu Visual Studio 2017 utworzyć pustą aplikację sieci Web ASP.NET i dodać do niej elementy sygnalizujące i jQuery. UI.

1. W programie Visual Studio Utwórz aplikację sieci Web ASP.NET.

    ![Tworzenie sieci Web](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

1. W oknie **Nowa aplikacja sieci Web ASP.NET — MoveShapeDemo** , pozostaw **puste** zaznaczone i wybierz **przycisk OK**.

1. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt, a następnie wybierz pozycję **Dodaj** > **nowy element**.

1. W obszarze **Dodaj nowy element — MoveShapeDemo**wybierz pozycję **zainstalowane** > **Visual C#**  > **sieci Web** > **sygnalizujący** , a następnie wybierz pozycję **Klasa centrum sygnałów (v2)** .

1. Nadaj klasie nazwę *MoveShapeHub* i Dodaj ją do projektu.

    Ten krok powoduje utworzenie pliku klasy *MoveShapeHub.cs* . Jednocześnie dodaje zestaw plików skryptów i odwołania do zestawów, które obsługują sygnalizujący do projektu.

1. Wybierz kolejno pozycje **narzędzia** > **menedżer pakietów NuGet** > **konsola Menedżera pakietów**.

1. W **konsoli Menedżera pakietów**Uruchom następujące polecenie:

    ```console
    Install-Package jQuery.UI.Combined
    ```

    Polecenie instaluje bibliotekę interfejsu użytkownika jQuery. Służy do animowania kształtu.

1. W **Eksplorator rozwiązań**rozwiń węzeł skrypty.

    ![Odwołania do biblioteki skryptów](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)

    Biblioteki skryptów dla jQuery, jQueryUI i sygnalizujący są widoczne w projekcie.

## <a name="create-the-base-application"></a>Tworzenie aplikacji podstawowej

W tej sekcji utworzysz aplikację przeglądarki. Aplikacja wysyła lokalizację kształtu do serwera podczas każdego zdarzenia przenoszenia myszy. Serwer emituje te informacje do wszystkich innych podłączonych klientów w czasie rzeczywistym. Więcej informacji na temat tej aplikacji znajdziesz w kolejnych sekcjach.

1. Otwórz plik *MoveShapeHub.cs* .

1. Zastąp kod w pliku *MoveShapeHub.cs* tym kodem:

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.cs)]

1. Zapisz plik.

Klasa `MoveShapeHub` jest implementacją centrum sygnałów. Podobnie jak w przypadku [wprowadzenie z samouczkiem sygnalizującym](tutorial-getting-started-with-signalr.md) , koncentrator ma metodę, którą klienci bezpośrednio wywołują. W takim przypadku klient wysyła obiekt z nowymi współrzędnymi X i Y kształtu do serwera. Te współrzędne są wysyłane do wszystkich innych podłączonych klientów. Program sygnalizujący automatycznie serializować ten obiekt przy użyciu formatu JSON.

Aplikacja wysyła do klienta obiekt `ShapeModel`. Ma elementy członkowskie do przechowywania pozycji kształtu. Wersja obiektu na serwerze ma również element członkowski służący do śledzenia, które dane klienta są przechowywane. Ten obiekt uniemożliwia serwerowi wysyłanie danych klienta z powrotem do samego siebie. Ten element członkowski używa atrybutu `JsonIgnore`, aby uniemożliwić aplikacji Serializowanie danych i wysłanie jej z powrotem do klienta.

## <a name="map-to-the-hub-when-app-starts"></a>Mapuj do centrum podczas uruchamiania aplikacji

Następnie należy skonfigurować mapowanie do centrum podczas uruchamiania aplikacji. W sygnalizacji 2 dodanie klasy uruchomieniowej OWIN powoduje utworzenie mapowania.

1. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt, a następnie wybierz pozycję **Dodaj** > **nowy element**.

1. W obszarze **Dodaj nowy element — MoveShapeDemo** wybierz pozycję **zainstalowane** >  **C# Visual** > **Web** , a następnie wybierz pozycję **Owin klasy startowej**.

1. Nazwij klasę *uruchamiania* i wybierz **przycisk OK**.

1. Zastąp domyślny kod w pliku *Startup.cs* tym kodem:

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.cs)]

Klasa uruchomieniowa OWIN wywołuje `MapSignalR`, gdy aplikacja wykonuje `Configuration` metodę. Aplikacja dodaje klasę do procesu uruchamiania OWIN przy użyciu atrybutu zestawu `OwinStartup`.

## <a name="add-the-client"></a>Dodawanie klienta

Dodaj stronę HTML dla klienta.

1. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt i wybierz polecenie **Dodaj** > **stronę HTML**.

1. Nadaj nazwę stronie **domyślne** i wybierz przycisk **OK**.

1. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy pozycję *default. html* i wybierz pozycję **Ustaw jako stronę startową**.

1. Zastąp domyślny kod w pliku *default. html* tym kodem:

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.html?highlight=14-16)]

1. W **Eksplorator rozwiązań**rozwiń węzeł **skrypty**.

    Biblioteki skryptów dla jQuery i sygnalizujących są widoczne w projekcie.

    > [!IMPORTANT]
    > Menedżer pakietów instaluje nowszą wersję skryptów sygnalizujących.

1. Zaktualizuj odwołania do skryptu w bloku kodu, aby odpowiadały wersji plików skryptów w projekcie.

Ten kod HTML i JavaScript tworzy czerwoną `div` o nazwie `shape`. Umożliwia ona zachowanie przeciągnięcia kształtu przy użyciu biblioteki jQuery i użycie zdarzenia `drag` do wysłania położenia kształtu do serwera.

## <a name="run-the-app"></a>Uruchamianie aplikacji

Możesz uruchomić aplikację, aby se'e jej działanie. Po przeciągnięciu kształtu wokół okna przeglądarki, kształt jest również przenoszony w innych przeglądarkach.

1. Na pasku narzędzi Włącz **debugowanie skryptów** , a następnie wybierz przycisk Odtwórz, aby uruchomić aplikację w trybie debugowania.

    ![Zrzut ekranu przedstawiający Włączanie trybu debugowania przez użytkownika i wybieranie opcji Odtwórz.](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)

    Zostanie otwarte okno przeglądarki z czerwonym kształtem w prawym górnym rogu.

1. Skopiuj adres URL strony.

1. Otwórz kolejną przeglądarkę i wklej adres URL na pasku adresu.

1. Przeciągnij kształt w jednym z okien przeglądarki. Poniższy kształt w drugim oknie przeglądarki.

Gdy aplikacja korzysta z tej metody, nie jest to zalecany model programowania. Nie ma górnego limitu liczby wysyłanych komunikatów. W związku z tym klienci i serwery są przeciążone komunikatami i obniżeniem wydajności. Ponadto aplikacja wyświetla rozłączne animacje na kliencie. Ta animacja Jerky ma miejsce, ponieważ kształt jest przesuwany natychmiast przez każdą metodę. Lepiej, jeśli kształt zostanie bezproblemowo przeniesiony do każdej nowej lokalizacji. Następnie dowiesz się, jak rozwiązać te problemy.

## <a name="add-the-client-loop"></a>Dodaj pętlę klienta

Wysyłanie lokalizacji kształtu przy każdym zdarzeniu przenoszenia myszy powoduje utworzenie niepotrzebnego ruchu sieciowego. Aplikacja musi ograniczyć komunikaty z klienta.

Użyj funkcji `setInterval` JavaScript, aby skonfigurować pętlę, która wysyła nowe informacje o pozycji do serwera ze stałą stawką. Ta pętla to podstawowa reprezentacja "pętli gry". Jest to wielokrotnie wywoływana funkcja, która steruje wszystkimi funkcjami gry.

1. Zastąp kod klienta w pliku *default. html* tym kodem:

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.html?highlight=14-16)]

    > [!IMPORTANT]
    > Należy ponownie zastąpić odwołania do skryptu. Muszą one być zgodne z wersjami skryptów w projekcie.

    Ten nowy kod dodaje funkcję `updateServerModel`. Jest wywoływana z ustaloną częstotliwością. Funkcja wysyła dane położenia do serwera za każdym razem, gdy flaga `moved` wskazuje, że nowe dane położenia są wysyłane.

1. Wybierz przycisk Odtwórz, aby uruchomić aplikację

1. Skopiuj adres URL strony.

1. Otwórz kolejną przeglądarkę i wklej adres URL na pasku adresu.

1. Przeciągnij kształt w jednym z okien przeglądarki. Poniższy kształt w drugim oknie przeglądarki.

Ponieważ aplikacja ogranicza liczbę komunikatów wysyłanych do serwera, animacja nie będzie wyświetlana jako gładka.

## <a name="add-the-server-loop"></a>Dodaj pętlę serwera

W bieżącej aplikacji komunikaty wysyłane z serwera do klienta są wykonywane tak często, jak są odbierane. Ten ruch sieciowy przedstawia podobny problem, który jest widoczny na kliencie.

Aplikacja może wysyłać komunikaty częściej niż jest to zbędne. Połączenie może zostać zalanie w wyniku. W tej sekcji opisano sposób aktualizowania serwera programu w celu dodania czasomierza, który ogranicza szybkość wychodzących komunikatów.

1. Zastąp zawartość `MoveShapeHub.cs` tym kodem:

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

1. Wybierz przycisk Odtwórz, aby uruchomić aplikację.

1. Skopiuj adres URL strony.

1. Otwórz kolejną przeglądarkę i wklej adres URL na pasku adresu.

1. Przeciągnij kształt w jednym z okien przeglądarki.

Ten kod rozszerza klienta, aby dodać klasę `Broadcaster`. Nowa Klasa ogranicza komunikaty wychodzące przy użyciu klasy `Timer` z programu .NET Framework.

Warto dowiedzieć się, że sam koncentrator jest przejściowy. Jest on tworzony za każdym razem, gdy jest to konieczne. Aplikacja tworzy `Broadcaster` jako pojedynczą. Używa inicjalizacji z opóźnieniem, aby odroczyć tworzenie `Broadcaster`do momentu, gdy jest to konieczne. Gwarantuje to, że aplikacja tworzy pierwsze wystąpienie centrum w całości przed rozpoczęciem czasomierza.

Wywołanie funkcji `UpdateShape` klientów zostanie następnie przeniesione z metody `UpdateModel` centrum. Nie jest już wywoływana natychmiast po każdym odebraniu wiadomości przychodzących przez aplikację. Zamiast tego aplikacja wysyła komunikaty do klientów z szybkością 25 wywołań na sekundę. Proces jest zarządzany przez `_broadcastLoop` Timer z poziomu klasy `Broadcaster`.

Na koniec zamiast wywoływania metody klienta bezpośrednio z poziomu centrum Klasa `Broadcaster` musi uzyskać odwołanie do centrum `_hubContext` operacyjnego. Pobiera odwołanie do `GlobalHost`.

## <a name="add-smooth-animation"></a>Dodaj gładką animację

Aplikacja jest niemal zakończona, ale możemy wprowadzić jeszcze jedną poprawę. Aplikacja przenosi kształt na kliencie w odpowiedzi na komunikaty serwera. Zamiast ustawiania położenia kształtu do nowej lokalizacji wskazanej przez serwer, użyj funkcji `animate` biblioteki interfejsu użytkownika JQuery. Może bezproblemowo przenosić kształt między jego bieżącą i nową pozycją.

1. Zaktualizuj metodę `updateShape` klienta w pliku *default. html* , aby wyglądać jak wyróżniony kod:

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.html?highlight=33-40)]

1. Wybierz przycisk Odtwórz, aby uruchomić aplikację.

1. Skopiuj adres URL strony.

1. Otwórz kolejną przeglądarkę i wklej adres URL na pasku adresu.

1. Przeciągnij kształt w jednym z okien przeglądarki.

Przenoszenie kształtu w drugim oknie jest mniej Jerky. Aplikacja interpoluje swój ruch w czasie, a nie ustawia raz na komunikat przychodzący.

Ten kod przenosi kształt ze starej lokalizacji do nowej. Serwer podaje pozycję kształtu w trakcie interwału animacji. W tym przypadku jest to 100 milisekund. Aplikacja czyści wszystkie poprzednie animacje uruchomione na kształcie przed rozpoczęciem nowej animacji.

## <a name="get-the-code"></a>Pobierz kod

[Pobierz ukończony projekt](https://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a)

## <a name="additional-resources"></a>Dodatkowe zasoby

Właśnie zdobyty model komunikacji jest przydatny do tworzenia gier online i innych symulacji, takich jak [gra w ramach programu do tworzenia sygnałów](https://shootr.azurewebsites.net/).

Aby uzyskać więcej informacji o sygnalizacji, zobacz następujące zasoby:

* [Projekt sygnalizującego](http://signalr.net)

* [Usługi GitHub i przykłady dla sygnałów](https://github.com/SignalR/SignalR)

* [Strona typu wiki sygnalizująca](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a>Następne kroki

W tym samouczku przedstawiono następujące instrukcje:

> [!div class="checklist"]
> * Konfigurowanie projektu
> * Utworzono aplikację podstawową
> * Zamapowane do centrum podczas uruchamiania aplikacji
> * Dodano klienta
> * Uruchomiono aplikację
> * Dodano pętlę klienta
> * Dodano pętlę serwera
> * Dodano gładką animację

Przejdź do następnego artykułu, aby dowiedzieć się, jak utworzyć aplikację sieci Web, która korzysta z ASP.NET sygnalizującego 2, aby zapewnić funkcję emisji serwera.
> [!div class="nextstepaction"]
> [Sygnalizacja 2 i emisja serwera](tutorial-server-broadcast-with-signalr.md)
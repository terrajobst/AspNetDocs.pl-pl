---
uid: signalr/overview/older-versions/tutorial-high-frequency-realtime-with-signalr
title: Czas rzeczywisty o wysokiej częstotliwości z sygnałem 1. x | Microsoft Docs
author: bradygaster
description: W tym samouczku pokazano, jak utworzyć aplikację sieci Web korzystającą z sygnalizującego ASP.NET, aby zapewnić funkcję obsługi komunikatów o wysokiej częstotliwości. Komunikaty o wysokiej częstotliwości w...
ms.author: bradyg
ms.date: 04/16/2013
ms.assetid: ad2a5da5-2e79-40ea-bc84-028d327f5982
msc.legacyurl: /signalr/overview/older-versions/tutorial-high-frequency-realtime-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: e37e63a410952ec170cbbaadeeb54eae7e82b3b5
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78623502"
---
# <a name="high-frequency-realtime-with-signalr-1x"></a>Wysyłanie w czasie rzeczywistym z wysoką częstotliwością przy użyciu usługi SignalR 1.x

[Fletcher Patryka](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> W tym samouczku pokazano, jak utworzyć aplikację sieci Web korzystającą z sygnalizującego ASP.NET, aby zapewnić funkcję obsługi komunikatów o wysokiej częstotliwości. Komunikaty o wysokiej częstotliwości w tym przypadku oznaczają aktualizacje, które są wysyłane ze stałą stawką; w przypadku tej aplikacji, do 10 komunikatów w drugim.
> 
> Aplikacja, którą utworzysz w tym samouczku, wyświetla kształt, który użytkownicy mogą przeciągać. Pozycja kształtu we wszystkich innych połączonych przeglądarkach zostanie zaktualizowana w celu dopasowania do położenia przeciągniętego kształtu przy użyciu aktualizacji czasowych.
> 
> Koncepcje wprowadzone w tym samouczku zawierają aplikacje w czasie rzeczywistym oraz inne aplikacje symulacyjne.
> 
> Komentarze w samouczku są powitane. Jeśli masz pytania, które nie są bezpośrednio związane z samouczkiem, możesz je ogłosić na [forum ASP.NET](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com).

## <a name="overview"></a>Omówienie

W tym samouczku pokazano, jak utworzyć aplikację, która współużytkuje stan obiektu z innymi przeglądarkami w czasie rzeczywistym. Utworzona aplikacja jest nazywana MoveShape. Na stronie MoveShape zostanie wyświetlony element DIV HTML, który użytkownik może przeciągnąć; gdy użytkownik przeciągnie DIV, jego nowe położenie zostanie wysłane do serwera, co spowoduje, że wszyscy pozostali połączeni klienci zaktualizują położenie kształtu do dopasowania.

![Okno aplikacji](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

Aplikacja utworzona w tym samouczku jest oparta na demonstracji Damian Edwards. Film wideo zawierający tę wersję demonstracyjną można zobaczyć [tutaj](https://channel9.msdn.com/Series/Building-Web-Apps-with-ASP-NET-Jump-Start/Building-Web-Apps-with-ASPNET-Jump-Start-08-Real-time-Communication-with-SignalR).

Samouczek rozpocznie się, pokazując, jak wysyłać komunikaty sygnalizujące z każdego zdarzenia, które jest wyzwalane podczas przeciągania kształtu. Każdy połączony klient zaktualizuje pozycję lokalnej wersji kształtu za każdym razem, gdy komunikat zostanie odebrany.

Gdy aplikacja będzie działać przy użyciu tej metody, nie jest to zalecany model programowania, ponieważ nie byłby górny limit liczby wysyłanych komunikatów, dzięki czemu klienci i serwery mogą być przeciążone komunikatami, a wydajność może być gorsza . Wyświetlana animacja na kliencie również zostanie rozłączona, ponieważ kształt zostanie natychmiast przesunięty przez każdą metodę, a nie zostanie bezproblemowo przenoszony do każdej nowej lokalizacji. Dalsze sekcje samouczka pokazują, jak utworzyć funkcję Timer, która ogranicza maksymalną szybkość przesyłania komunikatów przez klienta lub serwer oraz sposób bezproblemowego przenoszenia kształtu między lokalizacjami. Końcową wersję aplikacji utworzoną w tym samouczku można pobrać z [galerii kodu](https://code.msdn.microsoft.com/SignalR-MoveShape-demo-3366dac6).

Ten samouczek zawiera następujące sekcje:

- [Wymagania wstępne](#prerequisites)
- [Tworzenie projektu](#createtheproject)
- [Dodaj ASP.NET sygnalizujący i JQuery. UI pakiety NuGet](#nugetpackages)
- [Tworzenie aplikacji podstawowej](#baseapp)
- [Dodaj pętlę klienta](#clientloop)
- [Dodaj pętlę serwera](#serverloop)
- [Dodawanie gładkiej animacji na kliencie](#animation)
- [Dalsze kroki](#furthersteps)

<a id="prerequisites"></a>

## <a name="prerequisites"></a>Wymagania wstępne

Ten samouczek wymaga programu Visual Studio 2012 lub Visual Studio 2010. Jeśli używany jest program Visual Studio 2010, projekt będzie używać .NET Framework 4, a nie .NET Framework 4,5.

W przypadku korzystania z programu Visual Studio 2012 zaleca się zainstalowanie [aktualizacji ASP.NET and Web Tools 2012,2](https://go.microsoft.com/fwlink/?LinkId=282650). Ta aktualizacja zawiera nowe funkcje, takie jak ulepszenia publikowania, nowych funkcji i nowych szablonów.

Jeśli masz program Visual Studio 2010, upewnij się, że jest zainstalowany pakiet [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) .

<a id="createtheproject"></a>

## <a name="create-the-project"></a>Tworzenie projektu

W tej sekcji utworzymy projekt w programie Visual Studio.

1. W menu **plik** kliknij pozycję **Nowy projekt**.
2. W oknie dialogowym **Nowy projekt** rozwiń węzeł **C#** w obszarze **Szablony** i wybierz pozycję **Sieć Web**.
3. Wybierz szablon **ASP.NET pustej aplikacji sieci Web** , Nadaj projektowi nazwę *MoveShapeDemo*i kliknij przycisk **OK**.

    ![Tworzenie nowego projektu](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)

<a id="nugetpackages"></a>

## <a name="add-the-signalr-and-jqueryui-nuget-packages"></a>Dodaj sygnały sygnalizujące i JQuery. UI pakiety NuGet

Do projektu można dodać funkcję sygnalizującą, instalując pakiet NuGet. Ten samouczek będzie również używać pakietu JQuery. UI, aby umożliwić przeciąganie i animowanie kształtu.

1. Kliknij pozycję **Narzędzia | Menedżer pakietów NuGet | Konsola Menedżera pakietów**.
2. W Menedżerze pakietów wprowadź następujące polecenie.

    [!code-powershell[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.ps1)]

    Pakiet sygnalizujący instaluje wiele innych pakietów NuGet jako zależności. Po zakończeniu instalacji masz wszystkie składniki serwera i klienta wymagane do użycia programu sygnalizującego w aplikacji ASP.NET.
3. Wprowadź następujące polecenie w konsoli Menedżera pakietów, aby zainstalować pakiety JQuery i JQuery. UI.

    [!code-powershell[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.ps1)]

<a id="baseapp"></a>

## <a name="create-the-base-application"></a>Tworzenie aplikacji podstawowej

W tej sekcji utworzymy aplikację przeglądarki, która wyśle lokalizację kształtu do serwera podczas każdego zdarzenia przenoszenia myszy. Następnie serwer emituje te informacje do wszystkich innych podłączonych klientów w miarę ich odbierania. Ta aplikacja zostanie rozwinięta w kolejnych sekcjach.

1. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt i wybierz polecenie **Dodaj**, **Klasa.** .. Nadaj klasie nazwę **MoveShapeHub** i kliknij przycisk **Dodaj**.
2. Zastąp kod w nowej klasie **MoveShapeHub** następującym kodem.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.cs)]

    Powyższa Klasa `MoveShapeHub` jest implementacją centrum sygnałów. Podobnie jak w [wprowadzenie z samouczkiem sygnalizującym](index.md) , koncentrator ma metodę, którą klienci będą wywoływać bezpośrednio. W takim przypadku klient wyśle obiekt zawierający nowe współrzędne X i Y kształtu do serwera, który następnie jest emitowany do wszystkich innych podłączonych klientów. Program sygnalizujący automatycznie serializować ten obiekt przy użyciu formatu JSON.

    Obiekt, który zostanie wysłany do klienta (`ShapeModel`) zawiera elementy członkowskie do przechowywania pozycji kształtu. Wersja obiektu na serwerze zawiera również element członkowski służący do śledzenia, które dane klienta są przechowywane, dzięki czemu dany klient nie będzie wysyłał swoich danych. Ten element członkowski używa atrybutu `JsonIgnore`, aby zachować jego serializację i przesłać go do klienta.
3. Następnie skonfigurujemy centrum podczas uruchamiania aplikacji. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt, a następnie kliknij polecenie **Dodaj | Globalna Klasa aplikacji**. Zaakceptuj domyślną nazwę *globalną* , a następnie kliknij przycisk **OK**.

    ![Dodaj globalną klasę aplikacji](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)
4. Dodaj następującą instrukcję `using` po podane instrukcje **using** w klasie Global.asax.cs.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.cs)]
5. Dodaj następujący wiersz kodu w metodzie `Application_Start` klasy globalnej w celu zarejestrowania trasy domyślnej dla sygnalizującego.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

    Plik Global. asax powinien wyglądać następująco:

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.cs)]
6. Następnie dodamy klienta. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt, a następnie kliknij polecenie **Dodaj | Nowy element**. W oknie dialogowym **Dodaj nowy element** wybierz pozycję **strona HTML**. Nadaj stronie odpowiednią nazwę (np **. default. html**), a następnie kliknij przycisk **Dodaj**.
7. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy utworzoną właśnie stronę i kliknij pozycję **Ustaw jako stronę startową**.
8. Zastąp domyślny kod na stronie HTML następującym fragmentem kodu.

    > [!NOTE]
    > Sprawdź, czy poniższe odwołania do skryptu pasują do pakietów dodanych do projektu w folderze skryptów. W programie Visual Studio 2010 wersja JQuery i sygnalizującego dodanego do projektu może być niezgodna z numerami wersji poniżej.

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample7.html)]

    Powyższy kod HTML i JavaScript tworzy czerwony blok DIV o nazwie Shape, włącza zachowanie przeciągania kształtu przy użyciu biblioteki jQuery i używa zdarzenia `drag` kształtu do wysłania położenia kształtu do serwera.
9. Uruchom aplikację, naciskając klawisz F5. Skopiuj adres URL strony i wklej go w drugim oknie przeglądarki. Przeciągnij kształt w jednym z okien przeglądarki; kształt w innym oknie przeglądarki powinien zostać przeniesiony.

    ![Okno aplikacji](tutorial-high-frequency-realtime-with-signalr/_static/image4.png)

<a id="clientloop"></a>

## <a name="add-the-client-loop"></a>Dodaj pętlę klienta

Ponieważ wysyłanie lokalizacji kształtu przy każdym zdarzeniu przenoszenia myszy spowoduje utworzenie niepotrzebnego ruchu sieciowego, komunikaty z klienta muszą być ograniczone. Użyjemy funkcji `setInterval` JavaScript w celu skonfigurowania pętli, która wysyła nowe informacje o pozycji do serwera ze stałą stawką. Ta pętla to bardzo podstawowa reprezentacja "pętli gry", wielokrotnie wywoływanej funkcji, która służy do obsługi wszystkich funkcji gry lub innej symulacji.

1. Zaktualizuj kod klienta na stronie HTML tak, aby pasował do poniższego fragmentu kodu.

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample8.html)]

    Powyższa aktualizacja dodaje funkcję `updateServerModel`, która jest wywoływana z ustaloną częstotliwością. Ta funkcja wysyła dane położenia do serwera za każdym razem, gdy flaga `moved` wskazuje, że nowe dane położenia są wysyłane.
2. Uruchom aplikację, naciskając klawisz F5. Skopiuj adres URL strony i wklej go w drugim oknie przeglądarki. Przeciągnij kształt w jednym z okien przeglądarki; kształt w innym oknie przeglądarki powinien zostać przeniesiony. Ponieważ liczba komunikatów wysłanych do serwera zostanie ograniczona, animacja nie będzie wyświetlana jako gładka, jak w poprzedniej sekcji.

    ![Okno aplikacji](tutorial-high-frequency-realtime-with-signalr/_static/image5.png)

<a id="serverloop"></a>

## <a name="add-the-server-loop"></a>Dodaj pętlę serwera

W bieżącej aplikacji komunikaty wysyłane z serwera do klienta są wykonywane tak często, jak są odbierane. Przedstawia to podobny problem, który był widoczny na kliencie; komunikaty mogą być wysyłane częściej niż są one zbędne, a połączenie może zostać zalewane jako wynik. W tej sekcji opisano sposób aktualizowania serwera w celu zaimplementowania czasomierza, który ogranicza szybkość wychodzących komunikatów.

1. Zastąp zawartość `MoveShapeHub.cs` poniższym fragmentem kodu.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample9.cs)]

    Powyższy kod rozszerza klienta, aby dodać klasę `Broadcaster`, która ogranicza komunikaty wychodzące przy użyciu klasy `Timer` z programu .NET Framework.

    Ponieważ sam koncentrator jest tymczasowy (jest tworzony za każdym razem, gdy jest to konieczne), `Broadcaster` zostanie utworzona jako pojedyncza. Inicjalizacja z opóźnieniem (wprowadzona w programie .NET 4) jest używana do odroczenia jego tworzenia do momentu, gdy jest to konieczne, co gwarantuje, że pierwsze wystąpienie centrum zostało całkowicie utworzone przed uruchomieniem czasomierza.

    Wywołanie funkcji `UpdateShape` klientów zostanie następnie przeniesione z metody `UpdateModel` centrum, aby nie było już wywoływana natychmiast po odebraniu komunikatów przychodzących. Zamiast tego komunikaty wysyłane do klientów będą przesyłane z częstotliwością 25 wywołań na sekundę, zarządzane przez czasomierz `_broadcastLoop` z poziomu klasy `Broadcaster`.

    Na koniec zamiast wywołania metody klienta bezpośrednio z centrum, Klasa `Broadcaster` musi uzyskać odwołanie do aktualnie działającego centrum (`_hubContext`) przy użyciu `GlobalHost`.
2. Uruchom aplikację, naciskając klawisz F5. Skopiuj adres URL strony i wklej go w drugim oknie przeglądarki. Przeciągnij kształt w jednym z okien przeglądarki; kształt w innym oknie przeglądarki powinien zostać przeniesiony. W przeglądarce nie będzie widoczna różnica w stosunku do poprzedniej sekcji, ale liczba komunikatów wysłanych do klienta zostanie ograniczona.

    ![Okno aplikacji](tutorial-high-frequency-realtime-with-signalr/_static/image6.png)

<a id="animation"></a>

## <a name="add-smooth-animation-on-the-client"></a>Dodawanie gładkiej animacji na kliencie

Aplikacja jest prawie ukończona, ale możemy wprowadzić jeszcze większą poprawę w obruchie kształtu na kliencie w miarę ich przenoszenia w odpowiedzi na komunikaty serwera. Zamiast ustawiania pozycji kształtu do nowej lokalizacji podaną przez serwer, użyjemy `animate` funkcji biblioteki interfejsu użytkownika JQuery, aby bezproblemowo przesunąć kształt między jego bieżącą i nową pozycją.

1. Zaktualizuj metodę `updateShape` klienta, aby wyglądała jak wyróżniony kod poniżej:

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample10.html?highlight=35-42)]

    Powyższy kod przenosi kształt ze starej lokalizacji do nowego, podanego przez serwer w czasie trwania interwału animacji (w tym przypadku 100 milisekund). Wszystkie poprzednie animacje uruchomione na kształcie są czyszczone przed rozpoczęciem nowej animacji.
2. Uruchom aplikację, naciskając klawisz F5. Skopiuj adres URL strony i wklej go w drugim oknie przeglądarki. Przeciągnij kształt w jednym z okien przeglądarki; kształt w innym oknie przeglądarki powinien zostać przeniesiony. Przenoszenie kształtu w drugim oknie powinno być mniej Jerky, ponieważ jego przenoszenie jest interpolowane w czasie, a nie ustawiane raz na komunikat przychodzący.

    ![Okno aplikacji](tutorial-high-frequency-realtime-with-signalr/_static/image7.png)

<a id="furthersteps"></a>

## <a name="further-steps"></a>Dalsze kroki

W tym samouczku pokazano, jak programować aplikację sygnalizującą, która wysyła komunikaty o wysokiej częstotliwości między klientami a serwerami. Ten model komunikacji jest przydatny do tworzenia gier online i innych symulacji, takich jak [rozgrywka utworzona za pomocą sygnalizującego](http://shootr.signalr.net).

Kompletną aplikację utworzoną w tym samouczku można pobrać z [galerii kodu](https://code.msdn.microsoft.com/SignalR-MoveShape-demo-3366dac6).

Aby dowiedzieć się więcej na temat pojęć związanych z programowaniem sygnałów, odwiedź następujące witryny dotyczące kodu źródłowego i zasobów usługi sygnalizującego:

- [Projekt sygnalizującego](http://signalr.net)
- [Usługi GitHub i przykłady dla sygnałów](https://github.com/SignalR/SignalR)
- [Strona typu wiki sygnalizująca](https://github.com/SignalR/SignalR/wiki)

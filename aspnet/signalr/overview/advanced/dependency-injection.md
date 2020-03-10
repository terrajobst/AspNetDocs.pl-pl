---
uid: signalr/overview/advanced/dependency-injection
title: Iniekcja zależności w sygnale | Microsoft Docs
author: bradygaster
description: Wersje oprogramowania używane w tym temacie Visual Studio 2013 programu .NET 4,5 sygnalizującego w wersji 2 poprzednie wersje tego tematu, aby uzyskać informacje o wcześniejszych wersjach programu...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: a14121ae-02cf-4024-8af0-9dd0dc810690
msc.legacyurl: /signalr/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 52978b10b6c131ac8eff4535216cc60b43fdf3de
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78537332"
---
# <a name="dependency-injection-in-signalr"></a>Wstrzykiwanie zależności w usłudze SignalR

według [Jan Wasson](https://github.com/MikeWasson), [Patryk Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> ## <a name="software-versions-used-in-this-topic"></a>Wersje oprogramowania używane w tym temacie
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - Sygnalizujący wersja 2
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

Iniekcja zależności polega na usunięciu sztywnych zależności między obiektami, ułatwiając zastępowanie zależności obiektu, w przypadku testowania (przy użyciu obiektów makiety) lub zmiany zachowania w czasie wykonywania. Ten samouczek pokazuje, jak przeprowadzić iniekcję zależności w centrach sygnałów. Przedstawiono w nim również, jak używać kontenerów IoC z sygnalizacyjnym. Kontener IoC to ogólna struktura iniekcji zależności.

## <a name="what-is-dependency-injection"></a>Co to jest iniekcja zależności?

Pomiń tę sekcję, jeśli znasz już funkcję iniekcji zależności.

*Iniekcja zależności* (di) jest wzorcem, w którym obiekty nie są odpowiedzialne za tworzenie własnych zależności. Oto prosty przykład do motywu DI. Załóżmy, że masz obiekt, który musi rejestrować komunikaty. Można zdefiniować interfejs rejestrowania:

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

W obiekcie można utworzyć `ILogger` do rejestrowania komunikatów:

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

To działa, ale nie jest najlepszym projektem. Jeśli chcesz zastąpić `FileLogger` inną implementacją `ILogger`, musisz zmodyfikować `SomeComponent`. Załóżmy, że wiele innych obiektów używa `FileLogger`, należy zmienić wszystkie z nich. Lub jeśli zdecydujesz się wprowadzić `FileLogger` pojedynczej, musisz również wprowadzić zmiany w całej aplikacji.

Lepszym rozwiązaniem jest "wstrzyknięcie" `ILogger` do obiektu — na przykład przy użyciu argumentu konstruktora:

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

Teraz obiekt nie odpowiada za wybór `ILogger` do użycia. Można zmienić implementacje `ILogger` bez zmiany obiektów, które są od niego zależne.

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

Ten wzorzec jest nazywany [iniekcją konstruktora](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection). Inny wzorzec to iniekcja setter, w której ustawia się zależność za pomocą metody ustawiającej lub właściwości.

## <a name="simple-dependency-injection-in-signalr"></a>Proste iniekcja zależności w sygnale

Rozważmy aplikację Chat z samouczka [wprowadzenie za pomocą programu sygnalizującego](../getting-started/tutorial-getting-started-with-signalr.md). Oto Klasa centrum z tej aplikacji:

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

Załóżmy, że chcesz przechowywać komunikaty czatu na serwerze przed ich wysłaniem. Można zdefiniować interfejs, który jest abstrakcyjny dla tej funkcji, i użyć funkcji DI, aby wstrzyknąć interfejs do klasy `ChatHub`.

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

Jedyny problem polega na tym, że aplikacja sygnalizująca nie tworzy bezpośrednio centrów. Tworzy je dla Ciebie. Domyślnie program sygnalizujący oczekuje, że Klasa centrum ma Konstruktor bez parametrów. Można jednak łatwo zarejestrować funkcję do tworzenia wystąpień centrów i użyć tej funkcji do wykonania DI. Zarejestruj funkcję przez wywołanie **GlobalHost. DependencyResolver. Register**.

[!code-csharp[Main](dependency-injection/samples/sample7.cs)]

Teraz sygnalizujący wywoła tę funkcję anonimową za każdym razem, gdy musi utworzyć wystąpienie `ChatHub`.

## <a name="ioc-containers"></a>Kontenery IoC

Poprzedni kod jest bardzo drobny w przypadku prostych przypadków. Jednak nadal musiałeś napisać:

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

W złożonej aplikacji z wieloma zależnościami może być konieczne napisanie wielu tych kodów "okablowania". Ten kod może być trudny do utrzymania, szczególnie jeśli zależności są zagnieżdżone. Test jednostkowy jest również trudne.

Jednym z rozwiązań jest użycie kontenera IoC. Kontener IoC to składnik oprogramowania, który jest odpowiedzialny za zarządzanie zależnościami. Możesz zarejestrować typy w kontenerze, a następnie użyć kontenera do tworzenia obiektów. Kontener automatycznie określa relacje zależności. Wiele kontenerów IoC umożliwia również Sterowanie elementami, takimi jak okres istnienia obiektu i zakres.

> [!NOTE]
> "IoC" oznacza "Inversion of Control", który jest ogólnym wzorcem, w którym struktura wywołuje kod aplikacji. Kontener IoC konstruuje obiekty, co oznacza, że jest to normalny przepływ sterowania.

## <a name="using-ioc-containers-in-signalr"></a>Używanie kontenerów IoC w sygnalizacji

Aplikacja Chat jest prawdopodobnie zbyt prosta, aby można było skorzystać z kontenera IoC. Zamiast tego Przyjrzyjmy się przykładowi [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) .

Przykład StockTicker definiuje dwie klasy główne:

- `StockTickerHub`: Klasa Hub, która zarządza połączeniami klientów.
- `StockTicker`: pojedynczy, który przechowuje ceny giełdowe, i okresowo aktualizuje je.

`StockTickerHub` przechowuje odwołanie do `StockTicker` pojedynczej, podczas gdy `StockTicker` przechowuje odwołanie do **IHubConnectionContext** dla `StockTickerHub`. Używa tego interfejsu do komunikowania się z wystąpieniami `StockTickerHub`. (Aby uzyskać więcej informacji, zobacz [Server Broadcast with ASP.NET signaler](../getting-started/tutorial-server-broadcast-with-signalr.md)).

Możemy użyć kontenera IoC, aby porządkowaniem te zależności jako bity. Najpierw uprośmy klasy `StockTickerHub` i `StockTicker`. W poniższym kodzie mam komentarz do niepotrzebnych elementów.

Usuń Konstruktor bez parametrów z `StockTickerHub`. Zamiast tego zawsze będziemy używać narzędzia DI do utworzenia centrum.

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

W przypadku StockTicker Usuń pojedyncze wystąpienie. Później będziemy używać kontenera IoC do kontrolowania okresu istnienia StockTicker. Ponadto ustaw konstruktora jako publiczny.

[!code-csharp[Main](dependency-injection/samples/sample10.cs?highlight=7)]

Następnie możemy zakodować kod przez utworzenie interfejsu dla `StockTicker`. Użyjemy tego interfejsu, aby rozdzielić `StockTickerHub` z klasy `StockTicker`.

Program Visual Studio ułatwia ten rodzaj refaktoryzacji. Otwórz plik StockTicker.cs, kliknij prawym przyciskiem myszy deklarację klasy `StockTicker` i wybierz pozycję **Refaktoryzacja** ... **Wyodrębnij interfejs**.

![](dependency-injection/_static/image1.png)

W oknie dialogowym **wyodrębnianie interfejsu** kliknij pozycję **Zaznacz wszystko**. Pozostaw inne wartości domyślne. Kliknij przycisk **OK**.

![](dependency-injection/_static/image2.png)

Program Visual Studio tworzy nowy interfejs o nazwie `IStockTicker`, a także zmienia `StockTicker` na pochodny od `IStockTicker`.

Otwórz plik IStockTicker.cs i Zmień interfejs na **publiczny**.

[!code-csharp[Main](dependency-injection/samples/sample11.cs?highlight=1)]

W klasie `StockTickerHub` Zmień dwa wystąpienia `StockTicker` na `IStockTicker`:

[!code-csharp[Main](dependency-injection/samples/sample12.cs?highlight=4,6)]

Tworzenie interfejsu `IStockTicker` nie jest absolutnie konieczne, ale chcę pokazać, jak DI może pomóc w zmniejszeniu sprzęgania między składnikami w aplikacji.

## <a name="add-the-ninject-library"></a>Dodaj bibliotekę Ninject

Istnieje wiele kontenerów IoC "open source" dla platformy .NET. W tym samouczku użyjemy [Ninject](http://www.ninject.org/). (Inne popularne biblioteki obejmują [Castle Windsor](http://www.castleproject.org/), [Spring.NET](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity)i [StructureMap](http://docs.structuremap.net)).

Zainstaluj [bibliotekę Ninject](https://nuget.org/packages/Ninject/3.0.1.10)za pomocą Menedżera pakietów NuGet. W programie Visual Studio, w menu **Narzędzia** wybierz pozycję **menedżer pakietów NuGet** > **konsola Menedżera pakietów**. W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:

[!code-powershell[Main](dependency-injection/samples/sample13.ps1)]

## <a name="replace-the-signalr-dependency-resolver"></a>Zastępowanie programu rozpoznawania zależności sygnalizującego

Aby użyć Ninject w ramach sygnalizującego, Utwórz klasę, która pochodzi od **DefaultDependencyResolver**.

[!code-csharp[Main](dependency-injection/samples/sample14.cs)]

Ta klasa przesłania metody **GetService** i **GetServices** **DefaultDependencyResolver**. Sygnał wywołuje te metody, aby utworzyć różne obiekty w środowisku uruchomieniowym, w tym wystąpienia centrów, a także różne usługi używane wewnętrznie przez program sygnalizujący.

- Metoda **GetService** tworzy pojedyncze wystąpienie typu. Zastąp tę metodę, aby wywołać metodę **TryGet** jądra Ninject. Jeśli ta metoda zwróci wartość null, powraca do domyślnego programu rozpoznawania nazw.
- Metoda **GetServices** tworzy kolekcję obiektów określonego typu. Zastąp tę metodę, aby połączyć wyniki z Ninject z wynikami domyślnego programu rozpoznawania nazw.

## <a name="configure-ninject-bindings"></a>Konfigurowanie powiązań Ninject

Teraz będziemy używać Ninject do deklarowania powiązań typów.

Otwórz klasę Startup.cs aplikacji (utworzoną ręcznie zgodnie z instrukcjami pakietu w `readme.txt`lub utworzoną przez dodanie uwierzytelniania do projektu). W metodzie `Startup.Configuration` Utwórz kontener Ninject, który Ninject wywołuje *jądro*.

[!code-csharp[Main](dependency-injection/samples/sample15.cs)]

Utwórz wystąpienie naszego niestandardowego programu rozpoznawania zależności:

[!code-csharp[Main](dependency-injection/samples/sample16.cs)]

Utwórz powiązanie dla `IStockTicker` w następujący sposób:

[!code-csharp[Main](dependency-injection/samples/sample17.cs)]

Ten kod zawiera dwie rzeczy. Po pierwsze, zawsze, gdy aplikacja wymaga `IStockTicker`, jądro powinno utworzyć wystąpienie `StockTicker`. Druga klasa `StockTicker` powinna być utworzona jako obiekt pojedynczy. Ninject utworzy jedno wystąpienie obiektu i zwróci to samo wystąpienie dla każdego żądania.

Utwórz powiązanie dla **IHubConnectionContext** w następujący sposób:

[!code-csharp[Main](dependency-injection/samples/sample18.cs)]

Ten kod tworzy funkcję anonimową zwracającą **IHubConnection**. Metoda **WhenInjectedInto** informuje Ninject o użyciu tej funkcji tylko podczas tworzenia wystąpień `IStockTicker`. Przyczyną jest to, że sygnalizujący tworzy wewnętrznie wystąpienia **IHubConnectionContext** i nie chcemy przesłonić sposobu tworzenia przez program sygnalizującego. Ta funkcja ma zastosowanie tylko do naszej klasy `StockTicker`.

Przekaż program rozpoznawania zależności do metody **MapSignalR** , dodając konfigurację centrum:

[!code-csharp[Main](dependency-injection/samples/sample19.cs)]

Zaktualizuj metodę Startup. ConfigureSignalR w klasie startowej przykładu przy użyciu nowego parametru:

[!code-csharp[Main](dependency-injection/samples/sample20.cs)]

Teraz sygnalizujący będzie używać mechanizmu rozwiązywania konfliktów określonego w **MapSignalR**zamiast domyślnego programu rozpoznawania nazw.

Oto kompletna lista kodu dla `Startup.Configuration`.

[!code-csharp[Main](dependency-injection/samples/sample21.cs)]

Aby uruchomić aplikację StockTicker w programie Visual Studio, naciśnij klawisz F5. W oknie przeglądarki przejdź do `http://localhost:*port*/SignalR.Sample/StockTicker.html`.

![](dependency-injection/_static/image3.png)

Aplikacja ma dokładnie te same funkcje jak wcześniej. (Aby uzyskać opis, zobacz [Server Broadcast with ASP.NET signaler](../getting-started/tutorial-server-broadcast-with-signalr.md)). Zachowanie nie zostało zmienione; znacznie ułatwiają testowanie, konserwację i rozwijanie kodu.

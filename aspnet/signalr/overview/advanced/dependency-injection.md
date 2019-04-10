---
uid: signalr/overview/advanced/dependency-injection
title: Wstrzykiwanie zależności w SignalR | Dokumentacja firmy Microsoft
author: bradygaster
description: Wersje oprogramowania, używaną w tym temacie dodatku .NET 4.5 SignalR dla programu Visual Studio 2013 w wersji 2, poprzednie wersje w tym temacie informacji o wcześniejszych wersjach...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: a14121ae-02cf-4024-8af0-9dd0dc810690
msc.legacyurl: /signalr/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 1b5d36529b52dfcbebf34cbfa230b3b3b4e83b81
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/09/2019
ms.locfileid: "59405381"
---
# <a name="dependency-injection-in-signalr"></a>Wstrzykiwanie zależności w usłudze SignalR

przez [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> ## <a name="software-versions-used-in-this-topic"></a>Wersje oprogramowania używaną w tym temacie
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - SignalR w wersji 2
>
>
>
> ## <a name="previous-versions-of-this-topic"></a>Poprzednie wersje tego tematu
>
> Aby uzyskać informacje dotyczące starszych wersji biblioteki SignalR, zobacz [starsze wersje biblioteki SignalR](../older-versions/index.md).
>
> ## <a name="questions-and-comments"></a>Pytania i komentarze
>
> Jak się podoba w tym samouczku, i co można było ulepszyć proces w komentarzach u dołu strony, wystaw opinię. Jeśli masz pytania, na które nie są bezpośrednio związane z tego samouczka, możesz zamieścić je do [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).


Wstrzykiwanie zależności jest sposobem usunięcia ustalonych zależności między obiektami, ułatwiając Zastąp zależności obiektu do testowania (przy użyciu obiektów makiety) lub zmienić zachowanie w czasie wykonywania. W tym samouczku pokazano, jak wykonać wstrzykiwanie zależności na koncentratorów SignalR. Pokazano również, jak używać kontenerów IoC za pomocą biblioteki SignalR. Kontenera IoC to ogólne umożliwiająca iniekcji zależności.

## <a name="what-is-dependency-injection"></a>Co to jest wstrzykiwanie zależności?

Pomiń tę sekcję, jeśli już znasz iniekcji zależności.

*Wstrzykiwanie zależności* (DI) jest wzorcem, w którym obiekty nie są odpowiedzialne za tworzenie własnych zależności. Poniżej przedstawiono prosty przykład bodziec DI. Załóżmy, że obiekt, który trzeba komunikaty dziennika. Można zdefiniować interfejs rejestrowania:

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

Obiekt, umożliwia tworzenie `ILogger` na komunikaty dziennika:

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

Ta funkcja działa, ale nie jest to najlepszy projekt. Jeśli chcesz zastąpić `FileLogger` z inną `ILogger` implementacji, trzeba będzie zmodyfikować `SomeComponent`. Przypuszczenia, że wiele innych obiektów użyj `FileLogger`, musisz zmienić ich wszystkich. Lub jeśli użytkownik zdecyduje się wprowadzić `FileLogger` pojedynczych, należy także wprowadzić zmiany w całej aplikacji.

Lepszym rozwiązaniem jest "wstrzyknąć" `ILogger` do obiektu — na przykład przy użyciu argumentu konstruktora:

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

Teraz obiekt nie jest odpowiedzialny za wybierając, które `ILogger` do użycia. Możesz przełączać `ILogger` implementacje bez wprowadzania zmian obiektów, które od niego zależne.

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

Ten wzorzec jest nazywany [iniekcji konstruktora](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection). Inny wzorzec jest iniekcji setter, gdzie ustawiasz zależności za pomocą metody ustawiającej lub właściwości.

## <a name="simple-dependency-injection-in-signalr"></a>Wstrzykiwanie zależności proste w SignalR

Należy wziąć pod uwagę aplikacji rozmów z tego samouczka [wprowadzenie do SignalR](../getting-started/tutorial-getting-started-with-signalr.md). Poniżej przedstawiono klasy koncentratora z tej aplikacji:

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

Załóżmy, że chcesz przechowywanie wiadomości rozmów na serwerze przed ich wysłaniem. Może być zdefiniuj interfejs zapewniający abstrakcję tej funkcji i umożliwia DI wstrzyknięcie interfejsu do `ChatHub` klasy.

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

Problem polega na to, że aplikacji SignalR nie tworzy bezpośrednio koncentratory; SignalR utworzy je. Domyślnie SignalR oczekuje, że ma domyślnego konstruktora klasy koncentratora. Jednak można łatwo zarejestrowaniu funkcję, aby tworzyć wystąpienia koncentratora i użyć tej funkcji do wykonywania DI. Rejestrowanie funkcji przez wywołanie metody **GlobalHost.DependencyResolver.Register**.

[!code-csharp[Main](dependency-injection/samples/sample7.cs)]

Teraz SignalR wywoła tę funkcję anonimowe zawsze wtedy, gdy wymagane do utworzenia `ChatHub` wystąpienia.

## <a name="ioc-containers"></a>Kontenery IoC

Powyższy kod jest w dobrym stanie, w przypadku prostych. Jednak nadal musiały zapisu to:

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

W aplikacji złożonych z wielu zależnościami może być konieczne zapisu mnóstwo ten kod "zespół". Ten kod może być trudne do utrzymania, zwłaszcza w przypadku, gdy są zagnieżdżone zależności. Jest również trudne do testów jednostkowych.

Jednym rozwiązaniem jest do używania kontenera IoC. Kontenera IoC jest składnikiem oprogramowania, który jest odpowiedzialny za zarządzanie zależnościami. Rejestrowanie typów z kontenerem, a następnie użyć do tworzenia obiektów kontenera. Kontener automatycznie wpadł na pomysł relacji zależności. Wiele kontenerów IoC umożliwiają także kontrolowanie elementów, takich jak okres istnienia obiektów i zakresu.

> [!NOTE]
> "IoC" oznacza "Inwersja kontroli", który jest ogólny wzorzec gdzie struktura wywołuje kod aplikacji. Kontenera IoC tworzy obiekty, które zwykle przepływu sterowania "odwraca".


## <a name="using-ioc-containers-in-signalr"></a>Używanie kontenerów IoC w SignalR

Aplikacja rozmowy prawdopodobnie jest zbyt proste do korzystania z kontenera IoC. Zamiast tego, Przyjrzyjmy się [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) próbki.

Przykładowe StockTicker definiuje dwie klasy głównej:

- `StockTickerHub`: Klasa Centrum zarządza połączeniami klienta.
- `StockTicker`: Pojedyncze, który przechowuje giełdowych i okresowo aktualizuje je.

`StockTickerHub` zawiera odwołanie do `StockTicker` singleton, podczas gdy `StockTicker` zawiera odwołanie do **IHubConnectionContext** dla `StockTickerHub`. Ten interfejs używa do komunikowania się z `StockTickerHub` wystąpień. (Aby uzyskać więcej informacji, zobacz [emisje serwera z użyciem ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).)

Możemy użyć kontenera IoC związanych z porządkowaniem nieco te zależności. Najpierw Przyjrzyjmy uprościć `StockTickerHub` i `StockTicker` klasy. W poniższym kodzie I zostały oznaczone jako komentarz części, nie są nam potrzebne.

Usuń konstruktora bez parametrów z `StockTickerHub`. Zamiast tego należy zawsze używamy DI do Utwórz koncentrator.

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

W przypadku StockTicker należy usunąć pojedyncze wystąpienie. Później użyjemy kontenera IoC kontrolować okres istnienia StockTicker. Ponadto upublicznić konstruktora.

[!code-csharp[Main](dependency-injection/samples/sample10.cs?highlight=7)]

Następnie możemy refaktoryzować kod, tworząc interfejsu `StockTicker`. Użyjemy tego interfejsu do oddzielenia `StockTickerHub` z `StockTicker` klasy.

Program Visual Studio sprawia, że ten rodzaj refaktoryzacji proste. Otwórz plik StockTicker.cs, kliknij prawym przyciskiem myszy `StockTicker` deklaracji klasy, a następnie wybierz **Refaktoryzuj** ... **Wyodrębnij interfejs**.

![](dependency-injection/_static/image1.png)

W **Wyodrębnij interfejs** okno dialogowe, kliknij przycisk **Zaznacz wszystko**. Pozostaw inne wartości domyślne. Kliknij przycisk **OK**.

![](dependency-injection/_static/image2.png)

Program Visual Studio tworzy nowy interfejs o nazwie `IStockTicker`i zmienia także `StockTicker` wyprowadzenia z `IStockTicker`.

Otwórz plik IStockTicker.cs i zmień interfejs służący do **publicznych**.

[!code-csharp[Main](dependency-injection/samples/sample11.cs?highlight=1)]

W `StockTickerHub` klasy, należy zastąpić dwa wystąpienia `StockTicker` do `IStockTicker`:

[!code-csharp[Main](dependency-injection/samples/sample12.cs?highlight=4,6)]

Tworzenie `IStockTicker` interfejsu nie jest to niezbędne, ale chciałem pokazać, jak DI może pomóc zmniejszyć sprzężenia między składnikami aplikacji.

## <a name="add-the-ninject-library"></a>Dodaj bibliotekę Ninject

Istnieje wiele kontenerów IoC typu open source dla platformy .NET. Na potrzeby tego samouczka przy użyciu [Ninject](http://www.ninject.org/). (Obejmują innych popularnych bibliotek [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), i [StructureMap ](http://docs.structuremap.net).)

Menedżer pakietów NuGet Użyj, aby zainstalować [biblioteki Ninject](https://nuget.org/packages/Ninject/3.0.1.10). W programie Visual Studio z **narzędzia** menu wybierz opcję **Menedżera pakietów NuGet** > **Konsola Menedżera pakietów**. W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:

[!code-powershell[Main](dependency-injection/samples/sample13.ps1)]

## <a name="replace-the-signalr-dependency-resolver"></a>Zastąp mechanizmu rozpoznawania zależności biblioteki SignalR

Aby użyć Ninject w SignalR, należy utworzyć klasę, która pochodzi od klasy **DefaultDependencyResolver**.

[!code-csharp[Main](dependency-injection/samples/sample14.cs)]

Ta klasa zastępuje **GetService** i **funkcji GetServices** metody **DefaultDependencyResolver**. SignalR wywołuje te metody do tworzenia różnych obiektów w czasie wykonywania, w tym Centrum wystąpień, a także różne usługi używane wewnętrznie przez SignalR.

- **GetService** metody tworzy pojedyncze wystąpienie typu. Przesłonić tę metodę do wywołania jądra Ninject **TryGet** metody. Jeśli ta metoda zwraca wartość null, wrócić do domyślnego programu rozpoznawania nazw.
- **Funkcji GetServices** metoda tworzy kolekcję obiektów określonego typu. Przesłoń tę metodę, aby łączyć wyniki Ninject z wynikami domyślny mechanizm rozwiązywania konfliktów.

## <a name="configure-ninject-bindings"></a>Skonfiguruj Ninject powiązania

Teraz użyjemy Ninject do deklarowania typu powiązania.

Otwórz klasę Startup.cs aplikacji (albo utworzony ręcznie zgodnie z instrukcjami pakietu `readme.txt`, lub który został utworzony przez dodawanie uwierzytelniania do projektu). W `Startup.Configuration` metody, Tworzenie kontenera Ninject, która wywołuje Ninject *jądra*.

[!code-csharp[Main](dependency-injection/samples/sample15.cs)]

Utwórz wystąpienie obiektu nasz mechanizm rozpoznawania zależności niestandardowej:

[!code-csharp[Main](dependency-injection/samples/sample16.cs)]

Utwórz powiązanie dla `IStockTicker` w następujący sposób:

[!code-csharp[Main](dependency-injection/samples/sample17.cs)]

Ten kod jest informacją dwie rzeczy. Pierwszy, zawsze wtedy, gdy aplikacja potrzebuje `IStockTicker`, jądra należy utworzyć wystąpienie `StockTicker`. Drugi `StockTicker` klasa mają zostać utworzone jako pojedynczego obiektu. Ninject utworzyć jedno wystąpienie obiektu i zwracane to samo wystąpienie dla każdego żądania.

Utwórz powiązanie dla **IHubConnectionContext** w następujący sposób:

[!code-csharp[Main](dependency-injection/samples/sample18.cs)]

Ten kod tworzy funkcja anonimowa, która zwraca **IHubConnection**. **WhenInjectedInto** metoda informuje Ninject, aby użyć tej funkcji, tylko wtedy, gdy tworzenie `IStockTicker` wystąpień. Przyczyną jest to, że SignalR tworzy **IHubConnectionContext** wystąpień wewnętrznie i nie chcemy do zastąpienia, jak SignalR ich tworzenia. Ta funkcja ma zastosowanie tylko do naszych `StockTicker` klasy.

Mechanizm rozpoznawania zależności do przekazania **MapSignalR** metody, dodając konfiguracji Centrum:

[!code-csharp[Main](dependency-injection/samples/sample19.cs)]

Zaktualizuj metodę Startup.ConfigureSignalR w klasie uruchamiania przykładowej nowy parametr:

[!code-csharp[Main](dependency-injection/samples/sample20.cs)]

Teraz SignalR będzie używać programu rozpoznawania nazw określonych w **MapSignalR**, zamiast domyślny mechanizm rozwiązywania konfliktów.

Oto kompletny kod dla `Startup.Configuration`.

[!code-csharp[Main](dependency-injection/samples/sample21.cs)]

Aby uruchomić aplikację StockTicker w programie Visual Studio, naciśnij klawisz F5. W oknie przeglądarki przejdź do `http://localhost:*port*/SignalR.Sample/StockTicker.html`.

![](dependency-injection/_static/image3.png)

Aplikacja ma dokładnie taką samą funkcjonalność jak przed. (Aby uzyskać opis, zobacz [emisje serwera z użyciem ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).) Firma Microsoft nie zostały zmienione zachowanie; po prostu ułatwiono kod do testowania, obsługa i rozwijać.

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
# <a name="tutorial-server-broadcast-with-signalr-2"></a><span data-ttu-id="30f82-103">Samouczek: emisja serwera z sygnałem 2</span><span class="sxs-lookup"><span data-stu-id="30f82-103">Tutorial: Server broadcast with SignalR 2</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

<span data-ttu-id="30f82-104">W tym samouczku pokazano, jak utworzyć aplikację sieci Web korzystającą z ASP.NET sygnalizującego 2, aby zapewnić funkcję emisji serwera.</span><span class="sxs-lookup"><span data-stu-id="30f82-104">This tutorial shows how to create a web application that uses ASP.NET SignalR 2 to provide server broadcast functionality.</span></span> <span data-ttu-id="30f82-105">Emisja serwera oznacza, że serwer uruchamia komunikację wysyłaną do klientów.</span><span class="sxs-lookup"><span data-stu-id="30f82-105">Server broadcast means that the server starts the communications sent to clients.</span></span>

<span data-ttu-id="30f82-106">Aplikacja, którą utworzysz w tym samouczku, symuluje taktowanie giełdowe, typowy scenariusz dla funkcji emisji serwera.</span><span class="sxs-lookup"><span data-stu-id="30f82-106">The application that you'll create in this tutorial simulates a stock ticker, a typical scenario for server broadcast functionality.</span></span> <span data-ttu-id="30f82-107">Okresowo serwer losowo aktualizuje ceny akcji i emituje aktualizacje do wszystkich podłączonych klientów.</span><span class="sxs-lookup"><span data-stu-id="30f82-107">Periodically, the server randomly updates stock prices and broadcast the updates to all connected clients.</span></span> <span data-ttu-id="30f82-108">W przeglądarce numery i symbole w kolumnach **Zmień** i **%** zmieniają się dynamicznie w odpowiedzi na powiadomienia z serwera.</span><span class="sxs-lookup"><span data-stu-id="30f82-108">In the browser, the numbers and symbols in the **Change** and **%** columns dynamically change in response to notifications from the server.</span></span> <span data-ttu-id="30f82-109">Jeśli otworzysz dodatkowe przeglądarki pod tym samym adresem URL, wszystkie będą wyświetlać te same dane i te same zmiany w danych jednocześnie.</span><span class="sxs-lookup"><span data-stu-id="30f82-109">If you open additional browsers to the same URL, they all show the same data and the same changes to the data simultaneously.</span></span>

![Tworzenie sieci Web](tutorial-server-broadcast-with-signalr/_static/image1.png)

<span data-ttu-id="30f82-111">W tym samouczku zostaną wykonane następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="30f82-111">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="30f82-112">Tworzenie projektu</span><span class="sxs-lookup"><span data-stu-id="30f82-112">Create the project</span></span>
> * <span data-ttu-id="30f82-113">Konfigurowanie kodu serwera</span><span class="sxs-lookup"><span data-stu-id="30f82-113">Set up the server code</span></span>
> * <span data-ttu-id="30f82-114">Sprawdzanie kodu serwera</span><span class="sxs-lookup"><span data-stu-id="30f82-114">Examine the server code</span></span>
> * <span data-ttu-id="30f82-115">Konfigurowanie kodu klienta</span><span class="sxs-lookup"><span data-stu-id="30f82-115">Set up the client code</span></span>
> * <span data-ttu-id="30f82-116">Sprawdzanie kodu klienta</span><span class="sxs-lookup"><span data-stu-id="30f82-116">Examine the client code</span></span>
> * <span data-ttu-id="30f82-117">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="30f82-117">Test the application</span></span>
> * <span data-ttu-id="30f82-118">Włączanie rejestrowania</span><span class="sxs-lookup"><span data-stu-id="30f82-118">Enable logging</span></span>

> [!IMPORTANT]
> <span data-ttu-id="30f82-119">Jeśli nie chcesz wykonać kroków tworzenia aplikacji, możesz zainstalować program sygnalizujący. przykładowego pakietu w nowym pustym projekcie aplikacji sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="30f82-119">If you don't want to work through the steps of building the application, you can install the SignalR.Sample package in a new Empty ASP.NET Web Application project.</span></span> <span data-ttu-id="30f82-120">W przypadku zainstalowania pakietu NuGet bez wykonywania czynności opisanych w tym samouczku należy wykonać instrukcje zawarte w pliku *README. txt* .</span><span class="sxs-lookup"><span data-stu-id="30f82-120">If you install the NuGet package without performing the steps in this tutorial, you must follow the instructions in the *readme.txt* file.</span></span> <span data-ttu-id="30f82-121">Aby uruchomić pakiet, należy dodać klasę uruchomieniową OWIN, która wywołuje metodę `ConfigureSignalR` w zainstalowanym pakiecie.</span><span class="sxs-lookup"><span data-stu-id="30f82-121">To run the package you need to add an OWIN startup class which calls the `ConfigureSignalR` method in the installed package.</span></span> <span data-ttu-id="30f82-122">Jeśli nie dodasz klasy uruchomieniowej OWIN, zostanie wyświetlony komunikat o błędzie.</span><span class="sxs-lookup"><span data-stu-id="30f82-122">You will receive an error if you do not add the OWIN startup class.</span></span> <span data-ttu-id="30f82-123">Zobacz sekcję [Instalowanie przykładu StockTicker](#install-the-stockticker-sample) w tym artykule.</span><span class="sxs-lookup"><span data-stu-id="30f82-123">See the [Install the StockTicker sample](#install-the-stockticker-sample) section of this article.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="30f82-124">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="30f82-124">Prerequisites</span></span>

* <span data-ttu-id="30f82-125">Program [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) z pakietem roboczym **Tworzenie aplikacji na platformie ASP.NET i aplikacji internetowych**.</span><span class="sxs-lookup"><span data-stu-id="30f82-125">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) with the **ASP.NET and web development** workload.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="30f82-126">Tworzenie projektu</span><span class="sxs-lookup"><span data-stu-id="30f82-126">Create the project</span></span>

<span data-ttu-id="30f82-127">W tej sekcji pokazano, jak utworzyć pustą aplikację sieci Web ASP.NET za pomocą programu Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="30f82-127">This section shows how to use Visual Studio 2017 to create an empty ASP.NET Web Application.</span></span>

1. <span data-ttu-id="30f82-128">W programie Visual Studio Utwórz aplikację sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="30f82-128">In Visual Studio, create an ASP.NET Web Application.</span></span>

    ![Tworzenie sieci Web](tutorial-server-broadcast-with-signalr/_static/image2.png)

1. <span data-ttu-id="30f82-130">W oknie **nowy ASP.NET Web Application-sygnalizującer. StockTicker** pozostaw **puste** zaznaczone i wybierz **przycisk OK**.</span><span class="sxs-lookup"><span data-stu-id="30f82-130">In the **New ASP.NET Web Application - SignalR.StockTicker** window, leave **Empty** selected and select **OK**.</span></span>

## <a name="set-up-the-server-code"></a><span data-ttu-id="30f82-131">Konfigurowanie kodu serwera</span><span class="sxs-lookup"><span data-stu-id="30f82-131">Set up the server code</span></span>

<span data-ttu-id="30f82-132">W tej sekcji skonfigurujesz kod, który jest uruchamiany na serwerze.</span><span class="sxs-lookup"><span data-stu-id="30f82-132">In this section, you set up the code that runs on the server.</span></span>

### <a name="create-the-stock-class"></a><span data-ttu-id="30f82-133">Tworzenie klasy giełdowej</span><span class="sxs-lookup"><span data-stu-id="30f82-133">Create the Stock class</span></span>

<span data-ttu-id="30f82-134">Zacznij od utworzenia klasy modelu *giełdowego* , która będzie używana do przechowywania i przesyłania informacji o magazynie.</span><span class="sxs-lookup"><span data-stu-id="30f82-134">You begin by creating the *Stock* model class that you'll use to store and transmit information about a stock.</span></span>

1. <span data-ttu-id="30f82-135">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt i wybierz polecenie Dodaj **klasę** > .</span><span class="sxs-lookup"><span data-stu-id="30f82-135">In **Solution Explorer**, right-click the project and select **Add** > **Class**.</span></span>

1. <span data-ttu-id="30f82-136">Nadaj *klasie nazwę* i Dodaj ją do projektu.</span><span class="sxs-lookup"><span data-stu-id="30f82-136">Name the class *Stock* and add it to the project.</span></span>

1. <span data-ttu-id="30f82-137">Zastąp kod w pliku *Stock.cs* tym kodem:</span><span class="sxs-lookup"><span data-stu-id="30f82-137">Replace the code in the *Stock.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample1.cs)]

    <span data-ttu-id="30f82-138">Dwie właściwości, które zostaną ustawione podczas tworzenia zasobów, są `Symbol` (na przykład MSFT dla firmy Microsoft) i `Price`.</span><span class="sxs-lookup"><span data-stu-id="30f82-138">The two properties that you'll set when you create stocks are `Symbol` (for example, MSFT for Microsoft) and `Price`.</span></span> <span data-ttu-id="30f82-139">Inne właściwości zależą od tego, jak i kiedy ustawiasz `Price`.</span><span class="sxs-lookup"><span data-stu-id="30f82-139">The other properties depend on how and when you set `Price`.</span></span> <span data-ttu-id="30f82-140">Przy pierwszym ustawianiu `Price`wartość jest przekazywana do `DayOpen`.</span><span class="sxs-lookup"><span data-stu-id="30f82-140">The first time you set `Price`, the value gets propagated to `DayOpen`.</span></span> <span data-ttu-id="30f82-141">Po ustawieniu `Price`aplikacja oblicza wartości właściwości `Change` i `PercentChange` na podstawie różnicy między `Price` i `DayOpen`.</span><span class="sxs-lookup"><span data-stu-id="30f82-141">After that, when you set `Price`, the app calculates the `Change` and `PercentChange` property values based on the difference between `Price` and `DayOpen`.</span></span>

### <a name="create-the-stocktickerhub-and-stockticker-classes"></a><span data-ttu-id="30f82-142">Tworzenie klas StockTickerHub i StockTicker</span><span class="sxs-lookup"><span data-stu-id="30f82-142">Create the StockTickerHub and StockTicker classes</span></span>

<span data-ttu-id="30f82-143">Użyjesz interfejsu API centrum sygnału do obsługi interakcji między serwerem a klientem.</span><span class="sxs-lookup"><span data-stu-id="30f82-143">You'll use the SignalR Hub API to handle server-to-client interaction.</span></span> <span data-ttu-id="30f82-144">Klasa `StockTickerHub`, która pochodzi od klasy sygnalizującej `Hub`, będzie obsługiwać odbierające połączenia i wywołania metod od klientów.</span><span class="sxs-lookup"><span data-stu-id="30f82-144">A `StockTickerHub` class that derives from the SignalR `Hub` class will handle receiving connections and method calls from clients.</span></span> <span data-ttu-id="30f82-145">Należy również zachować dane giełdowe i uruchomić `Timer` obiektu.</span><span class="sxs-lookup"><span data-stu-id="30f82-145">You also need to maintain stock data and run a `Timer` object.</span></span> <span data-ttu-id="30f82-146">Obiekt `Timer` będzie okresowo wyzwalać aktualizacje cen niezależnie od połączeń klientów.</span><span class="sxs-lookup"><span data-stu-id="30f82-146">The `Timer` object will periodically trigger price updates independent of client connections.</span></span> <span data-ttu-id="30f82-147">Nie można umieścić tych funkcji w klasie `Hub`, ponieważ centra są przejściowe.</span><span class="sxs-lookup"><span data-stu-id="30f82-147">You can't put these functions in a `Hub` class, because Hubs are transient.</span></span> <span data-ttu-id="30f82-148">Aplikacja tworzy wystąpienie klasy `Hub` dla każdego zadania w centrum, takie jak połączenia i wywołania z klienta do serwera.</span><span class="sxs-lookup"><span data-stu-id="30f82-148">The app creates a `Hub` class instance for each task on the hub, like connections and calls from the client to the server.</span></span> <span data-ttu-id="30f82-149">Dlatego mechanizm, który przechowuje dane giełdowe, aktualizuje ceny i emituje aktualizacje cen, musi działać w oddzielnym klasie.</span><span class="sxs-lookup"><span data-stu-id="30f82-149">So the mechanism that keeps stock data, updates prices, and broadcasts the price updates has to run in a separate class.</span></span> <span data-ttu-id="30f82-150">Nazwa klasy `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="30f82-150">You'll name the class `StockTicker`.</span></span>

![Emitowanie z StockTicker](tutorial-server-broadcast-with-signalr/_static/image3.png)

<span data-ttu-id="30f82-152">Tylko jedno wystąpienie klasy `StockTicker` może być uruchomione na serwerze, dlatego należy skonfigurować odwołanie z każdego wystąpienia `StockTickerHub` do wystąpienia pojedynczego `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="30f82-152">You only want one instance of the `StockTicker` class to run on the server, so you'll need to set up a reference from each `StockTickerHub` instance to the singleton `StockTicker` instance.</span></span> <span data-ttu-id="30f82-153">Klasa `StockTicker` musi emitować do klientów, ponieważ ma dane podstawowe i wyzwala aktualizacje, ale `StockTicker` nie jest klasą `Hub`.</span><span class="sxs-lookup"><span data-stu-id="30f82-153">The `StockTicker` class has to broadcast to clients because it has the stock data and triggers updates, but `StockTicker` isn't a `Hub` class.</span></span> <span data-ttu-id="30f82-154">Klasa `StockTicker` musi uzyskać odwołanie do obiektu kontekstu połączenia centrum sygnału.</span><span class="sxs-lookup"><span data-stu-id="30f82-154">The `StockTicker` class has to get a reference to the SignalR Hub connection context object.</span></span> <span data-ttu-id="30f82-155">Następnie może użyć obiektu kontekstu połączenia sygnalizującego do emisji do klientów.</span><span class="sxs-lookup"><span data-stu-id="30f82-155">It can then use the SignalR connection context object to broadcast to clients.</span></span>

#### <a name="create-stocktickerhubcs"></a><span data-ttu-id="30f82-156">Create StockTickerHub.cs</span><span class="sxs-lookup"><span data-stu-id="30f82-156">Create StockTickerHub.cs</span></span>

1. <span data-ttu-id="30f82-157">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt, a następnie wybierz pozycję **Dodaj** > **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="30f82-157">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="30f82-158">W obszarze **Dodaj nowy element — sygnalizującer. StockTicker**wybierz **pozycję zainstalowane** > **Visual C#**  > **sieci Web** > **sygnalizujący** , a następnie wybierz pozycję **Klasa centrum sygnałów (v2)** .</span><span class="sxs-lookup"><span data-stu-id="30f82-158">In **Add New Item - SignalR.StockTicker**, select **Installed** > **Visual C#** > **Web** > **SignalR**  and then select **SignalR Hub Class (v2)**.</span></span>

1. <span data-ttu-id="30f82-159">Nadaj klasie nazwę *StockTickerHub* i Dodaj ją do projektu.</span><span class="sxs-lookup"><span data-stu-id="30f82-159">Name the class *StockTickerHub* and add it to the project.</span></span>

    <span data-ttu-id="30f82-160">Ten krok powoduje utworzenie pliku klasy *StockTickerHub.cs* .</span><span class="sxs-lookup"><span data-stu-id="30f82-160">This step creates the *StockTickerHub.cs* class file.</span></span> <span data-ttu-id="30f82-161">Jednocześnie dodaje zestaw plików skryptów i odwołań do zestawów, które obsługują program sygnalizujący do projektu.</span><span class="sxs-lookup"><span data-stu-id="30f82-161">Simultaneously, it adds a set of script files and assembly references that supports SignalR to the project.</span></span>

1. <span data-ttu-id="30f82-162">Zastąp kod w pliku *StockTickerHub.cs* tym kodem:</span><span class="sxs-lookup"><span data-stu-id="30f82-162">Replace the code in the *StockTickerHub.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample2.cs)]

1. <span data-ttu-id="30f82-163">Zapisz plik.</span><span class="sxs-lookup"><span data-stu-id="30f82-163">Save the file.</span></span>

<span data-ttu-id="30f82-164">Aplikacja używa klasy [Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx) do definiowania metod, które klienci mogą wywoływać na serwerze.</span><span class="sxs-lookup"><span data-stu-id="30f82-164">The app uses the [Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx) class to define methods the clients can call on the server.</span></span> <span data-ttu-id="30f82-165">Definiujesz jedną metodę: `GetAllStocks()`.</span><span class="sxs-lookup"><span data-stu-id="30f82-165">You're defining one method: `GetAllStocks()`.</span></span> <span data-ttu-id="30f82-166">Gdy klient początkowo nawiązuje połączenie z serwerem, wywoła tę metodę, aby uzyskać listę wszystkich zasobów z ich bieżącymi cenami.</span><span class="sxs-lookup"><span data-stu-id="30f82-166">When a client initially connects to the server, it will call this method to get a list of all of the stocks with their current prices.</span></span> <span data-ttu-id="30f82-167">Metoda może być uruchomiona synchronicznie i zwracać `IEnumerable<Stock>`, ponieważ zwraca dane z pamięci.</span><span class="sxs-lookup"><span data-stu-id="30f82-167">The method can run synchronously and return `IEnumerable<Stock>` because it's returning data from memory.</span></span>

<span data-ttu-id="30f82-168">Jeśli metoda musiała pobrać dane, wykonując coś, co będzie wymagało oczekiwania, takiego jak wyszukiwanie bazy danych lub wywołanie usługi sieci Web, należy określić `Task<IEnumerable<Stock>>` jako wartość zwrotną, aby włączyć asynchroniczne przetwarzanie.</span><span class="sxs-lookup"><span data-stu-id="30f82-168">If the method had to get the data by doing something that would involve waiting, like a database lookup or a web service call, you would specify `Task<IEnumerable<Stock>>` as the return value to enable asynchronous processing.</span></span> <span data-ttu-id="30f82-169">Aby uzyskać więcej informacji, zobacz [Podręcznik interfejsu API centrów ASP.NETer — serwer — Kiedy należy wykonać asynchronicznie](../guide-to-the-api/hubs-api-guide-server.md#asyncmethods).</span><span class="sxs-lookup"><span data-stu-id="30f82-169">For more information, see [ASP.NET SignalR Hubs API Guide - Server - When to execute asynchronously](../guide-to-the-api/hubs-api-guide-server.md#asyncmethods).</span></span>

<span data-ttu-id="30f82-170">Atrybut `HubName` określa, w jaki sposób aplikacja będzie odwoływać się do centrum w kodzie JavaScript na kliencie.</span><span class="sxs-lookup"><span data-stu-id="30f82-170">The `HubName` attribute specifies how the app will reference the Hub in JavaScript code on the client.</span></span> <span data-ttu-id="30f82-171">Nazwa domyślna na kliencie, jeśli ten atrybut nie jest używany, to camelCase wersja klasy, która w tym przypadku byłaby `stockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="30f82-171">The default name on the client if you don't use this attribute, is a camelCase version of the class name, which in this case would be `stockTickerHub`.</span></span>

<span data-ttu-id="30f82-172">Jak zobaczysz później podczas tworzenia klasy `StockTicker`, aplikacja utworzy pojedyncze wystąpienie tej klasy w swojej statycznej `Instance` właściwości.</span><span class="sxs-lookup"><span data-stu-id="30f82-172">As you'll see later when you create the `StockTicker` class, the app creates a singleton instance of that class in its static `Instance` property.</span></span> <span data-ttu-id="30f82-173">To pojedyncze wystąpienie `StockTicker` znajduje się w pamięci, niezależnie od tego, ile klientów nawiązuje połączenie lub rozłączanie.</span><span class="sxs-lookup"><span data-stu-id="30f82-173">That singleton instance of `StockTicker` is in memory no matter how many clients connect or disconnect.</span></span> <span data-ttu-id="30f82-174">To wystąpienie jest używane przez metodę `GetAllStocks()` do zwracania bieżących informacji o zapasach.</span><span class="sxs-lookup"><span data-stu-id="30f82-174">That instance is what the `GetAllStocks()` method uses to return current stock information.</span></span>

#### <a name="create-stocktickercs"></a><span data-ttu-id="30f82-175">Utwórz StockTicker.cs</span><span class="sxs-lookup"><span data-stu-id="30f82-175">Create StockTicker.cs</span></span>

1. <span data-ttu-id="30f82-176">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt i wybierz polecenie Dodaj **klasę** > .</span><span class="sxs-lookup"><span data-stu-id="30f82-176">In **Solution Explorer**, right-click the project and select **Add** > **Class**.</span></span>

1. <span data-ttu-id="30f82-177">Nadaj klasie nazwę *StockTicker* i Dodaj ją do projektu.</span><span class="sxs-lookup"><span data-stu-id="30f82-177">Name the class *StockTicker* and add it to the project.</span></span>

1. <span data-ttu-id="30f82-178">Zastąp kod w pliku *StockTicker.cs* tym kodem:</span><span class="sxs-lookup"><span data-stu-id="30f82-178">Replace the code in the *StockTicker.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample3.cs)]

<span data-ttu-id="30f82-179">Ponieważ wszystkie wątki będą działać w tym samym wystąpieniu kodu StockTicker, Klasa StockTicker musi być bezpieczna wątkowo.</span><span class="sxs-lookup"><span data-stu-id="30f82-179">Since all threads will be running the same instance of StockTicker code, the StockTicker class has to be thread-safe.</span></span>

### <a name="examine-the-server-code"></a><span data-ttu-id="30f82-180">Sprawdzanie kodu serwera</span><span class="sxs-lookup"><span data-stu-id="30f82-180">Examine the server code</span></span>

<span data-ttu-id="30f82-181">Jeśli sprawdzisz kod serwera, pomoże Ci zrozumieć, jak działa aplikacja.</span><span class="sxs-lookup"><span data-stu-id="30f82-181">If you examine the server code, it will help you understand how the app works.</span></span>

#### <a name="storing-the-singleton-instance-in-a-static-field"></a><span data-ttu-id="30f82-182">Przechowywanie pojedynczego wystąpienia w polu statycznym</span><span class="sxs-lookup"><span data-stu-id="30f82-182">Storing the singleton instance in a static field</span></span>

<span data-ttu-id="30f82-183">Kod inicjuje statyczne `_instance` pole, które wykonuje kopię zapasową właściwości `Instance` z wystąpieniem klasy.</span><span class="sxs-lookup"><span data-stu-id="30f82-183">The code initializes the static `_instance` field that backs the `Instance` property with an instance of the class.</span></span> <span data-ttu-id="30f82-184">Ponieważ Konstruktor jest prywatny, jest to jedyne wystąpienie klasy, które może utworzyć aplikacja.</span><span class="sxs-lookup"><span data-stu-id="30f82-184">Because the constructor is private, it's the only instance of the class that the app can create.</span></span> <span data-ttu-id="30f82-185">Aplikacja używa [inicjalizacji z opóźnieniem](/dotnet/framework/performance/lazy-initialization) dla pola `_instance`.</span><span class="sxs-lookup"><span data-stu-id="30f82-185">The app uses [Lazy initialization](/dotnet/framework/performance/lazy-initialization) for the `_instance` field.</span></span> <span data-ttu-id="30f82-186">Nie jest ze względu na wydajność.</span><span class="sxs-lookup"><span data-stu-id="30f82-186">It's not for performance reasons.</span></span> <span data-ttu-id="30f82-187">Należy upewnić się, że tworzenie wystąpienia jest bezpieczne wątkowo.</span><span class="sxs-lookup"><span data-stu-id="30f82-187">It's to make sure the instance creation is thread-safe.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample4.cs)]

<span data-ttu-id="30f82-188">Za każdym razem, gdy klient nawiązuje połączenie z serwerem, nowe wystąpienie klasy StockTickerHub uruchomione w osobnym wątku Pobiera pojedyncze wystąpienie StockTicker z właściwości statycznej `StockTicker.Instance`, jak pokazano wcześniej w klasie `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="30f82-188">Each time a client connects to the server, a new instance of the StockTickerHub class running in a separate thread gets the StockTicker singleton instance from the `StockTicker.Instance` static property, as you saw earlier in the `StockTickerHub` class.</span></span>

#### <a name="storing-stock-data-in-a-concurrentdictionary"></a><span data-ttu-id="30f82-189">Przechowywanie danych giełdowych w ConcurrentDictionary</span><span class="sxs-lookup"><span data-stu-id="30f82-189">Storing stock data in a ConcurrentDictionary</span></span>

<span data-ttu-id="30f82-190">Konstruktor inicjuje kolekcję `_stocks` z niektórymi przykładowymi danymi zapasowymi, a `GetAllStocks` zwraca te zasoby.</span><span class="sxs-lookup"><span data-stu-id="30f82-190">The constructor initializes the `_stocks` collection with some sample stock data, and `GetAllStocks` returns the stocks.</span></span> <span data-ttu-id="30f82-191">Jak widać wcześniej, ta kolekcja zapasów jest zwracana przez `StockTickerHub.GetAllStocks`, czyli metodę serwera w klasie `Hub`, którą klienci mogą wywoływać.</span><span class="sxs-lookup"><span data-stu-id="30f82-191">As you saw earlier, this collection of stocks is returned by `StockTickerHub.GetAllStocks`, which is a server method in the `Hub` class that clients can call.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample5.cs)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample6.cs)]

<span data-ttu-id="30f82-192">Kolekcja magazynów jest definiowana jako typ [ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx) dla bezpieczeństwa wątków.</span><span class="sxs-lookup"><span data-stu-id="30f82-192">The stocks collection is defined as a [ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx) type for thread safety.</span></span> <span data-ttu-id="30f82-193">Alternatywnie, można użyć obiektu [dictionary](https://msdn.microsoft.com/library/xfhwa508.aspx) i jawnie zablokować słownik po wprowadzeniu w nim zmian.</span><span class="sxs-lookup"><span data-stu-id="30f82-193">As an alternative, you could use a [Dictionary](https://msdn.microsoft.com/library/xfhwa508.aspx) object and explicitly lock the dictionary when you make changes to it.</span></span>

<span data-ttu-id="30f82-194">W przypadku tej aplikacji przykładowej można przechowywać dane aplikacji w pamięci i utracić dane, gdy aplikacja zostanie oddysponowana wystąpieniem `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="30f82-194">For this sample application, it's OK to store application data in memory and to lose the data when the app disposes of the  `StockTicker` instance.</span></span> <span data-ttu-id="30f82-195">W rzeczywistej aplikacji można korzystać z magazynu danych zaplecza, takiego jak baza danych.</span><span class="sxs-lookup"><span data-stu-id="30f82-195">In a real application, you would work with a back-end data store like a database.</span></span>

#### <a name="periodically-updating-stock-prices"></a><span data-ttu-id="30f82-196">Okresowe aktualizowanie cen akcji</span><span class="sxs-lookup"><span data-stu-id="30f82-196">Periodically updating stock prices</span></span>

<span data-ttu-id="30f82-197">Konstruktor uruchamia obiekt `Timer`, który okresowo wywołuje metody, które codziennie aktualizują ceny giełdowe.</span><span class="sxs-lookup"><span data-stu-id="30f82-197">The constructor starts up a `Timer` object that periodically calls methods that update stock prices on a random basis.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample7.cs)]

 <span data-ttu-id="30f82-198">`Timer` wywołań `UpdateStockPrices`, które są przekazywane do wartości null w parametrze stanu.</span><span class="sxs-lookup"><span data-stu-id="30f82-198">`Timer` calls `UpdateStockPrices`, which passes in null in the state parameter.</span></span> <span data-ttu-id="30f82-199">Przed aktualizacją cen aplikacja wykonuje blokadę na obiekcie `_updateStockPricesLock`.</span><span class="sxs-lookup"><span data-stu-id="30f82-199">Before updating prices, the app takes a lock on the `_updateStockPricesLock` object.</span></span> <span data-ttu-id="30f82-200">Kod sprawdza, czy inny wątek już aktualizuje ceny, a następnie wywołuje `TryUpdateStockPrice` na każdym spisie na liście.</span><span class="sxs-lookup"><span data-stu-id="30f82-200">The code checks if another thread is already updating prices, and then it calls `TryUpdateStockPrice` on each stock in the list.</span></span> <span data-ttu-id="30f82-201">Metoda `TryUpdateStockPrice` decyduje o tym, czy należy zmienić cenę giełdową, i ile jej zmienić.</span><span class="sxs-lookup"><span data-stu-id="30f82-201">The `TryUpdateStockPrice` method decides whether to change the stock price, and how much to change it.</span></span> <span data-ttu-id="30f82-202">W przypadku zmiany ceny giełdowej aplikacja wywoła `BroadcastStockPrice`, aby emitować zmianę cen giełdowych do wszystkich połączonych klientów.</span><span class="sxs-lookup"><span data-stu-id="30f82-202">If the stock price changes, the app calls `BroadcastStockPrice` to broadcast the stock price change to all connected clients.</span></span>

<span data-ttu-id="30f82-203">Flaga `_updatingStockPrices` oznaczona jako [nietrwała](https://msdn.microsoft.com/library/x13ttww7.aspx) , aby upewnić się, że jest bezpieczna wątkowo.</span><span class="sxs-lookup"><span data-stu-id="30f82-203">The `_updatingStockPrices` flag designated [volatile](https://msdn.microsoft.com/library/x13ttww7.aspx) to make sure it is thread-safe.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample8.cs)]

<span data-ttu-id="30f82-204">W rzeczywistej aplikacji Metoda `TryUpdateStockPrice` wywoła usługę sieci Web w celu wyszukania ceny.</span><span class="sxs-lookup"><span data-stu-id="30f82-204">In a real application, the `TryUpdateStockPrice` method would call a web service to look up the price.</span></span> <span data-ttu-id="30f82-205">W tym kodzie aplikacja używa generatora liczb losowych, aby wprowadzać zmiany losowo.</span><span class="sxs-lookup"><span data-stu-id="30f82-205">In this code, the app uses a random number generator to make changes randomly.</span></span>

#### <a name="getting-the-signalr-context-so-that-the-stockticker-class-can-broadcast-to-clients"></a><span data-ttu-id="30f82-206">Pobieranie kontekstu sygnalizującego, aby Klasa StockTicker mogła emitować się do klientów</span><span class="sxs-lookup"><span data-stu-id="30f82-206">Getting the SignalR context so that the StockTicker class can broadcast to clients</span></span>

<span data-ttu-id="30f82-207">Ponieważ zmiany cen pochodzą z tego miejsca w obiekcie `StockTicker`, jest to obiekt, który musi wywołać metodę `updateStockPrice` na wszystkich połączonych klientach.</span><span class="sxs-lookup"><span data-stu-id="30f82-207">Because the price changes originate here in the `StockTicker` object, it's the object that needs to call an `updateStockPrice` method on all connected clients.</span></span> <span data-ttu-id="30f82-208">W klasie `Hub` masz interfejs API do wywoływania metod klienta, ale `StockTicker` nie pochodzi od klasy `Hub` i nie zawiera odwołania do żadnego obiektu `Hub`.</span><span class="sxs-lookup"><span data-stu-id="30f82-208">In a `Hub` class, you have an API for calling client methods, but `StockTicker` doesn't derive from the `Hub` class and doesn't have a reference to any `Hub` object.</span></span> <span data-ttu-id="30f82-209">Aby emitować do podłączonych klientów, Klasa `StockTicker` musi uzyskać wystąpienie kontekstu sygnalizującego dla klasy `StockTickerHub` i używać go do wywoływania metod na klientach.</span><span class="sxs-lookup"><span data-stu-id="30f82-209">To broadcast to connected clients, the `StockTicker` class has to get the SignalR context instance for the `StockTickerHub` class and use that to call methods on clients.</span></span>

<span data-ttu-id="30f82-210">Kod pobiera odwołanie do kontekstu sygnalizującego, gdy tworzy wystąpienie klasy pojedynczej, przekazuje odwołanie do konstruktora, a Konstruktor umieszcza go we właściwości `Clients`.</span><span class="sxs-lookup"><span data-stu-id="30f82-210">The code gets a reference to the SignalR context when it creates the singleton class instance, passes that reference to the constructor, and the constructor puts it in the `Clients` property.</span></span>

<span data-ttu-id="30f82-211">Istnieją dwa powody, dla których chcesz uzyskać kontekst tylko raz: uzyskanie kontekstu jest kosztownym zadaniem i wprowadzenie go, aby zachować zaplanowaną kolejność komunikatów wysyłanych do klientów.</span><span class="sxs-lookup"><span data-stu-id="30f82-211">There are two reasons why you want to get the context only once: getting the context is an expensive task, and getting it once makes sure the app preserves the intended order of messages sent to the clients.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample9.cs)]

<span data-ttu-id="30f82-212">Pobranie właściwości `Clients` kontekstu i umieszczenie jej we właściwości `StockTickerClient` umożliwia pisanie kodu w celu wywołania metod klienta, które wyglądają tak samo jak w klasie `Hub`.</span><span class="sxs-lookup"><span data-stu-id="30f82-212">Getting the `Clients` property of the context and putting it in the `StockTickerClient` property lets you write code to call client methods that looks the same as it would in a `Hub` class.</span></span> <span data-ttu-id="30f82-213">Na przykład do emisji do wszystkich klientów można pisać `Clients.All.updateStockPrice(stock)`.</span><span class="sxs-lookup"><span data-stu-id="30f82-213">For instance, to broadcast to all clients you can write `Clients.All.updateStockPrice(stock)`.</span></span>

<span data-ttu-id="30f82-214">Metoda `updateStockPrice` wywoływana w `BroadcastStockPrice` nie istnieje jeszcze.</span><span class="sxs-lookup"><span data-stu-id="30f82-214">The `updateStockPrice` method that you're calling in `BroadcastStockPrice` doesn't exist yet.</span></span> <span data-ttu-id="30f82-215">Dodasz go później podczas pisania kodu, który jest uruchamiany na kliencie.</span><span class="sxs-lookup"><span data-stu-id="30f82-215">You'll add it later when you write code that runs on the client.</span></span> <span data-ttu-id="30f82-216">Możesz odwołać się do `updateStockPrice` tutaj, ponieważ `Clients.All` jest dynamiczna, co oznacza, że aplikacja będzie szacować wyrażenie w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="30f82-216">You can refer to `updateStockPrice` here because `Clients.All` is dynamic, which means the app will evaluate the expression at runtime.</span></span> <span data-ttu-id="30f82-217">Gdy to wywołanie metody jest wykonywane, sygnalizujący wyśle do klienta nazwę metody i wartość parametru, a jeśli klient ma metodę o nazwie `updateStockPrice`, aplikacja wywoła tę metodę i przekaże do niej wartość parametru.</span><span class="sxs-lookup"><span data-stu-id="30f82-217">When this method call executes, SignalR will send the method name and the parameter value to the client, and if the client has a method named `updateStockPrice`, the app will call that method and pass the parameter value to it.</span></span>

<span data-ttu-id="30f82-218">`Clients.All` oznacza wysyłanie do wszystkich klientów.</span><span class="sxs-lookup"><span data-stu-id="30f82-218">`Clients.All` means send to all clients.</span></span> <span data-ttu-id="30f82-219">Sygnalizujący oferuje inne opcje umożliwiające określenie klientów lub grup klientów, do których mają być wysyłane.</span><span class="sxs-lookup"><span data-stu-id="30f82-219">SignalR gives you other options to specify which clients or groups of clients to send to.</span></span> <span data-ttu-id="30f82-220">Aby uzyskać więcej informacji, zobacz [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="30f82-220">For more information, see [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx).</span></span>

### <a name="register-the-signalr-route"></a><span data-ttu-id="30f82-221">Rejestrowanie trasy sygnalizującej</span><span class="sxs-lookup"><span data-stu-id="30f82-221">Register the SignalR route</span></span>

<span data-ttu-id="30f82-222">Serwer musi wiedzieć, który adres URL przechwycić i bezpośrednio do sygnalizującego.</span><span class="sxs-lookup"><span data-stu-id="30f82-222">The server needs to know which URL to intercept and direct to SignalR.</span></span> <span data-ttu-id="30f82-223">Aby to zrobić, Dodaj klasę uruchomieniową OWIN:</span><span class="sxs-lookup"><span data-stu-id="30f82-223">To do that, add an OWIN startup class:</span></span>

1. <span data-ttu-id="30f82-224">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt, a następnie wybierz pozycję **Dodaj** > **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="30f82-224">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="30f82-225">W obszarze **Dodaj nowy element — sygnalizujący. StockTicker** wybierz pozycję **zainstalowane** > **Visual C#**  > **Web** , a następnie wybierz pozycję **Owin klasy startowej**.</span><span class="sxs-lookup"><span data-stu-id="30f82-225">In **Add New Item - SignalR.StockTicker** select **Installed** > **Visual C#** > **Web** and then select **OWIN Startup Class**.</span></span>

1. <span data-ttu-id="30f82-226">Nazwij klasę *uruchamiania* i wybierz **przycisk OK**.</span><span class="sxs-lookup"><span data-stu-id="30f82-226">Name the class *Startup* and select **OK**.</span></span>

1. <span data-ttu-id="30f82-227">Zastąp domyślny kod w pliku *Startup.cs* tym kodem:</span><span class="sxs-lookup"><span data-stu-id="30f82-227">Replace the default code in the *Startup.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample10.cs)]

<span data-ttu-id="30f82-228">Konfiguracja kodu serwera została zakończona.</span><span class="sxs-lookup"><span data-stu-id="30f82-228">You have now finished setting up the server code.</span></span> <span data-ttu-id="30f82-229">W następnej sekcji zostanie skonfigurowany klient.</span><span class="sxs-lookup"><span data-stu-id="30f82-229">In the next section, you'll set up the client.</span></span>

## <a name="set-up-the-client-code"></a><span data-ttu-id="30f82-230">Konfigurowanie kodu klienta</span><span class="sxs-lookup"><span data-stu-id="30f82-230">Set up the client code</span></span>

<span data-ttu-id="30f82-231">W tej sekcji skonfigurujesz kod, który jest uruchamiany na kliencie programu.</span><span class="sxs-lookup"><span data-stu-id="30f82-231">In this section, you set up the code that runs on the client.</span></span>

### <a name="create-the-html-page-and-javascript-file"></a><span data-ttu-id="30f82-232">Tworzenie strony HTML i pliku JavaScript</span><span class="sxs-lookup"><span data-stu-id="30f82-232">Create the HTML page and JavaScript file</span></span>

<span data-ttu-id="30f82-233">Na stronie HTML zostaną wyświetlone dane, a plik JavaScript będzie organizować dane.</span><span class="sxs-lookup"><span data-stu-id="30f82-233">The HTML page will display the data and the JavaScript file will organize the data.</span></span>

#### <a name="create-stocktickerhtml"></a><span data-ttu-id="30f82-234">Create StockTicker.html</span><span class="sxs-lookup"><span data-stu-id="30f82-234">Create StockTicker.html</span></span>

<span data-ttu-id="30f82-235">Najpierw należy dodać klienta HTML.</span><span class="sxs-lookup"><span data-stu-id="30f82-235">First, you'll add the HTML client.</span></span>

1. <span data-ttu-id="30f82-236">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt i wybierz polecenie **Dodaj** > **stronę HTML**.</span><span class="sxs-lookup"><span data-stu-id="30f82-236">In **Solution Explorer**, right-click the project and select **Add** > **HTML Page**.</span></span>

1. <span data-ttu-id="30f82-237">Nazwij plik *StockTicker* i wybierz **przycisk OK**.</span><span class="sxs-lookup"><span data-stu-id="30f82-237">Name the file *StockTicker* and select **OK**.</span></span>

1. <span data-ttu-id="30f82-238">Zastąp domyślny kod w pliku *StockTicker. html* tym kodem:</span><span class="sxs-lookup"><span data-stu-id="30f82-238">Replace the default code in the *StockTicker.html* file with this code:</span></span>

    [!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample11.html?highlight=40-43)]

    <span data-ttu-id="30f82-239">KOD HTML tworzy tabelę z pięcioma kolumnami, wierszem nagłówka i wierszem danych z pojedynczą komórką obejmującą wszystkie pięć kolumn.</span><span class="sxs-lookup"><span data-stu-id="30f82-239">The HTML creates a table with five columns, a header row, and a data row with a single cell that spans all five columns.</span></span> <span data-ttu-id="30f82-240">W wierszu danych zostanie wyświetlona wartość "ładowanie..." chwilę po uruchomieniu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="30f82-240">The data row shows "loading..." momentarily when the app starts.</span></span> <span data-ttu-id="30f82-241">Kod JavaScript spowoduje usunięcie tego wiersza i dodanie go do swoich wierszy z danymi podstawowymi pobranymi z serwera.</span><span class="sxs-lookup"><span data-stu-id="30f82-241">JavaScript code will remove that row and add in its place rows with stock data retrieved from the server.</span></span>

    <span data-ttu-id="30f82-242">Tagi skryptu określają:</span><span class="sxs-lookup"><span data-stu-id="30f82-242">The script tags specify:</span></span>

    * <span data-ttu-id="30f82-243">Plik skryptu jQuery.</span><span class="sxs-lookup"><span data-stu-id="30f82-243">The jQuery script file.</span></span>

    * <span data-ttu-id="30f82-244">Podstawowy plik skryptu sygnalizującego.</span><span class="sxs-lookup"><span data-stu-id="30f82-244">The SignalR core script file.</span></span>

    * <span data-ttu-id="30f82-245">Plik skryptu dla serwerów proxy sygnałów.</span><span class="sxs-lookup"><span data-stu-id="30f82-245">The SignalR proxies script file.</span></span>

    * <span data-ttu-id="30f82-246">Plik skryptu StockTicker, który utworzysz później.</span><span class="sxs-lookup"><span data-stu-id="30f82-246">A StockTicker script file that you'll create later.</span></span>

    <span data-ttu-id="30f82-247">Aplikacja dynamicznie generuje plik skryptu dla serwerów proxy sygnalizującego.</span><span class="sxs-lookup"><span data-stu-id="30f82-247">The app dynamically generates the SignalR proxies script file.</span></span> <span data-ttu-id="30f82-248">Określa adres URL "/SignalR/Hubs" i definiuje metody serwera proxy dla metod klasy Hub, w tym przypadku `StockTickerHub.GetAllStocks`.</span><span class="sxs-lookup"><span data-stu-id="30f82-248">It specifies the "/signalr/hubs" URL and defines proxy methods for the methods on the Hub class, in this case, for `StockTickerHub.GetAllStocks`.</span></span> <span data-ttu-id="30f82-249">Jeśli wolisz, możesz wygenerować ten plik JavaScript ręcznie przy użyciu [narzędzi sygnalizujących](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/).</span><span class="sxs-lookup"><span data-stu-id="30f82-249">If you prefer, you can generate this JavaScript file manually by using [SignalR Utilities](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/).</span></span> <span data-ttu-id="30f82-250">Nie zapomnij wyłączyć tworzenia pliku dynamicznego w wywołaniu metody `MapHubs`.</span><span class="sxs-lookup"><span data-stu-id="30f82-250">Don't forget to disable dynamic file creation in the `MapHubs` method call.</span></span>

1. <span data-ttu-id="30f82-251">W **Eksplorator rozwiązań**rozwiń węzeł **skrypty**.</span><span class="sxs-lookup"><span data-stu-id="30f82-251">In **Solution Explorer**, expand **Scripts**.</span></span>

    <span data-ttu-id="30f82-252">Biblioteki skryptów dla jQuery i sygnalizujących są widoczne w projekcie.</span><span class="sxs-lookup"><span data-stu-id="30f82-252">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="30f82-253">Menedżer pakietów zainstaluje nowszą wersję skryptów sygnalizujących.</span><span class="sxs-lookup"><span data-stu-id="30f82-253">The package manager will install a later version of the SignalR scripts.</span></span>

1. <span data-ttu-id="30f82-254">Zaktualizuj odwołania do skryptu w bloku kodu, aby odpowiadały wersji plików skryptów w projekcie.</span><span class="sxs-lookup"><span data-stu-id="30f82-254">Update the script references in the code block to correspond to the versions of the script files in the project.</span></span>

1. <span data-ttu-id="30f82-255">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy pozycję *StockTicker. html*, a następnie wybierz pozycję **Ustaw jako stronę początkową**.</span><span class="sxs-lookup"><span data-stu-id="30f82-255">In **Solution Explorer**, right-click *StockTicker.html*, and then select **Set as Start Page**.</span></span>

#### <a name="create-stocktickerjs"></a><span data-ttu-id="30f82-256">Create StockTicker.js</span><span class="sxs-lookup"><span data-stu-id="30f82-256">Create StockTicker.js</span></span>

<span data-ttu-id="30f82-257">Teraz Utwórz plik JavaScript.</span><span class="sxs-lookup"><span data-stu-id="30f82-257">Now create the JavaScript file.</span></span>

1. <span data-ttu-id="30f82-258">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt i wybierz polecenie **Dodaj** > **plik JavaScript**.</span><span class="sxs-lookup"><span data-stu-id="30f82-258">In **Solution Explorer**, right-click the project and select **Add** > **JavaScript File**.</span></span>

1. <span data-ttu-id="30f82-259">Nazwij plik *StockTicker* i wybierz **przycisk OK**.</span><span class="sxs-lookup"><span data-stu-id="30f82-259">Name the file *StockTicker* and select **OK**.</span></span>

1. <span data-ttu-id="30f82-260">Dodaj ten kod do pliku *StockTicker. js* :</span><span class="sxs-lookup"><span data-stu-id="30f82-260">Add this code to the *StockTicker.js* file:</span></span>

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample12.js)]

### <a name="examine-the-client-code"></a><span data-ttu-id="30f82-261">Sprawdzanie kodu klienta</span><span class="sxs-lookup"><span data-stu-id="30f82-261">Examine the client code</span></span>

<span data-ttu-id="30f82-262">Po sprawdzeniu kodu klienta pomoże Ci dowiedzieć się, jak kod klienta współdziała z kodem serwera, aby umożliwić działanie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="30f82-262">If you examine the client code, it will help you learn how the client code interacts with the server code to make the app work.</span></span>

#### <a name="starting-the-connection"></a><span data-ttu-id="30f82-263">Uruchamianie połączenia</span><span class="sxs-lookup"><span data-stu-id="30f82-263">Starting the connection</span></span>

<span data-ttu-id="30f82-264">`$.connection` odnosi się do serwerów proxy sygnalizujących.</span><span class="sxs-lookup"><span data-stu-id="30f82-264">`$.connection` refers to the SignalR proxies.</span></span> <span data-ttu-id="30f82-265">Kod pobiera odwołanie do serwera proxy dla klasy `StockTickerHub` i umieszcza je w zmiennej `ticker`.</span><span class="sxs-lookup"><span data-stu-id="30f82-265">The code gets a reference to the proxy for the `StockTickerHub` class and puts it in the `ticker` variable.</span></span> <span data-ttu-id="30f82-266">Nazwa serwera proxy jest nazwą ustawioną przez atrybut `HubName`:</span><span class="sxs-lookup"><span data-stu-id="30f82-266">The proxy name is the name that was set by the `HubName` attribute:</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample13.js)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample14.cs)]

<span data-ttu-id="30f82-267">Po zdefiniowaniu wszystkich zmiennych i funkcji ostatni wiersz kodu w pliku inicjuje połączenie sygnalizujące przez wywołanie funkcji `start` sygnałów.</span><span class="sxs-lookup"><span data-stu-id="30f82-267">After you define all the variables and functions, the last line of code in the file initializes the SignalR connection by calling the SignalR `start` function.</span></span> <span data-ttu-id="30f82-268">Funkcja `start` wykonuje asynchroniczne i zwraca [odroczony obiekt jQuery](http://api.jquery.com/category/deferred-object/).</span><span class="sxs-lookup"><span data-stu-id="30f82-268">The `start` function executes asynchronously and returns a [jQuery Deferred object](http://api.jquery.com/category/deferred-object/).</span></span> <span data-ttu-id="30f82-269">Można wywołać funkcję gotowe, aby określić funkcję do wywołania, gdy aplikacja zakończy akcję asynchroniczną.</span><span class="sxs-lookup"><span data-stu-id="30f82-269">You can call the done function to specify the function to call when the app finishes the asynchronous action.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample15.js)]

#### <a name="getting-all-the-stocks"></a><span data-ttu-id="30f82-270">Pobieranie wszystkich zasobów</span><span class="sxs-lookup"><span data-stu-id="30f82-270">Getting all the stocks</span></span>

<span data-ttu-id="30f82-271">Funkcja `init` wywołuje funkcję `getAllStocks` na serwerze i używa informacji zwracanych przez serwer w celu zaktualizowania tabeli giełdowej.</span><span class="sxs-lookup"><span data-stu-id="30f82-271">The `init` function calls the `getAllStocks` function on the server and uses the information that the server returns to update the stock table.</span></span> <span data-ttu-id="30f82-272">Należy zauważyć, że domyślnie należy używać camelCasing na kliencie, nawet jeśli nazwa metody jest w języku Pascal-wielkość liter na serwerze.</span><span class="sxs-lookup"><span data-stu-id="30f82-272">Notice that, by default, you have to use camelCasing on the client even though the method name is pascal-cased on the server.</span></span> <span data-ttu-id="30f82-273">Reguła camelCasing dotyczy tylko metod, a nie obiektów.</span><span class="sxs-lookup"><span data-stu-id="30f82-273">The camelCasing rule only applies to methods, not objects.</span></span> <span data-ttu-id="30f82-274">Załóżmy na przykład, że odwołujesz się do `stock.Symbol` i `stock.Price`, a nie `stock.symbol` lub `stock.price`.</span><span class="sxs-lookup"><span data-stu-id="30f82-274">For example, you refer to `stock.Symbol` and `stock.Price`, not `stock.symbol` or `stock.price`.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample16.js)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample17.cs)]

<span data-ttu-id="30f82-275">W metodzie `init` aplikacja tworzy kod HTML dla wiersza tabeli dla każdego obiektu giełdowego otrzymanego z serwera, wywołując `formatStock` do formatowania właściwości obiektu `stock`, a następnie wywołując `supplant`, aby zamienić symbole zastępcze w zmiennej `rowTemplate` na wartości właściwości obiektu `stock`.</span><span class="sxs-lookup"><span data-stu-id="30f82-275">In the `init` method, the app creates HTML for a table row for each stock object received from the server by calling `formatStock` to format properties of the `stock` object, and then by calling `supplant` to replace placeholders in the `rowTemplate` variable with the `stock` object property values.</span></span> <span data-ttu-id="30f82-276">Otrzymany kod HTML jest następnie dołączany do tabeli giełdowej.</span><span class="sxs-lookup"><span data-stu-id="30f82-276">The resulting HTML is then appended to the stock table.</span></span>

> [!NOTE]
> <span data-ttu-id="30f82-277">`init` przez przekazanie go jako funkcji `callback`, która jest wykonywana po zakończeniu asynchronicznej funkcji `start`.</span><span class="sxs-lookup"><span data-stu-id="30f82-277">You call `init` by passing it in as a `callback` function that executes after the asynchronous `start` function finishes.</span></span> <span data-ttu-id="30f82-278">Jeśli wywołano `init` jako oddzielną instrukcję JavaScript po wywołaniu `start`, funkcja nie powiedzie się, ponieważ zostanie uruchomiona natychmiast bez oczekiwania na zakończenie tworzenia połączenia przez funkcję startową.</span><span class="sxs-lookup"><span data-stu-id="30f82-278">If you called `init` as a separate JavaScript statement after calling `start`, the function would fail because it would run immediately without waiting for the start function to finish establishing the connection.</span></span> <span data-ttu-id="30f82-279">W takim przypadku funkcja `init` spróbuje wywołać funkcję `getAllStocks`, zanim aplikacja nawiąże połączenie z serwerem.</span><span class="sxs-lookup"><span data-stu-id="30f82-279">In that case, the `init` function would try to call the `getAllStocks` function before the app establishes a server connection.</span></span>

#### <a name="getting-updated-stock-prices"></a><span data-ttu-id="30f82-280">Pobieranie zaktualizowanych cen giełdowych</span><span class="sxs-lookup"><span data-stu-id="30f82-280">Getting updated stock prices</span></span>

<span data-ttu-id="30f82-281">Gdy serwer zmieni cenę giełdową, wywoła `updateStockPrice` na podłączonych klientach.</span><span class="sxs-lookup"><span data-stu-id="30f82-281">When the server changes a stock's price, it calls the `updateStockPrice` on connected clients.</span></span> <span data-ttu-id="30f82-282">Aplikacja dodaje funkcję do właściwości client serwera proxy `stockTicker`, aby była dostępna dla wywołań z serwera.</span><span class="sxs-lookup"><span data-stu-id="30f82-282">The app adds the function to the client property of the `stockTicker` proxy to make it available to calls from the server.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample18.js)]

<span data-ttu-id="30f82-283">Funkcja `updateStockPrice` formatuje obiekt giełdowy otrzymany z serwera w wierszu tabeli w taki sam sposób jak w funkcji `init`.</span><span class="sxs-lookup"><span data-stu-id="30f82-283">The `updateStockPrice` function formats a stock object received from the server into a table row the same way as in the `init` function.</span></span> <span data-ttu-id="30f82-284">Zamiast dołączyć wiersz do tabeli, znajduje bieżący wiersz zapasu w tabeli i zamienia ten wiersz na nowy.</span><span class="sxs-lookup"><span data-stu-id="30f82-284">Instead of appending the row to the table, it finds the stock's current row in the table and replaces that row with the new one.</span></span>

## <a name="test-the-application"></a><span data-ttu-id="30f82-285">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="30f82-285">Test the application</span></span>

<span data-ttu-id="30f82-286">Możesz przetestować aplikację, aby upewnić się, że działa.</span><span class="sxs-lookup"><span data-stu-id="30f82-286">You can test the app to make sure it's working.</span></span> <span data-ttu-id="30f82-287">Zobaczysz wszystkie okna przeglądarki wyświetlają dynamiczną tabelę giełdową z wahaniami cen giełdowych.</span><span class="sxs-lookup"><span data-stu-id="30f82-287">You'll see all browser windows display the live stock table with stock prices fluctuating.</span></span>

1. <span data-ttu-id="30f82-288">Na pasku narzędzi Włącz **debugowanie skryptów** , a następnie wybierz przycisk Odtwórz, aby uruchomić aplikację w trybie debugowania.</span><span class="sxs-lookup"><span data-stu-id="30f82-288">In the toolbar, turn on **Script Debugging** and then select the play button to run the app in Debug mode.</span></span>

    ![Zrzut ekranu przedstawiający Włączanie trybu debugowania przez użytkownika i wybieranie opcji Odtwórz.](tutorial-server-broadcast-with-signalr/_static/image4.png)

    <span data-ttu-id="30f82-290">Zostanie otwarte okno przeglądarki z wyświetlaniem **tabeli giełdowej na żywo**.</span><span class="sxs-lookup"><span data-stu-id="30f82-290">A browser window will open displaying the **Live Stock Table**.</span></span> <span data-ttu-id="30f82-291">W tabeli giełdowej początkowo jest wyświetlana wartość "ładowanie..." Po krótkim czasie aplikacja wyświetli początkowe dane giełdowe, a następnie ceny giełdowe zaczynają się zmienić.</span><span class="sxs-lookup"><span data-stu-id="30f82-291">The stock table initially shows the "loading..." line, then, after a short time, the app shows the initial stock data, and then the stock prices start to change.</span></span>

1. <span data-ttu-id="30f82-292">Skopiuj adres URL z przeglądarki, Otwórz dwie inne przeglądarki i wklej adresy URL do pasków adresów.</span><span class="sxs-lookup"><span data-stu-id="30f82-292">Copy the URL from the browser, open two other browsers, and paste the URLs into the address bars.</span></span>

    <span data-ttu-id="30f82-293">Początkowy wyświetlacz giełdowy jest taki sam jak pierwsza przeglądarka i zmiany są wykonywane jednocześnie.</span><span class="sxs-lookup"><span data-stu-id="30f82-293">The initial stock display is the same as the first browser and changes happen simultaneously.</span></span>

1. <span data-ttu-id="30f82-294">Zamknij wszystkie przeglądarki, Otwórz nową przeglądarkę i przejdź do tego samego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="30f82-294">Close all browsers, open a new browser, and go to the same URL.</span></span>

    <span data-ttu-id="30f82-295">Obiekt StockTicker singleton nadal działa na serwerze.</span><span class="sxs-lookup"><span data-stu-id="30f82-295">The StockTicker singleton object continued to run in the server.</span></span> <span data-ttu-id="30f82-296">W **tabeli magazynu na żywo** widać, że zasoby nadal się zmieniają.</span><span class="sxs-lookup"><span data-stu-id="30f82-296">The **Live Stock Table** shows that the stocks have continued to change.</span></span> <span data-ttu-id="30f82-297">Nie widzisz tabeli początkowej ze zmianami o wartości zero.</span><span class="sxs-lookup"><span data-stu-id="30f82-297">You don't see the initial table with zero change figures.</span></span>

1. <span data-ttu-id="30f82-298">Zamknij okno przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="30f82-298">Close the browser.</span></span>

## <a name="enable-logging"></a><span data-ttu-id="30f82-299">Włączanie rejestrowania</span><span class="sxs-lookup"><span data-stu-id="30f82-299">Enable logging</span></span>

<span data-ttu-id="30f82-300">Sygnalizujący ma wbudowaną funkcję rejestrowania, którą można włączyć na kliencie, aby pomóc w rozwiązywaniu problemów.</span><span class="sxs-lookup"><span data-stu-id="30f82-300">SignalR has a built-in logging function that you can enable on the client to aid in troubleshooting.</span></span> <span data-ttu-id="30f82-301">W tej części należy włączyć rejestrowanie i zobaczyć przykłady pokazujące, w jaki sposób dzienniki poinformują o następujących metodach transportu:</span><span class="sxs-lookup"><span data-stu-id="30f82-301">In this section, you enable logging and see examples that show how logs tell you which of the following transport methods SignalR is using:</span></span>

* <span data-ttu-id="30f82-302">Obiekty [WebSockets](http://en.wikipedia.org/wiki/WebSocket)obsługiwane przez usługi IIS 8 i bieżące przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="30f82-302">[WebSockets](http://en.wikipedia.org/wiki/WebSocket), supported by IIS 8 and current browsers.</span></span>

* <span data-ttu-id="30f82-303">[Zdarzenia wysyłane przez serwer](http://en.wikipedia.org/wiki/Server-sent_events)obsługiwane przez przeglądarki inne niż Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="30f82-303">[Server-sent events](http://en.wikipedia.org/wiki/Server-sent_events), supported by browsers other than Internet Explorer.</span></span>

* <span data-ttu-id="30f82-304">[Ramka bez ograniczeń](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe)obsługiwana przez program Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="30f82-304">[Forever frame](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe), supported by Internet Explorer.</span></span>

* <span data-ttu-id="30f82-305">[Długotrwałe sondowanie AJAX](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling)obsługiwane przez wszystkie przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="30f82-305">[Ajax long polling](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling), supported by all browsers.</span></span>

<span data-ttu-id="30f82-306">Dla dowolnego połączenia sygnalizujący wybiera najlepszą metodę transportu, którą obsługuje serwer i klient.</span><span class="sxs-lookup"><span data-stu-id="30f82-306">For any given connection, SignalR chooses the best transport method that both the server and the client support.</span></span>

1. <span data-ttu-id="30f82-307">Otwórz *StockTicker. js*.</span><span class="sxs-lookup"><span data-stu-id="30f82-307">Open *StockTicker.js*.</span></span>

1. <span data-ttu-id="30f82-308">Dodaj ten wyróżniony wiersz kodu, aby włączyć rejestrowanie bezpośrednio przed kodem, który inicjuje połączenie na końcu pliku:</span><span class="sxs-lookup"><span data-stu-id="30f82-308">Add this highlighted line of code to enable logging immediately before the code that initializes the connection at the end of the file:</span></span>

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample19.js?highlight=2)]

1. <span data-ttu-id="30f82-309">Naciśnij klawisz **F5** , aby uruchomić projekt.</span><span class="sxs-lookup"><span data-stu-id="30f82-309">Press **F5** to run the project.</span></span>

1. <span data-ttu-id="30f82-310">Otwórz okno narzędzia deweloperskie w przeglądarce i wybierz konsolę, aby wyświetlić dzienniki.</span><span class="sxs-lookup"><span data-stu-id="30f82-310">Open your browser's developer tools window, and select the Console to see the logs.</span></span> <span data-ttu-id="30f82-311">Może być konieczne odświeżenie strony, aby wyświetlić dzienniki sygnalizujące transportowanie metody transportu dla nowego połączenia.</span><span class="sxs-lookup"><span data-stu-id="30f82-311">You might have to refresh the page to see the logs of SignalR negotiating the transport method for a new connection.</span></span>

    * <span data-ttu-id="30f82-312">Jeśli używasz programu Internet Explorer 10 w systemie Windows 8 (IIS 8), Metoda transportowa to **WebSockets**.</span><span class="sxs-lookup"><span data-stu-id="30f82-312">If you're running Internet Explorer 10 on Windows 8 (IIS 8), the transport method is **WebSockets**.</span></span>

    * <span data-ttu-id="30f82-313">Jeśli używasz programu Internet Explorer 10 w systemie Windows 7 (IIS 7,5), Metoda transportu jest **iframe**.</span><span class="sxs-lookup"><span data-stu-id="30f82-313">If you're running Internet Explorer 10 on Windows 7 (IIS 7.5), the transport method is **iframe**.</span></span>

    * <span data-ttu-id="30f82-314">Jeśli korzystasz z programu Firefox 19 w systemie Windows 8 (IIS 8), Metoda transportu jest funkcją **WebSockets**.</span><span class="sxs-lookup"><span data-stu-id="30f82-314">If you're running Firefox 19 on Windows 8 (IIS 8), the transport method is **WebSockets**.</span></span>

        > [!TIP]
        > <span data-ttu-id="30f82-315">W programie Firefox zainstaluj dodatek Firebug, aby uzyskać okno konsoli.</span><span class="sxs-lookup"><span data-stu-id="30f82-315">In Firefox, install the Firebug add-in to get a Console window.</span></span>

    * <span data-ttu-id="30f82-316">W przypadku korzystania z programu Firefox 19 w systemie Windows 7 (IIS 7,5) Metoda transportu to zdarzenia **wysłane przez serwer** .</span><span class="sxs-lookup"><span data-stu-id="30f82-316">If you're running Firefox 19 on Windows 7 (IIS 7.5), the transport method is **server-sent** events.</span></span>

## <a name="install-the-stockticker-sample"></a><span data-ttu-id="30f82-317">Instalowanie przykładu StockTicker</span><span class="sxs-lookup"><span data-stu-id="30f82-317">Install the StockTicker sample</span></span>

<span data-ttu-id="30f82-318">[Microsoft. ASPNET. Signal. przykład](http://nuget.org/packages/microsoft.aspnet.signalr.sample) instaluje aplikację StockTicker.</span><span class="sxs-lookup"><span data-stu-id="30f82-318">The [Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) installs the StockTicker application.</span></span> <span data-ttu-id="30f82-319">Pakiet NuGet zawiera więcej funkcji niż wersja uproszczona utworzona od podstaw.</span><span class="sxs-lookup"><span data-stu-id="30f82-319">The NuGet package includes more features than the simplified version that you created from scratch.</span></span> <span data-ttu-id="30f82-320">W tej części samouczka zainstalujesz pakiet NuGet i zapoznajesz się z nowymi funkcjami oraz kodem, który je implementuje.</span><span class="sxs-lookup"><span data-stu-id="30f82-320">In this section of the tutorial, you install the NuGet package and review the new features and the code that implements them.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="30f82-321">Jeśli pakiet zostanie zainstalowany bez wykonywania wcześniejszych kroków tego samouczka, należy dodać do projektu klasę uruchomieniową OWIN.</span><span class="sxs-lookup"><span data-stu-id="30f82-321">If you install the package without performing the earlier steps of this tutorial, you must add an OWIN startup class to your project.</span></span> <span data-ttu-id="30f82-322">Ten krok zawiera opis tego pliku Readme. txt dla pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="30f82-322">This readme.txt file for the NuGet package explains this step.</span></span>

### <a name="install-the-signalrsample-nuget-package"></a><span data-ttu-id="30f82-323">Zainstaluj program sygnalizujący. przykładowy pakiet NuGet</span><span class="sxs-lookup"><span data-stu-id="30f82-323">Install the SignalR.Sample NuGet package</span></span>

1. <span data-ttu-id="30f82-324">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt i wybierz polecenie **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="30f82-324">In **Solution Explorer**, right-click the project and select **Manage NuGet Packages**.</span></span>

1. <span data-ttu-id="30f82-325">W **Menedżerze pakietów NuGet: signaler. StockTicker**, wybierz pozycję **Przeglądaj**.</span><span class="sxs-lookup"><span data-stu-id="30f82-325">In **NuGet Package manager: SignalR.StockTicker**, select **Browse**.</span></span>

1. <span data-ttu-id="30f82-326">W obszarze **Źródło pakietu**wybierz pozycję **NuGet.org**.</span><span class="sxs-lookup"><span data-stu-id="30f82-326">From **Package source**, select **nuget.org**.</span></span>

1. <span data-ttu-id="30f82-327">Wprowadź *sygnalizowanie. przykład* w polu wyszukiwania i wybierz pozycję **Microsoft. ASPNET. signal. sample** > **Install**.</span><span class="sxs-lookup"><span data-stu-id="30f82-327">Enter *SignalR.Sample* in the search box and select **Microsoft.AspNet.SignalR.Sample** > **Install**.</span></span>

1. <span data-ttu-id="30f82-328">W **Eksplorator rozwiązań**rozwiń folder *sygnalizujący. sample* .</span><span class="sxs-lookup"><span data-stu-id="30f82-328">In **Solution Explorer**, expand the *SignalR.Sample* folder.</span></span>

    <span data-ttu-id="30f82-329">Instalowanie programu sygnalizującego. przykładowy pakiet utworzył folder i jego zawartość.</span><span class="sxs-lookup"><span data-stu-id="30f82-329">Installing the SignalR.Sample package created the folder and its contents.</span></span>

1. <span data-ttu-id="30f82-330">W folderze *sygnalizującym. sample* kliknij prawym przyciskiem myszy *plik StockTicker. html*, a następnie wybierz pozycję **Ustaw jako stronę początkową**.</span><span class="sxs-lookup"><span data-stu-id="30f82-330">In the *SignalR.Sample* folder, right-click *StockTicker.html*, and then select **Set As Start Page**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="30f82-331">Instalowanie programu sygnalizującego. przykładowy pakiet NuGet może zmienić wersję platformy jQuery, która znajduje się w folderze *skryptów* .</span><span class="sxs-lookup"><span data-stu-id="30f82-331">Installing The SignalR.Sample NuGet package might change the version of jQuery that you have in your *Scripts* folder.</span></span> <span data-ttu-id="30f82-332">Nowy plik *StockTicker. html* instalowany przez pakiet w *sygnale. przykładowy* folder będzie synchronizowany z wersją jQuery, którą instaluje pakiet, ale jeśli chcesz ponownie uruchomić oryginalny plik *StockTicker. html* , być może trzeba będzie najpierw zaktualizować odwołanie do jQuery w tagu skryptu.</span><span class="sxs-lookup"><span data-stu-id="30f82-332">The new *StockTicker.html* file that the package installs in the *SignalR.Sample* folder will be in sync with the jQuery version that the package installs, but if you want to run your original *StockTicker.html* file again, you might have to update the jQuery reference in the script tag first.</span></span>

### <a name="run-the-application"></a><span data-ttu-id="30f82-333">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="30f82-333">Run the application</span></span>

 <span data-ttu-id="30f82-334">Tabela, która została zaprojektowana w pierwszej aplikacji, miała przydatne funkcje.</span><span class="sxs-lookup"><span data-stu-id="30f82-334">The table that you saw in the first app had useful features.</span></span> <span data-ttu-id="30f82-335">W przypadku pełnego spisu aplikacji są widoczne nowe funkcje: okno przewijania w poziomie, które pokazuje dane giełdowe i zasoby, które zmieniają kolor w miarę wzrostu i spadku.</span><span class="sxs-lookup"><span data-stu-id="30f82-335">The full stock ticker application shows new features: a horizontally scrolling window that shows the stock data and stocks that change color as they rise and fall.</span></span>

1. <span data-ttu-id="30f82-336">Naciśnij klawisz **F5** , aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="30f82-336">Press **F5** to run the app.</span></span>

     <span data-ttu-id="30f82-337">Po uruchomieniu aplikacji po raz pierwszy "rynek" jest "zamknięty" i zobaczysz tabelę statyczną i okno znaczników, które nie jest przewijane.</span><span class="sxs-lookup"><span data-stu-id="30f82-337">When you run the app for the first time, the "market" is "closed" and you see a static table and a ticker window that isn't scrolling.</span></span>

1. <span data-ttu-id="30f82-338">Wybierz pozycję **Otwórz rynek**.</span><span class="sxs-lookup"><span data-stu-id="30f82-338">Select **Open Market**.</span></span>

    ![Zrzut ekranu dynamicznego znacznika.](tutorial-server-broadcast-with-signalr/_static/image5.png)

    * <span data-ttu-id="30f82-340">Skrzynka **giełdowa na żywo** zostanie przesunięta w poziomie, a serwer rozpocznie okresowe emitowanie zmian cen giełdowych.</span><span class="sxs-lookup"><span data-stu-id="30f82-340">The **Live Stock Ticker** box starts to scroll horizontally, and the server starts to periodically broadcast stock price changes on a random basis.</span></span>

    * <span data-ttu-id="30f82-341">Za każdym razem, gdy zmieni się cena giełdowa, aplikacja aktualizuje zarówno **dynamiczną** , jak i giełdę na **żywo**.</span><span class="sxs-lookup"><span data-stu-id="30f82-341">Each time a stock price changes, the app updates both the **Live Stock Table** and the **Live Stock Ticker**.</span></span>

    * <span data-ttu-id="30f82-342">Gdy zmiana ceny zapasów jest dodatnia, aplikacja pokazuje magazyn z zielonym tłem.</span><span class="sxs-lookup"><span data-stu-id="30f82-342">When a stock's price change is positive, the app shows the stock with a green background.</span></span>

    * <span data-ttu-id="30f82-343">Gdy zmiana jest ujemna, aplikacja pokazuje zapas z czerwonym tłem.</span><span class="sxs-lookup"><span data-stu-id="30f82-343">When the change is negative, the app shows the stock with a red background.</span></span>

1. <span data-ttu-id="30f82-344">Wybierz pozycję **Zamknij rynek**.</span><span class="sxs-lookup"><span data-stu-id="30f82-344">Select **Close Market**.</span></span>

    * <span data-ttu-id="30f82-345">Aktualizacja tabeli zatrzymuje się.</span><span class="sxs-lookup"><span data-stu-id="30f82-345">The table updates stop.</span></span>

    * <span data-ttu-id="30f82-346">Znacznik przestaje przewijać.</span><span class="sxs-lookup"><span data-stu-id="30f82-346">The ticker stops scrolling.</span></span>

1. <span data-ttu-id="30f82-347">Wybierz pozycję **Zresetuj**.</span><span class="sxs-lookup"><span data-stu-id="30f82-347">Select **Reset**.</span></span>

    * <span data-ttu-id="30f82-348">Wszystkie dane giełdowe są resetowane.</span><span class="sxs-lookup"><span data-stu-id="30f82-348">All stock data is reset.</span></span>

    * <span data-ttu-id="30f82-349">Aplikacja przywraca stan początkowy przed rozpoczęciem zmian cen.</span><span class="sxs-lookup"><span data-stu-id="30f82-349">The app restores the initial state before price changes started.</span></span>

1. <span data-ttu-id="30f82-350">Skopiuj adres URL z przeglądarki, Otwórz dwie inne przeglądarki i wklej adresy URL do pasków adresów.</span><span class="sxs-lookup"><span data-stu-id="30f82-350">Copy the URL from the browser, open two other browsers, and paste the URLs into the address bars.</span></span>

1. <span data-ttu-id="30f82-351">Te same dane są automatycznie aktualizowane w każdej przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="30f82-351">You see the same data dynamically updated at the same time in each browser.</span></span>

1. <span data-ttu-id="30f82-352">Po wybraniu dowolnej z tych kontrolek wszystkie przeglądarki reagują w ten sam sposób w tym samym czasie.</span><span class="sxs-lookup"><span data-stu-id="30f82-352">When you select any of the controls, all browsers respond the same way at the same time.</span></span>

### <a name="live-stock-ticker-display"></a><span data-ttu-id="30f82-353">Wyświetlanie grafu giełdowego na żywo</span><span class="sxs-lookup"><span data-stu-id="30f82-353">Live Stock Ticker display</span></span>

<span data-ttu-id="30f82-354">Wyświetlacz **giełdowy na żywo** jest nieuporządkowaną listą w elemencie `<div>` sformatowanym w jednym wierszu według stylów CSS.</span><span class="sxs-lookup"><span data-stu-id="30f82-354">The **Live Stock Ticker** display is an unordered list in a `<div>` element formatted into a single line by CSS styles.</span></span> <span data-ttu-id="30f82-355">Aplikacja inicjuje i aktualizuje znaczniki w taki sam sposób jak tabela: przez zastąpienie symboli zastępczych w ciągu szablonu `<li>` i dynamiczne dodanie elementów `<li>` do elementu `<ul>`.</span><span class="sxs-lookup"><span data-stu-id="30f82-355">The app initializes and updates the ticker the same way as the table: by replacing placeholders in an `<li>` template string and dynamically adding the `<li>` elements to the `<ul>` element.</span></span> <span data-ttu-id="30f82-356">Aplikacja obejmuje przewijanie przy użyciu funkcji jQuery `animate` w celu zaróżnienia lewego marginesu listy nieuporządkowanej w `<div>`.</span><span class="sxs-lookup"><span data-stu-id="30f82-356">The app includes  scrolling by using the jQuery `animate` function to vary the margin-left of the unordered list within the `<div>`.</span></span>

#### <a name="signalrsample-stocktickerhtml"></a><span data-ttu-id="30f82-357">Sygnalizujący. przykład StockTicker. html</span><span class="sxs-lookup"><span data-stu-id="30f82-357">SignalR.Sample StockTicker.html</span></span>

<span data-ttu-id="30f82-358">Kod HTML znacznika giełdowego:</span><span class="sxs-lookup"><span data-stu-id="30f82-358">The stock ticker HTML code:</span></span>

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample20.html)]

#### <a name="signalrsample-stocktickercss"></a><span data-ttu-id="30f82-359">Sygnalizujący. przykład StockTicker. CSS</span><span class="sxs-lookup"><span data-stu-id="30f82-359">SignalR.Sample StockTicker.css</span></span>

<span data-ttu-id="30f82-360">Kod CSS znacznika giełdowego:</span><span class="sxs-lookup"><span data-stu-id="30f82-360">The stock ticker CSS code:</span></span>

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample21.html)]

#### <a name="signalrsample-signalrstocktickerjs"></a><span data-ttu-id="30f82-361">Sygnalizujący. przykładowy sygnał. StockTicker. js</span><span class="sxs-lookup"><span data-stu-id="30f82-361">SignalR.Sample SignalR.StockTicker.js</span></span>

<span data-ttu-id="30f82-362">Kod jQuery, który umożliwia przewinięcie:</span><span class="sxs-lookup"><span data-stu-id="30f82-362">The jQuery code that makes it scroll:</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample22.js)]

### <a name="additional-methods-on-the-server-that-the-client-can-call"></a><span data-ttu-id="30f82-363">Dodatkowe metody na serwerze, z którym klient może wywoływać</span><span class="sxs-lookup"><span data-stu-id="30f82-363">Additional methods on the server that the client can call</span></span>

<span data-ttu-id="30f82-364">Aby dodać elastyczność do aplikacji, istnieją dodatkowe metody, które może wywoływać aplikacja.</span><span class="sxs-lookup"><span data-stu-id="30f82-364">To add flexibility to the app, there are additional methods the app can call.</span></span>

#### <a name="signalrsample-stocktickerhubcs"></a><span data-ttu-id="30f82-365">SignalR.Sample StockTickerHub.cs</span><span class="sxs-lookup"><span data-stu-id="30f82-365">SignalR.Sample StockTickerHub.cs</span></span>

<span data-ttu-id="30f82-366">Klasa `StockTickerHub` definiuje cztery dodatkowe metody, które klient może wywoływać:</span><span class="sxs-lookup"><span data-stu-id="30f82-366">The `StockTickerHub` class defines four additional methods that the client can call:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample23.cs)]

<span data-ttu-id="30f82-367">Aplikacja wywołuje `OpenMarket`, `CloseMarket`i `Reset` w odpowiedzi na przyciski w górnej części strony.</span><span class="sxs-lookup"><span data-stu-id="30f82-367">The app calls `OpenMarket`, `CloseMarket`, and `Reset` in response to the buttons at the top of the page.</span></span> <span data-ttu-id="30f82-368">Pokazują one wzorzec jednego klienta wyzwalającego zmianę stanu natychmiast propagowany do wszystkich klientów.</span><span class="sxs-lookup"><span data-stu-id="30f82-368">They demonstrate the pattern of one client triggering a change in state immediately propagated to all clients.</span></span> <span data-ttu-id="30f82-369">Każda z tych metod wywołuje metodę w klasie `StockTicker`, która powoduje zmianę stanu rynku, a następnie emituje nowy stan.</span><span class="sxs-lookup"><span data-stu-id="30f82-369">Each of these methods calls a method in the `StockTicker` class that causes the market state change and then broadcasts the new state.</span></span>

#### <a name="signalrsample-stocktickercs"></a><span data-ttu-id="30f82-370">Sygnalizujący. przykład StockTicker.cs</span><span class="sxs-lookup"><span data-stu-id="30f82-370">SignalR.Sample StockTicker.cs</span></span>

<span data-ttu-id="30f82-371">W klasie `StockTicker` aplikacja zachowuje stan rynku z właściwością `MarketState`, która zwraca `MarketState` wartość wyliczenia:</span><span class="sxs-lookup"><span data-stu-id="30f82-371">In the `StockTicker` class, the app maintains the state of the market with a `MarketState` property that returns a `MarketState` enum value:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample24.cs)]

<span data-ttu-id="30f82-372">Każda z metod, które zmieniają stan rynku, to w bloku blokady, ponieważ Klasa `StockTicker` musi być bezpieczna wątkowo:</span><span class="sxs-lookup"><span data-stu-id="30f82-372">Each of the methods that change the market state do so inside a lock block because the `StockTicker` class has to be thread-safe:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample25.cs)]

<span data-ttu-id="30f82-373">Aby upewnić się, że ten kod jest bezpieczny dla wątków, pole `_marketState`, które wykonuje kopię zapasową właściwości `MarketState` wyznaczono `volatile`:</span><span class="sxs-lookup"><span data-stu-id="30f82-373">To make sure this code is thread-safe, the `_marketState` field that backs the `MarketState` property designated `volatile`:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample26.cs)]

<span data-ttu-id="30f82-374">Metody `BroadcastMarketStateChange` i `BroadcastMarketReset` są podobne do metody BroadcastStockPrice, która została już wykorzystana, z tą różnicą, że wywołują różne metody zdefiniowane na kliencie:</span><span class="sxs-lookup"><span data-stu-id="30f82-374">The `BroadcastMarketStateChange` and `BroadcastMarketReset` methods are similar to the BroadcastStockPrice method that you already saw, except they call different methods defined at the client:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample27.cs)]

### <a name="additional-functions-on-the-client-that-the-server-can-call"></a><span data-ttu-id="30f82-375">Dodatkowe funkcje na kliencie, które może wywoływać serwer</span><span class="sxs-lookup"><span data-stu-id="30f82-375">Additional functions on the client that the server can call</span></span>

<span data-ttu-id="30f82-376">Funkcja `updateStockPrice` obsługuje teraz zarówno tabelę, jak i wyświetlanie znaczników i używa `jQuery.Color` do Flash Red i Green Color.</span><span class="sxs-lookup"><span data-stu-id="30f82-376">The `updateStockPrice` function now handles both the table and the ticker display, and it uses `jQuery.Color` to flash red and green colors.</span></span>

<span data-ttu-id="30f82-377">Nowe funkcje w programie *sygnalizującer. StockTicker. js* włącza i wyłącza przyciski na podstawie stanu na rynku.</span><span class="sxs-lookup"><span data-stu-id="30f82-377">New functions in *SignalR.StockTicker.js* enable and disable the buttons based on market state.</span></span> <span data-ttu-id="30f82-378">Zatrzymają one również lub uruchamiają przewijanie w poziomie giełdy na **żywo** .</span><span class="sxs-lookup"><span data-stu-id="30f82-378">They also stop or start the **Live Stock Ticker** horizontal scrolling.</span></span> <span data-ttu-id="30f82-379">Ponieważ wiele funkcji jest dodawanych do `ticker.client`, aplikacja używa [funkcji rozszerzającej jQuery](http://api.jquery.com/jQuery.extend/) , aby je dodać.</span><span class="sxs-lookup"><span data-stu-id="30f82-379">Since many functions are being added to `ticker.client`, the app uses the [jQuery extend function](http://api.jquery.com/jQuery.extend/) to add them.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample28.js)]

### <a name="additional-client-setup-after-establishing-the-connection"></a><span data-ttu-id="30f82-380">Dodatkowa konfiguracja klienta po ustanowieniu połączenia</span><span class="sxs-lookup"><span data-stu-id="30f82-380">Additional client setup after establishing the connection</span></span>

<span data-ttu-id="30f82-381">Po nawiązaniu połączenia klient ma kilka dodatkowych czynności do wykonania:</span><span class="sxs-lookup"><span data-stu-id="30f82-381">After the client establishes the connection, it has some additional work to do:</span></span>

* <span data-ttu-id="30f82-382">Sprawdź, czy rynek jest otwarty lub zamknięty w celu wywołania odpowiedniej funkcji `marketOpened` lub `marketClosed`.</span><span class="sxs-lookup"><span data-stu-id="30f82-382">Find out if the market is open or closed to call the appropriate `marketOpened` or `marketClosed` function.</span></span>

* <span data-ttu-id="30f82-383">Dołącz wywołania metody serwera do przycisków.</span><span class="sxs-lookup"><span data-stu-id="30f82-383">Attach the server method calls to the buttons.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample29.js)]

<span data-ttu-id="30f82-384">Metody serwera nie są połączone z przyciskami do momentu, gdy aplikacja nawiąże połączenie.</span><span class="sxs-lookup"><span data-stu-id="30f82-384">The server methods aren't wired up to the buttons until after the app establishes the connection.</span></span> <span data-ttu-id="30f82-385">Kod nie może wywoływać metod serwera przed ich udostępnieniem.</span><span class="sxs-lookup"><span data-stu-id="30f82-385">It's so the code can't call the server methods before they're available.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="30f82-386">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="30f82-386">Additional resources</span></span>

<span data-ttu-id="30f82-387">W tym samouczku przedstawiono sposób programowania aplikacji sygnalizującej, która emituje komunikaty z serwera do wszystkich połączonych klientów.</span><span class="sxs-lookup"><span data-stu-id="30f82-387">In this tutorial you've learned how to program a SignalR application that broadcasts messages from the server to all connected clients.</span></span> <span data-ttu-id="30f82-388">Teraz można emitować wiadomości okresowo i w odpowiedzi na powiadomienia z dowolnego klienta.</span><span class="sxs-lookup"><span data-stu-id="30f82-388">Now you can broadcast messages on a periodic basis and in response to notifications from any client.</span></span> <span data-ttu-id="30f82-389">Można użyć koncepcji wielowątkowego wystąpienia pojedynczego, aby zachować stan serwera w scenariuszach gier online obejmujących wiele graczy.</span><span class="sxs-lookup"><span data-stu-id="30f82-389">You can use the concept of multi-threaded singleton instance to maintain server state in multi-player online game scenarios.</span></span> <span data-ttu-id="30f82-390">Aby zapoznać się z przykładem, zapoznaj [się z grą korzystającą z usługi sygnalizującej](https://github.com/NTaylorMullen/ShootR).</span><span class="sxs-lookup"><span data-stu-id="30f82-390">For an example, see [the ShootR game based on SignalR](https://github.com/NTaylorMullen/ShootR).</span></span>

<span data-ttu-id="30f82-391">Aby zapoznać się z samouczkami pokazującymi scenariusze komunikacji równorzędnej, zobacz [wprowadzenie z sygnalizacją](introduction-to-signalr.md) i [aktualizacją w czasie rzeczywistym za pomocą usługi sygnalizującego](tutorial-high-frequency-realtime-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="30f82-391">For tutorials that show peer-to-peer communication scenarios, see [Getting Started with SignalR](introduction-to-signalr.md) and [Real-Time Updating with SignalR](tutorial-high-frequency-realtime-with-signalr.md).</span></span>

<span data-ttu-id="30f82-392">Aby uzyskać więcej informacji o sygnalizacji, zobacz następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="30f82-392">For more about SignalR, see the following resources:</span></span>

* [<span data-ttu-id="30f82-393">ASP.NET sygnalizujący</span><span class="sxs-lookup"><span data-stu-id="30f82-393">ASP.NET SignalR</span></span>](../../index.md)
* [<span data-ttu-id="30f82-394">Projekt sygnalizującego</span><span class="sxs-lookup"><span data-stu-id="30f82-394">SignalR Project</span></span>](http://signalr.net/)
* [<span data-ttu-id="30f82-395">Usługi GitHub i przykłady dla sygnałów</span><span class="sxs-lookup"><span data-stu-id="30f82-395">SignalR GitHub and Samples</span></span>](https://github.com/SignalR/SignalR)
* [<span data-ttu-id="30f82-396">Strona typu wiki sygnalizująca</span><span class="sxs-lookup"><span data-stu-id="30f82-396">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a><span data-ttu-id="30f82-397">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="30f82-397">Next steps</span></span>

<span data-ttu-id="30f82-398">W tym samouczku zostaną wykonane następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="30f82-398">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="30f82-399">Utworzono projekt</span><span class="sxs-lookup"><span data-stu-id="30f82-399">Created the project</span></span>
> * <span data-ttu-id="30f82-400">Konfigurowanie kodu serwera</span><span class="sxs-lookup"><span data-stu-id="30f82-400">Set up the server code</span></span>
> * <span data-ttu-id="30f82-401">Zbadano kod serwera</span><span class="sxs-lookup"><span data-stu-id="30f82-401">Examined the server code</span></span>
> * <span data-ttu-id="30f82-402">Konfigurowanie kodu klienta</span><span class="sxs-lookup"><span data-stu-id="30f82-402">Set up the client code</span></span>
> * <span data-ttu-id="30f82-403">Zbadano kod klienta</span><span class="sxs-lookup"><span data-stu-id="30f82-403">Examined the client code</span></span>
> * <span data-ttu-id="30f82-404">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="30f82-404">Tested the application</span></span>
> * <span data-ttu-id="30f82-405">Włączone rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="30f82-405">Enabled logging</span></span>

<span data-ttu-id="30f82-406">Przejdź do następnego artykułu, aby dowiedzieć się, jak utworzyć aplikację sieci Web w czasie rzeczywistym korzystającą z ASP.NET sygnalizującego 2.</span><span class="sxs-lookup"><span data-stu-id="30f82-406">Advance to the next article to learn how to create a real-time web application that uses ASP.NET SignalR 2.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="30f82-407">Tworzenie aplikacji sieci Web w czasie rzeczywistym za pomocą sygnalizującego</span><span class="sxs-lookup"><span data-stu-id="30f82-407">Create real-time web app with SignalR</span></span>](real-time-web-applications-with-signalr.md)

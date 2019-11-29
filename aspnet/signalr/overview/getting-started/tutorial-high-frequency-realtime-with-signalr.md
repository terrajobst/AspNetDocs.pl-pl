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
# <a name="tutorial-create-high-frequency-real-time-app-with-signalr-2"></a><span data-ttu-id="0bb54-103">Samouczek: Tworzenie aplikacji w czasie rzeczywistym o wysokiej częstotliwości z sygnałem 2</span><span class="sxs-lookup"><span data-stu-id="0bb54-103">Tutorial: Create high-frequency real-time app with SignalR 2</span></span>

<span data-ttu-id="0bb54-104">W tym samouczku pokazano, jak utworzyć aplikację sieci Web korzystającą z ASP.NET sygnalizującego 2, aby zapewnić funkcję obsługi komunikatów o wysokiej częstotliwości.</span><span class="sxs-lookup"><span data-stu-id="0bb54-104">This tutorial shows how to create a web application that uses ASP.NET SignalR 2 to provide high-frequency messaging functionality.</span></span> <span data-ttu-id="0bb54-105">W takim przypadku "Obsługa komunikatów o wysokiej częstotliwości" oznacza, że serwer wysyła aktualizacje ze stałą stawką.</span><span class="sxs-lookup"><span data-stu-id="0bb54-105">In this case, "high-frequency messaging" means the server sends updates at a fixed rate.</span></span> <span data-ttu-id="0bb54-106">Wysyłasz do 10 wiadomości w drugim.</span><span class="sxs-lookup"><span data-stu-id="0bb54-106">You send up to 10 messages a second.</span></span>

<span data-ttu-id="0bb54-107">Utworzona aplikacja wyświetla kształt, który użytkownicy mogą przeciągać.</span><span class="sxs-lookup"><span data-stu-id="0bb54-107">The application you create displays a shape that users can drag.</span></span> <span data-ttu-id="0bb54-108">Serwer aktualizuje położenie kształtu we wszystkich połączonych przeglądarkach, aby dopasować położenie przeciąganego kształtu przy użyciu aktualizacji z limitem czasu.</span><span class="sxs-lookup"><span data-stu-id="0bb54-108">The server updates the position of the shape in all connected browsers to match the position of the dragged shape using timed updates.</span></span>

<span data-ttu-id="0bb54-109">Koncepcje wprowadzone w tym samouczku zawierają aplikacje w czasie rzeczywistym oraz inne aplikacje symulacyjne.</span><span class="sxs-lookup"><span data-stu-id="0bb54-109">Concepts introduced in this tutorial have applications in real-time gaming and other simulation applications.</span></span>

<span data-ttu-id="0bb54-110">W tym samouczku przedstawiono następujące instrukcje:</span><span class="sxs-lookup"><span data-stu-id="0bb54-110">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="0bb54-111">Konfigurowanie projektu</span><span class="sxs-lookup"><span data-stu-id="0bb54-111">Set up the project</span></span>
> * <span data-ttu-id="0bb54-112">Tworzenie aplikacji podstawowej</span><span class="sxs-lookup"><span data-stu-id="0bb54-112">Create the base application</span></span>
> * <span data-ttu-id="0bb54-113">Mapuj do centrum podczas uruchamiania aplikacji</span><span class="sxs-lookup"><span data-stu-id="0bb54-113">Map to the hub when app starts</span></span>
> * <span data-ttu-id="0bb54-114">Dodawanie klienta</span><span class="sxs-lookup"><span data-stu-id="0bb54-114">Add the client</span></span>
> * <span data-ttu-id="0bb54-115">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="0bb54-115">Run the app</span></span>
> * <span data-ttu-id="0bb54-116">Dodaj pętlę klienta</span><span class="sxs-lookup"><span data-stu-id="0bb54-116">Add the client loop</span></span>
> * <span data-ttu-id="0bb54-117">Dodaj pętlę serwera</span><span class="sxs-lookup"><span data-stu-id="0bb54-117">Add the server loop</span></span>
> * <span data-ttu-id="0bb54-118">Dodaj gładką animację</span><span class="sxs-lookup"><span data-stu-id="0bb54-118">Add smooth animation</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a><span data-ttu-id="0bb54-119">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="0bb54-119">Prerequisites</span></span>

* <span data-ttu-id="0bb54-120">[Program Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) z **ASP.NET i programowaniem w sieci Web** .</span><span class="sxs-lookup"><span data-stu-id="0bb54-120">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) with the **ASP.NET and web development** workload.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="0bb54-121">Konfigurowanie projektu</span><span class="sxs-lookup"><span data-stu-id="0bb54-121">Set up the project</span></span>

<span data-ttu-id="0bb54-122">W tej sekcji utworzysz projekt w programie Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="0bb54-122">In this section, you create the project in Visual Studio 2017.</span></span>

<span data-ttu-id="0bb54-123">W tej sekcji pokazano, jak za pomocą programu Visual Studio 2017 utworzyć pustą aplikację sieci Web ASP.NET i dodać do niej elementy sygnalizujące i jQuery. UI.</span><span class="sxs-lookup"><span data-stu-id="0bb54-123">This section shows how to use Visual Studio 2017 to create an empty ASP.NET Web Application and add the SignalR and jQuery.UI libraries.</span></span>

1. <span data-ttu-id="0bb54-124">W programie Visual Studio Utwórz aplikację sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="0bb54-124">In Visual Studio, create an ASP.NET Web Application.</span></span>

    ![Tworzenie sieci Web](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

1. <span data-ttu-id="0bb54-126">W oknie **Nowa aplikacja sieci Web ASP.NET — MoveShapeDemo** , pozostaw **puste** zaznaczone i wybierz **przycisk OK**.</span><span class="sxs-lookup"><span data-stu-id="0bb54-126">In the **New ASP.NET Web Application - MoveShapeDemo** window, leave **Empty** selected and select **OK**.</span></span>

1. <span data-ttu-id="0bb54-127">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt, a następnie wybierz pozycję **Dodaj** > **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="0bb54-127">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="0bb54-128">W obszarze **Dodaj nowy element — MoveShapeDemo**wybierz pozycję **zainstalowane** > **Visual C#**  > **sieci Web** > **sygnalizujący** , a następnie wybierz pozycję **Klasa centrum sygnałów (v2)** .</span><span class="sxs-lookup"><span data-stu-id="0bb54-128">In **Add New Item - MoveShapeDemo**, select **Installed** > **Visual C#** > **Web** > **SignalR**  and then select **SignalR Hub Class (v2)**.</span></span>

1. <span data-ttu-id="0bb54-129">Nadaj klasie nazwę *MoveShapeHub* i Dodaj ją do projektu.</span><span class="sxs-lookup"><span data-stu-id="0bb54-129">Name the class *MoveShapeHub* and add it to the project.</span></span>

    <span data-ttu-id="0bb54-130">Ten krok powoduje utworzenie pliku klasy *MoveShapeHub.cs* .</span><span class="sxs-lookup"><span data-stu-id="0bb54-130">This step creates the *MoveShapeHub.cs* class file.</span></span> <span data-ttu-id="0bb54-131">Jednocześnie dodaje zestaw plików skryptów i odwołania do zestawów, które obsługują sygnalizujący do projektu.</span><span class="sxs-lookup"><span data-stu-id="0bb54-131">Simultaneously, it adds  a set of script files and assembly references that support SignalR to the project.</span></span>

1. <span data-ttu-id="0bb54-132">Wybierz kolejno pozycje **narzędzia** > **menedżer pakietów NuGet** > **konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="0bb54-132">Select **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>

1. <span data-ttu-id="0bb54-133">W **konsoli Menedżera pakietów**Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="0bb54-133">In **Package Manager Console**, run this command:</span></span>

    ```console
    Install-Package jQuery.UI.Combined
    ```

    <span data-ttu-id="0bb54-134">Polecenie instaluje bibliotekę interfejsu użytkownika jQuery.</span><span class="sxs-lookup"><span data-stu-id="0bb54-134">The command installs the jQuery UI library.</span></span> <span data-ttu-id="0bb54-135">Służy do animowania kształtu.</span><span class="sxs-lookup"><span data-stu-id="0bb54-135">You use it to animate the shape.</span></span>

1. <span data-ttu-id="0bb54-136">W **Eksplorator rozwiązań**rozwiń węzeł skrypty.</span><span class="sxs-lookup"><span data-stu-id="0bb54-136">In **Solution Explorer**, expand the Scripts node.</span></span>

    ![Odwołania do biblioteki skryptów](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)

    <span data-ttu-id="0bb54-138">Biblioteki skryptów dla jQuery, jQueryUI i sygnalizujący są widoczne w projekcie.</span><span class="sxs-lookup"><span data-stu-id="0bb54-138">Script libraries for jQuery, jQueryUI, and SignalR are visible in the project.</span></span>

## <a name="create-the-base-application"></a><span data-ttu-id="0bb54-139">Tworzenie aplikacji podstawowej</span><span class="sxs-lookup"><span data-stu-id="0bb54-139">Create the base application</span></span>

<span data-ttu-id="0bb54-140">W tej sekcji utworzysz aplikację przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="0bb54-140">In this section, you create a browser application.</span></span> <span data-ttu-id="0bb54-141">Aplikacja wysyła lokalizację kształtu do serwera podczas każdego zdarzenia przenoszenia myszy.</span><span class="sxs-lookup"><span data-stu-id="0bb54-141">The app sends the location of the shape to the server during each mouse move event.</span></span> <span data-ttu-id="0bb54-142">Serwer emituje te informacje do wszystkich innych podłączonych klientów w czasie rzeczywistym.</span><span class="sxs-lookup"><span data-stu-id="0bb54-142">The server broadcasts this information to all other connected clients in real time.</span></span> <span data-ttu-id="0bb54-143">Więcej informacji na temat tej aplikacji znajdziesz w kolejnych sekcjach.</span><span class="sxs-lookup"><span data-stu-id="0bb54-143">You learn more about this application in later sections.</span></span>

1. <span data-ttu-id="0bb54-144">Otwórz plik *MoveShapeHub.cs* .</span><span class="sxs-lookup"><span data-stu-id="0bb54-144">Open the *MoveShapeHub.cs* file.</span></span>

1. <span data-ttu-id="0bb54-145">Zastąp kod w pliku *MoveShapeHub.cs* tym kodem:</span><span class="sxs-lookup"><span data-stu-id="0bb54-145">Replace the code in the *MoveShapeHub.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.cs)]

1. <span data-ttu-id="0bb54-146">Zapisz plik.</span><span class="sxs-lookup"><span data-stu-id="0bb54-146">Save the file.</span></span>

<span data-ttu-id="0bb54-147">Klasa `MoveShapeHub` jest implementacją centrum sygnałów.</span><span class="sxs-lookup"><span data-stu-id="0bb54-147">The `MoveShapeHub` class is an implementation of a SignalR hub.</span></span> <span data-ttu-id="0bb54-148">Podobnie jak w przypadku [wprowadzenie z samouczkiem sygnalizującym](tutorial-getting-started-with-signalr.md) , koncentrator ma metodę, którą klienci bezpośrednio wywołują.</span><span class="sxs-lookup"><span data-stu-id="0bb54-148">As in the [Getting Started with SignalR](tutorial-getting-started-with-signalr.md) tutorial, the hub has a method that the clients call directly.</span></span> <span data-ttu-id="0bb54-149">W takim przypadku klient wysyła obiekt z nowymi współrzędnymi X i Y kształtu do serwera.</span><span class="sxs-lookup"><span data-stu-id="0bb54-149">In this case, the client sends an object with the new X and Y coordinates of the shape to the server.</span></span> <span data-ttu-id="0bb54-150">Te współrzędne są wysyłane do wszystkich innych podłączonych klientów.</span><span class="sxs-lookup"><span data-stu-id="0bb54-150">Those coordinates get broadcasted to all other connected clients.</span></span> <span data-ttu-id="0bb54-151">Program sygnalizujący automatycznie serializować ten obiekt przy użyciu formatu JSON.</span><span class="sxs-lookup"><span data-stu-id="0bb54-151">SignalR automatically serializes this object using JSON.</span></span>

<span data-ttu-id="0bb54-152">Aplikacja wysyła do klienta obiekt `ShapeModel`.</span><span class="sxs-lookup"><span data-stu-id="0bb54-152">The app sends the `ShapeModel` object to the client.</span></span> <span data-ttu-id="0bb54-153">Ma elementy członkowskie do przechowywania pozycji kształtu.</span><span class="sxs-lookup"><span data-stu-id="0bb54-153">It has members to store the position of the shape.</span></span> <span data-ttu-id="0bb54-154">Wersja obiektu na serwerze ma również element członkowski służący do śledzenia, które dane klienta są przechowywane.</span><span class="sxs-lookup"><span data-stu-id="0bb54-154">The version of the object on the server also has a member to track which client's data is being stored.</span></span> <span data-ttu-id="0bb54-155">Ten obiekt uniemożliwia serwerowi wysyłanie danych klienta z powrotem do samego siebie.</span><span class="sxs-lookup"><span data-stu-id="0bb54-155">This object prevents the server from sending a client's data back to itself.</span></span> <span data-ttu-id="0bb54-156">Ten element członkowski używa atrybutu `JsonIgnore`, aby uniemożliwić aplikacji Serializowanie danych i wysłanie jej z powrotem do klienta.</span><span class="sxs-lookup"><span data-stu-id="0bb54-156">This member uses the `JsonIgnore` attribute to keep the application from serializing the data and sending it back to the client.</span></span>

## <a name="map-to-the-hub-when-app-starts"></a><span data-ttu-id="0bb54-157">Mapuj do centrum podczas uruchamiania aplikacji</span><span class="sxs-lookup"><span data-stu-id="0bb54-157">Map to the hub when app starts</span></span>

<span data-ttu-id="0bb54-158">Następnie należy skonfigurować mapowanie do centrum podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0bb54-158">Next, you set up mapping to the hub when the application starts.</span></span> <span data-ttu-id="0bb54-159">W sygnalizacji 2 dodanie klasy uruchomieniowej OWIN powoduje utworzenie mapowania.</span><span class="sxs-lookup"><span data-stu-id="0bb54-159">In SignalR 2, adding an OWIN startup class creates the mapping.</span></span>

1. <span data-ttu-id="0bb54-160">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt, a następnie wybierz pozycję **Dodaj** > **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="0bb54-160">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="0bb54-161">W obszarze **Dodaj nowy element — MoveShapeDemo** wybierz pozycję **zainstalowane** >  **C# Visual** > **Web** , a następnie wybierz pozycję **Owin klasy startowej**.</span><span class="sxs-lookup"><span data-stu-id="0bb54-161">In **Add New Item - MoveShapeDemo** select **Installed** > **Visual C#** > **Web** and then select **OWIN Startup Class**.</span></span>

1. <span data-ttu-id="0bb54-162">Nazwij klasę *uruchamiania* i wybierz **przycisk OK**.</span><span class="sxs-lookup"><span data-stu-id="0bb54-162">Name the class *Startup* and select **OK**.</span></span>

1. <span data-ttu-id="0bb54-163">Zastąp domyślny kod w pliku *Startup.cs* tym kodem:</span><span class="sxs-lookup"><span data-stu-id="0bb54-163">Replace the default code in the *Startup.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.cs)]

<span data-ttu-id="0bb54-164">Klasa uruchomieniowa OWIN wywołuje `MapSignalR`, gdy aplikacja wykonuje `Configuration` metodę.</span><span class="sxs-lookup"><span data-stu-id="0bb54-164">The OWIN startup class calls `MapSignalR` when the app executes the `Configuration` method.</span></span> <span data-ttu-id="0bb54-165">Aplikacja dodaje klasę do procesu uruchamiania OWIN przy użyciu atrybutu zestawu `OwinStartup`.</span><span class="sxs-lookup"><span data-stu-id="0bb54-165">The app adds the class to OWIN's startup process using the `OwinStartup` assembly attribute.</span></span>

## <a name="add-the-client"></a><span data-ttu-id="0bb54-166">Dodawanie klienta</span><span class="sxs-lookup"><span data-stu-id="0bb54-166">Add the client</span></span>

<span data-ttu-id="0bb54-167">Dodaj stronę HTML dla klienta.</span><span class="sxs-lookup"><span data-stu-id="0bb54-167">Add the HTML page for the client.</span></span>

1. <span data-ttu-id="0bb54-168">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt i wybierz polecenie **Dodaj** > **stronę HTML**.</span><span class="sxs-lookup"><span data-stu-id="0bb54-168">In **Solution Explorer**, right-click the project and select **Add** > **HTML Page**.</span></span>

1. <span data-ttu-id="0bb54-169">Nadaj nazwę stronie **domyślne** i wybierz przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="0bb54-169">Name the page **Default** and select **OK**.</span></span>

1. <span data-ttu-id="0bb54-170">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy pozycję *default. html* i wybierz pozycję **Ustaw jako stronę startową**.</span><span class="sxs-lookup"><span data-stu-id="0bb54-170">In **Solution Explorer**, right-click *Default.html* and select **Set as Start Page**.</span></span>

1. <span data-ttu-id="0bb54-171">Zastąp domyślny kod w pliku *default. html* tym kodem:</span><span class="sxs-lookup"><span data-stu-id="0bb54-171">Replace the default code in the *Default.html* file with this code:</span></span>

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.html?highlight=14-16)]

1. <span data-ttu-id="0bb54-172">W **Eksplorator rozwiązań**rozwiń węzeł **skrypty**.</span><span class="sxs-lookup"><span data-stu-id="0bb54-172">In **Solution Explorer**, expand **Scripts**.</span></span>

    <span data-ttu-id="0bb54-173">Biblioteki skryptów dla jQuery i sygnalizujących są widoczne w projekcie.</span><span class="sxs-lookup"><span data-stu-id="0bb54-173">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="0bb54-174">Menedżer pakietów instaluje nowszą wersję skryptów sygnalizujących.</span><span class="sxs-lookup"><span data-stu-id="0bb54-174">The package manager installs a later version of the SignalR scripts.</span></span>

1. <span data-ttu-id="0bb54-175">Zaktualizuj odwołania do skryptu w bloku kodu, aby odpowiadały wersji plików skryptów w projekcie.</span><span class="sxs-lookup"><span data-stu-id="0bb54-175">Update the script references in the code block to correspond to the versions of the script files in the project.</span></span>

<span data-ttu-id="0bb54-176">Ten kod HTML i JavaScript tworzy czerwoną `div` o nazwie `shape`.</span><span class="sxs-lookup"><span data-stu-id="0bb54-176">This HTML and JavaScript code creates a red `div` called `shape`.</span></span> <span data-ttu-id="0bb54-177">Umożliwia ona zachowanie przeciągnięcia kształtu przy użyciu biblioteki jQuery i użycie zdarzenia `drag` do wysłania położenia kształtu do serwera.</span><span class="sxs-lookup"><span data-stu-id="0bb54-177">It enables the shape's dragging behavior using the jQuery library and uses the `drag` event to send the shape's position to the server.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="0bb54-178">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="0bb54-178">Run the app</span></span>

<span data-ttu-id="0bb54-179">Możesz uruchomić aplikację, aby se'e jej działanie.</span><span class="sxs-lookup"><span data-stu-id="0bb54-179">You can run the app to se\`e it work.</span></span> <span data-ttu-id="0bb54-180">Po przeciągnięciu kształtu wokół okna przeglądarki, kształt jest również przenoszony w innych przeglądarkach.</span><span class="sxs-lookup"><span data-stu-id="0bb54-180">When you drag the shape around a browser window, the shape moves in the other browsers too.</span></span>

1. <span data-ttu-id="0bb54-181">Na pasku narzędzi Włącz **debugowanie skryptów** , a następnie wybierz przycisk Odtwórz, aby uruchomić aplikację w trybie debugowania.</span><span class="sxs-lookup"><span data-stu-id="0bb54-181">In the toolbar, turn on **Script Debugging** and then select the play button to run the application in Debug mode.</span></span>

    ![Zrzut ekranu przedstawiający Włączanie trybu debugowania przez użytkownika i wybieranie opcji Odtwórz.](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)

    <span data-ttu-id="0bb54-183">Zostanie otwarte okno przeglądarki z czerwonym kształtem w prawym górnym rogu.</span><span class="sxs-lookup"><span data-stu-id="0bb54-183">A browser window opens with the red shape in the upper-right corner.</span></span>

1. <span data-ttu-id="0bb54-184">Skopiuj adres URL strony.</span><span class="sxs-lookup"><span data-stu-id="0bb54-184">Copy the page's URL.</span></span>

1. <span data-ttu-id="0bb54-185">Otwórz kolejną przeglądarkę i wklej adres URL na pasku adresu.</span><span class="sxs-lookup"><span data-stu-id="0bb54-185">Open another browser and paste the URL into the address bar.</span></span>

1. <span data-ttu-id="0bb54-186">Przeciągnij kształt w jednym z okien przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="0bb54-186">Drag the shape in one of the browser windows.</span></span> <span data-ttu-id="0bb54-187">Poniższy kształt w drugim oknie przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="0bb54-187">The shape in the other browser window follows.</span></span>

<span data-ttu-id="0bb54-188">Gdy aplikacja korzysta z tej metody, nie jest to zalecany model programowania.</span><span class="sxs-lookup"><span data-stu-id="0bb54-188">While the application functions using this method, it's not a recommended programming model.</span></span> <span data-ttu-id="0bb54-189">Nie ma górnego limitu liczby wysyłanych komunikatów.</span><span class="sxs-lookup"><span data-stu-id="0bb54-189">There's no upper limit to the number of messages getting sent.</span></span> <span data-ttu-id="0bb54-190">W związku z tym klienci i serwery są przeciążone komunikatami i obniżeniem wydajności.</span><span class="sxs-lookup"><span data-stu-id="0bb54-190">As a result, the clients and server get overwhelmed with messages and performance degrades.</span></span> <span data-ttu-id="0bb54-191">Ponadto aplikacja wyświetla rozłączne animacje na kliencie.</span><span class="sxs-lookup"><span data-stu-id="0bb54-191">Also, the app displays a disjointed animation on the client.</span></span> <span data-ttu-id="0bb54-192">Ta animacja Jerky ma miejsce, ponieważ kształt jest przesuwany natychmiast przez każdą metodę.</span><span class="sxs-lookup"><span data-stu-id="0bb54-192">This jerky animation happens because the shape moves instantly by each method.</span></span> <span data-ttu-id="0bb54-193">Lepiej, jeśli kształt zostanie bezproblemowo przeniesiony do każdej nowej lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="0bb54-193">It's better if the shape moves smoothly to each new location.</span></span> <span data-ttu-id="0bb54-194">Następnie dowiesz się, jak rozwiązać te problemy.</span><span class="sxs-lookup"><span data-stu-id="0bb54-194">Next, you learn how to fix those issues.</span></span>

## <a name="add-the-client-loop"></a><span data-ttu-id="0bb54-195">Dodaj pętlę klienta</span><span class="sxs-lookup"><span data-stu-id="0bb54-195">Add the client loop</span></span>

<span data-ttu-id="0bb54-196">Wysyłanie lokalizacji kształtu przy każdym zdarzeniu przenoszenia myszy powoduje utworzenie niepotrzebnego ruchu sieciowego.</span><span class="sxs-lookup"><span data-stu-id="0bb54-196">Sending the location of the shape on every mouse move event creates an unnecessary amount of network traffic.</span></span> <span data-ttu-id="0bb54-197">Aplikacja musi ograniczyć komunikaty z klienta.</span><span class="sxs-lookup"><span data-stu-id="0bb54-197">The app needs to throttle the messages from the client.</span></span>

<span data-ttu-id="0bb54-198">Użyj funkcji `setInterval` JavaScript, aby skonfigurować pętlę, która wysyła nowe informacje o pozycji do serwera ze stałą stawką.</span><span class="sxs-lookup"><span data-stu-id="0bb54-198">Use the javascript `setInterval` function to set up a loop that sends new position information to the server at a fixed rate.</span></span> <span data-ttu-id="0bb54-199">Ta pętla to podstawowa reprezentacja "pętli gry".</span><span class="sxs-lookup"><span data-stu-id="0bb54-199">This loop is a basic representation of a "game loop."</span></span> <span data-ttu-id="0bb54-200">Jest to wielokrotnie wywoływana funkcja, która steruje wszystkimi funkcjami gry.</span><span class="sxs-lookup"><span data-stu-id="0bb54-200">It's a repeatedly called function that drives all the functionality of a game.</span></span>

1. <span data-ttu-id="0bb54-201">Zastąp kod klienta w pliku *default. html* tym kodem:</span><span class="sxs-lookup"><span data-stu-id="0bb54-201">Replace the client code in the *Default.html* file with this code:</span></span>

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.html?highlight=14-16)]

    > [!IMPORTANT]
    > <span data-ttu-id="0bb54-202">Należy ponownie zastąpić odwołania do skryptu.</span><span class="sxs-lookup"><span data-stu-id="0bb54-202">You have to replace the script references again.</span></span> <span data-ttu-id="0bb54-203">Muszą one być zgodne z wersjami skryptów w projekcie.</span><span class="sxs-lookup"><span data-stu-id="0bb54-203">They must match the versions of the scripts in the project.</span></span>

    <span data-ttu-id="0bb54-204">Ten nowy kod dodaje funkcję `updateServerModel`.</span><span class="sxs-lookup"><span data-stu-id="0bb54-204">This new code adds the `updateServerModel` function.</span></span> <span data-ttu-id="0bb54-205">Jest wywoływana z ustaloną częstotliwością.</span><span class="sxs-lookup"><span data-stu-id="0bb54-205">It gets called on a fixed frequency.</span></span> <span data-ttu-id="0bb54-206">Funkcja wysyła dane położenia do serwera za każdym razem, gdy flaga `moved` wskazuje, że nowe dane położenia są wysyłane.</span><span class="sxs-lookup"><span data-stu-id="0bb54-206">The function sends the position data to the server whenever the `moved` flag indicates that there's new position data to send.</span></span>

1. <span data-ttu-id="0bb54-207">Wybierz przycisk Odtwórz, aby uruchomić aplikację</span><span class="sxs-lookup"><span data-stu-id="0bb54-207">Select the play button to start the application</span></span>

1. <span data-ttu-id="0bb54-208">Skopiuj adres URL strony.</span><span class="sxs-lookup"><span data-stu-id="0bb54-208">Copy the page's URL.</span></span>

1. <span data-ttu-id="0bb54-209">Otwórz kolejną przeglądarkę i wklej adres URL na pasku adresu.</span><span class="sxs-lookup"><span data-stu-id="0bb54-209">Open another browser and paste the URL into the address bar.</span></span>

1. <span data-ttu-id="0bb54-210">Przeciągnij kształt w jednym z okien przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="0bb54-210">Drag the shape in one of the browser windows.</span></span> <span data-ttu-id="0bb54-211">Poniższy kształt w drugim oknie przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="0bb54-211">The shape in the other browser window follows.</span></span>

<span data-ttu-id="0bb54-212">Ponieważ aplikacja ogranicza liczbę komunikatów wysyłanych do serwera, animacja nie będzie wyświetlana jako gładka.</span><span class="sxs-lookup"><span data-stu-id="0bb54-212">Since the app throttles the number of messages that get sent to the server, the animation won't appear as smooth did at first.</span></span>

## <a name="add-the-server-loop"></a><span data-ttu-id="0bb54-213">Dodaj pętlę serwera</span><span class="sxs-lookup"><span data-stu-id="0bb54-213">Add the server loop</span></span>

<span data-ttu-id="0bb54-214">W bieżącej aplikacji komunikaty wysyłane z serwera do klienta są wykonywane tak często, jak są odbierane.</span><span class="sxs-lookup"><span data-stu-id="0bb54-214">In the current application, messages sent from the server to the client go out as often as they're received.</span></span> <span data-ttu-id="0bb54-215">Ten ruch sieciowy przedstawia podobny problem, który jest widoczny na kliencie.</span><span class="sxs-lookup"><span data-stu-id="0bb54-215">This network traffic presents a similar problem as we see on the client.</span></span>

<span data-ttu-id="0bb54-216">Aplikacja może wysyłać komunikaty częściej niż jest to zbędne.</span><span class="sxs-lookup"><span data-stu-id="0bb54-216">The app can send messages more often than they're needed.</span></span> <span data-ttu-id="0bb54-217">Połączenie może zostać zalanie w wyniku.</span><span class="sxs-lookup"><span data-stu-id="0bb54-217">The connection can become flooded as a result.</span></span> <span data-ttu-id="0bb54-218">W tej sekcji opisano sposób aktualizowania serwera programu w celu dodania czasomierza, który ogranicza szybkość wychodzących komunikatów.</span><span class="sxs-lookup"><span data-stu-id="0bb54-218">This section describes how to update the server to add a timer that throttles the rate of the outgoing messages.</span></span>

1. <span data-ttu-id="0bb54-219">Zastąp zawartość `MoveShapeHub.cs` tym kodem:</span><span class="sxs-lookup"><span data-stu-id="0bb54-219">Replace the contents of `MoveShapeHub.cs` with this code:</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

1. <span data-ttu-id="0bb54-220">Wybierz przycisk Odtwórz, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="0bb54-220">Select the play button to start the application.</span></span>

1. <span data-ttu-id="0bb54-221">Skopiuj adres URL strony.</span><span class="sxs-lookup"><span data-stu-id="0bb54-221">Copy the page's URL.</span></span>

1. <span data-ttu-id="0bb54-222">Otwórz kolejną przeglądarkę i wklej adres URL na pasku adresu.</span><span class="sxs-lookup"><span data-stu-id="0bb54-222">Open another browser and paste the URL into the address bar.</span></span>

1. <span data-ttu-id="0bb54-223">Przeciągnij kształt w jednym z okien przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="0bb54-223">Drag the shape in one of the browser windows.</span></span>

<span data-ttu-id="0bb54-224">Ten kod rozszerza klienta, aby dodać klasę `Broadcaster`.</span><span class="sxs-lookup"><span data-stu-id="0bb54-224">This code expands the client to add the `Broadcaster` class.</span></span> <span data-ttu-id="0bb54-225">Nowa Klasa ogranicza komunikaty wychodzące przy użyciu klasy `Timer` z programu .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="0bb54-225">The new class throttles the outgoing messages using the `Timer` class from the .NET framework.</span></span>

<span data-ttu-id="0bb54-226">Warto dowiedzieć się, że sam koncentrator jest przejściowy.</span><span class="sxs-lookup"><span data-stu-id="0bb54-226">It's good to learn that the hub itself is transitory.</span></span> <span data-ttu-id="0bb54-227">Jest on tworzony za każdym razem, gdy jest to konieczne.</span><span class="sxs-lookup"><span data-stu-id="0bb54-227">It's created every time it's needed.</span></span> <span data-ttu-id="0bb54-228">Aplikacja tworzy `Broadcaster` jako pojedynczą.</span><span class="sxs-lookup"><span data-stu-id="0bb54-228">So the app creates the `Broadcaster` as a singleton.</span></span> <span data-ttu-id="0bb54-229">Używa inicjalizacji z opóźnieniem, aby odroczyć tworzenie `Broadcaster`do momentu, gdy jest to konieczne.</span><span class="sxs-lookup"><span data-stu-id="0bb54-229">It uses lazy initialization to defer the `Broadcaster`'s creation until it's needed.</span></span> <span data-ttu-id="0bb54-230">Gwarantuje to, że aplikacja tworzy pierwsze wystąpienie centrum w całości przed rozpoczęciem czasomierza.</span><span class="sxs-lookup"><span data-stu-id="0bb54-230">That guarantees that the app creates the first hub instance completely before starting the timer.</span></span>

<span data-ttu-id="0bb54-231">Wywołanie funkcji `UpdateShape` klientów zostanie następnie przeniesione z metody `UpdateModel` centrum.</span><span class="sxs-lookup"><span data-stu-id="0bb54-231">The call to the clients' `UpdateShape` function is then moved out of the hub's `UpdateModel` method.</span></span> <span data-ttu-id="0bb54-232">Nie jest już wywoływana natychmiast po każdym odebraniu wiadomości przychodzących przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="0bb54-232">It's no longer called immediately whenever the app receives incoming messages.</span></span> <span data-ttu-id="0bb54-233">Zamiast tego aplikacja wysyła komunikaty do klientów z szybkością 25 wywołań na sekundę.</span><span class="sxs-lookup"><span data-stu-id="0bb54-233">Instead, the app sends the messages to the clients at a rate of 25 calls per second.</span></span> <span data-ttu-id="0bb54-234">Proces jest zarządzany przez `_broadcastLoop` Timer z poziomu klasy `Broadcaster`.</span><span class="sxs-lookup"><span data-stu-id="0bb54-234">The process is  managed by the `_broadcastLoop` timer from within the `Broadcaster` class.</span></span>

<span data-ttu-id="0bb54-235">Na koniec zamiast wywoływania metody klienta bezpośrednio z poziomu centrum Klasa `Broadcaster` musi uzyskać odwołanie do centrum `_hubContext` operacyjnego.</span><span class="sxs-lookup"><span data-stu-id="0bb54-235">Lastly, instead of calling the client method from the hub directly, the `Broadcaster` class needs to get a reference to the currently operating `_hubContext` hub.</span></span> <span data-ttu-id="0bb54-236">Pobiera odwołanie do `GlobalHost`.</span><span class="sxs-lookup"><span data-stu-id="0bb54-236">It gets the reference with the `GlobalHost`.</span></span>

## <a name="add-smooth-animation"></a><span data-ttu-id="0bb54-237">Dodaj gładką animację</span><span class="sxs-lookup"><span data-stu-id="0bb54-237">Add smooth animation</span></span>

<span data-ttu-id="0bb54-238">Aplikacja jest niemal zakończona, ale możemy wprowadzić jeszcze jedną poprawę.</span><span class="sxs-lookup"><span data-stu-id="0bb54-238">The application is almost finished, but we could make one more improvement.</span></span> <span data-ttu-id="0bb54-239">Aplikacja przenosi kształt na kliencie w odpowiedzi na komunikaty serwera.</span><span class="sxs-lookup"><span data-stu-id="0bb54-239">The app moves the shape on the client in response to server messages.</span></span> <span data-ttu-id="0bb54-240">Zamiast ustawiania położenia kształtu do nowej lokalizacji wskazanej przez serwer, użyj funkcji `animate` biblioteki interfejsu użytkownika JQuery.</span><span class="sxs-lookup"><span data-stu-id="0bb54-240">Instead of setting the position of the shape to the new location given by the server, use the JQuery UI library's `animate` function.</span></span> <span data-ttu-id="0bb54-241">Może bezproblemowo przenosić kształt między jego bieżącą i nową pozycją.</span><span class="sxs-lookup"><span data-stu-id="0bb54-241">It can move the shape smoothly between its current and new position.</span></span>

1. <span data-ttu-id="0bb54-242">Zaktualizuj metodę `updateShape` klienta w pliku *default. html* , aby wyglądać jak wyróżniony kod:</span><span class="sxs-lookup"><span data-stu-id="0bb54-242">Update the client's `updateShape` method in the *Default.html* file to look like the highlighted code:</span></span>

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.html?highlight=33-40)]

1. <span data-ttu-id="0bb54-243">Wybierz przycisk Odtwórz, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="0bb54-243">Select the play button to start the application.</span></span>

1. <span data-ttu-id="0bb54-244">Skopiuj adres URL strony.</span><span class="sxs-lookup"><span data-stu-id="0bb54-244">Copy the page's URL.</span></span>

1. <span data-ttu-id="0bb54-245">Otwórz kolejną przeglądarkę i wklej adres URL na pasku adresu.</span><span class="sxs-lookup"><span data-stu-id="0bb54-245">Open another browser and paste the URL into the address bar.</span></span>

1. <span data-ttu-id="0bb54-246">Przeciągnij kształt w jednym z okien przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="0bb54-246">Drag the shape in one of the browser windows.</span></span>

<span data-ttu-id="0bb54-247">Przenoszenie kształtu w drugim oknie jest mniej Jerky.</span><span class="sxs-lookup"><span data-stu-id="0bb54-247">The movement of the shape in the other window appears less jerky.</span></span> <span data-ttu-id="0bb54-248">Aplikacja interpoluje swój ruch w czasie, a nie ustawia raz na komunikat przychodzący.</span><span class="sxs-lookup"><span data-stu-id="0bb54-248">The app interpolates its movement over time rather than being set once per incoming message.</span></span>

<span data-ttu-id="0bb54-249">Ten kod przenosi kształt ze starej lokalizacji do nowej.</span><span class="sxs-lookup"><span data-stu-id="0bb54-249">This code moves the shape from the old location to the new one.</span></span> <span data-ttu-id="0bb54-250">Serwer podaje pozycję kształtu w trakcie interwału animacji.</span><span class="sxs-lookup"><span data-stu-id="0bb54-250">The server gives the position of the shape over the course of the animation interval.</span></span> <span data-ttu-id="0bb54-251">W tym przypadku jest to 100 milisekund.</span><span class="sxs-lookup"><span data-stu-id="0bb54-251">In this case, that's 100 milliseconds.</span></span> <span data-ttu-id="0bb54-252">Aplikacja czyści wszystkie poprzednie animacje uruchomione na kształcie przed rozpoczęciem nowej animacji.</span><span class="sxs-lookup"><span data-stu-id="0bb54-252">The app clears any previous animation running on the shape before the new animation starts.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="0bb54-253">Pobierz kod</span><span class="sxs-lookup"><span data-stu-id="0bb54-253">Get the code</span></span>

[<span data-ttu-id="0bb54-254">Pobierz ukończony projekt</span><span class="sxs-lookup"><span data-stu-id="0bb54-254">Download Completed Project</span></span>](https://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a)

## <a name="additional-resources"></a><span data-ttu-id="0bb54-255">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="0bb54-255">Additional resources</span></span>

<span data-ttu-id="0bb54-256">Właśnie zdobyty model komunikacji jest przydatny do tworzenia gier online i innych symulacji, takich jak [gra w ramach programu do tworzenia sygnałów](https://shootr.azurewebsites.net/).</span><span class="sxs-lookup"><span data-stu-id="0bb54-256">The communication paradigm you just learned about is useful for developing online games and other simulations, like [the ShootR game created with SignalR](https://shootr.azurewebsites.net/).</span></span>

<span data-ttu-id="0bb54-257">Aby uzyskać więcej informacji o sygnalizacji, zobacz następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="0bb54-257">For more about SignalR, see the following resources:</span></span>

* [<span data-ttu-id="0bb54-258">Projekt sygnalizującego</span><span class="sxs-lookup"><span data-stu-id="0bb54-258">SignalR Project</span></span>](http://signalr.net)

* [<span data-ttu-id="0bb54-259">Usługi GitHub i przykłady dla sygnałów</span><span class="sxs-lookup"><span data-stu-id="0bb54-259">SignalR GitHub and Samples</span></span>](https://github.com/SignalR/SignalR)

* [<span data-ttu-id="0bb54-260">Strona typu wiki sygnalizująca</span><span class="sxs-lookup"><span data-stu-id="0bb54-260">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a><span data-ttu-id="0bb54-261">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="0bb54-261">Next steps</span></span>

<span data-ttu-id="0bb54-262">W tym samouczku przedstawiono następujące instrukcje:</span><span class="sxs-lookup"><span data-stu-id="0bb54-262">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="0bb54-263">Konfigurowanie projektu</span><span class="sxs-lookup"><span data-stu-id="0bb54-263">Set up the project</span></span>
> * <span data-ttu-id="0bb54-264">Utworzono aplikację podstawową</span><span class="sxs-lookup"><span data-stu-id="0bb54-264">Created the base application</span></span>
> * <span data-ttu-id="0bb54-265">Zamapowane do centrum podczas uruchamiania aplikacji</span><span class="sxs-lookup"><span data-stu-id="0bb54-265">Mapped to the hub when app starts</span></span>
> * <span data-ttu-id="0bb54-266">Dodano klienta</span><span class="sxs-lookup"><span data-stu-id="0bb54-266">Added the client</span></span>
> * <span data-ttu-id="0bb54-267">Uruchomiono aplikację</span><span class="sxs-lookup"><span data-stu-id="0bb54-267">Ran the app</span></span>
> * <span data-ttu-id="0bb54-268">Dodano pętlę klienta</span><span class="sxs-lookup"><span data-stu-id="0bb54-268">Added the client loop</span></span>
> * <span data-ttu-id="0bb54-269">Dodano pętlę serwera</span><span class="sxs-lookup"><span data-stu-id="0bb54-269">Added the server loop</span></span>
> * <span data-ttu-id="0bb54-270">Dodano gładką animację</span><span class="sxs-lookup"><span data-stu-id="0bb54-270">Added smooth animation</span></span>

<span data-ttu-id="0bb54-271">Przejdź do następnego artykułu, aby dowiedzieć się, jak utworzyć aplikację sieci Web, która korzysta z ASP.NET sygnalizującego 2, aby zapewnić funkcję emisji serwera.</span><span class="sxs-lookup"><span data-stu-id="0bb54-271">Advance to the next article to learn how to create a web application that uses ASP.NET SignalR 2 to provide server broadcast functionality.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="0bb54-272">Sygnalizacja 2 i emisja serwera</span><span class="sxs-lookup"><span data-stu-id="0bb54-272">SignalR 2 and server broadcasting</span></span>](tutorial-server-broadcast-with-signalr.md)
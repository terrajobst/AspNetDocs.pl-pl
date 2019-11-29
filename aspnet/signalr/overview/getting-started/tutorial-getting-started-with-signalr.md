---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr
title: 'Samouczek: rozmowa w czasie rzeczywistym z sygnałem 2 | Microsoft Docs'
author: bradygaster
description: Ten samouczek pokazuje, jak utworzyć aplikację do obsługi rozmów w czasie rzeczywistym przy użyciu SignalR. Dodajesz sygnał do pustej aplikacji sieci Web ASP.NET.
ms.author: bradyg
ms.date: 01/22/2019
ms.assetid: a8b3b778-f009-4369-85c7-e90f9878d8b4
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: bc4ef190b6e36812b6fe7ca4e16eb763431e0e82
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600470"
---
# <a name="tutorial-real-time-chat-with-signalr-2"></a><span data-ttu-id="2461e-104">Samouczek: rozmowa w czasie rzeczywistym z sygnałem 2</span><span class="sxs-lookup"><span data-stu-id="2461e-104">Tutorial: Real-time chat with SignalR 2</span></span>

<span data-ttu-id="2461e-105">W tym samouczku pokazano, jak za pomocą programu sygnalizujący utworzyć aplikację czatu w czasie rzeczywistym.</span><span class="sxs-lookup"><span data-stu-id="2461e-105">This tutorial shows you how to use SignalR to create a real-time chat application.</span></span> <span data-ttu-id="2461e-106">Dodajesz sygnał do pustej aplikacji sieci Web ASP.NET i utworzysz stronę HTML, aby wysyłać i wyświetlać komunikaty.</span><span class="sxs-lookup"><span data-stu-id="2461e-106">You add SignalR to an empty ASP.NET web application and create an HTML page to send and display messages.</span></span>

<span data-ttu-id="2461e-107">W tym samouczku przedstawiono następujące instrukcje:</span><span class="sxs-lookup"><span data-stu-id="2461e-107">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="2461e-108">Konfigurowanie projektu</span><span class="sxs-lookup"><span data-stu-id="2461e-108">Set up the project</span></span>
> * <span data-ttu-id="2461e-109">Uruchamianie przykładu</span><span class="sxs-lookup"><span data-stu-id="2461e-109">Run the sample</span></span>
> * <span data-ttu-id="2461e-110">Sprawdzanie kodu</span><span class="sxs-lookup"><span data-stu-id="2461e-110">Examine the code</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a><span data-ttu-id="2461e-111">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="2461e-111">Prerequisites</span></span>

* <span data-ttu-id="2461e-112">[Program Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) z **ASP.NET i programowaniem w sieci Web** .</span><span class="sxs-lookup"><span data-stu-id="2461e-112">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) with the **ASP.NET and web development** workload.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="2461e-113">Konfigurowanie projektu</span><span class="sxs-lookup"><span data-stu-id="2461e-113">Set up the Project</span></span>

<span data-ttu-id="2461e-114">W tej sekcji pokazano, jak za pomocą programu Visual Studio 2017 i sygnalizacji 2 utworzyć pustą aplikację sieci Web ASP.NET, dodać program sygnalizujący i utworzyć aplikację czatu.</span><span class="sxs-lookup"><span data-stu-id="2461e-114">This section shows how to use Visual Studio 2017 and SignalR 2 to create an empty ASP.NET web application, add SignalR, and create the chat application.</span></span>

1. <span data-ttu-id="2461e-115">W programie Visual Studio Utwórz aplikację sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="2461e-115">In Visual Studio, create an ASP.NET Web Application.</span></span>

    ![Tworzenie sieci Web](tutorial-getting-started-with-signalr/_static/image2.png)

1. <span data-ttu-id="2461e-117">W oknie **nowy ASP.NET Project-SignalRChat** , pozostaw **puste** zaznaczone i wybierz **przycisk OK**.</span><span class="sxs-lookup"><span data-stu-id="2461e-117">In the **New ASP.NET Project - SignalRChat** window, leave **Empty** selected and select **OK**.</span></span>

1. <span data-ttu-id="2461e-118">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt, a następnie wybierz pozycję **Dodaj** > **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="2461e-118">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="2461e-119">W obszarze **Dodaj nowy element — SignalRChat**wybierz pozycję **zainstalowane** > **Visual C#**  > **sieci Web** > **sygnalizujący** , a następnie wybierz pozycję **Klasa centrum sygnałów (v2)** .</span><span class="sxs-lookup"><span data-stu-id="2461e-119">In **Add New Item - SignalRChat**, select **Installed** > **Visual C#** > **Web** > **SignalR**  and then select **SignalR Hub Class (v2)**.</span></span>

1. <span data-ttu-id="2461e-120">Nadaj klasie nazwę *ChatHub* i Dodaj ją do projektu.</span><span class="sxs-lookup"><span data-stu-id="2461e-120">Name the class *ChatHub* and add it to the project.</span></span>

    <span data-ttu-id="2461e-121">Ten krok tworzy plik klasy *ChatHub.cs* i dodaje zestaw plików skryptów oraz odwołania do zestawów, które obsługują sygnalizujący do projektu.</span><span class="sxs-lookup"><span data-stu-id="2461e-121">This step creates the *ChatHub.cs* class file and adds a set of script files and assembly references that support SignalR to the project.</span></span>

1. <span data-ttu-id="2461e-122">Zastąp kod w nowym pliku klasy *ChatHub.cs* tym kodem:</span><span class="sxs-lookup"><span data-stu-id="2461e-122">Replace the code in the new *ChatHub.cs* class file with this code:</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]

1. <span data-ttu-id="2461e-123">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt, a następnie wybierz pozycję **Dodaj** > **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="2461e-123">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="2461e-124">W obszarze **Dodaj nowy element — SignalRChat** wybierz pozycję **zainstalowane** >  **C# Visual** > **Web** , a następnie wybierz pozycję **Owin klasy startowej**.</span><span class="sxs-lookup"><span data-stu-id="2461e-124">In **Add New Item - SignalRChat** select **Installed** > **Visual C#** > **Web**  and then select **OWIN Startup Class**.</span></span>

1. <span data-ttu-id="2461e-125">Nazwij klasę *uruchamiania* i Dodaj ją do projektu.</span><span class="sxs-lookup"><span data-stu-id="2461e-125">Name the class *Startup* and add it to the project.</span></span>

1. <span data-ttu-id="2461e-126">Zastąp domyślny kod w klasie *startowej* tym kodem:</span><span class="sxs-lookup"><span data-stu-id="2461e-126">Replace the default code in *Startup* class with this code:</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]

1. <span data-ttu-id="2461e-127">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt i wybierz polecenie **Dodaj** > **stronę HTML**.</span><span class="sxs-lookup"><span data-stu-id="2461e-127">In **Solution Explorer**, right-click the project and select **Add** > **HTML Page**.</span></span>

1. <span data-ttu-id="2461e-128">Nazwij nowy *indeks* strony i wybierz **przycisk OK**.</span><span class="sxs-lookup"><span data-stu-id="2461e-128">Name the new page *index* and select **OK**.</span></span>

1. <span data-ttu-id="2461e-129">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy UTWORZONĄ stronę HTML i wybierz polecenie **Ustaw jako stronę startową**.</span><span class="sxs-lookup"><span data-stu-id="2461e-129">In **Solution Explorer**, right-click the HTML page you created and select **Set as Start Page**.</span></span>

1. <span data-ttu-id="2461e-130">Zastąp domyślny kod na stronie HTML tym kodem:</span><span class="sxs-lookup"><span data-stu-id="2461e-130">Replace the default code in the HTML page with this code:</span></span>

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample3.html)]

1. <span data-ttu-id="2461e-131">W **Eksplorator rozwiązań**rozwiń węzeł **skrypty**.</span><span class="sxs-lookup"><span data-stu-id="2461e-131">In **Solution Explorer**, expand **Scripts**.</span></span>

    <span data-ttu-id="2461e-132">Biblioteki skryptów dla jQuery i sygnalizujących są widoczne w projekcie.</span><span class="sxs-lookup"><span data-stu-id="2461e-132">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="2461e-133">Menedżer pakietów mógł zainstalować nowszą wersję skryptów sygnalizującego.</span><span class="sxs-lookup"><span data-stu-id="2461e-133">The package manager may have installed a later version of the SignalR scripts.</span></span>

1. <span data-ttu-id="2461e-134">Sprawdź, czy odwołania do skryptu w bloku kodu odpowiadają wersji plików skryptów w projekcie.</span><span class="sxs-lookup"><span data-stu-id="2461e-134">Check that the script references in the code block correspond to the versions of the script files in the project.</span></span>

    <span data-ttu-id="2461e-135">Odwołania do skryptu z oryginalnego bloku kodu:</span><span class="sxs-lookup"><span data-stu-id="2461e-135">Script references from the original code block:</span></span>

    ```html
    <!--Script references. -->
    <!--Reference the jQuery library. -->
    <script src="Scripts/jquery-3.1.1.min.js" ></script>
    <!--Reference the SignalR library. -->
    <script src="Scripts/jquery.signalR-2.2.1.min.js"></script>
    ```

1. <span data-ttu-id="2461e-136">Jeśli nie są zgodne, zaktualizuj plik *. html* .</span><span class="sxs-lookup"><span data-stu-id="2461e-136">If they don't match, update the *.html* file.</span></span>

1. <span data-ttu-id="2461e-137">Na pasku menu wybierz pozycję **plik** > **Zapisz wszystko**.</span><span class="sxs-lookup"><span data-stu-id="2461e-137">From the menu bar, select **File** > **Save All**.</span></span>

## <a name="run-the-sample"></a><span data-ttu-id="2461e-138">Uruchamianie przykładu</span><span class="sxs-lookup"><span data-stu-id="2461e-138">Run the Sample</span></span>

1. <span data-ttu-id="2461e-139">Na pasku narzędzi Włącz **debugowanie skryptów** , a następnie wybierz przycisk Odtwórz, aby uruchomić przykład w trybie debugowania.</span><span class="sxs-lookup"><span data-stu-id="2461e-139">In the toolbar, turn on **Script Debugging** and then select the play button to run the sample in Debug mode.</span></span>

    ![Wprowadź nazwę użytkownika](tutorial-getting-started-with-signalr/_static/image3.png)

1. <span data-ttu-id="2461e-141">Po otwarciu przeglądarki wprowadź nazwę swojej tożsamości rozmowy.</span><span class="sxs-lookup"><span data-stu-id="2461e-141">When the browser opens, enter a name for your chat identity.</span></span>

1. <span data-ttu-id="2461e-142">Skopiuj adres URL z przeglądarki, Otwórz dwie inne przeglądarki i wklej adresy URL do pasków adresów.</span><span class="sxs-lookup"><span data-stu-id="2461e-142">Copy the URL from the browser, open two other browsers, and paste the URLs into the address bars.</span></span>

1. <span data-ttu-id="2461e-143">W każdej przeglądarce wprowadź unikatową nazwę.</span><span class="sxs-lookup"><span data-stu-id="2461e-143">In each browser, enter a unique name.</span></span>

1. <span data-ttu-id="2461e-144">Teraz Dodaj komentarz i wybierz pozycję **Wyślij**.</span><span class="sxs-lookup"><span data-stu-id="2461e-144">Now, add a comment and select **Send**.</span></span> <span data-ttu-id="2461e-145">Powtórz te czynności w innych przeglądarkach.</span><span class="sxs-lookup"><span data-stu-id="2461e-145">Repeat that in the other browsers.</span></span> <span data-ttu-id="2461e-146">Komentarze pojawiają się w czasie rzeczywistym.</span><span class="sxs-lookup"><span data-stu-id="2461e-146">The comments appear in real-time.</span></span>

    > [!NOTE]
    > <span data-ttu-id="2461e-147">Ta prosta aplikacja czatu nie utrzymuje kontekstu dyskusji na serwerze.</span><span class="sxs-lookup"><span data-stu-id="2461e-147">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="2461e-148">Centrum emituje Komentarze do wszystkich bieżących użytkowników.</span><span class="sxs-lookup"><span data-stu-id="2461e-148">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="2461e-149">Użytkownicy, którzy dołączą do rozmowy później zobaczą komunikaty dodawane od momentu dołączenia.</span><span class="sxs-lookup"><span data-stu-id="2461e-149">Users who join the chat later will see messages added from the time they join.</span></span>

    <span data-ttu-id="2461e-150">Zobacz, jak działa aplikacja czatu w trzech różnych przeglądarkach.</span><span class="sxs-lookup"><span data-stu-id="2461e-150">See how the chat application runs in three different browsers.</span></span> <span data-ttu-id="2461e-151">Po wysłaniu komunikatów Anand i Susan wszystkie przeglądarki są aktualizowane w czasie rzeczywistym:</span><span class="sxs-lookup"><span data-stu-id="2461e-151">When Tom, Anand, and Susan send messages, all browsers update in real time:</span></span>

    ![Wszystkie trzy przeglądarki wyświetlają tę samą historię rozmowy](tutorial-getting-started-with-signalr/_static/image4.png)

1. <span data-ttu-id="2461e-153">W **Eksplorator rozwiązań**Sprawdź, czy węzeł **dokumenty skryptu** dla uruchomionej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2461e-153">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="2461e-154">Istnieje plik skryptu o nazwie *Hubs* , który jest generowany przez bibliotekę sygnałów w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="2461e-154">There's a script file named *hubs* that the SignalR library generates at runtime.</span></span> <span data-ttu-id="2461e-155">Ten plik zarządza komunikacją między skryptami jQuery i kodem po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="2461e-155">This file manages the communication between jQuery script and server-side code.</span></span>

    ![wygenerowany automatycznie skrypt centrów w węźle dokumenty skryptu](tutorial-getting-started-with-signalr/_static/image5.png)

## <a name="examine-the-code"></a><span data-ttu-id="2461e-157">Sprawdzanie kodu</span><span class="sxs-lookup"><span data-stu-id="2461e-157">Examine the Code</span></span>

<span data-ttu-id="2461e-158">Aplikacja SignalRChat demonstruje dwa podstawowe zadania programistyczne sygnalizujące.</span><span class="sxs-lookup"><span data-stu-id="2461e-158">The SignalRChat application demonstrates two basic SignalR development tasks.</span></span> <span data-ttu-id="2461e-159">Pokazano, jak utworzyć centrum.</span><span class="sxs-lookup"><span data-stu-id="2461e-159">It shows you how to create a hub.</span></span> <span data-ttu-id="2461e-160">Serwer używa tego centrum jako głównego obiektu koordynacji.</span><span class="sxs-lookup"><span data-stu-id="2461e-160">The server uses that hub as the main coordination object.</span></span> <span data-ttu-id="2461e-161">Do wysyłania i odbierania wiadomości centrum jest wykorzystywana Biblioteka sygnałów sygnalizujących.</span><span class="sxs-lookup"><span data-stu-id="2461e-161">The hub uses the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs-in-the-chathubcs"></a><span data-ttu-id="2461e-162">Centra sygnałów w ChatHub.cs</span><span class="sxs-lookup"><span data-stu-id="2461e-162">SignalR Hubs in the ChatHub.cs</span></span>

<span data-ttu-id="2461e-163">W powyższym przykładzie kodu Klasa `ChatHub` dziedziczy z klasy `Microsoft.AspNet.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="2461e-163">In the code sample above, the `ChatHub` class derives from the `Microsoft.AspNet.SignalR.Hub` class.</span></span> <span data-ttu-id="2461e-164">Wyprowadzanie z klasy `Hub` jest użytecznym sposobem tworzenia aplikacji sygnalizującej.</span><span class="sxs-lookup"><span data-stu-id="2461e-164">Deriving from the `Hub` class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="2461e-165">Metody publiczne można utworzyć w klasie centrów, a następnie użyć tych metod, wywołując je ze skryptów na stronie sieci Web.</span><span class="sxs-lookup"><span data-stu-id="2461e-165">You can create public methods on your hub class and then use those methods by calling them from scripts in a web page.</span></span>

<span data-ttu-id="2461e-166">W kodzie rozmowy klienci wywołują metodę `ChatHub.Send` w celu wysłania nowej wiadomości.</span><span class="sxs-lookup"><span data-stu-id="2461e-166">In the chat code, clients call the `ChatHub.Send` method to send a new message.</span></span> <span data-ttu-id="2461e-167">Następnie centrum wysyła komunikat do wszystkich klientów, wywołując `Clients.All.broadcastMessage`.</span><span class="sxs-lookup"><span data-stu-id="2461e-167">The hub then sends the message to all clients by calling `Clients.All.broadcastMessage`.</span></span>

<span data-ttu-id="2461e-168">Metoda `Send` ilustruje kilka pojęć centrum:</span><span class="sxs-lookup"><span data-stu-id="2461e-168">The `Send` method demonstrates several hub concepts:</span></span>

* <span data-ttu-id="2461e-169">Zadeklaruj metody publiczne w centrum, aby klienci mogli je wywoływać.</span><span class="sxs-lookup"><span data-stu-id="2461e-169">Declare public methods on a hub so that clients can call them.</span></span>

* <span data-ttu-id="2461e-170">Użyj właściwości dynamicznej `Microsoft.AspNet.SignalR.Hub.Clients`, aby komunikować się ze wszystkimi klientami podłączonymi do tego centrum.</span><span class="sxs-lookup"><span data-stu-id="2461e-170">Use the `Microsoft.AspNet.SignalR.Hub.Clients` dynamic property to communicate with all clients connected to this hub.</span></span>

* <span data-ttu-id="2461e-171">Wywołaj funkcję na kliencie (na przykład funkcję `broadcastMessage`), aby zaktualizować klientów.</span><span class="sxs-lookup"><span data-stu-id="2461e-171">Call a function on the client (like the `broadcastMessage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample4.cs)]

### <a name="signalr-and-jquery-in-the-indexhtml"></a><span data-ttu-id="2461e-172">Sygnalizujący i jQuery w pliku index. html</span><span class="sxs-lookup"><span data-stu-id="2461e-172">SignalR and jQuery in the index.html</span></span>

<span data-ttu-id="2461e-173">Na stronie *index. html* w przykładzie kodu pokazano, jak używać biblioteki sygnalizującej jQuery do komunikowania się z centrum sygnałów.</span><span class="sxs-lookup"><span data-stu-id="2461e-173">The *index.html* page in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="2461e-174">Kod wykonuje wiele ważnych zadań.</span><span class="sxs-lookup"><span data-stu-id="2461e-174">The code carries out many important tasks.</span></span> <span data-ttu-id="2461e-175">Deklaruje on serwer proxy do odwoływania się do centrum, deklaruje funkcję, którą serwer może wywoływać do wypychania zawartości do klientów i uruchamia połączenie w celu wysyłania komunikatów do centrum.</span><span class="sxs-lookup"><span data-stu-id="2461e-175">It declares a proxy to reference the hub, declares a function that the server can call to push content to clients, and it starts a connection to send messages to the hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample5.js)]

> [!NOTE]
> <span data-ttu-id="2461e-176">W języku JavaScript odwołanie do klasy serwera i jej elementów członkowskich musi być camelCase.</span><span class="sxs-lookup"><span data-stu-id="2461e-176">In JavaScript the reference to the server class and its members has to be camelCase.</span></span> <span data-ttu-id="2461e-177">Przykładowy kod odwołuje się C# do klasy *ChatHub* w języku JavaScript jako `chatHub`.</span><span class="sxs-lookup"><span data-stu-id="2461e-177">The code sample references the C# *ChatHub* class in JavaScript as `chatHub`.</span></span>

<span data-ttu-id="2461e-178">W tym bloku kodu utworzysz funkcję wywołania zwrotnego w skrypcie.</span><span class="sxs-lookup"><span data-stu-id="2461e-178">In this code block, you create a callback function in the script.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample6.html)]

<span data-ttu-id="2461e-179">Klasa centrum na serwerze wywołuje tę funkcję, aby wypchnąć aktualizacje zawartości do każdego klienta.</span><span class="sxs-lookup"><span data-stu-id="2461e-179">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="2461e-180">Te dwa wiersze, w których zawartość HTML jest zakodowana przed wyświetleniem, są opcjonalne i pokazują dobry sposób, aby zapobiec iniekcji skryptu.</span><span class="sxs-lookup"><span data-stu-id="2461e-180">The two lines that HTML-encode the content before displaying it are optional and show a good way to prevent script injection.</span></span>

<span data-ttu-id="2461e-181">Ten kod otwiera połączenie z centrum.</span><span class="sxs-lookup"><span data-stu-id="2461e-181">This code opens a connection with the hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample7.js)]

> [!NOTE]
> <span data-ttu-id="2461e-182">Takie podejście zapewnia, że kod nawiązuje połączenie przed wykonaniem procedury obsługi zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="2461e-182">This approach ensures that the code establishes a connection before the event handler executes.</span></span>

<span data-ttu-id="2461e-183">Kod uruchamia połączenie, a następnie przekazuje go do funkcji, aby obsłużyć zdarzenie kliknięcia na przycisku **Wyślij** na stronie HTML.</span><span class="sxs-lookup"><span data-stu-id="2461e-183">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the HTML page.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="2461e-184">Pobierz kod</span><span class="sxs-lookup"><span data-stu-id="2461e-184">Get the code</span></span>

[<span data-ttu-id="2461e-185">Pobierz ukończony projekt</span><span class="sxs-lookup"><span data-stu-id="2461e-185">Download Completed Project</span></span>](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9)

## <a name="additional-resources"></a><span data-ttu-id="2461e-186">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="2461e-186">Additional resources</span></span>

<span data-ttu-id="2461e-187">Aby uzyskać więcej informacji o sygnalizacji, zobacz następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="2461e-187">For more about SignalR, see the following resources:</span></span>

* [<span data-ttu-id="2461e-188">Projekt sygnalizującego</span><span class="sxs-lookup"><span data-stu-id="2461e-188">SignalR Project</span></span>](http://signalr.net)

* [<span data-ttu-id="2461e-189">Usługi GitHub i przykłady dla sygnałów</span><span class="sxs-lookup"><span data-stu-id="2461e-189">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)

* [<span data-ttu-id="2461e-190">Strona typu wiki sygnalizująca</span><span class="sxs-lookup"><span data-stu-id="2461e-190">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a><span data-ttu-id="2461e-191">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="2461e-191">Next steps</span></span>

<span data-ttu-id="2461e-192">W tym samouczku:</span><span class="sxs-lookup"><span data-stu-id="2461e-192">In this tutorial you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="2461e-193">Konfigurowanie projektu</span><span class="sxs-lookup"><span data-stu-id="2461e-193">Set up the project</span></span>
> * <span data-ttu-id="2461e-194">Uruchomiono przykład</span><span class="sxs-lookup"><span data-stu-id="2461e-194">Ran the sample</span></span>
> * <span data-ttu-id="2461e-195">Zbadano kod</span><span class="sxs-lookup"><span data-stu-id="2461e-195">Examined the code</span></span>

<span data-ttu-id="2461e-196">Przejdź do następnego artykułu, aby dowiedzieć się, jak używać sygnałów i MVC 5.</span><span class="sxs-lookup"><span data-stu-id="2461e-196">Advance to the next article to learn how to use SignalR and MVC 5.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="2461e-197">Sygnalizujący 2 i MVC 5</span><span class="sxs-lookup"><span data-stu-id="2461e-197">SignalR 2 and MVC 5</span></span>](tutorial-getting-started-with-signalr-and-mvc.md)
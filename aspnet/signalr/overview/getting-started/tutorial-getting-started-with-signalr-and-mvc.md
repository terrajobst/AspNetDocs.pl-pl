---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
title: 'Samouczek: rozmowa w czasie rzeczywistym z sygnałami 2 i MVC 5 | Microsoft Docs'
author: bradygaster
description: W tym samouczku pokazano, jak za pomocą ASP.NET sygnalizujący 2 utworzyć aplikację czatu w czasie rzeczywistym. Dodajesz sygnał do aplikacji MVC 5.
ms.author: bradyg
ms.date: 01/22/2019
ms.assetid: 80bfe5fb-bdfc-41fe-ac43-2132e5d69fac
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: 5671e4f0123ca2b0cb5314336cf4411467feac70
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600484"
---
# <a name="tutorial-real-time-chat-with-signalr-2-and-mvc-5"></a><span data-ttu-id="eb467-104">Samouczek: rozmowa w czasie rzeczywistym z sygnałami 2 i MVC 5</span><span class="sxs-lookup"><span data-stu-id="eb467-104">Tutorial: Real-time chat with SignalR 2 and MVC 5</span></span>

<span data-ttu-id="eb467-105">W tym samouczku pokazano, jak za pomocą ASP.NET sygnalizujący 2 utworzyć aplikację czatu w czasie rzeczywistym.</span><span class="sxs-lookup"><span data-stu-id="eb467-105">This tutorial shows how to use ASP.NET SignalR 2 to create a real-time chat application.</span></span> <span data-ttu-id="eb467-106">Dodajesz sygnalizację do aplikacji MVC 5 i utworzysz widok rozmowy w celu wysyłania i wyświetlania komunikatów.</span><span class="sxs-lookup"><span data-stu-id="eb467-106">You add SignalR to an MVC 5 application and create a chat view to send and display messages.</span></span>

<span data-ttu-id="eb467-107">W tym samouczku przedstawiono następujące instrukcje:</span><span class="sxs-lookup"><span data-stu-id="eb467-107">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="eb467-108">Konfigurowanie projektu</span><span class="sxs-lookup"><span data-stu-id="eb467-108">Set up the project</span></span>
> * <span data-ttu-id="eb467-109">Uruchamianie przykładu</span><span class="sxs-lookup"><span data-stu-id="eb467-109">Run the sample</span></span>
> * <span data-ttu-id="eb467-110">Sprawdzanie kodu</span><span class="sxs-lookup"><span data-stu-id="eb467-110">Examine the code</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a><span data-ttu-id="eb467-111">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="eb467-111">Prerequisites</span></span>

* <span data-ttu-id="eb467-112">[Program Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) z **ASP.NET i programowaniem w sieci Web** .</span><span class="sxs-lookup"><span data-stu-id="eb467-112">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) with the **ASP.NET and web development** workload.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="eb467-113">Konfigurowanie projektu</span><span class="sxs-lookup"><span data-stu-id="eb467-113">Set up the Project</span></span>

<span data-ttu-id="eb467-114">W tej sekcji pokazano, jak za pomocą programu Visual Studio 2017 i sygnalizacji 2 utworzyć pustą aplikację ASP.NET MVC 5, dodać bibliotekę sygnałów i utworzyć aplikację czatu.</span><span class="sxs-lookup"><span data-stu-id="eb467-114">This section shows how to use Visual Studio 2017 and SignalR 2 to create an empty ASP.NET MVC 5 application, add the SignalR library, and create the chat application.</span></span>

1. <span data-ttu-id="eb467-115">W programie Visual Studio Utwórz aplikację C# ASP.NET, która jest przeznaczona dla .NET Framework 4,5, nadaj jej nazwę SignalRChat, a następnie kliknij przycisk OK.</span><span class="sxs-lookup"><span data-stu-id="eb467-115">In Visual Studio, create a C# ASP.NET application that targets .NET Framework 4.5, name it SignalRChat, and click OK.</span></span>

    ![Tworzenie sieci Web](tutorial-getting-started-with-signalr-and-mvc/_static/image1.png)

1. <span data-ttu-id="eb467-117">W obszarze **Nowa aplikacja sieci Web ASP.NET — SignalRMvcChat**wybierz pozycję **MVC** , a następnie wybierz pozycję **Zmień uwierzytelnianie**.</span><span class="sxs-lookup"><span data-stu-id="eb467-117">In **New ASP.NET Web Application - SignalRMvcChat**, select **MVC** and then select **Change Authentication**.</span></span>

1. <span data-ttu-id="eb467-118">W obszarze **Zmień uwierzytelnianie**wybierz pozycję **bez uwierzytelniania** i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="eb467-118">In **Change Authentication**, select **No Authentication** and click **OK**.</span></span>

    ![Wybierz bez uwierzytelniania](tutorial-getting-started-with-signalr-and-mvc/_static/image2.png)

1. <span data-ttu-id="eb467-120">W obszarze **Nowa aplikacja sieci Web ASP.NET — SignalRMvcChat**wybierz **przycisk OK**.</span><span class="sxs-lookup"><span data-stu-id="eb467-120">In **New ASP.NET Web Application - SignalRMvcChat**, select **OK**.</span></span>

1. <span data-ttu-id="eb467-121">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt, a następnie wybierz pozycję **Dodaj** > **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="eb467-121">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="eb467-122">W obszarze **Dodaj nowy element — SignalRChat**wybierz pozycję **zainstalowane** > **Visual C#**  > **sieci Web** > **sygnalizujący** , a następnie wybierz pozycję **Klasa centrum sygnałów (v2)** .</span><span class="sxs-lookup"><span data-stu-id="eb467-122">In **Add New Item - SignalRChat**, select **Installed** > **Visual C#** > **Web** > **SignalR**  and then select **SignalR Hub Class (v2)**.</span></span>

1. <span data-ttu-id="eb467-123">Nadaj klasie nazwę *ChatHub* i Dodaj ją do projektu.</span><span class="sxs-lookup"><span data-stu-id="eb467-123">Name the class *ChatHub* and add it to the project.</span></span>

    <span data-ttu-id="eb467-124">Ten krok tworzy plik klasy *ChatHub.cs* i dodaje zestaw plików skryptów oraz odwołania do zestawów, które obsługują sygnalizujący do projektu.</span><span class="sxs-lookup"><span data-stu-id="eb467-124">This step creates the *ChatHub.cs* class file and adds a set of script files and assembly references that support SignalR to the project.</span></span>

1. <span data-ttu-id="eb467-125">Zastąp kod w nowym pliku klasy *ChatHub.cs* tym kodem:</span><span class="sxs-lookup"><span data-stu-id="eb467-125">Replace the code in the new *ChatHub.cs* class file with this code:</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample1.cs)]

1. <span data-ttu-id="eb467-126">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt i wybierz polecenie Dodaj **klasę** > .</span><span class="sxs-lookup"><span data-stu-id="eb467-126">In **Solution Explorer**, right-click the project and select **Add** > **Class**.</span></span>

1. <span data-ttu-id="eb467-127">Nazwij nową klasę *uruchamiania* i Dodaj ją do projektu.</span><span class="sxs-lookup"><span data-stu-id="eb467-127">Name the new class *Startup* and add it to the project.</span></span>

1. <span data-ttu-id="eb467-128">Zastąp kod w pliku klasy *Startup.cs* tym kodem:</span><span class="sxs-lookup"><span data-stu-id="eb467-128">Replace the code in the *Startup.cs* class file with this code:</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample2.cs)]

1. <span data-ttu-id="eb467-129">W **Eksplorator rozwiązań**wybierz pozycję **Kontrolery** > **HomeController.cs**.</span><span class="sxs-lookup"><span data-stu-id="eb467-129">In **Solution Explorer**, select **Controllers** > **HomeController.cs**.</span></span>

1. <span data-ttu-id="eb467-130">Dodaj tę metodę do *HomeController.cs*.</span><span class="sxs-lookup"><span data-stu-id="eb467-130">Add this method to the *HomeController.cs*.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample3.cs)]

    <span data-ttu-id="eb467-131">Ta metoda zwraca widok **rozmowy** , który został utworzony w późniejszym kroku.</span><span class="sxs-lookup"><span data-stu-id="eb467-131">This method returns the **Chat** view that you create in a later step.</span></span>

1. <span data-ttu-id="eb467-132">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy pozycję **widoki** > **Strona główna**, a następnie wybierz polecenie **Dodaj** **Widok** >  .</span><span class="sxs-lookup"><span data-stu-id="eb467-132">In **Solution Explorer**, right-click **Views** > **Home**, and select **Add** >  **View**.</span></span>

1. <span data-ttu-id="eb467-133">W polu **Dodaj widok**Nadaj nazwę nowemu widokowi **rozmowy** i wybierz pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="eb467-133">In **Add View**, name the new view **Chat** and select **Add**.</span></span>

1. <span data-ttu-id="eb467-134">Zastąp zawartość **czatu. cshtml** tym kodem:</span><span class="sxs-lookup"><span data-stu-id="eb467-134">Replace the contents of **Chat.cshtml** with this code:</span></span>

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample4.cshtml)]

1. <span data-ttu-id="eb467-135">W **Eksplorator rozwiązań**rozwiń węzeł **skrypty**.</span><span class="sxs-lookup"><span data-stu-id="eb467-135">In **Solution Explorer**, expand **Scripts**.</span></span>

    <span data-ttu-id="eb467-136">Biblioteki skryptów dla jQuery i sygnalizujących są widoczne w projekcie.</span><span class="sxs-lookup"><span data-stu-id="eb467-136">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="eb467-137">Menedżer pakietów mógł zainstalować nowszą wersję skryptów sygnalizującego.</span><span class="sxs-lookup"><span data-stu-id="eb467-137">The package manager may have installed a later version of the SignalR scripts.</span></span>

1. <span data-ttu-id="eb467-138">Sprawdź, czy odwołania do skryptu w bloku kodu odpowiadają wersji plików skryptów w projekcie.</span><span class="sxs-lookup"><span data-stu-id="eb467-138">Check that the script references in the code block correspond to the versions of the script files in the project.</span></span>

    <span data-ttu-id="eb467-139">Odwołania do skryptu z oryginalnego bloku kodu:</span><span class="sxs-lookup"><span data-stu-id="eb467-139">Script references from the original code block:</span></span>

    ```cshtml
    <!--Script references. -->
    <!--The jQuery library is required and is referenced by default in _Layout.cshtml. -->
    <!--Reference the SignalR library. -->
    <script src="~/Scripts/jquery.signalR-2.1.0.min.js"></script>
    ```

1. <span data-ttu-id="eb467-140">Jeśli nie są zgodne, zaktualizuj plik *. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="eb467-140">If they don't match, update the *.cshtml* file.</span></span>

1. <span data-ttu-id="eb467-141">Na pasku menu wybierz pozycję **plik** > **Zapisz wszystko**.</span><span class="sxs-lookup"><span data-stu-id="eb467-141">From the menu bar, select **File** > **Save All**.</span></span>

## <a name="run-the-sample"></a><span data-ttu-id="eb467-142">Uruchamianie przykładu</span><span class="sxs-lookup"><span data-stu-id="eb467-142">Run the Sample</span></span>

1. <span data-ttu-id="eb467-143">Na pasku narzędzi Włącz **debugowanie skryptów** , a następnie wybierz przycisk Odtwórz, aby uruchomić przykład w trybie debugowania.</span><span class="sxs-lookup"><span data-stu-id="eb467-143">In the toolbar, turn on **Script Debugging** and then select the play button to run the sample in Debug mode.</span></span>

    ![Wprowadź nazwę użytkownika](tutorial-getting-started-with-signalr-and-mvc/_static/image3.png)

1. <span data-ttu-id="eb467-145">Po otwarciu przeglądarki wprowadź nazwę swojej tożsamości rozmowy.</span><span class="sxs-lookup"><span data-stu-id="eb467-145">When the browser opens, enter a name for your chat identity.</span></span>

1. <span data-ttu-id="eb467-146">Skopiuj adres URL z przeglądarki, Otwórz dwie inne przeglądarki i wklej adresy URL do pasków adresów.</span><span class="sxs-lookup"><span data-stu-id="eb467-146">Copy the URL from the browser, open two other browsers, and paste the URLs into the address bars.</span></span>

1. <span data-ttu-id="eb467-147">W każdej przeglądarce wprowadź unikatową nazwę.</span><span class="sxs-lookup"><span data-stu-id="eb467-147">In each browser, enter a unique name.</span></span>

1. <span data-ttu-id="eb467-148">Teraz Dodaj komentarz i wybierz pozycję **Wyślij**.</span><span class="sxs-lookup"><span data-stu-id="eb467-148">Now, add a comment and select **Send**.</span></span> <span data-ttu-id="eb467-149">Powtórz te czynności w innych przeglądarkach.</span><span class="sxs-lookup"><span data-stu-id="eb467-149">Repeat that in the other browsers.</span></span> <span data-ttu-id="eb467-150">Komentarze pojawiają się w czasie rzeczywistym.</span><span class="sxs-lookup"><span data-stu-id="eb467-150">The comments appear in real time.</span></span>

    > [!NOTE]
    > <span data-ttu-id="eb467-151">Ta prosta aplikacja czatu nie utrzymuje kontekstu dyskusji na serwerze.</span><span class="sxs-lookup"><span data-stu-id="eb467-151">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="eb467-152">Centrum emituje Komentarze do wszystkich bieżących użytkowników.</span><span class="sxs-lookup"><span data-stu-id="eb467-152">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="eb467-153">Użytkownicy, którzy dołączą do rozmowy później zobaczą komunikaty dodawane od momentu dołączenia.</span><span class="sxs-lookup"><span data-stu-id="eb467-153">Users who join the chat later will see messages added from the time they join.</span></span>

    <span data-ttu-id="eb467-154">Zobacz, jak działa aplikacja czatu w trzech różnych przeglądarkach.</span><span class="sxs-lookup"><span data-stu-id="eb467-154">See how the chat application runs in three different browsers.</span></span> <span data-ttu-id="eb467-155">Po wysłaniu komunikatów Anand i Susan wszystkie przeglądarki są aktualizowane w czasie rzeczywistym:</span><span class="sxs-lookup"><span data-stu-id="eb467-155">When Tom, Anand, and Susan send messages, all browsers update in real time:</span></span>

    ![Wszystkie trzy przeglądarki wyświetlają tę samą historię rozmowy](tutorial-getting-started-with-signalr-and-mvc/_static/image4.png)

1. <span data-ttu-id="eb467-157">W **Eksplorator rozwiązań**Sprawdź, czy węzeł **dokumenty skryptu** dla uruchomionej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="eb467-157">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="eb467-158">Istnieje plik skryptu o nazwie *Hubs* , który jest generowany przez bibliotekę sygnałów w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="eb467-158">There's a script file named *hubs* that the SignalR library generates at runtime.</span></span> <span data-ttu-id="eb467-159">Ten plik zarządza komunikacją między skryptami jQuery i kodem po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="eb467-159">This file manages the communication between jQuery script and server-side code.</span></span>

    ![wygenerowany automatycznie skrypt centrów w węźle dokumenty skryptu](tutorial-getting-started-with-signalr-and-mvc/_static/image5.png)

## <a name="examine-the-code"></a><span data-ttu-id="eb467-161">Sprawdzanie kodu</span><span class="sxs-lookup"><span data-stu-id="eb467-161">Examine the Code</span></span>

<span data-ttu-id="eb467-162">Aplikacja rozmowy sygnalizującej pokazuje dwa podstawowe zadania programistyczne sygnalizujące.</span><span class="sxs-lookup"><span data-stu-id="eb467-162">The SignalR chat application demonstrates two basic SignalR development tasks.</span></span> <span data-ttu-id="eb467-163">Pokazano, jak utworzyć centrum.</span><span class="sxs-lookup"><span data-stu-id="eb467-163">It shows you how to create a hub.</span></span> <span data-ttu-id="eb467-164">Serwer używa tego centrum jako głównego obiektu koordynacji.</span><span class="sxs-lookup"><span data-stu-id="eb467-164">The server uses that hub as the main coordination object.</span></span> <span data-ttu-id="eb467-165">Do wysyłania i odbierania wiadomości centrum jest wykorzystywana Biblioteka sygnałów sygnalizujących.</span><span class="sxs-lookup"><span data-stu-id="eb467-165">The hub uses the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs-in-the-chathubcs"></a><span data-ttu-id="eb467-166">Centra sygnałów w ChatHub.cs</span><span class="sxs-lookup"><span data-stu-id="eb467-166">SignalR Hubs in the ChatHub.cs</span></span>

<span data-ttu-id="eb467-167">W przykładzie kodu Klasa `ChatHub` dziedziczy z klasy `Microsoft.AspNet.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="eb467-167">In the code sample, the `ChatHub` class derives from the `Microsoft.AspNet.SignalR.Hub` class.</span></span> <span data-ttu-id="eb467-168">Wyprowadzanie z klasy `Hub` jest użytecznym sposobem tworzenia aplikacji sygnalizującej.</span><span class="sxs-lookup"><span data-stu-id="eb467-168">Deriving from the `Hub` class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="eb467-169">Można tworzyć metody publiczne w klasie centrów, a następnie uzyskiwać dostęp do tych metod, wywołując je ze skryptów na stronie sieci Web.</span><span class="sxs-lookup"><span data-stu-id="eb467-169">You can create public methods on your hub class and then access those methods by calling them from scripts in a web page.</span></span>

<span data-ttu-id="eb467-170">W kodzie rozmowy klienci wywołują metodę `ChatHub.Send` w celu wysłania nowej wiadomości.</span><span class="sxs-lookup"><span data-stu-id="eb467-170">In the chat code, clients call the `ChatHub.Send` method to send a new message.</span></span> <span data-ttu-id="eb467-171">Koncentrator z kolei wysyła komunikat do wszystkich klientów, wywołując `Clients.All.addNewMessageToPage`.</span><span class="sxs-lookup"><span data-stu-id="eb467-171">The hub in turn sends the message to all clients by calling `Clients.All.addNewMessageToPage`.</span></span>

<span data-ttu-id="eb467-172">Metoda `Send` ilustruje kilka pojęć centrum:</span><span class="sxs-lookup"><span data-stu-id="eb467-172">The `Send` method demonstrates several hub concepts:</span></span>

* <span data-ttu-id="eb467-173">Zadeklaruj metody publiczne w centrum, aby klienci mogli je wywoływać.</span><span class="sxs-lookup"><span data-stu-id="eb467-173">Declare public methods on a hub so that clients can call them.</span></span>

* <span data-ttu-id="eb467-174">Użyj właściwości dynamicznej `Microsoft.AspNet.SignalR.Hub.Clients`, aby komunikować się ze wszystkimi klientami podłączonymi do tego centrum.</span><span class="sxs-lookup"><span data-stu-id="eb467-174">Use the `Microsoft.AspNet.SignalR.Hub.Clients` dynamic property to communicate with all clients connected to this hub.</span></span>

* <span data-ttu-id="eb467-175">Wywołaj funkcję na kliencie (na przykład funkcję `addNewMessageToPage`), aby zaktualizować klientów.</span><span class="sxs-lookup"><span data-stu-id="eb467-175">Call a function on the client (like the `addNewMessageToPage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample5.cs)]

### <a name="signalr-and-jquery-chatcshtml"></a><span data-ttu-id="eb467-176">Sygnalizujący i jQuery Chat. cshtml</span><span class="sxs-lookup"><span data-stu-id="eb467-176">SignalR and jQuery Chat.cshtml</span></span>

<span data-ttu-id="eb467-177">Plik widoku *Chat. cshtml* w przykładowym kodzie pokazuje, w jaki sposób używać biblioteki sygnalizującej jQuery do komunikowania się z centrum sygnałów.</span><span class="sxs-lookup"><span data-stu-id="eb467-177">The *Chat.cshtml* view file in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span>  <span data-ttu-id="eb467-178">Kod wykonuje wiele ważnych zadań.</span><span class="sxs-lookup"><span data-stu-id="eb467-178">The code carries out many important tasks.</span></span> <span data-ttu-id="eb467-179">Tworzy odwołanie do automatycznie generowanego serwera proxy dla centrum, deklaruje funkcję, którą serwer może wywoływać do wypychania zawartości do klientów i uruchamia połączenie w celu wysyłania komunikatów do centrum.</span><span class="sxs-lookup"><span data-stu-id="eb467-179">It creates a reference to the autogenerated proxy for the hub, declares a function that the server can call to push content to clients, and it starts a connection to send messages to the hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample6.js)]

> [!NOTE]
> <span data-ttu-id="eb467-180">W języku JavaScript odwołanie do klasy serwera i jej członków jest w camelCase.</span><span class="sxs-lookup"><span data-stu-id="eb467-180">In JavaScript, the reference to the server class and its members is in camelCase.</span></span> <span data-ttu-id="eb467-181">Przykładowy kod odwołuje się C# do klasy `ChatHub` w języku JavaScript jako `chatHub`.</span><span class="sxs-lookup"><span data-stu-id="eb467-181">The code sample references the C# `ChatHub` class in JavaScript as `chatHub`.</span></span>

<span data-ttu-id="eb467-182">W tym bloku kodu utworzysz funkcję wywołania zwrotnego w skrypcie.</span><span class="sxs-lookup"><span data-stu-id="eb467-182">In this code block, you create a callback function in the script.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample7.html)]

<span data-ttu-id="eb467-183">Klasa centrum na serwerze wywołuje tę funkcję, aby wypchnąć aktualizacje zawartości do każdego klienta.</span><span class="sxs-lookup"><span data-stu-id="eb467-183">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="eb467-184">Opcjonalne wywołanie funkcji `htmlEncode` pokazuje sposób kodowania zawartości wiadomości w formacie HTML przed wyświetleniem jej na stronie.</span><span class="sxs-lookup"><span data-stu-id="eb467-184">The optional call to the `htmlEncode` function shows a way to HTML encode the message content before displaying it in the page.</span></span> <span data-ttu-id="eb467-185">Jest to sposób, aby zapobiec iniekcji skryptu.</span><span class="sxs-lookup"><span data-stu-id="eb467-185">It's a way to prevent script injection.</span></span>

<span data-ttu-id="eb467-186">Ten kod otwiera połączenie z centrum.</span><span class="sxs-lookup"><span data-stu-id="eb467-186">This code opens a connection with the hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample8.js)]

> [!NOTE]
> <span data-ttu-id="eb467-187">To podejście zapewnia, że połączenie zostanie nawiązane przed wykonaniem procedury obsługi zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="eb467-187">This approach ensures that you establish a connection before the event handler executes.</span></span>

<span data-ttu-id="eb467-188">Kod uruchamia połączenie, a następnie przekazuje go do funkcji, aby obsłużyć zdarzenie kliknięcia na przycisku **Wyślij** na stronie rozmowy.</span><span class="sxs-lookup"><span data-stu-id="eb467-188">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the Chat page.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="eb467-189">Pobierz kod</span><span class="sxs-lookup"><span data-stu-id="eb467-189">Get the code</span></span>

[<span data-ttu-id="eb467-190">Pobierz ukończony projekt</span><span class="sxs-lookup"><span data-stu-id="eb467-190">Download Completed Project</span></span>](https://code.msdn.microsoft.com/Getting-Started-with-c366b2f3)

## <a name="additional-resources"></a><span data-ttu-id="eb467-191">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="eb467-191">Additional resources</span></span>

<span data-ttu-id="eb467-192">Aby uzyskać więcej informacji o sygnalizacji, zobacz następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="eb467-192">For more about SignalR, see the following resources:</span></span>

* [<span data-ttu-id="eb467-193">Projekt sygnalizującego</span><span class="sxs-lookup"><span data-stu-id="eb467-193">SignalR Project</span></span>](http://signalr.net)

* [<span data-ttu-id="eb467-194">Usługi GitHub i przykłady dla sygnałów</span><span class="sxs-lookup"><span data-stu-id="eb467-194">SignalR GitHub and Samples</span></span>](https://github.com/SignalR/SignalR)

* [<span data-ttu-id="eb467-195">Strona typu wiki sygnalizująca</span><span class="sxs-lookup"><span data-stu-id="eb467-195">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a><span data-ttu-id="eb467-196">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="eb467-196">Next steps</span></span>

<span data-ttu-id="eb467-197">W tym samouczku przedstawiono następujące instrukcje:</span><span class="sxs-lookup"><span data-stu-id="eb467-197">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="eb467-198">Konfigurowanie projektu</span><span class="sxs-lookup"><span data-stu-id="eb467-198">Set up the project</span></span>
> * <span data-ttu-id="eb467-199">Uruchomiono przykład</span><span class="sxs-lookup"><span data-stu-id="eb467-199">Ran the sample</span></span>
> * <span data-ttu-id="eb467-200">Zbadano kod</span><span class="sxs-lookup"><span data-stu-id="eb467-200">Examined the code</span></span>

<span data-ttu-id="eb467-201">Przejdź do następnego artykułu, aby dowiedzieć się, jak utworzyć aplikację sieci Web, która używa ASP.NET sygnalizującego 2, aby zapewnić funkcję obsługi komunikatów o wysokiej częstotliwości.</span><span class="sxs-lookup"><span data-stu-id="eb467-201">Advance to the next article to learn how to create a web application that uses ASP.NET SignalR 2 to provide high-frequency messaging functionality.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="eb467-202">Aplikacja internetowa z obsługą komunikatów o wysokiej częstotliwości</span><span class="sxs-lookup"><span data-stu-id="eb467-202">Web app with high-frequency messaging</span></span>](tutorial-high-frequency-realtime-with-signalr.md)
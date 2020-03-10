---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
title: 'Samouczek: Wprowadzenie z sygnałem 1. x i MVC 4 | Microsoft Docs'
author: bradygaster
description: Użyj ASP.NET Signaler i ASP.NET MVC 4 do kompilowania aplikacji czatu w czasie rzeczywistym.
ms.author: bradyg
ms.date: 03/29/2013
ms.assetid: eeef9f73-6de3-49f9-b50b-9af22108f2ce
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 9186915df6d5de6bc20dfc0adabc54056d2f3a8c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78579570"
---
# <a name="tutorial-getting-started-with-signalr-1x-and-mvc-4"></a><span data-ttu-id="d9398-103">Samouczek: wprowadzenie do usługi SignalR 1.x i wzorca MVC 4</span><span class="sxs-lookup"><span data-stu-id="d9398-103">Tutorial: Getting Started with SignalR 1.x and MVC 4</span></span>

<span data-ttu-id="d9398-104">[Fletcher Patryk](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span><span class="sxs-lookup"><span data-stu-id="d9398-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="d9398-105">W tym samouczku pokazano, jak za pomocą sygnalizacji ASP.NET utworzyć aplikację czatu w czasie rzeczywistym.</span><span class="sxs-lookup"><span data-stu-id="d9398-105">This tutorial shows how to use ASP.NET SignalR to create a real-time chat application.</span></span> <span data-ttu-id="d9398-106">Dodasz sygnał do aplikacji MVC 4 i utworzysz widok rozmowy do wysyłania i wyświetlania komunikatów.</span><span class="sxs-lookup"><span data-stu-id="d9398-106">You will add SignalR to an MVC 4 application and create a chat view to send and display messages.</span></span>

## <a name="overview"></a><span data-ttu-id="d9398-107">Omówienie</span><span class="sxs-lookup"><span data-stu-id="d9398-107">Overview</span></span>

<span data-ttu-id="d9398-108">W tym samouczku przedstawiono tworzenie aplikacji sieci Web w czasie rzeczywistym za pomocą sygnalizacji ASP.NET oraz ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="d9398-108">This tutorial introduces you to real-time web application development with ASP.NET SignalR and ASP.NET MVC 4.</span></span> <span data-ttu-id="d9398-109">Samouczek używa tego samego kodu aplikacji czatu, co w [samouczku wprowadzenie](tutorial-getting-started-with-signalr.md), ale pokazuje, jak dodać go do aplikacji MVC 4 na podstawie szablonu internetowego.</span><span class="sxs-lookup"><span data-stu-id="d9398-109">The tutorial uses the same chat application code as the [SignalR Getting Started tutorial](tutorial-getting-started-with-signalr.md), but shows how to add it to an MVC 4 application based on the Internet template.</span></span>

<span data-ttu-id="d9398-110">W tym temacie przedstawiono następujące zadania programistyczne sygnalizujące:</span><span class="sxs-lookup"><span data-stu-id="d9398-110">In this topic you will learn the following SignalR development tasks:</span></span>

- <span data-ttu-id="d9398-111">Dodawanie biblioteki sygnałów do aplikacji MVC 4.</span><span class="sxs-lookup"><span data-stu-id="d9398-111">Adding the SignalR library to an MVC 4 application.</span></span>
- <span data-ttu-id="d9398-112">Tworzenie klasy centrów do wypychania zawartości do klientów.</span><span class="sxs-lookup"><span data-stu-id="d9398-112">Creating a hub class to push content to clients.</span></span>
- <span data-ttu-id="d9398-113">Używanie biblioteki Sygnalizującer na stronie sieci Web do wysyłania komunikatów i wyświetlania aktualizacji z centrum.</span><span class="sxs-lookup"><span data-stu-id="d9398-113">Using the SignalR jQuery library in a web page to send messages and display updates from the hub.</span></span>

<span data-ttu-id="d9398-114">Poniższy zrzut ekranu przedstawia ukończoną aplikację czatu działającą w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="d9398-114">The following screen shot shows the completed chat application running in a browser.</span></span>

![Wystąpienia rozmów](tutorial-getting-started-with-signalr-and-mvc-4/_static/image2.png)

<span data-ttu-id="d9398-116">Poszczególne</span><span class="sxs-lookup"><span data-stu-id="d9398-116">Sections:</span></span>

- [<span data-ttu-id="d9398-117">Konfigurowanie projektu</span><span class="sxs-lookup"><span data-stu-id="d9398-117">Set up the Project</span></span>](#setup)
- [<span data-ttu-id="d9398-118">Uruchamianie przykładu</span><span class="sxs-lookup"><span data-stu-id="d9398-118">Run the Sample</span></span>](#run)
- [<span data-ttu-id="d9398-119">Sprawdzanie kodu</span><span class="sxs-lookup"><span data-stu-id="d9398-119">Examine the Code</span></span>](#code)
- [<span data-ttu-id="d9398-120">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="d9398-120">Next Steps</span></span>](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a><span data-ttu-id="d9398-121">Konfigurowanie projektu</span><span class="sxs-lookup"><span data-stu-id="d9398-121">Set up the Project</span></span>

<span data-ttu-id="d9398-122">Wymagania wstępne:</span><span class="sxs-lookup"><span data-stu-id="d9398-122">Prerequisites:</span></span>

- <span data-ttu-id="d9398-123">Visual Studio 2010 z dodatkiem SP1, Visual Studio 2012 lub Visual Studio 2012 Express.</span><span class="sxs-lookup"><span data-stu-id="d9398-123">Visual Studio 2010 SP1, Visual Studio 2012, or Visual Studio 2012 Express.</span></span> <span data-ttu-id="d9398-124">Jeśli nie masz programu Visual Studio, zobacz [ASP.NET downloads](https://www.asp.net/downloads) , aby uzyskać bezpłatne narzędzie programistyczne programu Visual Studio 2012 Express.</span><span class="sxs-lookup"><span data-stu-id="d9398-124">If you do not have Visual Studio, see [ASP.NET Downloads](https://www.asp.net/downloads) to get the free Visual Studio 2012 Express Development Tool.</span></span>
- <span data-ttu-id="d9398-125">W przypadku programu Visual Studio 2010 zainstaluj [ASP.NET MVC 4](https://www.microsoft.com/download/details.aspx?id=30683).</span><span class="sxs-lookup"><span data-stu-id="d9398-125">For Visual Studio 2010, install [ASP.NET MVC 4](https://www.microsoft.com/download/details.aspx?id=30683).</span></span>

<span data-ttu-id="d9398-126">W tej sekcji pokazano, jak utworzyć aplikację ASP.NET MVC 4, dodać bibliotekę sygnałów i utworzyć aplikację czatu.</span><span class="sxs-lookup"><span data-stu-id="d9398-126">This section shows how to create an ASP.NET MVC 4 application, add the SignalR library, and create the chat application.</span></span>

1. 1. <span data-ttu-id="d9398-127">W programie Visual Studio Utwórz aplikację ASP.NET MVC 4, nadaj jej nazwę SignalRChat, a następnie kliknij przycisk OK.</span><span class="sxs-lookup"><span data-stu-id="d9398-127">In Visual Studio create an ASP.NET MVC 4 application, name it SignalRChat, and click OK.</span></span>

        > [!NOTE]
        > <span data-ttu-id="d9398-128">W programie VS 2010 wybierz pozycję **.NET Framework 4** w kontrolce listy rozwijanej wersji platformy.</span><span class="sxs-lookup"><span data-stu-id="d9398-128">In VS 2010, select **.NET Framework 4** in the Framework version dropdown control.</span></span> <span data-ttu-id="d9398-129">Kod sygnalizujący działa na .NET Framework wersjach 4 i 4,5.</span><span class="sxs-lookup"><span data-stu-id="d9398-129">SignalR code runs on .NET Framework versions 4 and 4.5.</span></span>

        ![Tworzenie sieci Web MVC](tutorial-getting-started-with-signalr-and-mvc-4/_static/image3.png)
      2. <span data-ttu-id="d9398-131">Wybierz szablon aplikacja internetowa, usuń zaznaczenie opcji **Utwórz projekt testu jednostkowego**, a następnie kliknij przycisk OK.</span><span class="sxs-lookup"><span data-stu-id="d9398-131">Select the Internet Application template, clear the option to **Create a unit test project**, and click OK.</span></span>

         ![Utwórz witrynę internetową MVC](tutorial-getting-started-with-signalr-and-mvc-4/_static/image4.png)
      3. <span data-ttu-id="d9398-133">Otwórz **narzędzia > menedżerze pakietów NuGet > konsolę Menedżera pakietów** i uruchom następujące polecenie.</span><span class="sxs-lookup"><span data-stu-id="d9398-133">Open the **Tools > NuGet Package Manager > Package Manager Console** and run the following command.</span></span> <span data-ttu-id="d9398-134">Ten krok powoduje dodanie do projektu zestawu plików skryptów i odwołań do zestawów, które włączają funkcję sygnalizującą.</span><span class="sxs-lookup"><span data-stu-id="d9398-134">This step adds to the project a set of script files and assembly references that enable SignalR functionality.</span></span>

         `install-package Microsoft.AspNet.SignalR -Version 1.1.3`
      4. <span data-ttu-id="d9398-135">W **Eksplorator rozwiązań** rozwiń folder skrypty.</span><span class="sxs-lookup"><span data-stu-id="d9398-135">In **Solution Explorer** expand the Scripts folder.</span></span> <span data-ttu-id="d9398-136">Zwróć uwagę, że biblioteki skryptów dla sygnalizującego zostały dodane do projektu.</span><span class="sxs-lookup"><span data-stu-id="d9398-136">Note that script libraries for SignalR have been added to the project.</span></span>

         ![Odwołania do biblioteki](tutorial-getting-started-with-signalr-and-mvc-4/_static/image6.png)
      5. <span data-ttu-id="d9398-138">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt, a następnie wybierz pozycję **Dodaj | Nowy folder**i Dodaj nowy folder o nazwie **Hubs**.</span><span class="sxs-lookup"><span data-stu-id="d9398-138">In **Solution Explorer**, right-click the project, select **Add | New Folder**, and add a new folder named **Hubs**.</span></span>
      6. <span data-ttu-id="d9398-139">Kliknij prawym przyciskiem myszy folder **Hubs** , a następnie kliknij pozycję **Dodaj |** I Utwórz nową C# klasę o nazwie **ChatHub.cs**.</span><span class="sxs-lookup"><span data-stu-id="d9398-139">Right-click the **Hubs** folder, click **Add | Class**, and create a new C# class named **ChatHub.cs**.</span></span> <span data-ttu-id="d9398-140">Ta klasa będzie używana jako centrum serwera sygnalizującego, który wysyła komunikaty do wszystkich klientów.</span><span class="sxs-lookup"><span data-stu-id="d9398-140">You will use this class as a SignalR server hub that sends messages to all clients.</span></span>

> [!NOTE]
> <span data-ttu-id="d9398-141">Jeśli używasz programu Visual Studio 2012 i zainstalowano [aktualizację ASP.NET and Web Tools 2012,2](../../../visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw.md#_Installation), możesz użyć nowego szablonu elementu sygnalizującego, aby utworzyć klasę centrum.</span><span class="sxs-lookup"><span data-stu-id="d9398-141">If you use Visual Studio 2012 and have installed the [ASP.NET and Web Tools 2012.2 update](../../../visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw.md#_Installation), you can use the new SignalR item template to create the hub class.</span></span> <span data-ttu-id="d9398-142">W tym celu kliknij prawym przyciskiem myszy folder **Hubs** , a następnie kliknij pozycję **Dodaj | Nowy element**, wybierz pozycję **centrum sygnałów Klasa (v1)** i nadaj klasie **ChatHub.cs**.</span><span class="sxs-lookup"><span data-stu-id="d9398-142">To do that, right-click the **Hubs** folder, click **Add | New Item**, select **SignalR Hub Class (v1)**, and name the class **ChatHub.cs**.</span></span>

1. <span data-ttu-id="d9398-143">Zastąp kod w klasie **ChatHub** następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="d9398-143">Replace the code in the **ChatHub** class with the following code.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample1.cs)]
2. <span data-ttu-id="d9398-144">Otwórz plik **Global. asax** dla projektu i Dodaj wywołanie do metody `RouteTable.Routes.MapHubs();` jako pierwszy wiersz kodu w metodzie `Application_Start`.</span><span class="sxs-lookup"><span data-stu-id="d9398-144">Open the **Global.asax** file for the project, and add a call to the method `RouteTable.Routes.MapHubs();` as the first line of code in the `Application_Start` method.</span></span> <span data-ttu-id="d9398-145">Ten kod rejestruje domyślną trasę dla centrów sygnałów i musi być wywoływana przed zarejestrowaniem innych tras.</span><span class="sxs-lookup"><span data-stu-id="d9398-145">This code registers the default route for SignalR hubs and must be called before you register any other routes.</span></span> <span data-ttu-id="d9398-146">Zakończono `Application_Start` Metoda wygląda podobnie do poniższego przykładu.</span><span class="sxs-lookup"><span data-stu-id="d9398-146">The completed `Application_Start` method looks like the following example.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample2.cs)]
3. <span data-ttu-id="d9398-147">Edytuj klasę `HomeController` znalezioną w obszarze **controllers/HomeController. cs** i Dodaj następującą metodę do klasy.</span><span class="sxs-lookup"><span data-stu-id="d9398-147">Edit the `HomeController` class found in **Controllers/HomeController.cs** and add the following method to the class.</span></span> <span data-ttu-id="d9398-148">Ta metoda zwraca widok **rozmowy** , który zostanie utworzony w późniejszym kroku.</span><span class="sxs-lookup"><span data-stu-id="d9398-148">This method returns the **Chat** view that you will create in a later step.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample3.cs)]
4. <span data-ttu-id="d9398-149">Kliknij prawym przyciskiem myszy w właśnie utworzonej metodzie `Chat` i kliknij przycisk **Dodaj widok** , aby utworzyć nowy plik widoku.</span><span class="sxs-lookup"><span data-stu-id="d9398-149">Right-click within the `Chat` method you just created, and click **Add View** to create a new view file.</span></span>
5. <span data-ttu-id="d9398-150">W oknie dialogowym **Dodawanie widoku** upewnij się, że pole wyboru jest zaznaczone, aby **użyć układu lub strony wzorcowej** (wyczyść inne pola wyboru), a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="d9398-150">In the **Add View** dialog, make sure the check box is selected to **Use a layout or master page** (clear the other check boxes), and then click **Add**.</span></span>

    ![Dodawanie widoku](tutorial-getting-started-with-signalr-and-mvc-4/_static/image8.png)
6. <span data-ttu-id="d9398-152">Edytuj nowy plik widoku o nazwie **Chat. cshtml**.</span><span class="sxs-lookup"><span data-stu-id="d9398-152">Edit the new view file named **Chat.cshtml**.</span></span> <span data-ttu-id="d9398-153">Po tagu &lt;H2&gt; wklej następującą sekcję &lt;DIV&gt; i `@section scripts` blok kodu na stronie.</span><span class="sxs-lookup"><span data-stu-id="d9398-153">After the &lt;h2&gt; tag, paste the following &lt;div&gt; section and `@section scripts` code block into the page.</span></span> <span data-ttu-id="d9398-154">Ten skrypt umożliwia stronie wysyłanie komunikatów rozmowy i wyświetlanie komunikatów z serwera.</span><span class="sxs-lookup"><span data-stu-id="d9398-154">This script enables the page to send chat messages and display messages from the server.</span></span> <span data-ttu-id="d9398-155">Pełny kod widoku rozmowy zostanie wyświetlony w poniższym bloku kodu.</span><span class="sxs-lookup"><span data-stu-id="d9398-155">The complete code for the chat view appears in the following code block.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="d9398-156">Po dodaniu sygnalizującego i innych bibliotek skryptów do projektu programu Visual Studio Menedżer pakietów może instalować wersje skryptów, które są nowsze niż wersje przedstawione w tym temacie.</span><span class="sxs-lookup"><span data-stu-id="d9398-156">When you add SignalR and other script libraries to your Visual Studio project, the Package Manager might install versions of the scripts that are more recent than the versions shown in this topic.</span></span> <span data-ttu-id="d9398-157">Upewnij się, że odwołania do skryptu w kodzie są zgodne z wersjami bibliotek skryptów zainstalowanych w projekcie.</span><span class="sxs-lookup"><span data-stu-id="d9398-157">Make sure that the script references in your code match the versions of the script libraries installed in your project.</span></span>

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample4.cshtml)]
7. <span data-ttu-id="d9398-158">**Zapisz wszystko** dla projektu.</span><span class="sxs-lookup"><span data-stu-id="d9398-158">**Save All** for the project.</span></span>

<a id="run"></a>

## <a name="run-the-sample"></a><span data-ttu-id="d9398-159">Uruchamianie przykładu</span><span class="sxs-lookup"><span data-stu-id="d9398-159">Run the Sample</span></span>

1. <span data-ttu-id="d9398-160">Naciśnij klawisz F5, aby uruchomić projekt w trybie debugowania.</span><span class="sxs-lookup"><span data-stu-id="d9398-160">Press F5 to run the project in debug mode.</span></span>
2. <span data-ttu-id="d9398-161">W linii adresowej przeglądarki Dołącz **/Home/chat** do adresu URL strony domyślnej dla projektu.</span><span class="sxs-lookup"><span data-stu-id="d9398-161">In the browser address line, append **/home/chat** to the URL of the default page for the project.</span></span> <span data-ttu-id="d9398-162">Strona rozmowy zostanie załadowana w wystąpieniu przeglądarki i zostanie wyświetlony komunikat z prośbą o nazwę użytkownika.</span><span class="sxs-lookup"><span data-stu-id="d9398-162">The Chat page loads in a browser instance and prompts for a user name.</span></span>

    ![Wprowadź nazwę użytkownika](tutorial-getting-started-with-signalr-and-mvc-4/_static/image9.png)
3. <span data-ttu-id="d9398-164">Wprowadź nazwę użytkownika.</span><span class="sxs-lookup"><span data-stu-id="d9398-164">Enter a user name.</span></span>
4. <span data-ttu-id="d9398-165">Skopiuj adres URL z linii adresowej przeglądarki i użyj go, aby otworzyć dwa więcej wystąpień przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="d9398-165">Copy the URL from the address line of the browser and use it to open two more browser instances.</span></span> <span data-ttu-id="d9398-166">W każdym wystąpieniu przeglądarki wprowadź unikatową nazwę użytkownika.</span><span class="sxs-lookup"><span data-stu-id="d9398-166">In each browser instance, enter a unique user name.</span></span>
5. <span data-ttu-id="d9398-167">W każdym wystąpieniu przeglądarki Dodaj komentarz, a następnie kliknij przycisk **Wyślij**.</span><span class="sxs-lookup"><span data-stu-id="d9398-167">In each browser instance, add a comment and click **Send**.</span></span> <span data-ttu-id="d9398-168">Komentarze powinny być wyświetlane we wszystkich wystąpieniach przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="d9398-168">The comments should display in all browser instances.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d9398-169">Ta prosta aplikacja czatu nie utrzymuje kontekstu dyskusji na serwerze.</span><span class="sxs-lookup"><span data-stu-id="d9398-169">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="d9398-170">Centrum emituje Komentarze do wszystkich bieżących użytkowników.</span><span class="sxs-lookup"><span data-stu-id="d9398-170">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="d9398-171">Użytkownicy, którzy dołączą do rozmowy później zobaczą komunikaty dodawane od momentu dołączenia.</span><span class="sxs-lookup"><span data-stu-id="d9398-171">Users who join the chat later will see messages added from the time they join.</span></span>
6. <span data-ttu-id="d9398-172">Poniższy zrzut ekranu przedstawia aplikację czatu działającą w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="d9398-172">The following screen shot shows the chat application running in a browser.</span></span>

    ![Przeglądarki rozmowy](tutorial-getting-started-with-signalr-and-mvc-4/_static/image11.png)
7. <span data-ttu-id="d9398-174">W **Eksplorator rozwiązań**Sprawdź, czy węzeł **dokumenty skryptu** dla uruchomionej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d9398-174">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="d9398-175">Ten węzeł jest widoczny w trybie debugowania, jeśli używasz programu Internet Explorer jako przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="d9398-175">This node is visible in debug mode if you are using Internet Explorer as your browser.</span></span> <span data-ttu-id="d9398-176">Istnieje plik skryptu o nazwie **Hubs** , który dynamicznie generuje Biblioteka sygnałów w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="d9398-176">There is a script file named **hubs** that the SignalR library dynamically generates at runtime.</span></span> <span data-ttu-id="d9398-177">Ten plik zarządza komunikacją między skryptami jQuery i kodem po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="d9398-177">This file manages the communication between jQuery script and server-side code.</span></span> <span data-ttu-id="d9398-178">Jeśli używasz przeglądarki innej niż Internet Explorer, możesz również uzyskać dostęp do pliku **centrów** dynamicznych, przechodząc do niego bezpośrednio, na przykład http://mywebsite/signalr/hubs.</span><span class="sxs-lookup"><span data-stu-id="d9398-178">If you use a browser other than Internet Explorer, you can also access the dynamic **hubs** file by browsing to it directly, for example http://mywebsite/signalr/hubs.</span></span>

    ![Wygenerowany skrypt centralny](tutorial-getting-started-with-signalr-and-mvc-4/_static/image13.png)

<a id="code"></a>

## <a name="examine-the-code"></a><span data-ttu-id="d9398-180">Sprawdzanie kodu</span><span class="sxs-lookup"><span data-stu-id="d9398-180">Examine the Code</span></span>

<span data-ttu-id="d9398-181">Aplikacja rozmowy sygnalizująca pokazuje dwa podstawowe zadania programistyczne sygnalizujące: tworzenie centrum jako głównego obiektu koordynacji na serwerze i używanie biblioteki sygnalizującej jQuery do wysyłania i odbierania wiadomości.</span><span class="sxs-lookup"><span data-stu-id="d9398-181">The SignalR chat application demonstrates two basic SignalR development tasks: creating a hub as the main coordination object on the server, and using the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs"></a><span data-ttu-id="d9398-182">Centra sygnałów</span><span class="sxs-lookup"><span data-stu-id="d9398-182">SignalR Hubs</span></span>

<span data-ttu-id="d9398-183">W przykładzie kodu Klasa **ChatHub** pochodzi od klasy **Microsoft. ASPNET. Signaler. Hub** .</span><span class="sxs-lookup"><span data-stu-id="d9398-183">In the code sample the **ChatHub** class derives from the **Microsoft.AspNet.SignalR.Hub** class.</span></span> <span data-ttu-id="d9398-184">Wyprowadzanie z klasy **centrów** to przydatny sposób tworzenia aplikacji sygnalizującej.</span><span class="sxs-lookup"><span data-stu-id="d9398-184">Deriving from the **Hub** class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="d9398-185">Można tworzyć metody publiczne w klasie centrów, a następnie uzyskiwać dostęp do tych metod, wywołując je ze skryptów jQuery na stronie sieci Web.</span><span class="sxs-lookup"><span data-stu-id="d9398-185">You can create public methods on your hub class and then access those methods by calling them from jQuery scripts in a web page.</span></span>

<span data-ttu-id="d9398-186">W kodzie rozmowy klienci wywołują metodę **ChatHub. Send** w celu wysłania nowej wiadomości.</span><span class="sxs-lookup"><span data-stu-id="d9398-186">In the chat code, clients call the **ChatHub.Send** method to send a new message.</span></span> <span data-ttu-id="d9398-187">Koncentrator z kolei wysyła komunikat do wszystkich klientów, wywołując **klientów. ALL. addNewMessageToPage**.</span><span class="sxs-lookup"><span data-stu-id="d9398-187">The hub in turn sends the message to all clients by calling **Clients.All.addNewMessageToPage**.</span></span>

<span data-ttu-id="d9398-188">Metoda **send** ilustruje kilka pojęć centrum:</span><span class="sxs-lookup"><span data-stu-id="d9398-188">The **Send** method demonstrates several hub concepts :</span></span>

- <span data-ttu-id="d9398-189">Zadeklaruj metody publiczne w centrum, aby klienci mogli je wywoływać.</span><span class="sxs-lookup"><span data-stu-id="d9398-189">Declare public methods on a hub so that clients can call them.</span></span>
- <span data-ttu-id="d9398-190">Właściwość **Microsoft. ASPNET. signaler. Hub** umożliwia dostęp do wszystkich klientów podłączonych do tego centrum.</span><span class="sxs-lookup"><span data-stu-id="d9398-190">Use the **Microsoft.AspNet.SignalR.Hub.Clients** property to access all clients connected to this hub.</span></span>
- <span data-ttu-id="d9398-191">Wywołaj funkcję jQuery na kliencie (na przykład funkcję `addNewMessageToPage`), aby zaktualizować klientów.</span><span class="sxs-lookup"><span data-stu-id="d9398-191">Call a jQuery function on the client (such as the `addNewMessageToPage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a><span data-ttu-id="d9398-192">Sygnalizujący i jQuery</span><span class="sxs-lookup"><span data-stu-id="d9398-192">SignalR and jQuery</span></span>

<span data-ttu-id="d9398-193">Plik widoku **Chat. cshtml** w przykładowym kodzie pokazuje, w jaki sposób używać biblioteki sygnalizującej jQuery do komunikowania się z centrum sygnałów.</span><span class="sxs-lookup"><span data-stu-id="d9398-193">The **Chat.cshtml** view file in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="d9398-194">Zasadnicze zadania w kodzie tworzą odwołanie do automatycznie generowanego serwera proxy dla centrum, deklarując funkcję, którą serwer może wywoływać, aby wypchnąć zawartość do klientów i rozpocząć połączenie w celu wysyłania komunikatów do centrum.</span><span class="sxs-lookup"><span data-stu-id="d9398-194">The essential tasks in the code are creating a reference to the auto-generated proxy for the hub, declaring a function that the server can call to push content to clients, and starting a connection to send messages to the hub.</span></span>

<span data-ttu-id="d9398-195">Poniższy kod deklaruje serwer proxy dla centrum.</span><span class="sxs-lookup"><span data-stu-id="d9398-195">The following code declares a proxy for a hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample6.js)]

> [!NOTE]
> <span data-ttu-id="d9398-196">W jQuery odwołanie do klasy serwera i jej członków jest w notacji CamelCase przypadku.</span><span class="sxs-lookup"><span data-stu-id="d9398-196">In jQuery the reference to the server class and its members is in camel case.</span></span> <span data-ttu-id="d9398-197">Przykładowy kod odwołuje się C# do klasy **ChatHub** w jQuery jako **ChatHub**.</span><span class="sxs-lookup"><span data-stu-id="d9398-197">The code sample references the C# **ChatHub** class in jQuery as **chatHub**.</span></span> <span data-ttu-id="d9398-198">Jeśli chcesz odwołać się do klasy `ChatHub` w platformie jQuery przy użyciu konwencjonalnej wielkości liter języka C#Pascal jak w programie, edytuj plik klasy ChatHub.cs.</span><span class="sxs-lookup"><span data-stu-id="d9398-198">If you want to reference the `ChatHub` class in jQuery with conventional Pascal casing as you would in C#, edit the ChatHub.cs class file.</span></span> <span data-ttu-id="d9398-199">Dodaj instrukcję `using`, aby odwoływać się do przestrzeni nazw `Microsoft.AspNet.SignalR.Hubs`.</span><span class="sxs-lookup"><span data-stu-id="d9398-199">Add a `using` statement to reference the `Microsoft.AspNet.SignalR.Hubs` namespace.</span></span> <span data-ttu-id="d9398-200">Następnie Dodaj atrybut `HubName` do klasy `ChatHub`, na przykład `[HubName("ChatHub")]`.</span><span class="sxs-lookup"><span data-stu-id="d9398-200">Then add the `HubName` attribute to the `ChatHub` class, for example `[HubName("ChatHub")]`.</span></span> <span data-ttu-id="d9398-201">Na koniec zaktualizuj odwołanie jQuery do klasy `ChatHub`.</span><span class="sxs-lookup"><span data-stu-id="d9398-201">Finally, update your jQuery reference to the `ChatHub` class.</span></span>

<span data-ttu-id="d9398-202">Poniższy kod przedstawia sposób tworzenia funkcji wywołania zwrotnego w skrypcie.</span><span class="sxs-lookup"><span data-stu-id="d9398-202">The following code shows how to create a callback function in the script.</span></span> <span data-ttu-id="d9398-203">Klasa centrum na serwerze wywołuje tę funkcję, aby wypchnąć aktualizacje zawartości do każdego klienta.</span><span class="sxs-lookup"><span data-stu-id="d9398-203">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="d9398-204">Opcjonalne wywołanie funkcji `htmlEncode` pokazuje sposób kodowania zawartości wiadomości w formacie HTML przed wyświetleniem jej na stronie jako sposób, aby zapobiec iniekcji skryptu.</span><span class="sxs-lookup"><span data-stu-id="d9398-204">The optional call to the `htmlEncode` function shows a way to HTML encode the message content before displaying it in the page, as a way to prevent script injection.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample7.html)]

<span data-ttu-id="d9398-205">Poniższy kod pokazuje, jak otworzyć połączenie z centrum.</span><span class="sxs-lookup"><span data-stu-id="d9398-205">The following code shows how to open a connection with the hub.</span></span> <span data-ttu-id="d9398-206">Kod uruchamia połączenie, a następnie przekazuje go do funkcji, aby obsłużyć zdarzenie kliknięcia na przycisku **Wyślij** na stronie rozmowy.</span><span class="sxs-lookup"><span data-stu-id="d9398-206">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the Chat page.</span></span>

> [!NOTE]
> <span data-ttu-id="d9398-207">Takie podejście zapewnia, że połączenie zostanie nawiązane przed wykonaniem procedury obsługi zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="d9398-207">This approach ensures that the connection is established before the event handler executes.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a><span data-ttu-id="d9398-208">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="d9398-208">Next Steps</span></span>

<span data-ttu-id="d9398-209">Zapoznaj się z tym, że sygnalizujący jest platforma do tworzenia aplikacji sieci Web w czasie rzeczywistym.</span><span class="sxs-lookup"><span data-stu-id="d9398-209">You learned that SignalR is a framework for building real-time web applications.</span></span> <span data-ttu-id="d9398-210">Poznasz również kilka zadań programistycznych sygnalizujących: jak dodać odbiornik do aplikacji ASP.NET, jak utworzyć klasę centrum oraz jak wysyłać i odbierać komunikaty z centrum.</span><span class="sxs-lookup"><span data-stu-id="d9398-210">You also learned several SignalR development tasks: how to add SignalR to an ASP.NET application, how to create a hub class, and how to send and receive messages from the hub.</span></span>

<span data-ttu-id="d9398-211">Aby dowiedzieć się więcej na temat zaawansowanych pojęć związanych z założeniami, odwiedź następujące witryny dotyczące kodu źródłowego i zasobów usługi sygnalizującego:</span><span class="sxs-lookup"><span data-stu-id="d9398-211">To learn more advanced SignalR developments concepts, visit the following sites for SignalR source code and resources :</span></span>

- [<span data-ttu-id="d9398-212">Projekt sygnalizującego</span><span class="sxs-lookup"><span data-stu-id="d9398-212">SignalR Project</span></span>](http://signalr.net)
- [<span data-ttu-id="d9398-213">Usługi GitHub i przykłady dla sygnałów</span><span class="sxs-lookup"><span data-stu-id="d9398-213">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="d9398-214">Strona typu wiki sygnalizująca</span><span class="sxs-lookup"><span data-stu-id="d9398-214">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)

---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr
title: 'Samouczek: Wprowadzenie z sygnałem 1. x | Microsoft Docs'
author: bradygaster
description: Użyj sygnalizującego ASP.NET, aby skompilować aplikację czatu w czasie rzeczywistym na stronie HTML.
ms.author: bradyg
ms.date: 02/18/2013
ms.assetid: fdc3599a-5217-44c1-951f-0eec9812dce7
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 87a90b47ae30bee43e0b0c1e078597db54b8e67d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78623544"
---
# <a name="tutorial-getting-started-with-signalr-1x"></a><span data-ttu-id="23a83-103">Samouczek: wprowadzenie do usługi SignalR 1.x</span><span class="sxs-lookup"><span data-stu-id="23a83-103">Tutorial: Getting Started with SignalR 1.x</span></span>

<span data-ttu-id="23a83-104">[Fletcher Patryk](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span><span class="sxs-lookup"><span data-stu-id="23a83-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="23a83-105">Ten samouczek pokazuje, jak utworzyć aplikację do obsługi rozmów w czasie rzeczywistym przy użyciu SignalR.</span><span class="sxs-lookup"><span data-stu-id="23a83-105">This tutorial shows how to use SignalR to create a real-time chat application.</span></span> <span data-ttu-id="23a83-106">Dodasz sygnał do pustej aplikacji sieci Web ASP.NET i utworzysz stronę HTML do wysyłania i wyświetlania komunikatów.</span><span class="sxs-lookup"><span data-stu-id="23a83-106">You will add SignalR to an empty ASP.NET web application and create an HTML page to send and display messages.</span></span>

## <a name="overview"></a><span data-ttu-id="23a83-107">Omówienie</span><span class="sxs-lookup"><span data-stu-id="23a83-107">Overview</span></span>

<span data-ttu-id="23a83-108">Ten samouczek przedstawia programowanie sygnałów przez pokazanie sposobu tworzenia prostej aplikacji czatu opartej na przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="23a83-108">This tutorial introduces SignalR development by showing how to build a simple browser-based chat application.</span></span> <span data-ttu-id="23a83-109">Dodasz bibliotekę sygnałów do pustej aplikacji sieci Web ASP.NET, utworzysz klasę centrum do wysyłania komunikatów do klientów i utworzysz stronę HTML, która umożliwia użytkownikom wysyłanie i odbieranie komunikatów rozmowy.</span><span class="sxs-lookup"><span data-stu-id="23a83-109">You will add the SignalR library to an empty ASP.NET web application, create a hub class for sending messages to clients, and create an HTML page that lets users send and receive chat messages.</span></span> <span data-ttu-id="23a83-110">Aby poznać podobny samouczek, w którym pokazano, jak utworzyć aplikację czatu w MVC 4 przy użyciu widoku MVC, zobacz [wprowadzenie z sygnalizującer i MVC 4](index.md).</span><span class="sxs-lookup"><span data-stu-id="23a83-110">For a similar tutorial that shows how to create a chat application in MVC 4 using an MVC view, see [Getting Started with SignalR and MVC 4](index.md).</span></span>

> [!NOTE]
> <span data-ttu-id="23a83-111">W tym samouczku zostanie użyta wersja programu System (1. x).</span><span class="sxs-lookup"><span data-stu-id="23a83-111">This tutorial uses the release (1.x) version of SignalR.</span></span> <span data-ttu-id="23a83-112">Aby uzyskać szczegółowe informacje na temat zmian między sygnałami 1. x i 2,0, zobacz [uaktualnianie projektów sygnalizujących 1. x](../releases/upgrading-signalr-1x-projects-to-20.md).</span><span class="sxs-lookup"><span data-stu-id="23a83-112">For details on changes between SignalR 1.x and 2.0, see [Upgrading SignalR 1.x Projects](../releases/upgrading-signalr-1x-projects-to-20.md).</span></span>

<span data-ttu-id="23a83-113">Sygnalizujący to biblioteka .NET open source do tworzenia aplikacji sieci Web, które wymagają interakcji z użytkownikiem na żywo lub aktualizacji danych w czasie rzeczywistym.</span><span class="sxs-lookup"><span data-stu-id="23a83-113">SignalR is an open-source .NET library for building web applications that require live user interaction or real-time data updates.</span></span> <span data-ttu-id="23a83-114">Przykładami mogą być aplikacje społecznościowe, gry wielodostępne, Współpraca biznesowa i wiadomości, Pogoda lub aktualizacje finansowe.</span><span class="sxs-lookup"><span data-stu-id="23a83-114">Examples include social applications, multiuser games, business collaboration, and news, weather, or financial update applications.</span></span> <span data-ttu-id="23a83-115">Są one często nazywane aplikacjami w czasie rzeczywistym.</span><span class="sxs-lookup"><span data-stu-id="23a83-115">These are often called real-time applications.</span></span>

<span data-ttu-id="23a83-116">Sygnalizujący upraszcza proces tworzenia aplikacji w czasie rzeczywistym.</span><span class="sxs-lookup"><span data-stu-id="23a83-116">SignalR simplifies the process of building real-time applications.</span></span> <span data-ttu-id="23a83-117">Zawiera bibliotekę serwera ASP.NET i bibliotekę klienta JavaScript, która ułatwia zarządzanie połączeniami klienta i serwera oraz wypychanie aktualizacji zawartości do klientów.</span><span class="sxs-lookup"><span data-stu-id="23a83-117">It includes an ASP.NET server library and a JavaScript client library to make it easier to manage client-server connections and push content updates to clients.</span></span> <span data-ttu-id="23a83-118">Do istniejącej aplikacji ASP.NET można dodać bibliotekę sygnalizującą w celu uzyskania funkcjonalności w czasie rzeczywistym.</span><span class="sxs-lookup"><span data-stu-id="23a83-118">You can add the SignalR library to an existing ASP.NET application to gain real-time functionality.</span></span>

<span data-ttu-id="23a83-119">Samouczek ilustruje następujące zadania programistyczne sygnalizujące:</span><span class="sxs-lookup"><span data-stu-id="23a83-119">The tutorial demonstrates the following SignalR development tasks:</span></span>

- <span data-ttu-id="23a83-120">Dodawanie biblioteki sygnałów do aplikacji sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="23a83-120">Adding the SignalR library to an ASP.NET web application.</span></span>
- <span data-ttu-id="23a83-121">Tworzenie klasy centrów do wypychania zawartości do klientów.</span><span class="sxs-lookup"><span data-stu-id="23a83-121">Creating a hub class to push content to clients.</span></span>
- <span data-ttu-id="23a83-122">Używanie biblioteki Sygnalizującer na stronie sieci Web do wysyłania komunikatów i wyświetlania aktualizacji z centrum.</span><span class="sxs-lookup"><span data-stu-id="23a83-122">Using the SignalR jQuery library in a web page to send messages and display updates from the hub.</span></span>

<span data-ttu-id="23a83-123">Poniższy zrzut ekranu przedstawia aplikację czatu działającą w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="23a83-123">The following screen shot shows the chat application running in a browser.</span></span> <span data-ttu-id="23a83-124">Każdy nowy użytkownik może publikować komentarze i wyświetlać komentarze dodane po przyłączeniu do rozmowy przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="23a83-124">Each new user can post comments and see comments added after the user joins the chat.</span></span>

![Wystąpienia rozmów](tutorial-getting-started-with-signalr/_static/image1.png)

<span data-ttu-id="23a83-126">Poszczególne</span><span class="sxs-lookup"><span data-stu-id="23a83-126">Sections:</span></span>

- [<span data-ttu-id="23a83-127">Konfigurowanie projektu</span><span class="sxs-lookup"><span data-stu-id="23a83-127">Set up the Project</span></span>](#setup)
- [<span data-ttu-id="23a83-128">Uruchamianie przykładu</span><span class="sxs-lookup"><span data-stu-id="23a83-128">Run the Sample</span></span>](#run)
- [<span data-ttu-id="23a83-129">Sprawdzanie kodu</span><span class="sxs-lookup"><span data-stu-id="23a83-129">Examine the Code</span></span>](#code)
- [<span data-ttu-id="23a83-130">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="23a83-130">Next Steps</span></span>](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a><span data-ttu-id="23a83-131">Konfigurowanie projektu</span><span class="sxs-lookup"><span data-stu-id="23a83-131">Set up the Project</span></span>

<span data-ttu-id="23a83-132">W tej sekcji pokazano, jak utworzyć pustą aplikację sieci Web ASP.NET, dodać program sygnalizujący i utworzyć aplikację czatu.</span><span class="sxs-lookup"><span data-stu-id="23a83-132">This section shows how to create an empty ASP.NET web application, add SignalR, and create the chat application.</span></span>

<span data-ttu-id="23a83-133">Wymagania wstępne:</span><span class="sxs-lookup"><span data-stu-id="23a83-133">Prerequisites:</span></span>

- <span data-ttu-id="23a83-134">Program Visual Studio 2010 z dodatkiem SP1 lub 2012.</span><span class="sxs-lookup"><span data-stu-id="23a83-134">Visual Studio 2010 SP1 or 2012.</span></span> <span data-ttu-id="23a83-135">Jeśli nie masz programu Visual Studio, zobacz [ASP.NET downloads](https://www.asp.net/downloads) , aby uzyskać bezpłatne narzędzie programistyczne programu Visual Studio 2012 Express.</span><span class="sxs-lookup"><span data-stu-id="23a83-135">If you do not have Visual Studio, see [ASP.NET Downloads](https://www.asp.net/downloads) to get the free Visual Studio 2012 Express Development Tool.</span></span>
- <span data-ttu-id="23a83-136">[Microsoft ASP.NET and Web Tools 2012,2](https://go.microsoft.com/fwlink/?LinkId=279941).</span><span class="sxs-lookup"><span data-stu-id="23a83-136">[Microsoft ASP.NET and Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=279941).</span></span> <span data-ttu-id="23a83-137">W przypadku programu Visual Studio 2012 ten Instalator dodaje nowe funkcje ASP.NET, w tym szablony sygnałów do programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="23a83-137">For Visual Studio 2012, this installer adds new ASP.NET features including SignalR templates to Visual Studio.</span></span> <span data-ttu-id="23a83-138">W przypadku programu Visual Studio 2010 z dodatkiem SP1 Instalator nie jest dostępny, ale można wykonać ten samouczek, instalując pakiet NuGet sygnalizujący zgodnie z opisem w kroku instalacji.</span><span class="sxs-lookup"><span data-stu-id="23a83-138">For Visual Studio 2010 SP1, an installer is not available but you can complete the tutorial by installing the SignalR NuGet package as described in the setup steps.</span></span>

<span data-ttu-id="23a83-139">Poniższe kroki używają programu Visual Studio 2012 do utworzenia pustej aplikacji sieci Web ASP.NET i dodania biblioteki sygnałów:</span><span class="sxs-lookup"><span data-stu-id="23a83-139">The following steps use Visual Studio 2012 to create an ASP.NET Empty Web Application and add the SignalR library:</span></span>

1. <span data-ttu-id="23a83-140">W programie Visual Studio Utwórz pustą aplikację sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="23a83-140">In Visual Studio create an ASP.NET Empty Web Application.</span></span>

    ![Tworzenie pustej sieci Web](tutorial-getting-started-with-signalr/_static/image2.png)
2. <span data-ttu-id="23a83-142">Otwórz **konsolę Menedżera pakietów** , wybierając pozycję **Narzędzia | Menedżer pakietów NuGet | Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="23a83-142">Open the **Package Manager Console** by selecting **Tools | NuGet Package Manager | Package Manager Console**.</span></span> <span data-ttu-id="23a83-143">Wprowadź następujące polecenie w oknie konsoli:</span><span class="sxs-lookup"><span data-stu-id="23a83-143">Enter the following command into the console window:</span></span>

    `Install-Package Microsoft.AspNet.SignalR -Version 1.1.3`

    <span data-ttu-id="23a83-144">To polecenie powoduje zainstalowanie najnowszej wersji programu sygnalizującego 1. x.</span><span class="sxs-lookup"><span data-stu-id="23a83-144">This command installs the latest version of SignalR 1.x.</span></span>
3. <span data-ttu-id="23a83-145">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt, a następnie wybierz pozycję **Dodaj | Klasa**.</span><span class="sxs-lookup"><span data-stu-id="23a83-145">In **Solution Explorer**, right-click the project, select **Add | Class**.</span></span> <span data-ttu-id="23a83-146">Nadaj nowej klasie nazwę **ChatHub**.</span><span class="sxs-lookup"><span data-stu-id="23a83-146">Name the new class **ChatHub**.</span></span>
4. <span data-ttu-id="23a83-147">W **Eksplorator rozwiązań** rozwiń węzeł skrypty.</span><span class="sxs-lookup"><span data-stu-id="23a83-147">In **Solution Explorer** expand the Scripts node.</span></span> <span data-ttu-id="23a83-148">Biblioteki skryptów dla jQuery i sygnalizujących są widoczne w projekcie.</span><span class="sxs-lookup"><span data-stu-id="23a83-148">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    ![Odwołania do biblioteki](tutorial-getting-started-with-signalr/_static/image3.png)
5. <span data-ttu-id="23a83-150">Zastąp kod w klasie **ChatHub** następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="23a83-150">Replace the code in the **ChatHub** class with the following code.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. <span data-ttu-id="23a83-151">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt, a następnie kliknij polecenie **Dodaj | Nowy element**.</span><span class="sxs-lookup"><span data-stu-id="23a83-151">In **Solution Explorer**, right-click the project, then click **Add | New Item**.</span></span> <span data-ttu-id="23a83-152">W oknie dialogowym **Dodaj nowy element** wybierz pozycję **globalna Klasa aplikacji** , a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="23a83-152">In the **Add New Item** dialog, select **Global Application Class** and click **Add**.</span></span>

    ![Dodaj globalny](tutorial-getting-started-with-signalr/_static/image4.png)
7. <span data-ttu-id="23a83-154">Dodaj następujące instrukcje `using` po podanych instrukcjach `using` w klasie Global.asax.cs.</span><span class="sxs-lookup"><span data-stu-id="23a83-154">Add the following `using` statements after the provided `using` statements in the Global.asax.cs class.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. <span data-ttu-id="23a83-155">Dodaj następujący wiersz kodu w metodzie `Application_Start` klasy globalnej w celu zarejestrowania trasy domyślnej dla centrów sygnałów.</span><span class="sxs-lookup"><span data-stu-id="23a83-155">Add the following line of code in the `Application_Start` method of the Global class to register the default route for SignalR hubs.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample3.cs)]
9. <span data-ttu-id="23a83-156">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt, a następnie kliknij polecenie **Dodaj | Nowy element**.</span><span class="sxs-lookup"><span data-stu-id="23a83-156">In **Solution Explorer**, right-click the project, then click **Add | New Item**.</span></span> <span data-ttu-id="23a83-157">W oknie dialogowym **Dodaj nowy element** wybierz pozycję Strona HTML, a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="23a83-157">In the **Add New Item** dialog, select Html Page and click **Add**.</span></span>
10. <span data-ttu-id="23a83-158">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy właśnie UTWORZONĄ stronę HTML, a następnie kliknij pozycję **Ustaw jako stronę startową**.</span><span class="sxs-lookup"><span data-stu-id="23a83-158">In **Solution Explorer**, right-click the HTML page you just created and click **Set as Start Page**.</span></span>
11. <span data-ttu-id="23a83-159">Zastąp domyślny kod na stronie HTML następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="23a83-159">Replace the default code in the HTML page with the following code.</span></span>

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample4.html)]
12. <span data-ttu-id="23a83-160">**Zapisz wszystko** dla projektu.</span><span class="sxs-lookup"><span data-stu-id="23a83-160">**Save All** for the project.</span></span>

<a id="run"></a>

## <a name="run-the-sample"></a><span data-ttu-id="23a83-161">Uruchamianie przykładu</span><span class="sxs-lookup"><span data-stu-id="23a83-161">Run the Sample</span></span>

1. <span data-ttu-id="23a83-162">Naciśnij klawisz F5, aby uruchomić projekt w trybie debugowania.</span><span class="sxs-lookup"><span data-stu-id="23a83-162">Press F5 to run the project in debug mode.</span></span> <span data-ttu-id="23a83-163">Strona HTML zostanie załadowana w wystąpieniu przeglądarki i zostanie wyświetlony komunikat z prośbą o nazwę użytkownika.</span><span class="sxs-lookup"><span data-stu-id="23a83-163">The HTML page loads in a browser instance and prompts for a user name.</span></span>

    ![Wprowadź nazwę użytkownika](tutorial-getting-started-with-signalr/_static/image5.png)
2. <span data-ttu-id="23a83-165">Wprowadź nazwę użytkownika.</span><span class="sxs-lookup"><span data-stu-id="23a83-165">Enter a user name.</span></span>
3. <span data-ttu-id="23a83-166">Skopiuj adres URL z linii adresowej przeglądarki i użyj go, aby otworzyć dwa więcej wystąpień przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="23a83-166">Copy the URL from the address line of the browser and use it to open two more browser instances.</span></span> <span data-ttu-id="23a83-167">W każdym wystąpieniu przeglądarki wprowadź unikatową nazwę użytkownika.</span><span class="sxs-lookup"><span data-stu-id="23a83-167">In each browser instance, enter a unique user name.</span></span>
4. <span data-ttu-id="23a83-168">W każdym wystąpieniu przeglądarki Dodaj komentarz, a następnie kliknij przycisk **Wyślij**.</span><span class="sxs-lookup"><span data-stu-id="23a83-168">In each browser instance, add a comment and click **Send**.</span></span> <span data-ttu-id="23a83-169">Komentarze powinny być wyświetlane we wszystkich wystąpieniach przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="23a83-169">The comments should display in all browser instances.</span></span>

    > [!NOTE]
    > <span data-ttu-id="23a83-170">Ta prosta aplikacja czatu nie utrzymuje kontekstu dyskusji na serwerze.</span><span class="sxs-lookup"><span data-stu-id="23a83-170">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="23a83-171">Centrum emituje Komentarze do wszystkich bieżących użytkowników.</span><span class="sxs-lookup"><span data-stu-id="23a83-171">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="23a83-172">Użytkownicy, którzy dołączą do rozmowy później zobaczą komunikaty dodawane od momentu dołączenia.</span><span class="sxs-lookup"><span data-stu-id="23a83-172">Users who join the chat later will see messages added from the time they join.</span></span>

    <span data-ttu-id="23a83-173">Poniższy zrzut ekranu przedstawia aplikację czatu działającą w trzech wystąpieniach przeglądarki, z której wszystkie są aktualizowane, gdy jedno wystąpienie wysyła komunikat:</span><span class="sxs-lookup"><span data-stu-id="23a83-173">The following screen shot shows the chat application running in three browser instances, all of which are updated when one instance sends a message:</span></span>

    ![Przeglądarki rozmowy](tutorial-getting-started-with-signalr/_static/image6.png)
5. <span data-ttu-id="23a83-175">W **Eksplorator rozwiązań**Sprawdź, czy węzeł **dokumenty skryptu** dla uruchomionej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="23a83-175">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="23a83-176">Istnieje plik skryptu o nazwie **Hubs** , który dynamicznie generuje Biblioteka sygnałów w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="23a83-176">There is a script file named **hubs** that the SignalR library dynamically generates at runtime.</span></span> <span data-ttu-id="23a83-177">Ten plik zarządza komunikacją między skryptami jQuery i kodem po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="23a83-177">This file manages the communication between jQuery script and server-side code.</span></span>

    ![Wygenerowany skrypt centralny](tutorial-getting-started-with-signalr/_static/image7.png)

<a id="code"></a>

## <a name="examine-the-code"></a><span data-ttu-id="23a83-179">Sprawdzanie kodu</span><span class="sxs-lookup"><span data-stu-id="23a83-179">Examine the Code</span></span>

<span data-ttu-id="23a83-180">Aplikacja rozmowy sygnalizująca pokazuje dwa podstawowe zadania programistyczne sygnalizujące: tworzenie centrum jako głównego obiektu koordynacji na serwerze i używanie biblioteki sygnalizującej jQuery do wysyłania i odbierania wiadomości.</span><span class="sxs-lookup"><span data-stu-id="23a83-180">The SignalR chat application demonstrates two basic SignalR development tasks: creating a hub as the main coordination object on the server, and using the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs"></a><span data-ttu-id="23a83-181">Centra sygnałów</span><span class="sxs-lookup"><span data-stu-id="23a83-181">SignalR Hubs</span></span>

<span data-ttu-id="23a83-182">W przykładzie kodu Klasa **ChatHub** pochodzi od klasy **Microsoft. ASPNET. Signaler. Hub** .</span><span class="sxs-lookup"><span data-stu-id="23a83-182">In the code sample the **ChatHub** class derives from the **Microsoft.AspNet.SignalR.Hub** class.</span></span> <span data-ttu-id="23a83-183">Wyprowadzanie z klasy **centrów** to przydatny sposób tworzenia aplikacji sygnalizującej.</span><span class="sxs-lookup"><span data-stu-id="23a83-183">Deriving from the **Hub** class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="23a83-184">Można tworzyć metody publiczne w klasie centrów, a następnie uzyskiwać dostęp do tych metod, wywołując je ze skryptów jQuery na stronie sieci Web.</span><span class="sxs-lookup"><span data-stu-id="23a83-184">You can create public methods on your hub class and then access those methods by calling them from jQuery scripts in a web page.</span></span>

<span data-ttu-id="23a83-185">W kodzie rozmowy klienci wywołują metodę **ChatHub. Send** w celu wysłania nowej wiadomości.</span><span class="sxs-lookup"><span data-stu-id="23a83-185">In the chat code, clients call the **ChatHub.Send** method to send a new message.</span></span> <span data-ttu-id="23a83-186">Koncentrator z kolei wysyła komunikat do wszystkich klientów, wywołując **klientów. ALL. broadcastMessage**.</span><span class="sxs-lookup"><span data-stu-id="23a83-186">The hub in turn sends the message to all clients by calling **Clients.All.broadcastMessage**.</span></span>

<span data-ttu-id="23a83-187">Metoda **send** ilustruje kilka pojęć centrum:</span><span class="sxs-lookup"><span data-stu-id="23a83-187">The **Send** method demonstrates several hub concepts :</span></span>

- <span data-ttu-id="23a83-188">Zadeklaruj metody publiczne w centrum, aby klienci mogli je wywoływać.</span><span class="sxs-lookup"><span data-stu-id="23a83-188">Declare public methods on a hub so that clients can call them.</span></span>
- <span data-ttu-id="23a83-189">Do uzyskiwania dostępu do wszystkich klientów podłączonych do tego centrum należy używać właściwości dynamicznej **Microsoft. ASPNET. signaler. Hub**</span><span class="sxs-lookup"><span data-stu-id="23a83-189">Use the **Microsoft.AspNet.SignalR.Hub.Clients** dynamic property to access all clients connected to this hub.</span></span>
- <span data-ttu-id="23a83-190">Wywołaj funkcję jQuery na kliencie (na przykład funkcję `broadcastMessage`), aby zaktualizować klientów.</span><span class="sxs-lookup"><span data-stu-id="23a83-190">Call a jQuery function on the client (such as the `broadcastMessage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a><span data-ttu-id="23a83-191">Sygnalizujący i jQuery</span><span class="sxs-lookup"><span data-stu-id="23a83-191">SignalR and jQuery</span></span>

<span data-ttu-id="23a83-192">Na stronie HTML w przykładzie kodu pokazano, jak używać biblioteki sygnalizującej jQuery do komunikowania się z centrum sygnałów.</span><span class="sxs-lookup"><span data-stu-id="23a83-192">The HTML page in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="23a83-193">Zasadnicze zadania w kodzie deklarują serwer proxy, aby odwołać się do centrum, deklarując funkcję, którą serwer może wywoływać, aby wypchnąć zawartość do klientów i rozpocząć połączenie w celu wysyłania komunikatów do centrum.</span><span class="sxs-lookup"><span data-stu-id="23a83-193">The essential tasks in the code are declaring a proxy to reference the hub, declaring a function that the server can call to push content to clients, and starting a connection to send messages to the hub.</span></span>

<span data-ttu-id="23a83-194">Poniższy kod deklaruje serwer proxy dla centrum.</span><span class="sxs-lookup"><span data-stu-id="23a83-194">The following code declares a proxy for a hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample6.js)]

> [!NOTE]
> <span data-ttu-id="23a83-195">W jQuery odwołanie do klasy serwera i jej członków jest w notacji CamelCase przypadku.</span><span class="sxs-lookup"><span data-stu-id="23a83-195">In jQuery the reference to the server class and its members is in camel case.</span></span> <span data-ttu-id="23a83-196">Przykładowy kod odwołuje się C# do klasy **ChatHub** w jQuery jako **ChatHub**.</span><span class="sxs-lookup"><span data-stu-id="23a83-196">The code sample references the C# **ChatHub** class in jQuery as **chatHub**.</span></span>

<span data-ttu-id="23a83-197">Poniższy kod przedstawia sposób tworzenia funkcji wywołania zwrotnego w skrypcie.</span><span class="sxs-lookup"><span data-stu-id="23a83-197">The following code is how you create a callback function in the script.</span></span> <span data-ttu-id="23a83-198">Klasa centrum na serwerze wywołuje tę funkcję, aby wypchnąć aktualizacje zawartości do każdego klienta.</span><span class="sxs-lookup"><span data-stu-id="23a83-198">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="23a83-199">Dwa wiersze, w których kod HTML koduje zawartość przed wyświetleniem, są opcjonalne i pokazują prostą metodę zapobiegania iniekcji skryptu.</span><span class="sxs-lookup"><span data-stu-id="23a83-199">The two lines that HTML encode the content before displaying it are optional and show a simple way to prevent script injection.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample7.html)]

<span data-ttu-id="23a83-200">Poniższy kod pokazuje, jak otworzyć połączenie z centrum.</span><span class="sxs-lookup"><span data-stu-id="23a83-200">The following code shows how to open a connection with the hub.</span></span> <span data-ttu-id="23a83-201">Kod uruchamia połączenie, a następnie przekazuje go do funkcji, aby obsłużyć zdarzenie kliknięcia na przycisku **Wyślij** na stronie HTML.</span><span class="sxs-lookup"><span data-stu-id="23a83-201">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the HTML page.</span></span>

> [!NOTE]
> <span data-ttu-id="23a83-202">Takie podejście umożliwia upewnienie się, że połączenie zostało nawiązane przed wykonaniem procedury obsługi zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="23a83-202">This approach insures that the connection is established before the event handler executes.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a><span data-ttu-id="23a83-203">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="23a83-203">Next Steps</span></span>

<span data-ttu-id="23a83-204">Zapoznaj się z tym, że sygnalizujący jest platforma do tworzenia aplikacji sieci Web w czasie rzeczywistym.</span><span class="sxs-lookup"><span data-stu-id="23a83-204">You learned that SignalR is a framework for building real-time web applications.</span></span> <span data-ttu-id="23a83-205">Poznasz również kilka zadań programistycznych sygnalizujących: jak dodać odbiornik do aplikacji ASP.NET, jak utworzyć klasę centrum oraz jak wysyłać i odbierać komunikaty z centrum.</span><span class="sxs-lookup"><span data-stu-id="23a83-205">You also learned several SignalR development tasks: how to add SignalR to an ASP.NET application, how to create a hub class, and how to send and receive messages from the hub.</span></span>

<span data-ttu-id="23a83-206">Przykładową aplikację można utworzyć w tym samouczku lub innych aplikacjach sygnalizujących dostępnych za pośrednictwem Internetu, wdrażając je u dostawcy hostingu.</span><span class="sxs-lookup"><span data-stu-id="23a83-206">You can make the sample application in this tutorial or other SignalR applications available over the Internet by deploying them to a hosting provider.</span></span> <span data-ttu-id="23a83-207">Firma Microsoft oferuje bezpłatny hosting w sieci Web dla maksymalnie 10 witryn sieci Web w ramach bezpłatnego [konta wersji próbnej platformy Microsoft Azure](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604).</span><span class="sxs-lookup"><span data-stu-id="23a83-207">Microsoft offers free web hosting for up to 10 web sites in a free [Windows Azure trial account](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604).</span></span> <span data-ttu-id="23a83-208">Aby zapoznać się z przewodnikiem dotyczącym wdrażania przykładowej aplikacji sygnalizującej, zobacz temat [publikowanie wprowadzenie próbnika jako witryny sieci Web systemu Windows Azure](https://blogs.msdn.com/b/timlee/archive/2013/02/27/deploy-the-signalr-getting-started-sample-as-a-windows-azure-web-site.aspx).</span><span class="sxs-lookup"><span data-stu-id="23a83-208">For a walkthrough on how to deploy the sample SignalR application, see [Publish the SignalR Getting Started Sample as a Windows Azure Web Site](https://blogs.msdn.com/b/timlee/archive/2013/02/27/deploy-the-signalr-getting-started-sample-as-a-windows-azure-web-site.aspx).</span></span> <span data-ttu-id="23a83-209">Aby uzyskać szczegółowe informacje na temat wdrażania projektu sieci Web programu Visual Studio w witrynie sieci Web systemu Windows Azure, zobacz [wdrażanie aplikacji ASP.NET w witrynie sieci Web systemu Windows Azure](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).</span><span class="sxs-lookup"><span data-stu-id="23a83-209">For detailed information about how to deploy a Visual Studio web project to a Windows Azure Web Site, see [Deploying an ASP.NET Application to a Windows Azure Web Site](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).</span></span> <span data-ttu-id="23a83-210">(Uwaga: transport WebSocket nie jest obecnie obsługiwany w witrynach sieci Web systemu Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="23a83-210">(Note: The WebSocket transport is not currently supported for Windows Azure Web Sites.</span></span> <span data-ttu-id="23a83-211">Gdy transport WebSocket nie jest dostępny, sygnalizujący używa innych dostępnych transportów, zgodnie z opisem w sekcji Transports w [temacie Wprowadzenie do sygnalizacji](index.md).</span><span class="sxs-lookup"><span data-stu-id="23a83-211">When WebSocket transport is not available, SignalR uses the other available transports as described in the Transports section of the [Introduction to SignalR topic](index.md).)</span></span>

<span data-ttu-id="23a83-212">Aby dowiedzieć się więcej na temat zaawansowanych pojęć związanych z założeniami, odwiedź następujące witryny dotyczące kodu źródłowego i zasobów usługi sygnalizującego:</span><span class="sxs-lookup"><span data-stu-id="23a83-212">To learn more advanced SignalR developments concepts, visit the following sites for SignalR source code and resources:</span></span>

- [<span data-ttu-id="23a83-213">Projekt sygnalizującego</span><span class="sxs-lookup"><span data-stu-id="23a83-213">SignalR Project</span></span>](http://signalr.net)
- [<span data-ttu-id="23a83-214">Usługi GitHub i przykłady dla sygnałów</span><span class="sxs-lookup"><span data-stu-id="23a83-214">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="23a83-215">Strona typu wiki sygnalizująca</span><span class="sxs-lookup"><span data-stu-id="23a83-215">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)

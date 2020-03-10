---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
title: 'Część 2: kontrolery | Microsoft Docs'
author: jongalloway
description: Ta seria samouczków zawiera szczegółowe informacje na temat wszystkich kroków podjętych w celu skompilowania przykładowej aplikacji do sklepu ASP.NET MVC Music. Część 2 obejmuje kontrolery.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 998ce4e1-9d72-435b-8f1c-399a10ae4360
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
msc.type: authoredcontent
ms.openlocfilehash: 9dc2226f4951d4bed122df37d35bbb94730a00ad
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78559879"
---
# <a name="part-2-controllers"></a><span data-ttu-id="aea36-104">Część 2. Kontrolery</span><span class="sxs-lookup"><span data-stu-id="aea36-104">Part 2: Controllers</span></span>

<span data-ttu-id="aea36-105">przez [Jan Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="aea36-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="aea36-106">Sklep MVC Music jest aplikacją samouczka, która wprowadza i objaśnia krok po kroku, jak używać ASP.NET MVC i Visual Studio do programowania w sieci Web.</span><span class="sxs-lookup"><span data-stu-id="aea36-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="aea36-107">Sklep MVC Music jest lekkim przykładowym wdrożeniem magazynu, który sprzedaje Albumy muzyczne w trybie online i implementuje podstawowe funkcje administracyjne, logowania użytkownika i koszyka.</span><span class="sxs-lookup"><span data-stu-id="aea36-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="aea36-108">Ta seria samouczków zawiera szczegółowe informacje na temat wszystkich kroków podjętych w celu skompilowania przykładowej aplikacji do sklepu ASP.NET MVC Music.</span><span class="sxs-lookup"><span data-stu-id="aea36-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="aea36-109">Część 2 obejmuje kontrolery.</span><span class="sxs-lookup"><span data-stu-id="aea36-109">Part 2 covers Controllers.</span></span>

<span data-ttu-id="aea36-110">W przypadku tradycyjnych platform sieci Web przychodzące adresy URL są zazwyczaj mapowane na pliki na dysku.</span><span class="sxs-lookup"><span data-stu-id="aea36-110">With traditional web frameworks, incoming URLs are typically mapped to files on disk.</span></span> <span data-ttu-id="aea36-111">Na przykład: żądanie dotyczące adresu URL, takiego jak "/Products.aspx" lub "/Products.php", może być przetwarzane przez plik "Products. aspx" lub "Products. php".</span><span class="sxs-lookup"><span data-stu-id="aea36-111">For example: a request for a URL like "/Products.aspx" or "/Products.php" might be processed by a "Products.aspx" or "Products.php" file.</span></span>

<span data-ttu-id="aea36-112">Sieci Web platformy MVC mapują adresy URL na kod serwera w nieco inny sposób.</span><span class="sxs-lookup"><span data-stu-id="aea36-112">Web-based MVC frameworks map URLs to server code in a slightly different way.</span></span> <span data-ttu-id="aea36-113">Zamiast mapować przychodzące adresy URL na pliki, zamiast tego mapują adresy URL na metody klasy.</span><span class="sxs-lookup"><span data-stu-id="aea36-113">Instead of mapping incoming URLs to files, they instead map URLs to methods on classes.</span></span> <span data-ttu-id="aea36-114">Klasy te są nazywane "kontrolerami" i są odpowiedzialne za przetwarzanie przychodzących żądań HTTP, obsługę danych wejściowych użytkownika, pobieranie i zapisywanie danych oraz określanie odpowiedzi na odesłanie do klienta (wyświetlanie kodu HTML, pobieranie pliku, przekierowanie do innego Adres URL itp.).</span><span class="sxs-lookup"><span data-stu-id="aea36-114">These classes are called "Controllers" and they are responsible for processing incoming HTTP requests, handling user input, retrieving and saving data, and determining the response to send back to the client (display HTML, download a file, redirect to a different URL, etc.).</span></span>

## <a name="adding-a-homecontroller"></a><span data-ttu-id="aea36-115">Dodawanie elementu HomeController</span><span class="sxs-lookup"><span data-stu-id="aea36-115">Adding a HomeController</span></span>

<span data-ttu-id="aea36-116">Zaczniemy nasze aplikacje ze sklepu MVC Music, dodając klasę kontrolera, która będzie obsługiwać adresy URL na stronie głównej naszej witryny.</span><span class="sxs-lookup"><span data-stu-id="aea36-116">We'll begin our MVC Music Store application by adding a Controller class that will handle URLs to the Home page of our site.</span></span> <span data-ttu-id="aea36-117">Będziemy przestrzegać domyślnych konwencji nazewnictwa ASP.NET MVC i wywoływać IT HomeController.</span><span class="sxs-lookup"><span data-stu-id="aea36-117">We'll follow the default naming conventions of ASP.NET MVC and call it HomeController.</span></span>

<span data-ttu-id="aea36-118">Kliknij prawym przyciskiem myszy folder "controllers" w ramach Eksplorator rozwiązań i wybierz pozycję "Dodaj", a następnie "kontroler..." dotyczące</span><span class="sxs-lookup"><span data-stu-id="aea36-118">Right-click the "Controllers" folder within the Solution Explorer and select "Add", and then the "Controller…" command:</span></span>

![](mvc-music-store-part-2/_static/image1.jpg)

<span data-ttu-id="aea36-119">Spowoduje to wyświetlenie okna dialogowego "Dodawanie kontrolera".</span><span class="sxs-lookup"><span data-stu-id="aea36-119">This will bring up the "Add Controller" dialog.</span></span> <span data-ttu-id="aea36-120">Nazwij kontroler "HomeController" i naciśnij przycisk Dodaj.</span><span class="sxs-lookup"><span data-stu-id="aea36-120">Name the controller "HomeController" and press the Add button.</span></span>

![](mvc-music-store-part-2/_static/image1.png)

<span data-ttu-id="aea36-121">Spowoduje to utworzenie nowego pliku HomeController.cs z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="aea36-121">This will create a new file, HomeController.cs, with the following code:</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample1.cs)]

<span data-ttu-id="aea36-122">Aby zacząć jak najszybciej, Zamień metodę index na prostą metodę, która po prostu zwraca ciąg.</span><span class="sxs-lookup"><span data-stu-id="aea36-122">To start as simply as possible, let's replace the Index method with a simple method that just returns a string.</span></span> <span data-ttu-id="aea36-123">Wprowadzimy dwie zmiany:</span><span class="sxs-lookup"><span data-stu-id="aea36-123">We'll make two changes:</span></span>

- <span data-ttu-id="aea36-124">Zmień metodę, aby zwracała ciąg zamiast elementu ActionResult</span><span class="sxs-lookup"><span data-stu-id="aea36-124">Change the method to return a string instead of an ActionResult</span></span>
- <span data-ttu-id="aea36-125">Zmień instrukcję return, aby zwracała wartość "Hello z domu"</span><span class="sxs-lookup"><span data-stu-id="aea36-125">Change the return statement to return "Hello from Home"</span></span>

<span data-ttu-id="aea36-126">Metoda powinna teraz wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="aea36-126">The method should now look like this:</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample2.cs)]

## <a name="running-the-application"></a><span data-ttu-id="aea36-127">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="aea36-127">Running the Application</span></span>

<span data-ttu-id="aea36-128">Teraz Uruchommy lokację.</span><span class="sxs-lookup"><span data-stu-id="aea36-128">Now let's run the site.</span></span> <span data-ttu-id="aea36-129">Możemy uruchomić nasz serwer sieci Web i wypróbować lokację przy użyciu dowolnego z następujących elementów:</span><span class="sxs-lookup"><span data-stu-id="aea36-129">We can start our web-server and try out the site using any of the following::</span></span>

- <span data-ttu-id="aea36-130">Wybierz element menu Debuguj debugowanie ⇨</span><span class="sxs-lookup"><span data-stu-id="aea36-130">Choose the Debug ⇨ Start Debugging menu item</span></span>
- <span data-ttu-id="aea36-131">Kliknij przycisk Zielona strzałka na pasku narzędzi ![](mvc-music-store-part-2/_static/image2.jpg)</span><span class="sxs-lookup"><span data-stu-id="aea36-131">Click the Green arrow button in the toolbar ![](mvc-music-store-part-2/_static/image2.jpg)</span></span>
- <span data-ttu-id="aea36-132">Użyj skrótu klawiaturowego F5.</span><span class="sxs-lookup"><span data-stu-id="aea36-132">Use the keyboard shortcut, F5.</span></span>

<span data-ttu-id="aea36-133">Użycie któregokolwiek z powyższych kroków spowoduje skompilowanie projektu, a następnie uruchomienie serwera deweloperskiego ASP.NET, który jest wbudowanym Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="aea36-133">Using any of the above steps will compile our project, and then cause the ASP.NET Development Server that is built-into Visual Web Developer to start.</span></span> <span data-ttu-id="aea36-134">W dolnym rogu ekranu zostanie wyświetlone powiadomienie informujące o tym, że serwer ASP.NET Development został uruchomiony i zostanie wyświetlony numer portu, w którym jest uruchomiony.</span><span class="sxs-lookup"><span data-stu-id="aea36-134">A notification will appear in the bottom corner of the screen to indicate that the ASP.NET Development Server has started up, and will show the port number that it is running under.</span></span>

![](mvc-music-store-part-2/_static/image2.png)

<span data-ttu-id="aea36-135">Visual Web Developer następnie automatycznie otworzy okno przeglądarki, którego adres URL wskazuje nasz serwer sieci Web.</span><span class="sxs-lookup"><span data-stu-id="aea36-135">Visual Web Developer will then automatically open a browser window whose URL points to our web-server.</span></span> <span data-ttu-id="aea36-136">Pozwoli to na szybkie wypróbowanie naszej aplikacji sieci Web:</span><span class="sxs-lookup"><span data-stu-id="aea36-136">This will allow us to quickly try out our web application:</span></span>

![](mvc-music-store-part-2/_static/image3.png)

<span data-ttu-id="aea36-137">Tak stało się bardzo szybkie — utworzyliśmy nową witrynę sieci Web, dodaliśmy funkcję z trzema wierszami, a w przeglądarce mamy tekst.</span><span class="sxs-lookup"><span data-stu-id="aea36-137">Okay, that was pretty quick – we created a new website, added a three line function, and we've got text in a browser.</span></span> <span data-ttu-id="aea36-138">Nie nauka rakiet, ale jest rozpoczęciem.</span><span class="sxs-lookup"><span data-stu-id="aea36-138">Not rocket science, but it's a start.</span></span>

<span data-ttu-id="aea36-139">*Uwaga: Visual Web Developer zawiera serwer ASP.NET Development, który będzie uruchamiał witrynę internetową przy użyciu losowego, bezpłatnego numeru "Port". Na poniższym zrzucie ekranu lokacja jest uruchomiona w `http://localhost:26641/`, więc korzysta z portu 26641. Numer portu będzie różny. Gdy będziemy komunikować się z adresem URL podobnym do/Store/Browse w tym samouczku, będzie on przekroczyć numer portu. Przy założeniu, że numer portu 26641, przeglądanie do/Store/Browse będzie oznaczało przechodzenie do `http://localhost:26641/Store/Browse`.*</span><span class="sxs-lookup"><span data-stu-id="aea36-139">*Note: Visual Web Developer includes the ASP.NET Development Server, which will run your website on a random free "port" number. In the screenshot above, the site is running at `http://localhost:26641/`, so it's using port 26641. Your port number will be different. When we talk about URL's like /Store/Browse in this tutorial, that will go after the port number. Assuming a port number of 26641, browsing to /Store/Browse will mean browsing to `http://localhost:26641/Store/Browse`.*</span></span>

## <a name="adding-a-storecontroller"></a><span data-ttu-id="aea36-140">Dodawanie elementu StoreController</span><span class="sxs-lookup"><span data-stu-id="aea36-140">Adding a StoreController</span></span>

<span data-ttu-id="aea36-141">Dodaliśmy prostą HomeController, która implementuje stronę główną naszej witryny.</span><span class="sxs-lookup"><span data-stu-id="aea36-141">We added a simple HomeController that implements the Home Page of our site.</span></span> <span data-ttu-id="aea36-142">Dodajmy teraz kolejny kontroler, którego będziemy używać do implementowania funkcji przeglądania naszego sklepu z muzyką.</span><span class="sxs-lookup"><span data-stu-id="aea36-142">Let's now add another controller that we'll use to implement the browsing functionality of our music store.</span></span> <span data-ttu-id="aea36-143">Nasz kontroler sklepu będzie obsługiwał trzy scenariusze:</span><span class="sxs-lookup"><span data-stu-id="aea36-143">Our store controller will support three scenarios:</span></span>

- <span data-ttu-id="aea36-144">Strona z listą gatunków utworów muzycznych w naszym sklepie z muzyką</span><span class="sxs-lookup"><span data-stu-id="aea36-144">A listing page of the music genres in our music store</span></span>
- <span data-ttu-id="aea36-145">Strona przeglądania zawierająca listę wszystkich albumów muzycznych w konkretnym gatunku</span><span class="sxs-lookup"><span data-stu-id="aea36-145">A browse page that lists all of the music albums in a particular genre</span></span>
- <span data-ttu-id="aea36-146">Strona szczegółów, która zawiera informacje o konkretnym albumie muzycznym</span><span class="sxs-lookup"><span data-stu-id="aea36-146">A details page that shows information about a specific music album</span></span>

<span data-ttu-id="aea36-147">Zaczniemy od dodania nowej klasy StoreController..</span><span class="sxs-lookup"><span data-stu-id="aea36-147">We'll start by adding a new StoreController class..</span></span> <span data-ttu-id="aea36-148">Jeśli jeszcze tego nie zrobiono, Zatrzymaj uruchamianie aplikacji przez zamknięcie przeglądarki lub wybranie elementu menu Debuguj ⇨ zatrzymanie debugowania.</span><span class="sxs-lookup"><span data-stu-id="aea36-148">If you haven't already, stop running the application either by closing the browser or selecting the Debug ⇨ Stop Debugging menu item.</span></span>

<span data-ttu-id="aea36-149">Teraz Dodaj nowy StoreController.</span><span class="sxs-lookup"><span data-stu-id="aea36-149">Now add a new StoreController.</span></span> <span data-ttu-id="aea36-150">Podobnie jak w przypadku HomeController, wykonajmy tę czynność, klikając prawym przyciskiem myszy folder "controllers" w Eksplorator rozwiązań i wybierając pozycję menu kontrolera Dodaj kontroler&gt;</span><span class="sxs-lookup"><span data-stu-id="aea36-150">Just like we did with HomeController, we'll do this by right-clicking on the "Controllers" folder within the Solution Explorer and choosing the Add-&gt;Controller menu item</span></span>

![](mvc-music-store-part-2/_static/image4.png)

<span data-ttu-id="aea36-151">Nasz nowy StoreController ma już metodę "index".</span><span class="sxs-lookup"><span data-stu-id="aea36-151">Our new StoreController already has an "Index" method.</span></span> <span data-ttu-id="aea36-152">Użyjemy tej metody "index", aby zaimplementować naszą stronę aukcji, która zawiera listę wszystkich gatunków w naszym sklepie z muzyką.</span><span class="sxs-lookup"><span data-stu-id="aea36-152">We'll use this "Index" method to implement our listing page that lists all genres in our music store.</span></span> <span data-ttu-id="aea36-153">Będziemy również dodawać dwie dodatkowe metody, aby zaimplementować te dwa inne scenariusze, które chcemy obsługiwać nasze StoreController: Przeglądaj i szczegóły.</span><span class="sxs-lookup"><span data-stu-id="aea36-153">We'll also add two additional methods to implement the two other scenarios we want our StoreController to handle: Browse and Details.</span></span>

<span data-ttu-id="aea36-154">Te metody (index, Browse i Details) w naszym kontrolerze nazywają się "akcjami kontrolera", jak już widzisz metodę akcji HomeController. index (), ich zadaniem jest odpowiadanie na żądania adresów URL i (ogólnie mówiąc) Ustalanie zawartości powinien zostać wysłany z powrotem do przeglądarki lub użytkownika, który wywołał adres URL.</span><span class="sxs-lookup"><span data-stu-id="aea36-154">These methods (Index, Browse and Details) within our Controller are called "Controller Actions", and as you've already seen with the HomeController.Index()action method, their job is to respond to URL requests and (generally speaking) determine what content should be sent back to the browser or user that invoked the URL.</span></span>

<span data-ttu-id="aea36-155">Zaczniemy nasze implementacje StoreController, zmieniając metodę theIndex () w celu zwrócenia ciągu "Hello from Store. index ()" i dodamy podobne metody dla przeglądania () i szczegółów ():</span><span class="sxs-lookup"><span data-stu-id="aea36-155">We'll start our StoreController implementation by changing theIndex() method to return the string "Hello from Store.Index()" and we'll add similar methods for Browse() and Details():</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample3.cs)]

<span data-ttu-id="aea36-156">Uruchom projekt ponownie i przejdź do następujących adresów URL:</span><span class="sxs-lookup"><span data-stu-id="aea36-156">Run the project again and browse the following URLs:</span></span>

- <span data-ttu-id="aea36-157">/Store</span><span class="sxs-lookup"><span data-stu-id="aea36-157">/Store</span></span>
- <span data-ttu-id="aea36-158">/Store/Browse</span><span class="sxs-lookup"><span data-stu-id="aea36-158">/Store/Browse</span></span>
- <span data-ttu-id="aea36-159">/Store/Details</span><span class="sxs-lookup"><span data-stu-id="aea36-159">/Store/Details</span></span>

<span data-ttu-id="aea36-160">Dostęp do tych adresów URL spowoduje wywołanie metod akcji w naszym kontrolerze i zwrócenie odpowiedzi na ciąg:</span><span class="sxs-lookup"><span data-stu-id="aea36-160">Accessing these URLs will invoke the action methods within our Controller and return string responses:</span></span>

![](mvc-music-store-part-2/_static/image5.png)

<span data-ttu-id="aea36-161">Jest to doskonałe rozwiązanie, ale są to tylko stałe ciągi.</span><span class="sxs-lookup"><span data-stu-id="aea36-161">That's great, but these are just constant strings.</span></span> <span data-ttu-id="aea36-162">Zmieńmy je na dynamiczne, aby pobierali informacje z adresu URL i wyświetlały je w danych wyjściowych strony.</span><span class="sxs-lookup"><span data-stu-id="aea36-162">Let's make them dynamic, so they take information from the URL and display it in the page output.</span></span>

<span data-ttu-id="aea36-163">Najpierw zmienimy metodę przeglądania w celu pobrania wartości QueryString z adresu URL.</span><span class="sxs-lookup"><span data-stu-id="aea36-163">First we'll change the Browse action method to retrieve a querystring value from the URL.</span></span> <span data-ttu-id="aea36-164">Możemy to zrobić, dodając parametr "gatunek" do naszej metody działania.</span><span class="sxs-lookup"><span data-stu-id="aea36-164">We can do this by adding a "genre" parameter to our action method.</span></span> <span data-ttu-id="aea36-165">Gdy to zrobisz, ASP.NET MVC automatycznie przekaże wszystkie parametry kwerendy lub formularza post o nazwie "gatunek" do naszej metody działania, gdy zostanie wywołana.</span><span class="sxs-lookup"><span data-stu-id="aea36-165">When we do this ASP.NET MVC will automatically pass any querystring or form post parameters named "genre" to our action method when it is invoked.</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample4.cs)]

<span data-ttu-id="aea36-166">*Uwaga: używamy metody narzędzia HttpUtility. HtmlEncode do oczyszczenia danych wejściowych użytkownika. Uniemożliwia to użytkownikom wprowadzanie kodu JavaScript do naszego widoku przy użyciu linku, takiego jak/Store/Browse? Gatunek =&lt;&gt;Window. Location = "http://hackersite.com"&lt;/Script&gt;.*</span><span class="sxs-lookup"><span data-stu-id="aea36-166">*Note: We're using the HttpUtility.HtmlEncode utility method to sanitize the user input. This prevents users from injecting Javascript into our View with a link like /Store/Browse?Genre=&lt;script&gt;window.location='http://hackersite.com'&lt;/script&gt;.*</span></span>

<span data-ttu-id="aea36-167">Teraz przejdźmy do/Store/Browse? Gatunek = Disco</span><span class="sxs-lookup"><span data-stu-id="aea36-167">Now let's browse to /Store/Browse?Genre=Disco</span></span>

![](mvc-music-store-part-2/_static/image6.png)

<span data-ttu-id="aea36-168">Następnie zmień akcję szczegóły w celu odczytania i wyświetlenia parametru wejściowego o nazwie ID.</span><span class="sxs-lookup"><span data-stu-id="aea36-168">Let's next change the Details action to read and display an input parameter named ID.</span></span> <span data-ttu-id="aea36-169">W przeciwieństwie do poprzedniej metody, nie będziemy osadzać wartości identyfikatora jako parametru QueryString.</span><span class="sxs-lookup"><span data-stu-id="aea36-169">Unlike our previous method, we won't be embedding the ID value as a querystring parameter.</span></span> <span data-ttu-id="aea36-170">Zamiast tego osadzimy go bezpośrednio w samym adresie URL.</span><span class="sxs-lookup"><span data-stu-id="aea36-170">Instead we'll embed it directly within the URL itself.</span></span> <span data-ttu-id="aea36-171">Na przykład:/Store/Details/5.</span><span class="sxs-lookup"><span data-stu-id="aea36-171">For example: /Store/Details/5.</span></span>

<span data-ttu-id="aea36-172">ASP.NET MVC pozwala łatwo to zrobić bez konieczności konfigurowania żadnych elementów.</span><span class="sxs-lookup"><span data-stu-id="aea36-172">ASP.NET MVC lets us easily do this without having to configure anything.</span></span> <span data-ttu-id="aea36-173">Domyślna konwencja routingu ASP.NET MVC to traktowanie segmentu adresu URL po nazwie metody akcji jako parametru o nazwie "ID".</span><span class="sxs-lookup"><span data-stu-id="aea36-173">ASP.NET MVC's default routing convention is to treat the segment of a URL after the action method name as a parameter named "ID".</span></span> <span data-ttu-id="aea36-174">Jeśli metoda akcji ma parametr o nazwie ID, ASP.NET MVC automatycznie przekaże segment adresu URL jako parametr.</span><span class="sxs-lookup"><span data-stu-id="aea36-174">If your action method has a parameter named ID then ASP.NET MVC will automatically pass the URL segment to you as a parameter.</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample5.cs)]

<span data-ttu-id="aea36-175">Uruchom aplikację i przejdź do/Store/Details/5:</span><span class="sxs-lookup"><span data-stu-id="aea36-175">Run the application and browse to /Store/Details/5:</span></span>

![](mvc-music-store-part-2/_static/image7.png)

<span data-ttu-id="aea36-176">Podsumowaniemy do tej pory:</span><span class="sxs-lookup"><span data-stu-id="aea36-176">Let's recap what we've done so far:</span></span>

- <span data-ttu-id="aea36-177">Utworzyliśmy nowy projekt ASP.NET MVC w programie Visual Web Developer</span><span class="sxs-lookup"><span data-stu-id="aea36-177">We've created a new ASP.NET MVC project in Visual Web Developer</span></span>
- <span data-ttu-id="aea36-178">Omawiamy podstawową strukturę folderów aplikacji ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="aea36-178">We've discussed the basic folder structure of an ASP.NET MVC application</span></span>
- <span data-ttu-id="aea36-179">Dowiesz się, jak uruchomić naszą witrynę sieci Web przy użyciu serwera ASP.NET Development</span><span class="sxs-lookup"><span data-stu-id="aea36-179">We've learned how to run our website using the ASP.NET Development Server</span></span>
- <span data-ttu-id="aea36-180">Utworzyliśmy dwie klasy kontrolera: HomeController i StoreController</span><span class="sxs-lookup"><span data-stu-id="aea36-180">We've created two Controller classes: a HomeController and a StoreController</span></span>
- <span data-ttu-id="aea36-181">Dodaliśmy metody akcji do naszych kontrolerów, które odpowiadają na żądania adresów URL i zwracają tekst do przeglądarki</span><span class="sxs-lookup"><span data-stu-id="aea36-181">We've added Action Methods to our controllers which respond to URL requests and return text to the browser</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="aea36-182">[Poprzednie](mvc-music-store-part-1.md)
> [dalej](mvc-music-store-part-3.md)</span><span class="sxs-lookup"><span data-stu-id="aea36-182">[Previous](mvc-music-store-part-1.md)
[Next](mvc-music-store-part-3.md)</span></span>

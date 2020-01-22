---
uid: mvc/overview/getting-started/introduction/adding-a-controller
title: Dodawanie kontrolera | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: cc764f3b-6921-486a-8f44-c6ccd1249acd
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 80000b366203eff4b9524b7a5995832753b9eed3
ms.sourcegitcommit: 88fc80e3f65aebdf61ec9414810ddbc31c543f04
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/22/2020
ms.locfileid: "76519053"
---
# <a name="adding-a-controller"></a><span data-ttu-id="6d7bf-102">Dodawanie kontrolera</span><span class="sxs-lookup"><span data-stu-id="6d7bf-102">Adding a Controller</span></span>

<span data-ttu-id="6d7bf-103">Autor [Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="6d7bf-103">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

[!INCLUDE [Tutorial Note](index.md)]

<span data-ttu-id="6d7bf-104">MVC oznacza *Model-View-Controller*.</span><span class="sxs-lookup"><span data-stu-id="6d7bf-104">MVC stands for *model-view-controller*.</span></span> <span data-ttu-id="6d7bf-105">MVC to wzorzec służący do tworzenia aplikacji, które są dobrze zaprojektowane, weryfikowalne i łatwe w obsłudze.</span><span class="sxs-lookup"><span data-stu-id="6d7bf-105">MVC is a pattern for developing applications that are well architected, testable and easy to maintain.</span></span> <span data-ttu-id="6d7bf-106">Aplikacje oparte na MVC zawierają:</span><span class="sxs-lookup"><span data-stu-id="6d7bf-106">MVC-based applications contain:</span></span>

- <span data-ttu-id="6d7bf-107">**M** Odels: klasy reprezentujące dane aplikacji i używające logiki walidacji do wymuszania reguł dla tych danych.</span><span class="sxs-lookup"><span data-stu-id="6d7bf-107">**M** odels: Classes that represent the data of the application and that use validation logic to enforce business rules for that data.</span></span>
- <span data-ttu-id="6d7bf-108">**V** iews: pliki szablonów używane przez aplikację do dynamicznego generowania odpowiedzi html.</span><span class="sxs-lookup"><span data-stu-id="6d7bf-108">**V** iews: Template files that your application uses to dynamically generate HTML responses.</span></span>
- <span data-ttu-id="6d7bf-109">**C** Ontrollers: klasy obsługujące żądania przeglądarki przychodzącej, pobieranie danych modelu, a następnie określanie szablonów wyświetlania, które zwracają odpowiedź do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="6d7bf-109">**C** ontrollers: Classes that handle incoming browser requests, retrieve model data, and then specify view templates that return a response to the browser.</span></span>

<span data-ttu-id="6d7bf-110">Będziemy omawiać wszystkie te koncepcje w tej serii samouczków i pokazują, jak używać ich do kompilowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6d7bf-110">We'll be covering all these concepts in this tutorial series and show you how to use them to build an application.</span></span>

<span data-ttu-id="6d7bf-111">Zacznijmy od utworzenia klasy kontrolera.</span><span class="sxs-lookup"><span data-stu-id="6d7bf-111">Let's begin by creating a controller class.</span></span> <span data-ttu-id="6d7bf-112">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy folder *controllers* , a następnie kliknij polecenie **Dodaj**, a następnie pozycję **kontroler**.</span><span class="sxs-lookup"><span data-stu-id="6d7bf-112">In **Solution Explorer**, right-click the *Controllers* folder and then click **Add**, then **Controller**.</span></span>

![](adding-a-controller/_static/image1.png)

<span data-ttu-id="6d7bf-113">W oknie dialogowym **Dodawanie szkieletu** kliknij pozycję **kontroler MVC 5 — pusty**, a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="6d7bf-113">In the **Add Scaffold** dialog box, click **MVC 5 Controller - Empty**, and then click **Add**.</span></span>

![](adding-a-controller/_static/image2.png)  

<span data-ttu-id="6d7bf-114">Nazwij nowy kontroler "HelloWorldController" i kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="6d7bf-114">Name your new controller "HelloWorldController" and click **Add**.</span></span>

![Dodaj kontroler](adding-a-controller/_static/image3.png)

<span data-ttu-id="6d7bf-116">Zwróć uwagę, że w **Eksplorator rozwiązań** został utworzony nowy plik o nazwie *HelloWorldController.cs* i nowy folder *Views\HelloWorld*.</span><span class="sxs-lookup"><span data-stu-id="6d7bf-116">Notice in **Solution Explorer** that a new file has been created named *HelloWorldController.cs* and a new folder *Views\HelloWorld*.</span></span> <span data-ttu-id="6d7bf-117">Kontroler jest otwarty w IDE.</span><span class="sxs-lookup"><span data-stu-id="6d7bf-117">The controller is open in the IDE.</span></span>

![](adding-a-controller/_static/image4.png)

<span data-ttu-id="6d7bf-118">Zastąp zawartość pliku następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="6d7bf-118">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

<span data-ttu-id="6d7bf-119">Metody kontrolera zwrócą ciąg HTML jako przykład.</span><span class="sxs-lookup"><span data-stu-id="6d7bf-119">The controller methods will return a string of HTML as an example.</span></span> <span data-ttu-id="6d7bf-120">Kontroler ma nazwę `HelloWorldController` a pierwsza metoda ma nazwę `Index`.</span><span class="sxs-lookup"><span data-stu-id="6d7bf-120">The controller is named `HelloWorldController` and the first method is named `Index`.</span></span> <span data-ttu-id="6d7bf-121">Wywołajmy ją z przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="6d7bf-121">Let's invoke it from a browser.</span></span> <span data-ttu-id="6d7bf-122">Uruchom aplikację (naciśnij klawisz F5 lub CTRL + F5).</span><span class="sxs-lookup"><span data-stu-id="6d7bf-122">Run the application (press F5 or Ctrl+F5).</span></span> <span data-ttu-id="6d7bf-123">W przeglądarce Dołącz &quot;HelloWorld&quot; do ścieżki na pasku adresu.</span><span class="sxs-lookup"><span data-stu-id="6d7bf-123">In the browser, append &quot;HelloWorld&quot; to the path in the address bar.</span></span> <span data-ttu-id="6d7bf-124">(Na przykład na poniższej ilustracji jest `http://localhost:1234/HelloWorld.`) Strona w przeglądarce będzie wyglądać podobnie do poniższego zrzutu ekranu.</span><span class="sxs-lookup"><span data-stu-id="6d7bf-124">(For example, in the illustration below, it's `http://localhost:1234/HelloWorld.`) The page in the browser will look like the following screenshot.</span></span> <span data-ttu-id="6d7bf-125">W powyższej metodzie kod zwrócił ciąg bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="6d7bf-125">In the method above, the code returned a string directly.</span></span> <span data-ttu-id="6d7bf-126">Otrzymasz informację o tym, że system po prostu zwróci kod HTML.</span><span class="sxs-lookup"><span data-stu-id="6d7bf-126">You told the system to just return some HTML, and it did!</span></span>

![](adding-a-controller/_static/image5.png)

<span data-ttu-id="6d7bf-127">ASP.NET MVC wywołuje różne klasy kontrolera (i różne metody akcji w nich) w zależności od przychodzącego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="6d7bf-127">ASP.NET MVC invokes different controller classes (and different action methods within them) depending on the incoming URL.</span></span> <span data-ttu-id="6d7bf-128">Domyślna logika routingu adresów URL używana przez ASP.NET MVC używa formatu, takiego jak to, aby określić kod, który ma zostać wywołany:</span><span class="sxs-lookup"><span data-stu-id="6d7bf-128">The default URL routing logic used by ASP.NET MVC uses a format like this to determine what code to invoke:</span></span>

`/[Controller]/[ActionName]/[Parameters]`

<span data-ttu-id="6d7bf-129">Format routingu w aplikacji jest ustawiany *\_Start/RouteConfig. cs* .</span><span class="sxs-lookup"><span data-stu-id="6d7bf-129">You set the format for routing in the *App\_Start/RouteConfig.cs* file.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample2.cs?highlight=7-8)]

<span data-ttu-id="6d7bf-130">Gdy uruchamiasz aplikację i nie podasz żadnych segmentów adresów URL, domyślnym kontrolerem "Home" i akcją "index" określono w sekcji wartości domyślne w powyższym kodzie.</span><span class="sxs-lookup"><span data-stu-id="6d7bf-130">When you run the application and don't supply any URL segments, it defaults to the "Home" controller and the "Index" action method specified in the defaults section of the code above.</span></span>

<span data-ttu-id="6d7bf-131">Pierwsza część adresu URL określa klasę kontrolera do wykonania.</span><span class="sxs-lookup"><span data-stu-id="6d7bf-131">The first part of the URL determines the controller class to execute.</span></span> <span data-ttu-id="6d7bf-132">Tak więc */HelloWorld* mapuje do klasy `HelloWorldController`.</span><span class="sxs-lookup"><span data-stu-id="6d7bf-132">So */HelloWorld* maps to the `HelloWorldController` class.</span></span> <span data-ttu-id="6d7bf-133">Druga część adresu URL określa metodę akcji dla klasy, która ma zostać wykonana.</span><span class="sxs-lookup"><span data-stu-id="6d7bf-133">The second part of the URL determines the action method on the class to execute.</span></span> <span data-ttu-id="6d7bf-134">Dlatego */HelloWorld/index* może spowodować wykonanie metody `Index` klasy `HelloWorldController`.</span><span class="sxs-lookup"><span data-stu-id="6d7bf-134">So */HelloWorld/Index* would cause the `Index` method of the `HelloWorldController` class to execute.</span></span> <span data-ttu-id="6d7bf-135">Zwróć uwagę, że tylko chcemy przeglądać do */HelloWorld* , a metoda `Index` była używana domyślnie.</span><span class="sxs-lookup"><span data-stu-id="6d7bf-135">Notice that we only had to browse to */HelloWorld* and the `Index` method was used by default.</span></span> <span data-ttu-id="6d7bf-136">Wynika to z faktu, że metoda o nazwie `Index` jest metodą domyślną, która zostanie wywołana na kontrolerze, jeśli nie została określona jawnie.</span><span class="sxs-lookup"><span data-stu-id="6d7bf-136">This is because a method named `Index` is the default method that will be called on a controller if one is not explicitly specified.</span></span> <span data-ttu-id="6d7bf-137">Trzecia część segmentu adresu URL (`Parameters`) jest dla danych trasy.</span><span class="sxs-lookup"><span data-stu-id="6d7bf-137">The third part of the URL segment ( `Parameters`) is for route data.</span></span> <span data-ttu-id="6d7bf-138">Będziemy widzieć dane trasy w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="6d7bf-138">We'll see route data later on in this tutorial.</span></span>

<span data-ttu-id="6d7bf-139">Przejdź do `http://localhost:xxxx/HelloWorld/Welcome`.</span><span class="sxs-lookup"><span data-stu-id="6d7bf-139">Browse to `http://localhost:xxxx/HelloWorld/Welcome`.</span></span> <span data-ttu-id="6d7bf-140">Metoda `Welcome` jest uruchamiana i zwraca ciąg &quot;jest to metoda akcji powitalnej...&quot;.</span><span class="sxs-lookup"><span data-stu-id="6d7bf-140">The `Welcome` method runs and returns the string &quot;This is the Welcome action method...&quot;.</span></span> <span data-ttu-id="6d7bf-141">Domyślne mapowanie MVC jest `/[Controller]/[ActionName]/[Parameters]`.</span><span class="sxs-lookup"><span data-stu-id="6d7bf-141">The default MVC mapping is `/[Controller]/[ActionName]/[Parameters]`.</span></span> <span data-ttu-id="6d7bf-142">Dla tego adresu URL kontroler jest `HelloWorld` i `Welcome` jest metodą akcji.</span><span class="sxs-lookup"><span data-stu-id="6d7bf-142">For this URL, the controller is `HelloWorld` and `Welcome` is the action method.</span></span> <span data-ttu-id="6d7bf-143">Nie użyto jeszcze `[Parameters]` części adresu URL.</span><span class="sxs-lookup"><span data-stu-id="6d7bf-143">You haven't used the `[Parameters]` part of the URL yet.</span></span>

![](adding-a-controller/_static/image6.png)

<span data-ttu-id="6d7bf-144">Zmodyfikujmy przykład nieco tak, aby można było przekazać niektóre informacje o parametrach z adresu URL do kontrolera (na przykład */HelloWorld/Welcome? Name = Scott&amp;numtimes = 4*).</span><span class="sxs-lookup"><span data-stu-id="6d7bf-144">Let's modify the example slightly so that you can pass some parameter information from the URL to the controller (for example, */HelloWorld/Welcome?name=Scott&amp;numtimes=4*).</span></span> <span data-ttu-id="6d7bf-145">Zmień metodę `Welcome` tak, aby obejmowała dwa parametry, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="6d7bf-145">Change your `Welcome` method to include two parameters as shown below.</span></span> <span data-ttu-id="6d7bf-146">Należy zauważyć, że kod używa C# funkcji opcjonalnej parametru, aby wskazać, że parametr `numTimes` powinien domyślnie mieć wartość 1, jeśli dla tego parametru nie zostanie przeniesiona żadnej wartości.</span><span class="sxs-lookup"><span data-stu-id="6d7bf-146">Note that the code uses the C# optional-parameter feature to indicate that the `numTimes` parameter should default to 1 if no value is passed for that parameter.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample3.cs)]

> [!NOTE]
> <span data-ttu-id="6d7bf-147">Uwaga dotycząca zabezpieczeń: powyższy kod używa [HttpUtility. HtmlEncode](https://msdn.microsoft.com/library/ee360286(v=vs.110).aspx) do ochrony aplikacji przed złośliwymi danymi wejściowymi (tj. JavaScript).</span><span class="sxs-lookup"><span data-stu-id="6d7bf-147">Security Note: The code above uses [HttpUtility.HtmlEncode](https://msdn.microsoft.com/library/ee360286(v=vs.110).aspx) to protect the application from malicious input (namely JavaScript).</span></span> <span data-ttu-id="6d7bf-148">Aby uzyskać więcej informacji [, zobacz jak: Ochrona przed programami wykorzystującymi luki w zabezpieczeniach w aplikacji sieci Web przez zastosowanie kodowania HTML do ciągów znaków](https://msdn.microsoft.com/library/a2a4yykt(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="6d7bf-148">For more information see [How to: Protect Against Script Exploits in a Web Application by Applying HTML Encoding to Strings](https://msdn.microsoft.com/library/a2a4yykt(v=vs.100).aspx).</span></span>

 <span data-ttu-id="6d7bf-149">Uruchom aplikację i przejdź do przykładowego adresu URL (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4`).</span><span class="sxs-lookup"><span data-stu-id="6d7bf-149">Run your application and browse to the example URL (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4`).</span></span> <span data-ttu-id="6d7bf-150">Możesz wypróbować różne wartości `name` i `numtimes` w adresie URL.</span><span class="sxs-lookup"><span data-stu-id="6d7bf-150">You can try different values for `name` and `numtimes` in the URL.</span></span> <span data-ttu-id="6d7bf-151">[System powiązań modelu MVC ASP.NET](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) automatycznie mapuje nazwane parametry z ciągu zapytania na pasku adresu na parametry w metodzie.</span><span class="sxs-lookup"><span data-stu-id="6d7bf-151">The [ASP.NET MVC model binding system](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) automatically maps the named parameters from the query string in the address bar to parameters in your method.</span></span>

![](adding-a-controller/_static/image7.png)

<span data-ttu-id="6d7bf-152">W powyższym przykładzie segment adresu URL (`Parameters`) nie jest używany, parametry `name` i `numTimes` są przesyłane jako [ciągi zapytań](http://en.wikipedia.org/wiki/Query_string).</span><span class="sxs-lookup"><span data-stu-id="6d7bf-152">In the sample above, the URL segment ( `Parameters`) is not used, the `name` and `numTimes` parameters are passed as [query strings](http://en.wikipedia.org/wiki/Query_string).</span></span> <span data-ttu-id="6d7bf-153">Znak ?</span><span class="sxs-lookup"><span data-stu-id="6d7bf-153">The ?</span></span> <span data-ttu-id="6d7bf-154">(znak zapytania) w powyższym adresie URL jest separatorem, a ciągi zapytania są zgodne.</span><span class="sxs-lookup"><span data-stu-id="6d7bf-154">(question mark) in the above URL is a separator, and the query strings follow.</span></span> <span data-ttu-id="6d7bf-155">Znak &amp; oddziela ciągi zapytań.</span><span class="sxs-lookup"><span data-stu-id="6d7bf-155">The &amp; character separates query strings.</span></span>

<span data-ttu-id="6d7bf-156">Zastąp metodę powitalną następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="6d7bf-156">Replace the Welcome method with the following code:</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample4.cs)]

<span data-ttu-id="6d7bf-157">Uruchom aplikację i wprowadź następujący adres URL: `http://localhost:xxx/HelloWorld/Welcome/1?name=Scott`</span><span class="sxs-lookup"><span data-stu-id="6d7bf-157">Run the application and enter the following URL: `http://localhost:xxx/HelloWorld/Welcome/1?name=Scott`</span></span>

![](adding-a-controller/_static/image8.png)

<span data-ttu-id="6d7bf-158">Tym razem trzeci segment adresu URL pasuje do parametru Route `ID.` Metoda akcji `Welcome` zawiera parametr (`ID`), który pasował do specyfikacji adresu URL w metodzie `RegisterRoutes`.</span><span class="sxs-lookup"><span data-stu-id="6d7bf-158">This time the third URL segment matched the route parameter `ID.` The `Welcome` action method contains a parameter (`ID`) that matched the URL specification in the `RegisterRoutes` method.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample5.cs?highlight=7)]

<span data-ttu-id="6d7bf-159">W aplikacjach ASP.NET MVC bardziej typowe jest przekazywanie parametrów jako danych tras (podobnie jak w przypadku powyższego identyfikatora) niż przekazywanie ich jako ciągów zapytań.</span><span class="sxs-lookup"><span data-stu-id="6d7bf-159">In ASP.NET MVC applications, it's more typical to pass in parameters as route data (like we did with ID above) than passing them as query strings.</span></span> <span data-ttu-id="6d7bf-160">Można również dodać trasę, aby przekazywać `name` i `numtimes` w parametrach jako dane trasy w adresie URL.</span><span class="sxs-lookup"><span data-stu-id="6d7bf-160">You could also add a route to pass both the `name` and `numtimes` in parameters as route data in the URL.</span></span> <span data-ttu-id="6d7bf-161">W pliku *\_aplikacji Start\RouteConfig.cs* Dodaj trasę "Hello":</span><span class="sxs-lookup"><span data-stu-id="6d7bf-161">In the *App\_Start\RouteConfig.cs* file, add the "Hello" route:</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample6.cs?highlight=13-16)]

<span data-ttu-id="6d7bf-162">Uruchom aplikację i przejdź do `/localhost:XXX/HelloWorld/Welcome/Scott/3`.</span><span class="sxs-lookup"><span data-stu-id="6d7bf-162">Run the application and browse to `/localhost:XXX/HelloWorld/Welcome/Scott/3`.</span></span>

![](adding-a-controller/_static/image9.png)

<span data-ttu-id="6d7bf-163">W przypadku wielu aplikacji MVC domyślna trasa działa prawidłowo.</span><span class="sxs-lookup"><span data-stu-id="6d7bf-163">For many MVC applications, the default route works fine.</span></span> <span data-ttu-id="6d7bf-164">Więcej informacji można znaleźć w dalszej części tego samouczka, aby przekazać dane przy użyciu spinacza modelu i nie trzeba będzie modyfikować trasy domyślnej dla tego programu.</span><span class="sxs-lookup"><span data-stu-id="6d7bf-164">You'll learn later in this tutorial to pass data using the model binder, and you won't have to modify the default route for that.</span></span>

<span data-ttu-id="6d7bf-165">W tych przykładach kontroler wykonywał &quot;VC&quot; część MVC — to znaczy, że widok i działanie kontrolera.</span><span class="sxs-lookup"><span data-stu-id="6d7bf-165">In these examples the controller has been doing the &quot;VC&quot; portion of MVC — that is, the view and controller work.</span></span> <span data-ttu-id="6d7bf-166">Kontroler zwraca bezpośrednio kod HTML.</span><span class="sxs-lookup"><span data-stu-id="6d7bf-166">The controller is returning HTML directly.</span></span> <span data-ttu-id="6d7bf-167">Zwykle nie chcesz, aby kontrolery zwracające kod HTML bezpośrednio, ponieważ staną się bardzo uciążliwe w kodzie.</span><span class="sxs-lookup"><span data-stu-id="6d7bf-167">Ordinarily you don't want controllers returning HTML directly, since that becomes very cumbersome to code.</span></span> <span data-ttu-id="6d7bf-168">Zamiast tego zwykle użyjesz osobnego pliku szablonu widoku, aby ułatwić generowanie odpowiedzi HTML.</span><span class="sxs-lookup"><span data-stu-id="6d7bf-168">Instead we'll typically use a separate view template file to help generate the HTML response.</span></span> <span data-ttu-id="6d7bf-169">Przyjrzyjmy się dalej, jak to zrobić.</span><span class="sxs-lookup"><span data-stu-id="6d7bf-169">Let's look next at how we can do this.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="6d7bf-170">[Poprzedni](getting-started.md)
> [Następny](adding-a-view.md)</span><span class="sxs-lookup"><span data-stu-id="6d7bf-170">[Previous](getting-started.md)
[Next](adding-a-view.md)</span></span>

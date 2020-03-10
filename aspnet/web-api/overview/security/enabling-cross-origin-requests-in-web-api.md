---
uid: web-api/overview/security/enabling-cross-origin-requests-in-web-api
title: Włączanie żądań między źródłami w programie ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: Pokazuje, jak obsługiwać udostępnianie zasobów między źródłami (CORS) w interfejsie Web API ASP.NET.
ms.author: riande
ms.date: 01/29/2019
ms.assetid: 9b265a5a-6a70-4a82-adce-2d7c56ae8bdd
msc.legacyurl: /web-api/overview/security/enabling-cross-origin-requests-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 9d3016d98fa6c3a55359c6dab0737407b29925f1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78555707"
---
# <a name="enable-cross-origin-requests-in-aspnet-web-api-2"></a><span data-ttu-id="bbe35-103">Włączanie żądań między źródłami w programie ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="bbe35-103">Enable cross-origin requests in ASP.NET Web API 2</span></span>

<span data-ttu-id="bbe35-104">według [Jan Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="bbe35-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="bbe35-105">Zabezpieczenia przeglądarki uniemożliwiają stronie internetowej wysyłanie żądań AJAX do innej domeny.</span><span class="sxs-lookup"><span data-stu-id="bbe35-105">Browser security prevents a web page from making AJAX requests to another domain.</span></span> <span data-ttu-id="bbe35-106">To ograniczenie jest nazywane *zasadami tego samego źródła*i uniemożliwia złośliwym lokacjom odczytywanie poufnych danych z innej lokacji.</span><span class="sxs-lookup"><span data-stu-id="bbe35-106">This restriction is called the *same-origin policy*, and prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="bbe35-107">Czasami jednak może być konieczne, aby inne Lokacje wywoływały internetowy interfejs API.</span><span class="sxs-lookup"><span data-stu-id="bbe35-107">However, sometimes you might want to let other sites call your web API.</span></span>
>
> <span data-ttu-id="bbe35-108">[Udostępnianie zasobów między źródłami](http://www.w3.org/TR/cors/) (CORS) jest standardem W3C, który umożliwia serwerowi złagodzenie zasad tego samego źródła.</span><span class="sxs-lookup"><span data-stu-id="bbe35-108">[Cross Origin Resource Sharing](http://www.w3.org/TR/cors/) (CORS) is a W3C standard that allows a server to relax the same-origin policy.</span></span> <span data-ttu-id="bbe35-109">Przy użyciu mechanizmu CORS serwer może jawnie zezwolić na niektóre żądania między źródłami podczas odrzucania innych.</span><span class="sxs-lookup"><span data-stu-id="bbe35-109">Using CORS, a server can explicitly allow some cross-origin requests while rejecting others.</span></span> <span data-ttu-id="bbe35-110">Mechanizm CORS jest bezpieczniejszy i bardziej elastyczny niż wcześniejsze techniki, takie jak [JSONP](http://en.wikipedia.org/wiki/JSONP).</span><span class="sxs-lookup"><span data-stu-id="bbe35-110">CORS is safer and more flexible than earlier techniques such as [JSONP](http://en.wikipedia.org/wiki/JSONP).</span></span> <span data-ttu-id="bbe35-111">W tym samouczku pokazano, jak włączyć mechanizm CORS w aplikacji interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="bbe35-111">This tutorial shows how to enable CORS in your Web API application.</span></span>
>
> ## <a name="software-used-in-the-tutorial"></a><span data-ttu-id="bbe35-112">Oprogramowanie używane w samouczku</span><span class="sxs-lookup"><span data-stu-id="bbe35-112">Software used in the tutorial</span></span>
>
> - [<span data-ttu-id="bbe35-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bbe35-113">Visual Studio</span></span>](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - <span data-ttu-id="bbe35-114">Internetowy interfejs API 2,2</span><span class="sxs-lookup"><span data-stu-id="bbe35-114">Web API 2.2</span></span>

## <a name="introduction"></a><span data-ttu-id="bbe35-115">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="bbe35-115">Introduction</span></span>

<span data-ttu-id="bbe35-116">W tym samouczku przedstawiono obsługę mechanizmu CORS w interfejsie API sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="bbe35-116">This tutorial demonstrates CORS support in ASP.NET Web API.</span></span> <span data-ttu-id="bbe35-117">Zaczniemy od utworzenia dwóch projektów ASP.NET — jeden o nazwie "WebService", który hostuje kontroler interfejsu API sieci Web, a drugi o nazwie "WebClient", który wywołuje usługę WebService.</span><span class="sxs-lookup"><span data-stu-id="bbe35-117">We'll start by creating two ASP.NET projects – one called "WebService", which hosts a Web API controller, and the other called "WebClient", which calls WebService.</span></span> <span data-ttu-id="bbe35-118">Ponieważ dwie aplikacje są hostowane w różnych domenach, żądanie AJAX od klienta WebClient do usługi WebService jest żądaniem między źródłami.</span><span class="sxs-lookup"><span data-stu-id="bbe35-118">Because the two applications are hosted at different domains, an AJAX request from WebClient to WebService is a cross-origin request.</span></span>

![](enabling-cross-origin-requests-in-web-api/_static/image1.png)

### <a name="what-is-same-origin"></a><span data-ttu-id="bbe35-119">Co to jest "to samo Źródło"?</span><span class="sxs-lookup"><span data-stu-id="bbe35-119">What is "same origin"?</span></span>

<span data-ttu-id="bbe35-120">Dwa adresy URL mają te same źródła, jeśli mają identyczne schematy, hosty i porty.</span><span class="sxs-lookup"><span data-stu-id="bbe35-120">Two URLs have the same origin if they have identical schemes, hosts, and ports.</span></span> <span data-ttu-id="bbe35-121">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span><span class="sxs-lookup"><span data-stu-id="bbe35-121">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span></span>

<span data-ttu-id="bbe35-122">Te dwa adresy URL mają te same źródła:</span><span class="sxs-lookup"><span data-stu-id="bbe35-122">These two URLs have the same origin:</span></span>

- `http://example.com/foo.html`
- `http://example.com/bar.html`

<span data-ttu-id="bbe35-123">Te adresy URL mają różne źródła niż poprzednie dwa:</span><span class="sxs-lookup"><span data-stu-id="bbe35-123">These URLs have different origins than the previous two:</span></span>

- <span data-ttu-id="bbe35-124">`http://example.net` — inna domena</span><span class="sxs-lookup"><span data-stu-id="bbe35-124">`http://example.net` - Different domain</span></span>
- <span data-ttu-id="bbe35-125">`http://example.com:9000/foo.html` — inny port</span><span class="sxs-lookup"><span data-stu-id="bbe35-125">`http://example.com:9000/foo.html` - Different port</span></span>
- <span data-ttu-id="bbe35-126">`https://example.com/foo.html` — inny schemat</span><span class="sxs-lookup"><span data-stu-id="bbe35-126">`https://example.com/foo.html` - Different scheme</span></span>
- <span data-ttu-id="bbe35-127">`http://www.example.com/foo.html` — inna poddomena</span><span class="sxs-lookup"><span data-stu-id="bbe35-127">`http://www.example.com/foo.html` - Different subdomain</span></span>

> [!NOTE]
> <span data-ttu-id="bbe35-128">Program Internet Explorer nie traktuje portu podczas porównywania źródeł.</span><span class="sxs-lookup"><span data-stu-id="bbe35-128">Internet Explorer does not consider the port when comparing origins.</span></span>

## <a name="create-the-webservice-project"></a><span data-ttu-id="bbe35-129">Tworzenie projektu usługi WebService</span><span class="sxs-lookup"><span data-stu-id="bbe35-129">Create the WebService project</span></span>

> [!NOTE]
> <span data-ttu-id="bbe35-130">W tej sekcji założono, że wiesz już, jak tworzyć projekty interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="bbe35-130">This section assumes you already know how to create Web API projects.</span></span> <span data-ttu-id="bbe35-131">Jeśli nie, zobacz [wprowadzenie z interfejsem API sieci Web ASP.NET](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="bbe35-131">If not, see [Getting Started with ASP.NET Web API](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span></span>

1. <span data-ttu-id="bbe35-132">Uruchom program Visual Studio i Utwórz nowy projekt **aplikacji sieci Web ASP.NET (.NET Framework)** .</span><span class="sxs-lookup"><span data-stu-id="bbe35-132">Start Visual Studio and create a new **ASP.NET Web Application (.NET Framework)** project.</span></span>
2. <span data-ttu-id="bbe35-133">W oknie dialogowym **Nowa aplikacja sieci Web ASP.NET** wybierz szablon **pusty** projekt.</span><span class="sxs-lookup"><span data-stu-id="bbe35-133">In the **New ASP.NET Web Application** dialog box, select the **Empty** project template.</span></span> <span data-ttu-id="bbe35-134">W obszarze **Dodaj foldery i podstawowe odwołania dla programu**zaznacz pole wyboru **internetowy interfejs API** .</span><span class="sxs-lookup"><span data-stu-id="bbe35-134">Under **Add folders and core references for**, select the **Web API** checkbox.</span></span>

   ![Nowe okno dialogowe projektu ASP.NET w programie Visual Studio](enabling-cross-origin-requests-in-web-api/_static/new-web-app-dialog.png)

3. <span data-ttu-id="bbe35-136">Dodaj kontroler interfejsu API sieci Web o nazwie `TestController` przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="bbe35-136">Add a Web API controller named `TestController` with the following code:</span></span>

   [!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample1.cs)]

4. <span data-ttu-id="bbe35-137">Aplikację można uruchomić lokalnie lub wdrożyć na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="bbe35-137">You can run the application locally or deploy to Azure.</span></span> <span data-ttu-id="bbe35-138">(W przypadku zrzutów ekranu w tym samouczku aplikacja zostanie wdrożona w Azure App Service Web Apps). Aby sprawdzić, czy interfejs API sieci Web działa, przejdź do `http://hostname/api/test/`, gdzie *hostname* jest domeną, w której została wdrożona aplikacja.</span><span class="sxs-lookup"><span data-stu-id="bbe35-138">(For the screenshots in this tutorial, the app deploys to Azure App Service Web Apps.) To verify that the web API is working, navigate to `http://hostname/api/test/`, where *hostname* is the domain where you deployed the application.</span></span> <span data-ttu-id="bbe35-139">Powinien pojawić się tekst odpowiedzi, &quot;GET: Komunikat testowy&quot;.</span><span class="sxs-lookup"><span data-stu-id="bbe35-139">You should see the response text, &quot;GET: Test Message&quot;.</span></span>

   ![Przeglądarka sieci Web wyświetlająca komunikat testowy](enabling-cross-origin-requests-in-web-api/_static/image4.png)

## <a name="create-the-webclient-project"></a><span data-ttu-id="bbe35-141">Tworzenie projektu WebClient</span><span class="sxs-lookup"><span data-stu-id="bbe35-141">Create the WebClient project</span></span>

1. <span data-ttu-id="bbe35-142">Utwórz inny projekt **aplikacji sieci Web ASP.NET (.NET Framework)** i wybierz szablon projektu **MVC** .</span><span class="sxs-lookup"><span data-stu-id="bbe35-142">Create another **ASP.NET Web Application (.NET Framework)** project and select the **MVC** project template.</span></span> <span data-ttu-id="bbe35-143">Opcjonalnie możesz wybrać pozycję **Zmień uwierzytelnianie** > **bez uwierzytelniania**.</span><span class="sxs-lookup"><span data-stu-id="bbe35-143">Optionally, select **Change Authentication** > **No Authentication**.</span></span> <span data-ttu-id="bbe35-144">Nie musisz uwierzytelniać się w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="bbe35-144">You don't need authentication for this tutorial.</span></span>

   ![Szablon MVC w nowym oknie dialogowym projektu ASP.NET w programie Visual Studio](enabling-cross-origin-requests-in-web-api/_static/new-web-app-dialog-mvc.png)

2. <span data-ttu-id="bbe35-146">W **Eksplorator rozwiązań**Otwórz plik *widoki/Home/index. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="bbe35-146">In **Solution Explorer**, open the file *Views/Home/Index.cshtml*.</span></span> <span data-ttu-id="bbe35-147">Zastąp kod w tym pliku zgodnie z poniższym przykładem:</span><span class="sxs-lookup"><span data-stu-id="bbe35-147">Replace the code in this file with the following:</span></span>

   [!code-cshtml[Main](enabling-cross-origin-requests-in-web-api/samples/sample2.cshtml?highlight=13)]

   <span data-ttu-id="bbe35-148">Dla zmiennej *serviceUrl* Użyj identyfikatora URI aplikacji usługi WebService.</span><span class="sxs-lookup"><span data-stu-id="bbe35-148">For the *serviceUrl* variable, use the URI of the WebService app.</span></span>

3. <span data-ttu-id="bbe35-149">Uruchom aplikację WebClient lokalnie lub opublikuj ją w innej witrynie sieci Web.</span><span class="sxs-lookup"><span data-stu-id="bbe35-149">Run the WebClient app locally or publish it to another website.</span></span>

<span data-ttu-id="bbe35-150">Po kliknięciu przycisku "Wypróbuj" zostanie przesłane żądanie AJAX do aplikacji usługi WebService przy użyciu metody HTTP wymienionej w polu listy rozwijanej (GET, POST lub PUT).</span><span class="sxs-lookup"><span data-stu-id="bbe35-150">When you click the "Try It" button, an AJAX request is submitted to the WebService app using the HTTP method listed in the dropdown box (GET, POST, or PUT).</span></span> <span data-ttu-id="bbe35-151">Pozwala to na badanie różnych żądań między źródłami.</span><span class="sxs-lookup"><span data-stu-id="bbe35-151">This lets you examine different cross-origin requests.</span></span> <span data-ttu-id="bbe35-152">Obecnie aplikacja usługi WebService nie obsługuje mechanizmu CORS, więc po kliknięciu przycisku zostanie wyświetlony komunikat o błędzie.</span><span class="sxs-lookup"><span data-stu-id="bbe35-152">Currently, the WebService app does not support CORS, so if you click the button you'll get an error.</span></span>

![Błąd "try" w przeglądarce](enabling-cross-origin-requests-in-web-api/_static/image7.png)

> [!NOTE]
> <span data-ttu-id="bbe35-154">Jeśli monitorujesz ruch HTTP w narzędziu takim jak [programu Fiddler](https://www.telerik.com/fiddler), zobaczysz, że przeglądarka wysyła żądanie Get, a żądanie jest pomyślne, ale wywołanie AJAX zwraca błąd.</span><span class="sxs-lookup"><span data-stu-id="bbe35-154">If you watch the HTTP traffic in a tool like [Fiddler](https://www.telerik.com/fiddler), you'll see that the browser does send the GET request, and the request succeeds, but the AJAX call returns an error.</span></span> <span data-ttu-id="bbe35-155">Ważne jest, aby zrozumieć, że zasady tego samego źródła nie uniemożliwiają *wysłania* żądania przez przeglądarkę.</span><span class="sxs-lookup"><span data-stu-id="bbe35-155">It's important to understand that same-origin policy does not prevent the browser from *sending* the request.</span></span> <span data-ttu-id="bbe35-156">Zamiast tego uniemożliwia aplikacji wyświetlanie *odpowiedzi*.</span><span class="sxs-lookup"><span data-stu-id="bbe35-156">Instead, it prevents the application from seeing the *response*.</span></span>

![Debuger sieci Web programu Fiddler pokazujący żądania sieci Web](enabling-cross-origin-requests-in-web-api/_static/image8.png)

## <a name="enable-cors"></a><span data-ttu-id="bbe35-158">Włączanie mechanizmu CORS</span><span class="sxs-lookup"><span data-stu-id="bbe35-158">Enable CORS</span></span>

<span data-ttu-id="bbe35-159">Teraz włączmy mechanizm CORS w aplikacji usługi WebService.</span><span class="sxs-lookup"><span data-stu-id="bbe35-159">Now let's enable CORS in the WebService app.</span></span> <span data-ttu-id="bbe35-160">Najpierw Dodaj pakiet NuGet CORS.</span><span class="sxs-lookup"><span data-stu-id="bbe35-160">First, add the CORS NuGet package.</span></span> <span data-ttu-id="bbe35-161">W programie Visual Studio w menu **Narzędzia** wybierz pozycję **Menedżer pakietów NuGet**, a następnie wybierz pozycję **konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="bbe35-161">In Visual Studio, from the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="bbe35-162">W oknie Konsola Menedżera pakietów wpisz następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="bbe35-162">In the Package Manager Console window, type the following command:</span></span>

[!code-powershell[Main](enabling-cross-origin-requests-in-web-api/samples/sample3.ps1)]

<span data-ttu-id="bbe35-163">To polecenie instaluje najnowszy pakiet i aktualizuje wszystkie zależności, w tym biblioteki podstawowych interfejsów API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="bbe35-163">This command installs the latest package and updates all dependencies, including the core Web API libraries.</span></span> <span data-ttu-id="bbe35-164">Użyj flagi `-Version`, aby określić wersję docelową.</span><span class="sxs-lookup"><span data-stu-id="bbe35-164">Use the `-Version` flag to target a specific version.</span></span> <span data-ttu-id="bbe35-165">Pakiet CORS wymaga interfejsu Web API 2,0 lub nowszego.</span><span class="sxs-lookup"><span data-stu-id="bbe35-165">The CORS package requires Web API 2.0 or later.</span></span>

<span data-ttu-id="bbe35-166">Otwórz aplikację pliku *\_Start/WebApiConfig. cs*.</span><span class="sxs-lookup"><span data-stu-id="bbe35-166">Open the file *App\_Start/WebApiConfig.cs*.</span></span> <span data-ttu-id="bbe35-167">Dodaj następujący kod do metody **WebApiConfig. Register** :</span><span class="sxs-lookup"><span data-stu-id="bbe35-167">Add the following code to the **WebApiConfig.Register** method:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample4.cs?highlight=9)]

<span data-ttu-id="bbe35-168">Następnie Dodaj atrybut **[EnableCors]** do klasy `TestController`:</span><span class="sxs-lookup"><span data-stu-id="bbe35-168">Next, add the **[EnableCors]** attribute to the `TestController` class:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample5.cs?highlight=3,7)]

<span data-ttu-id="bbe35-169">W przypadku parametru *Origins* Użyj identyfikatora URI, w którym wdrożono aplikację WebClient.</span><span class="sxs-lookup"><span data-stu-id="bbe35-169">For the *origins* parameter, use the URI where you deployed the WebClient application.</span></span> <span data-ttu-id="bbe35-170">Pozwala to na żądania między źródłami z klienta WebClient, mimo że nie zezwala na wszystkie inne żądania między domenami.</span><span class="sxs-lookup"><span data-stu-id="bbe35-170">This allows cross-origin requests from WebClient, while still disallowing all other cross-domain requests.</span></span> <span data-ttu-id="bbe35-171">Później opisano parametry dla **[EnableCors]** w bardziej szczegółowy sposób.</span><span class="sxs-lookup"><span data-stu-id="bbe35-171">Later, I'll describe the parameters for **[EnableCors]** in more detail.</span></span>

<span data-ttu-id="bbe35-172">Nie uwzględniaj ukośnika na końcu adresu URL *początkowych* elementów.</span><span class="sxs-lookup"><span data-stu-id="bbe35-172">Do not include a forward slash at the end of the *origins* URL.</span></span>

<span data-ttu-id="bbe35-173">Wdróż ponownie zaktualizowaną aplikację sieci Web.</span><span class="sxs-lookup"><span data-stu-id="bbe35-173">Redeploy the updated WebService application.</span></span> <span data-ttu-id="bbe35-174">Nie musisz aktualizować klienta WebClient.</span><span class="sxs-lookup"><span data-stu-id="bbe35-174">You don't need to update WebClient.</span></span> <span data-ttu-id="bbe35-175">Teraz żądanie AJAX od klienta WebClient powinno zakończyć się pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="bbe35-175">Now the AJAX request from WebClient should succeed.</span></span> <span data-ttu-id="bbe35-176">Metody GET, PUT i POST są dozwolone.</span><span class="sxs-lookup"><span data-stu-id="bbe35-176">The GET, PUT, and POST methods are all allowed.</span></span>

![Przeglądarka sieci Web przedstawiająca pomyślne wiadomości testowe](enabling-cross-origin-requests-in-web-api/_static/image9.png)

## <a name="how-cors-works"></a><span data-ttu-id="bbe35-178">Jak działa mechanizm CORS</span><span class="sxs-lookup"><span data-stu-id="bbe35-178">How CORS Works</span></span>

<span data-ttu-id="bbe35-179">W tej sekcji opisano, co się dzieje w żądaniu CORS, na poziomie komunikatów HTTP.</span><span class="sxs-lookup"><span data-stu-id="bbe35-179">This section describes what happens in a CORS request, at the level of the HTTP messages.</span></span> <span data-ttu-id="bbe35-180">Ważne jest, aby zrozumieć, jak działa mechanizm CORS, dzięki czemu można prawidłowo skonfigurować atrybut **[EnableCors]** i rozwiązywać problemy, jeśli elementy nie działają zgodnie z oczekiwaniami.</span><span class="sxs-lookup"><span data-stu-id="bbe35-180">It's important to understand how CORS works, so that you can configure the **[EnableCors]** attribute correctly and troubleshoot if things don't work as you expect.</span></span>

<span data-ttu-id="bbe35-181">Specyfikacja CORS zawiera kilka nowych nagłówków HTTP, które umożliwiają żądania między źródłami.</span><span class="sxs-lookup"><span data-stu-id="bbe35-181">The CORS specification introduces several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="bbe35-182">Jeśli przeglądarka obsługuje mechanizm CORS, ustawia te nagłówki automatycznie dla żądań cross-Origin; nie musisz wykonywać żadnych dodatkowych czynności w kodzie JavaScript.</span><span class="sxs-lookup"><span data-stu-id="bbe35-182">If a browser supports CORS, it sets these headers automatically for cross-origin requests; you don't need to do anything special in your JavaScript code.</span></span>

<span data-ttu-id="bbe35-183">Oto przykład żądania między źródłami.</span><span class="sxs-lookup"><span data-stu-id="bbe35-183">Here is an example of a cross-origin request.</span></span> <span data-ttu-id="bbe35-184">Nagłówek "Origin" zapewnia domenę lokacji, która żąda żądania.</span><span class="sxs-lookup"><span data-stu-id="bbe35-184">The "Origin" header gives the domain of the site that is making the request.</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample6.cmd?highlight=5)]

<span data-ttu-id="bbe35-185">Jeśli serwer zezwala na żądanie, ustawia nagłówek Access-Control-Allow-Origin.</span><span class="sxs-lookup"><span data-stu-id="bbe35-185">If the server allows the request, it sets the Access-Control-Allow-Origin header.</span></span> <span data-ttu-id="bbe35-186">Wartość tego nagłówka jest zgodna z nagłówkiem pierwotnym lub jest wartością wieloznaczną "\*", co oznacza, że wszystkie źródła są dozwolone.</span><span class="sxs-lookup"><span data-stu-id="bbe35-186">The value of this header either matches the Origin header, or is the wildcard value "\*", meaning that any origin is allowed.</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample7.cmd?highlight=5)]

<span data-ttu-id="bbe35-187">Jeśli odpowiedź nie zawiera nagłówka Access-Control-Allow-Origin, żądanie AJAX zakończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="bbe35-187">If the response does not include the Access-Control-Allow-Origin header, the AJAX request fails.</span></span> <span data-ttu-id="bbe35-188">W programie przeglądarka nie zezwala na żądanie.</span><span class="sxs-lookup"><span data-stu-id="bbe35-188">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="bbe35-189">Nawet jeśli serwer zwróci pomyślną odpowiedź, przeglądarka nie udostępni odpowiedzi dla aplikacji klienckiej.</span><span class="sxs-lookup"><span data-stu-id="bbe35-189">Even if the server returns a successful response, the browser does not make the response available to the client application.</span></span>

<span data-ttu-id="bbe35-190">**Żądania wstępnego lotu**</span><span class="sxs-lookup"><span data-stu-id="bbe35-190">**Preflight Requests**</span></span>

<span data-ttu-id="bbe35-191">W przypadku niektórych żądań CORS przeglądarka wysyła dodatkowe żądanie o nazwie "żądanie wstępne", zanim wyśle ono rzeczywiste żądanie dotyczące zasobu.</span><span class="sxs-lookup"><span data-stu-id="bbe35-191">For some CORS requests, the browser sends an additional request, called a "preflight request", before it sends the actual request for the resource.</span></span>

<span data-ttu-id="bbe35-192">Jeśli spełnione są następujące warunki, przeglądarka może pominąć żądanie wstępne:</span><span class="sxs-lookup"><span data-stu-id="bbe35-192">The browser can skip the preflight request if the following conditions are true:</span></span>

- <span data-ttu-id="bbe35-193">Metoda żądania jest GET, główna lub OPUBLIKOWANa, *a*</span><span class="sxs-lookup"><span data-stu-id="bbe35-193">The request method is GET, HEAD, or POST, *and*</span></span>
- <span data-ttu-id="bbe35-194">Aplikacja nie ustawia żadnego nagłówka żądania innego niż Accept, Accept-Language, Content-language, Content-Type lub Last-Event-ID *oraz*</span><span class="sxs-lookup"><span data-stu-id="bbe35-194">The application does not set any request headers other than Accept, Accept-Language, Content-Language, Content-Type, or Last-Event-ID, *and*</span></span>
- <span data-ttu-id="bbe35-195">Nagłówek Content-Type (jeśli jest ustawiony) jest jednym z następujących:</span><span class="sxs-lookup"><span data-stu-id="bbe35-195">The Content-Type header (if set) is one of the following:</span></span>

    - <span data-ttu-id="bbe35-196">application/x-www-form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="bbe35-196">application/x-www-form-urlencoded</span></span>
    - <span data-ttu-id="bbe35-197">wieloczęściowe/formularz-dane</span><span class="sxs-lookup"><span data-stu-id="bbe35-197">multipart/form-data</span></span>
    - <span data-ttu-id="bbe35-198">tekst/zwykły</span><span class="sxs-lookup"><span data-stu-id="bbe35-198">text/plain</span></span>

<span data-ttu-id="bbe35-199">Zasada dotycząca nagłówków żądań dotyczy nagłówków, które są ustawiane przez aplikację przez wywołanie **setRequestHeader** w obiekcie **XMLHttpRequest** .</span><span class="sxs-lookup"><span data-stu-id="bbe35-199">The rule about request headers applies to headers that the application sets by calling **setRequestHeader** on the **XMLHttpRequest** object.</span></span> <span data-ttu-id="bbe35-200">(Specyfikacja CORS wywołuje te "nagłówki żądania autorów"). Reguła nie ma zastosowania do nagłówków, które można ustawić w *przeglądarce* , takich jak User-Agent, Host lub Content-Length.</span><span class="sxs-lookup"><span data-stu-id="bbe35-200">(The CORS specification calls these "author request headers".) The rule does not apply to headers the *browser* can set, such as User-Agent, Host, or Content-Length.</span></span>

<span data-ttu-id="bbe35-201">Oto przykład żądania wstępnego:</span><span class="sxs-lookup"><span data-stu-id="bbe35-201">Here is an example of a preflight request:</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample8.cmd?highlight=4-5)]

<span data-ttu-id="bbe35-202">Żądanie przed inspekcją używa metody HTTP OPTIONS.</span><span class="sxs-lookup"><span data-stu-id="bbe35-202">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="bbe35-203">Zawiera dwa specjalne nagłówki:</span><span class="sxs-lookup"><span data-stu-id="bbe35-203">It includes two special headers:</span></span>

- <span data-ttu-id="bbe35-204">Access-Control-Request-Method: metoda HTTP, która będzie używana dla rzeczywistego żądania.</span><span class="sxs-lookup"><span data-stu-id="bbe35-204">Access-Control-Request-Method: The HTTP method that will be used for the actual request.</span></span>
- <span data-ttu-id="bbe35-205">Access-Control-Request-Heads: lista nagłówków żądań, które *aplikacja* ustawi w rzeczywistym żądaniu.</span><span class="sxs-lookup"><span data-stu-id="bbe35-205">Access-Control-Request-Headers: A list of request headers that the *application* set on the actual request.</span></span> <span data-ttu-id="bbe35-206">(Ponownie nie obejmuje to nagłówków, które są ustawiane w przeglądarce).</span><span class="sxs-lookup"><span data-stu-id="bbe35-206">(Again, this does not include headers that the browser sets.)</span></span>

<span data-ttu-id="bbe35-207">Oto przykład odpowiedzi, przy założeniu, że serwer zezwala na żądanie:</span><span class="sxs-lookup"><span data-stu-id="bbe35-207">Here is an example response, assuming that the server allows the request:</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample9.cmd?highlight=6-7)]

<span data-ttu-id="bbe35-208">Odpowiedź zawiera nagłówek Access-Control-Allow-Methods, który zawiera listę dozwolonych metod i opcjonalnie nagłówek Access-Control-Allow-Heads, który zawiera listę dozwolonych nagłówków.</span><span class="sxs-lookup"><span data-stu-id="bbe35-208">The response includes an Access-Control-Allow-Methods header that lists the allowed methods, and optionally an Access-Control-Allow-Headers header, which lists the allowed headers.</span></span> <span data-ttu-id="bbe35-209">W przypadku pomyślnego przeprowadzenia żądania wstępnego przeglądarka wyśle rzeczywiste żądanie zgodnie z wcześniejszym opisem.</span><span class="sxs-lookup"><span data-stu-id="bbe35-209">If the preflight request succeeds, the browser sends the actual request, as described earlier.</span></span>

<span data-ttu-id="bbe35-210">Narzędzia często używane do testowania punktów końcowych z żądaniami opcji wstępnego lotu (na przykład [programu Fiddler](https://www.telerik.com/fiddler) i [Poster](https://www.getpostman.com/)) nie domyślnie wysyłają wymaganych nagłówków opcji.</span><span class="sxs-lookup"><span data-stu-id="bbe35-210">Tools commonly used to test endpoints with preflight OPTIONS requests (for example, [Fiddler](https://www.telerik.com/fiddler) and [Postman](https://www.getpostman.com/)) don't send the required OPTIONS headers by default.</span></span> <span data-ttu-id="bbe35-211">Upewnij się, że nagłówki `Access-Control-Request-Method` i `Access-Control-Request-Headers` są wysyłane wraz z żądaniem, a nagłówki opcji docierają do aplikacji za pomocą usług IIS.</span><span class="sxs-lookup"><span data-stu-id="bbe35-211">Confirm that the `Access-Control-Request-Method` and `Access-Control-Request-Headers` headers are sent with the request and that OPTIONS headers reach the app through IIS.</span></span>

<span data-ttu-id="bbe35-212">Aby skonfigurować usługi IIS do zezwalania aplikacji ASP.NET na odbieranie i obsługę żądań opcji, Dodaj następującą konfigurację do pliku *Web. config* aplikacji w sekcji `<system.webServer><handlers>`:</span><span class="sxs-lookup"><span data-stu-id="bbe35-212">To configure IIS to allow an ASP.NET app to receive and handle OPTION requests, add the following configuration to the app's *web.config* file in the `<system.webServer><handlers>` section:</span></span>

```xml
<system.webServer>
  <handlers>
    <remove name="ExtensionlessUrlHandler-Integrated-4.0" />
    <remove name="OPTIONSVerbHandler" />
    <add name="ExtensionlessUrlHandler-Integrated-4.0" path="*." verb="*" type="System.Web.Handlers.TransferRequestHandler" preCondition="integratedMode,runtimeVersionv4.0" />
  </handlers>
</system.webServer>
```

<span data-ttu-id="bbe35-213">Usunięcie `OPTIONSVerbHandler` uniemożliwia Obsługiwanie żądań opcji przez usługi IIS.</span><span class="sxs-lookup"><span data-stu-id="bbe35-213">The removal of `OPTIONSVerbHandler` prevents IIS from handling OPTIONS requests.</span></span> <span data-ttu-id="bbe35-214">Zastąpienie `ExtensionlessUrlHandler-Integrated-4.0` zezwala na dostęp do aplikacji, ponieważ domyślna Rejestracja modułu zezwala na żądania GET, głowy, POST i DEBUG z adresami URL bez rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="bbe35-214">The replacement of `ExtensionlessUrlHandler-Integrated-4.0` allows OPTIONS requests to reach the app because the default module registration only allows GET, HEAD, POST, and DEBUG requests with extensionless URLs.</span></span>

## <a name="scope-rules-for-enablecors"></a><span data-ttu-id="bbe35-215">Reguły zakresu dla [EnableCors]</span><span class="sxs-lookup"><span data-stu-id="bbe35-215">Scope Rules for [EnableCors]</span></span>

<span data-ttu-id="bbe35-216">Można włączyć funkcję CORS na akcję, na kontroler lub globalnie dla wszystkich kontrolerów internetowego interfejsu API w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bbe35-216">You can enable CORS per action, per controller, or globally for all Web API controllers in your application.</span></span>

<span data-ttu-id="bbe35-217">**Na akcję**</span><span class="sxs-lookup"><span data-stu-id="bbe35-217">**Per Action**</span></span>

<span data-ttu-id="bbe35-218">Aby włączyć mechanizm CORS dla jednej akcji, należy ustawić atrybut **[EnableCors]** dla metody Action.</span><span class="sxs-lookup"><span data-stu-id="bbe35-218">To enable CORS for a single action, set the **[EnableCors]** attribute on the action method.</span></span> <span data-ttu-id="bbe35-219">Poniższy przykład umożliwia włączenie mechanizmu CORS tylko dla metody `GetItem`.</span><span class="sxs-lookup"><span data-stu-id="bbe35-219">The following example enables CORS for the `GetItem` method only.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample10.cs)]

<span data-ttu-id="bbe35-220">**Na kontroler**</span><span class="sxs-lookup"><span data-stu-id="bbe35-220">**Per Controller**</span></span>

<span data-ttu-id="bbe35-221">Jeśli ustawisz **[EnableCors]** w klasie kontrolera, ma ona zastosowanie do wszystkich akcji na kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="bbe35-221">If you set **[EnableCors]** on the controller class, it applies to all the actions on the controller.</span></span> <span data-ttu-id="bbe35-222">Aby wyłączyć mechanizm CORS dla akcji, Dodaj atrybut **[DisableCors]** do akcji.</span><span class="sxs-lookup"><span data-stu-id="bbe35-222">To disable CORS for an action, add the **[DisableCors]** attribute to the action.</span></span> <span data-ttu-id="bbe35-223">Poniższy przykład włącza funkcję CORS dla każdej metody z wyjątkiem `PutItem`.</span><span class="sxs-lookup"><span data-stu-id="bbe35-223">The following example enables CORS for every method except `PutItem`.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample11.cs)]

<span data-ttu-id="bbe35-224">**Globalnie**</span><span class="sxs-lookup"><span data-stu-id="bbe35-224">**Globally**</span></span>

<span data-ttu-id="bbe35-225">Aby włączyć mechanizm CORS dla wszystkich kontrolerów interfejsu API sieci Web w aplikacji, należy przekazać wystąpienie **EnableCorsAttribute** do metody **EnableCors** :</span><span class="sxs-lookup"><span data-stu-id="bbe35-225">To enable CORS for all Web API controllers in your application, pass an **EnableCorsAttribute** instance to the **EnableCors** method:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample12.cs)]

<span data-ttu-id="bbe35-226">Jeśli ustawisz atrybut w więcej niż jednym zakresie, kolejność pierwszeństwa to:</span><span class="sxs-lookup"><span data-stu-id="bbe35-226">If you set the attribute at more than one scope, the order of precedence is:</span></span>

1. <span data-ttu-id="bbe35-227">Akcja</span><span class="sxs-lookup"><span data-stu-id="bbe35-227">Action</span></span>
2. <span data-ttu-id="bbe35-228">Kontroler</span><span class="sxs-lookup"><span data-stu-id="bbe35-228">Controller</span></span>
3. <span data-ttu-id="bbe35-229">Globalny</span><span class="sxs-lookup"><span data-stu-id="bbe35-229">Global</span></span>

## <a name="set-the-allowed-origins"></a><span data-ttu-id="bbe35-230">Ustaw dozwolone źródła</span><span class="sxs-lookup"><span data-stu-id="bbe35-230">Set the allowed origins</span></span>

<span data-ttu-id="bbe35-231">Parametr *Origins* atrybutu **[EnableCors]** określa, które źródła mogą uzyskać dostęp do zasobu.</span><span class="sxs-lookup"><span data-stu-id="bbe35-231">The *origins* parameter of the **[EnableCors]** attribute specifies which origins are allowed to access the resource.</span></span> <span data-ttu-id="bbe35-232">Wartość jest rozdzielaną przecinkami listą dozwolonych źródeł.</span><span class="sxs-lookup"><span data-stu-id="bbe35-232">The value is a comma-separated list of the allowed origins.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample13.cs)]

<span data-ttu-id="bbe35-233">Można również użyć symbolu wieloznacznego "\*", aby zezwolić na żądania z dowolnego źródła.</span><span class="sxs-lookup"><span data-stu-id="bbe35-233">You can also use the wildcard value "\*" to allow requests from any origins.</span></span>

<span data-ttu-id="bbe35-234">Należy uważnie rozważyć przed zezwoleniem na żądania z dowolnego źródła.</span><span class="sxs-lookup"><span data-stu-id="bbe35-234">Consider carefully before allowing requests from any origin.</span></span> <span data-ttu-id="bbe35-235">Oznacza to, że dosłownie każda witryna sieci Web może wykonywać wywołania AJAX do internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="bbe35-235">It means that literally any website can make AJAX calls to your web API.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample14.cs)]

## <a name="set-the-allowed-http-methods"></a><span data-ttu-id="bbe35-236">Ustaw dozwolone metody HTTP</span><span class="sxs-lookup"><span data-stu-id="bbe35-236">Set the allowed HTTP methods</span></span>

<span data-ttu-id="bbe35-237">Parametr *Methods* atrybutu **[EnableCors]** określa, które metody http mogą uzyskać dostęp do zasobu.</span><span class="sxs-lookup"><span data-stu-id="bbe35-237">The *methods* parameter of the **[EnableCors]** attribute specifies which HTTP methods are allowed to access the resource.</span></span> <span data-ttu-id="bbe35-238">Aby zezwolić na wszystkie metody, użyj wartości wieloznacznej "\*".</span><span class="sxs-lookup"><span data-stu-id="bbe35-238">To allow all methods, use the wildcard value "\*".</span></span> <span data-ttu-id="bbe35-239">Poniższy przykład umożliwia tylko żądania GET i POST.</span><span class="sxs-lookup"><span data-stu-id="bbe35-239">The following example allows only GET and POST requests.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample15.cs)]

## <a name="set-the-allowed-request-headers"></a><span data-ttu-id="bbe35-240">Ustaw nagłówki dozwolonych żądań</span><span class="sxs-lookup"><span data-stu-id="bbe35-240">Set the allowed request headers</span></span>

<span data-ttu-id="bbe35-241">W tym artykule opisano wcześniej, jak żądanie wstępnego lotu może zawierać nagłówek Access-Control-Request-Heads, zawierający listę nagłówków HTTP ustawionych przez aplikację (nazywanych dalej "nagłówkami żądań autorów").</span><span class="sxs-lookup"><span data-stu-id="bbe35-241">This article described earlier how a preflight request might include an Access-Control-Request-Headers header, listing the HTTP headers set by the application (the so-called "author request headers").</span></span> <span data-ttu-id="bbe35-242">Parametr *Heads* atrybutu **[EnableCors]** określa, które nagłówki żądań autora są dozwolone.</span><span class="sxs-lookup"><span data-stu-id="bbe35-242">The *headers* parameter of the **[EnableCors]** attribute specifies which author request headers are allowed.</span></span> <span data-ttu-id="bbe35-243">Aby zezwolić na nagłówki, ustaw *nagłówki* na "\*".</span><span class="sxs-lookup"><span data-stu-id="bbe35-243">To allow any headers, set *headers* to "\*".</span></span> <span data-ttu-id="bbe35-244">Aby dozwolonych określone nagłówki, ustaw *nagłówki* na listę dozwolonych nagłówków rozdzielanych przecinkami:</span><span class="sxs-lookup"><span data-stu-id="bbe35-244">To whitelist specific headers, set *headers* to a comma-separated list of the allowed headers:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample16.cs)]

<span data-ttu-id="bbe35-245">Jednak przeglądarki nie są w pełni spójne w sposobie ustawiania dostępu-nagłówka-żądania-nagłówków.</span><span class="sxs-lookup"><span data-stu-id="bbe35-245">However, browsers are not entirely consistent in how they set Access-Control-Request-Headers.</span></span> <span data-ttu-id="bbe35-246">Na przykład program Chrome obecnie zawiera "Źródło".</span><span class="sxs-lookup"><span data-stu-id="bbe35-246">For example, Chrome currently includes "origin".</span></span> <span data-ttu-id="bbe35-247">FireFox nie zawiera standardowych nagłówków, takich jak "Akceptuj", nawet gdy aplikacja ustawia je w skrypcie.</span><span class="sxs-lookup"><span data-stu-id="bbe35-247">FireFox does not include standard headers such as "Accept", even when the application sets them in script.</span></span>

<span data-ttu-id="bbe35-248">Jeśli ustawisz *nagłówki* na inne niż "\*", należy uwzględnić co najmniej "Akceptuj", "Content-Type" i "Origin" oraz wszystkie niestandardowe nagłówki, które mają być obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="bbe35-248">If you set *headers* to anything other than "\*", you should include at least "accept", "content-type", and "origin", plus any custom headers that you want to support.</span></span>

## <a name="set-the-allowed-response-headers"></a><span data-ttu-id="bbe35-249">Ustaw nagłówki dozwolonych odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="bbe35-249">Set the allowed response headers</span></span>

<span data-ttu-id="bbe35-250">Domyślnie przeglądarka nie uwidacznia wszystkich nagłówków odpowiedzi dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bbe35-250">By default, the browser does not expose all of the response headers to the application.</span></span> <span data-ttu-id="bbe35-251">Domyślnie dostępne są nagłówki odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="bbe35-251">The response headers that are available by default are:</span></span>

- <span data-ttu-id="bbe35-252">Cache-Control</span><span class="sxs-lookup"><span data-stu-id="bbe35-252">Cache-Control</span></span>
- <span data-ttu-id="bbe35-253">Content-Language</span><span class="sxs-lookup"><span data-stu-id="bbe35-253">Content-Language</span></span>
- <span data-ttu-id="bbe35-254">Content-Type</span><span class="sxs-lookup"><span data-stu-id="bbe35-254">Content-Type</span></span>
- <span data-ttu-id="bbe35-255">Wygasł</span><span class="sxs-lookup"><span data-stu-id="bbe35-255">Expires</span></span>
- <span data-ttu-id="bbe35-256">Last-Modified</span><span class="sxs-lookup"><span data-stu-id="bbe35-256">Last-Modified</span></span>
- <span data-ttu-id="bbe35-257">Pragm</span><span class="sxs-lookup"><span data-stu-id="bbe35-257">Pragma</span></span>

<span data-ttu-id="bbe35-258">Specyfikacja CORS wywołuje te [proste nagłówki odpowiedzi](https://dvcs.w3.org/hg/cors/raw-file/tip/Overview.html#simple-response-header).</span><span class="sxs-lookup"><span data-stu-id="bbe35-258">The CORS spec calls these [simple response headers](https://dvcs.w3.org/hg/cors/raw-file/tip/Overview.html#simple-response-header).</span></span> <span data-ttu-id="bbe35-259">Aby udostępnić inne nagłówki dla aplikacji, ustaw parametr *exposedHeaders* **[EnableCors]** .</span><span class="sxs-lookup"><span data-stu-id="bbe35-259">To make other headers available to the application, set the *exposedHeaders* parameter of **[EnableCors]**.</span></span>

<span data-ttu-id="bbe35-260">W poniższym przykładzie metoda `Get` kontrolera ustawia niestandardowy nagłówek o nazwie "X-Custom-Header".</span><span class="sxs-lookup"><span data-stu-id="bbe35-260">In the following example, the controller's `Get` method sets a custom header named ‘X-Custom-Header'.</span></span> <span data-ttu-id="bbe35-261">Domyślnie przeglądarka nie ujawnia tego nagłówka w żądaniu między źródłami.</span><span class="sxs-lookup"><span data-stu-id="bbe35-261">By default, the browser will not expose this header in a cross-origin request.</span></span> <span data-ttu-id="bbe35-262">Aby uzyskać dostęp do nagłówka, Dołącz symbol "X-Custom-Header" w *exposedHeaders*.</span><span class="sxs-lookup"><span data-stu-id="bbe35-262">To make the header available, include ‘X-Custom-Header' in *exposedHeaders*.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample17.cs)]

## <a name="pass-credentials-in-cross-origin-requests"></a><span data-ttu-id="bbe35-263">Przekaż poświadczenia w żądaniach między źródłami</span><span class="sxs-lookup"><span data-stu-id="bbe35-263">Pass credentials in cross-origin requests</span></span>

<span data-ttu-id="bbe35-264">Poświadczenia wymagają specjalnej obsługi w żądaniu CORS.</span><span class="sxs-lookup"><span data-stu-id="bbe35-264">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="bbe35-265">Domyślnie przeglądarka nie wysyła żadnych poświadczeń z żądaniem między źródłami.</span><span class="sxs-lookup"><span data-stu-id="bbe35-265">By default, the browser does not send any credentials with a cross-origin request.</span></span> <span data-ttu-id="bbe35-266">Poświadczenia obejmują pliki cookie oraz schematy uwierzytelniania HTTP.</span><span class="sxs-lookup"><span data-stu-id="bbe35-266">Credentials include cookies as well as HTTP authentication schemes.</span></span> <span data-ttu-id="bbe35-267">Aby wysłać poświadczenia z żądaniem między źródłami, klient musi ustawić atrybut **XMLHttpRequest. withCredentials** na wartość true.</span><span class="sxs-lookup"><span data-stu-id="bbe35-267">To send credentials with a cross-origin request, the client must set **XMLHttpRequest.withCredentials** to true.</span></span>

<span data-ttu-id="bbe35-268">Bezpośrednie używanie elementu **XMLHttpRequest** :</span><span class="sxs-lookup"><span data-stu-id="bbe35-268">Using **XMLHttpRequest** directly:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample18.cs)]

<span data-ttu-id="bbe35-269">W platformie jQuery:</span><span class="sxs-lookup"><span data-stu-id="bbe35-269">In jQuery:</span></span>

[!code-javascript[Main](enabling-cross-origin-requests-in-web-api/samples/sample19.js)]

<span data-ttu-id="bbe35-270">Ponadto serwer musi zezwalać na poświadczenia.</span><span class="sxs-lookup"><span data-stu-id="bbe35-270">In addition, the server must allow the credentials.</span></span> <span data-ttu-id="bbe35-271">Aby zezwolić na poświadczenia między źródłami w interfejsie API sieci Web, ustaw właściwość **SupportsCredentials** na wartość true w atrybucie **[EnableCors]** :</span><span class="sxs-lookup"><span data-stu-id="bbe35-271">To allow cross-origin credentials in Web API, set the **SupportsCredentials** property to true on the **[EnableCors]** attribute:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample20.cs)]

<span data-ttu-id="bbe35-272">Jeśli ta właściwość ma wartość true, odpowiedź HTTP będzie zawierać nagłówek Access-Control-Allow-Credentials.</span><span class="sxs-lookup"><span data-stu-id="bbe35-272">If this property is true, the HTTP response will include an Access-Control-Allow-Credentials header.</span></span> <span data-ttu-id="bbe35-273">Ten nagłówek nakazuje przeglądarce, że serwer zezwala na poświadczenia dla żądania między źródłami.</span><span class="sxs-lookup"><span data-stu-id="bbe35-273">This header tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="bbe35-274">Jeśli przeglądarka wysyła poświadczenia, ale odpowiedź nie zawiera prawidłowego nagłówka Access-Control-Allow-Credentials, przeglądarka nie udostępni odpowiedzi aplikacji, a żądanie AJAX zakończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="bbe35-274">If the browser sends credentials, but the response does not include a valid Access-Control-Allow-Credentials header, the browser will not expose the response to the application, and the AJAX request fails.</span></span>

<span data-ttu-id="bbe35-275">Należy zachować ostrożność w przypadku ustawienia **SupportsCredentials** na true, ponieważ oznacza to, że witryna sieci Web w innej domenie może wysyłać poświadczenia zalogowanego użytkownika do internetowego interfejsu API w imieniu użytkownika, bez wiedzy użytkownika.</span><span class="sxs-lookup"><span data-stu-id="bbe35-275">Be careful about setting **SupportsCredentials** to true, because it means a website at another domain can send a logged-in user's credentials to your Web API on the user's behalf, without the user being aware.</span></span> <span data-ttu-id="bbe35-276">Specyfikacja CORS określa również, że *źródła* ustawień &quot;\*&quot; jest nieprawidłowe, jeśli **SupportsCredentials** ma wartość true.</span><span class="sxs-lookup"><span data-stu-id="bbe35-276">The CORS spec also states that setting *origins* to &quot;\*&quot; is invalid if **SupportsCredentials** is true.</span></span>

## <a name="custom-cors-policy-providers"></a><span data-ttu-id="bbe35-277">Niestandardowi dostawcy zasad CORS</span><span class="sxs-lookup"><span data-stu-id="bbe35-277">Custom CORS policy providers</span></span>

<span data-ttu-id="bbe35-278">Atrybut **[EnableCors]** implementuje interfejs **ICorsPolicyProvider** .</span><span class="sxs-lookup"><span data-stu-id="bbe35-278">The **[EnableCors]** attribute implements the **ICorsPolicyProvider** interface.</span></span> <span data-ttu-id="bbe35-279">Aby zapewnić własną implementację, można utworzyć klasę, która pochodzi od **atrybutu** i implementuje **ICorsPolicyProvider**.</span><span class="sxs-lookup"><span data-stu-id="bbe35-279">You can provide your own implementation by creating a class that derives from **Attribute** and implements **ICorsPolicyProvider**.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample21.cs)]

<span data-ttu-id="bbe35-280">Teraz można zastosować atrybut, który można umieścić w dowolnym miejscu **[EnableCors]** .</span><span class="sxs-lookup"><span data-stu-id="bbe35-280">Now you can apply the attribute any place that you would put **[EnableCors]**.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample22.cs)]

<span data-ttu-id="bbe35-281">Na przykład niestandardowy dostawca zasad CORS może odczytać ustawienia z pliku konfiguracyjnego.</span><span class="sxs-lookup"><span data-stu-id="bbe35-281">For example, a custom CORS policy provider could read the settings from a configuration file.</span></span>

<span data-ttu-id="bbe35-282">Zamiast używać atrybutów, można zarejestrować obiekt **ICorsPolicyProviderFactory** , który tworzy obiekty **ICorsPolicyProvider** .</span><span class="sxs-lookup"><span data-stu-id="bbe35-282">As an alternative to using attributes, you can register an **ICorsPolicyProviderFactory** object that creates **ICorsPolicyProvider** objects.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample23.cs)]

<span data-ttu-id="bbe35-283">Aby ustawić **ICorsPolicyProviderFactory**, wywołaj metodę rozszerzenia **SetCorsPolicyProviderFactory** przy uruchamianiu w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="bbe35-283">To set the **ICorsPolicyProviderFactory**, call the **SetCorsPolicyProviderFactory** extension method at startup, as follows:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample24.cs)]

## <a name="browser-support"></a><span data-ttu-id="bbe35-284">Obsługa przeglądarki</span><span class="sxs-lookup"><span data-stu-id="bbe35-284">Browser support</span></span>

<span data-ttu-id="bbe35-285">Pakiet CORS interfejsu API sieci Web jest technologią po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="bbe35-285">The Web API CORS package is a server-side technology.</span></span> <span data-ttu-id="bbe35-286">Przeglądarka użytkownika wymaga również obsługi mechanizmu CORS.</span><span class="sxs-lookup"><span data-stu-id="bbe35-286">The user's browser also needs to support CORS.</span></span> <span data-ttu-id="bbe35-287">Na szczęście bieżące wersje wszystkich głównych przeglądarek obejmują [obsługę mechanizmu CORS](http://caniuse.com/cors).</span><span class="sxs-lookup"><span data-stu-id="bbe35-287">Fortunately, the current versions of all major browsers include [support for CORS](http://caniuse.com/cors).</span></span>

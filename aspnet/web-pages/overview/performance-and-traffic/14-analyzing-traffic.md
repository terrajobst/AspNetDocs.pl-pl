---
uid: web-pages/overview/performance-and-traffic/14-analyzing-traffic
title: Śledzenie informacji odwiedzających (analiza) dla witryny ASP.NET Web Pages (Razor) | Microsoft Docs
author: Rick-Anderson
description: Po zakończeniu korzystania z witryny sieci Web warto przeanalizować ruch w witrynie sieci Web.
ms.author: riande
ms.date: 02/17/2014
ms.assetid: 360bc6e1-84c5-4b8e-a84c-ea48ab807aa4
msc.legacyurl: /web-pages/overview/performance-and-traffic/14-analyzing-traffic
msc.type: authoredcontent
ms.openlocfilehash: 095a5572c755446e0661c052ca9de82d636429fd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78525187"
---
# <a name="tracking-visitor-information-analytics-for-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="1f093-103">Śledzenie informacji odwiedzających (analiza) dla witryny ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="1f093-103">Tracking Visitor Information (Analytics) for an ASP.NET Web Pages (Razor) Site</span></span>

<span data-ttu-id="1f093-104">Autor [FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="1f093-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="1f093-105">W tym artykule opisano sposób dodawania usługi Web Analytics do stron w witrynie sieci Web ASP.NET (Razor) przy użyciu pomocnika.</span><span class="sxs-lookup"><span data-stu-id="1f093-105">This article describes how to use a helper to add website analytics to pages in an ASP.NET Web Pages (Razor) website.</span></span>
> 
> <span data-ttu-id="1f093-106">Zawartość:</span><span class="sxs-lookup"><span data-stu-id="1f093-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="1f093-107">Jak wysyłać informacje o ruchu w witrynie sieci Web do dostawcy analizy.</span><span class="sxs-lookup"><span data-stu-id="1f093-107">How to send information about your website traffic to an analytics provider.</span></span>
> 
> <span data-ttu-id="1f093-108">Są to funkcje programowania ASP.NET wprowadzone w artykule:</span><span class="sxs-lookup"><span data-stu-id="1f093-108">These are the ASP.NET programming features introduced in the article:</span></span>
> 
> - <span data-ttu-id="1f093-109">Pomocnik `Analytics`.</span><span class="sxs-lookup"><span data-stu-id="1f093-109">The `Analytics` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="1f093-110">Wersje oprogramowania używane w samouczku</span><span class="sxs-lookup"><span data-stu-id="1f093-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="1f093-111">ASP.NET strony sieci Web (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="1f093-111">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="1f093-112">Biblioteka pomocników sieci Web ASP.NET (pakiet NuGet)</span><span class="sxs-lookup"><span data-stu-id="1f093-112">ASP.NET Web Helpers Library (NuGet package)</span></span>

<span data-ttu-id="1f093-113">Analiza jest ogólnym terminem dla technologii, która mierzy ruch w witrynie sieci Web, dzięki czemu można zrozumieć, jak użytkownicy korzystają z tej witryny.</span><span class="sxs-lookup"><span data-stu-id="1f093-113">Analytics is a general term for technology that measures traffic on your website so you can understand how people use the site.</span></span> <span data-ttu-id="1f093-114">Dostępnych jest wiele usług analitycznych, w tym usług firmy Google, Yahoo, StatCounter i innych.</span><span class="sxs-lookup"><span data-stu-id="1f093-114">Many analytics services are available, including services from Google, Yahoo, StatCounter, and others.</span></span>

<span data-ttu-id="1f093-115">Sposób działania analizy polega na zarejestrowaniu się w celu uzyskania konta u dostawcy analizy, w którym można zarejestrować lokację, którą chcesz śledzić. Dostawca wysyła fragment kodu JavaScript, który zawiera identyfikator lub kod śledzenia dla Twojego konta.</span><span class="sxs-lookup"><span data-stu-id="1f093-115">The way analytics works is that you sign up for an account with the analytics provider, where you register the site that you want to track. The provider sends you a snippet of JavaScript code that includes an ID or tracking code for your account.</span></span> <span data-ttu-id="1f093-116">Dodaj fragment kodu JavaScript do stron sieci Web w witrynie, która ma być śledzona. (Zazwyczaj można dodać fragment analityczny do stopki lub strony układu lub innego znacznika HTML, który pojawia się na każdej stronie w witrynie). Gdy użytkownicy zażądają strony zawierającej jeden z tych fragmentów kodu JavaScript, fragment kodu wyśle informacje o bieżącej stronie do dostawcy analitycznego, który rejestruje różne szczegółowe informacje o stronie.</span><span class="sxs-lookup"><span data-stu-id="1f093-116">You add the JavaScript snippet to the web pages on the site that you want to track. (You typically add the analytics snippet to a footer or layout page or other HTML markup that appears on every page in your site.) When users request a page that contains one of these JavaScript snippets, the snippet sends information about the current page to the analytics provider, who records various details about the page.</span></span>

<span data-ttu-id="1f093-117">Aby zapoznać się z statystyką lokacji, należy zalogować się do witryny sieci Web dostawcy analizy.</span><span class="sxs-lookup"><span data-stu-id="1f093-117">When you want to have a look at your site statistics, you log into the analytics provider's website.</span></span> <span data-ttu-id="1f093-118">Następnie można wyświetlić wszystkie rodzaje raportów o swojej witrynie, takie jak:</span><span class="sxs-lookup"><span data-stu-id="1f093-118">You can then view all sorts of reports about your site, like:</span></span>

- <span data-ttu-id="1f093-119">Liczba wyświetleń stron dla poszczególnych stron.</span><span class="sxs-lookup"><span data-stu-id="1f093-119">The number of page views for individual pages.</span></span> <span data-ttu-id="1f093-120">Oznacza to, że (w przybliżeniu) liczbę osób odwiedzających witrynę i strony, które są najbardziej popularne.</span><span class="sxs-lookup"><span data-stu-id="1f093-120">This tells you (roughly) how many people are visiting the site, and which pages on your site are the most popular.</span></span>
- <span data-ttu-id="1f093-121">Jak długo ludzie spędzają na określonych stronach.</span><span class="sxs-lookup"><span data-stu-id="1f093-121">How long people spend on specific pages.</span></span> <span data-ttu-id="1f093-122">Może to pomóc w zapewnieniu, że Strona główna ma zainteresować osoby.</span><span class="sxs-lookup"><span data-stu-id="1f093-122">This can tell you things like whether your home page is keeping people's interest.</span></span>
- <span data-ttu-id="1f093-123">Jakie lokacje znajdowały się przed odwiedzeniem Twojej witryny.</span><span class="sxs-lookup"><span data-stu-id="1f093-123">What sites people were on before they visited your site.</span></span> <span data-ttu-id="1f093-124">Dzięki temu można zrozumieć, czy ruch pochodzi z linków, od wyszukiwań i tak dalej.</span><span class="sxs-lookup"><span data-stu-id="1f093-124">This helps you understand whether your traffic is coming from links, from searches, and so on.</span></span>
- <span data-ttu-id="1f093-125">Osoby odwiedzające witrynę i czas ich pobytu.</span><span class="sxs-lookup"><span data-stu-id="1f093-125">When people visit your site and how long they stay.</span></span>
- <span data-ttu-id="1f093-126">Kraje, z których pochodzą odwiedzający.</span><span class="sxs-lookup"><span data-stu-id="1f093-126">What countries your visitors are from.</span></span>
- <span data-ttu-id="1f093-127">Jakich przeglądarek i systemów operacyjnych używają osoby odwiedzające.</span><span class="sxs-lookup"><span data-stu-id="1f093-127">What browsers and operating systems your visitors are using.</span></span>

    ![Ch14traffic-1](14-analyzing-traffic/_static/image1.jpg)

## <a name="using-a-helper-to-add-analytics-to-a-page"></a><span data-ttu-id="1f093-129">Dodawanie analiz do strony przy użyciu pomocnika</span><span class="sxs-lookup"><span data-stu-id="1f093-129">Using a Helper to Add Analytics to a Page</span></span>

<span data-ttu-id="1f093-130">ASP.NET strony sieci Web zawierają kilka pomocników analizy (`Analytics.GetGoogleHtml`, `Analytics.GetYahooHtml`i `Analytics.GetStatCounterHtml`), które ułatwiają zarządzanie fragmentami kodu JavaScript używanymi do analizy.</span><span class="sxs-lookup"><span data-stu-id="1f093-130">ASP.NET Web Pages includes several analytics helpers (`Analytics.GetGoogleHtml`, `Analytics.GetYahooHtml`, and `Analytics.GetStatCounterHtml`) that make it easy to manage the JavaScript snippets used for analytics.</span></span> <span data-ttu-id="1f093-131">Zamiast postanowić się, jak i gdzie umieścić kod JavaScript, wystarczy dodać pomocnika do strony.</span><span class="sxs-lookup"><span data-stu-id="1f093-131">Instead of figuring out how and where to put the JavaScript code, all you have to do is add the helper to a page.</span></span> <span data-ttu-id="1f093-132">Jedyne informacje, które należy podać, to nazwa konta, identyfikator lub kod śledzenia.</span><span class="sxs-lookup"><span data-stu-id="1f093-132">The only information you need to provide is your account name, ID, or tracking code.</span></span> <span data-ttu-id="1f093-133">(W przypadku StatCounter należy również podać kilka dodatkowych wartości).</span><span class="sxs-lookup"><span data-stu-id="1f093-133">(For StatCounter, you also have to provide a few additional values.)</span></span>

<span data-ttu-id="1f093-134">W tej procedurze utworzysz stronę układu, która używa pomocnika `GetGoogleHtml`.</span><span class="sxs-lookup"><span data-stu-id="1f093-134">In this procedure, you'll create a layout page that uses the `GetGoogleHtml` helper.</span></span> <span data-ttu-id="1f093-135">Jeśli masz już konto z innym dostawcą analitycznym, możesz użyć tego konta zamiast tego w razie konieczności wprowadzić niewielkie korekty.</span><span class="sxs-lookup"><span data-stu-id="1f093-135">If you already have an account with one of the other analytics providers, you can use that account instead and make slight adjustments as needed.</span></span>

> [!NOTE]
> <span data-ttu-id="1f093-136">Podczas tworzenia konta usługi Analytics należy zarejestrować adres URL witryny, która ma być śledzona.</span><span class="sxs-lookup"><span data-stu-id="1f093-136">When you create an analytics account, you register the URL of the site that you want to be tracking.</span></span> <span data-ttu-id="1f093-137">Jeśli testujesz wszystko na komputerze lokalnym, nie będziesz śledzić rzeczywistego ruchu (jedynym ruchem), więc nie będzie można rejestrować i wyświetlać statystyk lokacji.</span><span class="sxs-lookup"><span data-stu-id="1f093-137">If you're testing everything on your local computer, you won't be tracking actual traffic (the only traffic is you), so you won't be able to record and view site statistics.</span></span> <span data-ttu-id="1f093-138">Jednak ta procedura pokazuje, jak dodać pomocnika analizy do strony.</span><span class="sxs-lookup"><span data-stu-id="1f093-138">But this procedure shows how you add an analytics helper to a page.</span></span> <span data-ttu-id="1f093-139">Gdy publikujesz swoją witrynę, na żywo witryna wyśle informacje do dostawcy analizy.</span><span class="sxs-lookup"><span data-stu-id="1f093-139">When you publish your site, the live site will send information to your analytics provider.</span></span>

1. <span data-ttu-id="1f093-140">Dodaj bibliotekę pomocników sieci Web ASP.NET do witryny internetowej zgodnie z opisem w temacie [Instalowanie pomocników w witrynie ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), jeśli jeszcze nie została dodana.</span><span class="sxs-lookup"><span data-stu-id="1f093-140">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already added it.</span></span>
2. <span data-ttu-id="1f093-141">Utwórz konto z usługą Google Analytics i Zapisz nazwę konta.</span><span class="sxs-lookup"><span data-stu-id="1f093-141">Create an account with Google Analytics and record the account name.</span></span>
3. <span data-ttu-id="1f093-142">Utwórz stronę układu o nazwie *Analytics. cshtml* i Dodaj następujące znaczniki:</span><span class="sxs-lookup"><span data-stu-id="1f093-142">Create a layout page named *Analytics.cshtml* and add the following markup:</span></span>

    [!code-cshtml[Main](14-analyzing-traffic/samples/sample1.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="1f093-143">Należy umieścić wywołanie pomocnika `Analytics` w treści strony sieci Web (przed tagiem `</body>`).</span><span class="sxs-lookup"><span data-stu-id="1f093-143">You must place the call to the `Analytics` helper in the body of your web page (before the `</body>` tag).</span></span> <span data-ttu-id="1f093-144">W przeciwnym razie przeglądarka nie uruchomi skryptu.</span><span class="sxs-lookup"><span data-stu-id="1f093-144">Otherwise, the browser will not run the script.</span></span>

    <span data-ttu-id="1f093-145">Jeśli używasz innego dostawcy analitycznego, Użyj zamiast niego jednego z następujących pomocników:</span><span class="sxs-lookup"><span data-stu-id="1f093-145">If you're using a different analytics provider, use one of the following helpers instead:</span></span>

    - <span data-ttu-id="1f093-146">(Yahoo) `@Analytics.GetYahooHtml("myaccount")`</span><span class="sxs-lookup"><span data-stu-id="1f093-146">(Yahoo) `@Analytics.GetYahooHtml("myaccount")`</span></span>
    - <span data-ttu-id="1f093-147">(StatCounter) `@Analytics.GetStatCounterHtml("project", "security")`</span><span class="sxs-lookup"><span data-stu-id="1f093-147">(StatCounter) `@Analytics.GetStatCounterHtml("project", "security")`</span></span>
4. <span data-ttu-id="1f093-148">Zastąp `myaccount` nazwą konta, identyfikatora lub kodu śledzenia utworzonego w kroku 1.</span><span class="sxs-lookup"><span data-stu-id="1f093-148">Replace `myaccount` with the name of the account, ID, or tracking code that you created in step 1.</span></span>
5. <span data-ttu-id="1f093-149">Uruchom stronę w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="1f093-149">Run the page in the browser.</span></span> <span data-ttu-id="1f093-150">(Upewnij się, że strona została wybrana w obszarze roboczym **pliki** przed jej uruchomieniem).</span><span class="sxs-lookup"><span data-stu-id="1f093-150">(Make sure the page is selected in the **Files** workspace before you run it.)</span></span>
6. <span data-ttu-id="1f093-151">W przeglądarce Wyświetl źródło strony.</span><span class="sxs-lookup"><span data-stu-id="1f093-151">In the browser, view the page source.</span></span> <span data-ttu-id="1f093-152">Będzie można zobaczyć renderowany kod analityczny:</span><span class="sxs-lookup"><span data-stu-id="1f093-152">You'll be able to see the rendered analytics code:</span></span>

    [!code-html[Main](14-analyzing-traffic/samples/sample2.html)]
7. <span data-ttu-id="1f093-153">Zaloguj się do witryny Google Analytics i sprawdź statystyki dla swojej witryny.</span><span class="sxs-lookup"><span data-stu-id="1f093-153">Log onto the Google Analytics site and examine the statistics for your site.</span></span> <span data-ttu-id="1f093-154">Jeśli uruchamiasz stronę w aktywnej witrynie, zobaczysz wpis służący do rejestrowania odwiedzin na stronie.</span><span class="sxs-lookup"><span data-stu-id="1f093-154">If you're running the page on a live site, you see an entry that logs the visit to your page.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="1f093-155">Dodatkowe materiały</span><span class="sxs-lookup"><span data-stu-id="1f093-155">Additional Resources</span></span>

- [<span data-ttu-id="1f093-156">Witryna usługi Google Analytics</span><span class="sxs-lookup"><span data-stu-id="1f093-156">Google Analytics site</span></span>](https://www.google.com/analytics/)
- [<span data-ttu-id="1f093-157">Witryna usługi Web Analytics dla usługi Yahoo!</span><span class="sxs-lookup"><span data-stu-id="1f093-157">Yahoo! Web Analytics site</span></span>](http://help.yahoo.com/l/us/yahoo/ywa/)
- [<span data-ttu-id="1f093-158">Witryna StatCounter</span><span class="sxs-lookup"><span data-stu-id="1f093-158">StatCounter site</span></span>](http://statcounter.com/)

---
uid: aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
title: Co nie rób w programie ASP.NET i co należy zrobić w zamian | Microsoft Docs
author: Rick-Anderson
description: W tym temacie opisano kilka typowych błędów podejmowanych przez osoby w ramach projektów sieci Web ASP.NET. Zawiera zalecenia dotyczące tego, co należy zrobić, aby uniknąć tych Commo...
ms.author: riande
ms.date: 01/28/2019
ms.assetid: c39b9965-545c-4b04-8f55-21be7f28a9e5
msc.legacyurl: /aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
msc.type: authoredcontent
ms.openlocfilehash: 980d3544df70643043391e6573803ce21b3a824f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78616992"
---
# <a name="what-not-to-do-in-aspnet-and-what-to-do-instead"></a><span data-ttu-id="b9dfd-104">Czego nie robić na platformie ASP.NET i co zrobić zamiast tego</span><span class="sxs-lookup"><span data-stu-id="b9dfd-104">What not to do in ASP.NET, and what to do instead</span></span>

> <span data-ttu-id="b9dfd-105">W tym temacie opisano kilka typowych błędów podejmowanych przez osoby w ramach projektów sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-105">This topic describes several common mistakes people make within ASP.NET web projects.</span></span> <span data-ttu-id="b9dfd-106">Zawiera zalecenia dotyczące tego, co należy zrobić, aby uniknąć tych typowych błędów.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-106">It provides recommendations for what you should do to avoid these common mistakes.</span></span> <span data-ttu-id="b9dfd-107">Jest on oparty na [prezentacji](http://vimeo.com/68390507) **Damian Edwards** na konferencji dla deweloperów w języku norweskim.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-107">It is based on a [presentation](http://vimeo.com/68390507) by **Damian Edwards** at Norwegian Developers Conference.</span></span>

## <a name="disclaimer"></a><span data-ttu-id="b9dfd-108">Zastrzeżenie</span><span class="sxs-lookup"><span data-stu-id="b9dfd-108">Disclaimer</span></span>

<span data-ttu-id="b9dfd-109">Ten temat nie jest przeznaczony dla pełnego przewodnika, aby zapewnić bezpieczeństwo i wydajność aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-109">This topic is not intended as a complete guide to ensure your application is secure and efficient.</span></span> <span data-ttu-id="b9dfd-110">Nadal należy stosować najlepsze rozwiązania w zakresie zabezpieczeń i wydajności, które nie zostały opisane w tym temacie.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-110">You still need to follow best practices for security and performance that are not outlined in this topic.</span></span> <span data-ttu-id="b9dfd-111">Sugeruje tylko, jak uniknąć typowych błędów związanych z klasami i procesami .NET.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-111">It only suggests how to avoid common mistakes related to .NET classes and processes.</span></span>

## <a name="overview"></a><span data-ttu-id="b9dfd-112">Omówienie</span><span class="sxs-lookup"><span data-stu-id="b9dfd-112">Overview</span></span>

<span data-ttu-id="b9dfd-113">Ten temat zawiera następujące sekcje:</span><span class="sxs-lookup"><span data-stu-id="b9dfd-113">This topic contains the following sections:</span></span>

- [<span data-ttu-id="b9dfd-114">Zgodność ze standardami</span><span class="sxs-lookup"><span data-stu-id="b9dfd-114">Standards Compliance</span></span>](#standards)

    - [<span data-ttu-id="b9dfd-115">Karty sterowania</span><span class="sxs-lookup"><span data-stu-id="b9dfd-115">Control Adapters</span></span>](#adapters)
    - [<span data-ttu-id="b9dfd-116">Właściwości stylu w kontrolkach</span><span class="sxs-lookup"><span data-stu-id="b9dfd-116">Style Properties on Controls</span></span>](#styleprop)
    - [<span data-ttu-id="b9dfd-117">Wywołania zwrotne strony i sterowania</span><span class="sxs-lookup"><span data-stu-id="b9dfd-117">Page and Control Callbacks</span></span>](#callback)
    - [<span data-ttu-id="b9dfd-118">Wykrywanie możliwości przeglądarki</span><span class="sxs-lookup"><span data-stu-id="b9dfd-118">Browser Capability Detection</span></span>](#browsercap)
- [<span data-ttu-id="b9dfd-119">Bezpieczeństwo</span><span class="sxs-lookup"><span data-stu-id="b9dfd-119">Security</span></span>](#security)

    - [<span data-ttu-id="b9dfd-120">Żądaj weryfikacji</span><span class="sxs-lookup"><span data-stu-id="b9dfd-120">Request Validation</span></span>](#validation)
    - [<span data-ttu-id="b9dfd-121">Uwierzytelnianie i sesja formularzy bez plików cookie</span><span class="sxs-lookup"><span data-stu-id="b9dfd-121">Cookieless Forms Authentication and Session</span></span>](#cookieless)
    - [<span data-ttu-id="b9dfd-122">EnableViewStateMac</span><span class="sxs-lookup"><span data-stu-id="b9dfd-122">EnableViewStateMac</span></span>](#viewstatemac)
    - [<span data-ttu-id="b9dfd-123">Średnie zaufanie</span><span class="sxs-lookup"><span data-stu-id="b9dfd-123">Medium Trust</span></span>](#medium)
    - [<span data-ttu-id="b9dfd-124">&lt;appSettings&gt;</span><span class="sxs-lookup"><span data-stu-id="b9dfd-124">&lt;appSettings&gt;</span></span>](#appsettings)
    - [<span data-ttu-id="b9dfd-125">UrlPathEncode</span><span class="sxs-lookup"><span data-stu-id="b9dfd-125">UrlPathEncode</span></span>](#urlpathencode)
- [<span data-ttu-id="b9dfd-126">Niezawodność i wydajność</span><span class="sxs-lookup"><span data-stu-id="b9dfd-126">Reliability and Performance</span></span>](#performance)

    - [<span data-ttu-id="b9dfd-127">PreSendRequestHeaders i PreSendRequestContent</span><span class="sxs-lookup"><span data-stu-id="b9dfd-127">PreSendRequestHeaders and PreSendRequestContent</span></span>](#presend)
    - [<span data-ttu-id="b9dfd-128">Asynchroniczne zdarzenia strony z formularzami sieci Web</span><span class="sxs-lookup"><span data-stu-id="b9dfd-128">Asynchronous Page Events with Web Forms</span></span>](#asyncevents)
    - [<span data-ttu-id="b9dfd-129">Działanie ognia i zapomnij</span><span class="sxs-lookup"><span data-stu-id="b9dfd-129">Fire-and-Forget Work</span></span>](#fire)
    - [<span data-ttu-id="b9dfd-130">Treść jednostki żądania</span><span class="sxs-lookup"><span data-stu-id="b9dfd-130">Request Entity Body</span></span>](#requestentity)
    - [<span data-ttu-id="b9dfd-131">Response. Redirect i Response. end</span><span class="sxs-lookup"><span data-stu-id="b9dfd-131">Response.Redirect and Response.End</span></span>](#redirect)
    - [<span data-ttu-id="b9dfd-132">EnableViewState i ViewStatemode</span><span class="sxs-lookup"><span data-stu-id="b9dfd-132">EnableViewState and ViewStateMode</span></span>](#viewstatemode)
    - [<span data-ttu-id="b9dfd-133">SqlMembershipProvider</span><span class="sxs-lookup"><span data-stu-id="b9dfd-133">SqlMembershipProvider</span></span>](#sqlprovider)
    - [<span data-ttu-id="b9dfd-134">Długotrwałe żądania (> 110 s)</span><span class="sxs-lookup"><span data-stu-id="b9dfd-134">Long Running Requests (>110 seconds)</span></span>](#long)

<a id="standards"></a>

## <a name="standards-compliance"></a><span data-ttu-id="b9dfd-135">Zgodność ze standardami</span><span class="sxs-lookup"><span data-stu-id="b9dfd-135">Standards Compliance</span></span>

<a id="adapters"></a>

### <a name="control-adapters"></a><span data-ttu-id="b9dfd-136">Karty sterowania</span><span class="sxs-lookup"><span data-stu-id="b9dfd-136">Control adapters</span></span>

<span data-ttu-id="b9dfd-137">Zalecenie: Przestań korzystać z adapterów kontrolnych na potrzeby adaptacyjnego renderowania, a zamiast tego użyj zapytań dotyczących multimediów CSS i HTML zgodnych ze standardami.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-137">Recommendation: Stop using control adapters for adaptive rendering, and instead use CSS media queries and standards-compliant HTML.</span></span>

<span data-ttu-id="b9dfd-138">Karty sterowania zostały wprowadzone w programie .NET 2,0 w celu renderowania kodu prezentacji, który został dostosowany dla różnych urządzeń i środowisk.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-138">Controls Adapters were introduced in .NET 2.0 to render presentation code that was customized for different devices and environments.</span></span> <span data-ttu-id="b9dfd-139">Teraz takie adaptacyjne renderowanie można wykonać za pomocą CSS i HTML.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-139">Now, this adaptive rendering can be accomplished with CSS and HTML.</span></span> <span data-ttu-id="b9dfd-140">Należy zatrzymać używanie adapterów sterowania i przekonwertować wszystkie istniejące karty na CSS i HTML.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-140">You should stop using Control Adapters and convert any existing adapters to CSS and HTML.</span></span>

<span data-ttu-id="b9dfd-141">Aby uzyskać więcej informacji, zobacz temat [zapytania dotyczące multimediów](http://www.w3.org/TR/css3-mediaqueries/) i [instrukcje: dodawanie stron mobilnych do aplikacji Web Forms/MVC ASP.NET](../../../whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="b9dfd-141">For more information, see [Media Queries](http://www.w3.org/TR/css3-mediaqueries/) and [How To: Add Mobile Pages to Your ASP.NET Web Forms / MVC Application](../../../whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application.md).</span></span>

<a id="styleprop"></a>

### <a name="style-properties-on-controls"></a><span data-ttu-id="b9dfd-142">Właściwości stylu w kontrolkach</span><span class="sxs-lookup"><span data-stu-id="b9dfd-142">Style properties on controls</span></span>

<span data-ttu-id="b9dfd-143">Zalecenie: Zatrzymaj Ustawianie wartości stylu w znaczniku kontrolki, a zamiast tego Ustaw wartości formatowania w arkuszach stylów CSS.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-143">Recommendation: Stop setting style values in the control markup, and instead set formatting values in CSS stylesheets.</span></span>

<span data-ttu-id="b9dfd-144">Formanty serwera sieci Web zawierają dziesiątki właściwości, których można użyć do ustawiania właściwości stylu wbudowanego.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-144">Web server controls contain dozens of properties which can be used to set in-line style properties.</span></span> <span data-ttu-id="b9dfd-145">Na przykład właściwość ForeColor ustawia kolor tekstu dla kontrolki.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-145">For example, the ForeColor property sets the color of the text for a control.</span></span> <span data-ttu-id="b9dfd-146">Ten sam efekt można osiągnąć wydajniej za poorednictwem arkuszy stylów CSS.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-146">You can accomplish this same effect more efficiently through CSS stylesheets.</span></span> <span data-ttu-id="b9dfd-147">Arkusze stylów umożliwiają scentralizowanie wartości stylu i unikanie ustawiania tych wartości w całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-147">Stylesheets enable you to centralize style values and avoid setting these values throughout your application.</span></span>

<span data-ttu-id="b9dfd-148">Poniższy przykład pokazuje klasę CSS, ustawia tekst na czerwony.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-148">The following example shows a CSS class the sets text to red.</span></span>

[!code-css[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample1.css)]

<span data-ttu-id="b9dfd-149">W następnym przykładzie pokazano sposób dynamicznego zastosowania klasy CSS.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-149">The next example shows how to dynamically apply the CSS class.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample2.cs)]

<a id="callback"></a>

### <a name="page-and-control-callbacks"></a><span data-ttu-id="b9dfd-150">Wywołania zwrotne strony i sterowania</span><span class="sxs-lookup"><span data-stu-id="b9dfd-150">Page and control callbacks</span></span>

<span data-ttu-id="b9dfd-151">Zalecenie: Przestań używać funkcji wywołania zwrotne stron i kontrolek, a zamiast tego użyj dowolnej z następujących metod: AJAX, UpdatePanel, MVC, WebAPI lub Signal.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-151">Recommendation: Stop using page and control callbacks, and instead use any of the following: AJAX, UpdatePanel, MVC action methods, Web API, or SignalR.</span></span>

<span data-ttu-id="b9dfd-152">We wcześniejszych wersjach ASP.NET metody wywołania zwrotnego strony i kontroli umożliwiają aktualizowanie części strony sieci Web bez odświeżania całej strony.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-152">In earlier versions of ASP.NET, Page and Control callback methods enabled you to update part of the web page without refreshing an entire page.</span></span> <span data-ttu-id="b9dfd-153">Teraz można wykonywać aktualizacje częściowej strony za poorednictwem [technologii AJAX](../../../ajax/index.md), [UpdatePanel](https://msdn.microsoft.com/library/bb386454.aspx), [MVC](../../../mvc/index.md), [Web API](../../../web-api/index.md) lub [sygnalizującego](../../../signalr/index.md).</span><span class="sxs-lookup"><span data-stu-id="b9dfd-153">You can now accomplish partial-page updates through [AJAX](../../../ajax/index.md), [UpdatePanel](https://msdn.microsoft.com/library/bb386454.aspx), [MVC](../../../mvc/index.md), [Web API](../../../web-api/index.md) or [SignalR](../../../signalr/index.md).</span></span> <span data-ttu-id="b9dfd-154">Należy zatrzymać korzystanie z metod wywołania zwrotnego, ponieważ mogą one powodować problemy z przyjaznymi adresami URL i routingiem.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-154">You should stop using callback methods because they can cause issues with friendly URLs and routing.</span></span> <span data-ttu-id="b9dfd-155">Domyślnie formanty nie włączają metod wywołania zwrotnego, ale jeśli ta funkcja została włączona w kontrolce, należy ją wyłączyć.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-155">By default, controls do not enable callback methods, but if you enabled this feature in a control, you should disable it.</span></span>

<a id="browsercap"></a>

### <a name="browser-capability-detection"></a><span data-ttu-id="b9dfd-156">Wykrywanie możliwości przeglądarki</span><span class="sxs-lookup"><span data-stu-id="b9dfd-156">Browser capability detection</span></span>

<span data-ttu-id="b9dfd-157">Zalecenie: Przestań korzystać z wykrywania możliwości przeglądarki statycznej, a zamiast tego użyj wykrywania funkcji dynamicznych.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-157">Recommendation: Stop using static browser capability detection, and instead use dynamic feature detection.</span></span>

<span data-ttu-id="b9dfd-158">We wcześniejszych wersjach programu ASP.NET obsługiwane funkcje dla każdej przeglądarki zostały zapisane w pliku XML.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-158">In earlier versions of ASP.NET, the supported features for each browser were stored in an XML file.</span></span> <span data-ttu-id="b9dfd-159">Wykrywanie obsługi funkcji za pomocą wyszukiwania statycznego nie jest najlepszym rozwiązaniem.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-159">Detecting feature support through a static lookup is not the best approach.</span></span> <span data-ttu-id="b9dfd-160">Teraz można dynamicznie wykryć obsługiwane funkcje przeglądarki za pomocą platformy wykrywania funkcji, takiej jak program [modernizacja](http://modernizr.com/).</span><span class="sxs-lookup"><span data-stu-id="b9dfd-160">Now, you can dynamically detect a browser's supported features by using a feature detection framework, such as [Modernizr](http://modernizr.com/).</span></span> <span data-ttu-id="b9dfd-161">Wykrywanie funkcji określa pomoc techniczną, próbując użyć metody lub właściwości, a następnie sprawdzić, czy przeglądarka wygenerowała żądany wynik.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-161">Feature detection determines support by attempting to use a method or property and then checking to see if the browser produced the desired result.</span></span> <span data-ttu-id="b9dfd-162">Domyślnie program modernizowany jest dołączony do szablonów aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-162">By default, Modernizr is included in the Web application templates.</span></span>

<a id="security"></a>

## <a name="security"></a><span data-ttu-id="b9dfd-163">Zabezpieczenia</span><span class="sxs-lookup"><span data-stu-id="b9dfd-163">Security</span></span>

<a id="validation"></a>

### <a name="request-validation"></a><span data-ttu-id="b9dfd-164">Żądaj weryfikacji</span><span class="sxs-lookup"><span data-stu-id="b9dfd-164">Request validation</span></span>

<span data-ttu-id="b9dfd-165">Zalecenie: Weryfikuj dane wejściowe użytkownika i Koduj dane wyjściowe od użytkowników.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-165">Recommendation: Validate user input, and encode output from users.</span></span>

<span data-ttu-id="b9dfd-166">Sprawdzanie poprawności żądania jest funkcją ASP.NET, która sprawdza każde żądanie i przerywa żądanie, jeśli zostanie znalezione postrzegane zagrożenie.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-166">Request validation is a feature of ASP.NET that inspects each request and stops the request if a perceived threat is found.</span></span> <span data-ttu-id="b9dfd-167">Nie należy polegać na weryfikacji żądań w celu zabezpieczania aplikacji przed atakami na skrypty między lokacjami.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-167">Do not depend on request validation for securing your application against cross-site scripting attacks.</span></span> <span data-ttu-id="b9dfd-168">Zamiast tego Sprawdź poprawność wszystkich danych wejściowych od użytkowników i Koduj dane wyjściowe.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-168">Instead, validate all input from users and encode the output.</span></span> <span data-ttu-id="b9dfd-169">W niektórych ograniczonych przypadkach można użyć wyrażeń regularnych do walidacji danych wejściowych, ale w bardziej skomplikowanych przypadkach należy zweryfikować dane wejściowe użytkownika przy użyciu klas platformy .NET, które określają, czy wartość jest zgodna z dozwolonymi wartościami.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-169">In some limited cases, you can use regular expressions to validate the input, but in more complicated cases you should validate user input by using .NET classes that determine if the value matches allowed values.</span></span>

<span data-ttu-id="b9dfd-170">Poniższy przykład pokazuje, jak używać metody statycznej w klasie URI w celu ustalenia, czy identyfikator URI podany przez użytkownika jest prawidłowy.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-170">The following example shows how to use a static method in the Uri class to determine whether the Uri provided by a user is valid.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample3.cs)]

<span data-ttu-id="b9dfd-171">Aby jednak zapewnić wystarczającą weryfikację identyfikatora URI, należy również sprawdzić, czy określa `http` lub `https`.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-171">However, to sufficiently verify the Uri, you should also check to make sure it specifies `http` or `https`.</span></span> <span data-ttu-id="b9dfd-172">W poniższym przykładzie są używane metody wystąpień do sprawdzenia, czy identyfikator URI jest prawidłowy.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-172">The following example uses instance methods to verify that the Uri is valid.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample4.cs)]

<span data-ttu-id="b9dfd-173">Przed renderowaniem danych wejściowych użytkownika jako HTML lub z uwzględnieniem danych wejściowych użytkownika w kwerendzie SQL Zakoduj wartości w celu zapewnienia, że złośliwy kod nie jest uwzględniony.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-173">Before rendering user input as HTML or including user input in a SQL query, encode the values to ensure malicious code is not included.</span></span>

<span data-ttu-id="b9dfd-174">Możesz kodować wartość w kodzie HTML za pomocą składni &lt;%:%&gt;, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-174">You can HTML encode the value in markup with the &lt;%: %&gt; syntax, as shown below.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample5.aspx?highlight=1)]

<span data-ttu-id="b9dfd-175">Lub, w składnia Razor, można kodować kod HTML za pomocą @, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-175">Or, in Razor syntax, you can HTML encode with @, as shown below.</span></span>

[!code-cshtml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample6.cshtml?highlight=1)]

<span data-ttu-id="b9dfd-176">W następnym przykładzie pokazano, jak kod HTML kodować wartość w kodzie.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-176">The next example shows how to HTML encode a value in code-behind.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample7.cs)]

<span data-ttu-id="b9dfd-177">Aby bezpiecznie zakodować wartość poleceń SQL, należy użyć parametrów polecenia, takich jak [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx).</span><span class="sxs-lookup"><span data-stu-id="b9dfd-177">To safely encode a value for SQL commands, use command parameters such as the [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx).</span></span> <a id="cookieless"></a>

### <a name="cookieless-forms-authentication-and-session"></a><span data-ttu-id="b9dfd-178">Uwierzytelnianie i sesja formularzy bez plików cookie</span><span class="sxs-lookup"><span data-stu-id="b9dfd-178">Cookieless forms authentication and session</span></span>

<span data-ttu-id="b9dfd-179">Zalecenie: Wymagaj plików cookie.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-179">Recommendation: Require cookies.</span></span>

<span data-ttu-id="b9dfd-180">Przekazywanie informacji o uwierzytelnianiu w ciągu zapytania nie jest bezpieczne.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-180">Passing authentication information in the query string is not secure.</span></span> <span data-ttu-id="b9dfd-181">W związku z tym Wymagaj plików cookie, gdy aplikacja obejmuje uwierzytelnianie.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-181">Therefore, require cookies when your application includes authentication.</span></span> <span data-ttu-id="b9dfd-182">Jeśli plik cookie przechowuje poufne informacje, należy rozważyć wymaganie protokołu SSL dla pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-182">If your cookie stores sensitive information, consider requiring SSL for the cookie.</span></span>

<span data-ttu-id="b9dfd-183">Poniższy przykład pokazuje, jak określić w pliku Web. config, który uwierzytelnianie formularzy wymaga pliku cookie, który jest przesyłany za pośrednictwem protokołu SSL.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-183">The following example shows how to specify in the Web.config file that Forms Authentication requires a cookie that is transmitted over SSL.</span></span>

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample8.xml)]

<a id="viewstatemac"></a>

### <a name="enableviewstatemac"></a><span data-ttu-id="b9dfd-184">EnableViewStateMac</span><span class="sxs-lookup"><span data-stu-id="b9dfd-184">EnableViewStateMac</span></span>

<span data-ttu-id="b9dfd-185">Zalecenie: nigdy nie ustawiono wartości false.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-185">Recommendation: Never set to false.</span></span>

<span data-ttu-id="b9dfd-186">Domyślnie EnableViewStateMac ma wartość true.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-186">By default, EnableViewStateMac is set to true.</span></span> <span data-ttu-id="b9dfd-187">Nawet jeśli aplikacja nie używa stanu widoku, nie należy ustawiać EnableViewStateMac na false.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-187">Even if your application is not using view state, do not set EnableViewStateMac to false.</span></span> <span data-ttu-id="b9dfd-188">Ustawienie tej wartości na false spowoduje, że aplikacja będzie narażona na obsługę skryptów między lokacjami.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-188">Setting this value to false will make your application vulnerable to cross-site scripting.</span></span>

<span data-ttu-id="b9dfd-189">Począwszy od ASP.NET 4.5.2, środowisko uruchomieniowe wymusza **EnableViewStateMac = true**.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-189">Starting with ASP.NET 4.5.2, the runtime enforces **EnableViewStateMac=true**.</span></span> <span data-ttu-id="b9dfd-190">Nawet jeśli zostanie ustawiona na wartość false, środowisko uruchomieniowe zignoruje tę wartość i kontynuuje wartość ustawioną na wartość true.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-190">Even if you set it to false, the runtime ignores this value and proceeds with the value set to true.</span></span> <span data-ttu-id="b9dfd-191">Aby uzyskać więcej informacji, zobacz [ASP.NET 4.5.2 i EnableViewStateMac](https://blogs.msdn.com/b/webdev/archive/2014/05/07/asp-net-4-5-2-and-enableviewstatemac.aspx).</span><span class="sxs-lookup"><span data-stu-id="b9dfd-191">For more information, see [ASP.NET 4.5.2 and EnableViewStateMac](https://blogs.msdn.com/b/webdev/archive/2014/05/07/asp-net-4-5-2-and-enableviewstatemac.aspx).</span></span>

<span data-ttu-id="b9dfd-192">Poniższy przykład pokazuje, jak ustawić EnableViewStateMac na true.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-192">The following example shows how to set EnableViewStateMac to true.</span></span> <span data-ttu-id="b9dfd-193">Nie ma potrzeby rzeczywistego ustawiania tej wartości na wartość true, ponieważ jest ona domyślnie prawdziwa.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-193">You do not need to actually set this value to true because it is true by default.</span></span> <span data-ttu-id="b9dfd-194">Jeśli jednak ustawisz wartość false na dowolnej stronie aplikacji, musisz natychmiast poprawić tę wartość.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-194">However, if you have set it to false on any page in your application, you must immediately correct this value.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample9.aspx)]

<a id="medium"></a>

### <a name="medium-trust"></a><span data-ttu-id="b9dfd-195">Średnie zaufanie</span><span class="sxs-lookup"><span data-stu-id="b9dfd-195">Medium trust</span></span>

<span data-ttu-id="b9dfd-196">Zalecenie: nie należy polegać na średnim zaufaniu (lub żadnym innym poziomie zaufania) jako granicy zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-196">Recommendation: Do not depend on Medium Trust (or any other trust level) as a security boundary.</span></span>

<span data-ttu-id="b9dfd-197">Częściowe zaufanie nie chroni odpowiedniej aplikacji i nie powinno być używane.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-197">Partial trust does not adequately protect your application and should not be used.</span></span> <span data-ttu-id="b9dfd-198">Zamiast tego należy używać pełnego zaufania i izolować aplikacje niezaufane w oddzielnych pulach aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-198">Instead, use Full Trust, and isolate untrusted applications in separate application pools.</span></span> <span data-ttu-id="b9dfd-199">Ponadto Uruchom każdą pulę aplikacji w ramach unikatowej tożsamości.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-199">Also, run each application pool under a unique identity.</span></span> <span data-ttu-id="b9dfd-200">Aby uzyskać więcej informacji, zobacz [ASP.NET częściowe zaufanie nie gwarantuje izolacji aplikacji](https://support.microsoft.com/kb/2698981).</span><span class="sxs-lookup"><span data-stu-id="b9dfd-200">For more information, see [ASP.NET Partial Trust does not guarantee application isolation](https://support.microsoft.com/kb/2698981).</span></span>

<a id="appsettings"></a>

### <a name="ltappsettingsgt"></a><span data-ttu-id="b9dfd-201">&lt;appSettings&gt;</span><span class="sxs-lookup"><span data-stu-id="b9dfd-201">&lt;appSettings&gt;</span></span>

<span data-ttu-id="b9dfd-202">Zalecenie: nie należy wyłączać ustawień zabezpieczeń w &lt;appSettings&gt; elementu.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-202">Recommendation: Do not disable security settings in &lt;appSettings&gt; element.</span></span>

<span data-ttu-id="b9dfd-203">Element appSettings zawiera wiele wartości, które są wymagane dla aktualizacji zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-203">The appSettings element contains many values which are required for security updates.</span></span> <span data-ttu-id="b9dfd-204">Nie należy zmieniać ani wyłączać tych wartości.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-204">You should not change or disable these values.</span></span> <span data-ttu-id="b9dfd-205">Jeśli podczas wdrażania aktualizacji należy wyłączyć te wartości, po zakończeniu wdrażania ponownie włącz je.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-205">If you must disable these values when deploying an update, immediately re-enable after completing deployment.</span></span>

<span data-ttu-id="b9dfd-206">Aby uzyskać szczegółowe informacje, zobacz [ASP.NET AppSettings elementu](https://msdn.microsoft.com/library/hh975440.aspx).</span><span class="sxs-lookup"><span data-stu-id="b9dfd-206">For details, see [ASP.NET appSettings Element](https://msdn.microsoft.com/library/hh975440.aspx).</span></span>

<a id="urlpathencode"></a>

### <a name="urlpathencode"></a><span data-ttu-id="b9dfd-207">UrlPathEncode</span><span class="sxs-lookup"><span data-stu-id="b9dfd-207">UrlPathEncode</span></span>

<span data-ttu-id="b9dfd-208">Zalecenie: zamiast tego należy użyć [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx) .</span><span class="sxs-lookup"><span data-stu-id="b9dfd-208">Recommendation: Use [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx) instead.</span></span>

<span data-ttu-id="b9dfd-209">Metoda UrlPathEncode została dodana do .NET Framework, aby rozwiązać szczególny problem ze zgodnością przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-209">The UrlPathEncode method was added to the .NET Framework to resolve a very specific browser compatibility problem.</span></span> <span data-ttu-id="b9dfd-210">Nie jest to odpowiednio zakodowany adres URL i nie chroni aplikacji przed wykonywaniem skryptów między lokacjami.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-210">It does not adequately encode a URL, and does not protect your application from cross-site scripting.</span></span> <span data-ttu-id="b9dfd-211">Nigdy nie należy używać go w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-211">You should never use it in your application.</span></span> <span data-ttu-id="b9dfd-212">Zamiast tego należy użyć [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx).</span><span class="sxs-lookup"><span data-stu-id="b9dfd-212">Instead, use [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx).</span></span>

<span data-ttu-id="b9dfd-213">Poniższy przykład pokazuje, jak przekazać zakodowany adres URL jako parametr ciągu zapytania dla kontrolki Hyperlink.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-213">The following example shows how to pass an encoded URL as a query string parameter for a hyperlink control.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample10.cs)]

<a id="performance"></a>

## <a name="reliability-and-performance"></a><span data-ttu-id="b9dfd-214">Wydajność i niezawodność</span><span class="sxs-lookup"><span data-stu-id="b9dfd-214">Reliability and performance</span></span>

<a id="presend"></a>

### <a name="presendrequestheaders-and-presendrequestcontent"></a><span data-ttu-id="b9dfd-215">PreSendRequestHeaders i PreSendRequestContent</span><span class="sxs-lookup"><span data-stu-id="b9dfd-215">PreSendRequestHeaders and PreSendRequestContent</span></span>

<span data-ttu-id="b9dfd-216">Zalecenie: nie używaj tych zdarzeń z modułami zarządzanymi.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-216">Recommendation: Do not use these events with managed modules.</span></span> <span data-ttu-id="b9dfd-217">Zamiast tego należy napisać natywny moduł usług IIS w celu wykonania wymaganego zadania.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-217">Instead, write a native IIS module to perform the required task.</span></span> <span data-ttu-id="b9dfd-218">Zobacz [Tworzenie modułów HTTP kodu natywnego](https://msdn.microsoft.com/library/ms693629.aspx).</span><span class="sxs-lookup"><span data-stu-id="b9dfd-218">See [Creating Native-Code HTTP Modules](https://msdn.microsoft.com/library/ms693629.aspx).</span></span>

<span data-ttu-id="b9dfd-219">Można użyć zdarzeń [PreSendRequestHeaders](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestheaders.aspx) i [PreSendRequestContent](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestcontent.aspx) z NATYWNYMI modułami usług IIS.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-219">You can use the [PreSendRequestHeaders](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestheaders.aspx) and [PreSendRequestContent](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestcontent.aspx) events with native IIS modules.</span></span>
> [!WARNING]
> <span data-ttu-id="b9dfd-220">Nie należy używać `PreSendRequestHeaders` i `PreSendRequestContent` z modułami zarządzanymi, które implementują `IHttpModule`.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-220">Do not use `PreSendRequestHeaders` and `PreSendRequestContent` with managed modules that implement `IHttpModule`.</span></span> <span data-ttu-id="b9dfd-221">Ustawienie tych właściwości może powodować problemy z żądaniami asynchronicznymi.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-221">Setting these properties can cause issues with asynchronous requests.</span></span> <span data-ttu-id="b9dfd-222">Kombinacja aplikacji do routingu (ARR) i obiektów WebSockets może prowadzić do wyjątków naruszeń, które mogą spowodować awarię w3wp.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-222">The combination of Application Requested Routing (ARR) and websockets might lead to access violation exceptions that can cause w3wp to crash.</span></span> <span data-ttu-id="b9dfd-223">Na przykład iiscore! W3_CONTEXT_BASE:: GetIsLastNotification + 68 w iiscore. dll spowodował wyjątek naruszenia zasad dostępu (0xC0000005).</span><span class="sxs-lookup"><span data-stu-id="b9dfd-223">For example, iiscore!W3_CONTEXT_BASE::GetIsLastNotification+68 in iiscore.dll has caused an access violation exception (0xC0000005).</span></span>

<a id="asyncevents"></a>

### <a name="asynchronous-page-events-with-web-forms"></a><span data-ttu-id="b9dfd-224">Asynchroniczne zdarzenia strony z formularzami sieci Web</span><span class="sxs-lookup"><span data-stu-id="b9dfd-224">Asynchronous page events with web forms</span></span>

<span data-ttu-id="b9dfd-225">Zalecenie: w formularzach sieci Web Unikaj pisania asynchronicznych metod dla zdarzeń związanych z cyklem życia strony, a zamiast tego użyj [Page. RegisterAsyncTask](https://msdn.microsoft.com/library/system.web.ui.page.registerasynctask.aspx) dla kodu asynchronicznego.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-225">Recommendation: In Web Forms, avoid writing async void methods for Page lifecycle events, and instead use [Page.RegisterAsyncTask](https://msdn.microsoft.com/library/system.web.ui.page.registerasynctask.aspx) for asynchronous code.</span></span>

<span data-ttu-id="b9dfd-226">Po oznaczeniu zdarzenia strony z opcją **Async** i **void**nie można określić, kiedy kod asynchroniczny został zakończony.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-226">When you mark a page event with **async** and **void**, you cannot determine when the asynchronous code has finished.</span></span> <span data-ttu-id="b9dfd-227">Zamiast tego należy użyć Page. RegisterAsyncTask, aby uruchomić kod asynchroniczny w sposób umożliwiający śledzenie jego ukończenia.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-227">Instead, use Page.RegisterAsyncTask to run the asynchronous code in a way that enables you to track its completion.</span></span>

<span data-ttu-id="b9dfd-228">Poniższy przykład pokazuje przycisk obsługi, który zawiera kod asynchroniczny.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-228">The following example shows a button click handler that contains asynchronous code.</span></span> <span data-ttu-id="b9dfd-229">Ten przykład obejmuje Odczytywanie wartości typu ciąg asynchronicznie, który jest dostarczany tylko jako uproszczony przykład zadania asynchronicznego, a nie jako zalecane rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-229">This example includes reading a string value asynchronously, which is provided only as a simplified example of an asynchronous task and not as a recommended practice.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample11.cs)]

<span data-ttu-id="b9dfd-230">W przypadku korzystania z zadań asynchronicznych należy ustawić platformę docelową http środowiska uruchomieniowego na 4,5 (lub nowszą) w pliku Web. config.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-230">If you are using asynchronous Tasks, set the Http runtime target framework to 4.5 (or later) in the Web.config file.</span></span> <span data-ttu-id="b9dfd-231">Ustawienie platformy docelowej na 4,5 powoduje włączenie nowego kontekstu synchronizacji, który został dodany w programie .NET 4,5.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-231">Setting the target framework to 4.5 turns on the new synchronization context that was added in .NET 4.5.</span></span> <span data-ttu-id="b9dfd-232">Ta wartość jest domyślnie ustawiana w nowych projektach w programie Visual Studio, ale nie jest ustawiona, jeśli pracujesz z istniejącym projektem.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-232">This value is set by default in new projects in Visual Studio, but is not be set if you are working with an existing project.</span></span>

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample12.xml)]

<a id="fire"></a>

### <a name="fire-and-forget-work"></a><span data-ttu-id="b9dfd-233">Działanie ognia i zapomnij</span><span class="sxs-lookup"><span data-stu-id="b9dfd-233">Fire-and-forget work</span></span>

<span data-ttu-id="b9dfd-234">Zalecenie: podczas obsługi żądania w ramach ASP.NET, unikaj uruchamiania i zapomnienia działania (takie jak wywołanie metody puli wątków. QueueUserWorkItem lub utworzenie czasomierza, który wielokrotnie wywołuje delegata).</span><span class="sxs-lookup"><span data-stu-id="b9dfd-234">Recommendation: When handling a request within ASP.NET, avoid launching fire-and-forget work (such calling the ThreadPool.QueueUserWorkItem method or creating a timer that repeatedly calls a delegate).</span></span>

<span data-ttu-id="b9dfd-235">Jeśli aplikacja ma działające w ramach programu ASP.NET i zapomnij, że aplikacja może nie być zsynchronizowana. W dowolnym momencie można zniszczyć domenę aplikacji, co oznacza, że ciągły proces nie jest już zgodny z bieżącym stanem aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-235">If your application has fire-and-forget work that runs within ASP.NET, your application can get out of sync. At any time, the app domain can be destroyed which means your ongoing process may no longer match the current state of the application.</span></span>

<span data-ttu-id="b9dfd-236">Ten typ pracy należy przenieść poza ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-236">You should move this type of work outside of ASP.NET.</span></span> <span data-ttu-id="b9dfd-237">Możesz użyć zadań sieci Web, usługi systemu Windows lub roli procesu roboczego na platformie Azure do wykonywania bieżących zadań i uruchamiania tego kodu z innego procesu.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-237">You can use a Web Jobs, Windows Service or a Worker role in Azure to perform ongoing work, and run that code from another process.</span></span>

<span data-ttu-id="b9dfd-238">Jeśli musisz wykonać tę operację w ASP.NET, możesz dodać pakiet NuGet o nazwie [Webbackgrounder](http://www.nuget.org/packages/webbackgrounder) , aby uruchomić kod.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-238">If you must perform this work within ASP.NET, you can add the Nuget package called [WebBackgrounder](http://www.nuget.org/packages/webbackgrounder) to run the code.</span></span>

<a id="requestentity"></a>

### <a name="request-entity-body"></a><span data-ttu-id="b9dfd-239">Treść jednostki żądania</span><span class="sxs-lookup"><span data-stu-id="b9dfd-239">Request entity body</span></span>

<span data-ttu-id="b9dfd-240">Zalecenie: unikanie odczytywania żądania. form lub Request. InputStream przed zdarzeniem wykonania procedury obsługi.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-240">Recommendation: Avoid reading Request.Form or Request.InputStream before the handler's execute event.</span></span>

<span data-ttu-id="b9dfd-241">Najwcześniejsza powinna zostać odczytana z metody Request. form lub Request. InputStream w trakcie zdarzenia wykonania procedury obsługi.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-241">The earliest you should read from Request.Form or Request.InputStream is during the handler's execute event.</span></span> <span data-ttu-id="b9dfd-242">W MVC kontroler jest programem obsługi, a zdarzenie Execute jest wykonywane po uruchomieniu metody akcji.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-242">In MVC, the Controller is the handler and the execute event is when the action method runs.</span></span> <span data-ttu-id="b9dfd-243">W formularzach sieci Web strona jest programem obsługi, a zdarzenie Execute jest wyzwalane po wypełnieniu zdarzenia Page. init.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-243">In Web Forms, the Page is the handler and the execute event is when the Page.Init event fires.</span></span> <span data-ttu-id="b9dfd-244">Jeśli odczytasz treść jednostki żądania wcześniejszą od zdarzenia Execute, zakłócasz przetwarzanie żądania.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-244">If you read the request entity body earlier than the execute event, you interfere with the processing of the request.</span></span>

<span data-ttu-id="b9dfd-245">Jeśli musisz odczytać treść jednostki żądania przed zdarzeniem Execute, użyj metody [Request. GetBufferlessInputStream](https://msdn.microsoft.com/library/ff406798.aspx) lub [Request. GetBufferedInputStream](https://msdn.microsoft.com/library/system.web.httprequest.getbufferedinputstream.aspx).</span><span class="sxs-lookup"><span data-stu-id="b9dfd-245">If you need to read the request entity body before the execute event, use either [Request.GetBufferlessInputStream](https://msdn.microsoft.com/library/ff406798.aspx) or [Request.GetBufferedInputStream](https://msdn.microsoft.com/library/system.web.httprequest.getbufferedinputstream.aspx).</span></span> <span data-ttu-id="b9dfd-246">Korzystając z GetBufferlessInputStream, uzyskujesz strumień nieprzetworzony z żądania i przyjmujemy odpowiedzialność za przetwarzanie całego żądania.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-246">When you use GetBufferlessInputStream, you get the raw stream from the request, and assume responsibility for processing the entire request.</span></span> <span data-ttu-id="b9dfd-247">Po wywołaniu metody GetBufferlessInputStream, Request. form i Request. InputStream są niedostępne, ponieważ nie zostały wypełnione przez ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-247">After calling GetBufferlessInputStream, Request.Form and Request.InputStream are not available because they have not been populated by ASP.NET.</span></span> <span data-ttu-id="b9dfd-248">Gdy używasz GetBufferedInputStream, otrzymujesz kopię strumienia z żądania.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-248">When you use GetBufferedInputStream, you get a copy of the stream from the request.</span></span> <span data-ttu-id="b9dfd-249">Żądania. form i Request. InputStream są nadal dostępne w dalszej części żądania, ponieważ ASP.NET wypełnia drugą kopię.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-249">Request.Form and Request.InputStream are still available later in the request because ASP.NET populates the other copy.</span></span>

<a id="redirect"></a>

### <a name="responseredirect-and-responseend"></a><span data-ttu-id="b9dfd-250">Response. Redirect i Response. end</span><span class="sxs-lookup"><span data-stu-id="b9dfd-250">Response.Redirect and Response.End</span></span>

<span data-ttu-id="b9dfd-251">Zalecenie: należy pamiętać o różnicach w sposobie obsługi wątku po wywołaniu metody [Response. Redirect (String)](https://msdn.microsoft.com/library/t9dwyts4.aspx).</span><span class="sxs-lookup"><span data-stu-id="b9dfd-251">Recommendation: Be aware of differences in how thread is handled after calling [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx).</span></span>

<span data-ttu-id="b9dfd-252">Metoda [Response. Redirect (String)](https://msdn.microsoft.com/library/t9dwyts4.aspx) wywołuje metodę Response. end.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-252">The [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx) method calls the Response.End method.</span></span> <span data-ttu-id="b9dfd-253">W procesie synchronicznym wywołanie metody Request. Redirect powoduje natychmiastowe przerwanie działania bieżącego wątku.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-253">In a synchronous process, calling Request.Redirect causes the current thread to immediately abort.</span></span> <span data-ttu-id="b9dfd-254">Jednak w procesie asynchronicznym wywołanie metody Response. Redirect nie przerywa bieżącego wątku, więc wykonanie kodu będzie kontynuowane dla żądania.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-254">However, in an asynchronous process, calling Response.Redirect does not abort the current thread, so code execution continues for the request.</span></span> <span data-ttu-id="b9dfd-255">W procesie asynchronicznym należy zwrócić zadanie z metody, aby zatrzymać wykonywanie kodu.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-255">In an asynchronous process, you must return the Task from the method to stop the code execution.</span></span>

<span data-ttu-id="b9dfd-256">W projekcie MVC nie należy wywoływać metody Response. Redirect.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-256">In an MVC project, you should not call Response.Redirect.</span></span> <span data-ttu-id="b9dfd-257">Zamiast tego należy zwrócić RedirectResult.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-257">Instead, return a RedirectResult.</span></span>

<a id="viewstatemode"></a>

### <a name="enableviewstate-and-viewstatemode"></a><span data-ttu-id="b9dfd-258">EnableViewState i ViewStatemode</span><span class="sxs-lookup"><span data-stu-id="b9dfd-258">EnableViewState and ViewStateMode</span></span>

<span data-ttu-id="b9dfd-259">Zalecenie: Użyj elementu ViewStatemode zamiast EnableViewState, aby zapewnić szczegółową kontrolę nad tym, które kontrolki używają stanu widoku.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-259">Recommendation: Use ViewStateMode, instead of EnableViewState, to provide granular control over which controls use view state.</span></span>

<span data-ttu-id="b9dfd-260">Gdy ustawisz EnableViewState na false w dyrektywie page, stan widoku jest wyłączony dla wszystkich kontrolek na stronie i nie można go włączyć.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-260">When you set EnableViewState to false in the Page directive, view state is disabled for all controls within the page and cannot be enabled.</span></span> <span data-ttu-id="b9dfd-261">Jeśli chcesz włączyć opcję wyświetlania stanu tylko dla niektórych kontrolek na stronie, ustaw wartość ViewStatemode na wyłączone dla strony.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-261">If you want to enable view state for only certain controls in your page, set ViewStateMode to Disabled for the Page.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample13.aspx)]

<span data-ttu-id="b9dfd-262">Następnie ustaw wartość ViewStatemode na włączone tylko dla kontrolek, które rzeczywiście wymagają stanu widoku.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-262">Then, set ViewStateMode to Enabled on only the controls that actually need view state.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample14.aspx)]

<span data-ttu-id="b9dfd-263">Przez włączenie stanu widoku tylko dla kontrolek, które go potrzebują, można zmniejszyć rozmiar widoku dla stron sieci Web.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-263">By enabling view state for only the controls that need it, you can shrink the size of the view state for your web pages.</span></span>

<a id="sqlprovider"></a>

### <a name="sqlmembershipprovider"></a><span data-ttu-id="b9dfd-264">SqlMembershipProvider</span><span class="sxs-lookup"><span data-stu-id="b9dfd-264">SqlMembershipProvider</span></span>

<span data-ttu-id="b9dfd-265">Zalecenie: Użyj dostawców uniwersalnych.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-265">Recommendation: Use Universal Providers.</span></span>

<span data-ttu-id="b9dfd-266">W bieżących szablonach projektu SqlMembershipProvider został zastąpiony przez [dostawców uniwersalnych ASP.NET](http://www.nuget.org/packages/Microsoft.AspNet.Providers), który jest dostępny jako pakiet NuGet.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-266">In the current project templates, SqlMembershipProvider has been replaced by [ASP.NET Universal Providers](http://www.nuget.org/packages/Microsoft.AspNet.Providers), which is available as a NuGet package.</span></span> <span data-ttu-id="b9dfd-267">Jeśli używasz SqlMembershipProvider w projekcie, który został skompilowany przy użyciu wcześniejszej wersji szablonów, należy przełączyć się na uniwersalne dostawcy.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-267">If you are using SqlMembershipProvider in a project that was built with an earlier version of the templates, you should switch to Universal Providers.</span></span> <span data-ttu-id="b9dfd-268">Dostawcy Uniwersalni pracują ze wszystkimi bazami danych, które są obsługiwane przez Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-268">The Universal Providers work with all databases that are supported by Entity Framework.</span></span>

<span data-ttu-id="b9dfd-269">Aby uzyskać więcej informacji, zobacz [wprowadzenie do uniwersalnych dostawców ASP.NET](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx).</span><span class="sxs-lookup"><span data-stu-id="b9dfd-269">For more information, see [Introducing ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx).</span></span>

<a id="long"></a>

### <a name="long-running-requests-110-seconds"></a><span data-ttu-id="b9dfd-270">Długotrwałe żądania (> 110 sekund)</span><span class="sxs-lookup"><span data-stu-id="b9dfd-270">Long-running requests (>110 seconds)</span></span>

<span data-ttu-id="b9dfd-271">Zalecenie: Użyj obiektów [WebSockets](https://msdn.microsoft.com/library/system.net.websockets.websocket.aspx) lub [sygnalizujących](../../../signalr/index.md) dla podłączonych klientów i Użyj asynchronicznych operacji we/wy.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-271">Recommendation: Use [WebSockets](https://msdn.microsoft.com/library/system.net.websockets.websocket.aspx) or [SignalR](../../../signalr/index.md) for connected clients, and use asynchronous I/O operations.</span></span>

<span data-ttu-id="b9dfd-272">Długotrwałe żądania mogą spowodować nieprzewidywalne wyniki oraz niską wydajność aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-272">Long-running requests can cause unpredictable results and poor performance in your web application.</span></span> <span data-ttu-id="b9dfd-273">Domyślne ustawienie limitu czasu dla żądania wynosi 110 sekund.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-273">The default timeout setting for a request is 110 seconds.</span></span> <span data-ttu-id="b9dfd-274">Jeśli używasz stanu sesji z długotrwałym żądaniem, ASP.NET zwolni blokadę obiektu sesji po 110 sekundach.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-274">If you are using session state with a long-running request, ASP.NET will release the lock on the Session object after 110 seconds.</span></span> <span data-ttu-id="b9dfd-275">Aplikacja może jednak znajdować się w środku operacji w obiekcie sesji, gdy blokada jest wydana, a operacja może nie zakończyć się pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-275">However, your application might be in the middle of an operation on the Session object when the lock is released, and the operation might not complete successfully.</span></span> <span data-ttu-id="b9dfd-276">Jeśli drugie żądanie od użytkownika jest blokowane podczas pierwszego żądania, drugie żądanie może uzyskać dostęp do obiektu sesji w stanie niespójnym.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-276">If a second request from the user is blocked while the first request is running, the second request might access the Session object in an inconsistent state.</span></span>

<span data-ttu-id="b9dfd-277">Jeśli aplikacja zawiera blokowanie (lub synchroniczne) operacje we/wy, aplikacja nie będzie odpowiadać.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-277">If your application includes blocking (or synchronous) I/O operations, the application will be unresponsive.</span></span>

<span data-ttu-id="b9dfd-278">Aby zwiększyć wydajność, Użyj asynchronicznych operacji we/wy w .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-278">To improve performance, use the asynchronous I/O operations in the .NET Framework.</span></span> <span data-ttu-id="b9dfd-279">Należy również użyć obiektów WebSockets lub sygnalizujących do łączenia klientów z serwerem.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-279">Also, use WebSockets or SignalR for connecting clients to the server.</span></span> <span data-ttu-id="b9dfd-280">Te funkcje zostały zaprojektowane w celu wydajnego obsłużenia długotrwałych żądań.</span><span class="sxs-lookup"><span data-stu-id="b9dfd-280">These features are designed to efficiently handle long-running requests.</span></span>

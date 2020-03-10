---
uid: web-pages/overview/ui-layouts-and-themes/twitter-helper
title: Pomocnik usługi Twitter ze stronami sieci Web ASP.NET | Microsoft Docs
author: Rick-Anderson
description: W tym temacie i aplikacji pokazano, jak dodać pomocnika usługi Twitter do projektu WebMatrix 3. Zawiera kod pomocnika usługi Twitter i pokazuje, jak wywołać pomocnika...
ms.author: riande
ms.date: 11/26/2018
ms.assetid: c1a1244e-b9c8-42e6-a00b-8456a4ec027c
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/twitter-helper
msc.type: authoredcontent
ms.openlocfilehash: 76e32b7c808467a9a87c70017dac02bdb895e1df
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78638559"
---
# <a name="twitter-helper-with-aspnet-web-pages"></a><span data-ttu-id="a8d7e-104">Pomocnik usługi Twitter we wzorcu ASP.NET Web Pages</span><span class="sxs-lookup"><span data-stu-id="a8d7e-104">Twitter Helper with ASP.NET Web Pages</span></span>

<span data-ttu-id="a8d7e-105">Autor [FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="a8d7e-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a8d7e-106">Pomocnicy usługi Twitter są przestarzałi.</span><span class="sxs-lookup"><span data-stu-id="a8d7e-106">Twitter Helpers are obsolete.</span></span> <span data-ttu-id="a8d7e-107">Najnowsze narzędzia do zakontraktowań dla witryn sieci Web w usłudze Twitter można znaleźć w temacie [Omówienie usługi Twitter for websites](https://developer.twitter.com/en/docs/twitter-for-websites/overview)</span><span class="sxs-lookup"><span data-stu-id="a8d7e-107">For Twitter's latest engagement tools for websites, see [Twitter for Websites Overview](https://developer.twitter.com/en/docs/twitter-for-websites/overview).</span></span>

> <span data-ttu-id="a8d7e-108">W tym temacie i aplikacji pokazano, jak dodać pomocnika usługi Twitter do projektu WebMatrix 3.</span><span class="sxs-lookup"><span data-stu-id="a8d7e-108">This topic and application show how to add a Twitter Helper to your WebMatrix 3 project.</span></span> <span data-ttu-id="a8d7e-109">Zawiera kod pomocnika usługi Twitter i pokazuje, jak wywołać metody pomocnika.</span><span class="sxs-lookup"><span data-stu-id="a8d7e-109">It contains the Twitter Helper code and shows how to call the helper methods.</span></span>
> 
> <span data-ttu-id="a8d7e-110">Ten kod dla pliku Twitter. cshtml został opracowany przez **Tian przesunięcia** firmy Microsoft.</span><span class="sxs-lookup"><span data-stu-id="a8d7e-110">This code for the Twitter.cshtml file was developed by **Tian Pan** of Microsoft.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="a8d7e-111">Wersje oprogramowania używane w samouczku</span><span class="sxs-lookup"><span data-stu-id="a8d7e-111">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="a8d7e-112">ASP.NET strony sieci Web (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="a8d7e-112">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="a8d7e-113">Ten samouczek działa również z ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="a8d7e-113">This tutorial also works with ASP.NET Web Pages 2.</span></span>

## <a name="introduction"></a><span data-ttu-id="a8d7e-114">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="a8d7e-114">Introduction</span></span>

<span data-ttu-id="a8d7e-115">W tym temacie pokazano, jak dodać pomocnika usługi Twitter do aplikacji i użyć składnia Razor, aby wywołać metody pomocnika.</span><span class="sxs-lookup"><span data-stu-id="a8d7e-115">This topic demonstrates how to add a Twitter Helper to your application and use Razor syntax to call the helper methods.</span></span> <span data-ttu-id="a8d7e-116">Pomocnik usługi Twitter ułatwia włączanie przycisków i widżetów serwisu Twitter w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a8d7e-116">The Twitter Helper makes it easy to incorporate Twitter buttons and widgets in your application.</span></span> <span data-ttu-id="a8d7e-117">Aby użyć widżetu usługi Twitter, takiego jak oś czasu użytkownika lub wyniki wyszukiwania dla tego elementu, musisz najpierw utworzyć [widżet w serwisie Twitter](https://twitter.com/settings/widgets).</span><span class="sxs-lookup"><span data-stu-id="a8d7e-117">To use a Twitter widget, such as a user's timeline or the search results for a hashtag, you must first create the [widget on Twitter](https://twitter.com/settings/widgets).</span></span> <span data-ttu-id="a8d7e-118">Po utworzeniu widżetu zostanie wyświetlony identyfikator widżetu. Ten identyfikator widżetu jest przekazywany jako parametr podczas wywoływania metod pomocnika, które pokazują widżet.</span><span class="sxs-lookup"><span data-stu-id="a8d7e-118">After creating your widget, you will receive a widget id. You pass this widget id as a parameter when calling the helper methods that show widget.</span></span>

<span data-ttu-id="a8d7e-119">Ten temat został zapisany w wersji 1,1 interfejsu API usługi Twitter.</span><span class="sxs-lookup"><span data-stu-id="a8d7e-119">This topic was written for version 1.1 of the Twitter API.</span></span> <span data-ttu-id="a8d7e-120">Bezpośrednio dodając kod pomocnika usługi Twitter do projektu, można zaktualizować kod pomocnika, jeśli interfejs API usługi Twitter ulegnie zmianie.</span><span class="sxs-lookup"><span data-stu-id="a8d7e-120">By directly adding the Twitter Helper code to your project, you can update the helper code if the Twitter API changes.</span></span>

<span data-ttu-id="a8d7e-121">Aby uzyskać informacje na temat instalowania programu WebMatrix, zobacz [wprowadzenie do stron sieci Web ASP.NET 2 — wprowadzenie](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="a8d7e-121">For information about installing WebMatrix, see [Introducing ASP.NET Web Pages 2 - Getting Started](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).</span></span>

## <a name="add-twitter-helper-to-your-project"></a><span data-ttu-id="a8d7e-122">Dodawanie pomocnika usługi Twitter do projektu</span><span class="sxs-lookup"><span data-stu-id="a8d7e-122">Add Twitter Helper to your project</span></span>

<span data-ttu-id="a8d7e-123">Aby dodać pomocnika usługi Twitter, najpierw Dodaj folder o nazwie **App\_Code** do projektu.</span><span class="sxs-lookup"><span data-stu-id="a8d7e-123">To add the Twitter Helper, first, add a folder named **App\_Code** to your project.</span></span> <span data-ttu-id="a8d7e-124">Następnie utwórz plik o nazwie **Twitter. cshtml**.</span><span class="sxs-lookup"><span data-stu-id="a8d7e-124">Then, create a file named **Twitter.cshtml**.</span></span>

![App_Code folder](twitter-helper/_static/image1.png)

<span data-ttu-id="a8d7e-126">Zastąp domyślny kod w serwisie Twitter. cshtml poniższym kodem.</span><span class="sxs-lookup"><span data-stu-id="a8d7e-126">Replace the default code in Twitter.cshtml with the following code.</span></span>

[!code-cshtml[Main](twitter-helper/samples/sample1.cshtml)]

## <a name="call-twitter-methods-from-your-web-pages"></a><span data-ttu-id="a8d7e-127">Wywoływanie metod serwisu Twitter ze stron sieci Web</span><span class="sxs-lookup"><span data-stu-id="a8d7e-127">Call Twitter methods from your web pages</span></span>

<span data-ttu-id="a8d7e-128">Poniższy przykład pokazuje, jak używać metod pomocnika usługi Twitter ze strony w projekcie.</span><span class="sxs-lookup"><span data-stu-id="a8d7e-128">The following example shows how to use the Twitter Helper methods from a page in your project.</span></span> <span data-ttu-id="a8d7e-129">W projekcie należy zamienić wartości parametrów na wartości, które są istotne dla Twoich potrzeb.</span><span class="sxs-lookup"><span data-stu-id="a8d7e-129">In your project, you will want to replace the parameter values with values that are relevant to your needs.</span></span> <span data-ttu-id="a8d7e-130">Możesz użyć dostarczonych identyfikatorów widżetów, aby dowiedzieć się, jak działają metody, ale chcesz wygenerować własne widżety dla projektu.</span><span class="sxs-lookup"><span data-stu-id="a8d7e-130">You can use the provided widget ids to explore how the methods work, but you will want to generate your own widgets for your project.</span></span>

<span data-ttu-id="a8d7e-131">Nie wszystkie parametry podane poniżej są wymagane.</span><span class="sxs-lookup"><span data-stu-id="a8d7e-131">Not all of the parameters shown below are required.</span></span> <span data-ttu-id="a8d7e-132">Parametry opcjonalne służą do dostosowywania sposobu wyświetlania przycisku lub widżetu.</span><span class="sxs-lookup"><span data-stu-id="a8d7e-132">The optional parameters are used to customize how the button or widget is displayed.</span></span> <span data-ttu-id="a8d7e-133">Na przykład przycisk Obserwuj wymaga tylko nazwy użytkownika, ale przykład pokazuje, jak uwzględnić liczbę obserwatorów oraz określić rozmiar przycisku i języka.</span><span class="sxs-lookup"><span data-stu-id="a8d7e-133">For example, the Follow Button only requires the user name to follow, but the example shows how to include the number of followers, and how specify the size of the button and the language.</span></span>

[!code-html[Main](twitter-helper/samples/sample2.html)]

## <a name="see-the-results"></a><span data-ttu-id="a8d7e-134">Zobacz wyniki</span><span class="sxs-lookup"><span data-stu-id="a8d7e-134">See the results</span></span>

<span data-ttu-id="a8d7e-135">Powyższy kod tworzy następujące przyciski i widżety.</span><span class="sxs-lookup"><span data-stu-id="a8d7e-135">The above code produces the following buttons and widgets.</span></span> <span data-ttu-id="a8d7e-136">Te przyciski i widżety są w pełni funkcjonalne, a nie zrzuty ekranu.</span><span class="sxs-lookup"><span data-stu-id="a8d7e-136">These buttons and widgets are fully-functional, not screenshots.</span></span> <span data-ttu-id="a8d7e-137">Przycisk Obserwuj jest wyświetlany w języku hiszpańskim, ponieważ parametr Language został ustawiony na **es**.</span><span class="sxs-lookup"><span data-stu-id="a8d7e-137">The Follow Button is displayed in Spanish because the language parameter was set to **es**.</span></span>

### <a name="follow-button"></a><span data-ttu-id="a8d7e-138">Przycisk Obserwuj</span><span class="sxs-lookup"><span data-stu-id="a8d7e-138">Follow Button</span></span>

<span data-ttu-id="a8d7e-139">[Obserwuj @aspnet)](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span><span class="sxs-lookup"><span data-stu-id="a8d7e-139">[Follow @aspnet)](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span></span>

### <a name="tweet-button"></a><span data-ttu-id="a8d7e-140">Przycisk Tweet</span><span class="sxs-lookup"><span data-stu-id="a8d7e-140">Tweet Button</span></span>

<span data-ttu-id="a8d7e-141">`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>` [tweetu](https://twitter.com/share)</span><span class="sxs-lookup"><span data-stu-id="a8d7e-141">[Tweet](https://twitter.com/share)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span></span>

### <a name="user-timeline-profile"></a><span data-ttu-id="a8d7e-142">Oś czasu użytkownika (profil)</span><span class="sxs-lookup"><span data-stu-id="a8d7e-142">User Timeline (Profile)</span></span>

<span data-ttu-id="a8d7e-143">[Tweety według @aspnet](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="a8d7e-143">[Tweets by @aspnet](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>

### <a name="favorites"></a><span data-ttu-id="a8d7e-144">Ulubione</span><span class="sxs-lookup"><span data-stu-id="a8d7e-144">Favorites</span></span>

<span data-ttu-id="a8d7e-145">[Ulubione tweety według @Microsoft](https://twitter.com/Microsoft/favorites)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="a8d7e-145">[Favorite Tweets by @Microsoft](https://twitter.com/Microsoft/favorites)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>

### <a name="list"></a><span data-ttu-id="a8d7e-146">List</span><span class="sxs-lookup"><span data-stu-id="a8d7e-146">List</span></span>

<span data-ttu-id="a8d7e-147">[Tweety z @Microsoft/MS\_\_grup użytkowników](https://twitter.com/microsoft/ms-consumer-brands/)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="a8d7e-147">[Tweets from @Microsoft/MS\_Consumer\_Bands](https://twitter.com/microsoft/ms-consumer-brands/)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>

### <a name="search"></a><span data-ttu-id="a8d7e-148">Wyszukiwanie</span><span class="sxs-lookup"><span data-stu-id="a8d7e-148">Search</span></span>

<span data-ttu-id="a8d7e-149">[Tweety dotyczące &quot;#asp .net&quot;](https://twitter.com/search?q=%23asp.net)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="a8d7e-149">[Tweets about &quot;#asp.net&quot;](https://twitter.com/search?q=%23asp.net)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>

---
uid: web-pages/overview/ui-layouts-and-themes/twitter-helper
title: Pomocnik ze strony sieci Web ASP.NET w usłudze Twitter | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: W tym temacie i aplikacji pokazują, jak dodać Pomocnik usługi Twitter do projektu program WebMatrix 3. Zawiera kod Pomocnik usługi Twitter i przedstawiono sposób wywoływania pomocnika...
ms.author: riande
ms.date: 11/26/2018
ms.assetid: c1a1244e-b9c8-42e6-a00b-8456a4ec027c
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/twitter-helper
msc.type: authoredcontent
ms.openlocfilehash: 76e32b7c808467a9a87c70017dac02bdb895e1df
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65132768"
---
# <a name="twitter-helper-with-aspnet-web-pages"></a><span data-ttu-id="f4a00-104">Pomocnik usługi Twitter we wzorcu ASP.NET Web Pages</span><span class="sxs-lookup"><span data-stu-id="f4a00-104">Twitter Helper with ASP.NET Web Pages</span></span>

<span data-ttu-id="f4a00-105">przez [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="f4a00-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f4a00-106">Pomocnicy Twitter są nieaktualne.</span><span class="sxs-lookup"><span data-stu-id="f4a00-106">Twitter Helpers are obsolete.</span></span> <span data-ttu-id="f4a00-107">Aby w serwisie Twitter najnowsze narzędzia zaangażowania dla witryn sieci Web, zobacz [usługi Twitter dla witryn sieci Web — Omówienie](https://developer.twitter.com/en/docs/twitter-for-websites/overview).</span><span class="sxs-lookup"><span data-stu-id="f4a00-107">For Twitter's latest engagement tools for websites, see [Twitter for Websites Overview](https://developer.twitter.com/en/docs/twitter-for-websites/overview).</span></span>

> <span data-ttu-id="f4a00-108">W tym temacie i aplikacji pokazują, jak dodać Pomocnik usługi Twitter do projektu program WebMatrix 3.</span><span class="sxs-lookup"><span data-stu-id="f4a00-108">This topic and application show how to add a Twitter Helper to your WebMatrix 3 project.</span></span> <span data-ttu-id="f4a00-109">Zawiera kod Pomocnik usługi Twitter i przedstawiono sposób wywoływania metody pomocnika.</span><span class="sxs-lookup"><span data-stu-id="f4a00-109">It contains the Twitter Helper code and shows how to call the helper methods.</span></span>
> 
> <span data-ttu-id="f4a00-110">Ten kod do pliku Twitter.cshtml został opracowany przez **Pan niż** przez firmę Microsoft.</span><span class="sxs-lookup"><span data-stu-id="f4a00-110">This code for the Twitter.cshtml file was developed by **Tian Pan** of Microsoft.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="f4a00-111">Wersje oprogramowania używanego w tym samouczku</span><span class="sxs-lookup"><span data-stu-id="f4a00-111">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="f4a00-112">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="f4a00-112">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="f4a00-113">W tym samouczku współpracuje również z wzorca ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="f4a00-113">This tutorial also works with ASP.NET Web Pages 2.</span></span>

## <a name="introduction"></a><span data-ttu-id="f4a00-114">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="f4a00-114">Introduction</span></span>

<span data-ttu-id="f4a00-115">W tym temacie przedstawiono sposób dodawania Pomocnik usługi Twitter do aplikacji i użyć składni Razor do wywołania metody pomocnika.</span><span class="sxs-lookup"><span data-stu-id="f4a00-115">This topic demonstrates how to add a Twitter Helper to your application and use Razor syntax to call the helper methods.</span></span> <span data-ttu-id="f4a00-116">Pomocnik usługi Twitter ułatwia przycisków w usłudze Twitter i widżetów w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f4a00-116">The Twitter Helper makes it easy to incorporate Twitter buttons and widgets in your application.</span></span> <span data-ttu-id="f4a00-117">Aby użyć elementu widget Twitter, takie jak osi czasu użytkownika lub wyniki wyszukiwania dla hasztagiem, należy najpierw utworzyć [widżetów w serwisie Twitter](https://twitter.com/settings/widgets).</span><span class="sxs-lookup"><span data-stu-id="f4a00-117">To use a Twitter widget, such as a user's timeline or the search results for a hashtag, you must first create the [widget on Twitter](https://twitter.com/settings/widgets).</span></span> <span data-ttu-id="f4a00-118">Po utworzeniu usługi widżet, zostanie wyświetlony identyfikator elementu widget. Przekażesz ten widżet identyfikator jako parametr podczas wywoływania metody pomocnika, które pokazują elementu widget.</span><span class="sxs-lookup"><span data-stu-id="f4a00-118">After creating your widget, you will receive a widget id. You pass this widget id as a parameter when calling the helper methods that show widget.</span></span>

<span data-ttu-id="f4a00-119">Ten temat został napisany dla wersji 1.1 interfejsu API usługi Twitter.</span><span class="sxs-lookup"><span data-stu-id="f4a00-119">This topic was written for version 1.1 of the Twitter API.</span></span> <span data-ttu-id="f4a00-120">Dodając bezpośrednio kodu Pomocnik usługi Twitter do projektu, należy zaktualizować kod pomocnika zmiany interfejsu API usługi Twitter.</span><span class="sxs-lookup"><span data-stu-id="f4a00-120">By directly adding the Twitter Helper code to your project, you can update the helper code if the Twitter API changes.</span></span>

<span data-ttu-id="f4a00-121">Aby uzyskać informacje o instalowaniu programu WebMatrix, zobacz [Przedstawiamy ASP.NET Web Pages 2 — wprowadzenie](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="f4a00-121">For information about installing WebMatrix, see [Introducing ASP.NET Web Pages 2 - Getting Started](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).</span></span>

## <a name="add-twitter-helper-to-your-project"></a><span data-ttu-id="f4a00-122">Dodaj Pomocnik usługi Twitter do projektu</span><span class="sxs-lookup"><span data-stu-id="f4a00-122">Add Twitter Helper to your project</span></span>

<span data-ttu-id="f4a00-123">Aby dodać Pomocnik usługi Twitter, należy najpierw dodać folder o nazwie **aplikacji\_kodu** do projektu.</span><span class="sxs-lookup"><span data-stu-id="f4a00-123">To add the Twitter Helper, first, add a folder named **App\_Code** to your project.</span></span> <span data-ttu-id="f4a00-124">Następnie utwórz plik o nazwie **Twitter.cshtml**.</span><span class="sxs-lookup"><span data-stu-id="f4a00-124">Then, create a file named **Twitter.cshtml**.</span></span>

![Folderze App_Code](twitter-helper/_static/image1.png)

<span data-ttu-id="f4a00-126">Zamień domyślny kod w Twitter.cshtml następujący kod.</span><span class="sxs-lookup"><span data-stu-id="f4a00-126">Replace the default code in Twitter.cshtml with the following code.</span></span>

[!code-cshtml[Main](twitter-helper/samples/sample1.cshtml)]

## <a name="call-twitter-methods-from-your-web-pages"></a><span data-ttu-id="f4a00-127">Wywoływanie metod Twitter ze stron sieci web</span><span class="sxs-lookup"><span data-stu-id="f4a00-127">Call Twitter methods from your web pages</span></span>

<span data-ttu-id="f4a00-128">Poniższy przykład przedstawia sposób użycia metody Pomocnik usługi Twitter ze strony w twoim projekcie.</span><span class="sxs-lookup"><span data-stu-id="f4a00-128">The following example shows how to use the Twitter Helper methods from a page in your project.</span></span> <span data-ttu-id="f4a00-129">W projekcie należy zastąpić wartości parametru z wartościami, które mają zastosowanie do Twoich potrzeb.</span><span class="sxs-lookup"><span data-stu-id="f4a00-129">In your project, you will want to replace the parameter values with values that are relevant to your needs.</span></span> <span data-ttu-id="f4a00-130">Identyfikatory podana, widżetów można użyć, aby poznać sposób działania metody, ale chcesz wygenerować własne elementy widget w projekcie.</span><span class="sxs-lookup"><span data-stu-id="f4a00-130">You can use the provided widget ids to explore how the methods work, but you will want to generate your own widgets for your project.</span></span>

<span data-ttu-id="f4a00-131">Nie wszystkie parametry poniżej są wymagane.</span><span class="sxs-lookup"><span data-stu-id="f4a00-131">Not all of the parameters shown below are required.</span></span> <span data-ttu-id="f4a00-132">Następujące parametry opcjonalne są używane do dostosować sposób wyświetlania przycisku lub elementu widget.</span><span class="sxs-lookup"><span data-stu-id="f4a00-132">The optional parameters are used to customize how the button or widget is displayed.</span></span> <span data-ttu-id="f4a00-133">Na przykład przycisk Obserwuj wymaga tylko nazwę użytkownika, aby wykonać, ale w przykładzie pokazano sposób obejmują liczbę obserwatorów i jak określić rozmiar przycisku i język.</span><span class="sxs-lookup"><span data-stu-id="f4a00-133">For example, the Follow Button only requires the user name to follow, but the example shows how to include the number of followers, and how specify the size of the button and the language.</span></span>

[!code-html[Main](twitter-helper/samples/sample2.html)]

## <a name="see-the-results"></a><span data-ttu-id="f4a00-134">Zobacz wyniki</span><span class="sxs-lookup"><span data-stu-id="f4a00-134">See the results</span></span>

<span data-ttu-id="f4a00-135">Powyższy kod generuje następujące przyciski i elementy widget.</span><span class="sxs-lookup"><span data-stu-id="f4a00-135">The above code produces the following buttons and widgets.</span></span> <span data-ttu-id="f4a00-136">Te przyciski i elementy widget są w pełni funkcjonalne, nie zrzuty ekranu.</span><span class="sxs-lookup"><span data-stu-id="f4a00-136">These buttons and widgets are fully-functional, not screenshots.</span></span> <span data-ttu-id="f4a00-137">Postępuj zgodnie z przycisku jest wyświetlany w języku hiszpańskim, ponieważ ustawiono parametr języka **es**.</span><span class="sxs-lookup"><span data-stu-id="f4a00-137">The Follow Button is displayed in Spanish because the language parameter was set to **es**.</span></span>

### <a name="follow-button"></a><span data-ttu-id="f4a00-138">Postępuj zgodnie z przycisku</span><span class="sxs-lookup"><span data-stu-id="f4a00-138">Follow Button</span></span>

<span data-ttu-id="f4a00-139">[Postępuj zgodnie z @aspnet)](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span><span class="sxs-lookup"><span data-stu-id="f4a00-139">[Follow @aspnet)](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span></span>

### <a name="tweet-button"></a><span data-ttu-id="f4a00-140">Przycisk tweetu</span><span class="sxs-lookup"><span data-stu-id="f4a00-140">Tweet Button</span></span>

<span data-ttu-id="f4a00-141">[Tweetu](https://twitter.com/share)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span><span class="sxs-lookup"><span data-stu-id="f4a00-141">[Tweet](https://twitter.com/share)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span></span>

### <a name="user-timeline-profile"></a><span data-ttu-id="f4a00-142">Oś czasu użytkownika (profil)</span><span class="sxs-lookup"><span data-stu-id="f4a00-142">User Timeline (Profile)</span></span>

<span data-ttu-id="f4a00-143">[Tweety według @aspnet](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="f4a00-143">[Tweets by @aspnet](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>

### <a name="favorites"></a><span data-ttu-id="f4a00-144">Ulubione</span><span class="sxs-lookup"><span data-stu-id="f4a00-144">Favorites</span></span>

<span data-ttu-id="f4a00-145">[Ulubione Tweety według @Microsoft](https://twitter.com/Microsoft/favorites)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="f4a00-145">[Favorite Tweets by @Microsoft](https://twitter.com/Microsoft/favorites)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>

### <a name="list"></a><span data-ttu-id="f4a00-146">Lista</span><span class="sxs-lookup"><span data-stu-id="f4a00-146">List</span></span>

<span data-ttu-id="f4a00-147">[Opublikuje z @Microsoft/MS \_konsumenta\_Bollingera](https://twitter.com/microsoft/ms-consumer-brands/)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="f4a00-147">[Tweets from @Microsoft/MS\_Consumer\_Bands](https://twitter.com/microsoft/ms-consumer-brands/)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>

### <a name="search"></a><span data-ttu-id="f4a00-148">Wyszukaj</span><span class="sxs-lookup"><span data-stu-id="f4a00-148">Search</span></span>

<span data-ttu-id="f4a00-149">[Opublikuje tweet dotyczący &quot;#asp.net&quot;](https://twitter.com/search?q=%23asp.net)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="f4a00-149">[Tweets about &quot;#asp.net&quot;](https://twitter.com/search?q=%23asp.net)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>

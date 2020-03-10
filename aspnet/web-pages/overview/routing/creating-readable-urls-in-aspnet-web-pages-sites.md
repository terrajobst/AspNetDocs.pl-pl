---
uid: web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
title: Tworzenie możliwych do odczytania adresów URL w witrynach ASP.NET Web Pages (Razor) | Microsoft Docs
author: Rick-Anderson
description: W tym artykule opisano Routing w witrynie internetowej ASP.NET Web Pages (Razor) i sposób, w jaki można korzystać z adresów URL, które są bardziej czytelne i lepsze w przypadku optymalizacji. Co się stanie...
ms.author: riande
ms.date: 02/17/2014
ms.assetid: a8aac1ac-89de-4415-afe0-97a41c6423d2
msc.legacyurl: /web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: 832db8e144cab730f16c78f67c12feb9b7c92c7c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78628395"
---
# <a name="creating-readable-urls-in-aspnet-web-pages-razor-sites"></a><span data-ttu-id="3dc5c-104">Tworzenie możliwych do odczytania adresów URL w witrynach ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="3dc5c-104">Creating Readable URLs in ASP.NET Web Pages (Razor) Sites</span></span>

<span data-ttu-id="3dc5c-105">Autor [FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="3dc5c-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="3dc5c-106">W tym artykule opisano Routing w witrynie internetowej ASP.NET Web Pages (Razor) i sposób, w jaki można korzystać z adresów URL, które są bardziej czytelne i lepsze w przypadku optymalizacji.</span><span class="sxs-lookup"><span data-stu-id="3dc5c-106">This article describes routing in an ASP.NET Web Pages (Razor) website, and how this lets you use URLs that are more readable and better for SEO.</span></span>
> 
> <span data-ttu-id="3dc5c-107">Zawartość:</span><span class="sxs-lookup"><span data-stu-id="3dc5c-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="3dc5c-108">Jak ASP.NET korzysta z routingu, aby umożliwić korzystanie z bardziej czytelnych i przeszukiwanych adresów URL.</span><span class="sxs-lookup"><span data-stu-id="3dc5c-108">How ASP.NET uses routing to let you use more readable and searchable URLs.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="3dc5c-109">Wersje oprogramowania używane w samouczku</span><span class="sxs-lookup"><span data-stu-id="3dc5c-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="3dc5c-110">ASP.NET strony sieci Web (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="3dc5c-110">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="3dc5c-111">Ten samouczek działa również z ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="3dc5c-111">This tutorial also works with ASP.NET Web Pages 2.</span></span>

## <a name="about-routing"></a><span data-ttu-id="3dc5c-112">Informacje o routingu</span><span class="sxs-lookup"><span data-stu-id="3dc5c-112">About Routing</span></span>

<span data-ttu-id="3dc5c-113">Adresy URL stron w witrynie mogą mieć wpływ na działanie lokacji.</span><span class="sxs-lookup"><span data-stu-id="3dc5c-113">The URLs for the pages in your site can have an impact on how well the site works.</span></span> <span data-ttu-id="3dc5c-114">Adres URL, który jest &quot;przyjazny&quot; może ułatwić innym osobom korzystanie z tej witryny.</span><span class="sxs-lookup"><span data-stu-id="3dc5c-114">A URL that's &quot;friendly&quot; can make it easier for people to use the site.</span></span> <span data-ttu-id="3dc5c-115">Może również pomóc w połączeniu z optymalizacją aparatu wyszukiwania (wyszukiwarka) dla witryny.</span><span class="sxs-lookup"><span data-stu-id="3dc5c-115">It can also help with search-engine optimization (SEO) for the site.</span></span> <span data-ttu-id="3dc5c-116">Witryny sieci Web ASP.NET umożliwiają automatyczne korzystanie z przyjaznych adresów URL.</span><span class="sxs-lookup"><span data-stu-id="3dc5c-116">ASP.NET websites include the ability to use friendly URLs automatically.</span></span>

<span data-ttu-id="3dc5c-117">ASP.NET umożliwia tworzenie znaczących adresów URL, które opisują akcje użytkownika zamiast wskazywać na plik na serwerze.</span><span class="sxs-lookup"><span data-stu-id="3dc5c-117">ASP.NET lets you create meaningful URLs that describe user actions instead of just pointing to a file on the server.</span></span> <span data-ttu-id="3dc5c-118">Te adresy URL należy wziąć pod uwagę dla fikcyjnego bloga:</span><span class="sxs-lookup"><span data-stu-id="3dc5c-118">Consider these URLs for a fictional blog:</span></span>

- `http://www.contoso.com/Blog/blog.cshtml?categories=hardware`
- `http://www.contoso.com//Blog/blog.cshtml?startdate=2009-11-01&enddate=2009-11-30`

<span data-ttu-id="3dc5c-119">Porównaj te adresy URL z następującymi:</span><span class="sxs-lookup"><span data-stu-id="3dc5c-119">Compare those URLs to the following ones:</span></span>

- `http://www.contoso.com/Blog/categories/hardware/`
- `http://www.contoso.com/Blog/2009/November`

<span data-ttu-id="3dc5c-120">W pierwszej parze użytkownik musi wiedzieć, że blog jest wyświetlany przy użyciu strony *blog. cshtml* , a następnie musi utworzyć ciąg zapytania, który pobiera prawą kategorię lub zakres dat.</span><span class="sxs-lookup"><span data-stu-id="3dc5c-120">In the first pair, a user would have to know that the blog is displayed using the *blog.cshtml* page, and would then have to construct a query string that gets the right category or date range.</span></span> <span data-ttu-id="3dc5c-121">Drugi zestaw przykładów jest znacznie łatwiejszy do comprehend i tworzenia.</span><span class="sxs-lookup"><span data-stu-id="3dc5c-121">The second set of examples is much easier to comprehend and create.</span></span>

<span data-ttu-id="3dc5c-122">Adresy URL pierwszego przykładu wskazują również na określony plik (*blog. cshtml*).</span><span class="sxs-lookup"><span data-stu-id="3dc5c-122">The URLs for the first example also point directly to a specific file (*blog.cshtml*).</span></span> <span data-ttu-id="3dc5c-123">Jeśli z jakiegoś powodu blog został przeniesiony do innego folderu na serwerze, lub jeśli blog został ponownie zapisany w celu użycia innej strony, linki byłyby nieodpowiednie.</span><span class="sxs-lookup"><span data-stu-id="3dc5c-123">If for some reason the blog were moved to another folder on the server, or if the blog were rewritten to use a different page, the links would be wrong.</span></span> <span data-ttu-id="3dc5c-124">Drugi zestaw adresów URL nie wskazuje na określoną stronę, więc nawet w przypadku zmiany implementacji lub lokalizacji blogu adresy URL nadal będą prawidłowe.</span><span class="sxs-lookup"><span data-stu-id="3dc5c-124">The second set of URLs doesn't point to a specific page, so even if the blog implementation or location changes, the URLs would still be valid.</span></span>

<span data-ttu-id="3dc5c-125">Na stronach sieci Web ASP.NET można tworzyć bardziej przyjaznej adresy URL, takie jak te w powyższych przykładach, ponieważ usługa ASP.NET używa *routingu*.</span><span class="sxs-lookup"><span data-stu-id="3dc5c-125">In ASP.NET Web Pages, you can create friendlier URLs like those in the above examples because ASP.NET uses *routing*.</span></span> <span data-ttu-id="3dc5c-126">Routing tworzy logiczne mapowanie na podstawie adresu URL strony (lub stron), które mogą spełnić żądanie.</span><span class="sxs-lookup"><span data-stu-id="3dc5c-126">Routing creates logical mapping from a URL to a page (or pages) that can fulfill the request.</span></span> <span data-ttu-id="3dc5c-127">Ponieważ mapowanie jest logiczne (nie fizyczne, do określonego pliku), routing zapewnia dużą elastyczność w zakresie definiowania adresów URL dla witryny.</span><span class="sxs-lookup"><span data-stu-id="3dc5c-127">Because the mapping is logical (not physical, to a specific file), routing provides great flexibility in how you define the URLs for your site.</span></span>

## <a name="how-routing-works"></a><span data-ttu-id="3dc5c-128">Jak działa Routing</span><span class="sxs-lookup"><span data-stu-id="3dc5c-128">How Routing Works</span></span>

<span data-ttu-id="3dc5c-129">Gdy ASP.NET przetwarza żądanie, odczytuje adres URL, aby określić sposób ich trasy.</span><span class="sxs-lookup"><span data-stu-id="3dc5c-129">When ASP.NET processes a request, it reads the URL to determine how to route it.</span></span> <span data-ttu-id="3dc5c-130">ASP.NET próbuje dopasować poszczególne segmenty adresu URL do plików na dysku, przechodząc od lewej do prawej.</span><span class="sxs-lookup"><span data-stu-id="3dc5c-130">ASP.NET tries to match individual segments of the URL to files on disk, going from left to right.</span></span> <span data-ttu-id="3dc5c-131">W przypadku dopasowania wszystkie pozostałe elementy w adresie URL są przesyłane do strony jako *Informacje o ścieżce*.</span><span class="sxs-lookup"><span data-stu-id="3dc5c-131">If there's a match, anything remaining in the URL is passed to the page as *path information*.</span></span>

<span data-ttu-id="3dc5c-132">Załóżmy, że ktoś wysyła żądanie przy użyciu tego adresu URL:</span><span class="sxs-lookup"><span data-stu-id="3dc5c-132">Imagine that someone makes a request using this URL:</span></span>

`http://www.contoso.com/a/b/c`

<span data-ttu-id="3dc5c-133">Wyszukiwanie przebiega następująco:</span><span class="sxs-lookup"><span data-stu-id="3dc5c-133">The search goes like this:</span></span>

1. <span data-ttu-id="3dc5c-134">Czy istnieje plik o ścieżce i nazwie */a/b/c.cshtml*?</span><span class="sxs-lookup"><span data-stu-id="3dc5c-134">Is there a file with the path and name of */a/b/c.cshtml*?</span></span> <span data-ttu-id="3dc5c-135">Jeśli tak, uruchom tę stronę i Przekaż do niej informacje.</span><span class="sxs-lookup"><span data-stu-id="3dc5c-135">If so, run that page and pass no information to it.</span></span> <span data-ttu-id="3dc5c-136">W innym przypadku...</span><span class="sxs-lookup"><span data-stu-id="3dc5c-136">Otherwise ...</span></span>
2. <span data-ttu-id="3dc5c-137">Czy istnieje plik o ścieżce i nazwie */a/b.cshtml*?</span><span class="sxs-lookup"><span data-stu-id="3dc5c-137">Is there a file with the path and name of */a/b.cshtml*?</span></span> <span data-ttu-id="3dc5c-138">Jeśli tak, uruchom tę stronę i przekaż wartość `c`.</span><span class="sxs-lookup"><span data-stu-id="3dc5c-138">If so, run that page and pass the value `c` to it.</span></span> <span data-ttu-id="3dc5c-139">W przeciwnym razie...</span><span class="sxs-lookup"><span data-stu-id="3dc5c-139">Otherwise …</span></span>
3. <span data-ttu-id="3dc5c-140">Czy istnieje plik o ścieżce i nazwie */a.cshtml*?</span><span class="sxs-lookup"><span data-stu-id="3dc5c-140">Is there a file with the path and name of */a.cshtml*?</span></span> <span data-ttu-id="3dc5c-141">Jeśli tak, uruchom tę stronę i przekaż wartość `b/c`.</span><span class="sxs-lookup"><span data-stu-id="3dc5c-141">If so, run that page and pass the value `b/c` to it.</span></span>

<span data-ttu-id="3dc5c-142">Jeśli w określonych folderach nie znaleziono dokładnego dopasowania dla plików *. cshtml* , ASP.NET nadal szuka następujących plików:</span><span class="sxs-lookup"><span data-stu-id="3dc5c-142">If the search found no exact matches for *.cshtml* files in their specified folders, ASP.NET continues looking for these files in turn:</span></span>

1. <span data-ttu-id="3dc5c-143">*/a/b/c/default.cshtml* (brak informacji o ścieżce).</span><span class="sxs-lookup"><span data-stu-id="3dc5c-143">*/a/b/c/default.cshtml* (no path information).</span></span>
2. <span data-ttu-id="3dc5c-144">*/a/b/c/index.cshtml* (brak informacji o ścieżce).</span><span class="sxs-lookup"><span data-stu-id="3dc5c-144">*/a/b/c/index.cshtml* (no path information).</span></span>

> [!NOTE]
> <span data-ttu-id="3dc5c-145">Aby można było wyczyścić, żądania dla określonych stron (czyli żądań zawierających rozszerzenie nazwy pliku *. cshtml* ) działają podobnie jak w przypadku oczekiwań.</span><span class="sxs-lookup"><span data-stu-id="3dc5c-145">To be clear, requests for specific pages (that is, requests that include the *.cshtml* filename extension) work just like you'd expect.</span></span> <span data-ttu-id="3dc5c-146">Żądanie, takie jak `http://www.contoso.com/a/b.cshtml`, uruchomi stronę *b. cshtml* w prawidłowy sposób.</span><span class="sxs-lookup"><span data-stu-id="3dc5c-146">A request like `http://www.contoso.com/a/b.cshtml` will run the page *b.cshtml* just fine.</span></span>

<span data-ttu-id="3dc5c-147">Wewnątrz strony możesz uzyskać informacje o ścieżce za pośrednictwem właściwości `UrlData` strony, która jest słownikiem.</span><span class="sxs-lookup"><span data-stu-id="3dc5c-147">Inside a page, you can get the path information via the page's `UrlData` property, which is a dictionary.</span></span> <span data-ttu-id="3dc5c-148">Załóżmy, że masz plik o nazwie *ViewCustomers. cshtml* , a witryna otrzymuje to żądanie:</span><span class="sxs-lookup"><span data-stu-id="3dc5c-148">Imagine that you have a file named *ViewCustomers.cshtml* and your site gets this request:</span></span>

`http://mysite.com/myWebSite/ViewCustomers/1000`

<span data-ttu-id="3dc5c-149">Zgodnie z opisem w powyższych regułach żądanie zostanie umieszczone na stronie.</span><span class="sxs-lookup"><span data-stu-id="3dc5c-149">As described in the rules above, the request will go to your page.</span></span> <span data-ttu-id="3dc5c-150">Wewnątrz strony można użyć kodu, takiego jak następujące, aby uzyskać i wyświetlić informacje o ścieżce (w tym przypadku wartość &quot;1000&quot;):</span><span class="sxs-lookup"><span data-stu-id="3dc5c-150">Inside the page, you can use code like the following to get and display the path information (in this case, the value &quot;1000&quot;):</span></span>

[!code-html[Main](creating-readable-urls-in-aspnet-web-pages-sites/samples/sample1.html)]

> [!NOTE]
> <span data-ttu-id="3dc5c-151">Ponieważ Routing nie obejmuje pełnych nazw plików, może wystąpić niejednoznaczność, jeśli masz strony mające taką samą nazwę, ale inne rozszerzenia nazwy pliku (np *. webpage. cshtml* i *webpage. html*).</span><span class="sxs-lookup"><span data-stu-id="3dc5c-151">Because routing doesn't involve complete file names, there can be ambiguity if you have pages that have the same name but different file-name extensions (for example, *MyPage.cshtml* and *MyPage.html*).</span></span> <span data-ttu-id="3dc5c-152">Aby uniknąć problemów z routingiem, najlepiej upewnić się, że nie masz stron w swojej witrynie, których nazwy różnią się tylko w ich rozszerzeniu.</span><span class="sxs-lookup"><span data-stu-id="3dc5c-152">In order to avoid problems with routing, it's best to make sure that you don't have pages in your site whose names differ only in their extension.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="3dc5c-153">Dodatkowe materiały</span><span class="sxs-lookup"><span data-stu-id="3dc5c-153">Additional Resources</span></span>

<span data-ttu-id="3dc5c-154">[WebMatrix — adresy URL, UrlData i Routing dla aparatu optymalizacji](http://www.mikesdotnetting.com/Article/165/WebMatrix-URLs-UrlData-and-Routing-for-SEO).</span><span class="sxs-lookup"><span data-stu-id="3dc5c-154">[WebMatrix - URLs, UrlData and Routing for SEO](http://www.mikesdotnetting.com/Article/165/WebMatrix-URLs-UrlData-and-Routing-for-SEO).</span></span> <span data-ttu-id="3dc5c-155">Ten wpis w blogu z Jan solance zawiera kilka dodatkowych informacji na temat sposobu działania routingu na stronach sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="3dc5c-155">This blog entry by Mike Brind provides some additional details on how routing works in ASP.NET Web Pages.</span></span>

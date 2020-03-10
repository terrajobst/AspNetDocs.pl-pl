---
uid: web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
title: Wyświetlanie map w witrynie ASP.NET Web Pages (Razor) | Microsoft Docs
author: Rick-Anderson
description: W tym artykule wyjaśniono, jak wyświetlać mapy interaktywne na stronach w witrynie internetowej ASP.NET Web Pages (Razor) na podstawie mapowania usług świadczonych przez usługę Bing, Google, ma...
ms.author: riande
ms.date: 02/20/2014
ms.assetid: b5c268dd-ca6a-4562-b94c-a220fcf01f58
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 36f3b753cf312504892872ff54bef49854588990
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78638678"
---
# <a name="displaying-maps-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="17e3f-103">Wyświetlanie map w witrynie ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="17e3f-103">Displaying Maps in an ASP.NET Web Pages (Razor) Site</span></span>

<span data-ttu-id="17e3f-104">Autor [FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="17e3f-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="17e3f-105">W tym artykule wyjaśniono, jak wyświetlać mapy interaktywne na stronach w witrynie internetowej ASP.NET Web Pages (Razor) na podstawie mapowania usług świadczonych przez usługę Bing, Google, MapQuest i Yahoo.</span><span class="sxs-lookup"><span data-stu-id="17e3f-105">This article explains how to display interactive maps on pages in an ASP.NET Web Pages (Razor) website based on mapping services provided by Bing, Google, MapQuest, and Yahoo.</span></span>
> 
> <span data-ttu-id="17e3f-106">Zawartość:</span><span class="sxs-lookup"><span data-stu-id="17e3f-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="17e3f-107">Jak wygenerować mapę na podstawie adresu.</span><span class="sxs-lookup"><span data-stu-id="17e3f-107">How to generate a map based on an address.</span></span>
> - <span data-ttu-id="17e3f-108">Jak wygenerować mapę na podstawie współrzędnych szerokości i długości geograficznej.</span><span class="sxs-lookup"><span data-stu-id="17e3f-108">How to generate a map based on latitude and longitude coordinates.</span></span>
> - <span data-ttu-id="17e3f-109">Jak zarejestrować konto dewelopera map Bing i uzyskać klucz do użycia z mapami Bing.</span><span class="sxs-lookup"><span data-stu-id="17e3f-109">How to register a Bing Maps Developer Account and get a key to use with Bing Maps.</span></span>
> 
> <span data-ttu-id="17e3f-110">Jest to funkcja ASP.NET wprowadzona w artykule:</span><span class="sxs-lookup"><span data-stu-id="17e3f-110">This is the ASP.NET feature introduced in the article:</span></span>
> 
> - <span data-ttu-id="17e3f-111">Pomocnik `Maps`.</span><span class="sxs-lookup"><span data-stu-id="17e3f-111">The `Maps` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="17e3f-112">Wersje oprogramowania używane w samouczku</span><span class="sxs-lookup"><span data-stu-id="17e3f-112">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="17e3f-113">ASP.NET strony sieci Web (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="17e3f-113">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="17e3f-114">WebMatrix 2</span><span class="sxs-lookup"><span data-stu-id="17e3f-114">WebMatrix 2</span></span>
>   
> 
> <span data-ttu-id="17e3f-115">Ten samouczek współpracuje również z programem WebMatrix 3.</span><span class="sxs-lookup"><span data-stu-id="17e3f-115">This tutorial also works with WebMatrix 3.</span></span>

<span data-ttu-id="17e3f-116">Na stronach sieci Web można wyświetlić mapy na stronie za pomocą pomocnika `Maps`.</span><span class="sxs-lookup"><span data-stu-id="17e3f-116">In Web Pages, you can display maps on a page by using `Maps` helper.</span></span> <span data-ttu-id="17e3f-117">Mapy można generować na podstawie adresu lub zestawu współrzędnych długości i szerokości geograficznej.</span><span class="sxs-lookup"><span data-stu-id="17e3f-117">You can generate maps based either on an address or on a set of longitude and latitude coordinates.</span></span> <span data-ttu-id="17e3f-118">Klasa `Maps` umożliwia wywoływanie popularnych aparatów mapy, w tym Bing, Google, MapQuest i Yahoo.</span><span class="sxs-lookup"><span data-stu-id="17e3f-118">The `Maps` class lets you call into popular map engines including Bing, Google, MapQuest, and Yahoo.</span></span>

<span data-ttu-id="17e3f-119">Kroki umożliwiające dodanie mapowania do strony są takie same, niezależnie od tego, które z nich są wywoływane.</span><span class="sxs-lookup"><span data-stu-id="17e3f-119">The steps for adding mapping to a page are the same regardless of which of the map engines you call.</span></span> <span data-ttu-id="17e3f-120">Wystarczy dodać odwołanie do pliku JavaScript, które udostępnia metody dostępne do wyświetlania mapy, a następnie wywołuje metody pomocnika `Maps`.</span><span class="sxs-lookup"><span data-stu-id="17e3f-120">You just add a JavaScript file reference that makes available methods to display the map, and then you call methods of the `Maps` helper.</span></span>

<span data-ttu-id="17e3f-121">Wybierz usługę mapy opartą na tym, która `Maps` używana metoda pomocnika.</span><span class="sxs-lookup"><span data-stu-id="17e3f-121">You choose a map service based on which `Maps` helper method you use.</span></span> <span data-ttu-id="17e3f-122">Można użyć dowolnego z nich:</span><span class="sxs-lookup"><span data-stu-id="17e3f-122">You can use any of these:</span></span>

- `Maps.GetBingHtml`
- `Maps.GetGoogleHtml`
- `Maps.GetYahooHtml`
- `Maps.GetMapQuestHtml`

## <a name="installing-the-pieces-you-need"></a><span data-ttu-id="17e3f-123">Instalowanie potrzebnych fragmentów</span><span class="sxs-lookup"><span data-stu-id="17e3f-123">Installing the Pieces You Need</span></span>

<span data-ttu-id="17e3f-124">Aby wyświetlić mapy, potrzebne są następujące elementy:</span><span class="sxs-lookup"><span data-stu-id="17e3f-124">To display maps, you need these pieces:</span></span>

- <span data-ttu-id="17e3f-125">Pomocnik `Maps`.</span><span class="sxs-lookup"><span data-stu-id="17e3f-125">The `Maps` helper.</span></span> <span data-ttu-id="17e3f-126">Ten pomocnik znajduje się w wersji 2 biblioteki pomocników sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="17e3f-126">This helper is in version 2 of the ASP.NET Web Helpers Library.</span></span> <span data-ttu-id="17e3f-127">Jeśli biblioteka nie została jeszcze dodana, można ją zainstalować w swojej lokacji jako pakiet NuGet.</span><span class="sxs-lookup"><span data-stu-id="17e3f-127">If you haven't already added the library, you can install it in your site as a NuGet package.</span></span> <span data-ttu-id="17e3f-128">Aby uzyskać szczegółowe informacje, zobacz [Instalowanie pomocników w witrynie ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372).</span><span class="sxs-lookup"><span data-stu-id="17e3f-128">For details, see [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372).</span></span> <span data-ttu-id="17e3f-129">(W galerii Wyszukaj pakiet `microsoft-web-helpers`).</span><span class="sxs-lookup"><span data-stu-id="17e3f-129">(In the Gallery, search for the `microsoft-web-helpers` package.)</span></span>
- <span data-ttu-id="17e3f-130">Biblioteka jQuery.</span><span class="sxs-lookup"><span data-stu-id="17e3f-130">The jQuery library.</span></span> <span data-ttu-id="17e3f-131">Kilka szablonów witryn programu WebMatrix zawiera już biblioteki jQuery w ich folderach *skryptów* .</span><span class="sxs-lookup"><span data-stu-id="17e3f-131">Several of the WebMatrix site templates already include jQuery libraries in their *Script* folders.</span></span> <span data-ttu-id="17e3f-132">Jeśli nie masz tych bibliotek, możesz pobrać najnowszą bibliotekę jQuery bezpośrednio z witryny [jQuery.org](http://jQuery.org) .</span><span class="sxs-lookup"><span data-stu-id="17e3f-132">If you do not have these libraries, you can download the latest jQuery library directly from the [jQuery.org](http://jQuery.org) site.</span></span> <span data-ttu-id="17e3f-133">Można też utworzyć nową lokację przy użyciu szablonu (na przykład szablonu **witryny startowej** ), a następnie skopiować pliki jQuery z tej lokacji do bieżącej lokacji.</span><span class="sxs-lookup"><span data-stu-id="17e3f-133">Or you can create a new site using a template (for example, the **Starter Site** template) and then copy the jQuery files from that site to your current site.</span></span>

<span data-ttu-id="17e3f-134">Na koniec Jeśli chcesz korzystać z usługi mapy Bing, musisz najpierw utworzyć konto (bezpłatne) i uzyskać klucz.</span><span class="sxs-lookup"><span data-stu-id="17e3f-134">Finally, if you want to use Bing maps, you must first create a (free) account and get a key.</span></span> <span data-ttu-id="17e3f-135">Aby uzyskać klucz, wykonaj następujące kroki:</span><span class="sxs-lookup"><span data-stu-id="17e3f-135">To get a key, follow these steps:</span></span>

1. <span data-ttu-id="17e3f-136">Utwórz konto na [koncie dewelopera mapy usługi Bing](https://www.microsoft.com/maps/developers/web.aspx).</span><span class="sxs-lookup"><span data-stu-id="17e3f-136">Create an account on the [Bing Maps Developer Account](https://www.microsoft.com/maps/developers/web.aspx).</span></span> <span data-ttu-id="17e3f-137">Musisz również mieć konto Microsoft (identyfikator Windows Live).</span><span class="sxs-lookup"><span data-stu-id="17e3f-137">You must have a Microsoft account (Windows Live ID) as well.</span></span>

    <span data-ttu-id="17e3f-138">Możesz określić, że chcesz użyć klucza do **oceny/testowania**.</span><span class="sxs-lookup"><span data-stu-id="17e3f-138">You can specify that you want to use the key for **Evaluation/Test**.</span></span> <span data-ttu-id="17e3f-139">Jeśli testujesz funkcję mapowania na własnym komputerze przy użyciu programu WebMatrix i IIS Express, przejdź do obszaru roboczego **witryny** i ZANOTUJ adres URL witryny (na przykład `http://localhost:50408`, mimo że numer portu będzie prawdopodobnie inny).</span><span class="sxs-lookup"><span data-stu-id="17e3f-139">If you are testing the mapping function on your own computer using WebMatrix and IIS Express, go the **Site** workspace and note the URL of your site (for example, `http://localhost:50408`, although your port number will probably be different).</span></span> <span data-ttu-id="17e3f-140">Ten adres *localhost* można użyć jako witryny podczas rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="17e3f-140">You can use this *localhost* address as the site when you register.</span></span>
2. <span data-ttu-id="17e3f-141">Po zarejestrowaniu konta przejdź do centrum konta mapy Bing i kliknij pozycję **Utwórz lub Wyświetl klucze**:</span><span class="sxs-lookup"><span data-stu-id="17e3f-141">After you have registered for an account, go to the Bing Maps Account Center and click **Create or view keys**:</span></span>

    ![Mapowanie — 2](displaying-maps-in-an-aspnet-web-pages-site/_static/image1.png)
3. <span data-ttu-id="17e3f-143">Zapisz klucz, który tworzy Bing.</span><span class="sxs-lookup"><span data-stu-id="17e3f-143">Record the key that Bing creates.</span></span>

## <a name="creating-a-map-based-on-an-address-using-google"></a><span data-ttu-id="17e3f-144">Tworzenie mapy na podstawie adresu (przy użyciu usługi Google)</span><span class="sxs-lookup"><span data-stu-id="17e3f-144">Creating a Map Based on an Address (Using Google)</span></span>

<span data-ttu-id="17e3f-145">Poniższy przykład pokazuje, jak utworzyć stronę, która renderuje mapę na podstawie adresu.</span><span class="sxs-lookup"><span data-stu-id="17e3f-145">The following example shows how to create a page that renders a map based on an address.</span></span> <span data-ttu-id="17e3f-146">Ten przykład pokazuje, jak używać usługi Google Maps.</span><span class="sxs-lookup"><span data-stu-id="17e3f-146">This example shows how to use Google Maps.</span></span>

1. <span data-ttu-id="17e3f-147">Utwórz plik o nazwie *mapaddress. cshtml* w folderze głównym witryny.</span><span class="sxs-lookup"><span data-stu-id="17e3f-147">Create a file named *MapAddress.cshtml* in the root of the site.</span></span> <span data-ttu-id="17e3f-148">Ta strona generuje mapę na podstawie przekazanego adresu.</span><span class="sxs-lookup"><span data-stu-id="17e3f-148">This page will generate a map based on an address that you pass to it.</span></span>
2. <span data-ttu-id="17e3f-149">Skopiuj następujący kod do pliku, zastępując istniejącą zawartość.</span><span class="sxs-lookup"><span data-stu-id="17e3f-149">Copy the following code into the file, overwriting the existing content.</span></span>

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    <span data-ttu-id="17e3f-150">Zwróć uwagę na następujące funkcje strony:</span><span class="sxs-lookup"><span data-stu-id="17e3f-150">Notice the following features of the page:</span></span>

    - <span data-ttu-id="17e3f-151">Element `<script>` w elemencie `<head>`.</span><span class="sxs-lookup"><span data-stu-id="17e3f-151">The `<script>` element in the `<head>` element.</span></span> <span data-ttu-id="17e3f-152">W tym przykładzie element `<script>` odwołuje się do pliku *jQuery-1.6.4. min. js* , który jest wersją zminimalizowanego (skompresowaną) biblioteki jQuery, w wersji 1.6.4.</span><span class="sxs-lookup"><span data-stu-id="17e3f-152">In the example, the `<script>` element references the *jquery-1.6.4.min.js* file, which is a minified (compressed) version of the jQuery library, version 1.6.4.</span></span> <span data-ttu-id="17e3f-153">Zwróć uwagę na to, że plik *js* znajduje się w folderze *skryptów* w witrynie.</span><span class="sxs-lookup"><span data-stu-id="17e3f-153">Note that the reference assumes that the *.js* file is in the *Scripts* folder of your site.</span></span> 

        > [!NOTE]
        > <span data-ttu-id="17e3f-154">Jeśli używasz innej wersji biblioteki jQuery, upewnij się, że jest ona poprawna.</span><span class="sxs-lookup"><span data-stu-id="17e3f-154">If you're using a different version of the jQuery library, just make sure that you're pointing to that version correctly.</span></span>
    - <span data-ttu-id="17e3f-155">Wywołanie `@Maps.GetGoogleHtml` w treści strony.</span><span class="sxs-lookup"><span data-stu-id="17e3f-155">The call to the `@Maps.GetGoogleHtml` in the body of the page.</span></span> <span data-ttu-id="17e3f-156">Aby zmapować adres, należy przekazać ciąg adresu.</span><span class="sxs-lookup"><span data-stu-id="17e3f-156">To map an address, you must pass an address string.</span></span> <span data-ttu-id="17e3f-157">Metody dla innych aparatów mapy działają w podobny sposób (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).</span><span class="sxs-lookup"><span data-stu-id="17e3f-157">The methods for the other map engines work in a similar way (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).</span></span>
3. <span data-ttu-id="17e3f-158">Uruchom stronę i wprowadź adres.</span><span class="sxs-lookup"><span data-stu-id="17e3f-158">Run the page and enter an address.</span></span> <span data-ttu-id="17e3f-159">Na stronie zostanie wyświetlona mapa oparta na usłudze mapy Google, która zawiera określoną lokalizację.</span><span class="sxs-lookup"><span data-stu-id="17e3f-159">The page displays a map, based on Google Maps, that shows the location that you specified.</span></span>

     ![mapowanie — 1](displaying-maps-in-an-aspnet-web-pages-site/_static/image2.png)

## <a name="creating-a-map-based-on-latitude-and-longitude-coordinates-using-bing"></a><span data-ttu-id="17e3f-161">Tworzenie mapy na podstawie współrzędnych szerokości i długości geograficznej (przy użyciu usługi Bing)</span><span class="sxs-lookup"><span data-stu-id="17e3f-161">Creating a Map Based on Latitude and Longitude Coordinates (Using Bing)</span></span>

<span data-ttu-id="17e3f-162">Ten przykład pokazuje, jak utworzyć mapę na podstawie współrzędnych.</span><span class="sxs-lookup"><span data-stu-id="17e3f-162">This example shows how to create a map based on coordinates.</span></span> <span data-ttu-id="17e3f-163">Ten przykład pokazuje, jak używać map Bing i jak uwzględnić klucz Bing.</span><span class="sxs-lookup"><span data-stu-id="17e3f-163">This example shows how to use Bing maps and how to include your Bing key.</span></span> <span data-ttu-id="17e3f-164">(Mapę można utworzyć na podstawie współrzędnych przy użyciu innych aparatów mapy również, bez użycia klucza Bing).</span><span class="sxs-lookup"><span data-stu-id="17e3f-164">(You can create a map based on coordinates using the other map engines also, without using a Bing key.)</span></span>

1. <span data-ttu-id="17e3f-165">Utwórz plik o nazwie *MapCoordinates. cshtml* w folderze głównym lokacji i Zastąp istniejącą zawartość następującym kodem i znacznikiem:</span><span class="sxs-lookup"><span data-stu-id="17e3f-165">Create a file named *MapCoordinates.cshtml* in the root of the site and replace the existing content with the following code and markup:</span></span>

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample2.cshtml)]
2. <span data-ttu-id="17e3f-166">Zamień `your-key-here` na wygenerowany wcześniej klucz usługi mapy Bing.</span><span class="sxs-lookup"><span data-stu-id="17e3f-166">Replace `your-key-here` with the Bing Maps key that you generated earlier.</span></span>
3. <span data-ttu-id="17e3f-167">Uruchom stronę *MapCoordinates. cshtml* , wprowadź współrzędne szerokości geograficznej i długości geograficznej, a następnie kliknij przycisk **Mapa!**</span><span class="sxs-lookup"><span data-stu-id="17e3f-167">Run the *MapCoordinates.cshtml* page, enter latitude and longitude coordinates, and then click the **Map It!**</span></span> <span data-ttu-id="17e3f-168">Dodaj...</span><span class="sxs-lookup"><span data-stu-id="17e3f-168">button.</span></span> <span data-ttu-id="17e3f-169">(Jeśli nie znasz żadnych współrzędnych, spróbuj wykonać poniższe czynności.</span><span class="sxs-lookup"><span data-stu-id="17e3f-169">(If you don't know any coordinates, try the following.</span></span> <span data-ttu-id="17e3f-170">Jest to lokalizacja w ramach kampusu Microsoft Redmond.)</span><span class="sxs-lookup"><span data-stu-id="17e3f-170">This is a location on the Microsoft Redmond campus.)</span></span>

   - <span data-ttu-id="17e3f-171">Szerokość geograficzna: 47.6781005859375</span><span class="sxs-lookup"><span data-stu-id="17e3f-171">Latitude: 47.6781005859375</span></span>
   - <span data-ttu-id="17e3f-172">Longitude: -122.158317565918</span><span class="sxs-lookup"><span data-stu-id="17e3f-172">Longitude: -122.158317565918</span></span>

     <span data-ttu-id="17e3f-173">Strona zostanie wyświetlona przy użyciu określonych współrzędnych.</span><span class="sxs-lookup"><span data-stu-id="17e3f-173">The page is displayed using the coordinates that you specified.</span></span>

     ![Mapowanie — 3](displaying-maps-in-an-aspnet-web-pages-site/_static/image3.png)

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="17e3f-175">Dodatkowe materiały</span><span class="sxs-lookup"><span data-stu-id="17e3f-175">Additional Resources</span></span>

[<span data-ttu-id="17e3f-176">Dokumentacja interfejsu API Microsoft. Maps</span><span class="sxs-lookup"><span data-stu-id="17e3f-176">Microsoft.Maps API Reference</span></span>](https://msdn.microsoft.com/library/gg427611.aspx)

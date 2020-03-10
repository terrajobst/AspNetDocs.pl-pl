---
uid: web-pages/overview/ui-layouts-and-themes/10-working-with-video
title: Wyświetlanie wideo w witrynie ASP.NET Web Pages (Razor) | Microsoft Docs
author: Rick-Anderson
description: W tym rozdziale wyjaśniono, jak wyświetlać wideo na stronach sieci Web ASP.NET za pomocą strony składnia Razor.
ms.author: riande
ms.date: 02/20/2014
ms.assetid: 332fb3da-e2a5-460d-bb90-dd911e1e2c95
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/10-working-with-video
msc.type: authoredcontent
ms.openlocfilehash: 516d46f38ce8910209f4207c474b0404bf012950
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78628948"
---
# <a name="displaying-video-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="c28b0-103">Wyświetlanie wideo w witrynie ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="c28b0-103">Displaying Video in an ASP.NET Web Pages (Razor) Site</span></span>

<span data-ttu-id="c28b0-104">Autor [FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="c28b0-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="c28b0-105">W tym artykule wyjaśniono, jak używać odtwarzacza wideo (multimediów) w witrynie internetowej ASP.NET Web Pages (Razor) w celu umożliwienia użytkownikom wyświetlania filmów wideo przechowywanych w witrynie.</span><span class="sxs-lookup"><span data-stu-id="c28b0-105">This article explains how to use a video (media) player in an ASP.NET Web Pages (Razor) website to let users view videos that are stored on the site.</span></span> <span data-ttu-id="c28b0-106">ASP.NET Web Pages with składnia Razor umożliwia odtwarzanie filmów Flash (*SWF*), Media Player (*WMV*) i Silverlight (*XAP*).</span><span class="sxs-lookup"><span data-stu-id="c28b0-106">ASP.NET Web Pages with Razor syntax lets you play Flash (*.swf*), Media Player (*.wmv*), and Silverlight (*.xap*) videos.</span></span>
> 
> <span data-ttu-id="c28b0-107">Zawartość:</span><span class="sxs-lookup"><span data-stu-id="c28b0-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="c28b0-108">Jak wybrać odtwarzacz wideo.</span><span class="sxs-lookup"><span data-stu-id="c28b0-108">How to choose a video player.</span></span>
> - <span data-ttu-id="c28b0-109">Jak dodać wideo do strony sieci Web.</span><span class="sxs-lookup"><span data-stu-id="c28b0-109">How to add video to a web page.</span></span>
> - <span data-ttu-id="c28b0-110">Jak ustawić atrybuty odtwarzacza wideo.</span><span class="sxs-lookup"><span data-stu-id="c28b0-110">How to set video player attributes.</span></span>
> 
> <span data-ttu-id="c28b0-111">Są to funkcje stron Razor ASP.NET wprowadzone w artykule:</span><span class="sxs-lookup"><span data-stu-id="c28b0-111">These are the ASP.NET Razor pages features introduced in the article:</span></span>
> 
> - <span data-ttu-id="c28b0-112">Pomocnik `Video`.</span><span class="sxs-lookup"><span data-stu-id="c28b0-112">The `Video` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="c28b0-113">Wersje oprogramowania używane w samouczku</span><span class="sxs-lookup"><span data-stu-id="c28b0-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="c28b0-114">ASP.NET strony sieci Web (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="c28b0-114">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="c28b0-115">WebMatrix 2</span><span class="sxs-lookup"><span data-stu-id="c28b0-115">WebMatrix 2</span></span>
>   
> 
> <span data-ttu-id="c28b0-116">Ten samouczek współpracuje również z programem WebMatrix 3.</span><span class="sxs-lookup"><span data-stu-id="c28b0-116">This tutorial also works with WebMatrix 3.</span></span>

## <a name="introduction"></a><span data-ttu-id="c28b0-117">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="c28b0-117">Introduction</span></span>

<span data-ttu-id="c28b0-118">Możesz chcieć wyświetlić wideo w swojej witrynie.</span><span class="sxs-lookup"><span data-stu-id="c28b0-118">You might want to display a video on your site.</span></span> <span data-ttu-id="c28b0-119">Jednym ze sposobów jest utworzenie linku do witryny, która zawiera już wideo, takich jak YouTube.</span><span class="sxs-lookup"><span data-stu-id="c28b0-119">One way to do that is to link to a site that already has the video, like YouTube.</span></span> <span data-ttu-id="c28b0-120">Jeśli chcesz osadzić wideo z tych witryn bezpośrednio na własnych stronach, możesz zwykle pobrać znacznik HTML z witryny, a następnie skopiować go na stronę.</span><span class="sxs-lookup"><span data-stu-id="c28b0-120">If you want to embed a video from these sites directly in your own pages, you can usually get HTML markup from the site and then copy it into your page.</span></span> <span data-ttu-id="c28b0-121">Na przykład poniższy przykład pokazuje, jak osadzić wideo w serwisie YouTube:</span><span class="sxs-lookup"><span data-stu-id="c28b0-121">For example, the following example shows how to embed a YouTube video:</span></span>

[!code-html[Main](10-working-with-video/samples/sample1.html?highlight=10-14)]

<span data-ttu-id="c28b0-122">Jeśli chcesz odtworzyć wideo znajdujące się we własnej witrynie sieci Web (nie w publicznej witrynie udostępniania wideo), nie możesz bezpośrednio połączyć się z nią przy użyciu osadzonych znaczników, takich jak ten.</span><span class="sxs-lookup"><span data-stu-id="c28b0-122">If you want to play a video that's on your own website (not on a public video-sharing site), you can't directly link to it using embedded markup like this.</span></span> <span data-ttu-id="c28b0-123">Można jednak odtwarzać wideo z witryny przy użyciu pomocnika `Video`, który renderuje odtwarzacz multimedialny bezpośrednio na stronie.</span><span class="sxs-lookup"><span data-stu-id="c28b0-123">However, you can play videos from your site by using the `Video` helper, which renders a media player directly in a page.</span></span>

<a id="Choosing_a_Video_Player"></a>
## <a name="choosing-a-video-player"></a><span data-ttu-id="c28b0-124">Wybieranie odtwarzacza wideo</span><span class="sxs-lookup"><span data-stu-id="c28b0-124">Choosing a Video Player</span></span>

<span data-ttu-id="c28b0-125">Istnieje wiele formatów plików wideo, a każdy format zazwyczaj wymaga innego odtwarzacza i innego sposobu konfigurowania odtwarzacza.</span><span class="sxs-lookup"><span data-stu-id="c28b0-125">There are lots of formats for video files, and each format typically requires a different player and a different way to configure the player.</span></span> <span data-ttu-id="c28b0-126">Na stronach ASP.NET Razor można odtwarzać wideo na stronie sieci Web przy użyciu pomocnika `Video`.</span><span class="sxs-lookup"><span data-stu-id="c28b0-126">In ASP.NET Razor pages, you can play a video in a web page using the `Video` helper.</span></span> <span data-ttu-id="c28b0-127">Pomocnik `Video` upraszcza proces osadzania wideo na stronie sieci Web, ponieważ automatycznie generuje `object` i `embed` elementy HTML, które są zwykle używane do dodawania wideo do strony.</span><span class="sxs-lookup"><span data-stu-id="c28b0-127">The `Video` helper simplifies the process of embedding videos in a web page because it automatically generates the `object` and `embed` HTML elements that are normally used to add video to the page.</span></span>

<span data-ttu-id="c28b0-128">Pomocnik `Video` obsługuje następujące odtwarzacze multimedialne:</span><span class="sxs-lookup"><span data-stu-id="c28b0-128">The `Video` helper supports the following media players:</span></span>

- <span data-ttu-id="c28b0-129">Adobe Flash</span><span class="sxs-lookup"><span data-stu-id="c28b0-129">Adobe Flash</span></span>
- <span data-ttu-id="c28b0-130">MediaPlayer systemu Windows</span><span class="sxs-lookup"><span data-stu-id="c28b0-130">Windows MediaPlayer</span></span>
- <span data-ttu-id="c28b0-131">Microsoft Silverlight</span><span class="sxs-lookup"><span data-stu-id="c28b0-131">Microsoft Silverlight</span></span>

### <a name="the-flash-player"></a><span data-ttu-id="c28b0-132">Program Flash Player</span><span class="sxs-lookup"><span data-stu-id="c28b0-132">The Flash Player</span></span>

<span data-ttu-id="c28b0-133">`Flash` Player pomocnika `Video` umożliwia odtwarzanie filmów wideo Flash (plików*SWF* ) na stronie sieci Web.</span><span class="sxs-lookup"><span data-stu-id="c28b0-133">The `Flash` player of the `Video` helper let you play Flash videos (*.swf* files) in a web page.</span></span> <span data-ttu-id="c28b0-134">Należy podać co najmniej ścieżkę do pliku wideo.</span><span class="sxs-lookup"><span data-stu-id="c28b0-134">At a minimum, you have to provide a path to the video file.</span></span> <span data-ttu-id="c28b0-135">Jeśli określisz Nothing, ale ścieżka, odtwarzacz użyje wartości domyślnych ustawionych przez bieżącą wersję programu Flash.</span><span class="sxs-lookup"><span data-stu-id="c28b0-135">If you specify nothing but the path, the player uses default values that are set by the current version of Flash.</span></span> <span data-ttu-id="c28b0-136">Typowe ustawienia domyślne to:</span><span class="sxs-lookup"><span data-stu-id="c28b0-136">Typical default settings are:</span></span>

- <span data-ttu-id="c28b0-137">Film wideo jest wyświetlany z domyślną szerokością i wysokością i bez koloru tła.</span><span class="sxs-lookup"><span data-stu-id="c28b0-137">The video is displayed using its default width and height and without a background color.</span></span>
- <span data-ttu-id="c28b0-138">Film wideo jest odtwarzany automatycznie po załadowaniu strony.</span><span class="sxs-lookup"><span data-stu-id="c28b0-138">The video plays automatically when the page loads.</span></span>
- <span data-ttu-id="c28b0-139">Film wideo jest ciągle pętli do momentu, gdy zostanie jawnie zatrzymany.</span><span class="sxs-lookup"><span data-stu-id="c28b0-139">The video loops continuously until it's explicitly stopped.</span></span>
- <span data-ttu-id="c28b0-140">Film wideo jest skalowany do wyświetlania całego wideo zamiast przycinania wideo w celu dopasowania go do określonego rozmiaru.</span><span class="sxs-lookup"><span data-stu-id="c28b0-140">The video is scaled to show all of the video, rather than cropping the video to fit a specific size.</span></span>
- <span data-ttu-id="c28b0-141">Film wideo jest odtwarzany w oknie.</span><span class="sxs-lookup"><span data-stu-id="c28b0-141">The video plays in a window.</span></span>

### <a name="the-mediaplayer-player"></a><span data-ttu-id="c28b0-142">Odtwarzacz MediaPlayer</span><span class="sxs-lookup"><span data-stu-id="c28b0-142">The MediaPlayer Player</span></span>

<span data-ttu-id="c28b0-143">`MediaPlayer` Player pomocnika `Video` umożliwia odtwarzanie filmów wideo w systemie Windows Media (pliki*WMV* ), audio systemu Windows Media (pliki *. WMA* ) oraz plików MP3 (*MP3* ) na stronie sieci Web.</span><span class="sxs-lookup"><span data-stu-id="c28b0-143">The `MediaPlayer` player of the `Video` helper lets you play Windows Media videos (*.wmv* files), Windows Media audio (*.wma* files), and MP3 (*.mp3* files) in a web page.</span></span> <span data-ttu-id="c28b0-144">Musisz dołączyć ścieżkę pliku multimedialnego do odtwarzania; wszystkie inne parametry są opcjonalne.</span><span class="sxs-lookup"><span data-stu-id="c28b0-144">You must include path of the media file to play; all other parameters are optional.</span></span> <span data-ttu-id="c28b0-145">W przypadku określenia tylko ścieżki odtwarzacz używa ustawień domyślnych ustawionych przez bieżącą wersję MediaPlayer, na przykład:</span><span class="sxs-lookup"><span data-stu-id="c28b0-145">If you specify only a path, the player uses default settings set by the current version of MediaPlayer, such as:</span></span>

- <span data-ttu-id="c28b0-146">Film wideo jest wyświetlany z domyślną szerokością i wysokością.</span><span class="sxs-lookup"><span data-stu-id="c28b0-146">The video is displayed using its default width and height.</span></span>
- <span data-ttu-id="c28b0-147">Film wideo jest odtwarzany automatycznie po załadowaniu strony.</span><span class="sxs-lookup"><span data-stu-id="c28b0-147">The video plays automatically when the page loads.</span></span>
- <span data-ttu-id="c28b0-148">Film wideo jest odtwarzany raz (pętla nie działa).</span><span class="sxs-lookup"><span data-stu-id="c28b0-148">The video plays once (it doesn't loop).</span></span>
- <span data-ttu-id="c28b0-149">W odtwarzaczu jest wyświetlany pełen zestaw kontrolek w interfejsie użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c28b0-149">The player displays the full set of controls in the user interface.</span></span>
- <span data-ttu-id="c28b0-150">Film wideo jest odtwarzany w oknie.</span><span class="sxs-lookup"><span data-stu-id="c28b0-150">The video plays in a window.</span></span>

### <a name="the-silverlight-player"></a><span data-ttu-id="c28b0-151">Odtwarzacz Silverlight</span><span class="sxs-lookup"><span data-stu-id="c28b0-151">The Silverlight Player</span></span>

<span data-ttu-id="c28b0-152">`Silverlight` Player pomocnika `Video` umożliwia odtwarzanie filmów Windows Media Video (*WMV* ), Windows Media audio (pliki *. WMA* ) oraz plików MP3 (*MP3* ).</span><span class="sxs-lookup"><span data-stu-id="c28b0-152">The `Silverlight` player of the `Video` helper lets you play Windows Media Video (*.wmv* files), Windows Media Audio (*.wma* files), and MP3 (*.mp3* files).</span></span> <span data-ttu-id="c28b0-153">Należy ustawić parametr path, aby wskazywał pakiet aplikacji oparty na technologii Silverlight (plik *. xap* ).</span><span class="sxs-lookup"><span data-stu-id="c28b0-153">You must set the path parameter to point to a Silverlight-based application package (*.xap* file).</span></span> <span data-ttu-id="c28b0-154">Należy również ustawić parametry width i height.</span><span class="sxs-lookup"><span data-stu-id="c28b0-154">You also must set the width and height parameters.</span></span> <span data-ttu-id="c28b0-155">Wszystkie inne parametry są opcjonalne.</span><span class="sxs-lookup"><span data-stu-id="c28b0-155">All other parameters are optional.</span></span> <span data-ttu-id="c28b0-156">Jeśli używasz odtwarzacza Silverlight do wideo, jeśli ustawisz tylko wymagane parametry, odtwarzacz Silverlight wyświetli film wideo bez koloru tła.</span><span class="sxs-lookup"><span data-stu-id="c28b0-156">When you use the Silverlight player for video, if you set only the required parameters, the Silverlight player displays the video without a background color.</span></span>

> [!NOTE]
> <span data-ttu-id="c28b0-157">Jeśli nie znasz jeszcze Silverlight: plik *XAP* jest skompresowanym plikiem, który zawiera instrukcje układu w pliku *XAML* , kod zarządzany w zestawach i zasoby opcjonalne.</span><span class="sxs-lookup"><span data-stu-id="c28b0-157">In case you don't already know Silverlight: the *.xap* file is a compressed file that contains layout instructions in a *.xaml* file, managed code in assemblies, and optional resources.</span></span> <span data-ttu-id="c28b0-158">Plik *XAP* można utworzyć w programie Visual Studio jako projekt aplikacji Silverlight.</span><span class="sxs-lookup"><span data-stu-id="c28b0-158">You can create a *.xap* file in Visual Studio as a Silverlight application project.</span></span>

<span data-ttu-id="c28b0-159">Odtwarzacz wideo `Silverlight` używa zarówno ustawień dostarczonych dla odtwarzacza, jak i ustawień, które są dostępne w pliku *XAP* .</span><span class="sxs-lookup"><span data-stu-id="c28b0-159">The `Silverlight` video player uses both the settings that you provide for the player and the settings that are provided in the *.xap* file.</span></span>

> [!TIP] 
> 
> <a id="SB_MimeTypes"></a>
> ### <a name="mime-types"></a><span data-ttu-id="c28b0-160">Typy MIME</span><span class="sxs-lookup"><span data-stu-id="c28b0-160">MIME Types</span></span>
> 
> <span data-ttu-id="c28b0-161">Gdy przeglądarka pobiera plik, przeglądarka upewnia się, że typ pliku pasuje do typu MIME określonego dla renderowanego dokumentu.</span><span class="sxs-lookup"><span data-stu-id="c28b0-161">When a browser downloads a file, the browser makes sure that the file type matches the MIME type that's specified for the document that's being rendered.</span></span> <span data-ttu-id="c28b0-162">Typ MIME jest typem zawartości lub typem nośnika pliku.</span><span class="sxs-lookup"><span data-stu-id="c28b0-162">The MIME type is the content type or media type of a file.</span></span> <span data-ttu-id="c28b0-163">Pomocnik `Video` używa następujących typów MIME:</span><span class="sxs-lookup"><span data-stu-id="c28b0-163">The `Video` helper uses the following MIME types:</span></span>
> 
> - `application/x-shockwave-flash`
> - `application/x-mplayer2`
> - `application/x-silverlight-2`

<a id="Playing_Flash"></a>
## <a name="playing-flash-swf-videos"></a><span data-ttu-id="c28b0-164">Odtwarzanie filmów wideo z Flash (SWF)</span><span class="sxs-lookup"><span data-stu-id="c28b0-164">Playing Flash (.swf) Videos</span></span>

<span data-ttu-id="c28b0-165">Ta procedura pokazuje, jak odtworzyć wideo Flash o nazwie *Sample. swf*.</span><span class="sxs-lookup"><span data-stu-id="c28b0-165">This procedure shows you how to play a Flash video named *sample.swf*.</span></span> <span data-ttu-id="c28b0-166">W procedurze przyjęto założenie, że w witrynie znajduje się folder o nazwie *multimedia* , a plik *SWF* znajduje się w tym folderze.</span><span class="sxs-lookup"><span data-stu-id="c28b0-166">The procedure assumes that you've got a folder named *Media* on your site and that the *.swf* file is in that folder.</span></span>

1. <span data-ttu-id="c28b0-167">Dodaj bibliotekę pomocników sieci Web ASP.NET do witryny internetowej zgodnie z opisem w temacie [Instalowanie pomocników w witrynie ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), jeśli jeszcze nie została dodana.</span><span class="sxs-lookup"><span data-stu-id="c28b0-167">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already added it.</span></span>
2. <span data-ttu-id="c28b0-168">W witrynie sieci Web Dodaj stronę i nadaj jej nazwę *FlashVideo. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="c28b0-168">In the website, add a page and name it *FlashVideo.cshtml*.</span></span>
3. <span data-ttu-id="c28b0-169">Dodaj do strony następujący znacznik:</span><span class="sxs-lookup"><span data-stu-id="c28b0-169">Add the following markup to the page:</span></span> 

    [!code-cshtml[Main](10-working-with-video/samples/sample2.cshtml)]
4. <span data-ttu-id="c28b0-170">Uruchom stronę w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="c28b0-170">Run the page in a browser.</span></span> <span data-ttu-id="c28b0-171">(Upewnij się, że strona została wybrana w obszarze roboczym **pliki** przed jej uruchomieniem). Zostanie wyświetlona strona, a film wideo jest odtwarzany automatycznie.</span><span class="sxs-lookup"><span data-stu-id="c28b0-171">(Make sure the page is selected in the **Files** workspace before you run it.) The page is displayed and the video plays automatically.</span></span> 

    <span data-ttu-id="c28b0-172">![Image](10-working-with-video/_static/image1.jpg "ch08_video -1. jpg")</span><span class="sxs-lookup"><span data-stu-id="c28b0-172">![[image]](10-working-with-video/_static/image1.jpg "ch08_video-1.jpg")</span></span>

<span data-ttu-id="c28b0-173">Można ustawić `quality` parametru wideo Flash do `low`, `autolow`, `autohigh`, `medium`, `high`i `best`:</span><span class="sxs-lookup"><span data-stu-id="c28b0-173">You can set the `quality` parameter for a Flash video to `low`, `autolow`, `autohigh`, `medium`, `high`, and `best`:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample3.cshtml)]

<span data-ttu-id="c28b0-174">Możesz zmienić wideo Flash, aby odtworzyć o określonym rozmiarze przy użyciu parametru `scale`, który można ustawić w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="c28b0-174">You can change the Flash video to play at a specific size using the `scale` parameter, which you can set to the following:</span></span>

- <span data-ttu-id="c28b0-175">`showall`.</span><span class="sxs-lookup"><span data-stu-id="c28b0-175">`showall`.</span></span> <span data-ttu-id="c28b0-176">To sprawia, że całe wideo jest widoczne przy zachowaniu oryginalnego współczynnika proporcji.</span><span class="sxs-lookup"><span data-stu-id="c28b0-176">This makes the entire video visible while maintaining the original aspect ratio.</span></span> <span data-ttu-id="c28b0-177">Można jednak na każdej stronie zakończyło się obramowaniem.</span><span class="sxs-lookup"><span data-stu-id="c28b0-177">However, you might end up with borders on each side.</span></span>
- <span data-ttu-id="c28b0-178">`noorder`.</span><span class="sxs-lookup"><span data-stu-id="c28b0-178">`noorder`.</span></span> <span data-ttu-id="c28b0-179">To skaluje wideo przy zachowaniu oryginalnego współczynnika proporcji, ale może być przycięty.</span><span class="sxs-lookup"><span data-stu-id="c28b0-179">This scales the video while maintaining the original aspect ratio, but it might be cropped.</span></span>
- <span data-ttu-id="c28b0-180">`exactfit`.</span><span class="sxs-lookup"><span data-stu-id="c28b0-180">`exactfit`.</span></span> <span data-ttu-id="c28b0-181">Dzięki temu całe wideo jest widoczne bez zachowania oryginalnego współczynnika proporcji, ale może wystąpić zniekształcenie.</span><span class="sxs-lookup"><span data-stu-id="c28b0-181">This makes the entire video visible without preserving the original aspect ratio, but distortion may occur.</span></span>

<span data-ttu-id="c28b0-182">Jeśli nie określisz parametru `scale`, całe wideo będzie widoczne i oryginalny współczynnik proporcji będzie utrzymywany bez przycinania.</span><span class="sxs-lookup"><span data-stu-id="c28b0-182">If you don't specify a `scale` parameter, the entire video will be visible and the original aspect ratio will be maintained without any cropping.</span></span> <span data-ttu-id="c28b0-183">Poniższy przykład pokazuje, jak używać `scale` parametru:</span><span class="sxs-lookup"><span data-stu-id="c28b0-183">The following example shows how to use the `scale` parameter:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample4.cshtml)]

<span data-ttu-id="c28b0-184">Program Flash Player obsługuje ustawienie trybu wideo o nazwie `windowMode`.</span><span class="sxs-lookup"><span data-stu-id="c28b0-184">The Flash player supports a video mode setting named `windowMode`.</span></span> <span data-ttu-id="c28b0-185">Możesz ustawić tę wartość na `window`, `opaque`i `transparent`.</span><span class="sxs-lookup"><span data-stu-id="c28b0-185">You can set this to `window`, `opaque`, and `transparent`.</span></span> <span data-ttu-id="c28b0-186">Domyślnie `windowMode` jest ustawiona na `window`, co spowoduje wyświetlenie wideo w osobnym oknie na stronie sieci Web.</span><span class="sxs-lookup"><span data-stu-id="c28b0-186">By default, the `windowMode` is set to `window`, which displays the video in a separate window on the web page.</span></span> <span data-ttu-id="c28b0-187">Ustawienie `opaque` ukrywa wszystko za wideo na stronie sieci Web.</span><span class="sxs-lookup"><span data-stu-id="c28b0-187">The `opaque` setting hides everything behind the video on the web page.</span></span> <span data-ttu-id="c28b0-188">Ustawienie `transparent` umożliwia tło strony sieci Web wyświetlanej przez wideo, przy założeniu, że jakakolwiek część filmu wideo jest niewidoczna.</span><span class="sxs-lookup"><span data-stu-id="c28b0-188">The `transparent` setting lets the background of the web page show through the video, assuming any part of the video is transparent.</span></span>

<a id="Playing_MediaPlayer"></a>
## <a name="playing-mediaplayer-wmv-videos"></a><span data-ttu-id="c28b0-189">Odtwarzanie filmów wideo z MediaPlayer ( *. wmv*)</span><span class="sxs-lookup"><span data-stu-id="c28b0-189">Playing MediaPlayer (*.wmv*) Videos</span></span>

<span data-ttu-id="c28b0-190">Poniższa procedura pokazuje, jak odtworzyć film wideo z systemem Windows o nazwie *Sample. wmv* , który znajduje się w folderze *Media* .</span><span class="sxs-lookup"><span data-stu-id="c28b0-190">The following procedure shows you how to play a Window Media video named *sample.wmv* that's in the *Media* folder.</span></span>

1. <span data-ttu-id="c28b0-191">Dodaj bibliotekę pomocników sieci Web ASP.NET do witryny sieci Web zgodnie z opisem w temacie [Instalowanie pomocników w witrynie ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372)(jeśli jeszcze nie zostało to zrobione).</span><span class="sxs-lookup"><span data-stu-id="c28b0-191">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already.</span></span>
2. <span data-ttu-id="c28b0-192">Utwórz nową stronę o nazwie *MediaPlayerVideo. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="c28b0-192">Create a new page named *MediaPlayerVideo.cshtml*.</span></span>
3. <span data-ttu-id="c28b0-193">Dodaj do strony następujący znacznik:</span><span class="sxs-lookup"><span data-stu-id="c28b0-193">Add the following markup to the page:</span></span> 

    [!code-cshtml[Main](10-working-with-video/samples/sample5.cshtml)]
4. <span data-ttu-id="c28b0-194">Uruchom stronę w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="c28b0-194">Run the page in a browser.</span></span> <span data-ttu-id="c28b0-195">Film wideo jest ładowany i odtwarzany automatycznie.</span><span class="sxs-lookup"><span data-stu-id="c28b0-195">The video loads and plays automatically.</span></span> 

    <span data-ttu-id="c28b0-196">![Image](10-working-with-video/_static/image2.jpg "ch08_video -2. jpg")</span><span class="sxs-lookup"><span data-stu-id="c28b0-196">![[image]](10-working-with-video/_static/image2.jpg "ch08_video-2.jpg")</span></span>

<span data-ttu-id="c28b0-197">Można ustawić `playCount` na liczbę całkowitą, która wskazuje, ile razy ma być odtwarzane wideo:</span><span class="sxs-lookup"><span data-stu-id="c28b0-197">You can set `playCount` to an integer that indicates how many times to play the video automatically:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample6.cshtml)]

<span data-ttu-id="c28b0-198">Parametr `uiMode` pozwala określić, które kontrolki będą wyświetlane w interfejsie użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c28b0-198">The `uiMode` parameter lets you specify which controls show up in the user interface.</span></span> <span data-ttu-id="c28b0-199">Można ustawić `uiMode` na `invisible`, `none`, `mini`lub `full`.</span><span class="sxs-lookup"><span data-stu-id="c28b0-199">You can set `uiMode` to `invisible`, `none`, `mini`, or `full`.</span></span> <span data-ttu-id="c28b0-200">Jeśli nie określisz parametru `uiMode`, film wideo będzie wyświetlany z oknem stanu, paskiem wyszukiwania, przyciskami sterowania i kontrolkami głośności obok okna wideo.</span><span class="sxs-lookup"><span data-stu-id="c28b0-200">If you don't specify a `uiMode` parameter, the video will be displayed with the status window, seek bar, control buttons, and volume controls in addition to the video window.</span></span> <span data-ttu-id="c28b0-201">Te kontrolki zostaną również wyświetlone, jeśli używasz odtwarzacza do odtwarzania pliku dźwiękowego.</span><span class="sxs-lookup"><span data-stu-id="c28b0-201">These controls will also be displayed if you use the player to play an audio file.</span></span> <span data-ttu-id="c28b0-202">Oto przykład sposobu użycia `uiMode` parametru:</span><span class="sxs-lookup"><span data-stu-id="c28b0-202">Here's an example of how to use the `uiMode` parameter:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample7.cshtml)]

<span data-ttu-id="c28b0-203">Domyślnie dźwięk jest włączony, gdy wideo jest odtwarzane.</span><span class="sxs-lookup"><span data-stu-id="c28b0-203">By default, audio is on when the video plays.</span></span> <span data-ttu-id="c28b0-204">Możesz wyciszyć dźwięk, ustawiając parametr `mute` na true:</span><span class="sxs-lookup"><span data-stu-id="c28b0-204">You can mute the audio by setting the `mute` parameter to true:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample8.cshtml)]

<span data-ttu-id="c28b0-205">Możesz kontrolować poziom audio wideo MediaPlayer, ustawiając parametr `volume` na wartość z zakresu od 0 do 100.</span><span class="sxs-lookup"><span data-stu-id="c28b0-205">You can control the audio level of the MediaPlayer video by setting the `volume` parameter to a value between 0 and 100.</span></span> <span data-ttu-id="c28b0-206">Wartość domyślna to 50.</span><span class="sxs-lookup"><span data-stu-id="c28b0-206">The default value is 50.</span></span> <span data-ttu-id="c28b0-207">Oto przykład:</span><span class="sxs-lookup"><span data-stu-id="c28b0-207">Here's an example:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample9.cshtml)]

<a id="Playing_Silverlight"></a>
## <a name="playing-silverlight-videos"></a><span data-ttu-id="c28b0-208">Odtwarzanie filmów wideo Silverlight</span><span class="sxs-lookup"><span data-stu-id="c28b0-208">Playing Silverlight Videos</span></span>

<span data-ttu-id="c28b0-209">Ta procedura pokazuje, jak odtworzyć wideo zawarte na stronie Silverlight *. xap* , która znajduje się w folderze o nazwie *multimedia*.</span><span class="sxs-lookup"><span data-stu-id="c28b0-209">This procedure shows you how to play video contained in a Silverlight *.xap* page that's in a folder named *Media*.</span></span>

1. <span data-ttu-id="c28b0-210">Dodaj bibliotekę pomocników sieci Web ASP.NET do witryny sieci Web zgodnie z opisem w temacie [Instalowanie pomocników w witrynie ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372)(jeśli jeszcze nie zostało to zrobione).</span><span class="sxs-lookup"><span data-stu-id="c28b0-210">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already .</span></span>
2. <span data-ttu-id="c28b0-211">Utwórz nową stronę o nazwie *SilverlightVideo. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="c28b0-211">Create a new page named *SilverlightVideo.cshtml*.</span></span>
3. <span data-ttu-id="c28b0-212">Dodaj do strony następujący znacznik:</span><span class="sxs-lookup"><span data-stu-id="c28b0-212">Add the following markup to the page:</span></span> 

    [!code-cshtml[Main](10-working-with-video/samples/sample10.cshtml)]
4. <span data-ttu-id="c28b0-213">Uruchom stronę w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="c28b0-213">Run the page in a browser.</span></span> 

    <span data-ttu-id="c28b0-214">![Image](10-working-with-video/_static/image3.jpg "ch08_video -3. jpg")</span><span class="sxs-lookup"><span data-stu-id="c28b0-214">![[image]](10-working-with-video/_static/image3.jpg "ch08_video-3.jpg")</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="c28b0-215">Dodatkowe materiały</span><span class="sxs-lookup"><span data-stu-id="c28b0-215">Additional Resources</span></span>

<span data-ttu-id="c28b0-216">[Silverlight — Omówienie](https://msdn.microsoft.com/library/bb404700(VS.95).aspx)</span><span class="sxs-lookup"><span data-stu-id="c28b0-216">[Silverlight Overview](https://msdn.microsoft.com/library/bb404700(VS.95).aspx)</span></span>

[<span data-ttu-id="c28b0-217">Atrybuty obiektu Flash i OSADZONEgo znacznika</span><span class="sxs-lookup"><span data-stu-id="c28b0-217">Flash OBJECT and EMBED tag attributes</span></span>](http://kb2.adobe.com/cps/127/tn_12701.html)

<span data-ttu-id="c28b0-218">[Tagi PARAM zestawu SDK systemu Windows Media Player 11](https://msdn.microsoft.com/library/aa392321(VS.85).aspx)</span><span class="sxs-lookup"><span data-stu-id="c28b0-218">[Windows Media Player 11 SDK PARAM Tags](https://msdn.microsoft.com/library/aa392321(VS.85).aspx)</span></span>

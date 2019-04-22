---
uid: ajax/cdn/overview
title: Microsoft Ajax Content Delivery Network | Dokumentacja firmy Microsoft
author: rick-anderson
description: ''
ms.author: riande
ms.date: 10/14/2017
ms.assetid: 8935bf14-ca6d-4a4e-9dbe-b96ce74cef49
msc.legacyurl: /ajax/cdn
msc.type: content
ms.openlocfilehash: 6cea53021ce92e3936b06481008a86dd0590a117
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/17/2019
ms.locfileid: "59387441"
---
# <a name="microsoft-ajax-content-delivery-network"></a><span data-ttu-id="4f2c6-102">Usługa Microsoft AJAX Content Delivery Network</span><span class="sxs-lookup"><span data-stu-id="4f2c6-102">Microsoft Ajax Content Delivery Network</span></span>

> [!WARNING]
> <span data-ttu-id="4f2c6-103">Aplikacje produkcyjne nie powinna przyjmować twardych zależności zasobów w sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="4f2c6-103">Production applications should not take a hard dependency on CDN assets.</span></span> <span data-ttu-id="4f2c6-104">Aplikacje powinny testu dla zasobu usługi CDN, do których odwołuje się i użyj zasób rezerwowy, gdy sieć CDN jest niedostępny.</span><span class="sxs-lookup"><span data-stu-id="4f2c6-104">Applications should test for the CDN asset referenced, and use a fallback asset when the CDN is not available.</span></span>
>
> <span data-ttu-id="4f2c6-105">Microsoft Ajax CDN jest objęta umową SLA niż przy użyciu usługi Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="4f2c6-105">The Microsoft Ajax CDN has no SLA above and beyond using an Azure CDN.</span></span>
>
> <span data-ttu-id="4f2c6-106">Użyj [problem w usłudze GitHub](https://github.com/aspnet/AspNetDocs/issues/116) na zgłaszanie problemów z Microsoft Ajax CDN.</span><span class="sxs-lookup"><span data-stu-id="4f2c6-106">Use [this GitHub issue](https://github.com/aspnet/AspNetDocs/issues/116) to report problems with the Microsoft Ajax CDN.</span></span>

## <a name="table-of-contents"></a><span data-ttu-id="4f2c6-107">Spis treści</span><span class="sxs-lookup"><span data-stu-id="4f2c6-107">Table of Contents</span></span>

<span data-ttu-id="4f2c6-108">**[zmieniona na ajax.aspnetcdn.com AJAX.microsoft.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span><span class="sxs-lookup"><span data-stu-id="4f2c6-108">**[ajax.microsoft.com renamed to ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span></span>  
<span data-ttu-id="4f2c6-109">**[Visual Studio .vsdoc Support](#Visual_Studio_vsdoc_Support_19)**</span><span class="sxs-lookup"><span data-stu-id="4f2c6-109">**[Visual Studio .vsdoc Support](#Visual_Studio_vsdoc_Support_19)**</span></span>  
<span data-ttu-id="4f2c6-110">**[Za pomocą kodu ASP.NET Ajax z sieci CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span><span class="sxs-lookup"><span data-stu-id="4f2c6-110">**[Using ASP.NET Ajax from the CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span></span>  
<span data-ttu-id="4f2c6-111">**[Przy użyciu jQuery z sieci CDN](#Using_jQuery_from_the_CDN_21)**</span><span class="sxs-lookup"><span data-stu-id="4f2c6-111">**[Using jQuery from the CDN](#Using_jQuery_from_the_CDN_21)**</span></span>  
<span data-ttu-id="4f2c6-112">**[Przy użyciu jQuery interfejsu użytkownika z usługi CDN](#Using_jQuery_UI_from_the_CDN_22)**</span><span class="sxs-lookup"><span data-stu-id="4f2c6-112">**[Using jQuery UI from the CDN](#Using_jQuery_UI_from_the_CDN_22)**</span></span>  
<span data-ttu-id="4f2c6-113">**[Pliki innych firm w usłudze CDN](#Third-Party_Files_on_the_CDN_23)**</span><span class="sxs-lookup"><span data-stu-id="4f2c6-113">**[Third-Party Files on the CDN](#Third-Party_Files_on_the_CDN_23)**</span></span>  
  
 [<span data-ttu-id="4f2c6-114">Wersje dostępne w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="4f2c6-114">jQuery Releases on the CDN</span></span>](#jQuery_Releases_on_the_CDN_0)  
 [<span data-ttu-id="4f2c6-115">Wersje jQuery migracji w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="4f2c6-115">jQuery Migrate Releases on the CDN</span></span>](#jQuery_Migrate_Releases_on_the_CDN_1)  
 [<span data-ttu-id="4f2c6-116">jQuery wersjach interfejsu użytkownika w usłudze CDN</span><span class="sxs-lookup"><span data-stu-id="4f2c6-116">jQuery UI Releases on the CDN</span></span>](#jQuery_UI_Releases_on_the_CDN_2)  
 [<span data-ttu-id="4f2c6-117">Sprawdzanie poprawności wersji w usłudze CDN technologii jQuery</span><span class="sxs-lookup"><span data-stu-id="4f2c6-117">jQuery Validation Releases on the CDN</span></span>](#jQuery_Validation_Releases_on_the_CDN_3)  
 [<span data-ttu-id="4f2c6-118">jQuery Mobile wersji w usłudze CDN</span><span class="sxs-lookup"><span data-stu-id="4f2c6-118">jQuery Mobile Releases on the CDN</span></span>](#jQuery_Mobile_Releases_on_the_CDN_4)  
 [<span data-ttu-id="4f2c6-119">jQuery szablony wydań w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="4f2c6-119">jQuery Templates Releases on the CDN</span></span>](#jQuery_Templates_Releases_on_the_CDN_5)  
 [<span data-ttu-id="4f2c6-120">jQuery cyklu wersji w usłudze CDN</span><span class="sxs-lookup"><span data-stu-id="4f2c6-120">jQuery Cycle Releases on the CDN</span></span>](#jQuery_Cycle_Releases_on_the_CDN_6)  
 [<span data-ttu-id="4f2c6-121">jQuery wersji DataTable w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="4f2c6-121">jQuery DataTables Releases on the CDN</span></span>](#jQuery_DataTables_Releases_on_the_CDN_7)  
 [<span data-ttu-id="4f2c6-122">Wersje Modernizr w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="4f2c6-122">Modernizr Releases on the CDN</span></span>](#Modernizr_Releases_on_the_CDN_8)  
 [<span data-ttu-id="4f2c6-123">Wersje JSHint w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="4f2c6-123">JSHint Releases on the CDN</span></span>](#JSHint_Releases_on_the_CDN_10)  
 [<span data-ttu-id="4f2c6-124">Wersje knockout w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="4f2c6-124">Knockout Releases on the CDN</span></span>](#Knockout_Releases_on_the_CDN_11)  
 [<span data-ttu-id="4f2c6-125">Sprzedawać wersji w usłudze CDN</span><span class="sxs-lookup"><span data-stu-id="4f2c6-125">Globalize Releases on the CDN</span></span>](#Globalize_Releases_on_the_CDN_12)  
 [<span data-ttu-id="4f2c6-126">Odpowiadać wersji w usłudze CDN</span><span class="sxs-lookup"><span data-stu-id="4f2c6-126">Respond Releases on the CDN</span></span>](#Respond_Releases_on_the_CDN_13)  
 [<span data-ttu-id="4f2c6-127">Wersje ładowania w usłudze CDN</span><span class="sxs-lookup"><span data-stu-id="4f2c6-127">Bootstrap Releases on the CDN</span></span>](#Bootstrap_Releases_on_the_CDN_14)  
 [<span data-ttu-id="4f2c6-128">Wersje TouchCarousel ładowania w usłudze CDN</span><span class="sxs-lookup"><span data-stu-id="4f2c6-128">Bootstrap TouchCarousel Releases on the CDN</span></span>](#BootstrapTouchCarousel_Releases_on_the_CDN_18)  
 [<span data-ttu-id="4f2c6-129">Wersje hammer.js w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="4f2c6-129">Hammer.js Releases on the CDN</span></span>](#Hammerjs_Releases_on_the_CDN_19)  
 [<span data-ttu-id="4f2c6-130">ASP.NET Web Forms i Ajax wersji w usłudze CDN</span><span class="sxs-lookup"><span data-stu-id="4f2c6-130">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>](#ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15)  
 [<span data-ttu-id="4f2c6-131">ASP.NET MVC udostępnia w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="4f2c6-131">ASP.NET MVC Releases on the CDN</span></span>](#ASPNET_MVC_Releases_on_the_CDN_16)  
 [<span data-ttu-id="4f2c6-132">Wersje biblioteki SignalR platformy ASP.NET, w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="4f2c6-132">ASP.NET SignalR Releases on the CDN</span></span>](#ASPNET_SignalR_Releases_on_the_CDN_17)

<span data-ttu-id="4f2c6-133">Microsoft Ajax Content Delivery Network (CDN) obsługuje popularne innej biblioteki JavaScript, np. jQuery i pozwala na łatwe dodawanie ich do aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="4f2c6-133">The Microsoft Ajax Content Delivery Network (CDN) hosts popular third party JavaScript libraries such as jQuery and enables you to easily add them to your Web applications.</span></span> <span data-ttu-id="4f2c6-134">Na przykład, można uruchomić przy użyciu jQuery, który znajduje się w tej sieci CDN, poprzez dodanie &lt;skryptu&gt; tag do strony, który wskazuje na ajax.aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="4f2c6-134">For example, you can start using jQuery which is hosted on this CDN simply by adding a &lt;script&gt; tag to your page that points to ajax.aspnetcdn.com.</span></span>

<span data-ttu-id="4f2c6-135">Dzięki wykorzystaniu usługi CDN, można znacznie zwiększyć wydajność aplikacji w technologii Ajax.</span><span class="sxs-lookup"><span data-stu-id="4f2c6-135">By taking advantage of the CDN, you can significantly improve the performance of your Ajax applications.</span></span> <span data-ttu-id="4f2c6-136">Zawartość usługi CDN są buforowane na serwerach znajdujących się na całym świecie.</span><span class="sxs-lookup"><span data-stu-id="4f2c6-136">The contents of the CDN are cached on servers located around the world.</span></span> <span data-ttu-id="4f2c6-137">Ponadto sieci CDN umożliwia ponowne użycie plików JavaScript pamięci podręcznej innych firm dla witryn sieci web, które znajdują się w różnych domenach w różnych przeglądarkach.</span><span class="sxs-lookup"><span data-stu-id="4f2c6-137">In addition, the CDN enables browsers to reuse cached third party JavaScript files for web sites that are located in different domains.</span></span>

<span data-ttu-id="4f2c6-138">Usługa CDN obsługuje protokół SSL (HTTPS), w razie potrzeby do obsługi strony sieci web przy użyciu protokołu Secure Sockets Layer.</span><span class="sxs-lookup"><span data-stu-id="4f2c6-138">The CDN supports SSL (HTTPS) in case you need to serve a web page using the Secure Sockets Layer.</span></span>

<span data-ttu-id="4f2c6-139">Usługa CDN obsługuje następujące biblioteki skryptów innych firm, które zostały przekazane i są licencjonowane, właściciele tych bibliotek:</span><span class="sxs-lookup"><span data-stu-id="4f2c6-139">The CDN hosts the following third party script libraries which have been uploaded, and are licensed to you, by the owners of those libraries:</span></span>

- <span data-ttu-id="4f2c6-140">jQuery (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="4f2c6-140">jQuery (www.jquery.com)</span></span>
- <span data-ttu-id="4f2c6-141">jQuery UI (www.jqueryui.com)</span><span class="sxs-lookup"><span data-stu-id="4f2c6-141">jQuery UI (www.jqueryui.com)</span></span>
- <span data-ttu-id="4f2c6-142">jQuery Mobile (www.jquerymobile.com)</span><span class="sxs-lookup"><span data-stu-id="4f2c6-142">jQuery Mobile (www.jquerymobile.com)</span></span>
- <span data-ttu-id="4f2c6-143">jQuery sprawdzania poprawności (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="4f2c6-143">jQuery Validation (www.jquery.com)</span></span>
- <span data-ttu-id="4f2c6-144">jQuery Cycle (www.malsup.com/jquery/cycle/)</span><span class="sxs-lookup"><span data-stu-id="4f2c6-144">jQuery Cycle (www.malsup.com/jquery/cycle/)</span></span>
- <span data-ttu-id="4f2c6-145">jQuery DataTables (http://datatables.net/)</span><span class="sxs-lookup"><span data-stu-id="4f2c6-145">jQuery DataTables (http://datatables.net/)</span></span>

<span data-ttu-id="4f2c6-146">Microsoft Ajax CDN zawiera również następujące biblioteki, które zostały przekazane przez firmę Microsoft:</span><span class="sxs-lookup"><span data-stu-id="4f2c6-146">The Microsoft Ajax CDN also includes the following libraries which have been uploaded by Microsoft:</span></span>

- <span data-ttu-id="4f2c6-147">ASP.NET AJAX</span><span class="sxs-lookup"><span data-stu-id="4f2c6-147">ASP.NET Ajax</span></span>
- <span data-ttu-id="4f2c6-148">ASP.NET MVC JavaScript Files</span><span class="sxs-lookup"><span data-stu-id="4f2c6-148">ASP.NET MVC JavaScript Files</span></span>
- <span data-ttu-id="4f2c6-149">Pliki ASP.NET SignalR JavaScript</span><span class="sxs-lookup"><span data-stu-id="4f2c6-149">ASP.NET SignalR JavaScript Files</span></span>

<span data-ttu-id="4f2c6-150">Microsoft zastrzega sobie prawa własności do żadnych bibliotek innych firm, hostowanych w tej sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="4f2c6-150">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="4f2c6-151">Właściciele praw autorskich bibliotek korzystają z licencji tych bibliotek do Ciebie.</span><span class="sxs-lookup"><span data-stu-id="4f2c6-151">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="4f2c6-152">Wszelkie prawa, które może być konieczne pobranie i używanie biblioteki są przyznawane wyłącznie przez właścicieli praw autorskich.</span><span class="sxs-lookup"><span data-stu-id="4f2c6-152">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="4f2c6-153">Ponieważ nie są to biblioteki Microsoft, firma Microsoft oferuje żadnych gwarancji ani licencji praw własności intelektualnej (w tym nie domyślnych praw patentowych) bibliotek innych firm, hostowanych w tej sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="4f2c6-153">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<span data-ttu-id="4f2c6-154">Jeśli chcesz przesłać biblioteki JavaScript i biblioteka jest jednym z najważniejszych biblioteki JavaScript (zgodnie z opisem na http://trends.builtwith.com) lub rozszerzeń/wtyczek do tych bibliotek, które () popularnych; lub (b) przydatne do użytku na platformie ASP.NET, a następnie skontaktuj się z pomocą AjaxCDNSubmission@Microsoft.com.</span><span class="sxs-lookup"><span data-stu-id="4f2c6-154">If you wish to submit your JavaScript library and your library is one of the top JavaScript libraries (as listed on http://trends.builtwith.com) or extensions/plugins to these libraries that are (a) popular; or (b) helpful for use on ASP.NET then please contact AjaxCDNSubmission@Microsoft.com.</span></span>

<a id="ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18"></a>

## <a name="ajaxmicrosoftcom-renamed-to-ajaxaspnetcdncom"></a><span data-ttu-id="4f2c6-155">zmieniona na ajax.aspnetcdn.com AJAX.microsoft.com</span><span class="sxs-lookup"><span data-stu-id="4f2c6-155">ajax.microsoft.com renamed to ajax.aspnetcdn.com</span></span>

<span data-ttu-id="4f2c6-156">Sieć CDN używany do używania nazwy domeny w domenie microsoft.com i został zmieniony tak, aby użyć nazwy domeny aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="4f2c6-156">The CDN used to use the microsoft.com domain name and has been changed to use the aspnetcdn.com domain name.</span></span> <span data-ttu-id="4f2c6-157">Ta zmiana została wprowadzona w celu zwiększenia wydajności, ponieważ gdy przeglądarkę w domenie microsoft.com, do których odwołuje się on prześle pliki cookie z tej domeny faktycznie z każdym żądaniem.</span><span class="sxs-lookup"><span data-stu-id="4f2c6-157">This change was made to increase performance because when a browser referenced the microsoft.com domain it would send any cookies from that domain across the wire with each request.</span></span> <span data-ttu-id="4f2c6-158">Przez zmianę nazwy, aby wskazywała nazwę domeny inne niż microsoft.com można zwiększyć wydajność przez tyle 25%.</span><span class="sxs-lookup"><span data-stu-id="4f2c6-158">By renaming to a domain name other than microsoft.com performance can be increased by as much to 25%.</span></span> <span data-ttu-id="4f2c6-159">Należy pamiętać, ajax.microsoft.com będą nadal działać, ale ajax.aspnetcdn.com jest zalecane.</span><span class="sxs-lookup"><span data-stu-id="4f2c6-159">Note ajax.microsoft.com will continue to function but ajax.aspnetcdn.com is recommended.</span></span>

- <span data-ttu-id="4f2c6-160">Stary Format: https://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="4f2c6-160">Old Format: https://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span></span>
- <span data-ttu-id="4f2c6-161">Nowy Format: https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="4f2c6-161">New Format: https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span></span>

<a id="Visual_Studio_vsdoc_Support_19"></a>

## <a name="visual-studio-vsdoc-support"></a><span data-ttu-id="4f2c6-162">Obsługa .vsdoc programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4f2c6-162">Visual Studio .vsdoc Support</span></span>

<span data-ttu-id="4f2c6-163">Do korzystania z plików .vsdoc poprawnie za pomocą programu Visual Studio 2008, należy upewnić się, że program VS 2008 z dodatkiem SP1 należy zainstalować i zainstalowana poprawka vsdoc plików.</span><span class="sxs-lookup"><span data-stu-id="4f2c6-163">To use the .vsdoc files properly with Visual Studio 2008 you need to make sure that you have VS 2008 SP1 installed and the hotfix for vsdoc files installed.</span></span> <span data-ttu-id="4f2c6-164">Możesz uzyskać je w tym miejscu:</span><span class="sxs-lookup"><span data-stu-id="4f2c6-164">You can get these from here:</span></span>

- [<span data-ttu-id="4f2c6-165">Pobierz program Visual Studio 2008 z dodatkiem SP1</span><span class="sxs-lookup"><span data-stu-id="4f2c6-165">Download Visual Studio 2008 SP1</span></span>](https://www.microsoft.com/downloads/en/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en "Pobierz program Visual Studio 2008 z dodatkiem SP1")
- [<span data-ttu-id="4f2c6-166">Pobierz .vsdoc poprawki dla programu Visual Studio 2008 z dodatkiem SP1</span><span class="sxs-lookup"><span data-stu-id="4f2c6-166">Download .vsdoc hotfix for Visual Studio 2008 SP1</span></span>](https://code.msdn.microsoft.com/KB958502/Release/ProjectReleases.aspx?ReleaseId=1736 "Pobierz .vsdoc poprawki dla programu Visual Studio 2008 z dodatkiem SP1")

<span data-ttu-id="4f2c6-167">Program Visual Studio 2010 obsługuje pliki .vsdoc bez żadnych dodatkowych poprawek.</span><span class="sxs-lookup"><span data-stu-id="4f2c6-167">Visual Studio 2010 supports .vsdoc files without any additional patches.</span></span>

<a id="Using_ASPNET_Ajax_from_the_CDN_20"></a>

## <a name="using-aspnet-ajax-from-the-cdn"></a><span data-ttu-id="4f2c6-168">Za pomocą kodu ASP.NET Ajax z sieci CDN</span><span class="sxs-lookup"><span data-stu-id="4f2c6-168">Using ASP.NET Ajax from the CDN</span></span>

<span data-ttu-id="4f2c6-169">Korzystając z platformy ASP.NET 4, można przekierować wszystkie żądania ASP.NET framework skryptów do sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="4f2c6-169">When using ASP.NET 4, you can redirect all requests for ASP.NET framework scripts to the CDN.</span></span> <span data-ttu-id="4f2c6-170">Pobieranie skryptów z sieci CDN, zamiast z lokalnego serwera internetowego może znacznie zwiększyć wydajność publicznych witryn sieci Web, ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="4f2c6-170">Retrieving scripts from the CDN instead of your local web server can substantially improve the performance of public ASP.NET websites.</span></span>

<span data-ttu-id="4f2c6-171">Aby przekierować wszystkie żądania programu ASP.NET framework skryptu do Microsoft Ajax CDN, należy użyć właściwości ScriptManager EnableCDN:</span><span class="sxs-lookup"><span data-stu-id="4f2c6-171">Use the ScriptManager EnableCDN property to redirect all ASP.NET framework script requests to the Microsoft Ajax CDN:</span></span>

[!code-aspx[Main](overview/samples/sample1.aspx)]

<a id="Using_jQuery_from_the_CDN_21"></a>

## <a name="using-jquery-from-the-cdn"></a><span data-ttu-id="4f2c6-172">Przy użyciu jQuery z sieci CDN</span><span class="sxs-lookup"><span data-stu-id="4f2c6-172">Using jQuery from the CDN</span></span>

<span data-ttu-id="4f2c6-173">Można użyć skryptów jQuery hostowanej w usłudze CDN w aplikacji sieci Web, dodając następujący element skryptu na stronie:</span><span class="sxs-lookup"><span data-stu-id="4f2c6-173">You can use jQuery scripts hosted on CDN in your Web application by adding the following script element to a page:</span></span>

[!code-html[Main](overview/samples/sample2.html)]

<span data-ttu-id="4f2c6-174">Sieć CDN obejmuje również zminimalizowany wersji skryptu jQuery, którą możesz pobrać przy użyciu następującego elementu:</span><span class="sxs-lookup"><span data-stu-id="4f2c6-174">The CDN also includes the minified version of the jQuery script, which you can get using the following element:</span></span>

[!code-html[Main](overview/samples/sample3.html)]

<span data-ttu-id="4f2c6-175">Aby zezwolić na stronie jest zastępowany na potrzeby ładowania jQuery ze ścieżki lokalnej we własnej witrynie internetowej, jeśli sieć CDN stanie się ona niedostępna, Dodaj następujący element bezpośrednio po elemencie odwołujące się do sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="4f2c6-175">To allow your page to fallback to loading jQuery from a local path on your own website if the CDN happens to be unavailable, add the following element immediately after the element referencing the CDN:</span></span>

[!code-html[Main](overview/samples/sample4.html)]

<span data-ttu-id="4f2c6-176">Poniższej przykładowej strony użyto CDN wersję biblioteki jQuery (za pomocą rezerwowe w lokalnej kopii), aby wyświetlić zawartość elementu div po kliknięciu przycisku.</span><span class="sxs-lookup"><span data-stu-id="4f2c6-176">The following sample page uses the CDN version of the jQuery library (with fallback to a local copy) to display the contents of a div element when a button is clicked.</span></span>

[!code-html[Main](overview/samples/sample5.html)]

<span data-ttu-id="4f2c6-177">Możesz dowiedzieć się więcej o technologii jQuery i pobrać kopię lokalną jQuery, odwiedzając [jQuery](http://jquery.com/) witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="4f2c6-177">You can learn more about jQuery and download a local copy of jQuery by visiting the [jQuery](http://jquery.com/) Web site.</span></span>

<a id="Using_jQuery_UI_from_the_CDN_22"></a>

## <a name="using-jquery-ui-from-the-cdn"></a><span data-ttu-id="4f2c6-178">Przy użyciu jQuery interfejsu użytkownika z usługi CDN</span><span class="sxs-lookup"><span data-stu-id="4f2c6-178">Using jQuery UI from the CDN</span></span>

<span data-ttu-id="4f2c6-179">Usługa CDN obsługuje również biblioteki interfejsu użytkownika jQuery.</span><span class="sxs-lookup"><span data-stu-id="4f2c6-179">The CDN also hosts the jQuery UI library.</span></span> <span data-ttu-id="4f2c6-180">Biblioteka interfejsu użytkownika jQuery zawiera bogaty zestaw elementów widget i efekty, których można używać w aplikacjach ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="4f2c6-180">The jQuery UI library includes a rich set of widgets and effects that you can use in your ASP.NET applications.</span></span> <span data-ttu-id="4f2c6-181">Na przykład następująca strona ilustruje, jak można użyć selektora daty interfejsu użytkownika jQuery w kontekście aplikacji formularzy sieci Web ASP.NET do wyświetlenia wyskakującego kalendarza:</span><span class="sxs-lookup"><span data-stu-id="4f2c6-181">For example, the following page illustrates how you can use the jQuery UI Datepicker in the context of an ASP.NET Web Forms application to display a pop-up calendar:</span></span>

[!code-aspx[Main](overview/samples/sample6.aspx)]

<span data-ttu-id="4f2c6-182">Po przeniesieniu fokusu do pola tekstowego, za pomocą klawiatury, zostanie wyświetlona kalendarza:</span><span class="sxs-lookup"><span data-stu-id="4f2c6-182">When you move focus to the TextBox using your keyboard, a calendar is displayed:</span></span>

![Kalendarz podręczny utworzonych za pomocą selektora daty](overview/_static/image1.png)

<span data-ttu-id="4f2c6-184">Zwróć uwagę, że musi zawierać trzy pliki z usługi CDN w powyższym kodzie:</span><span class="sxs-lookup"><span data-stu-id="4f2c6-184">Notice that you must include three files from the CDN in the code above:</span></span>

- <span data-ttu-id="4f2c6-185">Biblioteka jQuery &mdash; biblioteki interfejsu użytkownika jQuery jest zależna od biblioteki jQuery.</span><span class="sxs-lookup"><span data-stu-id="4f2c6-185">The jQuery library &mdash; The jQuery UI library depends on the jQuery library.</span></span> <span data-ttu-id="4f2c6-186">Przed dodaniem biblioteki interfejsu użytkownika jQuery, należy dodać do strony w bibliotece jQuery.</span><span class="sxs-lookup"><span data-stu-id="4f2c6-186">You must add the jQuery library to your page before you add the jQuery UI library.</span></span>
- <span data-ttu-id="4f2c6-187">Biblioteka interfejsu użytkownika jQuery &mdash; biblioteki interfejsu użytkownika jQuery zawiera wszystkie efekty interfejsu użytkownika jQuery i widżetów, takich jak widżet Datepicker używanych w powyższych strony.</span><span class="sxs-lookup"><span data-stu-id="4f2c6-187">The jQuery UI library &mdash; The jQuery UI library contains all of the jQuery UI effects and widgets such as the Datepicker widget used in the page above.</span></span>
- <span data-ttu-id="4f2c6-188">Motyw interfejsu użytkownika jQuery &mdash; interfejs użytkownika jQuery obsługuje różne tematy.</span><span class="sxs-lookup"><span data-stu-id="4f2c6-188">A jQuery UI theme &mdash; The jQuery UI supports different themes.</span></span> <span data-ttu-id="4f2c6-189">Strona powyżej zawiera link do pliku CSS, aby zaimportować motywu Redmond.</span><span class="sxs-lookup"><span data-stu-id="4f2c6-189">The page above includes a link to a CSS file to import the Redmond theme.</span></span>

<span data-ttu-id="4f2c6-190">Wszystkie motywy interfejsu użytkownika jQuery standardowe są hostowane w sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="4f2c6-190">All of the standard jQuery UI themes are hosted on the CDN.</span></span> <span data-ttu-id="4f2c6-191">[Odwiedź tę stronę](jquery-ui/cdnjqueryui1910.md "interfejs użytkownika jQuery 1.8.10 w usłudze Microsoft Ajax CDN") do wyświetlania miniatur dla każdego motywu.</span><span class="sxs-lookup"><span data-stu-id="4f2c6-191">[Visit this page](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 on the Microsoft Ajax CDN") to view thumbnails for each theme.</span></span>

<span data-ttu-id="4f2c6-192">Aby dowiedzieć się więcej na temat biblioteki interfejsu użytkownika jQuery, odwiedź oficjalną [interfejsu użytkownika jQuery witryny sieci Web](http://jQueryUI.com "interfejsu użytkownika jQuery witryny sieci Web").</span><span class="sxs-lookup"><span data-stu-id="4f2c6-192">To learn more about the jQuery UI library, visit the official [jQuery UI website](http://jQueryUI.com "jQuery UI website").</span></span>

<a id="Third-Party_Files_on_the_CDN_23"></a>

## <a name="third-party-files-on-the-cdn"></a><span data-ttu-id="4f2c6-193">Pliki innych firm w usłudze CDN</span><span class="sxs-lookup"><span data-stu-id="4f2c6-193">Third-Party Files on the CDN</span></span>

<span data-ttu-id="4f2c6-194">Usługa CDN obsługuje niektóre najpopularniejsze biblioteki języka JavaScript innych firm.</span><span class="sxs-lookup"><span data-stu-id="4f2c6-194">The CDN hosts some of the most popular third party JavaScript libraries.</span></span> <span data-ttu-id="4f2c6-195">Microsoft zastrzega sobie prawa własności do żadnych bibliotek innych firm, hostowanych w tej sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="4f2c6-195">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="4f2c6-196">Właściciele praw autorskich bibliotek korzystają z licencji tych bibliotek do Ciebie.</span><span class="sxs-lookup"><span data-stu-id="4f2c6-196">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="4f2c6-197">Wszelkie prawa, które może być konieczne pobranie i używanie biblioteki są przyznawane wyłącznie przez właścicieli praw autorskich.</span><span class="sxs-lookup"><span data-stu-id="4f2c6-197">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="4f2c6-198">Ponieważ nie są to biblioteki Microsoft, firma Microsoft oferuje żadnych gwarancji ani licencji praw własności intelektualnej (w tym nie domyślnych praw patentowych) bibliotek innych firm, hostowanych w tej sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="4f2c6-198">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<a id="jQuery_Releases_on_the_CDN_0"></a>

### <a name="jquery-releases-on-the-cdn"></a><span data-ttu-id="4f2c6-199">Wersje dostępne w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="4f2c6-199">jQuery Releases on the CDN</span></span>

<span data-ttu-id="4f2c6-200">Następujące wersje jQuery znajdują się w sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="4f2c6-200">The following releases of jQuery are hosted on the CDN:</span></span>

#### <a name="jquery-version-331"></a><span data-ttu-id="4f2c6-201">w wersji 3.3.1 jQuery</span><span class="sxs-lookup"><span data-stu-id="4f2c6-201">jQuery version 3.3.1</span></span>
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.map

#### <a name="jquery-version-321"></a><span data-ttu-id="4f2c6-202">jQuery, wersja 3.2.1</span><span class="sxs-lookup"><span data-stu-id="4f2c6-202">jQuery version 3.2.1</span></span>
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.map

#### <a name="jquery-version-320"></a><span data-ttu-id="4f2c6-203">Wersja jQuery 3.2.0</span><span class="sxs-lookup"><span data-stu-id="4f2c6-203">jQuery version 3.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.map

#### <a name="jquery-version-311"></a><span data-ttu-id="4f2c6-204">Wersja jQuery 3.1.1</span><span class="sxs-lookup"><span data-stu-id="4f2c6-204">jQuery version 3.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.map

#### <a name="jquery-version-310"></a><span data-ttu-id="4f2c6-205">Wersja jQuery 3.1.0</span><span class="sxs-lookup"><span data-stu-id="4f2c6-205">jQuery version 3.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.map

#### <a name="jquery-version-300"></a><span data-ttu-id="4f2c6-206">jQuery w wersji 3.0.0</span><span class="sxs-lookup"><span data-stu-id="4f2c6-206">jQuery version 3.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.map

#### <a name="jquery-version-224"></a><span data-ttu-id="4f2c6-207">Wersja jQuery 2.2.4</span><span class="sxs-lookup"><span data-stu-id="4f2c6-207">jQuery version 2.2.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.map

#### <a name="jquery-version-223"></a><span data-ttu-id="4f2c6-208">Wersja jQuery 2.2.3</span><span class="sxs-lookup"><span data-stu-id="4f2c6-208">jQuery version 2.2.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.map

#### <a name="jquery-version-222"></a><span data-ttu-id="4f2c6-209">Wersja jQuery 2.2.2</span><span class="sxs-lookup"><span data-stu-id="4f2c6-209">jQuery version 2.2.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.map

#### <a name="jquery-version-221"></a><span data-ttu-id="4f2c6-210">jQuery wersję 2.2.1</span><span class="sxs-lookup"><span data-stu-id="4f2c6-210">jQuery version 2.2.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.map

#### <a name="jquery-version-220"></a><span data-ttu-id="4f2c6-211">Wersja jQuery 2.2.0</span><span class="sxs-lookup"><span data-stu-id="4f2c6-211">jQuery version 2.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.map

#### <a name="jquery-version-214"></a><span data-ttu-id="4f2c6-212">jQuery wersji 2.1.4</span><span class="sxs-lookup"><span data-stu-id="4f2c6-212">jQuery version 2.1.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.map

#### <a name="jquery-version-213"></a><span data-ttu-id="4f2c6-213">Wersja jQuery 2.1.3</span><span class="sxs-lookup"><span data-stu-id="4f2c6-213">jQuery version 2.1.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.map

#### <a name="jquery-version-212"></a><span data-ttu-id="4f2c6-214">jQuery, wersja 2.1.2</span><span class="sxs-lookup"><span data-stu-id="4f2c6-214">jQuery version 2.1.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.min.js

#### <a name="jquery-version-211"></a><span data-ttu-id="4f2c6-215">Wersja jQuery 2.1.1</span><span class="sxs-lookup"><span data-stu-id="4f2c6-215">jQuery version 2.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.map

#### <a name="jquery-version-210"></a><span data-ttu-id="4f2c6-216">jQuery wersja 2.1.0</span><span class="sxs-lookup"><span data-stu-id="4f2c6-216">jQuery version 2.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.map

#### <a name="jquery-version-203"></a><span data-ttu-id="4f2c6-217">jQuery wersji 2.0.3</span><span class="sxs-lookup"><span data-stu-id="4f2c6-217">jQuery version 2.0.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.map

#### <a name="jquery-version-202"></a><span data-ttu-id="4f2c6-218">Wersja jQuery pkt 2.0.2</span><span class="sxs-lookup"><span data-stu-id="4f2c6-218">jQuery version 2.0.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.map

#### <a name="jquery-version-201"></a><span data-ttu-id="4f2c6-219">Wersja jQuery 2.0.1</span><span class="sxs-lookup"><span data-stu-id="4f2c6-219">jQuery version 2.0.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.map

#### <a name="jquery-version-200"></a><span data-ttu-id="4f2c6-220">w wersji 2.0.0 jQuery</span><span class="sxs-lookup"><span data-stu-id="4f2c6-220">jQuery version 2.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.map

#### <a name="jquery-version-1124"></a><span data-ttu-id="4f2c6-221">Wersja jQuery 1.12.4</span><span class="sxs-lookup"><span data-stu-id="4f2c6-221">jQuery version 1.12.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.map

#### <a name="jquery-version-1123"></a><span data-ttu-id="4f2c6-222">Wersja jQuery 1.12.3</span><span class="sxs-lookup"><span data-stu-id="4f2c6-222">jQuery version 1.12.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.map

#### <a name="jquery-version-1122"></a><span data-ttu-id="4f2c6-223">Wersja jQuery 1.12.2</span><span class="sxs-lookup"><span data-stu-id="4f2c6-223">jQuery version 1.12.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.map

#### <a name="jquery-version-1121"></a><span data-ttu-id="4f2c6-224">Wersja jQuery 1.12.1</span><span class="sxs-lookup"><span data-stu-id="4f2c6-224">jQuery version 1.12.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.map

#### <a name="jquery-version-1120"></a><span data-ttu-id="4f2c6-225">Wersja jQuery 1.12.0</span><span class="sxs-lookup"><span data-stu-id="4f2c6-225">jQuery version 1.12.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.map

#### <a name="jquery-version-1113"></a><span data-ttu-id="4f2c6-226">Wersja jQuery 1.11.3</span><span class="sxs-lookup"><span data-stu-id="4f2c6-226">jQuery version 1.11.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.map

#### <a name="jquery-version-1112"></a><span data-ttu-id="4f2c6-227">Wersja jQuery 1.11.2</span><span class="sxs-lookup"><span data-stu-id="4f2c6-227">jQuery version 1.11.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.map

#### <a name="jquery-version-1111"></a><span data-ttu-id="4f2c6-228">Wersja jQuery 1.11.1</span><span class="sxs-lookup"><span data-stu-id="4f2c6-228">jQuery version 1.11.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.map

#### <a name="jquery-version-1110"></a><span data-ttu-id="4f2c6-229">Wersja jQuery 1.11.0</span><span class="sxs-lookup"><span data-stu-id="4f2c6-229">jQuery version 1.11.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.map

#### <a name="jquery-version-1102"></a><span data-ttu-id="4f2c6-230">Wersja jQuery 1.10.2</span><span class="sxs-lookup"><span data-stu-id="4f2c6-230">jQuery version 1.10.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.map

#### <a name="jquery-version-1101"></a><span data-ttu-id="4f2c6-231">Wersja jQuery 1.10.1</span><span class="sxs-lookup"><span data-stu-id="4f2c6-231">jQuery version 1.10.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.map

#### <a name="jquery-version-1100"></a><span data-ttu-id="4f2c6-232">Wersja jQuery 1.10.0</span><span class="sxs-lookup"><span data-stu-id="4f2c6-232">jQuery version 1.10.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.map

#### <a name="jquery-version-191"></a><span data-ttu-id="4f2c6-233">Wersja jQuery 1.9.1</span><span class="sxs-lookup"><span data-stu-id="4f2c6-233">jQuery version 1.9.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.map

#### <a name="jquery-version-190"></a><span data-ttu-id="4f2c6-234">Wersja jQuery 1.9.0</span><span class="sxs-lookup"><span data-stu-id="4f2c6-234">jQuery version 1.9.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.map

#### <a name="jquery-version-183"></a><span data-ttu-id="4f2c6-235">Wersja jQuery 1.8.3</span><span class="sxs-lookup"><span data-stu-id="4f2c6-235">jQuery version 1.8.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3-vsdoc.js

#### <a name="jquery-version-182"></a><span data-ttu-id="4f2c6-236">Wersja jQuery 1.8.2</span><span class="sxs-lookup"><span data-stu-id="4f2c6-236">jQuery version 1.8.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2-vsdoc.js

#### <a name="jquery-version-181"></a><span data-ttu-id="4f2c6-237">jQuery wersja 1.8.1</span><span class="sxs-lookup"><span data-stu-id="4f2c6-237">jQuery version 1.8.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1-vsdoc.js

#### <a name="jquery-version-180"></a><span data-ttu-id="4f2c6-238">Wersja jQuery 1.8.0</span><span class="sxs-lookup"><span data-stu-id="4f2c6-238">jQuery version 1.8.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0-vsdoc.js

#### <a name="jquery-version-172"></a><span data-ttu-id="4f2c6-239">Wersja jQuery 1.7.2</span><span class="sxs-lookup"><span data-stu-id="4f2c6-239">jQuery version 1.7.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.min.js

#### <a name="jquery-version-171"></a><span data-ttu-id="4f2c6-240">jQuery wersji 1.7.1</span><span class="sxs-lookup"><span data-stu-id="4f2c6-240">jQuery version 1.7.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1-vsdoc.js

#### <a name="jquery-version-17"></a><span data-ttu-id="4f2c6-241">jQuery w wersji 1.7</span><span class="sxs-lookup"><span data-stu-id="4f2c6-241">jQuery version 1.7</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7-vsdoc.js

#### <a name="jquery-version-164"></a><span data-ttu-id="4f2c6-242">Wersja jQuery 1.6.4</span><span class="sxs-lookup"><span data-stu-id="4f2c6-242">jQuery version 1.6.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4-vsdoc.js

#### <a name="jquery-version-163"></a><span data-ttu-id="4f2c6-243">Wersja jQuery 1.6.3</span><span class="sxs-lookup"><span data-stu-id="4f2c6-243">jQuery version 1.6.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3-vsdoc.js

#### <a name="jquery-version-162"></a><span data-ttu-id="4f2c6-244">jQuery wersji 1.6.2</span><span class="sxs-lookup"><span data-stu-id="4f2c6-244">jQuery version 1.6.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2-vsdoc.js

#### <a name="jquery-version-161"></a><span data-ttu-id="4f2c6-245">jQuery, wersja 1.6.1</span><span class="sxs-lookup"><span data-stu-id="4f2c6-245">jQuery version 1.6.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1-vsdoc.js

#### <a name="jquery-version-16"></a><span data-ttu-id="4f2c6-246">jQuery w wersji 1.6</span><span class="sxs-lookup"><span data-stu-id="4f2c6-246">jQuery version 1.6</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6-vsdoc.js

#### <a name="jquery-version-152"></a><span data-ttu-id="4f2c6-247">Wersja jQuery 1.5.2</span><span class="sxs-lookup"><span data-stu-id="4f2c6-247">jQuery version 1.5.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2-vsdoc.js

#### <a name="jquery-version-151"></a><span data-ttu-id="4f2c6-248">Wersja jQuery 1.5.1</span><span class="sxs-lookup"><span data-stu-id="4f2c6-248">jQuery version 1.5.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1-vsdoc.js

#### <a name="jquery-version-15"></a><span data-ttu-id="4f2c6-249">jQuery w wersji 1.5</span><span class="sxs-lookup"><span data-stu-id="4f2c6-249">jQuery version 1.5</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5-vsdoc.js

#### <a name="jquery-version-144"></a><span data-ttu-id="4f2c6-250">Wersja jQuery 1.4.4</span><span class="sxs-lookup"><span data-stu-id="4f2c6-250">jQuery version 1.4.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4-vsdoc.js

#### <a name="jquery-version-143"></a><span data-ttu-id="4f2c6-251">Wersja jQuery 1.4.3</span><span class="sxs-lookup"><span data-stu-id="4f2c6-251">jQuery version 1.4.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3-vsdoc.js

#### <a name="jquery-version-142"></a><span data-ttu-id="4f2c6-252">w wersji 1.4.2 jQuery</span><span class="sxs-lookup"><span data-stu-id="4f2c6-252">jQuery version 1.4.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2-vsdoc.js

#### <a name="jquery-version-141"></a><span data-ttu-id="4f2c6-253">Wersja jQuery 1.4.1</span><span class="sxs-lookup"><span data-stu-id="4f2c6-253">jQuery version 1.4.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1-vsdoc.js

#### <a name="jquery-version-14"></a><span data-ttu-id="4f2c6-254">jQuery w wersji 1.4</span><span class="sxs-lookup"><span data-stu-id="4f2c6-254">jQuery version 1.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.min.js

#### <a name="jquery-version-132"></a><span data-ttu-id="4f2c6-255">w wersji 1.3.2 jQuery</span><span class="sxs-lookup"><span data-stu-id="4f2c6-255">jQuery version 1.3.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min-vsdoc.js

<a id="jQuery_Migrate_Releases_on_the_CDN_1"></a>

### <a name="jquery-migrate-releases-on-the-cdn"></a><span data-ttu-id="4f2c6-256">Wersje jQuery migracji w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="4f2c6-256">jQuery Migrate Releases on the CDN</span></span>

<span data-ttu-id="4f2c6-257">Następujące wersje jQuery migracji znajdują się w sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="4f2c6-257">The following releases of jQuery Migrate are hosted on the CDN:</span></span>

#### <a name="jquery-migrate-version-300"></a><span data-ttu-id="4f2c6-258">jQuery migracji wersji 3.0.0</span><span class="sxs-lookup"><span data-stu-id="4f2c6-258">jQuery Migrate version 3.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.min.js

#### <a name="jquery-migrate-version-121"></a><span data-ttu-id="4f2c6-259">jQuery migracji wersji 1.2.1</span><span class="sxs-lookup"><span data-stu-id="4f2c6-259">jQuery Migrate version 1.2.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.min.js

<span data-ttu-id="4f2c6-260">jQuery migracji wersji 1.2.0 lub nowszej</span><span class="sxs-lookup"><span data-stu-id="4f2c6-260">jQuery Migrate version 1.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.min.js

#### <a name="jquery-migrate-version-111"></a><span data-ttu-id="4f2c6-261">Migrowanie wersji 1.1.1 jQuery</span><span class="sxs-lookup"><span data-stu-id="4f2c6-261">jQuery Migrate version 1.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.min.js

#### <a name="jquery-migrate-version-110"></a><span data-ttu-id="4f2c6-262">jQuery migracji wersji 1.1.0</span><span class="sxs-lookup"><span data-stu-id="4f2c6-262">jQuery Migrate version 1.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.min.js

#### <a name="jquery-migrate-version-100"></a><span data-ttu-id="4f2c6-263">jQuery migracji wersji 1.0.0</span><span class="sxs-lookup"><span data-stu-id="4f2c6-263">jQuery Migrate version 1.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.min.js

<a id="jQuery_UI_Releases_on_the_CDN_2"></a>

### <a name="jquery-ui-releases-on-the-cdn"></a><span data-ttu-id="4f2c6-264">jQuery wersjach interfejsu użytkownika w usłudze CDN</span><span class="sxs-lookup"><span data-stu-id="4f2c6-264">jQuery UI Releases on the CDN</span></span>

<span data-ttu-id="4f2c6-265">Następujące wersje biblioteki interfejsu użytkownika jQuery znajdują się w tej sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="4f2c6-265">The following releases of the jQuery UI library are hosted on this CDN.</span></span> <span data-ttu-id="4f2c6-266">Kliknij każdy link, aby wyświetlić rzeczywiste listę plików.</span><span class="sxs-lookup"><span data-stu-id="4f2c6-266">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="4f2c6-267">Interfejs użytkownika jQuery 1.12.1</span><span class="sxs-lookup"><span data-stu-id="4f2c6-267">jQuery UI 1.12.1</span></span>](jquery-ui/cdnjqueryui1121.md "interfejs użytkownika jQuery 1.12.1 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="4f2c6-268">Interfejs użytkownika jQuery 1.12.0</span><span class="sxs-lookup"><span data-stu-id="4f2c6-268">jQuery UI 1.12.0</span></span>](jquery-ui/cdnjqueryui1120.md "interfejs użytkownika jQuery 1.12.0 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="4f2c6-269">Interfejs użytkownika jQuery 1.11.4</span><span class="sxs-lookup"><span data-stu-id="4f2c6-269">jQuery UI 1.11.4</span></span>](jquery-ui/cdnjqueryui1114.md "interfejs użytkownika jQuery 1.11.4 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="4f2c6-270">Interfejs użytkownika jQuery 1.11.3</span><span class="sxs-lookup"><span data-stu-id="4f2c6-270">jQuery UI 1.11.3</span></span>](jquery-ui/cdnjqueryui1113.md "interfejs użytkownika jQuery 1.11.3 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="4f2c6-271">Interfejs użytkownika jQuery 1.11.2</span><span class="sxs-lookup"><span data-stu-id="4f2c6-271">jQuery UI 1.11.2</span></span>](jquery-ui/cdnjqueryui1112.md "interfejs użytkownika jQuery 1.11.2 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="4f2c6-272">Interfejs użytkownika jQuery 1.11.1</span><span class="sxs-lookup"><span data-stu-id="4f2c6-272">jQuery UI 1.11.1</span></span>](jquery-ui/cdnjqueryui1111.md "interfejs użytkownika jQuery 1.11.1 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="4f2c6-273">Interfejs użytkownika jQuery 1.11.0</span><span class="sxs-lookup"><span data-stu-id="4f2c6-273">jQuery UI 1.11.0</span></span>](jquery-ui/cdnjqueryui1110.md "interfejs użytkownika jQuery 1.11.0 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="4f2c6-274">Interfejs użytkownika jQuery 1.10.4</span><span class="sxs-lookup"><span data-stu-id="4f2c6-274">jQuery UI 1.10.4</span></span>](jquery-ui/cdnjqueryui1104.md "interfejs użytkownika jQuery 1.10.4 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="4f2c6-275">Interfejs użytkownika jQuery 1.10.3</span><span class="sxs-lookup"><span data-stu-id="4f2c6-275">jQuery UI 1.10.3</span></span>](jquery-ui/cdnjqueryui1103.md "interfejs użytkownika jQuery 1.10.3 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="4f2c6-276">Interfejs użytkownika jQuery 1.10.2</span><span class="sxs-lookup"><span data-stu-id="4f2c6-276">jQuery UI 1.10.2</span></span>](jquery-ui/cdnjqueryui1102.md "interfejs użytkownika jQuery 1.10.2 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="4f2c6-277">Interfejs użytkownika jQuery 1.10.1</span><span class="sxs-lookup"><span data-stu-id="4f2c6-277">jQuery UI 1.10.1</span></span>](jquery-ui/cdnjqueryui1101.md "interfejs użytkownika jQuery 1.10.1 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="4f2c6-278">Interfejs użytkownika jQuery 1.10.0</span><span class="sxs-lookup"><span data-stu-id="4f2c6-278">jQuery UI 1.10.0</span></span>](jquery-ui/cdnjqueryui1100.md "interfejs użytkownika jQuery 1.10.0 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="4f2c6-279">Interfejs użytkownika jQuery 1.9.2</span><span class="sxs-lookup"><span data-stu-id="4f2c6-279">jQuery UI 1.9.2</span></span>](jquery-ui/cdnjqueryui192.md "interfejs użytkownika jQuery 1.9.2 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="4f2c6-280">Interfejs użytkownika jQuery 1.9.1</span><span class="sxs-lookup"><span data-stu-id="4f2c6-280">jQuery UI 1.9.1</span></span>](jquery-ui/cdnjqueryui191.md "interfejs użytkownika jQuery 1.9.1 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="4f2c6-281">Interfejs użytkownika jQuery 1.9.0</span><span class="sxs-lookup"><span data-stu-id="4f2c6-281">jQuery UI 1.9.0</span></span>](jquery-ui/cdnjqueryui190.md "interfejs użytkownika jQuery 1.9.0 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="4f2c6-282">Interfejs użytkownika jQuery 1.8.24</span><span class="sxs-lookup"><span data-stu-id="4f2c6-282">jQuery UI 1.8.24</span></span>](jquery-ui/cdnjqueryui1824.md "interfejs użytkownika jQuery 1.8.24 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="4f2c6-283">Interfejs użytkownika jQuery 1.8.23</span><span class="sxs-lookup"><span data-stu-id="4f2c6-283">jQuery UI 1.8.23</span></span>](jquery-ui/cdnjqueryui1823.md "interfejs użytkownika jQuery 1.8.23 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="4f2c6-284">Interfejs użytkownika jQuery 1.8.22</span><span class="sxs-lookup"><span data-stu-id="4f2c6-284">jQuery UI 1.8.22</span></span>](jquery-ui/cdnjqueryui1822.md "interfejs użytkownika jQuery 1.8.22 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="4f2c6-285">Interfejs użytkownika jQuery 1.8.21</span><span class="sxs-lookup"><span data-stu-id="4f2c6-285">jQuery UI 1.8.21</span></span>](jquery-ui/cdnjqueryui1821.md "interfejs użytkownika jQuery 1.8.21 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="4f2c6-286">Interfejs użytkownika jQuery 1.8.20</span><span class="sxs-lookup"><span data-stu-id="4f2c6-286">jQuery UI 1.8.20</span></span>](jquery-ui/cdnjqueryui1820.md "interfejs użytkownika jQuery 1.8.20 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="4f2c6-287">Interfejs użytkownika jQuery 1.8.19</span><span class="sxs-lookup"><span data-stu-id="4f2c6-287">jQuery UI 1.8.19</span></span>](jquery-ui/cdnjqueryui1819.md "interfejs użytkownika jQuery 1.8.19 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="4f2c6-288">Interfejs użytkownika jQuery 1.8.18</span><span class="sxs-lookup"><span data-stu-id="4f2c6-288">jQuery UI 1.8.18</span></span>](jquery-ui/cdnjqueryui1818.md "interfejs użytkownika jQuery 1.8.18 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="4f2c6-289">Interfejs użytkownika jQuery 1.8.17</span><span class="sxs-lookup"><span data-stu-id="4f2c6-289">jQuery UI 1.8.17</span></span>](jquery-ui/cdnjqueryui1817.md "interfejs użytkownika jQuery 1.8.17 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="4f2c6-290">Interfejs użytkownika jQuery 1.8.16</span><span class="sxs-lookup"><span data-stu-id="4f2c6-290">jQuery UI 1.8.16</span></span>](jquery-ui/cdnjqueryui1816.md "interfejs użytkownika jQuery 1.8.16 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="4f2c6-291">Interfejs użytkownika jQuery 1.8.15</span><span class="sxs-lookup"><span data-stu-id="4f2c6-291">jQuery UI 1.8.15</span></span>](jquery-ui/cdnjqueryui1815.md "interfejs użytkownika jQuery 1.8.15 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="4f2c6-292">Interfejs użytkownika jQuery 1.8.14</span><span class="sxs-lookup"><span data-stu-id="4f2c6-292">jQuery UI 1.8.14</span></span>](jquery-ui/cdnjqueryui1814.md "interfejs użytkownika jQuery 1.8.14 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="4f2c6-293">Interfejs użytkownika jQuery 1.8.13</span><span class="sxs-lookup"><span data-stu-id="4f2c6-293">jQuery UI 1.8.13</span></span>](jquery-ui/cdnjqueryui1813.md "interfejs użytkownika jQuery 1.8.13 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="4f2c6-294">Interfejs użytkownika jQuery 1.8.12</span><span class="sxs-lookup"><span data-stu-id="4f2c6-294">jQuery UI 1.8.12</span></span>](jquery-ui/cdnjqueryui1812.md "interfejs użytkownika jQuery 1.8.12 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="4f2c6-295">Interfejs użytkownika jQuery 1.8.11</span><span class="sxs-lookup"><span data-stu-id="4f2c6-295">jQuery UI 1.8.11</span></span>](jquery-ui/cdnjqueryui1811.md "interfejs użytkownika jQuery 1.8.11 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="4f2c6-296">Interfejs użytkownika jQuery 1.8.10</span><span class="sxs-lookup"><span data-stu-id="4f2c6-296">jQuery UI 1.8.10</span></span>](jquery-ui/cdnjqueryui1910.md "interfejs użytkownika jQuery 1.8.10 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="4f2c6-297">Interfejs użytkownika jQuery 1.8.9</span><span class="sxs-lookup"><span data-stu-id="4f2c6-297">jQuery UI 1.8.9</span></span>](jquery-ui/cdnjqueryui189.md "interfejs użytkownika jQuery 1.8.9 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="4f2c6-298">Interfejs użytkownika jQuery 1.8.8</span><span class="sxs-lookup"><span data-stu-id="4f2c6-298">jQuery UI 1.8.8</span></span>](jquery-ui/cdnjqueryui188.md "interfejs użytkownika jQuery 1.8.8 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="4f2c6-299">Interfejs użytkownika jQuery 1.8.7</span><span class="sxs-lookup"><span data-stu-id="4f2c6-299">jQuery UI 1.8.7</span></span>](jquery-ui/cdnjqueryui187.md "interfejs użytkownika jQuery 1.8.7 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="4f2c6-300">Interfejs użytkownika jQuery 1.8.6</span><span class="sxs-lookup"><span data-stu-id="4f2c6-300">jQuery UI 1.8.6</span></span>](jquery-ui/cdnjqueryui186.md "interfejs użytkownika jQuery 1.8.6 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="4f2c6-301">jQuery UI 1.8.5</span><span class="sxs-lookup"><span data-stu-id="4f2c6-301">jQuery UI 1.8.5</span></span>](jquery-ui/cdnjqueryui185.md "jQuery UI 1.8.5")

<a id="jQuery_Validation_Releases_on_the_CDN_3"></a>

### <a name="jquery-validation-releases-on-the-cdn"></a><span data-ttu-id="4f2c6-302">Sprawdzanie poprawności wersji w usłudze CDN technologii jQuery</span><span class="sxs-lookup"><span data-stu-id="4f2c6-302">jQuery Validation Releases on the CDN</span></span>

<span data-ttu-id="4f2c6-303">Następujące wersje weryfikacji biblioteki jQuery znajdują się w tej sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="4f2c6-303">The following releases of the jQuery Validation library are hosted on this CDN.</span></span> <span data-ttu-id="4f2c6-304">Kliknij każdy link, aby wyświetlić rzeczywiste listę plików.</span><span class="sxs-lookup"><span data-stu-id="4f2c6-304">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="4f2c6-305">Sprawdź poprawność 1.19.0 jQuery</span><span class="sxs-lookup"><span data-stu-id="4f2c6-305">jQuery Validate 1.19.0</span></span>](jquery-validate/cdnjqueryvalidate1190.md "jQuery 1.19.0 sprawdzania poprawności")
- [<span data-ttu-id="4f2c6-306">Sprawdź poprawność 1.17.0 jQuery</span><span class="sxs-lookup"><span data-stu-id="4f2c6-306">jQuery Validate 1.17.0</span></span>](jquery-validate/cdnjqueryvalidate1170.md "jQuery 1.17.0 sprawdzania poprawności")
- [<span data-ttu-id="4f2c6-307">jQuery Validate 1.16.0</span><span class="sxs-lookup"><span data-stu-id="4f2c6-307">jQuery Validate 1.16.0</span></span>](jquery-validate/cdnjqueryvalidate1160.md "jQuery Validation 1.16.0")
- [<span data-ttu-id="4f2c6-308">Sprawdź poprawność 1.15.1 jQuery</span><span class="sxs-lookup"><span data-stu-id="4f2c6-308">jQuery Validate 1.15.1</span></span>](jquery-validate/cdnjqueryvalidate1151.md "jQuery 1.15.1 sprawdzania poprawności")
- [<span data-ttu-id="4f2c6-309">Sprawdź poprawność 1.15.0 jQuery</span><span class="sxs-lookup"><span data-stu-id="4f2c6-309">jQuery Validate 1.15.0</span></span>](jquery-validate/cdnjqueryvalidate1150.md "jQuery 1.15.0 sprawdzania poprawności")
- [<span data-ttu-id="4f2c6-310">Sprawdź poprawność 1.14.0 jQuery</span><span class="sxs-lookup"><span data-stu-id="4f2c6-310">jQuery Validate 1.14.0</span></span>](jquery-validate/cdnjqueryvalidate1140.md "jQuery 1.14.0 sprawdzania poprawności")
- [<span data-ttu-id="4f2c6-311">Sprawdź poprawność 1.13.1 jQuery</span><span class="sxs-lookup"><span data-stu-id="4f2c6-311">jQuery Validate 1.13.1</span></span>](jquery-validate/cdnjqueryvalidate1131.md "jQuery 1.13.1 sprawdzania poprawności")
- [<span data-ttu-id="4f2c6-312">Sprawdź poprawność 1.13.0 jQuery</span><span class="sxs-lookup"><span data-stu-id="4f2c6-312">jQuery Validate 1.13.0</span></span>](jquery-validate/cdnjqueryvalidate1130.md "jQuery 1.13.0 sprawdzania poprawności")
- [<span data-ttu-id="4f2c6-313">jQuery 1.12.0 sprawdzania poprawności</span><span class="sxs-lookup"><span data-stu-id="4f2c6-313">jQuery Validate 1.12.0</span></span>](jquery-validate/cdnjqueryvalidate1120.md "jQuery 1.12.0 sprawdzania poprawności")
- [<span data-ttu-id="4f2c6-314">jQuery 1.11.1 sprawdzania poprawności</span><span class="sxs-lookup"><span data-stu-id="4f2c6-314">jQuery Validate 1.11.1</span></span>](jquery-validate/cdnjqueryvalidate1111.md "jQuery 1.11.1 sprawdzania poprawności")
- [<span data-ttu-id="4f2c6-315">jQuery Validate 1.11.0</span><span class="sxs-lookup"><span data-stu-id="4f2c6-315">jQuery Validate 1.11.0</span></span>](jquery-validate/cdnjqueryvalidate111.md "jQuery Validation 1.11.0")
- [<span data-ttu-id="4f2c6-316">jQuery 1.10.0 sprawdzania poprawności</span><span class="sxs-lookup"><span data-stu-id="4f2c6-316">jQuery Validate 1.10.0</span></span>](jquery-validate/cdnjqueryvalidate110.md "jQuery 1.10.0 sprawdzania poprawności")
- [<span data-ttu-id="4f2c6-317">jQuery Validate 1.9</span><span class="sxs-lookup"><span data-stu-id="4f2c6-317">jQuery Validate 1.9</span></span>](jquery-validate/cdnjqueryvalidate19.md "jquery.validate version 1.9")
- [<span data-ttu-id="4f2c6-318">Sprawdź poprawność 1.8.1 jQuery</span><span class="sxs-lookup"><span data-stu-id="4f2c6-318">jQuery Validate 1.8.1</span></span>](jquery-validate/cdnjqueryvalidate181.md "jquery.validate wersja 1.8.1")
- [<span data-ttu-id="4f2c6-319">jQuery Validate 1.8</span><span class="sxs-lookup"><span data-stu-id="4f2c6-319">jQuery Validate 1.8</span></span>](jquery-validate/cdnjqueryvalidate18.md "jquery.validate version 1.8")
- [<span data-ttu-id="4f2c6-320">jQuery weryfikacji 1.7</span><span class="sxs-lookup"><span data-stu-id="4f2c6-320">jQuery Validate 1.7</span></span>](jquery-validate/cdnjqueryvalidate17.md "jquery.validate wersja 1.7")
- [<span data-ttu-id="4f2c6-321">jQuery Validate 1.6</span><span class="sxs-lookup"><span data-stu-id="4f2c6-321">jQuery Validate 1.6</span></span>](jquery-validate/cdnjqueryvalidate16.md "jQuery Validate 1.6")
- [<span data-ttu-id="4f2c6-322">jQuery Validate 1.5.5</span><span class="sxs-lookup"><span data-stu-id="4f2c6-322">jQuery Validate 1.5.5</span></span>](jquery-validate/cdnjqueryvalidate155.md "jQuery Validate 1.5.5")

<a id="jQuery_Mobile_Releases_on_the_CDN_4"></a>

### <a name="jquery-mobile-releases-on-the-cdn"></a><span data-ttu-id="4f2c6-323">jQuery Mobile wersji w usłudze CDN</span><span class="sxs-lookup"><span data-stu-id="4f2c6-323">jQuery Mobile Releases on the CDN</span></span>

<span data-ttu-id="4f2c6-324">Następujące wersje biblioteki jQuery przenośnych znajdują się w tej sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="4f2c6-324">The following releases of the jQuery Mobile library are hosted on this CDN.</span></span> <span data-ttu-id="4f2c6-325">Kliknij każdy link, aby wyświetlić rzeczywiste listę plików.</span><span class="sxs-lookup"><span data-stu-id="4f2c6-325">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="4f2c6-326">jQuery Mobile 1.4.5</span><span class="sxs-lookup"><span data-stu-id="4f2c6-326">jQuery Mobile 1.4.5</span></span>](jquery-mobile/cdnjquerymobile145.md "jQuery Mobile 1.4.5 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="4f2c6-327">jQuery Mobile 1.4.2</span><span class="sxs-lookup"><span data-stu-id="4f2c6-327">jQuery Mobile 1.4.2</span></span>](jquery-mobile/cdnjquerymobile142.md "jQuery Mobile 1.4.2 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="4f2c6-328">jQuery Mobile 1.4.1</span><span class="sxs-lookup"><span data-stu-id="4f2c6-328">jQuery Mobile 1.4.1</span></span>](jquery-mobile/cdnjquerymobile141.md "jQuery Mobile 1.4.1 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="4f2c6-329">jQuery Mobile 1.4.0</span><span class="sxs-lookup"><span data-stu-id="4f2c6-329">jQuery Mobile 1.4.0</span></span>](jquery-mobile/cdnjquerymobile140.md "jQuery Mobile 1.4.0 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="4f2c6-330">jQuery Mobile 1.3.2</span><span class="sxs-lookup"><span data-stu-id="4f2c6-330">jQuery Mobile 1.3.2</span></span>](jquery-mobile/cdnjquerymobile132.md "jQuery Mobile 1.3.2 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="4f2c6-331">jQuery Mobile 1.3.1</span><span class="sxs-lookup"><span data-stu-id="4f2c6-331">jQuery Mobile 1.3.1</span></span>](jquery-mobile/cdnjquerymobile131.md "jQuery Mobile 1.3.1 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="4f2c6-332">jQuery Mobile 1.3.0</span><span class="sxs-lookup"><span data-stu-id="4f2c6-332">jQuery Mobile 1.3.0</span></span>](jquery-mobile/cdnjquerymobile130.md "jQuery Mobile 1.3.0 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="4f2c6-333">jQuery Mobile 1.2.0</span><span class="sxs-lookup"><span data-stu-id="4f2c6-333">jQuery Mobile 1.2.0</span></span>](jquery-mobile/cdnjquerymobile120.md "jQuery Mobile 1.2.0 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="4f2c6-334">jQuery Mobile 1.1.2</span><span class="sxs-lookup"><span data-stu-id="4f2c6-334">jQuery Mobile 1.1.2</span></span>](jquery-mobile/cdnjquerymobile112.md "jQuery Mobile 1.1.2 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="4f2c6-335">jQuery Mobile 1.1.1</span><span class="sxs-lookup"><span data-stu-id="4f2c6-335">jQuery Mobile 1.1.1</span></span>](jquery-mobile/cdnjquerymobile111.md "jQuery Mobile 1.1.1 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="4f2c6-336">jQuery Mobile 1.1.0</span><span class="sxs-lookup"><span data-stu-id="4f2c6-336">jQuery Mobile 1.1.0</span></span>](jquery-mobile/cdnjquerymobile110.md "jQuery Mobile 1.1.0 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="4f2c6-337">jQuery Mobile 1.1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="4f2c6-337">jQuery Mobile 1.1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile110rc2.md "jQuery Mobile 1.1.0 RC2 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="4f2c6-338">jQuery Mobile 1.0.1</span><span class="sxs-lookup"><span data-stu-id="4f2c6-338">jQuery Mobile 1.0.1</span></span>](jquery-mobile/cdnjquerymobile101.md "jQuery Mobile 1.0.1 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="4f2c6-339">jQuery Mobile 1.0</span><span class="sxs-lookup"><span data-stu-id="4f2c6-339">jQuery Mobile 1.0</span></span>](jquery-mobile/cdnjquerymobile10.md "jQuery Mobile 1.0 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="4f2c6-340">jQuery Mobile 1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="4f2c6-340">jQuery Mobile 1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile10rc2.md "jQuery Mobile 1.0 RC2 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="4f2c6-341">jQuery Mobile 1.0 RC 1</span><span class="sxs-lookup"><span data-stu-id="4f2c6-341">jQuery Mobile 1.0 RC 1</span></span>](jquery-mobile/cdnjquerymobile10rc1.md "jQuery Mobile 1.0 RC1 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="4f2c6-342">jQuery Mobile 1.0 beta 3</span><span class="sxs-lookup"><span data-stu-id="4f2c6-342">jQuery Mobile 1.0 beta 3</span></span>](jquery-mobile/cdnjquerymobile10b3.md "jQuery Mobile 1.0 Beta 3 w usłudze Microsoft Ajax CDN")

<a id="jQuery_Templates_Releases_on_the_CDN_5"></a>

### <a name="jquery-templates-releases-on-the-cdn"></a><span data-ttu-id="4f2c6-343">jQuery szablony wydań w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="4f2c6-343">jQuery Templates Releases on the CDN</span></span>

<span data-ttu-id="4f2c6-344">Następujące wersje wtyczki szablonów jQuery znajdują się w tej sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="4f2c6-344">The following releases of the jQuery Templates plugin are hosted on this CDN.</span></span> <span data-ttu-id="4f2c6-345">Kliknij każdy link, aby wyświetlić rzeczywiste listę plików.</span><span class="sxs-lookup"><span data-stu-id="4f2c6-345">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="4f2c6-346">jQuery szablonów w wersji Beta 1</span><span class="sxs-lookup"><span data-stu-id="4f2c6-346">jQuery Templates Beta 1</span></span>](jquery-templates/cdnjquerytemplatesbeta1.md "jQuery szablonów w wersji Beta 1")

<a id="jQuery_Cycle_Releases_on_the_CDN_6"></a>

### <a name="jquery-cycle-releases-on-the-cdn"></a><span data-ttu-id="4f2c6-347">jQuery cyklu wersji w usłudze CDN</span><span class="sxs-lookup"><span data-stu-id="4f2c6-347">jQuery Cycle Releases on the CDN</span></span>

<span data-ttu-id="4f2c6-348">Następujące wersje wtyczki cyklu jQuery znajdują się w tej sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="4f2c6-348">The following releases of the jQuery Cycle plugin are hosted on this CDN.</span></span> <span data-ttu-id="4f2c6-349">Kliknij każdy link, aby wyświetlić rzeczywiste listę plików.</span><span class="sxs-lookup"><span data-stu-id="4f2c6-349">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="4f2c6-350">jQuery Cycle 2.99</span><span class="sxs-lookup"><span data-stu-id="4f2c6-350">jQuery Cycle 2.99</span></span>](jquery-cycle/cdnjquerycycle299.md "jQuery Cycle 2.99")
- [<span data-ttu-id="4f2c6-351">jQuery Cycle 2.94</span><span class="sxs-lookup"><span data-stu-id="4f2c6-351">jQuery Cycle 2.94</span></span>](jquery-cycle/cdnjquerycycle294.md "jQuery Cycle 2.94")
- [<span data-ttu-id="4f2c6-352">jQuery Cycle 2.88</span><span class="sxs-lookup"><span data-stu-id="4f2c6-352">jQuery Cycle 2.88</span></span>](jquery-cycle/cdnjquerycycle288.md "jQuery Cycle 2.88")

<a id="jQuery_DataTables_Releases_on_the_CDN_7"></a>

### <a name="jquery-datatables-releases-on-the-cdn"></a><span data-ttu-id="4f2c6-353">jQuery wersji DataTable w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="4f2c6-353">jQuery DataTables Releases on the CDN</span></span>

<span data-ttu-id="4f2c6-354">Następujące wersje wtyczki DataTables jQuery znajdują się w tej sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="4f2c6-354">The following releases of the jQuery DataTables plugin are hosted on this CDN.</span></span> <span data-ttu-id="4f2c6-355">Kliknij każdy link, aby wyświetlić rzeczywiste listę plików.</span><span class="sxs-lookup"><span data-stu-id="4f2c6-355">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="4f2c6-356">jQuery DataTables 1.10.5</span><span class="sxs-lookup"><span data-stu-id="4f2c6-356">jQuery DataTables 1.10.5</span></span>](jquery-datatables/cdnjquerydatatables105.md "jQuery DataTables 1.10.5")
- [<span data-ttu-id="4f2c6-357">jQuery DataTables 1.10.4</span><span class="sxs-lookup"><span data-stu-id="4f2c6-357">jQuery DataTables 1.10.4</span></span>](jquery-datatables/cdnjquerydatatables104.md "jQuery DataTables 1.10.4")
- [<span data-ttu-id="4f2c6-358">jQuery DataTables 1.9.4</span><span class="sxs-lookup"><span data-stu-id="4f2c6-358">jQuery DataTables 1.9.4</span></span>](jquery-datatables/cdnjquerydatatables194.md "jQuery DataTables 1.9.4")
- [<span data-ttu-id="4f2c6-359">jQuery DataTables 1.9.3</span><span class="sxs-lookup"><span data-stu-id="4f2c6-359">jQuery DataTables 1.9.3</span></span>](jquery-datatables/cdnjquerydatatables193.md "jQuery DataTables 1.9.3")
- [<span data-ttu-id="4f2c6-360">jQuery DataTables 1.9.2</span><span class="sxs-lookup"><span data-stu-id="4f2c6-360">jQuery DataTables 1.9.2</span></span>](jquery-datatables/cdnjquerydatatables192.md "jQuery DataTables 1.9.2")
- [<span data-ttu-id="4f2c6-361">jQuery DataTables 1.9.1</span><span class="sxs-lookup"><span data-stu-id="4f2c6-361">jQuery DataTables 1.9.1</span></span>](jquery-datatables/cdnjquerydatatables191.md "jQuery DataTables 1.9.1")
- [<span data-ttu-id="4f2c6-362">jQuery DataTables 1.9.0</span><span class="sxs-lookup"><span data-stu-id="4f2c6-362">jQuery DataTables 1.9.0</span></span>](jquery-datatables/cdnjquerydatatables190.md "jQuery DataTables 1.9.0")
- [<span data-ttu-id="4f2c6-363">jQuery DataTables 1.8.2</span><span class="sxs-lookup"><span data-stu-id="4f2c6-363">jQuery DataTables 1.8.2</span></span>](jquery-datatables/cdnjquerydatatables182.md "jQuery DataTables 1.8.2")

<a id="Modernizr_Releases_on_the_CDN_8"></a>

### <a name="modernizr-releases-on-the-cdn"></a><span data-ttu-id="4f2c6-364">Wersje Modernizr w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="4f2c6-364">Modernizr Releases on the CDN</span></span>

<span data-ttu-id="4f2c6-365">Następujące wersje programu [Modernizr](http://www.modernizr.com "Modernizr") znajdują się w sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="4f2c6-365">The following releases of [Modernizr](http://www.modernizr.com "Modernizr") are hosted on the CDN:</span></span>

- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-3.5.0.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.8.3.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.2.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.1.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.6.2.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-1.7-development-only.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.0.6-development-only.js

<a id="JSHint_Releases_on_the_CDN_10"></a>

### <a name="jshint-releases-on-the-cdn"></a><span data-ttu-id="4f2c6-366">Wersje JSHint w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="4f2c6-366">JSHint Releases on the CDN</span></span>

<span data-ttu-id="4f2c6-367">Następujące wersje programu [JSHint](http://www.jshint.com "JSHint") znajdują się w sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="4f2c6-367">The following releases of [JSHint](http://www.jshint.com "JSHint") are hosted on the CDN:</span></span>

- https://ajax.aspnetcdn.com/ajax/jshint/r07/jshint.js

<a id="Knockout_Releases_on_the_CDN_11"></a>

### <a name="knockout-releases-on-the-cdn"></a><span data-ttu-id="4f2c6-368">Wersje knockout w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="4f2c6-368">Knockout Releases on the CDN</span></span>

<span data-ttu-id="4f2c6-369">Następujące wersje programu [Knockout](http://www.knockoutjs.com "Knockout") znajdują się w sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="4f2c6-369">The following releases of [Knockout](http://www.knockoutjs.com "Knockout") are hosted on the CDN:</span></span>

- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.debug.js

<a id="Globalize_Releases_on_the_CDN_12"></a>

### <a name="globalize-releases-on-the-cdn"></a><span data-ttu-id="4f2c6-370">Sprzedawać wersji w usłudze CDN</span><span class="sxs-lookup"><span data-stu-id="4f2c6-370">Globalize Releases on the CDN</span></span>

<span data-ttu-id="4f2c6-371">Następujące wersje programu [Globalize](https://github.com/jquery/globalize "Globalize") znajdują się w sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="4f2c6-371">The following releases of [Globalize](https://github.com/jquery/globalize "Globalize") are hosted on the CDN:</span></span>

#### <a name="globalize-version-100"></a><span data-ttu-id="4f2c6-372">Sprzedawać w wersji 1.0.0</span><span class="sxs-lookup"><span data-stu-id="4f2c6-372">Globalize version 1.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/node-main.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/currency.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/date.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/message.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/number.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/plural.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/relative-time.js

#### <a name="globalize-version-011"></a><span data-ttu-id="4f2c6-373">Sprzedawać wersji 0.1.1</span><span class="sxs-lookup"><span data-stu-id="4f2c6-373">Globalize version 0.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.min.js
- https://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.js
- https://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.cultures.js

    - <span data-ttu-id="4f2c6-374">wszystkich kultur</span><span class="sxs-lookup"><span data-stu-id="4f2c6-374">all cultures</span></span>
- https://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.culture.{culture-code}.js

    - <span data-ttu-id="4f2c6-375">Zamień na kod żądaną kultury "{kodu kultury}", np. Microsoft globalize.culture.en GB.js== plików w usłudze CDN == te biblioteki zostały przekazane przez firmę Microsoft.</span><span class="sxs-lookup"><span data-stu-id="4f2c6-375">Replace "{culture-code}" with the desired culture code, e.g. globalize.culture.en-GB.js== Microsoft Files on the CDN ==These libraries were uploaded by Microsoft.</span></span>

<a id="Respond_Releases_on_the_CDN_13"></a>

### <a name="respond-releases-on-the-cdn"></a><span data-ttu-id="4f2c6-376">Odpowiadać wersji w usłudze CDN</span><span class="sxs-lookup"><span data-stu-id="4f2c6-376">Respond Releases on the CDN</span></span>

<span data-ttu-id="4f2c6-377">Następujące wersje programu [https://github.com/scottjehl/Respond](https://github.com/scottjehl/Respond "https://github.com/scottjehl/Respond") odpowiada znajdują się w sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="4f2c6-377">The following releases of [https://github.com/scottjehl/Respond](https://github.com/scottjehl/Respond "https://github.com/scottjehl/Respond") Respond are hosted on the CDN:</span></span>

#### <a name="respond-version-142"></a><span data-ttu-id="4f2c6-378">Odpowiedź w wersji 1.4.2</span><span class="sxs-lookup"><span data-stu-id="4f2c6-378">Respond version 1.4.2</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.min.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.min.js

#### <a name="respond-version-141"></a><span data-ttu-id="4f2c6-379">Wersja odpowiada 1.4.1</span><span class="sxs-lookup"><span data-stu-id="4f2c6-379">Respond version 1.4.1</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.min.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.min.js

#### <a name="respond-version-140"></a><span data-ttu-id="4f2c6-380">Odpowiadać wersji 1.4.0</span><span class="sxs-lookup"><span data-stu-id="4f2c6-380">Respond version 1.4.0</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.min.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.min.js

#### <a name="respond-version-130"></a><span data-ttu-id="4f2c6-381">Wersja odpowiada 1.3.0</span><span class="sxs-lookup"><span data-stu-id="4f2c6-381">Respond version 1.3.0</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.3.0/respond.js

#### <a name="respond-version-120"></a><span data-ttu-id="4f2c6-382">Odpowiadać wersji 1.2.0 lub nowszej</span><span class="sxs-lookup"><span data-stu-id="4f2c6-382">Respond version 1.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.2.0/respond.js

<a id="Bootstrap_Releases_on_the_CDN_14"></a>

### <a name="bootstrap-releases-on-the-cdn"></a><span data-ttu-id="4f2c6-383">Wersje ładowania w usłudze CDN</span><span class="sxs-lookup"><span data-stu-id="4f2c6-383">Bootstrap Releases on the CDN</span></span>

<span data-ttu-id="4f2c6-384">Następujące wersje programu [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap znajdują się w sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="4f2c6-384">The following releases of [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap are hosted on the CDN:</span></span>

#### <a name="bootstrap-version-431"></a><span data-ttu-id="4f2c6-385">Uruchomienie wersji 4.3.1</span><span class="sxs-lookup"><span data-stu-id="4f2c6-385">Bootstrap version 4.3.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/4.3.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.3.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.3.1/bootstrap.bundle.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.3.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.3.1/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.3.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.3.1/css/bootstrap-grid.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.3.1/css/bootstrap-grid.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.3.1/css/bootstrap-grid.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.3.1/css/bootstrap-reboot.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.3.1/css/bootstrap-reboot.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.3.1/css/bootstrap-reboot.css.map

#### <a name="bootstrap-version-421"></a><span data-ttu-id="4f2c6-386">Wersja bootstrap 4.2.1</span><span class="sxs-lookup"><span data-stu-id="4f2c6-386">Bootstrap version 4.2.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/bootstrap.bundle.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/css/bootstrap-grid.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/css/bootstrap-grid.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/css/bootstrap-grid.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/css/bootstrap-reboot.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/css/bootstrap-reboot.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/css/bootstrap-reboot.css.map

#### <a name="bootstrap-version-411"></a><span data-ttu-id="4f2c6-387">Wersja bootstrap 4.1.1</span><span class="sxs-lookup"><span data-stu-id="4f2c6-387">Bootstrap version 4.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/bootstrap.bundle.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap-grid.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap-grid.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap-grid.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap-reboot.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap-reboot.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap-reboot.css.map

#### <a name="bootstrap-version-400"></a><span data-ttu-id="4f2c6-388">Uruchomienie wersji 4.0.0</span><span class="sxs-lookup"><span data-stu-id="4f2c6-388">Bootstrap version 4.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/bootstrap.bundle.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap-grid.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap-grid.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap-grid.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap-reboot.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap-reboot.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap-reboot.css.map

#### <a name="bootstrap-version-341"></a><span data-ttu-id="4f2c6-389">Wersja bootstrap 3.4.1</span><span class="sxs-lookup"><span data-stu-id="4f2c6-389">Bootstrap version 3.4.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-340"></a><span data-ttu-id="4f2c6-390">Wersja bootstrap 3.4.0</span><span class="sxs-lookup"><span data-stu-id="4f2c6-390">Bootstrap version 3.4.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-337"></a><span data-ttu-id="4f2c6-391">Wersja bootstrap 3.3.7</span><span class="sxs-lookup"><span data-stu-id="4f2c6-391">Bootstrap version 3.3.7</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-336"></a><span data-ttu-id="4f2c6-392">Wersja bootstrap 3.3.6</span><span class="sxs-lookup"><span data-stu-id="4f2c6-392">Bootstrap version 3.3.6</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-335"></a><span data-ttu-id="4f2c6-393">Wersja bootstrap 3.3.5</span><span class="sxs-lookup"><span data-stu-id="4f2c6-393">Bootstrap version 3.3.5</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-334"></a><span data-ttu-id="4f2c6-394">Wersja bootstrap 3.3.4</span><span class="sxs-lookup"><span data-stu-id="4f2c6-394">Bootstrap version 3.3.4</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-332"></a><span data-ttu-id="4f2c6-395">Wersja bootstrap 3.3.2</span><span class="sxs-lookup"><span data-stu-id="4f2c6-395">Bootstrap version 3.3.2</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-331"></a><span data-ttu-id="4f2c6-396">W wersji 3.3.1 bootstrap</span><span class="sxs-lookup"><span data-stu-id="4f2c6-396">Bootstrap version 3.3.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-330"></a><span data-ttu-id="4f2c6-397">Wersja bootstrap 3.3.0</span><span class="sxs-lookup"><span data-stu-id="4f2c6-397">Bootstrap version 3.3.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-320"></a><span data-ttu-id="4f2c6-398">Wersja bootstrap 3.2.0</span><span class="sxs-lookup"><span data-stu-id="4f2c6-398">Bootstrap version 3.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-311"></a><span data-ttu-id="4f2c6-399">Wersja bootstrap 3.1.1</span><span class="sxs-lookup"><span data-stu-id="4f2c6-399">Bootstrap version 3.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-310"></a><span data-ttu-id="4f2c6-400">Wersja bootstrap 3.1.0</span><span class="sxs-lookup"><span data-stu-id="4f2c6-400">Bootstrap version 3.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-303"></a><span data-ttu-id="4f2c6-401">Wersja bootstrap 3.0.3</span><span class="sxs-lookup"><span data-stu-id="4f2c6-401">Bootstrap version 3.0.3</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-302"></a><span data-ttu-id="4f2c6-402">Wersja 3.0.2 bootstrap</span><span class="sxs-lookup"><span data-stu-id="4f2c6-402">Bootstrap version 3.0.2</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-301"></a><span data-ttu-id="4f2c6-403">Uruchomienie wersji 3.0.1</span><span class="sxs-lookup"><span data-stu-id="4f2c6-403">Bootstrap version 3.0.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-300"></a><span data-ttu-id="4f2c6-404">Uruchomienie wersji 3.0.0</span><span class="sxs-lookup"><span data-stu-id="4f2c6-404">Bootstrap version 3.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-232"></a><span data-ttu-id="4f2c6-405">Uruchomienie wersji 2.3.2</span><span class="sxs-lookup"><span data-stu-id="4f2c6-405">Bootstrap version 2.3.2</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings.png
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings-white.png

#### <a name="bootstrap-version-231"></a><span data-ttu-id="4f2c6-406">Wersja 2.3.1 bootstrap</span><span class="sxs-lookup"><span data-stu-id="4f2c6-406">Bootstrap version 2.3.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings.png
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings-white.png

<a id="BootstrapTouchCarousel_Releases_on_the_CDN_18"></a>

### <a name="bootstrap-touchcarousel-releases-on-the-cdn"></a><span data-ttu-id="4f2c6-407">Wersje TouchCarousel ładowania w usłudze CDN</span><span class="sxs-lookup"><span data-stu-id="4f2c6-407">Bootstrap TouchCarousel Releases on the CDN</span></span>

<span data-ttu-id="4f2c6-408">Następujące wersje programu [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") Bootstrap TouchCarousel wersji znajdują się w sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="4f2c6-408">The following releases of [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") Bootstrap TouchCarousel releases are hosted on the CDN:</span></span>

#### <a name="bootstrap-touchcarousel-version-080"></a><span data-ttu-id="4f2c6-409">Bootstrap TouchCarousel version 0.8.0</span><span class="sxs-lookup"><span data-stu-id="4f2c6-409">Bootstrap TouchCarousel version 0.8.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/css/bootstrap-touch-carousel.css
- https://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/js/bootstrap-touch-carousel.js

<a id="Hammerjs_Releases_on_the_CDN_19"></a>

### <a name="hammerjs-releases-on-the-cdn"></a><span data-ttu-id="4f2c6-410">Wersje hammer.js w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="4f2c6-410">Hammer.js Releases on the CDN</span></span>

<span data-ttu-id="4f2c6-411">Następujące wersje programu [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js wersji znajdują się w sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="4f2c6-411">The following releases of [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js releases are hosted on the CDN:</span></span>

#### <a name="hammerjs-version-204"></a><span data-ttu-id="4f2c6-412">Hammer.js w wersji 2.0.4</span><span class="sxs-lookup"><span data-stu-id="4f2c6-412">Hammer.js version 2.0.4</span></span>

- https://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.js
- https://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.js
- https://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.map

<a id="ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15"></a>

### <a name="aspnet-web-forms-and-ajax-releases-on-the-cdn"></a><span data-ttu-id="4f2c6-413">ASP.NET Web Forms i Ajax wersji w usłudze CDN</span><span class="sxs-lookup"><span data-stu-id="4f2c6-413">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>

<span data-ttu-id="4f2c6-414">Następujące wersje biblioteki ASP.NET Ajax znajdują się w sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="4f2c6-414">The following releases of the ASP.NET Ajax Library are hosted on the CDN.</span></span> <span data-ttu-id="4f2c6-415">Kliknij każdy link, aby wyświetlić rzeczywiste listę plików.</span><span class="sxs-lookup"><span data-stu-id="4f2c6-415">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="4f2c6-416">Wersja ASP.NET Web Forms i Ajax 4.5.2</span><span class="sxs-lookup"><span data-stu-id="4f2c6-416">ASP.NET Web Forms and Ajax version 4.5.2</span></span>](cdnajax452.md "ASP.NET Web Forms i Ajax 4.5.2")
- [<span data-ttu-id="4f2c6-417">Wersja ASP.NET Web Forms i Ajax 4</span><span class="sxs-lookup"><span data-stu-id="4f2c6-417">ASP.NET Web Forms and Ajax version 4</span></span>](cdnajax4.md "ASP.NET Web Forms i Ajax 4")
- [<span data-ttu-id="4f2c6-418">ASP.NET Ajax w wersji 3.5</span><span class="sxs-lookup"><span data-stu-id="4f2c6-418">ASP.NET Ajax version 3.5</span></span>](cdnajax35.md "Ajax programu ASP.NET 3.5")

<a id="ASPNET_MVC_Releases_on_the_CDN_16"></a>

### <a name="aspnet-mvc-releases-on-the-cdn"></a><span data-ttu-id="4f2c6-419">ASP.NET MVC udostępnia w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="4f2c6-419">ASP.NET MVC Releases on the CDN</span></span>

<span data-ttu-id="4f2c6-420">Następujące pliki ASP.NET MVC JavaScript znajdują się w tej sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="4f2c6-420">The following ASP.NET MVC JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-mvc-523"></a><span data-ttu-id="4f2c6-421">ASP.NET MVC 5.2.3</span><span class="sxs-lookup"><span data-stu-id="4f2c6-421">ASP.NET MVC 5.2.3</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-51"></a><span data-ttu-id="4f2c6-422">ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="4f2c6-422">ASP.NET MVC 5.1</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-50"></a><span data-ttu-id="4f2c6-423">ASP.NET MVC 5.0</span><span class="sxs-lookup"><span data-stu-id="4f2c6-423">ASP.NET MVC 5.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-40"></a><span data-ttu-id="4f2c6-424">ASP.NET MVC 4.0</span><span class="sxs-lookup"><span data-stu-id="4f2c6-424">ASP.NET MVC 4.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-30"></a><span data-ttu-id="4f2c6-425">ASP.NET MVC 3.0</span><span class="sxs-lookup"><span data-stu-id="4f2c6-425">ASP.NET MVC 3.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.js
- https://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.min.js
- https://ajax.aspnetcdn.com/ajax/jquery.unobtrusive-ajax/3.2.5/jquery.unobtrusive-ajax.js
- https://ajax.aspnetcdn.com/ajax/jquery.unobtrusive-ajax/3.2.5/jquery.unobtrusive-ajax.min.js 
- https://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.min.js
- https://ajax.aspnetcdn.com/ajax/jquery.validation.unobtrusive/3.2.10/jquery.validate.unobtrusive.js 
- https://ajax.aspnetcdn.com/ajax/jquery.validation.unobtrusive/3.2.10/jquery.validate.unobtrusive.min.js
- https://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.js
- https://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.debug.js

#### <a name="aspnet-mvc-20"></a><span data-ttu-id="4f2c6-426">ASP.NET MVC 2.0</span><span class="sxs-lookup"><span data-stu-id="4f2c6-426">ASP.NET MVC 2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.js
- https://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.debug.js

#### <a name="aspnet-mvc-10"></a><span data-ttu-id="4f2c6-427">ASP.NET MVC 1.0</span><span class="sxs-lookup"><span data-stu-id="4f2c6-427">ASP.NET MVC 1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.js
- https://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.debug.js

<a id="ASPNET_SignalR_Releases_on_the_CDN_17"></a>

### <a name="aspnet-signalr-releases-on-the-cdn"></a><span data-ttu-id="4f2c6-428">Wersje biblioteki SignalR platformy ASP.NET, w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="4f2c6-428">ASP.NET SignalR Releases on the CDN</span></span>

<span data-ttu-id="4f2c6-429">Następujące pliki ASP.NET SignalR JavaScript znajdują się w tej sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="4f2c6-429">The following ASP.NET SignalR JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-signalr-222"></a><span data-ttu-id="4f2c6-430">ASP.NET SignalR 2.2.2</span><span class="sxs-lookup"><span data-stu-id="4f2c6-430">ASP.NET SignalR 2.2.2</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.js

#### <a name="aspnet-signalr-221"></a><span data-ttu-id="4f2c6-431">ASP.NET SignalR 2.2.1</span><span class="sxs-lookup"><span data-stu-id="4f2c6-431">ASP.NET SignalR 2.2.1</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.js

#### <a name="aspnet-signalr-220"></a><span data-ttu-id="4f2c6-432">ASP.NET SignalR 2.2.0</span><span class="sxs-lookup"><span data-stu-id="4f2c6-432">ASP.NET SignalR 2.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.js

#### <a name="aspnet-signalr-210"></a><span data-ttu-id="4f2c6-433">ASP.NET SignalR 2.1.0</span><span class="sxs-lookup"><span data-stu-id="4f2c6-433">ASP.NET SignalR 2.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.js

#### <a name="aspnet-signalr-203"></a><span data-ttu-id="4f2c6-434">ASP.NET SignalR 2.0.3</span><span class="sxs-lookup"><span data-stu-id="4f2c6-434">ASP.NET SignalR 2.0.3</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.js

#### <a name="aspnet-signalr-202"></a><span data-ttu-id="4f2c6-435">ASP.NET SignalR 2.0.2</span><span class="sxs-lookup"><span data-stu-id="4f2c6-435">ASP.NET SignalR 2.0.2</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.js

#### <a name="aspnet-signalr-201"></a><span data-ttu-id="4f2c6-436">ASP.NET SignalR 2.0.1</span><span class="sxs-lookup"><span data-stu-id="4f2c6-436">ASP.NET SignalR 2.0.1</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.js

#### <a name="aspnet-signalr-200"></a><span data-ttu-id="4f2c6-437">ASP.NET SignalR 2.0.0</span><span class="sxs-lookup"><span data-stu-id="4f2c6-437">ASP.NET SignalR 2.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.js

#### <a name="aspnet-signalr-113"></a><span data-ttu-id="4f2c6-438">ASP.NET SignalR 1.1.3</span><span class="sxs-lookup"><span data-stu-id="4f2c6-438">ASP.NET SignalR 1.1.3</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.js

#### <a name="aspnet-signalr-112"></a><span data-ttu-id="4f2c6-439">ASP.NET SignalR 1.1.2</span><span class="sxs-lookup"><span data-stu-id="4f2c6-439">ASP.NET SignalR 1.1.2</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.js

#### <a name="aspnet-signalr-111"></a><span data-ttu-id="4f2c6-440">ASP.NET SignalR 1.1.1</span><span class="sxs-lookup"><span data-stu-id="4f2c6-440">ASP.NET SignalR 1.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.js

#### <a name="aspnet-signalr-110"></a><span data-ttu-id="4f2c6-441">ASP.NET SignalR 1.1.0</span><span class="sxs-lookup"><span data-stu-id="4f2c6-441">ASP.NET SignalR 1.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.js

#### <a name="aspnet-signalr-101"></a><span data-ttu-id="4f2c6-442">ASP.NET SignalR 1.0.1</span><span class="sxs-lookup"><span data-stu-id="4f2c6-442">ASP.NET SignalR 1.0.1</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.js

<span data-ttu-id="4f2c6-443">Aby uzyskać informacji o warunkach użytkowania usługi CDN, zobacz [Microsoft Ajax CDN warunki użytkowania](https://www.asp.net/terms-of-use "Microsoft Ajax CDN warunki użytkowania").</span><span class="sxs-lookup"><span data-stu-id="4f2c6-443">For information about the terms of use for the CDN, see [Microsoft Ajax CDN Terms of Use](https://www.asp.net/terms-of-use "Microsoft Ajax CDN Terms of Use").</span></span>

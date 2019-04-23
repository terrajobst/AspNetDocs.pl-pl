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
ms.openlocfilehash: d98fc8ff72f7d32502d88329ef9d056393b92c92
ms.sourcegitcommit: 2d53ed9e4c8b19d3526cbc689bfa8394c9449cec
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/22/2019
ms.locfileid: "59905686"
---
# <a name="microsoft-ajax-content-delivery-network"></a><span data-ttu-id="c7ac9-102">Usługa Microsoft AJAX Content Delivery Network</span><span class="sxs-lookup"><span data-stu-id="c7ac9-102">Microsoft Ajax Content Delivery Network</span></span>

> [!WARNING]
> <span data-ttu-id="c7ac9-103">Aplikacje produkcyjne nie powinna przyjmować twardych zależności zasobów w sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="c7ac9-103">Production applications should not take a hard dependency on CDN assets.</span></span> <span data-ttu-id="c7ac9-104">Aplikacje powinny testu dla zasobu usługi CDN, do których odwołuje się i użyj zasób rezerwowy, gdy sieć CDN jest niedostępny.</span><span class="sxs-lookup"><span data-stu-id="c7ac9-104">Applications should test for the CDN asset referenced, and use a fallback asset when the CDN is not available.</span></span>
>
> <span data-ttu-id="c7ac9-105">Microsoft Ajax CDN jest objęta umową SLA niż przy użyciu usługi Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="c7ac9-105">The Microsoft Ajax CDN has no SLA above and beyond using an Azure CDN.</span></span>
>
> <span data-ttu-id="c7ac9-106">Użyj [problem w usłudze GitHub](https://github.com/aspnet/AspNetDocs/issues/116) na zgłaszanie problemów z Microsoft Ajax CDN.</span><span class="sxs-lookup"><span data-stu-id="c7ac9-106">Use [this GitHub issue](https://github.com/aspnet/AspNetDocs/issues/116) to report problems with the Microsoft Ajax CDN.</span></span>

## <a name="table-of-contents"></a><span data-ttu-id="c7ac9-107">Spis treści</span><span class="sxs-lookup"><span data-stu-id="c7ac9-107">Table of Contents</span></span>

<span data-ttu-id="c7ac9-108">**[zmieniona na ajax.aspnetcdn.com AJAX.microsoft.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span><span class="sxs-lookup"><span data-stu-id="c7ac9-108">**[ajax.microsoft.com renamed to ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span></span>  
<span data-ttu-id="c7ac9-109">**[Visual Studio .vsdoc Support](#Visual_Studio_vsdoc_Support_19)**</span><span class="sxs-lookup"><span data-stu-id="c7ac9-109">**[Visual Studio .vsdoc Support](#Visual_Studio_vsdoc_Support_19)**</span></span>  
<span data-ttu-id="c7ac9-110">**[Za pomocą kodu ASP.NET Ajax z sieci CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span><span class="sxs-lookup"><span data-stu-id="c7ac9-110">**[Using ASP.NET Ajax from the CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span></span>  
<span data-ttu-id="c7ac9-111">**[Przy użyciu jQuery z sieci CDN](#Using_jQuery_from_the_CDN_21)**</span><span class="sxs-lookup"><span data-stu-id="c7ac9-111">**[Using jQuery from the CDN](#Using_jQuery_from_the_CDN_21)**</span></span>  
<span data-ttu-id="c7ac9-112">**[Przy użyciu jQuery interfejsu użytkownika z usługi CDN](#Using_jQuery_UI_from_the_CDN_22)**</span><span class="sxs-lookup"><span data-stu-id="c7ac9-112">**[Using jQuery UI from the CDN](#Using_jQuery_UI_from_the_CDN_22)**</span></span>  
<span data-ttu-id="c7ac9-113">**[Pliki innych firm w usłudze CDN](#Third-Party_Files_on_the_CDN_23)**</span><span class="sxs-lookup"><span data-stu-id="c7ac9-113">**[Third-Party Files on the CDN](#Third-Party_Files_on_the_CDN_23)**</span></span>  
  
 [<span data-ttu-id="c7ac9-114">Wersje dostępne w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="c7ac9-114">jQuery Releases on the CDN</span></span>](#jQuery_Releases_on_the_CDN_0)  
 [<span data-ttu-id="c7ac9-115">Wersje jQuery migracji w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="c7ac9-115">jQuery Migrate Releases on the CDN</span></span>](#jQuery_Migrate_Releases_on_the_CDN_1)  
 [<span data-ttu-id="c7ac9-116">jQuery wersjach interfejsu użytkownika w usłudze CDN</span><span class="sxs-lookup"><span data-stu-id="c7ac9-116">jQuery UI Releases on the CDN</span></span>](#jQuery_UI_Releases_on_the_CDN_2)  
 [<span data-ttu-id="c7ac9-117">Sprawdzanie poprawności wersji w usłudze CDN technologii jQuery</span><span class="sxs-lookup"><span data-stu-id="c7ac9-117">jQuery Validation Releases on the CDN</span></span>](#jQuery_Validation_Releases_on_the_CDN_3)  
 [<span data-ttu-id="c7ac9-118">jQuery Mobile wersji w usłudze CDN</span><span class="sxs-lookup"><span data-stu-id="c7ac9-118">jQuery Mobile Releases on the CDN</span></span>](#jQuery_Mobile_Releases_on_the_CDN_4)  
 [<span data-ttu-id="c7ac9-119">jQuery szablony wydań w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="c7ac9-119">jQuery Templates Releases on the CDN</span></span>](#jQuery_Templates_Releases_on_the_CDN_5)  
 [<span data-ttu-id="c7ac9-120">jQuery cyklu wersji w usłudze CDN</span><span class="sxs-lookup"><span data-stu-id="c7ac9-120">jQuery Cycle Releases on the CDN</span></span>](#jQuery_Cycle_Releases_on_the_CDN_6)  
 [<span data-ttu-id="c7ac9-121">jQuery wersji DataTable w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="c7ac9-121">jQuery DataTables Releases on the CDN</span></span>](#jQuery_DataTables_Releases_on_the_CDN_7)  
 [<span data-ttu-id="c7ac9-122">Wersje Modernizr w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="c7ac9-122">Modernizr Releases on the CDN</span></span>](#Modernizr_Releases_on_the_CDN_8)  
 [<span data-ttu-id="c7ac9-123">Wersje JSHint w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="c7ac9-123">JSHint Releases on the CDN</span></span>](#JSHint_Releases_on_the_CDN_10)  
 [<span data-ttu-id="c7ac9-124">Wersje knockout w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="c7ac9-124">Knockout Releases on the CDN</span></span>](#Knockout_Releases_on_the_CDN_11)  
 [<span data-ttu-id="c7ac9-125">Sprzedawać wersji w usłudze CDN</span><span class="sxs-lookup"><span data-stu-id="c7ac9-125">Globalize Releases on the CDN</span></span>](#Globalize_Releases_on_the_CDN_12)  
 [<span data-ttu-id="c7ac9-126">Odpowiadać wersji w usłudze CDN</span><span class="sxs-lookup"><span data-stu-id="c7ac9-126">Respond Releases on the CDN</span></span>](#Respond_Releases_on_the_CDN_13)  
 [<span data-ttu-id="c7ac9-127">Wersje ładowania w usłudze CDN</span><span class="sxs-lookup"><span data-stu-id="c7ac9-127">Bootstrap Releases on the CDN</span></span>](#Bootstrap_Releases_on_the_CDN_14)  
 [<span data-ttu-id="c7ac9-128">Wersje TouchCarousel ładowania w usłudze CDN</span><span class="sxs-lookup"><span data-stu-id="c7ac9-128">Bootstrap TouchCarousel Releases on the CDN</span></span>](#BootstrapTouchCarousel_Releases_on_the_CDN_18)  
 [<span data-ttu-id="c7ac9-129">Wersje hammer.js w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="c7ac9-129">Hammer.js Releases on the CDN</span></span>](#Hammerjs_Releases_on_the_CDN_19)  
 [<span data-ttu-id="c7ac9-130">ASP.NET Web Forms i Ajax wersji w usłudze CDN</span><span class="sxs-lookup"><span data-stu-id="c7ac9-130">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>](#ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15)  
 [<span data-ttu-id="c7ac9-131">ASP.NET MVC udostępnia w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="c7ac9-131">ASP.NET MVC Releases on the CDN</span></span>](#ASPNET_MVC_Releases_on_the_CDN_16)  
 [<span data-ttu-id="c7ac9-132">Wersje biblioteki SignalR platformy ASP.NET, w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="c7ac9-132">ASP.NET SignalR Releases on the CDN</span></span>](#ASPNET_SignalR_Releases_on_the_CDN_17)

<span data-ttu-id="c7ac9-133">Microsoft Ajax Content Delivery Network (CDN) obsługuje popularne innej biblioteki JavaScript, np. jQuery i pozwala na łatwe dodawanie ich do aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="c7ac9-133">The Microsoft Ajax Content Delivery Network (CDN) hosts popular third party JavaScript libraries such as jQuery and enables you to easily add them to your Web applications.</span></span> <span data-ttu-id="c7ac9-134">Na przykład, można uruchomić przy użyciu jQuery, który znajduje się w tej sieci CDN, poprzez dodanie &lt;skryptu&gt; tag do strony, który wskazuje na ajax.aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="c7ac9-134">For example, you can start using jQuery which is hosted on this CDN simply by adding a &lt;script&gt; tag to your page that points to ajax.aspnetcdn.com.</span></span>

<span data-ttu-id="c7ac9-135">Dzięki wykorzystaniu usługi CDN, można znacznie zwiększyć wydajność aplikacji w technologii Ajax.</span><span class="sxs-lookup"><span data-stu-id="c7ac9-135">By taking advantage of the CDN, you can significantly improve the performance of your Ajax applications.</span></span> <span data-ttu-id="c7ac9-136">Zawartość usługi CDN są buforowane na serwerach znajdujących się na całym świecie.</span><span class="sxs-lookup"><span data-stu-id="c7ac9-136">The contents of the CDN are cached on servers located around the world.</span></span> <span data-ttu-id="c7ac9-137">Ponadto sieci CDN umożliwia ponowne użycie plików JavaScript pamięci podręcznej innych firm dla witryn sieci web, które znajdują się w różnych domenach w różnych przeglądarkach.</span><span class="sxs-lookup"><span data-stu-id="c7ac9-137">In addition, the CDN enables browsers to reuse cached third party JavaScript files for web sites that are located in different domains.</span></span>

<span data-ttu-id="c7ac9-138">Usługa CDN obsługuje protokół SSL (HTTPS), w razie potrzeby do obsługi strony sieci web przy użyciu protokołu Secure Sockets Layer.</span><span class="sxs-lookup"><span data-stu-id="c7ac9-138">The CDN supports SSL (HTTPS) in case you need to serve a web page using the Secure Sockets Layer.</span></span>

<span data-ttu-id="c7ac9-139">Usługa CDN obsługuje następujące biblioteki skryptów innych firm, które zostały przekazane i są licencjonowane, właściciele tych bibliotek:</span><span class="sxs-lookup"><span data-stu-id="c7ac9-139">The CDN hosts the following third party script libraries which have been uploaded, and are licensed to you, by the owners of those libraries:</span></span>

- <span data-ttu-id="c7ac9-140">jQuery (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="c7ac9-140">jQuery (www.jquery.com)</span></span>
- <span data-ttu-id="c7ac9-141">jQuery UI (www.jqueryui.com)</span><span class="sxs-lookup"><span data-stu-id="c7ac9-141">jQuery UI (www.jqueryui.com)</span></span>
- <span data-ttu-id="c7ac9-142">jQuery Mobile (www.jquerymobile.com)</span><span class="sxs-lookup"><span data-stu-id="c7ac9-142">jQuery Mobile (www.jquerymobile.com)</span></span>
- <span data-ttu-id="c7ac9-143">jQuery sprawdzania poprawności (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="c7ac9-143">jQuery Validation (www.jquery.com)</span></span>
- <span data-ttu-id="c7ac9-144">jQuery Cycle (www.malsup.com/jquery/cycle/)</span><span class="sxs-lookup"><span data-stu-id="c7ac9-144">jQuery Cycle (www.malsup.com/jquery/cycle/)</span></span>
- <span data-ttu-id="c7ac9-145">jQuery DataTables (http://datatables.net/)</span><span class="sxs-lookup"><span data-stu-id="c7ac9-145">jQuery DataTables (http://datatables.net/)</span></span>

<span data-ttu-id="c7ac9-146">Microsoft Ajax CDN zawiera również następujące biblioteki, które zostały przekazane przez firmę Microsoft:</span><span class="sxs-lookup"><span data-stu-id="c7ac9-146">The Microsoft Ajax CDN also includes the following libraries which have been uploaded by Microsoft:</span></span>

- <span data-ttu-id="c7ac9-147">ASP.NET AJAX</span><span class="sxs-lookup"><span data-stu-id="c7ac9-147">ASP.NET Ajax</span></span>
- <span data-ttu-id="c7ac9-148">ASP.NET MVC JavaScript Files</span><span class="sxs-lookup"><span data-stu-id="c7ac9-148">ASP.NET MVC JavaScript Files</span></span>
- <span data-ttu-id="c7ac9-149">Pliki ASP.NET SignalR JavaScript</span><span class="sxs-lookup"><span data-stu-id="c7ac9-149">ASP.NET SignalR JavaScript Files</span></span>

<span data-ttu-id="c7ac9-150">Microsoft zastrzega sobie prawa własności do żadnych bibliotek innych firm, hostowanych w tej sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="c7ac9-150">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="c7ac9-151">Właściciele praw autorskich bibliotek korzystają z licencji tych bibliotek do Ciebie.</span><span class="sxs-lookup"><span data-stu-id="c7ac9-151">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="c7ac9-152">Wszelkie prawa, które może być konieczne pobranie i używanie biblioteki są przyznawane wyłącznie przez właścicieli praw autorskich.</span><span class="sxs-lookup"><span data-stu-id="c7ac9-152">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="c7ac9-153">Ponieważ nie są to biblioteki Microsoft, firma Microsoft oferuje żadnych gwarancji ani licencji praw własności intelektualnej (w tym nie domyślnych praw patentowych) bibliotek innych firm, hostowanych w tej sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="c7ac9-153">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<span data-ttu-id="c7ac9-154">Jeśli chcesz przesłać biblioteki JavaScript i biblioteka jest jednym z najważniejszych biblioteki JavaScript (zgodnie z opisem na http://trends.builtwith.com) lub rozszerzeń/wtyczek do tych bibliotek, które () popularnych; lub (b) przydatne do użytku na platformie ASP.NET, a następnie skontaktuj się z pomocą AjaxCDNSubmission@Microsoft.com.</span><span class="sxs-lookup"><span data-stu-id="c7ac9-154">If you wish to submit your JavaScript library and your library is one of the top JavaScript libraries (as listed on http://trends.builtwith.com) or extensions/plugins to these libraries that are (a) popular; or (b) helpful for use on ASP.NET then please contact AjaxCDNSubmission@Microsoft.com.</span></span>

<a id="ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18"></a>

## <a name="ajaxmicrosoftcom-renamed-to-ajaxaspnetcdncom"></a><span data-ttu-id="c7ac9-155">zmieniona na ajax.aspnetcdn.com AJAX.microsoft.com</span><span class="sxs-lookup"><span data-stu-id="c7ac9-155">ajax.microsoft.com renamed to ajax.aspnetcdn.com</span></span>

<span data-ttu-id="c7ac9-156">Sieć CDN używany do używania nazwy domeny w domenie microsoft.com i został zmieniony tak, aby użyć nazwy domeny aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="c7ac9-156">The CDN used to use the microsoft.com domain name and has been changed to use the aspnetcdn.com domain name.</span></span> <span data-ttu-id="c7ac9-157">Ta zmiana została wprowadzona w celu zwiększenia wydajności, ponieważ gdy przeglądarkę w domenie microsoft.com, do których odwołuje się on prześle pliki cookie z tej domeny faktycznie z każdym żądaniem.</span><span class="sxs-lookup"><span data-stu-id="c7ac9-157">This change was made to increase performance because when a browser referenced the microsoft.com domain it would send any cookies from that domain across the wire with each request.</span></span> <span data-ttu-id="c7ac9-158">Przez zmianę nazwy, aby wskazywała nazwę domeny inne niż microsoft.com można zwiększyć wydajność przez tyle 25%.</span><span class="sxs-lookup"><span data-stu-id="c7ac9-158">By renaming to a domain name other than microsoft.com performance can be increased by as much to 25%.</span></span> <span data-ttu-id="c7ac9-159">Należy pamiętać, ajax.microsoft.com będą nadal działać, ale ajax.aspnetcdn.com jest zalecane.</span><span class="sxs-lookup"><span data-stu-id="c7ac9-159">Note ajax.microsoft.com will continue to function but ajax.aspnetcdn.com is recommended.</span></span>

- <span data-ttu-id="c7ac9-160">Stary Format: https://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="c7ac9-160">Old Format: https://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span></span>
- <span data-ttu-id="c7ac9-161">Nowy Format: https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="c7ac9-161">New Format: https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span></span>

<a id="Visual_Studio_vsdoc_Support_19"></a>

## <a name="visual-studio-vsdoc-support"></a><span data-ttu-id="c7ac9-162">Obsługa .vsdoc programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c7ac9-162">Visual Studio .vsdoc Support</span></span>

<span data-ttu-id="c7ac9-163">Do korzystania z plików .vsdoc poprawnie za pomocą programu Visual Studio 2008, należy upewnić się, że program VS 2008 z dodatkiem SP1 należy zainstalować i zainstalowana poprawka vsdoc plików.</span><span class="sxs-lookup"><span data-stu-id="c7ac9-163">To use the .vsdoc files properly with Visual Studio 2008 you need to make sure that you have VS 2008 SP1 installed and the hotfix for vsdoc files installed.</span></span> <span data-ttu-id="c7ac9-164">Możesz uzyskać je w tym miejscu:</span><span class="sxs-lookup"><span data-stu-id="c7ac9-164">You can get these from here:</span></span>

- [<span data-ttu-id="c7ac9-165">Pobierz program Visual Studio 2008 z dodatkiem SP1</span><span class="sxs-lookup"><span data-stu-id="c7ac9-165">Download Visual Studio 2008 SP1</span></span>](https://www.microsoft.com/downloads/en/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en "Pobierz program Visual Studio 2008 z dodatkiem SP1")
- [<span data-ttu-id="c7ac9-166">Pobierz .vsdoc poprawki dla programu Visual Studio 2008 z dodatkiem SP1</span><span class="sxs-lookup"><span data-stu-id="c7ac9-166">Download .vsdoc hotfix for Visual Studio 2008 SP1</span></span>](https://code.msdn.microsoft.com/KB958502/Release/ProjectReleases.aspx?ReleaseId=1736 "Pobierz .vsdoc poprawki dla programu Visual Studio 2008 z dodatkiem SP1")

<span data-ttu-id="c7ac9-167">Program Visual Studio 2010 obsługuje pliki .vsdoc bez żadnych dodatkowych poprawek.</span><span class="sxs-lookup"><span data-stu-id="c7ac9-167">Visual Studio 2010 supports .vsdoc files without any additional patches.</span></span>

<a id="Using_ASPNET_Ajax_from_the_CDN_20"></a>

## <a name="using-aspnet-ajax-from-the-cdn"></a><span data-ttu-id="c7ac9-168">Za pomocą kodu ASP.NET Ajax z sieci CDN</span><span class="sxs-lookup"><span data-stu-id="c7ac9-168">Using ASP.NET Ajax from the CDN</span></span>

<span data-ttu-id="c7ac9-169">Korzystając z platformy ASP.NET 4, można przekierować wszystkie żądania ASP.NET framework skryptów do sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="c7ac9-169">When using ASP.NET 4, you can redirect all requests for ASP.NET framework scripts to the CDN.</span></span> <span data-ttu-id="c7ac9-170">Pobieranie skryptów z sieci CDN, zamiast z lokalnego serwera internetowego może znacznie zwiększyć wydajność publicznych witryn sieci Web, ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c7ac9-170">Retrieving scripts from the CDN instead of your local web server can substantially improve the performance of public ASP.NET websites.</span></span>

<span data-ttu-id="c7ac9-171">Aby przekierować wszystkie żądania programu ASP.NET framework skryptu do Microsoft Ajax CDN, należy użyć właściwości ScriptManager EnableCDN:</span><span class="sxs-lookup"><span data-stu-id="c7ac9-171">Use the ScriptManager EnableCDN property to redirect all ASP.NET framework script requests to the Microsoft Ajax CDN:</span></span>

[!code-aspx[Main](overview/samples/sample1.aspx)]

<a id="Using_jQuery_from_the_CDN_21"></a>

## <a name="using-jquery-from-the-cdn"></a><span data-ttu-id="c7ac9-172">Przy użyciu jQuery z sieci CDN</span><span class="sxs-lookup"><span data-stu-id="c7ac9-172">Using jQuery from the CDN</span></span>

<span data-ttu-id="c7ac9-173">Można użyć skryptów jQuery hostowanej w usłudze CDN w aplikacji sieci Web, dodając następujący element skryptu na stronie:</span><span class="sxs-lookup"><span data-stu-id="c7ac9-173">You can use jQuery scripts hosted on CDN in your Web application by adding the following script element to a page:</span></span>

[!code-html[Main](overview/samples/sample2.html)]

<span data-ttu-id="c7ac9-174">Sieć CDN obejmuje również zminimalizowany wersji skryptu jQuery, którą możesz pobrać przy użyciu następującego elementu:</span><span class="sxs-lookup"><span data-stu-id="c7ac9-174">The CDN also includes the minified version of the jQuery script, which you can get using the following element:</span></span>

[!code-html[Main](overview/samples/sample3.html)]

<span data-ttu-id="c7ac9-175">Aby zezwolić na stronie jest zastępowany na potrzeby ładowania jQuery ze ścieżki lokalnej we własnej witrynie internetowej, jeśli sieć CDN stanie się ona niedostępna, Dodaj następujący element bezpośrednio po elemencie odwołujące się do sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="c7ac9-175">To allow your page to fallback to loading jQuery from a local path on your own website if the CDN happens to be unavailable, add the following element immediately after the element referencing the CDN:</span></span>

[!code-html[Main](overview/samples/sample4.html)]

<span data-ttu-id="c7ac9-176">Poniższej przykładowej strony użyto CDN wersję biblioteki jQuery (za pomocą rezerwowe w lokalnej kopii), aby wyświetlić zawartość elementu div po kliknięciu przycisku.</span><span class="sxs-lookup"><span data-stu-id="c7ac9-176">The following sample page uses the CDN version of the jQuery library (with fallback to a local copy) to display the contents of a div element when a button is clicked.</span></span>

[!code-html[Main](overview/samples/sample5.html)]

<span data-ttu-id="c7ac9-177">Możesz dowiedzieć się więcej o technologii jQuery i pobrać kopię lokalną jQuery, odwiedzając [jQuery](http://jquery.com/) witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="c7ac9-177">You can learn more about jQuery and download a local copy of jQuery by visiting the [jQuery](http://jquery.com/) Web site.</span></span>

<a id="Using_jQuery_UI_from_the_CDN_22"></a>

## <a name="using-jquery-ui-from-the-cdn"></a><span data-ttu-id="c7ac9-178">Przy użyciu jQuery interfejsu użytkownika z usługi CDN</span><span class="sxs-lookup"><span data-stu-id="c7ac9-178">Using jQuery UI from the CDN</span></span>

<span data-ttu-id="c7ac9-179">Usługa CDN obsługuje również biblioteki interfejsu użytkownika jQuery.</span><span class="sxs-lookup"><span data-stu-id="c7ac9-179">The CDN also hosts the jQuery UI library.</span></span> <span data-ttu-id="c7ac9-180">Biblioteka interfejsu użytkownika jQuery zawiera bogaty zestaw elementów widget i efekty, których można używać w aplikacjach ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c7ac9-180">The jQuery UI library includes a rich set of widgets and effects that you can use in your ASP.NET applications.</span></span> <span data-ttu-id="c7ac9-181">Na przykład następująca strona ilustruje, jak można użyć selektora daty interfejsu użytkownika jQuery w kontekście aplikacji formularzy sieci Web ASP.NET do wyświetlenia wyskakującego kalendarza:</span><span class="sxs-lookup"><span data-stu-id="c7ac9-181">For example, the following page illustrates how you can use the jQuery UI Datepicker in the context of an ASP.NET Web Forms application to display a pop-up calendar:</span></span>

[!code-aspx[Main](overview/samples/sample6.aspx)]

<span data-ttu-id="c7ac9-182">Po przeniesieniu fokusu do pola tekstowego, za pomocą klawiatury, zostanie wyświetlona kalendarza:</span><span class="sxs-lookup"><span data-stu-id="c7ac9-182">When you move focus to the TextBox using your keyboard, a calendar is displayed:</span></span>

![Kalendarz podręczny utworzonych za pomocą selektora daty](overview/_static/image1.png)

<span data-ttu-id="c7ac9-184">Zwróć uwagę, że musi zawierać trzy pliki z usługi CDN w powyższym kodzie:</span><span class="sxs-lookup"><span data-stu-id="c7ac9-184">Notice that you must include three files from the CDN in the code above:</span></span>

- <span data-ttu-id="c7ac9-185">Biblioteka jQuery &mdash; biblioteki interfejsu użytkownika jQuery jest zależna od biblioteki jQuery.</span><span class="sxs-lookup"><span data-stu-id="c7ac9-185">The jQuery library &mdash; The jQuery UI library depends on the jQuery library.</span></span> <span data-ttu-id="c7ac9-186">Przed dodaniem biblioteki interfejsu użytkownika jQuery, należy dodać do strony w bibliotece jQuery.</span><span class="sxs-lookup"><span data-stu-id="c7ac9-186">You must add the jQuery library to your page before you add the jQuery UI library.</span></span>
- <span data-ttu-id="c7ac9-187">Biblioteka interfejsu użytkownika jQuery &mdash; biblioteki interfejsu użytkownika jQuery zawiera wszystkie efekty interfejsu użytkownika jQuery i widżetów, takich jak widżet Datepicker używanych w powyższych strony.</span><span class="sxs-lookup"><span data-stu-id="c7ac9-187">The jQuery UI library &mdash; The jQuery UI library contains all of the jQuery UI effects and widgets such as the Datepicker widget used in the page above.</span></span>
- <span data-ttu-id="c7ac9-188">Motyw interfejsu użytkownika jQuery &mdash; interfejs użytkownika jQuery obsługuje różne tematy.</span><span class="sxs-lookup"><span data-stu-id="c7ac9-188">A jQuery UI theme &mdash; The jQuery UI supports different themes.</span></span> <span data-ttu-id="c7ac9-189">Strona powyżej zawiera link do pliku CSS, aby zaimportować motywu Redmond.</span><span class="sxs-lookup"><span data-stu-id="c7ac9-189">The page above includes a link to a CSS file to import the Redmond theme.</span></span>

<span data-ttu-id="c7ac9-190">Wszystkie motywy interfejsu użytkownika jQuery standardowe są hostowane w sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="c7ac9-190">All of the standard jQuery UI themes are hosted on the CDN.</span></span> <span data-ttu-id="c7ac9-191">[Odwiedź tę stronę](jquery-ui/cdnjqueryui1910.md "interfejs użytkownika jQuery 1.8.10 w usłudze Microsoft Ajax CDN") do wyświetlania miniatur dla każdego motywu.</span><span class="sxs-lookup"><span data-stu-id="c7ac9-191">[Visit this page](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 on the Microsoft Ajax CDN") to view thumbnails for each theme.</span></span>

<span data-ttu-id="c7ac9-192">Aby dowiedzieć się więcej na temat biblioteki interfejsu użytkownika jQuery, odwiedź oficjalną [interfejsu użytkownika jQuery witryny sieci Web](http://jQueryUI.com "interfejsu użytkownika jQuery witryny sieci Web").</span><span class="sxs-lookup"><span data-stu-id="c7ac9-192">To learn more about the jQuery UI library, visit the official [jQuery UI website](http://jQueryUI.com "jQuery UI website").</span></span>

<a id="Third-Party_Files_on_the_CDN_23"></a>

## <a name="third-party-files-on-the-cdn"></a><span data-ttu-id="c7ac9-193">Pliki innych firm w usłudze CDN</span><span class="sxs-lookup"><span data-stu-id="c7ac9-193">Third-Party Files on the CDN</span></span>

<span data-ttu-id="c7ac9-194">Usługa CDN obsługuje niektóre najpopularniejsze biblioteki języka JavaScript innych firm.</span><span class="sxs-lookup"><span data-stu-id="c7ac9-194">The CDN hosts some of the most popular third party JavaScript libraries.</span></span> <span data-ttu-id="c7ac9-195">Microsoft zastrzega sobie prawa własności do żadnych bibliotek innych firm, hostowanych w tej sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="c7ac9-195">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="c7ac9-196">Właściciele praw autorskich bibliotek korzystają z licencji tych bibliotek do Ciebie.</span><span class="sxs-lookup"><span data-stu-id="c7ac9-196">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="c7ac9-197">Wszelkie prawa, które może być konieczne pobranie i używanie biblioteki są przyznawane wyłącznie przez właścicieli praw autorskich.</span><span class="sxs-lookup"><span data-stu-id="c7ac9-197">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="c7ac9-198">Ponieważ nie są to biblioteki Microsoft, firma Microsoft oferuje żadnych gwarancji ani licencji praw własności intelektualnej (w tym nie domyślnych praw patentowych) bibliotek innych firm, hostowanych w tej sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="c7ac9-198">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<a id="jQuery_Releases_on_the_CDN_0"></a>

### <a name="jquery-releases-on-the-cdn"></a><span data-ttu-id="c7ac9-199">Wersje dostępne w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="c7ac9-199">jQuery Releases on the CDN</span></span>

<span data-ttu-id="c7ac9-200">Następujące wersje jQuery znajdują się w sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="c7ac9-200">The following releases of jQuery are hosted on the CDN:</span></span>

#### <a name="jquery-version-340"></a><span data-ttu-id="c7ac9-201">Wersja jQuery 3.4.0</span><span class="sxs-lookup"><span data-stu-id="c7ac9-201">jQuery version 3.4.0</span></span>
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.0.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.0.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.0.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.0.slim.min.map

#### <a name="jquery-version-331"></a><span data-ttu-id="c7ac9-202">w wersji 3.3.1 jQuery</span><span class="sxs-lookup"><span data-stu-id="c7ac9-202">jQuery version 3.3.1</span></span>
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.map

#### <a name="jquery-version-321"></a><span data-ttu-id="c7ac9-203">jQuery, wersja 3.2.1</span><span class="sxs-lookup"><span data-stu-id="c7ac9-203">jQuery version 3.2.1</span></span>
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.map

#### <a name="jquery-version-320"></a><span data-ttu-id="c7ac9-204">Wersja jQuery 3.2.0</span><span class="sxs-lookup"><span data-stu-id="c7ac9-204">jQuery version 3.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.map

#### <a name="jquery-version-311"></a><span data-ttu-id="c7ac9-205">Wersja jQuery 3.1.1</span><span class="sxs-lookup"><span data-stu-id="c7ac9-205">jQuery version 3.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.map

#### <a name="jquery-version-310"></a><span data-ttu-id="c7ac9-206">Wersja jQuery 3.1.0</span><span class="sxs-lookup"><span data-stu-id="c7ac9-206">jQuery version 3.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.map

#### <a name="jquery-version-300"></a><span data-ttu-id="c7ac9-207">jQuery w wersji 3.0.0</span><span class="sxs-lookup"><span data-stu-id="c7ac9-207">jQuery version 3.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.map

#### <a name="jquery-version-224"></a><span data-ttu-id="c7ac9-208">Wersja jQuery 2.2.4</span><span class="sxs-lookup"><span data-stu-id="c7ac9-208">jQuery version 2.2.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.map

#### <a name="jquery-version-223"></a><span data-ttu-id="c7ac9-209">Wersja jQuery 2.2.3</span><span class="sxs-lookup"><span data-stu-id="c7ac9-209">jQuery version 2.2.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.map

#### <a name="jquery-version-222"></a><span data-ttu-id="c7ac9-210">Wersja jQuery 2.2.2</span><span class="sxs-lookup"><span data-stu-id="c7ac9-210">jQuery version 2.2.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.map

#### <a name="jquery-version-221"></a><span data-ttu-id="c7ac9-211">jQuery wersję 2.2.1</span><span class="sxs-lookup"><span data-stu-id="c7ac9-211">jQuery version 2.2.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.map

#### <a name="jquery-version-220"></a><span data-ttu-id="c7ac9-212">Wersja jQuery 2.2.0</span><span class="sxs-lookup"><span data-stu-id="c7ac9-212">jQuery version 2.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.map

#### <a name="jquery-version-214"></a><span data-ttu-id="c7ac9-213">jQuery wersji 2.1.4</span><span class="sxs-lookup"><span data-stu-id="c7ac9-213">jQuery version 2.1.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.map

#### <a name="jquery-version-213"></a><span data-ttu-id="c7ac9-214">Wersja jQuery 2.1.3</span><span class="sxs-lookup"><span data-stu-id="c7ac9-214">jQuery version 2.1.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.map

#### <a name="jquery-version-212"></a><span data-ttu-id="c7ac9-215">jQuery, wersja 2.1.2</span><span class="sxs-lookup"><span data-stu-id="c7ac9-215">jQuery version 2.1.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.min.js

#### <a name="jquery-version-211"></a><span data-ttu-id="c7ac9-216">Wersja jQuery 2.1.1</span><span class="sxs-lookup"><span data-stu-id="c7ac9-216">jQuery version 2.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.map

#### <a name="jquery-version-210"></a><span data-ttu-id="c7ac9-217">jQuery wersja 2.1.0</span><span class="sxs-lookup"><span data-stu-id="c7ac9-217">jQuery version 2.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.map

#### <a name="jquery-version-203"></a><span data-ttu-id="c7ac9-218">jQuery wersji 2.0.3</span><span class="sxs-lookup"><span data-stu-id="c7ac9-218">jQuery version 2.0.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.map

#### <a name="jquery-version-202"></a><span data-ttu-id="c7ac9-219">Wersja jQuery pkt 2.0.2</span><span class="sxs-lookup"><span data-stu-id="c7ac9-219">jQuery version 2.0.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.map

#### <a name="jquery-version-201"></a><span data-ttu-id="c7ac9-220">Wersja jQuery 2.0.1</span><span class="sxs-lookup"><span data-stu-id="c7ac9-220">jQuery version 2.0.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.map

#### <a name="jquery-version-200"></a><span data-ttu-id="c7ac9-221">w wersji 2.0.0 jQuery</span><span class="sxs-lookup"><span data-stu-id="c7ac9-221">jQuery version 2.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.map

#### <a name="jquery-version-1124"></a><span data-ttu-id="c7ac9-222">Wersja jQuery 1.12.4</span><span class="sxs-lookup"><span data-stu-id="c7ac9-222">jQuery version 1.12.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.map

#### <a name="jquery-version-1123"></a><span data-ttu-id="c7ac9-223">Wersja jQuery 1.12.3</span><span class="sxs-lookup"><span data-stu-id="c7ac9-223">jQuery version 1.12.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.map

#### <a name="jquery-version-1122"></a><span data-ttu-id="c7ac9-224">Wersja jQuery 1.12.2</span><span class="sxs-lookup"><span data-stu-id="c7ac9-224">jQuery version 1.12.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.map

#### <a name="jquery-version-1121"></a><span data-ttu-id="c7ac9-225">Wersja jQuery 1.12.1</span><span class="sxs-lookup"><span data-stu-id="c7ac9-225">jQuery version 1.12.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.map

#### <a name="jquery-version-1120"></a><span data-ttu-id="c7ac9-226">Wersja jQuery 1.12.0</span><span class="sxs-lookup"><span data-stu-id="c7ac9-226">jQuery version 1.12.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.map

#### <a name="jquery-version-1113"></a><span data-ttu-id="c7ac9-227">Wersja jQuery 1.11.3</span><span class="sxs-lookup"><span data-stu-id="c7ac9-227">jQuery version 1.11.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.map

#### <a name="jquery-version-1112"></a><span data-ttu-id="c7ac9-228">Wersja jQuery 1.11.2</span><span class="sxs-lookup"><span data-stu-id="c7ac9-228">jQuery version 1.11.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.map

#### <a name="jquery-version-1111"></a><span data-ttu-id="c7ac9-229">Wersja jQuery 1.11.1</span><span class="sxs-lookup"><span data-stu-id="c7ac9-229">jQuery version 1.11.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.map

#### <a name="jquery-version-1110"></a><span data-ttu-id="c7ac9-230">Wersja jQuery 1.11.0</span><span class="sxs-lookup"><span data-stu-id="c7ac9-230">jQuery version 1.11.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.map

#### <a name="jquery-version-1102"></a><span data-ttu-id="c7ac9-231">Wersja jQuery 1.10.2</span><span class="sxs-lookup"><span data-stu-id="c7ac9-231">jQuery version 1.10.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.map

#### <a name="jquery-version-1101"></a><span data-ttu-id="c7ac9-232">Wersja jQuery 1.10.1</span><span class="sxs-lookup"><span data-stu-id="c7ac9-232">jQuery version 1.10.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.map

#### <a name="jquery-version-1100"></a><span data-ttu-id="c7ac9-233">Wersja jQuery 1.10.0</span><span class="sxs-lookup"><span data-stu-id="c7ac9-233">jQuery version 1.10.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.map

#### <a name="jquery-version-191"></a><span data-ttu-id="c7ac9-234">Wersja jQuery 1.9.1</span><span class="sxs-lookup"><span data-stu-id="c7ac9-234">jQuery version 1.9.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.map

#### <a name="jquery-version-190"></a><span data-ttu-id="c7ac9-235">Wersja jQuery 1.9.0</span><span class="sxs-lookup"><span data-stu-id="c7ac9-235">jQuery version 1.9.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.map

#### <a name="jquery-version-183"></a><span data-ttu-id="c7ac9-236">Wersja jQuery 1.8.3</span><span class="sxs-lookup"><span data-stu-id="c7ac9-236">jQuery version 1.8.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3-vsdoc.js

#### <a name="jquery-version-182"></a><span data-ttu-id="c7ac9-237">Wersja jQuery 1.8.2</span><span class="sxs-lookup"><span data-stu-id="c7ac9-237">jQuery version 1.8.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2-vsdoc.js

#### <a name="jquery-version-181"></a><span data-ttu-id="c7ac9-238">jQuery wersja 1.8.1</span><span class="sxs-lookup"><span data-stu-id="c7ac9-238">jQuery version 1.8.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1-vsdoc.js

#### <a name="jquery-version-180"></a><span data-ttu-id="c7ac9-239">Wersja jQuery 1.8.0</span><span class="sxs-lookup"><span data-stu-id="c7ac9-239">jQuery version 1.8.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0-vsdoc.js

#### <a name="jquery-version-172"></a><span data-ttu-id="c7ac9-240">Wersja jQuery 1.7.2</span><span class="sxs-lookup"><span data-stu-id="c7ac9-240">jQuery version 1.7.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.min.js

#### <a name="jquery-version-171"></a><span data-ttu-id="c7ac9-241">jQuery wersji 1.7.1</span><span class="sxs-lookup"><span data-stu-id="c7ac9-241">jQuery version 1.7.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1-vsdoc.js

#### <a name="jquery-version-17"></a><span data-ttu-id="c7ac9-242">jQuery w wersji 1.7</span><span class="sxs-lookup"><span data-stu-id="c7ac9-242">jQuery version 1.7</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7-vsdoc.js

#### <a name="jquery-version-164"></a><span data-ttu-id="c7ac9-243">Wersja jQuery 1.6.4</span><span class="sxs-lookup"><span data-stu-id="c7ac9-243">jQuery version 1.6.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4-vsdoc.js

#### <a name="jquery-version-163"></a><span data-ttu-id="c7ac9-244">Wersja jQuery 1.6.3</span><span class="sxs-lookup"><span data-stu-id="c7ac9-244">jQuery version 1.6.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3-vsdoc.js

#### <a name="jquery-version-162"></a><span data-ttu-id="c7ac9-245">jQuery wersji 1.6.2</span><span class="sxs-lookup"><span data-stu-id="c7ac9-245">jQuery version 1.6.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2-vsdoc.js

#### <a name="jquery-version-161"></a><span data-ttu-id="c7ac9-246">jQuery, wersja 1.6.1</span><span class="sxs-lookup"><span data-stu-id="c7ac9-246">jQuery version 1.6.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1-vsdoc.js

#### <a name="jquery-version-16"></a><span data-ttu-id="c7ac9-247">jQuery w wersji 1.6</span><span class="sxs-lookup"><span data-stu-id="c7ac9-247">jQuery version 1.6</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6-vsdoc.js

#### <a name="jquery-version-152"></a><span data-ttu-id="c7ac9-248">Wersja jQuery 1.5.2</span><span class="sxs-lookup"><span data-stu-id="c7ac9-248">jQuery version 1.5.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2-vsdoc.js

#### <a name="jquery-version-151"></a><span data-ttu-id="c7ac9-249">Wersja jQuery 1.5.1</span><span class="sxs-lookup"><span data-stu-id="c7ac9-249">jQuery version 1.5.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1-vsdoc.js

#### <a name="jquery-version-15"></a><span data-ttu-id="c7ac9-250">jQuery w wersji 1.5</span><span class="sxs-lookup"><span data-stu-id="c7ac9-250">jQuery version 1.5</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5-vsdoc.js

#### <a name="jquery-version-144"></a><span data-ttu-id="c7ac9-251">Wersja jQuery 1.4.4</span><span class="sxs-lookup"><span data-stu-id="c7ac9-251">jQuery version 1.4.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4-vsdoc.js

#### <a name="jquery-version-143"></a><span data-ttu-id="c7ac9-252">Wersja jQuery 1.4.3</span><span class="sxs-lookup"><span data-stu-id="c7ac9-252">jQuery version 1.4.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3-vsdoc.js

#### <a name="jquery-version-142"></a><span data-ttu-id="c7ac9-253">w wersji 1.4.2 jQuery</span><span class="sxs-lookup"><span data-stu-id="c7ac9-253">jQuery version 1.4.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2-vsdoc.js

#### <a name="jquery-version-141"></a><span data-ttu-id="c7ac9-254">Wersja jQuery 1.4.1</span><span class="sxs-lookup"><span data-stu-id="c7ac9-254">jQuery version 1.4.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1-vsdoc.js

#### <a name="jquery-version-14"></a><span data-ttu-id="c7ac9-255">jQuery w wersji 1.4</span><span class="sxs-lookup"><span data-stu-id="c7ac9-255">jQuery version 1.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.min.js

#### <a name="jquery-version-132"></a><span data-ttu-id="c7ac9-256">w wersji 1.3.2 jQuery</span><span class="sxs-lookup"><span data-stu-id="c7ac9-256">jQuery version 1.3.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min-vsdoc.js

<a id="jQuery_Migrate_Releases_on_the_CDN_1"></a>

### <a name="jquery-migrate-releases-on-the-cdn"></a><span data-ttu-id="c7ac9-257">Wersje jQuery migracji w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="c7ac9-257">jQuery Migrate Releases on the CDN</span></span>

<span data-ttu-id="c7ac9-258">Następujące wersje jQuery migracji znajdują się w sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="c7ac9-258">The following releases of jQuery Migrate are hosted on the CDN:</span></span>

#### <a name="jquery-migrate-version-300"></a><span data-ttu-id="c7ac9-259">jQuery migracji wersji 3.0.0</span><span class="sxs-lookup"><span data-stu-id="c7ac9-259">jQuery Migrate version 3.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.min.js

#### <a name="jquery-migrate-version-121"></a><span data-ttu-id="c7ac9-260">jQuery migracji wersji 1.2.1</span><span class="sxs-lookup"><span data-stu-id="c7ac9-260">jQuery Migrate version 1.2.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.min.js

<span data-ttu-id="c7ac9-261">jQuery migracji wersji 1.2.0 lub nowszej</span><span class="sxs-lookup"><span data-stu-id="c7ac9-261">jQuery Migrate version 1.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.min.js

#### <a name="jquery-migrate-version-111"></a><span data-ttu-id="c7ac9-262">Migrowanie wersji 1.1.1 jQuery</span><span class="sxs-lookup"><span data-stu-id="c7ac9-262">jQuery Migrate version 1.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.min.js

#### <a name="jquery-migrate-version-110"></a><span data-ttu-id="c7ac9-263">jQuery migracji wersji 1.1.0</span><span class="sxs-lookup"><span data-stu-id="c7ac9-263">jQuery Migrate version 1.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.min.js

#### <a name="jquery-migrate-version-100"></a><span data-ttu-id="c7ac9-264">jQuery migracji wersji 1.0.0</span><span class="sxs-lookup"><span data-stu-id="c7ac9-264">jQuery Migrate version 1.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.min.js

<a id="jQuery_UI_Releases_on_the_CDN_2"></a>

### <a name="jquery-ui-releases-on-the-cdn"></a><span data-ttu-id="c7ac9-265">jQuery wersjach interfejsu użytkownika w usłudze CDN</span><span class="sxs-lookup"><span data-stu-id="c7ac9-265">jQuery UI Releases on the CDN</span></span>

<span data-ttu-id="c7ac9-266">Następujące wersje biblioteki interfejsu użytkownika jQuery znajdują się w tej sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="c7ac9-266">The following releases of the jQuery UI library are hosted on this CDN.</span></span> <span data-ttu-id="c7ac9-267">Kliknij każdy link, aby wyświetlić rzeczywiste listę plików.</span><span class="sxs-lookup"><span data-stu-id="c7ac9-267">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="c7ac9-268">Interfejs użytkownika jQuery 1.12.1</span><span class="sxs-lookup"><span data-stu-id="c7ac9-268">jQuery UI 1.12.1</span></span>](jquery-ui/cdnjqueryui1121.md "interfejs użytkownika jQuery 1.12.1 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="c7ac9-269">Interfejs użytkownika jQuery 1.12.0</span><span class="sxs-lookup"><span data-stu-id="c7ac9-269">jQuery UI 1.12.0</span></span>](jquery-ui/cdnjqueryui1120.md "interfejs użytkownika jQuery 1.12.0 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="c7ac9-270">Interfejs użytkownika jQuery 1.11.4</span><span class="sxs-lookup"><span data-stu-id="c7ac9-270">jQuery UI 1.11.4</span></span>](jquery-ui/cdnjqueryui1114.md "interfejs użytkownika jQuery 1.11.4 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="c7ac9-271">Interfejs użytkownika jQuery 1.11.3</span><span class="sxs-lookup"><span data-stu-id="c7ac9-271">jQuery UI 1.11.3</span></span>](jquery-ui/cdnjqueryui1113.md "interfejs użytkownika jQuery 1.11.3 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="c7ac9-272">Interfejs użytkownika jQuery 1.11.2</span><span class="sxs-lookup"><span data-stu-id="c7ac9-272">jQuery UI 1.11.2</span></span>](jquery-ui/cdnjqueryui1112.md "interfejs użytkownika jQuery 1.11.2 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="c7ac9-273">Interfejs użytkownika jQuery 1.11.1</span><span class="sxs-lookup"><span data-stu-id="c7ac9-273">jQuery UI 1.11.1</span></span>](jquery-ui/cdnjqueryui1111.md "interfejs użytkownika jQuery 1.11.1 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="c7ac9-274">Interfejs użytkownika jQuery 1.11.0</span><span class="sxs-lookup"><span data-stu-id="c7ac9-274">jQuery UI 1.11.0</span></span>](jquery-ui/cdnjqueryui1110.md "interfejs użytkownika jQuery 1.11.0 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="c7ac9-275">Interfejs użytkownika jQuery 1.10.4</span><span class="sxs-lookup"><span data-stu-id="c7ac9-275">jQuery UI 1.10.4</span></span>](jquery-ui/cdnjqueryui1104.md "interfejs użytkownika jQuery 1.10.4 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="c7ac9-276">Interfejs użytkownika jQuery 1.10.3</span><span class="sxs-lookup"><span data-stu-id="c7ac9-276">jQuery UI 1.10.3</span></span>](jquery-ui/cdnjqueryui1103.md "interfejs użytkownika jQuery 1.10.3 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="c7ac9-277">Interfejs użytkownika jQuery 1.10.2</span><span class="sxs-lookup"><span data-stu-id="c7ac9-277">jQuery UI 1.10.2</span></span>](jquery-ui/cdnjqueryui1102.md "interfejs użytkownika jQuery 1.10.2 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="c7ac9-278">Interfejs użytkownika jQuery 1.10.1</span><span class="sxs-lookup"><span data-stu-id="c7ac9-278">jQuery UI 1.10.1</span></span>](jquery-ui/cdnjqueryui1101.md "interfejs użytkownika jQuery 1.10.1 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="c7ac9-279">Interfejs użytkownika jQuery 1.10.0</span><span class="sxs-lookup"><span data-stu-id="c7ac9-279">jQuery UI 1.10.0</span></span>](jquery-ui/cdnjqueryui1100.md "interfejs użytkownika jQuery 1.10.0 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="c7ac9-280">Interfejs użytkownika jQuery 1.9.2</span><span class="sxs-lookup"><span data-stu-id="c7ac9-280">jQuery UI 1.9.2</span></span>](jquery-ui/cdnjqueryui192.md "interfejs użytkownika jQuery 1.9.2 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="c7ac9-281">Interfejs użytkownika jQuery 1.9.1</span><span class="sxs-lookup"><span data-stu-id="c7ac9-281">jQuery UI 1.9.1</span></span>](jquery-ui/cdnjqueryui191.md "interfejs użytkownika jQuery 1.9.1 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="c7ac9-282">Interfejs użytkownika jQuery 1.9.0</span><span class="sxs-lookup"><span data-stu-id="c7ac9-282">jQuery UI 1.9.0</span></span>](jquery-ui/cdnjqueryui190.md "interfejs użytkownika jQuery 1.9.0 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="c7ac9-283">Interfejs użytkownika jQuery 1.8.24</span><span class="sxs-lookup"><span data-stu-id="c7ac9-283">jQuery UI 1.8.24</span></span>](jquery-ui/cdnjqueryui1824.md "interfejs użytkownika jQuery 1.8.24 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="c7ac9-284">Interfejs użytkownika jQuery 1.8.23</span><span class="sxs-lookup"><span data-stu-id="c7ac9-284">jQuery UI 1.8.23</span></span>](jquery-ui/cdnjqueryui1823.md "interfejs użytkownika jQuery 1.8.23 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="c7ac9-285">Interfejs użytkownika jQuery 1.8.22</span><span class="sxs-lookup"><span data-stu-id="c7ac9-285">jQuery UI 1.8.22</span></span>](jquery-ui/cdnjqueryui1822.md "interfejs użytkownika jQuery 1.8.22 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="c7ac9-286">Interfejs użytkownika jQuery 1.8.21</span><span class="sxs-lookup"><span data-stu-id="c7ac9-286">jQuery UI 1.8.21</span></span>](jquery-ui/cdnjqueryui1821.md "interfejs użytkownika jQuery 1.8.21 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="c7ac9-287">Interfejs użytkownika jQuery 1.8.20</span><span class="sxs-lookup"><span data-stu-id="c7ac9-287">jQuery UI 1.8.20</span></span>](jquery-ui/cdnjqueryui1820.md "interfejs użytkownika jQuery 1.8.20 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="c7ac9-288">Interfejs użytkownika jQuery 1.8.19</span><span class="sxs-lookup"><span data-stu-id="c7ac9-288">jQuery UI 1.8.19</span></span>](jquery-ui/cdnjqueryui1819.md "interfejs użytkownika jQuery 1.8.19 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="c7ac9-289">Interfejs użytkownika jQuery 1.8.18</span><span class="sxs-lookup"><span data-stu-id="c7ac9-289">jQuery UI 1.8.18</span></span>](jquery-ui/cdnjqueryui1818.md "interfejs użytkownika jQuery 1.8.18 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="c7ac9-290">Interfejs użytkownika jQuery 1.8.17</span><span class="sxs-lookup"><span data-stu-id="c7ac9-290">jQuery UI 1.8.17</span></span>](jquery-ui/cdnjqueryui1817.md "interfejs użytkownika jQuery 1.8.17 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="c7ac9-291">Interfejs użytkownika jQuery 1.8.16</span><span class="sxs-lookup"><span data-stu-id="c7ac9-291">jQuery UI 1.8.16</span></span>](jquery-ui/cdnjqueryui1816.md "interfejs użytkownika jQuery 1.8.16 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="c7ac9-292">Interfejs użytkownika jQuery 1.8.15</span><span class="sxs-lookup"><span data-stu-id="c7ac9-292">jQuery UI 1.8.15</span></span>](jquery-ui/cdnjqueryui1815.md "interfejs użytkownika jQuery 1.8.15 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="c7ac9-293">Interfejs użytkownika jQuery 1.8.14</span><span class="sxs-lookup"><span data-stu-id="c7ac9-293">jQuery UI 1.8.14</span></span>](jquery-ui/cdnjqueryui1814.md "interfejs użytkownika jQuery 1.8.14 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="c7ac9-294">Interfejs użytkownika jQuery 1.8.13</span><span class="sxs-lookup"><span data-stu-id="c7ac9-294">jQuery UI 1.8.13</span></span>](jquery-ui/cdnjqueryui1813.md "interfejs użytkownika jQuery 1.8.13 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="c7ac9-295">Interfejs użytkownika jQuery 1.8.12</span><span class="sxs-lookup"><span data-stu-id="c7ac9-295">jQuery UI 1.8.12</span></span>](jquery-ui/cdnjqueryui1812.md "interfejs użytkownika jQuery 1.8.12 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="c7ac9-296">Interfejs użytkownika jQuery 1.8.11</span><span class="sxs-lookup"><span data-stu-id="c7ac9-296">jQuery UI 1.8.11</span></span>](jquery-ui/cdnjqueryui1811.md "interfejs użytkownika jQuery 1.8.11 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="c7ac9-297">Interfejs użytkownika jQuery 1.8.10</span><span class="sxs-lookup"><span data-stu-id="c7ac9-297">jQuery UI 1.8.10</span></span>](jquery-ui/cdnjqueryui1910.md "interfejs użytkownika jQuery 1.8.10 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="c7ac9-298">Interfejs użytkownika jQuery 1.8.9</span><span class="sxs-lookup"><span data-stu-id="c7ac9-298">jQuery UI 1.8.9</span></span>](jquery-ui/cdnjqueryui189.md "interfejs użytkownika jQuery 1.8.9 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="c7ac9-299">Interfejs użytkownika jQuery 1.8.8</span><span class="sxs-lookup"><span data-stu-id="c7ac9-299">jQuery UI 1.8.8</span></span>](jquery-ui/cdnjqueryui188.md "interfejs użytkownika jQuery 1.8.8 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="c7ac9-300">Interfejs użytkownika jQuery 1.8.7</span><span class="sxs-lookup"><span data-stu-id="c7ac9-300">jQuery UI 1.8.7</span></span>](jquery-ui/cdnjqueryui187.md "interfejs użytkownika jQuery 1.8.7 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="c7ac9-301">Interfejs użytkownika jQuery 1.8.6</span><span class="sxs-lookup"><span data-stu-id="c7ac9-301">jQuery UI 1.8.6</span></span>](jquery-ui/cdnjqueryui186.md "interfejs użytkownika jQuery 1.8.6 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="c7ac9-302">jQuery UI 1.8.5</span><span class="sxs-lookup"><span data-stu-id="c7ac9-302">jQuery UI 1.8.5</span></span>](jquery-ui/cdnjqueryui185.md "jQuery UI 1.8.5")

<a id="jQuery_Validation_Releases_on_the_CDN_3"></a>

### <a name="jquery-validation-releases-on-the-cdn"></a><span data-ttu-id="c7ac9-303">Sprawdzanie poprawności wersji w usłudze CDN technologii jQuery</span><span class="sxs-lookup"><span data-stu-id="c7ac9-303">jQuery Validation Releases on the CDN</span></span>

<span data-ttu-id="c7ac9-304">Następujące wersje weryfikacji biblioteki jQuery znajdują się w tej sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="c7ac9-304">The following releases of the jQuery Validation library are hosted on this CDN.</span></span> <span data-ttu-id="c7ac9-305">Kliknij każdy link, aby wyświetlić rzeczywiste listę plików.</span><span class="sxs-lookup"><span data-stu-id="c7ac9-305">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="c7ac9-306">Sprawdź poprawność 1.19.0 jQuery</span><span class="sxs-lookup"><span data-stu-id="c7ac9-306">jQuery Validate 1.19.0</span></span>](jquery-validate/cdnjqueryvalidate1190.md "jQuery 1.19.0 sprawdzania poprawności")
- [<span data-ttu-id="c7ac9-307">Sprawdź poprawność 1.17.0 jQuery</span><span class="sxs-lookup"><span data-stu-id="c7ac9-307">jQuery Validate 1.17.0</span></span>](jquery-validate/cdnjqueryvalidate1170.md "jQuery 1.17.0 sprawdzania poprawności")
- [<span data-ttu-id="c7ac9-308">jQuery Validate 1.16.0</span><span class="sxs-lookup"><span data-stu-id="c7ac9-308">jQuery Validate 1.16.0</span></span>](jquery-validate/cdnjqueryvalidate1160.md "jQuery Validation 1.16.0")
- [<span data-ttu-id="c7ac9-309">Sprawdź poprawność 1.15.1 jQuery</span><span class="sxs-lookup"><span data-stu-id="c7ac9-309">jQuery Validate 1.15.1</span></span>](jquery-validate/cdnjqueryvalidate1151.md "jQuery 1.15.1 sprawdzania poprawności")
- [<span data-ttu-id="c7ac9-310">Sprawdź poprawność 1.15.0 jQuery</span><span class="sxs-lookup"><span data-stu-id="c7ac9-310">jQuery Validate 1.15.0</span></span>](jquery-validate/cdnjqueryvalidate1150.md "jQuery 1.15.0 sprawdzania poprawności")
- [<span data-ttu-id="c7ac9-311">Sprawdź poprawność 1.14.0 jQuery</span><span class="sxs-lookup"><span data-stu-id="c7ac9-311">jQuery Validate 1.14.0</span></span>](jquery-validate/cdnjqueryvalidate1140.md "jQuery 1.14.0 sprawdzania poprawności")
- [<span data-ttu-id="c7ac9-312">Sprawdź poprawność 1.13.1 jQuery</span><span class="sxs-lookup"><span data-stu-id="c7ac9-312">jQuery Validate 1.13.1</span></span>](jquery-validate/cdnjqueryvalidate1131.md "jQuery 1.13.1 sprawdzania poprawności")
- [<span data-ttu-id="c7ac9-313">Sprawdź poprawność 1.13.0 jQuery</span><span class="sxs-lookup"><span data-stu-id="c7ac9-313">jQuery Validate 1.13.0</span></span>](jquery-validate/cdnjqueryvalidate1130.md "jQuery 1.13.0 sprawdzania poprawności")
- [<span data-ttu-id="c7ac9-314">jQuery 1.12.0 sprawdzania poprawności</span><span class="sxs-lookup"><span data-stu-id="c7ac9-314">jQuery Validate 1.12.0</span></span>](jquery-validate/cdnjqueryvalidate1120.md "jQuery 1.12.0 sprawdzania poprawności")
- [<span data-ttu-id="c7ac9-315">jQuery 1.11.1 sprawdzania poprawności</span><span class="sxs-lookup"><span data-stu-id="c7ac9-315">jQuery Validate 1.11.1</span></span>](jquery-validate/cdnjqueryvalidate1111.md "jQuery 1.11.1 sprawdzania poprawności")
- [<span data-ttu-id="c7ac9-316">jQuery Validate 1.11.0</span><span class="sxs-lookup"><span data-stu-id="c7ac9-316">jQuery Validate 1.11.0</span></span>](jquery-validate/cdnjqueryvalidate111.md "jQuery Validation 1.11.0")
- [<span data-ttu-id="c7ac9-317">jQuery 1.10.0 sprawdzania poprawności</span><span class="sxs-lookup"><span data-stu-id="c7ac9-317">jQuery Validate 1.10.0</span></span>](jquery-validate/cdnjqueryvalidate110.md "jQuery 1.10.0 sprawdzania poprawności")
- [<span data-ttu-id="c7ac9-318">jQuery Validate 1.9</span><span class="sxs-lookup"><span data-stu-id="c7ac9-318">jQuery Validate 1.9</span></span>](jquery-validate/cdnjqueryvalidate19.md "jquery.validate version 1.9")
- [<span data-ttu-id="c7ac9-319">Sprawdź poprawność 1.8.1 jQuery</span><span class="sxs-lookup"><span data-stu-id="c7ac9-319">jQuery Validate 1.8.1</span></span>](jquery-validate/cdnjqueryvalidate181.md "jquery.validate wersja 1.8.1")
- [<span data-ttu-id="c7ac9-320">jQuery Validate 1.8</span><span class="sxs-lookup"><span data-stu-id="c7ac9-320">jQuery Validate 1.8</span></span>](jquery-validate/cdnjqueryvalidate18.md "jquery.validate version 1.8")
- [<span data-ttu-id="c7ac9-321">jQuery weryfikacji 1.7</span><span class="sxs-lookup"><span data-stu-id="c7ac9-321">jQuery Validate 1.7</span></span>](jquery-validate/cdnjqueryvalidate17.md "jquery.validate wersja 1.7")
- [<span data-ttu-id="c7ac9-322">jQuery Validate 1.6</span><span class="sxs-lookup"><span data-stu-id="c7ac9-322">jQuery Validate 1.6</span></span>](jquery-validate/cdnjqueryvalidate16.md "jQuery Validate 1.6")
- [<span data-ttu-id="c7ac9-323">jQuery Validate 1.5.5</span><span class="sxs-lookup"><span data-stu-id="c7ac9-323">jQuery Validate 1.5.5</span></span>](jquery-validate/cdnjqueryvalidate155.md "jQuery Validate 1.5.5")

<a id="jQuery_Mobile_Releases_on_the_CDN_4"></a>

### <a name="jquery-mobile-releases-on-the-cdn"></a><span data-ttu-id="c7ac9-324">jQuery Mobile wersji w usłudze CDN</span><span class="sxs-lookup"><span data-stu-id="c7ac9-324">jQuery Mobile Releases on the CDN</span></span>

<span data-ttu-id="c7ac9-325">Następujące wersje biblioteki jQuery przenośnych znajdują się w tej sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="c7ac9-325">The following releases of the jQuery Mobile library are hosted on this CDN.</span></span> <span data-ttu-id="c7ac9-326">Kliknij każdy link, aby wyświetlić rzeczywiste listę plików.</span><span class="sxs-lookup"><span data-stu-id="c7ac9-326">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="c7ac9-327">jQuery Mobile 1.4.5</span><span class="sxs-lookup"><span data-stu-id="c7ac9-327">jQuery Mobile 1.4.5</span></span>](jquery-mobile/cdnjquerymobile145.md "jQuery Mobile 1.4.5 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="c7ac9-328">jQuery Mobile 1.4.2</span><span class="sxs-lookup"><span data-stu-id="c7ac9-328">jQuery Mobile 1.4.2</span></span>](jquery-mobile/cdnjquerymobile142.md "jQuery Mobile 1.4.2 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="c7ac9-329">jQuery Mobile 1.4.1</span><span class="sxs-lookup"><span data-stu-id="c7ac9-329">jQuery Mobile 1.4.1</span></span>](jquery-mobile/cdnjquerymobile141.md "jQuery Mobile 1.4.1 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="c7ac9-330">jQuery Mobile 1.4.0</span><span class="sxs-lookup"><span data-stu-id="c7ac9-330">jQuery Mobile 1.4.0</span></span>](jquery-mobile/cdnjquerymobile140.md "jQuery Mobile 1.4.0 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="c7ac9-331">jQuery Mobile 1.3.2</span><span class="sxs-lookup"><span data-stu-id="c7ac9-331">jQuery Mobile 1.3.2</span></span>](jquery-mobile/cdnjquerymobile132.md "jQuery Mobile 1.3.2 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="c7ac9-332">jQuery Mobile 1.3.1</span><span class="sxs-lookup"><span data-stu-id="c7ac9-332">jQuery Mobile 1.3.1</span></span>](jquery-mobile/cdnjquerymobile131.md "jQuery Mobile 1.3.1 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="c7ac9-333">jQuery Mobile 1.3.0</span><span class="sxs-lookup"><span data-stu-id="c7ac9-333">jQuery Mobile 1.3.0</span></span>](jquery-mobile/cdnjquerymobile130.md "jQuery Mobile 1.3.0 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="c7ac9-334">jQuery Mobile 1.2.0</span><span class="sxs-lookup"><span data-stu-id="c7ac9-334">jQuery Mobile 1.2.0</span></span>](jquery-mobile/cdnjquerymobile120.md "jQuery Mobile 1.2.0 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="c7ac9-335">jQuery Mobile 1.1.2</span><span class="sxs-lookup"><span data-stu-id="c7ac9-335">jQuery Mobile 1.1.2</span></span>](jquery-mobile/cdnjquerymobile112.md "jQuery Mobile 1.1.2 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="c7ac9-336">jQuery Mobile 1.1.1</span><span class="sxs-lookup"><span data-stu-id="c7ac9-336">jQuery Mobile 1.1.1</span></span>](jquery-mobile/cdnjquerymobile111.md "jQuery Mobile 1.1.1 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="c7ac9-337">jQuery Mobile 1.1.0</span><span class="sxs-lookup"><span data-stu-id="c7ac9-337">jQuery Mobile 1.1.0</span></span>](jquery-mobile/cdnjquerymobile110.md "jQuery Mobile 1.1.0 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="c7ac9-338">jQuery Mobile 1.1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="c7ac9-338">jQuery Mobile 1.1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile110rc2.md "jQuery Mobile 1.1.0 RC2 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="c7ac9-339">jQuery Mobile 1.0.1</span><span class="sxs-lookup"><span data-stu-id="c7ac9-339">jQuery Mobile 1.0.1</span></span>](jquery-mobile/cdnjquerymobile101.md "jQuery Mobile 1.0.1 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="c7ac9-340">jQuery Mobile 1.0</span><span class="sxs-lookup"><span data-stu-id="c7ac9-340">jQuery Mobile 1.0</span></span>](jquery-mobile/cdnjquerymobile10.md "jQuery Mobile 1.0 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="c7ac9-341">jQuery Mobile 1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="c7ac9-341">jQuery Mobile 1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile10rc2.md "jQuery Mobile 1.0 RC2 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="c7ac9-342">jQuery Mobile 1.0 RC 1</span><span class="sxs-lookup"><span data-stu-id="c7ac9-342">jQuery Mobile 1.0 RC 1</span></span>](jquery-mobile/cdnjquerymobile10rc1.md "jQuery Mobile 1.0 RC1 w usłudze Microsoft Ajax CDN")
- [<span data-ttu-id="c7ac9-343">jQuery Mobile 1.0 beta 3</span><span class="sxs-lookup"><span data-stu-id="c7ac9-343">jQuery Mobile 1.0 beta 3</span></span>](jquery-mobile/cdnjquerymobile10b3.md "jQuery Mobile 1.0 Beta 3 w usłudze Microsoft Ajax CDN")

<a id="jQuery_Templates_Releases_on_the_CDN_5"></a>

### <a name="jquery-templates-releases-on-the-cdn"></a><span data-ttu-id="c7ac9-344">jQuery szablony wydań w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="c7ac9-344">jQuery Templates Releases on the CDN</span></span>

<span data-ttu-id="c7ac9-345">Następujące wersje wtyczki szablonów jQuery znajdują się w tej sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="c7ac9-345">The following releases of the jQuery Templates plugin are hosted on this CDN.</span></span> <span data-ttu-id="c7ac9-346">Kliknij każdy link, aby wyświetlić rzeczywiste listę plików.</span><span class="sxs-lookup"><span data-stu-id="c7ac9-346">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="c7ac9-347">jQuery szablonów w wersji Beta 1</span><span class="sxs-lookup"><span data-stu-id="c7ac9-347">jQuery Templates Beta 1</span></span>](jquery-templates/cdnjquerytemplatesbeta1.md "jQuery szablonów w wersji Beta 1")

<a id="jQuery_Cycle_Releases_on_the_CDN_6"></a>

### <a name="jquery-cycle-releases-on-the-cdn"></a><span data-ttu-id="c7ac9-348">jQuery cyklu wersji w usłudze CDN</span><span class="sxs-lookup"><span data-stu-id="c7ac9-348">jQuery Cycle Releases on the CDN</span></span>

<span data-ttu-id="c7ac9-349">Następujące wersje wtyczki cyklu jQuery znajdują się w tej sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="c7ac9-349">The following releases of the jQuery Cycle plugin are hosted on this CDN.</span></span> <span data-ttu-id="c7ac9-350">Kliknij każdy link, aby wyświetlić rzeczywiste listę plików.</span><span class="sxs-lookup"><span data-stu-id="c7ac9-350">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="c7ac9-351">jQuery Cycle 2.99</span><span class="sxs-lookup"><span data-stu-id="c7ac9-351">jQuery Cycle 2.99</span></span>](jquery-cycle/cdnjquerycycle299.md "jQuery Cycle 2.99")
- [<span data-ttu-id="c7ac9-352">jQuery Cycle 2.94</span><span class="sxs-lookup"><span data-stu-id="c7ac9-352">jQuery Cycle 2.94</span></span>](jquery-cycle/cdnjquerycycle294.md "jQuery Cycle 2.94")
- [<span data-ttu-id="c7ac9-353">jQuery Cycle 2.88</span><span class="sxs-lookup"><span data-stu-id="c7ac9-353">jQuery Cycle 2.88</span></span>](jquery-cycle/cdnjquerycycle288.md "jQuery Cycle 2.88")

<a id="jQuery_DataTables_Releases_on_the_CDN_7"></a>

### <a name="jquery-datatables-releases-on-the-cdn"></a><span data-ttu-id="c7ac9-354">jQuery wersji DataTable w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="c7ac9-354">jQuery DataTables Releases on the CDN</span></span>

<span data-ttu-id="c7ac9-355">Następujące wersje wtyczki DataTables jQuery znajdują się w tej sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="c7ac9-355">The following releases of the jQuery DataTables plugin are hosted on this CDN.</span></span> <span data-ttu-id="c7ac9-356">Kliknij każdy link, aby wyświetlić rzeczywiste listę plików.</span><span class="sxs-lookup"><span data-stu-id="c7ac9-356">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="c7ac9-357">jQuery DataTables 1.10.5</span><span class="sxs-lookup"><span data-stu-id="c7ac9-357">jQuery DataTables 1.10.5</span></span>](jquery-datatables/cdnjquerydatatables105.md "jQuery DataTables 1.10.5")
- [<span data-ttu-id="c7ac9-358">jQuery DataTables 1.10.4</span><span class="sxs-lookup"><span data-stu-id="c7ac9-358">jQuery DataTables 1.10.4</span></span>](jquery-datatables/cdnjquerydatatables104.md "jQuery DataTables 1.10.4")
- [<span data-ttu-id="c7ac9-359">jQuery DataTables 1.9.4</span><span class="sxs-lookup"><span data-stu-id="c7ac9-359">jQuery DataTables 1.9.4</span></span>](jquery-datatables/cdnjquerydatatables194.md "jQuery DataTables 1.9.4")
- [<span data-ttu-id="c7ac9-360">jQuery DataTables 1.9.3</span><span class="sxs-lookup"><span data-stu-id="c7ac9-360">jQuery DataTables 1.9.3</span></span>](jquery-datatables/cdnjquerydatatables193.md "jQuery DataTables 1.9.3")
- [<span data-ttu-id="c7ac9-361">jQuery DataTables 1.9.2</span><span class="sxs-lookup"><span data-stu-id="c7ac9-361">jQuery DataTables 1.9.2</span></span>](jquery-datatables/cdnjquerydatatables192.md "jQuery DataTables 1.9.2")
- [<span data-ttu-id="c7ac9-362">jQuery DataTables 1.9.1</span><span class="sxs-lookup"><span data-stu-id="c7ac9-362">jQuery DataTables 1.9.1</span></span>](jquery-datatables/cdnjquerydatatables191.md "jQuery DataTables 1.9.1")
- [<span data-ttu-id="c7ac9-363">jQuery DataTables 1.9.0</span><span class="sxs-lookup"><span data-stu-id="c7ac9-363">jQuery DataTables 1.9.0</span></span>](jquery-datatables/cdnjquerydatatables190.md "jQuery DataTables 1.9.0")
- [<span data-ttu-id="c7ac9-364">jQuery DataTables 1.8.2</span><span class="sxs-lookup"><span data-stu-id="c7ac9-364">jQuery DataTables 1.8.2</span></span>](jquery-datatables/cdnjquerydatatables182.md "jQuery DataTables 1.8.2")

<a id="Modernizr_Releases_on_the_CDN_8"></a>

### <a name="modernizr-releases-on-the-cdn"></a><span data-ttu-id="c7ac9-365">Wersje Modernizr w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="c7ac9-365">Modernizr Releases on the CDN</span></span>

<span data-ttu-id="c7ac9-366">Następujące wersje programu [Modernizr](http://www.modernizr.com "Modernizr") znajdują się w sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="c7ac9-366">The following releases of [Modernizr](http://www.modernizr.com "Modernizr") are hosted on the CDN:</span></span>

- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-3.5.0.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.8.3.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.2.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.1.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.6.2.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-1.7-development-only.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.0.6-development-only.js

<a id="JSHint_Releases_on_the_CDN_10"></a>

### <a name="jshint-releases-on-the-cdn"></a><span data-ttu-id="c7ac9-367">Wersje JSHint w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="c7ac9-367">JSHint Releases on the CDN</span></span>

<span data-ttu-id="c7ac9-368">Następujące wersje programu [JSHint](http://www.jshint.com "JSHint") znajdują się w sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="c7ac9-368">The following releases of [JSHint](http://www.jshint.com "JSHint") are hosted on the CDN:</span></span>

- https://ajax.aspnetcdn.com/ajax/jshint/r07/jshint.js

<a id="Knockout_Releases_on_the_CDN_11"></a>

### <a name="knockout-releases-on-the-cdn"></a><span data-ttu-id="c7ac9-369">Wersje knockout w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="c7ac9-369">Knockout Releases on the CDN</span></span>

<span data-ttu-id="c7ac9-370">Następujące wersje programu [Knockout](http://www.knockoutjs.com "Knockout") znajdują się w sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="c7ac9-370">The following releases of [Knockout](http://www.knockoutjs.com "Knockout") are hosted on the CDN:</span></span>

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

### <a name="globalize-releases-on-the-cdn"></a><span data-ttu-id="c7ac9-371">Sprzedawać wersji w usłudze CDN</span><span class="sxs-lookup"><span data-stu-id="c7ac9-371">Globalize Releases on the CDN</span></span>

<span data-ttu-id="c7ac9-372">Następujące wersje programu [Globalize](https://github.com/jquery/globalize "Globalize") znajdują się w sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="c7ac9-372">The following releases of [Globalize](https://github.com/jquery/globalize "Globalize") are hosted on the CDN:</span></span>

#### <a name="globalize-version-100"></a><span data-ttu-id="c7ac9-373">Sprzedawać w wersji 1.0.0</span><span class="sxs-lookup"><span data-stu-id="c7ac9-373">Globalize version 1.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/node-main.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/currency.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/date.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/message.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/number.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/plural.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/relative-time.js

#### <a name="globalize-version-011"></a><span data-ttu-id="c7ac9-374">Sprzedawać wersji 0.1.1</span><span class="sxs-lookup"><span data-stu-id="c7ac9-374">Globalize version 0.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.min.js
- https://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.js
- https://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.cultures.js

    - <span data-ttu-id="c7ac9-375">wszystkich kultur</span><span class="sxs-lookup"><span data-stu-id="c7ac9-375">all cultures</span></span>
- https://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.culture.{culture-code}.js

    - <span data-ttu-id="c7ac9-376">Zamień na kod żądaną kultury "{kodu kultury}", np. Microsoft globalize.culture.en GB.js== plików w usłudze CDN == te biblioteki zostały przekazane przez firmę Microsoft.</span><span class="sxs-lookup"><span data-stu-id="c7ac9-376">Replace "{culture-code}" with the desired culture code, e.g. globalize.culture.en-GB.js== Microsoft Files on the CDN ==These libraries were uploaded by Microsoft.</span></span>

<a id="Respond_Releases_on_the_CDN_13"></a>

### <a name="respond-releases-on-the-cdn"></a><span data-ttu-id="c7ac9-377">Odpowiadać wersji w usłudze CDN</span><span class="sxs-lookup"><span data-stu-id="c7ac9-377">Respond Releases on the CDN</span></span>

<span data-ttu-id="c7ac9-378">Następujące wersje programu [https://github.com/scottjehl/Respond](https://github.com/scottjehl/Respond "https://github.com/scottjehl/Respond") odpowiada znajdują się w sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="c7ac9-378">The following releases of [https://github.com/scottjehl/Respond](https://github.com/scottjehl/Respond "https://github.com/scottjehl/Respond") Respond are hosted on the CDN:</span></span>

#### <a name="respond-version-142"></a><span data-ttu-id="c7ac9-379">Odpowiedź w wersji 1.4.2</span><span class="sxs-lookup"><span data-stu-id="c7ac9-379">Respond version 1.4.2</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.min.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.min.js

#### <a name="respond-version-141"></a><span data-ttu-id="c7ac9-380">Wersja odpowiada 1.4.1</span><span class="sxs-lookup"><span data-stu-id="c7ac9-380">Respond version 1.4.1</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.min.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.min.js

#### <a name="respond-version-140"></a><span data-ttu-id="c7ac9-381">Odpowiadać wersji 1.4.0</span><span class="sxs-lookup"><span data-stu-id="c7ac9-381">Respond version 1.4.0</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.min.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.min.js

#### <a name="respond-version-130"></a><span data-ttu-id="c7ac9-382">Wersja odpowiada 1.3.0</span><span class="sxs-lookup"><span data-stu-id="c7ac9-382">Respond version 1.3.0</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.3.0/respond.js

#### <a name="respond-version-120"></a><span data-ttu-id="c7ac9-383">Odpowiadać wersji 1.2.0 lub nowszej</span><span class="sxs-lookup"><span data-stu-id="c7ac9-383">Respond version 1.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.2.0/respond.js

<a id="Bootstrap_Releases_on_the_CDN_14"></a>

### <a name="bootstrap-releases-on-the-cdn"></a><span data-ttu-id="c7ac9-384">Wersje ładowania w usłudze CDN</span><span class="sxs-lookup"><span data-stu-id="c7ac9-384">Bootstrap Releases on the CDN</span></span>

<span data-ttu-id="c7ac9-385">Następujące wersje programu [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap znajdują się w sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="c7ac9-385">The following releases of [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap are hosted on the CDN:</span></span>

#### <a name="bootstrap-version-431"></a><span data-ttu-id="c7ac9-386">Uruchomienie wersji 4.3.1</span><span class="sxs-lookup"><span data-stu-id="c7ac9-386">Bootstrap version 4.3.1</span></span>

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

#### <a name="bootstrap-version-421"></a><span data-ttu-id="c7ac9-387">Wersja bootstrap 4.2.1</span><span class="sxs-lookup"><span data-stu-id="c7ac9-387">Bootstrap version 4.2.1</span></span>

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

#### <a name="bootstrap-version-411"></a><span data-ttu-id="c7ac9-388">Wersja bootstrap 4.1.1</span><span class="sxs-lookup"><span data-stu-id="c7ac9-388">Bootstrap version 4.1.1</span></span>

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

#### <a name="bootstrap-version-400"></a><span data-ttu-id="c7ac9-389">Uruchomienie wersji 4.0.0</span><span class="sxs-lookup"><span data-stu-id="c7ac9-389">Bootstrap version 4.0.0</span></span>

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

#### <a name="bootstrap-version-341"></a><span data-ttu-id="c7ac9-390">Wersja bootstrap 3.4.1</span><span class="sxs-lookup"><span data-stu-id="c7ac9-390">Bootstrap version 3.4.1</span></span>

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

#### <a name="bootstrap-version-340"></a><span data-ttu-id="c7ac9-391">Wersja bootstrap 3.4.0</span><span class="sxs-lookup"><span data-stu-id="c7ac9-391">Bootstrap version 3.4.0</span></span>

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

#### <a name="bootstrap-version-337"></a><span data-ttu-id="c7ac9-392">Wersja bootstrap 3.3.7</span><span class="sxs-lookup"><span data-stu-id="c7ac9-392">Bootstrap version 3.3.7</span></span>

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

#### <a name="bootstrap-version-336"></a><span data-ttu-id="c7ac9-393">Wersja bootstrap 3.3.6</span><span class="sxs-lookup"><span data-stu-id="c7ac9-393">Bootstrap version 3.3.6</span></span>

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

#### <a name="bootstrap-version-335"></a><span data-ttu-id="c7ac9-394">Wersja bootstrap 3.3.5</span><span class="sxs-lookup"><span data-stu-id="c7ac9-394">Bootstrap version 3.3.5</span></span>

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

#### <a name="bootstrap-version-334"></a><span data-ttu-id="c7ac9-395">Wersja bootstrap 3.3.4</span><span class="sxs-lookup"><span data-stu-id="c7ac9-395">Bootstrap version 3.3.4</span></span>

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

#### <a name="bootstrap-version-332"></a><span data-ttu-id="c7ac9-396">Wersja bootstrap 3.3.2</span><span class="sxs-lookup"><span data-stu-id="c7ac9-396">Bootstrap version 3.3.2</span></span>

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

#### <a name="bootstrap-version-331"></a><span data-ttu-id="c7ac9-397">W wersji 3.3.1 bootstrap</span><span class="sxs-lookup"><span data-stu-id="c7ac9-397">Bootstrap version 3.3.1</span></span>

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

#### <a name="bootstrap-version-330"></a><span data-ttu-id="c7ac9-398">Wersja bootstrap 3.3.0</span><span class="sxs-lookup"><span data-stu-id="c7ac9-398">Bootstrap version 3.3.0</span></span>

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

#### <a name="bootstrap-version-320"></a><span data-ttu-id="c7ac9-399">Wersja bootstrap 3.2.0</span><span class="sxs-lookup"><span data-stu-id="c7ac9-399">Bootstrap version 3.2.0</span></span>

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

#### <a name="bootstrap-version-311"></a><span data-ttu-id="c7ac9-400">Wersja bootstrap 3.1.1</span><span class="sxs-lookup"><span data-stu-id="c7ac9-400">Bootstrap version 3.1.1</span></span>

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

#### <a name="bootstrap-version-310"></a><span data-ttu-id="c7ac9-401">Wersja bootstrap 3.1.0</span><span class="sxs-lookup"><span data-stu-id="c7ac9-401">Bootstrap version 3.1.0</span></span>

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

#### <a name="bootstrap-version-303"></a><span data-ttu-id="c7ac9-402">Wersja bootstrap 3.0.3</span><span class="sxs-lookup"><span data-stu-id="c7ac9-402">Bootstrap version 3.0.3</span></span>

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

#### <a name="bootstrap-version-302"></a><span data-ttu-id="c7ac9-403">Wersja 3.0.2 bootstrap</span><span class="sxs-lookup"><span data-stu-id="c7ac9-403">Bootstrap version 3.0.2</span></span>

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

#### <a name="bootstrap-version-301"></a><span data-ttu-id="c7ac9-404">Uruchomienie wersji 3.0.1</span><span class="sxs-lookup"><span data-stu-id="c7ac9-404">Bootstrap version 3.0.1</span></span>

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

#### <a name="bootstrap-version-300"></a><span data-ttu-id="c7ac9-405">Uruchomienie wersji 3.0.0</span><span class="sxs-lookup"><span data-stu-id="c7ac9-405">Bootstrap version 3.0.0</span></span>

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

#### <a name="bootstrap-version-232"></a><span data-ttu-id="c7ac9-406">Uruchomienie wersji 2.3.2</span><span class="sxs-lookup"><span data-stu-id="c7ac9-406">Bootstrap version 2.3.2</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings.png
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings-white.png

#### <a name="bootstrap-version-231"></a><span data-ttu-id="c7ac9-407">Wersja 2.3.1 bootstrap</span><span class="sxs-lookup"><span data-stu-id="c7ac9-407">Bootstrap version 2.3.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings.png
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings-white.png

<a id="BootstrapTouchCarousel_Releases_on_the_CDN_18"></a>

### <a name="bootstrap-touchcarousel-releases-on-the-cdn"></a><span data-ttu-id="c7ac9-408">Wersje TouchCarousel ładowania w usłudze CDN</span><span class="sxs-lookup"><span data-stu-id="c7ac9-408">Bootstrap TouchCarousel Releases on the CDN</span></span>

<span data-ttu-id="c7ac9-409">Następujące wersje programu [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") Bootstrap TouchCarousel wersji znajdują się w sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="c7ac9-409">The following releases of [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") Bootstrap TouchCarousel releases are hosted on the CDN:</span></span>

#### <a name="bootstrap-touchcarousel-version-080"></a><span data-ttu-id="c7ac9-410">Bootstrap TouchCarousel version 0.8.0</span><span class="sxs-lookup"><span data-stu-id="c7ac9-410">Bootstrap TouchCarousel version 0.8.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/css/bootstrap-touch-carousel.css
- https://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/js/bootstrap-touch-carousel.js

<a id="Hammerjs_Releases_on_the_CDN_19"></a>

### <a name="hammerjs-releases-on-the-cdn"></a><span data-ttu-id="c7ac9-411">Wersje hammer.js w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="c7ac9-411">Hammer.js Releases on the CDN</span></span>

<span data-ttu-id="c7ac9-412">Następujące wersje programu [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js wersji znajdują się w sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="c7ac9-412">The following releases of [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js releases are hosted on the CDN:</span></span>

#### <a name="hammerjs-version-204"></a><span data-ttu-id="c7ac9-413">Hammer.js w wersji 2.0.4</span><span class="sxs-lookup"><span data-stu-id="c7ac9-413">Hammer.js version 2.0.4</span></span>

- https://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.js
- https://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.js
- https://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.map

<a id="ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15"></a>

### <a name="aspnet-web-forms-and-ajax-releases-on-the-cdn"></a><span data-ttu-id="c7ac9-414">ASP.NET Web Forms i Ajax wersji w usłudze CDN</span><span class="sxs-lookup"><span data-stu-id="c7ac9-414">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>

<span data-ttu-id="c7ac9-415">Następujące wersje biblioteki ASP.NET Ajax znajdują się w sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="c7ac9-415">The following releases of the ASP.NET Ajax Library are hosted on the CDN.</span></span> <span data-ttu-id="c7ac9-416">Kliknij każdy link, aby wyświetlić rzeczywiste listę plików.</span><span class="sxs-lookup"><span data-stu-id="c7ac9-416">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="c7ac9-417">Wersja ASP.NET Web Forms i Ajax 4.5.2</span><span class="sxs-lookup"><span data-stu-id="c7ac9-417">ASP.NET Web Forms and Ajax version 4.5.2</span></span>](cdnajax452.md "ASP.NET Web Forms i Ajax 4.5.2")
- [<span data-ttu-id="c7ac9-418">Wersja ASP.NET Web Forms i Ajax 4</span><span class="sxs-lookup"><span data-stu-id="c7ac9-418">ASP.NET Web Forms and Ajax version 4</span></span>](cdnajax4.md "ASP.NET Web Forms i Ajax 4")
- [<span data-ttu-id="c7ac9-419">ASP.NET Ajax w wersji 3.5</span><span class="sxs-lookup"><span data-stu-id="c7ac9-419">ASP.NET Ajax version 3.5</span></span>](cdnajax35.md "Ajax programu ASP.NET 3.5")

<a id="ASPNET_MVC_Releases_on_the_CDN_16"></a>

### <a name="aspnet-mvc-releases-on-the-cdn"></a><span data-ttu-id="c7ac9-420">ASP.NET MVC udostępnia w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="c7ac9-420">ASP.NET MVC Releases on the CDN</span></span>

<span data-ttu-id="c7ac9-421">Następujące pliki ASP.NET MVC JavaScript znajdują się w tej sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="c7ac9-421">The following ASP.NET MVC JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-mvc-523"></a><span data-ttu-id="c7ac9-422">ASP.NET MVC 5.2.3</span><span class="sxs-lookup"><span data-stu-id="c7ac9-422">ASP.NET MVC 5.2.3</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-51"></a><span data-ttu-id="c7ac9-423">ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="c7ac9-423">ASP.NET MVC 5.1</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-50"></a><span data-ttu-id="c7ac9-424">ASP.NET MVC 5.0</span><span class="sxs-lookup"><span data-stu-id="c7ac9-424">ASP.NET MVC 5.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-40"></a><span data-ttu-id="c7ac9-425">ASP.NET MVC 4.0</span><span class="sxs-lookup"><span data-stu-id="c7ac9-425">ASP.NET MVC 4.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-30"></a><span data-ttu-id="c7ac9-426">ASP.NET MVC 3.0</span><span class="sxs-lookup"><span data-stu-id="c7ac9-426">ASP.NET MVC 3.0</span></span>

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

#### <a name="aspnet-mvc-20"></a><span data-ttu-id="c7ac9-427">ASP.NET MVC 2.0</span><span class="sxs-lookup"><span data-stu-id="c7ac9-427">ASP.NET MVC 2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.js
- https://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.debug.js

#### <a name="aspnet-mvc-10"></a><span data-ttu-id="c7ac9-428">ASP.NET MVC 1.0</span><span class="sxs-lookup"><span data-stu-id="c7ac9-428">ASP.NET MVC 1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.js
- https://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.debug.js

<a id="ASPNET_SignalR_Releases_on_the_CDN_17"></a>

### <a name="aspnet-signalr-releases-on-the-cdn"></a><span data-ttu-id="c7ac9-429">Wersje biblioteki SignalR platformy ASP.NET, w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="c7ac9-429">ASP.NET SignalR Releases on the CDN</span></span>

<span data-ttu-id="c7ac9-430">Następujące pliki ASP.NET SignalR JavaScript znajdują się w tej sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="c7ac9-430">The following ASP.NET SignalR JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-signalr-222"></a><span data-ttu-id="c7ac9-431">ASP.NET SignalR 2.2.2</span><span class="sxs-lookup"><span data-stu-id="c7ac9-431">ASP.NET SignalR 2.2.2</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.js

#### <a name="aspnet-signalr-221"></a><span data-ttu-id="c7ac9-432">ASP.NET SignalR 2.2.1</span><span class="sxs-lookup"><span data-stu-id="c7ac9-432">ASP.NET SignalR 2.2.1</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.js

#### <a name="aspnet-signalr-220"></a><span data-ttu-id="c7ac9-433">ASP.NET SignalR 2.2.0</span><span class="sxs-lookup"><span data-stu-id="c7ac9-433">ASP.NET SignalR 2.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.js

#### <a name="aspnet-signalr-210"></a><span data-ttu-id="c7ac9-434">ASP.NET SignalR 2.1.0</span><span class="sxs-lookup"><span data-stu-id="c7ac9-434">ASP.NET SignalR 2.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.js

#### <a name="aspnet-signalr-203"></a><span data-ttu-id="c7ac9-435">ASP.NET SignalR 2.0.3</span><span class="sxs-lookup"><span data-stu-id="c7ac9-435">ASP.NET SignalR 2.0.3</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.js

#### <a name="aspnet-signalr-202"></a><span data-ttu-id="c7ac9-436">ASP.NET SignalR 2.0.2</span><span class="sxs-lookup"><span data-stu-id="c7ac9-436">ASP.NET SignalR 2.0.2</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.js

#### <a name="aspnet-signalr-201"></a><span data-ttu-id="c7ac9-437">ASP.NET SignalR 2.0.1</span><span class="sxs-lookup"><span data-stu-id="c7ac9-437">ASP.NET SignalR 2.0.1</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.js

#### <a name="aspnet-signalr-200"></a><span data-ttu-id="c7ac9-438">ASP.NET SignalR 2.0.0</span><span class="sxs-lookup"><span data-stu-id="c7ac9-438">ASP.NET SignalR 2.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.js

#### <a name="aspnet-signalr-113"></a><span data-ttu-id="c7ac9-439">ASP.NET SignalR 1.1.3</span><span class="sxs-lookup"><span data-stu-id="c7ac9-439">ASP.NET SignalR 1.1.3</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.js

#### <a name="aspnet-signalr-112"></a><span data-ttu-id="c7ac9-440">ASP.NET SignalR 1.1.2</span><span class="sxs-lookup"><span data-stu-id="c7ac9-440">ASP.NET SignalR 1.1.2</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.js

#### <a name="aspnet-signalr-111"></a><span data-ttu-id="c7ac9-441">ASP.NET SignalR 1.1.1</span><span class="sxs-lookup"><span data-stu-id="c7ac9-441">ASP.NET SignalR 1.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.js

#### <a name="aspnet-signalr-110"></a><span data-ttu-id="c7ac9-442">ASP.NET SignalR 1.1.0</span><span class="sxs-lookup"><span data-stu-id="c7ac9-442">ASP.NET SignalR 1.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.js

#### <a name="aspnet-signalr-101"></a><span data-ttu-id="c7ac9-443">ASP.NET SignalR 1.0.1</span><span class="sxs-lookup"><span data-stu-id="c7ac9-443">ASP.NET SignalR 1.0.1</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.js

<span data-ttu-id="c7ac9-444">Aby uzyskać informacji o warunkach użytkowania usługi CDN, zobacz [Microsoft Ajax CDN warunki użytkowania](https://www.asp.net/terms-of-use "Microsoft Ajax CDN warunki użytkowania").</span><span class="sxs-lookup"><span data-stu-id="c7ac9-444">For information about the terms of use for the CDN, see [Microsoft Ajax CDN Terms of Use](https://www.asp.net/terms-of-use "Microsoft Ajax CDN Terms of Use").</span></span>

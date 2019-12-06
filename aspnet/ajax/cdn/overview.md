---
uid: ajax/cdn/overview
title: Microsoft Ajax Content Delivery Network | Microsoft Docs
author: rick-anderson
description: ''
ms.author: riande
ms.date: 10/14/2017
ms.assetid: 8935bf14-ca6d-4a4e-9dbe-b96ce74cef49
msc.legacyurl: /ajax/cdn
msc.type: content
ms.openlocfilehash: 51cb8d672139aaebd77bcdbe80bb579d4b3776aa
ms.sourcegitcommit: 969e7db924ebad3cc0f0cb0d65d148e8b9221b9a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/06/2019
ms.locfileid: "74899572"
---
# <a name="microsoft-ajax-content-delivery-network"></a><span data-ttu-id="74875-102">Usługa Microsoft AJAX Content Delivery Network</span><span class="sxs-lookup"><span data-stu-id="74875-102">Microsoft Ajax Content Delivery Network</span></span>

> [!WARNING]
> <span data-ttu-id="74875-103">Aplikacje produkcyjne nie powinny podejmować twardej zależności od zasobów sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="74875-103">Production applications should not take a hard dependency on CDN assets.</span></span> <span data-ttu-id="74875-104">Aplikacje powinny testować się w odniesieniu do elementu zawartości usługi CDN i używać rezerwowego elementu zawartości, gdy sieć CDN jest niedostępna.</span><span class="sxs-lookup"><span data-stu-id="74875-104">Applications should test for the CDN asset referenced, and use a fallback asset when the CDN is not available.</span></span>
>
> <span data-ttu-id="74875-105">Usługa Microsoft Ajax CDN nie ma umowy SLA powyżej i poza nią przy użyciu Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="74875-105">The Microsoft Ajax CDN has no SLA above and beyond using an Azure CDN.</span></span>
>
> <span data-ttu-id="74875-106">[Ten problem](https://github.com/aspnet/AspNetDocs/issues/116) w usłudze GitHub służy do zgłaszania problemów z usługą Microsoft Ajax CDN.</span><span class="sxs-lookup"><span data-stu-id="74875-106">Use [this GitHub issue](https://github.com/aspnet/AspNetDocs/issues/116) to report problems with the Microsoft Ajax CDN.</span></span>

## <a name="table-of-contents"></a><span data-ttu-id="74875-107">Spis treści</span><span class="sxs-lookup"><span data-stu-id="74875-107">Table of Contents</span></span>

<span data-ttu-id="74875-108">**[ajax.microsoft.com zmieniono nazwę na ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span><span class="sxs-lookup"><span data-stu-id="74875-108">**[ajax.microsoft.com renamed to ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span></span>  
<span data-ttu-id="74875-109">**[Obsługa programu Visual Studio. vsdoc](#Visual_Studio_vsdoc_Support_19)**</span><span class="sxs-lookup"><span data-stu-id="74875-109">**[Visual Studio .vsdoc Support](#Visual_Studio_vsdoc_Support_19)**</span></span>  
<span data-ttu-id="74875-110">**[Korzystanie z ASP.NET AJAX z sieci CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span><span class="sxs-lookup"><span data-stu-id="74875-110">**[Using ASP.NET Ajax from the CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span></span>  
<span data-ttu-id="74875-111">**[Korzystanie z platformy jQuery z sieci CDN](#Using_jQuery_from_the_CDN_21)**</span><span class="sxs-lookup"><span data-stu-id="74875-111">**[Using jQuery from the CDN](#Using_jQuery_from_the_CDN_21)**</span></span>  
<span data-ttu-id="74875-112">**[Korzystanie z interfejsu użytkownika jQuery z sieci CDN](#Using_jQuery_UI_from_the_CDN_22)**</span><span class="sxs-lookup"><span data-stu-id="74875-112">**[Using jQuery UI from the CDN](#Using_jQuery_UI_from_the_CDN_22)**</span></span>  
<span data-ttu-id="74875-113">**[Pliki innych firm w sieci CDN](#Third-Party_Files_on_the_CDN_23)**</span><span class="sxs-lookup"><span data-stu-id="74875-113">**[Third-Party Files on the CDN](#Third-Party_Files_on_the_CDN_23)**</span></span>  
  
 [<span data-ttu-id="74875-114">Wersje jQuery w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="74875-114">jQuery Releases on the CDN</span></span>](#jQuery_Releases_on_the_CDN_0)  
 [<span data-ttu-id="74875-115">Usługa jQuery migruje wydania w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="74875-115">jQuery Migrate Releases on the CDN</span></span>](#jQuery_Migrate_Releases_on_the_CDN_1)  
 [<span data-ttu-id="74875-116">Wersje interfejsu użytkownika jQuery w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="74875-116">jQuery UI Releases on the CDN</span></span>](#jQuery_UI_Releases_on_the_CDN_2)  
 [<span data-ttu-id="74875-117">Wersje walidacji jQuery w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="74875-117">jQuery Validation Releases on the CDN</span></span>](#jQuery_Validation_Releases_on_the_CDN_3)  
 [<span data-ttu-id="74875-118">Wersje programu jQuery Mobile w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="74875-118">jQuery Mobile Releases on the CDN</span></span>](#jQuery_Mobile_Releases_on_the_CDN_4)  
 [<span data-ttu-id="74875-119">Szablony jQuery są wydaniami w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="74875-119">jQuery Templates Releases on the CDN</span></span>](#jQuery_Templates_Releases_on_the_CDN_5)  
 [<span data-ttu-id="74875-120">Wersje usługi jQuery cykle w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="74875-120">jQuery Cycle Releases on the CDN</span></span>](#jQuery_Cycle_Releases_on_the_CDN_6)  
 [<span data-ttu-id="74875-121">Wersje jQuery DataTables w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="74875-121">jQuery DataTables Releases on the CDN</span></span>](#jQuery_DataTables_Releases_on_the_CDN_7)  
 [<span data-ttu-id="74875-122">Wersje programu modernizacji w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="74875-122">Modernizr Releases on the CDN</span></span>](#Modernizr_Releases_on_the_CDN_8)  
 [<span data-ttu-id="74875-123">JSHint wersje w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="74875-123">JSHint Releases on the CDN</span></span>](#JSHint_Releases_on_the_CDN_10)  
 [<span data-ttu-id="74875-124">Wycinanie odcięć w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="74875-124">Knockout Releases on the CDN</span></span>](#Knockout_Releases_on_the_CDN_11)  
 [<span data-ttu-id="74875-125">Globalizacja wydań w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="74875-125">Globalize Releases on the CDN</span></span>](#Globalize_Releases_on_the_CDN_12)  
 [<span data-ttu-id="74875-126">Odpowiedz na wydania w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="74875-126">Respond Releases on the CDN</span></span>](#Respond_Releases_on_the_CDN_13)  
 [<span data-ttu-id="74875-127">Wydania ładowania początkowego w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="74875-127">Bootstrap Releases on the CDN</span></span>](#Bootstrap_Releases_on_the_CDN_14)  
 [<span data-ttu-id="74875-128">Wydania TouchCarousel Bootstrap w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="74875-128">Bootstrap TouchCarousel Releases on the CDN</span></span>](#BootstrapTouchCarousel_Releases_on_the_CDN_18)  
 [<span data-ttu-id="74875-129">Wersje młota. js w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="74875-129">Hammer.js Releases on the CDN</span></span>](#Hammerjs_Releases_on_the_CDN_19)  
 [<span data-ttu-id="74875-130">ASP.NET Web Forms i wersje AJAX w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="74875-130">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>](#ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15)  
 [<span data-ttu-id="74875-131">Wersje ASP.NET MVC w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="74875-131">ASP.NET MVC Releases on the CDN</span></span>](#ASPNET_MVC_Releases_on_the_CDN_16)  
 [<span data-ttu-id="74875-132">Wydania sygnałów ASP.NET w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="74875-132">ASP.NET SignalR Releases on the CDN</span></span>](#ASPNET_SignalR_Releases_on_the_CDN_17)

<span data-ttu-id="74875-133">Microsoft Ajax Content Delivery Network (CDN) obsługuje popularne biblioteki JavaScript innych firm, takie jak jQuery, i umożliwia łatwe dodawanie ich do aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="74875-133">The Microsoft Ajax Content Delivery Network (CDN) hosts popular third party JavaScript libraries such as jQuery and enables you to easily add them to your Web applications.</span></span> <span data-ttu-id="74875-134">Na przykład możesz zacząć korzystać z platformy jQuery, która jest hostowana w tej sieci CDN po prostu, dodając tag &lt;skryptu&gt; do strony, która wskazuje na ajax.aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="74875-134">For example, you can start using jQuery which is hosted on this CDN simply by adding a &lt;script&gt; tag to your page that points to ajax.aspnetcdn.com.</span></span>

<span data-ttu-id="74875-135">Dzięki wykorzystaniu usługi CDN można znacząco poprawić wydajność aplikacji AJAX.</span><span class="sxs-lookup"><span data-stu-id="74875-135">By taking advantage of the CDN, you can significantly improve the performance of your Ajax applications.</span></span> <span data-ttu-id="74875-136">Zawartość usługi CDN jest buforowana na serwerach znajdujących się na całym świecie.</span><span class="sxs-lookup"><span data-stu-id="74875-136">The contents of the CDN are cached on servers located around the world.</span></span> <span data-ttu-id="74875-137">Ponadto usługa CDN umożliwia przeglądarce ponowne używanie buforowanych plików JavaScript innych firm dla witryn sieci Web, które znajdują się w różnych domenach.</span><span class="sxs-lookup"><span data-stu-id="74875-137">In addition, the CDN enables browsers to reuse cached third party JavaScript files for web sites that are located in different domains.</span></span>

<span data-ttu-id="74875-138">Usługa CDN obsługuje protokół SSL (HTTPS) na wypadek, gdyby trzeba było obsłużyć stronę sieci Web przy użyciu SSL.</span><span class="sxs-lookup"><span data-stu-id="74875-138">The CDN supports SSL (HTTPS) in case you need to serve a web page using the Secure Sockets Layer.</span></span>

<span data-ttu-id="74875-139">Usługa CDN obsługuje następujące biblioteki skryptów innych firm, które zostały przekazane i są licencjonowane przez właścicieli tych bibliotek:</span><span class="sxs-lookup"><span data-stu-id="74875-139">The CDN hosts the following third party script libraries which have been uploaded, and are licensed to you, by the owners of those libraries:</span></span>

- <span data-ttu-id="74875-140">jQuery (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="74875-140">jQuery (www.jquery.com)</span></span>
- <span data-ttu-id="74875-141">jQuery UI (www.jqueryui.com)</span><span class="sxs-lookup"><span data-stu-id="74875-141">jQuery UI (www.jqueryui.com)</span></span>
- <span data-ttu-id="74875-142">jQuery Mobile (www.jquerymobile.com)</span><span class="sxs-lookup"><span data-stu-id="74875-142">jQuery Mobile (www.jquerymobile.com)</span></span>
- <span data-ttu-id="74875-143">Walidacja jQuery (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="74875-143">jQuery Validation (www.jquery.com)</span></span>
- <span data-ttu-id="74875-144">jQuery Cycle (www.malsup.com/jquery/cycle/)</span><span class="sxs-lookup"><span data-stu-id="74875-144">jQuery Cycle (www.malsup.com/jquery/cycle/)</span></span>
- <span data-ttu-id="74875-145">jQuery DataTables (http://datatables.net/)</span><span class="sxs-lookup"><span data-stu-id="74875-145">jQuery DataTables (http://datatables.net/)</span></span>

<span data-ttu-id="74875-146">Usługa Microsoft Ajax CDN zawiera również następujące biblioteki, które zostały przekazane przez firmę Microsoft:</span><span class="sxs-lookup"><span data-stu-id="74875-146">The Microsoft Ajax CDN also includes the following libraries which have been uploaded by Microsoft:</span></span>

- <span data-ttu-id="74875-147">ASP.NET AJAX</span><span class="sxs-lookup"><span data-stu-id="74875-147">ASP.NET Ajax</span></span>
- <span data-ttu-id="74875-148">Pliki JavaScript ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="74875-148">ASP.NET MVC JavaScript Files</span></span>
- <span data-ttu-id="74875-149">Pliki JavaScript ASP.NET sygnalizujący</span><span class="sxs-lookup"><span data-stu-id="74875-149">ASP.NET SignalR JavaScript Files</span></span>

<span data-ttu-id="74875-150">Firma Microsoft nie rości sobie praw własności do żadnych bibliotek innych firm hostowanych w tej sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="74875-150">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="74875-151">Właściciele praw autorskich bibliotek mają licencję na te biblioteki.</span><span class="sxs-lookup"><span data-stu-id="74875-151">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="74875-152">Wszelkie prawa, które mogą być konieczne do pobrania i użycia takich bibliotek, są przyznawane wyłącznie przez odpowiednich właścicieli praw autorskich.</span><span class="sxs-lookup"><span data-stu-id="74875-152">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="74875-153">Ponieważ nie są to biblioteki firmy Microsoft, firma Microsoft nie udziela żadnych gwarancji ani licencji praw własności intelektualnej (w tym braku domniemanych praw patentowych) dla bibliotek innych firm hostowanych w tej sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="74875-153">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<span data-ttu-id="74875-154">Jeśli chcesz przesłać bibliotekę JavaScript, a biblioteka jest jedną z najpopularniejszych bibliotek JavaScript (wymienionych w http://trends.builtwith.com) lub rozszerzenia/wtyczki do tych bibliotek, które są popularne; lub (b) pomocne do użycia w ASP.NET, skontaktuj się z AjaxCDNSubmission@Microsoft.com.</span><span class="sxs-lookup"><span data-stu-id="74875-154">If you wish to submit your JavaScript library and your library is one of the top JavaScript libraries (as listed on http://trends.builtwith.com) or extensions/plugins to these libraries that are (a) popular; or (b) helpful for use on ASP.NET then please contact AjaxCDNSubmission@Microsoft.com.</span></span>

<a id="ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18"></a>

## <a name="ajaxmicrosoftcom-renamed-to-ajaxaspnetcdncom"></a><span data-ttu-id="74875-155">ajax.microsoft.com zmieniono nazwę na ajax.aspnetcdn.com</span><span class="sxs-lookup"><span data-stu-id="74875-155">ajax.microsoft.com renamed to ajax.aspnetcdn.com</span></span>

<span data-ttu-id="74875-156">Usługa CDN użyta do użycia nazwy domeny microsoft.com i została zmieniona w celu używania nazwy domeny aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="74875-156">The CDN used to use the microsoft.com domain name and has been changed to use the aspnetcdn.com domain name.</span></span> <span data-ttu-id="74875-157">Ta zmiana została wprowadzona w celu zwiększenia wydajności, ponieważ gdy przeglądarka odwołuje się do domeny microsoft.com, wyśle wszystkie pliki cookie z tej domeny do każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="74875-157">This change was made to increase performance because when a browser referenced the microsoft.com domain it would send any cookies from that domain across the wire with each request.</span></span> <span data-ttu-id="74875-158">Zmiana nazwy domeny innej niż microsoft.com Performance może zostać zwiększona o do 25%.</span><span class="sxs-lookup"><span data-stu-id="74875-158">By renaming to a domain name other than microsoft.com performance can be increased by as much to 25%.</span></span> <span data-ttu-id="74875-159">Uwaga ajax.microsoft.com będzie nadal działać, ale ajax.aspnetcdn.com jest zalecana.</span><span class="sxs-lookup"><span data-stu-id="74875-159">Note ajax.microsoft.com will continue to function but ajax.aspnetcdn.com is recommended.</span></span>

- <span data-ttu-id="74875-160">Stary format: https://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="74875-160">Old Format: https://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span></span>
- <span data-ttu-id="74875-161">Nowy format: https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="74875-161">New Format: https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span></span>

<a id="Visual_Studio_vsdoc_Support_19"></a>

## <a name="visual-studio-vsdoc-support"></a><span data-ttu-id="74875-162">Obsługa programu Visual Studio. vsdoc</span><span class="sxs-lookup"><span data-stu-id="74875-162">Visual Studio .vsdoc Support</span></span>

<span data-ttu-id="74875-163">Aby poprawnie używać plików. vsdoc w programie Visual Studio 2008, należy upewnić się, że zainstalowano program VS 2008 SP1 i zainstalowano poprawkę dla plików vsdoc.</span><span class="sxs-lookup"><span data-stu-id="74875-163">To use the .vsdoc files properly with Visual Studio 2008 you need to make sure that you have VS 2008 SP1 installed and the hotfix for vsdoc files installed.</span></span> <span data-ttu-id="74875-164">Można je uzyskać z tego miejsca:</span><span class="sxs-lookup"><span data-stu-id="74875-164">You can get these from here:</span></span>

- [<span data-ttu-id="74875-165">Pobierz program Visual Studio 2008 z dodatkiem SP1</span><span class="sxs-lookup"><span data-stu-id="74875-165">Download Visual Studio 2008 SP1</span></span>](https://www.microsoft.com/downloads/en/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en "Pobierz program Visual Studio 2008 z dodatkiem SP1")
- [<span data-ttu-id="74875-166">Download. vsdoc hotfix dla programu Visual Studio 2008 z dodatkiem SP1</span><span class="sxs-lookup"><span data-stu-id="74875-166">Download .vsdoc hotfix for Visual Studio 2008 SP1</span></span>](https://code.msdn.microsoft.com/KB958502/Release/ProjectReleases.aspx?ReleaseId=1736 "Download. vsdoc hotfix dla programu Visual Studio 2008 z dodatkiem SP1")

<span data-ttu-id="74875-167">Program Visual Studio 2010 obsługuje pliki vsdoc bez żadnych dodatkowych poprawek.</span><span class="sxs-lookup"><span data-stu-id="74875-167">Visual Studio 2010 supports .vsdoc files without any additional patches.</span></span>

<a id="Using_ASPNET_Ajax_from_the_CDN_20"></a>

## <a name="using-aspnet-ajax-from-the-cdn"></a><span data-ttu-id="74875-168">Korzystanie z ASP.NET AJAX z sieci CDN</span><span class="sxs-lookup"><span data-stu-id="74875-168">Using ASP.NET Ajax from the CDN</span></span>

<span data-ttu-id="74875-169">W przypadku korzystania z programu ASP.NET 4 można przekierować wszystkie żądania skryptów programu ASP.NET Framework do sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="74875-169">When using ASP.NET 4, you can redirect all requests for ASP.NET framework scripts to the CDN.</span></span> <span data-ttu-id="74875-170">Pobieranie skryptów z usługi CDN zamiast lokalnego serwera sieci Web może znacznie poprawić wydajność publicznych witryn ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="74875-170">Retrieving scripts from the CDN instead of your local web server can substantially improve the performance of public ASP.NET websites.</span></span>

<span data-ttu-id="74875-171">Użyj właściwości ScriptManager EnableCDN, aby przekierować wszystkie żądania skryptu ASP.NET Framework do usługi Microsoft Ajax CDN:</span><span class="sxs-lookup"><span data-stu-id="74875-171">Use the ScriptManager EnableCDN property to redirect all ASP.NET framework script requests to the Microsoft Ajax CDN:</span></span>

[!code-aspx[Main](overview/samples/sample1.aspx)]

<a id="Using_jQuery_from_the_CDN_21"></a>

## <a name="using-jquery-from-the-cdn"></a><span data-ttu-id="74875-172">Korzystanie z platformy jQuery z sieci CDN</span><span class="sxs-lookup"><span data-stu-id="74875-172">Using jQuery from the CDN</span></span>

<span data-ttu-id="74875-173">Skrypty jQuery hostowane w usłudze CDN można używać w aplikacji sieci Web przez dodanie następującego elementu skryptu do strony:</span><span class="sxs-lookup"><span data-stu-id="74875-173">You can use jQuery scripts hosted on CDN in your Web application by adding the following script element to a page:</span></span>

[!code-html[Main](overview/samples/sample2.html)]

<span data-ttu-id="74875-174">Sieć CDN zawiera również wersję zminimalizowanego skryptu jQuery, którą można uzyskać przy użyciu następującego elementu:</span><span class="sxs-lookup"><span data-stu-id="74875-174">The CDN also includes the minified version of the jQuery script, which you can get using the following element:</span></span>

[!code-html[Main](overview/samples/sample3.html)]

<span data-ttu-id="74875-175">Aby umożliwić stronie powrót do ładowania jQuery ze ścieżki lokalnej do własnej witryny sieci Web, jeśli sieć CDN stanie się niedostępna, Dodaj następujący element bezpośrednio po elemencie, który odwołuje się do sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="74875-175">To allow your page to fallback to loading jQuery from a local path on your own website if the CDN happens to be unavailable, add the following element immediately after the element referencing the CDN:</span></span>

[!code-html[Main](overview/samples/sample4.html)]

<span data-ttu-id="74875-176">Na poniższej przykładowej stronie zostanie użyta wersja sieci CDN biblioteki jQuery (z alternatywą dla kopii lokalnej), aby wyświetlić zawartość elementu DIV po kliknięciu przycisku.</span><span class="sxs-lookup"><span data-stu-id="74875-176">The following sample page uses the CDN version of the jQuery library (with fallback to a local copy) to display the contents of a div element when a button is clicked.</span></span>

[!code-html[Main](overview/samples/sample5.html)]

<span data-ttu-id="74875-177">Aby dowiedzieć się więcej na temat jQuery i pobrać lokalną kopię platformy jQuery, odwiedź witrynę sieci Web [jQuery](http://jquery.com/) .</span><span class="sxs-lookup"><span data-stu-id="74875-177">You can learn more about jQuery and download a local copy of jQuery by visiting the [jQuery](http://jquery.com/) Web site.</span></span>

<a id="Using_jQuery_UI_from_the_CDN_22"></a>

## <a name="using-jquery-ui-from-the-cdn"></a><span data-ttu-id="74875-178">Korzystanie z interfejsu użytkownika jQuery z sieci CDN</span><span class="sxs-lookup"><span data-stu-id="74875-178">Using jQuery UI from the CDN</span></span>

<span data-ttu-id="74875-179">Usługa CDN obsługuje również bibliotekę interfejsu użytkownika jQuery.</span><span class="sxs-lookup"><span data-stu-id="74875-179">The CDN also hosts the jQuery UI library.</span></span> <span data-ttu-id="74875-180">Biblioteka interfejsu użytkownika jQuery zawiera rozbudowany zestaw elementów widget i efektów, których można używać w aplikacjach ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="74875-180">The jQuery UI library includes a rich set of widgets and effects that you can use in your ASP.NET applications.</span></span> <span data-ttu-id="74875-181">Na przykład na poniższej stronie przedstawiono, jak można użyć interfejsu użytkownika jQuery DatePicker w kontekście aplikacji formularzy sieci Web ASP.NET w celu wyświetlenia kalendarza podręcznego:</span><span class="sxs-lookup"><span data-stu-id="74875-181">For example, the following page illustrates how you can use the jQuery UI Datepicker in the context of an ASP.NET Web Forms application to display a pop-up calendar:</span></span>

[!code-aspx[Main](overview/samples/sample6.aspx)]

<span data-ttu-id="74875-182">Po przeniesieniu fokusu do pola tekstowego przy użyciu klawiatury zostanie wyświetlony kalendarz:</span><span class="sxs-lookup"><span data-stu-id="74875-182">When you move focus to the TextBox using your keyboard, a calendar is displayed:</span></span>

![Kalendarz podręczny utworzony przy użyciu DatePicker](overview/_static/image1.png)

<span data-ttu-id="74875-184">Zwróć uwagę, że w powyższym kodzie musisz dołączyć trzy pliki z sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="74875-184">Notice that you must include three files from the CDN in the code above:</span></span>

- <span data-ttu-id="74875-185">Biblioteka jQuery &mdash; Biblioteka interfejsu użytkownika jQuery zależy od biblioteki jQuery.</span><span class="sxs-lookup"><span data-stu-id="74875-185">The jQuery library &mdash; The jQuery UI library depends on the jQuery library.</span></span> <span data-ttu-id="74875-186">Przed dodaniem biblioteki interfejsu użytkownika jQuery należy dodać bibliotekę jQuery do strony.</span><span class="sxs-lookup"><span data-stu-id="74875-186">You must add the jQuery library to your page before you add the jQuery UI library.</span></span>
- <span data-ttu-id="74875-187">Biblioteka interfejsu użytkownika jQuery &mdash; Biblioteka interfejsu użytkownika jQuery zawiera wszystkie efekty interfejsu użytkownika jQuery i widżety, takie jak widżet DatePicker używany na powyższej stronie.</span><span class="sxs-lookup"><span data-stu-id="74875-187">The jQuery UI library &mdash; The jQuery UI library contains all of the jQuery UI effects and widgets such as the Datepicker widget used in the page above.</span></span>
- <span data-ttu-id="74875-188">Motyw interfejsu użytkownika jQuery &mdash; interfejs użytkownika jQuery obsługuje różne motywy.</span><span class="sxs-lookup"><span data-stu-id="74875-188">A jQuery UI theme &mdash; The jQuery UI supports different themes.</span></span> <span data-ttu-id="74875-189">Powyższa Strona zawiera link do pliku CSS, aby zaimportować motyw Redmond.</span><span class="sxs-lookup"><span data-stu-id="74875-189">The page above includes a link to a CSS file to import the Redmond theme.</span></span>

<span data-ttu-id="74875-190">Wszystkie standardowe motywy interfejsu użytkownika jQuery są hostowane w sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="74875-190">All of the standard jQuery UI themes are hosted on the CDN.</span></span> <span data-ttu-id="74875-191">[Odwiedź tę stronę](jquery-ui/cdnjqueryui1910.md "j1\.8.10 interfejsu użytkownika zapytania w usłudze Microsoft Ajax CDN) , aby wyświetlić miniatury dla każdego motywu.</span><span class="sxs-lookup"><span data-stu-id="74875-191">[Visit this page](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 on the Microsoft Ajax CDN") to view thumbnails for each theme.</span></span>

<span data-ttu-id="74875-192">Aby dowiedzieć się więcej na temat biblioteki interfejsu użytkownika jQuery, odwiedź [witrynę internetową interfejsu użytkownika programu jQuery](http://jQueryUI.com "Witryna sieci Web interfejsu użytkownika jQuery").</span><span class="sxs-lookup"><span data-stu-id="74875-192">To learn more about the jQuery UI library, visit the official [jQuery UI website](http://jQueryUI.com "jQuery UI website").</span></span>

<a id="Third-Party_Files_on_the_CDN_23"></a>

## <a name="third-party-files-on-the-cdn"></a><span data-ttu-id="74875-193">Pliki innych firm w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="74875-193">Third-Party Files on the CDN</span></span>

<span data-ttu-id="74875-194">Sieć CDN zawiera najpopularniejsze biblioteki języka JavaScript oferowane przez inne firmy.</span><span class="sxs-lookup"><span data-stu-id="74875-194">The CDN hosts some of the most popular third party JavaScript libraries.</span></span> <span data-ttu-id="74875-195">Firma Microsoft nie rości sobie praw własności do żadnych bibliotek innych firm hostowanych w tej sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="74875-195">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="74875-196">Właściciele praw autorskich bibliotek mają licencję na te biblioteki.</span><span class="sxs-lookup"><span data-stu-id="74875-196">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="74875-197">Wszelkie prawa, które mogą być konieczne do pobrania i użycia takich bibliotek, są przyznawane wyłącznie przez odpowiednich właścicieli praw autorskich.</span><span class="sxs-lookup"><span data-stu-id="74875-197">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="74875-198">Ponieważ nie są to biblioteki firmy Microsoft, firma Microsoft nie udziela żadnych gwarancji ani licencji praw własności intelektualnej (w tym braku domniemanych praw patentowych) dla bibliotek innych firm hostowanych w tej sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="74875-198">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<a id="jQuery_Releases_on_the_CDN_0"></a>

### <a name="jquery-releases-on-the-cdn"></a><span data-ttu-id="74875-199">Wersje jQuery w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="74875-199">jQuery Releases on the CDN</span></span>

<span data-ttu-id="74875-200">Następujące wersje jQuery są hostowane w sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="74875-200">The following releases of jQuery are hosted on the CDN:</span></span>

#### <a name="jquery-version-341"></a><span data-ttu-id="74875-201">wersja jQuery</span><span class="sxs-lookup"><span data-stu-id="74875-201">jQuery version 3.4.1</span></span>
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.1.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.1.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.1.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.1.slim.min.map

#### <a name="jquery-version-340"></a><span data-ttu-id="74875-202">3\.4.0 wersja jQuery</span><span class="sxs-lookup"><span data-stu-id="74875-202">jQuery version 3.4.0</span></span>
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.0.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.0.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.0.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.0.slim.min.map

#### <a name="jquery-version-331"></a><span data-ttu-id="74875-203">jQuery wersja 3.3.1</span><span class="sxs-lookup"><span data-stu-id="74875-203">jQuery version 3.3.1</span></span>
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.map

#### <a name="jquery-version-321"></a><span data-ttu-id="74875-204">jQuery w wersji 3.2.1</span><span class="sxs-lookup"><span data-stu-id="74875-204">jQuery version 3.2.1</span></span>
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.map

#### <a name="jquery-version-320"></a><span data-ttu-id="74875-205">3\.2.0 wersja jQuery</span><span class="sxs-lookup"><span data-stu-id="74875-205">jQuery version 3.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.map

#### <a name="jquery-version-311"></a><span data-ttu-id="74875-206">jQuery w wersji 3.1.1</span><span class="sxs-lookup"><span data-stu-id="74875-206">jQuery version 3.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.map

#### <a name="jquery-version-310"></a><span data-ttu-id="74875-207">3\.1.0 wersja jQuery</span><span class="sxs-lookup"><span data-stu-id="74875-207">jQuery version 3.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.map

#### <a name="jquery-version-300"></a><span data-ttu-id="74875-208">3\.0.0 wersja jQuery</span><span class="sxs-lookup"><span data-stu-id="74875-208">jQuery version 3.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.map

#### <a name="jquery-version-224"></a><span data-ttu-id="74875-209">jQuery w wersji 2.2.4</span><span class="sxs-lookup"><span data-stu-id="74875-209">jQuery version 2.2.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.map

#### <a name="jquery-version-223"></a><span data-ttu-id="74875-210">jQuery w wersji 2.2.3</span><span class="sxs-lookup"><span data-stu-id="74875-210">jQuery version 2.2.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.map

#### <a name="jquery-version-222"></a><span data-ttu-id="74875-211">jQuery, wersja 2.2.2</span><span class="sxs-lookup"><span data-stu-id="74875-211">jQuery version 2.2.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.map

#### <a name="jquery-version-221"></a><span data-ttu-id="74875-212">jQuery, wersja 2.2.1</span><span class="sxs-lookup"><span data-stu-id="74875-212">jQuery version 2.2.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.map

#### <a name="jquery-version-220"></a><span data-ttu-id="74875-213">2\.2.0 wersja jQuery</span><span class="sxs-lookup"><span data-stu-id="74875-213">jQuery version 2.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.map

#### <a name="jquery-version-214"></a><span data-ttu-id="74875-214">jQuery w wersji 2.1.4</span><span class="sxs-lookup"><span data-stu-id="74875-214">jQuery version 2.1.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.map

#### <a name="jquery-version-213"></a><span data-ttu-id="74875-215">jQuery w wersji 2.1.3</span><span class="sxs-lookup"><span data-stu-id="74875-215">jQuery version 2.1.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.map

#### <a name="jquery-version-212"></a><span data-ttu-id="74875-216">jQuery w wersji 2.1.2</span><span class="sxs-lookup"><span data-stu-id="74875-216">jQuery version 2.1.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.min.js

#### <a name="jquery-version-211"></a><span data-ttu-id="74875-217">jQuery w wersji 2.1.1</span><span class="sxs-lookup"><span data-stu-id="74875-217">jQuery version 2.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.map

#### <a name="jquery-version-210"></a><span data-ttu-id="74875-218">2\.1.0 wersja jQuery</span><span class="sxs-lookup"><span data-stu-id="74875-218">jQuery version 2.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.map

#### <a name="jquery-version-203"></a><span data-ttu-id="74875-219">2\.0.3 wersja jQuery</span><span class="sxs-lookup"><span data-stu-id="74875-219">jQuery version 2.0.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.map

#### <a name="jquery-version-202"></a><span data-ttu-id="74875-220">2\.0.2 wersja jQuery</span><span class="sxs-lookup"><span data-stu-id="74875-220">jQuery version 2.0.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.map

#### <a name="jquery-version-201"></a><span data-ttu-id="74875-221">jQuery w wersji 2.0.1</span><span class="sxs-lookup"><span data-stu-id="74875-221">jQuery version 2.0.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.map

#### <a name="jquery-version-200"></a><span data-ttu-id="74875-222">2\.0.0 wersja jQuery</span><span class="sxs-lookup"><span data-stu-id="74875-222">jQuery version 2.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.map

#### <a name="jquery-version-1124"></a><span data-ttu-id="74875-223">1\.12.4 wersja jQuery</span><span class="sxs-lookup"><span data-stu-id="74875-223">jQuery version 1.12.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.map

#### <a name="jquery-version-1123"></a><span data-ttu-id="74875-224">1\.12.3 wersja jQuery</span><span class="sxs-lookup"><span data-stu-id="74875-224">jQuery version 1.12.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.map

#### <a name="jquery-version-1122"></a><span data-ttu-id="74875-225">1\.12.2 wersja jQuery</span><span class="sxs-lookup"><span data-stu-id="74875-225">jQuery version 1.12.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.map

#### <a name="jquery-version-1121"></a><span data-ttu-id="74875-226">1\.12.1 wersja jQuery</span><span class="sxs-lookup"><span data-stu-id="74875-226">jQuery version 1.12.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.map

#### <a name="jquery-version-1120"></a><span data-ttu-id="74875-227">1\.12.0 wersja jQuery</span><span class="sxs-lookup"><span data-stu-id="74875-227">jQuery version 1.12.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.map

#### <a name="jquery-version-1113"></a><span data-ttu-id="74875-228">1\.11.3 wersja jQuery</span><span class="sxs-lookup"><span data-stu-id="74875-228">jQuery version 1.11.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.map

#### <a name="jquery-version-1112"></a><span data-ttu-id="74875-229">1\.11.2 wersja jQuery</span><span class="sxs-lookup"><span data-stu-id="74875-229">jQuery version 1.11.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.map

#### <a name="jquery-version-1111"></a><span data-ttu-id="74875-230">1\.11.1 wersja jQuery</span><span class="sxs-lookup"><span data-stu-id="74875-230">jQuery version 1.11.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.map

#### <a name="jquery-version-1110"></a><span data-ttu-id="74875-231">1\.11.0 wersja jQuery</span><span class="sxs-lookup"><span data-stu-id="74875-231">jQuery version 1.11.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.map

#### <a name="jquery-version-1102"></a><span data-ttu-id="74875-232">1\.10.2 wersja jQuery</span><span class="sxs-lookup"><span data-stu-id="74875-232">jQuery version 1.10.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.map

#### <a name="jquery-version-1101"></a><span data-ttu-id="74875-233">1\.10.1 wersja jQuery</span><span class="sxs-lookup"><span data-stu-id="74875-233">jQuery version 1.10.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.map

#### <a name="jquery-version-1100"></a><span data-ttu-id="74875-234">1\.10.0 wersja jQuery</span><span class="sxs-lookup"><span data-stu-id="74875-234">jQuery version 1.10.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.map

#### <a name="jquery-version-191"></a><span data-ttu-id="74875-235">1\.9.1 wersja jQuery</span><span class="sxs-lookup"><span data-stu-id="74875-235">jQuery version 1.9.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.map

#### <a name="jquery-version-190"></a><span data-ttu-id="74875-236">1\.9.0 wersja jQuery</span><span class="sxs-lookup"><span data-stu-id="74875-236">jQuery version 1.9.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.map

#### <a name="jquery-version-183"></a><span data-ttu-id="74875-237">1\.8.3 wersja jQuery</span><span class="sxs-lookup"><span data-stu-id="74875-237">jQuery version 1.8.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3-vsdoc.js

#### <a name="jquery-version-182"></a><span data-ttu-id="74875-238">1\.8.2 wersja jQuery</span><span class="sxs-lookup"><span data-stu-id="74875-238">jQuery version 1.8.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2-vsdoc.js

#### <a name="jquery-version-181"></a><span data-ttu-id="74875-239">1\.8.1 wersja jQuery</span><span class="sxs-lookup"><span data-stu-id="74875-239">jQuery version 1.8.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1-vsdoc.js

#### <a name="jquery-version-180"></a><span data-ttu-id="74875-240">1\.8.0 wersja jQuery</span><span class="sxs-lookup"><span data-stu-id="74875-240">jQuery version 1.8.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0-vsdoc.js

#### <a name="jquery-version-172"></a><span data-ttu-id="74875-241">1\.7.2 wersja jQuery</span><span class="sxs-lookup"><span data-stu-id="74875-241">jQuery version 1.7.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.min.js

#### <a name="jquery-version-171"></a><span data-ttu-id="74875-242">1\.7.1 wersja jQuery</span><span class="sxs-lookup"><span data-stu-id="74875-242">jQuery version 1.7.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1-vsdoc.js

#### <a name="jquery-version-17"></a><span data-ttu-id="74875-243">jQuery w wersji 1,7</span><span class="sxs-lookup"><span data-stu-id="74875-243">jQuery version 1.7</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7-vsdoc.js

#### <a name="jquery-version-164"></a><span data-ttu-id="74875-244">1\.6.4 wersja jQuery</span><span class="sxs-lookup"><span data-stu-id="74875-244">jQuery version 1.6.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4-vsdoc.js

#### <a name="jquery-version-163"></a><span data-ttu-id="74875-245">1\.6.3 wersja jQuery</span><span class="sxs-lookup"><span data-stu-id="74875-245">jQuery version 1.6.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3-vsdoc.js

#### <a name="jquery-version-162"></a><span data-ttu-id="74875-246">jQuery wersja 1.6.2</span><span class="sxs-lookup"><span data-stu-id="74875-246">jQuery version 1.6.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2-vsdoc.js

#### <a name="jquery-version-161"></a><span data-ttu-id="74875-247">1\.6.1 wersja jQuery</span><span class="sxs-lookup"><span data-stu-id="74875-247">jQuery version 1.6.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1-vsdoc.js

#### <a name="jquery-version-16"></a><span data-ttu-id="74875-248">jQuery w wersji 1,6</span><span class="sxs-lookup"><span data-stu-id="74875-248">jQuery version 1.6</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6-vsdoc.js

#### <a name="jquery-version-152"></a><span data-ttu-id="74875-249">1\.5.2 wersja jQuery</span><span class="sxs-lookup"><span data-stu-id="74875-249">jQuery version 1.5.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2-vsdoc.js

#### <a name="jquery-version-151"></a><span data-ttu-id="74875-250">1\.5.1 wersja jQuery</span><span class="sxs-lookup"><span data-stu-id="74875-250">jQuery version 1.5.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1-vsdoc.js

#### <a name="jquery-version-15"></a><span data-ttu-id="74875-251">jQuery w wersji 1,5</span><span class="sxs-lookup"><span data-stu-id="74875-251">jQuery version 1.5</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5-vsdoc.js

#### <a name="jquery-version-144"></a><span data-ttu-id="74875-252">1\.4.4 wersja jQuery</span><span class="sxs-lookup"><span data-stu-id="74875-252">jQuery version 1.4.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4-vsdoc.js

#### <a name="jquery-version-143"></a><span data-ttu-id="74875-253">jQuery wersja 1.4.3</span><span class="sxs-lookup"><span data-stu-id="74875-253">jQuery version 1.4.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3-vsdoc.js

#### <a name="jquery-version-142"></a><span data-ttu-id="74875-254">jQuery, wersja 1.4.2</span><span class="sxs-lookup"><span data-stu-id="74875-254">jQuery version 1.4.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2-vsdoc.js

#### <a name="jquery-version-141"></a><span data-ttu-id="74875-255">jQuery w wersji 1.4.1</span><span class="sxs-lookup"><span data-stu-id="74875-255">jQuery version 1.4.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1-vsdoc.js

#### <a name="jquery-version-14"></a><span data-ttu-id="74875-256">jQuery w wersji 1,4</span><span class="sxs-lookup"><span data-stu-id="74875-256">jQuery version 1.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.min.js

#### <a name="jquery-version-132"></a><span data-ttu-id="74875-257">jQuery Version 1.3.2</span><span class="sxs-lookup"><span data-stu-id="74875-257">jQuery version 1.3.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min-vsdoc.js

<a id="jQuery_Migrate_Releases_on_the_CDN_1"></a>

### <a name="jquery-migrate-releases-on-the-cdn"></a><span data-ttu-id="74875-258">Usługa jQuery migruje wydania w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="74875-258">jQuery Migrate Releases on the CDN</span></span>

<span data-ttu-id="74875-259">Następujące wersje migracji jQuery są hostowane w sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="74875-259">The following releases of jQuery Migrate are hosted on the CDN:</span></span>

#### <a name="jquery-migrate-version-300"></a><span data-ttu-id="74875-260">Migrowanie w wersji jQuery 3.0.0</span><span class="sxs-lookup"><span data-stu-id="74875-260">jQuery Migrate version 3.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.min.js

#### <a name="jquery-migrate-version-121"></a><span data-ttu-id="74875-261">Migrowanie oprogramowania jQuery w wersji 1.2.1</span><span class="sxs-lookup"><span data-stu-id="74875-261">jQuery Migrate version 1.2.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.min.js

<span data-ttu-id="74875-262">Migrowanie w wersji jQuery 1.2.0</span><span class="sxs-lookup"><span data-stu-id="74875-262">jQuery Migrate version 1.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.min.js

#### <a name="jquery-migrate-version-111"></a><span data-ttu-id="74875-263">Migrowanie oprogramowania jQuery w wersji 1.1.1</span><span class="sxs-lookup"><span data-stu-id="74875-263">jQuery Migrate version 1.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.min.js

#### <a name="jquery-migrate-version-110"></a><span data-ttu-id="74875-264">Migrowanie w wersji jQuery 1.1.0</span><span class="sxs-lookup"><span data-stu-id="74875-264">jQuery Migrate version 1.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.min.js

#### <a name="jquery-migrate-version-100"></a><span data-ttu-id="74875-265">Migrowanie w wersji jQuery 1.0.0</span><span class="sxs-lookup"><span data-stu-id="74875-265">jQuery Migrate version 1.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.min.js

<a id="jQuery_UI_Releases_on_the_CDN_2"></a>

### <a name="jquery-ui-releases-on-the-cdn"></a><span data-ttu-id="74875-266">Wersje interfejsu użytkownika jQuery w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="74875-266">jQuery UI Releases on the CDN</span></span>

<span data-ttu-id="74875-267">Następujące wersje biblioteki interfejsu użytkownika jQuery są hostowane w tej sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="74875-267">The following releases of the jQuery UI library are hosted on this CDN.</span></span> <span data-ttu-id="74875-268">Kliknij każdy link, aby zobaczyć rzeczywistą listę plików.</span><span class="sxs-lookup"><span data-stu-id="74875-268">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="74875-269">Interfejs użytkownika jQuery 1.12.1</span><span class="sxs-lookup"><span data-stu-id="74875-269">jQuery UI 1.12.1</span></span>](jquery-ui/cdnjqueryui1121.md "Zestaw jQuery UI 1.12.1 w usłudze Microsoft AJAX CDN")
- [<span data-ttu-id="74875-270">Interfejs użytkownika jQuery 1.12.0</span><span class="sxs-lookup"><span data-stu-id="74875-270">jQuery UI 1.12.0</span></span>](jquery-ui/cdnjqueryui1120.md "Zestaw jQuery UI 1.12.0 w usłudze Microsoft AJAX CDN")
- [<span data-ttu-id="74875-271">Interfejs użytkownika jQuery 1.11.4</span><span class="sxs-lookup"><span data-stu-id="74875-271">jQuery UI 1.11.4</span></span>](jquery-ui/cdnjqueryui1114.md "Zestaw jQuery UI 1.11.4 w usłudze Microsoft AJAX CDN")
- [<span data-ttu-id="74875-272">Interfejs użytkownika jQuery 1.11.3</span><span class="sxs-lookup"><span data-stu-id="74875-272">jQuery UI 1.11.3</span></span>](jquery-ui/cdnjqueryui1113.md "Zestaw jQuery UI 1.11.3 w usłudze Microsoft AJAX CDN")
- [<span data-ttu-id="74875-273">Interfejs użytkownika jQuery 1.11.2</span><span class="sxs-lookup"><span data-stu-id="74875-273">jQuery UI 1.11.2</span></span>](jquery-ui/cdnjqueryui1112.md "Zestaw jQuery UI 1.11.2 w usłudze Microsoft AJAX CDN")
- [<span data-ttu-id="74875-274">Interfejs użytkownika jQuery 1.11.1</span><span class="sxs-lookup"><span data-stu-id="74875-274">jQuery UI 1.11.1</span></span>](jquery-ui/cdnjqueryui1111.md "Zestaw jQuery UI 1.11.1 w usłudze Microsoft AJAX CDN")
- [<span data-ttu-id="74875-275">Interfejs użytkownika jQuery 1.11.0</span><span class="sxs-lookup"><span data-stu-id="74875-275">jQuery UI 1.11.0</span></span>](jquery-ui/cdnjqueryui1110.md "Zestaw jQuery UI 1.11.0 w usłudze Microsoft AJAX CDN")
- [<span data-ttu-id="74875-276">Interfejs użytkownika jQuery 1.10.4</span><span class="sxs-lookup"><span data-stu-id="74875-276">jQuery UI 1.10.4</span></span>](jquery-ui/cdnjqueryui1104.md "Zestaw jQuery UI 1.10.4 w usłudze Microsoft AJAX CDN")
- [<span data-ttu-id="74875-277">Interfejs użytkownika jQuery 1.10.3</span><span class="sxs-lookup"><span data-stu-id="74875-277">jQuery UI 1.10.3</span></span>](jquery-ui/cdnjqueryui1103.md "Zestaw jQuery UI 1.10.3 w usłudze Microsoft AJAX CDN")
- [<span data-ttu-id="74875-278">Interfejs użytkownika jQuery 1.10.2</span><span class="sxs-lookup"><span data-stu-id="74875-278">jQuery UI 1.10.2</span></span>](jquery-ui/cdnjqueryui1102.md "Zestaw jQuery UI 1.10.2 w usłudze Microsoft AJAX CDN")
- [<span data-ttu-id="74875-279">Interfejs użytkownika jQuery 1.10.1</span><span class="sxs-lookup"><span data-stu-id="74875-279">jQuery UI 1.10.1</span></span>](jquery-ui/cdnjqueryui1101.md "Zestaw jQuery UI 1.10.1 w usłudze Microsoft AJAX CDN")
- [<span data-ttu-id="74875-280">Interfejs użytkownika jQuery 1.10.0</span><span class="sxs-lookup"><span data-stu-id="74875-280">jQuery UI 1.10.0</span></span>](jquery-ui/cdnjqueryui1100.md "Zestaw jQuery UI 1.10.0 w usłudze Microsoft AJAX CDN")
- [<span data-ttu-id="74875-281">Interfejs użytkownika jQuery 1.9.2</span><span class="sxs-lookup"><span data-stu-id="74875-281">jQuery UI 1.9.2</span></span>](jquery-ui/cdnjqueryui192.md "Zestaw jQuery UI 1.9.2 w usłudze Microsoft AJAX CDN")
- [<span data-ttu-id="74875-282">Interfejs użytkownika jQuery 1.9.1</span><span class="sxs-lookup"><span data-stu-id="74875-282">jQuery UI 1.9.1</span></span>](jquery-ui/cdnjqueryui191.md "Zestaw jQuery UI 1.9.1 w usłudze Microsoft AJAX CDN")
- [<span data-ttu-id="74875-283">Interfejs użytkownika jQuery 1.9.0</span><span class="sxs-lookup"><span data-stu-id="74875-283">jQuery UI 1.9.0</span></span>](jquery-ui/cdnjqueryui190.md "Zestaw jQuery UI 1.9.0 w usłudze Microsoft AJAX CDN")
- [<span data-ttu-id="74875-284">Interfejs użytkownika jQuery 1.8.24</span><span class="sxs-lookup"><span data-stu-id="74875-284">jQuery UI 1.8.24</span></span>](jquery-ui/cdnjqueryui1824.md "Zestaw jQuery UI 1.8.24 w usłudze Microsoft AJAX CDN")
- [<span data-ttu-id="74875-285">Interfejs użytkownika jQuery 1.8.23</span><span class="sxs-lookup"><span data-stu-id="74875-285">jQuery UI 1.8.23</span></span>](jquery-ui/cdnjqueryui1823.md "Zestaw jQuery UI 1.8.23 w usłudze Microsoft AJAX CDN")
- [<span data-ttu-id="74875-286">Interfejs użytkownika jQuery 1.8.22</span><span class="sxs-lookup"><span data-stu-id="74875-286">jQuery UI 1.8.22</span></span>](jquery-ui/cdnjqueryui1822.md "Zestaw jQuery UI 1.8.22 w usłudze Microsoft AJAX CDN")
- [<span data-ttu-id="74875-287">Interfejs użytkownika jQuery 1.8.21</span><span class="sxs-lookup"><span data-stu-id="74875-287">jQuery UI 1.8.21</span></span>](jquery-ui/cdnjqueryui1821.md "Zestaw jQuery UI 1.8.21 w usłudze Microsoft AJAX CDN")
- [<span data-ttu-id="74875-288">Interfejs użytkownika jQuery 1.8.20</span><span class="sxs-lookup"><span data-stu-id="74875-288">jQuery UI 1.8.20</span></span>](jquery-ui/cdnjqueryui1820.md "Zestaw jQuery UI 1.8.20 w usłudze Microsoft AJAX CDN")
- [<span data-ttu-id="74875-289">Interfejs użytkownika jQuery 1.8.19</span><span class="sxs-lookup"><span data-stu-id="74875-289">jQuery UI 1.8.19</span></span>](jquery-ui/cdnjqueryui1819.md "Zestaw jQuery UI 1.8.19 w usłudze Microsoft AJAX CDN")
- [<span data-ttu-id="74875-290">Interfejs użytkownika jQuery 1.8.18</span><span class="sxs-lookup"><span data-stu-id="74875-290">jQuery UI 1.8.18</span></span>](jquery-ui/cdnjqueryui1818.md "Zestaw jQuery UI 1.8.18 w usłudze Microsoft AJAX CDN")
- [<span data-ttu-id="74875-291">Interfejs użytkownika jQuery 1.8.17</span><span class="sxs-lookup"><span data-stu-id="74875-291">jQuery UI 1.8.17</span></span>](jquery-ui/cdnjqueryui1817.md "Zestaw jQuery UI 1.8.17 w usłudze Microsoft AJAX CDN")
- [<span data-ttu-id="74875-292">Interfejs użytkownika jQuery 1.8.16</span><span class="sxs-lookup"><span data-stu-id="74875-292">jQuery UI 1.8.16</span></span>](jquery-ui/cdnjqueryui1816.md "Zestaw jQuery UI 1.8.16 w usłudze Microsoft AJAX CDN")
- [<span data-ttu-id="74875-293">Interfejs użytkownika jQuery 1.8.15</span><span class="sxs-lookup"><span data-stu-id="74875-293">jQuery UI 1.8.15</span></span>](jquery-ui/cdnjqueryui1815.md "Zestaw jQuery UI 1.8.15 w usłudze Microsoft AJAX CDN")
- [<span data-ttu-id="74875-294">Interfejs użytkownika jQuery 1.8.14</span><span class="sxs-lookup"><span data-stu-id="74875-294">jQuery UI 1.8.14</span></span>](jquery-ui/cdnjqueryui1814.md "Zestaw jQuery UI 1.8.14 w usłudze Microsoft AJAX CDN")
- [<span data-ttu-id="74875-295">Interfejs użytkownika jQuery 1.8.13</span><span class="sxs-lookup"><span data-stu-id="74875-295">jQuery UI 1.8.13</span></span>](jquery-ui/cdnjqueryui1813.md "Zestaw jQuery UI 1.8.13 w usłudze Microsoft AJAX CDN")
- [<span data-ttu-id="74875-296">Interfejs użytkownika jQuery 1.8.12</span><span class="sxs-lookup"><span data-stu-id="74875-296">jQuery UI 1.8.12</span></span>](jquery-ui/cdnjqueryui1812.md "Zestaw jQuery UI 1.8.12 w usłudze Microsoft AJAX CDN")
- [<span data-ttu-id="74875-297">Interfejs użytkownika jQuery 1.8.11</span><span class="sxs-lookup"><span data-stu-id="74875-297">jQuery UI 1.8.11</span></span>](jquery-ui/cdnjqueryui1811.md "Zestaw jQuery UI 1.8.11 w usłudze Microsoft AJAX CDN")
- [<span data-ttu-id="74875-298">Interfejs użytkownika jQuery 1.8.10</span><span class="sxs-lookup"><span data-stu-id="74875-298">jQuery UI 1.8.10</span></span>](jquery-ui/cdnjqueryui1910.md "Zestaw jQuery UI 1.8.10 w usłudze Microsoft AJAX CDN")
- [<span data-ttu-id="74875-299">Interfejs użytkownika jQuery 1.8.9</span><span class="sxs-lookup"><span data-stu-id="74875-299">jQuery UI 1.8.9</span></span>](jquery-ui/cdnjqueryui189.md "Zestaw jQuery UI 1.8.9 w usłudze Microsoft AJAX CDN")
- [<span data-ttu-id="74875-300">Interfejs użytkownika jQuery 1.8.8</span><span class="sxs-lookup"><span data-stu-id="74875-300">jQuery UI 1.8.8</span></span>](jquery-ui/cdnjqueryui188.md "Zestaw jQuery UI 1.8.8 w usłudze Microsoft AJAX CDN")
- [<span data-ttu-id="74875-301">Interfejs użytkownika jQuery 1.8.7</span><span class="sxs-lookup"><span data-stu-id="74875-301">jQuery UI 1.8.7</span></span>](jquery-ui/cdnjqueryui187.md "Zestaw jQuery UI 1.8.7 w usłudze Microsoft AJAX CDN")
- [<span data-ttu-id="74875-302">Interfejs użytkownika jQuery 1.8.6</span><span class="sxs-lookup"><span data-stu-id="74875-302">jQuery UI 1.8.6</span></span>](jquery-ui/cdnjqueryui186.md "Zestaw jQuery UI 1.8.6 w usłudze Microsoft AJAX CDN")
- [<span data-ttu-id="74875-303">Interfejs użytkownika jQuery 1.8.5</span><span class="sxs-lookup"><span data-stu-id="74875-303">jQuery UI 1.8.5</span></span>](jquery-ui/cdnjqueryui185.md "jQuery UI 1.8.5")

<a id="jQuery_Validation_Releases_on_the_CDN_3"></a>

### <a name="jquery-validation-releases-on-the-cdn"></a><span data-ttu-id="74875-304">Wersje walidacji jQuery w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="74875-304">jQuery Validation Releases on the CDN</span></span>

<span data-ttu-id="74875-305">Następujące wersje biblioteki walidacji jQuery są hostowane w tej sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="74875-305">The following releases of the jQuery Validation library are hosted on this CDN.</span></span> <span data-ttu-id="74875-306">Kliknij każdy link, aby zobaczyć rzeczywistą listę plików.</span><span class="sxs-lookup"><span data-stu-id="74875-306">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="74875-307">jQuery Weryfikuj 1.19.1</span><span class="sxs-lookup"><span data-stu-id="74875-307">jQuery Validate 1.19.1</span></span>](jquery-validate/cdnjqueryvalidate1191.md "Sprawdzanie poprawności jQuery 1.19.1")
- [<span data-ttu-id="74875-308">jQuery Weryfikuj 1.19.0</span><span class="sxs-lookup"><span data-stu-id="74875-308">jQuery Validate 1.19.0</span></span>](jquery-validate/cdnjqueryvalidate1190.md "Sprawdzanie poprawności jQuery 1.19.0")
- [<span data-ttu-id="74875-309">jQuery Weryfikuj 1.17.0</span><span class="sxs-lookup"><span data-stu-id="74875-309">jQuery Validate 1.17.0</span></span>](jquery-validate/cdnjqueryvalidate1170.md "Sprawdzanie poprawności jQuery 1.17.0")
- [<span data-ttu-id="74875-310">jQuery Weryfikuj 1.16.0</span><span class="sxs-lookup"><span data-stu-id="74875-310">jQuery Validate 1.16.0</span></span>](jquery-validate/cdnjqueryvalidate1160.md "jQuery Validation 1.16.0")
- [<span data-ttu-id="74875-311">jQuery Weryfikuj 1.15.1</span><span class="sxs-lookup"><span data-stu-id="74875-311">jQuery Validate 1.15.1</span></span>](jquery-validate/cdnjqueryvalidate1151.md "jQuery Validation 1.15.1")
- [<span data-ttu-id="74875-312">jQuery Weryfikuj 1.15.0</span><span class="sxs-lookup"><span data-stu-id="74875-312">jQuery Validate 1.15.0</span></span>](jquery-validate/cdnjqueryvalidate1150.md "jQuery Validation 1.15.0")
- [<span data-ttu-id="74875-313">jQuery Weryfikuj 1.14.0</span><span class="sxs-lookup"><span data-stu-id="74875-313">jQuery Validate 1.14.0</span></span>](jquery-validate/cdnjqueryvalidate1140.md "jQuery Validation 1.14.0")
- [<span data-ttu-id="74875-314">jQuery Weryfikuj 1.13.1</span><span class="sxs-lookup"><span data-stu-id="74875-314">jQuery Validate 1.13.1</span></span>](jquery-validate/cdnjqueryvalidate1131.md "jQuery Validation 1.13.1")
- [<span data-ttu-id="74875-315">jQuery Weryfikuj 1.13.0</span><span class="sxs-lookup"><span data-stu-id="74875-315">jQuery Validate 1.13.0</span></span>](jquery-validate/cdnjqueryvalidate1130.md "jQuery Validation 1.13.0")
- [<span data-ttu-id="74875-316">jQuery Weryfikuj 1.12.0</span><span class="sxs-lookup"><span data-stu-id="74875-316">jQuery Validate 1.12.0</span></span>](jquery-validate/cdnjqueryvalidate1120.md "jQuery Validation 1.12.0")
- [<span data-ttu-id="74875-317">jQuery Weryfikuj 1.11.1</span><span class="sxs-lookup"><span data-stu-id="74875-317">jQuery Validate 1.11.1</span></span>](jquery-validate/cdnjqueryvalidate1111.md "jQuery Validation 1.11.1")
- [<span data-ttu-id="74875-318">jQuery Weryfikuj 1.11.0</span><span class="sxs-lookup"><span data-stu-id="74875-318">jQuery Validate 1.11.0</span></span>](jquery-validate/cdnjqueryvalidate111.md "jQuery Validation 1.11.0")
- [<span data-ttu-id="74875-319">jQuery Weryfikuj 1.10.0</span><span class="sxs-lookup"><span data-stu-id="74875-319">jQuery Validate 1.10.0</span></span>](jquery-validate/cdnjqueryvalidate110.md "jQuery Validation 1.10.0")
- [<span data-ttu-id="74875-320">jQuery Weryfikuj 1,9</span><span class="sxs-lookup"><span data-stu-id="74875-320">jQuery Validate 1.9</span></span>](jquery-validate/cdnjqueryvalidate19.md "jquery.validate wersja 1.9")
- [<span data-ttu-id="74875-321">jQuery Weryfikuj 1.8.1</span><span class="sxs-lookup"><span data-stu-id="74875-321">jQuery Validate 1.8.1</span></span>](jquery-validate/cdnjqueryvalidate181.md "jquery.validate wersja 1.8.1")
- [<span data-ttu-id="74875-322">jQuery Weryfikuj 1,8</span><span class="sxs-lookup"><span data-stu-id="74875-322">jQuery Validate 1.8</span></span>](jquery-validate/cdnjqueryvalidate18.md "jquery.validate wersja 1.8")
- [<span data-ttu-id="74875-323">jQuery Weryfikuj 1,7</span><span class="sxs-lookup"><span data-stu-id="74875-323">jQuery Validate 1.7</span></span>](jquery-validate/cdnjqueryvalidate17.md "jquery.validate wersja 1.7")
- [<span data-ttu-id="74875-324">jQuery Validate 1.6</span><span class="sxs-lookup"><span data-stu-id="74875-324">jQuery Validate 1.6</span></span>](jquery-validate/cdnjqueryvalidate16.md "jQuery Validate 1.6")
- [<span data-ttu-id="74875-325">jQuery Validate 1.5.5</span><span class="sxs-lookup"><span data-stu-id="74875-325">jQuery Validate 1.5.5</span></span>](jquery-validate/cdnjqueryvalidate155.md "jQuery Validate 1.5.5")

<a id="jQuery_Mobile_Releases_on_the_CDN_4"></a>

### <a name="jquery-mobile-releases-on-the-cdn"></a><span data-ttu-id="74875-326">Wersje programu jQuery Mobile w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="74875-326">jQuery Mobile Releases on the CDN</span></span>

<span data-ttu-id="74875-327">W tej sieci CDN są hostowane następujące wersje biblioteki programu jQuery Mobile.</span><span class="sxs-lookup"><span data-stu-id="74875-327">The following releases of the jQuery Mobile library are hosted on this CDN.</span></span> <span data-ttu-id="74875-328">Kliknij każdy link, aby zobaczyć rzeczywistą listę plików.</span><span class="sxs-lookup"><span data-stu-id="74875-328">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="74875-329">jQuery Mobile 1.4.5</span><span class="sxs-lookup"><span data-stu-id="74875-329">jQuery Mobile 1.4.5</span></span>](jquery-mobile/cdnjquerymobile145.md "System jQuery Mobile 1.4.5 w usłudze Microsoft AJAX CDN")
- [<span data-ttu-id="74875-330">jQuery Mobile 1.4.2</span><span class="sxs-lookup"><span data-stu-id="74875-330">jQuery Mobile 1.4.2</span></span>](jquery-mobile/cdnjquerymobile142.md "System jQuery Mobile 1.4.2 w usłudze Microsoft AJAX CDN")
- [<span data-ttu-id="74875-331">jQuery Mobile 1.4.1</span><span class="sxs-lookup"><span data-stu-id="74875-331">jQuery Mobile 1.4.1</span></span>](jquery-mobile/cdnjquerymobile141.md "System jQuery Mobile 1.4.1 w usłudze Microsoft AJAX CDN")
- [<span data-ttu-id="74875-332">jQuery Mobile 1.4.0</span><span class="sxs-lookup"><span data-stu-id="74875-332">jQuery Mobile 1.4.0</span></span>](jquery-mobile/cdnjquerymobile140.md "System jQuery Mobile 1.4.0 w usłudze Microsoft AJAX CDN")
- [<span data-ttu-id="74875-333">jQuery Mobile 1.3.2</span><span class="sxs-lookup"><span data-stu-id="74875-333">jQuery Mobile 1.3.2</span></span>](jquery-mobile/cdnjquerymobile132.md "System jQuery Mobile 1.3.2 w usłudze Microsoft AJAX CDN")
- [<span data-ttu-id="74875-334">jQuery Mobile 1.3.1</span><span class="sxs-lookup"><span data-stu-id="74875-334">jQuery Mobile 1.3.1</span></span>](jquery-mobile/cdnjquerymobile131.md "System jQuery Mobile 1.3.1 w usłudze Microsoft AJAX CDN")
- [<span data-ttu-id="74875-335">jQuery Mobile 1.3.0</span><span class="sxs-lookup"><span data-stu-id="74875-335">jQuery Mobile 1.3.0</span></span>](jquery-mobile/cdnjquerymobile130.md "System jQuery Mobile 1.3.0 w usłudze Microsoft AJAX CDN")
- [<span data-ttu-id="74875-336">jQuery Mobile 1.2.0</span><span class="sxs-lookup"><span data-stu-id="74875-336">jQuery Mobile 1.2.0</span></span>](jquery-mobile/cdnjquerymobile120.md "System jQuery Mobile 1.2.0 w usłudze Microsoft AJAX CDN")
- [<span data-ttu-id="74875-337">jQuery Mobile 1.1.2</span><span class="sxs-lookup"><span data-stu-id="74875-337">jQuery Mobile 1.1.2</span></span>](jquery-mobile/cdnjquerymobile112.md "System jQuery Mobile 1.1.2 w usłudze Microsoft AJAX CDN")
- [<span data-ttu-id="74875-338">jQuery Mobile 1.1.1</span><span class="sxs-lookup"><span data-stu-id="74875-338">jQuery Mobile 1.1.1</span></span>](jquery-mobile/cdnjquerymobile111.md "System jQuery Mobile 1.1.1 w usłudze Microsoft AJAX CDN")
- [<span data-ttu-id="74875-339">jQuery Mobile 1.1.0</span><span class="sxs-lookup"><span data-stu-id="74875-339">jQuery Mobile 1.1.0</span></span>](jquery-mobile/cdnjquerymobile110.md "System jQuery Mobile 1.1.0 w usłudze Microsoft AJAX CDN")
- [<span data-ttu-id="74875-340">jQuery Mobile 1.1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="74875-340">jQuery Mobile 1.1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile110rc2.md "System jQuery Mobile 1.1.0 RC2 w usłudze Microsoft AJAX CDN")
- [<span data-ttu-id="74875-341">jQuery Mobile 1.0.1</span><span class="sxs-lookup"><span data-stu-id="74875-341">jQuery Mobile 1.0.1</span></span>](jquery-mobile/cdnjquerymobile101.md "System jQuery Mobile 1.0.1 w usłudze Microsoft AJAX CDN")
- [<span data-ttu-id="74875-342">jQuery Mobile 1,0</span><span class="sxs-lookup"><span data-stu-id="74875-342">jQuery Mobile 1.0</span></span>](jquery-mobile/cdnjquerymobile10.md "System jQuery Mobile 1.0 w usłudze Microsoft AJAX CDN")
- [<span data-ttu-id="74875-343">jQuery Mobile 1,0 RC 2</span><span class="sxs-lookup"><span data-stu-id="74875-343">jQuery Mobile 1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile10rc2.md "System jQuery Mobile 1.0 RC2 w usłudze Microsoft AJAX CDN")
- [<span data-ttu-id="74875-344">jQuery Mobile 1,0 RC 1</span><span class="sxs-lookup"><span data-stu-id="74875-344">jQuery Mobile 1.0 RC 1</span></span>](jquery-mobile/cdnjquerymobile10rc1.md "System jQuery Mobile 1.0 RC1 w usłudze Microsoft AJAX CDN")
- [<span data-ttu-id="74875-345">jQuery Mobile 1,0 beta 3</span><span class="sxs-lookup"><span data-stu-id="74875-345">jQuery Mobile 1.0 beta 3</span></span>](jquery-mobile/cdnjquerymobile10b3.md "System jQuery Mobile 1.0 Beta 3 w usłudze Microsoft AJAX CDN")

<a id="jQuery_Templates_Releases_on_the_CDN_5"></a>

### <a name="jquery-templates-releases-on-the-cdn"></a><span data-ttu-id="74875-346">Szablony jQuery są wydaniami w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="74875-346">jQuery Templates Releases on the CDN</span></span>

<span data-ttu-id="74875-347">Następujące wersje wtyczki szablonów jQuery są hostowane w tej sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="74875-347">The following releases of the jQuery Templates plugin are hosted on this CDN.</span></span> <span data-ttu-id="74875-348">Kliknij każdy link, aby zobaczyć rzeczywistą listę plików.</span><span class="sxs-lookup"><span data-stu-id="74875-348">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="74875-349">jQuery Templates Beta 1</span><span class="sxs-lookup"><span data-stu-id="74875-349">jQuery Templates Beta 1</span></span>](jquery-templates/cdnjquerytemplatesbeta1.md "jQuery Templates Beta 1")

<a id="jQuery_Cycle_Releases_on_the_CDN_6"></a>

### <a name="jquery-cycle-releases-on-the-cdn"></a><span data-ttu-id="74875-350">Wersje usługi jQuery cykle w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="74875-350">jQuery Cycle Releases on the CDN</span></span>

<span data-ttu-id="74875-351">Następujące wersje wtyczki cyklu jQuery są hostowane w tej sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="74875-351">The following releases of the jQuery Cycle plugin are hosted on this CDN.</span></span> <span data-ttu-id="74875-352">Kliknij każdy link, aby zobaczyć rzeczywistą listę plików.</span><span class="sxs-lookup"><span data-stu-id="74875-352">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="74875-353">jQuery Cycle 2.99</span><span class="sxs-lookup"><span data-stu-id="74875-353">jQuery Cycle 2.99</span></span>](jquery-cycle/cdnjquerycycle299.md "jQuery Cycle 2.99")
- [<span data-ttu-id="74875-354">jQuery Cycle 2.94</span><span class="sxs-lookup"><span data-stu-id="74875-354">jQuery Cycle 2.94</span></span>](jquery-cycle/cdnjquerycycle294.md "jQuery Cycle 2.94")
- [<span data-ttu-id="74875-355">jQuery Cycle 2.88</span><span class="sxs-lookup"><span data-stu-id="74875-355">jQuery Cycle 2.88</span></span>](jquery-cycle/cdnjquerycycle288.md "jQuery Cycle 2.88")

<a id="jQuery_DataTables_Releases_on_the_CDN_7"></a>

### <a name="jquery-datatables-releases-on-the-cdn"></a><span data-ttu-id="74875-356">Wersje jQuery DataTables w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="74875-356">jQuery DataTables Releases on the CDN</span></span>

<span data-ttu-id="74875-357">Następujące wersje wtyczki DataTables jQuery są hostowane w tej sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="74875-357">The following releases of the jQuery DataTables plugin are hosted on this CDN.</span></span> <span data-ttu-id="74875-358">Kliknij każdy link, aby zobaczyć rzeczywistą listę plików.</span><span class="sxs-lookup"><span data-stu-id="74875-358">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="74875-359">jQuery DataTables 1.10.5</span><span class="sxs-lookup"><span data-stu-id="74875-359">jQuery DataTables 1.10.5</span></span>](jquery-datatables/cdnjquerydatatables105.md "jQuery DataTables 1.10.5")
- [<span data-ttu-id="74875-360">jQuery DataTables 1.10.4</span><span class="sxs-lookup"><span data-stu-id="74875-360">jQuery DataTables 1.10.4</span></span>](jquery-datatables/cdnjquerydatatables104.md "jQuery DataTables 1.10.4")
- [<span data-ttu-id="74875-361">jQuery DataTables 1.9.4</span><span class="sxs-lookup"><span data-stu-id="74875-361">jQuery DataTables 1.9.4</span></span>](jquery-datatables/cdnjquerydatatables194.md "jQuery DataTables 1.9.4")
- [<span data-ttu-id="74875-362">jQuery DataTables 1.9.3</span><span class="sxs-lookup"><span data-stu-id="74875-362">jQuery DataTables 1.9.3</span></span>](jquery-datatables/cdnjquerydatatables193.md "jQuery DataTables 1.9.3")
- [<span data-ttu-id="74875-363">jQuery DataTables 1.9.2</span><span class="sxs-lookup"><span data-stu-id="74875-363">jQuery DataTables 1.9.2</span></span>](jquery-datatables/cdnjquerydatatables192.md "jQuery DataTables 1.9.2")
- [<span data-ttu-id="74875-364">jQuery DataTables 1.9.1</span><span class="sxs-lookup"><span data-stu-id="74875-364">jQuery DataTables 1.9.1</span></span>](jquery-datatables/cdnjquerydatatables191.md "jQuery DataTables 1.9.1")
- [<span data-ttu-id="74875-365">jQuery DataTables 1.9.0</span><span class="sxs-lookup"><span data-stu-id="74875-365">jQuery DataTables 1.9.0</span></span>](jquery-datatables/cdnjquerydatatables190.md "jQuery DataTables 1.9.0")
- [<span data-ttu-id="74875-366">jQuery DataTables 1.8.2</span><span class="sxs-lookup"><span data-stu-id="74875-366">jQuery DataTables 1.8.2</span></span>](jquery-datatables/cdnjquerydatatables182.md "jQuery DataTables 1.8.2")

<a id="Modernizr_Releases_on_the_CDN_8"></a>

### <a name="modernizr-releases-on-the-cdn"></a><span data-ttu-id="74875-367">Wersje programu modernizacji w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="74875-367">Modernizr Releases on the CDN</span></span>

<span data-ttu-id="74875-368">Następujące wersje programu [modernizacji](http://www.modernizr.com "Modernizacja") są hostowane w sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="74875-368">The following releases of [Modernizr](http://www.modernizr.com "Modernizr") are hosted on the CDN:</span></span>

- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-3.5.0.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.8.3.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.2.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.1.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.6.2.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-1.7-development-only.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.0.6-development-only.js

<a id="JSHint_Releases_on_the_CDN_10"></a>

### <a name="jshint-releases-on-the-cdn"></a><span data-ttu-id="74875-369">JSHint wersje w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="74875-369">JSHint Releases on the CDN</span></span>

<span data-ttu-id="74875-370">Następujące wersje programu [JSHint](http://www.jshint.com "JSHint") są hostowane w sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="74875-370">The following releases of [JSHint](http://www.jshint.com "JSHint") are hosted on the CDN:</span></span>

- https://ajax.aspnetcdn.com/ajax/jshint/r07/jshint.js

<a id="Knockout_Releases_on_the_CDN_11"></a>

### <a name="knockout-releases-on-the-cdn"></a><span data-ttu-id="74875-371">Wycinanie odcięć w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="74875-371">Knockout Releases on the CDN</span></span>

<span data-ttu-id="74875-372">Następujące wersje [odcinania](http://www.knockoutjs.com "Separowanie") są hostowane w sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="74875-372">The following releases of [Knockout](http://www.knockoutjs.com "Knockout") are hosted on the CDN:</span></span>

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

### <a name="globalize-releases-on-the-cdn"></a><span data-ttu-id="74875-373">Globalizacja wydań w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="74875-373">Globalize Releases on the CDN</span></span>

<span data-ttu-id="74875-374">Następujące wersje [globalizacji](https://github.com/jquery/globalize "Globalizacja") są hostowane w sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="74875-374">The following releases of [Globalize](https://github.com/jquery/globalize "Globalize") are hosted on the CDN:</span></span>

#### <a name="globalize-version-100"></a><span data-ttu-id="74875-375">Globalizacja wersji 1.0.0</span><span class="sxs-lookup"><span data-stu-id="74875-375">Globalize version 1.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/node-main.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/currency.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/date.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/message.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/number.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/plural.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/relative-time.js

#### <a name="globalize-version-011"></a><span data-ttu-id="74875-376">Globalizacja wersji 0.1.1</span><span class="sxs-lookup"><span data-stu-id="74875-376">Globalize version 0.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.min.js
- https://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.js
- https://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.cultures.js

    - <span data-ttu-id="74875-377">wszystkie kultury</span><span class="sxs-lookup"><span data-stu-id="74875-377">all cultures</span></span>
- https://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.culture.{culture-code}.js

    - <span data-ttu-id="74875-378">Zamień "{Culture-Code}" na żądany kod kultury, np. globalizacja. Culture. en-GB. js = = Microsoft Files w sieci CDN = = te biblioteki zostały przekazane przez firmę Microsoft.</span><span class="sxs-lookup"><span data-stu-id="74875-378">Replace "{culture-code}" with the desired culture code, e.g. globalize.culture.en-GB.js== Microsoft Files on the CDN ==These libraries were uploaded by Microsoft.</span></span>

<a id="Respond_Releases_on_the_CDN_13"></a>

### <a name="respond-releases-on-the-cdn"></a><span data-ttu-id="74875-379">Odpowiedz na wydania w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="74875-379">Respond Releases on the CDN</span></span>

<span data-ttu-id="74875-380">W sieci CDN są hostowane następujące wersje [odpowiedzi](https://github.com/scottjehl/Respond "Odpowiedz") :</span><span class="sxs-lookup"><span data-stu-id="74875-380">The following releases of [Respond](https://github.com/scottjehl/Respond "Respond") are hosted on the CDN:</span></span>

#### <a name="respond-version-142"></a><span data-ttu-id="74875-381">Odpowiedz w wersji 1.4.2</span><span class="sxs-lookup"><span data-stu-id="74875-381">Respond version 1.4.2</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.min.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.min.js

#### <a name="respond-version-141"></a><span data-ttu-id="74875-382">Odpowiedź w wersji 1.4.1</span><span class="sxs-lookup"><span data-stu-id="74875-382">Respond version 1.4.1</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.min.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.min.js

#### <a name="respond-version-140"></a><span data-ttu-id="74875-383">Odpowiedz 1.4.0 wersji</span><span class="sxs-lookup"><span data-stu-id="74875-383">Respond version 1.4.0</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.min.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.min.js

#### <a name="respond-version-130"></a><span data-ttu-id="74875-384">Odpowiedz 1.3.0 wersji</span><span class="sxs-lookup"><span data-stu-id="74875-384">Respond version 1.3.0</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.3.0/respond.js

#### <a name="respond-version-120"></a><span data-ttu-id="74875-385">Odpowiedz 1.2.0 wersji</span><span class="sxs-lookup"><span data-stu-id="74875-385">Respond version 1.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.2.0/respond.js

<a id="Bootstrap_Releases_on_the_CDN_14"></a>

### <a name="bootstrap-releases-on-the-cdn"></a><span data-ttu-id="74875-386">Wydania ładowania początkowego w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="74875-386">Bootstrap Releases on the CDN</span></span>

<span data-ttu-id="74875-387">Następujące wersje programu [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") Bootstrap są hostowane w sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="74875-387">The following releases of [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap are hosted on the CDN:</span></span>

#### <a name="bootstrap-version-441"></a><span data-ttu-id="74875-388">Wersja 4.4.1 programu Bootstrap</span><span class="sxs-lookup"><span data-stu-id="74875-388">Bootstrap version 4.4.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/4.4.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.4.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.4.1/bootstrap.bundle.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.4.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.4.1/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.4.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.4.1/css/bootstrap-grid.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.4.1/css/bootstrap-grid.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.4.1/css/bootstrap-grid.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.4.1/css/bootstrap-reboot.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.4.1/css/bootstrap-reboot.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.4.1/css/bootstrap-reboot.css.map

#### <a name="bootstrap-version-431"></a><span data-ttu-id="74875-389">Wersja załadowania</span><span class="sxs-lookup"><span data-stu-id="74875-389">Bootstrap version 4.3.1</span></span>

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

#### <a name="bootstrap-version-421"></a><span data-ttu-id="74875-390">Wersja zabootstrap 4.2.1</span><span class="sxs-lookup"><span data-stu-id="74875-390">Bootstrap version 4.2.1</span></span>

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

#### <a name="bootstrap-version-411"></a><span data-ttu-id="74875-391">Wersja załadowania systemu 4.1.1</span><span class="sxs-lookup"><span data-stu-id="74875-391">Bootstrap version 4.1.1</span></span>

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

#### <a name="bootstrap-version-400"></a><span data-ttu-id="74875-392">Wersja ładowania początkowego 4.0.0</span><span class="sxs-lookup"><span data-stu-id="74875-392">Bootstrap version 4.0.0</span></span>

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

#### <a name="bootstrap-version-341"></a><span data-ttu-id="74875-393">Wersja narzędzia Bootstrap</span><span class="sxs-lookup"><span data-stu-id="74875-393">Bootstrap version 3.4.1</span></span>

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

#### <a name="bootstrap-version-340"></a><span data-ttu-id="74875-394">Wersja ładowania początkowego 3.4.0</span><span class="sxs-lookup"><span data-stu-id="74875-394">Bootstrap version 3.4.0</span></span>

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

#### <a name="bootstrap-version-337"></a><span data-ttu-id="74875-395">Wersja ładowania początkowego 3.3.7</span><span class="sxs-lookup"><span data-stu-id="74875-395">Bootstrap version 3.3.7</span></span>

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

#### <a name="bootstrap-version-336"></a><span data-ttu-id="74875-396">Wersja ładowania początkowego 3.3.6</span><span class="sxs-lookup"><span data-stu-id="74875-396">Bootstrap version 3.3.6</span></span>

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

#### <a name="bootstrap-version-335"></a><span data-ttu-id="74875-397">Wersja ładowania początkowego 3.3.5</span><span class="sxs-lookup"><span data-stu-id="74875-397">Bootstrap version 3.3.5</span></span>

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

#### <a name="bootstrap-version-334"></a><span data-ttu-id="74875-398">Wersja ładowania początkowego 3.3.4</span><span class="sxs-lookup"><span data-stu-id="74875-398">Bootstrap version 3.3.4</span></span>

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

#### <a name="bootstrap-version-332"></a><span data-ttu-id="74875-399">Wersja załadowania początkowego 3.3.2</span><span class="sxs-lookup"><span data-stu-id="74875-399">Bootstrap version 3.3.2</span></span>

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

#### <a name="bootstrap-version-331"></a><span data-ttu-id="74875-400">Wersja 3.3.1 programu Bootstrap</span><span class="sxs-lookup"><span data-stu-id="74875-400">Bootstrap version 3.3.1</span></span>

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

#### <a name="bootstrap-version-330"></a><span data-ttu-id="74875-401">Wersja ładowania początkowego 3.3.0</span><span class="sxs-lookup"><span data-stu-id="74875-401">Bootstrap version 3.3.0</span></span>

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

#### <a name="bootstrap-version-320"></a><span data-ttu-id="74875-402">Wersja ładowania początkowego 3.2.0</span><span class="sxs-lookup"><span data-stu-id="74875-402">Bootstrap version 3.2.0</span></span>

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

#### <a name="bootstrap-version-311"></a><span data-ttu-id="74875-403">Wersja załadowania początkowego 3.1.1</span><span class="sxs-lookup"><span data-stu-id="74875-403">Bootstrap version 3.1.1</span></span>

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

#### <a name="bootstrap-version-310"></a><span data-ttu-id="74875-404">Wersja ładowania początkowego 3.1.0</span><span class="sxs-lookup"><span data-stu-id="74875-404">Bootstrap version 3.1.0</span></span>

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

#### <a name="bootstrap-version-303"></a><span data-ttu-id="74875-405">Wersja ładowania początkowego 3.0.3</span><span class="sxs-lookup"><span data-stu-id="74875-405">Bootstrap version 3.0.3</span></span>

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

#### <a name="bootstrap-version-302"></a><span data-ttu-id="74875-406">Wersja ładowania początkowego 3.0.2</span><span class="sxs-lookup"><span data-stu-id="74875-406">Bootstrap version 3.0.2</span></span>

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

#### <a name="bootstrap-version-301"></a><span data-ttu-id="74875-407">Wersja ładowania początkowego 3.0.1</span><span class="sxs-lookup"><span data-stu-id="74875-407">Bootstrap version 3.0.1</span></span>

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

#### <a name="bootstrap-version-300"></a><span data-ttu-id="74875-408">Wersja ładowania początkowego 3.0.0</span><span class="sxs-lookup"><span data-stu-id="74875-408">Bootstrap version 3.0.0</span></span>

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

#### <a name="bootstrap-version-232"></a><span data-ttu-id="74875-409">Wersja Bootstrap 2.3.2</span><span class="sxs-lookup"><span data-stu-id="74875-409">Bootstrap version 2.3.2</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings.png
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings-white.png

#### <a name="bootstrap-version-231"></a><span data-ttu-id="74875-410">Wersja 2.3.1 programu Bootstrap</span><span class="sxs-lookup"><span data-stu-id="74875-410">Bootstrap version 2.3.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings.png
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings-white.png

<a id="BootstrapTouchCarousel_Releases_on_the_CDN_18"></a>

### <a name="bootstrap-touchcarousel-releases-on-the-cdn"></a><span data-ttu-id="74875-411">Wydania TouchCarousel Bootstrap w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="74875-411">Bootstrap TouchCarousel Releases on the CDN</span></span>

<span data-ttu-id="74875-412">Następujące wersje [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") ładowania początkowego TouchCarousel są hostowane w sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="74875-412">The following releases of [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") Bootstrap TouchCarousel releases are hosted on the CDN:</span></span>

#### <a name="bootstrap-touchcarousel-version-080"></a><span data-ttu-id="74875-413">TouchCarousel Bootstrap w wersji 0.8.0</span><span class="sxs-lookup"><span data-stu-id="74875-413">Bootstrap TouchCarousel version 0.8.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/css/bootstrap-touch-carousel.css
- https://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/js/bootstrap-touch-carousel.js

<a id="Hammerjs_Releases_on_the_CDN_19"></a>

### <a name="hammerjs-releases-on-the-cdn"></a><span data-ttu-id="74875-414">Wersje młota. js w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="74875-414">Hammer.js Releases on the CDN</span></span>

<span data-ttu-id="74875-415">Następujące wersje usługi [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") młot. js są hostowane w sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="74875-415">The following releases of [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js releases are hosted on the CDN:</span></span>

#### <a name="hammerjs-version-204"></a><span data-ttu-id="74875-416">2\.0.4 w wersji młota. js</span><span class="sxs-lookup"><span data-stu-id="74875-416">Hammer.js version 2.0.4</span></span>

- https://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.js
- https://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.js
- https://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.map

<a id="ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15"></a>

### <a name="aspnet-web-forms-and-ajax-releases-on-the-cdn"></a><span data-ttu-id="74875-417">ASP.NET Web Forms i wersje AJAX w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="74875-417">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>

<span data-ttu-id="74875-418">Następujące wersje biblioteki AJAX ASP.NET są hostowane w sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="74875-418">The following releases of the ASP.NET Ajax Library are hosted on the CDN.</span></span> <span data-ttu-id="74875-419">Kliknij każdy link, aby zobaczyć rzeczywistą listę plików.</span><span class="sxs-lookup"><span data-stu-id="74875-419">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="74875-420">ASP.NET Web Forms i AJAX w wersji 4.5.2</span><span class="sxs-lookup"><span data-stu-id="74875-420">ASP.NET Web Forms and Ajax version 4.5.2</span></span>](cdnajax452.md "ASP.NET Web Forms i AJAX 4.5.2")
- [<span data-ttu-id="74875-421">ASP.NET Web Forms i AJAX w wersji 4</span><span class="sxs-lookup"><span data-stu-id="74875-421">ASP.NET Web Forms and Ajax version 4</span></span>](cdnajax4.md "ASP.NET Web Forms i AJAX 4")
- [<span data-ttu-id="74875-422">ASP.NET AJAX w wersji 3,5</span><span class="sxs-lookup"><span data-stu-id="74875-422">ASP.NET Ajax version 3.5</span></span>](cdnajax35.md "ASP.NET AJAX 3.5")

<a id="ASPNET_MVC_Releases_on_the_CDN_16"></a>

### <a name="aspnet-mvc-releases-on-the-cdn"></a><span data-ttu-id="74875-423">Wersje ASP.NET MVC w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="74875-423">ASP.NET MVC Releases on the CDN</span></span>

<span data-ttu-id="74875-424">Następujące pliki JavaScript ASP.NET MVC są hostowane w tej sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="74875-424">The following ASP.NET MVC JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-mvc-523"></a><span data-ttu-id="74875-425">ASP.NET MVC 5.2.3</span><span class="sxs-lookup"><span data-stu-id="74875-425">ASP.NET MVC 5.2.3</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-51"></a><span data-ttu-id="74875-426">ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="74875-426">ASP.NET MVC 5.1</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-50"></a><span data-ttu-id="74875-427">ASP.NET MVC 5.0</span><span class="sxs-lookup"><span data-stu-id="74875-427">ASP.NET MVC 5.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-40"></a><span data-ttu-id="74875-428">ASP.NET MVC 4.0</span><span class="sxs-lookup"><span data-stu-id="74875-428">ASP.NET MVC 4.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-30"></a><span data-ttu-id="74875-429">ASP.NET MVC 3.0</span><span class="sxs-lookup"><span data-stu-id="74875-429">ASP.NET MVC 3.0</span></span>

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

#### <a name="aspnet-mvc-20"></a><span data-ttu-id="74875-430">ASP.NET MVC 2.0</span><span class="sxs-lookup"><span data-stu-id="74875-430">ASP.NET MVC 2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.js
- https://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.debug.js

#### <a name="aspnet-mvc-10"></a><span data-ttu-id="74875-431">ASP.NET MVC 1.0</span><span class="sxs-lookup"><span data-stu-id="74875-431">ASP.NET MVC 1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.js
- https://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.debug.js

<a id="ASPNET_SignalR_Releases_on_the_CDN_17"></a>

### <a name="aspnet-signalr-releases-on-the-cdn"></a><span data-ttu-id="74875-432">Wydania sygnałów ASP.NET w sieci CDN</span><span class="sxs-lookup"><span data-stu-id="74875-432">ASP.NET SignalR Releases on the CDN</span></span>

<span data-ttu-id="74875-433">Następujące pliki JavaScript ASP.NET sygnalizujący są hostowane w tej sieci CDN:</span><span class="sxs-lookup"><span data-stu-id="74875-433">The following ASP.NET SignalR JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-signalr-222"></a><span data-ttu-id="74875-434">ASP.NET SignalR 2.2.2</span><span class="sxs-lookup"><span data-stu-id="74875-434">ASP.NET SignalR 2.2.2</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.js

#### <a name="aspnet-signalr-221"></a><span data-ttu-id="74875-435">ASP.NET SignalR 2.2.1</span><span class="sxs-lookup"><span data-stu-id="74875-435">ASP.NET SignalR 2.2.1</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.js

#### <a name="aspnet-signalr-220"></a><span data-ttu-id="74875-436">ASP.NET SignalR 2.2.0</span><span class="sxs-lookup"><span data-stu-id="74875-436">ASP.NET SignalR 2.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.js

#### <a name="aspnet-signalr-210"></a><span data-ttu-id="74875-437">ASP.NET SignalR 2.1.0</span><span class="sxs-lookup"><span data-stu-id="74875-437">ASP.NET SignalR 2.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.js

#### <a name="aspnet-signalr-203"></a><span data-ttu-id="74875-438">ASP.NET SignalR 2.0.3</span><span class="sxs-lookup"><span data-stu-id="74875-438">ASP.NET SignalR 2.0.3</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.js

#### <a name="aspnet-signalr-202"></a><span data-ttu-id="74875-439">ASP.NET SignalR 2.0.2</span><span class="sxs-lookup"><span data-stu-id="74875-439">ASP.NET SignalR 2.0.2</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.js

#### <a name="aspnet-signalr-201"></a><span data-ttu-id="74875-440">ASP.NET SignalR 2.0.1</span><span class="sxs-lookup"><span data-stu-id="74875-440">ASP.NET SignalR 2.0.1</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.js

#### <a name="aspnet-signalr-200"></a><span data-ttu-id="74875-441">ASP.NET SignalR 2.0.0</span><span class="sxs-lookup"><span data-stu-id="74875-441">ASP.NET SignalR 2.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.js

#### <a name="aspnet-signalr-113"></a><span data-ttu-id="74875-442">ASP.NET SignalR 1.1.3</span><span class="sxs-lookup"><span data-stu-id="74875-442">ASP.NET SignalR 1.1.3</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.js

#### <a name="aspnet-signalr-112"></a><span data-ttu-id="74875-443">ASP.NET SignalR 1.1.2</span><span class="sxs-lookup"><span data-stu-id="74875-443">ASP.NET SignalR 1.1.2</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.js

#### <a name="aspnet-signalr-111"></a><span data-ttu-id="74875-444">ASP.NET SignalR 1.1.1</span><span class="sxs-lookup"><span data-stu-id="74875-444">ASP.NET SignalR 1.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.js

#### <a name="aspnet-signalr-110"></a><span data-ttu-id="74875-445">ASP.NET SignalR 1.1.0</span><span class="sxs-lookup"><span data-stu-id="74875-445">ASP.NET SignalR 1.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.js

#### <a name="aspnet-signalr-101"></a><span data-ttu-id="74875-446">ASP.NET SignalR 1.0.1</span><span class="sxs-lookup"><span data-stu-id="74875-446">ASP.NET SignalR 1.0.1</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.js

<span data-ttu-id="74875-447">Aby uzyskać informacje o warunkach użytkowania usługi CDN, zobacz [warunki użytkowania usługi Microsoft Ajax CDN](https://www.asp.net/terms-of-use "Warunki użytkowania usługi Microsoft Ajax CDN").</span><span class="sxs-lookup"><span data-stu-id="74875-447">For information about the terms of use for the CDN, see [Microsoft Ajax CDN Terms of Use](https://www.asp.net/terms-of-use "Microsoft Ajax CDN Terms of Use").</span></span>

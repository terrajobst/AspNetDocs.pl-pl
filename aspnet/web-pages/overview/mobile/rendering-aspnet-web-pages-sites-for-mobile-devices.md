---
uid: web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
title: Renderowanie witryn ASP.NET Web Pages (Razor) dla urządzeń przenośnych | Microsoft Docs
author: Rick-Anderson
description: W tym artykule opisano sposób tworzenia stron w witrynie ASP.NET Web Pages (Razor), która będzie odpowiednio renderowana na urządzeniach przenośnych. Dowiesz się, jak to zrobić...
ms.author: riande
ms.date: 02/17/2014
ms.assetid: f15ab392-c05e-4269-83bf-7c6d2b8c8ec8
msc.legacyurl: /web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
msc.type: authoredcontent
ms.openlocfilehash: c012348d65e48a275cb0e4808fef2a7f31e5fb33
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78563568"
---
# <a name="rendering-aspnet-web-pages-razor-sites-for-mobile-devices"></a><span data-ttu-id="2b6c1-104">Renderowanie witryn ASP.NET Web Pages (Razor) dla urządzeń przenośnych</span><span class="sxs-lookup"><span data-stu-id="2b6c1-104">Rendering ASP.NET Web Pages (Razor) Sites for Mobile Devices</span></span>

<span data-ttu-id="2b6c1-105">Autor [FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="2b6c1-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="2b6c1-106">W tym artykule opisano sposób tworzenia stron w witrynie ASP.NET Web Pages (Razor), która będzie odpowiednio renderowana na urządzeniach przenośnych.</span><span class="sxs-lookup"><span data-stu-id="2b6c1-106">This article describes how to create pages in an ASP.NET Web Pages (Razor) site that will render appropriately on mobile devices.</span></span>
> 
> <span data-ttu-id="2b6c1-107">Zawartość:</span><span class="sxs-lookup"><span data-stu-id="2b6c1-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="2b6c1-108">Jak używać konwencji nazewnictwa, aby określić, że strona jest zaprojektowana specjalnie dla urządzeń przenośnych.</span><span class="sxs-lookup"><span data-stu-id="2b6c1-108">How to use a naming convention to specify that a page is designed specifically for mobile devices.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="2b6c1-109">Wersje oprogramowania używane w samouczku</span><span class="sxs-lookup"><span data-stu-id="2b6c1-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="2b6c1-110">ASP.NET strony sieci Web (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="2b6c1-110">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="2b6c1-111">Ten samouczek działa również z ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="2b6c1-111">This tutorial also works with ASP.NET Web Pages 2.</span></span>

<span data-ttu-id="2b6c1-112">Strony sieci Web ASP.NET umożliwiają tworzenie niestandardowych ekranów do renderowania zawartości na urządzeniach przenośnych i innych.</span><span class="sxs-lookup"><span data-stu-id="2b6c1-112">ASP.NET Web Pages lets you create custom displays for rendering content on mobile or other devices.</span></span>

<span data-ttu-id="2b6c1-113">Najprostszym sposobem utworzenia strony specyficznej dla urządzenia w witrynie ASP.NET Web Pages jest użycie wzorca nazewnictwa plików, takiego jak: *filename. Mobile. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="2b6c1-113">The simplest way to create device-specific page in an ASP.NET Web Pages site is by using a file-naming pattern like this: *FileName.Mobile.cshtml*.</span></span> <span data-ttu-id="2b6c1-114">Można utworzyć dwie wersje strony (na przykład jeden nazwany *plik. cshtml* i jeden nazwany *plik. Mobile. cshtml*).</span><span class="sxs-lookup"><span data-stu-id="2b6c1-114">You can create two versions of a page (for example, one named *MyFile.cshtml* and one named *MyFile.Mobile.cshtml*).</span></span> <span data-ttu-id="2b6c1-115">W czasie wykonywania, gdy urządzenie przenośne zażąda *pliku. cshtml*, ASP.NET renderuje zawartość z *pliku. Mobile. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="2b6c1-115">At run time, when a mobile device requests *MyFile.cshtml*, ASP.NET renders the content from *MyFile.Mobile.cshtml*.</span></span> <span data-ttu-id="2b6c1-116">W przeciwnym razie *plik. cshtml* jest renderowany.</span><span class="sxs-lookup"><span data-stu-id="2b6c1-116">Otherwise, *MyFile.cshtml* is rendered.</span></span>

<span data-ttu-id="2b6c1-117">Poniższy przykład pokazuje, jak włączyć renderowanie mobilne przez dodanie strony zawartości dla urządzeń przenośnych.</span><span class="sxs-lookup"><span data-stu-id="2b6c1-117">The following example shows how to enable mobile rendering by adding a content page for mobile devices.</span></span> <span data-ttu-id="2b6c1-118">*Strona1. cshtml* zawiera zawartość i pasek boczny nawigacji.</span><span class="sxs-lookup"><span data-stu-id="2b6c1-118">*Page1.cshtml* contains content plus a navigation sidebar.</span></span> <span data-ttu-id="2b6c1-119">*Strona1. Mobile. cshtml* zawiera tę samą zawartość, ale pomija pasek boczny.</span><span class="sxs-lookup"><span data-stu-id="2b6c1-119">*Page1.Mobile.cshtml* contains the same content, but omits the sidebar.</span></span>

1. <span data-ttu-id="2b6c1-120">W witrynie ASP.NET Web Pages Utwórz plik o nazwie *Strona1. cshtml* i Zastąp bieżącą zawartość następującym znacznikiem.</span><span class="sxs-lookup"><span data-stu-id="2b6c1-120">In an ASP.NET Web Pages site, create a file named *Page1.cshtml* and replace the current content with following markup.</span></span>

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample1.html)]
2. <span data-ttu-id="2b6c1-121">Utwórz plik o nazwie *Strona1. Mobile. cshtml* i Zastąp istniejącą zawartość następującym znacznikiem.</span><span class="sxs-lookup"><span data-stu-id="2b6c1-121">Create a file named *Page1.Mobile.cshtml* and replace the existing content with the following markup.</span></span> <span data-ttu-id="2b6c1-122">Zwróć uwagę, że wersja aplikacji mobilnej pomija sekcję nawigacji dla lepszego renderowania na mniejszym ekranie.</span><span class="sxs-lookup"><span data-stu-id="2b6c1-122">Notice that the mobile version of the page omits the navigation section for better rendering on a smaller screen.</span></span>

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample2.html)]
3. <span data-ttu-id="2b6c1-123">Uruchom przeglądarkę pulpitu i przejdź do elementu *Strona1. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="2b6c1-123">Run a desktop browser and browse to *Page1.cshtml*.</span></span> <span data-ttu-id="2b6c1-124">![mobilesites-1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="2b6c1-124">![mobilesites-1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)</span></span>
4. <span data-ttu-id="2b6c1-125">Uruchom przeglądarkę mobilną (lub emulator urządzenia przenośnego) i przejdź do pliku *Strona1. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="2b6c1-125">Run a mobile browser (or a mobile device emulator) and browse to *Page1.cshtml*.</span></span> <span data-ttu-id="2b6c1-126">(Należy zauważyć, że nie zawieramy *. Mobile.*</span><span class="sxs-lookup"><span data-stu-id="2b6c1-126">(Notice that you do not include *.mobile.*</span></span> <span data-ttu-id="2b6c1-127">w ramach adresu URL). Mimo że żądanie ma wartość *Strona1. cshtml*, ASP.NET renderuje *Strona1. Mobile. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="2b6c1-127">as part of the URL.) Even though the request is to *Page1.cshtml*, ASP.NET renders *Page1.Mobile.cshtml*.</span></span>

    ![mobilesites-2](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="2b6c1-129">Aby przetestować strony mobilne, można użyć symulatora urządzenia przenośnego, który działa na komputerze stacjonarnym.</span><span class="sxs-lookup"><span data-stu-id="2b6c1-129">To test mobile pages, you can use a mobile device simulator that runs on a desktop computer.</span></span> <span data-ttu-id="2b6c1-130">To narzędzie umożliwia testowanie stron sieci Web w taki sposób, w jaki będą wyglądały na urządzeniach przenośnych (zazwyczaj z znacznie mniejszym obszarem wyświetlania).</span><span class="sxs-lookup"><span data-stu-id="2b6c1-130">This tool lets you test web pages as they would look on mobile devices (that is, typically with a much smaller display area).</span></span> <span data-ttu-id="2b6c1-131">Jednym z przykładów symulatora jest [dodatek agent użytkownika](http://addons.mozilla.org/firefox/addon/user-agent-switcher/) dla przeglądarki Mozilla Firefox, który pozwala emulować różne przeglądarki dla urządzeń przenośnych z wersji klasycznej programu Firefox.</span><span class="sxs-lookup"><span data-stu-id="2b6c1-131">One example of a simulator is the [User Agent Switcher add-on](http://addons.mozilla.org/firefox/addon/user-agent-switcher/) for Mozilla Firefox, which lets you emulate various mobile browsers from a desktop version of Firefox.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="2b6c1-132">Dodatkowe materiały</span><span class="sxs-lookup"><span data-stu-id="2b6c1-132">Additional Resources</span></span>

<span data-ttu-id="2b6c1-133">[Emulator Windows Phone](https://msdn.microsoft.com/library/ff402563(v=VS.92).aspx)</span><span class="sxs-lookup"><span data-stu-id="2b6c1-133">[Windows Phone Emulator](https://msdn.microsoft.com/library/ff402563(v=VS.92).aspx)</span></span>

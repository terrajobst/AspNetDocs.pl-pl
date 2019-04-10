---
uid: web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
title: Renderowania ASP.NET Web Pages witryny (Razor) dla urządzeń przenośnych | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: 'W tym artykule opisano sposób tworzenia stron w witrynie ASP.NET Web Pages (Razor), która będzie renderowana odpowiednio na urządzeniach przenośnych. Zawartość: Jak można...'
ms.author: riande
ms.date: 02/17/2014
ms.assetid: f15ab392-c05e-4269-83bf-7c6d2b8c8ec8
msc.legacyurl: /web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
msc.type: authoredcontent
ms.openlocfilehash: dbcd25331387f8606343e551302bc3ed1f9b2c25
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/09/2019
ms.locfileid: "59379511"
---
# <a name="rendering-aspnet-web-pages-razor-sites-for-mobile-devices"></a><span data-ttu-id="500d8-104">Renderowanie witrynach ASP.NET Web Pages (Razor) dla urządzeń przenośnych</span><span class="sxs-lookup"><span data-stu-id="500d8-104">Rendering ASP.NET Web Pages (Razor) Sites for Mobile Devices</span></span>

<span data-ttu-id="500d8-105">przez [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="500d8-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="500d8-106">W tym artykule opisano sposób tworzenia stron w witrynie ASP.NET Web Pages (Razor), która będzie renderowana odpowiednio na urządzeniach przenośnych.</span><span class="sxs-lookup"><span data-stu-id="500d8-106">This article describes how to create pages in an ASP.NET Web Pages (Razor) site that will render appropriately on mobile devices.</span></span>
> 
> <span data-ttu-id="500d8-107">Zawartość:</span><span class="sxs-lookup"><span data-stu-id="500d8-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="500d8-108">Jak używać konwencji nazewnictwa, aby określić, że strona jest zaprojektowany specjalnie dla urządzeń przenośnych.</span><span class="sxs-lookup"><span data-stu-id="500d8-108">How to use a naming convention to specify that a page is designed specifically for mobile devices.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="500d8-109">Wersje oprogramowania używanego w tym samouczku</span><span class="sxs-lookup"><span data-stu-id="500d8-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="500d8-110">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="500d8-110">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="500d8-111">W tym samouczku współpracuje również z wzorca ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="500d8-111">This tutorial also works with ASP.NET Web Pages 2.</span></span>


<span data-ttu-id="500d8-112">Strony ASP.NET Web Pages umożliwia tworzenie niestandardowych służy do renderowania zawartości na telefon komórkowy lub innych urządzeń.</span><span class="sxs-lookup"><span data-stu-id="500d8-112">ASP.NET Web Pages lets you create custom displays for rendering content on mobile or other devices.</span></span>

<span data-ttu-id="500d8-113">Najprostszym sposobem utworzenia strony specyficznych dla urządzenia w witrynie ASP.NET Web Pages jest przy użyciu wzorca nazewnictwa plików następująco: *FileName.Mobile.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="500d8-113">The simplest way to create device-specific page in an ASP.NET Web Pages site is by using a file-naming pattern like this: *FileName.Mobile.cshtml*.</span></span> <span data-ttu-id="500d8-114">Możesz utworzyć dwie wersje strony (na przykład jeden o nazwie *MyFile.cshtml* i jedną o nazwie *MyFile.Mobile.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="500d8-114">You can create two versions of a page (for example, one named *MyFile.cshtml* and one named *MyFile.Mobile.cshtml*).</span></span> <span data-ttu-id="500d8-115">W czasie, gdy urządzenie przenośne żąda wykonywania *MyFile.cshtml*, platforma ASP.NET renderuje zawartość z *MyFile.Mobile.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="500d8-115">At run time, when a mobile device requests *MyFile.cshtml*, ASP.NET renders the content from *MyFile.Mobile.cshtml*.</span></span> <span data-ttu-id="500d8-116">W przeciwnym razie *MyFile.cshtml* jest renderowany.</span><span class="sxs-lookup"><span data-stu-id="500d8-116">Otherwise, *MyFile.cshtml* is rendered.</span></span>

<span data-ttu-id="500d8-117">Poniższy przykład pokazuje, jak umożliwiające renderowanie mobilnych, dodając strony zawartości dla urządzeń przenośnych.</span><span class="sxs-lookup"><span data-stu-id="500d8-117">The following example shows how to enable mobile rendering by adding a content page for mobile devices.</span></span> <span data-ttu-id="500d8-118">*Page1.cshtml* zawiera zawartość oraz pasek boczny nawigacji.</span><span class="sxs-lookup"><span data-stu-id="500d8-118">*Page1.cshtml* contains content plus a navigation sidebar.</span></span> <span data-ttu-id="500d8-119">*Page1.Mobile.cshtml* zawiera tę samą zawartość, ale pomija pasku bocznym.</span><span class="sxs-lookup"><span data-stu-id="500d8-119">*Page1.Mobile.cshtml* contains the same content, but omits the sidebar.</span></span>

1. <span data-ttu-id="500d8-120">W witrynie ASP.NET Web Pages, Utwórz plik o nazwie *Page1.cshtml* i Zastąp zawartość bieżącego następujące znaczniki.</span><span class="sxs-lookup"><span data-stu-id="500d8-120">In an ASP.NET Web Pages site, create a file named *Page1.cshtml* and replace the current content with following markup.</span></span>

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample1.html)]
2. <span data-ttu-id="500d8-121">Utwórz plik o nazwie *Page1.Mobile.cshtml* i zastąpić istniejącą zawartość następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="500d8-121">Create a file named *Page1.Mobile.cshtml* and replace the existing content with the following markup.</span></span> <span data-ttu-id="500d8-122">Należy zauważyć, że mobilnej wersji strony pomija lepsze renderowanie na mniejsze ekranu można znaleźć w sekcji nawigacji.</span><span class="sxs-lookup"><span data-stu-id="500d8-122">Notice that the mobile version of the page omits the navigation section for better rendering on a smaller screen.</span></span>

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample2.html)]
3. <span data-ttu-id="500d8-123">Uruchom przeglądarkę dla komputerów, a następnie przejdź do *Page1.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="500d8-123">Run a desktop browser and browse to *Page1.cshtml*.</span></span> ![mobilesites-1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)
4. <span data-ttu-id="500d8-125">Uruchom w przeglądarce dla urządzeń przenośnych (lub w emulatorze urządzenia przenośnego), a następnie przejdź do *Page1.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="500d8-125">Run a mobile browser (or a mobile device emulator) and browse to *Page1.cshtml*.</span></span> <span data-ttu-id="500d8-126">(Zwróć uwagę, że nie zostanie uwzględniony *.mobile.*</span><span class="sxs-lookup"><span data-stu-id="500d8-126">(Notice that you do not include *.mobile.*</span></span> <span data-ttu-id="500d8-127">jako część adresu URL). Nawet jeśli żądanie dotyczy *Page1.cshtml*, platforma ASP.NET renderuje *Page1.Mobile.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="500d8-127">as part of the URL.) Even though the request is to *Page1.cshtml*, ASP.NET renders *Page1.Mobile.cshtml*.</span></span>

    ![mobilesites-2](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="500d8-129">Aby przetestować stron dla urządzeń przenośnych, można użyć symulatora urządzenia przenośnego, uruchomionym na komputerze stacjonarnym.</span><span class="sxs-lookup"><span data-stu-id="500d8-129">To test mobile pages, you can use a mobile device simulator that runs on a desktop computer.</span></span> <span data-ttu-id="500d8-130">To narzędzie umożliwia testowanie stron sieci web tak, jak będą wyglądały na urządzeniach przenośnych (oznacza to, zwykle za pomocą znacznie mniejszy powoduje wyświetlenie obszaru).</span><span class="sxs-lookup"><span data-stu-id="500d8-130">This tool lets you test web pages as they would look on mobile devices (that is, typically with a much smaller display area).</span></span> <span data-ttu-id="500d8-131">Przykład symulatora [przełącznik agenta użytkownika dodatku](http://addons.mozilla.org/firefox/addon/user-agent-switcher/) dla Mozilla Firefox, który umożliwia emulowanie różnych przeglądarek dla urządzeń przenośnych z wersji klasycznej programu Firefox.</span><span class="sxs-lookup"><span data-stu-id="500d8-131">One example of a simulator is the [User Agent Switcher add-on](http://addons.mozilla.org/firefox/addon/user-agent-switcher/) for Mozilla Firefox, which lets you emulate various mobile browsers from a desktop version of Firefox.</span></span>


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="500d8-132">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="500d8-132">Additional Resources</span></span>


[<span data-ttu-id="500d8-133">Emulator Windows Phone</span><span class="sxs-lookup"><span data-stu-id="500d8-133">Windows Phone Emulator</span></span>](https://msdn.microsoft.com/library/ff402563(v=VS.92).aspx)

---
uid: web-pages/overview/ui-layouts-and-themes/installing-helpers
title: Instalowanie pomocnika w witrynie ASP.NET Web Pages (Razor) | Microsoft Docs
author: Rick-Anderson
description: W tym artykule opisano sposób instalowania pomocnika w witrynie internetowej ASP.NET Web Pages (Razor). Pomocnik to składnik wielokrotnego użytku, który zawiera kod i adiustację do...
ms.author: riande
ms.date: 02/18/2014
ms.assetid: 5e968ead-906a-45ea-ac2a-c70e57e1a9b1
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/installing-helpers
msc.type: authoredcontent
ms.openlocfilehash: 41e33c04a53a6ad257c3937cdadcec767e9217c8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78638587"
---
# <a name="installing-a-helper-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="2a1e3-104">Instalowanie pomocnika w witrynie ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="2a1e3-104">Installing a Helper in an ASP.NET Web Pages (Razor) Site</span></span>

<span data-ttu-id="2a1e3-105">Autor [FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="2a1e3-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="2a1e3-106">W tym artykule opisano sposób instalowania pomocnika w witrynie internetowej ASP.NET Web Pages (Razor).</span><span class="sxs-lookup"><span data-stu-id="2a1e3-106">This article describes how to install a helper in an ASP.NET Web Pages (Razor) website.</span></span> <span data-ttu-id="2a1e3-107">*Pomocnik* to składnik wielokrotnego użytku, który zawiera kod i znaczniki umożliwiające wykonanie zadania, które może być żmudnym lub złożone.</span><span class="sxs-lookup"><span data-stu-id="2a1e3-107">A *helper* is a reusable component that includes code and markup to perform a task that might be tedious or complex.</span></span>
> 
> <span data-ttu-id="2a1e3-108">Zawartość:</span><span class="sxs-lookup"><span data-stu-id="2a1e3-108">What you'll learn:</span></span>
> 
> - <span data-ttu-id="2a1e3-109">Jak zainstalować pomocnika w witrynie sieci Web utworzonej przy użyciu programu WebMatrix 3.</span><span class="sxs-lookup"><span data-stu-id="2a1e3-109">How to install a helper in a website created using WebMatrix 3.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="2a1e3-110">Wersje oprogramowania używane w samouczku</span><span class="sxs-lookup"><span data-stu-id="2a1e3-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="2a1e3-111">WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="2a1e3-111">WebMatrix 3</span></span>

## <a name="overview-of-helpers"></a><span data-ttu-id="2a1e3-112">Przegląd pomocników</span><span class="sxs-lookup"><span data-stu-id="2a1e3-112">Overview of Helpers</span></span>

<span data-ttu-id="2a1e3-113">Niektóre zadania, które często mają być wykonywane na stronach sieci Web, wymagają dużej ilości kodu lub wymagają dodatkowej wiedzy.</span><span class="sxs-lookup"><span data-stu-id="2a1e3-113">Some tasks that people often want to do on web pages require a lot of code or require extra knowledge.</span></span> <span data-ttu-id="2a1e3-114">Przykłady obejmują Wyświetlanie wykresu dla danych; Umieszczanie przycisku "Obserwuj" w usłudze Twitter na stronie; Wysyłanie wiadomości e-mail z witryny sieci Web; Przycinanie lub zmienianie rozmiarów obrazów; Korzystanie z systemu PayPal dla witryny.</span><span class="sxs-lookup"><span data-stu-id="2a1e3-114">Examples include displaying a chart for data; putting a Twitter "Follow" button on a page; sending email from your website; cropping or resizing images; using PayPal for your site.</span></span> <span data-ttu-id="2a1e3-115">Aby ułatwić Ci wykonywanie tego rodzaju rzeczy, ASP.NET strony sieci Web umożliwiają korzystanie z *pomocników*.</span><span class="sxs-lookup"><span data-stu-id="2a1e3-115">To make it easy to do these kinds of things, ASP.NET Web Pages lets you use *helpers*.</span></span> <span data-ttu-id="2a1e3-116">Pomocnicy są składnikami instalowanymi dla lokacji i, które umożliwiają wykonywanie typowych zadań przy użyciu tylko linii lub dwóch kodów Razor.</span><span class="sxs-lookup"><span data-stu-id="2a1e3-116">Helpers are components that you install for a site and that let you perform typical tasks by using just a line or two of Razor code.</span></span>

<span data-ttu-id="2a1e3-117">Strony sieci Web ASP.NET zawierają kilka pomocników wbudowanych w.</span><span class="sxs-lookup"><span data-stu-id="2a1e3-117">ASP.NET Web Pages has a few helpers built in.</span></span> <span data-ttu-id="2a1e3-118">Jednak wiele pomocników jest dostępnych w pakietach (dodatków) udostępnianych za pomocą Menedżera pakietów NuGet.</span><span class="sxs-lookup"><span data-stu-id="2a1e3-118">However, many helpers are available in packages (add-ins) that are provided using the NuGet package manager.</span></span> <span data-ttu-id="2a1e3-119">Pakiet NuGet umożliwia wybranie pakietu do zainstalowania, a następnie uwzględnia wszystkie szczegóły instalacji.</span><span class="sxs-lookup"><span data-stu-id="2a1e3-119">NuGet lets you select a package to install and then it takes care of all the details of the installation.</span></span>

## <a name="installing-a-helper-in-webmatrix-3"></a><span data-ttu-id="2a1e3-120">Instalowanie pomocnika w programie WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="2a1e3-120">Installing a Helper in WebMatrix 3</span></span>

1. <span data-ttu-id="2a1e3-121">W programie WebMatrix 3 Kliknij przycisk **NuGet** .</span><span class="sxs-lookup"><span data-stu-id="2a1e3-121">In WebMatrix 3, click the **NuGet** button.</span></span>

    ![Okno dialogowe Galeria NuGet w programie WebMatrix](installing-helpers/_static/image1.png)
2. <span data-ttu-id="2a1e3-123">Spowoduje to uruchomienie Menedżera pakietów NuGet i wyświetlenie dostępnych pakietów.</span><span class="sxs-lookup"><span data-stu-id="2a1e3-123">This launches the NuGet package manager and displays available packages.</span></span> <span data-ttu-id="2a1e3-124">W polu wyszukiwania wprowadź słowo kluczowe pomocnika, który chcesz zainstalować.</span><span class="sxs-lookup"><span data-stu-id="2a1e3-124">In the search box, enter a keyword for the helper you want to install.</span></span>

    ![Okno dialogowe Galeria NuGet w programie WebMatrix](installing-helpers/_static/image2.png)
3. <span data-ttu-id="2a1e3-126">Wybierz pakiet, a następnie kliknij przycisk **Instaluj**.</span><span class="sxs-lookup"><span data-stu-id="2a1e3-126">Select the package and then click **Install**.</span></span> <span data-ttu-id="2a1e3-127">Kliknij przycisk **tak** po wyświetleniu monitu, jeśli chcesz zainstalować pakiet i wskazać, że akceptujesz warunki.</span><span class="sxs-lookup"><span data-stu-id="2a1e3-127">Click **Yes** when asked if you want to install the package and indicate that you accept the terms.</span></span>

     <span data-ttu-id="2a1e3-128">Jeśli po raz pierwszy zainstalowano pomocnika, program NuGet utworzy foldery w witrynie sieci Web dla kodu, który tworzy pomocnika.</span><span class="sxs-lookup"><span data-stu-id="2a1e3-128">If this is the first time you've installed a helper, NuGet creates folders in your website for the code that makes up the helper.</span></span>
4. <span data-ttu-id="2a1e3-129">Aby odinstalować pomocnika, kliknij przycisk **Galeria** , kliknij kartę **zainstalowane** i wybierz pakiet, który chcesz odinstalować.</span><span class="sxs-lookup"><span data-stu-id="2a1e3-129">To uninstall a helper, click the **Gallery** button, click the **Installed** tab, and pick the package you want to uninstall.</span></span>

## <a name="installing-the-twitter-helper"></a><span data-ttu-id="2a1e3-130">Instalowanie pomocnika usługi Twitter</span><span class="sxs-lookup"><span data-stu-id="2a1e3-130">Installing the Twitter helper</span></span>

<span data-ttu-id="2a1e3-131">Najnowsza wersja interfejsu API usługi Twitter nie jest zgodna z pomocnikiem usługi Twitter instalowanym za pomocą narzędzia NuGet.</span><span class="sxs-lookup"><span data-stu-id="2a1e3-131">The latest version of the Twitter API is not compatible with the Twitter helper you install through NuGet.</span></span> <span data-ttu-id="2a1e3-132">Zamiast tego zapoznaj się z tematem [pomocnik usługi Twitter z WebMatrix](twitter-helper.md) , aby uzyskać informacje na temat sposobu konfigurowania pomocnika usługi Twitter w projekcie.</span><span class="sxs-lookup"><span data-stu-id="2a1e3-132">Instead, see the [Twitter Helper with WebMatrix](twitter-helper.md) topic for information about how to set up the Twitter helper in your project.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="2a1e3-133">Dodatkowe materiały</span><span class="sxs-lookup"><span data-stu-id="2a1e3-133">Additional Resources</span></span>

[<span data-ttu-id="2a1e3-134">Wprowadzenie do stron sieci Web ASP.NET 2 — Podstawy programowania</span><span class="sxs-lookup"><span data-stu-id="2a1e3-134">Introducing ASP.NET Web Pages 2 - Programming Basics</span></span>](../getting-started/introducing-razor-syntax-c.md)

[<span data-ttu-id="2a1e3-135">Pomocnik usługi Twitter w programie WebMatrix</span><span class="sxs-lookup"><span data-stu-id="2a1e3-135">Twitter Helper with WebMatrix</span></span>](twitter-helper.md)

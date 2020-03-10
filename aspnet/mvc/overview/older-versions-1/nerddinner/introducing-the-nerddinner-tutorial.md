---
uid: mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
title: Wprowadzenie do samouczka NerdDinner | Microsoft Docs
author: shanselman
description: Najlepszym sposobem uczenia się nowego środowiska jest skompilowanie czegoś. Ten samouczek przeprowadzi Cię przez proces tworzenia małego, ale kompletnej aplikacji przy użyciu usługi ASP.NE...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 397522d5-0402-4b94-b810-a2fb564f869d
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
msc.type: authoredcontent
ms.openlocfilehash: 154cfe6694cf723c0a1f8e33bfdb42c97594518f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78580578"
---
# <a name="introducing-the-nerddinner-tutorial"></a><span data-ttu-id="a1a91-104">Wprowadzenie do samouczka NerdDinner</span><span class="sxs-lookup"><span data-stu-id="a1a91-104">Introducing the NerdDinner Tutorial</span></span>

<span data-ttu-id="a1a91-105">przez [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="a1a91-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

[<span data-ttu-id="a1a91-106">Pobierz plik PDF</span><span class="sxs-lookup"><span data-stu-id="a1a91-106">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="a1a91-107">Najlepszym sposobem uczenia się nowego środowiska jest skompilowanie czegoś.</span><span class="sxs-lookup"><span data-stu-id="a1a91-107">The best way to learn a new framework is to build something with it.</span></span> <span data-ttu-id="a1a91-108">W tym samouczku przedstawiono sposób tworzenia małego, ale kompletnej aplikacji używającej ASP.NET MVC 1 i wprowadzono niektóre podstawowe pojęcia.</span><span class="sxs-lookup"><span data-stu-id="a1a91-108">This tutorial walks through how to build a small, but complete, application using ASP.NET MVC 1, and introduces some of the core concepts behind it.</span></span>
> 
> <span data-ttu-id="a1a91-109">Jeśli używasz ASP.NET MVC 3, zalecamy użycie [wprowadzenie ze samouczkami ze sklepu MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) lub [MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) .</span><span class="sxs-lookup"><span data-stu-id="a1a91-109">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>

## <a name="nerddinner-tutorial"></a><span data-ttu-id="a1a91-110">Samouczek NerdDinner</span><span class="sxs-lookup"><span data-stu-id="a1a91-110">NerdDinner Tutorial</span></span>

<span data-ttu-id="a1a91-111">Najlepszym sposobem uczenia się nowego środowiska jest skompilowanie czegoś.</span><span class="sxs-lookup"><span data-stu-id="a1a91-111">The best way to learn a new framework is to build something with it.</span></span> <span data-ttu-id="a1a91-112">W tym samouczku przedstawiono sposób tworzenia małego, ale kompletnej aplikacji używającej ASP.NET MVC i wprowadzono niektóre podstawowe pojęcia.</span><span class="sxs-lookup"><span data-stu-id="a1a91-112">This tutorial walks through how to build a small, but complete, application using ASP.NET MVC, and introduces some of the core concepts behind it.</span></span>

<span data-ttu-id="a1a91-113">Aplikacja, którą zamierzamy skompilować, nosi nazwę "NerdDinner".</span><span class="sxs-lookup"><span data-stu-id="a1a91-113">The application we are going to build is called "NerdDinner".</span></span> <span data-ttu-id="a1a91-114">NerdDinner zapewnia łatwy sposób, aby użytkownicy mogli znajdować i organizować obiady w trybie online:</span><span class="sxs-lookup"><span data-stu-id="a1a91-114">NerdDinner provides an easy way for people to find and organize dinners online:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image1.png)

<span data-ttu-id="a1a91-115">NerdDinner umożliwia zarejestrowanym użytkownikom tworzenie, edytowanie i usuwanie obiadów.</span><span class="sxs-lookup"><span data-stu-id="a1a91-115">NerdDinner enables registered users to create, edit and delete dinners.</span></span> <span data-ttu-id="a1a91-116">Wymusza spójny zestaw weryfikacji i reguł firmy w aplikacji:</span><span class="sxs-lookup"><span data-stu-id="a1a91-116">It enforces a consistent set of validation and business rules across the application:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image2.png)

<span data-ttu-id="a1a91-117">Osoby odwiedzające mogą używać mapy opartej na technologii AJAX, aby wyszukiwać nadchodzące uroczyste obiady w ich sąsiedztwie:</span><span class="sxs-lookup"><span data-stu-id="a1a91-117">Visitors can use an AJAX-based map to search for upcoming dinners being held near them:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image3.png)

<span data-ttu-id="a1a91-118">Kliknięcie obiadu spowoduje przejście do strony szczegółów, na której można dowiedzieć się więcej na jej temat:</span><span class="sxs-lookup"><span data-stu-id="a1a91-118">Clicking a dinner will take them to a details page where they can learn more about it:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image4.png)

<span data-ttu-id="a1a91-119">Jeśli interesują Cię obiad, mogą oni zalogować się lub zarejestrować w witrynie:</span><span class="sxs-lookup"><span data-stu-id="a1a91-119">If they are interested in attending the dinner they can login or register on the site:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image5.png)

<span data-ttu-id="a1a91-120">Następnie mogą kliknąć link RSVP oparty na technologii AJAX, aby wziąć udział w zdarzeniu:</span><span class="sxs-lookup"><span data-stu-id="a1a91-120">They can then click an AJAX-based RSVP link to attend the event:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image6.png)

![](introducing-the-nerddinner-tutorial/_static/image7.png)

### <a name="implementing-nerddinner"></a><span data-ttu-id="a1a91-121">Implementowanie NerdDinner</span><span class="sxs-lookup"><span data-stu-id="a1a91-121">Implementing NerdDinner</span></span>

<span data-ttu-id="a1a91-122">Zamierzamy zacząć korzystać z naszej aplikacji NerdDinner przy użyciu polecenia plik&gt;nowy projekt w programie Visual Studio, aby utworzyć nowy projekt ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="a1a91-122">We are going to begin our NerdDinner application by using the File-&gt;New Project command within Visual Studio to create a brand new ASP.NET MVC project.</span></span> <span data-ttu-id="a1a91-123">Następnie będziemy stopniowo dodawać funkcje i funkcje.</span><span class="sxs-lookup"><span data-stu-id="a1a91-123">We will then incrementally add functionality and features.</span></span> <span data-ttu-id="a1a91-124">W ten sposób będziemy:</span><span class="sxs-lookup"><span data-stu-id="a1a91-124">Along the way we'll cover:</span></span>

1. [<span data-ttu-id="a1a91-125">Jak utworzyć nowy projekt ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="a1a91-125">How to create a new ASP.NET MVC Project</span></span>](create-a-new-aspnet-mvc-project.md)
2. [<span data-ttu-id="a1a91-126">Jak utworzyć bazę danych</span><span class="sxs-lookup"><span data-stu-id="a1a91-126">How to create a database</span></span>](create-a-database.md)
3. [<span data-ttu-id="a1a91-127">Jak skompilować model przy użyciu walidacji reguł firmy</span><span class="sxs-lookup"><span data-stu-id="a1a91-127">How to build a model with business rule validations</span></span>](build-a-model-with-business-rule-validations.md)
4. [<span data-ttu-id="a1a91-128">Jak używać kontrolerów i widoków do implementowania interfejsu użytkownika listy/szczegółów</span><span class="sxs-lookup"><span data-stu-id="a1a91-128">How to use controllers and views to implement a listing/details UI</span></span>](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
5. [<span data-ttu-id="a1a91-129">Jak zapewnić obsługę wpisów w formularzu danych CRUD (tworzenie, odczytywanie, aktualizowanie, usuwanie)</span><span class="sxs-lookup"><span data-stu-id="a1a91-129">How to provide CRUD (create, read, update, delete) data form entry support</span></span>](provide-crud-create-read-update-delete-data-form-entry-support.md)
6. [<span data-ttu-id="a1a91-130">Jak używać ViewData i implementować klasy ViewModel</span><span class="sxs-lookup"><span data-stu-id="a1a91-130">How to use ViewData and implement ViewModel classes</span></span>](use-viewdata-and-implement-viewmodel-classes.md)
7. [<span data-ttu-id="a1a91-131">Jak ponowne używać interfejsu użytkownika za pomocą stron wzorcowych i częściowych</span><span class="sxs-lookup"><span data-stu-id="a1a91-131">How to re-use UI using master pages and partials</span></span>](re-use-ui-using-master-pages-and-partials.md)
8. [<span data-ttu-id="a1a91-132">Jak zaimplementować wydajne stronicowanie danych</span><span class="sxs-lookup"><span data-stu-id="a1a91-132">How to implement efficient data paging</span></span>](implement-efficient-data-paging.md)
9. [<span data-ttu-id="a1a91-133">Jak zabezpieczyć aplikacje przy użyciu uwierzytelniania i autoryzacji</span><span class="sxs-lookup"><span data-stu-id="a1a91-133">How to secure applications using authentication and authorization</span></span>](secure-applications-using-authentication-and-authorization.md)
10. [<span data-ttu-id="a1a91-134">Jak używać technologii AJAX do dostarczania aktualizacji dynamicznych</span><span class="sxs-lookup"><span data-stu-id="a1a91-134">How to use AJAX to deliver dynamic updates</span></span>](use-ajax-to-deliver-dynamic-updates.md)
11. [<span data-ttu-id="a1a91-135">Jak zaimplementować scenariusze mapowania przy użyciu technologii AJAX</span><span class="sxs-lookup"><span data-stu-id="a1a91-135">How to use AJAX to implement mapping scenarios</span></span>](use-ajax-to-implement-mapping-scenarios.md)
12. [<span data-ttu-id="a1a91-136">Jak włączyć automatyczne testowanie jednostek</span><span class="sxs-lookup"><span data-stu-id="a1a91-136">How to enable automated unit testing</span></span>](enable-automated-unit-testing.md)

<span data-ttu-id="a1a91-137">Możesz utworzyć własną kopię NerdDinner od podstaw, wypełniając każdy krok w tym rozdziale.</span><span class="sxs-lookup"><span data-stu-id="a1a91-137">You can build your own copy of NerdDinner from scratch by completing each step we walkthrough in this chapter.</span></span> <span data-ttu-id="a1a91-138">Możesz też pobrać kompletną wersję kodu źródłowego tutaj: [NerdDinner w witrynie GitHub](https://github.com/AspNetMVPSamples/NerdDinner).</span><span class="sxs-lookup"><span data-stu-id="a1a91-138">Alternatively, you can download a completed version of the source code here: [NerdDinner on GitHub](https://github.com/AspNetMVPSamples/NerdDinner).</span></span> <span data-ttu-id="a1a91-139">Opcjonalnie możesz również [pobrać bezpłatną wersję pliku PDF z tego samouczka](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf) , jeśli chcesz przeczytać samouczek w trybie offline.</span><span class="sxs-lookup"><span data-stu-id="a1a91-139">You can also optionally also [download a free PDF version of this tutorial](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf) if you want to read the tutorial offline.</span></span>

<span data-ttu-id="a1a91-140">Do skompilowania aplikacji możesz użyć programu Visual Studio 2008 lub bezpłatnej wersji Visual Web Developer 2008 Express.</span><span class="sxs-lookup"><span data-stu-id="a1a91-140">You can use either Visual Studio 2008 or the free Visual Web Developer 2008 Express to build the application.</span></span> <span data-ttu-id="a1a91-141">Możesz użyć dowolnej SQL Server lub bezpłatnej SQL Server Express bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a1a91-141">You can use either SQL Server or the free SQL Server Express for the database.</span></span>

<span data-ttu-id="a1a91-142">Możesz zainstalować ASP.NET MVC, Visual Web Developer 2008 Express i SQL Server Express (wszystko bezpłatne) przy użyciu wersji 2 [Instalator platformy Microsoft Web](https://www.microsoft.com/web/downloads/platform.aspx)</span><span class="sxs-lookup"><span data-stu-id="a1a91-142">You can install ASP.NET MVC, Visual Web Developer 2008 Express, and SQL Server Express (all free) using V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)</span></span>

### <a name="now-lets-get-started"></a><span data-ttu-id="a1a91-143">Zacznijmy teraz...</span><span class="sxs-lookup"><span data-stu-id="a1a91-143">Now let's get started....</span></span>

<span data-ttu-id="a1a91-144">Teraz, w jaki sposób NerdDinner się, przyjrzyjmy nasze rękawy i napisać kod.</span><span class="sxs-lookup"><span data-stu-id="a1a91-144">Now that we've covered what NerdDinner is, let's roll up our sleeves and write some code.</span></span>

<span data-ttu-id="a1a91-145">Rozpocznie się tworzenie aplikacji NerdDinner przy użyciu pliku&gt;nowego projektu w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a1a91-145">We'll begin by using File-&gt;New Project within Visual Studio to create the NerdDinner application.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="a1a91-146">Dalej</span><span class="sxs-lookup"><span data-stu-id="a1a91-146">Next</span></span>](create-a-new-aspnet-mvc-project.md)

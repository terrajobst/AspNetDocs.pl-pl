---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-vb
title: Tworzenie kontrolera (VB) | Microsoft Docs
author: StephenWalther
description: W tym samouczku Stephen Walther pokazuje, jak można dodać kontroler do aplikacji ASP.NET MVC.
ms.author: riande
ms.date: 03/02/2009
ms.assetid: 204b7e86-f560-4611-8adb-785b33e777b9
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-vb
msc.type: authoredcontent
ms.openlocfilehash: 60636b79ab5fc06ca904dee90ce74f256e046d12
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78601459"
---
# <a name="creating-a-controller-vb"></a><span data-ttu-id="d5d0b-103">Tworzenie kontrolera (VB)</span><span class="sxs-lookup"><span data-stu-id="d5d0b-103">Creating a Controller (VB)</span></span>

<span data-ttu-id="d5d0b-104">Autor [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="d5d0b-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="d5d0b-105">W tym samouczku Stephen Walther pokazuje, jak można dodać kontroler do aplikacji ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="d5d0b-105">In this tutorial, Stephen Walther demonstrates how you can add a controller to an ASP.NET MVC application.</span></span>

<span data-ttu-id="d5d0b-106">Celem tego samouczka jest wyjaśnienie, jak można tworzyć nowe kontrolery MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="d5d0b-106">The goal of this tutorial is to explain how you can create new ASP.NET MVC controllers.</span></span> <span data-ttu-id="d5d0b-107">Dowiesz się, jak utworzyć kontrolery przy użyciu opcji menu Dodaj kontroler programu Visual Studio i ręcznie tworząc plik klasy.</span><span class="sxs-lookup"><span data-stu-id="d5d0b-107">You learn how to create controllers both by using the Visual Studio Add Controller menu option and by creating a class file by hand.</span></span>

### <a name="using-the-add-controller-menu-option"></a><span data-ttu-id="d5d0b-108">Korzystanie z opcji menu Dodaj kontroler</span><span class="sxs-lookup"><span data-stu-id="d5d0b-108">Using the Add Controller Menu Option</span></span>

<span data-ttu-id="d5d0b-109">Najprostszym sposobem utworzenia nowego kontrolera jest kliknięcie prawym przyciskiem myszy folderu controllers w oknie Eksplorator rozwiązań programu Visual Studio i wybranie opcji **Dodaj, kontroler** (patrz rysunek 1).</span><span class="sxs-lookup"><span data-stu-id="d5d0b-109">The easiest way to create a new controller is to right-click the Controllers folder in the Visual Studio Solution Explorer window and select the **Add, Controller** menu option (see Figure 1).</span></span> <span data-ttu-id="d5d0b-110">Wybranie tej opcji menu spowoduje otwarcie okna dialogowego **Dodawanie kontrolera** (patrz rysunek 2).</span><span class="sxs-lookup"><span data-stu-id="d5d0b-110">Selecting this menu option opens the **Add Controller** dialog (see Figure 2).</span></span>

<span data-ttu-id="d5d0b-111">[![okno dialogowe Nowy projekt](creating-a-controller-vb/_static/image1.jpg)](creating-a-controller-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="d5d0b-111">[![The New Project dialog box](creating-a-controller-vb/_static/image1.jpg)](creating-a-controller-vb/_static/image1.png)</span></span>

<span data-ttu-id="d5d0b-112">**Ilustracja 01**. Dodawanie nowego kontrolera ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-controller-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="d5d0b-112">**Figure 01**: Adding a new controller([Click to view full-size image](creating-a-controller-vb/_static/image2.png))</span></span>

<span data-ttu-id="d5d0b-113">[![okno dialogowe Nowy projekt](creating-a-controller-vb/_static/image2.jpg)](creating-a-controller-vb/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="d5d0b-113">[![The New Project dialog box](creating-a-controller-vb/_static/image2.jpg)](creating-a-controller-vb/_static/image3.png)</span></span>

<span data-ttu-id="d5d0b-114">**Ilustracja 02**. okno dialogowe Dodawanie kontrolera ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-a-controller-vb/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="d5d0b-114">**Figure 02**: The Add Controller dialog ([Click to view full-size image](creating-a-controller-vb/_static/image4.png))</span></span>

<span data-ttu-id="d5d0b-115">Zwróć uwagę, że pierwsza część nazwy kontrolera jest wyróżniona w oknie dialogowym **Dodaj kontroler** .</span><span class="sxs-lookup"><span data-stu-id="d5d0b-115">Notice that the first part of the controller name is highlighted in the **Add Controller** dialog.</span></span> <span data-ttu-id="d5d0b-116">Każda nazwa kontrolera musi kończyć się *kontrolerem*sufiksu.</span><span class="sxs-lookup"><span data-stu-id="d5d0b-116">Every controller name must end with the suffix *Controller*.</span></span> <span data-ttu-id="d5d0b-117">Na przykład można utworzyć kontroler o nazwie *ProductController* , ale nie kontroler o nazwie *Product*.</span><span class="sxs-lookup"><span data-stu-id="d5d0b-117">For example, you can create a controller named *ProductController* but not a controller named *Product*.</span></span>

<span data-ttu-id="d5d0b-118">Jeśli utworzysz kontroler, na którym brakuje sufiksu *kontrolera* , nie będzie możliwe wywołanie kontrolera.</span><span class="sxs-lookup"><span data-stu-id="d5d0b-118">If you create a controller that is missing the *Controller* suffix then you won't be able to invoke the controller.</span></span> <span data-ttu-id="d5d0b-119">Nie wykonuj tej czynności — po wprowadzeniu tego błędu niezliczone kilka godzin użytkowania.</span><span class="sxs-lookup"><span data-stu-id="d5d0b-119">Don't do this -- I've wasted countless hours of my life after making this mistake.</span></span>

<span data-ttu-id="d5d0b-120">**Lista 1 — Controllers\ProductController.vb**</span><span class="sxs-lookup"><span data-stu-id="d5d0b-120">**Listing 1 - Controllers\ProductController.vb**</span></span>

[!code-vb[Main](creating-a-controller-vb/samples/sample1.vb)]

<span data-ttu-id="d5d0b-121">Należy zawsze tworzyć kontrolery w folderze controllers.</span><span class="sxs-lookup"><span data-stu-id="d5d0b-121">You should always create controllers in the Controllers folder.</span></span> <span data-ttu-id="d5d0b-122">W przeciwnym razie nastąpi naruszenie Konwencji ASP.NET MVC, a inni deweloperzy będą mieli trudniejszy czas na zrozumienie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d5d0b-122">Otherwise, you'll be violating the conventions of ASP.NET MVC and other developers will have a more difficult time understanding your application.</span></span>

### <a name="scaffolding-action-methods"></a><span data-ttu-id="d5d0b-123">Metody akcji tworzenia szkieletu</span><span class="sxs-lookup"><span data-stu-id="d5d0b-123">Scaffolding Action Methods</span></span>

<span data-ttu-id="d5d0b-124">Podczas tworzenia kontrolera można automatycznie generować metody akcji Create, Update i Details (zobacz rysunek 3).</span><span class="sxs-lookup"><span data-stu-id="d5d0b-124">When you create a controller, you have the option to generate Create, Update, and Details action methods automatically (see Figure 3).</span></span> <span data-ttu-id="d5d0b-125">Jeśli wybierzesz tę opcję, zostanie wygenerowana Klasa kontrolera na liście 2.</span><span class="sxs-lookup"><span data-stu-id="d5d0b-125">If you select this option then the controller class in Listing 2 is generated.</span></span>

<span data-ttu-id="d5d0b-126">[![tworzenia metod akcji automatycznie](creating-a-controller-vb/_static/image3.jpg)](creating-a-controller-vb/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="d5d0b-126">[![Creating action methods automatically](creating-a-controller-vb/_static/image3.jpg)](creating-a-controller-vb/_static/image5.png)</span></span>

<span data-ttu-id="d5d0b-127">**Ilustracja 03**: automatyczne tworzenie metod akcji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-controller-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="d5d0b-127">**Figure 03**: Creating action methods automatically ([Click to view full-size image](creating-a-controller-vb/_static/image6.png))</span></span>

<span data-ttu-id="d5d0b-128">**Lista 2 — Controllers\CustomerController.vb**</span><span class="sxs-lookup"><span data-stu-id="d5d0b-128">**Listing 2 - Controllers\CustomerController.vb**</span></span>

[!code-vb[Main](creating-a-controller-vb/samples/sample2.vb)]

<span data-ttu-id="d5d0b-129">Te wygenerowane metody są metodami zastępczymi.</span><span class="sxs-lookup"><span data-stu-id="d5d0b-129">These generated methods are stub methods.</span></span> <span data-ttu-id="d5d0b-130">Należy dodać rzeczywistą logikę do tworzenia, aktualizowania i pokazywania szczegółowych informacji dla klienta.</span><span class="sxs-lookup"><span data-stu-id="d5d0b-130">You must add the actual logic for creating, updating, and showing details for a customer yourself.</span></span> <span data-ttu-id="d5d0b-131">Jednak metody zastępcze zapewniają całkiem punkt wyjścia.</span><span class="sxs-lookup"><span data-stu-id="d5d0b-131">But, the stub methods provide you with a nice starting point.</span></span>

### <a name="creating-a-controller-class"></a><span data-ttu-id="d5d0b-132">Tworzenie klasy kontrolera</span><span class="sxs-lookup"><span data-stu-id="d5d0b-132">Creating a Controller Class</span></span>

<span data-ttu-id="d5d0b-133">Kontroler ASP.NET MVC jest tylko klasą.</span><span class="sxs-lookup"><span data-stu-id="d5d0b-133">The ASP.NET MVC controller is just a class.</span></span> <span data-ttu-id="d5d0b-134">Jeśli wolisz, możesz zignorować wygodne tworzenie szkieletu kontrolera programu Visual Studio i ręcznie utworzyć klasę kontrolera.</span><span class="sxs-lookup"><span data-stu-id="d5d0b-134">If you prefer, you can ignore the convenient Visual Studio controller scaffolding and create a controller class by hand.</span></span> <span data-ttu-id="d5d0b-135">Wykonaj następujące kroki:</span><span class="sxs-lookup"><span data-stu-id="d5d0b-135">Follow these steps:</span></span>

1. <span data-ttu-id="d5d0b-136">Kliknij prawym przyciskiem myszy folder controllers, a następnie wybierz opcję menu **Dodaj, nowy element** i wybierz szablon **klasy** (patrz rysunek 4).</span><span class="sxs-lookup"><span data-stu-id="d5d0b-136">Right-click the Controllers folder and select the menu option **Add, New Item** and select the **Class** template (see Figure 4).</span></span>
2. <span data-ttu-id="d5d0b-137">Nazwij nową klasę PersonController. vb i kliknij przycisk **Dodaj** .</span><span class="sxs-lookup"><span data-stu-id="d5d0b-137">Name the new class PersonController.vb and click the **Add** button.</span></span>
3. <span data-ttu-id="d5d0b-138">Zmodyfikuj plik klasy powstającej w taki sposób, aby Klasa dziedziczy z klasy podstawowa system. Web. MVC. Controller (patrz lista 3).</span><span class="sxs-lookup"><span data-stu-id="d5d0b-138">Modify the resulting class file so that the class inherits from the base System.Web.Mvc.Controller class (see Listing 3).</span></span>

<span data-ttu-id="d5d0b-139">[![tworzenia nowej klasy](creating-a-controller-vb/_static/image4.jpg)](creating-a-controller-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="d5d0b-139">[![Creating a new class](creating-a-controller-vb/_static/image4.jpg)](creating-a-controller-vb/_static/image7.png)</span></span>

<span data-ttu-id="d5d0b-140">**Ilustracja 04**: Tworzenie nowej klasy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-controller-vb/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="d5d0b-140">**Figure 04**: Creating a new class([Click to view full-size image](creating-a-controller-vb/_static/image8.png))</span></span>

<span data-ttu-id="d5d0b-141">**Lista 3 — Controllers\PersonController.vb**</span><span class="sxs-lookup"><span data-stu-id="d5d0b-141">**Listing 3 - Controllers\PersonController.vb**</span></span>

[!code-vb[Main](creating-a-controller-vb/samples/sample3.vb)]

<span data-ttu-id="d5d0b-142">Kontroler z listy 3 uwidacznia jedną akcję o nazwie index (), która zwraca ciąg "Hello world!".</span><span class="sxs-lookup"><span data-stu-id="d5d0b-142">The controller in Listing 3 exposes one action named Index() that returns the string "Hello World!".</span></span> <span data-ttu-id="d5d0b-143">Możesz wywołać tę akcję kontrolera, uruchamiając aplikację i żądając adresu URL, takiego jak:</span><span class="sxs-lookup"><span data-stu-id="d5d0b-143">You can invoke this controller action by running your application and requesting a URL like the following:</span></span>

`http://localhost:40071/Person`

> [!NOTE]
> 
> <span data-ttu-id="d5d0b-144">Serwer ASP.NET Development używa losowego numeru portu (na przykład 40071).</span><span class="sxs-lookup"><span data-stu-id="d5d0b-144">The ASP.NET Development Server uses a random port number (for example, 40071).</span></span> <span data-ttu-id="d5d0b-145">Po wprowadzeniu adresu URL w celu wywołania kontrolera należy podać odpowiedni numer portu.</span><span class="sxs-lookup"><span data-stu-id="d5d0b-145">When entering a URL to invoke a controller, you'll need to supply the right port number.</span></span> <span data-ttu-id="d5d0b-146">Możesz określić numer portu, aktywując wskaźnik myszy nad ikoną serwera deweloperskiego ASP.NET w obszarze powiadomień systemu Windows (w prawym dolnym rogu ekranu).</span><span class="sxs-lookup"><span data-stu-id="d5d0b-146">You can determine the port number by hovering your mouse over the icon for the ASP.NET Development Server in the Windows Notification Area (bottom-right of your screen).</span></span>
> 
> [!div class="step-by-step"]
> <span data-ttu-id="d5d0b-147">[Poprzednie](adding-dynamic-content-to-a-cached-page-vb.md)
> [dalej](creating-an-action-vb.md)</span><span class="sxs-lookup"><span data-stu-id="d5d0b-147">[Previous](adding-dynamic-content-to-a-cached-page-vb.md)
[Next](creating-an-action-vb.md)</span></span>

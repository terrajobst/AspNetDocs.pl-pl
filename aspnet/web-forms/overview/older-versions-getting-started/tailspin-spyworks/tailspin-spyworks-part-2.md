---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
title: 'Część 2: Warstwa dostępu do danych | Microsoft Docs'
author: JoeStagner
description: Ta seria samouczków zawiera szczegółowe informacje na temat wszystkich kroków podjętych w celu skompilowania przykładowej aplikacji Tailspin Spyworks. Część 2 obejmuje dodanie warstwy dostępu do danych.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 5a9d5429-d70b-411c-8474-f42cf7ef8a2b
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
msc.type: authoredcontent
ms.openlocfilehash: 342d2c54dfba5d052570e890f85dcf9739f9884f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78573249"
---
# <a name="part-2-data-access-layer"></a><span data-ttu-id="6ca29-104">Część 2. Warstwa dostępu do danych</span><span class="sxs-lookup"><span data-stu-id="6ca29-104">Part 2: Data Access Layer</span></span>

<span data-ttu-id="6ca29-105">Jan [Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="6ca29-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="6ca29-106">Tailspin Spyworks ilustruje, w jaki sposób bardzo jest prosta, aby tworzyć zaawansowane, skalowalne aplikacje dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="6ca29-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="6ca29-107">W tym artykule przedstawiono sposób korzystania z doskonałych nowych funkcji w programie ASP.NET 4 do tworzenia sklepu online, w tym zakupów, wyewidencjonowywania i administrowania.</span><span class="sxs-lookup"><span data-stu-id="6ca29-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="6ca29-108">Ta seria samouczków zawiera szczegółowe informacje na temat wszystkich kroków podjętych w celu skompilowania przykładowej aplikacji Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="6ca29-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="6ca29-109">Część 2 obejmuje dodanie warstwy dostępu do danych.</span><span class="sxs-lookup"><span data-stu-id="6ca29-109">Part 2 covers adding the data access layer.</span></span>

## <a id="_Toc260221668"></a><span data-ttu-id="6ca29-110">Dodawanie warstwy dostępu do danych</span><span class="sxs-lookup"><span data-stu-id="6ca29-110">Adding the Data Access Layer</span></span>

<span data-ttu-id="6ca29-111">Nasza aplikacja handlu elektronicznego będzie zależeć od dwóch baz danych.</span><span class="sxs-lookup"><span data-stu-id="6ca29-111">Our ecommerce application will depend on two databases.</span></span>

<span data-ttu-id="6ca29-112">Informacje o klientach używają standardowej bazy danych członkostwa ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="6ca29-112">For customer information we'll use the standard ASP.NET Membership database.</span></span> <span data-ttu-id="6ca29-113">W naszym koszyku i katalogu produktów zostanie wdrożona baza danych SQL Express w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="6ca29-113">For our shopping cart and product catalog we'll implement a SQL Express database as follows.</span></span>

![](tailspin-spyworks-part-2/_static/image1.jpg)

<span data-ttu-id="6ca29-114">Po utworzeniu bazy danych (commerce. mdf) w aplikacji aplikacji\_folderze danych możemy utworzyć naszą warstwę dostępu do danych za pomocą programu .NET Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="6ca29-114">Having created the database (Commerce.mdf) in the application's App\_Data folder we can proceed to create our Data Access Layer using the .NET Entity Framework.</span></span>

<span data-ttu-id="6ca29-115">Utworzymy folder o nazwie "dane\_Access" i kliknij prawym przyciskiem myszy ten folder i wybierz pozycję "Dodaj nowy element".</span><span class="sxs-lookup"><span data-stu-id="6ca29-115">We'll create a folder named "Data\_Access" and them right click on that folder and select "Add New Item".</span></span>

<span data-ttu-id="6ca29-116">W elemencie "zainstalowane szablony", a następnie wybierz pozycję "ADO.NET Entity Data Model" wpisz EDM\_commerce. edmx jako nazwę, a następnie kliknij przycisk "Dodaj".</span><span class="sxs-lookup"><span data-stu-id="6ca29-116">In the "Installed Templates" item and then select "ADO.NET Entity Data Model" enter EDM\_Commerce.edmx as the name and click the "Add" button.</span></span>

![](tailspin-spyworks-part-2/_static/image2.jpg)

<span data-ttu-id="6ca29-117">Wybierz pozycję "Generuj z bazy danych".</span><span class="sxs-lookup"><span data-stu-id="6ca29-117">Choose "Generate from Database".</span></span>

![](tailspin-spyworks-part-2/_static/image1.png)

![](tailspin-spyworks-part-2/_static/image2.png)

![](tailspin-spyworks-part-2/_static/image3.png)

![](tailspin-spyworks-part-2/_static/image3.jpg)

<span data-ttu-id="6ca29-118">Zapisz i skompiluj.</span><span class="sxs-lookup"><span data-stu-id="6ca29-118">Save and build.</span></span>

<span data-ttu-id="6ca29-119">Teraz jesteśmy gotowi do dodania naszej pierwszej funkcji — menu kategorii produktów.</span><span class="sxs-lookup"><span data-stu-id="6ca29-119">Now we are ready to add our first feature – a product category menu.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="6ca29-120">[Poprzednie](tailspin-spyworks-part-1.md)
> [dalej](tailspin-spyworks-part-3.md)</span><span class="sxs-lookup"><span data-stu-id="6ca29-120">[Previous](tailspin-spyworks-part-1.md)
[Next](tailspin-spyworks-part-3.md)</span></span>

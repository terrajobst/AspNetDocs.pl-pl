---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
title: 'Część 3: menu Układ i Kategoria | Microsoft Docs'
author: JoeStagner
description: Ta seria samouczków zawiera szczegółowe informacje na temat wszystkich kroków podjętych w celu skompilowania przykładowej aplikacji Tailspin Spyworks. Część 3 dotyczy dodawania układu i menu kategorii.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 94ea1a70-a9bc-4241-8f36-08366d64bab9
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
msc.type: authoredcontent
ms.openlocfilehash: a223b97fd362ecf73ecde431e141021c1dcc6a6d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78639119"
---
# <a name="part-3-layout-and-category-menu"></a><span data-ttu-id="0c4eb-104">Część 3. Układ i menu kategorii</span><span class="sxs-lookup"><span data-stu-id="0c4eb-104">Part 3: Layout and Category Menu</span></span>

<span data-ttu-id="0c4eb-105">Jan [Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="0c4eb-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="0c4eb-106">Tailspin Spyworks ilustruje, w jaki sposób bardzo jest prosta, aby tworzyć zaawansowane, skalowalne aplikacje dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="0c4eb-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="0c4eb-107">W tym artykule przedstawiono sposób korzystania z doskonałych nowych funkcji w programie ASP.NET 4 do tworzenia sklepu online, w tym zakupów, wyewidencjonowywania i administrowania.</span><span class="sxs-lookup"><span data-stu-id="0c4eb-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="0c4eb-108">Ta seria samouczków zawiera szczegółowe informacje na temat wszystkich kroków podjętych w celu skompilowania przykładowej aplikacji Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="0c4eb-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="0c4eb-109">Część 3 dotyczy dodawania układu i menu kategorii.</span><span class="sxs-lookup"><span data-stu-id="0c4eb-109">Part 3 covers adding layout and a category menu.</span></span>

## <a id="_Toc260221669"></a><span data-ttu-id="0c4eb-110">Dodawanie niektórych układów i menu kategorii</span><span class="sxs-lookup"><span data-stu-id="0c4eb-110">Adding Some Layout and a Category Menu</span></span>

<span data-ttu-id="0c4eb-111">Na naszej stronie wzorcowej witryny dodamy blok DIV dla kolumny po lewej stronie, która będzie zawierać menu kategorii produktów.</span><span class="sxs-lookup"><span data-stu-id="0c4eb-111">In our site master page we'll add a div for the left side column that will contain our product category menu.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample1.aspx)]

<span data-ttu-id="0c4eb-112">Należy zauważyć, że odpowiednie wyrównanie i inne formatowanie będzie zapewnione przez klasę CSS dodaną do naszego pliku style. css.</span><span class="sxs-lookup"><span data-stu-id="0c4eb-112">Note that the desired aligning and other formatting will be provided by the CSS class that we added to our Style.css file.</span></span>

[!code-css[Main](tailspin-spyworks-part-3/samples/sample2.css)]

<span data-ttu-id="0c4eb-113">Menu Kategoria produktu zostanie dynamicznie utworzone w środowisku uruchomieniowym przez wykonanie zapytania dotyczącego bazy danych commerce dla istniejących kategorii produktów i utworzenie elementów menu i odpowiednich linków.</span><span class="sxs-lookup"><span data-stu-id="0c4eb-113">The product category menu will be dynamically created at runtime by querying the Commerce database for existing product categories and creating the menu items and corresponding links.</span></span>

<span data-ttu-id="0c4eb-114">Aby to osiągnąć, użyjemy dwóch z ASP. Zaawansowane kontrolki danych sieci.</span><span class="sxs-lookup"><span data-stu-id="0c4eb-114">To accomplish this we will use two of ASP.NET's powerful data controls.</span></span> <span data-ttu-id="0c4eb-115">Kontrolka "Źródło danych jednostki" i kontrolka "ListView".</span><span class="sxs-lookup"><span data-stu-id="0c4eb-115">The "Entity Data Source" control and the "ListView" control.</span></span>

![](tailspin-spyworks-part-3/_static/image1.jpg)

<span data-ttu-id="0c4eb-116">Przełączmy na "Widok projektu" i użyj pomocników, aby skonfigurować nasze kontrolki.</span><span class="sxs-lookup"><span data-stu-id="0c4eb-116">Let's switch to "Design View" and use the helpers to configure our controls.</span></span>

![](tailspin-spyworks-part-3/_static/image2.jpg)

<span data-ttu-id="0c4eb-117">Skonfigurujmy Właściwość Identyfikator obiektu EntityDataSource do menu EDS\_kategorii\_i kliknij pozycję "Konfiguruj źródło danych".</span><span class="sxs-lookup"><span data-stu-id="0c4eb-117">Let's set the EntityDataSource ID property to EDS\_Category\_Menu and click on "Configure Data Source".</span></span>

![](tailspin-spyworks-part-3/_static/image3.jpg)

<span data-ttu-id="0c4eb-118">Wybierz połączenie CommerceEntities, które zostało utworzone dla nas po utworzeniu modelu źródła danych jednostki dla bazy danych firmy Commerce i kliknij przycisk "dalej".</span><span class="sxs-lookup"><span data-stu-id="0c4eb-118">Select the CommerceEntities Connection that was created for us when we created the Entity Data Source Model for our Commerce Database and click "Next".</span></span>

![](tailspin-spyworks-part-3/_static/image4.jpg)

<span data-ttu-id="0c4eb-119">Wybierz nazwę zestawu jednostek "Kategorie" i pozostaw pozostałe opcje jako domyślne.</span><span class="sxs-lookup"><span data-stu-id="0c4eb-119">Select the "Categories" Entity set name and leave the rest of the options as default.</span></span> <span data-ttu-id="0c4eb-120">Kliknij przycisk "Zakończ".</span><span class="sxs-lookup"><span data-stu-id="0c4eb-120">Click "Finish".</span></span>

<span data-ttu-id="0c4eb-121">Teraz Skonfigurujmy Właściwość ID instancji formantu ListView, która została umieszczona na naszej stronie w celu udostępnienia elementu ListView\_ProductsMenu i aktywowania pomocnika.</span><span class="sxs-lookup"><span data-stu-id="0c4eb-121">Now let's set the ID property of the ListView control instance that we placed on our page to ListView\_ProductsMenu and activate its helper.</span></span>

![](tailspin-spyworks-part-3/_static/image5.jpg)

<span data-ttu-id="0c4eb-122">Mimo że możemy użyć opcji kontroli do formatowania wyświetlania i formatowania elementów danych, nasze polecenie tworzenia menu będzie wymagało tylko prostego znacznika, więc w widoku źródła wprowadzimy kod.</span><span class="sxs-lookup"><span data-stu-id="0c4eb-122">Though we could use control options to format the data item display and formatting, our menu creation will only require simple markup so we will enter the code in the source view.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample3.aspx)]

<span data-ttu-id="0c4eb-123">Zanotuj instrukcję "eval": &lt;% # eval ("CategoryName")%&gt;</span><span class="sxs-lookup"><span data-stu-id="0c4eb-123">Please note the "Eval" statement : &lt;%# Eval("CategoryName") %&gt;</span></span>

<span data-ttu-id="0c4eb-124">Składnia ASP.NET &lt;% #%&gt; jest konwencją skróconą, która instruuje środowisko uruchomieniowe, aby wykonać dowolne, które jest zawarte w i wyprowadza wyniki "w wierszu".</span><span class="sxs-lookup"><span data-stu-id="0c4eb-124">The ASP.NET syntax &lt;%# %&gt; is a shorthand convention that instructs the runtime to execute whatever is contained within and output the results "in Line".</span></span>

<span data-ttu-id="0c4eb-125">Instrukcja eval ("CategoryName") instruuje, że dla bieżącego wpisu w powiązanej kolekcji elementów danych Pobierz wartość elementów modelu jednostki "CategoryName".</span><span class="sxs-lookup"><span data-stu-id="0c4eb-125">The statement Eval("CategoryName") instructs that, for the current entry in the bound collection of data items, fetch the value of the Entity Model item names "CategoryName".</span></span> <span data-ttu-id="0c4eb-126">Jest to zwięzła składnia dla bardzo zaawansowanej funkcji.</span><span class="sxs-lookup"><span data-stu-id="0c4eb-126">This is concise syntax for a very powerful feature.</span></span>

<span data-ttu-id="0c4eb-127">Umożliwia uruchomienie aplikacji teraz.</span><span class="sxs-lookup"><span data-stu-id="0c4eb-127">Lets run the application now.</span></span>

![](tailspin-spyworks-part-3/_static/image6.jpg)

<span data-ttu-id="0c4eb-128">Należy zauważyć, że nasze menu Kategoria produktu jest teraz wyświetlane, a po umieszczeniu wskaźnika myszy nad jednym z elementów menu Kategoria zobaczysz, że na stronie nie ma jeszcze zaimplementowanej wartości Nazwa ProductsList. aspx i że został skompilowany argument ciągu zapytania dynamicznego zawierający  Identyfikator kategorii.</span><span class="sxs-lookup"><span data-stu-id="0c4eb-128">Note that our product category menu is now displayed and when we hover over one of the category menu items we can see the menu item link points to a page we have yet to implement named ProductsList.aspx and that we have built a dynamic query string argument that contains the category id.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0c4eb-129">[Poprzednie](tailspin-spyworks-part-2.md)
> [dalej](tailspin-spyworks-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="0c4eb-129">[Previous](tailspin-spyworks-part-2.md)
[Next](tailspin-spyworks-part-4.md)</span></span>

---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
title: 'Część 4: Tworzenie listy produktów | Microsoft Docs'
author: JoeStagner
description: Ta seria samouczków zawiera szczegółowe informacje na temat wszystkich kroków podjętych w celu skompilowania przykładowej aplikacji Tailspin Spyworks. Część 4 omawia produkty z licytacją...
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 4fab47d5-a6ec-4fdc-91f0-651a093a24b9
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
msc.type: authoredcontent
ms.openlocfilehash: 7af1b8afa2ecc8df9846f2edd2091b26b93a811c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78566984"
---
# <a name="part-4-listing-products"></a><span data-ttu-id="1e59d-104">Część 4. Tworzenie listy produktów</span><span class="sxs-lookup"><span data-stu-id="1e59d-104">Part 4: Listing Products</span></span>

<span data-ttu-id="1e59d-105">Jan [Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="1e59d-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="1e59d-106">Tailspin Spyworks ilustruje, w jaki sposób bardzo jest prosta, aby tworzyć zaawansowane, skalowalne aplikacje dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="1e59d-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="1e59d-107">W tym artykule przedstawiono sposób korzystania z doskonałych nowych funkcji w programie ASP.NET 4 do tworzenia sklepu online, w tym zakupów, wyewidencjonowywania i administrowania.</span><span class="sxs-lookup"><span data-stu-id="1e59d-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="1e59d-108">Ta seria samouczków zawiera szczegółowe informacje na temat wszystkich kroków podjętych w celu skompilowania przykładowej aplikacji Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="1e59d-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="1e59d-109">Część 4 obejmuje listę produktów z kontrolką GridView.</span><span class="sxs-lookup"><span data-stu-id="1e59d-109">Part 4 covers listing products with the GridView control.</span></span>

## <a id="_Toc260221670"></a><span data-ttu-id="1e59d-110">Wyświetlanie listy produktów za pomocą kontrolki GridView</span><span class="sxs-lookup"><span data-stu-id="1e59d-110">Listing Products with the GridView Control</span></span>

<span data-ttu-id="1e59d-111">Zacznijmy od wdrożenia naszej strony ProductsList. aspx, "kliknij prawym przyciskiem myszy" w naszym rozwiązaniu i wybierając pozycję "Dodaj" i "nowy element".</span><span class="sxs-lookup"><span data-stu-id="1e59d-111">Let's begin implementing our ProductsList.aspx page by "Right Clicking" on our solution and selecting "Add" and "New Item".</span></span>

![](tailspin-spyworks-part-4/_static/image1.jpg)

<span data-ttu-id="1e59d-112">Wybierz pozycję "formularz sieci Web przy użyciu strony głównej" i wprowadź nazwę strony ProductsList. aspx ".</span><span class="sxs-lookup"><span data-stu-id="1e59d-112">Choose "Web Form Using Master Page" and enter a page name of ProductsList.aspx".</span></span>

<span data-ttu-id="1e59d-113">Kliknij pozycję "Dodaj".</span><span class="sxs-lookup"><span data-stu-id="1e59d-113">Click "Add".</span></span>

![](tailspin-spyworks-part-4/_static/image2.jpg)

<span data-ttu-id="1e59d-114">Następnie wybierz folder "Style", w którym została umieszczona strona site. Master, a następnie wybierz ją z okna "zawartość folderu".</span><span class="sxs-lookup"><span data-stu-id="1e59d-114">Next choose the "Styles" folder where we placed the Site.Master page and select it from the "Contents of folder" window.</span></span>

![](tailspin-spyworks-part-4/_static/image3.jpg)

<span data-ttu-id="1e59d-115">Kliknij przycisk OK, aby utworzyć stronę.</span><span class="sxs-lookup"><span data-stu-id="1e59d-115">Click "Ok" to create the page.</span></span>

<span data-ttu-id="1e59d-116">Baza danych jest wypełniana danymi produktu, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="1e59d-116">Our database is populated with product data as seen below.</span></span>

![](tailspin-spyworks-part-4/_static/image4.jpg)

<span data-ttu-id="1e59d-117">Po utworzeniu strony będziemy ponownie używać źródła danych jednostki w celu uzyskania dostępu do danych produktu, ale w tym wystąpieniu musimy wybrać jednostki produktu i musimy ograniczyć liczbę zwracanych elementów do wybranych kategorii.</span><span class="sxs-lookup"><span data-stu-id="1e59d-117">After our page is created we'll again use an Entity Data Source to access that product data, but in this instance we need to select the Product Entities and we need to restrict the items that are returned to only those for the selected Category.</span></span>

<span data-ttu-id="1e59d-118">W tym celu poinformujemy EntityDataSource o tym, aby automatycznie generował klauzulę WHERE, i określimy WhereParameter.</span><span class="sxs-lookup"><span data-stu-id="1e59d-118">To accomplish this we'll tell the EntityDataSource to Auto Generate the WHERE clause and we'll specify the WhereParameter.</span></span>

<span data-ttu-id="1e59d-119">Przywrócisz, że po utworzeniu elementów menu w naszym menu "kategoria produktów" automatycznie utworzysz link, dodając IDKategorii do kwerendy QueryString dla każdego łącza.</span><span class="sxs-lookup"><span data-stu-id="1e59d-119">You'll recall that when we created the Menu Items in our "Product Category Menu" we dynamically built the link by adding the CategoryID to the QueryString for each link.</span></span> <span data-ttu-id="1e59d-120">Poinformujemy źródło danych jednostki, aby wyprowadził parametr WHERE z tego parametru QueryString.</span><span class="sxs-lookup"><span data-stu-id="1e59d-120">We will tell the Entity Data Source to derive the WHERE parameter from that QueryString parameter.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample1.aspx)]

<span data-ttu-id="1e59d-121">Następnie skonfigurujemy kontrolkę ListView do wyświetlania listy produktów.</span><span class="sxs-lookup"><span data-stu-id="1e59d-121">Next, we'll configure the ListView control to display a list of products.</span></span> <span data-ttu-id="1e59d-122">Aby utworzyć optymalne środowisko zakupów, będziemy kompaktować kilka zwięzłych funkcji do każdego pojedynczego produktu wyświetlanego w naszym ListVew.</span><span class="sxs-lookup"><span data-stu-id="1e59d-122">To create an optimal shopping experience we'll compact several concise features into each individual product displayed in our ListVew.</span></span>

- <span data-ttu-id="1e59d-123">Nazwa produktu będzie łączem do widoku szczegółowego produktu.</span><span class="sxs-lookup"><span data-stu-id="1e59d-123">The product name will be a link to the product's detail view.</span></span>
- <span data-ttu-id="1e59d-124">Zostanie wyświetlona cena produktu.</span><span class="sxs-lookup"><span data-stu-id="1e59d-124">The product's price will be displayed.</span></span>
- <span data-ttu-id="1e59d-125">Zostanie wyświetlony obraz produktu, który zostanie dynamicznie wybrany z katalogu obrazów wykazu w naszej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1e59d-125">An image of the product will be displayed and we'll dynamically select the image from a catalog images directory in our application.</span></span>
- <span data-ttu-id="1e59d-126">Zostanie dołączony link umożliwiający natychmiastowe dodanie określonego produktu do koszyka zakupów.</span><span class="sxs-lookup"><span data-stu-id="1e59d-126">We will include a link to immediately add the specific product to the shopping cart.</span></span>

<span data-ttu-id="1e59d-127">Oto znaczniki dla naszego wystąpienia formantu ListView.</span><span class="sxs-lookup"><span data-stu-id="1e59d-127">Here is the markup for our ListView control instance.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample2.aspx)]

<span data-ttu-id="1e59d-128">Dynamicznie tworzysz kilka linków dla każdego wyświetlanego produktu.</span><span class="sxs-lookup"><span data-stu-id="1e59d-128">We are dynamically building several links for each displayed product.</span></span>

<span data-ttu-id="1e59d-129">Ponadto przed przetestem nowej strony musimy utworzyć strukturę katalogów dla obrazów wykazu produktów w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="1e59d-129">Also, before we test own new page we need to create the directory structure for the product catalog images as follows.</span></span>

![](tailspin-spyworks-part-4/_static/image1.png)

<span data-ttu-id="1e59d-130">Gdy nasze obrazy produktu są dostępne, możemy przetestować naszą stronę z listą produktów.</span><span class="sxs-lookup"><span data-stu-id="1e59d-130">Once our product images are accessible we can test our product list page.</span></span>

![](tailspin-spyworks-part-4/_static/image5.jpg)

<span data-ttu-id="1e59d-131">Na stronie głównej witryny kliknij jeden z linków listy kategorii.</span><span class="sxs-lookup"><span data-stu-id="1e59d-131">From the site's home page, click on one of the Category List Links.</span></span>

![](tailspin-spyworks-part-4/_static/image6.jpg)

<span data-ttu-id="1e59d-132">Teraz musimy zaimplementować stronę ProductDetails. aspx i funkcje AddToCart.</span><span class="sxs-lookup"><span data-stu-id="1e59d-132">Now we need to implement the ProductDetails.aspx page and the AddToCart functionality.</span></span>

<span data-ttu-id="1e59d-133">Użyj opcji plik —&gt;nowy, aby utworzyć nazwę strony ProductDetails. aspx przy użyciu strony wzorca witryny, tak jak wcześniej.</span><span class="sxs-lookup"><span data-stu-id="1e59d-133">Use File-&gt;New to create a page name ProductDetails.aspx using the site Master Page as we did previously.</span></span>

<span data-ttu-id="1e59d-134">Będziemy ponownie używać kontrolki EntityDataSource do uzyskiwania dostępu do określonego rekordu produktu w bazie danych i użyjemy kontrolki FormView ASP.NET, aby wyświetlić dane produktu w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="1e59d-134">We will again use an EntityDataSource control to access the specific product record in the database and we will use an ASP.NET FormView control to display the product data as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample3.aspx)]

<span data-ttu-id="1e59d-135">Nie martw się, jeśli formatowanie nie będzie wyglądało na nieco zabawne.</span><span class="sxs-lookup"><span data-stu-id="1e59d-135">Don't worry if the formatting looks a bit funny to you.</span></span> <span data-ttu-id="1e59d-136">Znacznik powyżej pozostawia miejsce w układzie wyświetlania dla kilku funkcji, które zostaną zaimplementowane później.</span><span class="sxs-lookup"><span data-stu-id="1e59d-136">The markup above leaves room in the display layout for a couple of features we'll implement later on.</span></span>

<span data-ttu-id="1e59d-137">Koszyk będzie reprezentował bardziej złożoną logikę w naszej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1e59d-137">The Shopping Cart will represent the more complex logic in our application.</span></span> <span data-ttu-id="1e59d-138">Aby rozpocząć, użyj pliku&gt;New, aby utworzyć stronę o nazwie MyShoppingCart. aspx.</span><span class="sxs-lookup"><span data-stu-id="1e59d-138">To get started, use File-&gt;New to create a page called MyShoppingCart.aspx.</span></span>

<span data-ttu-id="1e59d-139">Pamiętaj, że nie wybieramy nazwy ShoppingCart. aspx.</span><span class="sxs-lookup"><span data-stu-id="1e59d-139">Note that we are not choosing the name ShoppingCart.aspx.</span></span>

<span data-ttu-id="1e59d-140">Nasza baza danych zawiera tabelę o nazwie "ShoppingCart".</span><span class="sxs-lookup"><span data-stu-id="1e59d-140">Our database contains a table named "ShoppingCart".</span></span> <span data-ttu-id="1e59d-141">Wygenerowany Entity Data Model Klasa została utworzona dla każdej tabeli w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="1e59d-141">When we generated an Entity Data Model a class was created for each table in the database.</span></span> <span data-ttu-id="1e59d-142">W związku z tym Entity Data Model wygenerowała klasy jednostki o nazwie "ShoppingCart".</span><span class="sxs-lookup"><span data-stu-id="1e59d-142">Therefore, the Entity Data Model generated an Entity Class named "ShoppingCart".</span></span> <span data-ttu-id="1e59d-143">Możemy edytować model, aby można było użyć tej nazwy dla implementacji koszyka zakupów lub przełożyć ją na nasze potrzeby, ale zamiast tego wybieramy nazwę, która pozwoli uniknąć konfliktu.</span><span class="sxs-lookup"><span data-stu-id="1e59d-143">We could edit the model so that we could use that name for our shopping cart implementation or extend it for our needs, but we will opt instead to simply select a name that will avoid the conflict.</span></span>

<span data-ttu-id="1e59d-144">Warto również zauważyć, że będziemy tworzyć prosty koszyk i osadzać logikę koszyka zakupów z uwzględnieniem koszyka zakupów.</span><span class="sxs-lookup"><span data-stu-id="1e59d-144">It's also worth noting that we will be creating a simple shopping cart and embedding the shopping cart logic with the shopping cart display.</span></span> <span data-ttu-id="1e59d-145">Możemy również zdecydować się na wdrożenie naszego koszyka w całkowicie oddzielnej warstwie biznesowej.</span><span class="sxs-lookup"><span data-stu-id="1e59d-145">We might also choose to implement our shopping cart in a completely separate Business Layer.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="1e59d-146">[Poprzednie](tailspin-spyworks-part-3.md)
> [dalej](tailspin-spyworks-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="1e59d-146">[Previous](tailspin-spyworks-part-3.md)
[Next](tailspin-spyworks-part-5.md)</span></span>

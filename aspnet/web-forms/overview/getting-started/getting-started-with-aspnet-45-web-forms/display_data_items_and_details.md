---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
title: Wyświetlanie elementów danych i szczegółów | Microsoft Docs
author: Erikre
description: W tej serii samouczków przedstawiono podstawowe informacje na temat tworzenia aplikacji ASP.NET Web Forms z ASP.NET 4,7 i Microsoft Visual Studio 2017
ms.author: riande
ms.date: 1/04/2019
ms.assetid: 64a491a8-0ed6-4c2f-9c1c-412962eb6006
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
msc.type: authoredcontent
ms.openlocfilehash: 130c9ffd29df612dac5bb954830a2eb9b738aaf0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78641114"
---
# <a name="display-data-items-and-details"></a><span data-ttu-id="9bf84-103">Wyświetlanie elementów danych i szczegółów</span><span class="sxs-lookup"><span data-stu-id="9bf84-103">Display data items and details</span></span>

<span data-ttu-id="9bf84-104">Autor [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="9bf84-104">by [Erik Reitan](https://github.com/Erikre)</span></span>

> <span data-ttu-id="9bf84-105">Ta seria samouczków uczy się podstaw tworzenia aplikacji ASP.NET Web Forms z ASP.NET 4,7 i Microsoft Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="9bf84-105">This tutorial series teaches you the basics of building an ASP.NET Web Forms application with ASP.NET 4.7 and Microsoft Visual Studio 2017.</span></span>

<span data-ttu-id="9bf84-106">W tym samouczku dowiesz się, jak wyświetlać elementy danych i szczegóły elementów danych za pomocą formularzy sieci Web ASP.NET i Code First Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="9bf84-106">In this tutorial, you'll learn how to display data items and data item details with ASP.NET Web Forms and Entity Framework Code First.</span></span> <span data-ttu-id="9bf84-107">Ten samouczek jest oparty na poprzednim samouczku "interfejs użytkownika i Nawigacja" w ramach serii samouczków Wingtip.</span><span class="sxs-lookup"><span data-stu-id="9bf84-107">This tutorial builds on the previous "UI and Navigation" tutorial as part of the Wingtip Toy Store tutorial series.</span></span> <span data-ttu-id="9bf84-108">Po ukończeniu tego samouczka zobaczysz produkty na stronie *ProductsList. aspx* oraz szczegóły produktu na stronie *ProductDetails. aspx* .</span><span class="sxs-lookup"><span data-stu-id="9bf84-108">After completing this tutorial, you'll see products on the *ProductsList.aspx* page and a product's details on the *ProductDetails.aspx* page.</span></span>

## <a name="youll-learn-how-to"></a><span data-ttu-id="9bf84-109">Omawiane tematy:</span><span class="sxs-lookup"><span data-stu-id="9bf84-109">You'll learn how to:</span></span>

- <span data-ttu-id="9bf84-110">Dodaj kontrolkę danych, aby wyświetlić produkty z bazy danych</span><span class="sxs-lookup"><span data-stu-id="9bf84-110">Add a data control to display products from the database</span></span>
- <span data-ttu-id="9bf84-111">Połącz formant danych z wybranymi danymi</span><span class="sxs-lookup"><span data-stu-id="9bf84-111">Connect a data control to the selected data</span></span>
- <span data-ttu-id="9bf84-112">Dodaj kontrolkę danych, aby wyświetlić szczegóły produktu z bazy danych</span><span class="sxs-lookup"><span data-stu-id="9bf84-112">Add a data control to display product details from the database</span></span>
- <span data-ttu-id="9bf84-113">Pobierz wartość z ciągu zapytania i Użyj tej wartości, aby ograniczyć dane pobierane z bazy danych</span><span class="sxs-lookup"><span data-stu-id="9bf84-113">Retrieve a value from the query string and use that value to limit the data that's retrieved from the database</span></span>

### <a name="features-introduced-in-this-tutorial"></a><span data-ttu-id="9bf84-114">Funkcje wprowadzone w tym samouczku:</span><span class="sxs-lookup"><span data-stu-id="9bf84-114">Features introduced in this tutorial:</span></span>

- <span data-ttu-id="9bf84-115">Powiązanie modelu</span><span class="sxs-lookup"><span data-stu-id="9bf84-115">Model binding</span></span>
- <span data-ttu-id="9bf84-116">Dostawcy wartości</span><span class="sxs-lookup"><span data-stu-id="9bf84-116">Value providers</span></span>

## <a name="add-a-data-control"></a><span data-ttu-id="9bf84-117">Dodawanie kontrolki danych</span><span class="sxs-lookup"><span data-stu-id="9bf84-117">Add a data control</span></span>

<span data-ttu-id="9bf84-118">Możesz użyć kilku różnych opcji, aby powiązać dane z kontrolką serwera.</span><span class="sxs-lookup"><span data-stu-id="9bf84-118">You can use a few different options to bind data to a server control.</span></span> <span data-ttu-id="9bf84-119">Najpopularniejsze obejmują:</span><span class="sxs-lookup"><span data-stu-id="9bf84-119">The most common include:</span></span>

* <span data-ttu-id="9bf84-120">Dodawanie kontrolki źródła danych</span><span class="sxs-lookup"><span data-stu-id="9bf84-120">Adding a data source control</span></span>
* <span data-ttu-id="9bf84-121">Ręczne dodawanie kodu</span><span class="sxs-lookup"><span data-stu-id="9bf84-121">Adding code by hand</span></span>
* <span data-ttu-id="9bf84-122">Korzystanie z powiązania modelu</span><span class="sxs-lookup"><span data-stu-id="9bf84-122">Using model binding</span></span>

### <a name="use-a-data-source-control-to-bind-data"></a><span data-ttu-id="9bf84-123">Używanie kontroli źródła danych do wiązania danych</span><span class="sxs-lookup"><span data-stu-id="9bf84-123">Use a data source control to bind data</span></span>

<span data-ttu-id="9bf84-124">Dodanie kontroli źródła danych umożliwia połączenie kontroli źródła danych z kontrolką wyświetlającą dane.</span><span class="sxs-lookup"><span data-stu-id="9bf84-124">Adding a data source control allows you to link the data source control to the control that displays the data.</span></span> <span data-ttu-id="9bf84-125">Dzięki temu można deklaratywnie, a nie programowo, łączyć kontrolki po stronie serwera ze źródłami danych.</span><span class="sxs-lookup"><span data-stu-id="9bf84-125">With this approach, you can declaratively,  rather than programmatically, connect server-side controls to data sources.</span></span>

### <a name="code-by-hand-to-bind-data"></a><span data-ttu-id="9bf84-126">Zakoduj ręcznie, aby powiązać dane</span><span class="sxs-lookup"><span data-stu-id="9bf84-126">Code by hand to bind data</span></span>

<span data-ttu-id="9bf84-127">Ręczne kodowanie obejmuje:</span><span class="sxs-lookup"><span data-stu-id="9bf84-127">Coding by hand involves:</span></span>

1. <span data-ttu-id="9bf84-128">Odczytywanie wartości</span><span class="sxs-lookup"><span data-stu-id="9bf84-128">Reading a value</span></span>
2. <span data-ttu-id="9bf84-129">Sprawdzanie, czy jest to wartość zerowa</span><span class="sxs-lookup"><span data-stu-id="9bf84-129">Checking if it's null</span></span>
3. <span data-ttu-id="9bf84-130">Konwertowanie na odpowiedni typ</span><span class="sxs-lookup"><span data-stu-id="9bf84-130">Converting it to an appropriate type</span></span>
4. <span data-ttu-id="9bf84-131">Sprawdzanie powodzenia konwersji</span><span class="sxs-lookup"><span data-stu-id="9bf84-131">Checking conversion success</span></span>
5. <span data-ttu-id="9bf84-132">Używanie wartości w zapytaniu</span><span class="sxs-lookup"><span data-stu-id="9bf84-132">Using the value in the query</span></span> 

<span data-ttu-id="9bf84-133">Takie podejście umożliwia pełną kontrolę nad logiką dostępu do danych.</span><span class="sxs-lookup"><span data-stu-id="9bf84-133">This approach lets you have full control over your data-access logic.</span></span>

### <a name="use-model-binding-to-bind-data"></a><span data-ttu-id="9bf84-134">Użyj powiązania modelu, aby powiązać dane</span><span class="sxs-lookup"><span data-stu-id="9bf84-134">Use model binding to bind data</span></span>

<span data-ttu-id="9bf84-135">Powiązanie modelu pozwala powiązać wyniki z znacznie mniejszym kodem i umożliwia ponowne używanie funkcji w całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9bf84-135">Model binding lets you bind results with far less code and gives you the ability to reuse the functionality throughout your application.</span></span> <span data-ttu-id="9bf84-136">Upraszcza ona pracę z logiką dostępu do danych ukierunkowanych na kod, zapewniając jednocześnie rozbudowaną strukturę powiązań danych.</span><span class="sxs-lookup"><span data-stu-id="9bf84-136">It simplifies working with code-focused data-access logic while still providing a rich, data-binding framework.</span></span>

## <a name="display-products"></a><span data-ttu-id="9bf84-137">Wyświetl produkty</span><span class="sxs-lookup"><span data-stu-id="9bf84-137">Display products</span></span>

<span data-ttu-id="9bf84-138">W tym samouczku użyjesz powiązania modelu, aby powiązać dane.</span><span class="sxs-lookup"><span data-stu-id="9bf84-138">In this tutorial, you'll use model binding to bind data.</span></span> <span data-ttu-id="9bf84-139">Aby skonfigurować kontrolkę danych do używania powiązania modelu w celu wybrania danych, należy ustawić właściwość `SelectMethod` formantu na nazwę metody w kodzie strony.</span><span class="sxs-lookup"><span data-stu-id="9bf84-139">To configure a data control to use model binding to select data, you set the control's `SelectMethod` property to a method name in the page's code.</span></span> <span data-ttu-id="9bf84-140">Kontrola danych wywołuje metodę w odpowiednim czasie w cyklu życia strony i automatycznie wiąże zwrócone dane.</span><span class="sxs-lookup"><span data-stu-id="9bf84-140">The data control calls the method at the appropriate time in the page life cycle and automatically binds the returned data.</span></span> <span data-ttu-id="9bf84-141">Nie trzeba jawnie wywoływać metody `DataBind`.</span><span class="sxs-lookup"><span data-stu-id="9bf84-141">There's no need to explicitly call the `DataBind` method.</span></span>

1. <span data-ttu-id="9bf84-142">W **Eksplorator rozwiązań**Otwórz *ProductList. aspx*.</span><span class="sxs-lookup"><span data-stu-id="9bf84-142">In **Solution Explorer**, open *ProductList.aspx*.</span></span>
2. <span data-ttu-id="9bf84-143">Zastąp istniejące znaczniki tym znacznikiem:</span><span class="sxs-lookup"><span data-stu-id="9bf84-143">Replace the existing markup with this markup:</span></span>   

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample1.aspx)]

<span data-ttu-id="9bf84-144">Ten kod używa kontrolki **ListView** o nazwie `productList` do wyświetlania produktów.</span><span class="sxs-lookup"><span data-stu-id="9bf84-144">This code uses a **ListView** control named `productList` to display products.</span></span>

[!code-aspx-csharp[Main](display_data_items_and_details/samples/sample2.aspx)]

<span data-ttu-id="9bf84-145">Za pomocą szablonów i stylów definiujesz sposób wyświetlania danych w formancie **ListView** .</span><span class="sxs-lookup"><span data-stu-id="9bf84-145">With templates and styles, you define how the **ListView** control displays data.</span></span> <span data-ttu-id="9bf84-146">Jest to przydatne w przypadku danych w każdej powtarzającej się strukturze.</span><span class="sxs-lookup"><span data-stu-id="9bf84-146">It's useful for data in any repeating structure.</span></span> <span data-ttu-id="9bf84-147">Chociaż ten **element ListView** przykład po prostu wyświetla dane bazy danych, można również bez kodu, umożliwić użytkownikom edytowanie, wstawianie i usuwanie danych oraz sortowanie i dane stronicowe.</span><span class="sxs-lookup"><span data-stu-id="9bf84-147">Though this **ListView** example simply displays database data, you can also, without code, enable users to edit, insert, and delete data, and to sort and page data.</span></span>

<span data-ttu-id="9bf84-148">Ustawienie właściwości `ItemType` w formancie **ListView** powoduje, że wyrażenie powiązania danych `Item` jest dostępne, a kontrolka będzie silnie wpisana.</span><span class="sxs-lookup"><span data-stu-id="9bf84-148">By setting the `ItemType` property in the **ListView** control, the data-binding expression `Item` is available and the control becomes strongly typed.</span></span> <span data-ttu-id="9bf84-149">Jak wspomniano w poprzednim samouczku, można wybrać szczegóły obiektu elementu z technologią IntelliSense, na przykład określając `ProductName`:</span><span class="sxs-lookup"><span data-stu-id="9bf84-149">As mentioned in the previous tutorial, you can select Item object details with IntelliSense, such as specifying the `ProductName`:</span></span>

![Wyświetlanie elementów danych i szczegółów — IntelliSense](display_data_items_and_details/_static/image1.png)

<span data-ttu-id="9bf84-151">Używasz również powiązania modelu, aby określić wartość `SelectMethod`.</span><span class="sxs-lookup"><span data-stu-id="9bf84-151">You're also using model binding to specify a `SelectMethod` value.</span></span> <span data-ttu-id="9bf84-152">Ta wartość (`GetProducts`) odnosi się do metody, która zostanie dodana do kodu, aby wyświetlić produkty w następnym kroku.</span><span class="sxs-lookup"><span data-stu-id="9bf84-152">This value (`GetProducts`) corresponds to the method you'll add to the code behind to display products in the next step.</span></span>

### <a name="add-code-to-display-products"></a><span data-ttu-id="9bf84-153">Dodaj kod, aby wyświetlić produkty</span><span class="sxs-lookup"><span data-stu-id="9bf84-153">Add code to display products</span></span>

<span data-ttu-id="9bf84-154">W tym kroku dodasz kod, aby wypełnić formant **ListView** danymi produktu z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="9bf84-154">In this step, you'll add code to populate the **ListView** control with product data from the database.</span></span> <span data-ttu-id="9bf84-155">Kod obsługuje wyświetlanie wszystkich produktów i produktów poszczególnych kategorii.</span><span class="sxs-lookup"><span data-stu-id="9bf84-155">The code supports showing all products and  individual category products.</span></span>

1. <span data-ttu-id="9bf84-156">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy *ProductList. aspx* , a następnie wybierz polecenie **Wyświetl kod**.</span><span class="sxs-lookup"><span data-stu-id="9bf84-156">In **Solution Explorer**, right-click *ProductList.aspx* and then select **View Code**.</span></span>
2. <span data-ttu-id="9bf84-157">Zastąp istniejący kod w pliku *ProductList.aspx.cs* :</span><span class="sxs-lookup"><span data-stu-id="9bf84-157">Replace the existing code in the *ProductList.aspx.cs* file with this:</span></span>   

    [!code-csharp[Main](display_data_items_and_details/samples/sample3.cs)]

<span data-ttu-id="9bf84-158">Ten kod przedstawia metodę `GetProducts`, do której odwołuje się Właściwość `ItemType` formantu **ListView** na stronie *ProductList. aspx* .</span><span class="sxs-lookup"><span data-stu-id="9bf84-158">This code shows the `GetProducts` method that the **ListView** control's `ItemType` property references in the *ProductList.aspx* page.</span></span> <span data-ttu-id="9bf84-159">Aby ograniczyć wyniki do określonej kategorii bazy danych, kod ustawia wartość `categoryId` z wartości ciągu zapytania przesłanej do strony *ProductList. aspx* po przejściu do strony *ProductList. aspx* .</span><span class="sxs-lookup"><span data-stu-id="9bf84-159">To limit the results to a specific database category, the code sets the `categoryId` value from the query string value passed to the *ProductList.aspx* page when the *ProductList.aspx* page is navigated to.</span></span> <span data-ttu-id="9bf84-160">Klasa `QueryStringAttribute` w przestrzeni nazw `System.Web.ModelBinding` służy do pobierania wartości zmiennej ciągu zapytania `id`.</span><span class="sxs-lookup"><span data-stu-id="9bf84-160">The `QueryStringAttribute` class in the `System.Web.ModelBinding` namespace is used to retrieve the value of the query string variable `id`.</span></span> <span data-ttu-id="9bf84-161">Powoduje to, że powiązanie modelu próbuje powiązać wartość z ciągu zapytania do parametru `categoryId` w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="9bf84-161">This instructs model binding to try to bind a value from the query string to the `categoryId` parameter at run time.</span></span>

<span data-ttu-id="9bf84-162">Gdy poprawna kategoria jest przenoszona jako ciąg zapytania do strony, wyniki zapytania są ograniczone do tych produktów w bazie danych, które pasują do wartości `categoryId`.</span><span class="sxs-lookup"><span data-stu-id="9bf84-162">When a valid category is passed as a query string to the page, the results of the query are limited to those products in the database that match the `categoryId` value.</span></span> <span data-ttu-id="9bf84-163">Na przykład jeśli adres URL strony *ProductsList. aspx* jest następujący:</span><span class="sxs-lookup"><span data-stu-id="9bf84-163">For instance, if the *ProductsList.aspx* page URL is this:</span></span>

[!code-console[Main](display_data_items_and_details/samples/sample4.cmd)]

<span data-ttu-id="9bf84-164">Na stronie są wyświetlane tylko te produkty, dla których `categoryId` jest równa `1`.</span><span class="sxs-lookup"><span data-stu-id="9bf84-164">The page displays only the products where the `categoryId` equals `1`.</span></span>

<span data-ttu-id="9bf84-165">Wszystkie produkty są wyświetlane, jeśli nie zostanie uwzględniony ciąg zapytania po wywołaniu strony *ProductList. aspx* .</span><span class="sxs-lookup"><span data-stu-id="9bf84-165">All products are displayed if no query string is included when the *ProductList.aspx* page is called.</span></span>

<span data-ttu-id="9bf84-166">Źródła wartości dla tych metod są określane jako *dostawcy wartości* (takie jak *QueryString*) i atrybuty parametrów wskazujące, który dostawca wartości ma być używany, jest określany jako *atrybuty dostawcy wartości* (takie jak `id`).</span><span class="sxs-lookup"><span data-stu-id="9bf84-166">The sources of values for these methods are referred to as *value providers* (such as *QueryString*), and the parameter attributes that indicate which value provider to use are referred to as *value provider attributes* (such as `id`).</span></span> <span data-ttu-id="9bf84-167">ASP.NET obejmuje dostawców wartości i odpowiadające im atrybuty dla wszystkich typowych źródeł danych wejściowych użytkownika w aplikacji formularzy sieci Web, takich jak ciąg zapytania, pliki cookie, wartości formularzy, kontrolki, stan widoku, stan sesji i właściwości profilu.</span><span class="sxs-lookup"><span data-stu-id="9bf84-167">ASP.NET includes value providers and corresponding attributes for all of the typical sources of user input in a Web Forms application such as the query string, cookies, form values, controls, view state, session state, and profile properties.</span></span> <span data-ttu-id="9bf84-168">Możesz również pisać dostawców wartości niestandardowych.</span><span class="sxs-lookup"><span data-stu-id="9bf84-168">You can also write custom value providers.</span></span>

### <a name="run-the-application"></a><span data-ttu-id="9bf84-169">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="9bf84-169">Run the application</span></span>

<span data-ttu-id="9bf84-170">Uruchom aplikację teraz, aby wyświetlić wszystkie produkty lub produkty kategorii.</span><span class="sxs-lookup"><span data-stu-id="9bf84-170">Run the application now to view all products or a category's products.</span></span>

1. <span data-ttu-id="9bf84-171">Naciśnij klawisz **F5** w programie Visual Studio, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="9bf84-171">Press **F5** while in Visual Studio to run the application.</span></span>  
   <span data-ttu-id="9bf84-172">Zostanie otwarta przeglądarka i zostanie wyświetlona strona *default. aspx* .</span><span class="sxs-lookup"><span data-stu-id="9bf84-172">The browser opens and shows the *Default.aspx* page.</span></span>

2. <span data-ttu-id="9bf84-173">Wybierz pozycję **samochody** z menu nawigacji kategorii produktów.</span><span class="sxs-lookup"><span data-stu-id="9bf84-173">Select **Cars** from the product category navigation menu.</span></span>  
   <span data-ttu-id="9bf84-174">Na stronie *ProductList. aspx* wyświetlane są tylko produkty kategorii **samochodów** .</span><span class="sxs-lookup"><span data-stu-id="9bf84-174">The *ProductList.aspx* page displays showing only **Cars** category products.</span></span> <span data-ttu-id="9bf84-175">W dalszej części tego samouczka zostaną wyświetlone szczegółowe informacje o produkcie.</span><span class="sxs-lookup"><span data-stu-id="9bf84-175">Later in this tutorial, you'll display product details.</span></span>  

    ![Wyświetlanie elementów danych i szczegółów — samochody](display_data_items_and_details/_static/image2.png)

3. <span data-ttu-id="9bf84-177">Wybierz pozycję **produkty** z menu nawigacji u góry.</span><span class="sxs-lookup"><span data-stu-id="9bf84-177">Select **Products** from the navigation menu at the top.</span></span>  
   <span data-ttu-id="9bf84-178">Ponownie zostanie wyświetlona strona *ProductList. aspx* , jednak spowoduje to wyświetlenie całej listy produktów.</span><span class="sxs-lookup"><span data-stu-id="9bf84-178">Again, the *ProductList.aspx* page is displayed, however this time it shows the entire list of products.</span></span>   

    ![Wyświetlanie elementów danych i szczegółów — produkty](display_data_items_and_details/_static/image3.png)

4. <span data-ttu-id="9bf84-180">Zamknij przeglądarkę i wróć do programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9bf84-180">Close the browser and return to Visual Studio.</span></span>

### <a name="add-a-data-control-to-display-product-details"></a><span data-ttu-id="9bf84-181">Dodaj kontrolkę danych, aby wyświetlić szczegóły produktu</span><span class="sxs-lookup"><span data-stu-id="9bf84-181">Add a data control to display product details</span></span>

<span data-ttu-id="9bf84-182">Następnie zmodyfikujesz znaczniki na stronie *ProductDetails. aspx* , która została dodana w poprzednim samouczku, aby wyświetlić określone informacje o produkcie.</span><span class="sxs-lookup"><span data-stu-id="9bf84-182">Next, you'll modify the markup in the *ProductDetails.aspx* page that you added in the previous tutorial to display specific product information.</span></span>

1. <span data-ttu-id="9bf84-183">W **Eksplorator rozwiązań**Otwórz *ProductDetails. aspx*.</span><span class="sxs-lookup"><span data-stu-id="9bf84-183">In **Solution Explorer**, open *ProductDetails.aspx*.</span></span>

2. <span data-ttu-id="9bf84-184">Zastąp istniejące znaczniki tym znacznikiem:</span><span class="sxs-lookup"><span data-stu-id="9bf84-184">Replace the existing markup with this markup:</span></span>

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample5.aspx)] 

    <span data-ttu-id="9bf84-185">Ten kod używa kontrolki **FormView** do wyświetlania szczegółowych informacji o produkcie.</span><span class="sxs-lookup"><span data-stu-id="9bf84-185">This code uses a **FormView** control to display specific product details.</span></span> <span data-ttu-id="9bf84-186">Ten znacznik używa metod, takich jak metody używane do wyświetlania danych na stronie *ProductList. aspx* .</span><span class="sxs-lookup"><span data-stu-id="9bf84-186">This markup uses methods like the methods used to display data in the *ProductList.aspx* page.</span></span> <span data-ttu-id="9bf84-187">Kontrolka **FormView** służy do wyświetlania pojedynczego rekordu w czasie ze źródła danych.</span><span class="sxs-lookup"><span data-stu-id="9bf84-187">The **FormView** control is used to display a single record at a time from a data source.</span></span> <span data-ttu-id="9bf84-188">Gdy używasz kontrolki **FormView** , tworzysz szablony, aby wyświetlać i edytować wartości powiązane z danymi.</span><span class="sxs-lookup"><span data-stu-id="9bf84-188">When you use the **FormView** control, you create templates to display and edit data-bound values.</span></span> <span data-ttu-id="9bf84-189">Szablony te zawierają kontrolki, wyrażenia powiązań i formatowanie, które definiują wygląd i funkcjonalność formularza.</span><span class="sxs-lookup"><span data-stu-id="9bf84-189">These templates contain controls, binding expressions, and formatting that define the form's look and functionality.</span></span>

<span data-ttu-id="9bf84-190">Połączenie z poprzednim znacznikiem do bazy danych wymaga dodatkowego kodu.</span><span class="sxs-lookup"><span data-stu-id="9bf84-190">Connecting the previous markup to the database requires additional code.</span></span>

1. <span data-ttu-id="9bf84-191">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy *ProductDetails. aspx* , a następnie kliknij polecenie **Wyświetl kod**.</span><span class="sxs-lookup"><span data-stu-id="9bf84-191">In **Solution Explorer**, right-click *ProductDetails.aspx* and then click **View Code**.</span></span>  
   <span data-ttu-id="9bf84-192">Zostanie wyświetlony plik *ProductDetails.aspx.cs* .</span><span class="sxs-lookup"><span data-stu-id="9bf84-192">The *ProductDetails.aspx.cs* file is displayed.</span></span>

2. <span data-ttu-id="9bf84-193">Zastąp istniejący kod tym kodem:</span><span class="sxs-lookup"><span data-stu-id="9bf84-193">Replace the existing code with this code:</span></span>   

    [!code-csharp[Main](display_data_items_and_details/samples/sample6.cs)]

<span data-ttu-id="9bf84-194">Ten kod sprawdza wartość ciągu zapytania "`productID`".</span><span class="sxs-lookup"><span data-stu-id="9bf84-194">This code checks for a "`productID`" query-string value.</span></span> <span data-ttu-id="9bf84-195">Jeśli zostanie znaleziona prawidłowa wartość ciągu zapytania, zostanie wyświetlony pasujący produkt.</span><span class="sxs-lookup"><span data-stu-id="9bf84-195">If a valid query-string value is found, the matching product is displayed.</span></span> <span data-ttu-id="9bf84-196">Jeśli nie można odnaleźć ciągu zapytania lub jego wartość jest nieprawidłowa, nie jest wyświetlany żaden produkt.</span><span class="sxs-lookup"><span data-stu-id="9bf84-196">If the query-string isn't found, or its value isn't valid, no product is displayed.</span></span>

### <a name="run-the-application"></a><span data-ttu-id="9bf84-197">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="9bf84-197">Run the application</span></span>

<span data-ttu-id="9bf84-198">Teraz możesz uruchomić aplikację, aby zobaczyć pojedynczy produkt wyświetlany na podstawie identyfikatora produktu.</span><span class="sxs-lookup"><span data-stu-id="9bf84-198">Now you can run the application to see an individual product displayed based on product ID.</span></span>

1. <span data-ttu-id="9bf84-199">Naciśnij klawisz **F5** w programie Visual Studio, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="9bf84-199">Press **F5** while in Visual Studio to run the application.</span></span>  
   <span data-ttu-id="9bf84-200">Zostanie otwarta przeglądarka i zostanie wyświetlona strona *default. aspx* .</span><span class="sxs-lookup"><span data-stu-id="9bf84-200">The browser opens and shows the *Default.aspx* page.</span></span>

2. <span data-ttu-id="9bf84-201">Wybierz pozycję **łodzie** w menu nawigacji kategorii.</span><span class="sxs-lookup"><span data-stu-id="9bf84-201">Select **Boats** from the category navigation menu.</span></span>  
   <span data-ttu-id="9bf84-202">Zostanie wyświetlona strona *ProductList. aspx* .</span><span class="sxs-lookup"><span data-stu-id="9bf84-202">The *ProductList.aspx* page is displayed.</span></span>

3. <span data-ttu-id="9bf84-203">Wybierz pozycję **papierowa łodzi** z listy produkt.</span><span class="sxs-lookup"><span data-stu-id="9bf84-203">Select **Paper Boat** from the product list.</span></span>
   <span data-ttu-id="9bf84-204">Zostanie wyświetlona strona *ProductDetails. aspx* .</span><span class="sxs-lookup"><span data-stu-id="9bf84-204">The *ProductDetails.aspx* page is displayed.</span></span>

    ![Wyświetlanie elementów danych i szczegółów — produkty](display_data_items_and_details/_static/image4.png)
    
4. <span data-ttu-id="9bf84-206">Zamknij okno przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="9bf84-206">Close the browser.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9bf84-207">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="9bf84-207">Additional resources</span></span>

[<span data-ttu-id="9bf84-208">Pobieranie i wyświetlanie danych przy użyciu powiązania modelu i formularzy sieci Web</span><span class="sxs-lookup"><span data-stu-id="9bf84-208">Retrieving and displaying data with model binding and web forms</span></span>](../../presenting-and-managing-data/model-binding/retrieving-data.md)

## <a name="next-steps"></a><span data-ttu-id="9bf84-209">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="9bf84-209">Next steps</span></span>

<span data-ttu-id="9bf84-210">W tym samouczku dodano znaczniki i kod, aby wyświetlić produkty i informacje o produkcie.</span><span class="sxs-lookup"><span data-stu-id="9bf84-210">In this tutorial, you added markup and code to display products and product details.</span></span> <span data-ttu-id="9bf84-211">Informacje o silnych typach kontroli danych, powiązaniach modelu i dostawcach wartości.</span><span class="sxs-lookup"><span data-stu-id="9bf84-211">You learned about strongly typed data controls, model binding, and value providers.</span></span> <span data-ttu-id="9bf84-212">W następnym samouczku dodasz koszyk do przykładowej aplikacji Wingtip zabawki.</span><span class="sxs-lookup"><span data-stu-id="9bf84-212">In the next tutorial, you'll add a shopping cart to the Wingtip Toys sample application.</span></span> 

> [!div class="step-by-step"]
> <span data-ttu-id="9bf84-213">[Poprzednie](ui_and_navigation.md)
> [dalej](shopping-cart.md)</span><span class="sxs-lookup"><span data-stu-id="9bf84-213">[Previous](ui_and_navigation.md)
[Next](shopping-cart.md)</span></span>

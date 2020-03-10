---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
title: 'Część 7: Dodawanie funkcji | Microsoft Docs'
author: JoeStagner
description: Ta seria samouczków zawiera szczegółowe informacje na temat wszystkich kroków podjętych w celu skompilowania przykładowej aplikacji Tailspin Spyworks. Część 7 dodaje dodatkowe funkcje, takie jak Revie konta...
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 50223ee9-11b9-4cf3-bca2-e2f10bf471f3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
msc.type: authoredcontent
ms.openlocfilehash: ffd2b862c727db9572c272b7b21bcc33c822fffa
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78641996"
---
# <a name="part-7-adding-features"></a><span data-ttu-id="ed95e-104">Część 7. Dodawanie funkcji</span><span class="sxs-lookup"><span data-stu-id="ed95e-104">Part 7: Adding Features</span></span>

<span data-ttu-id="ed95e-105">Jan [Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="ed95e-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="ed95e-106">Tailspin Spyworks ilustruje, w jaki sposób bardzo jest prosta, aby tworzyć zaawansowane, skalowalne aplikacje dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="ed95e-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="ed95e-107">W tym artykule przedstawiono sposób korzystania z doskonałych nowych funkcji w programie ASP.NET 4 do tworzenia sklepu online, w tym zakupów, wyewidencjonowywania i administrowania.</span><span class="sxs-lookup"><span data-stu-id="ed95e-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="ed95e-108">Ta seria samouczków zawiera szczegółowe informacje na temat wszystkich kroków podjętych w celu skompilowania przykładowej aplikacji Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="ed95e-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="ed95e-109">Część 7 dodaje dodatkowe funkcje, takie jak przegląd konta, przeglądy produktów i "popularne elementy" oraz "zakupione również" kontrolki użytkownika.</span><span class="sxs-lookup"><span data-stu-id="ed95e-109">Part 7 adds additional features, such as account review, product reviews, and "popular items" and "also purchased" user controls.</span></span>

## <a id="_Toc260221673"></a><span data-ttu-id="ed95e-110">Dodawanie funkcji</span><span class="sxs-lookup"><span data-stu-id="ed95e-110">Adding Features</span></span>

<span data-ttu-id="ed95e-111">Chociaż użytkownicy mogą przeglądać nasz katalog, umieszczać elementy w koszyku zakupów i dokończyć proces wyewidencjonowania, można dołączać do nich wiele funkcji pomocniczych w celu ulepszania naszej witryny.</span><span class="sxs-lookup"><span data-stu-id="ed95e-111">Though users can browse our catalog, place items in their shopping cart, and complete the checkout process, there are a number of supporting features that we will include to improve our site.</span></span>

1. <span data-ttu-id="ed95e-112">Przegląd konta (Lista zamówień umieszczonych i Wyświetl szczegóły).</span><span class="sxs-lookup"><span data-stu-id="ed95e-112">Account Review (List orders placed and view details.)</span></span>
2. <span data-ttu-id="ed95e-113">Dodaj zawartość specyficzną dla kontekstu do strony frontonu.</span><span class="sxs-lookup"><span data-stu-id="ed95e-113">Add some context specific content to the front page.</span></span>
3. <span data-ttu-id="ed95e-114">Dodaj funkcję, aby umożliwić użytkownikom przeglądanie produktów w wykazie.</span><span class="sxs-lookup"><span data-stu-id="ed95e-114">Add a feature to let users Review the products in the catalog.</span></span>
4. <span data-ttu-id="ed95e-115">Utwórz kontrolkę użytkownika, aby wyświetlić popularne elementy i umieścić tę kontrolkę na stronie frontonu.</span><span class="sxs-lookup"><span data-stu-id="ed95e-115">Create a User Control to display Popular Items and Place that control on the front page.</span></span>
5. <span data-ttu-id="ed95e-116">Utwórz "zakupione również" kontrolkę użytkownika i Dodaj ją do strony szczegółów produktu.</span><span class="sxs-lookup"><span data-stu-id="ed95e-116">Create an "Also Purchased" user control and add it to the product details page.</span></span>
6. <span data-ttu-id="ed95e-117">Dodaj stronę kontaktu.</span><span class="sxs-lookup"><span data-stu-id="ed95e-117">Add a Contact Page.</span></span>
7. <span data-ttu-id="ed95e-118">Dodaj stronę informacje.</span><span class="sxs-lookup"><span data-stu-id="ed95e-118">Add an About Page.</span></span>
8. <span data-ttu-id="ed95e-119">Błąd globalny</span><span class="sxs-lookup"><span data-stu-id="ed95e-119">Global Error</span></span>

## <a id="_Toc260221674"></a><span data-ttu-id="ed95e-120">Przegląd konta</span><span class="sxs-lookup"><span data-stu-id="ed95e-120">Account Review</span></span>

<span data-ttu-id="ed95e-121">W folderze "Account" Utwórz dwie strony. aspx o nazwie OrderList. aspx, a drugie o nazwie OrderDetails. aspx</span><span class="sxs-lookup"><span data-stu-id="ed95e-121">In the "Account" folder create two .aspx pages one named OrderList.aspx and the other named OrderDetails.aspx</span></span>

<span data-ttu-id="ed95e-122">OrderList. aspx będzie korzystać z kontrolek GridView i EntityDataSource, tak jak wcześniej.</span><span class="sxs-lookup"><span data-stu-id="ed95e-122">OrderList.aspx will leverage the GridView and EntityDataSource controls much as we have previously.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample1.aspx)]

<span data-ttu-id="ed95e-123">Obiekt EntityDataSource wybiera rekordy z tabeli Orders filtrowanej według nazwy użytkownika (zobacz WhereParameter), która została ustawiona w zmiennej sesji podczas logowania użytkownika.</span><span class="sxs-lookup"><span data-stu-id="ed95e-123">The EntityDataSource selects records from the Orders table filtered on the UserName (see the WhereParameter) which we set in a session variable when the user log's in.</span></span>

<span data-ttu-id="ed95e-124">Zwróć uwagę na to, że te parametry HyperlinkField widoku GridView:</span><span class="sxs-lookup"><span data-stu-id="ed95e-124">Note also these parameters in the HyperlinkField of the GridView:</span></span>

[!code-xml[Main](tailspin-spyworks-part-7/samples/sample2.xml)]

<span data-ttu-id="ed95e-125">Określają one łącze do widoku szczegółów zamówienia dla każdego produktu, określając pole IDZamówienia jako parametr QueryString na stronie OrderDetails. aspx.</span><span class="sxs-lookup"><span data-stu-id="ed95e-125">These specify the link to the Order details view for each product specifying the OrderID field as a QueryString parameter to the OrderDetails.aspx page.</span></span>

## <a id="_Toc260221675"></a><span data-ttu-id="ed95e-126">OrderDetails. aspx</span><span class="sxs-lookup"><span data-stu-id="ed95e-126">OrderDetails.aspx</span></span>

<span data-ttu-id="ed95e-127">Użyjemy kontrolki EntityDataSource do uzyskiwania dostępu do zamówień i FormView, aby wyświetlić dane zamówienia i inny obiekt EntityDataSource z elementem GridView, aby wyświetlić wszystkie elementy wiersza zamówienia.</span><span class="sxs-lookup"><span data-stu-id="ed95e-127">We will use an EntityDataSource control to access the Orders and a FormView to display the Order data and another EntityDataSource with a GridView to display all the Order's line items.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample3.aspx)]

<span data-ttu-id="ed95e-128">W pliku związanym z kodem (OrderDetails.aspx.cs) mamy dwa małe bity dla gospodarstw domowych.</span><span class="sxs-lookup"><span data-stu-id="ed95e-128">In the Code Behind file (OrderDetails.aspx.cs) we have two little bits of housekeeping.</span></span>

<span data-ttu-id="ed95e-129">Najpierw musimy upewnić się, że OrderDetails zawsze otrzymuje IDZamówienia.</span><span class="sxs-lookup"><span data-stu-id="ed95e-129">First we need to make sure that OrderDetails always gets an OrderId.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample4.cs)]

<span data-ttu-id="ed95e-130">Musimy również obliczyć i wyświetlić sumę zamówień z elementów wiersza.</span><span class="sxs-lookup"><span data-stu-id="ed95e-130">We also need to calculate and display the order total from the line items.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample5.cs)]

## <a id="_Toc260221676"></a><span data-ttu-id="ed95e-131">Strona główna</span><span class="sxs-lookup"><span data-stu-id="ed95e-131">The Home Page</span></span>

<span data-ttu-id="ed95e-132">Dodajmy zawartość statyczną do domyślnej strony. aspx.</span><span class="sxs-lookup"><span data-stu-id="ed95e-132">Let's add some static content to the Default.aspx page.</span></span>

<span data-ttu-id="ed95e-133">Najpierw utworzymy folder "Content" (zawartość), a w tym folderze obrazów (i dodamy obraz do użycia na stronie głównej).</span><span class="sxs-lookup"><span data-stu-id="ed95e-133">First I'll create a "Content" folder and within it an Images folder (and I'll include an image to be used on the home page.)</span></span>

<span data-ttu-id="ed95e-134">Na dolny symbol zastępczy domyślnej strony. aspx Dodaj następujący znacznik.</span><span class="sxs-lookup"><span data-stu-id="ed95e-134">Into the bottom placeholder of the Default.aspx page, add the following markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample6.aspx)]

## <a id="_Toc260221677"></a><span data-ttu-id="ed95e-135">Przeglądy produktu</span><span class="sxs-lookup"><span data-stu-id="ed95e-135">Product Reviews</span></span>

<span data-ttu-id="ed95e-136">Najpierw dodamy przycisk z linkiem do formularza, za pomocą którego możemy wprowadzić przegląd produktu.</span><span class="sxs-lookup"><span data-stu-id="ed95e-136">First we'll add a button with a link to a form that we can use to enter a product review.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample7.aspx)]

![](tailspin-spyworks-part-7/_static/image1.jpg)

<span data-ttu-id="ed95e-137">Zwróć uwagę, że przekazujemy klucz ProductID w ciągu zapytania</span><span class="sxs-lookup"><span data-stu-id="ed95e-137">Note that we are passing the ProductID in the query string</span></span>

<span data-ttu-id="ed95e-138">Dodaj stronę o nazwie ReviewAdd. aspx</span><span class="sxs-lookup"><span data-stu-id="ed95e-138">Next let's add page named ReviewAdd.aspx</span></span>

<span data-ttu-id="ed95e-139">Ta strona będzie używać zestawu narzędzi ASP.NET AJAX Control.</span><span class="sxs-lookup"><span data-stu-id="ed95e-139">This page will use the ASP.NET AJAX Control Toolkit.</span></span> <span data-ttu-id="ed95e-140">Jeśli jeszcze tego nie zrobiono, możesz pobrać go z [subskrypcja DevExpress](http://devexpress.com/act) i uzyskać wskazówki dotyczące konfigurowania zestawu narzędzi do użycia z programem Visual Studio tutaj [https://www.asp.net/learn/ajax-videos/video-76.aspx](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md).</span><span class="sxs-lookup"><span data-stu-id="ed95e-140">If you have not already done so you can download it from [DevExpress](http://devexpress.com/act) and there is guidance on setting up the toolkit for use with Visual Studio here [https://www.asp.net/learn/ajax-videos/video-76.aspx](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md).</span></span>

<span data-ttu-id="ed95e-141">W trybie projektowania przeciągnij kontrolki i moduły walidacji z przybornika i Utwórz formularz podobny do przedstawionego poniżej.</span><span class="sxs-lookup"><span data-stu-id="ed95e-141">In design mode, drag controls and validators from the toolbox and build a form like the one below.</span></span>

![](tailspin-spyworks-part-7/_static/image2.jpg)

<span data-ttu-id="ed95e-142">Oznakowanie będzie wyglądać podobnie do tego.</span><span class="sxs-lookup"><span data-stu-id="ed95e-142">The markup will look something like this.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample8.aspx)]

<span data-ttu-id="ed95e-143">Teraz, gdy możemy wprowadzić przeglądy, umożliwia wyświetlenie tych przeglądów na stronie produkt.</span><span class="sxs-lookup"><span data-stu-id="ed95e-143">Now that we can enter reviews, lets display those reviews on the product page.</span></span>

<span data-ttu-id="ed95e-144">Dodaj tę adiustację do strony ProductDetails. aspx.</span><span class="sxs-lookup"><span data-stu-id="ed95e-144">Add this markup to the ProductDetails.aspx page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample9.aspx)]

<span data-ttu-id="ed95e-145">Uruchomienie naszej aplikacji teraz i przechodzenie do produktu powoduje wyświetlenie informacji o produkcie, w tym przeglądów klientów.</span><span class="sxs-lookup"><span data-stu-id="ed95e-145">Running our application now and navigating to a product shows the product information including customer reviews.</span></span>

![](tailspin-spyworks-part-7/_static/image3.jpg)

## <a id="_Toc260221678"></a><span data-ttu-id="ed95e-146">Kontrolka popularne elementy (Tworzenie formantów użytkownika)</span><span class="sxs-lookup"><span data-stu-id="ed95e-146">Popular Items Control (Creating User Controls)</span></span>

<span data-ttu-id="ed95e-147">Aby zwiększyć sprzedaż w witrynie sieci Web, dodamy kilka funkcji do popularnych lub powiązanych produktów "z sugestiami".</span><span class="sxs-lookup"><span data-stu-id="ed95e-147">In order to increase sales on your web site we will add a couple of features to "suggestive sell" popular or related products.</span></span>

<span data-ttu-id="ed95e-148">Pierwszy z tych funkcji będzie listą bardziej popularnego produktu w naszym katalogu produktów.</span><span class="sxs-lookup"><span data-stu-id="ed95e-148">The first of these features will be a list of the more popular product in our product catalog.</span></span>

<span data-ttu-id="ed95e-149">Utworzymy "kontrolkę użytkownika" w celu wyświetlenia najważniejszych elementów sprzedaży na stronie głównej naszej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ed95e-149">We will create a "User Control" to display the top selling items on the home page of our application.</span></span> <span data-ttu-id="ed95e-150">Ponieważ będzie to formant, można go używać na dowolnej stronie przez przeciąganie i upuszczanie kontrolki w projektancie programu Visual Studio na dowolną stronę, którą lubimy.</span><span class="sxs-lookup"><span data-stu-id="ed95e-150">Since this will be a control, we can use it on any page by simply dragging and dropping the control in Visual Studio's designer onto any page that we like.</span></span>

<span data-ttu-id="ed95e-151">W Eksploratorze rozwiązań programu Visual Studio kliknij prawym przyciskiem myszy nazwę rozwiązania i Utwórz nowy katalog o nazwie "Controls" (kontrolki).</span><span class="sxs-lookup"><span data-stu-id="ed95e-151">In Visual Studio's solutions explorer, right-click on the solution name and create a new directory named "Controls".</span></span> <span data-ttu-id="ed95e-152">Chociaż nie jest to konieczne, pomożemy nam zorganizować projekt, tworząc wszystkie formanty użytkownika w katalogu "Controls".</span><span class="sxs-lookup"><span data-stu-id="ed95e-152">While it is not necessary to do so, we will help keep our project organized by creating all our user controls in the "Controls" directory.</span></span>

<span data-ttu-id="ed95e-153">Kliknij prawym przyciskiem myszy folder Controls i wybierz pozycję "nowy element":</span><span class="sxs-lookup"><span data-stu-id="ed95e-153">Right-click on the controls folder and choose "New Item" :</span></span>

![](tailspin-spyworks-part-7/_static/image4.jpg)

<span data-ttu-id="ed95e-154">Określ nazwę naszej kontroli "PopularItems".</span><span class="sxs-lookup"><span data-stu-id="ed95e-154">Specify a name for our control of "PopularItems".</span></span> <span data-ttu-id="ed95e-155">Należy zauważyć, że rozszerzenie pliku dla kontrolek użytkownika to. ascx not. aspx.</span><span class="sxs-lookup"><span data-stu-id="ed95e-155">Note that the file extension for user controls is .ascx not .aspx.</span></span>

<span data-ttu-id="ed95e-156">Nasze popularne elementy formant użytkownika zostanie zdefiniowany w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="ed95e-156">Our Popular Items User control will be defined as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample10.aspx)]

<span data-ttu-id="ed95e-157">W tym miejscu korzystamy z metody, która nie została jeszcze użyta w tej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ed95e-157">Here we're using a method we have not used yet in this application.</span></span> <span data-ttu-id="ed95e-158">Używamy formantu wzmacniania, a nie za pomocą kontroli źródła danych, aby powiązać formant wzmacnia z wynikami zapytania LINQ to Entitiesowego.</span><span class="sxs-lookup"><span data-stu-id="ed95e-158">We're using the repeater control and instead of using a data source control we're binding the Repeater Control to the results of a LINQ to Entities query.</span></span>

<span data-ttu-id="ed95e-159">W kodzie związanym z naszym formantem Wykonujemy poniższe czynności.</span><span class="sxs-lookup"><span data-stu-id="ed95e-159">In the code behind of our control we do that as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample11.cs)]

<span data-ttu-id="ed95e-160">Pamiętaj również, że ten ważny wiersz znajduje się w górnej części znacznika kontrolki.</span><span class="sxs-lookup"><span data-stu-id="ed95e-160">Note also this important line at the top of our control's markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample12.aspx)]

<span data-ttu-id="ed95e-161">Ze względu na to, że najpopularniejsze elementy nie zostaną zmienione na minutę, możemy dodać dyrektywę aching, aby zwiększyć wydajność naszej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ed95e-161">Since the most popular items won't be changing on a minute to minute basis we can add a aching directive to improve the performance of our application.</span></span> <span data-ttu-id="ed95e-162">Ta dyrektywa spowoduje, że kod kontrolny zostanie wykonany tylko po wygaśnięciu zbuforowanego danych wyjściowych formantu.</span><span class="sxs-lookup"><span data-stu-id="ed95e-162">This directive will cause the controls code to only be executed when the cached output of the control expires.</span></span> <span data-ttu-id="ed95e-163">W przeciwnym razie zostanie użyta buforowana wersja danych wyjściowych kontrolki.</span><span class="sxs-lookup"><span data-stu-id="ed95e-163">Otherwise, the cached version of the control's output will be used.</span></span>

<span data-ttu-id="ed95e-164">Teraz wszystko, co należy zrobić, obejmuje nasz nowy formant na stronie Default. aspx.</span><span class="sxs-lookup"><span data-stu-id="ed95e-164">Now all we have to do is include our new control in our Default.aspx page.</span></span>

<span data-ttu-id="ed95e-165">Użyj przeciągania i upuszczania, aby umieścić wystąpienie kontrolki w kolumnie Otwórz w naszym formularzu domyślnym.</span><span class="sxs-lookup"><span data-stu-id="ed95e-165">Use drag and drop to place an instance of the control in the open column of our Default form.</span></span>

![](tailspin-spyworks-part-7/_static/image5.jpg)

<span data-ttu-id="ed95e-166">Teraz podczas uruchamiania naszej aplikacji na stronie głównej są wyświetlane najbardziej popularne elementy.</span><span class="sxs-lookup"><span data-stu-id="ed95e-166">Now when we run our application the home page displays the most popular items.</span></span>

![](tailspin-spyworks-part-7/_static/image6.jpg)

## <a id="_Toc260221679"></a><span data-ttu-id="ed95e-167">Kontrolka "kupione również" (kontrolki użytkownika z parametrami)</span><span class="sxs-lookup"><span data-stu-id="ed95e-167">"Also Purchased" Control (User Controls with Parameters)</span></span>

<span data-ttu-id="ed95e-168">Druga kontrolka użytkownika, którą utworzymy, zajmie się sugerowanym sprzedażą do następnego poziomu, dodając specyficzność kontekstu.</span><span class="sxs-lookup"><span data-stu-id="ed95e-168">The second User Control that we'll create will take suggestive selling to the next level by adding context specificity.</span></span>

<span data-ttu-id="ed95e-169">Logika służąca do obliczania elementów Top "zakupionych" jest nieprosta.</span><span class="sxs-lookup"><span data-stu-id="ed95e-169">The logic to calculate the top "Also Purchased" items is non-trivial.</span></span>

<span data-ttu-id="ed95e-170">Nasz "zakupiony" formant wybierze rekordy OrderDetails (wcześniej zakupione) dla aktualnie wybranego identyfikatora ProductID i odnalezienie identyfikatorów unikatowych dla każdego unikatowego zamówienia.</span><span class="sxs-lookup"><span data-stu-id="ed95e-170">Our "Also Purchased" control will select the OrderDetails records (previously purchased) for the currently selected ProductID and grab the OrderIDs for each unique order that is found.</span></span>

<span data-ttu-id="ed95e-171">Następnie wybierzemy pozycję Al produkty ze wszystkich tych zamówień i zasumujesz zakupione ilości.</span><span class="sxs-lookup"><span data-stu-id="ed95e-171">Then we will select al the products from all those Orders and sum the quantities purchased.</span></span> <span data-ttu-id="ed95e-172">Posortujemy produkty według tej ilości sum i wyświetlamy pięć pierwszych elementów.</span><span class="sxs-lookup"><span data-stu-id="ed95e-172">We'll sort the products by that quantity sum and display the top five items.</span></span>

<span data-ttu-id="ed95e-173">Ze względu na złożoność tej logiki zaimplementujmy ten algorytm jako procedurę składowaną.</span><span class="sxs-lookup"><span data-stu-id="ed95e-173">Given the complexity of this logic, we will implement this algorithm as a stored procedure.</span></span>

<span data-ttu-id="ed95e-174">Język T-SQL dla procedury składowanej jest następujący:</span><span class="sxs-lookup"><span data-stu-id="ed95e-174">The T-SQL for the stored procedure is as follows.</span></span>

[!code-sql[Main](tailspin-spyworks-part-7/samples/sample13.sql)]

<span data-ttu-id="ed95e-175">Należy zauważyć, że ta procedura składowana (SelectPurchasedWithProducts) istniała w bazie danych, gdy została ona uwzględniona w naszej aplikacji, a w przypadku wygenerowania Entity Data Model określić, że oprócz tabel i widoków, które są niezbędne, Entity Data Model powinna zawierać tę procedurę składowaną.</span><span class="sxs-lookup"><span data-stu-id="ed95e-175">Note that this stored procedure (SelectPurchasedWithProducts) existed in the database when we included it in our application and when we generated the Entity Data Model we specified that, in addition to the Tables and Views that we needed, the Entity Data Model should include this stored procedure.</span></span>

<span data-ttu-id="ed95e-176">Aby uzyskać dostęp do procedury składowanej z Entity Data Model musimy zaimportować funkcję.</span><span class="sxs-lookup"><span data-stu-id="ed95e-176">To access the stored procedure from the Entity Data Model we need to import the function.</span></span>

<span data-ttu-id="ed95e-177">Kliknij dwukrotnie Entity Data Model w Eksploratorze rozwiązań, aby otworzyć go w Projektancie i otworzyć przeglądarkę modelu, a następnie kliknij prawym przyciskiem myszy w Projektancie i wybierz pozycję "Dodaj Import funkcji".</span><span class="sxs-lookup"><span data-stu-id="ed95e-177">Double Click on the Entity Data Model in the Solutions Explorer to open it in the designer and open the Model Browser, then right-click in the designer and select "Add Function Import".</span></span>

![](tailspin-spyworks-part-7/_static/image1.png)

<span data-ttu-id="ed95e-178">Spowoduje to otwarcie tego okna dialogowego.</span><span class="sxs-lookup"><span data-stu-id="ed95e-178">Doing so will open this dialog.</span></span>

![](tailspin-spyworks-part-7/_static/image2.png)

<span data-ttu-id="ed95e-179">Wypełnij pola, jak widać powyżej, wybierając "SelectPurchasedWithProducts" i użyj nazwy procedury dla nazwy naszej funkcji zaimportowanej.</span><span class="sxs-lookup"><span data-stu-id="ed95e-179">Fill out the fields as you see above, selecting the "SelectPurchasedWithProducts" and use the procedure name for the name of our imported function.</span></span>

<span data-ttu-id="ed95e-180">Kliknij przycisk "OK".</span><span class="sxs-lookup"><span data-stu-id="ed95e-180">Click "Ok".</span></span>

<span data-ttu-id="ed95e-181">Po wykonaniu tej czynności można po prostu programować w odniesieniu do procedury składowanej, ponieważ możemy użyć dowolnego innego elementu w modelu.</span><span class="sxs-lookup"><span data-stu-id="ed95e-181">Having done this we can simply program against the stored procedure as we might any other item in the model.</span></span>

<span data-ttu-id="ed95e-182">W naszym folderze "Controls" Utwórz nową kontrolkę użytkownika o nazwie AlsoPurchased. ascx.</span><span class="sxs-lookup"><span data-stu-id="ed95e-182">So, in our "Controls" folder create a new user control named AlsoPurchased.ascx.</span></span>

<span data-ttu-id="ed95e-183">Znaczniki dla tej kontrolki będą wyglądały na bardzo zaznajomione z kontrolką PopularItems.</span><span class="sxs-lookup"><span data-stu-id="ed95e-183">The markup for this control will look very familiar to the PopularItems control.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample14.aspx)]

<span data-ttu-id="ed95e-184">Istotną różnicą jest to, że nie buforowanie danych wyjściowych, ponieważ element, który ma być renderowany, będzie różnić się w zależności od produktu.</span><span class="sxs-lookup"><span data-stu-id="ed95e-184">The notable difference is that are not caching the output since the item's to be rendered will differ by product.</span></span>

<span data-ttu-id="ed95e-185">Identyfikator ProductId będzie "właściwości" kontrolki.</span><span class="sxs-lookup"><span data-stu-id="ed95e-185">The ProductId will be a "property" to the control.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample15.cs)]

<span data-ttu-id="ed95e-186">W programie obsługi zdarzeń zdarzenia PreRender EED do wykonania trzy rzeczy.</span><span class="sxs-lookup"><span data-stu-id="ed95e-186">In the control's PreRender event handler we eed to do three things.</span></span>

1. <span data-ttu-id="ed95e-187">Upewnij się, że identyfikator ProductID jest ustawiony.</span><span class="sxs-lookup"><span data-stu-id="ed95e-187">Make sure the ProductID is set.</span></span>
2. <span data-ttu-id="ed95e-188">Sprawdź, czy istnieją jakieś produkty, które zostały zakupione w ramach bieżącego.</span><span class="sxs-lookup"><span data-stu-id="ed95e-188">See if there are any products that have been purchased with the current one.</span></span>
3. <span data-ttu-id="ed95e-189">Wyprowadzanie niektórych elementów jako określonych w #2.</span><span class="sxs-lookup"><span data-stu-id="ed95e-189">Output some items as determined in #2.</span></span>

<span data-ttu-id="ed95e-190">Zwróć uwagę na to, jak łatwo jest wywołać procedurę składowaną za pomocą modelu.</span><span class="sxs-lookup"><span data-stu-id="ed95e-190">Note how easy it is to call the stored procedure through the model.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample16.cs)]

<span data-ttu-id="ed95e-191">Po ustaleniu, że istnieje również "zakup", można po prostu powiązać wzmacniak z wynikami zwróconymi przez zapytanie.</span><span class="sxs-lookup"><span data-stu-id="ed95e-191">After determining that there ARE "also purchased" we can simply bind the repeater to the results returned by the query.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample17.cs)]

<span data-ttu-id="ed95e-192">Jeśli nie zostały jeszcze zakupione elementy, po prostu wyświetlamy inne popularne elementy z naszego katalogu.</span><span class="sxs-lookup"><span data-stu-id="ed95e-192">If there were not any "also purchased" items we'll simply display other popular items from our catalog.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample18.cs)]

<span data-ttu-id="ed95e-193">Aby wyświetlić elementy "kupione również", Otwórz stronę ProductDetails. aspx i przeciągnij kontrolkę AlsoPurchased z Eksploratora rozwiązań, aby była wyświetlana w tym miejscu w znaczniku.</span><span class="sxs-lookup"><span data-stu-id="ed95e-193">To view the "Also Purchased" items, open the ProductDetails.aspx page and drag the AlsoPurchased control from the Solutions Explorer so that it appears in this position in the markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample19.aspx)]

<span data-ttu-id="ed95e-194">Spowoduje to utworzenie odwołania do kontrolki w górnej części strony ProductDetails.</span><span class="sxs-lookup"><span data-stu-id="ed95e-194">Doing so will create a reference to the control at the top of the ProductDetails page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample20.aspx)]

<span data-ttu-id="ed95e-195">Ponieważ kontrolka użytkownika AlsoPurchased wymaga numeru ProductId, ustawimy Właściwość ProductID formantu przy użyciu instrukcji eval względem bieżącego elementu modelu danych strony.</span><span class="sxs-lookup"><span data-stu-id="ed95e-195">Since the AlsoPurchased user control requires a ProductId number we will set the ProductID property of our control by using an Eval statement against the current data model item of the page.</span></span>

![](tailspin-spyworks-part-7/_static/image3.png)

<span data-ttu-id="ed95e-196">Po skompilowaniu i uruchomieniu teraz i przejściu do produktu zobaczysz "kupione również elementy".</span><span class="sxs-lookup"><span data-stu-id="ed95e-196">When we build and run now and browse to a product we see the "Also Purchased" items.</span></span>

![](tailspin-spyworks-part-7/_static/image7.jpg)

> [!div class="step-by-step"]
> <span data-ttu-id="ed95e-197">[Poprzednie](tailspin-spyworks-part-6.md)
> [dalej](tailspin-spyworks-part-8.md)</span><span class="sxs-lookup"><span data-stu-id="ed95e-197">[Previous](tailspin-spyworks-part-6.md)
[Next](tailspin-spyworks-part-8.md)</span></span>

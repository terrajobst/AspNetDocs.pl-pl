---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
title: 'Część 5: edytowanie formularzy i tworzenia szablonów | Microsoft Docs'
author: jongalloway
description: Ta seria samouczków zawiera szczegółowe informacje na temat wszystkich kroków podjętych w celu skompilowania przykładowej aplikacji do sklepu ASP.NET MVC Music. Część 5 obejmuje edycję formularzy i tworzenia szablonów.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 6b09413a-6d6a-425a-87c9-629f91b91b28
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
msc.type: authoredcontent
ms.openlocfilehash: 20b99cbe57b5dfa623205838a5929733a6c2d70d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78559550"
---
# <a name="part-5-edit-forms-and-templating"></a><span data-ttu-id="2ed22-104">Część 5. Edycja formularzy i tworzenie szablonów</span><span class="sxs-lookup"><span data-stu-id="2ed22-104">Part 5: Edit Forms and Templating</span></span>

<span data-ttu-id="2ed22-105">przez [Jan Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="2ed22-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="2ed22-106">Sklep MVC Music jest aplikacją samouczka, która wprowadza i objaśnia krok po kroku, jak używać ASP.NET MVC i Visual Studio do programowania w sieci Web.</span><span class="sxs-lookup"><span data-stu-id="2ed22-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="2ed22-107">Sklep MVC Music jest lekkim przykładowym wdrożeniem magazynu, który sprzedaje Albumy muzyczne w trybie online i implementuje podstawowe funkcje administracyjne, logowania użytkownika i koszyka.</span><span class="sxs-lookup"><span data-stu-id="2ed22-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>
> 
> <span data-ttu-id="2ed22-108">Ta seria samouczków zawiera szczegółowe informacje na temat wszystkich kroków podjętych w celu skompilowania przykładowej aplikacji do sklepu ASP.NET MVC Music.</span><span class="sxs-lookup"><span data-stu-id="2ed22-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="2ed22-109">Część 5 obejmuje edycję formularzy i tworzenia szablonów.</span><span class="sxs-lookup"><span data-stu-id="2ed22-109">Part 5 covers Edit Forms and Templating.</span></span>

<span data-ttu-id="2ed22-110">W ostatnim rozdziale załadujemy dane z naszej bazy danych i ich wyświetlanie.</span><span class="sxs-lookup"><span data-stu-id="2ed22-110">In the past chapter, we were loading data from our database and displaying it.</span></span> <span data-ttu-id="2ed22-111">W tym rozdziale umożliwimy również edytowanie danych.</span><span class="sxs-lookup"><span data-stu-id="2ed22-111">In this chapter, we'll also enable editing the data.</span></span>

## <a name="creating-the-storemanagercontroller"></a><span data-ttu-id="2ed22-112">Tworzenie StoreManagerController</span><span class="sxs-lookup"><span data-stu-id="2ed22-112">Creating the StoreManagerController</span></span>

<span data-ttu-id="2ed22-113">Zaczniemy od utworzenia nowego kontrolera o nazwie **StoreManagerController**.</span><span class="sxs-lookup"><span data-stu-id="2ed22-113">We'll begin by creating a new controller called **StoreManagerController**.</span></span> <span data-ttu-id="2ed22-114">W przypadku tego kontrolera będziemy korzystać z funkcji tworzenia szkieletów dostępnych w ramach aktualizacji narzędzi ASP.NET MVC 3.</span><span class="sxs-lookup"><span data-stu-id="2ed22-114">For this controller, we will be taking advantage of the Scaffolding features available in the ASP.NET MVC 3 Tools Update.</span></span> <span data-ttu-id="2ed22-115">Ustaw opcje okna dialogowego Dodaj kontroler, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="2ed22-115">Set the options for the Add Controller dialog as shown below.</span></span>

![](mvc-music-store-part-5/_static/image1.png)

<span data-ttu-id="2ed22-116">Po kliknięciu przycisku Dodaj zobaczysz, że mechanizm tworzenia szkieletu ASP.NET MVC 3 wykonuje dobrą ilość pracy:</span><span class="sxs-lookup"><span data-stu-id="2ed22-116">When you click the Add button, you'll see that the ASP.NET MVC 3 scaffolding mechanism does a good amount of work for you:</span></span>

- <span data-ttu-id="2ed22-117">Tworzy nowy StoreManagerController z lokalną zmienną Entity Framework</span><span class="sxs-lookup"><span data-stu-id="2ed22-117">It creates the new StoreManagerController with a local Entity Framework variable</span></span>
- <span data-ttu-id="2ed22-118">Dodaje folder Storemanager do folderu widoków projektu</span><span class="sxs-lookup"><span data-stu-id="2ed22-118">It adds a StoreManager folder to the project's Views folder</span></span>
- <span data-ttu-id="2ed22-119">Dodaje do klasy albumu polecenie CREATE. cshtml, DELETE. cshtml, details. cshtml, Edit. cshtml i index. cshtml.</span><span class="sxs-lookup"><span data-stu-id="2ed22-119">It adds Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml, and Index.cshtml view, strongly typed to the Album class</span></span>

![](mvc-music-store-part-5/_static/image2.png)

<span data-ttu-id="2ed22-120">Nowa klasa kontrolera Magazynumanager obejmuje akcje kontrolera CRUD (tworzenie, odczytywanie, aktualizowanie, usuwanie), które wiedzą, jak współpracować z klasą modelu albumu i korzystać z naszego kontekstu Entity Framework do uzyskiwania dostępu do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="2ed22-120">The new StoreManager controller class includes CRUD (create, read, update, delete) controller actions which know how to work with the Album model class and use our Entity Framework context for database access.</span></span>

## <a name="modifying-a-scaffolded-view"></a><span data-ttu-id="2ed22-121">Modyfikowanie widoku szkieletowego</span><span class="sxs-lookup"><span data-stu-id="2ed22-121">Modifying a Scaffolded View</span></span>

<span data-ttu-id="2ed22-122">Należy pamiętać, że podczas gdy ten kod został wygenerowany dla nas, jest to standardowy kod ASP.NET MVC, podobnie jak w przypadku pisania w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="2ed22-122">It's important to remember that, while this code was generated for us, it's standard ASP.NET MVC code, just like we've been writing throughout this tutorial.</span></span> <span data-ttu-id="2ed22-123">Zachodzi konieczność zaoszczędzenia czasu poświęcanego na pisanie kodu standardowego kontrolera i ręczne utworzenie widoków o jednoznacznie określonym typie, ale nie jest to rodzaj wygenerowanego kodu, który może zostać wcześniej wyświetlony z ostrzeżeniami fatalne w komentarzach dotyczących sposobu nie może zmiany kodu.</span><span class="sxs-lookup"><span data-stu-id="2ed22-123">It's intended to save you the time you'd spend on writing boilerplate controller code and creating the strongly typed views manually, but this isn't the kind of generated code you may have seen prefaced with dire warnings in comments about how you mustn't change the code.</span></span> <span data-ttu-id="2ed22-124">Jest to Twój kod, który powinien zostać zmieniony.</span><span class="sxs-lookup"><span data-stu-id="2ed22-124">This is your code, and you're expected to change it.</span></span>

<span data-ttu-id="2ed22-125">Zacznijmy od szybkiej edycji do widoku indeksu Magazynumanager (/Views/StoreManager/Index.cshtml).</span><span class="sxs-lookup"><span data-stu-id="2ed22-125">So, let's start with a quick edit to the StoreManager Index view (/Views/StoreManager/Index.cshtml).</span></span> <span data-ttu-id="2ed22-126">W tym widoku zostanie wyświetlona tabela zawierająca listę albumów w naszym sklepie z linkami Edytuj/szczegóły/Usuń i zawiera właściwości publiczne albumu.</span><span class="sxs-lookup"><span data-stu-id="2ed22-126">This view will display a table which lists the Albums in our store with Edit / Details / Delete links, and includes the Album's public properties.</span></span> <span data-ttu-id="2ed22-127">Usuniemy pole AlbumArtUrl, ponieważ nie jest ono bardzo przydatne w przypadku tego ekranu.</span><span class="sxs-lookup"><span data-stu-id="2ed22-127">We'll remove the AlbumArtUrl field, as it's not very useful in this display.</span></span> <span data-ttu-id="2ed22-128">W sekcji &lt;tabeli&gt; w kodzie widoku Usuń &lt;tej&gt; i &lt;TD&gt; elementy otaczające odwołania AlbumArtUrl, jak wskazano w wyróżnionych wierszach poniżej:</span><span class="sxs-lookup"><span data-stu-id="2ed22-128">In &lt;table&gt; section of the view code, remove the &lt;th&gt; and &lt;td&gt; elements surrounding AlbumArtUrl references, as indicated by the highlighted lines below:</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample1.cshtml)]

<span data-ttu-id="2ed22-129">Zmodyfikowany kod widoku zostanie wyświetlony w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="2ed22-129">The modified view code will appear as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample2.cshtml)]

## <a name="a-first-look-at-the-store-manager"></a><span data-ttu-id="2ed22-130">Pierwsze spojrzenie na Menedżera sklepu</span><span class="sxs-lookup"><span data-stu-id="2ed22-130">A first look at the Store Manager</span></span>

<span data-ttu-id="2ed22-131">Teraz uruchom aplikację i przejdź do/StoreManager/.</span><span class="sxs-lookup"><span data-stu-id="2ed22-131">Now run the application and browse to /StoreManager/.</span></span> <span data-ttu-id="2ed22-132">Spowoduje to wyświetlenie indeksu Menedżera sklepu, który został właśnie zmodyfikowany, pokazującego listę albumów w sklepie z linkami do edycji, szczegółów i usuwania.</span><span class="sxs-lookup"><span data-stu-id="2ed22-132">This displays the Store Manager Index we just modified, showing a list of the albums in the store with links to Edit, Details, and Delete.</span></span>

![](mvc-music-store-part-5/_static/image3.png)

<span data-ttu-id="2ed22-133">Kliknięcie linku edycji wyświetla formularz edycji z polami dla albumu, włącznie z listami rozwijanymi dla gatunku i wykonawcy.</span><span class="sxs-lookup"><span data-stu-id="2ed22-133">Clicking the Edit link displays an edit form with fields for the Album, including dropdowns for Genre and Artist.</span></span>

![](mvc-music-store-part-5/_static/image4.png)

<span data-ttu-id="2ed22-134">Kliknij link "z powrotem do listy" u dołu, a następnie kliknij link szczegóły dla albumu.</span><span class="sxs-lookup"><span data-stu-id="2ed22-134">Click the "Back to List" link at the bottom, then click on the Details link for an Album.</span></span> <span data-ttu-id="2ed22-135">Spowoduje to wyświetlenie szczegółowych informacji o pojedynczym albumie.</span><span class="sxs-lookup"><span data-stu-id="2ed22-135">This displays the detail information for an individual Album.</span></span>

![](mvc-music-store-part-5/_static/image5.png)

<span data-ttu-id="2ed22-136">Ponownie kliknij link Wróć do listy, a następnie kliknij link Usuń.</span><span class="sxs-lookup"><span data-stu-id="2ed22-136">Again, click the Back to List link, then click on a Delete link.</span></span> <span data-ttu-id="2ed22-137">Spowoduje to wyświetlenie okna dialogowego potwierdzenia, pokazującego szczegóły dotyczące albumu i pytanie, czy na pewno chcemy go usunąć.</span><span class="sxs-lookup"><span data-stu-id="2ed22-137">This displays a confirmation dialog, showing the album details and asking if we're sure we want to delete it.</span></span>

![](mvc-music-store-part-5/_static/image6.png)

<span data-ttu-id="2ed22-138">Kliknięcie przycisku Usuń u dołu spowoduje usunięcie albumu i powrót do strony indeks, która pokazuje, że album został usunięty.</span><span class="sxs-lookup"><span data-stu-id="2ed22-138">Clicking the Delete button at the bottom will delete the album and return you to the Index page, which shows the album deleted.</span></span>

<span data-ttu-id="2ed22-139">Nie zakończymy pracy z menedżerem sklepu, ale mamy do uruchomienia kontroler roboczy i kod wyświetlania CRUD operacji.</span><span class="sxs-lookup"><span data-stu-id="2ed22-139">We're not done with the Store Manager, but we have working controller and view code for the CRUD operations to start from.</span></span>

## <a name="looking-at-the-store-manager-controller-code"></a><span data-ttu-id="2ed22-140">Spojrzenie na kod kontrolera Menedżera sklepu</span><span class="sxs-lookup"><span data-stu-id="2ed22-140">Looking at the Store Manager Controller code</span></span>

<span data-ttu-id="2ed22-141">Kontroler Menedżera sklepu zawiera dobrą ilość kodu.</span><span class="sxs-lookup"><span data-stu-id="2ed22-141">The Store Manager Controller contains a good amount of code.</span></span> <span data-ttu-id="2ed22-142">Zacznijmy od góry do dołu.</span><span class="sxs-lookup"><span data-stu-id="2ed22-142">Let's go through this from top to bottom.</span></span> <span data-ttu-id="2ed22-143">Kontroler zawiera kilka standardowych przestrzeni nazw dla kontrolera MVC, a także odwołanie do naszych nazw modeli.</span><span class="sxs-lookup"><span data-stu-id="2ed22-143">The controller includes some standard namespaces for an MVC controller, as well as a reference to our Models namespace.</span></span> <span data-ttu-id="2ed22-144">Kontroler ma prywatne wystąpienie MusicStoreEntities używane przez poszczególne akcje kontrolera na potrzeby dostępu do danych.</span><span class="sxs-lookup"><span data-stu-id="2ed22-144">The controller has a private instance of MusicStoreEntities, used by each of the controller actions for data access.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample3.cs)]

### <a name="store-manager-index-and-details-actions"></a><span data-ttu-id="2ed22-145">Akcje dotyczące indeksu i szczegółów Menedżera sklepu</span><span class="sxs-lookup"><span data-stu-id="2ed22-145">Store Manager Index and Details actions</span></span>

<span data-ttu-id="2ed22-146">Widok indeksu pobiera listę albumów, w tym informacje o gatunku i wykonawcach z odwołaniami do poszczególnych albumów, ponieważ zostały one wcześniej wyświetlone podczas pracy nad metodą przeglądania w sklepie.</span><span class="sxs-lookup"><span data-stu-id="2ed22-146">The index view retrieves a list of Albums, including each album's referenced Genre and Artist information, as we previously saw when working on the Store Browse method.</span></span> <span data-ttu-id="2ed22-147">Widok indeksu jest następujący po odwołaniach do połączonych obiektów, aby można było wyświetlić nazwę gatunku każdego albumu i nazwę wykonawcy, aby kontroler był wydajny i badał zapytania dotyczące tych informacji w oryginalnym żądaniu.</span><span class="sxs-lookup"><span data-stu-id="2ed22-147">The Index view is following the references to the linked objects so that it can display each album's Genre name and Artist name, so the controller is being efficient and querying for this information in the original request.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample4.cs)]

<span data-ttu-id="2ed22-148">Akcja kontrolera szczegółów kontrolera Sklepumanager działa dokładnie tak samo jak akcja szczegóły kontrolera sklepu, która została wcześniej zapisana — wykonuje zapytanie dotyczące albumu według identyfikatora przy użyciu metody Find (), a następnie zwraca je do widoku.</span><span class="sxs-lookup"><span data-stu-id="2ed22-148">The StoreManager Controller's Details controller action works exactly the same as the Store Controller Details action we wrote previously - it queries for the Album by ID using the Find() method, then returns it to the view.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample5.cs)]

### <a name="the-create-action-methods"></a><span data-ttu-id="2ed22-149">Metody tworzenia akcji</span><span class="sxs-lookup"><span data-stu-id="2ed22-149">The Create Action Methods</span></span>

<span data-ttu-id="2ed22-150">Metody tworzenia akcji są nieco inne niż te, które były już widoczne, ponieważ obsługują dane wejściowe formularza.</span><span class="sxs-lookup"><span data-stu-id="2ed22-150">The Create action methods are a little different from ones we've seen so far, because they handle form input.</span></span> <span data-ttu-id="2ed22-151">Gdy użytkownik najpierw odwiedzi/StoreManager/Create/, będzie wyświetlany pusty formularz.</span><span class="sxs-lookup"><span data-stu-id="2ed22-151">When a user first visits /StoreManager/Create/ they will be shown an empty form.</span></span> <span data-ttu-id="2ed22-152">Ta strona HTML będzie zawierać &lt;formularz&gt; element, który zawiera elementy listy rozwijanej i TextBox, gdzie mogą wprowadzać szczegóły albumu.</span><span class="sxs-lookup"><span data-stu-id="2ed22-152">This HTML page will contain a &lt;form&gt; element that contains dropdown and textbox input elements where they can enter the album's details.</span></span>

<span data-ttu-id="2ed22-153">Po wypełnieniu przez użytkownika wartości formularza albumu można nacisnąć przycisk "Save" (Zapisz), aby przesłać te zmiany z powrotem do naszej aplikacji w celu zapisania ich w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="2ed22-153">After the user fills in the Album form values, they can press the "Save" button to submit these changes back to our application to save within the database.</span></span> <span data-ttu-id="2ed22-154">Gdy użytkownik naciśnie przycisk "Save" (Zapisz),&gt; formularz &lt;spowoduje przeprowadzenie żądania HTTP zwrotnego w adresie URL/StoreManager/Create/i przesłanie &lt;&gt; wartości w ramach POST protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="2ed22-154">When the user presses the "save" button the &lt;form&gt; will perform an HTTP-POST back to the /StoreManager/Create/ URL and submit the &lt;form&gt; values as part of the HTTP-POST.</span></span>

<span data-ttu-id="2ed22-155">ASP.NET MVC umożliwia łatwe rozdzielenie logiki tych dwóch scenariuszy wywołania adresu URL przez umożliwienie nam implementacji dwóch oddzielnych metod akcji "Create" w naszej klasie StoreManagerController — jeden do obsługi początkowego protokołu HTTP-GET w przejściu do adresu URL/StoreManager/Create/, a drugi do obsługi POST protokołu HTTP dla przesłanych zmian.</span><span class="sxs-lookup"><span data-stu-id="2ed22-155">ASP.NET MVC allows us to easily split up the logic of these two URL invocation scenarios by enabling us to implement two separate "Create" action methods within our StoreManagerController class – one to handle the initial HTTP-GET browse to the /StoreManager/Create/ URL, and the other to handle the HTTP-POST of the submitted changes.</span></span>

### <a name="passing-information-to-a-view-using-viewbag"></a><span data-ttu-id="2ed22-156">Przekazywanie informacji do widoku przy użyciu ViewBag</span><span class="sxs-lookup"><span data-stu-id="2ed22-156">Passing information to a View using ViewBag</span></span>

<span data-ttu-id="2ed22-157">ViewBag wcześniej w tym samouczku, ale nie porozmawiasz z nim.</span><span class="sxs-lookup"><span data-stu-id="2ed22-157">We've used the ViewBag earlier in this tutorial, but haven't talked much about it.</span></span> <span data-ttu-id="2ed22-158">ViewBag umożliwia nam przekazywanie informacji do widoku bez użycia obiektu modelu silnie określonego typu.</span><span class="sxs-lookup"><span data-stu-id="2ed22-158">The ViewBag allows us to pass information to the view without using a strongly typed model object.</span></span> <span data-ttu-id="2ed22-159">W takim przypadku nasza akcja Edytuj kontroler HTTP-GET musi przekazywać listę gatunku i artystów do formularza w celu wypełnienia list rozwijanych, a Najprostszym sposobem, aby to zrobić, jako elementów ViewBag.</span><span class="sxs-lookup"><span data-stu-id="2ed22-159">In this case, our Edit HTTP-GET controller action needs to pass both a list of Genres and Artists to the form to populate the dropdowns, and the simplest way to do that is to return them as ViewBag items.</span></span>

<span data-ttu-id="2ed22-160">ViewBag jest obiektem dynamicznym, co oznacza, że można wpisać ViewBag. foo lub ViewBag. YourNameHere bez pisania kodu, aby zdefiniować te właściwości.</span><span class="sxs-lookup"><span data-stu-id="2ed22-160">The ViewBag is a dynamic object, meaning that you can type ViewBag.Foo or ViewBag.YourNameHere without writing code to define those properties.</span></span> <span data-ttu-id="2ed22-161">W takim przypadku kod kontrolera używa ViewBag. GenreId i ViewBag. ArtistId, tak aby wartości rozwijane przesłane z formularzem będą GenreId i ArtistId, które są właściwościami albumu, które będą ustawiane.</span><span class="sxs-lookup"><span data-stu-id="2ed22-161">In this case, the controller code uses ViewBag.GenreId and ViewBag.ArtistId so that the dropdown values submitted with the form will be GenreId and ArtistId, which are the Album properties they will be setting.</span></span>

<span data-ttu-id="2ed22-162">Te wartości rozwijane są zwracane do formularza przy użyciu obiektu SelectList, który jest zbudowany tylko do tego celu.</span><span class="sxs-lookup"><span data-stu-id="2ed22-162">These dropdown values are returned to the form using the SelectList object, which is built just for that purpose.</span></span> <span data-ttu-id="2ed22-163">Odbywa się to przy użyciu kodu w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="2ed22-163">This is done using code like this:</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample6.cs)]

<span data-ttu-id="2ed22-164">Jak widać w kodzie metody akcji, do utworzenia tego obiektu są używane trzy parametry:</span><span class="sxs-lookup"><span data-stu-id="2ed22-164">As you can see from the action method code, three parameters are being used to create this object:</span></span>

- <span data-ttu-id="2ed22-165">Lista elementów, które zostaną wyświetlone na liście rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="2ed22-165">The list of items the dropdown will be displaying.</span></span> <span data-ttu-id="2ed22-166">Zwróć uwagę, że nie jest to tylko ciąg — przekazujemy listę gatunku.</span><span class="sxs-lookup"><span data-stu-id="2ed22-166">Note that this isn't just a string - we're passing a list of Genres.</span></span>
- <span data-ttu-id="2ed22-167">Następny parametr, który jest przesyłany do SelectList jest wybraną wartością.</span><span class="sxs-lookup"><span data-stu-id="2ed22-167">The next parameter being passed to the SelectList is the Selected Value.</span></span> <span data-ttu-id="2ed22-168">W ten sposób SelectList wie, jak wstępnie wybrać element na liście.</span><span class="sxs-lookup"><span data-stu-id="2ed22-168">This how the SelectList knows how to pre-select an item in the list.</span></span> <span data-ttu-id="2ed22-169">Będzie to łatwiejsze do zrozumienia, gdy zobaczymy formularz edycji, który jest bardzo podobny.</span><span class="sxs-lookup"><span data-stu-id="2ed22-169">This will be easier to understand when we look at the Edit form, which is pretty similar.</span></span>
- <span data-ttu-id="2ed22-170">Ostatnim parametrem jest właściwość, która ma zostać wyświetlona.</span><span class="sxs-lookup"><span data-stu-id="2ed22-170">The final parameter is the property to be displayed.</span></span> <span data-ttu-id="2ed22-171">W tym przypadku wskazuje, że właściwość Genre.Name jest pokazywana użytkownikowi.</span><span class="sxs-lookup"><span data-stu-id="2ed22-171">In this case, this is indicating that the Genre.Name property is what will be shown to the user.</span></span>

<span data-ttu-id="2ed22-172">Z tego względu akcja HTTP-GET Create jest bardzo prosta — dwa SelectLists są dodawane do ViewBag i żaden obiekt modelu nie jest przekazano do formularza (ponieważ nie został jeszcze utworzony).</span><span class="sxs-lookup"><span data-stu-id="2ed22-172">With that in mind, then, the HTTP-GET Create action is pretty simple - two SelectLists are added to the ViewBag, and no model object is passed to the form (since it hasn't been created yet).</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample7.cs)]

### <a name="html-helpers-to-display-the-drop-downs-in-the-create-view"></a><span data-ttu-id="2ed22-173">Pomocników HTML do wyświetlania list rozwijanych w widoku tworzenia</span><span class="sxs-lookup"><span data-stu-id="2ed22-173">HTML Helpers to display the Drop Downs in the Create View</span></span>

<span data-ttu-id="2ed22-174">Ponieważ porozmawiamy o sposobie przekazywania wartości listy rozwijanej do widoku, przyjrzyjmy się widokowi, aby zobaczyć, jak te wartości są wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="2ed22-174">Since we've talked about how the drop down values are passed to the view, let's take a quick look at the view to see how those values are displayed.</span></span> <span data-ttu-id="2ed22-175">W widoku Kod (/Views/StoreManager/Create.cshtml) zobaczysz następujące wywołanie, aby wyświetlić listę rozwijaną gatunku.</span><span class="sxs-lookup"><span data-stu-id="2ed22-175">In the view code (/Views/StoreManager/Create.cshtml), you'll see the following call is made to display the Genre drop down.</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample8.cshtml)]

<span data-ttu-id="2ed22-176">Jest to znane jako pomocnik HTML — Metoda narzędziowa, która wykonuje typowe zadanie wyświetlania.</span><span class="sxs-lookup"><span data-stu-id="2ed22-176">This is known as an HTML Helper - a utility method which performs a common view task.</span></span> <span data-ttu-id="2ed22-177">Pomocniki HTML są bardzo przydatne w zachowaniu zwięzłego i czytelnego kodu widoku.</span><span class="sxs-lookup"><span data-stu-id="2ed22-177">HTML Helpers are very useful in keeping our view code concise and readable.</span></span> <span data-ttu-id="2ed22-178">Pomocnik html. DropDownList jest udostępniany przez ASP.NET MVC, ale w dalszej części będzie możliwe utworzenie własnych pomocników na potrzeby wyświetlania kodu, który zostanie ponownie użyty w naszej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2ed22-178">The Html.DropDownList helper is provided by ASP.NET MVC, but as we'll see later it's possible to create our own helpers for view code we'll reuse in our application.</span></span>

<span data-ttu-id="2ed22-179">Wywołanie html. DropDownList musi mieć pytania dotyczące dwóch rzeczy — miejsca, w którym ma zostać wyświetlona lista, i jaka wartość (jeśli istnieje) powinna być wstępnie wybrana.</span><span class="sxs-lookup"><span data-stu-id="2ed22-179">The Html.DropDownList call just needs to be told two things - where to get the list to display, and what value (if any) should be pre-selected.</span></span> <span data-ttu-id="2ed22-180">Pierwszy parametr, GenreId, informuje DropDownList o wyszukiwaniu wartości o nazwie GenreId w modelu lub ViewBag.</span><span class="sxs-lookup"><span data-stu-id="2ed22-180">The first parameter, GenreId, tells the DropDownList to look for a value named GenreId in either the model or ViewBag.</span></span> <span data-ttu-id="2ed22-181">Drugi parametr jest używany do wskazania wartości, która ma być wyświetlana jako początkowo wybrana na liście rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="2ed22-181">The second parameter is used to indicate the value to show as initially selected in the drop down list.</span></span> <span data-ttu-id="2ed22-182">Ponieważ ten formularz jest formularzem tworzenia, nie ma wartości, która ma zostać wybrana, a parametr String. Empty jest przenoszona.</span><span class="sxs-lookup"><span data-stu-id="2ed22-182">Since this form is a Create form, there's no value to be preselected and String.Empty is passed.</span></span>

### <a name="handling-the-posted-form-values"></a><span data-ttu-id="2ed22-183">Obsługa wartości opublikowanych formularzy</span><span class="sxs-lookup"><span data-stu-id="2ed22-183">Handling the Posted Form values</span></span>

<span data-ttu-id="2ed22-184">Jak opisano wcześniej, istnieją dwie metody akcji skojarzone z każdym formularzem.</span><span class="sxs-lookup"><span data-stu-id="2ed22-184">As we discussed before, there are two action methods associated with each form.</span></span> <span data-ttu-id="2ed22-185">Pierwszy obsługuje żądanie HTTP-GET i wyświetla formularz.</span><span class="sxs-lookup"><span data-stu-id="2ed22-185">The first handles the HTTP-GET request and displays the form.</span></span> <span data-ttu-id="2ed22-186">Drugi obsługuje żądanie HTTP-POST, które zawiera przesłane wartości formularza.</span><span class="sxs-lookup"><span data-stu-id="2ed22-186">The second handles the HTTP-POST request, which contains the submitted form values.</span></span> <span data-ttu-id="2ed22-187">Zauważ, że akcja kontrolera ma atrybut [HttpPost], który informuje ASP.NET MVC, że powinien on odpowiadać tylko na żądania HTTP-POST.</span><span class="sxs-lookup"><span data-stu-id="2ed22-187">Notice that controller action has an [HttpPost] attribute, which tells ASP.NET MVC that it should only respond to HTTP-POST requests.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample9.cs)]

<span data-ttu-id="2ed22-188">Ta akcja ma cztery obowiązki:</span><span class="sxs-lookup"><span data-stu-id="2ed22-188">This action has four responsibilities:</span></span>

- 1. <span data-ttu-id="2ed22-189">Odczytywanie wartości formularza</span><span class="sxs-lookup"><span data-stu-id="2ed22-189">Read the form values</span></span>
- 2. <span data-ttu-id="2ed22-190">Sprawdź, czy wartości formularza przechodzą wszystkie reguły walidacji</span><span class="sxs-lookup"><span data-stu-id="2ed22-190">Check if the form values pass any validation rules</span></span>
- 3. <span data-ttu-id="2ed22-191">Jeśli przesyłanie formularza jest prawidłowe, Zapisz dane i Wyświetl zaktualizowaną listę</span><span class="sxs-lookup"><span data-stu-id="2ed22-191">If the form submission is valid, save the data and display the updated list</span></span>
- 4. <span data-ttu-id="2ed22-192">Jeśli przesyłanie formularza jest nieprawidłowe, należy ponownie wyświetlić formularz z błędami walidacji</span><span class="sxs-lookup"><span data-stu-id="2ed22-192">If the form submission is not valid, redisplay the form with validation errors</span></span>

#### <a name="reading-form-values-with-model-binding"></a><span data-ttu-id="2ed22-193">Odczytywanie wartości formularza z powiązaniem modelu</span><span class="sxs-lookup"><span data-stu-id="2ed22-193">Reading Form Values with Model Binding</span></span>

<span data-ttu-id="2ed22-194">Akcja kontrolera przetwarza przesyłanie formularza, który zawiera wartości dla GenreId i ArtistId (z listy rozwijanej) oraz wartości pól TextBox dla tytułu, ceny i AlbumArtUrl.</span><span class="sxs-lookup"><span data-stu-id="2ed22-194">The controller action is processing a form submission that includes values for GenreId and ArtistId (from the drop down list) and textbox values for Title, Price, and AlbumArtUrl.</span></span> <span data-ttu-id="2ed22-195">Chociaż istnieje możliwość bezpośredniego dostępu do wartości formularzy, lepszym rozwiązaniem jest użycie funkcji powiązania modelu wbudowanych w ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="2ed22-195">While it's possible to directly access form values, a better approach is to use the Model Binding capabilities built into ASP.NET MVC.</span></span> <span data-ttu-id="2ed22-196">Gdy akcja kontrolera przyjmuje typ modelu jako parametr, ASP.NET MVC podejmie próbę wypełnienia obiektu tego typu przy użyciu danych wejściowych formularza (a także wartości trasy i QueryString).</span><span class="sxs-lookup"><span data-stu-id="2ed22-196">When a controller action takes a model type as a parameter, ASP.NET MVC will attempt to populate an object of that type using form inputs (as well as route and querystring values).</span></span> <span data-ttu-id="2ed22-197">Jest to możliwe dzięki szukaniu wartości, których nazwy pasują do właściwości obiektu modelu, np. podczas ustawiania wartości GenreId nowego obiektu albumu szuka danych wejściowych o nazwie GenreId.</span><span class="sxs-lookup"><span data-stu-id="2ed22-197">It does this by looking for values whose names match properties of the model object, e.g. when setting the new Album object's GenreId value, it looks for an input with the name GenreId.</span></span> <span data-ttu-id="2ed22-198">Podczas tworzenia widoków przy użyciu standardowych metod w ASP.NET MVC, formularze będą zawsze renderowane przy użyciu nazw właściwości jako nazw pól wejściowych, więc te nazwy pól będą się zgadzać.</span><span class="sxs-lookup"><span data-stu-id="2ed22-198">When you create views using the standard methods in ASP.NET MVC, the forms will always be rendered using property names as input field names, so this the field names will just match up.</span></span>

#### <a name="validating-the-model"></a><span data-ttu-id="2ed22-199">Sprawdzanie poprawności modelu</span><span class="sxs-lookup"><span data-stu-id="2ed22-199">Validating the Model</span></span>

<span data-ttu-id="2ed22-200">Model jest sprawdzany z prostym wywołaniem metody ModelState. IsValid.</span><span class="sxs-lookup"><span data-stu-id="2ed22-200">The model is validated with a simple call to ModelState.IsValid.</span></span> <span data-ttu-id="2ed22-201">Nie Dodaliśmy jeszcze żadnych reguł sprawdzania poprawności do naszej klasy albumu — wykonujemy tę czynność w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="2ed22-201">We haven't added any validation rules to our Album class yet - we'll do that in a bit - so right now this check doesn't have much to do.</span></span> <span data-ttu-id="2ed22-202">Ważne jest, aby to ModelStat. IsValid Check przywróci reguły sprawdzania poprawności, które zostały umieszczone w naszym modelu, więc przyszłe zmiany reguł walidacji nie będą wymagały żadnych aktualizacji kodu akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="2ed22-202">What's important is that this ModelStat.IsValid check will adapt to the validation rules we put on our model, so future changes to validation rules won't require any updates to the controller action code.</span></span>

#### <a name="saving-the-submitted-values"></a><span data-ttu-id="2ed22-203">Zapisywanie przesłanych wartości</span><span class="sxs-lookup"><span data-stu-id="2ed22-203">Saving the submitted values</span></span>

<span data-ttu-id="2ed22-204">Jeśli przesyłanie formularza przejdzie walidację, czas na zapisanie wartości w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="2ed22-204">If the form submission passes validation, it's time to save the values to the database.</span></span> <span data-ttu-id="2ed22-205">Mając Entity Framework, to właśnie wymaga dodania modelu do kolekcji albumy i wywołania metody SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="2ed22-205">With Entity Framework, that just requires adding the model to the Albums collection and calling SaveChanges.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample10.cs)]

<span data-ttu-id="2ed22-206">Entity Framework generuje odpowiednie polecenia SQL, aby zachować wartość.</span><span class="sxs-lookup"><span data-stu-id="2ed22-206">Entity Framework generates the appropriate SQL commands to persist the value.</span></span> <span data-ttu-id="2ed22-207">Po zapisaniu danych przekierowujemy z powrotem do listy albumów, aby zobaczyć naszą aktualizację.</span><span class="sxs-lookup"><span data-stu-id="2ed22-207">After saving the data, we redirect back to the list of Albums so we can see our update.</span></span> <span data-ttu-id="2ed22-208">Jest to realizowane przez zwrócenie RedirectToAction z nazwą akcji kontrolera, która ma zostać wyświetlona.</span><span class="sxs-lookup"><span data-stu-id="2ed22-208">This is done by returning RedirectToAction with the name of the controller action we want displayed.</span></span> <span data-ttu-id="2ed22-209">W tym przypadku jest to metoda index.</span><span class="sxs-lookup"><span data-stu-id="2ed22-209">In this case, that's the Index method.</span></span>

#### <a name="displaying-invalid-form-submissions-with-validation-errors"></a><span data-ttu-id="2ed22-210">Wyświetlanie nieprawidłowych przesłanych formularzy z błędami walidacji</span><span class="sxs-lookup"><span data-stu-id="2ed22-210">Displaying invalid form submissions with Validation Errors</span></span>

<span data-ttu-id="2ed22-211">W przypadku nieprawidłowych danych wejściowych formularza listy rozwijane są dodawane do ViewBag (podobnie jak w przypadku protokołu HTTP-GET), a powiązane wartości modelu są przesyłane z powrotem do widoku w celu wyświetlenia.</span><span class="sxs-lookup"><span data-stu-id="2ed22-211">In the case of invalid form input, the dropdown values are added to the ViewBag (as in the HTTP-GET case) and the bound model values are passed back to the view for display.</span></span> <span data-ttu-id="2ed22-212">Błędy walidacji są automatycznie wyświetlane przy użyciu pomocnika HTML @Html.ValidationMessageFor.</span><span class="sxs-lookup"><span data-stu-id="2ed22-212">Validation errors are automatically displayed using the @Html.ValidationMessageFor HTML Helper.</span></span>

#### <a name="testing-the-create-form"></a><span data-ttu-id="2ed22-213">Testowanie formularza tworzenia</span><span class="sxs-lookup"><span data-stu-id="2ed22-213">Testing the Create Form</span></span>

<span data-ttu-id="2ed22-214">Aby przetestować tę wartość, uruchom aplikację i przejdź do/StoreManager/Create/— spowoduje to wyświetlenie pustego formularza, który został zwrócony przez StoreController tworzenia HTTP-GET.</span><span class="sxs-lookup"><span data-stu-id="2ed22-214">To test this out, run the application and browse to /StoreManager/Create/ - this will show you the blank form which was returned by the StoreController Create HTTP-GET method.</span></span>

<span data-ttu-id="2ed22-215">Wypełnij pewne wartości, a następnie kliknij przycisk Utwórz, aby przesłać formularz.</span><span class="sxs-lookup"><span data-stu-id="2ed22-215">Fill in some values and click the Create button to submit the form.</span></span>

![](mvc-music-store-part-5/_static/image7.png)

![](mvc-music-store-part-5/_static/image8.png)

### <a name="handling-edits"></a><span data-ttu-id="2ed22-216">Obsługa edycji</span><span class="sxs-lookup"><span data-stu-id="2ed22-216">Handling Edits</span></span>

<span data-ttu-id="2ed22-217">Para akcji Edytuj (HTTP-GET i HTTP-POST) jest bardzo podobna do metod tworzenia akcji, które właśnie oglądamy.</span><span class="sxs-lookup"><span data-stu-id="2ed22-217">The Edit action pair (HTTP-GET and HTTP-POST) are very similar to the Create action methods we just looked at.</span></span> <span data-ttu-id="2ed22-218">Ponieważ scenariusz edycji obejmuje pracę z istniejącym albumem, Metoda Edytuj HTTP-GET ładuje album na podstawie parametru "ID" przekazanego za pośrednictwem trasy.</span><span class="sxs-lookup"><span data-stu-id="2ed22-218">Since the edit scenario involves working with an existing album, the Edit HTTP-GET method loads the Album based on the "id" parameter, passed in via the route.</span></span> <span data-ttu-id="2ed22-219">Ten kod służący do pobierania albumu przez AlbumId jest taki sam, jak poprzednio oglądamy akcję kontrolera szczegółów.</span><span class="sxs-lookup"><span data-stu-id="2ed22-219">This code for retrieving an album by AlbumId is the same as we've previously looked at in the Details controller action.</span></span> <span data-ttu-id="2ed22-220">Podobnie jak w przypadku metody Create/HTTP-GET, wartości rozwijane są zwracane za pośrednictwem ViewBag.</span><span class="sxs-lookup"><span data-stu-id="2ed22-220">As with the Create / HTTP-GET method, the drop down values are returned via the ViewBag.</span></span> <span data-ttu-id="2ed22-221">Dzięki temu możemy zwrócić album jako obiekt modelu do widoku (który jest silnie wpisanych do klasy albumu) podczas przekazywania dodatkowych danych (np. listy gatunków) za pośrednictwem ViewBag.</span><span class="sxs-lookup"><span data-stu-id="2ed22-221">This allows us to return an Album as our model object to the view (which is strongly typed to the Album class) while passing additional data (e.g. a list of Genres) via the ViewBag.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample11.cs)]

<span data-ttu-id="2ed22-222">Akcja Edytuj HTTP-POST jest bardzo podobna do akcji tworzenia protokołu HTTP-POST.</span><span class="sxs-lookup"><span data-stu-id="2ed22-222">The Edit HTTP-POST action is very similar to the Create HTTP-POST action.</span></span> <span data-ttu-id="2ed22-223">Jedyną różnicą jest to, że zamiast dodawania nowego albumu do bazy danych. Kolekcja albumów szuka bieżącego wystąpienia albumu przy użyciu bazy danych. Wpis (album) i ustawienie jego stanu na zmodyfikowano.</span><span class="sxs-lookup"><span data-stu-id="2ed22-223">The only difference is that instead of adding a new album to the db.Albums collection, we're finding the current instance of the Album using db.Entry(album) and setting its state to Modified.</span></span> <span data-ttu-id="2ed22-224">Oznacza to, że Entity Framework modyfikowanie istniejącego albumu zamiast tworzenia nowego.</span><span class="sxs-lookup"><span data-stu-id="2ed22-224">This tells Entity Framework that we are modifying an existing album as opposed to creating a new one.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample12.cs)]

<span data-ttu-id="2ed22-225">Możemy to przetestować, uruchamiając aplikację i przechodząc do/StoreManger/, a następnie klikając link Edytuj dla albumu.</span><span class="sxs-lookup"><span data-stu-id="2ed22-225">We can test this out by running the application and browsing to /StoreManger/, then clicking the Edit link for an album.</span></span>

![](mvc-music-store-part-5/_static/image9.png)

<span data-ttu-id="2ed22-226">Zostanie wyświetlony formularz Edycja pokazywany przez metodę Edytuj HTTP-GET.</span><span class="sxs-lookup"><span data-stu-id="2ed22-226">This displays the Edit form shown by the Edit HTTP-GET method.</span></span> <span data-ttu-id="2ed22-227">Wypełnij pewne wartości, a następnie kliknij przycisk Zapisz.</span><span class="sxs-lookup"><span data-stu-id="2ed22-227">Fill in some values and click the Save button.</span></span>

![](mvc-music-store-part-5/_static/image10.png)

<span data-ttu-id="2ed22-228">Spowoduje to opublikowanie formularza, zapisanie wartości i zwrócenie do listy albumów z informacją o tym, że wartości zostały zaktualizowane.</span><span class="sxs-lookup"><span data-stu-id="2ed22-228">This posts the form, saves the values, and returns us to the Album list, showing that the values were updated.</span></span>

![](mvc-music-store-part-5/_static/image11.png)

### <a name="handling-deletion"></a><span data-ttu-id="2ed22-229">Obsługa usuwania</span><span class="sxs-lookup"><span data-stu-id="2ed22-229">Handling Deletion</span></span>

<span data-ttu-id="2ed22-230">Usunięcie jest zgodne z tym samym wzorcem, co Edytuj i Utwórz, przy użyciu jednej akcji kontrolera do wyświetlania formularza potwierdzenia i innej akcji kontrolera do obsługi przesłania formularza.</span><span class="sxs-lookup"><span data-stu-id="2ed22-230">Deletion follows the same pattern as Edit and Create, using one controller action to display the confirmation form, and another controller action to handle the form submission.</span></span>

<span data-ttu-id="2ed22-231">Akcja protokołu HTTP-GET Delete Controller jest dokładnie taka sama jak w poprzedniej akcji kontrolera. szczegóły Menedżera sklepu.</span><span class="sxs-lookup"><span data-stu-id="2ed22-231">The HTTP-GET Delete controller action is exactly the same as our previous Store Manager Details controller action.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample13.cs)]

<span data-ttu-id="2ed22-232">Wyświetlamy formularz, który jest silnie wpisany do typu albumu, przy użyciu szablonu usuwania zawartości.</span><span class="sxs-lookup"><span data-stu-id="2ed22-232">We display a form that's strongly typed to an Album type, using the Delete view content template.</span></span>

![](mvc-music-store-part-5/_static/image12.png)

<span data-ttu-id="2ed22-233">W obszarze usuwanie szablonu są wyświetlane wszystkie pola dla modelu, ale możemy uprościć, że jest to dość bit.</span><span class="sxs-lookup"><span data-stu-id="2ed22-233">The Delete template shows all the fields for the model, but we can simplify that down quite a bit.</span></span> <span data-ttu-id="2ed22-234">Zmień kod widoku w/Views/StoreManager/Delete.cshtml na następujący.</span><span class="sxs-lookup"><span data-stu-id="2ed22-234">Change the view code in /Views/StoreManager/Delete.cshtml to the following.</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample14.cshtml)]

<span data-ttu-id="2ed22-235">Spowoduje to wyświetlenie uproszczonego potwierdzania usuwania.</span><span class="sxs-lookup"><span data-stu-id="2ed22-235">This displays a simplified Delete confirmation.</span></span>

![](mvc-music-store-part-5/_static/image13.png)

<span data-ttu-id="2ed22-236">Kliknięcie przycisku Usuń powoduje, że formularz zostanie ogłoszony ponownie na serwerze, który wykonuje akcję DeleteConfirmed.</span><span class="sxs-lookup"><span data-stu-id="2ed22-236">Clicking the Delete button causes the form to be posted back to the server, which executes the DeleteConfirmed action.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample15.cs)]

<span data-ttu-id="2ed22-237">Nasza akcja w ramach kontrolera usuwania HTTP-POST wykonuje następujące akcje:</span><span class="sxs-lookup"><span data-stu-id="2ed22-237">Our HTTP-POST Delete Controller Action takes the following actions:</span></span>

- 1. <span data-ttu-id="2ed22-238">Ładuje album według identyfikatora</span><span class="sxs-lookup"><span data-stu-id="2ed22-238">Loads the Album by ID</span></span>
- 2. <span data-ttu-id="2ed22-239">Usuwa album i zapisuje zmiany</span><span class="sxs-lookup"><span data-stu-id="2ed22-239">Deletes it the album and save changes</span></span>
- 3. <span data-ttu-id="2ed22-240">Przekierowuje do indeksu, pokazując, że album został usunięty z listy</span><span class="sxs-lookup"><span data-stu-id="2ed22-240">Redirects to the Index, showing that the Album was removed from the list</span></span>

<span data-ttu-id="2ed22-241">Aby to przetestować, uruchom aplikację i przejdź do/StoreManager.</span><span class="sxs-lookup"><span data-stu-id="2ed22-241">To test this, run the application and browse to /StoreManager.</span></span> <span data-ttu-id="2ed22-242">Wybierz album z listy i kliknij link Usuń.</span><span class="sxs-lookup"><span data-stu-id="2ed22-242">Select an album from the list and click the Delete link.</span></span>

![](mvc-music-store-part-5/_static/image14.png)

<span data-ttu-id="2ed22-243">Spowoduje to wyświetlenie ekranu potwierdzania usuwania.</span><span class="sxs-lookup"><span data-stu-id="2ed22-243">This displays our Delete confirmation screen.</span></span>

![](mvc-music-store-part-5/_static/image15.png)

<span data-ttu-id="2ed22-244">Kliknięcie przycisku Usuń powoduje usunięcie albumu i zwrócenie do strony indeksu Menedżera sklepu, która pokazuje, że album został usunięty.</span><span class="sxs-lookup"><span data-stu-id="2ed22-244">Clicking the Delete button removes the album and returns us to the Store Manager Index page, which shows that the album has been deleted.</span></span>

![](mvc-music-store-part-5/_static/image16.png)

### <a name="using-a-custom-html-helper-to-truncate-text"></a><span data-ttu-id="2ed22-245">Używanie niestandardowego pomocnika HTML do obcinania tekstu</span><span class="sxs-lookup"><span data-stu-id="2ed22-245">Using a custom HTML Helper to truncate text</span></span>

<span data-ttu-id="2ed22-246">Mamy jeden potencjalny problem z naszą stroną indeksu Menedżera sklepu.</span><span class="sxs-lookup"><span data-stu-id="2ed22-246">We've got one potential issue with our Store Manager Index page.</span></span> <span data-ttu-id="2ed22-247">Nasz tytuł albumu i właściwości nazwy wykonawcy mogą być wystarczająco długie, aby mogły zgłosić nasze formatowanie tabeli.</span><span class="sxs-lookup"><span data-stu-id="2ed22-247">Our Album Title and Artist Name properties can both be long enough that they could throw off our table formatting.</span></span> <span data-ttu-id="2ed22-248">Utworzymy niestandardowy pomocnik HTML, aby umożliwić nam łatwe obcinanie tych i innych właściwości w naszych widokach.</span><span class="sxs-lookup"><span data-stu-id="2ed22-248">We'll create a custom HTML Helper to allow us to easily truncate these and other properties in our Views.</span></span>

![](mvc-music-store-part-5/_static/image17.png)

<span data-ttu-id="2ed22-249">Składnia @helper Razor ułatwia tworzenie własnych funkcji pomocnika do użycia w widokach.</span><span class="sxs-lookup"><span data-stu-id="2ed22-249">Razor's @helper syntax has made it pretty easy to create your own helper functions for use in your views.</span></span> <span data-ttu-id="2ed22-250">Otwórz widok/Views/StoreManager/Index.cshtml i Dodaj następujący kod bezpośrednio po wierszu @model.</span><span class="sxs-lookup"><span data-stu-id="2ed22-250">Open the /Views/StoreManager/Index.cshtml view and add the following code directly after the @model line.</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample16.cshtml)]

<span data-ttu-id="2ed22-251">Ta metoda pomocnika przyjmuje ciąg i maksymalną dozwoloną długość.</span><span class="sxs-lookup"><span data-stu-id="2ed22-251">This helper method takes a string and a maximum length to allow.</span></span> <span data-ttu-id="2ed22-252">Jeśli podany tekst jest krótszy niż określona długość, pomocnik będzie wyprowadzał go jako-is.</span><span class="sxs-lookup"><span data-stu-id="2ed22-252">If the text supplied is shorter than the length specified, the helper outputs it as-is.</span></span> <span data-ttu-id="2ed22-253">Jeśli jest dłuższa, obcina tekst i renderuje "..." dla reszty.</span><span class="sxs-lookup"><span data-stu-id="2ed22-253">If it is longer, then it truncates the text and renders "…" for the remainder.</span></span>

<span data-ttu-id="2ed22-254">Teraz możemy użyć naszego pomocnika obcinania, aby upewnić się, że właściwości tytułu albumu i wykonawcy mają mniej niż 25 znaków.</span><span class="sxs-lookup"><span data-stu-id="2ed22-254">Now we can use our Truncate helper to ensure that both the Album Title and Artist Name properties are less than 25 characters.</span></span> <span data-ttu-id="2ed22-255">Pełny kod widoku przy użyciu naszego nowego pomocnika obcinania pojawia się poniżej.</span><span class="sxs-lookup"><span data-stu-id="2ed22-255">The complete view code using our new Truncate helper appears below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample17.cshtml)]

<span data-ttu-id="2ed22-256">Teraz, gdy przeglądasz adres URL/StoreManager/, albumy i tytuły będą przechowywane poniżej naszych maksymalnych długości.</span><span class="sxs-lookup"><span data-stu-id="2ed22-256">Now when we browse the /StoreManager/ URL, the albums and titles are kept below our maximum lengths.</span></span>

![](mvc-music-store-part-5/_static/image18.png)

<span data-ttu-id="2ed22-257">Uwaga: pokazuje prosty przypadek tworzenia i używania pomocnika w jednym widoku.</span><span class="sxs-lookup"><span data-stu-id="2ed22-257">Note: This shows the simple case of creating and using a helper in one view.</span></span> <span data-ttu-id="2ed22-258">Aby dowiedzieć się więcej na temat tworzenia pomocników, których możesz użyć w całej lokacji, zobacz mój wpis w blogu: [http://bit.ly/mvc3-helper-options](http://bit.ly/mvc3-helper-options)</span><span class="sxs-lookup"><span data-stu-id="2ed22-258">To learn more about creating helpers that you can use throughout your site, see my blog post: [http://bit.ly/mvc3-helper-options](http://bit.ly/mvc3-helper-options)</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2ed22-259">[Poprzednie](mvc-music-store-part-4.md)
> [dalej](mvc-music-store-part-6.md)</span><span class="sxs-lookup"><span data-stu-id="2ed22-259">[Previous](mvc-music-store-part-4.md)
[Next](mvc-music-store-part-6.md)</span></span>

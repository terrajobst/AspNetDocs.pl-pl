---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
title: 'Część 3: widoki i modele widoków | Microsoft Docs'
author: jongalloway
description: Ta seria samouczków zawiera szczegółowe informacje na temat wszystkich kroków podjętych w celu skompilowania przykładowej aplikacji do sklepu ASP.NET MVC Music. Część 3 obejmuje widoki i modele widoków.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 94297aa0-1f2d-4d72-bbcb-63f64653e0c0
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
msc.type: authoredcontent
ms.openlocfilehash: 3fcfc816cde22c697a78bab2c9ea7ace1bf68501
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78559809"
---
# <a name="part-3-views-and-viewmodels"></a><span data-ttu-id="8addd-104">Część 3. Widoki i modele widoków</span><span class="sxs-lookup"><span data-stu-id="8addd-104">Part 3: Views and ViewModels</span></span>

<span data-ttu-id="8addd-105">przez [Jan Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="8addd-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="8addd-106">Sklep MVC Music jest aplikacją samouczka, która wprowadza i objaśnia krok po kroku, jak używać ASP.NET MVC i Visual Studio do programowania w sieci Web.</span><span class="sxs-lookup"><span data-stu-id="8addd-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="8addd-107">Sklep MVC Music jest lekkim przykładowym wdrożeniem magazynu, który sprzedaje Albumy muzyczne w trybie online i implementuje podstawowe funkcje administracyjne, logowania użytkownika i koszyka.</span><span class="sxs-lookup"><span data-stu-id="8addd-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="8addd-108">Ta seria samouczków zawiera szczegółowe informacje na temat wszystkich kroków podjętych w celu skompilowania przykładowej aplikacji do sklepu ASP.NET MVC Music.</span><span class="sxs-lookup"><span data-stu-id="8addd-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="8addd-109">Część 3 obejmuje widoki i modele widoków.</span><span class="sxs-lookup"><span data-stu-id="8addd-109">Part 3 covers Views and ViewModels.</span></span>

<span data-ttu-id="8addd-110">Do tej pory właśnie zwracamy ciągi z akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="8addd-110">So far we've just been returning strings from controller actions.</span></span> <span data-ttu-id="8addd-111">Jest to świetny sposób, aby poznać sposób działania kontrolerów, ale nie jest to, w jaki sposób chcesz utworzyć rzeczywistą aplikację sieci Web.</span><span class="sxs-lookup"><span data-stu-id="8addd-111">That's a nice way to get an idea of how controllers work, but it's not how you'd want to build a real web application.</span></span> <span data-ttu-id="8addd-112">Chcemy ulepszyć generowanie kodu HTML z powrotem do przeglądarek odwiedzających naszą witrynę — jeden tam, gdzie możemy używać plików szablonów, aby łatwiej dostosować zawartość HTML.</span><span class="sxs-lookup"><span data-stu-id="8addd-112">We are going to want a better way to generate HTML back to browsers visiting our site – one where we can use template files to more easily customize the HTML content send back.</span></span> <span data-ttu-id="8addd-113">To dokładnie, które widoki robią.</span><span class="sxs-lookup"><span data-stu-id="8addd-113">That's exactly what Views do.</span></span>

## <a name="adding-a-view-template"></a><span data-ttu-id="8addd-114">Dodawanie szablonu widoku</span><span class="sxs-lookup"><span data-stu-id="8addd-114">Adding a View template</span></span>

<span data-ttu-id="8addd-115">Aby użyć szablonu widoku, zmienimy metodę indeksu HomeController, aby zwracała ActionResult, i będzie mieć widok zwracany (), podobny do poniższego:</span><span class="sxs-lookup"><span data-stu-id="8addd-115">To use a view-template, we'll change the HomeController Index method to return an ActionResult, and have it return View(), like below:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample1.cs)]

<span data-ttu-id="8addd-116">Powyższa zmiana wskazuje, że zamiast zwrócić ciąg, chcemy użyć "widoku" do wygenerowania wyniku z powrotem.</span><span class="sxs-lookup"><span data-stu-id="8addd-116">The above change indicates that instead of returned a string, we instead want to use a "View" to generate a result back.</span></span>

<span data-ttu-id="8addd-117">Teraz dodamy odpowiedni szablon widoku do projektu.</span><span class="sxs-lookup"><span data-stu-id="8addd-117">We'll now add an appropriate View template to our project.</span></span> <span data-ttu-id="8addd-118">Aby to zrobić, ustaw kursor tekstu w metodzie akcji indeksu, a następnie kliknij prawym przyciskiem myszy i wybierz pozycję "Dodaj widok".</span><span class="sxs-lookup"><span data-stu-id="8addd-118">To do this we'll position the text cursor within the Index action method, then right-click and select "Add View".</span></span> <span data-ttu-id="8addd-119">Spowoduje to wyświetlenie okna dialogowego Dodawanie widoku:</span><span class="sxs-lookup"><span data-stu-id="8addd-119">This will bring up the Add View dialog:</span></span>

<span data-ttu-id="8addd-120">![](mvc-music-store-part-3/_static/image1.jpg) ![](mvc-music-store-part-3/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="8addd-120">![](mvc-music-store-part-3/_static/image1.jpg) ![](mvc-music-store-part-3/_static/image1.png)</span></span>

<span data-ttu-id="8addd-121">Okno dialogowe "Dodawanie widoku" pozwala nam szybko i łatwo generować pliki szablonów widoków.</span><span class="sxs-lookup"><span data-stu-id="8addd-121">The "Add View" dialog allows us to quickly and easily generate View template files.</span></span> <span data-ttu-id="8addd-122">Domyślnie okno dialogowe "Dodaj widok" wstępnie wypełnia nazwę szablonu widoku do utworzenia, aby była zgodna z metodą akcji, która będzie go używać.</span><span class="sxs-lookup"><span data-stu-id="8addd-122">By default the "Add View" dialog pre-populates the name of the View template to create so that it matches the action method that will use it.</span></span> <span data-ttu-id="8addd-123">Ze względu na to, że użyto menu kontekstowego "Dodaj widok" w metodzie akcji index () naszego HomeController, w oknie dialogowym "Dodaj widok" powyżej znajduje się wartość "index" (nazwa widoku jest wstępnie wypełniona).</span><span class="sxs-lookup"><span data-stu-id="8addd-123">Because we used the "Add View" context menu within the Index() action method of our HomeController, the "Add View" dialog above has "Index" as the view name pre-populated by default.</span></span> <span data-ttu-id="8addd-124">Nie trzeba zmieniać żadnych opcji w tym oknie dialogowym, dlatego kliknij przycisk Dodaj.</span><span class="sxs-lookup"><span data-stu-id="8addd-124">We don't need to change any of the options on this dialog, so click the Add button.</span></span>

<span data-ttu-id="8addd-125">Po kliknięciu przycisku Dodaj, Visual Web Developer utworzy nowy szablon widoku index. cshtml dla nas w katalogu \Views\Home, tworząc folder, jeśli jeszcze nie istnieje.</span><span class="sxs-lookup"><span data-stu-id="8addd-125">When we click the Add button, Visual Web Developer will create a new Index.cshtml view template for us in the \Views\Home directory, creating the folder if doesn't already exist.</span></span>

![](mvc-music-store-part-3/_static/image2.png)

<span data-ttu-id="8addd-126">Nazwa i lokalizacja pliku "index. cshtml" są ważne i są zgodne z domyślnymi konwencjami nazewnictwa MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="8addd-126">The name and folder location of the "Index.cshtml" file is important, and follows the default ASP.NET MVC naming conventions.</span></span> <span data-ttu-id="8addd-127">Nazwa katalogu, \Views\Home, pasuje do kontrolera o nazwie HomeController.</span><span class="sxs-lookup"><span data-stu-id="8addd-127">The directory name, \Views\Home, matches the controller - which is named HomeController.</span></span> <span data-ttu-id="8addd-128">Nazwa szablonu widoku, indeks, dopasowuje metodę akcji kontrolera, która będzie wyświetlać widok.</span><span class="sxs-lookup"><span data-stu-id="8addd-128">The view template name, Index, matches the controller action method which will be displaying the view.</span></span>

<span data-ttu-id="8addd-129">ASP.NET MVC umożliwia nam uniknięcie jawnego określenia nazwy lub lokalizacji szablonu widoku, gdy używamy tej konwencji nazewnictwa do zwrócenia widoku.</span><span class="sxs-lookup"><span data-stu-id="8addd-129">ASP.NET MVC allows us to avoid having to explicitly specify the name or location of a view template when we use this naming convention to return a view.</span></span> <span data-ttu-id="8addd-130">Domyślnie renderuje szablon widoku \Views\Home\Index.cshtml, gdy piszesz kod podobny do poniższego w naszym HomeController:</span><span class="sxs-lookup"><span data-stu-id="8addd-130">It will by default render the \Views\Home\Index.cshtml view template when we write code like below within our HomeController:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample2.cs)]

<span data-ttu-id="8addd-131">Visual Web Developer utworzył i otworzył szablon widoku "index. cshtml" po kliknięciu przycisku "Dodaj" w oknie dialogowym "Dodawanie widoku".</span><span class="sxs-lookup"><span data-stu-id="8addd-131">Visual Web Developer created and opened the "Index.cshtml" view template after we clicked the "Add" button within the "Add View" dialog.</span></span> <span data-ttu-id="8addd-132">Zawartość index. cshtml pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="8addd-132">The contents of Index.cshtml are shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample3.cshtml)]

<span data-ttu-id="8addd-133">Ten widok używa składnia Razor, który jest bardziej zwięzły niż aparat widoku formularzy sieci Web używany w ASP.NET Web Forms i poprzednich wersjach ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="8addd-133">This view is using the Razor syntax, which is more concise than the Web Forms view engine used in ASP.NET Web Forms and previous versions of ASP.NET MVC.</span></span> <span data-ttu-id="8addd-134">Aparat widoku formularzy sieci Web jest nadal dostępny w ASP.NET MVC 3, ale wielu deweloperów stwierdzi, że aparat widoku Razor dopasowuje ASP.NET rozwój MVC.</span><span class="sxs-lookup"><span data-stu-id="8addd-134">The Web Forms view engine is still available in ASP.NET MVC 3, but many developers find that the Razor view engine fits ASP.NET MVC development really well.</span></span>

<span data-ttu-id="8addd-135">Pierwsze trzy wiersze ustawiają tytuł strony przy użyciu ViewBag. title.</span><span class="sxs-lookup"><span data-stu-id="8addd-135">The first three lines set the page title using ViewBag.Title.</span></span> <span data-ttu-id="8addd-136">Wkrótce sprawdzimy, jak to działa bardziej szczegółowo, ale najpierw zaktualizujemy tekst nagłówka tekstu i wyświetli stronę.</span><span class="sxs-lookup"><span data-stu-id="8addd-136">We'll look at how this works in more detail soon, but first let's update the text heading text and view the page.</span></span> <span data-ttu-id="8addd-137">Zaktualizuj tag &lt;H2&gt;, aby powiedzieć "to jest Strona główna", jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="8addd-137">Update the &lt;h2&gt; tag to say "This is the Home Page" as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample4.cshtml)]

<span data-ttu-id="8addd-138">Uruchomienie aplikacji pokazuje, że nowy tekst jest widoczny na stronie głównej.</span><span class="sxs-lookup"><span data-stu-id="8addd-138">Running the application shows that our new text is visible on the home page.</span></span>

![](mvc-music-store-part-3/_static/image3.png)

## <a name="using-a-layout-for-common-site-elements"></a><span data-ttu-id="8addd-139">Korzystanie z układu dla wspólnych elementów lokacji</span><span class="sxs-lookup"><span data-stu-id="8addd-139">Using a Layout for common site elements</span></span>

<span data-ttu-id="8addd-140">Większość witryn sieci Web ma zawartość, która jest udostępniana między wieloma stronami: nawigowanie, stopki, obrazy logo, odwołania arkusza stylów itp. Aparat widoku Razor ułatwia zarządzanie przy użyciu strony o nazwie \_Layout. cshtml, która została automatycznie utworzona dla nas w folderze/Views/Shared.</span><span class="sxs-lookup"><span data-stu-id="8addd-140">Most websites have content which is shared between many pages: navigation, footers, logo images, stylesheet references, etc. The Razor view engine makes this easy to manage using a page called \_Layout.cshtml which has automatically been created for us inside the /Views/Shared folder.</span></span>

![](mvc-music-store-part-3/_static/image4.png)

<span data-ttu-id="8addd-141">Kliknij dwukrotnie ten folder, aby wyświetlić zawartość, która jest pokazana poniżej.</span><span class="sxs-lookup"><span data-stu-id="8addd-141">Double-click on this folder to view the contents, which are shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample5.cshtml)]

<span data-ttu-id="8addd-142">Zawartość z poszczególnych widoków zostanie wyświetlona przy użyciu polecenia @RenderBody(), a każda Wspólna zawartość, która ma być wyświetlana poza programem, można dodać do znacznika \_Layout. cshtml.</span><span class="sxs-lookup"><span data-stu-id="8addd-142">The content from our individual views will be displayed by the @RenderBody() command, and any common content that we want to appear outside of that can be added to the \_Layout.cshtml markup.</span></span> <span data-ttu-id="8addd-143">Chcemy, aby nasz sklep MVC Music miał wspólny nagłówek z linkami do naszej strony głównej i obszaru przechowywania na wszystkich stronach w witrynie, dlatego dodamy go do szablonu bezpośrednio powyżej tej instrukcji @RenderBody().</span><span class="sxs-lookup"><span data-stu-id="8addd-143">We'll want our MVC Music Store to have a common header with links to our Home page and Store area on all pages in the site, so we'll add that to the template directly above that @RenderBody() statement.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample6.cshtml)]

## <a name="updating-the-stylesheet"></a><span data-ttu-id="8addd-144">Aktualizowanie arkusza stylów</span><span class="sxs-lookup"><span data-stu-id="8addd-144">Updating the StyleSheet</span></span>

<span data-ttu-id="8addd-145">Szablon pustego projektu zawiera bardzo uproszczony plik CSS, który zawiera tylko style używane do wyświetlania komunikatów sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="8addd-145">The empty project template includes a very streamlined CSS file which just includes styles used to display validation messages.</span></span> <span data-ttu-id="8addd-146">Nasz Projektant udostępnił kilka dodatkowych arkuszy CSS i obrazów, aby zdefiniować wygląd i działanie naszej witryny, dlatego dodamy je teraz.</span><span class="sxs-lookup"><span data-stu-id="8addd-146">Our designer has provided some additional CSS and images to define the look and feel for our site, so we'll add those in now.</span></span>

<span data-ttu-id="8addd-147">Zaktualizowany plik i obrazy CSS są zawarte w katalogu zawartości MvcMusicStore-Assets. zip, który jest dostępny w [sklepie MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="8addd-147">The updated CSS file and Images are included in the Content directory of MvcMusicStore-Assets.zip which is available at [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span> <span data-ttu-id="8addd-148">Będziemy wybierać obydwie te elementy w Eksploratorze Windows i upuszczać je do folderu zawartości naszego rozwiązania w programie Visual Web Developer, jak pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="8addd-148">We'll select both of them in Windows Explorer and drop them into our Solution's Content folder in Visual Web Developer, as shown below:</span></span>

![](mvc-music-store-part-3/_static/image5.png)

<span data-ttu-id="8addd-149">Zostanie wyświetlony monit o potwierdzenie, czy chcesz zastąpić istniejący plik. css.</span><span class="sxs-lookup"><span data-stu-id="8addd-149">You'll be asked to confirm if you want to overwrite the existing Site.css file.</span></span> <span data-ttu-id="8addd-150">kliknij przycisk Tak.</span><span class="sxs-lookup"><span data-stu-id="8addd-150">Click Yes.</span></span>

![](mvc-music-store-part-3/_static/image6.png)

<span data-ttu-id="8addd-151">Folder zawartości aplikacji będzie teraz wyświetlany w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="8addd-151">The Content folder of your application will now appear as follows:</span></span>

![](mvc-music-store-part-3/_static/image7.png)

<span data-ttu-id="8addd-152">Teraz Uruchommy aplikację i zobacz, w jaki sposób zmiany wyglądają na stronie głównej.</span><span class="sxs-lookup"><span data-stu-id="8addd-152">Now let's run the application and see how our changes look on the Home page.</span></span>

![](mvc-music-store-part-3/_static/image8.png)

- <span data-ttu-id="8addd-153">Zapoznaj się z informacjami o zmianach: odnaleziono metodę akcji indeksu HomeController i Wyświetlono szablon \Views\Home\Index.cshtmlView, mimo że nasz kod nosi nazwę "return View ()", ponieważ nasz szablon widoku przestrzega standardowej konwencji nazewnictwa.</span><span class="sxs-lookup"><span data-stu-id="8addd-153">Let's review what's changed: The HomeController's Index action method found and displayed the \Views\Home\Index.cshtmlView template, even though our code called "return View()", because our View template followed the standard naming convention.</span></span>
- <span data-ttu-id="8addd-154">Na stronie głównej jest wyświetlany prosty komunikat powitalny, który jest zdefiniowany w szablonie widoku \Views\Home\Index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="8addd-154">The Home Page is displaying a simple welcome message that is defined within the \Views\Home\Index.cshtml view template.</span></span>
- <span data-ttu-id="8addd-155">Strona główna używa szablonu \_Layout. cshtml, dlatego Komunikat powitalny jest zawarty w układzie HTML witryny standardowej.</span><span class="sxs-lookup"><span data-stu-id="8addd-155">The Home Page is using our \_Layout.cshtml template, and so the welcome message is contained within the standard site HTML layout.</span></span>

## <a name="using-a-model-to-pass-information-to-our-view"></a><span data-ttu-id="8addd-156">Używanie modelu do przekazywania informacji do naszego widoku</span><span class="sxs-lookup"><span data-stu-id="8addd-156">Using a Model to pass information to our View</span></span>

<span data-ttu-id="8addd-157">Szablon widoku, który po prostu wyświetla kod HTML stałe, nie może utworzyć bardzo interesującej witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="8addd-157">A View template that just displays hardcoded HTML isn't going to make a very interesting web site.</span></span> <span data-ttu-id="8addd-158">Aby utworzyć dynamiczną witrynę sieci Web, należy przekazać informacje z naszych akcji kontrolera do naszych szablonów widoków.</span><span class="sxs-lookup"><span data-stu-id="8addd-158">To create a dynamic web site, we'll instead want to pass information from our controller actions to our view templates.</span></span>

<span data-ttu-id="8addd-159">W wzorcu Model-View-Controller pojęcie model odnosi się do obiektów, które reprezentują dane w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8addd-159">In the Model-View-Controller pattern, the term Model refers to objects which represent the data in the application.</span></span> <span data-ttu-id="8addd-160">Często obiekty modelu są zgodne z tabelami w bazie danych, ale nie muszą.</span><span class="sxs-lookup"><span data-stu-id="8addd-160">Often, model objects correspond to tables in your database, but they don't have to.</span></span>

<span data-ttu-id="8addd-161">Metody akcji kontrolera, które zwracają ActionResult, mogą przekazać obiekt modelu do widoku.</span><span class="sxs-lookup"><span data-stu-id="8addd-161">Controller action methods which return an ActionResult can pass a model object to the view.</span></span> <span data-ttu-id="8addd-162">Dzięki temu kontroler może oczyścić wszystkie informacje potrzebne do wygenerowania odpowiedzi, a następnie przekazać te informacje do szablonu widoku, który zostanie użyty do wygenerowania odpowiedniej odpowiedzi HTML.</span><span class="sxs-lookup"><span data-stu-id="8addd-162">This allows a Controller to cleanly package up all the information needed to generate a response, and then pass this information off to a View template to use to generate the appropriate HTML response.</span></span> <span data-ttu-id="8addd-163">Jest to najłatwiej zrozumieć, wyświetlając go w działaniu, więc zacznijmy.</span><span class="sxs-lookup"><span data-stu-id="8addd-163">This is easiest to understand by seeing it in action, so let's get started.</span></span>

<span data-ttu-id="8addd-164">Najpierw utworzysz klasy modelu, aby reprezentować gatunek i albumy w naszym sklepie.</span><span class="sxs-lookup"><span data-stu-id="8addd-164">First we'll create some Model classes to represent Genres and Albums within our store.</span></span> <span data-ttu-id="8addd-165">Zacznijmy od utworzenia klasy gatunku.</span><span class="sxs-lookup"><span data-stu-id="8addd-165">Let's start by creating a Genre class.</span></span> <span data-ttu-id="8addd-166">Kliknij prawym przyciskiem myszy folder "models" w projekcie, wybierz opcję "Dodaj klasę" i Nazwij plik "Genre.cs".</span><span class="sxs-lookup"><span data-stu-id="8addd-166">Right-click the "Models" folder within your project, choose the "Add Class" option, and name the file "Genre.cs".</span></span>

![](mvc-music-store-part-3/_static/image2.jpg)

![](mvc-music-store-part-3/_static/image9.png)

<span data-ttu-id="8addd-167">Następnie Dodaj publiczną właściwość nazwy ciągu do klasy, która została utworzona:</span><span class="sxs-lookup"><span data-stu-id="8addd-167">Then add a public string Name property to the class that was created:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample7.cs)]

<span data-ttu-id="8addd-168">*Uwaga: na wypadek zastanawiasz się, że w notacji {get; set;} jest C#używana funkcja właściwości, która została zaimplementowana. Daje to nam zalety właściwości bez konieczności deklarowania pola zapasowego.*</span><span class="sxs-lookup"><span data-stu-id="8addd-168">*Note: In case you're wondering, the { get; set; } notation is making use of C#'s auto-implemented properties feature. This gives us the benefits of a property without requiring us to declare a backing field.*</span></span>

<span data-ttu-id="8addd-169">Następnie wykonaj te same kroki, aby utworzyć klasę albumu (o nazwie Album.cs), która ma tytuł i Właściwość gatunku:</span><span class="sxs-lookup"><span data-stu-id="8addd-169">Next, follow the same steps to create an Album class (named Album.cs) that has a Title and a Genre property:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample8.cs)]

<span data-ttu-id="8addd-170">Teraz możemy zmodyfikować StoreController tak, aby korzystał z widoków, które wyświetlają dynamiczne informacje z naszego modelu.</span><span class="sxs-lookup"><span data-stu-id="8addd-170">Now we can modify the StoreController to use Views which display dynamic information from our Model.</span></span> <span data-ttu-id="8addd-171">Jeśli — dla celów demonstracyjnych już teraz — nazywamy Twoje albumy na podstawie identyfikatora żądania, możemy wyświetlić te informacje jak w poniższym widoku.</span><span class="sxs-lookup"><span data-stu-id="8addd-171">If - for demonstration purposes right now - we named our Albums based on the request ID, we could display that information as in the view below.</span></span>

![](mvc-music-store-part-3/_static/image10.png)

<span data-ttu-id="8addd-172">Zaczniemy od zmiany akcji szczegóły sklepu, aby wyświetlić informacje o pojedynczym albumie.</span><span class="sxs-lookup"><span data-stu-id="8addd-172">We'll start by changing the Store Details action so it shows the information for a single album.</span></span> <span data-ttu-id="8addd-173">Dodaj instrukcję "Using" na początku klasy **StoreControllers** , aby uwzględnić przestrzeń nazw MvcMusicStore. Models, więc nie trzeba wpisywać MvcMusicStore. models. album za każdym razem, gdy chcemy użyć klasy albumu.</span><span class="sxs-lookup"><span data-stu-id="8addd-173">Add a "using" statement to the top of the **StoreControllers** class to include the MvcMusicStore.Models namespace, so we don't need to type MvcMusicStore.Models.Album every time we want to use the album class.</span></span> <span data-ttu-id="8addd-174">Sekcja "usings" tej klasy powinna teraz wyglądać następująco.</span><span class="sxs-lookup"><span data-stu-id="8addd-174">The "usings" section of that class should now appear as below.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample9.cs)]

<span data-ttu-id="8addd-175">Następnie zaktualizujemy akcję kontrolera szczegółów w taki sposób, aby zwracała ActionResult zamiast ciągu, podobnie jak w przypadku metody indeksu HomeController.</span><span class="sxs-lookup"><span data-stu-id="8addd-175">Next, we'll update the Details controller action so that it returns an ActionResult rather than a string, as we did with the HomeController's Index method.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample10.cs)]

<span data-ttu-id="8addd-176">Teraz możemy zmodyfikować logikę, aby zwracała obiekt albumu do widoku.</span><span class="sxs-lookup"><span data-stu-id="8addd-176">Now we can modify the logic to return an Album object to the view.</span></span> <span data-ttu-id="8addd-177">W dalszej części tego samouczka będziemy pobierać dane z bazy danych, ale w celu rozpoczęcia pracy będziemy używać "fikcyjnych danych".</span><span class="sxs-lookup"><span data-stu-id="8addd-177">Later in this tutorial we will be retrieving the data from a database – but for right now we will use "dummy data" to get started.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample11.cs)]

<span data-ttu-id="8addd-178">*Uwaga: Jeśli nie masz doświadczenia w programie C#, możesz założyć, że użycie funkcji VAR oznacza, że nasza zmienna albumu jest opóźniona. To nie jest poprawne — C# kompilator używa wnioskowania typu na podstawie tego, co przypiszemy do zmiennej, aby określić, że album jest typu albumu i kompiluje lokalną zmienną albumu jako typ albumu, dzięki czemu będziemy mogli sprawdzać czas kompilacji i obsługiwać Edytor kodu programu Visual Studio.*</span><span class="sxs-lookup"><span data-stu-id="8addd-178">*Note: If you're unfamiliar with C#, you may assume that using var means that our album variable is late-bound. That's not correct – the C# compiler is using type-inference based on what we're assigning to the variable to determine that album is of type Album and compiling the local album variable as an Album type, so we get compile-time checking and Visual Studio code-editor support.*</span></span>

<span data-ttu-id="8addd-179">Teraz utworzysz szablon widoku, który używa naszego albumu do wygenerowania odpowiedzi HTML.</span><span class="sxs-lookup"><span data-stu-id="8addd-179">Let's now create a View template that uses our Album to generate an HTML response.</span></span> <span data-ttu-id="8addd-180">Przed wykonaniem tej czynności musimy skompilować projekt, aby okno dialogowe Dodawanie widoku wie o naszej nowo utworzonej klasie albumu.</span><span class="sxs-lookup"><span data-stu-id="8addd-180">Before we do that we need to build the project so that the Add View dialog knows about our newly created Album class.</span></span> <span data-ttu-id="8addd-181">Można skompilować projekt, wybierając element menu Debuguj ⇨ kompilację MvcMusicStore (Aby uzyskać dodatkowe środki, można użyć skrótu Ctrl-Shift-B do skompilowania projektu).</span><span class="sxs-lookup"><span data-stu-id="8addd-181">You can build the project by selecting the Debug⇨Build MvcMusicStore menu item (for extra credit, you can use the Ctrl-Shift-B shortcut to build the project).</span></span>

![](mvc-music-store-part-3/_static/image11.png)

<span data-ttu-id="8addd-182">Teraz, po skonfigurowaniu naszych klas pomocniczych, jesteśmy gotowi do skompilowania naszego szablonu widoku.</span><span class="sxs-lookup"><span data-stu-id="8addd-182">Now that we've set up our supporting classes, we're ready to build our View template.</span></span> <span data-ttu-id="8addd-183">Kliknij prawym przyciskiem myszy w ramach metody szczegóły i wybierz pozycję "Dodaj widok..." z menu kontekstowego.</span><span class="sxs-lookup"><span data-stu-id="8addd-183">Right-click within the Details method and select "Add View…" from the context menu.</span></span>

![](mvc-music-store-part-3/_static/image12.png)

<span data-ttu-id="8addd-184">Utworzymy nowy szablon widoku, taki jak wcześniej z HomeController.</span><span class="sxs-lookup"><span data-stu-id="8addd-184">We are going to create a new View template like we did before with the HomeController.</span></span> <span data-ttu-id="8addd-185">Ponieważ tworzymy ją z StoreController, zostanie ona domyślnie wygenerowana w pliku \Views\Store\Index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="8addd-185">Because we are creating it from the StoreController it will by default be generated in a \Views\Store\Index.cshtml file.</span></span>

<span data-ttu-id="8addd-186">W przeciwieństwie do wcześniej sprawdzimy pole wyboru "Utwórz silnie wpisaną" widok.</span><span class="sxs-lookup"><span data-stu-id="8addd-186">Unlike before, we are going to check the "Create a strongly-typed" view checkbox.</span></span> <span data-ttu-id="8addd-187">Następnie wybieramy naszą klasę "album" w menu rozwijanym "Wyświetl dane" klasy "downlist".</span><span class="sxs-lookup"><span data-stu-id="8addd-187">We are then going to select our "Album" class within the "View data-class" drop-downlist.</span></span> <span data-ttu-id="8addd-188">Spowoduje to wyświetlenie okna dialogowego "Dodawanie widoku" w celu utworzenia szablonu widoku, który oczekuje, że obiekt albumu zostanie przesłany do niego do użycia.</span><span class="sxs-lookup"><span data-stu-id="8addd-188">This will cause the "Add View" dialog to create a View template that expects that an Album object will be passed to it to use.</span></span>

![](mvc-music-store-part-3/_static/image13.png)

<span data-ttu-id="8addd-189">Po kliknięciu przycisku "Dodaj" zostanie utworzony szablon widoku \Views\Store\Details.cshtml zawierający poniższy kod.</span><span class="sxs-lookup"><span data-stu-id="8addd-189">When we click the "Add" button our \Views\Store\Details.cshtml View template will be created, containing the following code.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample12.cshtml)]

<span data-ttu-id="8addd-190">Zwróć uwagę na pierwszy wiersz, który wskazuje, że ten widok jest silnie wpisana do naszej klasy albumu.</span><span class="sxs-lookup"><span data-stu-id="8addd-190">Notice the first line, which indicates that this view is strongly-typed to our Album class.</span></span> <span data-ttu-id="8addd-191">Aparat widoku Razor rozumie, że został przekazano obiekt albumu, dzięki czemu możemy łatwo uzyskać dostęp do właściwości modelu i nawet korzystać z funkcji IntelliSense w edytorze Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="8addd-191">The Razor view engine understands that it has been passed an Album object, so we can easily access model properties and even have the benefit of IntelliSense in the Visual Web Developer editor.</span></span>

<span data-ttu-id="8addd-192">Zaktualizuj tag &lt;H2&gt; tak, aby wyświetlał Właściwość tytuł albumu, modyfikując ten wiersz tak, aby pojawił się w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="8addd-192">Update the &lt;h2&gt; tag so it displays the Album's Title property by modifying that line to appear as follows.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample13.cshtml)]

<span data-ttu-id="8addd-193">Zauważ, że funkcja IntelliSense jest wyzwalana po wprowadzeniu kropki po słowie kluczowym @Model, pokazując właściwości i metody obsługiwane przez klasę albumu.</span><span class="sxs-lookup"><span data-stu-id="8addd-193">Notice that IntelliSense is triggered when you enter the period after the @Model keyword, showing the properties and methods that the Album class supports.</span></span>

<span data-ttu-id="8addd-194">Teraz ponownie uruchom nasz projekt i przejdź do adresu URL/Store/Details/5.</span><span class="sxs-lookup"><span data-stu-id="8addd-194">Let's now re-run our project and visit the /Store/Details/5 URL.</span></span> <span data-ttu-id="8addd-195">Zobaczymy szczegółowe informacje o albumie, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="8addd-195">We'll see details of an Album like below.</span></span>

![](mvc-music-store-part-3/_static/image14.png)

<span data-ttu-id="8addd-196">Teraz wprowadzimy podobną aktualizację metody operacji przeglądania sklepu.</span><span class="sxs-lookup"><span data-stu-id="8addd-196">Now we'll make a similar update for the Store Browse action method.</span></span> <span data-ttu-id="8addd-197">Zaktualizuj metodę, tak aby zwracała ActionResult, i zmodyfikuj logikę metody, aby tworzyła nowy obiekt gatunku i zwracał go do widoku.</span><span class="sxs-lookup"><span data-stu-id="8addd-197">Update the method so it returns an ActionResult, and modify the method logic so it creates a new Genre object and returns it to the View.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample14.cs)]

<span data-ttu-id="8addd-198">Kliknij prawym przyciskiem myszy w metodzie Przeglądaj i wybierz pozycję "Dodaj widok..." z menu kontekstowego Dodaj widok, który jest silnie określony, Dodaj silnie wpisanej klasy gatunku.</span><span class="sxs-lookup"><span data-stu-id="8addd-198">Right-click in the Browse method and select "Add View…" from the context menu, then add a View that is strongly-typed add a strongly typed to the Genre class.</span></span>

![](mvc-music-store-part-3/_static/image15.png)

<span data-ttu-id="8addd-199">Zaktualizuj element &lt;H2&gt; w kodzie widoku (w/Views/Store/Browse.cshtml), aby wyświetlić informacje o gatunku.</span><span class="sxs-lookup"><span data-stu-id="8addd-199">Update the &lt;h2&gt; element in the view code (in /Views/Store/Browse.cshtml) to display the Genre information.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample15.cshtml)]

<span data-ttu-id="8addd-200">Teraz ponownie uruchom nasz projekt i przejdź do/Store/Browse? Gatunek = adres URL Disco.</span><span class="sxs-lookup"><span data-stu-id="8addd-200">Now let's re-run our project and browse to the /Store/Browse?Genre=Disco URL.</span></span> <span data-ttu-id="8addd-201">Zobaczysz stronę przeglądania, która wygląda jak poniżej.</span><span class="sxs-lookup"><span data-stu-id="8addd-201">We'll see the Browse page displayed like below.</span></span>

![](mvc-music-store-part-3/_static/image16.png)

<span data-ttu-id="8addd-202">Na koniec Przyjrzyjmy nieco bardziej skomplikowaną aktualizację do metody akcji **indeksu magazynu** i widoku, aby wyświetlić listę wszystkich gatunków w naszym sklepie.</span><span class="sxs-lookup"><span data-stu-id="8addd-202">Finally, let's make a slightly more complex update to the **Store Index** action method and view to display a list of all the Genres in our store.</span></span> <span data-ttu-id="8addd-203">Zajmiemy się tym, korzystając z listy gatunków jako obiektu modelu, a nie tylko jednego gatunku.</span><span class="sxs-lookup"><span data-stu-id="8addd-203">We'll do that by using a List of Genres as our model object, rather than just a single Genre.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample16.cs)]

<span data-ttu-id="8addd-204">Kliknij prawym przyciskiem myszy metodę akcji indeks magazynu i wybierz opcję Dodaj widok jako poprzednio, wybierz gatunek jako klasę modelu i naciśnij przycisk Dodaj.</span><span class="sxs-lookup"><span data-stu-id="8addd-204">Right-click in the Store Index action method and select Add View as before, select Genre as the Model class, and press the Add button.</span></span>

![](mvc-music-store-part-3/_static/image17.png)

<span data-ttu-id="8addd-205">Najpierw zmienimy deklarację @model, aby wskazać, że widok będzie oczekiwać kilku obiektów gatunku, a nie tylko jednego.</span><span class="sxs-lookup"><span data-stu-id="8addd-205">First we'll change the @model declaration to indicate that the view will be expecting several Genre objects rather than just one.</span></span> <span data-ttu-id="8addd-206">Zmień pierwszy wiersz/Store/Index.cshtml w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="8addd-206">Change the first line of /Store/Index.cshtml to read as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample17.cshtml)]

<span data-ttu-id="8addd-207">Oznacza to, że aparat widoku Razor będzie pracował z obiektem modelu, który może zawierać kilka obiektów gatunku.</span><span class="sxs-lookup"><span data-stu-id="8addd-207">This tells the Razor view engine that it will be working with a model object that can hold several Genre objects.</span></span> <span data-ttu-id="8addd-208">Używamy gatunku IEnumerable&lt;&gt;, a nie listy&lt;&gt; gatunku, ponieważ jest to bardziej ogólny, dzięki czemu możemy zmienić nasz Typ modelu później na dowolny typ obiektu, który obsługuje interfejs IEnumerable.</span><span class="sxs-lookup"><span data-stu-id="8addd-208">We're using an IEnumerable&lt;Genre&gt; rather than a List&lt;Genre&gt; since it's more generic, allowing us to change our model type later to any object type that supports the IEnumerable interface.</span></span>

<span data-ttu-id="8addd-209">Następnie będziemy przepętlać przez obiekty gatunku w modelu, jak pokazano w poniższym kodzie widoku poniżej.</span><span class="sxs-lookup"><span data-stu-id="8addd-209">Next, we'll loop through the Genre objects in the model as shown in the completed view code below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample18.cshtml)]

<span data-ttu-id="8addd-210">Zwróć uwagę, że firma Microsoft zapewnia pełną obsługę technologii IntelliSense, ponieważ wprowadzamy ten kod, więc w przypadku wpisania "@Model".</span><span class="sxs-lookup"><span data-stu-id="8addd-210">Notice that we have full IntelliSense support as we enter this code, so that when we type "@Model."</span></span> <span data-ttu-id="8addd-211">zobaczymy wszystkie metody i właściwości obsługiwane przez interfejs IEnumerable typu gatunek.</span><span class="sxs-lookup"><span data-stu-id="8addd-211">we see all methods and properties supported by an IEnumerable of type Genre.</span></span>

![](mvc-music-store-part-3/_static/image18.png)

<span data-ttu-id="8addd-212">W naszej pętli "foreach" Visual Web Developer wie, że każdy element jest typu gatunek, więc dla każdego typu gatunku widzimy funkcję IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="8addd-212">Within our "foreach" loop, Visual Web Developer knows that each item is of type Genre, so we see IntelliSense for each the Genre type.</span></span>

![](mvc-music-store-part-3/_static/image19.png)

<span data-ttu-id="8addd-213">Następnie funkcja tworzenia szkieletów bada obiekt gatunku i stwierdził, że każdy z nich będzie miał Właściwość Name, dlatego pętle i zapisuje je. Generuje również linki Edytuj, szczegóły i Usuń do każdego pojedynczego elementu.</span><span class="sxs-lookup"><span data-stu-id="8addd-213">Next, the scaffolding feature examined the Genre object and determined that each will have a Name property, so it loops through and writes them out. It also generates Edit, Details, and Delete links to each individual item.</span></span> <span data-ttu-id="8addd-214">Będziemy korzystać z tej usługi później w naszym Menedżerze sklepu, ale teraz będziemy mieć prostą listę.</span><span class="sxs-lookup"><span data-stu-id="8addd-214">We'll take advantage of that later in our store manager, but for now we'd like to have a simple list instead.</span></span>

<span data-ttu-id="8addd-215">Gdy uruchomimy aplikację i przejdziesz do/Store, zobaczymy, że jest wyświetlana zarówno liczba, jak i Lista gatunków.</span><span class="sxs-lookup"><span data-stu-id="8addd-215">When we run the application and browse to /Store, we see that both the count and list of Genres is displayed.</span></span>

![](mvc-music-store-part-3/_static/image20.png)

## <a name="adding-links-between-pages"></a><span data-ttu-id="8addd-216">Dodawanie linków między stronami</span><span class="sxs-lookup"><span data-stu-id="8addd-216">Adding Links between pages</span></span>

<span data-ttu-id="8addd-217">Nasz adres URL/Store, który zawiera listę gatunku, w postaci zwykłego tekstu.</span><span class="sxs-lookup"><span data-stu-id="8addd-217">Our /Store URL that lists Genres currently lists the Genre names simply as plain text.</span></span> <span data-ttu-id="8addd-218">Zmieńmy to w taki sposób, aby zamiast zwykłego tekstu zamiast tego był miał połączenie z odpowiednim adresem URL/Store/Browse, dzięki czemu kliknięcie gatunku muzycznego, takiego jak "Disco", spowoduje przejście do adresu URL/Store/Browse? gatunek = Disco.</span><span class="sxs-lookup"><span data-stu-id="8addd-218">Let's change this so that instead of plain text we instead have the Genre names link to the appropriate /Store/Browse URL, so that clicking on a music genre like "Disco" will navigate to the /Store/Browse?genre=Disco URL.</span></span> <span data-ttu-id="8addd-219">Możemy zaktualizować nasz szablon widoku \Views\Store\Index.cshtml, aby wyprowadził te linki przy użyciu kodu, takiego jak poniżej **(nie należy wpisywać tego polecenia w trakcie działania)** :</span><span class="sxs-lookup"><span data-stu-id="8addd-219">We could update our \Views\Store\Index.cshtml View template to output these links using code like below **(don't type this in - we're going to improve on it)**:</span></span>

[!code-html[Main](mvc-music-store-part-3/samples/sample19.html)]

<span data-ttu-id="8addd-220">To działa, ale może prowadzić do późniejszego problemu, ponieważ opiera się on na ciągu stałe.</span><span class="sxs-lookup"><span data-stu-id="8addd-220">That works, but it could lead to trouble later since it relies on a hardcoded string.</span></span> <span data-ttu-id="8addd-221">Na przykład jeśli chcemy zmienić nazwę kontrolera, musimy przeszukać nasz kod szukający linków, które wymagają aktualizacji.</span><span class="sxs-lookup"><span data-stu-id="8addd-221">For instance, if we wanted to rename the Controller, we'd need to search through our code looking for links that need to be updated.</span></span>

<span data-ttu-id="8addd-222">Alternatywną metodą, której można użyć, jest skorzystanie z metody pomocnika HTML.</span><span class="sxs-lookup"><span data-stu-id="8addd-222">An alternative approach we can use is to take advantage of an HTML Helper method.</span></span> <span data-ttu-id="8addd-223">ASP.NET MVC zawiera metody pomocników HTML, które są dostępne w naszym kodzie szablonu widoku do wykonywania różnych typowych zadań w podobny sposób.</span><span class="sxs-lookup"><span data-stu-id="8addd-223">ASP.NET MVC includes HTML Helper methods which are available from our View template code to perform a variety of common tasks just like this.</span></span> <span data-ttu-id="8addd-224">Metoda pomocnika html. ActionLink () jest szczególnie przydatna i ułatwia tworzenie kodu HTML &lt;&gt; linków i bierze pod uwagę irytujące szczegóły, takie jak upewnienie się, że ścieżki URL są poprawnie zakodowane w adresie URL.</span><span class="sxs-lookup"><span data-stu-id="8addd-224">The Html.ActionLink() helper method is a particularly useful one, and makes it easy to build HTML &lt;a&gt; links and takes care of annoying details like making sure URL paths are properly URL encoded.</span></span>

<span data-ttu-id="8addd-225">Plik HTML. ActionLink () ma kilka różnych przeciążeń, aby można było określić tyle informacji, ile potrzebujesz w przypadku linków.</span><span class="sxs-lookup"><span data-stu-id="8addd-225">Html.ActionLink() has several different overloads to allow specifying as much information as you need for your links.</span></span> <span data-ttu-id="8addd-226">W najprostszym przypadku należy podać tylko tekst łącza i metodę akcji, aby przejść do po kliknięciu hiperlinku na kliencie.</span><span class="sxs-lookup"><span data-stu-id="8addd-226">In the simplest case, you'll supply just the link text and the Action method to go to when the hyperlink is clicked on the client.</span></span> <span data-ttu-id="8addd-227">Można na przykład połączyć się z metodą "/Store/" index () na stronie szczegółów sklepu z tekstem linku "przejdź do indeksu magazynu" przy użyciu następującego wywołania:</span><span class="sxs-lookup"><span data-stu-id="8addd-227">For example, we can link to "/Store/" Index() method on the Store Details page with the link text "Go to the Store Index" using the following call:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample20.cshtml)]

<span data-ttu-id="8addd-228">*Uwaga: w tym przypadku nie musimy określić nazwy kontrolera, ponieważ po prostu łączysz się z inną akcją w ramach tego samego kontrolera, który renderuje bieżący widok.*</span><span class="sxs-lookup"><span data-stu-id="8addd-228">*Note: In this case, we didn't need to specify the controller name because we're just linking to another action within the same controller that's rendering the current view.*</span></span>

<span data-ttu-id="8addd-229">Nasze linki do strony przeglądania muszą przekazać parametr, mimo że będziemy używać innego przeciążenia metody html. ActionLink, która przyjmuje trzy parametry:</span><span class="sxs-lookup"><span data-stu-id="8addd-229">Our links to the Browse page will need to pass a parameter, though, so we'll use another overload of the Html.ActionLink method that takes three parameters:</span></span>

- 1. <span data-ttu-id="8addd-230">Tekst łącza, który będzie wyświetlał nazwę gatunku</span><span class="sxs-lookup"><span data-stu-id="8addd-230">Link text, which will display the Genre name</span></span>
- 2. <span data-ttu-id="8addd-231">Nazwa akcji kontrolera (Przeglądaj)</span><span class="sxs-lookup"><span data-stu-id="8addd-231">Controller action name (Browse)</span></span>
- 3. <span data-ttu-id="8addd-232">Wartości parametrów trasy, określające zarówno nazwę (gatunek), jak i wartość (nazwę gatunku)</span><span class="sxs-lookup"><span data-stu-id="8addd-232">Route parameter values, specifying both the name (Genre) and the value (Genre name)</span></span>

<span data-ttu-id="8addd-233">W tym celu należy napisać wszystkie te linki do widoku indeksu magazynu:</span><span class="sxs-lookup"><span data-stu-id="8addd-233">Putting that all together, here's how we'll write those links to the Store Index view:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample21.cshtml)]

<span data-ttu-id="8addd-234">Teraz po ponownym uruchomieniu projektu i otrzymaniu dostępu do adresu URL/Store/zostanie wyświetlona lista gatunku.</span><span class="sxs-lookup"><span data-stu-id="8addd-234">Now when we run our project again and access the /Store/ URL we will see a list of genres.</span></span> <span data-ttu-id="8addd-235">Każdy gatunek jest hiperłączem — po jego kliknięciu zajmiemy nas/Store/Browse? gatunek = *[Gatunek]* URL.</span><span class="sxs-lookup"><span data-stu-id="8addd-235">Each genre is a hyperlink – when clicked it will take us to our /Store/Browse?genre=*[genre]* URL.</span></span>

![](mvc-music-store-part-3/_static/image3.jpg)

<span data-ttu-id="8addd-236">KOD HTML dla listy gatunek wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="8addd-236">The HTML for the genre list looks like this:</span></span>

[!code-html[Main](mvc-music-store-part-3/samples/sample22.html)]

> [!div class="step-by-step"]
> <span data-ttu-id="8addd-237">[Poprzednie](mvc-music-store-part-2.md)
> [dalej](mvc-music-store-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="8addd-237">[Previous](mvc-music-store-part-2.md)
[Next](mvc-music-store-part-4.md)</span></span>

---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
title: 'Część 4: modele i dostęp do danych | Microsoft Docs'
author: jongalloway
description: Ta seria samouczków zawiera szczegółowe informacje na temat wszystkich kroków podjętych w celu skompilowania przykładowej aplikacji do sklepu ASP.NET MVC Music. Część 4 obejmuje modele i dostęp do danych.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: ab55ca81-ab9b-44a0-8700-dc6da2599335
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
msc.type: authoredcontent
ms.openlocfilehash: 402be340f1ea3344675e7b859cea8c5130cfc8ee
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78559676"
---
# <a name="part-4-models-and-data-access"></a><span data-ttu-id="b3c28-104">Część 4. Modele i dostęp do danych</span><span class="sxs-lookup"><span data-stu-id="b3c28-104">Part 4: Models and Data Access</span></span>

<span data-ttu-id="b3c28-105">przez [Jan Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="b3c28-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="b3c28-106">Sklep MVC Music jest aplikacją samouczka, która wprowadza i objaśnia krok po kroku, jak używać ASP.NET MVC i Visual Studio do programowania w sieci Web.</span><span class="sxs-lookup"><span data-stu-id="b3c28-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="b3c28-107">Sklep MVC Music jest lekkim przykładowym wdrożeniem magazynu, który sprzedaje Albumy muzyczne w trybie online i implementuje podstawowe funkcje administracyjne, logowania użytkownika i koszyka.</span><span class="sxs-lookup"><span data-stu-id="b3c28-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>
> 
> <span data-ttu-id="b3c28-108">Ta seria samouczków zawiera szczegółowe informacje na temat wszystkich kroków podjętych w celu skompilowania przykładowej aplikacji do sklepu ASP.NET MVC Music.</span><span class="sxs-lookup"><span data-stu-id="b3c28-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="b3c28-109">Część 4 obejmuje modele i dostęp do danych.</span><span class="sxs-lookup"><span data-stu-id="b3c28-109">Part 4 covers Models and Data Access.</span></span>

<span data-ttu-id="b3c28-110">Do tej pory właśnie przekazano "fikcyjne dane" z naszych kontrolerów do naszego szablonu widoku.</span><span class="sxs-lookup"><span data-stu-id="b3c28-110">So far, we've just been passing "dummy data" from our Controllers to our View templates.</span></span> <span data-ttu-id="b3c28-111">Teraz jesteśmy gotowi do podłączania rzeczywistej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="b3c28-111">Now we're ready to hook up a real database.</span></span> <span data-ttu-id="b3c28-112">W tym samouczku opisano sposób korzystania z wersji SQL Server Compact (często nazywanej standardem SQL CE) jako aparatu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="b3c28-112">In this tutorial we'll be covering how to use SQL Server Compact Edition (often called SQL CE) as our database engine.</span></span> <span data-ttu-id="b3c28-113">SQL CE to bezpłatna, osadzona baza danych oparta na plikach, która nie wymaga żadnej instalacji ani konfiguracji, co sprawia, że jest ona naprawdę wygodna do lokalnego tworzenia.</span><span class="sxs-lookup"><span data-stu-id="b3c28-113">SQL CE is a free, embedded, file based database that doesn't require any installation or configuration, which makes it really convenient for local development.</span></span>

## <a name="database-access-with-entity-framework-code-first"></a><span data-ttu-id="b3c28-114">Dostęp do bazy danych z kodem Entity Framework — pierwszy</span><span class="sxs-lookup"><span data-stu-id="b3c28-114">Database access with Entity Framework Code-First</span></span>

<span data-ttu-id="b3c28-115">Użyjemy obsługi Entity Framework (EF), która jest dołączona do projektów ASP.NET MVC 3 do wykonywania zapytań i aktualizacji bazy danych.</span><span class="sxs-lookup"><span data-stu-id="b3c28-115">We'll use the Entity Framework (EF) support that is included in ASP.NET MVC 3 projects to query and update the database.</span></span> <span data-ttu-id="b3c28-116">Dr to elastyczny interfejs API danych relacyjnego mapowania obiektów (ORM), który umożliwia deweloperom wykonywanie zapytań i aktualizowanie danych przechowywanych w bazie danych w sposób zorientowany obiektowo.</span><span class="sxs-lookup"><span data-stu-id="b3c28-116">EF is a flexible object relational mapping (ORM) data API that enables developers to query and update data stored in a database in an object-oriented way.</span></span>

<span data-ttu-id="b3c28-117">Entity Framework w wersji 4 obsługuje model programistyczny o nazwie Code-First.</span><span class="sxs-lookup"><span data-stu-id="b3c28-117">Entity Framework version 4 supports a development paradigm called code-first.</span></span> <span data-ttu-id="b3c28-118">Kod — w pierwszej kolejności można utworzyć obiekt modelu, pisząc proste klasy (znane również jako POCO z "zwykłych-starych" obiektów CLR) i można nawet utworzyć bazę danych na bieżąco z klas.</span><span class="sxs-lookup"><span data-stu-id="b3c28-118">Code-first allows you to create model object by writing simple classes (also known as POCO from "plain-old" CLR objects), and can even create the database on the fly from your classes.</span></span>

### <a name="changes-to-our-model-classes"></a><span data-ttu-id="b3c28-119">Zmiany naszych klas modelu</span><span class="sxs-lookup"><span data-stu-id="b3c28-119">Changes to our Model Classes</span></span>

<span data-ttu-id="b3c28-120">W ramach tego samouczka będziemy korzystać z funkcji tworzenia bazy danych w Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="b3c28-120">We will be leveraging the database creation feature in Entity Framework in this tutorial.</span></span> <span data-ttu-id="b3c28-121">Przed wykonaniem tej czynności należy wprowadzić kilka drobnych zmian w naszych klasach modelu w celu dodania ich do innych elementów, które będą używane później.</span><span class="sxs-lookup"><span data-stu-id="b3c28-121">Before we do that, though, let's make a few minor changes to our model classes to add in some things we'll be using later on.</span></span>

#### <a name="adding-the-artist-model-classes"></a><span data-ttu-id="b3c28-122">Dodawanie klas modelu wykonawcy</span><span class="sxs-lookup"><span data-stu-id="b3c28-122">Adding the Artist Model Classes</span></span>

<span data-ttu-id="b3c28-123">Nasze albumy zostaną skojarzone z artyści, dlatego dodamy prostą klasę modelu do opisania wykonawcy.</span><span class="sxs-lookup"><span data-stu-id="b3c28-123">Our Albums will be associated with Artists, so we'll add a simple model class to describe an Artist.</span></span> <span data-ttu-id="b3c28-124">Dodaj nową klasę do folderu models o nazwie Artist.cs przy użyciu kodu pokazanego poniżej.</span><span class="sxs-lookup"><span data-stu-id="b3c28-124">Add a new class to the Models folder named Artist.cs using the code shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample1.cs)]

#### <a name="updating-our-model-classes"></a><span data-ttu-id="b3c28-125">Aktualizowanie naszych klas modelu</span><span class="sxs-lookup"><span data-stu-id="b3c28-125">Updating our Model Classes</span></span>

<span data-ttu-id="b3c28-126">Zaktualizuj klasę albumu, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="b3c28-126">Update the Album class as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample2.cs)]

<span data-ttu-id="b3c28-127">Następnie wprowadź następujące aktualizacje klasy gatunek.</span><span class="sxs-lookup"><span data-stu-id="b3c28-127">Next, make the following updates to the Genre class.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample3.cs)]

### <a name="adding-the-app_data-folder"></a><span data-ttu-id="b3c28-128">Dodawanie folderu danych\_aplikacji</span><span class="sxs-lookup"><span data-stu-id="b3c28-128">Adding the App\_Data folder</span></span>

<span data-ttu-id="b3c28-129">Dodamy\_katalogu danych aplikacji do naszego projektu, aby przechowywać pliki bazy danych SQL Server Express.</span><span class="sxs-lookup"><span data-stu-id="b3c28-129">We'll add an App\_Data directory to our project to hold our SQL Server Express database files.</span></span> <span data-ttu-id="b3c28-130">\_danych aplikacji jest specjalnym katalogiem w ASP.NET, który ma już odpowiednie uprawnienia dostępu do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="b3c28-130">App\_Data is a special directory in ASP.NET which already has the correct security access permissions for database access.</span></span> <span data-ttu-id="b3c28-131">W menu Projekt wybierz pozycję Dodaj folder ASP.NET, a następnie kliknij pozycję Aplikacja\_dane.</span><span class="sxs-lookup"><span data-stu-id="b3c28-131">From the Project menu, select Add ASP.NET Folder, then App\_Data.</span></span>

![](mvc-music-store-part-4/_static/image1.png)

### <a name="creating-a-connection-string-in-the-webconfig-file"></a><span data-ttu-id="b3c28-132">Tworzenie parametrów połączenia w pliku Web. config</span><span class="sxs-lookup"><span data-stu-id="b3c28-132">Creating a Connection String in the web.config file</span></span>

<span data-ttu-id="b3c28-133">Dodamy kilka wierszy do pliku konfiguracji witryny sieci Web, aby Entity Framework wie, jak nawiązać połączenie z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="b3c28-133">We will add a few lines to the website's configuration file so that Entity Framework knows how to connect to our database.</span></span> <span data-ttu-id="b3c28-134">Kliknij dwukrotnie plik Web. config znajdujący się w katalogu głównym projektu.</span><span class="sxs-lookup"><span data-stu-id="b3c28-134">Double-click on the Web.config file located in the root of the project.</span></span>

![](mvc-music-store-part-4/_static/image2.png)

<span data-ttu-id="b3c28-135">Przewiń w dół tego pliku i Dodaj sekcję &lt;connectionStrings&gt; bezpośrednio nad ostatnim wierszem, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="b3c28-135">Scroll to the bottom of this file and add a &lt;connectionStrings&gt; section directly above the last line, as shown below.</span></span>

[!code-xml[Main](mvc-music-store-part-4/samples/sample4.xml)]

### <a name="adding-a-context-class"></a><span data-ttu-id="b3c28-136">Dodawanie klasy kontekstu</span><span class="sxs-lookup"><span data-stu-id="b3c28-136">Adding a Context Class</span></span>

<span data-ttu-id="b3c28-137">Kliknij prawym przyciskiem myszy folder modele i Dodaj nową klasę o nazwie MusicStoreEntities.cs.</span><span class="sxs-lookup"><span data-stu-id="b3c28-137">Right-click the Models folder and add a new class named MusicStoreEntities.cs.</span></span>

![](mvc-music-store-part-4/_static/image3.png)

<span data-ttu-id="b3c28-138">Ta klasa będzie reprezentować kontekst bazy danych Entity Framework i będzie obsługiwać nasze operacje tworzenia, odczytywania, aktualizowania i usuwania.</span><span class="sxs-lookup"><span data-stu-id="b3c28-138">This class will represent the Entity Framework database context, and will handle our create, read, update, and delete operations for us.</span></span> <span data-ttu-id="b3c28-139">Kod dla tej klasy pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="b3c28-139">The code for this class is shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample5.cs)]

<span data-ttu-id="b3c28-140">To nie istnieje inna konfiguracja, interfejsy specjalne itp. Rozszerzając klasę bazową DbContext, Nasza Klasa MusicStoreEntities może obsługiwać nasze operacje bazy danych.</span><span class="sxs-lookup"><span data-stu-id="b3c28-140">That's it - there's no other configuration, special interfaces, etc. By extending the DbContext base class, our MusicStoreEntities class is able to handle our database operations for us.</span></span> <span data-ttu-id="b3c28-141">Teraz, gdy mamy już podłączane, dodajmy kilka więcej właściwości do naszych klas modelu, aby korzystać z niektórych dodatkowych informacji w naszej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="b3c28-141">Now that we've got that hooked up, let's add a few more properties to our model classes to take advantage of some of the additional information in our database.</span></span>

### <a name="adding-our-store-catalog-data"></a><span data-ttu-id="b3c28-142">Dodawanie naszych danych katalogu sklepu</span><span class="sxs-lookup"><span data-stu-id="b3c28-142">Adding our store catalog data</span></span>

<span data-ttu-id="b3c28-143">Będziemy korzystać z funkcji w Entity Framework, która dodaje dane "inicjatora" do nowo utworzonej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="b3c28-143">We will take advantage of a feature in Entity Framework which adds "seed" data to a newly created database.</span></span> <span data-ttu-id="b3c28-144">Spowoduje to wstępne wypełnienie naszego katalogu sklepu listą gatunków, artystów i albumów.</span><span class="sxs-lookup"><span data-stu-id="b3c28-144">This will pre-populate our store catalog with a list of Genres, Artists, and Albums.</span></span> <span data-ttu-id="b3c28-145">Pobieranie pliku MvcMusicStore-Assets. zip — który zawiera pliki projektu witryny używane wcześniej w tym samouczku — zawiera plik klasy z danymi tego inicjatora znajdującą się w folderze o nazwie Code.</span><span class="sxs-lookup"><span data-stu-id="b3c28-145">The MvcMusicStore-Assets.zip download - which included our site design files used earlier in this tutorial - has a class file with this seed data, located in a folder named Code.</span></span>

<span data-ttu-id="b3c28-146">W folderze Code/models Znajdź plik SampleData.cs i upuść go do folderu models w naszym projekcie, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="b3c28-146">Within the Code / Models folder, locate the SampleData.cs file and drop it into the Models folder in our project, as shown below.</span></span>

![](mvc-music-store-part-4/_static/image4.png)

<span data-ttu-id="b3c28-147">Teraz musimy dodać jeden wiersz kodu, aby poinformować Entity Framework o tej klasie SampleData.</span><span class="sxs-lookup"><span data-stu-id="b3c28-147">Now we need to add one line of code to tell Entity Framework about that SampleData class.</span></span> <span data-ttu-id="b3c28-148">Kliknij dwukrotnie plik Global. asax w folderze głównym projektu, aby go otworzyć, a następnie Dodaj następujący wiersz do początku metody\_uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b3c28-148">Double-click on the Global.asax file in the root of the project to open it and add the following line to the top the Application\_Start method.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample6.cs)]

<span data-ttu-id="b3c28-149">W tym momencie wykonamy czynności niezbędne do skonfigurowania Entity Framework dla naszego projektu.</span><span class="sxs-lookup"><span data-stu-id="b3c28-149">At this point, we've completed the work necessary to configure Entity Framework for our project.</span></span>

## <a name="querying-the-database"></a><span data-ttu-id="b3c28-150">wykonywanie zapytania w bazie danych</span><span class="sxs-lookup"><span data-stu-id="b3c28-150">Querying the Database</span></span>

<span data-ttu-id="b3c28-151">Teraz zaktualizujmy nasze StoreController, tak aby zamiast korzystania z "fikcyjnych danych" zamiast tego wywoływać do naszej bazy danych zapytania dotyczące wszystkich informacji.</span><span class="sxs-lookup"><span data-stu-id="b3c28-151">Now let's update our StoreController so that instead of using "dummy data" it instead calls into our database to query all of its information.</span></span> <span data-ttu-id="b3c28-152">Zaczniemy od zadeklarowania pola na **StoreController** , aby pomieścić wystąpienie klasy MusicStoreEntities o nazwie storeDB:</span><span class="sxs-lookup"><span data-stu-id="b3c28-152">We'll start by declaring a field on the **StoreController** to hold an instance of the MusicStoreEntities class, named storeDB:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample7.cs)]

### <a name="updating-the-store-index-to-query-the-database"></a><span data-ttu-id="b3c28-153">Aktualizowanie indeksu magazynu w celu wysyłania zapytań do bazy danych</span><span class="sxs-lookup"><span data-stu-id="b3c28-153">Updating the Store Index to query the database</span></span>

<span data-ttu-id="b3c28-154">Klasa MusicStoreEntities jest obsługiwana przez Entity Framework i uwidacznia Właściwość kolekcji dla każdej tabeli w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="b3c28-154">The MusicStoreEntities class is maintained by the Entity Framework and exposes a collection property for each table in our database.</span></span> <span data-ttu-id="b3c28-155">Zaktualizujmy akcję indeksu StoreController w celu pobrania wszystkich gatunków w naszej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="b3c28-155">Let's update our StoreController's Index action to retrieve all Genres in our database.</span></span> <span data-ttu-id="b3c28-156">Wcześniej było to spowodowane przez stałe kodowanie danych ciągu.</span><span class="sxs-lookup"><span data-stu-id="b3c28-156">Previously we did this by hard-coding string data.</span></span> <span data-ttu-id="b3c28-157">Teraz możemy używać kolekcji generes kontekstu Entity Framework:</span><span class="sxs-lookup"><span data-stu-id="b3c28-157">Now we can instead just use the Entity Framework context Generes collection:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample8.cs)]

<span data-ttu-id="b3c28-158">W naszym szablonie widoku nie trzeba zmieniać żadnych zmian, ponieważ nadal zwracamy ten sam StoreIndexViewModel, który zwróciłeś wcześniej — teraz zwracamy dane na żywo z naszej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="b3c28-158">No changes need to happen to our View template since we're still returning the same StoreIndexViewModel we returned before - we're just returning live data from our database now.</span></span>

<span data-ttu-id="b3c28-159">Po ponownym uruchomieniu projektu i odwiedzeniu adresu URL "/Store" zostanie teraz wyświetlona lista wszystkich gatunków w bazie danych:</span><span class="sxs-lookup"><span data-stu-id="b3c28-159">When we run the project again and visit the "/Store" URL, we'll now see a list of all Genres in our database:</span></span>

![](mvc-music-store-part-4/_static/image1.jpg)

### <a name="updating-store-browse-and-details-to-use-live-data"></a><span data-ttu-id="b3c28-160">Aktualizowanie przeglądania i szczegółów sklepu w celu korzystania z danych na żywo</span><span class="sxs-lookup"><span data-stu-id="b3c28-160">Updating Store Browse and Details to use live data</span></span>

<span data-ttu-id="b3c28-161">Za pomocą metody akcji/Store/Browse? gatunek = *[część-gatunek]* szukamy gatunku według nazwy.</span><span class="sxs-lookup"><span data-stu-id="b3c28-161">With the /Store/Browse?genre=*[some-genre]* action method, we're searching for a Genre by name.</span></span> <span data-ttu-id="b3c28-162">Oczekiwany jest tylko jeden wynik, ponieważ nie będziemy musieli mieć dwóch wpisów dla tej samej nazwy gatunku i dlatego możemy użyć. Pojedyncze () rozszerzenie w LINQ do zapytania dla odpowiedniego obiektu gatunku, takiego jak ten (nie należy jeszcze wpisywać):</span><span class="sxs-lookup"><span data-stu-id="b3c28-162">We only expect one result, since we shouldn't ever have two entries for the same Genre name, and so we can use the .Single() extension in LINQ to query for the appropriate Genre object like this (don't type this yet):</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample9.cs)]

<span data-ttu-id="b3c28-163">Pojedyncza Metoda przyjmuje wyrażenie lambda jako parametr, który określa, że chcemy, aby obiekt o pojedynczym gatunku był zgodny z określoną wartością.</span><span class="sxs-lookup"><span data-stu-id="b3c28-163">The Single method takes a Lambda expression as a parameter, which specifies that we want a single Genre object such that its name matches the value we've defined.</span></span> <span data-ttu-id="b3c28-164">W powyższym przypadku ładujemy pojedynczy obiekt typu gatunek z wartością nazwy zgodną z disco.</span><span class="sxs-lookup"><span data-stu-id="b3c28-164">In the case above, we are loading a single Genre object with a Name value matching Disco.</span></span>

<span data-ttu-id="b3c28-165">Będziemy korzystać z funkcji Entity Framework, która umożliwia wskazanie innych powiązanych jednostek, które chcemy załadować, a także po pobraniu obiektu gatunku.</span><span class="sxs-lookup"><span data-stu-id="b3c28-165">We'll take advantage of an Entity Framework feature that allows us to indicate other related entities we want loaded as well when the Genre object is retrieved.</span></span> <span data-ttu-id="b3c28-166">Ta funkcja jest nazywana kształtem wyników zapytania i umożliwia nam skrócenie liczby potrzeb dostępu do bazy danych w celu pobrania wszystkich potrzebnych informacji.</span><span class="sxs-lookup"><span data-stu-id="b3c28-166">This feature is called Query Result Shaping, and enables us to reduce the number of times we need to access the database to retrieve all of the information we need.</span></span> <span data-ttu-id="b3c28-167">Chcemy wstępnie pobrać albumy dla gatunku, który pobieramy, dlatego będziemy aktualizować zapytanie w celu uwzględnienia z gatunku. Dołącz ("albumy"), aby wskazać, że chcemy również mieć powiązane albumy.</span><span class="sxs-lookup"><span data-stu-id="b3c28-167">We want to pre-fetch the Albums for Genre we retrieve, so we'll update our query to include from Genres.Include("Albums") to indicate that we want related albums as well.</span></span> <span data-ttu-id="b3c28-168">Jest to bardziej wydajne, ponieważ spowoduje to pobranie zarówno naszego gatunku, jak i danych z albumu w ramach pojedynczego żądania bazy danych.</span><span class="sxs-lookup"><span data-stu-id="b3c28-168">This is more efficient, since it will retrieve both our Genre and Album data in a single database request.</span></span>

<span data-ttu-id="b3c28-169">Dzięki wyjaśnieniu z metody, Oto jak wygląda zaktualizowana akcja przeglądania kontrolera:</span><span class="sxs-lookup"><span data-stu-id="b3c28-169">With the explanations out of the way, here's how our updated Browse controller action looks:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample10.cs)]

<span data-ttu-id="b3c28-170">Teraz można zaktualizować Widok przeglądania sklepu, aby wyświetlić albumy, które są dostępne w każdym gatunku.</span><span class="sxs-lookup"><span data-stu-id="b3c28-170">We can now update the Store Browse View to display the albums which are available in each Genre.</span></span> <span data-ttu-id="b3c28-171">Otwórz szablon widoku (znaleziony w/Views/Store/Browse.cshtml) i Dodaj listę punktowaną z listy Albumy, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="b3c28-171">Open the view template (found in /Views/Store/Browse.cshtml) and add a bulleted list of Albums as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample11.cshtml)]

<span data-ttu-id="b3c28-172">Uruchamiając aplikację i przechodząc do/Store/Browse? gatunek = Jazz pokazuje, że nasze wyniki są teraz ściągane z bazy danych, wyświetlając wszystkie albumy w naszym wybranym gatunku.</span><span class="sxs-lookup"><span data-stu-id="b3c28-172">Running our application and browsing to /Store/Browse?genre=Jazz shows that our results are now being pulled from the database, displaying all albums in our selected Genre.</span></span>

![](mvc-music-store-part-4/_static/image2.jpg)

<span data-ttu-id="b3c28-173">Wprowadzimy tę samą zmianę w adresie URL/Store/Details/[ID] i zamienimy nasze fikcyjne dane z zapytaniem bazy danych, które ładuje album, którego identyfikator pasuje do wartości parametru.</span><span class="sxs-lookup"><span data-stu-id="b3c28-173">We'll make the same change to our /Store/Details/[id] URL, and replace our dummy data with a database query which loads an Album whose ID matches the parameter value.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample12.cs)]

<span data-ttu-id="b3c28-174">Uruchomienie naszej aplikacji i przechodzenie do/Store/Details/1 pokazuje, że nasze wyniki są teraz ściągane z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="b3c28-174">Running our application and browsing to /Store/Details/1 shows that our results are now being pulled from the database.</span></span>

![](mvc-music-store-part-4/_static/image5.png)

<span data-ttu-id="b3c28-175">Teraz, gdy strona szczegółów sklepu została skonfigurowana do wyświetlania albumu przez identyfikator albumu, zaktualizujmy widok **przeglądania** , aby połączyć się z widokiem szczegółów.</span><span class="sxs-lookup"><span data-stu-id="b3c28-175">Now that our Store Details page is set up to display an album by the Album ID, let's update the **Browse** view to link to the Details view.</span></span> <span data-ttu-id="b3c28-176">Będziemy używać języka HTML. ActionLink dokładnie tak samo jak w przypadku linków z indeksu magazynu, aby przechować wyszukiwanie na końcu poprzedniej sekcji.</span><span class="sxs-lookup"><span data-stu-id="b3c28-176">We will use Html.ActionLink, exactly as we did to link from Store Index to Store Browse at the end of the previous section.</span></span> <span data-ttu-id="b3c28-177">Pełne Źródło dla widoku przeglądania jest wyświetlane poniżej.</span><span class="sxs-lookup"><span data-stu-id="b3c28-177">The complete source for the Browse view appears below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample13.cshtml)]

<span data-ttu-id="b3c28-178">Teraz można przeglądać stronę ze swoim sklepem na stronie z gatunku, która zawiera listę dostępnych albumów, a następnie klikając album, aby wyświetlić szczegółowe informacje o tym albumie.</span><span class="sxs-lookup"><span data-stu-id="b3c28-178">We're now able to browse from our Store page to a Genre page, which lists the available albums, and by clicking on an album we can view details for that album.</span></span>

![](mvc-music-store-part-4/_static/image6.png)

> [!div class="step-by-step"]
> <span data-ttu-id="b3c28-179">[Poprzednie](mvc-music-store-part-3.md)
> [dalej](mvc-music-store-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="b3c28-179">[Previous](mvc-music-store-part-3.md)
[Next](mvc-music-store-part-5.md)</span></span>

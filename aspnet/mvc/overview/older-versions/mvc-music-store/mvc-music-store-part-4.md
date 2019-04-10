---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
title: Część 4. Modele i dostęp do danych | Dokumentacja firmy Microsoft
author: jongalloway
description: W tej serii samouczków szczegółowo opisuje wszystkie etapy, tworzenie przykładowej aplikacji platformy ASP.NET MVC Music Store. Część 4 obejmuje modele i dostęp do danych.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: ab55ca81-ab9b-44a0-8700-dc6da2599335
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
msc.type: authoredcontent
ms.openlocfilehash: 40fec3a2ef4ee8d5e4abe4be4dfa144720a88a41
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/09/2019
ms.locfileid: "59391185"
---
# <a name="part-4-models-and-data-access"></a><span data-ttu-id="4dfb7-104">Część 4. Modele i dostęp do danych</span><span class="sxs-lookup"><span data-stu-id="4dfb7-104">Part 4: Models and Data Access</span></span>

<span data-ttu-id="4dfb7-105">przez [Galloway'em Jon](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="4dfb7-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="4dfb7-106">MVC Music Store jest aplikacją z samouczka, który wprowadzono i opisano krok po kroku, jak używać platformy ASP.NET MVC i programu Visual Studio do tworzenia aplikacji internetowych.</span><span class="sxs-lookup"><span data-stu-id="4dfb7-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="4dfb7-107">MVC Music Store jest uproszczone przykładową implementację magazynu sprzedaje utworów muzycznych albumy online, która implementuje podstawowej witryny administracji, logowania użytkownika i funkcje koszyka zakupów.</span><span class="sxs-lookup"><span data-stu-id="4dfb7-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>
> 
> <span data-ttu-id="4dfb7-108">W tej serii samouczków szczegółowo opisuje wszystkie etapy, tworzenie przykładowej aplikacji platformy ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="4dfb7-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="4dfb7-109">Część 4 obejmuje modele i dostęp do danych.</span><span class="sxs-lookup"><span data-stu-id="4dfb7-109">Part 4 covers Models and Data Access.</span></span>


<span data-ttu-id="4dfb7-110">Do tej pory firma Microsoft została tylko zostały przekazywanie "fikcyjne dane" z naszych kontrolerów do nasze szablony widoku.</span><span class="sxs-lookup"><span data-stu-id="4dfb7-110">So far, we've just been passing "dummy data" from our Controllers to our View templates.</span></span> <span data-ttu-id="4dfb7-111">Teraz jesteśmy gotowi zaczepić rzeczywista baza danych.</span><span class="sxs-lookup"><span data-stu-id="4dfb7-111">Now we're ready to hook up a real database.</span></span> <span data-ttu-id="4dfb7-112">W tym samouczku będzie dotyczyć sposób używania programu SQL Server Compact Edition (często nazywanej SQL CE) jako naszego aparatu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="4dfb7-112">In this tutorial we'll be covering how to use SQL Server Compact Edition (often called SQL CE) as our database engine.</span></span> <span data-ttu-id="4dfb7-113">SQL CE jest bezpłatna, plik osadzony na podstawie bazy danych, która nie wymaga instalacji ani konfiguracji, która sprawia, że naprawdę wygodne do tworzenia aplikacji lokalnej.</span><span class="sxs-lookup"><span data-stu-id="4dfb7-113">SQL CE is a free, embedded, file based database that doesn't require any installation or configuration, which makes it really convenient for local development.</span></span>

## <a name="database-access-with-entity-framework-code-first"></a><span data-ttu-id="4dfb7-114">Dostęp do bazy danych za pomocą Entity Framework najpierw kod</span><span class="sxs-lookup"><span data-stu-id="4dfb7-114">Database access with Entity Framework Code-First</span></span>

<span data-ttu-id="4dfb7-115">Użyjemy obsługi Entity Framework (EF), który znajduje się w projektach ASP.NET MVC 3 do wykonywania zapytań i aktualizacji bazy danych.</span><span class="sxs-lookup"><span data-stu-id="4dfb7-115">We'll use the Entity Framework (EF) support that is included in ASP.NET MVC 3 projects to query and update the database.</span></span> <span data-ttu-id="4dfb7-116">EF jest obiektem elastyczne, relacyjne mapowanie danych (ORM) interfejsu API, który umożliwia deweloperom zapytania i aktualizację danych przechowywanych w bazie danych w sposób zorientowane obiektowo.</span><span class="sxs-lookup"><span data-stu-id="4dfb7-116">EF is a flexible object relational mapping (ORM) data API that enables developers to query and update data stored in a database in an object-oriented way.</span></span>

<span data-ttu-id="4dfb7-117">Entity Framework w wersji 4 obsługuje paradygmat programowania, o nazwie najpierw kod.</span><span class="sxs-lookup"><span data-stu-id="4dfb7-117">Entity Framework version 4 supports a development paradigm called code-first.</span></span> <span data-ttu-id="4dfb7-118">Najpierw kod umożliwia utworzenie obiektu modelu, pisząc klasy proste (znany także jako POCO z obiektów CLR "zwykły stary"), a nawet utworzyć bazy danych na bieżąco z klas.</span><span class="sxs-lookup"><span data-stu-id="4dfb7-118">Code-first allows you to create model object by writing simple classes (also known as POCO from "plain-old" CLR objects), and can even create the database on the fly from your classes.</span></span>

### <a name="changes-to-our-model-classes"></a><span data-ttu-id="4dfb7-119">Zmiany w naszych zajęć modelu</span><span class="sxs-lookup"><span data-stu-id="4dfb7-119">Changes to our Model Classes</span></span>

<span data-ttu-id="4dfb7-120">Firma Microsoft będzie się korzystanie z funkcji tworzenia bazy danych platformy Entity Framework w ramach tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="4dfb7-120">We will be leveraging the database creation feature in Entity Framework in this tutorial.</span></span> <span data-ttu-id="4dfb7-121">Zanim do tego, jednak upewnijmy się kilka drobne zmiany do naszych zajęć modelu, aby dodać niektóre czynności, które będzie używana później.</span><span class="sxs-lookup"><span data-stu-id="4dfb7-121">Before we do that, though, let's make a few minor changes to our model classes to add in some things we'll be using later on.</span></span>

#### <a name="adding-the-artist-model-classes"></a><span data-ttu-id="4dfb7-122">Dodawanie klasy modelu wykonawcy</span><span class="sxs-lookup"><span data-stu-id="4dfb7-122">Adding the Artist Model Classes</span></span>

<span data-ttu-id="4dfb7-123">Nasze albumów, zostanie skojarzona z artyści, dlatego dodamy klasę modelu proste do opisania wykonawcy.</span><span class="sxs-lookup"><span data-stu-id="4dfb7-123">Our Albums will be associated with Artists, so we'll add a simple model class to describe an Artist.</span></span> <span data-ttu-id="4dfb7-124">Dodaj nową klasę do folderu modeli o nazwie Artist.cs przy użyciu kodu pokazany poniżej.</span><span class="sxs-lookup"><span data-stu-id="4dfb7-124">Add a new class to the Models folder named Artist.cs using the code shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample1.cs)]

#### <a name="updating-our-model-classes"></a><span data-ttu-id="4dfb7-125">Aktualizowanie naszych zajęć modelu</span><span class="sxs-lookup"><span data-stu-id="4dfb7-125">Updating our Model Classes</span></span>

<span data-ttu-id="4dfb7-126">Aktualizacja klasy fotograficzne, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="4dfb7-126">Update the Album class as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample2.cs)]

<span data-ttu-id="4dfb7-127">Następnie wprowadź następujące aktualizacje do klasy gatunku.</span><span class="sxs-lookup"><span data-stu-id="4dfb7-127">Next, make the following updates to the Genre class.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample3.cs)]

### <a name="adding-the-appdata-folder"></a><span data-ttu-id="4dfb7-128">Dodawanie aplikacji\_folderu danych</span><span class="sxs-lookup"><span data-stu-id="4dfb7-128">Adding the App\_Data folder</span></span>

<span data-ttu-id="4dfb7-129">Dodamy aplikacji\_katalog danych do naszego projektu do przechowywania plików bazy danych programu SQL Server Express.</span><span class="sxs-lookup"><span data-stu-id="4dfb7-129">We'll add an App\_Data directory to our project to hold our SQL Server Express database files.</span></span> <span data-ttu-id="4dfb7-130">Aplikacja\_dane są specjalne katalogu w programie ASP.NET, który już ma poprawne uprawnienia dostępu dla dostępu do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="4dfb7-130">App\_Data is a special directory in ASP.NET which already has the correct security access permissions for database access.</span></span> <span data-ttu-id="4dfb7-131">Menu projektu i wybierz polecenie Dodaj Folder programu ASP.NET, a następnie aplikacja\_danych.</span><span class="sxs-lookup"><span data-stu-id="4dfb7-131">From the Project menu, select Add ASP.NET Folder, then App\_Data.</span></span>

![](mvc-music-store-part-4/_static/image1.png)

### <a name="creating-a-connection-string-in-the-webconfig-file"></a><span data-ttu-id="4dfb7-132">Tworzenie parametrów połączenia w pliku web.config</span><span class="sxs-lookup"><span data-stu-id="4dfb7-132">Creating a Connection String in the web.config file</span></span>

<span data-ttu-id="4dfb7-133">Dodamy kilka wierszy do pliku konfiguracji witryny internetowej tak aby wie, jak połączyć się z naszym bazy danych platformy Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="4dfb7-133">We will add a few lines to the website's configuration file so that Entity Framework knows how to connect to our database.</span></span> <span data-ttu-id="4dfb7-134">Kliknij dwukrotnie plik Web.config znajduje się w folderze głównym projektu.</span><span class="sxs-lookup"><span data-stu-id="4dfb7-134">Double-click on the Web.config file located in the root of the project.</span></span>

![](mvc-music-store-part-4/_static/image2.png)

<span data-ttu-id="4dfb7-135">Przewiń w dół tego pliku i Dodaj &lt;connectionStrings&gt; sekcji bezpośrednio nad ostatni wiersz, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="4dfb7-135">Scroll to the bottom of this file and add a &lt;connectionStrings&gt; section directly above the last line, as shown below.</span></span>

[!code-xml[Main](mvc-music-store-part-4/samples/sample4.xml)]

### <a name="adding-a-context-class"></a><span data-ttu-id="4dfb7-136">Dodawanie klasy kontekstu</span><span class="sxs-lookup"><span data-stu-id="4dfb7-136">Adding a Context Class</span></span>

<span data-ttu-id="4dfb7-137">Kliknij prawym przyciskiem myszy folderu modeli i Dodaj nową klasę o nazwie MusicStoreEntities.cs.</span><span class="sxs-lookup"><span data-stu-id="4dfb7-137">Right-click the Models folder and add a new class named MusicStoreEntities.cs.</span></span>

![](mvc-music-store-part-4/_static/image3.png)

<span data-ttu-id="4dfb7-138">Tej klasy będzie reprezentują kontekstu bazy danych programu Entity Framework, będzie obsłużyć nasz tworzenia, odczytu, aktualizacji i operacje usuwania dotyczące nam.</span><span class="sxs-lookup"><span data-stu-id="4dfb7-138">This class will represent the Entity Framework database context, and will handle our create, read, update, and delete operations for us.</span></span> <span data-ttu-id="4dfb7-139">Poniżej przedstawiono kod dla tej klasy.</span><span class="sxs-lookup"><span data-stu-id="4dfb7-139">The code for this class is shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample5.cs)]

<span data-ttu-id="4dfb7-140">To wszystko — Brak nie innych konfiguracji specjalnych interfejsów, itp. Rozszerzając klasy bazowej typu DbContext, klasy Nasze MusicStoreEntities będzie mogło obsłużyć nasz operacji bazy danych dla nas.</span><span class="sxs-lookup"><span data-stu-id="4dfb7-140">That's it - there's no other configuration, special interfaces, etc. By extending the DbContext base class, our MusicStoreEntities class is able to handle our database operations for us.</span></span> <span data-ttu-id="4dfb7-141">Skoro mamy podłączyłeś, Dodajmy kilka innych właściwości do naszych zajęć na model, aby móc korzystać z niektórych dodatkowych informacji w naszej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="4dfb7-141">Now that we've got that hooked up, let's add a few more properties to our model classes to take advantage of some of the additional information in our database.</span></span>

### <a name="adding-our-store-catalog-data"></a><span data-ttu-id="4dfb7-142">Dodawanie magazynu danych katalogu</span><span class="sxs-lookup"><span data-stu-id="4dfb7-142">Adding our store catalog data</span></span>

<span data-ttu-id="4dfb7-143">Firma Microsoft będzie korzystać z funkcji platformy Entity Framework, który dodaje dane "seed" nowo utworzoną bazę danych.</span><span class="sxs-lookup"><span data-stu-id="4dfb7-143">We will take advantage of a feature in Entity Framework which adds "seed" data to a newly created database.</span></span> <span data-ttu-id="4dfb7-144">Spowoduje to wstępnie wypełnić nasz katalog sklepu za pomocą listy gatunki, artystów i albumów.</span><span class="sxs-lookup"><span data-stu-id="4dfb7-144">This will pre-populate our store catalog with a list of Genres, Artists, and Albums.</span></span> <span data-ttu-id="4dfb7-145">Pobrania MvcMusicStore Assets.zip - uwzględnione naszych plików projektu lokacji używanych wcześniej w tym samouczku — zawiera plik klasy z tymi danymi inicjatora, znajduje się w folderze o nazwie kodu.</span><span class="sxs-lookup"><span data-stu-id="4dfb7-145">The MvcMusicStore-Assets.zip download - which included our site design files used earlier in this tutorial - has a class file with this seed data, located in a folder named Code.</span></span>

<span data-ttu-id="4dfb7-146">W kodzie / folderu modeli, zlokalizuj plik SampleData.cs i upuść go w folderze modeli w projekcie, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="4dfb7-146">Within the Code / Models folder, locate the SampleData.cs file and drop it into the Models folder in our project, as shown below.</span></span>

![](mvc-music-store-part-4/_static/image4.png)

<span data-ttu-id="4dfb7-147">Teraz należy dodać jeden wiersz kodu, aby poinformować Entity Framework o tej klasy SampleData.</span><span class="sxs-lookup"><span data-stu-id="4dfb7-147">Now we need to add one line of code to tell Entity Framework about that SampleData class.</span></span> <span data-ttu-id="4dfb7-148">Kliknij dwukrotnie plik Global.asax w katalogu głównym projektu, aby otworzyć go i Dodaj następujący wiersz u góry aplikacji\_Uruchom metodę.</span><span class="sxs-lookup"><span data-stu-id="4dfb7-148">Double-click on the Global.asax file in the root of the project to open it and add the following line to the top the Application\_Start method.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample6.cs)]

<span data-ttu-id="4dfb7-149">W tym momencie możemy ukończono pracę niezbędne do skonfigurowania programu Entity Framework naszego projektu wygląda.</span><span class="sxs-lookup"><span data-stu-id="4dfb7-149">At this point, we've completed the work necessary to configure Entity Framework for our project.</span></span>

## <a name="querying-the-database"></a><span data-ttu-id="4dfb7-150">wykonywanie zapytania w bazie danych</span><span class="sxs-lookup"><span data-stu-id="4dfb7-150">Querying the Database</span></span>

<span data-ttu-id="4dfb7-151">Teraz zaktualizujmy naszych StoreController tak, aby zamiast "fikcyjny dane" zamiast niego w naszej bazie danych do wykonywania zapytań, wszystkie jej informacje.</span><span class="sxs-lookup"><span data-stu-id="4dfb7-151">Now let's update our StoreController so that instead of using "dummy data" it instead calls into our database to query all of its information.</span></span> <span data-ttu-id="4dfb7-152">Rozpoczniemy od zadeklarowania pola na **StoreController** zawierającą wystąpienia klasy MusicStoreEntities o nazwie storeDB:</span><span class="sxs-lookup"><span data-stu-id="4dfb7-152">We'll start by declaring a field on the **StoreController** to hold an instance of the MusicStoreEntities class, named storeDB:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample7.cs)]

### <a name="updating-the-store-index-to-query-the-database"></a><span data-ttu-id="4dfb7-153">Aktualizowanie indeksu Store do wykonywania zapytań w bazie danych</span><span class="sxs-lookup"><span data-stu-id="4dfb7-153">Updating the Store Index to query the database</span></span>

<span data-ttu-id="4dfb7-154">Klasa MusicStoreEntities jest obsługiwany przez program Entity Framework i ujawnia właściwość kolekcji, dla każdej tabeli w naszej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="4dfb7-154">The MusicStoreEntities class is maintained by the Entity Framework and exposes a collection property for each table in our database.</span></span> <span data-ttu-id="4dfb7-155">Zaktualizujmy naszych StoreController akcji indeksu można pobrać wszystkich gatunki w naszej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="4dfb7-155">Let's update our StoreController's Index action to retrieve all Genres in our database.</span></span> <span data-ttu-id="4dfb7-156">Wcześniej zrobiliśmy to zakodowane na stałe danych ciągu.</span><span class="sxs-lookup"><span data-stu-id="4dfb7-156">Previously we did this by hard-coding string data.</span></span> <span data-ttu-id="4dfb7-157">Teraz możemy zamiast tego użyć kontekstu platformy Entity Framework Generes kolekcji:</span><span class="sxs-lookup"><span data-stu-id="4dfb7-157">Now we can instead just use the Entity Framework context Generes collection:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample8.cs)]

<span data-ttu-id="4dfb7-158">Nie zmiany muszą zostać przeprowadzona do naszych szablon widoku, ponieważ firma Microsoft nadal zwracany tego samego StoreIndexViewModel, firma Microsoft zwrócone, zanim — firma Microsoft jest po prostu zwracanie danych na żywo z naszej bazie danych teraz.</span><span class="sxs-lookup"><span data-stu-id="4dfb7-158">No changes need to happen to our View template since we're still returning the same StoreIndexViewModel we returned before - we're just returning live data from our database now.</span></span>

<span data-ttu-id="4dfb7-159">Jeśli firma Microsoft Uruchom projekt ponownie, odwiedź adres URL "/ Store" teraz zobaczymy listę wszystkich gatunki w naszej bazie danych:</span><span class="sxs-lookup"><span data-stu-id="4dfb7-159">When we run the project again and visit the "/Store" URL, we'll now see a list of all Genres in our database:</span></span>

![](mvc-music-store-part-4/_static/image1.jpg)

### <a name="updating-store-browse-and-details-to-use-live-data"></a><span data-ttu-id="4dfb7-160">Aktualizowanie Przeglądaj Store i szczegółowe informacje na korzystanie z danych na żywo</span><span class="sxs-lookup"><span data-stu-id="4dfb7-160">Updating Store Browse and Details to use live data</span></span>

<span data-ttu-id="4dfb7-161">Za pomocą/Store/przeglądania? gatunku =*[niektóre gatunek]* metody akcji, Trwa przeszukiwanie dla określonego rodzaju według nazwy.</span><span class="sxs-lookup"><span data-stu-id="4dfb7-161">With the /Store/Browse?genre=*[some-genre]* action method, we're searching for a Genre by name.</span></span> <span data-ttu-id="4dfb7-162">Tylko oczekujemy, że jeden wynik, ponieważ firma Microsoft nigdy nie powinny mieć dwóch wpisów dla tej samej nazwie gatunku, więc możemy użyć. Funkcja Single() rozszerzenia w składniku LINQ do kwerendy do odpowiedniego obiektu gatunku następująco (nie należy umieszczać to jeszcze):</span><span class="sxs-lookup"><span data-stu-id="4dfb7-162">We only expect one result, since we shouldn't ever have two entries for the same Genre name, and so we can use the .Single() extension in LINQ to query for the appropriate Genre object like this (don't type this yet):</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample9.cs)]

<span data-ttu-id="4dfb7-163">Pojedyncza metoda przyjmuje wyrażenia Lambda jako parametr, który określa, że chcemy, aby pojedynczy obiekt gatunku taki sposób, że jego nazwa pasuje do wartości, który zdefiniowaliśmy.</span><span class="sxs-lookup"><span data-stu-id="4dfb7-163">The Single method takes a Lambda expression as a parameter, which specifies that we want a single Genre object such that its name matches the value we've defined.</span></span> <span data-ttu-id="4dfb7-164">W przypadku powyższych ładujemy pojedynczy obiekt gatunku dopasowania Najdywania wartości nazw.</span><span class="sxs-lookup"><span data-stu-id="4dfb7-164">In the case above, we are loading a single Genre object with a Name value matching Disco.</span></span>

<span data-ttu-id="4dfb7-165">Firma Microsoft będzie korzystać z to funkcja programu Entity Framework, która pozwala nam w celu wskazania innych powiązanych jednostek, które chcemy, aby załadować także po pobraniu obiektu gatunku.</span><span class="sxs-lookup"><span data-stu-id="4dfb7-165">We'll take advantage of an Entity Framework feature that allows us to indicate other related entities we want loaded as well when the Genre object is retrieved.</span></span> <span data-ttu-id="4dfb7-166">Ta funkcja jest wywoływana, kształtowanie wynik zapytania i pozwala na zmniejszenie liczby potrzebujemy dostępu do bazy danych, aby pobrać wszystkie informacje potrzebne.</span><span class="sxs-lookup"><span data-stu-id="4dfb7-166">This feature is called Query Result Shaping, and enables us to reduce the number of times we need to access the database to retrieve all of the information we need.</span></span> <span data-ttu-id="4dfb7-167">Chcemy wstępnego pobierania albumów dla gatunku Pobieramy, dlatego zaktualizujemy naszego zapytania do dołączenia z Genres.Include("Albums"), aby wskazać, że chcemy także powiązane ze zdjęciami.</span><span class="sxs-lookup"><span data-stu-id="4dfb7-167">We want to pre-fetch the Albums for Genre we retrieve, so we'll update our query to include from Genres.Include("Albums") to indicate that we want related albums as well.</span></span> <span data-ttu-id="4dfb7-168">Może to być bardziej efektywne, ponieważ pobierze dane naszych gatunku i albumów w żądaniu pojedynczej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="4dfb7-168">This is more efficient, since it will retrieve both our Genre and Album data in a single database request.</span></span>

<span data-ttu-id="4dfb7-169">Wraz z wyjaśnieniem na bok poniżej przedstawiono, jak wygląda naszej zaktualizowane akcji kontrolera przeglądania:</span><span class="sxs-lookup"><span data-stu-id="4dfb7-169">With the explanations out of the way, here's how our updated Browse controller action looks:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample10.cs)]

<span data-ttu-id="4dfb7-170">Firma Microsoft jest teraz zaktualizować widoku Przeglądaj Store, aby wyświetlić ze zdjęciami, które są dostępne w każdego gatunku.</span><span class="sxs-lookup"><span data-stu-id="4dfb7-170">We can now update the Store Browse View to display the albums which are available in each Genre.</span></span> <span data-ttu-id="4dfb7-171">Otwórz szablon widoku (znaleziony w /Views/Store/Browse.cshtml), a następnie Dodaj listę punktowaną ze zdjęciami, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="4dfb7-171">Open the view template (found in /Views/Store/Browse.cshtml) and add a bulleted list of Albums as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample11.cshtml)]

<span data-ttu-id="4dfb7-172">Uruchamianie aplikacji i przechodząc do/Store/przeglądania? gatunku = pokazują Jazz naszych wyników są teraz pochodzi z bazy danych, wyświetlanie albumów wszystkich naszych wybranego rodzaju.</span><span class="sxs-lookup"><span data-stu-id="4dfb7-172">Running our application and browsing to /Store/Browse?genre=Jazz shows that our results are now being pulled from the database, displaying all albums in our selected Genre.</span></span>

![](mvc-music-store-part-4/_static/image2.jpg)

<span data-ttu-id="4dfb7-173">Wybierzemy taką samą Zmień/Store/szczegóły / [id] adresu URL i Zamień zastępczy danych zapytanie bazy danych, która ładuje albumu o identyfikatorze pasuje do wartości parametru.</span><span class="sxs-lookup"><span data-stu-id="4dfb7-173">We'll make the same change to our /Store/Details/[id] URL, and replace our dummy data with a database query which loads an Album whose ID matches the parameter value.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample12.cs)]

<span data-ttu-id="4dfb7-174">Uruchamianie aplikacji i przechodząc do /Store/Details/1 pokazuje, że naszych wyników są teraz pobierane z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="4dfb7-174">Running our application and browsing to /Store/Details/1 shows that our results are now being pulled from the database.</span></span>

![](mvc-music-store-part-4/_static/image5.png)

<span data-ttu-id="4dfb7-175">Teraz, że nasza strona szczegółów Store jest skonfigurowany do wyświetlania albumu według Identyfikatora fotograficzne, zaktualizujmy **Przeglądaj** widok, aby utworzyć łącze do widoku szczegółów.</span><span class="sxs-lookup"><span data-stu-id="4dfb7-175">Now that our Store Details page is set up to display an album by the Album ID, let's update the **Browse** view to link to the Details view.</span></span> <span data-ttu-id="4dfb7-176">Firma Microsoft użyje Html.ActionLink, dokładnie tak, jak zrobiliśmy się połączyć z indeksu Store Przeglądaj Store na końcu w poprzedniej sekcji.</span><span class="sxs-lookup"><span data-stu-id="4dfb7-176">We will use Html.ActionLink, exactly as we did to link from Store Index to Store Browse at the end of the previous section.</span></span> <span data-ttu-id="4dfb7-177">Pełne źródło widoku przeglądania pojawia się poniżej.</span><span class="sxs-lookup"><span data-stu-id="4dfb7-177">The complete source for the Browse view appears below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample13.cshtml)]

<span data-ttu-id="4dfb7-178">Teraz możemy przejść z poziomu strony Store do strony gatunku, którym jest wyświetlana lista dostępnych ze zdjęciami, i klikając albumu możemy wyświetlić szczegóły dotyczące danego albumu.</span><span class="sxs-lookup"><span data-stu-id="4dfb7-178">We're now able to browse from our Store page to a Genre page, which lists the available albums, and by clicking on an album we can view details for that album.</span></span>

![](mvc-music-store-part-4/_static/image6.png)

> [!div class="step-by-step"]
> <span data-ttu-id="4dfb7-179">[Poprzednie](mvc-music-store-part-3.md)
> [dalej](mvc-music-store-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="4dfb7-179">[Previous](mvc-music-store-part-3.md)
[Next](mvc-music-store-part-5.md)</span></span>

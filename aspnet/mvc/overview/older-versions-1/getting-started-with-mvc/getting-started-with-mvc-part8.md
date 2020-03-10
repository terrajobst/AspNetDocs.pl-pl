---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
title: Dodawanie kolumny do modelu | Microsoft Docs
author: shanselman
description: Jest to samouczek początkującego, który wprowadza podstawy ASP.NET MVC. Utwórz prostą aplikację sieci Web, która odczytuje i zapisuje dane w bazie danych.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: 7ae696b9-348f-4993-8ebb-a838acbe0c28
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
msc.type: authoredcontent
ms.openlocfilehash: 1cf092c3db3959d6f47006f1be2ba82833c5dc06
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78543583"
---
# <a name="adding-a-column-to-the-model"></a><span data-ttu-id="4f403-104">Dodawanie kolumny do modelu</span><span class="sxs-lookup"><span data-stu-id="4f403-104">Adding a Column to the Model</span></span>

<span data-ttu-id="4f403-105">przez [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="4f403-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="4f403-106">Jest to samouczek początkującego, który wprowadza podstawy ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="4f403-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="4f403-107">Utworzysz prostą aplikację sieci Web, która odczytuje i zapisuje dane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="4f403-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="4f403-108">Odwiedź [centrum learning ASP.NET MVC](../../../index.md) , aby znaleźć inne samouczki i przykłady MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="4f403-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>

<span data-ttu-id="4f403-109">W tej sekcji omówiono, jak możemy wprowadzić zmiany w schemacie bazy danych i obsłużyć zmiany w naszej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4f403-109">In this section we are going to walk through how we can make changes to the schema of our database, and handle the changes within our application.</span></span>

<span data-ttu-id="4f403-110">Dodajmy kolumnę "Rating" do tabeli filmów.</span><span class="sxs-lookup"><span data-stu-id="4f403-110">Let's add a "Rating" Column to the Movie table.</span></span> <span data-ttu-id="4f403-111">Wróć do środowiska IDE i kliknij Eksplorator bazy danych.</span><span class="sxs-lookup"><span data-stu-id="4f403-111">Go back to the IDE and click the Database Explorer.</span></span> <span data-ttu-id="4f403-112">Kliknij prawym przyciskiem myszy tabelę filmów i wybierz polecenie Otwórz definicję tabeli.</span><span class="sxs-lookup"><span data-stu-id="4f403-112">Right click the Movie table and select Open Table Definition.</span></span>

<span data-ttu-id="4f403-113">Dodaj kolumnę "Rating", jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="4f403-113">Add a "Rating" column as seen below.</span></span> <span data-ttu-id="4f403-114">Ponieważ nie mamy teraz żadnych ocen, kolumna może zezwalać na wartości null.</span><span class="sxs-lookup"><span data-stu-id="4f403-114">Since we don't have any Ratings now, the column can allow nulls.</span></span> <span data-ttu-id="4f403-115">Kliknij polecenie Zapisz.</span><span class="sxs-lookup"><span data-stu-id="4f403-115">Click Save.</span></span>

<span data-ttu-id="4f403-116">[![edytowania filmów](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="4f403-116">[![Editing Movies Table](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)</span></span>

<span data-ttu-id="4f403-117">Następnie wróć do Eksplorator rozwiązań i Otwórz plik filmy. edmx (znajdujący się w folderze \Models).</span><span class="sxs-lookup"><span data-stu-id="4f403-117">Next, return to the Solution Explorer and open up the Movies.edmx file (which is in the \Models folder).</span></span> <span data-ttu-id="4f403-118">Kliknij prawym przyciskiem myszy powierzchnię projektu (biały obszar) i wybierz pozycję Aktualizuj model z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="4f403-118">Right click on the design surface (the white area) and select Update Model from Database.</span></span>

<span data-ttu-id="4f403-119">[![filmów — Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="4f403-119">[![Movies - Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)</span></span>

<span data-ttu-id="4f403-120">Spowoduje to uruchomienie "kreatora aktualizacji".</span><span class="sxs-lookup"><span data-stu-id="4f403-120">This will launch the "Update Wizard".</span></span> <span data-ttu-id="4f403-121">Kliknij znajdującą się na niej kartę Odśwież i kliknij przycisk Zakończ.</span><span class="sxs-lookup"><span data-stu-id="4f403-121">Click the Refresh tab within it and click Finish.</span></span> <span data-ttu-id="4f403-122">Nasze klasy modelu filmu zostaną następnie zaktualizowane przy użyciu nowej kolumny.</span><span class="sxs-lookup"><span data-stu-id="4f403-122">Our Movie model class will then be updated with the new column.</span></span>

![Kreator aktualizacji (2)](getting-started-with-mvc-part8/_static/image5.png)

<span data-ttu-id="4f403-124">Po kliknięciu przycisku Zakończ zobaczysz, że nowa kolumna ocen została dodana do jednostki filmu w naszym modelu.</span><span class="sxs-lookup"><span data-stu-id="4f403-124">After clicking Finish, you can see the new Rating Column has been added to the Movie Entity in our model.</span></span>

<span data-ttu-id="4f403-125">[![jednostka filmu](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="4f403-125">[![Movie Entity](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)</span></span>

<span data-ttu-id="4f403-126">Dodaliśmy kolumnę w modelu bazy danych, ale widoki nie są dla niego znane.</span><span class="sxs-lookup"><span data-stu-id="4f403-126">We've added a column in the database model, but the Views don't know about it.</span></span>

## <a name="update-views-with-model-changes"></a><span data-ttu-id="4f403-127">Aktualizowanie widoków ze zmianami modelu</span><span class="sxs-lookup"><span data-stu-id="4f403-127">Update Views with Model Changes</span></span>

<span data-ttu-id="4f403-128">Istnieje kilka sposobów aktualizowania naszych szablonów widoku w celu odzwierciedlenia nowej kolumny klasyfikacji.</span><span class="sxs-lookup"><span data-stu-id="4f403-128">There are a few ways we could update our view templates to reflect the new Rating column.</span></span> <span data-ttu-id="4f403-129">Ponieważ Utworzyliśmy te widoki przez wygenerowanie ich za pomocą okna dialogowego Dodaj widok, możemy je usunąć i ponownie utworzyć.</span><span class="sxs-lookup"><span data-stu-id="4f403-129">Since we created these Views by generating them via the Add View dialog, we could delete them and recreate them again.</span></span> <span data-ttu-id="4f403-130">Jednak zazwyczaj ludzie już wprowadziły modyfikacje w swoich szablonach widoków od początkowej generacji szkieletowej i chcą ręcznie dodawać lub usuwać pola, podobnie jak w przypadku pola ID dla tworzenia.</span><span class="sxs-lookup"><span data-stu-id="4f403-130">However, typically people will have already made modifications to their View templates from the initial scaffolded generation and will want to add or delete fields manually, just as we did with the ID field for Create.</span></span>

<span data-ttu-id="4f403-131">Otwórz szablon \Views\Movies\Index.aspx i Dodaj &lt;ową ocenę&gt;&lt;/th&gt; do nagłówka tabeli filmów.</span><span class="sxs-lookup"><span data-stu-id="4f403-131">Open up the \Views\Movies\Index.aspx template and add a &lt;th&gt;Rating&lt;/th&gt; to the head of the Movie table.</span></span> <span data-ttu-id="4f403-132">Dodano mnie po gatunku.</span><span class="sxs-lookup"><span data-stu-id="4f403-132">I added mine after Genre.</span></span> <span data-ttu-id="4f403-133">Następnie w tej samej pozycji kolumny, ale mniej więcej, Dodaj wiersz do danych wyjściowych naszej nowej klasyfikacji.</span><span class="sxs-lookup"><span data-stu-id="4f403-133">Then, in the same column position but lower down, add a line to output our new Rating.</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample1.aspx)]

<span data-ttu-id="4f403-134">Nasz końcowy szablon index. aspx będzie wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="4f403-134">Our final Index.aspx template will look like this:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample2.aspx)]

<span data-ttu-id="4f403-135">Następnie Otwórzmy szablon \Views\Movies\Create.aspx i dodasz etykietę i pole tekstowe nowej właściwości rankingu:</span><span class="sxs-lookup"><span data-stu-id="4f403-135">Let's then open up the \Views\Movies\Create.aspx template and add a Label and Textbox for our new Rating property:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample3.aspx)]

<span data-ttu-id="4f403-136">Nasz końcowy szablon Create. aspx będzie wyglądać następująco i zmienimy tytuł przeglądarki i pomocniczą &lt;H2&gt; tytułu w taki sam sposób jak "Utwórz film" w tym miejscu.</span><span class="sxs-lookup"><span data-stu-id="4f403-136">Our final Create.aspx template will look like this, and let's change our browser's title and secondary &lt;h2&gt; title to something like "Create a Movie" while we're in here!</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample4.aspx)]

<span data-ttu-id="4f403-137">Uruchom aplikację, a teraz masz nowe pole w bazie danych, które zostało dodane do strony Tworzenie.</span><span class="sxs-lookup"><span data-stu-id="4f403-137">Run your app and now you've got a new field in the database that's been added to the Create page.</span></span> <span data-ttu-id="4f403-138">Dodaj nowy film — tym razem z klasyfikacją, a następnie kliknij przycisk Utwórz.</span><span class="sxs-lookup"><span data-stu-id="4f403-138">Add a new Movie - this time with a Rating - and click Create.</span></span>

<span data-ttu-id="4f403-139">[![utworzyć filmu — Windows Internet Explorer](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="4f403-139">[![Create a Movie - Windows Internet Explorer](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)</span></span>

<span data-ttu-id="4f403-140">Po kliknięciu przycisku Utwórz zostanie wysłana do strony indeks, na której jest wyświetlany nowy film z nową kolumną oceny w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="4f403-140">After you click Create, you're sent to the Index page where you new Movie is listed with the new Rating Column in the database</span></span>

<span data-ttu-id="4f403-141">[Lista filmów ![— Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="4f403-141">[![Movie List - Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)</span></span>

<span data-ttu-id="4f403-142">W tym podstawowym samouczku przedstawiono tworzenie kontrolerów, kojarzenie ich z widokami i przekazywaniem danych z zakodowanymi danymi.</span><span class="sxs-lookup"><span data-stu-id="4f403-142">This basic tutorial got you started making Controllers, associating them with Views and passing around hard-coded data.</span></span> <span data-ttu-id="4f403-143">Następnie utworzyliśmy i zaprojektowano bazę danych oraz umieszczamy w niej dane.</span><span class="sxs-lookup"><span data-stu-id="4f403-143">Then we created and designed a Database and put some data it in.</span></span> <span data-ttu-id="4f403-144">Pobrano dane z bazy danych i są one wyświetlane w tabeli HTML.</span><span class="sxs-lookup"><span data-stu-id="4f403-144">We retrieved the data from the database and displayed it in an HTML table.</span></span> <span data-ttu-id="4f403-145">Następnie dodaliśmy formularz Create, który umożliwia użytkownikowi dodawanie danych do bazy danych z poziomu aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="4f403-145">Then we added a Create form that let the user add data to the database themselves from within the Web Application.</span></span> <span data-ttu-id="4f403-146">Dodaliśmy sprawdzanie poprawności, a następnie Walidacja używa języka JavaScript po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="4f403-146">We added validation, then made the validation use JavaScript on the client-side.</span></span> <span data-ttu-id="4f403-147">Na koniec baza danych została zmieniona w taki sposób, aby zawierała nową kolumnę danych, a następnie Zaktualizowano te dwie strony, aby utworzyć i wyświetlić te nowe dane.</span><span class="sxs-lookup"><span data-stu-id="4f403-147">Finally, we changed the database to include a new column of data, then updated our two pages to create and display this new data.</span></span>

<span data-ttu-id="4f403-148">Teraz zachęcamy do przechodzenia do naszego samouczka pośredniego "[sklepu z muzyką MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)" oraz wielu filmów wideo i zasobów w [https://asp.net/mvc](https://asp.net/mvc) , aby dowiedzieć się więcej o ASP.NET MVC!</span><span class="sxs-lookup"><span data-stu-id="4f403-148">I now encourage you to move on to our intermediate-level tutorial "[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)" as well as the many videos and resources at [https://asp.net/mvc](https://asp.net/mvc) to learn even more about ASP.NET MVC!</span></span>

<span data-ttu-id="4f403-149">Owocnej pracy.</span><span class="sxs-lookup"><span data-stu-id="4f403-149">Enjoy!</span></span>

- <span data-ttu-id="4f403-150">Scott Hanselman — [http://hanselman.com](http://hanselman.com) i [@shanselman](http://twitter.com/shanselman) w serwisie Twitter.</span><span class="sxs-lookup"><span data-stu-id="4f403-150">Scott Hanselman - [http://hanselman.com](http://hanselman.com) and [@shanselman](http://twitter.com/shanselman) on Twitter.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="4f403-151">Wstecz</span><span class="sxs-lookup"><span data-stu-id="4f403-151">Previous</span></span>](getting-started-with-mvc-part7.md)

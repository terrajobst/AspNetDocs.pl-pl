---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
title: Tworzenie bazy danych | Microsoft Docs
author: shanselman
description: Jest to samouczek początkującego, który wprowadza podstawy ASP.NET MVC. Utwórz prostą aplikację sieci Web, która odczytuje i zapisuje dane w bazie danych.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: 742df67f-484d-4ef3-af6b-8c791e556b43
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
msc.type: authoredcontent
ms.openlocfilehash: 995db714ce6365415d06dc458aee84a31c7c8fd6
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78581439"
---
# <a name="creating-a-database"></a><span data-ttu-id="a17fa-104">Tworzenie bazy danych</span><span class="sxs-lookup"><span data-stu-id="a17fa-104">Creating a Database</span></span>

<span data-ttu-id="a17fa-105">przez [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="a17fa-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="a17fa-106">Jest to samouczek początkującego, który wprowadza podstawy ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="a17fa-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="a17fa-107">Utworzysz prostą aplikację sieci Web, która odczytuje i zapisuje dane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="a17fa-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="a17fa-108">Odwiedź [centrum learning ASP.NET MVC](../../../index.md) , aby znaleźć inne samouczki i przykłady MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="a17fa-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>

<span data-ttu-id="a17fa-109">W tej sekcji zamierzamy utworzyć nową bazę danych SQL Express, która będzie używana do przechowywania i pobierania danych filmów.</span><span class="sxs-lookup"><span data-stu-id="a17fa-109">In this section we are going to create a new SQL Express database that we'll use to store and retrieve our movie data.</span></span> <span data-ttu-id="a17fa-110">W środowisku IDE programu Visual Web Developer wybierz pozycję Widok | Eksplorator serwera.</span><span class="sxs-lookup"><span data-stu-id="a17fa-110">From within the Visual Web Developer IDE, select View | Server Explorer.</span></span> <span data-ttu-id="a17fa-111">Kliknij prawym przyciskiem myszy pozycję połączenia danych, a następnie kliknij pozycję Dodaj połączenie.</span><span class="sxs-lookup"><span data-stu-id="a17fa-111">Right click on Data Connections and click Add Connection...</span></span>

![Addconnection](getting-started-with-mvc-part4/_static/image1.png)

<span data-ttu-id="a17fa-113">W oknie dialogowym Wybierz źródło danych wybierz pozycję Microsoft SQL Server i wybierz pozycję Kontynuuj.</span><span class="sxs-lookup"><span data-stu-id="a17fa-113">In the Choose Data Source dialog, select Microsoft SQL Server and select Continue.</span></span>

![](getting-started-with-mvc-part4/_static/image2.png)

<span data-ttu-id="a17fa-114">W oknie dialogowym Dodawanie połączenia wprowadź wartość ".\SQLEXPRESS" dla nazwy serwera, a następnie wprowadź "filmy" jako nazwę nowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a17fa-114">In the Add Connection dialog, enter ".\SQLEXPRESS" for your Server Name, and enter "Movies" as the name for your new database.</span></span>

<span data-ttu-id="a17fa-115">[okno dialogowe Dodawanie połączenia ![](getting-started-with-mvc-part4/_static/image4.png)](getting-started-with-mvc-part4/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="a17fa-115">[![Add Connection dialog](getting-started-with-mvc-part4/_static/image4.png)](getting-started-with-mvc-part4/_static/image3.png)</span></span>

<span data-ttu-id="a17fa-116">Kliknij przycisk OK. pojawi się monit z prośbą o utworzenie tej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a17fa-116">Click OK and you'll be asked if you want to create that database.</span></span> <span data-ttu-id="a17fa-117">Wybierz pozycję tak.</span><span class="sxs-lookup"><span data-stu-id="a17fa-117">Select yes.</span></span>

<span data-ttu-id="a17fa-118">[![tworzyć filmy?](getting-started-with-mvc-part4/_static/image6.png)](getting-started-with-mvc-part4/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="a17fa-118">[![Create Movies?](getting-started-with-mvc-part4/_static/image6.png)](getting-started-with-mvc-part4/_static/image5.png)</span></span>

<span data-ttu-id="a17fa-119">Teraz masz pustą bazę danych w Eksplorator serwera.</span><span class="sxs-lookup"><span data-stu-id="a17fa-119">Now you've got an empty database in Server Explorer.</span></span>

![Dodaj nową tabelę](getting-started-with-mvc-part4/_static/image7.png)

<span data-ttu-id="a17fa-121">Kliknij prawym przyciskiem myszy tabele i kliknij polecenie Dodaj tabelę.</span><span class="sxs-lookup"><span data-stu-id="a17fa-121">Right click on Tables and click Add Table.</span></span> <span data-ttu-id="a17fa-122">Zostanie wyświetlony Projektant tabel.</span><span class="sxs-lookup"><span data-stu-id="a17fa-122">The Table Designer will appear.</span></span> <span data-ttu-id="a17fa-123">Dodaj kolumny dla identyfikatora, tytułu, ReleaseDate, gatunku i ceny.</span><span class="sxs-lookup"><span data-stu-id="a17fa-123">Add columns for Id, Title, ReleaseDate, Genre, and Price.</span></span> <span data-ttu-id="a17fa-124">Kliknij prawym przyciskiem myszy kolumnę ID, a następnie kliknij pozycję Ustaw klucz podstawowy.</span><span class="sxs-lookup"><span data-stu-id="a17fa-124">Right click on the ID column and click set Primary Key.</span></span> <span data-ttu-id="a17fa-125">Oto, jak wyglądają moje obszary projektowania.</span><span class="sxs-lookup"><span data-stu-id="a17fa-125">Here's what my design areas looks like.</span></span>

<span data-ttu-id="a17fa-126">[Edytor tabeli bazy danych ![](getting-started-with-mvc-part4/_static/image9.png)](getting-started-with-mvc-part4/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="a17fa-126">[![Database Table Editor](getting-started-with-mvc-part4/_static/image9.png)](getting-started-with-mvc-part4/_static/image8.png)</span></span>

<span data-ttu-id="a17fa-127">Ponadto wybierz kolumnę ID, a następnie w obszarze właściwości kolumny poniżej Zmień wartość "Specyfikacja tożsamości" na "tak".</span><span class="sxs-lookup"><span data-stu-id="a17fa-127">Also, select the Id column and under Column Properties below change "Identity Specification" to "Yes."</span></span>

<span data-ttu-id="a17fa-128">[![właściwości IsIdentity-Column](getting-started-with-mvc-part4/_static/image11.png)](getting-started-with-mvc-part4/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="a17fa-128">[![IsIdentity - Column Properties](getting-started-with-mvc-part4/_static/image11.png)](getting-started-with-mvc-part4/_static/image10.png)</span></span>

<span data-ttu-id="a17fa-129">Gdy wszystko będzie gotowe, kliknij ikonę Zapisz na pasku narzędzi lub wybierz pozycję plik | Zapisz z menu i nazwij tabelę "**Movie**" (pojedynczo).</span><span class="sxs-lookup"><span data-stu-id="a17fa-129">When you've got it done, click the Save icon in the toolbar or select File | Save from the menu, and name your table "**Movie**" (singular).</span></span> <span data-ttu-id="a17fa-130">Mamy bazę danych i tabelę.</span><span class="sxs-lookup"><span data-stu-id="a17fa-130">We've got a database and a table!</span></span>

<span data-ttu-id="a17fa-131">[![wybierz nazwę](getting-started-with-mvc-part4/_static/image13.png)](getting-started-with-mvc-part4/_static/image12.png)</span><span class="sxs-lookup"><span data-stu-id="a17fa-131">[![Choose Name](getting-started-with-mvc-part4/_static/image13.png)](getting-started-with-mvc-part4/_static/image12.png)</span></span>

<span data-ttu-id="a17fa-132">Wróć do Eksplorator serwera i kliknij prawym przyciskiem myszy tabelę filmu, a następnie wybierz polecenie "Pokaż dane tabeli".</span><span class="sxs-lookup"><span data-stu-id="a17fa-132">Go back to Server Explorer and right click the Movie table, then select "Show Table Data."</span></span> <span data-ttu-id="a17fa-133">Wprowadź kilka filmów, aby nasze bazy danych miały pewne dane.</span><span class="sxs-lookup"><span data-stu-id="a17fa-133">Enter a few movies so our database has some data.</span></span>

<span data-ttu-id="a17fa-134">[Edytowanie tabeli bazy danych ![](getting-started-with-mvc-part4/_static/image15.png)](getting-started-with-mvc-part4/_static/image14.png)</span><span class="sxs-lookup"><span data-stu-id="a17fa-134">[![Database Table Editing](getting-started-with-mvc-part4/_static/image15.png)](getting-started-with-mvc-part4/_static/image14.png)</span></span>

## <a name="creating-a-model"></a><span data-ttu-id="a17fa-135">Tworzenie modelu</span><span class="sxs-lookup"><span data-stu-id="a17fa-135">Creating a Model</span></span>

<span data-ttu-id="a17fa-136">Teraz przełącz się z powrotem do Eksplorator rozwiązań po prawej stronie IDE, a następnie kliknij prawym przyciskiem myszy folder modele i wybierz polecenie Dodaj | Nowy element.</span><span class="sxs-lookup"><span data-stu-id="a17fa-136">Now, switch back to the Solution Explorer on the right hand side of the IDE and right-click on the Models folder and select Add | New Item.</span></span>

<span data-ttu-id="a17fa-137">[![addnewmodelitem](getting-started-with-mvc-part4/_static/image17.png)](getting-started-with-mvc-part4/_static/image16.png)</span><span class="sxs-lookup"><span data-stu-id="a17fa-137">[![addnewmodelitem](getting-started-with-mvc-part4/_static/image17.png)](getting-started-with-mvc-part4/_static/image16.png)</span></span>

<span data-ttu-id="a17fa-138">Utworzymy model jednostki na podstawie naszej nowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a17fa-138">We're going to create an Entity Model from our new database.</span></span> <span data-ttu-id="a17fa-139">Spowoduje to dodanie zestawu klas do naszego projektu, który ułatwia wykonywanie zapytań i manipulowanie danymi w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="a17fa-139">This will add a set of classes to our project that makes it easy for us to query and manipulate the data within our database.</span></span> <span data-ttu-id="a17fa-140">Wybierz węzeł dane po lewej stronie okna dialogowego, a następnie wybierz szablon elementu ADO.NET Entity Data Model.</span><span class="sxs-lookup"><span data-stu-id="a17fa-140">Select the Data node on the left-hand side of the dialog, and then select the ADO.NET Entity Data Model item template.</span></span> <span data-ttu-id="a17fa-141">Nadaj mu nazwę filmy. edmx.</span><span class="sxs-lookup"><span data-stu-id="a17fa-141">Name it Movies.edmx.</span></span>

<span data-ttu-id="a17fa-142">[![AddNewDataModel](getting-started-with-mvc-part4/_static/image19.png)](getting-started-with-mvc-part4/_static/image18.png)</span><span class="sxs-lookup"><span data-stu-id="a17fa-142">[![AddNewDataModel](getting-started-with-mvc-part4/_static/image19.png)](getting-started-with-mvc-part4/_static/image18.png)</span></span>

<span data-ttu-id="a17fa-143">Kliknij przycisk "Dodaj".</span><span class="sxs-lookup"><span data-stu-id="a17fa-143">Click the "Add" button.</span></span> <span data-ttu-id="a17fa-144">Następnie zostanie uruchomiony Kreator Entity Data Model.</span><span class="sxs-lookup"><span data-stu-id="a17fa-144">This will then launch the "Entity Data Model Wizard".</span></span>

<span data-ttu-id="a17fa-145">W nowym oknie dialogowym, które się pojawi, wybierz pozycję Generuj z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a17fa-145">In the new dialog that pops up, select Generate from Database.</span></span> <span data-ttu-id="a17fa-146">Ponieważ właśnie została utworzona baza danych, będziemy musieli powiedzieć Entity Framework o naszej nowej bazie danych i jej tabeli.</span><span class="sxs-lookup"><span data-stu-id="a17fa-146">Since we've just made a database, we'll only need to tell the Entity Framework about our new database and its table.</span></span> <span data-ttu-id="a17fa-147">Kliknij przycisk Dalej, aby zapisać połączenie z bazą danych w konfiguracji aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="a17fa-147">Click Next to save our database connection in our web application's configuration.</span></span> <span data-ttu-id="a17fa-148">Zaznacz pole wyboru tabele i film, a następnie kliknij przycisk Zakończ.</span><span class="sxs-lookup"><span data-stu-id="a17fa-148">Now, check the Tables and Movie checkbox and click Finish.</span></span>

<span data-ttu-id="a17fa-149">[Kreator Entity Data Model ![](getting-started-with-mvc-part4/_static/image21.png)](getting-started-with-mvc-part4/_static/image20.png)</span><span class="sxs-lookup"><span data-stu-id="a17fa-149">[![Entity Data Model Wizard](getting-started-with-mvc-part4/_static/image21.png)](getting-started-with-mvc-part4/_static/image20.png)</span></span>

<span data-ttu-id="a17fa-150">Teraz możemy zobaczyć nową tabelę filmów w Entity Framework Designer i uzyskać do niej dostęp z kodu.</span><span class="sxs-lookup"><span data-stu-id="a17fa-150">Now we can see our new Movie table in the Entity Framework Designer and access it from code.</span></span>

<span data-ttu-id="a17fa-151">[![filmów — Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part4/_static/image23.png)](getting-started-with-mvc-part4/_static/image22.png)</span><span class="sxs-lookup"><span data-stu-id="a17fa-151">[![Movies - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part4/_static/image23.png)](getting-started-with-mvc-part4/_static/image22.png)</span></span>

<span data-ttu-id="a17fa-152">Na powierzchni projektowej można zobaczyć "film".</span><span class="sxs-lookup"><span data-stu-id="a17fa-152">On the design surface you can see a "Movie" class.</span></span> <span data-ttu-id="a17fa-153">Ta klasa jest mapowana na tabelę "film" w naszej bazie danych, a każda właściwość w niej jest mapowana na kolumnę z tabelą.</span><span class="sxs-lookup"><span data-stu-id="a17fa-153">This class maps to the "Movie" table in our database, and each property within it maps to a column with the table.</span></span> <span data-ttu-id="a17fa-154">Każde wystąpienie klasy "Movie" będzie odpowiadać wierszowi w tabeli "Movie".</span><span class="sxs-lookup"><span data-stu-id="a17fa-154">Each instance of a "Movie" class will correspond to a row within the "Movie" table.</span></span>

<span data-ttu-id="a17fa-155">Jeśli nie chcesz używać domyślnych konwencji nazewnictwa i mapowania używanych przez Entity Framework, możesz użyć projektanta Entity Framework, aby zmienić lub dostosować je.</span><span class="sxs-lookup"><span data-stu-id="a17fa-155">If you don't like the default naming and mapping conventions used by the Entity Framework, you can use the Entity Framework designer to change or customize them.</span></span> <span data-ttu-id="a17fa-156">W przypadku tej aplikacji będziemy używać ustawień domyślnych i po prostu zapiszą plik w stanie takim, w jakim się znajdują.</span><span class="sxs-lookup"><span data-stu-id="a17fa-156">For this application we'll use the defaults and just save the file as-is.</span></span>

<span data-ttu-id="a17fa-157">Teraz pracujemy nad niektórymi rzeczywistymi danymi!</span><span class="sxs-lookup"><span data-stu-id="a17fa-157">Now, let's work with some real data!</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a17fa-158">[Poprzednie](getting-started-with-mvc-part3.md)
> [dalej](getting-started-with-mvc-part5.md)</span><span class="sxs-lookup"><span data-stu-id="a17fa-158">[Previous](getting-started-with-mvc-part3.md)
[Next](getting-started-with-mvc-part5.md)</span></span>

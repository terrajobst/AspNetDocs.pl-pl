---
uid: mvc/overview/older-versions-1/nerddinner/create-a-database
title: Tworzenie bazy danych | Microsoft Docs
author: microsoft
description: Krok 2 przedstawia kroki tworzenia bazy danych przechowującej wszystkie dane dotyczące obiadu i RSVP dla naszej aplikacji NerdDinner.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 983f3ffa-08b8-4868-b8c9-aa34593fc683
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-database
msc.type: authoredcontent
ms.openlocfilehash: b0aa7c8cdf741f44e09ed18e2b2f73fe6bf786ae
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78581005"
---
# <a name="create-a-database"></a><span data-ttu-id="87731-103">Tworzenie bazy danych</span><span class="sxs-lookup"><span data-stu-id="87731-103">Create a Database</span></span>

<span data-ttu-id="87731-104">przez [firmę Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="87731-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="87731-105">Pobierz plik PDF</span><span class="sxs-lookup"><span data-stu-id="87731-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="87731-106">Jest to krok 2 bezpłatnego [samouczka dotyczącego aplikacji "NerdDinner"](introducing-the-nerddinner-tutorial.md) , który zawiera instrukcje tworzenia niewielkiej, ale kompletnej aplikacji sieci Web przy użyciu ASP.NET MVC 1.</span><span class="sxs-lookup"><span data-stu-id="87731-106">This is step 2 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="87731-107">Krok 2 przedstawia kroki tworzenia bazy danych przechowującej wszystkie dane dotyczące obiadu i RSVP dla naszej aplikacji NerdDinner.</span><span class="sxs-lookup"><span data-stu-id="87731-107">Step 2 shows the steps to create the database holding all of the dinner and RSVP data for our NerdDinner application.</span></span>
> 
> <span data-ttu-id="87731-108">Jeśli używasz ASP.NET MVC 3, zalecamy użycie [wprowadzenie ze samouczkami ze sklepu MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) lub [MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) .</span><span class="sxs-lookup"><span data-stu-id="87731-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>

## <a name="nerddinner-step-2-creating-the-database"></a><span data-ttu-id="87731-109">NerdDinner krok 2: Tworzenie bazy danych</span><span class="sxs-lookup"><span data-stu-id="87731-109">NerdDinner Step 2: Creating the Database</span></span>

<span data-ttu-id="87731-110">Będziemy używać bazy danych do przechowywania wszystkich danych obiadu i RSVP dla naszej aplikacji NerdDinner.</span><span class="sxs-lookup"><span data-stu-id="87731-110">We'll be using a database to store all of the Dinner and RSVP data for our NerdDinner application.</span></span>

<span data-ttu-id="87731-111">Poniższe kroki pokazują, jak utworzyć bazę danych przy użyciu wersji bezpłatna SQL Server Express (którą można łatwo zainstalować przy użyciu wersji 2 [Instalator platformy Microsoft Web](https://www.microsoft.com/web/downloads/platform.aspx)).</span><span class="sxs-lookup"><span data-stu-id="87731-111">The steps below show creating the database using the free SQL Server Express edition (which you can easily install using V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)).</span></span> <span data-ttu-id="87731-112">Cały kod, który piszemy, współdziała z zarówno SQL Server Express, jak i pełnymi SQL Server.</span><span class="sxs-lookup"><span data-stu-id="87731-112">All of the code we'll write works with both SQL Server Express and the full SQL Server.</span></span>

### <a name="creating-a-new-sql-server-express-database"></a><span data-ttu-id="87731-113">Tworzenie nowej bazy danych SQL Server Express</span><span class="sxs-lookup"><span data-stu-id="87731-113">Creating a new SQL Server Express database</span></span>

<span data-ttu-id="87731-114">Zaczniemy od kliknięcia prawym przyciskiem myszy projektu sieci Web, a następnie wybierz polecenie menu **dodaj&gt;nowy element** :</span><span class="sxs-lookup"><span data-stu-id="87731-114">We'll begin by right-clicking on our web project, and then select the **Add-&gt;New Item** menu command:</span></span>

![](create-a-database/_static/image1.png)

<span data-ttu-id="87731-115">Spowoduje to wyświetlenie okna dialogowego "Dodawanie nowego elementu" programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="87731-115">This will bring up Visual Studio's "Add New Item" dialog.</span></span> <span data-ttu-id="87731-116">Będziemy filtrować według kategorii "dane" i wybieramy szablon elementu "SQL Server Database":</span><span class="sxs-lookup"><span data-stu-id="87731-116">We'll filter by the "Data" category and select the "SQL Server Database" item template:</span></span>

![](create-a-database/_static/image2.png)

<span data-ttu-id="87731-117">Nazwamy SQL Server Express bazą danych, w której chcemy utworzyć "NerdDinner. mdf" i nacisnąć przycisk OK.</span><span class="sxs-lookup"><span data-stu-id="87731-117">We'll name the SQL Server Express database we want to create "NerdDinner.mdf" and hit ok.</span></span> <span data-ttu-id="87731-118">Program Visual Studio będzie następnie pytał nas, jeśli chcemy dodać ten plik do naszego katalogu danych \app\_(który jest już skonfigurowany do odczytu i zapisu na listach ACL zabezpieczeń):</span><span class="sxs-lookup"><span data-stu-id="87731-118">Visual Studio will then ask us if we want to add this file to our \App\_Data directory (which is a directory already setup with both read and write security ACLs):</span></span>

![](create-a-database/_static/image3.png)

<span data-ttu-id="87731-119">Po kliknięciu przycisku "tak" zostanie utworzona nowa baza danych i zostanie ona dodana do naszego Eksplorator rozwiązań:</span><span class="sxs-lookup"><span data-stu-id="87731-119">We'll click "Yes" and our new database will be created and added to our Solution Explorer:</span></span>

![](create-a-database/_static/image4.png)

### <a name="creating-tables-within-our-database"></a><span data-ttu-id="87731-120">Tworzenie tabel w bazie danych</span><span class="sxs-lookup"><span data-stu-id="87731-120">Creating Tables within our Database</span></span>

<span data-ttu-id="87731-121">Mamy teraz nową pustą bazę danych.</span><span class="sxs-lookup"><span data-stu-id="87731-121">We now have a new empty database.</span></span> <span data-ttu-id="87731-122">Dodajmy do niego tabele.</span><span class="sxs-lookup"><span data-stu-id="87731-122">Let's add some tables to it.</span></span>

<span data-ttu-id="87731-123">W tym celu przejdziemy do okna karty "Eksplorator serwera" w programie Visual Studio, co umożliwi nam zarządzanie bazami danych i serwerami.</span><span class="sxs-lookup"><span data-stu-id="87731-123">To do this we'll navigate to the "Server Explorer" tab window within Visual Studio, which enables us to manage databases and servers.</span></span> <span data-ttu-id="87731-124">SQL Server Express bazy danych przechowywane w folderze dane\_\app naszej aplikacji będą automatycznie wyświetlane w Eksplorator serwera.</span><span class="sxs-lookup"><span data-stu-id="87731-124">SQL Server Express databases stored in the \App\_Data folder of our application will automatically show up within the Server Explorer.</span></span> <span data-ttu-id="87731-125">Opcjonalnie można użyć ikony "Połącz z bazą danych" w górnej części okna "Eksplorator serwera", aby dodać do listy dodatkowe SQL Server bazy danych (lokalne i zdalne):</span><span class="sxs-lookup"><span data-stu-id="87731-125">We can optionally use the "Connect to Database" icon on the top of the "Server Explorer" window to add additional SQL Server databases (both local and remote) to the list as well:</span></span>

![](create-a-database/_static/image5.png)

<span data-ttu-id="87731-126">Dodamy dwie tabele do naszej bazy danych NerdDinner — jedną do przechowywania naszych obiadów, a druga do śledzenia akceptacji RSVP do nich.</span><span class="sxs-lookup"><span data-stu-id="87731-126">We will add two tables to our NerdDinner database – one to store our Dinners, and the other to track RSVP acceptances to them.</span></span> <span data-ttu-id="87731-127">Aby utworzyć nowe tabele, kliknij prawym przyciskiem myszy folder "tabele" w naszej bazie danych i wybierz polecenie menu "Dodaj nową tabelę":</span><span class="sxs-lookup"><span data-stu-id="87731-127">We can create new tables by right-clicking on the "Tables" folder within our database and choosing the "Add New Table" menu command:</span></span>

![](create-a-database/_static/image6.png)

<span data-ttu-id="87731-128">Spowoduje to otwarcie projektanta tabel, który pozwala skonfigurować schemat naszej tabeli.</span><span class="sxs-lookup"><span data-stu-id="87731-128">This will open up a table designer that allows us to configure the schema of our table.</span></span> <span data-ttu-id="87731-129">W naszej tabeli "obiady" dodamy 10 kolumn danych:</span><span class="sxs-lookup"><span data-stu-id="87731-129">For our "Dinners" table we will add 10 columns of data:</span></span>

![](create-a-database/_static/image7.png)

<span data-ttu-id="87731-130">Chcemy, aby kolumna "DinnerID" była unikatowym kluczem podstawowym dla tabeli.</span><span class="sxs-lookup"><span data-stu-id="87731-130">We want the "DinnerID" column to be a unique primary key for the table.</span></span> <span data-ttu-id="87731-131">Można to skonfigurować, klikając prawym przyciskiem myszy kolumnę "DinnerID" i wybierając pozycję "Ustaw klucz podstawowy":</span><span class="sxs-lookup"><span data-stu-id="87731-131">We can configure this by right-clicking on the "DinnerID" column and choosing the "Set Primary Key" menu item:</span></span>

![](create-a-database/_static/image8.png)

<span data-ttu-id="87731-132">Oprócz tworzenia DinnerID klucza podstawowego należy również skonfigurować go jako kolumnę "Identity", której wartość jest automatycznie zwiększana w miarę dodawania nowych wierszy danych do tabeli (oznacza to, że pierwszy wstawiony wiersz obiadu będzie miał DinnerID 1, drugi wstawiony wiersz będzie miał DinnerID 2 itd.</span><span class="sxs-lookup"><span data-stu-id="87731-132">In addition to making DinnerID a primary key, we also want configure it as an "identity" column whose value is automatically incremented as new rows of data are added to the table (meaning the first inserted Dinner row will have a DinnerID of 1, the second inserted row will have a DinnerID of 2, etc).</span></span>

<span data-ttu-id="87731-133">Możemy to zrobić, wybierając kolumnę "DinnerID", a następnie użyć edytora "Właściwości kolumny", aby ustawić właściwość "(is Identity)" dla kolumny na wartość "yes".</span><span class="sxs-lookup"><span data-stu-id="87731-133">We can do this by selecting the "DinnerID" column and then use the "Column Properties" editor to set the "(Is Identity)" property on the column to "Yes".</span></span> <span data-ttu-id="87731-134">Będziemy używać standardowych ustawień domyślnych tożsamości (Zacznij od 1 i przyrost 1 w każdym nowym wierszu obiadu):</span><span class="sxs-lookup"><span data-stu-id="87731-134">We will use the standard identity defaults (start at 1 and increment 1 on each new Dinner row):</span></span>

![](create-a-database/_static/image9.png)

<span data-ttu-id="87731-135">Następnie zapiszemy naszą tabelę, wpisując CTRL-S lub przy użyciu polecenia menu **zapisz&gt;pliku** .</span><span class="sxs-lookup"><span data-stu-id="87731-135">We'll then save our table by typing Ctrl-S or by using the **File-&gt;Save** menu command.</span></span> <span data-ttu-id="87731-136">Spowoduje to wyświetlenie monitu o podanie nazwy tabeli.</span><span class="sxs-lookup"><span data-stu-id="87731-136">This will prompt us to name the table.</span></span> <span data-ttu-id="87731-137">Nazwamy "obiad":</span><span class="sxs-lookup"><span data-stu-id="87731-137">We'll name it "Dinners":</span></span>

![](create-a-database/_static/image10.png)

<span data-ttu-id="87731-138">Nasza nowa tabela obiadów zostanie następnie wyświetlona w bazie danych w Eksploratorze serwera.</span><span class="sxs-lookup"><span data-stu-id="87731-138">Our new Dinners table will then show up within our database in the server explorer.</span></span>

<span data-ttu-id="87731-139">Następnie powtórzę powyższe kroki i utworzysz tabelę "RSVP".</span><span class="sxs-lookup"><span data-stu-id="87731-139">We'll then repeat the above steps and create a "RSVP" table.</span></span> <span data-ttu-id="87731-140">Ta tabela zawiera 3 kolumny.</span><span class="sxs-lookup"><span data-stu-id="87731-140">This table with have 3 columns.</span></span> <span data-ttu-id="87731-141">Skonfigurujemy kolumnę RsvpID jako klucz podstawowy, a także ustawimy ją jako kolumnę tożsamości:</span><span class="sxs-lookup"><span data-stu-id="87731-141">We will setup the RsvpID column as the primary key, and also make it an identity column:</span></span>

![](create-a-database/_static/image11.png)

<span data-ttu-id="87731-142">Zapiszemy ją i nadaj jej nazwę "RSVP".</span><span class="sxs-lookup"><span data-stu-id="87731-142">We'll save it and give it the name "RSVP".</span></span>

### <a name="setting-up-a-foreign-key-relationship-between-tables"></a><span data-ttu-id="87731-143">Konfigurowanie relacji klucza obcego między tabelami</span><span class="sxs-lookup"><span data-stu-id="87731-143">Setting up a Foreign Key Relationship between Tables</span></span>

<span data-ttu-id="87731-144">Mamy teraz dwie tabele w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="87731-144">We now have two tables within our database.</span></span> <span data-ttu-id="87731-145">Nasz ostatni krok projektowy schematu będzie w stanie skonfigurować relację "jeden do wielu" między tymi dwiema tabelami, dzięki czemu możemy skojarzyć każdy wiersz obiadu z zerowymi lub większymi wierszami RSVP, które mają zastosowanie do tego.</span><span class="sxs-lookup"><span data-stu-id="87731-145">Our last schema design step will be to setup a "one-to-many" relationship between these two tables – so that we can associate each Dinner row with zero or more RSVP rows that apply to it.</span></span> <span data-ttu-id="87731-146">Możemy to zrobić przez skonfigurowanie kolumny "DinnerID" w tabeli protokołu RSVP w taki sposób, aby zawierała ona relację klucza obcego z kolumną "DinnerID" w tabeli "obiady".</span><span class="sxs-lookup"><span data-stu-id="87731-146">We will do this by configuring the RSVP table's "DinnerID" column to have a foreign-key relationship to the "DinnerID" column in the "Dinners" table.</span></span>

<span data-ttu-id="87731-147">W tym celu otworzymy tabelę protokołu RSVP w Projektancie tabel, klikając ją dwukrotnie w Eksploratorze serwera.</span><span class="sxs-lookup"><span data-stu-id="87731-147">To do this we'll open up the RSVP table within the table designer by double-clicking it in the server explorer.</span></span> <span data-ttu-id="87731-148">Następnie wybierz kolumnę "DinnerID" w tym miejscu, kliknij prawym przyciskiem myszy i wybierz "relacje..." polecenie menu kontekstowego:</span><span class="sxs-lookup"><span data-stu-id="87731-148">We'll then select the "DinnerID" column within it, right-click, and choose the "Relationships…" context menu command:</span></span>

![](create-a-database/_static/image12.png)

<span data-ttu-id="87731-149">Spowoduje to wyświetlenie okna dialogowego, za pomocą którego można skonfigurować relacje między tabelami:</span><span class="sxs-lookup"><span data-stu-id="87731-149">This will bring up a dialog that we can use to setup relationships between tables:</span></span>

![](create-a-database/_static/image13.png)

<span data-ttu-id="87731-150">Kliknij przycisk "Dodaj", aby dodać nową relację do okna dialogowego.</span><span class="sxs-lookup"><span data-stu-id="87731-150">We'll click the "Add" button to add a new relationship to the dialog.</span></span> <span data-ttu-id="87731-151">Po dodaniu relacji rozwiniesz węzeł widoku drzewa "tabele i kolumny" w siatce właściwości z prawej strony okna dialogowego, a następnie kliknij przycisk "...". po prawej stronie:</span><span class="sxs-lookup"><span data-stu-id="87731-151">Once a relationship has been added, we'll expand the "Tables and Column Specification" tree-view node within the property grid to the right of the dialog, and then click the "…" button to the right of it:</span></span>

![](create-a-database/_static/image14.png)

<span data-ttu-id="87731-152">Kliknięcie "..." zostanie wyświetlone inne okno dialogowe, które pozwala określić, które tabele i kolumny są uwzględnione w relacji, a także zezwolić nam na nazwę relacji.</span><span class="sxs-lookup"><span data-stu-id="87731-152">Clicking the "…" button will bring up another dialog that allows us to specify which tables and columns are involved in the relationship, as well as allow us to name the relationship.</span></span>

<span data-ttu-id="87731-153">Zmienimy tabelę klucza podstawowego na obiady, a następnie wybierz kolumnę "DinnerID" w tabeli obiady jako klucz podstawowy.</span><span class="sxs-lookup"><span data-stu-id="87731-153">We will change the Primary Key Table to be "Dinners", and select the "DinnerID" column within the Dinners table as the primary key.</span></span> <span data-ttu-id="87731-154">Nasza tabela protokołu RSVP będzie tabelą kluczy obcych i RSVP. Kolumna DinnerID zostanie skojarzona jako klucz obcy:</span><span class="sxs-lookup"><span data-stu-id="87731-154">Our RSVP table will be the foreign-key table, and the RSVP.DinnerID column will be associated as the foreign-key:</span></span>

![](create-a-database/_static/image15.png)

<span data-ttu-id="87731-155">Teraz każdy wiersz w tabeli RSVP zostanie skojarzony z wierszem w tabeli obiadu.</span><span class="sxs-lookup"><span data-stu-id="87731-155">Now each row in the RSVP table will be associated with a row in the Dinner table.</span></span> <span data-ttu-id="87731-156">SQL Server będzie zachować integralność referencyjną dla nas — i uniemożliwić nam Dodawanie nowego wiersza RSVP, jeśli nie wskazuje na prawidłowy wiersz obiadu.</span><span class="sxs-lookup"><span data-stu-id="87731-156">SQL Server will maintain referential integrity for us – and prevent us from adding a new RSVP row if it does not point to a valid Dinner row.</span></span> <span data-ttu-id="87731-157">Zapobiega to również usunięciu wiersza obiadu, jeśli nadal istnieją wiersze RSVP.</span><span class="sxs-lookup"><span data-stu-id="87731-157">It will also prevent us from deleting a Dinner row if there are still RSVP rows referring to it.</span></span>

### <a name="adding-data-to-our-tables"></a><span data-ttu-id="87731-158">Dodawanie danych do naszych tabel</span><span class="sxs-lookup"><span data-stu-id="87731-158">Adding Data to our Tables</span></span>

<span data-ttu-id="87731-159">Zakończmy Dodawanie przykładowych danych do naszej tabeli obiadów.</span><span class="sxs-lookup"><span data-stu-id="87731-159">Let's finish by adding some sample data to our Dinners table.</span></span> <span data-ttu-id="87731-160">Aby dodać dane do tabeli, kliknij ją prawym przyciskiem myszy w Eksplorator serwera i wybierz polecenie "Pokaż dane tabeli":</span><span class="sxs-lookup"><span data-stu-id="87731-160">We can add data to a table by right-clicking on it within the Server Explorer and choosing the "Show Table Data" command:</span></span>

![](create-a-database/_static/image16.png)

<span data-ttu-id="87731-161">Dodamy kilka wierszy danych obiadu, których możemy użyć później, podczas wdrażania aplikacji:</span><span class="sxs-lookup"><span data-stu-id="87731-161">We'll add a few rows of Dinner data that we can use later as we start implementing the application:</span></span>

![](create-a-database/_static/image17.png)

### <a name="next-step"></a><span data-ttu-id="87731-162">Następny krok</span><span class="sxs-lookup"><span data-stu-id="87731-162">Next Step</span></span>

<span data-ttu-id="87731-163">Zakończono Tworzenie bazy danych.</span><span class="sxs-lookup"><span data-stu-id="87731-163">We've finished creating our database.</span></span> <span data-ttu-id="87731-164">Teraz Utwórz klasy modelu, za pomocą których możemy wysyłać zapytania i aktualizować je.</span><span class="sxs-lookup"><span data-stu-id="87731-164">Let's now create model classes that we can use to query and update it.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="87731-165">[Poprzednie](create-a-new-aspnet-mvc-project.md)
> [dalej](build-a-model-with-business-rule-validations.md)</span><span class="sxs-lookup"><span data-stu-id="87731-165">[Previous](create-a-new-aspnet-mvc-project.md)
[Next](build-a-model-with-business-rule-validations.md)</span></span>

---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
title: Dodawanie nowej kategorii do DropDownList przy użyciu interfejsu użytkownika jQuery | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/12/2012
ms.assetid: 44aa1ac4-6ea2-48a2-972d-52710c48eae5
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: 3207079ee468232e5f75b081421241c232936baf
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78538837"
---
# <a name="adding-a-new-category-to-the-dropdownlist-using-jquery-ui"></a><span data-ttu-id="abab1-102">Dodawanie nowej kategorii do metody DropDownList przy użyciu zestawu jQuery UI</span><span class="sxs-lookup"><span data-stu-id="abab1-102">Adding a New Category to the DropDownList using jQuery UI</span></span>

<span data-ttu-id="abab1-103">Autor [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="abab1-103">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="abab1-104">Tag `Select` HTML jest idealnym rozwiązaniem do prezentowania listy stałych danych kategorii, ale często trzeba dodać nową kategorię.</span><span class="sxs-lookup"><span data-stu-id="abab1-104">The HTML `Select` tag is ideal for presenting a list of fixed category data, but often times you need to add a new category.</span></span> <span data-ttu-id="abab1-105">Załóżmy, że chcemy dodać gatunek "Opera" do kategorii w naszej bazie danych?</span><span class="sxs-lookup"><span data-stu-id="abab1-105">Suppose we want to add the genre "Opera" to the categories in our database?</span></span> <span data-ttu-id="abab1-106">W tej sekcji użyjesz interfejsu użytkownika jQuery do dodania okna dialogowego, którego można użyć do dodania nowej kategorii.</span><span class="sxs-lookup"><span data-stu-id="abab1-106">In this section, we will use jQuery UI to add a dialog box we can use to add a new category.</span></span> <span data-ttu-id="abab1-107">Na poniższej ilustracji przedstawiono sposób wyświetlania interfejsu użytkownika w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="abab1-107">The image below shows how the UI will present in the browser.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image1.png)

<span data-ttu-id="abab1-108">Gdy użytkownik wybierze link **Dodaj nowy gatunek** , wyskakujące okno dialogowe wyświetli użytkownikowi komunikat o nowej nazwie gatunku (i opcjonalnie opis).</span><span class="sxs-lookup"><span data-stu-id="abab1-108">When a user selects the **Add New Genre** link, a pop-up dialog box prompts the user for a new genre name (and optionally a description).</span></span> <span data-ttu-id="abab1-109">Na poniższej ilustracji przedstawiono okno dialogowe **Dodawanie gatunku** .</span><span class="sxs-lookup"><span data-stu-id="abab1-109">The image below show the **Add Genre** pop-up dialog.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image2.png)

<span data-ttu-id="abab1-110">Po wprowadzeniu nowej nazwy gatunku, gdy przycisk **Zapisz** jest wypychany, następuje:</span><span class="sxs-lookup"><span data-stu-id="abab1-110">When a new genre name is entered and the **Save** button is pushed, the following happens:</span></span>

1. <span data-ttu-id="abab1-111">Wywołanie AJAX zapisuje dane do metody Create kontrolera gatunku, która zapisuje nowy gatunek do bazy danych i zwraca nowe informacje o gatunku (nazwę i identyfikator gatunku) jako kod JSON.</span><span class="sxs-lookup"><span data-stu-id="abab1-111">An AJAX call posts the data to the Create method of the Genre Controller, which saves the new genre to the database and returns the new genre information (genre name and ID) as JSON.</span></span>
2. <span data-ttu-id="abab1-112">Język JavaScript dodaje nowe dane gatunku do listy wyboru.</span><span class="sxs-lookup"><span data-stu-id="abab1-112">JavaScript adds the new genre data to the select list.</span></span>
3. <span data-ttu-id="abab1-113">Język JavaScript sprawia, że nowy gatunek wybranego elementu.</span><span class="sxs-lookup"><span data-stu-id="abab1-113">JavaScript makes the new genre the selected item.</span></span>

   <span data-ttu-id="abab1-114">Na poniższym obrazie program **Opera** został dodany do bazy danych i wybrany na liście rozwijanej **gatunek** .</span><span class="sxs-lookup"><span data-stu-id="abab1-114">In the image below, **Opera** was added to the database and selected in the **Genre** drop down list.</span></span> 

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image3.png)

<span data-ttu-id="abab1-115">Otwórz plik *Views\StoreManager\Create.cshtml* i Zastąp znacznik gatunek następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="abab1-115">Open the *Views\StoreManager\Create.cshtml* file and replace the genre markup with the following the following code:</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample1.cshtml)]

<span data-ttu-id="abab1-116">`_ChooseGenre` widok częściowy zawiera całą logikę do podłączania kodu JavaScript i jQuery używanego do implementowania funkcji Dodaj nowy gatunek.</span><span class="sxs-lookup"><span data-stu-id="abab1-116">The `_ChooseGenre` partial view will contain all the logic to hook up the JavaScript and jQuery used to implement the add new genre feature.</span></span> <span data-ttu-id="abab1-117">Po zakończeniu wykonywania kodu będzie on prosty do wykonania tego samego przy użyciu interfejsu użytkownika wykonawcy.</span><span class="sxs-lookup"><span data-stu-id="abab1-117">Once we have completed the code it will be simple to do the same with the artist UI.</span></span>

<span data-ttu-id="abab1-118">W Eksplorator rozwiązań kliknij prawym przyciskiem myszy folder *Views\StoreManager* , a następnie wybierz polecenie **Dodaj**, a następnie **Wyświetl**.</span><span class="sxs-lookup"><span data-stu-id="abab1-118">In Solution Explorer, right click the *Views\StoreManager* folder and select **Add**, then **View**.</span></span> <span data-ttu-id="abab1-119">W polu wprowadzanie **nazwy widoku** wprowadź `_ChooseGenre` a następnie wybierz pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="abab1-119">In the **View name** input, enter `_ChooseGenre` then select **Add**.</span></span> <span data-ttu-id="abab1-120">Zastąp znaczniki w pliku *Views\StoreManager\\_ChooseGenre. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="abab1-120">Replace the markup in the *Views\StoreManager\\_ChooseGenre.cshtml* file with the following:</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample2.cshtml)]

<span data-ttu-id="abab1-121">Pierwszy wiersz deklaruje, że przekazujemy `Album` jako nasz model, dokładnie tę samą instrukcję modelową, która znajduje się w widoku Create (Tworzenie).</span><span class="sxs-lookup"><span data-stu-id="abab1-121">The first line declares that we are passing in an `Album` as our model, exactly the same model statement found in the Create view.</span></span> <span data-ttu-id="abab1-122">Kilka następnych wierszy jest znacznikiem pomocnika **etykiet** .</span><span class="sxs-lookup"><span data-stu-id="abab1-122">The next few lines are the **Label** helper markup.</span></span> <span data-ttu-id="abab1-123">Następnym wierszem jest wywołanie pomocnika **DropDownList** , dokładnie takie samo jak w przypadku oryginalnego widoku tworzenia.</span><span class="sxs-lookup"><span data-stu-id="abab1-123">The next line is the **DropDownList** helper call, exactly the same as in the original Create view.</span></span> <span data-ttu-id="abab1-124">Następny wiersz dodaje łącze o nazwie `Add New Genre`i style, takie jak przycisk.</span><span class="sxs-lookup"><span data-stu-id="abab1-124">The next line adds a link with the name `Add New Genre`, and styles it like a button.</span></span> <span data-ttu-id="abab1-125">Wiersz zawierający `ValidationMessageFor` jest kopiowany bezpośrednio z widoku tworzenie.</span><span class="sxs-lookup"><span data-stu-id="abab1-125">The line containing `ValidationMessageFor` is copied directly from the Create view.</span></span> <span data-ttu-id="abab1-126">Następujące wiersze:</span><span class="sxs-lookup"><span data-stu-id="abab1-126">The following lines:</span></span>

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample3.html)]

<span data-ttu-id="abab1-127">tworzy ukryty blok DIV o IDENTYFIKATORze `genreDialog`.</span><span class="sxs-lookup"><span data-stu-id="abab1-127">creates a hidden div, with the ID of `genreDialog`.</span></span> <span data-ttu-id="abab1-128">Będziemy używać platformy jQuery do przełączania okna dialogowego **Dodawanie gatunku** z identyfikatorem `genreDialog` w tym elemencie DIV.</span><span class="sxs-lookup"><span data-stu-id="abab1-128">We will use jQuery to hook up our **Add Genre** dialog box with the ID `genreDialog` in this div.</span></span> <span data-ttu-id="abab1-129">Ostatnie dwa Tagi skryptu zawierają linki do plików języka JavaScript, które zostaną użyte do zaimplementowania funkcji Dodaj nowy gatunek.</span><span class="sxs-lookup"><span data-stu-id="abab1-129">The last two script tags contain links to the JavaScript files we will use to implement the add new genre feature.</span></span> <span data-ttu-id="abab1-130">Plik */scripts/chooseGenre.js* jest dostępny dla Ciebie w projekcie. przeanalizuje go później w samouczku.</span><span class="sxs-lookup"><span data-stu-id="abab1-130">The */Scripts/chooseGenre.js* file is provided for you in the project, we will examine it later in the tutorial.</span></span>

<span data-ttu-id="abab1-131">Uruchom aplikację i kliknij przycisk **Dodaj nowy gatunek** .</span><span class="sxs-lookup"><span data-stu-id="abab1-131">Run the application and click on the **Add New Genre** button.</span></span> <span data-ttu-id="abab1-132">W oknie dialogowym **Dodaj gatunek** wprowadź wartość **Opera** w polu **Nazwa** wejściowa.</span><span class="sxs-lookup"><span data-stu-id="abab1-132">In the **Add Genre** dialog box, enter **Opera** in the **Name** input box.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image4.png)

<span data-ttu-id="abab1-133">Kliknij przycisk **Zapisz**.</span><span class="sxs-lookup"><span data-stu-id="abab1-133">Click the **Save** button.</span></span> <span data-ttu-id="abab1-134">Wywołanie AJAX tworzy kategorię Opera, a następnie wypełnia listę rozwijaną za pomocą programu Opera i ustawia program Opera jako wybrany gatunek.</span><span class="sxs-lookup"><span data-stu-id="abab1-134">An AJAX call creates the Opera category and then populates the dropdown list with Opera, and sets Opera as the selected genre.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image5.png)

<span data-ttu-id="abab1-135">Wprowadź wykonawcę, tytuł i cenę, a następnie wybierz przycisk **Utwórz** .</span><span class="sxs-lookup"><span data-stu-id="abab1-135">Enter an artist, title and price, then select the **Create** button.</span></span> <span data-ttu-id="abab1-136">Jeśli wprowadzisz cenę niższą niż $8,99, nowy album pojawi się u góry widoku indeksu.</span><span class="sxs-lookup"><span data-stu-id="abab1-136">If you enter a price less than $8.99, the new album will appear at the top of the Index view.</span></span> <span data-ttu-id="abab1-137">Sprawdź, czy nowy wpis albumu został zapisany w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="abab1-137">Verify the new album entry was saved in the database.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image6.png)

<span data-ttu-id="abab1-138">Spróbuj utworzyć nowy gatunek z tylko jedną literą.</span><span class="sxs-lookup"><span data-stu-id="abab1-138">Try creating a new genre with only one letter.</span></span> <span data-ttu-id="abab1-139">Poniższy kod w pliku *Models\Genre.cs* ustawia minimalną i maksymalną długość nazwy gatunku.</span><span class="sxs-lookup"><span data-stu-id="abab1-139">The following code in the *Models\Genre.cs* file sets the minimum and maximum length of the genre name.</span></span>

[!code-csharp[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample4.cs)]

<span data-ttu-id="abab1-140">Raporty weryfikacji po stronie klienta należy wprowadzić ciąg z zakresu od 2 do 20 znaków.</span><span class="sxs-lookup"><span data-stu-id="abab1-140">Client side validation reports you must enter a string between 2 and 20 characters.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image7.png)

### <a name="examining-how-a-new-genre-is-added-to-the-database-and-the-select-list"></a><span data-ttu-id="abab1-141">Sprawdzanie, w jaki sposób nowy gatunek jest dodawany do bazy danych i listy wyboru.</span><span class="sxs-lookup"><span data-stu-id="abab1-141">Examining How a New Genre is Added to the Database and the Select List.</span></span>

<span data-ttu-id="abab1-142">Otwórz plik *Scripts\chooseGenre.js* i zapoznaj się z kodem.</span><span class="sxs-lookup"><span data-stu-id="abab1-142">Open the *Scripts\chooseGenre.js* file and examine the code.</span></span>

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample5.js)]

<span data-ttu-id="abab1-143">Drugi wiersz używa identyfikatora `genreDialog` do tworzenia okna dialogowego znacznika DIV w pliku *Views\StoreManager\\_ChooseGenre. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="abab1-143">The second line uses the ID `genreDialog` to create a dialog box on the div tag in the *Views\StoreManager\\_ChooseGenre.cshtml* file.</span></span> <span data-ttu-id="abab1-144">Większość nazwanych parametrów nie ma wyjaśnień.</span><span class="sxs-lookup"><span data-stu-id="abab1-144">Most of the named parameters are self explanatory.</span></span> <span data-ttu-id="abab1-145">Parametr `autoOpen` ma wartość FAŁSZ, a wybranie przycisku **Utwórz gatunek** spowoduje otwarcie okna dialogowego w sposób jawny (jest to opisane w drugim dniu).</span><span class="sxs-lookup"><span data-stu-id="abab1-145">The `autoOpen` parameter is set to false, selecting the **Create Genre** button will open the dialogue explicitly (this is described latter on).</span></span> <span data-ttu-id="abab1-146">Okno dialogowe ma dwa przyciski, **Zapisz** i **Anuluj**.</span><span class="sxs-lookup"><span data-stu-id="abab1-146">The dialog has two buttons, **Save** and **Cancel**.</span></span> <span data-ttu-id="abab1-147">Przycisk **Anuluj** zamyka okno dialogowe.</span><span class="sxs-lookup"><span data-stu-id="abab1-147">The **Cancel** button closes the dialog.</span></span> <span data-ttu-id="abab1-148">Poniższy kod przedstawia funkcję **Save** Button.</span><span class="sxs-lookup"><span data-stu-id="abab1-148">The following code shows the **Save** button function.</span></span>

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample6.js)]

<span data-ttu-id="abab1-149">Wybrano `var createGenreForm` z identyfikatora `createGenreForm`.</span><span class="sxs-lookup"><span data-stu-id="abab1-149">The `var createGenreForm` is selected from the `createGenreForm` ID.</span></span> <span data-ttu-id="abab1-150">Identyfikator `createGenreForm` został ustawiony w następującym kodzie znalezionym w pliku *Views\Genre\\_CreateGenre. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="abab1-150">The `createGenreForm` ID was set in the following code found in the *Views\Genre\\_CreateGenre.cshtml* file.</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample7.cshtml)]

<span data-ttu-id="abab1-151">Przeciążenie pomocnika [HTML. BeginForm](https://msdn.microsoft.com/library/dd492714.aspx) użyte w pliku *Views\Genre\\_CreateGenre. cshtml* generuje HTML z atrybutem Action zawierającym adres URL służący do przesyłania formularza.</span><span class="sxs-lookup"><span data-stu-id="abab1-151">The [Html.BeginForm](https://msdn.microsoft.com/library/dd492714.aspx) helper overload used in the *Views\Genre\\_CreateGenre.cshtml* file generates HTML with an action attribute containing the URL to submit the form.</span></span> <span data-ttu-id="abab1-152">Możesz to zobaczyć, wyświetlając stronę tworzenie albumu w przeglądarce i wybierając pozycję Pokaż źródło w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="abab1-152">You can see this by displaying the create album page in a browser and selecting show source in the browser.</span></span> <span data-ttu-id="abab1-153">Poniższy znacznik pokazuje wygenerowany kod HTML zawierający tag formularza.</span><span class="sxs-lookup"><span data-stu-id="abab1-153">The following markup shows the generated HTML containing the form tag.</span></span>

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample8.html)]

<span data-ttu-id="abab1-154">Linia `$.post` jQuery wykonuje wywołanie AJAX do atrybutu Action (`/StoreManager/Create`) i przekazuje dane z okna dialogowego **Tworzenie gatunku** .</span><span class="sxs-lookup"><span data-stu-id="abab1-154">The jQuery `$.post` line makes an AJAX call to the action attribute (`/StoreManager/Create`) and passes in the data from the **Create Genre** dialog box.</span></span> <span data-ttu-id="abab1-155">Dane składają się z nazwy nowego gatunku i opcjonalnego opisu.</span><span class="sxs-lookup"><span data-stu-id="abab1-155">The data consists of the name for the new genre and an optional description.</span></span> <span data-ttu-id="abab1-156">Jeśli wywołanie AJAX zakończy się pomyślnie, Nowa nazwa i wartość gatunku są dodawane do Select Markup, a nowy gatunek jest ustawiony na wybraną wartość.</span><span class="sxs-lookup"><span data-stu-id="abab1-156">If the AJAX call is successful, the new genre name and value are added to the Select markup, and the new genre is set to the selected value.</span></span> <span data-ttu-id="abab1-157">Ponieważ jest to dynamicznie generowana adiustacja, nie można wyświetlić nowej opcji Select, wyświetlając źródło w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="abab1-157">Because this is dynamically generated markup, you can't see the new select option by viewing the source in the browser.</span></span> <span data-ttu-id="abab1-158">Nowy kod HTML można zobaczyć przy użyciu narzędzi deweloperskich programu IE 9 F12.</span><span class="sxs-lookup"><span data-stu-id="abab1-158">You can see the new HTML with the IE 9 F12 developer tools.</span></span> <span data-ttu-id="abab1-159">Aby wyświetlić nową opcję wyboru, w programie Internet Explorer 9 Naciśnij klawisz F12, aby uruchomić narzędzia deweloperskie F12.</span><span class="sxs-lookup"><span data-stu-id="abab1-159">To view the new select option, in Internet Explorer 9, hit the F12 key to start the F12 developer tools.</span></span> <span data-ttu-id="abab1-160">Przejdź do strony Tworzenie i Dodaj nowy gatunek, aby nowy gatunek został wybrany na liście wyboru gatunku.</span><span class="sxs-lookup"><span data-stu-id="abab1-160">Navigate to the Create page and add a new genre so the new genre is selected in the genre select list.</span></span> <span data-ttu-id="abab1-161">W narzędziach deweloperskich F12:</span><span class="sxs-lookup"><span data-stu-id="abab1-161">In the F12 developer tools:</span></span>

1. <span data-ttu-id="abab1-162">Wybierz kartę HTML.</span><span class="sxs-lookup"><span data-stu-id="abab1-162">Select the HTML tab.</span></span>
2. <span data-ttu-id="abab1-163">Naciśnij ikonę odświeżania.</span><span class="sxs-lookup"><span data-stu-id="abab1-163">Hit the refresh icon.</span></span>  
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image8.png)
3. <span data-ttu-id="abab1-164">W polu wyszukiwania wprowadź GenreID.</span><span class="sxs-lookup"><span data-stu-id="abab1-164">In the search box, enter GenreID.</span></span>
4. <span data-ttu-id="abab1-165">Przy użyciu następnej ikony</span><span class="sxs-lookup"><span data-stu-id="abab1-165">Using the next icon,</span></span>   
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image9.png)  
   <span data-ttu-id="abab1-166">Przejdź do następującego tagu SELECT:</span><span class="sxs-lookup"><span data-stu-id="abab1-166">navigate to the following select tag:</span></span>

    [!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample9.html)]
5. <span data-ttu-id="abab1-167">Rozwiń ostatnią wartość opcji.</span><span class="sxs-lookup"><span data-stu-id="abab1-167">Expand the last option value.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image10.png)

<span data-ttu-id="abab1-168">Poniższy kod w pliku *Scripts\chooseGenre.js* pokazuje, jak przycisk **Dodaj nowy gatunek** jest połączony z zdarzeniem kliknięcia i jak zostanie utworzone okno dialogowe **Dodaj nowy gatunek** .</span><span class="sxs-lookup"><span data-stu-id="abab1-168">The following code in the *Scripts\chooseGenre.js* file shows the how the **Add New Genre** button gets connected to the click event, and how the **Add New Genre** dialog box is created.</span></span>

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample10.js)]

<span data-ttu-id="abab1-169">Pierwszy wiersz tworzy funkcję kliknięcia dołączoną do przycisku **Dodaj nowy gatunek** .</span><span class="sxs-lookup"><span data-stu-id="abab1-169">The first line creates a click function attached to the **Add New Genre** button.</span></span> <span data-ttu-id="abab1-170">Następujące znaczniki z pliku Views\StoreManager\\_ChooseGenre. cshtml pokazują, jak zostanie utworzony przycisk **Dodaj nowy gatunek** :</span><span class="sxs-lookup"><span data-stu-id="abab1-170">The following markup from the Views\StoreManager\\_ChooseGenre.cshtml file shows how the **Add New Genre** button is created:</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample11.cshtml)]

<span data-ttu-id="abab1-171">Metoda Load tworzy i otwiera okno dialogowe Dodawanie gatunku i wywołuje metodę jQuery `parse`, dzięki czemu sprawdzanie poprawności klienta odbywa się na danych wprowadzonych w oknie dialogowym.</span><span class="sxs-lookup"><span data-stu-id="abab1-171">The load method creates and opens the Add Genre dialog and calls the jQuery `parse` method so client validation occurs on data entered in the dialog.</span></span>

<span data-ttu-id="abab1-172">W tej sekcji przedstawiono sposób tworzenia okna dialogowego, które może służyć do dodawania nowych danych kategorii do listy wyboru.</span><span class="sxs-lookup"><span data-stu-id="abab1-172">In this section you have learned how to create a dialog that can be used to add new category data to a select list.</span></span> <span data-ttu-id="abab1-173">Za pomocą tej samej procedury można utworzyć interfejs użytkownika w celu dodania nowego wykonawcy do listy wyboru wykonawcy.</span><span class="sxs-lookup"><span data-stu-id="abab1-173">You can follow the same procedure to create UI to add a new artist to the artist select list.</span></span> <span data-ttu-id="abab1-174">W tym samouczku przedstawiono przegląd pracy z pomocnikiem HTML ASP.NET MVC **DropDownList**.</span><span class="sxs-lookup"><span data-stu-id="abab1-174">This tutorial has given an overview of working with the ASP.NET MVC HTML helper **DropDownList**.</span></span> <span data-ttu-id="abab1-175">Aby uzyskać dodatkowe informacje na temat pracy z **DropDownList**, zobacz sekcję dotyczącą dodawania odwołań poniżej.</span><span class="sxs-lookup"><span data-stu-id="abab1-175">For additional information on working with the **DropDownList**, see the addition references section below.</span></span> <span data-ttu-id="abab1-176">Poinformuj nas o tym, czy ten samouczek był pomocny.</span><span class="sxs-lookup"><span data-stu-id="abab1-176">Please let us know if this tutorial has been helpful.</span></span>

<span data-ttu-id="abab1-177">Rick. Anderson [at] Microsoft. com</span><span class="sxs-lookup"><span data-stu-id="abab1-177">Rick.Anderson[at]Microsoft.com</span></span>

### <a name="additional-references"></a><span data-ttu-id="abab1-178">Dodatkowe informacje</span><span class="sxs-lookup"><span data-stu-id="abab1-178">Additional References</span></span>

- <span data-ttu-id="abab1-179">[ASP.NET MVC — kaskadowe listy rozwijane samouczków](https://weblogs.asp.net/raduenuca/archive/2011/03/06/asp-net-mvc-cascading-dropdown-lists-tutorial-part-1-defining-the-problem-and-the-context.aspx) według [Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)</span><span class="sxs-lookup"><span data-stu-id="abab1-179">[ASP.NET MVC–Cascading Dropdown Lists Tutorial](https://weblogs.asp.net/raduenuca/archive/2011/03/06/asp-net-mvc-cascading-dropdown-lists-tutorial-part-1-defining-the-problem-and-the-context.aspx) by [Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)</span></span>
- <span data-ttu-id="abab1-180">[Wybrane](https://harvesthq.github.com/chosen/) Wtyczka języka JavaScript, która obsługuje wiele zaznaczeń i filtrowanie.</span><span class="sxs-lookup"><span data-stu-id="abab1-180">[Chosen](https://harvesthq.github.com/chosen/) A JavaScript plugin that support multi-select and filtering.</span></span>

### <a name="contributors"></a><span data-ttu-id="abab1-181">Współautorzy</span><span class="sxs-lookup"><span data-stu-id="abab1-181">Contributors</span></span>

- [<span data-ttu-id="abab1-182">Radu Enuca</span><span class="sxs-lookup"><span data-stu-id="abab1-182">Radu Enuca</span></span>](https://weblogs.asp.net/raduenuca/default.aspx)
- <span data-ttu-id="abab1-183">Jean-Sébastien Goupil</span><span class="sxs-lookup"><span data-stu-id="abab1-183">Jean-Sébastien Goupil</span></span>
- [<span data-ttu-id="abab1-184">Brad Wilson</span><span class="sxs-lookup"><span data-stu-id="abab1-184">Brad Wilson</span></span>](http://bradwilson.typepad.com/)

### <a name="reviewers"></a><span data-ttu-id="abab1-185">Recenzenci</span><span class="sxs-lookup"><span data-stu-id="abab1-185">Reviewers</span></span>

- <span data-ttu-id="abab1-186">Jean-Sébastien Goupil</span><span class="sxs-lookup"><span data-stu-id="abab1-186">Jean-Sébastien Goupil</span></span>
- [<span data-ttu-id="abab1-187">Brad Wilson</span><span class="sxs-lookup"><span data-stu-id="abab1-187">Brad Wilson</span></span>](http://bradwilson.typepad.com/)
- <span data-ttu-id="abab1-188">Jan Pope</span><span class="sxs-lookup"><span data-stu-id="abab1-188">Mike Pope</span></span>
- <span data-ttu-id="abab1-189">Dykstra Tomasz</span><span class="sxs-lookup"><span data-stu-id="abab1-189">Tom Dykstra</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="abab1-190">Wstecz</span><span class="sxs-lookup"><span data-stu-id="abab1-190">Previous</span></span>](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)

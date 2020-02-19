---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
title: Badanie sposobu, w jaki ASP.NET MVC szkieletuje pomocnika DropDownList | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/12/2012
ms.assetid: 8921d7f2-21f0-427a-8b27-2df7251174b0
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
msc.type: authoredcontent
ms.openlocfilehash: 275b20ad964b3e8ddc272a7448f0740ed0891eff
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457612"
---
# <a name="examining--how--aspnet-mvc-scaffolds-the-dropdownlist-helper"></a><span data-ttu-id="ecebb-102">Badanie sposobu tworzenia szkieletu pomocnika DropDownList przez wzorzec ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="ecebb-102">Examining  how  ASP.NET MVC scaffolds the DropDownList Helper</span></span>

<span data-ttu-id="ecebb-103">Autor [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ecebb-103">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="ecebb-104">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy folder *controllers* , a następnie wybierz polecenie **Dodaj kontroler**.</span><span class="sxs-lookup"><span data-stu-id="ecebb-104">In **Solution Explorer**, right-click the *Controllers* folder and then select **Add Controller**.</span></span> <span data-ttu-id="ecebb-105">Nadaj nazwę kontrolerowi **StoreManagerController**.</span><span class="sxs-lookup"><span data-stu-id="ecebb-105">Name the controller **StoreManagerController**.</span></span> <span data-ttu-id="ecebb-106">Ustaw opcje okna dialogowego **Dodaj kontroler** , jak pokazano na poniższej ilustracji.</span><span class="sxs-lookup"><span data-stu-id="ecebb-106">Set the options for the **Add Controller** dialog as shown in the image below.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image1.png)

<span data-ttu-id="ecebb-107">Edytuj widok *StoreManager\Index.cshtml* i Usuń `AlbumArtUrl`.</span><span class="sxs-lookup"><span data-stu-id="ecebb-107">Edit the *StoreManager\Index.cshtml* view and remove `AlbumArtUrl`.</span></span> <span data-ttu-id="ecebb-108">Usunięcie `AlbumArtUrl` sprawia, że prezentacja będzie bardziej czytelna.</span><span class="sxs-lookup"><span data-stu-id="ecebb-108">Removing `AlbumArtUrl` will make the presentation more readable.</span></span> <span data-ttu-id="ecebb-109">Poniżej przedstawiono kompletny kod.</span><span class="sxs-lookup"><span data-stu-id="ecebb-109">The completed code is shown below.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample1.cshtml)]

<span data-ttu-id="ecebb-110">Otwórz plik *Controllers\StoreManagerController.cs* i znajdź metodę `Index`.</span><span class="sxs-lookup"><span data-stu-id="ecebb-110">Open the *Controllers\StoreManagerController.cs* file and find the `Index` method.</span></span> <span data-ttu-id="ecebb-111">Dodaj klauzulę `OrderBy`, aby albumy były sortowane według cen.</span><span class="sxs-lookup"><span data-stu-id="ecebb-111">Add the `OrderBy` clause so the albums will be sorted by price.</span></span> <span data-ttu-id="ecebb-112">Pełny kod jest przedstawiony poniżej.</span><span class="sxs-lookup"><span data-stu-id="ecebb-112">The complete code is shown below.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample2.cs)]

<span data-ttu-id="ecebb-113">Sortowanie według cen ułatwia testowanie zmian w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="ecebb-113">Sorting by price will make it easier to test changes to the database.</span></span> <span data-ttu-id="ecebb-114">Podczas testowania metod edycji i tworzenia można użyć niskich cen, aby zapisane dane były wyświetlane jako pierwsze.</span><span class="sxs-lookup"><span data-stu-id="ecebb-114">When you are testing the edit and create methods, you can use a low price so the saved data will appear first.</span></span>

<span data-ttu-id="ecebb-115">Otwórz plik *StoreManager\Edit.cshtml* .</span><span class="sxs-lookup"><span data-stu-id="ecebb-115">Open the *StoreManager\Edit.cshtml* file.</span></span> <span data-ttu-id="ecebb-116">Dodaj następujący wiersz tuż po tagu legendy.</span><span class="sxs-lookup"><span data-stu-id="ecebb-116">Add the following line just after the legend tag.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample3.cshtml)]

<span data-ttu-id="ecebb-117">Poniższy kod przedstawia kontekst tej zmiany:</span><span class="sxs-lookup"><span data-stu-id="ecebb-117">The following code shows the context of this change:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample4.cshtml)]

<span data-ttu-id="ecebb-118">`AlbumId` jest wymagana do wprowadzania zmian w rekordzie albumu.</span><span class="sxs-lookup"><span data-stu-id="ecebb-118">The `AlbumId` is required to make changes to an album record.</span></span>

<span data-ttu-id="ecebb-119">Naciśnij klawisze CTRL+F5, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="ecebb-119">Press CTRL+F5 to run the application.</span></span> <span data-ttu-id="ecebb-120">Wybierz łącze **administratora** , a następnie wybierz link **Utwórz nowy** , aby utworzyć nowy album.</span><span class="sxs-lookup"><span data-stu-id="ecebb-120">Select to the **Admin** link, then select the **Create New** link to create a new album.</span></span> <span data-ttu-id="ecebb-121">Sprawdź, czy Zapisano informacje o albumie.</span><span class="sxs-lookup"><span data-stu-id="ecebb-121">Verify the album information was saved.</span></span> <span data-ttu-id="ecebb-122">Edytuj album i sprawdź, czy wprowadzone zmiany są utrwalane.</span><span class="sxs-lookup"><span data-stu-id="ecebb-122">Edit an album and verify the changes you made are persisted.</span></span>

### <a name="the-album-schema"></a><span data-ttu-id="ecebb-123">Schemat albumu</span><span class="sxs-lookup"><span data-stu-id="ecebb-123">The Album Schema</span></span>

<span data-ttu-id="ecebb-124">Kontroler `StoreManager` utworzony przez mechanizm szkieletu MVC umożliwia CRUD (tworzenie, odczytywanie, aktualizowanie i usuwanie) dostępu do albumów w bazie danych magazynu utworów muzycznych.</span><span class="sxs-lookup"><span data-stu-id="ecebb-124">The `StoreManager` controller created by the MVC scaffolding mechanism allows CRUD (Create, Read, Update, Delete) access to the albums in the music store database.</span></span> <span data-ttu-id="ecebb-125">Poniżej przedstawiono schemat informacji o albumie:</span><span class="sxs-lookup"><span data-stu-id="ecebb-125">The schema for album information is shown below:</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image2.png)

<span data-ttu-id="ecebb-126">W tabeli `Albums` nie jest przechowywany gatunek i opis albumu, który przechowuje klucz obcy w tabeli `Genres`.</span><span class="sxs-lookup"><span data-stu-id="ecebb-126">The `Albums` table does not store the album genre and description, it stores a foreign key to the `Genres` table.</span></span> <span data-ttu-id="ecebb-127">Tabela `Genres` zawiera nazwę i Opis gatunku.</span><span class="sxs-lookup"><span data-stu-id="ecebb-127">The `Genres` table contains the genre name and description.</span></span> <span data-ttu-id="ecebb-128">Podobnie tabela `Albums` nie zawiera nazwy wykonawców albumów, ale klucz obcy do tabeli `Artists`.</span><span class="sxs-lookup"><span data-stu-id="ecebb-128">Likewise, the `Albums` table doesn't contain the album artists name, but a foreign key to the `Artists` table.</span></span> <span data-ttu-id="ecebb-129">Tabela `Artists` zawiera nazwę wykonawcy.</span><span class="sxs-lookup"><span data-stu-id="ecebb-129">The `Artists` table contains the artist's name.</span></span> <span data-ttu-id="ecebb-130">W przypadku badania danych w tabeli `Albums` można zobaczyć, że każdy wiersz zawiera klucz obcy do tabeli `Genres` i klucz obcy do tabeli `Artists`.</span><span class="sxs-lookup"><span data-stu-id="ecebb-130">If you examine the data in the `Albums` table, you can see each row contains a foreign key to the `Genres` table and a foreign key to the `Artists` table.</span></span> <span data-ttu-id="ecebb-131">Na poniższej ilustracji przedstawiono niektóre dane tabeli z tabeli `Albums`.</span><span class="sxs-lookup"><span data-stu-id="ecebb-131">The image below show some table data from the `Albums` table.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image3.png)

### <a name="the-html-select-tag"></a><span data-ttu-id="ecebb-132">Tag SELECT HTML</span><span class="sxs-lookup"><span data-stu-id="ecebb-132">The HTML Select Tag</span></span>

<span data-ttu-id="ecebb-133">Element `<select>` HTML (utworzony przez pomocnika HTML [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) ) służy do wyświetlania kompletnej listy wartości (takich jak lista gatunków).</span><span class="sxs-lookup"><span data-stu-id="ecebb-133">The HTML `<select>` element (created by the HTML [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) helper) is used to display a complete list of values (such as the list of genres).</span></span> <span data-ttu-id="ecebb-134">W przypadku formularzy edycji, gdy bieżąca wartość jest znana, na liście wyboru można wyświetlić bieżącą wartość.</span><span class="sxs-lookup"><span data-stu-id="ecebb-134">For edit forms, when the current value is known, the select list can display the current value.</span></span> <span data-ttu-id="ecebb-135">Te wartości zostały wcześniej wykorzystane, gdy ustawimy wybraną wartość na **komedia**.</span><span class="sxs-lookup"><span data-stu-id="ecebb-135">We saw this previously when we set the selected value to **Comedy**.</span></span> <span data-ttu-id="ecebb-136">Lista wyboru jest idealnym rozwiązaniem do wyświetlania danych kategorii lub kluczy obcych.</span><span class="sxs-lookup"><span data-stu-id="ecebb-136">The select list is ideal for displaying category or foreign key data.</span></span> <span data-ttu-id="ecebb-137">Element `<select>` dla gatunku klucz obcy zawiera listę możliwych nazw gatunku, ale podczas zapisywania formularza Właściwość gatunek jest aktualizowana o wartości klucza obcego gatunku, a nie nazwy wyświetlanego gatunku.</span><span class="sxs-lookup"><span data-stu-id="ecebb-137">The `<select>` element for the Genre foreign key displays the list of possible genre names, but when you save the form the Genre property is updated with the Genre foreign key value, not the displayed genre name.</span></span> <span data-ttu-id="ecebb-138">Na poniższym obrazie wybrany gatunek to **Disco** , a wykonawca to **Donną lato**.</span><span class="sxs-lookup"><span data-stu-id="ecebb-138">In the image below, the genre selected is **Disco** and the artist is **Donna Summer**.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image4.png)

### <a name="examining-the-aspnet-mvc-scaffolded-code"></a><span data-ttu-id="ecebb-139">Badanie kodu szkieletowego ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="ecebb-139">Examining the ASP.NET MVC Scaffolded Code</span></span>

<span data-ttu-id="ecebb-140">Otwórz plik *Controllers\StoreManagerController.cs* i znajdź metodę `HTTP GET Create`.</span><span class="sxs-lookup"><span data-stu-id="ecebb-140">Open the *Controllers\StoreManagerController.cs* file and find the `HTTP GET Create` method.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample5.cs)]

<span data-ttu-id="ecebb-141">Metoda `Create` dodaje dwa obiekty [SelectList](https://msdn.microsoft.com/library/system.web.mvc.selectlist.aspx) do `ViewBag`, jeden do zawierają informacje o gatunku i jeden do zawierają informacje o wykonawcy.</span><span class="sxs-lookup"><span data-stu-id="ecebb-141">The `Create` method adds two [SelectList](https://msdn.microsoft.com/library/system.web.mvc.selectlist.aspx) objects to the `ViewBag`, one to contain the genre information, and one to contain the artist information.</span></span> <span data-ttu-id="ecebb-142">Użycie przeciążenia konstruktora [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) powyżej przyjmuje trzy argumenty:</span><span class="sxs-lookup"><span data-stu-id="ecebb-142">The [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) constructor overload used above takes three arguments:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample6.cs)]

1. <span data-ttu-id="ecebb-143">*elementy*: element [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) zawierający elementy z listy.</span><span class="sxs-lookup"><span data-stu-id="ecebb-143">*items*: An [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) containing the items in the list.</span></span> <span data-ttu-id="ecebb-144">W powyższym przykładzie lista gatunków zwracana przez `db.Genres`.</span><span class="sxs-lookup"><span data-stu-id="ecebb-144">In the example above, the list of genres returned by `db.Genres`.</span></span>
2. <span data-ttu-id="ecebb-145">*dataValueField*: Nazwa właściwości na liście **IEnumerable** , która zawiera wartość klucza.</span><span class="sxs-lookup"><span data-stu-id="ecebb-145">*dataValueField*: The name of the property in the **IEnumerable** list that contains the key value.</span></span> <span data-ttu-id="ecebb-146">W powyższym przykładzie `GenreId` i `ArtistId`.</span><span class="sxs-lookup"><span data-stu-id="ecebb-146">In the example above, `GenreId` and `ArtistId`.</span></span>
3. <span data-ttu-id="ecebb-147">*dataTextField*: Nazwa właściwości na liście **IEnumerable** , która zawiera informacje do wyświetlenia.</span><span class="sxs-lookup"><span data-stu-id="ecebb-147">*dataTextField*: The name of the property in the **IEnumerable** list that contains the information to display.</span></span> <span data-ttu-id="ecebb-148">W tabeli artyści i gatunek jest używane pole `name`.</span><span class="sxs-lookup"><span data-stu-id="ecebb-148">In both the artists and genre table, the `name` field is used.</span></span>

<span data-ttu-id="ecebb-149">Otwórz plik *Views\StoreManager\Create.cshtml* i przejrzyj znacznik pomocnika `Html.DropDownList` dla pola gatunek.</span><span class="sxs-lookup"><span data-stu-id="ecebb-149">Open the *Views\StoreManager\Create.cshtml* file and examine the `Html.DropDownList` helper markup for the genre field.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample7.cshtml)]

<span data-ttu-id="ecebb-150">Pierwszy wiersz pokazuje, że widok tworzenia ma `Album` model.</span><span class="sxs-lookup"><span data-stu-id="ecebb-150">The first line shows that the create view takes an `Album` model.</span></span> <span data-ttu-id="ecebb-151">W podanej powyżej metodzie `Create` żaden model nie został przesłany, więc widok pobiera model `Album` o **wartości null** .</span><span class="sxs-lookup"><span data-stu-id="ecebb-151">In the `Create` method shown above, no model was passed, so the view gets a **null** `Album` model.</span></span> <span data-ttu-id="ecebb-152">W tym momencie tworzymy nowy album, dlatego nie mamy żadnych danych `Album`.</span><span class="sxs-lookup"><span data-stu-id="ecebb-152">At this point we are creating a new album so we don't have any `Album` data for it.</span></span>

<span data-ttu-id="ecebb-153">Przeciążanie [HTML. DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) pokazane powyżej Pobiera nazwę pola do powiązania z modelem.</span><span class="sxs-lookup"><span data-stu-id="ecebb-153">The [Html.DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) overload shown above takes the name of the field to bind to the model.</span></span> <span data-ttu-id="ecebb-154">Używa ona również tej nazwy do wyszukania obiektu **ViewBag** zawierającego obiekt [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) .</span><span class="sxs-lookup"><span data-stu-id="ecebb-154">It also uses this name to look for a **ViewBag** object containing a [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) object.</span></span> <span data-ttu-id="ecebb-155">Przy użyciu tego przeciążenia należy nazwać obiekt **ViewBag SelectList** `GenreId`.</span><span class="sxs-lookup"><span data-stu-id="ecebb-155">Using this overload, you are required to name the **ViewBag SelectList** object `GenreId`.</span></span> <span data-ttu-id="ecebb-156">Drugi parametr (`String.Empty`) jest tekstem, który ma być wyświetlany, gdy nie wybrano żadnego elementu.</span><span class="sxs-lookup"><span data-stu-id="ecebb-156">The second parameter (`String.Empty`) is the text to display when no item is selected.</span></span> <span data-ttu-id="ecebb-157">Jest to dokładnie to, co chcemy zrobić podczas tworzenia nowego albumu.</span><span class="sxs-lookup"><span data-stu-id="ecebb-157">This is exactly what we want when creating a new album.</span></span> <span data-ttu-id="ecebb-158">Jeśli usunięto drugi parametr i użyto następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="ecebb-158">If you removed the second parameter and used the following code:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample8.cshtml)]

<span data-ttu-id="ecebb-159">Lista wyboru będzie domyślnie do pierwszego elementu lub skały w naszym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="ecebb-159">The select list would default to the first element, or Rock in our sample.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image5.png)

<span data-ttu-id="ecebb-160">Badanie metody `HTTP POST Create`.</span><span class="sxs-lookup"><span data-stu-id="ecebb-160">Examining the `HTTP POST Create` method.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample9.cs)]

<span data-ttu-id="ecebb-161">To Przeciążenie metody `Create` przyjmuje obiekt `album`, utworzony przez system powiązania modelu MVC ASP.NET z wartości formularza ogłoszone.</span><span class="sxs-lookup"><span data-stu-id="ecebb-161">This overload of the `Create` method takes an `album` object, created by the ASP.NET MVC model binding system from the form values posted.</span></span> <span data-ttu-id="ecebb-162">Gdy przesyłasz nowy album, jeśli stan modelu jest prawidłowy i nie ma żadnych błędów bazy danych, nowy album zostanie dodany do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ecebb-162">When you submit a new album, if model state is valid and there are no database errors, the new album is added the database.</span></span> <span data-ttu-id="ecebb-163">Na poniższej ilustracji przedstawiono tworzenie nowego albumu.</span><span class="sxs-lookup"><span data-stu-id="ecebb-163">The following image shows the creation of a new album.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image6.png)

<span data-ttu-id="ecebb-164">Można użyć [Narzędzia programu Fiddler](http://www.fiddler2.com/fiddler2/) do sprawdzenia wartości ogłoszonych formularzy, które są używane przez powiązanie modelu MVC ASP.NET do tworzenia obiektu albumu.</span><span class="sxs-lookup"><span data-stu-id="ecebb-164">You can use the [fiddler tool](http://www.fiddler2.com/fiddler2/) to examine the posted form values that ASP.NET MVC model binding uses to create the album object.</span></span>

<span data-ttu-id="ecebb-165">![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image7.png).</span><span class="sxs-lookup"><span data-stu-id="ecebb-165">![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image7.png).</span></span>

### <a name="refactoring-the-viewbag-selectlist-creation"></a><span data-ttu-id="ecebb-166">Refaktoryzacja tworzenia SelectList ViewBag</span><span class="sxs-lookup"><span data-stu-id="ecebb-166">Refactoring the ViewBag SelectList Creation</span></span>

<span data-ttu-id="ecebb-167">Metody `Edit` i `HTTP POST Create` mają identyczny kod w celu skonfigurowania **SelectList** w **ViewBag**.</span><span class="sxs-lookup"><span data-stu-id="ecebb-167">Both the `Edit` methods and the `HTTP POST Create` method have identical code to set up the **SelectList** in the **ViewBag**.</span></span> <span data-ttu-id="ecebb-168">W duchu [suchego](http://en.wikipedia.org/wiki/Don't_repeat_yourself)kod będzie refaktoryzacji.</span><span class="sxs-lookup"><span data-stu-id="ecebb-168">In the spirit of [DRY](http://en.wikipedia.org/wiki/Don't_repeat_yourself), we will refactor this code.</span></span> <span data-ttu-id="ecebb-169">Użyjemy tego kodu refaktoryzacji później.</span><span class="sxs-lookup"><span data-stu-id="ecebb-169">We'll make use of this refactored code later.</span></span>

<span data-ttu-id="ecebb-170">Utwórz nową metodę, aby dodać gatunek i **SelectList** wykonawcy do **ViewBag**.</span><span class="sxs-lookup"><span data-stu-id="ecebb-170">Create a new method to add a genre and artist **SelectList** to the **ViewBag**.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample10.cs)]

<span data-ttu-id="ecebb-171">Zastąp dwa wiersze ustawiające `ViewBag` w każdej `Create` i `Edit` metody z wywołaniem metody `SetGenreArtistViewBag`.</span><span class="sxs-lookup"><span data-stu-id="ecebb-171">Replace the two lines setting the `ViewBag` in each of the `Create` and `Edit` methods with a call to the `SetGenreArtistViewBag` method.</span></span> <span data-ttu-id="ecebb-172">Poniżej przedstawiono kompletny kod.</span><span class="sxs-lookup"><span data-stu-id="ecebb-172">The completed code is shown below.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample11.cs)]

<span data-ttu-id="ecebb-173">Utwórz nowy album i edytuj album, aby sprawdzić, czy zmiany zostały wykonane.</span><span class="sxs-lookup"><span data-stu-id="ecebb-173">Create a new album and edit an album to verify the changes work.</span></span>

### <a name="explicitly-passing-the-selectlist-to-the-dropdownlist"></a><span data-ttu-id="ecebb-174">Jawne przekazanie SelectList do DropDownList</span><span class="sxs-lookup"><span data-stu-id="ecebb-174">Explicitly Passing the SelectList to the DropDownList</span></span>

<span data-ttu-id="ecebb-175">Widoki tworzenie i edytowanie utworzone przez szkielet ASP.NET MVC używają następujących przeciążeń **DropDownList** :</span><span class="sxs-lookup"><span data-stu-id="ecebb-175">The create and edit views created by the ASP.NET MVC scaffolding use the following **DropDownList** overload:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample12.cs)]

<span data-ttu-id="ecebb-176">Poniżej pokazano `DropDownList` znaczników dla widoku tworzenia.</span><span class="sxs-lookup"><span data-stu-id="ecebb-176">The `DropDownList` markup for the create view is shown below.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample13.cshtml)]

<span data-ttu-id="ecebb-177">Ponieważ właściwość `ViewBag` dla `SelectList` ma nazwę `GenreId`, pomocnik **DropDownList** będzie używać `GenreId`**SelectList** w **ViewBag**.</span><span class="sxs-lookup"><span data-stu-id="ecebb-177">Because the `ViewBag` property for the `SelectList` is named `GenreId`, the **DropDownList** helper will use the `GenreId`**SelectList** in the **ViewBag**.</span></span> <span data-ttu-id="ecebb-178">W poniższym przeciążeniu **DropDownList** `SelectList` jest jawnie przekazywać.</span><span class="sxs-lookup"><span data-stu-id="ecebb-178">In the following **DropDownList** overload, the `SelectList` is explicitly passed in.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample14.cs)]

<span data-ttu-id="ecebb-179">Otwórz plik *Views\StoreManager\Edit.cshtml* i Zmień wywołanie **DropDownList** , aby jawnie przekazać **SelectList**, używając powyżej przeciążenia.</span><span class="sxs-lookup"><span data-stu-id="ecebb-179">Open the *Views\StoreManager\Edit.cshtml* file, and change the **DropDownList** call to explicitly pass in the **SelectList**, using the overload above.</span></span> <span data-ttu-id="ecebb-180">Zrób to dla kategorii gatunek.</span><span class="sxs-lookup"><span data-stu-id="ecebb-180">Do this for the Genre category.</span></span> <span data-ttu-id="ecebb-181">Ukończony kod jest przedstawiony poniżej:</span><span class="sxs-lookup"><span data-stu-id="ecebb-181">The completed code is shown below:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample15.cshtml)]

<span data-ttu-id="ecebb-182">Uruchom aplikację i kliknij link **administratora** , a następnie przejdź do albumu Jazz i wybierz łącze **Edytuj** .</span><span class="sxs-lookup"><span data-stu-id="ecebb-182">Run the application and click the **Admin** link, then navigate to a Jazz album and select the **Edit** link.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image8.png)

<span data-ttu-id="ecebb-183">Zamiast wyświetlania Jazz jako aktualnie wybranego gatunku, zostanie wyświetlona skała.</span><span class="sxs-lookup"><span data-stu-id="ecebb-183">Instead of showing Jazz as the currently selected genre, Rock is displayed.</span></span> <span data-ttu-id="ecebb-184">Gdy argument ciągu (właściwość do powiązania) i obiekt **SelectList** mają taką samą nazwę, wybrana wartość nie jest używana.</span><span class="sxs-lookup"><span data-stu-id="ecebb-184">When the string argument (the property to bind) and the **SelectList** object have the same name, the selected value is not used.</span></span> <span data-ttu-id="ecebb-185">Gdy nie zostanie podana wybrana wartość, przeglądarki domyślnie są pierwszym elementem w **SelectList**(który jest w powyższym przykładzie **skalą** ).</span><span class="sxs-lookup"><span data-stu-id="ecebb-185">When there is no selected value provided, browsers default to the first element in the **SelectList**(which is **Rock** in the example above).</span></span> <span data-ttu-id="ecebb-186">Jest to znane ograniczenie pomocnika **DropDownList** .</span><span class="sxs-lookup"><span data-stu-id="ecebb-186">This is a known limitation of the **DropDownList** helper.</span></span>

<span data-ttu-id="ecebb-187">Otwórz plik *Controllers\StoreManagerController.cs* i Zmień nazwy obiektów **SelectList** na `Genres` i `Artists`.</span><span class="sxs-lookup"><span data-stu-id="ecebb-187">Open the *Controllers\StoreManagerController.cs* file and change the **SelectList** object names to `Genres` and `Artists`.</span></span> <span data-ttu-id="ecebb-188">Ukończony kod jest przedstawiony poniżej:</span><span class="sxs-lookup"><span data-stu-id="ecebb-188">The completed code is shown below:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample16.cs)]

<span data-ttu-id="ecebb-189">Nazwy gatunków i artystów są lepszymi nazwami dla kategorii, ponieważ zawierają więcej niż tylko identyfikator każdej kategorii.</span><span class="sxs-lookup"><span data-stu-id="ecebb-189">The names Genres and Artists are better names for the categories, as they contain more than just the ID of each category.</span></span> <span data-ttu-id="ecebb-190">Refaktoryzacja była wcześniej płacona.</span><span class="sxs-lookup"><span data-stu-id="ecebb-190">The refactoring we did earlier paid off.</span></span> <span data-ttu-id="ecebb-191">Zamiast zmieniać **ViewBag** w czterech metodach, zmiany zostały odizolowane od metody `SetGenreArtistViewBag`.</span><span class="sxs-lookup"><span data-stu-id="ecebb-191">Instead of changing the **ViewBag** in four methods, our changes were isolated to the `SetGenreArtistViewBag` method.</span></span>

<span data-ttu-id="ecebb-192">Zmień wywołanie **DropDownList** w widokach tworzenie i edytowanie, aby użyć nowych nazw **SelectList** .</span><span class="sxs-lookup"><span data-stu-id="ecebb-192">Change the **DropDownList** call in the create and edit views to use the new **SelectList** names.</span></span> <span data-ttu-id="ecebb-193">Poniżej przedstawiono nowe znaczniki widoku edycji:</span><span class="sxs-lookup"><span data-stu-id="ecebb-193">The new markup for the edit view is shown below:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample17.cshtml)]

<span data-ttu-id="ecebb-194">Widok tworzenia wymaga pustego ciągu, aby zapobiec wyświetlaniu pierwszego elementu w SelectList.</span><span class="sxs-lookup"><span data-stu-id="ecebb-194">The Create view requires an empty string to prevent the first item in the SelectList from being displayed.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample18.cshtml)]

<span data-ttu-id="ecebb-195">Utwórz nowy album i edytuj album, aby sprawdzić, czy zmiany zostały wykonane.</span><span class="sxs-lookup"><span data-stu-id="ecebb-195">Create a new album and edit an album to verify the changes work.</span></span> <span data-ttu-id="ecebb-196">Przetestuj edycję kodu, wybierając album o gatunku innym niż Rock.</span><span class="sxs-lookup"><span data-stu-id="ecebb-196">Test the edit code by selecting an album with a genre other than Rock.</span></span>

### <a name="using-a-view-model-with-the-dropdownlist-helper"></a><span data-ttu-id="ecebb-197">Używanie modelu widoku z pomocnikiem DropDownList</span><span class="sxs-lookup"><span data-stu-id="ecebb-197">Using a View Model with the DropDownList Helper</span></span>

<span data-ttu-id="ecebb-198">Utwórz nową klasę w folderze modele widoków o nazwie `AlbumSelectListViewModel`.</span><span class="sxs-lookup"><span data-stu-id="ecebb-198">Create a new class in the ViewModels folder named `AlbumSelectListViewModel`.</span></span> <span data-ttu-id="ecebb-199">Zastąp kod w klasie `AlbumSelectListViewModel` następującym:</span><span class="sxs-lookup"><span data-stu-id="ecebb-199">Replace the code in the `AlbumSelectListViewModel` class with the following:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample19.cs)]

<span data-ttu-id="ecebb-200">Konstruktor `AlbumSelectListViewModel` pobiera album, listę artystów i gatunek oraz tworzy obiekt zawierający album i `SelectList` dla gatunku i artystów.</span><span class="sxs-lookup"><span data-stu-id="ecebb-200">The `AlbumSelectListViewModel` constructor takes an album, a list of artists and genres and creates an object containing the album and a `SelectList` for genres and artists.</span></span>

<span data-ttu-id="ecebb-201">Skompiluj projekt, aby `AlbumSelectListViewModel` był dostępny podczas tworzenia widoku w następnym kroku.</span><span class="sxs-lookup"><span data-stu-id="ecebb-201">Build the project so the `AlbumSelectListViewModel` is available when we create a view in the next step.</span></span>

<span data-ttu-id="ecebb-202">Dodaj do `StoreManagerController`metodę `EditVM`.</span><span class="sxs-lookup"><span data-stu-id="ecebb-202">Add an `EditVM` method to the `StoreManagerController`.</span></span> <span data-ttu-id="ecebb-203">Poniżej przedstawiono kompletny kod.</span><span class="sxs-lookup"><span data-stu-id="ecebb-203">The completed code is shown below.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample20.cs)]

<span data-ttu-id="ecebb-204">Kliknij prawym przyciskiem myszy `AlbumSelectListViewModel`, wybierz opcję **Rozwiąż**, a następnie **za pomocą MvcMusicStore. modele widoków;** .</span><span class="sxs-lookup"><span data-stu-id="ecebb-204">Right click `AlbumSelectListViewModel`, select **Resolve**, then **using MvcMusicStore.ViewModels;**.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image9.png)

<span data-ttu-id="ecebb-205">Alternatywnie można dodać następującą instrukcję using:</span><span class="sxs-lookup"><span data-stu-id="ecebb-205">Alternatively, you can add the following using statement:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample21.cs)]

<span data-ttu-id="ecebb-206">Kliknij prawym przyciskiem myszy `EditVM` i wybierz polecenie **Dodaj widok**.</span><span class="sxs-lookup"><span data-stu-id="ecebb-206">Right click `EditVM` and select **Add View**.</span></span> <span data-ttu-id="ecebb-207">Użyj opcji przedstawionych poniżej.</span><span class="sxs-lookup"><span data-stu-id="ecebb-207">Use the options shown below.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image10.png)

<span data-ttu-id="ecebb-208">Wybierz pozycję **Dodaj**, a następnie zastąp zawartość pliku *Views\StoreManager\EditVM.cshtml* następującym:</span><span class="sxs-lookup"><span data-stu-id="ecebb-208">Select **Add**, then replace the contents of the *Views\StoreManager\EditVM.cshtml* file with the following:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample22.cshtml)]

<span data-ttu-id="ecebb-209">Znaczniki `EditVM` są bardzo podobne do oryginalnego znacznika `Edit` z następującymi wyjątkami.</span><span class="sxs-lookup"><span data-stu-id="ecebb-209">The `EditVM` markup is very similar to the original `Edit` markup with the following exceptions.</span></span>

- <span data-ttu-id="ecebb-210">Właściwości modelu w widoku `Edit` są `model.property`(na przykład `model.Title`).</span><span class="sxs-lookup"><span data-stu-id="ecebb-210">Model properties in the `Edit` view are of the form `model.property`(for example, `model.Title` ).</span></span> <span data-ttu-id="ecebb-211">Właściwości modelu w widoku `EditVm` są `model.Album.property`(na przykład `model.Album.Title`).</span><span class="sxs-lookup"><span data-stu-id="ecebb-211">Model properties in the `EditVm` view are of the form `model.Album.property`(for example, `model.Album.Title`).</span></span> <span data-ttu-id="ecebb-212">Dzieje się tak, ponieważ widok `EditVM` jest przekazywać kontener dla `Album`, a nie `Album` jak w widoku `Edit`.</span><span class="sxs-lookup"><span data-stu-id="ecebb-212">That's because the `EditVM` view is passed a container for an `Album`, not an `Album` as in the `Edit` view.</span></span>
- <span data-ttu-id="ecebb-213">Drugi parametr **DropDownList** pochodzi z modelu widoku, a nie **ViewBag**.</span><span class="sxs-lookup"><span data-stu-id="ecebb-213">The **DropDownList** second parameter comes from the view model, not the **ViewBag**.</span></span>
- <span data-ttu-id="ecebb-214">Pomocnik **BeginForm** w `EditVM` widoku jawnie zapisuje z powrotem do `Edit` metody akcji.</span><span class="sxs-lookup"><span data-stu-id="ecebb-214">The **BeginForm** helper in the `EditVM` view explicitly posts back to the `Edit` action method.</span></span> <span data-ttu-id="ecebb-215">Przez zaksięgowanie z powrotem do akcji `Edit` nie musimy pisać akcji `HTTP POST EditVM` i mogą ponownie wykorzystać akcję `HTTP POST` `Edit`.</span><span class="sxs-lookup"><span data-stu-id="ecebb-215">By posting back to the `Edit` action, we don't have to write an `HTTP POST EditVM` action and can reuse the `HTTP POST` `Edit` action.</span></span>

<span data-ttu-id="ecebb-216">Uruchom aplikację i edytuj album.</span><span class="sxs-lookup"><span data-stu-id="ecebb-216">Run the application and edit an album.</span></span> <span data-ttu-id="ecebb-217">Zmień adres URL, aby użyć `EditVM`.</span><span class="sxs-lookup"><span data-stu-id="ecebb-217">Change the URL to use `EditVM`.</span></span> <span data-ttu-id="ecebb-218">Zmień pole i naciśnij przycisk **Zapisz** , aby sprawdzić, czy kod działa.</span><span class="sxs-lookup"><span data-stu-id="ecebb-218">Change a field and hit the **Save** button to verify the code is working.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image11.png)

### <a name="which-approach-should-you-use"></a><span data-ttu-id="ecebb-219">Której metody należy użyć?</span><span class="sxs-lookup"><span data-stu-id="ecebb-219">Which Approach Should You Use?</span></span>

<span data-ttu-id="ecebb-220">Wszystkie trzy wymienione metody są akceptowalne.</span><span class="sxs-lookup"><span data-stu-id="ecebb-220">All three approaches shown are acceptable.</span></span> <span data-ttu-id="ecebb-221">Wielu deweloperów preferuje jawne przekazywanie `SelectList` do `DropDownList` przy użyciu `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="ecebb-221">Many developers prefer to explicitly pass the `SelectList` to the `DropDownList` using the `ViewBag`.</span></span> <span data-ttu-id="ecebb-222">To podejście ma dodatkową zaletę, która zapewnia elastyczność używania bardziej odpowiedniej nazwy dla kolekcji.</span><span class="sxs-lookup"><span data-stu-id="ecebb-222">This approach has the added advantage of giving you the flexibility of using a more appropriate name for the collection.</span></span> <span data-ttu-id="ecebb-223">Jedynym zastrzeżeń nie jest nazwa obiektu `ViewBag SelectList` o tej samej nazwie co właściwość modelu.</span><span class="sxs-lookup"><span data-stu-id="ecebb-223">The one caveat is you cannot name the `ViewBag SelectList` object the same name as the model property.</span></span>

<span data-ttu-id="ecebb-224">Niektórzy deweloperzy preferują podejście ViewModel.</span><span class="sxs-lookup"><span data-stu-id="ecebb-224">Some developers prefer the ViewModel approach.</span></span> <span data-ttu-id="ecebb-225">Inne zapoznają się z bardziej szczegółowym znacznikiem i wygenerowanym kodem HTML ViewModel.</span><span class="sxs-lookup"><span data-stu-id="ecebb-225">Others consider the more verbose markup and generated HTML of the ViewModel approach a disadvantage.</span></span>

<span data-ttu-id="ecebb-226">W tej sekcji poznasz trzy podejścia do używania **DropDownList** z danymi kategorii.</span><span class="sxs-lookup"><span data-stu-id="ecebb-226">In this section we have learned three approaches to using the **DropDownList** with category data.</span></span> <span data-ttu-id="ecebb-227">W następnej sekcji pokażemy, jak dodać nową kategorię.</span><span class="sxs-lookup"><span data-stu-id="ecebb-227">In the next section, we'll show how to add a new category.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ecebb-228">[Poprzednie](using-the-dropdownlist-helper-with-aspnet-mvc.md)
> [dalej](adding-a-new-category-to-the-dropdownlist-using-jquery-ui.md)</span><span class="sxs-lookup"><span data-stu-id="ecebb-228">[Previous](using-the-dropdownlist-helper-with-aspnet-mvc.md)
[Next](adding-a-new-category-to-the-dropdownlist-using-jquery-ui.md)</span></span>

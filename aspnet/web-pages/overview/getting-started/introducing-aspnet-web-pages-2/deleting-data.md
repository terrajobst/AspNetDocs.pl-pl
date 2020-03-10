---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
title: Wprowadzenie do stron sieci Web ASP.NET — usuwanie danych bazy danych | Microsoft Docs
author: Rick-Anderson
description: W tym samouczku przedstawiono sposób usuwania poszczególnych wpisów bazy danych. Przyjęto założenie, że została ukończona seria przez zaktualizowanie danych bazy danych w sieci Web ASP.NET...
ms.author: riande
ms.date: 01/02/2018
ms.assetid: 75b5c1cf-84bd-434f-8a86-85c568eb5b09
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
msc.type: authoredcontent
ms.openlocfilehash: c8620fc1abc61d514bdc039c66f7a84e67e89abe
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78629039"
---
# <a name="introducing-aspnet-web-pages---deleting-database-data"></a><span data-ttu-id="fba13-104">Wprowadzenie do stron sieci Web ASP.NET — usuwanie danych bazy danych</span><span class="sxs-lookup"><span data-stu-id="fba13-104">Introducing ASP.NET Web Pages - Deleting Database Data</span></span>

<span data-ttu-id="fba13-105">Autor [FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="fba13-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="fba13-106">W tym samouczku przedstawiono sposób usuwania poszczególnych wpisów bazy danych.</span><span class="sxs-lookup"><span data-stu-id="fba13-106">This tutorial shows you how to delete an individual database entry.</span></span> <span data-ttu-id="fba13-107">Przyjęto założenie, że wykonano serię przez [zaktualizowanie danych bazy danych na stronach sieci Web ASP.NET](updating-data.md).</span><span class="sxs-lookup"><span data-stu-id="fba13-107">It assumes you have completed the series through [Updating Database Data in ASP.NET Web Pages](updating-data.md).</span></span>
> 
> <span data-ttu-id="fba13-108">Zawartość:</span><span class="sxs-lookup"><span data-stu-id="fba13-108">What you'll learn:</span></span>
> 
> - <span data-ttu-id="fba13-109">Jak wybrać pojedynczy rekord z listy rekordów.</span><span class="sxs-lookup"><span data-stu-id="fba13-109">How to select an individual record from a listing of records.</span></span>
> - <span data-ttu-id="fba13-110">Jak usunąć pojedynczy rekord z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="fba13-110">How to delete a single record from a database.</span></span>
> - <span data-ttu-id="fba13-111">Jak sprawdzić, czy określony przycisk został kliknięty w formularzu.</span><span class="sxs-lookup"><span data-stu-id="fba13-111">How to check that a specific button was clicked in a form.</span></span>
>   
> 
> <span data-ttu-id="fba13-112">Omówione funkcje/technologie:</span><span class="sxs-lookup"><span data-stu-id="fba13-112">Features/technologies discussed:</span></span>
> 
> - <span data-ttu-id="fba13-113">Pomocnik `WebGrid`.</span><span class="sxs-lookup"><span data-stu-id="fba13-113">The `WebGrid` helper.</span></span>
> - <span data-ttu-id="fba13-114">Polecenie SQL `Delete`.</span><span class="sxs-lookup"><span data-stu-id="fba13-114">The SQL `Delete` command.</span></span>
> - <span data-ttu-id="fba13-115">Metoda `Database.Execute` do uruchamiania polecenia SQL `Delete`.</span><span class="sxs-lookup"><span data-stu-id="fba13-115">The `Database.Execute` method to run a SQL `Delete` command.</span></span>

## <a name="what-youll-build"></a><span data-ttu-id="fba13-116">Co będziesz kompilować</span><span class="sxs-lookup"><span data-stu-id="fba13-116">What You'll Build</span></span>

<span data-ttu-id="fba13-117">W poprzednim samouczku przedstawiono sposób aktualizowania istniejącego rekordu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="fba13-117">In the previous tutorial, you learned how to update an existing database record.</span></span> <span data-ttu-id="fba13-118">Ten samouczek jest podobny, z tą różnicą, że zamiast aktualizacji rekordu, zostanie usunięty.</span><span class="sxs-lookup"><span data-stu-id="fba13-118">This tutorial is similar, except that instead of updating the record, you'll delete it.</span></span> <span data-ttu-id="fba13-119">Procesy są znacznie takie same, z tą różnicą, że usuwanie jest prostsze, więc ten samouczek będzie krótki.</span><span class="sxs-lookup"><span data-stu-id="fba13-119">The processes are much the same, except that deleting is simpler, so this tutorial will be short.</span></span>

<span data-ttu-id="fba13-120">Na stronie *filmy* należy zaktualizować pomocnika `WebGrid` tak, aby wyświetlał link **usuwania** obok każdego filmu, który zostanie dołączony do dodanego wcześniej linku **edycji** .</span><span class="sxs-lookup"><span data-stu-id="fba13-120">In the *Movies* page, you'll update the `WebGrid` helper so that it displays a **Delete** link next to each movie to accompany the **Edit** link you added earlier.</span></span>

![Strona filmów z linkiem usuwania dla każdego filmu](deleting-data/_static/image1.png)

<span data-ttu-id="fba13-122">Podobnie jak w przypadku edycji, kliknięcie linku **usuwania** spowoduje przejście do innej strony, gdzie informacje o filmie są już w postaci:</span><span class="sxs-lookup"><span data-stu-id="fba13-122">As with editing, when you click the **Delete** link, it takes you to a different page, where the movie information is already in a form:</span></span>

![Usuń stronę filmu z wyświetlonym filmem](deleting-data/_static/image2.png)

<span data-ttu-id="fba13-124">Następnie możesz kliknąć przycisk, aby trwale usunąć rekord.</span><span class="sxs-lookup"><span data-stu-id="fba13-124">You can then click the button to delete the record permanently.</span></span>

## <a name="adding-a-delete-link-to-the-movie-listing"></a><span data-ttu-id="fba13-125">Dodawanie linku usuwania do listy filmów</span><span class="sxs-lookup"><span data-stu-id="fba13-125">Adding a Delete Link to the Movie Listing</span></span>

<span data-ttu-id="fba13-126">Zacznij od dodania linku **usuwania** do pomocnika `WebGrid`.</span><span class="sxs-lookup"><span data-stu-id="fba13-126">You'll start by adding a **Delete** link to the `WebGrid` helper.</span></span> <span data-ttu-id="fba13-127">Ten link jest podobny do linku **edycji** , który został dodany w poprzednim samouczku.</span><span class="sxs-lookup"><span data-stu-id="fba13-127">This link is similar to the **Edit** link you added in a previous tutorial.</span></span>

<span data-ttu-id="fba13-128">Otwórz plik *Films. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="fba13-128">Open the *Movies.cshtml* file.</span></span>

<span data-ttu-id="fba13-129">Zmień `WebGrid` znaczników w treści strony, dodając kolumnę.</span><span class="sxs-lookup"><span data-stu-id="fba13-129">Change the `WebGrid` markup in the body of the page by adding a column.</span></span> <span data-ttu-id="fba13-130">Oto zmodyfikowano znaczniki:</span><span class="sxs-lookup"><span data-stu-id="fba13-130">Here's the modified markup:</span></span>

[!code-html[Main](deleting-data/samples/sample1.html?highlight=9-10)]

<span data-ttu-id="fba13-131">Nowa kolumna to:</span><span class="sxs-lookup"><span data-stu-id="fba13-131">The new column is this one:</span></span>

[!code-html[Main](deleting-data/samples/sample2.html)]

<span data-ttu-id="fba13-132">W sposób skonfigurowania siatki, kolumna **Edycja** jest lewej strony siatki, a kolumna **Usuń** jest przysunięta do prawej strony.</span><span class="sxs-lookup"><span data-stu-id="fba13-132">The way the grid is configured, the **Edit** column is leftmost in the grid and the **Delete** column is rightmost.</span></span> <span data-ttu-id="fba13-133">(Istnieje przecinek po `Year` kolumnie teraz, jeśli nie zauważysz, że.) Nie ma żadnych specjalnych informacji o tym, gdzie znajdują się te kolumny linków, i można je łatwo umieścić obok siebie.</span><span class="sxs-lookup"><span data-stu-id="fba13-133">(There's a comma after the `Year` column now, in case you didn't notice that.) There's nothing special about where these link columns go, and you could as easily put them next to each other.</span></span> <span data-ttu-id="fba13-134">W takim przypadku są one oddzielone, aby utrudnić ich przetworzenie.</span><span class="sxs-lookup"><span data-stu-id="fba13-134">In this case, they're separate to make them harder to get mixed up.</span></span>

![Strona filmów z linkami Edytuj i szczegóły oznaczona jako widoczna, aby pokazać, że nie są obok siebie](deleting-data/_static/image3.png)

<span data-ttu-id="fba13-136">Nowa kolumna zawiera link (`<a>` element), którego tekst mówi "Delete".</span><span class="sxs-lookup"><span data-stu-id="fba13-136">The new column shows a link (`<a>` element) whose text says "Delete".</span></span> <span data-ttu-id="fba13-137">Obiekt docelowy linku (jego atrybut `href`) to kod, który ostatecznie jest rozpoznawany jako taki, jak ten adres URL, z wartością `id` inną dla każdego filmu:</span><span class="sxs-lookup"><span data-stu-id="fba13-137">The target of the link (its `href` attribute) is code that ultimately resolves to something like this URL, with the `id` value different for each movie:</span></span>

[!code-css[Main](deleting-data/samples/sample3.css)]

<span data-ttu-id="fba13-138">Ten link spowoduje wywołanie strony o nazwie *DeleteMovie* i przekazanie jej identyfikatora wybranego filmu.</span><span class="sxs-lookup"><span data-stu-id="fba13-138">This link will invoke a page named *DeleteMovie* and pass it the ID of the movie you've selected.</span></span>

<span data-ttu-id="fba13-139">Ten samouczek nie zawiera szczegółowych informacji o sposobie konstruowania tego linku, ponieważ jest prawie identyczny z linkiem **edycji** z poprzedniego samouczka ([Aktualizowanie danych bazy danych na stronach sieci Web ASP.NET](updating-data.md)).</span><span class="sxs-lookup"><span data-stu-id="fba13-139">This tutorial won't go into detail about how this link is constructed, because it's almost identical to the **Edit** link from the previous tutorial ([Updating Database Data in ASP.NET Web Pages](updating-data.md)).</span></span>

## <a name="creating-the-delete-page"></a><span data-ttu-id="fba13-140">Tworzenie strony usuwania</span><span class="sxs-lookup"><span data-stu-id="fba13-140">Creating the Delete Page</span></span>

<span data-ttu-id="fba13-141">Teraz można utworzyć stronę, która będzie elementem docelowym linku **usuwania** w siatce.</span><span class="sxs-lookup"><span data-stu-id="fba13-141">Now you can create the page that will be the target for the **Delete** link in the grid.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="fba13-142">**Ważne** Technika pierwszego wyboru rekordu do usunięcia, a następnie użycie osobnej strony i przycisku w celu potwierdzenia, że proces jest niezwykle istotny dla bezpieczeństwa.</span><span class="sxs-lookup"><span data-stu-id="fba13-142">**Important** The technique of first selecting a record to delete and then using a separate page and button to confirm the process is extremely important for security.</span></span> <span data-ttu-id="fba13-143">Po przeczytaniu w poprzednich samouczkach, wprowadzanie *wszelkich* zmian w witrynie sieci Web powinno odbywać się *zawsze* przy użyciu formularza &mdash;, czyli przy użyciu operacji post protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="fba13-143">As you've read in previous tutorials, making *any* sort of change to your website should *always* be done using a form &mdash; that is, using an HTTP POST operation.</span></span> <span data-ttu-id="fba13-144">Jeśli można zmienić witrynę po prostu przez kliknięcie linku (czyli przy użyciu operacji GET), użytkownicy mogą tworzyć proste żądania do witryny i usuwać dane.</span><span class="sxs-lookup"><span data-stu-id="fba13-144">If you made it possible to change the site just by clicking a link (that is, using a GET operation), people could make simple requests to your site and delete your data.</span></span> <span data-ttu-id="fba13-145">Nawet przeszukiwarka wyszukiwarki, która indeksuje witrynę, może przypadkowo usunąć dane po prostu przez następujące linki.</span><span class="sxs-lookup"><span data-stu-id="fba13-145">Even a search-engine crawler that's indexing your site could inadvertently delete data just by following links.</span></span>
> 
> <span data-ttu-id="fba13-146">Gdy aplikacja pozwala użytkownikom na zmianę rekordu, należy zaprezentować rekord użytkownikowi do edycji.</span><span class="sxs-lookup"><span data-stu-id="fba13-146">When your app lets people change a record, you have to present the record to the user for editing anyway.</span></span> <span data-ttu-id="fba13-147">Może jednak być skłonny do pominięcia tego kroku w celu usunięcia rekordu.</span><span class="sxs-lookup"><span data-stu-id="fba13-147">But you might be tempted to skip this step for deleting a record.</span></span> <span data-ttu-id="fba13-148">Nie pomijaj tego kroku, chociaż.</span><span class="sxs-lookup"><span data-stu-id="fba13-148">Don't skip that step, though.</span></span> <span data-ttu-id="fba13-149">(Przydatne jest również, aby użytkownicy mogli zobaczyć rekord i potwierdzić, że usuwają one rekord, którego zamierzą).</span><span class="sxs-lookup"><span data-stu-id="fba13-149">(It's also helpful for users to see the record and confirm that they're deleting the record that they intended.)</span></span>
> 
> <span data-ttu-id="fba13-150">W kolejnym zestawie samouczków zobaczysz, jak dodać funkcje logowania, aby użytkownik musiał się zalogować przed usunięciem rekordu.</span><span class="sxs-lookup"><span data-stu-id="fba13-150">In a subsequent tutorial set, you'll see how to add login functionality so a user would have to log in before deleting a record.</span></span>

<span data-ttu-id="fba13-151">Utwórz stronę o nazwie *DeleteMovie. cshtml* i Zastąp zawartość pliku następującym znacznikiem:</span><span class="sxs-lookup"><span data-stu-id="fba13-151">Create a page named *DeleteMovie.cshtml* and replace what's in the file with the following markup:</span></span>

[!code-cshtml[Main](deleting-data/samples/sample4.cshtml)]

<span data-ttu-id="fba13-152">Znaczniki te są podobne do stron *EditMovie* , z tą różnicą, że zamiast używania pól tekstowych (`<input type="text">`), znaczniki zawierają `<span>` elementy.</span><span class="sxs-lookup"><span data-stu-id="fba13-152">This markup is like the *EditMovie* pages, except that instead of using text boxes (`<input type="text">`), the markup includes `<span>` elements.</span></span> <span data-ttu-id="fba13-153">Nie ma nic tutaj do edycji.</span><span class="sxs-lookup"><span data-stu-id="fba13-153">There's nothing here to edit.</span></span> <span data-ttu-id="fba13-154">Wystarczy wyświetlić szczegóły filmu, aby użytkownicy mogli upewnić się, że usuwa właściwy film.</span><span class="sxs-lookup"><span data-stu-id="fba13-154">All you have to do is display the movie details so that users can make sure that they're deleting the right movie.</span></span>

<span data-ttu-id="fba13-155">Znacznik zawiera już link umożliwiający powrót użytkownika do strony z listą filmów.</span><span class="sxs-lookup"><span data-stu-id="fba13-155">The markup already contains a link that lets the user return to the movie listing page.</span></span>

<span data-ttu-id="fba13-156">Podobnie jak na stronie *EditMovie* , identyfikator wybranego filmu jest przechowywany w ukrytym polu.</span><span class="sxs-lookup"><span data-stu-id="fba13-156">As in the *EditMovie* page, the ID of the selected movie is stored in a hidden field.</span></span> <span data-ttu-id="fba13-157">(Jest ona przenoszona do strony w pierwszym miejscu jako wartość ciągu zapytania). Istnieje `Html.ValidationSummary` wywołanie, które spowoduje wyświetlenie błędów walidacji.</span><span class="sxs-lookup"><span data-stu-id="fba13-157">(It's passed into the page in the first place as a query string value.) There's an `Html.ValidationSummary` call that will display validation errors.</span></span> <span data-ttu-id="fba13-158">W takim przypadku przyczyną błędu może być to, że żaden z identyfikatorów filmów nie został przesłany do strony lub identyfikator filmu jest nieprawidłowy.</span><span class="sxs-lookup"><span data-stu-id="fba13-158">In this case, the error might be that no movie ID was passed to the page or that the movie ID is invalid.</span></span> <span data-ttu-id="fba13-159">Taka sytuacja może wystąpić, jeśli ktoś uruchomił Tę stronę bez uprzedniego wybrania filmu na stronie *filmów* .</span><span class="sxs-lookup"><span data-stu-id="fba13-159">This situation could occur if someone ran this page without first selecting a movie in the *Movies* page.</span></span>

<span data-ttu-id="fba13-160">Podpis przycisku to **Usuń film**, a jego atrybut name jest ustawiony na `buttonDelete`.</span><span class="sxs-lookup"><span data-stu-id="fba13-160">The button caption is **Delete Movie**, and its name attribute is set to `buttonDelete`.</span></span> <span data-ttu-id="fba13-161">Atrybut `name` zostanie użyty w kodzie do zidentyfikowania przycisku, który przesłał formularz.</span><span class="sxs-lookup"><span data-stu-id="fba13-161">The `name` attribute will be used in the code to identify the button that submitted the form.</span></span>

<span data-ttu-id="fba13-162">Musisz napisać kod do 1) odczytać szczegóły filmu, gdy strona jest wyświetlana po raz pierwszy i 2) w rzeczywistości usunie film, gdy użytkownik kliknie przycisk.</span><span class="sxs-lookup"><span data-stu-id="fba13-162">You'll have to write code to 1) read the movie details when the page is first displayed and 2) actually delete the movie when the user clicks the button.</span></span>

## <a name="adding-code-to-read-a-single-movie"></a><span data-ttu-id="fba13-163">Dodawanie kodu w celu odczytania pojedynczego filmu</span><span class="sxs-lookup"><span data-stu-id="fba13-163">Adding Code to Read a Single Movie</span></span>

<span data-ttu-id="fba13-164">W górnej części strony *DeleteMovie. cshtml* Dodaj następujący blok kodu:</span><span class="sxs-lookup"><span data-stu-id="fba13-164">At the top of the *DeleteMovie.cshtml* page, add the following code block:</span></span>

[!code-cshtml[Main](deleting-data/samples/sample5.cshtml)]

<span data-ttu-id="fba13-165">Ten znacznik jest taki sam jak odpowiedni kod na stronie *EditMovie* .</span><span class="sxs-lookup"><span data-stu-id="fba13-165">This markup is the same as the corresponding code in the *EditMovie* page.</span></span> <span data-ttu-id="fba13-166">Pobiera identyfikator filmu z ciągu zapytania i używa identyfikatora do odczytywania rekordu z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="fba13-166">It gets the movie ID out of the query string and uses the ID to read a record from the database.</span></span> <span data-ttu-id="fba13-167">Kod zawiera test weryfikacyjny (`IsInt()` i `row != null`), aby upewnić się, że identyfikator filmu jest prawidłowy.</span><span class="sxs-lookup"><span data-stu-id="fba13-167">The code includes the validation test (`IsInt()` and `row != null`) to make sure that the movie ID being passed to the page is valid.</span></span>

<span data-ttu-id="fba13-168">Należy pamiętać, że ten kod powinien zostać uruchomiony tylko podczas pierwszego uruchomienia strony.</span><span class="sxs-lookup"><span data-stu-id="fba13-168">Remember that this code should only run the first time the page runs.</span></span> <span data-ttu-id="fba13-169">Nie ma potrzeby ponownego odczytywania rekordu filmu z bazy danych, gdy użytkownik kliknie przycisk **Usuń film** .</span><span class="sxs-lookup"><span data-stu-id="fba13-169">You don't want to re-read the movie record from the database when the user clicks the **Delete Movie** button.</span></span> <span data-ttu-id="fba13-170">W związku z tym kod odczytywania filmu znajduje się w teście, który brzmi `if(!IsPost)` &mdash; to, *Jeśli żądanie nie jest operacją post (Przesyłanie formularza)* .</span><span class="sxs-lookup"><span data-stu-id="fba13-170">Therefore, code to read the movie is inside a test that says `if(!IsPost)` &mdash; that is, *if the request is not a post operation (form submission)*.</span></span>

## <a name="adding-code-to-delete-the-selected-movie"></a><span data-ttu-id="fba13-171">Dodawanie kodu w celu usunięcia wybranego filmu</span><span class="sxs-lookup"><span data-stu-id="fba13-171">Adding Code to Delete the Selected Movie</span></span>

<span data-ttu-id="fba13-172">Aby usunąć film, gdy użytkownik kliknie przycisk, Dodaj następujący kod tuż wewnątrz zamykającego nawiasu klamrowego bloku `@`:</span><span class="sxs-lookup"><span data-stu-id="fba13-172">To delete the movie when the user clicks the button, add the following code just inside the closing brace of the `@` block:</span></span>

[!code-csharp[Main](deleting-data/samples/sample6.cs)]

<span data-ttu-id="fba13-173">Ten kod jest podobny do kodu do aktualizowania istniejącego rekordu, ale łatwiejszy.</span><span class="sxs-lookup"><span data-stu-id="fba13-173">This code is similar to the code for updating an existing record, but simpler.</span></span> <span data-ttu-id="fba13-174">Kod zasadniczo uruchamia instrukcję SQL `Delete`.</span><span class="sxs-lookup"><span data-stu-id="fba13-174">The code basically runs a SQL `Delete` statement.</span></span>

 <span data-ttu-id="fba13-175">Podobnie jak na stronie *EditMovie* , kod znajduje się w bloku `if(IsPost)`.</span><span class="sxs-lookup"><span data-stu-id="fba13-175">As in the *EditMovie* page, the code is in an `if(IsPost)` block.</span></span> <span data-ttu-id="fba13-176">Tym razem `if()` warunku jest nieco bardziej skomplikowany:</span><span class="sxs-lookup"><span data-stu-id="fba13-176">This time, the `if()` condition is a little more complicated:</span></span> 

[!code-csharp[Main](deleting-data/samples/sample7.cs)]

<span data-ttu-id="fba13-177">Poniżej przedstawiono dwa warunki.</span><span class="sxs-lookup"><span data-stu-id="fba13-177">There are two conditions here.</span></span> <span data-ttu-id="fba13-178">Pierwszy polega na tym, że strona jest przesyłana, jak widać przed &mdash; `if(IsPost)`.</span><span class="sxs-lookup"><span data-stu-id="fba13-178">The first is that the page is being submitted, as you've seen before &mdash; `if(IsPost)`.</span></span>

<span data-ttu-id="fba13-179">Drugi warunek jest `!Request["buttonDelete"].IsEmpty()`, co oznacza, że żądanie ma obiekt o nazwie `buttonDelete`.</span><span class="sxs-lookup"><span data-stu-id="fba13-179">The second condition is `!Request["buttonDelete"].IsEmpty()`, meaning that the request has an object named `buttonDelete`.</span></span> <span data-ttu-id="fba13-180">Jest to pośredni sposób testowania, który przycisk przesłał formularz.</span><span class="sxs-lookup"><span data-stu-id="fba13-180">Admittedly, it's an indirect way of testing which button submitted the form.</span></span> <span data-ttu-id="fba13-181">Jeśli formularz zawiera wiele przycisków przesyłania, w żądaniu zostanie wyświetlona tylko nazwa klikniętego przycisku.</span><span class="sxs-lookup"><span data-stu-id="fba13-181">If a form contains multiple submit buttons, only the name of the button that was clicked appears in the request.</span></span> <span data-ttu-id="fba13-182">W związku z tym, logicznie, jeśli nazwa określonego przycisku pojawia się w żądaniu &mdash; lub zgodnie z opisem w kodzie, jeśli ten przycisk nie jest pusty &mdash; jest to przycisk, który przesłał formularz.</span><span class="sxs-lookup"><span data-stu-id="fba13-182">Therefore, logically, if the name of a particular button appears in the request &mdash; or as stated in the code, if that button isn't empty &mdash; that's the button that submitted the form.</span></span>

<span data-ttu-id="fba13-183">Operator `&&` oznacza "i" (logiczny i).</span><span class="sxs-lookup"><span data-stu-id="fba13-183">The `&&` operator means "and" (logical AND).</span></span> <span data-ttu-id="fba13-184">W związku z tym cały `if` warunek to...</span><span class="sxs-lookup"><span data-stu-id="fba13-184">Therefore the entire `if` condition is ...</span></span>

<span data-ttu-id="fba13-185">*To żądanie jest ogłoszeniem (nie jest żądaniem pierwszego uruchomienia)*</span><span class="sxs-lookup"><span data-stu-id="fba13-185">*This request is a post (not a first-time request)*</span></span>  
  
 <span data-ttu-id="fba13-186">AND</span><span class="sxs-lookup"><span data-stu-id="fba13-186">AND</span></span>  
  
<span data-ttu-id="fba13-187">Przycisk `buttonDelete`*był przyciskiem, który przesłał formularz.*</span><span class="sxs-lookup"><span data-stu-id="fba13-187">*The* `buttonDelete`*button was the button that submitted the form.*</span></span>

<span data-ttu-id="fba13-188">Ten formularz (w rzeczywistości ta strona) zawiera tylko jeden przycisk, dlatego dodatkowy test dla `buttonDelete` nie jest technicznie wymagany.</span><span class="sxs-lookup"><span data-stu-id="fba13-188">This form (in fact, this page) contains only one button, so the additional test for `buttonDelete` is technically not required.</span></span> <span data-ttu-id="fba13-189">Nadal można wykonać operację, która spowoduje trwałe usunięcie danych.</span><span class="sxs-lookup"><span data-stu-id="fba13-189">Still, you're about to perform an operation that will permanently remove data.</span></span> <span data-ttu-id="fba13-190">Tak więc należy się upewnić, że wykonywanie operacji jest możliwe tylko wtedy, gdy użytkownik jawnie zażądał tego.</span><span class="sxs-lookup"><span data-stu-id="fba13-190">So you want to be as sure as possible that you're performing the operation only when the user has explicitly requested it.</span></span> <span data-ttu-id="fba13-191">Załóżmy na przykład, że ta strona została rozszerzona później i dodano do niej inne przyciski.</span><span class="sxs-lookup"><span data-stu-id="fba13-191">For example, suppose that you expanded this page later and added other buttons to it.</span></span> <span data-ttu-id="fba13-192">Nawet kod usuwający film zostanie uruchomiony tylko wtedy, gdy kliknięto przycisk `buttonDelete`.</span><span class="sxs-lookup"><span data-stu-id="fba13-192">Even then, the code that deletes the movie will run only if the `buttonDelete` button was clicked.</span></span>

<span data-ttu-id="fba13-193">Podobnie jak na stronie *EditMovie* , otrzymujesz identyfikator z ukrytego pola, a następnie uruchamiasz polecenie SQL.</span><span class="sxs-lookup"><span data-stu-id="fba13-193">As in the *EditMovie* page, you get the ID from the hidden field and then run the SQL command.</span></span> <span data-ttu-id="fba13-194">Składnia instrukcji `Delete` jest następująca:</span><span class="sxs-lookup"><span data-stu-id="fba13-194">The syntax for the `Delete` statement is:</span></span>

`DELETE FROM table WHERE ID = value`

<span data-ttu-id="fba13-195">Należy uwzględnić klauzulę `WHERE` i identyfikator.</span><span class="sxs-lookup"><span data-stu-id="fba13-195">It's vital to include the `WHERE` clause and the ID.</span></span> <span data-ttu-id="fba13-196">Jeśli opuścisz klauzulę WHERE, *wszystkie rekordy w tabeli zostaną usunięte*.</span><span class="sxs-lookup"><span data-stu-id="fba13-196">If you leave out the WHERE clause, *all the records in the table will be deleted*.</span></span> <span data-ttu-id="fba13-197">Jak widać, przekaż wartość identyfikatora do polecenia SQL przy użyciu symbolu zastępczego.</span><span class="sxs-lookup"><span data-stu-id="fba13-197">As you have seen, you pass the ID value to the SQL command by using a placeholder.</span></span>

## <a name="testing-the-movie-delete-process"></a><span data-ttu-id="fba13-198">Testowanie procesu usuwania filmu</span><span class="sxs-lookup"><span data-stu-id="fba13-198">Testing the Movie Delete Process</span></span>

<span data-ttu-id="fba13-199">Teraz można testować.</span><span class="sxs-lookup"><span data-stu-id="fba13-199">Now you can test.</span></span> <span data-ttu-id="fba13-200">Uruchom stronę *filmy* , a następnie kliknij pozycję **Usuń** obok filmu.</span><span class="sxs-lookup"><span data-stu-id="fba13-200">Run the *Movies* page, and click **Delete** next to a movie.</span></span> <span data-ttu-id="fba13-201">Gdy zostanie wyświetlona strona *DeleteMovie* , kliknij przycisk **Usuń film**.</span><span class="sxs-lookup"><span data-stu-id="fba13-201">When the *DeleteMovie* page appears, click **Delete Movie**.</span></span>

![Usuń stronę filmu z wyróżnionym przyciskiem Usuń film](deleting-data/_static/image4.png)

<span data-ttu-id="fba13-203">Po kliknięciu przycisku kod usuwa filmy i wraca do listy filmów.</span><span class="sxs-lookup"><span data-stu-id="fba13-203">When you click the button, the code deletes the movies and returns to the movie listing.</span></span> <span data-ttu-id="fba13-204">Można wyszukać usunięty film i potwierdzić, że został on usunięty.</span><span class="sxs-lookup"><span data-stu-id="fba13-204">There you can search for the deleted movie and confirm that it's been deleted.</span></span>

## <a name="coming-up-next"></a><span data-ttu-id="fba13-205">Przyszłe przejście</span><span class="sxs-lookup"><span data-stu-id="fba13-205">Coming Up Next</span></span>

<span data-ttu-id="fba13-206">W następnym samouczku przedstawiono sposób nadawania wszystkim stronom w witrynie typowego wyglądu i układu.</span><span class="sxs-lookup"><span data-stu-id="fba13-206">The next tutorial shows you how to give all the pages on your site a common look and layout.</span></span>

## <a name="complete-listing-for-movie-page-updated-with-delete-links"></a><span data-ttu-id="fba13-207">Ukończ listę na stronie filmu (Zaktualizowano za pomocą linków usuwania)</span><span class="sxs-lookup"><span data-stu-id="fba13-207">Complete Listing for Movie Page (Updated with Delete Links)</span></span>

[!code-cshtml[Main](deleting-data/samples/sample8.cshtml)]

## <a name="complete-listing-for-deletemovie-page"></a><span data-ttu-id="fba13-208">Ukończ listę dla strony DeleteMovie</span><span class="sxs-lookup"><span data-stu-id="fba13-208">Complete Listing for DeleteMovie Page</span></span>

[!code-cshtml[Main](deleting-data/samples/sample9.cshtml)]

## <a name="additional-resources"></a><span data-ttu-id="fba13-209">Dodatkowe materiały</span><span class="sxs-lookup"><span data-stu-id="fba13-209">Additional Resources</span></span>

- [<span data-ttu-id="fba13-210">Wprowadzenie do programowania w ASP.NET sieci Web przy użyciu składni Razor</span><span class="sxs-lookup"><span data-stu-id="fba13-210">Introduction to ASP.NET Web Programming by Using the Razor Syntax</span></span>](../introducing-razor-syntax-c.md)
- <span data-ttu-id="fba13-211">[Instrukcja DELETE SQL](http://www.w3schools.com/sql/sql_delete.asp) w witrynie w3schools</span><span class="sxs-lookup"><span data-stu-id="fba13-211">[SQL DELETE Statement](http://www.w3schools.com/sql/sql_delete.asp) on the W3Schools site</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="fba13-212">[Poprzednie](updating-data.md)
> [dalej](layouts.md)</span><span class="sxs-lookup"><span data-stu-id="fba13-212">[Previous](updating-data.md)
[Next](layouts.md)</span></span>

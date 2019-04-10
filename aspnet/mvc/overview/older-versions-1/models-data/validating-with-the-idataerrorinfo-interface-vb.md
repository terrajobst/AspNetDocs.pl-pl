---
uid: mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-vb
title: Weryfikowanie z użyciem interfejsu IDataErrorInfo (VB) | Dokumentacja firmy Microsoft
author: StephenWalther
description: 'Autor: Stephen Walther dowiesz się, jak wyświetlać komunikaty o błędach niestandardowego sprawdzania poprawności poprzez implementację interfejsu IDataErrorInfo w klasie modelu.'
ms.author: riande
ms.date: 03/02/2009
ms.assetid: 3a8a9d9f-82dd-4959-b7c6-960e9ce95df1
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: c64e1ea1562c3a0cfe4fb33f1c3033bb9c31bd2c
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/09/2019
ms.locfileid: "59402742"
---
# <a name="validating-with-the-idataerrorinfo-interface-vb"></a><span data-ttu-id="b94b3-103">Walidacja z użyciem interfejsu IDataErrorInfo (VB)</span><span class="sxs-lookup"><span data-stu-id="b94b3-103">Validating with the IDataErrorInfo Interface (VB)</span></span>

<span data-ttu-id="b94b3-104">przez [Walther Autor: Stephen](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="b94b3-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="b94b3-105">Autor: Stephen Walther dowiesz się, jak wyświetlać komunikaty o błędach niestandardowego sprawdzania poprawności poprzez implementację interfejsu IDataErrorInfo w klasie modelu.</span><span class="sxs-lookup"><span data-stu-id="b94b3-105">Stephen Walther shows you how to display custom validation error messages by implementing the IDataErrorInfo interface in a model class.</span></span>


<span data-ttu-id="b94b3-106">Celem tego samouczka jest wyjaśnić jedno z podejść do przeprowadzania weryfikacji w aplikacji ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="b94b3-106">The goal of this tutorial is to explain one approach to performing validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="b94b3-107">Dowiesz się, jak zapobiec ktoś przesyłania formularza HTML bez podawania wartości wymaganych pól formularza.</span><span class="sxs-lookup"><span data-stu-id="b94b3-107">You learn how to prevent someone from submitting an HTML form without providing values for required form fields.</span></span> <span data-ttu-id="b94b3-108">W tym samouczku dowiesz się, jak przeprowadzić weryfikacji za pomocą interfejsu IErrorDataInfo.</span><span class="sxs-lookup"><span data-stu-id="b94b3-108">In this tutorial, you learn how to perform validation by using the IErrorDataInfo interface.</span></span>

## <a name="assumptions"></a><span data-ttu-id="b94b3-109">Założenia</span><span class="sxs-lookup"><span data-stu-id="b94b3-109">Assumptions</span></span>

<span data-ttu-id="b94b3-110">W tym samouczku czy mogę użyć MoviesDB bazy danych i tabeli bazy danych filmów.</span><span class="sxs-lookup"><span data-stu-id="b94b3-110">In this tutorial, I'll use the MoviesDB database and the Movies database table.</span></span> <span data-ttu-id="b94b3-111">Ta tabela zawiera następujące kolumny:</span><span class="sxs-lookup"><span data-stu-id="b94b3-111">This table has the following columns:</span></span>

<a id="0.6_table01"></a>


| **<span data-ttu-id="b94b3-112">Nazwa kolumny</span><span class="sxs-lookup"><span data-stu-id="b94b3-112">Column Name</span></span>** | **<span data-ttu-id="b94b3-113">Typ danych</span><span class="sxs-lookup"><span data-stu-id="b94b3-113">Data Type</span></span>** | **<span data-ttu-id="b94b3-114">Zezwalaj na wartości null</span><span class="sxs-lookup"><span data-stu-id="b94b3-114">Allow Nulls</span></span>** |
| --- | --- | --- |
| <span data-ttu-id="b94b3-115">Id</span><span class="sxs-lookup"><span data-stu-id="b94b3-115">Id</span></span> | <span data-ttu-id="b94b3-116">int</span><span class="sxs-lookup"><span data-stu-id="b94b3-116">Int</span></span> | <span data-ttu-id="b94b3-117">False</span><span class="sxs-lookup"><span data-stu-id="b94b3-117">False</span></span> |
| <span data-ttu-id="b94b3-118">Tytuł</span><span class="sxs-lookup"><span data-stu-id="b94b3-118">Title</span></span> | <span data-ttu-id="b94b3-119">Nvarchar(100)</span><span class="sxs-lookup"><span data-stu-id="b94b3-119">Nvarchar(100)</span></span> | <span data-ttu-id="b94b3-120">False</span><span class="sxs-lookup"><span data-stu-id="b94b3-120">False</span></span> |
| <span data-ttu-id="b94b3-121">Dyrektor ds.</span><span class="sxs-lookup"><span data-stu-id="b94b3-121">Director</span></span> | <span data-ttu-id="b94b3-122">Nvarchar(100)</span><span class="sxs-lookup"><span data-stu-id="b94b3-122">Nvarchar(100)</span></span> | <span data-ttu-id="b94b3-123">False</span><span class="sxs-lookup"><span data-stu-id="b94b3-123">False</span></span> |
| <span data-ttu-id="b94b3-124">DateReleased</span><span class="sxs-lookup"><span data-stu-id="b94b3-124">DateReleased</span></span> | <span data-ttu-id="b94b3-125">DataGodzina</span><span class="sxs-lookup"><span data-stu-id="b94b3-125">DateTime</span></span> | <span data-ttu-id="b94b3-126">False</span><span class="sxs-lookup"><span data-stu-id="b94b3-126">False</span></span> |


<span data-ttu-id="b94b3-127">W tym samouczku używam Microsoft Entity Framework do generowania klasy modelu mojej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="b94b3-127">In this tutorial, I use the Microsoft Entity Framework to generate my database model classes.</span></span> <span data-ttu-id="b94b3-128">Klasa filmu generowane przez program Entity Framework jest wyświetlany na rysunku 1.</span><span class="sxs-lookup"><span data-stu-id="b94b3-128">The Movie class generated by the Entity Framework is displayed in Figure 1.</span></span>


[![T<span data-ttu-id="b94b3-129">ADAM jednostki filmu]</span><span class="sxs-lookup"><span data-stu-id="b94b3-129">he Movie entity]</span></span>(validating-with-the-idataerrorinfo-interface-vb/_static/image1.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image1.png)

<span data-ttu-id="b94b3-130">**Rysunek 01**: Jednostki Movie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](validating-with-the-idataerrorinfo-interface-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="b94b3-130">**Figure 01**: The Movie entity([Click to view full-size image](validating-with-the-idataerrorinfo-interface-vb/_static/image2.png))</span></span>


> [!NOTE] 
> 
> <span data-ttu-id="b94b3-131">Aby dowiedzieć się więcej o korzystaniu z programu Entity Framework do generowania klasy modelu z bazy danych, zobacz temat zatytułowany Moje samouczek: Tworzenie klas modelu za pomocą programu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="b94b3-131">To learn more about using the Entity Framework to generate your database model classes, see my tutorial entitled Creating Model Classes with the Entity Framework.</span></span>


## <a name="the-controller-class"></a><span data-ttu-id="b94b3-132">Klasa kontrolera</span><span class="sxs-lookup"><span data-stu-id="b94b3-132">The Controller Class</span></span>

<span data-ttu-id="b94b3-133">Używane kontrolera głównego do filmów z listy i tworzenia nowych filmów.</span><span class="sxs-lookup"><span data-stu-id="b94b3-133">We use the Home controller to list movies and create new movies.</span></span> <span data-ttu-id="b94b3-134">Kod dla tej klasy są zawarte w ofercie 1.</span><span class="sxs-lookup"><span data-stu-id="b94b3-134">The code for this class is contained in Listing 1.</span></span>

**<span data-ttu-id="b94b3-135">Wyświetlanie listy 1 - Controllers\HomeController.vb</span><span class="sxs-lookup"><span data-stu-id="b94b3-135">Listing 1 - Controllers\HomeController.vb</span></span>**

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample1.vb)]

<span data-ttu-id="b94b3-136">Klasa kontrolera głównej w ofercie 1 zawiera dwie akcje Create().</span><span class="sxs-lookup"><span data-stu-id="b94b3-136">The Home controller class in Listing 1 contains two Create() actions.</span></span> <span data-ttu-id="b94b3-137">Pierwszą akcją Wyświetla formularza HTML do tworzenia nowych filmów.</span><span class="sxs-lookup"><span data-stu-id="b94b3-137">The first action displays the HTML form for creating a new movie.</span></span> <span data-ttu-id="b94b3-138">Drugiej akcji Create() przeprowadza rzeczywiste Wstaw nowy film do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="b94b3-138">The second Create() action performs the actual insert of the new movie into the database.</span></span> <span data-ttu-id="b94b3-139">Drugiej akcji Create() jest wywoływana po przesłaniu formularza wyświetlany przez pierwszą akcją Create() do serwera.</span><span class="sxs-lookup"><span data-stu-id="b94b3-139">The second Create() action is invoked when the form displayed by the first Create() action is submitted to the server.</span></span>

<span data-ttu-id="b94b3-140">Zwróć uwagę, że druga Akcja Create() zawiera następujące wiersze kodu:</span><span class="sxs-lookup"><span data-stu-id="b94b3-140">Notice that the second Create() action contains the following lines of code:</span></span>

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample2.vb)]

<span data-ttu-id="b94b3-141">Właściwość IsValid zwraca wartość false, gdy występuje błąd weryfikacji.</span><span class="sxs-lookup"><span data-stu-id="b94b3-141">The IsValid property returns false when there is a validation error.</span></span> <span data-ttu-id="b94b3-142">W takim przypadku widok Utwórz, który zawiera formularza HTML do tworzenia film zostanie wyświetlony ponownie.</span><span class="sxs-lookup"><span data-stu-id="b94b3-142">In that case, the Create view that contains the HTML form for creating a movie is redisplayed.</span></span>

## <a name="creating-a-partial-class"></a><span data-ttu-id="b94b3-143">Tworzenie klasy częściowe</span><span class="sxs-lookup"><span data-stu-id="b94b3-143">Creating a Partial Class</span></span>

<span data-ttu-id="b94b3-144">Klasa film jest generowany przez program Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="b94b3-144">The Movie class is generated by the Entity Framework.</span></span> <span data-ttu-id="b94b3-145">Widać kod klasy filmu, jeśli Rozwiń plik MoviesDBModel.edmx w oknie Eksploratora rozwiązań i Otwórz plik MoviesDBModel.Designer.vb w edytorze kodu (patrz rysunek 2).</span><span class="sxs-lookup"><span data-stu-id="b94b3-145">You can see the code for the Movie class if you expand the MoviesDBModel.edmx file in the Solution Explorer window and open the MoviesDBModel.Designer.vb file in the Code Editor (see Figure 2).</span></span>


[![T<span data-ttu-id="b94b3-146">Kod HE jednostki filmu]</span><span class="sxs-lookup"><span data-stu-id="b94b3-146">he code for the Movie entity]</span></span>(validating-with-the-idataerrorinfo-interface-vb/_static/image2.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image3.png)

<span data-ttu-id="b94b3-147">**Rysunek 02**: Kod dla jednostki Movie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](validating-with-the-idataerrorinfo-interface-vb/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="b94b3-147">**Figure 02**: The code for the Movie entity([Click to view full-size image](validating-with-the-idataerrorinfo-interface-vb/_static/image4.png))</span></span>


<span data-ttu-id="b94b3-148">Klasa film jest klasy częściowej.</span><span class="sxs-lookup"><span data-stu-id="b94b3-148">The Movie class is a partial class.</span></span> <span data-ttu-id="b94b3-149">Oznacza to, że możemy dodać innej klasy częściowej o takiej samej nazwie, aby rozszerzyć funkcjonalność klasy filmu.</span><span class="sxs-lookup"><span data-stu-id="b94b3-149">That means that we can add another partial class with the same name to extend the functionality of the Movie class.</span></span> <span data-ttu-id="b94b3-150">Dodamy naszych logikę walidacji do nowej klasy częściowej.</span><span class="sxs-lookup"><span data-stu-id="b94b3-150">We'll add our validation logic to the new partial class.</span></span>

<span data-ttu-id="b94b3-151">Dodaj klasę w ofercie 2 do folderu modeli.</span><span class="sxs-lookup"><span data-stu-id="b94b3-151">Add the class in Listing 2 to the Models folder.</span></span>

**<span data-ttu-id="b94b3-152">Wyświetlanie listy 2 - Models\Movie.vb</span><span class="sxs-lookup"><span data-stu-id="b94b3-152">Listing 2 - Models\Movie.vb</span></span>**

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample3.vb)]

<span data-ttu-id="b94b3-153">Należy zauważyć, że klasa w ofercie 2 obejmuje *częściowe* modyfikator.</span><span class="sxs-lookup"><span data-stu-id="b94b3-153">Notice that the class in Listing 2 includes the *partial* modifier.</span></span> <span data-ttu-id="b94b3-154">Wszelkie metody lub właściwości, które możesz dodać do tej klasy stają się częścią klasy filmu generowane przez program Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="b94b3-154">Any methods or properties that you add to this class become part of the Movie class generated by the Entity Framework.</span></span>

## <a name="adding-onchanging-and-onchanged-partial-methods"></a><span data-ttu-id="b94b3-155">Dodawanie OnChanging i metod OnChanged częściowego</span><span class="sxs-lookup"><span data-stu-id="b94b3-155">Adding OnChanging and OnChanged Partial Methods</span></span>

<span data-ttu-id="b94b3-156">Gdy platforma Entity Framework generuje klasę jednostki, platformy Entity Framework metod częściowych do klasy automatycznie dodaje.</span><span class="sxs-lookup"><span data-stu-id="b94b3-156">When the Entity Framework generates an entity class, the Entity Framework adds partial methods to the class automatically.</span></span> <span data-ttu-id="b94b3-157">Entity Framework generuje OnChanging i OnChanged metod częściowych, które odpowiadają każdej właściwości klasy.</span><span class="sxs-lookup"><span data-stu-id="b94b3-157">The Entity Framework generates OnChanging and OnChanged partial methods that correspond to each property of the class.</span></span>

<span data-ttu-id="b94b3-158">W przypadku klasy filmu programu Entity Framework tworzy następujące metody:</span><span class="sxs-lookup"><span data-stu-id="b94b3-158">In the case of the Movie class, the Entity Framework creates the following methods:</span></span>

- <span data-ttu-id="b94b3-159">OnIdChanging</span><span class="sxs-lookup"><span data-stu-id="b94b3-159">OnIdChanging</span></span>
- <span data-ttu-id="b94b3-160">OnIdChanged</span><span class="sxs-lookup"><span data-stu-id="b94b3-160">OnIdChanged</span></span>
- <span data-ttu-id="b94b3-161">OnTitleChanging</span><span class="sxs-lookup"><span data-stu-id="b94b3-161">OnTitleChanging</span></span>
- <span data-ttu-id="b94b3-162">OnTitleChanged</span><span class="sxs-lookup"><span data-stu-id="b94b3-162">OnTitleChanged</span></span>
- <span data-ttu-id="b94b3-163">OnDirectorChanging</span><span class="sxs-lookup"><span data-stu-id="b94b3-163">OnDirectorChanging</span></span>
- <span data-ttu-id="b94b3-164">OnDirectorChanged</span><span class="sxs-lookup"><span data-stu-id="b94b3-164">OnDirectorChanged</span></span>
- <span data-ttu-id="b94b3-165">OnDateReleasedChanging</span><span class="sxs-lookup"><span data-stu-id="b94b3-165">OnDateReleasedChanging</span></span>
- <span data-ttu-id="b94b3-166">OnDateReleasedChanged</span><span class="sxs-lookup"><span data-stu-id="b94b3-166">OnDateReleasedChanged</span></span>

<span data-ttu-id="b94b3-167">Metoda OnChanging jest wywoływana prawo, przed zmianą odpowiadającą właściwość.</span><span class="sxs-lookup"><span data-stu-id="b94b3-167">The OnChanging method is called right before the corresponding property is changed.</span></span> <span data-ttu-id="b94b3-168">Metoda OnChanged jest wywoływana prawej, po zmianie właściwości.</span><span class="sxs-lookup"><span data-stu-id="b94b3-168">The OnChanged method is called right after the property is changed.</span></span>

<span data-ttu-id="b94b3-169">Możesz korzystać z zalet tych metod częściowych, aby dodać logikę walidacji do klasy filmu.</span><span class="sxs-lookup"><span data-stu-id="b94b3-169">You can take advantage of these partial methods to add validation logic to the Movie class.</span></span> <span data-ttu-id="b94b3-170">Aktualizacja klasy filmu w ofercie 3 sprawdza, czy tytuł i dyrektor ds. właściwości są przypisywane niepustych wartości.</span><span class="sxs-lookup"><span data-stu-id="b94b3-170">The update Movie class in Listing 3 verifies that the Title and Director properties are assigned nonempty values.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="b94b3-171">Metoda częściowa jest metoda zdefiniowana w klasie, które nie są wymagane do wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="b94b3-171">A partial method is a method defined in a class that you are not required to implement.</span></span> <span data-ttu-id="b94b3-172">Jeśli nie implementować metodę częściową kompilator usuwa podpis metody i wszystkie wywołania do metody, więc są bez kosztów czasu wykonywania skojarzony z metody częściowej.</span><span class="sxs-lookup"><span data-stu-id="b94b3-172">If you don't implement a partial method then the compiler removes the method signature and all calls to the method so there are no run-time costs associated with the partial method.</span></span> <span data-ttu-id="b94b3-173">W edytorze programu Visual Studio Code można dodać metody częściowej, wpisując słowa kluczowego *częściowe* następuje spacja, aby wyświetlić listę częściowych do zaimplementowania.</span><span class="sxs-lookup"><span data-stu-id="b94b3-173">In the Visual Studio Code Editor, you can add a partial method by typing the keyword *partial* followed by a space to view a list of partials to implement.</span></span>


**<span data-ttu-id="b94b3-174">Wyświetlanie listy 3 - Models\Movie.vb</span><span class="sxs-lookup"><span data-stu-id="b94b3-174">Listing 3 - Models\Movie.vb</span></span>**

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample4.vb)]

<span data-ttu-id="b94b3-175">Na przykład, jeśli użytkownik podejmie próbę przypisania pustego ciągu do właściwości tytułu, komunikat o błędzie jest przypisywany do słownika o nazwie \_błędy.</span><span class="sxs-lookup"><span data-stu-id="b94b3-175">For example, if you attempt to assign an empty string to the Title property, then an error message is assigned to a Dictionary named \_errors.</span></span>

<span data-ttu-id="b94b3-176">W tym momencie faktycznie nie powoduje żadnych zmian przypisania pustego ciągu do właściwości tytułu, a błąd jest dodawany do prywatnego \_pola błędy.</span><span class="sxs-lookup"><span data-stu-id="b94b3-176">At this point, nothing actually happens when you assign an empty string to the Title property and an error is added to the private \_errors field.</span></span> <span data-ttu-id="b94b3-177">Musimy zaimplementować interfejsu IDataErrorInfo, aby udostępnić te błędy sprawdzania poprawności, platforma ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="b94b3-177">We need to implement the IDataErrorInfo interface to expose these validation errors to the ASP.NET MVC framework.</span></span>

## <a name="implementing-the-idataerrorinfo-interface"></a><span data-ttu-id="b94b3-178">Implementowanie interfejsu IDataErrorInfo</span><span class="sxs-lookup"><span data-stu-id="b94b3-178">Implementing the IDataErrorInfo Interface</span></span>

<span data-ttu-id="b94b3-179">Interfejsu IDataErrorInfo było częścią programu .NET framework od pierwszej wersji.</span><span class="sxs-lookup"><span data-stu-id="b94b3-179">The IDataErrorInfo interface has been part of the .NET framework since the first version.</span></span> <span data-ttu-id="b94b3-180">Ten interfejs jest bardzo prosty interfejs:</span><span class="sxs-lookup"><span data-stu-id="b94b3-180">This interface is a very simple interface:</span></span>

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample5.vb)]

<span data-ttu-id="b94b3-181">Jeśli klasa implementuje interfejsu IDataErrorInfo, platforma ASP.NET MVC użyje tego interfejsu, podczas tworzenia wystąpienia klasy.</span><span class="sxs-lookup"><span data-stu-id="b94b3-181">If a class implements the IDataErrorInfo interface, the ASP.NET MVC framework will use this interface when creating an instance of the class.</span></span> <span data-ttu-id="b94b3-182">Na przykład kontrolera głównego działania Create() akceptuje wystąpienia klasy film:</span><span class="sxs-lookup"><span data-stu-id="b94b3-182">For example, the Home controller Create() action accepts an instance of the Movie class:</span></span>

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample6.vb)]

<span data-ttu-id="b94b3-183">Platforma ASP.NET MVC tworzy wystąpienie filmu przekazywane do działania Create() za pomocą integratora modelu (DefaultModelBinder).</span><span class="sxs-lookup"><span data-stu-id="b94b3-183">The ASP.NET MVC framework creates the instance of the Movie passed to the Create() action by using a model binder (the DefaultModelBinder).</span></span> <span data-ttu-id="b94b3-184">Integrator modelu jest odpowiedzialny za utworzenie wystąpienia obiektu filmu przez powiązanie pola formularza HTML do wystąpienia obiektu filmu.</span><span class="sxs-lookup"><span data-stu-id="b94b3-184">The model binder is responsible for creating an instance of the Movie object by binding the HTML form fields to an instance of the Movie object.</span></span>

<span data-ttu-id="b94b3-185">DefaultModelBinder wykrywa, czy klasa implementuje interfejsu IDataErrorInfo.</span><span class="sxs-lookup"><span data-stu-id="b94b3-185">The DefaultModelBinder detects whether or not a class implements the IDataErrorInfo interface.</span></span> <span data-ttu-id="b94b3-186">Jeśli klasa implementuje ten interfejs integratora modelu wywołuje indeksatora IDataErrorInfo.this dla każdej właściwości klasy.</span><span class="sxs-lookup"><span data-stu-id="b94b3-186">If a class implements this interface then the model binder invokes the IDataErrorInfo.this indexer for each property of the class.</span></span> <span data-ttu-id="b94b3-187">Jeśli indeksatora zwróci komunikat o błędzie integratora modelu dodaje ten komunikat o błędzie do modelowania stanu automatycznie.</span><span class="sxs-lookup"><span data-stu-id="b94b3-187">If the indexer returns an error message then the model binder adds this error message to model state automatically.</span></span>

<span data-ttu-id="b94b3-188">DefaultModelBinder sprawdza również właściwość IDataErrorInfo.Error.</span><span class="sxs-lookup"><span data-stu-id="b94b3-188">The DefaultModelBinder also checks the IDataErrorInfo.Error property.</span></span> <span data-ttu-id="b94b3-189">Ta właściwość jest przeznaczona do reprezentowania błędy sprawdzania poprawności określonego inną niż właściwość skojarzony z klasą.</span><span class="sxs-lookup"><span data-stu-id="b94b3-189">This property is intended to represent non-property specific validation errors associated with the class.</span></span> <span data-ttu-id="b94b3-190">Na przykład możesz chcieć wymusić regułę poprawności, który zależy od wartości wielu właściwości klasy filmu.</span><span class="sxs-lookup"><span data-stu-id="b94b3-190">For example, you might want to enforce a validation rule that depends on the values of multiple properties of the Movie class.</span></span> <span data-ttu-id="b94b3-191">W takiej sytuacji zwróci błąd sprawdzania poprawności z właściwości błędu.</span><span class="sxs-lookup"><span data-stu-id="b94b3-191">In that case, you would return a validation error from the Error property.</span></span>

<span data-ttu-id="b94b3-192">Zaktualizowano klasy filmu w ofercie 4 implementuje interfejsu IDataErrorInfo.</span><span class="sxs-lookup"><span data-stu-id="b94b3-192">The updated Movie class in Listing 4 implements the IDataErrorInfo interface.</span></span>

**<span data-ttu-id="b94b3-193">Wyświetlanie listy 4 - Models\Movie.vb (implementuje IDataErrorInfo)</span><span class="sxs-lookup"><span data-stu-id="b94b3-193">Listing 4 - Models\Movie.vb (implements IDataErrorInfo)</span></span>**

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample7.vb)]

<span data-ttu-id="b94b3-194">W ofercie 4, sprawdza właściwości indeksatora \_kolekcji błędów, aby zobaczyć, czy zawiera klucz, który odpowiada nazwie właściwości przekazany do indeksatora.</span><span class="sxs-lookup"><span data-stu-id="b94b3-194">In Listing 4, the indexer property checks the \_errors collection to see if it contains a key that corresponds to the property name passed to the indexer.</span></span> <span data-ttu-id="b94b3-195">W przypadku braku błędów weryfikacji skojarzony z właściwością, zwracany jest pusty ciąg.</span><span class="sxs-lookup"><span data-stu-id="b94b3-195">If there is no validation error associated with the property then an empty string is returned.</span></span>

<span data-ttu-id="b94b3-196">Nie potrzebujesz zmodyfikować kontrolera głównego w jakikolwiek sposób, aby użyć zmodyfikowane klasy filmu.</span><span class="sxs-lookup"><span data-stu-id="b94b3-196">You don't need to modify the Home controller in any way to use the modified Movie class.</span></span> <span data-ttu-id="b94b3-197">Strona wyświetlona na rysunku 3 przedstawiono, co się stanie, gdy została wprowadzona żadna wartość dla pola formularza tytuł lub dyrektor ds.</span><span class="sxs-lookup"><span data-stu-id="b94b3-197">The page displayed in Figure 3 illustrates what happens when no value is entered for the Title or Director form fields.</span></span>


[![C<span data-ttu-id="b94b3-198">metody akcji worzenie automatycznie]</span><span class="sxs-lookup"><span data-stu-id="b94b3-198">reating action methods automatically]</span></span>(validating-with-the-idataerrorinfo-interface-vb/_static/image3.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image5.png)

<span data-ttu-id="b94b3-199">**Rysunek 03**: Formularz z brakującymi wartościami ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](validating-with-the-idataerrorinfo-interface-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="b94b3-199">**Figure 03**: A form with missing values ([Click to view full-size image](validating-with-the-idataerrorinfo-interface-vb/_static/image6.png))</span></span>


<span data-ttu-id="b94b3-200">Zwróć uwagę, że wartość DateReleased jest automatycznie wykonane sprawdzanie poprawności.</span><span class="sxs-lookup"><span data-stu-id="b94b3-200">Notice that the DateReleased value is validated automatically.</span></span> <span data-ttu-id="b94b3-201">Ponieważ właściwość DateReleased nie akceptuje wartości NULL, DefaultModelBinder generuje błąd sprawdzania poprawności dla tej właściwości automatycznie, gdy nie ma wartości.</span><span class="sxs-lookup"><span data-stu-id="b94b3-201">Because the DateReleased property does not accept NULL values, the DefaultModelBinder generates a validation error for this property automatically when it does not have a value.</span></span> <span data-ttu-id="b94b3-202">Jeśli chcesz zmodyfikować komunikat o błędzie dla właściwości DateReleased, a następnie należy utworzyć niestandardowego integratora modelu.</span><span class="sxs-lookup"><span data-stu-id="b94b3-202">If you want to modify the error message for the DateReleased property then you need to create a custom model binder.</span></span>

## <a name="summary"></a><span data-ttu-id="b94b3-203">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="b94b3-203">Summary</span></span>

<span data-ttu-id="b94b3-204">W tym samouczku przedstawiono sposób korzystania z użyciem interfejsu IDataErrorInfo do generowania komunikatów o błędach weryfikacji.</span><span class="sxs-lookup"><span data-stu-id="b94b3-204">In this tutorial, you learned how to use the IDataErrorInfo interface to generate validation error messages.</span></span> <span data-ttu-id="b94b3-205">Po pierwsze utworzyliśmy częściową klasą filmów, która rozszerza funkcjonalność klasy częściowej filmu generowane przez program Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="b94b3-205">First, we created a partial Movie class that extends the functionality of the partial Movie class generated by the Entity Framework.</span></span> <span data-ttu-id="b94b3-206">Następnie dodaliśmy logikę walidacji do OnTitleChanging() i OnDirectorChanging() metod częściowych dla klasy filmu.</span><span class="sxs-lookup"><span data-stu-id="b94b3-206">Next, we added validation logic to the Movie class OnTitleChanging() and OnDirectorChanging() partial methods.</span></span> <span data-ttu-id="b94b3-207">Ponadto wprowadziliśmy interfejsu IDataErrorInfo, aby udostępnić te komunikaty weryfikacji platformę ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="b94b3-207">Finally, we implemented the IDataErrorInfo interface in order to expose these validation messages to the ASP.NET MVC framework.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b94b3-208">[Poprzednie](performing-simple-validation-vb.md)
> [dalej](validating-with-a-service-layer-vb.md)</span><span class="sxs-lookup"><span data-stu-id="b94b3-208">[Previous](performing-simple-validation-vb.md)
[Next](validating-with-a-service-layer-vb.md)</span></span>

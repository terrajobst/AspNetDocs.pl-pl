---
uid: mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-cs
title: Weryfikowanie z użyciem interfejsu IDataErrorInfo (C#) | Dokumentacja firmy Microsoft
author: StephenWalther
description: 'Autor: Stephen Walther dowiesz się, jak wyświetlać komunikaty o błędach niestandardowego sprawdzania poprawności poprzez implementację interfejsu IDataErrorInfo w klasie modelu.'
ms.author: riande
ms.date: 03/02/2009
ms.assetid: 4733b9f1-9999-48fb-8b73-6038fbcc5ecb
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: 3e1399d17840a2f5301349cb91deb07b0cc34363
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/09/2019
ms.locfileid: "59421982"
---
# <a name="validating-with-the-idataerrorinfo-interface-c"></a><span data-ttu-id="50305-103">Walidacja z użyciem interfejsu IDataErrorInfo (C#)</span><span class="sxs-lookup"><span data-stu-id="50305-103">Validating with the IDataErrorInfo Interface (C#)</span></span>

<span data-ttu-id="50305-104">przez [Walther Autor: Stephen](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="50305-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="50305-105">Autor: Stephen Walther dowiesz się, jak wyświetlać komunikaty o błędach niestandardowego sprawdzania poprawności poprzez implementację interfejsu IDataErrorInfo w klasie modelu.</span><span class="sxs-lookup"><span data-stu-id="50305-105">Stephen Walther shows you how to display custom validation error messages by implementing the IDataErrorInfo interface in a model class.</span></span>


<span data-ttu-id="50305-106">Celem tego samouczka jest wyjaśnić jedno z podejść do przeprowadzania weryfikacji w aplikacji ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="50305-106">The goal of this tutorial is to explain one approach to performing validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="50305-107">Dowiesz się, jak zapobiec ktoś przesyłania formularza HTML bez podawania wartości wymaganych pól formularza.</span><span class="sxs-lookup"><span data-stu-id="50305-107">You learn how to prevent someone from submitting an HTML form without providing values for required form fields.</span></span> <span data-ttu-id="50305-108">W tym samouczku dowiesz się, jak przeprowadzić weryfikacji za pomocą interfejsu IErrorDataInfo.</span><span class="sxs-lookup"><span data-stu-id="50305-108">In this tutorial, you learn how to perform validation by using the IErrorDataInfo interface.</span></span>

## <a name="assumptions"></a><span data-ttu-id="50305-109">Założenia</span><span class="sxs-lookup"><span data-stu-id="50305-109">Assumptions</span></span>

<span data-ttu-id="50305-110">W tym samouczku czy mogę użyć MoviesDB bazy danych i tabeli bazy danych filmów.</span><span class="sxs-lookup"><span data-stu-id="50305-110">In this tutorial, I'll use the MoviesDB database and the Movies database table.</span></span> <span data-ttu-id="50305-111">Ta tabela zawiera następujące kolumny:</span><span class="sxs-lookup"><span data-stu-id="50305-111">This table has the following columns:</span></span>

<a id="0.5_table01"></a>


| **<span data-ttu-id="50305-112">Nazwa kolumny</span><span class="sxs-lookup"><span data-stu-id="50305-112">Column Name</span></span>** | **<span data-ttu-id="50305-113">Typ danych</span><span class="sxs-lookup"><span data-stu-id="50305-113">Data Type</span></span>** | **<span data-ttu-id="50305-114">Zezwalaj na wartości null</span><span class="sxs-lookup"><span data-stu-id="50305-114">Allow Nulls</span></span>** |
| --- | --- | --- |
| <span data-ttu-id="50305-115">Id</span><span class="sxs-lookup"><span data-stu-id="50305-115">Id</span></span> | <span data-ttu-id="50305-116">int</span><span class="sxs-lookup"><span data-stu-id="50305-116">Int</span></span> | <span data-ttu-id="50305-117">False</span><span class="sxs-lookup"><span data-stu-id="50305-117">False</span></span> |
| <span data-ttu-id="50305-118">Tytuł</span><span class="sxs-lookup"><span data-stu-id="50305-118">Title</span></span> | <span data-ttu-id="50305-119">Nvarchar(100)</span><span class="sxs-lookup"><span data-stu-id="50305-119">Nvarchar(100)</span></span> | <span data-ttu-id="50305-120">False</span><span class="sxs-lookup"><span data-stu-id="50305-120">False</span></span> |
| <span data-ttu-id="50305-121">Dyrektor ds.</span><span class="sxs-lookup"><span data-stu-id="50305-121">Director</span></span> | <span data-ttu-id="50305-122">Nvarchar(100)</span><span class="sxs-lookup"><span data-stu-id="50305-122">Nvarchar(100)</span></span> | <span data-ttu-id="50305-123">False</span><span class="sxs-lookup"><span data-stu-id="50305-123">False</span></span> |
| <span data-ttu-id="50305-124">DateReleased</span><span class="sxs-lookup"><span data-stu-id="50305-124">DateReleased</span></span> | <span data-ttu-id="50305-125">DataGodzina</span><span class="sxs-lookup"><span data-stu-id="50305-125">DateTime</span></span> | <span data-ttu-id="50305-126">False</span><span class="sxs-lookup"><span data-stu-id="50305-126">False</span></span> |


<span data-ttu-id="50305-127">W tym samouczku używam Microsoft Entity Framework do generowania klasy modelu mojej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="50305-127">In this tutorial, I use the Microsoft Entity Framework to generate my database model classes.</span></span> <span data-ttu-id="50305-128">Klasa filmu generowane przez program Entity Framework jest wyświetlany na rysunku 1.</span><span class="sxs-lookup"><span data-stu-id="50305-128">The Movie class generated by the Entity Framework is displayed in Figure 1.</span></span>


[![T<span data-ttu-id="50305-129">ADAM jednostki filmu]</span><span class="sxs-lookup"><span data-stu-id="50305-129">he Movie entity]</span></span>(validating-with-the-idataerrorinfo-interface-cs/_static/image1.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image1.png)

<span data-ttu-id="50305-130">**Rysunek 01**: Jednostki Movie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](validating-with-the-idataerrorinfo-interface-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="50305-130">**Figure 01**: The Movie entity([Click to view full-size image](validating-with-the-idataerrorinfo-interface-cs/_static/image2.png))</span></span>


> [!NOTE] 
> 
> <span data-ttu-id="50305-131">Aby dowiedzieć się więcej o korzystaniu z programu Entity Framework do generowania klasy modelu z bazy danych, zobacz temat zatytułowany Moje samouczek: Tworzenie klas modelu za pomocą programu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="50305-131">To learn more about using the Entity Framework to generate your database model classes, see my tutorial entitled Creating Model Classes with the Entity Framework.</span></span>


## <a name="the-controller-class"></a><span data-ttu-id="50305-132">Klasa kontrolera</span><span class="sxs-lookup"><span data-stu-id="50305-132">The Controller Class</span></span>

<span data-ttu-id="50305-133">Używane kontrolera głównego do filmów z listy i tworzenia nowych filmów.</span><span class="sxs-lookup"><span data-stu-id="50305-133">We use the Home controller to list movies and create new movies.</span></span> <span data-ttu-id="50305-134">Kod dla tej klasy są zawarte w ofercie 1.</span><span class="sxs-lookup"><span data-stu-id="50305-134">The code for this class is contained in Listing 1.</span></span>

**<span data-ttu-id="50305-135">Wyświetlanie listy 1 - Controllers\HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="50305-135">Listing 1 - Controllers\HomeController.cs</span></span>**

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample1.cs)]

<span data-ttu-id="50305-136">Klasa kontrolera głównej w ofercie 1 zawiera dwie akcje Create().</span><span class="sxs-lookup"><span data-stu-id="50305-136">The Home controller class in Listing 1 contains two Create() actions.</span></span> <span data-ttu-id="50305-137">Pierwszą akcją Wyświetla formularza HTML do tworzenia nowych filmów.</span><span class="sxs-lookup"><span data-stu-id="50305-137">The first action displays the HTML form for creating a new movie.</span></span> <span data-ttu-id="50305-138">Drugiej akcji Create() przeprowadza rzeczywiste Wstaw nowy film do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="50305-138">The second Create() action performs the actual insert of the new movie into the database.</span></span> <span data-ttu-id="50305-139">Drugiej akcji Create() jest wywoływana po przesłaniu formularza wyświetlany przez pierwszą akcją Create() do serwera.</span><span class="sxs-lookup"><span data-stu-id="50305-139">The second Create() action is invoked when the form displayed by the first Create() action is submitted to the server.</span></span>

<span data-ttu-id="50305-140">Zwróć uwagę, że druga Akcja Create() zawiera następujące wiersze kodu:</span><span class="sxs-lookup"><span data-stu-id="50305-140">Notice that the second Create() action contains the following lines of code:</span></span>

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample2.cs)]

<span data-ttu-id="50305-141">Właściwość IsValid zwraca wartość false, gdy występuje błąd weryfikacji.</span><span class="sxs-lookup"><span data-stu-id="50305-141">The IsValid property returns false when there is a validation error.</span></span> <span data-ttu-id="50305-142">W takim przypadku widok Utwórz, który zawiera formularza HTML do tworzenia film zostanie wyświetlony ponownie.</span><span class="sxs-lookup"><span data-stu-id="50305-142">In that case, the Create view that contains the HTML form for creating a movie is redisplayed.</span></span>

## <a name="creating-a-partial-class"></a><span data-ttu-id="50305-143">Tworzenie klasy częściowe</span><span class="sxs-lookup"><span data-stu-id="50305-143">Creating a Partial Class</span></span>

<span data-ttu-id="50305-144">Klasa film jest generowany przez program Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="50305-144">The Movie class is generated by the Entity Framework.</span></span> <span data-ttu-id="50305-145">Widać kod klasy filmu, jeśli Rozwiń plik MoviesDBModel.edmx w oknie Eksploratora rozwiązań i Otwórz plik MoviesDBModel.Designer.cs w edytorze kodu (patrz rysunek 2).</span><span class="sxs-lookup"><span data-stu-id="50305-145">You can see the code for the Movie class if you expand the MoviesDBModel.edmx file in the Solution Explorer window and open the MoviesDBModel.Designer.cs file in the Code Editor (see Figure 2).</span></span>


[![T<span data-ttu-id="50305-146">Kod HE jednostki filmu]</span><span class="sxs-lookup"><span data-stu-id="50305-146">he code for the Movie entity]</span></span>(validating-with-the-idataerrorinfo-interface-cs/_static/image2.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image3.png)

<span data-ttu-id="50305-147">**Rysunek 02**: Kod dla jednostki Movie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](validating-with-the-idataerrorinfo-interface-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="50305-147">**Figure 02**: The code for the Movie entity([Click to view full-size image](validating-with-the-idataerrorinfo-interface-cs/_static/image4.png))</span></span>


<span data-ttu-id="50305-148">Klasa film jest klasy częściowej.</span><span class="sxs-lookup"><span data-stu-id="50305-148">The Movie class is a partial class.</span></span> <span data-ttu-id="50305-149">Oznacza to, że możemy dodać innej klasy częściowej o takiej samej nazwie, aby rozszerzyć funkcjonalność klasy filmu.</span><span class="sxs-lookup"><span data-stu-id="50305-149">That means that we can add another partial class with the same name to extend the functionality of the Movie class.</span></span> <span data-ttu-id="50305-150">Dodamy naszych logikę walidacji do nowej klasy częściowej.</span><span class="sxs-lookup"><span data-stu-id="50305-150">We'll add our validation logic to the new partial class.</span></span>

<span data-ttu-id="50305-151">Dodaj klasę w ofercie 2 do folderu modeli.</span><span class="sxs-lookup"><span data-stu-id="50305-151">Add the class in Listing 2 to the Models folder.</span></span>

**<span data-ttu-id="50305-152">Wyświetlanie listy 2 - Models\Movie.cs</span><span class="sxs-lookup"><span data-stu-id="50305-152">Listing 2 - Models\Movie.cs</span></span>**

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample3.cs)]

<span data-ttu-id="50305-153">Należy zauważyć, że klasa w ofercie 2 obejmuje *częściowe* modyfikator.</span><span class="sxs-lookup"><span data-stu-id="50305-153">Notice that the class in Listing 2 includes the *partial* modifier.</span></span> <span data-ttu-id="50305-154">Wszelkie metody lub właściwości, które możesz dodać do tej klasy stają się częścią klasy filmu generowane przez program Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="50305-154">Any methods or properties that you add to this class become part of the Movie class generated by the Entity Framework.</span></span>

## <a name="adding-onchanging-and-onchanged-partial-methods"></a><span data-ttu-id="50305-155">Dodawanie OnChanging i metod OnChanged częściowego</span><span class="sxs-lookup"><span data-stu-id="50305-155">Adding OnChanging and OnChanged Partial Methods</span></span>

<span data-ttu-id="50305-156">Gdy platforma Entity Framework generuje klasę jednostki, platformy Entity Framework metod częściowych do klasy automatycznie dodaje.</span><span class="sxs-lookup"><span data-stu-id="50305-156">When the Entity Framework generates an entity class, the Entity Framework adds partial methods to the class automatically.</span></span> <span data-ttu-id="50305-157">Entity Framework generuje OnChanging i OnChanged metod częściowych, które odpowiadają każdej właściwości klasy.</span><span class="sxs-lookup"><span data-stu-id="50305-157">The Entity Framework generates OnChanging and OnChanged partial methods that correspond to each property of the class.</span></span>

<span data-ttu-id="50305-158">W przypadku klasy filmu programu Entity Framework tworzy następujące metody:</span><span class="sxs-lookup"><span data-stu-id="50305-158">In the case of the Movie class, the Entity Framework creates the following methods:</span></span>

- <span data-ttu-id="50305-159">OnIdChanging</span><span class="sxs-lookup"><span data-stu-id="50305-159">OnIdChanging</span></span>
- <span data-ttu-id="50305-160">OnIdChanged</span><span class="sxs-lookup"><span data-stu-id="50305-160">OnIdChanged</span></span>
- <span data-ttu-id="50305-161">OnTitleChanging</span><span class="sxs-lookup"><span data-stu-id="50305-161">OnTitleChanging</span></span>
- <span data-ttu-id="50305-162">OnTitleChanged</span><span class="sxs-lookup"><span data-stu-id="50305-162">OnTitleChanged</span></span>
- <span data-ttu-id="50305-163">OnDirectorChanging</span><span class="sxs-lookup"><span data-stu-id="50305-163">OnDirectorChanging</span></span>
- <span data-ttu-id="50305-164">OnDirectorChanged</span><span class="sxs-lookup"><span data-stu-id="50305-164">OnDirectorChanged</span></span>
- <span data-ttu-id="50305-165">OnDateReleasedChanging</span><span class="sxs-lookup"><span data-stu-id="50305-165">OnDateReleasedChanging</span></span>
- <span data-ttu-id="50305-166">OnDateReleasedChanged</span><span class="sxs-lookup"><span data-stu-id="50305-166">OnDateReleasedChanged</span></span>

<span data-ttu-id="50305-167">Metoda OnChanging jest wywoływana prawo, przed zmianą odpowiadającą właściwość.</span><span class="sxs-lookup"><span data-stu-id="50305-167">The OnChanging method is called right before the corresponding property is changed.</span></span> <span data-ttu-id="50305-168">Metoda OnChanged jest wywoływana prawej, po zmianie właściwości.</span><span class="sxs-lookup"><span data-stu-id="50305-168">The OnChanged method is called right after the property is changed.</span></span>

<span data-ttu-id="50305-169">Możesz korzystać z zalet tych metod częściowych, aby dodać logikę walidacji do klasy filmu.</span><span class="sxs-lookup"><span data-stu-id="50305-169">You can take advantage of these partial methods to add validation logic to the Movie class.</span></span> <span data-ttu-id="50305-170">Aktualizacja klasy filmu w ofercie 3 sprawdza, czy tytuł i dyrektor ds. właściwości są przypisywane niepustych wartości.</span><span class="sxs-lookup"><span data-stu-id="50305-170">The update Movie class in Listing 3 verifies that the Title and Director properties are assigned nonempty values.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="50305-171">Metoda częściowa jest metoda zdefiniowana w klasie, które nie są wymagane do wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="50305-171">A partial method is a method defined in a class that you are not required to implement.</span></span> <span data-ttu-id="50305-172">Jeśli nie implementować metodę częściową kompilator usuwa podpis metody i wszystkie wywołania do metody, więc są bez kosztów czasu wykonywania skojarzony z metody częściowej.</span><span class="sxs-lookup"><span data-stu-id="50305-172">If you don't implement a partial method then the compiler removes the method signature and all calls to the method so there are no run-time costs associated with the partial method.</span></span> <span data-ttu-id="50305-173">W edytorze programu Visual Studio Code można dodać metody częściowej, wpisując słowa kluczowego *częściowe* następuje spacja, aby wyświetlić listę częściowych do zaimplementowania.</span><span class="sxs-lookup"><span data-stu-id="50305-173">In the Visual Studio Code Editor, you can add a partial method by typing the keyword *partial* followed by a space to view a list of partials to implement.</span></span>


**<span data-ttu-id="50305-174">Wyświetlanie listy 3 - Models\Movie.cs</span><span class="sxs-lookup"><span data-stu-id="50305-174">Listing 3 - Models\Movie.cs</span></span>**

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample4.cs)]

<span data-ttu-id="50305-175">Na przykład, jeśli użytkownik podejmie próbę przypisania pustego ciągu do właściwości tytułu, komunikat o błędzie jest przypisywany do słownika o nazwie \_błędy.</span><span class="sxs-lookup"><span data-stu-id="50305-175">For example, if you attempt to assign an empty string to the Title property, then an error message is assigned to a Dictionary named \_errors.</span></span>

<span data-ttu-id="50305-176">W tym momencie faktycznie nie powoduje żadnych zmian przypisania pustego ciągu do właściwości tytułu, a błąd jest dodawany do prywatnego \_pola błędy.</span><span class="sxs-lookup"><span data-stu-id="50305-176">At this point, nothing actually happens when you assign an empty string to the Title property and an error is added to the private \_errors field.</span></span> <span data-ttu-id="50305-177">Musimy zaimplementować interfejsu IDataErrorInfo, aby udostępnić te błędy sprawdzania poprawności, platforma ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="50305-177">We need to implement the IDataErrorInfo interface to expose these validation errors to the ASP.NET MVC framework.</span></span>

## <a name="implementing-the-idataerrorinfo-interface"></a><span data-ttu-id="50305-178">Implementowanie interfejsu IDataErrorInfo</span><span class="sxs-lookup"><span data-stu-id="50305-178">Implementing the IDataErrorInfo Interface</span></span>

<span data-ttu-id="50305-179">Interfejsu IDataErrorInfo było częścią programu .NET framework od pierwszej wersji.</span><span class="sxs-lookup"><span data-stu-id="50305-179">The IDataErrorInfo interface has been part of the .NET framework since the first version.</span></span> <span data-ttu-id="50305-180">Ten interfejs jest bardzo prosty interfejs:</span><span class="sxs-lookup"><span data-stu-id="50305-180">This interface is a very simple interface:</span></span>

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample5.cs)]

<span data-ttu-id="50305-181">Jeśli klasa implementuje interfejsu IDataErrorInfo, platforma ASP.NET MVC użyje tego interfejsu, podczas tworzenia wystąpienia klasy.</span><span class="sxs-lookup"><span data-stu-id="50305-181">If a class implements the IDataErrorInfo interface, the ASP.NET MVC framework will use this interface when creating an instance of the class.</span></span> <span data-ttu-id="50305-182">Na przykład kontrolera głównego działania Create() akceptuje wystąpienia klasy film:</span><span class="sxs-lookup"><span data-stu-id="50305-182">For example, the Home controller Create() action accepts an instance of the Movie class:</span></span>

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample6.cs)]

<span data-ttu-id="50305-183">Platforma ASP.NET MVC tworzy wystąpienie filmu przekazywane do działania Create() za pomocą integratora modelu (DefaultModelBinder).</span><span class="sxs-lookup"><span data-stu-id="50305-183">The ASP.NET MVC framework creates the instance of the Movie passed to the Create() action by using a model binder (the DefaultModelBinder).</span></span> <span data-ttu-id="50305-184">Integrator modelu jest odpowiedzialny za utworzenie wystąpienia obiektu filmu przez powiązanie pola formularza HTML do wystąpienia obiektu filmu.</span><span class="sxs-lookup"><span data-stu-id="50305-184">The model binder is responsible for creating an instance of the Movie object by binding the HTML form fields to an instance of the Movie object.</span></span>

<span data-ttu-id="50305-185">DefaultModelBinder wykrywa, czy klasa implementuje interfejsu IDataErrorInfo.</span><span class="sxs-lookup"><span data-stu-id="50305-185">The DefaultModelBinder detects whether or not a class implements the IDataErrorInfo interface.</span></span> <span data-ttu-id="50305-186">Jeśli klasa implementuje ten interfejs integratora modelu wywołuje indeksatora IDataErrorInfo.this dla każdej właściwości klasy.</span><span class="sxs-lookup"><span data-stu-id="50305-186">If a class implements this interface then the model binder invokes the IDataErrorInfo.this indexer for each property of the class.</span></span> <span data-ttu-id="50305-187">Jeśli indeksatora zwróci komunikat o błędzie integratora modelu dodaje ten komunikat o błędzie do modelowania stanu automatycznie.</span><span class="sxs-lookup"><span data-stu-id="50305-187">If the indexer returns an error message then the model binder adds this error message to model state automatically.</span></span>

<span data-ttu-id="50305-188">DefaultModelBinder sprawdza również właściwość IDataErrorInfo.Error.</span><span class="sxs-lookup"><span data-stu-id="50305-188">The DefaultModelBinder also checks the IDataErrorInfo.Error property.</span></span> <span data-ttu-id="50305-189">Ta właściwość jest przeznaczona do reprezentowania błędy sprawdzania poprawności określonego inną niż właściwość skojarzony z klasą.</span><span class="sxs-lookup"><span data-stu-id="50305-189">This property is intended to represent non-property specific validation errors associated with the class.</span></span> <span data-ttu-id="50305-190">Na przykład możesz chcieć wymusić regułę poprawności, który zależy od wartości wielu właściwości klasy filmu.</span><span class="sxs-lookup"><span data-stu-id="50305-190">For example, you might want to enforce a validation rule that depends on the values of multiple properties of the Movie class.</span></span> <span data-ttu-id="50305-191">W takiej sytuacji zwróci błąd sprawdzania poprawności z właściwości błędu.</span><span class="sxs-lookup"><span data-stu-id="50305-191">In that case, you would return a validation error from the Error property.</span></span>

<span data-ttu-id="50305-192">Zaktualizowano klasy filmu w ofercie 4 implementuje interfejsu IDataErrorInfo.</span><span class="sxs-lookup"><span data-stu-id="50305-192">The updated Movie class in Listing 4 implements the IDataErrorInfo interface.</span></span>

**<span data-ttu-id="50305-193">Wyświetlanie listy 4 - Models\Movie.cs (implementuje IDataErrorInfo)</span><span class="sxs-lookup"><span data-stu-id="50305-193">Listing 4 - Models\Movie.cs (implements IDataErrorInfo)</span></span>**

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample7.cs)]

<span data-ttu-id="50305-194">W ofercie 4, sprawdza właściwości indeksatora \_kolekcji błędów, aby zobaczyć, czy zawiera klucz, który odpowiada nazwie właściwości przekazany do indeksatora.</span><span class="sxs-lookup"><span data-stu-id="50305-194">In Listing 4, the indexer property checks the \_errors collection to see if it contains a key that corresponds to the property name passed to the indexer.</span></span> <span data-ttu-id="50305-195">W przypadku braku błędów weryfikacji skojarzony z właściwością, zwracany jest pusty ciąg.</span><span class="sxs-lookup"><span data-stu-id="50305-195">If there is no validation error associated with the property then an empty string is returned.</span></span>

<span data-ttu-id="50305-196">Nie potrzebujesz zmodyfikować kontrolera głównego w jakikolwiek sposób, aby użyć zmodyfikowane klasy filmu.</span><span class="sxs-lookup"><span data-stu-id="50305-196">You don't need to modify the Home controller in any way to use the modified Movie class.</span></span> <span data-ttu-id="50305-197">Strona wyświetlona na rysunku 3 przedstawiono, co się stanie, gdy została wprowadzona żadna wartość dla pola formularza tytuł lub dyrektor ds.</span><span class="sxs-lookup"><span data-stu-id="50305-197">The page displayed in Figure 3 illustrates what happens when no value is entered for the Title or Director form fields.</span></span>


[![C<span data-ttu-id="50305-198">metody akcji worzenie automatycznie]</span><span class="sxs-lookup"><span data-stu-id="50305-198">reating action methods automatically]</span></span>(validating-with-the-idataerrorinfo-interface-cs/_static/image3.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image5.png)

<span data-ttu-id="50305-199">**Rysunek 03**: Formularz z brakującymi wartościami ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](validating-with-the-idataerrorinfo-interface-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="50305-199">**Figure 03**: A form with missing values ([Click to view full-size image](validating-with-the-idataerrorinfo-interface-cs/_static/image6.png))</span></span>


<span data-ttu-id="50305-200">Zwróć uwagę, że wartość DateReleased jest automatycznie wykonane sprawdzanie poprawności.</span><span class="sxs-lookup"><span data-stu-id="50305-200">Notice that the DateReleased value is validated automatically.</span></span> <span data-ttu-id="50305-201">Ponieważ właściwość DateReleased nie akceptuje wartości NULL, DefaultModelBinder generuje błąd sprawdzania poprawności dla tej właściwości automatycznie, gdy nie ma wartości.</span><span class="sxs-lookup"><span data-stu-id="50305-201">Because the DateReleased property does not accept NULL values, the DefaultModelBinder generates a validation error for this property automatically when it does not have a value.</span></span> <span data-ttu-id="50305-202">Jeśli chcesz zmodyfikować komunikat o błędzie dla właściwości DateReleased, a następnie należy utworzyć niestandardowego integratora modelu.</span><span class="sxs-lookup"><span data-stu-id="50305-202">If you want to modify the error message for the DateReleased property then you need to create a custom model binder.</span></span>

## <a name="summary"></a><span data-ttu-id="50305-203">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="50305-203">Summary</span></span>

<span data-ttu-id="50305-204">W tym samouczku przedstawiono sposób korzystania z użyciem interfejsu IDataErrorInfo do generowania komunikatów o błędach weryfikacji.</span><span class="sxs-lookup"><span data-stu-id="50305-204">In this tutorial, you learned how to use the IDataErrorInfo interface to generate validation error messages.</span></span> <span data-ttu-id="50305-205">Po pierwsze utworzyliśmy częściową klasą filmów, która rozszerza funkcjonalność klasy częściowej filmu generowane przez program Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="50305-205">First, we created a partial Movie class that extends the functionality of the partial Movie class generated by the Entity Framework.</span></span> <span data-ttu-id="50305-206">Następnie dodaliśmy logikę walidacji do OnTitleChanging() i OnDirectorChanging() metod częściowych dla klasy filmu.</span><span class="sxs-lookup"><span data-stu-id="50305-206">Next, we added validation logic to the Movie class OnTitleChanging() and OnDirectorChanging() partial methods.</span></span> <span data-ttu-id="50305-207">Ponadto wprowadziliśmy interfejsu IDataErrorInfo, aby udostępnić te komunikaty weryfikacji platformę ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="50305-207">Finally, we implemented the IDataErrorInfo interface in order to expose these validation messages to the ASP.NET MVC framework.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="50305-208">[Poprzednie](performing-simple-validation-cs.md)
> [dalej](validating-with-a-service-layer-cs.md)</span><span class="sxs-lookup"><span data-stu-id="50305-208">[Previous](performing-simple-validation-cs.md)
[Next](validating-with-a-service-layer-cs.md)</span></span>

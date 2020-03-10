---
uid: mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-cs
title: Weryfikowanie przy użyciu interfejsu IDataErrorInfoC#() | Microsoft Docs
author: StephenWalther
description: Stephen Walther pokazuje, jak wyświetlać niestandardowe komunikaty o błędach walidacji, implementując interfejs IDataErrorInfo w klasie modelu.
ms.author: riande
ms.date: 03/02/2009
ms.assetid: 4733b9f1-9999-48fb-8b73-6038fbcc5ecb
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: 938b180da02b1963acffd021d18621d75d1d0447
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78542561"
---
# <a name="validating-with-the-idataerrorinfo-interface-c"></a><span data-ttu-id="fa4fe-103">Walidacja z użyciem interfejsu IDataErrorInfo (C#)</span><span class="sxs-lookup"><span data-stu-id="fa4fe-103">Validating with the IDataErrorInfo Interface (C#)</span></span>

<span data-ttu-id="fa4fe-104">Autor [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="fa4fe-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="fa4fe-105">Stephen Walther pokazuje, jak wyświetlać niestandardowe komunikaty o błędach walidacji, implementując interfejs IDataErrorInfo w klasie modelu.</span><span class="sxs-lookup"><span data-stu-id="fa4fe-105">Stephen Walther shows you how to display custom validation error messages by implementing the IDataErrorInfo interface in a model class.</span></span>

<span data-ttu-id="fa4fe-106">Celem tego samouczka jest wyjaśnienie jednego podejścia do wykonywania walidacji w aplikacji ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="fa4fe-106">The goal of this tutorial is to explain one approach to performing validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="fa4fe-107">Dowiesz się, jak uniemożliwić komuś przesłanie formularza HTML bez podawania wartości dla wymaganych pól formularza.</span><span class="sxs-lookup"><span data-stu-id="fa4fe-107">You learn how to prevent someone from submitting an HTML form without providing values for required form fields.</span></span> <span data-ttu-id="fa4fe-108">W tym samouczku dowiesz się, jak przeprowadzić walidację za pomocą interfejsu IErrorDataInfo.</span><span class="sxs-lookup"><span data-stu-id="fa4fe-108">In this tutorial, you learn how to perform validation by using the IErrorDataInfo interface.</span></span>

## <a name="assumptions"></a><span data-ttu-id="fa4fe-109">Założenia</span><span class="sxs-lookup"><span data-stu-id="fa4fe-109">Assumptions</span></span>

<span data-ttu-id="fa4fe-110">W tym samouczku użyjemy bazy danych MoviesDB i tabeli bazy danych filmów.</span><span class="sxs-lookup"><span data-stu-id="fa4fe-110">In this tutorial, I'll use the MoviesDB database and the Movies database table.</span></span> <span data-ttu-id="fa4fe-111">Ta tabela zawiera następujące kolumny:</span><span class="sxs-lookup"><span data-stu-id="fa4fe-111">This table has the following columns:</span></span>

<a id="0.5_table01"></a>

| <span data-ttu-id="fa4fe-112">**Nazwa kolumny**</span><span class="sxs-lookup"><span data-stu-id="fa4fe-112">**Column Name**</span></span> | <span data-ttu-id="fa4fe-113">**Typ danych**</span><span class="sxs-lookup"><span data-stu-id="fa4fe-113">**Data Type**</span></span> | <span data-ttu-id="fa4fe-114">**Zezwalaj na wartości null**</span><span class="sxs-lookup"><span data-stu-id="fa4fe-114">**Allow Nulls**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="fa4fe-115">Identyfikator</span><span class="sxs-lookup"><span data-stu-id="fa4fe-115">Id</span></span> | <span data-ttu-id="fa4fe-116">int</span><span class="sxs-lookup"><span data-stu-id="fa4fe-116">Int</span></span> | <span data-ttu-id="fa4fe-117">Fałsz</span><span class="sxs-lookup"><span data-stu-id="fa4fe-117">False</span></span> |
| <span data-ttu-id="fa4fe-118">Tytuł</span><span class="sxs-lookup"><span data-stu-id="fa4fe-118">Title</span></span> | <span data-ttu-id="fa4fe-119">Nvarchar(100)</span><span class="sxs-lookup"><span data-stu-id="fa4fe-119">Nvarchar(100)</span></span> | <span data-ttu-id="fa4fe-120">Fałsz</span><span class="sxs-lookup"><span data-stu-id="fa4fe-120">False</span></span> |
| <span data-ttu-id="fa4fe-121">Dyrektor ds.</span><span class="sxs-lookup"><span data-stu-id="fa4fe-121">Director</span></span> | <span data-ttu-id="fa4fe-122">Nvarchar(100)</span><span class="sxs-lookup"><span data-stu-id="fa4fe-122">Nvarchar(100)</span></span> | <span data-ttu-id="fa4fe-123">Fałsz</span><span class="sxs-lookup"><span data-stu-id="fa4fe-123">False</span></span> |
| <span data-ttu-id="fa4fe-124">DateReleased</span><span class="sxs-lookup"><span data-stu-id="fa4fe-124">DateReleased</span></span> | <span data-ttu-id="fa4fe-125">DateTime</span><span class="sxs-lookup"><span data-stu-id="fa4fe-125">DateTime</span></span> | <span data-ttu-id="fa4fe-126">Fałsz</span><span class="sxs-lookup"><span data-stu-id="fa4fe-126">False</span></span> |

<span data-ttu-id="fa4fe-127">W tym samouczku użyjemy Entity Framework firmy Microsoft do wygenerowania klas modelu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="fa4fe-127">In this tutorial, I use the Microsoft Entity Framework to generate my database model classes.</span></span> <span data-ttu-id="fa4fe-128">Klasa filmu wygenerowana przez Entity Framework zostanie wyświetlona na rysunku 1.</span><span class="sxs-lookup"><span data-stu-id="fa4fe-128">The Movie class generated by the Entity Framework is displayed in Figure 1.</span></span>

<span data-ttu-id="fa4fe-129">[![jednostki filmu](validating-with-the-idataerrorinfo-interface-cs/_static/image1.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="fa4fe-129">[![The Movie entity](validating-with-the-idataerrorinfo-interface-cs/_static/image1.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image1.png)</span></span>

<span data-ttu-id="fa4fe-130">**Ilustracja 01**: jednostka filmu ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](validating-with-the-idataerrorinfo-interface-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="fa4fe-130">**Figure 01**: The Movie entity([Click to view full-size image](validating-with-the-idataerrorinfo-interface-cs/_static/image2.png))</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="fa4fe-131">Aby dowiedzieć się więcej o używaniu Entity Framework do generowania klas modelu bazy danych, zobacz mój Samouczek zatytułowany Tworzenie klas modelu z Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="fa4fe-131">To learn more about using the Entity Framework to generate your database model classes, see my tutorial entitled Creating Model Classes with the Entity Framework.</span></span>

## <a name="the-controller-class"></a><span data-ttu-id="fa4fe-132">Klasa kontrolera</span><span class="sxs-lookup"><span data-stu-id="fa4fe-132">The Controller Class</span></span>

<span data-ttu-id="fa4fe-133">Używamy kontrolera macierzystego do wyświetlania filmów i tworzenia nowych filmów.</span><span class="sxs-lookup"><span data-stu-id="fa4fe-133">We use the Home controller to list movies and create new movies.</span></span> <span data-ttu-id="fa4fe-134">Kod dla tej klasy znajduje się na liście 1.</span><span class="sxs-lookup"><span data-stu-id="fa4fe-134">The code for this class is contained in Listing 1.</span></span>

<span data-ttu-id="fa4fe-135">**Lista 1 — Controllers\HomeController.cs**</span><span class="sxs-lookup"><span data-stu-id="fa4fe-135">**Listing 1 - Controllers\HomeController.cs**</span></span>

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample1.cs)]

<span data-ttu-id="fa4fe-136">Klasa kontrolera macierzystego na liście 1 zawiera dwie akcje create ().</span><span class="sxs-lookup"><span data-stu-id="fa4fe-136">The Home controller class in Listing 1 contains two Create() actions.</span></span> <span data-ttu-id="fa4fe-137">Pierwsza akcja wyświetla formularz HTML służący do tworzenia nowego filmu.</span><span class="sxs-lookup"><span data-stu-id="fa4fe-137">The first action displays the HTML form for creating a new movie.</span></span> <span data-ttu-id="fa4fe-138">Druga akcja Create () wykonuje rzeczywiste wstawianie nowego filmu do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="fa4fe-138">The second Create() action performs the actual insert of the new movie into the database.</span></span> <span data-ttu-id="fa4fe-139">Druga akcja Create () jest wywoływana, gdy formularz wyświetlany przez pierwszą akcję Utwórz () zostanie przesłany do serwera.</span><span class="sxs-lookup"><span data-stu-id="fa4fe-139">The second Create() action is invoked when the form displayed by the first Create() action is submitted to the server.</span></span>

<span data-ttu-id="fa4fe-140">Należy zauważyć, że druga akcja Create () zawiera następujące wiersze kodu:</span><span class="sxs-lookup"><span data-stu-id="fa4fe-140">Notice that the second Create() action contains the following lines of code:</span></span>

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample2.cs)]

<span data-ttu-id="fa4fe-141">Właściwość IsValid zwraca wartość false, jeśli wystąpił błąd walidacji.</span><span class="sxs-lookup"><span data-stu-id="fa4fe-141">The IsValid property returns false when there is a validation error.</span></span> <span data-ttu-id="fa4fe-142">W takim przypadku zostanie wyświetlony widok tworzenie, który zawiera formularz HTML służący do tworzenia filmu.</span><span class="sxs-lookup"><span data-stu-id="fa4fe-142">In that case, the Create view that contains the HTML form for creating a movie is redisplayed.</span></span>

## <a name="creating-a-partial-class"></a><span data-ttu-id="fa4fe-143">Tworzenie klasy częściowej</span><span class="sxs-lookup"><span data-stu-id="fa4fe-143">Creating a Partial Class</span></span>

<span data-ttu-id="fa4fe-144">Klasa filmu jest generowana przez Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="fa4fe-144">The Movie class is generated by the Entity Framework.</span></span> <span data-ttu-id="fa4fe-145">Kod dla klasy filmów można zobaczyć, jeśli rozwiniesz plik MoviesDBModel. edmx w oknie Eksplorator rozwiązań i otworzysz plik MoviesDBModel.Designer.cs w edytorze kodu (patrz rysunek 2).</span><span class="sxs-lookup"><span data-stu-id="fa4fe-145">You can see the code for the Movie class if you expand the MoviesDBModel.edmx file in the Solution Explorer window and open the MoviesDBModel.Designer.cs file in the Code Editor (see Figure 2).</span></span>

<span data-ttu-id="fa4fe-146">[![kod dla jednostki filmu](validating-with-the-idataerrorinfo-interface-cs/_static/image2.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="fa4fe-146">[![The code for the Movie entity](validating-with-the-idataerrorinfo-interface-cs/_static/image2.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image3.png)</span></span>

<span data-ttu-id="fa4fe-147">**Ilustracja 02**. kod dla jednostki filmu ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](validating-with-the-idataerrorinfo-interface-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="fa4fe-147">**Figure 02**: The code for the Movie entity([Click to view full-size image](validating-with-the-idataerrorinfo-interface-cs/_static/image4.png))</span></span>

<span data-ttu-id="fa4fe-148">Klasa filmu jest klasą częściową.</span><span class="sxs-lookup"><span data-stu-id="fa4fe-148">The Movie class is a partial class.</span></span> <span data-ttu-id="fa4fe-149">Oznacza to, że można dodać kolejną klasę częściową o tej samej nazwie, aby zwiększyć funkcjonalność klasy filmu.</span><span class="sxs-lookup"><span data-stu-id="fa4fe-149">That means that we can add another partial class with the same name to extend the functionality of the Movie class.</span></span> <span data-ttu-id="fa4fe-150">Będziemy dodawać nasze logiki walidacji do nowej klasy częściowej.</span><span class="sxs-lookup"><span data-stu-id="fa4fe-150">We'll add our validation logic to the new partial class.</span></span>

<span data-ttu-id="fa4fe-151">Dodaj klasę z listy 2 do folderu modele.</span><span class="sxs-lookup"><span data-stu-id="fa4fe-151">Add the class in Listing 2 to the Models folder.</span></span>

<span data-ttu-id="fa4fe-152">**Lista 2 — Models\Movie.cs**</span><span class="sxs-lookup"><span data-stu-id="fa4fe-152">**Listing 2 - Models\Movie.cs**</span></span>

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample3.cs)]

<span data-ttu-id="fa4fe-153">Należy zauważyć, że Klasa na liście 2 zawiera modyfikator *częściowy* .</span><span class="sxs-lookup"><span data-stu-id="fa4fe-153">Notice that the class in Listing 2 includes the *partial* modifier.</span></span> <span data-ttu-id="fa4fe-154">Wszelkie metody lub właściwości dodawane do tej klasy stają się częścią klasy filmu wygenerowanej przez Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="fa4fe-154">Any methods or properties that you add to this class become part of the Movie class generated by the Entity Framework.</span></span>

## <a name="adding-onchanging-and-onchanged-partial-methods"></a><span data-ttu-id="fa4fe-155">Dodawanie metod OnChanging i onchangeed</span><span class="sxs-lookup"><span data-stu-id="fa4fe-155">Adding OnChanging and OnChanged Partial Methods</span></span>

<span data-ttu-id="fa4fe-156">Gdy Entity Framework generuje klasę jednostki, Entity Framework automatycznie dodaje metody częściowe do klasy.</span><span class="sxs-lookup"><span data-stu-id="fa4fe-156">When the Entity Framework generates an entity class, the Entity Framework adds partial methods to the class automatically.</span></span> <span data-ttu-id="fa4fe-157">Entity Framework generuje OnChanged i OnChanged metody, które odpowiadają każdej właściwości klasy.</span><span class="sxs-lookup"><span data-stu-id="fa4fe-157">The Entity Framework generates OnChanging and OnChanged partial methods that correspond to each property of the class.</span></span>

<span data-ttu-id="fa4fe-158">W przypadku klasy film Entity Framework tworzy następujące metody:</span><span class="sxs-lookup"><span data-stu-id="fa4fe-158">In the case of the Movie class, the Entity Framework creates the following methods:</span></span>

- <span data-ttu-id="fa4fe-159">OnIdChanging</span><span class="sxs-lookup"><span data-stu-id="fa4fe-159">OnIdChanging</span></span>
- <span data-ttu-id="fa4fe-160">OnIdChanged</span><span class="sxs-lookup"><span data-stu-id="fa4fe-160">OnIdChanged</span></span>
- <span data-ttu-id="fa4fe-161">OnTitleChanging</span><span class="sxs-lookup"><span data-stu-id="fa4fe-161">OnTitleChanging</span></span>
- <span data-ttu-id="fa4fe-162">OnTitleChanged</span><span class="sxs-lookup"><span data-stu-id="fa4fe-162">OnTitleChanged</span></span>
- <span data-ttu-id="fa4fe-163">OnDirectorChanging</span><span class="sxs-lookup"><span data-stu-id="fa4fe-163">OnDirectorChanging</span></span>
- <span data-ttu-id="fa4fe-164">OnDirectorChanged</span><span class="sxs-lookup"><span data-stu-id="fa4fe-164">OnDirectorChanged</span></span>
- <span data-ttu-id="fa4fe-165">OnDateReleasedChanging</span><span class="sxs-lookup"><span data-stu-id="fa4fe-165">OnDateReleasedChanging</span></span>
- <span data-ttu-id="fa4fe-166">OnDateReleasedChanged</span><span class="sxs-lookup"><span data-stu-id="fa4fe-166">OnDateReleasedChanged</span></span>

<span data-ttu-id="fa4fe-167">Metoda onzmiana jest wywoływana bezpośrednio przed zmianą odpowiedniej właściwości.</span><span class="sxs-lookup"><span data-stu-id="fa4fe-167">The OnChanging method is called right before the corresponding property is changed.</span></span> <span data-ttu-id="fa4fe-168">Metoda OnChanged jest wywoływana bezpośrednio po zmianie właściwości.</span><span class="sxs-lookup"><span data-stu-id="fa4fe-168">The OnChanged method is called right after the property is changed.</span></span>

<span data-ttu-id="fa4fe-169">Możesz skorzystać z tych metod częściowych, aby dodać logikę walidacji do klasy film.</span><span class="sxs-lookup"><span data-stu-id="fa4fe-169">You can take advantage of these partial methods to add validation logic to the Movie class.</span></span> <span data-ttu-id="fa4fe-170">Klasa aktualizacji filmów na liście 3 sprawdza, czy tytuł i właściwości dyrektora mają przypisane niepuste wartości.</span><span class="sxs-lookup"><span data-stu-id="fa4fe-170">The update Movie class in Listing 3 verifies that the Title and Director properties are assigned nonempty values.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="fa4fe-171">Metoda częściowa to metoda zdefiniowana w klasie, która nie jest wymagana do wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="fa4fe-171">A partial method is a method defined in a class that you are not required to implement.</span></span> <span data-ttu-id="fa4fe-172">Jeśli nie zaimplementujesz metody częściowej, kompilator usunie sygnaturę metody i wszystkie wywołania metody, aby nie było kosztów wykonywania skojarzonych z metodą częściową.</span><span class="sxs-lookup"><span data-stu-id="fa4fe-172">If you don't implement a partial method then the compiler removes the method signature and all calls to the method so there are no run-time costs associated with the partial method.</span></span> <span data-ttu-id="fa4fe-173">W edytorze Visual Studio Code można dodać metodę częściową, wpisując słowo kluczowe *częściowe* , po którym następuje spacja, aby wyświetlić listę części do wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="fa4fe-173">In the Visual Studio Code Editor, you can add a partial method by typing the keyword *partial* followed by a space to view a list of partials to implement.</span></span>

<span data-ttu-id="fa4fe-174">**Lista 3 — Models\Movie.cs**</span><span class="sxs-lookup"><span data-stu-id="fa4fe-174">**Listing 3 - Models\Movie.cs**</span></span>

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample4.cs)]

<span data-ttu-id="fa4fe-175">Na przykład w przypadku próby przypisania pustego ciągu do właściwości title, komunikat o błędzie zostanie przypisany do słownika o nazwie błędy \_.</span><span class="sxs-lookup"><span data-stu-id="fa4fe-175">For example, if you attempt to assign an empty string to the Title property, then an error message is assigned to a Dictionary named \_errors.</span></span>

<span data-ttu-id="fa4fe-176">W tym momencie nic się nie dzieje, gdy przypiszesz pusty ciąg do właściwości title i zostanie dodany błąd do pola prywatne błędy \_.</span><span class="sxs-lookup"><span data-stu-id="fa4fe-176">At this point, nothing actually happens when you assign an empty string to the Title property and an error is added to the private \_errors field.</span></span> <span data-ttu-id="fa4fe-177">Musimy zaimplementować interfejs IDataErrorInfo, aby uwidocznić te błędy walidacji w strukturze ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="fa4fe-177">We need to implement the IDataErrorInfo interface to expose these validation errors to the ASP.NET MVC framework.</span></span>

## <a name="implementing-the-idataerrorinfo-interface"></a><span data-ttu-id="fa4fe-178">Implementowanie interfejsu IDataErrorInfo</span><span class="sxs-lookup"><span data-stu-id="fa4fe-178">Implementing the IDataErrorInfo Interface</span></span>

<span data-ttu-id="fa4fe-179">Interfejs IDataErrorInfo został częścią programu .NET Framework od momentu pierwszej wersji.</span><span class="sxs-lookup"><span data-stu-id="fa4fe-179">The IDataErrorInfo interface has been part of the .NET framework since the first version.</span></span> <span data-ttu-id="fa4fe-180">Ten interfejs jest bardzo prostym interfejsem:</span><span class="sxs-lookup"><span data-stu-id="fa4fe-180">This interface is a very simple interface:</span></span>

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample5.cs)]

<span data-ttu-id="fa4fe-181">Jeśli klasa implementuje interfejs IDataErrorInfo, platforma MVC ASP.NET będzie używać tego interfejsu podczas tworzenia wystąpienia klasy.</span><span class="sxs-lookup"><span data-stu-id="fa4fe-181">If a class implements the IDataErrorInfo interface, the ASP.NET MVC framework will use this interface when creating an instance of the class.</span></span> <span data-ttu-id="fa4fe-182">Na przykład akcja Create () na kontrolerze głównym akceptuje wystąpienie klasy Movie:</span><span class="sxs-lookup"><span data-stu-id="fa4fe-182">For example, the Home controller Create() action accepts an instance of the Movie class:</span></span>

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample6.cs)]

<span data-ttu-id="fa4fe-183">Struktura ASP.NET MVC tworzy wystąpienie filmu przesłane do akcji Create () przy użyciu spinacza modelu (DefaultModelBinder).</span><span class="sxs-lookup"><span data-stu-id="fa4fe-183">The ASP.NET MVC framework creates the instance of the Movie passed to the Create() action by using a model binder (the DefaultModelBinder).</span></span> <span data-ttu-id="fa4fe-184">Model spinacza jest odpowiedzialny za tworzenie wystąpienia obiektu filmu przez powiązanie pól formularza HTML z wystąpieniem obiektu filmu.</span><span class="sxs-lookup"><span data-stu-id="fa4fe-184">The model binder is responsible for creating an instance of the Movie object by binding the HTML form fields to an instance of the Movie object.</span></span>

<span data-ttu-id="fa4fe-185">DefaultModelBinder wykrywa, czy Klasa implementuje interfejs IDataErrorInfo.</span><span class="sxs-lookup"><span data-stu-id="fa4fe-185">The DefaultModelBinder detects whether or not a class implements the IDataErrorInfo interface.</span></span> <span data-ttu-id="fa4fe-186">Jeśli klasa implementuje ten interfejs, spinacz modelu wywoła IDataErrorInfo. ten indeksator dla każdej właściwości klasy.</span><span class="sxs-lookup"><span data-stu-id="fa4fe-186">If a class implements this interface then the model binder invokes the IDataErrorInfo.this indexer for each property of the class.</span></span> <span data-ttu-id="fa4fe-187">Jeśli indeksator zwróci komunikat o błędzie, spinacz modelu automatycznie dodaje ten komunikat o błędzie do stanu modelu.</span><span class="sxs-lookup"><span data-stu-id="fa4fe-187">If the indexer returns an error message then the model binder adds this error message to model state automatically.</span></span>

<span data-ttu-id="fa4fe-188">DefaultModelBinder również sprawdza Właściwość IDataErrorInfo. Error.</span><span class="sxs-lookup"><span data-stu-id="fa4fe-188">The DefaultModelBinder also checks the IDataErrorInfo.Error property.</span></span> <span data-ttu-id="fa4fe-189">Ta właściwość jest przeznaczona do reprezentowania błędów walidacji specyficznych dla niewłaściwości skojarzonych z klasą.</span><span class="sxs-lookup"><span data-stu-id="fa4fe-189">This property is intended to represent non-property specific validation errors associated with the class.</span></span> <span data-ttu-id="fa4fe-190">Na przykład możesz chcieć wymusić regułę walidacji, która zależy od wartości wielu właściwości klasy filmu.</span><span class="sxs-lookup"><span data-stu-id="fa4fe-190">For example, you might want to enforce a validation rule that depends on the values of multiple properties of the Movie class.</span></span> <span data-ttu-id="fa4fe-191">W takim przypadku należy zwrócić błąd walidacji z właściwości Error.</span><span class="sxs-lookup"><span data-stu-id="fa4fe-191">In that case, you would return a validation error from the Error property.</span></span>

<span data-ttu-id="fa4fe-192">Zaktualizowana Klasa filmów w liście 4 implementuje interfejs IDataErrorInfo.</span><span class="sxs-lookup"><span data-stu-id="fa4fe-192">The updated Movie class in Listing 4 implements the IDataErrorInfo interface.</span></span>

<span data-ttu-id="fa4fe-193">**Lista 4-Models\Movie.cs (implementuje IDataErrorInfo)**</span><span class="sxs-lookup"><span data-stu-id="fa4fe-193">**Listing 4 - Models\Movie.cs (implements IDataErrorInfo)**</span></span>

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample7.cs)]

<span data-ttu-id="fa4fe-194">W przypadku listy 4 Właściwość indeksatora sprawdza zbieranie \_błędów, aby sprawdzić, czy zawiera on klucz odpowiadający nazwie właściwości przesłanej do indeksatora.</span><span class="sxs-lookup"><span data-stu-id="fa4fe-194">In Listing 4, the indexer property checks the \_errors collection to see if it contains a key that corresponds to the property name passed to the indexer.</span></span> <span data-ttu-id="fa4fe-195">Jeśli nie ma błędu walidacji skojarzonego z właściwością, zwracany jest pusty ciąg.</span><span class="sxs-lookup"><span data-stu-id="fa4fe-195">If there is no validation error associated with the property then an empty string is returned.</span></span>

<span data-ttu-id="fa4fe-196">Nie musisz modyfikować kontrolera macierzystego w dowolny sposób, aby użyć zmodyfikowanej klasy filmów.</span><span class="sxs-lookup"><span data-stu-id="fa4fe-196">You don't need to modify the Home controller in any way to use the modified Movie class.</span></span> <span data-ttu-id="fa4fe-197">Strona wyświetlana na rysunku 3 ilustruje, co się dzieje, gdy nie wprowadzono wartości dla pól tytuł lub dyrektor.</span><span class="sxs-lookup"><span data-stu-id="fa4fe-197">The page displayed in Figure 3 illustrates what happens when no value is entered for the Title or Director form fields.</span></span>

<span data-ttu-id="fa4fe-198">[![tworzenia metod akcji automatycznie](validating-with-the-idataerrorinfo-interface-cs/_static/image3.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="fa4fe-198">[![Creating action methods automatically](validating-with-the-idataerrorinfo-interface-cs/_static/image3.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image5.png)</span></span>

<span data-ttu-id="fa4fe-199">**Ilustracja 03**: formularz z brakującymi wartościami ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](validating-with-the-idataerrorinfo-interface-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="fa4fe-199">**Figure 03**: A form with missing values ([Click to view full-size image](validating-with-the-idataerrorinfo-interface-cs/_static/image6.png))</span></span>

<span data-ttu-id="fa4fe-200">Zauważ, że wartość DateReleased jest weryfikowana automatycznie.</span><span class="sxs-lookup"><span data-stu-id="fa4fe-200">Notice that the DateReleased value is validated automatically.</span></span> <span data-ttu-id="fa4fe-201">Ponieważ właściwość DateReleased nie akceptuje wartości NULL, DefaultModelBinder generuje błąd walidacji dla tej właściwości automatycznie, gdy nie ma wartości.</span><span class="sxs-lookup"><span data-stu-id="fa4fe-201">Because the DateReleased property does not accept NULL values, the DefaultModelBinder generates a validation error for this property automatically when it does not have a value.</span></span> <span data-ttu-id="fa4fe-202">Jeśli chcesz zmodyfikować komunikat o błędzie dla właściwości DateReleased, musisz utworzyć spinacz modelu niestandardowego.</span><span class="sxs-lookup"><span data-stu-id="fa4fe-202">If you want to modify the error message for the DateReleased property then you need to create a custom model binder.</span></span>

## <a name="summary"></a><span data-ttu-id="fa4fe-203">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="fa4fe-203">Summary</span></span>

<span data-ttu-id="fa4fe-204">W tym samouczku przedstawiono sposób użycia interfejsu IDataErrorInfo do generowania komunikatów o błędach walidacji.</span><span class="sxs-lookup"><span data-stu-id="fa4fe-204">In this tutorial, you learned how to use the IDataErrorInfo interface to generate validation error messages.</span></span> <span data-ttu-id="fa4fe-205">Najpierw tworzymy częściową klasę filmu, która rozszerza funkcjonalność częściowej klasy filmowej wygenerowanej przez Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="fa4fe-205">First, we created a partial Movie class that extends the functionality of the partial Movie class generated by the Entity Framework.</span></span> <span data-ttu-id="fa4fe-206">Następnie dodaliśmy logikę sprawdzania poprawności do klasy filmów OnTitleChanging () i OnDirectorChanging () metod częściowych.</span><span class="sxs-lookup"><span data-stu-id="fa4fe-206">Next, we added validation logic to the Movie class OnTitleChanging() and OnDirectorChanging() partial methods.</span></span> <span data-ttu-id="fa4fe-207">Na koniec zaimplementowano interfejs IDataErrorInfo, aby uwidocznić te komunikaty weryfikacyjne w strukturze ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="fa4fe-207">Finally, we implemented the IDataErrorInfo interface in order to expose these validation messages to the ASP.NET MVC framework.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="fa4fe-208">[Poprzednie](performing-simple-validation-cs.md)
> [dalej](validating-with-a-service-layer-cs.md)</span><span class="sxs-lookup"><span data-stu-id="fa4fe-208">[Previous](performing-simple-validation-cs.md)
[Next](validating-with-a-service-layer-cs.md)</span></span>

---
uid: web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
title: Aktualizowanie, usuwanie i tworzenie danych za pomocą powiązania modelu i formularzy sieci Web | Microsoft Docs
author: Rick-Anderson
description: Ta seria samouczków pokazuje podstawowe aspekty używania powiązania modelu z projektem formularzy sieci Web ASP.NET. Powiązanie modelu sprawia, że interakcje danych są bardziej proste-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 602baa94-5a4f-46eb-a717-7a9e539c1db4
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
msc.type: authoredcontent
ms.openlocfilehash: 11dc52ec79ca91119b37ea60e08164eb1b1d0e2b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78586647"
---
# <a name="updating-deleting-and-creating-data-with-model-binding-and-web-forms"></a><span data-ttu-id="b2132-104">Aktualizowanie, usuwanie i tworzenie danych za pomocą powiązania modelu i formularzy sieci Web</span><span class="sxs-lookup"><span data-stu-id="b2132-104">Updating, deleting, and creating data with model binding and web forms</span></span>

<span data-ttu-id="b2132-105">Autor [FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="b2132-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="b2132-106">Ta seria samouczków pokazuje podstawowe aspekty używania powiązania modelu z projektem formularzy sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="b2132-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="b2132-107">Powiązanie modelu sprawia, że interakcje danych są bardziej proste, niż w przypadku obiektów źródła danych (np. ObjectDataSource lub kontrolki SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="b2132-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="b2132-108">Ta seria rozpoczyna się od materiału wstępnego i przenosi do bardziej zaawansowanych koncepcji w kolejnych samouczkach.</span><span class="sxs-lookup"><span data-stu-id="b2132-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="b2132-109">W tym samouczku pokazano, jak tworzyć, aktualizować i usuwać dane przy użyciu powiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="b2132-109">This tutorial shows how to create, update, and delete data with model binding.</span></span> <span data-ttu-id="b2132-110">Należy ustawić następujące właściwości:</span><span class="sxs-lookup"><span data-stu-id="b2132-110">You will set the following properties:</span></span>
> 
> - <span data-ttu-id="b2132-111">DeleteMethod</span><span class="sxs-lookup"><span data-stu-id="b2132-111">DeleteMethod</span></span>
> - <span data-ttu-id="b2132-112">InsertMethod</span><span class="sxs-lookup"><span data-stu-id="b2132-112">InsertMethod</span></span>
> - <span data-ttu-id="b2132-113">UpdateMethod</span><span class="sxs-lookup"><span data-stu-id="b2132-113">UpdateMethod</span></span>
> 
> <span data-ttu-id="b2132-114">Te właściwości otrzymują nazwę metody, która obsługuje odpowiednią operację.</span><span class="sxs-lookup"><span data-stu-id="b2132-114">These properties receive the name of the method that handles the corresponding operation.</span></span> <span data-ttu-id="b2132-115">W ramach tej metody należy zapewnić logikę do współpracy z danymi.</span><span class="sxs-lookup"><span data-stu-id="b2132-115">Within that method, you provide the logic for interacting with the data.</span></span>
> 
> <span data-ttu-id="b2132-116">Ten samouczek jest oparty na projekcie utworzonym w pierwszej [części](retrieving-data.md) serii.</span><span class="sxs-lookup"><span data-stu-id="b2132-116">This tutorial builds on the project created in the first [part](retrieving-data.md) of the series.</span></span>
> 
> <span data-ttu-id="b2132-117">Możesz [pobrać](https://go.microsoft.com/fwlink/?LinkId=286116) kompletny projekt w C# języku lub VB.</span><span class="sxs-lookup"><span data-stu-id="b2132-117">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="b2132-118">Kod do pobrania współdziała z programem Visual Studio 2012 lub Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="b2132-118">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="b2132-119">Używa szablonu programu Visual Studio 2012, który jest nieco inny niż szablon Visual Studio 2013 przedstawiony w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="b2132-119">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>

## <a name="what-youll-build"></a><span data-ttu-id="b2132-120">Co będziesz kompilować</span><span class="sxs-lookup"><span data-stu-id="b2132-120">What you'll build</span></span>

<span data-ttu-id="b2132-121">W tym samouczku przedstawiono następujące instrukcje:</span><span class="sxs-lookup"><span data-stu-id="b2132-121">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="b2132-122">Dodawanie dynamicznych szablonów danych</span><span class="sxs-lookup"><span data-stu-id="b2132-122">Add dynamic data templates</span></span>
2. <span data-ttu-id="b2132-123">Włącz aktualizowanie i usuwanie danych za poorednictwem metod powiązania modelu</span><span class="sxs-lookup"><span data-stu-id="b2132-123">Enable updating and deleting data through model binding methods</span></span>
3. <span data-ttu-id="b2132-124">Zastosuj reguły sprawdzania poprawności danych — Włącz tworzenie nowego rekordu w bazie danych</span><span class="sxs-lookup"><span data-stu-id="b2132-124">Apply data validation rules - Enable creating a new record in the database</span></span>

## <a name="add-dynamic-data-templates"></a><span data-ttu-id="b2132-125">Dodawanie dynamicznych szablonów danych</span><span class="sxs-lookup"><span data-stu-id="b2132-125">Add dynamic data templates</span></span>

<span data-ttu-id="b2132-126">Aby zapewnić najlepsze środowisko użytkownika i zminimalizować powtarzanie kodu, będziesz używać dynamicznych szablonów danych.</span><span class="sxs-lookup"><span data-stu-id="b2132-126">To provide the best user experience and minimize code repetition, you will use dynamic data templates.</span></span> <span data-ttu-id="b2132-127">Możesz łatwo zintegrować wstępnie skompilowane szablony danych dynamicznych z istniejącą lokacją, instalując pakiet NuGet.</span><span class="sxs-lookup"><span data-stu-id="b2132-127">You can easily integrate pre-built dynamic data templates into your existing site by installing a NuGet package.</span></span>

<span data-ttu-id="b2132-128">Na stronie **Zarządzanie pakietami NuGet**zainstaluj **DynamicDataTemplatesCS**.</span><span class="sxs-lookup"><span data-stu-id="b2132-128">From the **Manage NuGet Packages**, install the **DynamicDataTemplatesCS**.</span></span>

![dynamiczne szablony danych](updating-deleting-and-creating-data/_static/image1.png)

<span data-ttu-id="b2132-130">Zwróć uwagę, że projekt zawiera teraz folder o nazwie **DynamicData**.</span><span class="sxs-lookup"><span data-stu-id="b2132-130">Notice that your project now includes a folder named **DynamicData**.</span></span> <span data-ttu-id="b2132-131">W tym folderze znajdują się szablony, które są automatycznie stosowane do formantów dynamicznych w formularzach sieci Web.</span><span class="sxs-lookup"><span data-stu-id="b2132-131">In that folder, you will find the templates that are automatically applied to dynamic controls in your web forms.</span></span>

![folder danych dynamicznych](updating-deleting-and-creating-data/_static/image2.png)

## <a name="enable-updating-and-deleting"></a><span data-ttu-id="b2132-133">Włącz aktualizowanie i usuwanie</span><span class="sxs-lookup"><span data-stu-id="b2132-133">Enable updating and deleting</span></span>

<span data-ttu-id="b2132-134">Umożliwienie użytkownikom aktualizowania i usuwania rekordów w bazie danych jest bardzo podobne do procesu pobierania danych.</span><span class="sxs-lookup"><span data-stu-id="b2132-134">Enabling users to update and delete records in the database is very similar to the process for retrieving data.</span></span> <span data-ttu-id="b2132-135">We właściwościach **UpdateMethod** i **DeleteMethod** należy określić nazwy metod, które wykonują te operacje.</span><span class="sxs-lookup"><span data-stu-id="b2132-135">In the **UpdateMethod** and **DeleteMethod** properties, you specify the names of the methods that perform those operations.</span></span> <span data-ttu-id="b2132-136">Za pomocą kontrolki GridView można także określić automatyczne generowanie przycisków Edytuj i Usuń.</span><span class="sxs-lookup"><span data-stu-id="b2132-136">With a GridView control, you can also specify the automatic generation of edit and delete buttons.</span></span> <span data-ttu-id="b2132-137">Poniższy wyróżniony kod pokazuje Dodatki do kodu GridView.</span><span class="sxs-lookup"><span data-stu-id="b2132-137">The following highlighted code shows the additions to the GridView code.</span></span>

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample1.aspx?highlight=4-5)]

<span data-ttu-id="b2132-138">W pliku związanym z kodem Dodaj instrukcję using dla elementu **System. Data. Entity. Infrastructure**.</span><span class="sxs-lookup"><span data-stu-id="b2132-138">In the code-behind file, add a using statement for **System.Data.Entity.Infrastructure**.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample2.cs)]

<span data-ttu-id="b2132-139">Następnie Dodaj następujące metody Update i DELETE.</span><span class="sxs-lookup"><span data-stu-id="b2132-139">Then, add the following update and delete methods.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample3.cs)]

<span data-ttu-id="b2132-140">Metoda **TryUpdateModel** stosuje odpowiednie wartości powiązane z danymi z formularza sieci Web do elementu danych.</span><span class="sxs-lookup"><span data-stu-id="b2132-140">The **TryUpdateModel** method applies the matching data-bound values from the web form to the data item.</span></span> <span data-ttu-id="b2132-141">Element danych jest pobierany na podstawie wartości parametru ID.</span><span class="sxs-lookup"><span data-stu-id="b2132-141">The data item is retrieved based on the value of the id parameter.</span></span>

## <a name="enforce-validation-requirements"></a><span data-ttu-id="b2132-142">Wymuś wymagania dotyczące weryfikacji</span><span class="sxs-lookup"><span data-stu-id="b2132-142">Enforce validation requirements</span></span>

<span data-ttu-id="b2132-143">Atrybuty walidacji zastosowane do właściwości FirstName, LastName i Year w klasie student są automatycznie wymuszane podczas aktualizacji danych.</span><span class="sxs-lookup"><span data-stu-id="b2132-143">The validation attributes that you applied to the FirstName, LastName, and Year properties in the Student class are automatically enforced when updating the data.</span></span> <span data-ttu-id="b2132-144">Kontrolki DynamicField dodają moduł sprawdzania poprawności klienta i serwera na podstawie atrybutów walidacji.</span><span class="sxs-lookup"><span data-stu-id="b2132-144">The DynamicField controls add client and server validators based on the validation attributes.</span></span> <span data-ttu-id="b2132-145">Właściwości FirstName i LastName są wymagane.</span><span class="sxs-lookup"><span data-stu-id="b2132-145">The FirstName and LastName properties are both required.</span></span> <span data-ttu-id="b2132-146">Długość FirstName nie może przekraczać 20 znaków, a LastName nie może przekraczać 40 znaków.</span><span class="sxs-lookup"><span data-stu-id="b2132-146">FirstName cannot exceed 20 characters in length, and LastName cannot exceed 40 characters.</span></span> <span data-ttu-id="b2132-147">Rok musi być prawidłową wartością wyliczenia AcademicYear.</span><span class="sxs-lookup"><span data-stu-id="b2132-147">Year must be a valid value for the AcademicYear enumeration.</span></span>

<span data-ttu-id="b2132-148">Jeśli użytkownik narusza jedno z wymagań dotyczących weryfikacji, aktualizacja nie zostanie przebiegać.</span><span class="sxs-lookup"><span data-stu-id="b2132-148">If the user violates one of the validation requirements, the update does not proceed.</span></span> <span data-ttu-id="b2132-149">Aby wyświetlić komunikat o błędzie, Dodaj kontrolkę podsumowania walidacji nad elementem GridView.</span><span class="sxs-lookup"><span data-stu-id="b2132-149">To see the error message, add a ValidationSummary control above the GridView.</span></span> <span data-ttu-id="b2132-150">Aby wyświetlić błędy walidacji z powiązania modelu, ustaw dla właściwości **ShowModelStateErrors** **wartość true**.</span><span class="sxs-lookup"><span data-stu-id="b2132-150">To display the validation errors from model binding, set the **ShowModelStateErrors** property set to **true**.</span></span> 

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample4.aspx)]

<span data-ttu-id="b2132-151">Uruchom aplikację sieci Web i zaktualizuj i Usuń wszystkie rekordy.</span><span class="sxs-lookup"><span data-stu-id="b2132-151">Run the web application, and update and delete any of the records.</span></span>

![Aktualizowanie danych](updating-deleting-and-creating-data/_static/image3.png)

<span data-ttu-id="b2132-153">Zauważ, że w trybie edycji wartość właściwości Year jest automatycznie renderowany jako lista rozwijana.</span><span class="sxs-lookup"><span data-stu-id="b2132-153">Notice that when in the edit mode the value for the Year property is automatically rendered as a drop down list.</span></span> <span data-ttu-id="b2132-154">Właściwość Year jest wartością wyliczenia, a dynamiczny szablon danych dla wartości wyliczenia określa listę rozwijaną do edycji.</span><span class="sxs-lookup"><span data-stu-id="b2132-154">The Year property is an enumeration value, and the dynamic data template for an enumeration value specifies a drop down list for editing.</span></span> <span data-ttu-id="b2132-155">Ten szablon można znaleźć, otwierając **wyliczenie\_Edytuj plik. ascx** w folderze **DynamicData**/**FieldTemplates** .</span><span class="sxs-lookup"><span data-stu-id="b2132-155">You can find that template by opening the **Enumeration\_Edit.ascx** file in the **DynamicData**/**FieldTemplates** folder.</span></span>

<span data-ttu-id="b2132-156">W przypadku podania prawidłowych wartości aktualizacja zostanie zakończona pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="b2132-156">If you provide valid values, the update completes successfully.</span></span> <span data-ttu-id="b2132-157">W przypadku naruszenia jednego z wymagań dotyczących weryfikacji aktualizacja nie zostanie przebiegać i zostanie wyświetlony komunikat o błędzie powyżej siatki.</span><span class="sxs-lookup"><span data-stu-id="b2132-157">If you violate one of the validation requirements, the update does not proceed and an error message is displayed above the grid.</span></span>

![komunikat o błędzie](updating-deleting-and-creating-data/_static/image4.png)

## <a name="add-new-records"></a><span data-ttu-id="b2132-159">Dodaj nowe rekordy</span><span class="sxs-lookup"><span data-stu-id="b2132-159">Add new records</span></span>

<span data-ttu-id="b2132-160">Formant GridView nie zawiera właściwości **InsertMethod** i dlatego nie można go użyć do dodania nowego rekordu z powiązaniem modelu.</span><span class="sxs-lookup"><span data-stu-id="b2132-160">The GridView control does not include the **InsertMethod** property and therefore cannot be used for adding a new record with model binding.</span></span> <span data-ttu-id="b2132-161">Właściwość InsertMethod można znaleźć w kontrolkach **FormView**, **DetailsView**lub **ListView** .</span><span class="sxs-lookup"><span data-stu-id="b2132-161">You can find the InsertMethod property in the **FormView**, **DetailsView**, or **ListView** controls.</span></span> <span data-ttu-id="b2132-162">W tym samouczku użyjesz kontrolki FormView, aby dodać nowy rekord.</span><span class="sxs-lookup"><span data-stu-id="b2132-162">In this tutorial, you will use a FormView control to add a new record.</span></span>

<span data-ttu-id="b2132-163">Najpierw Dodaj link do nowej strony, która zostanie utworzona w celu dodania nowego rekordu.</span><span class="sxs-lookup"><span data-stu-id="b2132-163">First, add a link to the new page you will create for adding a new record.</span></span> <span data-ttu-id="b2132-164">Powyżej podsumowania walidacji Dodaj:</span><span class="sxs-lookup"><span data-stu-id="b2132-164">Above the ValidationSummary, add:</span></span>

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample5.aspx)]

<span data-ttu-id="b2132-165">Nowe łącze pojawi się u góry zawartości strony uczniów.</span><span class="sxs-lookup"><span data-stu-id="b2132-165">The new link will appear at the top of the content for the Students page.</span></span>

![nowy link](updating-deleting-and-creating-data/_static/image5.png)

<span data-ttu-id="b2132-167">Następnie Dodaj nowy formularz sieci Web przy użyciu strony głównej, a następnie nadaj mu nazwę **addstudent**.</span><span class="sxs-lookup"><span data-stu-id="b2132-167">Then, add a new web form using a master page, and name it **AddStudent**.</span></span> <span data-ttu-id="b2132-168">Wybierz pozycję site. Master jako stronę wzorcową.</span><span class="sxs-lookup"><span data-stu-id="b2132-168">Select Site.Master as the master page.</span></span>

<span data-ttu-id="b2132-169">Zostaną wyrenderowane pola umożliwiające dodanie nowego ucznia przy użyciu kontrolki **DynamicControl** .</span><span class="sxs-lookup"><span data-stu-id="b2132-169">You will render the fields for adding a new student by using a **DynamicEntity** control.</span></span> <span data-ttu-id="b2132-170">Kontrolka DynamicControl renderuje te edytowalne właściwości w klasie określonej we właściwości ItemType.</span><span class="sxs-lookup"><span data-stu-id="b2132-170">The DynamicEntity control renders that editable properties in the class specified in the ItemType property.</span></span> <span data-ttu-id="b2132-171">Właściwość StudentID została oznaczona przy użyciu atrybutu **[ScaffoldColumn (false)]** , więc nie jest renderowana.</span><span class="sxs-lookup"><span data-stu-id="b2132-171">The StudentID property was marked with the **[ScaffoldColumn(false)]** attribute so it is not rendered.</span></span> <span data-ttu-id="b2132-172">W symbolu zastępczym kontrolka mainContent strony addstudent Dodaj następujący kod.</span><span class="sxs-lookup"><span data-stu-id="b2132-172">In the MainContent placeholder of the AddStudent page, add the following code.</span></span>

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample6.aspx)]

<span data-ttu-id="b2132-173">W pliku związanym z kodem (AddStudent.aspx.cs) Dodaj instrukcję **using** dla przestrzeni nazw **ContosoUniversityModelBinding. models** .</span><span class="sxs-lookup"><span data-stu-id="b2132-173">In the code-behind file (AddStudent.aspx.cs), add a **using** statement for the **ContosoUniversityModelBinding.Models** namespace.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample7.cs)]

<span data-ttu-id="b2132-174">Następnie Dodaj następujące metody, aby określić, jak wstawić nowy rekord i program obsługi zdarzeń dla przycisku Anuluj.</span><span class="sxs-lookup"><span data-stu-id="b2132-174">Then, add the following methods to specify how to insert a new record and an event handler for the cancel button.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample8.cs)]

<span data-ttu-id="b2132-175">Zapisz wszystkie zmiany.</span><span class="sxs-lookup"><span data-stu-id="b2132-175">Save all of the changes.</span></span>

<span data-ttu-id="b2132-176">Uruchom aplikację sieci Web i Utwórz nowego ucznia.</span><span class="sxs-lookup"><span data-stu-id="b2132-176">Run the web application and create a new student.</span></span>

![Dodaj nowego ucznia](updating-deleting-and-creating-data/_static/image6.png)

<span data-ttu-id="b2132-178">Kliknij przycisk **Wstaw** , aby zauważyć, że nowy student został utworzony.</span><span class="sxs-lookup"><span data-stu-id="b2132-178">Click **Insert** and notice the new student has been created.</span></span>

![Wyświetl nowych uczniów](updating-deleting-and-creating-data/_static/image7.png)

## <a name="conclusion"></a><span data-ttu-id="b2132-180">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="b2132-180">Conclusion</span></span>

<span data-ttu-id="b2132-181">W ramach tego samouczka włączono aktualizowanie, usuwanie i tworzenie danych.</span><span class="sxs-lookup"><span data-stu-id="b2132-181">In this tutorial, you enabled updating, deleting, and creating data.</span></span> <span data-ttu-id="b2132-182">Reguły sprawdzania poprawności są stosowane podczas korzystania z danych.</span><span class="sxs-lookup"><span data-stu-id="b2132-182">You ensured validation rules are applied when interacting with the data.</span></span>

<span data-ttu-id="b2132-183">W następnym [samouczku](sorting-paging-and-filtering-data.md) w tej serii zostanie włączone sortowanie, stronicowanie i filtrowanie danych.</span><span class="sxs-lookup"><span data-stu-id="b2132-183">In the next [tutorial](sorting-paging-and-filtering-data.md) in this series, you will enable sorting, paging, and filtering data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b2132-184">[Poprzednie](retrieving-data.md)
> [dalej](sorting-paging-and-filtering-data.md)</span><span class="sxs-lookup"><span data-stu-id="b2132-184">[Previous](retrieving-data.md)
[Next](sorting-paging-and-filtering-data.md)</span></span>

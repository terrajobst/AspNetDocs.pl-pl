---
uid: web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
title: Integrowanie interfejsu użytkownika JQuery DatePicker z powiązaniem modelu i formularzami sieci Web | Microsoft Docs
author: Rick-Anderson
description: Ta seria samouczków pokazuje podstawowe aspekty używania powiązania modelu z projektem formularzy sieci Web ASP.NET. Powiązanie modelu sprawia, że interakcje danych są bardziej proste-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 3cbab37b-fb0f-4751-9ec4-74e068c3f380
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: c8d711dd44950116f3a3e09d5d12c507918c543f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78642479"
---
# <a name="integrating-jquery-ui-datepicker-with-model-binding-and-web-forms"></a><span data-ttu-id="d744e-104">Integrowanie interfejsu użytkownika JQuery DatePicker z powiązaniem modelu i formularzami sieci Web</span><span class="sxs-lookup"><span data-stu-id="d744e-104">Integrating JQuery UI Datepicker with model binding and web forms</span></span>

<span data-ttu-id="d744e-105">Autor [FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="d744e-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="d744e-106">Ta seria samouczków pokazuje podstawowe aspekty używania powiązania modelu z projektem formularzy sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="d744e-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="d744e-107">Powiązanie modelu sprawia, że interakcje danych są bardziej proste, niż w przypadku obiektów źródła danych (np. ObjectDataSource lub kontrolki SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="d744e-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="d744e-108">Ta seria rozpoczyna się od materiału wstępnego i przenosi do bardziej zaawansowanych koncepcji w kolejnych samouczkach.</span><span class="sxs-lookup"><span data-stu-id="d744e-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="d744e-109">W tym samouczku pokazano, jak dodać [widżet DatePicker](http://jqueryui.com/datepicker/) interfejsu użytkownika jQuery do formularza sieci Web, a następnie użyć powiązania modelu w celu zaktualizowania bazy danych przy użyciu wybranej wartości.</span><span class="sxs-lookup"><span data-stu-id="d744e-109">This tutorial shows how to add the JQuery UI [Datepicker widget](http://jqueryui.com/datepicker/) to a Web Form, and use model binding to update the database with the selected value.</span></span>
> 
> <span data-ttu-id="d744e-110">Ten samouczek jest oparty na projekcie utworzonym w [pierwszej](retrieving-data.md) i [drugiej](updating-deleting-and-creating-data.md) części serii.</span><span class="sxs-lookup"><span data-stu-id="d744e-110">This tutorial builds on the project created in the [first](retrieving-data.md) and [second](updating-deleting-and-creating-data.md) parts of the series.</span></span>
> 
> <span data-ttu-id="d744e-111">Możesz [pobrać](https://go.microsoft.com/fwlink/?LinkId=286116) kompletny projekt w C# języku lub VB.</span><span class="sxs-lookup"><span data-stu-id="d744e-111">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="d744e-112">Kod do pobrania współdziała z programem Visual Studio 2012 lub Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="d744e-112">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="d744e-113">Używa szablonu programu Visual Studio 2012, który jest nieco inny niż szablon Visual Studio 2013 przedstawiony w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="d744e-113">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>

## <a name="what-youll-build"></a><span data-ttu-id="d744e-114">Co będziesz kompilować</span><span class="sxs-lookup"><span data-stu-id="d744e-114">What you'll build</span></span>

<span data-ttu-id="d744e-115">W tym samouczku przedstawiono następujące instrukcje:</span><span class="sxs-lookup"><span data-stu-id="d744e-115">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="d744e-116">Dodaj właściwość do modelu, aby zarejestrować datę rejestracji ucznia</span><span class="sxs-lookup"><span data-stu-id="d744e-116">Add a property to your model to record the student's enrollment date</span></span>
2. <span data-ttu-id="d744e-117">Umożliwia użytkownikowi wybranie daty rejestracji za pomocą widżetu DatePicker interfejsu użytkownika JQuery</span><span class="sxs-lookup"><span data-stu-id="d744e-117">Enable the user to select the enrollment date using the JQuery UI Datepicker widget</span></span>
3. <span data-ttu-id="d744e-118">Wymuś reguły walidacji dla daty rejestracji</span><span class="sxs-lookup"><span data-stu-id="d744e-118">Enforce validation rules for the enrollment date</span></span>

<span data-ttu-id="d744e-119">Widżet DatePicker interfejsu użytkownika JQuery pozwala użytkownikom łatwo wybrać datę z kalendarza, który pojawia się, gdy użytkownik współdziała z polem.</span><span class="sxs-lookup"><span data-stu-id="d744e-119">The JQuery UI Datepicker widget enables users to easily select a date from a calendar that pops up when the user interacts with the field.</span></span> <span data-ttu-id="d744e-120">Korzystanie z tego widżetu może być wygodniejsze dla użytkowników niż Ręczne wpisywanie daty.</span><span class="sxs-lookup"><span data-stu-id="d744e-120">Using this widget can be more convenient for users than manually typing a date.</span></span> <span data-ttu-id="d744e-121">Integrowanie widżetu DatePicker ze stroną, która używa powiązania modelu dla operacji na danych, wymaga jedynie niewielkiej ilości dodatkowego nakładu pracy.</span><span class="sxs-lookup"><span data-stu-id="d744e-121">Integrating the Datepicker widget into a page that uses model binding for data operations requires only a small amount of additional work.</span></span>

## <a name="add-a-new-property-to-the-model"></a><span data-ttu-id="d744e-122">Dodaj nową właściwość do modelu</span><span class="sxs-lookup"><span data-stu-id="d744e-122">Add a new property to the model</span></span>

<span data-ttu-id="d744e-123">Najpierw należy dodać właściwość **DateTime** do modelu studenta i zmigrować tę zmianę do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="d744e-123">First, you will add a **Datetime** property to your Student model and migrate that change to the database.</span></span> <span data-ttu-id="d744e-124">Otwórz **UniversityModels.cs**i Dodaj wyróżniony kod do modelu ucznia.</span><span class="sxs-lookup"><span data-stu-id="d744e-124">Open **UniversityModels.cs**, and add the highlighted code to the Student model.</span></span>

[!code-csharp[Main](integrating-jquery-ui/samples/sample1.cs?highlight=16-18)]

<span data-ttu-id="d744e-125">Element **RangeAttribute** jest dołączany do wymuszania reguł walidacji dla właściwości.</span><span class="sxs-lookup"><span data-stu-id="d744e-125">The **RangeAttribute** is included to enforce validation rules for the property.</span></span> <span data-ttu-id="d744e-126">W tym samouczku przyjęto założenie, że firma Contoso University była oparta na 1 stycznia 2013 i dlatego wcześniejsze daty rejestracji są nieprawidłowe.</span><span class="sxs-lookup"><span data-stu-id="d744e-126">For this tutorial, we will assume that Contoso University was founded on January 1st, 2013 and therefore earlier enrollment dates are not valid.</span></span>

<span data-ttu-id="d744e-127">W oknie Zarządzanie pakietami Dodaj migrację, uruchamiając polecenie **Add-Migration AddEnrollmentDate**.</span><span class="sxs-lookup"><span data-stu-id="d744e-127">In the Package Management window, add a migration by running the command **add-migration AddEnrollmentDate**.</span></span> <span data-ttu-id="d744e-128">Zwróć uwagę, że kod migracji dodaje nową kolumnę datetime do tabeli student.</span><span class="sxs-lookup"><span data-stu-id="d744e-128">Notice that the migration code adds the new Datetime column to the Student table.</span></span> <span data-ttu-id="d744e-129">Aby dopasować wartość określoną w polu RangeAttribute, Dodaj wartość domyślną dla nowej kolumny, jak pokazano w wyróżnionym kodzie poniżej.</span><span class="sxs-lookup"><span data-stu-id="d744e-129">To match the value you specified in the RangeAttribute, add a default value for the new column, as shown in the highlighted code below.</span></span>

[!code-csharp[Main](integrating-jquery-ui/samples/sample2.cs?highlight=11)]

<span data-ttu-id="d744e-130">Zapisz zmiany w pliku migracji.</span><span class="sxs-lookup"><span data-stu-id="d744e-130">Save your change to the migration file.</span></span>

<span data-ttu-id="d744e-131">Nie trzeba ponownie wypełniać danych.</span><span class="sxs-lookup"><span data-stu-id="d744e-131">You do not need to seed the data again.</span></span> <span data-ttu-id="d744e-132">W związku z tym Otwórz **Configuration.cs** w folderze migrations i Usuń lub Skomentuj kod w metodzie **inicjatora** .</span><span class="sxs-lookup"><span data-stu-id="d744e-132">Therefore, open **Configuration.cs** in the Migrations folder and remove or comment out the code in the **Seed** method.</span></span> <span data-ttu-id="d744e-133">Zapisz i zamknij plik.</span><span class="sxs-lookup"><span data-stu-id="d744e-133">Save and close the file.</span></span>

<span data-ttu-id="d744e-134">Teraz uruchom polecenie **Update-Database**.</span><span class="sxs-lookup"><span data-stu-id="d744e-134">Now, run the command **update-database**.</span></span> <span data-ttu-id="d744e-135">Zwróć uwagę, że kolumna już istnieje w bazie danych, a wszystkie istniejące rekordy mają wartość domyślną dla EnrollmentDate.</span><span class="sxs-lookup"><span data-stu-id="d744e-135">Notice that the column now exists in the database and all of the existing records have the default value for EnrollmentDate.</span></span>

## <a name="add-dynamic-controls-for-enrollment-date"></a><span data-ttu-id="d744e-136">Dodaj kontrolki dynamiczne dla daty rejestracji</span><span class="sxs-lookup"><span data-stu-id="d744e-136">Add dynamic controls for enrollment date</span></span>

<span data-ttu-id="d744e-137">Teraz dodasz kontrolki do wyświetlania i edytowania daty rejestracji.</span><span class="sxs-lookup"><span data-stu-id="d744e-137">You will now add controls for displaying and editing the enrollment date.</span></span> <span data-ttu-id="d744e-138">W tym momencie wartość jest edytowana za pomocą pola tekstowego.</span><span class="sxs-lookup"><span data-stu-id="d744e-138">At this point, the value is edited through a text box.</span></span> <span data-ttu-id="d744e-139">W dalszej części tego samouczka zmienisz pole tekstowe do widżetu JQuery DatePicker.</span><span class="sxs-lookup"><span data-stu-id="d744e-139">Later in the tutorial, you will change the text box to the JQuery Datepicker widget.</span></span>

<span data-ttu-id="d744e-140">Najpierw należy zauważyć, że nie trzeba wprowadzać zmian w pliku **addstudent. aspx** .</span><span class="sxs-lookup"><span data-stu-id="d744e-140">First, it is important to note that you do not need to make any change to the **AddStudent.aspx** file.</span></span> <span data-ttu-id="d744e-141">Kontrolka DynamicControl automatycznie renderuje nową właściwość.</span><span class="sxs-lookup"><span data-stu-id="d744e-141">The DynamicEntity control will automatically render the new property.</span></span>

<span data-ttu-id="d744e-142">Otwórz **uczniów. aspx**i Dodaj następujący wyróżniony kod.</span><span class="sxs-lookup"><span data-stu-id="d744e-142">Open **Students.aspx**, and add the following highlighted code.</span></span>

[!code-aspx[Main](integrating-jquery-ui/samples/sample3.aspx?highlight=13)]

<span data-ttu-id="d744e-143">Uruchom aplikację i zwróć uwagę, że wartość daty rejestracji można ustawić, wpisując datę.</span><span class="sxs-lookup"><span data-stu-id="d744e-143">Run the application and notice that you can set the value of the enrollment date by typing a date.</span></span> <span data-ttu-id="d744e-144">Podczas dodawania nowego ucznia:</span><span class="sxs-lookup"><span data-stu-id="d744e-144">When adding a new student:</span></span>

![Ustaw datę](integrating-jquery-ui/_static/image1.png)

<span data-ttu-id="d744e-146">Lub, edytując istniejącą wartość:</span><span class="sxs-lookup"><span data-stu-id="d744e-146">Or, editing an existing value:</span></span>

![Edytuj datę](integrating-jquery-ui/_static/image2.png)

<span data-ttu-id="d744e-148">Wpisywanie daty działa, ale może to nie być środowisko klienta, które chcesz podać.</span><span class="sxs-lookup"><span data-stu-id="d744e-148">Typing the date works, but it might not be the customer experience you wish to provide.</span></span> <span data-ttu-id="d744e-149">W następnej sekcji zostanie włączone wybieranie daty przez kalendarz.</span><span class="sxs-lookup"><span data-stu-id="d744e-149">In the next section, you will enable selecting a date through a calendar.</span></span>

## <a name="install-nuget-package-to-work-with-jquery-ui"></a><span data-ttu-id="d744e-150">Zainstaluj pakiet NuGet do pracy z interfejsem użytkownika JQuery</span><span class="sxs-lookup"><span data-stu-id="d744e-150">Install NuGet package to work with JQuery UI</span></span>

<span data-ttu-id="d744e-151">Pakiet NuGet **interfejsu użytkownika soku** umożliwia łatwą integrację WIDŻETÓW interfejsu użytkownika jQuery z aplikacją sieci Web.</span><span class="sxs-lookup"><span data-stu-id="d744e-151">The **Juice UI** NuGet package enables easy integration of the JQuery UI widgets into your web application.</span></span> <span data-ttu-id="d744e-152">Aby użyć tego pakietu, zainstaluj go za pomocą narzędzia NuGet.</span><span class="sxs-lookup"><span data-stu-id="d744e-152">To use this package, install it through NuGet.</span></span>

![Dodaj interfejs użytkownika soku](integrating-jquery-ui/_static/image3.png)

<span data-ttu-id="d744e-154">Instalowana wersja interfejsu użytkownika soku może powodować konflikt z wersją JQuery w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d744e-154">The version of Juice UI that you install may conflict with the version of JQuery in your application.</span></span> <span data-ttu-id="d744e-155">Przed kontynuowaniem pracy z tym samouczkiem spróbuj uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="d744e-155">Before proceeding with this tutorial, try running your application.</span></span> <span data-ttu-id="d744e-156">Jeśli wystąpi błąd języka JavaScript, należy uzgodnić wersję JQuery.</span><span class="sxs-lookup"><span data-stu-id="d744e-156">If you encounter a JavaScript error, you need to reconcile the JQuery version.</span></span> <span data-ttu-id="d744e-157">Możesz dodać oczekiwaną wersję platformy JQuery do folderu Scripts (wersja 1.8.2 w czasie pisania tego samouczka) lub w obszarze site. Master określ ścieżkę do pliku JQuery.</span><span class="sxs-lookup"><span data-stu-id="d744e-157">You can either add the expected version of JQuery to your Scripts folder (version 1.8.2 at time of writing this tutorial), or in Site.master specify the path to the JQuery file.</span></span>

[!code-aspx[Main](integrating-jquery-ui/samples/sample4.aspx)]

## <a name="customize-datetime-template-to-include-datepicker-widget"></a><span data-ttu-id="d744e-158">Dostosuj szablon DateTime w celu uwzględnienia widżetu DatePicker</span><span class="sxs-lookup"><span data-stu-id="d744e-158">Customize DateTime template to include Datepicker widget</span></span>

<span data-ttu-id="d744e-159">Do edycji wartości DateTime zostanie dodany widżet DatePicker do szablonu danych dynamicznych.</span><span class="sxs-lookup"><span data-stu-id="d744e-159">You will add the Datepicker widget to the dynamic data template for editing a datetime value.</span></span> <span data-ttu-id="d744e-160">Przez dodanie widżetu do szablonu, jest on automatycznie renderowany w formularzu dodawania nowego ucznia i w widoku siatki do edytowania uczniów.</span><span class="sxs-lookup"><span data-stu-id="d744e-160">By adding the widget to the template, it is automatically rendered in both the form for adding a new student and in the grid view for editing students.</span></span> <span data-ttu-id="d744e-161">Otwórz **element DateTime\_Edit. ascx**i Dodaj następujący wyróżniony kod.</span><span class="sxs-lookup"><span data-stu-id="d744e-161">Open **DateTime\_Edit.ascx**, and add the following highlighted code.</span></span>

[!code-aspx[Main](integrating-jquery-ui/samples/sample5.aspx?highlight=3)]

<span data-ttu-id="d744e-162">W pliku związanym z kodem należy określić minimalną i maksymalną datę dla DatePicker.</span><span class="sxs-lookup"><span data-stu-id="d744e-162">In the code-behind file, you will set the minimum and maximum dates for the DatePicker.</span></span> <span data-ttu-id="d744e-163">Ustawienie tych wartości uniemożliwi użytkownikom przechodzenie do nieprawidłowych dat.</span><span class="sxs-lookup"><span data-stu-id="d744e-163">By setting these values, you will prevent users from navigating to invalid dates.</span></span> <span data-ttu-id="d744e-164">Wartości minimalne i maksymalne zostaną pobrane z **klasy RangeAttribute** we właściwości DateTime, jeśli została podana.</span><span class="sxs-lookup"><span data-stu-id="d744e-164">You will retrieve the minimum and maximum values from the **RangeAttribute** on the DateTime property, if one is provided.</span></span> <span data-ttu-id="d744e-165">Otwórz **element DateTime\_Edit.ascx.cs**i Dodaj następujący wyróżniony kod do strony\_metody ładowania.</span><span class="sxs-lookup"><span data-stu-id="d744e-165">Open **DateTime\_Edit.ascx.cs**, and add the following highlighted code to the Page\_Load method.</span></span>

[!code-csharp[Main](integrating-jquery-ui/samples/sample6.cs?highlight=9-14)]

<span data-ttu-id="d744e-166">Uruchom aplikację sieci Web i przejdź do strony addstudent.</span><span class="sxs-lookup"><span data-stu-id="d744e-166">Run the web application and navigate to the AddStudent page.</span></span> <span data-ttu-id="d744e-167">Podaj wartości pól i Zauważ, że po kliknięciu pola tekstowego dla daty rejestracji zostanie wyświetlony kalendarz.</span><span class="sxs-lookup"><span data-stu-id="d744e-167">Provide values for the fields and notice that when you click on the text box for Enrollment Date, the calendar is displayed.</span></span>

![wybór daty](integrating-jquery-ui/_static/image4.png)

<span data-ttu-id="d744e-169">Wybierz datę, a następnie kliknij pozycję **Wstaw**.</span><span class="sxs-lookup"><span data-stu-id="d744e-169">Pick a date, and click **Insert**.</span></span> <span data-ttu-id="d744e-170">Element RangeAttribute wymusza weryfikację na serwerze.</span><span class="sxs-lookup"><span data-stu-id="d744e-170">The RangeAttribute enforces validation on the server.</span></span> <span data-ttu-id="d744e-171">Ustawiając właściwość minDate w DatePicker, należy również zastosować weryfikację na kliencie.</span><span class="sxs-lookup"><span data-stu-id="d744e-171">By setting the minDate property on the Datepicker, you also apply validation on the client.</span></span> <span data-ttu-id="d744e-172">Kalendarz nie zezwala użytkownikowi na przechodzenie do daty przed wartością minDate.</span><span class="sxs-lookup"><span data-stu-id="d744e-172">The calendar does not let the user navigate to a date prior to the value of minDate.</span></span>

<span data-ttu-id="d744e-173">Podczas edytowania rekordu w widoku siatki zostanie również wyświetlony kalendarz.</span><span class="sxs-lookup"><span data-stu-id="d744e-173">When you edit a record in the grid view, the calendar is also displayed.</span></span>

![DatePicker w GridView](integrating-jquery-ui/_static/image5.png)

## <a name="conclusion"></a><span data-ttu-id="d744e-175">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="d744e-175">Conclusion</span></span>

<span data-ttu-id="d744e-176">W tym samouczku dowiesz się, jak dołączyć widżet JQuery do formularza sieci Web, który używa powiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="d744e-176">In this tutorial, you learned how to incorporate a JQuery widget into a web form that uses model binding.</span></span>

<span data-ttu-id="d744e-177">W następnym [samouczku](using-query-string-values-to-retrieve-data.md)podczas wybierania danych zostanie użyta wartość ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="d744e-177">In the next [tutorial](using-query-string-values-to-retrieve-data.md), you will use a query string value when selecting data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d744e-178">[Poprzednie](sorting-paging-and-filtering-data.md)
> [dalej](using-query-string-values-to-retrieve-data.md)</span><span class="sxs-lookup"><span data-stu-id="d744e-178">[Previous](sorting-paging-and-filtering-data.md)
[Next](using-query-string-values-to-retrieve-data.md)</span></span>

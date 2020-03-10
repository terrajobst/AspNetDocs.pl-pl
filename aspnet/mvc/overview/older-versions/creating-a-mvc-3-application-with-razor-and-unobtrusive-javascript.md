---
uid: mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
title: Tworzenie aplikacji MVC 3 z użyciem Razor i niewygodnego języka JavaScript | Microsoft Docs
author: microsoft
description: Przykładowa aplikacja internetowa z listą użytkowników pokazuje, jak prosta służy do tworzenia aplikacji ASP.NET MVC 3 przy użyciu aparatu widoku Razor. Przykładowa aplikacja s...
ms.author: riande
ms.date: 11/01/2010
ms.assetid: 658b149b-d770-46bf-8b4b-4e47cca242f3
msc.legacyurl: /mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
msc.type: authoredcontent
ms.openlocfilehash: fb63493ff22c9261fc5746a998a32f2511141f87
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78540986"
---
# <a name="creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript"></a><span data-ttu-id="d417f-104">Tworzenie aplikacji MVC 3 ze składnią Razor i dyskretnym kodem JavaScript</span><span class="sxs-lookup"><span data-stu-id="d417f-104">Creating a MVC 3 Application with Razor and Unobtrusive JavaScript</span></span>

<span data-ttu-id="d417f-105">przez [firmę Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="d417f-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="d417f-106">Przykładowa aplikacja internetowa z listą użytkowników pokazuje, jak prosta służy do tworzenia aplikacji ASP.NET MVC 3 przy użyciu aparatu widoku Razor.</span><span class="sxs-lookup"><span data-stu-id="d417f-106">The User List sample web application demonstrates how simple it is to create ASP.NET MVC 3 applications using the Razor view engine.</span></span> <span data-ttu-id="d417f-107">Przykładowa aplikacja pokazuje, jak używać nowego aparatu widoku Razor z ASP.NET MVC w wersji 3 i Visual Studio 2010 w celu utworzenia fikcyjnej witryny sieci Web z listą użytkowników, która obejmuje funkcje, takie jak tworzenie, wyświetlanie, edytowanie i usuwanie użytkowników.</span><span class="sxs-lookup"><span data-stu-id="d417f-107">The sample application shows how to use the new Razor view engine with ASP.NET MVC version 3 and Visual Studio 2010 to create a fictional User List website that includes functionality such as creating, displaying, editing, and deleting users.</span></span>
> 
> <span data-ttu-id="d417f-108">W tym samouczku opisano kroki, które zostały wykonane w celu skompilowania przykładowej aplikacji ASP.NET MVC 3.</span><span class="sxs-lookup"><span data-stu-id="d417f-108">This tutorial describes the steps that were taken in order to build the User List sample ASP.NET MVC 3 application.</span></span> <span data-ttu-id="d417f-109">Projekt programu Visual Studio z C# kodem źródłowym i VB można dołączyć do tego tematu: [Pobierz](https://code.msdn.microsoft.com/aspnetmvcsamples/Release/ProjectReleases.aspx?ReleaseId=5114).</span><span class="sxs-lookup"><span data-stu-id="d417f-109">A Visual Studio project with C# and VB source code is available to accompany this topic: [Download](https://code.msdn.microsoft.com/aspnetmvcsamples/Release/ProjectReleases.aspx?ReleaseId=5114).</span></span> <span data-ttu-id="d417f-110">Jeśli masz pytania dotyczące tego samouczka, Opublikuj je na [forum MVC](https://forums.asp.net/1146.aspx).</span><span class="sxs-lookup"><span data-stu-id="d417f-110">If you have questions about this tutorial, please post them to the [MVC forum](https://forums.asp.net/1146.aspx).</span></span>

## <a name="overview"></a><span data-ttu-id="d417f-111">Omówienie</span><span class="sxs-lookup"><span data-stu-id="d417f-111">Overview</span></span>

<span data-ttu-id="d417f-112">Aplikacja, którą tworzysz, jest prostą witryną z listą użytkowników.</span><span class="sxs-lookup"><span data-stu-id="d417f-112">The application you'll be building is a simple user list website.</span></span> <span data-ttu-id="d417f-113">Użytkownicy mogą wprowadzać, wyświetlać i aktualizować informacje o użytkowniku.</span><span class="sxs-lookup"><span data-stu-id="d417f-113">Users can enter, view, and update user information.</span></span>

![Witryna Przykładowa](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image1.png)

<span data-ttu-id="d417f-115">W [tym miejscu](https://code.msdn.microsoft.com/Creating-a-MVC-3-28883c0f)możesz pobrać projekt C# VB i zakończony.</span><span class="sxs-lookup"><span data-stu-id="d417f-115">You can download the VB and C# completed project [here](https://code.msdn.microsoft.com/Creating-a-MVC-3-28883c0f).</span></span>

## <a name="creating-the-web-application"></a><span data-ttu-id="d417f-116">Tworzenie aplikacji sieci Web</span><span class="sxs-lookup"><span data-stu-id="d417f-116">Creating the Web Application</span></span>

<span data-ttu-id="d417f-117">Aby rozpocząć pracę z samouczkiem, Otwórz program Visual Studio 2010 i Utwórz nowy projekt za pomocą szablonu *aplikacji sieci Web ASP.NET MVC 3* .</span><span class="sxs-lookup"><span data-stu-id="d417f-117">To start the tutorial, open Visual Studio 2010 and create a new project using the *ASP.NET MVC 3 Web Application* template.</span></span> <span data-ttu-id="d417f-118">Nadaj aplikacji nazwę &quot;Mvc3Razor&quot;.</span><span class="sxs-lookup"><span data-stu-id="d417f-118">Name the application &quot;Mvc3Razor&quot;.</span></span>

<span data-ttu-id="d417f-119">[![nowy projekt MVC 3](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image3.png)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="d417f-119">[![New MVC 3 project](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image3.png)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image2.png)</span></span>

<span data-ttu-id="d417f-120">W oknie dialogowym **Nowy projekt ASP.NET MVC 3** wybierz pozycję **aplikacja internetowa**, wybierz aparat widoku Razor, a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="d417f-120">In the **New ASP.NET MVC 3 Project** dialog, select **Internet Application**, select the Razor view engine, and then click **OK**.</span></span>

![Nowe okno dialogowe projektu ASP.NET MVC 3](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image4.png)

<span data-ttu-id="d417f-122">W tym samouczku nie będziesz używać dostawcy członkostwa ASP.NET, więc możesz usunąć wszystkie pliki skojarzone z logowaniem i członkostwem.</span><span class="sxs-lookup"><span data-stu-id="d417f-122">In this tutorial you will not be using the ASP.NET membership provider, so you can delete all the files associated with logon and membership.</span></span> <span data-ttu-id="d417f-123">W **Eksplorator rozwiązań**Usuń następujące pliki i katalogi:</span><span class="sxs-lookup"><span data-stu-id="d417f-123">In **Solution Explorer**, remove the following files and directories:</span></span>

- <span data-ttu-id="d417f-124">*Controllers\AccountController*</span><span class="sxs-lookup"><span data-stu-id="d417f-124">*Controllers\AccountController*</span></span>
- <span data-ttu-id="d417f-125">*Models\AccountModels*</span><span class="sxs-lookup"><span data-stu-id="d417f-125">*Models\AccountModels*</span></span>
- <span data-ttu-id="d417f-126">*Views\Shared\\_LogOnPartial*</span><span class="sxs-lookup"><span data-stu-id="d417f-126">*Views\Shared\\_LogOnPartial*</span></span>
- <span data-ttu-id="d417f-127">*Views\Account* (i wszystkie pliki w tym katalogu)</span><span class="sxs-lookup"><span data-stu-id="d417f-127">*Views\Account* (and all the files in this directory)</span></span>

![Soln](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image5.png)

<span data-ttu-id="d417f-129">Edytuj plik <em>\_Layout. cshtml</em> i Zastąp znacznik wewnątrz elementu `<div>` o nazwie `logindisplay` komunikatem <em>&quot;</em>logowanie wyłączone&quot;.</span><span class="sxs-lookup"><span data-stu-id="d417f-129">Edit the <em>\_Layout.cshtml</em> file and replace the markup inside the `<div>` element named `logindisplay` with the message <em>&quot;</em>Login Disabled&quot;.</span></span> <span data-ttu-id="d417f-130">W poniższym przykładzie przedstawiono nowe oznakowanie:</span><span class="sxs-lookup"><span data-stu-id="d417f-130">The following example shows the new markup:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample1.cshtml)]

## <a name="adding-the-model"></a><span data-ttu-id="d417f-131">Dodawanie modelu</span><span class="sxs-lookup"><span data-stu-id="d417f-131">Adding the Model</span></span>

<span data-ttu-id="d417f-132">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy folder *modele* , wybierz polecenie **Dodaj**, a następnie kliknij pozycję **Klasa**.</span><span class="sxs-lookup"><span data-stu-id="d417f-132">In **Solution Explorer**, right-click the *Models* folder, select **Add**, and then click **Class**.</span></span>

![Nowa Klasa MDL użytkownika](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image6.png)

<span data-ttu-id="d417f-134">Nadaj klasie nazwę `UserModel`.</span><span class="sxs-lookup"><span data-stu-id="d417f-134">Name the class `UserModel`.</span></span> <span data-ttu-id="d417f-135">Zastąp zawartość pliku *UserModel* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="d417f-135">Replace the contents of the *UserModel* file with the following code:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample2.cs)]

<span data-ttu-id="d417f-136">Klasa `UserModel` reprezentuje użytkowników.</span><span class="sxs-lookup"><span data-stu-id="d417f-136">The `UserModel` class represents users.</span></span> <span data-ttu-id="d417f-137">Każdy element członkowski klasy ma adnotację z [wymaganym](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) atrybutem z przestrzeni nazw [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) .</span><span class="sxs-lookup"><span data-stu-id="d417f-137">Each member of the class is annotated with the [Required](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) attribute from the [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) namespace.</span></span> <span data-ttu-id="d417f-138">Atrybuty w przestrzeni nazw [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) zapewniają automatyczne sprawdzanie poprawności klienta i serwera dla aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="d417f-138">The attributes in the [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) namespace provide automatic client- and server-side validation for web applications.</span></span>

<span data-ttu-id="d417f-139">Otwórz klasę `HomeController` i Dodaj dyrektywę `using`, aby można było uzyskać dostęp do klas `UserModel` i `Users`:</span><span class="sxs-lookup"><span data-stu-id="d417f-139">Open the `HomeController` class and add a `using` directive so that you can access the `UserModel` and `Users` classes:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample3.cs)]

<span data-ttu-id="d417f-140">Zaraz po deklaracji `HomeController` Dodaj następujący komentarz i odwołanie do klasy `Users`:</span><span class="sxs-lookup"><span data-stu-id="d417f-140">Just after the `HomeController` declaration, add the following comment and the reference to a `Users` class:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample4.cs)]

<span data-ttu-id="d417f-141">Klasa `Users` to uproszczony magazyn danych znajdujący się w pamięci, który będzie używany w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="d417f-141">The `Users` class is a simplified, in-memory data store that you'll use in this tutorial.</span></span> <span data-ttu-id="d417f-142">W prawdziwej aplikacji należy użyć bazy danych do przechowywania informacji o użytkownikach.</span><span class="sxs-lookup"><span data-stu-id="d417f-142">In a real application you would use a database to store user information.</span></span> <span data-ttu-id="d417f-143">W poniższym przykładzie przedstawiono kilka pierwszych wierszy pliku `HomeController`:</span><span class="sxs-lookup"><span data-stu-id="d417f-143">The first few lines of the `HomeController` file are shown in the following example:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample5.cs)]

<span data-ttu-id="d417f-144">Skompiluj aplikację, aby model użytkownika był dostępny dla Kreatora tworzenia szkieletów w następnym kroku.</span><span class="sxs-lookup"><span data-stu-id="d417f-144">Build the application so that the user model will be available to the scaffolding wizard in the next step.</span></span>

## <a name="creating-the-default-view"></a><span data-ttu-id="d417f-145">Tworzenie widoku domyślnego</span><span class="sxs-lookup"><span data-stu-id="d417f-145">Creating the Default View</span></span>

<span data-ttu-id="d417f-146">Następnym krokiem jest dodanie metody akcji i widoku w celu wyświetlenia użytkowników.</span><span class="sxs-lookup"><span data-stu-id="d417f-146">The next step is to add an action method and view to display the users.</span></span>

<span data-ttu-id="d417f-147">Usuń istniejący plik *Views\Home\Index* .</span><span class="sxs-lookup"><span data-stu-id="d417f-147">Delete the existing *Views\Home\Index* file.</span></span> <span data-ttu-id="d417f-148">Zostanie utworzony nowy plik *indeksu* w celu wyświetlenia użytkowników.</span><span class="sxs-lookup"><span data-stu-id="d417f-148">You will create a new *Index* file to display the users.</span></span>

<span data-ttu-id="d417f-149">W klasie `HomeController` Zastąp zawartość metody `Index` następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="d417f-149">In the `HomeController` class, replace the contents of the `Index` method with the following code:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample6.cs)]

<span data-ttu-id="d417f-150">Kliknij prawym przyciskiem myszy wewnątrz metody `Index`, a następnie kliknij polecenie **Dodaj widok**.</span><span class="sxs-lookup"><span data-stu-id="d417f-150">Right-click inside the `Index` method and then click **Add View**.</span></span>

![Dodaj widok](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image7.png)

<span data-ttu-id="d417f-152">Wybierz opcję **Utwórz widok z silną typem** .</span><span class="sxs-lookup"><span data-stu-id="d417f-152">Select the **Create a strongly-typed view** option.</span></span> <span data-ttu-id="d417f-153">W obszarze **Klasa danych widoku**wybierz pozycję **Mvc3Razor. models. UserModel**.</span><span class="sxs-lookup"><span data-stu-id="d417f-153">For **View data class**, select **Mvc3Razor.Models.UserModel**.</span></span> <span data-ttu-id="d417f-154">(Jeśli nie widzisz **Mvc3Razor. models. UserModel** w polu **klasy widoku** , należy skompilować projekt). Upewnij się, że aparat widoku jest ustawiony na **Razor**.</span><span class="sxs-lookup"><span data-stu-id="d417f-154">(If you don't see **Mvc3Razor.Models.UserModel** in the **View data class** box, you need to build the project.) Make sure that the view engine is set to **Razor**.</span></span> <span data-ttu-id="d417f-155">Ustaw wartość **Wyświetl zawartość** na **listę** , a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="d417f-155">Set **View content** to **List** and then click **Add**.</span></span>

![Dodaj widok indeksu](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image8.png)

<span data-ttu-id="d417f-157">Nowy widok automatycznie szkieletuje dane użytkownika, które są przesyłane do widoku `Index`.</span><span class="sxs-lookup"><span data-stu-id="d417f-157">The new view automatically scaffolds the user data that's passed to the `Index` view.</span></span> <span data-ttu-id="d417f-158">Przejrzyj nowo wygenerowany plik *Views\Home\Index* .</span><span class="sxs-lookup"><span data-stu-id="d417f-158">Examine the newly generated *Views\Home\Index* file.</span></span> <span data-ttu-id="d417f-159">Linki **Utwórz nowe**, **Edytuj**, **szczegóły**i **Usuń** nie działają, ale pozostała część strony jest funkcjonalna.</span><span class="sxs-lookup"><span data-stu-id="d417f-159">The **Create New**, **Edit**, **Details**, and **Delete** links don't work, but the rest of the page is functional.</span></span> <span data-ttu-id="d417f-160">Uruchom stronę.</span><span class="sxs-lookup"><span data-stu-id="d417f-160">Run the page.</span></span> <span data-ttu-id="d417f-161">Zostanie wyświetlona lista użytkowników.</span><span class="sxs-lookup"><span data-stu-id="d417f-161">You see a list of users.</span></span>

![Strona indeksu](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image9.png)

<span data-ttu-id="d417f-163">Otwórz plik *index. cshtml* i zastąp `ActionLink` znacznikami do **edycji**, **szczegółów**i **usuwania** przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="d417f-163">Open the *Index.cshtml* file and replace the `ActionLink` markup for **Edit**, **Details**, and **Delete** with the following code:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample7.cshtml)]

<span data-ttu-id="d417f-164">Nazwa użytkownika jest używana jako identyfikator, aby znaleźć wybrany rekord w łączach **Edytuj**, **szczegóły**i **Usuń** .</span><span class="sxs-lookup"><span data-stu-id="d417f-164">The user name is used as the ID to find the selected record in the **Edit**, **Details**, and **Delete** links.</span></span>

## <a name="creating-the-details-view"></a><span data-ttu-id="d417f-165">Tworzenie widoku szczegółów</span><span class="sxs-lookup"><span data-stu-id="d417f-165">Creating the Details View</span></span>

<span data-ttu-id="d417f-166">Następnym krokiem jest dodanie `Details` akcji i widoku w celu wyświetlenia szczegółów użytkownika.</span><span class="sxs-lookup"><span data-stu-id="d417f-166">The next step is to add a `Details` action method and view in order to display user details.</span></span>

![Szczegóły](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image10.png)

<span data-ttu-id="d417f-168">Dodaj następującą metodę `Details` do kontrolera macierzystego:</span><span class="sxs-lookup"><span data-stu-id="d417f-168">Add the following `Details` method to the home controller:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample8.cs)]

<span data-ttu-id="d417f-169">Kliknij prawym przyciskiem myszy wewnątrz metody `Details` a następnie wybierz polecenie <strong>Dodaj widok</strong>.</span><span class="sxs-lookup"><span data-stu-id="d417f-169">Right-click inside the `Details` method and then select <strong>Add View</strong>.</span></span> <span data-ttu-id="d417f-170">Sprawdź, czy pole <strong>Klasa widoku</strong> zawiera <strong>Mvc3Razor. models. UserModel</strong><em>.</em></span><span class="sxs-lookup"><span data-stu-id="d417f-170">Verify that the <strong>View data class</strong> box contains <strong>Mvc3Razor.Models.UserModel</strong><em>.</em></span></span> <span data-ttu-id="d417f-171">Ustaw wartość <strong>Wyświetl zawartość</strong> na <strong>szczegóły</strong> , a następnie kliknij przycisk <strong>Dodaj</strong>.</span><span class="sxs-lookup"><span data-stu-id="d417f-171">Set <strong>View content</strong> to <strong>Details</strong> and then click <strong>Add</strong>.</span></span>

![Dodaj widok szczegółów](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image11.png)

<span data-ttu-id="d417f-173">Uruchom aplikację i wybierz łącze Szczegóły.</span><span class="sxs-lookup"><span data-stu-id="d417f-173">Run the application and select a details link.</span></span> <span data-ttu-id="d417f-174">Funkcja automatycznego tworzenia szkieletów pokazuje każdą właściwość w modelu.</span><span class="sxs-lookup"><span data-stu-id="d417f-174">The automatic scaffolding shows each property in the model.</span></span>

![Szczegóły](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image12.png)

## <a name="creating-the-edit-view"></a><span data-ttu-id="d417f-176">Tworzenie widoku edycji</span><span class="sxs-lookup"><span data-stu-id="d417f-176">Creating the Edit View</span></span>

<span data-ttu-id="d417f-177">Dodaj następującą metodę `Edit` do kontrolera macierzystego.</span><span class="sxs-lookup"><span data-stu-id="d417f-177">Add the following `Edit` method to the home controller.</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample9.cs)]

<span data-ttu-id="d417f-178">Dodaj widok jak w poprzednich krokach, ale ustaw **zawartość widoku** do **edycji**.</span><span class="sxs-lookup"><span data-stu-id="d417f-178">Add a view as in the previous steps, but set **View content** to **Edit**.</span></span>

![Dodaj widok edycji](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image13.png)

<span data-ttu-id="d417f-180">Uruchom aplikację i edytuj pierwszą i nazwisko jednego z użytkowników.</span><span class="sxs-lookup"><span data-stu-id="d417f-180">Run the application and edit the first and last name of one of the users.</span></span> <span data-ttu-id="d417f-181">W przypadku naruszenia wszelkich `DataAnnotation` ograniczeń, które zostały zastosowane do klasy `UserModel`, podczas przesyłania formularza zostaną wyświetlone błędy sprawdzania poprawności, które są generowane przez kod serwera.</span><span class="sxs-lookup"><span data-stu-id="d417f-181">If you violate any `DataAnnotation` constraints that have been applied to the `UserModel` class, when you submit the form, you will see validation errors that are produced by server code.</span></span> <span data-ttu-id="d417f-182">Na przykład jeśli zmienisz imię &quot;Ann&quot; na &quot;&quot;, podczas przesyłania formularza zostanie wyświetlony następujący błąd:</span><span class="sxs-lookup"><span data-stu-id="d417f-182">For example, if you change the first name &quot;Ann&quot; to &quot;A&quot;, when you submit the form, the following error is displayed on the form:</span></span>

`The field First Name must be a string with a minimum length of 3 and a maximum length of 8.`

<span data-ttu-id="d417f-183">W tym samouczku potraktowano nazwę użytkownika jako klucz podstawowy.</span><span class="sxs-lookup"><span data-stu-id="d417f-183">In this tutorial, you're treating the user name as the primary key.</span></span> <span data-ttu-id="d417f-184">W związku z tym nie można zmienić właściwości Nazwa użytkownika.</span><span class="sxs-lookup"><span data-stu-id="d417f-184">Therefore, the user name property cannot be changed.</span></span> <span data-ttu-id="d417f-185">W pliku *Edit. cshtml* , tuż po instrukcji `Html.BeginForm`, ustaw wartość pola Nazwa użytkownika na ukryte.</span><span class="sxs-lookup"><span data-stu-id="d417f-185">In the *Edit.cshtml* file, just after the `Html.BeginForm` statement, set the user name to be a hidden field.</span></span> <span data-ttu-id="d417f-186">Powoduje to, że właściwość zostanie przekazana w modelu.</span><span class="sxs-lookup"><span data-stu-id="d417f-186">This causes the property to be passed in the model.</span></span> <span data-ttu-id="d417f-187">Poniższy fragment kodu przedstawia położenie instrukcji `Hidden`:</span><span class="sxs-lookup"><span data-stu-id="d417f-187">The following code fragment shows the placement of the `Hidden` statement:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample10.cshtml)]

<span data-ttu-id="d417f-188">Zastąp `TextBoxFor` i `ValidationMessageFor` znaczników dla nazwy użytkownika z wywołaniem `DisplayFor`.</span><span class="sxs-lookup"><span data-stu-id="d417f-188">Replace the `TextBoxFor` and `ValidationMessageFor` markup for the user name with a `DisplayFor` call.</span></span> <span data-ttu-id="d417f-189">Metoda `DisplayFor` wyświetla właściwość jako element tylko do odczytu.</span><span class="sxs-lookup"><span data-stu-id="d417f-189">The `DisplayFor` method displays the property as a read-only element.</span></span> <span data-ttu-id="d417f-190">W poniższym przykładzie pokazano ukończony znacznik.</span><span class="sxs-lookup"><span data-stu-id="d417f-190">The following example shows the completed markup.</span></span> <span data-ttu-id="d417f-191">Pierwotne wywołania `TextBoxFor` i `ValidationMessageFor` są oznaczone jako komentarze BEGIN-Comment i End-comment (`@* *@`).</span><span class="sxs-lookup"><span data-stu-id="d417f-191">The original `TextBoxFor` and `ValidationMessageFor` calls are commented out with the Razor begin-comment and end-comment characters (`@* *@`)</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample11.cshtml)]

## <a name="enabling-client-side-validation"></a><span data-ttu-id="d417f-192">Włączanie walidacji po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="d417f-192">Enabling Client-Side Validation</span></span>

<span data-ttu-id="d417f-193">Aby włączyć weryfikację po stronie klienta w ASP.NET MVC 3, należy ustawić dwie flagi i należy dołączyć trzy pliki JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d417f-193">To enable client-side validation in ASP.NET MVC 3, you must set two flags and you must include three JavaScript files.</span></span>

<span data-ttu-id="d417f-194">Otwórz plik *Web. config* aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d417f-194">Open the application's *Web.config* file.</span></span> <span data-ttu-id="d417f-195">Sprawdź, czy `that ClientValidationEnabled` i `UnobtrusiveJavaScriptEnabled` są ustawione na wartość true w ustawieniach aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d417f-195">Verify `that ClientValidationEnabled` and `UnobtrusiveJavaScriptEnabled` are set to true in the application settings.</span></span> <span data-ttu-id="d417f-196">Następujący fragment z głównego pliku *Web. config* zawiera poprawne ustawienia:</span><span class="sxs-lookup"><span data-stu-id="d417f-196">The following fragment from the root *Web.config* file shows the correct settings:</span></span>

[!code-xml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample12.xml)]

<span data-ttu-id="d417f-197">Ustawienie `UnobtrusiveJavaScriptEnabled` na true włącza niezauważalną technologię AJAX i niewygodną weryfikację klienta.</span><span class="sxs-lookup"><span data-stu-id="d417f-197">Setting `UnobtrusiveJavaScriptEnabled` to true enables unobtrusive Ajax and unobtrusive client validation.</span></span> <span data-ttu-id="d417f-198">W przypadku korzystania z niedyskretnej weryfikacji reguły sprawdzania poprawności są włączone do atrybutów HTML5.</span><span class="sxs-lookup"><span data-stu-id="d417f-198">When you use unobtrusive validation, the validation rules are turned into HTML5 attributes.</span></span> <span data-ttu-id="d417f-199">Nazwy atrybutów HTML5 mogą zawierać tylko małe litery, cyfry i łączniki.</span><span class="sxs-lookup"><span data-stu-id="d417f-199">HTML5 attribute names can consist of only lowercase letters, numbers, and dashes.</span></span>

<span data-ttu-id="d417f-200">Ustawienie `ClientValidationEnabled` na true powoduje włączenie walidacji po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="d417f-200">Setting `ClientValidationEnabled` to true enables client-side validation.</span></span> <span data-ttu-id="d417f-201">Ustawiając te klucze w pliku *Web. config* aplikacji, można włączyć sprawdzanie poprawności klienta i niewygodne środowisko JavaScript dla całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d417f-201">By setting these keys in the application *Web.config* file, you enable client validation and unobtrusive JavaScript for the entire application.</span></span> <span data-ttu-id="d417f-202">Możesz również włączyć lub wyłączyć te ustawienia w poszczególnych widokach lub w metodach kontrolera przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="d417f-202">You can also enable or disable these settings in individual views or in controller methods using the following code:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample13.cs)]

<span data-ttu-id="d417f-203">Musisz również dołączyć kilka plików JavaScript w renderowanym widoku.</span><span class="sxs-lookup"><span data-stu-id="d417f-203">You also need to include several JavaScript files in the rendered view.</span></span> <span data-ttu-id="d417f-204">Łatwy sposób dołączania kodu JavaScript we wszystkich widokach polega na dodaniu ich do pliku *Views\Shared\\_Layout. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="d417f-204">An easy way to include the JavaScript in all views is to add them to the *Views\Shared\\_Layout.cshtml* file.</span></span> <span data-ttu-id="d417f-205">Zastąp element `<head>` pliku *\_Layout. cshtml* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="d417f-205">Replace the `<head>` element of the *\_Layout.cshtml* file with the following code:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample14.cshtml)]

<span data-ttu-id="d417f-206">Pierwsze dwa skrypty jQuery są obsługiwane przez Microsoft Ajax Content Delivery Network (CDN).</span><span class="sxs-lookup"><span data-stu-id="d417f-206">The first two jQuery scripts are hosted by the Microsoft Ajax Content Delivery Network (CDN).</span></span> <span data-ttu-id="d417f-207">Korzystając z zalet usługi Microsoft Ajax CDN, możesz znacząco poprawić wydajność aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d417f-207">By taking advantage of the Microsoft Ajax CDN, you can significantly improve the first-hit performance of your applications.</span></span>

<span data-ttu-id="d417f-208">Uruchom aplikację i kliknij link edycji.</span><span class="sxs-lookup"><span data-stu-id="d417f-208">Run the application and click an edit link.</span></span> <span data-ttu-id="d417f-209">Wyświetl źródło strony w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="d417f-209">View the page's source in the browser.</span></span> <span data-ttu-id="d417f-210">Źródło przeglądarki pokazuje wiele atrybutów formularza `data-val` (na potrzeby sprawdzania poprawności danych).</span><span class="sxs-lookup"><span data-stu-id="d417f-210">The browser source shows many attributes of the form `data-val` (for data validation).</span></span> <span data-ttu-id="d417f-211">Gdy jest włączone sprawdzanie poprawności klienta i niezauważalny kod JavaScript, pola wejściowe z regułą walidacji klienta zawierają atrybut `data-val="true"`, aby wyzwolić niezauważalną weryfikację klienta.</span><span class="sxs-lookup"><span data-stu-id="d417f-211">When client validation and unobtrusive JavaScript is enabled, input fields with a client-validation rule contain the `data-val="true"` attribute to trigger unobtrusive client validation.</span></span> <span data-ttu-id="d417f-212">Na przykład pole `City` w modelu zostało dekoracyjne przy użyciu [wymaganego](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) atrybutu, co spowoduje, że w kodzie HTML pokazano w następującym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="d417f-212">For example, the `City` field in the model was decorated with the [Required](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) attribute, which results in the HTML shown in the following example:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample15.cshtml)]

<span data-ttu-id="d417f-213">Dla każdej reguły sprawdzania poprawności klienta dodawany jest atrybut, który ma `data-val-rulename="message"`formularza.</span><span class="sxs-lookup"><span data-stu-id="d417f-213">For each client-validation rule, an attribute is added that has the form `data-val-rulename="message"`.</span></span> <span data-ttu-id="d417f-214">Korzystając z przykładowego pola `City` pokazanego wcześniej, wymagana reguła sprawdzania poprawności klienta generuje atrybut `data-val-required` i komunikat &quot;pole Miasto jest wymagane&quot;.</span><span class="sxs-lookup"><span data-stu-id="d417f-214">Using the `City` field example shown earlier, the required client-validation rule generates the `data-val-required` attribute and the message &quot;The City field is required&quot;.</span></span> <span data-ttu-id="d417f-215">Uruchom aplikację, Edytuj jednego z użytkowników i usuń zaznaczenie pola `City`.</span><span class="sxs-lookup"><span data-stu-id="d417f-215">Run the application, edit one of the users, and clear the `City` field.</span></span> <span data-ttu-id="d417f-216">Po wybraniu karty z pola zobaczysz komunikat o błędzie weryfikacji po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="d417f-216">When you tab out of the field, you see a client-side validation error message.</span></span>

![Miasto jest wymagane](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image14.png)

<span data-ttu-id="d417f-218">Podobnie dla każdego parametru w regule sprawdzania poprawności klienta dodawany jest atrybut, który ma `data-val-rulename-paramname=paramvalue`.</span><span class="sxs-lookup"><span data-stu-id="d417f-218">Similarly, for each parameter in the client-validation rule, an attribute is added that has the form `data-val-rulename-paramname=paramvalue`.</span></span> <span data-ttu-id="d417f-219">Na przykład właściwość `FirstName` ma adnotację z atrybutem [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) i określa minimalną długość równą 3 i maksymalną 8.</span><span class="sxs-lookup"><span data-stu-id="d417f-219">For example, the `FirstName` property is annotated with the [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) attribute and specifies a minimum length of 3 and a maximum length of 8.</span></span> <span data-ttu-id="d417f-220">Reguła walidacji danych o nazwie `length` ma nazwę parametru `max` i wartość parametru 8.</span><span class="sxs-lookup"><span data-stu-id="d417f-220">The data validation rule named `length` has the parameter name `max` and the parameter value 8.</span></span> <span data-ttu-id="d417f-221">Poniżej przedstawiono kod HTML wygenerowany dla pola `FirstName` podczas edytowania jednego z użytkowników:</span><span class="sxs-lookup"><span data-stu-id="d417f-221">The following shows the HTML that is generated for the `FirstName` field when you edit one of the users:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample16.cshtml)]

<span data-ttu-id="d417f-222">Aby uzyskać więcej informacji o niezauważalnej weryfikacji klienta, zobacz wpis [niezauważalna Weryfikacja klienta w ASP.NET MVC 3](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) w blogu Brada Wilson.</span><span class="sxs-lookup"><span data-stu-id="d417f-222">For more information about unobtrusive client validation, see the entry [Unobtrusive Client Validation in ASP.NET MVC 3](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) in Brad Wilson's blog.</span></span>

> [!NOTE]
> <span data-ttu-id="d417f-223">W programie ASP.NET MVC 3 beta czasami trzeba przesłać formularz w celu uruchomienia walidacji po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="d417f-223">In ASP.NET MVC 3 Beta, you sometimes need to submit the form in order to start client-side validation.</span></span> <span data-ttu-id="d417f-224">Można to zmienić w przypadku ostatecznej wersji.</span><span class="sxs-lookup"><span data-stu-id="d417f-224">This might be changed for the final release.</span></span>

## <a name="creating-the-create-view"></a><span data-ttu-id="d417f-225">Tworzenie widoku</span><span class="sxs-lookup"><span data-stu-id="d417f-225">Creating the Create View</span></span>

<span data-ttu-id="d417f-226">Następnym krokiem jest dodanie `Create` akcji i widoku w celu umożliwienia użytkownikowi utworzenia nowego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="d417f-226">The next step is to add a `Create` action method and view in order to enable the user to create a new user.</span></span> <span data-ttu-id="d417f-227">Dodaj następującą metodę `Create` do kontrolera macierzystego:</span><span class="sxs-lookup"><span data-stu-id="d417f-227">Add the following `Create` method to the home controller:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample17.cs)]

<span data-ttu-id="d417f-228">Dodaj widok jak w poprzednich krokach, ale ustaw **zawartość widoku** do **utworzenia**.</span><span class="sxs-lookup"><span data-stu-id="d417f-228">Add a view as in the previous steps, but set **View content** to **Create**.</span></span>

![Utwórz widok](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image15.png)

<span data-ttu-id="d417f-230">Uruchom aplikację, wybierz link **Utwórz** i Dodaj nowego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="d417f-230">Run the application, select the **Create** link, and add a new user.</span></span> <span data-ttu-id="d417f-231">Metoda `Create` automatycznie korzysta z funkcji weryfikacji po stronie klienta i po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="d417f-231">The `Create` method automatically takes advantage of client-side and server-side validation.</span></span> <span data-ttu-id="d417f-232">Spróbuj wprowadzić nazwę użytkownika, która zawiera białe znaki, takie jak &quot;Ben X&quot;.</span><span class="sxs-lookup"><span data-stu-id="d417f-232">Try to enter a user name that contains white space, such as &quot;Ben X&quot;.</span></span> <span data-ttu-id="d417f-233">Po wybraniu karty z pola Nazwa użytkownika zostanie wyświetlony komunikat o błędzie weryfikacji po stronie klienta (`White space is not allowed`).</span><span class="sxs-lookup"><span data-stu-id="d417f-233">When you tab out of the user name field, a client-side validation error (`White space is not allowed`) is displayed.</span></span>

## <a name="add-the-delete-method"></a><span data-ttu-id="d417f-234">Dodaj metodę Delete</span><span class="sxs-lookup"><span data-stu-id="d417f-234">Add the Delete method</span></span>

<span data-ttu-id="d417f-235">Aby ukończyć ten samouczek, Dodaj następującą metodę `Delete` do kontrolera macierzystego:</span><span class="sxs-lookup"><span data-stu-id="d417f-235">To complete the tutorial, add the following `Delete` method to the home controller:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample18.cs)]

<span data-ttu-id="d417f-236">Dodaj widok `Delete` jak w poprzednich krokach ustawienia **Wyświetl zawartość** do **usunięcia**.</span><span class="sxs-lookup"><span data-stu-id="d417f-236">Add a `Delete` view as in the previous steps, setting **View content** to **Delete**.</span></span>

![Usuń widok](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image16.png)

<span data-ttu-id="d417f-238">Masz teraz prostą, ale w pełni funkcjonalną ASP.NET aplikację MVC 3 z walidacją.</span><span class="sxs-lookup"><span data-stu-id="d417f-238">You now have a simple but fully functional ASP.NET MVC 3 application with validation.</span></span>

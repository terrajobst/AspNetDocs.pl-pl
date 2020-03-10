---
uid: mvc/overview/getting-started/database-first-development/enhancing-data-validation
title: 'Samouczek: ulepszanie sprawdzania poprawności danych dla EF Database First z aplikacją ASP.NET MVC'
description: Ten samouczek koncentruje się na dodawaniu adnotacji do danych do modelu danych, aby określić wymagania dotyczące weryfikacji i formatowanie wyświetlania.
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: 0ed5e67a-34c0-4b57-84a6-802b0fb3cd00
msc.legacyurl: /mvc/overview/getting-started/database-first-development/enhancing-data-validation
msc.type: authoredcontent
ms.openlocfilehash: 897cd7c6a40445e2a4abede50d81e101372d3233
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78616278"
---
# <a name="tutorial-enhance-data-validation-for-ef-database-first-with-aspnet-mvc-app"></a><span data-ttu-id="0ab5b-103">Samouczek: ulepszanie sprawdzania poprawności danych dla EF Database First z aplikacją ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="0ab5b-103">Tutorial: Enhance data validation for EF Database First with ASP.NET MVC app</span></span>

<span data-ttu-id="0ab5b-104">Korzystając z szkieletów MVC, Entity Framework i ASP.NET, można utworzyć aplikację sieci Web, która udostępnia interfejs istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="0ab5b-104">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="0ab5b-105">W tej serii samouczków pokazano, jak automatycznie generować kod, który umożliwia użytkownikom wyświetlanie, edytowanie, tworzenie i usuwanie danych znajdujących się w tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="0ab5b-105">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="0ab5b-106">Wygenerowany kod odpowiada kolumnom w tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="0ab5b-106">The generated code corresponds to the columns in the database table.</span></span>

<span data-ttu-id="0ab5b-107">Ten samouczek koncentruje się na dodawaniu adnotacji do danych do modelu danych, aby określić wymagania dotyczące weryfikacji i formatowanie wyświetlania.</span><span class="sxs-lookup"><span data-stu-id="0ab5b-107">This tutorial focuses on adding data annotations to the data model to specify validation requirements and display formatting.</span></span> <span data-ttu-id="0ab5b-108">Ulepszono je na podstawie opinii użytkowników w sekcji komentarzy.</span><span class="sxs-lookup"><span data-stu-id="0ab5b-108">It was improved based on feedback from users in the comments section.</span></span>

<span data-ttu-id="0ab5b-109">W tym samouczku zostaną wykonane następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="0ab5b-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="0ab5b-110">Dodawanie adnotacji do danych</span><span class="sxs-lookup"><span data-stu-id="0ab5b-110">Add data annotations</span></span>
> * <span data-ttu-id="0ab5b-111">Dodaj klasy metadanych</span><span class="sxs-lookup"><span data-stu-id="0ab5b-111">Add metadata classes</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0ab5b-112">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="0ab5b-112">Prerequisites</span></span>

* [<span data-ttu-id="0ab5b-113">Dostosowywanie widoku</span><span class="sxs-lookup"><span data-stu-id="0ab5b-113">Customize a view</span></span>](customizing-a-view.md)

## <a name="add-data-annotations"></a><span data-ttu-id="0ab5b-114">Dodawanie adnotacji do danych</span><span class="sxs-lookup"><span data-stu-id="0ab5b-114">Add data annotations</span></span>

<span data-ttu-id="0ab5b-115">Zgodnie z wcześniejszym tematem niektóre reguły sprawdzania poprawności danych są automatycznie stosowane do danych wejściowych użytkownika.</span><span class="sxs-lookup"><span data-stu-id="0ab5b-115">As you saw in an earlier topic, some data validation rules are automatically applied to the user input.</span></span> <span data-ttu-id="0ab5b-116">Na przykład można podać tylko liczbę dla właściwości klasy.</span><span class="sxs-lookup"><span data-stu-id="0ab5b-116">For example, you can only provide a number for the Grade property.</span></span> <span data-ttu-id="0ab5b-117">Aby określić więcej reguł walidacji danych, można dodać adnotacje do danych klasy modelu.</span><span class="sxs-lookup"><span data-stu-id="0ab5b-117">To specify more data validation rules, you can add data annotations to your model class.</span></span> <span data-ttu-id="0ab5b-118">Adnotacje są stosowane w całej aplikacji sieci Web dla określonej właściwości.</span><span class="sxs-lookup"><span data-stu-id="0ab5b-118">These annotations are applied throughout your web application for the specified property.</span></span> <span data-ttu-id="0ab5b-119">Można również zastosować atrybuty formatowania, które zmieniają sposób wyświetlania właściwości. takie jak, zmiana wartości używanej w etykietach tekstowych.</span><span class="sxs-lookup"><span data-stu-id="0ab5b-119">You can also apply formatting attributes that change how the properties are displayed; such as, changing the value used for text labels.</span></span>

<span data-ttu-id="0ab5b-120">W tym samouczku dodasz adnotacje danych w celu ograniczenia długości wartości podanych dla właściwości FirstName, LastName i MiddleName.</span><span class="sxs-lookup"><span data-stu-id="0ab5b-120">In this tutorial, you will add data annotations to restrict the length of the values provided for the FirstName, LastName, and MiddleName properties.</span></span> <span data-ttu-id="0ab5b-121">W bazie danych te wartości są ograniczone do 50 znaków; Niemniej jednak w aplikacji sieci Web limit znaków nie jest wymuszany.</span><span class="sxs-lookup"><span data-stu-id="0ab5b-121">In the database, these values are limited to 50 characters; however, in your web application that character limit is currently not enforced.</span></span> <span data-ttu-id="0ab5b-122">Jeśli użytkownik poda więcej niż 50 znaków dla jednej z tych wartości, strona ulegnie awarii podczas próby zapisania wartości w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="0ab5b-122">If a user provides more than 50 characters for one of those values, the page will crash when attempting to save the value to the database.</span></span> <span data-ttu-id="0ab5b-123">Należy również ograniczyć zakres do wartości z zakresu od 0 do 4.</span><span class="sxs-lookup"><span data-stu-id="0ab5b-123">You will also restrict Grade to values between 0 and 4.</span></span>

<span data-ttu-id="0ab5b-124">Wybierz pozycję **modele** > **ContosoModel. edmx** > **ContosoModel.tt** i Otwórz plik *student.cs* .</span><span class="sxs-lookup"><span data-stu-id="0ab5b-124">Select **Models** > **ContosoModel.edmx** > **ContosoModel.tt** and open the *Student.cs* file.</span></span> <span data-ttu-id="0ab5b-125">Dodaj następujący wyróżniony kod do klasy.</span><span class="sxs-lookup"><span data-stu-id="0ab5b-125">Add the following highlighted code to the class.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample1.cs?highlight=5,15,17,20)]

<span data-ttu-id="0ab5b-126">Otwórz *Enrollment.cs* i Dodaj następujący wyróżniony kod.</span><span class="sxs-lookup"><span data-stu-id="0ab5b-126">Open *Enrollment.cs* and add the following highlighted code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample2.cs?highlight=5,10)]

<span data-ttu-id="0ab5b-127">Skompiluj rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="0ab5b-127">Build the solution.</span></span>

<span data-ttu-id="0ab5b-128">Kliknij pozycję **Lista uczniów** i wybierz pozycję **Edytuj**.</span><span class="sxs-lookup"><span data-stu-id="0ab5b-128">Click **List of students** and select **Edit**.</span></span> <span data-ttu-id="0ab5b-129">Jeśli spróbujesz wprowadzić więcej niż 50 znaków, zostanie wyświetlony komunikat o błędzie.</span><span class="sxs-lookup"><span data-stu-id="0ab5b-129">If you attempt to enter more than 50 characters, an error message is displayed.</span></span>

![Pokaż komunikat o błędzie](enhancing-data-validation/_static/image1.png)

<span data-ttu-id="0ab5b-131">Wróć do strony głównej.</span><span class="sxs-lookup"><span data-stu-id="0ab5b-131">Go back to the home page.</span></span> <span data-ttu-id="0ab5b-132">Kliknij pozycję **Lista rejestracji** i wybierz pozycję **Edytuj**.</span><span class="sxs-lookup"><span data-stu-id="0ab5b-132">Click **List of enrollments** and select **Edit**.</span></span> <span data-ttu-id="0ab5b-133">Próba dostarczenia klasy powyżej 4.</span><span class="sxs-lookup"><span data-stu-id="0ab5b-133">Attempt to provide a grade above 4.</span></span> <span data-ttu-id="0ab5b-134">Zostanie wyświetlony następujący błąd: *Klasa pola musi zawierać się w przedziale od 0 do 4.*</span><span class="sxs-lookup"><span data-stu-id="0ab5b-134">You will receive this error: *The field Grade must be between 0 and 4.*</span></span>

## <a name="add-metadata-classes"></a><span data-ttu-id="0ab5b-135">Dodaj klasy metadanych</span><span class="sxs-lookup"><span data-stu-id="0ab5b-135">Add metadata classes</span></span>

<span data-ttu-id="0ab5b-136">Dodawanie atrybutów walidacji bezpośrednio do klasy modelu działa, gdy nie oczekuje się, że baza danych nie zostanie zmieniona; Jeśli jednak baza danych ulegnie zmianie i trzeba będzie ponownie wygenerować klasę modelu, utracisz wszystkie atrybuty zastosowane do klasy modelu.</span><span class="sxs-lookup"><span data-stu-id="0ab5b-136">Adding the validation attributes directly to the model class works when you do not expect the database to change; however, if your database changes and you need to regenerate the model class, you will lose all of the attributes you had applied to the model class.</span></span> <span data-ttu-id="0ab5b-137">Takie podejście może być bardzo wydajne i podatne na utratę ważnych reguł sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="0ab5b-137">This approach can be very inefficient and prone to losing important validation rules.</span></span>

<span data-ttu-id="0ab5b-138">Aby uniknąć tego problemu, można dodać klasę metadanych, która zawiera atrybuty.</span><span class="sxs-lookup"><span data-stu-id="0ab5b-138">To avoid this problem, you can add a metadata class that contains the attributes.</span></span> <span data-ttu-id="0ab5b-139">Po skojarzeniu klasy modelu z klasą metadanych te atrybuty są stosowane do modelu.</span><span class="sxs-lookup"><span data-stu-id="0ab5b-139">When you associate the model class to the metadata class, those attributes are applied to the model.</span></span> <span data-ttu-id="0ab5b-140">W tym podejściu klasy modelu można ponownie wygenerować bez utraty wszystkich atrybutów, które zostały zastosowane do klasy metadanych.</span><span class="sxs-lookup"><span data-stu-id="0ab5b-140">In this approach, the model class can be regenerated without losing all of the attributes that have been applied to the metadata class.</span></span>

<span data-ttu-id="0ab5b-141">W folderze **modele** Dodaj klasę o nazwie *Metadata.cs*.</span><span class="sxs-lookup"><span data-stu-id="0ab5b-141">In the **Models** folder, add a class named *Metadata.cs*.</span></span>

<span data-ttu-id="0ab5b-142">Zastąp kod w *Metadata.cs* następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="0ab5b-142">Replace the code in *Metadata.cs* with the following code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample3.cs)]

<span data-ttu-id="0ab5b-143">Te klasy metadanych zawierają wszystkie atrybuty walidacji, które zostały wcześniej zastosowane do klas modelu.</span><span class="sxs-lookup"><span data-stu-id="0ab5b-143">These metadata classes contain all of the validation attributes that you had previously applied to the model classes.</span></span> <span data-ttu-id="0ab5b-144">Atrybut **Display** służy do zmiany wartości używanej w etykietach tekstowych.</span><span class="sxs-lookup"><span data-stu-id="0ab5b-144">The **Display** attribute is used to change the value used for text labels.</span></span>

<span data-ttu-id="0ab5b-145">Teraz należy skojarzyć klasy modelu z klasami metadanych.</span><span class="sxs-lookup"><span data-stu-id="0ab5b-145">Now, you must associate the model classes with the metadata classes.</span></span>

<span data-ttu-id="0ab5b-146">W folderze **modele** Dodaj klasę o nazwie *PartialClasses.cs*.</span><span class="sxs-lookup"><span data-stu-id="0ab5b-146">In the **Models** folder, add a class named *PartialClasses.cs*.</span></span>

<span data-ttu-id="0ab5b-147">Zastąp zawartość pliku następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="0ab5b-147">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample4.cs)]

<span data-ttu-id="0ab5b-148">Zauważ, że każda klasa jest oznaczona jako Klasa `partial`, a każda z nich jest zgodna z nazwą i przestrzenią nazw jako klasą, która jest generowana automatycznie.</span><span class="sxs-lookup"><span data-stu-id="0ab5b-148">Notice that each class is marked as a `partial` class, and each matches the name and namespace as the class that is automatically generated.</span></span> <span data-ttu-id="0ab5b-149">Stosując atrybut metadanych do klasy częściowej, upewnij się, że atrybuty walidacji danych zostaną zastosowane do klasy generowanej automatycznie.</span><span class="sxs-lookup"><span data-stu-id="0ab5b-149">By applying the metadata attribute to the partial class, you ensure that the data validation attributes will be applied to the automatically-generated class.</span></span> <span data-ttu-id="0ab5b-150">Te atrybuty nie zostaną utracone w przypadku ponownego wygenerowania klas modelu, ponieważ atrybut metadanych jest stosowany w klasach częściowych, które nie są ponownie generowane.</span><span class="sxs-lookup"><span data-stu-id="0ab5b-150">These attributes will not be lost when you regenerate the model classes because the metadata attribute is applied in partial classes that are not regenerated.</span></span>

<span data-ttu-id="0ab5b-151">Aby ponownie wygenerować klasy generowane automatycznie, Otwórz plik *ContosoModel. edmx* .</span><span class="sxs-lookup"><span data-stu-id="0ab5b-151">To regenerate the automatically-generated classes, open the *ContosoModel.edmx* file.</span></span> <span data-ttu-id="0ab5b-152">Ponownie kliknij prawym przyciskiem myszy powierzchnię projektu i wybierz polecenie **Aktualizuj model z bazy danych**.</span><span class="sxs-lookup"><span data-stu-id="0ab5b-152">Once again, right-click on the design surface and select **Update Model from Database**.</span></span> <span data-ttu-id="0ab5b-153">Mimo że baza danych nie została zmieniona, proces ten spowoduje ponowne wygenerowanie klas.</span><span class="sxs-lookup"><span data-stu-id="0ab5b-153">Even though you have not changed the database, this process will regenerate the classes.</span></span> <span data-ttu-id="0ab5b-154">Na karcie **Odśwież** wybierz pozycję **tabele** i **Zakończ**.</span><span class="sxs-lookup"><span data-stu-id="0ab5b-154">In the **Refresh** tab, select **Tables** and **Finish**.</span></span>

<span data-ttu-id="0ab5b-155">Zapisz plik *ContosoModel. edmx* , aby zastosować zmiany.</span><span class="sxs-lookup"><span data-stu-id="0ab5b-155">Save the *ContosoModel.edmx* file to apply the changes.</span></span>

<span data-ttu-id="0ab5b-156">Otwórz plik *student.cs* lub plik *Enrollment.cs* i Zauważ, że zastosowane wcześniej atrybuty sprawdzania poprawności danych nie są już w pliku.</span><span class="sxs-lookup"><span data-stu-id="0ab5b-156">Open the *Student.cs* file or the *Enrollment.cs* file, and notice that the data validation attributes you applied earlier are no longer in the file.</span></span> <span data-ttu-id="0ab5b-157">Jednak Uruchom aplikację i zwróć uwagę, że reguły walidacji są nadal stosowane podczas wprowadzania danych.</span><span class="sxs-lookup"><span data-stu-id="0ab5b-157">However, run the application, and notice that the validation rules are still applied when you enter data.</span></span>

## <a name="conclusion"></a><span data-ttu-id="0ab5b-158">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="0ab5b-158">Conclusion</span></span>

<span data-ttu-id="0ab5b-159">W tej serii przedstawiono prosty przykład generowania kodu z istniejącej bazy danych, który umożliwia użytkownikom edytowanie, aktualizowanie, tworzenie i usuwanie danych.</span><span class="sxs-lookup"><span data-stu-id="0ab5b-159">This series provided a simple example of how to generate code from an existing database that enables users to edit, update, create and delete data.</span></span> <span data-ttu-id="0ab5b-160">Do tworzenia projektu używane są ASP.NET MVC 5, Entity Framework i ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="0ab5b-160">It used ASP.NET MVC 5, Entity Framework and ASP.NET Scaffolding to create the project.</span></span> 

<span data-ttu-id="0ab5b-161">Przykładowy przykład tworzenia Code First można znaleźć [w temacie Wprowadzenie with ASP.NET MVC 5](../introduction/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="0ab5b-161">For an introductory example of Code First development, see [Getting Started with ASP.NET MVC 5](../introduction/getting-started.md).</span></span> 

<span data-ttu-id="0ab5b-162">Aby zapoznać się z bardziej zaawansowanym przykładem, zobacz [Tworzenie modelu danych Entity Framework dla aplikacji ASP.NET MVC 4](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="0ab5b-162">For a more advanced example, see [Creating an Entity Framework Data Model for an ASP.NET MVC 4 App](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span> <span data-ttu-id="0ab5b-163">Należy zauważyć, że interfejs API DbContext używany do pracy z danymi w Database First jest taki sam jak interfejs API służący do pracy z danymi w Code First.</span><span class="sxs-lookup"><span data-stu-id="0ab5b-163">Note that the DbContext API that you use for working with data in Database First is the same as the API you use for working with data in Code First.</span></span> <span data-ttu-id="0ab5b-164">Nawet jeśli zamierzasz używać Database First, możesz dowiedzieć się, jak obsługiwać bardziej złożone scenariusze, takie jak odczytywanie i aktualizowanie powiązanych danych, obsługa konfliktów współbieżności i tak dalej z samouczka Code First.</span><span class="sxs-lookup"><span data-stu-id="0ab5b-164">Even if you intend to use Database First, you can learn how to handle more complex scenarios such as reading and updating related data, handling concurrency conflicts, and so forth from a Code First tutorial.</span></span> <span data-ttu-id="0ab5b-165">Jedyną różnicą jest to, jak tworzona jest baza danych, Klasa kontekstowa i klasy jednostek.</span><span class="sxs-lookup"><span data-stu-id="0ab5b-165">The only difference is in how the database, context class, and entity classes are created.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0ab5b-166">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="0ab5b-166">Additional resources</span></span>

<span data-ttu-id="0ab5b-167">Aby uzyskać pełną listę adnotacji dotyczących walidacji danych, które można zastosować do właściwości i klas, zobacz [System. ComponentModel. DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx).</span><span class="sxs-lookup"><span data-stu-id="0ab5b-167">For a full list of data validation annotations you can apply to properties and classes, see [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx).</span></span>

## <a name="next-steps"></a><span data-ttu-id="0ab5b-168">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="0ab5b-168">Next steps</span></span>

<span data-ttu-id="0ab5b-169">W tym samouczku zostaną wykonane następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="0ab5b-169">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="0ab5b-170">Dodano adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="0ab5b-170">Added data annotations</span></span>
> * <span data-ttu-id="0ab5b-171">Dodano klasy metadanych</span><span class="sxs-lookup"><span data-stu-id="0ab5b-171">Added metadata classes</span></span>

<span data-ttu-id="0ab5b-172">Aby dowiedzieć się, jak wdrożyć aplikację internetową i bazę danych SQL na Azure App Service, zobacz ten samouczek:</span><span class="sxs-lookup"><span data-stu-id="0ab5b-172">To learn how to deploy a web app and SQL database to Azure App Service, see this tutorial:</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="0ab5b-173">Wdrażanie aplikacji .NET do Azure App Service</span><span class="sxs-lookup"><span data-stu-id="0ab5b-173">Deploy a .NET app to Azure App Service</span></span>](/azure/app-service/app-service-web-tutorial-dotnet-sqldatabase/)

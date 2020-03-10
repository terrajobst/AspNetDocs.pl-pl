---
uid: mvc/overview/getting-started/database-first-development/generating-views
title: 'Samouczek: Generowanie widoków dla Database First EF z aplikacją ASP.NET MVC'
description: Ten samouczek koncentruje się na używaniu szkieletu ASP.NET w celu wygenerowania kontrolerów i widoków.
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: 669367cf-8e30-4eb6-821d-10a7d9bb906c
msc.legacyurl: /mvc/overview/getting-started/database-first-development/generating-views
msc.type: authoredcontent
ms.openlocfilehash: e71e13e22d8a72e1699cfc70d4d93af603edba5b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78616208"
---
# <a name="tutorial-generate-views-for-ef-database-first-with-aspnet-mvc-app"></a><span data-ttu-id="58d17-103">Samouczek: Generowanie widoków dla Database First EF z aplikacją ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="58d17-103">Tutorial: Generate views for EF Database First with ASP.NET MVC app</span></span>

<span data-ttu-id="58d17-104">Korzystając z szkieletów MVC, Entity Framework i ASP.NET, można utworzyć aplikację sieci Web, która udostępnia interfejs istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="58d17-104">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="58d17-105">W tej serii samouczków pokazano, jak automatycznie generować kod, który umożliwia użytkownikom wyświetlanie, edytowanie, tworzenie i usuwanie danych znajdujących się w tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="58d17-105">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="58d17-106">Wygenerowany kod odpowiada kolumnom w tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="58d17-106">The generated code corresponds to the columns in the database table.</span></span>

<span data-ttu-id="58d17-107">Ten samouczek koncentruje się na używaniu szkieletu ASP.NET w celu wygenerowania kontrolerów i widoków.</span><span class="sxs-lookup"><span data-stu-id="58d17-107">This tutorial focuses on using ASP.NET Scaffolding to generate the controllers and views.</span></span>

<span data-ttu-id="58d17-108">W tym samouczku zostaną wykonane następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="58d17-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="58d17-109">Dodawanie szkieletu</span><span class="sxs-lookup"><span data-stu-id="58d17-109">Add scaffold</span></span>
> * <span data-ttu-id="58d17-110">Dodawanie linków do nowych widoków</span><span class="sxs-lookup"><span data-stu-id="58d17-110">Add links to new views</span></span>
> * <span data-ttu-id="58d17-111">Wyświetl widoki uczniów</span><span class="sxs-lookup"><span data-stu-id="58d17-111">Display student views</span></span>
> * <span data-ttu-id="58d17-112">Wyświetl widoki rejestracji</span><span class="sxs-lookup"><span data-stu-id="58d17-112">Display enrollment views</span></span>

## <a name="prerequisite"></a><span data-ttu-id="58d17-113">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="58d17-113">Prerequisite</span></span>

* [<span data-ttu-id="58d17-114">Tworzenie aplikacji sieci Web i modeli danych</span><span class="sxs-lookup"><span data-stu-id="58d17-114">Create the web application and data models</span></span>](creating-the-web-application.md)

## <a name="add-scaffold"></a><span data-ttu-id="58d17-115">Dodawanie szkieletu</span><span class="sxs-lookup"><span data-stu-id="58d17-115">Add scaffold</span></span>

<span data-ttu-id="58d17-116">Możesz przystąpić do generowania kodu, który będzie dostarczać standardowe operacje na danych dla klas modelu.</span><span class="sxs-lookup"><span data-stu-id="58d17-116">You are ready to generate code that will provide standard data operations for the model classes.</span></span> <span data-ttu-id="58d17-117">Kod można dodać poprzez dodanie elementu szkieletu.</span><span class="sxs-lookup"><span data-stu-id="58d17-117">You add the code by adding a scaffold item.</span></span> <span data-ttu-id="58d17-118">Istnieje wiele opcji dla typu szkieletu, który można dodać; w tym samouczku szkielet obejmuje kontroler i widoki zgodne z modelami uczniów i rejestracji utworzonymi w poprzedniej sekcji.</span><span class="sxs-lookup"><span data-stu-id="58d17-118">There are many options for the type of scaffolding you can add; in this tutorial, the scaffold will include a controller and views that correspond to the Student and Enrollment models you created in the previous section.</span></span>

<span data-ttu-id="58d17-119">Aby zachować spójność w projekcie, należy dodać nowy kontroler do folderu istniejące **Kontrolery** .</span><span class="sxs-lookup"><span data-stu-id="58d17-119">To maintain consistency in your project, you will add the new controller to the existing **Controllers** folder.</span></span> <span data-ttu-id="58d17-120">Kliknij prawym przyciskiem myszy folder **controllers** , a następnie wybierz pozycję **Dodaj** > **nowy element szkieletowy**.</span><span class="sxs-lookup"><span data-stu-id="58d17-120">Right-click the **Controllers** folder, and select **Add** > **New Scaffolded Item**.</span></span>

<span data-ttu-id="58d17-121">Wybierz **kontroler MVC 5 z widokami, używając opcji Entity Framework** .</span><span class="sxs-lookup"><span data-stu-id="58d17-121">Select the **MVC 5 Controller with views, using Entity Framework** option.</span></span> <span data-ttu-id="58d17-122">Ta opcja spowoduje wygenerowanie kontrolera i widoków do aktualizowania, usuwania, tworzenia i wyświetlania danych w modelu.</span><span class="sxs-lookup"><span data-stu-id="58d17-122">This option will generate the controller and views for updating, deleting, creating and displaying the data in your model.</span></span>

![Dodaj kontroler MVC](generating-views/_static/image2.png)

<span data-ttu-id="58d17-124">Wybierz pozycję **student (ContosoSite. models)** dla klasy model i wybierz pozycję **ContosoUniversityDataEntities (ContosoSite. models)** dla klasy kontekstu.</span><span class="sxs-lookup"><span data-stu-id="58d17-124">Select **Student (ContosoSite.Models)** for the model class and select the **ContosoUniversityDataEntities (ContosoSite.Models)** for the context class.</span></span> <span data-ttu-id="58d17-125">Zachowaj nazwę kontrolera jako **StudentsController**.</span><span class="sxs-lookup"><span data-stu-id="58d17-125">Keep the controller name as **StudentsController**.</span></span>

<span data-ttu-id="58d17-126">Kliknij pozycję **Add** (Dodaj).</span><span class="sxs-lookup"><span data-stu-id="58d17-126">Click **Add**.</span></span>

<span data-ttu-id="58d17-127">Jeśli wystąpi błąd, może to być spowodowane tym, że projekt nie został skompilowany w poprzedniej sekcji.</span><span class="sxs-lookup"><span data-stu-id="58d17-127">If you receive an error, it may be because you did not build the project in the previous section.</span></span> <span data-ttu-id="58d17-128">Jeśli tak, spróbuj skompilować projekt, a następnie ponownie Dodaj element szkieletowy.</span><span class="sxs-lookup"><span data-stu-id="58d17-128">If so, try building the project, and then add the scaffolded item again.</span></span>

<span data-ttu-id="58d17-129">Po zakończeniu procesu generowania kodu zobaczysz nowy kontroler i widoki w **kontrolerach** i **widokach** projektu, > foldery **uczniów** .</span><span class="sxs-lookup"><span data-stu-id="58d17-129">After the code generation process is complete, you will see a new controller and views in your project's **Controllers** and **Views** > **Students** folders.</span></span>

<span data-ttu-id="58d17-130">Wykonaj ponownie te same kroki, ale Dodaj szkielet dla klasy **rejestracji** .</span><span class="sxs-lookup"><span data-stu-id="58d17-130">Perform the same steps again, but add a scaffold for the **Enrollment** class.</span></span> <span data-ttu-id="58d17-131">Po zakończeniu masz plik **EnrollmentsController.cs** i folder w obszarze **widoki** o nazwie **rejestracje** za pomocą widoków Utwórz, Usuń, szczegóły, Edytuj i Indeksuj.</span><span class="sxs-lookup"><span data-stu-id="58d17-131">When finished, you have an **EnrollmentsController.cs** file, and a folder under **Views** named **Enrollments** with the Create, Delete, Details, Edit and Index views.</span></span>

## <a name="add-links-to-new-views"></a><span data-ttu-id="58d17-132">Dodawanie linków do nowych widoków</span><span class="sxs-lookup"><span data-stu-id="58d17-132">Add links to new views</span></span>

<span data-ttu-id="58d17-133">Aby ułatwić przechodzenie do nowych widoków, można dodać kilka hiperlinków do widoków indeksów dla studentów i rejestracji.</span><span class="sxs-lookup"><span data-stu-id="58d17-133">To make it easier for you to navigate to your new views, you can add a couple of hyperlinks to the Index views for students and enrollments.</span></span> <span data-ttu-id="58d17-134">Otwórz plik w **widokach** > **Home** > *index. cshtml*, który jest stroną główną Twojej witryny.</span><span class="sxs-lookup"><span data-stu-id="58d17-134">Open the file at **Views** > **Home** > *Index.cshtml*, which is the home page for your site.</span></span> <span data-ttu-id="58d17-135">Dodaj następujący kod poniżej Jumbotron.</span><span class="sxs-lookup"><span data-stu-id="58d17-135">Add the following code below the jumbotron.</span></span>

[!code-cshtml[Main](generating-views/samples/sample1.cshtml)]

<span data-ttu-id="58d17-136">Dla metody ActionLink pierwszy parametr jest tekstem, który ma być wyświetlany w łączu.</span><span class="sxs-lookup"><span data-stu-id="58d17-136">For the ActionLink method, the first parameter is the text to display in the link.</span></span> <span data-ttu-id="58d17-137">Drugi parametr jest akcją, a trzeci parametr jest nazwą kontrolera.</span><span class="sxs-lookup"><span data-stu-id="58d17-137">The second parameter is the action and the third parameter is the name of the controller.</span></span> <span data-ttu-id="58d17-138">Na przykład pierwszy link wskazuje akcję indeks w StudentsController.</span><span class="sxs-lookup"><span data-stu-id="58d17-138">For example, the first link points to the Index action in StudentsController.</span></span> <span data-ttu-id="58d17-139">Rzeczywiste hiperłącze jest zbudowane z tych wartości.</span><span class="sxs-lookup"><span data-stu-id="58d17-139">The actual hyperlink is constructed from these values.</span></span> <span data-ttu-id="58d17-140">Pierwszy link ostatecznie przybierze użytkownikom plik **index. cshtml** w folderze **widoki/uczniowie** .</span><span class="sxs-lookup"><span data-stu-id="58d17-140">The first link ultimately takes users to the **Index.cshtml** file within the **Views/Students** folder.</span></span>

## <a name="display-student-views"></a><span data-ttu-id="58d17-141">Wyświetl widoki uczniów</span><span class="sxs-lookup"><span data-stu-id="58d17-141">Display student views</span></span>

<span data-ttu-id="58d17-142">Upewnij się, że kod dodany do projektu prawidłowo wyświetla listę studentów i umożliwia użytkownikom edytowanie, tworzenie i usuwanie rekordów uczniów w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="58d17-142">You will verify that the code added to your project correctly displays a list of the students, and enables users to edit, create, or delete the student records in the database.</span></span>

<span data-ttu-id="58d17-143">Kliknij prawym przyciskiem myszy **widoki** > **Home** > *index. cshtml* plik, a następnie wybierz pozycję **Wyświetl w przeglądarce**.</span><span class="sxs-lookup"><span data-stu-id="58d17-143">Right-click the **Views** > **Home** > *Index.cshtml* file, and select **View in Browser**.</span></span> <span data-ttu-id="58d17-144">Na stronie głównej aplikacji wybierz pozycję **Lista uczniów**.</span><span class="sxs-lookup"><span data-stu-id="58d17-144">On the application home page, select **List of students**.</span></span>

![](generating-views/_static/image6.png)

<span data-ttu-id="58d17-145">Na stronie **indeks** należy zwrócić uwagę na listę studentów i linki do zmodyfikowania tych danych.</span><span class="sxs-lookup"><span data-stu-id="58d17-145">On the **Index** page, notice the list of the students and links to modify this data.</span></span> <span data-ttu-id="58d17-146">Wybierz pozycję **Utwórz nowe** łącze i podaj wartości dla nowego ucznia.</span><span class="sxs-lookup"><span data-stu-id="58d17-146">Select the **Create New** link and provide some values for a new student.</span></span> <span data-ttu-id="58d17-147">Kliknij przycisk **Utwórz**i zwróć uwagę na to, że nowy student zostanie dodany do listy.</span><span class="sxs-lookup"><span data-stu-id="58d17-147">Click **Create**, and notice the new student is added to your list.</span></span>

<span data-ttu-id="58d17-148">Wróć na stronę **indeks** , wybierz link **Edytuj** i Zmień niektóre wartości dla ucznia.</span><span class="sxs-lookup"><span data-stu-id="58d17-148">Back on the **Index** page, select the **Edit** link, and change some of the values for a student.</span></span> <span data-ttu-id="58d17-149">Kliknij przycisk **Zapisz**i zwróć uwagę, że rekord ucznia został zmieniony.</span><span class="sxs-lookup"><span data-stu-id="58d17-149">Click **Save**, and notice the student record has been changed.</span></span>

<span data-ttu-id="58d17-150">Na koniec wybierz łącze **Usuń** i Potwierdź, że chcesz usunąć rekord, klikając przycisk **Usuń** .</span><span class="sxs-lookup"><span data-stu-id="58d17-150">Finally, select the **Delete** link and confirm that you want to delete the record by clicking the **Delete** button.</span></span>

<span data-ttu-id="58d17-151">Bez pisania kodu, dodano widoki, które wykonują Typowe operacje na danych w tabeli uczniów.</span><span class="sxs-lookup"><span data-stu-id="58d17-151">Without writing any code, you have added views that perform common operations on the data in the Student table.</span></span>

<span data-ttu-id="58d17-152">Być może zauważono, że etykieta tekstowa dla pola jest oparta na właściwości bazy danych (takiej jak **LastName**), która nie musi być wyświetlana na stronie sieci Web.</span><span class="sxs-lookup"><span data-stu-id="58d17-152">You may have noticed that the text label for a field is based on the database property (such as **LastName**) which is not necessarily what you want to display on the web page.</span></span> <span data-ttu-id="58d17-153">Na przykład możesz preferować etykietę jako **nazwisko**.</span><span class="sxs-lookup"><span data-stu-id="58d17-153">For example, you may prefer the label to be **Last Name**.</span></span> <span data-ttu-id="58d17-154">Ten problem z wyświetlaniem zostanie naprawiony w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="58d17-154">You will fix this display issue later in the tutorial.</span></span>

## <a name="display-enrollment-views"></a><span data-ttu-id="58d17-155">Wyświetl widoki rejestracji</span><span class="sxs-lookup"><span data-stu-id="58d17-155">Display enrollment views</span></span>

<span data-ttu-id="58d17-156">Baza danych zawiera relację jeden do wielu między tabelami uczniów i rejestracji oraz relacją jeden-do-wielu między tabelami kurs i rejestracja.</span><span class="sxs-lookup"><span data-stu-id="58d17-156">Your database includes a one-to-many relationship between the Student and Enrollment tables, and a one-to-many relationship between the Course and Enrollment tables.</span></span> <span data-ttu-id="58d17-157">Widoki na potrzeby rejestracji poprawnie obsługują te relacje.</span><span class="sxs-lookup"><span data-stu-id="58d17-157">The views for Enrollment correctly handle these relationships.</span></span> <span data-ttu-id="58d17-158">Przejdź do strony głównej witryny i wybierz łącze **Lista rejestracji** , a następnie **Utwórz nowy** link.</span><span class="sxs-lookup"><span data-stu-id="58d17-158">Navigate to the home page for your site and select the **List of enrollments** link and then the **Create New** link.</span></span>

<span data-ttu-id="58d17-159">Widok wyświetla formularz służący do tworzenia nowego rekordu rejestracji.</span><span class="sxs-lookup"><span data-stu-id="58d17-159">The view displays a form for creating a new enrollment record.</span></span> <span data-ttu-id="58d17-160">W szczególności należy zauważyć, że formularz zawiera listę rozwijaną **CourseID** i listę rozwijaną **StudentID** .</span><span class="sxs-lookup"><span data-stu-id="58d17-160">In particular, notice that the form contains a **CourseID** drop-down list and a **StudentID** drop-down list.</span></span> <span data-ttu-id="58d17-161">Oba są wypełniane wartościami z powiązanych tabel.</span><span class="sxs-lookup"><span data-stu-id="58d17-161">Both are populated with values from the related tables.</span></span>

<span data-ttu-id="58d17-162">Ponadto sprawdzanie poprawności podanych wartości jest automatycznie stosowane na podstawie typu danych pola.</span><span class="sxs-lookup"><span data-stu-id="58d17-162">Furthermore, validation of the provided values is automatically applied based on the data type of the field.</span></span> <span data-ttu-id="58d17-163">**Klasa** wymaga liczby, więc zostanie wyświetlony komunikat o błędzie, jeśli spróbujesz podać niezgodną wartość: *Klasa pola musi być liczbą.*</span><span class="sxs-lookup"><span data-stu-id="58d17-163">**Grade** requires a number, so an error message is displayed if you try to provide an incompatible value: *The field Grade must be a number.*</span></span>

<span data-ttu-id="58d17-164">Sprawdzono, że automatycznie generowane widoki umożliwiają użytkownikom współpracują z danymi w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="58d17-164">You have verified that the automatically-generated views enable users to work with the data in the database.</span></span> <span data-ttu-id="58d17-165">W następnym samouczku w tej serii będziesz aktualizować bazę danych i wprowadzać odpowiednie zmiany w aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="58d17-165">In the next tutorial in this series, you will update the database and make the corresponding changes in the web application.</span></span>

## <a name="next-steps"></a><span data-ttu-id="58d17-166">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="58d17-166">Next steps</span></span>

<span data-ttu-id="58d17-167">W tym samouczku zostaną wykonane następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="58d17-167">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="58d17-168">Dodano szkielet</span><span class="sxs-lookup"><span data-stu-id="58d17-168">Added scaffold</span></span>
> * <span data-ttu-id="58d17-169">Dodano linki do nowych widoków</span><span class="sxs-lookup"><span data-stu-id="58d17-169">Added links to new views</span></span>
> * <span data-ttu-id="58d17-170">Wyświetlone widoki uczniów</span><span class="sxs-lookup"><span data-stu-id="58d17-170">Displayed student views</span></span>
> * <span data-ttu-id="58d17-171">Wyświetlone widoki rejestracji</span><span class="sxs-lookup"><span data-stu-id="58d17-171">Displayed enrollment views</span></span>

<span data-ttu-id="58d17-172">Przejdź do następnego samouczka, aby dowiedzieć się, jak zmienić bazę danych.</span><span class="sxs-lookup"><span data-stu-id="58d17-172">Advance to the next tutorial to learn how to change the database.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="58d17-173">Zmiana bazy danych</span><span class="sxs-lookup"><span data-stu-id="58d17-173">Change the database</span></span>](changing-the-database.md)
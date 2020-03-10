---
uid: web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
title: Wywoływanie interfejsu API sieci Web z aplikacji Windows PhoneC#8 () — ASP.NET 4. x
author: rmcmurray
description: 'Samouczek z kodem: Tworzenie aplikacji internetowego interfejsu API ASP.NET w ASP.NET 4. x, która udostępnia Katalog książek w aplikacji Windows Phone 8.'
ms.author: riande
ms.date: 10/09/2013
ms.custom: seoapril2019
ms.assetid: b9775f41-352a-4f82-baa6-23e95b342e20
msc.legacyurl: /web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
msc.type: authoredcontent
ms.openlocfilehash: c5da14a6856f551343b6fb14f0aedc659e792f6b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78614738"
---
# <a name="calling-web-api-from-a-windows-phone-8-application-c"></a><span data-ttu-id="6c143-103">Wywoływanie wzorca Web API z aplikacji Windows Phone 8 (C#)</span><span class="sxs-lookup"><span data-stu-id="6c143-103">Calling Web API from a Windows Phone 8 Application (C#)</span></span>

<span data-ttu-id="6c143-104">Autorzy [Robert McMurraya](https://github.com/rmcmurray)</span><span class="sxs-lookup"><span data-stu-id="6c143-104">by [Robert McMurray](https://github.com/rmcmurray)</span></span>

<span data-ttu-id="6c143-105">W tym samouczku dowiesz się, jak utworzyć kompletny scenariusz kompleksowy składający się z aplikacji internetowego interfejsu API ASP.NET, która udostępnia Katalog książek do aplikacji Windows Phone 8.</span><span class="sxs-lookup"><span data-stu-id="6c143-105">In this tutorial, you will learn how to create a complete end-to-end scenario consisting of an ASP.NET Web API application that provides a catalog of books to a Windows Phone 8 application.</span></span>

### <a name="overview"></a><span data-ttu-id="6c143-106">Omówienie</span><span class="sxs-lookup"><span data-stu-id="6c143-106">Overview</span></span>

<span data-ttu-id="6c143-107">Usługi RESTful Services, takie jak ASP.NET Web API, upraszczają tworzenie aplikacji opartych na protokole HTTP dla deweloperów przez abstrakcję architektury dla aplikacji po stronie serwera i klienta.</span><span class="sxs-lookup"><span data-stu-id="6c143-107">RESTful services like ASP.NET Web API simplify the creation of HTTP-based applications for developers by abstracting the architecture for server-side and client-side applications.</span></span> <span data-ttu-id="6c143-108">Zamiast tworzenia własnościowego protokołu opartego na gnieździe w celu komunikacji deweloperzy interfejsu API sieci Web po prostu muszą opublikować wymagane metody HTTP dla swojej aplikacji (na przykład: GET, POST, PUT, DELETE) i deweloperzy aplikacji klienckich muszą używać tylko Metody HTTP, które są niezbędne dla swojej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6c143-108">Instead of creating a proprietary socket-based protocol for communication, Web API developers simply need to publish the requisite HTTP methods for their application, (for example: GET, POST, PUT, DELETE), and client application developers only need to consume the HTTP methods that are necessary for their application.</span></span>

<span data-ttu-id="6c143-109">W tym kompleksowym samouczku dowiesz się, jak za pomocą interfejsu API sieci Web utworzyć następujące projekty:</span><span class="sxs-lookup"><span data-stu-id="6c143-109">In this end-to-end tutorial, you will learn how to use Web API to create the following projects:</span></span>

- <span data-ttu-id="6c143-110">W [pierwszej części tego samouczka](#STEP1)utworzysz aplikację internetowego interfejsu API ASP.NET, która obsługuje wszystkie operacje tworzenia, odczytu, aktualizacji i usuwania (CRUD) w celu zarządzania katalogiem książek.</span><span class="sxs-lookup"><span data-stu-id="6c143-110">In the [first part of this tutorial](#STEP1), you will create an ASP.NET Web API application that supports all of the Create, Read, Update, and Delete (CRUD) operations to manage a book catalog.</span></span> <span data-ttu-id="6c143-111">Ta aplikacja będzie używać [przykładowego pliku XML (Books. xml)](https://msdn.microsoft.com/library/windows/desktop/ms762271.aspx) w witrynie MSDN.</span><span class="sxs-lookup"><span data-stu-id="6c143-111">This application will use the [Sample XML File (books.xml)](https://msdn.microsoft.com/library/windows/desktop/ms762271.aspx) from MSDN.</span></span>
- <span data-ttu-id="6c143-112">W [drugiej części tego samouczka](#STEP2)utworzysz interaktywną aplikację Windows Phone 8, która pobiera dane z aplikacji internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="6c143-112">In the [second part of this tutorial](#STEP2), you will create an interactive Windows Phone 8 application that retrieves the data from your Web API application.</span></span>

#### <a name="prerequisites"></a><span data-ttu-id="6c143-113">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="6c143-113">Prerequisites</span></span>

- <span data-ttu-id="6c143-114">Visual Studio 2013 z zainstalowanym zestawem SDK Windows Phone 8</span><span class="sxs-lookup"><span data-stu-id="6c143-114">Visual Studio 2013 with the Windows Phone 8 SDK installed</span></span>
- <span data-ttu-id="6c143-115">System Windows 8 lub nowszy w systemie 64-bitowym z zainstalowaną funkcją Hyper-V</span><span class="sxs-lookup"><span data-stu-id="6c143-115">Windows 8 or later on a 64-bit system with Hyper-V installed</span></span>
- <span data-ttu-id="6c143-116">Aby uzyskać listę dodatkowych wymagań, zobacz sekcję *wymagania systemowe* na stronie pobierania [zestawu Windows Phone SDK 8,0](https://www.microsoft.com/download/details.aspx?id=35471) .</span><span class="sxs-lookup"><span data-stu-id="6c143-116">For a list of additional requirements, see the *System Requirements* section on the [Windows Phone SDK 8.0](https://www.microsoft.com/download/details.aspx?id=35471) download page.</span></span>

> [!NOTE]
> <span data-ttu-id="6c143-117">Jeśli zamierzasz przetestować łączność między interfejsem API sieci Web i Windows Phone 8 projektów w systemie lokalnym, musisz postępować zgodnie z instrukcjami zawartymi w temacie *[łączenie emulatora Windows Phone 8 z aplikacjami interfejsu API sieci Web na komputerze lokalnym](https://go.microsoft.com/fwlink/?LinkId=324014)* w celu skonfigurowania środowiska testowego.</span><span class="sxs-lookup"><span data-stu-id="6c143-117">If you are going to test the connectivity between Web API and Windows Phone 8 projects on your local system, you will need to follow the instructions in the *[Connecting the Windows Phone 8 Emulator to Web API Applications on a Local Computer](https://go.microsoft.com/fwlink/?LinkId=324014)* article to set up your testing environment.</span></span>

<a id="STEP1"></a>
### <a name="step-1-creating-the-web-api-bookstore-project"></a><span data-ttu-id="6c143-118">Krok 1. Tworzenie projektu internetowego interfejsu API księgarni</span><span class="sxs-lookup"><span data-stu-id="6c143-118">Step 1: Creating the Web API Bookstore Project</span></span>

<span data-ttu-id="6c143-119">Pierwszym krokiem tego kompleksowego samouczka jest utworzenie projektu interfejsu API sieci Web, który obsługuje wszystkie operacje CRUD; należy pamiętać, że w [kroku 2](#STEP2) tego samouczka dodasz projekt aplikacji Windows Phone do tego rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="6c143-119">The first step of this end-to-end tutorial is to create a Web API project that supports all of the CRUD operations; note that you will add the Windows Phone application project to this solution in [Step 2](#STEP2) of this tutorial.</span></span>

1. <span data-ttu-id="6c143-120">Otwórz **Visual Studio 2013**.</span><span class="sxs-lookup"><span data-stu-id="6c143-120">Open **Visual Studio 2013**.</span></span>
2. <span data-ttu-id="6c143-121">Kliknij kolejno pozycje **plik**, **Nowy**i **projekt**.</span><span class="sxs-lookup"><span data-stu-id="6c143-121">Click **File**, then **New**, and then **Project**.</span></span>
3. <span data-ttu-id="6c143-122">Po wyświetleniu okna dialogowego **Nowy projekt** rozwiń węzeł **zainstalowane**, a następnie pozycję **Szablony**, a następnie pozycję **Wizualizacja C#** , a następnie kliknij pozycję **Sieć Web**.</span><span class="sxs-lookup"><span data-stu-id="6c143-122">When the **New Project** dialog box is displayed, expand **Installed**, then **Templates**, then **Visual C#**, and then **Web**.</span></span>

   | [![](calling-web-api-from-a-windows-phone-8-application/_static/image2.png)](calling-web-api-from-a-windows-phone-8-application/_static/image1.png) |
   |-----------------------------------------------------------------------------------------------------------------------------------------------------|
   |                                                                <span data-ttu-id="6c143-123">Kliknij obraz, aby rozwinąć</span><span class="sxs-lookup"><span data-stu-id="6c143-123">Click image to expand</span></span>                                                                |

4. <span data-ttu-id="6c143-124">Wyróżnij **aplikację sieci Web ASP.NET**, wprowadź **księgarni** dla nazwy projektu, a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="6c143-124">Highlight **ASP.NET Web Application**, enter **BookStore** for the project name, and then click **OK**.</span></span>
5. <span data-ttu-id="6c143-125">Gdy zostanie wyświetlone okno dialogowe **Nowy projekt ASP.NET** , wybierz szablon **internetowego interfejsu API** , a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="6c143-125">When the **New ASP.NET Project** dialog box is displayed, select the **Web API** template, and then click **OK**.</span></span>

   | [![](calling-web-api-from-a-windows-phone-8-application/_static/image4.png)](calling-web-api-from-a-windows-phone-8-application/_static/image3.png) |
   |-----------------------------------------------------------------------------------------------------------------------------------------------------|
   |                                                                <span data-ttu-id="6c143-126">Kliknij obraz, aby rozwinąć</span><span class="sxs-lookup"><span data-stu-id="6c143-126">Click image to expand</span></span>                                                                |

6. <span data-ttu-id="6c143-127">Po otwarciu projektu interfejsu API sieci Web Usuń przykładowy kontroler z projektu:</span><span class="sxs-lookup"><span data-stu-id="6c143-127">When the Web API project opens, remove the sample controller from the project:</span></span>

    1. <span data-ttu-id="6c143-128">Rozwiń folder **controllers** w Eksploratorze rozwiązań.</span><span class="sxs-lookup"><span data-stu-id="6c143-128">Expand the **Controllers** folder in the solution explorer.</span></span>
    2. <span data-ttu-id="6c143-129">Kliknij prawym przyciskiem myszy plik **ValuesController.cs** , a następnie kliknij polecenie **Usuń**.</span><span class="sxs-lookup"><span data-stu-id="6c143-129">Right-click the **ValuesController.cs** file, and then click **Delete**.</span></span>
    3. <span data-ttu-id="6c143-130">Kliknij przycisk **OK** po wyświetleniu monitu o potwierdzenie usunięcia.</span><span class="sxs-lookup"><span data-stu-id="6c143-130">Click **OK** when prompted to confirm the deletion.</span></span>
7. <span data-ttu-id="6c143-131">Dodaj plik danych XML do projektu interfejsu API sieci Web; Ten plik zawiera zawartość wykazu księgarni:</span><span class="sxs-lookup"><span data-stu-id="6c143-131">Add an XML data file to the Web API project; this file contains the contents of the bookstore catalog:</span></span>

   1. <span data-ttu-id="6c143-132">Kliknij prawym przyciskiem myszy folder **\_danych aplikacji** w Eksploratorze rozwiązań, a następnie kliknij pozycję **Dodaj**, a następnie kliknij pozycję **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="6c143-132">Right-click the **App\_Data** folder in the solution explorer, then click **Add**, and then click **New Item**.</span></span>
   2. <span data-ttu-id="6c143-133">Gdy zostanie wyświetlone okno dialogowe **Dodaj nowy element** , zaznacz szablon **pliku XML** .</span><span class="sxs-lookup"><span data-stu-id="6c143-133">When the **Add New Item** dialog box is displayed, highlight the **XML File** template.</span></span>
   3. <span data-ttu-id="6c143-134">Nazwij plik **Books. XML**, a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="6c143-134">Name the file **Books.xml**, and then click **Add**.</span></span>
   4. <span data-ttu-id="6c143-135">Po otwarciu pliku **Books. XML** Zastąp kod w pliku XML z przykładowego pliku **Books. XML** w witrynie MSDN:</span><span class="sxs-lookup"><span data-stu-id="6c143-135">When the **Books.xml** file is opened, replace the code in the file with the XML from the sample **books.xml** file on MSDN:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample1.xml)]
   5. <span data-ttu-id="6c143-136">Zapisz i zamknij plik XML.</span><span class="sxs-lookup"><span data-stu-id="6c143-136">Save and close the XML file.</span></span>

8. <span data-ttu-id="6c143-137">Dodaj model księgarni do projektu interfejsu API sieci Web; Ten model zawiera logikę tworzenia, odczytu, aktualizacji i usuwania (CRUD) dla aplikacji księgarni:</span><span class="sxs-lookup"><span data-stu-id="6c143-137">Add the bookstore model to the Web API project; this model contains the Create, Read, Update, and Delete (CRUD) logic for the bookstore application:</span></span>

   1. <span data-ttu-id="6c143-138">Kliknij prawym przyciskiem myszy folder **modele** w Eksploratorze rozwiązań, a następnie kliknij polecenie **Dodaj**, a następnie kliknij pozycję **Klasa**.</span><span class="sxs-lookup"><span data-stu-id="6c143-138">Right-click the **Models** folder in the solution explorer, then click **Add**, and then click **Class**.</span></span>
   2. <span data-ttu-id="6c143-139">Gdy zostanie wyświetlone okno dialogowe **Dodaj nowy element** , Nazwij plik klasy **BookDetails.cs**, a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="6c143-139">When the **Add New Item** dialog box is displayed, name the class file **BookDetails.cs**, and then click **Add**.</span></span>
   3. <span data-ttu-id="6c143-140">Po otwarciu pliku **BookDetails.cs** Zastąp kod w pliku następującym:</span><span class="sxs-lookup"><span data-stu-id="6c143-140">When the **BookDetails.cs** file is opened, replace the code in the file with the following:</span></span> 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample2.cs)]
   4. <span data-ttu-id="6c143-141">Zapisz i zamknij plik **BookDetails.cs** .</span><span class="sxs-lookup"><span data-stu-id="6c143-141">Save and close the **BookDetails.cs** file.</span></span>

9. <span data-ttu-id="6c143-142">Dodaj kontroler księgarni do projektu interfejsu API sieci Web:</span><span class="sxs-lookup"><span data-stu-id="6c143-142">Add the bookstore controller to the Web API project:</span></span>

   1. <span data-ttu-id="6c143-143">Kliknij prawym przyciskiem myszy folder **controllers** w Eksploratorze rozwiązań, a następnie kliknij pozycję **Dodaj**, a następnie kliknij pozycję **kontroler**.</span><span class="sxs-lookup"><span data-stu-id="6c143-143">Right-click the **Controllers** folder in the solution explorer, then click **Add**, and then click **Controller**.</span></span>
   2. <span data-ttu-id="6c143-144">Po wyświetleniu okna dialogowego **Dodawanie szkieletu** zaznacz opcję **kontroler Web API 2 — pusty**, a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="6c143-144">When the **Add Scaffold** dialog box is displayed, highlight **Web API 2 Controller - Empty**, and then click **Add**.</span></span>
   3. <span data-ttu-id="6c143-145">Gdy zostanie wyświetlone okno dialogowe **Dodaj kontroler** , nazwij kontroler **BooksController**, a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="6c143-145">When the **Add Controller** dialog box is displayed, name the controller **BooksController**, and then click **Add**.</span></span>
   4. <span data-ttu-id="6c143-146">Po otwarciu pliku **BooksController.cs** Zastąp kod w pliku następującym:</span><span class="sxs-lookup"><span data-stu-id="6c143-146">When the **BooksController.cs** file is opened, replace the code in the file with the following:</span></span> 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample3.cs)]
   5. <span data-ttu-id="6c143-147">Zapisz i zamknij plik **BooksController.cs** .</span><span class="sxs-lookup"><span data-stu-id="6c143-147">Save and close the **BooksController.cs** file.</span></span>

10. <span data-ttu-id="6c143-148">Utwórz aplikację interfejsu API sieci Web, aby sprawdzić pod kątem błędów.</span><span class="sxs-lookup"><span data-stu-id="6c143-148">Build the Web API application to check for errors.</span></span>

<a id="STEP2"></a>
### <a name="step-2-adding-the-windows-phone-8-bookstore-catalog-project"></a><span data-ttu-id="6c143-149">Krok 2. Dodawanie projektu katalogu Windows Phone 8 księgarni</span><span class="sxs-lookup"><span data-stu-id="6c143-149">Step 2: Adding the Windows Phone 8 Bookstore Catalog Project</span></span>

<span data-ttu-id="6c143-150">Następnym krokiem tego kompleksowego scenariusza jest utworzenie aplikacji katalogu dla Windows Phone 8.</span><span class="sxs-lookup"><span data-stu-id="6c143-150">The next step of this end-to-end scenario is to create the catalog application for Windows Phone 8.</span></span> <span data-ttu-id="6c143-151">Ta aplikacja będzie używać szablonu *aplikacji Windows Phone* danych dla domyślnego interfejsu użytkownika i będzie używać aplikacji internetowego interfejsu API, która została utworzona w [kroku 1](#STEP1) tego samouczka jako źródła danych.</span><span class="sxs-lookup"><span data-stu-id="6c143-151">This application will use the *Windows Phone Databound App* template for the default user interface, and it will use the Web API application that you created in [Step 1](#STEP1) of this tutorial as the data source.</span></span>

1. <span data-ttu-id="6c143-152">Kliknij prawym przyciskiem myszy rozwiązanie **księgarni** w Eksploratorze rozwiązań, a następnie kliknij przycisk **Dodaj**, a następnie **Nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="6c143-152">Right-click the **BookStore** solution in the in the solution explorer, then click **Add**, and then **New Project**.</span></span>
2. <span data-ttu-id="6c143-153">Po wyświetleniu okna dialogowego **Nowy projekt** rozwiń węzeł **zainstalowane**, a następnie **pozycję C#Wizualizacja** , a następnie **Windows Phone**.</span><span class="sxs-lookup"><span data-stu-id="6c143-153">When the **New Project** dialog box is displayed, expand **Installed**, then **Visual C#**, and then **Windows Phone**.</span></span>
3. <span data-ttu-id="6c143-154">Wyróżnij **Windows Phone aplikacji powiązanej z danymi**, wprowadź **BookCatalog** jako nazwę, a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="6c143-154">Highlight **Windows Phone Databound App**, enter **BookCatalog** for the name, and then click **OK**.</span></span>
4. <span data-ttu-id="6c143-155">Dodaj pakiet NuGet Json.NET do projektu **BookCatalog** :</span><span class="sxs-lookup"><span data-stu-id="6c143-155">Add the Json.NET NuGet package to the **BookCatalog** project:</span></span>

    1. <span data-ttu-id="6c143-156">Kliknij prawym przyciskiem myszy pozycję **odwołania** dla projektu **BookCatalog** w Eksploratorze rozwiązań, a następnie kliknij polecenie **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="6c143-156">Right-click **References** for the **BookCatalog** project in the solution explorer, and then click **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="6c143-157">Gdy zostanie wyświetlone okno dialogowe **Zarządzanie pakietami NuGet** , rozwiń sekcję **online** i podświetl pozycję **NuGet.org**.</span><span class="sxs-lookup"><span data-stu-id="6c143-157">When the **Manage NuGet Packages** dialog box is displayed, expand the **Online** section, and highlight **nuget.org**.</span></span>
    3. <span data-ttu-id="6c143-158">Wprowadź **JSON.NET** w polu wyszukiwania, a następnie kliknij ikonę wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="6c143-158">Enter **Json.NET** in the search field and click the search icon.</span></span>
    4. <span data-ttu-id="6c143-159">Zaznacz **JSON.NET** w wynikach wyszukiwania, a następnie kliknij przycisk **Instaluj**.</span><span class="sxs-lookup"><span data-stu-id="6c143-159">Highlight **Json.NET** in the search results, and then click **Install**.</span></span>
    5. <span data-ttu-id="6c143-160">Po zakończeniu instalacji kliknij przycisk **Zamknij**.</span><span class="sxs-lookup"><span data-stu-id="6c143-160">When the installation has completed, click **Close**.</span></span>
5. <span data-ttu-id="6c143-161">Dodaj model **BookDetails** do projektu **BookCatalog** ; zawiera ogólny model klasy księgarni:</span><span class="sxs-lookup"><span data-stu-id="6c143-161">Add the **BookDetails** model to the **BookCatalog** project; this contains a generic model of the bookstore class:</span></span>

   1. <span data-ttu-id="6c143-162">Kliknij prawym przyciskiem myszy projekt **BookCatalog** w Eksploratorze rozwiązań, a następnie kliknij pozycję **Dodaj**, a następnie kliknij pozycję **Nowy folder**.</span><span class="sxs-lookup"><span data-stu-id="6c143-162">Right-click the **BookCatalog** project in the solution explorer, then click **Add**, and then click **New Folder**.</span></span>
   2. <span data-ttu-id="6c143-163">Nazwij nowe **modele**folderów.</span><span class="sxs-lookup"><span data-stu-id="6c143-163">Name the new folder **Models**.</span></span>
   3. <span data-ttu-id="6c143-164">Kliknij prawym przyciskiem myszy folder **modele** w Eksploratorze rozwiązań, a następnie kliknij polecenie **Dodaj**, a następnie kliknij pozycję **Klasa**.</span><span class="sxs-lookup"><span data-stu-id="6c143-164">Right-click the **Models** folder in the solution explorer, then click **Add**, and then click **Class**.</span></span>
   4. <span data-ttu-id="6c143-165">Gdy zostanie wyświetlone okno dialogowe **Dodaj nowy element** , Nazwij plik klasy **BookDetails.cs**, a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="6c143-165">When the **Add New Item** dialog box is displayed, name the class file **BookDetails.cs**, and then click **Add**.</span></span>
   5. <span data-ttu-id="6c143-166">Po otwarciu pliku **BookDetails.cs** Zastąp kod w pliku następującym:</span><span class="sxs-lookup"><span data-stu-id="6c143-166">When the **BookDetails.cs** file is opened, replace the code in the file with the following:</span></span> 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample4.cs)]
   6. <span data-ttu-id="6c143-167">Zapisz i zamknij plik **BookDetails.cs** .</span><span class="sxs-lookup"><span data-stu-id="6c143-167">Save and close the **BookDetails.cs** file.</span></span>

6. <span data-ttu-id="6c143-168">Zaktualizuj klasę **MainViewModel.cs** , aby obejmowała funkcję komunikacji z aplikacją internetowego interfejsu API księgarni:</span><span class="sxs-lookup"><span data-stu-id="6c143-168">Update the **MainViewModel.cs** class to include the functionality to communicate with the BookStore Web API application:</span></span>

   1. <span data-ttu-id="6c143-169">Rozwiń folder **modele widoków** w Eksploratorze rozwiązań, a następnie kliknij dwukrotnie plik **MainViewModel.cs** .</span><span class="sxs-lookup"><span data-stu-id="6c143-169">Expand the **ViewModels** folder in the solution explorer, and then double-click the **MainViewModel.cs** file.</span></span>
   2. <span data-ttu-id="6c143-170">Po otwarciu pliku **MainViewModel.cs** Zastąp kod w pliku następującym poleceniem: należy pamiętać, że należy zaktualizować wartość stałej `apiUrl` z rzeczywistym adresem URL internetowego interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="6c143-170">When the **MainViewModel.cs** file is opened, replace the code in the file with the following; note that you will need to update the value of the `apiUrl` constant with the actual URL of your Web API:</span></span> 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample5.cs)]
   3. <span data-ttu-id="6c143-171">Zapisz i zamknij plik **MainViewModel.cs** .</span><span class="sxs-lookup"><span data-stu-id="6c143-171">Save and close the **MainViewModel.cs** file.</span></span>

7. <span data-ttu-id="6c143-172">Zaktualizuj plik **MainPage. XAML** , aby dostosować nazwę aplikacji:</span><span class="sxs-lookup"><span data-stu-id="6c143-172">Update the **MainPage.xaml** file to customize the application name:</span></span>

   1. <span data-ttu-id="6c143-173">Kliknij dwukrotnie plik **MainPage. XAML** w Eksploratorze rozwiązań.</span><span class="sxs-lookup"><span data-stu-id="6c143-173">Double-click the **MainPage.xaml** file in the solution explorer.</span></span>
   2. <span data-ttu-id="6c143-174">Po otwarciu pliku **MainPage. XAML** Znajdź następujące wiersze kodu:</span><span class="sxs-lookup"><span data-stu-id="6c143-174">When the **MainPage.xaml** file is opened, locate the following lines of code:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample6.xml)]
   3. <span data-ttu-id="6c143-175">Zamień te wiersze na następujące:</span><span class="sxs-lookup"><span data-stu-id="6c143-175">Replace those lines with the following:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample7.xml)]
   4. <span data-ttu-id="6c143-176">Zapisz i zamknij plik **MainPage. XAML** .</span><span class="sxs-lookup"><span data-stu-id="6c143-176">Save and close the **MainPage.xaml** file.</span></span>

8. <span data-ttu-id="6c143-177">Zaktualizuj plik **DetailsPage. XAML** , aby dostosować wyświetlane elementy:</span><span class="sxs-lookup"><span data-stu-id="6c143-177">Update the **DetailsPage.xaml** file to customize the displayed items:</span></span>

   1. <span data-ttu-id="6c143-178">Kliknij dwukrotnie plik **DetailsPage. XAML** w Eksploratorze rozwiązań.</span><span class="sxs-lookup"><span data-stu-id="6c143-178">Double-click the **DetailsPage.xaml** file in the solution explorer.</span></span>
   2. <span data-ttu-id="6c143-179">Po otwarciu pliku **DetailsPage. XAML** Znajdź następujące wiersze kodu:</span><span class="sxs-lookup"><span data-stu-id="6c143-179">When the **DetailsPage.xaml** file is opened, locate the following lines of code:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample8.xml)]
   3. <span data-ttu-id="6c143-180">Zamień te wiersze na następujące:</span><span class="sxs-lookup"><span data-stu-id="6c143-180">Replace those lines with the following:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample9.xml)]
   4. <span data-ttu-id="6c143-181">Zapisz i zamknij plik **DetailsPage. XAML** .</span><span class="sxs-lookup"><span data-stu-id="6c143-181">Save and close the **DetailsPage.xaml** file.</span></span>

9. <span data-ttu-id="6c143-182">Skompiluj aplikację Windows Phone, aby wyszukać błędy.</span><span class="sxs-lookup"><span data-stu-id="6c143-182">Build the Windows Phone application to check for errors.</span></span>

### <a name="step-3-testing-the-end-to-end-solution"></a><span data-ttu-id="6c143-183">Krok 3. Testowanie kompleksowego rozwiązania</span><span class="sxs-lookup"><span data-stu-id="6c143-183">Step 3: Testing the End-to-End Solution</span></span>

<span data-ttu-id="6c143-184">Jak wspomniano w sekcji *wymagania wstępne* w tym samouczku, podczas testowania łączności między interfejsem API sieci web i Windows Phone 8 projektów w systemie lokalnym, należy postępować zgodnie z instrukcjami podanymi w temacie *[łączenie emulatora systemu Windows Phone 8 z aplikacjami interfejsu API sieci Web na komputerze lokalnym](https://go.microsoft.com/fwlink/?LinkId=324014)* w celu skonfigurowania środowiska testowego.</span><span class="sxs-lookup"><span data-stu-id="6c143-184">As mentioned in the *Prerequisites* section of this tutorial, when you are testing the connectivity between Web API and Windows Phone 8 projects on your local system, you will need to follow the instructions in the *[Connecting the Windows Phone 8 Emulator to Web API Applications on a Local Computer](https://go.microsoft.com/fwlink/?LinkId=324014)* article to set up your testing environment.</span></span>

<span data-ttu-id="6c143-185">Po skonfigurowaniu środowiska testowego należy ustawić aplikację Windows Phone jako projekt startowy.</span><span class="sxs-lookup"><span data-stu-id="6c143-185">Once you have the testing environment configured, you will need to set the Windows Phone application as the startup project.</span></span> <span data-ttu-id="6c143-186">Aby to zrobić, zaznacz aplikację **BookCatalog** w Eksploratorze rozwiązań, a następnie kliknij pozycję **Ustaw jako projekt startowy**:</span><span class="sxs-lookup"><span data-stu-id="6c143-186">To do so, highlight the **BookCatalog** application in the solution explorer, and then click **Set as StartUp Project**:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image6.png)](calling-web-api-from-a-windows-phone-8-application/_static/image5.png) |
| --- |
| <span data-ttu-id="6c143-187">Kliknij obraz, aby rozwinąć</span><span class="sxs-lookup"><span data-stu-id="6c143-187">Click image to expand</span></span> |

<span data-ttu-id="6c143-188">Po naciśnięciu klawisza F5 program Visual Studio uruchomi emulator Windows Phone, który wyświetli &quot;, poczekaj&quot; komunikat, gdy dane aplikacji zostaną pobrane z internetowego interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="6c143-188">When you press F5, Visual Studio will start both the Windows Phone Emulator, which will display a &quot;Please Wait&quot; message while the application data is retrieved from your Web API:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image8.png)](calling-web-api-from-a-windows-phone-8-application/_static/image7.png) |
| --- |
| <span data-ttu-id="6c143-189">Kliknij obraz, aby rozwinąć</span><span class="sxs-lookup"><span data-stu-id="6c143-189">Click image to expand</span></span> |

<span data-ttu-id="6c143-190">Jeśli wszystko powiedzie się, powinien pojawić się wyświetlony wykaz:</span><span class="sxs-lookup"><span data-stu-id="6c143-190">If everything is successful, you should see the catalog displayed:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image10.png)](calling-web-api-from-a-windows-phone-8-application/_static/image9.png) |
| --- |
| <span data-ttu-id="6c143-191">Kliknij obraz, aby rozwinąć</span><span class="sxs-lookup"><span data-stu-id="6c143-191">Click image to expand</span></span> |

<span data-ttu-id="6c143-192">W przypadku wybrania dowolnego tytułu książki aplikacja wyświetli opis książki:</span><span class="sxs-lookup"><span data-stu-id="6c143-192">If you tap on any book title, the application will display the book description:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image12.png)](calling-web-api-from-a-windows-phone-8-application/_static/image11.png) |
| --- |
| <span data-ttu-id="6c143-193">Kliknij obraz, aby rozwinąć</span><span class="sxs-lookup"><span data-stu-id="6c143-193">Click image to expand</span></span> |

<span data-ttu-id="6c143-194">Jeśli aplikacja nie może komunikować się z interfejsem API sieci Web, zostanie wyświetlony komunikat o błędzie:</span><span class="sxs-lookup"><span data-stu-id="6c143-194">If the application cannot communicate with your Web API, an error message will be displayed:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image14.png)](calling-web-api-from-a-windows-phone-8-application/_static/image13.png) |
| --- |
| <span data-ttu-id="6c143-195">Kliknij obraz, aby rozwinąć</span><span class="sxs-lookup"><span data-stu-id="6c143-195">Click image to expand</span></span> |

<span data-ttu-id="6c143-196">Jeśli naciśniesz komunikat o błędzie, zostaną wyświetlone wszystkie dodatkowe szczegóły dotyczące błędu:</span><span class="sxs-lookup"><span data-stu-id="6c143-196">If you tap on the error message, any additional details about the error will be displayed:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image16.png)](calling-web-api-from-a-windows-phone-8-application/_static/image15.png) |
|-------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                                                 <span data-ttu-id="6c143-197">Kliknij obraz, aby rozwinąć</span><span class="sxs-lookup"><span data-stu-id="6c143-197">Click image to expand</span></span>                                                                 |

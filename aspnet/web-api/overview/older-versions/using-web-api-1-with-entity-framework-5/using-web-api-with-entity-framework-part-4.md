---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
title: 'Część 4: Dodawanie widoku administratora | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 792f4513-a508-4d14-a0dd-1a2fe282c7bb
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
msc.type: authoredcontent
ms.openlocfilehash: 664aeb33031e933322886a6d6bdd989277e9fda2
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600015"
---
# <a name="part-4-adding-an-admin-view"></a><span data-ttu-id="2815d-102">Część 4: Dodawanie widoku administratora</span><span class="sxs-lookup"><span data-stu-id="2815d-102">Part 4: Adding an Admin View</span></span>

<span data-ttu-id="2815d-103">według [Jan Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="2815d-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="2815d-104">Pobierz ukończony projekt</span><span class="sxs-lookup"><span data-stu-id="2815d-104">Download Completed Project</span></span>](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-view"></a><span data-ttu-id="2815d-105">Dodawanie widoku administratora</span><span class="sxs-lookup"><span data-stu-id="2815d-105">Add an Admin View</span></span>

<span data-ttu-id="2815d-106">Teraz zmienimy się po stronie klienta i dodasz stronę, która będzie mogła korzystać z danych z kontrolera administratora.</span><span class="sxs-lookup"><span data-stu-id="2815d-106">Now we'll turn to the client side, and add a page that can consume data from the Admin controller.</span></span> <span data-ttu-id="2815d-107">Na stronie użytkownicy będą mogli tworzyć, edytować lub usuwać produkty, wysyłając żądania AJAX do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="2815d-107">The page will allow users to create, edit, or delete products, by sending AJAX requests to the controller.</span></span>

<span data-ttu-id="2815d-108">W Eksplorator rozwiązań rozwiń folder kontrolery i Otwórz plik o nazwie HomeController.cs.</span><span class="sxs-lookup"><span data-stu-id="2815d-108">In Solution Explorer, expand the Controllers folder and open the file named HomeController.cs.</span></span> <span data-ttu-id="2815d-109">Ten plik zawiera kontroler MVC.</span><span class="sxs-lookup"><span data-stu-id="2815d-109">This file contains an MVC controller.</span></span> <span data-ttu-id="2815d-110">Dodaj metodę o nazwie `Admin`:</span><span class="sxs-lookup"><span data-stu-id="2815d-110">Add a method named `Admin`:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample1.cs)]

<span data-ttu-id="2815d-111">Metoda **HttpRouteUrl** tworzy identyfikator URI dla internetowego interfejsu API i zapisuje go w zbiorze widoku dla późniejszej.</span><span class="sxs-lookup"><span data-stu-id="2815d-111">The **HttpRouteUrl** method creates the URI to the web API, and we store this in the view bag for later.</span></span>

<span data-ttu-id="2815d-112">Następnie umieść kursor tekstu w metodzie `Admin` akcji, a następnie kliknij prawym przyciskiem myszy i wybierz polecenie **Dodaj widok**.</span><span class="sxs-lookup"><span data-stu-id="2815d-112">Next, position the text cursor within the `Admin` action method, then right-click and select **Add View**.</span></span> <span data-ttu-id="2815d-113">Spowoduje to wyświetlenie okna dialogowego **Dodawanie widoku** .</span><span class="sxs-lookup"><span data-stu-id="2815d-113">This will bring up the **Add View** dialog.</span></span>

![](using-web-api-with-entity-framework-part-4/_static/image1.png)

<span data-ttu-id="2815d-114">W oknie dialogowym **Dodawanie widoku** Nazwij widok "Administrator".</span><span class="sxs-lookup"><span data-stu-id="2815d-114">In the **Add View** dialog, name the view "Admin".</span></span> <span data-ttu-id="2815d-115">Zaznacz pole wyboru z etykietą **Utwórz widok o jednoznacznie określonym typie**.</span><span class="sxs-lookup"><span data-stu-id="2815d-115">Select the check box labeled **Create a strongly-typed view**.</span></span> <span data-ttu-id="2815d-116">W obszarze **Klasa modelu**wybierz pozycję "produkt (ProductStore. models)".</span><span class="sxs-lookup"><span data-stu-id="2815d-116">Under **Model Class**, select "Product (ProductStore.Models)".</span></span> <span data-ttu-id="2815d-117">Pozostaw wszystkie inne opcje jako wartości domyślne.</span><span class="sxs-lookup"><span data-stu-id="2815d-117">Leave all the other options as their default values.</span></span>

![](using-web-api-with-entity-framework-part-4/_static/image2.png)

<span data-ttu-id="2815d-118">Kliknięcie przycisku **Dodaj** dodaje plik o nazwie admin. cshtml w widokach/na stronie głównej.</span><span class="sxs-lookup"><span data-stu-id="2815d-118">Clicking **Add** adds a file named Admin.cshtml under Views/Home.</span></span> <span data-ttu-id="2815d-119">Otwórz ten plik i Dodaj następujący kod HTML.</span><span class="sxs-lookup"><span data-stu-id="2815d-119">Open this file and add the following HTML.</span></span> <span data-ttu-id="2815d-120">Ten kod HTML definiuje strukturę strony, ale żadne funkcje nie są jeszcze przewodne.</span><span class="sxs-lookup"><span data-stu-id="2815d-120">This HTML defines the structure of the page, but no functionality is wired up yet.</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample2.cshtml)]

## <a name="create-a-link-to-the-admin-page"></a><span data-ttu-id="2815d-121">Tworzenie linku do strony administratora</span><span class="sxs-lookup"><span data-stu-id="2815d-121">Create a Link to the Admin Page</span></span>

<span data-ttu-id="2815d-122">W Eksplorator rozwiązań rozwiń folder widoki, a następnie rozwiń folder udostępniony.</span><span class="sxs-lookup"><span data-stu-id="2815d-122">In Solution Explorer, expand the Views folder and then expand the Shared folder.</span></span> <span data-ttu-id="2815d-123">Otwórz plik o nazwie \_Layout. cshtml.</span><span class="sxs-lookup"><span data-stu-id="2815d-123">Open the file named \_Layout.cshtml.</span></span> <span data-ttu-id="2815d-124">Znajdź element **ul** z identyfikatorem "menu" i link akcji dla widoku administratora:</span><span class="sxs-lookup"><span data-stu-id="2815d-124">Locate the **ul** element with id = "menu", and an action link for the Admin view:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample3.cshtml)]

> [!NOTE]
> <span data-ttu-id="2815d-125">W przykładowym projekcie zostały wykonane pewne inne zmiany, takie jak zastąpienie ciągu "logo" w tym miejscu.</span><span class="sxs-lookup"><span data-stu-id="2815d-125">In the sample project, I made a few other cosmetic changes, such as replacing the string "Your logo here".</span></span> <span data-ttu-id="2815d-126">Nie wpływają one na działanie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2815d-126">These don't affect the functionality of the application.</span></span> <span data-ttu-id="2815d-127">Możesz pobrać projekt i porównać pliki.</span><span class="sxs-lookup"><span data-stu-id="2815d-127">You can download the project and compare the files.</span></span>

<span data-ttu-id="2815d-128">Uruchom aplikację i kliknij link "Administrator", który pojawia się w górnej części strony głównej.</span><span class="sxs-lookup"><span data-stu-id="2815d-128">Run the application and click the "Admin" link that appears at the top of the home page.</span></span> <span data-ttu-id="2815d-129">Strona administrator powinna wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="2815d-129">The Admin page should look like the following:</span></span>

![](using-web-api-with-entity-framework-part-4/_static/image3.png)

<span data-ttu-id="2815d-130">Teraz Strona nie wykonuje żadnych czynności.</span><span class="sxs-lookup"><span data-stu-id="2815d-130">Right now, the page doesn't do anything.</span></span> <span data-ttu-id="2815d-131">W następnej sekcji użyjemy narzędzia separowania. js do utworzenia dynamicznego interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="2815d-131">In the next section, we'll use Knockout.js to create a dynamic UI.</span></span>

## <a name="add-authorization"></a><span data-ttu-id="2815d-132">Dodaj autoryzację</span><span class="sxs-lookup"><span data-stu-id="2815d-132">Add Authorization</span></span>

<span data-ttu-id="2815d-133">Strona administratora jest obecnie dostępna dla wszystkich osób odwiedzających witrynę.</span><span class="sxs-lookup"><span data-stu-id="2815d-133">The Admin page is currently accessible to anyone visiting the site.</span></span> <span data-ttu-id="2815d-134">Zmieńmy ten sposób, aby ograniczyć uprawnienia do administratorów.</span><span class="sxs-lookup"><span data-stu-id="2815d-134">Let's change this to restrict permission to administrators.</span></span>

<span data-ttu-id="2815d-135">Zacznij od dodania roli "Administrator" i administratora.</span><span class="sxs-lookup"><span data-stu-id="2815d-135">Start by adding an "Administrator" role and an administrator user.</span></span> <span data-ttu-id="2815d-136">W Eksplorator rozwiązań rozwiń folder filtry i Otwórz plik o nazwie InitializeSimpleMembershipAttribute.cs.</span><span class="sxs-lookup"><span data-stu-id="2815d-136">In Solution Explorer, expand the Filters folder and open the file named InitializeSimpleMembershipAttribute.cs.</span></span> <span data-ttu-id="2815d-137">Znajdź konstruktora `SimpleMembershipInitializer`.</span><span class="sxs-lookup"><span data-stu-id="2815d-137">Locate the `SimpleMembershipInitializer` constructor.</span></span> <span data-ttu-id="2815d-138">Po wywołaniu metody **WebSecurity. InitializeDatabaseConnection**Dodaj następujący kod:</span><span class="sxs-lookup"><span data-stu-id="2815d-138">After the call to **WebSecurity.InitializeDatabaseConnection**, add the following code:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample4.cs)]

<span data-ttu-id="2815d-139">Jest to szybka i zanieczyszczona metoda dodawania roli "Administrator" i tworzenia użytkownika dla roli.</span><span class="sxs-lookup"><span data-stu-id="2815d-139">This is a quick-and-dirty way to add the "Administrator" role and create a user for the role.</span></span>

<span data-ttu-id="2815d-140">W Eksplorator rozwiązań rozwiń folder kontrolery i Otwórz plik HomeController.cs.</span><span class="sxs-lookup"><span data-stu-id="2815d-140">In Solution Explorer, expand the Controllers folder and open the HomeController.cs file.</span></span> <span data-ttu-id="2815d-141">Dodaj atrybut **Autoryzuj** do metody `Admin`.</span><span class="sxs-lookup"><span data-stu-id="2815d-141">Add the **Authorize** attribute to the `Admin` method.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample5.cs)]

<span data-ttu-id="2815d-142">Otwórz plik AdminController.cs i Dodaj atrybut **Autoryzuj** do całej klasy `AdminController`.</span><span class="sxs-lookup"><span data-stu-id="2815d-142">Open the AdminController.cs file and add the **Authorize** attribute to the entire `AdminController` class.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample6.cs)]

> [!NOTE]
> <span data-ttu-id="2815d-143">Zarówno MVC, jak i interfejs API sieci Web definiują atrybuty **autoryzacji** w różnych przestrzeniach nazw.</span><span class="sxs-lookup"><span data-stu-id="2815d-143">MVC and Web API both define **Authorize** attributes, in different namespaces.</span></span> <span data-ttu-id="2815d-144">MVC używa **System. Web. MVC. AuthorizeAttribute**, a interfejs API sieci Web używa **System. Web. http. AuthorizeAttribute**.</span><span class="sxs-lookup"><span data-stu-id="2815d-144">MVC uses **System.Web.Mvc.AuthorizeAttribute**, while Web API uses **System.Web.Http.AuthorizeAttribute**.</span></span>

<span data-ttu-id="2815d-145">Teraz tylko Administratorzy mogą wyświetlać stronę administratora.</span><span class="sxs-lookup"><span data-stu-id="2815d-145">Now only administrators can view the Admin page.</span></span> <span data-ttu-id="2815d-146">Ponadto w przypadku wysłania żądania HTTP do kontrolera administratora żądanie musi zawierać plik cookie uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="2815d-146">Also, if you send an HTTP request to the Admin controller, the request must contain an authentication cookie.</span></span> <span data-ttu-id="2815d-147">W przeciwnym razie serwer wysyła odpowiedź HTTP 401 (bez autoryzacji).</span><span class="sxs-lookup"><span data-stu-id="2815d-147">If not, the server sends an HTTP 401 (Unauthorized) response.</span></span> <span data-ttu-id="2815d-148">Ten element można zobaczyć w programu Fiddler, wysyłając żądanie GET do `http://localhost:*port*/api/admin`.</span><span class="sxs-lookup"><span data-stu-id="2815d-148">You can see this in Fiddler by sending a GET request to `http://localhost:*port*/api/admin`.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2815d-149">[Poprzednie](using-web-api-with-entity-framework-part-3.md)
> [dalej](using-web-api-with-entity-framework-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="2815d-149">[Previous](using-web-api-with-entity-framework-part-3.md)
[Next](using-web-api-with-entity-framework-part-5.md)</span></span>

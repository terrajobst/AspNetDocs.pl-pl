---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
title: Część 4. Dodawanie widoku administratora | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 792f4513-a508-4d14-a0dd-1a2fe282c7bb
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
msc.type: authoredcontent
ms.openlocfilehash: 54b3afac9b19962b02336a35909b208c4e3f7504
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/09/2019
ms.locfileid: "59400558"
---
# <a name="part-4-adding-an-admin-view"></a><span data-ttu-id="1740e-102">Część 4. Dodawanie widoku administratora</span><span class="sxs-lookup"><span data-stu-id="1740e-102">Part 4: Adding an Admin View</span></span>

<span data-ttu-id="1740e-103">przez [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="1740e-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="1740e-104">Pobieranie ukończone projektu</span><span class="sxs-lookup"><span data-stu-id="1740e-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-view"></a><span data-ttu-id="1740e-105">Dodawanie widoku administratora</span><span class="sxs-lookup"><span data-stu-id="1740e-105">Add an Admin View</span></span>

<span data-ttu-id="1740e-106">Teraz utworzymy sięgają po stronie klienta i dodamy strony, którą może wykorzystać dane z kontrolera administratora.</span><span class="sxs-lookup"><span data-stu-id="1740e-106">Now we'll turn to the client side, and add a page that can consume data from the Admin controller.</span></span> <span data-ttu-id="1740e-107">Strona Użytkownicy mogą tworzyć, edytować lub usunąć produktów, przez wysyłanie żądań AJAX do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="1740e-107">The page will allow users to create, edit, or delete products, by sending AJAX requests to the controller.</span></span>

<span data-ttu-id="1740e-108">W Eksploratorze rozwiązań rozwiń folder kontrolerów, a następnie otwórz plik o nazwie HomeController.cs.</span><span class="sxs-lookup"><span data-stu-id="1740e-108">In Solution Explorer, expand the Controllers folder and open the file named HomeController.cs.</span></span> <span data-ttu-id="1740e-109">Ten plik zawiera kontroler MVC.</span><span class="sxs-lookup"><span data-stu-id="1740e-109">This file contains an MVC controller.</span></span> <span data-ttu-id="1740e-110">Dodaj metodę o nazwie `Admin`:</span><span class="sxs-lookup"><span data-stu-id="1740e-110">Add a method named `Admin`:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample1.cs)]

<span data-ttu-id="1740e-111">**HttpRouteUrl** metoda tworzy identyfikator URI w internetowym interfejsie API i przechowujemy to w zbiorze widok na później.</span><span class="sxs-lookup"><span data-stu-id="1740e-111">The **HttpRouteUrl** method creates the URI to the web API, and we store this in the view bag for later.</span></span>

<span data-ttu-id="1740e-112">Następnie umieść kursor tekstu w obrębie `Admin` metody akcji, a następnie kliknij prawym przyciskiem myszy i wybierz pozycję **Dodaj widok**.</span><span class="sxs-lookup"><span data-stu-id="1740e-112">Next, position the text cursor within the `Admin` action method, then right-click and select **Add View**.</span></span> <span data-ttu-id="1740e-113">Zostanie wyświetlone okno **Dodaj widok** okna dialogowego.</span><span class="sxs-lookup"><span data-stu-id="1740e-113">This will bring up the **Add View** dialog.</span></span>

![](using-web-api-with-entity-framework-part-4/_static/image1.png)

<span data-ttu-id="1740e-114">W **Dodaj widok** okno dialogowe, nazwę widoku, "Admin".</span><span class="sxs-lookup"><span data-stu-id="1740e-114">In the **Add View** dialog, name the view "Admin".</span></span> <span data-ttu-id="1740e-115">Zaznacz pole wyboru przy opcji **utworzyć widok silnie typizowane**.</span><span class="sxs-lookup"><span data-stu-id="1740e-115">Select the check box labeled **Create a strongly-typed view**.</span></span> <span data-ttu-id="1740e-116">W obszarze **klasy modelu**, wybierz pozycję "Product (ProductStore.Models)".</span><span class="sxs-lookup"><span data-stu-id="1740e-116">Under **Model Class**, select "Product (ProductStore.Models)".</span></span> <span data-ttu-id="1740e-117">Pozostaw wartości domyślne innych opcji.</span><span class="sxs-lookup"><span data-stu-id="1740e-117">Leave all the other options as their default values.</span></span>

![](using-web-api-with-entity-framework-part-4/_static/image2.png)

<span data-ttu-id="1740e-118">Klikając **Dodaj** dodaje plik o nazwie Admin.cshtml widoków/głównej.</span><span class="sxs-lookup"><span data-stu-id="1740e-118">Clicking **Add** adds a file named Admin.cshtml under Views/Home.</span></span> <span data-ttu-id="1740e-119">Otwórz ten plik i Dodaj poniższy kod HTML.</span><span class="sxs-lookup"><span data-stu-id="1740e-119">Open this file and add the following HTML.</span></span> <span data-ttu-id="1740e-120">Kod HTML definiuje strukturę strony, ale funkcjonalność nie jest jeszcze gotowe.</span><span class="sxs-lookup"><span data-stu-id="1740e-120">This HTML defines the structure of the page, but no functionality is wired up yet.</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample2.cshtml)]

## <a name="create-a-link-to-the-admin-page"></a><span data-ttu-id="1740e-121">Utwórz łącze do strony administratora</span><span class="sxs-lookup"><span data-stu-id="1740e-121">Create a Link to the Admin Page</span></span>

<span data-ttu-id="1740e-122">W Eksploratorze rozwiązań rozwiń folder widoki, a następnie rozwiń folder udostępniony.</span><span class="sxs-lookup"><span data-stu-id="1740e-122">In Solution Explorer, expand the Views folder and then expand the Shared folder.</span></span> <span data-ttu-id="1740e-123">Otwórz plik o nazwie \_Layout.cshtml.</span><span class="sxs-lookup"><span data-stu-id="1740e-123">Open the file named \_Layout.cshtml.</span></span> <span data-ttu-id="1740e-124">Znajdź **ul** element o identyfikatorze = "menu", a łącze akcji dla widoku administratora:</span><span class="sxs-lookup"><span data-stu-id="1740e-124">Locate the **ul** element with id = "menu", and an action link for the Admin view:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample3.cshtml)]

> [!NOTE]
> <span data-ttu-id="1740e-125">W przykładowym projekcie kilka innych kosmetycznych zmiany wprowadzone, takich jak zastępując ciąg "Twoje logo".</span><span class="sxs-lookup"><span data-stu-id="1740e-125">In the sample project, I made a few other cosmetic changes, such as replacing the string "Your logo here".</span></span> <span data-ttu-id="1740e-126">Nie mają one wpływu na funkcjonalność aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1740e-126">These don't affect the functionality of the application.</span></span> <span data-ttu-id="1740e-127">Można pobrać projektu i porównywanie plików.</span><span class="sxs-lookup"><span data-stu-id="1740e-127">You can download the project and compare the files.</span></span>


<span data-ttu-id="1740e-128">Uruchom aplikację, a następnie kliknij link "Admin", który pojawia się w górnej części strony głównej.</span><span class="sxs-lookup"><span data-stu-id="1740e-128">Run the application and click the "Admin" link that appears at the top of the home page.</span></span> <span data-ttu-id="1740e-129">Strona administratora powinny wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="1740e-129">The Admin page should look like the following:</span></span>

![](using-web-api-with-entity-framework-part-4/_static/image3.png)

<span data-ttu-id="1740e-130">Po prawej stronie teraz strony nie wprowadza żadnych zmian.</span><span class="sxs-lookup"><span data-stu-id="1740e-130">Right now, the page doesn't do anything.</span></span> <span data-ttu-id="1740e-131">W następnej sekcji użyjemy struktura Knockout.js do tworzenia dynamicznego interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="1740e-131">In the next section, we'll use Knockout.js to create a dynamic UI.</span></span>

## <a name="add-authorization"></a><span data-ttu-id="1740e-132">Dodaj autoryzację</span><span class="sxs-lookup"><span data-stu-id="1740e-132">Add Authorization</span></span>

<span data-ttu-id="1740e-133">Strona administratora jest obecnie dostępna dla każdego, kto w witrynie.</span><span class="sxs-lookup"><span data-stu-id="1740e-133">The Admin page is currently accessible to anyone visiting the site.</span></span> <span data-ttu-id="1740e-134">Zmieńmy ten element, aby ograniczyć uprawnienia do administratorów.</span><span class="sxs-lookup"><span data-stu-id="1740e-134">Let's change this to restrict permission to administrators.</span></span>

<span data-ttu-id="1740e-135">Rozpocznij, dodając rolę "Administrator" i użytkownika administrator.</span><span class="sxs-lookup"><span data-stu-id="1740e-135">Start by adding an "Administrator" role and an administrator user.</span></span> <span data-ttu-id="1740e-136">W Eksploratorze rozwiązań rozwiń folder filtry, a następnie otwórz plik o nazwie InitializeSimpleMembershipAttribute.cs.</span><span class="sxs-lookup"><span data-stu-id="1740e-136">In Solution Explorer, expand the Filters folder and open the file named InitializeSimpleMembershipAttribute.cs.</span></span> <span data-ttu-id="1740e-137">Znajdź `SimpleMembershipInitializer` konstruktora.</span><span class="sxs-lookup"><span data-stu-id="1740e-137">Locate the `SimpleMembershipInitializer` constructor.</span></span> <span data-ttu-id="1740e-138">Po wywołaniu **WebSecurity.InitializeDatabaseConnection**, Dodaj następujący kod:</span><span class="sxs-lookup"><span data-stu-id="1740e-138">After the call to **WebSecurity.InitializeDatabaseConnection**, add the following code:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample4.cs)]

<span data-ttu-id="1740e-139">Jest quick-and-dirty sposób, aby dodać rolę "Administrator" i Utwórz użytkownika dla roli.</span><span class="sxs-lookup"><span data-stu-id="1740e-139">This is a quick-and-dirty way to add the "Administrator" role and create a user for the role.</span></span>

<span data-ttu-id="1740e-140">W Eksploratorze rozwiązań rozwiń folder kontrolerów, a następnie otwórz plik HomeController.cs.</span><span class="sxs-lookup"><span data-stu-id="1740e-140">In Solution Explorer, expand the Controllers folder and open the HomeController.cs file.</span></span> <span data-ttu-id="1740e-141">Dodaj **Autoryzuj** atrybutu `Admin` metody.</span><span class="sxs-lookup"><span data-stu-id="1740e-141">Add the **Authorize** attribute to the `Admin` method.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample5.cs)]

<span data-ttu-id="1740e-142">Otwórz plik AdminController.cs i Dodaj **Autoryzuj** atrybutu do całej `AdminController` klasy.</span><span class="sxs-lookup"><span data-stu-id="1740e-142">Open the AdminController.cs file and add the **Authorize** attribute to the entire `AdminController` class.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample6.cs)]

> [!NOTE]
> <span data-ttu-id="1740e-143">MVC i internetowy interfejs API zarówno zdefiniować **Autoryzuj** atrybutów w różnych obszarach nazw.</span><span class="sxs-lookup"><span data-stu-id="1740e-143">MVC and Web API both define **Authorize** attributes, in different namespaces.</span></span> <span data-ttu-id="1740e-144">Używa MVC **System.Web.Mvc.AuthorizeAttribute**, podczas gdy interfejs API sieci Web używa **System.Web.Http.AuthorizeAttribute**.</span><span class="sxs-lookup"><span data-stu-id="1740e-144">MVC uses **System.Web.Mvc.AuthorizeAttribute**, while Web API uses **System.Web.Http.AuthorizeAttribute**.</span></span>


<span data-ttu-id="1740e-145">Tylko administratorzy mogą teraz wyświetlać strony administratora.</span><span class="sxs-lookup"><span data-stu-id="1740e-145">Now only administrators can view the Admin page.</span></span> <span data-ttu-id="1740e-146">Ponadto Jeśli wyślesz żądanie HTTP do kontrolera administratora żądanie musi zawierać plik cookie uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="1740e-146">Also, if you send an HTTP request to the Admin controller, the request must contain an authentication cookie.</span></span> <span data-ttu-id="1740e-147">Jeśli nie, serwer wysyła odpowiedź HTTP 401 (bez autoryzacji).</span><span class="sxs-lookup"><span data-stu-id="1740e-147">If not, the server sends an HTTP 401 (Unauthorized) response.</span></span> <span data-ttu-id="1740e-148">Można to zobaczyć w narzędziu Fiddler, wysyłając żądanie GET w celu `http://localhost:*port*/api/admin`.</span><span class="sxs-lookup"><span data-stu-id="1740e-148">You can see this in Fiddler by sending a GET request to `http://localhost:*port*/api/admin`.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="1740e-149">[Poprzednie](using-web-api-with-entity-framework-part-3.md)
> [dalej](using-web-api-with-entity-framework-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="1740e-149">[Previous](using-web-api-with-entity-framework-part-3.md)
[Next](using-web-api-with-entity-framework-part-5.md)</span></span>

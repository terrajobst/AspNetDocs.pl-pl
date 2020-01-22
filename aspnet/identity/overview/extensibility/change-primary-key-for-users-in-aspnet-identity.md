---
uid: identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
title: Zmień klucz podstawowy dla użytkowników w ASP.NET Identity ASP.NET 4. x
author: Rick-Anderson
description: W Visual Studio 2013 domyślna aplikacja sieci Web używa wartości ciągu klucza dla kont użytkowników. ASP.NET Identity umożliwia zmianę typu...
ms.author: riande
ms.date: 09/30/2014
ms.assetid: 44925849-5762-4504-a8cd-8f0cd06f6dc3
ms.custom: seoapril2019
msc.legacyurl: /identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 0afea8eacfc646f1489b87629fdb2d437815d88c
ms.sourcegitcommit: 88fc80e3f65aebdf61ec9414810ddbc31c543f04
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/22/2020
ms.locfileid: "76519144"
---
# <a name="change-primary-key-for-users-in-aspnet-identity"></a><span data-ttu-id="a2042-104">Zmiana klucza podstawowego dla użytkowników w systemie ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="a2042-104">Change Primary Key for Users in ASP.NET Identity</span></span>

<span data-ttu-id="a2042-105">przez [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="a2042-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="a2042-106">W Visual Studio 2013 domyślna aplikacja sieci Web używa wartości ciągu klucza dla kont użytkowników.</span><span class="sxs-lookup"><span data-stu-id="a2042-106">In Visual Studio 2013, the default web application uses a string value for the key for user accounts.</span></span> <span data-ttu-id="a2042-107">ASP.NET Identity umożliwia zmianę typu klucza w celu spełnienia wymagań dotyczących danych.</span><span class="sxs-lookup"><span data-stu-id="a2042-107">ASP.NET Identity enables you to change the type of the key to meet your data requirements.</span></span> <span data-ttu-id="a2042-108">Na przykład można zmienić typ klucza z ciągu na liczbę całkowitą.</span><span class="sxs-lookup"><span data-stu-id="a2042-108">For example, you can change the type of the key from a string to an integer.</span></span>
> 
> <span data-ttu-id="a2042-109">W tym temacie pokazano, jak rozpocząć od domyślnej aplikacji sieci Web i zmienić klucz konta użytkownika na liczbę całkowitą.</span><span class="sxs-lookup"><span data-stu-id="a2042-109">This topic shows how to start with the default web application and change the user account key to an integer.</span></span> <span data-ttu-id="a2042-110">Można użyć tych samych modyfikacji w celu zaimplementowania dowolnego typu klucza w projekcie.</span><span class="sxs-lookup"><span data-stu-id="a2042-110">You can use the same modifications to implement any type of key in your project.</span></span> <span data-ttu-id="a2042-111">Pokazuje, jak wprowadzić te zmiany w domyślnej aplikacji sieci Web, ale można zastosować podobne modyfikacje do niestandardowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a2042-111">It shows how to make these changes in the default web application, but you could apply similar modifications to a customized application.</span></span> <span data-ttu-id="a2042-112">Pokazuje zmiany, które są niezbędne podczas pracy z kontrolerami MVC lub Web Forms.</span><span class="sxs-lookup"><span data-stu-id="a2042-112">It shows the changes needed when working with MVC or Web Forms.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="a2042-113">Wersje oprogramowania używane w samouczku</span><span class="sxs-lookup"><span data-stu-id="a2042-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="a2042-114">Visual Studio 2013 z aktualizacją Update 2 (lub nowszą)</span><span class="sxs-lookup"><span data-stu-id="a2042-114">Visual Studio 2013 with Update 2 (or later)</span></span>
> - <span data-ttu-id="a2042-115">ASP.NET Identity 2,1 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="a2042-115">ASP.NET Identity 2.1 or later</span></span>

<span data-ttu-id="a2042-116">Aby wykonać kroki opisane w tym samouczku, musisz mieć Visual Studio 2013 Update 2 (lub nowsze wersje) oraz aplikację sieci Web utworzoną na podstawie szablonu aplikacji sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="a2042-116">To perform the steps in this tutorial, you must have Visual Studio 2013 Update 2 (or later), and a web application created from the ASP.NET Web Application template.</span></span> <span data-ttu-id="a2042-117">Szablon został zmieniony w Update 3.</span><span class="sxs-lookup"><span data-stu-id="a2042-117">The template changed in Update 3.</span></span> <span data-ttu-id="a2042-118">W tym temacie pokazano, jak zmienić szablon w Update 2 i Update 3.</span><span class="sxs-lookup"><span data-stu-id="a2042-118">This topic shows how to change the template in Update 2 and Update 3.</span></span>

<span data-ttu-id="a2042-119">Ten temat zawiera następujące sekcje:</span><span class="sxs-lookup"><span data-stu-id="a2042-119">This topic contains the following sections:</span></span>

- [<span data-ttu-id="a2042-120">Zmień typ klucza w klasie User Identity</span><span class="sxs-lookup"><span data-stu-id="a2042-120">Change the type of the key in the Identity user class</span></span>](#userclass)
- [<span data-ttu-id="a2042-121">Dodawanie niestandardowych klas tożsamości, które używają typu klucza</span><span class="sxs-lookup"><span data-stu-id="a2042-121">Add customized Identity classes that use the key type</span></span>](#customclass)
- [<span data-ttu-id="a2042-122">Zmień klasę kontekstu i Menedżera użytkowników, aby używały typu klucza</span><span class="sxs-lookup"><span data-stu-id="a2042-122">Change the context class and user manager to use the key type</span></span>](#context)
- [<span data-ttu-id="a2042-123">Zmień konfigurację początkową, aby użyć typu klucza</span><span class="sxs-lookup"><span data-stu-id="a2042-123">Change start-up configuration to use the key type</span></span>](#startup)
- [<span data-ttu-id="a2042-124">W przypadku składnika MVC z aktualizacją Update 2 Zmień elementu AccountController, aby przekazać typ klucza</span><span class="sxs-lookup"><span data-stu-id="a2042-124">For MVC with Update 2, change the AccountController to pass the key type</span></span>](#mvcupdate2)
- [<span data-ttu-id="a2042-125">W przypadku składnika MVC z aktualizacją Update 3 Zmień wartość elementu AccountController i ManageController, aby przekazać typ klucza</span><span class="sxs-lookup"><span data-stu-id="a2042-125">For MVC with Update 3, change the AccountController and ManageController to pass the key type</span></span>](#mvcupdate3)
- [<span data-ttu-id="a2042-126">W przypadku formularzy sieci Web z aktualizacją Update 2 Zmień strony kont w celu przekazania typu klucza</span><span class="sxs-lookup"><span data-stu-id="a2042-126">For Web Forms with Update 2, change Account pages to pass the key type</span></span>](#webformsupdate2)
- [<span data-ttu-id="a2042-127">W przypadku formularzy sieci Web z aktualizacją Update 3 Zmień strony kont w celu przekazania typu klucza</span><span class="sxs-lookup"><span data-stu-id="a2042-127">For Web Forms with Update 3, change Account pages to pass the key type</span></span>](#webformsupdate3)
- [<span data-ttu-id="a2042-128">Uruchom aplikację</span><span class="sxs-lookup"><span data-stu-id="a2042-128">Run application</span></span>](#run)
- [<span data-ttu-id="a2042-129">Inne zasoby</span><span class="sxs-lookup"><span data-stu-id="a2042-129">Other resources</span></span>](#other)

<a id="userclass"></a>
## <a name="change-the-type-of-the-key-in-the-identity-user-class"></a><span data-ttu-id="a2042-130">Zmień typ klucza w klasie User Identity</span><span class="sxs-lookup"><span data-stu-id="a2042-130">Change the type of the key in the Identity user class</span></span>

<span data-ttu-id="a2042-131">W projekcie utworzonym na podstawie szablonu aplikacji sieci Web ASP.NET Określ, że Klasa ApplicationUser używa liczby całkowitej dla klucza dla kont użytkowników.</span><span class="sxs-lookup"><span data-stu-id="a2042-131">In your project created from the ASP.NET Web Application template, specify that the ApplicationUser class uses an integer for the key for user accounts.</span></span> <span data-ttu-id="a2042-132">W IdentityModels.cs Zmień klasę ApplicationUser, aby dziedziczyć z IdentityUser, która ma typ **int** dla parametru ogólnego TKey.</span><span class="sxs-lookup"><span data-stu-id="a2042-132">In IdentityModels.cs, change the ApplicationUser class to inherit from IdentityUser that has a type of **int** for the TKey generic parameter.</span></span> <span data-ttu-id="a2042-133">Można również przekazać nazwy trzech dostosowanych klas, które jeszcze nie zostały zaimplementowane.</span><span class="sxs-lookup"><span data-stu-id="a2042-133">You also pass the names of three customized class which you have not implemented yet.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample1.cs?highlight=1-2)]

<span data-ttu-id="a2042-134">Typ klucza został zmieniony, ale domyślnie reszta aplikacji nadal zakłada, że klucz jest ciągiem.</span><span class="sxs-lookup"><span data-stu-id="a2042-134">You have changed the type of the key, but, by default, the rest of the application still assumes the key is a string.</span></span> <span data-ttu-id="a2042-135">Należy jawnie wskazać typ klucza w kodzie, który przyjmuje ciąg.</span><span class="sxs-lookup"><span data-stu-id="a2042-135">You must explicitly indicate the type of the key in code that assumes a string.</span></span>

<span data-ttu-id="a2042-136">W klasie **ApplicationUser** Zmień metodę **GenerateUserIdentityAsync** , tak aby obejmowała int, jak pokazano w wyróżnionym kodzie poniżej.</span><span class="sxs-lookup"><span data-stu-id="a2042-136">In the **ApplicationUser** class, change the **GenerateUserIdentityAsync** method to include int, as shown in the highlighted code below.</span></span> <span data-ttu-id="a2042-137">Ta zmiana nie jest wymagana w przypadku projektów formularzy sieci Web z szablonem Update 3.</span><span class="sxs-lookup"><span data-stu-id="a2042-137">This change is not necessary for Web Forms projects with the Update 3 template.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample2.cs?highlight=2)]

<a id="customclass"></a>
## <a name="add-customized-identity-classes-that-use-the-key-type"></a><span data-ttu-id="a2042-138">Dodawanie niestandardowych klas tożsamości, które używają typu klucza</span><span class="sxs-lookup"><span data-stu-id="a2042-138">Add customized Identity classes that use the key type</span></span>

<span data-ttu-id="a2042-139">Inne klasy tożsamości, takie jak IdentityUserRole, IdentityUserClaim, IdentityUserLogin, IdentityRole, UserStore, RoleStore, są nadal skonfigurowane do używania klucza ciągu.</span><span class="sxs-lookup"><span data-stu-id="a2042-139">The other Identity classes, such as IdentityUserRole, IdentityUserClaim, IdentityUserLogin, IdentityRole, UserStore, RoleStore, are still set up to use a string key.</span></span> <span data-ttu-id="a2042-140">Utwórz nowe wersje tych klas, które określają liczbę całkowitą dla klucza.</span><span class="sxs-lookup"><span data-stu-id="a2042-140">Create new versions of these classes that specify an integer for the key.</span></span> <span data-ttu-id="a2042-141">W tych klasach nie trzeba podawać wielu kodów implementacji, ale wystarczy tylko ustawić int jako klucz.</span><span class="sxs-lookup"><span data-stu-id="a2042-141">You do not need to provide much implementation code in these classes, you are primarily just setting int as the key.</span></span>

<span data-ttu-id="a2042-142">Dodaj następujące klasy do pliku IdentityModels.cs.</span><span class="sxs-lookup"><span data-stu-id="a2042-142">Add the following classes to your IdentityModels.cs file.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample3.cs)]

<a id="context"></a>
## <a name="change-the-context-class-and-user-manager-to-use-the-key-type"></a><span data-ttu-id="a2042-143">Zmień klasę kontekstu i Menedżera użytkowników, aby używały typu klucza</span><span class="sxs-lookup"><span data-stu-id="a2042-143">Change the context class and user manager to use the key type</span></span>

<span data-ttu-id="a2042-144">W IdentityModels.cs Zmień definicję klasy **ApplicationDbContext** , tak aby korzystała z nowych niestandardowych klas i **int** dla klucza, jak pokazano w wyróżnionym kodzie.</span><span class="sxs-lookup"><span data-stu-id="a2042-144">In IdentityModels.cs, change the definition of the **ApplicationDbContext** class to use your new customized classes and an **int** for the key, as shown in the highlighted code.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample4.cs?highlight=1-2)]

<span data-ttu-id="a2042-145">Parametr ThrowIfV1Schema nie jest już prawidłowy w konstruktorze.</span><span class="sxs-lookup"><span data-stu-id="a2042-145">The ThrowIfV1Schema parameter is no longer valid in the constructor.</span></span> <span data-ttu-id="a2042-146">Zmień konstruktora, aby nie przeszedł wartości ThrowIfV1Schema.</span><span class="sxs-lookup"><span data-stu-id="a2042-146">Change the constructor so it does not pass a ThrowIfV1Schema value.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample5.cs)]

<span data-ttu-id="a2042-147">Otwórz IdentityConfig.cs i Zmień klasę **ApplicationUserManger** , tak aby używała nowej klasy magazynu użytkownika do utrwalania danych i **int** dla klucza.</span><span class="sxs-lookup"><span data-stu-id="a2042-147">Open IdentityConfig.cs, and change the **ApplicationUserManger** class to use your new user store class for persisting data and an **int** for the key.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample6.cs?highlight=1,3,12,14,32,37,48)]

<span data-ttu-id="a2042-148">W szablonie Update 3 należy zmienić klasę ApplicationSignInManager.</span><span class="sxs-lookup"><span data-stu-id="a2042-148">In the Update 3 template, you must change the ApplicationSignInManager class.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample7.cs?highlight=1)]

<a id="startup"></a>
## <a name="change-start-up-configuration-to-use-the-key-type"></a><span data-ttu-id="a2042-149">Zmień konfigurację początkową, aby użyć typu klucza</span><span class="sxs-lookup"><span data-stu-id="a2042-149">Change start-up configuration to use the key type</span></span>

<span data-ttu-id="a2042-150">W Startup.Auth.cs Zastąp kod OnValidateIdentity jako wyróżniony poniżej.</span><span class="sxs-lookup"><span data-stu-id="a2042-150">In Startup.Auth.cs, replace the OnValidateIdentity code, as highlighted below.</span></span> <span data-ttu-id="a2042-151">Należy zauważyć, że definicja getUserIdCallback analizuje wartość ciągu na liczbę całkowitą.</span><span class="sxs-lookup"><span data-stu-id="a2042-151">Notice that the getUserIdCallback definition, parses the string value into an integer.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample8.cs?highlight=7-12)]

<span data-ttu-id="a2042-152">Jeśli projekt nie rozpoznaje ogólnej implementacji metody **GetUserID** , może być konieczne zaktualizowanie ASP.NET Identity pakietu NuGet do wersji 2,1</span><span class="sxs-lookup"><span data-stu-id="a2042-152">If your project does not recognize the generic implementation of the **GetUserId** method, you may need to update the ASP.NET Identity NuGet package to version 2.1</span></span>

<span data-ttu-id="a2042-153">Wprowadzono wiele zmian klas infrastruktury używanych przez ASP.NET Identity.</span><span class="sxs-lookup"><span data-stu-id="a2042-153">You have made a lot of changes to the infrastructure classes used by ASP.NET Identity.</span></span> <span data-ttu-id="a2042-154">Jeśli spróbujesz skompilować projekt, zobaczysz wiele błędów.</span><span class="sxs-lookup"><span data-stu-id="a2042-154">If you try compiling the project, you will notice a lot of errors.</span></span> <span data-ttu-id="a2042-155">Na szczęście pozostałe błędy są podobne.</span><span class="sxs-lookup"><span data-stu-id="a2042-155">Fortunately, the remaining errors are all similar.</span></span> <span data-ttu-id="a2042-156">Klasa Identity oczekuje liczby całkowitej dla klucza, ale kontroler (lub formularz sieci Web) przekazuje wartość ciągu.</span><span class="sxs-lookup"><span data-stu-id="a2042-156">The Identity class expects an integer for the key, but the controller (or Web Form) is passing a string value.</span></span> <span data-ttu-id="a2042-157">W każdym przypadku należy przekonwertować ciąg na i liczbę całkowitą, wywołując metodę **GetUserID&lt;int&gt;** .</span><span class="sxs-lookup"><span data-stu-id="a2042-157">In each case, you need to convert from a string to and integer by calling **GetUserId&lt;int&gt;**.</span></span> <span data-ttu-id="a2042-158">Możesz przejść przez listę błędów z kompilacji lub wykonać poniższe zmiany.</span><span class="sxs-lookup"><span data-stu-id="a2042-158">You can either work through the error list from compilation or follow the changes below.</span></span>

<span data-ttu-id="a2042-159">Pozostałe zmiany są zależne od typu tworzonego projektu i aktualizacji zainstalowanego w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a2042-159">The remaining changes depend on the type of project you are creating and which update you have installed in Visual Studio.</span></span> <span data-ttu-id="a2042-160">Możesz przejść bezpośrednio do odpowiedniej sekcji, korzystając z następujących linków</span><span class="sxs-lookup"><span data-stu-id="a2042-160">You can go directly to the relevant section through the following links</span></span>

- [<span data-ttu-id="a2042-161">W przypadku składnika MVC z aktualizacją Update 2 Zmień elementu AccountController, aby przekazać typ klucza</span><span class="sxs-lookup"><span data-stu-id="a2042-161">For MVC with Update 2, change the AccountController to pass the key type</span></span>](#mvcupdate2)
- [<span data-ttu-id="a2042-162">W przypadku składnika MVC z aktualizacją Update 3 Zmień wartość elementu AccountController i ManageController, aby przekazać typ klucza</span><span class="sxs-lookup"><span data-stu-id="a2042-162">For MVC with Update 3, change the AccountController and ManageController to pass the key type</span></span>](#mvcupdate3)
- [<span data-ttu-id="a2042-163">W przypadku formularzy sieci Web z aktualizacją Update 2 Zmień strony kont w celu przekazania typu klucza</span><span class="sxs-lookup"><span data-stu-id="a2042-163">For Web Forms with Update 2, change Account pages to pass the key type</span></span>](#webformsupdate2)
- [<span data-ttu-id="a2042-164">W przypadku formularzy sieci Web z aktualizacją Update 3 Zmień strony kont w celu przekazania typu klucza</span><span class="sxs-lookup"><span data-stu-id="a2042-164">For Web Forms with Update 3, change Account pages to pass the key type</span></span>](#webformsupdate3)

<a id="mvcupdate2"></a>
## <a name="for-mvc-with-update-2-change-the-accountcontroller-to-pass-the-key-type"></a><span data-ttu-id="a2042-165">W przypadku składnika MVC z aktualizacją Update 2 Zmień elementu AccountController, aby przekazać typ klucza</span><span class="sxs-lookup"><span data-stu-id="a2042-165">For MVC with Update 2, change the AccountController to pass the key type</span></span>

<span data-ttu-id="a2042-166">Otwórz plik AccountController.cs.</span><span class="sxs-lookup"><span data-stu-id="a2042-166">Open the AccountController.cs file.</span></span> <span data-ttu-id="a2042-167">Należy zmienić następujące metody.</span><span class="sxs-lookup"><span data-stu-id="a2042-167">You need to change the following methods.</span></span>

<span data-ttu-id="a2042-168">**ConfirmEmail** , Metoda</span><span class="sxs-lookup"><span data-stu-id="a2042-168">**ConfirmEmail** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample9.cs?highlight=1,3)]

<span data-ttu-id="a2042-169">**Usuń skojarzenie** metody</span><span class="sxs-lookup"><span data-stu-id="a2042-169">**Disassociate** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample10.cs?highlight=5,9)]

<span data-ttu-id="a2042-170">**Zarządzanie (ManageUserViewModel)** — Metoda</span><span class="sxs-lookup"><span data-stu-id="a2042-170">**Manage(ManageUserViewModel)** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample11.cs?highlight=11,17,41)]

<span data-ttu-id="a2042-171">**LinkLoginCallback** , Metoda</span><span class="sxs-lookup"><span data-stu-id="a2042-171">**LinkLoginCallback** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample12.cs?highlight=10)]

<span data-ttu-id="a2042-172">**RemoveAccountList** , Metoda</span><span class="sxs-lookup"><span data-stu-id="a2042-172">**RemoveAccountList** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample13.cs?highlight=3)]

<span data-ttu-id="a2042-173">**HasPassword** , Metoda</span><span class="sxs-lookup"><span data-stu-id="a2042-173">**HasPassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample14.cs?highlight=3)]

<span data-ttu-id="a2042-174">Teraz możesz [uruchomić aplikację](#run) i zarejestrować nowego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="a2042-174">You can now [run the application](#run) and register a new user.</span></span>

<a id="mvcupdate3"></a>
## <a name="for-mvc-with-update-3-change-the-accountcontroller-and-managecontroller-to-pass-the-key-type"></a><span data-ttu-id="a2042-175">W przypadku składnika MVC z aktualizacją Update 3 Zmień wartość elementu AccountController i ManageController, aby przekazać typ klucza</span><span class="sxs-lookup"><span data-stu-id="a2042-175">For MVC with Update 3, change the AccountController and ManageController to pass the key type</span></span>

<span data-ttu-id="a2042-176">Otwórz plik AccountController.cs.</span><span class="sxs-lookup"><span data-stu-id="a2042-176">Open the AccountController.cs file.</span></span> <span data-ttu-id="a2042-177">Należy zmienić następującą metodę.</span><span class="sxs-lookup"><span data-stu-id="a2042-177">You need to change the following method.</span></span>

<span data-ttu-id="a2042-178">**ConfirmEmail** , Metoda</span><span class="sxs-lookup"><span data-stu-id="a2042-178">**ConfirmEmail** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample15.cs?highlight=1,3)]

<span data-ttu-id="a2042-179">**Kontrolka sendcode** , Metoda</span><span class="sxs-lookup"><span data-stu-id="a2042-179">**SendCode** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample16.cs?highlight=4)]

<span data-ttu-id="a2042-180">Otwórz plik ManageController.cs.</span><span class="sxs-lookup"><span data-stu-id="a2042-180">Open the ManageController.cs file.</span></span> <span data-ttu-id="a2042-181">Należy zmienić następujące metody.</span><span class="sxs-lookup"><span data-stu-id="a2042-181">You need to change the following methods.</span></span>

<span data-ttu-id="a2042-182">**Index** — Metoda</span><span class="sxs-lookup"><span data-stu-id="a2042-182">**Index** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample17.cs?highlight=15-17)]

<span data-ttu-id="a2042-183">Metody **RemoveLogin**</span><span class="sxs-lookup"><span data-stu-id="a2042-183">**RemoveLogin** methods</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample18.cs?highlight=3,13,17)]

<span data-ttu-id="a2042-184">Add-No-- **-Metoda**</span><span class="sxs-lookup"><span data-stu-id="a2042-184">**AddPhoneNumber** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample19.cs?highlight=9)]

<span data-ttu-id="a2042-185">**EnableTwoFactorAuthentication** , Metoda</span><span class="sxs-lookup"><span data-stu-id="a2042-185">**EnableTwoFactorAuthentication** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample20.cs?highlight=3-4)]

<span data-ttu-id="a2042-186">**DisableTwoFactorAuthentication** , Metoda</span><span class="sxs-lookup"><span data-stu-id="a2042-186">**DisableTwoFactorAuthentication** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample21.cs?highlight=3-4)]

<span data-ttu-id="a2042-187">Metody **VerifyPhoneNumber**</span><span class="sxs-lookup"><span data-stu-id="a2042-187">**VerifyPhoneNumber** methods</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample22.cs?highlight=4,18,21)]

<span data-ttu-id="a2042-188">**RemovePhoneNumber** , Metoda</span><span class="sxs-lookup"><span data-stu-id="a2042-188">**RemovePhoneNumber** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample23.cs?highlight=3,8)]

<span data-ttu-id="a2042-189">**ChangePassword** — Metoda</span><span class="sxs-lookup"><span data-stu-id="a2042-189">**ChangePassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample24.cs?highlight=10,13)]

<span data-ttu-id="a2042-190">**SetPassword** — Metoda</span><span class="sxs-lookup"><span data-stu-id="a2042-190">**SetPassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample25.cs?highlight=5,8)]

<span data-ttu-id="a2042-191">**ManageLogins** , Metoda</span><span class="sxs-lookup"><span data-stu-id="a2042-191">**ManageLogins** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample26.cs?highlight=7,12)]

<span data-ttu-id="a2042-192">**LinkLoginCallback** , Metoda</span><span class="sxs-lookup"><span data-stu-id="a2042-192">**LinkLoginCallback** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample27.cs?highlight=8)]

<span data-ttu-id="a2042-193">**HasPassword** , Metoda</span><span class="sxs-lookup"><span data-stu-id="a2042-193">**HasPassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample28.cs?highlight=3)]

<span data-ttu-id="a2042-194">**HasPhoneNumber** , Metoda</span><span class="sxs-lookup"><span data-stu-id="a2042-194">**HasPhoneNumber** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample29.cs?highlight=3)]

<span data-ttu-id="a2042-195">Teraz możesz [uruchomić aplikację](#run) i zarejestrować nowego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="a2042-195">You can now [run the application](#run) and register a new user.</span></span>

<a id="webformsupdate2"></a>
## <a name="for-web-forms-with-update-2-change-account-pages-to-pass-the-key-type"></a><span data-ttu-id="a2042-196">W przypadku formularzy sieci Web z aktualizacją Update 2 Zmień strony kont w celu przekazania typu klucza</span><span class="sxs-lookup"><span data-stu-id="a2042-196">For Web Forms with Update 2, change Account pages to pass the key type</span></span>

<span data-ttu-id="a2042-197">W przypadku formularzy sieci Web z aktualizacją Update 2 należy zmienić następujące strony.</span><span class="sxs-lookup"><span data-stu-id="a2042-197">For Web Forms with Update 2, you need to change the following pages.</span></span>

<span data-ttu-id="a2042-198">**Confirm.aspx.cx**</span><span class="sxs-lookup"><span data-stu-id="a2042-198">**Confirm.aspx.cx**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample30.cs?highlight=8)]

<span data-ttu-id="a2042-199">**RegisterExternalLogin.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="a2042-199">**RegisterExternalLogin.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample31.cs?highlight=36)]

<span data-ttu-id="a2042-200">**Manage.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="a2042-200">**Manage.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample32.cs?highlight=3,22,47,52,69,85,93,98)]

<span data-ttu-id="a2042-201">Teraz możesz [uruchomić aplikację](#run) i zarejestrować nowego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="a2042-201">You can now [run the application](#run) and register a new user.</span></span>

<a id="webformsupdate3"></a>
## <a name="for-web-forms-with-update-3-change-account-pages-to-pass-the-key-type"></a><span data-ttu-id="a2042-202">W przypadku formularzy sieci Web z aktualizacją Update 3 Zmień strony kont w celu przekazania typu klucza</span><span class="sxs-lookup"><span data-stu-id="a2042-202">For Web Forms with Update 3, change Account pages to pass the key type</span></span>

<span data-ttu-id="a2042-203">W przypadku formularzy sieci Web z aktualizacją Update 3 należy zmienić następujące strony.</span><span class="sxs-lookup"><span data-stu-id="a2042-203">For Web Forms with Update 3, you need to change the following pages.</span></span>

<span data-ttu-id="a2042-204">**Confirm.aspx.cx**</span><span class="sxs-lookup"><span data-stu-id="a2042-204">**Confirm.aspx.cx**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample33.cs?highlight=8)]

<span data-ttu-id="a2042-205">**RegisterExternalLogin.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="a2042-205">**RegisterExternalLogin.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample34.cs?highlight=36)]

<span data-ttu-id="a2042-206">**Manage.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="a2042-206">**Manage.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample35.cs?highlight=11,27,32,34,82,87,99,108)]

<span data-ttu-id="a2042-207">**VerifyPhoneNumber.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="a2042-207">**VerifyPhoneNumber.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample36.cs?highlight=8,23,27)]

<span data-ttu-id="a2042-208">**AddPhoneNumber.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="a2042-208">**AddPhoneNumber.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample37.cs?highlight=7)]

<span data-ttu-id="a2042-209">**ManagePassword.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="a2042-209">**ManagePassword.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample38.cs?highlight=11,47,50,68)]

<span data-ttu-id="a2042-210">**ManageLogins.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="a2042-210">**ManageLogins.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample39.cs?highlight=16,23,32,41,45)]

<span data-ttu-id="a2042-211">**TwoFactorAuthenticationSignIn.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="a2042-211">**TwoFactorAuthenticationSignIn.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample40.cs?highlight=14-15,29,53)]

<a id="run"></a>
## <a name="run-application"></a><span data-ttu-id="a2042-212">Uruchom aplikację</span><span class="sxs-lookup"><span data-stu-id="a2042-212">Run application</span></span>

<span data-ttu-id="a2042-213">Zakończono wszystkie wymagane zmiany w szablonie domyślnego aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="a2042-213">You have finished all of the required changes to the default Web Application template.</span></span> <span data-ttu-id="a2042-214">Uruchom aplikację i Zarejestruj nowego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="a2042-214">Run the application and register a new user.</span></span> <span data-ttu-id="a2042-215">Po zarejestrowaniu użytkownika można zauważyć, że tabela AspNetUsers ma kolumnę ID, która jest liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="a2042-215">After registering the user, you will notice that the AspNetUsers table has an Id column that is an integer.</span></span>

![nowy klucz podstawowy](change-primary-key-for-users-in-aspnet-identity/_static/image1.png)

<span data-ttu-id="a2042-217">Jeśli wcześniej utworzono tabele ASP.NET Identity z innym kluczem podstawowym, musisz wprowadzić pewne dodatkowe zmiany.</span><span class="sxs-lookup"><span data-stu-id="a2042-217">If you have previously created the ASP.NET Identity tables with a different primary key, you need to make some additional changes.</span></span> <span data-ttu-id="a2042-218">Jeśli to możliwe, po prostu Usuń istniejącą bazę danych.</span><span class="sxs-lookup"><span data-stu-id="a2042-218">If possible, just delete the existing database.</span></span> <span data-ttu-id="a2042-219">Baza danych zostanie ponownie utworzona przy użyciu poprawnego projektu podczas uruchamiania aplikacji sieci Web i dodania nowego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="a2042-219">The database will be re-created with the correct design when you run the web application and add a new user.</span></span> <span data-ttu-id="a2042-220">Jeśli nie można usunąć, uruchom najpierw migracje kodu, aby zmienić tabele.</span><span class="sxs-lookup"><span data-stu-id="a2042-220">If deletion is not possible, run code first migrations to change the tables.</span></span> <span data-ttu-id="a2042-221">Jednak nowy klucz podstawowy w postaci wartości całkowitej nie zostanie skonfigurowany jako właściwość tożsamości SQL w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="a2042-221">However, the new integer primary key will not be set up as a SQL IDENTITY property in the database.</span></span> <span data-ttu-id="a2042-222">Należy ręcznie ustawić kolumnę ID jako tożsamość.</span><span class="sxs-lookup"><span data-stu-id="a2042-222">You must manually set the Id column as an IDENTITY.</span></span>

<a id="other"></a>
## <a name="other-resources"></a><span data-ttu-id="a2042-223">Inne zasoby</span><span class="sxs-lookup"><span data-stu-id="a2042-223">Other resources</span></span>

- [<span data-ttu-id="a2042-224">Omówienie niestandardowych dostawców magazynu dla produktu ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="a2042-224">Overview of Custom Storage Providers for ASP.NET Identity</span></span>](overview-of-custom-storage-providers-for-aspnet-identity.md)
- [<span data-ttu-id="a2042-225">Migrowanie istniejącej witryny internetowej z członkostwa SQL do produktu ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="a2042-225">Migrating an Existing Website from SQL Membership to ASP.NET Identity</span></span>](../migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md)
- [<span data-ttu-id="a2042-226">Migrowanie danych dostawcy uniwersalnego dla członkostwa i profilów użytkowników do ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="a2042-226">Migrating Universal Provider Data for Membership and User Profiles to ASP.NET Identity</span></span>](../migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity.md)
- <span data-ttu-id="a2042-227">[Przykładowa aplikacja](https://github.com/aspnet/samples/tree/master/samples/aspnet/Identity/ChangePK) ze zmienionym kluczem podstawowym</span><span class="sxs-lookup"><span data-stu-id="a2042-227">[Sample application](https://github.com/aspnet/samples/tree/master/samples/aspnet/Identity/ChangePK) with changed primary key</span></span>

---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
title: 'Część 7: członkostwo i autoryzacja | Microsoft Docs'
author: jongalloway
description: Ta seria samouczków zawiera szczegółowe informacje na temat wszystkich kroków podjętych w celu skompilowania przykładowej aplikacji do sklepu ASP.NET MVC Music. Część 7 obejmuje członkostwo i autoryzację.
ms.author: riande
ms.date: 10/13/2010
ms.assetid: c8511ebe-68bc-4240-87c3-d5ced84a3f37
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
msc.type: authoredcontent
ms.openlocfilehash: a6a1a936e0ea29ea36721ba78f35845401f74c01
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78539201"
---
# <a name="part-7-membership-and-authorization"></a><span data-ttu-id="c3b65-104">Część 7. Członkostwo i autoryzacja</span><span class="sxs-lookup"><span data-stu-id="c3b65-104">Part 7: Membership and Authorization</span></span>

<span data-ttu-id="c3b65-105">przez [Jan Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="c3b65-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="c3b65-106">Sklep MVC Music jest aplikacją samouczka, która wprowadza i objaśnia krok po kroku, jak używać ASP.NET MVC i Visual Studio do programowania w sieci Web.</span><span class="sxs-lookup"><span data-stu-id="c3b65-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="c3b65-107">Sklep MVC Music jest lekkim przykładowym wdrożeniem magazynu, który sprzedaje Albumy muzyczne w trybie online i implementuje podstawowe funkcje administracyjne, logowania użytkownika i koszyka.</span><span class="sxs-lookup"><span data-stu-id="c3b65-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="c3b65-108">Ta seria samouczków zawiera szczegółowe informacje na temat wszystkich kroków podjętych w celu skompilowania przykładowej aplikacji do sklepu ASP.NET MVC Music.</span><span class="sxs-lookup"><span data-stu-id="c3b65-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="c3b65-109">Część 7 obejmuje członkostwo i autoryzację.</span><span class="sxs-lookup"><span data-stu-id="c3b65-109">Part 7 covers Membership and Authorization.</span></span>

<span data-ttu-id="c3b65-110">Nasz kontroler Menedżera sklepu jest obecnie dostępny dla wszystkich osób odwiedzających naszą witrynę.</span><span class="sxs-lookup"><span data-stu-id="c3b65-110">Our Store Manager controller is currently accessible to anyone visiting our site.</span></span> <span data-ttu-id="c3b65-111">Zmieńmy ten sposób, aby ograniczyć uprawnienia do administratorów lokacji.</span><span class="sxs-lookup"><span data-stu-id="c3b65-111">Let's change this to restrict permission to site administrators.</span></span>

## <a name="adding-the-accountcontroller-and-views"></a><span data-ttu-id="c3b65-112">Dodawanie elementu AccountController i widoków</span><span class="sxs-lookup"><span data-stu-id="c3b65-112">Adding the AccountController and Views</span></span>

<span data-ttu-id="c3b65-113">Jedną z różnic między pełnym szablonem aplikacji sieci Web ASP.NET MVC 3 a szablonem pustej aplikacji sieci Web ASP.NET MVC 3 jest to, że pusty szablon nie zawiera kontrolera konta.</span><span class="sxs-lookup"><span data-stu-id="c3b65-113">One difference between the full ASP.NET MVC 3 Web Application template and the ASP.NET MVC 3 Empty Web Application template is that the empty template doesn't include an Account Controller.</span></span> <span data-ttu-id="c3b65-114">Dodamy kontroler konta przez skopiowanie kilku plików z nowej aplikacji ASP.NET MVC utworzonej na podstawie szablonu aplikacji sieci Web ASP.NET MVC 3.</span><span class="sxs-lookup"><span data-stu-id="c3b65-114">We'll add an Account Controller by copying a few files from a new ASP.NET MVC application created from the full ASP.NET MVC 3 Web Application template.</span></span>

<span data-ttu-id="c3b65-115">Utwórz nową aplikację ASP.NET MVC przy użyciu pełnego szablonu aplikacji sieci Web ASP.NET MVC 3 i skopiuj następujące pliki do tych samych katalogów w naszym projekcie:</span><span class="sxs-lookup"><span data-stu-id="c3b65-115">Create a new ASP.NET MVC application using the full ASP.NET MVC 3 Web Application template and copy the following files into the same directories in our project:</span></span>

1. <span data-ttu-id="c3b65-116">Kopiuj AccountController.cs w katalogu controllers</span><span class="sxs-lookup"><span data-stu-id="c3b65-116">Copy AccountController.cs in the Controllers directory</span></span>
2. <span data-ttu-id="c3b65-117">Kopiuj AccountModels w katalogu models</span><span class="sxs-lookup"><span data-stu-id="c3b65-117">Copy AccountModels in the Models directory</span></span>
3. <span data-ttu-id="c3b65-118">Utwórz katalog konta w katalogu widoki i skopiuj wszystkie cztery widoki w</span><span class="sxs-lookup"><span data-stu-id="c3b65-118">Create an Account directory inside the Views directory and copy all four views in</span></span>

<span data-ttu-id="c3b65-119">Zmień przestrzeń nazw dla kontrolera i klasy modelu, aby zaczynać się od MvcMusicStore.</span><span class="sxs-lookup"><span data-stu-id="c3b65-119">Change the namespace for the Controller and Model classes so they begin with MvcMusicStore.</span></span> <span data-ttu-id="c3b65-120">Klasa elementu AccountController powinna używać przestrzeni nazw MvcMusicStore. controllers, a Klasa AccountModels powinna używać przestrzeni nazw MvcMusicStore. models.</span><span class="sxs-lookup"><span data-stu-id="c3b65-120">The AccountController class should use the MvcMusicStore.Controllers namespace, and the AccountModels class should use the MvcMusicStore.Models namespace.</span></span>

<span data-ttu-id="c3b65-121">*Uwaga: te pliki są również dostępne na stronie pobierania MvcMusicStore-Assets. zip, z której zostały skopiowane pliki projektu witryny na początku samouczka. Pliki członkostwa znajdują się w katalogu kodu.*</span><span class="sxs-lookup"><span data-stu-id="c3b65-121">*Note: These files are also available in the MvcMusicStore-Assets.zip download from which we copied our site design files at the beginning of the tutorial. The Membership files are located in the Code directory.*</span></span>

<span data-ttu-id="c3b65-122">Zaktualizowane rozwiązanie powinno wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="c3b65-122">The updated solution should look like the following:</span></span>

![](mvc-music-store-part-7/_static/image1.png)

## <a name="adding-an-administrative-user-with-the-aspnet-configuration-site"></a><span data-ttu-id="c3b65-123">Dodawanie użytkownika administracyjnego przy użyciu lokacji konfiguracji ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c3b65-123">Adding an Administrative User with the ASP.NET Configuration site</span></span>

<span data-ttu-id="c3b65-124">Przed wymaganiem autoryzacji w naszej witrynie sieci Web należy utworzyć użytkownika z dostępem.</span><span class="sxs-lookup"><span data-stu-id="c3b65-124">Before we require Authorization in our website, we'll need to create a user with access.</span></span> <span data-ttu-id="c3b65-125">Najprostszym sposobem utworzenia użytkownika jest użycie wbudowanej witryny sieci Web ASP.NET Configuration.</span><span class="sxs-lookup"><span data-stu-id="c3b65-125">The easiest way to create a user is to use the built-in ASP.NET Configuration website.</span></span>

<span data-ttu-id="c3b65-126">Uruchom witrynę konfiguracji ASP.NET, klikając ikonę w Eksplorator rozwiązań.</span><span class="sxs-lookup"><span data-stu-id="c3b65-126">Launch the ASP.NET Configuration website by clicking following the icon in the Solution Explorer.</span></span>

![](mvc-music-store-part-7/_static/image2.png)

<span data-ttu-id="c3b65-127">Spowoduje to uruchomienie witryny sieci Web konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="c3b65-127">This launches a configuration website.</span></span> <span data-ttu-id="c3b65-128">Kliknij kartę Zabezpieczenia na ekranie głównym, a następnie kliknij link "Włącz role" w centrum ekranu.</span><span class="sxs-lookup"><span data-stu-id="c3b65-128">Click on the Security tab on the home screen, then click the "Enable roles" link in the center of the screen.</span></span>

![](mvc-music-store-part-7/_static/image3.png)

<span data-ttu-id="c3b65-129">Kliknij link "Utwórz role lub zarządzaj nimi".</span><span class="sxs-lookup"><span data-stu-id="c3b65-129">Click the "Create or Manage roles" link.</span></span>

![](mvc-music-store-part-7/_static/image4.png)

<span data-ttu-id="c3b65-130">Wprowadź wartość "Administrator" jako nazwę roli i naciśnij przycisk Dodaj rolę.</span><span class="sxs-lookup"><span data-stu-id="c3b65-130">Enter "Administrator" as the role name and press the Add Role button.</span></span>

![](mvc-music-store-part-7/_static/image5.png)

<span data-ttu-id="c3b65-131">Kliknij przycisk Wstecz, a następnie kliknij link Utwórz użytkownika po lewej stronie.</span><span class="sxs-lookup"><span data-stu-id="c3b65-131">Click the Back button, then click on the Create user link on the left side.</span></span>

![](mvc-music-store-part-7/_static/image6.png)

<span data-ttu-id="c3b65-132">Wypełnij pola informacje o użytkowniku po lewej stronie, używając następujących informacji:</span><span class="sxs-lookup"><span data-stu-id="c3b65-132">Fill in the user information fields on the left using the following information:</span></span>

| <span data-ttu-id="c3b65-133">**Pole**</span><span class="sxs-lookup"><span data-stu-id="c3b65-133">**Field**</span></span> | <span data-ttu-id="c3b65-134">**Wartość**</span><span class="sxs-lookup"><span data-stu-id="c3b65-134">**Value**</span></span> |
| --- | --- |
| <span data-ttu-id="c3b65-135">**Nazwa użytkownika**</span><span class="sxs-lookup"><span data-stu-id="c3b65-135">**User Name**</span></span> | <span data-ttu-id="c3b65-136">Administrator</span><span class="sxs-lookup"><span data-stu-id="c3b65-136">Administrator</span></span> |
| <span data-ttu-id="c3b65-137">**Hasło**</span><span class="sxs-lookup"><span data-stu-id="c3b65-137">**Password**</span></span> | <span data-ttu-id="c3b65-138">password123!</span><span class="sxs-lookup"><span data-stu-id="c3b65-138">password123!</span></span> |
| <span data-ttu-id="c3b65-139">**Potwierdź hasło**</span><span class="sxs-lookup"><span data-stu-id="c3b65-139">**Confirm Password**</span></span> | <span data-ttu-id="c3b65-140">password123!</span><span class="sxs-lookup"><span data-stu-id="c3b65-140">password123!</span></span> |
| <span data-ttu-id="c3b65-141">**E-mail**</span><span class="sxs-lookup"><span data-stu-id="c3b65-141">**E-mail**</span></span> | <span data-ttu-id="c3b65-142">(dowolny adres e-mail będzie działał)</span><span class="sxs-lookup"><span data-stu-id="c3b65-142">(any email address will work)</span></span> |
| <span data-ttu-id="c3b65-143">**Pytanie zabezpieczeń**</span><span class="sxs-lookup"><span data-stu-id="c3b65-143">**Security Question**</span></span> | <span data-ttu-id="c3b65-144">(dowolne z nich)</span><span class="sxs-lookup"><span data-stu-id="c3b65-144">(whatever you like)</span></span> |
| <span data-ttu-id="c3b65-145">**Odpowiedź zabezpieczeń**</span><span class="sxs-lookup"><span data-stu-id="c3b65-145">**Security Answer**</span></span> | <span data-ttu-id="c3b65-146">(dowolne z nich)</span><span class="sxs-lookup"><span data-stu-id="c3b65-146">(whatever you like)</span></span> |

<span data-ttu-id="c3b65-147">*Uwaga: możesz użyć dowolnego hasła, które chcesz. Domyślne ustawienia zabezpieczeń hasła wymagają hasła o długości 7 znaków i zawierają jeden znak inny niż alfanumeryczny.*</span><span class="sxs-lookup"><span data-stu-id="c3b65-147">*Note: You can of course use any password you'd like. The default password security settings require a password that is 7 characters long and contains one non-alphanumeric character.*</span></span>

<span data-ttu-id="c3b65-148">Wybierz rolę administratora dla tego użytkownika, a następnie kliknij przycisk Utwórz użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c3b65-148">Select the Administrator role for this user, and click the Create User button.</span></span>

![](mvc-music-store-part-7/_static/image7.png)

<span data-ttu-id="c3b65-149">W tym momencie powinien zostać wyświetlony komunikat informujący o tym, że użytkownik został utworzony pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="c3b65-149">At this point, you should see a message indicating that the user was created successfully.</span></span>

![](mvc-music-store-part-7/_static/image8.png)

<span data-ttu-id="c3b65-150">Teraz możesz zamknąć okno przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="c3b65-150">You can now close the browser window.</span></span>

## <a name="role-based-authorization"></a><span data-ttu-id="c3b65-151">Autoryzacja oparta na rolach</span><span class="sxs-lookup"><span data-stu-id="c3b65-151">Role-based Authorization</span></span>

<span data-ttu-id="c3b65-152">Teraz możemy ograniczyć dostęp do StoreManagerController przy użyciu atrybutu [autoryzuje], określając, że użytkownik musi należeć do roli administratora, aby uzyskać dostęp do dowolnej akcji kontrolera w klasie.</span><span class="sxs-lookup"><span data-stu-id="c3b65-152">Now we can restrict access to the StoreManagerController using the [Authorize] attribute, specifying that the user must be in the Administrator role to access any controller action in the class.</span></span>

[!code-csharp[Main](mvc-music-store-part-7/samples/sample1.cs)]

<span data-ttu-id="c3b65-153">*Uwaga: atrybut [autoryzuje] może być umieszczony w określonych metodach akcji, a także na poziomie klasy kontrolera.*</span><span class="sxs-lookup"><span data-stu-id="c3b65-153">*Note: The [Authorize] attribute can be placed on specific action methods as well as at the Controller class level.*</span></span>

<span data-ttu-id="c3b65-154">Teraz przechodzenie do/StoreManager powoduje wyświetlenie okna dialogowego logowania:</span><span class="sxs-lookup"><span data-stu-id="c3b65-154">Now browsing to /StoreManager brings up a Log On dialog:</span></span>

![](mvc-music-store-part-7/_static/image9.png)

<span data-ttu-id="c3b65-155">Po zalogowaniu się przy użyciu nowego konta administratora można przejść do ekranu edycji albumu jako poprzednio.</span><span class="sxs-lookup"><span data-stu-id="c3b65-155">After logging on with our new Administrator account, we're able to go to the Album Edit screen as before.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="c3b65-156">[Poprzednie](mvc-music-store-part-6.md)
> [dalej](mvc-music-store-part-8.md)</span><span class="sxs-lookup"><span data-stu-id="c3b65-156">[Previous](mvc-music-store-part-6.md)
[Next](mvc-music-store-part-8.md)</span></span>

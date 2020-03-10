---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
title: 'Część 6: ASP.NET Membership | Microsoft Docs'
author: JoeStagner
description: Ta seria samouczków zawiera szczegółowe informacje na temat wszystkich kroków podjętych w celu skompilowania przykładowej aplikacji Tailspin Spyworks. Część 6 dodaje członkostwo ASP.NET.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: f70a310c-9557-4743-82cb-655265676d39
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
msc.type: authoredcontent
ms.openlocfilehash: b0caa89dc9ffb5bb7451fa2d9d346c7db2bf1466
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78564184"
---
# <a name="part-6-aspnet-membership"></a><span data-ttu-id="fdfbc-104">Część 6. Członkostwo platformy ASP.NET</span><span class="sxs-lookup"><span data-stu-id="fdfbc-104">Part 6: ASP.NET Membership</span></span>

<span data-ttu-id="fdfbc-105">Jan [Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="fdfbc-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="fdfbc-106">Tailspin Spyworks ilustruje, w jaki sposób bardzo jest prosta, aby tworzyć zaawansowane, skalowalne aplikacje dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="fdfbc-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="fdfbc-107">W tym artykule przedstawiono sposób korzystania z doskonałych nowych funkcji w programie ASP.NET 4 do tworzenia sklepu online, w tym zakupów, wyewidencjonowywania i administrowania.</span><span class="sxs-lookup"><span data-stu-id="fdfbc-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="fdfbc-108">Ta seria samouczków zawiera szczegółowe informacje na temat wszystkich kroków podjętych w celu skompilowania przykładowej aplikacji Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="fdfbc-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="fdfbc-109">Część 6 dodaje członkostwo ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="fdfbc-109">Part 6 adds ASP.NET Membership.</span></span>

## <a id="_Toc260221672"></a><span data-ttu-id="fdfbc-110">Praca z członkostwem ASP.NET</span><span class="sxs-lookup"><span data-stu-id="fdfbc-110">Working with ASP.NET Membership</span></span>

![](tailspin-spyworks-part-6/_static/image1.png)

<span data-ttu-id="fdfbc-111">Kliknij pozycję Zabezpieczenia</span><span class="sxs-lookup"><span data-stu-id="fdfbc-111">Click Security</span></span>

![](tailspin-spyworks-part-6/_static/image1.jpg)

<span data-ttu-id="fdfbc-112">Upewnij się, że korzystamy z uwierzytelniania formularzy.</span><span class="sxs-lookup"><span data-stu-id="fdfbc-112">Make sure that we are using forms authentication.</span></span>

![](tailspin-spyworks-part-6/_static/image2.jpg)

<span data-ttu-id="fdfbc-113">Użyj linku "Utwórz użytkownika", aby utworzyć kilku użytkowników.</span><span class="sxs-lookup"><span data-stu-id="fdfbc-113">Use the "Create User" link to create a couple of users.</span></span>

![](tailspin-spyworks-part-6/_static/image3.jpg)

<span data-ttu-id="fdfbc-114">Po zakończeniu zapoznaj się z oknem Eksplorator rozwiązań i Odśwież widok.</span><span class="sxs-lookup"><span data-stu-id="fdfbc-114">When done, refer to the Solution Explorer window and refresh the view.</span></span>

![](tailspin-spyworks-part-6/_static/image2.png)

<span data-ttu-id="fdfbc-115">Należy pamiętać, że ASPNETDB. Została utworzona dobra MDF.</span><span class="sxs-lookup"><span data-stu-id="fdfbc-115">Note that the ASPNETDB.MDF fine has been created.</span></span> <span data-ttu-id="fdfbc-116">Ten plik zawiera tabele, które obsługują podstawowe usługi ASP.NET, takie jak członkostwo.</span><span class="sxs-lookup"><span data-stu-id="fdfbc-116">This file contains the tables to support the core ASP.NET services like membership.</span></span>

<span data-ttu-id="fdfbc-117">Teraz możemy zacząć implementować proces wyewidencjonowania.</span><span class="sxs-lookup"><span data-stu-id="fdfbc-117">Now we can begin implementing the checkout process.</span></span>

<span data-ttu-id="fdfbc-118">Zacznij od utworzenia strony wyewidencjonowywania. aspx.</span><span class="sxs-lookup"><span data-stu-id="fdfbc-118">Begin by creating a CheckOut.aspx page.</span></span>

<span data-ttu-id="fdfbc-119">Strona wyewidencjonowywanie. aspx powinna być dostępna tylko dla użytkowników, którzy są zalogowani, aby ograniczyć dostęp do zalogowanych użytkowników i przekierować użytkowników, którzy nie są zalogowani na stronie logowania.</span><span class="sxs-lookup"><span data-stu-id="fdfbc-119">The CheckOut.aspx page should only be available to users who are logged in so we will restrict access to logged in users and redirect users who are not logged in to the LogIn page.</span></span>

<span data-ttu-id="fdfbc-120">W tym celu dodamy następujący plik do sekcji konfiguracji naszego pliku Web. config.</span><span class="sxs-lookup"><span data-stu-id="fdfbc-120">To do this we'll add the following to the configuration section of our web.config file.</span></span>

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample1.xml)]

<span data-ttu-id="fdfbc-121">Szablon aplikacji formularzy sieci Web ASP.NET automatycznie dodaje sekcję uwierzytelniania do pliku Web. config i ustanowił domyślną stronę logowania.</span><span class="sxs-lookup"><span data-stu-id="fdfbc-121">The template for ASP.NET Web Forms applications automatically added an authentication section to our web.config file and established the default login page.</span></span>

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample2.xml)]

<span data-ttu-id="fdfbc-122">Przed zalogowaniem się użytkownika należy zmodyfikować kod login. aspx związany z plikiem.</span><span class="sxs-lookup"><span data-stu-id="fdfbc-122">We must modify the Login.aspx code behind file to migrate an anonymous shopping cart when the user logs in.</span></span> <span data-ttu-id="fdfbc-123">Zmień\_zdarzeń ładowania strony w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="fdfbc-123">Change the Page\_Load event as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample3.cs)]

<span data-ttu-id="fdfbc-124">Następnie Dodaj procedurę obsługi zdarzeń "zalogowany", taką jak ta, aby ustawić nazwę sesji dla nowo zalogowanego użytkownika i zmienić tymczasowy identyfikator sesji w koszyku na użytkownika przez wywołanie metody MigrateCart w naszej klasie MyShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="fdfbc-124">Then add a "LoggedIn" event handler like this to set the session name to the newly logged in user and change the temporary session id in the shopping cart to that of the user by calling the MigrateCart method in our MyShoppingCart class.</span></span> <span data-ttu-id="fdfbc-125">(Zaimplementowane w pliku CS)</span><span class="sxs-lookup"><span data-stu-id="fdfbc-125">(Implemented in the .cs file)</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample4.cs)]

<span data-ttu-id="fdfbc-126">Zaimplementuj metodę MigrateCart () w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="fdfbc-126">Implement the MigrateCart() method like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample5.cs)]

<span data-ttu-id="fdfbc-127">W obszarze wyewidencjonowywanie. aspx będziemy używać obiektu EntityDataSource i widoku GridView na naszej stronie wyewidencjonowywania, podobnie jak na naszej stronie koszyka zakupów.</span><span class="sxs-lookup"><span data-stu-id="fdfbc-127">In checkout.aspx we'll use an EntityDataSource and a GridView in our check out page much as we did in our shopping cart page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-6/samples/sample6.aspx)]

<span data-ttu-id="fdfbc-128">Należy zauważyć, że nasz formant GridView określa procedurę obsługi zdarzeń "OnDataBound" o nazwie Moje listy\_RowDataBound, aby umożliwić wdrożenie tego programu obsługi zdarzeń, takiego jak to.</span><span class="sxs-lookup"><span data-stu-id="fdfbc-128">Note that our GridView control specifies an "ondatabound" event handler named MyList\_RowDataBound so let's implement that event handler like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample7.cs)]

<span data-ttu-id="fdfbc-129">Ta metoda przechowuje sumę uruchamiania koszyka, ponieważ każdy wiersz jest powiązany, i aktualizuje dolny wiersz widoku GridView.</span><span class="sxs-lookup"><span data-stu-id="fdfbc-129">This method keeps a running total of the shopping cart as each row is bound and updates the bottom row of the GridView.</span></span>

<span data-ttu-id="fdfbc-130">Na tym etapie wprowadziliśmy prezentację "przegląd" kolejności, w której ma zostać wykonane.</span><span class="sxs-lookup"><span data-stu-id="fdfbc-130">At this stage we have implemented a "review" presentation of the order to be placed.</span></span>

<span data-ttu-id="fdfbc-131">Aby obsłużyć pusty scenariusz, Dodaj kilka wierszy kodu do naszej strony,\_zdarzenie ładowania:</span><span class="sxs-lookup"><span data-stu-id="fdfbc-131">Let's handle an empty cart scenario by adding a few lines of code to our Page\_Load event:</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample8.cs)]

<span data-ttu-id="fdfbc-132">Gdy użytkownik kliknie przycisk "Prześlij", zostanie wykonany następujący kod na przycisku Prześlij kliknij procedurę obsługi zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="fdfbc-132">When the user clicks on the "Submit" button we will execute the following code in the Submit Button Click Event handler.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample9.cs)]

<span data-ttu-id="fdfbc-133">"Mięso" procesu przekazywania zamówienia należy zaimplementować w metodzie SubmitOrder () klasy MyShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="fdfbc-133">The "meat" of the order submission process is to be implemented in the SubmitOrder() method of our MyShoppingCart class.</span></span>

<span data-ttu-id="fdfbc-134">SubmitOrder:</span><span class="sxs-lookup"><span data-stu-id="fdfbc-134">SubmitOrder will:</span></span>

- <span data-ttu-id="fdfbc-135">Wypełnij wszystkie elementy wiersza w koszyku i używaj ich do tworzenia nowego rekordu zamówienia i skojarzonych rekordów OrderDetails.</span><span class="sxs-lookup"><span data-stu-id="fdfbc-135">Take all the line items in the shopping cart and use them to create a new Order Record and the associated OrderDetails records.</span></span>
- <span data-ttu-id="fdfbc-136">Oblicz datę wysyłki.</span><span class="sxs-lookup"><span data-stu-id="fdfbc-136">Calculate Shipping Date.</span></span>
- <span data-ttu-id="fdfbc-137">Wyczyść koszyk.</span><span class="sxs-lookup"><span data-stu-id="fdfbc-137">Clear the shopping cart.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample10.cs)]

<span data-ttu-id="fdfbc-138">Na potrzeby tej przykładowej aplikacji obliczą datę wysyłki przez dodanie dwóch dni do bieżącej daty.</span><span class="sxs-lookup"><span data-stu-id="fdfbc-138">For the purposes of this sample application we'll calculate a ship date by simply adding two days to the current date.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample11.cs)]

<span data-ttu-id="fdfbc-139">Uruchomienie aplikacji umożliwi nam przetestowanie procesu zakupów od początku do końca.</span><span class="sxs-lookup"><span data-stu-id="fdfbc-139">Running the application now will permit us to test the shopping process from start to finish.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="fdfbc-140">[Poprzednie](tailspin-spyworks-part-5.md)
> [dalej](tailspin-spyworks-part-7.md)</span><span class="sxs-lookup"><span data-stu-id="fdfbc-140">[Previous](tailspin-spyworks-part-5.md)
[Next](tailspin-spyworks-part-7.md)</span></span>

---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
title: Część 6. Używanie adnotacji danych na potrzeby walidacji modelu | Microsoft Docs
author: jongalloway
description: Ta seria samouczków zawiera szczegółowe informacje na temat wszystkich kroków podjętych w celu skompilowania przykładowej aplikacji do sklepu ASP.NET MVC Music. Część 6 obejmuje używanie adnotacji danych dla modelu V...
ms.author: riande
ms.date: 04/21/2011
ms.assetid: b3193d33-2d0b-4d98-9712-58bd897c62ec
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
msc.type: authoredcontent
ms.openlocfilehash: bc031dd5be61cc6707c522f85f6af77a420c8b31
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78539278"
---
# <a name="part-6-using-data-annotations-for-model-validation"></a><span data-ttu-id="7bfc9-104">Część 6. Używanie adnotacji do danych na potrzeby walidacji modelu</span><span class="sxs-lookup"><span data-stu-id="7bfc9-104">Part 6: Using Data Annotations for Model Validation</span></span>

<span data-ttu-id="7bfc9-105">przez [Jan Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="7bfc9-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="7bfc9-106">Sklep MVC Music jest aplikacją samouczka, która wprowadza i objaśnia krok po kroku, jak używać ASP.NET MVC i Visual Studio do programowania w sieci Web.</span><span class="sxs-lookup"><span data-stu-id="7bfc9-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="7bfc9-107">Sklep MVC Music jest lekkim przykładowym wdrożeniem magazynu, który sprzedaje Albumy muzyczne w trybie online i implementuje podstawowe funkcje administracyjne, logowania użytkownika i koszyka.</span><span class="sxs-lookup"><span data-stu-id="7bfc9-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="7bfc9-108">Ta seria samouczków zawiera szczegółowe informacje na temat wszystkich kroków podjętych w celu skompilowania przykładowej aplikacji do sklepu ASP.NET MVC Music.</span><span class="sxs-lookup"><span data-stu-id="7bfc9-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="7bfc9-109">Część 6 obejmuje używanie adnotacji danych na potrzeby walidacji modelu.</span><span class="sxs-lookup"><span data-stu-id="7bfc9-109">Part 6 covers Using Data Annotations for Model Validation.</span></span>

<span data-ttu-id="7bfc9-110">Mamy ważny problem z naszymi formularzami tworzenia i edytowania: nie wykonuje żadnych weryfikacji.</span><span class="sxs-lookup"><span data-stu-id="7bfc9-110">We have a major issue with our Create and Edit forms: they're not doing any validation.</span></span> <span data-ttu-id="7bfc9-111">Możemy to zrobić w taki sposób, aby pozostawić wymagane pola jako puste lub wpisać litery w polu Cena, a pierwszy z nich zobaczymy z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="7bfc9-111">We can do things like leave required fields blank or type letters in the Price field, and the first error we'll see is from the database.</span></span>

<span data-ttu-id="7bfc9-112">Możemy łatwo dodać weryfikację do naszej aplikacji, dodając adnotacje danych do naszych klas modelu.</span><span class="sxs-lookup"><span data-stu-id="7bfc9-112">We can easily add validation to our application by adding Data Annotations to our model classes.</span></span> <span data-ttu-id="7bfc9-113">Adnotacje danych pozwalają nam opisać reguły, które chcemy zastosować do naszych właściwości modelu, a ASP.NET MVC zajmie się ich wymuszeniem i wyświetleniem odpowiednich komunikatów dla naszych użytkowników.</span><span class="sxs-lookup"><span data-stu-id="7bfc9-113">Data Annotations allow us to describe the rules we want applied to our model properties, and ASP.NET MVC will take care of enforcing them and displaying appropriate messages to our users.</span></span>

## <a name="adding-validation-to-our-album-forms"></a><span data-ttu-id="7bfc9-114">Dodawanie walidacji do naszych formularzy albumu</span><span class="sxs-lookup"><span data-stu-id="7bfc9-114">Adding Validation to our Album Forms</span></span>

<span data-ttu-id="7bfc9-115">Będziemy używać następujących atrybutów adnotacji danych:</span><span class="sxs-lookup"><span data-stu-id="7bfc9-115">We'll use the following Data Annotation attributes:</span></span>

- <span data-ttu-id="7bfc9-116">**Wymagane** — wskazuje, że właściwość jest polem wymaganym</span><span class="sxs-lookup"><span data-stu-id="7bfc9-116">**Required** – Indicates that the property is a required field</span></span>
- <span data-ttu-id="7bfc9-117">**DisplayName** — definiuje tekst, który ma być używany w polach formularzy i komunikatów weryfikacji</span><span class="sxs-lookup"><span data-stu-id="7bfc9-117">**DisplayName** – Defines the text we want used on form fields and validation messages</span></span>
- <span data-ttu-id="7bfc9-118">**StringLength** — definiuje maksymalną długość pola ciągu</span><span class="sxs-lookup"><span data-stu-id="7bfc9-118">**StringLength** – Defines a maximum length for a string field</span></span>
- <span data-ttu-id="7bfc9-119">**Range** — zapewnia maksymalną i minimalną wartość pola liczbowego</span><span class="sxs-lookup"><span data-stu-id="7bfc9-119">**Range** – Gives a maximum and minimum value for a numeric field</span></span>
- <span data-ttu-id="7bfc9-120">**Bind** — wyświetla listę pól do wykluczenia lub dołączenia podczas wiązania parametrów lub wartości formularza z właściwościami modelu</span><span class="sxs-lookup"><span data-stu-id="7bfc9-120">**Bind** – Lists fields to exclude or include when binding parameter or form values to model properties</span></span>
- <span data-ttu-id="7bfc9-121">**ScaffoldColumn** — umożliwia ukrywanie pól z formularzy edytora</span><span class="sxs-lookup"><span data-stu-id="7bfc9-121">**ScaffoldColumn** – Allows hiding fields from editor forms</span></span>

<span data-ttu-id="7bfc9-122">*Uwaga: Aby uzyskać więcej informacji na temat weryfikacji modelu przy użyciu atrybutów adnotacji danych, zapoznaj się z dokumentacją MSDN w witrynie* [`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)</span><span class="sxs-lookup"><span data-stu-id="7bfc9-122">*Note: For more information on Model Validation using Data Annotation attributes, see the MSDN documentation at*[`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)</span></span>

<span data-ttu-id="7bfc9-123">Otwórz klasę albumu i Dodaj następujące instrukcje *using* na początku.</span><span class="sxs-lookup"><span data-stu-id="7bfc9-123">Open the Album class and add the following *using* statements to the top.</span></span>

[!code-csharp[Main](mvc-music-store-part-6/samples/sample1.cs)]

<span data-ttu-id="7bfc9-124">Następnie zaktualizuj właściwości, aby dodać atrybuty wyświetlania i walidacji, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="7bfc9-124">Next, update the properties to add display and validation attributes as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-6/samples/sample2.cs)]

<span data-ttu-id="7bfc9-125">W tym czasie zmieniono także gatunek i wykonawcę na właściwości wirtualne.</span><span class="sxs-lookup"><span data-stu-id="7bfc9-125">While we're there, we've also changed the Genre and Artist to virtual properties.</span></span> <span data-ttu-id="7bfc9-126">Dzięki temu Entity Framework w razie potrzeby Ładuj z opóźnieniem.</span><span class="sxs-lookup"><span data-stu-id="7bfc9-126">This allows Entity Framework to lazy-load them as necessary.</span></span>

[!code-csharp[Main](mvc-music-store-part-6/samples/sample3.cs)]

<span data-ttu-id="7bfc9-127">Po dodaniu tych atrybutów do modelu albumu, nasz ekran Utwórz i edytuj natychmiast rozpocznie sprawdzanie poprawności pól i używa wybranych przez siebie nazw wyświetlanych (np. adres URL grafiki albumu zamiast AlbumArtUrl).</span><span class="sxs-lookup"><span data-stu-id="7bfc9-127">After having added these attributes to our Album model, our Create and Edit screen immediately begin validating fields and using the Display Names we've chosen (e.g. Album Art Url instead of AlbumArtUrl).</span></span> <span data-ttu-id="7bfc9-128">Uruchom aplikację i przejdź do/StoreManager/Create.</span><span class="sxs-lookup"><span data-stu-id="7bfc9-128">Run the application and browse to /StoreManager/Create.</span></span>

![](mvc-music-store-part-6/_static/image1.png)

<span data-ttu-id="7bfc9-129">Następnie zostaną rozdzielone pewne reguły walidacji.</span><span class="sxs-lookup"><span data-stu-id="7bfc9-129">Next, we'll break some validation rules.</span></span> <span data-ttu-id="7bfc9-130">Wprowadź wartość w polu cena 0 i pozostaw pole puste.</span><span class="sxs-lookup"><span data-stu-id="7bfc9-130">Enter a price of 0 and leave the Title blank.</span></span> <span data-ttu-id="7bfc9-131">Po kliknięciu przycisku Utwórz zostanie wyświetlony formularz z komunikatami o błędach walidacji pokazujący, które pola nie spełniały zdefiniowanej reguły sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="7bfc9-131">When we click on the Create button, we will see the form displayed with validation error messages showing which fields did not meet the validation rules we have defined.</span></span>

![](mvc-music-store-part-6/_static/image2.png)

## <a name="testing-the-client-side-validation"></a><span data-ttu-id="7bfc9-132">Testowanie weryfikacji po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="7bfc9-132">Testing the Client-Side Validation</span></span>

<span data-ttu-id="7bfc9-133">Sprawdzanie poprawności po stronie serwera jest bardzo ważne w perspektywie aplikacji, ponieważ użytkownicy mogą obejść weryfikację po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="7bfc9-133">Server-side validation is very important from an application perspective, because users can circumvent client-side validation.</span></span> <span data-ttu-id="7bfc9-134">Jednak na stronie sieci Web są wdrażane tylko te, które implementują walidację po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="7bfc9-134">However, webpage forms which only implement server-side validation exhibit three significant problems.</span></span>

1. <span data-ttu-id="7bfc9-135">Użytkownik musi czekać na opublikowanie formularza, zweryfikowanie go na serwerze i wysłanie odpowiedzi do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="7bfc9-135">The user has to wait for the form to be posted, validated on the server, and for the response to be sent to their browser.</span></span>
2. <span data-ttu-id="7bfc9-136">Użytkownik nie otrzymuje natychmiastowej opinii, gdy poprawi pole, aby przeszedł teraz reguły walidacji.</span><span class="sxs-lookup"><span data-stu-id="7bfc9-136">The user doesn't get immediate feedback when they correct a field so that it now passes the validation rules.</span></span>
3. <span data-ttu-id="7bfc9-137">Przed użyciem przeglądarki użytkownika wykorzystamy zasoby serwera do przeprowadzenia logiki walidacji.</span><span class="sxs-lookup"><span data-stu-id="7bfc9-137">We are wasting server resources to perform validation logic instead of leveraging the user's browser.</span></span>

<span data-ttu-id="7bfc9-138">Na szczęście szablony szkieletu ASP.NET MVC 3 mają wbudowaną funkcję weryfikacji po stronie klienta, co nie wymaga żadnych dodatkowych czynności.</span><span class="sxs-lookup"><span data-stu-id="7bfc9-138">Fortunately, the ASP.NET MVC 3 scaffold templates have client-side validation built in, requiring no additional work whatsoever.</span></span>

<span data-ttu-id="7bfc9-139">Wpisanie pojedynczej litery w polu title spełnia wymagania dotyczące walidacji, dlatego komunikat o weryfikacji zostanie natychmiast usunięty.</span><span class="sxs-lookup"><span data-stu-id="7bfc9-139">Typing a single letter in the Title field satisfies the validation requirements, so the validation message is immediately removed.</span></span>

![](mvc-music-store-part-6/_static/image3.png)

> [!div class="step-by-step"]
> <span data-ttu-id="7bfc9-140">[Poprzednie](mvc-music-store-part-5.md)
> [dalej](mvc-music-store-part-7.md)</span><span class="sxs-lookup"><span data-stu-id="7bfc9-140">[Previous](mvc-music-store-part-5.md)
[Next](mvc-music-store-part-7.md)</span></span>

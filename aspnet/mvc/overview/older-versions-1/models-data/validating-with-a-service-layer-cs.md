---
uid: mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-cs
title: Sprawdzanie poprawności przy użyciu warstwy usługC#() | Microsoft Docs
author: StephenWalther
description: Dowiedz się, jak przenieść logikę walidacji z akcji kontrolera i do oddzielnej warstwy usług. W tym samouczku Stephen Walther wyjaśnia, jak to zrobić...
ms.author: riande
ms.date: 03/02/2009
ms.assetid: 4eabc535-b8a1-43f5-bb99-cfeb86db0fca
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-cs
msc.type: authoredcontent
ms.openlocfilehash: da1a1c9cc79a452eb0d7597810e86f7bcf6cd179
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78542862"
---
# <a name="validating-with-a-service-layer-c"></a><span data-ttu-id="058ca-104">Walidacja z użyciem warstwy usług (C#)</span><span class="sxs-lookup"><span data-stu-id="058ca-104">Validating with a Service Layer (C#)</span></span>

<span data-ttu-id="058ca-105">Autor [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="058ca-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="058ca-106">Dowiedz się, jak przenieść logikę walidacji z akcji kontrolera i do oddzielnej warstwy usług.</span><span class="sxs-lookup"><span data-stu-id="058ca-106">Learn how to move your validation logic out of your controller actions and into a separate service layer.</span></span> <span data-ttu-id="058ca-107">W tym samouczku Stephen Walther wyjaśnia, jak można zachować wyraziste rozdzielenie problemów przez odizolowanie warstwy usług od warstwy kontrolera.</span><span class="sxs-lookup"><span data-stu-id="058ca-107">In this tutorial, Stephen Walther explains how you can maintain a sharp separation of concerns by isolating your service layer from your controller layer.</span></span>

<span data-ttu-id="058ca-108">Celem tego samouczka jest opisywanie jednej metody weryfikacji w aplikacji ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="058ca-108">The goal of this tutorial is to describe one method of performing validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="058ca-109">W tym samouczku dowiesz się, jak przenieść logikę walidacji z kontrolerów i do oddzielnej warstwy usług.</span><span class="sxs-lookup"><span data-stu-id="058ca-109">In this tutorial, you learn how to move your validation logic out of your controllers and into a separate service layer.</span></span>

## <a name="separating-concerns"></a><span data-ttu-id="058ca-110">Rozdzielenie problemów</span><span class="sxs-lookup"><span data-stu-id="058ca-110">Separating Concerns</span></span>

<span data-ttu-id="058ca-111">Podczas kompilowania aplikacji ASP.NET MVC nie należy umieszczać logiki bazy danych wewnątrz akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="058ca-111">When you build an ASP.NET MVC application, you should not place your database logic inside your controller actions.</span></span> <span data-ttu-id="058ca-112">Mieszanie logiki bazy danych i kontrolera sprawia, że aplikacja jest trudniejsza do utrzymania w czasie.</span><span class="sxs-lookup"><span data-stu-id="058ca-112">Mixing your database and controller logic makes your application more difficult to maintain over time.</span></span> <span data-ttu-id="058ca-113">Zalecenie polega na umieszczeniu całej logiki bazy danych w osobnej warstwie repozytorium.</span><span class="sxs-lookup"><span data-stu-id="058ca-113">The recommendation is that you place all of your database logic in a separate repository layer.</span></span>

<span data-ttu-id="058ca-114">Na przykład lista 1 zawiera proste repozytorium o nazwie ProductRepository.</span><span class="sxs-lookup"><span data-stu-id="058ca-114">For example, Listing 1 contains a simple repository named the ProductRepository.</span></span> <span data-ttu-id="058ca-115">Repozytorium produktu zawiera cały kod dostępu do danych dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="058ca-115">The product repository contains all of the data access code for the application.</span></span> <span data-ttu-id="058ca-116">Ta lista zawiera również interfejs IProductRepository, który implementuje repozytorium produktu.</span><span class="sxs-lookup"><span data-stu-id="058ca-116">The listing also includes the IProductRepository interface that the product repository implements.</span></span>

<span data-ttu-id="058ca-117">**Lista 1--Models\ProductRepository.cs**</span><span class="sxs-lookup"><span data-stu-id="058ca-117">**Listing 1 -- Models\ProductRepository.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample1.cs)]

<span data-ttu-id="058ca-118">Kontroler na liście 2 używa warstwy repozytorium w ramach akcji index () i Create ().</span><span class="sxs-lookup"><span data-stu-id="058ca-118">The controller in Listing 2 uses the repository layer in both its Index() and Create() actions.</span></span> <span data-ttu-id="058ca-119">Należy zauważyć, że ten kontroler nie zawiera żadnych logiki bazy danych.</span><span class="sxs-lookup"><span data-stu-id="058ca-119">Notice that this controller does not contain any database logic.</span></span> <span data-ttu-id="058ca-120">Utworzenie warstwy repozytorium pozwala zachować czyste rozdzielenie problemów.</span><span class="sxs-lookup"><span data-stu-id="058ca-120">Creating a repository layer enables you to maintain a clean separation of concerns.</span></span> <span data-ttu-id="058ca-121">Kontrolery są odpowiedzialni za logikę kontroli przepływu aplikacji i repozytorium są odpowiedzialne za logikę dostępu do danych.</span><span class="sxs-lookup"><span data-stu-id="058ca-121">Controllers are responsible for application flow control logic and the repository is responsible for data access logic.</span></span>

<span data-ttu-id="058ca-122">**Lista 2 — Controllers\ProductController.cs**</span><span class="sxs-lookup"><span data-stu-id="058ca-122">**Listing 2 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample2.cs)]

## <a name="creating-a-service-layer"></a><span data-ttu-id="058ca-123">Tworzenie warstwy usług</span><span class="sxs-lookup"><span data-stu-id="058ca-123">Creating a Service Layer</span></span>

<span data-ttu-id="058ca-124">W związku z tym logika kontroli przepływu aplikacji należy do kontrolera, a logika dostępu do danych należy do repozytorium.</span><span class="sxs-lookup"><span data-stu-id="058ca-124">So, application flow control logic belongs in a controller and data access logic belongs in a repository.</span></span> <span data-ttu-id="058ca-125">W takim przypadku należy umieścić logikę walidacji?</span><span class="sxs-lookup"><span data-stu-id="058ca-125">In that case, where do you put your validation logic?</span></span> <span data-ttu-id="058ca-126">Jedną z opcji jest umieszczenie logiki walidacji w *warstwie usług*.</span><span class="sxs-lookup"><span data-stu-id="058ca-126">One option is to place your validation logic in a *service layer*.</span></span>

<span data-ttu-id="058ca-127">Warstwa usługi jest dodatkową warstwą w aplikacji ASP.NET MVC, która koryguje komunikację między kontrolerem a warstwą repozytorium.</span><span class="sxs-lookup"><span data-stu-id="058ca-127">A service layer is an additional layer in an ASP.NET MVC application that mediates communication between a controller and repository layer.</span></span> <span data-ttu-id="058ca-128">Warstwa usługi zawiera logikę biznesową.</span><span class="sxs-lookup"><span data-stu-id="058ca-128">The service layer contains business logic.</span></span> <span data-ttu-id="058ca-129">W szczególności zawiera logikę walidacji.</span><span class="sxs-lookup"><span data-stu-id="058ca-129">In particular, it contains validation logic.</span></span>

<span data-ttu-id="058ca-130">Na przykład warstwa usługi produktu na liście 3 ma metodę/produkt ().</span><span class="sxs-lookup"><span data-stu-id="058ca-130">For example, the product service layer in Listing 3 has a CreateProduct() method.</span></span> <span data-ttu-id="058ca-131">Metoda do produkcji () wywołuje metodę ValidateProduct (), aby zweryfikować nowy produkt przed przekazaniem produktu do repozytorium produktu.</span><span class="sxs-lookup"><span data-stu-id="058ca-131">The CreateProduct() method calls the ValidateProduct() method to validate a new product before passing the product to the product repository.</span></span>

<span data-ttu-id="058ca-132">**Lista 3 — Models\ProductService.cs**</span><span class="sxs-lookup"><span data-stu-id="058ca-132">**Listing 3 - Models\ProductService.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample3.cs)]

<span data-ttu-id="058ca-133">Kontroler produktu został zaktualizowany na liście 4, aby użyć warstwy usługi zamiast warstwy repozytorium.</span><span class="sxs-lookup"><span data-stu-id="058ca-133">The Product controller has been updated in Listing 4 to use the service layer instead of the repository layer.</span></span> <span data-ttu-id="058ca-134">Warstwa kontrolera jest wychodząca do warstwy usługi.</span><span class="sxs-lookup"><span data-stu-id="058ca-134">The controller layer talks to the service layer.</span></span> <span data-ttu-id="058ca-135">Warstwa usługi jest wychodząca do warstwy repozytorium.</span><span class="sxs-lookup"><span data-stu-id="058ca-135">The service layer talks to the repository layer.</span></span> <span data-ttu-id="058ca-136">Każda warstwa ma osobną odpowiedzialność.</span><span class="sxs-lookup"><span data-stu-id="058ca-136">Each layer has a separate responsibility.</span></span>

<span data-ttu-id="058ca-137">**Lista 4 — Controllers\ProductController.cs**</span><span class="sxs-lookup"><span data-stu-id="058ca-137">**Listing 4 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample4.cs)]

<span data-ttu-id="058ca-138">Należy zauważyć, że usługa produktu jest tworzona w konstruktorze produktu.</span><span class="sxs-lookup"><span data-stu-id="058ca-138">Notice that the product service is created in the product controller constructor.</span></span> <span data-ttu-id="058ca-139">Po utworzeniu usługi produktu słownik stanu modelu jest przesyłany do usługi.</span><span class="sxs-lookup"><span data-stu-id="058ca-139">When the product service is created, the model state dictionary is passed to the service.</span></span> <span data-ttu-id="058ca-140">Usługa produktu używa stanu modelu do przekazywania komunikatów o błędach walidacji z powrotem do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="058ca-140">The product service uses model state to pass validation error messages back to the controller.</span></span>

## <a name="decoupling-the-service-layer"></a><span data-ttu-id="058ca-141">Oddzielenie warstwy usług</span><span class="sxs-lookup"><span data-stu-id="058ca-141">Decoupling the Service Layer</span></span>

<span data-ttu-id="058ca-142">Nie powiodło się izolowanie warstw kontrolera i usług.</span><span class="sxs-lookup"><span data-stu-id="058ca-142">We have failed to isolate the controller and service layers in one respect.</span></span> <span data-ttu-id="058ca-143">Warstwy kontrolera i usługi komunikują się za pomocą stanu modelu.</span><span class="sxs-lookup"><span data-stu-id="058ca-143">The controller and service layers communicate through model state.</span></span> <span data-ttu-id="058ca-144">Innymi słowy, warstwa usługi ma zależność od określonej funkcji struktury MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="058ca-144">In other words, the service layer has a dependency on a particular feature of the ASP.NET MVC framework.</span></span>

<span data-ttu-id="058ca-145">Chcemy odizolować warstwę usług od naszej warstwy kontrolera tak dużo, jak to możliwe.</span><span class="sxs-lookup"><span data-stu-id="058ca-145">We want to isolate the service layer from our controller layer as much as possible.</span></span> <span data-ttu-id="058ca-146">Teoretycznie powinno być możliwe użycie warstwy usług z dowolnym typem aplikacji, a nie tylko aplikacji ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="058ca-146">In theory, we should be able to use the service layer with any type of application and not only an ASP.NET MVC application.</span></span> <span data-ttu-id="058ca-147">Na przykład w przyszłości możemy chcieć utworzyć fronton platformy WPF dla naszej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="058ca-147">For example, in the future, we might want to build a WPF front-end for our application.</span></span> <span data-ttu-id="058ca-148">Powinienmy znaleźć sposób usunięcia zależności od stanu modelu MVC ASP.NET z naszej warstwy usług.</span><span class="sxs-lookup"><span data-stu-id="058ca-148">We should find a way to remove the dependency on ASP.NET MVC model state from our service layer.</span></span>

<span data-ttu-id="058ca-149">Na liście 5 warstwa usługi została zaktualizowana, aby nie korzystała już z stanu modelu.</span><span class="sxs-lookup"><span data-stu-id="058ca-149">In Listing 5, the service layer has been updated so that it no longer uses model state.</span></span> <span data-ttu-id="058ca-150">Zamiast tego używa klasy, która implementuje interfejs IValidationDictionary.</span><span class="sxs-lookup"><span data-stu-id="058ca-150">Instead, it uses any class that implements the IValidationDictionary interface.</span></span>

<span data-ttu-id="058ca-151">**Lista 5-Models\ProductService.cs (odłączony)**</span><span class="sxs-lookup"><span data-stu-id="058ca-151">**Listing 5 - Models\ProductService.cs (decoupled)**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample5.cs)]

<span data-ttu-id="058ca-152">Interfejs IValidationDictionary został zdefiniowany na liście 6.</span><span class="sxs-lookup"><span data-stu-id="058ca-152">The IValidationDictionary interface is defined in Listing 6.</span></span> <span data-ttu-id="058ca-153">Ten prosty interfejs ma pojedynczą metodę i pojedynczą właściwość.</span><span class="sxs-lookup"><span data-stu-id="058ca-153">This simple interface has a single method and a single property.</span></span>

<span data-ttu-id="058ca-154">**Lista 6 — Models\IValidationDictionary.cs**</span><span class="sxs-lookup"><span data-stu-id="058ca-154">**Listing 6 - Models\IValidationDictionary.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample6.cs)]

<span data-ttu-id="058ca-155">Klasa na liście 7 o nazwie Klasa ModelStateWrapper implementuje interfejs IValidationDictionary.</span><span class="sxs-lookup"><span data-stu-id="058ca-155">The class in Listing 7, named the ModelStateWrapper class, implements the IValidationDictionary interface.</span></span> <span data-ttu-id="058ca-156">Można utworzyć wystąpienie klasy ModelStateWrapper przez przekazanie słownika stanu modelu do konstruktora.</span><span class="sxs-lookup"><span data-stu-id="058ca-156">You can instantiate the ModelStateWrapper class by passing a model state dictionary to the constructor.</span></span>

<span data-ttu-id="058ca-157">**Lista 7 — Models\ModelStateWrapper.cs**</span><span class="sxs-lookup"><span data-stu-id="058ca-157">**Listing 7 - Models\ModelStateWrapper.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample7.cs)]

<span data-ttu-id="058ca-158">Na koniec zaktualizowany kontroler na liście 8 używa ModelStateWrapper podczas tworzenia warstwy usług w jej konstruktorze.</span><span class="sxs-lookup"><span data-stu-id="058ca-158">Finally, the updated controller in Listing 8 uses the ModelStateWrapper when creating the service layer in its constructor.</span></span>

<span data-ttu-id="058ca-159">**Lista 8-Controllers\ProductController.cs**</span><span class="sxs-lookup"><span data-stu-id="058ca-159">**Listing 8 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample8.cs)]

<span data-ttu-id="058ca-160">Użycie interfejsu IValidationDictionary i klasy ModelStateWrapper umożliwia nam całkowite odizolowanie naszej warstwy usług od naszej warstwy kontrolera.</span><span class="sxs-lookup"><span data-stu-id="058ca-160">Using the IValidationDictionary interface and the ModelStateWrapper class enables us to completely isolate our service layer from our controller layer.</span></span> <span data-ttu-id="058ca-161">Warstwa usługi nie jest już zależna od stanu modelu.</span><span class="sxs-lookup"><span data-stu-id="058ca-161">The service layer is no longer dependent on model state.</span></span> <span data-ttu-id="058ca-162">Do warstwy usług można przekazać dowolną klasę, która implementuje interfejs IValidationDictionary.</span><span class="sxs-lookup"><span data-stu-id="058ca-162">You can pass any class that implements the IValidationDictionary interface to the service layer.</span></span> <span data-ttu-id="058ca-163">Na przykład aplikacja WPF może zaimplementować interfejs IValidationDictionary przy użyciu prostej klasy kolekcji.</span><span class="sxs-lookup"><span data-stu-id="058ca-163">For example, a WPF application might implement the IValidationDictionary interface with a simple collection class.</span></span>

## <a name="summary"></a><span data-ttu-id="058ca-164">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="058ca-164">Summary</span></span>

<span data-ttu-id="058ca-165">Celem tego samouczka było omawianie jednego podejścia do wykonywania walidacji w aplikacji ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="058ca-165">The goal of this tutorial was to discuss one approach to performing validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="058ca-166">W tym samouczku przedstawiono sposób przenoszenia wszystkich logiki walidacji z kontrolerów i do oddzielnej warstwy usług.</span><span class="sxs-lookup"><span data-stu-id="058ca-166">In this tutorial, you learned how to move all of your validation logic out of your controllers and into a separate service layer.</span></span> <span data-ttu-id="058ca-167">Poznasz również sposób izolowania warstwy usług od warstwy kontrolera przez utworzenie klasy ModelStateWrapper.</span><span class="sxs-lookup"><span data-stu-id="058ca-167">You also learned how to isolate your service layer from your controller layer by creating a ModelStateWrapper class.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="058ca-168">[Poprzednie](validating-with-the-idataerrorinfo-interface-cs.md)
> [dalej](validation-with-the-data-annotation-validators-cs.md)</span><span class="sxs-lookup"><span data-stu-id="058ca-168">[Previous](validating-with-the-idataerrorinfo-interface-cs.md)
[Next](validation-with-the-data-annotation-validators-cs.md)</span></span>

---
uid: mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-vb
title: Sprawdzanie poprawności przy użyciu warstwy usług (VB) | Microsoft Docs
author: StephenWalther
description: Dowiedz się, jak przenieść logikę walidacji z akcji kontrolera i do oddzielnej warstwy usług. W tym samouczku Stephen Walther wyjaśnia, jak to zrobić...
ms.author: riande
ms.date: 03/02/2009
ms.assetid: 344bb38e-4965-4c47-bda1-f6d29ae5b83a
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-vb
msc.type: authoredcontent
ms.openlocfilehash: 704657ffe6f50eaf3eb0d91d0d334567003ab7f4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78542827"
---
# <a name="validating-with-a-service-layer-vb"></a><span data-ttu-id="529e5-104">Walidacja z użyciem warstwy usług (VB)</span><span class="sxs-lookup"><span data-stu-id="529e5-104">Validating with a Service Layer (VB)</span></span>

<span data-ttu-id="529e5-105">Autor [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="529e5-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="529e5-106">Dowiedz się, jak przenieść logikę walidacji z akcji kontrolera i do oddzielnej warstwy usług.</span><span class="sxs-lookup"><span data-stu-id="529e5-106">Learn how to move your validation logic out of your controller actions and into a separate service layer.</span></span> <span data-ttu-id="529e5-107">W tym samouczku Stephen Walther wyjaśnia, jak można zachować wyraziste rozdzielenie problemów przez odizolowanie warstwy usług od warstwy kontrolera.</span><span class="sxs-lookup"><span data-stu-id="529e5-107">In this tutorial, Stephen Walther explains how you can maintain a sharp separation of concerns by isolating your service layer from your controller layer.</span></span>

<span data-ttu-id="529e5-108">Celem tego samouczka jest opisywanie jednej metody weryfikacji w aplikacji ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="529e5-108">The goal of this tutorial is to describe one method of performing validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="529e5-109">W tym samouczku dowiesz się, jak przenieść logikę walidacji z kontrolerów i do oddzielnej warstwy usług.</span><span class="sxs-lookup"><span data-stu-id="529e5-109">In this tutorial, you learn how to move your validation logic out of your controllers and into a separate service layer.</span></span>

## <a name="separating-concerns"></a><span data-ttu-id="529e5-110">Rozdzielenie problemów</span><span class="sxs-lookup"><span data-stu-id="529e5-110">Separating Concerns</span></span>

<span data-ttu-id="529e5-111">Podczas kompilowania aplikacji ASP.NET MVC nie należy umieszczać logiki bazy danych wewnątrz akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="529e5-111">When you build an ASP.NET MVC application, you should not place your database logic inside your controller actions.</span></span> <span data-ttu-id="529e5-112">Mieszanie logiki bazy danych i kontrolera sprawia, że aplikacja jest trudniejsza do utrzymania w czasie.</span><span class="sxs-lookup"><span data-stu-id="529e5-112">Mixing your database and controller logic makes your application more difficult to maintain over time.</span></span> <span data-ttu-id="529e5-113">Zalecenie polega na umieszczeniu całej logiki bazy danych w osobnej warstwie repozytorium.</span><span class="sxs-lookup"><span data-stu-id="529e5-113">The recommendation is that you place all of your database logic in a separate repository layer.</span></span>

<span data-ttu-id="529e5-114">Na przykład lista 1 zawiera proste repozytorium o nazwie ProductRepository.</span><span class="sxs-lookup"><span data-stu-id="529e5-114">For example, Listing 1 contains a simple repository named the ProductRepository.</span></span> <span data-ttu-id="529e5-115">Repozytorium produktu zawiera cały kod dostępu do danych dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="529e5-115">The product repository contains all of the data access code for the application.</span></span> <span data-ttu-id="529e5-116">Ta lista zawiera również interfejs IProductRepository, który implementuje repozytorium produktu.</span><span class="sxs-lookup"><span data-stu-id="529e5-116">The listing also includes the IProductRepository interface that the product repository implements.</span></span>

<span data-ttu-id="529e5-117">**Lista 1 — Models\ProductRepository.vb**</span><span class="sxs-lookup"><span data-stu-id="529e5-117">**Listing 1 - Models\ProductRepository.vb**</span></span>

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample1.vb)]

<span data-ttu-id="529e5-118">Kontroler na liście 2 używa warstwy repozytorium w ramach akcji index () i Create ().</span><span class="sxs-lookup"><span data-stu-id="529e5-118">The controller in Listing 2 uses the repository layer in both its Index() and Create() actions.</span></span> <span data-ttu-id="529e5-119">Należy zauważyć, że ten kontroler nie zawiera żadnych logiki bazy danych.</span><span class="sxs-lookup"><span data-stu-id="529e5-119">Notice that this controller does not contain any database logic.</span></span> <span data-ttu-id="529e5-120">Utworzenie warstwy repozytorium pozwala zachować czyste rozdzielenie problemów.</span><span class="sxs-lookup"><span data-stu-id="529e5-120">Creating a repository layer enables you to maintain a clean separation of concerns.</span></span> <span data-ttu-id="529e5-121">Kontrolery są odpowiedzialni za logikę kontroli przepływu aplikacji i repozytorium są odpowiedzialne za logikę dostępu do danych.</span><span class="sxs-lookup"><span data-stu-id="529e5-121">Controllers are responsible for application flow control logic and the repository is responsible for data access logic.</span></span>

<span data-ttu-id="529e5-122">**Lista 2 — Controllers\ProductController.vb**</span><span class="sxs-lookup"><span data-stu-id="529e5-122">**Listing 2 - Controllers\ProductController.vb**</span></span>

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample2.vb)]

## <a name="creating-a-service-layer"></a><span data-ttu-id="529e5-123">Tworzenie warstwy usług</span><span class="sxs-lookup"><span data-stu-id="529e5-123">Creating a Service Layer</span></span>

<span data-ttu-id="529e5-124">W związku z tym logika kontroli przepływu aplikacji należy do kontrolera, a logika dostępu do danych należy do repozytorium.</span><span class="sxs-lookup"><span data-stu-id="529e5-124">So, application flow control logic belongs in a controller and data access logic belongs in a repository.</span></span> <span data-ttu-id="529e5-125">W takim przypadku należy umieścić logikę walidacji?</span><span class="sxs-lookup"><span data-stu-id="529e5-125">In that case, where do you put your validation logic?</span></span> <span data-ttu-id="529e5-126">Jedną z opcji jest umieszczenie logiki walidacji w *warstwie usług*.</span><span class="sxs-lookup"><span data-stu-id="529e5-126">One option is to place your validation logic in a *service layer*.</span></span>

<span data-ttu-id="529e5-127">Warstwa usługi jest dodatkową warstwą w aplikacji ASP.NET MVC, która koryguje komunikację między kontrolerem a warstwą repozytorium.</span><span class="sxs-lookup"><span data-stu-id="529e5-127">A service layer is an additional layer in an ASP.NET MVC application that mediates communication between a controller and repository layer.</span></span> <span data-ttu-id="529e5-128">Warstwa usługi zawiera logikę biznesową.</span><span class="sxs-lookup"><span data-stu-id="529e5-128">The service layer contains business logic.</span></span> <span data-ttu-id="529e5-129">W szczególności zawiera logikę walidacji.</span><span class="sxs-lookup"><span data-stu-id="529e5-129">In particular, it contains validation logic.</span></span>

<span data-ttu-id="529e5-130">Na przykład warstwa usługi produktu na liście 3 ma metodę/produkt ().</span><span class="sxs-lookup"><span data-stu-id="529e5-130">For example, the product service layer in Listing 3 has a CreateProduct() method.</span></span> <span data-ttu-id="529e5-131">Metoda do produkcji () wywołuje metodę ValidateProduct (), aby zweryfikować nowy produkt przed przekazaniem produktu do repozytorium produktu.</span><span class="sxs-lookup"><span data-stu-id="529e5-131">The CreateProduct() method calls the ValidateProduct() method to validate a new product before passing the product to the product repository.</span></span>

<span data-ttu-id="529e5-132">**Lista 3 — Models\ProductService.vb**</span><span class="sxs-lookup"><span data-stu-id="529e5-132">**Listing 3 - Models\ProductService.vb**</span></span>

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample3.vb)]

<span data-ttu-id="529e5-133">Kontroler produktu został zaktualizowany na liście 4, aby użyć warstwy usługi zamiast warstwy repozytorium.</span><span class="sxs-lookup"><span data-stu-id="529e5-133">The Product controller has been updated in Listing 4 to use the service layer instead of the repository layer.</span></span> <span data-ttu-id="529e5-134">Warstwa kontrolera jest wychodząca do warstwy usługi.</span><span class="sxs-lookup"><span data-stu-id="529e5-134">The controller layer talks to the service layer.</span></span> <span data-ttu-id="529e5-135">Warstwa usługi jest wychodząca do warstwy repozytorium.</span><span class="sxs-lookup"><span data-stu-id="529e5-135">The service layer talks to the repository layer.</span></span> <span data-ttu-id="529e5-136">Każda warstwa ma osobną odpowiedzialność.</span><span class="sxs-lookup"><span data-stu-id="529e5-136">Each layer has a separate responsibility.</span></span>

<span data-ttu-id="529e5-137">**Lista 4 — Controllers\ProductController.vb**</span><span class="sxs-lookup"><span data-stu-id="529e5-137">**Listing 4 - Controllers\ProductController.vb**</span></span>

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample4.vb)]

<span data-ttu-id="529e5-138">Należy zauważyć, że usługa produktu jest tworzona w konstruktorze produktu.</span><span class="sxs-lookup"><span data-stu-id="529e5-138">Notice that the product service is created in the product controller constructor.</span></span> <span data-ttu-id="529e5-139">Po utworzeniu usługi produktu słownik stanu modelu jest przesyłany do usługi.</span><span class="sxs-lookup"><span data-stu-id="529e5-139">When the product service is created, the model state dictionary is passed to the service.</span></span> <span data-ttu-id="529e5-140">Usługa produktu używa stanu modelu do przekazywania komunikatów o błędach walidacji z powrotem do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="529e5-140">The product service uses model state to pass validation error messages back to the controller.</span></span>

## <a name="decoupling-the-service-layer"></a><span data-ttu-id="529e5-141">Oddzielenie warstwy usług</span><span class="sxs-lookup"><span data-stu-id="529e5-141">Decoupling the Service Layer</span></span>

<span data-ttu-id="529e5-142">Nie powiodło się izolowanie warstw kontrolera i usług.</span><span class="sxs-lookup"><span data-stu-id="529e5-142">We have failed to isolate the controller and service layers in one respect.</span></span> <span data-ttu-id="529e5-143">Warstwy kontrolera i usługi komunikują się za pomocą stanu modelu.</span><span class="sxs-lookup"><span data-stu-id="529e5-143">The controller and service layers communicate through model state.</span></span> <span data-ttu-id="529e5-144">Innymi słowy, warstwa usługi ma zależność od określonej funkcji struktury MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="529e5-144">In other words, the service layer has a dependency on a particular feature of the ASP.NET MVC framework.</span></span>

<span data-ttu-id="529e5-145">Chcemy odizolować warstwę usług od naszej warstwy kontrolera tak dużo, jak to możliwe.</span><span class="sxs-lookup"><span data-stu-id="529e5-145">We want to isolate the service layer from our controller layer as much as possible.</span></span> <span data-ttu-id="529e5-146">Teoretycznie powinno być możliwe użycie warstwy usług z dowolnym typem aplikacji, a nie tylko aplikacji ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="529e5-146">In theory, we should be able to use the service layer with any type of application and not only an ASP.NET MVC application.</span></span> <span data-ttu-id="529e5-147">Na przykład w przyszłości możemy chcieć utworzyć fronton platformy WPF dla naszej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="529e5-147">For example, in the future, we might want to build a WPF front-end for our application.</span></span> <span data-ttu-id="529e5-148">Powinienmy znaleźć sposób usunięcia zależności od stanu modelu MVC ASP.NET z naszej warstwy usług.</span><span class="sxs-lookup"><span data-stu-id="529e5-148">We should find a way to remove the dependency on ASP.NET MVC model state from our service layer.</span></span>

<span data-ttu-id="529e5-149">Na liście 5 warstwa usługi została zaktualizowana, aby nie korzystała już z stanu modelu.</span><span class="sxs-lookup"><span data-stu-id="529e5-149">In Listing 5, the service layer has been updated so that it no longer uses model state.</span></span> <span data-ttu-id="529e5-150">Zamiast tego używa klasy, która implementuje interfejs IValidationDictionary.</span><span class="sxs-lookup"><span data-stu-id="529e5-150">Instead, it uses any class that implements the IValidationDictionary interface.</span></span>

<span data-ttu-id="529e5-151">**Lista 5-Models\ProductService.vb (odłączony)**</span><span class="sxs-lookup"><span data-stu-id="529e5-151">**Listing 5 - Models\ProductService.vb (decoupled)**</span></span>

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample5.vb)]

<span data-ttu-id="529e5-152">Interfejs IValidationDictionary został zdefiniowany na liście 6.</span><span class="sxs-lookup"><span data-stu-id="529e5-152">The IValidationDictionary interface is defined in Listing 6.</span></span> <span data-ttu-id="529e5-153">Ten prosty interfejs ma pojedynczą metodę i pojedynczą właściwość.</span><span class="sxs-lookup"><span data-stu-id="529e5-153">This simple interface has a single method and a single property.</span></span>

<span data-ttu-id="529e5-154">**Lista 6 — Models\IValidationDictionary.cs**</span><span class="sxs-lookup"><span data-stu-id="529e5-154">**Listing 6 - Models\IValidationDictionary.cs**</span></span>

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample6.vb)]

<span data-ttu-id="529e5-155">Klasa na liście 7 o nazwie Klasa ModelStateWrapper implementuje interfejs IValidationDictionary.</span><span class="sxs-lookup"><span data-stu-id="529e5-155">The class in Listing 7, named the ModelStateWrapper class, implements the IValidationDictionary interface.</span></span> <span data-ttu-id="529e5-156">Można utworzyć wystąpienie klasy ModelStateWrapper przez przekazanie słownika stanu modelu do konstruktora.</span><span class="sxs-lookup"><span data-stu-id="529e5-156">You can instantiate the ModelStateWrapper class by passing a model state dictionary to the constructor.</span></span>

<span data-ttu-id="529e5-157">**Lista 7 — Models\ModelStateWrapper.vb**</span><span class="sxs-lookup"><span data-stu-id="529e5-157">**Listing 7 - Models\ModelStateWrapper.vb**</span></span>

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample7.vb)]

<span data-ttu-id="529e5-158">Na koniec zaktualizowany kontroler na liście 8 używa ModelStateWrapper podczas tworzenia warstwy usług w jej konstruktorze.</span><span class="sxs-lookup"><span data-stu-id="529e5-158">Finally, the updated controller in Listing 8 uses the ModelStateWrapper when creating the service layer in its constructor.</span></span>

<span data-ttu-id="529e5-159">**Lista 8-Controllers\ProductController.vb**</span><span class="sxs-lookup"><span data-stu-id="529e5-159">**Listing 8 - Controllers\ProductController.vb**</span></span>

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample8.vb)]

<span data-ttu-id="529e5-160">Użycie interfejsu IValidationDictionary i klasy ModelStateWrapper umożliwia nam całkowite odizolowanie naszej warstwy usług od naszej warstwy kontrolera.</span><span class="sxs-lookup"><span data-stu-id="529e5-160">Using the IValidationDictionary interface and the ModelStateWrapper class enables us to completely isolate our service layer from our controller layer.</span></span> <span data-ttu-id="529e5-161">Warstwa usługi nie jest już zależna od stanu modelu.</span><span class="sxs-lookup"><span data-stu-id="529e5-161">The service layer is no longer dependent on model state.</span></span> <span data-ttu-id="529e5-162">Do warstwy usług można przekazać dowolną klasę, która implementuje interfejs IValidationDictionary.</span><span class="sxs-lookup"><span data-stu-id="529e5-162">You can pass any class that implements the IValidationDictionary interface to the service layer.</span></span> <span data-ttu-id="529e5-163">Na przykład aplikacja WPF może zaimplementować interfejs IValidationDictionary przy użyciu prostej klasy kolekcji.</span><span class="sxs-lookup"><span data-stu-id="529e5-163">For example, a WPF application might implement the IValidationDictionary interface with a simple collection class.</span></span>

## <a name="summary"></a><span data-ttu-id="529e5-164">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="529e5-164">Summary</span></span>

<span data-ttu-id="529e5-165">Celem tego samouczka było omawianie jednego podejścia do wykonywania walidacji w aplikacji ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="529e5-165">The goal of this tutorial was to discuss one approach to performing validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="529e5-166">W tym samouczku przedstawiono sposób przenoszenia wszystkich logiki walidacji z kontrolerów i do oddzielnej warstwy usług.</span><span class="sxs-lookup"><span data-stu-id="529e5-166">In this tutorial, you learned how to move all of your validation logic out of your controllers and into a separate service layer.</span></span> <span data-ttu-id="529e5-167">Poznasz również sposób izolowania warstwy usług od warstwy kontrolera przez utworzenie klasy ModelStateWrapper.</span><span class="sxs-lookup"><span data-stu-id="529e5-167">You also learned how to isolate your service layer from your controller layer by creating a ModelStateWrapper class.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="529e5-168">[Poprzednie](validating-with-the-idataerrorinfo-interface-vb.md)
> [dalej](validation-with-the-data-annotation-validators-vb.md)</span><span class="sxs-lookup"><span data-stu-id="529e5-168">[Previous](validating-with-the-idataerrorinfo-interface-vb.md)
[Next](validation-with-the-data-annotation-validators-vb.md)</span></span>

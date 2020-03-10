---
uid: web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
title: Walidacja modelu w ASP.NET Web API-ASP.NET 4. x
author: MikeWasson
description: Przegląd weryfikacji modelu w interfejsie Web API ASP.NET dla ASP.NET 4. x.
ms.author: riande
ms.date: 07/20/2012
ms.custom: seoapril2019
ms.assetid: 7d061207-22b8-4883-bafa-e89b1e7749ca
msc.legacyurl: /web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 531a66b7ab642bd012663517640f2766f1917f25
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557240"
---
# <a name="model-validation-in-aspnet-web-api"></a><span data-ttu-id="f1b05-103">Walidacja modelu w interfejsie API sieci Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="f1b05-103">Model Validation in ASP.NET Web API</span></span>

<span data-ttu-id="f1b05-104">według [Jan Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="f1b05-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="f1b05-105">W tym artykule przedstawiono sposób dodawania adnotacji do modeli, używania adnotacji do sprawdzania poprawności danych i obsługi błędów walidacji w interfejsie API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="f1b05-105">This article shows how to annotate your models, use the annotations for data validation, and handle validation errors in your web API.</span></span> <span data-ttu-id="f1b05-106">Gdy klient wysyła dane do internetowego interfejsu API, często chcesz sprawdzić poprawność danych przed wykonaniem jakiegokolwiek przetwarzania.</span><span class="sxs-lookup"><span data-stu-id="f1b05-106">When a client sends data to your web API, often you want to validate the data before doing any processing.</span></span> 

## <a name="data-annotations"></a><span data-ttu-id="f1b05-107">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="f1b05-107">Data Annotations</span></span>

<span data-ttu-id="f1b05-108">W ASP.NET Web API można używać atrybutów z przestrzeni nazw [System. ComponentModel. DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) do ustawiania reguł walidacji dla właściwości w modelu.</span><span class="sxs-lookup"><span data-stu-id="f1b05-108">In ASP.NET Web API, you can use attributes from the [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) namespace to set validation rules for properties on your model.</span></span> <span data-ttu-id="f1b05-109">Rozważmy następujący model:</span><span class="sxs-lookup"><span data-stu-id="f1b05-109">Consider the following model:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="f1b05-110">Jeśli używasz walidacji modelu w ASP.NET MVC, powinno to wyglądać znajomo.</span><span class="sxs-lookup"><span data-stu-id="f1b05-110">If you have used model validation in ASP.NET MVC, this should look familiar.</span></span> <span data-ttu-id="f1b05-111">**Wymagany** atrybut wskazuje, że właściwość `Name` nie może mieć wartości null.</span><span class="sxs-lookup"><span data-stu-id="f1b05-111">The **Required** attribute says that the `Name` property must not be null.</span></span> <span data-ttu-id="f1b05-112">Atrybut **zakresu** wskazuje, że `Weight` musi należeć do zakresu od zero do 999.</span><span class="sxs-lookup"><span data-stu-id="f1b05-112">The **Range** attribute says that `Weight` must be between zero and 999.</span></span>

<span data-ttu-id="f1b05-113">Załóżmy, że klient wysyła żądanie POST z następującą reprezentacją JSON:</span><span class="sxs-lookup"><span data-stu-id="f1b05-113">Suppose that a client sends a POST request with the following JSON representation:</span></span>

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample2.json)]

<span data-ttu-id="f1b05-114">Można zobaczyć, że klient nie zawiera właściwości `Name`, która jest oznaczona jako wymagana.</span><span class="sxs-lookup"><span data-stu-id="f1b05-114">You can see that the client did not include the `Name` property, which is marked as required.</span></span> <span data-ttu-id="f1b05-115">Gdy interfejs API sieci Web konwertuje kod JSON na wystąpienie `Product`, sprawdza poprawność `Product` względem atrybutów walidacji.</span><span class="sxs-lookup"><span data-stu-id="f1b05-115">When Web API converts the JSON into a `Product` instance, it validates the `Product` against the validation attributes.</span></span> <span data-ttu-id="f1b05-116">W akcji kontrolera możesz sprawdzić, czy model jest prawidłowy:</span><span class="sxs-lookup"><span data-stu-id="f1b05-116">In your controller action, you can check whether the model is valid:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="f1b05-117">Walidacja modelu nie gwarantuje, że dane klienta są bezpieczne.</span><span class="sxs-lookup"><span data-stu-id="f1b05-117">Model validation does not guarantee that client data is safe.</span></span> <span data-ttu-id="f1b05-118">W innych warstwach aplikacji może być wymagana dodatkowa weryfikacja.</span><span class="sxs-lookup"><span data-stu-id="f1b05-118">Additional validation might be needed in other layers of the application.</span></span> <span data-ttu-id="f1b05-119">(Na przykład warstwa danych może wymusić ograniczenia klucza obcego). Samouczek [korzystający z interfejsu API sieci Web i Entity Framework](../data/using-web-api-with-entity-framework/part-1.md) Eksplorowanie niektórych z tych problemów.</span><span class="sxs-lookup"><span data-stu-id="f1b05-119">(For example, the data layer might enforce foreign key constraints.) The tutorial [Using Web API with Entity Framework](../data/using-web-api-with-entity-framework/part-1.md) explores some of these issues.</span></span>

<span data-ttu-id="f1b05-120">**"W obszarze Księgowanie"** : w obszarze Księgowanie występuje, gdy klient opuści pewne właściwości.</span><span class="sxs-lookup"><span data-stu-id="f1b05-120">**"Under-Posting"**: Under-posting happens when the client leaves out some properties.</span></span> <span data-ttu-id="f1b05-121">Załóżmy na przykład, że klient wysyła następujące elementy:</span><span class="sxs-lookup"><span data-stu-id="f1b05-121">For example, suppose the client sends the following:</span></span>

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample4.json)]

<span data-ttu-id="f1b05-122">W tym miejscu klient nie określił wartości dla `Price` lub `Weight`.</span><span class="sxs-lookup"><span data-stu-id="f1b05-122">Here, the client did not specify values for `Price` or `Weight`.</span></span> <span data-ttu-id="f1b05-123">Program formatujący JSON przypisuje wartość domyślną zero do brakujących właściwości.</span><span class="sxs-lookup"><span data-stu-id="f1b05-123">The JSON formatter assigns a default value of zero to the missing properties.</span></span>

![](model-validation-in-aspnet-web-api/_static/image1.png)

<span data-ttu-id="f1b05-124">Stan modelu jest prawidłowy, ponieważ zero jest prawidłową wartością dla tych właściwości.</span><span class="sxs-lookup"><span data-stu-id="f1b05-124">The model state is valid, because zero is a valid value for these properties.</span></span> <span data-ttu-id="f1b05-125">To, czy jest to problem, zależy od danego scenariusza.</span><span class="sxs-lookup"><span data-stu-id="f1b05-125">Whether this is a problem depends on your scenario.</span></span> <span data-ttu-id="f1b05-126">Na przykład w operacji aktualizacji można chcieć rozróżnić wartości "zero" i "nie ustawiono".</span><span class="sxs-lookup"><span data-stu-id="f1b05-126">For example, in an update operation, you might want to distinguish between "zero" and "not set."</span></span> <span data-ttu-id="f1b05-127">Aby wymusić ustawienie wartości przez klientów, należy wprowadzić wartość null i ustawić **wymagany** atrybut:</span><span class="sxs-lookup"><span data-stu-id="f1b05-127">To force clients to set a value, make the property nullable and set the **Required** attribute:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample5.cs?highlight=1-2)]

<span data-ttu-id="f1b05-128">**"Za pośrednictwem publikowania"** : Klient może również wysyłać *więcej* danych niż oczekiwano.</span><span class="sxs-lookup"><span data-stu-id="f1b05-128">**"Over-Posting"**: A client can also send *more* data than you expected.</span></span> <span data-ttu-id="f1b05-129">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="f1b05-129">For example:</span></span>

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample6.json)]

<span data-ttu-id="f1b05-130">W tym miejscu plik JSON zawiera właściwość ("Color"), która nie istnieje w modelu `Product`.</span><span class="sxs-lookup"><span data-stu-id="f1b05-130">Here, the JSON includes a property ("Color") that does not exist in the `Product` model.</span></span> <span data-ttu-id="f1b05-131">W takim przypadku program formatujący JSON po prostu ignoruje tę wartość.</span><span class="sxs-lookup"><span data-stu-id="f1b05-131">In this case, the JSON formatter simply ignores this value.</span></span> <span data-ttu-id="f1b05-132">(Program formatujący XML robi to tak samo). Nadmierne Księgowanie powoduje problemy, jeśli model ma właściwości, które są przeznaczone tylko do odczytu.</span><span class="sxs-lookup"><span data-stu-id="f1b05-132">(The XML formatter does the same.) Over-posting causes problems if your model has properties that you intended to be read-only.</span></span> <span data-ttu-id="f1b05-133">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="f1b05-133">For example:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="f1b05-134">Nie chcesz, aby użytkownicy mogli aktualizować Właściwość `IsAdmin` i Podnieś poziom uprawnień do administratorów.</span><span class="sxs-lookup"><span data-stu-id="f1b05-134">You don't want users to update the `IsAdmin` property and elevate themselves to administrators!</span></span> <span data-ttu-id="f1b05-135">Najbezpieczniejsza strategia polega na użyciu klasy modelu, która dokładnie pasuje do tego, co klient może wysłać:</span><span class="sxs-lookup"><span data-stu-id="f1b05-135">The safest strategy is to use a model class that exactly matches what the client is allowed to send:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample8.cs)]

> [!NOTE]
> <span data-ttu-id="f1b05-136">Wpis w blogu z programu Brada Wilson "[weryfikacja danych wejściowych a Walidacja modelu w ASP.NET MVC](http://bradwilson.typepad.com/blog/2010/01/input-validation-vs-model-validation-in-aspnet-mvc.html)" ma dobrą dyskusję na temat księgowania i nadmiernego księgowania.</span><span class="sxs-lookup"><span data-stu-id="f1b05-136">Brad Wilson's blog post "[Input Validation vs. Model Validation in ASP.NET MVC](http://bradwilson.typepad.com/blog/2010/01/input-validation-vs-model-validation-in-aspnet-mvc.html)" has a good discussion of under-posting and over-posting.</span></span> <span data-ttu-id="f1b05-137">Mimo że wpis dotyczy ASP.NET MVC 2, problemy nadal dotyczą interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="f1b05-137">Although the post is about ASP.NET MVC 2, the issues are still relevant to Web API.</span></span>

## <a name="handling-validation-errors"></a><span data-ttu-id="f1b05-138">Obsługa błędów walidacji</span><span class="sxs-lookup"><span data-stu-id="f1b05-138">Handling Validation Errors</span></span>

<span data-ttu-id="f1b05-139">Gdy Walidacja nie powiedzie się, internetowy interfejs API nie zwróci automatycznie błędu do klienta.</span><span class="sxs-lookup"><span data-stu-id="f1b05-139">Web API does not automatically return an error to the client when validation fails.</span></span> <span data-ttu-id="f1b05-140">Jest to akcja kontrolera, aby sprawdzić stan modelu i odpowiednio odpowiedzieć.</span><span class="sxs-lookup"><span data-stu-id="f1b05-140">It is up to the controller action to check the model state and respond appropriately.</span></span>

<span data-ttu-id="f1b05-141">Możesz również utworzyć filtr akcji, aby sprawdzić stan modelu przed wywołaniem akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="f1b05-141">You can also create an action filter to check the model state before the controller action is invoked.</span></span> <span data-ttu-id="f1b05-142">Poniżej znajduje się kod przykładowy:</span><span class="sxs-lookup"><span data-stu-id="f1b05-142">The following code shows an example:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample9.cs)]

<span data-ttu-id="f1b05-143">Jeśli walidacja modelu zakończy się niepowodzeniem, ten filtr zwróci odpowiedź HTTP zawierającą błędy walidacji.</span><span class="sxs-lookup"><span data-stu-id="f1b05-143">If model validation fails, this filter returns an HTTP response that contains the validation errors.</span></span> <span data-ttu-id="f1b05-144">W takim przypadku akcja kontrolera nie zostanie wywołana.</span><span class="sxs-lookup"><span data-stu-id="f1b05-144">In that case, the controller action is not invoked.</span></span>

[!code-console[Main](model-validation-in-aspnet-web-api/samples/sample10.cmd)]

<span data-ttu-id="f1b05-145">Aby zastosować ten filtr do wszystkich kontrolerów internetowego interfejsu API, Dodaj wystąpienie filtru do kolekcji **HttpConfiguration. filters** podczas konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="f1b05-145">To apply this filter to all Web API controllers, add an instance of the filter to the **HttpConfiguration.Filters** collection during configuration:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample11.cs)]

<span data-ttu-id="f1b05-146">Innym rozwiązaniem jest ustawienie filtru jako atrybutu na poszczególnych kontrolerach lub akcjach kontrolera:</span><span class="sxs-lookup"><span data-stu-id="f1b05-146">Another option is to set the filter as an attribute on individual controllers or controller actions:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample12.cs)]

---
uid: web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
title: Model sprawdzania poprawności we wzorcu ASP.NET Web API — ASP.NET 4.x
author: MikeWasson
description: Omówienie sprawdzania poprawności modelu programu ASP.NET Web API dla platformy ASP.NET 4.x.
ms.author: riande
ms.date: 07/20/2012
ms.custom: seoapril2019
ms.assetid: 7d061207-22b8-4883-bafa-e89b1e7749ca
msc.legacyurl: /web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 531a66b7ab642bd012663517640f2766f1917f25
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65112821"
---
# <a name="model-validation-in-aspnet-web-api"></a><span data-ttu-id="aa5a3-103">Weryfikacja modelu we wzorcu ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="aa5a3-103">Model Validation in ASP.NET Web API</span></span>

<span data-ttu-id="aa5a3-104">przez [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="aa5a3-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="aa5a3-105">W tym artykule pokazano, jak dodawać adnotacje do modeli, korzystanie z adnotacji do sprawdzania poprawności danych i obsługi błędów sprawdzania poprawności w interfejsie API sieci web.</span><span class="sxs-lookup"><span data-stu-id="aa5a3-105">This article shows how to annotate your models, use the annotations for data validation, and handle validation errors in your web API.</span></span> <span data-ttu-id="aa5a3-106">Gdy klient wysyła dane do interfejsu API sieci web, często chcesz sprawdzić poprawność danych przed wykonaniem jakiegokolwiek przetwarzania.</span><span class="sxs-lookup"><span data-stu-id="aa5a3-106">When a client sends data to your web API, often you want to validate the data before doing any processing.</span></span> 

## <a name="data-annotations"></a><span data-ttu-id="aa5a3-107">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="aa5a3-107">Data Annotations</span></span>

<span data-ttu-id="aa5a3-108">ASP.NET Web API, można użyć atrybuty z [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) przestrzeni nazw, aby ustawić reguły sprawdzania poprawności dla właściwości modelu.</span><span class="sxs-lookup"><span data-stu-id="aa5a3-108">In ASP.NET Web API, you can use attributes from the [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) namespace to set validation rules for properties on your model.</span></span> <span data-ttu-id="aa5a3-109">Należy wziąć pod uwagę następujący wzór:</span><span class="sxs-lookup"><span data-stu-id="aa5a3-109">Consider the following model:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="aa5a3-110">Sprawdzanie poprawności modelu użycie we wzorcu ASP.NET MVC to powinna wyglądać znajomo.</span><span class="sxs-lookup"><span data-stu-id="aa5a3-110">If you have used model validation in ASP.NET MVC, this should look familiar.</span></span> <span data-ttu-id="aa5a3-111">**Wymagane** atrybutu jest wyświetlany komunikat, który `Name` właściwość nie może mieć wartości null.</span><span class="sxs-lookup"><span data-stu-id="aa5a3-111">The **Required** attribute says that the `Name` property must not be null.</span></span> <span data-ttu-id="aa5a3-112">**Zakres** atrybutu jest wyświetlany komunikat, który `Weight` musi należeć do zakresu od 0 do 999.</span><span class="sxs-lookup"><span data-stu-id="aa5a3-112">The **Range** attribute says that `Weight` must be between zero and 999.</span></span>

<span data-ttu-id="aa5a3-113">Załóżmy, że klient wysyła żądanie POST z następującą reprezentację JSON:</span><span class="sxs-lookup"><span data-stu-id="aa5a3-113">Suppose that a client sends a POST request with the following JSON representation:</span></span>

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample2.json)]

<span data-ttu-id="aa5a3-114">Widać, że klient nie zawiera `Name` właściwość, która jest oznaczona jako wymagane.</span><span class="sxs-lookup"><span data-stu-id="aa5a3-114">You can see that the client did not include the `Name` property, which is marked as required.</span></span> <span data-ttu-id="aa5a3-115">Kiedy internetowy interfejs API konwertuje dane JSON do `Product` wystąpienia, weryfikuje `Product` względem atrybutów sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="aa5a3-115">When Web API converts the JSON into a `Product` instance, it validates the `Product` against the validation attributes.</span></span> <span data-ttu-id="aa5a3-116">W akcji kontrolera można sprawdzić, czy model jest prawidłowy:</span><span class="sxs-lookup"><span data-stu-id="aa5a3-116">In your controller action, you can check whether the model is valid:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="aa5a3-117">Sprawdzanie poprawności modelu nie gwarantuje, że dane klienta są bezpieczne.</span><span class="sxs-lookup"><span data-stu-id="aa5a3-117">Model validation does not guarantee that client data is safe.</span></span> <span data-ttu-id="aa5a3-118">Mogą być wymagane dodatkowe sprawdzenie poprawności w innych warstwach aplikacji.</span><span class="sxs-lookup"><span data-stu-id="aa5a3-118">Additional validation might be needed in other layers of the application.</span></span> <span data-ttu-id="aa5a3-119">(Na przykład warstwa danych może wymuszać ograniczenia klucza obcego). Samouczek [przy użyciu interfejsu API sieci Web za pomocą platformy Entity Framework](../data/using-web-api-with-entity-framework/part-1.md) analizuje niektóre z tych problemów.</span><span class="sxs-lookup"><span data-stu-id="aa5a3-119">(For example, the data layer might enforce foreign key constraints.) The tutorial [Using Web API with Entity Framework](../data/using-web-api-with-entity-framework/part-1.md) explores some of these issues.</span></span>

<span data-ttu-id="aa5a3-120">**"Niepełną ogłaszając"**: Publikowanie niepełną występuje, gdy klient powoduje, że niektóre właściwości.</span><span class="sxs-lookup"><span data-stu-id="aa5a3-120">**"Under-Posting"**: Under-posting happens when the client leaves out some properties.</span></span> <span data-ttu-id="aa5a3-121">Na przykład załóżmy, że klient wysyła następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="aa5a3-121">For example, suppose the client sends the following:</span></span>

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample4.json)]

<span data-ttu-id="aa5a3-122">W tym miejscu klienta nie określono wartości dla `Price` lub `Weight`.</span><span class="sxs-lookup"><span data-stu-id="aa5a3-122">Here, the client did not specify values for `Price` or `Weight`.</span></span> <span data-ttu-id="aa5a3-123">Element formatujący JSON przypisuje wartość domyślną równą zero, aby brakuje właściwości.</span><span class="sxs-lookup"><span data-stu-id="aa5a3-123">The JSON formatter assigns a default value of zero to the missing properties.</span></span>

![](model-validation-in-aspnet-web-api/_static/image1.png)

<span data-ttu-id="aa5a3-124">Stan modelu jest prawidłowa, ponieważ zero jest prawidłową wartością dla tych właściwości.</span><span class="sxs-lookup"><span data-stu-id="aa5a3-124">The model state is valid, because zero is a valid value for these properties.</span></span> <span data-ttu-id="aa5a3-125">Czy jest to problem, zależy od danego scenariusza.</span><span class="sxs-lookup"><span data-stu-id="aa5a3-125">Whether this is a problem depends on your scenario.</span></span> <span data-ttu-id="aa5a3-126">Na przykład w operacji aktualizacji, możesz chcieć rozróżnienia między "zero" i "nenastaveno."</span><span class="sxs-lookup"><span data-stu-id="aa5a3-126">For example, in an update operation, you might want to distinguish between "zero" and "not set."</span></span> <span data-ttu-id="aa5a3-127">Wymuś na klientach, aby ustawić wartość, ustawić właściwość jako dopuszczającego wartość null i ustaw **wymagane** atrybutu:</span><span class="sxs-lookup"><span data-stu-id="aa5a3-127">To force clients to set a value, make the property nullable and set the **Required** attribute:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample5.cs?highlight=1-2)]

<span data-ttu-id="aa5a3-128">**"Polegającymi"**: Klient może także wysłać *więcej* danych niż oczekiwano.</span><span class="sxs-lookup"><span data-stu-id="aa5a3-128">**"Over-Posting"**: A client can also send *more* data than you expected.</span></span> <span data-ttu-id="aa5a3-129">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="aa5a3-129">For example:</span></span>

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample6.json)]

<span data-ttu-id="aa5a3-130">W tym miejscu za pomocą pliku JSON zawiera właściwość ("Color"), która nie istnieje w `Product` modelu.</span><span class="sxs-lookup"><span data-stu-id="aa5a3-130">Here, the JSON includes a property ("Color") that does not exist in the `Product` model.</span></span> <span data-ttu-id="aa5a3-131">W takim przypadku program formatujący JSON po prostu ignoruje tę wartość.</span><span class="sxs-lookup"><span data-stu-id="aa5a3-131">In this case, the JSON formatter simply ignores this value.</span></span> <span data-ttu-id="aa5a3-132">(Element formatujący XML działa tak samo). Publikowanie nadmiernego powoduje występowanie problemów, jeśli model ma właściwości, które mają być tylko do odczytu.</span><span class="sxs-lookup"><span data-stu-id="aa5a3-132">(The XML formatter does the same.) Over-posting causes problems if your model has properties that you intended to be read-only.</span></span> <span data-ttu-id="aa5a3-133">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="aa5a3-133">For example:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="aa5a3-134">Nie chcesz, aby użytkownikom aktualizowanie `IsAdmin` właściwości i podnieść samodzielnie administratorom!</span><span class="sxs-lookup"><span data-stu-id="aa5a3-134">You don't want users to update the `IsAdmin` property and elevate themselves to administrators!</span></span> <span data-ttu-id="aa5a3-135">Najbezpieczniejszym strategii jest użycie klasy modelu, który dokładnie pasuje, klient może wysyłać:</span><span class="sxs-lookup"><span data-stu-id="aa5a3-135">The safest strategy is to use a model class that exactly matches what the client is allowed to send:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample8.cs)]

> [!NOTE]
> <span data-ttu-id="aa5a3-136">Wpis w blogu Brad Wilson "[vs sprawdzania poprawności danych wejściowych. Walidacja we wzorcu ASP.NET MVC modelu](http://bradwilson.typepad.com/blog/2010/01/input-validation-vs-model-validation-in-aspnet-mvc.html)"jest dobre omówienie niepełną ogłaszania i nadmiernego ogłaszania.</span><span class="sxs-lookup"><span data-stu-id="aa5a3-136">Brad Wilson's blog post "[Input Validation vs. Model Validation in ASP.NET MVC](http://bradwilson.typepad.com/blog/2010/01/input-validation-vs-model-validation-in-aspnet-mvc.html)" has a good discussion of under-posting and over-posting.</span></span> <span data-ttu-id="aa5a3-137">Mimo że ogłoszenie dotyczące platformy ASP.NET MVC 2 problemy dotyczą nadal interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="aa5a3-137">Although the post is about ASP.NET MVC 2, the issues are still relevant to Web API.</span></span>

## <a name="handling-validation-errors"></a><span data-ttu-id="aa5a3-138">Obsługa błędów sprawdzania poprawności</span><span class="sxs-lookup"><span data-stu-id="aa5a3-138">Handling Validation Errors</span></span>

<span data-ttu-id="aa5a3-139">Interfejs API sieci Web nie automatycznie zwraca błąd do klienta podczas sprawdzania poprawności zakończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="aa5a3-139">Web API does not automatically return an error to the client when validation fails.</span></span> <span data-ttu-id="aa5a3-140">Jest akcji kontrolera, aby sprawdzić stan modelu i odpowiednio reagować.</span><span class="sxs-lookup"><span data-stu-id="aa5a3-140">It is up to the controller action to check the model state and respond appropriately.</span></span>

<span data-ttu-id="aa5a3-141">Można również utworzyć filtr akcji, aby sprawdzić stan modelu przed wywołaniem akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="aa5a3-141">You can also create an action filter to check the model state before the controller action is invoked.</span></span> <span data-ttu-id="aa5a3-142">Poniżej znajduje się kod przykładowy:</span><span class="sxs-lookup"><span data-stu-id="aa5a3-142">The following code shows an example:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample9.cs)]

<span data-ttu-id="aa5a3-143">Jeśli sprawdzanie poprawności modelu nie powiedzie się, filtr zwróci odpowiedź HTTP, który zawiera błędy weryfikacji.</span><span class="sxs-lookup"><span data-stu-id="aa5a3-143">If model validation fails, this filter returns an HTTP response that contains the validation errors.</span></span> <span data-ttu-id="aa5a3-144">W takim przypadku nie jest wywoływany akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="aa5a3-144">In that case, the controller action is not invoked.</span></span>

[!code-console[Main](model-validation-in-aspnet-web-api/samples/sample10.cmd)]

<span data-ttu-id="aa5a3-145">Aby zastosować filtr do wszystkich kontrolerów składnika Web API, należy dodać wystąpienia filtru w celu **HttpConfiguration.Filters** kolekcji podczas konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="aa5a3-145">To apply this filter to all Web API controllers, add an instance of the filter to the **HttpConfiguration.Filters** collection during configuration:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample11.cs)]

<span data-ttu-id="aa5a3-146">Innym rozwiązaniem jest, aby ustawić filtr jako atrybut na poszczególnych kontrolerach lub akcji kontrolera:</span><span class="sxs-lookup"><span data-stu-id="aa5a3-146">Another option is to set the filter as an attribute on individual controllers or controller actions:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample12.cs)]

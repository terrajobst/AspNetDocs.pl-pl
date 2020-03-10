---
uid: web-api/overview/error-handling/exception-handling
title: Obsługa wyjątków w interfejsie Web API ASP.NET — ASP.NET 4. x
author: MikeWasson
description: ''
ms.author: riande
ms.date: 03/12/2012
ms.custom: seoapril2019
ms.assetid: cbebeb37-2594-41f2-b71a-f4f26520d512
msc.legacyurl: /web-api/overview/error-handling/exception-handling
msc.type: authoredcontent
ms.openlocfilehash: dbdbab6aefec840e2fec9e9cd33f3d124093750e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622319"
---
# <a name="exception-handling-in-aspnet-web-api"></a><span data-ttu-id="791d2-102">Obsługa wyjątków w interfejsie API sieci Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="791d2-102">Exception Handling in ASP.NET Web API</span></span>

<span data-ttu-id="791d2-103">według [Jan Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="791d2-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="791d2-104">W tym artykule opisano obsługę błędów i wyjątków w interfejsie API sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="791d2-104">This article describes error and exception handling in ASP.NET Web API.</span></span>

- [<span data-ttu-id="791d2-105">HttpResponseException</span><span class="sxs-lookup"><span data-stu-id="791d2-105">HttpResponseException</span></span>](#httpresponserexception)
- [<span data-ttu-id="791d2-106">Filtry wyjątków</span><span class="sxs-lookup"><span data-stu-id="791d2-106">Exception Filters</span></span>](#exception_filters)
- [<span data-ttu-id="791d2-107">Rejestrowanie filtrów wyjątków</span><span class="sxs-lookup"><span data-stu-id="791d2-107">Registering Exception Filters</span></span>](#registering_exception_filters)
- [<span data-ttu-id="791d2-108">HttpError</span><span class="sxs-lookup"><span data-stu-id="791d2-108">HttpError</span></span>](#httperror)

<a id="httpresponserexception"></a>
## <a name="httpresponseexception"></a><span data-ttu-id="791d2-109">HttpResponseException</span><span class="sxs-lookup"><span data-stu-id="791d2-109">HttpResponseException</span></span>

<span data-ttu-id="791d2-110">Co się stanie, Jeśli kontroler internetowego interfejsu API zgłasza nieprzechwycony wyjątek?</span><span class="sxs-lookup"><span data-stu-id="791d2-110">What happens if a Web API controller throws an uncaught exception?</span></span> <span data-ttu-id="791d2-111">Domyślnie większość wyjątków jest tłumaczona na odpowiedź HTTP z kodem stanu 500, wewnętrzny błąd serwera.</span><span class="sxs-lookup"><span data-stu-id="791d2-111">By default, most exceptions are translated into an HTTP response with status code 500, Internal Server Error.</span></span>

<span data-ttu-id="791d2-112">Typ **HttpResponseException** jest szczególnym przypadkiem.</span><span class="sxs-lookup"><span data-stu-id="791d2-112">The **HttpResponseException** type is a special case.</span></span> <span data-ttu-id="791d2-113">Ten wyjątek zwraca kod stanu HTTP określony w konstruktorze wyjątków.</span><span class="sxs-lookup"><span data-stu-id="791d2-113">This exception returns any HTTP status code that you specify in the exception constructor.</span></span> <span data-ttu-id="791d2-114">Na przykład następująca metoda zwraca 404, nie znaleziono, jeśli parametr *ID* jest nieprawidłowy.</span><span class="sxs-lookup"><span data-stu-id="791d2-114">For example, the following method returns 404, Not Found, if the *id* parameter is not valid.</span></span>

[!code-csharp[Main](exception-handling/samples/sample1.cs)]

<span data-ttu-id="791d2-115">Aby uzyskać większą kontrolę nad odpowiedzią, można również utworzyć cały komunikat odpowiedzi i dołączyć go do **HttpResponseException:**</span><span class="sxs-lookup"><span data-stu-id="791d2-115">For more control over the response, you can also construct the entire response message and include it with the **HttpResponseException:**</span></span> 

[!code-csharp[Main](exception-handling/samples/sample2.cs)]

<a id="exception_filters"></a>
## <a name="exception-filters"></a><span data-ttu-id="791d2-116">Filtry wyjątków</span><span class="sxs-lookup"><span data-stu-id="791d2-116">Exception Filters</span></span>

<span data-ttu-id="791d2-117">Możesz dostosować sposób, w jaki interfejs API sieci Web obsługuje wyjątki, pisząc *Filtr wyjątków*.</span><span class="sxs-lookup"><span data-stu-id="791d2-117">You can customize how Web API handles exceptions by writing an *exception filter*.</span></span> <span data-ttu-id="791d2-118">Filtr wyjątku jest wykonywany, gdy metoda kontrolera zgłasza dowolny nieobsługiwany wyjątek, który *nie* jest wyjątkiem **HttpResponseException** .</span><span class="sxs-lookup"><span data-stu-id="791d2-118">An exception filter is executed when a controller method throws any unhandled exception that is *not* an **HttpResponseException** exception.</span></span> <span data-ttu-id="791d2-119">Typ **HttpResponseException** jest szczególnym przypadkiem, ponieważ został zaprojektowany specjalnie w celu ZWRÓCENIA odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="791d2-119">The **HttpResponseException** type is a special case, because it is designed specifically for returning an HTTP response.</span></span>

<span data-ttu-id="791d2-120">Filtry wyjątków implementują interfejs **System. Web. http. filters. IExceptionFilter** .</span><span class="sxs-lookup"><span data-stu-id="791d2-120">Exception filters implement the **System.Web.Http.Filters.IExceptionFilter** interface.</span></span> <span data-ttu-id="791d2-121">Najprostszym sposobem pisania filtru wyjątków jest uzyskanie z klasy **System. Web. http. filters. ExceptionFilterAttribute** i zastąpienie metody **onException** .</span><span class="sxs-lookup"><span data-stu-id="791d2-121">The simplest way to write an exception filter is to derive from the **System.Web.Http.Filters.ExceptionFilterAttribute** class and override the **OnException** method.</span></span>

> [!NOTE]
> <span data-ttu-id="791d2-122">Filtry wyjątków w interfejsie Web API ASP.NET są podobne do tych w ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="791d2-122">Exception filters in ASP.NET Web API are similar to those in ASP.NET MVC.</span></span> <span data-ttu-id="791d2-123">Jednak są one deklarowane w odrębnej przestrzeni nazw i funkcji oddzielnie.</span><span class="sxs-lookup"><span data-stu-id="791d2-123">However, they are declared in a separate namespace and function separately.</span></span> <span data-ttu-id="791d2-124">W szczególności Klasa **HandleErrorAttribute** używana w MVC nie obsługuje wyjątków zgłoszonych przez kontrolery interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="791d2-124">In particular, the **HandleErrorAttribute** class used in MVC does not handle exceptions thrown by Web API controllers.</span></span>

<span data-ttu-id="791d2-125">Poniżej znajduje się filtr, który konwertuje **NotImplementedException** wyjątków na kod stanu HTTP 501, nie zaimplementowane:</span><span class="sxs-lookup"><span data-stu-id="791d2-125">Here is a filter that converts **NotImplementedException** exceptions into HTTP status code 501, Not Implemented:</span></span>

[!code-csharp[Main](exception-handling/samples/sample3.cs)]

<span data-ttu-id="791d2-126">Właściwość **Response** obiektu **HttpActionExecutedContext** zawiera komunikat odpowiedzi HTTP, który zostanie wysłany do klienta.</span><span class="sxs-lookup"><span data-stu-id="791d2-126">The **Response** property of the **HttpActionExecutedContext** object contains the HTTP response message that will be sent to the client.</span></span>

<a id="registering_exception_filters"></a>
## <a name="registering-exception-filters"></a><span data-ttu-id="791d2-127">Rejestrowanie filtrów wyjątków</span><span class="sxs-lookup"><span data-stu-id="791d2-127">Registering Exception Filters</span></span>

<span data-ttu-id="791d2-128">Istnieje kilka sposobów rejestrowania filtru wyjątków interfejsu API sieci Web:</span><span class="sxs-lookup"><span data-stu-id="791d2-128">There are several ways to register a Web API exception filter:</span></span>

- <span data-ttu-id="791d2-129">Według akcji</span><span class="sxs-lookup"><span data-stu-id="791d2-129">By action</span></span>
- <span data-ttu-id="791d2-130">Według kontrolera</span><span class="sxs-lookup"><span data-stu-id="791d2-130">By controller</span></span>
- <span data-ttu-id="791d2-131">globalnie</span><span class="sxs-lookup"><span data-stu-id="791d2-131">Globally</span></span>

<span data-ttu-id="791d2-132">Aby zastosować filtr do określonej akcji, Dodaj filtr jako atrybut do akcji:</span><span class="sxs-lookup"><span data-stu-id="791d2-132">To apply the filter to a specific action, add the filter as an attribute to the action:</span></span>

[!code-csharp[Main](exception-handling/samples/sample4.cs)]

<span data-ttu-id="791d2-133">Aby zastosować filtr do wszystkich akcji na kontrolerze, Dodaj filtr jako atrybut do klasy kontrolera:</span><span class="sxs-lookup"><span data-stu-id="791d2-133">To apply the filter to all of the actions on a controller, add the filter as an attribute to the controller class:</span></span>

[!code-csharp[Main](exception-handling/samples/sample5.cs)]

<span data-ttu-id="791d2-134">Aby zastosować filtr globalnie do wszystkich kontrolerów interfejsu API sieci Web, Dodaj wystąpienie filtru do kolekcji **GlobalConfiguration. Configuration. filters** .</span><span class="sxs-lookup"><span data-stu-id="791d2-134">To apply the filter globally to all Web API controllers, add an instance of the filter to the **GlobalConfiguration.Configuration.Filters** collection.</span></span> <span data-ttu-id="791d2-135">Filtry wyjątków w tej kolekcji mają zastosowanie do dowolnej akcji kontrolera interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="791d2-135">Exception filters in this collection apply to any Web API controller action.</span></span>

[!code-csharp[Main](exception-handling/samples/sample6.cs)]

<span data-ttu-id="791d2-136">Jeśli tworzysz projekt przy użyciu szablonu projektu "aplikacja sieci Web ASP.NET MVC 4", umieść kod konfiguracji interfejsu API sieci Web wewnątrz klasy `WebApiConfig`, która znajduje się w aplikacji\_folderze początkowym:</span><span class="sxs-lookup"><span data-stu-id="791d2-136">If you use the "ASP.NET MVC 4 Web Application" project template to create your project, put your Web API configuration code inside the `WebApiConfig` class, which is located in the App\_Start folder:</span></span>

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

<a id="httperror"></a>
## <a name="httperror"></a><span data-ttu-id="791d2-137">HttpError</span><span class="sxs-lookup"><span data-stu-id="791d2-137">HttpError</span></span>

<span data-ttu-id="791d2-138">Obiekt **HttpError** zapewnia spójny sposób zwrócenia informacji o błędach w treści odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="791d2-138">The **HttpError** object provides a consistent way to return error information in the response body.</span></span> <span data-ttu-id="791d2-139">Poniższy przykład pokazuje, jak zwrócić kod stanu HTTP 404 (nie znaleziono) z **HttpError** w treści odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="791d2-139">The following example shows how to return HTTP status code 404 (Not Found) with an **HttpError** in the response body.</span></span>

[!code-csharp[Main](exception-handling/samples/sample8.cs)]

<span data-ttu-id="791d2-140">**CreateErrorResponse** to metoda rozszerzająca zdefiniowana w klasie **System .NET. http. HttpRequestMessageExtensions** .</span><span class="sxs-lookup"><span data-stu-id="791d2-140">**CreateErrorResponse** is an extension method defined in the **System.Net.Http.HttpRequestMessageExtensions** class.</span></span> <span data-ttu-id="791d2-141">Wewnętrznie, **CreateErrorResponse** tworzy wystąpienie **HttpError** , a następnie tworzy **HttpResponseMessage** , który zawiera **HttpError**.</span><span class="sxs-lookup"><span data-stu-id="791d2-141">Internally, **CreateErrorResponse** creates an **HttpError** instance and then creates an **HttpResponseMessage** that contains the **HttpError**.</span></span>

<span data-ttu-id="791d2-142">W tym przykładzie, jeśli metoda zakończy się pomyślnie, zwraca produkt w odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="791d2-142">In this example, if the method is successful, it returns the product in the HTTP response.</span></span> <span data-ttu-id="791d2-143">Ale jeśli żądany produkt nie zostanie znaleziony, odpowiedź HTTP zawiera **HttpError** w treści żądania.</span><span class="sxs-lookup"><span data-stu-id="791d2-143">But if the requested product is not found, the HTTP response contains an **HttpError** in the request body.</span></span> <span data-ttu-id="791d2-144">Odpowiedź może wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="791d2-144">The response might look like the following:</span></span>

[!code-console[Main](exception-handling/samples/sample9.cmd)]

<span data-ttu-id="791d2-145">Zwróć uwagę, że w tym przykładzie **HttpError** został Zserializowany do formatu JSON.</span><span class="sxs-lookup"><span data-stu-id="791d2-145">Notice that the **HttpError** was serialized to JSON in this example.</span></span> <span data-ttu-id="791d2-146">Jedną z zalet korzystania z **HttpError** jest to, że przechodzą przez ten sam proces [negocjacji zawartości](../formats-and-model-binding/content-negotiation.md) i serializacji co inny model o jednoznacznie określonym typie.</span><span class="sxs-lookup"><span data-stu-id="791d2-146">One advantage of using **HttpError** is that it goes through the same [content-negotiation](../formats-and-model-binding/content-negotiation.md) and serialization process as any other strongly-typed model.</span></span>

### <a name="httperror-and-model-validation"></a><span data-ttu-id="791d2-147">HttpError i walidacja modelu</span><span class="sxs-lookup"><span data-stu-id="791d2-147">HttpError and Model Validation</span></span>

<span data-ttu-id="791d2-148">Aby sprawdzić poprawność modelu, można przekazać stan modelu do **CreateErrorResponse**, aby uwzględnić błędy walidacji w odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="791d2-148">For model validation, you can pass the model state to **CreateErrorResponse**, to include the validation errors in the response:</span></span>

[!code-csharp[Main](exception-handling/samples/sample10.cs)]

<span data-ttu-id="791d2-149">Ten przykład może zwrócić następujące odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="791d2-149">This example might return the following response:</span></span>

[!code-console[Main](exception-handling/samples/sample11.cmd)]

<span data-ttu-id="791d2-150">Aby uzyskać więcej informacji na temat weryfikacji modelu, zobacz [Walidacja modelu w ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="791d2-150">For more information about model validation, see [Model Validation in ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span></span>

### <a name="using-httperror-with-httpresponseexception"></a><span data-ttu-id="791d2-151">Używanie HttpError z HttpResponseException</span><span class="sxs-lookup"><span data-stu-id="791d2-151">Using HttpError with HttpResponseException</span></span>

<span data-ttu-id="791d2-152">Poprzednie przykłady zwracają komunikat **HttpResponseMessage** z akcji kontrolera, ale można również użyć **HttpResponseException** do zwrócenia **HttpError**.</span><span class="sxs-lookup"><span data-stu-id="791d2-152">The previous examples return an **HttpResponseMessage** message from the controller action, but you can also use **HttpResponseException** to return an **HttpError**.</span></span> <span data-ttu-id="791d2-153">Dzięki temu można zwrócić silnie wpisaną model w normalnym przypadku sukcesu, ale nadal zwraca **HttpError** , jeśli wystąpi błąd:</span><span class="sxs-lookup"><span data-stu-id="791d2-153">This lets you return a strongly-typed model in the normal success case, while still returning **HttpError** if there is an error:</span></span>

[!code-csharp[Main](exception-handling/samples/sample12.cs)]

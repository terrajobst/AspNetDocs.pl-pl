---
title: Pomocnicy tagów w formularzach w programie ASP.NET Core
author: rick-anderson
description: W tym artykule opisano wbudowane pomocników tagów używane w formularzach.
ms.author: riande
ms.custom: mvc
ms.date: 1/11/2019
uid: mvc/views/working-with-forms
ms.openlocfilehash: cd15c641fbf702071bd57510a1d51737f6ab8e19
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57068537"
---
# <a name="tag-helpers-in-forms-in-aspnet-core"></a><span data-ttu-id="d95c9-103">Pomocnicy tagów w formularzach w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d95c9-103">Tag Helpers in forms in ASP.NET Core</span></span>

<span data-ttu-id="d95c9-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT), [Dave Paquette](https://twitter.com/Dave_Paquette), i [Jerrie Pelser](https://github.com/jerriep)</span><span class="sxs-lookup"><span data-stu-id="d95c9-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Dave Paquette](https://twitter.com/Dave_Paquette), and [Jerrie Pelser](https://github.com/jerriep)</span></span>

<span data-ttu-id="d95c9-105">W tym dokumencie przedstawiono pracy z formularzami i często używane w formularzu elementów HTML.</span><span class="sxs-lookup"><span data-stu-id="d95c9-105">This document demonstrates working with Forms and the HTML elements commonly used on a Form.</span></span> <span data-ttu-id="d95c9-106">Kod HTML [formularza](https://www.w3.org/TR/html401/interact/forms.html) element udostępnia użycia aplikacji sieci web podstawowego mechanizmu publikować dane do serwera.</span><span class="sxs-lookup"><span data-stu-id="d95c9-106">The HTML [Form](https://www.w3.org/TR/html401/interact/forms.html) element provides the primary mechanism web apps use to post back data to the server.</span></span> <span data-ttu-id="d95c9-107">Większość w tym dokumencie opisano [pomocników tagów](tag-helpers/intro.md) i jak mogą one pomóc produktywną tworzyć niezawodne formularzy HTML.</span><span class="sxs-lookup"><span data-stu-id="d95c9-107">Most of this document describes [Tag Helpers](tag-helpers/intro.md) and how they can help you productively create robust HTML forms.</span></span> <span data-ttu-id="d95c9-108">Zalecamy przeczytanie [wprowadzenie do pomocników tagów](tag-helpers/intro.md) przed przeczytaniem tego dokumentu.</span><span class="sxs-lookup"><span data-stu-id="d95c9-108">We recommend you read [Introduction to Tag Helpers](tag-helpers/intro.md) before you read this document.</span></span>

<span data-ttu-id="d95c9-109">W wielu przypadkach pomocników HTML zapewnić alternatywne podejście do określonych Pomocnik tagu, ale ważne jest, aby rozpoznać, czy pomocników tagów nie zastąpić pomocników HTML i nie jest pomocnika tagów dla każdego pomocnika kodu HTML.</span><span class="sxs-lookup"><span data-stu-id="d95c9-109">In many cases, HTML Helpers provide an alternative approach to a specific Tag Helper, but it's important to recognize that Tag Helpers don't replace HTML Helpers and there's not a Tag Helper for each HTML Helper.</span></span> <span data-ttu-id="d95c9-110">Jeśli istnieje alternatywna pomocnika kodu HTML, jest wymienione.</span><span class="sxs-lookup"><span data-stu-id="d95c9-110">When an HTML Helper alternative exists, it's mentioned.</span></span>

<a name="my-asp-route-param-ref-label"></a>

## <a name="the-form-tag-helper"></a><span data-ttu-id="d95c9-111">Pomocnik tagu formularza</span><span class="sxs-lookup"><span data-stu-id="d95c9-111">The Form Tag Helper</span></span>

<span data-ttu-id="d95c9-112">[Formularza](https://www.w3.org/TR/html401/interact/forms.html) pomocnika tagów:</span><span class="sxs-lookup"><span data-stu-id="d95c9-112">The [Form](https://www.w3.org/TR/html401/interact/forms.html) Tag Helper:</span></span>

* <span data-ttu-id="d95c9-113">Generuje kod HTML [ \<formularza >](https://www.w3.org/TR/html401/interact/forms.html) `action` wartość atrybutu dla akcji kontrolera MVC lub nazwanej trasy</span><span class="sxs-lookup"><span data-stu-id="d95c9-113">Generates the HTML [\<FORM>](https://www.w3.org/TR/html401/interact/forms.html) `action` attribute value for a MVC controller action or named route</span></span>

* <span data-ttu-id="d95c9-114">Generuje ukryty [tokenu weryfikacji żądania](/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) celu zapobiegania fałszowaniu żądania między witrynami (gdy jest używane z `[ValidateAntiForgeryToken]` atrybutu w metodzie akcji Post protokołu HTTP)</span><span class="sxs-lookup"><span data-stu-id="d95c9-114">Generates a hidden [Request Verification Token](/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) to prevent cross-site request forgery (when used with the `[ValidateAntiForgeryToken]` attribute in the HTTP Post action method)</span></span>

* <span data-ttu-id="d95c9-115">Udostępnia `asp-route-<Parameter Name>` atrybutu, gdzie `<Parameter Name>` jest dodawany do wartości trasy.</span><span class="sxs-lookup"><span data-stu-id="d95c9-115">Provides the `asp-route-<Parameter Name>` attribute, where `<Parameter Name>` is added to the route values.</span></span> <span data-ttu-id="d95c9-116">`routeValues` Parametry `Html.BeginForm` i `Html.BeginRouteForm` zapewniają podobne funkcje.</span><span class="sxs-lookup"><span data-stu-id="d95c9-116">The  `routeValues` parameters to `Html.BeginForm` and `Html.BeginRouteForm` provide similar functionality.</span></span>

* <span data-ttu-id="d95c9-117">Ma alternatywa pomocnika kodu HTML `Html.BeginForm` i `Html.BeginRouteForm`</span><span class="sxs-lookup"><span data-stu-id="d95c9-117">Has an HTML Helper alternative `Html.BeginForm` and `Html.BeginRouteForm`</span></span>

<span data-ttu-id="d95c9-118">Przykład:</span><span class="sxs-lookup"><span data-stu-id="d95c9-118">Sample:</span></span>

[!code-HTML[](working-with-forms/sample/final/Views/Demo/RegisterFormOnly.cshtml)]

<span data-ttu-id="d95c9-119">Pomocnik tagu formularza powyżej generuje poniższy kod HTML:</span><span class="sxs-lookup"><span data-stu-id="d95c9-119">The Form Tag Helper above generates the following HTML:</span></span>

```HTML
<form method="post" action="/Demo/Register">
    <!-- Input and Submit elements -->
    <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

<span data-ttu-id="d95c9-120">Środowisko wykonawcze MVC zgłasza `action` wartość atrybutu z atrybutów Pomocnik tagu formularza `asp-controller` i `asp-action`.</span><span class="sxs-lookup"><span data-stu-id="d95c9-120">The MVC runtime generates the `action` attribute value from the Form Tag Helper attributes `asp-controller` and `asp-action`.</span></span> <span data-ttu-id="d95c9-121">Pomocnik tagu formularza również generuje ukryty [tokenu weryfikacji żądania](/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) celu zapobiegania fałszowaniu żądania między witrynami (gdy jest używane z `[ValidateAntiForgeryToken]` atrybutu w metodzie akcji Post protokołu HTTP).</span><span class="sxs-lookup"><span data-stu-id="d95c9-121">The Form Tag Helper also generates a hidden [Request Verification Token](/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) to prevent cross-site request forgery (when used with the `[ValidateAntiForgeryToken]` attribute in the HTTP Post action method).</span></span> <span data-ttu-id="d95c9-122">Ochrona przed fałszerstwem żądań między witrynami czysty formularz HTML jest trudne, Pomocnik tagu formularza udostępnia tę usługę dla Ciebie.</span><span class="sxs-lookup"><span data-stu-id="d95c9-122">Protecting a pure HTML Form from cross-site request forgery is difficult, the Form Tag Helper provides this service for you.</span></span>

### <a name="using-a-named-route"></a><span data-ttu-id="d95c9-123">Korzystanie z nazwanej trasy</span><span class="sxs-lookup"><span data-stu-id="d95c9-123">Using a named route</span></span>

<span data-ttu-id="d95c9-124">`asp-route` Atrybut pomocnika tagów można również wygenerować kod znaczników dla kodu HTML `action` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="d95c9-124">The `asp-route` Tag Helper attribute can also generate markup for the HTML `action` attribute.</span></span> <span data-ttu-id="d95c9-125">Aplikację przy użyciu [trasy](../../fundamentals/routing.md) o nazwie `register` można użyć następujących znaczników dla strony rejestracji:</span><span class="sxs-lookup"><span data-stu-id="d95c9-125">An app with a [route](../../fundamentals/routing.md)  named `register` could use the following markup for the registration page:</span></span>

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterRoute.cshtml)]

<span data-ttu-id="d95c9-126">Wiele widoków w *widoków/konto* folder (generowane podczas tworzenia nowej aplikacji sieci web za pomocą *indywidualne konta użytkowników*) zawierają [asp trasy returnurl](xref:mvc/views/working-with-forms) atrybutu:</span><span class="sxs-lookup"><span data-stu-id="d95c9-126">Many of the views in the *Views/Account* folder (generated when you create a new web app with *Individual User Accounts*) contain the [asp-route-returnurl](xref:mvc/views/working-with-forms) attribute:</span></span>

```cshtml
<form asp-controller="Account" asp-action="Login"
     asp-route-returnurl="@ViewData["ReturnUrl"]"
     method="post" class="form-horizontal" role="form">
```

>[!NOTE]
><span data-ttu-id="d95c9-127">Za pomocą wbudowanych szablonów `returnUrl` tylko jest wypełniane automatycznie podczas spróbuj uzyskać dostęp do autoryzowanych zasobów, ale nie uwierzytelniony i autoryzowany.</span><span class="sxs-lookup"><span data-stu-id="d95c9-127">With the built in templates, `returnUrl` is only populated automatically when you try to access an authorized resource but are not authenticated or authorized.</span></span> <span data-ttu-id="d95c9-128">Podczas próby nieautoryzowanego dostępu do oprogramowania pośredniczącego zabezpieczeń przekieruje Cię do strony logowania za pomocą `returnUrl` zestawu.</span><span class="sxs-lookup"><span data-stu-id="d95c9-128">When you attempt an unauthorized access, the security middleware redirects you to the login page with the `returnUrl` set.</span></span>

## <a name="the-input-tag-helper"></a><span data-ttu-id="d95c9-129">Pomocnik tagu wejściowego</span><span class="sxs-lookup"><span data-stu-id="d95c9-129">The Input Tag Helper</span></span>

<span data-ttu-id="d95c9-130">Wejściowy Pomocnik tagu wiąże HTML [ \<wejściowych >](https://www.w3.org/wiki/HTML/Elements/input) element wyrażenia modelu, w tym widoku razor.</span><span class="sxs-lookup"><span data-stu-id="d95c9-130">The Input Tag Helper binds an HTML [\<input>](https://www.w3.org/wiki/HTML/Elements/input) element to a model expression in your razor view.</span></span>

<span data-ttu-id="d95c9-131">Składnia:</span><span class="sxs-lookup"><span data-stu-id="d95c9-131">Syntax:</span></span>

```HTML
<input asp-for="<Expression Name>" />
```

<span data-ttu-id="d95c9-132">Pomocnik tagu wejściowego:</span><span class="sxs-lookup"><span data-stu-id="d95c9-132">The Input Tag Helper:</span></span>

* <span data-ttu-id="d95c9-133">Generuje `id` i `name` atrybuty kodu HTML nazwa wyrażenia określone w `asp-for` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="d95c9-133">Generates the `id` and `name` HTML attributes for the expression name specified in the `asp-for` attribute.</span></span> <span data-ttu-id="d95c9-134">`asp-for="Property1.Property2"` jest odpowiednikiem `m => m.Property1.Property2`.</span><span class="sxs-lookup"><span data-stu-id="d95c9-134">`asp-for="Property1.Property2"` is equivalent to `m => m.Property1.Property2`.</span></span> <span data-ttu-id="d95c9-135">Nazwa wyrażenia jest, do czego służy `asp-for` wartość atrybutu.</span><span class="sxs-lookup"><span data-stu-id="d95c9-135">The name of the expression is what is used for the `asp-for` attribute value.</span></span> <span data-ttu-id="d95c9-136">Zobacz [nazwy wyrażeń](#expression-names) sekcji, aby uzyskać dodatkowe informacje.</span><span class="sxs-lookup"><span data-stu-id="d95c9-136">See the [Expression names](#expression-names) section for additional information.</span></span>

* <span data-ttu-id="d95c9-137">Ustawia kod HTML `type` atrybutu wartości na podstawie typu modelu i [adnotacji danych](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) atrybuty stosowane do właściwości modelu</span><span class="sxs-lookup"><span data-stu-id="d95c9-137">Sets the HTML `type` attribute value based on the model type and  [data annotation](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) attributes applied to the model property</span></span>

* <span data-ttu-id="d95c9-138">Nie zastępować HTML `type` wartość atrybutu, gdy został określony</span><span class="sxs-lookup"><span data-stu-id="d95c9-138">Won't overwrite the HTML `type` attribute value when one is specified</span></span>

* <span data-ttu-id="d95c9-139">Generuje [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) sprawdzania poprawności atrybutów z [adnotacji danych](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) atrybuty stosowane do właściwości modelu</span><span class="sxs-lookup"><span data-stu-id="d95c9-139">Generates [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5)  validation attributes from [data annotation](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) attributes applied to model properties</span></span>

* <span data-ttu-id="d95c9-140">Ma funkcję pomocnika kodu HTML, nakładają się na `Html.TextBoxFor` i `Html.EditorFor`.</span><span class="sxs-lookup"><span data-stu-id="d95c9-140">Has an HTML Helper feature overlap with `Html.TextBoxFor` and `Html.EditorFor`.</span></span> <span data-ttu-id="d95c9-141">Zobacz **alternatywy pomocnika kodu HTML dla danych wejściowych Pomocnik tagu** sekcji, aby uzyskać szczegółowe informacje.</span><span class="sxs-lookup"><span data-stu-id="d95c9-141">See the **HTML Helper alternatives to Input Tag Helper** section for details.</span></span>

* <span data-ttu-id="d95c9-142">Zapewnia silne wpisywanie.</span><span class="sxs-lookup"><span data-stu-id="d95c9-142">Provides strong typing.</span></span> <span data-ttu-id="d95c9-143">Jeśli nazwa zmiany właściwości, a nie Aktualizuj Pomocnik tagu otrzymasz błąd podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="d95c9-143">If the name of the property changes and you don't update the Tag Helper you'll get an error similar to the following:</span></span>

```HTML
An error occurred during the compilation of a resource required to process
this request. Please review the following specific error details and modify
your source code appropriately.

Type expected
 'RegisterViewModel' does not contain a definition for 'Email' and no
 extension method 'Email' accepting a first argument of type 'RegisterViewModel'
 could be found (are you missing a using directive or an assembly reference?)
```

<span data-ttu-id="d95c9-144">`Input` Pomocnik tagu ustawia HTML `type` atrybut oparty na typie .NET.</span><span class="sxs-lookup"><span data-stu-id="d95c9-144">The `Input` Tag Helper sets the HTML `type` attribute based on the .NET type.</span></span> <span data-ttu-id="d95c9-145">W poniższej tabeli wymieniono niektóre typowe typy .NET i wygenerowany typ HTML (wymienione nie każdy typ .NET).</span><span class="sxs-lookup"><span data-stu-id="d95c9-145">The following table lists some common .NET types and generated HTML type (not every .NET type is listed).</span></span>

|<span data-ttu-id="d95c9-146">Typ architektury .NET</span><span class="sxs-lookup"><span data-stu-id="d95c9-146">.NET type</span></span>|<span data-ttu-id="d95c9-147">Typ danych wejściowych</span><span class="sxs-lookup"><span data-stu-id="d95c9-147">Input Type</span></span>|
|---|---|
|<span data-ttu-id="d95c9-148">Bool</span><span class="sxs-lookup"><span data-stu-id="d95c9-148">Bool</span></span>|<span data-ttu-id="d95c9-149">type="checkbox"</span><span class="sxs-lookup"><span data-stu-id="d95c9-149">type="checkbox"</span></span>|
|<span data-ttu-id="d95c9-150">String</span><span class="sxs-lookup"><span data-stu-id="d95c9-150">String</span></span>|<span data-ttu-id="d95c9-151">type="text"</span><span class="sxs-lookup"><span data-stu-id="d95c9-151">type="text"</span></span>|
|<span data-ttu-id="d95c9-152">DataGodzina</span><span class="sxs-lookup"><span data-stu-id="d95c9-152">DateTime</span></span>|<span data-ttu-id="d95c9-153">type=["datetime-local"](https://developer.mozilla.org/docs/Web/HTML/Element/input/datetime-local)</span><span class="sxs-lookup"><span data-stu-id="d95c9-153">type=["datetime-local"](https://developer.mozilla.org/docs/Web/HTML/Element/input/datetime-local)</span></span>|
|<span data-ttu-id="d95c9-154">Byte</span><span class="sxs-lookup"><span data-stu-id="d95c9-154">Byte</span></span>|<span data-ttu-id="d95c9-155">type="number"</span><span class="sxs-lookup"><span data-stu-id="d95c9-155">type="number"</span></span>|
|<span data-ttu-id="d95c9-156">int</span><span class="sxs-lookup"><span data-stu-id="d95c9-156">Int</span></span>|<span data-ttu-id="d95c9-157">type="number"</span><span class="sxs-lookup"><span data-stu-id="d95c9-157">type="number"</span></span>|
|<span data-ttu-id="d95c9-158">Pojedynczy Double</span><span class="sxs-lookup"><span data-stu-id="d95c9-158">Single, Double</span></span>|<span data-ttu-id="d95c9-159">type="number"</span><span class="sxs-lookup"><span data-stu-id="d95c9-159">type="number"</span></span>|


<span data-ttu-id="d95c9-160">W poniższej tabeli przedstawiono niektóre typowe [adnotacje danych](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) atrybutów, które Pomocnik tagu wejściowego będzie zmapowana do określonych typów wejściowych (nie każdy atrybut weryfikacji znajduje się):</span><span class="sxs-lookup"><span data-stu-id="d95c9-160">The following table shows some common [data annotations](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) attributes that the input tag helper will map to specific input types (not every validation attribute is listed):</span></span>


|<span data-ttu-id="d95c9-161">Atrybut</span><span class="sxs-lookup"><span data-stu-id="d95c9-161">Attribute</span></span>|<span data-ttu-id="d95c9-162">Typ danych wejściowych</span><span class="sxs-lookup"><span data-stu-id="d95c9-162">Input Type</span></span>|
|---|---|
|<span data-ttu-id="d95c9-163">[EmailAddress]</span><span class="sxs-lookup"><span data-stu-id="d95c9-163">[EmailAddress]</span></span>|<span data-ttu-id="d95c9-164">type="email"</span><span class="sxs-lookup"><span data-stu-id="d95c9-164">type="email"</span></span>|
|<span data-ttu-id="d95c9-165">[Url]</span><span class="sxs-lookup"><span data-stu-id="d95c9-165">[Url]</span></span>|<span data-ttu-id="d95c9-166">type="url"</span><span class="sxs-lookup"><span data-stu-id="d95c9-166">type="url"</span></span>|
|<span data-ttu-id="d95c9-167">[HiddenInput]</span><span class="sxs-lookup"><span data-stu-id="d95c9-167">[HiddenInput]</span></span>|<span data-ttu-id="d95c9-168">type="hidden"</span><span class="sxs-lookup"><span data-stu-id="d95c9-168">type="hidden"</span></span>|
|<span data-ttu-id="d95c9-169">[Phone]</span><span class="sxs-lookup"><span data-stu-id="d95c9-169">[Phone]</span></span>|<span data-ttu-id="d95c9-170">type="tel"</span><span class="sxs-lookup"><span data-stu-id="d95c9-170">type="tel"</span></span>|
|<span data-ttu-id="d95c9-171">[DataType(DataType.Password)]</span><span class="sxs-lookup"><span data-stu-id="d95c9-171">[DataType(DataType.Password)]</span></span>| <span data-ttu-id="d95c9-172">type="password"</span><span class="sxs-lookup"><span data-stu-id="d95c9-172">type="password"</span></span>|
|<span data-ttu-id="d95c9-173">[DataType(DataType.Date)]</span><span class="sxs-lookup"><span data-stu-id="d95c9-173">[DataType(DataType.Date)]</span></span>| <span data-ttu-id="d95c9-174">type="date"</span><span class="sxs-lookup"><span data-stu-id="d95c9-174">type="date"</span></span>|
|<span data-ttu-id="d95c9-175">[DataType(DataType.Time)]</span><span class="sxs-lookup"><span data-stu-id="d95c9-175">[DataType(DataType.Time)]</span></span>| <span data-ttu-id="d95c9-176">type="time"</span><span class="sxs-lookup"><span data-stu-id="d95c9-176">type="time"</span></span>|


<span data-ttu-id="d95c9-177">Przykład:</span><span class="sxs-lookup"><span data-stu-id="d95c9-177">Sample:</span></span>

[!code-csharp[](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]

[!code-HTML[](working-with-forms/sample/final/Views/Demo/RegisterInput.cshtml)]

<span data-ttu-id="d95c9-178">Powyższy kod generuje poniższy kod HTML:</span><span class="sxs-lookup"><span data-stu-id="d95c9-178">The code above generates the following HTML:</span></span>

```HTML
  <form method="post" action="/Demo/RegisterInput">
       Email:
       <input type="email" data-val="true"
              data-val-email="The Email Address field is not a valid email address."
              data-val-required="The Email Address field is required."
              id="Email" name="Email" value="" /> <br>
       Password:
       <input type="password" data-val="true"
              data-val-required="The Password field is required."
              id="Password" name="Password" /><br>
       <button type="submit">Register</button>
     <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
   </form>
```

<span data-ttu-id="d95c9-179">Adnotacje danych dotyczą `Email` i `Password` właściwości Generowanie metadanych na podstawie modelu.</span><span class="sxs-lookup"><span data-stu-id="d95c9-179">The data annotations applied to the `Email` and `Password` properties generate metadata on the model.</span></span> <span data-ttu-id="d95c9-180">Pomocnik tagu dane wejściowe zużywa metadanych modelu i tworzy [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-val-*` atrybutów (zobacz [sprawdzania poprawności modelu](../models/validation.md)).</span><span class="sxs-lookup"><span data-stu-id="d95c9-180">The Input Tag Helper consumes the model metadata and produces [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-val-*` attributes (see [Model Validation](../models/validation.md)).</span></span> <span data-ttu-id="d95c9-181">Te atrybuty opisać moduły weryfikacji, aby dołączyć do pól wejściowych.</span><span class="sxs-lookup"><span data-stu-id="d95c9-181">These attributes describe the validators to attach to the input fields.</span></span> <span data-ttu-id="d95c9-182">Dzięki temu dyskretnego kodu HTML5 i [jQuery](https://jquery.com/) sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="d95c9-182">This provides unobtrusive HTML5 and [jQuery](https://jquery.com/) validation.</span></span> <span data-ttu-id="d95c9-183">Atrybuty dyskretnego kodu mają format `data-val-rule="Error Message"`, gdzie reguła jest nazwa reguły sprawdzania poprawności (takie jak `data-val-required`, `data-val-email`, `data-val-maxlength`itp.) Jeśli komunikat o błędzie zostanie podany w atrybucie, jest on wyświetlany jako wartość pozycji `data-val-rule` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="d95c9-183">The unobtrusive attributes have the format `data-val-rule="Error Message"`, where rule is the name of the validation rule (such as `data-val-required`, `data-val-email`, `data-val-maxlength`, etc.) If an error message is provided in the attribute, it's displayed as the value for the `data-val-rule` attribute.</span></span> <span data-ttu-id="d95c9-184">Dostępne są także atrybutów w formie `data-val-ruleName-argumentName="argumentValue"` zapewniające dodatkowe szczegóły dotyczące reguły, na przykład `data-val-maxlength-max="1024"` .</span><span class="sxs-lookup"><span data-stu-id="d95c9-184">There are also attributes of the form `data-val-ruleName-argumentName="argumentValue"` that provide additional details about the rule, for example, `data-val-maxlength-max="1024"` .</span></span>

### <a name="html-helper-alternatives-to-input-tag-helper"></a><span data-ttu-id="d95c9-185">Pomocnik tagu dane wejściowe alternatywy dla pomocnika kodu HTML</span><span class="sxs-lookup"><span data-stu-id="d95c9-185">HTML Helper alternatives to Input Tag Helper</span></span>

<span data-ttu-id="d95c9-186">`Html.TextBox`, `Html.TextBoxFor`, `Html.Editor` i `Html.EditorFor` nakładających się funkcji pomocnika Tag danych wejściowych.</span><span class="sxs-lookup"><span data-stu-id="d95c9-186">`Html.TextBox`, `Html.TextBoxFor`, `Html.Editor` and `Html.EditorFor` have overlapping features with the Input Tag Helper.</span></span> <span data-ttu-id="d95c9-187">Pomocnik tagu dane wejściowe automatycznie ustawi `type` atrybutu; `Html.TextBox` i `Html.TextBoxFor` nie będzie.</span><span class="sxs-lookup"><span data-stu-id="d95c9-187">The Input Tag Helper will automatically set the `type` attribute; `Html.TextBox` and `Html.TextBoxFor` won't.</span></span> <span data-ttu-id="d95c9-188">`Html.Editor` i `Html.EditorFor` obsługi kolekcji, złożonych obiektów i szablony nie Pomocnik tagu danych wejściowych.</span><span class="sxs-lookup"><span data-stu-id="d95c9-188">`Html.Editor` and `Html.EditorFor` handle collections, complex objects and templates; the Input Tag Helper doesn't.</span></span> <span data-ttu-id="d95c9-189">Pomocnik tagu danych wejściowych, `Html.EditorFor` i `Html.TextBoxFor` są silnie typizowane (ich użycie wyrażeń lambda); `Html.TextBox` i `Html.Editor` nie (używają nazwy wyrażeń).</span><span class="sxs-lookup"><span data-stu-id="d95c9-189">The Input Tag Helper, `Html.EditorFor`  and  `Html.TextBoxFor` are strongly typed (they use lambda expressions); `Html.TextBox` and `Html.Editor` are not (they use expression names).</span></span>

### <a name="htmlattributes"></a><span data-ttu-id="d95c9-190">HtmlAttributes</span><span class="sxs-lookup"><span data-stu-id="d95c9-190">HtmlAttributes</span></span>

<span data-ttu-id="d95c9-191">`@Html.Editor()` i `@Html.EditorFor()` użyć specjalnej `ViewDataDictionary` wpis o nazwie `htmlAttributes` podczas wykonywania ich domyślnych szablonów.</span><span class="sxs-lookup"><span data-stu-id="d95c9-191">`@Html.Editor()` and `@Html.EditorFor()` use a special `ViewDataDictionary` entry named `htmlAttributes` when executing their default templates.</span></span> <span data-ttu-id="d95c9-192">To zachowanie jest opcjonalnie rozszerzone przy użyciu `additionalViewData` parametrów.</span><span class="sxs-lookup"><span data-stu-id="d95c9-192">This behavior is optionally augmented using `additionalViewData` parameters.</span></span> <span data-ttu-id="d95c9-193">Klucz "htmlAttributes" jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="d95c9-193">The key "htmlAttributes" is case-insensitive.</span></span> <span data-ttu-id="d95c9-194">Klucz "htmlAttributes" odbywa się podobnie jak `htmlAttributes` przekazano obiekt jako danych wejściowych pomocników, takich jak `@Html.TextBox()`.</span><span class="sxs-lookup"><span data-stu-id="d95c9-194">The key "htmlAttributes" is handled similarly to the `htmlAttributes` object passed to input helpers like `@Html.TextBox()`.</span></span>

```HTML
@Html.EditorFor(model => model.YourProperty, 
  new { htmlAttributes = new { @class="myCssClass", style="Width:100px" } })
```

### <a name="expression-names"></a><span data-ttu-id="d95c9-195">Wyrażenie nazwy</span><span class="sxs-lookup"><span data-stu-id="d95c9-195">Expression names</span></span>

<span data-ttu-id="d95c9-196">`asp-for` Wartość atrybutu jest `ModelExpression` i po prawej stronie wyrażenia lambda.</span><span class="sxs-lookup"><span data-stu-id="d95c9-196">The `asp-for` attribute value is a `ModelExpression` and the right hand side of a lambda expression.</span></span> <span data-ttu-id="d95c9-197">W związku z tym `asp-for="Property1"` staje się `m => m.Property1` w wygenerowanym kodzie, co jest Dlaczego nie trzeba prefiks z `Model`.</span><span class="sxs-lookup"><span data-stu-id="d95c9-197">Therefore, `asp-for="Property1"` becomes `m => m.Property1` in the generated code which is why you don't need to prefix with `Model`.</span></span> <span data-ttu-id="d95c9-198">Można użyć "\@" znaku, aby uruchomić wyrażenia wbudowane i przenieść przed `m.`:</span><span class="sxs-lookup"><span data-stu-id="d95c9-198">You can use the "\@" character to start an inline expression and move before the `m.`:</span></span>

```HTML
@{
       var joe = "Joe";
   }
   <input asp-for="@joe" />
```

<span data-ttu-id="d95c9-199">Generuje następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="d95c9-199">Generates the following:</span></span>

```HTML
<input type="text" id="joe" name="joe" value="Joe" />
```

<span data-ttu-id="d95c9-200">Z właściwościami kolekcji `asp-for="CollectionProperty[23].Member"` generuje taką samą nazwę jak `asp-for="CollectionProperty[i].Member"` podczas `i` ma wartość `23`.</span><span class="sxs-lookup"><span data-stu-id="d95c9-200">With collection properties, `asp-for="CollectionProperty[23].Member"` generates the same name as `asp-for="CollectionProperty[i].Member"` when `i` has the value `23`.</span></span>

<span data-ttu-id="d95c9-201">Gdy program ASP.NET Core MVC oblicza wartość `ModelExpression`, sprawdza ono kilka źródeł, w tym `ModelState`.</span><span class="sxs-lookup"><span data-stu-id="d95c9-201">When ASP.NET Core MVC calculates the value of `ModelExpression`, it inspects several sources, including `ModelState`.</span></span> <span data-ttu-id="d95c9-202">Należy wziąć pod uwagę `<input type="text" asp-for="@Name" />`.</span><span class="sxs-lookup"><span data-stu-id="d95c9-202">Consider `<input type="text" asp-for="@Name" />`.</span></span> <span data-ttu-id="d95c9-203">Obliczony `value` atrybut jest pierwsza wartość inną niż null z:</span><span class="sxs-lookup"><span data-stu-id="d95c9-203">The calculated `value` attribute is the first non-null value from:</span></span>

* <span data-ttu-id="d95c9-204">`ModelState` wpis z kluczem "Name".</span><span class="sxs-lookup"><span data-stu-id="d95c9-204">`ModelState` entry with key "Name".</span></span>
* <span data-ttu-id="d95c9-205">Wynik wyrażenia `Model.Name`.</span><span class="sxs-lookup"><span data-stu-id="d95c9-205">Result of the expression `Model.Name`.</span></span>

### <a name="navigating-child-properties"></a><span data-ttu-id="d95c9-206">Nawigacja właściwości podrzędnej</span><span class="sxs-lookup"><span data-stu-id="d95c9-206">Navigating child properties</span></span>

<span data-ttu-id="d95c9-207">Możesz także przejść do właściwości podrzędnej przy użyciu ścieżki właściwości modelu widoku.</span><span class="sxs-lookup"><span data-stu-id="d95c9-207">You can also navigate to child properties using the property path of the view model.</span></span> <span data-ttu-id="d95c9-208">Należy wziąć pod uwagę bardziej złożonych klasy modelu, który zawiera element podrzędny `Address` właściwości.</span><span class="sxs-lookup"><span data-stu-id="d95c9-208">Consider a more complex model class that contains a child `Address` property.</span></span>

[!code-csharp[](../../mvc/views/working-with-forms/sample/final/ViewModels/AddressViewModel.cs?highlight=1,2,3,4&range=5-8)]

[!code-csharp[](../../mvc/views/working-with-forms/sample/final/ViewModels/RegisterAddressViewModel.cs?highlight=8&range=5-13)]

<span data-ttu-id="d95c9-209">W widoku, możemy powiązać `Address.AddressLine1`:</span><span class="sxs-lookup"><span data-stu-id="d95c9-209">In the view, we bind to `Address.AddressLine1`:</span></span>

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterAddress.cshtml?highlight=6)]

<span data-ttu-id="d95c9-210">Poniższy kod HTML jest generowany dla `Address.AddressLine1`:</span><span class="sxs-lookup"><span data-stu-id="d95c9-210">The following HTML is generated for `Address.AddressLine1`:</span></span>

```HTML
<input type="text" id="Address_AddressLine1" name="Address.AddressLine1" value="" />
```

### <a name="expression-names-and-collections"></a><span data-ttu-id="d95c9-211">Wyrażenie nazwy i kolekcji</span><span class="sxs-lookup"><span data-stu-id="d95c9-211">Expression names and Collections</span></span>

<span data-ttu-id="d95c9-212">Przykładowy model zawierający tablicę `Colors`:</span><span class="sxs-lookup"><span data-stu-id="d95c9-212">Sample, a model containing an array of `Colors`:</span></span>

[!code-csharp[](../../mvc/views/working-with-forms/sample/final/ViewModels/Person.cs?highlight=3&range=5-10)]

<span data-ttu-id="d95c9-213">Metody akcji:</span><span class="sxs-lookup"><span data-stu-id="d95c9-213">The action method:</span></span>

```csharp
public IActionResult Edit(int id, int colorIndex)
   {
       ViewData["Index"] = colorIndex;
       return View(GetPerson(id));
   }
```

<span data-ttu-id="d95c9-214">Następujące Razor pokazano, jak uzyskiwać dostęp do określonego `Color` elementu:</span><span class="sxs-lookup"><span data-stu-id="d95c9-214">The following Razor shows how you access a specific `Color` element:</span></span>

[!code-HTML[](working-with-forms/sample/final/Views/Demo/EditColor.cshtml)]

<span data-ttu-id="d95c9-215">*Views/Shared/EditorTemplates/String.cshtml* szablonu:</span><span class="sxs-lookup"><span data-stu-id="d95c9-215">The *Views/Shared/EditorTemplates/String.cshtml* template:</span></span>

[!code-HTML[](working-with-forms/sample/final/Views/Shared/EditorTemplates/String.cshtml)]

<span data-ttu-id="d95c9-216">Przykładowy przy użyciu `List<T>`:</span><span class="sxs-lookup"><span data-stu-id="d95c9-216">Sample using `List<T>`:</span></span>

[!code-csharp[](working-with-forms/sample/final/ViewModels/ToDoItem.cs?range=3-8)]

<span data-ttu-id="d95c9-217">Następujące Razor pokazuje, jak iteracji po kolekcji:</span><span class="sxs-lookup"><span data-stu-id="d95c9-217">The following Razor shows how to iterate over a collection:</span></span>

[!code-HTML[](working-with-forms/sample/final/Views/Demo/Edit.cshtml)]

<span data-ttu-id="d95c9-218">*Views/Shared/EditorTemplates/ToDoItem.cshtml* szablonu:</span><span class="sxs-lookup"><span data-stu-id="d95c9-218">The *Views/Shared/EditorTemplates/ToDoItem.cshtml* template:</span></span>

[!code-HTML[](working-with-forms/sample/final/Views/Shared/EditorTemplates/ToDoItem.cshtml)]

<span data-ttu-id="d95c9-219">`foreach` powinny być używane, jeśli jest to możliwe, gdy wartość ma być używany w `asp-for` lub `Html.DisplayFor` równoważne kontekstu.</span><span class="sxs-lookup"><span data-stu-id="d95c9-219">`foreach` should be used if possible when the value is going to be used in an `asp-for` or `Html.DisplayFor` equivalent context.</span></span> <span data-ttu-id="d95c9-220">Ogólnie rzecz biorąc `for` jest lepsze niż `foreach` (jeśli scenariusz umożliwia), ponieważ nie trzeba ją przydzielić moduł wyliczający; jednak ocena indeksatora w wyrażeniu LINQ może być kosztowne i należy zminimalizować.</span><span class="sxs-lookup"><span data-stu-id="d95c9-220">In general, `for` is better than `foreach` (if the scenario allows it) because it doesn't need to allocate an enumerator; however, evaluating an indexer in a LINQ expression can be expensive and should be minimized.</span></span>

&nbsp;

>[!NOTE]
><span data-ttu-id="d95c9-221">Powyższy kod przykładowy komentarzem pokazuje, jak należy zamienić wyrażenia lambda z `@` operator, aby uzyskać dostęp każdy `ToDoItem` na liście.</span><span class="sxs-lookup"><span data-stu-id="d95c9-221">The commented sample code above shows how you would replace the lambda expression with the `@` operator to access each `ToDoItem` in the list.</span></span>

## <a name="the-textarea-tag-helper"></a><span data-ttu-id="d95c9-222">Pomocnik tagu Textarea</span><span class="sxs-lookup"><span data-stu-id="d95c9-222">The Textarea Tag Helper</span></span>

<span data-ttu-id="d95c9-223">`Textarea Tag Helper` Pomocnik tagu jest podobny do Pomocnik tagu danych wejściowych.</span><span class="sxs-lookup"><span data-stu-id="d95c9-223">The `Textarea Tag Helper` tag helper is  similar to the Input Tag Helper.</span></span>

* <span data-ttu-id="d95c9-224">Generuje `id` i `name` atrybuty i atrybutów sprawdzania poprawności danych z modelu dla [ \<textarea >](https://www.w3.org/wiki/HTML/Elements/textarea) elementu.</span><span class="sxs-lookup"><span data-stu-id="d95c9-224">Generates the `id` and `name` attributes, and the data validation attributes from the model for a [\<textarea>](https://www.w3.org/wiki/HTML/Elements/textarea) element.</span></span>

* <span data-ttu-id="d95c9-225">Zapewnia silne wpisywanie.</span><span class="sxs-lookup"><span data-stu-id="d95c9-225">Provides strong typing.</span></span>

* <span data-ttu-id="d95c9-226">Alternatywa pomocnika kodu HTML: `Html.TextAreaFor`</span><span class="sxs-lookup"><span data-stu-id="d95c9-226">HTML Helper alternative: `Html.TextAreaFor`</span></span>

<span data-ttu-id="d95c9-227">Przykład:</span><span class="sxs-lookup"><span data-stu-id="d95c9-227">Sample:</span></span>

[!code-csharp[](working-with-forms/sample/final/ViewModels/DescriptionViewModel.cs)]

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterTextArea.cshtml?highlight=4)]

<span data-ttu-id="d95c9-228">Poniższy kod HTML jest generowany:</span><span class="sxs-lookup"><span data-stu-id="d95c9-228">The following HTML is generated:</span></span>

```HTML
<form method="post" action="/Demo/RegisterTextArea">
  <textarea data-val="true"
   data-val-maxlength="The field Description must be a string or array type with a maximum length of &#x27;1024&#x27;."
   data-val-maxlength-max="1024"
   data-val-minlength="The field Description must be a string or array type with a minimum length of &#x27;5&#x27;."
   data-val-minlength-min="5"
   id="Description" name="Description">
  </textarea>
  <button type="submit">Test</button>
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

## <a name="the-label-tag-helper"></a><span data-ttu-id="d95c9-229">Pomocnik tagu etykiet</span><span class="sxs-lookup"><span data-stu-id="d95c9-229">The Label Tag Helper</span></span>

* <span data-ttu-id="d95c9-230">Generuje podpis etykiety i `for` atrybutu na [ <label> ](https://www.w3.org/wiki/HTML/Elements/label) element to nazwa wyrażenia</span><span class="sxs-lookup"><span data-stu-id="d95c9-230">Generates the label caption and `for` attribute on a [<label>](https://www.w3.org/wiki/HTML/Elements/label) element for an expression name</span></span>

* <span data-ttu-id="d95c9-231">Alternatywa pomocnika kodu HTML: `Html.LabelFor`.</span><span class="sxs-lookup"><span data-stu-id="d95c9-231">HTML Helper alternative: `Html.LabelFor`.</span></span>

<span data-ttu-id="d95c9-232">`Label Tag Helper` Zapewnia następujące korzyści za pośrednictwem czystego element label kodu HTML:</span><span class="sxs-lookup"><span data-stu-id="d95c9-232">The `Label Tag Helper`  provides the following benefits over a pure HTML label element:</span></span>

* <span data-ttu-id="d95c9-233">Automatycznie pobrana wartość opisową etykietę `Display` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="d95c9-233">You automatically get the descriptive label value from the `Display` attribute.</span></span> <span data-ttu-id="d95c9-234">Nazwę wyświetlaną zamierzony mogą ulec zmianie czasu i kombinacja `Display` zastosuje atrybut i Pomocnik tagu etykiet `Display` wszędzie, gdzie jest używany.</span><span class="sxs-lookup"><span data-stu-id="d95c9-234">The intended display name might change over time, and the combination of `Display` attribute and Label Tag Helper will apply the `Display` everywhere it's used.</span></span>

* <span data-ttu-id="d95c9-235">Mniej znacznika w kodzie źródłowym</span><span class="sxs-lookup"><span data-stu-id="d95c9-235">Less markup in source code</span></span>

* <span data-ttu-id="d95c9-236">Silne wpisywanie z właściwością modelu.</span><span class="sxs-lookup"><span data-stu-id="d95c9-236">Strong typing with the model property.</span></span>

<span data-ttu-id="d95c9-237">Przykład:</span><span class="sxs-lookup"><span data-stu-id="d95c9-237">Sample:</span></span>

[!code-csharp[](working-with-forms/sample/final/ViewModels/SimpleViewModel.cs)]

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterLabel.cshtml?highlight=4)]

<span data-ttu-id="d95c9-238">Poniższy kod HTML jest generowany dla `<label>` elementu:</span><span class="sxs-lookup"><span data-stu-id="d95c9-238">The following HTML is generated for the `<label>` element:</span></span>

```HTML
<label for="Email">Email Address</label>
```

<span data-ttu-id="d95c9-239">Pomocnik tagu etykiet, które są generowane `for` wartość atrybutu "Email", czyli identyfikator skojarzony z `<input>` elementu.</span><span class="sxs-lookup"><span data-stu-id="d95c9-239">The Label Tag Helper generated the `for` attribute value of "Email", which is the ID associated with the `<input>` element.</span></span> <span data-ttu-id="d95c9-240">Pomocnicy tagów Generowanie spójne `id` i `for` elementów, tak aby były prawidłowo skojarzone.</span><span class="sxs-lookup"><span data-stu-id="d95c9-240">The Tag Helpers generate consistent `id` and `for` elements so they can be correctly associated.</span></span> <span data-ttu-id="d95c9-241">Podpis w tym przykładzie pochodzi z `Display` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="d95c9-241">The caption in this sample comes from the `Display` attribute.</span></span> <span data-ttu-id="d95c9-242">Jeśli model nie zawiera `Display` atrybutu, podpis będzie nazwa właściwości wyrażenia.</span><span class="sxs-lookup"><span data-stu-id="d95c9-242">If the model didn't contain a `Display` attribute, the caption would be the property name of the expression.</span></span>

## <a name="the-validation-tag-helpers"></a><span data-ttu-id="d95c9-243">Sprawdzanie poprawności pomocnicy tagów</span><span class="sxs-lookup"><span data-stu-id="d95c9-243">The Validation Tag Helpers</span></span>

<span data-ttu-id="d95c9-244">Istnieją dwa pomocników tagów sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="d95c9-244">There are two Validation Tag Helpers.</span></span> <span data-ttu-id="d95c9-245">`Validation Message Tag Helper` (Która wyświetla komunikat sprawdzania poprawności dla jednej właściwości modelu), a `Validation Summary Tag Helper` (które jest wyświetlane podsumowanie błędów sprawdzania poprawności).</span><span class="sxs-lookup"><span data-stu-id="d95c9-245">The `Validation Message Tag Helper` (which displays a validation message for a single property on your model), and the `Validation Summary Tag Helper` (which displays a summary of validation errors).</span></span> <span data-ttu-id="d95c9-246">`Input Tag Helper` Dodaje atrybuty weryfikacji po stronie klienta HTML5 do wprowadzania elementów na podstawie danych atrybuty adnotacji w klasach modeli.</span><span class="sxs-lookup"><span data-stu-id="d95c9-246">The `Input Tag Helper` adds HTML5 client side validation attributes to input elements based on data annotation attributes on your model classes.</span></span> <span data-ttu-id="d95c9-247">Sprawdzanie poprawności, również odbywa się na serwerze.</span><span class="sxs-lookup"><span data-stu-id="d95c9-247">Validation is also performed on the server.</span></span> <span data-ttu-id="d95c9-248">Pomocnik tagu weryfikacji wyświetla te komunikaty o błędach, gdy wystąpi błąd sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="d95c9-248">The Validation Tag Helper displays these error messages when a validation error occurs.</span></span>

### <a name="the-validation-message-tag-helper"></a><span data-ttu-id="d95c9-249">Pomocnik tagu komunikat sprawdzania poprawności</span><span class="sxs-lookup"><span data-stu-id="d95c9-249">The Validation Message Tag Helper</span></span>

* <span data-ttu-id="d95c9-250">Dodaje [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-valmsg-for="property"` atrybutu [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) element, który dołącza komunikatów o błędach weryfikacji na pole wejściowe właściwości określonego modelu.</span><span class="sxs-lookup"><span data-stu-id="d95c9-250">Adds the [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5)  `data-valmsg-for="property"` attribute to the [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) element, which attaches the validation error messages on the input field of the specified model property.</span></span> <span data-ttu-id="d95c9-251">Gdy wystąpi błąd weryfikacji po stronie klienta, [jQuery](https://jquery.com/) wyświetla komunikat o błędzie w `<span>` elementu.</span><span class="sxs-lookup"><span data-stu-id="d95c9-251">When a client side validation error occurs, [jQuery](https://jquery.com/) displays the error message in the `<span>` element.</span></span>

* <span data-ttu-id="d95c9-252">Sprawdzanie poprawności również odbywa się na serwerze.</span><span class="sxs-lookup"><span data-stu-id="d95c9-252">Validation also takes place on the server.</span></span> <span data-ttu-id="d95c9-253">Klienci mogą mieć Obsługa skryptów JavaScript wyłączona i niektóre sprawdzania poprawności jest możliwe tylko po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="d95c9-253">Clients may have JavaScript disabled and some validation can only be done on the server side.</span></span>

* <span data-ttu-id="d95c9-254">Alternatywa pomocnika kodu HTML: `Html.ValidationMessageFor`</span><span class="sxs-lookup"><span data-stu-id="d95c9-254">HTML Helper alternative: `Html.ValidationMessageFor`</span></span>

<span data-ttu-id="d95c9-255">`Validation Message Tag Helper` Jest używana z `asp-validation-for` atrybutu HTML [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) elementu.</span><span class="sxs-lookup"><span data-stu-id="d95c9-255">The `Validation Message Tag Helper`  is used with the `asp-validation-for` attribute on a HTML [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) element.</span></span>

```HTML
<span asp-validation-for="Email"></span>
```

<span data-ttu-id="d95c9-256">Pomocnik tagu komunikat sprawdzania poprawności generuje poniższy kod HTML:</span><span class="sxs-lookup"><span data-stu-id="d95c9-256">The Validation Message Tag Helper will generate the following HTML:</span></span>

```HTML
<span class="field-validation-valid"
  data-valmsg-for="Email"
  data-valmsg-replace="true"></span>
```

<span data-ttu-id="d95c9-257">Na ogół służy `Validation Message Tag Helper` po `Input` Pomocnik tagu dla tej samej właściwości.</span><span class="sxs-lookup"><span data-stu-id="d95c9-257">You generally use the `Validation Message Tag Helper`  after an `Input` Tag Helper for the same property.</span></span> <span data-ttu-id="d95c9-258">Ten sposób wyświetla komunikaty o błędach weryfikacji w danych wejściowych, które spowodowały błąd.</span><span class="sxs-lookup"><span data-stu-id="d95c9-258">Doing so displays any validation error messages near the input that caused the error.</span></span>

> [!NOTE]
> <span data-ttu-id="d95c9-259">Konieczne jest posiadanie widoku z poprawną JavaScript i [jQuery](https://jquery.com/) skryptu odwołania w miejscu na potrzeby weryfikacji po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="d95c9-259">You must have a view with the correct JavaScript and [jQuery](https://jquery.com/) script references in place for client side validation.</span></span> <span data-ttu-id="d95c9-260">Zobacz [sprawdzania poprawności modelu](../models/validation.md) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="d95c9-260">See [Model Validation](../models/validation.md) for more information.</span></span>

<span data-ttu-id="d95c9-261">Gdy pojawia się błąd weryfikacji po stronie serwera, (na przykład gdy masz weryfikacji po stronie niestandardowego serwera lub Weryfikacja po stronie klienta jest wyłączona), MVC umieszcza komunikat o błędzie jako treść metody `<span>` elementu.</span><span class="sxs-lookup"><span data-stu-id="d95c9-261">When a server side validation error occurs (for example when you have custom server side validation or client-side validation is disabled), MVC places that error message as the body of the `<span>` element.</span></span>

```HTML
<span class="field-validation-error" data-valmsg-for="Email"
            data-valmsg-replace="true">
   The Email Address field is required.
</span>
```

### <a name="the-validation-summary-tag-helper"></a><span data-ttu-id="d95c9-262">Pomocnik tagu podsumowania sprawdzania poprawności</span><span class="sxs-lookup"><span data-stu-id="d95c9-262">The Validation Summary Tag Helper</span></span>

* <span data-ttu-id="d95c9-263">Obiekty docelowe `<div>` elementów za pomocą `asp-validation-summary` atrybutu</span><span class="sxs-lookup"><span data-stu-id="d95c9-263">Targets `<div>` elements with the `asp-validation-summary` attribute</span></span>

* <span data-ttu-id="d95c9-264">Alternatywa pomocnika kodu HTML: `@Html.ValidationSummary`</span><span class="sxs-lookup"><span data-stu-id="d95c9-264">HTML Helper alternative: `@Html.ValidationSummary`</span></span>

<span data-ttu-id="d95c9-265">`Validation Summary Tag Helper` Służy do wyświetlania podsumowania komunikatów dotyczących sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="d95c9-265">The `Validation Summary Tag Helper`  is used to display a summary of validation messages.</span></span> <span data-ttu-id="d95c9-266">`asp-validation-summary` Wartość atrybutu może być dowolną z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="d95c9-266">The `asp-validation-summary` attribute value can be any of the following:</span></span>

|<span data-ttu-id="d95c9-267">asp-validation-summary</span><span class="sxs-lookup"><span data-stu-id="d95c9-267">asp-validation-summary</span></span>|<span data-ttu-id="d95c9-268">Wyświetlane komunikatów dotyczących sprawdzania poprawności</span><span class="sxs-lookup"><span data-stu-id="d95c9-268">Validation messages displayed</span></span>|
|--- |--- |
|<span data-ttu-id="d95c9-269">ValidationSummary.All</span><span class="sxs-lookup"><span data-stu-id="d95c9-269">ValidationSummary.All</span></span>|<span data-ttu-id="d95c9-270">Poziom właściwości i model</span><span class="sxs-lookup"><span data-stu-id="d95c9-270">Property and model level</span></span>|
|<span data-ttu-id="d95c9-271">ValidationSummary.ModelOnly</span><span class="sxs-lookup"><span data-stu-id="d95c9-271">ValidationSummary.ModelOnly</span></span>|<span data-ttu-id="d95c9-272">Model</span><span class="sxs-lookup"><span data-stu-id="d95c9-272">Model</span></span>|
|<span data-ttu-id="d95c9-273">ValidationSummary.None</span><span class="sxs-lookup"><span data-stu-id="d95c9-273">ValidationSummary.None</span></span>|<span data-ttu-id="d95c9-274">Brak</span><span class="sxs-lookup"><span data-stu-id="d95c9-274">None</span></span>|

### <a name="sample"></a><span data-ttu-id="d95c9-275">Przykład</span><span class="sxs-lookup"><span data-stu-id="d95c9-275">Sample</span></span>

<span data-ttu-id="d95c9-276">W poniższym przykładzie zostanie nadany modelu danych `DataAnnotation` atrybuty, które generuje komunikaty o błędach weryfikacji na `<input>` elementu.</span><span class="sxs-lookup"><span data-stu-id="d95c9-276">In the following example, the data model is decorated with `DataAnnotation` attributes, which generates validation error messages on the `<input>` element.</span></span>  <span data-ttu-id="d95c9-277">Gdy wystąpi błąd sprawdzania poprawności, Pomocnik tagu weryfikacji wyświetla komunikat o błędzie:</span><span class="sxs-lookup"><span data-stu-id="d95c9-277">When a validation error occurs, the Validation Tag Helper displays the error message:</span></span>

[!code-csharp[](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterValidation.cshtml?highlight=4,6,8&range=1-10)]

<span data-ttu-id="d95c9-278">Wygenerowany kod HTML, (gdy model jest prawidłowy):</span><span class="sxs-lookup"><span data-stu-id="d95c9-278">The generated HTML (when the model is valid):</span></span>

```HTML
<form action="/DemoReg/Register" method="post">
  <div class="validation-summary-valid" data-valmsg-summary="true">
  <ul><li style="display:none"></li></ul></div>
  Email:  <input name="Email" id="Email" type="email" value=""
   data-val-required="The Email field is required."
   data-val-email="The Email field is not a valid email address."
   data-val="true"> <br>
  <span class="field-validation-valid" data-valmsg-replace="true"
   data-valmsg-for="Email"></span><br>
  Password: <input name="Password" id="Password" type="password"
   data-val-required="The Password field is required." data-val="true"><br>
  <span class="field-validation-valid" data-valmsg-replace="true"
   data-valmsg-for="Password"></span><br>
  <button type="submit">Register</button>
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

## <a name="the-select-tag-helper"></a><span data-ttu-id="d95c9-279">Pomocnik tagu Select</span><span class="sxs-lookup"><span data-stu-id="d95c9-279">The Select Tag Helper</span></span>

* <span data-ttu-id="d95c9-280">Generuje [wybierz](https://www.w3.org/wiki/HTML/Elements/select) i skojarzone [opcji](https://www.w3.org/wiki/HTML/Elements/option) elementy dla właściwości modelu.</span><span class="sxs-lookup"><span data-stu-id="d95c9-280">Generates [select](https://www.w3.org/wiki/HTML/Elements/select) and associated [option](https://www.w3.org/wiki/HTML/Elements/option) elements for properties of your model.</span></span>

* <span data-ttu-id="d95c9-281">Ma alternatywa pomocnika kodu HTML `Html.DropDownListFor` i `Html.ListBoxFor`</span><span class="sxs-lookup"><span data-stu-id="d95c9-281">Has an HTML Helper alternative `Html.DropDownListFor` and `Html.ListBoxFor`</span></span>

<span data-ttu-id="d95c9-282">`Select Tag Helper` `asp-for` Określa nazwę właściwości modelu [wybierz](https://www.w3.org/wiki/HTML/Elements/select) elementu i `asp-items` Określa [opcji](https://www.w3.org/wiki/HTML/Elements/option) elementów.</span><span class="sxs-lookup"><span data-stu-id="d95c9-282">The `Select Tag Helper` `asp-for` specifies the model property  name for the [select](https://www.w3.org/wiki/HTML/Elements/select) element  and `asp-items` specifies the [option](https://www.w3.org/wiki/HTML/Elements/option) elements.</span></span>  <span data-ttu-id="d95c9-283">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="d95c9-283">For example:</span></span>

[!code-HTML[](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]

<span data-ttu-id="d95c9-284">Przykład:</span><span class="sxs-lookup"><span data-stu-id="d95c9-284">Sample:</span></span>

[!code-csharp[](working-with-forms/sample/final/ViewModels/CountryViewModel.cs)]

<span data-ttu-id="d95c9-285">`Index` Inicjuje metodę `CountryViewModel`, ustawia wybranym kraju i przekazuje go do `Index` widoku.</span><span class="sxs-lookup"><span data-stu-id="d95c9-285">The `Index` method initializes the `CountryViewModel`, sets the selected country and passes it to the `Index` view.</span></span>

[!code-csharp[](working-with-forms/sample/final/Controllers/HomeController.cs?range=8-13)]

<span data-ttu-id="d95c9-286">HTTP POST `Index` metoda Wyświetla zaznaczenie:</span><span class="sxs-lookup"><span data-stu-id="d95c9-286">The HTTP POST `Index` method displays the selection:</span></span>

[!code-csharp[](working-with-forms/sample/final/Controllers/HomeController.cs?range=15-27)]

<span data-ttu-id="d95c9-287">`Index` Widoku:</span><span class="sxs-lookup"><span data-stu-id="d95c9-287">The `Index` view:</span></span>

[!code-cshtml[](working-with-forms/sample/final/Views/Home/Index.cshtml?highlight=4)]

<span data-ttu-id="d95c9-288">Która generuje poniższy kod HTML (się od liter "CA" wybrane):</span><span class="sxs-lookup"><span data-stu-id="d95c9-288">Which generates the following HTML (with "CA" selected):</span></span>

```html
<form method="post" action="/">
     <select id="Country" name="Country">
       <option value="MX">Mexico</option>
       <option selected="selected" value="CA">Canada</option>
       <option value="US">USA</option>
     </select>
       <br /><button type="submit">Register</button>
     <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
   </form>
```

> [!NOTE]
> <span data-ttu-id="d95c9-289">Nie zalecamy używania `ViewBag` lub `ViewData` przy użyciu Pomocnika tagów wybierz.</span><span class="sxs-lookup"><span data-stu-id="d95c9-289">We don't recommend using `ViewBag` or `ViewData` with the Select Tag Helper.</span></span> <span data-ttu-id="d95c9-290">Model widoku jest bardziej niezawodna w zapewnieniu metadanych platformy MVC i zazwyczaj mniej problematyczne.</span><span class="sxs-lookup"><span data-stu-id="d95c9-290">A view model is more robust at providing MVC metadata and generally less problematic.</span></span>

<span data-ttu-id="d95c9-291">`asp-for` Wartość atrybutu jest przypadkiem szczególnym i nie wymaga `Model` prefiksu, inne są atrybuty Pomocnik tagu (takie jak `asp-items`)</span><span class="sxs-lookup"><span data-stu-id="d95c9-291">The `asp-for` attribute value is a special case and doesn't require a `Model` prefix, the other Tag Helper attributes do (such as `asp-items`)</span></span>

[!code-HTML[](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]

### <a name="enum-binding"></a><span data-ttu-id="d95c9-292">Powiązanie typu wyliczeniowego</span><span class="sxs-lookup"><span data-stu-id="d95c9-292">Enum binding</span></span>

<span data-ttu-id="d95c9-293">Często jest to łatwe w użyciu `<select>` z `enum` właściwości i wygenerować `SelectListItem` elementy z `enum` wartości.</span><span class="sxs-lookup"><span data-stu-id="d95c9-293">It's often convenient to use `<select>` with an `enum` property and generate the `SelectListItem` elements from the `enum` values.</span></span>

<span data-ttu-id="d95c9-294">Przykład:</span><span class="sxs-lookup"><span data-stu-id="d95c9-294">Sample:</span></span>

[!code-csharp[](working-with-forms/sample/final/ViewModels/CountryEnumViewModel.cs?range=3-7)]

[!code-csharp[](working-with-forms/sample/final/ViewModels/CountryEnum.cs)]

<span data-ttu-id="d95c9-295">`GetEnumSelectList` Metoda generuje `SelectList` obiektu dla typu wyliczeniowego.</span><span class="sxs-lookup"><span data-stu-id="d95c9-295">The `GetEnumSelectList` method generates a `SelectList` object for an enum.</span></span>

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEnum.cshtml?highlight=5)]

<span data-ttu-id="d95c9-296">Można dekoracji moduł wyliczający listę o `Display` atrybutu, aby uzyskać bardziej rozbudowane interfejs użytkownika:</span><span class="sxs-lookup"><span data-stu-id="d95c9-296">You can decorate your enumerator list with the `Display` attribute to get a richer UI:</span></span>

[!code-csharp[](working-with-forms/sample/final/ViewModels/CountryEnum.cs?highlight=5,7)]

<span data-ttu-id="d95c9-297">Poniższy kod HTML jest generowany:</span><span class="sxs-lookup"><span data-stu-id="d95c9-297">The following HTML is generated:</span></span>

```HTML
  <form method="post" action="/Home/IndexEnum">
         <select data-val="true" data-val-required="The EnumCountry field is required."
                 id="EnumCountry" name="EnumCountry">
             <option value="0">United Mexican States</option>
             <option value="1">United States of America</option>
             <option value="2">Canada</option>
             <option value="3">France</option>
             <option value="4">Germany</option>
             <option selected="selected" value="5">Spain</option>
         </select>
         <br /><button type="submit">Register</button>
         <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
    </form>
```

### <a name="option-group"></a><span data-ttu-id="d95c9-298">Opcja grupy</span><span class="sxs-lookup"><span data-stu-id="d95c9-298">Option Group</span></span>

<span data-ttu-id="d95c9-299">Kod HTML [ \<optgroup >](https://www.w3.org/wiki/HTML/Elements/optgroup) element jest generowany, gdy model widoku zawiera jeden lub więcej `SelectListGroup` obiektów.</span><span class="sxs-lookup"><span data-stu-id="d95c9-299">The HTML  [\<optgroup>](https://www.w3.org/wiki/HTML/Elements/optgroup) element is generated when the view model contains one or more `SelectListGroup` objects.</span></span>

<span data-ttu-id="d95c9-300">`CountryViewModelGroup` Grup `SelectListItem` elementów do grupy "Ameryka Północna" i "Europa":</span><span class="sxs-lookup"><span data-stu-id="d95c9-300">The `CountryViewModelGroup` groups the `SelectListItem` elements into the "North America" and "Europe" groups:</span></span>

[!code-csharp[](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelGroup.cs?highlight=5,6,14,20,26,32,38,44&range=6-56)]

<span data-ttu-id="d95c9-301">Poniżej przedstawiono dwie grupy:</span><span class="sxs-lookup"><span data-stu-id="d95c9-301">The two groups are shown below:</span></span>

![przykład grupy opcji](working-with-forms/_static/grp.png)

<span data-ttu-id="d95c9-303">Wygenerowany kod HTML:</span><span class="sxs-lookup"><span data-stu-id="d95c9-303">The generated HTML:</span></span>

```HTML
 <form method="post" action="/Home/IndexGroup">
      <select id="Country" name="Country">
          <optgroup label="North America">
              <option value="MEX">Mexico</option>
              <option value="CAN">Canada</option>
              <option value="US">USA</option>
          </optgroup>
          <optgroup label="Europe">
              <option value="FR">France</option>
              <option value="ES">Spain</option>
              <option value="DE">Germany</option>
          </optgroup>
      </select>
      <br /><button type="submit">Register</button>
      <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
 </form>
```

### <a name="multiple-select"></a><span data-ttu-id="d95c9-304">Wybór wielokrotny</span><span class="sxs-lookup"><span data-stu-id="d95c9-304">Multiple select</span></span>

<span data-ttu-id="d95c9-305">Pomocnik tagu wybierz automatycznie wygeneruje [wielu = "wielu"](http://w3c.github.io/html-reference/select.html) atrybutu, jeśli właściwość określona w `asp-for` atrybut jest `IEnumerable`.</span><span class="sxs-lookup"><span data-stu-id="d95c9-305">The Select Tag Helper  will automatically generate the [multiple = "multiple"](http://w3c.github.io/html-reference/select.html)  attribute if the property specified in the `asp-for` attribute is an `IEnumerable`.</span></span> <span data-ttu-id="d95c9-306">Na przykład biorąc pod uwagę następujący wzór:</span><span class="sxs-lookup"><span data-stu-id="d95c9-306">For example, given the following model:</span></span>

[!code-csharp[](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelIEnumerable.cs?highlight=6)]

<span data-ttu-id="d95c9-307">Za pomocą następującego widoku:</span><span class="sxs-lookup"><span data-stu-id="d95c9-307">With the following view:</span></span>

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexMultiSelect.cshtml?highlight=4)]

<span data-ttu-id="d95c9-308">Generuje poniższy kod HTML:</span><span class="sxs-lookup"><span data-stu-id="d95c9-308">Generates the following HTML:</span></span>

```HTML
<form method="post" action="/Home/IndexMultiSelect">
    <select id="CountryCodes"
    multiple="multiple"
    name="CountryCodes"><option value="MX">Mexico</option>
<option value="CA">Canada</option>
<option value="US">USA</option>
<option value="FR">France</option>
<option value="ES">Spain</option>
<option value="DE">Germany</option>
</select>
    <br /><button type="submit">Register</button>
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

### <a name="no-selection"></a><span data-ttu-id="d95c9-309">Brak zaznaczenia</span><span class="sxs-lookup"><span data-stu-id="d95c9-309">No selection</span></span>

<span data-ttu-id="d95c9-310">Jeśli okaże się, przy użyciu opcji "nie został określony" na wielu stronach, można utworzyć szablon, aby wyeliminować powtarzające się kodu HTML:</span><span class="sxs-lookup"><span data-stu-id="d95c9-310">If you find yourself using the "not specified" option in multiple pages, you can create a template to eliminate repeating the HTML:</span></span>

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEmptyTemplate.cshtml?highlight=5)]

<span data-ttu-id="d95c9-311">*Views/Shared/EditorTemplates/CountryViewModel.cshtml* szablonu:</span><span class="sxs-lookup"><span data-stu-id="d95c9-311">The *Views/Shared/EditorTemplates/CountryViewModel.cshtml* template:</span></span>

[!code-HTML[](working-with-forms/sample/final/Views/Shared/EditorTemplates/CountryViewModel.cshtml)]

<span data-ttu-id="d95c9-312">Dodawanie HTML [ \<opcja >](https://www.w3.org/wiki/HTML/Elements/option) elementów nie jest ograniczona do *Brak zaznaczenia* przypadek.</span><span class="sxs-lookup"><span data-stu-id="d95c9-312">Adding HTML [\<option>](https://www.w3.org/wiki/HTML/Elements/option) elements isn't limited to the *No selection* case.</span></span> <span data-ttu-id="d95c9-313">Na przykład następującej metody akcji i widoku spowoduje wygenerowanie podobny do powyższego kodu HTML:</span><span class="sxs-lookup"><span data-stu-id="d95c9-313">For example, the following view and action method will generate HTML similar to the code above:</span></span>

[!code-csharp[](working-with-forms/sample/final/Controllers/HomeController.cs?range=114-119)]

[!code-HTML[](working-with-forms/sample/final/Views/Home/IndexOption.cshtml)]

<span data-ttu-id="d95c9-314">Poprawny `<option>` element zostanie wybrana (zawierają `selected="selected"` atrybutów) w zależności od bieżącego `Country` wartość.</span><span class="sxs-lookup"><span data-stu-id="d95c9-314">The correct `<option>` element will be selected ( contain the `selected="selected"` attribute) depending on the current `Country` value.</span></span>

```HTML
 <form method="post" action="/Home/IndexEmpty">
      <select id="Country" name="Country">
          <option value="">&lt;none&gt;</option>
          <option value="MX">Mexico</option>
          <option value="CA" selected="selected">Canada</option>
          <option value="US">USA</option>
      </select>
      <br /><button type="submit">Register</button>
   <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
 </form>
 ```

## <a name="additional-resources"></a><span data-ttu-id="d95c9-315">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="d95c9-315">Additional resources</span></span>

* <xref:mvc/views/tag-helpers/intro>
* [<span data-ttu-id="d95c9-316">Element formularza HTML</span><span class="sxs-lookup"><span data-stu-id="d95c9-316">HTML Form element</span></span>](https://www.w3.org/TR/html401/interact/forms.html)
* [<span data-ttu-id="d95c9-317">Żądanie weryfikacji tokenu</span><span class="sxs-lookup"><span data-stu-id="d95c9-317">Request Verification Token</span></span>](/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages)
* <xref:mvc/models/model-binding>
* <xref:mvc/models/validation>
* [<span data-ttu-id="d95c9-318">Interfejs IAttributeAdapter</span><span class="sxs-lookup"><span data-stu-id="d95c9-318">IAttributeAdapter Interface</span></span>](/dotnet/api/Microsoft.AspNetCore.Mvc.DataAnnotations.IAttributeAdapter)
* [<span data-ttu-id="d95c9-319">Fragmenty kodu dla tego dokumentu</span><span class="sxs-lookup"><span data-stu-id="d95c9-319">Code snippets for this document</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/working-with-forms/sample/final)

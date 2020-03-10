---
uid: web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
title: Sprawdzanie poprawności danych wejściowych użytkownika w witrynach ASP.NET Web Pages (Razor) | Microsoft Docs
author: Rick-Anderson
description: W tym artykule omówiono sposób sprawdzania poprawności informacji uzyskanych od użytkowników &mdash; to, aby upewnić się, że użytkownicy wprowadzają prawidłowe informacje w formularzach HTML w formie jako...
ms.author: riande
ms.date: 02/20/2014
ms.assetid: 4eb060cc-cf14-41ae-bab1-14a2c15332d0
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: e6f8e1051d09d11f1756bfada44a73ba7c2a1db2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78563505"
---
# <a name="validating-user-input-in-aspnet-web-pages-razor-sites"></a><span data-ttu-id="7462a-103">Sprawdzanie poprawności danych wejściowych użytkownika w witrynach ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="7462a-103">Validating User Input in ASP.NET Web Pages (Razor) Sites</span></span>

<span data-ttu-id="7462a-104">Autor [FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="7462a-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="7462a-105">W tym artykule omówiono sposób sprawdzania poprawności informacji uzyskanych od użytkowników &mdash; to, aby upewnić się, że użytkownicy wprowadzają prawidłowe informacje w formularzach HTML w witrynie ASP.NET Web Pages (Razor).</span><span class="sxs-lookup"><span data-stu-id="7462a-105">This article discusses how to validate information you get from users &mdash; that is, to make sure that users enter valid information in HTML forms in an ASP.NET Web Pages (Razor) site.</span></span>
> 
> <span data-ttu-id="7462a-106">Zawartość:</span><span class="sxs-lookup"><span data-stu-id="7462a-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="7462a-107">Sprawdzanie, czy dane wejściowe użytkownika odpowiadają zdefiniowanym kryteriom walidacji.</span><span class="sxs-lookup"><span data-stu-id="7462a-107">How to check that a user's input matches validation criteria that you define.</span></span>
> - <span data-ttu-id="7462a-108">Jak ustalić, czy wszystkie testy weryfikacyjne zostały zakończone pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="7462a-108">How to determine whether all validation tests have passed.</span></span>
> - <span data-ttu-id="7462a-109">Jak wyświetlić błędy walidacji (i sposób formatowania).</span><span class="sxs-lookup"><span data-stu-id="7462a-109">How to display validation errors (and how to format them).</span></span>
> - <span data-ttu-id="7462a-110">Sprawdzanie poprawności danych, które nie pochodzą bezpośrednio od użytkowników.</span><span class="sxs-lookup"><span data-stu-id="7462a-110">How to validate data that doesn't come directly from users.</span></span>
> 
> <span data-ttu-id="7462a-111">Są to koncepcje programowania ASP.NET wprowadzone w artykule:</span><span class="sxs-lookup"><span data-stu-id="7462a-111">These are the ASP.NET programming concepts introduced in the article:</span></span>
> 
> - <span data-ttu-id="7462a-112">Pomocnik `Validation`.</span><span class="sxs-lookup"><span data-stu-id="7462a-112">The `Validation` helper.</span></span>
> - <span data-ttu-id="7462a-113">Metody `Html.ValidationSummary` i `Html.ValidationMessage`.</span><span class="sxs-lookup"><span data-stu-id="7462a-113">The `Html.ValidationSummary` and `Html.ValidationMessage` methods.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="7462a-114">Wersje oprogramowania używane w samouczku</span><span class="sxs-lookup"><span data-stu-id="7462a-114">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="7462a-115">ASP.NET strony sieci Web (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="7462a-115">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="7462a-116">Ten samouczek działa również z ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="7462a-116">This tutorial also works with ASP.NET Web Pages 2.</span></span>

<span data-ttu-id="7462a-117">Ten artykuł zawiera następujące sekcje:</span><span class="sxs-lookup"><span data-stu-id="7462a-117">This article contains the following sections:</span></span>

- [<span data-ttu-id="7462a-118">Przegląd weryfikacji danych wejściowych użytkownika</span><span class="sxs-lookup"><span data-stu-id="7462a-118">Overview of User Input Validation</span></span>](#Overview_of_User_Input_Validation)
- [<span data-ttu-id="7462a-119">Sprawdzanie poprawności danych wejściowych użytkownika</span><span class="sxs-lookup"><span data-stu-id="7462a-119">Validating User Input</span></span>](#Validating_User_Input)
- [<span data-ttu-id="7462a-120">Dodawanie weryfikacji po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="7462a-120">Adding Client-Side Validation</span></span>](#Adding_Client-Side_Validation)
- [<span data-ttu-id="7462a-121">Błędy walidacji formatowania</span><span class="sxs-lookup"><span data-stu-id="7462a-121">Formatting Validation Errors</span></span>](#Formatting_Validation_Errors)
- [<span data-ttu-id="7462a-122">Sprawdzanie poprawności danych, które nie pochodzą bezpośrednio od użytkowników</span><span class="sxs-lookup"><span data-stu-id="7462a-122">Validating Data That Doesn't Come Directly from Users</span></span>](#Validating_Data_That_Doesnt_Come_Directly_from_Users)

<a id="Overview_of_User_Input_Validation"></a>
## <a name="overview-of-user-input-validation"></a><span data-ttu-id="7462a-123">Przegląd weryfikacji danych wejściowych użytkownika</span><span class="sxs-lookup"><span data-stu-id="7462a-123">Overview of User Input Validation</span></span>

<span data-ttu-id="7462a-124">Jeśli podasz użytkownikowi monit o wprowadzenie informacji na stronie — na przykład w formularzu — ważne jest, aby upewnić się, że wprowadzane wartości są prawidłowe.</span><span class="sxs-lookup"><span data-stu-id="7462a-124">If you ask users to enter information in a page — for example, into a form — it's important to make sure that the values that they enter are valid.</span></span> <span data-ttu-id="7462a-125">Na przykład nie chcesz przetworzyć formularza, który nie zawiera krytycznych informacji.</span><span class="sxs-lookup"><span data-stu-id="7462a-125">For example, you don't want to process a form that's missing critical information.</span></span>

<span data-ttu-id="7462a-126">Gdy użytkownicy wprowadzają wartości do formularza HTML, wprowadzane przez nie wartości są ciągami.</span><span class="sxs-lookup"><span data-stu-id="7462a-126">When users enter values into an HTML form, the values that they enter are strings.</span></span> <span data-ttu-id="7462a-127">W wielu przypadkach potrzebne wartości to inne typy danych, takie jak liczby całkowite lub daty.</span><span class="sxs-lookup"><span data-stu-id="7462a-127">In many cases, the values you need are some other data types, like integers or dates.</span></span> <span data-ttu-id="7462a-128">W związku z tym należy również upewnić się, że wartości wprowadzane przez użytkowników mogą być prawidłowo konwertowane na odpowiednie typy danych.</span><span class="sxs-lookup"><span data-stu-id="7462a-128">Therefore, you also have to make sure that the values that users enter can be correctly converted to the appropriate data types.</span></span>

<span data-ttu-id="7462a-129">Mogą być również określone ograniczenia dotyczące wartości.</span><span class="sxs-lookup"><span data-stu-id="7462a-129">You might also have certain restrictions on the values.</span></span> <span data-ttu-id="7462a-130">Nawet jeśli użytkownicy poprawnie wprowadzają liczbę całkowitą, na przykład może być konieczne upewnienie się, że wartość znajduje się w określonym zakresie.</span><span class="sxs-lookup"><span data-stu-id="7462a-130">Even if users correctly enter an integer, for example, you might need to make sure that the value falls within a certain range.</span></span>

![Błędy walidacji korzystające z klas stylów CSS](validating-user-input-in-aspnet-web-pages-sites/_static/image1.png)

> [!NOTE] 
> 
> <span data-ttu-id="7462a-132">**Ważne** Sprawdzanie poprawności danych wejściowych użytkownika jest również ważne dla bezpieczeństwa.</span><span class="sxs-lookup"><span data-stu-id="7462a-132">**Important** Validating user input is also important for security.</span></span> <span data-ttu-id="7462a-133">Po ograniczeniu wartości, które użytkownicy mogą wprowadzać w formularzach, zmniejszasz prawdopodobieństwo, że ktoś może wprowadzić wartość, która może naruszyć bezpieczeństwo witryny.</span><span class="sxs-lookup"><span data-stu-id="7462a-133">When you restrict the values that users can enter in forms, you reduce the chance that someone can enter a value that can compromise the security of your site.</span></span>

<a id="Validating_User_Input"></a>
## <a name="validating-user-input"></a><span data-ttu-id="7462a-134">Sprawdzanie poprawności danych wejściowych użytkownika</span><span class="sxs-lookup"><span data-stu-id="7462a-134">Validating User Input</span></span>

<span data-ttu-id="7462a-135">W witrynie ASP.NET Web Pages 2 można używać pomocnika `Validator` do testowania danych wejściowych użytkownika.</span><span class="sxs-lookup"><span data-stu-id="7462a-135">In ASP.NET Web Pages 2, you can use the `Validator` helper to test user input.</span></span> <span data-ttu-id="7462a-136">Podstawowym podejściem jest wykonanie następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="7462a-136">The basic approach is to do the following:</span></span>

1. <span data-ttu-id="7462a-137">Ustal, które elementy wejściowe (pola) chcesz zweryfikować.</span><span class="sxs-lookup"><span data-stu-id="7462a-137">Determine which input elements (fields) you want to validate.</span></span>

    <span data-ttu-id="7462a-138">Zwykle sprawdzane są wartości w `<input>` elementów w formularzu.</span><span class="sxs-lookup"><span data-stu-id="7462a-138">You typically validate values in `<input>` elements in a form.</span></span> <span data-ttu-id="7462a-139">Jednak dobrym sposobem jest zweryfikowanie wszystkich danych wejściowych, nawet danych wejściowych pochodzących z ograniczonego elementu, takich jak lista `<select>`.</span><span class="sxs-lookup"><span data-stu-id="7462a-139">However, it's a good practice to validate all input, even input that comes from a constrained element like a `<select>` list.</span></span> <span data-ttu-id="7462a-140">Pozwala to upewnić się, że użytkownicy nie pomijają formantów na stronie i przesyłają formularz.</span><span class="sxs-lookup"><span data-stu-id="7462a-140">This helps to make sure that users don't bypass the controls on a page and submit a form.</span></span>
2. <span data-ttu-id="7462a-141">W kodzie strony Dodaj indywidualne sprawdzanie poprawności dla każdego elementu wejściowego przy użyciu metod pomocnika `Validation`.</span><span class="sxs-lookup"><span data-stu-id="7462a-141">In the page code, add individual validation checks for each input element by using methods of the `Validation` helper.</span></span>

    <span data-ttu-id="7462a-142">Aby sprawdzić wymagane pola, użyj `Validation.RequireField(field, [error message])` (dla pojedynczego pola) lub `Validation.RequireFields(field1, field2, ...))` (dla listy pól).</span><span class="sxs-lookup"><span data-stu-id="7462a-142">To check for required fields, use `Validation.RequireField(field, [error message])` (for an individual field) or `Validation.RequireFields(field1, field2, ...))` (for a list of fields).</span></span> <span data-ttu-id="7462a-143">W przypadku innych typów walidacji należy użyć `Validation.Add(field, ValidationType)`.</span><span class="sxs-lookup"><span data-stu-id="7462a-143">For other types of validation, use `Validation.Add(field, ValidationType)`.</span></span> <span data-ttu-id="7462a-144">Aby uzyskać `ValidationType`, można użyć następujących opcji:</span><span class="sxs-lookup"><span data-stu-id="7462a-144">For `ValidationType`, you can use these options:</span></span>

    `Validator.DateTime ([error message])`  
   `Validator.Decimal([error message])`  
   `Validator.EqualsTo(otherField [, error message])`  
   `Validator.Float([error message])`  
   `Validator.Integer([error message])`  
   `Validator.Range(min, max [, error message])`  
   `Validator.RegEx(pattern [, error message])`  
   `Validator.Required([error message])`  
   `Validator.StringLength(length)`  
   `Validator.Url([error message])`
3. <span data-ttu-id="7462a-145">Gdy strona zostanie przesłana, sprawdź, czy sprawdzanie poprawności zakończyło się pomyślnie, sprawdzając `Validation.IsValid`:</span><span class="sxs-lookup"><span data-stu-id="7462a-145">When the page is submitted, check whether validation has passed by checking `Validation.IsValid`:</span></span>

    [!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample1.cs)]

    <span data-ttu-id="7462a-146">Jeśli wystąpią jakieś błędy sprawdzania poprawności, można pominąć normalne przetwarzanie strony.</span><span class="sxs-lookup"><span data-stu-id="7462a-146">If there are any validation errors, you skip normal page processing.</span></span> <span data-ttu-id="7462a-147">Na przykład, jeśli celem strony jest zaktualizowanie bazy danych, nie należy tego robić, dopóki nie zostaną naprawione wszystkie błędy walidacji.</span><span class="sxs-lookup"><span data-stu-id="7462a-147">For example, if the purpose of the page is to update a database, you don't do that until all validation errors have been fixed.</span></span>
4. <span data-ttu-id="7462a-148">Jeśli występują błędy walidacji, Wyświetl komunikaty o błędach w znacznikach strony przy użyciu `Html.ValidationSummary` lub `Html.ValidationMessage`lub obu tych elementów.</span><span class="sxs-lookup"><span data-stu-id="7462a-148">If there are validation errors, display error messages in the page's markup by using `Html.ValidationSummary` or `Html.ValidationMessage`, or both.</span></span>

<span data-ttu-id="7462a-149">Poniższy przykład przedstawia stronę, która ilustruje te kroki.</span><span class="sxs-lookup"><span data-stu-id="7462a-149">The following example shows a page that illustrates these steps.</span></span>

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample2.cshtml)]

<span data-ttu-id="7462a-150">Aby zobaczyć, jak działa Walidacja, uruchom tę stronę i świadomie wypełniania błędów.</span><span class="sxs-lookup"><span data-stu-id="7462a-150">To see how validation works, run this page and deliberately make mistakes.</span></span> <span data-ttu-id="7462a-151">Na przykład poniżej przedstawiono, jak wygląda strona, jeśli zapomnisz wprowadzić nazwę kursu, jeśli wprowadzisz i wprowadzisz nieprawidłową datę:</span><span class="sxs-lookup"><span data-stu-id="7462a-151">For example, here's what the page looks like if you forget to enter a course name, if you enter an, and if you enter an invalid date:</span></span>

![Błędy walidacji na renderowanej stronie](validating-user-input-in-aspnet-web-pages-sites/_static/image2.png)

<a id="Adding_Client-Side_Validation"></a>
## <a name="adding-client-side-validation"></a><span data-ttu-id="7462a-153">Dodawanie weryfikacji po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="7462a-153">Adding Client-Side Validation</span></span>

<span data-ttu-id="7462a-154">Domyślnie dane wejściowe użytkownika są sprawdzane po przesłaniu strony przez użytkowników — to znaczy, że sprawdzanie poprawności jest wykonywane w kodzie serwera.</span><span class="sxs-lookup"><span data-stu-id="7462a-154">By default, user input is validated after users submit the page — that is, the validation is performed in server code.</span></span> <span data-ttu-id="7462a-155">Wadą tego podejścia jest to, że użytkownicy nie wiedzą, że wystąpił błąd do momentu przesłania strony.</span><span class="sxs-lookup"><span data-stu-id="7462a-155">A disadvantage of this approach is that users don't know that they've made an error until after they submit the page.</span></span> <span data-ttu-id="7462a-156">Jeśli formularz jest długi lub złożony, raportowanie błędów tylko po przesłaniu strony może być niewygodny dla użytkownika.</span><span class="sxs-lookup"><span data-stu-id="7462a-156">If a form is long or complex, reporting errors only after the page is submitted can be inconvenient to the user.</span></span>

<span data-ttu-id="7462a-157">Możesz dodać obsługę, aby przeprowadzić walidację w skrypcie klienta.</span><span class="sxs-lookup"><span data-stu-id="7462a-157">You can add support to perform validation in client script.</span></span> <span data-ttu-id="7462a-158">W takim przypadku sprawdzanie poprawności jest wykonywane, gdy użytkownicy pracują w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="7462a-158">In that case, the validation is performed as users work in the browser.</span></span> <span data-ttu-id="7462a-159">Załóżmy na przykład, że określisz, że wartość powinna być liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="7462a-159">For example, suppose you specify that a value should be an integer.</span></span> <span data-ttu-id="7462a-160">Jeśli użytkownik wprowadzi wartość niebędącą liczbą całkowitą, zostanie zgłoszony błąd zaraz po opuszczeniu pola entry przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="7462a-160">If a user enters a non-integer value, the error is reported as soon as the user leaves the entry field.</span></span> <span data-ttu-id="7462a-161">Użytkownicy uzyskują natychmiastowe Opinie, które są dla nich wygodne.</span><span class="sxs-lookup"><span data-stu-id="7462a-161">Users get immediate feedback, which is convenient for them.</span></span> <span data-ttu-id="7462a-162">Walidacja oparta na kliencie może również zmniejszyć liczbę przypadków, w których użytkownik musi przesłać formularz, aby skorygować wiele błędów.</span><span class="sxs-lookup"><span data-stu-id="7462a-162">Client-based validation can also reduce the number of times that the user has to submit the form to correct multiple errors.</span></span>

> [!NOTE]
> <span data-ttu-id="7462a-163">Nawet w przypadku korzystania z walidacji po stronie klienta sprawdzanie poprawności jest zawsze wykonywane również w kodzie serwera.</span><span class="sxs-lookup"><span data-stu-id="7462a-163">Even if you use client-side validation, validation is always also performed in server code.</span></span> <span data-ttu-id="7462a-164">Wykonanie walidacji w kodzie serwera jest środkiem bezpieczeństwa, w przypadku gdy użytkownicy omijają weryfikację opartą na kliencie.</span><span class="sxs-lookup"><span data-stu-id="7462a-164">Performing validation in server code is a security measure, in case users bypass client-based validation.</span></span>

1. <span data-ttu-id="7462a-165">Zarejestruj następujące biblioteki JavaScript na stronie:</span><span class="sxs-lookup"><span data-stu-id="7462a-165">Register the following JavaScript libraries in the page:</span></span>  

    [!code-html[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample3.html)]

   <span data-ttu-id="7462a-166">Dwie biblioteki są ładowane z usługi Content Delivery Network (CDN), dzięki czemu nie trzeba mieć ich na komputerze lub serwerze.</span><span class="sxs-lookup"><span data-stu-id="7462a-166">Two of the libraries are loadable from a content delivery network (CDN), so you don't necessarily have to have them on your computer or server.</span></span> <span data-ttu-id="7462a-167">Wymagana jest jednak lokalna kopia *platformy jQuery. Sprawdź poprawność*.</span><span class="sxs-lookup"><span data-stu-id="7462a-167">However, you must have a local copy of *jquery.validate.unobtrusive.js*.</span></span> <span data-ttu-id="7462a-168">Jeśli nie pracujesz jeszcze z szablonem WebMatrix (na przykład **lokacją startową** ) zawierającym bibliotekę, Utwórz witrynę strony sieci Web opartą na **witrynie Starter**.</span><span class="sxs-lookup"><span data-stu-id="7462a-168">If you are not already working with a WebMatrix template (like **Starter Site** ) that includes the library, create a Web Pages site that's based on **Starter Site**.</span></span> <span data-ttu-id="7462a-169">Następnie skopiuj plik *js* do bieżącej lokacji.</span><span class="sxs-lookup"><span data-stu-id="7462a-169">Then copy the *.js* file to your current site.</span></span>
2. <span data-ttu-id="7462a-170">W znaczniku dla każdego elementu, który jest weryfikowany, Dodaj wywołanie do `Validation.For(field)`.</span><span class="sxs-lookup"><span data-stu-id="7462a-170">In markup, for each element that you're validating, add a call to `Validation.For(field)`.</span></span> <span data-ttu-id="7462a-171">Ta metoda emituje atrybuty, które są używane przez weryfikację po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="7462a-171">This method emits attributes that are used by client-side validation.</span></span> <span data-ttu-id="7462a-172">(Zamiast emitowania rzeczywistego kodu JavaScript metoda emituje atrybuty, takie jak `data-val-...`.</span><span class="sxs-lookup"><span data-stu-id="7462a-172">(Rather than emitting actual JavaScript code, the method emits attributes like `data-val-...`.</span></span> <span data-ttu-id="7462a-173">Te atrybuty obsługują niezauważalną weryfikację klienta, która korzysta z technologii jQuery do wykonania pracy.</span><span class="sxs-lookup"><span data-stu-id="7462a-173">These attributes support unobtrusive client validation that uses jQuery to do the work.)</span></span>

<span data-ttu-id="7462a-174">Na poniższej stronie pokazano, jak dodać funkcje sprawdzania poprawności klienta do pokazanego wcześniej przykładu.</span><span class="sxs-lookup"><span data-stu-id="7462a-174">The following page shows how to add client validation features to the example shown earlier.</span></span>

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample4.cshtml?highlight=35-39,51,61,71)]

<span data-ttu-id="7462a-175">Nie wszystkie sprawdzenia poprawności są uruchamiane na kliencie.</span><span class="sxs-lookup"><span data-stu-id="7462a-175">Not all validation checks run on the client.</span></span> <span data-ttu-id="7462a-176">W szczególności Walidacja typu danych (liczba całkowita, Data i tak dalej) nie jest uruchamiana na kliencie.</span><span class="sxs-lookup"><span data-stu-id="7462a-176">In particular, data-type validation (integer, date, and so on) don't run on the client.</span></span> <span data-ttu-id="7462a-177">Następujące sprawdzenia działają zarówno na kliencie, jak i na serwerze:</span><span class="sxs-lookup"><span data-stu-id="7462a-177">The following checks work on both the client and server:</span></span>

- `Required`
- `Range(minValue, maxValue)`
- `StringLength(maxLength[, minLength])`
- `Regex(pattern)`
- `EqualsTo(otherField)`

<span data-ttu-id="7462a-178">W tym przykładzie test dla prawidłowej daty nie będzie działał w kodzie klienta.</span><span class="sxs-lookup"><span data-stu-id="7462a-178">In this example, the test for a valid date won't work in client code.</span></span> <span data-ttu-id="7462a-179">Jednak test zostanie wykonany w kodzie serwera.</span><span class="sxs-lookup"><span data-stu-id="7462a-179">However, the test will be performed in server code.</span></span>

<a id="Formatting_Validation_Errors"></a>
## <a name="formatting-validation-errors"></a><span data-ttu-id="7462a-180">Błędy walidacji formatowania</span><span class="sxs-lookup"><span data-stu-id="7462a-180">Formatting Validation Errors</span></span>

<span data-ttu-id="7462a-181">Można kontrolować sposób wyświetlania błędów sprawdzania poprawności, definiując klasy CSS, które mają następujące zastrzeżone nazwy:</span><span class="sxs-lookup"><span data-stu-id="7462a-181">You can control how validation errors are displayed by defining CSS classes that have the following reserved names:</span></span>

- <span data-ttu-id="7462a-182">`field-validation-error`.</span><span class="sxs-lookup"><span data-stu-id="7462a-182">`field-validation-error`.</span></span> <span data-ttu-id="7462a-183">Definiuje dane wyjściowe metody `Html.ValidationMessage`, gdy jest wyświetlany błąd.</span><span class="sxs-lookup"><span data-stu-id="7462a-183">Defines the output of the `Html.ValidationMessage` method when it's displaying an error.</span></span>
- <span data-ttu-id="7462a-184">`field-validation-valid`.</span><span class="sxs-lookup"><span data-stu-id="7462a-184">`field-validation-valid`.</span></span> <span data-ttu-id="7462a-185">Definiuje dane wyjściowe metody `Html.ValidationMessage` w przypadku braku błędu.</span><span class="sxs-lookup"><span data-stu-id="7462a-185">Defines the output of the `Html.ValidationMessage` method when there is no error.</span></span>
- <span data-ttu-id="7462a-186">`input-validation-error`.</span><span class="sxs-lookup"><span data-stu-id="7462a-186">`input-validation-error`.</span></span> <span data-ttu-id="7462a-187">Definiuje, w jaki sposób elementy `<input>` są renderowane w przypadku wystąpienia błędu.</span><span class="sxs-lookup"><span data-stu-id="7462a-187">Defines how `<input>` elements are rendered when there's an error.</span></span> <span data-ttu-id="7462a-188">(Na przykład można użyć tej klasy, aby ustawić kolor tła &lt;wejściowego&gt; elementu na inny kolor, jeśli jego wartość jest nieprawidłowa.) Ta klasa CSS jest używana tylko podczas weryfikacji klienta (w ASP.NET Web Pages 2).</span><span class="sxs-lookup"><span data-stu-id="7462a-188">(For example, you can use this class to set the background color of an &lt;input&gt; element to a different color if its value is invalid.) This CSS class is used only during client validation (in ASP.NET Web Pages 2).</span></span>
- <span data-ttu-id="7462a-189">`input-validation-valid`.</span><span class="sxs-lookup"><span data-stu-id="7462a-189">`input-validation-valid`.</span></span> <span data-ttu-id="7462a-190">Definiuje wygląd elementów `<input>` w przypadku braku błędu.</span><span class="sxs-lookup"><span data-stu-id="7462a-190">Defines the appearance of `<input>` elements when there is no error.</span></span>
- <span data-ttu-id="7462a-191">`validation-summary-errors`.</span><span class="sxs-lookup"><span data-stu-id="7462a-191">`validation-summary-errors`.</span></span> <span data-ttu-id="7462a-192">Definiuje dane wyjściowe metody `Html.ValidationSummary`, która wyświetla listę błędów.</span><span class="sxs-lookup"><span data-stu-id="7462a-192">Defines the output of the `Html.ValidationSummary` method it's displaying a list of errors.</span></span>
- <span data-ttu-id="7462a-193">`validation-summary-valid`.</span><span class="sxs-lookup"><span data-stu-id="7462a-193">`validation-summary-valid`.</span></span> <span data-ttu-id="7462a-194">Definiuje dane wyjściowe metody `Html.ValidationSummary` w przypadku braku błędu.</span><span class="sxs-lookup"><span data-stu-id="7462a-194">Defines the output of the `Html.ValidationSummary` method when there is no error.</span></span>

<span data-ttu-id="7462a-195">Poniższy `<style>` bloku pokazuje reguły dotyczące warunków błędu.</span><span class="sxs-lookup"><span data-stu-id="7462a-195">The following `<style>` block shows rules for error conditions.</span></span>

[!code-css[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample5.css)]

<span data-ttu-id="7462a-196">Jeśli ten blok stylu zostanie uwzględniony na przykładowych stronach z wcześniejszego artykułu, zostanie wyświetlony komunikat o błędzie podobny do poniższego:</span><span class="sxs-lookup"><span data-stu-id="7462a-196">If you include this style block in the example pages from earlier in the article, the error display will look like the following illustration:</span></span>

![Błędy walidacji korzystające z klas stylów CSS](validating-user-input-in-aspnet-web-pages-sites/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="7462a-198">Jeśli weryfikacja klienta nie jest używana w programie ASP.NET Web Pages 2, klasy CSS dla elementów `<input>` (`input-validation-error` i `input-validation-valid` nie mają żadnego efektu.</span><span class="sxs-lookup"><span data-stu-id="7462a-198">If you're not using client validation in ASP.NET Web Pages 2, the CSS classes for the `<input>` elements (`input-validation-error` and `input-validation-valid` don't have any effect.</span></span>

### <a name="static-and-dynamic-error-display"></a><span data-ttu-id="7462a-199">Wyświetlanie błędów statycznych i dynamicznych</span><span class="sxs-lookup"><span data-stu-id="7462a-199">Static and Dynamic Error Display</span></span>

<span data-ttu-id="7462a-200">Reguły CSS są dostępne w parach, takich jak `validation-summary-errors` i `validation-summary-valid`.</span><span class="sxs-lookup"><span data-stu-id="7462a-200">The CSS rules come in pairs, such as `validation-summary-errors` and `validation-summary-valid`.</span></span> <span data-ttu-id="7462a-201">Te pary umożliwiają definiowanie reguł dla obu warunków: warunku błędu i warunku "normal" (bez błędu).</span><span class="sxs-lookup"><span data-stu-id="7462a-201">These pairs let you define rules for both conditions: an error condition and a "normal" (non-error) condition.</span></span> <span data-ttu-id="7462a-202">Ważne jest, aby zrozumieć, że znaczniki wyświetlania błędów są zawsze renderowane, nawet jeśli nie ma błędów.</span><span class="sxs-lookup"><span data-stu-id="7462a-202">It's important to understand that the markup for the error display is always rendered, even if there are no errors.</span></span> <span data-ttu-id="7462a-203">Na przykład jeśli strona ma metodę `Html.ValidationSummary` w znaczniku, Źródło strony będzie zawierać następujące znaczniki, nawet gdy strona jest żądana po raz pierwszy:</span><span class="sxs-lookup"><span data-stu-id="7462a-203">For example, if a page has an `Html.ValidationSummary` method in the markup, the page source will contain the following markup even when the page is requested for the first time:</span></span>

`<div class="validation-summary-valid" data-valmsg-summary="true"><ul></ul></div>`

<span data-ttu-id="7462a-204">Innymi słowy, Metoda `Html.ValidationSummary` zawsze renderuje element `<div>` i listę, nawet jeśli lista błędów jest pusta.</span><span class="sxs-lookup"><span data-stu-id="7462a-204">In other words, the `Html.ValidationSummary` method always renders a `<div>` element and a list, even if the error list is empty.</span></span> <span data-ttu-id="7462a-205">Podobnie Metoda `Html.ValidationMessage` zawsze renderuje element `<span>` jako symbol zastępczy dla pojedynczego błędu pola, nawet jeśli nie ma błędu.</span><span class="sxs-lookup"><span data-stu-id="7462a-205">Similarly, the `Html.ValidationMessage` method always renders a `<span>` element as a placeholder for an individual field error, even if there is no error.</span></span>

<span data-ttu-id="7462a-206">W niektórych sytuacjach wyświetlenie komunikatu o błędzie może spowodować zmianę przepływu strony i spowodować, że elementy na stronie mogą się poruszać.</span><span class="sxs-lookup"><span data-stu-id="7462a-206">In some situations, displaying an error message can cause the page to reflow and can cause elements on the page to move around.</span></span> <span data-ttu-id="7462a-207">Reguły CSS, które kończą się w `-valid` umożliwiają zdefiniowanie układu, który może pomóc uniknąć tego problemu.</span><span class="sxs-lookup"><span data-stu-id="7462a-207">The CSS rules that end in `-valid` let you define a layout that can help prevent this problem.</span></span> <span data-ttu-id="7462a-208">Na przykład można zdefiniować `field-validation-error` i `field-validation-valid` do obu mają ten sam stały rozmiar.</span><span class="sxs-lookup"><span data-stu-id="7462a-208">For example, you can define `field-validation-error` and `field-validation-valid` to both have the same fixed size.</span></span> <span data-ttu-id="7462a-209">Dzięki temu obszar wyświetlania pola jest statyczny i nie zmienia przepływu strony, jeśli zostanie wyświetlony komunikat o błędzie.</span><span class="sxs-lookup"><span data-stu-id="7462a-209">That way, the display area for the field is static and won't change the page flow if an error message is displayed.</span></span>

<a id="Validating_Data_That_Doesnt_Come_Directly_from_Users"></a>
## <a name="validating-data-that-doesnt-come-directly-from-users"></a><span data-ttu-id="7462a-210">Sprawdzanie poprawności danych, które nie pochodzą bezpośrednio od użytkowników</span><span class="sxs-lookup"><span data-stu-id="7462a-210">Validating Data That Doesn't Come Directly from Users</span></span>

<span data-ttu-id="7462a-211">Czasami musisz sprawdzić poprawność informacji, które nie pochodzą bezpośrednio z formularza HTML.</span><span class="sxs-lookup"><span data-stu-id="7462a-211">Sometimes you have to validate information that doesn't come directly from an HTML form.</span></span> <span data-ttu-id="7462a-212">Typowy przykład to strona, w której wartość jest przenoszona w ciągu zapytania, jak w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="7462a-212">A typical example is a page where a value is passed in a query string, as in the following example:</span></span>

`http://server/myapp/EditClassInformation?classid=1022`

<span data-ttu-id="7462a-213">W tym przypadku chcesz upewnić się, że wartość, która jest przenoszona na stronę (tutaj, 1022 dla wartości `classid`) jest prawidłowa.</span><span class="sxs-lookup"><span data-stu-id="7462a-213">In this case, you want to make sure that the value that's passed to the page (here, 1022 for the value of `classid`) is valid.</span></span> <span data-ttu-id="7462a-214">Nie można bezpośrednio użyć pomocnika `Validation` do przeprowadzenia tej walidacji.</span><span class="sxs-lookup"><span data-stu-id="7462a-214">You can't directly use the `Validation` helper to perform this validation.</span></span> <span data-ttu-id="7462a-215">Można jednak użyć innych funkcji systemu sprawdzania poprawności, takich jak możliwość wyświetlania komunikatów o błędach walidacji.</span><span class="sxs-lookup"><span data-stu-id="7462a-215">However, you can use other features of the validation system, like the ability to display validation error messages.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="7462a-216">**Ważne** Zawsze sprawdzaj poprawność wartości pobieranych z *dowolnego* źródła, w tym wartości pól formularza, wartości ciągu zapytania i wartości plików cookie.</span><span class="sxs-lookup"><span data-stu-id="7462a-216">**Important** Always validate values that you get from *any* source, including form-field values, query-string values, and cookie values.</span></span> <span data-ttu-id="7462a-217">Można łatwo zmienić te wartości (na przykład w przypadku złośliwych celów).</span><span class="sxs-lookup"><span data-stu-id="7462a-217">It's easy for people to change these values (perhaps for malicious purposes).</span></span> <span data-ttu-id="7462a-218">Dlatego należy sprawdzić te wartości w celu ochrony aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7462a-218">So you must check these values in order to protect your application.</span></span>

<span data-ttu-id="7462a-219">Poniższy przykład pokazuje, jak można sprawdzić poprawność wartości, która została przekazana w ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="7462a-219">The following example shows how you might validate a value that's passed in a query string.</span></span> <span data-ttu-id="7462a-220">Kod sprawdza, czy wartość nie jest pusta i czy jest liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="7462a-220">The code tests that the value is not empty and that it's an integer.</span></span>

[!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample6.cs)]

<span data-ttu-id="7462a-221">Należy zauważyć, że test jest wykonywany, gdy żądanie nie jest przesłanym formularzem (`if(!IsPost)`).</span><span class="sxs-lookup"><span data-stu-id="7462a-221">Notice that the test is performed when the request is not a form submission (`if(!IsPost)`).</span></span> <span data-ttu-id="7462a-222">Ten test zostałby przekazany po raz pierwszy, gdy żądanie strony jest wymagane, ale nie w przypadku przesłania formularza.</span><span class="sxs-lookup"><span data-stu-id="7462a-222">This test would pass the first time that the page is requested, but not when the request is a form submission.</span></span>

<span data-ttu-id="7462a-223">Aby wyświetlić ten błąd, można dodać błąd do listy błędów walidacji, wywołując `Validation.AddFormError("message")`.</span><span class="sxs-lookup"><span data-stu-id="7462a-223">To display this error, you can add the error to the list of validation errors by calling `Validation.AddFormError("message")`.</span></span> <span data-ttu-id="7462a-224">Jeśli strona zawiera wywołanie metody `Html.ValidationSummary`, błąd jest wyświetlany w tym miejscu, podobnie jak w przypadku błędu walidacji danych wejściowych przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="7462a-224">If the page contains a call to the `Html.ValidationSummary` method, the error is displayed there, just like a user-input validation error.</span></span>

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a><span data-ttu-id="7462a-225">Dodatkowe materiały</span><span class="sxs-lookup"><span data-stu-id="7462a-225">Additional Resources</span></span>

[<span data-ttu-id="7462a-226">Praca z formularzami HTML w witrynach stron sieci Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="7462a-226">Working with HTML Forms in ASP.NET Web Pages Sites</span></span>](https://go.microsoft.com/fwlink/?LinkID=202892)

---
uid: web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
title: Walidacja danych wejściowych użytkownika we wzorcu ASP.NET Web Pages witryny (Razor) | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: W tym artykule omówiono sposób sprawdzania poprawności informacji można uzyskać od użytkowników &mdash; oznacza to, się upewnić, że użytkownicy wprowadzają prawidłowe informacje w formacie HTML formularzy w programie sz jako...
ms.author: riande
ms.date: 02/20/2014
ms.assetid: 4eb060cc-cf14-41ae-bab1-14a2c15332d0
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: e6f8e1051d09d11f1756bfada44a73ba7c2a1db2
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65108595"
---
# <a name="validating-user-input-in-aspnet-web-pages-razor-sites"></a><span data-ttu-id="a7b35-103">Sprawdzanie poprawności danych wejściowych użytkownika w witrynach ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="a7b35-103">Validating User Input in ASP.NET Web Pages (Razor) Sites</span></span>

<span data-ttu-id="a7b35-104">przez [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="a7b35-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="a7b35-105">W tym artykule omówiono sposób sprawdzania poprawności informacji można uzyskać od użytkowników &mdash; czyli się upewnić, że użytkownicy wprowadzają prawidłowe informacje w formacie HTML formularzy w witrynie ASP.NET Web Pages (Razor).</span><span class="sxs-lookup"><span data-stu-id="a7b35-105">This article discusses how to validate information you get from users &mdash; that is, to make sure that users enter valid information in HTML forms in an ASP.NET Web Pages (Razor) site.</span></span>
> 
> <span data-ttu-id="a7b35-106">Zawartość:</span><span class="sxs-lookup"><span data-stu-id="a7b35-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="a7b35-107">Jak sprawdzić, czy użytkownik dane wejściowe podane przez kryteria weryfikacji zdefiniowanych przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="a7b35-107">How to check that a user's input matches validation criteria that you define.</span></span>
> - <span data-ttu-id="a7b35-108">Jak ustalić, czy zostały pomyślnie sprawdzone wszystkie testy sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="a7b35-108">How to determine whether all validation tests have passed.</span></span>
> - <span data-ttu-id="a7b35-109">Jak wyświetlić błędy sprawdzania poprawności (i jak sformatować je).</span><span class="sxs-lookup"><span data-stu-id="a7b35-109">How to display validation errors (and how to format them).</span></span>
> - <span data-ttu-id="a7b35-110">Jak sprawdzić poprawność danych, które nie pochodzi bezpośrednio od użytkowników.</span><span class="sxs-lookup"><span data-stu-id="a7b35-110">How to validate data that doesn't come directly from users.</span></span>
> 
> <span data-ttu-id="a7b35-111">Poniżej przedstawiono ASP.NET programowania pojęciami opisanymi w artykule:</span><span class="sxs-lookup"><span data-stu-id="a7b35-111">These are the ASP.NET programming concepts introduced in the article:</span></span>
> 
> - <span data-ttu-id="a7b35-112">`Validation` Pomocnika.</span><span class="sxs-lookup"><span data-stu-id="a7b35-112">The `Validation` helper.</span></span>
> - <span data-ttu-id="a7b35-113">`Html.ValidationSummary` i `Html.ValidationMessage` metody.</span><span class="sxs-lookup"><span data-stu-id="a7b35-113">The `Html.ValidationSummary` and `Html.ValidationMessage` methods.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="a7b35-114">Wersje oprogramowania używanego w tym samouczku</span><span class="sxs-lookup"><span data-stu-id="a7b35-114">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="a7b35-115">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="a7b35-115">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="a7b35-116">W tym samouczku współpracuje również z wzorca ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="a7b35-116">This tutorial also works with ASP.NET Web Pages 2.</span></span>

<span data-ttu-id="a7b35-117">Ten artykuł zawiera następujące sekcje:</span><span class="sxs-lookup"><span data-stu-id="a7b35-117">This article contains the following sections:</span></span>

- [<span data-ttu-id="a7b35-118">Omówienie sprawdzania poprawności danych wejściowych użytkownika</span><span class="sxs-lookup"><span data-stu-id="a7b35-118">Overview of User Input Validation</span></span>](#Overview_of_User_Input_Validation)
- [<span data-ttu-id="a7b35-119">Walidacja danych wejściowych użytkownika</span><span class="sxs-lookup"><span data-stu-id="a7b35-119">Validating User Input</span></span>](#Validating_User_Input)
- [<span data-ttu-id="a7b35-120">Dodawanie walidacji po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="a7b35-120">Adding Client-Side Validation</span></span>](#Adding_Client-Side_Validation)
- [<span data-ttu-id="a7b35-121">Błędy sprawdzania poprawności formatowania</span><span class="sxs-lookup"><span data-stu-id="a7b35-121">Formatting Validation Errors</span></span>](#Formatting_Validation_Errors)
- [<span data-ttu-id="a7b35-122">Sprawdzanie poprawności danych, które nie pochodzi bezpośrednio od użytkowników</span><span class="sxs-lookup"><span data-stu-id="a7b35-122">Validating Data That Doesn't Come Directly from Users</span></span>](#Validating_Data_That_Doesnt_Come_Directly_from_Users)

<a id="Overview_of_User_Input_Validation"></a>
## <a name="overview-of-user-input-validation"></a><span data-ttu-id="a7b35-123">Omówienie sprawdzania poprawności danych wejściowych użytkownika</span><span class="sxs-lookup"><span data-stu-id="a7b35-123">Overview of User Input Validation</span></span>

<span data-ttu-id="a7b35-124">Jeśli Poproś użytkowników o podanie informacji na stronie — na przykład w formie — jest ważne upewnić się, że wartości, które użytkownik podał są prawidłowe.</span><span class="sxs-lookup"><span data-stu-id="a7b35-124">If you ask users to enter information in a page — for example, into a form — it's important to make sure that the values that they enter are valid.</span></span> <span data-ttu-id="a7b35-125">Na przykład nie chcesz do przetwarzania formularza, brak jest krytycznych informacji.</span><span class="sxs-lookup"><span data-stu-id="a7b35-125">For example, you don't want to process a form that's missing critical information.</span></span>

<span data-ttu-id="a7b35-126">Podczas wprowadzania wartości do formularza HTML, wartości, które użytkownik podał są ciągami.</span><span class="sxs-lookup"><span data-stu-id="a7b35-126">When users enter values into an HTML form, the values that they enter are strings.</span></span> <span data-ttu-id="a7b35-127">W wielu przypadkach wartości, potrzebne są niektóre inne typy danych, takich jak liczby całkowite lub daty.</span><span class="sxs-lookup"><span data-stu-id="a7b35-127">In many cases, the values you need are some other data types, like integers or dates.</span></span> <span data-ttu-id="a7b35-128">W związku z tym również należy upewnić się, że wartości, które użytkownicy wprowadzają może zostać prawidłowo skonwertowany do formatu odpowiedni typ danych.</span><span class="sxs-lookup"><span data-stu-id="a7b35-128">Therefore, you also have to make sure that the values that users enter can be correctly converted to the appropriate data types.</span></span>

<span data-ttu-id="a7b35-129">Można również zainstalować pewne ograniczenia na podstawie wartości.</span><span class="sxs-lookup"><span data-stu-id="a7b35-129">You might also have certain restrictions on the values.</span></span> <span data-ttu-id="a7b35-130">Nawet wtedy, gdy użytkownicy poprawnie wprowadź liczbę całkowitą, na przykład, konieczne może być upewnij się, że wartość mieści się w pewnym zakresie.</span><span class="sxs-lookup"><span data-stu-id="a7b35-130">Even if users correctly enter an integer, for example, you might need to make sure that the value falls within a certain range.</span></span>

![Błędy sprawdzania poprawności, które używają klasy stylów CSS](validating-user-input-in-aspnet-web-pages-sites/_static/image1.png)

> [!NOTE] 
> 
> <span data-ttu-id="a7b35-132">**Ważne** sprawdzanie poprawności danych wejściowych użytkownika jest również ważne dla bezpieczeństwa.</span><span class="sxs-lookup"><span data-stu-id="a7b35-132">**Important** Validating user input is also important for security.</span></span> <span data-ttu-id="a7b35-133">Ograniczenie wartości, które użytkownicy mogą wprowadzać w formularzach, można zmniejszyć prawdopodobieństwo, że ktoś wprowadź wartość, która może naruszyć bezpieczeństwo witryny sieci.</span><span class="sxs-lookup"><span data-stu-id="a7b35-133">When you restrict the values that users can enter in forms, you reduce the chance that someone can enter a value that can compromise the security of your site.</span></span>

<a id="Validating_User_Input"></a>
## <a name="validating-user-input"></a><span data-ttu-id="a7b35-134">Walidacja danych wejściowych użytkownika</span><span class="sxs-lookup"><span data-stu-id="a7b35-134">Validating User Input</span></span>

<span data-ttu-id="a7b35-135">W programie ASP.NET Web Pages 2 można użyć `Validator` element pomocniczy służący do danych wejściowych użytkownika.</span><span class="sxs-lookup"><span data-stu-id="a7b35-135">In ASP.NET Web Pages 2, you can use the `Validator` helper to test user input.</span></span> <span data-ttu-id="a7b35-136">Podstawowe podejście jest wykonanie następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="a7b35-136">The basic approach is to do the following:</span></span>

1. <span data-ttu-id="a7b35-137">Określ, której dane wejściowe elementów (pola), którą chcesz zweryfikować.</span><span class="sxs-lookup"><span data-stu-id="a7b35-137">Determine which input elements (fields) you want to validate.</span></span>

    <span data-ttu-id="a7b35-138">Zazwyczaj Sprawdź poprawność wartości w `<input>` elementów w formularzu.</span><span class="sxs-lookup"><span data-stu-id="a7b35-138">You typically validate values in `<input>` elements in a form.</span></span> <span data-ttu-id="a7b35-139">Jednak jest dobrą praktyką Aby sprawdzić poprawność wszystkich danych wejściowych, nawet w danych wejściowych, która pochodzi z elementu ograniczone, takich jak `<select>` listy.</span><span class="sxs-lookup"><span data-stu-id="a7b35-139">However, it's a good practice to validate all input, even input that comes from a constrained element like a `<select>` list.</span></span> <span data-ttu-id="a7b35-140">Pomaga to upewnij się, że użytkownicy nie obejścia formantów na stronie i przesyłania formularza.</span><span class="sxs-lookup"><span data-stu-id="a7b35-140">This helps to make sure that users don't bypass the controls on a page and submit a form.</span></span>
2. <span data-ttu-id="a7b35-141">W kodzie strony dodać sprawdzanie poprawności poszczególnych dla każdego elementu wejściowego przy użyciu metody `Validation` pomocnika.</span><span class="sxs-lookup"><span data-stu-id="a7b35-141">In the page code, add individual validation checks for each input element by using methods of the `Validation` helper.</span></span>

    <span data-ttu-id="a7b35-142">Aby sprawdzić, czy wymagane pola, użyj `Validation.RequireField(field, [error message])` (dla poszczególnych pól) lub `Validation.RequireFields(field1, field2, ...))` (Aby uzyskać listę pól).</span><span class="sxs-lookup"><span data-stu-id="a7b35-142">To check for required fields, use `Validation.RequireField(field, [error message])` (for an individual field) or `Validation.RequireFields(field1, field2, ...))` (for a list of fields).</span></span> <span data-ttu-id="a7b35-143">W przypadku innych typów weryfikacji, użyj `Validation.Add(field, ValidationType)`.</span><span class="sxs-lookup"><span data-stu-id="a7b35-143">For other types of validation, use `Validation.Add(field, ValidationType)`.</span></span> <span data-ttu-id="a7b35-144">Aby uzyskać `ValidationType`, możesz użyć tych opcji:</span><span class="sxs-lookup"><span data-stu-id="a7b35-144">For `ValidationType`, you can use these options:</span></span>

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
3. <span data-ttu-id="a7b35-145">Po przesłaniu strony należy sprawdzić, czy Weryfikacja został przekazany, sprawdzając `Validation.IsValid`:</span><span class="sxs-lookup"><span data-stu-id="a7b35-145">When the page is submitted, check whether validation has passed by checking `Validation.IsValid`:</span></span>

    [!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample1.cs)]

    <span data-ttu-id="a7b35-146">Jeśli występują błędy sprawdzania poprawności, możesz pominąć strony normalnego przetwarzania.</span><span class="sxs-lookup"><span data-stu-id="a7b35-146">If there are any validation errors, you skip normal page processing.</span></span> <span data-ttu-id="a7b35-147">Na przykład jeśli strona ma na celu aktualizowanie bazy danych, nie zrobisz, dopóki nie zostały naprawione wszystkie błędy weryfikacji.</span><span class="sxs-lookup"><span data-stu-id="a7b35-147">For example, if the purpose of the page is to update a database, you don't do that until all validation errors have been fixed.</span></span>
4. <span data-ttu-id="a7b35-148">Jeśli występują błędy sprawdzania poprawności, wyświetlić komunikaty o błędach w znaczniku strony przy użyciu `Html.ValidationSummary` lub `Html.ValidationMessage`, lub obu.</span><span class="sxs-lookup"><span data-stu-id="a7b35-148">If there are validation errors, display error messages in the page's markup by using `Html.ValidationSummary` or `Html.ValidationMessage`, or both.</span></span>

<span data-ttu-id="a7b35-149">Strona, która ilustruje te kroki można znaleźć w poniższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="a7b35-149">The following example shows a page that illustrates these steps.</span></span>

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample2.cshtml)]

<span data-ttu-id="a7b35-150">Aby zobaczyć, jak działa sprawdzania poprawności, uruchom na tej stronie, a następnie celowo popełnione.</span><span class="sxs-lookup"><span data-stu-id="a7b35-150">To see how validation works, run this page and deliberately make mistakes.</span></span> <span data-ttu-id="a7b35-151">Na przykład, w tym miejscu wygląda strony Jeśli zapomnisz o wprowadzenie nazwy kurs, po wprowadzeniu, a jeśli wprowadzasz nieprawidłowa data:</span><span class="sxs-lookup"><span data-stu-id="a7b35-151">For example, here's what the page looks like if you forget to enter a course name, if you enter an, and if you enter an invalid date:</span></span>

![Błędy sprawdzania poprawności na renderowanej stronie](validating-user-input-in-aspnet-web-pages-sites/_static/image2.png)

<a id="Adding_Client-Side_Validation"></a>
## <a name="adding-client-side-validation"></a><span data-ttu-id="a7b35-153">Dodawanie walidacji po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="a7b35-153">Adding Client-Side Validation</span></span>

<span data-ttu-id="a7b35-154">Domyślnie dane wejściowe użytkownika sprawdzania poprawności po użytkowników przedstawia stronę — oznacza to, sprawdzanie poprawności jest wykonywane w kodzie serwera.</span><span class="sxs-lookup"><span data-stu-id="a7b35-154">By default, user input is validated after users submit the page — that is, the validation is performed in server code.</span></span> <span data-ttu-id="a7b35-155">Wadą tego podejścia jest to, że użytkownicy nie wiedzą one wprowadzone błąd aż po ich Prześlij na stronie.</span><span class="sxs-lookup"><span data-stu-id="a7b35-155">A disadvantage of this approach is that users don't know that they've made an error until after they submit the page.</span></span> <span data-ttu-id="a7b35-156">Jeśli formularz jest długie lub zbyt złożone, raportowanie błędów tylko wtedy, gdy strona jest przekazywana może być niewygodne do użytkownika.</span><span class="sxs-lookup"><span data-stu-id="a7b35-156">If a form is long or complex, reporting errors only after the page is submitted can be inconvenient to the user.</span></span>

<span data-ttu-id="a7b35-157">Możesz dodać obsługę przeprowadzania weryfikacji w skrypt po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="a7b35-157">You can add support to perform validation in client script.</span></span> <span data-ttu-id="a7b35-158">W takiej sytuacji sprawdzanie poprawności jest wykonywane, gdy użytkownik pracuje w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="a7b35-158">In that case, the validation is performed as users work in the browser.</span></span> <span data-ttu-id="a7b35-159">Na przykład załóżmy, że możesz określić, czy wartość powinna być liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="a7b35-159">For example, suppose you specify that a value should be an integer.</span></span> <span data-ttu-id="a7b35-160">Jeśli użytkownik wprowadza wartość nie jest liczbą całkowitą, zgłaszany jest błąd, zaraz po użytkownik opuści pole wprowadzania.</span><span class="sxs-lookup"><span data-stu-id="a7b35-160">If a user enters a non-integer value, the error is reported as soon as the user leaves the entry field.</span></span> <span data-ttu-id="a7b35-161">Użytkownicy natychmiast otrzymać opinię, która jest wygodne w przypadku ich.</span><span class="sxs-lookup"><span data-stu-id="a7b35-161">Users get immediate feedback, which is convenient for them.</span></span> <span data-ttu-id="a7b35-162">Walidacja oparta na klienta również pozwala zmniejszyć liczbę prób użytkownik musi przesłać formularz, aby rozwiązać wiele błędów.</span><span class="sxs-lookup"><span data-stu-id="a7b35-162">Client-based validation can also reduce the number of times that the user has to submit the form to correct multiple errors.</span></span>

> [!NOTE]
> <span data-ttu-id="a7b35-163">Nawet w przypadku używania weryfikacji po stronie klienta jest zawsze również przeprowadzana Walidacja w kodzie serwera.</span><span class="sxs-lookup"><span data-stu-id="a7b35-163">Even if you use client-side validation, validation is always also performed in server code.</span></span> <span data-ttu-id="a7b35-164">Wykonywanie sprawdzania poprawności w kodzie serwera jest miarą zabezpieczeń, w przypadku, gdy użytkownicy pominąć weryfikacji opartą na kliencie.</span><span class="sxs-lookup"><span data-stu-id="a7b35-164">Performing validation in server code is a security measure, in case users bypass client-based validation.</span></span>

1. <span data-ttu-id="a7b35-165">Zarejestruj następujące biblioteki JavaScript na stronie:</span><span class="sxs-lookup"><span data-stu-id="a7b35-165">Register the following JavaScript libraries in the page:</span></span>  

    [!code-html[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample3.html)]

   <span data-ttu-id="a7b35-166">Są dwa bibliotek obciążana z sieci dostarczania zawartości (CDN), dzięki czemu masz zawsze mieć je na komputerze lub serwerze.</span><span class="sxs-lookup"><span data-stu-id="a7b35-166">Two of the libraries are loadable from a content delivery network (CDN), so you don't necessarily have to have them on your computer or server.</span></span> <span data-ttu-id="a7b35-167">Jednak musi mieć kopię lokalną *jquery.validate.unobtrusive.js*.</span><span class="sxs-lookup"><span data-stu-id="a7b35-167">However, you must have a local copy of *jquery.validate.unobtrusive.js*.</span></span> <span data-ttu-id="a7b35-168">Jeśli nie już pracujesz za pomocą szablonu programu WebMatrix (takich jak **witryny początkowej** ) zawierającej biblioteki, utworzyć witrynę stron sieci Web, która opiera się na **witryny początkowej**.</span><span class="sxs-lookup"><span data-stu-id="a7b35-168">If you are not already working with a WebMatrix template (like **Starter Site** ) that includes the library, create a Web Pages site that's based on **Starter Site**.</span></span> <span data-ttu-id="a7b35-169">Następnie skopiuj *js* pliku do bieżącej lokacji.</span><span class="sxs-lookup"><span data-stu-id="a7b35-169">Then copy the *.js* file to your current site.</span></span>
2. <span data-ttu-id="a7b35-170">W znaczniku dla każdego elementu, który jest sprawdzanie poprawności, należy dodać wywołanie `Validation.For(field)`.</span><span class="sxs-lookup"><span data-stu-id="a7b35-170">In markup, for each element that you're validating, add a call to `Validation.For(field)`.</span></span> <span data-ttu-id="a7b35-171">Ta metoda generuje atrybuty, które są używane przez weryfikację po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="a7b35-171">This method emits attributes that are used by client-side validation.</span></span> <span data-ttu-id="a7b35-172">(Zamiast emitowania rzeczywisty kod JavaScript, metoda emituje atrybutów, takich jak `data-val-...`.</span><span class="sxs-lookup"><span data-stu-id="a7b35-172">(Rather than emitting actual JavaScript code, the method emits attributes like `data-val-...`.</span></span> <span data-ttu-id="a7b35-173">Te atrybuty obsługuje sprawdzanie poprawności dyskretnego kodu klienta, która używa technologii jQuery, które wykonają tę pracę).</span><span class="sxs-lookup"><span data-stu-id="a7b35-173">These attributes support unobtrusive client validation that uses jQuery to do the work.)</span></span>

<span data-ttu-id="a7b35-174">Następująca strona przedstawiono sposób dodawania funkcji weryfikacji klienta, jak w przykładzie przedstawionym wcześniej.</span><span class="sxs-lookup"><span data-stu-id="a7b35-174">The following page shows how to add client validation features to the example shown earlier.</span></span>

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample4.cshtml?highlight=35-39,51,61,71)]

<span data-ttu-id="a7b35-175">Sprawdzanie poprawności nie wszystkie uruchomienia na kliencie.</span><span class="sxs-lookup"><span data-stu-id="a7b35-175">Not all validation checks run on the client.</span></span> <span data-ttu-id="a7b35-176">W szczególności weryfikacji typu danych (liczba całkowita, daty i tak dalej) nie działają na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="a7b35-176">In particular, data-type validation (integer, date, and so on) don't run on the client.</span></span> <span data-ttu-id="a7b35-177">Następujące testy pracować na kliencie i serwerze:</span><span class="sxs-lookup"><span data-stu-id="a7b35-177">The following checks work on both the client and server:</span></span>

- `Required`
- `Range(minValue, maxValue)`
- `StringLength(maxLength[, minLength])`
- `Regex(pattern)`
- `EqualsTo(otherField)`

<span data-ttu-id="a7b35-178">W tym przykładzie test na prawidłową datę nie będą działać w kodzie klienta.</span><span class="sxs-lookup"><span data-stu-id="a7b35-178">In this example, the test for a valid date won't work in client code.</span></span> <span data-ttu-id="a7b35-179">Jednak test zostanie wykonany w kodzie serwera.</span><span class="sxs-lookup"><span data-stu-id="a7b35-179">However, the test will be performed in server code.</span></span>

<a id="Formatting_Validation_Errors"></a>
## <a name="formatting-validation-errors"></a><span data-ttu-id="a7b35-180">Błędy sprawdzania poprawności formatowania</span><span class="sxs-lookup"><span data-stu-id="a7b35-180">Formatting Validation Errors</span></span>

<span data-ttu-id="a7b35-181">Można kontrolować sposób wyświetlania błędów sprawdzania poprawności, definiując klas CSS, które mają następujące nazwy zarezerwowane:</span><span class="sxs-lookup"><span data-stu-id="a7b35-181">You can control how validation errors are displayed by defining CSS classes that have the following reserved names:</span></span>

- <span data-ttu-id="a7b35-182">`field-validation-error`.</span><span class="sxs-lookup"><span data-stu-id="a7b35-182">`field-validation-error`.</span></span> <span data-ttu-id="a7b35-183">Definiuje dane wyjściowe `Html.ValidationMessage` metody, gdy go jest wyświetlany błąd.</span><span class="sxs-lookup"><span data-stu-id="a7b35-183">Defines the output of the `Html.ValidationMessage` method when it's displaying an error.</span></span>
- <span data-ttu-id="a7b35-184">`field-validation-valid`.</span><span class="sxs-lookup"><span data-stu-id="a7b35-184">`field-validation-valid`.</span></span> <span data-ttu-id="a7b35-185">Definiuje dane wyjściowe `Html.ValidationMessage` metody, gdy nie ma błędów.</span><span class="sxs-lookup"><span data-stu-id="a7b35-185">Defines the output of the `Html.ValidationMessage` method when there is no error.</span></span>
- <span data-ttu-id="a7b35-186">`input-validation-error`.</span><span class="sxs-lookup"><span data-stu-id="a7b35-186">`input-validation-error`.</span></span> <span data-ttu-id="a7b35-187">Definiuje sposób `<input>` elementy są renderowane po wystąpieniu błędu.</span><span class="sxs-lookup"><span data-stu-id="a7b35-187">Defines how `<input>` elements are rendered when there's an error.</span></span> <span data-ttu-id="a7b35-188">(Na przykład, można użyć tej klasy, aby ustawić kolor tła &lt;wejściowych&gt; elementu na inny kolor, jeśli jego wartość jest nieprawidłowa.) Ta klasa CSS jest używana tylko podczas weryfikacji klienta (w programie ASP.NET Web Pages 2).</span><span class="sxs-lookup"><span data-stu-id="a7b35-188">(For example, you can use this class to set the background color of an &lt;input&gt; element to a different color if its value is invalid.) This CSS class is used only during client validation (in ASP.NET Web Pages 2).</span></span>
- <span data-ttu-id="a7b35-189">`input-validation-valid`.</span><span class="sxs-lookup"><span data-stu-id="a7b35-189">`input-validation-valid`.</span></span> <span data-ttu-id="a7b35-190">Definiuje wygląd elementów `<input>` elementów, gdy nie ma błędów.</span><span class="sxs-lookup"><span data-stu-id="a7b35-190">Defines the appearance of `<input>` elements when there is no error.</span></span>
- <span data-ttu-id="a7b35-191">`validation-summary-errors`.</span><span class="sxs-lookup"><span data-stu-id="a7b35-191">`validation-summary-errors`.</span></span> <span data-ttu-id="a7b35-192">Definiuje dane wyjściowe `Html.ValidationSummary` metoda Wyświetla listę błędów.</span><span class="sxs-lookup"><span data-stu-id="a7b35-192">Defines the output of the `Html.ValidationSummary` method it's displaying a list of errors.</span></span>
- <span data-ttu-id="a7b35-193">`validation-summary-valid`.</span><span class="sxs-lookup"><span data-stu-id="a7b35-193">`validation-summary-valid`.</span></span> <span data-ttu-id="a7b35-194">Definiuje dane wyjściowe `Html.ValidationSummary` metody, gdy nie ma błędów.</span><span class="sxs-lookup"><span data-stu-id="a7b35-194">Defines the output of the `Html.ValidationSummary` method when there is no error.</span></span>

<span data-ttu-id="a7b35-195">Następujące `<style>` bloku przedstawiono zasady dotyczące warunków błędów.</span><span class="sxs-lookup"><span data-stu-id="a7b35-195">The following `<style>` block shows rules for error conditions.</span></span>

[!code-css[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample5.css)]

<span data-ttu-id="a7b35-196">Jeśli dodasz tego bloku stylu w przykładzie strony z we wcześniejszej części tego artykułu, wyświetlania błędów będzie wyglądać podobnie do poniższej ilustracji:</span><span class="sxs-lookup"><span data-stu-id="a7b35-196">If you include this style block in the example pages from earlier in the article, the error display will look like the following illustration:</span></span>

![Błędy sprawdzania poprawności, które używają klasy stylów CSS](validating-user-input-in-aspnet-web-pages-sites/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="a7b35-198">Jeśli nie używasz Sprawdzanie poprawności klienta w programie ASP.NET Web Pages 2, arkusze CSS klasy dla `<input>` elementów (`input-validation-error` i `input-validation-valid` nie ma żadnego efektu.</span><span class="sxs-lookup"><span data-stu-id="a7b35-198">If you're not using client validation in ASP.NET Web Pages 2, the CSS classes for the `<input>` elements (`input-validation-error` and `input-validation-valid` don't have any effect.</span></span>

### <a name="static-and-dynamic-error-display"></a><span data-ttu-id="a7b35-199">Wyświetlanie błędów statycznych i dynamicznych</span><span class="sxs-lookup"><span data-stu-id="a7b35-199">Static and Dynamic Error Display</span></span>

<span data-ttu-id="a7b35-200">Reguły CSS są dostępne w parach, takich jak `validation-summary-errors` i `validation-summary-valid`.</span><span class="sxs-lookup"><span data-stu-id="a7b35-200">The CSS rules come in pairs, such as `validation-summary-errors` and `validation-summary-valid`.</span></span> <span data-ttu-id="a7b35-201">Te pary pozwalają zdefiniować zasady dla obu warunków: warunek błędu i warunek "normal" (bez błędów).</span><span class="sxs-lookup"><span data-stu-id="a7b35-201">These pairs let you define rules for both conditions: an error condition and a "normal" (non-error) condition.</span></span> <span data-ttu-id="a7b35-202">Jest ważne dowiedzieć się, że zawsze renderowania kodu znaczników dla wyświetlania błędów, nawet, jeśli nie ma żadnych błędów.</span><span class="sxs-lookup"><span data-stu-id="a7b35-202">It's important to understand that the markup for the error display is always rendered, even if there are no errors.</span></span> <span data-ttu-id="a7b35-203">Na przykład, jeśli strona ma `Html.ValidationSummary` metody w znaczniku, źródło strony będzie zawierać następujące znaczniki nawet wtedy, gdy strona jest wykonywana po raz pierwszy:</span><span class="sxs-lookup"><span data-stu-id="a7b35-203">For example, if a page has an `Html.ValidationSummary` method in the markup, the page source will contain the following markup even when the page is requested for the first time:</span></span>

`<div class="validation-summary-valid" data-valmsg-summary="true"><ul></ul></div>`

<span data-ttu-id="a7b35-204">Innymi słowy `Html.ValidationSummary` metody zawsze powoduje wyświetlenie `<div>` elementu i listy, nawet jeśli lista błędów jest pusty.</span><span class="sxs-lookup"><span data-stu-id="a7b35-204">In other words, the `Html.ValidationSummary` method always renders a `<div>` element and a list, even if the error list is empty.</span></span> <span data-ttu-id="a7b35-205">Podobnie `Html.ValidationMessage` metody zawsze powoduje wyświetlenie `<span>` elementu jako symbolu zastępczego dla poszczególnych pól błąd, nawet jeśli nie ma błędów.</span><span class="sxs-lookup"><span data-stu-id="a7b35-205">Similarly, the `Html.ValidationMessage` method always renders a `<span>` element as a placeholder for an individual field error, even if there is no error.</span></span>

<span data-ttu-id="a7b35-206">W niektórych sytuacjach, wyświetlając komunikat o błędzie może spowodować, że strona ułożenia i może spowodować, że elementy na stronie, aby poruszać się.</span><span class="sxs-lookup"><span data-stu-id="a7b35-206">In some situations, displaying an error message can cause the page to reflow and can cause elements on the page to move around.</span></span> <span data-ttu-id="a7b35-207">Reguły CSS, które kończą się na `-valid` pozwalają zdefiniować układ, które mogą pomóc uniknąć tego problemu.</span><span class="sxs-lookup"><span data-stu-id="a7b35-207">The CSS rules that end in `-valid` let you define a layout that can help prevent this problem.</span></span> <span data-ttu-id="a7b35-208">Na przykład można zdefiniować `field-validation-error` i `field-validation-valid` zarówno mają takie same stałe rozmiar.</span><span class="sxs-lookup"><span data-stu-id="a7b35-208">For example, you can define `field-validation-error` and `field-validation-valid` to both have the same fixed size.</span></span> <span data-ttu-id="a7b35-209">W ten sposób obszaru wyświetlania pola jest statyczna i nie ulegnie zmianie przepływem strony, jeśli jest wyświetlany komunikat o błędzie.</span><span class="sxs-lookup"><span data-stu-id="a7b35-209">That way, the display area for the field is static and won't change the page flow if an error message is displayed.</span></span>

<a id="Validating_Data_That_Doesnt_Come_Directly_from_Users"></a>
## <a name="validating-data-that-doesnt-come-directly-from-users"></a><span data-ttu-id="a7b35-210">Sprawdzanie poprawności danych, które nie pochodzi bezpośrednio od użytkowników</span><span class="sxs-lookup"><span data-stu-id="a7b35-210">Validating Data That Doesn't Come Directly from Users</span></span>

<span data-ttu-id="a7b35-211">Czasami trzeba sprawdzić informacje, które nie pochodzą bezpośrednio z formularza HTML.</span><span class="sxs-lookup"><span data-stu-id="a7b35-211">Sometimes you have to validate information that doesn't come directly from an HTML form.</span></span> <span data-ttu-id="a7b35-212">Typowym przykładem jest strona, której wartością jest przekazywany w ciągu zapytania, jak w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="a7b35-212">A typical example is a page where a value is passed in a query string, as in the following example:</span></span>

`http://server/myapp/EditClassInformation?classid=1022`

<span data-ttu-id="a7b35-213">W tym przypadku chcesz upewnij się, że wartość, która jest przekazywana do strony (tutaj, 1022 dla wartości `classid`) jest nieprawidłowy.</span><span class="sxs-lookup"><span data-stu-id="a7b35-213">In this case, you want to make sure that the value that's passed to the page (here, 1022 for the value of `classid`) is valid.</span></span> <span data-ttu-id="a7b35-214">Nie można używać bezpośrednio `Validation` element pomocniczy służący do wykonywania tej weryfikacji.</span><span class="sxs-lookup"><span data-stu-id="a7b35-214">You can't directly use the `Validation` helper to perform this validation.</span></span> <span data-ttu-id="a7b35-215">Jednak można użyć innych funkcji systemu sprawdzania poprawności, takich jak możliwość wyświetlania komunikatów o błędach weryfikacji.</span><span class="sxs-lookup"><span data-stu-id="a7b35-215">However, you can use other features of the validation system, like the ability to display validation error messages.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="a7b35-216">**Ważne** zawsze sprawdzają poprawność wartości, które można uzyskać z *wszelkie* źródła, takiego jak wartości pola formularza, wartości ciągu zapytania i wartości pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="a7b35-216">**Important** Always validate values that you get from *any* source, including form-field values, query-string values, and cookie values.</span></span> <span data-ttu-id="a7b35-217">To proste, użytkownicy będą mogli zmieniać te wartości (np. do złośliwych celów).</span><span class="sxs-lookup"><span data-stu-id="a7b35-217">It's easy for people to change these values (perhaps for malicious purposes).</span></span> <span data-ttu-id="a7b35-218">Dlatego należy sprawdzić te wartości w celu ochrony aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a7b35-218">So you must check these values in order to protect your application.</span></span>

<span data-ttu-id="a7b35-219">Poniższy przykład pokazuje, jak może zweryfikować wartości, który jest przekazywany w ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="a7b35-219">The following example shows how you might validate a value that's passed in a query string.</span></span> <span data-ttu-id="a7b35-220">Kod sprawdza, czy wartość nie jest pusty i że jest liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="a7b35-220">The code tests that the value is not empty and that it's an integer.</span></span>

[!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample6.cs)]

<span data-ttu-id="a7b35-221">Należy zauważyć, że test jest wykonywane, gdy żądanie jest przesłanie formularza (`if(!IsPost)`).</span><span class="sxs-lookup"><span data-stu-id="a7b35-221">Notice that the test is performed when the request is not a form submission (`if(!IsPost)`).</span></span> <span data-ttu-id="a7b35-222">Ten test przejdzie przy pierwszym żądaniu strony, ale nie, gdy żądanie jest przesyłania formularza.</span><span class="sxs-lookup"><span data-stu-id="a7b35-222">This test would pass the first time that the page is requested, but not when the request is a form submission.</span></span>

<span data-ttu-id="a7b35-223">Aby wyświetlić ten błąd, można dodać błąd do listy błędów sprawdzania poprawności, wywołując `Validation.AddFormError("message")`.</span><span class="sxs-lookup"><span data-stu-id="a7b35-223">To display this error, you can add the error to the list of validation errors by calling `Validation.AddFormError("message")`.</span></span> <span data-ttu-id="a7b35-224">Jeśli strona zawiera wywołanie `Html.ValidationSummary` metody, błąd jest wyświetlany, podobnie jak błąd sprawdzania poprawności danych wejściowych użytkownika.</span><span class="sxs-lookup"><span data-stu-id="a7b35-224">If the page contains a call to the `Html.ValidationSummary` method, the error is displayed there, just like a user-input validation error.</span></span>

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a><span data-ttu-id="a7b35-225">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="a7b35-225">Additional Resources</span></span>

[<span data-ttu-id="a7b35-226">Praca z formularzami HTML w witrynach ASP.NET Web Pages</span><span class="sxs-lookup"><span data-stu-id="a7b35-226">Working with HTML Forms in ASP.NET Web Pages Sites</span></span>](https://go.microsoft.com/fwlink/?LinkID=202892)

---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
title: Korzystanie z menu podręcznego interfejsu użytkownika HTML5 i jQuery DatePicker z ASP.NET MVC — część 3 | Microsoft Docs
author: Rick-Anderson
description: W tym samouczku przedstawiono podstawowe informacje dotyczące pracy z szablonami edytora, szablonów wyświetlania i menu podręcznego interfejsu użytkownika jQuery DatePicker w ASP.NET MV...
ms.author: riande
ms.date: 08/29/2011
ms.assetid: 8f5f91ae-12d7-4cf3-ac09-4bb53d07ee60
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
msc.type: authoredcontent
ms.openlocfilehash: b3249397e54e64538c4dc78e5fe8b94656e8962b
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457898"
---
# <a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-3"></a><span data-ttu-id="8f664-103">Korzystanie z menu podręcznego interfejsu użytkownika HTML5 i jQuery DatePicker z ASP.NET MVC — część 3</span><span class="sxs-lookup"><span data-stu-id="8f664-103">Using the HTML5 and jQuery UI Datepicker Popup Calendar with ASP.NET MVC - Part 3</span></span>

<span data-ttu-id="8f664-104">Autor [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8f664-104">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

> <span data-ttu-id="8f664-105">W tym samouczku przedstawiono podstawowe informacje dotyczące pracy z szablonami edytora, szablonów wyświetlania i okienka podręcznego interfejsu użytkownika jQuery DatePicker w aplikacji sieci Web ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="8f664-105">This tutorial will teach you the basics of how to work with editor templates, display templates, and the jQuery UI datepicker popup calendar in an ASP.NET MVC Web application.</span></span>

## <a name="working-with-complex-types"></a><span data-ttu-id="8f664-106">Praca z typami złożonymi</span><span class="sxs-lookup"><span data-stu-id="8f664-106">Working with Complex Types</span></span>

<span data-ttu-id="8f664-107">W tej sekcji utworzysz klasę adresów i dowiesz się, jak utworzyć szablon w celu wyświetlenia go.</span><span class="sxs-lookup"><span data-stu-id="8f664-107">In this section you'll create an address class and learn how to create a template to display it.</span></span>

<span data-ttu-id="8f664-108">W folderze *modele* Utwórz nowy plik klasy o nazwie *Person.cs* , gdzie umieścisz dwa typy: klasy `Person` i klasy `Address`.</span><span class="sxs-lookup"><span data-stu-id="8f664-108">In the *Models* folder, create a new class file named *Person.cs* where you'll put two types: a `Person` class and an `Address` class.</span></span> <span data-ttu-id="8f664-109">Klasa `Person` będzie zawierać właściwość, która jest wpisana jako `Address`.</span><span class="sxs-lookup"><span data-stu-id="8f664-109">The `Person` class will contain a property that's typed as `Address`.</span></span> <span data-ttu-id="8f664-110">Typ `Address` jest typu złożonego, co oznacza, że nie jest to jeden z wbudowanych typów, takich jak `int`, `string`lub `double`.</span><span class="sxs-lookup"><span data-stu-id="8f664-110">The `Address` type is a complex type, meaning it's not one of the built-in types like `int`, `string`, or `double`.</span></span> <span data-ttu-id="8f664-111">Zamiast tego ma kilka właściwości.</span><span class="sxs-lookup"><span data-stu-id="8f664-111">Instead, it has several properties.</span></span> <span data-ttu-id="8f664-112">Kod dla nowych klas wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="8f664-112">The code for the new classes looks like this:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample1.cs)]

<span data-ttu-id="8f664-113">Na kontrolerze `Movie` Dodaj następującą akcję `PersonDetail`, aby wyświetlić wystąpienie osoby:</span><span class="sxs-lookup"><span data-stu-id="8f664-113">In the `Movie` controller, add the following `PersonDetail` action to display a person instance:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample2.cs)]

<span data-ttu-id="8f664-114">Następnie Dodaj następujący kod do kontrolera `Movie`, aby wypełnić model `Person` niektórymi przykładowymi danymi:</span><span class="sxs-lookup"><span data-stu-id="8f664-114">Then add the following code to the `Movie` controller to populate the `Person` model with some sample data:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample3.cs)]

<span data-ttu-id="8f664-115">Otwórz plik *Views\Movies\PersonDetail.cshtml* i Dodaj następujący znacznik dla widoku `PersonDetail`.</span><span class="sxs-lookup"><span data-stu-id="8f664-115">Open the *Views\Movies\PersonDetail.cshtml* file and add the following markup for the `PersonDetail` view.</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample4.cshtml)]

<span data-ttu-id="8f664-116">Naciśnij klawisze CTRL + F5, aby uruchomić aplikację, a następnie przejdź do *filmów/PersonDetail*.</span><span class="sxs-lookup"><span data-stu-id="8f664-116">Press Ctrl+F5 to run the application and navigate to *Movies/PersonDetail*.</span></span>

<span data-ttu-id="8f664-117">Widok `PersonDetail` nie zawiera `Address` typu złożonego, co można zobaczyć na tym zrzucie ekranu.</span><span class="sxs-lookup"><span data-stu-id="8f664-117">The `PersonDetail` view doesn't contain the `Address` complex type, as you can see in this screenshot.</span></span> <span data-ttu-id="8f664-118">(Żaden adres nie jest wyświetlany).</span><span class="sxs-lookup"><span data-stu-id="8f664-118">(No address is shown.)</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image1.png)

<span data-ttu-id="8f664-119">Dane modelu `Address` nie są wyświetlane, ponieważ jest typem złożonym.</span><span class="sxs-lookup"><span data-stu-id="8f664-119">The `Address` model data is not displayed because it's a complex type.</span></span> <span data-ttu-id="8f664-120">Aby wyświetlić informacje o adresie, ponownie otwórz plik *Views\Movies\PersonDetail.cshtml* i Dodaj następujący znacznik.</span><span class="sxs-lookup"><span data-stu-id="8f664-120">To display the address information, open the *Views\Movies\PersonDetail.cshtml* file again and add the following markup.</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample5.cshtml)]

<span data-ttu-id="8f664-121">Kompletny znacznik dla `PersonDetail` teraz będzie wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="8f664-121">The complete markup for the `PersonDetail` now view looks like this:</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample6.cshtml)]

<span data-ttu-id="8f664-122">Uruchom aplikację ponownie i Wyświetl widok `PersonDetail`.</span><span class="sxs-lookup"><span data-stu-id="8f664-122">Run the application again and display the `PersonDetail` view.</span></span> <span data-ttu-id="8f664-123">Informacje o adresie są teraz wyświetlane:</span><span class="sxs-lookup"><span data-stu-id="8f664-123">The address information is now displayed:</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image2.png)

### <a name="creating-a-template-for-a-complex-type"></a><span data-ttu-id="8f664-124">Tworzenie szablonu dla typu złożonego</span><span class="sxs-lookup"><span data-stu-id="8f664-124">Creating a Template for a Complex Type</span></span>

<span data-ttu-id="8f664-125">W tej sekcji utworzysz szablon, który będzie używany do renderowania `Address` typu złożonego.</span><span class="sxs-lookup"><span data-stu-id="8f664-125">In this section you'll create a template that will be used to render the `Address` complex type.</span></span> <span data-ttu-id="8f664-126">Podczas tworzenia szablonu dla typu `Address`, ASP.NET MVC może automatycznie używać go do formatowania modelu adresów w dowolnym miejscu w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8f664-126">When you create a template for the `Address` type, ASP.NET MVC can automatically use it to format an address model anywhere in the application.</span></span> <span data-ttu-id="8f664-127">Dzięki temu można kontrolować renderowanie typu `Address` z tylko jednego miejsca w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8f664-127">This gives you a way to control the rendering of the `Address` type from just one place in the application.</span></span>

<span data-ttu-id="8f664-128">W folderze *Views\Shared\DisplayTemplates* Utwórz silnie nazwany widok częściowy o nazwie **Address**:</span><span class="sxs-lookup"><span data-stu-id="8f664-128">In the *Views\Shared\DisplayTemplates* folder, create a strongly typed partial view named **Address**:</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image3.png)

<span data-ttu-id="8f664-129">Kliknij przycisk **Dodaj**, a następnie otwórz nowy plik *Views\Shared\DisplayTemplates\Address.cshtml* .</span><span class="sxs-lookup"><span data-stu-id="8f664-129">Click **Add**, and then open the new *Views\Shared\DisplayTemplates\Address.cshtml* file.</span></span> <span data-ttu-id="8f664-130">Nowy widok zawiera następujące wygenerowane znaczniki:</span><span class="sxs-lookup"><span data-stu-id="8f664-130">The new view contains the following generated markup:</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample7.cshtml)]

<span data-ttu-id="8f664-131">Uruchom aplikację i Wyświetl widok `PersonDetail`.</span><span class="sxs-lookup"><span data-stu-id="8f664-131">Run the application and display the `PersonDetail` view.</span></span> <span data-ttu-id="8f664-132">Tym razem szablon `Address`, który właśnie został utworzony, jest używany do wyświetlania `Address` typu złożonego, tak aby ekran wyglądał następująco:</span><span class="sxs-lookup"><span data-stu-id="8f664-132">This time, the `Address` template that you just created is used to display the `Address` complex type, so the display looks like the following:</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image4.png)

### <a name="summary-ways-to-specify-the-model-display-format-and-template"></a><span data-ttu-id="8f664-133">Podsumowanie: sposoby określania formatu i szablonu wyświetlania modelu</span><span class="sxs-lookup"><span data-stu-id="8f664-133">Summary: Ways to Specify the Model Display Format and Template</span></span>

<span data-ttu-id="8f664-134">Widzisz, że możesz określić format lub szablon właściwości modelu, wykonując następujące podejścia:</span><span class="sxs-lookup"><span data-stu-id="8f664-134">You've seen that you can specify the format or template for a model property by using the following approaches:</span></span>

- <span data-ttu-id="8f664-135">Stosowanie atrybutu `DisplayFormat` do właściwości w modelu.</span><span class="sxs-lookup"><span data-stu-id="8f664-135">Applying the `DisplayFormat` attribute to a property in the model.</span></span> <span data-ttu-id="8f664-136">Na przykład poniższy kod powoduje wyświetlenie daty bez czasu:</span><span class="sxs-lookup"><span data-stu-id="8f664-136">For example, the following code causes the date to be displayed without the time:</span></span>

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample8.cs)]
- <span data-ttu-id="8f664-137">Stosowanie atrybutu [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) do właściwości w modelu i Określanie typu danych.</span><span class="sxs-lookup"><span data-stu-id="8f664-137">Applying a [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) attribute to a property in the model and specifying the data type.</span></span> <span data-ttu-id="8f664-138">Na przykład poniższy kod powoduje wyświetlenie daty bez czasu.</span><span class="sxs-lookup"><span data-stu-id="8f664-138">For example, the following code causes the date to be displayed without the time.</span></span>

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample9.cs)]

    <span data-ttu-id="8f664-139">Jeśli aplikacja zawiera szablon *Date. cshtml* w folderze *Views\Shared\DisplayTemplates* lub folderze *Views\Movies\DisplayTemplates* , ten szablon zostanie użyty do renderowania właściwości `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="8f664-139">If the application contains a *date.cshtml* template in the *Views\Shared\DisplayTemplates* folder or the *Views\Movies\DisplayTemplates* folder, that template will be used to render the `DateTime` property.</span></span> <span data-ttu-id="8f664-140">W przeciwnym razie wbudowany system ASP.NET tworzenia szablonów wyświetla właściwość jako datę.</span><span class="sxs-lookup"><span data-stu-id="8f664-140">Otherwise the built-in ASP.NET templating system displays the property as a date.</span></span>
- <span data-ttu-id="8f664-141">Tworzenie szablonu wyświetlania w folderze *Views\Shared\DisplayTemplates* lub folderze *Views\Movies\DisplayTemplates* , którego nazwa pasuje do typu danych, który chcesz sformatować.</span><span class="sxs-lookup"><span data-stu-id="8f664-141">Creating a display template in the *Views\Shared\DisplayTemplates* folder or the *Views\Movies\DisplayTemplates* folder whose name matches the data type that you want to format.</span></span> <span data-ttu-id="8f664-142">Załóżmy na przykład, że *Views\Shared\DisplayTemplates\DateTime.cshtml* był używany do renderowania właściwości `DateTime` w modelu, bez dodawania atrybutu do modelu i bez dodawania znaczników do widoków.</span><span class="sxs-lookup"><span data-stu-id="8f664-142">For example, you saw that the *Views\Shared\DisplayTemplates\DateTime.cshtml* was used to render `DateTime` properties in a model, without adding an attribute to the model and without adding any markup to views.</span></span>
- <span data-ttu-id="8f664-143">Przy użyciu atrybutu [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) w modelu, aby określić szablon do wyświetlania właściwości model.</span><span class="sxs-lookup"><span data-stu-id="8f664-143">Using the [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) attribute on the model to specify the template to display the model property.</span></span>
- <span data-ttu-id="8f664-144">Jawne dodanie nazwy szablonu wyświetlania do wywołania [HTML. DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) w widoku.</span><span class="sxs-lookup"><span data-stu-id="8f664-144">Explicitly adding the display template name to the [Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) call in a view.</span></span>

<span data-ttu-id="8f664-145">Używane podejście zależy od tego, co należy zrobić w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8f664-145">The approach you use depends on what you need to do in your application.</span></span> <span data-ttu-id="8f664-146">Te podejścia nie są rzadko spotykane, aby uzyskać dokładnie wymagany rodzaj formatowania.</span><span class="sxs-lookup"><span data-stu-id="8f664-146">It's not uncommon to mix these approaches to get exactly the kind of formatting that you need.</span></span>

<span data-ttu-id="8f664-147">W następnej sekcji nastąpi przełączenie na bit i przejście przez dostosowanie sposobu wyświetlania danych w celu dostosowania sposobu wprowadzania.</span><span class="sxs-lookup"><span data-stu-id="8f664-147">In the next section, you'll switch gears a bit and move from customizing how data is displayed to customizing how it's entered.</span></span> <span data-ttu-id="8f664-148">Przełączymy się z DatePicker jQuery do widoków edycji w aplikacji, aby zapewnić sprawny sposób określania dat.</span><span class="sxs-lookup"><span data-stu-id="8f664-148">You'll hook up the jQuery datepicker to the edit views in the application in order to provide a slick way to specify dates.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="8f664-149">[Poprzednie](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
> [dalej](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="8f664-149">[Previous](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
[Next](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4.md)</span></span>

---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
title: Dodawanie walidacji do modelu | Microsoft Docs
author: shanselman
description: Jest to samouczek początkującego, który wprowadza podstawy ASP.NET MVC. Utwórz prostą aplikację sieci Web, która odczytuje i zapisuje dane w bazie danych.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: aa7b3e8e-e23d-49f1-b160-f99a7f2982bd
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
msc.type: authoredcontent
ms.openlocfilehash: 9403be574324c34edf93bef1e0e4fd7ba68a3a9d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78543646"
---
# <a name="adding-validation-to-the-model"></a><span data-ttu-id="1e6c8-104">Dodawanie walidacji do modelu</span><span class="sxs-lookup"><span data-stu-id="1e6c8-104">Adding Validation to the Model</span></span>

<span data-ttu-id="1e6c8-105">przez [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="1e6c8-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="1e6c8-106">Jest to samouczek początkującego, który wprowadza podstawy ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="1e6c8-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="1e6c8-107">Utworzysz prostą aplikację sieci Web, która odczytuje i zapisuje dane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="1e6c8-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="1e6c8-108">Odwiedź [centrum learning ASP.NET MVC](../../../index.md) , aby znaleźć inne samouczki i przykłady MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="1e6c8-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>

<span data-ttu-id="1e6c8-109">W tej sekcji zamierzamy wdrożyć pomoc techniczną, aby umożliwić weryfikację danych wejściowych w naszej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1e6c8-109">In this section we are going to implement the support necessary to enable input validation within our application.</span></span> <span data-ttu-id="1e6c8-110">Upewnij się, że zawartość bazy danych jest zawsze poprawna, i zapewnij użytkownikom końcowym pomocne komunikaty o błędach, a następnie wprowadź nieprawidłowe dane filmu.</span><span class="sxs-lookup"><span data-stu-id="1e6c8-110">We'll ensure that our database content is always correct, and provide helpful error messages to end users when they try and enter Movie data which is not valid.</span></span> <span data-ttu-id="1e6c8-111">Zaczniemy od dodania niewielkiej logiki walidacji do klasy filmów.</span><span class="sxs-lookup"><span data-stu-id="1e6c8-111">We'll begin by adding a little validation logic to the Movie class.</span></span>

<span data-ttu-id="1e6c8-112">Kliknij prawym przyciskiem myszy folder model i wybierz polecenie Dodaj klasę.</span><span class="sxs-lookup"><span data-stu-id="1e6c8-112">Right click on the Model folder and select Add Class.</span></span> <span data-ttu-id="1e6c8-113">Nazwij film klasy.</span><span class="sxs-lookup"><span data-stu-id="1e6c8-113">Name your class Movie.</span></span>

<span data-ttu-id="1e6c8-114">Po utworzeniu modelu obiektu filmu wcześniej środowisko IDE utworzyło klasę filmu.</span><span class="sxs-lookup"><span data-stu-id="1e6c8-114">When we created the Movie Entity Model earlier, the IDE created a Movie class.</span></span> <span data-ttu-id="1e6c8-115">W rzeczywistości część klasy filmu może znajdować się w jednym pliku i części w innym.</span><span class="sxs-lookup"><span data-stu-id="1e6c8-115">In fact, part of the Movie class can be in one file and part in another.</span></span> <span data-ttu-id="1e6c8-116">Ta nazwa jest nazywana klasą częściową.</span><span class="sxs-lookup"><span data-stu-id="1e6c8-116">This is called a Partial Class.</span></span> <span data-ttu-id="1e6c8-117">Zajmiemy się rozszerzaniem klasy filmu z innego pliku.</span><span class="sxs-lookup"><span data-stu-id="1e6c8-117">We're going to extend the Movie class from another file.</span></span>

<span data-ttu-id="1e6c8-118">Utworzymy częściową klasę filmową, która wskazuje na "kolega klasy" z pewnymi atrybutami, które umożliwią wskazówkom walidacji w systemie.</span><span class="sxs-lookup"><span data-stu-id="1e6c8-118">We'll create a partial movie class that points to a "buddy class" with some attributes that will give validation hints to the system.</span></span> <span data-ttu-id="1e6c8-119">Oznaczę tytuł i cenę zgodnie z wymaganiami, a także zapewnią, że cena należy do określonego zakresu.</span><span class="sxs-lookup"><span data-stu-id="1e6c8-119">We'll mark the Title and Price as Required, and also insist that the Price be within a certain range.</span></span> <span data-ttu-id="1e6c8-120">Kliknij prawym przyciskiem myszy folder modele i wybierz polecenie Dodaj klasę.</span><span class="sxs-lookup"><span data-stu-id="1e6c8-120">Right click the Models folder and select Add Class.</span></span> <span data-ttu-id="1e6c8-121">Nazwij film klasy, a następnie kliknij przycisk OK.</span><span class="sxs-lookup"><span data-stu-id="1e6c8-121">Name your class Movie and Click the OK button.</span></span> <span data-ttu-id="1e6c8-122">Oto jak wygląda częściowa Klasa filmu.</span><span class="sxs-lookup"><span data-stu-id="1e6c8-122">Here's what our partial Movie class looks like.</span></span>

[!code-csharp[Main](getting-started-with-mvc-part7/samples/sample1.cs)]

<span data-ttu-id="1e6c8-123">Uruchom ponownie aplikację i spróbuj wprowadzić film o cenie powyżej 100.</span><span class="sxs-lookup"><span data-stu-id="1e6c8-123">Re-Run your application and try to enter a movie with a price over 100.</span></span> <span data-ttu-id="1e6c8-124">Po przesłaniu formularza wystąpi błąd.</span><span class="sxs-lookup"><span data-stu-id="1e6c8-124">You'll get an error after you've submitted the form.</span></span> <span data-ttu-id="1e6c8-125">Ten błąd jest przechwytywany po stronie serwera i występuje po opublikowaniu formularza.</span><span class="sxs-lookup"><span data-stu-id="1e6c8-125">The error is caught on the server side and occurs after the Form is POSTed.</span></span> <span data-ttu-id="1e6c8-126">Zwróć uwagę, jak wbudowane pomocniki HTML ASP.NET MVC były wystarczająco inteligentne, aby wyświetlić komunikat o błędzie i zachować wartości dla nas w elementach TextBox:</span><span class="sxs-lookup"><span data-stu-id="1e6c8-126">Notice how ASP.NET MVC's built-in HTML helpers were smart enough to display the error message and maintain the values for us within the textbox elements:</span></span>

<span data-ttu-id="1e6c8-127">[![CreateMovieWithValidation](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="1e6c8-127">[![CreateMovieWithValidation](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)</span></span>

<span data-ttu-id="1e6c8-128">Jest to doskonałe rozwiązanie, ale warto poinstruować użytkownika po stronie klienta natychmiast, zanim serwer zostanie uwzględniony.</span><span class="sxs-lookup"><span data-stu-id="1e6c8-128">This works great, but it'd be nice if we could tell the user on the client-side, immediately, before the server gets involved.</span></span>

<span data-ttu-id="1e6c8-129">Włączmy kilka weryfikacji po stronie klienta przy użyciu języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="1e6c8-129">Let's enable some client-side validation with JavaScript.</span></span>

## <a name="adding-client-side-validation"></a><span data-ttu-id="1e6c8-130">Dodawanie weryfikacji po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="1e6c8-130">Adding Client-Side Validation</span></span>

<span data-ttu-id="1e6c8-131">Ponieważ Klasa filmu ma już pewne atrybuty walidacji, wystarczy dodać kilka plików JavaScript do naszego szablonu Create. aspx, a następnie dodać wiersz kodu, aby włączyć weryfikację po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="1e6c8-131">Since our Movie class already has some validation attributes, we'll just need to add a few JavaScript files to our Create.aspx View template and add a line of code to enable client-side validation to take place.</span></span>

<span data-ttu-id="1e6c8-132">Z poziomu programu pliku VWD przejdź do naszego folderu Viewss/Movie, a następnie otwórz pozycję Create. aspx.</span><span class="sxs-lookup"><span data-stu-id="1e6c8-132">From within VWD go our Views/Movie folder and open up Create.aspx.</span></span>

<span data-ttu-id="1e6c8-133">Otwórz folder skryptów w Eksplorator rozwiązań i przeciągnij następujące trzy skrypty do wewnątrz tagu &lt;&gt;.</span><span class="sxs-lookup"><span data-stu-id="1e6c8-133">Open up the Scripts folder in the Solution Explorer and drag the following three scripts to within the &lt;head&gt; tag.</span></span>

- <span data-ttu-id="1e6c8-134">MicrosoftAjax.js</span><span class="sxs-lookup"><span data-stu-id="1e6c8-134">MicrosoftAjax.js</span></span>
- <span data-ttu-id="1e6c8-135">MicrosoftMvcValidation.js</span><span class="sxs-lookup"><span data-stu-id="1e6c8-135">MicrosoftMvcValidation.js</span></span>

<span data-ttu-id="1e6c8-136">Te pliki skryptów mają być wyświetlane w tej kolejności.</span><span class="sxs-lookup"><span data-stu-id="1e6c8-136">You want these script files to appear in this order.</span></span>

[!code-html[Main](getting-started-with-mvc-part7/samples/sample2.html)]

<span data-ttu-id="1e6c8-137">Ponadto Dodaj ten pojedynczy wiersz powyżej pliku HTML. BeginForm:</span><span class="sxs-lookup"><span data-stu-id="1e6c8-137">Also, add this single line above the Html.BeginForm:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part7/samples/sample3.aspx)]

<span data-ttu-id="1e6c8-138">Oto kod wyświetlany w środowisku IDE.</span><span class="sxs-lookup"><span data-stu-id="1e6c8-138">Here's the code shown within the IDE.</span></span>

<span data-ttu-id="1e6c8-139">[![filmów — Microsoft Visual Web Developer 2010 Express (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="1e6c8-139">[![Movies - Microsoft Visual Web Developer 2010 Express (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)</span></span>

<span data-ttu-id="1e6c8-140">Uruchom aplikację i odwiedź witrynę/Movies/Create ponownie, a następnie kliknij pozycję Utwórz bez wprowadzania żadnych danych.</span><span class="sxs-lookup"><span data-stu-id="1e6c8-140">Run your application and visit /Movies/Create again, and click Create without entering any data.</span></span> <span data-ttu-id="1e6c8-141">Komunikaty o błędach pojawiają się natychmiast bez strony Flash, która jest skojarzona z wysyłaniem danych z powrotem do serwera programu.</span><span class="sxs-lookup"><span data-stu-id="1e6c8-141">The error messages appear immediately without the page flash that we associate with sending data all the way back to the server.</span></span> <span data-ttu-id="1e6c8-142">Jest to spowodowane tym, że ASP.NET MVC sprawdza teraz dane wejściowe na kliencie (przy użyciu języka JavaScript) i na serwerze.</span><span class="sxs-lookup"><span data-stu-id="1e6c8-142">This is because ASP.NET MVC is now validating the input on both the client (using JavaScript) and on the server.</span></span>

<span data-ttu-id="1e6c8-143">[![tworzenie — Windows Internet Explorer](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="1e6c8-143">[![Create - Windows Internet Explorer](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)</span></span>

<span data-ttu-id="1e6c8-144">Wygląda to dobrze!</span><span class="sxs-lookup"><span data-stu-id="1e6c8-144">This is looking good!</span></span> <span data-ttu-id="1e6c8-145">Dodajmy teraz jedną dodatkową kolumnę do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="1e6c8-145">Let's now add one additional column to the database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="1e6c8-146">[Poprzednie](getting-started-with-mvc-part6.md)
> [dalej](getting-started-with-mvc-part8.md)</span><span class="sxs-lookup"><span data-stu-id="1e6c8-146">[Previous](getting-started-with-mvc-part6.md)
[Next](getting-started-with-mvc-part8.md)</span></span>

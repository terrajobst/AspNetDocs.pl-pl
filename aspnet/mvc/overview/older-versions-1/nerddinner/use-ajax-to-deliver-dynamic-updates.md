---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
title: Korzystanie z technologii AJAX w celu dostarczania aktualizacji dynamicznych | Microsoft Docs
author: microsoft
description: Krok 10 implementuje obsługę zalogowanych użytkowników w celu założenia swojego zainteresowania na obiad przy użyciu opartego na technologii AJAX podejścia zintegrowanego w ramach szczegółów obiadu...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 18700815-8e6c-4489-91af-7ea9dab6529e
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
msc.type: authoredcontent
ms.openlocfilehash: 3edc02fec546609505b5e085440fa684abe7acd0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78600850"
---
# <a name="use-ajax-to-deliver-dynamic-updates"></a><span data-ttu-id="d581a-103">Korzystanie z technologii AJAX w celu dostarczania aktualizacji dynamicznych</span><span class="sxs-lookup"><span data-stu-id="d581a-103">Use AJAX to Deliver Dynamic Updates</span></span>

<span data-ttu-id="d581a-104">przez [firmę Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="d581a-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="d581a-105">Pobierz plik PDF</span><span class="sxs-lookup"><span data-stu-id="d581a-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="d581a-106">Jest to krok 10 bezpłatnego [samouczka dotyczącego aplikacji "NerdDinner"](introducing-the-nerddinner-tutorial.md) , który zawiera instrukcje tworzenia niewielkiej, ale kompletnej aplikacji sieci Web przy użyciu ASP.NET MVC 1.</span><span class="sxs-lookup"><span data-stu-id="d581a-106">This is step 10 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="d581a-107">Krok 10 implementuje obsługę zalogowanych użytkowników, aby dowiedzieć się, jak wziąć udział w obiadie przy użyciu podejścia opartego na technologii AJAX zintegrowanego na stronie szczegółów obiadu.</span><span class="sxs-lookup"><span data-stu-id="d581a-107">Step 10 implements support for logged-in users to RSVP their interest in attending a dinner, using an Ajax-based approach integrated within the dinner details page.</span></span>
> 
> <span data-ttu-id="d581a-108">Jeśli używasz ASP.NET MVC 3, zalecamy użycie [wprowadzenie ze samouczkami ze sklepu MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) lub [MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) .</span><span class="sxs-lookup"><span data-stu-id="d581a-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>

## <a name="nerddinner-step-10-ajax-enabling-rsvps-accepts"></a><span data-ttu-id="d581a-109">NerdDinner krok 10: akceptowane jest włączenie funkcji AJAX</span><span class="sxs-lookup"><span data-stu-id="d581a-109">NerdDinner Step 10: AJAX Enabling RSVPs Accepts</span></span>

<span data-ttu-id="d581a-110">Zaimplementujmy teraz obsługę zalogowanych użytkowników, aby dopuścić do wzięcia udziału w obiadie.</span><span class="sxs-lookup"><span data-stu-id="d581a-110">Let's now implement support for logged-in users to RSVP their interest in attending a dinner.</span></span> <span data-ttu-id="d581a-111">Włączmy to przy użyciu podejścia opartego na technologii AJAX zintegrowanego na stronie szczegółów obiadu.</span><span class="sxs-lookup"><span data-stu-id="d581a-111">We'll enable this using an AJAX-based approach integrated within the dinner details page.</span></span>

### <a name="indicating-whether-the-user-is-rsvpd"></a><span data-ttu-id="d581a-112">Wskazuje, czy użytkownik jest w usłudze RSVP</span><span class="sxs-lookup"><span data-stu-id="d581a-112">Indicating whether the user is RSVP'd</span></span>

<span data-ttu-id="d581a-113">Użytkownicy mogą odwiedzać adres URL */Dinners/Details/[ID*], aby wyświetlić szczegółowe informacje dotyczące konkretnego obiadu:</span><span class="sxs-lookup"><span data-stu-id="d581a-113">Users can visit the */Dinners/Details/[id*] URL to see details about a particular dinner:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image1.png)

<span data-ttu-id="d581a-114">Metoda akcja szczegóły () jest zaimplementowana tak, jak:</span><span class="sxs-lookup"><span data-stu-id="d581a-114">The Details() action method is implemented like so:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample1.cs)]

<span data-ttu-id="d581a-115">Pierwszym krokiem w celu zaimplementowania obsługi protokołu RSVP będzie dodanie metody pomocnika "IsUserRegistered (username)" do naszego obiektu obiadu (w ramach klasy Dinner.cs częściowej utworzonej wcześniej).</span><span class="sxs-lookup"><span data-stu-id="d581a-115">Our first step to implement RSVP support will be to add an "IsUserRegistered(username)" helper method to our Dinner object (within the Dinner.cs partial class we built earlier).</span></span> <span data-ttu-id="d581a-116">Ta metoda pomocnika zwraca wartość true lub false, w zależności od tego, czy użytkownik jest obecnie RSVP w przypadku obiadu:</span><span class="sxs-lookup"><span data-stu-id="d581a-116">This helper method returns true or false depending on whether the user is currently RSVP'd for the Dinner:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample2.cs)]

<span data-ttu-id="d581a-117">Następnie można dodać poniższy kod do naszego szablonu widoku szczegółów. aspx, aby wyświetlić odpowiedni komunikat wskazujący, czy użytkownik jest zarejestrowany, czy nie dla zdarzenia:</span><span class="sxs-lookup"><span data-stu-id="d581a-117">We can then add the following code to our Details.aspx view template to display an appropriate message indicating whether the user is registered or not for the event:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample3.html)]

<span data-ttu-id="d581a-118">A teraz po zarejestrowaniu się obiadu dla użytkowników zostanie wyświetlony następujący komunikat:</span><span class="sxs-lookup"><span data-stu-id="d581a-118">And now when a user visits a Dinner they are registered for they'll see this message:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image2.png)

<span data-ttu-id="d581a-119">Po odwiedzeniu obiadu nie są one zarejestrowane, zobaczysz następujący komunikat:</span><span class="sxs-lookup"><span data-stu-id="d581a-119">And when they visit a Dinner they are not registered for they'll see the below message:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image3.png)

### <a name="implementing-the-register-action-method"></a><span data-ttu-id="d581a-120">Implementowanie metody akcji Register</span><span class="sxs-lookup"><span data-stu-id="d581a-120">Implementing the Register Action Method</span></span>

<span data-ttu-id="d581a-121">Dodajmy teraz funkcje niezbędne do umożliwienia użytkownikom zawieszania się na obiad ze strony szczegółów.</span><span class="sxs-lookup"><span data-stu-id="d581a-121">Let's now add the functionality necessary to enable users to RSVP for a dinner from the details page.</span></span>

<span data-ttu-id="d581a-122">Aby zaimplementować ten element, utworzymy nową klasę "RSVPController", klikając prawym przyciskiem myszy katalog \Controllers i wybierając polecenie menu Dodaj kontroler&gt;.</span><span class="sxs-lookup"><span data-stu-id="d581a-122">To implement this, we'll create a new "RSVPController" class by right-clicking on the \Controllers directory and choosing the Add-&gt;Controller menu command.</span></span>

<span data-ttu-id="d581a-123">Zaimplementujmy metodę akcji "Register" w ramach nowej klasy RSVPController, która przyjmuje identyfikator dla obiadu jako argument, pobiera odpowiedni obiekt obiadu i sprawdza, czy zalogowany użytkownik znajduje się obecnie na liście użytkowników, którzy zostali zarejestrowani. nie dodaje obiektu RSVP dla nich:</span><span class="sxs-lookup"><span data-stu-id="d581a-123">We'll implement a "Register" action method within the new RSVPController class that takes an id for a Dinner as an argument, retrieves the appropriate Dinner object, checks to see if the logged-in user is currently in the list of users who have registered for it, and if not adds an RSVP object for them:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample4.cs)]

<span data-ttu-id="d581a-124">Zwróć uwagę na to, jak zwracamy prosty ciąg jako dane wyjściowe metody akcji.</span><span class="sxs-lookup"><span data-stu-id="d581a-124">Notice above how we are returning a simple string as the output of the action method.</span></span> <span data-ttu-id="d581a-125">Ten komunikat został osadzony w szablonie widoku — ale ponieważ jest to małe użycie metody pomocnika Content () w klasie podstawowej kontrolera i zwrócenie komunikatu ciągu jak powyżej.</span><span class="sxs-lookup"><span data-stu-id="d581a-125">We could have embedded this message within a view template – but since it is so small we'll just use the Content() helper method on the controller base class and return a string message like above.</span></span>

### <a name="calling-the-rsvpforevent-action-method-using-ajax"></a><span data-ttu-id="d581a-126">Wywoływanie metody akcji RSVPForEvent przy użyciu technologii AJAX</span><span class="sxs-lookup"><span data-stu-id="d581a-126">Calling the RSVPForEvent Action Method using AJAX</span></span>

<span data-ttu-id="d581a-127">Użyjemy technologii AJAX do wywołania metody rejestracji z naszego widoku szczegółów.</span><span class="sxs-lookup"><span data-stu-id="d581a-127">We'll use AJAX to invoke the Register action method from our Details view.</span></span> <span data-ttu-id="d581a-128">Zaimplementowanie tego jest bardzo proste.</span><span class="sxs-lookup"><span data-stu-id="d581a-128">Implementing this is pretty easy.</span></span> <span data-ttu-id="d581a-129">Najpierw dodamy dwa odwołania do biblioteki skryptów:</span><span class="sxs-lookup"><span data-stu-id="d581a-129">First we'll add two script library references:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample5.html)]

<span data-ttu-id="d581a-130">Pierwsza Biblioteka odwołuje się do podstawowej biblioteki skryptów po stronie klienta ASP.NET AJAX.</span><span class="sxs-lookup"><span data-stu-id="d581a-130">The first library references the core ASP.NET AJAX client-side script library.</span></span> <span data-ttu-id="d581a-131">Ten plik ma około 24k (skompresowany) i zawiera podstawowe funkcje AJAX po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="d581a-131">This file is approximately 24k in size (compressed) and contains core client-side AJAX functionality.</span></span> <span data-ttu-id="d581a-132">Druga biblioteka zawiera funkcje narzędziowe, które integrują się z wbudowanymi metodami pomocnika AJAX ASP.NET MVC (których użyjemy wkrótce).</span><span class="sxs-lookup"><span data-stu-id="d581a-132">The second library contains utility functions that integrate with ASP.NET MVC's built-in AJAX helper methods (which we'll use shortly).</span></span>

<span data-ttu-id="d581a-133">Następnie możemy zaktualizować kod widoku, który dodałeś wcześniej, tak aby zamiast wycofać komunikat "nie zarejestrowano dla tego zdarzenia". zamiast tego zostanie wyświetlona próba wywołania AJAX, które wywołuje nasze metody akcji RSVPForEvent na naszym kontrolerze RSVP i RSVP użytkownika:</span><span class="sxs-lookup"><span data-stu-id="d581a-133">We can then update the view template code we added earlier so that instead of outputting a "You are not registered for this event" message, we instead render a link that when pushed performs an AJAX call that invokes our RSVPForEvent action method on our RSVP controller and RSVPs the user:</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample6.aspx)]

<span data-ttu-id="d581a-134">Użyta powyżej metoda pomocnika AJAX. ActionLink () jest wbudowana w ASP.NET MVC i jest podobna do metody pomocnika html. ActionLink (), z wyjątkiem sytuacji, gdy nie wykonuje się standardowej nawigacji, wykonuje wywołanie AJAX do metody akcji po kliknięciu łącza.</span><span class="sxs-lookup"><span data-stu-id="d581a-134">The Ajax.ActionLink() helper method used above is built-into ASP.NET MVC and is similar to the Html.ActionLink() helper method except that instead of performing a standard navigation it makes an AJAX call to the action method when the link is clicked.</span></span> <span data-ttu-id="d581a-135">Powyżej wywołujemy metodę akcji "Register" na kontrolerze "RSVP" i przekazując DinnerID jako parametr "ID".</span><span class="sxs-lookup"><span data-stu-id="d581a-135">Above we are calling the "Register" action method on the "RSVP" controller and passing the DinnerID as the "id" parameter to it.</span></span> <span data-ttu-id="d581a-136">Końcowy parametr AjaxOptions, który przekazujemy, wskazuje, że chcemy pobrać zawartość zwróconą z metody akcji i zaktualizować element HTML &lt;DIV&gt; na stronie, której identyfikator to "rsvpmsg".</span><span class="sxs-lookup"><span data-stu-id="d581a-136">The final AjaxOptions parameter we are passing indicates that we want to take the content returned from the action method and update the HTML &lt;div&gt; element on the page whose id is "rsvpmsg".</span></span>

<span data-ttu-id="d581a-137">A teraz po przejściu użytkownika do obiadu, którego nie zarejestrowano, zobaczysz link do usługi RSVP dla tego:</span><span class="sxs-lookup"><span data-stu-id="d581a-137">And now when a user browses to a dinner they aren't registered for yet they'll see a link to RSVP for it:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image4.png)

<span data-ttu-id="d581a-138">Jeśli klikniesz link "RSVP dla tego zdarzenia", nastąpi wywołanie AJAX do metody Register na kontrolerze RSVP, a po jej zakończeniu zobaczysz zaktualizowany komunikat podobny do poniższego:</span><span class="sxs-lookup"><span data-stu-id="d581a-138">If they click the "RSVP for this event" link they'll make an AJAX call to the Register action method on the RSVP controller, and when it completes they'll see an updated message like below:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image5.png)

<span data-ttu-id="d581a-139">Przepustowość sieci i ruch związany z wykonywaniem tego wywołania AJAX jest naprawdę lekki.</span><span class="sxs-lookup"><span data-stu-id="d581a-139">The network bandwidth and traffic involved when making this AJAX call is really lightweight.</span></span> <span data-ttu-id="d581a-140">Gdy użytkownik kliknie łącze "RSVP dla tego zdarzenia", na adres URL */Dinners/Register/1* , który wygląda jak poniżej, zostanie wysłane małe żądanie HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="d581a-140">When the user clicks on the "RSVP for this event" link, a small HTTP POST network request is made to the */Dinners/Register/1* URL that looks like below on the wire:</span></span>

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample7.cmd)]

<span data-ttu-id="d581a-141">A odpowiedź z naszej metody działania Register jest po prostu:</span><span class="sxs-lookup"><span data-stu-id="d581a-141">And the response from our Register action method is simply:</span></span>

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample8.cmd)]

<span data-ttu-id="d581a-142">To lekkie wywołanie jest szybkie i będzie działało nawet za pośrednictwem wolnej sieci.</span><span class="sxs-lookup"><span data-stu-id="d581a-142">This lightweight call is fast and will work even over a slow network.</span></span>

### <a name="adding-a-jquery-animation"></a><span data-ttu-id="d581a-143">Dodawanie animacji jQuery</span><span class="sxs-lookup"><span data-stu-id="d581a-143">Adding a jQuery Animation</span></span>

<span data-ttu-id="d581a-144">Wdrożone funkcje AJAX działają dobrze i szybko.</span><span class="sxs-lookup"><span data-stu-id="d581a-144">The AJAX functionality we implemented works well and fast.</span></span> <span data-ttu-id="d581a-145">Czasami może to być spowodowane tym, że użytkownik może nie zauważyć, że łącze RSVP zostało zastąpione nowym tekstem.</span><span class="sxs-lookup"><span data-stu-id="d581a-145">Sometimes it can happen so fast, though, that a user might not notice that the RSVP link has been replaced with new text.</span></span> <span data-ttu-id="d581a-146">Aby wynik był nieco bardziej oczywisty, możemy dodać prostą animację, aby zwrócić uwagę na komunikat dotyczący aktualizacji.</span><span class="sxs-lookup"><span data-stu-id="d581a-146">To make the outcome a little more obvious we can add a simple animation to draw attention to the update message.</span></span>

<span data-ttu-id="d581a-147">Domyślny szablon projektu ASP.NET MVC obejmuje jQuery — doskonałe (i bardzo popularne) biblioteki języka JavaScript Open Source, które są również obsługiwane przez firmę Microsoft.</span><span class="sxs-lookup"><span data-stu-id="d581a-147">The default ASP.NET MVC project template includes jQuery – an excellent (and very popular) open source JavaScript library that is also supported by Microsoft.</span></span> <span data-ttu-id="d581a-148">jQuery udostępnia wiele funkcji, w tym całkiem do wyboru język DOM HTML i bibliotekę efektów.</span><span class="sxs-lookup"><span data-stu-id="d581a-148">jQuery provides a number of features, including a nice HTML DOM selection and effects library.</span></span>

<span data-ttu-id="d581a-149">Aby użyć jQuery, najpierw dodamy do niego odwołanie do skryptu.</span><span class="sxs-lookup"><span data-stu-id="d581a-149">To use jQuery we'll first add a script reference to it.</span></span> <span data-ttu-id="d581a-150">Ze względu na to, że będziemy używać platformy jQuery w różnych miejscach w naszej witrynie, dodamy odwołanie do skryptu w naszej witrynie. główny plik strony głównej, tak aby wszystkie strony mogły go używać.</span><span class="sxs-lookup"><span data-stu-id="d581a-150">Because we are going to be using jQuery within a variety of places within our site, we'll add the script reference within our Site.master master page file so that all pages can use it.</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample9.html)]

<span data-ttu-id="d581a-151">*Porada: Upewnij się, że zainstalowano poprawkę IntelliSense języka JavaScript dla programu VS 2008 z dodatkiem SP1, która umożliwia zaawansowaną obsługę funkcji IntelliSense dla plików JavaScript (w tym jQuery). Można go pobrać z: http://tinyurl.com/vs2008javascripthotfix*</span><span class="sxs-lookup"><span data-stu-id="d581a-151">*Tip: make sure you have installed the JavaScript intellisense hotfix for VS 2008 SP1 that enables richer intellisense support for JavaScript files (including jQuery). You can download it from: http://tinyurl.com/vs2008javascripthotfix*</span></span>

<span data-ttu-id="d581a-152">Kod zapisany przy użyciu JQuery często używa globalnej metody JavaScript "$ ()", która pobiera jeden lub więcej elementów HTML przy użyciu selektora CSS.</span><span class="sxs-lookup"><span data-stu-id="d581a-152">Code written using JQuery often uses a global "$()" JavaScript method that retrieves one or more HTML elements using a CSS selector.</span></span> <span data-ttu-id="d581a-153">Na przykład *$ ("#rsvpmsg")* wybiera każdy element HTML o identyfikatorze rsvpmsg, podczas gdy *$ (". coś")* wybierze wszystkie elementy z nazwą klasy CSS "coś".</span><span class="sxs-lookup"><span data-stu-id="d581a-153">For example, *$("#rsvpmsg")* selects any HTML element with the id of rsvpmsg, while *$(".something")* would select all elements with the "something" CSS class name.</span></span> <span data-ttu-id="d581a-154">Możesz również napisać bardziej zaawansowane zapytania, takie jak "Zwróć wszystkie sprawdzone przyciski radiowe" przy użyciu zapytania selektora, takiego jak: *$ ("Input [@type= Radio] [@checked]")* .</span><span class="sxs-lookup"><span data-stu-id="d581a-154">You can also write more advanced queries like "return all of the checked radio buttons" using a selector query like: *$("input[@type=radio][@checked]")*.</span></span>

<span data-ttu-id="d581a-155">Po wybraniu elementów możesz wywoływać metody na nich w celu podjęcia działania, takich jak ukrywanie: *$ ("#rsvpmsg"). Hide ();*</span><span class="sxs-lookup"><span data-stu-id="d581a-155">Once you've selected elements, you can call methods on them to take action, like hiding them: *$("#rsvpmsg").hide();*</span></span>

<span data-ttu-id="d581a-156">W naszym scenariuszu RSVP zdefiniujemy prostą funkcję języka JavaScript o nazwie "AnimateRSVPMessage", która wybiera&gt; "rsvpmsg" &lt;DIV, i Animuj rozmiar zawartości tekstowej.</span><span class="sxs-lookup"><span data-stu-id="d581a-156">For our RSVP scenario, we'll define a simple JavaScript function named "AnimateRSVPMessage" that selects the "rsvpmsg" &lt;div&gt; and animates the size of its text content.</span></span> <span data-ttu-id="d581a-157">Poniższy kod uruchamia mały tekst, a następnie powoduje zwiększenie przez 400 milisekund czasu:</span><span class="sxs-lookup"><span data-stu-id="d581a-157">The below code starts the text small and then causes it to increase over a 400 milliseconds timeframe:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample10.html)]

<span data-ttu-id="d581a-158">Możemy następnie skorzystać z tej funkcji języka JavaScript, która będzie wywoływana po pomyślnym ukończeniu wywołania AJAX przez przekazanie jej nazwy do naszej metody pomocnika AJAX. ActionLink () (za pomocą właściwości zdarzenia AjaxOptions "onSuccess"):</span><span class="sxs-lookup"><span data-stu-id="d581a-158">We can then wire-up this JavaScript function to be called after our AJAX call successfully completes by passing its name to our Ajax.ActionLink() helper method (via the AjaxOptions "OnSuccess" event property):</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample11.aspx)]

<span data-ttu-id="d581a-159">A teraz, gdy zostanie kliknięte łącze "RSVP dla tego zdarzenia", a nasze wywołanie AJAX zostanie zakończone pomyślnie, komunikat zawartości wysłany z powrotem zostanie animowany i powiększony:</span><span class="sxs-lookup"><span data-stu-id="d581a-159">And now when the "RSVP for this event" link is clicked and our AJAX call completes successfully, the content message sent back will animate and grow large:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image6.png)

<span data-ttu-id="d581a-160">Oprócz podania zdarzenia "onSuccess" obiekt AjaxOptions uwidacznia zdarzenia onbegin, OnFailure i OnComplete, które można obsłużyć (wraz z różnymi innymi właściwościami i opcjami użytecznymi).</span><span class="sxs-lookup"><span data-stu-id="d581a-160">In addition to providing an "OnSuccess" event, the AjaxOptions object exposes OnBegin, OnFailure, and OnComplete events that you can handle (along with a variety of other properties and useful options).</span></span>

### <a name="cleanup---refactor-out-a-rsvp-partial-view"></a><span data-ttu-id="d581a-161">Czyszczenie — Refaktoryzacja widoku częściowego RSVP</span><span class="sxs-lookup"><span data-stu-id="d581a-161">Cleanup - Refactor out a RSVP Partial View</span></span>

<span data-ttu-id="d581a-162">Nasz szablon widoku szczegółów rozpoczyna się od dłuższego czasu, który jest nieco trudniejszy do zrozumienia.</span><span class="sxs-lookup"><span data-stu-id="d581a-162">Our details view template is starting to get a little long, which overtime will make it a little harder to understand.</span></span> <span data-ttu-id="d581a-163">Aby poprawić czytelność kodu, Zakończmy Tworzenie widoku częściowego — RSVPStatus. ascx — który hermetyzuje wszystkie kody widoku RSVP dla naszej strony szczegółów.</span><span class="sxs-lookup"><span data-stu-id="d581a-163">To help improve the code readability, let's finish up by creating a partial view – RSVPStatus.ascx – that encapsulate all of the RSVP view code for our Details page.</span></span>

<span data-ttu-id="d581a-164">Można to zrobić, klikając prawym przyciskiem myszy folder \Views\Dinners, a następnie wybierając polecenie menu Widok Dodaj&gt;.</span><span class="sxs-lookup"><span data-stu-id="d581a-164">We can do this by right-clicking on the \Views\Dinners folder and then choosing the Add-&gt;View menu command.</span></span> <span data-ttu-id="d581a-165">Będziemy korzystać z obiektu obiadu jako swojej jednoznacznie wpisanej ViewModel.</span><span class="sxs-lookup"><span data-stu-id="d581a-165">We'll have it take a Dinner object as its strongly-typed ViewModel.</span></span> <span data-ttu-id="d581a-166">Następnie możemy skopiować/wkleić zawartość RSVP z naszego widoku Szczegóły. aspx.</span><span class="sxs-lookup"><span data-stu-id="d581a-166">We can then copy/paste the RSVP content from our Details.aspx view into it.</span></span>

<span data-ttu-id="d581a-167">Po wykonaniu tej czynności utworzymy również inny widok częściowy — EditAndDeleteLinks. ascx — który hermetyzuje nasz kod widoku linku Edytuj i Usuń.</span><span class="sxs-lookup"><span data-stu-id="d581a-167">Once we've done that, let's also create another partial view – EditAndDeleteLinks.ascx - that encapsulates our Edit and Delete link view code.</span></span> <span data-ttu-id="d581a-168">Będziemy również korzystać z obiektu obiadu jako swojej jednoznacznie wpisanej ViewModel, a następnie skopiuj/wklej logikę Edytuj i Usuń z naszego widoku Szczegóły. aspx.</span><span class="sxs-lookup"><span data-stu-id="d581a-168">We'll also have it take a Dinner object as its strongly-typed ViewModel, and copy/paste the Edit and Delete logic from our Details.aspx view into it.</span></span>

<span data-ttu-id="d581a-169">Nasz szablon widoku szczegółów może następnie zawierać dwa wywołania metody html. RenderPartial () na dole:</span><span class="sxs-lookup"><span data-stu-id="d581a-169">Our details view template can then just include two Html.RenderPartial() method calls at the bottom:</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample12.aspx)]

<span data-ttu-id="d581a-170">Pozwala to na odczytywanie i konserwowanie oczyszczarki kodu.</span><span class="sxs-lookup"><span data-stu-id="d581a-170">This makes the code cleaner to read and maintain.</span></span>

### <a name="next-step"></a><span data-ttu-id="d581a-171">Następny krok</span><span class="sxs-lookup"><span data-stu-id="d581a-171">Next Step</span></span>

<span data-ttu-id="d581a-172">Teraz przyjrzyjmy się sposobom, w jaki możemy jeszcze bardziej wykorzystać technologię AJAX i dodać obsługę mapowania interaktywnego do naszej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d581a-172">Let's now look at how we can use AJAX even further and add interactive mapping support to our application.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d581a-173">[Poprzednie](secure-applications-using-authentication-and-authorization.md)
> [dalej](use-ajax-to-implement-mapping-scenarios.md)</span><span class="sxs-lookup"><span data-stu-id="d581a-173">[Previous](secure-applications-using-authentication-and-authorization.md)
[Next](use-ajax-to-implement-mapping-scenarios.md)</span></span>

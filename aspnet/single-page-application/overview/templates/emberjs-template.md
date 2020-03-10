---
uid: single-page-application/overview/templates/emberjs-template
title: Szablon EmberJS | Microsoft Docs
author: xqiu
description: Szablon EmberJS
ms.author: riande
ms.date: 01/30/2013
ms.assetid: 04d5f142-5f62-494a-b5ea-4f3d068d34cb
msc.legacyurl: /single-page-application/overview/templates/emberjs-template
msc.type: authoredcontent
ms.openlocfilehash: 1aefa46dd0841b1b06675409cc8a09f9a218d7ac
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78578506"
---
# <a name="emberjs-template"></a><span data-ttu-id="c2aed-103">Szablon EmberJS</span><span class="sxs-lookup"><span data-stu-id="c2aed-103">EmberJS template</span></span>

<span data-ttu-id="c2aed-104">Autor [Xinyang Qiu](https://github.com/xqiu)</span><span class="sxs-lookup"><span data-stu-id="c2aed-104">by [Xinyang Qiu](https://github.com/xqiu)</span></span>

> <span data-ttu-id="c2aed-105">Szablon EmberJS MVC jest pisany przez Nathana Totten, Thiago Santos i Xinyang Qiu.</span><span class="sxs-lookup"><span data-stu-id="c2aed-105">The EmberJS MVC Template is written by Nathan Totten, Thiago Santos, and Xinyang Qiu.</span></span>
> 
> [<span data-ttu-id="c2aed-106">Pobierz szablon EmberJS MVC</span><span class="sxs-lookup"><span data-stu-id="c2aed-106">Download the EmberJS MVC Template</span></span>](https://go.microsoft.com/fwlink/?LinkId=282647)

<span data-ttu-id="c2aed-107">Szablon SPA EmberJS został zaprojektowany, aby szybko rozpocząć tworzenie interaktywnych aplikacji sieci Web po stronie klienta przy użyciu EmberJS.</span><span class="sxs-lookup"><span data-stu-id="c2aed-107">The EmberJS SPA template is designed to get you started quickly building interactive client-side web apps using EmberJS.</span></span>

<span data-ttu-id="c2aed-108">"Aplikacja jednostronicowa" (SPA) to ogólny termin aplikacji sieci Web, który ładuje pojedynczą stronę HTML, a następnie automatycznie aktualizuje stronę, zamiast ładować nowe strony.</span><span class="sxs-lookup"><span data-stu-id="c2aed-108">"Single-page application" (SPA) is the general term for a web application that loads a single HTML page and then updates the page dynamically, instead of loading new pages.</span></span> <span data-ttu-id="c2aed-109">Po początkowym załadowaniu strony SPA komunikuje się z serwerem przez żądania AJAX.</span><span class="sxs-lookup"><span data-stu-id="c2aed-109">After the initial page load, the SPA talks with the server through AJAX requests.</span></span>

![](emberjs-template/_static/image1.png)

<span data-ttu-id="c2aed-110">Technologia AJAX nie ma nic nowego, ale dzisiaj istnieją struktury języka JavaScript, które ułatwiają tworzenie i konserwowanie dużej zaawansowanej aplikacji SPA.</span><span class="sxs-lookup"><span data-stu-id="c2aed-110">AJAX is nothing new, but today there are JavaScript frameworks that make it easier to build and maintain a large sophisticated SPA application.</span></span> <span data-ttu-id="c2aed-111">Ponadto kod HTML 5 i CSS3 ułatwiają tworzenie bogatych interfejsów użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c2aed-111">Also, HTML 5 and CSS3 are making it easier to create rich UIs.</span></span>

<span data-ttu-id="c2aed-112">Szablon SPA EmberJS używa biblioteki JavaScript [wpływ](http://emberjs.com/) do obsługi aktualizacji stron z żądań AJAX.</span><span class="sxs-lookup"><span data-stu-id="c2aed-112">The EmberJS SPA Template uses the [Ember](http://emberjs.com/) JavaScript library to handle page updates from AJAX requests.</span></span> <span data-ttu-id="c2aed-113">Wpływ. js używa powiązania danych do synchronizowania strony z najnowszymi danymi.</span><span class="sxs-lookup"><span data-stu-id="c2aed-113">Ember.js uses data binding to synchronize the page with the latest data.</span></span> <span data-ttu-id="c2aed-114">Dzięki temu nie trzeba pisać żadnego kodu, który przegląda dane JSON i aktualizuje DOM.</span><span class="sxs-lookup"><span data-stu-id="c2aed-114">That way, you don't have to write any of the code that walks through the JSON data and updates the DOM.</span></span> <span data-ttu-id="c2aed-115">Zamiast tego należy umieścić atrybuty deklaracyjne w kodzie HTML, które informują wpływ. js, jak przedstawić dane.</span><span class="sxs-lookup"><span data-stu-id="c2aed-115">Instead, you put declarative attributes in the HTML that tell Ember.js how to present the data.</span></span>

<span data-ttu-id="c2aed-116">Po stronie serwera szablon EmberJS jest niemal identyczny z [szablonem Spa KnockoutJS](../introduction/knockoutjs-template.md).</span><span class="sxs-lookup"><span data-stu-id="c2aed-116">On the server side, the EmberJS template is almost identical to the [KnockoutJS SPA template](../introduction/knockoutjs-template.md).</span></span> <span data-ttu-id="c2aed-117">Używa ASP.NET MVC do obsługi dokumentów HTML i ASP.NET interfejsu API sieci Web do obsługi żądań AJAX od klienta.</span><span class="sxs-lookup"><span data-stu-id="c2aed-117">It uses ASP.NET MVC to serve HTML documents, and ASP.NET Web API to handle AJAX requests from the client.</span></span> <span data-ttu-id="c2aed-118">Aby uzyskać więcej informacji na temat tych aspektów szablonu, zapoznaj się z dokumentacją [szablonu KnockoutJS](../introduction/knockoutjs-template.md) .</span><span class="sxs-lookup"><span data-stu-id="c2aed-118">For more information about those aspects of the template, refer to the [KnockoutJS template](../introduction/knockoutjs-template.md) documentation.</span></span> <span data-ttu-id="c2aed-119">W tym temacie omówiono różnice między szablonem odcinania i szablonem EmberJS.</span><span class="sxs-lookup"><span data-stu-id="c2aed-119">This topic focuses on the differences between the Knockout template and the EmberJS template.</span></span>

## <a name="create-an-emberjs-spa-template-project"></a><span data-ttu-id="c2aed-120">Utwórz projekt szablonu SPA EmberJS</span><span class="sxs-lookup"><span data-stu-id="c2aed-120">Create an EmberJS SPA Template Project</span></span>

<span data-ttu-id="c2aed-121">Pobierz i zainstaluj szablon, klikając przycisk Pobierz powyżej.</span><span class="sxs-lookup"><span data-stu-id="c2aed-121">Download and install the template by clicking the Download button above.</span></span> <span data-ttu-id="c2aed-122">Może być konieczne ponowne uruchomienie programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c2aed-122">You might need to restart Visual Studio.</span></span>

<span data-ttu-id="c2aed-123">W okienku **Szablony** wybierz pozycję **zainstalowane szablony** i rozwiń węzeł **Wizualizacja C#**  .</span><span class="sxs-lookup"><span data-stu-id="c2aed-123">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="c2aed-124">W **obszarze C#Wizualizacja** wybierz pozycję **Sieć Web**.</span><span class="sxs-lookup"><span data-stu-id="c2aed-124">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="c2aed-125">Na liście szablonów projektu wybierz pozycję **aplikacja sieci Web MVC 4 ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="c2aed-125">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="c2aed-126">Nazwij projekt, a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="c2aed-126">Name the project and click **OK**.</span></span>

![](emberjs-template/_static/image2.png)

<span data-ttu-id="c2aed-127">W kreatorze **nowego projektu** wybierz **projekt wpływ. js Spa**.</span><span class="sxs-lookup"><span data-stu-id="c2aed-127">In the **New Project** wizard, select **Ember.js SPA Project**.</span></span>

![](emberjs-template/_static/image4.png)

## <a name="emberjs-spa-template-overview"></a><span data-ttu-id="c2aed-128">Przegląd szablonu SPA EmberJS</span><span class="sxs-lookup"><span data-stu-id="c2aed-128">EmberJS SPA Template Overview</span></span>

<span data-ttu-id="c2aed-129">Szablon EmberJS używa kombinacji jQuery, wpływ. js, kierownicy. js, aby utworzyć płynny, interaktywny interfejs użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c2aed-129">The EmberJS template uses a combination of jQuery, Ember.js, Handlebars.js to create a smooth, interactive UI.</span></span>

<span data-ttu-id="c2aed-130">Wpływ. js to biblioteka języka JavaScript, która korzysta ze wzorca MVC po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="c2aed-130">Ember.js is a JavaScript library that uses a client-side MVC pattern.</span></span>

- <span data-ttu-id="c2aed-131">*Szablon*, zapisany w języku tworzenia szablonów, zawiera opis interfejsu użytkownika aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c2aed-131">A *template*, written in the Handlebars templating language, describes the application user interface.</span></span> <span data-ttu-id="c2aed-132">W trybie wydania [kompilator kierownicy](https://github.com/Myslik/csharp-ember-handlebars) służy do łączenia i kompilowania szablonu kierownicy.</span><span class="sxs-lookup"><span data-stu-id="c2aed-132">In release mode, the [Handlebars compiler](https://github.com/Myslik/csharp-ember-handlebars) is used to bundle and compile the handlebars template.</span></span>
- <span data-ttu-id="c2aed-133">*Model* przechowuje dane aplikacji, które pobiera z serwera (listy zadań do zrobienia i zadania do wykonania).</span><span class="sxs-lookup"><span data-stu-id="c2aed-133">A *model* stores the application data that it gets from the server (ToDo lists and ToDo items).</span></span>
- <span data-ttu-id="c2aed-134">*Kontroler* przechowuje stan aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c2aed-134">A *controller* stores application state.</span></span> <span data-ttu-id="c2aed-135">Kontrolery często są obecne dane modelu do odpowiednich szablonów.</span><span class="sxs-lookup"><span data-stu-id="c2aed-135">Controllers often present model data to the corresponding templates.</span></span>
- <span data-ttu-id="c2aed-136">*Widok umożliwia* przetłumaczenie zdarzeń pierwotnych z aplikacji i przekazanie ich do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="c2aed-136">A *view* translates primitive events from the application and passes these to the controller.</span></span>
- <span data-ttu-id="c2aed-137">*Router* zarządza stanem aplikacji, utrzymując w synchronizacji adresy URL i szablony.</span><span class="sxs-lookup"><span data-stu-id="c2aed-137">A *router* manages application state, keeping URLs and templates in sync.</span></span>

<span data-ttu-id="c2aed-138">Ponadto biblioteka danych wpływ może służyć do synchronizowania obiektów JSON (uzyskanych z serwera za pośrednictwem interfejsu API RESTful) i modeli klienta.</span><span class="sxs-lookup"><span data-stu-id="c2aed-138">In addition, the Ember Data library can be used to synchronize JSON objects (obtained from the server through a RESTful API) and the client models.</span></span>

<span data-ttu-id="c2aed-139">Szablon EmberJS SPA organizuje skrypty w osiem warstw:</span><span class="sxs-lookup"><span data-stu-id="c2aed-139">The EmberJS SPA template organizes the scripts into eight layers:</span></span>

- <span data-ttu-id="c2aed-140">WebAPI\_adapter. js, WebAPI\_serializator. js: rozszerza bibliotekę danych wpływ do pracy z interfejsem API sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c2aed-140">webapi\_adapter.js, webapi\_serializer.js: Extends the Ember Data library to work with ASP.NET Web API.</span></span>
- <span data-ttu-id="c2aed-141">Skrypty/pomocniks. js: definiuje nowych pomocników wpływ kierownicy.</span><span class="sxs-lookup"><span data-stu-id="c2aed-141">Scripts/helpers.js: Defines new Ember Handlebars helpers.</span></span>
- <span data-ttu-id="c2aed-142">Skrypty/App. js: tworzy aplikację i konfiguruje kartę i Serializator.</span><span class="sxs-lookup"><span data-stu-id="c2aed-142">Scripts/app.js: Creates the app and configures the adapter and serializer.</span></span>
- <span data-ttu-id="c2aed-143">Skrypty/aplikacje/modele/\*. js: definiuje modele.</span><span class="sxs-lookup"><span data-stu-id="c2aed-143">Scripts/app/models/\*.js: Defines the models.</span></span>
- <span data-ttu-id="c2aed-144">Skrypty/aplikacja/widoki/\*. js: definiuje widoki.</span><span class="sxs-lookup"><span data-stu-id="c2aed-144">Scripts/app/views/\*.js: Defines the views.</span></span>
- <span data-ttu-id="c2aed-145">Skrypty/aplikacja/kontrolery/\*. js: definiuje kontrolery.</span><span class="sxs-lookup"><span data-stu-id="c2aed-145">Scripts/app/controllers/\*.js: Defines the controllers.</span></span>
- <span data-ttu-id="c2aed-146">Skrypty/aplikacje/trasy, skrypty/aplikacja/router. js: definiuje trasy.</span><span class="sxs-lookup"><span data-stu-id="c2aed-146">Scripts/app/routes, Scripts/app/router.js: Defines the routes.</span></span>
- <span data-ttu-id="c2aed-147">Szablony/\*. HBS: definiuje szablony kierownicy.</span><span class="sxs-lookup"><span data-stu-id="c2aed-147">Templates/\*.hbs: Defines the handlebars templates.</span></span>

<span data-ttu-id="c2aed-148">Przyjrzyjmy się kilku skryptom bardziej szczegółowym.</span><span class="sxs-lookup"><span data-stu-id="c2aed-148">Let's look at some of these scripts in more detail.</span></span>

## <a name="models"></a><span data-ttu-id="c2aed-149">Modele</span><span class="sxs-lookup"><span data-stu-id="c2aed-149">Models</span></span>

<span data-ttu-id="c2aed-150">Modele są zdefiniowane w folderze skrypty/aplikacja/modele.</span><span class="sxs-lookup"><span data-stu-id="c2aed-150">The models are defined in the Scripts/app/models folder.</span></span> <span data-ttu-id="c2aed-151">Istnieją dwa pliki modelu: todoItem. js i todoList. js.</span><span class="sxs-lookup"><span data-stu-id="c2aed-151">There are two model files: todoItem.js and todoList.js.</span></span>

<span data-ttu-id="c2aed-152">do **zrobienia. model. js** definiuje modele po stronie klienta (przeglądarki) dla list zadań do wykonania.</span><span class="sxs-lookup"><span data-stu-id="c2aed-152">**todo.model.js** defines the client-side (browser) models for the to-do lists.</span></span> <span data-ttu-id="c2aed-153">Istnieją dwie klasy modelu: todoItem i todoList.</span><span class="sxs-lookup"><span data-stu-id="c2aed-153">There are two model classes: todoItem and todoList.</span></span> <span data-ttu-id="c2aed-154">W wpływ, modele są podklasami DS. Wzorów.</span><span class="sxs-lookup"><span data-stu-id="c2aed-154">In Ember, models are subclasses of DS.Model.</span></span> <span data-ttu-id="c2aed-155">Model może mieć właściwości z atrybutami:</span><span class="sxs-lookup"><span data-stu-id="c2aed-155">A model can have properties with attributes:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample1.js)]

<span data-ttu-id="c2aed-156">Modele mogą definiować relacje z innymi modelami:</span><span class="sxs-lookup"><span data-stu-id="c2aed-156">Models can define relationships to other models:</span></span>

[!code-css[Main](emberjs-template/samples/sample2.css)]

<span data-ttu-id="c2aed-157">Modele mogą mieć obliczone właściwości, które są powiązane z innymi właściwościami:</span><span class="sxs-lookup"><span data-stu-id="c2aed-157">Models can have computed properties that bind to other properties:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample3.js)]

<span data-ttu-id="c2aed-158">Modele mogą mieć funkcje obserwatorów, które są wywoływane, gdy obserwowana właściwość zostanie zmieniona:</span><span class="sxs-lookup"><span data-stu-id="c2aed-158">Models can have observer functions, which are invoked when an observed property changes:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample4.js)]

## <a name="views"></a><span data-ttu-id="c2aed-159">Widoki</span><span class="sxs-lookup"><span data-stu-id="c2aed-159">Views</span></span>

<span data-ttu-id="c2aed-160">Widoki są zdefiniowane w folderze skrypty/aplikacje/widoki.</span><span class="sxs-lookup"><span data-stu-id="c2aed-160">The views are defined in the Scripts/app/views folder.</span></span> <span data-ttu-id="c2aed-161">Widok umożliwia przetłumaczenie zdarzeń z interfejsu użytkownika aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c2aed-161">A view translates events from the application UI.</span></span> <span data-ttu-id="c2aed-162">Procedura obsługi zdarzeń może wywoływać z powrotem do funkcji kontrolera lub bezpośrednio wywołać kontekst danych.</span><span class="sxs-lookup"><span data-stu-id="c2aed-162">An event handler can call back to controller functions, or simply call the data context directly.</span></span>

<span data-ttu-id="c2aed-163">Na przykład poniższy kod pochodzi z widoku/TodoItemEditView. js.</span><span class="sxs-lookup"><span data-stu-id="c2aed-163">For example, the following code is from views/TodoItemEditView.js.</span></span> <span data-ttu-id="c2aed-164">Definiuje obsługę zdarzeń dla wejściowego pola tekstowego.</span><span class="sxs-lookup"><span data-stu-id="c2aed-164">It defines the event handling for an input text field.</span></span>

[!code-javascript[Main](emberjs-template/samples/sample5.js)]

## <a name="controller"></a><span data-ttu-id="c2aed-165">Kontroler</span><span class="sxs-lookup"><span data-stu-id="c2aed-165">Controller</span></span>

<span data-ttu-id="c2aed-166">Kontrolery są zdefiniowane w folderze skrypty/aplikacja/kontrolery.</span><span class="sxs-lookup"><span data-stu-id="c2aed-166">The controllers are defined in the Scripts/app/controllers folder.</span></span> <span data-ttu-id="c2aed-167">Aby przedstawić jeden model, należy zwiększyć `Ember.ObjectController`:</span><span class="sxs-lookup"><span data-stu-id="c2aed-167">To represent a single model, extend `Ember.ObjectController`:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample6.js)]

<span data-ttu-id="c2aed-168">Kontroler może również reprezentować kolekcję modeli, rozszerzając `Ember.ArrayController`.</span><span class="sxs-lookup"><span data-stu-id="c2aed-168">A controller can also represent a collection of models by extending `Ember.ArrayController`.</span></span> <span data-ttu-id="c2aed-169">Na przykład TodoListController reprezentuje tablicę obiektów `todoList`.</span><span class="sxs-lookup"><span data-stu-id="c2aed-169">For example, the TodoListController represents an array of `todoList` objects.</span></span> <span data-ttu-id="c2aed-170">Kontroler sortuje według identyfikatora todoList, w kolejności malejącej:</span><span class="sxs-lookup"><span data-stu-id="c2aed-170">The controller sorts by todoList ID, in descending order:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample7.js)]

<span data-ttu-id="c2aed-171">Kontroler definiuje funkcję o nazwie `addTodoList`, która tworzy nowy todoList i dodaje ją do tablicy.</span><span class="sxs-lookup"><span data-stu-id="c2aed-171">The controller defines a function named `addTodoList`, which creates a new todoList and adds it to the array.</span></span> <span data-ttu-id="c2aed-172">Aby zobaczyć, jak ta funkcja jest wywoływana, Otwórz plik szablonu o nazwie todoListTemplate. html w folderze Templates.</span><span class="sxs-lookup"><span data-stu-id="c2aed-172">To see how this function gets called, open the template file named todoListTemplate.html, in the Templates folder.</span></span> <span data-ttu-id="c2aed-173">Poniższy kod szablonu wiąże przycisk z funkcją `addTodoList`:</span><span class="sxs-lookup"><span data-stu-id="c2aed-173">The following template code binds a button to the `addTodoList` function:</span></span>

[!code-html[Main](emberjs-template/samples/sample8.html)]

<span data-ttu-id="c2aed-174">Kontroler zawiera również właściwość `error`, która zawiera komunikat o błędzie.</span><span class="sxs-lookup"><span data-stu-id="c2aed-174">The controller also contains an `error` property, which holds an error message.</span></span> <span data-ttu-id="c2aed-175">Oto kod szablonu do wyświetlania komunikatu o błędzie (również w todoListTemplate. html):</span><span class="sxs-lookup"><span data-stu-id="c2aed-175">Here is the template code to display the error message (also in todoListTemplate.html):</span></span>

[!code-html[Main](emberjs-template/samples/sample9.html)]

## <a name="routes"></a><span data-ttu-id="c2aed-176">Trasy</span><span class="sxs-lookup"><span data-stu-id="c2aed-176">Routes</span></span>

<span data-ttu-id="c2aed-177">Router. js definiuje trasy i domyślny szablon do wyświetlania, konfiguruje stan aplikacji i dopasowuje adresy URL do tras:</span><span class="sxs-lookup"><span data-stu-id="c2aed-177">Router.js defines the routes and the default template to display, sets up application state, and matches URLs to routes:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample10.js)]

<span data-ttu-id="c2aed-178">TodoListRoute. js ładuje dane dla TodoListRoute, zastępując funkcję setupController:</span><span class="sxs-lookup"><span data-stu-id="c2aed-178">TodoListRoute.js loads data for the TodoListRoute by overriding the setupController function:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample11.js)]

<span data-ttu-id="c2aed-179">Wpływ używa konwencji nazewnictwa w celu dopasowania adresów URL, nazw tras, kontrolerów i szablonów.</span><span class="sxs-lookup"><span data-stu-id="c2aed-179">Ember uses naming conventions to match URLs, route names, controllers, and templates.</span></span> <span data-ttu-id="c2aed-180">Aby uzyskać więcej informacji, zobacz [http://emberjs.com/guides/routing/defining-your-routes/](http://emberjs.com/guides/routing/defining-your-routes/) w dokumentacji EmberJS.</span><span class="sxs-lookup"><span data-stu-id="c2aed-180">For more information, see [http://emberjs.com/guides/routing/defining-your-routes/](http://emberjs.com/guides/routing/defining-your-routes/) at the EmberJS documentation.</span></span>

## <a name="templates"></a><span data-ttu-id="c2aed-181">Szablony</span><span class="sxs-lookup"><span data-stu-id="c2aed-181">Templates</span></span>

<span data-ttu-id="c2aed-182">Folder szablonów zawiera cztery szablony:</span><span class="sxs-lookup"><span data-stu-id="c2aed-182">The Templates folder contains four templates:</span></span>

- <span data-ttu-id="c2aed-183">Application. HBS: domyślny szablon, który jest renderowany podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c2aed-183">application.hbs: The default template that is rendered when the application is started.</span></span>
- <span data-ttu-id="c2aed-184">about. HBS: szablon dla trasy "/about".</span><span class="sxs-lookup"><span data-stu-id="c2aed-184">about.hbs: The template for the "/about" route.</span></span>
- <span data-ttu-id="c2aed-185">index. HBS: szablon dotyczący trasy głównej "/".</span><span class="sxs-lookup"><span data-stu-id="c2aed-185">index.hbs: The template for the root "/" route.</span></span>
- <span data-ttu-id="c2aed-186">todoList. HBS: szablon dla trasy "/Todo".</span><span class="sxs-lookup"><span data-stu-id="c2aed-186">todoList.hbs: The template for the "/todo" route.</span></span>
- <span data-ttu-id="c2aed-187">\_pasek nawigacyjny. HBS: szablon definiuje menu nawigacji.</span><span class="sxs-lookup"><span data-stu-id="c2aed-187">\_navbar.hbs: The template defines the navigation menu.</span></span>

<span data-ttu-id="c2aed-188">Szablon aplikacji działa jak strona wzorcowa.</span><span class="sxs-lookup"><span data-stu-id="c2aed-188">The application template acts like a master page.</span></span> <span data-ttu-id="c2aed-189">Zawiera nagłówek, stopkę i "{{gniazdko}}", aby wstawić inne szablony w zależności od trasy.</span><span class="sxs-lookup"><span data-stu-id="c2aed-189">It contains a header, a footer, and an "{{outlet}}" to insert other templates in depending on the route.</span></span> <span data-ttu-id="c2aed-190">Aby uzyskać więcej informacji na temat szablonów aplikacji w programie wpływ, zobacz [http://guides.emberjs.com/v1.10.0/templates/the-application-template//](http://guides.emberjs.com/v1.10.0/templates/the-application-template/).</span><span class="sxs-lookup"><span data-stu-id="c2aed-190">For more information about application templates in Ember, see [http://guides.emberjs.com/v1.10.0/templates/the-application-template//](http://guides.emberjs.com/v1.10.0/templates/the-application-template/).</span></span>

<span data-ttu-id="c2aed-191">Szablon "/todoList" zawiera dwa wyrażenia pętli.</span><span class="sxs-lookup"><span data-stu-id="c2aed-191">The "/todoList" template contains two loop expressions.</span></span> <span data-ttu-id="c2aed-192">Pętla zewnętrzna jest `{{#each controller}}`, a pętla wewnątrz jest `{{#each todos}}`.</span><span class="sxs-lookup"><span data-stu-id="c2aed-192">The outside loop is `{{#each controller}}`, and the inside loop is `{{#each todos}}`.</span></span> <span data-ttu-id="c2aed-193">Poniższy kod przedstawia wbudowany widok `Ember.Checkbox`, dostosowany `App.TodoItemEditView`i link z akcją `deleteTodo`.</span><span class="sxs-lookup"><span data-stu-id="c2aed-193">The following code shows a built-in `Ember.Checkbox` view, a customized `App.TodoItemEditView`, and a link with a `deleteTodo` action.</span></span>

[!code-html[Main](emberjs-template/samples/sample12.html)]

<span data-ttu-id="c2aed-194">Klasa `HtmlHelperExtensions` zdefiniowana w obszarze controllers/HtmlHelperExtensions. cs definiuje funkcję pomocnika do buforowania i wstawiania plików szablonów, gdy **debugowanie** jest ustawione na **wartość true** w pliku Web. config.</span><span class="sxs-lookup"><span data-stu-id="c2aed-194">The `HtmlHelperExtensions` class, defined in Controllers/HtmlHelperExtensions.cs, defines a helper function to cache and insert template files when **debug** is set to **true** in the Web.config file.</span></span> <span data-ttu-id="c2aed-195">Ta funkcja jest wywoływana z pliku widoku ASP.NET MVC zdefiniowanego w widokach/Home/App. cshtml:</span><span class="sxs-lookup"><span data-stu-id="c2aed-195">This function is called from the ASP.NET MVC view file defined in Views/Home/App.cshtml:</span></span>

[!code-cshtml[Main](emberjs-template/samples/sample13.cshtml)]

<span data-ttu-id="c2aed-196">Wywoływana bez argumentów, funkcja renderuje wszystkie pliki szablonu w folderze Templates.</span><span class="sxs-lookup"><span data-stu-id="c2aed-196">Called with no arguments, the function renders all of the template files in the Templates folder.</span></span> <span data-ttu-id="c2aed-197">Można również określić podfolder lub określony plik szablonu.</span><span class="sxs-lookup"><span data-stu-id="c2aed-197">You can also specify a subfolder or a specific template file.</span></span>

<span data-ttu-id="c2aed-198">Gdy **debugowanie** ma **wartość false** w pliku Web. config, aplikacja zawiera element pakietu "~/bundles/templates".</span><span class="sxs-lookup"><span data-stu-id="c2aed-198">When **debug** is **false** in Web.config, the application includes the bundle item "~/bundles/templates".</span></span> <span data-ttu-id="c2aed-199">Ten element pakietu jest dodawany w BundleConfig.cs, przy użyciu biblioteki współpracującej z kompilatorem:</span><span class="sxs-lookup"><span data-stu-id="c2aed-199">This bundle item is added in BundleConfig.cs, using the Handlebars compiler library:</span></span>

[!code-csharp[Main](emberjs-template/samples/sample14.cs)]

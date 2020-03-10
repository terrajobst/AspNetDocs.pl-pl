---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-6
title: Tworzenie klienta JavaScript | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 20360326-b123-4b1e-abae-1d350edf4ce4
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-6
msc.type: authoredcontent
ms.openlocfilehash: 74f2cc4e5e401d690042b05b028dfc0c46ae282a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622347"
---
# <a name="create-the-javascript-client"></a><span data-ttu-id="d19d5-102">Tworzenie klienta JavaScript</span><span class="sxs-lookup"><span data-stu-id="d19d5-102">Create the JavaScript Client</span></span>

<span data-ttu-id="d19d5-103">według [Jan Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="d19d5-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="d19d5-104">Pobierz ukończony projekt</span><span class="sxs-lookup"><span data-stu-id="d19d5-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="d19d5-105">W tej sekcji utworzysz klienta dla aplikacji, używając języka HTML, JavaScript i biblioteki [separowania. js](http://knockoutjs.com/) .</span><span class="sxs-lookup"><span data-stu-id="d19d5-105">In this section, you will create the client for the application, using HTML, JavaScript, and the [Knockout.js](http://knockoutjs.com/) library.</span></span> <span data-ttu-id="d19d5-106">Utworzymy aplikację kliencką na etapach:</span><span class="sxs-lookup"><span data-stu-id="d19d5-106">We'll build the client app in stages:</span></span>

- <span data-ttu-id="d19d5-107">Wyświetlanie listy książek.</span><span class="sxs-lookup"><span data-stu-id="d19d5-107">Showing a list of books.</span></span>
- <span data-ttu-id="d19d5-108">Wyświetlanie szczegółów książki.</span><span class="sxs-lookup"><span data-stu-id="d19d5-108">Showing a book detail.</span></span>
- <span data-ttu-id="d19d5-109">Dodawanie nowej książki.</span><span class="sxs-lookup"><span data-stu-id="d19d5-109">Adding a new book.</span></span>

<span data-ttu-id="d19d5-110">Biblioteka odcinania używa wzorca Model-View-ViewModel (MVVM):</span><span class="sxs-lookup"><span data-stu-id="d19d5-110">The Knockout library uses the Model-View-ViewModel (MVVM) pattern:</span></span>

- <span data-ttu-id="d19d5-111">**Model** jest reprezentacją po stronie serwera danych w domenie biznesowej (w naszym przypadku, książki i autorzy).</span><span class="sxs-lookup"><span data-stu-id="d19d5-111">The **model** is the server-side representation of the data in the business domain (in our case, books and authors).</span></span>
- <span data-ttu-id="d19d5-112">**Widok** jest warstwą prezentacji (html).</span><span class="sxs-lookup"><span data-stu-id="d19d5-112">The **view** is the presentation layer (HTML).</span></span>
- <span data-ttu-id="d19d5-113">**Model widoku** to obiekt JavaScript, który zawiera modele.</span><span class="sxs-lookup"><span data-stu-id="d19d5-113">The **view model** is a JavaScript object that holds the models.</span></span> <span data-ttu-id="d19d5-114">Model widoku jest abstrakcją kodu interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="d19d5-114">The view model is a code abstraction of the UI.</span></span> <span data-ttu-id="d19d5-115">Nie ma informacji o reprezentacji HTML.</span><span class="sxs-lookup"><span data-stu-id="d19d5-115">It has no knowledge of the HTML representation.</span></span> <span data-ttu-id="d19d5-116">Zamiast tego reprezentuje funkcje abstrakcyjne widoku, takie jak &quot;listy książek&quot;.</span><span class="sxs-lookup"><span data-stu-id="d19d5-116">Instead, it represents abstract features of the view, such as &quot;a list of books&quot;.</span></span>

<span data-ttu-id="d19d5-117">Widok jest powiązany z danymi z modelem widoku.</span><span class="sxs-lookup"><span data-stu-id="d19d5-117">The view is data-bound to the view model.</span></span> <span data-ttu-id="d19d5-118">Aktualizacje modelu widoku są automatycznie odzwierciedlane w widoku.</span><span class="sxs-lookup"><span data-stu-id="d19d5-118">Updates to the view model are automatically reflected in the view.</span></span> <span data-ttu-id="d19d5-119">Model widoku pobiera również zdarzenia z widoku, takie jak kliknięcia przycisku.</span><span class="sxs-lookup"><span data-stu-id="d19d5-119">The view model also gets events from the view, such as button clicks.</span></span>

![](part-6/_static/image1.png)

<span data-ttu-id="d19d5-120">Takie podejście ułatwia zmianę układu i interfejsu użytkownika aplikacji, ponieważ można zmienić powiązania, bez konieczności ponownego pisania kodu.</span><span class="sxs-lookup"><span data-stu-id="d19d5-120">This approach makes it easy to change the layout and UI of your app, because you can change the bindings, without rewriting any code.</span></span> <span data-ttu-id="d19d5-121">Na przykład można wyświetlić listę elementów jako `<ul>`, a następnie zmienić ją później na tabelę.</span><span class="sxs-lookup"><span data-stu-id="d19d5-121">For example, you might show a list of items as a `<ul>`, then change it later to a table.</span></span>

## <a name="add-the-knockout-library"></a><span data-ttu-id="d19d5-122">Dodawanie biblioteki odcinania</span><span class="sxs-lookup"><span data-stu-id="d19d5-122">Add the Knockout Library</span></span>

<span data-ttu-id="d19d5-123">W programie Visual Studio w menu **Narzędzia** wybierz pozycję **Menedżer pakietów NuGet**.</span><span class="sxs-lookup"><span data-stu-id="d19d5-123">In Visual Studio, from the **Tools** menu, select **NuGet Package Manager**.</span></span> <span data-ttu-id="d19d5-124">Następnie wybierz pozycję **konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="d19d5-124">Then select **Package Manager Console**.</span></span> <span data-ttu-id="d19d5-125">W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="d19d5-125">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](part-6/samples/sample1.cmd)]

<span data-ttu-id="d19d5-126">To polecenie dodaje pliki odcinania do folderu skrypty.</span><span class="sxs-lookup"><span data-stu-id="d19d5-126">This command adds the Knockout files to the Scripts folder.</span></span>

## <a name="create-the-view-model"></a><span data-ttu-id="d19d5-127">Tworzenie modelu widoku</span><span class="sxs-lookup"><span data-stu-id="d19d5-127">Create the View Model</span></span>

<span data-ttu-id="d19d5-128">Dodaj plik języka JavaScript o nazwie App. js do folderu Scripts.</span><span class="sxs-lookup"><span data-stu-id="d19d5-128">Add a JavaScript file named app.js to the Scripts folder.</span></span> <span data-ttu-id="d19d5-129">(W Eksplorator rozwiązań kliknij prawym przyciskiem myszy folder skrypty, wybierz polecenie **Dodaj**, a następnie wybierz pozycję **plik JavaScript**). Wklej następujący kod:</span><span class="sxs-lookup"><span data-stu-id="d19d5-129">(In Solution Explorer, right-click the Scripts folder, select **Add**, then select **JavaScript File**.) Paste in the following code:</span></span>

[!code-javascript[Main](part-6/samples/sample2.js)]

<span data-ttu-id="d19d5-130">W odcinania Klasa `observable` włącza powiązanie danych.</span><span class="sxs-lookup"><span data-stu-id="d19d5-130">In Knockout, the `observable` class enables data-binding.</span></span> <span data-ttu-id="d19d5-131">Gdy zawartość zauważalnej zmiany, zauważalnie powiadamia wszystkie kontrolki powiązane z danymi, aby mogli ją zaktualizować.</span><span class="sxs-lookup"><span data-stu-id="d19d5-131">When the contents of an observable change, the observable notifies all of the data-bound controls, so they can update themselves.</span></span> <span data-ttu-id="d19d5-132">(Klasa `observableArray` to wersja tablicy, która jest *zauważalna*). Aby zacząć od, nasz model widoku ma dwa observables:</span><span class="sxs-lookup"><span data-stu-id="d19d5-132">(The `observableArray` class is the array version of *observable*.) To start with, our view model has two observables:</span></span>

- <span data-ttu-id="d19d5-133">`books` zawiera listę książek.</span><span class="sxs-lookup"><span data-stu-id="d19d5-133">`books` holds the list of books.</span></span>
- <span data-ttu-id="d19d5-134">`error` zawiera komunikat o błędzie, jeśli wywołanie AJAX zakończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="d19d5-134">`error` contains an error message if an AJAX call fails.</span></span>

<span data-ttu-id="d19d5-135">Metoda `getAllBooks` umożliwia wywołanie AJAX w celu uzyskania listy książek.</span><span class="sxs-lookup"><span data-stu-id="d19d5-135">The `getAllBooks` method makes an AJAX call to get the list of books.</span></span> <span data-ttu-id="d19d5-136">Następnie wypycha wynik do tablicy `books`.</span><span class="sxs-lookup"><span data-stu-id="d19d5-136">Then it pushes the result onto the `books` array.</span></span>

<span data-ttu-id="d19d5-137">Metoda `ko.applyBindings` jest częścią biblioteki odcinania.</span><span class="sxs-lookup"><span data-stu-id="d19d5-137">The `ko.applyBindings` method is part of the Knockout library.</span></span> <span data-ttu-id="d19d5-138">Przyjmuje model widoku jako parametr i konfiguruje powiązanie danych.</span><span class="sxs-lookup"><span data-stu-id="d19d5-138">It takes the view model as a parameter and sets up the data binding.</span></span>

## <a name="add-a-script-bundle"></a><span data-ttu-id="d19d5-139">Dodawanie pakietu skryptu</span><span class="sxs-lookup"><span data-stu-id="d19d5-139">Add a Script Bundle</span></span>

<span data-ttu-id="d19d5-140">Zgrupowanie to funkcja w programie ASP.NET 4,5, która ułatwia łączenie wielu plików w jeden plik i tworzenie ich.</span><span class="sxs-lookup"><span data-stu-id="d19d5-140">Bundling is a feature in ASP.NET 4.5 that makes it easy to combine or bundle multiple files into a single file.</span></span> <span data-ttu-id="d19d5-141">Zgrupowanie zmniejsza liczbę żądań do serwera, co może poprawić czas ładowania strony.</span><span class="sxs-lookup"><span data-stu-id="d19d5-141">Bundling reduces the number of requests to the server, which can improve page load time.</span></span>

<span data-ttu-id="d19d5-142">Otwórz aplikację pliku\_Start/BundleConfig. cs.</span><span class="sxs-lookup"><span data-stu-id="d19d5-142">Open the file App\_Start/BundleConfig.cs.</span></span> <span data-ttu-id="d19d5-143">Dodaj następujący kod do metody RegisterBundles.</span><span class="sxs-lookup"><span data-stu-id="d19d5-143">Add the following code to the RegisterBundles method.</span></span>

[!code-csharp[Main](part-6/samples/sample3.cs)]

> [!div class="step-by-step"]
> <span data-ttu-id="d19d5-144">[Poprzednie](part-5.md)
> [dalej](part-7.md)</span><span class="sxs-lookup"><span data-stu-id="d19d5-144">[Previous](part-5.md)
[Next](part-7.md)</span></span>

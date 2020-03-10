---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
title: Wprowadzenie z formularzami sieci Web ASP.NET 4,7 i Visual Studio 2017 | Microsoft Docs
author: Erikre
description: W tej serii samouczków krok po kroku przedstawiono podstawowe informacje na temat tworzenia aplikacji ASP.NET Web Forms przy użyciu ASP.NET 4,7 i Microsoft Visual Studio
ms.author: riande
ms.date: 01/09/2019
ms.assetid: 9b96eaa1-8ef0-4338-a2e8-e0f970bfaf68
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
msc.type: authoredcontent
ms.openlocfilehash: 52d5eb7abe4520ebdf6667d299d055fc7619a635
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78568223"
---
# <a name="getting-started-with-aspnet-45-web-forms-and-visual-studio-2017"></a><span data-ttu-id="31190-103">Wprowadzenie z formularzami sieci Web ASP.NET 4,5 i programem Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="31190-103">Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2017</span></span>

<span data-ttu-id="31190-104">[Pobierz program Wingtip zabawki (C#)](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) lub [Pobierz książkę elektroniczną (PDF)](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span><span class="sxs-lookup"><span data-stu-id="31190-104">[Download Wingtip Toys Sample Project (C#)](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) or [Download E-book (PDF)](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span></span>

<span data-ttu-id="31190-105">W tej serii samouczków pokazano, jak utworzyć aplikację ASP.NET Web Forms z ASP.NET 4,5 i Microsoft Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="31190-105">This tutorial series shows you how to build an ASP.NET Web Forms application with ASP.NET 4.5 and Microsoft Visual Studio 2017.</span></span> 

## <a name="introduction"></a><span data-ttu-id="31190-106">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="31190-106">Introduction</span></span>

<span data-ttu-id="31190-107">Ta seria samouczków prowadzi użytkownika przez proces tworzenia aplikacji ASP.NET Web Forms przy użyciu programu Visual Studio 2017 i ASP.NET 4,5.</span><span class="sxs-lookup"><span data-stu-id="31190-107">This tutorial series guides you through creating an ASP.NET Web Forms application using Visual Studio 2017 and ASP.NET 4.5.</span></span> <span data-ttu-id="31190-108">Utworzysz aplikację o nazwie **zabawki Wingtip** — uproszczona witryna sieci Web w sklepie do sprzedaży elementów sprzedawanych w trybie online.</span><span class="sxs-lookup"><span data-stu-id="31190-108">You'll create an application named **Wingtip Toys** - a simplified storefront web site selling items online.</span></span> <span data-ttu-id="31190-109">W ramach serii są wyróżnione nowe funkcje ASP.NET 4,5.</span><span class="sxs-lookup"><span data-stu-id="31190-109">During the series, new ASP.NET 4.5 features are highlighted.</span></span>

### <a name="target-audience"></a><span data-ttu-id="31190-110">Odbiorcy docelowi</span><span class="sxs-lookup"><span data-stu-id="31190-110">Target audience</span></span>

<span data-ttu-id="31190-111">Deweloperzy, którzy korzystają z formularzy sieci Web ASP.NET, są docelowymi odbiorcami tej serii samouczków.</span><span class="sxs-lookup"><span data-stu-id="31190-111">Developers new to ASP.NET Web Forms are the target audience for this tutorial series.</span></span>

<span data-ttu-id="31190-112">Należy mieć pewną wiedzę w następujących obszarach:</span><span class="sxs-lookup"><span data-stu-id="31190-112">You should have some knowledge in the following areas:</span></span>

- <span data-ttu-id="31190-113">Programowanie zorientowane obiektowo (OOP) i Języki</span><span class="sxs-lookup"><span data-stu-id="31190-113">Object-oriented programming (OOP) and languages</span></span>
- <span data-ttu-id="31190-114">Programowanie dla sieci Web (HTML, CSS, JavaScript)</span><span class="sxs-lookup"><span data-stu-id="31190-114">Web development (HTML, CSS, JavaScript)</span></span>
- <span data-ttu-id="31190-115">Relacyjne bazy danych</span><span class="sxs-lookup"><span data-stu-id="31190-115">Relational databases</span></span>
- <span data-ttu-id="31190-116">Architektura N-warstwowa</span><span class="sxs-lookup"><span data-stu-id="31190-116">N-tier architecture</span></span>

<span data-ttu-id="31190-117">Aby przejrzeć te obszary, Rozważ zbadanie następującej zawartości:</span><span class="sxs-lookup"><span data-stu-id="31190-117">To review these areas, consider studying the following content:</span></span>

- [<span data-ttu-id="31190-118">Wprowadzenie z wizualizacjąC#</span><span class="sxs-lookup"><span data-stu-id="31190-118">Getting Started with Visual C#</span></span>](https://msdn.microsoft.com/library/a72418yk.aspx)
- <span data-ttu-id="31190-119">[Programowanie dla sieci Web](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, php, jQuery](http://w3schools.com/)</span><span class="sxs-lookup"><span data-stu-id="31190-119">[Web Development](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)</span></span>
- [<span data-ttu-id="31190-120">Relacyjna baza danych</span><span class="sxs-lookup"><span data-stu-id="31190-120">Relational database</span></span>](http://en.wikipedia.org/wiki/Relational_database)
- [<span data-ttu-id="31190-121">Architektura wielowarstwowa</span><span class="sxs-lookup"><span data-stu-id="31190-121">Multitier architecture</span></span>](http://en.wikipedia.org/wiki/Multitier_architecture)

### <a name="application-features"></a><span data-ttu-id="31190-122">Funkcje aplikacji</span><span class="sxs-lookup"><span data-stu-id="31190-122">Application features</span></span>

<span data-ttu-id="31190-123">Funkcje formularza sieci Web ASP.NET przedstawione w tej serii obejmują:</span><span class="sxs-lookup"><span data-stu-id="31190-123">The ASP.NET Web Form features presented in this series include:</span></span>

- <span data-ttu-id="31190-124">Projekt aplikacji sieci Web (nie projekt witryny sieci Web)</span><span class="sxs-lookup"><span data-stu-id="31190-124">The Web Application Project (not Web Site Project)</span></span>
- <span data-ttu-id="31190-125">Formularze sieci Web</span><span class="sxs-lookup"><span data-stu-id="31190-125">Web Forms</span></span>
- <span data-ttu-id="31190-126">Strony wzorcowe, konfiguracja</span><span class="sxs-lookup"><span data-stu-id="31190-126">Master Pages, Configuration</span></span>
- <span data-ttu-id="31190-127">Bootstrap</span><span class="sxs-lookup"><span data-stu-id="31190-127">Bootstrap</span></span>
- <span data-ttu-id="31190-128">Entity Framework Code First, LocalDB</span><span class="sxs-lookup"><span data-stu-id="31190-128">Entity Framework Code First, LocalDB</span></span>
- <span data-ttu-id="31190-129">Żądaj weryfikacji</span><span class="sxs-lookup"><span data-stu-id="31190-129">Request Validation</span></span>
- <span data-ttu-id="31190-130">Kontrolki danych o jednoznacznie określonym typie</span><span class="sxs-lookup"><span data-stu-id="31190-130">Strongly-typed Data Controls</span></span>
- <span data-ttu-id="31190-131">Powiązanie modelu</span><span class="sxs-lookup"><span data-stu-id="31190-131">Model Binding</span></span>
- <span data-ttu-id="31190-132">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="31190-132">Data Annotations</span></span>
- <span data-ttu-id="31190-133">Dostawcy wartości</span><span class="sxs-lookup"><span data-stu-id="31190-133">Value Providers</span></span>
- <span data-ttu-id="31190-134">SSL i OAuth</span><span class="sxs-lookup"><span data-stu-id="31190-134">SSL and OAuth</span></span>
- <span data-ttu-id="31190-135">ASP.NET Identity, konfiguracja i autoryzacja</span><span class="sxs-lookup"><span data-stu-id="31190-135">ASP.NET Identity, Configuration, and Authorization</span></span>
- <span data-ttu-id="31190-136">Niezauważalna weryfikacja</span><span class="sxs-lookup"><span data-stu-id="31190-136">Unobtrusive Validation</span></span>
- <span data-ttu-id="31190-137">Routing</span><span class="sxs-lookup"><span data-stu-id="31190-137">Routing</span></span>
- <span data-ttu-id="31190-138">Obsługa błędów platformy ASP.NET</span><span class="sxs-lookup"><span data-stu-id="31190-138">ASP.NET Error Handling</span></span>

### <a name="application-scenarios-and-tasks"></a><span data-ttu-id="31190-139">Scenariusze i zadania aplikacji</span><span class="sxs-lookup"><span data-stu-id="31190-139">Application scenarios and tasks</span></span>

<span data-ttu-id="31190-140">Zadania serii samouczków obejmują:</span><span class="sxs-lookup"><span data-stu-id="31190-140">Tutorial series tasks include:</span></span>

- <span data-ttu-id="31190-141">Tworzenie, przeglądanie i uruchamianie nowego projektu</span><span class="sxs-lookup"><span data-stu-id="31190-141">Creating, reviewing, and running a new project</span></span>
- <span data-ttu-id="31190-142">Tworzenie struktury bazy danych</span><span class="sxs-lookup"><span data-stu-id="31190-142">Creating a database structure</span></span>
- <span data-ttu-id="31190-143">Inicjowanie i wypełnianie bazy danych</span><span class="sxs-lookup"><span data-stu-id="31190-143">Initializing and seeding a database</span></span>
- <span data-ttu-id="31190-144">Dostosowywanie interfejsu użytkownika za pomocą stylów, grafiki i strony wzorcowej</span><span class="sxs-lookup"><span data-stu-id="31190-144">Customizing the UI with styles, graphics, and a master page</span></span>
- <span data-ttu-id="31190-145">Dodawanie stron i nawigowania</span><span class="sxs-lookup"><span data-stu-id="31190-145">Adding pages and navigation</span></span>
- <span data-ttu-id="31190-146">Wyświetlanie szczegółów menu i danych produktu</span><span class="sxs-lookup"><span data-stu-id="31190-146">Displaying menu details and product data</span></span>
- <span data-ttu-id="31190-147">Tworzenie koszyka</span><span class="sxs-lookup"><span data-stu-id="31190-147">Creating a shopping cart</span></span>
- <span data-ttu-id="31190-148">Dodawanie obsługi protokołów SSL i OAuth</span><span class="sxs-lookup"><span data-stu-id="31190-148">Adding SSL and OAuth support</span></span>
- <span data-ttu-id="31190-149">Dodawanie metody płatności</span><span class="sxs-lookup"><span data-stu-id="31190-149">Adding a payment method</span></span>
- <span data-ttu-id="31190-150">Dołączenie roli administratora i użytkownika do aplikacji</span><span class="sxs-lookup"><span data-stu-id="31190-150">Including an administrator role and a user to the application</span></span>
- <span data-ttu-id="31190-151">Ograniczanie dostępu do określonych stron i folderów</span><span class="sxs-lookup"><span data-stu-id="31190-151">Restricting access to specific pages and folder</span></span>
- <span data-ttu-id="31190-152">Przekazywanie pliku do aplikacji sieci Web</span><span class="sxs-lookup"><span data-stu-id="31190-152">Uploading a file to the web application</span></span>
- <span data-ttu-id="31190-153">Implementowanie walidacji danych wejściowych</span><span class="sxs-lookup"><span data-stu-id="31190-153">Implementing input validation</span></span>
- <span data-ttu-id="31190-154">Rejestrowanie tras dla aplikacji sieci Web</span><span class="sxs-lookup"><span data-stu-id="31190-154">Registering routes for the web application</span></span>
- <span data-ttu-id="31190-155">Implementowanie obsługi błędów i rejestrowania błędów</span><span class="sxs-lookup"><span data-stu-id="31190-155">Implementing error handling and error logging</span></span>

## <a name="overview"></a><span data-ttu-id="31190-156">Omówienie</span><span class="sxs-lookup"><span data-stu-id="31190-156">Overview</span></span>

<span data-ttu-id="31190-157">Ta seria samouczków jest przeznaczona dla kogoś, kto zna koncepcje programowania, ale nowy ASP.NET formularzy sieci Web.</span><span class="sxs-lookup"><span data-stu-id="31190-157">This tutorial series is intended for someone familiar with programming concepts, but new to ASP.NET Web Forms.</span></span> <span data-ttu-id="31190-158">Jeśli masz już doświadczenie w korzystaniu z formularzy sieci Web ASP.NET, ta seria może pomóc Ci w Poznaniu nowych funkcji ASP.NET 4,5.</span><span class="sxs-lookup"><span data-stu-id="31190-158">If you're already familiar with ASP.NET Web Forms, this series can still help you learn about new ASP.NET 4.5 features.</span></span> <span data-ttu-id="31190-159">W przypadku czytelników nieznających pojęć związanych z programowaniem i formularzy sieci Web ASP.NET zapoznaj się z samouczkami dotyczącymi dodatkowych formularzy sieci Web znajdujących się w sekcji [wprowadzenie](../../../index.md) w witrynie sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="31190-159">For readers unfamiliar with programming concepts and ASP.NET Web Forms, see the additional Web Forms tutorials provided in the [Getting Started](../../../index.md) section on the ASP.NET Web site.</span></span>

<span data-ttu-id="31190-160">ASP.NET 4,5 podany w tej serii samouczków obejmuje następujące funkcje:</span><span class="sxs-lookup"><span data-stu-id="31190-160">The ASP.NET 4.5 provided in this tutorial series includes the following features:</span></span>

- <span data-ttu-id="31190-161">Prosty interfejs użytkownika do tworzenia projektów, które oferują [obsługę wielu platform ASP.NET](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Forms, MVC i Web API).</span><span class="sxs-lookup"><span data-stu-id="31190-161">A simple UI for creating projects that offers [support for many ASP.NET frameworks](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Forms, MVC, and Web API).</span></span>
- <span data-ttu-id="31190-162">Struktura projektu [Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), układu, tworzenia i reagowania.</span><span class="sxs-lookup"><span data-stu-id="31190-162">[Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), a layout, theming, and responsive design framework.</span></span>
- <span data-ttu-id="31190-163">[ASP.NET Identity](../../../../identity/index.md)nowy system członkostwa ASP.NET, który działa tak samo we wszystkich strukturach ASP.NET i współpracuje z oprogramowaniem hostingu w sieci Web innym niż IIS.</span><span class="sxs-lookup"><span data-stu-id="31190-163">[ASP.NET Identity](../../../../identity/index.md), a new ASP.NET membership system that works the same in all ASP.NET frameworks and works with web hosting software other than IIS.</span></span>
- [<span data-ttu-id="31190-164">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="31190-164">Entity Framework 6</span></span>](https://msdn.microsoft.com/data/ef.aspx)

  <span data-ttu-id="31190-165">Aktualizacja Entity Framework umożliwiająca:</span><span class="sxs-lookup"><span data-stu-id="31190-165">An update to the Entity Framework enabling you to:</span></span>
  - <span data-ttu-id="31190-166">Pobieranie i manipulowanie danymi jako obiektami o jednoznacznie określonym typie</span><span class="sxs-lookup"><span data-stu-id="31190-166">Retrieve and manipulate data as strongly-typed objects</span></span>
  - <span data-ttu-id="31190-167">Dostęp do danych asynchronicznie</span><span class="sxs-lookup"><span data-stu-id="31190-167">Access data asynchronously</span></span>
  - <span data-ttu-id="31190-168">Obsługa błędów przejściowych połączeń</span><span class="sxs-lookup"><span data-stu-id="31190-168">Handle transient connection faults</span></span>
  - <span data-ttu-id="31190-169">Rejestruj instrukcje SQL</span><span class="sxs-lookup"><span data-stu-id="31190-169">Log SQL statements</span></span>

<span data-ttu-id="31190-170">Aby zapoznać się z pełną listą funkcji ASP.NET 4,5, zobacz [ASP.NET and Web Tools do Visual Studio 2013 informacji o wersji](../../../../visual-studio/overview/2013/release-notes.md).</span><span class="sxs-lookup"><span data-stu-id="31190-170">For a complete ASP.NET 4.5 feature list, see [ASP.NET and Web Tools for Visual Studio 2013 Release Notes](../../../../visual-studio/overview/2013/release-notes.md).</span></span>

### <a name="the-wingtip-toys-sample-application"></a><span data-ttu-id="31190-171">Przykładowa aplikacja Wingtip zabawki</span><span class="sxs-lookup"><span data-stu-id="31190-171">The Wingtip Toys sample application</span></span>

<span data-ttu-id="31190-172">Poniższe zrzuty ekranu pochodzą z aplikacji ASP.NET Web Forms utworzonej w tej serii samouczków.</span><span class="sxs-lookup"><span data-stu-id="31190-172">The following screenshots are from the ASP.NET Web Forms application that you create in this tutorial series.</span></span> <span data-ttu-id="31190-173">Po uruchomieniu aplikacji w programie Visual Studio zostanie wyświetlona następująca Strona główna sieci Web.</span><span class="sxs-lookup"><span data-stu-id="31190-173">When you run the application in Visual Studio, the following web Home page appears.</span></span>

![Zabawki Wingtip — strona domyślna](introduction-and-overview/_static/image1.png)

<span data-ttu-id="31190-175">Możesz zarejestrować się jako nowy użytkownik lub zalogować się jako istniejący użytkownik.</span><span class="sxs-lookup"><span data-stu-id="31190-175">You can register as a new user, or sign in as an existing user.</span></span> <span data-ttu-id="31190-176">Górna Nawigacja zawiera linki do kategorii produktów i ich produktów z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="31190-176">The top navigation has links to product categories and their products from the database.</span></span>

<span data-ttu-id="31190-177">W przypadku wybrania opcji **produkty**zostaną wyświetlone wszystkie dostępne produkty.</span><span class="sxs-lookup"><span data-stu-id="31190-177">If you select **Products**, all available products are displayed.</span></span> 

![Zabawki Wingtip — produkty](introduction-and-overview/_static/image2.png)

<span data-ttu-id="31190-179">W przypadku wybrania określonego produktu zostaną wyświetlone szczegóły produktu.</span><span class="sxs-lookup"><span data-stu-id="31190-179">If you select a specific product, product details are displayed.</span></span>

![Zabawki Wingtip — szczegóły produktu](introduction-and-overview/_static/image3.png)

<span data-ttu-id="31190-181">Jako użytkownik możesz zarejestrować i zalogować się przy użyciu domyślnych funkcji szablonu formularzy sieci Web.</span><span class="sxs-lookup"><span data-stu-id="31190-181">As a user, you can register and sign in with Web Forms template default functionality.</span></span> <span data-ttu-id="31190-182">W tym samouczku wyjaśniono również, jak zalogować się przy użyciu istniejącego konta usługi Gmail.</span><span class="sxs-lookup"><span data-stu-id="31190-182">This tutorial also explains how to sign in using an existing Gmail account.</span></span> <span data-ttu-id="31190-183">Ponadto możesz zalogować się jako administrator, aby dodawać i usuwać produkty z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="31190-183">Additionally, you can sign in as the administrator to add and remove products from the database.</span></span>

![Zabawki Wingtip — Zaloguj się](introduction-and-overview/_static/image4.png)

<span data-ttu-id="31190-185">Po zalogowaniu się jako użytkownik możesz dodać produkty do koszyka zakupów i wyewidencjonować je w systemie PayPal.</span><span class="sxs-lookup"><span data-stu-id="31190-185">Once you've signed in as a user, you can add products to the shopping cart and checkout with PayPal.</span></span> <span data-ttu-id="31190-186">Przykładowa aplikacja została zaprojektowana tak, aby działała w piaskownicy deweloperów w systemie PayPal.</span><span class="sxs-lookup"><span data-stu-id="31190-186">The sample application is designed to work in PayPal's developer sandbox.</span></span> <span data-ttu-id="31190-187">Nie ma rzeczywistej transakcji pieniężnej.</span><span class="sxs-lookup"><span data-stu-id="31190-187">No actual money transaction takes place.</span></span>

![Zabawki Wingtip — koszyk](introduction-and-overview/_static/image5.png)

<span data-ttu-id="31190-189">System PayPal potwierdza Twoje konto, zamówienie i informacje o płatności.</span><span class="sxs-lookup"><span data-stu-id="31190-189">PayPal confirms your account, order, and payment information.</span></span>

![Zabawki Wingtip — system PayPal](introduction-and-overview/_static/image6.png)

<span data-ttu-id="31190-191">Po powrocie z systemu PayPal możesz przejrzeć i zakończyć zamówienie.</span><span class="sxs-lookup"><span data-stu-id="31190-191">After returning from PayPal, you can review and complete your order.</span></span>

![Zabawki Wingtip — przegląd kolejności](introduction-and-overview/_static/image7.png)

## <a name="prerequisites"></a><span data-ttu-id="31190-193">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="31190-193">Prerequisites</span></span>

<span data-ttu-id="31190-194">Przed rozpoczęciem upewnij się, że na komputerze zainstalowano następujące oprogramowanie:</span><span class="sxs-lookup"><span data-stu-id="31190-194">Before you start, make sure the following software is installed on your computer:</span></span>

- <span data-ttu-id="31190-195">[Microsoft Visual Studio 2017 lub Microsoft Visual Studio Community 2017](https://visualstudio.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="31190-195">[Microsoft Visual Studio 2017 or Microsoft Visual Studio Community 2017](https://visualstudio.microsoft.com/downloads/).</span></span>

<span data-ttu-id="31190-196">.NET Framework jest instalowana automatycznie.</span><span class="sxs-lookup"><span data-stu-id="31190-196">The .NET Framework is installed automatically.</span></span>

<span data-ttu-id="31190-197">Ta seria samouczków używa Microsoft Visual Studio Community 2017.</span><span class="sxs-lookup"><span data-stu-id="31190-197">This tutorial series uses Microsoft Visual Studio Community 2017.</span></span> <span data-ttu-id="31190-198">Aby ukończyć tę serię samouczków, możesz użyć opcji lub Microsoft Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="31190-198">You can use either that or Microsoft Visual Studio 2017 to complete this tutorial series.</span></span>

<span data-ttu-id="31190-199">Zwróć uwagę na następujące informacje o programie Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="31190-199">Note the following about Visual Studio:</span></span>

* <span data-ttu-id="31190-200">W tej serii samouczków Microsoft Visual Studio 2017 i Microsoft Visual Studio Community 2017 są określane jako *Visual Studio* .</span><span class="sxs-lookup"><span data-stu-id="31190-200">Microsoft Visual Studio 2017 and Microsoft Visual Studio Community 2017 are referred to as *Visual Studio* throughout this tutorial series.</span></span>

* <span data-ttu-id="31190-201">Program Visual Studio 2017 jest instalowany obok wszystkich starszych wersji, które są już zainstalowane.</span><span class="sxs-lookup"><span data-stu-id="31190-201">Visual Studio 2017 is installed next to any older versions already installed.</span></span> <span data-ttu-id="31190-202">Lokacje utworzone we wcześniejszych wersjach mogą być otwierane w programie Visual Studio 2017 i nadal otwierane w poprzednich wersjach.</span><span class="sxs-lookup"><span data-stu-id="31190-202">Sites created in earlier versions can be opened in Visual Studio 2017 and continue to open in previous versions.</span></span>

* <span data-ttu-id="31190-203">Przy pierwszym uruchomieniu programu Visual Studio przyjęto założenie, że wybrano ustawienia *deweloperskie sieci Web* .</span><span class="sxs-lookup"><span data-stu-id="31190-203">The first time you started Visual Studio, it is assumed you selected the *Web Development* settings.</span></span> <span data-ttu-id="31190-204">Aby uzyskać więcej informacji, zobacz [How to: SELECT Web Development Environment Settings](https://msdn.microsoft.com/library/ff521558.aspx).</span><span class="sxs-lookup"><span data-stu-id="31190-204">For more information, see [How to: Select Web Development Environment Settings](https://msdn.microsoft.com/library/ff521558.aspx).</span></span>

<span data-ttu-id="31190-205">Po zainstalowaniu wymagań wstępnych możesz zacząć tworzyć projekt sieci Web przedstawiony w tej serii samouczków.</span><span class="sxs-lookup"><span data-stu-id="31190-205">After installing the prerequisites, you're ready to begin creating the Web project presented in this tutorial series.</span></span>

## <a name="download-the-sample-application"></a><span data-ttu-id="31190-206">Pobieranie przykładowej aplikacji</span><span class="sxs-lookup"><span data-stu-id="31190-206">Download the sample application</span></span>

 <span data-ttu-id="31190-207">Ukończoną przykładową aplikację można pobrać w dowolnym momencie z witryny przykładów MSDN:</span><span class="sxs-lookup"><span data-stu-id="31190-207">You can download the completed sample application at anytime from the MSDN Samples site:</span></span>

<span data-ttu-id="31190-208">[Wprowadzenie z formularzami sieci Web ASP.NET 4,5 i zabawkami Visual Studio 2013-Wingtip](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span><span class="sxs-lookup"><span data-stu-id="31190-208">[Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span></span> 

 <span data-ttu-id="31190-209">Ten pobieranie zawiera następujące elementy:</span><span class="sxs-lookup"><span data-stu-id="31190-209">This download has the following items:</span></span>

- <span data-ttu-id="31190-210">Przykładowa aplikacja w folderze *WingtipToys* .</span><span class="sxs-lookup"><span data-stu-id="31190-210">The sample application in the *WingtipToys* folder.</span></span>
- <span data-ttu-id="31190-211">Zasoby używane do tworzenia przykładowej aplikacji w folderze *WingtipToys-Assets* w folderze *WingtipToys* .</span><span class="sxs-lookup"><span data-stu-id="31190-211">The resources used to create the sample application in the *WingtipToys-Assets* folder in the *WingtipToys* folder.</span></span>

<span data-ttu-id="31190-212">Pobranie to plik *. zip* .</span><span class="sxs-lookup"><span data-stu-id="31190-212">The download is a *.zip* file.</span></span> <span data-ttu-id="31190-213">Aby wyświetlić ukończony Projekt tworzony przez tę serię samouczków, Znajdź i wybierz *C#* folder w pliku zip.</span><span class="sxs-lookup"><span data-stu-id="31190-213">To see the completed project that this tutorial series creates, find and select the *C#* folder in the .zip file.</span></span> <span data-ttu-id="31190-214">Zapisz C# folder w folderze używanym do pracy z projektami programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="31190-214">Save the C# folder to the folder you use to work with Visual Studio projects.</span></span> <span data-ttu-id="31190-215">Domyślnie folder projektów programu Visual Studio 2017 to:</span><span class="sxs-lookup"><span data-stu-id="31190-215">By default, the Visual Studio 2017 projects folder is:</span></span>

<span data-ttu-id="31190-216"><strong>C:\Users&#92;</strong>  <strong><em>&lt;username&gt;</em></strong> <strong>\source\repos</strong></span><span class="sxs-lookup"><span data-stu-id="31190-216"><strong>C:\Users&#92;</strong><strong><em>&lt;username&gt;</em></strong><strong>\source\repos</strong></span></span>

<span data-ttu-id="31190-217">Zmień nazwę ***C#*** folderu na ***WingtipToys***.</span><span class="sxs-lookup"><span data-stu-id="31190-217">Rename the ***C#*** folder to ***WingtipToys***.</span></span>

> [!NOTE]
> <span data-ttu-id="31190-218">Jeśli masz już folder o nazwie *WingtipToys* w folderze projekty, tymczasowo Zmień nazwę istniejącego folderu przed zmianą nazwy *C#* folderu na *WingtipToys*.</span><span class="sxs-lookup"><span data-stu-id="31190-218">If you already have a folder named *WingtipToys* in your Projects folder, temporarily rename that existing folder before renaming the *C#* folder to *WingtipToys*.</span></span>

<span data-ttu-id="31190-219">Aby uruchomić ukończony projekt, Otwórz folder *WingtipToys* i kliknij dwukrotnie plik *WingtipToys. sln* .</span><span class="sxs-lookup"><span data-stu-id="31190-219">To run the completed project, open the *WingtipToys* folder and double-click the *WingtipToys.sln* file.</span></span> <span data-ttu-id="31190-220">Program Visual Studio 2017 otwiera projekt.</span><span class="sxs-lookup"><span data-stu-id="31190-220">Visual Studio 2017 opens the project.</span></span> <span data-ttu-id="31190-221">Następnie kliknij prawym przyciskiem myszy plik *default. aspx* w **Eksplorator rozwiązań** i wybierz pozycję **Widok w przeglądarce**.</span><span class="sxs-lookup"><span data-stu-id="31190-221">Next, right-click the *Default.aspx* file in **Solution Explorer** and select **View In Browser**.</span></span>

## <a name="take-a-aspnet-web-forms-quiz-to-review-content"></a><span data-ttu-id="31190-222">Skorzystaj z quizu formularzy sieci Web ASP.NET, aby przejrzeć zawartość</span><span class="sxs-lookup"><span data-stu-id="31190-222">Take a ASP.NET Web Forms quiz to review content</span></span>

<span data-ttu-id="31190-223">Po zakończeniu serii samouczków zapoznaj się z quizem, aby przetestować swoją wiedzę i wzmocnić kluczowe pojęcia.</span><span class="sxs-lookup"><span data-stu-id="31190-223">After completing the tutorial series, take a quiz to test your knowledge and reinforce key concepts.</span></span> <span data-ttu-id="31190-224">Każde pytanie zawiera wyjaśnienie i linki do dodatkowych wskazówek.</span><span class="sxs-lookup"><span data-stu-id="31190-224">Each question provides an explanation and links to additional guidance.</span></span>

* [<span data-ttu-id="31190-225">Quiz formularzy sieci Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="31190-225">ASP.NET Web Forms Quiz</span></span>](https://blogs.msdn.microsoft.com/erikreitan/2016/01/08/asp-net-web-forms-quiz/) 

## <a name="tutorial-support-and-comments"></a><span data-ttu-id="31190-226">Obsługa samouczków i komentarzy</span><span class="sxs-lookup"><span data-stu-id="31190-226">Tutorial support and comments</span></span>

<span data-ttu-id="31190-227">Pytania i Komentarze można znaleźć w sekcji pytań i odpowiedzi, które znajdują się na [wprowadzenie za pomocą formularzy sieci Web ASP.NET 4,5 i Visual Studio 2013-Wingtip zabawki](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#).</span><span class="sxs-lookup"><span data-stu-id="31190-227">For questions and comments, use the Q and A section included on the [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) sample page.</span></span>

<span data-ttu-id="31190-228">Komentarze do tej serii samouczków są powitane.</span><span class="sxs-lookup"><span data-stu-id="31190-228">Comments on this tutorial series are welcome.</span></span> <span data-ttu-id="31190-229">Gdy ta seria samouczków zostanie zaktualizowana, należy wziąć pod uwagę poprawki lub sugestie dotyczące ulepszeń.</span><span class="sxs-lookup"><span data-stu-id="31190-229">When this tutorial series is updated, every effort is made to consider corrections or suggestions for improvements.</span></span>

<span data-ttu-id="31190-230">Jeśli wystąpi błąd, odpowiednie komunikaty o błędach mogą być mylące i nie są dobrym wyjaśnieniem, jak to naprawić.</span><span class="sxs-lookup"><span data-stu-id="31190-230">If an error occurs, the corresponding error messages could be confusing, with no good explanation on how to fix it.</span></span> <span data-ttu-id="31190-231">Aby uzyskać pomoc, możesz sprawdzić [fora ASP.NET](https://forums.asp.net/).</span><span class="sxs-lookup"><span data-stu-id="31190-231">For help, you can check the [ASP.NET forums](https://forums.asp.net/).</span></span> <span data-ttu-id="31190-232">Innym dobrym źródłem jest sekcja pytań i odpowiedzi w [wprowadzenie za pomocą formularzy sieci Web ASP.NET 4,5 i Visual Studio 2013-Wingtip zabawki](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#).</span><span class="sxs-lookup"><span data-stu-id="31190-232">Another good source is the Q and A section in the [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) sample page.</span></span> 

> [!div class="step-by-step"]
> [<span data-ttu-id="31190-233">Dalej</span><span class="sxs-lookup"><span data-stu-id="31190-233">Next</span></span>](create-the-project.md)

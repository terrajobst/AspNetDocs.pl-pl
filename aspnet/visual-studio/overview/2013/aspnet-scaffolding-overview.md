---
uid: visual-studio/overview/2013/aspnet-scaffolding-overview
title: Tworzenie szkieletu ASP.NET w Visual Studio 2013 | Microsoft Docs
author: Rick-Anderson
description: Tworzenie szkieletu ASP.NET to nowa funkcja, która jest zawarta w Visual Studio 2013.
ms.author: riande
ms.date: 04/09/2014
ms.assetid: a41ec9d4-8287-4f31-9e2a-460e7b7f04be
msc.legacyurl: /visual-studio/overview/2013/aspnet-scaffolding-overview
msc.type: authoredcontent
ms.openlocfilehash: cf4669b769cee28475e2dd6a6ddf07ea1434d04d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557982"
---
# <a name="aspnet-scaffolding-in-visual-studio-2013"></a><span data-ttu-id="825d3-103">Funkcja tworzenia szkieletu ASP.NET w programie Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="825d3-103">ASP.NET Scaffolding in Visual Studio 2013</span></span>

<span data-ttu-id="825d3-104">Autor [FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="825d3-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="825d3-105">Tworzenie szkieletu ASP.NET to nowa funkcja, która jest zawarta w Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="825d3-105">ASP.NET Scaffolding is a new feature that is included in Visual Studio 2013.</span></span>

## <a name="overview"></a><span data-ttu-id="825d3-106">Omówienie</span><span class="sxs-lookup"><span data-stu-id="825d3-106">Overview</span></span>

<span data-ttu-id="825d3-107">Tworzenie szkieletów ASP.NET jest strukturą generowania kodu dla aplikacji sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="825d3-107">ASP.NET Scaffolding is a code generation framework for ASP.NET Web applications.</span></span> <span data-ttu-id="825d3-108">Visual Studio 2013 obejmuje wstępnie zainstalowane generatory kodu dla projektów MVC i Web API.</span><span class="sxs-lookup"><span data-stu-id="825d3-108">Visual Studio 2013 includes pre-installed code generators for MVC and Web API projects.</span></span> <span data-ttu-id="825d3-109">Dodawanie szkieletu do projektu, gdy chcesz szybko dodać kod, który współdziała z modelami danych.</span><span class="sxs-lookup"><span data-stu-id="825d3-109">You add scaffolding to your project when you want to quickly add code that interacts with data models.</span></span> <span data-ttu-id="825d3-110">Użycie szkieletu może skrócić czas projektowania standardowych operacji na danych w projekcie.</span><span class="sxs-lookup"><span data-stu-id="825d3-110">Using scaffolding can reduce the amount of time to develop standard data operations in your project.</span></span>

<span data-ttu-id="825d3-111">Domyślnie Visual Studio 2013 nie obsługuje generowania kodu dla projektu formularzy sieci Web, ale można użyć szkieletów z formularzami sieci Web przez dodanie zależności MVC do projektu lub zainstalowanie rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="825d3-111">By default, Visual Studio 2013 does not support generating code for a Web Forms project, but you can use scaffolding with Web Forms by either adding MVC dependencies to the project or installing an extension.</span></span> <span data-ttu-id="825d3-112">Poniżej przedstawiono oba podejścia.</span><span class="sxs-lookup"><span data-stu-id="825d3-112">Both approaches are shown below.</span></span>

<span data-ttu-id="825d3-113">Visual Studio 2013 Update 2 (obecnie RC) zapewnia możliwość rozbudowania szkieletu ASP.NET w celu spełnienia wymagań danego scenariusza.</span><span class="sxs-lookup"><span data-stu-id="825d3-113">Visual Studio 2013 Update 2 (currently RC) provides the ability to extend ASP.NET Scaffolding to meet the requirements of your scenario.</span></span> <span data-ttu-id="825d3-114">Za pomocą tej funkcji można utworzyć dostosowany szablon tworzenia szkieletu i dodać go do okna dialogowego Dodawanie nowej szkieletu.</span><span class="sxs-lookup"><span data-stu-id="825d3-114">With this functionality, you can create a customized scaffolding template and add it to Add New Scaffold dialog.</span></span> <span data-ttu-id="825d3-115">W ramach niestandardowego szablonu należy określić kod, który jest generowany podczas dodawania szkieletu elementu.</span><span class="sxs-lookup"><span data-stu-id="825d3-115">Within the customized template, you specify the code that is generated when adding a scaffolded item.</span></span> <span data-ttu-id="825d3-116">Aby uzyskać więcej informacji, zobacz [Tworzenie niestandardowego szkieletu dla programu Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span><span class="sxs-lookup"><span data-stu-id="825d3-116">For more information, see [Creating a Custom Scaffolder for Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="825d3-117">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="825d3-117">Prerequisites</span></span>

<span data-ttu-id="825d3-118">Aby korzystać z szkieletu ASP.NET, musisz mieć:</span><span class="sxs-lookup"><span data-stu-id="825d3-118">To use ASP.NET Scaffolding, you must have:</span></span>

- <span data-ttu-id="825d3-119">Microsoft Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="825d3-119">Microsoft Visual Studio 2013</span></span>
- <span data-ttu-id="825d3-120">Narzędzia deweloperskie sieci Web (część domyślnej instalacji Visual Studio 2013)</span><span class="sxs-lookup"><span data-stu-id="825d3-120">Web Developer Tools (part of default Visual Studio 2013 installation)</span></span>
- <span data-ttu-id="825d3-121">ASP.NET Web Frameworks and Tools 2013 (część domyślnej instalacji Visual Studio 2013)</span><span class="sxs-lookup"><span data-stu-id="825d3-121">ASP.NET Web Frameworks and Tools 2013 (part of default Visual Studio 2013 installation)</span></span>

## <a name="add-a-scaffolded-item-to-mvc-or-web-api"></a><span data-ttu-id="825d3-122">Dodaj element szkieletowy do MVC lub interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="825d3-122">Add a scaffolded item to MVC or Web API</span></span>

<span data-ttu-id="825d3-123">Aby dodać szkielet, kliknij prawym przyciskiem myszy projekt lub folder w projekcie, a następnie wybierz polecenie **Dodaj** — **nowy element szkieletowy**, jak pokazano na poniższej ilustracji.</span><span class="sxs-lookup"><span data-stu-id="825d3-123">To add a scaffold, right-click either the project or a folder within the project, and select **Add** – **New Scaffolded Item**, as shown in the following image.</span></span>

![Dodaj element szkieletu](aspnet-scaffolding-overview/_static/image1.png)

<span data-ttu-id="825d3-125">W oknie **Dodawanie szkieletu** wybierz typ szkieletu do dodania.</span><span class="sxs-lookup"><span data-stu-id="825d3-125">From the **Add Scaffold** window, select the type of scaffold to add.</span></span>

![Wybierz typ szkieletu](aspnet-scaffolding-overview/_static/image2.png)

<span data-ttu-id="825d3-127">Okno **Dodawanie kontrolera** umożliwia wybranie opcji związanych z generowaniem kontrolera, w tym o tym, czy mają być używane nowe funkcje asynchroniczne z Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="825d3-127">The **Add Controller** window gives you the opportunity to select options for generating the controller, including whether you want to use the new async features from Entity Framework 6.</span></span>

![Dodaj kontroler](aspnet-scaffolding-overview/_static/image3.png)

<span data-ttu-id="825d3-129">Dla danego scenariusza tworzone są odpowiednie klasy i strony.</span><span class="sxs-lookup"><span data-stu-id="825d3-129">The relevant classes and pages are created for your scenario.</span></span> <span data-ttu-id="825d3-130">Na przykład na poniższej ilustracji przedstawiono kontroler MVC i widoki, które zostały utworzone za pomocą szkieletów dla klasy modelu o nazwie filmy.</span><span class="sxs-lookup"><span data-stu-id="825d3-130">For example, the following image shows the MVC controller and views that were created through scaffolding for a model class named Movies.</span></span>

![Utworzone pliki](aspnet-scaffolding-overview/_static/image4.png)

## <a name="add-a-scaffolded-item-to-web-forms"></a><span data-ttu-id="825d3-132">Dodawanie elementu szkieletowego do formularzy sieci Web</span><span class="sxs-lookup"><span data-stu-id="825d3-132">Add a scaffolded item to Web Forms</span></span>

<span data-ttu-id="825d3-133">Aby dodać szkielet, który generuje kod formularzy sieci Web, należy zainstalować rozszerzenie programu Visual Studio lub dodać zależności MVC.</span><span class="sxs-lookup"><span data-stu-id="825d3-133">To add scaffolding that generates Web Forms code, you must either install an extension to Visual Studio or add MVC dependencies.</span></span> <span data-ttu-id="825d3-134">Poniżej przedstawiono oba podejścia, ale należy wykonać tylko jedną z tych metod.</span><span class="sxs-lookup"><span data-stu-id="825d3-134">Both approaches are shown below, but you only need to do one of these approaches.</span></span>

### <a name="web-forms-scaffolding-extension"></a><span data-ttu-id="825d3-135">Rozszerzenie szkieletu formularzy sieci Web</span><span class="sxs-lookup"><span data-stu-id="825d3-135">Web Forms Scaffolding Extension</span></span>

<span data-ttu-id="825d3-136">Można zainstalować rozszerzenie programu Visual Studio, które umożliwia korzystanie z szkieletów z projektem formularzy sieci Web.</span><span class="sxs-lookup"><span data-stu-id="825d3-136">You can install a Visual Studio extension that enable you to use scaffolding with a Web Forms project.</span></span> <span data-ttu-id="825d3-137">W programie Visual Studio wybierz pozycje **Narzędzia** , a następnie **rozszerzenia i aktualizacje**.</span><span class="sxs-lookup"><span data-stu-id="825d3-137">In Visual Studio, select **Tools** and then **Extensions and Updates**.</span></span> <span data-ttu-id="825d3-138">Z tego okna dialogowego Przeszukaj galerię programu Visual Studio dla **szkieletu formularzy sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="825d3-138">From this dialog search the Visual Studio Gallery for **Web Forms Scaffolding**.</span></span>

![Instalowanie szkieletu formularzy sieci Web](aspnet-scaffolding-overview/_static/image5.png)

<span data-ttu-id="825d3-140">Aby uzyskać więcej informacji, zobacz Tworzenie [szkieletu formularzy sieci Web](https://go.microsoft.com/fwlink/p/?LinkId=396478).</span><span class="sxs-lookup"><span data-stu-id="825d3-140">For more information, see [Web Forms Scaffolding](https://go.microsoft.com/fwlink/p/?LinkId=396478).</span></span>

### <a name="mvc-dependencies"></a><span data-ttu-id="825d3-141">Zależności MVC</span><span class="sxs-lookup"><span data-stu-id="825d3-141">MVC Dependencies</span></span>

<span data-ttu-id="825d3-142">Aby dodać zależności MVC, wybierz pozycję **dodaj** - **nowy element szkieletowy**.</span><span class="sxs-lookup"><span data-stu-id="825d3-142">To add MVC dependencies, select **Add** - **New Scaffolded Item**.</span></span> <span data-ttu-id="825d3-143">W oknie Dodawanie szkieletu wybierz pozycję **zależności MVC**, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="825d3-143">In the Add Scaffold window, select **MVC Dependencies**, as shown below.</span></span>

![Dodawanie zależności MVC](aspnet-scaffolding-overview/_static/image6.png)

<span data-ttu-id="825d3-145">Dostępne są dwie opcje tworzenia szkieletów MVC; Minimalny i pełny.</span><span class="sxs-lookup"><span data-stu-id="825d3-145">There are two options for scaffolding MVC; Minimal and Full.</span></span> <span data-ttu-id="825d3-146">W przypadku wybrania opcji minimalny tylko pakiety NuGet i odwołania dla ASP.NET MVC zostaną dodane do projektu.</span><span class="sxs-lookup"><span data-stu-id="825d3-146">If you select Minimal, only the NuGet packages and references for ASP.NET MVC are added to your project.</span></span> <span data-ttu-id="825d3-147">W przypadku wybrania opcji Pełna są dodawane minimalne zależności, a także wymagane pliki zawartości dla projektu MVC.</span><span class="sxs-lookup"><span data-stu-id="825d3-147">If you select the Full option, the Minimal dependencies are added, as well as the required content files for an MVC project.</span></span> <span data-ttu-id="825d3-148">Aby łatwo korzystać z szkieletów, wybierz pozycję pełne zależności.</span><span class="sxs-lookup"><span data-stu-id="825d3-148">To easily use scaffolding, select Full dependencies.</span></span>

![Wybierz pełne zależności](aspnet-scaffolding-overview/_static/image7.png)

<span data-ttu-id="825d3-150">Po dodaniu zależności zobaczysz plik **README. txt** .</span><span class="sxs-lookup"><span data-stu-id="825d3-150">After adding the dependencies, you will see a **readme.txt** file.</span></span> <span data-ttu-id="825d3-151">Uważnie postępuj zgodnie z instrukcjami w tym pliku, aby upewnić się, że projekt działa prawidłowo.</span><span class="sxs-lookup"><span data-stu-id="825d3-151">Carefully follow the instructions in this file to ensure that your project works correctly.</span></span>

<span data-ttu-id="825d3-152">Po wykonaniu kroków opisanych w pliku Readme. txt można dodać nowy element szkieletowy, jak pokazano w poprzedniej sekcji dotyczącej składnika MVC i interfejsu Web API.</span><span class="sxs-lookup"><span data-stu-id="825d3-152">When you have completed the steps in the readme.txt file, you can add a new scaffolded item as shown in the previous section about MVC and Web API.</span></span> <span data-ttu-id="825d3-153">Automatycznie generowane widoki i kontroler będą działać prawidłowo w ramach projektu.</span><span class="sxs-lookup"><span data-stu-id="825d3-153">The automatically-generated views and controller will function correctly within your project.</span></span>

## <a name="tutorials"></a><span data-ttu-id="825d3-154">Samouczki</span><span class="sxs-lookup"><span data-stu-id="825d3-154">Tutorials</span></span>

<span data-ttu-id="825d3-155">Aby utworzyć dostosowany szkielet, zobacz [Tworzenie niestandardowego szkieletu dla programu Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span><span class="sxs-lookup"><span data-stu-id="825d3-155">To create a customized scaffolder, see [Creating a Custom Scaffolder for Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span></span>

<span data-ttu-id="825d3-156">Aby dostosować wygenerowane pliki, zobacz [How to Dostosowywanie wygenerowanych plików z okna dialogowego Nowy element szkieletowy](https://blogs.msdn.com/b/webdev/archive/2013/12/26/how-to-customize-the-generated-files-from-the-new-scaffolded-item-dialog.aspx).</span><span class="sxs-lookup"><span data-stu-id="825d3-156">To customize the generated files, see [How to customize the generated files from the New Scaffolded Item dialog](https://blogs.msdn.com/b/webdev/archive/2013/12/26/how-to-customize-the-generated-files-from-the-new-scaffolded-item-dialog.aspx).</span></span>

<span data-ttu-id="825d3-157">Aby zapoznać się z przykładem korzystania z szkieletów z **programowaniem Database First**, zobacz [EF Database First z ASP.NET MVC](../../../mvc/overview/getting-started/database-first-development/setting-up-database.md).</span><span class="sxs-lookup"><span data-stu-id="825d3-157">For an example of using scaffolding with **Database First development**, see [EF Database First with ASP.NET MVC](../../../mvc/overview/getting-started/database-first-development/setting-up-database.md).</span></span>

<span data-ttu-id="825d3-158">Aby zapoznać się z przykładem użycia szkieletu w projekcie **MVC** , zobacz [wprowadzenie with ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="825d3-158">For an example of using scaffolding in an **MVC** project, see [Getting Started with ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span></span>

<span data-ttu-id="825d3-159">Aby zapoznać się z przykładem użycia szkieletu w projekcie **interfejsu API sieci Web** , zobacz [Tworzenie interfejsu API REST z routingiem atrybutów w interfejsie Web API 2](../../../web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing.md).</span><span class="sxs-lookup"><span data-stu-id="825d3-159">For an example of using scaffolding in a **Web API** project, see [Create a REST API with Attribute Routing in Web API 2](../../../web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing.md).</span></span>

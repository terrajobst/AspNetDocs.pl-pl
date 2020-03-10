---
uid: visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
title: Informacje o wersji dla ASP.NET and Web Tools 2013,1 dla programu Visual Studio 2012 | Microsoft Docs
author: microsoft
description: W tym dokumencie opisano wydanie ASP.NET and Web Tools 2013,1 dla programu Visual Studio 2012.
ms.author: riande
ms.date: 11/13/2013
ms.assetid: ca26e5bb-630e-41d2-8512-2a9386c431cb
msc.legacyurl: /visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: 260af1018064d60d80cbb1002001f28c4daffffd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78578443"
---
# <a name="release-notes-for-aspnet-and-web-tools-20131-for-visual-studio-2012"></a><span data-ttu-id="f7125-103">Informacje o wersji rozszerzenia ASP.NET and Web Tools 2013.1 dla programu Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="f7125-103">Release Notes for ASP.NET and Web Tools 2013.1 for Visual Studio 2012</span></span>

<span data-ttu-id="f7125-104">przez [firmę Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="f7125-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="f7125-105">W tym dokumencie opisano wydanie ASP.NET and Web Tools 2013,1 dla programu Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="f7125-105">This document describes the release of ASP.NET and Web Tools 2013.1 for Visual Studio 2012.</span></span>

## <a name="contents"></a><span data-ttu-id="f7125-106">Spis treści</span><span class="sxs-lookup"><span data-stu-id="f7125-106">Contents</span></span>

- [<span data-ttu-id="f7125-107">Uwagi dotyczące instalacji</span><span class="sxs-lookup"><span data-stu-id="f7125-107">Installation Notes</span></span>](#install)
- [<span data-ttu-id="f7125-108">Wymagania dotyczące oprogramowania</span><span class="sxs-lookup"><span data-stu-id="f7125-108">Software Requirements</span></span>](#requirements)
- <span data-ttu-id="f7125-109">Nowe funkcje w ASP.NET and Web Tools 2013,1 dla programu Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="f7125-109">New Features in ASP.NET and Web Tools 2013.1 for Visual Studio 2012</span></span>

    - [<span data-ttu-id="f7125-110">Bootstrap</span><span class="sxs-lookup"><span data-stu-id="f7125-110">Bootstrap</span></span>](#bootstrap)
    - [<span data-ttu-id="f7125-111">Szablony</span><span class="sxs-lookup"><span data-stu-id="f7125-111">Templates</span></span>](#templates)

        - [<span data-ttu-id="f7125-112">Szablon ASP.NET MVC 5</span><span class="sxs-lookup"><span data-stu-id="f7125-112">ASP.NET MVC 5 template</span></span>](#mvc5template)
        - [<span data-ttu-id="f7125-113">Szablon ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="f7125-113">ASP.NET Web API 2 template</span></span>](#apitemplate)
        - [<span data-ttu-id="f7125-114">Szablony elementów</span><span class="sxs-lookup"><span data-stu-id="f7125-114">Item Templates</span></span>](#itemtemplate)
    - [<span data-ttu-id="f7125-115">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="f7125-115">Entity Framework 6</span></span>](#ef6)
    - [<span data-ttu-id="f7125-116">Tworzenie szkieletu ASP.NET</span><span class="sxs-lookup"><span data-stu-id="f7125-116">ASP.NET Scaffolding</span></span>](#scaffold)
    - [<span data-ttu-id="f7125-117">Edytor Razor</span><span class="sxs-lookup"><span data-stu-id="f7125-117">Razor Editor</span></span>](#razor)
    - [<span data-ttu-id="f7125-118">NuGet 2.7</span><span class="sxs-lookup"><span data-stu-id="f7125-118">NuGet 2.7</span></span>](#nuget)
- <span data-ttu-id="f7125-119">Znane problemy i istotne zmiany</span><span class="sxs-lookup"><span data-stu-id="f7125-119">Known Issues and Breaking Changes</span></span>

    - [<span data-ttu-id="f7125-120">Tworzenie szkieletu ASP.NET</span><span class="sxs-lookup"><span data-stu-id="f7125-120">ASP.NET Scaffolding</span></span>](#issuescaffolding)

        - [<span data-ttu-id="f7125-121">Tworzenie szkieletu kodu MVC i interfejsu API sieci Web-HTTP 404, nie znaleziono błędu</span><span class="sxs-lookup"><span data-stu-id="f7125-121">MVC and Web API Scaffolding - HTTP 404, Not Found error</span></span>](#404issue)
        - [<span data-ttu-id="f7125-122">Visual Studio Express 2012 dla sieci Web przestaje działać po dodaniu szkieletu elementu</span><span class="sxs-lookup"><span data-stu-id="f7125-122">Visual Studio Express 2012 for Web stops working after adding a scaffolded item</span></span>](#expressissue)
    - [<span data-ttu-id="f7125-123">ASP.NET Razor 3</span><span class="sxs-lookup"><span data-stu-id="f7125-123">ASP.NET Razor 3</span></span>](#issuerazor)

        - [<span data-ttu-id="f7125-124">Wyświetlanie pliku cshtml z przeglądaniem lub F5 powoduje błąd serwera</span><span class="sxs-lookup"><span data-stu-id="f7125-124">Viewing cshtml file with Browse With or F5 causes a server error</span></span>](#browseissue)
        - [<span data-ttu-id="f7125-125">Ponowne zapisywanie adresów URL i Tylda (~)</span><span class="sxs-lookup"><span data-stu-id="f7125-125">Url Rewrite and Tilde(~)</span></span>](#rewriteissue)
    - [<span data-ttu-id="f7125-126">Szablony</span><span class="sxs-lookup"><span data-stu-id="f7125-126">Templates</span></span>](#templateissue)

<a id="install"></a>
## <a name="installation-notes"></a><span data-ttu-id="f7125-127">Uwagi dotyczące instalacji</span><span class="sxs-lookup"><span data-stu-id="f7125-127">Installation Notes</span></span>

<span data-ttu-id="f7125-128">[Zainstaluj](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WebNode11Pack.appids) program ASP.NET and Web Tools 2013,1 dla programu Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="f7125-128">[Install](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WebNode11Pack.appids) ASP.NET and Web Tools 2013.1 for Visual Studio 2012.</span></span>

<a id="requirements"></a>
## <a name="software-requirements"></a><span data-ttu-id="f7125-129">Wymagania programowe</span><span class="sxs-lookup"><span data-stu-id="f7125-129">Software Requirements</span></span>

<span data-ttu-id="f7125-130">Musisz mieć program Visual Studio 2012 lub Visual Studio Express 2012 dla sieci Web.</span><span class="sxs-lookup"><span data-stu-id="f7125-130">You must have either Visual Studio 2012 or Visual Studio Express 2012 for Web.</span></span>

## <a name="new-features-in-aspnet-and-web-tools-20131-for-visual-studio-2012"></a><span data-ttu-id="f7125-131">Nowe funkcje w ASP.NET and Web Tools 2013,1 dla programu Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="f7125-131">New Features in ASP.NET and Web Tools 2013.1 for Visual Studio 2012</span></span>

<a id="bootstrap"></a>
### <a name="bootstrap"></a><span data-ttu-id="f7125-132">Bootstrap</span><span class="sxs-lookup"><span data-stu-id="f7125-132">Bootstrap</span></span>

<span data-ttu-id="f7125-133">W przypadku tworzenia szkieletów kontrolerów i widoków MVC 5 znaczniki dla widoków używają [ładowania początkowego](http://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="f7125-133">When you scaffold MVC 5 controllers and views, the markup for the views uses [Bootstrap](http://getbootstrap.com/).</span></span>

<a id="templates"></a>
### <a name="templates"></a><span data-ttu-id="f7125-134">Szablony</span><span class="sxs-lookup"><span data-stu-id="f7125-134">Templates</span></span>

<a id="mvc5template"></a>
#### <a name="aspnet-mvc-5-template"></a><span data-ttu-id="f7125-135">Szablon ASP.NET MVC 5</span><span class="sxs-lookup"><span data-stu-id="f7125-135">ASP.NET MVC 5 template</span></span>

<span data-ttu-id="f7125-136">Dodaliśmy nowy szablon MVC 5.</span><span class="sxs-lookup"><span data-stu-id="f7125-136">We added a new MVC 5 template.</span></span> <span data-ttu-id="f7125-137">Odwołuje się do najnowszych pakietów NuGet MVC 5 i można użyć szkieletu do dodawania kontrolerów i widoków.</span><span class="sxs-lookup"><span data-stu-id="f7125-137">It references the latest MVC 5 NuGet packages, and you can use scaffolding to add controllers and views.</span></span>

<a id="apitemplate"></a>
#### <a name="aspnet-web-api-2-template"></a><span data-ttu-id="f7125-138">Szablon ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="f7125-138">ASP.NET Web API 2 template</span></span>

<span data-ttu-id="f7125-139">Dodaliśmy nowy szablon interfejsu Web API 2.</span><span class="sxs-lookup"><span data-stu-id="f7125-139">We added a new Web API 2 template.</span></span> <span data-ttu-id="f7125-140">Odwołuje się do najnowszych pakietów NuGet interfejsu Web API 2 i można użyć szkieletu do dodawania kontrolerów i widoków.</span><span class="sxs-lookup"><span data-stu-id="f7125-140">It references the latest Web API 2 NuGet packages, and you can use scaffolding to add controllers and views.</span></span>

<a id="itemtemplate"></a>
#### <a name="item-templates"></a><span data-ttu-id="f7125-141">Szablony elementów</span><span class="sxs-lookup"><span data-stu-id="f7125-141">Item Templates</span></span>

<span data-ttu-id="f7125-142">Dodaliśmy nowe szablony elementów dla widoków MVC 5, Web Pages (Razor 3) i Web API 2 controllers.</span><span class="sxs-lookup"><span data-stu-id="f7125-142">We added new item templates for MVC 5 views, Web Pages (Razor 3), and Web API 2 controllers.</span></span> <span data-ttu-id="f7125-143">Instalują one powiązane pakiety NuGet do projektu podczas dodawania nowych elementów.</span><span class="sxs-lookup"><span data-stu-id="f7125-143">They install the related NuGet packages to the project while adding new items.</span></span>

<a id="ef6"></a>
### <a name="entity-framework-6"></a><span data-ttu-id="f7125-144">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="f7125-144">Entity Framework 6</span></span>

<span data-ttu-id="f7125-145">W przypadku tworzenia szkieletu kontrolera MVC lub interfejsu API sieci Web przy użyciu Entity Framework używany jest program Framework 6.</span><span class="sxs-lookup"><span data-stu-id="f7125-145">When you scaffold an MVC or Web API controller using Entity Framework, we use Framework 6.</span></span> <span data-ttu-id="f7125-146">Aby uzyskać więcej informacji na temat Entity Framework, zobacz [Entity Framework historię wersji](https://msdn.com/data/jj574253).</span><span class="sxs-lookup"><span data-stu-id="f7125-146">For more information about Entity Framework, see the [Entity Framework Version History](https://msdn.com/data/jj574253).</span></span>

<span data-ttu-id="f7125-147">Możesz również pobrać i zainstalować Entity Framework 6 Tools for Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="f7125-147">You can also download and install the Entity Framework 6 Tools for Visual Studio 2012.</span></span> <span data-ttu-id="f7125-148">Zobacz [Entity Framework pobierania](https://msdn.com/data/ee712906#tooling).</span><span class="sxs-lookup"><span data-stu-id="f7125-148">See the [Get Entity Framework](https://msdn.com/data/ee712906#tooling).</span></span>

<a id="scaffold"></a>
### <a name="aspnet-scaffolding"></a><span data-ttu-id="f7125-149">Tworzenie szkieletu ASP.NET</span><span class="sxs-lookup"><span data-stu-id="f7125-149">ASP.NET Scaffolding</span></span>

<span data-ttu-id="f7125-150">Tworzenie szkieletów ASP.NET jest strukturą generowania kodu dla aplikacji sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f7125-150">ASP.NET Scaffolding is a code generation framework for ASP.NET Web applications.</span></span> <span data-ttu-id="f7125-151">Ułatwia to dodawanie kodu standardowego do projektu, który współdziała z modelem danych.</span><span class="sxs-lookup"><span data-stu-id="f7125-151">It makes it easy to add boilerplate code to your project that interacts with a data model.</span></span>

<span data-ttu-id="f7125-152">W poprzednich wersjach programu Visual Studio funkcja tworzenia szkieletów została ograniczona do projektów ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="f7125-152">In previous versions of Visual Studio, scaffolding was limited to ASP.NET MVC projects.</span></span> <span data-ttu-id="f7125-153">Dzięki tej aktualizacji można teraz używać szkieletów dla dowolnego projektu ASP.NET, w tym formularzy sieci Web.</span><span class="sxs-lookup"><span data-stu-id="f7125-153">With this update, you can now use scaffolding for any ASP.NET project, including Web Forms.</span></span> <span data-ttu-id="f7125-154">Ta aktualizacja nie obsługuje generowania stron dla projektu formularzy sieci Web, ale nadal można używać szkieletów z formularzami sieci Web, dodając zależności MVC do projektu.</span><span class="sxs-lookup"><span data-stu-id="f7125-154">This update does not support generating pages for a Web Forms project, but you can still use scaffolding with Web Forms by adding MVC dependencies to the project.</span></span> <span data-ttu-id="f7125-155">Obsługa generowania stron dla formularzy sieci Web zostanie dodana w przyszłej aktualizacji.</span><span class="sxs-lookup"><span data-stu-id="f7125-155">Support for generating pages for Web Forms will be added in a future update.</span></span>

<span data-ttu-id="f7125-156">W przypadku korzystania z szkieletów upewnij się, że wszystkie wymagane zależności są zainstalowane w projekcie.</span><span class="sxs-lookup"><span data-stu-id="f7125-156">When using scaffolding, we ensure that all required dependencies are installed in the project.</span></span> <span data-ttu-id="f7125-157">Na przykład, jeśli zaczniesz od projektu ASP.NET Web Forms, a następnie użyjesz szkieletu do dodania kontrolera interfejsu API sieci Web, wymagane pakiety i odwołania NuGet są dodawane do projektu automatycznie.</span><span class="sxs-lookup"><span data-stu-id="f7125-157">For example, if you start with an ASP.NET Web Forms project and then use scaffolding to add a Web API Controller, the required NuGet packages and references are added to your project automatically.</span></span>

<span data-ttu-id="f7125-158">Aby dodać szkielet MVC do projektu formularzy sieci Web, Dodaj **nowy element szkieletowy** i wybierz **zależności MVC 5** w oknie dialogowym.</span><span class="sxs-lookup"><span data-stu-id="f7125-158">To add MVC scaffolding to a Web Forms project, add a **New Scaffolded Item** and select **MVC 5 Dependencies** in the dialog window.</span></span> <span data-ttu-id="f7125-159">Dostępne są dwie opcje tworzenia szkieletów MVC; Minimalny i pełny.</span><span class="sxs-lookup"><span data-stu-id="f7125-159">There are two options for scaffolding MVC; Minimal and Full.</span></span> <span data-ttu-id="f7125-160">W przypadku wybrania opcji minimalny tylko pakiety NuGet i odwołania dla ASP.NET MVC zostaną dodane do projektu.</span><span class="sxs-lookup"><span data-stu-id="f7125-160">If you select Minimal, only the NuGet packages and references for ASP.NET MVC are added to your project.</span></span> <span data-ttu-id="f7125-161">W przypadku wybrania opcji Pełna są dodawane minimalne zależności, a także wymagane pliki zawartości dla projektu MVC.</span><span class="sxs-lookup"><span data-stu-id="f7125-161">If you select the Full option, the Minimal dependencies are added, as well as the required content files for an MVC project.</span></span>

<span data-ttu-id="f7125-162">Obsługa tworzenia szkieletów kontrolerów asynchronicznych używa nowych funkcji asynchronicznych programu Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="f7125-162">Support for scaffolding async controllers uses the new async features from Entity Framework 6.</span></span>

<span data-ttu-id="f7125-163">Aby uzyskać więcej informacji i samouczków, zobacz [Omówienie tworzenia szkieletów ASP.NET](../2013/aspnet-scaffolding-overview.md).</span><span class="sxs-lookup"><span data-stu-id="f7125-163">For more information and tutorials, see [ASP.NET Scaffolding Overview](../2013/aspnet-scaffolding-overview.md).</span></span> <span data-ttu-id="f7125-164">Te samouczki przedstawiają tworzenie szkieletów Visual Studio 2013, ale mają również zastosowanie do ASP.NET and Web Tools 2013,1 dla programu Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="f7125-164">These tutorials show scaffolding with Visual Studio 2013, but they are also applicable to ASP.NET and Web Tools 2013.1 for Visual Studio 2012.</span></span>

<a id="razor"></a>
### <a name="razor-editor"></a><span data-ttu-id="f7125-165">Edytor Razor</span><span class="sxs-lookup"><span data-stu-id="f7125-165">Razor Editor</span></span>

<span data-ttu-id="f7125-166">Dzięki tej aktualizacji program Visual Studio 2012 obsługuje teraz narzędzia/edycję Razor 3.</span><span class="sxs-lookup"><span data-stu-id="f7125-166">With this update, Visual Studio 2012 now supports Razor 3 tooling/editing.</span></span>

<a id="nuget"></a>
### <a name="nuget-27"></a><span data-ttu-id="f7125-167">NuGet 2.7</span><span class="sxs-lookup"><span data-stu-id="f7125-167">NuGet 2.7</span></span>

<span data-ttu-id="f7125-168">Pakiet NuGet 2,7 zawiera rozbudowany zestaw nowych funkcji, które są szczegółowo opisane w [informacjach o wersji programu nuget 2,7](http://docs.nuget.org/docs/release-notes/nuget-2.7).</span><span class="sxs-lookup"><span data-stu-id="f7125-168">NuGet 2.7 includes a rich set of new features which are described in detail at [NuGet 2.7 Release Notes](http://docs.nuget.org/docs/release-notes/nuget-2.7).</span></span>

<span data-ttu-id="f7125-169">Ta wersja programu NuGet eliminuje konieczność jawnego zezwalania użytkownikom na Przywracanie brakujących pakietów przez pakiet NuGet.</span><span class="sxs-lookup"><span data-stu-id="f7125-169">This version of NuGet removes the need for users to explicitly allow NuGet to restore missing packages.</span></span> <span data-ttu-id="f7125-170">Podczas instalowania programu NuGet 2,7 Użytkownicy niejawnie wyrażają zgodę na automatyczne przywracanie brakujących pakietów.</span><span class="sxs-lookup"><span data-stu-id="f7125-170">When installing NuGet 2.7, users implicitly consent to automatically restoring missing packages.</span></span> <span data-ttu-id="f7125-171">Użytkownicy mogą jawnie zrezygnować z przywracania pakietu za pomocą ustawień NuGet w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f7125-171">Users can explicitly opt out of package restoration through the NuGet settings in Visual Studio.</span></span> <span data-ttu-id="f7125-172">Ta zmiana upraszcza działanie przywracania pakietu.</span><span class="sxs-lookup"><span data-stu-id="f7125-172">This change simplifies how package restoration works.</span></span>

## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="f7125-173">Znane problemy i istotne zmiany</span><span class="sxs-lookup"><span data-stu-id="f7125-173">Known Issues and Breaking Changes</span></span>

<a id="issuescaffolding"></a>
### <a name="aspnet-scaffolding"></a><span data-ttu-id="f7125-174">Tworzenie szkieletu ASP.NET</span><span class="sxs-lookup"><span data-stu-id="f7125-174">ASP.NET Scaffolding</span></span>

<a id="404issue"></a>
#### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a><span data-ttu-id="f7125-175">Tworzenie szkieletu kodu MVC i interfejsu API sieci Web-HTTP 404, nie znaleziono błędu</span><span class="sxs-lookup"><span data-stu-id="f7125-175">MVC and Web API Scaffolding - HTTP 404, Not Found error</span></span>

<span data-ttu-id="f7125-176">Jeśli wystąpi błąd podczas dodawania elementu szkieletowego do projektu, istnieje możliwość, że projekt zostanie pozostawiony w stanie niespójnym.</span><span class="sxs-lookup"><span data-stu-id="f7125-176">If you encounter an error when adding a scaffolded item to a project, it is possible your project will be left in an inconsistent state.</span></span> <span data-ttu-id="f7125-177">Niektóre zmiany zostaną wycofane, ale inne zmiany, takie jak zainstalowane pakiety NuGet, nie zostaną wycofane.</span><span class="sxs-lookup"><span data-stu-id="f7125-177">Some of the changes made be scaffolding will be rolled back but other changes, such as the installed NuGet packages, will not be rolled back.</span></span> <span data-ttu-id="f7125-178">Jeśli zmiany konfiguracji routingu są wycofywane, użytkownicy otrzymają błąd HTTP 404 podczas przechodzenia do elementów szkieletowych.</span><span class="sxs-lookup"><span data-stu-id="f7125-178">If the routing configuration changes are rolled back, users will receive an HTTP 404 error when navigating to scaffolded items.</span></span>

<span data-ttu-id="f7125-179">Aby naprawić ten błąd dla składnika MVC, Dodaj nowy element szkieletowy i wybierz zależności MVC 5 (minimalnie lub pełne).</span><span class="sxs-lookup"><span data-stu-id="f7125-179">To fix this error for MVC, add a new scaffolded item and select MVC 5 Dependencies (either Minimal or Full).</span></span> <span data-ttu-id="f7125-180">Ten proces spowoduje dodanie wszystkich wymaganych zmian do projektu.</span><span class="sxs-lookup"><span data-stu-id="f7125-180">This process will add all of the required changes to your project.</span></span>

<span data-ttu-id="f7125-181">Aby naprawić ten błąd dla interfejsu API sieci Web:</span><span class="sxs-lookup"><span data-stu-id="f7125-181">To fix this error for Web API:</span></span>

1. <span data-ttu-id="f7125-182">Dodaj następującą klasę WebApiConfig do projektu.</span><span class="sxs-lookup"><span data-stu-id="f7125-182">Add the following WebApiConfig class to your project.</span></span>

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample1.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample2.vb)]
2. <span data-ttu-id="f7125-183">Skonfiguruj WebApiConfig. Register w aplikacji\_Start w programie Global. asax w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="f7125-183">Configure WebApiConfig.Register in the Application\_Start method in Global.asax as follows:</span></span>

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample3.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample4.vb)]

<a id="expressissue"></a>
#### <a name="visual-studio-express-2012-for-web-stops-working-after-adding-a-scaffolded-item"></a><span data-ttu-id="f7125-184">Visual Studio Express 2012 dla sieci Web przestaje działać po dodaniu szkieletu elementu</span><span class="sxs-lookup"><span data-stu-id="f7125-184">Visual Studio Express 2012 for Web stops working after adding a scaffolded item</span></span>

<span data-ttu-id="f7125-185">Jeśli Visual Studio Express 2012 dla sieci Web przestaje działać po dodaniu szkieletu elementu z Entity Framework (takich jak kontroler Web API 2 z akcjami, przy użyciu Entity Framework), istnieje możliwość, że Visual Studio Express nie załadowała obrazu natywnego zestawu zależne od system. Web. Extensions.</span><span class="sxs-lookup"><span data-stu-id="f7125-185">If Visual Studio Express 2012 for Web stops working after adding scaffolded item with Entity Framework (such as Web API 2 Controller with actions, using Entity Framework), it is possible that Visual Studio Express failed to load the native image of an assembly dependent on System.Web.Extensions.</span></span>

<span data-ttu-id="f7125-186">Aby rozwiązać ten problem, skonfiguruj Visual Studio Express do pracy z obrazem MSIL systemu system. Web. Extensions:</span><span class="sxs-lookup"><span data-stu-id="f7125-186">To correct this problem, configure Visual Studio Express to work with the MSIL image of System.Web.Extensions:</span></span>

1. <span data-ttu-id="f7125-187">Otwórz wiersz polecenia w trybie administratora.</span><span class="sxs-lookup"><span data-stu-id="f7125-187">Open Command Prompt in the Administrator mode.</span></span>
2. <span data-ttu-id="f7125-188">Przejdź do pozycji%ProgramFiles%\Microsoft Visual Studio 11.0 \ Common7\IDE lub% ProgramFiles (x86)% \ Microsoft Visual Studio 11.0 \ Common7\IDE (w przypadku 64 bitowego systemu Windows).</span><span class="sxs-lookup"><span data-stu-id="f7125-188">Go to %ProgramFiles%\Microsoft Visual Studio 11.0\Common7\IDE or %ProgramFiles(x86)%\Microsoft Visual Studio 11.0\Common7\IDE (for 64 bit Windows).</span></span>
3. <span data-ttu-id="f7125-189">Otwórz plik VWDExpress. exe. config w edytorze tekstów.</span><span class="sxs-lookup"><span data-stu-id="f7125-189">Open VWDExpress.exe.config in a text editor.</span></span>
4. <span data-ttu-id="f7125-190">Dodaj następujący wiersz w obszarze &lt;konfiguracji&gt;/&lt;środowisko uruchomieniowe&gt;:</span><span class="sxs-lookup"><span data-stu-id="f7125-190">Add the following line under the &lt;configuration&gt;/&lt;runtime&gt; element:</span></span>  

    [!code-xml[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample5.xml)]
5. <span data-ttu-id="f7125-191">Uruchom ponownie Visual Studio Express 2012 dla sieci Web.</span><span class="sxs-lookup"><span data-stu-id="f7125-191">Restart Visual Studio Express 2012 for Web.</span></span>

<a id="issuerazor"></a>
### <a name="aspnet-razor-3"></a><span data-ttu-id="f7125-192">ASP.NET Razor 3</span><span class="sxs-lookup"><span data-stu-id="f7125-192">ASP.NET Razor 3</span></span>

<a id="browseissue"></a>
#### <a name="viewing-cshtml-file-with-browse-with-or-f5-causes-a-server-error"></a><span data-ttu-id="f7125-193">Wyświetlanie pliku cshtml z przeglądaniem lub F5 powoduje błąd serwera</span><span class="sxs-lookup"><span data-stu-id="f7125-193">Viewing cshtml file with Browse With or F5 causes a server error</span></span>

<span data-ttu-id="f7125-194">Podczas tworzenia projektu MVC 5 w programie Visual Studio 2012 (lub Otwórz w programie Visual Studio 2012 projekt MVC 5, który został utworzony w Visual Studio 2013) i próba wyświetlenia pliku cshtml przy użyciu polecenia Przeglądaj z lub F5, zostanie wyświetlony komunikat o błędzie z informacją o **błędzie serwera w aplikacji "/"** .</span><span class="sxs-lookup"><span data-stu-id="f7125-194">When you create an MVC 5 project in Visual Studio 2012 (or open in Visual Studio 2012 an MVC 5 project that was created in Visual Studio 2013) and attempt to view a cshtml file by using Browse With or F5, you will receive an error that states - **Server Error in '/' Application**.</span></span> <span data-ttu-id="f7125-195">Serwer próbuje przejść do `http://localhost:XXXX/Views/../XXXX.cshtml`</span><span class="sxs-lookup"><span data-stu-id="f7125-195">The server attempts to navigate to `http://localhost:XXXX/Views/../XXXX.cshtml`</span></span>

<span data-ttu-id="f7125-196">Aby rozwiązać ten problem, Zmień ustawienie **akcji Rozpocznij** w projekcie na **określoną stronę**.</span><span class="sxs-lookup"><span data-stu-id="f7125-196">To resolve this issue, change the **Start Action** setting in your project to **Specific Page**.</span></span> <span data-ttu-id="f7125-197">Nie musisz podawać wartości dla strony.</span><span class="sxs-lookup"><span data-stu-id="f7125-197">You do not need to provide a value for the page.</span></span>

![](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image1.png)

<span data-ttu-id="f7125-198">Po wprowadzeniu tej zmiany wybranie opcji F5 spowoduje przejście do katalogu głównego aplikacji (`http://localhost:XXXX`).</span><span class="sxs-lookup"><span data-stu-id="f7125-198">After making this change, selecting F5 navigates to the root of your application (`http://localhost:XXXX`).</span></span> <span data-ttu-id="f7125-199">Takie zachowanie nie jest takie samo jak zachowanie dla projektów MVC 5 w Visual Studio 2013, gdzie bieżące ustawienie **strony** powoduje uruchomienie otwartej strony.</span><span class="sxs-lookup"><span data-stu-id="f7125-199">This behavior is not the same as the behavior for MVC 5 projects in Visual Studio 2013, where the **Current Page** setting launches the open page.</span></span>

<a id="rewriteissue"></a>
#### <a name="url-rewrite-and-tilde"></a><span data-ttu-id="f7125-200">Ponowne zapisywanie adresów URL i Tylda (~)</span><span class="sxs-lookup"><span data-stu-id="f7125-200">Url Rewrite and Tilde(~)</span></span>

<span data-ttu-id="f7125-201">Po uaktualnieniu do ASP.NET Razor 3 lub ASP.NET MVC 5, notacja tyldy (~) może przestać działać poprawnie, jeśli używasz funkcji ponownego zapisywania adresów URL.</span><span class="sxs-lookup"><span data-stu-id="f7125-201">After upgrading to ASP.NET Razor 3 or ASP.NET MVC 5, the tilde(~) notation may no longer work correctly if you are using URL rewrites.</span></span> <span data-ttu-id="f7125-202">Ponowny zapis URL ma wpływ na notację tyldy (~) w elementach HTML, takich jak &lt;A/&gt;, &lt;skrypt/&gt;, &lt;LINK/&gt;, i w związku z tym tylda nie jest już mapowana do katalogu głównego.</span><span class="sxs-lookup"><span data-stu-id="f7125-202">The URL rewrite affects the tilde(~) notation in HTML elements such as &lt;A/&gt;, &lt;SCRIPT/&gt;, &lt;LINK/&gt;, and as a result the tilde no longer maps to the root directory.</span></span>

<span data-ttu-id="f7125-203">Na przykład w przypadku ponownego zapisu żądania dla **ASP.net/content** do **ASP.NET**, atrybut href w &lt;a href = "~/Content/"/&gt; jest rozpoznawany jako **/Content/Content/** zamiast **/** .</span><span class="sxs-lookup"><span data-stu-id="f7125-203">For example, if you rewrite requests for **asp.net/content** to **asp.net**, the href attribute in &lt;A href="~/content/"/&gt; resolves to **/content/content/** instead of **/**.</span></span> <span data-ttu-id="f7125-204">Aby pominąć tę zmianę, można ustawić kontekst **WasUrlRewritten usługi IIS\_** na wartość false na każdej stronie sieci Web lub w **aplikacji\_BeginRequest** w Global. asax.</span><span class="sxs-lookup"><span data-stu-id="f7125-204">To suppress this change, you can set the **IIS\_WasUrlRewritten** context to false in each Web Page or in **Application\_BeginRequest** in Global.asax.</span></span>

<a id="templateissue"></a>
### <a name="templates"></a><span data-ttu-id="f7125-205">Szablony</span><span class="sxs-lookup"><span data-stu-id="f7125-205">Templates</span></span>

<span data-ttu-id="f7125-206">Podczas tworzenia projektów ASP.NET MVC przy użyciu programu Visual Studio 2012 w systemie Windows 8.1 lub Windows Server 2012 R2 program Visual Studio wyświetli komunikat o błędzie z informacją o tym, że "Konfigurowanie sieci Web [URL] dla ASP.NET 4,5 nie powiodło się".</span><span class="sxs-lookup"><span data-stu-id="f7125-206">When you create ASP.NET MVC projects with Visual Studio 2012 on Windows 8.1 or Windows Server 2012 R2, Visual Studio displays an error message that states "Configuring Web [url] for ASP.NET 4.5 failed."</span></span>

![Błąd konfiguracji](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image2.png)

<span data-ttu-id="f7125-208">Ten błąd jest wyświetlany, ponieważ program Visual Studio 2012 nie włącza funkcji ASP.NET 4,5, gdy jest ona zainstalowana w tych wersjach systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="f7125-208">You see this error because Visual Studio 2012 does not enable the ASP.NET 4.5 feature when it is installed on those releases of Windows.</span></span> <span data-ttu-id="f7125-209">Aby włączyć ASP.NET 4,5, wykonaj kroki opisane w sekcji [Włączanie lub wyłączanie funkcji systemu Windows](https://windows.microsoft.com/windows-8/turn-windows-features-on-off).</span><span class="sxs-lookup"><span data-stu-id="f7125-209">To enable ASP.NET 4.5, perform the steps described in [Turn Windows features on or off](https://windows.microsoft.com/windows-8/turn-windows-features-on-off).</span></span>

![Włącz lub wyłącz funkcje systemu Windows](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image3.png)

<span data-ttu-id="f7125-211">Alternatywnie można włączyć ASP.NET 4,5 za pomocą wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="f7125-211">Alternatively, you can enable ASP.NET 4.5 through the command line.</span></span>

1. <span data-ttu-id="f7125-212">Otwórz wiersz polecenia w trybie administratora.</span><span class="sxs-lookup"><span data-stu-id="f7125-212">Open Command Prompt in the Administrator mode.</span></span>
2. <span data-ttu-id="f7125-213">Uruchom następujące polecenie, aby włączyć ASP.NET 4,5.</span><span class="sxs-lookup"><span data-stu-id="f7125-213">Run the following command to enable ASP.NET 4.5.</span></span>  
    `dism /Online /Enable-Feature /FeatureName:NetFx4Extended-ASPNET45 /Quiet /NoRestart`

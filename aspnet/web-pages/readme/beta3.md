---
uid: web-pages/readme/beta3
title: Web Matrix i ASP.NET Web Pages (Razor) beta 3 wersja Readme | Microsoft Docs
author: rick-anderson
description: Plik Readme dla programu WebMatrix i wzorca ASP.NET Web Pages (Razor) w wersji Beta 3
ms.author: riande
ms.date: 01/10/2011
ms.assetid: ffa3d5c9-91e5-4da3-b409-560b0c7fbbf0
msc.legacyurl: /web-pages/readme/beta3
msc.type: content
ms.openlocfilehash: dc1d9237c04a7fcdbf4db6ccc8c36d255f6de003
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78628899"
---
# <a name="web-matrix-and-aspnet-web-pages-razor-beta-3-release-readme"></a><span data-ttu-id="c9b6f-103">Plik Readme dla programu WebMatrix i wzorca ASP.NET Web Pages (Razor) w wersji Beta 3</span><span class="sxs-lookup"><span data-stu-id="c9b6f-103">Web Matrix and ASP.NET Web Pages (Razor) Beta 3 Release Readme</span></span>

> <span data-ttu-id="c9b6f-104">Plik Readme dla programu WebMatrix i wzorca ASP.NET Web Pages (Razor) w wersji Beta 3</span><span class="sxs-lookup"><span data-stu-id="c9b6f-104">Web Matrix and ASP.NET Web Pages (Razor) Beta 3 Release Readme</span></span>

<span data-ttu-id="c9b6f-105">9 listopada 2010</span><span class="sxs-lookup"><span data-stu-id="c9b6f-105">9 November 2010</span></span>

## <a name="contents"></a><span data-ttu-id="c9b6f-106">Spis treści</span><span class="sxs-lookup"><span data-stu-id="c9b6f-106">Contents</span></span>

- [<span data-ttu-id="c9b6f-107">Omówienie</span><span class="sxs-lookup"><span data-stu-id="c9b6f-107">Overview</span></span>](#Overview)
- [<span data-ttu-id="c9b6f-108">Instalacja</span><span class="sxs-lookup"><span data-stu-id="c9b6f-108">Installation</span></span>](#Installation_Notes)
- [<span data-ttu-id="c9b6f-109">Nowe funkcje, zmiany i znane problemy w wersji beta 3</span><span class="sxs-lookup"><span data-stu-id="c9b6f-109">New Features, Changes, and Known Issues in the Beta 3 release</span></span>](#Known_Issues)

    - [<span data-ttu-id="c9b6f-110">Problemy z instalacją programu WebMatrix</span><span class="sxs-lookup"><span data-stu-id="c9b6f-110">WebMatrix Installation Issues</span></span>](#Known_Issues_Installation)
    - [<span data-ttu-id="c9b6f-111">ASP.NET Web Pages</span><span class="sxs-lookup"><span data-stu-id="c9b6f-111">ASP.NET Web Pages</span></span>](#Known_Issues_ASPNET)
    - [<span data-ttu-id="c9b6f-112">SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="c9b6f-112">SQL Server Compact</span></span>](#Known_Issues_SQL_Server_Compact)
    - [<span data-ttu-id="c9b6f-113">Instalowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="c9b6f-113">Installing Applications</span></span>](#Known_Issues_Installing_Applications)
    - [<span data-ttu-id="c9b6f-114">Publikowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="c9b6f-114">Publishing Applications</span></span>](#Known_Issues_Publishing_Applications)
    - [<span data-ttu-id="c9b6f-115">Inne problemy</span><span class="sxs-lookup"><span data-stu-id="c9b6f-115">Other Issues</span></span>](#Known_Issues_Other_Issues)
- [<span data-ttu-id="c9b6f-116">Aby uzyskać więcej informacji</span><span class="sxs-lookup"><span data-stu-id="c9b6f-116">For More Information</span></span>](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a><span data-ttu-id="c9b6f-117">Omówienie</span><span class="sxs-lookup"><span data-stu-id="c9b6f-117">Overview</span></span>

> <span data-ttu-id="c9b6f-118">Microsoft WebMatrix beta to bezpłatny stos rozwoju sieci Web, który jest instalowany w ciągu kilku minut.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-118">Microsoft WebMatrix Beta is a free web development stack that installs in minutes.</span></span> <span data-ttu-id="c9b6f-119">Integruje on serwer sieci Web z bazami danych i strukturami programowania w celu utworzenia jednego, zintegrowanego środowiska.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-119">It integrates a web server with database and programming frameworks to create a single, integrated experience.</span></span> <span data-ttu-id="c9b6f-120">Za pomocą programu WebMatrix beta można usprawnić wykonywanie kodu, testować i publikować własną witrynę sieci Web ASP.NET lub PHP lub użyć programu WebMatrix beta do uruchamiania nowej witryny sieci Web przy użyciu popularnych aplikacji typu "open source", takich jak DotNetNuke, Umbraco, WordPress lub Joomla.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-120">You can use WebMatrix Beta to streamline the way you code, test, and publish your own ASP.NET or PHP website, or you can use WebMatrix Beta to start a new website using popular open-source apps like DotNetNuke, Umbraco, WordPress, or Joomla.</span></span> <span data-ttu-id="c9b6f-121">Program WebMatrix beta korzysta z tego samego zaawansowanego serwera sieci Web, aparatu bazy danych i środowiska struktur, które będą uruchamiać witrynę internetową w Internecie, co sprawia, że przejście od projektowania do produkcji jest płynne i bezproblemowe.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-121">WebMatrix Beta uses the same powerful web server, database engine, and frameworks environment that will run your website on the internet, which makes the transition from development to production smooth and seamless.</span></span>

<a id="Installation_Notes"></a>

## <a name="installation"></a><span data-ttu-id="c9b6f-122">Instalacja</span><span class="sxs-lookup"><span data-stu-id="c9b6f-122">Installation</span></span>

> <span data-ttu-id="c9b6f-123">Aby zainstalować program WebMatrix beta 3, należy użyć [Instalator platformy Microsoft Web 3,0](https://go.microsoft.com/fwlink/?LinkID=194638).</span><span class="sxs-lookup"><span data-stu-id="c9b6f-123">To install WebMatrix Beta 3, you use [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span></span> <span data-ttu-id="c9b6f-124">Po zainstalowaniu Instalatora platformy sieci Web można go użyć do zainstalowania programu WebMatrix beta 3.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-124">After you've installed the Web Platform Installer, you can use it to install WebMatrix Beta 3.</span></span>
> 
> <span data-ttu-id="c9b6f-125">Jeśli masz problemy podczas instalacji, zapoznaj się z [tematem Rozwiązywanie problemów z Instalator platformy Microsoft Web](https://go.microsoft.com/fwlink/?LinkId=196212).</span><span class="sxs-lookup"><span data-stu-id="c9b6f-125">If you have problems during installation, refer to [Troubleshooting Problems with Microsoft Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=196212).</span></span>

<a id="Installation_Notes0"></a>

## <a name="instructions-for-publishing-applications"></a><span data-ttu-id="c9b6f-126">Instrukcje dotyczące publikowania aplikacji</span><span class="sxs-lookup"><span data-stu-id="c9b6f-126">Instructions for Publishing Applications</span></span>

> <span data-ttu-id="c9b6f-127">Zapoznaj się z [instrukcjami krok po kroku dotyczącymi publikowania aplikacji](https://go.microsoft.com/fwlink/?LinkID=196149)</span><span class="sxs-lookup"><span data-stu-id="c9b6f-127">See [Step-by-Step Instructions for Publishing Applications](https://go.microsoft.com/fwlink/?LinkID=196149)</span></span>

<a id="Known_Issues"></a>

## <a name="new-features-changes-andknown-issues"></a><span data-ttu-id="c9b6f-128">Nowe funkcje, zmiany, andKnown problemy</span><span class="sxs-lookup"><span data-stu-id="c9b6f-128">New Features, Changes, andKnown Issues</span></span>

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-beta-3-installation"></a><span data-ttu-id="c9b6f-129">Instalacja programu WebMatrix beta 3</span><span class="sxs-lookup"><span data-stu-id="c9b6f-129">WebMatrix Beta 3 Installation</span></span>

#### <a name="issue-webmatrix-beta-3-is-only-available-on-platforms-that-support-microsoft-net-framework-4"></a><span data-ttu-id="c9b6f-130">Problem: WebMatrix beta 3 jest dostępna tylko na platformach, które obsługują Microsoft .NET Framework 4</span><span class="sxs-lookup"><span data-stu-id="c9b6f-130">Issue: WebMatrix Beta 3 is only available on platforms that support Microsoft .NET Framework 4</span></span>

> <span data-ttu-id="c9b6f-131">W programie WebMatrix beta jest wymagany .NET Framework wersja 4.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-131">The .NET Framework version 4 is required for WebMatrix Beta.</span></span> <span data-ttu-id="c9b6f-132">W niektórych przypadkach Instalator programu WebMatrix beta podejmie próbę instalacji na platformie, która nie jest częścią obsługiwanego zestawu konfiguracyjnego.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-132">In certain cases, the WebMatrix Beta installer will let you try to install on a platform that is not part of the supported configuration set.</span></span> <span data-ttu-id="c9b6f-133">W szczególności system Windows Vista bez aktualizacji programu SP1 umożliwi rozpoczęcie instalacji programu WebMatrix beta, ale składnik .NET Framework 4 zakończy się niepowodzeniem i zablokuje instalację.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-133">In particular, Windows Vista without the SP1 update will let you begin the installation of WebMatrix Beta, but the .NET Framework 4 component will fail and block your installation.</span></span>
> 
> <span data-ttu-id="c9b6f-134">**Obejście**</span><span class="sxs-lookup"><span data-stu-id="c9b6f-134">**Workaround**</span></span>  
> <span data-ttu-id="c9b6f-135">Zainstaluj program na obsługiwanej platformie, w tym:</span><span class="sxs-lookup"><span data-stu-id="c9b6f-135">Install on a supported platform, which includes:</span></span>
> 
> - <span data-ttu-id="c9b6f-136">Windows 7</span><span class="sxs-lookup"><span data-stu-id="c9b6f-136">Windows 7</span></span>
> - <span data-ttu-id="c9b6f-137">Windows Server 2008</span><span class="sxs-lookup"><span data-stu-id="c9b6f-137">Windows Server 2008</span></span>
> - <span data-ttu-id="c9b6f-138">Windows Server 2008 R2</span><span class="sxs-lookup"><span data-stu-id="c9b6f-138">Windows Server 2008 R2</span></span>
> - <span data-ttu-id="c9b6f-139">Windows Vista z dodatkiem SP1 lub nowszym</span><span class="sxs-lookup"><span data-stu-id="c9b6f-139">Windows Vista SP1 or later</span></span>
> - <span data-ttu-id="c9b6f-140">Windows XP z dodatkiem SP3</span><span class="sxs-lookup"><span data-stu-id="c9b6f-140">Windows XP SP3</span></span>
> - <span data-ttu-id="c9b6f-141">Windows Server 2003 SP2</span><span class="sxs-lookup"><span data-stu-id="c9b6f-141">Windows Server 2003 SP2</span></span>

#### <a name="issue-cannot-install-webmatrix-beta-3-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a><span data-ttu-id="c9b6f-142">Problem: nie można zainstalować programu WebMatrix beta 3, jeśli Microsoft Visual Studio 2008 jest zainstalowana bez Microsoft Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="c9b6f-142">Issue: Cannot install WebMatrix Beta 3 if Microsoft Visual Studio 2008 is installed without Microsoft Visual Studio 2008 SP1</span></span>

> <span data-ttu-id="c9b6f-143">**Obejście**</span><span class="sxs-lookup"><span data-stu-id="c9b6f-143">**Workaround**</span></span>  
> <span data-ttu-id="c9b6f-144">Zainstaluj [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) z centrum pobierania Microsoft.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-144">Install [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) from the Microsoft Download Center.</span></span>

#### <a name="issue-some-assemblies-for-sql-server-compact-40-are-not-installed-in-the-gac"></a><span data-ttu-id="c9b6f-145">Problem: Niektóre zestawy dla SQL Server Compact 4,0 nie są zainstalowane w pamięci podręcznej GAC</span><span class="sxs-lookup"><span data-stu-id="c9b6f-145">Issue: Some assemblies for SQL Server Compact 4.0 are not installed in the GAC</span></span>

> <span data-ttu-id="c9b6f-146">Zestawy zarządzane dla SQL Server Compact 4,0 nie są umieszczane w globalnej pamięci podręcznej zestawów (GAC) podczas instalacji SQL Server Compact 4,0 na komputerze 64-bitowym, a komputer ma tylko .NET Framework zainstalowany profil klienta programu 3,5 z dodatkiem SP1.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-146">The managed assemblies for SQL Server Compact 4.0 are not placed in the global assembly cache (GAC) when you install SQL Server Compact 4.0 on a 64-bit computer and the computer has only the .NET Framework 3.5 SP1 Client Profile installed.</span></span> <span data-ttu-id="c9b6f-147">Zarządzane zestawy, które nie są zainstalowane w pamięci podręcznej GAC, to:</span><span class="sxs-lookup"><span data-stu-id="c9b6f-147">The managed assemblies that are not installed in the GAC are:</span></span>
> 
> - <span data-ttu-id="c9b6f-148">*System. Data. SqlServerCe. dll* (dostawca ADO.NET)</span><span class="sxs-lookup"><span data-stu-id="c9b6f-148">*System.Data.SqlServerCe.dll* (ADO.NET provider)</span></span>
> - <span data-ttu-id="c9b6f-149">*System. Data. SqlServerCe. Entity. dll* (ADO.NET Entity Framework)</span><span class="sxs-lookup"><span data-stu-id="c9b6f-149">*System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework )</span></span>
> 
> <span data-ttu-id="c9b6f-150">**Obejście**</span><span class="sxs-lookup"><span data-stu-id="c9b6f-150">**Workaround**</span></span>  
> <span data-ttu-id="c9b6f-151">Odinstaluj SQL Server Compact 4,0.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-151">Uninstall SQL Server Compact 4.0.</span></span> <span data-ttu-id="c9b6f-152">Pobierz i zainstaluj pełną wersję .NET Framework 3,5 SP1 z następującej lokalizacji:</span><span class="sxs-lookup"><span data-stu-id="c9b6f-152">Download and install the full version of .NET Framework 3.5 SP1 from the following location:</span></span>  
>   
> [<span data-ttu-id="c9b6f-153">Microsoft .NET Framework 3,5 z dodatkiem Service Pack 1 (pełny pakiet)</span><span class="sxs-lookup"><span data-stu-id="c9b6f-153">Microsoft .NET Framework 3.5 Service pack 1 (Full Package)</span></span>](https://go.microsoft.com/fwlink/?LinkId=194828)  
>   
> <span data-ttu-id="c9b6f-154">Następnie ponownie zainstaluj SQL Server Compact 4,0.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-154">Then reinstall SQL Server Compact 4.0.</span></span>

#### <a name="issue-cannot-uninstall-sql-server-compact-using-the-command-line"></a><span data-ttu-id="c9b6f-155">Problem: nie można odinstalować SQL Server Compact przy użyciu wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="c9b6f-155">Issue: Cannot uninstall SQL Server Compact using the command line</span></span>

> <span data-ttu-id="c9b6f-156">Dezinstalacja SQL Server Compact przy użyciu opcji wiersza polecenia nie działa w tej wersji.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-156">Uninstallation of SQL Server Compact using command-line options does not work in this release.</span></span>
> 
> <span data-ttu-id="c9b6f-157">**Obejście**</span><span class="sxs-lookup"><span data-stu-id="c9b6f-157">**Workaround**</span></span>  
> <span data-ttu-id="c9b6f-158">Aby odinstalować Microsoft SQL Server Compact 4,0, użyj *apletu programy i funkcje* w panelu sterowania systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-158">Use *Programs and Features* in the Windows Control Panel to uninstall Microsoft SQL Server Compact 4.0.</span></span>

<a id="Known_Issues_ASPNET"></a>

### <a name="aspnet-web-pages"></a><span data-ttu-id="c9b6f-159">Strony sieci Web programu ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c9b6f-159">ASP.NET Web Pages</span></span>

<span data-ttu-id="c9b6f-160">W tej sekcji dokumentu opisano nowe funkcje, zmiany i znane problemy w wersji beta 3 ASP.NET stron sieci Web z programem składnia Razor.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-160">This section of the document describes new features, changes, and known issues with the Beta 3 release of ASP.NET Web Pages with Razor syntax.</span></span>

- [<span data-ttu-id="c9b6f-161">Nowe funkcje</span><span class="sxs-lookup"><span data-stu-id="c9b6f-161">New features</span></span>](#NewFeatures)
- [<span data-ttu-id="c9b6f-162">Wprowadzane</span><span class="sxs-lookup"><span data-stu-id="c9b6f-162">Changes</span></span>](#Changes)
- [<span data-ttu-id="c9b6f-163">Problemy</span><span class="sxs-lookup"><span data-stu-id="c9b6f-163">Issues</span></span>](#Issues)

<a id="NewFeatures"></a>

#### <a name="new-features-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="c9b6f-164">Nowe funkcje w wersji beta 3 dla ASP.NET stron sieci Web ze składnią Razor</span><span class="sxs-lookup"><span data-stu-id="c9b6f-164">New Features in Beta 3 for ASP.NET Web Pages with Razor Syntax</span></span>

#### <a name="new-htmlraw-method-renders-unencoded-markup"></a><span data-ttu-id="c9b6f-165">New: Metoda "html. RAW" renderuje niezakodowane znaczniki</span><span class="sxs-lookup"><span data-stu-id="c9b6f-165">New: "Html.Raw" method renders unencoded markup</span></span>

> <span data-ttu-id="c9b6f-166">Nowa metoda `Html.Raw` umożliwia renderowanie znaczników HTML jako znaczników zamiast renderowania zakodowanych danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-166">The new `Html.Raw` method lets you render HTML markup as markup instead of rendering encoded output.</span></span> <span data-ttu-id="c9b6f-167">(Domyślnie ASP.NET Razor koduje ciągi przed renderowaniem.) Składnia jest następująca:</span><span class="sxs-lookup"><span data-stu-id="c9b6f-167">(By default, ASP.NET Razor encodes strings before rendering them.) The syntax is:</span></span>
> 
> `Html.Raw(value)`
> 
> <span data-ttu-id="c9b6f-168">Poniższy przykład pokazuje, jak używać `Html.Raw`:</span><span class="sxs-lookup"><span data-stu-id="c9b6f-168">The following example shows how to use `Html.Raw`:</span></span>
> 
> [!code-cshtml[Main](beta3/samples/sample1.cshtml)]

<a id="Changes"></a>

#### <a name="changes-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="c9b6f-169">Zmiany w wersji beta 3 dla ASP.NET stron sieci Web ze składnią Razor</span><span class="sxs-lookup"><span data-stu-id="c9b6f-169">Changes in Beta 3 for ASP.NET Web Pages with Razor Syntax</span></span>

#### <a name="change-hrefattribute-method-removed"></a><span data-ttu-id="c9b6f-170">Zmiana: Metoda "Hrefattribute" została usunięta</span><span class="sxs-lookup"><span data-stu-id="c9b6f-170">Change: "HrefAttribute" method removed</span></span>

> <span data-ttu-id="c9b6f-171">Metoda `HrefAttribute` klasy `WebPage` została usunięta.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-171">The `HrefAttribute` method of the `WebPage` class has been removed.</span></span> <span data-ttu-id="c9b6f-172">Ten pomocnik został użyty do zakodowania niebezpiecznych znaków w adresach URL.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-172">This helper was used to encode unsafe characters in URLs.</span></span> <span data-ttu-id="c9b6f-173">Nie jest już wymagane, ponieważ ASP.NET Razor automatycznie koduje ciągi.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-173">It is no longer required because ASP.NET Razor automatically encodes strings.</span></span> <span data-ttu-id="c9b6f-174">(Użyj nowej metody `Html.Raw`, aby renderować niezakodowane ciągi).</span><span class="sxs-lookup"><span data-stu-id="c9b6f-174">(Use the new `Html.Raw` method to render unencoded strings.)</span></span>

#### <a name="change-syntax-for-declarative-helper-helpers-changed"></a><span data-ttu-id="c9b6f-175">Zmiana: Składnia niezmienionych pomocników "@helper"</span><span class="sxs-lookup"><span data-stu-id="c9b6f-175">Change: Syntax for declarative "@helper" helpers changed</span></span>

> <span data-ttu-id="c9b6f-176">W wersji beta 3 ASP.NET zmienia sposób analizowania pomocników, które są tworzone przy użyciu składni `@helper`.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-176">In the Beta 3 release, ASP.NET changes how it parses helpers that are created using the `@helper` syntax.</span></span> <span data-ttu-id="c9b6f-177">W zasadzie składnia `@helper` jest teraz analizowana jako blok kodu, a nie jako blok znaczników, które mogą zawierać kod.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-177">In essence, the `@helper` syntax is now parsed as a code block instead of as a block of markup that can include code.</span></span> <span data-ttu-id="c9b6f-178">W związku z tym kod wewnątrz pomocnika nie musi być ujęty w bloki `@{ }`.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-178">Therefore, code inside the helper does not need to be enclosed in `@{ }` blocks.</span></span> <span data-ttu-id="c9b6f-179">Odwrotnie, Adiustacja wewnątrz pomocnika musi być jawnie dołączona do elementów HTML lub w tagach ASP.NET Razor `<text></text>`.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-179">Conversely, markup inside the helper has to be explicitly included in HTML elements or in ASP.NET Razor `<text></text>` tags.</span></span>
> 
> <span data-ttu-id="c9b6f-180">Na przykład następująca składnia `@helper` działa w wersji beta 3:</span><span class="sxs-lookup"><span data-stu-id="c9b6f-180">For example, the following `@helper` syntax works in the Beta 3 release:</span></span>
> 
> [!code-cshtml[Main](beta3/samples/sample2.cshtml)]
> 
> <span data-ttu-id="c9b6f-181">W wersji beta 3 należy zmienić ten pomocnik, aby wyglądał jak w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="c9b6f-181">In the Beta 3 release, this helper must be changed to look like the following example:</span></span>
> 
> [!code-cshtml[Main](beta3/samples/sample3.cshtml)]
> 
> <span data-ttu-id="c9b6f-182">Należy zauważyć, że `@{ }` znaki wokół początkowego kodu w Pomocniku nie są już używane.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-182">Notice that the `@{ }` characters around the initial code in the helper is no longer used.</span></span> <span data-ttu-id="c9b6f-183">Dzieje się tak, ponieważ zawartość pomocników jest traktowana jako blok kodu domyślnie.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-183">This is because the contents of the helpers are treated as a code block by default.</span></span> <span data-ttu-id="c9b6f-184">Pomocnik renderuje znacznik, który rozpoczyna się od tagu otwierającego `<a>`.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-184">The helper renders markup, which starts with the opening `<a>` tag.</span></span> <span data-ttu-id="c9b6f-185">Jeśli pomocnik musi renderować zwykły tekst lub Tagi, które nie zawierają taga zamykającego (na przykład tagów `<meta>`), zawartość do renderowania musi znajdować się w `<text></text>` tagach.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-185">If the helper must render plain text or tags that do not include a closing tag (for example, `<meta>` tags), the content to be rendered must be in `<text></text>` tags.</span></span>

#### <a name="change-webpagecontexthttpcontext-removed"></a><span data-ttu-id="c9b6f-186">Change: "WebPageContext.HttpContext" removed</span><span class="sxs-lookup"><span data-stu-id="c9b6f-186">Change: "WebPageContext.HttpContext" removed</span></span>

> <span data-ttu-id="c9b6f-187">Właściwość `WebPageContext.HttpContext` została usunięta.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-187">The `WebPageContext.HttpContext` property has been removed.</span></span> <span data-ttu-id="c9b6f-188">Zamiast tego użyj polecenia cmdlet `HttpContext.Current`.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-188">Use `HttpContext.Current` instead.</span></span> <span data-ttu-id="c9b6f-189">(Właściwość `WebPageContext.HttpContext` po prostu zawinięte).</span><span class="sxs-lookup"><span data-stu-id="c9b6f-189">(The `WebPageContext.HttpContext` property simply wrapped this.)</span></span>

#### <a name="change-facebook-helper-moved-to-new-package"></a><span data-ttu-id="c9b6f-190">Zmiana: pomocnik "Facebook" został przeniesiony do nowego pakietu</span><span class="sxs-lookup"><span data-stu-id="c9b6f-190">Change: "Facebook" helper moved to new package</span></span>

> <span data-ttu-id="c9b6f-191">Pomocnik `Facebook` został przeniesiony do biblioteki *Facebook. pomocnika* , w tym pomocnika `Facebook` i dodatkowych funkcji.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-191">The `Facebook` helper has been moved to the *Facebook.Helper* library, which includes the `Facebook` helper and additional functionality.</span></span> <span data-ttu-id="c9b6f-192">Tę bibliotekę należy zainstalować jako oddzielny pakiet, zgodnie z opisem w temacie "Instalowanie pomocników z menedżerem pakietów" w samouczku [wprowadzenie ze stronami ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202889).</span><span class="sxs-lookup"><span data-stu-id="c9b6f-192">You must install this library as a separate package, as described in "Installing Helpers with Package Manager" in the tutorial [Getting Started with ASP.NET Pages](https://go.microsoft.com/fwlink/?LinkId=202889).</span></span>

#### <a name="change-membership-role-and-security-types-moves-to-new-assembly"></a><span data-ttu-id="c9b6f-193">Zmiana: członkostwo, rola i typy zabezpieczeń są przenoszone do nowego zestawu</span><span class="sxs-lookup"><span data-stu-id="c9b6f-193">Change: Membership, Role, and Security types moves to new assembly</span></span>

> <span data-ttu-id="c9b6f-194">Następujące typy zostały przeniesione do zestawu `WebMatrix.WebData`:</span><span class="sxs-lookup"><span data-stu-id="c9b6f-194">The following types were moved to the `WebMatrix.WebData` assembly:</span></span>
> 
> - `ExtendedMembershipProvider`
> - `SimpleMembershipProvider`
> - `SimpleRoleProvider`
> - `WebSecurity`

#### <a name="change-tagbuilder-class-moved-to-systemwebwebpagesdll-assembly"></a><span data-ttu-id="c9b6f-195">Zmiana: Klasa "TagBuilder" została przeniesiona do zestawu System. Web. Webpages. dll</span><span class="sxs-lookup"><span data-stu-id="c9b6f-195">Change: "TagBuilder" class moved to System.Web.WebPages.dll assembly</span></span>

> <span data-ttu-id="c9b6f-196">Klasa języka r `TagBuilde` została przeniesiona do zestawu System. Web. Webpages. dll.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-196">The `TagBuilde` r class has been moved to the System.Web.WebPages.dll assembly.</span></span> <span data-ttu-id="c9b6f-197">Wcześniej było to w zestawie, który był częścią ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-197">Previously, this was in an assembly that was part of ASP.NET MVC.</span></span> <span data-ttu-id="c9b6f-198">Ta zmiana oznacza, że nie trzeba instalować ASP.NET MVC, aby można było użyć klasy `TagBuilder`.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-198">This change means that you do not have to install ASP.NET MVC in order to use the `TagBuilder` class.</span></span>
> 
> <span data-ttu-id="c9b6f-199">Jednak Klasa nadal znajduje się w przestrzeni nazw `System.Web.Mvc`.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-199">However, the class is still in the `System.Web.Mvc` namespace.</span></span> <span data-ttu-id="c9b6f-200">Aby można było użyć klasy `TagBuilder` (na przykład w niestandardowym Pomocniku Razor ASP.NET), należy odwołać się do przestrzeni nazw (na przykład przez dodanie `@using System.Web.Mvc` do kodu).</span><span class="sxs-lookup"><span data-stu-id="c9b6f-200">In order to use the `TagBuilder` class (for example, in a custom ASP.NET Razor helper), you must reference the namespace (for example, by adding `@using System.Web.Mvc` to your code).</span></span>

#### <a name="change-request-validation-syntax-changed-validation-class-removed"></a><span data-ttu-id="c9b6f-201">Zmiana: zmieniono składnię walidacji żądania; Usunięto klasę "Walidacja"</span><span class="sxs-lookup"><span data-stu-id="c9b6f-201">Change: Request validation syntax changed; "Validation" class removed</span></span>

> <span data-ttu-id="c9b6f-202">W wersji beta 3, aby wyłączyć walidację dla pojedynczego pola lub zestawu pól, można wywołać metodę `Validation.Exclude`, przekazując nazwę lub nazwy pól, które mają zostać wykluczone z walidacji.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-202">In the Beta 3 release, to disable validation for an individual field or set of fields, you can call the `Validation.Exclude` method, passing in the name or names of the fields to exclude from validation.</span></span> <span data-ttu-id="c9b6f-203">Nowa składnia jest dostępna w wersji beta 3 w celu obejścia walidacji.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-203">A new syntax is available in the Beta 3 release for bypassing validation.</span></span> <span data-ttu-id="c9b6f-204">Metoda `Validation` użyta w wersji beta 3 została usunięta.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-204">The `Validation` method used in Beta 3 has been removed.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="c9b6f-205">Jeśli nie wyłączysz walidacji żądania, jeśli użytkownicy spróbują przekazać znaczniki HTML (na przykład przy użyciu edytora tekstu sformatowanego na stronie), witryna internetowa zgłosi błąd, jak *potencjalnie niebezpieczne żądanie. wartość formularza została wykryta przez klienta* , a dane wejściowe użytkownika nie są akceptowane.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-205">If you do not disable request validation, if users try to upload HTML markup (for example, by using a rich text editor on a page), the website will report an error like *A potentially dangerous Request.Form value was detected from the client* and the user input is not accepted.</span></span> <span data-ttu-id="c9b6f-206">Jeśli wyłączysz weryfikację żądań, musisz ręcznie sprawdzić dane wprowadzane przez użytkownika, aby upewnić się, że nie zawiera potencjalnie niebezpiecznego znacznika lub skryptu korzystającego ze podobnej do [biblioteki skryptów Microsoft Anti-Cross Site Scripting Library v 4.0](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=f4cd231b-7e06-445b-bec7-343e5884e651).</span><span class="sxs-lookup"><span data-stu-id="c9b6f-206">If you disable request validation, you must manually check user input to make sure that it does not contain potentially dangerous markup or script using something like the [Microsoft Anti-Cross Site Scripting Library V4.0](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=f4cd231b-7e06-445b-bec7-343e5884e651).</span></span>
> 
> 
> <span data-ttu-id="c9b6f-207">Aby wyłączyć automatyczne sprawdzanie poprawności żądań, wywołaj metodę `Request.Unvalidated`, przekazując ją do nazwy pola lub innego obiektu post, dla którego chcesz pominąć sprawdzanie poprawności żądania.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-207">To disable automatic request validation, call the `Request.Unvalidated` method, passing it the name of the field or other post object that you want to bypass request validation for.</span></span> <span data-ttu-id="c9b6f-208">Tej metody można użyć do obejścia walidacji dla wszystkich elementów w `Form`, `QueryString`, `Cookies`i `ServerVariables` kolekcji.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-208">You can use this method to bypass validation for any items in the `Form`, `QueryString`, `Cookies`, and `ServerVariables` collections.</span></span> <span data-ttu-id="c9b6f-209">W poniższych przykładach pokazano, jak używać metody `Unvalidated`:</span><span class="sxs-lookup"><span data-stu-id="c9b6f-209">The following examples show how to use the `Unvalidated` method:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample4.cs)]

<a id="Issues"></a>

#### <a name="known-issues-for-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="c9b6f-210">Znane problemy dotyczące ASP.NET stron sieci Web ze składnią Razor</span><span class="sxs-lookup"><span data-stu-id="c9b6f-210">Known Issues for ASP.NET Web Pages with Razor Syntax</span></span>

#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a><span data-ttu-id="c9b6f-211">Problem: nieoczekiwane zachowanie podczas korzystania z niestandardowej tabeli użytkownika na potrzeby członkostwa</span><span class="sxs-lookup"><span data-stu-id="c9b6f-211">Issue: Unexpected behavior when using a custom user table for membership</span></span>

> <span data-ttu-id="c9b6f-212">Aby zainicjować dostawcę członkostwa dla witryny sieci Web ASP.NET Razor, należy wywołać metodę `WebSecurity.InitializeDatabaseConnection`.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-212">To initialize the membership provider for an ASP.NET Razor website, you call the `WebSecurity.InitializeDatabaseConnection` method.</span></span> <span data-ttu-id="c9b6f-213">(W programie WebMatrix szablon witryny Starter zawiera wywołanie tej metody w pliku *\_AppStart. cshtml* ). Jeśli parametr `autoCreateTables` tej metody ma wartość true (domyślnie jest ustawiona wartość true w szablonie witryny Starter), a jeśli Nierozpoznana nazwa tabeli jest przenoszona do metody (drugi parametr), metoda nie zgłasza błędu.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-213">(In WebMatrix, the Starter Site template includes a call to this method in the *\_AppStart.cshtml* file.) If the `autoCreateTables` parameter of this method is set to true (by default, it is set to true in the Starter Site template), and if an unrecognized table name is passed to the method (the second parameter), the method does not throw an error.</span></span> <span data-ttu-id="c9b6f-214">Zamiast tego automatycznie tworzy tabelę.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-214">Instead, it automatically creates the table.</span></span>
> 
> <span data-ttu-id="c9b6f-215">Może to być problem, jeśli zamierzasz użyć niestandardowej tabeli użytkownika do członkostwa, ale Przekaż nieprawidłową nazwę tabeli do metody `WebSecurity.InitializeDatabaseConnection`.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-215">This can be a problem if you intend to use a custom user table for membership but pass the wrong table name to the `WebSecurity.InitializeDatabaseConnection` method.</span></span> <span data-ttu-id="c9b6f-216">Ponieważ metoda nie jest domyślnie zgłaszana błędem, jeśli określona tabela nie istnieje i ponieważ zamiast niej tworzy nową tabelę, aplikacja może wydawać się niedziałająca.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-216">Because the method does not by default raise an error if the table you specify does not exist, and because it instead creates a new table, the application can appear to be working.</span></span> <span data-ttu-id="c9b6f-217">Jednak kod aplikacji, który zależy od niestandardowej tabeli użytkownika (i znajdujących się w nim pól) może zakończyć się niepowodzeniem z nieoczekiwanymi błędami.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-217">However, application code that relies on your custom user table (and on fields in it) can eventually fail with unexpected errors.</span></span>
> 
> <span data-ttu-id="c9b6f-218">**Obejście**</span><span class="sxs-lookup"><span data-stu-id="c9b6f-218">**Workaround**</span></span>  
> <span data-ttu-id="c9b6f-219">Upewnij się, że nazwa przenoszona przez metodę `InitializeDatabaseConnection` jest zgodna z tabelą profilu użytkownika w bazie danych członkostwa, lub upewnij się, że parametr `autoCreateTables` ma wartość false.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-219">Make sure that the name passed in the `InitializeDatabaseConnection` method matches the user profile table in the membership database, or make sure that the `autoCreateTables` parameter is set to false.</span></span>

#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a><span data-ttu-id="c9b6f-220">Problem: "nie można wygenerować wystąpienia użytkownika SQL Server"</span><span class="sxs-lookup"><span data-stu-id="c9b6f-220">Issue: "Failed to generate a user instance of SQL Server" error</span></span>

> <span data-ttu-id="c9b6f-221">Jeśli aplikacja sieci Web WebMatrix używa SQL Server Express i korzysta z usług IIS 7,5 w systemie Windows 7 lub Windows Server 2008 R2, może zostać wyświetlony komunikat o błędzie z informacją, że SQL Server nie może pobrać lokalnej ścieżki aplikacji użytkownika w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-221">If a WebMatrix Web application uses SQL Server Express and is running IIS 7.5 on Windows 7 or Windows Server 2008 R2, you might see an error that indicates that SQL Server cannot retrieve the user's local application path at run time.</span></span>
> 
> <span data-ttu-id="c9b6f-222">**Obejście problemu** Upewnij się, że konto systemu Windows, w którym jest uruchomiona aplikacja (zazwyczaj usługa sieciowa) ma uprawnienia do odczytu i zapisu dla folderów głównych aplikacji oraz dla podfolderów, takich jak *aplikacja\_dane*.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-222">**Workaround** Make sure that the Windows account that the application runs under (typically NETWORK SERVICE) has read/write permissions for root folders of the application and for subfolders such as *App\_Data*.</span></span> <span data-ttu-id="c9b6f-223">Bardziej szczegółowe informacje są dostępne w artykule bazy wiedzy [problemy związane z SQL Server Express wystąpieniami użytkowników i projektami aplikacji sieci Web ASP.NET](https://support.microsoft.com/kb/2002980).</span><span class="sxs-lookup"><span data-stu-id="c9b6f-223">More detailed information is available in the KnowledgeBase article [Problems with SQL Server Express user instancing and ASP.net Web Application Projects](https://support.microsoft.com/kb/2002980).</span></span>

#### <a name="issue-in-visual-studio-namespaces-for-custom-assemblies-dlls-are-not-imported-automatically"></a><span data-ttu-id="c9b6f-224">Problem: w programie Visual Studio obszary nazw dla zestawów niestandardowych (dll) nie są importowane automatycznie</span><span class="sxs-lookup"><span data-stu-id="c9b6f-224">Issue: In Visual Studio, namespaces for custom assemblies (DLLs) are not imported automatically</span></span>

> <span data-ttu-id="c9b6f-225">Jeśli używasz niestandardowych zestawów w projekcie w programie Visual Studio, obszary nazw zadeklarowane w tych zestawach nie są automatycznie importowane w czasie projektowania.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-225">If you use custom assemblies in a project in Visual Studio, the namespaces declared in those assemblies are not automatically imported at design time.</span></span> <span data-ttu-id="c9b6f-226">W związku z tym odwołania do typów niestandardowych mogą nie być rozpoznawane w czasie projektowania i są oznaczane jako nierozpoznane w programie Visual Studio (przy użyciu "zygzak").</span><span class="sxs-lookup"><span data-stu-id="c9b6f-226">As a result, references to custom types might not be recognized at design time and are marked as not recognized in Visual Studio (using a "squiggle").</span></span> <span data-ttu-id="c9b6f-227">Ten problem występuje tylko w czasie projektowania w programie Visual Studio; sama aplikacja działa prawidłowo.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-227">This problem occurs only at design time in Visual Studio; the application itself runs properly.</span></span>
> 
> <span data-ttu-id="c9b6f-228">**Obejście**</span><span class="sxs-lookup"><span data-stu-id="c9b6f-228">**Workaround**</span></span>  
> <span data-ttu-id="c9b6f-229">Dołącz instrukcję `using` (`imports` w Visual Basic), która odwołuje się do jednostek, które nie są rozpoznawane w czasie projektowania.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-229">Include a `using` statement (`imports` in Visual Basic) that references the entities that are not recognized at design time.</span></span>

#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a><span data-ttu-id="c9b6f-230">Problem: program Visual Studio IntelliSense i szablony projektów są dostępne tylko w ASP.NET MVC w wersji 3</span><span class="sxs-lookup"><span data-stu-id="c9b6f-230">Issue: Visual Studio IntelliSense and project templates available only in ASP.NET MVC version 3</span></span>

> <span data-ttu-id="c9b6f-231">Instalowanie stron sieci Web ASP.NET nie instaluje również narzędzi dla programu Visual Studio, takich jak IntelliSense i szablony projektów dla aplikacji ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-231">Installing ASP.NET Web Pages does not also install tools for Visual Studio such as IntelliSense and project templates for ASP.NET Web Pages applications.</span></span>
> 
> <span data-ttu-id="c9b6f-232">**Obejście problemu** Aby użyć funkcji IntelliSense i szablonów projektu dla aplikacji ASP.NET Web Pages w programie Visual Studio, zainstaluj ASP.NET MVC 3 RC przez Instalatora platformy sieci Web lub [autonomicznego Instalatora](https://go.microsoft.com/fwlink/?LinkID=191797).</span><span class="sxs-lookup"><span data-stu-id="c9b6f-232">**Workaround** To use IntelliSense and project templates for ASP.NET Web Pages applications in Visual Studio, install ASP.NET MVC 3 RC either through the Web Platform Installer or the [stand-alone installer](https://go.microsoft.com/fwlink/?LinkID=191797).</span></span>

#### <a name="issue-lthelpergt-class-cannot-be-found-error"></a><span data-ttu-id="c9b6f-233">Problem: nie można odnaleźć klasy&gt; pomocnika&lt;"błąd</span><span class="sxs-lookup"><span data-stu-id="c9b6f-233">Issue: "&lt;helper&gt; class cannot be found" error</span></span>

> <span data-ttu-id="c9b6f-234">Po uaktualnieniu do wersji beta 3 może zostać wyświetlony błąd, że nie można odnaleźć klasy pomocnika (na przykład klasy `Facebook`).</span><span class="sxs-lookup"><span data-stu-id="c9b6f-234">After you upgrade to Beta 3, you might see an error that a helper class (for example, the `Facebook` class) cannot not be found.</span></span> <span data-ttu-id="c9b6f-235">Począwszy od wersji beta 2 i kontynuując wersję beta 3, pomocnicy zostały przesunięte do pakietów, które należy jawnie zainstalować.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-235">Starting in Beta 2 and continuing in Beta 3, helpers have been moved to packages that you must explicitly install.</span></span> <span data-ttu-id="c9b6f-236">Istniejące lokacje nie są uaktualnione w celu uwzględnienia tych pakietów; obejmuje to lokacje w folderach *\Moje Documents\IISExpress* lub *\Moje Dokumenty\moje witryny sieci Web* .</span><span class="sxs-lookup"><span data-stu-id="c9b6f-236">Existing sites are not upgraded to include these packages; this includes sites in the *\My Documents\IISExpress* or *\My Documents\My Web Sites* folders.</span></span> <span data-ttu-id="c9b6f-237">W szczególności zobaczysz ten błąd, jeśli używasz domyślnej lokacji w *witrynach* (WebSite1), która zawiera odwołanie do pomocnika `Twitter`.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-237">In particular, you will see this error if you use the default site in *My Sites* (WebSite1), which includes a reference to the `Twitter` helper.</span></span>
> 
> <span data-ttu-id="c9b6f-238">**Obejście**</span><span class="sxs-lookup"><span data-stu-id="c9b6f-238">**Workaround**</span></span>  
> <span data-ttu-id="c9b6f-239">Należy skomentować wywołania do dowolnych pomocników w lokacji, uruchomić stronę *administratora\_* i zainstalować pakiet lub pakiety zawierające pomocników, których chcesz użyć.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-239">Comment out calls to any helpers in the site, run the *\_Admin* page, and install the package or packages that include the helpers that you want to use.</span></span> <span data-ttu-id="c9b6f-240">Po zainstalowaniu pakietu można usunąć komentarz do wierszy, które odwołują się do pomocników.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-240">After you've installed the package, you can uncomment the lines that reference helpers.</span></span>

#### <a name="issue-deploying-beta-3-aspnet-razor-assemblies-to-the-bin-folder-might-not-work-on-hosting-sites"></a><span data-ttu-id="c9b6f-241">Problem: wdrażanie zestawów ASP.NET w wersji beta 3 do folderu bin może nie zadziałać w witrynach hostingu</span><span class="sxs-lookup"><span data-stu-id="c9b6f-241">Issue: Deploying Beta 3 ASP.NET Razor assemblies to the Bin folder might not work on hosting sites</span></span>

> <span data-ttu-id="c9b6f-242">W przypadku wdrażania witryny internetowej ASP.NET Web Pages w lokacji hostingu i wdrożenia zestawów ASP.NET Razor beta 3 do folderu *bin* lokacji mogą wystąpić błędy, w tym następujące:</span><span class="sxs-lookup"><span data-stu-id="c9b6f-242">If you deploy an ASP.NET Web Pages website to a hosting site, and if you deploy the ASP.NET Razor Beta 3 assemblies to the site's *Bin* folder, you might experience errors, including the following:</span></span>
> 
> `Could not load type 'Microsoft.Web.Infrastructure.DynamicModuleHelper.DynamicModuleUtility' from assembly 'Microsoft.Web.Infrastructure, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'.`
> 
> <span data-ttu-id="c9b6f-243">Taka sytuacja może wystąpić, jeśli dostawca hostingu zainstalował ASP.NET strony sieci Web beta 1 zestawów w globalnej pamięci podręcznej aplikacji serwera.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-243">This can happen if the hosting provider has installed the ASP.NET Web Pages Beta 1 assemblies into the server's global application cache (GAC).</span></span> <span data-ttu-id="c9b6f-244">Zestawy w pamięci podręcznej GAC mają pierwszeństwo przed zestawami zainstalowanymi lokalnie w folderze *bin* .</span><span class="sxs-lookup"><span data-stu-id="c9b6f-244">Assemblies in the GAC get precedence over assemblies installed locally in the *Bin* folder.</span></span>
> 
> <span data-ttu-id="c9b6f-245">**Obejście problemu** Skontaktuj się z dostawcą hostingu, aby upewnić się, że wykryte błędy są spowodowane konfliktem między wersjami zestawów a dostawcą.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-245">**Workaround** Contact your hosting provider to confirm that the errors you are seeing are due to a conflict between the provider's versions of the assemblies and yours.</span></span> <span data-ttu-id="c9b6f-246">W takim przypadku należy zażądać, aby dostawca hostingu zaktualizował zestawy w pamięci podręcznej GAC serwera.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-246">If so, request that the hosting provider update the assemblies in the server's GAC.</span></span>

#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a><span data-ttu-id="c9b6f-247">Problem: odczytywanie kanałów informacyjnych lub innych danych zewnętrznych za pośrednictwem serwera proxy</span><span class="sxs-lookup"><span data-stu-id="c9b6f-247">Issue: Reading feeds or other external data via a proxy server</span></span>

> <span data-ttu-id="c9b6f-248">Jeśli serwer z uruchomioną lokacją znajduje się za serwerem proxy, może być konieczne skonfigurowanie informacji o serwerze proxy w pliku *Web. config* , aby można było odczytywać informacje pochodzące spoza witryny.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-248">If the server running the site is behind a proxy server, you might need to configure proxy information in the *Web.config* file in order to be able to read information that comes from outside your site.</span></span> <span data-ttu-id="c9b6f-249">Jeśli na przykład użyjesz pomocnika `ReCaptcha`, pomocnik komunikuje się z usługą reCAPTCHA, ale może być blokowany przez serwer proxy.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-249">For example, if you use the `ReCaptcha` helper, the helper communicates with the reCAPTCHA service, but might be blocked by your proxy server.</span></span> <span data-ttu-id="c9b6f-250">Podobnie, kanały informacyjne, które są używane na stronach sieci Web ASP.NET, takie jak źródło danych używane przez Menedżera pakietów, mogą wymagać konfiguracji serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-250">Similarly, feeds that are used in ASP.NET Web Pages, such as the feed used by the package manager, might require proxy configuration.</span></span>
> 
> <span data-ttu-id="c9b6f-251">Jeśli wystąpią problemy podczas pracy z usługą zewnętrzną lub praca z kanałem informacyjnym pakietu, umieść następujące elementy w głównym pliku *Web. config* aplikacji:</span><span class="sxs-lookup"><span data-stu-id="c9b6f-251">If you experience problems in working with an external service or working with the package feed, put the following elements into your application's root *Web.config* file:</span></span>
> 
> [!code-xml[Main](beta3/samples/sample5.xml)]
> 
> <span data-ttu-id="c9b6f-252">Aby uzyskać więcej informacji o konfigurowaniu serwera proxy, zobacz [&lt;proxy&gt; element (Ustawienia sieci)](https://msdn.microsoft.com/library/sa91de1e.aspx) w witrynie MSDN w sieci Web.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-252">For more information about configuring a proxy server, see [&lt;proxy&gt; Element (Network Settings)](https://msdn.microsoft.com/library/sa91de1e.aspx) on the MSDN Web site.</span></span>

#### <a name="issue-microsoftwebinfrastructuredll-cannot-be-loaded-error"></a><span data-ttu-id="c9b6f-253">Problem: nie można załadować pliku "Microsoft. Web. Infrastructure. dll"</span><span class="sxs-lookup"><span data-stu-id="c9b6f-253">Issue: "Microsoft.Web.Infrastructure.dll cannot be loaded" error</span></span>

> <span data-ttu-id="c9b6f-254">Jeśli wcześniej zainstalowano wersję beta 1 stron sieci Web ASP.NET z składnia Razor a następnie Zainstaluj wersję beta 3, wszystkie odpowiednie zestawy są instalowane w pamięci GAC z wyjątkiem *Microsoft. Web. Infrastructure. dll*.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-254">If you previously installed the Beta 1 version of ASP.NET Web Pages with Razor syntax and then install the Beta 3 version, all appropriate assemblies are installed in the GAC except *Microsoft.Web.Infrastructure.dll*.</span></span> <span data-ttu-id="c9b6f-255">W związku z tym po uruchomieniu stron Razor ASP.NET zostanie wyświetlony komunikat o błędzie informujący o tym, że nie można załadować *pliku Microsoft. Web. Infrastructure. dll* .</span><span class="sxs-lookup"><span data-stu-id="c9b6f-255">As a consequence, when you run ASP.NET Razor pages, you see an error that indicates that *Microsoft.Web.Infrastructure.dll* could not be loaded.</span></span>
> 
> <span data-ttu-id="c9b6f-256">Ten problem nie występuje, jeśli wersja beta 3 została załadowana na czystym komputerze.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-256">This issue does not occur if you loaded the Beta 3 release on a clean computer.</span></span>
> 
> <span data-ttu-id="c9b6f-257">**Obejście**</span><span class="sxs-lookup"><span data-stu-id="c9b6f-257">**Workaround**</span></span>  
> <span data-ttu-id="c9b6f-258">W panelu sterowania odinstaluj ASP.NET strony sieci Web.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-258">In Control Panel, uninstall ASP.NET Web Pages.</span></span> <span data-ttu-id="c9b6f-259">Następnie zainstaluj ponownie wersję beta 3.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-259">Then reinstall the Beta 3 release.</span></span>

#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="c9b6f-260">Problem: Odinstalowywanie .NET Framework w wersji 4 wyłącza ASP.NET stron sieci Web ze składnią Razor</span><span class="sxs-lookup"><span data-stu-id="c9b6f-260">Issue: Uninstalling the .NET Framework version 4 disables ASP.NET Web Pages with Razor Syntax</span></span>

> <span data-ttu-id="c9b6f-261">Jeśli odinstalujesz .NET Framework w wersji 4, a następnie zainstalujesz ją ponownie, ASP.NET strony sieci Web z składnia Razor jest wyłączone.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-261">If you uninstall the .NET Framework version 4 and then reinstall it, ASP.NET Web Pages with Razor syntax is disabled.</span></span> <span data-ttu-id="c9b6f-262">Strony z rozszerzeniem *. cshtml* nie działają poprawnie.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-262">Pages with the *.cshtml* extension do not run correctly.</span></span> <span data-ttu-id="c9b6f-263">Strony sieci Web ASP.NET rejestrują zestaw w głównym pliku *Web. config* maszyny, a usunięcie .NET Framework usuwa ten plik.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-263">ASP.NET Web Pages registers an assembly in the machine root *Web.config* file, and removing the .NET Framework removes that file.</span></span> <span data-ttu-id="c9b6f-264">Ponowne zainstalowanie .NET Framework instaluje nową wersję pliku konfiguracji, ale nie dodaje odwołania do zestawu stron sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-264">Reinstalling the .NET Framework installs a new version of the configuration file, but does not add the reference for the ASP.NET Web Pages assembly.</span></span>
> 
> <span data-ttu-id="c9b6f-265">**Obejście problemu** Po ponownym zainstalowaniu .NET Framework ponownie zainstaluj strony sieci Web ASP.NET z składnia Razor.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-265">**Workaround** After reinstalling the .NET Framework, reinstall ASP.NET Web Pages with Razor syntax.</span></span> <span data-ttu-id="c9b6f-266">Spowoduje to dodanie następującego elementu do pliku *Web. config* w katalogu głównym maszyny, który zwykle znajduje się w następującej lokalizacji:</span><span class="sxs-lookup"><span data-stu-id="c9b6f-266">This adds the following element to the *Web.config* file in the machine root, which is typically in the following location:</span></span>  
> 
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> 
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](beta3/samples/sample6.xml)]

#### <a name="issue-applications-previously-deployed-with-aspnet-assemblies-in-the-bin-folder-experience-errors"></a><span data-ttu-id="c9b6f-267">Problem: aplikacje wdrożone wcześniej z zestawami ASP.NET w folderze bin zawierają błędy</span><span class="sxs-lookup"><span data-stu-id="c9b6f-267">Issue: Applications previously deployed with ASP.NET assemblies in the Bin folder experience errors</span></span>

> <span data-ttu-id="c9b6f-268">Podczas wdrażania kopie zestawów stron sieci Web ASP.NET (na przykład *Microsoft. Webpages. dll*) do folderu *bin* witryny sieci Web na serwerze.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-268">During deployment, copies of the ASP.NET Web Pages assemblies (for example, *Microsoft.WebPages.dll*) to the *Bin* folder of the website on the server.</span></span> <span data-ttu-id="c9b6f-269">(Mogło to nastąpiło automatycznie podczas wdrażania lub, ponieważ deweloper jawnie skopiował zestawy). Jeśli jednak wersja beta 3 jest zainstalowana, występują błędy, takie jak błędy, które nie mogą zostać znalezione.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-269">(This might have happened automatically during deployment or because the developer explicitly copied the assemblies.) However, when the Beta 3 release is installed, errors occurs, such as errors that certain types cannot be found.</span></span> <span data-ttu-id="c9b6f-270">Dzieje się tak, ponieważ wiele typów stron sieci Web ASP.NET zostało przeniesionych do różnych obszarów nazw dla wersji beta 3.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-270">This occurs because a number of ASP.NET Web Pages types were moved into different namespaces for the Beta 3 release.</span></span>
> 
> <span data-ttu-id="c9b6f-271">**Obejście** </span><span class="sxs-lookup"><span data-stu-id="c9b6f-271">**Workaround** </span></span>  
> <span data-ttu-id="c9b6f-272">Usuń zaznaczenie folderu *bin* wdrożonej aplikacji, skopiuj nowe zestawy do folderu (lub ponownie Wdróż aplikację), a następnie uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-272">Clear the *Bin* folder of the deployed application, copy the new assemblies to the folder (or redeploy the application), and then restart the application.</span></span>

#### <a name="issue-extensionless-urls-do-not-find-cshtmlvbhtml-files-on-iis-7-or-iis-75"></a><span data-ttu-id="c9b6f-273">Problem: w przypadku adresów URL bez rozszerzenia nie znajdują się pliki cshtml/. vbhtml w usługach IIS 7 lub IIS 7,5</span><span class="sxs-lookup"><span data-stu-id="c9b6f-273">Issue: Extensionless URLs do not find .cshtml/.vbhtml files on IIS 7 or IIS 7.5</span></span>

> <span data-ttu-id="c9b6f-274">W przypadku usług IIS 7 lub IIS 7,5 żądania z adresem URL podobnym do następującego nie mogą znaleźć stron, które mają rozszerzenie *. cshtml* lub *. vbhtml* :</span><span class="sxs-lookup"><span data-stu-id="c9b6f-274">On IIS 7 or IIS 7.5, requests with a URL like the following are not able to find pages that have the *.cshtml* or *.vbhtml* extension:</span></span>  
> 
> `http://www.example.com/ExampleSite/ExampleFile`  
> 
> <span data-ttu-id="c9b6f-275">Problem występuje, ponieważ ponowne zapisywanie adresów URL nie jest domyślnie włączone dla usług IIS 7 lub IIS 7,5.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-275">The issue arises because URL rewriting is not enabled by default for IIS 7 or IIS 7.5.</span></span> <span data-ttu-id="c9b6f-276">Likeliest scenariusz polega na tym, że problem nie jest wyświetlany podczas testowania lokalnego przy użyciu IIS Express, ale podczas wdrażania witryny sieci Web w witrynie sieci Web hostowanie jest to możliwe.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-276">The likeliest scenario is that you do not see the problem when testing locally using IIS Express, but you experience it when you deploy your website to a hosting website.</span></span>
> 
> <span data-ttu-id="c9b6f-277">**Obejście**</span><span class="sxs-lookup"><span data-stu-id="c9b6f-277">**Workaround**</span></span>
> 
> - <span data-ttu-id="c9b6f-278">Jeśli masz kontrolę nad komputerem serwera, na komputerze serwera Zainstaluj aktualizację, która jest opisana w [aktualizacji, która umożliwia obsługę określonych usług iis 7,0 lub iis 7,5 obsługi żądań, których adresy URL nie kończą się kropką](https://support.microsoft.com/kb/980368).</span><span class="sxs-lookup"><span data-stu-id="c9b6f-278">If you have control over the server computer, on the server computer install the update that is described in [A update is available that enables certain IIS 7.0 or IIS 7.5 handlers to handle requests whose URLs do not end with a period](https://support.microsoft.com/kb/980368).</span></span>
> - <span data-ttu-id="c9b6f-279">Jeśli nie masz kontroli nad komputerem serwera (na przykład wdrażasz je w witrynie sieci Web hostingu), Dodaj następujący kod do pliku *Web. config* witryny sieci Web:</span><span class="sxs-lookup"><span data-stu-id="c9b6f-279">If you do not have control over the server computer (for example, you are deploying to a hosting website), add the following to the website's *Web.config* file:</span></span>
> 
> 
> [!code-xml[Main](beta3/samples/sample7.xml)]

#### <a name="issue-using-web-application-project-or-aspnet-mvc-and-aspnet-web-pages-in-the-same-application"></a><span data-ttu-id="c9b6f-280">Problem: korzystanie z projektu aplikacji sieci Web lub ASP.NET MVC i ASP.NET stron sieci Web w tej samej aplikacji</span><span class="sxs-lookup"><span data-stu-id="c9b6f-280">Issue: Using Web Application Project or ASP.NET MVC and ASP.NET Web pages in the same application</span></span>

> <span data-ttu-id="c9b6f-281">Jeśli używasz ASP.NET stron sieci Web w projekcie aplikacji sieci Web lub aplikacji ASP.NET MVC, może zostać wyświetlony błąd, którego nie można znaleźć *WebPageHttpApplication* .</span><span class="sxs-lookup"><span data-stu-id="c9b6f-281">If you were using ASP.NET Web Pages in a Web Application project or ASP.NET MVC application, you might see an error that *WebPageHttpApplication* cannot be found.</span></span>
> 
> <span data-ttu-id="c9b6f-282">**Obejście**</span><span class="sxs-lookup"><span data-stu-id="c9b6f-282">**Workaround**</span></span>  
> <span data-ttu-id="c9b6f-283">Jeśli wystąpi ten błąd, Zmień klasę bazową, z której pochodzi aplikacja.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-283">If you get this error, change the base class from which the application derives.</span></span> <span data-ttu-id="c9b6f-284">W pliku *Global. asax* Zmień następujący wiersz:</span><span class="sxs-lookup"><span data-stu-id="c9b6f-284">In the *Global.asax* file, change the following line:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample8.cs)]
> 
> <span data-ttu-id="c9b6f-285">Do tego:</span><span class="sxs-lookup"><span data-stu-id="c9b6f-285">To this:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample9.cs)]
> 
> <span data-ttu-id="c9b6f-286">W efekcie ta zmiana została wprowadzona w przypadku wersji beta 1 programu ASP.NET stron sieci Web z programem składnia Razor.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-286">This in effect reverses a change that was introduced for the Beta 1 release of ASP.NET Web Pages with Razor syntax.</span></span>

#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a><span data-ttu-id="c9b6f-287">Problem: wdrażanie aplikacji na komputerze, na którym nie zainstalowano SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="c9b6f-287">Issue: Deploying an application to a computer that does not have SQL Server Compact installed</span></span>

> <span data-ttu-id="c9b6f-288">Aplikacje zawierające SQL Server Compact bazy danych można uruchamiać na komputerze, na którym SQL Server Compact nie jest zainstalowana.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-288">Applications that include SQL Server Compact databases can run on a computer where SQL Server Compact is not installed.</span></span> <span data-ttu-id="c9b6f-289">Program Microsoft WebMatrix beta 3 automatycznie kopiuje te pliki binarne i wykonuje transformacje pliku *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="c9b6f-289">Microsoft WebMatrix Beta 3 automatically copies these binaries for you and performs the appropriate *Web.config* file transforms.</span></span>
> 
> <span data-ttu-id="c9b6f-290">**Obejście problemu** Jeśli musisz skopiować te pliki i ręcznie wprowadzić zmiany w pliku *Web. config* , wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="c9b6f-290">**Workaround** If you need to copy these files and make the *Web.config* file changes manually, do the following :</span></span>
> 
> 1. <span data-ttu-id="c9b6f-291">Skopiuj zestawy aparatów bazy danych do folderu *bin* (i podfolderów) aplikacji na komputerze docelowym:</span><span class="sxs-lookup"><span data-stu-id="c9b6f-291">Copy the database engine assemblies to the *Bin* folder (and subfolders) of the application on the target computer:</span></span> 
> 
>     - <span data-ttu-id="c9b6f-292">Skopiuj *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* **do** *\Bin*</span><span class="sxs-lookup"><span data-stu-id="c9b6f-292">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* **to** *\Bin*</span></span>
>     - <span data-ttu-id="c9b6f-293">Skopiuj *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\* \* **do** *\Bin\x86*</span><span class="sxs-lookup"><span data-stu-id="c9b6f-293">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\*\* **to** *\Bin\x86*</span></span>
>     - <span data-ttu-id="c9b6f-294">Skopiuj *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\* \* **do** *\Bin\amd64*</span><span class="sxs-lookup"><span data-stu-id="c9b6f-294">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\*\* **to** *\Bin\amd64*</span></span>
> 2. <span data-ttu-id="c9b6f-295">W folderze głównym witryny sieci Web Utwórz lub Otwórz plik *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="c9b6f-295">In the root folder of the website, create or open a *Web.config* file.</span></span> <span data-ttu-id="c9b6f-296">(W programie WebMatrix beta 3 ten typ pliku jest dostępny po kliknięciu przycisku **wszystkie** w oknie dialogowym **Wybierz typ pliku** ).</span><span class="sxs-lookup"><span data-stu-id="c9b6f-296">(In WebMatrix Beta 3, this file type is available if you click **All** in the **Choose a File Type** dialog box.)</span></span>
> 3. <span data-ttu-id="c9b6f-297">Dodaj następujący element jako element podrzędny elementu **&lt;configuration&gt;** (nie wewnątrz elementu **&lt;system. Web&gt;** ):</span><span class="sxs-lookup"><span data-stu-id="c9b6f-297">Add the following element as a child of the **&lt;configuration&gt;** element (not inside the **&lt;system.web&gt;** element):</span></span>
> 
> 
> [!code-xml[Main](beta3/samples/sample10.xml)]

#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a><span data-ttu-id="c9b6f-298">Problem: pomocniki bazy danych i WebGrid nie działają w średnim zaufaniu w Visual Basic</span><span class="sxs-lookup"><span data-stu-id="c9b6f-298">Issue: Database and WebGrid helpers do not work in Medium Trust in Visual Basic</span></span>

> <span data-ttu-id="c9b6f-299">Jeśli używasz Visual Basic (Tworzenie plików *. vbhtml* ), pomocniki `Database` i `WebGrid` nie będą działały, jeśli aplikacja jest ustawiona do używania średniego zaufania.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-299">If you are using Visual Basic (creating *.vbhtml* files), the `Database` and `WebGrid` helpers will not work if the application is set to use Medium Trust.</span></span>
> 
> <span data-ttu-id="c9b6f-300">**Obejście**</span><span class="sxs-lookup"><span data-stu-id="c9b6f-300">**Workaround**</span></span>  
> <span data-ttu-id="c9b6f-301">Tymczasowo Ustaw aplikację tak, aby korzystała z pełnego zaufania.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-301">Temporarily set the application to use Full Trust.</span></span>

<a id="Known_Issues_SQL_Server_Compact"></a>
### <a name="sql-server-compact"></a><span data-ttu-id="c9b6f-302">SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="c9b6f-302">SQL Server Compact</span></span>

#### <a name="issue-encrypt-property-is-not-recognized"></a><span data-ttu-id="c9b6f-303">Problem: Właściwość "Szyfruj" nie została rozpoznana</span><span class="sxs-lookup"><span data-stu-id="c9b6f-303">Issue: "Encrypt" property is not recognized</span></span>

> <span data-ttu-id="c9b6f-304">SQL Server Compact 4,0 nie rozpoznaje właściwości `Encrypt` klasy `SqlCeConnection`.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-304">SQL Server Compact 4.0 does not recognize the `Encrypt` property of the `SqlCeConnection` class.</span></span> <span data-ttu-id="c9b6f-305">Nie należy używać tej właściwości do szyfrowania plików bazy danych.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-305">You should not use this property to encrypt database files.</span></span> <span data-ttu-id="c9b6f-306">Właściwość `Encrypt` była przestarzała w wersji SQL Server Compact 3,5 i została zachowana tylko w celu zapewnienia zgodności z poprzednimi wersjami.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-306">The `Encrypt` property was deprecated in SQL Server Compact 3.5 release and was retained only for backward compatibility.</span></span> 
> 
> <span data-ttu-id="c9b6f-307">**Obejście**</span><span class="sxs-lookup"><span data-stu-id="c9b6f-307">**Workaround**</span></span>  
> <span data-ttu-id="c9b6f-308">Użyj właściwości `Encryption Mode` klasy `SqlCeConnection`, aby szyfrować pliki bazy danych SQL Server Compact 4,0.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-308">Use the `Encryption Mode` property of the `SqlCeConnection` class to encrypt SQL Server Compact 4.0 database files.</span></span> <span data-ttu-id="c9b6f-309">Poniższy przykład przedstawia sposób tworzenia zaszyfrowanej bazy danych SQL Server Compact 4,0 przy użyciu właściwości `Encryption Mode`:</span><span class="sxs-lookup"><span data-stu-id="c9b6f-309">The following example shows how to create an encrypted SQL Server Compact 4.0 database using the `Encryption Mode` property:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample11.cs)]
> 
> [!code-vb[Main](beta3/samples/sample12.vb)]
> 
> <span data-ttu-id="c9b6f-310">Aby zmienić tryb szyfrowania istniejącej bazy danych SQL Server Compact 4,0, wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="c9b6f-310">To change the encryption mode of an existing SQL Server Compact 4.0 database, do the following:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample13.cs)]
> 
> [!code-vb[Main](beta3/samples/sample14.vb)]
> 
> <span data-ttu-id="c9b6f-311">Aby zaszyfrować niezaszyfrowaną bazę danych SQL Server Compact 4,0, wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="c9b6f-311">To encrypt an unencrypted SQL Server Compact 4.0 database, do the following:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample15.cs)]
> 
> [!code-vb[Main](beta3/samples/sample16.vb)]

#### <a name="issue-microsoft-visual-c-2008-runtime-libraries-are-required"></a><span data-ttu-id="c9b6f-312">Problem: biblioteki środowiska C++ uruchomieniowego programu Microsoft Visual 2008 są wymagane</span><span class="sxs-lookup"><span data-stu-id="c9b6f-312">Issue: Microsoft Visual C++ 2008 runtime libraries are required</span></span>

> <span data-ttu-id="c9b6f-313">Natywne biblioteki DLL SQL Server Compact 4,0 potrzebują bibliotek środowiska uruchomieniowego Microsoft Visual C++ 2008 (x86, IA64 i x64) z dodatkiem Service Pack 1.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-313">The native DLLs of SQL Server Compact 4.0 need the Microsoft Visual C++ 2008 Runtime Libraries (x86, IA64, and x64), Service Pack 1.</span></span>
> 
> <span data-ttu-id="c9b6f-314">**Obejście**</span><span class="sxs-lookup"><span data-stu-id="c9b6f-314">**Workaround**</span></span>  
> <span data-ttu-id="c9b6f-315">Zainstaluj .NET Framework 3,5 z dodatkiem SP1.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-315">Install the .NET Framework 3.5 SP1.</span></span> <span data-ttu-id="c9b6f-316">Spowoduje to również zainstalowanie bibliotek C++ środowiska uruchomieniowego Visual 2008 z dodatkiem SP1.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-316">This also installs the Visual C++ 2008 Runtime Libraries SP1.</span></span> <span data-ttu-id="c9b6f-317">Biblioteki można pobrać z następującej lokalizacji:</span><span class="sxs-lookup"><span data-stu-id="c9b6f-317">You can download the libraries from the following location:</span></span>   
>   
> [<span data-ttu-id="c9b6f-318">Aktualizacja zabezpieczeń C++ ATL pakietu redystrybucyjnego programu Microsoft Visual 2008 Service Pack 1</span><span class="sxs-lookup"><span data-stu-id="c9b6f-318">Microsoft Visual C++ 2008 Service Pack 1 Redistributable Package ATL Security Update</span></span>](https://go.microsoft.com/fwlink/?LinkId=194827)
> 
> [!NOTE]
> <span data-ttu-id="c9b6f-319">Należy zauważyć, że zainstalowanie .NET Framework 2,0, 3,0 lub 4 nie *instaluje bibliotek* środowiska uruchomieniowego Visual C++ 2008 SP1.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-319">Note that installing the .NET Framework 2.0, 3.0, or 4 does *not* install the Visual C++ 2008 Runtime Libraries SP1.</span></span>

#### <a name="issue-if-sql-server-compact-is-installed-prior-to-installing-net-framework-on-the-computer-its-provider-invariant-name-is-not-registered-in-the-net-framework-machineconfig-file"></a><span data-ttu-id="c9b6f-320">Problem: Jeśli zainstalowano SQL Server Compact przed zainstalowaniem .NET Framework na komputerze, jego niezmienna nazwa dostawcy nie jest zarejestrowana w pliku Machine. config .NET Framework</span><span class="sxs-lookup"><span data-stu-id="c9b6f-320">Issue: If SQL Server Compact is installed prior to installing .NET Framework on the computer, its provider invariant name is not registered in the .NET Framework machine.config file</span></span>

> <span data-ttu-id="c9b6f-321">SQL Server Compact można zainstalować na komputerze, na którym nie zainstalowano .NET Framework, ponieważ SQL Server Compact wymaga programu .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-321">SQL Server Compact can be installed on a machine that does not have .NET Framework installed because SQL Server Compact does require the .NET framework.</span></span> <span data-ttu-id="c9b6f-322">Jeśli przed zainstalowaniem programu SQL Server Compact nie zostanie zainstalowana żadna .NET Framework wersja 3,5 ani 4, Instalator SQL Server Compact nie zarejestruje nazwy niezmiennej dostawcy w pliku *Machine. config* .</span><span class="sxs-lookup"><span data-stu-id="c9b6f-322">If neither .NET Framework version 3.5 nor 4 is installed before you install SQL Server Compact, the SQL Server Compact Setup does not register its provider invariant name in the *machine.config* file.</span></span> <span data-ttu-id="c9b6f-323">Wszystkie aplikacje, które opierają się na wpisie SQL Server Compact w pliku *Machine. config* , zakończą się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-323">Any application that relies on the SQL Server Compact entry in the *machine.config* file will fail.</span></span> <span data-ttu-id="c9b6f-324">Wpis rejestracji nazwy niezmiennej w *pliku Machine. config* wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="c9b6f-324">The invariant name registration entry in *machine.config* looks like the following example:</span></span>
> 
> [!code-xml[Main](beta3/samples/sample17.xml)]
> 
> <span data-ttu-id="c9b6f-325">**Obejście**</span><span class="sxs-lookup"><span data-stu-id="c9b6f-325">**Workaround**</span></span>  
> <span data-ttu-id="c9b6f-326">Odinstaluj SQL Server Compact 4,0 CTP1.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-326">Uninstall SQL Server Compact 4.0 CTP1.</span></span> <span data-ttu-id="c9b6f-327">Pobierz i zainstaluj pełne wersje .NET Framework z następującej lokalizacji:</span><span class="sxs-lookup"><span data-stu-id="c9b6f-327">Download and install the full versions of the .NET Framework from the following location:</span></span>
> 
> [<span data-ttu-id="c9b6f-328">Microsoft .NET Framework 3,5 z dodatkiem Service Pack 1 (pełny pakiet)</span><span class="sxs-lookup"><span data-stu-id="c9b6f-328">Microsoft .NET Framework 3.5 Service pack 1 (Full Package)</span></span>](https://go.microsoft.com/fwlink/?LinkId=194828)  
> [<span data-ttu-id="c9b6f-329">Wydanie w Microsoft .NET Framework 4,0 (pełen pakiet)</span><span class="sxs-lookup"><span data-stu-id="c9b6f-329">Microsoft .NET Framework 4.0 Release (Full Package)</span></span>](https://www.microsoft.com/downloads/details.aspx?FamilyID=9cfb2d51-5ff4-4491-b0e5-b386f32c0992&amp;displaylang=en)
> 
> <span data-ttu-id="c9b6f-330">Następnie zainstaluj ponownie [SQL Server Compact 4,0 CTP1](https://www.microsoft.com/downloads/details.aspx?FamilyID=0d2357ea-324f-46fd-88fc-7364c80e4fdb&amp;displaylang=en).</span><span class="sxs-lookup"><span data-stu-id="c9b6f-330">Then reinstall [SQL Server Compact 4.0 CTP1](https://www.microsoft.com/downloads/details.aspx?FamilyID=0d2357ea-324f-46fd-88fc-7364c80e4fdb&amp;displaylang=en).</span></span>

<a id="Known_Issues_Installing_Applications"></a>

### <a name="installing-applications"></a><span data-ttu-id="c9b6f-331">Instalowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="c9b6f-331">Installing Applications</span></span>

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a><span data-ttu-id="c9b6f-332">Problem: instalacja aplikacji może zająć dużo czasu, jeśli folder Moje dokumenty użytkownika zostanie przekierowany do udziału sieciowego</span><span class="sxs-lookup"><span data-stu-id="c9b6f-332">Issue: Installing an application can take a long time if the user's My Documents folder is redirected to a network share</span></span>

> <span data-ttu-id="c9b6f-333">**Obejście**</span><span class="sxs-lookup"><span data-stu-id="c9b6f-333">**Workaround**</span></span>  
> <span data-ttu-id="c9b6f-334">Brak.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-334">None.</span></span> <span data-ttu-id="c9b6f-335">Instalacja aplikacji może potrwać trochę czasu, ale instalacja zostanie poprawna.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-335">The application might take a while to install, but will install correctly.</span></span>

<a id="Known_Issues_Publishing_Applications"></a>

### <a name="publishing-applications"></a><span data-ttu-id="c9b6f-336">Publikowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="c9b6f-336">Publishing Applications</span></span>

#### <a name="issue-site-might-not-work-after-publishing-if-the-destination-url-field-is-not-prefixed-with-http-or-https"></a><span data-ttu-id="c9b6f-337">Problem: witryna może nie zadziałać po opublikowaniu, jeśli pole "docelowy adres URL" nie jest poprzedzone prefiksem http://lub https://</span><span class="sxs-lookup"><span data-stu-id="c9b6f-337">Issue: Site might not work after publishing if the "Destination URL" field is not prefixed with http:// or https://</span></span>

> <span data-ttu-id="c9b6f-338">Jeśli docelowy adres URL nie zaczyna się od `http://` lub `https://`, w oknie dialogowym **Ustawienia publikowania** witryna może nie zadziałać po wdrożeniu.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-338">In the **Publishing Settings** dialog box, if the destination URL does not begin with `http://` or `https://`, the site might not work after deployment.</span></span>
> 
> <span data-ttu-id="c9b6f-339">**Obejście**</span><span class="sxs-lookup"><span data-stu-id="c9b6f-339">**Workaround**</span></span>  
> <span data-ttu-id="c9b6f-340">Przed opublikowaniem witryny upewnij się, że docelowy adres URL w oknie dialogowym **Ustawienia publikowania** zaczyna się od `http://` lub `https://`.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-340">Make sure that before you publish a site, the destination URL in the **Publish Settings** dialog box starts with `http://` or `https://`.</span></span>

#### <a name="issue-publishing-a-mysql-database-fails-with-the-error-failed-to-publish-the-database-this-can-happen-if-the-remote-database-cannot-run-the-script"></a><span data-ttu-id="c9b6f-341">Problem: Publikowanie bazy danych MySQL kończy się niepowodzeniem z powodu błędu "nie można opublikować bazy danych.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-341">Issue: Publishing a MySQL database fails with the error "Failed to publish the database.</span></span> <span data-ttu-id="c9b6f-342">Może się tak zdarzyć, jeśli zdalna baza danych nie może uruchomić skryptu ".</span><span class="sxs-lookup"><span data-stu-id="c9b6f-342">This can happen if the remote database cannot run the script."</span></span>

> <span data-ttu-id="c9b6f-343">Ten błąd może wystąpić z kilku powodów.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-343">The error can occur for a number of reasons.</span></span> <span data-ttu-id="c9b6f-344">Jedną z przyczyn tego błędu jest to, że ten błąd występuje, jeśli skrypt bazy danych zawiera znak pojedynczego cudzysłowu ('), a docelowy zestaw znaków bazy danych MySQL nie jest w formacie UTF-8.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-344">One reason you can see this error is if the database script contains a single quotation character (') and the destination MySQL database's default character set is not to UTF-8.</span></span>
> 
> <span data-ttu-id="c9b6f-345">**Obejście**</span><span class="sxs-lookup"><span data-stu-id="c9b6f-345">**Workaround**</span></span>  
> <span data-ttu-id="c9b6f-346">Ustaw domyślny zestaw znaków dla zdalnej bazy danych MySQL na UTF-8.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-346">Set the default character set for the remote MySQL database to UTF-8.</span></span>

<a id="Known_Issues_Other_Issues"></a>

### <a name="other-issues"></a><span data-ttu-id="c9b6f-347">Inne problemy</span><span class="sxs-lookup"><span data-stu-id="c9b6f-347">Other Issues</span></span>

#### <a name="issue-searchfilter-does-not-work-in-reports-for-group-by-issue-type"></a><span data-ttu-id="c9b6f-348">Problem: wyszukiwanie/filtrowanie nie działa w raportach dla elementu Grupuj według: typ problemu</span><span class="sxs-lookup"><span data-stu-id="c9b6f-348">Issue: Search/Filter does not work in Reports for Group By: Issue Type</span></span>

> <span data-ttu-id="c9b6f-349">Po uruchomieniu raportu dla witryny, jeśli wprowadzisz tekst w polu *Filtruj według adresu URL* , a następnie klikniesz przycisk *Wyszukaj*, nic się nie dzieje.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-349">When you run a report for a site, if you enter text in the *Filter by URL* box and click *Search*, nothing happens.</span></span> <span data-ttu-id="c9b6f-350">Jest to spowodowane tym, że ta kontrolka nie działa, podczas gdy *Grupa według* stanu raportu jest ustawiona na wartość *typ problemu*, co jest ustawieniem domyślnym.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-350">This is because this control is not functional while the *Group By* state of the report is set to *Issue Type*, which is the default.</span></span>
> 
> <span data-ttu-id="c9b6f-351">**Obejście problemu** Na karcie *Grupuj według* na wstążce kliknij pozycję *adres URL* , aby zgrupować wpisy według ich źródłowego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-351">**Workaround** In the *Group By* tab of ribbon, click *URL* to group the entries by their source URL.</span></span> <span data-ttu-id="c9b6f-352">Pole tekstowe i przycisk służący do filtrowania wpisów są funkcjonalne w tym stanie.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-352">The text box and button to filter the entries are functional while in this state.</span></span>

#### <a name="issue-wcf-applications-fail-to-run-with-iis-express"></a><span data-ttu-id="c9b6f-353">Problem: uruchamianie aplikacji WCF nie powiodło się z IIS Express</span><span class="sxs-lookup"><span data-stu-id="c9b6f-353">Issue: WCF applications fail to run with IIS Express</span></span>

> <span data-ttu-id="c9b6f-354">Przechodzenie do aplikacji WCF spowoduje wystąpienie błędu podobne do następującego:</span><span class="sxs-lookup"><span data-stu-id="c9b6f-354">Browsing to a WCF application results in an error like the following one:</span></span>
> 
> <span data-ttu-id="c9b6f-355">*Nie można załadować pliku lub zestawu "Microsoft. Web. Administration, version = 7.0.0.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35" lub jednej z jego zależności. System nie może znaleźć określonego pliku.*</span><span class="sxs-lookup"><span data-stu-id="c9b6f-355">*Could not load file or assembly 'Microsoft.Web.Administration, Version=7.0.0.0, Culture=neutral,PublicKeyToken=31bf3856ad364e35' or one of its dependencies. The system cannot find the file specified.*</span></span>
> 
> <span data-ttu-id="c9b6f-356">Dzieje się tak, ponieważ IIS Express wersja beta nie obsługuje domyślnie programu WCF.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-356">This occurs because IIS Express Beta release doesn't support WCF by default.</span></span>
> 
> <span data-ttu-id="c9b6f-357">**Obejście problemu** Użyj jednego z następujących obejść (obejście #2 wymaga systemu Microsoft Windows Vista lub nowszego):</span><span class="sxs-lookup"><span data-stu-id="c9b6f-357">**Workaround** Use any one of the following workarounds (workaround #2 requires Microsoft Windows Vista or higher):</span></span>
> 
> 
> 1. <span data-ttu-id="c9b6f-358">Skopiuj zestawy *Microsoft. Web. dll* i *Microsoft. Web. Administration. dll* z lokalizacji instalacji programu WebMatrix do katalogu *bin* aplikacji WCF.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-358">Copy the *Microsoft.Web.dll* and *Microsoft.Web.Administration.dll* assemblies from the WebMatrix installation location to the *bin* directory of the WCF application.</span></span> <span data-ttu-id="c9b6f-359">Domyślnie program WebMatrix jest instalowany w podfolderze *Microsoft WebMatrix* w folderze *Program Files* .</span><span class="sxs-lookup"><span data-stu-id="c9b6f-359">By default, WebMatrix is installed in the *Microsoft WebMatrix* subfolder under the system's *Program Files* folder.</span></span>
> 2. <span data-ttu-id="c9b6f-360">W systemie Microsoft Windows Vista lub nowszym Utwórz link symboliczny do zestawów w katalogu *bin* przy użyciu następujących poleceń.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-360">On Microsoft Windows Vista or higher, create a symlink to the assemblies in the *bin* directory using the following commands.</span></span> <span data-ttu-id="c9b6f-361">(Takie podejście ma zaletę, że nie tworzy kopii zestawów).</span><span class="sxs-lookup"><span data-stu-id="c9b6f-361">(This approach has the advantage that it does not create a copy of the assemblies.)</span></span>
> 
>     [!code-console[Main](beta3/samples/sample18.cmd)]
> 3. <span data-ttu-id="c9b6f-362">Zainstaluj dwa zestawy w pamięci podręcznej GAC.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-362">Install the two assemblies in the GAC.</span></span> <span data-ttu-id="c9b6f-363">W wierszu polecenia z podwyższonym poziomem uprawnień uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="c9b6f-363">From an elevated prompt, run the following commands:</span></span>
> 
>     [!code-console[Main](beta3/samples/sample19.cmd)]

#### <a name="issue-webmatrix-beta-3-is-unable-to-perform-certain-tasks-that-require-elevation"></a><span data-ttu-id="c9b6f-364">Problem: w programie WebMatrix beta 3 nie można wykonać określonych zadań wymagających podniesienia uprawnień</span><span class="sxs-lookup"><span data-stu-id="c9b6f-364">Issue: WebMatrix Beta 3 is unable to perform certain tasks that require elevation</span></span>

> <span data-ttu-id="c9b6f-365">Program WebMatrix beta 3 nie może wykonać niektórych zadań wymagających podniesienia uprawnień, takich jak instalowanie dodatkowych składników w następujących sytuacjach:</span><span class="sxs-lookup"><span data-stu-id="c9b6f-365">WebMatrix Beta 3 is unable to perform certain tasks that require elevation, such as installing additional components in the following situations:</span></span>
> 
> - <span data-ttu-id="c9b6f-366">W systemie Windows Vista lub Windows 7 użytkownik zalogował się przy użyciu konta, które nie ma uprawnień administracyjnych i kontrola konta użytkownika (UAC) jest wyłączona.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-366">On Windows Vista or Windows 7, you are logged in with an account that does not have administrative privileges and User Account Control (UAC) is disabled.</span></span>
> - <span data-ttu-id="c9b6f-367">Używasz systemu Microsoft Windows XP lub Microsoft Windows Server 2003.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-367">You are using Microsoft Windows XP or Microsoft Windows Server 2003.</span></span>
> 
> <span data-ttu-id="c9b6f-368">**Obejście**</span><span class="sxs-lookup"><span data-stu-id="c9b6f-368">**Workaround**</span></span>  
> <span data-ttu-id="c9b6f-369">Większość zadań w programie WebMatrix beta 3 nie wymaga uprawnień administracyjnych.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-369">Most tasks in WebMatrix Beta 3 do not require administrative permission.</span></span> <span data-ttu-id="c9b6f-370">Dla tych, które wykonują operację jako administrator, lub wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="c9b6f-370">For those that do, you can perform the operation as an administrator, or follow these steps:</span></span>
> 
> - <span data-ttu-id="c9b6f-371">W systemie Windows Vista lub Windows 7 Włącz kontrolę konta użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-371">On Windows Vista or Windows 7, enable UAC.</span></span>
> - <span data-ttu-id="c9b6f-372">W systemie Windows XP Dodaj użytkownika do grupy zabezpieczeń Administratorzy.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-372">On Windows XP, add the user to the Administrators security group.</span></span>

#### <a name="issue-site-from-web-gallery-is-disabled"></a><span data-ttu-id="c9b6f-373">Problem: "Witryna z galerii sieci Web" jest wyłączona</span><span class="sxs-lookup"><span data-stu-id="c9b6f-373">Issue: "Site from Web Gallery" is disabled</span></span>

> <span data-ttu-id="c9b6f-374">Opcja **Witryna z galerii sieci Web** jest wyłączona, jeśli nie zainstalowano Instalatora platformy sieci Web 3,0.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-374">The **Site from Web Gallery** option is disabled if the Web Platform Installer 3.0 is not installed.</span></span>
> 
> <span data-ttu-id="c9b6f-375">**Obejście**</span><span class="sxs-lookup"><span data-stu-id="c9b6f-375">**Workaround**</span></span>  
> <span data-ttu-id="c9b6f-376">Zainstaluj [Instalator platformy Microsoft Web 3,0](https://go.microsoft.com/fwlink/?LinkID=194638).</span><span class="sxs-lookup"><span data-stu-id="c9b6f-376">Install the [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span></span>

#### <a name="issue-on-windows-server-2003-iis-express-does-not-start-for-a-non-administrative-user"></a><span data-ttu-id="c9b6f-377">Problem: w systemie Windows Server 2003, IIS Express nie zostanie uruchomiony dla użytkownika niebędącego administratorami</span><span class="sxs-lookup"><span data-stu-id="c9b6f-377">Issue: On Windows Server 2003, IIS Express does not start for a non-administrative user</span></span>

> <span data-ttu-id="c9b6f-378">W systemie Windows Server 2003 podczas uruchamiania strony lub uruchamiania IIS Express, IIS Express nie zostanie uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-378">On Windows Server 2003, when you launch a page or start IIS Express, IIS Express does not start.</span></span> <span data-ttu-id="c9b6f-379">W przypadku stron sieci Web zostanie wyświetlony komunikat o błędzie informujący o tym, że aplikacja została uruchomiona przez użytkownika niebędącego administratorami.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-379">For Web pages, an error is displayed that indicates that the application has been started by a non-administrative user.</span></span>
> 
> <span data-ttu-id="c9b6f-380">**Obejście**</span><span class="sxs-lookup"><span data-stu-id="c9b6f-380">**Workaround**</span></span>  
> <span data-ttu-id="c9b6f-381">Uruchom aplikację WebMatrix beta 3 jako użytkownik administracyjny.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-381">Start WebMatrix Beta 3 as an administrative user.</span></span> <span data-ttu-id="c9b6f-382">Aby uzyskać więcej informacji, zobacz następujący artykuł bazy wiedzy:</span><span class="sxs-lookup"><span data-stu-id="c9b6f-382">For more details, see the following KnowledgeBase article:</span></span>  
>   
> [<span data-ttu-id="c9b6f-383">Aplikacja uruchamiana przez nieadministracyjnego użytkownika nie może nasłuchiwać ruchu HTTP komputera, na którym aplikacja jest uruchomiona w systemie Windows Vista, Windows Server 2003 lub Windows XP.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-383">An application that is started by a non-administrative user cannot listen to the HTTP traffic of the computer on which the application is running in Windows Vista, Windows Server 2003, or Windows XP.</span></span>](https://support.microsoft.com/kb/939786)

#### <a name="issue-google-chrome-is-not-available-as-a-run-option"></a><span data-ttu-id="c9b6f-384">Problem: Google Chrome nie jest dostępna jako opcja uruchamiania</span><span class="sxs-lookup"><span data-stu-id="c9b6f-384">Issue: Google Chrome is not available as a Run option</span></span>

> <span data-ttu-id="c9b6f-385">Przeglądarka Google Chrome nie jest wyświetlana na liście przeglądarek w obszarze **Uruchom** na karcie **Narzędzia główne** .</span><span class="sxs-lookup"><span data-stu-id="c9b6f-385">Google Chrome is not displayed in the list of browsers under **Run** on the **Home** tab.</span></span>
> 
> <span data-ttu-id="c9b6f-386">**Obejście**</span><span class="sxs-lookup"><span data-stu-id="c9b6f-386">**Workaround**</span></span>  
> <span data-ttu-id="c9b6f-387">Niektóre wersje usługi Google Chrome nie rejestrują się prawidłowo przy użyciu funkcji domyślnych programów w systemie Windows.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-387">Some versions of Google Chrome do not register themselves correctly with the Default Programs feature in Windows.</span></span> <span data-ttu-id="c9b6f-388">Aby obejść ten element, uruchom program Google Chrome, kliknij menu *Dostosowywanie i kontrolowanie Google Chrome* , kliknij pozycję *Opcje*, a następnie kliknij pozycję *Udostępnij Google Chrome moją domyślną przeglądarkę*.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-388">As a workaround, start Google Chrome, click the *Customize and control Google Chrome* menu, click *Options*, and then click *Make Google Chrome my default browser*.</span></span>

#### <a name="issue-the-foreign-key-dialog-box-doesnt-allow-entering-a-primary-key"></a><span data-ttu-id="c9b6f-389">Problem: okno dialogowe "klucz obcy" nie zezwala na wprowadzenie klucza podstawowego</span><span class="sxs-lookup"><span data-stu-id="c9b6f-389">Issue: The "Foreign Key" dialog box doesn't allow entering a primary key</span></span>

> <span data-ttu-id="c9b6f-390">Okno dialogowe **klucz obcy** nie pozwala na wprowadzenie nazwy klucza podstawowego z tabeli klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-390">The **Foreign Key** dialog box does not allow you to enter the primary key name from the primary key table.</span></span>
> 
> <span data-ttu-id="c9b6f-391">**Obejście**</span><span class="sxs-lookup"><span data-stu-id="c9b6f-391">**Workaround**</span></span>  
> <span data-ttu-id="c9b6f-392">Jest to zamierzone.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-392">This is intentional.</span></span> <span data-ttu-id="c9b6f-393">Nie trzeba wprowadzać nazwy klucza podstawowego z tabeli klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-393">You do not need to enter the name of the primary key from the primary key table.</span></span>

#### <a name="issue-the-relationships-button-is-disabled"></a><span data-ttu-id="c9b6f-394">Problem: przycisk "relacje" jest wyłączony</span><span class="sxs-lookup"><span data-stu-id="c9b6f-394">Issue: The "Relationships" button is disabled</span></span>

> <span data-ttu-id="c9b6f-395">Przycisk **relacje** na karcie **tabela** w obszarze roboczym **bazy danych** jest wyłączony dla SQL Server Compact baz danych.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-395">The **Relationships** button under the **Table** tab in the **Databases** workspace is disabled for SQL Server Compact databases.</span></span>
> 
> <span data-ttu-id="c9b6f-396">**Obejście**</span><span class="sxs-lookup"><span data-stu-id="c9b6f-396">**Workaround**</span></span>  
> <span data-ttu-id="c9b6f-397">Brak.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-397">None.</span></span> <span data-ttu-id="c9b6f-398">SQL Server Compact nie obsługuje relacji między tabelami.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-398">SQL Server Compact does not support relationships between tables.</span></span>

#### <a name="issue-parameterized-sql-queries-throw-exceptions"></a><span data-ttu-id="c9b6f-399">Problem: sparametryzowane zapytania SQL zgłaszają wyjątki</span><span class="sxs-lookup"><span data-stu-id="c9b6f-399">Issue: Parameterized SQL queries throw exceptions</span></span>

> <span data-ttu-id="c9b6f-400">W SQL Server Compact 4,0, jeśli nie określisz typu danych, takiego jak `SqlDbType` lub `DbType` dla parametrów w zapytaniach parametrycznych, zostanie zgłoszony wyjątek, gdy zostanie uruchomione zapytanie.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-400">In SQL Server Compact 4.0, if you do not specify a data type such as `SqlDbType` or `DbType` for parameters in parameterized queries, an exception is thrown when the query runs.</span></span>
> 
> <span data-ttu-id="c9b6f-401">**Obejście**</span><span class="sxs-lookup"><span data-stu-id="c9b6f-401">**Workaround**</span></span>  
> <span data-ttu-id="c9b6f-402">Jawnie ustaw typ danych dla parametrów, takich jak `SqlDbType` lub `DbType`.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-402">Explicitly set the data type for parameters such as `SqlDbType` or `DbType`.</span></span> <span data-ttu-id="c9b6f-403">Jest to krytyczne w przypadku typów danych obiektów BLOB (`image` i `ntext`).</span><span class="sxs-lookup"><span data-stu-id="c9b6f-403">This is critical in the case of BLOB data types (`image` and `ntext`).</span></span> <span data-ttu-id="c9b6f-404">Użyj kodu w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="c9b6f-404">Use code like the following:</span></span>
> 
> [!code-sql[Main](beta3/samples/sample20.sql)]
> 
> [!code-vb[Main](beta3/samples/sample21.vb)]

<a id="More_Info"></a>

## <a name="for-more-information"></a><span data-ttu-id="c9b6f-405">Aby uzyskać więcej informacji</span><span class="sxs-lookup"><span data-stu-id="c9b6f-405">For More Information</span></span>

<span data-ttu-id="c9b6f-406">Aby uzyskać więcej informacji o programie WebMatrix beta 3, zobacz następujące witryny sieci Web:</span><span class="sxs-lookup"><span data-stu-id="c9b6f-406">For more information about WebMatrix Beta 3, see the following websites:</span></span>

- [<span data-ttu-id="c9b6f-407">IIS.net</span><span class="sxs-lookup"><span data-stu-id="c9b6f-407">IIS.net</span></span>](http://iis.net/)
- [<span data-ttu-id="c9b6f-408">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c9b6f-408">ASP.NET</span></span>](https://asp.net/webmatrix)
- [<span data-ttu-id="c9b6f-409">Microsoft.com/web</span><span class="sxs-lookup"><span data-stu-id="c9b6f-409">Microsoft.com/web</span></span>](https://www.microsoft.com/web)

---

<span data-ttu-id="c9b6f-410">© 2010 Microsoft Corporation.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-410">© 2010 Microsoft Corporation.</span></span> <span data-ttu-id="c9b6f-411">Wszelkie prawa zastrzeżone.</span><span class="sxs-lookup"><span data-stu-id="c9b6f-411">All Rights Reserved.</span></span> <span data-ttu-id="c9b6f-412">[Warunki użytkowania](https://msdn.microsoft.cos/cc300389.aspx).</span><span class="sxs-lookup"><span data-stu-id="c9b6f-412">[Terms of Use](https://msdn.microsoft.cos/cc300389.aspx).</span></span>

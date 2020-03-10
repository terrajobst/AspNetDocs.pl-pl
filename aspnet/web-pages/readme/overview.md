---
uid: web-pages/readme/overview
title: Plik Readme WebMatrix | Microsoft Docs
author: rick-anderson
description: Informacje o wersji programu WebMatrix i ASP.NET Web Pages (Razor) 1,0
ms.author: riande
ms.date: 01/06/2011
ms.assetid: 36c5beeb-45a7-48a0-9c30-f82cdf5c5f5f
msc.legacyurl: /web-pages/readme
msc.type: content
ms.openlocfilehash: fac53e935860a90d8f2aa96699d56d66ade3a40f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78563470"
---
# <a name="webmatrix-readme"></a><span data-ttu-id="53550-103">Plik Readme programu WebMatrix</span><span class="sxs-lookup"><span data-stu-id="53550-103">WebMatrix Readme</span></span>

<span data-ttu-id="53550-104">13 stycznia 2011</span><span class="sxs-lookup"><span data-stu-id="53550-104">13 January 2011</span></span>

## <a name="contents"></a><span data-ttu-id="53550-105">Spis treści</span><span class="sxs-lookup"><span data-stu-id="53550-105">Contents</span></span>

> [!NOTE]
> <span data-ttu-id="53550-106">Ten plik Readme dotyczy wersji 1,0 programu WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="53550-106">This readme applies to the 1.0 release of WebMatrix.</span></span>

- [<span data-ttu-id="53550-107">Omówienie</span><span class="sxs-lookup"><span data-stu-id="53550-107">Overview</span></span>](#Overview)
- [<span data-ttu-id="53550-108">Instalacja</span><span class="sxs-lookup"><span data-stu-id="53550-108">Installation</span></span>](#Installation_Notes)
- [<span data-ttu-id="53550-109">Jak publikować aplikacje</span><span class="sxs-lookup"><span data-stu-id="53550-109">How to Publish Applications</span></span>](#InstructionsForPublishingApplications)
- [<span data-ttu-id="53550-110">Zmiany i problemy</span><span class="sxs-lookup"><span data-stu-id="53550-110">Changes and Issues</span></span>](#ChangesAndIssues)

    - [<span data-ttu-id="53550-111">Instalacja programu WebMatrix 1,0</span><span class="sxs-lookup"><span data-stu-id="53550-111">WebMatrix 1.0 Installation</span></span>](#Known_Issues_Installation)
    - [<span data-ttu-id="53550-112">ASP.NET Web Pages</span><span class="sxs-lookup"><span data-stu-id="53550-112">ASP.NET Web Pages</span></span>](#Known_Issues_ASPNET)
    - [<span data-ttu-id="53550-113">Programu WebMatrix</span><span class="sxs-lookup"><span data-stu-id="53550-113">WebMatrix</span></span>](#Known_Issues_WebMatrix)
    - [<span data-ttu-id="53550-114">IIS Express</span><span class="sxs-lookup"><span data-stu-id="53550-114">IIS Express</span></span>](#Known_Issues_IISExpress)
    - [<span data-ttu-id="53550-115">SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="53550-115">SQL Server Compact</span></span>](#Known_Issues_SQLServerCompact)
    - [<span data-ttu-id="53550-116">Instalowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="53550-116">Installing Applications</span></span>](#Known_Issues_Installing_Applications)
    - [<span data-ttu-id="53550-117">Publikowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="53550-117">Publishing Applications</span></span>](#Known_Issues_Publishing_Applications)
- [<span data-ttu-id="53550-118">Aby uzyskać więcej informacji</span><span class="sxs-lookup"><span data-stu-id="53550-118">For More Information</span></span>](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a><span data-ttu-id="53550-119">Omówienie</span><span class="sxs-lookup"><span data-stu-id="53550-119">Overview</span></span>

> <span data-ttu-id="53550-120">Microsoft WebMatrix 1,0 to bezpłatny stos rozwoju sieci Web, który jest instalowany w ciągu kilku minut.</span><span class="sxs-lookup"><span data-stu-id="53550-120">Microsoft WebMatrix 1.0 is a free web development stack that installs in minutes.</span></span> <span data-ttu-id="53550-121">Integruje on serwer sieci Web z bazami danych i strukturami programowania w celu utworzenia jednego, zintegrowanego środowiska.</span><span class="sxs-lookup"><span data-stu-id="53550-121">It integrates a web server with database and programming frameworks to create a single, integrated experience.</span></span> <span data-ttu-id="53550-122">Za pomocą programu WebMatrix można usprawnić wykonywanie kodu, testowanie i publikowanie własnej witryny sieci Web ASP.NET lub PHP lub użyć programu WebMatrix do uruchomienia nowej witryny sieci Web przy użyciu popularnych aplikacji typu "open source", takich jak DotNetNuke, Umbraco, WordPress lub Joomla.</span><span class="sxs-lookup"><span data-stu-id="53550-122">You can use WebMatrix to streamline the way you code, test, and publish your own ASP.NET or PHP website, or you can use WebMatrix to start a new website using popular open-source apps like DotNetNuke, Umbraco, WordPress, or Joomla.</span></span> <span data-ttu-id="53550-123">Program WebMatrix używa tego samego zaawansowanego serwera sieci Web, aparatu bazy danych i środowiska środowisk, które będą uruchamiać witrynę internetową w Internecie, co sprawia, że przejście od projektowania do produkcji płynnej i bezproblemowe.</span><span class="sxs-lookup"><span data-stu-id="53550-123">WebMatrix uses the same powerful web server, database engine, and frameworks environment that will run your website on the internet, which makes the transition from development to production smooth and seamless.</span></span>

<a id="Installation_Notes"></a>

## <a name="installation"></a><span data-ttu-id="53550-124">Instalacja</span><span class="sxs-lookup"><span data-stu-id="53550-124">Installation</span></span>

> <span data-ttu-id="53550-125">Aby zainstalować program WebMatrix 1,0, należy najpierw zainstalować [Instalator platformy Microsoft Web 3,0](https://go.microsoft.com/fwlink/?LinkID=194638).</span><span class="sxs-lookup"><span data-stu-id="53550-125">To install WebMatrix 1.0, you must first install the [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span></span> <span data-ttu-id="53550-126">Po zainstalowaniu Instalatora platformy sieci Web można go użyć do zainstalowania programu WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="53550-126">After you've installed the Web Platform Installer, you can use it to install WebMatrix.</span></span>
> 
> <span data-ttu-id="53550-127">Jeśli masz problemy podczas instalacji, zapoznaj się z [tematem Rozwiązywanie problemów z Instalator platformy Microsoft Web](https://go.microsoft.com/fwlink/?LinkId=196212).</span><span class="sxs-lookup"><span data-stu-id="53550-127">If you have problems during installation, refer to [Troubleshooting Problems with Microsoft Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=196212).</span></span>

<a id="InstructionsForPublishingApplications"></a>
## <a name="how-to-publish-applications"></a><span data-ttu-id="53550-128">Jak publikować aplikacje</span><span class="sxs-lookup"><span data-stu-id="53550-128">How to Publish Applications</span></span>

> <span data-ttu-id="53550-129">Zapoznaj się z [instrukcjami krok po kroku dotyczącymi publikowania aplikacji](https://go.microsoft.com/fwlink/?LinkID=196149)</span><span class="sxs-lookup"><span data-stu-id="53550-129">See [Step-by-Step Instructions for Publishing Applications](https://go.microsoft.com/fwlink/?LinkID=196149)</span></span>

<a id="ChangesAndIssues"></a>

## <a name="changes-and-issues"></a><span data-ttu-id="53550-130">Zmiany i problemy</span><span class="sxs-lookup"><span data-stu-id="53550-130">Changes and Issues</span></span>

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-10-installation-issues"></a><span data-ttu-id="53550-131">Problemy z instalacją programu WebMatrix 1,0</span><span class="sxs-lookup"><span data-stu-id="53550-131">WebMatrix 1.0 Installation Issues</span></span>

#### <a name="issue-webmatrix-10-is-available-only-on-platforms-that-support-microsoft-net-framework-4"></a><span data-ttu-id="53550-132">Problem: WebMatrix 1,0 jest dostępna tylko na platformach, które obsługują Microsoft .NET Framework 4</span><span class="sxs-lookup"><span data-stu-id="53550-132">Issue: WebMatrix 1.0 is available only on platforms that support Microsoft .NET Framework 4</span></span>

> <span data-ttu-id="53550-133">W programie WebMatrix jest wymagany .NET Framework wersja 4.</span><span class="sxs-lookup"><span data-stu-id="53550-133">The .NET Framework version 4 is required for WebMatrix.</span></span> <span data-ttu-id="53550-134">W niektórych przypadkach Instalator WebMatrix 1,0 pozwoli spróbować zainstalować program na platformie, która nie jest częścią obsługiwanego zestawu konfiguracyjnego.</span><span class="sxs-lookup"><span data-stu-id="53550-134">In certain cases, the WebMatrix 1.0 installer will let you try to install on a platform that is not part of the supported configuration set.</span></span> <span data-ttu-id="53550-135">W szczególności system Windows Vista bez aktualizacji programu SP1 umożliwi rozpoczęcie instalacji programu WebMatrix, ale składnik .NET Framework 4 zakończy się niepowodzeniem i zablokuje instalację.</span><span class="sxs-lookup"><span data-stu-id="53550-135">In particular, Windows Vista without the SP1 update will let you begin the installation of WebMatrix, but the .NET Framework 4 component will fail and block your installation.</span></span>
> 
> <span data-ttu-id="53550-136">**Obejście**</span><span class="sxs-lookup"><span data-stu-id="53550-136">**Workaround**</span></span>  
> <span data-ttu-id="53550-137">Zainstaluj program na obsługiwanej platformie, w tym:</span><span class="sxs-lookup"><span data-stu-id="53550-137">Install on a supported platform, which includes:</span></span>
> 
> - <span data-ttu-id="53550-138">Windows 7</span><span class="sxs-lookup"><span data-stu-id="53550-138">Windows 7</span></span>
> - <span data-ttu-id="53550-139">Windows Server 2008</span><span class="sxs-lookup"><span data-stu-id="53550-139">Windows Server 2008</span></span>
> - <span data-ttu-id="53550-140">Windows Server 2008 R2</span><span class="sxs-lookup"><span data-stu-id="53550-140">Windows Server 2008 R2</span></span>
> - <span data-ttu-id="53550-141">Windows Vista z dodatkiem SP1 lub nowszym</span><span class="sxs-lookup"><span data-stu-id="53550-141">Windows Vista SP1 or later</span></span>
> - <span data-ttu-id="53550-142">Windows XP z dodatkiem SP3</span><span class="sxs-lookup"><span data-stu-id="53550-142">Windows XP SP3</span></span>
> - <span data-ttu-id="53550-143">Windows Server 2003 SP2</span><span class="sxs-lookup"><span data-stu-id="53550-143">Windows Server 2003 SP2</span></span>

#### <a name="issue-cannot-install-webmatrix-10-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a><span data-ttu-id="53550-144">Problem: nie można zainstalować programu WebMatrix 1,0, jeśli Microsoft Visual Studio 2008 jest zainstalowana bez Microsoft Visual Studio 2008 z dodatkiem SP1</span><span class="sxs-lookup"><span data-stu-id="53550-144">Issue: Cannot install WebMatrix 1.0 if Microsoft Visual Studio 2008 is installed without Microsoft Visual Studio 2008 SP1</span></span>

> <span data-ttu-id="53550-145">**Obejście**</span><span class="sxs-lookup"><span data-stu-id="53550-145">**Workaround**</span></span>  
> <span data-ttu-id="53550-146">Zainstaluj [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) z centrum pobierania Microsoft.</span><span class="sxs-lookup"><span data-stu-id="53550-146">Install [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) from the Microsoft Download Center.</span></span>

#### <a name="issue-some-assemblies-for-sql-server-compact-40-are-not-installed-in-the-gac"></a><span data-ttu-id="53550-147">Problem: Niektóre zestawy dla SQL Server Compact 4,0 nie są zainstalowane w pamięci podręcznej GAC</span><span class="sxs-lookup"><span data-stu-id="53550-147">Issue: Some assemblies for SQL Server Compact 4.0 are not installed in the GAC</span></span>

> <span data-ttu-id="53550-148">Zestawy zarządzane dla SQL Server Compact 4,0 nie są umieszczane w globalnej pamięci podręcznej zestawów (GAC) podczas instalacji SQL Server Compact 4,0 na komputerze 64-bitowym, a komputer ma tylko .NET Framework zainstalowany profil klienta programu 3,5 z dodatkiem SP1.</span><span class="sxs-lookup"><span data-stu-id="53550-148">The managed assemblies for SQL Server Compact 4.0 are not placed in the global assembly cache (GAC) when you install SQL Server Compact 4.0 on a 64-bit computer and the computer has only the .NET Framework 3.5 SP1 Client Profile installed.</span></span> <span data-ttu-id="53550-149">Zarządzane zestawy, które nie są zainstalowane w pamięci podręcznej GAC, to:</span><span class="sxs-lookup"><span data-stu-id="53550-149">The managed assemblies that are not installed in the GAC are:</span></span>
> 
> - <span data-ttu-id="53550-150">*System. Data. SqlServerCe. dll* (dostawca ADO.NET)</span><span class="sxs-lookup"><span data-stu-id="53550-150">*System.Data.SqlServerCe.dll* (ADO.NET provider)</span></span>
> - <span data-ttu-id="53550-151">*System. Data. SqlServerCe. Entity. dll* (ADO.NET Entity Framework)</span><span class="sxs-lookup"><span data-stu-id="53550-151">*System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework )</span></span>
> 
> <span data-ttu-id="53550-152">**Obejście**</span><span class="sxs-lookup"><span data-stu-id="53550-152">**Workaround**</span></span>  
> <span data-ttu-id="53550-153">Odinstaluj SQL Server Compact 4,0.</span><span class="sxs-lookup"><span data-stu-id="53550-153">Uninstall SQL Server Compact 4.0.</span></span> <span data-ttu-id="53550-154">Pobierz i zainstaluj pełną wersję .NET Framework 3,5 SP1 z następującej lokalizacji:</span><span class="sxs-lookup"><span data-stu-id="53550-154">Download and install the full version of .NET Framework 3.5 SP1 from the following location:</span></span>  
>   
> [<span data-ttu-id="53550-155">Microsoft .NET Framework 3,5 z dodatkiem Service Pack 1 (pełny pakiet)</span><span class="sxs-lookup"><span data-stu-id="53550-155">Microsoft .NET Framework 3.5 Service pack 1 (Full Package)</span></span>](https://go.microsoft.com/fwlink/?LinkId=194828)  
>   
> <span data-ttu-id="53550-156">Następnie ponownie zainstaluj SQL Server Compact 4,0.</span><span class="sxs-lookup"><span data-stu-id="53550-156">Then reinstall SQL Server Compact 4.0.</span></span>

#### <a name="issue-cannot-uninstall-sql-server-compact-using-the-command-line"></a><span data-ttu-id="53550-157">Problem: nie można odinstalować SQL Server Compact przy użyciu wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="53550-157">Issue: Cannot uninstall SQL Server Compact using the command line</span></span>

> <span data-ttu-id="53550-158">Dezinstalacja SQL Server Compact przy użyciu opcji wiersza polecenia nie działa w tej wersji.</span><span class="sxs-lookup"><span data-stu-id="53550-158">Uninstallation of SQL Server Compact using command-line options does not work in this release.</span></span>
> 
> <span data-ttu-id="53550-159">**Obejście**</span><span class="sxs-lookup"><span data-stu-id="53550-159">**Workaround**</span></span>  
> <span data-ttu-id="53550-160">Aby odinstalować Microsoft SQL Server Compact 4,0, użyj *apletu programy i funkcje* w panelu sterowania systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="53550-160">Use *Programs and Features* in the Windows Control Panel to uninstall Microsoft SQL Server Compact 4.0.</span></span>

<a id="Known_Issues_ASPNET"></a>

### <a name="aspnet-web-pages"></a><span data-ttu-id="53550-161">Strony sieci Web programu ASP.NET</span><span class="sxs-lookup"><span data-stu-id="53550-161">ASP.NET Web Pages</span></span>

<span data-ttu-id="53550-162">W tej sekcji dokumentu opisano nowe funkcje, zmiany i znane problemy w wersji 1,0 stron sieci Web ASP.NET składnia Razor z programem.</span><span class="sxs-lookup"><span data-stu-id="53550-162">This section of the document describes new features, changes, and known issues with the 1.0 release of ASP.NET Web Pages with Razor syntax.</span></span>

- [<span data-ttu-id="53550-163">Nowe funkcje</span><span class="sxs-lookup"><span data-stu-id="53550-163">New features</span></span>](#NewFeatures)
- [<span data-ttu-id="53550-164">Wprowadzane</span><span class="sxs-lookup"><span data-stu-id="53550-164">Changes</span></span>](#Changes)
- [<span data-ttu-id="53550-165">Problemy</span><span class="sxs-lookup"><span data-stu-id="53550-165">Issues</span></span>](#Issues)

#### <a id="NewFeatures"></a><span data-ttu-id="53550-166">Nowe funkcje</span><span class="sxs-lookup"><span data-stu-id="53550-166">New Features</span></span>

#### <a name="new-configuration-setting-added-to-disable-the-package-manager"></a><span data-ttu-id="53550-167">Nowość: dodano ustawienie konfiguracji w celu wyłączenia Menedżera pakietów</span><span class="sxs-lookup"><span data-stu-id="53550-167">New: Configuration setting added to disable the package manager</span></span>

> <span data-ttu-id="53550-168">Nowy klucz `asp:AdminManagerEnabled` jest dostępny dla elementu `<appSettings>` w pliku *Web. config* , który umożliwia całkowite wyłączenie Menedżera pakietów.</span><span class="sxs-lookup"><span data-stu-id="53550-168">A new `asp:AdminManagerEnabled` key is available for the `<appSettings>` element in the *web.config* file, which lets you completely disable the package manager.</span></span> <span data-ttu-id="53550-169">Wartość domyślna dla tego elementu to true, co oznacza, że jeśli nie jest uwzględniona w pliku *Web. config* , Menedżer pakietów jest włączony.</span><span class="sxs-lookup"><span data-stu-id="53550-169">The default value for this element is true, meaning that if it is not included in the *web.config* file, the package manager is enabled.</span></span> <span data-ttu-id="53550-170">Aby wyłączyć menedżera pakietów, Dodaj następujący element do pliku *Web. config* w katalogu głównym witryny sieci Web:</span><span class="sxs-lookup"><span data-stu-id="53550-170">To disable the package manager, add the following element to the *web.config* file in the root of the website:</span></span>
> 
> [!code-xml[Main](overview/samples/sample1.xml)]

#### <a id="Changes"></a><span data-ttu-id="53550-171">Wprowadzane</span><span class="sxs-lookup"><span data-stu-id="53550-171">Changes</span></span>

#### <a name="change-webpagesadminfoldervirtualpath-key-renamed-to-aspadminfoldervirtualpath"></a><span data-ttu-id="53550-172">Zmiana: zmieniono nazwę klucza "Webpages: AdminFolderVirtualPath" na "ASP: AdminFolderVirtualPath"</span><span class="sxs-lookup"><span data-stu-id="53550-172">Change: "webPages:AdminFolderVirtualPath" key renamed to "asp:AdminFolderVirtualPath"</span></span>

> <span data-ttu-id="53550-173">Klucz `webPages:AdminFolderVirtualPath`, który można dodać do pliku *Web. config* , aby określić lokalizację Menedżera pakietów została zmieniona na Użyj przestrzeni nazw `asp:` zamiast przestrzeni nazw `webPages`.</span><span class="sxs-lookup"><span data-stu-id="53550-173">The `webPages:AdminFolderVirtualPath` key that can be added to the *web.config* file to specify the location of the package manager has been renamed to use the `asp:` namespace instead of the `webPages` namespace.</span></span> <span data-ttu-id="53550-174">Jeśli użyto tego elementu, należy zmienić jego nazwę w pliku konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="53550-174">If you have used this element, you must rename it in the configuration file.</span></span>

#### <a id="Issues"></a> <span data-ttu-id="53550-175">Znane problemy</span><span class="sxs-lookup"><span data-stu-id="53550-175">Known Issues</span></span>

#### <a name="issue-passwords-for-membership-users-no-longer-recognized"></a><span data-ttu-id="53550-176">Problem: hasła dla użytkowników członkostwa nie są już rozpoznawane</span><span class="sxs-lookup"><span data-stu-id="53550-176">Issue: Passwords for membership users no longer recognized</span></span>

> <span data-ttu-id="53550-177">Algorytm tworzenia i przechowywania haseł związanych z członkostwem (login) został zmieniony na bardziej bezpieczny.</span><span class="sxs-lookup"><span data-stu-id="53550-177">The algorithm for creating and storing membership (login) passwords has been changed to be more secure.</span></span> <span data-ttu-id="53550-178">W związku z tym hasła przechowywane dla członków (użytkowników) utworzonych w wersjach beta ASP.NET Razor nie będą rozpoznawane.</span><span class="sxs-lookup"><span data-stu-id="53550-178">As a result, the passwords stored for members (users) created in Beta versions of ASP.NET Razor will not be recognized.</span></span> 
> 
> <span data-ttu-id="53550-179">**Obejście problemu** Jeśli lokacja nie została jeszcze umieszczona w środowisku produkcyjnym, usuń rekordy użytkowników z bazy danych członkostwa.</span><span class="sxs-lookup"><span data-stu-id="53550-179">**Workaround** If the site has not yet been put into production, remove the user records from the membership database.</span></span> <span data-ttu-id="53550-180">Jeśli baza danych jest na żywo, program programowo ponownie generuje istniejące hasła w bazie danych członkostwa.</span><span class="sxs-lookup"><span data-stu-id="53550-180">If database is live, programmatically regenerate existing passwords in the membership database.</span></span>

#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a><span data-ttu-id="53550-181">Problem: nieoczekiwane zachowanie podczas korzystania z niestandardowej tabeli użytkownika na potrzeby członkostwa</span><span class="sxs-lookup"><span data-stu-id="53550-181">Issue: Unexpected behavior when using a custom user table for membership</span></span>

> <span data-ttu-id="53550-182">Aby zainicjować dostawcę członkostwa dla witryny sieci Web ASP.NET Razor, należy wywołać metodę `WebSecurity.InitializeDatabaseConnection`.</span><span class="sxs-lookup"><span data-stu-id="53550-182">To initialize the membership provider for an ASP.NET Razor website, you call the `WebSecurity.InitializeDatabaseConnection` method.</span></span> <span data-ttu-id="53550-183">(W programie WebMatrix szablon witryny Starter zawiera wywołanie tej metody w pliku *\_AppStart. cshtml* ). Jeśli parametr `autoCreateTables` tej metody ma wartość true (domyślnie jest ustawiona wartość true w szablonie witryny Starter), a jeśli Nierozpoznana nazwa tabeli jest przenoszona do metody (drugi parametr), metoda nie zgłasza błędu.</span><span class="sxs-lookup"><span data-stu-id="53550-183">(In WebMatrix, the Starter Site template includes a call to this method in the *\_AppStart.cshtml* file.) If the `autoCreateTables` parameter of this method is set to true (by default, it is set to true in the Starter Site template), and if an unrecognized table name is passed to the method (the second parameter), the method does not throw an error.</span></span> <span data-ttu-id="53550-184">Zamiast tego automatycznie tworzy tabelę.</span><span class="sxs-lookup"><span data-stu-id="53550-184">Instead, it automatically creates the table.</span></span>
> 
> <span data-ttu-id="53550-185">Może to być problem, jeśli zamierzasz użyć niestandardowej tabeli użytkownika do członkostwa, ale Przekaż nieprawidłową nazwę tabeli do metody `WebSecurity.InitializeDatabaseConnection`.</span><span class="sxs-lookup"><span data-stu-id="53550-185">This can be a problem if you intend to use a custom user table for membership but pass the wrong table name to the `WebSecurity.InitializeDatabaseConnection` method.</span></span> <span data-ttu-id="53550-186">Ponieważ metoda nie jest domyślnie zgłaszana błędem, jeśli określona tabela nie istnieje i ponieważ zamiast niej tworzy nową tabelę, aplikacja może wydawać się niedziałająca.</span><span class="sxs-lookup"><span data-stu-id="53550-186">Because the method does not by default raise an error if the table you specify does not exist, and because it instead creates a new table, the application can appear to be working.</span></span> <span data-ttu-id="53550-187">Jednak kod aplikacji, który zależy od niestandardowej tabeli użytkownika (i znajdujących się w nim pól) może zakończyć się niepowodzeniem z nieoczekiwanymi błędami.</span><span class="sxs-lookup"><span data-stu-id="53550-187">However, application code that relies on your custom user table (and on fields in it) can eventually fail with unexpected errors.</span></span>
> 
> <span data-ttu-id="53550-188">**Obejście**</span><span class="sxs-lookup"><span data-stu-id="53550-188">**Workaround**</span></span>  
> <span data-ttu-id="53550-189">Upewnij się, że nazwa przenoszona przez metodę `InitializeDatabaseConnection` jest zgodna z tabelą profilu użytkownika w bazie danych członkostwa, lub upewnij się, że parametr `autoCreateTables` ma wartość false.</span><span class="sxs-lookup"><span data-stu-id="53550-189">Make sure that the name passed in the `InitializeDatabaseConnection` method matches the user profile table in the membership database, or make sure that the `autoCreateTables` parameter is set to false.</span></span>

#### <a name="issue-error-message-the-admin-module-requires-access-to-app_data"></a><span data-ttu-id="53550-190">Problem: komunikat o błędzie "moduł administracyjny wymaga dostępu do ~/App\_danych"</span><span class="sxs-lookup"><span data-stu-id="53550-190">Issue: Error message "The Admin Module requires access to ~/App\_Data"</span></span>

> <span data-ttu-id="53550-191">W pewnych okolicznościach próba utworzenia użytkowników lub pracy z systemem członkostwa ASP.NET może spowodować, że strona wyświetli błąd *moduł administracyjny wymaga dostępu do ~/App\_danych*.</span><span class="sxs-lookup"><span data-stu-id="53550-191">Under some circumstances, trying to create users or otherwise work with the ASP.NET membership system can cause the page to display the error *The Admin Module requires access to ~/App\_Data*.</span></span> <span data-ttu-id="53550-192">Dzieje się tak, jeśli konto, na którym są uruchomione usługi IIS lub IIS Express, nie ma uprawnień do tworzenia i zapisywania do folderu *danych\_aplikacji* w katalogu głównym witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="53550-192">This occurs if the account that IIS or IIS Express is running under does not have permissions to create and write to the *App\_Data* folder under the website root.</span></span> 
> 
> <span data-ttu-id="53550-193">**Obejście problemu** Utwórz ręcznie folder *danych\_aplikacji* dla witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="53550-193">**Workaround** Manually create an *App\_Data* folder for the website.</span></span> <span data-ttu-id="53550-194">Następnie upewnij się, że konto systemu Windows, w którym działa aplikacja (zazwyczaj usługa sieciowa) ma uprawnienia do odczytu i zapisu dla folderów głównych aplikacji oraz dla podfolderów, takich jak dane\_aplikacji.</span><span class="sxs-lookup"><span data-stu-id="53550-194">Then make sure that the Windows account that the application runs under (typically NETWORK SERVICE) has read/write permissions for root folders of the application and for subfolders such as App\_Data.</span></span> <span data-ttu-id="53550-195">Bardziej szczegółowe informacje są dostępne w artykule bazy wiedzy [problemy związane z SQL Server Express wystąpieniami użytkowników i projektami aplikacji sieci Web ASP.NET](https://support.microsoft.com/kb/2002980).</span><span class="sxs-lookup"><span data-stu-id="53550-195">More detailed information is available in the KnowledgeBase article [Problems with SQL Server Express user instancing and ASP.net Web Application Projects](https://support.microsoft.com/kb/2002980).</span></span>

#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a><span data-ttu-id="53550-196">Problem: "nie można wygenerować wystąpienia użytkownika SQL Server"</span><span class="sxs-lookup"><span data-stu-id="53550-196">Issue: "Failed to generate a user instance of SQL Server" error</span></span>

> <span data-ttu-id="53550-197">Jeśli aplikacja sieci Web WebMatrix używa SQL Server Express i korzysta z usług IIS 7,5 w systemie Windows 7 lub Windows Server 2008 R2, może zostać wyświetlony komunikat o błędzie z informacją, że SQL Server nie może pobrać lokalnej ścieżki aplikacji użytkownika w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="53550-197">If a WebMatrix Web application uses SQL Server Express and is running IIS 7.5 on Windows 7 or Windows Server 2008 R2, you might see an error that indicates that SQL Server cannot retrieve the user's local application path at run time.</span></span>
> 
> <span data-ttu-id="53550-198">**Obejście problemu** Upewnij się, że konto systemu Windows, w którym jest uruchomiona aplikacja (zazwyczaj usługa sieciowa) ma uprawnienia do odczytu i zapisu dla folderów głównych aplikacji oraz dla podfolderów, takich jak *aplikacja\_dane*.</span><span class="sxs-lookup"><span data-stu-id="53550-198">**Workaround** Make sure that the Windows account that the application runs under (typically NETWORK SERVICE) has read/write permissions for root folders of the application and for subfolders such as *App\_Data*.</span></span> <span data-ttu-id="53550-199">Bardziej szczegółowe informacje są dostępne w artykule bazy wiedzy [problemy związane z SQL Server Express wystąpieniami użytkowników i projektami aplikacji sieci Web ASP.NET](https://support.microsoft.com/kb/2002980).</span><span class="sxs-lookup"><span data-stu-id="53550-199">More detailed information is available in the KnowledgeBase article [Problems with SQL Server Express user instancing and ASP.net Web Application Projects](https://support.microsoft.com/kb/2002980).</span></span>

#### <a name="issue-files-that-contains-package-manager-resources-or-package-manager-passwords-are-servable-under-iis-60-and-earlier"></a><span data-ttu-id="53550-200">Problem: pliki, które zawierają zasoby Menedżera pakietów lub hasła Menedżera pakietów, są objęte usługami IIS 6,0 i wcześniejszymi</span><span class="sxs-lookup"><span data-stu-id="53550-200">Issue: Files that contains package-manager resources or package-manager passwords are servable under IIS 6.0 and earlier</span></span>

> <span data-ttu-id="53550-201">W przypadku wdrażania aplikacji ASP.NET Web Pages (Razor), która została skompilowana przy użyciu wersji RC2, a aplikacja zawiera plik *Password. txt* lub *packagesources. txt* w obszarze */App\_dane/administrator*, usługi IIS 6,0 będą obsłużyć plik, jeśli jest to wymagane, co może spowodować udostępnienie haseł dla wystąpienia Menedżera pakietów.</span><span class="sxs-lookup"><span data-stu-id="53550-201">If you deploy an ASP.NET Web Pages (Razor) application that was built using the RC2 release, and if the application contains a *password.txt* or *packagesources.txt* file under */App\_Data/admin*, IIS 6.0 will serve the file if requested, potentially exposing the passwords for your package manager instance.</span></span> 
> 
> <span data-ttu-id="53550-202">**Obejście problemu** Zmień nazwę pliku *Password. txt* lub *packagesources. txt* na *Password. config* lub *packagesources. config*. Domyślnie usługi IIS 6,0 nie obsługują plików z rozszerzeniem *. config* .</span><span class="sxs-lookup"><span data-stu-id="53550-202">**Workaround** Rename the *password.txt* or *packagesources.txt* file to *password.config* or *packagesources.config*. By default, IIS 6.0 does not serve files that have the *.config* extension.</span></span> <span data-ttu-id="53550-203">(W usługach IIS 7 nie są obsługiwane żadne pliki w *\_aplikacji folder danych* , więc nie trzeba zmieniać nazw plików).</span><span class="sxs-lookup"><span data-stu-id="53550-203">(In IIS 7, no files in the *App\_Data* folder are served, so you do not need to rename the files.)</span></span>

#### <a name="issue-uninstalling-packages-installed-using-the-beta-3-release-does-not-completely-remove-package-components"></a><span data-ttu-id="53550-204">Problem: Odinstalowywanie pakietów zainstalowanych przy użyciu wersji beta 3 nie powoduje całkowitego usunięcia składników pakietu</span><span class="sxs-lookup"><span data-stu-id="53550-204">Issue: Uninstalling packages installed using the Beta 3 release does not completely remove package components</span></span>

> <span data-ttu-id="53550-205">Jeśli zainstalowano pakiet za pomocą Menedżera pakietów w wersji beta 3, a następnie podjęto próbę odinstalowania go przy użyciu bieżącej wersji, pakiet nie zostanie całkowicie odinstalowany.</span><span class="sxs-lookup"><span data-stu-id="53550-205">If you installed a package using the package manager in the Beta 3 release and then try to uninstall it using the current release, the package is not completely uninstalled.</span></span> <span data-ttu-id="53550-206">Użycie przycisku **Odinstaluj** Menedżera pakietów powoduje usunięcie niektórych składników, ale pozostawia kod biblioteki pakietu i nie aktualizuje pliku *Package. config* .</span><span class="sxs-lookup"><span data-stu-id="53550-206">Using the package manager's **Uninstall** button removes some components, but leaves the package's library code and does not update the *package.config* file.</span></span>
> 
> <span data-ttu-id="53550-207">**Obejście** </span><span class="sxs-lookup"><span data-stu-id="53550-207">**Workaround** </span></span>  
> <span data-ttu-id="53550-208">Wykonaj następujące kroki:</span><span class="sxs-lookup"><span data-stu-id="53550-208">Perform these steps:</span></span>  
> 1. <span data-ttu-id="53550-209">Usuń folder *\_Data\packages aplikacji* .</span><span class="sxs-lookup"><span data-stu-id="53550-209">Delete the *App\_Data\packages* folder.</span></span> <span data-ttu-id="53550-210">Spowoduje to usunięcie wszystkich pakietów.</span><span class="sxs-lookup"><span data-stu-id="53550-210">This removes all packages.</span></span>   
> 2. <span data-ttu-id="53550-211">Usuń plik *Packages. config* w folderze głównym witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="53550-211">Delete the *packages.config* file in the root of the website.</span></span>

#### <a name="issue-in-visual-studio-invoking-the-web-based-package-manager-takes-the-application-offline"></a><span data-ttu-id="53550-212">Problem: w programie Visual Studio wywoływanie Menedżera pakietów opartych na sieci Web powoduje, że aplikacja jest w trybie offline</span><span class="sxs-lookup"><span data-stu-id="53550-212">Issue: In Visual Studio, invoking the web-based package manager takes the application offline</span></span>

> <span data-ttu-id="53550-213">Jeśli pracujesz w programie Visual Studio (nie WebMatrix) i używasz funkcji *administratora\_* , aby uruchomić Menedżera pakietów, program Visual Studio przełączy aplikację w tryb offline i opublikuje *aplikację\_w trybie offline. htm* w katalogu głównym witryny sieci Web, co zakłóca możliwość korzystania z Menedżera pakietów.</span><span class="sxs-lookup"><span data-stu-id="53550-213">If you are working in Visual Studio (not WebMatrix) and use the *\_admin* functionality to start the package manager, Visual Studio takes the application offline and posts the *app\_offline.htm* into the website root, which disrupts your ability to use the package manager.</span></span>
> 
> [!NOTE]
> <span data-ttu-id="53550-214">Mimo że zwykle widzisz to zachowanie w przypadku korzystania z interfejsu Menedżera pakietów opartych na sieci Web, takie samo zachowanie występuje w przypadku dodawania, usuwania lub modyfikowania plików w folderze *dane\_aplikacji* .</span><span class="sxs-lookup"><span data-stu-id="53550-214">Although you would most typically see this behavior when using the web-based package manager interface, the same behavior occurs if you add, remove, or modify any files in the *App\_Data* folder.</span></span>
> 
> <span data-ttu-id="53550-215">**Obejście** </span><span class="sxs-lookup"><span data-stu-id="53550-215">**Workaround** </span></span>  
> <span data-ttu-id="53550-216">Aby korzystać z pakietów w programie Visual Studio, użyj rozszerzenia NuGet zamiast Menedżera pakietów opartych na sieci Web.</span><span class="sxs-lookup"><span data-stu-id="53550-216">To work with packages in Visual Studio, use the NuGet extension instead of the web-based package manager.</span></span> <span data-ttu-id="53550-217">Aby uzyskać więcej informacji, zapoznaj się z [dokumentacją programu NuGet](https://docs.microsoft.com/nuget/).</span><span class="sxs-lookup"><span data-stu-id="53550-217">For information, see the [NuGet documentation](https://docs.microsoft.com/nuget/).</span></span> <span data-ttu-id="53550-218">Jeśli pracujesz z innymi plikami w folderze *danych\_aplikacji* , rozważ pozostawienie plików w innym miejscu, aby uniknąć tego problemu.</span><span class="sxs-lookup"><span data-stu-id="53550-218">If you are working with other files in the *App\_Data* folder, consider keeping the files elsewhere to avoid this issue.</span></span> <span data-ttu-id="53550-219">Jeśli nie jest to praktyczne, Usuń *aplikację\_pliku offline. htm* ręcznie lub zaczekaj, aż lokacja przewróci do trybu online automatycznie (domyślnie po 30 sekundach).</span><span class="sxs-lookup"><span data-stu-id="53550-219">If that's not practical, delete the *app\_offline.htm* file manually or wait until the site comes back online automatically (by default, after 30 seconds).</span></span>

#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a><span data-ttu-id="53550-220">Problem: program Visual Studio IntelliSense i szablony projektów są dostępne tylko w ASP.NET MVC w wersji 3</span><span class="sxs-lookup"><span data-stu-id="53550-220">Issue: Visual Studio IntelliSense and project templates available only in ASP.NET MVC version 3</span></span>

> <span data-ttu-id="53550-221">Instalowanie stron sieci Web ASP.NET nie instaluje również narzędzi dla programu Visual Studio, takich jak IntelliSense i szablony projektów dla aplikacji ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="53550-221">Installing ASP.NET Web Pages does not also install tools for Visual Studio such as IntelliSense and project templates for ASP.NET Web Pages applications.</span></span>
> 
> <span data-ttu-id="53550-222">**Obejście problemu** Aby użyć funkcji IntelliSense i szablonów projektu dla aplikacji ASP.NET Web Pages w programie Visual Studio, zainstaluj ASP.NET MVC 3 RC przez Instalatora platformy sieci Web lub [autonomicznego Instalatora](https://go.microsoft.com/fwlink/?LinkID=191797).</span><span class="sxs-lookup"><span data-stu-id="53550-222">**Workaround** To use IntelliSense and project templates for ASP.NET Web Pages applications in Visual Studio, install ASP.NET MVC 3 RC either through the Web Platform Installer or the [stand-alone installer](https://go.microsoft.com/fwlink/?LinkID=191797).</span></span>

#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a><span data-ttu-id="53550-223">Problem: odczytywanie kanałów informacyjnych lub innych danych zewnętrznych za pośrednictwem serwera proxy</span><span class="sxs-lookup"><span data-stu-id="53550-223">Issue: Reading feeds or other external data via a proxy server</span></span>

> <span data-ttu-id="53550-224">Jeśli serwer z uruchomioną lokacją znajduje się za serwerem proxy, może być konieczne skonfigurowanie informacji o serwerze proxy w pliku *Web. config* , aby można było odczytywać informacje pochodzące spoza witryny.</span><span class="sxs-lookup"><span data-stu-id="53550-224">If the server running the site is behind a proxy server, you might need to configure proxy information in the *web.config* file in order to be able to read information that comes from outside your site.</span></span> <span data-ttu-id="53550-225">Jeśli na przykład użyjesz pomocnika `ReCaptcha`, pomocnik komunikuje się z usługą reCAPTCHA, ale może być blokowany przez serwer proxy.</span><span class="sxs-lookup"><span data-stu-id="53550-225">For example, if you use the `ReCaptcha` helper, the helper communicates with the reCAPTCHA service, but might be blocked by your proxy server.</span></span> <span data-ttu-id="53550-226">Podobnie, kanały informacyjne, które są używane na stronach sieci Web ASP.NET, takie jak źródło danych używane przez Menedżera pakietów, mogą wymagać konfiguracji serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="53550-226">Similarly, feeds that are used in ASP.NET Web Pages, such as the feed used by the package manager, might require proxy configuration.</span></span>
> 
> <span data-ttu-id="53550-227">Jeśli wystąpią problemy podczas pracy z usługą zewnętrzną lub praca z kanałem informacyjnym pakietu, umieść następujące elementy w głównym pliku *Web. config* aplikacji:</span><span class="sxs-lookup"><span data-stu-id="53550-227">If you experience problems in working with an external service or working with the package feed, put the following elements into your application's root *web.config* file:</span></span>
> 
> [!code-xml[Main](overview/samples/sample2.xml)]
> 
> <span data-ttu-id="53550-228">Aby uzyskać więcej informacji o konfigurowaniu serwera proxy, zobacz [&lt;proxy&gt; element (Ustawienia sieci)](https://msdn.microsoft.com/library/sa91de1e.aspx) w witrynie MSDN w sieci Web.</span><span class="sxs-lookup"><span data-stu-id="53550-228">For more information about configuring a proxy server, see [&lt;proxy&gt; Element (Network Settings)](https://msdn.microsoft.com/library/sa91de1e.aspx) on the MSDN Web site.</span></span>

#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="53550-229">Problem: Odinstalowywanie .NET Framework w wersji 4 wyłącza ASP.NET stron sieci Web ze składnią Razor</span><span class="sxs-lookup"><span data-stu-id="53550-229">Issue: Uninstalling the .NET Framework version 4 disables ASP.NET Web Pages with Razor Syntax</span></span>

> <span data-ttu-id="53550-230">Jeśli odinstalujesz .NET Framework w wersji 4, a następnie zainstalujesz ją ponownie, ASP.NET strony sieci Web z składnia Razor jest wyłączone.</span><span class="sxs-lookup"><span data-stu-id="53550-230">If you uninstall the .NET Framework version 4 and then reinstall it, ASP.NET Web Pages with Razor syntax is disabled.</span></span> <span data-ttu-id="53550-231">Strony z rozszerzeniem *. cshtml* nie działają poprawnie.</span><span class="sxs-lookup"><span data-stu-id="53550-231">Pages with the *.cshtml* extension do not run correctly.</span></span> <span data-ttu-id="53550-232">Strony sieci Web ASP.NET rejestrują zestaw w głównym pliku *Web. config* maszyny, a usunięcie .NET Framework usuwa ten plik.</span><span class="sxs-lookup"><span data-stu-id="53550-232">ASP.NET Web Pages registers an assembly in the machine root *web.config* file, and removing the .NET Framework removes that file.</span></span> <span data-ttu-id="53550-233">Ponowne zainstalowanie .NET Framework instaluje nową wersję pliku konfiguracji, ale nie dodaje odwołania do zestawu stron sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="53550-233">Reinstalling the .NET Framework installs a new version of the configuration file, but does not add the reference for the ASP.NET Web Pages assembly.</span></span>
> 
> <span data-ttu-id="53550-234">**Obejście problemu** Po ponownym zainstalowaniu .NET Framework ponownie zainstaluj strony sieci Web ASP.NET z składnia Razor.</span><span class="sxs-lookup"><span data-stu-id="53550-234">**Workaround** After reinstalling the .NET Framework, reinstall ASP.NET Web Pages with Razor syntax.</span></span> <span data-ttu-id="53550-235">Spowoduje to dodanie następującego elementu do pliku *Web. config* w katalogu głównym maszyny, który zwykle znajduje się w następującej lokalizacji:</span><span class="sxs-lookup"><span data-stu-id="53550-235">This adds the following element to the *web.config* file in the machine root, which is typically in the following location:</span></span>  
> 
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](overview/samples/sample3.xml)]

#### <a name="issue-extensionless-urls-do-not-find-cshtmlvbhtml-files-on-iis-7-or-iis-75"></a><span data-ttu-id="53550-236">Problem: w przypadku adresów URL bez rozszerzenia nie znajdują się pliki cshtml/. vbhtml w usługach IIS 7 lub IIS 7,5</span><span class="sxs-lookup"><span data-stu-id="53550-236">Issue: Extensionless URLs do not find .cshtml/.vbhtml files on IIS 7 or IIS 7.5</span></span>

> <span data-ttu-id="53550-237">W przypadku usług IIS 7 lub IIS 7,5 żądania z adresem URL podobnym do następującego nie mogą znaleźć stron, które mają rozszerzenie *. cshtml* lub *. vbhtml* :</span><span class="sxs-lookup"><span data-stu-id="53550-237">On IIS 7 or IIS 7.5, requests with a URL like the following are not able to find pages that have the *.cshtml* or *.vbhtml* extension:</span></span>  
> 
> `http://www.example.com/ExampleSite/ExampleFile`  
> 
> <span data-ttu-id="53550-238">Problem występuje, ponieważ ponowne zapisywanie adresów URL nie jest domyślnie włączone dla usług IIS 7 lub IIS 7,5.</span><span class="sxs-lookup"><span data-stu-id="53550-238">The issue arises because URL rewriting is not enabled by default for IIS 7 or IIS 7.5.</span></span> <span data-ttu-id="53550-239">Likeliest scenariusz polega na tym, że problem nie jest wyświetlany podczas testowania lokalnego przy użyciu IIS Express, ale podczas wdrażania witryny sieci Web w witrynie sieci Web hostowanie jest to możliwe.</span><span class="sxs-lookup"><span data-stu-id="53550-239">The likeliest scenario is that you do not see the problem when testing locally using IIS Express, but you experience it when you deploy your website to a hosting website.</span></span>
> 
> <span data-ttu-id="53550-240">**Obejście**</span><span class="sxs-lookup"><span data-stu-id="53550-240">**Workaround**</span></span>
> 
> - <span data-ttu-id="53550-241">Jeśli masz kontrolę nad komputerem serwera, na komputerze serwera Zainstaluj aktualizację, która jest opisana w [aktualizacji, która umożliwia obsługę określonych usług iis 7,0 lub iis 7,5 obsługi żądań, których adresy URL nie kończą się kropką](https://support.microsoft.com/kb/980368).</span><span class="sxs-lookup"><span data-stu-id="53550-241">If you have control over the server computer, on the server computer install the update that is described in [A update is available that enables certain IIS 7.0 or IIS 7.5 handlers to handle requests whose URLs do not end with a period](https://support.microsoft.com/kb/980368).</span></span>
> - <span data-ttu-id="53550-242">Jeśli nie masz kontroli nad komputerem serwera (na przykład wdrażasz je w witrynie sieci Web hostingu), Dodaj następujący kod do pliku *Web. config* witryny sieci Web:</span><span class="sxs-lookup"><span data-stu-id="53550-242">If you do not have control over the server computer (for example, you are deploying to a hosting website), add the following to the website's *web.config* file:</span></span> 
> 
>     [!code-xml[Main](overview/samples/sample4.xml)]

#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a><span data-ttu-id="53550-243">Problem: wdrażanie aplikacji na komputerze, na którym nie zainstalowano SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="53550-243">Issue: Deploying an application to a computer that does not have SQL Server Compact installed</span></span>

> <span data-ttu-id="53550-244">Aplikacje zawierające SQL Server Compact bazy danych można uruchamiać na komputerze, na którym SQL Server Compact nie jest zainstalowana.</span><span class="sxs-lookup"><span data-stu-id="53550-244">Applications that include SQL Server Compact databases can run on a computer where SQL Server Compact is not installed.</span></span> <span data-ttu-id="53550-245">Firma Microsoft WebMatrix 1,0 automatycznie kopiuje te pliki binarne i wykonuje transformacje pliku *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="53550-245">Microsoft WebMatrix 1.0 automatically copies these binaries for you and performs the appropriate *web.config* file transforms.</span></span>
> 
> <span data-ttu-id="53550-246">**Obejście problemu** Jeśli musisz skopiować te pliki i ręcznie wprowadzić zmiany w pliku *Web. config* , wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="53550-246">**Workaround** If you need to copy these files and make the *web.config* file changes manually, do the following:</span></span>
> 
> 1. <span data-ttu-id="53550-247">Skopiuj zestawy aparatów bazy danych do folderu *bin* (i podfolderów) aplikacji na komputerze docelowym:</span><span class="sxs-lookup"><span data-stu-id="53550-247">Copy the database engine assemblies to the *Bin* folder (and subfolders) of the application on the target computer:</span></span>  
> 
>    - <span data-ttu-id="53550-248">Kopiuj *C:\Program Files\Microsoft SQL Server Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* </span><span class="sxs-lookup"><span data-stu-id="53550-248">Copy *C:\Program Files\Microsoft SQL Server Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* </span></span>  
>      <span data-ttu-id="53550-249">**do** *\Bin*</span><span class="sxs-lookup"><span data-stu-id="53550-249">**to** *\Bin*</span></span>
>    - <span data-ttu-id="53550-250">Skopiuj *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\* **do** *\Bin\x86*</span><span class="sxs-lookup"><span data-stu-id="53550-250">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\* **to** *\Bin\x86*</span></span>
>    - <span data-ttu-id="53550-251">Skopiuj *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\* \* **do** *\Bin\amd64*</span><span class="sxs-lookup"><span data-stu-id="53550-251">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\*\* **to** *\Bin\amd64*</span></span>
> 
> 2. <span data-ttu-id="53550-252">W folderze głównym witryny sieci Web Utwórz lub Otwórz plik *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="53550-252">In the root folder of the website, create or open a *web.config* file.</span></span> <span data-ttu-id="53550-253">(W programie WebMatrix 1,0 ten typ pliku jest dostępny po kliknięciu przycisku **wszystkie** w oknie dialogowym **Wybierz typ pliku** ).</span><span class="sxs-lookup"><span data-stu-id="53550-253">(In WebMatrix 1.0, this file type is available if you click **All** in the **Choose a File Type** dialog box.)</span></span>
> 3. <span data-ttu-id="53550-254">Dodaj następujący element jako element podrzędny elementu `<configuration>` (nie wewnątrz elementu `<system.web>`):</span><span class="sxs-lookup"><span data-stu-id="53550-254">Add the following element as a child of the `<configuration>` element (not inside the `<system.web>` element):</span></span>
> 
>     [!code-xml[Main](overview/samples/sample5.xml)]

#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a><span data-ttu-id="53550-255">Problem: pomocniki "Database" i "WebGrid" nie działają w średnim zaufaniu w Visual Basic</span><span class="sxs-lookup"><span data-stu-id="53550-255">Issue: "Database" and "WebGrid" helpers do not work in Medium Trust in Visual Basic</span></span>

> <span data-ttu-id="53550-256">Jeśli używasz Visual Basic (Tworzenie plików *. vbhtml* ), pomocniki `Database` i `WebGrid` nie będą działały, jeśli aplikacja jest ustawiona do używania średniego zaufania.</span><span class="sxs-lookup"><span data-stu-id="53550-256">If you are using Visual Basic (creating *.vbhtml* files), the `Database` and `WebGrid` helpers will not work if the application is set to use Medium Trust.</span></span>
> 
> <span data-ttu-id="53550-257">**Obejście**</span><span class="sxs-lookup"><span data-stu-id="53550-257">**Workaround**</span></span>  
> <span data-ttu-id="53550-258">Jeśli używasz programu Visual Studio 2010, możesz rozwiązać ten problem, instalując dodatek Service Pack 1.</span><span class="sxs-lookup"><span data-stu-id="53550-258">If you use Visual Studio 2010, you can resolve this problem by installing the Service Pack 1 release.</span></span> <span data-ttu-id="53550-259">Do momentu udostępnienia ostatecznej wersji programu SP1 można pobrać wersję beta programu SP1 ze strony [Microsoft Visual Studio 2010 Service Pack 1 beta](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=11ea69cb-cf12-4842-a3d7-b32a1e5642e2&amp;displaylang=en) w centrum pobierania Microsoft.</span><span class="sxs-lookup"><span data-stu-id="53550-259">Until the final version of the SP1 release is available, you can download the Beta version of SP1 from the [Microsoft Visual Studio 2010 Service Pack 1 Beta](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=11ea69cb-cf12-4842-a3d7-b32a1e5642e2&amp;displaylang=en) page on the Microsoft Download Center.</span></span>   
>   
> <span data-ttu-id="53550-260">Jeśli nie jest to praktyczne, lub jeśli nie korzystasz z programu Visual Studio 2010, możesz tymczasowo ustawić aplikację tak, aby korzystała z pełnego zaufania.</span><span class="sxs-lookup"><span data-stu-id="53550-260">If this is not practical, or if you do not use Visual Studio 2010, you can temporarily set the application to use Full Trust.</span></span>

#### <a name="issue-applicationpart-resources-are-externally-accessible"></a><span data-ttu-id="53550-261">Problem: zasoby "ApplicationPart" są dostępne na zewnątrz</span><span class="sxs-lookup"><span data-stu-id="53550-261">Issue: "ApplicationPart" resources are externally accessible</span></span>

> <span data-ttu-id="53550-262">Jeśli zestaw zawiera obiekty, które pochodzą z klasy `ApplicationPart`, zasoby tego zestawu są uwidaczniane przez klasę `ResourceRouteHandler`.</span><span class="sxs-lookup"><span data-stu-id="53550-262">If an assembly contains objects that derives from the `ApplicationPart` class, that assembly's resources are exposed by the `ResourceRouteHandler` class.</span></span> <span data-ttu-id="53550-263">Rozważmy na przykład następujący adres URL:</span><span class="sxs-lookup"><span data-stu-id="53550-263">For example, consider the following URL:</span></span>  
>   
> `~/r.ashx/System.Web.WebPages.Administration/Resources/AdminResources.resources`  
>   
> <span data-ttu-id="53550-264">To żądanie pobiera wszystkie ciągi zasobów w zestawie *System. Web. Webpages. Administration. dll* .</span><span class="sxs-lookup"><span data-stu-id="53550-264">This request downloads all of the resource strings in the *System.Web.WebPages.Administration.dll* assembly.</span></span> <span data-ttu-id="53550-265">Pobierane są wszystkie zasoby osadzone (nawet te, które nie są przeznaczone do użycia jako zawartość statyczna).</span><span class="sxs-lookup"><span data-stu-id="53550-265">All of the embedded resources (even those that are not intended to be served as static content) are downloaded.</span></span> <span data-ttu-id="53550-266">Jeśli zasoby osadzone zawierają informacje poufne, może to stanowić zagrożenie dla bezpieczeństwa.</span><span class="sxs-lookup"><span data-stu-id="53550-266">If the embedded resources contain sensitive information, this can represent a security risk.</span></span> 
> 
> <span data-ttu-id="53550-267">**Obejście** </span><span class="sxs-lookup"><span data-stu-id="53550-267">**Workaround** </span></span>  
> <span data-ttu-id="53550-268">W przypadku tworzenia obiektu **ApplicationPart** upewnij się, że osadzone zasoby skojarzone z tym zestawem obiektu **ApplicationPart** nie zawierają informacji poufnych.</span><span class="sxs-lookup"><span data-stu-id="53550-268">If you create an **ApplicationPart** object, make sure that the embedded resources associated with that **ApplicationPart** object's assembly do not contain sensitive information.</span></span>

<a id="Known_Issues_WebMatrix"></a>

### <a name="webmatrix"></a><span data-ttu-id="53550-269">WebMatrix</span><span class="sxs-lookup"><span data-stu-id="53550-269">WebMatrix</span></span>

> [!NOTE]
> <span data-ttu-id="53550-270">Aby uzyskać informacje o problemach z instalacją w programie WebMatrix, zobacz [problemy z instalacją WebMatrix](#Known_Issues_Installation) wcześniej w tym dokumencie.</span><span class="sxs-lookup"><span data-stu-id="53550-270">For information about installation issues for WebMatrix, see [WebMatrix Installation Issues](#Known_Issues_Installation) earlier in this document.</span></span>

<span data-ttu-id="53550-271">W tej sekcji dokumentu opisano znane problemy dotyczące środowiska deweloperskiego WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="53550-271">This section of the document describes known issues for the WebMatrix development environment.</span></span>

#### <a name="issue-changes-in-the-username-or-password-of-a-database-connection-string-in-a-webconfig-file-are-not-reflected-in-the-databases-workspace"></a><span data-ttu-id="53550-272">Problem: zmiany nazwy użytkownika lub hasła parametrów połączenia z bazą danych w pliku Web. config nie są odzwierciedlone w obszarze roboczym bazy danych</span><span class="sxs-lookup"><span data-stu-id="53550-272">Issue: Changes in the username or password of a database connection string in a web.config file are not reflected in the Databases workspace</span></span>

> <span data-ttu-id="53550-273">**Obejście**</span><span class="sxs-lookup"><span data-stu-id="53550-273">**Workaround**</span></span>  
> 
> 1. <span data-ttu-id="53550-274">W pliku *Web. config* Zmień nazwę bazy danych w parametrach połączenia (na przykład Dodaj do niej "1").</span><span class="sxs-lookup"><span data-stu-id="53550-274">In the *web.config* file, change the database name in the connection string (for example, add "1" to it).</span></span>
> 2. <span data-ttu-id="53550-275">Zapisz plik *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="53550-275">Save the *web.config* file.</span></span>
> 3. <span data-ttu-id="53550-276">Kliknij pozycję **bazy danych** i Odśwież.</span><span class="sxs-lookup"><span data-stu-id="53550-276">Click **Databases** and refresh.</span></span>
> 4. <span data-ttu-id="53550-277">Zmień nazwę bazy danych w parametrach połączenia w pliku *Web. config* z powrotem na oryginalną nazwę bazy danych.</span><span class="sxs-lookup"><span data-stu-id="53550-277">Change the database name in the connection string in the *web.config* file back to the original database name.</span></span>
> 5. <span data-ttu-id="53550-278">Zapisz plik *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="53550-278">Save the *web.config* file.</span></span>
> 6. <span data-ttu-id="53550-279">Kliknij pozycję **bazy danych** i Odśwież.</span><span class="sxs-lookup"><span data-stu-id="53550-279">Click **Databases** and refresh.</span></span>

#### <a name="issue-folders-created-by-webmatrix-cannot-be-deleted"></a><span data-ttu-id="53550-280">Problem: nie można usunąć folderów utworzonych przez program WebMatrix</span><span class="sxs-lookup"><span data-stu-id="53550-280">Issue: Folders created by WebMatrix cannot be deleted</span></span>

> <span data-ttu-id="53550-281">Jeśli program WebMatrix jest uruchomiony przy użyciu podwyższonych uprawnień (to oznacza, że uruchomiono polecenie WebMatrix przy użyciu opcji **Uruchom jako administrator** w systemie Windows), foldery tworzone przez program WebMatrix nie mogą zostać usunięte za pomocą Eksploratora Windows.</span><span class="sxs-lookup"><span data-stu-id="53550-281">If WebMatrix is running using elevated permissions (that is, you started WebMatrix using the **Run as Administrator** option in Windows), folders that are created by WebMatrix cannot be deleted using Windows Explorer.</span></span>
> 
> <span data-ttu-id="53550-282">**Obejście**</span><span class="sxs-lookup"><span data-stu-id="53550-282">**Workaround**</span></span>  
> <span data-ttu-id="53550-283">Uruchom Eksploratora Windows przy użyciu podwyższonych uprawnień.</span><span class="sxs-lookup"><span data-stu-id="53550-283">Run Windows Explorer using elevated permissions.</span></span> <span data-ttu-id="53550-284">Wykonaj następujące kroki:</span><span class="sxs-lookup"><span data-stu-id="53550-284">Follow these steps:</span></span>  
> 
> 1. <span data-ttu-id="53550-285">W systemie Windows kliknij przycisk **Uruchom**.</span><span class="sxs-lookup"><span data-stu-id="53550-285">In Windows, click **Start**.</span></span>
> 2. <span data-ttu-id="53550-286">Wprowadź "Eksplorator Windows" i kliknij prawym przyciskiem myszy wpis dla **Eksploratora Windows**.</span><span class="sxs-lookup"><span data-stu-id="53550-286">Enter "Windows Explorer" and right-click the entry for **Windows Explorer**.</span></span>
> 3. <span data-ttu-id="53550-287">Kliknij przycisk **Uruchom jako administrator**.</span><span class="sxs-lookup"><span data-stu-id="53550-287">Click **Run as Administrator**.</span></span> <span data-ttu-id="53550-288">Następnie można usunąć foldery.</span><span class="sxs-lookup"><span data-stu-id="53550-288">You can then delete the folders.</span></span>

#### <a name="issue-webmatrix-10-is-unable-to-perform-certain-tasks-that-require-elevation"></a><span data-ttu-id="53550-289">Problem: w programie WebMatrix 1,0 nie można wykonać niektórych zadań wymagających podniesienia uprawnień</span><span class="sxs-lookup"><span data-stu-id="53550-289">Issue: WebMatrix 1.0 is unable to perform certain tasks that require elevation</span></span>

> <span data-ttu-id="53550-290">W programie WebMatrix 1,0 nie jest możliwe wykonanie pewnych zadań wymagających podniesienia uprawnień, takich jak instalowanie dodatkowych składników w następujących sytuacjach:</span><span class="sxs-lookup"><span data-stu-id="53550-290">WebMatrix 1.0 is unable to perform certain tasks that require elevation, such as installing additional components in the following situations:</span></span>
> 
> - <span data-ttu-id="53550-291">W systemie Windows Vista lub Windows 7 użytkownik zalogował się przy użyciu konta, które nie ma uprawnień administracyjnych i kontrola konta użytkownika (UAC) jest wyłączona.</span><span class="sxs-lookup"><span data-stu-id="53550-291">On Windows Vista or Windows 7, you are logged in with an account that does not have administrative privileges and User Account Control (UAC) is disabled.</span></span>
> - <span data-ttu-id="53550-292">Używasz systemu Microsoft Windows XP lub Microsoft Windows Server 2003.</span><span class="sxs-lookup"><span data-stu-id="53550-292">You are using Microsoft Windows XP or Microsoft Windows Server 2003.</span></span>
> 
> <span data-ttu-id="53550-293">**Obejście**</span><span class="sxs-lookup"><span data-stu-id="53550-293">**Workaround**</span></span>  
> <span data-ttu-id="53550-294">Większość zadań w programie WebMatrix 1,0 nie wymaga uprawnień administracyjnych.</span><span class="sxs-lookup"><span data-stu-id="53550-294">Most tasks in WebMatrix 1.0 do not require administrative permission.</span></span> <span data-ttu-id="53550-295">Dla tych, które wykonują operację jako administrator, lub wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="53550-295">For those that do, you can perform the operation as an administrator, or follow these steps:</span></span>
> 
> - <span data-ttu-id="53550-296">W systemie Windows Vista lub Windows 7 Włącz kontrolę konta użytkownika.</span><span class="sxs-lookup"><span data-stu-id="53550-296">On Windows Vista or Windows 7, enable UAC.</span></span>
> - <span data-ttu-id="53550-297">W systemie Windows XP Dodaj użytkownika do grupy zabezpieczeń Administratorzy.</span><span class="sxs-lookup"><span data-stu-id="53550-297">On Windows XP, add the user to the Administrators security group.</span></span>

#### <a name="issue-site-from-web-gallery-is-disabled"></a><span data-ttu-id="53550-298">Problem: "Witryna z galerii sieci Web" jest wyłączona</span><span class="sxs-lookup"><span data-stu-id="53550-298">Issue: "Site from Web Gallery" is disabled</span></span>

> <span data-ttu-id="53550-299">Opcja **Witryna z galerii sieci Web** jest wyłączona, jeśli nie zainstalowano Instalatora platformy sieci Web 3,0.</span><span class="sxs-lookup"><span data-stu-id="53550-299">The **Site from Web Gallery** option is disabled if the Web Platform Installer 3.0 is not installed.</span></span>
> 
> <span data-ttu-id="53550-300">**Obejście**</span><span class="sxs-lookup"><span data-stu-id="53550-300">**Workaround**</span></span>  
> <span data-ttu-id="53550-301">Zainstaluj [Instalator platformy Microsoft Web 3,0](https://go.microsoft.com/fwlink/?LinkID=194638).</span><span class="sxs-lookup"><span data-stu-id="53550-301">Install the [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span></span>

#### <a name="issue-google-chrome-is-not-available-as-a-run-option"></a><span data-ttu-id="53550-302">Problem: Google Chrome nie jest dostępna jako opcja uruchamiania</span><span class="sxs-lookup"><span data-stu-id="53550-302">Issue: Google Chrome is not available as a Run option</span></span>

> <span data-ttu-id="53550-303">Przeglądarka Google Chrome nie jest wyświetlana na liście przeglądarek w obszarze **Uruchom** na karcie **Narzędzia główne** .</span><span class="sxs-lookup"><span data-stu-id="53550-303">Google Chrome is not displayed in the list of browsers under **Run** on the **Home** tab.</span></span>
> 
> <span data-ttu-id="53550-304">**Obejście**</span><span class="sxs-lookup"><span data-stu-id="53550-304">**Workaround**</span></span>  
> <span data-ttu-id="53550-305">Niektóre wersje usługi Google Chrome nie rejestrują się prawidłowo przy użyciu funkcji domyślnych programów w systemie Windows.</span><span class="sxs-lookup"><span data-stu-id="53550-305">Some versions of Google Chrome do not register themselves correctly with the Default Programs feature in Windows.</span></span> <span data-ttu-id="53550-306">Aby obejść ten element, uruchom program Google Chrome, kliknij menu *Dostosowywanie i kontrolowanie Google Chrome* , kliknij pozycję *Opcje*, a następnie kliknij pozycję *Udostępnij Google Chrome moją domyślną przeglądarkę*.</span><span class="sxs-lookup"><span data-stu-id="53550-306">As a workaround, start Google Chrome, click the *Customize and control Google Chrome* menu, click *Options*, and then click *Make Google Chrome my default browser*.</span></span>

#### <a name="issue-the-foreign-key-dialog-box-doesnt-allow-entering-a-primary-key"></a><span data-ttu-id="53550-307">Problem: okno dialogowe "klucz obcy" nie zezwala na wprowadzenie klucza podstawowego</span><span class="sxs-lookup"><span data-stu-id="53550-307">Issue: The "Foreign Key" dialog box doesn't allow entering a primary key</span></span>

> <span data-ttu-id="53550-308">Okno dialogowe **klucz obcy** nie pozwala na wprowadzenie nazwy klucza podstawowego z tabeli klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="53550-308">The **Foreign Key** dialog box does not allow you to enter the primary key name from the primary key table.</span></span>
> 
> <span data-ttu-id="53550-309">**Obejście**</span><span class="sxs-lookup"><span data-stu-id="53550-309">**Workaround**</span></span>  
> <span data-ttu-id="53550-310">Jest to zamierzone.</span><span class="sxs-lookup"><span data-stu-id="53550-310">This is intentional.</span></span> <span data-ttu-id="53550-311">Nie trzeba wprowadzać nazwy klucza podstawowego z tabeli klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="53550-311">You do not need to enter the name of the primary key from the primary key table.</span></span>

#### <a name="issue-intellisense-is-not-available-in-webmatrix-for-razor-syntax-c-or-visual-basic"></a><span data-ttu-id="53550-312">Problem: Funkcja IntelliSense nie jest dostępna w programie WebMatrix dla C#składnia Razor, lub Visual Basic</span><span class="sxs-lookup"><span data-stu-id="53550-312">Issue: IntelliSense is not available in WebMatrix for Razor syntax, C#, or Visual Basic</span></span>

> <span data-ttu-id="53550-313">Technologia IntelliSense jest obsługiwana w programie WebMatrix dla HTML i CSS.</span><span class="sxs-lookup"><span data-stu-id="53550-313">IntelliSense is supported in WebMatrix for HTML and CSS.</span></span> <span data-ttu-id="53550-314">Nie jest to jednak dostępne w innych językach.</span><span class="sxs-lookup"><span data-stu-id="53550-314">However, it is not available for other languages.</span></span> 
> 
> <span data-ttu-id="53550-315">**Obejście** </span><span class="sxs-lookup"><span data-stu-id="53550-315">**Workaround** </span></span>  
> <span data-ttu-id="53550-316">Brak.</span><span class="sxs-lookup"><span data-stu-id="53550-316">None.</span></span>

#### <a name="issue-intellisense-for-html-and-css-suggests-elements-that-are-not-contextually-appropriate"></a><span data-ttu-id="53550-317">Problem: Funkcja IntelliSense dla HTML i CSS sugeruje elementy, które nie są kontekstowe.</span><span class="sxs-lookup"><span data-stu-id="53550-317">Issue: IntelliSense for HTML and CSS suggests elements that are not contextually appropriate</span></span>

> <span data-ttu-id="53550-318">Funkcja IntelliSense for Markup w programie WebMatrix obsługuje kod HTML przy użyciu [schematu przejściowego XHTML 1,0](http://www.w3.org/TR/2002/NOTE-xhtml1-schema-20020902/#xhtml1-transitional) i CSS przy użyciu [schematu CSS 2,1](http://www.w3.org/TR/CSS2/).</span><span class="sxs-lookup"><span data-stu-id="53550-318">IntelliSense for markup in WebMatrix supports HTML using the [XHTML 1.0 Transitional schema](http://www.w3.org/TR/2002/NOTE-xhtml1-schema-20020902/#xhtml1-transitional) and CSS using the [CSS 2.1 schema](http://www.w3.org/TR/CSS2/).</span></span> <span data-ttu-id="53550-319">Ponieważ technologia IntelliSense jest oparta na tych konkretnych schematach, można zasugerować niektóre Tagi, atrybuty lub właściwości, które nie są odpowiednie dla bieżącej definicji strony lub stylu.</span><span class="sxs-lookup"><span data-stu-id="53550-319">Because IntelliSense is based on these specific schemas, certain tags, attributes, or properties might be suggested that are not appropriate for the current page or style definition.</span></span> <span data-ttu-id="53550-320">W przypadku języka HTML może również prowadzić do nieoczekiwanych sugestii zawartości, które mogą być interpretowane jako źle sformułowane XHTML (na przykład gdy Tagi nie są zamknięte).</span><span class="sxs-lookup"><span data-stu-id="53550-320">For HTML, it can also lead to unexpected suggestions in content that might be interpreted as malformed XHTML (for example, when tags are not closed).</span></span> <span data-ttu-id="53550-321">Ten problem może być bardziej zauważalny, gdy punkt wstawiania znajduje się wewnątrz niekompletnego tagu; w takim przypadku technologia IntelliSense może zasugerować nowe Tagi otwierające lub zaoferować inne błędne sugestie.</span><span class="sxs-lookup"><span data-stu-id="53550-321">This issue might be more noticeable if the insertion point is inside an incomplete tag; in that case, IntelliSense might suggest new opening tags or offer other incorrect suggestions.</span></span> 
> 
> <span data-ttu-id="53550-322">**Obejście** </span><span class="sxs-lookup"><span data-stu-id="53550-322">**Workaround** </span></span>  
> <span data-ttu-id="53550-323">W przypadku języka HTML upewnij się, że Pracujesz w dobrze sformułowanej, kompletnej stronie XHTML.</span><span class="sxs-lookup"><span data-stu-id="53550-323">For HTML, make sure that you are working within a well-formed, complete XHTML page.</span></span> <span data-ttu-id="53550-324">W przypadku CSS nie istnieje obejście tego problemu.</span><span class="sxs-lookup"><span data-stu-id="53550-324">For CSS, there is no workaround.</span></span>

#### <a name="issue-intellisense-is-not-invoked-while-you-type"></a><span data-ttu-id="53550-325">Problem: Funkcja IntelliSense nie jest wywoływana podczas wpisywania</span><span class="sxs-lookup"><span data-stu-id="53550-325">Issue: IntelliSense is not invoked while you type</span></span>

> <span data-ttu-id="53550-326">Czasami funkcja IntelliSense może nie zostać wywołana, ponieważ w edytorze wprowadzono kod HTML lub CSS.</span><span class="sxs-lookup"><span data-stu-id="53550-326">At times, IntelliSense might not be invoked as HTML or CSS is being entered in the editor.</span></span> <span data-ttu-id="53550-327">W szczególności może się to zdarzyć, gdy punkt wstawiania znajduje się bezpośrednio obok innego elementu lub na końcu pliku.</span><span class="sxs-lookup"><span data-stu-id="53550-327">In particular, this might happen when the insertion point is directly next to another element or at the end of a file.</span></span> 
> 
> <span data-ttu-id="53550-328">**Obejście** </span><span class="sxs-lookup"><span data-stu-id="53550-328">**Workaround** </span></span>  
> <span data-ttu-id="53550-329">Upewnij się, że występuje odstęp wokół punktu wstawiania i że punkt wstawiania nie znajduje się na końcu pliku.</span><span class="sxs-lookup"><span data-stu-id="53550-329">Make sure that there is whitespace around the insertion point and that the insertion point is not at the end of a file.</span></span> <span data-ttu-id="53550-330">Możesz również ręcznie wywołać technologię IntelliSense, naciskając klawisze CTRL + SPACJA.</span><span class="sxs-lookup"><span data-stu-id="53550-330">You can also invoke IntelliSense manually by pressing Ctrl+Space.</span></span>

#### <a name="issue-no-ui-is-available-for-disabling-intellisense"></a><span data-ttu-id="53550-331">Problem: Brak dostępnego interfejsu użytkownika do wyłączania funkcji IntelliSense</span><span class="sxs-lookup"><span data-stu-id="53550-331">Issue: No UI is available for disabling IntelliSense</span></span>

> <span data-ttu-id="53550-332">Program WebMatrix 1,0 nie zapewnia interfejsu użytkownika ani gestu do wyłączania funkcji IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="53550-332">WebMatrix 1.0 provides no UI or gesture for disabling IntelliSense.</span></span> 
> 
> <span data-ttu-id="53550-333">**Obejście** </span><span class="sxs-lookup"><span data-stu-id="53550-333">**Workaround** </span></span>  
> <span data-ttu-id="53550-334">Uruchom program WebMatrix przy użyciu następującego polecenia, które obejmuje przełącznik, który wyłącza funkcję IntelliSense:</span><span class="sxs-lookup"><span data-stu-id="53550-334">Start WebMatrix using the following command, which includes a switch that disables IntelliSense:</span></span>  
>   
> `WebMatrix.exe #ExecuteCommand# EditorIntelliSense off`

<a id="Known_Issues_IISExpress"></a>
### <a name="iis-express"></a><span data-ttu-id="53550-335">Usługi IIS Express</span><span class="sxs-lookup"><span data-stu-id="53550-335">IIS Express</span></span>

<span data-ttu-id="53550-336">IIS Express ma swój własny plik Readme, który jest dostępny pod następującym adresem URL:</span><span class="sxs-lookup"><span data-stu-id="53550-336">IIS Express has its own readme file, which is available at the following URL:</span></span>

[<span data-ttu-id="53550-337">https://go.microsoft.com/fwlink/?LinkID=207675&amp; clcid = 0x409</span><span class="sxs-lookup"><span data-stu-id="53550-337">https://go.microsoft.com/fwlink/?LinkID=207675&amp;clcid=0x409</span></span>](https://go.microsoft.com/fwlink/?LinkID=207675&amp;clcid=0x409)

<a id="Known_Issues_SQLServerCompact"></a>

### <a name="sql-server-compact"></a><span data-ttu-id="53550-338">SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="53550-338">SQL Server Compact</span></span>

<span data-ttu-id="53550-339">SQL Server Compact ma swój własny plik Readme, który jest dostępny pod następującym adresem URL:</span><span class="sxs-lookup"><span data-stu-id="53550-339">SQL Server Compact has its own readme file, which is available at the following URL:</span></span>

[https://go.microsoft.com/fwlink/?LinkID=208545](https://go.microsoft.com/fwlink/?LinkID=208545&amp;clcid=0x409)

<span data-ttu-id="53550-340">Aby uzyskać informacje o problemach związanych z instalowaniem SQL Server Compact w ramach programu WebMatrix, zobacz [problemy dotyczące instalacji programu WebMatrix](#Known_Issues_Installation) wcześniej w tym dokumencie.</span><span class="sxs-lookup"><span data-stu-id="53550-340">For information about issues that involve installing SQL Server Compact as part of WebMatrix, see [WebMatrix Installation Issues](#Known_Issues_Installation) earlier in this document.</span></span>

### <a id="Known_Issues_Installing_Applications"></a><span data-ttu-id="53550-341">Instalowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="53550-341">Installing Applications</span></span>

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a><span data-ttu-id="53550-342">Problem: instalacja aplikacji może zająć dużo czasu, jeśli folder Moje dokumenty użytkownika zostanie przekierowany do udziału sieciowego</span><span class="sxs-lookup"><span data-stu-id="53550-342">Issue: Installing an application can take a long time if the user's My Documents folder is redirected to a network share</span></span>

> <span data-ttu-id="53550-343">**Obejście**</span><span class="sxs-lookup"><span data-stu-id="53550-343">**Workaround**</span></span>  
> <span data-ttu-id="53550-344">Brak.</span><span class="sxs-lookup"><span data-stu-id="53550-344">None.</span></span> <span data-ttu-id="53550-345">Instalacja aplikacji może potrwać trochę czasu, ale instalacja zostanie poprawna.</span><span class="sxs-lookup"><span data-stu-id="53550-345">The application might take a while to install, but will install correctly.</span></span>

### <a id="Known_Issues_Publishing_Applications"></a><span data-ttu-id="53550-346">Publikowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="53550-346">Publishing Applications</span></span>

#### <a name="issue-required-permissions-cannot-be-acquired-error-when-publishing-a-sql-compact-database"></a><span data-ttu-id="53550-347">Problem: "nie można pobrać wymaganych uprawnień" podczas publikowania bazy danych SQL Compact</span><span class="sxs-lookup"><span data-stu-id="53550-347">Issue: "Required permissions cannot be acquired" error when publishing a SQL Compact Database</span></span>

> <span data-ttu-id="53550-348">Program WebMatrix nie obsługuje w pełni wdrażania pomocniczych plików binarnych dla SQL Server Compact na serwerze z uruchomionym .NET Framework wersji 3,5 z konfiguracją średniego zaufania.</span><span class="sxs-lookup"><span data-stu-id="53550-348">WebMatrix does not fully support deploying supporting binaries for SQL Server Compact to a server that is running .NET Framework version 3.5 with a medium trust configuration.</span></span>
> 
> <span data-ttu-id="53550-349">**Obejście**</span><span class="sxs-lookup"><span data-stu-id="53550-349">**Workaround**</span></span>  
> <span data-ttu-id="53550-350">Zalecane obejście polega na zainstalowaniu na serwerze .NET Framework 4.</span><span class="sxs-lookup"><span data-stu-id="53550-350">The preferred workaround is to install the .NET Framework 4 on the server.</span></span> <span data-ttu-id="53550-351">Alternatywnie wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="53550-351">Alternatively, do the following:</span></span>
> 
> 1. <span data-ttu-id="53550-352">Dodaj następujące elementy do sekcji `SecurityClasses` w pliku *Web\_MediumTrust. config* :</span><span class="sxs-lookup"><span data-stu-id="53550-352">Add the following elements to the `SecurityClasses` section in *Web\_MediumTrust.config* file:</span></span>
> 
>     [!code-html[Main](overview/samples/sample6.html)]
> 2. <span data-ttu-id="53550-353">Utwórz nowy zestaw uprawnień w pliku *Web\_MediumTrust. config* z następującymi wymaganymi uprawnieniami:</span><span class="sxs-lookup"><span data-stu-id="53550-353">Create a new permission set in the *Web\_MediumTrust.config* file with the following required permissions:</span></span>
> 
>     [!code-html[Main](overview/samples/sample7.html)]
> 3. <span data-ttu-id="53550-354">Zastosuj zestaw uprawnień do SQL Server Compact, umieszczając następujące elementy w pliku *\_MediumTrust. config sieci Web* :</span><span class="sxs-lookup"><span data-stu-id="53550-354">Apply the permission set to SQL Server Compact by putting the following elements in the *Web\_MediumTrust.config* file:</span></span>
> 
>     [!code-html[Main](overview/samples/sample8.html)]

#### <a name="issue-gallery-and-phpbb-web-applications-display-a-service-is-unavailable-error-after-publishing"></a><span data-ttu-id="53550-355">Problem: w galerii i aplikacjach sieci Web PhpBB jest wyświetlany komunikat o błędzie "usługa jest niedostępna" po opublikowaniu</span><span class="sxs-lookup"><span data-stu-id="53550-355">Issue: Gallery and PhpBB web applications display a "Service is unavailable" error after publishing</span></span>

> <span data-ttu-id="53550-356">W pewnych okolicznościach opublikowanie aplikacji powoduje błąd "usługa jest niedostępna".</span><span class="sxs-lookup"><span data-stu-id="53550-356">Under some circumstances, publishing an application causes a "service is unavailable" error.</span></span>
> 
> <span data-ttu-id="53550-357">**Obejście**</span><span class="sxs-lookup"><span data-stu-id="53550-357">**Workaround**</span></span>  
> <span data-ttu-id="53550-358">W programie WebMatrix Dodaj ukośnik odwrotny (\) na końcu nazwy serwera w oknie **Ustawienia publikowania** , a następnie ponownie Opublikuj aplikację.</span><span class="sxs-lookup"><span data-stu-id="53550-358">In WebMatrix, add a backslash (\) to the end of the server name in the **Publish Settings** window and then publish the application again.</span></span>

#### <a name="issue-moodle-website-layout-and-links-are-broken-after-publishing"></a><span data-ttu-id="53550-359">Problem: układ i linki witryny sieci Web Moodle są zerwane po opublikowaniu</span><span class="sxs-lookup"><span data-stu-id="53550-359">Issue: Moodle website layout and links are broken after publishing</span></span>

> <span data-ttu-id="53550-360">Po opublikowaniu aplikacji Moodle aplikacja nie działa prawidłowo.</span><span class="sxs-lookup"><span data-stu-id="53550-360">After you publish a Moodle application, the application does not work correctly.</span></span>
> 
> <span data-ttu-id="53550-361">**Obejście**</span><span class="sxs-lookup"><span data-stu-id="53550-361">**Workaround**</span></span>  
> <span data-ttu-id="53550-362">W programie WebMatrix Dodaj ukośnik (/) na końcu pola **Nazwa lokacji** w oknie **Ustawienia publikowania** , a następnie ponownie Opublikuj aplikację.</span><span class="sxs-lookup"><span data-stu-id="53550-362">In WebMatrix, add a slash (/) to the end of the **Site Name** field in the **Publish Settings** window and then publish the application again.</span></span>

#### <a name="issue-publishing-nopcommerce-fails-with-a-database-error"></a><span data-ttu-id="53550-363">Problem: publikowanie witryny systemu NopCommerce kończy się niepowodzeniem z powodu błędu bazy danych</span><span class="sxs-lookup"><span data-stu-id="53550-363">Issue: Publishing nopCommerce fails with a database error</span></span>

> <span data-ttu-id="53550-364">Publikowanie witryny systemu NopCommerce kończy się niepowodzeniem i zgłasza błąd bazy danych, na przykład "Wstaw do tabeli dziennika\_NOP."</span><span class="sxs-lookup"><span data-stu-id="53550-364">Publishing nopCommerce fails and reports a database error like "Insert into the nop\_log table failed."</span></span>
> 
> <span data-ttu-id="53550-365">**Obejście**</span><span class="sxs-lookup"><span data-stu-id="53550-365">**Workaround**</span></span>  
> 
> 1. <span data-ttu-id="53550-366">W programie WebMatrix kliknij pozycję **Uruchom** , aby uruchomić witryny systemu NopCommerce lokalnie.</span><span class="sxs-lookup"><span data-stu-id="53550-366">In WebMatrix, click **Run** to launch nopCommerce locally.</span></span>
> 2. <span data-ttu-id="53550-367">Zaloguj się na stronie Administracja.</span><span class="sxs-lookup"><span data-stu-id="53550-367">Log into the administration page.</span></span>
> 3. <span data-ttu-id="53550-368">Kliknij menu **system** .</span><span class="sxs-lookup"><span data-stu-id="53550-368">Click the **System** menu.</span></span>
> 4. <span data-ttu-id="53550-369">Kliknij opcję **Dziennik** .</span><span class="sxs-lookup"><span data-stu-id="53550-369">Click the **Log** option.</span></span>
> 5. <span data-ttu-id="53550-370">Kliknij przycisk **Wyczyść dziennik**.</span><span class="sxs-lookup"><span data-stu-id="53550-370">Click the **Clear Log** button.</span></span>
> 6. <span data-ttu-id="53550-371">Ponownie Opublikuj witryny systemu NopCommerce.</span><span class="sxs-lookup"><span data-stu-id="53550-371">Publish nopCommerce again.</span></span>

#### <a name="issue-silverstripe-cms-displays-a-http-500-php-fcgi-error-when-you-download-a-published-site"></a><span data-ttu-id="53550-372">Problem: podczas pobierania opublikowanej witryny w usłudze CMS SilverStripe jest wyświetlany komunikat o błędzie "HTTP 500 PHP FCGI"</span><span class="sxs-lookup"><span data-stu-id="53550-372">Issue: Silverstripe CMS displays a "HTTP 500 PHP FCGI Error" when you download a published site</span></span>

> <span data-ttu-id="53550-373">**Obejście**</span><span class="sxs-lookup"><span data-stu-id="53550-373">**Workaround**</span></span>  
> <span data-ttu-id="53550-374">Po kliknięciu przycisku **Pobierz opublikowaną witrynę**Pomiń `silverstripe-cache/manifest_main` w **wersji zapoznawczej publikowania**.</span><span class="sxs-lookup"><span data-stu-id="53550-374">After you click **Download published site**, skip `silverstripe-cache/manifest_main` in **Publish Preview**.</span></span> <span data-ttu-id="53550-375">Ten plik jest używany na potrzeby buforowania i jest przeznaczony dla każdego komputera.</span><span class="sxs-lookup"><span data-stu-id="53550-375">This file is used for caching purposes and is specific to each computer.</span></span>

#### <a name="issue-subtext-displays-server-error-in--application-when-you-download-a-published-site"></a><span data-ttu-id="53550-376">Problem: w przypadku pobierania opublikowanej witryny w polu "błąd serwera w aplikacji"/"jest wyświetlany komunikat o błędzie</span><span class="sxs-lookup"><span data-stu-id="53550-376">Issue: Subtext displays "Server Error in '/' Application" when you download a published site</span></span>

> <span data-ttu-id="53550-377">**Obejście**</span><span class="sxs-lookup"><span data-stu-id="53550-377">**Workaround**</span></span>  
> <span data-ttu-id="53550-378">Otwórz plik *Web. config* witryny i Zastąp identyfikator użytkownika i hasło w parametrach połączenia z bazą danych poświadczeniami administratora SQL Server (poświadczeniami "sa").</span><span class="sxs-lookup"><span data-stu-id="53550-378">Open the site's *web.config* file and replace the user ID and password in the database connection string with the SQL Server administrator credentials (the "sa" credentials).</span></span>
> 
> <span data-ttu-id="53550-379">Alternatywnie wykonaj następujące kroki, aby nawiązać konto użytkownika, za pomocą którego zalogowano się `db_owner` uprawnień:</span><span class="sxs-lookup"><span data-stu-id="53550-379">Alternatively, follow these steps in order to give the user account you are logged in with `db_owner` permissions:</span></span>
> 
> 1. <span data-ttu-id="53550-380">Zainstaluj SQL Server Management Studio przy użyciu Instalatora platformy sieci Web.</span><span class="sxs-lookup"><span data-stu-id="53550-380">Install SQL Server Management Studio using the Web Platform Installer.</span></span>
> 2. <span data-ttu-id="53550-381">Połącz się z lokalnym wystąpieniem SQL Server Express (domyślnie `.\SQLEXPRESS`).</span><span class="sxs-lookup"><span data-stu-id="53550-381">Connect to the local SQL Server Express instance (by default, `.\SQLEXPRESS`).</span></span>
> 3. <span data-ttu-id="53550-382">Kliknij kolejno pozycje **bazy danych** &gt; *[LocalSubtextDatabase]* &gt; **zabezpieczenia** &gt; **Użytkownicy** &gt; *[localSubtextUser*] (wartość domyślna to `subtextuser`], kliknij prawym przyciskiem myszy, a następnie kliknij pozycję **Właściwości**.</span><span class="sxs-lookup"><span data-stu-id="53550-382">Click **Databases** &gt; *[localSubtextDatabase]* &gt; **Security** &gt; **Users** &gt; *[localSubtextUser*] (default is `subtextuser`], right-click, and click **Properties**.</span></span>
> 4. <span data-ttu-id="53550-383">Wybierz pozycję **db\_Owner** w sekcji członkostwo w roli.</span><span class="sxs-lookup"><span data-stu-id="53550-383">Select **db\_owner** in the role membership section.</span></span>

#### <a name="issue-site-might-not-work-after-publishing-if-the-destination-url-field-is-not-prefixed-with-http-or-https"></a><span data-ttu-id="53550-384">Problem: witryna może nie zadziałać po opublikowaniu, jeśli pole "docelowy adres URL" nie jest poprzedzone prefiksem http://lub https://</span><span class="sxs-lookup"><span data-stu-id="53550-384">Issue: Site might not work after publishing if the "Destination URL" field is not prefixed with http:// or https://</span></span>

> <span data-ttu-id="53550-385">Jeśli docelowy adres URL nie zaczyna się od `http://` lub `https://`, w oknie dialogowym **Ustawienia publikowania** witryna może nie zadziałać po wdrożeniu.</span><span class="sxs-lookup"><span data-stu-id="53550-385">In the **Publishing Settings** dialog box, if the destination URL does not begin with `http://` or `https://`, the site might not work after deployment.</span></span>
> 
> <span data-ttu-id="53550-386">**Obejście**</span><span class="sxs-lookup"><span data-stu-id="53550-386">**Workaround**</span></span>  
> <span data-ttu-id="53550-387">Przed opublikowaniem witryny upewnij się, że docelowy adres URL w oknie dialogowym **Ustawienia publikowania** zaczyna się od `http://` lub `https://`.</span><span class="sxs-lookup"><span data-stu-id="53550-387">Make sure that before you publish a site, the destination URL in the **Publish Settings** dialog box starts with `http://` or `https://`.</span></span>

#### <a name="issue-publishing-a-mysql-database-fails-with-the-error-failed-to-publish-the-database-this-can-happen-if-the-remote-database-cannot-run-the-script"></a><span data-ttu-id="53550-388">Problem: Publikowanie bazy danych MySQL kończy się niepowodzeniem z powodu błędu "nie można opublikować bazy danych.</span><span class="sxs-lookup"><span data-stu-id="53550-388">Issue: Publishing a MySQL database fails with the error "Failed to publish the database.</span></span> <span data-ttu-id="53550-389">Może się tak zdarzyć, jeśli zdalna baza danych nie może uruchomić skryptu ".</span><span class="sxs-lookup"><span data-stu-id="53550-389">This can happen if the remote database cannot run the script."</span></span>

> <span data-ttu-id="53550-390">Ten błąd może wystąpić z kilku powodów.</span><span class="sxs-lookup"><span data-stu-id="53550-390">The error can occur for a number of reasons.</span></span> <span data-ttu-id="53550-391">Jedną z przyczyn tego błędu jest to, że ten błąd występuje, jeśli skrypt bazy danych zawiera znak pojedynczego cudzysłowu ('), a docelowy zestaw znaków bazy danych MySQL nie jest w formacie UTF-8.</span><span class="sxs-lookup"><span data-stu-id="53550-391">One reason you can see this error is if the database script contains a single quotation character (') and the destination MySQL database's default character set is not to UTF-8.</span></span>
> 
> <span data-ttu-id="53550-392">**Obejście**</span><span class="sxs-lookup"><span data-stu-id="53550-392">**Workaround**</span></span>  
> <span data-ttu-id="53550-393">Ustaw domyślny zestaw znaków dla zdalnej bazy danych MySQL na UTF-8.</span><span class="sxs-lookup"><span data-stu-id="53550-393">Set the default character set for the remote MySQL database to UTF-8.</span></span>

#### <a name="issue-some-links-are-not-visible-in-dotnetnuke-after-publishing-or-downloading-the-site"></a><span data-ttu-id="53550-394">Problem: niektóre linki nie są widoczne w programie DotNetNuke po opublikowaniu lub pobraniu witryny</span><span class="sxs-lookup"><span data-stu-id="53550-394">Issue: Some links are not visible in DotNetNuke after publishing or downloading the site</span></span>

> <span data-ttu-id="53550-395">Jeśli publikujesz lub pobierasz witrynę DotNetNuke, może być konieczne wyczyszczenie pamięci podręcznej w celu pobrania nowych linków do wyświetlania w witrynie.</span><span class="sxs-lookup"><span data-stu-id="53550-395">If you publish or download a DotNetNuke site, you might need to clear the cache to get the new links to appear on the site.</span></span>
> 
> <span data-ttu-id="53550-396">**Obejście**</span><span class="sxs-lookup"><span data-stu-id="53550-396">**Workaround**</span></span>
> 
> 1. <span data-ttu-id="53550-397">Zaloguj się jako "host".</span><span class="sxs-lookup"><span data-stu-id="53550-397">Log in as "Host".</span></span>
> 2. <span data-ttu-id="53550-398">Przejdź do menu hosta, a następnie wybierz pozycję **Ustawienia hosta**.</span><span class="sxs-lookup"><span data-stu-id="53550-398">Go to the host menu and select **Host Settings**.</span></span>
> 3. <span data-ttu-id="53550-399">Przewiń w dół i w obszarze **Ustawienia zaawansowane**, rozwiń węzeł **Ustawienia wydajności**.</span><span class="sxs-lookup"><span data-stu-id="53550-399">Scroll down and under **Advanced Settings**, expand **Performance Settings**.</span></span>
> 4. <span data-ttu-id="53550-400">Kliknij link **Wyczyść pamięć podręczną** dla stron.</span><span class="sxs-lookup"><span data-stu-id="53550-400">Click the **Clear Cache** link for pages.</span></span>
> 5. <span data-ttu-id="53550-401">Przejdź do dolnej części strony i ponownie uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="53550-401">Go to the bottom of the page and restart the application.</span></span>

#### <a name="issue-some-links-in-atomsite-are-broken-after-you-download-a-published-site"></a><span data-ttu-id="53550-402">Problem: niektóre linki w programie AtomSite są uszkodzone po pobraniu opublikowanej witryny</span><span class="sxs-lookup"><span data-stu-id="53550-402">Issue: Some links in AtomSite are broken after you download a published site</span></span>

> <span data-ttu-id="53550-403">**Obejście**</span><span class="sxs-lookup"><span data-stu-id="53550-403">**Workaround**</span></span>  
> <span data-ttu-id="53550-404">W pliku *Service. config* plik *users. config* , a wszystkie pliki *. XML* , Zastąp ciąg adresu URL (na przykład `http://myhost.com/atomsite`) lokalnym (na przykład `http://localhost:1239`).</span><span class="sxs-lookup"><span data-stu-id="53550-404">In the *service.config* file, *users.config* file, and all *.xml* files, replace the URL string (for example, `http://myhost.com/atomsite`) with the local one (for example, `http://localhost:1239`).</span></span>

#### <a name="issue-mysql-based-applications-like-wordpress-fail-to-publish-and-report-a-database-error"></a><span data-ttu-id="53550-405">Problem: w przypadku aplikacji opartych na bazie MySQL, takich jak WordPress, nie można opublikować i zgłosić błędu bazy danych</span><span class="sxs-lookup"><span data-stu-id="53550-405">Issue: MySQL-based applications like WordPress fail to publish and report a database error</span></span>

> <span data-ttu-id="53550-406">Domyślnie program WebMatrix instaluje MySQL z zestawem znaków UTF-8.</span><span class="sxs-lookup"><span data-stu-id="53550-406">By default, WebMatrix installs MySQL with the UTF-8 character set.</span></span> <span data-ttu-id="53550-407">W przypadku instalowania programu MySQL we własnym zestawie znaków nie jest to UTF-8 (na przykład Latin1), proces publikowania dla baz danych może zakończyć się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="53550-407">If you install MySQL on your own, and the character set is not UTF-8 (for example, it is Latin1), the publish process for databases might fail.</span></span>
> 
> <span data-ttu-id="53550-408">**Obejście**</span><span class="sxs-lookup"><span data-stu-id="53550-408">**Workaround**</span></span>
> 
> 1. <span data-ttu-id="53550-409">Zmień zestaw znaków dla programu MySQL na UTF-8.</span><span class="sxs-lookup"><span data-stu-id="53550-409">Change the character set for MySQL to UTF-8.</span></span> <span data-ttu-id="53550-410">(Aby uzyskać szczegółowe informacje, zobacz [zestaw znaków serwera i sortowanie](http://dev.mysql.com/doc/refman/5.0/en/charset-server.html) w witrynie MySQL).</span><span class="sxs-lookup"><span data-stu-id="53550-410">(For details, see [Server Character Set and Collation](http://dev.mysql.com/doc/refman/5.0/en/charset-server.html) on the MySQL website.)</span></span>
> 2. <span data-ttu-id="53550-411">Zainstaluj ponownie aplikację.</span><span class="sxs-lookup"><span data-stu-id="53550-411">Reinstall the application.</span></span>
> 3. <span data-ttu-id="53550-412">Opublikuj ponownie aplikację.</span><span class="sxs-lookup"><span data-stu-id="53550-412">Republish the application.</span></span>

#### <a name="issue-download-published-site-fails-for-applications-that-have-browser-based-setup"></a><span data-ttu-id="53550-413">Problem: "Pobierz opublikowaną witrynę" kończy się niepowodzeniem w przypadku aplikacji korzystających z konfiguracji opartej na przeglądarce</span><span class="sxs-lookup"><span data-stu-id="53550-413">Issue: "Download published site" fails for applications that have browser-based setup</span></span>

> <span data-ttu-id="53550-414">Niektóre aplikacje (na przykład Kentico CMS) wymagają uruchomienia ich w przeglądarce w celu przeprowadzenia instalacji po instalacji, takiej jak tworzenie bazy danych.</span><span class="sxs-lookup"><span data-stu-id="53550-414">Some applications (for example, Kentico CMS) require you to launch them in the browser in order to perform post-installation setup such as creating a database.</span></span> <span data-ttu-id="53550-415">Jeśli opublikujesz aplikację, taką jak ta, bez kończenia konfiguracji opartej na przeglądarce, próba pobrania tej samej lokacji z serwera zdalnego zakończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="53550-415">If you publish an application like this without completing the browser-based setup, attempting to download the same site from a remote server will fail.</span></span>
> 
> <span data-ttu-id="53550-416">**Obejście**</span><span class="sxs-lookup"><span data-stu-id="53550-416">**Workaround**</span></span>  
> <span data-ttu-id="53550-417">Zakończ konfigurację opartą na przeglądarce przed opublikowaniem lokacji.</span><span class="sxs-lookup"><span data-stu-id="53550-417">Finish browser-based setup before publishing the site.</span></span>

#### <a name="issue-download-published-site-fails-with-a-database-error-for-dotnetnuke-and-kooboo-cms"></a><span data-ttu-id="53550-418">Problem: "Pobieranie opublikowanej witryny" kończy się niepowodzeniem z powodu błędu bazy danych dla DotNetNuke i Kooboo CMS</span><span class="sxs-lookup"><span data-stu-id="53550-418">Issue: "Download published site" fails with a database error for DotNetNuke and Kooboo CMS</span></span>

> <span data-ttu-id="53550-419">Jeśli spróbujesz pobrać aplikację z serwera i masz poświadczenia administratora w parametrach połączenia bazy danych w oknie dialogowym **Ustawienia publikowania** , w dzienniku publikacji może zostać wyświetlony następujący błąd:</span><span class="sxs-lookup"><span data-stu-id="53550-419">If you try to download an application from a server and you have administrator credentials in the database connection string in the **Publish Settings** dialog, you might see the following error in the publish log:</span></span>
> 
> [!code-console[Main](overview/samples/sample9.cmd)]
> 
> <span data-ttu-id="53550-420">**Obejście**</span><span class="sxs-lookup"><span data-stu-id="53550-420">**Workaround**</span></span>  
> <span data-ttu-id="53550-421">W razie potrzeby należy ponownie opublikować lokację (lub opublikować ją) przy użyciu poświadczeń innych niż administrator dla bazy danych.</span><span class="sxs-lookup"><span data-stu-id="53550-421">If practical, republish the site (or have it published) using non-administrator credentials for the database.</span></span>

<a id="More_Info"></a>

## <a name="for-more-information"></a><span data-ttu-id="53550-422">Aby uzyskać więcej informacji</span><span class="sxs-lookup"><span data-stu-id="53550-422">For More Information</span></span>

<span data-ttu-id="53550-423">Aby uzyskać więcej informacji o programie WebMatrix 1,0, zobacz następujące witryny sieci Web:</span><span class="sxs-lookup"><span data-stu-id="53550-423">For more information about WebMatrix 1.0, see the following websites:</span></span>

- [<span data-ttu-id="53550-424">IIS.net</span><span class="sxs-lookup"><span data-stu-id="53550-424">IIS.net</span></span>](http://iis.net/)
- [<span data-ttu-id="53550-425">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="53550-425">ASP.NET</span></span>](https://asp.net/webmatrix)
- [<span data-ttu-id="53550-426">Microsoft.com/web</span><span class="sxs-lookup"><span data-stu-id="53550-426">Microsoft.com/web</span></span>](https://www.microsoft.com/web)

<span data-ttu-id="53550-427">© 2011 Microsoft Corporation.</span><span class="sxs-lookup"><span data-stu-id="53550-427">© 2011 Microsoft Corporation.</span></span> <span data-ttu-id="53550-428">Wszelkie prawa zastrzeżone.</span><span class="sxs-lookup"><span data-stu-id="53550-428">All Rights Reserved.</span></span> <span data-ttu-id="53550-429">[Warunki użytkowania](https://msdn.microsoft.cos/cc300389.aspx).</span><span class="sxs-lookup"><span data-stu-id="53550-429">[Terms of Use](https://msdn.microsoft.cos/cc300389.aspx).</span></span>

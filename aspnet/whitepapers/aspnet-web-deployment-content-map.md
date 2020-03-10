---
uid: whitepapers/aspnet-web-deployment-content-map
title: ASP.NET Web Deployment — zalecane zasoby | Microsoft Docs
author: rick-anderson
description: Ten temat zawiera linki do zasobów dokumentacji dotyczących sposobu wdrażania (publikowania) ASP.NET aplikacji sieci Web w usługach IIS przy użyciu programu Visual Studio 2010, Visual Web de...
ms.author: riande
ms.date: 03/14/2014
ms.assetid: 58b583cd-c4ab-47a3-8527-8c92c298c91f
msc.legacyurl: /whitepapers/aspnet-web-deployment-content-map
msc.type: content
ms.openlocfilehash: 96873c8f2b0ad2415f371aceb651400c801a3338
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78636991"
---
# <a name="aspnet-web-deployment---recommended-resources"></a>Wdrożenie internetowe na platformie ASP.NET — zalecane zasoby

> Ten temat zawiera linki do zasobów dokumentacji dotyczących sposobu wdrażania (publikowania) ASP.NET aplikacji sieci Web w usługach IIS przy użyciu programu Visual Studio 2010, Visual Web Developer 2010 i nowszych wersji.
> 
> Jeśli znasz dobry wpis w blogu, wątek [StackOverflow](http://stackoverflow.com) lub inny link, który będzie przydatny, Wyślij do [nas wiadomość e-mail](mailto:aspnetue@microsoft.com?subject=Deployment%20Content%20Map) z linkiem.
> 
> > [!NOTE] 
> > 
> > Wiele z tych zasobów opisuje funkcje wdrażania, które są dostępne tylko w przypadku zainstalowania najnowszej wersji [aktualizacji publikacji w sieci Web programu Visual Studio](https://go.microsoft.com/fwlink/?LinkID=208120). Niektóre funkcje są dostępne tylko w programie Visual Studio 2012 lub Visual Studio 2013.

Ten temat zawiera następujące sekcje:

- [Informacje o opcjach wdrażania dla projektów sieci Web](#understanding)
- [Znajdowanie dostawców hostingu dla aplikacji ASP.NET](#findinghosting)
- [Wdrażanie aplikacji sieci Web z programu Visual Studio](#fromvs)
- [Wdrażanie aplikacji sieci Web przez utworzenie i zainstalowanie pakietu wdrożeniowego sieci Web](#package)
- [Wdrażanie aplikacji sieci Web przy użyciu procesu ciągłej integracji (CI)](#ci)
- [Za pomocą transformacji pliku Web. config zmień ustawienia w pliku docelowym Web. config lub plik App. config podczas wdrażania](#transforms)
- [Zmienianie ustawień w docelowej aplikacji sieci Web przy użyciu parametrów Web Deploy](#webdeployparms)
- [Sprawdzanie, czy aplikacja jest w trybie offline podczas wdrażania](#appoffline)
- [Wdrażanie bazy danych lub zmian w bazie danych w ramach wdrażania aplikacji sieci Web](#databasewithweb)
- [Wdrażanie bazy danych niezależnie od wdrożenia aplikacji sieci Web](#databaseseparate)
- [Wdrażanie aplikacji sieci Web, która korzysta z usług aplikacji ASP.NET, takich jak członkostwo i profilowanie](#aspnetmembership)
- [Prekompilowanie na potrzeby wdrożenia](#precompiling)
- [Wdrażanie intranetowej aplikacji sieci Web](#intranet)
- [Automatyzowanie typowych zadań wdrażania, które nie są zautomatyzowane z pola](#automating)
- [Konfigurowanie serwerów sieci Web tak, aby deweloperzy mogli wdrażać aplikacje sieci Web przy użyciu Web Deploy](#configuringservers)
- [Konfigurowanie serwerów dla dostawcy hostingu](#hostingprovider)
- [Rozwiązywanie problemów z wdrażaniem](#troubleshooting)
- [Uzyskiwanie pomocy dotyczącej określonego pytania wdrożenia](#gettinghelp)
- [Dodatkowe zasoby](#additional)

<a id="understanding"></a>

## <a name="understanding-deployment-options-for-web-projects"></a>Informacje o opcjach wdrażania dla projektów sieci Web

- [Omówienie wdrażania w sieci Web dla programu Visual Studio i ASP.NET](https://msdn.microsoft.com/library/dd394698.aspx) (MSDN).
- [Jak wdrożyć witrynę sieci Web systemu Windows Azure](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy). W tym artykule opisano opcje i linki do zasobów związanych z wdrażaniem projektów sieci Web w witrynach sieci Web systemu Windows Azure, w tym [ciągłego dostarczania](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) (zautomatyzowany z [kontroli źródła](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md)), jak również przy użyciu programu Visual Studio.
- [Ulepszenia publikowania w sieci Web w programie Visual Studio 2012](../visual-studio/overview/2012/visual-studio-2012-web-publishing-improvements.md) (wideo przez Scott Hanselman).
- [Przegląd dotyczący wdrażania w sieci Web w programie VS 2010](http://vishaljoshi.blogspot.com/2009/09/overview-post-for-web-deployment-in-vs.html) (blog Vishal Joshi). Starszy wpis w blogu, ale niektóre z zasobów programu Visual Studio 2010, do których prowadzi łącze, zawierają informacje, które są nadal istotne dla programu Visual Studio 2012.

<a id="findinghosting"></a>

## <a name="finding-hosting-providers-for-an-aspnet-application"></a>Znajdowanie dostawców hostingu dla aplikacji ASP.NET

- [Hosting ASP.NET](https://asp.net/hosting)

<a id="fromvs"></a>

## <a name="deploying-a-web-application-from-visual-studio"></a>Wdrażanie aplikacji sieci Web z programu Visual Studio

- [Jak wdrożyć witrynę sieci Web systemu Windows Azure](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy). Objaśnia opcje i zawiera linki do zasobów związanych z wdrażaniem projektów sieci Web w witrynach sieci Web systemu Windows Azure. Zawiera sekcję dotyczącą wdrażania z programu Visual Studio.
- [ASP.NET wdrażanie w sieci Web przy użyciu programu Visual Studio](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). Seria z 12-częściowym samouczkiem pokazuje, jak wdrażać aplikacje sieci Web za pomocą baz danych SQL Server. W przypadku wdrażania bazy danych jest stosowany zarówno dostawca dbDacFx, jak i migracje Code First platformy Entity Framework. Zawiera również informacje o [przekształceniach plików Web. config](../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md), [wdrażaniu poszczególnych plików](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md#specificfiles), [wdrożeniu wiersza polecenia](../web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment.md)i [sposobach dostosowywania potoku publikowania w sieci Web w programie Visual Studio przez edycję plików pubxml](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files.md). Dotyczy wszystkich ASP.NETych projektów sieci Web, w tym formularzy sieci Web, MVC i Web API.
- [Instrukcje: wdrażanie projektu sieci Web za pomocą jednego kliknięcia przycisk Publikuj w programie Visual Studio](https://msdn.microsoft.com/library/dd465337.aspx) (informacje referencyjne dla Kreatora publikacji w sieci Web w programie Visual Studio).
- [Wdrażanie SQL Server Compact aplikacji sieci Web ASP.NET przy użyciu programu Visual Studio](../web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12.md). Jest to wcześniejsza wersja **ASP.NET Web Deployment przy użyciu programu Visual Studio** w górnej części tej sekcji. Szczególnie przydatne teraz, aby uzyskać informacje na temat sposobu wdrażania baz danych SQL Server Compact i przeprowadzania migracji z SQL Server Compact do pełnej wersji SQL Server.
- [Wielowarstwowa aplikacja platformy .NET korzystająca z tabel, kolejek i obiektów blob magazynu](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36) (Microsoft Azure Site). Seria samouczków z 5 częściami pokazuje, jak utworzyć projekt MVC i wdrożyć go w usłudze w chmurze systemu Windows Azure.

<a id="package"></a>
## <a name="deploying-a-web-application-by-creating-and-installing-a-web-deployment-package"></a>Wdrażanie aplikacji sieci Web przez utworzenie i zainstalowanie pakietu wdrożeniowego sieci Web

- [Instrukcje: Tworzenie pakietu wdrożeniowego sieci Web w programie Visual Studio](https://msdn.microsoft.com/library/dd465323.aspx) (MSDN).
- [Instrukcje: Instalowanie pakietu wdrożeniowego przy użyciu pliku Deploy. cmd utworzonego w programie Visual Studio](https://msdn.microsoft.com/library/ff356104.aspx) (MSDN).
- [Użycie pakietu Web Deploy do wdrożenia w usługach IIS w polu dev i na potrzeby hosta innej firmy](http://sedodream.com/2011/11/08/UsingAWebDeployPackageToDeployToIISOnTheDevBoxAndToAThirdPartyHost.aspx) (blog Sayed Hashimi). Jak zainstalować pakiet wdrożeniowy w usługach IIS na komputerze lokalnym i w firmie hostingu, która obsługuje Menedżera usług IIS na potrzeby administracji zdalnej, za pomocą Menedżera usług IIS.
- [Kompilowanie pakietu Web Deploy z programu Visual Studio 2010](https://www.iis.net/learn/publish/using-web-deploy/building-a-web-deploy-package-from-visual-studio-2010) (witryna sieci Web IIS.NET). Zawiera instrukcje dotyczące tworzenia i instalowania pakietów w wierszu polecenia.
- [Pakiet po opublikowaniu w dowolnym miejscu](http://sedodream.com/2012/03/14/PackageWebUpdatedAndVideoBelow.aspx) (blog Sayed Hashimi). Wprowadza pakiet NuGet, który automatyzuje proces przekształcania pliku Web. config dla wielu środowisk docelowych, dzięki czemu można wdrożyć jeden pakiet na wielu serwerach. Zobacz również [wideo PackageWeb](https://www.youtube.com/watch?v=-LvUJFI8CzM) przez Sayed Hashimi.

Zobacz również następującą sekcję.

<a id="ci"></a>

## <a name="deploying-a-web-application-using-a-continuous-integration-ci-process"></a>Wdrażanie aplikacji sieci Web przy użyciu procesu ciągłej integracji (CI)

- [Ciągła integracja i ciągłe dostarczanie (Tworzenie aplikacji w chmurze w rzeczywistych warunkach w systemie Windows Azure).](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) Rozdział książki elektronicznej, który zapewnia ciągłą integrację i ciągłe dostarczanie.
- [Jak wdrożyć witrynę sieci Web systemu Windows Azure](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy). W tym artykule opisano opcje i linki do zasobów związanych z wdrażaniem projektów sieci Web w witrynach sieci Web systemu Windows Azure. Zawiera sekcję dotyczącą automatyzowania wdrożenia z kontroli źródła.
- [Wdrażanie aplikacji sieci Web w scenariuszach dla przedsiębiorstw](../web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). 40 — seria samouczków, pokazuje, jak zautomatyzować wdrażanie w procesie CI przy użyciu programu Visual Studio 2010 i Team Foundation Server 2010.
- [Wewnątrz Microsoft Build Engine: przy użyciu programu MSBuild i Team Foundation Build, Sayed Hashimi i William Bartholomew](http://msbuildbook.com). Jest to książka, a nie zasób sieci Web, ale jest to zasadniczy Przewodnik dotyczący sposobu konfigurowania programu MSBuild do scenariuszy ciągłej integracji.
- [Pakiet rozszerzeń programu MSBuild](https://github.com/mikefourie/MSBuildExtensionPack). Obejmuje zadania wdrażania.
- [Przewodnik dostosowywania Team Foundation Build](https://aka.ms/vsarsolutions). Dokumentacja według zakresów ALM na konfigurowaniu Team Foundation Server obejmuje wdrażanie w sieci Web oraz samouczki i filmy wideo.
- [SlowCheetah przekształcenia XML z serwera Ci](http://sedodream.com/2011/12/12/SlowCheetahXMLTransformsFromACIServer.aspx) (blog Sayed Hashimi). Wyjaśnia, jak używać programu SlowCheetah, dodatku Visual Studio do przekształcania pliku App. config i innych plików XML.

Zobacz również [, jak upewnić się, że aplikacja jest w trybie offline podczas wdrażania](aspnet-web-deployment-content-map.md#appoffline) w dalszej części tej strony.

<a id="transforms"></a>

## <a name="using-webconfig-transformations-to-change-settings-in-the-destination-webconfig-file-or-appconfig-file-during-deployment"></a>Za pomocą transformacji pliku Web. config zmień ustawienia w pliku docelowym Web. config lub plik App. config podczas wdrażania

- [Przekształcenia pliku Web. config](../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md).
- [Składnia transformacji Web. config dla wdrożenia projektu sieci Web przy użyciu programu Visual Studio](https://msdn.microsoft.com/library/dd465326.aspx) (MSDN).
- [Web Tools 2012,2 — przekształcenia Web. config](https://www.youtube.com/watch?v=HdPK8mxpKEI) (wideo w serwisie YouTube według Sayed Hashimi). Pokazuje, jak skonfigurować i wyświetlić podgląd transformacji Web. config.
- [Jak mogę wyłączyć transformację pliku Web. config?](https://msdn.microsoft.com/library/ee942158.aspx#disable_web_config_transformation) (MSDN).
- [Kiedy należy używać parametrów Web Deploy zamiast transformacji Web. config?](https://msdn.microsoft.com/library/ee942158.aspx#web_deploy_parameters) (MSDN).
- [XDT (transformacja dokumentów XML) wydano w CodePlex.com (Blog dotyczący](https://blogs.msdn.com/b/webdev/archive/2013/04/23/xdt-xml-document-transform-released-on-codeplex-com.aspx) programowania i projektowania sieci Web platformy .NET). Ogłasza dostępność kodu źródłowego dla aparatu transformacji plików Web. config i wyświetla listę narzędzi, które go używają.
- [Witryny sieci Web systemu Windows Azure: sposób działania ciągów aplikacji i parametrów połączenia](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx) (Microsoft Azure blog). Alternatywa dla programu Web. config jest przekształceń, jeśli Środowisko docelowe jest witryną sieci Web systemu Windows Azure i chcesz przekształcić `appSettings` lub `connectionStrings`.

<a id="webdeployparms"></a>

## <a name="using-web-deploy-parameters-to-change-settings-in-the-destination-web-application-during-deployment"></a>Zmienianie ustawień w docelowej aplikacji sieci Web przy użyciu parametrów Web Deploy

- [Instrukcje: Używanie parametrów Web Deploy w pakiecie wdrożeniowym sieci Web](https://msdn.microsoft.com/library/ff398068.aspx) (MSDN).
- [MSDeploy: jak zaktualizować ustawienia aplikacji przy publikowaniu w oparciu o profil publikowania](http://sedodream.com/2013/03/02/MSDeployHowToUpdateAppSettingsOnPublishBasedOnThePublishProfile.aspx) (blog Sayed Hashimi). Pokazuje, jak zintegrować parametry narzędzia Web Deploy z profilami publikacji programu Visual Studio.
- [Web Deploy parametryzacja](https://www.iis.net/learn/publish/using-web-deploy/web-deploy-parameterization) (witryna sieci Web IIS.NET).
- [Web Deploy parametryzacja w akcji](http://vishaljoshi.blogspot.com/2010/07/web-deploy-parameterization-in-action.html) (blog Vishal Joshi).
- [Web Deploy przekształcenie parametryzacja a Web. config](http://vishaljoshi.blogspot.com/2010/06/parameterization-vs-webconfig.html) (blog Vishal Joshi).
- [Witryny sieci Web systemu Windows Azure: sposób działania ciągów aplikacji i parametrów połączenia](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx) (Microsoft Azure blog). Alternatywą dla parametrów narzędzia Web Deploy, jeśli Środowisko docelowe jest witrynami sieci Web systemu Windows Azure i chcesz Sparametryzuj `appSettings` lub `connectionStrings`.

<a id="appoffline"></a>

## <a name="making-sure-an-application-is-off-line-during-deployment"></a>Sprawdzanie, czy aplikacja jest w trybie offline podczas wdrażania

- [ASP.NET wdrażanie w sieci Web przy użyciu programu Visual Studio: wdrażanie aktualizacji kodu](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md). Zobacz sekcję **przejmowanie aplikacji w tryb offline podczas wdrażania.**
- [Przełączanie aplikacji do trybu offline przed opublikowaniem](https://www.iis.net/learn/publish/deploying-application-packages/taking-an-application-offline-before-publishing) (witryna IIS.NET). Wyjaśnia funkcję wbudowaną w Web Deploy 3,0, która automatyzuje obsługę aplikacji\_pliku w trybie offline. htm. Ta funkcja nie działa z aplikacją niestandardową\_plików offline. htm.
- [Jak przełączyć aplikację sieci Web w tryb offline podczas publikowania](http://sedodream.com/2012/01/08/HowToTakeYourWebAppOfflineDuringPublishing.aspx) (blog Sayed Hashimi). Jak zautomatyzować proces używania aplikacji niestandardowej\_pliku w trybie offline. htm.
- [Aktualizacje publikacji w sieci Web dla aplikacji w trybie offline i usechecksum](https://blogs.msdn.com/b/webdev/archive/2013/10/30/web-publishing-updates-for-app-offline-and-usechecksum.aspx) (blog Microsoft Web Development). Inna opcja automatyzacji używania aplikacji\_pliku w trybie offline. htm.
- [Web Deploy 3,5 RTW](https://blogs.iis.net/msdeploy/archive/2013/07/09/webdeploy-3-5-rtw.aspx) (IIS.NET Site). Nowa funkcja w Web Deploy 3,5 dla aplikacji niestandardowej\_pliki w trybie offline. htm.

<a id="databasewithweb"></a>

## <a name="deploying-a-database-or-changes-to-a-database-as-part-of-web-application-deployment"></a>Wdrażanie bazy danych lub zmian w bazie danych w ramach wdrażania aplikacji sieci Web

- [Konfigurowanie wdrażania bazy danych w programie Visual Studio](https://msdn.microsoft.com/library/dd394698.aspx#dbdeployment) (MSDN). Omówienie opcji wdrażania bazy danych za pomocą projektu sieci Web.
- [ASP.NET wdrażanie w sieci Web przy użyciu programu Visual Studio](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). Seria z 12-częściowym samouczkiem zawiera instrukcje wdrażania bazy danych za pomocą dostawcy dbDacFx i migracje Code First platformy Entity Framework.
- [Instrukcje: wdrażanie projektu sieci Web za pomocą jednego kliknięcia przycisku Publikuj w programie Visual Studio](https://msdn.microsoft.com/library/dd465337.aspx) (MSDN).
- [Wdróż aplikację Secure ASP.NET MVC 5 z członkostwem, uwierzytelnianiem OAuth i SQL Database w witrynie sieci Web systemu Windows Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Długi samouczek, który kompiluje i wdraża aplikację, która korzysta z pojedynczej bazy danych SQL Server zarówno w przypadku członkostwa, jak i danych aplikacji.
- [Wdrażanie SQL Server Compact aplikacji sieci Web ASP.NET przy użyciu programu Visual Studio](../web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12.md). Seria 12-częściowego samouczka zawiera informacje na temat wdrażania SQL Server Compact baz danych oraz sposobu migrowania z SQL Server Compact do pełnej wersji SQL Server.

Zobacz również wdrażanie aplikacji sieci Web, tworząc i instalując pakiet wdrażania w sieci Web i wdrażając aplikację sieci Web przy użyciu procesu ciągłej integracji (CI) wcześniej na tej stronie.

<a id="databaseseparate"></a>

## <a name="deploying-a-database-separately-from-web-application-deployment"></a>Wdrażanie bazy danych niezależnie od wdrożenia aplikacji sieci Web

- [Narzędzia do SQL Server Data Tools](https://msdn.microsoft.com/library/hh272686(v=vs.103).aspx) (MSDN).
- [Dołączanie danych do projektu bazy danych SQL Server](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx) (SQL Server Blog zespołu narzędzi danych). Jak wdrożyć schemat i dane podczas wdrażania bazy danych.
- [Jak wdrożyć bazę danych w systemie Windows Azure](https://docs.microsoft.com/azure/sql-database/sql-database-cloud-migrate) (lokacja Microsoft Azure)
- [Migrowanie baz danych do systemu Windows Azure SQL Database (dawniej SQL Azure)](https://msdn.microsoft.com/library/windowsazure/ee730904.aspx) (MSDN).
- [Migrowanie bazy danych do platformy SQL Azure za pomocą SSDT](https://blogs.msdn.com/b/ssdt/archive/2012/04/19/migrating-a-database-to-sql-azure-using-ssdt.aspx) (blog zespołu narzędzi danych SQL Server Data Tools).
- [Migrowanie aplikacji zorientowanych na dane do systemu Windows Azure](https://msdn.microsoft.com/library/jj156154.aspx) (MSDN).
- [Migrowanie baz danych SQL Server do Azure SQL Database systemu Windows](https://msdn.microsoft.com/library/windowsazure/jj156160.aspx) (MSDN).

<a id="aspnetmembership"></a>

## <a name="deploying-a-web-application-that-uses-aspnet-application-services-such-as-membership-and-profiling"></a>Wdrażanie aplikacji sieci Web, która korzysta z usług aplikacji ASP.NET, takich jak członkostwo i profilowanie

- [Wdróż aplikację Secure ASP.NET MVC 5 z członkostwem, uwierzytelnianiem OAuth i SQL Database w witrynie sieci Web systemu Windows Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Długi samouczek, który kompiluje i wdraża aplikację, która korzysta z pojedynczej bazy danych SQL Server zarówno w przypadku członkostwa, jak i danych aplikacji.
- [ASP.NET Identity](https://asp.net/identity/). Zasoby dla ASP.NET Identity.
- [ASP.NET wdrażanie w sieci Web przy użyciu programu Visual Studio](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). Seria z 12-częściowym samouczkiem pokazuje, jak wdrożyć bazę danych członkostwa ASP.NET.
- [Konfigurowanie witryny sieci Web, która używa usługi aplikacji](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-a-website-that-uses-application-services-cs.md). W przypadku projektów witryny sieci Web, ale również dotyczy projektów aplikacji sieci Web.
- [Użytkownicy i role w produkcyjnej witrynie sieci Web](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-cs.md). W przypadku projektów witryny sieci Web, ale również dotyczy projektów aplikacji sieci Web.

<a id="precompiling"></a>

## <a name="precompiling-for-deployment"></a>Prekompilowanie na potrzeby wdrożenia

- [Przegląd prekompilowania projektu aplikacji sieci Web ASP.NET](https://msdn.microsoft.com/library/aa983464.aspx) (MSDN).
- [Karta Sieć Web pakowanie/publikowanie, właściwości projektu](https://msdn.microsoft.com/library/dd410108.aspx) (MSDN).
- [Zaawansowane ustawienia](https://msdn.microsoft.com/library/hh475319.aspx) wstępnej kompilacji — okno dialogowe (MSDN).

<a id="intranet"></a>

## <a name="deploying-an-intranet-web-application"></a>Wdrażanie intranetowej aplikacji sieci Web

- [Użyj opcji lokalnego uwierzytelniania organizacji (AD FS) z usługą ASP.NET w Visual Studio 2013](http://www.cloudidentity.com/blog/2014/02/12/use-the-on-premises-organizational-authentication-option-adfs-with-asp-net-in-visual-studio-2013/) (blog przez Vittorio Bertocci).
- [Jak utworzyć witrynę intranetową przy użyciu ASP.NET MVC](https://msdn.microsoft.com/library/gg703322(VS.98).aspx) (MSDN). Starsze wskazówki WRITEN dla programu Visual Studio 2010 nie odzwierciedlają głównych zmian w szablonach projektów intranetowych wprowadzonych w Visual Studio 2013.

<a id="automating"></a>

## <a name="automating-common-deployment-tasks-that-are-not-automated-out-of-the-box"></a>Automatyzowanie typowych zadań wdrażania, które nie są zautomatyzowane z pola

- [ASP.NET wdrażanie w sieci Web przy użyciu programu Visual Studio: wdrażanie dodatkowych plików](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files.md).
- [Ustawianie uprawnień do folderu w witrynie sieci Web publikowanie](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx) (blog Sayed Hashimi).
- [Jak rozłożyć plik targets w celu uwzględnienia ustawień rejestru dla pakietu projektu sieci Web](https://blogs.msdn.com/webdevtools/archive/2010/02/09/how-to-extend-target-file-to-include-registry-settings-for-web-project-package.aspx) (blog Web Development Tools).
- [Rozszerzanie transformacji XML (Web. config)](http://sedodream.com/2010/09/09/ExtendingXMLWebconfigConfigTransformation.aspx) (blog Sayed Hashimi). Pokazuje, jak utworzyć niestandardowe przekształcenia XDT.
- [Dostawca niestandardowy narzędzia do wdrażania w sieci Web (MSDeploy) zajmie 1](http://sedodream.com/2010/03/11/WebDeploymentToolMSDeployCustomProviderTake1.aspx) (blog Sayed Hashimi). Pokazuje, jak utworzyć niestandardowego dostawcę Web Deploy.
- [Jak spakować i wdrożyć składniki com](https://blogs.msdn.com/webdevtools/archive/2010/03/03/how-to-package-and-deploy-com-component.aspx) (Blog dotyczący narzędzi programistycznych dla sieci Web).
- [Jak spakować zestawy .NET](https://blogs.msdn.com/webdevtools/archive/2010/02/19/how-to-package-com-component.aspx) (blog narzędzi programistycznych dla sieci Web). Jak wdrażać zestawy w pamięci podręcznej GAC.
- Dodaj [skrypt do wszystkiego — zainicjuj maszynę wirtualną systemu Windows Azure dla serwera sieci Web za pomocą usług IIS, Web Deploy i innych rzeczy](http://www.tugberkugurlu.com/archive/script-out-everything-initialize-your-windows-azure-vm-for-your-web-server-with-iis-web-deploy-and-other-stuff) (blog Tugberk Ugurlu).

<a id="configuringservers"></a>

## <a name="configuring-web-servers-so-that-developers-can-deploy-web-applications-to-them-using-web-deploy"></a>Konfigurowanie serwerów sieci Web tak, aby deweloperzy mogli wdrażać aplikacje sieci Web przy użyciu Web Deploy

- [Instalowanie i konfigurowanie Web Deploy na potrzeby wdrożeń administratorów i niezwiązanych z administratorami](https://www.iis.net/learn/install/installing-publishing-technologies/installing-and-configuring-web-deploy) (lokacja IIS.NET).

<a id="hostingprovider"></a>

## <a name="configuring-servers-for-a-hosting-provider"></a>Konfigurowanie serwerów dla dostawcy hostingu

- [Przewodnik wdrażania hostingu Microsoft ASP.NET 4](https://go.microsoft.com/fwlink/?LinkId=191365) (centrum pobierania Microsoft).
- [Wygeneruj plik XML profilu](https://www.iis.net/learn/web-hosting/joining-the-web-hosting-gallery/generate-a-profile-xml-file) (witrynę IIS.NET).

<a id="troubleshooting"></a>

## <a name="troubleshooting-deployment-problems"></a>Rozwiązywanie problemów z wdrażaniem

- [Rozwiązywanie problemów z witrynami sieci Web systemu Windows Azure w programie Visual Studio](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio) (Microsoft Azure Site).
- [ASP.NET wdrażanie w sieci Web przy użyciu programu Visual Studio: Rozwiązywanie problemów](../web-forms/overview/deployment/visual-studio-web-deployment/troubleshooting.md).
- [Rozwiązywanie typowych problemów z Web Deploy](https://www.iis.net/learn/publish/troubleshooting-web-deploy/troubleshooting-common-problems-with-web-deploy).
- [Web Deploy kody błędów](https://www.iis.net/learn/publish/troubleshooting-web-deploy/web-deploy-error-codes) (witryna IIS.NET).
- [Wdrażanie w sieci Web — często zadawane pytania dotyczące programu Visual Studio i ASP.NET](https://msdn.microsoft.com/library/ee942158.aspx) (MSDN).
- [Podstawowe różnice między usługami IIS a serwerem deweloperskim ASP.NET](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md).
- [Typowe różnice konfiguracji między programowaniem i produkcją](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/common-configuration-differences-between-development-and-production-cs.md).
- [Hosting aplikacji ASP.NET w średnim zaufaniu](http://www.4guysfromrolla.com/articles/100307-1.aspx) (4 złoczyńców z witryny rolla).

<a id="gettinghelp"></a>

## <a name="getting-help-with-a-specific-deployment-question"></a>Uzyskiwanie pomocy dotyczącej określonego pytania wdrożenia

- [Forum dotyczące konfiguracji i wdrażania ASP.NET](https://forums.asp.net/26.aspx/1?Configuration and Deployment).
- [StackOverflow.com](http://www.StackOverflow.com).

<a id="additional"></a>

## <a name="additional-resources"></a>Dodatkowe materiały

Ta sekcja zawiera linki do dodatkowych zasobów, które są przydatne do uczenia się, jak korzystać z narzędzi wdrażania programu Visual Studio i usług IIS.

Następujące Blogi często zawierają informacje o wdrażaniu w sieci Web programu Visual Studio:

- [Narzędzia programistyczne dla sieci Web w blogu firmy Microsoft](https://blogs.msdn.com/b/webdevtools/).
- [Blog Sayed Hashimi](http://www.sedodream.com/).

Poniższe zasoby zawierają dokumentację dotyczącą Web Deploy, struktury usług IIS używanej przez program Visual Studio do wykonywania zadań wdrażania projektu aplikacji sieci Web. Pytania dotyczące Web Deploy można zadawać na [forum narzędzi do wdrażania w sieci](https://go.microsoft.com/fwlink/?LinkId=149411) Web w witrynie sieci Web IIS.NET.

- [Wprowadzenie do Web Deploy](https://www.iis.net/learn/publish/using-web-deploy/introduction-to-web-deploy).
- [Instalowanie i konfigurowanie Web Deploy](https://www.iis.net/learn/install/installing-publishing-technologies/installing-and-configuring-web-deploy).
- [Skrypty programu PowerShell służące do automatyzowania instalacji Web Deploy](https://www.iis.net/learn/publish/using-web-deploy/powershell-scripts-for-automating-web-deploy-setup).
- [Narzędzie Web Deployment](https://go.microsoft.com/fwlink/?LinkId=151481). Węzeł spisu treści najwyższego poziomu dla dokumentacji Web Deploy w witrynie TechNet. Zawiera przydatne informacje referencyjne, ale większość stron TechNet nie została zaktualizowana przez lata.
- [Przestrzeń nazw Microsoft. Web. Deployment](https://go.microsoft.com/fwlink/?LinkId=148630). Dokumentacja interfejsu API nie została zaktualizowana od wersji 1,0.
- [Blog zespołu wdrażania w sieci Web firmy Microsoft](https://blogs.iis.net/msdeploy/default.aspx).
- [Karta publikowanie w witrynie sieci web IIS.NET](https://www.iis.net/learn/publish).

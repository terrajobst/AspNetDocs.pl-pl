---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/troubleshooting-the-packaging-process
title: Rozwiązywanie problemów z procesem pakowania | Microsoft Docs
author: jrjlee
description: W tym temacie opisano, jak zbierać szczegółowe informacje o procesie pakowania przy użyciu właściwości EnablePackageProcessLoggingAndAssert w M...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 794bd819-00fc-47e2-876d-fc5d15e0de1c
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/troubleshooting-the-packaging-process
msc.type: authoredcontent
ms.openlocfilehash: 8ad649dfff085a8774cc13c11d8a3e3d48277d66
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78628213"
---
# <a name="troubleshooting-the-packaging-process"></a>Rozwiązywanie problemów z procesem tworzenia pakietów

Autor [Jason Lewandowski](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie opisano, jak zbierać szczegółowe informacje o procesie pakowania przy użyciu właściwości **EnablePackageProcessLoggingAndAssert** w Microsoft Build Engine (MSBuild).
> 
> Po ustawieniu właściwości **EnablePackageProcessLoggingAndAssert** na **true**, MSBuild będzie:
> 
> - Dodaj dodatkowe informacje o procesie pakowania do dzienników kompilacji.
> - Rejestruj błędy pod pewnymi warunkami, na przykład jeśli na liście pakowanie znajdują się zduplikowane pliki.
> - Utwórz katalog dziennika w folderze pakietu *ProjectName*\_i użyj go, aby zarejestrować informacje o plikach, które są pakowane.
> 
> Jeśli proces tworzenia pakietu zakończy się niepowodzeniem lub pakiety wdrażania sieci Web nie zawierają oczekiwanych plików, możesz użyć tych informacji do rozwiązywania problemów z procesem i Pinpoint, gdzie problemy są błędne.
> 
> > [!NOTE]
> > Właściwość **EnablePackageProcessLoggingAndAssert** działa tylko w przypadku kompilowania projektu przy użyciu konfiguracji **debugowania** . Właściwość zostanie zignorowana w innych konfiguracjach.

Ten temat stanowi część szeregu samouczków opartych na wymaganiach dotyczących wdrażania w przedsiębiorstwie fikcyjnej firmy o nazwie Fabrikam, Inc. W tej serii samouczków jest stosowane&#x2014;przykładowe rozwiązanie&#x2014;do reprezentowania aplikacji sieci Web, [które ma](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)realistyczny poziom złożoności, w tym aplikacji ASP.NET MVC 3, usługi Windows Communication Foundation (WCF) i projektu bazy danych.

Metoda wdrażania w tym samouczku jest oparta na rozłożeniu pliku projektu dzielonego opisanym w artykule [Omówienie pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), w którym proces kompilacji jest kontrolowany przez dwa pliki&#x2014;projektu, zawierający instrukcje kompilacji, które mają zastosowanie do każdego środowiska docelowego, oraz jeden zawierający ustawienia kompilacji i wdrożenia specyficznego dla środowiska. W czasie kompilacji plik projektu specyficzny dla środowiska jest scalany z plikiem projektu Environment-niezależny od w celu utworzenia kompletnego zestawu instrukcji kompilacji.

## <a name="understanding-the-enablepackageprocessloggingandassert-property"></a>Zrozumienie właściwości EnablePackageProcessLoggingAndAssert

[Kompilowanie i pakowanie projektów aplikacji sieci Web](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md) opisanych w tym, jak potok publikowania w sieci Web (WPP) zawiera zestaw obiektów docelowych programu MSBuild, które zwiększają funkcjonalność programu MSBuild i umożliwiają integrację z Internet Information Services (IIS) narzędzia do wdrażania w sieci Web (Web Deploy). Podczas pakowania projektu aplikacji sieci Web jest wywoływanie obiektów docelowych WPP.

Wiele z tych elementów docelowych WPP obejmuje logikę warunkową, która rejestruje dodatkowe informacje, gdy właściwość **EnablePackageProcessLoggingAndAssert** ma **wartość true**. Na przykład w przypadku przejrzenia obiektu docelowego **pakietu** można zobaczyć, że tworzy dodatkowy katalog dziennika i zapisuje listę plików do pliku tekstowego, jeśli **EnablePackageProcessLoggingAndAssert** jest równa **true**.

[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample1.xml)]

> [!NOTE]
> Elementy docelowe WPP są zdefiniowane w pliku *Microsoft. Web. Publishing. targets* w folderze% ProgramFiles (x86)% \ MSBuild\Microsoft\VisualStudio\v10.0\Web. Można otworzyć ten plik i przejrzeć elementy docelowe w programie Visual Studio 2010 lub dowolnym edytorze XML. Pamiętaj, aby nie modyfikować zawartości pliku.

## <a name="enabling-the-additional-logging"></a>Włączanie rejestrowania dodatkowego

Możesz podać wartość właściwości **EnablePackageProcessLoggingAndAssert** na różne sposoby, w zależności od sposobu kompilowania projektu.

Jeśli kompilujesz projekt z wiersza polecenia, możesz podać wartość właściwości **EnablePackageProcessLoggingAndAssert** jako argument wiersza polecenia:

[!code-console[Main](troubleshooting-the-packaging-process/samples/sample2.cmd)]

Jeśli używasz niestandardowego pliku projektu do kompilowania projektów, możesz uwzględnić wartość **EnablePackageProcessLoggingAndAssert** w atrybucie **Właściwości** zadania **MSBuild** :

[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample3.xml)]

Jeśli używasz definicji kompilacji Team Foundation Server (TFS) do kompilowania projektów, możesz podać wartość właściwości **EnablePackageProcessLoggingAndAssert** w wierszu **argumenty MSBuild** :![](troubleshooting-the-packaging-process/_static/image1.png)

> [!NOTE]
> Aby uzyskać więcej informacji na temat tworzenia i konfigurowania definicji kompilacji, zobacz [Tworzenie definicji kompilacji, która obsługuje wdrażanie](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).

Alternatywnie, jeśli chcesz uwzględnić pakiet w każdej kompilacji, możesz zmodyfikować plik projektu dla projektu aplikacji sieci Web, aby ustawić **wartość true**dla właściwości **EnablePackageProcessLoggingAndAssert** . Należy dodać właściwość do pierwszej **Właściwości** elementu w pliku. csproj lub. vbproj.

[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample4.xml)]

## <a name="reviewing-the-log-files"></a>Przeglądanie plików dziennika

Podczas kompilowania i pakowania projektu aplikacji sieci Web z **EnablePackageProcessLoggingAndAssert** ustawioną na **true**, MSBuild tworzy dodatkowy folder o nazwie log w folderze pakietu *ProjectName*\_. Folder dziennika zawiera różne pliki:

![](troubleshooting-the-packaging-process/_static/image2.png)

Lista wyświetlanych plików będzie różnić się w zależności od elementów projektu i procesu kompilacji. Jednak te pliki są zwykle używane do rejestrowania listy plików zbieranych przez WPP na potrzeby pakowania na różnych etapach procesu:

- Plik *PreExcludePipelineCollectFilesPhaseFileList. txt* zawiera listę plików zbieranych przez program MSBuild na potrzeby pakowania przed usunięciem plików określonych do wykluczenia.
- Plik *AfterExcludeFilesFilesList. txt* zawiera zmodyfikowaną listę plików po usunięciu plików określonych do wykluczenia.

    > [!NOTE]
    > Aby uzyskać więcej informacji na temat wykluczania plików i folderów z procesu tworzenia pakietów, zobacz [wykluczanie plików i folderów z wdrożenia](excluding-files-and-folders-from-deployment.md).
- Plik *AfterTransformWebConfig. txt* zawiera listę plików zebranych do spakowania po wykonaniu jakichkolwiek przekształceń *Web. config* . Na tej liście wszystkie pliki transformacji *Web. config* specyficzne dla konfiguracji, takie jak *Web. Debug. config* i *Web. release. config*, są wykluczone z listy plików do spakowania. W swoim miejscu znajduje się pojedynczy przekształcony plik *Web. config* .
- Plik *PostAutoParameterizationWebConfigConnectionStrings. txt* zawiera listę plików po wpisaniu parametrów połączenia w pliku *Web. config* . Jest to proces, który umożliwia zastępowanie parametrów połączenia z prawidłowymi ustawieniami środowiska docelowego podczas wdrażania pakietu.
- Plik *Package. txt* zawiera końcową listę plików, które mają zostać dołączone do pakietu.

> [!NOTE]
> Nazwy dodatkowych plików dziennika zwykle odpowiadają obiektom docelowym WPP. Możesz przejrzeć te cele, badając plik *Microsoft. Web. Publishing. targets* w folderze% ProgramFiles (x86)% \ MSBuild\Microsoft\VisualStudio\v10.0\Web.

Jeśli zawartość pakietu sieci Web nie jest oczekiwana, przejrzenie tych plików może być przydatnym sposobem na ustalenie, w jakim momencie proces wystąpił problem.

## <a name="conclusion"></a>Podsumowanie

W tym temacie opisano, jak można użyć właściwości **EnablePackageProcessLoggingAndAssert** w programie MSBuild do rozwiązywania problemów z procesem pakowania. Wyjaśniono różne sposoby, w których można podać wartość właściwości do procesu kompilacji i opisano dodatkowe informacje, które są rejestrowane podczas ustawiania właściwości na **wartość true**.

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat używania niestandardowych plików projektów programu MSBuild do kontrolowania procesu wdrażania, zobacz [Omówienie pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md) i [zrozumienie procesu kompilacji](../web-deployment-in-the-enterprise/understanding-the-build-process.md). Aby uzyskać więcej informacji na temat WPP i sposobu zarządzania procesem tworzenia pakietów, zobacz [Kompilowanie i pakowanie projektów aplikacji sieci Web](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md). Aby uzyskać informacje na temat wykluczania określonych plików i folderów z pakietów wdrażania w sieci Web, zobacz [wykluczanie plików i folderów z wdrożenia](excluding-files-and-folders-from-deployment.md).

> [!div class="step-by-step"]
> [Wstecz](running-windows-powershell-scripts-from-msbuild-project-files.md)

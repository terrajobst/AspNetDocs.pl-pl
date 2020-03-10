---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
title: 'Scenariusz: Konfigurowanie środowiska testowego na potrzeby wdrażania w sieci Web | Microsoft Docs'
author: jrjlee
description: W tym temacie opisano typowy scenariusz wdrażania w sieci Web dla środowisk deweloperskich i testowych oraz wyjaśniono zadania, które należy wykonać w celu skonfigurowania si...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 44a22ac7-1fc7-4174-b946-c6129fb6a19b
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: d580e550f2461837f0e8a4e477273348b49cb53e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78637817"
---
# <a name="scenario-configuring-a-test-environment-for-web-deployment"></a>Scenariusz: konfigurowanie środowiska testowego na potrzeby wdrażania w Internecie

Autor [Jason Lewandowski](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie opisano typowy scenariusz wdrażania sieci Web dla środowisk deweloperskich i testowych oraz wyjaśniono zadania, które należy wykonać w celu skonfigurowania podobnego środowiska.

Gdy deweloperzy pracują z aplikacjami sieci Web, często uzyskują dostęp do środowiska serwera, którego mogą używać do testowania zmian w aplikacjach w postaci realistycznych ustawień. Ten rodzaj środowiska programistycznego lub testowego ma zwykle następujące cechy:

- Środowisko składa się z jednego serwera sieci Web i jednego serwera bazy danych.
- Deweloperzy mają zazwyczaj uprawnienia administratora na serwerach, aby umożliwić im skonfigurowanie środowiska pod kątem wymagań aplikacji.
- Zmiany w aplikacjach są wdrażane regularnie, dlatego środowisko musi obsługiwać wdrożenie jednoetapowe lub zautomatyzowane.

Na przykład w naszym [scenariuszu samouczka](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md)otoczka hink jest deweloperem w firmie Fabrikam, Inc. matowy działa w rozwiązaniu Contact Manager i regularnie musi wdrożyć zmiany w środowisku testowym. Matowy jest administratorem na serwerze testowym sieci Web i testowanym serwerze bazy danych. Najpierw matowy musi być w stanie wdrożyć rozwiązanie bezpośrednio w środowisku testowym.

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image1.png)

W miarę jak postęp pracy i większa deweloperzy przyłączają się do zespołu, rozwiązanie Contact Manager jest skonfigurowane pod kątem ciągłej integracji (CI) w Team Foundation Server (TFS). Za każdym razem, gdy deweloper sprawdzi zawartość, Kompilacja zespołu powinna kompilować rozwiązanie, uruchamiać dowolne testy jednostkowe i automatycznie wdrażać rozwiązanie w środowisku testowym.

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image2.png)

## <a name="solution-overview"></a>Przegląd rozwiązania

Środowisko testowe musi obsługiwać jednoetapowy lub zautomatyzowany wdrożenia z komputera zdalnego, dzięki czemu można wybrać dwa główne podejścia. Możesz:

- Skonfiguruj testowy serwer sieci Web do obsługi wdrożenia za pomocą usługi Deployment Agent sieci Web ("Agent zdalny").
- Skonfiguruj testowy serwer sieci Web do obsługi wdrożenia przy użyciu programu obsługi Web Deploy.

> [!NOTE]
> Można również użyć [Web Deploy na żądanie](https://technet.microsoft.com/library/ee517345(WS.10).aspx) ("tymczasowy Agent"). Jest to podobne do podejścia zdalnego agenta w zakresie wymagań i ograniczeń.

W takim przypadku deweloperzy mają uprawnienia administratora na serwerach docelowych, a środowisko testowe nie podlega ścisłym ograniczeniom zabezpieczeń, więc logiczną opcją jest skonfigurowanie testowego serwera sieci Web do obsługi wdrożenia za pomocą zdalnego agenta. Jest to mniej skomplikowane i wymaga mniejszej konfiguracji początkowej niż metoda obsługi Web Deploy. Należy również skonfigurować serwer bazy danych do obsługi dostępu zdalnego i wdrożenia.

Te tematy zawierają wszystkie informacje potrzebne do wykonania następujących zadań:

- [Skonfiguruj serwer sieci Web do publikowania Web Deploy (Agent zdalny)](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md). W tym temacie opisano sposób tworzenia serwera sieci Web, który obsługuje publikowanie Web Deploy przy użyciu metody zdalnego agenta, rozpoczynając od czystej kompilacji systemu Windows Server 2008 R2.
- [Skonfiguruj serwer bazy danych na potrzeby publikowania Web Deploy](configuring-a-database-server-for-web-deploy-publishing.md). W tym temacie opisano sposób konfigurowania serwera bazy danych do obsługi dostępu zdalnego i wdrażania, rozpoczynając od domyślnej instalacji SQL Server 2008 R2.

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać wskazówki dotyczące konfigurowania typowego środowiska przejściowego, zobacz [Scenariusz: Konfigurowanie środowiska przejściowego na potrzeby wdrażania w sieci Web](scenario-configuring-a-staging-environment-for-web-deployment.md). Aby uzyskać wskazówki dotyczące konfigurowania typowego środowiska produkcyjnego, zobacz [Scenariusz: Konfigurowanie środowiska produkcyjnego na potrzeby wdrażania w sieci Web](scenario-configuring-a-production-environment-for-web-deployment.md).

> [!div class="step-by-step"]
> [Poprzednie](choosing-the-right-approach-to-web-deployment.md)
> [dalej](scenario-configuring-a-staging-environment-for-web-deployment.md)

---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment
title: Konfigurowanie środowisk serwera do wdrożenia w sieci Web | Microsoft Docs
author: jrjlee
description: W tym samouczku przedstawiono sposób konfigurowania środowisk serwera do obsługi jednego kliknięcia, automatycznego wdrożenia witryny sieci Web i publikowania ich w różnych scen...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 0bf0959b-4ca8-45de-bd13-b15347543b5a
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 073161ce4faa3b7ba6749d7dfbaa5309eeca4f74
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78634471"
---
# <a name="configuring-server-environments-for-web-deployment"></a>Konfigurowanie środowisk serwera na potrzeby wdrażania w Internecie

Autor [Jason Lewandowski](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym samouczku przedstawiono sposób konfigurowania środowisk serwera do obsługi jednego kliknięcia lub automatycznego wdrożenia witryny sieci Web i publikowania w różnych scenariuszach. Ten samouczek zawiera tematy, które przeprowadzą Cię przez proces wykonywania różnych zadań, takich jak Konfigurowanie serwera sieci Web do obsługi określonych metod wdrażania i konfigurowania farmy serwerów WFF (Web Farm Framework), wraz z omówieniami opartymi na scenariuszu, które zapewniają kompleksowe wskazówki na temat wyższego poziomu.
> 
> W samouczku jest używany scenariusz wdrażania firmy Fabrikam, Inc. opisany w artykule [wdrażanie w przedsiębiorstwie sieci Web: Omówienie scenariusza](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md) jako punkt odniesienia dla przykładów i infrastruktury sieciowej.
> 
> Aby uzyskać włoskie tłumaczenie tych samouczków, odwiedź stronę [http://www.lucamorelli.it](http://www.lucamorelli.it).

Ten samouczek zawiera następujące tematy:

- [Wybieranie właściwego podejścia do wdrażania w Internecie](choosing-the-right-approach-to-web-deployment.md)
- [Scenariusz: Konfigurowanie środowiska testowego na potrzeby wdrażania w Internecie](scenario-configuring-a-test-environment-for-web-deployment.md)
- [Scenariusz: Konfigurowanie środowiska przejściowego na potrzeby wdrażania w Internecie](scenario-configuring-a-staging-environment-for-web-deployment.md)
- [Scenariusz: Konfigurowanie środowiska produkcyjnego na potrzeby wdrażania w Internecie](scenario-configuring-a-production-environment-for-web-deployment.md)
- [Konfigurowanie serwera sieci Web dla usługi publikowania Web Deploy (agent zdalny)](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)
- [Konfigurowanie serwera sieci Web dla usługi publikowania Web Deploy (program obsługi narzędzia Web Deploy)](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)
- [Konfigurowanie serwera sieci Web dla usługi publikowania Web Deploy (wdrożenie w trybie offline)](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)
- [Konfigurowanie serwera bazy danych dla usługi publikowania Web Deploy](configuring-a-database-server-for-web-deploy-publishing.md)
- [Tworzenie farmy serwerów za pomocą rozwiązania Web Farm Framework](creating-a-server-farm-with-the-web-farm-framework.md)
- [Konfigurowanie właściwości wdrożenia dla środowiska docelowego](configuring-deployment-properties-for-a-target-environment.md)

Pierwszy temat, który [wybiera odpowiednie podejście do wdrażania w sieci Web](choosing-the-right-approach-to-web-deployment.md), opisuje główne podejścia, których można użyć do publikowania aplikacji sieci Web za pomocą narzędzia do wdrażania w sieci Web Internet Information Services (Web Deploy) 2,0. Identyfikuje także scenariusze, które są mapowane na każde podejście. W tym miejscu każdy temat scenariusza zawiera ogólne omówienie zadań, które należy wykonać, i identyfikuje tematy, które będą potrzebne do wykonania tych zadań.

Jeśli używasz podejścia do podzielonego pliku projektu opisanego w artykule [Opis procesu kompilacji](../web-deployment-in-the-enterprise/understanding-the-build-process.md) do kompilowania i wdrażania rozwiązania, końcowy temat, [Konfigurowanie właściwości wdrożenia dla środowiska docelowego](configuring-deployment-properties-for-a-target-environment.md), zawiera opis sposobu konfigurowania plików projektu specyficznych dla środowiska dla wdrożenia w różnych środowiskach docelowych.

## <a name="key-technologies"></a>Najważniejsze technologie

Ten samouczek koncentruje się na sposobach używania tych produktów i technologii do obsługi wdrażania w sieci Web:

- IIS 7.5
- Web Deploy 2. x
- WFF 2.x
- Usługa zarządzania siecią Web (WMSvc) usług IIS

Samouczek ten jest również używany w systemie Windows Server 2008 R2, SQL Server 2008 R2, ASP.NET 4,0 i ASP.NET MVC 3.

## <a name="other-tutorials-in-this-series"></a>Inne samouczki w tej serii

Jest to część serii pięciu samouczków dotyczących wdrażania w sieci Web w skali korporacyjnej. Oto inne samouczki z serii:

- [Wdrażanie aplikacji sieci Web w scenariuszach dla przedsiębiorstw](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Ta zawartość wprowadzająca zawiera kontekstowe tło dla serii samouczków. Opisano w nim Scenariusz samouczka, który ilustruje sposób, w jaki zadania i instruktaże opisane w całej serii mieszczą się w szerszym procesie zarządzania cyklem życia aplikacji (ALM).
- [Wdrażanie w sieci Web w przedsiębiorstwie](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Ten samouczek zawiera ogólne wprowadzenie do plików projektu programu Microsoft Build Engine (MSBuild), potoku publikowania w sieci Web, Web Deploy i innych powiązanych technologii. Wyjaśniono, jak można używać tych narzędzi razem do zarządzania złożonymi procesami wdrażania.
- [Konfigurowanie Team Foundation Server wdrażania w sieci Web](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). W tym samouczku opisano sposób konfigurowania Team Foundation Server (TFS) w celu obsługi różnych scenariuszy wdrażania, w tym automatycznego wdrażania w ramach procesu integracji ciągłej (CI) i ręcznie wyzwalanych wdrożeń określonych kompilacji.
- [Zaawansowane wdrażanie w sieci Web dla przedsiębiorstw](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). W tym samouczku opisano, jak wykonać różne bardziej zaawansowane zadania wdrażania, takie jak Dostosowywanie wdrożeń baz danych dla wielu środowisk, wykluczanie plików i folderów z wdrożenia oraz pobieranie aplikacji sieci Web w trybie offline podczas procesu wdrażania .

> [!div class="step-by-step"]
> [Dalej](choosing-the-right-approach-to-web-deployment.md)

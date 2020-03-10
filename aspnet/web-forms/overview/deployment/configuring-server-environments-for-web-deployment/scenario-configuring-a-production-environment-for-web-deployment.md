---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment
title: 'Scenariusz: Konfigurowanie środowiska produkcyjnego na potrzeby wdrażania w sieci Web | Microsoft Docs'
author: jrjlee
description: W tym temacie opisano typowy scenariusz wdrażania w sieci Web dla środowiska produkcyjnego i wyjaśniono zadania, które należy wykonać w celu skonfigurowania podobnego...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 2e861511-450e-4752-a61e-4a01933f9b6e
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 2d76e715cdbf6ec484fa0ff98b3b3d1d8dfd3961
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78635423"
---
# <a name="scenario-configuring-a-production-environment-for-web-deployment"></a>Scenariusz: konfigurowanie środowiska produkcyjnego na potrzeby wdrażania w Internecie

Autor [Jason Lewandowski](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie opisano typowy scenariusz wdrażania w sieci Web dla środowiska produkcyjnego i wyjaśniono zadania, które należy wykonać w celu skonfigurowania podobnego środowiska.

Środowisko produkcyjne jest ostatnim miejscem docelowym aplikacji sieci Web lub witryny internetowej. W tym momencie aplikacja została przetestowana, została wdrożona w środowisku przejściowym i jest gotowa do użycia. Cechy środowiska produkcyjnego mogą się znacznie różnić w zależności od rodzaju i przeznaczenia zawartości sieci Web, rozmiaru organizacji, odbiorców docelowych i wielu innych czynników. W scenariuszu w skali przedsiębiorstwa środowisko produkcyjne może mieć następujące cechy:

- Środowisko składa się z wielu serwerów sieci Web o zrównoważonym obciążeniu i jednego lub większej liczby serwerów baz danych, często w przypadku klastrów trybu failover i dublowania baz danych.
- Jeśli środowisko jest dostępne z Internetu, prawdopodobnie zostanie rozdzielone z sieci wewnętrznej. Może ona znajdować się w innej podsieci w sieci obwodowej, może znajdować się w innej domenie i może znajdować się w całkowicie innej infrastrukturze sieci.
- Konta deweloperów i procesów serwera kompilacji bardzo mało prawdopodobne, aby mieć uprawnienia administratora na serwerach produkcyjnych.
- Zmiany w aplikacjach są wdrażane rzadziej niż wdrożenia testowe lub przejściowe.

> [!NOTE]
> Skalowanie wdrożenia bazy danych na wielu serwerach wykracza poza zakres tego samouczka. Aby uzyskać więcej informacji na temat tego obszaru, zapoznaj się z tematem [SQL Server Books Online](https://technet.microsoft.com/library/ms130214.aspx).

Na przykład w naszym [scenariuszu samouczka](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md)serwer kompilacji zespołu zawiera definicje kompilacji, które umożliwiają użytkownikom tworzenie rozwiązania Contact Manager i wdrażanie go w środowisku przejściowym w jednym kroku. Gdy aplikacja jest gotowa do wdrożenia w środowisku produkcyjnym ze względu na ograniczenia wynikające z wymagań dotyczących zabezpieczeń i infrastruktury sieciowej, administrator środowiska produkcyjnego musi ręcznie skopiować pakiet sieci Web na produkcyjny serwer sieci Web i zaimportować za poorednictwem Menedżera Internet Information Services (IIS).

![](scenario-configuring-a-production-environment-for-web-deployment/_static/image1.png)

## <a name="solution-overview"></a>Przegląd rozwiązania

W tym scenariuszu można wywnioskować te fakty z analizy wymagań dotyczących wdrożenia:

- Ze względu na ograniczenia zabezpieczeń i konfigurację sieci nie można skonfigurować środowiska produkcyjnego do obsługi jednego kliknięcia lub automatycznego wdrożenia. Wdrożenie w trybie offline jest jedynym możliwym rozwiązaniem w tym scenariuszu.
- Środowisko produkcyjne obejmuje wiele serwerów sieci Web, dzięki czemu można utworzyć farmę serwerów za pomocą struktury sieci Web WFF. Korzystając z tego podejścia, administrator musi jedynie zaimportować aplikację na jeden serwer sieci Web (serwer podstawowy), a WFF będzie replikować wdrożenie na wszystkich pozostałych serwerach sieci Web w środowisku produkcyjnym.

Te tematy zawierają wszystkie informacje potrzebne do wykonania następujących zadań:

- [Utwórz farmę serwerów za pomocą struktury farmy sieci Web](configuring-a-database-server-for-web-deploy-publishing.md). W tym temacie opisano sposób tworzenia i konfigurowania farmy serwerów przy użyciu programu WFF, dzięki czemu produkty i składniki platformy sieci Web, ustawienia konfiguracji i witryny sieci Web i aplikacje są replikowane na wielu serwerach internetowych o zrównoważonym obciążeniu.
- [Skonfiguruj serwer sieci Web na potrzeby publikowania Web Deploy (Wdrażanie w trybie offline)](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md). W tym temacie opisano sposób tworzenia serwera sieci Web, który umożliwia administratorom Ręczne importowanie i wdrażanie pakietów sieci Web, począwszy od czystej kompilacji systemu Windows Server 2008 R2.
- [Skonfiguruj serwer bazy danych na potrzeby publikowania Web Deploy](configuring-a-database-server-for-web-deploy-publishing.md). W tym temacie opisano sposób konfigurowania serwera bazy danych do obsługi dostępu zdalnego i wdrażania, rozpoczynając od domyślnej instalacji SQL Server 2008 R2.

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać wskazówki dotyczące konfigurowania typowego środowiska testowego dla deweloperów, zobacz [Scenariusz: Konfigurowanie środowiska testowego na potrzeby wdrażania w sieci Web](scenario-configuring-a-test-environment-for-web-deployment.md). Aby uzyskać wskazówki dotyczące konfigurowania typowego środowiska przejściowego, zobacz [Scenariusz: Konfigurowanie środowiska przejściowego na potrzeby wdrażania w sieci Web](scenario-configuring-a-staging-environment-for-web-deployment.md).

> [!div class="step-by-step"]
> [Poprzednie](scenario-configuring-a-staging-environment-for-web-deployment.md)
> [dalej](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)

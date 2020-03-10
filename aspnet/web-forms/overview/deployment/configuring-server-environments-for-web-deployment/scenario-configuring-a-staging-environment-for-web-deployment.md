---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment
title: 'Scenariusz: Konfigurowanie środowiska przejściowego na potrzeby wdrażania w sieci Web | Microsoft Docs'
author: jrjlee
description: W tym temacie opisano typowy scenariusz wdrażania sieci Web dla środowiska tymczasowego i wyjaśniono zadania, które należy wykonać w celu skonfigurowania podobnej ENV...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 5a8e49b7-5317-4125-b107-7e2466b47bb3
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: eaa61ca850817f8dd98955b59e94be93389bf256
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78637845"
---
# <a name="scenario-configuring-a-staging-environment-for-web-deployment"></a>Scenariusz: konfigurowanie środowiska przejściowego na potrzeby wdrażania w Internecie

Autor [Jason Lewandowski](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie opisano typowy scenariusz wdrażania sieci Web dla środowiska przejściowego i wyjaśniono zadania, które należy wykonać w celu skonfigurowania podobnego środowiska.

Wiele organizacji używa środowisk przejściowych do wyświetlania podglądu aktualizacji aplikacji lub witryn sieci Web. Dzięki temu osoby w organizacji mogą eksplorować i przeglądać nowe funkcje lub zawartość, zanim witryna "przejdzie na żywo" lub innymi słowy jest wdrażana w środowisku produkcyjnym. Środowisko przejściowe jest przeznaczone do replikowania środowiska produkcyjnego tak blisko, jak to możliwe, w celu zapewnienia realistycznej wersji zapoznawczej. Ten rodzaj środowiska przejściowego ma zwykle następujące cechy:

- Środowisko składa się z wielu serwerów sieci Web o zrównoważonym obciążeniu i jednego lub większej liczby serwerów baz danych, często w przypadku klastrów trybu failover i dublowania baz danych.
- Aplikacje mogą być wdrażane ręcznie przez zespół programistyczny lub automatycznie przez serwer kompilacji zespołu.
- Konta użytkowników lub procesów, które wdrażają aplikacje, prawdopodobnie nie będą mieć uprawnień administratora na serwerach przejściowych.
- Zmiany w aplikacjach są wdrażane regularnie, dlatego środowisko musi obsługiwać wdrożenie jednoetapowe lub zautomatyzowane.

> [!NOTE]
> Skalowanie wdrożenia bazy danych na wielu serwerach wykracza poza zakres tego samouczka. Aby uzyskać więcej informacji na temat tego obszaru, zapoznaj się z tematem [SQL Server Books Online](https://technet.microsoft.com/library/ms130214.aspx).

Na przykład w naszym [scenariuszu samouczka](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md)Team Foundation Server (TFS) zarządza rozwiązaniem Contact Manager. Administrator TFS, Rob Walters, utworzył definicję kompilacji, która umożliwia deweloperom wyzwalanie wdrożenia w środowisku przejściowym zgodnie z wymaganiami.

![](scenario-configuring-a-staging-environment-for-web-deployment/_static/image1.png)

Należy pamiętać, że w większości przypadków nie trzeba wdrażać najnowszej kompilacji w środowisku przejściowym. Zamiast tego bardzo dużo łatwiej jest wdrożyć konkretną kompilację, która została już zweryfikowana i zweryfikowana w środowisku testowym.

## <a name="solution-overview"></a>Przegląd rozwiązania

W tym scenariuszu można wywnioskować te fakty z analizy wymagań dotyczących wdrożenia:

- Konto użytkownika lub procesu wykonujące wdrożenie nie ma uprawnień administratora na serwerach przejściowych, więc tymczasowe serwery sieci Web muszą obsługiwać wdrożenie niebędące administratorami. W związku z tym należy skonfigurować tymczasowe serwery sieci Web do korzystania z programu obsługi Web Deploy, a nie agenta zdalnego.
- Środowisko przejściowe obejmuje wiele serwerów sieci Web, ale wymaga obsługi jednego kliknięcia lub zautomatyzowanego wdrożenia, aby utworzyć farmę serwerów za pomocą struktury farmy sieci Web (WFF). Korzystając z tej metody, można wdrożyć aplikację na jednym serwerze sieci Web (serwerze podstawowym), a WFF będzie replikować wdrożenie na wszystkich pozostałych serwerach sieci Web w środowisku przejściowym.
- Konto użytkownika lub procesu wykonujące wdrożenie musi mieć uprawnienia do tworzenia baz danych. W związku z tym należy dodać konto do roli serwera **dbcreator** na serwerze bazy danych, a także skonfigurować serwer bazy danych do obsługi dostępu zdalnego i wdrożenia.

Te tematy zawierają wszystkie informacje potrzebne do wykonania następujących zadań:

- [Utwórz farmę serwerów za pomocą struktury farmy sieci Web](creating-a-server-farm-with-the-web-farm-framework.md). W tym temacie opisano sposób tworzenia i konfigurowania farmy serwerów przy użyciu programu WFF, dzięki czemu produkty i składniki platformy sieci Web, ustawienia konfiguracji i witryny sieci Web i aplikacje są replikowane na wielu serwerach internetowych o zrównoważonym obciążeniu.
- [Skonfiguruj serwer sieci Web na potrzeby publikowania Web Deploy (obsługa Web Deploy)](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md). W tym temacie opisano sposób tworzenia serwera sieci Web, który obsługuje publikowanie Web Deploy przy użyciu metody zdalnego agenta, rozpoczynając od czystej kompilacji systemu Windows Server 2008 R2.
- [Skonfiguruj serwer bazy danych na potrzeby publikowania Web Deploy](configuring-a-database-server-for-web-deploy-publishing.md). W tym temacie opisano sposób konfigurowania serwera bazy danych do obsługi dostępu zdalnego i wdrażania, rozpoczynając od domyślnej instalacji SQL Server 2008 R2.

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać wskazówki dotyczące konfigurowania typowego środowiska testowego dla deweloperów, zobacz [Scenariusz: Konfigurowanie środowiska testowego na potrzeby wdrażania w sieci Web](scenario-configuring-a-test-environment-for-web-deployment.md). Aby uzyskać wskazówki dotyczące konfigurowania typowego środowiska produkcyjnego, zobacz [Scenariusz: Konfigurowanie środowiska produkcyjnego na potrzeby wdrażania w sieci Web](scenario-configuring-a-production-environment-for-web-deployment.md).

> [!div class="step-by-step"]
> [Poprzednie](scenario-configuring-a-test-environment-for-web-deployment.md)
> [dalej](scenario-configuring-a-production-environment-for-web-deployment.md)

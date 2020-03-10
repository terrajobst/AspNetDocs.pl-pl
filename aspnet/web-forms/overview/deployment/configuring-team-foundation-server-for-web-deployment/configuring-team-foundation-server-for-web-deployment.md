---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment
title: Konfigurowanie Team Foundation Server wdrożenia w sieci Web | Microsoft Docs
author: jrjlee
description: W tym samouczku przedstawiono sposób konfigurowania Team Foundation Server (TFS) 2010 do kompilowania rozwiązań i wdrażania zawartości sieci Web w różnych środowiskach docelowych. To...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: ff55233a-e795-4007-a4fc-861fe1bb590b
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 638d696abbc5f05957c0ed2eb7ebb65fce7813ea
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78528393"
---
# <a name="configuring-team-foundation-server-for-web-deployment"></a>Konfigurowanie serwera Team Foundation Server na potrzeby wdrażania w Internecie

Autor [Jason Lewandowski](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym samouczku przedstawiono sposób konfigurowania Team Foundation Server (TFS) 2010 do kompilowania rozwiązań i wdrażania zawartości sieci Web w różnych środowiskach docelowych. Obejmuje to scenariusze ciągłej integracji (CI), w przypadku których zawartość jest wdrażana automatycznie za każdym razem, gdy deweloper wprowadza zmianę. Może również zawierać scenariusze wyzwalania ręcznego, w którym administrator może chcieć wyzwolić wdrożenie określonej kompilacji w środowisku przejściowym, gdy kompilacja została zweryfikowana i zweryfikowana w środowisku testowym. Tematy w tym samouczku przeprowadzą Cię przez cały proces konfiguracji, w tym:
> 
> - Jak utworzyć nowy projekt zespołowy w programie TFS.
> - Jak dodać zawartość do kontroli źródła.
> - Jak skonfigurować serwer kompilacji do obsługi elementu konfiguracji i wdrożenia.
> - Jak utworzyć definicję kompilacji, która zawiera logikę wdrażania.
> - Jak skonfigurować uprawnienia do automatycznego wdrażania.
> 
> Aby uzyskać włoskie tłumaczenie tych samouczków, odwiedź stronę [http://www.lucamorelli.it](http://www.lucamorelli.it).

W tym samouczku przyjęto założenie, że zainstalowano TFS 2010 i utworzono kolekcję projektu zespołowego w ramach procesu konfiguracji początkowej. [Przewodnik instalacji programu Team Foundation dla programu Visual Studio 2010](https://go.microsoft.com/?linkid=9805132) zawiera kompleksowe wskazówki dotyczące tych zadań.

## <a name="context"></a>Kontekst

Ta część szeregu samouczków opiera się na wymaganiach dotyczących wdrażania w przedsiębiorstwie fikcyjnej firmy o nazwie Fabrikam, Inc. W tej serii samouczków jest stosowane&#x2014;przykładowe [](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) rozwiązanie&#x2014;do reprezentowania aplikacji sieci Web, które ma realistyczny poziom złożoności, w tym aplikacji ASP.NET MVC 3, usługi Windows Communication Foundation (WCF) i projektu bazy danych.

Metoda wdrażania w tym samouczku jest oparta na podejściu do pliku projektu podzielonego opisanego w artykule [Opis procesu kompilacji](../web-deployment-in-the-enterprise/understanding-the-build-process.md), w którym proces kompilacji jest kontrolowany przez dwa pliki&#x2014;projektu, zawierający instrukcje kompilacji, które mają zastosowanie do każdego środowiska docelowego, oraz jeden zawierający ustawienia kompilacji i wdrożenia specyficznego dla środowiska. W czasie kompilacji plik projektu specyficzny dla środowiska jest scalany z plikiem projektu Environment-niezależny od w celu utworzenia kompletnego zestawu instrukcji kompilacji.

## <a name="scenario-overview"></a>omówienie scenariusza

Scenariusz wysokiego poziomu dla tych samouczków został opisany w artykule [wdrażanie w sieci Web dla przedsiębiorstw: Omówienie scenariusza](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md). Zalecamy zapoznanie się z tym tematem przed rozpoczęciem pracy z tym samouczkiem.

## <a name="how-to-use-this-tutorial"></a>Jak korzystać z tego samouczka

Jeśli po raz pierwszy wykonano zadania opisane w tym samouczku, lub jeśli chcesz postępować zgodnie z przykładami przy użyciu przykładowego rozwiązania, musisz przejść przez tematy dotyczące samouczka w kolejności. Alternatywnie możesz użyć poszczególnych tematów jako wskazówek dotyczących konkretnych zadań. Ten samouczek zawiera następujące tematy:

- [Tworzenie projektu zespołowego w programie TFS](creating-a-team-project-in-tfs.md). Projekt zespołowy jest jednostką podstawową dla kontroli źródła, zarządzania procesami i kompilowania w programie TFS. Należy utworzyć projekt zespołowy, aby można było dodać zawartość do kontroli źródła lub utworzyć definicje kompilacji.
- [Dodawanie zawartości do kontroli źródła](adding-content-to-source-control.md). Po utworzeniu projektu zespołowego możesz rozpocząć dodawanie zawartości do kontroli źródła. Przed rozpoczęciem konfigurowania kompilacji należy dodać swoje projekty i rozwiązania wraz ze wszystkimi zależnościami zewnętrznymi.
- [Konfigurowanie serwera kompilacji TFS na potrzeby wdrażania w sieci Web](configuring-a-tfs-build-server-for-web-deployment.md). Jeśli chcesz skompilować zawartość projektu zespołowego, musisz skonfigurować serwer kompilacji. W większości przypadków powinna to być na oddzielnym komputerze od instalacji programu TFS. Aby skonfigurować serwer kompilacji, należy zainstalować i skonfigurować usługę kompilacji TFS, zainstalować program Visual Studio 2010, utworzyć kontrolery kompilacji i agentów kompilacji, zainstalować wszelkie produkty lub składniki wymagane przez kod w celu pomyślnego skompilowania, a następnie zainstalować Internet Information Services (IIS) narzędzie do wdrażania w sieci Web (Web Deploy).
- [Tworzenie definicji kompilacji, która obsługuje wdrażanie](creating-a-build-definition-that-supports-deployment.md). Aby można było uruchomić kolejkowanie lub wyzwolić kompilacje w programie TFS, należy utworzyć co najmniej jedną definicję kompilacji dla projektu zespołowego. Definicja kompilacji definiuje każdy aspekt kompilacji, w tym elementy, które powinny być zawarte w kompilacji, co powinno wyzwolić kompilację, a Kompilacja zespołu powinna wysyłać dane wyjściowe kompilacji. Można skonfigurować definicję kompilacji do uruchamiania plików projektu niestandardowego Microsoft Build Engine (MSBuild), które umożliwiają uwzględnienie logiki wdrażania w zautomatyzowanych kompilacjach.
- [Wdrażanie określonej kompilacji](deploying-a-specific-build.md). W wielu scenariuszach warto wdrożyć konkretną kompilację, a nie najnowszą kompilację, w środowisku docelowym. W takim przypadku można skonfigurować definicję kompilacji, która wdraża zawartość z określonego folderu docelowego.
- [Konfigurowanie uprawnień do wdrożenia kompilacji zespołowej](configuring-permissions-for-team-build-deployment.md). Jeśli usługa kompilacji ma wdrażać zawartość w ramach zautomatyzowanego procesu kompilacji, należy przyznać różne uprawnienia do konta usługi kompilacji na wszystkich docelowych serwerach sieci Web i serwerach baz danych.

## <a name="key-technologies"></a>Najważniejsze technologie

Ten samouczek koncentruje się na sposobach używania tych produktów i technologii do obsługi zautomatyzowanej kompilacji i wdrażania w sieci Web:

- Visual Studio Team Foundation Server 2010
- Kompilacja zespołu i MSBuild
- Web Deploy

Samouczek ten jest również używany w systemie Windows Server 2008 R2, IIS 7,5, SQL Server 2008 R2, ASP.NET 4,0 i ASP.NET MVC 3.

## <a name="other-tutorials-in-this-series"></a>Inne samouczki w tej serii

Jest to część serii pięciu samouczków dotyczących wdrażania w sieci Web w skali korporacyjnej. Oto inne samouczki z serii:

- [Wdrażanie aplikacji sieci Web w scenariuszach dla przedsiębiorstw](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Ta zawartość wprowadzająca zawiera kontekstowe tło dla serii samouczków. Opisano w nim Scenariusz samouczka, który ilustruje sposób, w jaki zadania i instruktaże opisane w całej serii mieszczą się w szerszym procesie zarządzania cyklem życia aplikacji (ALM).
- [Wdrażanie w sieci Web w przedsiębiorstwie](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Ten samouczek zawiera ogólne wprowadzenie do plików projektów programu MSBuild, potoku publikowania w sieci Web (WPP), Web Deploy i innych powiązanych technologii. Wyjaśniono, jak można używać tych narzędzi razem do zarządzania złożonymi procesami wdrażania.
- [Konfigurowanie środowisk serwera na potrzeby wdrażania w sieci Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). W tym samouczku opisano sposób konfigurowania serwerów z systemem Windows w celu obsługi różnych scenariuszy wdrażania, w tym wdrażania zdalnego pakietu sieci Web przy użyciu usługi Deployment Agent sieci Web (agenta zdalnego) lub obsługi Web Deploy i zdalnego wdrażania bazy danych. Zawiera on wskazówki dotyczące wybierania odpowiedniej metody wdrażania dla własnego środowiska. opisano w nim sposób korzystania z platformy Web Farms (WFF) w celu replikowania wdrożonych aplikacji sieci Web na wszystkich serwerach sieci Web w farmie serwerów.
- [Zaawansowane wdrażanie w sieci Web dla przedsiębiorstw](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). W tym samouczku opisano, jak wykonać różne bardziej zaawansowane zadania wdrażania, takie jak Dostosowywanie wdrożeń baz danych dla wielu środowisk, wykluczanie plików i folderów z wdrożenia oraz pobieranie aplikacji sieci Web w trybie offline podczas procesu wdrażania .

> [!div class="step-by-step"]
> [Dalej](creating-a-team-project-in-tfs.md)

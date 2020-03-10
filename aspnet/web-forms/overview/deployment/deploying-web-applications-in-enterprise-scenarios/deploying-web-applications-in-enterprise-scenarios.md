---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios
title: Wdrażanie aplikacji sieci Web w scenariuszach dla przedsiębiorstw przy użyciu programu Visual Studio 2010 | Microsoft Docs
author: jrjlee
description: Ten zestaw samouczków zawiera opis narzędzi i technik, których można użyć do wdrożenia aplikacji sieci Web w różnych scenariuszach dla przedsiębiorstw. Wyjaśniono, jak najlepiej wykorzystać...
ms.author: riande
ms.date: 05/03/2012
ms.assetid: 48cfe378-d62a-48c6-a4db-6be3cead6898
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios
msc.type: authoredcontent
ms.openlocfilehash: eb7b82d16079d4d086af1919d092fb28db60d8b3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78574159"
---
# <a name="deploying-web-applications-in-enterprise-scenarios-using-visual-studio-2010"></a>Wdrażanie aplikacji internetowych w scenariuszach dla przedsiębiorstw przy użyciu programu Visual Studio 2010

Autor [Jason Lewandowski](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Ten zestaw samouczków zawiera opis narzędzi i technik, których można użyć do wdrożenia aplikacji sieci Web w różnych scenariuszach dla przedsiębiorstw. Wyjaśniono, jak najlepiej wykorzystać technologie takie jak Visual Studio 2010, Microsoft Build Engine (MSBuild), Internet Information Services (IIS) 7,5, narzędzie do wdrażania w sieci Web (Web Deploy) usług IIS, środowisko kolektywu serwerów sieci Web (WFF) i narzędzia, takie jak VSDBCMD. exe, do Uprość proces wdrażania i Zarządzaj nim. Obejmuje to przeglądy koncepcyjne i wskazówki zorientowane na zadania, które ułatwią:
> 
> - Przejrzyj i ustal wymagania dotyczące wdrażania dla aplikacji sieci Web w skali przedsiębiorstwa.
> - Konfigurowanie środowisk testowych, przejściowych i produkcyjnych serwerów sieci Web do obsługi wdrażania w sieci Web.
> - Skonfiguruj procesy ciągłej integracji z programem Team Foundation Server (TFS), aby obsługiwać Zautomatyzowane wdrażanie w sieci Web.
> - Wdrażaj aplikacje sieci Web w skali korporacyjnej w różnych środowiskach serwera z różnymi wymaganiami i ograniczeniami.
> - Wdrażaj zmiany w aplikacjach sieci Web, które działają w różnych środowiskach serwera.
> 
> > [!NOTE]
> > Chociaż te samouczki opisują użycie programu TFS jako serwera elementu konfiguracji, wskazówki te można łatwo dostosować do dowolnego serwera elementu konfiguracji. Nie potrzebujesz szczegółowej wiedzy na temat TFS, aby zrozumieć i korzystać z samouczków.
> 
> 
> Aby uzyskać włoskie tłumaczenie tych samouczków, odwiedź stronę [http://www.lucamorelli.it](http://www.lucamorelli.it).

## <a name="about-the-authors"></a>Informacje o autorze

Jason Lewandowski to główna technologa ze [wzorcem zawartości](http://www.contentmaster.com/) , w której pracował z produktami i technologiami firmy Microsoft, w szczególności z programem SharePoint i ASP.NET, przez kilka lat. Jason przechowuje doktora w trakcie obliczeń i jest obecnie MCPD i MCTS certyfikowane. Blog techniczny Jason można przeczytać pod adresem [www.jrjlee.com](http://www.jrjlee.com/).

Benjamin curry to główna technolog z [wzorcem zawartości](http://www.contentmaster.com/) , który posiada oficjalne dokumenty, dokumentację zestawu SDK, prezentacje programu PowerPoint i szkolenia instruktora i kurs online w trakcie swojej kariery. Oryginalny członek zespołu ds. dokumentacji ASP.NET pracował z technologiami sieci Web firmy Microsoft w ciągu dekady.

## <a name="target-audience"></a>Odbiorcy docelowi

Ten zestaw samouczków jest przeznaczony dla deweloperów aplikacji sieci Web ASP.NET i architektów rozwiązań, którzy używają programu Visual Studio 2010 do tworzenia aplikacji sieci Web w skali korporacyjnej. Aby uzyskać największą wartość z zawartości, należy mieć doświadczenie w korzystaniu z programu Visual Studio 2010 i mieć podstawową wiedzę o programie TFS, a także poznać technologie platformy sieci Web firmy Microsoft, takie jak ASP.NET MVC 3, Windows Communication Foundation (WCF), IIS i SQL Serwer i projekty bazy danych programu Visual Studio. Nie trzeba jednak znać narzędzi wdrażania i technologii ani wiedzieć, jak skonfigurować systemy CI.

## <a name="requirements"></a>Wymagania

Aby wykonać czynności opisane w przewodnikach i wykonać zadania opisane w tych samouczkach, należy zainstalować to oprogramowanie na komputerze deweloperskim:

- Visual Studio 2010 Premium lub Ultimate z dodatkiem Service Pack 1
- .NET Framework 4.0
- .NET Framework 3,5 z dodatkiem Service Pack 1
- ASP.NET MVC 3.0
- IIS 7.5 Express
- SQL Server Express 2008 R2

Aby wykonać kroki wdrażania opisane w tym przewodniku, musisz mieć dostęp do środowisk wdrażania aplikacji sieci Web. W celu uzyskania najlepszych wyników te środowiska powinny odzwierciedlać wzorzec wdrożenia przedsiębiorstwa w organizacji. Następnie można zmodyfikować instruktaży podane w tej dokumentacji, aby odzwierciedlały środowiska wdrażania i wymagania organizacji.

## <a name="series-contents"></a>Zawartość serii

Ta sekcja wprowadzająca zawiera dwa dalsze tematy. Zostały one zaprojektowane w celu zapewnienia szerszego kontekstu dla samouczków, które są następujące:

- [Wdrażanie w sieci Web dla przedsiębiorstw: Omówienie scenariusza](enterprise-web-deployment-scenario-overview.md). W tym temacie opisano scenariusz, który podpisuje poszczególne samouczki w tej serii. Scenariusz koncentruje się na wymaganiach w zakresie zarządzania cyklem życia aplikacji (ALM) fikcyjnej firmy Fabrikam, Inc. podczas opracowywania aplikacji sieci Web w skali przedsiębiorstwa.
- [Zarządzanie cyklem życia aplikacji: od projektowania do produkcji](application-lifecycle-management-from-development-to-production.md). Ten temat zawiera szczegółowe omówienie procesu wdrażania w ramach pełnego poziomu. Ilustruje to, jak Fabrikam, Inc. przenosi aplikację sieci Web ASP.NET w skali przedsiębiorstwa za pomocą środowisk testowych, przejściowych i produkcyjnych w ramach procesu ciągłego tworzenia oprogramowania.

Seria zawiera cztery zestawy samouczków. Każdy koncentruje się na różnych aspektach wdrażania w sieci Web:

- [Wdrażanie w sieci Web w przedsiębiorstwie](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Ten samouczek zawiera ogólne wprowadzenie do plików projektów programu MSBuild, potoku publikowania w sieci Web, Web Deploy i innych powiązanych technologii. Wyjaśniono, jak można używać tych narzędzi razem do zarządzania złożonymi procesami wdrażania.
- [Konfigurowanie środowisk serwera na potrzeby wdrażania w sieci Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). W tym samouczku opisano sposób konfigurowania serwerów z systemem Windows w celu obsługi różnych scenariuszy wdrażania, w tym wdrażania zdalnego pakietu sieci Web przy użyciu usługi Deployment Agent sieci Web ("agenta zdalnego") lub obsługi Web Deploy i zdalnego wdrażania bazy danych. Zawiera on wskazówki dotyczące wybierania odpowiedniej metody wdrażania dla własnego środowiska. opisano w nim, jak używać WFF do replikowania wdrożonych aplikacji sieci Web na wszystkich serwerach sieci Web w farmie serwerów.
- [Konfigurowanie Team Foundation Server wdrażania w sieci Web](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). W tym samouczku opisano sposób konfigurowania TFS w celu obsługi różnych scenariuszy wdrażania, w tym automatycznego wdrażania w ramach procesu CI i ręcznie wyzwalania wdrożeń określonych kompilacji.
- [Zaawansowane wdrażanie w sieci Web dla przedsiębiorstw](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). W tym samouczku opisano, jak wykonać różne bardziej zaawansowane zadania wdrażania, takie jak Dostosowywanie wdrożeń baz danych dla wielu środowisk, wykluczanie plików i folderów z wdrożenia oraz pobieranie aplikacji sieci Web w trybie offline podczas procesu wdrażania .

## <a name="where-to-start"></a>Miejsce rozpoczęcia

Ten zestaw samouczków korzysta z przykładowego rozwiązania o realistycznym poziomie złożoności, a także fikcyjnego scenariusza wdrażania przedsiębiorstwa, aby zapewnić implementację referencyjną i zadawać zadania i instruktażować wspólny kontekst. W następnym temacie [wdrażanie w sieci Web dla przedsiębiorstw: Omówienie scenariusza](enterprise-web-deployment-scenario-overview.md), wprowadzono scenariusz i przykładowe rozwiązanie. Z tego miejsca można korzystać z samouczków i tematów, które najlepiej pasują do Twoich potrzeb.

> [!div class="step-by-step"]
> [Dalej](enterprise-web-deployment-scenario-overview.md)

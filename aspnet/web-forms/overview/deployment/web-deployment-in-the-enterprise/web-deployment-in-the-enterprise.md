---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/web-deployment-in-the-enterprise
title: Wdrażanie w sieci Web w przedsiębiorstwie | Microsoft Docs
author: jrjlee
description: W tym samouczku opisano sposób zaspokajania wielu wyzwań, które można napotkać podczas zarządzania wdrażaniem aplikacji sieci Web w skali korporacyjnej w devel...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: b8283698-7b82-42a8-8d83-3aeb18ca7fcc
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/web-deployment-in-the-enterprise
msc.type: authoredcontent
ms.openlocfilehash: bc7bb676a71af4ec3451aa3adf3c03ce3b5200d5
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78628171"
---
# <a name="web-deployment-in-the-enterprise"></a>Wdrażanie w Internecie w przedsiębiorstwie

Autor [Jason Lewandowski](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym samouczku opisano sposób zaspokajania wielu wyzwań, które można napotkać podczas zarządzania wdrażaniem aplikacji sieci Web w skali korporacyjnej w środowiskach programistycznych, testowych, przejściowych i produkcyjnych. Samouczek zawiera rozwiązanie referencyjne wraz z mieszaną treścią koncepcyjną i zorientowaną na zadania, która przeprowadzi Cię przez różne typowe zadania i procedury.
> 
> Aby uzyskać włoskie tłumaczenie tych samouczków, odwiedź stronę [http://www.lucamorelli.it](http://www.lucamorelli.it).

## <a name="enterprise-deployment-challenges"></a>Wyzwania dotyczące wdrażania w przedsiębiorstwie

Organizacje często napotykają te wyzwania, gdy chcą zarządzać wdrożeniem złożonych rozwiązań w skali korporacyjnej:

- Musisz być w stanie wdrożyć projekty w wielu środowiskach, takich jak środowiska deweloperskie lub testowe, platformy przejściowe i serwery produkcyjne. Rozwiązanie należy wdrożyć z różnymi ustawieniami konfiguracji dla każdego środowiska.
- Należy wdrożyć wiele zależnych projektów jednocześnie w ramach jednego kroku lub zautomatyzowanego procesu kompilowania i wdrażania.
- Musisz mieć możliwość nawiązywania wdrożenia przez proces zautomatyzowany. Na przykład, chcesz użyć procesu ciągłej integracji (CI) do wdrożenia aplikacji sieci Web w środowisku testowym, gdy nowy kod jest zaewidencjonowany.
- Musisz mieć możliwość kontrolowania procesu wdrażania i ustawiania zmiennych wdrożenia spoza programu Visual Studio, ponieważ deweloperzy nie będą mieć wystarczających ustawień konfiguracji lub wymaganych poświadczeń dla każdego środowiska docelowego.
- Należy wdrożyć projekty bazy danych oparte na schemacie i zachować istniejące dane w kolejnych wdrożeniach.
- Należy wdrożyć bazy danych członkostwa na zasadzie ad hoc bez wdrażania danych konta użytkownika. Może być również konieczne zaktualizowanie schematu wdrożonych baz danych członkostwa bez utraty istniejących danych konta użytkownika.
- Podczas wdrażania zawartości w różnych środowiskach docelowych należy wykluczyć określone pliki lub foldery.

## <a name="overview-of-approach"></a>Przegląd podejścia

Ten samouczek, wraz z innymi samouczkami w tej serii, używa tego podejścia wysokiego poziomu w celu spełnienia opisanych powyżej wyzwań.

- **Użyj niestandardowych plików projektu Microsoft Build Engine (MSBuild), aby kontrolować ogólny proces kompilowania i wdrażania.**
- Dzięki temu można kompilować i wdrażać każdy projekt w rozwiązaniu w ramach jednej operacji skryptowej.
- Ustawienia specyficzne dla środowiska są konfigurowane przy użyciu prostych plików projektu specyficznych dla środowiska. W przeciwieństwie do podejścia zorientowanego na program Visual Studio przy użyciu konfiguracji rozwiązań i profilów publikowania w celu skonfigurowania wdrożeń dla różnych środowisk, ta metoda umożliwia skonfigurowanie i zarządzanie procesem wdrażania spoza programu Visual Studio. Oznacza to, że deweloperzy nie potrzebują zaawansowaną wiedzę o ciągach połączeń, punktach końcowych usług, poświadczeniach serwera i innych zmiennych wdrożenia dla środowisk docelowych.
- Niestandardowe pliki projektu mogą być wywoływane przez kompilację zespołu w ramach przepływu pracy Team Foundation Server (TFS). Pozwala to skonfigurować Zautomatyzowane wdrażanie dla scenariuszy CI.

**Użyj Internet Information Services (IIS) narzędzia do wdrażania w sieci Web (Web Deploy) do pakowania i wdrażania projektów aplikacji sieci Web.**

- Web Deploy udostępnia strukturę, która umożliwia pakowanie i wdrażanie zawartości aplikacji sieci Web na docelowym serwerze sieci Web usług IIS, wraz z zależnościami, ustawieniami konfiguracji, ustawieniami zabezpieczeń i innymi wymaganiami.
- Można kontrolować całe pakowanie i proces wdrażania z poziomu niestandardowych plików projektu programu MSBuild. Można również manipulować ustawieniami konfiguracji dołączonymi do pakietu wdrożeniowego sieci Web, takimi jak parametry połączenia, punkty końcowe usługi i szczegóły docelowej usługi IIS.
- Web Deploy, wraz z potokiem publikowania w sieci Web, oferuje wiele punktów rozszerzalności, które pozwalają dostosować wdrożenia. Na przykład można łatwo wykluczyć niechciane pliki i foldery z pakietów wdrożeniowych sieci Web.

**Aby wdrażać i aktualizować schematy bazy danych, należy użyć narzędzia VSDBCMD. exe.**

- VSDBCMD umożliwia wdrażanie baz danych z pliku schematu bazy danych (. DbSchema), który jest generowany podczas tworzenia projektu bazy danych programu Visual Studio. Z kolei funkcje wdrażania bazy danych zawarte w Web Deploy są bardziej odpowiednie do wdrażania istniejących baz danych z lokalnego wystąpienia SQL Server.
- W przeciwieństwie do funkcji programu Visual Studio do wdrażania projektów bazy danych, VSDBCMD umożliwia wdrażanie różnicowych aktualizacji do istniejącej docelowej bazy danych. Pozwala to zachować istniejące dane podczas uaktualniania schematu bazy danych.
- Polecenia VSDBCMD można wykonywać z poziomu niestandardowych plików projektu programu MSBuild.

## <a name="content-map"></a>Mapa zawartości

Ten samouczek zawiera tematy, które znajdują się w czterech obszarach głównych.

W tych tematach wprowadzono rozwiązanie&#x2014;referencyjne rozwiązania&#x2014;Contact Manager i opisano sposób jego pobrania i skonfigurowania na komputerze lokalnym:

- [Rozwiązanie Contact Manager](the-contact-manager-solution.md)
- [Konfigurowanie rozwiązania Contact Manager](setting-up-the-contact-manager-solution.md)

Te tematy zawierają pliki projektów programu MSBuild, opisują sposób tworzenia i używania niestandardowych plików projektu oraz przechodzenia przez proces wdrażania rozwiązania Contact Manager:

- [Objaśnienie pliku projektu](understanding-the-project-file.md)
- [Objaśnienie procesu kompilacji](understanding-the-build-process.md)

W tych tematach opisano wdrażanie aplikacji sieci Web, w tym informacje o tym, jak działa kompilacja i proces pakowania, jak proces kompilacji integruje się z potokiem publikowania w sieci Web, jak modyfikować parametry wdrożenia oraz jak wdrażać pakiety internetowe w miejscu docelowym wiejski

- [Kompilowanie i tworzenie pakietów projektów aplikacji internetowych](building-and-packaging-web-application-projects.md)
- [Konfigurowanie parametrów na potrzeby wdrożenia pakietu internetowego](configuring-parameters-for-web-package-deployment.md)
- [Wdrażanie pakietów internetowych](deploying-web-packages.md)

- [Wdrażanie projektów bazy danych](deploying-database-projects.md) opisuje różne techniki, których można użyć do wdrożenia projektów bazy danych programu Visual Studio, wraz z zaletami i wadami poszczególnych rozwiązań. [Tworzenie i uruchamianie pliku poleceń wdrażania](creating-and-running-a-deployment-command-file.md) opisuje sposób tworzenia prostego pliku poleceń, który hermetyzuje logikę wdrażania i umożliwia wdrażanie złożonych rozwiązań jako procesu pojedynczego kroku.
- Na koniec [Ręczne instalowanie pakietów internetowych](manually-installing-web-packages.md) zawiera samouczek, pokazujący, jak zaimportować pakiety internetowe do usług IIS.

## <a name="key-technologies"></a>Najważniejsze technologie

Tematy w tym samouczku wykorzystują głównie te technologie do zarządzania kompilowaniem i wdrażaniem:

- Visual Studio 2010
- MSBuild
- IIS 7.5
- Wdrożenie w sieci Web 2.0
- Narzędzie wdrażania bazy danych VSDBCMD. exe

## <a name="other-tutorials-in-this-series"></a>Inne samouczki w tej serii

Jest to część serii pięciu samouczków dotyczących wdrażania w sieci Web w skali korporacyjnej. Oto inne samouczki z serii:

- [Wdrażanie aplikacji sieci Web w scenariuszach dla przedsiębiorstw](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Ta zawartość wprowadzająca zawiera kontekstowe tło dla serii samouczków. Opisano w nim Scenariusz samouczka, który ilustruje sposób, w jaki zadania i instruktaże opisane w całej serii mieszczą się w szerszym procesie zarządzania cyklem życia aplikacji (ALM).
- [Konfigurowanie środowisk serwera na potrzeby wdrażania w sieci Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). W tym samouczku opisano sposób konfigurowania serwerów z systemem Windows w celu obsługi różnych scenariuszy wdrażania, w tym wdrażania zdalnego pakietu sieci Web przy użyciu usługi Deployment Agent sieci Web (agenta zdalnego) lub obsługi Web Deploy i zdalnego wdrażania bazy danych. Zawiera on wskazówki dotyczące wybierania odpowiedniej metody wdrażania dla własnego środowiska. opisano w nim sposób korzystania z platformy Web Farms (WFF) w celu replikowania wdrożonych aplikacji sieci Web na wszystkich serwerach sieci Web w farmie serwerów.
- [Konfigurowanie Team Foundation Server wdrażania w sieci Web](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). W tym samouczku opisano sposób konfigurowania TFS w celu obsługi różnych scenariuszy wdrażania, w tym automatycznego wdrażania w ramach procesu CI i ręcznie wyzwalania wdrożeń określonych kompilacji.
- [Zaawansowane wdrażanie w sieci Web dla przedsiębiorstw](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). W tym samouczku opisano, jak wykonać różne bardziej zaawansowane zadania wdrażania, takie jak Dostosowywanie wdrożeń baz danych dla wielu środowisk, wykluczanie plików i folderów z wdrożenia oraz pobieranie aplikacji sieci Web w trybie offline podczas procesu wdrażania .

> [!div class="step-by-step"]
> [Dalej](the-contact-manager-solution.md)

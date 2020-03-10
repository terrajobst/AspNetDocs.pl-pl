---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/advanced-enterprise-web-deployment
title: Zaawansowane wdrażanie w sieci Web dla przedsiębiorstw | Microsoft Docs
author: jrjlee
description: W tym samouczku przedstawiono sposób wykonywania różnych zadań wymaganych lub pożądanych w wielu scenariuszach wdrażania w przedsiębiorstwie. Dla translati włoskiego...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 7dcaba80-f2ec-4db3-ad98-daadc3afdb49
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/advanced-enterprise-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: bf4c7d021763017592483df35cb717295c4924aa
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78587606"
---
# <a name="advanced-enterprise-web-deployment"></a>Zaawansowane wdrażanie w Internecie w przedsiębiorstwie

Autor [Jason Lewandowski](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym samouczku przedstawiono sposób wykonywania różnych zadań wymaganych lub pożądanych w wielu scenariuszach wdrażania w przedsiębiorstwie.
> 
> Aby uzyskać włoskie tłumaczenie tych samouczków, odwiedź stronę [http://www.lucamorelli.it](http://www.lucamorelli.it).

Jest to część szeregu samouczków w oparciu o wymagania dotyczące wdrażania w przedsiębiorstwie fikcyjnej firmy o nazwie Fabrikam, Inc. W tej serii samouczków jest stosowane&#x2014;przykładowe [](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) rozwiązanie&#x2014;do reprezentowania aplikacji sieci Web, które ma realistyczny poziom złożoności, w tym aplikacji ASP.NET MVC 3, usługi Windows Communication Foundation (WCF) i projektu bazy danych.

Metoda wdrażania w tym samouczku jest oparta na podejściu do pliku projektu podzielonego opisanego w artykule [Opis procesu kompilacji](../web-deployment-in-the-enterprise/understanding-the-build-process.md), w którym proces kompilacji jest kontrolowany przez dwa pliki&#x2014;projektu, zawierający instrukcje kompilacji, które mają zastosowanie do każdego środowiska docelowego, oraz jeden zawierający ustawienia kompilacji i wdrożenia specyficznego dla środowiska. W czasie kompilacji plik projektu specyficzny dla środowiska jest scalany z plikiem projektu Environment-niezależny od w celu utworzenia kompletnego zestawu instrukcji kompilacji.

## <a name="scenario-overview"></a>omówienie scenariusza

Scenariusz wysokiego poziomu dla tych samouczków został opisany w artykule [wdrażanie w sieci Web dla przedsiębiorstw: Omówienie scenariusza](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md). Zalecamy zapoznanie się z tym tematem przed rozpoczęciem pracy z tym samouczkiem.

## <a name="how-to-use-this-tutorial"></a>Jak korzystać z tego samouczka

- Każdy z tematów w tym samouczku jest samodzielny i dotyczy konkretnego wyzwania lub problemu występującego w scenariuszach wdrażania w przedsiębiorstwie. Nie musisz korzystać z tych tematów w żadnej określonej kolejności. Jednak w tym samouczku omówiono niektóre zaawansowane zadania. W związku z tym należy zapoznać się z pojęciami i technikami, które [w](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md) samouczku dotyczącym przedsiębiorstwa odnoszą się w celu uzyskania największej korzyści z tej zawartości.
- Ten samouczek zawiera następujące tematy:
- [Wykonywanie wdrożenia "What If"](performing-a-what-if-deployment.md). W wielu scenariuszach warto określić wpływ proponowanego wdrożenia na środowisko docelowe lub wszelką istniejącą zawartość przed faktycznym wprowadzeniem jakichkolwiek zmian. W tym temacie opisano, jak można uruchomić wdrożenie "co jeśli" w celu wygenerowania plików dziennika i skryptów aktualizacji bazy danych tak, jakby była wdrożona zawartość w środowisku docelowym, bez wprowadzania jakichkolwiek zmian. Analizowanie tych zasobów może pomóc w objściu wszelkich potencjalnych problemów przed wdrożeniem na żywo.
- [Dostosowywanie wdrożeń baz danych w wielu środowiskach](customizing-database-deployments-for-multiple-environments.md). Podczas wdrażania projektu bazy danych w wielu miejscach docelowych często chcesz dostosować właściwości wdrożenia dla każdego środowiska docelowego. Na przykład w środowiskach testowych zwykle ponownie utworzysz bazę danych w każdym wdrożeniu, podczas gdy w środowisku przejściowym lub produkcyjnym znacznie bardziej zachodzi potrzeba przeprowadzenia aktualizacji przyrostowych w celu zachowania danych. W tym temacie opisano, jak można dołączyć te zmiany właściwości do logiki wdrażania, tworząc plik konfiguracji wdrożenia specyficzny dla środowiska (. sqldeployment) dla każdego środowiska docelowego.
- Wdrażanie członkostw roli bazy danych w środowiskach testowych. Podczas ponownego tworzenia bazy danych na każdym wdrożeniu&#x2014;na przykład w ramach kompilacji ciągłej integracji (ci) i wdrożenia jej w środowisku&#x2014;testowym zwykle trzeba skonfigurować członkostwa ról bazy danych za każdym razem. Na przykład zwykle trzeba udzielić uprawnień do tożsamości puli aplikacji skojarzonej z aplikacją sieci Web. W tym temacie opisano, jak można zautomatyzować ten proces, dodając skrypt SQL po wdrożeniu do logiki wdrożenia.
- [Wdrażanie baz danych członkostwa w środowiskach przedsiębiorstw](deploying-membership-databases-to-enterprise-environments.md). Bazy danych członkostwa ASP.NET mają różne cechy, które mogą komplikują proces wdrażania. Na przykład wdrożenie tylko do schematu spowoduje pozostawienie bazy danych w stanie nieoperacyjnym. W większości scenariuszy zaleca się utworzenie bazy danych członkostwa bezpośrednio w każdym środowisku docelowym. Jednak w przypadku konieczności wdrożenia bazy danych członkostwa w tym temacie opisano niektóre podejścia, których można użyć w celu spełnienia wymagań.
- [Wykluczanie plików i folderów z wdrożenia](excluding-files-and-folders-from-deployment.md). W niektórych scenariuszach warto dostosować zawartość pakietu internetowego do określonych środowisk docelowych. Na przykład podczas wdrażania programu w środowisku testowym może być konieczne uwzględnienie pełnych wersji bibliotek języka JavaScript, które obsługują debugowanie po stronie klienta, ale podczas wdrażania programu w środowisku przejściowym lub produkcyjnym należy używać wersji zminimalizowanego bibliotek. W tym temacie opisano, jak można wykluczyć określone pliki i foldery z procesu tworzenia pakietu.
- [Przełączanie aplikacji sieci Web w tryb offline przy użyciu Web Deploy](taking-web-applications-offline-with-web-deploy.md). Podczas wdrażania rozwiązań w środowisku przejściowym lub produkcyjnym często warto przełączyć aplikacje sieci Web w tryb offline na czas trwania procesu wdrożenia. W tym temacie opisano, jak można dodać *aplikację\_pliku offline. htm* do aplikacji sieci Web na początku procesu wdrażania i usunąć ją na końcu. Gdy *aplikacja\_w trybie offline. htm* , wszyscy użytkownicy przeglądający aplikację sieci Web są automatycznie przekierowywani do *aplikacji\_pliku w trybie offline. htm* .
- [Uruchamianie skryptów programu Windows PowerShell z programu MSBuild](running-windows-powershell-scripts-from-msbuild-project-files.md). Wiele scenariuszy wdrażania wymaga bardziej złożonych akcji po wdrożeniu, takich jak Dodawanie niestandardowych źródeł zdarzeń do rejestru lub Konfigurowanie replikacji między wystąpieniami SQL Server. Te akcje są często wykonywane za poorednictwem skryptów programu Windows PowerShell. W tym temacie opisano sposób uruchamiania skryptów programu Windows PowerShell z pliku projektu Microsoft Build Engine (MSBuild) w ramach procesu kompilowania i wdrażania.
- [Rozwiązywanie problemów z procesem tworzenia pakietów](troubleshooting-the-packaging-process.md). Potok publikowania w sieci Web (WPP) definiuje Właściwość programu MSBuild o nazwie **EnablePackageProcessLoggingAndAssert** , której można użyć do generowania szczegółowych informacji o procesie tworzenia pakietów dla projektów aplikacji sieci Web. W tym temacie opisano działanie właściwości i sposób jej używania.

## <a name="key-technologies"></a>Najważniejsze technologie

Ten samouczek koncentruje się na sposobach używania tych produktów i technologii do obsługi zautomatyzowanej kompilacji i wdrażania w sieci Web:

- Visual Studio 2010 i Team Foundation Server (TFS) 2010
- Kompilacja kompilacji MSBuild i TFS
- Internet Information Services (IIS) 7,5
- Narzędzie do wdrażania w sieci Web usług IIS (Web Deploy) 2,1
- Narzędzie wdrażania bazy danych VSDBCMD. exe

## <a name="other-tutorials-in-this-series"></a>Inne samouczki w tej serii

Jest to część serii pięciu samouczków dotyczących wdrażania w sieci Web w skali korporacyjnej. Są to inne samouczki z serii:

- [Wdrażanie aplikacji sieci Web w scenariuszach dla przedsiębiorstw](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Ta zawartość wprowadzająca zawiera kontekstowe tło dla serii samouczków. Opisano w nim Scenariusz samouczka, który ilustruje sposób, w jaki zadania i instruktaże opisane w całej serii mieszczą się w szerszym procesie zarządzania cyklem życia aplikacji (ALM).
- [Wdrażanie w sieci Web w przedsiębiorstwie](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Ten samouczek zawiera ogólne wprowadzenie do plików projektu MSBuild, WPP, Web Deploy i innych powiązanych technologii. Wyjaśniono, jak można używać tych narzędzi razem do zarządzania złożonymi procesami wdrażania.
- [Konfigurowanie środowisk serwera na potrzeby wdrażania w sieci Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). W tym samouczku opisano sposób konfigurowania serwerów z systemem Windows w celu obsługi różnych scenariuszy wdrażania, w tym wdrażania zdalnego pakietu sieci Web przy użyciu usługi Deployment Agent sieci Web (agenta zdalnego) lub obsługi Web Deploy i zdalnego wdrażania bazy danych. Zawiera on wskazówki dotyczące wybierania odpowiedniej metody wdrażania dla własnego środowiska. opisano w nim sposób korzystania z platformy Web Farms (WFF) w celu replikowania wdrożonych aplikacji sieci Web na wszystkich serwerach sieci Web w farmie serwerów.
- [Konfigurowanie Team Foundation Server wdrażania w sieci Web](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). W tym samouczku opisano sposób konfigurowania TFS w celu obsługi różnych scenariuszy wdrażania, w tym automatycznego wdrażania w ramach procesu CI i ręcznie wyzwalania wdrożeń określonych kompilacji.

> [!div class="step-by-step"]
> [Dalej](performing-a-what-if-deployment.md)

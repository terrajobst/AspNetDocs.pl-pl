---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/application-lifecycle-management-from-development-to-production
title: 'Zarządzanie cyklem życia aplikacji: od projektowania do produkcji | Microsoft Docs'
author: jrjlee
description: W tym temacie przedstawiono sposób, w jaki fikcyjna firma zarządza wdrażaniem aplikacji sieci Web ASP.NET za pomocą środowisk testowych, przejściowych i produkcyjnych jako wartości nominalnych...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: f97a1145-6470-4bca-8f15-ccfb25fb903c
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/application-lifecycle-management-from-development-to-production
msc.type: authoredcontent
ms.openlocfilehash: 230cf4393db0ee19cfc42ed54359d61e7926a49d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78640666"
---
# <a name="application-lifecycle-management-from-development-to-production"></a>Zarządzanie cyklem życia aplikacji: od projektowania do produkcji

Autor [Jason Lewandowski](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie przedstawiono sposób, w jaki fikcyjna firma zarządza wdrażaniem aplikacji sieci Web ASP.NET za pomocą środowisk testowych, przejściowych i produkcyjnych w ramach procesu ciągłego tworzenia oprogramowania. W tym temacie przedstawiono linki do dalszych informacji i przewodników dotyczących wykonywania określonych zadań.
> 
> Temat został zaprojektowany w celu zapewnienia wysokiego poziomu ogólnych informacji na temat [szeregu samouczków](deploying-web-applications-in-enterprise-scenarios.md) dotyczących wdrażania w Internecie w przedsiębiorstwie. Nie martw się, jeśli nie znasz niektórych koncepcji opisanych w tym&#x2014;samouczku, aby uzyskać szczegółowe informacje na temat wszystkich tych zadań i technik.
> 
> > [!NOTE]
> > W celu uproszczenia ten temat nie omawia aktualizacji baz danych w ramach procesu wdrażania. Jednak wykonywanie aktualizacji przyrostowych baz danych jest wymaganiem wielu scenariuszy wdrażania w przedsiębiorstwie i można znaleźć wskazówki dotyczące tego, jak to zrobić w dalszej części tej serii samouczków. Aby uzyskać więcej informacji, zobacz [wdrażanie projektów bazy danych](../web-deployment-in-the-enterprise/deploying-database-projects.md).

## <a name="overview"></a>Omówienie

Opisany tutaj proces wdrażania jest oparty na scenariuszu wdrażania firmy Fabrikam, Inc. opisanego w artykule [wdrażanie w przedsiębiorstwie w sieci Web: Omówienie scenariusza](enterprise-web-deployment-scenario-overview.md). Zapoznaj się z omówieniem scenariusza przed rozpoczęciem badania tego tematu. Zasadniczo scenariusz analizuje sposób, w jaki organizacja zarządza wdrożeniem odpowiednio złożonej aplikacji sieci Web, [rozwiązania Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)w różnych fazach w typowym środowisku przedsiębiorstwa.

Na wysokim poziomie rozwiązanie Contact Manager przechodzi przez te etapy w ramach procesu tworzenia i wdrażania:

1. Deweloper sprawdza kod w Team Foundation Server (TFS) 2010.
2. TFS kompiluje kod i uruchamia wszystkie testy jednostkowe skojarzone z projektem zespołowym.
3. TFS wdraża rozwiązanie w środowisku testowym.
4. Zespół deweloperów sprawdza i weryfikuje rozwiązanie w środowisku testowym.
5. W celu ustalenia, czy wdrożenie spowoduje jakiekolwiek problemy, administrator środowiska tymczasowego wykonuje wdrożenie w środowisku "co jeśli".
6. Administrator środowiska tymczasowego wykonuje wdrożenie na żywo w środowisku przejściowym.
7. Rozwiązanie przeprowadzi testowanie akceptacji przez użytkownika w środowisku przejściowym.
8. Pakiety wdrażania sieci Web są ręcznie importowane do środowiska produkcyjnego.

Te etapy stanowią część ciągłego cyklu programowania.

![](application-lifecycle-management-from-development-to-production/_static/image1.png)

W ćwiczeniu proces jest nieco bardziej skomplikowany niż ten, ponieważ zobaczysz, że widzimy poszczególne etapy w bardziej szczegółowy sposób. Fabrikam, Inc. stosuje różne podejście do wdrożenia dla każdego środowiska docelowego.

![](application-lifecycle-management-from-development-to-production/_static/image2.png)

Pozostała część tego tematu dotyczy najważniejszych etapów tego cyklu wdrożenia:

- **Wymagania wstępne**: jak należy skonfigurować infrastrukturę serwera przed wprowadzeniem logiki wdrożenia.
- **Początkowe programowanie i wdrażanie**: co należy zrobić przed wdrożeniem rozwiązania po raz pierwszy.
- **Wdrożenie do przetestowania**: Jak spakować i wdrożyć zawartość w środowisku testowym automatycznie, gdy deweloper sprawdzi nowy kod.
- **Wdrażanie do przemieszczania**: jak wdrażać konkretne kompilacje w środowisku przejściowym oraz jak wykonywać wdrożenia "co jeśli", aby upewnić się, że wdrożenie nie spowoduje żadnych problemów.
- **Wdrażanie w środowisku produkcyjnym**: Importowanie pakietów sieci Web do środowiska produkcyjnego, gdy infrastruktura sieciowa uniemożliwia zdalne wdrażanie.

## <a name="prerequisites"></a>Wymagania wstępne

Pierwsze zadanie w dowolnym scenariuszu wdrożenia polega na zapewnieniu, że infrastruktura serwera spełnia wymagania narzędzi i technik wdrażania. W tym przypadku firma Fabrikam Inc. skonfigurował jej infrastrukturę serwera w następujący sposób:

- TFS jest skonfigurowany do uwzględnienia kolekcji projektów zespołowych, kontrolerów kompilacji i agentów kompilacji. Aby uzyskać więcej informacji, zobacz [konfigurowanie Team Foundation Server automatycznego wdrażania w sieci Web](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md) .
- Środowisko testowe jest skonfigurowane do akceptowania wdrożeń zdalnych przy użyciu usługi Deployment Agent sieci Web ("zdalnego agenta"), zgodnie z opisem w temacie [Scenariusz: Konfigurowanie środowiska testowego na potrzeby wdrażania w sieci Web](../configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment.md) i [Konfigurowanie serwera sieci Web do publikowania Web Deploy (Agent zdalny)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md).
- Środowisko przejściowe jest skonfigurowane do akceptowania wdrożeń zdalnych przy użyciu punktu końcowego procedury obsługi Web Deploy, zgodnie z opisem w temacie [Scenariusz: Konfigurowanie środowiska przejściowego na potrzeby wdrażania w sieci Web](../configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment.md) i [Konfigurowanie serwera sieci web do publikowania Web Deploy (obsługa Web Deploy)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md).
- Środowisko produkcyjne jest skonfigurowane tak, aby umożliwić administratorowi ręczne zaimportowanie pakietów wdrożeniowych sieci Web do Internet Information Services (IIS), zgodnie z opisem w temacie [Scenariusz: Konfigurowanie środowiska produkcyjnego na potrzeby wdrażania w sieci Web](../configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment.md) i [Konfigurowanie serwera sieci web do publikowania Web Deploy (Wdrażanie w trybie offline)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md).

## <a name="initial-development-and-deployment"></a>Początkowe programowanie i wdrażanie

Zanim zespół Fabrikam, Inc. programistyczny będzie mógł wdrożyć rozwiązanie Contact Manager po raz pierwszy, musi wykonać następujące zadania:

- Utwórz nowy projekt zespołowy w programie TFS.
- Utwórz pliki projektu Microsoft Build Engine (MSBuild), które zawierają logikę wdrażania.
- Utwórz definicje kompilacji TFS, które wyzwalają procesy wdrażania.

### <a name="create-a-new-team-project"></a>Utwórz nowy projekt zespołowy

- Administrator TFS, Rob Walters, tworzy nowy projekt zespołowy dla aplikacji, zgodnie z opisem w temacie [Tworzenie projektu zespołowego w programie TFS](../configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs.md). Następnie deweloper lidera, hink matowy, tworzy rozwiązanie szkieletowe. Sprawdza pliki w nowym projekcie zespołowym w programie TFS, zgodnie z opisem w temacie [Dodawanie zawartości do kontroli źródła](../configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control.md).

### <a name="create-the-deployment-logic"></a>Tworzenie logiki wdrażania

Hink matowy tworzy różne pliki niestandardowego projektu programu MSBuild przy użyciu podejścia rozdzielonego pliku projektu opisanego w artykule [Omówienie pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Tworzenie otoczki:

- Plik projektu o nazwie *Publish. proj* , który uruchamia proces wdrożenia. Ten plik zawiera elementy docelowe programu MSBuild, które kompilują projekty w rozwiązaniu, tworzą pakiety sieci Web i wdrażają pakiety w środowisku serwera docelowego.
- Pliki projektu specyficzne dla środowiska o nazwie *ENV-dev. proj* i *ENV-Stage. proj*. Zawierają one ustawienia specyficzne dla środowiska testowego i środowiska przejściowego, takie jak ciągi połączeń, punkty końcowe usługi i szczegóły usługi zdalnej, która będzie odbierać pakiet sieci Web. Aby uzyskać wskazówki dotyczące wybierania odpowiednich ustawień dla konkretnych środowisk docelowych, zobacz [Konfigurowanie właściwości wdrożenia dla środowiska docelowego](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).

Aby uruchomić wdrożenie, użytkownik wykonuje plik *Publish. proj* przy użyciu programu MSBuild lub kompilacji zespołu i określa lokalizację odpowiedniego pliku projektu specyficznego dla środowiska (*ENV-dev. proj* lub *ENV-Stage. proj*) jako argument wiersza polecenia. Plik *Publish. proj* importuje następnie plik projektu specyficzny dla środowiska, aby utworzyć kompletny zestaw instrukcji publikowania dla każdego środowiska docelowego.

> [!NOTE]
> Sposób działania tych niestandardowych plików projektu jest niezależny od mechanizmu używanego do wywoływania programu MSBuild. Na przykład można użyć wiersza polecenia MSBuild bezpośrednio, zgodnie z opisem w artykule [Omówienie pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Pliki projektu można uruchomić z pliku poleceń, zgodnie z opisem w temacie [Tworzenie i uruchamianie pliku poleceń wdrożenia](../web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file.md). Alternatywnie można uruchomić pliki projektu z definicji kompilacji w programie TFS, zgodnie z opisem w temacie [Tworzenie definicji kompilacji, która obsługuje wdrażanie](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).  
> W każdym przypadku wynikiem jest ten sam&#x2014;pakiet MSBuild, który wykonuje scalony plik projektu i wdraża rozwiązanie w środowisku docelowym. Zapewnia to doskonałą elastyczność w sposobie wyzwalania procesu publikowania.

Po utworzeniu niestandardowych plików projektu otoczka dodaje je do folderu rozwiązania i sprawdza je w kontroli źródła.

### <a name="create-build-definitions"></a>Tworzenie definicji kompilacji

Jako ostateczne zadanie przygotowania, matowy i Rob współpracują ze sobą, aby utworzyć trzy definicje kompilacji dla nowego projektu zespołowego:

- **DeployToTest**. To kompiluje rozwiązanie Contact Manager i wdraża je w środowisku testowym przy każdym wystąpieniu zaewidencjonowania.
- **DeployToStaging**. Spowoduje to wdrożenie zasobów z określonej poprzedniej kompilacji w środowisku przejściowym, gdy deweloper zakolejkuje kompilację.
- **DeployToStaging — whatIf**. Wykonuje to wdrażanie w środowisku przejściowym, gdy deweloper zakolejkuje kompilację.

Poniższe sekcje zawierają więcej szczegółów na temat każdej z tych definicji kompilacji.

## <a name="deployment-to-test"></a>Wdrożenie do przetestowania

Zespół programistyczny w firmie Fabrikam, Inc. obsługuje środowiska testowe, aby przeprowadzić różne działania związane z testowaniem oprogramowania, takie jak weryfikacja i weryfikacja, testowanie użyteczności, testowanie zgodności i badania ad hoc.

Zespół programistyczny utworzył definicję kompilacji w programie TFS o nazwie **DeployToTest**. Ta definicja kompilacji używa wyzwalacza ciągłej integracji, co oznacza, że proces kompilacji jest uruchamiany za każdym razem, gdy członek zespołu Fabrikam, Inc. programowanie wykonuje zaewidencjonowanie. Gdy kompilacja jest wyzwalana, definicja kompilacji będzie:

- Kompiluj rozwiązanie ContactManager. sln. To z kolei kompiluje każdy projekt w ramach rozwiązania.
- Uruchom wszystkie testy jednostkowe w strukturze folderów rozwiązań (Jeśli rozwiązanie zostanie pomyślnie skompilowane).
- Uruchom pliki projektu niestandardowego kontrolujące proces wdrażania (Jeśli rozwiązanie zostanie pomyślnie skompilowane i przeszedł wszystkie testy jednostkowe).

Wynikiem tego jest to, że jeśli rozwiązanie zostanie pomyślnie skompilowane i przejdzie testy jednostkowe, pakiety sieci Web i inne zasoby wdrożenia są wdrażane w środowisku testowym.

![](application-lifecycle-management-from-development-to-production/_static/image3.png)

### <a name="how-does-the-deployment-process-work"></a>Jak działa proces wdrażania?

Definicja kompilacji **DeployToTest** dostarcza następujące argumenty do programu MSBuild:

[!code-console[Main](application-lifecycle-management-from-development-to-production/samples/sample1.cmd)]

Właściwości **DeployOnBuild = true** i **DeployTarget =** są używane, gdy Kompilacja zespołu kompiluje projekty w ramach rozwiązania. Gdy projekt jest projektem aplikacji sieci Web, te właściwości instruują MSBuild, aby utworzyć pakiet wdrożeniowy sieci Web dla projektu. Właściwość **TargetEnvPropsFile** informuje plik *Publish. proj* , gdzie znaleźć plik projektu specyficzny dla środowiska do zaimportowania.

> [!NOTE]
> Aby uzyskać szczegółowy przewodnik dotyczący sposobu tworzenia definicji kompilacji podobnej do tego, zobacz [Tworzenie definicji kompilacji, która obsługuje wdrażanie](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).

Plik *Publish. proj* zawiera elementy docelowe, które kompilują każdy projekt w rozwiązaniu. Jednak zawiera również logikę warunkową, która pomija te cele kompilacji, jeśli jest wykonywany plik w kompilacji zespołu. Dzięki temu można korzystać z dodatkowych funkcji kompilacji oferowanych przez kompilację zespołową, takich jak możliwość uruchamiania testów jednostkowych. Jeśli kompilacja rozwiązania lub testy jednostkowe zakończą się niepowodzeniem, plik *Publish. proj* nie zostanie wykonany i aplikacja nie zostanie wdrożona.

Logika warunkowa jest realizowana poprzez obliczenie właściwości **BuildingInTeamBuild** . Jest to właściwość programu MSBuild, która jest automatycznie ustawiana na **wartość true** w przypadku kompilowania projektów przy użyciu kompilacji zespołowej.

## <a name="deployment-to-staging"></a>Wdrażanie do przemieszczania

Gdy kompilacja spełnia wszystkie wymagania zespołu deweloperów w środowisku testowym, zespół może chcieć wdrożyć tę samą kompilację w środowisku przejściowym. Środowiska tymczasowe są zwykle skonfigurowane tak, aby były zgodne z charakterystyką środowiska produkcyjnego lub "na żywo" tak jak to możliwe, na przykład w przypadku specyfikacji serwera, systemów operacyjnych i oprogramowania oraz konfiguracji sieci. Środowiska przejściowe są często używane do testowania obciążenia, testowania akceptacji użytkowników i szerszych przeglądów wewnętrznych. Kompilacje są wdrażane w środowisku przejściowym bezpośrednio z serwera kompilacji.

![](application-lifecycle-management-from-development-to-production/_static/image4.png)

Definicje kompilacji używane do wdrożenia rozwiązania w środowisku przejściowym, **DeployToStaging-whatIf** i **DeployToStaging**, mają następujące cechy:

- Nie kompilują niczego. Gdy Rob wdraża rozwiązanie w środowisku przejściowym, chce wdrożyć konkretną istniejącą kompilację, która została już zweryfikowana i sprawdzona w środowisku testowym. Definicje kompilacji wymagają tylko uruchomienia niestandardowych plików projektu, które kontrolują proces wdrażania.
- Gdy Rob wyzwala kompilację, używa parametrów kompilacji, aby określić, która kompilacja zawiera zasoby, które chcą wdrożyć z serwera kompilacji.
- Definicje kompilacji nie są wyzwalane automatycznie. Rob ręcznie kolejkuje kompilację, gdy chce wdrożyć rozwiązanie w środowisku przejściowym.

Jest to proces wysokiego poziomu dla wdrożenia w środowisku przejściowym:

1. Administrator środowiska tymczasowego, Rob Walters, kolejkuje kompilację przy użyciu definicji kompilacji **DeployToStaging-whatIf** . Rob korzysta z parametrów definicji kompilacji, aby określić kompilację, która ma zostać wdrożona.
2. Definicja kompilacji **DeployToStaging-whatIf** uruchamia niestandardowe pliki projektu w trybie "co jeśli". Spowoduje to wygenerowanie plików dziennika, tak jakby Rob wykonywał wdrożenie na żywo, ale nie wprowadza żadnych zmian w środowisku docelowym.
3. Rob przegląda pliki dziennika, aby upewnić się, że wdrożenia w środowisku przejściowym. W szczególności Rob chce sprawdzić, co zostanie dodane, co zostanie zaktualizowane i co zostanie usunięte.
4. Jeśli Rob jest przekonany, że wdrożenie nie spowoduje żadnych niepożądanych zmian istniejących zasobów lub danych, ustawia kompilację przy użyciu definicji kompilacji **DeployToStaging** .
5. W definicji kompilacji **DeployToStaging** są uruchamiane niestandardowe pliki projektu. Umożliwiają one Publikowanie zasobów wdrożenia na podstawowym serwerze sieci Web w środowisku przejściowym.
6. Kontroler Web Farm Framework (WFF) synchronizuje serwery sieci Web w środowisku przejściowym. Dzięki temu aplikacja jest dostępna na wszystkich serwerach sieci Web w farmie serwerów.

### <a name="how-does-the-deployment-process-work"></a>Jak działa proces wdrażania?

Definicja kompilacji **DeployToStaging** dostarcza następujące argumenty do programu MSBuild:

[!code-console[Main](application-lifecycle-management-from-development-to-production/samples/sample2.cmd)]

Właściwość **TargetEnvPropsFile** informuje plik *Publish. proj* , gdzie znaleźć plik projektu specyficzny dla środowiska do zaimportowania. Właściwość **OutputRoot** przesłania wartość wbudowaną i wskazuje lokalizację folderu kompilacji zawierającego zasoby, które mają zostać wdrożone. Gdy Rob kolejkuje kompilację, używa karty **Parametry** , aby podać zaktualizowaną wartość właściwości **OutputRoot** .

![](application-lifecycle-management-from-development-to-production/_static/image5.png)

> [!NOTE]
> Aby uzyskać więcej informacji na temat tworzenia definicji kompilacji podobnej do tego, zobacz [wdrażanie określonej kompilacji](../configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build.md).

Definicja kompilacji **DeployToStaging-whatIf** zawiera tę samą logikę wdrażania, co definicja kompilacji **DeployToStaging** . Jednak zawiera dodatkowy argument **whatIf = true**:

[!code-console[Main](application-lifecycle-management-from-development-to-production/samples/sample3.cmd)]

W pliku *Publish. proj* Właściwość **whatIf** wskazuje, że wszystkie zasoby wdrożenia powinny być publikowane w trybie "co IF". Inaczej mówiąc, pliki dziennika są generowane tak, jakby wdrożenie było już w przeszłości, ale w środowisku docelowym nie ma żadnych zmian. Dzięki temu można oszacować wpływ proponowanego wdrożenia&#x2014;, co zostanie dodane, co zostanie zaktualizowane i co zostanie usunięte&#x2014;przed faktycznym wprowadzeniem jakichkolwiek zmian.

> [!NOTE]
> Aby uzyskać więcej informacji na temat konfigurowania wdrożeń "co jeśli", zobacz [wykonywanie wdrożenia "What If"](../advanced-enterprise-web-deployment/performing-a-what-if-deployment.md).

Po wdrożeniu aplikacji na podstawowym serwerze sieci Web w środowisku przejściowym program WFF automatycznie zsynchronizuje aplikację na wszystkich serwerach w farmie serwerów.

> [!NOTE]
> Aby uzyskać więcej informacji na temat konfigurowania WFF do synchronizowania serwerów sieci Web, zobacz [tworzenie farmy serwerów za pomocą struktury farmy sieci Web](../configuring-server-environments-for-web-deployment/creating-a-server-farm-with-the-web-farm-framework.md).

## <a name="deployment-to-production"></a>Wdrażanie w środowisku produkcyjnym

Gdy kompilacja została zatwierdzona w środowisku przejściowym, firma Fabrikam, Inc. Team może opublikować aplikację w środowisku produkcyjnym. Środowisko produkcyjne to miejsce, w którym aplikacja działa "na żywo" i dociera do nich docelowych użytkowników końcowych.

Środowisko produkcyjne znajduje się w sieci obwodowej połączonej z Internetem. Jest to odizolowane od sieci wewnętrznej zawierającej serwer kompilacji. Administrator środowiska produkcyjnego, Lisa Andrews, musi ręcznie skopiować pakiety wdrażania sieci Web z serwera kompilacji i zaimportować je do usług IIS na podstawowym serwerze produkcyjnym sieci Web.

![](application-lifecycle-management-from-development-to-production/_static/image6.png)

Jest to proces wysokiego poziomu dla wdrożenia w środowisku produkcyjnym:

1. Zespół deweloperów doradza Lisa, że kompilacja jest gotowa do wdrożenia w środowisku produkcyjnym. Zespół doradza Lisa lokalizacji pakietów wdrożeniowych sieci Web w folderze docelowym na serwerze kompilacji.
2. Lisa zbiera pakiety internetowe z serwera kompilacji i kopiuje je na podstawowy serwer sieci Web w środowisku produkcyjnym.
3. Lisa używa Menedżera usług IIS do importowania i publikowania pakietów sieci Web na podstawowym serwerze sieci Web.
4. Kontroler WFF synchronizuje serwery sieci Web w środowisku produkcyjnym. Dzięki temu aplikacja jest dostępna na wszystkich serwerach sieci Web w farmie serwerów.

### <a name="how-does-the-deployment-process-work"></a>Jak działa proces wdrażania?

Menedżer usług IIS zawiera Kreatora importowania pakietu aplikacji, który ułatwia publikowanie pakietów sieci Web w witrynie sieci Web usług IIS. Aby zapoznać się z przewodnikiem dotyczącym wykonywania tej procedury, zobacz [Ręczne instalowanie pakietów internetowych](../web-deployment-in-the-enterprise/manually-installing-web-packages.md).

## <a name="conclusion"></a>Podsumowanie

W tym temacie przedstawiono ilustrację cyklu życia wdrożenia typowej aplikacji sieci Web w skali korporacyjnej.

Ten temat stanowi część szeregu samouczków, które zapewniają wskazówki dotyczące różnych aspektów wdrażania aplikacji sieci Web. W ramach tej procedury istnieją wiele dodatkowych zadań i zagadnień na każdym etapie procesu wdrażania. nie jest możliwe pokryć ich wszystkie w jednym przewodniku. Aby uzyskać więcej informacji, zapoznaj się z następującymi samouczkami:

- [Wdrażanie w sieci Web w przedsiębiorstwie](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Ten samouczek zawiera kompleksowe wprowadzenie do technik wdrażania w sieci Web przy użyciu programu MSBuild i narzędzia Web Deployment (Web Deploy) usług IIS.
- [Konfigurowanie środowisk serwera na potrzeby wdrażania w sieci Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). Ten samouczek zawiera wskazówki dotyczące sposobu konfigurowania środowisk systemu Windows Server w celu obsługi różnych scenariuszy wdrażania.
- [Konfigurowanie Team Foundation Server zautomatyzowanego wdrażania w sieci Web](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Ten samouczek zawiera wskazówki dotyczące integrowania logiki wdrażania z procesami kompilacji programu TFS.
- [Zaawansowane wdrażanie w sieci Web dla przedsiębiorstw](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). Ten samouczek zawiera wskazówki dotyczące sposobu zaspokajania niektórych bardziej złożonych wyzwań związanych z wdrażaniem, które są dostępne w organizacji.

> [!div class="step-by-step"]
> [Wstecz](enterprise-web-deployment-scenario-overview.md)

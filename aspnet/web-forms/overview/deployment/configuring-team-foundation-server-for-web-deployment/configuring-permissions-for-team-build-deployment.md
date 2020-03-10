---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-permissions-for-team-build-deployment
title: Konfigurowanie uprawnień do wdrożenia kompilacji zespołowej | Microsoft Docs
author: jrjlee
description: W tym temacie opisano sposób konfigurowania uprawnień umożliwiających serwerowi kompilacji wdrażanie zawartości na serwerach sieci Web i serwerach baz danych w ramach zautomatyzowanej części b...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 2488a91e-b0a8-465a-b874-3233f724b56b
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-permissions-for-team-build-deployment
msc.type: authoredcontent
ms.openlocfilehash: 5699f72af6b8d7f18d1a2c631dfdedd63c66e1e6
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78638426"
---
# <a name="configuring-permissions-for-team-build-deployment"></a>Konfigurowanie uprawnień dla wdrożenia kompilacji zespołowej

Autor [Jason Lewandowski](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie opisano, jak skonfigurować uprawnienia umożliwiające serwerowi kompilacji wdrażanie zawartości na serwerach sieci Web i serwerach baz danych w ramach zautomatyzowanego procesu kompilacji.

Ten temat stanowi część szeregu samouczków opartych na wymaganiach dotyczących wdrażania w przedsiębiorstwie fikcyjnej firmy o nazwie Fabrikam, Inc. W tej serii samouczków jest stosowane&#x2014;przykładowe rozwiązanie&#x2014;do reprezentowania aplikacji sieci Web, [które ma](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)realistyczny poziom złożoności, w tym aplikacji ASP.NET MVC 3, usługi Windows Communication Foundation (WCF) i projektu bazy danych.

Metoda wdrażania w tym samouczku jest oparta na rozłożeniu pliku projektu dzielonego opisanym w artykule [Omówienie pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), w którym proces kompilacji jest kontrolowany przez dwa pliki&#x2014;projektu, zawierający instrukcje kompilacji, które mają zastosowanie do każdego środowiska docelowego, oraz jeden zawierający ustawienia kompilacji i wdrożenia specyficznego dla środowiska. W czasie kompilacji plik projektu specyficzny dla środowiska jest scalany z plikiem projektu Environment-niezależny od w celu utworzenia kompletnego zestawu instrukcji kompilacji.

## <a name="task-overview"></a>Przegląd zadania

Podczas instalowania usługi kompilacji Team Foundation Server (TFS) 2010 należy określić tożsamość, z którą usługa ma zostać uruchomiona. Domyślnie jest to konto usługi sieciowej. Alternatywnie można skonfigurować usługę kompilacji do uruchamiania przy użyciu konta domeny.

Wszystkie zadania wdrażania, które wymagają uwierzytelniania systemu Windows i planujesz zautomatyzować za pomocą kompilacji zespołu, będą uruchamiane przy użyciu tożsamości usługi kompilacji. W związku z tym należy przyznać tożsamości usługi kompilacji wszelkie wymagane uprawnienia na serwerach sieci Web i serwerach baz danych.

> [!NOTE]
> Konto usługi sieciowej używa konta komputera do uwierzytelniania na innych komputerach. Konta komputerów przyjmują postać * [Domain Name]\[Machine Name] * **$** &#x2014;na przykład **FABRIKAM\TFSBUILD $** . W związku z tym, jeśli usługa kompilacji jest uruchomiona przy użyciu tożsamości usługi sieciowej, należy przyznać wszelkie wymagane uprawnienia do tożsamości konta komputera dla serwera kompilacji.

## <a name="configuring-web-server-permissions"></a>Konfigurowanie uprawnień serwera sieci Web

Zgodnie z opisem w temacie [Wybieranie odpowiedniego podejścia do wdrażania w sieci Web](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md), istnieją dwa główne podejścia, których można użyć, jeśli chcesz wdrożyć pakiety internetowe na zdalnym serwerze sieci Web:

- Wdróż aplikację z lokalizacji zdalnej, przeznaczoną dla *usługi Deployment Agent sieci Web* (znanej również jako agent zdalny) na serwerze docelowym.
- Wdróż aplikację z lokalizacji zdalnej, przeznaczoną dla*programu obsługi Web Deploy* *Internet Information Services* (IIS) na serwerze docelowym.

W takim przypadku Agent zdalny ma dwa kluczowe ograniczenia:

- Agent zdalny obsługuje tylko uwierzytelnianie NTLM. Innymi słowy, wdrożenie musi używać tożsamości&#x2014;usługi kompilacji, nie można spersonifikować innego konta.
- Aby można było korzystać z agenta zdalnego, konto, które wykonuje wdrożenie, musi być administratorem na serwerze docelowym.

Ze sobą te dwa ograniczenia sprawiają, że podejście zdalnego agenta jest niepożądane dla zautomatyzowanego wdrożenia kompilacji zespołowej. Aby skorzystać z tej metody, należy utworzyć konto usługi kompilacji jako administrator na dowolnym docelowym serwerze sieci Web.

Z kolei podejście do procedury obsługi Web Deploy oferuje różne zalety:

- Obsługa Web Deploy obsługuje uwierzytelnianie podstawowe za pośrednictwem protokołu HTTPS, co umożliwia przekazywanie poświadczeń alternatywnego konta do narzędzia Web Deployment (Web Deploy) usług IIS.
- Można skonfigurować docelowe serwery sieci Web, aby umożliwić użytkownikom innym niż administrator wdrażanie zawartości do określonych witryn sieci Web usług IIS przy użyciu programu obsługi Web Deploy.

W związku z tym jest wyraźnie preferowany dla programu obsługi Web Deploy podczas automatyzowania wdrażania pakietu internetowego z kompilacji zespołu. Jest to zalecany proces:

1. Utwórz konto domeny z niskim poziomem uprawnień do użycia we wdrożeniu.
2. Skonfiguruj procedurę obsługi Web Deploy i przyznaj kontu uprawnienia wymagane do wdrożenia zawartości w określonej witrynie sieci Web usług IIS, zgodnie z opisem w temacie [Konfigurowanie serwera sieci Web do Web Deploy Publishing (procedura obsługi Web Deploy)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md).
3. Wywołaj Web Deploy i docelową procedurę obsługi Web Deploy, używając uwierzytelniania podstawowego i dostarczając poświadczenia utworzonego konta domeny, aby wykonać wdrożenie.

W przykładowym rozwiązaniu [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) należy określić typ uwierzytelniania (podstawowa lub NTLM), poświadczenia Web Deploy i adres punktu końcowego (program obsługi agenta zdalnego lub Web Deploy) w pliku projektu specyficznym dla środowiska. Te wartości są używane do formułowania i uruchamiania polecenia Web Deploy, gdy plik projektu jest wykonywany. Aby uzyskać więcej informacji, zobacz [wdrażanie pakietów internetowych](../web-deployment-in-the-enterprise/deploying-web-packages.md).

Aby uzyskać więcej informacji na temat konfigurowania programu obsługi Web Deploy, w tym konfigurowania uprawnień, zobacz [Konfigurowanie serwera sieci Web do Web Deploy Publishing (procedura obsługi Web Deploy)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md). Aby uzyskać więcej informacji na temat konfigurowania agenta zdalnego, zobacz [Konfigurowanie serwera sieci Web do publikowania Web Deploy (Agent zdalny)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md).

## <a name="configuring-database-server-permissions"></a>Konfigurowanie uprawnień serwera bazy danych

Aby wdrożyć bazę danych w SQL Server, musisz:

- Utwórz nazwę logowania dla konta wdrożenia w wystąpieniu SQL Server.
- Udziel uprawnienia logowanie **dbcreator** do wystąpienia SQL Server.
- Po wdrożeniu początkowym Dodaj nazwę logowania do roli **właściciela\_bazy danych** w docelowej bazie danych. Jest to wymagane, ponieważ w kolejnych wdrożeniach modyfikowana jest istniejąca baza danych zamiast tworzenia nowej bazy danych.

Można uwierzytelnić się w wystąpieniu SQL Server przy użyciu uwierzytelniania NTLM lub uwierzytelniania SQL Server:

- W przypadku korzystania z uwierzytelniania NTLM należy przyznać uprawnienia opisane powyżej do konta usługi kompilacji.
- W przypadku korzystania z uwierzytelniania SQL Server należy przyznać uprawnienia opisane powyżej do konta SQL Server. W parametrach połączenia używanych do wdrażania bazy danych należy również podać nazwę użytkownika i hasło SQL Server.

Szczegółowe informacje na temat sposobu konfigurowania uprawnień do wdrażania bazy danych znajdują się w temacie [Configuring a Database Server for Web Deploy Publishing](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).

## <a name="conclusion"></a>Podsumowanie

W tym momencie należy zrozumieć wymagane uprawnienia wraz z opcjami uwierzytelniania otwartymi dla użytkownika, podczas automatyzowania wdrożeń aplikacji sieci Web i baz danych z kompilacji zespołu. Należy również mieć możliwość zaimplementowania niezbędnych uprawnień na serwerach sieci Web usług IIS i serwerach bazy danych SQL Server.

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat konfigurowania środowisk systemu Windows Server do obsługi zdalnego wdrażania, zobacz [Konfigurowanie środowisk serwera na potrzeby wdrażania w sieci Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md).

> [!div class="step-by-step"]
> [Wstecz](deploying-a-specific-build.md)

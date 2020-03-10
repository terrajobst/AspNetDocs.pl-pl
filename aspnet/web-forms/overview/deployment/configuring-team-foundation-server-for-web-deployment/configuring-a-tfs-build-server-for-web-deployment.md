---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-a-tfs-build-server-for-web-deployment
title: Konfigurowanie serwera kompilacji TFS na potrzeby wdrażania w sieci Web | Microsoft Docs
author: jrjlee
description: W tym temacie opisano sposób przygotowania serwera kompilacji programu Team Foundation Server (TFS) do kompilowania i wdrażania rozwiązań za pomocą kompilacji zespołowej i formatu Internet...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: f8400241-4f4b-4bbd-9994-54fb64909e6e
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-a-tfs-build-server-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: b3aaf7234706d149a3c784347528923f662c3511
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78631097"
---
# <a name="configuring-a-tfs-build-server-for-web-deployment"></a>Konfigurowanie serwera kompilacji TFS na potrzeby wdrażania w Internecie

Autor [Jason Lewandowski](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie opisano sposób przygotowania serwera kompilacji Team Foundation Server (TFS) do kompilowania i wdrażania rozwiązań za pomocą kompilacji zespołowej oraz narzędzia do wdrażania w sieci Web (Web Deploy) Internet Information Services (IIS).

Ten temat stanowi część szeregu samouczków opartych na wymaganiach dotyczących wdrażania w przedsiębiorstwie fikcyjnej firmy o nazwie Fabrikam, Inc. W tej serii samouczków jest stosowane&#x2014;przykładowe rozwiązanie&#x2014;do reprezentowania aplikacji sieci Web, [które ma](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)realistyczny poziom złożoności, w tym aplikacji ASP.NET MVC 3, usługi Windows Communication Foundation (WCF) i projektu bazy danych.

Metoda wdrażania w tym samouczku jest oparta na rozłożeniu pliku projektu dzielonego opisanym w artykule [Omówienie pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), w którym proces kompilacji jest kontrolowany przez dwa pliki&#x2014;projektu, zawierający instrukcje kompilacji, które mają zastosowanie do każdego środowiska docelowego, oraz jeden zawierający ustawienia kompilacji i wdrożenia specyficznego dla środowiska. W czasie kompilacji plik projektu specyficzny dla środowiska jest scalany z plikiem projektu Environment-niezależny od w celu utworzenia kompletnego zestawu instrukcji kompilacji.

## <a name="task-overview"></a>Przegląd zadania

Aby przygotować serwer kompilacji do kompilowania i wdrażania rozwiązań, należy:

- Zainstaluj i skonfiguruj usługę kompilacji TFS.
- Zainstaluj program Visual Studio 2010.
- Zainstaluj wszelkie produkty lub składniki wymagane do skompilowania rozwiązania, takie jak wersje .NET Framework lub ASP.NET MVC.
- Zainstaluj Web Deploy 2,0 lub nowszy.

W tym temacie pokazano, jak wykonać te procedury lub wskazać inne zasoby tam, gdzie istnieją. W zadaniach i przewodnikach w tym temacie przyjęto założenie, że:

- Rozpoczynasz pracę z czystą kompilacją serwera z systemem Windows Server 2008 R2 z dodatkiem Service Pack 1.
- Serwer jest przyłączony do domeny ze statycznym adresem IP.
- Warstwa aplikacji TFS została zainstalowana na osobnym serwerze, zgodnie z opisem w artykule [wdrażanie w sieci Web: Omówienie scenariusza](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md).

### <a name="who-performs-these-procedures"></a>Kto wykonuje te procedury?

W większości przypadków administrator programu TFS będzie odpowiedzialny za konfigurowanie serwerów kompilacji. W niektórych przypadkach zespół deweloperów może przejąć własność określonych serwerów kompilacji.

## <a name="install-and-configure-the-tfs-build-service"></a>Instalowanie i Konfigurowanie usługi kompilacji TFS

Podczas konfigurowania serwera kompilacji pierwsze zadanie to zainstalowanie i skonfigurowanie usługi kompilacji TFS. W ramach tego procesu należy:

- Zainstaluj usługę kompilacji TFS i skonfiguruj konto usługi. Wszystkie zadania kompilacji, w tym wdrażanie, będą uruchamiane przy użyciu tożsamości konta usługi kompilacji.
- Utwórz *kontroler kompilacji* i jednego lub więcej *agentów kompilacji*. Każdy kontroler kompilacji zarządza zestawem agentów kompilacji. Podczas kolejkowania kompilacji, kontroler kompilacji przypisuje zadanie kompilacji do dostępnego agenta kompilacji. Każda kolekcja projektów zespołowych w programie TFS jest zamapowana na jeden kontroler kompilacji.
- Skonfiguruj folder docelowy dla danych wyjściowych kompilacji. To jest udział sieciowy. Wszystkie dane wyjściowe kompilacji, takie jak pakiety wdrażania sieci Web, są wysyłane do folderu docelowego.

Rozdział [Zarządzanie Team Foundation Build](https://msdn.microsoft.com/library/ms252495.aspx) w witrynie MSDN zawiera wszystkie zasoby, które są potrzebne do wykonywania następujących zadań:

- Aby zapoznać się z omówieniem koncepcyjnym programu Team Foundation Build, w tym usługi kompilacji, kontrolerów kompilacji i agentów kompilacji, zobacz [Opis systemu kompilacji Team Foundation](https://msdn.microsoft.com/library/dd793166.aspx).
- Aby uzyskać informacje na temat instalowania i konfigurowania usługi kompilacji, zobacz [Konfigurowanie maszyny kompilacji](https://msdn.microsoft.com/library/ms181712.aspx).
- Aby uzyskać informacje na temat tworzenia kontrolerów kompilacji, zobacz [Tworzenie i współpraca z kontrolerem kompilacji](https://msdn.microsoft.com/library/ee330987.aspx).
- Aby uzyskać informacje na temat tworzenia agentów kompilacji, zobacz [Tworzenie i współpracuj z agentami kompilacji](https://msdn.microsoft.com/library/bb399135.aspx).
- Aby uzyskać informacje na temat tworzenia i konfigurowania folderów do wrzucania, zobacz [Konfigurowanie folderów do wrzucania](https://msdn.microsoft.com/library/bb778394.aspx).

## <a name="install-required-products-and-components"></a>Zainstaluj wymagane produkty i składniki

Aby umożliwić serwerowi kompilacji Kompilowanie rozwiązań, należy zainstalować wszelkie produkty, składniki lub zestawy wymagane przez Twoje rozwiązanie. Przed zainstalowaniem składników platformy sieci Web należy zainstalować program Visual Studio 2010 (dowolna wersja) na serwerze kompilacji. Daje to pewność, że pliki docelowe programu Core Microsoft Build Engine (MSBuild) i pliki docelowe potoku publikowania w sieci Web (WPP) są dostępne dla usługi kompilacji. Instalator programu Visual Studio powinien również zainstalować Web Deploy, co będzie potrzebne, jeśli planujesz wdrożyć pakiety sieci Web w ramach procesu kompilacji.

Najlepszym sposobem na zainstalowanie wspólnych składników platformy sieci Web jest użycie [Instalatora platformy sieci Web](https://go.microsoft.com/?linkid=9805118). Dzięki temu można zainstalować najnowszą wersję każdego produktu, a także automatycznie wykrywać i instalować wszystkie wymagania wstępne dla każdego produktu. W przypadku rozwiązania [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) należy użyć Instalatora platformy sieci Web, aby zainstalować te produkty i składniki:

- **.NET Framework 4,0**. Jest to wymagane do uruchamiania aplikacji, które zostały skompilowane w tej wersji .NET Framework.
- **Web Deployment Tool 2,1 lub nowszym**. Spowoduje to zainstalowanie Web Deploy (i jego podstawowego pliku wykonywalnego, MSDeploy. exe) na serwerze. W ramach tego procesu instaluje i uruchamia usługę Deployment Agent sieci Web. Ta usługa umożliwia wdrażanie pakietów internetowych z komputera zdalnego.
- **ASP.NET MVC 3**. Spowoduje to zainstalowanie zestawów potrzebnych do uruchamiania aplikacji ASP.NET MVC 3.

**Aby zainstalować wymagane produkty i składniki**

1. Zainstaluj program Visual Studio 2010. Po wyświetleniu monitu o wybranie funkcji do zainstalowania należy uwzględnić następujące elementy:

    1. Wszystkie języki programowania, które trzeba skompilować.
    2. Visual Web Developer. Daje to pewność, że elementy docelowe WPP są dodawane do serwera kompilacji.

        ![](configuring-a-tfs-build-server-for-web-deployment/_static/image1.png)
2. Po zakończeniu instalacji programu Visual Studio 2010 Pobierz i zainstaluj [program Visual studio 2010 z dodatkiem Service Pack 1](https://go.microsoft.com/?linkid=9805133) (jeśli nie znajduje się on jeszcze na nośniku instalacyjnym).

    > [!NOTE]
    > Program Visual Studio 2010 Service Pack 1 rozpoznaje usterkę, która może uniemożliwić programowi MSBuild znalezienie pliku wykonywalnego MSDeploy.
3. Pobierz i uruchom [Instalatora platformy sieci Web](https://go.microsoft.com/?linkid=9805118).
4. W górnej części okna **Instalatora platformy sieci Web 3,0** kliknij pozycję **produkty**.
5. Po lewej stronie okna w okienku nawigacji kliknij pozycję **struktury**.
6. W wierszu **Microsoft .NET Framework 4** , jeśli .NET Framework nie jest jeszcze zainstalowany, kliknij przycisk **Dodaj**.

    > [!NOTE]
    > Być może zainstalowano już .NET Framework 4,0 do Windows Update. Jeśli produkt lub składnik jest już zainstalowany, Instalator platformy sieci Web wskaże ten element, zastępując przycisk **Dodaj** **zainstalowanym**tekstem.

    ![](configuring-a-tfs-build-server-for-web-deployment/_static/image2.png)
7. W wierszu **ASP.NET MVC 3 (Visual Studio 2010)** kliknij przycisk **Dodaj**.
8. W okienku nawigacji kliknij pozycję **serwer**.
9. W wierszu **narzędzia Web Deployment 2,1** kliknij pozycję **Dodaj**.
10. Kliknij polecenie **Zainstaluj**. W Instalatorze platformy sieci Web zostanie wyświetlona lista&#x2014;produktów wraz ze wszystkimi skojarzonymi zależnościami&#x2014;, które zostaną zainstalowane, i wyświetli monit o zaakceptowanie postanowień licencyjnych.
11. Zapoznaj się z postanowieniami licencyjnymi, a jeśli wyrażasz zgodę na warunki, kliknij przycisk **Akceptuję**.
12. Po zakończeniu instalacji kliknij przycisk **Zakończ**, a następnie zamknij okno **Instalatora platformy sieci Web 3,0** .

> [!NOTE]
> Jeśli proces wdrażania obejmuje użycie narzędzi, takich jak VSDBCMD. exe lub SQLCMD. exe, należy upewnić się, że są one zainstalowane na serwerze kompilacji. VSDBCMD. exe to narzędzie programu Visual Studio, które jest zazwyczaj dodawane do serwera podczas instalowania programu Team Foundation Build. SQLCMD. exe to narzędzie SQL Server. Autonomiczną wersję pliku SQLCMD. exe można pobrać ze strony [pakietu Feature Pack Microsoft SQL Server 2008 R2](https://go.microsoft.com/?linkid=9805134) .

## <a name="conclusion"></a>Podsumowanie

Na tym etapie serwer kompilacji jest gotowy do rozpoczęcia kompilowania i wdrażania projektów aplikacji sieci Web. W następnym temacie [Tworzenie definicji kompilacji, która obsługuje wdrożenie](creating-a-build-definition-that-supports-deployment.md), zawiera opis sposobu tworzenia i konfigurowania definicji kompilacji w celu kontrolowania, kiedy i w jaki sposób projekty są kompilowane i wdrażane.

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać bardziej ogólne wskazówki dotyczące pracy z kompilacją zespołu, zobacz [Administrowanie Team Foundation Build](https://msdn.microsoft.com/library/ms252495.aspx).

> [!div class="step-by-step"]
> [Poprzednie](adding-content-to-source-control.md)
> [dalej](creating-a-build-definition-that-supports-deployment.md)

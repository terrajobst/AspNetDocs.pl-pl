---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/manually-installing-web-packages
title: Ręczne instalowanie pakietów sieci Web | Microsoft Docs
author: jrjlee
description: W tym temacie opisano, jak ręcznie zaimportować pakiet wdrażania internetowego do Internet Information Services (IIS). Temat kompilowania i pakowania Instalacja sieci Web...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: f11d22a7-5d32-4ad0-8a9b-276460a61c06
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/manually-installing-web-packages
msc.type: authoredcontent
ms.openlocfilehash: f778549d3e26989a2e71ef21171adec521842729
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78634205"
---
# <a name="manually-installing-web-packages"></a>Ręczne instalowanie pakietów internetowych

Autor [Jason Lewandowski](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie opisano, jak ręcznie zaimportować pakiet wdrażania internetowego do Internet Information Services (IIS).
> 
> Temat [kompilowania i pakowania projektów aplikacji sieci Web](building-and-packaging-web-application-projects.md) opisany w temacie narzędzie Web Deployment (Web Deploy) IIS, w połączeniu z Microsoft Build Engine (MSBuild) i potok publikowania w sieci Web (WPP), umożliwia spakowanie projektów aplikacji sieci Web w jednym pliku zip. Ten plik, powszechnie znany jako pakiet wdrażania w sieci Web (lub po prostu pakiet wdrożeniowy), zawiera wszystkie informacje o zawartości i konfiguracji, które są wymagane przez usługi IIS w celu ponownego utworzenia aplikacji sieci Web na serwerze sieci Web.
> 
> Po utworzeniu pakietu wdrożeniowego sieci Web można go opublikować na serwerze usług IIS na różne sposoby. W wielu scenariuszach warto skorzystać z punktów integracji między programem MSBuild, WPP i Web Deploy, aby zdalnie tworzyć i instalować pakiety sieci Web w ramach zautomatyzowanego lub jednoetapowego procesu kompilowania i wdrażania. Ten proces jest opisany w temacie [wdrażanie pakietów internetowych](deploying-web-packages.md). Nie jest to jednak zawsze możliwe. Załóżmy, że chcesz wdrożyć aplikację sieci Web w środowisku produkcyjnym dostępnym z Internetu. Ze względów bezpieczeństwa takie środowisko produkcyjne jest najprawdopodobniej za zaporą w podsieci, która jest oddzielona od serwera kompilacji, w sieci obwodowej (znanej także jako DMZ, strefa zdemilitaryzowana i podsieć z osłoną). W wielu przypadkach środowisko produkcyjne będzie znajdować się w oddzielnej domenie lub w sieci fizycznie izolowanej.
> 
> W tych scenariuszach jedyną opcją może być port pakietu sieci Web na serwerze docelowym i ręczne zaimportowanie go do usług IIS. Chociaż takie podejście wyklucza Automatyczne wdrażanie, nadal jest wysoce skuteczną techniką publikowania aplikacji&#x2014;sieci Web po prostu skopiowanie pojedynczego pliku zip na serwer sieci Web i użycie kreatora, który przeprowadzi Cię przez proces importowania.

Ten temat stanowi część szeregu samouczków opartych na wymaganiach dotyczących wdrażania w przedsiębiorstwie fikcyjnej firmy o nazwie Fabrikam, Inc. W tej serii samouczków jest stosowane&#x2014;przykładowe rozwiązanie&#x2014;do reprezentowania aplikacji sieci Web, [które ma](the-contact-manager-solution.md)realistyczny poziom złożoności, w tym aplikacji ASP.NET MVC 3, usługi Windows Communication Foundation (WCF) i projektu bazy danych.

## <a name="task-overview"></a>Przegląd zadania

Należy wykonać te zadania wysokiego poziomu, aby zaimportować pakiet wdrażania internetowego do usług IIS:

- Utwórz pakiet wdrożeniowy sieci Web przy użyciu wiersza polecenia programu MSBuild, kompilacji zespołowej lub programu Visual Studio 2010.
- Skopiuj pakiet sieci Web na docelowy serwer sieci Web.
- Użyj Kreatora importowania pakietów aplikacji w Menedżerze usług IIS, aby zainstalować pakiet sieci Web i podać wartości zmiennych, takich jak ciągi połączeń i punkty końcowe usługi.

W tym temacie pokazano, jak wykonać te procedury. W zadaniach i przewodnikach w tym temacie założono, że znasz już pojęcia związane z pakietami internetowymi, Web Deploy i WPP. Aby uzyskać więcej informacji, zobacz [Kompilowanie i pakowanie projektów aplikacji sieci Web](building-and-packaging-web-application-projects.md).

> [!NOTE]
> Ten temat jest najlepiej używany w połączeniu z [konfigurowaniem serwera sieci Web na potrzeby publikowania Web Deploy (wdrożenie w trybie offline)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md), w którym wyjaśniono, jak zainstalować wymagane składniki i przygotować witrynę sieci Web usług IIS do importowania pakietów.

## <a name="create-a-web-deployment-package"></a>Tworzenie pakietu wdrożeniowego sieci Web

Pierwsze zadanie polega na utworzeniu pakietu wdrożeniowego sieci Web dla projektu aplikacji sieci Web, który ma zostać wdrożony. Pakiety internetowe można tworzyć na wiele sposobów.

**Podejście 1: Tworzenie pakietu jako części procesu kompilacji przy użyciu programu Visual Studio**

Możesz skonfigurować projekt aplikacji sieci Web, aby utworzyć pakiet wdrożeniowy sieci Web po każdej kompilacji za pomocą karty **pakowanie/publikowanie w sieci Web** na stronach właściwości projektu. Ten proces jest opisany w temacie [Tworzenie i pakowanie projektów aplikacji sieci Web](building-and-packaging-web-application-projects.md).

**Podejście 2. Tworzenie pakietu jako części procesu kompilacji przy użyciu programu MSBuild**

Jeśli tworzysz projekt aplikacji sieci Web za pomocą programu MSBuild bezpośrednio, za pośrednictwem niestandardowego pliku projektu programu MSBuild lub z wiersza polecenia, możesz utworzyć pakiet wdrożeniowy sieci Web jako część procesu kompilacji, dołączając właściwości **DeployOnBuild = true** i **DeployTarget = Package** w poleceniu. Ten proces jest opisany w [opisie procesu kompilacji](understanding-the-build-process.md).

**Podejście 3: Tworzenie pakietu na żądanie w programie Visual Studio**

W dowolnym momencie można utworzyć pakiet wdrożeniowy sieci Web dla projektu aplikacji sieci Web w programie Visual Studio 2010. W tym celu w oknie **Eksplorator rozwiązań** kliknij prawym przyciskiem myszy projekt aplikacji sieci Web, a następnie kliknij polecenie **Kompiluj pakiet wdrożeniowy**.

![](manually-installing-web-packages/_static/image1.png)

**Podejście 4: Tworzenie pakietu na żądanie z poziomu wiersza polecenia**

Pakiet wdrożeniowy sieci Web można utworzyć z poziomu wiersza polecenia, wywołując obiekt docelowy **pakietu** w projekcie aplikacji sieci Web przy użyciu programu MSBuild. Polecenie powinno wyglądać następująco:

[!code-console[Main](manually-installing-web-packages/samples/sample1.cmd)]

W zależności od używanej metody wynik końcowy jest taki sam. WPP tworzy pakiet wdrożeniowy sieci Web jako plik zip, wraz z różnymi zasobami pomocniczymi, w folderze wyjściowym projektu aplikacji sieci Web.

![](manually-installing-web-packages/_static/image2.png)

Po zaplanowaniu ręcznego zaimportowania pakietu sieci Web wymagany jest tylko plik zip. Skopiuj ten plik na docelowy serwer sieci Web i możesz rozpocząć proces importowania.

## <a name="import-a-web-package-into-iis"></a>Importowanie pakietu internetowego do usług IIS

Przy użyciu następnej procedury można zaimportować pakiet wdrażania internetowego z lokalnego systemu plików do witryny sieci Web usług IIS. Przed wykonaniem tej procedury upewnij się, że masz:

- Skopiowano pakiet wdrożeniowy sieci Web na serwer sieci Web.
- Skonfigurowano serwer sieci Web usług IIS do hostowania aplikacji.

Aby uzyskać więcej informacji na temat konfigurowania serwera sieci Web usług IIS do obsługi pakietów wdrożeniowych sieci Web, zobacz [Konfigurowanie serwera sieci Web do publikowania Web Deploy (Wdrażanie w trybie offline)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md).

**Aby zaimportować pakiet wdrożeniowy sieci Web przy użyciu Menedżera usług IIS**

1. W Menedżerze usług IIS w okienku **połączenia** kliknij prawym przyciskiem myszy witrynę sieci Web usług IIS, wskaż polecenie **Wdróż**, a następnie kliknij polecenie **Importuj aplikację**.

    ![](manually-installing-web-packages/_static/image3.png)
2. W Kreatorze importowania pakietu aplikacji na stronie **Wybierz pakiet** przejdź do lokalizacji pakietu wdrożeniowego sieci Web, a następnie kliknij przycisk **dalej**.
3. Na stronie **Wybierz zawartość pakietu** Wyczyść dowolną niewymaganą zawartość, a następnie kliknij przycisk **dalej**.

    ![](manually-installing-web-packages/_static/image4.png)

    > [!NOTE]
    > W wielu przypadkach możesz nie chcieć zaimportować wszystkich elementów, które są dostarczane z pakietem wdrożeniowym sieci Web. Na przykład możesz nie chcieć zezwalać Web Deploy na zastępowanie skojarzonej bazy danych.  
    > Uprawnienia **przyznawania uprawnień** ustawiają ustawienia uprawnień w docelowym systemie plików, aby mieć pewność, że tożsamość puli aplikacji ma dostęp do folderu fizycznego, w którym jest przechowywana zawartość witryny sieci Web. Ponadto użytkownik uwierzytelniania anonimowego ma przyznane uprawnienie do odczytu w folderze, aby umożliwić aplikacji dostęp do plików typu MIME (Multipurpose Internet Mail Extensions). Jeśli wolisz, możesz usunąć te wpisy i ręcznie skonfigurować uprawnienia.
4. Na stronie **Wprowadź informacje o pakiecie aplikacji** podaj żądane informacje.

    ![](manually-installing-web-packages/_static/image5.png)
5. Podczas tworzenia pakietu sieci Web WPP analizuje plik konfiguracyjny aplikacji i wykrywa wszelkie zmienne, takie jak ciągi połączeń i punkty końcowe usługi. W takim przypadku:

    1. **Ścieżka aplikacji** to ścieżka IIS, w której chcesz zainstalować aplikację. To ustawienie jest wspólne dla wszystkich pakietów wdrożeniowych tworzonych przez WPP.
    2. **Adres punktu końcowego usługi ContactService** to adres, którego aplikacja ma używać do komunikacji z wdrożoną usługą WCF. To ustawienie odnosi się do wpisu w pliku *Web. config* .
    3. Pierwsze ustawienie **parametrów połączenia** to parametry połączenia, których Web Deploy należy użyć do wdrożenia bazy danych skojarzonej z aplikacją (w tym przypadku bazy danych członkostwa ASP.NET). To ustawienie odpowiada ustawieniu na karcie **pakiet/publikacja SQL** w programie Visual Studio.
    4. Drugie **Parametry połączenia** to parametry połączenia, które będą używane przez aplikację do komunikacji z bazą danych po jej uruchomieniu. Odnosi się to do wpisu parametrów połączenia w pliku *Web. config* .

        > [!NOTE]
        > Aby uzyskać więcej informacji o tym, skąd pochodzą te parametry, zobacz [Konfigurowanie parametrów do wdrażania pakietów sieci Web](configuring-parameters-for-web-package-deployment.md).
6. Kliknij przycisk **Dalej**.
7. Jeśli aplikacja została wdrożona po raz pierwszy, zostanie wyświetlony monit o określenie, czy przed instalacją ma zostać usunięta cała istniejąca zawartość. Wybierz opcję odpowiednią dla wymagań, a następnie kliknij przycisk **dalej**.

    ![](manually-installing-web-packages/_static/image6.png)
8. Po zakończeniu instalacji pakietu przez usługi IIS kliknij przycisk **Zakończ**.

    ![](manually-installing-web-packages/_static/image7.png)

W tym momencie aplikacja sieci Web została pomyślnie opublikowana w usługach IIS.

## <a name="conclusion"></a>Podsumowanie

W tym temacie opisano sposób importowania pakietu wdrażania internetowego do witryny sieci Web usług IIS przy użyciu Menedżera usług IIS. Takie podejście do publikowania aplikacji sieci Web jest odpowiednie, gdy ograniczenia dotyczące zabezpieczeń lub infrastruktury sprawiają, że zdalne wdrożenie jest niemożliwe lub niepożądane.

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać wskazówki dotyczące konfigurowania serwera sieci Web usług IIS do obsługi ręcznego importowania pakietu sieci Web, zobacz [Konfigurowanie serwera sieci Web do publikowania Web Deploy (Wdrażanie w trybie offline)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md). Aby uzyskać ogólne wskazówki dotyczące wdrażania pakietów sieci Web, zobacz [Przewodnik: wdrażanie projektu aplikacji sieci Web przy użyciu pakietu wdrożeniowego sieci Web (część 1 z 4)](https://msdn.microsoft.com/library/dd483479.aspx).

> [!div class="step-by-step"]
> [Wstecz](creating-and-running-a-deployment-command-file.md)

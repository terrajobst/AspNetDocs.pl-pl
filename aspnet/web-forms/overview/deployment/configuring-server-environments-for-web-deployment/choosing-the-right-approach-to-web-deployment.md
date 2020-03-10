---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment
title: Wybieranie odpowiedniego podejścia do wdrażania w sieci Web | Microsoft Docs
author: jrjlee
description: Podczas pracy z Internet Information Services (IIS) narzędzia do wdrażania w sieci Web (Web Deploy) 2,0 lub nowszym istnieją trzy główne podejścia, których można użyć do uzyskania...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 787a53fd-9901-4a11-9d58-61e0509cda45
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 13f784dd8e6404806104d56b026b3c41ca178892
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78548483"
---
# <a name="choosing-the-right-approach-to-web-deployment"></a>Wybieranie właściwego podejścia do wdrażania w Internecie

Autor [Jason Lewandowski](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Podczas pracy z Internet Information Services (IIS) narzędzia do wdrażania w sieci Web (Web Deploy) 2,0 lub nowszym istnieją trzy główne podejścia, których można użyć w celu uzyskania spakowanych aplikacji sieci Web na serwerze sieci Web. Można:
> 
> - Wdróż aplikację z lokalizacji zdalnej, przeznaczoną dla *usługi Deployment Agent sieci Web* (znanej również jako "zdalny Agent") na serwerze docelowym.
> - Wdróż aplikację z lokalizacji zdalnej przy użyciu Web Deploy na żądanie (znanego również jako "tymczasowy Agent").
> - Wdróż aplikację z lokalizacji zdalnej, przeznaczoną dla programu *obsługi Web Deploy IIS* na serwerze docelowym.
> - Wdróż aplikację, ręcznie kopiując pakiet sieci Web na serwer docelowy i importując go za pomocą Menedżera usług IIS.
> 
> Sposób konfigurowania docelowych serwerów sieci Web zależy od tego, które podejście ma być używane. Ten temat pomoże Ci zdecydować, które podejście do wdrożenia jest odpowiednie dla Ciebie.

W tej tabeli przedstawiono główne zalety i wady poszczególnych rozwiązań wdrażania, a także scenariusze, które najczęściej są odpowiednie dla każdego podejścia.

| Podejście | Zalety | Wady | Typowe scenariusze |
| --- | --- | --- | --- |
| Agent zdalny | Jest to łatwe do skonfigurowania. Jest to odpowiednie dla regularnych aktualizacji aplikacji sieci Web i zawartości. | Użytkownik musi być administratorem na serwerze docelowym. Użytkownik nie może dostarczyć alternatywnych poświadczeń. | Środowiska deweloperskie. Środowiska testowe. |
| Agent tymczasowy | Nie ma potrzeby instalowania Web Deploy na komputerze docelowym. Zostanie automatycznie użyta Najnowsza wersja Web Deploy. | Użytkownik musi być administratorem na serwerze docelowym. Użytkownik nie może dostarczyć alternatywnych poświadczeń. | Środowiska deweloperskie. Środowiska testowe. |
| Procedura obsługi Web Deploy | Użytkownicy niebędący administratorami mogą wdrażać zawartość. Jest to odpowiednie dla regularnych aktualizacji aplikacji sieci Web i zawartości. | Konfiguracja jest znacznie bardziej złożona. | Środowiska przejściowe. Intranetowe środowiska produkcyjne. Środowiska hostowane. |
| Wdrażanie w trybie offline | Konfiguracja jest bardzo łatwa. Jest to odpowiednie dla środowisk izolowanych. | Administrator serwera musi ręcznie skopiować i zaimportować pakiet sieci Web za każdym razem. | Środowiska produkcyjne dostępne w Internecie. Izolowane środowiska sieciowe. |

## <a name="using-the-remote-agent"></a>Korzystanie z zdalnego agenta

W przypadku instalowania Web Deploy przy użyciu ustawień domyślnych na serwerze docelowym Usługa Deployment Agent sieci Web ("zdalny Agent") zostanie automatycznie zainstalowana i uruchomiona. Domyślnie agent zdalny uwidacznia punkt końcowy HTTP pod tym adresem:

[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample1.cmd)]

> [!NOTE]
> Można zastąpić wartość [*Server*] nazwą komputera serwera sieci Web, adresem IP serwera sieci Web lub nazwą hosta, który jest rozpoznawany jako serwer sieci Web.

Administratorzy serwera mogą wdrażać pakiety internetowe z lokalizacji zdalnej, na przykład na komputerze dewelopera lub na serwerze kompilacji, określając ten adres punktu końcowego. Załóżmy na przykład, że hink matowy w firmie Fabrikam, Inc. utworzył projekt aplikacji sieci Web ContactManager. MVC na komputerze dewelopera. Proces kompilacji generuje pakiet sieci Web wraz z plikiem *. deploy. cmd* zawierającym polecenia Web Deploy wymagane do zainstalowania pakietu. Jeśli matowy jest administratorem serwera na serwerze TESTWEB1, może wdrożyć aplikację sieci Web na testowanym serwerze sieci Web przez uruchomienie tego polecenia na komputerze dewelopera:

[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample2.cmd)]

W rzeczywistości plik wykonywalny Web Deploy może wywnioskować adres punktu końcowego agenta zdalnego, jeśli podano nazwę komputera, więc otoczka musi wpisać:

[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample3.cmd)]

> [!NOTE]
> Aby uzyskać więcej informacji na temat Web Deploy składni wiersza polecenia i plików *. deploy. cmd* , zobacz [How to: Install a Deployment Package using the Deploy. cmd](https://msdn.microsoft.com/library/ff356104.aspx).

Zdalny Agent oferuje prosty sposób wdrażania zawartości z lokalizacji zdalnej, a takie podejście może dobrze współpracować z jednym kliknięciem lub automatycznym wdrożeniem. Jednak użytkownik, który uruchamia polecenie wdrożenia, musi być również administratorem domeny lub członkiem lokalnej grupy administratorów na serwerze docelowym. Dodatkowo Agent zdalny nie obsługuje uwierzytelniania podstawowego, dlatego nie można przekazać alternatywnych poświadczeń w wierszu polecenia.

Agent zdalny zapewnia przydatne podejście do wdrożenia w scenariuszach programistycznych lub testowych, w których deweloperzy mają pełną kontrolę nad środowiskiem serwera testowego, a aplikacje są zwykle ponownie kompilowane i ponownie wdrażane. ostatnio. Jednak takie podejście jest zwykle mniej akceptowalne dla środowisk przejściowych lub produkcyjnych.

Aby zapoznać się z kompleksowym przykładem scenariusza, który używa metody agenta zdalnego, zobacz [Scenariusz: Konfigurowanie środowiska testowego na potrzeby wdrażania w sieci Web](scenario-configuring-a-test-environment-for-web-deployment.md).

## <a name="using-the-temp-agent"></a>Korzystanie z agenta temp

Metoda tymczasowego agenta wdrożenia jest podobna do metody zdalnego agenta. Jednak w przeciwieństwie do podejścia zdalnego agenta nie trzeba instalować Web Deploy na docelowym serwerze sieci Web. Zamiast tego podczas wdrażania programu Web Deploy zainstaluje tymczasową wersję usługi agenta wdrażania sieci Web na serwerze docelowym i użyje tego do wdrożenia zawartości w usługach IIS. Po zakończeniu wdrażania wszystkie pliki tymczasowe zostaną usunięte.

Jeśli chcesz użyć ustawienia dostawcy agenta temp, Dodaj flagę **/g** do polecenia wdrożenia:

[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample4.cmd)]

> [!NOTE]
> Nie można użyć agenta tymczasowego, jeśli na komputerze docelowym jest zainstalowana usługa agenta wdrażania sieci Web, nawet jeśli usługa nie jest uruchomiona.

Zaletą tego podejścia jest to, że nie trzeba obsługiwać instalacji Web Deploy na serwerach docelowych. Ponadto nie ma potrzeby upewnienia się, że na komputerze źródłowym i docelowym działa ta sama wersja programu Web Deploy. Jednak takie podejście ma wpływ na te same ograniczenia podmiotu zabezpieczeń, co podejście zdalnego agenta, a mianowicie, że użytkownik musi być administratorem lokalnym na serwerze docelowym w celu wdrożenia zawartości i jest obsługiwane tylko uwierzytelnianie NTLM. Metoda tymczasowego agenta wymaga również dużej ilości konfiguracji początkowej środowiska docelowego.

Aby uzyskać więcej informacji na temat korzystania z agenta tymczasowego, zobacz [How to: Install a Deployment Package using the Deploy. cmd File](https://msdn.microsoft.com/library/ff356104.aspx) i [Web Deploy on Demand](https://technet.microsoft.com/library/ee517345(WS.10).aspx).

## <a name="using-the-web-deploy-handler"></a>Korzystanie z programu obsługi Web Deploy

W przypadku usług IIS 7, Web Deploy oferuje alternatywną metodę wdrażania za pomocą programu obsługi Web Deploy IIS. Procedura obsługi Web Deploy jest ściśle zintegrowana z usługą zarządzania siecią Web (WMSvc) usług IIS, która została zaprojektowana tak, aby umożliwić użytkownikom zarządzanie witrynami sieci Web usług IIS z lokalizacji zdalnych.

Domyślnie agent zdalny uwidacznia punkt końcowy HTTP pod tym adresem:

[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample5.cmd)]

> [!NOTE]
> Można zastąpić wartość [*Server*] nazwą komputera serwera sieci Web, adresem IP serwera sieci Web lub nazwą hosta, który jest rozpoznawany jako serwer sieci Web.

Zaletą korzystania z programu obsługi Web Deploy przez agenta zdalnego i agenta tymczasowego jest możliwość skonfigurowania usług IIS tak, aby zezwalała użytkownikom innym niż administrator na wdrażanie aplikacji i zawartości w określonych witrynach sieci Web usług IIS. Program obsługi Web Deploy obsługuje również uwierzytelnianie podstawowe, dzięki czemu można podać alternatywne poświadczenia jako parametry w poleceniach Web Deploy. Główna wada polega na tym, że program obsługi Web Deploy jest początkowo znacznie bardziej skomplikowany do skonfigurowania i skonfigurowania.

W przypadku użytkowników niebędących administratorami usługa zarządzania siecią Web (WMSvc) zezwoli użytkownikowi na łączenie się tylko z usługami IIS przy użyciu połączenia na poziomie lokacji, a nie z połączenia na poziomie serwera. Aby uzyskać dostęp do określonej lokacji, można uwzględnić w adresie punktu końcowego ciąg zapytania dotyczący konkretnej lokacji:

[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample6.cmd)]

Na przykład załóżmy, że proces kompilacji jest skonfigurowany tak, aby automatycznie wdrażał aplikację sieci Web w środowisku przejściowym po każdej pomyślnej kompilacji. W przypadku użycia metody agenta zdalnego należy przeprowadzić proces kompilacji tożsamość administratora na serwerach docelowych. W przeciwieństwie do procedury obsługi Web Deploy można przyznać użytkownikom&#x2014;innym niż administrator**FABRIKAM\stagingdeployer** w tym przypadku&#x2014;tylko uprawnienia do określonej witryny sieci Web usług IIS, a proces kompilacji może podać te poświadczenia, aby wdrożyć pakiet sieci Web.

[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample7.cmd)]

> [!NOTE]
> Aby uzyskać więcej informacji na temat Web Deploy operacji i składni wiersza polecenia, zobacz [Web Deploy informacje dotyczące wiersza polecenia](https://technet.microsoft.com/library/dd568991(v=ws.10).aspx). Aby uzyskać więcej informacji o korzystaniu z pliku *Deploy. cmd* , zobacz [How to: Install a Deployment Package using the Deploy. cmd plik](https://msdn.microsoft.com/library/ff356104.aspx).

Procedura obsługi Web Deploy zapewnia przydatne podejście do wdrożenia w środowiskach przejściowych, środowiskach hostowanych i środowiskach produkcyjnych opartych na intranecie, gdzie dostęp zdalny do serwera jest dostępny, ale poświadczenia administratora nie są dostępne.

Aby zapoznać się z kompleksowym przykładem scenariusza korzystającego z metody obsługi Web Deploy, zobacz [Scenariusz: Konfigurowanie środowiska przejściowego na potrzeby wdrażania w sieci Web](scenario-configuring-a-staging-environment-for-web-deployment.md).

## <a name="using-offline-deployment"></a>Korzystanie z wdrażania w trybie offline

W niektórych przypadkach nie jest możliwe ani praktyczne wdrażanie aplikacji i zawartości w witrynie sieci Web IIS z lokalizacji zdalnej. Na przykład Komputery źródłowe i docelowe mogą znajdować się w sieciach izolowanych lub segmentach sieci, a zasady zapory mogą nie zezwalać na dostęp zdalny.

W takich scenariuszach można nadal korzystać z możliwości tworzenia pakietów i publikowania Web Deploy; nie można ich używać z lokalizacji zdalnej. Zamiast tego administrator na serwerze docelowym musi skopiować pakiet sieci Web na serwer i zaimportować go za pomocą Menedżera usług IIS.

![](choosing-the-right-approach-to-web-deployment/_static/image1.png)

Podejście do wdrażania w trybie offline jest zazwyczaj przydatne w środowiskach produkcyjnych dostępnych w Internecie, gdzie serwery w sieci obwodowej mogą mieć ograniczoną łączność z komputerami w sieci wewnętrznej.

Aby zapoznać się z kompleksowym przykładem scenariusza korzystającego z metody wdrażania w trybie offline, zobacz [Scenariusz: Konfigurowanie środowiska produkcyjnego na potrzeby wdrażania w sieci Web](scenario-configuring-a-production-environment-for-web-deployment.md).

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat Web Deploy operacji i składni wiersza polecenia, zobacz [Web Deploy informacje dotyczące wiersza polecenia](https://technet.microsoft.com/library/dd568991(v=ws.10).aspx). Aby uzyskać więcej informacji o korzystaniu z pliku *Deploy. cmd* , zobacz [How to: Install a Deployment Package using the Deploy. cmd plik](https://msdn.microsoft.com/library/ff356104.aspx).

Aby uzyskać ogólne wskazówki na temat różnych sposobów wdrażania pakietów sieci Web z komputera zdalnego, zobacz [używanie Web Deploy zdalnie](https://technet.microsoft.com/library/ee461175(WS.10).aspx). Aby uzyskać więcej informacji na temat korzystania z Web Deploy na żądanie, zobacz [Web Deploy na żądanie](https://technet.microsoft.com/library/ee517345(WS.10).aspx).

> [!div class="step-by-step"]
> [Poprzednie](configuring-server-environments-for-web-deployment.md)
> [dalej](scenario-configuring-a-test-environment-for-web-deployment.md)

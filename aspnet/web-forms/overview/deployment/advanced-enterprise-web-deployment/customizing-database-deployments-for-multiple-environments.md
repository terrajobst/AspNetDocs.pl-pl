---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments
title: Dostosowywanie wdrożeń baz danych dla wielu środowisk | Microsoft Docs
author: jrjlee
description: 'W tym temacie opisano sposób dostosowywania właściwości bazy danych do określonych środowisk docelowych w ramach procesu wdrażania. Uwaga: w temacie założono, że...'
ms.author: riande
ms.date: 05/04/2012
ms.assetid: a172979a-1318-4318-a9c6-4f9560d26267
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments
msc.type: authoredcontent
ms.openlocfilehash: 8ae8cb1a322afb95c5d2e8d5e73c7825c7b2fe5a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78604028"
---
# <a name="customizing-database-deployments-for-multiple-environments"></a>Dostosowywanie wdrożeń bazy danych dla wielu środowisk

Autor [Jason Lewandowski](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie opisano sposób dostosowywania właściwości bazy danych do określonych środowisk docelowych w ramach procesu wdrażania.
> 
> > [!NOTE]
> > W temacie przyjęto założenie, że jest wdrażany projekt bazy danych programu Visual Studio 2010 przy użyciu programu MSBuild. exe i VSDBCMD. exe. Aby uzyskać więcej informacji na temat przyczyn tego podejścia, zobacz [wdrażanie w sieci Web w przedsiębiorstwie](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md) i [wdrażanie projektów bazy danych](../web-deployment-in-the-enterprise/deploying-database-projects.md).
> 
> 
> Podczas wdrażania projektu bazy danych w wielu miejscach docelowych często chcesz dostosować właściwości wdrażania bazy danych dla każdego środowiska docelowego. Na przykład w środowiskach testowych zwykle ponownie utworzysz bazę danych w każdym wdrożeniu, podczas gdy w środowisku przejściowym lub produkcyjnym znacznie bardziej zachodzi potrzeba przeprowadzenia aktualizacji przyrostowych w celu zachowania danych.
> 
> W projekcie bazy danych programu Visual Studio 2010 ustawienia wdrożenia są zawarte w pliku konfiguracji wdrożenia (. sqldeployment). W tym temacie pokazano, jak utworzyć pliki konfiguracji wdrożenia specyficzne dla środowiska i określić, który ma być używany jako parametr VSDBCMD.

Ten temat stanowi część szeregu samouczków opartych na wymaganiach dotyczących wdrażania w przedsiębiorstwie fikcyjnej firmy o nazwie Fabrikam, Inc. W tej serii samouczków jest stosowane&#x2014;przykładowe rozwiązanie&#x2014;do reprezentowania aplikacji sieci Web, [które ma](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)realistyczny poziom złożoności, w tym aplikacji ASP.NET MVC 3, usługi Windows Communication Foundation (WCF) i projektu bazy danych.

Metoda wdrażania w tym samouczku jest oparta na rozłożeniu pliku projektu dzielonego opisanym w artykule [Omówienie pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), w którym proces kompilacji jest kontrolowany przez dwa pliki&#x2014;projektu, zawierający instrukcje kompilacji, które mają zastosowanie do każdego środowiska docelowego, oraz jeden zawierający ustawienia kompilacji i wdrożenia specyficznego dla środowiska. W czasie kompilacji plik projektu specyficzny dla środowiska jest scalany z plikiem projektu Environment-niezależny od w celu utworzenia kompletnego zestawu instrukcji kompilacji.

## <a name="task-overview"></a>Przegląd zadania

W tym temacie założono, że:

- Należy użyć podejścia Rozdziel plik projektu do wdrożenia rozwiązania, zgodnie z opisem w artykule [Omówienie pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md).
- Należy wywołać VSDBCMD z pliku projektu, aby wdrożyć projekt bazy danych, zgodnie z opisem w [opisie procesu kompilacji](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

Aby utworzyć system wdrażania, który obsługuje Zmienianie właściwości wdrożenia bazy danych między środowiskami docelowymi, należy wykonać następujące:

- Utwórz plik konfiguracji wdrożenia (. sqldeployment) dla każdego środowiska docelowego.
- Utwórz polecenie VSDBCMD, które określa plik konfiguracji wdrożenia jako przełącznik wiersza polecenia.
- Sparametryzuj polecenie VSDBCMD w pliku projektu Microsoft Build Engine (MSBuild), tak aby opcje VSDBCMD były odpowiednie dla środowiska docelowego.

W tym temacie przedstawiono sposób wykonywania każdej z tych procedur.

## <a name="creating-environment-specific-deployment-configuration-files"></a>Tworzenie plików konfiguracji wdrożenia specyficznych dla środowiska

Domyślnie projekt bazy danych zawiera plik konfiguracji o pojedynczym wdrożeniu o nazwie *Database. sqldeployment*. Jeśli otworzysz ten plik w programie Visual Studio 2010, możesz zobaczyć różne dostępne opcje wdrażania:

- **Sortowanie porównania wdrożenia**. Pozwala to zdecydować, czy należy użyć sortowania bazy danych projektu (sortowania *źródła* ) czy sortowania bazy danych serwera docelowego (sortowanie *docelowe* ). W większości przypadków podczas wdrażania programu w środowisku deweloperskim lub testowym warto użyć sortowania źródłowego. Podczas wdrażania w środowisku przejściowym lub produkcyjnym zwykle warto pozostawić sortowanie docelowe bez zmian, aby uniknąć problemów ze współdziałaniem.
- **Wdróż właściwości bazy danych**. Dzięki temu można wybrać, czy mają być stosowane właściwości bazy danych, zgodnie z definicją w pliku *Database. sqlsettings* . Podczas wdrażania bazy danych po raz pierwszy należy wdrożyć właściwości bazy danych. W przypadku aktualizowania istniejącej bazy danych właściwości powinny być już na miejscu i nie trzeba ich wdrażać ponownie.
- **Zawsze należy ponownie utworzyć bazę danych**. Dzięki temu można zdecydować, czy należy ponownie utworzyć docelową bazę danych przy każdym wdrożeniu lub wprowadzić przyrostowe zmiany, aby zapewnić aktualność docelowej bazy danych ze schematem. Jeśli utworzysz ponownie bazę danych, utracisz wszystkie dane w istniejącej bazie danych. W związku z tym należy zwykle ustawić **wartość false** dla wdrożeń w środowisku przejściowym lub produkcyjnym.
- **Blokuj wdrożenie przyrostowe, jeśli może wystąpić utrata danych**. Dzięki temu można określić, czy wdrożenie ma zostać zatrzymane, jeśli zmiana schematu bazy danych spowoduje utratę danych. Zwykle to ustawienie ma **wartość true** w przypadku wdrożenia w środowisku produkcyjnym, co daje możliwość interwencji i ochrony wszelkich ważnych danych. Jeśli ustawiono opcję **zawsze ponownie utwórz bazę danych** na **wartość false**, to ustawienie nie będzie miało żadnego efektu.
- **Wykonaj wdrożenie w trybie jednego użytkownika**. Nie jest to zwykle problemem w środowiskach deweloperskich i testowych. Należy jednak zazwyczaj ustawić **wartość true** dla wdrożeń w środowisku przejściowym lub produkcyjnym. Uniemożliwia to użytkownikom wprowadzanie zmian w bazie danych w czasie trwania wdrożenia.
- **Wykonaj kopię zapasową bazy danych przed wdrożeniem**. Zazwyczaj ta wartość jest ustawiona na **true** podczas wdrażania w środowisku produkcyjnym, jako środek ostrożności przed utratą danych. Można również ustawić **wartość true** podczas wdrażania w środowisku przejściowym, jeśli Tymczasowa baza danych zawiera dużą ilość danych.
- **Generuj instrukcje drop dla obiektów, które znajdują się w docelowej bazie danych, ale nie znajdują się w projekcie bazy danych**. W większości przypadków jest to integralna i istotna część tworzenia przyrostowych zmian w bazie danych. Jeśli ustawiono opcję **zawsze ponownie utwórz bazę danych** na **wartość false**, to ustawienie nie będzie miało żadnego efektu.
- **Nie należy używać instrukcji ALTER Assembly do aktualizowania typów CLR**. To ustawienie określa, w jaki sposób SQL Server ma aktualizować typy środowiska uruchomieniowego języka wspólnego (CLR) do nowszych wersji zestawu. Ta wartość powinna być ustawiona na **false** w większości scenariuszy.

W tej tabeli przedstawiono typowe ustawienia wdrażania dla różnych środowisk docelowych. Jednak Twoje ustawienia mogą się różnić w zależności od konkretnych wymagań.

|  | Programowanie/testowanie | Przemieszczanie/integracja | Produkcja |
| --- | --- | --- | --- |
| **Sortowanie porównania wdrożenia** | Element źródłowy | Środowisko docelowe | Środowisko docelowe |
| **Wdróż właściwości bazy danych** | True | Tylko raz | Tylko raz |
| **Zawsze ponownie Twórz bazę danych** | True | Fałsz | Fałsz |
| **Blokuj wdrożenie przyrostowe, jeśli może wystąpić utrata danych** | Fałsz | Może | True |
| **Wykonaj skrypt wdrażania w trybie jednego użytkownika** | Fałsz | True | True |
| **Tworzenie kopii zapasowej bazy danych przed wdrożeniem** | Fałsz | Może | True |
| **Generuj instrukcje DROP dla obiektów, które znajdują się w docelowej bazie danych, ale nie znajdują się w projekcie bazy danych** | Fałsz | True | True |
| **Nie używaj instrukcji ALTER ASSEMBLY do aktualizowania typów CLR** | Fałsz | Fałsz | Fałsz |

> [!NOTE]
> Aby uzyskać więcej informacji na temat właściwości wdrażania bazy danych i zagadnień dotyczących środowiska, zobacz [Omówienie ustawień projektu bazy danych](https://msdn.microsoft.com/library/aa833291(v=VS.100).aspx), [instrukcje: Konfigurowanie właściwości szczegółów wdrożenia](https://msdn.microsoft.com/library/dd172125.aspx), [Kompilowanie i wdrażanie bazy danych w izolowanym środowisku programistycznym](https://msdn.microsoft.com/library/dd193409.aspx)oraz [Tworzenie i wdrażanie baz danych w środowisku przejściowym lub produkcyjnym](https://msdn.microsoft.com/library/dd193413.aspx).

Aby zapewnić obsługę wdrożenia projektu bazy danych w wielu miejscach docelowych, należy utworzyć plik konfiguracji wdrożenia dla każdego środowiska docelowego.

**Aby utworzyć plik konfiguracji specyficzny dla środowiska**

1. W programie Visual Studio 2010, w oknie **Eksplorator rozwiązań** kliknij prawym przyciskiem myszy projekt bazy danych, a następnie kliknij polecenie **Właściwości**.
2. Na stronie właściwości projektu bazy danych na karcie **wdrażanie** w wierszu **plik konfiguracji wdrożenia** kliknij pozycję **Nowy**.

    ![](customizing-database-deployments-for-multiple-environments/_static/image1.png)
3. W oknie dialogowym **nowy plik konfiguracji wdrożenia** Nadaj plikowi zrozumiałą nazwę (na przykład **TestEnvironment. sqldeployment**), a następnie kliknij przycisk **Zapisz**.
4. Na stronie *[filename] * * *. sqldeployment** ustaw właściwości wdrożenia zgodne z wymaganiami środowiska docelowego, a następnie Zapisz plik.

    ![](customizing-database-deployments-for-multiple-environments/_static/image2.png)
5. Zwróć uwagę, że nowy plik zostanie dodany do folderu właściwości w projekcie bazy danych.

    ![](customizing-database-deployments-for-multiple-environments/_static/image3.png)

## <a name="specifying-the-deployment-configuration-file-in-vsdbcmd"></a>Określanie pliku konfiguracji wdrożenia w VSDBCMD

W przypadku używania konfiguracji rozwiązania (takich jak debugowanie i wydawanie) w programie Visual Studio 2010 można skojarzyć plik konfiguracji wdrożenia z każdą konfiguracją. Podczas kompilowania określonej konfiguracji proces kompilacji generuje plik manifestu wdrożenia specyficzny dla konfiguracji, który wskazuje plik konfiguracyjny wdrożenia specyficzny dla konfiguracji. Jednym z głównych celów podejścia do wdrożenia opisanego w tych samouczkach jest jednak umożliwienie użytkownikom kontrolowania procesu wdrażania bez korzystania z programu Visual Studio 2010 i konfiguracji rozwiązań. W tym podejściu Konfiguracja rozwiązania jest taka sama, niezależnie od docelowego środowiska wdrażania. Aby dostosować wdrożenie bazy danych do określonego środowiska docelowego, można użyć opcji wiersza polecenia VSDBCMD, aby określić plik konfiguracji wdrożenia.

Aby określić plik konfiguracji wdrożenia w VSDBCMD, użyj przełącznika **p:/DeploymentConfigurationFile** i podaj pełną ścieżkę do pliku. Spowoduje to zastąpienie pliku konfiguracji wdrożenia, który identyfikuje manifest wdrożenia. Na przykład można użyć tego polecenia VSDBCMD do wdrożenia bazy danych **ContactManager** w środowisku testowym:

[!code-console[Main](customizing-database-deployments-for-multiple-environments/samples/sample1.cmd)]

> [!NOTE]
> Należy pamiętać, że proces kompilacji może zmienić nazwę pliku wdrożenia. sqldeployment, gdy plik jest kopiowany do katalogu wyjściowego.

Jeśli używasz zmiennych poleceń SQL w skryptach SQL przed wdrożeniem lub po wdrożeniu, możesz użyć podobnej metody, aby skojarzyć plik sqlcmdvars specyficzny dla środowiska ze wdrożeniem. W takim przypadku należy użyć przełącznika **p:/SqlCommandVariablesFile** , aby zidentyfikować plik. sqlcmdvars.

## <a name="running-the-vsdbcmd-command-from-an-msbuild-project-file"></a>Uruchamianie polecenia VSDBCMD z pliku projektu programu MSBuild

Można wywołać polecenie VSDBCMD z pliku projektu programu MSBuild przy użyciu zadania **exec** w elemencie docelowym programu MSBuild. W najprostszej postaci będzie wyglądać następująco:

[!code-xml[Main](customizing-database-deployments-for-multiple-environments/samples/sample2.xml)]

- W ćwiczeniu, aby ułatwić odczytywanie i ponowne używanie plików projektu, należy utworzyć właściwości do przechowywania różnych parametrów wiersza polecenia. Ułatwia to użytkownikom udostępnianie wartości właściwości w pliku projektu specyficznym dla środowiska lub przesłonięcie wartości domyślnych z wiersza polecenia programu MSBuild. Jeśli używasz podejścia do rozdzielnego pliku projektu opisanego w artykule [Omówienie pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), należy podzielić instrukcje kompilacji i właściwości między tymi dwoma plikami odpowiednio:
- Ustawienia specyficzne dla środowiska, takie jak nazwa pliku konfiguracji wdrożenia, parametry połączenia z bazą danych i Nazwa docelowej bazy danych, powinny przejść do pliku projektu specyficznego dla środowiska.
- Obiekt docelowy programu MSBuild, który uruchamia polecenie VSDBCMD, wraz ze wszystkimi właściwościami uniwersalnymi, takimi jak lokalizacja pliku wykonywalnego VSDBCMD, powinien znajdować się w pliku uniwersalnego projektu.

Należy również upewnić się, że projekt bazy danych został utworzony przed wywołaniem VSDBCMD w celu utworzenia pliku deploymanifest i gotowości do użycia. Możesz zobaczyć pełny przykład tego podejścia w temacie [opisującym proces kompilowania](../web-deployment-in-the-enterprise/understanding-the-build-process.md), który przeprowadzi Cię przez pliki projektu w [przykładowym rozwiązaniu Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md).

## <a name="conclusion"></a>Podsumowanie

W tym temacie opisano, jak można dopasować właściwości bazy danych do różnych środowisk docelowych podczas wdrażania projektów bazy danych za pomocą programu MSBuild i VSDBCMD. Takie podejście jest przydatne, gdy trzeba wdrożyć projekty bazy danych w ramach większych rozwiązań w skali korporacyjnej. Te rozwiązania są często wdrażane w wielu miejscach docelowych, takich jak środowiska deweloperskie lub testowe w trybie piaskownicy, platformy przygotowywania lub integracji oraz środowiska produkcyjne lub na żywo. Każdy z tych środowisk docelowych zazwyczaj wymaga unikatowego zestawu właściwości wdrożenia bazy danych.

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat wdrażania projektów bazy danych przy użyciu programu VSDBCMD. exe, zobacz [wdrażanie projektów bazy danych](../web-deployment-in-the-enterprise/deploying-database-projects.md). Aby uzyskać więcej informacji na temat używania niestandardowych plików projektów programu MSBuild do kontrolowania procesu wdrażania, zobacz [Omówienie pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md) i [zrozumienie procesu kompilacji](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

Te artykuły w witrynie MSDN zawierają bardziej ogólne wskazówki dotyczące wdrażania bazy danych:

- [Omówienie ustawień projektu bazy danych](https://msdn.microsoft.com/library/aa833291(v=VS.100).aspx)
- [Instrukcje: Konfigurowanie właściwości szczegółów wdrożenia](https://msdn.microsoft.com/library/dd172125.aspx)
- [Kompiluj i wdrażaj bazy danych w izolowanym środowisku programistycznym](https://msdn.microsoft.com/library/dd193409.aspx)
- [Kompilowanie i wdrażanie baz danych w środowisku przejściowym lub produkcyjnym](https://msdn.microsoft.com/library/dd193413.aspx)

> [!div class="step-by-step"]
> [Poprzednie](performing-a-what-if-deployment.md)
> [dalej](deploying-database-role-memberships-to-test-environments.md)

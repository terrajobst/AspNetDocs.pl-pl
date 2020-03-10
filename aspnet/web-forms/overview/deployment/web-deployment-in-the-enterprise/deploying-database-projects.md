---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-database-projects
title: Wdrażanie projektów bazy danych | Microsoft Docs
author: jrjlee
description: 'Uwaga: w wielu scenariuszach wdrażania w przedsiębiorstwie potrzebna jest możliwość publikowania aktualizacji przyrostowych w wdrożonej bazie danych. Alternatywą jest ponowne utworzenie...'
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 832f226a-1aa3-4093-8c29-ce4196793259
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-database-projects
msc.type: authoredcontent
ms.openlocfilehash: 221808758492aedb8e8329364e511df28fd11105
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78634268"
---
# <a name="deploying-database-projects"></a>Wdrażanie projektów baz danych

Autor [Jason Lewandowski](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> [!NOTE]
> W wielu scenariuszach wdrażania w przedsiębiorstwie potrzebna jest możliwość publikowania aktualizacji przyrostowych w wdrożonej bazie danych. Alternatywą jest ponowne utworzenie bazy danych na każdym wdrożeniu, co oznacza utratę wszystkich danych w istniejącej bazie danych. Podczas pracy z programem Visual Studio 2010 Zalecanym podejściem do przyrostowej publikacji bazy danych jest użycie VSDBCMD. Jednak Następna wersja programu Visual Studio i potok publikowania w sieci Web (WPP) zawierają narzędzia obsługujące publikowanie przyrostowe bezpośrednio.

Jeśli otworzysz przykładowe rozwiązanie Contact Manager w programie Visual Studio 2010, zobaczysz, że projekt bazy danych zawiera folder właściwości zawierający cztery pliki.

![](deploying-database-projects/_static/image1.png)

Wraz z plikiem projektu (*ContactManager. Database. DBPROJ* w tym przypadku) te pliki kontrolują różne aspekty procesu kompilowania i wdrażania:

- Plik *Database. sqlcmdvars* zawiera wartości dla dowolnych zmiennych narzędzia sqlcmd, które są używane podczas wdrażania projektu. Każda konfiguracja rozwiązania (na przykład Debug i Release) może określić inny plik. sqlcmdvars.
- Plik *Database. sqldeployment* zawiera ustawienia specyficzne dla wdrożenia, takie jak użycie sortowania zdefiniowanego w projekcie lub sortowanie serwera docelowego, czy należy ponownie utworzyć docelową bazę danych za każdym razem, czy po prostu zmienić istniejącą bazę danych w taki sposób, aby była ona aktualna i tak dalej. Każda konfiguracja rozwiązania może określać inny plik. sqldeployment.
- Plik *Database. sqlpermissionss* to dokument XML, którego można użyć do zdefiniowania wszelkich uprawnień, które mają zostać dodane do docelowej bazy danych. Wszystkie konfiguracje rozwiązania mają ten sam plik. sqlpermissionss.
- Plik *Database. sqlsettings* określa właściwości na poziomie bazy danych, które mają być używane podczas tworzenia bazy danych, takie jak sortowanie do użycia, zachowanie operatorów porównania i tak dalej. Wszystkie konfiguracje rozwiązania korzystają z tego samego pliku. sqlsettings.

Warto otworzyć te pliki w programie Visual Studio i zaznajomić się z zawartością.

Podczas kompilowania projektu bazy danych proces kompilacji tworzy dwa pliki:

- *Schemat bazy danych* (plik. DbSchema). W tym artykule opisano schemat bazy danych, która ma zostać utworzona w formacie XML.
- *Manifest wdrożenia* (plik. deploymanifest). Zawiera wszystkie informacje wymagane do utworzenia i wdrożenia bazy danych. Odwołuje się do pliku schematu. dboraz innych zasobów, takich jak instrukcje wdrożenia (plik. sqldeployment) i wszystkie skrypty SQL przed wdrożeniem lub po wdrożeniu.

Spowoduje to wyświetlenie relacji między tymi zasobami:

![](deploying-database-projects/_static/image2.png)

Jak widać, plik. sqlsettings i plik. sqlpermissionss są danymi wejściowymi do procesu kompilacji. Wraz z plikiem projektu bazy danych te pliki są używane do tworzenia pliku schematu bazy danych. Plik. sqldeployment i plik. sqlcmdvars są przekazywane przez proces kompilacji bez zmian. Manifest wdrożenia wskazuje lokalizację schematu bazy danych, plik. sqldeployment, plik. sqlcmdvars oraz wszystkie skrypty SQL przed wdrożeniem lub po wdrożeniu.

## <a name="why-use-vsdbcmd-to-deploy-a-database-project"></a>Dlaczego warto używać VSDBCMD do wdrożenia projektu bazy danych?

Istnieją różne podejścia do wdrażania projektów bazy danych. Jednak nie wszystkie z nich są odpowiednie do wdrożenia projektu bazy danych na serwerach zdalnych w środowisku przedsiębiorstwa. Zastanów się nad wdrożeniem projektu bazy danych. W scenariuszach wdrażania w przedsiębiorstwie można chcieć:

- Możliwość wdrażania projektu bazy danych z lokalizacji zdalnej.
- Możliwość wprowadzania aktualizacji przyrostowych do istniejącej bazy danych.
- Możliwość dołączania skryptów przed wdrożeniem lub skryptów po wdrożeniu.
- Możliwość dostosowywania wdrożenia do wielu środowisk docelowych.
- Możliwość wdrażania projektu bazy danych w ramach większego, typowego wdrożenia z jednym etapem.

Istnieją trzy główne podejścia, których można użyć do wdrożenia projektu bazy danych:

- Możesz użyć funkcji wdrażania z typem projektu bazy danych w programie Visual Studio 2010. Podczas kompilowania i wdrażania projektu bazy danych w programie Visual Studio 2010 proces wdrażania używa manifestu wdrażania do wygenerowania pliku wdrożenia opartego na języku SQL, który jest specyficzny dla konfiguracji kompilacji. Spowoduje to utworzenie bazy danych, jeśli jeszcze nie istnieje, lub wprowadzić niezbędne zmiany w bazie danych, jeśli już istnieją. Do uruchomienia tego pliku na serwerze docelowym można użyć narzędzia SQLCMD. exe lub można ustawić program Visual Studio w celu utworzenia i uruchomienia pliku. Wadą tego podejścia jest to, że masz ograniczoną kontrolę nad ustawieniami wdrożenia. Często trzeba również zmodyfikować plik wdrożenia SQL, aby zapewnić wartości zmiennych specyficznych dla środowiska. Tego podejścia można używać tylko na komputerze z zainstalowanym programem Visual Studio 2010, a Deweloper musi znać i podać parametry połączenia oraz poświadczenia dla wszystkich środowisk docelowych.
- Korzystając z Internet Information Services (IIS) narzędzia do wdrażania w sieci Web (Web Deploy) [, można wdrożyć bazę danych w ramach projektu aplikacji sieci Web](https://msdn.microsoft.com/library/dd465343.aspx). Jednak takie podejście jest znacznie bardziej złożone, jeśli chcesz wdrożyć projekt bazy danych, a nie po prostu replikacja istniejącej lokalnej bazy danych na serwerze docelowym. Można skonfigurować Web Deploy do uruchamiania skryptu wdrożenia SQL utworzonego przez projekt bazy danych, ale w tym celu należy utworzyć niestandardowy plik obiektów docelowych WPP dla projektu aplikacji sieci Web. Spowoduje to dodanie znacznej złożoności procesu wdrożenia. Ponadto Web Deploy nie obsługuje bezpośrednio aktualizacji przyrostowych istniejących baz danych. Aby uzyskać więcej informacji na temat tego podejścia, zobacz [rozszerzanie potoku publikowania w sieci Web do wdrożonego projektu bazy danych SQL](https://go.microsoft.com/?linkid=9805121).
- Narzędzia VSDBCMD można użyć do wdrożenia bazy danych przy użyciu schematu bazy danych lub manifestu wdrażania. Można wywołać VSDBCMD. exe z elementu docelowego programu MSBuild, który umożliwia publikowanie baz danych w ramach większego procesu wdrażania skryptowego. Można zastąpić zmienne w pliku. sqlcmdvars i wiele innych właściwości bazy danych z polecenia VSDBCMD, co pozwala na dostosowanie wdrożenia dla różnych środowisk bez tworzenia wielu konfiguracji kompilacji. VSDBCMD zapewnia funkcję rozróżniania, co oznacza, że będzie wprowadzać tylko niezbędne zmiany w celu dopasowania docelowej bazy danych do schematu bazy danych. VSDBCMD oferuje także szeroką gamę opcji wiersza polecenia, które zapewniają szczegółową kontrolę nad procesem wdrażania.

Z tego omówienia można zobaczyć, że użycie VSDBCMD z programem MSBuild jest najlepszym rozwiązaniem w typowym scenariuszu wdrażania przedsiębiorstwa:

|  | Visual Studio 2010 | Wdrożenie w sieci Web 2.0 | VSDBCMD.exe |
| --- | --- | --- | --- |
| Obsługuje zdalne wdrażanie? | Tak | Tak | Tak |
| Obsługuje aktualizacje przyrostowe? | Tak | Nie | Tak |
| Obsługuje skrypty pre-Deployment? | Tak | Tak | Tak |
| Obsługuje wdrożenie wiele środowisk? | Separator | Separator | Tak |
| Obsługuje wdrożenie skryptowe? | Separator | Tak | Tak |

W pozostałej części tego tematu opisano użycie VSDBCMD z programem MSBuild do wdrażania projektów bazy danych.

## <a name="understanding-the-deployment-process"></a>Informacje na temat procesu wdrażania

Narzędzie VSDBCMD umożliwia wdrażanie bazy danych przy użyciu schematu bazy danych (plik. DbSchema) lub manifestu wdrażania (plik. deploymanifest). W ćwiczeniu niemal zawsze będziesz używać manifestu wdrożenia, ponieważ manifest wdrożenia umożliwia podanie wartości domyślnych dla różnych właściwości wdrożenia i zidentyfikowanie wszelkich skryptów SQL przed wdrożeniem lub po wdrożeniu, które mają być uruchamiane. Na przykład to polecenie VSDBCMD służy do wdrażania bazy danych **ContactManager** na serwerze bazy danych w środowisku testowym:

[!code-console[Main](deploying-database-projects/samples/sample1.cmd)]

W takim przypadku:

- Przełącznik **/a** (lub **/Action**) określa, co ma być VSDBCMD. Można ustawić tę wartość na **Import** lub **wdrożenie**. Opcja **Importuj** służy do generowania pliku. DbSchema z istniejącej bazy danych, a opcja **Deploy** służy do wdrażania pliku. DbSchema w docelowej bazie danych.
- Przełącznik **/manifest** (lub **/MANIFESTFILE**) identyfikuje plik. deploymanifest, który ma zostać wdrożony. Jeśli zamiast tego chcesz użyć pliku. DbSchema, użyj przełącznika **/model** (lub **/ModelFile**).
- Przełącznik **/CS** (lub **/ConnectionString**) zawiera parametry połączenia dla docelowego serwera bazy danych. Należy pamiętać, że nie obejmuje to nazwy bazy danych&#x2014;VSDBCMD, która musi nawiązać połączenie z serwerem w celu utworzenia bazy danych; nie trzeba łączyć się z pojedynczą bazą danych. Jeśli plik. deploymanifest zawiera parametry połączenia, można pominąć ten przełącznik. Jeśli użyjesz przełącznika mimo to, wartość przełącznika zostanie zastąpiona wartością. deploymanifest.
- Właściwość <strong>/p: TargetDatabase</strong> zawiera nazwę, która ma zostać przypisana do docelowej bazy danych podczas tworzenia. Spowoduje to zastąpienie wartości właściwości <strong>TargetDatabase</strong> w pliku. deploymanifest. Można użyć składni <strong>/p:</strong> <em>[nazwa właściwości]</em>, aby ustawić szeroką gamę właściwości wdrożenia i zastąpić wszystkie zmienne sqlcmd zadeklarowane w pliku. sqlcmdvars.
- Przełącznik **/DD +** (lub **/DeployToDatabase +** ) wskazuje, że chcesz utworzyć wdrożenie i wdrożyć je w środowisku docelowym. Jeśli określisz **/DD-** lub pominięto przełącznik, VSDBCMD wygeneruje skrypt wdrożenia, ale nie zostanie wdrożony w środowisku docelowym. Ten przełącznik jest często źródłem pomyłek i wyjaśniono bardziej szczegółowo w następnej sekcji.
- Przełącznik **/Script** (lub **/DeploymentScriptFile**) określa, gdzie ma być generowany skrypt wdrożenia. Ta wartość nie ma wpływu na proces wdrażania.

Aby uzyskać więcej informacji na temat VSDBCMD, zobacz [informacje dotyczące wiersza polecenia dla VSDBCMD. EXE (wdrożenie i importowanie schematu)](https://msdn.microsoft.com/library/dd193283.aspx) i [instrukcje: Przygotowywanie bazy danych do wdrożenia z wiersza polecenia przy użyciu VSDBCMD. Plik EXE](https://msdn.microsoft.com/library/dd193258.aspx).

Przykład użycia VSDBCMD z pliku projektu programu MSBuild można znaleźć w temacie [Opis procesu kompilacji](understanding-the-build-process.md). Aby zapoznać się z przykładami konfigurowania ustawień wdrażania bazy danych dla wielu środowisk, zobacz [Dostosowywanie wdrożeń baz danych w wielu środowiskach](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md).

## <a name="understanding-the-deploytodatabase-switch"></a>Informacje o przełączniku DeployToDatabase

Zachowanie metody **/DD** lub **/DeployToDatabase** jest zależne od tego, czy używasz VSDBCMD z plikiem. DbSchema czy plikiem. deploymanifest. Jeśli używasz pliku ze schematem. DB, zachowanie jest dość proste:

- Jeśli określisz polecenie **/DD +** lub **/DD**, VSDBCMD wygeneruje skrypt wdrożenia i przeprowadzi wdrożenie bazy danych.
- Jeśli określisz **/DD-** lub pominięto przełącznik, VSDBCMD wygeneruje tylko skrypt wdrożenia.

Jeśli używasz pliku. deploymanifest, zachowanie jest znacznie bardziej skomplikowane. Wynika to z faktu, że plik. deploymanifest zawiera nazwę właściwości **DeployToDatabase** , która określa, czy baza danych została wdrożona.

[!code-xml[Main](deploying-database-projects/samples/sample2.xml)]

Wartość tej właściwości jest ustawiana zgodnie z właściwościami projektu bazy danych. Jeśli ustawisz **akcję Wdróż** w celu **utworzenia skryptu wdrożenia (. SQL)** , wartość będzie równa **false**. Jeśli ustawisz **akcję Wdróż** , aby **utworzyć skrypt wdrożenia (. SQL) i wdrożyć ją w bazie danych**, wartość będzie **równa true**.

> [!NOTE]
> Te ustawienia są skojarzone z określoną konfiguracją kompilacji i platformą. Na przykład jeśli skonfigurujesz ustawienia konfiguracji **debugowania** , a następnie opublikujesz przy użyciu konfiguracji **wydania** , Twoje ustawienia nie będą używane.

![](deploying-database-projects/_static/image3.png)

> [!NOTE]
> W tym scenariuszu **Akcja Wdróż** powinna zawsze być ustawiona, aby **utworzyć skrypt wdrożenia (. SQL)** , ponieważ nie chcesz, aby program Visual Studio 2010 został wdrożony. Innymi słowy Właściwość **DeployToDatabase** powinna zawsze mieć **wartość false**.

Gdy właściwość **DeployToDatabase** jest określona, przełącznik **/DD** przesłoni właściwość tylko wtedy, gdy wartość właściwości jest **równa false**:

- Jeśli właściwość **DeployToDatabase** ma **wartość false**, a określono polecenie **/DD +** lub **/DD**, VSDBCMD będzie przesłaniał Właściwość **DeployToDatabase** i wdrożyć bazę danych.
- Jeśli właściwość **DeployToDatabase** ma **wartość false**, a określono opcję **/DD-** lub Pomiń przełącznik, VSDBCMD nie będzie wdrażać bazy danych.
- Jeśli właściwość **DeployToDatabase** ma **wartość true**, VSDBCMD zignoruje przełącznik i Wdróż bazę danych.
- Skrypt wdrożenia jest generowany w każdym przypadku, niezależnie od tego, czy jest również wdrażana baza danych.

## <a name="conclusion"></a>Podsumowanie

Ten temat zawiera omówienie procesu kompilowania i wdrażania projektów bazy danych w programie Visual Studio 2010. Opisano w nim również, jak można użyć programu VSDBCMD. exe z programem MSBuild do obsługi wdrożenia bazy danych w skali przedsiębiorstwa.

Aby uzyskać więcej informacji na temat tego, jak to działa, zobacz [Dostosowywanie wdrożeń baz danych w wielu środowiskach](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md).

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać informacje na temat sposobu dostosowywania wdrożeń baz danych przez utworzenie oddzielnego pliku konfiguracji wdrożenia dla każdego środowiska, zobacz [Dostosowywanie wdrożeń baz danych w wielu środowiskach](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md). Aby uzyskać wskazówki dotyczące sposobu konfigurowania członkostw roli bazy danych, uruchamiając skrypt powdrożeniowy, zobacz [wdrażanie członkostw roli bazy danych w środowiskach testowych](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md). Aby uzyskać wskazówki dotyczące zarządzania niektórymi unikatowymi wyzwaniami dotyczącymi nakładania się baz danych, zobacz [wdrażanie baz danych członkostwa w środowiskach przedsiębiorstw](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md).

Te tematy w witrynie MSDN zapewniają szersze wskazówki i ogólne informacje dotyczące projektów baz danych programu Visual Studio i procesu wdrażania bazy danych:

- [Projekty bazy danych programu Visual Studio 2010 SQL Server](https://msdn.microsoft.com/library/ff678491.aspx)
- [Zarządzanie zmianą bazy danych](https://msdn.microsoft.com/library/aa833404.aspx)
- [Instrukcje: Przygotowywanie bazy danych do wdrożenia z wiersza polecenia przy użyciu VSDBCMD. EXE](https://msdn.microsoft.com/library/dd193258.aspx)
- [Omówienie Kompilowanie i wdrażanie bazy danych](https://msdn.microsoft.com/library/aa833165.aspx)

> [!div class="step-by-step"]
> [Poprzednie](deploying-web-packages.md)
> [dalej](creating-and-running-a-deployment-command-file.md)

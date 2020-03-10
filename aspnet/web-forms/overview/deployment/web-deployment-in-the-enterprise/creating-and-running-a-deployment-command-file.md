---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
title: Tworzenie i uruchamianie pliku poleceń wdrażania | Microsoft Docs
author: jrjlee
description: W tym temacie opisano, jak utworzyć plik poleceń, który umożliwi uruchomienie wdrożenia przy użyciu plików projektu Microsoft Build Engine (MSBuild) jako jednego kroku, ponownie...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: c61560e9-9f6c-4985-834a-08a3eabf9c3c
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
msc.type: authoredcontent
ms.openlocfilehash: f1477ff423e4898385066a35b42503f3c70dcc68
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78634310"
---
# <a name="creating-and-running-a-deployment-command-file"></a>Tworzenie i uruchamianie pliku poleceń wdrażania

Autor [Jason Lewandowski](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie opisano sposób tworzenia pliku poleceń, który umożliwia uruchomienie wdrożenia przy użyciu plików projektu Microsoft Build Engine (MSBuild) jako jednego kroku powtarzalnego procesu.

Ten temat stanowi część szeregu samouczków opartych na wymaganiach dotyczących wdrażania w przedsiębiorstwie fikcyjnej firmy o nazwie Fabrikam, Inc. W tej serii samouczków jest stosowane&#x2014;przykładowe [](the-contact-manager-solution.md) rozwiązanie&#x2014;do reprezentowania aplikacji sieci Web, które ma realistyczny poziom złożoności, w tym aplikacji ASP.NET MVC 3, usługi Windows Communication Foundation (WCF) i projektu bazy danych.

Metoda wdrażania w tym samouczku jest oparta na podejściu do pliku projektu podzielonego opisanego w artykule [Opis procesu kompilacji](understanding-the-build-process.md), w którym proces kompilacji jest kontrolowany przez dwa pliki&#x2014;projektu, zawierający instrukcje kompilacji, które mają zastosowanie do każdego środowiska docelowego, oraz jeden zawierający ustawienia kompilacji i wdrożenia specyficznego dla środowiska. W czasie kompilacji plik projektu specyficzny dla środowiska jest scalany z plikiem projektu Environment-niezależny od w celu utworzenia kompletnego zestawu instrukcji kompilacji.

## <a name="process-overview"></a>Przegląd procesu

W tym temacie dowiesz się, jak utworzyć i uruchomić plik poleceń, który używa tych plików projektu, aby przeprowadzić powtarzające wdrożenie w środowisku docelowym. Zasadniczo plik poleceń musi zawierać polecenie MSBuild, które:

- Informuje program MSBuild, aby wykonał plik niezależny od *Publish. proj* środowiska.
- Informuje plik *Publish. proj* , który plik zawiera ustawienia projektu specyficzne dla środowiska i miejsce, w którym można go znaleźć.

## <a name="create-an-msbuild-command"></a>Utwórz polecenie MSBuild

Zgodnie z opisem w temacie [proces kompilowania](understanding-the-build-process.md), plik&#x2014;projektu specyficzny dla środowiska, na przykład *ENV-dev. proj*&#x2014;, został zaprojektowany do zaimportowania do pliku Environment-niezależny od *Publish. proj* w czasie kompilacji. Razem te dwa pliki zawierają pełny zestaw instrukcji, które informują program MSBuild, jak skompilować i wdrożyć rozwiązanie.

Plik *Publish. proj* używa elementu **Import** w celu zaimportowania pliku projektu specyficznego dla środowiska.

[!code-xml[Main](creating-and-running-a-deployment-command-file/samples/sample1.xml)]

W związku z tym, gdy używasz programu MSBuild. exe do kompilowania i wdrażania rozwiązania Contact Manager, musisz:

- Uruchom program MSBuild. exe w pliku *Publish. proj* .
- Określ lokalizację pliku projektu specyficznego dla środowiska, dostarczając parametr wiersza polecenia o nazwie **TargetEnvPropsFile**.

Aby to zrobić, polecenie MSBuild powinno wyglądać następująco:

[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample2.cmd)]

W tym miejscu jest to prosty krok, aby przejść do powtarzalnego wdrożenia z jednym etapem. Wystarczy dodać polecenie MSBuild do pliku. cmd. W rozwiązaniu Contact Manager folder publikowania zawiera plik o nazwie *Publish-dev. cmd* , który dokładnie robi to.

[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample3.cmd)]

> [!NOTE]
> Przełącznik **/FL** instruuje program MSBuild, aby utworzył plik dziennika o nazwie *MSBuild. log* w katalogu roboczym, w którym został wywołany program MSBuild. exe.

Aby wdrożyć lub wdrożyć ponownie rozwiązanie Contact Manager, wystarczy uruchomić plik *Publish-dev. cmd* . Po uruchomieniu pliku MSBuild będzie:

- Kompiluj wszystkie projekty w rozwiązaniu.
- Generuj pakiety internetowe z możliwością wdrażania dla projektów aplikacji sieci Web.
- Generuj pliki. DbSchema i. deploymanifest dla projektów bazy danych.
- Wdróż pakiety sieci Web na serwerze sieci Web.
- Wdróż bazę danych na serwerze bazy danych.

## <a name="run-the-deployment"></a>Uruchamianie wdrożenia

Po utworzeniu pliku poleceń dla środowiska docelowego powinno być możliwe ukończenie całego wdrożenia, po prostu uruchamiając plik.

**Aby wdrożyć rozwiązanie Contact Manager w środowisku testowym**

1. Na stacji roboczej dewelopera Otwórz Eksploratora Windows, a następnie przejdź do lokalizacji pliku *Publish-dev. cmd* .
2. Kliknij dwukrotnie plik, aby go uruchomić.
3. Jeśli zostanie wyświetlone okno dialogowe **Otwórz plik — ostrzeżenie o zabezpieczeniach** , kliknij przycisk **Uruchom**.
4. Jeśli ustawienia konfiguracji i serwery testowe są prawidłowo skonfigurowane, w oknie wiersza polecenia zostanie wyświetlony komunikat **kompilacja zakończyła się powodzeniem** , gdy MSBuild zakończy przetwarzanie plików projektu.

    ![](creating-and-running-a-deployment-command-file/_static/image1.png)
5. Jeśli po raz pierwszy wdrożono rozwiązanie w tym środowisku, należy dodać konto testowego serwera sieci Web do roli bazy danych **\_** i bazy danych, **\_** a **w bazie danych** . Ta procedura została opisana w artykule [Konfigurowanie serwera bazy danych do publikowania Web Deploy](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).

    > [!NOTE]
    > Po utworzeniu bazy danych wystarczy przypisać te uprawnienia. Domyślnie proces kompilacji nie utworzy ponownie bazy danych na każdym wdrożeniu&#x2014;, a następnie przeprowadzi porównanie istniejącej bazy danych z najnowszym schematem i wprowadzi tylko wymagane zmiany. W związku z tym należy tylko zmapować te role bazy danych przy pierwszym wdrożeniu rozwiązania.
6. Otwórz program Internet Explorer i przejdź do adresu URL aplikacji Contact Manager (na przykład `http://testweb1:85/ContactManager/`).
7. Sprawdź, czy aplikacja działa zgodnie z oczekiwaniami, i możesz dodać kontakty.

    ![](creating-and-running-a-deployment-command-file/_static/image2.png)

## <a name="conclusion"></a>Podsumowanie

Tworzenie pliku poleceń zawierającego instrukcje programu MSBuild umożliwia szybkie i łatwe tworzenie i wdrażanie rozwiązań obejmujących wiele projektów w konkretnym środowisku docelowym. Jeśli musisz wielokrotnie wdrożyć rozwiązanie w wielu środowiskach docelowych, możesz utworzyć wiele plików poleceń. W każdym pliku poleceń MSBuild polecenie kompiluje ten sam plik projektu uniwersalnego, ale określi inny plik projektu specyficzny dla środowiska. Na przykład plik poleceń do opublikowania w środowisku deweloperskim lub testowym może zawierać to polecenie MSBuild:

[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample4.cmd)]

Plik polecenia do opublikowania w środowisku przejściowym może zawierać to polecenie MSBuild:

[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample5.cmd)]

> [!NOTE]
> Aby uzyskać wskazówki dotyczące dostosowywania plików projektu specyficznych dla środowiska dla własnych środowisk serwera, zobacz [Konfigurowanie właściwości wdrożenia dla środowiska docelowego](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).

Możesz również dostosować proces kompilacji dla każdego środowiska, zastępując właściwości lub ustawiając różne inne przełączniki w poleceniu MSBuild. Aby uzyskać więcej informacji, zobacz [Dokumentacja wiersza polecenia programu MSBuild](https://msdn.microsoft.com/library/ms164311.aspx).

> [!div class="step-by-step"]
> [Poprzednie](deploying-database-projects.md)
> [dalej](manually-installing-web-packages.md)

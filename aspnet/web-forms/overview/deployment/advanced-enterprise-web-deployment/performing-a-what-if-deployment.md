---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/performing-a-what-if-deployment
title: Wykonywanie wdrożenia What If | Microsoft Docs
author: jrjlee
description: W tym temacie opisano sposób wykonywania wdrożeń "co jeśli" (lub symulowanych) przy użyciu narzędzia Web Deployment Internet Information Services (Web Deploy) i programu V...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: c711b453-01ac-4e65-a48c-93d99bf22e58
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/performing-a-what-if-deployment
msc.type: authoredcontent
ms.openlocfilehash: 73a0e038cc0d4ebae0ffc8ed3fd2de4c9dad673c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78628892"
---
# <a name="performing-a-what-if-deployment"></a>Wykonywanie wdrożenia z użyciem analizy warunkowej

Autor [Jason Lewandowski](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie opisano sposób wykonywania wdrożeń "co jeśli" (lub symulowanych) przy użyciu narzędzia Web Deployment Internet Information Services (Web Deploy) i VSDBCMD. Dzięki temu można określić efekty logiki wdrożenia w określonym środowisku docelowym przed faktycznym wdrożeniem aplikacji.

Ten temat stanowi część szeregu samouczków opartych na wymaganiach dotyczących wdrażania w przedsiębiorstwie fikcyjnej firmy o nazwie Fabrikam, Inc. W tej serii samouczków jest stosowane&#x2014;przykładowe rozwiązanie&#x2014;do reprezentowania aplikacji sieci Web, [które ma](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)realistyczny poziom złożoności, w tym aplikacji ASP.NET MVC 3, usługi Windows Communication Foundation (WCF) i projektu bazy danych.

Metoda wdrażania w tym samouczku jest oparta na rozłożeniu pliku projektu dzielonego opisanym w artykule [Omówienie pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), w którym proces kompilacji i wdrożenia jest kontrolowany przez dwa pliki&#x2014;projektu, zawierający instrukcje kompilacji, które mają zastosowanie do każdego środowiska docelowego, oraz jeden zawierający ustawienia kompilacji i wdrożenia specyficznego dla środowiska. W czasie kompilacji plik projektu specyficzny dla środowiska jest scalany z plikiem projektu Environment-niezależny od w celu utworzenia kompletnego zestawu instrukcji kompilacji.

## <a name="performing-a-what-if-deployment-for-web-packages"></a>Wykonywanie wdrożenia "What If" dla pakietów internetowych

Web Deploy obejmuje funkcje, które umożliwiają wykonywanie wdrożeń w trybie "co jeśli" (lub wersja próbna). W przypadku wdrażania artefaktów w trybie "co w przypadku" Web Deploy generuje plik dziennika tak, jakby zostało wykonane wdrożenie, ale w rzeczywistości nie zmienia niczego na serwerze docelowym. Przegląd pliku dziennika może pomóc zrozumieć, jaki wpływ na wdrożenie będzie miał na serwerze docelowym, w szczególności:

- Co zostanie dodane.
- Co zostanie zaktualizowane.
- Co spowoduje usunięcie.

Ze względu na to, że wdrożenie "co jeśli" nie zmienia się w rzeczywistości na serwerze docelowym, to nie zawsze jest przewidywane, czy wdrożenie zakończy się pomyślnie.

Zgodnie z opisem w artykule [wdrażanie pakietów internetowych](../web-deployment-in-the-enterprise/deploying-web-packages.md)można wdrażać pakiety sieci Web przy użyciu Web Deploy&#x2014;na dwa sposoby przy użyciu narzędzia wiersza polecenia MSDeploy. exe bezpośrednio lub przez uruchomienie pliku *Deploy. cmd* , który generuje proces kompilacji.

Jeśli używasz programu MSDeploy. exe bezpośrednio, możesz uruchomić wdrożenie "co jeśli", dodając flagę **– whatIf** do polecenia. Na przykład, aby sprawdzić, co się stanie w przypadku wdrożenia pakietu ContactManager. MVC. zip w środowisku przejściowym, polecenie MSDeploy powinno wyglądać następująco:

[!code-console[Main](performing-a-what-if-deployment/samples/sample1.cmd)]

Gdy wyniki wdrożenia "co w przypadku" są zadowalające, można usunąć flagę **– whatIf** , aby uruchomić wdrożenie na żywo.

> [!NOTE]
> Aby uzyskać więcej informacji na temat opcji wiersza polecenia programu MSDeploy. exe, zobacz [Web Deploy ustawienia operacji](https://technet.microsoft.com/library/dd569089(WS.10).aspx).

Jeśli używasz pliku *. deploy. cmd* , możesz uruchomić wdrożenie "co jeśli", dołączając flagę **/t** flag (tryb wersji próbnej) zamiast flagi **/y** ("tak" lub "tryb aktualizacji") w poleceniu. Na przykład, aby sprawdzić, co się stanie w przypadku wdrożenia pakietu ContactManager. MVC. zip, uruchamiając plik *. deploy. cmd* , polecenie powinno wyglądać następująco:

[!code-console[Main](performing-a-what-if-deployment/samples/sample2.cmd)]

Gdy wyniki wdrożenia "w trybie próbnym" są zadowalające, można zamienić flagę **/t** na flagę **/y** , aby uruchomić wdrożenie na żywo:

[!code-console[Main](performing-a-what-if-deployment/samples/sample3.cmd)]

> [!NOTE]
> Aby uzyskać więcej informacji na temat opcji wiersza polecenia dla plików *Deploy. cmd* , zobacz [How to: Install a Deployment Package using the Deploy. cmd plik](https://msdn.microsoft.com/library/ff356104.aspx). Jeśli uruchomisz plik *Deploy. cmd* bez określania żadnych flag, w wierszu polecenia zostanie wyświetlona lista dostępnych flag.

## <a name="performing-a-what-if-deployment-for-databases"></a>Wykonywanie wdrożenia "What If" dla baz danych

W tej sekcji założono, że używasz narzędzia VSDBCMD do wykonywania przyrostowych, opartych na schemacie wdrożenia bazy danych. To podejście jest bardziej szczegółowo opisane w temacie [wdrażanie projektów bazy danych](../web-deployment-in-the-enterprise/deploying-database-projects.md). Zalecamy zapoznanie się z tym tematem przed zastosowaniem pojęć opisanych w tym miejscu.

W przypadku korzystania z VSDBCMD w trybie **wdrażania** można użyć flagi **/DD** (lub **/DeployToDatabase**), aby określić, czy VSDBCMD faktycznie wdraża bazę danych, czy po prostu generuje skrypt wdrożenia. Jeśli wdrażasz plik. DbSchema, jest to zachowanie:

- Jeśli określisz polecenie **/DD +** lub **/DD**, VSDBCMD wygeneruje skrypt wdrożenia i przeprowadzi wdrożenie bazy danych.
- Jeśli określisz **/DD-** lub pominięto przełącznik, VSDBCMD wygeneruje tylko skrypt wdrożenia.

> [!NOTE]
> Jeśli wdrażasz plik. deploymanifest, a nie plik. DbSchema, zachowanie przełącznika **/DD** jest znacznie bardziej skomplikowane. Zasadniczo, VSDBCMD zignoruje wartość przełącznika **/DD** , jeśli plik. deploymanifest zawiera element **DeployToDatabase** o wartości **true**. [Wdrożenie projektów bazy danych](../web-deployment-in-the-enterprise/deploying-database-projects.md) opisuje to zachowanie w całości.

Na przykład w celu wygenerowania skryptu wdrażania bazy danych **ContactManager** bez faktycznego wdrażania bazy danych polecenie VSDBCMD powinno wyglądać następująco:

[!code-console[Main](performing-a-what-if-deployment/samples/sample4.cmd)]

VSDBCMD to narzędzie do wdrażania różnicowych baz danych, a w związku z tym skrypt wdrożenia jest generowany dynamicznie w taki sposób, aby zawierał wszystkie polecenia SQL niezbędne do zaktualizowania bieżącej bazy danych, jeśli taka istnieje, do określonego schematu. Sprawdzenie, czy skrypt wdrożenia jest przydatny, aby określić, jaki ma wpływ wdrożenie na bieżącą bazę danych i zawarte w nim dane. Na przykład możesz chcieć określić:

- Czy istniejące tabele zostaną usunięte i czy spowoduje to utratę danych.
- Czy kolejność operacji polega na wykorzystaniu ryzyka utraty danych, na przykład w przypadku dzielenia lub scalania tabel.

Jeśli masz zadowolony ze skryptu wdrażania, możesz powtórzyć VSDBCMD z flagą **/DD +** , aby wprowadzić zmiany. Alternatywnie można edytować skrypt wdrożenia w celu spełnienia wymagań, a następnie wykonać go ręcznie na serwerze bazy danych.

## <a name="integrating-what-if-functionality-into-custom-project-files"></a>Integrowanie funkcji "What If" w niestandardowych plikach projektu

W bardziej złożonych scenariuszach wdrażania należy użyć pliku projektu niestandardowego Microsoft Build Engine (MSBuild) do hermetyzacji logiki kompilowania i wdrażania, zgodnie z opisem w artykule [Omówienie pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Na przykład w przykładowym rozwiązaniu [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) plik *Publish. proj* :

- Kompiluje rozwiązanie.
- Używa Web Deploy do pakowania i wdrażania aplikacji ContactManager. MVC.
- Używa Web Deploy do pakowania i wdrażania aplikacji ContactManager. Service.
- Wdraża bazę danych **ContactManager** .

Po zintegrowaniu wdrożenia wielu pakietów i/lub baz danych sieci Web w jednoetapowy proces w ten sposób można również użyć opcji wykonywania całego wdrożenia w trybie "co w przypadku".

Plik *Publish. proj* pokazuje, jak to zrobić. Najpierw należy utworzyć właściwość do przechowywania wartości "co jeśli":

[!code-xml[Main](performing-a-what-if-deployment/samples/sample5.xml)]

W tym przypadku utworzono właściwość o nazwie **whatIf** z wartością domyślną **false**. Użytkownicy mogą przesłonić tę wartość przez ustawienie dla właściwości wartości **true** w parametrze wiersza polecenia, ponieważ wkrótce zobaczysz.

Następnym etapem jest Sparametryzuj wszystkich poleceń Web Deploy i VSDBCMD, aby flagi odzwierciedlały wartość właściwości **whatIf** . Na przykład następny element docelowy (pobrany z pliku *Publish. proj* i uproszczony) uruchamia plik *Deploy. cmd* w celu wdrożenia pakietu internetowego. Domyślnie polecenie zawiera przełącznik **/y** ("tak" lub "tryb aktualizacji"). Jeśli wartość **whatIf** jest ustawiona na **true**, zostanie zastąpiona przez przełącznik **/t** (wersja próbna lub tryb "co w przypadku").

[!code-xml[Main](performing-a-what-if-deployment/samples/sample6.xml)]

Podobnie w następnym miejscu docelowym do wdrożenia bazy danych jest stosowane narzędzie VSDBCMD. Domyślnie przełącznik **/DD** nie jest uwzględniony. Oznacza to, że program VSDBCMD generuje skrypt wdrożenia, ale nie będzie wdrażał&#x2014;bazy danych inaczej mówiąc, scenariusza "co w przypadku". Jeśli właściwość **whatIf** nie jest ustawiona na **wartość true**, zostanie dodany przełącznik **/DD** , a VSDBCMD będzie wdrażał bazę danych.

[!code-xml[Main](performing-a-what-if-deployment/samples/sample7.xml)]

Możesz użyć tego samego podejścia, aby Sparametryzuj wszystkie odpowiednie polecenia w pliku projektu. Jeśli chcesz uruchomić wdrożenie "co jeśli", możesz po prostu podać wartość właściwości **whatIf** z wiersza polecenia:

[!code-console[Main](performing-a-what-if-deployment/samples/sample8.cmd)]

W ten sposób można uruchomić wdrożenie "co jeśli" dla wszystkich składników projektu w jednym kroku.

## <a name="conclusion"></a>Podsumowanie

W tym temacie opisano sposób uruchamiania wdrożeń "co jeśli" przy użyciu Web Deploy, VSDBCMD i MSBuild. Wdrożenie "co jeśli" pozwala oszacować wpływ proponowanego wdrożenia przed faktycznym wprowadzeniem zmian w środowisku docelowym.

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji o składni wiersza polecenia Web Deploy, zobacz [Web Deploy ustawienia operacji](https://technet.microsoft.com/library/dd569089(WS.10).aspx). Aby uzyskać wskazówki dotyczące opcji wiersza polecenia przy użyciu pliku *. deploy. cmd* , zobacz [How to: Install a Deployment Package using the Deploy. cmd plik](https://msdn.microsoft.com/library/ff356104.aspx). Wskazówki dotyczące składni wiersza polecenia VSDBCMD można znaleźć w temacie [Dokumentacja wiersza polecenia dla VSDBCMD. EXE (wdrożenie i importowanie schematu)](https://msdn.microsoft.com/library/dd193283.aspx).

> [!div class="step-by-step"]
> [Poprzednie](advanced-enterprise-web-deployment.md)
> [dalej](customizing-database-deployments-for-multiple-environments.md)

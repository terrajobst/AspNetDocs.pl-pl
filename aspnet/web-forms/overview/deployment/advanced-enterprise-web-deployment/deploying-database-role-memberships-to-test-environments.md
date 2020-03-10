---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments
title: Wdrażanie członkostw roli bazy danych w środowiskach testowych | Microsoft Docs
author: jrjlee
description: W tym temacie opisano sposób dodawania kont użytkowników do ról bazy danych w ramach wdrożenia rozwiązania w środowisku testowym. Po wdrożeniu rozwiązania zawierającego...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 9b2af539-7ad9-47aa-b66e-873bd9906e79
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments
msc.type: authoredcontent
ms.openlocfilehash: a15f5bf5f659d151e91ef9e53c5ad55bcd8e2b01
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78587592"
---
# <a name="deploying-database-role-memberships-to-test-environments"></a>Wdrażanie członkostw ról bazy danych w środowiskach testowych

Autor [Jason Lewandowski](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie opisano sposób dodawania kont użytkowników do ról bazy danych w ramach wdrożenia rozwiązania w środowisku testowym.
> 
> W przypadku wdrażania rozwiązania zawierającego projekt bazy danych w środowisku przejściowym lub produkcyjnym zazwyczaj nie ma możliwości automatyzacji dodawania kont użytkowników do ról bazy danych. W większości przypadków Deweloper nie wie, które konta użytkowników muszą zostać dodane do których ról bazy danych, a wymagania te mogą ulec zmianie w dowolnym momencie. Jednak w przypadku wdrożenia rozwiązania zawierającego projekt bazy danych w środowisku deweloperskim lub testowym sytuacja zazwyczaj jest inna:
> 
> - Deweloper zazwyczaj ponownie wdraża rozwiązanie regularnie, często kilka razy dziennie.
> - Baza danych jest zwykle tworzona w każdym wdrożeniu, co oznacza, że użytkownicy bazy danych muszą być utworzeni i dodawani do ról po każdym wdrożeniu.
> - Deweloper ma zwykle pełną kontrolę nad docelowym środowiskiem programistycznym lub testowym.
> 
> W tym scenariuszu często korzystne jest automatyczne tworzenie użytkowników bazy danych i przypisywanie członkostw roli bazy danych w ramach procesu wdrażania.
> 
> Kluczowym czynnikiem jest to, że ta operacja musi być oparta na środowisku docelowym. W przypadku wdrażania w środowisku przejściowym lub produkcyjnym należy pominąć operację. Jeśli wdrażasz program w środowisku deweloperskim lub testowym, chcesz wdrożyć członkostwa ról bez dalszej interwencji. W tym temacie opisano jedno podejście, za pomocą którego można rozwiązać ten problem.

Ten temat stanowi część szeregu samouczków opartych na wymaganiach dotyczących wdrażania w przedsiębiorstwie fikcyjnej firmy o nazwie Fabrikam, Inc. W tej serii samouczków jest stosowane&#x2014;przykładowe rozwiązanie&#x2014;do reprezentowania aplikacji sieci Web, [które ma](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)realistyczny poziom złożoności, w tym aplikacji ASP.NET MVC 3, usługi Windows Communication Foundation (WCF) i projektu bazy danych.

Metoda wdrażania w tym samouczku jest oparta na rozłożeniu pliku projektu dzielonego opisanym w artykule [Omówienie pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), w którym proces kompilacji jest kontrolowany przez dwa pliki&#x2014;projektu, zawierający instrukcje kompilacji, które mają zastosowanie do każdego środowiska docelowego, oraz jeden zawierający ustawienia kompilacji i wdrożenia specyficznego dla środowiska. W czasie kompilacji plik projektu specyficzny dla środowiska jest scalany z plikiem projektu Environment-niezależny od w celu utworzenia kompletnego zestawu instrukcji kompilacji.

## <a name="task-overview"></a>Przegląd zadania

W tym temacie założono, że:

- Należy użyć podejścia Rozdziel plik projektu do wdrożenia rozwiązania, zgodnie z opisem w artykule [Omówienie pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md).
- Należy wywołać VSDBCMD z pliku projektu, aby wdrożyć projekt bazy danych, zgodnie z opisem w [opisie procesu kompilacji](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

Aby utworzyć użytkowników bazy danych i przypisać członkostwa w rolach podczas wdrażania projektu bazy danych w środowisku testowym, należy:

- Utwórz skrypt Transact Structured Query Language (Transact-SQL), który wprowadza niezbędne zmiany w bazie danych.
- Utwórz obiekt docelowy Microsoft Build Engine (MSBuild), który używa narzędzia sqlcmd. exe, aby uruchomić skrypt SQL.
- Skonfiguruj pliki projektu do wywoływania obiektu docelowego podczas wdrażania rozwiązania w środowisku testowym.

W tym temacie przedstawiono sposób wykonywania każdej z tych procedur.

## <a name="scripting-the-database-role-memberships"></a>Tworzenie skryptów członkostw ról bazy danych

Skrypt języka Transact-SQL można utworzyć na wiele różnych sposobów i w dowolnej wybranej lokalizacji. Najprostszym rozwiązaniem jest utworzenie skryptu w ramach rozwiązania w programie Visual Studio 2010.

**Aby utworzyć skrypt SQL**

1. W oknie **Eksplorator rozwiązań** rozwiń węzeł projektu bazy danych.
2. Kliknij prawym przyciskiem myszy folder **skrypty** , wskaż polecenie **Dodaj**, a następnie kliknij pozycję **Nowy folder**.
3. Wpisz **test** jako nazwę folderu, a następnie naciśnij klawisz ENTER.
4. Kliknij prawym przyciskiem myszy folder **testowy** , wskaż polecenie **Dodaj**, a następnie kliknij pozycję **skrypt**.
5. W oknie dialogowym **Dodaj nowy element** nadaj skryptowi zrozumiałą nazwę (na przykład **AddRoleMemberships. SQL**), a następnie kliknij przycisk **Dodaj**.

    ![](deploying-database-role-memberships-to-test-environments/_static/image1.png)
6. W pliku *AddRoleMemberships. SQL* Dodaj instrukcje języka Transact-SQL, które:

    1. Utwórz użytkownika bazy danych dla identyfikatora logowania SQL Server, który będzie uzyskiwać dostęp do bazy danych.
    2. Dodaj użytkownika bazy danych do wszystkich wymaganych ról bazy danych.
7. Plik powinien wyglądać następująco:

    [!code-sql[Main](deploying-database-role-memberships-to-test-environments/samples/sample1.sql)]
8. Zapisz plik.

## <a name="executing-the-script-on-the-target-database"></a>Wykonywanie skryptu w docelowej bazie danych

Najlepiej uruchomić wszystkie wymagane skrypty Transact-SQL w ramach skryptu powdrożeniowego podczas wdrażania projektu bazy danych. Jednak skrypty powdrożeniowe nie pozwalają na wykonywanie logiki warunkowo na podstawie konfiguracji rozwiązania lub właściwości kompilacji. Alternatywą jest uruchomienie skryptów SQL bezpośrednio z pliku projektu MSBuild przez utworzenie elementu **docelowego** , który wykonuje polecenie sqlcmd. exe. Możesz użyć tego polecenia, aby uruchomić skrypt w docelowej bazie danych:

[!code-console[Main](deploying-database-role-memberships-to-test-environments/samples/sample2.cmd)]

> [!NOTE]
> Aby uzyskać więcej informacji na temat opcji wiersza polecenia sqlcmd, zobacz [Narzędzie sqlcmd](https://msdn.microsoft.com/library/ms162773.aspx).

Przed osadzeniem tego polecenia w elemencie docelowym programu MSBuild należy wziąć pod uwagę, jakie warunki ma uruchamiać skrypt:

- Docelowa baza danych musi istnieć przed zmianą członkostw ról. W związku z tym należy uruchomić ten skrypt *po* wdrożeniu bazy danych.
- Musisz dołączyć warunek, aby skrypt był wykonywany tylko w środowiskach testowych.
- Jeśli używasz wdrożenia&#x2014;"co jeśli" inaczej mówiąc, Jeśli generujesz skrypty wdrażania, ale nie są w nim&#x2014;uruchomione, nie należy uruchamiać skryptu SQL.

Jeśli używasz podejścia do podzielonego pliku projektu opisanego w artykule [Opis pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), jak pokazano w przykładowym rozwiązaniu Contact Manager, możesz podzielić instrukcje kompilacji dla skryptu SQL w następujący sposób:

- Wszelkie wymagane właściwości specyficzne dla środowiska, wraz z właściwością określającą, czy należy wdrożyć uprawnienia, powinny znajdować się w pliku projektu specyficznym dla środowiska (na przykład *ENV-dev. proj*).
- Sam obiekt docelowy programu MSBuild, wraz z wszelkimi właściwościami, które nie zmieni się między środowiskami docelowymi, powinien znajdować się w pliku uniwersalnego projektu (na przykład *Publish. proj*).

W pliku projektu specyficznym dla środowiska należy zdefiniować nazwę serwera bazy danych, nazwę docelowej bazy danych i Właściwość logiczną, która umożliwia użytkownikowi określenie, czy mają zostać wdrożone członkostwa ról.

[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample3.xml)]

W pliku uniwersalnego projektu należy podać lokalizację pliku wykonywalnego sqlcmd i lokalizację skryptu SQL, który ma zostać uruchomiony. Te właściwości pozostaną takie same, niezależnie od środowiska docelowego. Należy również utworzyć obiekt docelowy MSBuild, aby wykonać polecenie sqlcmd.

[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample4.xml)]

Należy zauważyć, że w celu dodania lokalizacji pliku wykonywalnego sqlcmd jako właściwości statycznej, ponieważ może to być przydatne dla innych obiektów docelowych. Z kolei definiujesz lokalizację skryptu SQL i składni polecenia sqlcmd jako właściwości dynamiczne w miejscu docelowym, ponieważ nie będą one wymagane przed wykonaniem elementu docelowego. W tym przypadku element docelowy **DeployTestDBPermissions** będzie wykonywany tylko w przypadku spełnienia następujących warunków:

- Właściwość **DeployTestDBRoleMemberships** ma **wartość true**.
- Użytkownik nie określił flagi **whatIf = true** .

Na koniec nie zapomnij wywołać elementu docelowego. W pliku *Publish. proj* można to zrobić, dodając obiekt docelowy do listy zależności dla domyślnego elementu docelowego **FullPublish** . Musisz się upewnić, że element docelowy **DeployTestDBPermissions** nie zostanie wykonany, dopóki nie zostanie wykonane miejsce docelowe **PublishDbPackages** .

[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample5.xml)]

## <a name="conclusion"></a>Podsumowanie

W tym temacie opisano jeden ze sposobów dodawania użytkowników bazy danych i członkostw ról w ramach akcji wykonywanej po wdrożeniu podczas wdrażania projektu bazy danych. Jest to zazwyczaj przydatne w przypadku regularnego ponownego tworzenia bazy danych w środowisku testowym, ale zazwyczaj należy unikać wdrażania baz danych w środowiskach przejściowych lub produkcyjnych. W związku z tym należy upewnić się, że jest używana niezbędna logika warunkowa, tak aby użytkownicy bazy danych i członkostwa w rolach były tworzone tylko wtedy, gdy jest to konieczne.

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat używania VSDBCMD do wdrażania projektów bazy danych, zobacz [wdrażanie projektów bazy danych](../web-deployment-in-the-enterprise/deploying-database-projects.md). Aby uzyskać wskazówki dotyczące dostosowywania wdrożeń baz danych dla różnych środowisk docelowych, zobacz [Dostosowywanie wdrożeń baz danych w wielu środowiskach](customizing-database-deployments-for-multiple-environments.md). Aby uzyskać więcej informacji na temat używania niestandardowych plików projektów programu MSBuild do kontrolowania procesu wdrażania, zobacz [Omówienie pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md) i [zrozumienie procesu kompilacji](../web-deployment-in-the-enterprise/understanding-the-build-process.md). Aby uzyskać więcej informacji na temat opcji wiersza polecenia sqlcmd, zobacz [Narzędzie sqlcmd](https://msdn.microsoft.com/library/ms162773.aspx).

> [!div class="step-by-step"]
> [Poprzednie](customizing-database-deployments-for-multiple-environments.md)
> [dalej](deploying-membership-databases-to-enterprise-environments.md)

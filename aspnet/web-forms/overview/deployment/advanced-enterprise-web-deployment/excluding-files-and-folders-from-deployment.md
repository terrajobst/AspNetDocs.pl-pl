---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment
title: Wykluczanie plików i folderów z wdrożenia | Microsoft Docs
author: jrjlee
description: W tym temacie opisano, jak można wykluczyć pliki i foldery z pakietu wdrożeniowego sieci Web podczas kompilowania i tworzenia pakietów projektu aplikacji sieci Web.
ms.author: riande
ms.date: 05/04/2012
ms.assetid: f4cc2d40-6a78-429b-b06f-07d000d4caad
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment
msc.type: authoredcontent
ms.openlocfilehash: a262ce43d7199fb1015d54d0b7c213857c360946
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78544976"
---
# <a name="excluding-files-and-folders-from-deployment"></a>Wykluczanie plików i folderów z wdrożenia

Autor [Jason Lewandowski](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie opisano, jak można wykluczyć pliki i foldery z pakietu wdrożeniowego sieci Web podczas kompilowania i tworzenia pakietów projektu aplikacji sieci Web.

Ten temat stanowi część szeregu samouczków opartych na wymaganiach dotyczących wdrażania w przedsiębiorstwie fikcyjnej firmy o nazwie Fabrikam, Inc. W tej serii samouczków jest stosowane&#x2014;przykładowe rozwiązanie&#x2014;do reprezentowania aplikacji sieci Web, [które ma](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)realistyczny poziom złożoności, w tym aplikacji ASP.NET MVC 3, usługi Windows Communication Foundation (WCF) i projektu bazy danych.

Metoda wdrażania w tym samouczku jest oparta na rozłożeniu pliku projektu dzielonego opisanym w artykule [Omówienie pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), w którym proces kompilacji jest kontrolowany przez dwa pliki&#x2014;projektu, zawierający instrukcje kompilacji, które mają zastosowanie do każdego środowiska docelowego, oraz jeden zawierający ustawienia kompilacji i wdrożenia specyficznego dla środowiska. W czasie kompilacji plik projektu specyficzny dla środowiska jest scalany z plikiem projektu Environment-niezależny od w celu utworzenia kompletnego zestawu instrukcji kompilacji.

## <a name="overview"></a>Omówienie

Podczas kompilowania projektu aplikacji sieci Web w programie Visual Studio 2010 potok publikowania w sieci Web (WPP) umożliwia rozbudowanie tego procesu kompilacji przez spakowanie skompilowanej aplikacji sieci Web do wdrożonego pakietu sieci Web. Następnie można użyć Web Deploy narzędzia Web Deployment Internet Information Services (IIS) w celu wdrożenia tego pakietu sieci Web na zdalnym serwerze sieci Web usług IIS lub ręcznie zaimportować pakiet sieci Web za pomocą Menedżera usług IIS. Ten proces tworzenia pakietów jest wyjaśniony w [kompilowaniu i pakowaniu projektów aplikacji sieci Web](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md).

Jak kontrolować, co jest zawarte w pakiecie sieci Web? Ustawienia projektu w programie Visual Studio, za pomocą bazowego pliku projektu, zapewniają wystarczającą kontrolę w wielu scenariuszach. Jednak w niektórych przypadkach możesz chcieć dostosować zawartość pakietu internetowego do określonych środowisk docelowych. Na przykład możesz chcieć uwzględnić folder plików dziennika podczas wdrażania aplikacji w środowisku testowym, ale wykluczyć ten folder podczas wdrażania aplikacji w środowisku przejściowym lub produkcyjnym. W tym temacie pokazano, jak to zrobić.

## <a name="what-gets-included-by-default"></a>Co jest domyślnie uwzględniane?

Po skonfigurowaniu właściwości projektu aplikacji sieci Web w programie Visual Studio Lista **elementy do wdrożenia** na stronie **pakowanie/publikowanie w sieci Web** umożliwia określenie, co chcesz uwzględnić w pakiecie wdrażania sieci Web. Domyślnie jest to ustawienie tylko pliki, które są **konieczne do uruchomienia tej aplikacji**.

![](excluding-files-and-folders-from-deployment/_static/image1.png)

W przypadku wybrania **tylko plików wymaganych do uruchomienia tej aplikacji**WPP spróbuje określić, które pliki powinny zostać dodane do pakietu sieci Web. Obejmuje to:

- Wszystkie dane wyjściowe kompilacji dla projektu.
- Wszystkie pliki oznaczone akcją kompilacji **zawartości**.

> [!NOTE]
> Logika określająca, które pliki mają być uwzględnione w tym pliku:   
> *%PROGRAMFILES%\MSBuild\Microsoft\VisualStudio\v10.0\Web\ Microsoft. Web. Publishing. OnlyFilesToRunTheApp. targets*

## <a name="excluding-specific-files-and-folders"></a>Wykluczanie określonych plików i folderów

W niektórych przypadkach warto bardziej precyzyjną kontrolę nad tym, które pliki i foldery są wdrażane. Jeśli wiesz, które pliki mają być wykluczone z wyprzedzeniem, a wykluczenie dotyczy wszystkich środowisk docelowych, można po prostu ustawić **akcję kompilacji** każdego pliku na **Brak**.

**Aby wykluczyć określone pliki z wdrożenia**

1. W oknie **Eksplorator rozwiązań** kliknij prawym przyciskiem myszy plik, a następnie kliknij polecenie **Właściwości**.
2. W oknie **Właściwości** , w wierszu **akcji kompilacji** , wybierz **Brak**.

Jednak takie podejście nie jest zawsze wygodne. Na przykład możesz chcieć zaróżnić, które pliki i foldery są uwzględniane zgodnie ze środowiskiem docelowym i spoza programu Visual Studio. Na przykład w przykładowym rozwiązaniu Contact Manager Obejrzyj zawartość projektu ContactManager. MVC:

![](excluding-files-and-folders-from-deployment/_static/image2.png)

- Folder wewnętrzny zawiera niektóre skrypty SQL używane przez programistę do tworzenia, upuszczania i wypełniania lokalnych baz danych w celach deweloperskich. Niczego w tym folderze nie należy wdrażać w środowisku przejściowym lub produkcyjnym.
- Folder skryptów zawiera kilka plików JavaScript. Wiele z tych plików jest zawarta w sposób czysty do obsługi debugowania lub udostępniania technologii IntelliSense w programie Visual Studio. Niektórych z tych plików nie należy wdrażać w środowisku przejściowym lub produkcyjnym. Można jednak wdrożyć je w środowisku testowym dewelopera, aby ułatwić rozwiązywanie problemów.

Chociaż można manipulować plikami projektu w celu wykluczenia określonych plików i folderów, istnieje łatwiejszy sposób. WPP zawiera mechanizm do wykluczania plików i folderów przez skompilowanie list elementów o nazwach **ExcludeFromPackageFolders** i **ExcludeFromPackageFiles**. Ten mechanizm można rozłożyć, dodając własne elementy do tych list. W tym celu należy wykonać następujące czynności wysokiego poziomu:

1. Utwórz niestandardowy plik projektu o nazwie *[nazwa projektu]. WPP. targets* w tym samym folderze, w którym znajduje się plik projektu.

    > [!NOTE]
    > Plik *. WPP. targets* musi być w tym samym folderze, w którym znajduje się plik&#x2014;projektu aplikacji sieci Web, na przykład *ContactManager. MVC. csproj*&#x2014;, a nie w tym samym folderze, co pliki niestandardowego projektu używane do kontrolowania procesu kompilacji i wdrożenia.
2. W pliku *. WPP. targets* Dodaj element **Item** .
3. W elemencie **itemmanager** Dodaj elementy **ExcludeFromPackageFolders** i **ExcludeFromPackageFiles** , aby wykluczyć określone pliki i foldery zgodnie z potrzebami.

Jest to podstawowa struktura tego pliku *. WPP. targets* :

[!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample1.xml)]

Należy zauważyć, że każdy element zawiera element metadanych elementu o nazwie **FromTarget**. Jest to opcjonalna wartość, która nie ma wpływu na proces kompilacji; po prostu służy do wskazywania, dlaczego konkretne pliki lub foldery zostały pominięte, jeśli ktoś przegląda dzienniki kompilacji.

## <a name="excluding-files-and-folders-from-a-web-package"></a>Wykluczanie plików i folderów z pakietu sieci Web

Następna procedura pokazuje, jak dodać plik *. WPP. targets* do projektu aplikacji sieci Web i jak używać pliku do wykluczania określonych plików i folderów z pakietu sieci Web podczas kompilowania projektu.

**Aby wykluczyć pliki i foldery z pakietu wdrożeniowego sieci Web**

1. Otwórz rozwiązanie w programie Visual Studio 2010.
2. W oknie **Eksplorator rozwiązań** kliknij prawym przyciskiem myszy węzeł projektu aplikacji sieci Web (na przykład **ContactManager. MVC**), wskaż polecenie **Dodaj**, a następnie kliknij pozycję **nowy element**.
3. W oknie dialogowym **Dodaj nowy element** wybierz szablon **pliku XML** .
4. W polu **Nazwa** wpisz *[nazwa projektu] * * *. WPP. targets** (na przykład **ContactManager. MVC. WPP. targets**), a następnie kliknij przycisk **Dodaj**.

    ![](excluding-files-and-folders-from-deployment/_static/image3.png)

    > [!NOTE]
    > W przypadku dodania nowego elementu do węzła głównego projektu, plik zostanie utworzony w tym samym folderze, w którym znajduje się plik projektu. Można to sprawdzić, otwierając folder w Eksploratorze Windows.
5. W pliku Dodaj element **projektu** i element **Item** :

    [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample2.xml)]
6. Jeśli chcesz wykluczyć foldery z pakietu sieci Web, Dodaj element **ExcludeFromPackageFolders** do elementu **Item** :

   1. W atrybucie **include** Podaj rozdzieloną średnikami listę folderów, które chcesz wykluczyć.
   2. W elemencie metadanych **FromTarget** Podaj znaczącą wartość, aby wskazać, dlaczego foldery są wykluczone, takie jak nazwa pliku *. WPP. targets* .

      [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample3.xml)]
7. Jeśli chcesz wykluczyć pliki z pakietu sieci Web, Dodaj element **ExcludeFromPackageFiles** do elementu **Item** :

   1. W atrybucie **include** Podaj rozdzieloną średnikami listę plików, które chcesz wykluczyć.
   2. W elemencie metadanych **FromTarget** Podaj znaczącą wartość, aby wskazać, dlaczego pliki są wykluczone, takie jak nazwa pliku *. WPP. targets* .

      [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample4.xml)]
8. Plik *[nazwa projektu]. WPP. targets* powinien teraz wyglądać następująco:

    [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample5.xml)]
9. Zapisz i zamknij plik *[nazwa projektu]. WPP. targets* .

Podczas następnego kompilowania i pakowania projektu aplikacji sieci Web WPP automatycznie wykryje plik *. WPP. targets* . Wszystkie określone pliki i foldery nie zostaną uwzględnione w pakiecie sieci Web.

## <a name="conclusion"></a>Podsumowanie

W tym temacie opisano, jak wykluczyć określone pliki i foldery podczas kompilowania pakietu sieci Web, tworząc plik Custom *. WPP. targets* w tym samym folderze co plik projektu aplikacji sieci Web.

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat używania plików projektu niestandardowych Microsoft Build Engine (MSBuild) do kontrolowania procesu wdrażania, zobacz [Omówienie pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md) i [zrozumienie procesu kompilacji](../web-deployment-in-the-enterprise/understanding-the-build-process.md). Aby uzyskać więcej informacji o procesie pakowania i wdrażania, zobacz [Kompilowanie i pakowanie projektów aplikacji sieci Web](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md), [Konfigurowanie parametrów wdrażania pakietów sieci Web](../web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment.md)i [wdrażanie pakietów internetowych](../web-deployment-in-the-enterprise/deploying-web-packages.md).

> [!div class="step-by-step"]
> [Poprzednie](deploying-membership-databases-to-enterprise-environments.md)
> [dalej](taking-web-applications-offline-with-web-deploy.md)

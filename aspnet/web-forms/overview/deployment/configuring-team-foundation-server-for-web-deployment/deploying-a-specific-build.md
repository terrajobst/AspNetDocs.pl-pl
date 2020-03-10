---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build
title: Wdrażanie określonej kompilacji | Microsoft Docs
author: jrjlee
description: W tym temacie opisano sposób wdrażania pakietów sieci Web i skryptów bazy danych z określonej poprzedniej kompilacji do nowego miejsca docelowego, takiego jak Enviro przejściowy lub produkcyjny...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: c979535f-48a3-4ec4-a633-a77889b86ddb
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build
msc.type: authoredcontent
ms.openlocfilehash: 6bede6b36c24ade928ab052e14daec1e017bd0b2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78639672"
---
# <a name="deploying-a-specific-build"></a>Wdrażanie określonej kompilacji

Autor [Jason Lewandowski](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie opisano sposób wdrażania pakietów sieci Web i skryptów bazy danych z określonej poprzedniej kompilacji do nowego miejsca docelowego, takiego jak środowisko przejściowe lub produkcyjne.

Ten temat stanowi część szeregu samouczków opartych na wymaganiach dotyczących wdrażania w przedsiębiorstwie fikcyjnej firmy o nazwie Fabrikam, Inc. W tej serii samouczków jest stosowane&#x2014;przykładowe rozwiązanie&#x2014;do reprezentowania aplikacji sieci Web, [które ma](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)realistyczny poziom złożoności, w tym aplikacji ASP.NET MVC 3, usługi Windows Communication Foundation (WCF) i projektu bazy danych.

Metoda wdrażania w tym samouczku jest oparta na rozłożeniu pliku projektu dzielonego opisanym w artykule [Omówienie pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), w którym proces kompilacji i wdrożenia jest kontrolowany przez dwa pliki&#x2014;projektu, zawierający instrukcje kompilacji, które mają zastosowanie do każdego środowiska docelowego, oraz jeden zawierający ustawienia kompilacji i wdrożenia specyficznego dla środowiska. W czasie kompilacji plik projektu specyficzny dla środowiska jest scalany z plikiem projektu Environment-niezależny od w celu utworzenia kompletnego zestawu instrukcji kompilacji.

## <a name="task-overview"></a>Przegląd zadania

Do tej pory tematy w tym zestawie samouczków koncentrują się na sposobach kompilowania, pakowania i wdrażania aplikacji sieci Web i baz danych w ramach procesu pojedynczego kroku lub zautomatyzowanego. Jednak w niektórych typowych scenariuszach warto wybrać zasoby wdrażane z listy kompilacji w folderze docelowym. Innymi słowy, Najnowsza kompilacja może nie być kompilacją, którą chcesz wdrożyć.

Zapoznaj się z scenariuszem ciągłej integracji (CI) opisanym w poprzednim temacie [Tworzenie definicji kompilacji, która obsługuje wdrażanie](creating-a-build-definition-that-supports-deployment.md). Utworzono definicję kompilacji w Team Foundation Server (TFS) 2010. Za każdym razem, gdy Deweloper sprawdza kod w programie TFS, Kompilacja zespołu kompiluje kod, tworzy pakiety sieci Web i skryptów bazy danych jako część procesu kompilacji, uruchamia wszystkie testy jednostkowe i wdraża zasoby w środowisku testowym. W zależności od zasad przechowywania skonfigurowanych podczas tworzenia definicji kompilacji TFS będzie utrzymywać określoną liczbę wcześniejszych kompilacji.

![](deploying-a-specific-build/_static/image1.png)

Teraz Załóżmy, że wykonano testy weryfikacyjne i sprawdzanie poprawności dla jednego z tych kompilacji w środowisku testowym i wszystko jest gotowe do wdrożenia aplikacji w środowisku przejściowym. W międzyczasie deweloperzy mogli zaewidencjonować nowy kod. Nie chcesz ponownie skompilować rozwiązania i wdrożyć go w środowisku przejściowym, a nie chcesz wdrożyć najnowszej kompilacji w środowisku przejściowym. Zamiast tego należy wdrożyć konkretną kompilację, która została zweryfikowana i sprawdzona na serwerach testowych.

Aby to osiągnąć, należy popowiedzieć Microsoft Build Engine (MSBuild), gdzie można znaleźć pakiety sieci Web i skrypty bazy danych utworzone przez określoną kompilację.

## <a name="overriding-the-outputroot-property"></a>Zastępowanie właściwości OutputRoot

W [przykładowym rozwiązaniu](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)plik *Publish. proj* deklaruje właściwość o nazwie **OutputRoot**. Jak sugeruje nazwa, jest to folder główny, który zawiera wszystkie elementy generowane przez proces kompilacji. W pliku *Publish. proj* można zobaczyć, że właściwość **OutputRoot** odnosi się do lokalizacji głównej dla wszystkich zasobów wdrożenia.

> [!NOTE]
> **OutputRoot** jest często używaną nazwą właściwości. Pliki C# projektu wizualizacji i Visual Basic również deklarują tę właściwość do przechowywania lokalizacji głównej dla wszystkich danych wyjściowych kompilacji.

[!code-xml[Main](deploying-a-specific-build/samples/sample1.xml)]

Jeśli chcesz, aby plik projektu wdrażał pakiety sieci Web i skrypty bazy danych z innej&#x2014;lokalizacji, takiej jak dane wyjściowe poprzedniej kompilacji&#x2014;TFS, wystarczy przesłonić Właściwość **OutputRoot** . Należy ustawić wartość właściwości na odpowiedni folder kompilacji na serwerze kompilacji zespołu. Jeśli używasz programu MSBuild z wiersza polecenia, możesz określić wartość dla **OutputRoot** jako argument wiersza polecenia:

[!code-console[Main](deploying-a-specific-build/samples/sample2.cmd)]

W tej sytuacji warto również pominąć cel&#x2014; **kompilacji** , ale nie ma miejsca na kompilowanie rozwiązania, jeśli nie planujesz użyć danych wyjściowych kompilacji. Można to zrobić, określając cele, które mają zostać wykonane z wiersza polecenia:

[!code-console[Main](deploying-a-specific-build/samples/sample3.cmd)]

Jednak w większości przypadków należy utworzyć logikę wdrożenia w definicji kompilacji TFS. Dzięki temu użytkownicy z **kolejką kompilują** uprawnienia do wyzwalania wdrożenia z dowolnej instalacji programu Visual Studio z połączeniem z serwerem TFS.

## <a name="creating-a-build-definition-to-deploy-specific-builds"></a>Tworzenie definicji kompilacji w celu wdrożenia określonych kompilacji

W następnej procedurze opisano sposób tworzenia definicji kompilacji, która umożliwia użytkownikom wyzwalanie wdrożeń w środowisku przejściowym za pomocą jednego polecenia.

W tym przypadku nie chcesz, aby definicja kompilacji mogła w rzeczywistości kompilować&#x2014;wszystkie elementy, które mają wykonać logikę wdrożenia w pliku projektu niestandardowego. Plik *Publish. proj* zawiera logikę warunkową, która pomija miejsce docelowe **kompilacji** , jeśli plik jest uruchomiony w kompilacji zespołowej. Robi to poprzez ocenę wbudowanej właściwości **BuildingInTeamBuild** , która jest automatycznie ustawiana na **true** , jeśli plik projektu jest uruchamiany w kompilacji zespołu. W związku z tym można pominąć proces kompilacji i po prostu uruchomić plik projektu, aby wdrożyć istniejącą kompilację.

**Aby utworzyć definicję kompilacji, aby wyzwolić wdrożenie ręcznie**

1. W programie Visual Studio 2010, w oknie **Team Explorer** rozwiń węzeł projektu zespołowego, kliknij prawym przyciskiem myszy pozycję **kompilacje**, a następnie kliknij pozycję **Nowa definicja kompilacji**.

    ![](deploying-a-specific-build/_static/image2.png)
2. Na karcie **Ogólne** nadaj definicji kompilacji nazwę (na przykład **DeployToStaging**) i opcjonalny opis.
3. Na karcie **wyzwalacz** wybierz pozycję **Ręczne — zaewidencjonowanie nie wyzwalają nowej kompilacji**.
4. Na karcie **Ustawienia domyślne kompilacji** w polu **Kopiuj dane wyjściowe kompilacji do poniższego folderu drop** wpisz ścieżkę Universal Naming Convention (UNC) folderu docelowego (na przykład **\\TFSBUILD\Drops**).

    ![](deploying-a-specific-build/_static/image3.png)
5. Na karcie **proces** na liście rozwijanej **plik procesu kompilacji** pozostaw wybraną opcję **DefaultTemplate. XAML** . Jest to jeden z domyślnych szablonów procesu kompilacji, które zostaną dodane do wszystkich nowych projektów zespołowych.
6. W tabeli **parametry procesu kompilacji** kliknij w wierszu **elementy do skompilowania** , a następnie kliknij przycisk **wielokropka** .

    ![](deploying-a-specific-build/_static/image4.png)
7. W oknie dialogowym **elementy do skompilowania** kliknij pozycję **Dodaj**.
8. Z listy rozwijanej **elementy typu** wybierz pozycję **pliki projektu programu MSBuild**.
9. Przejdź do lokalizacji niestandardowego pliku projektu, za pomocą którego kontrolujesz proces wdrażania, wybierz plik, a następnie kliknij przycisk **OK**.

    ![](deploying-a-specific-build/_static/image5.png)
10. W oknie dialogowym **elementy do skompilowania** kliknij przycisk **OK**.
11. W tabeli **parametry procesu kompilacji** rozwiń sekcję **Zaawansowane** .
12. W wierszu **argumenty programu MSBuild** Określ lokalizację pliku projektu specyficznego dla środowiska i Dodaj symbol zastępczy dla lokalizacji folderu kompilacji:

    [!code-console[Main](deploying-a-specific-build/samples/sample4.cmd)]

    ![](deploying-a-specific-build/_static/image6.png)

    > [!NOTE]
    > Należy przesłonić wartość **OutputRoot** za każdym razem, gdy kolejkuje kompilację. Zostało to omówione w następnej procedurze.
13. Kliknij przycisk **Save** (Zapisz).

Podczas wyzwalania kompilacji należy zaktualizować właściwość **OutputRoot** , aby wskazywała kompilację, którą chcesz wdrożyć.

**Aby wdrożyć konkretną kompilację z definicji kompilacji**

1. W oknie **Team Explorer** kliknij prawym przyciskiem myszy definicję kompilacji, a następnie kliknij pozycję **kolejka nowa kompilacja**.

    ![](deploying-a-specific-build/_static/image7.png)
2. W oknie dialogowym **kompilacja kolejki** na karcie **Parametry** rozwiń sekcję **Zaawansowane** .
3. W wierszu **argumenty programu MSBuild** Zastąp wartość właściwości **OutputRoot** lokalizacją folderu kompilacji. Na przykład:

    [!code-console[Main](deploying-a-specific-build/samples/sample5.cmd)]

    ![](deploying-a-specific-build/_static/image8.png)

    > [!NOTE]
    > Pamiętaj, aby uwzględnić końcowy ukośnik na końcu ścieżki do folderu kompilacji.
4. Kliknij pozycję **Kolejka**.

Podczas kolejkowania kompilacji, plik projektu wdraża skrypty bazy danych i pakiety internetowe z folderu Drop Build określonego we właściwości **OutputRoot** .

## <a name="conclusion"></a>Podsumowanie

W tym temacie opisano sposób publikowania zasobów wdrożenia, takich jak pakiety sieci Web i skrypty baz danych, z określonej poprzedniej kompilacji przy użyciu modelu wdrażania plików projektu rozdzielonego. Wyjaśniono, jak zastąpić właściwość **OutputRoot** i dołączać logikę wdrażania do definicji kompilacji TFS.

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat tworzenia definicji kompilacji, zobacz [Tworzenie podstawowej definicji kompilacji](https://msdn.microsoft.com/library/ms181716.aspx) i [Definiowanie procesu kompilacji](https://msdn.microsoft.com/library/ms181715.aspx). Aby uzyskać więcej wskazówek dotyczących kompilacji w kolejkach, zobacz [Queue a Build](https://msdn.microsoft.com/library/ms181722.aspx).

> [!div class="step-by-step"]
> [Poprzednie](creating-a-build-definition-that-supports-deployment.md)
> [dalej](configuring-permissions-for-team-build-deployment.md)

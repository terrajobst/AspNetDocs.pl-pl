---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/taking-web-applications-offline-with-web-deploy
title: Przełączanie aplikacji sieci Web w tryb offline za pomocą Web Deploy | Microsoft Docs
author: jrjlee
description: W tym temacie opisano, jak przełączyć aplikację sieci Web w tryb offline na czas wdrażania automatycznego przy Internet Information Services użyciu Depl (IIS) sieci Web...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 3e9f6e7d-8967-4586-94d5-d3a122f12529
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/taking-web-applications-offline-with-web-deploy
msc.type: authoredcontent
ms.openlocfilehash: ba60664a0c3daa0650cd7e7cfc4ab9da08df3440
ms.sourcegitcommit: e365196c75ce93cd8967412b1cfdc27121816110
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/07/2020
ms.locfileid: "77075141"
---
# <a name="taking-web-applications-offline-with-web-deploy"></a>Przełączanie aplikacji internetowych w tryb offline za pomocą narzędzia Web Deploy

Autor [Jason Lewandowski](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie opisano, jak przełączyć aplikację sieci Web w tryb offline na czas wdrażania automatycznego przy użyciu Web Deploy narzędzia Web Deployment Internet Information Services (IIS). Użytkownicy przeglądający aplikację sieci Web są przekierowywani do *aplikacji\_pliku offline. htm* do momentu ukończenia wdrożenia.

Ten temat stanowi część szeregu samouczków opartych na wymaganiach dotyczących wdrażania w przedsiębiorstwie fikcyjnej firmy o nazwie Fabrikam, Inc. W tej serii samouczków jest stosowane&#x2014;przykładowe rozwiązanie&#x2014;do reprezentowania aplikacji sieci Web, [które ma](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)realistyczny poziom złożoności, w tym aplikacji ASP.NET MVC 3, usługi Windows Communication Foundation (WCF) i projektu bazy danych.

Metoda wdrażania w tym samouczku jest oparta na rozłożeniu pliku projektu dzielonego opisanym w artykule [Omówienie pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), w którym proces kompilacji jest kontrolowany przez dwa pliki&#x2014;projektu, zawierający instrukcje kompilacji, które mają zastosowanie do każdego środowiska docelowego, oraz jeden zawierający ustawienia kompilacji i wdrożenia specyficznego dla środowiska. W czasie kompilacji plik projektu specyficzny dla środowiska jest scalany z plikiem projektu Environment-niezależny od w celu utworzenia kompletnego zestawu instrukcji kompilacji.

## <a name="task-overview"></a>Przegląd zadania

W wielu scenariuszach warto przełączyć aplikację sieci Web w tryb offline podczas wprowadzania zmian w powiązanych składnikach, takich jak bazy danych lub usługi sieci Web. Zazwyczaj w usługach IIS i ASP.NET można to zrobić, umieszczając plik o nazwie *App\_offline. htm* w folderze głównym witryny sieci Web lub aplikacji sieci Web usług IIS. *Aplikacja\_w trybie offline. htm* jest standardowym plikiem HTML i zwykle zawiera prosty komunikat doradzający użytkownikowi, że lokacja jest tymczasowo niedostępna z powodu konserwacji. Gdy *aplikacja\_plik w trybie offline. htm* istnieje w folderze głównym witryny sieci Web, usługi IIS automatycznie przekierują wszelkie żądania do pliku. Po zakończeniu wprowadzania aktualizacji należy usunąć *aplikację\_pliku offline. htm* , a witryna sieci Web wznawia obsługę żądań w zwykły sposób.

W przypadku używania Web Deploy do wykonywania zautomatyzowanych lub jednoetapowych wdrożeń w środowisku docelowym warto dodać i usunąć *aplikację\_pliku offline. htm* do procesu wdrożenia. W tym celu należy wykonać te zadania wysokiego poziomu:

- W pliku projektu Microsoft Build Engine (MSBuild), który służy do kontrolowania procesu wdrażania, Utwórz obiekt docelowy programu MSBuild, który kopiuje *aplikację\_pliku offline. htm* na serwer docelowy przed rozpoczęciem wykonywania jakichkolwiek zadań wdrażania.
- Dodaj inny obiekt docelowy MSBuild, który usuwa *aplikację\_pliku offline. htm* z serwera docelowego po zakończeniu wszystkich zadań wdrażania.
- W projekcie aplikacji sieci Web Utwórz plik *. WPP. targets* , który zapewnia, że *aplikacja\_w trybie offline. htm* zostanie dodana do pakietu wdrożeniowego, gdy Web Deploy jest wywoływana.

W tym temacie pokazano, jak wykonać te procedury. W zadaniach i instruktażach w tym temacie przyjęto założenie, że utworzono już rozwiązanie, które zawiera co najmniej jeden projekt aplikacji sieci Web, i że można użyć niestandardowego pliku projektu w celu sterowania procesem wdrażania zgodnie z opisem w artykule [wdrażanie w przedsiębiorstwie w sieci Web](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Alternatywnie możesz użyć przykładowego rozwiązania [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) , aby postępować zgodnie z przykładami w temacie.

## <a name="adding-an-app_offline-file-to-a-web-application-project"></a>Dodawanie aplikacji\_pliku offline do projektu aplikacji sieci Web

Pierwsze zadanie, które należy wykonać, to dodanie *aplikacji\_pliku offline* do projektu aplikacji sieci Web:

- Aby zapobiec zakłócaniu działania pliku przez proces opracowywania (nie chcesz, aby aplikacja była trwale w trybie offline), należy wywołać ją coś innego niż *aplikacja\_offline. htm*. Można na przykład nazwać *aplikację pliku\_offline-Template. htm*.
- Aby zapobiec wdrażaniu pliku jako-is, należy ustawić akcję kompilacja na **Brak**.

**Aby dodać aplikację\_pliku offline do projektu aplikacji sieci Web**

1. Otwórz rozwiązanie w programie Visual Studio 2010.
2. W oknie **Eksplorator rozwiązań** kliknij prawym przyciskiem myszy projekt aplikacji sieci Web, wskaż polecenie **Dodaj**, a następnie kliknij pozycję **nowy element**.
3. W oknie dialogowym **Dodaj nowy element** wybierz pozycję **strona HTML**.
4. W polu **Nazwa** wpisz **App\_offline-Template. htm**, a następnie kliknij przycisk **Dodaj**.

    ![](taking-web-applications-offline-with-web-deploy/_static/image1.png)
5. Dodaj prosty kod HTML, aby poinformować użytkowników, że aplikacja jest niedostępna, a następnie Zapisz plik. Nie dołączaj żadnych tagów po stronie serwera (na przykład żadnych tagów, które są poprzedzone prefiksem "ASP:"). 

    ![](taking-web-applications-offline-with-web-deploy/_static/image2.png)
6. W oknie **Eksplorator rozwiązań** kliknij prawym przyciskiem myszy nowy plik, a następnie kliknij polecenie **Właściwości**.
7. W oknie **Właściwości** , w wierszu **akcji kompilacji** , wybierz **Brak**.

    ![](taking-web-applications-offline-with-web-deploy/_static/image3.png)

## <a name="deploying-and-deleting-an-app_offline-file"></a>Wdrażanie i usuwanie aplikacji\_pliku offline

Następnym krokiem jest zmodyfikowanie logiki wdrożenia w celu skopiowania pliku na serwer docelowy na początku procesu wdrażania i usunięcie go na końcu.

> [!NOTE]
> W następnej procedurze przyjęto założenie, że używasz niestandardowego pliku projektu MSBuild do kontrolowania procesu wdrażania, zgodnie z opisem w artykule [Omówienie pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Jeśli wdrażasz bezpośrednio z programu Visual Studio, musisz użyć innego podejścia. Sayed Ibrahim Hashimi opisuje jedno z tych [metod w celu przełączenia aplikacji sieci Web w tryb offline podczas publikowania](http://sedodream.com/2012/01/08/HowToTakeYourWebAppOfflineDuringPublishing.aspx).

Aby wdrożyć *aplikację\_pliku offline* w docelowej witrynie sieci Web usług IIS, należy wywołać program MSDeploy. exe przy użyciu [dostawcy Web Deploy **contentPath** ](https://technet.microsoft.com/library/dd569034(WS.10).aspx). Dostawca **contentPath** obsługuje zarówno ścieżki katalogów fizycznych, jak i witryny sieci Web IIS lub ścieżki aplikacji, co sprawia, że jest idealnym wyborem w przypadku synchronizacji pliku między folderem projektu programu Visual Studio i aplikacją sieci Web usług IIS. Aby wdrożyć plik, polecenie MSDeploy powinno wyglądać następująco:

[!code-console[Main](taking-web-applications-offline-with-web-deploy/samples/sample1.cmd)]

Aby usunąć plik z lokacji docelowej na końcu procesu wdrażania, polecenie MSDeploy powinno wyglądać następująco:

[!code-console[Main](taking-web-applications-offline-with-web-deploy/samples/sample2.cmd)]

Aby zautomatyzować te polecenia w ramach procesu kompilowania i wdrażania, należy zintegrować je z niestandardowym plikiem projektu programu MSBuild. W następnej procedurze opisano, jak to zrobić.

**Aby wdrożyć i usunąć aplikację\_pliku offline**

1. W programie Visual Studio 2010 Otwórz plik projektu MSBuild, który kontroluje proces wdrożenia. W przykładowym rozwiązaniu [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) jest to plik *Publish. proj* .
2. W elemencie główny **projekt** Utwórz nowy element **Właściwości** , aby przechowywać zmienne dla *aplikacji\_wdrożenie w trybie offline* :

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample3.xml)]
3. Właściwość **SourceRoot** jest definiowana w innym miejscu w pliku *Publish. proj* . Wskazuje lokalizację folderu głównego dla zawartości źródłowej względem bieżącej ścieżki&#x2014;, w odniesieniu do lokalizacji pliku *Publish. proj* .
4. Dostawca **contentPath** nie akceptuje względnych ścieżek plików, dlatego przed wdrożeniem należy uzyskać ścieżkę bezwzględną do pliku źródłowego. W tym celu można użyć zadania [ConvertToAbsolutePath —](https://msdn.microsoft.com/library/bb882668.aspx) .
5. Dodaj nowy element **docelowy** o nazwie **GetAppOfflineAbsolutePath**. W tym elemencie docelowym Użyj zadania **ConvertToAbsolutePath —** , aby uzyskać bezwzględną ścieżkę do *aplikacji\_pliku szablonu offline* w folderze projektu.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample4.xml)]
6. Ten obiekt docelowy Pobiera ścieżkę względną do *aplikacji\_pliku szablonu offline* w folderze projektu i zapisuje ją na nowej właściwości jako bezwzględną ścieżkę do pliku. Atrybut **BeforeTargets** określa, że ten element docelowy ma być wykonywany przed obiektem docelowym **DeployAppOffline** , który zostanie utworzony w następnym kroku.
7. Dodaj nowy element docelowy o nazwie **DeployAppOffline**. W tym elemencie docelowym Wywołaj polecenie MSDeploy. exe, które wdraża *aplikację\_pliku offline* na docelowym serwerze sieci Web.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample5.xml)]
8. W tym przykładzie właściwość **ContactManagerIisPath** jest zdefiniowana w innym miejscu w pliku projektu. Jest to po prostu ścieżka aplikacji IIS, w postaci *[nazwa witryny sieci Web usług IIS]/[nazwa aplikacji]* . Dołączenie warunku do elementu docelowego umożliwia użytkownikom przełączanie *aplikacji\_wdrożenie w trybie offline* lub wyłączenie jej przez zmianę wartości właściwości lub podanie parametru wiersza polecenia.
9. Dodaj nowy element docelowy o nazwie **DeleteAppOffline**. W tym elemencie docelowym Wywołaj polecenie MSDeploy. exe, które usuwa *aplikację\_pliku offline* z docelowego serwera sieci Web.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample6.xml)]
10. Ostatnim zadaniem jest wywołanie tych nowych obiektów docelowych w odpowiednich punktach podczas wykonywania pliku projektu. Można to zrobić na różne sposoby. Na przykład w pliku *Publish. proj* Właściwość **FullPublishDependsOn** określa listę elementów docelowych, które muszą zostać wykonane w kolejności, gdy zostanie wywołana domyślna wartość docelowa **FullPublish** .
11. Zmodyfikuj plik projektu programu MSBuild, aby wywoływać elementy docelowe **DeployAppOffline** i **DeleteAppOffline** w odpowiednich punktach procesu publikowania.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample7.xml)]

Po uruchomieniu niestandardowego pliku projektu programu MSBuild *aplikacja\_pliku offline* zostanie wdrożona na serwerze bezpośrednio po pomyślnej kompilacji. Po zakończeniu wszystkich zadań wdrażania zostanie on usunięty z serwera.

## <a name="adding-an-app_offline-file-to-deployment-packages"></a>Dodawanie aplikacji\_pliku offline do pakietów wdrożeniowych

W zależności od sposobu skonfigurowania wdrożenia każda istniejąca zawartość w docelowej aplikacji&#x2014;sieci Web usług IIS, taka jak *aplikacja\_pliku* &#x2014;offline. htm, może zostać usunięta automatycznie podczas wdrażania pakietu internetowego do miejsca docelowego. Aby upewnić się, że *aplikacja\_w trybie offline. htm* na czas wdrażania, należy dołączyć plik do samego pakietu wdrażania w sieci Web, a nie wdrożyć go bezpośrednio na początku procesu wdrażania.

- Jeśli poprzednie zadania zostały wykonane w tym temacie, dodasz *aplikację\_pliku offline. htm* do projektu aplikacji sieci Web pod inną nazwą pliku (użyto *aplikacji\_offline-Template. htm*) i ustawisz akcję kompilacji na **none**. Te zmiany są niezbędne, aby zapobiec zakłócaniu działania pliku podczas tworzenia i debugowania. W związku z tym należy dostosować proces tworzenia pakietów, aby upewnić się, że *aplikacja\_w trybie offline. htm* jest dołączona do pakietu wdrożeniowego sieci Web.

Potok publikowania w sieci Web (WPP) używa listy elementów o nazwie **FilesForPackagingFromProject** w celu utworzenia listy plików, które powinny być dołączone do pakietu wdrożeniowego sieci Web. Zawartość pakietów sieci Web można dostosować, dodając własne elementy do tej listy. W tym celu należy wykonać następujące czynności wysokiego poziomu:

1. Utwórz niestandardowy plik projektu o nazwie *[nazwa projektu]. WPP. targets* w tym samym folderze, w którym znajduje się plik projektu.

    > [!NOTE]
    > Plik *. WPP. targets* musi być w tym samym folderze, w którym znajduje się plik&#x2014;projektu aplikacji sieci Web, na przykład *ContactManager. MVC. csproj*&#x2014;, a nie w tym samym folderze, co pliki niestandardowego projektu używane do kontrolowania procesu kompilacji i wdrożenia.
2. W pliku *. WPP. targets* Utwórz nowy obiekt docelowy MSBuild, który jest wykonywany *przed* elementem docelowym **CopyAllFilesToSingleFolderForPackage** . Jest to element docelowy WPP, który tworzy listę elementów do uwzględnienia w pakiecie.
3. W nowym miejscu docelowym Utwórz element **Item** .
4. W elemencie **element** , Dodaj element **FilesForPackagingFromProject** i określ *aplikację\_pliku offline. htm* .

Plik *. WPP. targets* powinien wyglądać następująco:

[!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample8.xml)]

Oto kluczowe punkty uwagi w tym przykładzie:

- Atrybut **BeforeTargets** wstawia ten cel do WPP, określając, że powinien być wykonany bezpośrednio przed elementem docelowym **CopyAllFilesToSingleFolderForPackage** .
- Element **FilesForPackagingFromProject** używa wartości metadanych **DestinationRelativePath** , aby zmienić nazwę pliku z *aplikacji\_offline-template. htm* na *aplikację\_offline. htm* , gdy zostanie on dodany do listy.

Następna procedura pokazuje, jak dodać plik *. WPP. targets* do projektu aplikacji sieci Web.

**Aby dodać plik. WPP. targets do pakietu wdrożeniowego sieci Web**

1. Otwórz rozwiązanie w programie Visual Studio 2010.
2. W oknie **Eksplorator rozwiązań** kliknij prawym przyciskiem myszy węzeł projektu aplikacji sieci Web (na przykład **ContactManager. MVC**), wskaż polecenie **Dodaj**, a następnie kliknij pozycję **nowy element**.
3. W oknie dialogowym **Dodaj nowy element** wybierz szablon **pliku XML** .
4. W polu **Nazwa** wpisz *[nazwa projektu] * * *. WPP. targets** (na przykład **ContactManager. MVC. WPP. targets**), a następnie kliknij przycisk **Dodaj**.

    ![](taking-web-applications-offline-with-web-deploy/_static/image4.png)

    > [!NOTE]
    > W przypadku dodania nowego elementu do węzła głównego projektu, plik zostanie utworzony w tym samym folderze, w którym znajduje się plik projektu. Można to sprawdzić, otwierając folder w Eksploratorze Windows.
5. W pliku Dodaj opisane wcześniej znaczniki programu MSBuild.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample9.xml)]
6. Zapisz i zamknij plik *[nazwa projektu]. WPP. targets* .

Podczas następnego kompilowania i pakowania projektu aplikacji sieci Web WPP automatycznie wykryje plik *. WPP. targets* . *Aplikacja\_offline-Template. htm* zostanie uwzględniona w utworzonym pakiecie wdrożeniowym sieci Web jako *aplikacja\_offline. htm*.

> [!NOTE]
> Jeśli wdrożenie nie powiedzie się, *aplikacja\_plik w trybie offline. htm* pozostanie na miejscu, a aplikacja pozostanie w trybie offline. Jest to zwykle wymagane zachowanie. Aby przywrócić aplikację do trybu online, możesz usunąć *aplikację\_pliku offline. htm* z serwera sieci Web. Alternatywnie, jeśli poprawisz wszelkie błędy i uruchomisz pomyślne wdrożenie, *aplikacja\_pliku offline. htm* zostanie usunięta.

## <a name="conclusion"></a>Podsumowanie

W tym temacie opisano, jak przełączyć aplikację sieci Web w tryb offline na czas trwania wdrożenia, publikując *aplikację\_pliku offline. htm* na serwerze docelowym na początku procesu wdrażania i usuwając ją na końcu. Uwzględniono również sposób dołączania *aplikacji\_pliku offline. htm* w pakiecie wdrożeniowym sieci Web.

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji o procesie pakowania i wdrażania, zobacz [Kompilowanie i pakowanie projektów aplikacji sieci Web](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md), [Konfigurowanie parametrów wdrażania pakietów sieci Web](../web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment.md)i [wdrażanie pakietów internetowych](../web-deployment-in-the-enterprise/deploying-web-packages.md).

W przypadku publikowania aplikacji sieci Web bezpośrednio z programu Visual Studio, zamiast korzystać z niestandardowego podejścia do pliku projektu MSBuild opisanego w tych samouczkach, należy użyć nieco innego podejścia do przełączenia aplikacji w tryb offline podczas publikowania podstawowych.

> [!div class="step-by-step"]
> [Poprzednie](excluding-files-and-folders-from-deployment.md)
> [dalej](running-windows-powershell-scripts-from-msbuild-project-files.md)

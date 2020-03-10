---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-build-process
title: Zrozumienie procesu kompilacji | Microsoft Docs
author: jrjlee
description: Ten temat zawiera Przewodnik po procesie kompilowania i wdrażania w skali przedsiębiorstwa. Podejście opisane w tym temacie używa niestandardowej kompilacji Microsoft Engin...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 5b982451-547b-4a2f-a5dc-79bc64d84d40
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-build-process
msc.type: authoredcontent
ms.openlocfilehash: 802d93f7ca987d018967275bae68b8c56d883a25
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78641142"
---
# <a name="understanding-the-build-process"></a>Objaśnienie procesu kompilacji

Autor [Jason Lewandowski](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Ten temat zawiera Przewodnik po procesie kompilowania i wdrażania w skali przedsiębiorstwa. Metoda opisana w tym temacie używa niestandardowych plików projektu Microsoft Build Engine (MSBuild) w celu zapewnienia precyzyjnej kontroli nad każdym aspektem procesu. W ramach plików projektu niestandardowe cele programu MSBuild są używane do uruchamiania narzędzi do wdrażania, takich jak narzędzie Web Deployment Internet Information Services (IIS), a narzędzie do wdrażania bazy danych VSDBCMD. exe.
> 
> > [!NOTE]
> > W poprzednim temacie opisano [plik projektu, opis](understanding-the-project-file.md)najważniejszych składników pliku projektu programu MSBuild i wprowadzono koncepcję podziału plików projektu w celu obsługi wdrożenia w wielu środowiskach docelowych. Jeśli nie znasz już tych koncepcji, przed rozpoczęciem pracy z tym tematem należy zapoznać się z tematem [plik projektu](understanding-the-project-file.md) .

Ten temat stanowi część szeregu samouczków opartych na wymaganiach dotyczących wdrażania w przedsiębiorstwie fikcyjnej firmy o nazwie Fabrikam, Inc. W tej serii samouczków jest stosowane&#x2014;przykładowe rozwiązanie&#x2014;do reprezentowania aplikacji sieci Web, [które ma](the-contact-manager-solution.md)realistyczny poziom złożoności, w tym aplikacji ASP.NET MVC 3, usługi Windows Communication Foundation (WCF) i projektu bazy danych.

Metoda wdrażania w tym samouczku jest oparta na rozłożeniu pliku projektu dzielonego opisanym w artykule [Omówienie pliku projektu](understanding-the-project-file.md), w którym proces kompilacji jest kontrolowany przez dwa pliki&#x2014;projektu, zawierający instrukcje kompilacji, które mają zastosowanie do każdego środowiska docelowego, oraz jeden zawierający ustawienia kompilacji i wdrożenia specyficznego dla środowiska. W czasie kompilacji plik projektu specyficzny dla środowiska jest scalany z plikiem projektu Environment-niezależny od w celu utworzenia kompletnego zestawu instrukcji kompilacji.

## <a name="build-and-deployment-overview"></a>Przegląd Kompilowanie i wdrażanie

W [rozwiązaniu Contact Manager](the-contact-manager-solution.md)trzy pliki kontrolują proces kompilowania i wdrażania:

- *Plik uniwersalnego projektu* (*Publish. proj*). Zawiera instrukcje kompilacji i wdrażania, które nie zmieniają się między środowiskami docelowymi.
- *Plik projektu specyficzny dla środowiska* (*ENV-dev. proj*). Zawiera ustawienia kompilacji i wdrożenia, które są specyficzne dla określonego środowiska docelowego. Na przykład można użyć pliku *ENV. proj* , aby podać ustawienia dla środowiska deweloperskiego lub testowego i utworzyć alternatywny plik o nazwie *ENV-Stage. proj* , aby podać ustawienia środowiska przejściowego.
- *Plik polecenia* (*Publish-dev. cmd*). Zawiera polecenie MSBuild. exe, które określa pliki projektu, które chcesz wykonać. Można utworzyć plik poleceń dla każdego środowiska docelowego, gdzie każdy plik zawiera polecenie MSBuild. exe, które określa inny plik projektu specyficzny dla środowiska. Dzięki temu deweloper może wdrożyć się w różnych środowiskach, uruchamiając odpowiedni plik polecenia.

W przykładowym rozwiązaniu można znaleźć te trzy pliki w folderze publikowanie rozwiązania.

![](understanding-the-build-process/_static/image1.png)

Zanim zaczniesz bardziej szczegółowo oglądać te pliki, przyjrzyjmy się, jak cały proces kompilacji działa podczas korzystania z tego podejścia. Na wysokim poziomie proces kompilowania i wdrażania wygląda następująco:

![](understanding-the-build-process/_static/image2.png)

W pierwszej kolejności następuje, że dwa pliki&#x2014;projektu, które zawierają instrukcje dotyczące kompilowania i wdrażania, a jedna z nich zawiera ustawienia&#x2014;specyficzne dla środowiska, są scalane w jeden plik projektu. Następnie program MSBuild działa przez instrukcje w pliku projektu. Kompiluje wszystkie projekty w rozwiązaniu przy użyciu pliku projektu dla każdego projektu. Następnie wywołuje on inne narzędzia, takie jak Web Deploy (MSDeploy. exe) i narzędzie VSDBCMD do wdrażania zawartości sieci Web i baz danych w środowisku docelowym.

Od początku do końca proces kompilowania i wdrażania wykonuje następujące zadania:

1. Usuwa zawartość katalogu wyjściowego w przygotowaniu dla nowej kompilacji.
2. Kompiluje każdy projekt w rozwiązaniu:

    1. W przypadku projektów&#x2014;sieci Web w tym przypadku aplikacja sieci Web ASP.NET MVC i usługa&#x2014;sieci Web programu WCF proces kompilacji tworzy pakiet wdrożeniowy sieci Web dla każdego projektu.
    2. W przypadku projektów bazy danych proces kompilacji tworzy manifest wdrożenia (plik. deploymanifest) dla każdego projektu.
3. Używa narzędzia VSDBCMD. exe do wdrożenia każdego projektu bazy danych w rozwiązaniu przy użyciu różnych właściwości z plików&#x2014;projektu, docelowego ciągu połączenia i nazwy&#x2014;bazy danych wraz z plikiem. deploymanifest.
4. Używa narzędzia MSDeploy. exe do wdrożenia każdego projektu sieci Web w rozwiązaniu przy użyciu różnych właściwości z plików projektu w celu sterowania procesem wdrażania.

Możesz użyć przykładowego rozwiązania do śledzenia tego procesu bardziej szczegółowo.

> [!NOTE]
> Aby uzyskać wskazówki dotyczące dostosowywania plików projektu specyficznych dla środowiska dla własnych środowisk serwera, zobacz [Konfigurowanie właściwości wdrożenia dla środowiska docelowego](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).

## <a name="invoking-the-build-and-deployment-process"></a>Wywoływanie procesu Kompilowanie i wdrażanie

Aby wdrożyć rozwiązanie Contact Manager w środowisku testowym dewelopera, programista uruchamia plik poleceń *Publish-dev. cmd* . Powoduje to wywołanie programu MSBuild. exe, określającego plik *Publish. proj* jako pliku projektu do wykonania i *ENV-dev. proj* jako wartości parametru.

[!code-console[Main](understanding-the-build-process/samples/sample1.cmd)]

> [!NOTE]
> Przełącznik **/FL** (Short for **/FileLogger**) rejestruje dane wyjściowe kompilacji do pliku o nazwie *MSBuild. log* w bieżącym katalogu. Aby uzyskać więcej informacji, zobacz [informacje dotyczące wiersza polecenia programu MSBuild](https://msdn.microsoft.com/library/ms164311.aspx).

W tym momencie program MSBuild zacznie działać, ładuje plik *Publish. proj* i zacznie przetwarzać zawarte w nim instrukcje. Pierwsza instrukcja instruuje program MSBuild, aby zaimportował plik projektu, który określa parametr **TargetEnvPropsFile** .

[!code-xml[Main](understanding-the-build-process/samples/sample2.xml)]

Parametr **TargetEnvPropsFile** określa plik *ENV. proj* , dlatego program MSBuild Scala zawartość pliku *ENV-dev. proj* z plikiem *Publish. proj* .

Kolejne elementy, które program MSBuild napotka w scalonym pliku projektu, są grupami właściwości. Właściwości są przetwarzane w kolejności, w jakiej występują w pliku. Program MSBuild tworzy parę klucz-wartość dla każdej właściwości, co zapewnia spełnienie wszelkich określonych warunków. Właściwości zdefiniowane w dalszej części pliku zastąpią wszystkie właściwości o tej samej nazwie zdefiniowanej wcześniej w pliku. Rozważmy na przykład właściwości **OutputRoot** .

[!code-xml[Main](understanding-the-build-process/samples/sample3.xml)]

Gdy MSBuild przetwarza pierwszy element **OutputRoot** , dostarczenie parametru o podobnej nazwie nie zostało dostarczone, ustawia wartość właściwości **OutputRoot** na **. \Publish\Out**. Gdy napotka drugi element **OutputRoot** , jeśli wynikiem warunku jest **true**, nadpisze wartość właściwości **OutputRoot** wartością parametru **OutDir** .

Następny element, którego wystąpienie MSBuild występuje, jest grupą pojedynczego elementu zawierającego element o nazwie **ProjectsToBuild**.

[!code-xml[Main](understanding-the-build-process/samples/sample4.xml)]

Program MSBuild przetwarza tę instrukcję, tworząc listę elementów o nazwie **ProjectsToBuild**. W takim przypadku lista elementów zawiera jedną wartość&#x2014;ścieżka i nazwa pliku rozwiązania.

W tym momencie pozostałe elementy są obiektami docelowymi. Elementy docelowe są przetwarzane inaczej od właściwości i&#x2014;elementów, dlatego elementy docelowe nie są przetwarzane, chyba że zostały jawnie określone przez użytkownika lub wywołane przez inną konstrukcję w pliku projektu. Odwołaj, że tag otwierającego **projektu** zawiera atrybut **DefaultTargets —** .

[!code-xml[Main](understanding-the-build-process/samples/sample5.xml)]

Powoduje to, że program MSBuild wywoła element docelowy **FullPublish** , jeśli nie określono elementów docelowych podczas wywoływania programu MSBuild. exe. Element docelowy **FullPublish** nie zawiera żadnych zadań. Zamiast tego określa listę zależności.

[!code-xml[Main](understanding-the-build-process/samples/sample6.xml)]

Ta zależność informuje program MSBuild, że w celu wykonania elementu docelowego **FullPublish** musi wywołać tę listę elementów docelowych w podanej kolejności:

1. Musi wywołać **czysty** element docelowy.
2. Musi wywołać element docelowy **BuildProjects** .
3. Musi wywołać element docelowy **GatherPackagesForPublishing** .
4. Musi wywołać element docelowy **PublishDbPackages** .
5. Musi wywołać element docelowy **PublishWebPackages** .

### <a name="the-clean-target"></a>Czysty element docelowy

**Czysty** obiekt docelowy zasadniczo usuwa katalog wyjściowy i całą jego zawartość jako przygotowanie do nowej kompilacji.

[!code-xml[Main](understanding-the-build-process/samples/sample7.xml)]

Zwróć uwagę, że element docelowy zawiera element **Item** . Podczas definiowania właściwości lub elementów w elemencie **docelowym** są tworzone *dynamiczne* właściwości i elementy. Innymi słowy, właściwości lub elementy nie są przetwarzane do momentu wykonania obiektu docelowego. Katalog wyjściowy może nie istnieć lub zawierać pliki do momentu rozpoczęcia procesu kompilacji, więc nie można utworzyć listy **\_FilesToDelete** jako elementu statycznego; musisz poczekać do momentu wykonania operacji. W związku z tym można skompilować listę jako element dynamiczny w miejscu docelowym.

> [!NOTE]
> W tym przypadku, ponieważ **czysty** element docelowy jest pierwszy do wykonania, nie ma rzeczywistej potrzeby używania dynamicznej grupy elementów. Jednak dobrym sposobem jest użycie właściwości dynamicznych i elementów w tym typie scenariusza, ponieważ może być konieczne wykonanie obiektów docelowych w innej kolejności w pewnym momencie.  
> Należy również zadbać o uniknięcie deklarowania elementów, które nigdy nie będą używane. Jeśli masz elementy, które będą używane tylko przez określony obiekt docelowy, Rozważ umieszczenie ich wewnątrz elementu docelowego, aby usunąć wszelkie niepotrzebne obciążenia w procesie kompilacji.

Elementy dynamiczne poza tym **czysty** element docelowy jest dość prosty i korzysta z wbudowanego **komunikatu**, **usuwania**i **RemoveDir —** zadań do:

1. Wyślij wiadomość do rejestratora.
2. Utwórz listę plików do usunięcia.
3. Usuń pliki.
4. Usuń katalog wyjściowy.

### <a name="the-buildprojects-target"></a>Element docelowy BuildProjects

Obiekt docelowy **BuildProjects** zasadniczo kompiluje wszystkie projekty w przykładowym rozwiązaniu.

[!code-xml[Main](understanding-the-build-process/samples/sample8.xml)]

Ten element docelowy został opisany szczegółowo w poprzednim temacie, [opisując plik projektu](understanding-the-project-file.md), aby zilustrować, w jaki sposób zadania i elementy docelowe odwołują się do właściwości i elementów. W tym momencie jesteś głównie zainteresowani zadaniami programu **MSBuild** . To zadanie służy do kompilowania wielu projektów. Zadanie nie tworzy nowego wystąpienia programu MSBuild. exe; używa bieżącego uruchomionego wystąpienia do kompilowania każdego projektu. Kluczowe znaczenie w tym przykładzie są właściwościami wdrożenia:

- Właściwość **DeployOnBuild** instruuje program MSBuild, aby uruchomił wszystkie instrukcje wdrożenia w ustawieniach projektu, gdy kompilacja każdego projektu została ukończona.
- Właściwość **DeployTarget** identyfikuje obiekt docelowy, który ma zostać wywołany po skompilowaniu projektu. W takim przypadku obiekt docelowy **pakietu** kompiluje dane wyjściowe projektu do wdrożonego pakietu sieci Web.

> [!NOTE]
> Obiekt docelowy **pakietu** wywołuje potok publikowania w sieci Web (WPP), który zapewnia integrację między programem MSBuild i Web Deploy. Jeśli chcesz zapoznać się z wbudowanymi obiektami docelowymi, które zapewnia WPP, przejrzyj plik *Microsoft. Web. Publishing. targets* w folderze% ProgramFiles (x86)% \ MSBuild\Microsoft\VisualStudio\v10.0\Web.

### <a name="the-gatherpackagesforpublishing-target"></a>Element docelowy GatherPackagesForPublishing

Jeśli analizujesz element docelowy **GatherPackagesForPublishing** , zauważysz, że nie zawiera on żadnych zadań. Zamiast tego zawiera jedną grupę elementów, która definiuje trzy elementy dynamiczne.

[!code-xml[Main](understanding-the-build-process/samples/sample9.xml)]

Te elementy odnoszą się do pakietów wdrożeniowych, które zostały utworzone podczas wykonywania elementu docelowego **BuildProjects** . Nie można zdefiniować tych elementów statycznie w pliku projektu, ponieważ pliki, do których odwołują się elementy, nie istnieją do momentu wykonania elementu docelowego **BuildProjects** . Zamiast tego elementy muszą być zdefiniowane dynamicznie w obiekcie docelowym, który nie jest wywoływany do momentu wykonania **BuildProjects** elementu docelowego.

Elementy nie są używane w tym elemencie docelowym&#x2014;po prostu kompilują elementy i metadane skojarzone z każdą wartością elementu. Po przetworzeniu tych elementów element **PublishPackages** będzie zawierał dwie wartości, ścieżkę do pliku *ContactManager. MVC. deploy. cmd* i ścieżkę do pliku *ContactManager. Service. deploy. cmd* . Web Deploy tworzy te pliki jako część pakietu sieci Web dla każdego projektu i są to pliki, które należy wywołać na serwerze docelowym w celu wdrożenia pakietów. Jeśli otworzysz jeden z tych plików, zobaczysz polecenie MSDeploy. exe z różnymi wartościami parametrów specyficznych dla kompilacji.

Element **DbPublishPackages** będzie zawierać pojedynczą wartość, ścieżkę do pliku *ContactManager. Database. deploymanifest* .

> [!NOTE]
> Podczas kompilowania projektu bazy danych jest generowany plik. deploymanifest, który używa tego samego schematu co plik projektu MSBuild. Zawiera wszystkie informacje wymagane do wdrożenia bazy danych, w tym lokalizację schematu bazy danych (DbSchema) i szczegółowe informacje o wszystkich skryptach przed wdrożeniem i po wdrożeniu. Aby uzyskać więcej informacji, zobacz [omówienie Kompilowanie i wdrażanie bazy danych](https://msdn.microsoft.com/library/aa833165.aspx).

Dowiesz się więcej na temat tworzenia i używania pakietów wdrożeniowych oraz manifestów wdrażania bazy danych, a następnie tworzenia [i pakowania projektów aplikacji sieci Web](building-and-packaging-web-application-projects.md) oraz [wdrażania projektów bazy danych](deploying-database-projects.md).

### <a name="the-publishdbpackages-target"></a>Element docelowy PublishDbPackages

Krótko mówiąc, obiekt docelowy **PublishDbPackages** wywołuje narzędzie VSDBCMD, aby wdrożyć bazę danych **ContactManager** w środowisku docelowym. Konfigurowanie wdrażania bazy danych obejmuje wiele decyzji i wszystkie szczegóły. dowiesz się więcej na ten temat w temacie [wdrażanie projektów bazy danych](deploying-database-projects.md) i [Dostosowywanie wdrożeń baz danych w wielu środowiskach](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md). W tym temacie omówiono sposób działania tego elementu docelowego w rzeczywistości.

Najpierw należy zauważyć, że tag otwierający zawiera **atrybut Output** .

[!code-xml[Main](understanding-the-build-process/samples/sample10.xml)]

Jest to przykład tworzenia *wsadowych elementów docelowych*. W plikach projektów programu MSBuild przetwarzanie wsadowe jest techniką do iteracji nad kolekcjami. Wartość atrybutu Outputs **"% (DbPublishPackages. Identity)"** odwołuje się do właściwości Metadata **Identity** listy elementów **DbPublishPackages** . Ten zapis, * Output *=% * * * (ITEMLIST. ItemMetaDataName)* , jest tłumaczony jako:

- Podziel elementy w **DbPublishPackages** na partie elementów, które zawierają tę samą wartość metadanych **tożsamości** .
- Wykonaj element docelowy raz na partię.

> [!NOTE]
> **Tożsamość** jest jedną z [wbudowanych wartości metadanych](https://msdn.microsoft.com/library/ms164313.aspx) , które są przypisane do każdego elementu podczas tworzenia. Odnosi się do wartości atrybutu **include** w elemencie&#x2014; **Item** innymi słowy, ścieżki i nazwy pliku elementu.

W tym przypadku, ponieważ nigdy nie powinno być więcej niż jeden element o tej samej ścieżce i nazwie pliku, firma Microsoft pracuje nad rozmiarem partii. Element docelowy jest wykonywany jednokrotnie dla każdego pakietu bazy danych.

Można zobaczyć podobną notację we właściwości **\_cmd** , która kompiluje polecenie VSDBCMD z odpowiednimi przełącznikami.

[!code-xml[Main](understanding-the-build-process/samples/sample11.xml)]

W takim przypadku **% (DbPublishPackages. DatabaseConnectionString)** , **% (DbPublishPackages. TargetDatabase)** i **% (DbPublishPackages. FullPath)** odwołuje się do wartości metadanych kolekcji elementów **DbPublishPackages** . Właściwość **\_cmd** jest używana przez zadanie **exec** , które wywołuje polecenie.

[!code-xml[Main](understanding-the-build-process/samples/sample12.xml)]

W wyniku tej notacji zadanie **exec** utworzy partie na podstawie unikatowych kombinacji wartości metadanych **DatabaseConnectionString**, **TargetDatabase**i **FullPath** , a zadanie zostanie wykonane raz dla każdej partii. Jest to przykład tworzenia *wsadowych zadań*. Jednak ponieważ przetwarzanie wsadowe na poziomie docelowym zostało już podzielone z kolekcji elementów na partie pojedynczego elementu, zadanie **exec** zostanie uruchomione raz i tylko raz dla każdej iteracji elementu docelowego. Innymi słowy to zadanie wywołuje narzędzie VSDBCMD raz dla każdego pakietu bazy danych w rozwiązaniu.

> [!NOTE]
> Aby uzyskać więcej informacji na temat tworzenia wsadowych obiektów docelowych i zadań, zobacz Tworzenie [wsadowe](https://msdn.microsoft.com/library/ms171473.aspx)programu MSBuild, [metadane elementu w docelowej partii](https://msdn.microsoft.com/library/ms228229.aspx)i [metadane elementu w ramach zadań wsadowych](https://msdn.microsoft.com/library/ms171474.aspx).

### <a name="the-publishwebpackages-target"></a>Element docelowy PublishWebPackages

W tym momencie został wywołany element docelowy **BuildProjects** , który generuje pakiet wdrożeniowy sieci Web dla każdego projektu w przykładowym rozwiązaniu. Towarzyszący każdemu pakietowi jest plik *Deploy. cmd* zawierający polecenia MSDeploy. exe wymagane do wdrożenia pakietu w środowisku docelowym oraz plik *SetParameters. XML* , który określa niezbędne szczegóły środowiska docelowego. Został również wywołany element docelowy **GatherPackagesForPublishing** , który generuje kolekcję elementów zawierającą pliki *Deploy. cmd* , które Cię interesują. Zasadniczo obiekt docelowy **PublishWebPackages** wykonuje następujące funkcje:

- Służy do manipulowania plikiem *SetParameters. XML* każdego pakietu, aby zawierała poprawne szczegóły dotyczące środowiska docelowego przy użyciu zadania **XmlPoke —** .
- Wywołuje plik *Deploy. cmd* dla każdego pakietu przy użyciu odpowiednich przełączników.

Tak samo jak element docelowy **PublishDbPackages** , obiekt docelowy **PublishWebPackages** używa docelowej partii, aby upewnić się, że element docelowy jest wykonywany raz dla każdego pakietu sieci Web.

[!code-xml[Main](understanding-the-build-process/samples/sample13.xml)]

W obszarze docelowym zadanie **exec** służy do uruchamiania pliku *Deploy. cmd* dla każdego pakietu sieci Web.

[!code-xml[Main](understanding-the-build-process/samples/sample14.xml)]

Aby uzyskać więcej informacji na temat konfigurowania wdrożenia pakietów sieci Web, zobacz [Kompilowanie i pakowanie projektów aplikacji sieci Web](building-and-packaging-web-application-projects.md).

## <a name="conclusion"></a>Podsumowanie

W tym temacie przedstawiono Przewodnik dotyczący sposobu, w jaki podzielone pliki projektu są używane do kontrolowania procesu kompilowania i wdrażania, od początku do końca dla przykładowego rozwiązania Contact Manager. Korzystając z tej metody, można uruchamiać złożone wdrożenia w skali korporacyjnej w jednym powtarzalnym kroku, po prostu uruchamiając plik poleceń specyficzny dla środowiska.

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać bardziej szczegółowe wprowadzenie do plików projektu i WPP, zobacz [wewnątrz Microsoft Build Engine: korzystanie z programu MSBuild i Team Foundation Build](http://amzn.com/0735645248) przez Sayed Ibrahim Hashimi i William BARTHOLOMEW, ISBN: 978-0-7356-4524-0.

> [!div class="step-by-step"]
> [Poprzednie](understanding-the-project-file.md)
> [dalej](building-and-packaging-web-application-projects.md)

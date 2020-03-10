---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment
title: Konfigurowanie parametrów wdrożenia pakietu sieci Web | Microsoft Docs
author: jrjlee
description: W tym temacie opisano sposób ustawiania wartości parametrów, takich jak nazwy aplikacji sieci Web Internet Information Services (IIS), parametry połączenia i punkty końcowe usługi,...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 37947d79-ab1e-4ba9-9017-52e7a2757414
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment
msc.type: authoredcontent
ms.openlocfilehash: f04ace98d81a33053b10cab7e40dbd75a6c0992c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78544955"
---
# <a name="configuring-parameters-for-web-package-deployment"></a>Konfigurowanie parametrów na potrzeby wdrożenia pakietu internetowego

Autor [Jason Lewandowski](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie opisano sposób ustawiania wartości parametrów, takich Internet Information Services jak nazwy aplikacji sieci Web (IIS), parametry połączeń i punkty końcowe usługi, podczas wdrażania pakietu sieci Web na zdalnym serwerze sieci Web usług IIS.

Podczas kompilowania projektu aplikacji sieci Web, proces kompilowania i tworzenia pakietów generuje trzy pliki kluczy:

- Plik *[nazwa projektu]. zip* . Jest to pakiet wdrażania sieci Web dla projektu aplikacji sieci Web. Ten pakiet zawiera wszystkie zestawy, pliki, skrypty bazy danych i zasoby wymagane do ponownego utworzenia aplikacji sieci Web na zdalnym serwerze sieci Web usług IIS.
- Plik *[nazwa projektu]. deploy. cmd* . Zawiera zestaw sparametryzowanych poleceń Web Deploy (MSDeploy. exe), które publikują pakiet wdrożeniowy sieci Web na zdalnym serwerze sieci Web usług IIS.
- *[Nazwa projektu]. SetParameters. XML* — plik. Zapewnia to zestaw wartości parametrów polecenia MSDeploy. exe. Możesz zaktualizować wartości w tym pliku i przekazać go do Web Deploy jako parametr wiersza polecenia podczas wdrażania pakietu sieci Web.

> [!NOTE]
> Aby uzyskać więcej informacji na temat procesu kompilowania i tworzenia pakietów, zobacz [Kompilowanie i pakowanie projektów aplikacji sieci Web](building-and-packaging-web-application-projects.md).

Plik *SetParameters. XML* jest generowana dynamicznie z pliku projektu aplikacji sieci Web i wszystkich plików konfiguracji w ramach projektu. Podczas kompilowania i pakowania projektu potok publikowania w sieci Web (WPP) automatycznie wykrywa wiele zmiennych, które mogą ulec zmianie między środowiskami wdrażania, takimi jak docelowa aplikacja sieci Web IIS i wszystkie parametry połączenia bazy danych. Te wartości są automatycznie sparametryzowane w pakiecie wdrażania sieci Web i dodawane do pliku *SetParameters. XML* . Na przykład, jeśli dodasz parametry połączenia do pliku *Web. config* w projekcie aplikacji sieci Web, proces kompilacji wykryje tę zmianę i doda odpowiednio wpis do pliku *SetParameters. XML* .

W wielu przypadkach ta automatyczna parametryzacja będzie wystarczająca. Jeśli jednak użytkownicy muszą zmieniać inne ustawienia między środowiskami wdrażania, takimi jak ustawienia aplikacji lub adresy URL punktów końcowych usługi, należy popowiedzieć WPP, aby Sparametryzuj te wartości w pakiecie wdrożeniowym i dodać odpowiednie wpisy do pliku *SetParameters. XML* . W poniższych sekcjach wyjaśniono, jak to zrobić.

### <a name="automatic-parameterization"></a>Automatyczne parametryzacja

Podczas kompilowania i tworzenia pakietów aplikacji sieci Web WPP automatycznie Sparametryzuj te czynności:

- Docelowa ścieżka i nazwa aplikacji sieci Web usług IIS.
- Wszystkie parametry połączenia w pliku *Web. config* .
- Parametry połączenia dla dowolnych baz danych, które można dodać do karty **pakiet/Publikuj SQL** na stronach właściwości projektu.

Na przykład jeśli planujesz skompilować i spakować przykładowe rozwiązanie [Contact Manager](the-contact-manager-solution.md) bez dotknięcia procesu parametryzacja w jakikolwiek sposób, WPP wygeneruje ten plik *ContactManager. MVC. SetParameters. XML* :

[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample1.xml)]

W takim przypadku:

- **Nazwa aplikacji sieci Web usług IIS** jest ścieżką IIS, w której chcesz wdrożyć aplikację sieci Web. Wartość domyślna jest pobierana ze strony **pakowanie/publikowanie** na stronach właściwości projektu.
- Parametr parametrów **połączenia ApplicationServices-Web. config** został wygenerowany z elementu **connectionStrings/add** w pliku *Web. config* . Reprezentuje parametry połączenia, które powinny być używane przez aplikację w celu kontaktowania się z bazą danych członkostwa. Wartość wprowadzona w tym miejscu zostanie zastąpiona wdrożonym plikiem *Web. config* . Wartość domyślna jest pobierana z pliku *Web. config* przed wdrożeniem.

WPP również parameterizes te właściwości w pakiecie wdrożeniowym, który generuje. Wartości tych właściwości można podać podczas instalowania pakietu wdrożeniowego. Jeśli pakiet zostanie zainstalowany ręcznie za pomocą Menedżera usług IIS, zgodnie z opisem w temacie [Ręczne instalowanie pakietów sieci Web](manually-installing-web-packages.md), Kreator instalacji będzie monitował o podanie wartości parametrów. Jeśli pakiet jest instalowany zdalnie przy użyciu pliku *. deploy. cmd* , zgodnie z opisem w artykule [wdrażanie pakietów sieci Web](deploying-web-packages.md), Web Deploy będzie szukać tego pliku *SetParameters. XML* w celu udostępnienia wartości parametrów. Można edytować wartości w pliku *SetParameters. XML* ręcznie lub dostosować plik jako część zautomatyzowanego procesu kompilowania i wdrażania. Ten proces został opisany bardziej szczegółowo w dalszej części tego tematu.

### <a name="custom-parameterization"></a>Niestandardowy parametryzacja

W bardziej złożonych scenariuszach wdrażania często warto Sparametryzuj dodatkowe właściwości przed wdrożeniem projektu. Ogólnie mówiąc, należy Sparametryzuj wszystkie właściwości i ustawienia, które będą się różnić między środowiskami docelowymi. Mogą to być następujące:

- Punkty końcowe usługi w pliku *Web. config* .
- Ustawienia aplikacji w pliku *Web. config* .
- Wszystkie inne właściwości deklaratywne, które mają być wyświetlane użytkownikom do określenia.

Najprostszym sposobem Sparametryzuj tych właściwości jest dodanie pliku *Parameters. XML* do folderu głównego projektu aplikacji sieci Web. Na przykład w rozwiązaniu Contact Manager projekt ContactManager. MVC zawiera plik *Parameters. XML* w folderze głównym.

![](configuring-parameters-for-web-package-deployment/_static/image1.png)

Jeśli otworzysz ten plik, zobaczysz, że zawiera on pojedynczy wpis **parametru** . Wpis używa zapytania XML Path Language (XPath) do lokalizowania i Sparametryzuj adresu URL punktu końcowego usługi ContactService Windows Communication Foundation (WCF) w pliku *Web. config* .

[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample2.xml)]

Oprócz parametryzacja adresu URL punktu końcowego w pakiecie wdrożeniowym WPP dodaje również odpowiedni wpis do pliku *SetParameters. XML* , który jest generowany wraz z pakietem wdrożeniowym.

[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample3.xml)]

W przypadku ręcznej instalacji pakietu wdrożeniowego Menedżer usług IIS wyświetli monit o podanie adresu punktu końcowego usługi obok właściwości, które zostały sparametryzowane automatycznie. W przypadku instalowania pakietu wdrożeniowego, uruchamiając plik *. deploy. cmd* , można edytować plik *SetParameters. XML* w celu udostępnienia wartości dla adresu punktu końcowego usługi wraz z wartościami właściwości, które zostały sparametryzowane automatycznie.

Aby uzyskać szczegółowe informacje na temat sposobu tworzenia pliku *Parameters. XML* , zobacz [How to: use Parameters to configure Deployment Settings on a Package](https://msdn.microsoft.com/library/ff398068.aspx). Procedura o nazwie **do używania parametrów wdrożenia dla ustawień pliku Web. config** zawiera instrukcje krok po kroku.

## <a name="modifying-the-setparametersxml-file"></a>Modyfikowanie pliku SetParameters. XML

Jeśli planujesz wdrożyć pakiet aplikacji sieci&#x2014;Web ręcznie, uruchamiając plik *. deploy. cmd* lub uruchamiając program MSDeploy. exe z wiersza&#x2014;polecenia, nie ma nic, aby uniemożliwić ręczne edytowanie pliku *SetParameters. XML* przed wdrożeniem. Jeśli jednak pracujesz nad rozwiązaniem w skali przedsiębiorstwa, może być konieczne wdrożenie pakietu aplikacji sieci Web w ramach większego, zautomatyzowanego procesu kompilowania i wdrażania. W tym scenariuszu potrzebujesz Microsoft Build Engine (MSBuild), aby zmodyfikować plik *SetParameters. XML* . Można to zrobić za pomocą zadania MSBuild **XmlPoke —** .

[Przykładowe rozwiązanie Contact Manager](the-contact-manager-solution.md) ilustruje ten proces. Poniższe przykłady kodu zostały zmodyfikowane w celu wyświetlenia tylko szczegółowych informacji, które są istotne dla tego przykładu.

> [!NOTE]
> Aby uzyskać szerszy przegląd modelu plików projektu w przykładowym rozwiązaniu, a ponadto wprowadzenie do niestandardowych plików projektu, zobacz [Opis pliku projektu](understanding-the-project-file.md) i [Opis procesu kompilacji](understanding-the-build-process.md).

Najpierw wartości parametrów, które są istotne, są definiowane jako właściwości w pliku projektu specyficznym dla środowiska (na przykład *ENV-dev. proj*).

[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample4.xml)]

> [!NOTE]
> Aby uzyskać wskazówki dotyczące dostosowywania plików projektu specyficznych dla środowiska dla własnych środowisk serwera, zobacz [Konfigurowanie właściwości wdrożenia dla środowiska docelowego](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).

Następnie plik *Publish. proj* importuje te właściwości. Ponieważ każdy plik *SetParameters. XML* jest skojarzony z plikiem *Deploy. cmd* i ostatecznie chcemy, aby plik projektu wywoływał każdy plik *. deploy. cmd* , plik projektu tworzy *element* MSBuild dla każdego pliku *. deploy. cmd* i definiuje właściwości zainteresowania jako *metadane elementu*.

[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample5.xml)]

W takim przypadku:

- Wartość metadanych **ParametersXml** wskazuje lokalizację pliku *SetParameters. XML* .
- Wartość **IisWebAppName** jest ścieżką IIS, w której chcesz wdrożyć aplikację sieci Web.
- Wartość **MembershipDBConnectionString** jest parametrami połączenia dla bazy danych członkostwa, a wartość **MembershipDBConnectionName** jest atrybutem **nazwy** odpowiadającego parametru w pliku *SetParameters. XML* .
- Wartość **ServiceEndpointValue** jest adresem punktu końcowego usługi WCF na serwerze docelowym, a wartość **ServiceEndpointParamName** jest atrybutem nazwy odpowiadającego parametru w pliku *XML SetParameters.*

Na koniec w pliku *Publish. proj* element docelowy **PublishWebPackages** używa zadania **XmlPoke —** , aby zmodyfikować te wartości w pliku *SetParameters. XML* .

[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample6.xml)]

Należy zauważyć, że każde zadanie **XmlPoke —** określa cztery wartości atrybutów:

- Atrybut **XmlInputPath** informuje zadanie, gdzie znaleźć plik, który chcesz zmodyfikować.
- Atrybut **zapytania** jest zapytanie XPath, które IDENTYFIKUJE węzeł XML, który ma zostać zmieniony.
- Atrybut **Value** jest nową wartością, którą chcesz wstawić do wybranego węzła XML.
- Atrybut **Condition** jest kryterium, na którym zadanie powinno być uruchamiane lub nie uruchamiane. W takich przypadkach warunek gwarantuje, że nie próbujesz wstawić wartości null lub pustej do pliku *SetParameters. XML* .

## <a name="conclusion"></a>Podsumowanie

W tym temacie opisano rolę pliku *SetParameters. XML* i wyjaśniono, jak jest generowany podczas kompilowania projektu aplikacji sieci Web. Wyjaśniono, jak można Sparametryzuj dodatkowe ustawienia przez dodanie pliku *Parameters. XML* do projektu. Opisano w nim również, jak można zmodyfikować plik *SetParameters. XML* w ramach większego, zautomatyzowanego procesu kompilacji, używając zadania **XmlPoke —** w plikach projektu.

W następnym temacie, [wdrażanie pakietów internetowych](deploying-web-packages.md), opisano, jak można wdrożyć pakiet sieci Web, uruchamiając plik *. deploy. cmd* lub bezpośrednio używając poleceń MSDeploy. exe. W obu przypadkach można określić plik *SetParameters. XML* jako parametr wdrożenia.

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać informacje na temat tworzenia pakietów sieci Web, zobacz [Kompilowanie i pakowanie projektów aplikacji sieci Web](building-and-packaging-web-application-projects.md). Aby uzyskać wskazówki dotyczące rzeczywistego wdrażania pakietu sieci Web, zobacz [wdrażanie pakietów internetowych](deploying-web-packages.md). Aby zapoznać się ze szczegółowymi krokami dotyczącymi sposobu tworzenia pliku *Parameters. XML* , zobacz [How to: use Parameters to configure Deployment Settings](https://msdn.microsoft.com/library/ff398068.aspx)on a Package.

Aby uzyskać więcej ogólnych informacji na temat parametryzacja w Web Deploy, zobacz [Web Deploy parametryzacja in Action](https://go.microsoft.com/?linkid=9805119) (wpis w blogu).

> [!div class="step-by-step"]
> [Poprzednie](building-and-packaging-web-application-projects.md)
> [dalej](deploying-web-packages.md)

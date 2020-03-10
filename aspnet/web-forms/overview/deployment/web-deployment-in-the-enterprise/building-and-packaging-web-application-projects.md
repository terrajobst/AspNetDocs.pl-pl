---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/building-and-packaging-web-application-projects
title: Kompilowanie i pakowanie projektów aplikacji sieci Web | Microsoft Docs
author: jrjlee
description: Jeśli chcesz wdrożyć projekt aplikacji sieci Web w środowisku serwera zdalnego, pierwsze zadanie polega na skompilowaniu projektu i wygenerowaniu pakietu wdrożeniowego sieci Web...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 94e92f80-a7e3-4d18-9375-ff8be5d666ac
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/building-and-packaging-web-application-projects
msc.type: authoredcontent
ms.openlocfilehash: 1d0ee0264ce6461d7b0159f1a44de4de31e2d079
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78573921"
---
# <a name="building-and-packaging-web-application-projects"></a>Kompilowanie i tworzenie pakietów projektów aplikacji internetowych

Autor [Jason Lewandowski](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Jeśli chcesz wdrożyć projekt aplikacji sieci Web w środowisku serwera zdalnego, pierwsze zadanie polega na skompilowaniu projektu i wygenerowaniu pakietu wdrożeniowego sieci Web. W tym temacie opisano, w jaki sposób proces kompilacji działa w przypadku projektów aplikacji sieci Web. W szczególności wyjaśnia:
> 
> - Jak potok publikowania w sieci Web (WPP) rozszerza proces kompilacji w celu uwzględnienia funkcji wdrożenia.
> - Jak narzędzie Web Deployment Internet Information Services (IIS) (Web Deploy) przekształca aplikację sieci Web w pakiet wdrożeniowy.
> - Jak działa kompilacja i proces pakowania oraz jakie pliki są tworzone.

W programie Visual Studio 2010 proces kompilowania i wdrażania projektów aplikacji sieci Web jest obsługiwany przez WPP. WPP zawiera zestaw elementów docelowych programu Microsoft Build Engine (MSBuild), które zwiększają funkcjonalność programu MSBuild i umożliwiają integrację z Web Deploy. W programie Visual Studio można zobaczyć tę rozszerzoną funkcję na stronach właściwości projektu aplikacji sieci Web. Strona **sieci Web pakowanie/Publish** wraz z **pakietem/publikowaniem SQL** umożliwia skonfigurowanie sposobu pakowania projektu aplikacji sieci Web na potrzeby wdrożenia po zakończeniu procesu kompilacji.

![](building-and-packaging-web-application-projects/_static/image1.png)

## <a name="how-does-the-wpp-work"></a>Jak działa WPP?

Jeśli przyjrzyjsz się plikowi projektu dla C#projektu aplikacji sieci Web, możesz zobaczyć, że importuje dwa pliki. targets.

[!code-xml[Main](building-and-packaging-web-application-projects/samples/sample1.xml)]

Pierwsza instrukcja **importu** jest wspólna dla wszystkich projektów Visual C# . Ten plik, *Microsoft. CSharp. targets*, zawiera elementy docelowe i zadania, które są C#specyficzne dla wizualizacji. Na przykład zadanie C# kompilatora (**CSC**) jest wywoływane w tym miejscu. Plik *Microsoft. CSharp. targets* z kolei importuje plik *Microsoft. Common. targets* . Definiuje elementy docelowe, które są wspólne dla wszystkich projektów, takich jak **Kompilowanie**, ponowne **Kompilowanie**, **Uruchamianie**, **Kompilowanie**i **czyszczenie**. Druga instrukcja **importowania** jest specyficzna dla projektów aplikacji sieci Web. Plik *Microsoft. WebApplication. targets* w programie importuje plik *Microsoft. Web. Publishing. targets* . Plik *Microsoft. Web. Publishing. targets* zasadniczo *jest* WPP. Definiuje ona elementy docelowe, takie jak **Package** i **MSDeployPublish**, które wywołują Web Deploy do wykonywania różnych zadań wdrażania.

Aby zrozumieć, jak są używane dodatkowe cele, w przykładowym rozwiązaniu Contact Manager Otwórz plik *Publish. proj* i spójrz na obiekt docelowy **BuildProjects** .

[!code-xml[Main](building-and-packaging-web-application-projects/samples/sample2.xml)]

Ten obiekt docelowy używa zadania **MSBuild** do tworzenia różnych projektów. Zwróć uwagę na właściwości **DeployOnBuild** i **DeployTarget** :

- Właściwość **DeployOnBuild = true** zasadniczo oznacza "Chcę wykonać dodatkowy cel po pomyślnym zakończeniu kompilacji".
- Właściwość **DeployTarget** identyfikuje nazwę obiektu docelowego, który ma zostać wykonany, gdy właściwość **DeployOnBuild** ma **wartość true**. W takim przypadku należy określić, że program MSBuild ma wykonać element docelowy **pakietu** po skompilowaniu projektu.

Element docelowy **pakietu** jest zdefiniowany w pliku *Microsoft. Web. Publishing. targets* . Zasadniczo ten obiekt docelowy pobiera dane wyjściowe kompilacji projektu aplikacji sieci Web i włącza go do pakietu wdrożeniowego sieci Web, który można opublikować na serwerze sieci Web usług IIS.

> [!NOTE]
> Aby wyświetlić plik projektu (na przykład <em>ContactManager. MVC. csproj</em>) w programie Visual Studio 2010, należy najpierw zwolnić projekt z rozwiązania. W oknie <strong>Eksplorator rozwiązań</strong> kliknij prawym przyciskiem myszy węzeł projektu, a następnie kliknij polecenie <strong>Zwolnij projekt</strong>. Ponownie kliknij prawym przyciskiem myszy węzeł projektu, a następnie kliknij polecenie <strong>Edytuj</strong><em>[plik projektu]</em>). Plik projektu zostanie otwarty w jego pierwotnym formacie XML. Pamiętaj, aby ponownie załadować projekt po zakończeniu.  
> Aby uzyskać więcej informacji na temat obiektów docelowych programu MSBuild, zadań i instrukcji <strong>importu</strong> , zobacz [Omówienie pliku projektu](understanding-the-project-file.md). Aby uzyskać bardziej szczegółowe wprowadzenie do plików projektu i WPP, zobacz [wewnątrz Microsoft Build Engine: korzystanie z programu MSBuild i Team Foundation Build](http://amzn.com/0735645248) przez Sayed Ibrahim Hashimi i William BARTHOLOMEW, ISBN: 978-0-7356-4524-0.

## <a name="what-is-a-web-deployment-package"></a>Co to jest pakiet wdrażania w sieci Web?

Podczas kompilowania i wdrażania projektu aplikacji sieci Web przy użyciu programu Visual Studio 2010 lub bezpośrednio z poziomu programu MSBuild wynik końcowy jest zwykle *pakietem wdrożeniowym sieci Web*. Pakiet wdrożeniowy sieci Web to plik. zip. Zawiera wszystkie informacje o tym, że usługi IIS i Web Deploy muszą być potrzebne do ponownego utworzenia aplikacji sieci Web, w tym:

- Skompilowane dane wyjściowe aplikacji sieci Web, w tym zawartość, pliki zasobów, pliki konfiguracji, zasoby JavaScript i kaskadowe arkusze stylów (CSS) itd.
- Zestawy dla projektu aplikacji sieci Web i dla wszystkich projektów, do których istnieją odwołania w rozwiązaniu.
- Skrypty SQL do generowania wszystkich baz danych, które są wdrażane za pomocą aplikacji sieci Web.

Po wygenerowaniu pakietu wdrożeniowego sieci Web można go opublikować na serwerze sieci Web usług IIS na różne sposoby. Można na przykład wdrożyć ją zdalnie, przeznaczoną dla usługi zdalnego agenta Web Deploy lub obsługi Web Deploy na docelowym serwerze sieci Web. można też użyć Menedżera usług IIS do ręcznego zaimportowania pakietu na docelowym serwerze sieci Web. Aby uzyskać więcej informacji na temat tych podejścia do wdrożenia, zobacz [Wybieranie odpowiedniego podejścia do wdrażania w sieci Web](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md).

## <a name="how-does-the-build-process-work"></a>Jak działa proces kompilacji?

Pokazuje to, co się stanie w przypadku kompilowania i pakowania projektu aplikacji sieci Web:

![](building-and-packaging-web-application-projects/_static/image2.png)

Podczas kompilowania projektu aplikacji sieci Web proces kompilacji generuje plik o nazwie *[nazwa projektu]. SourceManifest. XML*. Wraz z plikiem projektu i danymi wyjściowymi kompilacji *. Plik SourceManifest. XML* zawiera Web Deploy informacje o tym, co jest potrzebne do dołączenia do pakietu wdrożeniowego sieci Web. Korzystając z tych danych wejściowych, Web Deploy generuje pakiet wdrożeniowy sieci Web o nazwie *[nazwa projektu]. zip*.

Wraz z pakietem Web Deployment proces kompilacji generuje dwa pliki, które mogą ułatwić korzystanie z pakietu:

- Plik *. deploy. cmd* zawiera zestaw sparametryzowanych poleceń Web Deploy (MSDeploy. exe), które publikują pakiet wdrożeniowy sieci Web na zdalnym serwerze sieci Web usług IIS. Uruchomienie pliku *. deploy. cmd* z odpowiednimi parametrami zwykle zapewnia szybszy i łatwiejszy alternatywę do ręcznego konstruowania poleceń MSDeploy. exe.
- Plik *SetParameters. XML* zawiera zestaw wartości parametrów dla polecenia MSDeploy. exe. Te wartości obejmują właściwości, takie jak nazwa aplikacji sieci Web usług IIS, do której ma zostać wdrożony pakiet, wartości wszystkich punktów końcowych usługi i parametrów połączenia zdefiniowane w pliku *Web. config* oraz wszystkie wartości właściwości wdrożenia zdefiniowane na stronach właściwości projektu.

Plik *SetParameters. XML* jest kluczem do zarządzania procesem wdrażania. Ten plik jest generowany dynamicznie w oparciu o zawartość projektu aplikacji sieci Web. Na przykład, jeśli dodasz parametry połączenia do pliku *Web. config* , proces kompilacji automatycznie wykryje parametry połączenia, Sparametryzuj wdrożenie odpowiednio i utworzysz wpis w pliku *SetParameters. XML* , aby umożliwić modyfikowanie parametrów połączenia w ramach procesu wdrażania. W następnym temacie, [Konfigurowanie parametrów wdrożenia pakietu sieci Web](configuring-parameters-for-web-package-deployment.md), wyjaśniono rolę tego pliku bardziej szczegółowo i opisano różne sposoby ich modyfikacji podczas kompilowania i wdrażania.

> [!NOTE]
> W programie Visual Studio 2010 WPP nie obsługuje wstępnego kompilowania stron w aplikacji sieci Web przed opakowaniem. Następna wersja programu Visual Studio i WPP będzie zawierać możliwość wstępnego kompilowania aplikacji sieci Web jako opcji pakowania.

## <a name="conclusion"></a>Podsumowanie

Ten temat zawiera omówienie procesu kompilowania i tworzenia pakietów dla projektów aplikacji sieci Web w programie Visual Studio 2010. Opisano w nim, jak WPP umożliwia wywoływanie poleceń Web Deploy z programu MSBuild i wyjaśniono, jak działa proces kompilowania i tworzenia pakietów.

Po utworzeniu pakietu wdrożeniowego sieci Web następnym krokiem jest jego wdrożenie. Aby uzyskać więcej informacji na ten temat, zobacz [Konfigurowanie parametrów wdrażania pakietów sieci Web](configuring-parameters-for-web-package-deployment.md) i [wdrażanie pakietów internetowych](deploying-web-packages.md).

## <a name="further-reading"></a>Dalsze informacje

W następnych tematach w tym samouczku, [konfigurowaniu parametrów wdrażania pakietów sieci Web](configuring-parameters-for-web-package-deployment.md) i [wdrażania pakietów sieci Web](deploying-web-packages.md), należy zapoznać się ze wskazówkami dotyczącymi sposobu korzystania z utworzonego pakietu sieci Web. Ostatni samouczek z tej serii, [zaawansowane wdrażanie w sieci Web dla przedsiębiorstw](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md), zawiera wskazówki dotyczące dostosowywania i rozwiązywania problemów z procesem tworzenia pakietów.

Aby uzyskać bardziej szczegółowe wprowadzenie do plików projektu i WPP, zobacz [wewnątrz Microsoft Build Engine: korzystanie z programu MSBuild i Team Foundation Build](http://amzn.com/0735645248) przez Sayed Ibrahim Hashimi i William BARTHOLOMEW, ISBN: 978-0-7356-4524-0.

> [!div class="step-by-step"]
> [Poprzednie](understanding-the-build-process.md)
> [dalej](configuring-parameters-for-web-package-deployment.md)

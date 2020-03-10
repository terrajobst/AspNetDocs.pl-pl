---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-web-packages
title: Wdrażanie pakietów sieci Web | Microsoft Docs
author: jrjlee
description: W tym temacie opisano sposób publikowania pakietów wdrażania sieci Web na serwerze zdalnym przy użyciu narzędzia Web Deployment Internet Information Services (IIS).
ms.author: riande
ms.date: 05/04/2012
ms.assetid: a5c5eed2-8683-40a5-a2e1-35c9f8d17c29
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-web-packages
msc.type: authoredcontent
ms.openlocfilehash: 91b99e6e250342851aea6860164b6f6af54818d1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78575951"
---
# <a name="deploying-web-packages"></a>Wdrażanie pakietów internetowych

Autor [Jason Lewandowski](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie opisano sposób publikowania pakietów wdrażania sieci Web na serwerze zdalnym przy użyciu narzędzia Web Deployment Internet Information Services (IIS) (Web Deploy) 2,0.
> 
> Istnieją dwa główne sposoby wdrażania pakietu sieci Web na serwerze zdalnym:
> 
> - Bezpośrednio można użyć narzędzia wiersza polecenia MSDeploy. exe.
> - Można uruchomić plik *[nazwa projektu]. deploy. cmd* generowany przez proces kompilacji.
> 
> Wynik końcowy jest taki sam, niezależnie od tego, jakiego podejścia używasz. Zasadniczo plik *. deploy. cmd* ma na celu uruchomienie programu MSDeploy. exe z pewnymi wstępnie ustalonymi wartościami, dzięki czemu nie trzeba podawać tak dużej ilości informacji, aby wdrożyć pakiet. Upraszcza to proces wdrażania. Z drugiej strony, korzystanie z programu MSDeploy. exe bezpośrednio zapewnia znacznie większą elastyczność w porównaniu do wdrożonego pakietu.
> 
> Używane podejście będzie zależeć od różnych czynników, w tym od zakresu kontroli wymaganej przez proces wdrażania oraz tego, czy jest przeznaczony dla Web Deploy zdalnej usługi Agent czy Web Deploy obsługi. W tym temacie wyjaśniono, jak używać każdego podejścia i określić, kiedy każde podejście jest odpowiednie.
> 
> W zadaniach i przewodnikach w tym temacie przyjęto założenie, że:
> 
> - Aplikacja sieci Web została skompilowana i spakowana zgodnie z opisem w temacie [Tworzenie i pakowanie projektów aplikacji sieci Web](building-and-packaging-web-application-projects.md).
> - Zmodyfikowano plik *SetParameters. XML* w celu zapewnienia odpowiednich wartości parametrów dla środowiska docelowego, zgodnie z opisem w temacie [Konfigurowanie parametrów dla wdrożenia pakietu internetowego](configuring-parameters-for-web-package-deployment.md).

Uruchomienie pliku [*Nazwa projektu*] *. deploy. cmd* jest najprostszym sposobem wdrożenia pakietu internetowego. W szczególności użycie pliku *Deploy. cmd* oferuje następujące korzyści związane z korzystaniem z programu MSDeploy. exe bezpośrednio:

- Nie musisz określać lokalizacji pakietu&#x2014;wdrożeniowego sieci Web plik *. deploy. cmd* już wie, gdzie jest.
- Nie musisz określać lokalizacji pliku&#x2014; *SetParameters. XML* plik *. deploy. cmd* już wie, gdzie jest.
- Nie musisz określać źródłowych i docelowych dostawców&#x2014;MSDeploy, plik *. deploy. cmd* ma już znane wartości do użycia.
- Nie musisz określać ustawień&#x2014;operacji MSDeploy plik *. deploy. cmd* dodaje często wymagane wartości do polecenia MSDeploy. exe automatycznie.

Przed użyciem pliku *Deploy. cmd* do wdrożenia pakietu sieci Web należy upewnić się, że:

- Plik *. deploy. cmd* , [*Nazwa projektu*]. *SetParameters. XML* i pakiet sieci Web ([*Nazwa projektu*]. *plik zip*) znajduje się w tym samym folderze.
- Web Deploy (MSDeploy. exe) jest zainstalowany na komputerze, na którym jest uruchamiany plik *. deploy. cmd* .

Plik *. deploy. cmd* obsługuje różne opcje wiersza polecenia. Po uruchomieniu pliku z wiersza polecenia jest to podstawowa składnia:

[!code-console[Main](deploying-web-packages/samples/sample1.cmd)]

Należy określić flagę **/t** lub **/y** , aby wskazać, czy ma zostać wykonane uruchomienie w wersji próbnej, czy na żywo odpowiednio (nie należy używać obu flag w tym samym poleceniu). W tej tabeli wyjaśniono przeznaczenie poszczególnych flag.

| Flaga | Opis |
| --- | --- |
| **/T** | Wywołuje program MSDeploy. exe z flagą **– whatIf** , która wskazuje na uruchomienie wersji próbnej. Zamiast wdrażać pakiet, tworzy raport o tym, co się stanie, jeśli został wdrożony pakiet. |
| **/Y** | Wywołuje MSDeploy. exe bez flagi **– whatIf** . Powoduje to wdrożenie pakietu na komputerze lokalnym lub na określonym serwerze docelowym. |
| **Parametr** | Określa nazwę serwera docelowego lub adres URL usługi. Aby uzyskać więcej informacji na temat wartości, które można podać w tym miejscu, zobacz sekcję **uwagi dotyczące punktu końcowego** w tym temacie. W przypadku pominięcia flagi **/m** pakiet zostanie wdrożony na komputerze lokalnym. |
| **/A** | Określa typ uwierzytelniania, który powinien zostać użyty w programie MSDeploy. exe do wykonania wdrożenia. Możliwe wartości to **NTLM** i **Basic**. W przypadku pominięcia flagi **/a** typ uwierzytelniania jest domyślnie używany **w przypadku wdrożenia w ramach usługi** zdalnego agenta Web Deploy **i do wdrożenia w ramach programu** obsługi Web Deploy. |
| **Określ** | Określa nazwę użytkownika. Ma to zastosowanie tylko w przypadku korzystania z uwierzytelniania podstawowego. |
| **/P** | Określa hasło. Ma to zastosowanie tylko w przypadku korzystania z uwierzytelniania podstawowego. |
| **Przełącznika** | Wskazuje, że pakiet należy wdrożyć w lokalnym wystąpieniu IIS Express. |
| **/G** | Określa, że pakiet jest wdrażany przy użyciu [Ustawienia dostawcy tempAgent](https://technet.microsoft.com/library/ee517345(WS.10).aspx). Jeśli pominięto flagę **/g** , wartość domyślna to **false**. |

> [!NOTE]
> Za każdym razem, gdy proces kompilacji tworzy pakiet sieci Web, tworzy również plik o nazwie *[nazwa projektu]. deploy-Readme. txt* objaśniający te opcje wdrażania.

Oprócz tych flag można określić Web Deploy ustawienia operacji jako dodatkowe parametry *. deploy. cmd* . Wszelkie dodatkowe ustawienia, które określisz, są po prostu przenoszone do bazowego polecenia MSDeploy. exe. Aby uzyskać więcej informacji na temat tych ustawień, zobacz [Web Deploy ustawienia operacji](https://technet.microsoft.com/library/dd569089(WS.10).aspx).

Załóżmy, że chcesz wdrożyć projekt aplikacji sieci Web ContactManager. MVC w środowisku testowym, uruchamiając plik *. deploy. cmd* . Środowisko testowe jest skonfigurowane do korzystania z usługi zdalnego agenta Web Deploy, zgodnie z opisem w temacie [Konfigurowanie serwera sieci Web do Web Deploy Publishing (Agent zdalny)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md). Aby wdrożyć aplikację sieci Web, należy wykonać poniższe czynności.

**Aby wdrożyć aplikację sieci Web przy użyciu pliku. deploy. cmd**

1. Kompiluj i Pakuj projekt aplikacji sieci Web, zgodnie z opisem w temacie [Tworzenie i pakowanie projektów aplikacji sieci Web](building-and-packaging-web-application-projects.md).
2. Zmodyfikuj plik *ContactManager. MVC. SetParameters. XML* , aby zawierał poprawne wartości parametrów dla środowiska testowego, zgodnie z opisem w temacie [Configuring Parameters for Web Package Deployment](configuring-parameters-for-web-package-deployment.md).
3. Otwórz okno wiersza polecenia i przejdź do lokalizacji pliku *ContactManager. MVC. deploy. cmd* .
4. Wpisz następujące polecenie, a następnie naciśnij klawisz ENTER:

    [!code-console[Main](deploying-web-packages/samples/sample2.cmd)]

W tym przykładzie:

- Flaga **/y** wskazuje, że chcesz faktycznie wdrożyć pakiet, zamiast wykonywać próbne uruchomienie.
- Flaga **/m** wskazuje, że chcesz wdrożyć pakiet na serwerze o nazwie TESTWEB1. Z tej wartości program MSDeploy. exe podejmie próbę wdrożenia pakietu w usłudze zdalnego agenta Web Deploy w http://TESTWEB1/MSDeployAgentService.
- Flaga **/a** wskazuje, że chcesz użyć uwierzytelniania NTLM. W związku z tym nie trzeba podawać nazwy użytkownika i hasła.

Aby zilustrować, jak za pomocą pliku *. deploy. cmd* uprościć proces wdrażania, należy zapoznać się z poleceniem MSDeploy. exe, które zostanie wygenerowane i wykonane podczas uruchamiania programu *ContactManager. MVC. deploy. cmd* przy użyciu opcji przedstawionych powyżej.

[!code-console[Main](deploying-web-packages/samples/sample3.cmd)]

Aby uzyskać więcej informacji na temat wdrażania pakietu sieci Web przy użyciu pliku *. deploy. cmd* , zobacz [How to: Install a Deployment Package using the Deploy. cmd plik](https://msdn.microsoft.com/library/ff356104.aspx).

## <a name="using-msdeployexe"></a>Korzystanie z programu MSDeploy. exe

Chociaż korzystanie z pliku *. deploy. cmd* zwykle upraszcza proces wdrażania, w niektórych sytuacjach zaleca się bezpośrednie użycie programu MSDeploy. exe. Na przykład:

- Jeśli chcesz wdrożyć program w programie obsługi Web Deploy jako użytkownik niebędący administratorem, nie możesz użyć pliku *Deploy. cmd* . Jest to spowodowane usterką w Web Deploy 2,0, zgodnie z opisem w sekcji **uwagi dotyczące punktu końcowego**.
- Jeśli chcesz ręcznie przełączać między różnymi plikami *SetParameters. XML* w różnych lokalizacjach, możesz preferować bezpośrednio użyć programu MSDeploy. exe.
- Jeśli chcesz przesłonić kilka argumentów wiersza polecenia MSDeploy. exe, możesz użyć bezpośredniego pliku MSDeploy. exe.

W przypadku korzystania z programu MSDeploy. exe należy podać trzy kluczowe fragmenty informacji:

- Parametr **– Source** wskazujący miejsce, z którego pochodzą dane.
- A **– parametr docelowy** wskazujący miejsce, w którym dane są przeszukiwane.
- A **– parametr zlecenia** , który wskazuje [operację](https://technet.microsoft.com/library/dd568989(WS.10).aspx) , którą chcesz wykonać.

W programie MSDeploy. exe są oparte [Web Deploy dostawcy, którzy](https://technet.microsoft.com/library/dd569040(WS.10).aspx) przetwarzają dane źródłowe i docelowe. Web Deploy obejmuje wielu dostawców, którzy reprezentują zakres aplikacji i źródeł danych, z&#x2014;którymi może korzystać na przykład, istnieją dostawcy dla SQL Server baz danych, serwery sieci Web usług IIS, certyfikaty, zestawy globalnej pamięci podręcznej zestawów (GAC), różne różne pliki konfiguracji i wiele innych typów danych. Zarówno parametr **– Source** , jak i parametr **– docelowy** muszą określać dostawcę w postaci **— Źródło**: [*ProviderName*] = [*Lokalizacja*]. Podczas wdrażania pakietu sieci Web w witrynie sieci Web usług IIS należy użyć następujących wartości:

- Dostawca **– Źródło** jest zawsze [pakietem](https://technet.microsoft.com/library/dd569019(WS.10).aspx). Na przykład:

    [!code-console[Main](deploying-web-packages/samples/sample4.cmd)]
- Dostawca **,** do którego jest on zawsze [wybierany](https://technet.microsoft.com/library/dd569016(WS.10).aspx). Na przykład:

    [!code-console[Main](deploying-web-packages/samples/sample5.cmd)]
- **Zlecenie –** jest zawsze **synchronizowane**.

    [!code-console[Main](deploying-web-packages/samples/sample6.cmd)]

Ponadto należy określić różne inne [Ustawienia specyficzne dla dostawcy](https://technet.microsoft.com/library/dd569001(WS.10).aspx) i ogólne [Ustawienia operacji](https://technet.microsoft.com/library/dd569089(WS.10).aspx). Załóżmy na przykład, że chcesz wdrożyć aplikację sieci Web ContactManager. MVC w środowisku przejściowym. Wdrożenie będzie ukierunkowane na procedurę obsługi Web Deploy i musi korzystać z uwierzytelniania podstawowego. Aby wdrożyć aplikację sieci Web, należy wykonać poniższe czynności.

**Aby wdrożyć aplikację sieci Web przy użyciu programu MSDeploy. exe**

1. Kompiluj i Pakuj projekt aplikacji sieci Web, zgodnie z opisem w temacie [Tworzenie i pakowanie projektów aplikacji sieci Web](building-and-packaging-web-application-projects.md).
2. Zmodyfikuj plik *ContactManager. MVC. SetParameters. XML* , aby zawierał poprawne wartości parametrów dla środowiska tymczasowego, zgodnie z opisem w temacie [Konfigurowanie parametrów wdrożenia pakietu internetowego](configuring-parameters-for-web-package-deployment.md).
3. Otwórz okno wiersza polecenia i przejdź do lokalizacji programu MSDeploy. exe. Zwykle jest to%PROGRAMFILES%\IIS\Microsoft Web Deploy V2\msdeploy.exe.
4. Wpisz to polecenie, a następnie naciśnij klawisz ENTER (zignoruj znaki podziału wiersza):

    [!code-console[Main](deploying-web-packages/samples/sample7.cmd)]

W tym przykładzie:

- Parametr **– Source** określa dostawcę **pakietu** i wskazuje lokalizację pakietu sieci Web.
- Parametr **– docelowy** określa dostawcę **autostartu** . Ustawienie **computerName** zawiera adres URL usługi programu obsługi Web Deploy na serwerze docelowym. Ustawienie **AuthType** wskazuje, że chcesz używać uwierzytelniania podstawowego, a w związku z tym należy podać **nazwę użytkownika** i **hasło**. Na koniec ustawienie **includeAcls = "false"** wskazuje, że nie chcesz kopiować list kontroli dostępu (ACL) plików w źródłowej aplikacji sieci Web na serwer docelowy.
- Argument **– Verb: Sync** wskazuje, że chcesz replikować zawartość źródłową na serwerze docelowym.
- Argumenty **– disableLink** wskazują, że nie chcesz replikować pul aplikacji, konfiguracji katalogu wirtualnego lub certyfikatów SSL (SSL) na serwerze docelowym. Aby uzyskać więcej informacji, zobacz [Web Deploy rozszerzeń linków](https://technet.microsoft.com/library/dd569028(WS.10).aspx).
- Parametr **– setParamFile** zawiera lokalizację pliku *SetParameters. XML* .
- Przełącznik **– allowUntrusted** wskazuje, że Web Deploy powinna akceptować certyfikaty SSL, które nie zostały wystawione przez zaufany urząd certyfikacji. Jeśli wdrażasz program do obsługi Web Deploy i certyfikat z podpisem własnym został użyty do zabezpieczenia adresu URL usługi, należy dołączyć ten przełącznik.

## <a name="automating-web-package-deployment"></a>Automatyzowanie wdrożenia pakietu sieci Web

W wielu scenariuszach dla przedsiębiorstw warto wdrożyć pakiety sieci Web jako część większego lub zautomatyzowanego wdrożenia. Bez względu na to, czy chcesz wdrożyć pakiety sieci Web, uruchamiając plik *. deploy. cmd* lub bezpośrednio używając programu MSDeploy. exe, możesz Sparametryzuj polecenia i wywoływać je z obiektu docelowego w pliku projektu Microsoft Build Engine (MSBuild).

W przykładowym rozwiązaniu Contact Manager zapoznaj się z elementem docelowym **PublishWebPackages** w pliku *Publish. proj* . Ten element docelowy jest uruchamiany jednokrotnie dla każdego pliku *. deploy. cmd* identyfikowanego przez listę elementów o nazwie **PublishPackages**. Obiekt docelowy korzysta z metadanych właściwości i elementów w celu utworzenia pełnego zestawu wartości argumentów dla każdego pliku *. deploy. cmd* , a następnie używa zadania **exec** do uruchomienia polecenia.

[!code-xml[Main](deploying-web-packages/samples/sample8.xml)]

> [!NOTE]
> Aby uzyskać szerszy przegląd modelu plików projektu w przykładowym rozwiązaniu, a ponadto wprowadzenie do niestandardowych plików projektu, zobacz [Opis pliku projektu](understanding-the-project-file.md) i [Opis procesu kompilacji](understanding-the-build-process.md).

## <a name="endpoint-considerations"></a>Uwagi dotyczące punktu końcowego

Niezależnie od tego, czy pakiet sieci Web jest wdrażany przez uruchomienie pliku *. deploy. cmd* lub bezpośrednio przy użyciu programu MSDeploy. exe, należy określić nazwę komputera lub punkt końcowy usługi dla wdrożenia.

Jeśli docelowy serwer sieci Web jest skonfigurowany do wdrożenia przy użyciu usługi zdalnego agenta Web Deploy, określ adres URL usługi docelowej jako miejsce docelowe.

[!code-console[Main](deploying-web-packages/samples/sample9.cmd)]

Alternatywnie można określić nazwę serwera jako lokalizację docelową, a Web Deploy będzie wnioskować o adres URL usługi zdalnego agenta.

[!code-console[Main](deploying-web-packages/samples/sample10.cmd)]

Jeśli docelowy serwer sieci Web jest skonfigurowany do wdrożenia przy użyciu programu obsługi Web Deploy, należy określić adres punktu końcowego usługi zarządzania siecią Web (WMSvc) usług IIS jako lokalizację docelową. Domyślnie ma to formę:

[!code-console[Main](deploying-web-packages/samples/sample11.cmd)]

Możesz użyć dowolnego z tych punktów końcowych bezpośrednio przy użyciu pliku *. deploy. cmd* lub MSDeploy. exe. Jeśli jednak chcesz wdrożyć program w programie obsługi Web Deploy jako użytkownik niebędący administratorem, zgodnie z opisem w temacie [Konfigurowanie serwera sieci Web do Web Deploy publikowania (procedura obsługi Web Deploy)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md), należy dodać ciąg zapytania do adresu punktu końcowego usługi.

[!code-console[Main](deploying-web-packages/samples/sample12.cmd)]

Wynika to z faktu, że użytkownik niebędący administratorem nie ma dostępu na poziomie serwera do usług IIS. tylko ma dostęp do określonej witryny sieci Web usług IIS. W momencie zapisu z powodu błędu w potoku publikowania w sieci Web (WPP) nie można uruchomić pliku *Deploy. cmd* przy użyciu adresu punktu końcowego, który zawiera ciąg zapytania. W tym scenariuszu należy bezpośrednio wdrożyć pakiet sieci Web za pomocą programu MSDeploy. exe.

> [!NOTE]
> Aby uzyskać więcej informacji na temat usługi zdalnego agenta Web Deploy i programu obsługi Web Deploy, zobacz [Wybieranie odpowiedniego podejścia do wdrażania w sieci Web](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md). Aby uzyskać wskazówki dotyczące sposobu konfigurowania plików projektu specyficznych dla środowiska w celu wdrożenia ich w tych punktach końcowych, zobacz [Konfigurowanie właściwości wdrożenia dla środowiska docelowego](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).

## <a name="authentication-considerations"></a>Zagadnienia dotyczące uwierzytelniania

Niezależnie od tego, czy pakiet sieci Web jest wdrażany przez uruchomienie pliku *. deploy. cmd* lub bezpośrednio przy użyciu programu MSDeploy. exe, należy określić typ uwierzytelniania. Web Deploy akceptuje dwie możliwe wartości: **NTLM** lub **podstawowa**. W przypadku określenia uwierzytelniania podstawowego należy również podać nazwę użytkownika i hasło. W przypadku wybrania typu uwierzytelniania należy pamiętać o różnych czynnikach:

- W przypadku wdrażania w usłudze zdalnego agenta Web Deploy należy użyć uwierzytelniania NTLM. Usługa agenta zdalnego nie akceptuje poświadczeń uwierzytelniania podstawowego.
- Jeśli wdrażasz program do obsługi Web Deploy, możesz użyć uwierzytelniania NTLM lub uwierzytelnianie podstawowe. Ustawieniem domyślnym jest uwierzytelnianie podstawowe. Chociaż uwierzytelnianie podstawowe opiera się na nazwach użytkowników i hasłach przesyłanych w postaci zwykłego tekstu, poświadczenia są chronione, ponieważ program obsługi Web Deploy zawsze używa szyfrowania SSL.
- Jeśli pakiet sieci Web zawiera bazę danych, a serwer sieci Web i serwer baz danych są oddzielnymi maszynami, nie będzie można wdrożyć bazy danych przy użyciu uwierzytelniania NTLM ze względu na [ograniczenie NTLM "podwójnego przeskoku"](https://go.microsoft.com/?linkid=9805120). Musisz użyć poświadczeń SQL Server w parametrach połączenia wdrożenia lub podać poświadczenia uwierzytelniania podstawowego, aby Web Deploy. Ten problem został opisany bardziej szczegółowo w temacie [wdrażanie baz danych członkostwa w środowiskach korporacyjnych](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md).

## <a name="conclusion"></a>Podsumowanie

W tym temacie opisano, jak można wdrożyć pakiet sieci Web, uruchamiając plik *. deploy. cmd* lub bezpośrednio używając programu MSDeploy. exe. Wyjaśniono, kiedy każde podejście może być odpowiednie, i opisano, jak można Sparametryzuj i uruchomić polecenie wdrożenia jako część większego procesu kompilacji pojedynczego lub zautomatyzowanego.

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać wskazówki dotyczące sposobu tworzenia i Sparametryzuj pakietu wdrożeniowego sieci Web, zobacz [Kompilowanie i pakowanie projektów aplikacji sieci Web](building-and-packaging-web-application-projects.md) oraz [Konfigurowanie parametrów wdrażania pakietów sieci Web](configuring-parameters-for-web-package-deployment.md). Aby uzyskać wskazówki dotyczące sposobu kompilowania i wdrażania pakietów internetowych z wystąpienia Team Foundation Server (TFS), zobacz [konfigurowanie Team Foundation Server zautomatyzowanego wdrażania w Internecie](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Informacje o sposobach dostosowywania i rozwiązywania problemów z procesem wdrażania znajdują się w sekcji [wykluczanie plików i folderów z wdrożenia](../advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment.md).

> [!div class="step-by-step"]
> [Poprzednie](configuring-parameters-for-web-package-deployment.md)
> [dalej](deploying-database-projects.md)

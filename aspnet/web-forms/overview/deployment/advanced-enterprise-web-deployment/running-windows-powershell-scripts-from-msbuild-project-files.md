---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
title: Uruchamianie skryptów programu Windows PowerShell z plików projektu MSBuild | Microsoft Docs
author: jrjlee
description: W tym temacie opisano sposób uruchamiania skryptu programu Windows PowerShell w ramach procesu kompilowania i wdrażania. Skrypt można uruchomić lokalnie (innymi słowy, na b...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 55f1ae45-fcb5-43a9-8415-fa5b935fc9c9
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
msc.type: authoredcontent
ms.openlocfilehash: 7b09c07b8b7c2a61ca534f7a66a929593f3d04ca
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78548511"
---
# <a name="running-windows-powershell-scripts-from-msbuild-project-files"></a>Uruchamianie skryptów programu Windows PowerShell z poziomu plików projektów programu MSBuild

Autor [Jason Lewandowski](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie opisano sposób uruchamiania skryptu programu Windows PowerShell w ramach procesu kompilowania i wdrażania. Skrypt można uruchamiać lokalnie (inaczej mówiąc, na serwerze kompilacji) lub zdalnie, podobnie jak na docelowym serwerze sieci Web lub serwerze bazy danych.
> 
> Istnieje wiele powodów, dla których warto uruchomić skrypt programu Windows PowerShell po wdrożeniu. Na przykład możesz chcieć:
> 
> - Dodaj niestandardowe źródło zdarzeń do rejestru.
> - Wygeneruj katalog systemu plików na potrzeby przekazywania.
> - Oczyść katalogi kompilacji.
> - Zapisz wpisy w pliku dziennika niestandardowego.
> - Wysyłaj wiadomości e-mail z zaproszeniem użytkowników do nowo zainicjowanej aplikacji sieci Web.
> - Utwórz konta użytkowników z odpowiednimi uprawnieniami.
> - Skonfiguruj replikację między wystąpieniami SQL Server.
> 
> W tym temacie przedstawiono sposób lokalnego i zdalnego uruchamiania skryptów programu Windows PowerShell z niestandardowego obiektu docelowego w pliku projektu Microsoft Build Engine (MSBuild).

Ten temat stanowi część szeregu samouczków opartych na wymaganiach dotyczących wdrażania w przedsiębiorstwie fikcyjnej firmy o nazwie Fabrikam, Inc. W tej serii samouczków jest stosowane&#x2014;przykładowe rozwiązanie&#x2014;do reprezentowania aplikacji sieci Web, [które ma](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)realistyczny poziom złożoności, w tym aplikacji ASP.NET MVC 3, usługi Windows Communication Foundation (WCF) i projektu bazy danych.

Metoda wdrażania w tym samouczku jest oparta na rozłożeniu pliku projektu dzielonego opisanym w artykule [Omówienie pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), w którym proces kompilacji jest kontrolowany przez dwa pliki&#x2014;projektu, zawierający instrukcje kompilacji, które mają zastosowanie do każdego środowiska docelowego, oraz jeden zawierający ustawienia kompilacji i wdrożenia specyficznego dla środowiska. W czasie kompilacji plik projektu specyficzny dla środowiska jest scalany z plikiem projektu Environment-niezależny od w celu utworzenia kompletnego zestawu instrukcji kompilacji.

## <a name="task-overview"></a>Przegląd zadania

Aby uruchomić skrypt programu Windows PowerShell w ramach procesu wdrażania automatycznego lub pojedynczego kroku, należy wykonać te zadania wysokiego poziomu:

- Dodaj skrypt programu Windows PowerShell do rozwiązania i kontroli źródła.
- Utwórz polecenie, które wywołuje skrypt programu Windows PowerShell.
- Wykorzystaj wszystkie zastrzeżone znaki XML w poleceniu.
- Utwórz obiekt docelowy w pliku niestandardowego projektu programu MSBuild i Użyj zadania **exec** , aby uruchomić polecenie.

W tym temacie pokazano, jak wykonać te procedury. Zadania i instruktaże w tym temacie zakładają, że znasz już elementy docelowe i właściwości programu MSBuild oraz że wiesz, jak używać niestandardowego pliku projektu MSBuild do tworzenia procesu kompilacji i wdrażania. Aby uzyskać więcej informacji, zobacz [Omówienie pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md) i [zrozumienie procesu kompilacji](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

## <a name="creating-and-adding-windows-powershell-scripts"></a>Tworzenie i Dodawanie skryptów programu Windows PowerShell

Zadania w tym temacie używają przykładowego skryptu programu Windows PowerShell o nazwie **LogDeploy. ps1** , aby zilustrować sposób uruchamiania skryptów z programu MSBuild. Skrypt **LogDeploy. ps1** zawiera prostą funkcję, która zapisuje wpis jednowierszowy w pliku dziennika:

[!code-powershell[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample1.ps1)]

Skrypt **LogDeploy. ps1** akceptuje dwa parametry. Pierwszy parametr reprezentuje pełną ścieżkę do pliku dziennika, do którego chcesz dodać wpis, a drugi parametr reprezentuje miejsce docelowe wdrożenia, które chcesz zarejestrować w pliku dziennika. Po uruchomieniu skryptu dodaje wiersz do pliku dziennika w tym formacie:

[!code-html[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample2.html)]

Aby udostępnić skrypt **LogDeploy. ps1** dla programu MSBuild, należy wykonać następujące:

- Dodaj skrypt do kontroli źródła.
- Dodaj skrypt do rozwiązania w programie Visual Studio 2010.

Nie musisz wdrażać skryptu z zawartością rozwiązania, niezależnie od tego, czy zamierzasz uruchomić skrypt na serwerze kompilacji, czy na komputerze zdalnym. Jedną z opcji jest dodanie skryptu do folderu rozwiązania. Przykładowo w programie Contact Manager, ponieważ chcesz użyć skryptu programu Windows PowerShell w ramach procesu wdrażania, warto dodać skrypt do folderu publikowanie rozwiązania.

![](running-windows-powershell-scripts-from-msbuild-project-files/_static/image1.png)

Zawartość folderów rozwiązań jest kopiowana do serwerów kompilacji jako materiał źródłowy. Jednak nie są one częścią żadnego danych wyjściowych projektu.

## <a name="executing-a-windows-powershell-script-on-the-build-server"></a>Wykonywanie skryptu programu Windows PowerShell na serwerze kompilacji

W niektórych scenariuszach może być konieczne uruchomienie skryptów programu Windows PowerShell na komputerze, który kompiluje projekty. Można na przykład użyć skryptu programu Windows PowerShell do oczyszczenia folderów kompilacji lub zapisu wpisów do niestandardowego pliku dziennika.

Pod względem składni uruchomienie skryptu programu Windows PowerShell z pliku projektu MSBuild jest takie samo jak uruchamianie skryptu programu Windows PowerShell z poziomu regularnego wiersza polecenia. Należy wywołać plik wykonywalny programu PowerShell. exe i użyć przełącznika **– polecenia** , aby podać polecenia, które mają być uruchamiane w programie Windows PowerShell. (W programie Windows PowerShell V2 można również użyć przełącznika **– File** ). Polecenie powinno mieć następujący format:

[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample3.cmd)]

Na przykład:

[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample4.cmd)]

Jeśli ścieżka do skryptu zawiera spacje, należy ująć ścieżkę pliku w apostrofy poprzedzone znakiem handlowego "i". Nie można używać podwójnych cudzysłowów, ponieważ zostały one już użyte do objęcia polecenia:

[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample5.cmd)]

Istnieje kilka dodatkowych zagadnień związanych z wywoływaniem tego polecenia z programu MSBuild. Najpierw należy dołączyć flagę **– nieinteractive** , aby upewnić się, że skrypt jest wykonywany w trybie cichym. Następnie należy uwzględnić flagę **– ExecutionPolicy** z odpowiednią wartością argumentu. Określa zasady wykonywania, które program Windows PowerShell zastosuje do skryptu i umożliwia zastąpienie domyślnych zasad wykonywania, co może uniemożliwić wykonanie skryptu. Można wybrać jedną z następujących wartości argumentów:

- Wartość bez **ograniczeń** umożliwi programowi Windows PowerShell wykonywanie skryptu, niezależnie od tego, czy skrypt jest podpisany.
- Wartość **RemoteSigned** umożliwi programowi Windows PowerShell wykonywanie niepodpisanych skryptów, które zostały utworzone na komputerze lokalnym. Jednak skrypty, które zostały utworzone w innym miejscu, muszą być podpisane. (W tym przypadku bardzo mało prawdopodobne jest, że skrypt środowiska Windows PowerShell został utworzony lokalnie na serwerze kompilacji).
- Wartość **wszystkie podpisane** umożliwi programowi Windows PowerShell wykonywanie tylko podpisanych skryptów.

Domyślne zasady wykonywania są **ograniczone**, co uniemożliwia programowi Windows PowerShell uruchamianie plików skryptów.

Na koniec należy wprowadzić wszystkie zastrzeżone znaki XML występujące w poleceniu programu Windows PowerShell:

- Zamień pojedyncze cudzysłowy na **&amp;.**
- Zamień podwójne cudzysłowy na **&amp;quot;**
- Zamień znak "i" na **&amp;amp;**

- Po wprowadzeniu tych zmian polecenie będzie wyglądać następująco:

[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample6.cmd)]

W pliku niestandardowego projektu programu MSBuild można utworzyć nowy cel i użyć zadania **exec** do uruchomienia tego polecenia:

[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample7.xml)]

W tym przykładzie należy zauważyć, że:

- Wszystkie zmienne, takie jak wartości parametrów i lokalizacja pliku wykonywalnego programu Windows PowerShell, są deklarowane jako właściwości programu MSBuild.
- Warunki są dołączone, aby umożliwić użytkownikom przesłonięcie tych wartości z wiersza polecenia.
- Właściwość **MSDeployComputerName** jest zadeklarowana w innym miejscu w pliku projektu.

Po wykonaniu tego celu jako części procesu kompilacji program Windows PowerShell uruchomi polecenie i zapisze wpis dziennika do określonego pliku.

## <a name="executing-a-windows-powershell-script-on-a-remote-computer"></a>Wykonywanie skryptu programu Windows PowerShell na komputerze zdalnym

Program Windows PowerShell jest w stanie uruchamiać skrypty na komputerach zdalnych za poorednictwem [Windows Remote Management](https://msdn.microsoft.com/library/windows/desktop/aa384426.aspx) (WinRM). W tym celu należy użyć polecenia cmdlet [Invoke-Command](https://technet.microsoft.com/library/dd347578.aspx) . Dzięki temu można wykonać skrypt na co najmniej jednym komputerze zdalnym bez kopiowania skryptu do komputerów zdalnych. Wszystkie wyniki są zwracane do komputera lokalnego, z którego uruchomiono skrypt.

> [!NOTE]
> Przed użyciem polecenia cmdlet **Invoke-Command** do wykonywania skryptów programu Windows PowerShell na komputerze zdalnym należy skonfigurować odbiornik usługi WinRM do akceptowania komunikatów zdalnych. Można to zrobić, uruchamiając polecenie **winrm quickconfig** na komputerze zdalnym. Aby uzyskać więcej informacji, zobacz [Instalowanie i Konfigurowanie programu Windows Remote Management](https://msdn.microsoft.com/library/windows/desktop/aa384372(v=vs.85).aspx).

W oknie programu Windows PowerShell Użyj tej składni, aby uruchomić skrypt **LogDeploy. ps1** na komputerze zdalnym:

[!code-powershell[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample8.ps1)]

> [!NOTE]
> Istnieją różne sposoby używania **polecenia Invoke-Command** do uruchamiania pliku skryptu, ale takie podejście jest najbardziej proste, gdy konieczne jest podanie wartości parametrów i zarządzanie ścieżkami ze spacjami.

Po uruchomieniu tego z wiersza polecenia należy wywołać plik wykonywalny programu Windows PowerShell i użyć parametru **– Command** , aby podać instrukcje:

[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample9.cmd)]

Tak jak wcześniej, należy podać dodatkowe przełączniki i wszystkie zastrzeżone znaki XML podczas uruchamiania polecenia z programu MSBuild:

[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample10.cmd)]

Na koniec, tak jak wcześniej, można użyć zadania **exec** w obszarze docelowym niestandardowego programu MSBuild, aby wykonać polecenie:

[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample11.xml)]

Po wykonaniu tego elementu docelowego w ramach procesu kompilacji program Windows PowerShell uruchomi skrypt na komputerze wskazanym w argumencie **– ComputerName** .

## <a name="conclusion"></a>Podsumowanie

W tym temacie opisano sposób uruchamiania skryptu programu Windows PowerShell z pliku projektu programu MSBuild. Tego podejścia można użyć do uruchomienia skryptu programu Windows PowerShell lokalnie lub na komputerze zdalnym w ramach zautomatyzowanego lub jednoetapowego procesu kompilowania i wdrażania.

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać wskazówki dotyczące podpisywania skryptów programu Windows PowerShell i zarządzania zasadami wykonywania, zobacz [Uruchamianie skryptów programu Windows PowerShell](https://technet.microsoft.com/library/ee176949.aspx). Aby uzyskać wskazówki dotyczące uruchamiania poleceń programu Windows PowerShell z komputera zdalnego, zobacz [Uruchamianie poleceń zdalnych](https://technet.microsoft.com/library/dd819505.aspx).

Aby uzyskać więcej informacji na temat używania niestandardowych plików projektów programu MSBuild do kontrolowania procesu wdrażania, zobacz [Omówienie pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md) i [zrozumienie procesu kompilacji](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

> [!div class="step-by-step"]
> [Poprzednie](taking-web-applications-offline-with-web-deploy.md)
> [dalej](troubleshooting-the-packaging-process.md)

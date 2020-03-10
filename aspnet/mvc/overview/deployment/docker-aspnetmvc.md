---
uid: mvc/overview/deployment/docker
title: Migrowanie aplikacji ASP.NET MVC do kontenerów systemu Windows
description: Dowiedz się, jak korzystać z istniejącej aplikacji ASP.NET MVC i uruchamiać ją w kontenerze platformy Docker systemu Windows
keywords: Kontenery systemu Windows, Docker, ASP. NET MVC
author: BillWagner
ms.author: wiwagn
ms.date: 12/14/2018
ms.assetid: c9f1d52c-b4bd-4b5d-b7f9-8f9ceaf778c4
ms.openlocfilehash: ef184f4256c20e2a66de8fd2d4f8e67f07d9a086
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78583630"
---
# <a name="migrating-aspnet-mvc-applications-to-windows-containers"></a>Migrowanie aplikacji ASP.NET MVC do kontenerów systemu Windows

Uruchamianie istniejącej aplikacji opartej na .NET Framework w kontenerze systemu Windows nie wymaga wprowadzania żadnych zmian w aplikacji. Aby uruchomić aplikację w kontenerze systemu Windows, należy utworzyć obraz platformy Docker zawierający aplikację i uruchomić kontener. W tym temacie wyjaśniono, jak zastosować istniejącą [aplikację ASP.NET MVC](http://www.asp.net/mvc) i wdrożyć ją w kontenerze systemu Windows.

Zacznij od istniejącej aplikacji ASP.NET MVC, a następnie Kompiluj opublikowane elementy zawartości przy użyciu programu Visual Studio. Użyj platformy Docker, aby utworzyć obraz, który zawiera i uruchomi aplikację. Przejdź do lokacji działającej w kontenerze systemu Windows i sprawdź, czy aplikacja działa.

W tym artykule przyjęto założenie, że masz podstawową wiedzą dotyczącą platformy Docker. Aby uzyskać informacje dotyczące platformy Docker, przeczytaj artykuł [Docker Overview](https://docs.docker.com/engine/understanding-docker/) (Przegląd platformy Docker).

Aplikacja, która zostanie uruchomiona w kontenerze, jest prostą witryną sieci Web, która umożliwia losowe odpowiedzi na pytania. Ta aplikacja jest podstawową aplikacją MVC bez uwierzytelniania ani magazynu bazy danych. umożliwia skoncentrowanie się na przenoszeniu warstwy sieci Web do kontenera. W przyszłości tematy pokazują, jak przenieść magazyn trwały i zarządzać nim w aplikacjach kontenerowych.

Przeniesienie aplikacji obejmuje następujące kroki:

1. [Tworzenie zadania publikowania w celu skompilowania zasobów dla obrazu.](#publish-script)
1. [Tworzenie obrazu platformy Docker, na którym będzie uruchamiana aplikacja.](#build-the-image)
1. [Uruchamianie kontenera platformy Docker z uruchomionym obrazem.](#start-a-container)
1. [Weryfikowanie aplikacji przy użyciu przeglądarki.](#verify-in-the-browser)

[Gotowa aplikacja](https://github.com/dotnet/samples/tree/master/framework/docker/MVCRandomAnswerGenerator) znajduje się w serwisie GitHub.

## <a name="prerequisites"></a>Wymagania wstępne

Komputer deweloperski musi mieć następujące oprogramowanie:

- [Rocznicowa Aktualizacja systemu Windows 10](https://www.microsoft.com/software-download/windows10/) (lub nowsza) lub [Windows Server 2016](https://www.microsoft.com/cloud-platform/windows-server) (lub nowsza wersja)
- [Docker for Windows](https://docs.docker.com/docker-for-windows/) — wersja stabilna 1.13.0 lub 1,12 beta 26 (lub nowsze wersje)
- [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)

> [!IMPORTANT]
> Jeśli używasz systemu Windows Server 2016, postępuj zgodnie z instrukcjami dotyczącymi [wdrażania hosta kontenerów — Windows Server](https://msdn.microsoft.com/virtualization/windowscontainers/deployment/deployment).

Po zainstalowaniu i uruchomieniu programu Docker kliknij prawym przyciskiem myszy jego ikonę na pasku zadań i wybierz pozycję **Switch to Windows containers** (Przełącz na kontenery systemu Windows). Jest to wymagane do uruchomienia obrazów Docker opartych na systemie Windows. Wykonanie tego polecenia może potrwać kilka sekund:

![Kontener systemu Windows][windows-container]

## <a name="publish-script"></a>Publikuj skrypt

Zbierz w jednym miejscu wszystkie elementy zawartości konieczne do załadowania do obrazu Docker. Możesz użyć polecenia **Opublikuj** Visual Studio, aby utworzyć profil publikacji dla aplikacji. Ten profil umieści wszystkie zasoby w jednym drzewie katalogów, które zostaną skopiowane do obrazu docelowego w dalszej części tego samouczka.

**Kroki publikowania**

1. Kliknij prawym przyciskiem myszy projekt sieci Web w programie Visual Studio, a następnie wybierz pozycję **Publikuj**.
1. Kliknij **przycisk profil niestandardowy**, a następnie wybierz opcję **system plików** jako metodę.
1. Wybierz katalog. Zgodnie z Konwencją pobrany przykład używa `bin\Release\PublishOutput`.

![Publikuj połączenie][publish-connection]

Otwórz sekcję **Opcje publikowania plików** na karcie **Ustawienia** . Wybierz opcję **prekompilowanie podczas publikowania**. Ta optymalizacja oznacza, że zostaną skompilowane widoki w kontenerze platformy Docker, kopiując wstępnie skompilowane widoki.

![Ustawienia publikowania][publish-settings]

Kliknij przycisk **Publikuj**, a program Visual Studio skopiuje wszystkie zasoby, których potrzebujesz do folderu docelowego.

## <a name="build-the-image"></a>Tworzenie obrazu

Utwórz nowy plik o nazwie *pliku dockerfile* , aby zdefiniować obraz platformy Docker. *Pliku dockerfile* zawiera instrukcje dotyczące tworzenia końcowego obrazu i zawiera wszystkie podstawowe nazwy obrazów, wymagane składniki, aplikację, którą chcesz uruchomić, oraz inne obrazy konfiguracyjne. *Pliku dockerfile* to dane wejściowe polecenia `docker build`, które tworzy obraz.

W tym ćwiczeniu utworzysz obraz na podstawie obrazu `microsoft/aspnet` znajdującego się w usłudze [Docker Hub](https://hub.docker.com/r/microsoft/aspnet/).
Obraz podstawowy, `microsoft/aspnet`, jest obrazem systemu Windows Server. Zawiera on systemy Windows Server Core, IIS i ASP.NET 4.7.2. Po uruchomieniu tego obrazu w kontenerze zostaną automatycznie uruchomione usługi IIS i zainstalowane witryny sieci Web.

Pliku dockerfile tworzący obraz wygląda następująco:

```console
# The `FROM` instruction specifies the base image. You are
# extending the `microsoft/aspnet` image.

FROM microsoft/aspnet

# The final instruction copies the site you published earlier into the container.
COPY ./bin/Release/PublishOutput/ /inetpub/wwwroot
```

W tym pliku Dockerfile nie ma polecenia `ENTRYPOINT`. Nie jest ono potrzebne. W przypadku korzystania z systemu Windows Server z usługami IIS proces usług IIS jest punktem wejścia, który jest konfigurowany do uruchamiania na podstawowym obrazie ASPNET.

Uruchom polecenie Docker Build, aby utworzyć obraz z uruchomioną aplikacją ASP.NET. W tym celu Otwórz okno programu PowerShell w katalogu projektu i wpisz następujące polecenie w katalogu rozwiązania:

```console
docker build -t mvcrandomanswers .
```

To polecenie spowoduje skompilowanie nowego obrazu przy użyciu instrukcji w pliku dockerfile, nadawanie nazwy (-t znakowanie) obrazu jako mvcrandomanswers. Może to obejmować ściąganie obrazu podstawowego z usługi [Docker Hub](http://hub.docker.com), a następnie dodanie aplikacji do tego obrazu.

Po zakończeniu tego polecenia można uruchomić `docker images` polecenie, aby wyświetlić informacje o nowym obrazie:

```console
REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
mvcrandomanswers              latest              86838648aab6        2 minutes ago       10.1 GB
```

Identyfikator obrazu będzie różny na twoim komputerze. Teraz Uruchommy aplikację.

## <a name="start-a-container"></a>Uruchamianie kontenera

Uruchom kontener, wykonując następujące `docker run` polecenie:

```console
docker run -d --name randomanswers mvcrandomanswers
```

Argument `-d` informuje platformę Docker o konieczności uruchomienia obrazu w trybie odłączonym. Oznacza to, że obraz platformy Docker zostanie rozłączony z bieżącą powłoką.

W wielu przykładach platformy Docker można zobaczyć-p, aby zmapować kontener i porty hosta. Domyślny obraz ASPNET już skonfigurował kontener do nasłuchiwania na porcie 80 i uwidaczniania go.

`--name randomanswers` nadaje nazwę uruchomionemu kontenerowi. W większości poleceń można użyć tej nazwy zamiast identyfikatora kontenera.

`mvcrandomanswers` to nazwa obrazu do uruchomienia.

## <a name="verify-in-the-browser"></a>Weryfikowanie w przeglądarce

Po rozpoczęciu kontenera Połącz się z uruchomionym kontenerem przy użyciu `http://localhost` w pokazanym przykładzie. Wpisz ten adres URL w przeglądarce i zobacz działającą lokację.

> [!NOTE]
> Niektóre oprogramowanie sieci VPN lub serwera proxy może uniemożliwić przechodzenie do witryny.
> Możesz tymczasowo ją wyłączyć, aby upewnić się, że kontener działa.

Przykładowy katalog w witrynie GitHub zawiera [skrypt programu PowerShell](https://github.com/dotnet/samples/blob/master/framework/docker/MVCRandomAnswerGenerator/run.ps1) , który wykonuje te polecenia. Otwórz okno programu PowerShell, Zmień katalog na katalog rozwiązania, a następnie wpisz:

```console
./run.ps1
```

Powyższe polecenie kompiluje obraz, wyświetla listę obrazów na komputerze i uruchamia kontener.

Aby zatrzymać kontener, wydaj polecenie `docker stop`:

```console
docker stop randomanswers
```

Aby usunąć kontener, wydaj polecenie `docker rm`:

```console
docker rm randomanswers
```

[windows-container]: media/aspnetmvc/SwitchContainer.png "Przełącz do kontenera systemu Windows"
[publish-connection]: media/aspnetmvc/PublishConnection.png "Publikuj w systemie plików"
[publish-settings]: media/aspnetmvc/PublishSettings.png "Ustawienia publikowania"

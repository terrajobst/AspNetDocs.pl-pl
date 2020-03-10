---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview
title: 'Wdrażanie w sieci Web dla przedsiębiorstw: Omówienie scenariusza | Microsoft Docs'
author: jrjlee
description: Ten zestaw samouczków używa przykładowego rozwiązania o realistycznym poziomie złożoności, a także fikcyjnego scenariusza wdrażania przedsiębiorstwa, aby zapewnić odwołanie...
ms.author: riande
ms.date: 05/03/2012
ms.assetid: aa862153-4cd8-4e33-beeb-abf502c6664f
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview
msc.type: authoredcontent
ms.openlocfilehash: 9786879844da13c21e6a953b1ab24b29ca8121e2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78574131"
---
# <a name="enterprise-web-deployment-scenario-overview"></a>Wdrażanie w Internecie w przedsiębiorstwie: omówienie scenariuszy

Autor [Jason Lewandowski](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Ten zestaw samouczków korzysta z przykładowego rozwiązania o realistycznym poziomie złożoności, a także fikcyjnego scenariusza wdrażania przedsiębiorstwa, aby zapewnić implementację referencyjną i zadawać zadania i instruktażować wspólny kontekst. W tym temacie opisano Scenariusz samouczka i wprowadzono przykładowe rozwiązanie.

## <a name="scenario-description"></a>Opis scenariusza

Firma Fabrikam, Inc., fikcyjna firma, tworzy rozwiązanie, które umożliwia zespołom ds. sprzedaży przechowywanie i pobieranie informacji kontaktowych z interfejsu internetowego.

Procesy zarządzania cyklem życia aplikacji (ALM) w firmie Fabrikam, Inc. wymagają wdrożenia rozwiązania w trzech środowiskach serwerowych na różnych etapach procesu tworzenia oprogramowania:

- Test dewelopera lub środowisko piaskownicy.
- Środowisko przejściowe oparte na intranecie.
- Środowisko produkcyjne dostępne w Internecie.

Każdy z tych środowisk ma inne wymagania dotyczące konfiguracji i zabezpieczeń, a każdy z nich stanowi unikatowe wyzwania dotyczące wdrażania.

### <a name="the-fabrikam-inc-server-infrastructure"></a>Infrastruktura firmy Fabrikam, Inc. Server

Jest to infrastruktura tworzenia i wdrażania wysokiego poziomu w firmie Fabrikam, Inc.

![](enterprise-web-deployment-scenario-overview/_static/image1.png)

Stacje robocze deweloperów, infrastruktura kontroli źródła, środowisko testowe dewelopera i środowisko przejściowe znajdują się w sieci intranetowej w domenie Fabrikam.net. Środowisko produkcyjne znajduje się w sieci obwodowej (znanej także jako DMZ, strefa zdemilitaryzowana i podsieć z osłoną), która jest odizolowana od sieci intranetowej przez zaporę. Jest to typowy scenariusz wdrażania: zwykle izolujesz internetowe serwery sieci Web od wewnętrznej infrastruktury serwerów za pomocą zapór i serwerów bramy.

W tym przykładzie:

- Serwer Team Foundation Server (TFS) 2010 z osobnym serwerem kompilacji zapewnia funkcję kontroli źródła i ciągłej integracji (CI).
- Środowisko testowe dla deweloperów Internet Information Services obejmuje serwer sieci Web (IIS) 7,5 oraz serwer bazy danych SQL Server 2008 R2.
- Środowisko produkcyjne obejmuje wiele serwerów sieci Web usług IIS 7,5 synchronizowanych przez serwer kontrolera sieci Web (WFF), a także serwer bazy danych SQL Server 2008 R2. W tym przypadku serwer bazy danych może używać klastrowania lub dublowania w celu poprawy skalowalności i dostępności.
- Środowisko przejściowe jest przeznaczone do replikowania konfiguracji środowiska produkcyjnego tak blisko jak to możliwe.
- Zasady izolacji zapory i sieci nie zezwalają na bezpośrednie, zautomatyzowane wdrażanie z intranetu do sieci obwodowej.

Konfiguracja każdego z tych środowisk została szczegółowo opisana w drugim samouczku, [Konfigurowanie środowisk serwera do wdrażania w sieci Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md).

### <a name="team-roles-for-alm"></a>Role zespołu dla usługi ALM

Ci użytkownicy są włączeni do tworzenia i publikowania rozwiązania Contact Manager oraz zarządzania nim:

- Hink matowy to Deweloper aplikacji sieci Web w firmie Fabrikam, Inc. Jest częścią zespołu, który opracował rozwiązanie Contact Manager za pomocą programu Visual Studio 2010. Usuń otoczkę ma pełnych praw administratora na serwerach w środowisku testowym programisty, który umożliwia skonfigurowanie środowiska do zaspokajania potrzeb. Ma również dostęp użytkownika do wystąpienia programu Visual Studio 2010 TFS, gdzie przechowuje kod źródłowy dla rozwiązania Contact Manager.
- Rob Walters to administrator serwera dla zespołu deweloperów firmy Fabrikam. Rob ma dostęp administracyjny na serwerze TFS, dzięki czemu może skonfigurować wszystkie aspekty programu TFS i kompilacji zespołowej. Rob ma również dostęp administracyjny do serwerów testowych i przejściowych oraz działa jako administrator bazy danych (DBA) dla serwerów bazy danych w środowiskach testowych i przejściowych. Rob skonfigurował kompilację zespołu na serwerze TFS, aby wykonać następujące zadania:

    - Kompiluj i uruchamiaj testy jednostkowe na aplikacji za każdym razem, gdy użytkownik ewidencjonuje plik w programie TFS. Ta nazwa jest nazywana CI.
    - Wdróż aplikację Contact Manager w środowisku testowym automatycznie, gdy aplikacja przejdzie testy jednostkowe. Obejmuje to opublikowanie bazy danych na serwerach testowych przy początkowym wdrożeniu i wszystkie aktualizacje bazy danych po początkowym wdrożeniu.
    - Wdróż aplikację Contact Manager w środowisku przejściowym w procesie jednoetapowym.
    - Utwórz pakiet sieci Web, którego administrator serwera sieci Web i administrator może użyć do opublikowania aplikacji w środowisku produkcyjnym.
- Lisa Andrews to administrator serwera odpowiedzialny za wdrażanie aplikacji na serwerach produkcyjnych Fabrikam, Inc. Ma dostęp do odczytu do udziału, w którym Kompilacja zespołu TFS przechowuje pakiet wdrożeniowy sieci Web po kompilacji aplikacji Contact Manager. Ma również dostęp administracyjny do produkcyjnych serwerów sieci Web, dzięki czemu może wdrożyć aplikację w środowisku produkcyjnym. Ponadto działa jako administrator, który wdraża bazy danych i aktualizacje bazy danych na serwerze bazy danych w środowisku produkcyjnym.

<a id="_The_Contact_Manager"></a>

### <a name="the-contact-manager-solution"></a>Rozwiązanie Contact Manager

Rozwiązanie Contact Manager zostało zaprojektowane, aby umożliwić zarejestrowanym użytkownikom logowanie do dodawania i edytowania informacji kontaktowych za pomocą interfejsu internetowego. Rozwiązanie Contact Manager składa się z czterech pojedynczych projektów:

![](enterprise-web-deployment-scenario-overview/_static/image2.png)

- **ContactManager. MVC**. Jest to projekt aplikacji sieci Web ASP.NET MVC3, który reprezentuje punkt wejścia dla rozwiązania. Oferuje ona podstawowe funkcje aplikacji sieci Web, takie jak umożliwienie użytkownikom tworzenia i wyświetlania szczegółów kontaktu. Aplikacja korzysta z usługi Windows Communication Foundation (WCF) do zarządzania kontaktami i bazą danych usług aplikacji ASP.NET w celu zarządzania uwierzytelnianiem i autoryzacją.
- **ContactManager. Database**. To jest projekt bazy danych programu Visual Studio 2010. Projekt definiuje schemat bazy danych, która przechowuje szczegóły kontaktów.
- **ContactManager. Service**. To jest projekt usługi sieci Web WCF. Program WCF udostępnia punkt końcowy, który umożliwia programom wywołującym wykonywanie operacji tworzenia, pobierania, aktualizowania i usuwania (CRUD) w bazie danych programu Contact Manager. Usługa korzysta z bazy danych Menedżera kontaktów i zestawu ContactManager. Common. dll.
- **ContactManager. Common**. To jest projekt biblioteki klas. Usługa WCF opiera się na typach zdefiniowanych w tym zestawie.

Pełny przegląd rozwiązania i jego wymagania dotyczące wdrażania znajdują się w pierwszym samouczku w tej serii, w [ramach wdrożenia sieci Web w przedsiębiorstwie](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md).

<a id="_Deployment_Tasks"></a>

## <a name="deployment-tasks"></a>Zadania wdrażania

Istnieje kilka różnych zadań związanych z wdrażaniem aplikacji w różnych środowiskach w dużych organizacjach. Oto kluczowe zadania, które obejmują samouczki:

![](enterprise-web-deployment-scenario-overview/_static/image3.png)

Poniżej znajduje się lista wszystkich kroków w procesie wdrażania z perspektywy użytkowników opisanych wcześniej w tym dokumencie:

1. Wszyscy członkowie zespołu przeglądają rozwiązanie Contact Manager w programie Visual Studio 2010, aby określić wymagania i problemy związane z wdrażaniem.
2. Hink firmy matowej może wdrożyć rozwiązanie Contact Manager bezpośrednio z stacji roboczej dewelopera do środowiska testowego dewelopera, aby przeprowadzić początkowy test logiki wdrożenia.
3. Hink matowy dodaje aplikację do kontroli źródła w programie TFS.
4. Rob Walters tworzy różne definicje kompilacji dla rozwiązania Contact Manager w kompilacji zespołowej. Jedna definicja kompilacji używa elementu CI do wdrożenia rozwiązania w środowisku testowym dewelopera za każdym razem, gdy użytkownik ewidencjonuje nowy kod. Inna definicja kompilacji umożliwia użytkownikom wyzwalanie wdrożeń w środowisku przejściowym zgodnie z wymaganiami.
5. Za każdym razem, gdy użytkownik ewidencjonuje nowy kod, Kompilacja zespołu automatycznie kompiluje składniki rozwiązania, uruchamia testy jednostkowe i wdraża rozwiązanie w środowisku testowym dewelopera, jeśli kompilacja zakończyła się powodzeniem, a testy jednostkowe zostały zakończone pomyślnie.
6. Gdy użytkownik wyzwala wdrożenie w środowisku przejściowym, rozwiązanie zostanie spakowane i wdrożone w procesie jednoetapowym. Ten proces generuje również pakiet do ręcznego wdrożenia w środowisku produkcyjnym.
7. Lisa Andrews wdraża aplikację w środowisku produkcyjnym, ręcznie importując pakiet internetowy utworzony w kroku 6.

### <a name="key-deployment-issues"></a>Problemy z wdrażaniem kluczy

Rozwiązanie Contact Manager i Fabrikam, Inc. — wyróżnienie różnych typowych problemów i wyzwań, które mogą wystąpić podczas wdrażania złożonych rozwiązań w skali korporacyjnej. Na przykład:

- Musisz być w stanie wdrożyć projekty w wielu środowiskach, takich jak środowiska deweloperskie lub testowe, platformy przejściowe i serwery produkcyjne. Rozwiązanie należy wdrożyć z różnymi ustawieniami konfiguracji dla każdego środowiska.
- Należy wdrożyć wiele zależnych projektów jednocześnie w ramach jednego kroku lub zautomatyzowanego procesu kompilowania i wdrażania.
- Musisz mieć możliwość nawiązywania wdrożenia przez proces zautomatyzowany. Na przykład, chcesz użyć procesu CI do wdrożenia aplikacji sieci Web w środowisku przejściowym, gdy nowy kod jest zaewidencjonowany.
- Musisz mieć możliwość kontrolowania procesu wdrażania i ustawiania zmiennych wdrożenia spoza programu Visual Studio, ponieważ deweloperzy nie będą mieć wystarczających ustawień konfiguracji lub wymaganych poświadczeń dla każdego środowiska docelowego.
- Należy wdrożyć projekty bazy danych oparte na schemacie i zachować istniejące dane w kolejnych wdrożeniach.
- Należy wdrożyć bazy danych członkostwa na zasadzie ad hoc bez wdrażania danych konta użytkownika. Może być również konieczne zaktualizowanie schematu wdrożonych baz danych członkostwa bez utraty istniejących danych konta użytkownika.
- Podczas wdrażania zawartości w różnych środowiskach docelowych należy wykluczyć określone pliki lub foldery.

Ponadto zarządzanie wdrożeniem w przypadku częstej aktualizacji i przyrostowe zgłasza kilka dodatkowych wyzwań. Na przykład:

- Testy jednostkowe są uruchamiane za każdym razem, gdy deweloper sprawdzi nowy kod. Należy tylko wdrożyć rozwiązanie, jeśli kod przejdzie testy jednostkowe.
- Podczas wdrażania aplikacji sieci Web w środowisku przejściowym lub produkcyjnym należy przekierować użytkowników do *aplikacji\_pliku offline. htm* na czas trwania procesu wdrożenia.
- Chcesz rejestrować działania wdrożeniowe. Proces wdrażania powinien wysyłać powiadomienia e-mail o powodzeniu lub nieudanych wdrożeniach do wyznaczonych odbiorców.
- Jeśli automatyczne wdrożenie nie powiedzie się, proces wdrażania powinien ponowić próbę bieżącego wdrożenia lub wdrożyć poprzedni pakiet internetowy.

> [!div class="step-by-step"]
> [Poprzednie](deploying-web-applications-in-enterprise-scenarios.md)
> [dalej](application-lifecycle-management-from-development-to-production.md)

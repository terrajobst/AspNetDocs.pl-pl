---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/the-contact-manager-solution
title: Rozwiązanie Contact Manager | Microsoft Docs
author: jrjlee
description: W tej serii samouczków jest stosowane przykładowe&#x2014;rozwiązanie&#x2014;do reprezentowania aplikacji w skali korporacyjnej o realistycznej Leve...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 4d8c8d19-055b-4b70-9ee1-f748f0db3a01
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/the-contact-manager-solution
msc.type: authoredcontent
ms.openlocfilehash: 12ed7827f7392e559e04121386f7cd045de8462b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78586269"
---
# <a name="the-contact-manager-solution"></a>Rozwiązanie Contact Manager

Autor [Jason Lewandowski](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tej [serii samouczków](web-deployment-in-the-enterprise.md) jest stosowane przykładowe&#x2014;rozwiązanie&#x2014;do tworzenia aplikacji w skali korporacyjnej o realistycznym poziomie złożoności. W tym temacie przedstawiono rozwiązanie Contact Manager, opisano główne składniki rozwiązania i zidentyfikowano wyzwania w zakresie wdrażania tego rodzaju aplikacji na różnych platformach docelowych w środowisku przedsiębiorstwa.
> 
> Podczas pracy z tematami w tych samouczkach można użyć rozwiązania Contact Manager jako implementacji referencyjnej, która pokazuje, jak można spełnić konkretne wyzwania w scenariuszach wdrażania w przedsiębiorstwie. W następnym temacie [Konfigurowanie rozwiązania Contact Manager](setting-up-the-contact-manager-solution.md)opisano sposób pobierania i uruchamiania rozwiązania na stacji roboczej dewelopera.

## <a name="solution-overview"></a>Przegląd rozwiązania

Rozwiązanie Contact Manager składa się z czterech pojedynczych projektów:

![](the-contact-manager-solution/_static/image1.png)

- **ContactManager. MVC**. Jest to projekt aplikacji sieci Web ASP.NET MVC 3, który reprezentuje punkt wejścia dla rozwiązania. Oferuje ona podstawowe funkcje aplikacji sieci Web, takie jak umożliwienie użytkownikom tworzenia i wyświetlania szczegółów kontaktu. Aplikacja korzysta z usługi Windows Communication Foundation (WCF) do zarządzania kontaktami i bazą danych usług aplikacji ASP.NET w celu zarządzania uwierzytelnianiem i autoryzacją.
- **ContactManager. Database**. To jest projekt bazy danych programu Visual Studio. Projekt definiuje schemat bazy danych, która przechowuje szczegóły kontaktów.
- **ContactManager. Service**. To jest projekt usługi sieci Web WCF. Usługa WCF udostępnia punkt końcowy, który umożliwia programom wywołującym wykonywanie operacji tworzenia, pobierania, aktualizowania i usuwania (CRUD) w bazie danych **ContactManager** . Usługa jest zależna od bazy danych **ContactManager** i zestawu **ContactManager. Common. dll** .
- **ContactManager. Common**. To jest projekt biblioteki klas. Usługa WCF opiera się na typach zdefiniowanych w tym zestawie.

Rozwiązanie zawiera również folder rozwiązania o nazwie Publish. Zawiera różne pliki niestandardowego projektu i pliki poleceń, które pokazują, jak można kontrolować i manipulować procesem kompilowania i wdrażania. Są one omówione bardziej szczegółowo w dalszej części tego samouczka.

Na poziomie koncepcyjnym składniki rozwiązania pasują do siebie w następujący sposób:

![](the-contact-manager-solution/_static/image2.png)

> [!NOTE]
> Mimo że aplikacja sieci Web ASP.NET MVC 3 używa dostawcy członkostwa ASP.NET, wszystkie strony w aplikacji sieci Web zezwalają na dostęp anonimowy. Nie jest to wyraźnie konfiguracja realistyczna. Jednak rozwiązanie jest skonfigurowane w taki sposób, aby ułatwić wdrożenie i przetestowanie rozwiązania bez konfigurowania kont i ról użytkowników.

## <a name="deployment-challenges"></a>Wyzwania dotyczące wdrażania

Rozwiązanie Contact Manager ilustruje kilka wyzwań związanych z wdrażaniem, które są wspólne dla wielu scenariuszy wdrażania w przedsiębiorstwie:

- Rozwiązanie składa się z wielu projektów zależnych. Należy wdrożyć te projekty jednocześnie.
- Parametry połączenia i punkty końcowe usługi należy zaktualizować dla każdego środowiska, a w wielu przypadkach te informacje nie będą dostępne dla deweloperów.
- Podczas wdrażania bazy danych **ContactManager** w środowiskach przejściowych i produkcyjnych należy zachować istniejące dane w kolejnych wdrożeniach.
- Podczas wdrażania bazy danych usług aplikacji ASP.NET należy wdrożyć pewne dane konfiguracji, ale pominąć wszystkie dane konta użytkownika.
- Projekty obejmują niektóre pliki i foldery, które nie powinny zostać wdrożone. Należy wykluczyć te pliki i foldery z procesu wdrażania.
- Rozwiązanie musi obsługiwać Zautomatyzowane wdrażanie z serwera kompilacji Team Foundation Server (TFS).

## <a name="conclusion"></a>Podsumowanie

Ten temat zawiera ogólne omówienie rozwiązania Contact Manager i zidentyfikowano niektóre z problemów związanych z wdrażaniem, które są wspólne dla wielu scenariuszy wdrażania w przedsiębiorstwie. W pozostałych tematach w tym samouczku opisano niektóre techniki, których można użyć w celu spełnienia tych wyzwań.

W następnym temacie [Konfigurowanie rozwiązania Contact Manager](setting-up-the-contact-manager-solution.md)opisano sposób pobierania i uruchamiania rozwiązania na stacji roboczej dewelopera.

> [!div class="step-by-step"]
> [Poprzednie](web-deployment-in-the-enterprise.md)
> [dalej](setting-up-the-contact-manager-solution.md)

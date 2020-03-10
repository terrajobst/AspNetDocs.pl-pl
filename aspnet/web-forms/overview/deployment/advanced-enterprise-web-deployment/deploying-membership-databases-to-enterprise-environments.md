---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments
title: Wdrażanie baz danych członkostwa w środowiskach korporacyjnych | Microsoft Docs
author: jrjlee
description: W tym temacie wyjaśniono kluczowe zagadnienia i wyzwania, które należy wziąć pod uwagę w przypadku udostępniania baz danych usług aplikacji ASP.NET (części wspólną...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 3cf765df-d311-4f68-a295-c9685ceea830
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments
msc.type: authoredcontent
ms.openlocfilehash: 50f49af502b75aa5ad52756a76a5e7340aca53f7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78526006"
---
# <a name="deploying-membership-databases-to-enterprise-environments"></a>Wdrażanie baz danych członkostwa w środowiskach przedsiębiorstw

Autor [Jason Lewandowski](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie wyjaśniono kluczowe zagadnienia i wyzwania, które należy wziąć pod uwagę podczas udostępniania baz danych usług aplikacji ASP.NET (nazywanych najczęściej bazami danych członkostwa) w środowiskach testowych, przejściowych lub produkcyjnych. Opisano w nim również podejścia, których można użyć w celu spełnienia tych wyzwań.

Ten temat stanowi część szeregu samouczków opartych na wymaganiach dotyczących wdrażania w przedsiębiorstwie fikcyjnej firmy o nazwie Fabrikam, Inc. W tej serii samouczków jest stosowane&#x2014;przykładowe rozwiązanie&#x2014;do reprezentowania aplikacji sieci Web, [które ma](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)realistyczny poziom złożoności, w tym aplikacji ASP.NET MVC 3, usługi Windows Communication Foundation (WCF) i projektu bazy danych.

Metoda wdrażania w tym samouczku jest oparta na rozłożeniu pliku projektu dzielonego opisanym w artykule [Omówienie pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), w którym proces kompilacji jest kontrolowany przez dwa pliki&#x2014;projektu, zawierający instrukcje kompilacji, które mają zastosowanie do każdego środowiska docelowego, oraz jeden zawierający ustawienia kompilacji i wdrożenia specyficznego dla środowiska. W czasie kompilacji plik projektu specyficzny dla środowiska jest scalany z plikiem projektu Environment-niezależny od w celu utworzenia kompletnego zestawu instrukcji kompilacji.

## <a name="what-are-the-issues-when-you-deploy-a-membership-database"></a>Jakie problemy występują podczas wdrażania bazy danych członkostwa?

W większości przypadków podczas opracowywania strategii wdrażania bazy danych najpierw należy rozważyć, jakie dane mają zostać wdrożone. W środowisku deweloperskim lub testowym można wdrożyć dane konta użytkownika w celu ułatwienia szybkiego i łatwego testowania. W środowisku przejściowym lub produkcyjnym bardzo mało prawdopodobne jest, że chcesz wdrożyć dane konta użytkownika.

Niestety, ASP.NET bazy danych członkostwa zawierają pewne konkretne wyzwania, które czynią tę decyzję znacznie bardziej skomplikowaną:

- Wdrożenie tylko do schematu spowoduje pozostawienie bazy danych członkostwa w stanie nieoperacyjnym. Wynika to z faktu, że baza danych członkostwa zawiera dane konfiguracji (w tabeli **aspnet\_SchemaVersions** ), które są wymagane do działania bazy danych. W związku z tym w przypadku wykonywania wdrożenia tylko schematu bazy danych członkostwa w celu wykluczania danych konta użytkownika należy uruchomić skrypt po wdrożeniu, aby dodać podstawowe dane konfiguracyjne.
- W zależności od sposobu skonfigurowania bazy danych członkostwa dostawca członkostwa może użyć klucza komputera do szyfrowania haseł i przechowywania ich w bazie danych. W takim przypadku wszystkie dane konta użytkownika wdrażane za pomocą bazy danych staną się niedostępne na serwerze docelowym. Z tego powodu wdrażanie danych konta użytkownika nie jest obsługiwanym scenariuszem.

## <a name="choosing-a-membership-database-strategy"></a>Wybieranie strategii bazy danych członkostwa

Te wskazówki należy stosować podczas wybierania sposobu aprowizacji bazy danych członkostwa w środowisku serwera przedsiębiorstwa:

- Gdy jest to możliwe, nie należy wdrażać baz danych członkostwa. Zamiast tego Utwórz bazę danych członkostwa ręcznie na docelowym serwerze bazy danych. Jeśli schemat bazy danych członkostwa nie został dostosowany, można po prostu utworzyć nowe miejsce w miejscu docelowym przy użyciu [narzędzia rejestracji SQL Server ASP.NET (aspnet\_regsql. exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx).
- Jeśli nie ma opcji, ale do wdrożenia bazy danych&#x2014;członkostwa na przykład, jeśli wprowadzono liczne modyfikacje schematu&#x2014;bazy danych, należy wykonać wdrożenie tylko do schematu bazy danych członkostwa, aby wykluczyć dane konta użytkownika, a następnie uruchomić skrypt po wdrożeniu w celu dodania wymaganych danych konfiguracyjnych. Wskazówki dotyczące tych metod można znaleźć w następujących [tematach: Wdrażanie bazy danych ASP.NET Membership bez uwzględnienia kont użytkowników](https://msdn.microsoft.com/library/ff361972(v=vs.100).aspx).

Należy pamiętać, że *schemat bazy danych członkostwa może być dość statyczny*. Nawet jeśli dostosowano bazę danych członkostwa, nie jest mało prawdopodobne, że należy zaktualizować schemat regularnie,&#x2014;ale nie będzie on zmieniany z taką samą częstotliwością jak kod w aplikacji sieci Web lub projekt bazy danych. W związku z tym nie trzeba uwzględniać bazy danych członkostwa w żadnym zautomatyzowanym lub jednoetapowym procesie wdrażania.

## <a name="using-vsdbcmd-to-update-a-membership-database-schema"></a>Aktualizowanie schematu bazy danych członkostwa przy użyciu VSDBCMD

Jeśli zmodyfikujesz strukturę bazy danych członkostwa po pierwszym wdrożeniu, możesz nie chcieć używać narzędzia do wdrażania w sieci Web (Web Deploy) Internet Information Services (IIS), aby ponownie wdrożyć bazę danych. Funkcja wdrażania bazy danych w Web Deploy nie obejmuje możliwości wprowadzania różnicowych aktualizacji do docelowej bazy danych&#x2014;, Web Deploy musi porzucić i ponownie utworzyć bazę danych. Oznacza to utratę wszelkich istniejących danych konta użytkownika, które zwykle są niepożądane w środowisku przejściowym lub produkcyjnym.

Alternatywą jest użycie narzędzia VSDBCMD w celu zaktualizowania schematu docelowej bazy danych. VSDBCMD obejmuje dwie ważne funkcje. Najpierw można zaimportować schemat istniejącej bazy danych do pliku. DbSchema. Następnie może wdrożyć plik. DbSchema w istniejącej bazie danych jako aktualizację różnicową, co oznacza, że tylko zmiany wymagane do przeprowadzenia aktualnej docelowej bazy danych i nie zostaną utracone żadne dane.

Aby zaktualizować schemat bazy danych członkostwa, można użyć tych kroków wysokiego poziomu:

1. Użyj akcji **importu** VSDBCMD, aby wygenerować plik. DbSchema dla źródłowej bazy danych członkostwa. Ta procedura została opisana w artykule [jak: importowanie schematu z wiersza polecenia](https://msdn.microsoft.com/library/dd172135.aspx).
2. Użyj akcji **Wdróż** VSDBCMD, aby wdrożyć plik. DbSchema w docelowej bazie danych członkostwa. Ta procedura jest opisana w [wierszu polecenia Reference for VSDBCMD. EXE (wdrożenie i importowanie schematu)](https://msdn.microsoft.com/library/dd193283.aspx).

## <a name="conclusion"></a>Podsumowanie

W tym temacie opisano niektóre problemy, które mogą wystąpić w przypadku konieczności aprowizacji baz danych członkostwa ASP.NET w różnych środowiskach docelowych. W szczególności wyjaśniono, dlaczego wdrożenia tylko do schematu pozostawiają bazę danych członkostwa w stanie nieoperacyjnym i dlaczego wdrażanie danych konta użytkownika nie jest obsługiwane. Temat zawiera również wskazówki dotyczące udostępniania, wdrażania i aktualizowania baz danych członkostwa w różnych scenariuszach.

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej wskazówek i przykłady korzystania z VSDBCMD, zobacz [informacje dotyczące wiersza polecenia dla VSDBCMD. EXE (wdrożenie i importowanie schematu)](https://msdn.microsoft.com/library/dd193283.aspx) i [instrukcje: importowanie schematu z wiersza polecenia](https://msdn.microsoft.com/library/dd172135.aspx). Aby uzyskać więcej informacji na temat używania ASPNET\_regsql. exe do tworzenia baz danych członkostwa, zobacz [ASP.NET SQL Server Registration Tool (aspnet\_regsql. exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx). Aby uzyskać ogólne wskazówki dotyczące wdrażania baz danych członkostwa, zobacz [How to: Deploy the ASP.NET Membership Database bez uwzględnienia kont użytkowników](https://msdn.microsoft.com/library/ff361972(v=vs.100).aspx).

> [!div class="step-by-step"]
> [Poprzednie](deploying-database-role-memberships-to-test-environments.md)
> [dalej](excluding-files-and-folders-from-deployment.md)

---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment
title: Tworzenie definicji kompilacji, która obsługuje wdrożenie | Microsoft Docs
author: jrjlee
description: Jeśli chcesz wykonać dowolny rodzaj kompilacji w Team Foundation Server (TFS) 2010, musisz utworzyć definicję kompilacji w projekcie zespołowym. Ten temat des...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: fe47a018-f6d0-4979-80e7-5b1fa75a5865
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment
msc.type: authoredcontent
ms.openlocfilehash: e11c91a824446572aaf0b3bc6954b9b8ffb4eaff
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78604000"
---
# <a name="creating-a-build-definition-that-supports-deployment"></a>Tworzenie definicji kompilacji, która obsługuje wdrożenie

Autor [Jason Lewandowski](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Jeśli chcesz wykonać dowolny rodzaj kompilacji w Team Foundation Server (TFS) 2010, musisz utworzyć definicję kompilacji w projekcie zespołowym. W tym temacie opisano sposób tworzenia nowej definicji kompilacji w programie TFS oraz kontrolowania wdrożenia sieci Web w ramach procesu kompilacji w kompilacji zespołu.

Ten temat stanowi część szeregu samouczków opartych na wymaganiach dotyczących wdrażania w przedsiębiorstwie fikcyjnej firmy o nazwie Fabrikam, Inc. W tej serii samouczków jest stosowane&#x2014;przykładowe rozwiązanie&#x2014;do reprezentowania aplikacji sieci Web, [które ma](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)realistyczny poziom złożoności, w tym aplikacji ASP.NET MVC 3, usługi Windows Communication Foundation (WCF) i projektu bazy danych.

Metoda wdrażania w tym samouczku jest oparta na rozłożeniu pliku projektu dzielonego opisanym w artykule [Omówienie pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), w którym proces kompilacji i wdrożenia jest kontrolowany przez dwa pliki&#x2014;projektu, zawierający instrukcje kompilacji, które mają zastosowanie do każdego środowiska docelowego, oraz jeden zawierający ustawienia kompilacji i wdrożenia specyficznego dla środowiska. W czasie kompilacji plik projektu specyficzny dla środowiska jest scalany z plikiem projektu Environment-niezależny od w celu utworzenia kompletnego zestawu instrukcji kompilacji.

## <a name="task-overview"></a>Przegląd zadania

Definicja kompilacji jest mechanizmem, który kontroluje, jak i kiedy powstają kompilacje dla projektów zespołowych w programie TFS. Każda definicja kompilacji określa:

- Elementy, które chcesz skompilować, takie jak pliki rozwiązania programu Visual Studio lub pliki projektu niestandardowego Microsoft Build Engine (MSBuild).
- Kryteria, które określają, kiedy należy wykonać kompilację, takie jak wyzwalacze ręczne, ciągła integracja (CI) lub ewidencjonowanie warunkowe.
- Lokalizacja, do której Kompilacja zespołu powinna wysyłać dane wyjściowe kompilacji, w tym artefakty wdrożenia, takie jak pakiety sieci Web i skrypty baz danych.
- Czas, przez który każda kompilacja powinna zostać zachowana.
- Różne inne parametry procesu kompilacji.

> [!NOTE]
> Aby uzyskać więcej informacji na temat definicji kompilacji, zobacz [Definiowanie procesu kompilacji](https://msdn.microsoft.com/library/ms181715.aspx).

W tym temacie pokazano, jak utworzyć definicję kompilacji korzystającą z elementu CI, aby kompilacja była wyzwalana, gdy deweloper sprawdzi nową zawartość. Jeśli kompilacja zakończy się powodzeniem, usługa kompilacji uruchamia niestandardowy plik projektu w celu wdrożenia rozwiązania w środowisku testowym.

W przypadku wyzwolenia kompilacji muszą wystąpić następujące akcje:

- Najpierw Kompilacja zespołu powinna kompilować rozwiązanie. W ramach tego procesu Kompilacja zespołu wywoła potok publikowania w sieci Web (WPP) w celu generowania pakietów wdrażania sieci Web dla każdego projektu aplikacji sieci Web w rozwiązaniu. Kompilacja zespołu uruchomi również wszystkie testy jednostkowe skojarzone z rozwiązaniem.
- Jeśli kompilacja rozwiązania nie powiedzie się, Kompilacja zespołu nie powinna podejmować dalszych działań. Niepowodzenia testów jednostkowych powinny być traktowane jako niepowodzenie kompilacji.
- Jeśli kompilacja rozwiązania powiodła się, Kompilacja zespołu powinna uruchomić niestandardowy plik projektu, który kontroluje wdrożenie rozwiązania. W ramach tego procesu Kompilacja zespołu wywoła narzędzie Web Deployment (Web Deploy) Internet Information Services (IIS), aby zainstalować spakowane aplikacje sieci Web na docelowych serwerach sieci Web i wywoływać narzędzie VSDBCMD. exe w celu uruchomienia tworzenia bazy danych skrypty na docelowych serwerach baz danych.

Ilustruje to proces:

![](creating-a-build-definition-that-supports-deployment/_static/image1.png)

Przykładowe rozwiązanie [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) obejmuje plik niestandardowego projektu programu MSBuild, *Publish. proj*, który można uruchomić z poziomu programu MSBuild lub kompilacji zespołowej. Zgodnie z opisem w temacie [proces kompilacji](../web-deployment-in-the-enterprise/understanding-the-build-process.md)ten plik projektu definiuje logikę, która wdraża pakiety i bazy danych sieci Web w środowisku docelowym. Plik zawiera logikę, która pomija proces kompilowania i tworzenia pakietów, jeśli jest uruchomiony w kompilacji zespołowej, pozostawiając tylko zadania wdrażania do uruchomienia. Jest to spowodowane tym, że w przypadku automatyzowania wdrażania w ten sposób zwykle warto upewnić się, że rozwiązanie zostanie pomyślnie skompilowane i przejdzie wszystkie testy jednostkowe przed rozpoczęciem procesu wdrażania.

W następnej sekcji wyjaśniono, jak zaimplementować ten proces, tworząc nową definicję kompilacji.

> [!NOTE]
> Ta procedura&#x2014;, w której pojedynczy zautomatyzowany proces kompiluje, testuje i wdraża rozwiązanie&#x2014;, prawdopodobnie najlepiej nadaje się do wdrożenia w środowiskach testowych. W przypadku środowisk przejściowych i produkcyjnych znacznie bardziej zajdzie potrzeba wdrożenia zawartości z poprzedniej kompilacji, która została już zweryfikowana i sprawdzona w środowisku testowym. Ta metoda jest opisana w następnym temacie, który [wdraża konkretną kompilację](deploying-a-specific-build.md).

### <a name="who-performs-this-procedure"></a>Kto wykonuje tę procedurę?

Zazwyczaj ta procedura jest wykonywana przez administratora serwera TFS. W niektórych przypadkach lider zespołu deweloperów może podjąć odpowiedzialność za kolekcję projektu zespołowego w programie TFS. Aby utworzyć nową definicję kompilacji, musisz być członkiem grupy **administratorzy kompilacji kolekcji projektów** dla kolekcji projektów zespołowych, która zawiera Twoje rozwiązanie.

## <a name="create-a-build-definition-for-ci-and-deployment"></a>Tworzenie definicji kompilacji dla elementu konfiguracji i wdrożenia

W następnej procedurze opisano sposób tworzenia definicji kompilacji, która jest wyzwalana przez wyzwalacze konfiguracji. Jeśli kompilacja zakończy się pomyślnie, rozwiązanie zostanie wdrożone przy użyciu logiki w niestandardowym pliku projektu programu MSBuild.

**Aby utworzyć definicję kompilacji dla CI i wdrożenia**

1. W programie Visual Studio 2010, w oknie **Team Explorer** rozwiń węzeł projektu zespołowego, kliknij prawym przyciskiem myszy pozycję **kompilacje**, a następnie kliknij pozycję **Nowa definicja kompilacji**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image2.png)
2. Na karcie **Ogólne** nadaj definicji kompilacji nazwę (na przykład **DeployToTest**) i opcjonalny opis.
3. Na karcie **wyzwalacz** Wybierz kryteria, dla których chcesz wyzwolić nową kompilację. Jeśli na przykład chcesz skompilować rozwiązanie i wdrożyć je do środowiska testowego za każdym razem, gdy deweloper sprawdzi nowy kod, wybierz pozycję **ciągła integracja**.
4. Na karcie **Ustawienia domyślne kompilacji** w polu **Kopiuj dane wyjściowe kompilacji do poniższego folderu drop** wpisz ścieżkę Universal Naming Convention (UNC) folderu docelowego (na przykład **\\TFSBUILD\Drops**).

    ![](creating-a-build-definition-that-supports-deployment/_static/image3.png)

    > [!NOTE]
    > Ta lokalizacja docelowa przechowuje kilka kompilacji, w zależności od skonfigurowanych zasad przechowywania. Jeśli chcesz opublikować artefakty wdrożenia z określonej kompilacji w środowisku przejściowym lub produkcyjnym, możesz je znaleźć.
5. Na karcie **proces** na liście rozwijanej **plik procesu kompilacji** pozostaw wybraną opcję **DefaultTemplate. XAML** . Jest to jeden z domyślnych szablonów procesu kompilacji, które zostaną dodane do wszystkich nowych projektów zespołowych.
6. W tabeli **parametry procesu kompilacji** kliknij w wierszu **elementy do skompilowania** , a następnie kliknij przycisk **wielokropka** .

    ![](creating-a-build-definition-that-supports-deployment/_static/image4.png)
7. W oknie dialogowym **elementy do skompilowania** kliknij pozycję **Dodaj**.
8. Przejdź do lokalizacji pliku rozwiązania, a następnie kliknij przycisk **OK**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image5.png)
9. W oknie dialogowym **elementy do skompilowania** kliknij pozycję **Dodaj**.
10. Z listy rozwijanej **elementy typu** wybierz pozycję **pliki projektu programu MSBuild**.
11. Przejdź do lokalizacji niestandardowego pliku projektu, za pomocą którego kontrolujesz proces wdrażania, wybierz plik, a następnie kliknij przycisk **OK**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image6.png)
12. Okno dialogowe **elementy do skompilowania** powinno teraz pokazać dwa elementy. Kliknij przycisk **OK**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image7.png)
13. Na karcie **proces** w tabeli **parametry procesu kompilacji** rozwiń sekcję **Zaawansowane** .
14. W wierszu **argumenty programu MSBuild** Dodaj dowolne argumenty wiersza polecenia programu MSBuild, które są wymagane przez *każdy* z elementów do skompilowania. W scenariuszu rozwiązania Contact Manager wymagane są następujące argumenty:

    [!code-console[Main](creating-a-build-definition-that-supports-deployment/samples/sample1.cmd)]

    ![](creating-a-build-definition-that-supports-deployment/_static/image8.png)
15. W tym przykładzie:

    1. Argumenty **DeployOnBuild = true** i **DeployTarget = Package** są wymagane podczas tworzenia rozwiązania Contact Manager. Powoduje to utworzenie przez program MSBuild pakietów wdrażania sieci Web po skompilowaniu każdego projektu aplikacji sieci Web, zgodnie z opisem w temacie [Tworzenie i pakowanie projektów aplikacji sieci Web](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md).
    2. Argument **TargetEnvPropsFile** jest wymagany podczas kompilowania pliku *Publish. proj* . Ta właściwość wskazuje lokalizację pliku konfiguracji specyficznego dla środowiska, zgodnie z opisem w temacie [Opis procesu kompilacji](../web-deployment-in-the-enterprise/understanding-the-build-process.md).
16. Na karcie **zasady przechowywania** Skonfiguruj liczbę kompilacji poszczególnych typów, które mają być zachowane zgodnie z potrzebami.
17. Kliknij przycisk **Save** (Zapisz).

## <a name="queue-a-build"></a>Kolejkowanie kompilacji

W tym momencie utworzono co najmniej jedną nową definicję kompilacji. Zdefiniowany przez użytkownika proces kompilacji będzie teraz uruchamiany zgodnie z wyzwalaczami określonymi w definicji kompilacji.

Jeśli definicja kompilacji została skonfigurowana tak, aby korzystała z CI, można testować definicję kompilacji na dwa sposoby:

- Zaewidencjonuj część zawartości w projekcie zespołowym, aby wyzwolić automatyczną kompilację.
- Ręcznie Twórz kompilację do kolejki.

**Aby ręcznie umieścić kompilację w kolejce**

1. W oknie **Team Explorer** kliknij prawym przyciskiem myszy definicję kompilacji, a następnie kliknij pozycję **kolejka nowa kompilacja**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image9.png)
2. W oknie dialogowym **kompilacja kolejki** Przejrzyj właściwości kompilacji, a następnie kliknij pozycję **Kolejka**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image10.png)

Aby sprawdzić postęp i wynik kompilacji&#x2014;, niezależnie od tego, czy została ona wyzwolona ręcznie, czy automatycznie&#x2014;dwukrotnie kliknij definicję kompilacji w oknie **Team Explorer** . Spowoduje to otwarcie karty programu **Build Explorer** .

![](creating-a-build-definition-that-supports-deployment/_static/image11.png)

W tym miejscu możesz rozwiązywać problemy z kompilacjami zakończonymi niepowodzeniem. W przypadku dwukrotnego kliknięcia pojedynczej kompilacji można wyświetlić informacje podsumowujące i kliknąć pozycję, aby uzyskać szczegółowe pliki dziennika.

![](creating-a-build-definition-that-supports-deployment/_static/image12.png)

Tych informacji można użyć do rozwiązywania problemów z kompilacjami zakończonymi niepowodzeniem i rozwiązanie wszelkich problemów przed próbą kolejnej kompilacji.

> [!NOTE]
> Kompiluje, że logika wdrożenia wykonawczego może zakończyć się niepowodzeniem do momentu udzielenia serwerowi kompilacji wszelkich uprawnień wymaganych w środowisku docelowym. Aby uzyskać więcej informacji, zobacz [Konfigurowanie uprawnień do wdrożenia kompilacji zespołowej](configuring-permissions-for-team-build-deployment.md).

## <a name="monitor-the-build-process"></a>Monitorowanie procesu kompilacji

TFS oferuje szeroką gamę funkcji, które ułatwiają monitorowanie procesu kompilacji. Na przykład TFS może wysłać wiadomość e-mail lub wyświetlić alerty w obszarze powiadomień paska zadań, gdy kompilacja została ukończona. Aby uzyskać więcej informacji, zobacz [Uruchamianie i monitorowanie kompilacji](https://msdn.microsoft.com/library/ms181721.aspx).

## <a name="conclusion"></a>Podsumowanie

W tym temacie opisano sposób tworzenia definicji kompilacji w programie TFS. Definicja kompilacji jest skonfigurowana dla elementu konfiguracji, więc proces kompilacji jest uruchamiany za każdym razem, gdy deweloper sprawdzi zawartość w projekcie zespołowym. Definicja kompilacji wykonuje niestandardowy plik projektu MSBuild do wdrażania pakietów sieci Web i skryptów bazy danych w środowisku serwera docelowego.

Aby zautomatyzowane wdrożenie powiodło się w ramach procesu kompilacji, należy przyznać odpowiednie uprawnienia do konta usługi kompilacji na docelowych serwerach sieci Web i docelowym serwerze bazy danych. Ostatni temat w tym samouczku, [Konfigurowanie uprawnień do wdrożenia kompilacji zespołowej](configuring-permissions-for-team-build-deployment.md), opisuje sposób identyfikowania i konfigurowania uprawnień wymaganych do automatycznego wdrażania z serwera kompilacji zespołu.

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat tworzenia definicji kompilacji, zobacz [Tworzenie podstawowej definicji kompilacji](https://msdn.microsoft.com/library/ms181716.aspx) i [Definiowanie procesu kompilacji](https://msdn.microsoft.com/library/ms181715.aspx). Aby uzyskać więcej wskazówek dotyczących kompilacji w kolejkach, zobacz [Queue a Build](https://msdn.microsoft.com/library/ms181722.aspx).

> [!div class="step-by-step"]
> [Poprzednie](configuring-a-tfs-build-server-for-web-deployment.md)
> [dalej](deploying-a-specific-build.md)

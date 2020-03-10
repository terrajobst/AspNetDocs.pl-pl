---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
title: Tworzenie projektu zespołowego w programie TFS | Microsoft Docs
author: jrjlee
description: W tym temacie opisano sposób tworzenia nowego projektu zespołowego w Team Foundation Server (TFS) 2010.
ms.author: riande
ms.date: 05/04/2012
ms.assetid: b28d3e2d-0bb4-4e29-a780-af810b964722
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
msc.type: authoredcontent
ms.openlocfilehash: d12e0636ce3b6239d125305db4354278f9f24960
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78639658"
---
# <a name="creating-a-team-project-in-tfs"></a>Tworzenie projektu zespołowego na serwerze TFS

Autor [Jason Lewandowski](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie opisano sposób tworzenia nowego projektu zespołowego w Team Foundation Server (TFS) 2010.

Ten temat stanowi część szeregu samouczków opartych na wymaganiach dotyczących wdrażania w przedsiębiorstwie fikcyjnej firmy o nazwie Fabrikam, Inc. W tej serii samouczków jest stosowane&#x2014;przykładowe rozwiązanie&#x2014;do reprezentowania aplikacji sieci Web, [które ma](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)realistyczny poziom złożoności, w tym aplikacji ASP.NET MVC 3, usługi Windows Communication Foundation (WCF) i projektu bazy danych.

## <a name="task-overview"></a>Przegląd zadania

Aby zainicjować obsługę administracyjną nowego projektu zespołowego i korzystać z niego w programie TFS, należy wykonać następujące czynności wysokiego poziomu:

- Udziel uprawnień użytkownikowi, który utworzy nowy projekt zespołowy.
- Utwórz projekt zespołowy.
- Udziel uprawnień członkom zespołu, którzy będą współpracować nad projektem.
- Zaewidencjonuj zawartość.

W tym temacie pokazano, jak wykonać te procedury i zidentyfikować użytkowników i role zadań, które mogą być odpowiedzialne za każdą procedurę. Należy pamiętać, że w zależności od struktury organizacji każde z tych zadań może być obowiązkiem innej osoby.

W zadaniach i przewodnikach w tym temacie przyjęto założenie, że zainstalowano i skonfigurowano program TFS oraz że kolekcja projektów zespołowych została utworzona w ramach procesu konfiguracji. Aby uzyskać więcej informacji na temat tych założeń i bardziej ogólnych informacji w tle dotyczących scenariusza, zobacz [Configure a TFS Build Server for Web Deployment](configuring-a-tfs-build-server-for-web-deployment.md).

## <a name="grant-permissions-to-the-team-project-creator"></a>Udziel uprawnień do twórcy projektu zespołowego

Aby można było utworzyć nowy projekt zespołowy, potrzebne są następujące uprawnienia:

- Musisz mieć uprawnienie **Tworzenie nowych projektów** w warstwie aplikacji TFS. To uprawnienie jest zazwyczaj przyznawane przez dodanie użytkowników do grupy TFS **Administratorzy kolekcji projektów** . Grupa globalna **administratorów Team Foundation** obejmuje również to uprawnienie.
- Musisz mieć uprawnienia do tworzenia nowych witryn zespołu w kolekcji witryn programu SharePoint, która odpowiada kolekcji projektów zespołowych TFS. To uprawnienie jest zazwyczaj udzielane przez dodanie użytkownika do grupy programu SharePoint z uprawnieniami **pełna kontrola** w kolekcji witryn programu SharePoint.
- Jeśli używasz funkcji SQL Server Reporting Services, musisz być członkiem roli **Menedżer zawartości Team Foundation** w usługach Reporting Services.

### <a name="who-performs-these-procedures"></a>Kto wykonuje te procedury?

Zazwyczaj te procedury są wykonywane przez osobę lub grupę, która administruje wdrożeniem programu TFS.

Ponieważ jest to wysoce uprzywilejowany zestaw uprawnień, nowe projekty zespołowe są zwykle tworzone przez niewielki podzbiór użytkowników odpowiedzialnych za administrowanie wdrożeniem programu TFS. Deweloperzy nie będą zwykle przyznawać uprawnień wymaganych do tworzenia nowych projektów zespołowych.

### <a name="grant-permissions-in-tfs"></a>Przyznawanie uprawnień w programie TFS

Jeśli chcesz umożliwić użytkownikowi tworzenie nowych projektów zespołowych, pierwsze zadanie wysokiego poziomu polega na dodaniu użytkownika do grupy **Administratorzy kolekcji projektów** dla kolekcji projektów zespołowych.

**Aby dodać użytkownika do grupy administratorów kolekcji projektów**

1. Na serwerze TFS, w menu **Start** wskaż polecenie **Wszystkie programy**, kliknij pozycję **Microsoft Team Foundation Server 2010**, a następnie kliknij pozycję **Konsola administracyjna serwera Team Foundation**.
2. W widoku drzewa nawigacji rozwiń węzeł **warstwa aplikacji**, a następnie kliknij pozycję **kolekcje projektu zespołowego**.

    ![](creating-a-team-project-in-tfs/_static/image1.png)
3. W okienku **kolekcje projektu zespołowego** wybierz kolekcję projektu zespołowego, którą chcesz zarządzać.

    ![](creating-a-team-project-in-tfs/_static/image2.png)
4. Na karcie **Ogólne** kliknij opcję **członkostwo w grupie**.

    ![](creating-a-team-project-in-tfs/_static/image3.png)
5. W oknie dialogowym **grupy globalne** wybierz grupę **Administratorzy kolekcji projektu** , a następnie kliknij przycisk **Właściwości**.
6. W oknie dialogowym **Właściwości grupy Team Foundation Server** wybierz pozycję **użytkownik lub Grupa systemu Windows**, a następnie kliknij przycisk **Dodaj**.

    ![](creating-a-team-project-in-tfs/_static/image4.png)
7. W oknie dialogowym **Wybieranie użytkowników, komputerów lub grup** wpisz nazwę użytkownika, dla którego chcesz mieć możliwość tworzenia nowych projektów zespołowych, kliknij przycisk **Sprawdź nazwy**, a następnie kliknij przycisk **OK**.

    ![](creating-a-team-project-in-tfs/_static/image5.png)
8. W oknie dialogowym **Właściwości grupy Team Foundation Server** kliknij przycisk **OK**.
9. W oknie dialogowym **grupy globalne** kliknij przycisk **Zamknij**.

### <a name="grant-permissions-in-sharepoint-services"></a>Przyznawanie uprawnień w usługach SharePoint

Następnie należy przyznać użytkownikowi uprawnienia do tworzenia nowych witryn zespołu w kolekcji witryn programu SharePoint, która odpowiada kolekcji projektów zespołowych TFS.

**Aby udzielić uprawnień Pełna kontrola w zbiorze witryn programu SharePoint**

1. W Konsola administracyjna Team Foundation Server na stronie **kolekcje projektu zespołowego** wybierz kolekcję projektu zespołowego, którą chcesz zarządzać.
2. Na karcie **Witryna programu SharePoint** Zanotuj wartość **bieżącego domyślnego adresu URL lokalizacji witryny** .

    ![](creating-a-team-project-in-tfs/_static/image6.png)
3. Otwórz program Internet Explorer, a następnie przejdź do adresu URL zanotowanego w kroku 2.

    > [!NOTE]
    > Jeśli nie jesteś zalogowany do systemu Windows jako użytkownik, który utworzył kolekcję projektu zespołowego, musisz zalogować się do programu SharePoint jako ten użytkownik, aby kontynuować.
4. W menu **Akcje lokacji** kliknij polecenie **Ustawienia lokacji**.

    ![](creating-a-team-project-in-tfs/_static/image7.png)
5. Na stronie **Ustawienia witryny** w obszarze **Użytkownicy i uprawnienia**kliknij pozycję **osoby i grupy**.
6. W okienku nawigacji po lewej stronie kliknij pozycję **grupy**.

    ![](creating-a-team-project-in-tfs/_static/image8.png)
7. Na stronie **osoby i grupy: wszystkie grupy** kliknij pozycję **Skonfiguruj grupy dla tej witryny**.

    ![](creating-a-team-project-in-tfs/_static/image9.png)

   > [!NOTE]
   > Może zostać wyświetlony błąd <strong>HTTP 404 nie znaleziono</strong> z powodu podwójnej usterki kodowania http. Jeśli tak się stanie, Zastąp adres URL:   
   > `[site_collection_URL]/_layouts/permsetup.aspx` na przykład:  
   > `http://tfs/sites/Fabrikam%20Web%20Projects/_layouts/permsetup.aspx` 
8. Na stronie **Konfigurowanie grup dla tej lokacji** Dodaj użytkownika, który będzie tworzyć projekty zespołowe w grupie **właściciele** , a następnie kliknij przycisk **OK**.

    ![](creating-a-team-project-in-tfs/_static/image10.png)

Aby uzyskać więcej informacji na temat umożliwiania użytkownikom tworzenia nowych projektów zespołowych w kolekcji projektów zespołowych, zobacz [Ustawianie uprawnień administratora dla kolekcji projektów zespołowych](https://msdn.microsoft.com/library/dd547204.aspx).

## <a name="create-a-new-team-project-and-add-users"></a>Tworzenie nowego projektu zespołowego i Dodawanie użytkowników

Gdy masz wymagane uprawnienia, możesz użyć okna **Team Explorer** w programie Visual Studio 2010, aby utworzyć nowy projekt zespołowy. To podejście umożliwia kreatorowi gromadzenie wszystkich wymaganych informacji i wykonywanie niezbędnych zadań w programie TFS, SharePoint i SQL Server Reporting Services. Należy również przyznać uprawnienia do nowego projektu zespołowego członkom zespołu deweloperów, aby umożliwić im Dodawanie i modyfikowanie zawartości.

### <a name="who-performs-these-procedures"></a>Kto wykonuje te procedury?

Zwykle administrator programu TFS lub lider zespołu deweloperów wykonuje te procedury.

### <a name="create-a-new-team-project"></a>Utwórz nowy projekt zespołowy

W następnej procedurze opisano sposób tworzenia nowego projektu zespołowego w programie TFS 2010.

**Aby utworzyć nowy projekt zespołowy**

1. W menu **Start** wskaż polecenie **Wszystkie programy**, kliknij pozycję **Microsoft Visual Studio 2010**, kliknij prawym przyciskiem myszy pozycję **Microsoft Visual Studio 2010**, a następnie kliknij polecenie **Uruchom jako administrator**.

    > [!NOTE]
    > Jeśli nie uruchomisz programu Visual Studio 2010 jako administrator, Kreator nowego projektu zespołowego zakończy się niepowodzeniem w ostatnim kroku.
2. Jeśli zostanie wyświetlone okno dialogowe **Kontrola konta użytkownika** , kliknij przycisk **tak**.
3. W programie Visual Studio w menu **zespół** kliknij polecenie **Połącz z Team Foundation Server**.

    > [!NOTE]
    > Jeśli połączenie z serwerem TFS zostało już skonfigurowane, możesz pominąć kroki 4-7.
4. W oknie dialogowym **połączenie z projektem zespołowym** kliknij pozycję **serwery**.
5. W oknie dialogowym **Dodawanie/usuwanie Team Foundation Server** kliknij przycisk **Dodaj**.
6. W oknie dialogowym **dodawanie Team Foundation Server** Podaj szczegóły wystąpienia programu TFS, a następnie kliknij przycisk **OK**.

    ![](creating-a-team-project-in-tfs/_static/image11.png)
7. W oknie dialogowym **Dodawanie/usuwanie Team Foundation Server** kliknij przycisk **Zamknij**.
8. W oknie dialogowym **Połącz z projektem zespołowym** wybierz wystąpienie TFS, z którym chcesz nawiązać połączenie, wybierz kolekcję projektu zespołowego, do której chcesz dodać, a następnie kliknij przycisk **Połącz**.

    ![](creating-a-team-project-in-tfs/_static/image12.png)
9. W oknie **Team Explorer** kliknij prawym przyciskiem myszy kolekcję projektu zespołowego, a następnie kliknij pozycję **Nowy projekt zespołowy**.

    ![](creating-a-team-project-in-tfs/_static/image13.png)
10. W oknie dialogowym **Nowy projekt zespołowy** Podaj nazwę i opis projektu zespołowego, a następnie kliknij przycisk **dalej**.

    > [!NOTE]
    > Jeśli projekt zespołowy zawiera spacje, podczas korzystania z narzędzia do wdrażania w sieci Web (Web Deploy) Internet Information Services (IIS) w celu wdrożenia pakietów z ścieżki wyjściowej mogą wystąpić pewne problemy. Spacje w ścieżce mogą znacznie trudniejsze uruchamiać Web Deploy polecenia.

    ![](creating-a-team-project-in-tfs/_static/image14.png)
11. Na stronie **Wybierz szablon procesu** wybierz szablon procesu, który ma być używany do zarządzania procesem opracowywania, a następnie kliknij przycisk **dalej**.

    > [!NOTE]
    > Aby uzyskać więcej informacji na temat szablonów procesów dla TFS, zobacz [Szablony procesów i narzędzia](https://msdn.microsoft.com/vstudio/aa718795).
12. Na stronie **Ustawienia witryny zespołu** Pozostaw ustawienia domyślne bez zmian, a następnie kliknij przycisk **dalej**.
13. To ustawienie służy do tworzenia lub identyfikowania witryny zespołu programu SharePoint, która jest skojarzona z projektem zespołowym TFS. Zespół programistyczny może używać tej witryny do zarządzania dokumentacją, uczestniczenia w wątkach dyskusji, tworzenia stron typu wiki i wykonywania różnych zadań, które nie są związane z kodem. Aby uzyskać więcej informacji, zobacz [interakcje między produktami programu SharePoint i Team Foundation Server](https://msdn.microsoft.com/library/ms253177.aspx).
14. Na stronie **Określanie ustawień kontroli źródła** Pozostaw ustawienia domyślne bez zmian, a następnie kliknij przycisk **dalej**.
15. To ustawienie służy do identyfikowania lub tworzenia lokalizacji w hierarchii folderów TFS, która będzie pełnić rolę folderu głównego zawartości.
16. Na stronie **Potwierdzanie ustawień projektu zespołowego** kliknij przycisk **Zakończ**.
17. Po pomyślnym utworzeniu nowego projektu zespołowego na stronie **projekt zespołowy** kliknij przycisk **Zamknij**.

### <a name="add-users-to-a-team-project"></a>Dodawanie użytkowników do projektu zespołowego

Po utworzeniu nowego projektu zespołowego możesz udzielić uprawnień użytkownikom, aby mogli rozpocząć dodawanie i współpracę nad zawartością.

**Aby dodać użytkowników do projektu zespołowego**

1. W programie Visual Studio 2010, w oknie **Team Explorer** kliknij prawym przyciskiem myszy projekt zespołowy, wskaż polecenie **Ustawienia projektu zespołowego**, a następnie kliknij pozycję członkostwo w **grupie**.

    ![](creating-a-team-project-in-tfs/_static/image15.png)
2. Aby umożliwić użytkownikowi dodawanie, modyfikowanie i usuwanie kodu pod kontrolą źródła, należy dodać go do grupy **współautorów** .
3. W oknie dialogowym **grupy projektu** wybierz grupę **współautorów** , a następnie kliknij przycisk **Właściwości**.

    ![](creating-a-team-project-in-tfs/_static/image16.png)
4. W oknie dialogowym **Właściwości grupy Team Foundation Server** wybierz pozycję **użytkownik lub Grupa systemu Windows**, a następnie kliknij przycisk **Dodaj**.

    ![](creating-a-team-project-in-tfs/_static/image17.png)
5. W oknie dialogowym **Wybieranie użytkowników, komputerów lub grup** wpisz nazwę użytkownika, który chcesz dodać do projektu zespołowego, kliknij przycisk **Sprawdź nazwy**, a następnie kliknij przycisk **OK**.

    ![](creating-a-team-project-in-tfs/_static/image18.png)
6. W oknie dialogowym **Właściwości grupy Team Foundation Server** kliknij przycisk **OK**.
7. W oknie dialogowym **grupy projektu** kliknij przycisk **Zamknij**.

## <a name="conclusion"></a>Podsumowanie

W tym momencie nowy projekt zespołowy jest gotowy do użycia, a zespół deweloperów może rozpocząć dodawanie zawartości i współpracę w procesie opracowywania.

Następny temat, [Dodawanie zawartości do kontroli źródła](adding-content-to-source-control.md), zawiera opis sposobu dodawania zawartości do kontroli źródła.

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać szersze wskazówki dotyczące tworzenia projektów zespołowych w programie TFS, zobacz [Tworzenie projektu zespołowego](https://msdn.microsoft.com/library/ms181477(v=VS.100).aspx). Aby uzyskać więcej informacji na temat umożliwiania użytkownikom tworzenia nowych projektów zespołowych w kolekcji projektów zespołowych, zobacz [Ustawianie uprawnień administratora dla kolekcji projektów zespołowych](https://msdn.microsoft.com/library/dd547204.aspx). Aby uzyskać więcej informacji na temat dodawania użytkowników do projektów zespołowych, zobacz [Dodawanie użytkowników do projektów zespołowych](https://msdn.microsoft.com/library/bb558971.aspx).

> [!div class="step-by-step"]
> [Poprzednie](configuring-team-foundation-server-for-web-deployment.md)
> [dalej](adding-content-to-source-control.md)

---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
title: Dodawanie zawartości do kontroli źródła | Microsoft Docs
author: jrjlee
description: W tym temacie wyjaśniono, jak dodać zawartość do kontroli źródła w Team Foundation Server (TFS) 2010. Opisuje sposób dodawania rozwiązań i projektów do zespołu projektu...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 86c14aab-c2dd-4f73-b40c-c6d52fa44950
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
msc.type: authoredcontent
ms.openlocfilehash: 16073dd2fb0ea1cc4ddbc94c843181933dc174c1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78634464"
---
# <a name="adding-content-to-source-control"></a>Dodawanie zawartości do kontroli źródła

Autor [Jason Lewandowski](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie wyjaśniono, jak dodać zawartość do kontroli źródła w Team Foundation Server (TFS) 2010. Opisano w nim sposób dodawania rozwiązań i projektów do projektu zespołowego w programie TFS i wyjaśniono, jak dodać zależności zewnętrzne, takie jak struktury lub zestawy, do kontroli źródła.

Ten temat stanowi część szeregu samouczków opartych na wymaganiach dotyczących wdrażania w przedsiębiorstwie fikcyjnej firmy o nazwie Fabrikam, Inc. W tej serii samouczków jest stosowane&#x2014;przykładowe rozwiązanie&#x2014;do reprezentowania aplikacji sieci Web, [które ma](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)realistyczny poziom złożoności, w tym aplikacji ASP.NET MVC 3, usługi Windows Communication Foundation (WCF) i projektu bazy danych.

## <a name="task-overview"></a>Przegląd zadania

W większości przypadków każdy członek zespołu deweloperów powinien mieć możliwość dodawania zawartości do kontroli źródła. Aby dodać rozwiązanie do kontroli źródła w programie TFS, należy wykonać następujące czynności wysokiego poziomu:

- Połącz z projektem zespołowym.
- Mapuj strukturę folderów projektu zespołowego na serwerze do struktury folderów na komputerze lokalnym.
- Dodaj rozwiązanie i jego zawartość do kontroli źródła.
- Dodaj wszystkie zależności zewnętrzne do kontroli źródła.

W tym temacie pokazano, jak wykonać te procedury.

W zadaniach i instruktażach w tym temacie przyjęto założenie, że nowy projekt zespołowy TFS został już utworzony w celu zarządzania zawartością. Aby uzyskać więcej informacji na temat tworzenia nowego projektu zespołowego, zobacz [Tworzenie projektu zespołowego w programie TFS](creating-a-team-project-in-tfs.md).

### <a name="who-performs-these-procedures"></a>Kto wykonuje te procedury?

W większości przypadków każdy członek zespołu deweloperów powinien mieć możliwość dodawania i modyfikowania zawartości w określonych projektach zespołowych.

## <a name="connect-to-a-team-project-and-create-a-folder-mapping"></a>Nawiązywanie połączenia z projektem zespołowym i Tworzenie mapowania folderów

Przed dodaniem jakiejkolwiek zawartości do kontroli źródła należy połączyć się z projektem zespołowym i utworzyć mapowanie między strukturą folderów na serwerze i systemem plików na komputerze lokalnym.

**Aby połączyć się z projektem zespołowym i zmapować ścieżkę lokalną**

1. Na stacji roboczej dewelopera Otwórz program Visual Studio 2010.
2. W programie Visual Studio w menu **zespół** kliknij polecenie **Połącz z Team Foundation Server**.

    > [!NOTE]
    > Jeśli połączenie z serwerem TFS zostało już skonfigurowane, możesz pominąć kroki 3-6.
3. W oknie dialogowym **połączenie z projektem zespołowym** kliknij pozycję **serwery**.
4. W oknie dialogowym **Dodawanie/usuwanie Team Foundation Server** kliknij przycisk **Dodaj**.
5. W oknie dialogowym **dodawanie Team Foundation Server** Podaj szczegóły wystąpienia programu TFS, a następnie kliknij przycisk **OK**.

    ![](adding-content-to-source-control/_static/image1.png)
6. W oknie dialogowym **Dodawanie/usuwanie Team Foundation Server** kliknij przycisk **Zamknij**.
7. W oknie dialogowym **Połącz z projektem zespołowym** wybierz wystąpienie TFS, z którym chcesz nawiązać połączenie, wybierz kolekcję projektu zespołowego, wybierz projekt zespołowy, do którego chcesz dodać, a następnie kliknij przycisk **Połącz**.

    ![](adding-content-to-source-control/_static/image2.png)
8. W oknie **Team Explorer** rozwiń projekt zespołowy, a następnie kliknij dwukrotnie pozycję **Kontrola źródła**.

    ![](adding-content-to-source-control/_static/image3.png)
9. Na karcie **Eksploator kontroli źródła** kliknij pozycję **niezamapowane**.

    ![](adding-content-to-source-control/_static/image4.png)
10. W oknie dialogowym **Mapa** w polu **folder lokalny** przejdź do (lub Utwórz) folder lokalny do działania jako folder główny projektu zespołowego, a następnie kliknij pozycję **Mapuj**.

    ![](adding-content-to-source-control/_static/image5.png)
11. Po wyświetleniu monitu o pobranie plików źródłowych kliknij przycisk **tak**.

    ![](adding-content-to-source-control/_static/image6.png)

Na tym etapie został zmapowany folder po stronie serwera dla projektu zespołowego do folderu lokalnego na stacji roboczej dewelopera. Pobrano również istniejącą zawartość z projektu zespołowego do lokalnej struktury folderów. Teraz możesz zacząć dodawać własną zawartość do kontroli źródła.

## <a name="add-projects-and-solutions-to-source-control"></a>Dodaj projekty i rozwiązania do kontroli źródła

Aby dodać projekty i rozwiązania do kontroli źródła, należy najpierw przenieść je do folderu mapowanego dla projektu zespołowego na komputerze lokalnym. Następnie możesz zaewidencjonować zawartość, aby zsynchronizować dodatki z serwerem.

**Aby dodać projekty do kontroli źródła**

1. Na stacji roboczej dewelopera Przenieś swoje projekty i rozwiązania do odpowiedniej lokalizacji w mapowanej strukturze folderów dla projektu zespołowego.

    > [!NOTE]
    > Wiele organizacji będzie mieć preferowane podejście do organizowania projektów i rozwiązań w kontroli źródła. Aby uzyskać wskazówki dotyczące struktury folderów, zobacz [jak: strukturę folderów kontroli źródła w Team Foundation Server](https://msdn.microsoft.com/library/bb668992.aspx).
2. Otwórz rozwiązanie w programie Visual Studio 2010.
3. W oknie **Eksplorator rozwiązań** kliknij prawym przyciskiem myszy rozwiązanie, a następnie kliknij polecenie **Dodaj rozwiązanie do kontroli źródła**.

    ![](adding-content-to-source-control/_static/image7.png)

    > [!NOTE]
    > W niektórych przypadkach, w zależności od tego, jak Twoja organizacja lubi zawartość struktury w programie TFS, może być konieczne dodanie projektów do kontroli źródła pojedynczo, aby zapewnić bardziej precyzyjną kontrolę nad sposobem organizowania kodu źródłowego.
4. Sprawdź, czy karta **Eksploator kontroli źródła** wyświetla zawartość dodaną w strukturze folderów serwera dla projektu zespołowego.

    ![](adding-content-to-source-control/_static/image8.png)

    > [!NOTE]
    > Karta **Eksploator kontroli źródła** wyświetla zawartość bez dalszych monitów, ponieważ dodano rozwiązanie do zamapowanego folderu w lokalnym systemie plików. Jeśli Twoje rozwiązanie znajduje się w niemapowanej lokalizacji, zostanie wyświetlony monit o określenie lokalizacji folderów zarówno w programie TFS, jak i w lokalnym systemie plików.
5. Na karcie **Eksploator kontroli źródła** w okienku **foldery** kliknij prawym przyciskiem myszy projekt zespołowy (na przykład **ContactManager**), a następnie kliknij polecenie **Zaewidencjonuj oczekujące zmiany**.
6. W oknie dialogowym **zaewidencjonuj — pliki źródłowe** wpisz komentarz, a następnie kliknij przycisk **Zaewidencjonuj**.

    ![](adding-content-to-source-control/_static/image9.png)

W tym momencie dodaliśmy Twoje rozwiązanie do kontroli źródła w programie TFS.

## <a name="add-external-dependencies-to-source-control"></a>Dodaj zależności zewnętrzne do kontroli źródła

Dodanie projektu lub rozwiązania do kontroli źródła spowoduje również dodanie wszystkich plików i folderów w projekcie lub rozwiązaniu. Jednak w wielu przypadkach projekty i rozwiązania opierają się również na zależnościach zewnętrznych, takich jak zestawy lokalne, aby działać prawidłowo. Należy dodać wszystkie takie zasoby do kontroli źródła, aby umożliwić kompilację zespołową i innych członków zespołu deweloperów pomyślnie skompilowane kod.

Przykładowo struktura folderów dla przykładowego rozwiązania Contact Manager zawiera folder o nazwie Packages. Zawiera zestaw i różne zasoby pomocnicze dla ADO.NET Entity Framework 4,1. Folder Packages nie jest częścią rozwiązania Contact Manager, ale rozwiązanie nie zostanie pomyślnie skompilowane. Aby umożliwić kompilację zespołu w celu skompilowania rozwiązania, należy dodać folder Packages do kontroli źródła.

> [!NOTE]
> Dołączenie folderu Packages jest typowe w przypadku dodania Entity Framework lub podobnych zasobów do rozwiązania przy użyciu rozszerzenia NuGet dla programu Visual Studio 2010.

**Aby dodać zawartość spoza projektu do kontroli źródła**

1. Upewnij się, że elementy, które mają zostać dodane (na przykład folder Packages) znajdują się w odpowiedniej lokalizacji w zamapowanym folderze w lokalnym systemie plików.
2. W programie Visual Studio 2010, w oknie **Team Explorer** rozwiń projekt zespołowy, a następnie kliknij dwukrotnie pozycję **Kontrola źródła**.

    ![](adding-content-to-source-control/_static/image10.png)
3. Na karcie **Eksploator kontroli źródła** w okienku **foldery** wybierz folder zawierający element lub elementy, które chcesz dodać.
4. Kliknij przycisk **Dodaj elementy do folderu** .

    ![](adding-content-to-source-control/_static/image11.png)
5. W oknie dialogowym **Dodaj do kontroli źródła** zaznacz folder lub elementy, które chcesz dodać, a następnie kliknij przycisk **dalej**.

    ![](adding-content-to-source-control/_static/image12.png)
6. Na karcie **wykluczone elementy** wybierz wszystkie wymagane elementy, które zostały automatycznie wykluczone (na przykład zestawy), a następnie kliknij pozycję **Dołącz element (y)** .

    ![](adding-content-to-source-control/_static/image13.png)
7. Na karcie **elementy do dodania** Sprawdź, czy wszystkie pliki, które mają zostać uwzględnione, znajdują się na liście, a następnie kliknij przycisk **Zakończ**.

    ![](adding-content-to-source-control/_static/image14.png)
8. W oknie **Eksploator kontroli źródła** kliknij przycisk **Zaewidencjonuj** .

    ![](adding-content-to-source-control/_static/image15.png)
9. W oknie dialogowym **zaewidencjonuj — pliki źródłowe** wpisz komentarz, a następnie kliknij przycisk **Zaewidencjonuj**.

W tym momencie dodano zależności zewnętrzne dla rozwiązania do kontroli źródła.

## <a name="conclusion"></a>Podsumowanie

W tym temacie opisano sposób nawiązywania połączenia z projektem zespołowym, mapowania struktury folderów i dodawania zawartości do kontroli źródła. Aby uzyskać więcej informacji na temat pracy z elementami w obszarze kontroli źródła, zobacz [Używanie kontroli wersji](https://msdn.microsoft.com/library/ms181368.aspx).

W następnym temacie [Konfigurowanie serwera kompilacji TFS na potrzeby wdrażania w sieci Web](configuring-a-tfs-build-server-for-web-deployment.md)zawiera opis sposobu przygotowania serwera kompilacji zespołu TFS do kompilowania i wdrażania rozwiązania.

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać bardziej szczegółowe informacje na temat pracy z kontrolą źródła w programie TFS, zobacz [Korzystanie z kontroli wersji](https://msdn.microsoft.com/library/ms181368.aspx).

> [!div class="step-by-step"]
> [Poprzednie](creating-a-team-project-in-tfs.md)
> [dalej](configuring-a-tfs-build-server-for-web-deployment.md)

---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12
title: 'Wdrażanie SQL Server Compact aplikacji sieci Web ASP.NET za pomocą programu Visual Studio lub Visual Web Developer: wdrażanie aktualizacji zawierającej tylko kod-8 z 12 | Microsoft Docs'
author: tdykstra
description: W tej serii samouczków pokazano, jak wdrożyć (opublikować) projekt aplikacji sieci Web ASP.NET, który zawiera bazę danych SQL Server Compact przy użyciu stu Visual...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: ddf6252f-9413-4c0c-a360-2cef8d231717
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12
msc.type: authoredcontent
ms.openlocfilehash: e4d094ef84a747c36ce05ddb0e3d1ce0391d5605
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74572802"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-code-only-update---8-of-12"></a>Wdrażanie SQL Server Compact aplikacji sieci Web ASP.NET za pomocą programu Visual Studio lub Visual Web Developer: wdrażanie aktualizacji zawierającej tylko kod-8 z 12

Autor [Dykstra](https://github.com/tdykstra)

[Pobierz projekt początkowy](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> W tej serii samouczków pokazano, jak wdrożyć (opublikować) projekt aplikacji sieci Web ASP.NET, który zawiera bazę danych SQL Server Compact przy użyciu programu Visual Studio 2012 RC lub Visual Studio Express 2012 RC dla sieci Web. Możesz również użyć programu Visual Studio 2010, jeśli zostanie zainstalowana aktualizacja publikacji w sieci Web. Aby zapoznać się z wprowadzeniem do serii, zobacz [pierwszy samouczek w serii](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Aby zapoznać się z samouczkiem zawierającym funkcje wdrażania wprowadzone po wydaniu wersji RC programu Visual Studio 2012, przedstawiono sposób wdrażania wersji SQL Server innych niż SQL Server Compact i przedstawiono sposób wdrażania programu w Azure App Service Web Apps, zobacz [ASP.NET Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).

## <a name="overview"></a>Omówienie

Po wdrożeniu wstępnym można nadal utrzymywać i opracowywać swoją witrynę sieci Web oraz przed długim wdrożeniem aktualizacji. Ten samouczek przeprowadzi Cię przez proces wdrażania aktualizacji w kodzie aplikacji. Ta aktualizacja nie obejmuje zmiany bazy danych; w następnym samouczku znajdziesz informacje o sposobie wdrażania zmian w bazie danych.

Przypomnienie: Jeśli zostanie wyświetlony komunikat o błędzie lub coś nie działa, gdy przejdziesz do samouczka, pamiętaj o sprawdzeniu [strony rozwiązywania problemów](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="making-a-code-change"></a>Wprowadzanie zmian w kodzie

Jako prosty przykład aktualizacji aplikacji, dodasz do strony **instruktorów** listę kursów według wybranego instruktora.

Jeśli uruchomisz stronę **instruktorów** , zobaczysz, że w siatce znajdują **się linki,** ale nie wykonują żadnych innych czynności niż sprawia, że tło wierszy jest szare.

[![Instructors_page](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image1.png)

Teraz dodasz kod, który jest uruchamiany, gdy zostanie kliknięty link **Wybierz** i zostanie wyświetlona lista szkoleń szkoleniowych według wybranego instruktora.

W oknie *instruktors. aspx*Dodaj następujące znaczniki bezpośrednio po kontrolce `Label` **ErrorMessageLabel** :

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/samples/sample1.aspx)]

Uruchom stronę i wybierz instruktora. Zostanie wyświetlona lista szkoleń szkoleniowych według tego instruktora.

[![Instructors_page_with_courses](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image3.png)

## <a name="deploying-the-code-update-to-the-test-environment"></a>Wdrażanie aktualizacji kodu w środowisku testowym

Wdrażanie w środowisku testowym jest prostą kwestią ponownego uruchamiania jednego kliknięcia. Aby ten proces był szybszy, możesz skorzystać z paska narzędzi **Publikowanie w sieci Web** .

W menu **Widok** wybierz **paski narzędzi** , a następnie wybierz pozycję **Sieć Web jeden kliknij pozycję Publikuj**.

![Selecting_One_Click_Publish_toolbar](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image5.png)

W **Eksplorator rozwiązań**wybierz projekt ContosoUniversity.

na pasku narzędzi **Publikowanie w sieci Web** , wybierz profil publikacji **test** , a następnie kliknij pozycję **Publikuj sieć Web** (ikona z strzałkami wskazującymi w lewo i w prawo).

![Web_One_Click_Publish_toolbar](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image6.png)

Program Visual Studio wdraża zaktualizowaną aplikację, a przeglądarka automatycznie otwiera stronę główną. Uruchom stronę Instruktorzy i wybierz instruktora, aby sprawdzić, czy aktualizacja została pomyślnie wdrożona.

[![Instructors_page_with_courses_Test](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image7.png)

Zwykle można także przeprowadzić testy regresji (to oznacza pozostałą część lokacji, aby upewnić się, że Nowa zmiana nie przerywa żadnej istniejącej funkcjonalności). Jednak w tym samouczku pominiesz ten krok i przejdziesz do wdrożenia aktualizacji w środowisku produkcyjnym.

## <a name="preventing-redeployment-of-the-initial-database-state-to-production"></a>Uniemożliwianie ponownego wdrożenia stanu początkowej bazy danych w środowisku produkcyjnym

W rzeczywistej aplikacji użytkownicy pracują z witryną produkcyjną po początkowym wdrożeniu, a bazy danych są wypełniane danymi na żywo. W związku z tym nie chcesz ponownie wdrażać bazy danych członkostwa w stanie początkowym, co spowoduje wymazanie wszystkich danych na żywo. Ponieważ SQL Server Compact bazami danych są pliki w folderze *\_danych aplikacji* , należy je zapobiec, zmieniając ustawienia wdrożenia, aby pliki w *aplikacji\_folderze danych* nie zostały wdrożone.

Otwórz okno **właściwości projektu** dla projektu ContosoUniversity, a następnie wybierz kartę **pakowanie/publikowanie w sieci Web** . Upewnij się, że pole listy rozwijanej **Konfiguracja** ma zaznaczoną opcję **aktywne (wydanie)** lub **Zwolnij** , wybierz pozycję **Wyklucz pliki z folderu\_danych aplikacji**.

![Exclude_files_from_the_App_Data_folder](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image9.png)

W przypadku podjęcia decyzji o wdrożeniu kompilacji debugowania w przyszłości warto wprowadzić tę samą zmianę dla konfiguracji kompilacji debugowania: Zmień **konfigurację** na **Debuguj** , a następnie wybierz pozycję **wyklucz pliki z folderu\_danych aplikacji**.

Zapisz i Zamknij kartę **pakowanie/publikowanie w sieci Web** .

> [!NOTE] 
> 
> [!IMPORTANT]
> Upewnij się, że nie masz żadnych **dodatkowych plików w miejscu docelowym** wybranym w profilach publikowania. W przypadku wybrania tej opcji proces wdrażania spowoduje usunięcie baz danych znajdujących się w aplikacji\_danych we wdrożonej witrynie. spowoduje to usunięcie aplikacji\_samego folderu danych.

## <a name="preventing-user-access-to-the-production-site-during-update"></a>Uniemożliwianie użytkownikom dostępu do witryny produkcyjnej podczas aktualizacji

Wdrażana zmiana to prosta zmiana jednej strony. Czasami jednak wdrażane są większe zmiany, a w takim przypadku lokacja może zachowywać się dziwnie, jeśli użytkownik zażąda strony przed ukończeniem wdrożenia. Aby tego uniknąć, można użyć *aplikacji\_pliku w trybie offline. htm* . Po umieszczeniu pliku o nazwie *app\_offline. htm* w folderze głównym aplikacji usługi IIS automatycznie wyświetlają ten plik zamiast uruchamiania aplikacji. W celu uniemożliwienia dostępu podczas wdrażania należy umieścić *aplikację\_w trybie offline. htm* w folderze głównym, uruchomić proces wdrażania, a następnie usunąć *aplikację\_trybie offline. htm*.

W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy Rozwiązanie (nie jeden z projektów) i wybierz pozycję **Nowy folder rozwiązania**.

![Creating_a_solution_folder](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image10.png)

Nazwij folder *SolutionFiles*.

W nowym folderze utwórz stronę HTML o nazwie *app\_offline. htm*. Zastąp istniejącą zawartość następującym znacznikiem:

[!code-html[Main](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/samples/sample2.html)]

Możesz skopiować *aplikację\_pliku offline. htm* do lokacji za pomocą połączenia FTP lub narzędzia **Menedżer plików** w panelu sterowania dostawcy hostingu. W tym samouczku użyjesz **Menedżera plików**.

Otwórz Panel sterowania i wybierz pozycję **Menedżer plików** , podobnie jak w samouczku [wdrażanie do środowiska produkcyjnego](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) . Wybierz pozycję **contosouniversity.com** , a następnie katalogu **wwwroot** , aby przejść do folderu głównego aplikacji, a następnie kliknij przycisk **Przekaż**.

[![Upload_button_in_File_Manager](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image12.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image11.png)

W oknie dialogowym **Przekaż plik** wybierz *aplikację\_pliku offline. htm* , a następnie kliknij przycisk **Przekaż**.

[![Upload_dialog_box_in_File_Manager](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image14.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image13.png)

Przejdź do adresu URL witryny. Zobaczysz, że *aplikacja\_w trybie offline. htm* jest teraz wyświetlana zamiast strony głównej.

[![App_offline. htm_page_in_production](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image15.png)

Teraz można przystąpić do wdrażania w środowisku produkcyjnym.

## <a name="deploying-the-code-update-to-the-production-environment"></a>Wdrażanie aktualizacji kodu w środowisku produkcyjnym

W **sieci Web kliknij przycisk Publikuj** na pasku narzędzi, wybierz profil publikowania **produkcyjnego** , a następnie kliknij pozycję **Publikuj sieć Web**.

Program Visual Studio wdraża zaktualizowaną aplikację i otwiera przeglądarkę na stronie głównej witryny. Zostanie wyświetlony plik *\_w trybie offline. htm* . Przed przeprowadzeniem testów w celu zweryfikowania pomyślnego wdrożenia należy usunąć *aplikację\_pliku offline. htm* .

Wróć do aplikacji **Menedżer plików** w panelu sterowania. Wybierz pozycję **contosouniversity.com** i **wwwroot**, wybierz pozycję **aplikacja\_offline. htm**, a następnie kliknij pozycję **Usuń**.

[![Deleting_app_offline. htm](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image17.png)

W przeglądarce Otwórz stronę instruktorzy w witrynie publicznej i wybierz instruktora, aby sprawdzić, czy aktualizacja została pomyślnie wdrożona.

[![Instructors_page_with_courses_Prod](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image19.png)

Wdrożono teraz aktualizację aplikacji, która nie obejmuje zmiany bazy danych. W następnym samouczku pokazano, jak wdrożyć zmianę bazy danych.

> [!div class="step-by-step"]
> [Poprzednie](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)
> [dalej](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)

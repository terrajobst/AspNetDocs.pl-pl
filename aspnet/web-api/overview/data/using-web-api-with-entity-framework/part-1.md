---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-1
title: Używanie interfejsu Web API 2 z Entity Framework 6 | Microsoft Docs
author: MikeWasson
description: Ten samouczek zawiera informacje na temat tworzenia aplikacji sieci Web za pomocą zaplecza interfejsu API sieci Web ASP.NET. Samouczek używa Entity Framework 6 do rozmieszczenia danych...
ms.author: riande
ms.date: 01/17/2019
ms.assetid: e879487e-dbcd-4b33-b092-d67c37ae768c
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-1
msc.type: authoredcontent
ms.openlocfilehash: 0f5dc960f494af5bd4ce87863a510d1892319908
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622480"
---
# <a name="using-web-api-2-with-entity-framework-6"></a>Używanie wzorca Web API 2 z programem Entity Framework 6

[Pobierz ukończony projekt](https://github.com/MikeWasson/BookService)

> W tym samouczku przedstawiono podstawowe informacje na temat tworzenia aplikacji sieci Web za pomocą zaplecza interfejsu API sieci Web ASP.NET. Samouczek używa Entity Framework 6 dla warstwy danych i odcinania. js dla aplikacji JavaScript po stronie klienta. W tym samouczku pokazano również, jak wdrożyć aplikację w Web Apps Azure App Service.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używane w samouczku
>
> - Internetowy interfejs API 2,1
> - Visual Studio 2017 (Pobierz program Visual Studio 2017 [tutaj](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))
> - Entity Framework 6
> - .NET 4.7
> - [Odcinanie. js](http://knockoutjs.com/) 3,1

W tym samouczku jest używane ASP.NET Web API 2 z Entity Framework 6 do tworzenia aplikacji sieci Web, która manipuluje bazą danych zaplecza. Oto zrzut ekranu przedstawiający aplikację, która zostanie utworzona.

[![](part-1/_static/image2.png)](part-1/_static/image1.png)

Aplikacja używa projektu jednostronicowej aplikacji (SPA). "Aplikacja jednostronicowa" to ogólny termin aplikacji sieci Web, który ładuje pojedynczą stronę HTML, a następnie automatycznie aktualizuje stronę, zamiast ładować nowe strony. Po początkowym ładowaniu strony aplikacja komunikuje się z serwerem za pomocą żądań AJAX. Żądania AJAX zwracają dane JSON, których aplikacja używa do aktualizowania interfejsu użytkownika.

Technologia AJAX nie jest nowa, ale dzisiaj istnieją struktury języka JavaScript, które ułatwiają tworzenie i konserwowanie dużej, zaawansowanej aplikacji SPA. W tym samouczku jest używany program [separowania. js](http://knockoutjs.com/), ale można użyć dowolnej struktury klienta języka JavaScript.

Oto główne bloki konstrukcyjne dla tej aplikacji:

- ASP.NET MVC tworzy stronę HTML.
- Interfejs API sieci Web ASP.NET obsługuje żądania AJAX i zwraca dane JSON.
- Dane odcinania. js — wiążą elementy HTML z danymi JSON.
- Entity Framework rozmowy z bazą danych.

## <a name="see-this-app-running-on-azure"></a>Zobacz tę aplikację działającą na platformie Azure

Czy chcesz zobaczyć gotową witrynę działającą jako aplikacja internetowa na żywo? Pełną wersję aplikacji można wdrożyć na koncie platformy Azure, wybierając poniższy przycisk.

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/BookService)

Aby wdrożyć to rozwiązanie na platformie Azure, musisz mieć konto platformy Azure. Jeśli nie masz jeszcze konta, możesz:

- [Bezpłatnie Otwórz konto platformy Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) — Uzyskaj kredyty, których możesz użyć do wypróbowania płatnych usług platformy Azure, a nawet po ich użyciu możesz zachować konto i korzystać z bezpłatnych usług platformy Azure.
- [Aktywuj korzyści dla subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) — subskrypcja MSDN daje środki na korzystanie z płatnych usług platformy Azure.

## <a name="create-the-project"></a>Tworzenie projektu

Otwórz program Visual Studio. Z menu **plik** wybierz pozycję **Nowy**, a następnie wybierz pozycję **projekt**. (Lub wybierz pozycję **Nowy projekt** na stronie startowej).

W oknie dialogowym **Nowy projekt** wybierz pozycję **Sieć Web** w lewym okienku i **aplikację sieci Web ASP.NET (.NET Framework)** w środkowym okienku. Nazwij projekt **BookService** i wybierz **przycisk OK**.

[![](part-1/_static/image11.png)](part-1/_static/image11.png)

W oknie dialogowym **Nowy projekt ASP.NET** wybierz szablon **internetowego interfejsu API** .

[![](part-1/_static/image12.png)](part-1/_static/image12.png)

Wybierz przycisk **OK**, aby utworzyć projekt.

## <a name="configure-azure-settings-optional"></a>Konfigurowanie ustawień platformy Azure (opcjonalnie)

Po utworzeniu projektu można wybrać opcję wdrożenia do Azure App Service Web Apps w dowolnym momencie. 

1. W Eksplorator rozwiązań kliknij prawym przyciskiem myszy projekt i wybierz polecenie **Publikuj**.

2. W wyświetlonym oknie wybierz pozycję **Rozpocznij**. Zostanie wyświetlone okno dialogowe **Wybieranie elementu docelowego publikowania** .

   [![](part-1/_static/image14.png)](part-1/_static/image14.png)

3. Wybierz pozycję **Utwórz profil**. Zostanie wyświetlone okno dialogowe **tworzenie App Service** .

   [![](part-1/_static/image15.png)](part-1/_static/image15.png)

   Zaakceptuj wartości domyślne lub wprowadź różne wartości dla nazwy aplikacji, grupy zasobów, planu hostingu, subskrypcji platformy Azure i regionu geograficznego. 

4. Wybierz pozycję **Utwórz bazę danych SQL**. Zostanie wyświetlone okno dialogowe **konfigurowanie SQL Server** . 

   [![](part-1/_static/image16.png)](part-1/_static/image16.png)

   Zaakceptuj wartości domyślne lub wprowadź różne wartości. Wprowadź **nazwę użytkownika** i **hasło administratora** dla nowej bazy danych. Po zakończeniu wybierz **przycisk OK** . Zostanie wyświetlona strona **tworzenie App Service** .

5. Wybierz pozycję **Utwórz** , aby utworzyć profil. W prawym dolnym rogu pojawia się komunikat z informacją, że wdrożenie jest w toku. Po krótkim czasie okno **publikowania** zostanie wyświetlone ponownie.

    [![](part-1/_static/image17.png)](part-1/_static/image17.png)
   
    Profil utworzony w celu wdrożenia aplikacji jest teraz dostępny. 

> [!div class="step-by-step"]
> [Dalej](part-2.md)

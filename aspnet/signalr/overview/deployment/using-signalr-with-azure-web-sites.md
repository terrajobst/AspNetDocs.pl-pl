---
uid: signalr/overview/deployment/using-signalr-with-azure-web-sites
title: Używanie sygnalizującego z Web Apps w Azure App Service | Microsoft Docs
author: bradygaster
description: W tym dokumencie opisano sposób konfigurowania aplikacji sygnalizującej działającej na Microsoft Azure. Wersje oprogramowania używane w samouczku Visual Studio 2013 lub...
ms.author: bradyg
ms.date: 07/01/2015
ms.assetid: 2a7517a0-b88c-4162-ade3-9bf6ca7062fd
msc.legacyurl: /signalr/overview/deployment/using-signalr-with-azure-web-sites
msc.type: authoredcontent
ms.openlocfilehash: 0e6a18bdbe9cc47e89b5a458753845afb53595f1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78558703"
---
# <a name="using-signalr-with-web-apps-in-azure-app-service"></a>Używanie usługi SignalR z funkcją Web Apps w usłudze Azure App Service

[Fletcher Patryka](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> W tym dokumencie opisano sposób konfigurowania aplikacji sygnalizującej działającej na Microsoft Azure.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używane w samouczku
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013) lub Visual Studio 2012
> - .NET 4.5
> - Sygnalizujący wersja 2
> - Zestaw Azure SDK 2,3 dla Visual Studio 2013 lub 2012
>
>
>
> ## <a name="questions-and-comments"></a>Pytania i Komentarze
>
> Prosimy o opinię na temat sposobu, w jaki lubię ten samouczek, i co możemy ulepszyć w komentarzach w dolnej części strony. Jeśli masz pytania, które nie są bezpośrednio związane z tym samouczkiem, możesz je ogłosić na [forum ASP.NET sygnalizującego](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR), [StackOverflow.com](http://stackoverflow.com/)lub na [forach Microsoft Azure](https://social.msdn.microsoft.com/Forums/windowsazure/home?category=windowsazureplatform).

## <a name="table-of-contents"></a>Spis treści

- [Wprowadzenie](#introduction)
- [Wdrażanie aplikacji sieci Web sygnalizującej w Azure App Service](#deploying)
- [Włączanie obiektów WebSockets na Azure App Service](#websocket)
- [Korzystanie z Azure Redis Cache](#backplane)
- [Następne kroki](#nextsteps)

<a id="introduction"></a>
## <a name="introduction"></a>Wprowadzenie

Sygnalizujący ASP.NET może służyć do przeprowadzenia nowego poziomu interakcji między serwerami i klientami sieci Web lub .NET. Na platformie Azure aplikacje sygnalizujące mogą korzystać z wysoce dostępnych, skalowalnych i wydajnych środowisk, które działają w chmurze.

<a id="deploying"></a>
## <a name="deploying-a-signalr-web-app-to-azure-app-service"></a>Wdrażanie aplikacji sieci Web sygnalizującej w Azure App Service

Sygnalizujący nie dodaje szczególnych komplikacji do wdrożenia aplikacji na platformie Azure, a wdrożenie na serwerze lokalnym. Aplikacja, która korzysta z programu sygnalizującego, może być hostowana na platformie Azure bez wprowadzania zmian w konfiguracji ani innych ustawień (w przypadku obsługi usługi WebSockets znajduje się [w temacie Włączanie obiektów WebSockets na Azure App Service](#websocket) poniżej). W tym samouczku zostanie wdrożona aplikacja utworzona w [samouczku wprowadzenie](../getting-started/tutorial-getting-started-with-signalr.md) na platformie Azure.

**Wymagania wstępne**

- Visual Studio 2013. Jeśli nie masz programu Visual Studio, Visual Studio 2013 Express for Web jest dołączany do instalacji zestawu Azure SDK.
- [Zestaw Azure sdk 2,3 dla Visual Studio 2013](https://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409) lub [Azure SDK 2,3 dla programu Visual Studio 2012](https://go.microsoft.com/fwlink/p/?linkid=323511).
- Do ukończenia tego samouczka potrzebna jest subskrypcja platformy Azure. Możesz [aktywować korzyści dla subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)lub [zarejestrować się w celu uzyskania subskrypcji wersji próbnej](https://azure.microsoft.com/pricing/free-trial/).

### <a name="deploying-a-signalr-web-app-to-azure"></a>Wdrażanie aplikacji sieci Web sygnalizującej na platformie Azure

1. Ukończ [samouczek wprowadzenie](../getting-started/tutorial-getting-started-with-signalr.md)lub Pobierz gotowy projekt z [galerii kodu](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).
2. W programie Visual Studio wybierz kolejno opcje **kompilacja**i **rozmowa z czatem**.
3. W oknie dialogowym "Publikowanie w sieci Web" Wybierz pozycję "witryny sieci Web systemu Windows Azure".

    ![Wybieranie witryn sieci Web systemu Azure](using-signalr-with-azure-web-sites/_static/image1.png)
4. Jeśli nie zalogowano się do konto Microsoft, kliknij przycisk **Zaloguj się...** w oknie dialogowym "Wybierz istniejącą witrynę sieci Web" i zaloguj się.

    ![Wybierz istniejącą witrynę sieci Web](using-signalr-with-azure-web-sites/_static/image2.png)    ![Logowanie do platformy Azure](using-signalr-with-azure-web-sites/_static/image3.png)
5. W oknie dialogowym "Wybierz istniejącą witrynę sieci Web" kliknij pozycję **Nowy**.

    ![Nowa witryna sieci Web](using-signalr-with-azure-web-sites/_static/image4.png)
6. W oknie dialogowym "Tworzenie witryny w systemie Windows Azure" Wprowadź unikatową nazwę aplikacji. Wybierz region znajdujący się najbliżej siebie na liście rozwijanej region. Kliknij przycisk **Utwórz**.

    ![Tworzenie witryny na platformie Azure](using-signalr-with-azure-web-sites/_static/image5.png)
7. W oknie dialogowym "Publikowanie w sieci Web" kliknij pozycję **Publikuj**.

    ![Publikuj witrynę](using-signalr-with-azure-web-sites/_static/image6.png)
8. Gdy aplikacja zakończyła publikowanie, aplikacja rozmowy sygnalizująca hostowana w Azure App Service Web Apps zostanie otwarta w przeglądarce.

    ![Otwieranie witryny w przeglądarce](using-signalr-with-azure-web-sites/_static/image7.png)

<a id="websocket"></a>
### <a name="enabling-websockets-on-azure-app-service-web-apps"></a>Włączanie obiektów WebSockets na Azure App Service Web Apps

Obiekty WebSockets muszą być jawnie włączone w aplikacji sieci Web, aby można było ich używać w aplikacji sygnalizującej; w przeciwnym razie będą używane inne protokoły (zobacz [transporty i rezerwy](../getting-started/introduction-to-signalr.md#transports) , aby uzyskać szczegółowe informacje).

Aby można było używać obiektów WebSockets w Azure App Service Web Apps, włącz je w sekcji konfiguracji aplikacji sieci Web. Aby to zrobić, Otwórz aplikację sieci Web w [usłudze Azure Portal zarządzania](https://manage.windowsazure.com/)i wybierz pozycję Konfiguruj.

![Karta Konfigurowanie](using-signalr-with-azure-web-sites/_static/image8.png)

W górnej części strony konfiguracji upewnij się, że dla aplikacji sieci Web jest używany program .NET 4,5.

![Ustawienie programu .NET Framework w wersji 4,5](using-signalr-with-azure-web-sites/_static/image9.png)

Na stronie Konfiguracja w obszarze Ustawienia **WebSockets** wybierz pozycję **włączone**.

![Ustawienie obiektów WebSockets: włączone](using-signalr-with-azure-web-sites/_static/image10.png)

W dolnej części strony konfiguracja wybierz pozycję **Zapisz** , aby zapisać zmiany.

![Zapisz ustawienia](using-signalr-with-azure-web-sites/_static/image11.png)

<a id="backplane"></a>
## <a name="using-the-azure-redis-cache-backplane"></a>Korzystanie z Azure Redis Cache

W przypadku korzystania z wielu wystąpień aplikacji sieci Web, a użytkownicy tych wystąpień muszą współistnieć ze sobą (w związku z tym, że na przykład komunikaty czatu utworzone w jednym wystąpieniu mogą dotrzeć do użytkowników połączonych z innymi wystąpieniami), Azure Redis Cache w aplikacji musi być zaimplementowany [Plan](../performance/scaleout-with-redis.md) samodzielny.

<a id="nextsteps"></a>
## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji na Web Apps w Azure App Service, zobacz [omówienie Web Apps](https://azure.microsoft.com/documentation/articles/app-service-web-overview/).

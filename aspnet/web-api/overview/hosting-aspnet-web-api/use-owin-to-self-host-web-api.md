---
uid: web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
title: Korzystanie z OWIN do samoobsługowego hostowania ASP.NET Web API-ASP.NET 4. x
author: rick-anderson
description: Samouczek z kodem pokazujący, jak hostować interfejs API sieci Web ASP.NET w aplikacji konsolowej.
ms.author: riande
ms.date: 07/09/2013
ms.custom: seoapril2019
ms.assetid: a90a04ce-9d07-43ad-8250-8a92fb2bd3d5
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
msc.type: authoredcontent
ms.openlocfilehash: 872b931391a63ef82b96e5b264c070c0b5e9605d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556540"
---
# <a name="use-owin-to-self-host-aspnet-web-api"></a>Korzystanie z OWIN do samoobsługowego hostowania interfejsu API sieci Web ASP.NET 

> W tym samouczku pokazano, jak hostować interfejs API sieci Web ASP.NET w aplikacji konsolowej przy użyciu OWIN do samodzielnego hostowania struktury internetowego interfejsu API.
>
> [Otwórz interfejs sieci Web dla platformy .NET](http://owin.org) (Owin) definiuje abstrakcję między serwerami sieci Web i aplikacjami sieci Web programu .NET. OWIN oddziela aplikację sieci Web od serwera, co sprawia, że OWIN jest idealnym rozwiązaniem do samodzielnego udostępniania aplikacji sieci Web we własnym procesie poza programem IIS.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używane w samouczku
>
>
> - [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) 
> - 5\.2.7 internetowego interfejsu API

> [!NOTE]
> Pełny kod źródłowy dla tego samouczka można znaleźć w witrynie [GitHub.com/ASPNET/Samples](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/OwinSelfhostSample).

## <a name="create-a-console-application"></a>Tworzenie aplikacji konsolowej

W menu **plik** , **Nowy**, a następnie wybierz **projekt**. W **obszarze** **Wizualizacja C#** wybierz pozycję **Windows Desktop** , a następnie wybierz pozycję **Aplikacja konsolowa (.NET Framework)** . Nadaj projektowi nazwę "OwinSelfhostSample" i wybierz pozycję **OK**.

[![](use-owin-to-self-host-web-api/_static/image7.png)](use-owin-to-self-host-web-api/_static/image7.png)

## <a name="add-the-web-api-and-owin-packages"></a>Dodawanie interfejsu API sieci Web i pakietów OWIN

W menu **Narzędzia** wybierz pozycję **Menedżer pakietów NuGet**, a następnie wybierz pozycję **konsola Menedżera pakietów**. W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:

`Install-Package Microsoft.AspNet.WebApi.OwinSelfHost`

Spowoduje to zainstalowanie pakietu WebAPI OWIN SelfHost oraz wszystkich wymaganych pakietów OWIN.

[![](use-owin-to-self-host-web-api/_static/image4.png)](use-owin-to-self-host-web-api/_static/image3.png)

## <a name="configure-web-api-for-self-host"></a>Konfigurowanie interfejsu API sieci Web dla samego hosta

W Eksplorator rozwiązań kliknij prawym przyciskiem myszy projekt, a następnie wybierz polecenie **Dodaj** **klasę** / , aby dodać nową klasę. Nadaj klasie nazwę `Startup`.

![](use-owin-to-self-host-web-api/_static/image5.png)

Zastąp cały kod standardowy w tym pliku następującym:

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample1.cs)]

## <a name="add-a-web-api-controller"></a>Dodawanie kontrolera interfejsu API sieci Web

Następnie Dodaj klasę kontrolera interfejsu API sieci Web. W Eksplorator rozwiązań kliknij prawym przyciskiem myszy projekt, a następnie wybierz polecenie **Dodaj** **klasę** / , aby dodać nową klasę. Nadaj klasie nazwę `ValuesController`.

Zastąp cały kod standardowy w tym pliku następującym:

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample2.cs)]

## <a name="start-the-owin-host-and-make-a-request-with-httpclient"></a>Uruchom hosta OWIN i Utwórz żądanie z HttpClient

Zastąp cały kod standardowy w pliku Program.cs następującym:

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample3.cs)]

## <a name="run-the-application"></a>Uruchamianie aplikacji

Aby uruchomić aplikację, naciśnij klawisz F5 w programie Visual Studio. Dane wyjściowe powinny wyglądać następująco:

[!code-console[Main](use-owin-to-self-host-web-api/samples/sample4.cmd)]

![](use-owin-to-self-host-web-api/_static/image6.png)

## <a name="additional-resources"></a>Dodatkowe zasoby

[Omówienie projektu Katana](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)

[Hostowanie interfejsu API sieci Web ASP.NET w roli procesu roboczego platformy Azure](host-aspnet-web-api-in-an-azure-worker-role.md)

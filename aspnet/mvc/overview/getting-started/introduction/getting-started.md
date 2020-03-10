---
uid: mvc/overview/getting-started/introduction/getting-started
title: Wprowadzenie z ASP.NET MVC 5 | Microsoft Docs
author: Rick-Anderson
ms.author: riande
ms.date: 10/04/2018
ms.assetid: f3d8adbe-55e7-4fd4-84a8-7155bc45c676
msc.legacyurl: /mvc/overview/getting-started/introduction/getting-started
msc.type: authoredcontent
ms.openlocfilehash: ca39bc37c757c0452cf56624c8e37c04df4b41f2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78602768"
---
# <a name="getting-started-with-aspnet-mvc-5"></a>Wprowadzenie do korzystania z ASP.NET MVC 5

Autor [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [consider RP](../../../../includes/razor.md)]

W tym samouczku przedstawiono podstawowe informacje na temat tworzenia aplikacji sieci Web ASP.NET MVC 5 przy użyciu [programu Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017). Końcowy kod źródłowy samouczka znajduje się w witrynie [GitHub](https://github.com/dotnet/AspNetDocs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie/MvcMovie).

Ten samouczek został zapisany przez [Scott Guthrie](https://weblogs.asp.net/scottgu/) (Twitter[@scottgu](https://twitter.com/scottgu) ), [Scott Hanselman](http://www.hanselman.com/blog/) (Twitter: [@shanselman](https://twitter.com/shanselman) ) i [Rick Anderson](https://twitter.com/RickAndMSFT) ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) )

Do wdrożenia tej aplikacji na platformie Azure jest potrzebne konto platformy Azure:

- Możesz [bezpłatnie otworzyć konto platformy Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) — możesz uzyskać środki, których możesz użyć do wypróbowania płatnych usług platformy Azure, a nawet po ich użyciu możesz zachować konto i korzystać z bezpłatnych usług platformy Azure.
- Możesz [aktywować korzyści dla subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) — w ramach subskrypcji MSDN są dostępne środki na korzystanie z płatnych usług platformy Azure.

## <a name="get-started"></a>Rozpoczynanie pracy

Zacznij od [zainstalowania programu Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017). Następnie otwórz program Visual Studio.

Program Visual Studio to IDE lub zintegrowane środowisko programistyczne. Podobnie jak w przypadku używania programu Microsoft Word do pisania dokumentów, do tworzenia aplikacji będziesz używać środowiska IDE. W programie Visual Studio znajduje się lista znajdująca się na dole, pokazująca różne dostępne opcje. Istnieje również menu, które zapewnia inny sposób wykonywania zadań w środowisku IDE. Na przykład zamiast wybierania **nowego projektu** na **stronie Start**można użyć paska menu i wybrać polecenie **plik** > **Nowy projekt**.

![](getting-started/_static/image1.png)

## <a name="create-your-first-app"></a>Tworzenie pierwszej aplikacji

Na **stronie startowej**wybierz pozycję **Nowy projekt**. W oknie dialogowym **Nowy projekt** wybierz kategorię  **C# Wizualizacja** po lewej stronie, a następnie kliknij pozycję **Sieć Web**, a następnie wybierz szablon projektu **aplikacja sieci Web ASP.NET (.NET Framework)** . Nadaj projektowi nazwę "MvcMovie", a następnie wybierz przycisk **OK**.

![](getting-started/_static/image2.png)

W oknie dialogowym **Nowa aplikacja sieci Web ASP.NET** wybierz pozycję **MVC** , a następnie wybierz przycisk **OK**.

![](getting-started/_static/image3.png)

Program Visual Studio użył szablonu domyślnego dla projektu ASP.NET MVC, który właśnie został utworzony, więc masz działającą aplikację, która teraz nie wykonuje żadnych działań. To jest prosta "Hello world!" projekt i jest dobrym miejscem do uruchamiania aplikacji.

![](getting-started/_static/image4.png)

Naciśnij klawisz **F5**, aby uruchomić debugowanie. Po naciśnięciu klawisza **F5**program Visual Studio uruchomi [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) i uruchomi aplikację sieci Web. Program Visual Studio uruchomi przeglądarkę i otworzy stronę główną aplikacji. Zwróć uwagę, że na pasku adresu przeglądarki widnieją `localhost:port#` a nie `example.com`. Jest to spowodowane tym, że `localhost` zawsze wskazuje na własny komputer lokalny, w tym przypadku uruchamia właśnie utworzoną aplikację. Gdy program Visual Studio uruchamia projekt sieci Web, dla serwera sieci Web jest używany port losowy. Na poniższej ilustracji numer portu to 1234. Po uruchomieniu aplikacji zobaczysz inny numer portu.

![](getting-started/_static/image5.png)

Po prawej stronie pola ten szablon domyślny udostępnia strony `Home`, `Contact`i `About`. Na poniższym obrazie nie są wyświetlane linki **Home**, **about**i **Contact** . W zależności od rozmiaru okna przeglądarki, aby wyświetlić te linki, może być konieczne kliknięcie ikony nawigacji.

![](getting-started/_static/image6.png)

Aplikacja zapewnia również obsługę rejestrowania i logowania. Następnym krokiem jest zmiana sposobu działania tej aplikacji i uzyskanie nieco informacji o ASP.NET MVC. Zamknij aplikację ASP.NET MVC i Zmieńmy kod.

Aby zapoznać się z listą bieżących samouczków, zobacz [artykuły zalecane przez MVC](../mvc-learning-sequence.md).

## <a name="see-this-app-running-on-azure"></a>Zobacz tę aplikację działającą na platformie Azure

Czy chcesz zobaczyć gotową witrynę działającą jako aplikacja internetowa na żywo? Pełną wersję aplikacji możesz wdrożyć na koncie platformy Azure — wystarczy kliknąć poniższy przycisk.

[![](https://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?repository=https://github.com/dotnet/AspNetDocs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie&amp;WT.mc_id=deploy_azure_aspnet)

Aby wdrożyć to rozwiązanie na platformie Azure, musisz mieć konto platformy Azure. Jeśli nie masz jeszcze konta, użyj jednej z następujących opcji, aby je utworzyć:

- [Bezpłatnie Otwórz konto platformy Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) — Uzyskaj kredyty, których możesz użyć do wypróbowania płatnych usług platformy Azure, a nawet po ich użyciu możesz zachować konto i korzystać z bezpłatnych usług platformy Azure.
- [Aktywuj korzyści dla subskrybentów programu Visual Studio](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers) — Twoja subskrypcja programu Visual Studio zapewnia środki na korzystanie z płatnych usług platformy Azure.

> [!div class="step-by-step"]
> [Dalej](adding-a-controller.md)

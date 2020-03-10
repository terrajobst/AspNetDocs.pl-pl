---
uid: aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
title: OWIN hosta w roli procesu roboczego platformy Azure | Microsoft Docs
author: MikeWasson
description: W tym samouczku przedstawiono sposób samodzielnego hostowania OWIN w roli procesu roboczego Microsoft Azure. Otwórz interfejs sieci Web dla platformy .NET (OWIN) definiuje abstrakcję między serwerem sieci Web platformy .NET...
ms.author: riande
ms.date: 04/11/2014
ms.assetid: 07aa855a-92ee-4d43-ba66-5bfd7de20ee6
msc.legacyurl: /aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: 59d2e0d549427093f8a2424b17af81169b78ef30
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78584617"
---
# <a name="host-owin-in-an-azure-worker-role"></a>Hostowanie interfejsu OWIN w roli procesu roboczego platformy Azure

według [Jan Wasson](https://github.com/MikeWasson)

> W tym samouczku przedstawiono sposób samodzielnego hostowania OWIN w roli procesu roboczego Microsoft Azure.
>
> [Otwórz interfejs sieci Web dla platformy .NET](http://owin.org/) (Owin) definiuje abstrakcję między serwerami sieci Web i aplikacjami sieci Web programu .NET. OWIN oddziela aplikację sieci Web od serwera, co sprawia, że OWIN jest idealnym rozwiązaniem do samodzielnego udostępniania aplikacji sieci Web we własnym procesie poza usługami IIS — na przykład w ramach roli proces roboczy platformy Azure.
>
> W tym samouczku dowiesz się, jak samoobsługowo OWIN aplikacje w ramach roli procesu roboczego Microsoft Azure. Aby dowiedzieć się więcej o rolach procesów roboczych, zobacz [modele wykonywania platformy Azure](https://azure.microsoft.com/documentation/articles/fundamentals-application-models/#CloudServices).
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używane w samouczku
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - [Zestaw Azure SDK dla programu .NET 2,3](https://azure.microsoft.com/downloads/)
> - [Microsoft. Owin. SelfHost 2.1.0](http://www.nuget.org/packages/Microsoft.Owin.SelfHost/2.1.0)

## <a name="create-a-microsoft-azure-project"></a>Tworzenie projektu Microsoft Azure

Uruchom program Visual Studio z uprawnieniami administratora. Uprawnienia administratora są konieczne do lokalnego debugowania aplikacji przy użyciu emulatora obliczeń platformy Azure.

W menu **plik** kliknij pozycję **Nowy**, a następnie kliknij pozycję **projekt**. Z **zainstalowanych szablonów**, w obszarze C#Wizualizacja, kliknij pozycję **chmura** , a następnie kliknij pozycję **Usługa w chmurze platformy Microsoft Azure**. Nadaj projektowi nazwę "plik azureapp" i kliknij przycisk **OK**.

[![](host-owin-in-an-azure-worker-role/_static/image2.png)](host-owin-in-an-azure-worker-role/_static/image1.png)

W **nowej usłudze w chmurze platformy Microsoft Azure** okno dialogowe, kliknij dwukrotnie **rolę proces roboczy**. Pozostaw nazwę domyślną ("WorkerRole1"). Ten krok powoduje dodanie roli procesu roboczego do rozwiązania. Kliknij przycisk **OK**.

[![](host-owin-in-an-azure-worker-role/_static/image4.png)](host-owin-in-an-azure-worker-role/_static/image3.png)

Utworzone rozwiązanie programu Visual Studio zawiera dwa projekty:

- &quot;plik azureapp&quot; definiuje role i konfigurację dla aplikacji platformy Azure.
- &quot;WorkerRole1&quot; zawiera kod dla roli proces roboczy.

Ogólnie rzecz biorąc, aplikacja platformy Azure może zawierać wiele ról, chociaż w tym samouczku zostanie użyta jedna rola.

![](host-owin-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-owin-self-host-packages"></a>Dodawanie pakietów samoobsługi OWIN

W menu **Narzędzia** kliknij pozycję **Menedżer pakietów NuGet**, a następnie kliknij pozycję **konsola Menedżera pakietów**.

W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:

[!code-console[Main](host-owin-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a>Dodawanie punktu końcowego HTTP

W Eksplorator rozwiązań rozwiń projekt plik azureapp. Rozwiń węzeł role, kliknij prawym przyciskiem myszy pozycję WorkerRole1, a następnie wybierz pozycję **Właściwości**.

![](host-owin-in-an-azure-worker-role/_static/image6.png)

Kliknij pozycję **punkty końcowe**, a następnie kliknij pozycję **Dodaj punkt końcowy**.

Z listy rozwijanej **Protokół** wybierz pozycję "http". W polu **port publiczny** i **port prywatny**wpisz 80. Te numery portów mogą być różne. Port publiczny jest używany przez klientów podczas wysyłania żądania do roli.

[![](host-owin-in-an-azure-worker-role/_static/image8.png)](host-owin-in-an-azure-worker-role/_static/image7.png)

## <a name="create-the-owin-startup-class"></a>Utwórz klasę uruchomieniową OWIN

W Eksplorator rozwiązań kliknij prawym przyciskiem myszy projekt WorkerRole1 i wybierz polecenie **Dodaj** **klasę** / , aby dodać nową klasę. Nadaj klasie nazwę `Startup`.

Zastąp cały kod standardowy następującymi:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample2.cs)]

Metoda rozszerzenia `UseWelcomePage` dodaje prostą stronę HTML do aplikacji, aby sprawdzić, czy witryna działa.

## <a name="start-the-owin-host"></a>Uruchom hosta OWIN

Otwórz plik WorkerRole.cs. Ta klasa definiuje kod, który jest uruchamiany po uruchomieniu i zatrzymaniu roli procesu roboczego.

Dodaj następującą instrukcję using:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample3.cs)]

Dodaj element członkowski **IDisposable** do klasy `WorkerRole`:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample4.cs)]

W metodzie `OnStart` Dodaj następujący kod, aby uruchomić hosta:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample5.cs?highlight=5)]

Metoda **WEBAPP. Start** uruchamia hosta Owin. Nazwa klasy `Startup` jest parametrem typu dla metody. Zgodnie z Konwencją, Host wywoła metodę `Configure` tej klasy.

Przesłoń `OnStop`, aby usunąć wystąpienie *aplikacji\_* :

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample6.cs)]

Oto kompletny kod dla WorkerRole.cs:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample7.cs)]

Skompiluj rozwiązanie i naciśnij klawisz F5, aby uruchomić aplikację lokalnie w emulatorze obliczeń platformy Azure. W zależności od ustawień zapory może być konieczne zezwolenie emulatora przez zaporę.

Emulator obliczeń przypisuje lokalny adres IP do punktu końcowego. Adres IP można znaleźć, wyświetlając interfejs użytkownika emulatora obliczeń. Kliknij prawym przyciskiem myszy ikonę emulatora w obszarze powiadomień paska zadań, a następnie wybierz pozycję **Pokaż interfejs użytkownika emulatora obliczeń**.

[![](host-owin-in-an-azure-worker-role/_static/image10.png)](host-owin-in-an-azure-worker-role/_static/image9.png)

Znajdź adres IP w obszarze wdrożenia usługi, wdrożenie [ID], Szczegóły usługi. Otwórz przeglądarkę internetową i przejdź do adresu http:\/\/*Address*, gdzie *Address* to adres IP przypisany przez emulator obliczeń; na przykład `http://127.0.0.1:80`. Powinna zostać wyświetlona strona powitalna OWIN:

![](host-owin-in-an-azure-worker-role/_static/image11.png)

## <a name="deploy-to-azure"></a>Wdrażanie na platformie Azure

W tym kroku musisz mieć konto platformy Azure. Jeśli jeszcze tego nie masz, możesz utworzyć bezpłatne konto próbne w zaledwie kilka minut. Aby uzyskać szczegółowe informacje, zobacz [Microsoft Azure bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).

W Eksplorator rozwiązań kliknij prawym przyciskiem myszy projekt plik azureapp. Wybierz pozycję **Publikuj**.

![](host-owin-in-an-azure-worker-role/_static/image12.png)

Jeśli nie zalogowano się na koncie platformy Azure, kliknij przycisk **Zaloguj**.

[![](host-owin-in-an-azure-worker-role/_static/image14.png)](host-owin-in-an-azure-worker-role/_static/image13.png)

Po zalogowaniu wybierz subskrypcję i kliknij przycisk **dalej**.

[![](host-owin-in-an-azure-worker-role/_static/image16.png)](host-owin-in-an-azure-worker-role/_static/image15.png)

Wprowadź nazwę usługi w chmurze i wybierz region. Kliknij przycisk **Utwórz**.

![](host-owin-in-an-azure-worker-role/_static/image17.png)

Kliknij przycisk **Opublikuj**.

[![](host-owin-in-an-azure-worker-role/_static/image19.png)](host-owin-in-an-azure-worker-role/_static/image18.png)

W oknie Dziennik aktywności platformy Azure zostanie wyświetlony postęp wdrażania. Po wdrożeniu aplikacji przejdź do `http://appname.cloudapp.net/`, gdzie *nazwa_aplikacji* to nazwa usługi w chmurze.

## <a name="additional-resources"></a>Dodatkowe materiały

- [Omówienie projektu Katana](an-overview-of-project-katana.md)
- [Projekt Katana w witrynie GitHub](https://github.com/aspnet/AspNetKatana/)

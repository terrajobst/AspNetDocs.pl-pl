---
uid: web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
title: Hostowanie ASP.NET Web API 2 w roli proces roboczy platformy Azure — ASP.NET 4. x
author: MikeWasson
description: 'Samouczek: hostowanie interfejsu API sieci Web ASP.NET w roli procesu roboczego platformy Azure przy użyciu OWIN do samoobsługowego udostępniania struktury internetowego interfejsu API.'
ms.author: riande
ms.date: 04/02/2014
ms.custom: seoapril2019
ms.assetid: 6980ee2e-d6b0-4a08-8fb6-ab96362dd0e3
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: ec9904e0bff090be0f504036ae73977cfca0cb31
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556631"
---
# <a name="host-aspnet-web-api-2-in-an-azure-worker-role"></a>Hostowanie ASP.NET Web API 2 w roli procesu roboczego platformy Azure

według [Jan Wasson](https://github.com/MikeWasson)

> W tym samouczku pokazano, jak hostować interfejs API sieci Web ASP.NET w roli procesu roboczego platformy Azure przy użyciu OWIN do samoobsługowego udostępniania struktury internetowego interfejsu API.
>
> [Otwórz interfejs sieci Web dla platformy .NET](http://owin.org/) (Owin) definiuje abstrakcję między serwerami sieci Web i aplikacjami sieci Web programu .NET. OWIN oddziela aplikację sieci Web od serwera, co sprawia, że OWIN jest idealnym rozwiązaniem do samodzielnego udostępniania aplikacji sieci Web we własnym procesie poza usługami IIS — na przykład w ramach roli proces roboczy platformy Azure.
>
> W tym samouczku zostanie użyty pakiet Microsoft. Owin. host. odbiornika HttpListener, który udostępnia serwer HTTP, który będzie używany do samoobsługowego hostowania aplikacji OWIN.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używane w samouczku
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - Internetowy interfejs API 2
> - [Zestaw Azure SDK dla programu .NET 2,3](https://azure.microsoft.com/downloads/)

## <a name="create-a-microsoft-azure-project"></a>Tworzenie projektu Microsoft Azure

Uruchom program Visual Studio z uprawnieniami administratora. Uprawnienia administratora są konieczne do lokalnego debugowania aplikacji przy użyciu emulatora obliczeń platformy Azure.

W menu **plik** kliknij pozycję **Nowy**, a następnie kliknij pozycję **projekt**. Z **zainstalowanych szablonów**, w obszarze C#Wizualizacja, kliknij pozycję **chmura** , a następnie kliknij pozycję **Usługa w chmurze platformy Microsoft Azure**. Nadaj projektowi nazwę "plik azureapp" i kliknij przycisk **OK**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image2.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image1.png)

W **nowej usłudze w chmurze platformy Microsoft Azure** okno dialogowe, kliknij dwukrotnie **rolę proces roboczy**. Pozostaw nazwę domyślną ("WorkerRole1"). Ten krok powoduje dodanie roli procesu roboczego do rozwiązania. Kliknij przycisk **OK**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image4.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image3.png)

Utworzone rozwiązanie programu Visual Studio zawiera dwa projekty:

- &quot;plik azureapp&quot; definiuje role i konfigurację dla aplikacji platformy Azure.
- &quot;WorkerRole1&quot; zawiera kod dla roli proces roboczy.

Ogólnie rzecz biorąc, aplikacja platformy Azure może zawierać wiele ról, chociaż w tym samouczku zostanie użyta jedna rola.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-web-api-and-owin-packages"></a>Dodawanie interfejsu API sieci Web i pakietów OWIN

W menu **Narzędzia** kliknij pozycję **Menedżer pakietów NuGet**, a następnie kliknij pozycję **konsola Menedżera pakietów**.

W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:

[!code-console[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a>Dodawanie punktu końcowego HTTP

W Eksplorator rozwiązań rozwiń projekt plik azureapp. Rozwiń węzeł role, kliknij prawym przyciskiem myszy pozycję WorkerRole1, a następnie wybierz pozycję **Właściwości**.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image6.png)

Kliknij pozycję **punkty końcowe**, a następnie kliknij pozycję **Dodaj punkt końcowy**.

Z listy rozwijanej **Protokół** wybierz pozycję "http". W polu **port publiczny** i **port prywatny**wpisz 80. Te numery portów mogą być różne. Port publiczny jest używany przez klientów podczas wysyłania żądania do roli.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image8.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image7.png)

## <a name="configure-web-api-for-self-host"></a>Konfigurowanie interfejsu API sieci Web dla samego hosta

W Eksplorator rozwiązań kliknij prawym przyciskiem myszy projekt WorkerRole1 i wybierz polecenie **Dodaj** **klasę** / , aby dodać nową klasę. Nadaj klasie nazwę `Startup`.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image9.png)

Zastąp cały kod standardowy w tym pliku następującym:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample2.cs)]

## <a name="add-a-web-api-controller"></a>Dodawanie kontrolera interfejsu API sieci Web

Następnie Dodaj klasę kontrolera interfejsu API sieci Web. Kliknij prawym przyciskiem myszy projekt WorkerRole1 i wybierz polecenie **Dodaj** **klasę** / . Nadaj klasie nazwę TestController. Zastąp cały kod standardowy w tym pliku następującym:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample3.cs)]

Dla uproszczenia ten kontroler tylko definiuje dwie metody GET, które zwracają zwykły tekst.

## <a name="start-the-owin-host"></a>Uruchom hosta OWIN

Otwórz plik WorkerRole.cs. Ta klasa definiuje kod, który jest uruchamiany po uruchomieniu i zatrzymaniu roli procesu roboczego.

Dodaj następującą instrukcję using:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample4.cs)]

Dodaj element członkowski **IDisposable** do klasy `WorkerRole`:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample5.cs)]

W metodzie `OnStart` Dodaj następujący kod, aby uruchomić hosta:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample6.cs?highlight=5)]

Metoda **WEBAPP. Start** uruchamia hosta Owin. Nazwa klasy `Startup` jest parametrem typu dla metody. Zgodnie z Konwencją, Host wywoła metodę `Configure` tej klasy.

Przesłoń `OnStop`, aby usunąć wystąpienie *aplikacji\_* :

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample7.cs)]

Oto kompletny kod dla WorkerRole.cs:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample8.cs)]

Skompiluj rozwiązanie i naciśnij klawisz F5, aby uruchomić aplikację lokalnie w emulatorze obliczeń platformy Azure. W zależności od ustawień zapory może być konieczne zezwolenie emulatora przez zaporę.

> [!NOTE]
> Jeśli wystąpi wyjątek podobny do poniższego, zapoznaj się z [tym wpisem w blogu](https://blogs.msdn.com/b/praburaj/archive/2013/11/20/fileloadexception-on-microsoft-owin-when-running-on-worker-role.aspx) , aby zapoznać się z obejściem. "Nie można załadować pliku lub zestawu" Microsoft. Owin, Version = 2.0.2.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35 "lub jednej z jego zależności. Zlokalizowana definicja manifestu zestawu nie odpowiada odwołaniu do zestawu. (Wyjątek od HRESULT: 0x80131040) "

Emulator obliczeń przypisuje lokalny adres IP do punktu końcowego. Adres IP można znaleźć, wyświetlając interfejs użytkownika emulatora obliczeń. Kliknij prawym przyciskiem myszy ikonę emulatora w obszarze powiadomień paska zadań, a następnie wybierz pozycję **Pokaż interfejs użytkownika emulatora obliczeń**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image11.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image10.png)

Znajdź adres IP w obszarze wdrożenia usługi, wdrożenie [ID], Szczegóły usługi. Otwórz przeglądarkę internetową i przejdź do<em>adresu</em>http:///test/1, gdzie <em>Address</em> jest adresem IP przypisanym przez emulator obliczeń; na przykład `http://127.0.0.1:80/test/1`. Powinna zostać wyświetlona odpowiedź z kontrolera internetowego interfejsu API:

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image12.png)

## <a name="deploy-to-azure"></a>Wdrażanie na platformie Azure

W tym kroku musisz mieć konto platformy Azure. Jeśli jeszcze tego nie masz, możesz utworzyć bezpłatne konto próbne w zaledwie kilka minut. Aby uzyskać szczegółowe informacje, zobacz [Microsoft Azure bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).

W Eksplorator rozwiązań kliknij prawym przyciskiem myszy projekt plik azureapp. Wybierz pozycję **Publikuj**.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image13.png)

Jeśli nie zalogowano się na koncie platformy Azure, kliknij przycisk **Zaloguj**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image15.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image14.png)

Po zalogowaniu wybierz subskrypcję i kliknij przycisk **dalej**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image17.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image16.png)

Wprowadź nazwę usługi w chmurze i wybierz region. Kliknij przycisk **Utwórz**.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image18.png)

Kliknij przycisk **Opublikuj**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image20.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image19.png)

W oknie Dziennik aktywności platformy Azure zostanie wyświetlony postęp wdrażania. Po wdrożeniu aplikacji przejdź do http://appname.cloudapp.net/test/1.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image21.png)

## <a name="additional-resources"></a>Dodatkowe materiały

- [Omówienie projektu Katana](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)
- [Projekt Katana w witrynie GitHub](https://github.com/aspnet/AspNetKatana)

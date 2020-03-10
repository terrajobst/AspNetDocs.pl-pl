---
uid: signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
title: Skalowania sygnalizujący z Azure Service Bus (Signaler 1. x) | Microsoft Docs
author: bradygaster
description: ''
ms.author: bradyg
ms.date: 05/01/2013
ms.assetid: 501db899-e68c-49ff-81b2-1dc561bfe908
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: e64f84db00b571c01ea52f48d1ac1af46698d391
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78558416"
---
# <a name="signalr-scaleout-with-azure-service-bus-signalr-1x"></a>SignalR — skalowanie w poziomie z użyciem usługi Azure Service Bus (SignalR 1.x)

według [Jan Wasson](https://github.com/MikeWasson), [Patryk Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

W tym samouczku zostanie wdrożona aplikacja sygnalizująca w roli sieci Web systemu Windows Azure, przy użyciu planu Service Bus do dystrybucji komunikatów do każdego wystąpienia roli.

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

Wymagania wstępne:

- Konto platformy Microsoft Azure.
- [Zestaw SDK systemu Windows Azure](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).
- Visual Studio 2012.

Plan usługi Service Bus jest również zgodny z [Service Bus dla systemu Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx)w wersji 1,1. Nie jest to jednak zgodne z wersją 1,0 Service Bus dla systemu Windows Server.

## <a name="pricing"></a>Ceny

W przypadku korzystania z programu do przesyłania komunikatów Service Bus Aby uzyskać najnowsze informacje o cenach, zobacz [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/). W czasie tego pisania możesz wysyłać 1 000 000 komunikatów miesięcznie przez mniej niż $1. Plan wyjścia wysyła komunikat usługi Service Bus dla każdego wywołania metody centrum sygnałów. Istnieją także pewne komunikaty sterujące dotyczące połączeń, rozłączeń, łączenia lub opuszczania grup itd. W większości aplikacji większość ruchu komunikatów będzie stanowić wywołania metody centrum.

## <a name="overview"></a>Omówienie

Zanim przejdziemy do szczegółowego samouczka, poniżej przedstawiono krótkie omówienie tego, co robisz.

1. Użyj Azure Portal systemu Windows, aby utworzyć nową przestrzeń nazw Service Bus.
2. Dodaj te pakiety NuGet do aplikacji: 

    - [Microsoft. AspNet. Signal](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft. AspNet. Signal. ServiceBus](http://www.nuget.org/packages/SignalR.WindowsAzureServiceBus)
3. Tworzenie aplikacji sygnalizującej.
4. Dodaj następujący kod do szablonu Global. asax, aby skonfigurować plan: 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

Dla każdej aplikacji należy wybrać inną wartość dla "YourAppName". Nie należy używać tej samej wartości w wielu aplikacjach.

## <a name="create-the-azure-services"></a>Tworzenie usług platformy Azure

Utwórz usługę w chmurze, zgodnie z opisem w temacie [jak utworzyć i wdrożyć usługę w chmurze](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy). Wykonaj kroki opisane w sekcji "Instrukcje: Tworzenie usługi w chmurze przy użyciu funkcji Szybkie tworzenie". Na potrzeby tego samouczka nie trzeba przekazywać certyfikatu.

![](scaleout-with-windows-azure-service-bus/_static/image2.png)

Utwórz nową przestrzeń nazw Service Bus, zgodnie z opisem w artykule [Service Bus tematy/subskrypcje](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions). Postępuj zgodnie z instrukcjami w sekcji "Tworzenie przestrzeni nazw usługi".

![](scaleout-with-windows-azure-service-bus/_static/image3.png)

> [!NOTE]
> Upewnij się, że wybrano ten sam region dla usługi w chmurze i przestrzeni nazw Service Bus.

## <a name="create-the-visual-studio-project"></a>Tworzenie projektu programu Visual Studio

Uruchom program Visual Studio. W menu **Plik** kliknij pozycję **Nowy projekt**.

W oknie dialogowym **Nowy projekt** rozwiń **element Wizualizacja C#** . W obszarze **zainstalowane szablony**wybierz pozycję **chmura** , a następnie wybierz pozycję **Usługa w chmurze platformy Microsoft Azure**. Zachowaj domyślne .NET Framework 4,5. Nadaj aplikacji nazwę ChatService, a następnie kliknij przycisk **OK**.

![](scaleout-with-windows-azure-service-bus/_static/image4.png)

W oknie dialogowym **Nowa usługa w chmurze platformy Microsoft Azure** wybierz rolę sieci Web ASP.NET MVC 4. Kliknij przycisk strzałki w prawo ( **&gt;** ), aby dodać rolę do rozwiązania.

Umieść wskaźnik myszy nad nową rolą, aby ikona ołówka była widoczna. Kliknij tę ikonę, aby zmienić nazwę roli. Nazwij rolę "SignalRChat" i kliknij przycisk **OK**.

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

W kreatorze **nowego projektu ASP.NET MVC 4** wybierz pozycję **aplikacja internetowa**. Kliknij przycisk **OK**. Kreator projektu tworzy dwa projekty:

- ChatService: ten projekt jest aplikacją systemu Windows Azure. Definiuje role platformy Azure i inne opcje konfiguracji.
- SignalRChat: ten projekt jest projektem ASP.NET MVC 4.

## <a name="create-the-signalr-chat-application"></a>Tworzenie aplikacji czatu dla sygnałów

Aby utworzyć aplikację czatu, wykonaj kroki opisane w samouczku [wprowadzenie z sygnałami i MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).

Zainstaluj wymagane biblioteki przy użyciu narzędzia NuGet. W menu **Narzędzia** wybierz pozycję **Menedżer pakietów NuGet**, a następnie wybierz pozycję **konsola Menedżera pakietów**. W oknie **konsola Menedżera pakietów** wprowadź następujące polecenia:

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

Użyj opcji `-ProjectName`, aby zainstalować pakiety w projekcie ASP.NET MVC, a nie w projekcie systemu Windows Azure.

## <a name="configure-the-backplane"></a>Konfigurowanie planu

W pliku Global. asax aplikacji Dodaj następujący kod:

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

Teraz musisz uzyskać parametry połączenia usługi Service Bus. W Azure Portal Wybierz utworzoną przestrzeń nazw usługi Service Bus, a następnie kliknij ikonę klucz dostępu.

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

Skopiuj parametry połączenia do schowka, a następnie wklej je do zmiennej *ConnectionString* .

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a>Wdrażanie na platformie Azure

W Eksplorator rozwiązań rozwiń folder **role** w projekcie ChatService.

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

Kliknij prawym przyciskiem myszy rolę SignalRChat i wybierz pozycję **Właściwości**. Wybierz kartę **Konfiguracja** . W obszarze **wystąpienia** wybierz pozycję 2. Rozmiar maszyny wirtualnej można również ustawić na **bardzo mały**.

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

Zapisz zmiany.

W Eksplorator rozwiązań kliknij prawym przyciskiem myszy projekt ChatService. Wybierz pozycję **Publikuj**.

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

Jeśli jest to pierwsze publikowanie na platformie Microsoft Azure, musisz pobrać swoje poświadczenia. W kreatorze **publikacji** kliknij pozycję "Zaloguj się, aby pobrać poświadczenia". Spowoduje to wyświetlenie monitu o zalogowanie się do Azure Portal systemu Windows i pobranie pliku ustawień publikowania.

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

Kliknij przycisk **Importuj** i wybierz pobrany plik ustawień publikowania.

Kliknij przycisk **Dalej**. W oknie dialogowym **Ustawienia publikowania** w obszarze **Usługa w chmurze**Wybierz utworzoną wcześniej usługę w chmurze.

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

Kliknij przycisk **Opublikuj**. Wdrożenie aplikacji i uruchomienie maszyn wirtualnych może potrwać kilka minut.

Teraz po uruchomieniu aplikacji czatu wystąpienia roli komunikują się za pośrednictwem Azure Service Bus przy użyciu Service Bus temacie. Temat jest kolejką komunikatów, która umożliwia korzystanie z wielu subskrybentów.

Plan "automatycznie" tworzy temat i subskrypcje. Aby zobaczyć działanie subskrypcje i komunikaty, Otwórz Azure Portal, wybierz przestrzeń nazw Service Bus i kliknij pozycję tematy.

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

Wyświetlenie działania komunikatu na pulpicie nawigacyjnym potrwa kilka minut.

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

Program sygnalizujący zarządza okresem istnienia tematu. Dopóki aplikacja jest wdrożona, nie próbuj ręcznie usuwać tematów ani zmieniać ustawień w temacie.

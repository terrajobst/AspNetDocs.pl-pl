---
uid: signalr/overview/performance/scaleout-with-windows-azure-service-bus
title: Skalowania sygnalizujący z Azure Service Bus | Microsoft Docs
author: bradygaster
description: Wersje oprogramowania używane w tym temacie Visual Studio 2013 programu .NET 4,5 sygnalizującego w wersji 2 poprzednie wersje tego tematu dla sygnalizującego 1. x wersji tego tematu,...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: ce1305f9-30fd-49e3-bf38-d0a78dfb06c3
msc.legacyurl: /signalr/overview/performance/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: 73ed95c5027f57c7e390069dcb36b18a3714973f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78579178"
---
# <a name="signalr-scaleout-with-azure-service-bus"></a>SignalR — skalowanie w poziomie z użyciem usługi Azure Service Bus

według [Jan Wasson](https://github.com/MikeWasson), [Patryk Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

W tym samouczku zostanie wdrożona aplikacja sygnalizująca w roli sieci Web systemu Windows Azure, przy użyciu planu Service Bus do dystrybucji komunikatów do każdego wystąpienia roli. (Można również użyć planu Service Bus z [aplikacjami sieci Web w programie Azure App Service](https://docs.microsoft.com/azure/app-service-web/)).

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

Wymagania wstępne:

- Konto platformy Microsoft Azure.
- [Zestaw SDK systemu Windows Azure](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).
- Program Visual Studio 2012 lub 2013.

Plan usługi Service Bus jest również zgodny z [Service Bus dla systemu Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx)w wersji 1,1. Nie jest to jednak zgodne z wersją 1,0 Service Bus dla systemu Windows Server.

## <a name="pricing"></a>Ceny

W przypadku korzystania z programu do przesyłania komunikatów Service Bus Aby uzyskać najnowsze informacje o cenach, zobacz [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/). W czasie tego pisania możesz wysyłać 1 000 000 komunikatów miesięcznie przez mniej niż $1. Plan wyjścia wysyła komunikat usługi Service Bus dla każdego wywołania metody centrum sygnałów. Istnieją także pewne komunikaty sterujące dotyczące połączeń, rozłączeń, łączenia lub opuszczania grup itd. W większości aplikacji większość ruchu komunikatów będzie stanowić wywołania metody centrum.

## <a name="overview"></a>Omówienie

Zanim przejdziemy do szczegółowego samouczka, poniżej przedstawiono krótkie omówienie tego, co robisz.

1. Użyj Azure Portal systemu Windows, aby utworzyć nową przestrzeń nazw Service Bus.
2. Dodaj te pakiety NuGet do aplikacji: 

    - [Microsoft. AspNet. Signal](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft. ASPNET. Signal. ServiceBus3](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus3) lub [Microsoft. ASPNET. Signal. ServiceBus](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus)
3. Tworzenie aplikacji sygnalizującej.
4. Dodaj następujący kod do Startup.cs w celu skonfigurowania planu: 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

Ten kod konfiguruje plan z wartościami domyślnymi dla [TopicCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.servicebusscaleoutconfiguration.topiccount(v=vs.118).aspx) i [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx). Aby uzyskać informacje na temat zmiany tych wartości, zobacz [wydajność sygnałów: metryki skalowania](signalr-performance.md#scaleout_metrics).

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

W oknie dialogowym **Nowa usługa w chmurze platformy Microsoft Azure** wybierz pozycję ASP.NET Web role. Kliknij przycisk strzałki w prawo ( **&gt;** ), aby dodać rolę do rozwiązania.

Umieść wskaźnik myszy nad nową rolą, aby ikona ołówka była widoczna. Kliknij tę ikonę, aby zmienić nazwę roli. Nazwij rolę "SignalRChat" i kliknij przycisk **OK**.

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

W oknie dialogowym **Nowy projekt ASP.NET** wybierz pozycję **MVC**, a następnie kliknij przycisk OK.

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

Kreator projektu tworzy dwa projekty:

- ChatService: ten projekt jest aplikacją systemu Windows Azure. Definiuje role platformy Azure i inne opcje konfiguracji.
- SignalRChat: ten projekt jest projektem ASP.NET MVC 5.

## <a name="create-the-signalr-chat-application"></a>Tworzenie aplikacji czatu dla sygnałów

Aby utworzyć aplikację czatu, wykonaj kroki opisane w samouczku [wprowadzenie z sygnałami i MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).

Zainstaluj wymagane biblioteki przy użyciu narzędzia NuGet. W menu **Narzędzia** wybierz pozycję **Menedżer pakietów NuGet**, a następnie wybierz pozycję **konsola Menedżera pakietów**. W oknie **konsola Menedżera pakietów** wprowadź następujące polecenia:

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

Użyj opcji `-ProjectName`, aby zainstalować pakiety w projekcie ASP.NET MVC, a nie w projekcie systemu Windows Azure.

## <a name="configure-the-backplane"></a>Konfigurowanie planu

W pliku Startup.cs aplikacji Dodaj następujący kod:

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

Teraz musisz uzyskać parametry połączenia usługi Service Bus. W Azure Portal Wybierz utworzoną przestrzeń nazw usługi Service Bus, a następnie kliknij ikonę klucz dostępu.

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

Skopiuj parametry połączenia do schowka, a następnie wklej je do zmiennej *ConnectionString* .

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a>Wdrażanie na platformie Azure

W Eksplorator rozwiązań rozwiń folder **role** w projekcie ChatService.

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

Kliknij prawym przyciskiem myszy rolę SignalRChat i wybierz pozycję **Właściwości**. Wybierz kartę **Konfiguracja** . W obszarze **wystąpienia** wybierz pozycję 2. Rozmiar maszyny wirtualnej można również ustawić na **bardzo mały**.

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

Zapisz zmiany.

W Eksplorator rozwiązań kliknij prawym przyciskiem myszy projekt ChatService. Wybierz pozycję **Publikuj**.

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

Jeśli jest to pierwsze publikowanie na platformie Microsoft Azure, musisz pobrać swoje poświadczenia. W kreatorze **publikacji** kliknij pozycję "Zaloguj się, aby pobrać poświadczenia". Spowoduje to wyświetlenie monitu o zalogowanie się do Azure Portal systemu Windows i pobranie pliku ustawień publikowania.

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

Kliknij przycisk **Importuj** i wybierz pobrany plik ustawień publikowania.

Kliknij przycisk **Dalej**. W oknie dialogowym **Ustawienia publikowania** w obszarze **Usługa w chmurze**Wybierz utworzoną wcześniej usługę w chmurze.

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

Kliknij przycisk **Opublikuj**. Wdrożenie aplikacji i uruchomienie maszyn wirtualnych może potrwać kilka minut.

Teraz po uruchomieniu aplikacji czatu wystąpienia roli komunikują się za pośrednictwem Azure Service Bus przy użyciu Service Bus temacie. Temat jest kolejką komunikatów, która umożliwia korzystanie z wielu subskrybentów.

Plan "automatycznie" tworzy temat i subskrypcje. Aby zobaczyć działanie subskrypcje i komunikaty, Otwórz Azure Portal, wybierz przestrzeń nazw Service Bus i kliknij pozycję tematy.

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

Wyświetlenie działania komunikatu na pulpicie nawigacyjnym potrwa kilka minut.

![](scaleout-with-windows-azure-service-bus/_static/image15.png)

Program sygnalizujący zarządza okresem istnienia tematu. Dopóki aplikacja jest wdrożona, nie próbuj ręcznie usuwać tematów ani zmieniać ustawień w temacie.

## <a name="troubleshooting"></a>Rozwiązywanie problemów

**System. InvalidOperationException "jedyne obsługiwane IsolationLevel to" IsolationLevel. Serializable "."**

Ten błąd może wystąpić, jeśli poziom transakcji dla operacji jest ustawiony na coś innego niż `Serializable`. Sprawdź, czy żadne operacje nie są wykonywane z innymi poziomami transakcji.

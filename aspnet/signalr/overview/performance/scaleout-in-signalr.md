---
uid: signalr/overview/performance/scaleout-in-signalr
title: Wprowadzenie do skalowania w usłudze Signaler | Microsoft Docs
author: bradygaster
description: Wersje oprogramowania używane w tym temacie Visual Studio 2013 programu .NET 4,5 sygnalizującego w wersji 2 poprzednie wersje tego tematu, aby uzyskać informacje o wcześniejszych wersjach programu...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: 7e781fc1-1c1f-45a8-bc1d-338e96dbe9c9
msc.legacyurl: /signalr/overview/performance/scaleout-in-signalr
msc.type: authoredcontent
ms.openlocfilehash: 14dc22f99a43b566903c59fb23b7d419350f4a25
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78579234"
---
# <a name="introduction-to-scaleout-in-signalr"></a>Wprowadzenie do skalowania w poziomie w usłudze SignalR

według [Jan Wasson](https://github.com/MikeWasson), [Patryk Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> ## <a name="software-versions-used-in-this-topic"></a>Wersje oprogramowania używane w tym temacie
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - Sygnalizujący wersja 2
>
>
>
> ## <a name="previous-versions-of-this-topic"></a>Poprzednie wersje tego tematu
>
> Aby uzyskać informacje o wcześniejszych wersjach programu sygnalizującego, zobacz sekcję [sygnalizujące starsze wersje](../older-versions/index.md).
>
> ## <a name="questions-and-comments"></a>Pytania i Komentarze
>
> Prosimy o opinię na temat sposobu, w jaki lubię ten samouczek, i co możemy ulepszyć w komentarzach w dolnej części strony. Jeśli masz pytania, które nie są bezpośrednio związane z samouczkiem, możesz je ogłosić na [forum ASP.NET](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).

Ogólnie rzecz biorąc, istnieją dwa sposoby skalowania aplikacji sieci Web: *skalowanie w górę* i w *poziomie*.

- Skalowanie w górę oznacza użycie większego serwera (lub większej maszyny wirtualnej) z większą ilością pamięci RAM, procesorów CPU itd.
- Skalowanie w poziomie oznacza dodanie większej liczby serwerów do obsługi obciążenia.

Problem z skalowaniem w górę polega na tym, że szybko osiągniesz limit rozmiaru maszyny. Poza tym, należy skalować w poziomie. Jednak podczas skalowania w poziomie klienci mogą być kierowani do różnych serwerów. Klient połączony z jednym serwerem nie będzie odbierać komunikatów wysyłanych z innego serwera.

![](scaleout-in-signalr/_static/image1.png)

Jednym z rozwiązań jest przekazanie komunikatów między serwerami przy użyciu składnika zwanego *planem*. Po włączeniu obsługi planów, każde wystąpienie aplikacji wysyła komunikaty do planu i planuje je do innych wystąpień aplikacji. (W obszarze elektroniki plan jest grupą łączników równoległych. Analogowo, plan sygnalizujący łączy wiele serwerów.

![](scaleout-in-signalr/_static/image2.png)

Program sygnalizujący obecnie udostępnia trzy plany:

- **Azure Service Bus**. Service Bus to infrastruktura obsługi komunikatów, która umożliwia składnikom wysyłanie komunikatów w sposób swobodny.
- **Redis**. Redis jest magazynem wartości w pamięci. Redis obsługuje wzorzec publikowania/subskrybowania ("pub/Sub") do wysyłania wiadomości.
- **SQL Server**. SQL Server planuje zapisywanie komunikatów w tabelach SQL. Plan nie używa Service Broker do wydajnej obsługi komunikatów. Jednak działa również wtedy, gdy Service Broker nie jest włączona.

W przypadku wdrażania aplikacji na platformie Azure należy rozważyć użycie [Azure Redis Cache](https://azure.microsoft.com/services/cache/)planu Redis. W przypadku wdrażania w ramach własnej farmy serwerów należy wziąć pod uwagę SQL Server lub plan Redis.

Poniższe tematy zawierają Samouczki krok po kroku dotyczące poszczególnych planów:

- [SignalR — skalowanie w poziomie z użyciem usługi Azure Service Bus](scaleout-with-windows-azure-service-bus.md)
- [SignalR — skalowanie w poziomie z użyciem pamięci podręcznej Redis](scaleout-with-redis.md)
- [SignalR — skalowanie w poziomie z użyciem programu SQL Server](scaleout-with-sql-server.md)

## <a name="implementation"></a>Wdrażanie

W programie sygnalizujący każdy komunikat jest wysyłany przez magistralę komunikatów. Magistrala komunikatów implementuje interfejs [IMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.imessagebus(v=vs.100).aspx) , który zapewnia abstrakcję publikowania/subskrybowania. Planuje działanie, zastępując domyślne **IMessageBus** magistralą zaprojektowaną dla tego planu. Na przykład magistrala komunikatów dla Redis jest [RedisMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.redis.redismessagebus(v=vs.100).aspx)i używa mechanizmu Redis [pub/sub](http://redis.io/topics/pubsub) do wysyłania i odbierania komunikatów.

Każde wystąpienie serwera nawiązuje połączenie z planem w celu przechodzenia przez magistralę. Po wysłaniu wiadomości zostanie ona zaplanowana, a plan nie wysyła go do każdego serwera. Gdy serwer pobiera komunikat z planu, umieszcza komunikat w lokalnej pamięci podręcznej. Serwer następnie dostarcza komunikaty do klientów ze swojej lokalnej pamięci podręcznej.

W przypadku każdego połączenia z klientem postęp odczytywania strumienia komunikatów jest śledzony przy użyciu kursora. (Kursor reprezentuje pozycję w strumieniu wiadomości). Jeśli klient odłączy się i ponownie nawiąże połączenie, prosi magistralę o wszelkie komunikaty, które dotarły po wartości kursora klienta. Taka sama sytuacja ma miejsce, gdy połączenie używa [długich sondowania](../getting-started/introduction-to-signalr.md#transports). Po zakończeniu długotrwałego żądania sondowania klient otworzy nowe połączenie i wyświetli monit o podanie komunikatów, które dotarły po przejściu do kursora.

Mechanizm kursora działa nawet wtedy, gdy klient jest kierowany do innego serwera po ponownym nawiązaniu połączenia. Plan w planie IT jest świadomy wszystkich serwerów i nie ma znaczenia serwera, z którym łączy się klient.

## <a name="limitations"></a>Ograniczenia

W przypadku korzystania z planu wielowartościowego maksymalna przepływność komunikatów jest niższa niż w przypadku, gdy klienci komunikują się bezpośrednio z pojedynczym węzłem serwera. Dzieje się tak, ponieważ plan w przód przesyła każdy komunikat do każdego węzła, dzięki czemu plan może stać się wąskim gardłem. Czy to ograniczenie jest problemem zależnie od aplikacji. Na przykład poniżej przedstawiono kilka typowych scenariuszy sygnalizujących:

- [Emisja serwera](../getting-started/tutorial-server-broadcast-with-signalr.md) (np. giełdowego): plany pracy dla tego scenariusza są odpowiednie, ponieważ serwer kontroluje szybkość, z jaką wysyłane są komunikaty.
- [Klient-Klient](../getting-started/tutorial-getting-started-with-signalr.md) (np. rozmowa): w tym scenariuszu plan może być wąskim gardłem, jeśli liczba komunikatów skaluje się do liczby klientów. oznacza to, że jeśli szybkość komunikatów rośnie proporcjonalnie do przyłączenia większej liczby klientów.
- [Wysoka częstotliwość](../getting-started/tutorial-high-frequency-realtime-with-signalr.md) w czasie rzeczywistym (np. w grach czasu rzeczywistego): w tym scenariuszu nie zaleca się planowania.

## <a name="enabling-tracing-for-signalr-scaleout"></a>Włączanie śledzenia dla sygnałów skalowania

Aby włączyć śledzenie dla planów, Dodaj następujące sekcje do pliku Web. config w elemencie **konfiguracji** głównej:

[!code-html[Main](scaleout-in-signalr/samples/sample1.html)]

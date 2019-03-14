---
uid: aspnet/overview/owin-and-katana/katana-samples
title: Przykłady Katana | Dokumentacja firmy Microsoft
author: microsoft
description: ''
ms.author: riande
ms.date: 01/17/2014
ms.assetid: bec04f5d-2638-4417-b288-97c58c8d6379
msc.legacyurl: /aspnet/overview/owin-and-katana/katana-samples
msc.type: authoredcontent
ms.openlocfilehash: b8ce2b40a19e0429f1ccedb03b8f829582652d24
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57075581"
---
<a name="katana-samples"></a>Przykłady projektu Katana
====================
przez [firmy Microsoft](https://github.com/microsoft)

## <a name="katana-samples"></a>Przykłady projektu Katana

**ASP.NET kieruje przykładowe** | [kodu źródłowego](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/AspNetRoutes)  
W niektórych aplikacjach można dołączyć składniki OWIN w tabeli trasy Asp.Net równolegle składniki bez OWIN. Ten przykład pokazuje, jak użyć metod rozszerzających RouteCollection MapOwinPath i udostępniane przez Microsoft.Owin.Host.SystemWeb MapOwinRoute.

**Rozgałęzianie potoków przykładowe** | [kodu źródłowego](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/BranchingPipelines)  
Potoki przetwarzania żądania OWIN czy muszą być liniowego, może zostać rozgałęzione do przetwarzania żądań w różny sposób. W tym przykładzie pokazano, jak do budowy potoku rozgałęziania, na podstawie ścieżki żądania lub inne dane na żądanie, takich jak nagłówki. Te składniki są dostępne w pakiecie nuget Microsoft.Owin.Mapping.

**Przykład niestandardowych Server** | [kodu źródłowego](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/CustomServer)   
Pokazuje, jak użyć niestandardowego serwera OWIN, gdy hostingu samodzielnego OWIN.

**Przykładowy osadzony** | [kodu źródłowego](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/Embedded)  
Niektóre serwery OWIN mogą być uruchamiane wewnątrz własnego procesu (&quot;może być samodzielnie hostowane&quot;). W tym przykładzie pokazano, jak można uruchomić za pomocą narzędzi dostarczanych przez pakiet nuget Microsoft.Owin.Hosting aplikacji OWIN.

**Przykładowe HelloWorld** | [kodu źródłowego](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorld)  
OWIN to serwer HTTP abstrakcji interfejsu API, który umożliwia przenoszenia aplikacji na różnych serwerach. W tym przykładzie pokazano, jak napisać aplikację Hello World w niektórych **proste otoki** wokół nieprzetworzone abstrakcji OWIN i uruchom go na serwerze sieci web takich jak ASP.NET.

**Przykładowa aplikacja Hello World pierwotne OWIN** | [kodu źródłowego](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorldRawOwin)  
W tym przykładzie przedstawiono sposób pisania aplikacji Hello World za pomocą **pierwotne** abstrakcji OWIN i uruchom go na serwerze sieci web, takimi jak Asp.Net.

**Przykładowe SignalR** | [kodu źródłowego](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/SignalR)  
Pokazuje, jak na potrzeby samodzielnego hostowania SignalR przy użyciu OWIN / Katana. Aby uzyskać więcej informacji na temat biblioteki SignalR własnym hostingu, zobacz [samouczka: Host samodzielny SignalR](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

**Statyczne pliki przykładowe** | [kodu źródłowego](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/StaticFilesSample)   
Pokazuje, jak do obsługi żądań HTTP dla plików statycznych przy użyciu OWIN / Katana.

**Interfejs API sieci Web** | [kodu źródłowego](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebApi)   
W tym przykładzie pokazano, jak hostowanie OWIN w usługach IIS, a następnie Dodaj interfejs API sieci Web do potoku OWIN.

**Sieci Web przykładowej gniazda** | [kodu źródłowego](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebSocketSample)   
Pokazuje, jak do obsługi gniazda sieci Web w OWIN przy użyciu [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) klasy.

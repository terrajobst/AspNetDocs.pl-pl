---
uid: aspnet/overview/owin-and-katana/katana-samples
title: Przykłady Katana | Microsoft Docs
author: microsoft
description: ''
ms.author: riande
ms.date: 01/17/2014
ms.assetid: bec04f5d-2638-4417-b288-97c58c8d6379
msc.legacyurl: /aspnet/overview/owin-and-katana/katana-samples
msc.type: authoredcontent
ms.openlocfilehash: 1238f7d09492a6856d49dece5de75184ccfa4838
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78584561"
---
# <a name="katana-samples"></a>Przykłady projektu Katana

przez [firmę Microsoft](https://github.com/microsoft)

## <a name="katana-samples"></a>Przykłady projektu Katana

**Przykładowe trasy ASP.NET** | [kod źródłowy](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/AspNetRoutes)  
W niektórych aplikacjach chcesz podłączyć składniki OWIN w tabeli tras Asp.Net obok siebie ze składnikami nieOWIN. Ten przykład pokazuje, jak używać metod rozszerzenia RouteCollection MapOwinPath i MapOwinRoute dostarczonych przez firmę Microsoft. Owin. host. SystemWeb.

**Przykład rozgałęziania potoków** | [kod źródłowy](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/BranchingPipelines)  
Potoki przetwarzania żądań OWIN nie muszą być liniowe, mogą być rozgałęzienia na żądania procesów na różne sposoby. Ten przykład pokazuje, jak utworzyć potok rozgałęzienia na podstawie ścieżek żądań lub innych danych żądania, takich jak nagłówki. Te składniki są dostępne w pakiecie NuGet Microsoft. Owin. Mapping.

**Niestandardowy przykładowy serwer** | [kod źródłowy](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/CustomServer)   
Pokazuje, jak używać niestandardowego serwera OWIN podczas samodzielnego hostowania OWIN.

**Osadzony przykładowy** | [kod źródłowy](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/Embedded)  
Niektóre serwery OWIN można uruchamiać w ramach własnego procesu (&quot;samodzielnie hostowane&quot;). Ten przykład pokazuje, jak uruchomić aplikację OWIN przy użyciu narzędzi dostarczonych przez pakiet NuGet Microsoft. Owin. host.

**Przykładowy** [kod źródłowy](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorld) | HelloWorld  
OWIN to Abstrakcja interfejsu API serwera HTTP, która umożliwia przenoszenie aplikacji na różnych serwerach. W tym przykładzie pokazano, jak napisać aplikację Hello world przy użyciu kilku **prostych otok** wokół Owin abstrakcji RAW i uruchomić ją na serwerze sieci Web, takim jak ASP.NET.

**Hello World Raw | przykład** [kodu źródłowego](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorldRawOwin) Owin  
W tym przykładzie pokazano, jak napisać aplikację Hello world przy użyciu Owin abstrakcji **RAW** i uruchomić ją na serwerze sieci Web, takim jak ASP.NET.

Przykładowy [kod źródłowy](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/SignalR) **próbnika** |   
Pokazuje, w jaki sposób do samoobsługowego sygnalizowania przy użyciu OWIN/Katana. Aby uzyskać więcej informacji na temat sygnalizacji samoobsługowej, zobacz [Samouczek: program do](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)samoobsługowego hostowania.

**Pliki statyczne przykładowe** | [kodu źródłowego](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/StaticFilesSample)   
Pokazuje, jak obsługiwać żądania HTTP dla plików statycznych przy użyciu OWIN/Katana.

[Kod źródłowy](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebApi) | internetowego **interfejsu API**   
Ten przykład pokazuje, jak hostować OWIN w usługach IIS i dodawać interfejs API sieci Web do potoku OWIN.

Przykładowy [kod źródłowy](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebSocketSample) | **gniazda internetowego**   
Pokazuje, jak obsługiwać gniazda sieci Web w programie OWIN przy użyciu klasy [System .NET. WebSockets. WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) .

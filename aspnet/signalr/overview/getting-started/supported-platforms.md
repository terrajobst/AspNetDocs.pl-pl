---
uid: signalr/overview/getting-started/supported-platforms
title: Obsługiwane platformy | Microsoft Docs
author: bradygaster
description: W tym artykule opisano, co klienci i serwery są obsługiwane przez program sygnalizujący.
ms.author: bradyg
ms.date: 04/18/2018
ms.assetid: eac31beb-0f46-4afa-9def-e80904dea4f0
msc.legacyurl: /signalr/overview/getting-started/supported-platforms
msc.type: authoredcontent
ms.openlocfilehash: 89730e591bb94022d16fe1a78a4350b38e0bf7a4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78558633"
---
# <a name="supported-platforms"></a>Obsługiwane platformy

[Fletcher Patryka](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> W tym artykule opisano, co klienci i serwery są obsługiwane przez program sygnalizujący. 
> 
> ## <a name="questions-and-comments"></a>Pytania i Komentarze
> 
> Prosimy o opinię na temat sposobu, w jaki lubię ten samouczek, i co możemy ulepszyć w komentarzach w dolnej części strony. Jeśli masz pytania, które nie są bezpośrednio związane z samouczkiem, możesz je ogłosić na [forum ASP.NET](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).

Sygnał jest obsługiwany w różnych konfiguracjach serwera i klienta. Ponadto każda opcja transportu ma zestaw własnych wymagań; Jeśli wymagania systemowe dotyczące transportu nie są dostępne, sygnał zostanie bezpiecznie przejdzie w tryb failover do innych transportów. Aby uzyskać więcej informacji na temat transportów obsługiwanych przez program sygnalizujący, zobacz [Transports and fallbacks](introduction-to-signalr.md#transports).

## <a name="server-system-requirements"></a>Wymagania systemowe serwera

Składnik serwera sygnalizującego może być hostowany w różnych konfiguracjach serwera. W tej sekcji opisano obsługiwane wersje systemów operacyjnych, .NET Framework, Internet Information Server i inne składniki.

### <a name="supported-server-operating-systems"></a>Obsługiwane systemy operacyjne serwera

Składnik serwera sygnalizującego może być hostowany w następujących systemach operacyjnych serwera lub klienta. Należy pamiętać, że w przypadku programu sygnalizującego używanie obiektów WebSockets, Windows Server 2012, Windows Server 2016 lub Windows 8 jest wymagane (protokół WebSocket może być używany w witrynach sieci Web systemu Windows Azure, o ile wersja .NET Framework lokacji jest ustawiona na 4,5, a gniazda sieci Web są włączone w lokacji Strona konfiguracja).

- Windows Server 2016
- Windows Server 2012
- System Windows Server 2008 R2
- Windows 10
- Windows 8
- Windows 7
- Windows Azure

### <a name="supported-server-net-framework-version"></a>Obsługiwana wersja .NET Framework serwera

Sygnał 2 jest obsługiwany tylko w .NET Framework 4,5. Zapoznaj się z sekcją [zalecane aktualizacje](#updates) , aby uzyskać aktualizacje, które zwiększają niezawodność, zgodność, stabilność i wydajność.

### <a name="supported-server-iis-versions"></a>Obsługiwane wersje programu IIS serwera

Gdy usługa sygnalizująca jest hostowana w usługach IIS, obsługiwane są następujące wersje. Należy pamiętać, że jeśli używany jest system operacyjny klienta, na przykład na potrzeby programowania (system Windows 8 lub Windows 7), nie należy używać pełnych wersji usług IIS lub Cassini, ponieważ zostanie przekroczony limit 10 jednoczesnych połączeń, które zostaną nałożone bardzo szybko od połączeń są przejściowe, często ponownie ustanawiane i nie są usuwane natychmiast, gdy nie są już używane. IIS Express należy używać w systemach operacyjnych klienta.

Należy również pamiętać, że w przypadku programu sygnalizującego użycie protokołu WebSocket, usługi IIS 8 lub IIS 8 Express muszą być używane, serwer musi używać systemu Windows 8, Windows Server 2012 lub nowszego, a protokół WebSocket musi być włączony w usługach IIS. Aby uzyskać informacje na temat włączania protokołu WebSocket w usługach IIS, zobacz [Obsługa protokołu WebSocket usług iis 8,0](https://www.iis.net/learn/get-started/whats-new-in-iis-8/iis-80-websocket-protocol-support).

- IIS 8 lub IIS 8 Express.
- Usługi IIS 7 i 7,5. Wymagana jest obsługa [adresów URL bezrozszerzenia](https://support.microsoft.com/kb/980368) .
- Usługi IIS muszą działać w trybie zintegrowanym; Tryb klasyczny nie jest obsługiwany. Opóźnienia komunikatów do 30 sekund mogą wystąpić, jeśli usługi IIS są uruchamiane w trybie klasycznym przy użyciu transportu zdarzeń wysłanych przez serwer.
- Aplikacja hostingu musi działać w trybie pełnego zaufania.

## <a name="client-system-requirements"></a>Wymagania systemowe klienta

Można używać sygnałów w różnych platformach klienckich. W tej sekcji opisano wymagania systemowe dotyczące korzystania z programu sygnalizującego w przeglądarkach sieci Web, aplikacjach klasycznych systemu Windows, aplikacjach Silverlight i urządzeniach przenośnych.

### <a name="web-browsers"></a>Przeglądarki internetowe

Program sygnalizujący może być używany w różnych przeglądarkach sieci Web, ale zazwyczaj obsługiwane są tylko najnowsze wersje.

Aplikacje korzystające z programu sygnalizującego w przeglądarkach muszą używać systemu jQuery w wersji 1.6.4 lub większych wersji (takich jak 1.7.2, 1.8.2 lub 1.9.1).

Sygnalizujący może być używany w następujących przeglądarkach:

- Microsoft Internet Explorer w wersji 8, 9, 10 i 11. Obsługiwane są wersje nowoczesne, klasyczne i mobilne.
- Mozilla Firefox: bieżąca wersja — 1, wersje systemu Windows i Mac.
- Google Chrome: bieżąca wersja — 1, wersje systemu Windows i Mac.
- Safari: bieżąca wersja — 1, wersje dla komputerów Mac i iOS.
- Opera: bieżąca wersja — 1, tylko system Windows.
- Przeglądarka systemu Android

Oprócz wymagania niektórych przeglądarek, różne transporty używane przez sygnalizujących mają własne wymagania. Następujące transporty są obsługiwane w ramach następujących konfiguracji:

<a id="browser"></a>

**Wymagania dotyczące transportu przeglądarki sieci Web**

| Transport | Internet Explorer | Chrome (system Windows lub iOS) | Firefox | Safari (OSX lub iOS) | Android |
| --- | --- | --- | --- | --- | --- |
| Protokoły WebSocket | 10+ | Bieżąca-1 | Bieżąca-1 | Bieżąca-1 | N/D |
| Zdarzenia wysłane przez serwer | N/D | Bieżąca-1 | Bieżąca-1 | Bieżąca-1 | N/D |
| ForeverFrame | 8+ | N/D | N/D | N/D | 4.1 |
| Długotrwałe sondowanie | 8+ | Bieżąca-1 | Bieżąca-1 | Bieżąca-1 | 4.1 |

\*: 6 + wymagane w celu zapewnienia pełnej funkcjonalności.

#### <a name="unsupported-browsers"></a>Nieobsługiwane przeglądarki

Mimo że sygnalizujący *może* działać bez poważnych problemów w starszych wersjach przeglądarki, firma Microsoft nie będzie aktywnie testować sygnałów sygnalizujących i ogólnie nie usuwa błędów, które mogą się pojawić.

### <a name="windows-desktop-and-silverlight-applications"></a>Aplikacje klasyczne i Silverlight dla systemu Windows

Program sygnalizujący może być obsługiwany w przeglądarce internetowej autonomicznej lub aplikacji Silverlight. Aplikacje pulpitu i programu Silverlight dla systemu Windows mają następujące wymagania systemowe.

- Aplikacje korzystające z platformy .NET 4 są obsługiwane w systemie Windows XP z dodatkiem SP3 lub nowszym.
- Aplikacje korzystające z .NET Framework 4,5 są obsługiwane w systemie Windows Vista lub nowszym.

Oprócz wymagań systemu operacyjnego i programu .NET Framework Transporty dostępne dla sygnalizującego mają własne wymagania. Następujące transporty są obsługiwane w ramach następujących konfiguracji:

**Wymagania dotyczące transportu dla komputerów stacjonarnych i programu Silverlight w systemie Windows**

| Transport | Aplikacja .NET | Silverlight |
| --- | --- | --- |
| Gniazda sieci Web | Windows 8 + i .NET 4.5 + | N/D |
| Ramka bez ograniczeń | N/D | N/D |
| Zdarzenia wysłane przez serwer | .NET 4+ | 5+ |
| Długotrwałe sondowanie | .NET 4+ | 5+ |

<a id="android"></a>

### <a name="windows-store-and-windows-phone-applications"></a>Aplikacje ze sklepu Windows i Windows Phone

System sygnalizujący może być używany w aplikacjach do sklepu Windows i w aplikacjach Windows Phone 8. Następujące transporty są obsługiwane w ramach następujących konfiguracji:

**Wymagania dotyczące transportu sklepu Windows i Windows Phone**

| Transport | Windows Store/ .NET | Sklep Windows/JavaScript | Windows Phone/IE | Windows Phone/.NET |
| --- | --- | --- | --- | --- |
| Protokoły WebSocket | N/D | Win8+ | 8+ | N/D |
| Ramka bez ograniczeń | N/D | Win8+ | 7.5+ | N/D |
| Zdarzenia wysłane przez serwer | Win8+ | N/D | N/D | 8+ |
| Długotrwałe sondowanie | Win8+ | Win8+ | 7.5+ | 8+ |

<a id="updates"></a>

## <a name="recommended-updates"></a>Zalecane aktualizacje

Dla serwerów sygnalizujących zaleca się wykonanie następujących aktualizacji:

- Aktualizacja dla .NET Framework 4,5 jest dostępna [tutaj](https://support.microsoft.com/kb/2750149).
- Firma Microsoft okresowo wyda poprawki QFE dla ASP.NET. Te dane powinny być stosowane jako dostępne.

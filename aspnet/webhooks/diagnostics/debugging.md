---
uid: webhooks/diagnostics/debugging
title: Elementy Webhook ASP.NET debugowanie | Dokumentacja firmy Microsoft
author: rick-anderson
description: Jak debugować elementów Webhook programu ASP.NET.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 467da78b-3c35-4c51-8b08-77a32379e4a8
ms.openlocfilehash: 517d282fc22703b5861b748aea51023fa0a12a26
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57077411"
---
# <a name="aspnet-webhooks-debugging"></a>Elementy Webhook ASP.NET debugowania  

## <a name="debugging-in-azure"></a>Debugowanie w systemie Azure

Aby debugować aplikację sieci Web podczas uruchamiania na platformie Azure, zobacz samouczek [Rozwiązywanie problemów z aplikacją sieci web w usłudze Azure App Service przy użyciu programu Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).

## <a name="debugging-with-source-and-symbols"></a>Debugowanie przy użyciu źródła i symboli

Oprócz debugowania własnego kodu, jest możliwe debugowanie bezpośrednio do programu Microsoft ASP.NET WebHooks, a w rzeczywistości wszystkie platformy .NET. To działa, niezależnie od tego, czy debugujesz lokalnie lub zdalnie. Najpierw należy skonfigurować program Visual Studio można znaleźć, przechodząc do źródła i symboli **debugowania** i następnie **opcje i ustawienia**. Ustaw opcje następująco:

![Opcje i ustawienia](_static/SourceSymbols.png)

Następnie dodaj link do [symbolsource.org](http://symbolsource.org) pobierania źródła i symboli. Przejdź do **symbole** kartę menu powyżej i Dodaj następujący kod jako lokalizację symboli:

```
http://srv.symbolsource.org/pdb/Public
```

Ponadto upewnij się, że katalog pamięci podręcznej ma krótką nazwę; w przeciwnym razie nazwy plików można uzyskać za długa co spowoduje symboli, które nie były ładowane. Ścieżka próbki jest:

```
C:\SymCache
```

Ustawienia powinny wyglądać następująco:

![Przykład lokalizacji pliku symboli opcje](_static/SymSource.png)

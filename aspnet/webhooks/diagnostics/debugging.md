---
uid: webhooks/diagnostics/debugging
title: Debugowanie elementów webhook ASP.NET | Microsoft Docs
author: rick-anderson
description: Jak debugować elementy webhook ASP.NET.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 467da78b-3c35-4c51-8b08-77a32379e4a8
ms.openlocfilehash: 517d282fc22703b5861b748aea51023fa0a12a26
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78640869"
---
# <a name="aspnet-webhooks-debugging"></a>Debugowanie elementów webhook ASP.NET  

## <a name="debugging-in-azure"></a>Debugowanie na platformie Azure

Aby debugować aplikację sieci Web podczas działania na platformie Azure, zobacz samouczek [Rozwiązywanie problemów z aplikacją sieci Web w Azure App Service przy użyciu programu Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).

## <a name="debugging-with-source-and-symbols"></a>Debugowanie ze źródłem i symbolami

Oprócz debugowania własnego kodu możliwe jest debugowanie bezpośrednio do Microsoft ASP.NET elementów webhook, a w rzeczywistości wszystkie platformy .NET. Działa to niezależnie od tego, czy debugowanie odbywa się lokalnie, czy zdalnie. Najpierw należy skonfigurować program Visual Studio do znajdowania źródła i symboli, przechodząc do **debugowania** , a następnie **Opcje i ustawienia**. Ustaw opcje w następujący sposób:

![Opcje i ustawienia](_static/SourceSymbols.png)

Następnie Dodaj link do [symbolsource.org](http://symbolsource.org) w celu pobrania źródła i symboli. Przejdź do karty **symbole** w powyższym menu i Dodaj następujący element jako lokalizację symboli:

```
http://srv.symbolsource.org/pdb/Public
```

Ponadto upewnij się, że Katalog pamięci podręcznej ma krótką nazwę; w przeciwnym razie nazwy plików mogą być zbyt długie, co spowoduje, że symbole nie zostaną załadowane. Przykładowa ścieżka:

```
C:\SymCache
```

Ustawienia powinny wyglądać podobnie do tego:

![Przykład lokalizacji pliku symboli opcji](_static/SymSource.png)

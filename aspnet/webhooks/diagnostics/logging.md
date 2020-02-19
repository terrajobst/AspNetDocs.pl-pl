---
uid: webhooks/diagnostics/logging
title: Rejestrowanie elementów webhook ASP.NET | Microsoft Docs
author: rick-anderson
description: Jak rejestrować się w elementach webhook ASP.NET.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: f71bc442-5f80-481b-a32c-a0ec18dee9d6
ms.openlocfilehash: a05b32c4a8f9577bcf6170bd19a9e440b1aeb75b
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457586"
---
# <a name="aspnet-webhooks-logging"></a>Rejestrowanie elementów webhook ASP.NET

Microsoft ASP.NET elementy webhook używają rejestrowania w celu zgłaszania problemów i problemów. Dzienniki domyślne są zapisywane przy użyciu funkcji [System. Diagnostics. Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) , gdzie mogą być zarządzane przy użyciu [detektorów śledzenia](https://msdn.microsoft.com/library/system.diagnostics.tracelistener.aspx) , takich jak każdy inny strumień dzienników.

Podczas wdrażania aplikacji sieci Web jako aplikacji sieci Web platformy Azure dzienniki są automatycznie pobierane i można nimi zarządzać razem z innymi dziennikami [System. Diagnostics. Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) . Aby uzyskać szczegółowe informacje, zobacz [Włączanie rejestrowania diagnostycznego dla aplikacji sieci Web w Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/)

Ponadto Dzienniki można uzyskać bezpośrednio z programu Visual Studio, zgodnie z opisem w artykule [Rozwiązywanie problemów z aplikacją sieci Web w Azure App Service przy użyciu programu Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).

## <a name="redirecting-logs"></a>Przekierowywanie dzienników

Zamiast zapisywać dzienniki do [System. Diagnostics. Trace](https://msdn.microsoft.com/library/system.diagnostics.trace), można podać alternatywną implementację rejestrowania, która może być zarejestrowana bezpośrednio w Menedżerze dzienników, takim jak [Log4Net](http://logging.apache.org/log4net/) i [nLOG](http://nlog-project.org/). Wystarczy po prostu wprowadzić implementację [ILogger](https://github.com/aspnet/AspNetWebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Common/Diagnostics/ILogger.cs) i zarejestrować ją z wybranym Microsoft ASP.NET przez siebie aparatem iniekcji. Aby uzyskać szczegółowe informacje, zobacz [iniekcja zależności w programie ASP.NET Web API 2](https://www.asp.net/web-api/overview/advanced/dependency-injection) .

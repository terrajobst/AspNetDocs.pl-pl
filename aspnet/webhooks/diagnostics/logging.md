---
uid: webhooks/diagnostics/logging
title: Elementy Webhook ASP.NET rejestrowanie | Dokumentacja firmy Microsoft
author: rick-anderson
description: Jak przeprowadzić, logowania z elementów Webhook programu ASP.NET.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: f71bc442-5f80-481b-a32c-a0ec18dee9d6
ms.openlocfilehash: 2e86d519c24da102075b4da0a32787c90deb0f6b
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57077033"
---
# <a name="aspnet-webhooks-logging"></a>Elementy Webhook ASP.NET rejestrowania

Microsoft ASP.NET WebHooks rejestrowania jest używana jako sposób raportowania i problemach. Domyślnie dzienniki są zapisywane z użyciem [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) gdzie może być zarządzanych przy użyciu [detektorów śledzenia](https://msdn.microsoft.com/library/system.diagnostics.tracelistener.aspx) , takich jak inne strumień dziennika.

W przypadku wdrażania aplikacji sieci Web jako aplikację internetową platformy Azure, dzienniki są automatycznie wykrywane i mogą być zarządzane oraz wszelkich innych [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) rejestrowania. Aby uzyskać więcej informacji, zobacz [Włączanie rejestrowania diagnostycznego dla aplikacji sieci web w usłudze Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/)

Ponadto dzienników można uzyskać bezpośrednio z poziomu programu Visual Studio, zgodnie z opisem w [Rozwiązywanie problemów z aplikacją sieci web w usłudze Azure App Service przy użyciu programu Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).

## <a name="redirecting-logs"></a>Przekierowywanie dzienników

Zamiast zapisywania ich dzienniki, aby [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace), jest możliwe podanie implementacji alternatywnych rejestrowania, który może rejestrować bezpośrednio do Menedżera dziennika, takich jak [Log4Net](http://logging.apache.org/log4net/) i [NLog ](http://nlog-project.org/). Wystarczy podać implementacja [ILogger](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Common/Diagnostics/ILogger.cs) go będzie pobrać odebrane przez Microsoft ASP.NET WebHooks i zarejestruj je przy użyciu wybranego aparatu iniekcji zależności. Zobacz [wstrzykiwanie zależności w programie ASP.NET Web API 2](https://www.asp.net/web-api/overview/advanced/dependency-injection) Aby uzyskać szczegółowe informacje.

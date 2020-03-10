---
uid: webhooks/receiving/receivers
title: Odbiorniki elementów webhook ASP.NET | Microsoft Docs
author: rick-anderson
description: Odbiorniki elementów webhook ASP.NET
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 6cdea089-15b2-4732-8c68-921ca561a8f1
ms.openlocfilehash: d771a588b23abcd7b1b33e694af17b219683fc48
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78574194"
---
# <a name="aspnet-webhooks-receivers"></a>Odbiorniki elementów webhook ASP.NET

Otrzymywanie elementów webhook zależy od tego, kto jest nadawcą. Czasami istnieją dodatkowe kroki polegające na zarejestrowaniu elementu webhook, który sprawdza, czy subskrybent jest naprawdę nasłuchuje. Niektóre elementy webhook zapewniają model wypychania do ściągania, w którym żądanie HTTP POST zawiera tylko odwołanie do informacji o zdarzeniu, które są następnie pobierane niezależnie. Często model zabezpieczeń jest nieco różny.

Celem Microsoft ASP.NET elementów webhook jest uproszczenie i bardziej spójne połączenie z interfejsem API bez poświęcania dużo czasu na sposób obsługi dowolnych wariantów elementów webhook.

Odbiorca elementu webhook jest odpowiedzialny za akceptowanie i weryfikowanie elementów webhook od określonego nadawcy. Odbiornik elementu webhook może obsługiwać dowolną liczbę elementów webhook, z których każda ma własną konfigurację. Na przykład odbiornik elementu webhook usługi GitHub może akceptować elementy webhook z dowolnej liczby repozytoriów usługi GitHub.

## <a name="webhook-receiver-uris"></a>Identyfikatory URI odbiorników elementu webhook

Instalując Microsoft ASP.NET elementy webhook otrzymujesz ogólny kontroler elementu webhook, który akceptuje żądania elementu webhook z otwartej liczby usług. Po nadejściu żądania wybiera odpowiedni odbiornik, który został zainstalowany do obsługi określonego nadawcy elementu webhook.

Identyfikator URI tego kontrolera jest identyfikatorem URI elementu webhook zarejestrowanym w usłudze i ma postać:

```
https://<host>/api/webhooks/incoming/<receiver>/{id}
```

Ze względów bezpieczeństwa wiele odbiorników elementu webhook wymaga, aby identyfikator URI był adresem URI *https* , a w niektórych przypadkach musi także zawierać dodatkowy parametr zapytania, który służy do wymuszenia, że tylko zamierzona strona może wysyłać elementy webhook do powyższego identyfikatora URI.

Składnik `<receiver>` jest nazwą odbiorcy, na przykład `github` lub `slack`.

*{ID}* jest opcjonalnym identyfikatorem, który może służyć do identyfikowania określonej konfiguracji odbiornika elementu webhook. Ta wartość może służyć do rejestrowania N elementów webhook z określonym odbiornikiem. Na przykład następujące trzy identyfikatory URI mogą służyć do rejestrowania trzech niezależnych elementów webhook:

```
https://<host>/api/webhooks/incoming/github
https://<host>/api/webhooks/incoming/github/12345
https://<host>/api/webhooks/incoming/github/54321
```

## <a name="installing-a-webhook-receiver"></a>Instalowanie odbiornika elementu webhook

Aby odbierać elementy webhook przy użyciu Microsoft ASP.NET elementów webhook, należy najpierw zainstalować pakiet NuGet dla dostawcy lub dostawców elementu webhook, z którego mają być odbierane elementy webhook. Pakiety NuGet mają nazwę [Microsoft. ASPNET. webhooks. Receives. *](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers) , gdzie Ostatnia część wskazuje, że usługa jest obsługiwana. Na przykład

[Microsoft. ASPNET. webhooks. Receives. GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) obsługuje odbieranie elementów webhook z usługi GitHub i [Microsoft. ASPNET. webhooks. Customs](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) zapewnia obsługę odbierania elementów webhook generowanych przez elementy webhook ASP.NET.

Z usługi Box można znaleźć pomoc techniczną dla usługi Dropbox, GitHub, MailChimp, PayPal, wypychania, Salesforce, zapasowych, rozłożonych, Trello i WordPress, ale można obsługiwać dowolną liczbę innych dostawców.

## <a name="configuring-a-webhook-receiver"></a>Konfigurowanie odbiornika elementu webhook

Odbiorniki elementu webhook są konfigurowane za pośrednictwem [IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) interfejs, a konkretne implementacje tego interfejsu mogą być rejestrowane przy użyciu dowolnego modelu iniekcji zależności. Implementacja domyślna używa ustawień aplikacji, które można ustawić w pliku Web. config lub, w przypadku korzystania z usługi Azure Web Apps, można ustawić za pośrednictwem witryny [Azure Portal](https://portal.azure.com/).

![Ustawienia aplikacji platformy Azure](_static/AzureAppSettings.png)

Format kluczy ustawień aplikacji jest następujący:

```
MS_WebHookReceiverSecret_<receiver>
```

Wartość jest rozdzielaną przecinkami listą wartości pasujących do wartości *{ID}* , dla których zarejestrowano elementy webhook, na przykład:

```
MS_WebHookReceiverSecret_GitHub = <secret1>, 12345=<secret2>, 54321=<secret3>
```

## <a name="initializing-a-webhook-receiver"></a>Inicjowanie odbiornika elementu webhook

Odbiorniki elementu webhook są inicjowane przez zarejestrowanie ich, zazwyczaj w klasie statycznej *WebApiConfig* , na przykład:

```csharp
namespace WebHookReceivers
{
    public static class WebApiConfig
    {
        public static void Register(HttpConfiguration config)
        {
            // Web API configuration and services

            // Web API routes
            config.MapHttpAttributeRoutes();

            config.Routes.MapHttpRoute(
                name: "DefaultApi",
                routeTemplate: "api/{controller}/{id}",
                defaults: new { id = RouteParameter.Optional }
            );

            // Load receivers
            config.InitializeReceiveGitHubWebHooks();
        }
    }
}
```

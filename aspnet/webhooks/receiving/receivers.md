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
# <a name="aspnet-webhooks-receivers"></a><span data-ttu-id="cf33f-103">Odbiorniki elementów webhook ASP.NET</span><span class="sxs-lookup"><span data-stu-id="cf33f-103">ASP.NET WebHooks receivers</span></span>

<span data-ttu-id="cf33f-104">Otrzymywanie elementów webhook zależy od tego, kto jest nadawcą.</span><span class="sxs-lookup"><span data-stu-id="cf33f-104">Receiving WebHooks depends on who the sender is.</span></span> <span data-ttu-id="cf33f-105">Czasami istnieją dodatkowe kroki polegające na zarejestrowaniu elementu webhook, który sprawdza, czy subskrybent jest naprawdę nasłuchuje.</span><span class="sxs-lookup"><span data-stu-id="cf33f-105">Sometimes there are additional steps registering a WebHook verifying that the subscriber is really listening.</span></span> <span data-ttu-id="cf33f-106">Niektóre elementy webhook zapewniają model wypychania do ściągania, w którym żądanie HTTP POST zawiera tylko odwołanie do informacji o zdarzeniu, które są następnie pobierane niezależnie.</span><span class="sxs-lookup"><span data-stu-id="cf33f-106">Some WebHooks provide a push-to-pull model where the HTTP POST request only contains a reference to the event information which is then to be retrieved independently.</span></span> <span data-ttu-id="cf33f-107">Często model zabezpieczeń jest nieco różny.</span><span class="sxs-lookup"><span data-stu-id="cf33f-107">Often the security model varies quite a bit.</span></span>

<span data-ttu-id="cf33f-108">Celem Microsoft ASP.NET elementów webhook jest uproszczenie i bardziej spójne połączenie z interfejsem API bez poświęcania dużo czasu na sposób obsługi dowolnych wariantów elementów webhook.</span><span class="sxs-lookup"><span data-stu-id="cf33f-108">The purpose of Microsoft ASP.NET WebHooks is to make it both simpler and more consistent to wire up your API without spending a lot of time figuring out how to handle any particular variant of WebHooks.</span></span>

<span data-ttu-id="cf33f-109">Odbiorca elementu webhook jest odpowiedzialny za akceptowanie i weryfikowanie elementów webhook od określonego nadawcy.</span><span class="sxs-lookup"><span data-stu-id="cf33f-109">A WebHook receiver is responsible for accepting and verifying WebHooks from a particular sender.</span></span> <span data-ttu-id="cf33f-110">Odbiornik elementu webhook może obsługiwać dowolną liczbę elementów webhook, z których każda ma własną konfigurację.</span><span class="sxs-lookup"><span data-stu-id="cf33f-110">A WebHook receiver can support any number of WebHooks, each with their own configuration.</span></span> <span data-ttu-id="cf33f-111">Na przykład odbiornik elementu webhook usługi GitHub może akceptować elementy webhook z dowolnej liczby repozytoriów usługi GitHub.</span><span class="sxs-lookup"><span data-stu-id="cf33f-111">For example, the GitHub WebHook receiver can accept WebHooks from any number of GitHub repositories.</span></span>

## <a name="webhook-receiver-uris"></a><span data-ttu-id="cf33f-112">Identyfikatory URI odbiorników elementu webhook</span><span class="sxs-lookup"><span data-stu-id="cf33f-112">WebHook Receiver URIs</span></span>

<span data-ttu-id="cf33f-113">Instalując Microsoft ASP.NET elementy webhook otrzymujesz ogólny kontroler elementu webhook, który akceptuje żądania elementu webhook z otwartej liczby usług.</span><span class="sxs-lookup"><span data-stu-id="cf33f-113">By installing Microsoft ASP.NET WebHooks you get a general WebHook controller which accepts WebHook requests from an open-ended number of services.</span></span> <span data-ttu-id="cf33f-114">Po nadejściu żądania wybiera odpowiedni odbiornik, który został zainstalowany do obsługi określonego nadawcy elementu webhook.</span><span class="sxs-lookup"><span data-stu-id="cf33f-114">When a request arrives, it picks the appropriate receiver that you have installed for handling a particular WebHook sender.</span></span>

<span data-ttu-id="cf33f-115">Identyfikator URI tego kontrolera jest identyfikatorem URI elementu webhook zarejestrowanym w usłudze i ma postać:</span><span class="sxs-lookup"><span data-stu-id="cf33f-115">The URI of this controller is the WebHook URI that you register with the service and is of the form:</span></span>

```
https://<host>/api/webhooks/incoming/<receiver>/{id}
```

<span data-ttu-id="cf33f-116">Ze względów bezpieczeństwa wiele odbiorników elementu webhook wymaga, aby identyfikator URI był adresem URI *https* , a w niektórych przypadkach musi także zawierać dodatkowy parametr zapytania, który służy do wymuszenia, że tylko zamierzona strona może wysyłać elementy webhook do powyższego identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="cf33f-116">For security reasons, many WebHook receivers require that the URI is an *https* URI and in some cases it must also contain an additional query parameter which is used to enforce that only the intended party can send WebHooks to the URI above.</span></span>

<span data-ttu-id="cf33f-117">Składnik `<receiver>` jest nazwą odbiorcy, na przykład `github` lub `slack`.</span><span class="sxs-lookup"><span data-stu-id="cf33f-117">The `<receiver>` component is the name of the receiver, for example `github` or `slack`.</span></span>

<span data-ttu-id="cf33f-118">*{ID}* jest opcjonalnym identyfikatorem, który może służyć do identyfikowania określonej konfiguracji odbiornika elementu webhook.</span><span class="sxs-lookup"><span data-stu-id="cf33f-118">The *{id}* is an optional identifier which can be used to identify a particular WebHook receiver configuration.</span></span> <span data-ttu-id="cf33f-119">Ta wartość może służyć do rejestrowania N elementów webhook z określonym odbiornikiem.</span><span class="sxs-lookup"><span data-stu-id="cf33f-119">This can be used to register N WebHooks with a particular receiver.</span></span> <span data-ttu-id="cf33f-120">Na przykład następujące trzy identyfikatory URI mogą służyć do rejestrowania trzech niezależnych elementów webhook:</span><span class="sxs-lookup"><span data-stu-id="cf33f-120">For example, the following three URIs can be used to register for three independent WebHooks:</span></span>

```
https://<host>/api/webhooks/incoming/github
https://<host>/api/webhooks/incoming/github/12345
https://<host>/api/webhooks/incoming/github/54321
```

## <a name="installing-a-webhook-receiver"></a><span data-ttu-id="cf33f-121">Instalowanie odbiornika elementu webhook</span><span class="sxs-lookup"><span data-stu-id="cf33f-121">Installing a WebHook Receiver</span></span>

<span data-ttu-id="cf33f-122">Aby odbierać elementy webhook przy użyciu Microsoft ASP.NET elementów webhook, należy najpierw zainstalować pakiet NuGet dla dostawcy lub dostawców elementu webhook, z którego mają być odbierane elementy webhook.</span><span class="sxs-lookup"><span data-stu-id="cf33f-122">To receive WebHooks using Microsoft ASP.NET WebHooks, you first install the Nuget package for the WebHook provider or providers you want to receive WebHooks from.</span></span> <span data-ttu-id="cf33f-123">Pakiety NuGet mają nazwę [Microsoft. ASPNET. webhooks. Receives. \*](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers) , gdzie Ostatnia część wskazuje, że usługa jest obsługiwana.</span><span class="sxs-lookup"><span data-stu-id="cf33f-123">The Nuget packages are named [Microsoft.AspNet.WebHooks.Receivers.\*](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers) where the last part indicates the service supported.</span></span> <span data-ttu-id="cf33f-124">Na przykład</span><span class="sxs-lookup"><span data-stu-id="cf33f-124">For example</span></span>

<span data-ttu-id="cf33f-125">[Microsoft. ASPNET. webhooks. Receives. GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) obsługuje odbieranie elementów webhook z usługi GitHub i [Microsoft. ASPNET. webhooks. Customs](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) zapewnia obsługę odbierania elementów webhook generowanych przez elementy webhook ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="cf33f-125">[Microsoft.AspNet.WebHooks.Receivers.GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) provides support for receiving WebHooks from GitHub and [Microsoft.AspNet.WebHooks.Receivers.Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) provides support for receiving WebHooks generated by ASP.NET WebHooks.</span></span>

<span data-ttu-id="cf33f-126">Z usługi Box można znaleźć pomoc techniczną dla usługi Dropbox, GitHub, MailChimp, PayPal, wypychania, Salesforce, zapasowych, rozłożonych, Trello i WordPress, ale można obsługiwać dowolną liczbę innych dostawców.</span><span class="sxs-lookup"><span data-stu-id="cf33f-126">Out of the box you can find support for Dropbox, GitHub, MailChimp, PayPal, Pusher, Salesforce, Slack, Stripe, Trello, and WordPress but it is possible to support any number of other providers.</span></span>

## <a name="configuring-a-webhook-receiver"></a><span data-ttu-id="cf33f-127">Konfigurowanie odbiornika elementu webhook</span><span class="sxs-lookup"><span data-stu-id="cf33f-127">Configuring a WebHook Receiver</span></span>

<span data-ttu-id="cf33f-128">Odbiorniki elementu webhook są konfigurowane za pośrednictwem [IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) interfejs, a konkretne implementacje tego interfejsu mogą być rejestrowane przy użyciu dowolnego modelu iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="cf33f-128">WebHook Receivers are configured through the [IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) inteface and particular implementations of that interface can be registered using any dependency injection model.</span></span> <span data-ttu-id="cf33f-129">Implementacja domyślna używa ustawień aplikacji, które można ustawić w pliku Web. config lub, w przypadku korzystania z usługi Azure Web Apps, można ustawić za pośrednictwem witryny [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="cf33f-129">The default implementation uses Application Settings which can either be set in the Web.config file, or, if using Azure Web Apps, can be set through the [Azure Portal](https://portal.azure.com/).</span></span>

![Ustawienia aplikacji platformy Azure](_static/AzureAppSettings.png)

<span data-ttu-id="cf33f-131">Format kluczy ustawień aplikacji jest następujący:</span><span class="sxs-lookup"><span data-stu-id="cf33f-131">The format for Application Setting keys is as follows:</span></span>

```
MS_WebHookReceiverSecret_<receiver>
```

<span data-ttu-id="cf33f-132">Wartość jest rozdzielaną przecinkami listą wartości pasujących do wartości *{ID}* , dla których zarejestrowano elementy webhook, na przykład:</span><span class="sxs-lookup"><span data-stu-id="cf33f-132">The value is a comma-separated list of values matching the *{id}* values for which WebHooks have been registered, for example:</span></span>

```
MS_WebHookReceiverSecret_GitHub = <secret1>, 12345=<secret2>, 54321=<secret3>
```

## <a name="initializing-a-webhook-receiver"></a><span data-ttu-id="cf33f-133">Inicjowanie odbiornika elementu webhook</span><span class="sxs-lookup"><span data-stu-id="cf33f-133">Initializing a WebHook Receiver</span></span>

<span data-ttu-id="cf33f-134">Odbiorniki elementu webhook są inicjowane przez zarejestrowanie ich, zazwyczaj w klasie statycznej *WebApiConfig* , na przykład:</span><span class="sxs-lookup"><span data-stu-id="cf33f-134">WebHook Receivers are initialized by registering them, typically in the *WebApiConfig* static class, for example:</span></span>

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

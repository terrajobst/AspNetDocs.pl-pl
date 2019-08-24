---
uid: webhooks/index
title: Elementy webhook ASP.NET — Omówienie | Microsoft Docs
author: rick-anderson
description: Wprowadzenie do ASP.NET elementów webhook.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 5e2843f0-f499-448f-a712-33d4e9858321
ms.openlocfilehash: aa65a20e1af16d58533e37fafc77ac246e0fe327
ms.sourcegitcommit: b95316530fa51087d6c400ff91814fe37e73f7e8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/23/2019
ms.locfileid: "70000730"
---
# <a name="aspnet-webhooks-overview"></a><span data-ttu-id="8c5ea-103">Webhook ASP.NET — Omówienie</span><span class="sxs-lookup"><span data-stu-id="8c5ea-103">ASP.NET WebHooks overview</span></span>

<span data-ttu-id="8c5ea-104">Elementy webhook to lekki wzorzec HTTP udostępniający prosty model pub/sub dla połączeń interfejsów API sieci Web i usług SaaS.</span><span class="sxs-lookup"><span data-stu-id="8c5ea-104">WebHooks is a lightweight HTTP pattern providing a simple pub/sub model for wiring together Web APIs and SaaS services.</span></span> <span data-ttu-id="8c5ea-105">Po wystąpieniu zdarzenia w usłudze powiadomienie jest wysyłane w formie żądania HTTP POST do zarejestrowanych subskrybentów.</span><span class="sxs-lookup"><span data-stu-id="8c5ea-105">When an event happens in a service, a notification is sent in the form of an HTTP POST request to registered subscribers.</span></span> <span data-ttu-id="8c5ea-106">Żądanie POST zawiera informacje o zdarzeniu, które umożliwi odbiorcy odpowiednie działanie.</span><span class="sxs-lookup"><span data-stu-id="8c5ea-106">The POST request contains information about the event which makes it possible for the receiver to act accordingly.</span></span>

<span data-ttu-id="8c5ea-107">Ze względu na prostotę elementy webhook są już udostępniane przez dużą liczbę usług, w tym [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [BitBucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Zapasy](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/)i wiele szczegółowe.</span><span class="sxs-lookup"><span data-stu-id="8c5ea-107">Because of their simplicity, WebHooks are already exposed by a large number of services including [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/), and many more.</span></span> <span data-ttu-id="8c5ea-108">Na przykład element webhook może wskazywać, że plik został zmieniony w usłudze [Dropbox](http://dropbox.com/)lub że w usłudze GitHub została zatwierdzona zmiana kodu, lub Zainicjowanie płatności w [systemie PayPal](http://www.paypal.com/)lub utworzenie karty w usłudze [Trello](http://www.trello.com/).</span><span class="sxs-lookup"><span data-stu-id="8c5ea-108">For example, a WebHook can indicate that a file has changed in [Dropbox](http://dropbox.com/), or a code change has been committed in GitHub, or a payment has been initiated in [PayPal](http://www.paypal.com/), or a card has been created in [Trello](http://www.trello.com/).</span></span> <span data-ttu-id="8c5ea-109">Możliwości są nieograniczone!</span><span class="sxs-lookup"><span data-stu-id="8c5ea-109">The possibilities are endless!</span></span>

<span data-ttu-id="8c5ea-110">Microsoft ASP.NET webhook ułatwiają wysyłanie i odbieranie elementów webhook w ramach aplikacji ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="8c5ea-110">Microsoft ASP.NET WebHooks makes it easier to both send and receive WebHooks as part of your ASP.NET application:</span></span>

* <span data-ttu-id="8c5ea-111">Po stronie odbiorczej zapewnia wspólny model do odbioru i przetwarzania elementów webhook z dowolnej liczby dostawców elementów webhook.</span><span class="sxs-lookup"><span data-stu-id="8c5ea-111">On the receiving side, it provides a common model for receiving and processing WebHooks from any number of WebHook providers.</span></span> <span data-ttu-id="8c5ea-112">Usługa [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [BitBucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), Pusher, [Salesforce](http://www.salesforce.com), [Zapasy](http://www.slack.com), [](http://www.pusher.com) [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/),[WordPress](http://www.wordpress.com) i [Systemu Zendesk](https://www.zendesk.com/) , ale łatwo jest dodać obsługę więcej.</span><span class="sxs-lookup"><span data-stu-id="8c5ea-112">It comes out of the box with support for [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Pusher](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/),[WordPress](http://www.wordpress.com) and [Zendesk](https://www.zendesk.com/) but it is easy to add support for more.</span></span>

* <span data-ttu-id="8c5ea-113">Na stronie wysyłającej zapewnia obsługę zarządzania subskrypcjami i ich przechowywania oraz wysyłania powiadomień o zdarzeniach do odpowiedniego zestawu subskrybentów.</span><span class="sxs-lookup"><span data-stu-id="8c5ea-113">On the sending side it provides support for managing and storing subscriptions as well as for sending event notifications to the right set of subscribers.</span></span> <span data-ttu-id="8c5ea-114">Pozwala to definiować własny zestaw zdarzeń, które Subskrybenci mogą subskrybować i powiadamiać o nich, gdy wystąpią.</span><span class="sxs-lookup"><span data-stu-id="8c5ea-114">This allows you to define your own set of events that subscribers can subscribe to and notify them when things happens.</span></span>

<span data-ttu-id="8c5ea-115">Te dwie części mogą być używane razem lub w zależności od danego scenariusza.</span><span class="sxs-lookup"><span data-stu-id="8c5ea-115">The two parts can be used together or apart depending on your scenario.</span></span> <span data-ttu-id="8c5ea-116">Jeśli musisz tylko odbierać elementy webhook z innych usług, możesz użyć tylko części odbiornika. Jeśli chcesz tylko uwidocznić elementy webhook dla innych użytkowników, możesz to zrobić.</span><span class="sxs-lookup"><span data-stu-id="8c5ea-116">If you only need to receive WebHooks from other services then you can use just the receiver part; if you only want to expose WebHooks for others to consume, then you can do just that.</span></span>

<span data-ttu-id="8c5ea-117">Kod jest przeznaczony dla ASP.NET Web API 2 i ASP.NET MVC 5 i jest dostępny jako [OSS w serwisie GitHub](https://github.com/aspnet/WebHooks).</span><span class="sxs-lookup"><span data-stu-id="8c5ea-117">The code targets ASP.NET Web API 2 and ASP.NET MVC 5 and is available as [OSS on GitHub](https://github.com/aspnet/WebHooks).</span></span>

## <a name="webhooks-overview"></a><span data-ttu-id="8c5ea-118">Elementy webhook — Omówienie</span><span class="sxs-lookup"><span data-stu-id="8c5ea-118">WebHooks Overview</span></span>

<span data-ttu-id="8c5ea-119">Elementy webhook są wzorcem, co oznacza, że różni się w zależności od tego, w jaki sposób jest używany przez usługę do obsługi, ale podstawowy pomysł jest taki sam.</span><span class="sxs-lookup"><span data-stu-id="8c5ea-119">WebHooks is a pattern which means that it varies how it is used from service to service but the basic idea is the same.</span></span> <span data-ttu-id="8c5ea-120">Elementy webhook można traktować jako prosty model pub/sub, w którym użytkownik może subskrybować zdarzenia występujące w innym miejscu.</span><span class="sxs-lookup"><span data-stu-id="8c5ea-120">You can think of WebHooks as a simple pub/sub model where a user can subscribe to events happening elsewhere.</span></span> <span data-ttu-id="8c5ea-121">Powiadomienia o zdarzeniach są propagowane jako żądania HTTP POST zawierające informacje o samym zdarzeniu.</span><span class="sxs-lookup"><span data-stu-id="8c5ea-121">The event notifications are propagated as HTTP POST requests containing information about the event itself.</span></span>

<span data-ttu-id="8c5ea-122">Zwykle żądanie HTTP POST zawiera obiekt JSON lub dane formularza HTML określone przez nadawcę elementu webhook, w tym informacje o zdarzeniu powodującym wyzwolenie elementu webhook.</span><span class="sxs-lookup"><span data-stu-id="8c5ea-122">Typically the HTTP POST request contains a JSON object or HTML form data determined by the WebHook sender including information about the event causing the WebHook to trigger.</span></span> <span data-ttu-id="8c5ea-123">Na przykład treść żądania POST elementu webhook z usługi [GitHub](http://www.github.com/) wygląda następująco w wyniku nowego problemu w określonym repozytorium:</span><span class="sxs-lookup"><span data-stu-id="8c5ea-123">For example, a WebHook POST request body from [GitHub](http://www.github.com/) looks like this as a result of a new issue being opened in a particular repository:</span></span>

```json
{
  "action": "opened",
  "issue": {
      "url": "https://api.github.com/repos/octocat/Hello-World/issues/1347",
      "number": 1347,
      ...
  },
  "repository": {
      "id": 1296269,
      "full_name": "octocat/Hello-World",
      "owner": {
          "login": "octocat",
          "id": 1
          ...
      },
      ...
  },
  "sender": {
      "login": "octocat",
      "id": 1,
      ...
  }
}
```

<span data-ttu-id="8c5ea-124">Aby upewnić się, że element webhook jest rzeczywiście z zamierzonego nadawcy, żądanie POST jest zabezpieczone w jakiś sposób, a następnie zweryfikowane przez odbiornik.</span><span class="sxs-lookup"><span data-stu-id="8c5ea-124">To ensure that the WebHook is indeed from the intended sender, the POST request is secured in some way and then verified by the receiver.</span></span> <span data-ttu-id="8c5ea-125">Na przykład elementy [webhook usługi GitHub](https://developer.github.com/webhooks/) zawierają nagłówek HTTP *podpisu X-Hub* z skrótem treści żądania, który jest sprawdzany przez implementację odbiorcy, dzięki czemu nie trzeba martwić się o to.</span><span class="sxs-lookup"><span data-stu-id="8c5ea-125">For example, [GitHub WebHooks](https://developer.github.com/webhooks/) includes an *X-Hub-Signature* HTTP header with a hash of the request body which is checked by the receiver implementation so you don't have to worry about it.</span></span>

<span data-ttu-id="8c5ea-126">Przepływ elementu webhook zwykle jest podobny do tego:</span><span class="sxs-lookup"><span data-stu-id="8c5ea-126">The WebHook flow generally goes something like this:</span></span>

* <span data-ttu-id="8c5ea-127">Nadawca elementu webhook ujawnia zdarzenia, które klient może subskrybować.</span><span class="sxs-lookup"><span data-stu-id="8c5ea-127">The WebHook sender exposes events that a client can subscribe to.</span></span> <span data-ttu-id="8c5ea-128">Zdarzenia opisują zauważalne zmiany w systemie, na przykład, że został wstawiony nowy element danych, że proces został ukończony lub coś innego.</span><span class="sxs-lookup"><span data-stu-id="8c5ea-128">The events describe observable changes to the system, for example that a new data item has been inserted, that a process has completed, or something else.</span></span>

* <span data-ttu-id="8c5ea-129">Odbiorca elementu webhook subskrybuje się, rejestrując element webhook składający się z czterech rzeczy:</span><span class="sxs-lookup"><span data-stu-id="8c5ea-129">The WebHook receiver subscribes by registering a WebHook consisting of four things:</span></span>

     1. <span data-ttu-id="8c5ea-130">Identyfikator URI, w którym ma być ogłaszane powiadomienie o zdarzeniu w formie żądania HTTP POST;</span><span class="sxs-lookup"><span data-stu-id="8c5ea-130">A URI for where the event notification should be posted in the form of an HTTP POST request;</span></span>

     2. <span data-ttu-id="8c5ea-131">Zestaw filtrów opisujących konkretne zdarzenia, dla których powinien być uruchamiany element webhook;</span><span class="sxs-lookup"><span data-stu-id="8c5ea-131">A set of filters describing the particular events for which the WebHook should be fired;</span></span>

     3. <span data-ttu-id="8c5ea-132">Klucz tajny, który jest używany do podpisywania żądania HTTP POST;</span><span class="sxs-lookup"><span data-stu-id="8c5ea-132">A secret key which is used to sign the HTTP POST request;</span></span>

     4. <span data-ttu-id="8c5ea-133">Dodatkowe dane, które mają zostać uwzględnione w żądaniu POST protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="8c5ea-133">Additional data which is to be included in the HTTP POST request.</span></span> <span data-ttu-id="8c5ea-134">Może to być na przykład dodatkowe pola nagłówka HTTP lub właściwości zawarte w treści żądania POST protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="8c5ea-134">This can for example be additional HTTP header fields or properties included in the HTTP POST request body.</span></span>

* <span data-ttu-id="8c5ea-135">Po wystąpieniu zdarzenia zostaną znalezione zgodne rejestracje elementu webhook i przesłane żądania POST HTTP.</span><span class="sxs-lookup"><span data-stu-id="8c5ea-135">Once an event happens, the matching WebHook registrations are found and HTTP POST requests are submitted.</span></span> <span data-ttu-id="8c5ea-136">Zazwyczaj generowanie żądań POST protokołu HTTP zostanie ponowione kilka razy, jeśli z jakiegoś powodu odbiorca nie odpowiada lub żądanie HTTP POST spowoduje wystąpienie błędu.</span><span class="sxs-lookup"><span data-stu-id="8c5ea-136">Typically, the generation of the HTTP POST requests are retried several times if for some reason the recipient is not responding or the HTTP POST request results in an error response.</span></span>

## <a name="webhooks-processing-pipeline"></a><span data-ttu-id="8c5ea-137">Potok przetwarzania elementów webhook</span><span class="sxs-lookup"><span data-stu-id="8c5ea-137">WebHooks Processing Pipeline</span></span>

<span data-ttu-id="8c5ea-138">Potok przetwarzania Microsoft ASP.NET elementów webhook dla przychodzących elementów webhook wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="8c5ea-138">The Microsoft ASP.NET WebHooks processing pipeline for incoming WebHooks looks like this:</span></span>

![Potok przetwarzania elementów webhook ASP.NET](_static/WebHookReceivers.png)

<span data-ttu-id="8c5ea-140">Te dwa kluczowe pojęcia są *odbiornikami* i *programami obsługi*:</span><span class="sxs-lookup"><span data-stu-id="8c5ea-140">The two key concepts here are *Receivers* and *Handlers*:</span></span>

* <span data-ttu-id="8c5ea-141">*Odbiorcy* są odpowiedzialni za obsługę określonej wersji elementu webhook od danego nadawcy i wymuszanie kontroli zabezpieczeń, aby upewnić się, że żądanie elementu webhook rzeczywiście pochodzi od zamierzonego nadawcy.</span><span class="sxs-lookup"><span data-stu-id="8c5ea-141">*Receivers* are responsible for handling the particular flavor of WebHook from a given sender and for enforcing security checks to ensure that the WebHook request indeed is from the intended sender.</span></span>

* <span data-ttu-id="8c5ea-142">*Programy obsługi* zazwyczaj mają miejsce, gdzie kod użytkownika uruchamia przetwarzanie określonego elementu webhook.</span><span class="sxs-lookup"><span data-stu-id="8c5ea-142">*Handlers* are typically where user code runs processing the particular WebHook.</span></span>

<span data-ttu-id="8c5ea-143">W poniższych węzłach te pojęcia są opisane bardziej szczegółowo.</span><span class="sxs-lookup"><span data-stu-id="8c5ea-143">In the following nodes these concepts are described in more details.</span></span>

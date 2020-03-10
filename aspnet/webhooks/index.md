---
uid: webhooks/index
title: Elementy webhook ASP.NET — Omówienie | Microsoft Docs
author: rick-anderson
description: Wprowadzenie do ASP.NET elementów webhook.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 5e2843f0-f499-448f-a712-33d4e9858321
ms.openlocfilehash: 1e21c92e950893c0ff87c63f03f4710a158441fd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78637292"
---
# <a name="aspnet-webhooks-overview"></a>Webhook ASP.NET — Omówienie

Elementy webhook to lekki wzorzec HTTP udostępniający prosty model pub/sub dla połączeń interfejsów API sieci Web i usług SaaS. Po wystąpieniu zdarzenia w usłudze powiadomienie jest wysyłane w formie żądania HTTP POST do zarejestrowanych subskrybentów. Żądanie POST zawiera informacje o zdarzeniu, które umożliwi odbiorcy odpowiednie działanie.

Ze względu na prostotę elementy webhook są już udostępniane przez dużą liczbę usług, w tym [Dropbox](http://dropbox.com/), [GitHub](https://www.github.com/), [BitBucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Zapasy](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/)i wiele innych. Na przykład element webhook może wskazywać, że plik został zmieniony w usłudze [Dropbox](http://dropbox.com/)lub że w usłudze GitHub została zatwierdzona zmiana kodu, lub Zainicjowanie płatności w [systemie PayPal](http://www.paypal.com/)lub utworzenie karty w usłudze [Trello](http://www.trello.com/). Możliwości są nieograniczone!

Microsoft ASP.NET webhook ułatwiają wysyłanie i odbieranie elementów webhook w ramach aplikacji ASP.NET:

* Po stronie odbiorczej zapewnia wspólny model do odbioru i przetwarzania elementów webhook z dowolnej liczby dostawców elementów webhook. Jest on dostępny z pudełka z obsługą usług [Dropbox](http://dropbox.com/), [GitHub](https://www.github.com/), [BitBucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Pusher](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [zapasowych](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/),[WordPress](http://www.wordpress.com) i [systemu Zendesk](https://www.zendesk.com/) , ale łatwo jest dodać obsługę więcej.

* Na stronie wysyłającej zapewnia obsługę zarządzania subskrypcjami i ich przechowywania oraz wysyłania powiadomień o zdarzeniach do odpowiedniego zestawu subskrybentów. Pozwala to definiować własny zestaw zdarzeń, które Subskrybenci mogą subskrybować i powiadamiać o nich, gdy wystąpią.

Te dwie części mogą być używane razem lub w zależności od danego scenariusza. Jeśli musisz tylko odbierać elementy webhook z innych usług, możesz użyć tylko części odbiornika. Jeśli chcesz tylko uwidocznić elementy webhook dla innych użytkowników, możesz to zrobić.

Kod jest przeznaczony dla ASP.NET Web API 2 i ASP.NET MVC 5 i jest dostępny jako [OSS w serwisie GitHub](https://github.com/aspnet/WebHooks).

## <a name="webhooks-overview"></a>Elementy webhook — Omówienie

Elementy webhook są wzorcem, co oznacza, że różni się w zależności od tego, w jaki sposób jest używany przez usługę do obsługi, ale podstawowy pomysł jest taki sam. Elementy webhook można traktować jako prosty model pub/sub, w którym użytkownik może subskrybować zdarzenia występujące w innym miejscu. Powiadomienia o zdarzeniach są propagowane jako żądania HTTP POST zawierające informacje o samym zdarzeniu.

Zwykle żądanie HTTP POST zawiera obiekt JSON lub dane formularza HTML określone przez nadawcę elementu webhook, w tym informacje o zdarzeniu powodującym wyzwolenie elementu webhook. Na przykład treść żądania POST elementu webhook z usługi [GitHub](https://www.github.com/) wygląda następująco w wyniku nowego problemu w określonym repozytorium:

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

Aby upewnić się, że element webhook jest rzeczywiście z zamierzonego nadawcy, żądanie POST jest zabezpieczone w jakiś sposób, a następnie zweryfikowane przez odbiornik. Na przykład elementy [webhook usługi GitHub](https://developer.github.com/webhooks/) zawierają nagłówek HTTP *podpisu X-Hub* z skrótem treści żądania, który jest sprawdzany przez implementację odbiorcy, dzięki czemu nie trzeba martwić się o to.

Przepływ elementu webhook zwykle jest podobny do tego:

* Nadawca elementu webhook ujawnia zdarzenia, które klient może subskrybować. Zdarzenia opisują zauważalne zmiany w systemie, na przykład, że został wstawiony nowy element danych, że proces został ukończony lub coś innego.

* Odbiorca elementu webhook subskrybuje się, rejestrując element webhook składający się z czterech rzeczy:

     1. Identyfikator URI, w którym ma być ogłaszane powiadomienie o zdarzeniu w formie żądania HTTP POST;

     2. Zestaw filtrów opisujących konkretne zdarzenia, dla których powinien być uruchamiany element webhook;

     3. Klucz tajny, który jest używany do podpisywania żądania HTTP POST;

     4. Dodatkowe dane, które mają zostać uwzględnione w żądaniu POST protokołu HTTP. Może to być na przykład dodatkowe pola nagłówka HTTP lub właściwości zawarte w treści żądania POST protokołu HTTP.

* Po wystąpieniu zdarzenia zostaną znalezione zgodne rejestracje elementu webhook i przesłane żądania POST HTTP. Zazwyczaj generowanie żądań POST protokołu HTTP zostanie ponowione kilka razy, jeśli z jakiegoś powodu odbiorca nie odpowiada lub żądanie HTTP POST spowoduje wystąpienie błędu.

## <a name="webhooks-processing-pipeline"></a>Potok przetwarzania elementów webhook

Potok przetwarzania Microsoft ASP.NET elementów webhook dla przychodzących elementów webhook wygląda następująco:

![Potok przetwarzania elementów webhook ASP.NET](_static/WebHookReceivers.png)

Te dwa kluczowe pojęcia są *odbiornikami* i *programami obsługi*:

* *Odbiorcy* są odpowiedzialni za obsługę określonej wersji elementu webhook od danego nadawcy i wymuszanie kontroli zabezpieczeń, aby upewnić się, że żądanie elementu webhook rzeczywiście pochodzi od zamierzonego nadawcy.

* *Programy obsługi* zazwyczaj mają miejsce, gdzie kod użytkownika uruchamia przetwarzanie określonego elementu webhook.

W poniższych węzłach te pojęcia są opisane bardziej szczegółowo.

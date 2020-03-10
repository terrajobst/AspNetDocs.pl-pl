---
uid: webhooks/receiving/handlers
title: Programy obsługi elementów webhook ASP.NET | Microsoft Docs
author: rick-anderson
description: Jak obsługiwać żądania w elementach webhook ASP.NET.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: a55b0d20-9c90-4bd3-a471-20da6f569f0c
ms.openlocfilehash: 01c9a283d105c4a0973ff88c8de646c5f49a34db
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78637873"
---
# <a name="aspnet-webhooks-handlers"></a>Procedury obsługi elementów webhook ASP.NET

Po zweryfikowaniu żądań elementów webhook przez odbiornik elementu webhook jest on gotowy do przetworzenia przez kod użytkownika. Jest to miejsce, w którym są one *obsługiwane* . Procedury obsługi pochodzą z interfejsu [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) , ale zazwyczaj używają klasy [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) zamiast wyprowadzania bezpośrednio z interfejsu.

Żądanie elementu webhook może być przetwarzane przez jeden lub więcej programów obsługi. Procedury obsługi są wywoływane w kolejności na podstawie ich właściwości *Order* , która przechodzą z najmniejszego do najwyższego, gdzie Order jest prostą liczbą całkowitą (sugerowaną dla zakresu od 1 do 100):

![Diagram właściwości kolejności obsługi elementu webhook](_static/Handlers.png)

Program obsługi może opcjonalnie ustawić właściwość *Response* w WebHookHandlerContext, co spowoduje zatrzymanie przetwarzania i wysłanie odpowiedzi z powrotem jako odpowiedzi HTTP do elementu webhook. W powyższym przypadku procedura obsługi C nie zostanie wywołana, ponieważ ma wyższą kolejność niż B i B ustawia odpowiedź.

Ustawienie odpowiedzi jest zazwyczaj odpowiednie dla elementów webhook, w których odpowiedź może przenosić informacje z powrotem do źródłowego interfejsu API. Dotyczy to na przykład przypadku elementu webhook zapasowych, gdzie odpowiedź jest księgowana z powrotem do kanału, z którego pochodzi element webhook. Programy obsługi mogą ustawiać właściwość Receiver, jeśli chcą otrzymywać tylko elementy webhook z tego konkretnego odbiorcy. Jeśli nie ustawili odbiorcy, są one wywoływane dla wszystkich z nich.

Innym typowym wykorzystaniem odpowiedzi jest użycie *410* nieistniejącej odpowiedzi, aby wskazać, że element webhook nie jest już aktywny i nie powinny być przesyłane żadne dalsze żądania.

Domyślnie program obsługi będzie wywoływany przez wszystkie odbiorniki elementu webhook. Jeśli jednak właściwość *Receiver* jest ustawiona na nazwę programu obsługi, to program obsługi będzie odbierać tylko żądania elementu webhook z tego odbiorcy.

## <a name="processing-a-webhook"></a>Przetwarzanie elementu webhook

Gdy wywoływana jest procedura obsługi, pobiera [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) zawierający informacje o żądaniu elementu webhook. Dane, zazwyczaj treść żądania HTTP, są dostępne na podstawie właściwości *Data* .

Typ danych to zwykle dane JSON lub HTML formularza, ale w razie potrzeby można rzutować na bardziej konkretny typ. Na przykład niestandardowe elementy webhook wygenerowane przez elementy webhook ASP.NET mogą być rzutowane na typ [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) w następujący sposób:

```csharp
public class MyWebHookHandler : WebHookHandler
{
    public MyWebHookHandler()
    {
        this.Receiver = "custom";
    }

    public override Task ExecuteAsync(string generator, WebHookHandlerContext context)
    {
        CustomNotifications notifications = context.GetDataOrDefault<CustomNotifications>();
        foreach (var notification in notifications.Notifications)
        {
            ...
        }
        return Task.FromResult(true);
    }
}
```

  ## <a name="queued-processing"></a>Przetwarzanie w kolejce

Większość nadawców elementu webhook wyśle ponownie element webhook, jeśli odpowiedź nie zostanie wygenerowana w ciągu kilku sekund kilku. Oznacza to, że program obsługi musi zakończyć przetwarzanie w tym przedziale czasowym, aby nie był on ponownie wywoływany.

Jeśli przetwarzanie trwa dłużej lub jest lepiej obsługiwane oddzielnie, [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) można użyć do przesłania żądania elementu webhook do kolejki, na przykład z [kolejki usługi Azure Storage](https://msdn.microsoft.com/library/azure/dd179353.aspx).

Konspekt implementacji [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) jest dostępny tutaj:

```csharp
public class QueueHandler : WebHookQueueHandler
{
    public override Task EnqueueAsync(WebHookQueueContext context)
    {

        // Enqueue WebHookQueueContext to your queuing system of choice

        return Task.FromResult(true);
    }
}
```

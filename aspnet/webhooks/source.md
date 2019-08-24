---
uid: webhooks/source
title: Kod źródłowy i pakiety NuGet ASP.NET webhook | Microsoft Docs
author: rick-anderson
description: Linki do kodu źródłowego i pakietów NuGet ASP.NET webhook
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 91a62bfa-ea3a-41f9-a2e1-e90d2c8fc8ca
ms.openlocfilehash: 8d07848754d9efda9c893b8ba54ac6d0c0214a53
ms.sourcegitcommit: b95316530fa51087d6c400ff91814fe37e73f7e8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/23/2019
ms.locfileid: "70000699"
---
# <a name="aspnet-webhooks-source-code-and-nuget-packages"></a>Kod źródłowy i pakiety NuGet ASP.NET webhook

Microsoft ASP.NET elementy webhook są częścią Microsoft ASP.NETj rodziny modułów i są hostowane jako [projekt typu open source w witrynie GitHub](https://github.com/aspnet/WebHooks). Oznacza to, że akceptujemy wkłady, ale zapoznaj się z [wytycznymi dotyczącymi udziałów](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) przed przesłaniem żądania ściągnięcia.

Ta dokumentacja online, którą odczytujesz teraz, jest również hostowana jako [Open Source w serwisie GitHub](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) , a także akceptuje wkłady.

## <a name="nuget-packages"></a>Pakiety NuGet

[Pakiety NuGet](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) są podzielone na trzy części:

* [Wspólny](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): Wspólny pakiet współużytkowany przez nadawców i odbiorników.

* [Nadawca](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): Zestaw pakietów obsługujący wysyłanie własnych elementów webhook do innych osób. Funkcje wysyłania elementów webhook zostały szczegółowo opisane w temacie Wysyłanie elementów [webhook](sending/senders.md).

* [Odbiorcy](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): Zestaw pakietów, które obsługują otrzymywanie elementów webhook od innych użytkowników. Funkcja otrzymywania elementów webhook została szczegółowo opisana w temacie [otrzymywanie](receiving/index.md)elementów webhook.

---
uid: webhooks/receiving/dependencies
title: Zależności odbiornika elementów Webhook programu ASP.NET | Dokumentacja firmy Microsoft
author: rick-anderson
description: Zależności odbiornika i wstrzykiwanie zależności w elementów Webhook programu ASP.NET.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 5125e483-c2bb-435b-8cd1-21d3499bfaaf
ms.openlocfilehash: c44cfe3ed310aa728a989b108c410e8786e4f514
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57073262"
---
# <a name="aspnet-webhooks-receiver-dependencies"></a>Zależności odbiornika elementów Webhook programu ASP.NET

Microsoft ASP.NET WebHooks zaprojektowano przy użyciu iniekcji zależności na uwadze. Większość zależności w systemie można zastąpić za pomocą alternatywnych implementacji przy użyciu aparatu iniekcji zależności.

Zobacz [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) listę zależności odbiornika. Jeśli nie zarejestrowano żadnych zależności, używana jest domyślna implementacja. Zobacz [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) listę domyślnej implementacji.

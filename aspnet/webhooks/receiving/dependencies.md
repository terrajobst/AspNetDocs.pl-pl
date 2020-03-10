---
uid: webhooks/receiving/dependencies
title: Zależności odbiorników elementów webhook (ASP.NET) | Microsoft Docs
author: rick-anderson
description: Zależności odbiornika i iniekcja zależności w elementach webhook ASP.NET.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 5125e483-c2bb-435b-8cd1-21d3499bfaaf
ms.openlocfilehash: 477b8828209d0da1d485ef883b0f99b4e1b9b5bf
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78637285"
---
# <a name="aspnet-webhooks-receiver-dependencies"></a>Zależności odbiorników elementów webhook ASP.NET

Microsoft ASP.NET elementy webhook zostały zaprojektowane z myślą o iniekcji zależności. Większość zależności w systemie można zastąpić implementacjami alternatywnymi przy użyciu aparatu iniekcji zależności.

Aby uzyskać listę zależności odbiorników, zobacz [DependencyScopeExtensions](https://github.com/aspnet/aspnetWebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) . Jeśli żadna zależność nie została zarejestrowana, używana jest domyślna implementacja. Zobacz [ReceiverServices](https://github.com/aspnet/aspnetWebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) , aby zapoznać się z listą domyślnych implementacji.

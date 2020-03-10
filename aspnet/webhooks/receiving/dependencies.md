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
# <a name="aspnet-webhooks-receiver-dependencies"></a><span data-ttu-id="8314b-103">Zależności odbiorników elementów webhook ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8314b-103">ASP.NET WebHooks receiver dependencies</span></span>

<span data-ttu-id="8314b-104">Microsoft ASP.NET elementy webhook zostały zaprojektowane z myślą o iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="8314b-104">Microsoft ASP.NET WebHooks is designed with dependency injection in mind.</span></span> <span data-ttu-id="8314b-105">Większość zależności w systemie można zastąpić implementacjami alternatywnymi przy użyciu aparatu iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="8314b-105">Most dependencies in the system can be replaced with alternative implementations using a dependency injection engine.</span></span>

<span data-ttu-id="8314b-106">Aby uzyskać listę zależności odbiorników, zobacz [DependencyScopeExtensions](https://github.com/aspnet/aspnetWebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) .</span><span class="sxs-lookup"><span data-stu-id="8314b-106">See [DependencyScopeExtensions](https://github.com/aspnet/aspnetWebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) for a list of receiver dependencies.</span></span> <span data-ttu-id="8314b-107">Jeśli żadna zależność nie została zarejestrowana, używana jest domyślna implementacja.</span><span class="sxs-lookup"><span data-stu-id="8314b-107">If no dependency has been registered, a default implementation is used.</span></span> <span data-ttu-id="8314b-108">Zobacz [ReceiverServices](https://github.com/aspnet/aspnetWebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) , aby zapoznać się z listą domyślnych implementacji.</span><span class="sxs-lookup"><span data-stu-id="8314b-108">See [ReceiverServices](https://github.com/aspnet/aspnetWebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) for a list of default implementations.</span></span>

---
uid: signalr/overview/older-versions/persistent-connection-authorization
title: Uwierzytelnianie i autoryzacja połączeń trwałych SignalR (SignalR 1.x) | Dokumentacja firmy Microsoft
author: bradygaster
description: W tym temacie opisano sposób wymusić autoryzację dla trwałego połączenia. Aby uzyskać ogólne informacje na temat integracji zabezpieczeń do aplikacji SignalR...
ms.author: bradyg
ms.date: 10/21/2013
ms.assetid: c34bc627-41af-4c21-a817-e97a19a7f252
msc.legacyurl: /signalr/overview/older-versions/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: 9ccc59e3ea502daf12ce82382ab30ca73ca0f9b5
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65117042"
---
# <a name="authentication-and-authorization-for-signalr-persistent-connections-signalr-1x"></a><span data-ttu-id="dd7f0-104">Uwierzytelnianie i autoryzacja połączeń trwałych usługi SignalR (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="dd7f0-104">Authentication and Authorization for SignalR Persistent Connections (SignalR 1.x)</span></span>

<span data-ttu-id="dd7f0-105">przez [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="dd7f0-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="dd7f0-106">W tym temacie opisano sposób wymusić autoryzację dla trwałego połączenia.</span><span class="sxs-lookup"><span data-stu-id="dd7f0-106">This topic describes how to enforce authorization on a persistent connection.</span></span> <span data-ttu-id="dd7f0-107">Aby uzyskać ogólne informacje na temat integracji zabezpieczeń do aplikacji SignalR zobacz [wprowadzenie do zabezpieczeń](index.md).</span><span class="sxs-lookup"><span data-stu-id="dd7f0-107">For general information about integrating security into a SignalR application, see [Introduction to Security](index.md).</span></span>

## <a name="enforce-authorization"></a><span data-ttu-id="dd7f0-108">Wymuszanie autoryzacji</span><span class="sxs-lookup"><span data-stu-id="dd7f0-108">Enforce authorization</span></span>

<span data-ttu-id="dd7f0-109">Aby wymusić reguł autoryzacji, korzystając z [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) konieczne jest przesłonięcie `AuthorizeRequest` metody.</span><span class="sxs-lookup"><span data-stu-id="dd7f0-109">To enforce authorization rules when using a [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) you must override the `AuthorizeRequest` method.</span></span> <span data-ttu-id="dd7f0-110">Nie można użyć `Authorize` atrybutu z połączeń trwałych.</span><span class="sxs-lookup"><span data-stu-id="dd7f0-110">You cannot use the `Authorize` attribute with persistent connections.</span></span> <span data-ttu-id="dd7f0-111">`AuthorizeRequest` Metoda jest wywoływana przez SignalR Framework przed każdym żądaniem, aby sprawdzić, czy użytkownik jest autoryzowany do wykonania żądanej akcji.</span><span class="sxs-lookup"><span data-stu-id="dd7f0-111">The `AuthorizeRequest` method is called by the SignalR Framework before every request to verify that the user is authorized to perform the requested action.</span></span> <span data-ttu-id="dd7f0-112">`AuthorizeRequest` Metoda nie jest wywoływana przez klienta; zamiast tego należy uwierzytelnić użytkownika za pośrednictwem mechanizmu uwierzytelniania standardowego w Twojej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="dd7f0-112">The `AuthorizeRequest` method is not called from the client; instead, you authenticate the user through your application's standard authentication mechanism.</span></span>

<span data-ttu-id="dd7f0-113">W poniższym przykładzie pokazano sposób ograniczania żądań do uwierzytelnionych użytkowników.</span><span class="sxs-lookup"><span data-stu-id="dd7f0-113">The example below shows how to limit requests to authenticated users.</span></span>

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

<span data-ttu-id="dd7f0-114">Można dodać wszelka logika autoryzacji dostosowanego w metodzie AuthorizeRequest; takich jak sprawdzanie, czy użytkownik należy do określonej roli.</span><span class="sxs-lookup"><span data-stu-id="dd7f0-114">You can add any customized authorization logic in the AuthorizeRequest method; such as, checking whether a user belongs to a particular role.</span></span>

---
uid: signalr/overview/older-versions/persistent-connection-authorization
title: Uwierzytelnianie i autoryzacja dla połączeń trwałych sygnalizujących (sygnał 1. x) | Microsoft Docs
author: bradygaster
description: W tym temacie opisano, jak wymusić autoryzację dla trwałego połączenia. Aby uzyskać ogólne informacje na temat integrowania zabezpieczeń z aplikacją sygnalizującą,...
ms.author: bradyg
ms.date: 10/21/2013
ms.assetid: c34bc627-41af-4c21-a817-e97a19a7f252
msc.legacyurl: /signalr/overview/older-versions/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: 9ccc59e3ea502daf12ce82382ab30ca73ca0f9b5
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536541"
---
# <a name="authentication-and-authorization-for-signalr-persistent-connections-signalr-1x"></a><span data-ttu-id="f65ff-104">Uwierzytelnianie i autoryzacja połączeń trwałych usługi SignalR (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="f65ff-104">Authentication and Authorization for SignalR Persistent Connections (SignalR 1.x)</span></span>

<span data-ttu-id="f65ff-105">[Fletcher Patryk](https://github.com/pfletcher), [Tomasz FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="f65ff-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="f65ff-106">W tym temacie opisano, jak wymusić autoryzację dla trwałego połączenia.</span><span class="sxs-lookup"><span data-stu-id="f65ff-106">This topic describes how to enforce authorization on a persistent connection.</span></span> <span data-ttu-id="f65ff-107">Aby uzyskać ogólne informacje o integrowaniu zabezpieczeń z aplikacją sygnalizującą, zobacz [wprowadzenie do zabezpieczeń](index.md).</span><span class="sxs-lookup"><span data-stu-id="f65ff-107">For general information about integrating security into a SignalR application, see [Introduction to Security](index.md).</span></span>

## <a name="enforce-authorization"></a><span data-ttu-id="f65ff-108">Wymuś autoryzację</span><span class="sxs-lookup"><span data-stu-id="f65ff-108">Enforce authorization</span></span>

<span data-ttu-id="f65ff-109">Aby wymusić reguły autoryzacji podczas korzystania z [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) , należy zastąpić metodę `AuthorizeRequest`.</span><span class="sxs-lookup"><span data-stu-id="f65ff-109">To enforce authorization rules when using a [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) you must override the `AuthorizeRequest` method.</span></span> <span data-ttu-id="f65ff-110">Nie można użyć atrybutu `Authorize` z połączeniami trwałymi.</span><span class="sxs-lookup"><span data-stu-id="f65ff-110">You cannot use the `Authorize` attribute with persistent connections.</span></span> <span data-ttu-id="f65ff-111">Metoda `AuthorizeRequest` jest wywoływana przez środowisko sygnalizujące przed każdym żądaniem, aby sprawdzić, czy użytkownik ma uprawnienia do wykonywania żądanych akcji.</span><span class="sxs-lookup"><span data-stu-id="f65ff-111">The `AuthorizeRequest` method is called by the SignalR Framework before every request to verify that the user is authorized to perform the requested action.</span></span> <span data-ttu-id="f65ff-112">Metoda `AuthorizeRequest` nie jest wywoływana z klienta; Zamiast tego użytkownik uwierzytelnia użytkownika za pomocą standardowego mechanizmu uwierzytelniania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f65ff-112">The `AuthorizeRequest` method is not called from the client; instead, you authenticate the user through your application's standard authentication mechanism.</span></span>

<span data-ttu-id="f65ff-113">W poniższym przykładzie pokazano, jak ograniczyć liczbę żądań do uwierzytelnionych użytkowników.</span><span class="sxs-lookup"><span data-stu-id="f65ff-113">The example below shows how to limit requests to authenticated users.</span></span>

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

<span data-ttu-id="f65ff-114">Dowolna dostosowana logika autoryzacji można dodać w metodzie AuthorizeRequest. na przykład sprawdzanie, czy użytkownik należy do określonej roli.</span><span class="sxs-lookup"><span data-stu-id="f65ff-114">You can add any customized authorization logic in the AuthorizeRequest method; such as, checking whether a user belongs to a particular role.</span></span>

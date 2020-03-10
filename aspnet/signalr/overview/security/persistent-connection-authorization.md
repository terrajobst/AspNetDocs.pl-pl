---
uid: signalr/overview/security/persistent-connection-authorization
title: Uwierzytelnianie i autoryzacja dla połączeń trwałych sygnalizujących | Microsoft Docs
author: bradygaster
description: W tym temacie opisano, jak wymusić autoryzację dla trwałego połączenia. Aby uzyskać ogólne informacje na temat integrowania zabezpieczeń z aplikacją sygnalizującą,...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: e264677b-9c01-47ec-94f9-3cd8f08f94af
msc.legacyurl: /signalr/overview/security/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: 7e69d3c1a18f1239142891734ba58cd2b0078f84
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78578877"
---
# <a name="authentication-and-authorization-for-signalr-persistent-connections"></a><span data-ttu-id="52d56-104">Uwierzytelnianie i autoryzacja połączeń trwałych usługi SignalR</span><span class="sxs-lookup"><span data-stu-id="52d56-104">Authentication and Authorization for SignalR Persistent Connections</span></span>

<span data-ttu-id="52d56-105">[Fletcher Patryk](https://github.com/pfletcher), [Tomasz FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="52d56-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="52d56-106">W tym temacie opisano, jak wymusić autoryzację dla trwałego połączenia.</span><span class="sxs-lookup"><span data-stu-id="52d56-106">This topic describes how to enforce authorization on a persistent connection.</span></span> <span data-ttu-id="52d56-107">Aby uzyskać ogólne informacje o integrowaniu zabezpieczeń z aplikacją sygnalizującą, zobacz [wprowadzenie do zabezpieczeń](introduction-to-security.md).</span><span class="sxs-lookup"><span data-stu-id="52d56-107">For general information about integrating security into a SignalR application, see [Introduction to Security](introduction-to-security.md).</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="52d56-108">Wersje oprogramowania używane w tym temacie</span><span class="sxs-lookup"><span data-stu-id="52d56-108">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="52d56-109">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="52d56-109">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="52d56-110">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="52d56-110">.NET 4.5</span></span>
> - <span data-ttu-id="52d56-111">Sygnalizujący wersja 2</span><span class="sxs-lookup"><span data-stu-id="52d56-111">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="52d56-112">Poprzednie wersje tego tematu</span><span class="sxs-lookup"><span data-stu-id="52d56-112">Previous versions of this topic</span></span>
>
> <span data-ttu-id="52d56-113">Aby uzyskać informacje o wcześniejszych wersjach programu sygnalizującego, zobacz sekcję [sygnalizujące starsze wersje](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="52d56-113">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="52d56-114">Pytania i Komentarze</span><span class="sxs-lookup"><span data-stu-id="52d56-114">Questions and comments</span></span>
>
> <span data-ttu-id="52d56-115">Prosimy o opinię na temat sposobu, w jaki lubię ten samouczek, i co możemy ulepszyć w komentarzach w dolnej części strony.</span><span class="sxs-lookup"><span data-stu-id="52d56-115">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="52d56-116">Jeśli masz pytania, które nie są bezpośrednio związane z samouczkiem, możesz je ogłosić na [forum ASP.NET](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="52d56-116">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

## <a name="enforce-authorization"></a><span data-ttu-id="52d56-117">Wymuś autoryzację</span><span class="sxs-lookup"><span data-stu-id="52d56-117">Enforce authorization</span></span>

<span data-ttu-id="52d56-118">Aby wymusić reguły autoryzacji podczas korzystania z [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) , należy zastąpić metodę `AuthorizeRequest`.</span><span class="sxs-lookup"><span data-stu-id="52d56-118">To enforce authorization rules when using a [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) you must override the `AuthorizeRequest` method.</span></span> <span data-ttu-id="52d56-119">Nie można użyć atrybutu `Authorize` z połączeniami trwałymi.</span><span class="sxs-lookup"><span data-stu-id="52d56-119">You cannot use the `Authorize` attribute with persistent connections.</span></span> <span data-ttu-id="52d56-120">Metoda `AuthorizeRequest` jest wywoływana przez środowisko sygnalizujące przed każdym żądaniem, aby sprawdzić, czy użytkownik ma uprawnienia do wykonywania żądanych akcji.</span><span class="sxs-lookup"><span data-stu-id="52d56-120">The `AuthorizeRequest` method is called by the SignalR Framework before every request to verify that the user is authorized to perform the requested action.</span></span> <span data-ttu-id="52d56-121">Metoda `AuthorizeRequest` nie jest wywoływana z klienta; Zamiast tego użytkownik uwierzytelnia użytkownika za pomocą standardowego mechanizmu uwierzytelniania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="52d56-121">The `AuthorizeRequest` method is not called from the client; instead, you authenticate the user through your application's standard authentication mechanism.</span></span>

<span data-ttu-id="52d56-122">W poniższym przykładzie pokazano, jak ograniczyć liczbę żądań do uwierzytelnionych użytkowników.</span><span class="sxs-lookup"><span data-stu-id="52d56-122">The example below shows how to limit requests to authenticated users.</span></span>

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

<span data-ttu-id="52d56-123">Dowolna dostosowana logika autoryzacji można dodać w metodzie AuthorizeRequest. na przykład sprawdzanie, czy użytkownik należy do określonej roli.</span><span class="sxs-lookup"><span data-stu-id="52d56-123">You can add any customized authorization logic in the AuthorizeRequest method; such as, checking whether a user belongs to a particular role.</span></span>

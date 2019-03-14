---
title: Zarządzanie użytkownikami i grupami w SignalR
author: bradygaster
description: Omówienie platformy ASP.NET Core SignalR użytkowników i grup zarządzania.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 06/04/2018
uid: signalr/groups
ms.openlocfilehash: 45f2bb44e03a586b7fc186525fdd3a2645c820d5
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57067676"
---
# <a name="manage-users-and-groups-in-signalr"></a><span data-ttu-id="10fda-103">Zarządzanie użytkownikami i grupami w SignalR</span><span class="sxs-lookup"><span data-stu-id="10fda-103">Manage users and groups in SignalR</span></span>

<span data-ttu-id="10fda-104">Przez [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="10fda-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="10fda-105">SignalR umożliwia wiadomości do wysłania do wszystkie połączenia skojarzone z określonym użytkownikiem, a także do nazwanych grup połączeń.</span><span class="sxs-lookup"><span data-stu-id="10fda-105">SignalR allows messages to be sent to all connections associated with a specific user, as well as to named groups of connections.</span></span>

<span data-ttu-id="10fda-106">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [(jak pobrać)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="10fda-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="users-in-signalr"></a><span data-ttu-id="10fda-107">Użytkownicy w SignalR</span><span class="sxs-lookup"><span data-stu-id="10fda-107">Users in SignalR</span></span>

<span data-ttu-id="10fda-108">SignalR umożliwia wysyłanie komunikatów do wszystkich połączeń, skojarzone z określonym użytkownikiem.</span><span class="sxs-lookup"><span data-stu-id="10fda-108">SignalR allows you to send messages to all connections associated with a specific user.</span></span> <span data-ttu-id="10fda-109">Domyślnie używa SignalR `ClaimTypes.NameIdentifier` z `ClaimsPrincipal` skojarzonych z tym połączeniem jako identyfikator użytkownika.</span><span class="sxs-lookup"><span data-stu-id="10fda-109">By default, SignalR uses the `ClaimTypes.NameIdentifier` from the `ClaimsPrincipal` associated with the connection as the user identifier.</span></span> <span data-ttu-id="10fda-110">Jeden użytkownik może mieć wiele połączeń aplikacji SignalR.</span><span class="sxs-lookup"><span data-stu-id="10fda-110">A single user can have multiple connections to a SignalR app.</span></span> <span data-ttu-id="10fda-111">Można na przykład połączenia użytkownika, na pulpicie, a także swojego telefonu.</span><span class="sxs-lookup"><span data-stu-id="10fda-111">For example, a user could be connected on their desktop as well as their phone.</span></span> <span data-ttu-id="10fda-112">Każde urządzenie ma oddzielne połączenia SignalR, ale są one wszystkie skojarzone z tym użytkownikiem.</span><span class="sxs-lookup"><span data-stu-id="10fda-112">Each device has a separate SignalR connection, but they're all associated with the same user.</span></span> <span data-ttu-id="10fda-113">Jeśli komunikat jest wysyłany do użytkownika, wszystkie połączenia skojarzone z tym użytkownikiem komunikat.</span><span class="sxs-lookup"><span data-stu-id="10fda-113">If a message is sent to the user, all of the connections associated with that user receive the message.</span></span> <span data-ttu-id="10fda-114">Identyfikator użytkownika dla połączenia może zostać oceniony przez `Context.UserIdentifier` właściwości Centrum.</span><span class="sxs-lookup"><span data-stu-id="10fda-114">The user identifier for a connection can be accessed by the `Context.UserIdentifier` property in your hub.</span></span>

<span data-ttu-id="10fda-115">Wyślij wiadomość do określonego użytkownika, przekazując identyfikator użytkownika, aby `User` działać w metodzie Centrum, jak pokazano w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="10fda-115">Send a message to a specific user by passing the user identifier to the `User` function in your hub method as shown in the following example:</span></span>

> [!NOTE]
> <span data-ttu-id="10fda-116">Identyfikator użytkownika jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="10fda-116">The user identifier is case-sensitive.</span></span>

[!code-csharp[Configure service](groups/sample/hubs/chathub.cs?range=29-32)]

## <a name="groups-in-signalr"></a><span data-ttu-id="10fda-117">Grupami w SignalR</span><span class="sxs-lookup"><span data-stu-id="10fda-117">Groups in SignalR</span></span>

<span data-ttu-id="10fda-118">Grupy to zbiór połączenia skojarzone z nazwą.</span><span class="sxs-lookup"><span data-stu-id="10fda-118">A group is a collection of connections associated with a name.</span></span> <span data-ttu-id="10fda-119">Komunikaty mogą być wysyłane do wszystkich połączeń w grupie.</span><span class="sxs-lookup"><span data-stu-id="10fda-119">Messages can be sent to all connections in a group.</span></span> <span data-ttu-id="10fda-120">Grupy są zalecanym sposobem wysyłania do połączenia lub wielu połączeń, ponieważ te grupy są zarządzane przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="10fda-120">Groups are the recommended way to send to a connection or multiple connections because the groups are managed by the application.</span></span> <span data-ttu-id="10fda-121">Połączenie może należeć do wielu grup.</span><span class="sxs-lookup"><span data-stu-id="10fda-121">A connection can be a member of multiple groups.</span></span> <span data-ttu-id="10fda-122">Dzięki temu grupy idealne podobny aplikację do obsługi rozmów, gdzie każdego pomieszczenia mogą być reprezentowane jako grupa.</span><span class="sxs-lookup"><span data-stu-id="10fda-122">This makes groups ideal for something like a chat application, where each room can be represented as a group.</span></span> <span data-ttu-id="10fda-123">Połączenia, które mogą być dodawane do lub usunięte z grup za pośrednictwem `AddToGroupAsync` i `RemoveFromGroupAsync` metody.</span><span class="sxs-lookup"><span data-stu-id="10fda-123">Connections can be added to or removed from groups via the `AddToGroupAsync` and `RemoveFromGroupAsync` methods.</span></span>

[!code-csharp[Hub methods](groups/sample/hubs/chathub.cs?range=15-27)]

<span data-ttu-id="10fda-124">Członkostwo w grupie nie są zachowywane podczas ponownego nawiązania połączenia.</span><span class="sxs-lookup"><span data-stu-id="10fda-124">Group membership isn't preserved when a connection reconnects.</span></span> <span data-ttu-id="10fda-125">Musi ponownie dołączyć do grupy po ponownym nawiązaniu połączenia.</span><span class="sxs-lookup"><span data-stu-id="10fda-125">The connection needs to rejoin the group when it's re-established.</span></span> <span data-ttu-id="10fda-126">Nie jest możliwe do zliczenia członkowie danej grupy, ponieważ te informacje nie są dostępne, jeśli aplikacja będzie skalowana na wielu serwerach.</span><span class="sxs-lookup"><span data-stu-id="10fda-126">It's not possible to count the members of a group, since this information is not available if the application is scaled to multiple servers.</span></span>

<span data-ttu-id="10fda-127">Aby chronić dostęp do zasobów podczas korzystania z grup, użyj [uwierzytelnianie i autoryzacja](xref:signalr/authn-and-authz) funkcji w programie ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="10fda-127">To protect access to resources while using groups, use [authentication and authorization](xref:signalr/authn-and-authz) functionality in ASP.NET Core.</span></span> <span data-ttu-id="10fda-128">Tylko dodanie użytkowników do grupy, jeśli poświadczenia są prawidłowe dla tej grupy, komunikaty wysyłane do tej grupy zaczną się tylko dla autoryzowanych użytkowników.</span><span class="sxs-lookup"><span data-stu-id="10fda-128">If you only add users to a group when the credentials are valid for that group, messages sent to that group will only go to authorized users.</span></span> <span data-ttu-id="10fda-129">Jednak grupy nie są funkcją zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="10fda-129">However, groups are not a security feature.</span></span> <span data-ttu-id="10fda-130">Uwierzytelniania oświadczeń ma funkcje, których nie grupy, na przykład wygaśnięcia i odwoływania praw dostępu.</span><span class="sxs-lookup"><span data-stu-id="10fda-130">Authentication claims have features that groups do not, such as expiry and revocation.</span></span> <span data-ttu-id="10fda-131">Uprawnienia użytkownika, aby uzyskać dostęp do grupy jest odwołany, trzeba ręcznie wykryje to i usunąć je z niej.</span><span class="sxs-lookup"><span data-stu-id="10fda-131">If a user's permission to access the group is revoked, you have to manually detect that and remove them from the group.</span></span>

> [!NOTE]
> <span data-ttu-id="10fda-132">Nazwy grup jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="10fda-132">Group names are case-sensitive.</span></span>

## <a name="related-resources"></a><span data-ttu-id="10fda-133">Powiązane zasoby</span><span class="sxs-lookup"><span data-stu-id="10fda-133">Related resources</span></span>

* [<span data-ttu-id="10fda-134">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="10fda-134">Get started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="10fda-135">Centra</span><span class="sxs-lookup"><span data-stu-id="10fda-135">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="10fda-136">Publikowanie na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="10fda-136">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)

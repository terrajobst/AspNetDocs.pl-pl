---
uid: signalr/overview/security/hub-authorization
title: Uwierzytelnianie i autoryzacja dla centrów sygnałów | Microsoft Docs
author: bradygaster
description: W tym temacie opisano sposób ograniczania, którzy użytkownicy lub role mogą uzyskać dostęp do metod centrów. Wersje oprogramowania używane w tym temacie Visual Studio 2013 programu .NET 4,5 sygnalizujące...
ms.author: bradyg
ms.date: 01/05/2015
ms.assetid: a610c796-c131-473c-baef-2e6c568cb2a2
msc.legacyurl: /signalr/overview/security/hub-authorization
msc.type: authoredcontent
ms.openlocfilehash: 5006af5e623da6958a6d59949c6f2cf776c77fc3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78578919"
---
# <a name="authentication-and-authorization-for-signalr-hubs"></a><span data-ttu-id="fe7b8-104">Uwierzytelnianie i autoryzacja dla centrów usługi SignalR</span><span class="sxs-lookup"><span data-stu-id="fe7b8-104">Authentication and Authorization for SignalR Hubs</span></span>

<span data-ttu-id="fe7b8-105">[Fletcher Patryk](https://github.com/pfletcher), [Tomasz FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="fe7b8-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="fe7b8-106">W tym temacie opisano sposób ograniczania, którzy użytkownicy lub role mogą uzyskać dostęp do metod centrów.</span><span class="sxs-lookup"><span data-stu-id="fe7b8-106">This topic describes how to restrict which users or roles can access hub methods.</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="fe7b8-107">Wersje oprogramowania używane w tym temacie</span><span class="sxs-lookup"><span data-stu-id="fe7b8-107">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="fe7b8-108">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="fe7b8-108">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="fe7b8-109">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="fe7b8-109">.NET 4.5</span></span>
> - <span data-ttu-id="fe7b8-110">Sygnalizujący wersja 2</span><span class="sxs-lookup"><span data-stu-id="fe7b8-110">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="fe7b8-111">Poprzednie wersje tego tematu</span><span class="sxs-lookup"><span data-stu-id="fe7b8-111">Previous versions of this topic</span></span>
>
> <span data-ttu-id="fe7b8-112">Aby uzyskać informacje o wcześniejszych wersjach programu sygnalizującego, zobacz sekcję [sygnalizujące starsze wersje](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="fe7b8-112">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="fe7b8-113">Pytania i Komentarze</span><span class="sxs-lookup"><span data-stu-id="fe7b8-113">Questions and comments</span></span>
>
> <span data-ttu-id="fe7b8-114">Prosimy o opinię na temat sposobu, w jaki lubię ten samouczek, i co możemy ulepszyć w komentarzach w dolnej części strony.</span><span class="sxs-lookup"><span data-stu-id="fe7b8-114">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="fe7b8-115">Jeśli masz pytania, które nie są bezpośrednio związane z samouczkiem, możesz je ogłosić na [forum ASP.NET](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="fe7b8-115">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

## <a name="overview"></a><span data-ttu-id="fe7b8-116">Omówienie</span><span class="sxs-lookup"><span data-stu-id="fe7b8-116">Overview</span></span>

<span data-ttu-id="fe7b8-117">Ten temat zawiera następujące sekcje:</span><span class="sxs-lookup"><span data-stu-id="fe7b8-117">This topic contains the following sections:</span></span>

- [<span data-ttu-id="fe7b8-118">Autoryzuj atrybut</span><span class="sxs-lookup"><span data-stu-id="fe7b8-118">Authorize attribute</span></span>](#authorizeattribute)
- [<span data-ttu-id="fe7b8-119">Wymagaj uwierzytelniania dla wszystkich centrów</span><span class="sxs-lookup"><span data-stu-id="fe7b8-119">Require authentication for all hubs</span></span>](#requireauth)
- [<span data-ttu-id="fe7b8-120">Dostosowana autoryzacja</span><span class="sxs-lookup"><span data-stu-id="fe7b8-120">Customized authorization</span></span>](#custom)
- [<span data-ttu-id="fe7b8-121">Przekazywanie informacji uwierzytelniania do klientów</span><span class="sxs-lookup"><span data-stu-id="fe7b8-121">Pass authentication information to clients</span></span>](#passauth)
- [<span data-ttu-id="fe7b8-122">Opcje uwierzytelniania dla klientów platformy .NET</span><span class="sxs-lookup"><span data-stu-id="fe7b8-122">Authentication options for .NET clients</span></span>](#authoptions)

    - [<span data-ttu-id="fe7b8-123">Plik cookie z uwierzytelnianiem formularzy</span><span class="sxs-lookup"><span data-stu-id="fe7b8-123">Cookie with Forms Authentication</span></span>](#cookie)
    - [<span data-ttu-id="fe7b8-124">Uwierzytelnianie systemu Windows</span><span class="sxs-lookup"><span data-stu-id="fe7b8-124">Windows Authentication</span></span>](#windows)
    - [<span data-ttu-id="fe7b8-125">Nagłówek połączenia</span><span class="sxs-lookup"><span data-stu-id="fe7b8-125">Connection header</span></span>](#header)
    - [<span data-ttu-id="fe7b8-126">Certyfikat</span><span class="sxs-lookup"><span data-stu-id="fe7b8-126">Certificate</span></span>](#certificate)

<a id="authorizeattribute"></a>

## <a name="authorize-attribute"></a><span data-ttu-id="fe7b8-127">Autoryzuj atrybut</span><span class="sxs-lookup"><span data-stu-id="fe7b8-127">Authorize attribute</span></span>

<span data-ttu-id="fe7b8-128">Sygnalizujący udostępnia atrybut [Autoryzuj](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) , aby określić, którzy użytkownicy lub które role mają dostęp do centrum lub metody.</span><span class="sxs-lookup"><span data-stu-id="fe7b8-128">SignalR provides the [Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) attribute to specify which users or roles have access to a hub or method.</span></span> <span data-ttu-id="fe7b8-129">Ten atrybut znajduje się w przestrzeni nazw `Microsoft.AspNet.SignalR`.</span><span class="sxs-lookup"><span data-stu-id="fe7b8-129">This attribute is located in the `Microsoft.AspNet.SignalR` namespace.</span></span> <span data-ttu-id="fe7b8-130">Należy zastosować atrybut `Authorize` do koncentratora lub określonych metod w centrum.</span><span class="sxs-lookup"><span data-stu-id="fe7b8-130">You apply the `Authorize` attribute to either a hub or particular methods in a hub.</span></span> <span data-ttu-id="fe7b8-131">Po zastosowaniu atrybutu `Authorize` do klasy Hub określone wymaganie autoryzacji jest stosowane do wszystkich metod w centrum.</span><span class="sxs-lookup"><span data-stu-id="fe7b8-131">When you apply the `Authorize` attribute to a hub class, the specified authorization requirement is applied to all of the methods in the hub.</span></span> <span data-ttu-id="fe7b8-132">W tym temacie przedstawiono przykłady różnych typów wymagań dotyczących autoryzacji, które można zastosować.</span><span class="sxs-lookup"><span data-stu-id="fe7b8-132">This topic provides examples of the different types of authorization requirements that you can apply.</span></span> <span data-ttu-id="fe7b8-133">Bez atrybutu `Authorize` połączony klient może uzyskać dostęp do dowolnej metody publicznej w centrum.</span><span class="sxs-lookup"><span data-stu-id="fe7b8-133">Without the `Authorize` attribute, a connected client can access any public method on the hub.</span></span>

<span data-ttu-id="fe7b8-134">Jeśli zdefiniowano rolę "admin" w aplikacji sieci Web, można określić, że tylko użytkownicy w tej roli mają dostęp do centrum z poniższym kodem.</span><span class="sxs-lookup"><span data-stu-id="fe7b8-134">If you have defined a role named "Admin" in your web application, you could specify that only users in that role can access a hub with the following code.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample1.cs)]

<span data-ttu-id="fe7b8-135">Można też określić, że koncentrator zawiera jedną metodę, która jest dostępna dla wszystkich użytkowników, i drugą metodę dostępną tylko dla uwierzytelnionych użytkowników, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="fe7b8-135">Or, you can specify that a hub contains one method that is available to all users, and a second method that is only available to authenticated users, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample2.cs)]

<span data-ttu-id="fe7b8-136">Poniższe przykłady dotyczą różnych scenariuszy autoryzacji:</span><span class="sxs-lookup"><span data-stu-id="fe7b8-136">The following examples address different authorization scenarios:</span></span>

- <span data-ttu-id="fe7b8-137">`[Authorize]` — tylko uwierzytelnieni użytkownicy</span><span class="sxs-lookup"><span data-stu-id="fe7b8-137">`[Authorize]` – only authenticated users</span></span>
- <span data-ttu-id="fe7b8-138">`[Authorize(Roles = "Admin,Manager")]` — tylko uwierzytelnieni użytkownicy w określonych rolach</span><span class="sxs-lookup"><span data-stu-id="fe7b8-138">`[Authorize(Roles = "Admin,Manager")]` – only authenticated users in the specified roles</span></span>
- <span data-ttu-id="fe7b8-139">`[Authorize(Users = "user1,user2")]` — tylko uwierzytelnieni użytkownicy z określonymi nazwami użytkowników</span><span class="sxs-lookup"><span data-stu-id="fe7b8-139">`[Authorize(Users = "user1,user2")]` – only authenticated users with the specified user names</span></span>
- <span data-ttu-id="fe7b8-140">`[Authorize(RequireOutgoing=false)]` — tylko uwierzytelnieni użytkownicy mogą wywoływać centrum, ale wywołania z serwera z powrotem do klientów nie są ograniczone przez autoryzację, na przykład w przypadku, gdy tylko niektórzy użytkownicy będą mogli wysyłać wiadomości, ale wszystkie inne mogą odbierać wiadomości.</span><span class="sxs-lookup"><span data-stu-id="fe7b8-140">`[Authorize(RequireOutgoing=false)]` – only authenticated users can invoke the hub, but calls from the server back to clients are not limited by authorization, such as, when only certain users can send a message but all others can receive the message.</span></span> <span data-ttu-id="fe7b8-141">Właściwość RequireOutgoing może być stosowana tylko do całego centrum, a nie dla poszczególnych metod w centrum.</span><span class="sxs-lookup"><span data-stu-id="fe7b8-141">The RequireOutgoing property can only be applied to the entire hub, not on individuals methods within the hub.</span></span> <span data-ttu-id="fe7b8-142">Gdy RequireOutgoing nie jest ustawiona na wartość false, z serwera są wywoływane tylko użytkownicy, którzy spełniają wymagania dotyczące autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="fe7b8-142">When RequireOutgoing is not set to false, only users that meet the authorization requirement are called from the server.</span></span>

<a id="requireauth"></a>

## <a name="require-authentication-for-all-hubs"></a><span data-ttu-id="fe7b8-143">Wymagaj uwierzytelniania dla wszystkich centrów</span><span class="sxs-lookup"><span data-stu-id="fe7b8-143">Require authentication for all hubs</span></span>

<span data-ttu-id="fe7b8-144">Możesz wymagać uwierzytelniania dla wszystkich centrów i metod koncentratora w aplikacji, wywołując metodę [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="fe7b8-144">You can require authentication for all hubs and hub methods in your application by calling the [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) method when the application starts.</span></span> <span data-ttu-id="fe7b8-145">Tej metody można użyć, jeśli masz wiele centrów i chcesz wymusić wymaganie uwierzytelniania dla wszystkich z nich.</span><span class="sxs-lookup"><span data-stu-id="fe7b8-145">You might use this method when you have multiple hubs and want to enforce an authentication requirement for all of them.</span></span> <span data-ttu-id="fe7b8-146">W przypadku tej metody nie można określić wymagań dotyczących autoryzacji roli, użytkownika lub wychodzącego.</span><span class="sxs-lookup"><span data-stu-id="fe7b8-146">With this method, you cannot specify requirements for role, user, or outgoing authorization.</span></span> <span data-ttu-id="fe7b8-147">Można określić, że dostęp do metod centrów jest ograniczony tylko do użytkowników uwierzytelnionych.</span><span class="sxs-lookup"><span data-stu-id="fe7b8-147">You can only specify that access to the hub methods is restricted to authenticated users.</span></span> <span data-ttu-id="fe7b8-148">Można jednak nadal zastosować atrybut Autoryzuj do centrów lub metod, aby określić dodatkowe wymagania.</span><span class="sxs-lookup"><span data-stu-id="fe7b8-148">However, you can still apply the Authorize attribute to hubs or methods to specify additional requirements.</span></span> <span data-ttu-id="fe7b8-149">Każde wymaganie określone w atrybucie jest dodawane do podstawowego wymagania uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="fe7b8-149">Any requirement you specify in an attribute is added to the basic requirement of authentication.</span></span>

<span data-ttu-id="fe7b8-150">W poniższym przykładzie przedstawiono plik startowy, który ogranicza wszystkie metody centrum do uwierzytelnionych użytkowników.</span><span class="sxs-lookup"><span data-stu-id="fe7b8-150">The following example shows a Startup file which restricts all hub methods to authenticated users.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample3.cs)]

<span data-ttu-id="fe7b8-151">Jeśli wywołasz metodę `RequireAuthentication()` po przetworzeniu żądania sygnalizującego, sygnalizujący zgłosi wyjątek `InvalidOperationException`.</span><span class="sxs-lookup"><span data-stu-id="fe7b8-151">If you call the `RequireAuthentication()` method after a SignalR request has been processed, SignalR will throw a `InvalidOperationException` exception.</span></span> <span data-ttu-id="fe7b8-152">Usługa sygnalizująca zgłasza ten wyjątek, ponieważ nie można dodać modułu do HubPipeline po wywołaniu potoku.</span><span class="sxs-lookup"><span data-stu-id="fe7b8-152">SignalR throws this exception because you cannot add a module to the HubPipeline after the pipeline has been invoked.</span></span> <span data-ttu-id="fe7b8-153">W poprzednim przykładzie pokazano wywoływanie metody `RequireAuthentication` w metodzie `Configuration`, która jest wykonywana jednokrotnie przed rozpoczęciem obsługi pierwszego żądania.</span><span class="sxs-lookup"><span data-stu-id="fe7b8-153">The previous example shows calling the `RequireAuthentication` method in the `Configuration` method which is executed one time prior to handling the first request.</span></span>

<a id="custom"></a>

## <a name="customized-authorization"></a><span data-ttu-id="fe7b8-154">Dostosowana autoryzacja</span><span class="sxs-lookup"><span data-stu-id="fe7b8-154">Customized authorization</span></span>

<span data-ttu-id="fe7b8-155">Aby dostosować sposób określania autoryzacji, można utworzyć klasę pochodzącą z `AuthorizeAttribute` i zastąpić metodę [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) .</span><span class="sxs-lookup"><span data-stu-id="fe7b8-155">If you need to customize how authorization is determined, you can create a class that derives from `AuthorizeAttribute` and override the [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) method.</span></span> <span data-ttu-id="fe7b8-156">Dla każdego żądania sygnalizujący wywołuje tę metodę, aby określić, czy użytkownik jest autoryzowany do wykonania żądania.</span><span class="sxs-lookup"><span data-stu-id="fe7b8-156">For each request, SignalR invokes this method to determine whether the user is authorized to complete the request.</span></span> <span data-ttu-id="fe7b8-157">W zastąpionej metodzie należy zapewnić logikę niezbędną dla scenariusza autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="fe7b8-157">In the overridden method, you provide the necessary logic for your authorization scenario.</span></span> <span data-ttu-id="fe7b8-158">Poniższy przykład pokazuje, jak wymusić autoryzację za pośrednictwem tożsamości opartej na oświadczeniach.</span><span class="sxs-lookup"><span data-stu-id="fe7b8-158">The following example shows how to enforce authorization through claims-based identity.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample4.cs)]

<a id="passauth"></a>

## <a name="pass-authentication-information-to-clients"></a><span data-ttu-id="fe7b8-159">Przekazywanie informacji uwierzytelniania do klientów</span><span class="sxs-lookup"><span data-stu-id="fe7b8-159">Pass authentication information to clients</span></span>

<span data-ttu-id="fe7b8-160">Może być konieczne użycie informacji o uwierzytelnianiu w kodzie, który jest uruchamiany na kliencie programu.</span><span class="sxs-lookup"><span data-stu-id="fe7b8-160">You may need to use authentication information in the code that runs on the client.</span></span> <span data-ttu-id="fe7b8-161">Wymagane informacje są przekazywane podczas wywoływania metod na kliencie.</span><span class="sxs-lookup"><span data-stu-id="fe7b8-161">You pass the required information when calling the methods on the client.</span></span> <span data-ttu-id="fe7b8-162">Na przykład metoda aplikacji Chat może przekazać jako parametr nazwę użytkownika osoby ogłaszającej komunikat, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="fe7b8-162">For example, a chat application method could pass as a parameter the user name of the person posting a message, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample5.cs)]

<span data-ttu-id="fe7b8-163">Można też utworzyć obiekt reprezentujący informacje o uwierzytelnianiu i przekazać go jako parametr, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="fe7b8-163">Or, you can create an object to represent the authentication information and pass that object as a parameter, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample6.cs)]

<span data-ttu-id="fe7b8-164">Nie należy przekazywać identyfikatora połączenia jednego klienta do innych klientów, ponieważ złośliwy użytkownik może użyć go do naśladowania żądania od tego klienta.</span><span class="sxs-lookup"><span data-stu-id="fe7b8-164">You should never pass one client's connection id to other clients, as a malicious user could use it to mimic a request from that client.</span></span>

<a id="authoptions"></a>

## <a name="authentication-options-for-net-clients"></a><span data-ttu-id="fe7b8-165">Opcje uwierzytelniania dla klientów platformy .NET</span><span class="sxs-lookup"><span data-stu-id="fe7b8-165">Authentication options for .NET clients</span></span>

<span data-ttu-id="fe7b8-166">W przypadku klienta platformy .NET, takiego jak Aplikacja konsolowa, która współdziała z koncentratorem ograniczonym do uwierzytelnionych użytkowników, można przekazać poświadczenia uwierzytelniania w pliku cookie, w nagłówku połączenia lub w certyfikacie.</span><span class="sxs-lookup"><span data-stu-id="fe7b8-166">When you have a .NET client, such as a console app, which interacts with a hub that is limited to authenticated users, you can pass the authentication credentials in a cookie, the connection header, or a certificate.</span></span> <span data-ttu-id="fe7b8-167">W przykładach w tej sekcji pokazano, jak używać różnych metod uwierzytelniania użytkownika.</span><span class="sxs-lookup"><span data-stu-id="fe7b8-167">The examples in this section show how to use those different methods for authenticating a user.</span></span> <span data-ttu-id="fe7b8-168">Nie są w pełni funkcjonalne aplikacje sygnalizujące.</span><span class="sxs-lookup"><span data-stu-id="fe7b8-168">They are not fully-functional SignalR apps.</span></span> <span data-ttu-id="fe7b8-169">Aby uzyskać więcej informacji na temat klientów .NET z sygnalizującym, zobacz temat [interfejs API centrów — klient platformy .NET](../guide-to-the-api/hubs-api-guide-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="fe7b8-169">For more information about .NET clients with SignalR, see [Hubs API Guide - .NET Client](../guide-to-the-api/hubs-api-guide-net-client.md).</span></span>

<a id="cookie"></a>

### <a name="cookie"></a><span data-ttu-id="fe7b8-170">Plik cookie</span><span class="sxs-lookup"><span data-stu-id="fe7b8-170">Cookie</span></span>

<span data-ttu-id="fe7b8-171">Gdy klient platformy .NET współdziała z centrum korzystającym z uwierzytelniania formularzy ASP.NET, trzeba ręcznie ustawić plik cookie uwierzytelniania w ramach połączenia.</span><span class="sxs-lookup"><span data-stu-id="fe7b8-171">When your .NET client interacts with a hub that uses ASP.NET Forms Authentication, you will need to manually set the authentication cookie on the connection.</span></span> <span data-ttu-id="fe7b8-172">Plik cookie należy dodać do właściwości `CookieContainer` obiektu [HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) .</span><span class="sxs-lookup"><span data-stu-id="fe7b8-172">You add the cookie to the `CookieContainer` property on the [HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) object.</span></span> <span data-ttu-id="fe7b8-173">W poniższym przykładzie przedstawiono aplikację konsolową, która pobiera plik cookie uwierzytelniania ze strony sieci Web i dodaje ten plik cookie do połączenia.</span><span class="sxs-lookup"><span data-stu-id="fe7b8-173">The following example shows a console app that retrieves an authentication cookie from a web page and adds that cookie to the connection.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample7.cs)]

<span data-ttu-id="fe7b8-174">Aplikacja konsoli ogłasza poświadczenia <strong>www.contoso.com/RemoteLogin</strong> , które mogą odwoływać się do pustej strony zawierającej następujący plik związany z kodem.</span><span class="sxs-lookup"><span data-stu-id="fe7b8-174">The console app posts the credentials to <strong>www.contoso.com/RemoteLogin</strong> which could refer to an empty page that contains the following code-behind file.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample8.cs)]

<a id="windows"></a>

### <a name="windows-authentication"></a><span data-ttu-id="fe7b8-175">Uwierzytelnianie systemu Windows</span><span class="sxs-lookup"><span data-stu-id="fe7b8-175">Windows authentication</span></span>

<span data-ttu-id="fe7b8-176">W przypadku korzystania z uwierzytelniania systemu Windows można przekazać poświadczenia bieżącego użytkownika za pomocą właściwości [DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx) .</span><span class="sxs-lookup"><span data-stu-id="fe7b8-176">When using Windows authentication, you can pass the current user's credentials by using the [DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx) property.</span></span> <span data-ttu-id="fe7b8-177">Poświadczenia dla połączenia należy ustawić na wartość DefaultCredentials.</span><span class="sxs-lookup"><span data-stu-id="fe7b8-177">You set the credentials for the connection to the value of the DefaultCredentials.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample9.cs?highlight=6)]

<a id="header"></a>

### <a name="connection-header"></a><span data-ttu-id="fe7b8-178">Nagłówek połączenia</span><span class="sxs-lookup"><span data-stu-id="fe7b8-178">Connection header</span></span>

<span data-ttu-id="fe7b8-179">Jeśli aplikacja nie używa plików cookie, można przekazać informacje o użytkowniku w nagłówku połączenia.</span><span class="sxs-lookup"><span data-stu-id="fe7b8-179">If your application is not using cookies, you can pass user information in the connection header.</span></span> <span data-ttu-id="fe7b8-180">Na przykład można przekazać token w nagłówku połączenia.</span><span class="sxs-lookup"><span data-stu-id="fe7b8-180">For example, you can pass a token in the connection header.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample10.cs?highlight=6)]

<span data-ttu-id="fe7b8-181">Następnie w centrum należy zweryfikować token użytkownika.</span><span class="sxs-lookup"><span data-stu-id="fe7b8-181">Then, in the hub, you would verify the user's token.</span></span>

<a id="certificate"></a>

### <a name="certificate"></a><span data-ttu-id="fe7b8-182">Certyfikat</span><span class="sxs-lookup"><span data-stu-id="fe7b8-182">Certificate</span></span>

<span data-ttu-id="fe7b8-183">Można przekazać certyfikat klienta, aby zweryfikować użytkownika.</span><span class="sxs-lookup"><span data-stu-id="fe7b8-183">You can pass a client certificate to verify the user.</span></span> <span data-ttu-id="fe7b8-184">Podczas tworzenia połączenia zostanie dodany certyfikat.</span><span class="sxs-lookup"><span data-stu-id="fe7b8-184">You add the certificate when creating the connection.</span></span> <span data-ttu-id="fe7b8-185">Poniższy przykład pokazuje, jak dodać certyfikat klienta do połączenia; nie jest wyświetlana pełna Aplikacja konsolowa.</span><span class="sxs-lookup"><span data-stu-id="fe7b8-185">The following example shows only how to add a client certificate to the connection; it does not show the full console app.</span></span> <span data-ttu-id="fe7b8-186">Używa klasy [x509](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx) , która zapewnia kilka różnych sposobów tworzenia certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="fe7b8-186">It uses the [X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx) class which provides several different ways to create the certificate.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample11.cs?highlight=6)]

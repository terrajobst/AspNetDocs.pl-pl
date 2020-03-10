---
uid: signalr/overview/older-versions/hub-authorization
title: Uwierzytelnianie i autoryzacja dla centrów sygnałów (Signaler 1. x) | Microsoft Docs
author: bradygaster
description: W tym temacie opisano sposób ograniczania, którzy użytkownicy lub role mogą uzyskać dostęp do metod centrów.
ms.author: bradyg
ms.date: 10/17/2013
ms.assetid: 3d2dfc0e-eac2-4076-a468-325d3d01cc7b
msc.legacyurl: /signalr/overview/older-versions/hub-authorization
msc.type: authoredcontent
ms.openlocfilehash: 8182677c8931f060d98d17008b16ad545bee4e69
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78558535"
---
# <a name="authentication-and-authorization-for-signalr-hubs-signalr-1x"></a><span data-ttu-id="2ab21-103">Uwierzytelnianie i autoryzacja centrów usługi SignalR (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="2ab21-103">Authentication and Authorization for SignalR Hubs (SignalR 1.x)</span></span>

<span data-ttu-id="2ab21-104">[Fletcher Patryk](https://github.com/pfletcher), [Tomasz FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="2ab21-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="2ab21-105">W tym temacie opisano sposób ograniczania, którzy użytkownicy lub role mogą uzyskać dostęp do metod centrów.</span><span class="sxs-lookup"><span data-stu-id="2ab21-105">This topic describes how to restrict which users or roles can access hub methods.</span></span>

## <a name="overview"></a><span data-ttu-id="2ab21-106">Omówienie</span><span class="sxs-lookup"><span data-stu-id="2ab21-106">Overview</span></span>

<span data-ttu-id="2ab21-107">Ten temat zawiera następujące sekcje:</span><span class="sxs-lookup"><span data-stu-id="2ab21-107">This topic contains the following sections:</span></span>

- [<span data-ttu-id="2ab21-108">Autoryzuj atrybut</span><span class="sxs-lookup"><span data-stu-id="2ab21-108">Authorize attribute</span></span>](#authorizeattribute)
- [<span data-ttu-id="2ab21-109">Wymagaj uwierzytelniania dla wszystkich centrów</span><span class="sxs-lookup"><span data-stu-id="2ab21-109">Require authentication for all hubs</span></span>](#requireauth)
- [<span data-ttu-id="2ab21-110">Dostosowana autoryzacja</span><span class="sxs-lookup"><span data-stu-id="2ab21-110">Customized authorization</span></span>](#custom)
- [<span data-ttu-id="2ab21-111">Przekazywanie informacji uwierzytelniania do klientów</span><span class="sxs-lookup"><span data-stu-id="2ab21-111">Pass authentication information to clients</span></span>](#passauth)
- [<span data-ttu-id="2ab21-112">Opcje uwierzytelniania dla klientów platformy .NET</span><span class="sxs-lookup"><span data-stu-id="2ab21-112">Authentication options for .NET clients</span></span>](#authoptions)

    - [<span data-ttu-id="2ab21-113">Plik cookie z uwierzytelnianiem formularzy</span><span class="sxs-lookup"><span data-stu-id="2ab21-113">Cookie with Forms Authentication</span></span>](#cookie)
    - [<span data-ttu-id="2ab21-114">Uwierzytelnianie systemu Windows</span><span class="sxs-lookup"><span data-stu-id="2ab21-114">Windows Authentication</span></span>](#windows)
    - [<span data-ttu-id="2ab21-115">Nagłówek połączenia</span><span class="sxs-lookup"><span data-stu-id="2ab21-115">Connection header</span></span>](#header)
    - [<span data-ttu-id="2ab21-116">Certyfikat</span><span class="sxs-lookup"><span data-stu-id="2ab21-116">Certificate</span></span>](#certificate)

<a id="authorizeattribute"></a>

## <a name="authorize-attribute"></a><span data-ttu-id="2ab21-117">Autoryzuj atrybut</span><span class="sxs-lookup"><span data-stu-id="2ab21-117">Authorize attribute</span></span>

<span data-ttu-id="2ab21-118">Sygnalizujący udostępnia atrybut [Autoryzuj](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) , aby określić, którzy użytkownicy lub które role mają dostęp do centrum lub metody.</span><span class="sxs-lookup"><span data-stu-id="2ab21-118">SignalR provides the [Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) attribute to specify which users or roles have access to a hub or method.</span></span> <span data-ttu-id="2ab21-119">Ten atrybut znajduje się w przestrzeni nazw `Microsoft.AspNet.SignalR`.</span><span class="sxs-lookup"><span data-stu-id="2ab21-119">This attribute is located in the `Microsoft.AspNet.SignalR` namespace.</span></span> <span data-ttu-id="2ab21-120">Należy zastosować atrybut `Authorize` do koncentratora lub określonych metod w centrum.</span><span class="sxs-lookup"><span data-stu-id="2ab21-120">You apply the `Authorize` attribute to either a hub or particular methods in a hub.</span></span> <span data-ttu-id="2ab21-121">Po zastosowaniu atrybutu `Authorize` do klasy Hub określone wymaganie autoryzacji jest stosowane do wszystkich metod w centrum.</span><span class="sxs-lookup"><span data-stu-id="2ab21-121">When you apply the `Authorize` attribute to a hub class, the specified authorization requirement is applied to all of the methods in the hub.</span></span> <span data-ttu-id="2ab21-122">Poniżej przedstawiono różne typy wymagań dotyczących autoryzacji, które można zastosować.</span><span class="sxs-lookup"><span data-stu-id="2ab21-122">The different types of authorization requirements that you can apply are shown below.</span></span> <span data-ttu-id="2ab21-123">Bez atrybutu `Authorize` wszystkie metody publiczne w centrum są dostępne dla klienta, który jest podłączony do centrum.</span><span class="sxs-lookup"><span data-stu-id="2ab21-123">Without the `Authorize` attribute, all public methods on the hub are available to a client that is connected to the hub.</span></span>

<span data-ttu-id="2ab21-124">Jeśli zdefiniowano rolę "admin" w aplikacji sieci Web, można określić, że tylko użytkownicy w tej roli mają dostęp do centrum z poniższym kodem.</span><span class="sxs-lookup"><span data-stu-id="2ab21-124">If you have defined a role named "Admin" in your web application, you could specify that only users in that role can access a hub with the following code.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample1.cs)]

<span data-ttu-id="2ab21-125">Można też określić, że koncentrator zawiera jedną metodę, która jest dostępna dla wszystkich użytkowników, i drugą metodę dostępną tylko dla uwierzytelnionych użytkowników, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="2ab21-125">Or, you can specify that a hub contains one method that is available to all users, and a second method that is only available to authenticated users, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample2.cs)]

<span data-ttu-id="2ab21-126">Poniższe przykłady dotyczą różnych scenariuszy autoryzacji:</span><span class="sxs-lookup"><span data-stu-id="2ab21-126">The following examples address different authorization scenarios:</span></span>

- <span data-ttu-id="2ab21-127">`[Authorize]` — tylko uwierzytelnieni użytkownicy</span><span class="sxs-lookup"><span data-stu-id="2ab21-127">`[Authorize]` – only authenticated users</span></span>
- <span data-ttu-id="2ab21-128">`[Authorize(Roles = "Admin,Manager")]` — tylko uwierzytelnieni użytkownicy w określonych rolach</span><span class="sxs-lookup"><span data-stu-id="2ab21-128">`[Authorize(Roles = "Admin,Manager")]` – only authenticated users in the specified roles</span></span>
- <span data-ttu-id="2ab21-129">`[Authorize(Users = "user1,user2")]` — tylko uwierzytelnieni użytkownicy z określonymi nazwami użytkowników</span><span class="sxs-lookup"><span data-stu-id="2ab21-129">`[Authorize(Users = "user1,user2")]` – only authenticated users with the specified user names</span></span>
- <span data-ttu-id="2ab21-130">`[Authorize(RequireOutgoing=false)]` — tylko uwierzytelnieni użytkownicy mogą wywoływać centrum, ale wywołania z serwera z powrotem do klientów nie są ograniczone przez autoryzację, na przykład w przypadku, gdy tylko niektórzy użytkownicy będą mogli wysyłać wiadomości, ale wszystkie inne mogą odbierać wiadomości.</span><span class="sxs-lookup"><span data-stu-id="2ab21-130">`[Authorize(RequireOutgoing=false)]` – only authenticated users can invoke the hub, but calls from the server back to clients are not limited by authorization, such as, when only certain users can send a message but all others can receive the message.</span></span> <span data-ttu-id="2ab21-131">Właściwość RequireOutgoing może być stosowana tylko do całego centrum, a nie dla poszczególnych metod w centrum.</span><span class="sxs-lookup"><span data-stu-id="2ab21-131">The RequireOutgoing property can only be applied to the entire hub, not on individuals methods within the hub.</span></span> <span data-ttu-id="2ab21-132">Gdy RequireOutgoing nie jest ustawiona na wartość false, z serwera są wywoływane tylko użytkownicy, którzy spełniają wymagania dotyczące autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="2ab21-132">When RequireOutgoing is not set to false, only users that meet the authorization requirement are called from the server.</span></span>

<a id="requireauth"></a>

## <a name="require-authentication-for-all-hubs"></a><span data-ttu-id="2ab21-133">Wymagaj uwierzytelniania dla wszystkich centrów</span><span class="sxs-lookup"><span data-stu-id="2ab21-133">Require authentication for all hubs</span></span>

<span data-ttu-id="2ab21-134">Możesz wymagać uwierzytelniania dla wszystkich centrów i metod koncentratora w aplikacji, wywołując metodę [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2ab21-134">You can require authentication for all hubs and hub methods in your application by calling the [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) method when the application starts.</span></span> <span data-ttu-id="2ab21-135">Tej metody można użyć, jeśli masz wiele centrów i chcesz wymusić wymaganie uwierzytelniania dla wszystkich z nich.</span><span class="sxs-lookup"><span data-stu-id="2ab21-135">You might use this method when you have multiple hubs and want to enforce an authentication requirement for all of them.</span></span> <span data-ttu-id="2ab21-136">W przypadku tej metody nie można określić roli, użytkownika lub autoryzacji wychodzącej.</span><span class="sxs-lookup"><span data-stu-id="2ab21-136">With this method, you cannot specify role, user, or outgoing authorization.</span></span> <span data-ttu-id="2ab21-137">Można określić, że dostęp do metod centrów jest ograniczony tylko do użytkowników uwierzytelnionych.</span><span class="sxs-lookup"><span data-stu-id="2ab21-137">You can only specify that access to the hub methods is restricted to authenticated users.</span></span> <span data-ttu-id="2ab21-138">Można jednak nadal zastosować atrybut Autoryzuj do centrów lub metod, aby określić dodatkowe wymagania.</span><span class="sxs-lookup"><span data-stu-id="2ab21-138">However, you can still apply the Authorize attribute to hubs or methods to specify additional requirements.</span></span> <span data-ttu-id="2ab21-139">Wszelkie wymagania określone w atrybutach są stosowane oprócz podstawowego wymagania uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="2ab21-139">Any requirement you specify in attributes is applied in addition to the basic requirement of authentication.</span></span>

<span data-ttu-id="2ab21-140">W poniższym przykładzie przedstawiono plik Global. asax, który ogranicza wszystkie metody centrum do uwierzytelnionych użytkowników.</span><span class="sxs-lookup"><span data-stu-id="2ab21-140">The following example shows a Global.asax file which restricts all hub methods to authenticated users.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample3.cs)]

<span data-ttu-id="2ab21-141">Jeśli wywołasz metodę `RequireAuthentication()` po przetworzeniu żądania sygnalizującego, sygnalizujący zgłosi wyjątek `InvalidOperationException`.</span><span class="sxs-lookup"><span data-stu-id="2ab21-141">If you call the `RequireAuthentication()` method after a SignalR request has been processed, SignalR will throw a `InvalidOperationException` exception.</span></span> <span data-ttu-id="2ab21-142">Ten wyjątek jest zgłaszany, ponieważ nie można dodać modułu do HubPipeline po wywołaniu potoku.</span><span class="sxs-lookup"><span data-stu-id="2ab21-142">This exception is thrown because you cannot add a module to the HubPipeline after the pipeline has been invoked.</span></span> <span data-ttu-id="2ab21-143">W poprzednim przykładzie pokazano wywoływanie metody `RequireAuthentication` w metodzie `Application_Start`, która jest wykonywana jednokrotnie przed rozpoczęciem obsługi pierwszego żądania.</span><span class="sxs-lookup"><span data-stu-id="2ab21-143">The previous example shows calling the `RequireAuthentication` method in the `Application_Start` method which is executed one time prior to handling the first request.</span></span>

<a id="custom"></a>

## <a name="customized-authorization"></a><span data-ttu-id="2ab21-144">Dostosowana autoryzacja</span><span class="sxs-lookup"><span data-stu-id="2ab21-144">Customized authorization</span></span>

<span data-ttu-id="2ab21-145">Aby dostosować sposób określania autoryzacji, można utworzyć klasę pochodzącą z `AuthorizeAttribute` i zastąpić metodę [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) .</span><span class="sxs-lookup"><span data-stu-id="2ab21-145">If you need to customize how authorization is determined, you can create a class that derives from `AuthorizeAttribute` and override the [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) method.</span></span> <span data-ttu-id="2ab21-146">Ta metoda jest wywoływana dla każdego żądania, aby określić, czy użytkownik jest autoryzowany do wykonania żądania.</span><span class="sxs-lookup"><span data-stu-id="2ab21-146">This method is called for each request to determine whether the user is authorized to complete the request.</span></span> <span data-ttu-id="2ab21-147">W zastąpionej metodzie należy zapewnić logikę niezbędną dla scenariusza autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="2ab21-147">In the overridden method, you provide the necessary logic for your authorization scenario.</span></span> <span data-ttu-id="2ab21-148">Poniższy przykład pokazuje, jak wymusić autoryzację za pośrednictwem tożsamości opartej na oświadczeniach.</span><span class="sxs-lookup"><span data-stu-id="2ab21-148">The following example shows how to enforce authorization through claims-based identity.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample4.cs)]

<a id="passauth"></a>

## <a name="pass-authentication-information-to-clients"></a><span data-ttu-id="2ab21-149">Przekazywanie informacji uwierzytelniania do klientów</span><span class="sxs-lookup"><span data-stu-id="2ab21-149">Pass authentication information to clients</span></span>

<span data-ttu-id="2ab21-150">Może być konieczne użycie informacji o uwierzytelnianiu w kodzie, który jest uruchamiany na kliencie programu.</span><span class="sxs-lookup"><span data-stu-id="2ab21-150">You may need to use authentication information in the code that runs on the client.</span></span> <span data-ttu-id="2ab21-151">Wymagane informacje są przekazywane podczas wywoływania metod na kliencie.</span><span class="sxs-lookup"><span data-stu-id="2ab21-151">You pass the required information when calling the methods on the client.</span></span> <span data-ttu-id="2ab21-152">Na przykład metoda aplikacji Chat może przekazać jako parametr nazwę użytkownika osoby ogłaszającej komunikat, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="2ab21-152">For example, a chat application method could pass as a parameter the user name of the person posting a message, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample5.cs)]

<span data-ttu-id="2ab21-153">Można też utworzyć obiekt reprezentujący informacje o uwierzytelnianiu i przekazać go jako parametr, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="2ab21-153">Or, you can create an object to represent the authentication information and pass that object as a parameter, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample6.cs)]

<span data-ttu-id="2ab21-154">Nie należy przekazywać identyfikatora połączenia jednego klienta do innych klientów, ponieważ złośliwy użytkownik może użyć go do naśladowania żądania od tego klienta.</span><span class="sxs-lookup"><span data-stu-id="2ab21-154">You should never pass one client's connection id to other clients, as a malicious user could use it to mimic a request from that client.</span></span>

<a id="authoptions"></a>

## <a name="authentication-options-for-net-clients"></a><span data-ttu-id="2ab21-155">Opcje uwierzytelniania dla klientów platformy .NET</span><span class="sxs-lookup"><span data-stu-id="2ab21-155">Authentication options for .NET clients</span></span>

<span data-ttu-id="2ab21-156">W przypadku klienta platformy .NET, takiego jak Aplikacja konsolowa, która współdziała z koncentratorem ograniczonym do uwierzytelnionych użytkowników, można przekazać poświadczenia uwierzytelniania w pliku cookie, w nagłówku połączenia lub w certyfikacie.</span><span class="sxs-lookup"><span data-stu-id="2ab21-156">When you have a .NET client, such as a console app, which interacts with a hub that is limited to authenticated users, you can pass the authentication credentials in a cookie, the connection header, or a certificate.</span></span> <span data-ttu-id="2ab21-157">W przykładach w tej sekcji pokazano, jak używać różnych metod uwierzytelniania użytkownika.</span><span class="sxs-lookup"><span data-stu-id="2ab21-157">The examples in this section show how to use those different methods for authenticating a user.</span></span> <span data-ttu-id="2ab21-158">Nie są w pełni funkcjonalne aplikacje sygnalizujące.</span><span class="sxs-lookup"><span data-stu-id="2ab21-158">They are not fully-functional SignalR apps.</span></span> <span data-ttu-id="2ab21-159">Aby uzyskać więcej informacji na temat klientów .NET z sygnalizującym, zobacz temat [interfejs API centrów — klient platformy .NET](../guide-to-the-api/hubs-api-guide-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="2ab21-159">For more information about .NET clients with SignalR, see [Hubs API Guide - .NET Client](../guide-to-the-api/hubs-api-guide-net-client.md).</span></span>

<a id="cookie"></a>

### <a name="cookie"></a><span data-ttu-id="2ab21-160">Plik cookie</span><span class="sxs-lookup"><span data-stu-id="2ab21-160">Cookie</span></span>

<span data-ttu-id="2ab21-161">Gdy klient platformy .NET współdziała z centrum korzystającym z uwierzytelniania formularzy ASP.NET, trzeba ręcznie ustawić plik cookie uwierzytelniania w ramach połączenia.</span><span class="sxs-lookup"><span data-stu-id="2ab21-161">When your .NET client interacts with a hub that uses ASP.NET Forms Authentication, you will need to manually set the authentication cookie on the connection.</span></span> <span data-ttu-id="2ab21-162">Plik cookie należy dodać do właściwości `CookieContainer` obiektu [HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) .</span><span class="sxs-lookup"><span data-stu-id="2ab21-162">You add the cookie to the `CookieContainer` property on the [HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) object.</span></span> <span data-ttu-id="2ab21-163">W poniższym przykładzie przedstawiono aplikację konsolową, która pobiera plik cookie uwierzytelniania ze strony sieci Web i dodaje ten plik cookie do połączenia.</span><span class="sxs-lookup"><span data-stu-id="2ab21-163">The following example shows a console app that retrieves an authentication cookie from a web page and adds that cookie to the connection.</span></span> <span data-ttu-id="2ab21-164">Adres URL `https://www.contoso.com/RemoteLogin` w przykładzie wskazuje stronę sieci Web, którą należy utworzyć.</span><span class="sxs-lookup"><span data-stu-id="2ab21-164">The URL `https://www.contoso.com/RemoteLogin` in the example points to a web page that you would need to create.</span></span> <span data-ttu-id="2ab21-165">Strona pobierze nazwę użytkownika i hasło, a następnie spróbuje zalogować użytkownika przy użyciu poświadczeń.</span><span class="sxs-lookup"><span data-stu-id="2ab21-165">The page would retrieve the posted user name and password, and attempt to log in the user with the credentials.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample7.cs)]

<span data-ttu-id="2ab21-166">Aplikacja konsoli ogłasza poświadczenia www.contoso.com/RemoteLogin, które mogą odwoływać się do pustej strony zawierającej następujący plik związany z kodem.</span><span class="sxs-lookup"><span data-stu-id="2ab21-166">The console app posts the credentials to www.contoso.com/RemoteLogin which could refer to an empty page that contains the following code-behind file.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample8.cs)]

<a id="windows"></a>

### <a name="windows-authentication"></a><span data-ttu-id="2ab21-167">Uwierzytelnianie systemu Windows</span><span class="sxs-lookup"><span data-stu-id="2ab21-167">Windows authentication</span></span>

<span data-ttu-id="2ab21-168">W przypadku korzystania z uwierzytelniania systemu Windows można przekazać poświadczenia bieżącego użytkownika za pomocą właściwości [DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx) .</span><span class="sxs-lookup"><span data-stu-id="2ab21-168">When using Windows authentication, you can pass the current user's credentials by using the [DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx) property.</span></span> <span data-ttu-id="2ab21-169">Poświadczenia dla połączenia należy ustawić na wartość DefaultCredentials.</span><span class="sxs-lookup"><span data-stu-id="2ab21-169">You set the credentials for the connection to the value of the DefaultCredentials.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample9.cs?highlight=6)]

<a id="header"></a>

### <a name="connection-header"></a><span data-ttu-id="2ab21-170">Nagłówek połączenia</span><span class="sxs-lookup"><span data-stu-id="2ab21-170">Connection header</span></span>

<span data-ttu-id="2ab21-171">Jeśli aplikacja nie używa plików cookie, można przekazać informacje o użytkowniku w nagłówku połączenia.</span><span class="sxs-lookup"><span data-stu-id="2ab21-171">If your application is not using cookies, you can pass user information in the connection header.</span></span> <span data-ttu-id="2ab21-172">Na przykład można przekazać token w nagłówku połączenia.</span><span class="sxs-lookup"><span data-stu-id="2ab21-172">For example, you can pass a token in the connection header.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample10.cs?highlight=6)]

<span data-ttu-id="2ab21-173">Następnie w centrum należy zweryfikować token użytkownika.</span><span class="sxs-lookup"><span data-stu-id="2ab21-173">Then, in the hub, you would verify the user's token.</span></span>

<a id="certificate"></a>

### <a name="certificate"></a><span data-ttu-id="2ab21-174">Certyfikat</span><span class="sxs-lookup"><span data-stu-id="2ab21-174">Certificate</span></span>

<span data-ttu-id="2ab21-175">Można przekazać certyfikat klienta, aby zweryfikować użytkownika.</span><span class="sxs-lookup"><span data-stu-id="2ab21-175">You can pass a client certificate to verify the user.</span></span> <span data-ttu-id="2ab21-176">Podczas tworzenia połączenia zostanie dodany certyfikat.</span><span class="sxs-lookup"><span data-stu-id="2ab21-176">You add the certificate when creating the connection.</span></span> <span data-ttu-id="2ab21-177">Poniższy przykład pokazuje, jak dodać certyfikat klienta do połączenia; nie jest wyświetlana pełna Aplikacja konsolowa.</span><span class="sxs-lookup"><span data-stu-id="2ab21-177">The following example shows only how to add a client certificate to the connection; it does not show the full console app.</span></span> <span data-ttu-id="2ab21-178">Używa klasy [x509](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx) , która zapewnia kilka różnych sposobów tworzenia certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="2ab21-178">It uses the [X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx) class which provides several different ways to create the certificate.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample11.cs?highlight=6)]

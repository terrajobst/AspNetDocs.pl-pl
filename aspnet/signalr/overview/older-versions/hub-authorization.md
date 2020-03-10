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
# <a name="authentication-and-authorization-for-signalr-hubs-signalr-1x"></a>Uwierzytelnianie i autoryzacja centrów usługi SignalR (SignalR 1.x)

[Fletcher Patryk](https://github.com/pfletcher), [Tomasz FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> W tym temacie opisano sposób ograniczania, którzy użytkownicy lub role mogą uzyskać dostęp do metod centrów.

## <a name="overview"></a>Omówienie

Ten temat zawiera następujące sekcje:

- [Autoryzuj atrybut](#authorizeattribute)
- [Wymagaj uwierzytelniania dla wszystkich centrów](#requireauth)
- [Dostosowana autoryzacja](#custom)
- [Przekazywanie informacji uwierzytelniania do klientów](#passauth)
- [Opcje uwierzytelniania dla klientów platformy .NET](#authoptions)

    - [Plik cookie z uwierzytelnianiem formularzy](#cookie)
    - [Uwierzytelnianie systemu Windows](#windows)
    - [Nagłówek połączenia](#header)
    - [Certyfikat](#certificate)

<a id="authorizeattribute"></a>

## <a name="authorize-attribute"></a>Autoryzuj atrybut

Sygnalizujący udostępnia atrybut [Autoryzuj](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) , aby określić, którzy użytkownicy lub które role mają dostęp do centrum lub metody. Ten atrybut znajduje się w przestrzeni nazw `Microsoft.AspNet.SignalR`. Należy zastosować atrybut `Authorize` do koncentratora lub określonych metod w centrum. Po zastosowaniu atrybutu `Authorize` do klasy Hub określone wymaganie autoryzacji jest stosowane do wszystkich metod w centrum. Poniżej przedstawiono różne typy wymagań dotyczących autoryzacji, które można zastosować. Bez atrybutu `Authorize` wszystkie metody publiczne w centrum są dostępne dla klienta, który jest podłączony do centrum.

Jeśli zdefiniowano rolę "admin" w aplikacji sieci Web, można określić, że tylko użytkownicy w tej roli mają dostęp do centrum z poniższym kodem.

[!code-csharp[Main](hub-authorization/samples/sample1.cs)]

Można też określić, że koncentrator zawiera jedną metodę, która jest dostępna dla wszystkich użytkowników, i drugą metodę dostępną tylko dla uwierzytelnionych użytkowników, jak pokazano poniżej.

[!code-csharp[Main](hub-authorization/samples/sample2.cs)]

Poniższe przykłady dotyczą różnych scenariuszy autoryzacji:

- `[Authorize]` — tylko uwierzytelnieni użytkownicy
- `[Authorize(Roles = "Admin,Manager")]` — tylko uwierzytelnieni użytkownicy w określonych rolach
- `[Authorize(Users = "user1,user2")]` — tylko uwierzytelnieni użytkownicy z określonymi nazwami użytkowników
- `[Authorize(RequireOutgoing=false)]` — tylko uwierzytelnieni użytkownicy mogą wywoływać centrum, ale wywołania z serwera z powrotem do klientów nie są ograniczone przez autoryzację, na przykład w przypadku, gdy tylko niektórzy użytkownicy będą mogli wysyłać wiadomości, ale wszystkie inne mogą odbierać wiadomości. Właściwość RequireOutgoing może być stosowana tylko do całego centrum, a nie dla poszczególnych metod w centrum. Gdy RequireOutgoing nie jest ustawiona na wartość false, z serwera są wywoływane tylko użytkownicy, którzy spełniają wymagania dotyczące autoryzacji.

<a id="requireauth"></a>

## <a name="require-authentication-for-all-hubs"></a>Wymagaj uwierzytelniania dla wszystkich centrów

Możesz wymagać uwierzytelniania dla wszystkich centrów i metod koncentratora w aplikacji, wywołując metodę [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) podczas uruchamiania aplikacji. Tej metody można użyć, jeśli masz wiele centrów i chcesz wymusić wymaganie uwierzytelniania dla wszystkich z nich. W przypadku tej metody nie można określić roli, użytkownika lub autoryzacji wychodzącej. Można określić, że dostęp do metod centrów jest ograniczony tylko do użytkowników uwierzytelnionych. Można jednak nadal zastosować atrybut Autoryzuj do centrów lub metod, aby określić dodatkowe wymagania. Wszelkie wymagania określone w atrybutach są stosowane oprócz podstawowego wymagania uwierzytelniania.

W poniższym przykładzie przedstawiono plik Global. asax, który ogranicza wszystkie metody centrum do uwierzytelnionych użytkowników.

[!code-csharp[Main](hub-authorization/samples/sample3.cs)]

Jeśli wywołasz metodę `RequireAuthentication()` po przetworzeniu żądania sygnalizującego, sygnalizujący zgłosi wyjątek `InvalidOperationException`. Ten wyjątek jest zgłaszany, ponieważ nie można dodać modułu do HubPipeline po wywołaniu potoku. W poprzednim przykładzie pokazano wywoływanie metody `RequireAuthentication` w metodzie `Application_Start`, która jest wykonywana jednokrotnie przed rozpoczęciem obsługi pierwszego żądania.

<a id="custom"></a>

## <a name="customized-authorization"></a>Dostosowana autoryzacja

Aby dostosować sposób określania autoryzacji, można utworzyć klasę pochodzącą z `AuthorizeAttribute` i zastąpić metodę [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) . Ta metoda jest wywoływana dla każdego żądania, aby określić, czy użytkownik jest autoryzowany do wykonania żądania. W zastąpionej metodzie należy zapewnić logikę niezbędną dla scenariusza autoryzacji. Poniższy przykład pokazuje, jak wymusić autoryzację za pośrednictwem tożsamości opartej na oświadczeniach.

[!code-csharp[Main](hub-authorization/samples/sample4.cs)]

<a id="passauth"></a>

## <a name="pass-authentication-information-to-clients"></a>Przekazywanie informacji uwierzytelniania do klientów

Może być konieczne użycie informacji o uwierzytelnianiu w kodzie, który jest uruchamiany na kliencie programu. Wymagane informacje są przekazywane podczas wywoływania metod na kliencie. Na przykład metoda aplikacji Chat może przekazać jako parametr nazwę użytkownika osoby ogłaszającej komunikat, jak pokazano poniżej.

[!code-csharp[Main](hub-authorization/samples/sample5.cs)]

Można też utworzyć obiekt reprezentujący informacje o uwierzytelnianiu i przekazać go jako parametr, jak pokazano poniżej.

[!code-csharp[Main](hub-authorization/samples/sample6.cs)]

Nie należy przekazywać identyfikatora połączenia jednego klienta do innych klientów, ponieważ złośliwy użytkownik może użyć go do naśladowania żądania od tego klienta.

<a id="authoptions"></a>

## <a name="authentication-options-for-net-clients"></a>Opcje uwierzytelniania dla klientów platformy .NET

W przypadku klienta platformy .NET, takiego jak Aplikacja konsolowa, która współdziała z koncentratorem ograniczonym do uwierzytelnionych użytkowników, można przekazać poświadczenia uwierzytelniania w pliku cookie, w nagłówku połączenia lub w certyfikacie. W przykładach w tej sekcji pokazano, jak używać różnych metod uwierzytelniania użytkownika. Nie są w pełni funkcjonalne aplikacje sygnalizujące. Aby uzyskać więcej informacji na temat klientów .NET z sygnalizującym, zobacz temat [interfejs API centrów — klient platformy .NET](../guide-to-the-api/hubs-api-guide-net-client.md).

<a id="cookie"></a>

### <a name="cookie"></a>Plik cookie

Gdy klient platformy .NET współdziała z centrum korzystającym z uwierzytelniania formularzy ASP.NET, trzeba ręcznie ustawić plik cookie uwierzytelniania w ramach połączenia. Plik cookie należy dodać do właściwości `CookieContainer` obiektu [HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) . W poniższym przykładzie przedstawiono aplikację konsolową, która pobiera plik cookie uwierzytelniania ze strony sieci Web i dodaje ten plik cookie do połączenia. Adres URL `https://www.contoso.com/RemoteLogin` w przykładzie wskazuje stronę sieci Web, którą należy utworzyć. Strona pobierze nazwę użytkownika i hasło, a następnie spróbuje zalogować użytkownika przy użyciu poświadczeń.

[!code-csharp[Main](hub-authorization/samples/sample7.cs)]

Aplikacja konsoli ogłasza poświadczenia www.contoso.com/RemoteLogin, które mogą odwoływać się do pustej strony zawierającej następujący plik związany z kodem.

[!code-csharp[Main](hub-authorization/samples/sample8.cs)]

<a id="windows"></a>

### <a name="windows-authentication"></a>Uwierzytelnianie systemu Windows

W przypadku korzystania z uwierzytelniania systemu Windows można przekazać poświadczenia bieżącego użytkownika za pomocą właściwości [DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx) . Poświadczenia dla połączenia należy ustawić na wartość DefaultCredentials.

[!code-csharp[Main](hub-authorization/samples/sample9.cs?highlight=6)]

<a id="header"></a>

### <a name="connection-header"></a>Nagłówek połączenia

Jeśli aplikacja nie używa plików cookie, można przekazać informacje o użytkowniku w nagłówku połączenia. Na przykład można przekazać token w nagłówku połączenia.

[!code-csharp[Main](hub-authorization/samples/sample10.cs?highlight=6)]

Następnie w centrum należy zweryfikować token użytkownika.

<a id="certificate"></a>

### <a name="certificate"></a>Certyfikat

Można przekazać certyfikat klienta, aby zweryfikować użytkownika. Podczas tworzenia połączenia zostanie dodany certyfikat. Poniższy przykład pokazuje, jak dodać certyfikat klienta do połączenia; nie jest wyświetlana pełna Aplikacja konsolowa. Używa klasy [x509](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx) , która zapewnia kilka różnych sposobów tworzenia certyfikatu.

[!code-csharp[Main](hub-authorization/samples/sample11.cs?highlight=6)]

---
uid: signalr/overview/guide-to-the-api/mapping-users-to-connections
title: Mapowanie użytkowników sygnalizujących do połączeń | Microsoft Docs
author: bradygaster
description: W tym temacie pokazano, jak zachować informacje o użytkownikach i ich połączeniach. W tym temacie pomogła Ci zanotować ten temat. Wersje oprogramowania używane w tym temacie...
ms.author: bradyg
ms.date: 12/30/2014
ms.assetid: f80c08b1-3f1f-432c-980c-c7b6edeb31b1
msc.legacyurl: /signalr/overview/guide-to-the-api/mapping-users-to-connections
msc.type: authoredcontent
ms.openlocfilehash: d55d40848e1e9d40570850c3552b225235c5e814
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536779"
---
# <a name="mapping-signalr-users-to-connections"></a><span data-ttu-id="c2777-105">Mapowanie użytkowników usługi SignalR na połączenia</span><span class="sxs-lookup"><span data-stu-id="c2777-105">Mapping SignalR Users to Connections</span></span>

<span data-ttu-id="c2777-106">Autor [FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="c2777-106">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="c2777-107">W tym temacie pokazano, jak zachować informacje o użytkownikach i ich połączeniach.</span><span class="sxs-lookup"><span data-stu-id="c2777-107">This topic shows how to retain information about users and their connections.</span></span>
>
> <span data-ttu-id="c2777-108">W tym temacie pomogła Ci zanotować ten temat.</span><span class="sxs-lookup"><span data-stu-id="c2777-108">Patrick Fletcher helped write this topic.</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="c2777-109">Wersje oprogramowania używane w tym temacie</span><span class="sxs-lookup"><span data-stu-id="c2777-109">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="c2777-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="c2777-110">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="c2777-111">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="c2777-111">.NET 4.5</span></span>
> - <span data-ttu-id="c2777-112">Sygnalizujący wersja 2</span><span class="sxs-lookup"><span data-stu-id="c2777-112">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="c2777-113">Poprzednie wersje tego tematu</span><span class="sxs-lookup"><span data-stu-id="c2777-113">Previous versions of this topic</span></span>
>
> <span data-ttu-id="c2777-114">Aby uzyskać informacje o wcześniejszych wersjach programu sygnalizującego, zobacz sekcję [sygnalizujące starsze wersje](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="c2777-114">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="c2777-115">Pytania i Komentarze</span><span class="sxs-lookup"><span data-stu-id="c2777-115">Questions and comments</span></span>
>
> <span data-ttu-id="c2777-116">Prosimy o opinię na temat sposobu, w jaki lubię ten samouczek, i co możemy ulepszyć w komentarzach w dolnej części strony.</span><span class="sxs-lookup"><span data-stu-id="c2777-116">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="c2777-117">Jeśli masz pytania, które nie są bezpośrednio związane z samouczkiem, możesz je ogłosić na [forum ASP.NET](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="c2777-117">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

## <a name="introduction"></a><span data-ttu-id="c2777-118">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="c2777-118">Introduction</span></span>

<span data-ttu-id="c2777-119">Każdy Klient nawiązujący połączenie z centrum przekazuje unikatowy identyfikator połączenia. Tę wartość można pobrać we właściwości `Context.ConnectionId` kontekstu centrum.</span><span class="sxs-lookup"><span data-stu-id="c2777-119">Each client connecting to a hub passes a unique connection id. You can retrieve this value in the `Context.ConnectionId` property of the hub context.</span></span> <span data-ttu-id="c2777-120">Jeśli aplikacja musi zmapować użytkownika na identyfikator połączenia i zachować to mapowanie, można użyć jednej z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="c2777-120">If your application needs to map a user to the connection id and persist that mapping, you can use one of the following:</span></span>

- [<span data-ttu-id="c2777-121">Dostawca identyfikatora użytkownika (sygnalizujący 2)</span><span class="sxs-lookup"><span data-stu-id="c2777-121">The User ID Provider (SignalR 2)</span></span>](#IUserIdProvider)
- <span data-ttu-id="c2777-122">[Magazyn w pamięci](#inmemory), taki jak słownik</span><span class="sxs-lookup"><span data-stu-id="c2777-122">[In-memory storage](#inmemory), such as a dictionary</span></span>
- [<span data-ttu-id="c2777-123">Grupa sygnałów dla każdego użytkownika</span><span class="sxs-lookup"><span data-stu-id="c2777-123">SignalR group for each user</span></span>](#groups)
- <span data-ttu-id="c2777-124">[Trwały, zewnętrzny magazyn](#database), taki jak tabela bazy danych lub usługa Azure Table Storage</span><span class="sxs-lookup"><span data-stu-id="c2777-124">[Permanent, external storage](#database), such as a database table or Azure table storage</span></span>

<span data-ttu-id="c2777-125">Każda z tych implementacji jest przedstawiona w tym temacie.</span><span class="sxs-lookup"><span data-stu-id="c2777-125">Each of these implementations is shown in this topic.</span></span> <span data-ttu-id="c2777-126">Możesz użyć metod `OnConnected`, `OnDisconnected`i `OnReconnected` klasy `Hub` do śledzenia stanu połączenia użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c2777-126">You use the `OnConnected`, `OnDisconnected`, and `OnReconnected` methods of the `Hub` class to track the user connection status.</span></span>

<span data-ttu-id="c2777-127">Najlepsze podejście do aplikacji zależy od następujących:</span><span class="sxs-lookup"><span data-stu-id="c2777-127">The best approach for your application depends on:</span></span>

- <span data-ttu-id="c2777-128">Liczba serwerów sieci Web, które obsługują aplikację.</span><span class="sxs-lookup"><span data-stu-id="c2777-128">The number of web servers hosting your application.</span></span>
- <span data-ttu-id="c2777-129">Czy musisz uzyskać listę aktualnie połączonych użytkowników.</span><span class="sxs-lookup"><span data-stu-id="c2777-129">Whether you need to get a list of the currently connected users.</span></span>
- <span data-ttu-id="c2777-130">Czy po ponownym uruchomieniu aplikacji lub serwera należy zachować informacje o grupach i użytkownikach.</span><span class="sxs-lookup"><span data-stu-id="c2777-130">Whether you need to persist group and user information when the application or server restarts.</span></span>
- <span data-ttu-id="c2777-131">Czy opóźnienie wywołania serwera zewnętrznego jest problemem.</span><span class="sxs-lookup"><span data-stu-id="c2777-131">Whether the latency of calling an external server is an issue.</span></span>

<span data-ttu-id="c2777-132">W poniższej tabeli przedstawiono podejście, które ma zastosowanie do tych zagadnień.</span><span class="sxs-lookup"><span data-stu-id="c2777-132">The following table shows which approach works for these considerations.</span></span>

|  | <span data-ttu-id="c2777-133">Więcej niż jeden serwer</span><span class="sxs-lookup"><span data-stu-id="c2777-133">More than one server</span></span> | <span data-ttu-id="c2777-134">Pobierz listę aktualnie połączonych użytkowników</span><span class="sxs-lookup"><span data-stu-id="c2777-134">Get list of currently connected users</span></span> | <span data-ttu-id="c2777-135">Utrwalaj informacje po ponownym uruchomieniu</span><span class="sxs-lookup"><span data-stu-id="c2777-135">Persist information after restarts</span></span> | <span data-ttu-id="c2777-136">Optymalna wydajność</span><span class="sxs-lookup"><span data-stu-id="c2777-136">Optimal performance</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="c2777-137">Dostawca UserID</span><span class="sxs-lookup"><span data-stu-id="c2777-137">UserID Provider</span></span> | ![](mapping-users-to-connections/_static/image1.png) |  |  | ![](mapping-users-to-connections/_static/image2.png) |
| <span data-ttu-id="c2777-138">W pamięci</span><span class="sxs-lookup"><span data-stu-id="c2777-138">In-memory</span></span> |  | ![](mapping-users-to-connections/_static/image3.png) |  | ![](mapping-users-to-connections/_static/image4.png) |
| <span data-ttu-id="c2777-139">Grupy pojedynczego użytkownika</span><span class="sxs-lookup"><span data-stu-id="c2777-139">Single-user groups</span></span> | ![](mapping-users-to-connections/_static/image5.png) |  |  | ![](mapping-users-to-connections/_static/image6.png) |
| <span data-ttu-id="c2777-140">Stałe, zewnętrzne</span><span class="sxs-lookup"><span data-stu-id="c2777-140">Permanent, external</span></span> | ![](mapping-users-to-connections/_static/image7.png) | ![](mapping-users-to-connections/_static/image8.png) | ![](mapping-users-to-connections/_static/image9.png) |  |

<a id="IUserIdProvider"></a>

## <a name="iuserid-provider"></a><span data-ttu-id="c2777-141">Dostawca IUserID</span><span class="sxs-lookup"><span data-stu-id="c2777-141">IUserID provider</span></span>

<span data-ttu-id="c2777-142">Ta funkcja umożliwia użytkownikom określenie, jakie elementy userId bazują na IRequest za pośrednictwem nowego interfejsu IUserIdProvider.</span><span class="sxs-lookup"><span data-stu-id="c2777-142">This feature allows users to specify what the userId is based on an IRequest via a new interface IUserIdProvider.</span></span>

<span data-ttu-id="c2777-143">**IUserIdProvider**</span><span class="sxs-lookup"><span data-stu-id="c2777-143">**The IUserIdProvider**</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

<span data-ttu-id="c2777-144">Domyślnie zostanie zaimplementowana implementacja, która używa `IPrincipal.Identity.Name` użytkownika jako nazwy użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c2777-144">By default, there will be an implementation that uses the user's `IPrincipal.Identity.Name` as the user name.</span></span> <span data-ttu-id="c2777-145">Aby to zmienić, zarejestruj implementację `IUserIdProvider` z hostem globalnym podczas uruchamiania aplikacji:</span><span class="sxs-lookup"><span data-stu-id="c2777-145">To change this, register your implementation of `IUserIdProvider` with the global host when your application starts:</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

<span data-ttu-id="c2777-146">Z poziomu centrum będziesz w stanie wysyłać komunikaty do tych użytkowników za pośrednictwem następującego interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="c2777-146">From within a hub, you'll be able to send messages to these users via the following API:</span></span>

<span data-ttu-id="c2777-147">**Wysyłanie komunikatu do określonego użytkownika**</span><span class="sxs-lookup"><span data-stu-id="c2777-147">**Sending a message to a specific user**</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs?highlight=5)]

<a id="inmemory"></a>

## <a name="in-memory-storage"></a><span data-ttu-id="c2777-148">Magazyn w pamięci</span><span class="sxs-lookup"><span data-stu-id="c2777-148">In-memory storage</span></span>

<span data-ttu-id="c2777-149">W poniższych przykładach pokazano, jak zachować połączenie i informacje o użytkowniku w słowniku, który jest przechowywany w pamięci.</span><span class="sxs-lookup"><span data-stu-id="c2777-149">The following examples show how to retain connection and user information in a dictionary that is stored in memory.</span></span> <span data-ttu-id="c2777-150">Słownik używa `HashSet` do przechowywania identyfikatora połączenia. W dowolnym momencie użytkownik może mieć więcej niż jedno połączenie z aplikacją sygnalizującej.</span><span class="sxs-lookup"><span data-stu-id="c2777-150">The dictionary uses a `HashSet` to store the connection id. At any time a user could have more than one connection to the SignalR application.</span></span> <span data-ttu-id="c2777-151">Na przykład użytkownik połączony za pomocą wielu urządzeń lub więcej niż jedna karta przeglądarki ma więcej niż jeden identyfikator połączenia.</span><span class="sxs-lookup"><span data-stu-id="c2777-151">For example, a user who is connected through multiple devices or more than one browser tab would have more than one connection id.</span></span>

<span data-ttu-id="c2777-152">Jeśli aplikacja zostanie ZAMKNIĘTA, wszystkie informacje zostaną utracone, ale zostanie ponownie wypełnione, gdy użytkownicy ponownie ustalą swoje połączenia.</span><span class="sxs-lookup"><span data-stu-id="c2777-152">If the application shuts down, all of the information is lost, but it will be re-populated as the users re-establish their connections.</span></span> <span data-ttu-id="c2777-153">Magazyn w pamięci nie działa, jeśli środowisko zawiera więcej niż jeden serwer sieci Web, ponieważ każdy serwer będzie miał osobną kolekcję połączeń.</span><span class="sxs-lookup"><span data-stu-id="c2777-153">In-memory storage does not work if your environment includes more than one web server because each server would have a separate collection of connections.</span></span>

<span data-ttu-id="c2777-154">Pierwszy przykład przedstawia klasę, która zarządza mapowaniem użytkowników do połączeń.</span><span class="sxs-lookup"><span data-stu-id="c2777-154">The first example shows a class that manages the mapping of users to connections.</span></span> <span data-ttu-id="c2777-155">Kluczem dla HashSet — będzie nazwa użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c2777-155">The key for the HashSet will be the user's name.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

<span data-ttu-id="c2777-156">W następnym przykładzie pokazano, jak używać klasy mapowania połączeń z poziomu centrum.</span><span class="sxs-lookup"><span data-stu-id="c2777-156">The next example shows how to use the connection mapping class from a hub.</span></span> <span data-ttu-id="c2777-157">Wystąpienie klasy jest przechowywane w nazwie zmiennej `_connections`.</span><span class="sxs-lookup"><span data-stu-id="c2777-157">The instance of the class is stored in a variable name `_connections`.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a><span data-ttu-id="c2777-158">Grupy pojedynczego użytkownika</span><span class="sxs-lookup"><span data-stu-id="c2777-158">Single-user groups</span></span>

<span data-ttu-id="c2777-159">Możesz utworzyć grupę dla każdego użytkownika, a następnie wysłać wiadomość do tej grupy, gdy chcesz uzyskać dostęp tylko do tego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c2777-159">You can create a group for each user, and then send a message to that group when you want to reach only that user.</span></span> <span data-ttu-id="c2777-160">Nazwa każdej grupy to nazwa użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c2777-160">The name of each group is the name of the user.</span></span> <span data-ttu-id="c2777-161">Jeśli użytkownik ma więcej niż jedno połączenie, każdy identyfikator połączenia zostanie dodany do grupy użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c2777-161">If a user has more than one connection, each connection id is added to the user's group.</span></span>

<span data-ttu-id="c2777-162">Nie należy ręcznie usuwać użytkownika z grupy, gdy nastąpi odłączenie użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c2777-162">You should not manually remove the user from the group when the user disconnects.</span></span> <span data-ttu-id="c2777-163">Ta akcja jest wykonywana automatycznie przez platformę sygnalizującą.</span><span class="sxs-lookup"><span data-stu-id="c2777-163">This action is automatically performed by the SignalR framework.</span></span>

<span data-ttu-id="c2777-164">Poniższy przykład pokazuje, jak zaimplementować grupy pojedynczego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c2777-164">The following example shows how to implement single-user groups.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a><span data-ttu-id="c2777-165">Trwały magazyn zewnętrzny</span><span class="sxs-lookup"><span data-stu-id="c2777-165">Permanent, external storage</span></span>

<span data-ttu-id="c2777-166">W tym temacie przedstawiono sposób korzystania z bazy danych lub magazynu tabel platformy Azure do przechowywania informacji o połączeniu.</span><span class="sxs-lookup"><span data-stu-id="c2777-166">This topic shows how to use either a database or Azure table storage for storing connection information.</span></span> <span data-ttu-id="c2777-167">Takie podejście działa, gdy istnieje wiele serwerów sieci Web, ponieważ każdy serwer sieci Web może współdziałać z tym samym repozytorium danych.</span><span class="sxs-lookup"><span data-stu-id="c2777-167">This approach works when you have multiple web servers because each web server can interact with the same data repository.</span></span> <span data-ttu-id="c2777-168">Jeśli serwery sieci Web przestaną działać lub aplikacja zostanie uruchomiona ponownie, Metoda `OnDisconnected` nie zostanie wywołana.</span><span class="sxs-lookup"><span data-stu-id="c2777-168">If your web servers stop working or the application restarts, the `OnDisconnected` method is not called.</span></span> <span data-ttu-id="c2777-169">W związku z tym, istnieje możliwość, że repozytorium danych będzie zawierało rekordy identyfikatorów połączeń, które nie są już prawidłowe.</span><span class="sxs-lookup"><span data-stu-id="c2777-169">Therefore, it is possible that your data repository will have records for connection ids that are no longer valid.</span></span> <span data-ttu-id="c2777-170">Aby wyczyścić te oddzielone rekordy, możesz unieważnić wszelkie połączenia, które zostały utworzone poza przedziałem czasu, który jest istotny dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c2777-170">To clean up these orphaned records, you may wish to invalidate any connection that was created outside of a timeframe that is relevant to your application.</span></span> <span data-ttu-id="c2777-171">Przykłady w tej sekcji zawierają wartość śledzenia podczas tworzenia połączenia, ale nie pokazują, jak oczyścić stare rekordy, ponieważ warto to zrobić jako proces w tle.</span><span class="sxs-lookup"><span data-stu-id="c2777-171">The examples in this section include a value for tracking when the connection was created, but do not show how to clean up old records because you may want to do that as background process.</span></span>

### <a name="database"></a><span data-ttu-id="c2777-172">Baza danych</span><span class="sxs-lookup"><span data-stu-id="c2777-172">Database</span></span>

<span data-ttu-id="c2777-173">W poniższych przykładach pokazano, jak zachować połączenie i informacje o użytkowniku w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="c2777-173">The following examples show how to retain connection and user information in a database.</span></span> <span data-ttu-id="c2777-174">Możesz użyć dowolnej technologii dostępu do danych. w poniższym przykładzie pokazano, jak definiować modele przy użyciu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="c2777-174">You can use any data access technology; however, the example below shows how to define models using Entity Framework.</span></span> <span data-ttu-id="c2777-175">Te modele jednostek odpowiadają tabelom i polom bazy danych.</span><span class="sxs-lookup"><span data-stu-id="c2777-175">These entity models correspond to database tables and fields.</span></span> <span data-ttu-id="c2777-176">Struktura danych może się znacznie różnić w zależności od wymagań aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c2777-176">Your data structure could vary considerably depending on the requirements of your application.</span></span>

<span data-ttu-id="c2777-177">Pierwszy przykład pokazuje, jak zdefiniować jednostkę użytkownika, która może być skojarzona z wieloma jednostkami połączenia.</span><span class="sxs-lookup"><span data-stu-id="c2777-177">The first example shows how to define a user entity that can be associated with many connection entities.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]

<span data-ttu-id="c2777-178">Następnie z poziomu centrum można śledzić stan każdego połączenia z kodem pokazanym poniżej.</span><span class="sxs-lookup"><span data-stu-id="c2777-178">Then, from the hub, you can track the state of each connection with the code shown below.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample8.cs)]

<a id="azure"></a>
### <a name="azure-table-storage"></a><span data-ttu-id="c2777-179">Azure Table Storage</span><span class="sxs-lookup"><span data-stu-id="c2777-179">Azure table storage</span></span>

<span data-ttu-id="c2777-180">Poniższy przykład usługi Azure Table Storage jest podobny do przykładu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="c2777-180">The following Azure table storage example is similar to the database example.</span></span> <span data-ttu-id="c2777-181">Nie obejmuje to wszystkich informacji, które należy rozpocząć od usługi Azure Table Storage.</span><span class="sxs-lookup"><span data-stu-id="c2777-181">It does not include all of the information that you would need to get started with Azure Table Storage Service.</span></span> <span data-ttu-id="c2777-182">Aby uzyskać więcej informacji, zobacz [jak używać magazynu tabel z platformy .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).</span><span class="sxs-lookup"><span data-stu-id="c2777-182">For information, see [How to use Table storage from .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).</span></span>

<span data-ttu-id="c2777-183">Poniższy przykład przedstawia jednostkę tabeli służącą do przechowywania informacji o połączeniu.</span><span class="sxs-lookup"><span data-stu-id="c2777-183">The following example shows a table entity for storing connection information.</span></span> <span data-ttu-id="c2777-184">Umożliwia ona Partycjonowanie danych według nazwy użytkownika i identyfikuje poszczególne jednostki według identyfikatora połączenia, dzięki czemu użytkownik może mieć wiele połączeń w dowolnym momencie.</span><span class="sxs-lookup"><span data-stu-id="c2777-184">It partitions the data by user name, and identifies each entity by the connection id, so a user can have multiple connections at any time.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample9.cs)]

<span data-ttu-id="c2777-185">W centrum jest śledzony stan połączenia poszczególnych użytkowników.</span><span class="sxs-lookup"><span data-stu-id="c2777-185">In the hub, you track the status of each user's connection.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample10.cs)]

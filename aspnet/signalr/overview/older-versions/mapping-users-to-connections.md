---
uid: signalr/overview/older-versions/mapping-users-to-connections
title: Mapowanie użytkowników sygnalizujących do połączeń w sygnale 1. x | Microsoft Docs
author: bradygaster
description: W tym temacie pokazano, jak zachować informacje o użytkownikach i ich połączeniach.
ms.author: bradyg
ms.date: 10/17/2013
ms.assetid: ebbc93a8-e6c4-4122-8e0d-3aa42293c747
msc.legacyurl: /signalr/overview/older-versions/mapping-users-to-connections
msc.type: authoredcontent
ms.openlocfilehash: 9d948495e9b8821fcb465611b6926603c3756a19
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78558486"
---
# <a name="mapping-signalr-users-to-connections-in-signalr-1x"></a><span data-ttu-id="04539-103">Mapowanie użytkowników usługi SignalR na połączenia w usłudze SignalR 1.x</span><span class="sxs-lookup"><span data-stu-id="04539-103">Mapping SignalR Users to Connections in SignalR 1.x</span></span>

<span data-ttu-id="04539-104">[Fletcher Patryk](https://github.com/pfletcher), [Tomasz FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="04539-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="04539-105">W tym temacie pokazano, jak zachować informacje o użytkownikach i ich połączeniach.</span><span class="sxs-lookup"><span data-stu-id="04539-105">This topic shows how to retain information about users and their connections.</span></span>

## <a name="introduction"></a><span data-ttu-id="04539-106">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="04539-106">Introduction</span></span>

<span data-ttu-id="04539-107">Każdy Klient nawiązujący połączenie z centrum przekazuje unikatowy identyfikator połączenia. Tę wartość można pobrać we właściwości `Context.ConnectionId` kontekstu centrum.</span><span class="sxs-lookup"><span data-stu-id="04539-107">Each client connecting to a hub passes a unique connection id. You can retrieve this value in the `Context.ConnectionId` property of the hub context.</span></span> <span data-ttu-id="04539-108">Jeśli aplikacja musi zmapować użytkownika na identyfikator połączenia i zachować to mapowanie, można użyć jednej z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="04539-108">If your application needs to map a user to the connection id and persist that mapping, you can use one of the following:</span></span>

- <span data-ttu-id="04539-109">[Magazyn w pamięci](#inmemory), taki jak słownik</span><span class="sxs-lookup"><span data-stu-id="04539-109">[In-memory storage](#inmemory), such as a dictionary</span></span>
- [<span data-ttu-id="04539-110">Grupa sygnałów dla każdego użytkownika</span><span class="sxs-lookup"><span data-stu-id="04539-110">SignalR group for each user</span></span>](#groups)
- <span data-ttu-id="04539-111">[Trwały, zewnętrzny magazyn](#database), taki jak tabela bazy danych lub usługa Azure Table Storage</span><span class="sxs-lookup"><span data-stu-id="04539-111">[Permanent, external storage](#database), such as a database table or Azure table storage</span></span>

<span data-ttu-id="04539-112">Każda z tych implementacji jest przedstawiona w tym temacie.</span><span class="sxs-lookup"><span data-stu-id="04539-112">Each of these implementations is shown in this topic.</span></span> <span data-ttu-id="04539-113">Możesz użyć metod `OnConnected`, `OnDisconnected`i `OnReconnected` klasy `Hub` do śledzenia stanu połączenia użytkownika.</span><span class="sxs-lookup"><span data-stu-id="04539-113">You use the `OnConnected`, `OnDisconnected`, and `OnReconnected` methods of the `Hub` class to track the user connection status.</span></span>

<span data-ttu-id="04539-114">Najlepsze podejście do aplikacji zależy od następujących:</span><span class="sxs-lookup"><span data-stu-id="04539-114">The best approach for your application depends on:</span></span>

- <span data-ttu-id="04539-115">Liczba serwerów sieci Web, które obsługują aplikację.</span><span class="sxs-lookup"><span data-stu-id="04539-115">The number of web servers hosting your application.</span></span>
- <span data-ttu-id="04539-116">Czy musisz uzyskać listę aktualnie połączonych użytkowników.</span><span class="sxs-lookup"><span data-stu-id="04539-116">Whether you need to get a list of the currently connected users.</span></span>
- <span data-ttu-id="04539-117">Czy po ponownym uruchomieniu aplikacji lub serwera należy zachować informacje o grupach i użytkownikach.</span><span class="sxs-lookup"><span data-stu-id="04539-117">Whether you need to persist group and user information when the application or server restarts.</span></span>
- <span data-ttu-id="04539-118">Czy opóźnienie wywołania serwera zewnętrznego jest problemem.</span><span class="sxs-lookup"><span data-stu-id="04539-118">Whether the latency of calling an external server is an issue.</span></span>

<span data-ttu-id="04539-119">W poniższej tabeli przedstawiono podejście, które ma zastosowanie do tych zagadnień.</span><span class="sxs-lookup"><span data-stu-id="04539-119">The following table shows which approach works for these considerations.</span></span>

|  | <span data-ttu-id="04539-120">Więcej niż jeden serwer</span><span class="sxs-lookup"><span data-stu-id="04539-120">More than one server</span></span> | <span data-ttu-id="04539-121">Pobierz listę aktualnie połączonych użytkowników</span><span class="sxs-lookup"><span data-stu-id="04539-121">Get list of currently connected users</span></span> | <span data-ttu-id="04539-122">Utrwalaj informacje po ponownym uruchomieniu</span><span class="sxs-lookup"><span data-stu-id="04539-122">Persist information after restarts</span></span> | <span data-ttu-id="04539-123">Optymalna wydajność</span><span class="sxs-lookup"><span data-stu-id="04539-123">Optimal performance</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="04539-124">W pamięci</span><span class="sxs-lookup"><span data-stu-id="04539-124">In-memory</span></span> |  | ![](mapping-users-to-connections/_static/image1.png) |  | ![](mapping-users-to-connections/_static/image2.png) |
| <span data-ttu-id="04539-125">Grupy pojedynczego użytkownika</span><span class="sxs-lookup"><span data-stu-id="04539-125">Single-user groups</span></span> | ![](mapping-users-to-connections/_static/image3.png) |  |  | ![](mapping-users-to-connections/_static/image4.png) |
| <span data-ttu-id="04539-126">Stałe, zewnętrzne</span><span class="sxs-lookup"><span data-stu-id="04539-126">Permanent, external</span></span> | ![](mapping-users-to-connections/_static/image5.png) | ![](mapping-users-to-connections/_static/image6.png) | ![](mapping-users-to-connections/_static/image7.png) |  |

<a id="inmemory"></a>

## <a name="in-memory-storage"></a><span data-ttu-id="04539-127">Magazyn w pamięci</span><span class="sxs-lookup"><span data-stu-id="04539-127">In-memory storage</span></span>

<span data-ttu-id="04539-128">W poniższych przykładach pokazano, jak zachować połączenie i informacje o użytkowniku w słowniku, który jest przechowywany w pamięci.</span><span class="sxs-lookup"><span data-stu-id="04539-128">The following examples show how to retain connection and user information in a dictionary that is stored in memory.</span></span> <span data-ttu-id="04539-129">Słownik używa `HashSet` do przechowywania identyfikatora połączenia. W dowolnym momencie użytkownik może mieć więcej niż jedno połączenie z aplikacją sygnalizującej.</span><span class="sxs-lookup"><span data-stu-id="04539-129">The dictionary uses a `HashSet` to store the connection id. At any time a user could have more than one connection to the SignalR application.</span></span> <span data-ttu-id="04539-130">Na przykład użytkownik połączony za pomocą wielu urządzeń lub więcej niż jedna karta przeglądarki ma więcej niż jeden identyfikator połączenia.</span><span class="sxs-lookup"><span data-stu-id="04539-130">For example, a user who is connected through multiple devices or more than one browser tab would have more than one connection id.</span></span>

<span data-ttu-id="04539-131">Jeśli aplikacja zostanie ZAMKNIĘTA, wszystkie informacje zostaną utracone, ale zostanie ponownie wypełnione, gdy użytkownicy ponownie ustalą swoje połączenia.</span><span class="sxs-lookup"><span data-stu-id="04539-131">If the application shuts down, all of the information is lost, but it will be re-populated as the users re-establish their connections.</span></span> <span data-ttu-id="04539-132">Magazyn w pamięci nie działa, jeśli środowisko zawiera więcej niż jeden serwer sieci Web, ponieważ każdy serwer będzie miał osobną kolekcję połączeń.</span><span class="sxs-lookup"><span data-stu-id="04539-132">In-memory storage does not work if your environment includes more than one web server because each server would have a separate collection of connections.</span></span>

<span data-ttu-id="04539-133">Pierwszy przykład przedstawia klasę, która zarządza mapowaniem użytkowników do połączeń.</span><span class="sxs-lookup"><span data-stu-id="04539-133">The first example shows a class that manages the mapping of users to connections.</span></span> <span data-ttu-id="04539-134">Kluczem dla HashSet — będzie nazwa użytkownika.</span><span class="sxs-lookup"><span data-stu-id="04539-134">The key for the HashSet will be the user's name.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

<span data-ttu-id="04539-135">W następnym przykładzie pokazano, jak używać klasy mapowania połączeń z poziomu centrum.</span><span class="sxs-lookup"><span data-stu-id="04539-135">The next example shows how to use the connection mapping class from a hub.</span></span> <span data-ttu-id="04539-136">Wystąpienie klasy jest przechowywane w nazwie zmiennej `_connections`.</span><span class="sxs-lookup"><span data-stu-id="04539-136">The instance of the class is stored in a variable name `_connections`.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a><span data-ttu-id="04539-137">Grupy pojedynczego użytkownika</span><span class="sxs-lookup"><span data-stu-id="04539-137">Single-user groups</span></span>

<span data-ttu-id="04539-138">Możesz utworzyć grupę dla każdego użytkownika, a następnie wysłać wiadomość do tej grupy, gdy chcesz uzyskać dostęp tylko do tego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="04539-138">You can create a group for each user, and then send a message to that group when you want to reach only that user.</span></span> <span data-ttu-id="04539-139">Nazwa każdej grupy to nazwa użytkownika.</span><span class="sxs-lookup"><span data-stu-id="04539-139">The name of each group is the name of the user.</span></span> <span data-ttu-id="04539-140">Jeśli użytkownik ma więcej niż jedno połączenie, każdy identyfikator połączenia zostanie dodany do grupy użytkownika.</span><span class="sxs-lookup"><span data-stu-id="04539-140">If a user has more than one connection, each connection id is added to the user's group.</span></span>

<span data-ttu-id="04539-141">Nie należy ręcznie usuwać użytkownika z grupy, gdy nastąpi odłączenie użytkownika.</span><span class="sxs-lookup"><span data-stu-id="04539-141">You should not manually remove the user from the group when the user disconnects.</span></span> <span data-ttu-id="04539-142">Ta akcja jest wykonywana automatycznie przez platformę sygnalizującą.</span><span class="sxs-lookup"><span data-stu-id="04539-142">This action is automatically performed by the SignalR framework.</span></span>

<span data-ttu-id="04539-143">Poniższy przykład pokazuje, jak zaimplementować grupy pojedynczego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="04539-143">The following example shows how to implement single-user groups.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a><span data-ttu-id="04539-144">Trwały magazyn zewnętrzny</span><span class="sxs-lookup"><span data-stu-id="04539-144">Permanent, external storage</span></span>

<span data-ttu-id="04539-145">W tym temacie przedstawiono sposób korzystania z bazy danych lub magazynu tabel platformy Azure do przechowywania informacji o połączeniu.</span><span class="sxs-lookup"><span data-stu-id="04539-145">This topic shows how to use either a database or Azure table storage for storing connection information.</span></span> <span data-ttu-id="04539-146">Takie podejście działa, gdy istnieje wiele serwerów sieci Web, ponieważ każdy serwer sieci Web może współdziałać z tym samym repozytorium danych.</span><span class="sxs-lookup"><span data-stu-id="04539-146">This approach works when you have multiple web servers because each web server can interact with the same data repository.</span></span> <span data-ttu-id="04539-147">Jeśli serwery sieci Web przestaną działać lub aplikacja zostanie uruchomiona ponownie, Metoda `OnDisconnected` nie zostanie wywołana.</span><span class="sxs-lookup"><span data-stu-id="04539-147">If your web servers stop working or the application restarts, the `OnDisconnected` method is not called.</span></span> <span data-ttu-id="04539-148">W związku z tym, istnieje możliwość, że repozytorium danych będzie zawierało rekordy identyfikatorów połączeń, które nie są już prawidłowe.</span><span class="sxs-lookup"><span data-stu-id="04539-148">Therefore, it is possible that your data repository will have records for connection ids that are no longer valid.</span></span> <span data-ttu-id="04539-149">Aby wyczyścić te oddzielone rekordy, możesz unieważnić wszelkie połączenia, które zostały utworzone poza przedziałem czasu, który jest istotny dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="04539-149">To clean up these orphaned records, you may wish to invalidate any connection that was created outside of a timeframe that is relevant to your application.</span></span> <span data-ttu-id="04539-150">Przykłady w tej sekcji zawierają wartość śledzenia podczas tworzenia połączenia, ale nie pokazują, jak oczyścić stare rekordy, ponieważ warto to zrobić jako proces w tle.</span><span class="sxs-lookup"><span data-stu-id="04539-150">The examples in this section include a value for tracking when the connection was created, but do not show how to clean up old records because you may want to do that as background process.</span></span>

### <a name="database"></a><span data-ttu-id="04539-151">Baza danych</span><span class="sxs-lookup"><span data-stu-id="04539-151">Database</span></span>

<span data-ttu-id="04539-152">W poniższych przykładach pokazano, jak zachować połączenie i informacje o użytkowniku w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="04539-152">The following examples show how to retain connection and user information in a database.</span></span> <span data-ttu-id="04539-153">Możesz użyć dowolnej technologii dostępu do danych. w poniższym przykładzie pokazano, jak definiować modele przy użyciu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="04539-153">You can use any data access technology; however, the example below shows how to define models using Entity Framework.</span></span> <span data-ttu-id="04539-154">Te modele jednostek odpowiadają tabelom i polom bazy danych.</span><span class="sxs-lookup"><span data-stu-id="04539-154">These entity models correspond to database tables and fields.</span></span> <span data-ttu-id="04539-155">Struktura danych może się znacznie różnić w zależności od wymagań aplikacji.</span><span class="sxs-lookup"><span data-stu-id="04539-155">Your data structure could vary considerably depending on the requirements of your application.</span></span>

<span data-ttu-id="04539-156">Pierwszy przykład pokazuje, jak zdefiniować jednostkę użytkownika, która może być skojarzona z wieloma jednostkami połączenia.</span><span class="sxs-lookup"><span data-stu-id="04539-156">The first example shows how to define a user entity that can be associated with many connection entities.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

<span data-ttu-id="04539-157">Następnie z poziomu centrum można śledzić stan każdego połączenia z kodem pokazanym poniżej.</span><span class="sxs-lookup"><span data-stu-id="04539-157">Then, from the hub, you can track the state of each connection with the code shown below.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

### <a name="azure-table-storage"></a><span data-ttu-id="04539-158">Azure Table Storage</span><span class="sxs-lookup"><span data-stu-id="04539-158">Azure table storage</span></span>

<span data-ttu-id="04539-159">Poniższy przykład usługi Azure Table Storage jest podobny do przykładu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="04539-159">The following Azure table storage example is similar to the database example.</span></span> <span data-ttu-id="04539-160">Nie obejmuje to wszystkich informacji, które należy rozpocząć od usługi Azure Table Storage.</span><span class="sxs-lookup"><span data-stu-id="04539-160">It does not include all of the information that you would need to get started with Azure Table Storage Service.</span></span> <span data-ttu-id="04539-161">Aby uzyskać więcej informacji, zobacz [jak używać magazynu tabel z platformy .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).</span><span class="sxs-lookup"><span data-stu-id="04539-161">For information, see [How to use Table storage from .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).</span></span>

<span data-ttu-id="04539-162">Poniższy przykład przedstawia jednostkę tabeli służącą do przechowywania informacji o połączeniu.</span><span class="sxs-lookup"><span data-stu-id="04539-162">The following example shows a table entity for storing connection information.</span></span> <span data-ttu-id="04539-163">Umożliwia ona Partycjonowanie danych według nazwy użytkownika i identyfikuje poszczególne jednostki według identyfikatora połączenia, dzięki czemu użytkownik może mieć wiele połączeń w dowolnym momencie.</span><span class="sxs-lookup"><span data-stu-id="04539-163">It partitions the data by user name, and identifies each entity by the connection id, so a user can have multiple connections at any time.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

<span data-ttu-id="04539-164">W centrum jest śledzony stan połączenia poszczególnych użytkowników.</span><span class="sxs-lookup"><span data-stu-id="04539-164">In the hub, you track the status of each user's connection.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]

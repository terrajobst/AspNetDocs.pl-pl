---
uid: signalr/overview/older-versions/working-with-groups
title: Praca z grupami w sygnalizacji 1. x | Microsoft Docs
author: bradygaster
description: W tym temacie opisano sposób utrwalania informacji o członkostwie w grupie za pomocą interfejsu API centrum.
ms.author: bradyg
ms.date: 10/21/2013
ms.assetid: 22929efd-68c9-4609-b76d-f8ba42fda01e
msc.legacyurl: /signalr/overview/older-versions/working-with-groups
msc.type: authoredcontent
ms.openlocfilehash: 5f50dc162d6cdcfbf2261e6a751f5f99078d5c54
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78579367"
---
# <a name="working-with-groups-in-signalr-1x"></a><span data-ttu-id="16f20-103">Praca z grupami w usłudze SignalR 1.x</span><span class="sxs-lookup"><span data-stu-id="16f20-103">Working with Groups in SignalR 1.x</span></span>

<span data-ttu-id="16f20-104">[Fletcher Patryk](https://github.com/pfletcher), [Tomasz FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="16f20-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="16f20-105">W tym temacie opisano sposób dodawania użytkowników do grup i utrwalania informacji o członkostwie w grupach.</span><span class="sxs-lookup"><span data-stu-id="16f20-105">This topic describes how to add users to groups and persist group membership information.</span></span>

## <a name="overview"></a><span data-ttu-id="16f20-106">Omówienie</span><span class="sxs-lookup"><span data-stu-id="16f20-106">Overview</span></span>

<span data-ttu-id="16f20-107">Grupy w sygnalizacji zapewniają metodę rozgłaszania komunikatów do określonych podzestawów połączonych klientów.</span><span class="sxs-lookup"><span data-stu-id="16f20-107">Groups in SignalR provide a method for broadcasting messages to specified subsets of connected clients.</span></span> <span data-ttu-id="16f20-108">Grupa może mieć dowolną liczbę klientów, a klient może być członkiem dowolnej liczby grup.</span><span class="sxs-lookup"><span data-stu-id="16f20-108">A group can have any number of clients, and a client can be a member of any number of groups.</span></span> <span data-ttu-id="16f20-109">Nie musisz jawnie tworzyć grup.</span><span class="sxs-lookup"><span data-stu-id="16f20-109">You don't have to explicitly create groups.</span></span> <span data-ttu-id="16f20-110">W efekcie Grupa jest tworzona automatycznie przy pierwszym określeniu jej nazwy w wywołaniu groups. Add i jest usuwana po usunięciu ostatniego połączenia z członkostwa w nim.</span><span class="sxs-lookup"><span data-stu-id="16f20-110">In effect, a group is automatically created the first time you specify its name in a call to Groups.Add, and it is deleted when you remove the last connection from membership in it.</span></span> <span data-ttu-id="16f20-111">Aby zapoznać się z wprowadzeniem do korzystania z grup, zobacz [jak zarządzać członkostwem w grupie z klasy Hub](index.md) w podręczniku API Hubs-Server.</span><span class="sxs-lookup"><span data-stu-id="16f20-111">For an introduction to using groups, see [How to manage group membership from the Hub class](index.md) in the Hubs API - Server Guide.</span></span>

<span data-ttu-id="16f20-112">Brak interfejsu API do uzyskiwania listy członkostwa w grupie lub listy grup.</span><span class="sxs-lookup"><span data-stu-id="16f20-112">There is no API for getting a group membership list or a list of groups.</span></span> <span data-ttu-id="16f20-113">Program sygnalizujący wysyła komunikaty do klientów i grup na podstawie modelu pub/sub, a serwer nie zachowuje list grup ani członkostw w grupach.</span><span class="sxs-lookup"><span data-stu-id="16f20-113">SignalR sends messages to clients and groups based on a pub/sub model, and the server does not maintain lists of groups or group memberships.</span></span> <span data-ttu-id="16f20-114">Pozwala to zmaksymalizować skalowalność, ponieważ po dodaniu węzła do kolektywu serwerów sieci Web, każdy stan, który utrzymuje usługa sygnalizująca, musi być propagowany do nowego węzła.</span><span class="sxs-lookup"><span data-stu-id="16f20-114">This helps maximize scalability, because whenever you add a node to a web farm, any state that SignalR maintains has to be propagated to the new node.</span></span>

<span data-ttu-id="16f20-115">Po dodaniu użytkownika do grupy przy użyciu metody `Groups.Add` użytkownik otrzymuje komunikaty skierowane do tej grupy na czas trwania bieżącego połączenia, ale członkostwo użytkownika w tej grupie nie jest utrwalane poza bieżącym połączeniem.</span><span class="sxs-lookup"><span data-stu-id="16f20-115">When you add a user to a group using the `Groups.Add` method, the user receives messages directed to that group for the duration of the current connection, but the user's membership in that group is not persisted beyond the current connection.</span></span> <span data-ttu-id="16f20-116">Jeśli chcesz trwale zachować informacje o grupach i członkostwie w grupie, musisz przechowywać te dane w repozytorium, takim jak baza danych lub Azure Table Storage.</span><span class="sxs-lookup"><span data-stu-id="16f20-116">If you want to permanently retain information about groups and group membership, you must store that data in a repository such as a database or Azure table storage.</span></span> <span data-ttu-id="16f20-117">Następnie za każdym razem, gdy użytkownik nawiązuje połączenie z aplikacją, pobiera z repozytorium, do którego należy Grupa, i ręcznie Dodaj tego użytkownika do tych grup.</span><span class="sxs-lookup"><span data-stu-id="16f20-117">Then, each time a user connects to your application, you retrieve from the repository which groups the user belongs to, and manually add that user to those groups.</span></span>

<span data-ttu-id="16f20-118">Po ponownym nawiązaniu połączenia po tymczasowym zakłóceniu użytkownik automatycznie ponownie przyłącza do wcześniej przypisanych grup.</span><span class="sxs-lookup"><span data-stu-id="16f20-118">When reconnecting after a temporary disruption, the user automatically re-joins the previously-assigned groups.</span></span> <span data-ttu-id="16f20-119">Automatyczne ponowne sprzęganie grupy ma zastosowanie tylko w przypadku ponownego połączenia, a nie podczas ustanawiania nowego połączenia.</span><span class="sxs-lookup"><span data-stu-id="16f20-119">Automatically rejoining a group only applies when reconnecting, not when establishing a new connection.</span></span> <span data-ttu-id="16f20-120">Token podpisany cyfrowo jest przesyłany z klienta, który zawiera listę wcześniej przypisanych grup.</span><span class="sxs-lookup"><span data-stu-id="16f20-120">A digitally-signed token is passed from the client that contains the list of previously-assigned groups.</span></span> <span data-ttu-id="16f20-121">Jeśli chcesz sprawdzić, czy użytkownik należy do żądanych grup, można zastąpić zachowanie domyślne.</span><span class="sxs-lookup"><span data-stu-id="16f20-121">If you want to verify whether the user belongs to the requested groups, you can override the default behavior.</span></span>

<span data-ttu-id="16f20-122">Ten temat zawiera następujące sekcje:</span><span class="sxs-lookup"><span data-stu-id="16f20-122">This topic includes the following sections:</span></span>

- [<span data-ttu-id="16f20-123">Dodawanie i usuwanie użytkowników</span><span class="sxs-lookup"><span data-stu-id="16f20-123">Adding and removing users</span></span>](#add)
- [<span data-ttu-id="16f20-124">Wywoływanie elementów członkowskich grupy</span><span class="sxs-lookup"><span data-stu-id="16f20-124">Calling members of a group</span></span>](#call)
- [<span data-ttu-id="16f20-125">Przechowywanie członkostwa w grupie w bazie danych</span><span class="sxs-lookup"><span data-stu-id="16f20-125">Storing group membership in a database</span></span>](#storedatabase)
- [<span data-ttu-id="16f20-126">Przechowywanie członkostwa w grupie w usłudze Azure Table Storage</span><span class="sxs-lookup"><span data-stu-id="16f20-126">Storing group membership in Azure table storage</span></span>](#storeazuretable)
- [<span data-ttu-id="16f20-127">Weryfikowanie członkostwa w grupie podczas ponownego nawiązywania połączenia</span><span class="sxs-lookup"><span data-stu-id="16f20-127">Verifying group membership when reconnecting</span></span>](#verify)

<a id="add"></a>

## <a name="adding-and-removing-users"></a><span data-ttu-id="16f20-128">Dodawanie i usuwanie użytkowników</span><span class="sxs-lookup"><span data-stu-id="16f20-128">Adding and removing users</span></span>

<span data-ttu-id="16f20-129">Aby dodać lub usunąć użytkowników z grupy, należy wywołać metody [dodawania](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) lub [usuwania](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) , a także przekazać identyfikator połączenia użytkownika i nazwę grupy jako parametry.</span><span class="sxs-lookup"><span data-stu-id="16f20-129">To add or remove users from a group, you call the [Add](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) or [Remove](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) methods, and pass in the user's connection id and group's name as parameters.</span></span> <span data-ttu-id="16f20-130">Po zakończeniu połączenia nie trzeba ręcznie usuwać użytkownika z grupy.</span><span class="sxs-lookup"><span data-stu-id="16f20-130">You do not need to manually remove a user from a group when the connection ends.</span></span>

<span data-ttu-id="16f20-131">W poniższym przykładzie przedstawiono metody `Groups.Add` i `Groups.Remove` używane w metodach centrum.</span><span class="sxs-lookup"><span data-stu-id="16f20-131">The following example shows the `Groups.Add` and `Groups.Remove` methods used in Hub methods.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample1.cs?highlight=5,10)]

<span data-ttu-id="16f20-132">Metody `Groups.Add` i `Groups.Remove` wykonują asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="16f20-132">The `Groups.Add` and `Groups.Remove` methods execute asynchronously.</span></span>

<span data-ttu-id="16f20-133">Aby dodać klienta do grupy i natychmiast wysłać komunikat do klienta przy użyciu grupy, należy się upewnić, że metoda Groups. Add została najpierw ukończona.</span><span class="sxs-lookup"><span data-stu-id="16f20-133">If you want to add a client to a group and immediately send a message to the client by using the group, you have to make sure that the Groups.Add method finishes first.</span></span> <span data-ttu-id="16f20-134">Poniższe przykłady kodu pokazują, jak to zrobić, przy użyciu kodu, który działa w programie .NET 4,5 i jeden przy użyciu kodu, który działa w programie .NET 4.</span><span class="sxs-lookup"><span data-stu-id="16f20-134">The following code examples show how to do that, one by using code that works in .NET 4.5, and one by using code that works in .NET 4.</span></span>

#### <a name="asynchronous-net-45-example"></a><span data-ttu-id="16f20-135">Przykład asynchronicznej platformy .NET 4,5</span><span class="sxs-lookup"><span data-stu-id="16f20-135">Asynchronous .NET 4.5 Example</span></span>

[!code-csharp[Main](working-with-groups/samples/sample2.cs?highlight=1,3)]

#### <a name="asynchronous-net-4-example"></a><span data-ttu-id="16f20-136">Przykład asynchronicznej platformy .NET 4</span><span class="sxs-lookup"><span data-stu-id="16f20-136">Asynchronous .NET 4 Example</span></span>

[!code-csharp[Main](working-with-groups/samples/sample3.cs?highlight=3-4)]

<span data-ttu-id="16f20-137">Ogólnie rzecz biorąc nie należy uwzględniać `await` podczas wywoływania metody `Groups.Remove`, ponieważ identyfikator połączenia, który próbujesz usunąć, nie jest już dostępny.</span><span class="sxs-lookup"><span data-stu-id="16f20-137">In general, you should not include `await` when calling the `Groups.Remove` method because the connection id that you are trying to remove might no longer be available.</span></span> <span data-ttu-id="16f20-138">W takim przypadku `TaskCanceledException` jest generowany po upływie limitu czasu żądania. Jeśli aplikacja musi upewnić się, że użytkownik został usunięty z grupy przed wysłaniem komunikatu do grupy, można dodać `await` przed grupą. Usuń, a następnie wychwycić wyjątek `TaskCanceledException`, który może zostać wygenerowany.</span><span class="sxs-lookup"><span data-stu-id="16f20-138">In that case, `TaskCanceledException` is thrown after the request times out. If your application must ensure that the user has been removed from the group before sending a message to the group, you can add `await` before Groups.Remove, and then catch the `TaskCanceledException` exception that might be thrown.</span></span>

<a id="call"></a>

## <a name="calling-members-of-a-group"></a><span data-ttu-id="16f20-139">Wywoływanie elementów członkowskich grupy</span><span class="sxs-lookup"><span data-stu-id="16f20-139">Calling members of a group</span></span>

<span data-ttu-id="16f20-140">Można wysyłać wiadomości do wszystkich członków grupy lub tylko do określonych członków grupy, jak pokazano w poniższych przykładach.</span><span class="sxs-lookup"><span data-stu-id="16f20-140">You can send messages to all of the members of a group or only specified members of the group, as shown in the following examples.</span></span>

- <span data-ttu-id="16f20-141">**Wszyscy** połączeni klienci w określonej grupie.</span><span class="sxs-lookup"><span data-stu-id="16f20-141">**All** connected clients in a specified group.</span></span> 

    [!code-css[Main](working-with-groups/samples/sample4.css)]
- <span data-ttu-id="16f20-142">Wszyscy połączeni klienci w określonej grupie **z wyjątkiem określonych klientów**identyfikowane przez identyfikator połączenia.</span><span class="sxs-lookup"><span data-stu-id="16f20-142">All connected clients in a specified group **except the specified clients**, identified by connection ID.</span></span> 

    [!code-csharp[Main](working-with-groups/samples/sample5.cs)]
- <span data-ttu-id="16f20-143">Wszyscy połączeni klienci w określonej grupie **z wyjątkiem klienta wywołującego**.</span><span class="sxs-lookup"><span data-stu-id="16f20-143">All connected clients in a specified group **except the calling client**.</span></span> 

    [!code-css[Main](working-with-groups/samples/sample6.css)]

<a id="storedatabase"></a>

## <a name="storing-group-membership-in-a-database"></a><span data-ttu-id="16f20-144">Przechowywanie członkostwa w grupie w bazie danych</span><span class="sxs-lookup"><span data-stu-id="16f20-144">Storing group membership in a database</span></span>

<span data-ttu-id="16f20-145">W poniższych przykładach pokazano, jak zachować informacje o grupach i użytkownikach w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="16f20-145">The following examples show how to retain group and user information in a database.</span></span> <span data-ttu-id="16f20-146">Możesz użyć dowolnej technologii dostępu do danych. w poniższym przykładzie pokazano, jak definiować modele przy użyciu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="16f20-146">You can use any data access technology; however, the example below shows how to define models using Entity Framework.</span></span> <span data-ttu-id="16f20-147">Te modele jednostek odpowiadają tabelom i polom bazy danych.</span><span class="sxs-lookup"><span data-stu-id="16f20-147">These entity models correspond to database tables and fields.</span></span> <span data-ttu-id="16f20-148">Struktura danych może się znacznie różnić w zależności od wymagań aplikacji.</span><span class="sxs-lookup"><span data-stu-id="16f20-148">Your data structure could vary considerably depending on the requirements of your application.</span></span> <span data-ttu-id="16f20-149">Ten przykład zawiera klasę o nazwie `ConversationRoom`, która może być unikatowa dla aplikacji, która umożliwia użytkownikom dołączanie do konwersacji dotyczących różnych tematów, takich jak sport lub ogród.</span><span class="sxs-lookup"><span data-stu-id="16f20-149">This example includes a class named `ConversationRoom` which would be unique to an application that enables users to join conversations about different subjects, such as sports or gardening.</span></span> <span data-ttu-id="16f20-150">Ten przykład zawiera również klasę dla połączeń.</span><span class="sxs-lookup"><span data-stu-id="16f20-150">This example also includes a class for the connections.</span></span> <span data-ttu-id="16f20-151">Klasa połączenia nie jest absolutnie wymagana do śledzenia członkostwa w grupach, ale jest często częścią niezawodnego rozwiązania do śledzenia użytkowników.</span><span class="sxs-lookup"><span data-stu-id="16f20-151">The connection class is not absolutely required for tracking group membership but is frequently part of robust solution to tracking users.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample7.cs)]

<span data-ttu-id="16f20-152">Następnie w centrum można pobrać informacje o grupach i użytkownikach z bazy danych i ręcznie dodać użytkownika do odpowiednich grup.</span><span class="sxs-lookup"><span data-stu-id="16f20-152">Then, in the hub, you can retrieve the group and user information from the database and manually add the user to the appropriate groups.</span></span> <span data-ttu-id="16f20-153">Przykład nie zawiera kodu do śledzenia połączeń użytkownika.</span><span class="sxs-lookup"><span data-stu-id="16f20-153">The example does not include code for tracking the user connections.</span></span> <span data-ttu-id="16f20-154">W tym przykładzie słowo kluczowe `await` nie jest stosowane przed `Groups.Add`, ponieważ komunikat nie jest natychmiast wysyłany do członków grupy.</span><span class="sxs-lookup"><span data-stu-id="16f20-154">In this example, the `await` keyword is not applied before `Groups.Add` because a message is not immediately sent to members of the group.</span></span> <span data-ttu-id="16f20-155">Jeśli chcesz wysłać wiadomość do wszystkich członków grupy natychmiast po dodaniu nowego elementu członkowskiego, należy zastosować słowo kluczowe `await`, aby upewnić się, że operacja asynchroniczna została ukończona.</span><span class="sxs-lookup"><span data-stu-id="16f20-155">If you want to send a message to all members of the group immediately after adding the new member, you would want to apply the `await` keyword to make sure the asynchronous operation has completed.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample8.cs)]

<a id="storeazuretable"></a>

## <a name="storing-group-membership-in-azure-table-storage"></a><span data-ttu-id="16f20-156">Przechowywanie członkostwa w grupie w usłudze Azure Table Storage</span><span class="sxs-lookup"><span data-stu-id="16f20-156">Storing group membership in Azure table storage</span></span>

<span data-ttu-id="16f20-157">Korzystanie z usługi Azure Table Storage do przechowywania informacji o grupach i użytkownikach jest podobne do korzystania z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="16f20-157">Using Azure table storage to store group and user information is similar to using a database.</span></span> <span data-ttu-id="16f20-158">Poniższy przykład pokazuje jednostkę tabeli przechowującą nazwę użytkownika i nazwę grupy.</span><span class="sxs-lookup"><span data-stu-id="16f20-158">The following example shows a table entity that stores the user name and group name.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample9.cs)]

<span data-ttu-id="16f20-159">W centrum należy pobrać przypisane grupy, gdy użytkownik nawiązuje połączenie.</span><span class="sxs-lookup"><span data-stu-id="16f20-159">In the hub, you retrieve the assigned groups when the user connects.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample10.cs)]

<a id="verify"></a>

## <a name="verifying-group-membership-when-reconnecting"></a><span data-ttu-id="16f20-160">Weryfikowanie członkostwa w grupie podczas ponownego nawiązywania połączenia</span><span class="sxs-lookup"><span data-stu-id="16f20-160">Verifying group membership when reconnecting</span></span>

<span data-ttu-id="16f20-161">Domyślnie program sygnalizujący automatycznie ponownie przypisuje użytkownika do odpowiednich grup podczas ponownego nawiązywania połączenia po tymczasowym przerwaniu, na przykład w przypadku porzucenia i ponownego ustanowienia połączenia przed upływem limitu czasu. Informacje o grupie użytkownika są przesyłane w tokenie podczas ponownego nawiązywania połączenia, a ten token jest weryfikowany na serwerze.</span><span class="sxs-lookup"><span data-stu-id="16f20-161">By default, SignalR automatically re-assigns a user to the appropriate groups when reconnecting from a temporary disruption, such as when a connection is dropped and re-established before the connection times out. The user's group information is passed in a token when reconnecting, and that token is verified on the server.</span></span> <span data-ttu-id="16f20-162">Aby uzyskać informacje na temat procesu weryfikacji dla ponownych dołączania użytkowników do grup, zobacz [ponowne łączenie grup po ponownym połączeniu](index.md).</span><span class="sxs-lookup"><span data-stu-id="16f20-162">For information about the verification process for rejoining users to groups, see [Rejoining groups when reconnecting](index.md).</span></span>

<span data-ttu-id="16f20-163">Ogólnie rzecz biorąc, należy użyć domyślnego zachowania automatycznego ponownego przyłączania grup przy ponownym połączeniu.</span><span class="sxs-lookup"><span data-stu-id="16f20-163">In general, you should use the default behavior of automatically rejoining groups on reconnect.</span></span> <span data-ttu-id="16f20-164">Grupy sygnałów nie stanowią mechanizmu zabezpieczeń w celu ograniczenia dostępu do poufnych danych.</span><span class="sxs-lookup"><span data-stu-id="16f20-164">SignalR groups are not intended as a security mechanism for restricting access to sensitive data.</span></span> <span data-ttu-id="16f20-165">Jeśli jednak aplikacja musi mieć podwójne sprawdzenie członkostwa w grupie użytkownika podczas ponownego nawiązywania połączenia, można zastąpić zachowanie domyślne.</span><span class="sxs-lookup"><span data-stu-id="16f20-165">However, if your application must double-check a user's group membership when reconnecting, you can override the default behavior.</span></span> <span data-ttu-id="16f20-166">Zmiana domyślnego zachowania może zwiększyć obciążenie bazy danych, ponieważ członkostwo w grupie użytkownika musi być pobrane dla każdego ponownego połączenia, a nie tylko wtedy, gdy użytkownik nawiązuje połączenie.</span><span class="sxs-lookup"><span data-stu-id="16f20-166">Changing the default behavior can add a burden to your database because a user's group membership must be retrieved for each reconnection rather than just when the user connects.</span></span>

<span data-ttu-id="16f20-167">Jeśli konieczne jest zweryfikowanie członkostwa w grupie przy ponownym nawiązaniu połączenia, Utwórz nowy moduł potoku centrum, który zwraca listę przypisanych grup, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="16f20-167">If you must verify group membership on reconnect, create a new hub pipeline module that returns a list of assigned groups, as shown below.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample11.cs)]

<span data-ttu-id="16f20-168">Następnie Dodaj ten moduł do potoku centrum, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="16f20-168">Then, add that module to the hub pipeline, as highlighted below.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample12.cs?highlight=10)]

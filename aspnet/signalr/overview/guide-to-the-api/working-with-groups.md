---
uid: signalr/overview/guide-to-the-api/working-with-groups
title: Praca z grupami w sygnalizacji | Microsoft Docs
author: bradygaster
description: W tym temacie opisano sposób utrwalania informacji o członkostwie w grupie za pomocą interfejsu API centrum.
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: cd378ecd-3e9e-4236-b902-65916d85a048
msc.legacyurl: /signalr/overview/guide-to-the-api/working-with-groups
msc.type: authoredcontent
ms.openlocfilehash: 46dd952fc4902b37c981a111dfa344dad79bb668
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78558584"
---
# <a name="working-with-groups-in-signalr"></a><span data-ttu-id="614a1-103">Praca z grupami w usłudze SignalR</span><span class="sxs-lookup"><span data-stu-id="614a1-103">Working with Groups in SignalR</span></span>

<span data-ttu-id="614a1-104">[Fletcher Patryk](https://github.com/pfletcher), [Tomasz FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="614a1-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="614a1-105">W tym temacie opisano sposób dodawania użytkowników do grup i utrwalania informacji o członkostwie w grupach.</span><span class="sxs-lookup"><span data-stu-id="614a1-105">This topic describes how to add users to groups and persist group membership information.</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="614a1-106">Wersje oprogramowania używane w tym temacie</span><span class="sxs-lookup"><span data-stu-id="614a1-106">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="614a1-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="614a1-107">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="614a1-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="614a1-108">.NET 4.5</span></span>
> - <span data-ttu-id="614a1-109">Sygnalizujący wersja 2</span><span class="sxs-lookup"><span data-stu-id="614a1-109">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="614a1-110">Poprzednie wersje tego tematu</span><span class="sxs-lookup"><span data-stu-id="614a1-110">Previous versions of this topic</span></span>
>
> <span data-ttu-id="614a1-111">Aby uzyskać informacje o wcześniejszych wersjach programu sygnalizującego, zobacz sekcję [sygnalizujące starsze wersje](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="614a1-111">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="614a1-112">Pytania i Komentarze</span><span class="sxs-lookup"><span data-stu-id="614a1-112">Questions and comments</span></span>
>
> <span data-ttu-id="614a1-113">Prosimy o opinię na temat sposobu, w jaki lubię ten samouczek, i co możemy ulepszyć w komentarzach w dolnej części strony.</span><span class="sxs-lookup"><span data-stu-id="614a1-113">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="614a1-114">Jeśli masz pytania, które nie są bezpośrednio związane z samouczkiem, możesz je ogłosić na [forum ASP.NET](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="614a1-114">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

## <a name="overview"></a><span data-ttu-id="614a1-115">Omówienie</span><span class="sxs-lookup"><span data-stu-id="614a1-115">Overview</span></span>

<span data-ttu-id="614a1-116">Grupy w sygnalizacji zapewniają metodę rozgłaszania komunikatów do określonych podzestawów połączonych klientów.</span><span class="sxs-lookup"><span data-stu-id="614a1-116">Groups in SignalR provide a method for broadcasting messages to specified subsets of connected clients.</span></span> <span data-ttu-id="614a1-117">Grupa może mieć dowolną liczbę klientów, a klient może być członkiem dowolnej liczby grup.</span><span class="sxs-lookup"><span data-stu-id="614a1-117">A group can have any number of clients, and a client can be a member of any number of groups.</span></span> <span data-ttu-id="614a1-118">Nie musisz jawnie tworzyć grup.</span><span class="sxs-lookup"><span data-stu-id="614a1-118">You don't have to explicitly create groups.</span></span> <span data-ttu-id="614a1-119">W efekcie Grupa jest tworzona automatycznie przy pierwszym określeniu jej nazwy w wywołaniu groups. Add i jest usuwana po usunięciu ostatniego połączenia z członkostwa w nim.</span><span class="sxs-lookup"><span data-stu-id="614a1-119">In effect, a group is automatically created the first time you specify its name in a call to Groups.Add, and it is deleted when you remove the last connection from membership in it.</span></span> <span data-ttu-id="614a1-120">Aby zapoznać się z wprowadzeniem do korzystania z grup, zobacz [jak zarządzać członkostwem w grupie z klasy Hub](hubs-api-guide-server.md#groupsfromhub) w podręczniku API Hubs-Server.</span><span class="sxs-lookup"><span data-stu-id="614a1-120">For an introduction to using groups, see [How to manage group membership from the Hub class](hubs-api-guide-server.md#groupsfromhub) in the Hubs API - Server Guide.</span></span>

<span data-ttu-id="614a1-121">Brak interfejsu API do uzyskiwania listy członkostwa w grupie lub listy grup.</span><span class="sxs-lookup"><span data-stu-id="614a1-121">There is no API for getting a group membership list or a list of groups.</span></span> <span data-ttu-id="614a1-122">Program sygnalizujący wysyła komunikaty do klientów i grup na podstawie modelu pub/sub, a serwer nie zachowuje list grup ani członkostw w grupach.</span><span class="sxs-lookup"><span data-stu-id="614a1-122">SignalR sends messages to clients and groups based on a pub/sub model, and the server does not maintain lists of groups or group memberships.</span></span> <span data-ttu-id="614a1-123">Pozwala to zmaksymalizować skalowalność, ponieważ po dodaniu węzła do kolektywu serwerów sieci Web, każdy stan, który utrzymuje usługa sygnalizująca, musi być propagowany do nowego węzła.</span><span class="sxs-lookup"><span data-stu-id="614a1-123">This helps maximize scalability, because whenever you add a node to a web farm, any state that SignalR maintains has to be propagated to the new node.</span></span>

<span data-ttu-id="614a1-124">Po dodaniu użytkownika do grupy przy użyciu metody `Groups.Add` użytkownik otrzymuje komunikaty skierowane do tej grupy na czas trwania bieżącego połączenia, ale członkostwo użytkownika w tej grupie nie jest utrwalane poza bieżącym połączeniem.</span><span class="sxs-lookup"><span data-stu-id="614a1-124">When you add a user to a group using the `Groups.Add` method, the user receives messages directed to that group for the duration of the current connection, but the user's membership in that group is not persisted beyond the current connection.</span></span> <span data-ttu-id="614a1-125">Jeśli chcesz trwale zachować informacje o grupach i członkostwie w grupie, musisz przechowywać te dane w repozytorium, takim jak baza danych lub Azure Table Storage.</span><span class="sxs-lookup"><span data-stu-id="614a1-125">If you want to permanently retain information about groups and group membership, you must store that data in a repository such as a database or Azure table storage.</span></span> <span data-ttu-id="614a1-126">Następnie za każdym razem, gdy użytkownik nawiązuje połączenie z aplikacją, pobiera z repozytorium, do którego należy Grupa, i ręcznie Dodaj tego użytkownika do tych grup.</span><span class="sxs-lookup"><span data-stu-id="614a1-126">Then, each time a user connects to your application, you retrieve from the repository which groups the user belongs to, and manually add that user to those groups.</span></span>

<span data-ttu-id="614a1-127">Po ponownym nawiązaniu połączenia po tymczasowym zakłóceniu użytkownik automatycznie ponownie przyłącza do wcześniej przypisanych grup.</span><span class="sxs-lookup"><span data-stu-id="614a1-127">When reconnecting after a temporary disruption, the user automatically re-joins the previously-assigned groups.</span></span> <span data-ttu-id="614a1-128">Automatyczne ponowne sprzęganie grupy ma zastosowanie tylko w przypadku ponownego połączenia, a nie podczas ustanawiania nowego połączenia.</span><span class="sxs-lookup"><span data-stu-id="614a1-128">Automatically rejoining a group only applies when reconnecting, not when establishing a new connection.</span></span> <span data-ttu-id="614a1-129">Token podpisany cyfrowo jest przesyłany z klienta, który zawiera listę wcześniej przypisanych grup.</span><span class="sxs-lookup"><span data-stu-id="614a1-129">A digitally-signed token is passed from the client that contains the list of previously-assigned groups.</span></span> <span data-ttu-id="614a1-130">Jeśli chcesz sprawdzić, czy użytkownik należy do żądanych grup, można zastąpić zachowanie domyślne.</span><span class="sxs-lookup"><span data-stu-id="614a1-130">If you want to verify whether the user belongs to the requested groups, you can override the default behavior.</span></span>

<span data-ttu-id="614a1-131">Ten temat zawiera następujące sekcje:</span><span class="sxs-lookup"><span data-stu-id="614a1-131">This topic includes the following sections:</span></span>

- [<span data-ttu-id="614a1-132">Dodawanie i usuwanie użytkowników</span><span class="sxs-lookup"><span data-stu-id="614a1-132">Adding and removing users</span></span>](#add)
- [<span data-ttu-id="614a1-133">Wywoływanie elementów członkowskich grupy</span><span class="sxs-lookup"><span data-stu-id="614a1-133">Calling members of a group</span></span>](#call)
- [<span data-ttu-id="614a1-134">Przechowywanie członkostwa w grupie w bazie danych</span><span class="sxs-lookup"><span data-stu-id="614a1-134">Storing group membership in a database</span></span>](#storedatabase)
- [<span data-ttu-id="614a1-135">Przechowywanie członkostwa w grupie w usłudze Azure Table Storage</span><span class="sxs-lookup"><span data-stu-id="614a1-135">Storing group membership in Azure table storage</span></span>](#storeazuretable)
- [<span data-ttu-id="614a1-136">Weryfikowanie członkostwa w grupie podczas ponownego nawiązywania połączenia</span><span class="sxs-lookup"><span data-stu-id="614a1-136">Verifying group membership when reconnecting</span></span>](#verify)

<a id="add"></a>

## <a name="adding-and-removing-users"></a><span data-ttu-id="614a1-137">Dodawanie i usuwanie użytkowników</span><span class="sxs-lookup"><span data-stu-id="614a1-137">Adding and removing users</span></span>

<span data-ttu-id="614a1-138">Aby dodać lub usunąć użytkowników z grupy, należy wywołać metody [dodawania](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) lub [usuwania](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) , a także przekazać identyfikator połączenia użytkownika i nazwę grupy jako parametry.</span><span class="sxs-lookup"><span data-stu-id="614a1-138">To add or remove users from a group, you call the [Add](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) or [Remove](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) methods, and pass in the user's connection id and group's name as parameters.</span></span> <span data-ttu-id="614a1-139">Po zakończeniu połączenia nie trzeba ręcznie usuwać użytkownika z grupy.</span><span class="sxs-lookup"><span data-stu-id="614a1-139">You do not need to manually remove a user from a group when the connection ends.</span></span>

<span data-ttu-id="614a1-140">W poniższym przykładzie przedstawiono metody `Groups.Add` i `Groups.Remove` używane w metodach centrum.</span><span class="sxs-lookup"><span data-stu-id="614a1-140">The following example shows the `Groups.Add` and `Groups.Remove` methods used in Hub methods.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample1.cs?highlight=5,10)]

<span data-ttu-id="614a1-141">Metody `Groups.Add` i `Groups.Remove` wykonują asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="614a1-141">The `Groups.Add` and `Groups.Remove` methods execute asynchronously.</span></span>

<span data-ttu-id="614a1-142">Aby dodać klienta do grupy i natychmiast wysłać komunikat do klienta przy użyciu grupy, należy się upewnić, że metoda Groups. Add została najpierw ukończona.</span><span class="sxs-lookup"><span data-stu-id="614a1-142">If you want to add a client to a group and immediately send a message to the client by using the group, you have to make sure that the Groups.Add method finishes first.</span></span> <span data-ttu-id="614a1-143">Poniższe przykłady kodu pokazują, jak to zrobić.</span><span class="sxs-lookup"><span data-stu-id="614a1-143">The following code examples show how to do that.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample2.cs?highlight=1,3)]

<span data-ttu-id="614a1-144">Ogólnie rzecz biorąc nie należy uwzględniać `await` podczas wywoływania metody `Groups.Remove`, ponieważ identyfikator połączenia, który próbujesz usunąć, nie jest już dostępny.</span><span class="sxs-lookup"><span data-stu-id="614a1-144">In general, you should not include `await` when calling the `Groups.Remove` method because the connection id that you are trying to remove might no longer be available.</span></span> <span data-ttu-id="614a1-145">W takim przypadku `TaskCanceledException` jest generowany po upływie limitu czasu żądania. Jeśli aplikacja musi upewnić się, że użytkownik został usunięty z grupy przed wysłaniem komunikatu do grupy, można dodać `await` przed `Groups.Remove`, a następnie przechwytywać `TaskCanceledException` wyjątek, który może zostać wygenerowany.</span><span class="sxs-lookup"><span data-stu-id="614a1-145">In that case, `TaskCanceledException` is thrown after the request times out. If your application must ensure that the user has been removed from the group before sending a message to the group, you can add `await` before `Groups.Remove`, and then catch the `TaskCanceledException` exception that might be thrown.</span></span>

<a id="call"></a>

## <a name="calling-members-of-a-group"></a><span data-ttu-id="614a1-146">Wywoływanie elementów członkowskich grupy</span><span class="sxs-lookup"><span data-stu-id="614a1-146">Calling members of a group</span></span>

<span data-ttu-id="614a1-147">Można wysyłać wiadomości do wszystkich członków grupy lub tylko do określonych członków grupy, jak pokazano w poniższych przykładach.</span><span class="sxs-lookup"><span data-stu-id="614a1-147">You can send messages to all of the members of a group or only specified members of the group, as shown in the following examples.</span></span>

- <span data-ttu-id="614a1-148">**Wszyscy** połączeni klienci w określonej grupie.</span><span class="sxs-lookup"><span data-stu-id="614a1-148">**All** connected clients in a specified group.</span></span>

    [!code-css[Main](working-with-groups/samples/sample3.css)]
- <span data-ttu-id="614a1-149">Wszyscy połączeni klienci w określonej grupie **z wyjątkiem określonych klientów**identyfikowane przez identyfikator połączenia.</span><span class="sxs-lookup"><span data-stu-id="614a1-149">All connected clients in a specified group **except the specified clients**, identified by connection ID.</span></span>

    [!code-csharp[Main](working-with-groups/samples/sample4.cs)]
- <span data-ttu-id="614a1-150">Wszyscy połączeni klienci w określonej grupie **z wyjątkiem klienta wywołującego**.</span><span class="sxs-lookup"><span data-stu-id="614a1-150">All connected clients in a specified group **except the calling client**.</span></span>

    [!code-css[Main](working-with-groups/samples/sample5.css)]

<a id="storedatabase"></a>

## <a name="storing-group-membership-in-a-database"></a><span data-ttu-id="614a1-151">Przechowywanie członkostwa w grupie w bazie danych</span><span class="sxs-lookup"><span data-stu-id="614a1-151">Storing group membership in a database</span></span>

<span data-ttu-id="614a1-152">W poniższych przykładach pokazano, jak zachować informacje o grupach i użytkownikach w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="614a1-152">The following examples show how to retain group and user information in a database.</span></span> <span data-ttu-id="614a1-153">Możesz użyć dowolnej technologii dostępu do danych. w poniższym przykładzie pokazano, jak definiować modele przy użyciu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="614a1-153">You can use any data access technology; however, the example below shows how to define models using Entity Framework.</span></span> <span data-ttu-id="614a1-154">Te modele jednostek odpowiadają tabelom i polom bazy danych.</span><span class="sxs-lookup"><span data-stu-id="614a1-154">These entity models correspond to database tables and fields.</span></span> <span data-ttu-id="614a1-155">Struktura danych może się znacznie różnić w zależności od wymagań aplikacji.</span><span class="sxs-lookup"><span data-stu-id="614a1-155">Your data structure could vary considerably depending on the requirements of your application.</span></span> <span data-ttu-id="614a1-156">Ten przykład zawiera klasę o nazwie `ConversationRoom`, która może być unikatowa dla aplikacji, która umożliwia użytkownikom dołączanie do konwersacji dotyczących różnych tematów, takich jak sport lub ogród.</span><span class="sxs-lookup"><span data-stu-id="614a1-156">This example includes a class named `ConversationRoom` which would be unique to an application that enables users to join conversations about different subjects, such as sports or gardening.</span></span> <span data-ttu-id="614a1-157">Ten przykład zawiera również klasę dla połączeń.</span><span class="sxs-lookup"><span data-stu-id="614a1-157">This example also includes a class for the connections.</span></span> <span data-ttu-id="614a1-158">Klasa połączenia nie jest absolutnie wymagana do śledzenia członkostwa w grupach, ale jest często częścią niezawodnego rozwiązania do śledzenia użytkowników.</span><span class="sxs-lookup"><span data-stu-id="614a1-158">The connection class is not absolutely required for tracking group membership but is frequently part of robust solution to tracking users.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample6.cs)]

<span data-ttu-id="614a1-159">Następnie w centrum można pobrać informacje o grupach i użytkownikach z bazy danych i ręcznie dodać użytkownika do odpowiednich grup.</span><span class="sxs-lookup"><span data-stu-id="614a1-159">Then, in the hub, you can retrieve the group and user information from the database and manually add the user to the appropriate groups.</span></span> <span data-ttu-id="614a1-160">Przykład nie zawiera kodu do śledzenia połączeń użytkownika.</span><span class="sxs-lookup"><span data-stu-id="614a1-160">The example does not include code for tracking the user connections.</span></span> <span data-ttu-id="614a1-161">W tym przykładzie słowo kluczowe `await` nie jest stosowane przed `Groups.Add`, ponieważ komunikat nie jest natychmiast wysyłany do członków grupy.</span><span class="sxs-lookup"><span data-stu-id="614a1-161">In this example, the `await` keyword is not applied before `Groups.Add` because a message is not immediately sent to members of the group.</span></span> <span data-ttu-id="614a1-162">Jeśli chcesz wysłać wiadomość do wszystkich członków grupy natychmiast po dodaniu nowego elementu członkowskiego, należy zastosować słowo kluczowe `await`, aby upewnić się, że operacja asynchroniczna została ukończona.</span><span class="sxs-lookup"><span data-stu-id="614a1-162">If you want to send a message to all members of the group immediately after adding the new member, you would want to apply the `await` keyword to make sure the asynchronous operation has completed.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample7.cs)]

<a id="storeazuretable"></a>

## <a name="storing-group-membership-in-azure-table-storage"></a><span data-ttu-id="614a1-163">Przechowywanie członkostwa w grupie w usłudze Azure Table Storage</span><span class="sxs-lookup"><span data-stu-id="614a1-163">Storing group membership in Azure table storage</span></span>

<span data-ttu-id="614a1-164">Korzystanie z usługi Azure Table Storage do przechowywania informacji o grupach i użytkownikach jest podobne do korzystania z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="614a1-164">Using Azure table storage to store group and user information is similar to using a database.</span></span> <span data-ttu-id="614a1-165">Poniższy przykład pokazuje jednostkę tabeli przechowującą nazwę użytkownika i nazwę grupy.</span><span class="sxs-lookup"><span data-stu-id="614a1-165">The following example shows a table entity that stores the user name and group name.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample8.cs)]

<span data-ttu-id="614a1-166">W centrum należy pobrać przypisane grupy, gdy użytkownik nawiązuje połączenie.</span><span class="sxs-lookup"><span data-stu-id="614a1-166">In the hub, you retrieve the assigned groups when the user connects.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample9.cs)]

<a id="verify"></a>

## <a name="verifying-group-membership-when-reconnecting"></a><span data-ttu-id="614a1-167">Weryfikowanie członkostwa w grupie podczas ponownego nawiązywania połączenia</span><span class="sxs-lookup"><span data-stu-id="614a1-167">Verifying group membership when reconnecting</span></span>

<span data-ttu-id="614a1-168">Domyślnie program sygnalizujący automatycznie ponownie przypisuje użytkownika do odpowiednich grup podczas ponownego nawiązywania połączenia po tymczasowym przerwaniu, na przykład w przypadku porzucenia i ponownego ustanowienia połączenia przed upływem limitu czasu. Informacje o grupie użytkownika są przesyłane w tokenie podczas ponownego nawiązywania połączenia, a ten token jest weryfikowany na serwerze.</span><span class="sxs-lookup"><span data-stu-id="614a1-168">By default, SignalR automatically re-assigns a user to the appropriate groups when reconnecting from a temporary disruption, such as when a connection is dropped and re-established before the connection times out. The user's group information is passed in a token when reconnecting, and that token is verified on the server.</span></span> <span data-ttu-id="614a1-169">Aby uzyskać informacje na temat procesu weryfikacji dla ponownych dołączania użytkowników do grup, zobacz [ponowne łączenie grup po ponownym połączeniu](../security/introduction-to-security.md#rejoingroup).</span><span class="sxs-lookup"><span data-stu-id="614a1-169">For information about the verification process for rejoining users to groups, see [Rejoining groups when reconnecting](../security/introduction-to-security.md#rejoingroup).</span></span>

<span data-ttu-id="614a1-170">Ogólnie rzecz biorąc, należy użyć domyślnego zachowania automatycznego ponownego przyłączania grup przy ponownym połączeniu.</span><span class="sxs-lookup"><span data-stu-id="614a1-170">In general, you should use the default behavior of automatically rejoining groups on reconnect.</span></span> <span data-ttu-id="614a1-171">Grupy sygnałów nie stanowią mechanizmu zabezpieczeń w celu ograniczenia dostępu do poufnych danych.</span><span class="sxs-lookup"><span data-stu-id="614a1-171">SignalR groups are not intended as a security mechanism for restricting access to sensitive data.</span></span> <span data-ttu-id="614a1-172">Jeśli jednak aplikacja musi mieć podwójne sprawdzenie członkostwa w grupie użytkownika podczas ponownego nawiązywania połączenia, można zastąpić zachowanie domyślne.</span><span class="sxs-lookup"><span data-stu-id="614a1-172">However, if your application must double-check a user's group membership when reconnecting, you can override the default behavior.</span></span> <span data-ttu-id="614a1-173">Zmiana domyślnego zachowania może zwiększyć obciążenie bazy danych, ponieważ członkostwo w grupie użytkownika musi być pobrane dla każdego ponownego połączenia, a nie tylko wtedy, gdy użytkownik nawiązuje połączenie.</span><span class="sxs-lookup"><span data-stu-id="614a1-173">Changing the default behavior can add a burden to your database because a user's group membership must be retrieved for each reconnection rather than just when the user connects.</span></span>

<span data-ttu-id="614a1-174">Jeśli konieczne jest zweryfikowanie członkostwa w grupie przy ponownym nawiązaniu połączenia, Utwórz nowy moduł potoku centrum, który zwraca listę przypisanych grup, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="614a1-174">If you must verify group membership on reconnect, create a new hub pipeline module that returns a list of assigned groups, as shown below.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample10.cs)]

<span data-ttu-id="614a1-175">Następnie Dodaj ten moduł do potoku centrum, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="614a1-175">Then, add that module to the hub pipeline, as highlighted below.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample11.cs?highlight=4)]

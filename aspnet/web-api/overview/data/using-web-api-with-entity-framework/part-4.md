---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-4
title: Obsługa relacji jednostek | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: d2f5710c-23c7-40a5-9cd9-5d0516570cba
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-4
msc.type: authoredcontent
ms.openlocfilehash: be4948e5443a5eb4e1824c63dd0c445a7ee1928e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557464"
---
# <a name="handling-entity-relations"></a><span data-ttu-id="c1bcd-102">Obsługa relacji jednostek</span><span class="sxs-lookup"><span data-stu-id="c1bcd-102">Handling Entity Relations</span></span>

<span data-ttu-id="c1bcd-103">według [Jan Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="c1bcd-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="c1bcd-104">Pobierz ukończony projekt</span><span class="sxs-lookup"><span data-stu-id="c1bcd-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="c1bcd-105">W tej sekcji opisano niektóre szczegóły dotyczące tego, jak dr ładuje powiązane jednostki i jak obsłużyć właściwości nawigacji cyklicznej w klasach modelu.</span><span class="sxs-lookup"><span data-stu-id="c1bcd-105">This section describes some details of how EF loads related entities, and how to handle circular navigation properties in your model classes.</span></span> <span data-ttu-id="c1bcd-106">(Ta sekcja zawiera informacje w tle i nie jest wymagana do ukończenia tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="c1bcd-106">(This section provides background knowledge, and is not required to complete the tutorial.</span></span> <span data-ttu-id="c1bcd-107">Jeśli wolisz, przejdź do [części 5.](part-5.md).)</span><span class="sxs-lookup"><span data-stu-id="c1bcd-107">If you prefer, skip to [Part 5.](part-5.md).)</span></span>

## <a name="eager-loading-versus-lazy-loading"></a><span data-ttu-id="c1bcd-108">Ładowanie eager a ładowanie z opóźnieniem</span><span class="sxs-lookup"><span data-stu-id="c1bcd-108">Eager Loading versus Lazy Loading</span></span>

<span data-ttu-id="c1bcd-109">W przypadku korzystania z programu EF z relacyjną bazą danych ważne jest, aby zrozumieć, jak dr ładuje powiązane dane.</span><span class="sxs-lookup"><span data-stu-id="c1bcd-109">When using EF with a relational database, it's important to understand how EF loads related data.</span></span>

<span data-ttu-id="c1bcd-110">Warto również zobaczyć zapytania SQL, które generuje program Dr.</span><span class="sxs-lookup"><span data-stu-id="c1bcd-110">It's also useful to see the SQL queries that EF generates.</span></span> <span data-ttu-id="c1bcd-111">Aby śledzić SQL, Dodaj następujący wiersz kodu do konstruktora `BookServiceContext`:</span><span class="sxs-lookup"><span data-stu-id="c1bcd-111">To trace the SQL, add the following line of code to the `BookServiceContext` constructor:</span></span>

[!code-csharp[Main](part-4/samples/sample1.cs)]

<span data-ttu-id="c1bcd-112">Wysłanie żądania GET do/API/Books zwraca kod JSON podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="c1bcd-112">If you send a GET request to /api/books, it returns JSON like the following:</span></span>

[!code-console[Main](part-4/samples/sample2.cmd)]

<span data-ttu-id="c1bcd-113">Można zobaczyć, że właściwość Author ma wartość null, mimo że książka zawiera prawidłowy AuthorId.</span><span class="sxs-lookup"><span data-stu-id="c1bcd-113">You can see that the Author property is null, even though the book contains a valid AuthorId.</span></span> <span data-ttu-id="c1bcd-114">To dlatego, że EF nie ładuje powiązanych jednostek autora.</span><span class="sxs-lookup"><span data-stu-id="c1bcd-114">That's because EF is not loading the related Author entities.</span></span> <span data-ttu-id="c1bcd-115">Dziennik śledzenia zapytania SQL potwierdza:</span><span class="sxs-lookup"><span data-stu-id="c1bcd-115">The trace log of the SQL query confirms this:</span></span>

[!code-console[Main](part-4/samples/sample3.sql)]

<span data-ttu-id="c1bcd-116">Instrukcja SELECT pobiera z tabeli Books i nie odwołuje się do tabeli autora.</span><span class="sxs-lookup"><span data-stu-id="c1bcd-116">The SELECT statement takes from the Books table, and does not reference the Author table.</span></span>

<span data-ttu-id="c1bcd-117">Poniżej przedstawiono metodę w klasie `BooksController`, która zwraca listę książek.</span><span class="sxs-lookup"><span data-stu-id="c1bcd-117">For reference, here is the method in the `BooksController` class that returns the list of books.</span></span>

[!code-csharp[Main](part-4/samples/sample4.cs)]

<span data-ttu-id="c1bcd-118">Zobaczmy, jak możemy zwrócić autora w ramach danych JSON.</span><span class="sxs-lookup"><span data-stu-id="c1bcd-118">Let's see how we can return the Author as part of the JSON data.</span></span> <span data-ttu-id="c1bcd-119">Istnieją trzy sposoby ładowania powiązanych danych w Entity Framework: ładowanie eager, ładowanie z opóźnieniem i jawne ładowanie.</span><span class="sxs-lookup"><span data-stu-id="c1bcd-119">There are three ways to load related data in Entity Framework: eager loading, lazy loading, and explicit loading.</span></span> <span data-ttu-id="c1bcd-120">Każda Technika ma pewne kompromisy, dlatego ważne jest, aby zrozumieć, jak działają.</span><span class="sxs-lookup"><span data-stu-id="c1bcd-120">There are trade-offs with each technique, so it's important to understand how they work.</span></span>

### <a name="eager-loading"></a><span data-ttu-id="c1bcd-121">Ładowanie eager</span><span class="sxs-lookup"><span data-stu-id="c1bcd-121">Eager Loading</span></span>

<span data-ttu-id="c1bcd-122">W przypadku *ładowania eager*EF ładuje powiązane jednostki jako część początkowej kwerendy bazy danych.</span><span class="sxs-lookup"><span data-stu-id="c1bcd-122">With *eager loading*, EF loads related entities as part of the initial database query.</span></span> <span data-ttu-id="c1bcd-123">Aby przeprowadzić ładowanie eager, użyj metody rozszerzenia **System. Data. Entity. include** .</span><span class="sxs-lookup"><span data-stu-id="c1bcd-123">To perform eager loading, use the **System.Data.Entity.Include** extension method.</span></span>

[!code-csharp[Main](part-4/samples/sample5.cs)]

<span data-ttu-id="c1bcd-124">To informuje Dr o uwzględnieniu danych autora w zapytaniu.</span><span class="sxs-lookup"><span data-stu-id="c1bcd-124">This tells EF to include the Author data in the query.</span></span> <span data-ttu-id="c1bcd-125">Jeśli wprowadzisz tę zmianę i uruchomisz aplikację, teraz dane JSON wyglądają następująco:</span><span class="sxs-lookup"><span data-stu-id="c1bcd-125">If you make this change and run the app, now the JSON data looks like this:</span></span>

[!code-console[Main](part-4/samples/sample6.cmd)]

<span data-ttu-id="c1bcd-126">Dziennik śledzenia pokazuje, że EF wykonał sprzężenie w tabelach książki i autora.</span><span class="sxs-lookup"><span data-stu-id="c1bcd-126">The trace log shows that EF performed a join on the Book and Author tables.</span></span>

[!code-console[Main](part-4/samples/sample7.cmd)]

### <a name="lazy-loading"></a><span data-ttu-id="c1bcd-127">Ładowanie z opóźnieniem</span><span class="sxs-lookup"><span data-stu-id="c1bcd-127">Lazy Loading</span></span>

<span data-ttu-id="c1bcd-128">W przypadku ładowania z opóźnieniem EF automatycznie ładuje powiązaną jednostkę, gdy właściwość nawigacji dla tej jednostki zostanie wykorzystana.</span><span class="sxs-lookup"><span data-stu-id="c1bcd-128">With lazy loading, EF automatically loads a related entity when the navigation property for that entity is dereferenced.</span></span> <span data-ttu-id="c1bcd-129">Aby włączyć ładowanie z opóźnieniem, ustaw właściwość nawigacji na wirtualną.</span><span class="sxs-lookup"><span data-stu-id="c1bcd-129">To enable lazy loading, make the navigation property virtual.</span></span> <span data-ttu-id="c1bcd-130">Na przykład w klasie Book:</span><span class="sxs-lookup"><span data-stu-id="c1bcd-130">For example, in the Book class:</span></span>

[!code-csharp[Main](part-4/samples/sample8.cs?highlight=6)]

<span data-ttu-id="c1bcd-131">Teraz Rozważmy następujący kod:</span><span class="sxs-lookup"><span data-stu-id="c1bcd-131">Now consider the following code:</span></span>

[!code-csharp[Main](part-4/samples/sample9.cs)]

<span data-ttu-id="c1bcd-132">Gdy ładowanie z opóźnieniem jest włączone, uzyskanie dostępu do właściwości `Author` na `books[0]` powoduje, że program Dr będzie wysyłać zapytania do bazy danych dla autora.</span><span class="sxs-lookup"><span data-stu-id="c1bcd-132">When lazy loading is enabled, accessing the `Author` property on `books[0]` causes EF to query the database for the author.</span></span>

<span data-ttu-id="c1bcd-133">Ładowanie z opóźnieniem wymaga przeprowadzenia wielu operacji związanych z bazą danych, ponieważ dr wysyła zapytanie przy każdym pobieraniu powiązanej jednostki.</span><span class="sxs-lookup"><span data-stu-id="c1bcd-133">Lazy loading requires multiple database trips, because EF sends a query each time it retrieves a related entity.</span></span> <span data-ttu-id="c1bcd-134">Ogólnie rzecz biorąc, ładowanie z opóźnieniem jest wyłączone dla obiektów, które można serializować.</span><span class="sxs-lookup"><span data-stu-id="c1bcd-134">Generally, you want lazy loading disabled for objects that you serialize.</span></span> <span data-ttu-id="c1bcd-135">Serializator musi odczytać wszystkie właściwości modelu, które wyzwalają ładowanie powiązanych jednostek.</span><span class="sxs-lookup"><span data-stu-id="c1bcd-135">The serializer has to read all of the properties on the model, which triggers loading the related entities.</span></span> <span data-ttu-id="c1bcd-136">Na przykład poniżej przedstawiono zapytania SQL, gdy EF serializować listę książek z włączonym ładowaniem z opóźnieniem.</span><span class="sxs-lookup"><span data-stu-id="c1bcd-136">For example, here are the SQL queries when EF serializes the list of books with lazy loading enabled.</span></span> <span data-ttu-id="c1bcd-137">Można zobaczyć, że Dr wykonuje trzy osobne zapytania dla trzech autorów.</span><span class="sxs-lookup"><span data-stu-id="c1bcd-137">You can see that EF makes three separate queries for the three authors.</span></span>

[!code-console[Main](part-4/samples/sample10.sql)]

<span data-ttu-id="c1bcd-138">W dalszym ciągu możesz chcieć użyć ładowania z opóźnieniem.</span><span class="sxs-lookup"><span data-stu-id="c1bcd-138">There are still times when you might want to use lazy loading.</span></span> <span data-ttu-id="c1bcd-139">Ładowanie eager może spowodować, że EF generuje bardzo złożone sprzężenia.</span><span class="sxs-lookup"><span data-stu-id="c1bcd-139">Eager loading can cause EF to generate a very complex join.</span></span> <span data-ttu-id="c1bcd-140">Lub mogą być potrzebne powiązane jednostki dla małego podzestawu danych, a ładowanie z opóźnieniem będzie bardziej wydajne.</span><span class="sxs-lookup"><span data-stu-id="c1bcd-140">Or you might need related entities for a small subset of the data, and lazy loading would be more efficient.</span></span>

<span data-ttu-id="c1bcd-141">Jednym ze sposobów uniknięcia problemów serializacji jest Serializacja obiektów transferu danych (DTO) zamiast obiektów jednostek.</span><span class="sxs-lookup"><span data-stu-id="c1bcd-141">One way to avoid serialization problems is to serialize data transfer objects (DTOs) instead of entity objects.</span></span> <span data-ttu-id="c1bcd-142">Pokażę to podejście w dalszej części artykułu.</span><span class="sxs-lookup"><span data-stu-id="c1bcd-142">I'll show this approach later in the article.</span></span>

### <a name="explicit-loading"></a><span data-ttu-id="c1bcd-143">Jawne ładowanie</span><span class="sxs-lookup"><span data-stu-id="c1bcd-143">Explicit Loading</span></span>

<span data-ttu-id="c1bcd-144">Jawne ładowanie jest podobne do ładowania z opóźnieniem, z tą różnicą, że jawnie otrzymujesz powiązane dane w kodzie; nie odbywa się to automatycznie podczas uzyskiwania dostępu do właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="c1bcd-144">Explicit loading is similar to lazy loading, except that you explicitly get the related data in code; it doesn't happen automatically when you access a navigation property.</span></span> <span data-ttu-id="c1bcd-145">Jawne ładowanie daje większą kontrolę nad tym, kiedy ładować powiązane dane, ale wymaga dodatkowego kodu.</span><span class="sxs-lookup"><span data-stu-id="c1bcd-145">Explicit loading gives you more control over when to load related data, but requires extra code.</span></span> <span data-ttu-id="c1bcd-146">Aby uzyskać więcej informacji na temat bezpośredniego ładowania, zobacz [ładowanie powiązanych jednostek](https://msdn.microsoft.com/data/jj574232#explicit).</span><span class="sxs-lookup"><span data-stu-id="c1bcd-146">For more information about explicit loading, see [Loading Related Entities](https://msdn.microsoft.com/data/jj574232#explicit).</span></span>

## <a name="navigation-properties-and-circular-references"></a><span data-ttu-id="c1bcd-147">Właściwości nawigacji i odwołania cykliczne</span><span class="sxs-lookup"><span data-stu-id="c1bcd-147">Navigation Properties and Circular References</span></span>

<span data-ttu-id="c1bcd-148">Po zdefiniowaniu modelu książki i autora definiujełem właściwość nawigacji w klasie `Book` dla relacji Tworzenie książki, ale właściwość nawigacji nie została zdefiniowana w innym kierunku.</span><span class="sxs-lookup"><span data-stu-id="c1bcd-148">When I defined the Book and Author models, I defined a navigation property on the `Book` class for the Book-Author relationship, but I did not define a navigation property in the other direction.</span></span>

<span data-ttu-id="c1bcd-149">Co się stanie w przypadku dodania odpowiedniej właściwości nawigacji do klasy `Author`?</span><span class="sxs-lookup"><span data-stu-id="c1bcd-149">What happens if you add the corresponding navigation property to the `Author` class?</span></span>

[!code-csharp[Main](part-4/samples/sample11.cs?highlight=7)]

<span data-ttu-id="c1bcd-150">Niestety, spowoduje to utworzenie problemu podczas serializacji modeli.</span><span class="sxs-lookup"><span data-stu-id="c1bcd-150">Unfortunately, this creates a problem when you serialize the models.</span></span> <span data-ttu-id="c1bcd-151">Jeśli załadujesz powiązane dane, tworzy cykliczny wykres obiektu.</span><span class="sxs-lookup"><span data-stu-id="c1bcd-151">If you load the related data, it creates a circular object graph.</span></span>

![](part-4/_static/image1.png)

<span data-ttu-id="c1bcd-152">Gdy program formatujący JSON lub XML próbuje serializować grafu, zgłosi wyjątek.</span><span class="sxs-lookup"><span data-stu-id="c1bcd-152">When the JSON or XML formatter tries to serialize the graph, it will throw an exception.</span></span> <span data-ttu-id="c1bcd-153">Dwa elementy formatującego generują różne komunikaty o wyjątkach.</span><span class="sxs-lookup"><span data-stu-id="c1bcd-153">The two formatters throw different exception messages.</span></span> <span data-ttu-id="c1bcd-154">Oto przykład dla programu formatującego JSON:</span><span class="sxs-lookup"><span data-stu-id="c1bcd-154">Here is an example for the JSON formatter:</span></span>

[!code-console[Main](part-4/samples/sample12.cmd)]

<span data-ttu-id="c1bcd-155">Oto elementy formatujące XML:</span><span class="sxs-lookup"><span data-stu-id="c1bcd-155">Here is the XML formatter:</span></span>

[!code-xml[Main](part-4/samples/sample13.xml)]

<span data-ttu-id="c1bcd-156">Jednym z rozwiązań jest użycie DTO, które opisano w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="c1bcd-156">One solution is to use DTOs, which I describe in the next section.</span></span> <span data-ttu-id="c1bcd-157">Alternatywnie można skonfigurować elementy formatujące JSON i XML do obsługi cykli wykresu.</span><span class="sxs-lookup"><span data-stu-id="c1bcd-157">Alternatively, you can configure the JSON and XML formatters to handle graph cycles.</span></span> <span data-ttu-id="c1bcd-158">Aby uzyskać więcej informacji, zobacz [Obsługa cyklicznych odwołań do obiektów](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).</span><span class="sxs-lookup"><span data-stu-id="c1bcd-158">For more information, see [Handling Circular Object References](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).</span></span>

<span data-ttu-id="c1bcd-159">W tym samouczku nie jest potrzebna właściwość nawigacji `Author.Book`, więc możesz ją pozostawić.</span><span class="sxs-lookup"><span data-stu-id="c1bcd-159">For this tutorial, you don't need the `Author.Book` navigation property, so you can leave it out.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="c1bcd-160">[Poprzednie](part-3.md)
> [dalej](part-5.md)</span><span class="sxs-lookup"><span data-stu-id="c1bcd-160">[Previous](part-3.md)
[Next](part-5.md)</span></span>

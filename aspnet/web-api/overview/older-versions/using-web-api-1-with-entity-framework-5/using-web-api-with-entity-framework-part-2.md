---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
title: 'Część 2: Tworzenie modeli domeny | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/03/2012
ms.assetid: fe3ef85f-bdc6-4e10-9768-25aa565c01d0
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
msc.type: authoredcontent
ms.openlocfilehash: 7c5ed1bdb4b390c94907b14e208231f16ad42d96
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600360"
---
# <a name="part-2-creating-the-domain-models"></a><span data-ttu-id="141fd-102">Część 2: Tworzenie modeli domeny</span><span class="sxs-lookup"><span data-stu-id="141fd-102">Part 2: Creating the Domain Models</span></span>

<span data-ttu-id="141fd-103">według [Jan Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="141fd-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="141fd-104">Pobierz ukończony projekt</span><span class="sxs-lookup"><span data-stu-id="141fd-104">Download Completed Project</span></span>](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-models"></a><span data-ttu-id="141fd-105">Dodaj modele</span><span class="sxs-lookup"><span data-stu-id="141fd-105">Add Models</span></span>

<span data-ttu-id="141fd-106">Istnieją trzy sposoby podejścia Entity Framework:</span><span class="sxs-lookup"><span data-stu-id="141fd-106">There are three ways to approach Entity Framework:</span></span>

- <span data-ttu-id="141fd-107">Baza danych — pierwsza: rozpoczyna się od bazy danych, a Entity Framework generuje kod.</span><span class="sxs-lookup"><span data-stu-id="141fd-107">Database-first: You start with a database, and Entity Framework generates the code.</span></span>
- <span data-ttu-id="141fd-108">Model — pierwszy: rozpoczyna się od modelu wizualnego, a Entity Framework generuje zarówno bazę danych, jak i kod.</span><span class="sxs-lookup"><span data-stu-id="141fd-108">Model-first: You start with a visual model, and Entity Framework generates both the database and code.</span></span>
- <span data-ttu-id="141fd-109">Kod — pierwszy: rozpoczyna się od kodu, a Entity Framework generuje bazę danych.</span><span class="sxs-lookup"><span data-stu-id="141fd-109">Code-first: You start with code, and Entity Framework generates the database.</span></span>

<span data-ttu-id="141fd-110">Korzystamy z metody pierwszego kodu, więc zacznijmy od definiowania naszych obiektów domeny jako POCOs (zwykłych obiektów CLR).</span><span class="sxs-lookup"><span data-stu-id="141fd-110">We are using the code-first approach, so we start by defining our domain objects as POCOs (plain-old CLR objects).</span></span> <span data-ttu-id="141fd-111">W przypadku pierwszego podejścia do kodu obiekty domeny nie potrzebują dodatkowego kodu do obsługi warstwy bazy danych, takich jak transakcje lub trwałość.</span><span class="sxs-lookup"><span data-stu-id="141fd-111">With the code-first approach, domain objects don't need any extra code to support the database layer, such as transactions or persistence.</span></span> <span data-ttu-id="141fd-112">(W przeciwnym razie nie muszą dziedziczyć z klasy [EntityObject](https://msdn.microsoft.com/library/system.data.objects.dataclasses.entityobject.aspx) ). Można nadal używać adnotacji danych do kontrolowania sposobu, w jaki Entity Framework tworzy schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="141fd-112">(Specifically, they do not need to inherit from the [EntityObject](https://msdn.microsoft.com/library/system.data.objects.dataclasses.entityobject.aspx) class.) You can still use data annotations to control how Entity Framework creates the database schema.</span></span>

<span data-ttu-id="141fd-113">Ponieważ POCOs nie posiadają żadnych dodatkowych właściwości, które opisują [stan bazy danych](https://msdn.microsoft.com/library/system.data.entitystate.aspx), można je łatwo serializować do formatu JSON lub XML.</span><span class="sxs-lookup"><span data-stu-id="141fd-113">Because POCOs do not carry any extra properties that describe [database state](https://msdn.microsoft.com/library/system.data.entitystate.aspx), they can easily be serialized to JSON or XML.</span></span> <span data-ttu-id="141fd-114">Nie oznacza to jednak, że zawsze należy uwidocznić modele Entity Framework bezpośrednio dla klientów, ponieważ zostaną one wyświetlone w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="141fd-114">However, that does not mean you should always expose your Entity Framework models directly to clients, as we'll see later in the tutorial.</span></span>

<span data-ttu-id="141fd-115">Utworzymy następujący POCOs:</span><span class="sxs-lookup"><span data-stu-id="141fd-115">We will create the following POCOs:</span></span>

- <span data-ttu-id="141fd-116">Produkt</span><span class="sxs-lookup"><span data-stu-id="141fd-116">Product</span></span>
- <span data-ttu-id="141fd-117">Porządek</span><span class="sxs-lookup"><span data-stu-id="141fd-117">Order</span></span>
- <span data-ttu-id="141fd-118">OrderDetail</span><span class="sxs-lookup"><span data-stu-id="141fd-118">OrderDetail</span></span>

<span data-ttu-id="141fd-119">Aby utworzyć każdą klasę, kliknij prawym przyciskiem myszy folder modele w Eksplorator rozwiązań.</span><span class="sxs-lookup"><span data-stu-id="141fd-119">To create each class, right-click the Models folder in Solution Explorer.</span></span> <span data-ttu-id="141fd-120">Z menu kontekstowego wybierz pozycję **Dodaj** , a następnie wybierz pozycję **Klasa.**</span><span class="sxs-lookup"><span data-stu-id="141fd-120">From the context menu, select **Add** and then select **Class.**</span></span>

![](using-web-api-with-entity-framework-part-2/_static/image1.png)

<span data-ttu-id="141fd-121">Dodaj klasę `Product` przy użyciu następującej implementacji:</span><span class="sxs-lookup"><span data-stu-id="141fd-121">Add a `Product` class with the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample1.cs)]

<span data-ttu-id="141fd-122">Zgodnie z Konwencją Entity Framework używa właściwości `Id` jako klucza podstawowego i mapuje ją do kolumny tożsamości w tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="141fd-122">By convention, Entity Framework uses the `Id` property as the primary key and maps it to an identity column in the database table.</span></span> <span data-ttu-id="141fd-123">Gdy tworzysz nowe wystąpienie `Product`, nie ustawisz wartości dla `Id`, ponieważ baza danych generuje wartość.</span><span class="sxs-lookup"><span data-stu-id="141fd-123">When you create a new `Product` instance, you won't set a value for `Id`, because the database generates the value.</span></span>

<span data-ttu-id="141fd-124">Atrybut **ScaffoldColumn** informuje ASP.NET MVC, aby pominąć Właściwość `Id` podczas generowania formularza edytora.</span><span class="sxs-lookup"><span data-stu-id="141fd-124">The **ScaffoldColumn** attribute tells ASP.NET MVC to skip the `Id` property when generating an editor form.</span></span> <span data-ttu-id="141fd-125">**Wymagany** atrybut jest używany do walidacji modelu.</span><span class="sxs-lookup"><span data-stu-id="141fd-125">The **Required** attribute is used to validate the model.</span></span> <span data-ttu-id="141fd-126">Określa, że właściwość `Name` nie może być pustym ciągiem.</span><span class="sxs-lookup"><span data-stu-id="141fd-126">It specifies that the `Name` property must be a non-empty string.</span></span>

<span data-ttu-id="141fd-127">Dodaj klasę `Order`:</span><span class="sxs-lookup"><span data-stu-id="141fd-127">Add the `Order` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample2.cs)]

<span data-ttu-id="141fd-128">Dodaj klasę `OrderDetail`:</span><span class="sxs-lookup"><span data-stu-id="141fd-128">Add the `OrderDetail` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample3.cs)]

## <a name="foreign-key-relations"></a><span data-ttu-id="141fd-129">Relacje klucza obcego</span><span class="sxs-lookup"><span data-stu-id="141fd-129">Foreign Key Relations</span></span>

<span data-ttu-id="141fd-130">Zamówienie zawiera wiele szczegółów zamówienia, a wszystkie szczegóły zamówienia odnoszą się do jednego produktu.</span><span class="sxs-lookup"><span data-stu-id="141fd-130">An order contains many order details, and each order detail refers to a single product.</span></span> <span data-ttu-id="141fd-131">Aby przedstawić te relacje, Klasa `OrderDetail` definiuje właściwości o nazwie `OrderId` i `ProductId`.</span><span class="sxs-lookup"><span data-stu-id="141fd-131">To represent these relations, the `OrderDetail` class defines properties named `OrderId` and `ProductId`.</span></span> <span data-ttu-id="141fd-132">Entity Framework będzie wnioskować, że te właściwości reprezentują klucze obce i doda do bazy danych ograniczenia klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="141fd-132">Entity Framework will infer that these properties represent foreign keys, and will add foreign-key constraints to the database.</span></span>

![](using-web-api-with-entity-framework-part-2/_static/image2.png)

<span data-ttu-id="141fd-133">Klasy `Order` i `OrderDetail` obejmują również właściwości "Nawigacja", które zawierają odwołania do obiektów pokrewnych.</span><span class="sxs-lookup"><span data-stu-id="141fd-133">The `Order` and `OrderDetail` classes also include "navigation" properties, which contain references to the related objects.</span></span> <span data-ttu-id="141fd-134">Mając zamówienie, można przejść do produktów w kolejności, postępując zgodnie z właściwościami nawigacji.</span><span class="sxs-lookup"><span data-stu-id="141fd-134">Given an order, you can navigate to the products in the order by following the navigation properties.</span></span>

<span data-ttu-id="141fd-135">Kompiluj projekt teraz.</span><span class="sxs-lookup"><span data-stu-id="141fd-135">Compile the project now.</span></span> <span data-ttu-id="141fd-136">Entity Framework używa odbicia w celu odnajdywania właściwości modeli, dlatego wymaga skompilowanego zestawu do utworzenia schematu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="141fd-136">Entity Framework uses reflection to discover the properties of the models, so it requires a compiled assembly to create the database schema.</span></span>

## <a name="configure-the-media-type-formatters"></a><span data-ttu-id="141fd-137">Konfigurowanie elementów formatujących typ nośnika</span><span class="sxs-lookup"><span data-stu-id="141fd-137">Configure the Media-Type Formatters</span></span>

<span data-ttu-id="141fd-138">Program [formatujący typ nośnika](../../formats-and-model-binding/media-formatters.md) jest obiektem, który serializować dane, gdy interfejs API sieci Web zapisuje treść odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="141fd-138">A [media-type formatter](../../formats-and-model-binding/media-formatters.md) is an object that serializes your data when Web API writes the HTTP response body.</span></span> <span data-ttu-id="141fd-139">Wbudowane elementy formatujące obsługują dane JSON i XML.</span><span class="sxs-lookup"><span data-stu-id="141fd-139">The built-in formatters support JSON and XML output.</span></span> <span data-ttu-id="141fd-140">Domyślnie oba te programy formatujące są serializowane wszystkie obiekty według wartości.</span><span class="sxs-lookup"><span data-stu-id="141fd-140">By default, both of these formatters serialize all objects by value.</span></span>

<span data-ttu-id="141fd-141">Serializacja przez wartość tworzy problem, jeśli Graf obiektu zawiera odwołania cykliczne.</span><span class="sxs-lookup"><span data-stu-id="141fd-141">Serialization by value creates a problem if an object graph contains circular references.</span></span> <span data-ttu-id="141fd-142">Jest to dokładnie przypadek z klasami `Order` i `OrderDetail`, ponieważ każdy z nich przechowuje odwołanie do drugiego.</span><span class="sxs-lookup"><span data-stu-id="141fd-142">That's exactly the case with the `Order` and `OrderDetail` classes, because each holds a reference to the other.</span></span> <span data-ttu-id="141fd-143">Program formatujący będzie podążał za odwołaniami, pisząc każdy obiekt według wartości i przejść w okręgach.</span><span class="sxs-lookup"><span data-stu-id="141fd-143">The formatter will follow the references, writing each object by value, and go in circles.</span></span> <span data-ttu-id="141fd-144">W związku z tym musimy zmienić zachowanie domyślne.</span><span class="sxs-lookup"><span data-stu-id="141fd-144">Therefore, we need to change the default behavior.</span></span>

<span data-ttu-id="141fd-145">W Eksplorator rozwiązań rozwiń węzeł aplikacja\_folder Start i Otwórz plik o nazwie WebApiConfig.cs.</span><span class="sxs-lookup"><span data-stu-id="141fd-145">In Solution Explorer, expand the App\_Start folder and open the file named WebApiConfig.cs.</span></span> <span data-ttu-id="141fd-146">Dodaj następujący kod do klasy `WebApiConfig`:</span><span class="sxs-lookup"><span data-stu-id="141fd-146">Add the following code to the `WebApiConfig` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample4.cs?highlight=11)]

<span data-ttu-id="141fd-147">Ten kod ustawia program formatujący JSON w celu zachowania odwołań do obiektów i całkowicie usuwa plik XML z potoku.</span><span class="sxs-lookup"><span data-stu-id="141fd-147">This code sets the JSON formatter to preserve object references, and removes the XML formatter from the pipeline entirely.</span></span> <span data-ttu-id="141fd-148">(Można skonfigurować program formatujący XML, aby zachować odwołania do obiektów, ale jest nieco bardziej wydajny i potrzebujemy tylko formatu JSON dla tej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="141fd-148">(You can configure the XML formatter to preserve object references, but it's a little more work, and we only need JSON for this application.</span></span> <span data-ttu-id="141fd-149">Aby uzyskać więcej informacji, zobacz [Obsługa cyklicznych odwołań do obiektów](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).)</span><span class="sxs-lookup"><span data-stu-id="141fd-149">For more information, see [Handling Circular Object References](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).)</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="141fd-150">[Poprzednie](using-web-api-with-entity-framework-part-1.md)
> [dalej](using-web-api-with-entity-framework-part-3.md)</span><span class="sxs-lookup"><span data-stu-id="141fd-150">[Previous](using-web-api-with-entity-framework-part-1.md)
[Next](using-web-api-with-entity-framework-part-3.md)</span></span>

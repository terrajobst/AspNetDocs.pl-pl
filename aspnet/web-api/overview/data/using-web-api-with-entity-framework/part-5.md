---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-5
title: Tworzenie Transfer danych obiektów (DTO) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 0fd07176-b74b-48f0-9fac-0f02e3ffa213
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-5
msc.type: authoredcontent
ms.openlocfilehash: fc0463420207eba764014b8ec7123c5150e38247
ms.sourcegitcommit: 84b1681d4e6253e30468c8df8a09fe03beea9309
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/02/2019
ms.locfileid: "73445764"
---
# <a name="create-data-transfer-objects-dtos"></a><span data-ttu-id="c2344-102">Tworzenie obiektów transferu danych (DTO)</span><span class="sxs-lookup"><span data-stu-id="c2344-102">Create Data Transfer Objects (DTOs)</span></span>

<span data-ttu-id="c2344-103">według [Jan Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="c2344-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="c2344-104">Pobierz ukończony projekt</span><span class="sxs-lookup"><span data-stu-id="c2344-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="c2344-105">Teraz nasz interfejs API sieci Web uwidacznia jednostki bazy danych klientowi.</span><span class="sxs-lookup"><span data-stu-id="c2344-105">Right now, our web API exposes the database entities to the client.</span></span> <span data-ttu-id="c2344-106">Klient odbiera dane, które są mapowane bezpośrednio do tabel bazy danych.</span><span class="sxs-lookup"><span data-stu-id="c2344-106">The client receives data that maps directly to your database tables.</span></span> <span data-ttu-id="c2344-107">Jednak nie zawsze jest dobrym pomysłem.</span><span class="sxs-lookup"><span data-stu-id="c2344-107">However, that's not always a good idea.</span></span> <span data-ttu-id="c2344-108">Czasami chcesz zmienić kształt danych wysyłanych na klienta.</span><span class="sxs-lookup"><span data-stu-id="c2344-108">Sometimes you want to change the shape of the data that you send to client.</span></span> <span data-ttu-id="c2344-109">Na przykład możesz chcieć:</span><span class="sxs-lookup"><span data-stu-id="c2344-109">For example, you might want to:</span></span>

- <span data-ttu-id="c2344-110">Usuń odwołania cykliczne (zobacz poprzednią sekcję).</span><span class="sxs-lookup"><span data-stu-id="c2344-110">Remove circular references (see previous section).</span></span>
- <span data-ttu-id="c2344-111">Ukryj konkretne właściwości, które nie powinny być wyświetlane dla klientów.</span><span class="sxs-lookup"><span data-stu-id="c2344-111">Hide particular properties that clients are not supposed to view.</span></span>
- <span data-ttu-id="c2344-112">Pomiń niektóre właściwości, aby zmniejszyć rozmiar ładunku.</span><span class="sxs-lookup"><span data-stu-id="c2344-112">Omit some properties in order to reduce payload size.</span></span>
- <span data-ttu-id="c2344-113">Spłaszcz wykresy obiektów zawierające obiekty zagnieżdżone, aby zwiększyć ich wygodę dla klientów.</span><span class="sxs-lookup"><span data-stu-id="c2344-113">Flatten object graphs that contain nested objects, to make them more convenient for clients.</span></span>
- <span data-ttu-id="c2344-114">Unikaj luk w zabezpieczeniach.</span><span class="sxs-lookup"><span data-stu-id="c2344-114">Avoid "over-posting" vulnerabilities.</span></span> <span data-ttu-id="c2344-115">(Zobacz [Walidacja modelu](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md) w przypadku dyskusji o nadmiernym księgowaniu).</span><span class="sxs-lookup"><span data-stu-id="c2344-115">(See [Model Validation](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md) for a discussion of over-posting.)</span></span>
- <span data-ttu-id="c2344-116">Oddziel warstwę usługi od warstwy bazy danych.</span><span class="sxs-lookup"><span data-stu-id="c2344-116">Decouple your service layer from your database layer.</span></span>

<span data-ttu-id="c2344-117">Aby to osiągnąć, można zdefiniować *obiekt transferu danych* (DTO).</span><span class="sxs-lookup"><span data-stu-id="c2344-117">To accomplish this, you can define a *data transfer object* (DTO).</span></span> <span data-ttu-id="c2344-118">DTO jest obiektem, który definiuje sposób przesyłania danych przez sieć.</span><span class="sxs-lookup"><span data-stu-id="c2344-118">A DTO is an object that defines how the data will be sent over the network.</span></span> <span data-ttu-id="c2344-119">Zobaczmy, jak działa z jednostką książki.</span><span class="sxs-lookup"><span data-stu-id="c2344-119">Let's see how that works with the Book entity.</span></span> <span data-ttu-id="c2344-120">W folderze modele Dodaj dwie klasy DTO:</span><span class="sxs-lookup"><span data-stu-id="c2344-120">In the Models folder, add two DTO classes:</span></span>

[!code-csharp[Main](part-5/samples/sample1.cs)]

<span data-ttu-id="c2344-121">Klasa `BookDetailDto` zawiera wszystkie właściwości modelu książki, z tą różnicą, że `AuthorName` jest ciągiem, który będzie zawierać nazwę autora.</span><span class="sxs-lookup"><span data-stu-id="c2344-121">The `BookDetailDto` class includes all of the properties from the Book model, except that `AuthorName` is a string that will hold the author name.</span></span> <span data-ttu-id="c2344-122">Klasa `BookDto` zawiera podzbiór właściwości z `BookDetailDto`.</span><span class="sxs-lookup"><span data-stu-id="c2344-122">The `BookDto` class contains a subset of properties from `BookDetailDto`.</span></span>

<span data-ttu-id="c2344-123">Następnie zastąp dwie metody GET w klasie `BooksController` z wersjami, które zwracają DTO.</span><span class="sxs-lookup"><span data-stu-id="c2344-123">Next, replace the two GET methods in the `BooksController` class, with versions that return DTOs.</span></span> <span data-ttu-id="c2344-124">Użyjemy instrukcji **SELECT** języka LINQ do konwersji z jednostek książki na DTO.</span><span class="sxs-lookup"><span data-stu-id="c2344-124">We'll use the LINQ **Select** statement to convert from Book entities into DTOs.</span></span>

[!code-csharp[Main](part-5/samples/sample2.cs)]

<span data-ttu-id="c2344-125">Oto kod SQL generowany przez nową metodę `GetBooks`.</span><span class="sxs-lookup"><span data-stu-id="c2344-125">Here is the SQL generated by the new `GetBooks` method.</span></span> <span data-ttu-id="c2344-126">Można zobaczyć, że EF tłumaczy element LINQ **SELECT** na instrukcję SELECT języka SQL.</span><span class="sxs-lookup"><span data-stu-id="c2344-126">You can see that EF translates the LINQ **Select** into a SQL SELECT statement.</span></span>

[!code-sql[Main](part-5/samples/sample3.sql)]

<span data-ttu-id="c2344-127">Na koniec zmodyfikuj metodę `PostBook`, aby zwracała DTO.</span><span class="sxs-lookup"><span data-stu-id="c2344-127">Finally, modify the `PostBook` method to return a DTO.</span></span>

[!code-csharp[Main](part-5/samples/sample4.cs)]

> [!NOTE]
> <span data-ttu-id="c2344-128">W tym samouczku Konwertujemy się na DTO ręcznie w kodzie.</span><span class="sxs-lookup"><span data-stu-id="c2344-128">In this tutorial, we're converting to DTOs manually in code.</span></span> <span data-ttu-id="c2344-129">Inną opcją jest użycie biblioteki, takiej jak [automapowanie](http://automapper.org/) , która obsługuje konwersję automatycznie.</span><span class="sxs-lookup"><span data-stu-id="c2344-129">Another option is to use a library like [AutoMapper](http://automapper.org/) that handles the conversion automatically.</span></span>
> 
> [!div class="step-by-step"]
> <span data-ttu-id="c2344-130">[Poprzedni](part-4.md)
> [Następny](part-6.md)</span><span class="sxs-lookup"><span data-stu-id="c2344-130">[Previous](part-4.md)
[Next](part-6.md)</span></span>

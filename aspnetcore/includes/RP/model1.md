---
ms.openlocfilehash: 934db70fe49ba9a5330b25596dc694e0ac50c04e
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57069995"
---
<span data-ttu-id="0caa4-101">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0caa4-101">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="0caa4-102">W tej sekcji dodasz klasy zarządzania filmów w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="0caa4-102">In this section, you add classes for managing movies in a database.</span></span> <span data-ttu-id="0caa4-103">Użyj tych klas z [Entity Framework Core](/ef/core) (EF Core) do pracy z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="0caa4-103">You use these classes with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="0caa4-104">EF Core to platforma mapowania obiektowo relacyjny (ORM), która upraszcza kod dostępu do danych, który trzeba napisać.</span><span class="sxs-lookup"><span data-stu-id="0caa4-104">EF Core is an object-relational mapping (ORM) framework that simplifies the data access code that you have to write.</span></span>

<span data-ttu-id="0caa4-105">Klasy modeli, które możesz utworzyć są znane jako klasy POCO (od "old zwykły CLR obiekty"), ponieważ nie mają żadnych zależności programu EF Core.</span><span class="sxs-lookup"><span data-stu-id="0caa4-105">The model classes you create are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="0caa4-106">Mogą określać właściwości danych, które są przechowywane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="0caa4-106">They define the properties of the data that are stored in the database.</span></span>

<span data-ttu-id="0caa4-107">W tym samouczku pisania klasy modeli i programem EF Core tworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="0caa4-107">In this tutorial, you write the model classes first, and EF Core creates the database.</span></span> <span data-ttu-id="0caa4-108">Alternatywnym podejściu, nieuwzględnione w tym miejscu jest [wygenerowane klasy modelu z istniejącej bazy danych](/ef/core/get-started/aspnetcore/existing-db).</span><span class="sxs-lookup"><span data-stu-id="0caa4-108">An alternate approach not covered here is to [generate model classes from an existing database](/ef/core/get-started/aspnetcore/existing-db).</span></span>

<span data-ttu-id="0caa4-109">[Wyświetlanie lub pobieranie](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) próbki.</span><span class="sxs-lookup"><span data-stu-id="0caa4-109">[View or download](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) sample.</span></span>

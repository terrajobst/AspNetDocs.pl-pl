---
ms.openlocfilehash: 72e33ea44976963193d2560427fc418875be450e
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57070292"
---
# <a name="add-a-model-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="388a4-101">Dodawanie modelu do aplikacji ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="388a4-101">Add a model to an ASP.NET Core MVC app</span></span>

<span data-ttu-id="388a4-102">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="388a4-102">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="388a4-103">W tej sekcji dodasz niektóre klasy zarządzania filmów w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="388a4-103">In this section, you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="388a4-104">Te klasy będzie "**M**odelu" wchodzi w skład **M**VC aplikacji.</span><span class="sxs-lookup"><span data-stu-id="388a4-104">These classes will be the "**M**odel" part of the **M**VC app.</span></span>

<span data-ttu-id="388a4-105">Użyj tych klas z [Entity Framework Core](/ef/core) (EF Core) do pracy z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="388a4-105">You use these classes with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="388a4-106">EF Core to platforma mapowania obiektowo relacyjny (ORM), która upraszcza kod dostępu do danych, który trzeba napisać.</span><span class="sxs-lookup"><span data-stu-id="388a4-106">EF Core is an object-relational mapping (ORM) framework that simplifies the data access code that you have to write.</span></span> <span data-ttu-id="388a4-107">[EF Core obsługuje wiele baz danych](/ef/core/providers/).</span><span class="sxs-lookup"><span data-stu-id="388a4-107">[EF Core supports many database engines](/ef/core/providers/).</span></span>

<span data-ttu-id="388a4-108">Klasy modeli, które zostaną utworzone są znane jako klasy POCO (od "old zwykły CLR obiekty"), ponieważ nie mają żadnych zależności programu EF Core.</span><span class="sxs-lookup"><span data-stu-id="388a4-108">The model classes you'll create are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="388a4-109">Określają one po prostu właściwości danych, które będą przechowywane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="388a4-109">They just define the properties of the data that will be stored in the database.</span></span>

<span data-ttu-id="388a4-110">W tym samouczku przedstawiono tworzenie klas modelu najpierw i programem EF Core spowoduje utworzenie bazy danych.</span><span class="sxs-lookup"><span data-stu-id="388a4-110">In this tutorial you'll write the model classes first, and EF Core will create the database.</span></span> <span data-ttu-id="388a4-111">Alternatywnym podejściu, nieuwzględnione w tym miejscu jest do generowania klasy modelu z bazy danych już istnieje.</span><span class="sxs-lookup"><span data-stu-id="388a4-111">An alternate approach not covered here is to generate model classes from an already-existing database.</span></span> <span data-ttu-id="388a4-112">Aby uzyskać informacji na temat tego podejścia, zobacz [ASP.NET Core — istniejąca baza danych](/ef/core/get-started/aspnetcore/existing-db).</span><span class="sxs-lookup"><span data-stu-id="388a4-112">For information about that approach, see [ASP.NET Core - Existing Database](/ef/core/get-started/aspnetcore/existing-db).</span></span>

## <a name="add-a-data-model-class"></a><span data-ttu-id="388a4-113">Dodaj klasę modelu danych</span><span class="sxs-lookup"><span data-stu-id="388a4-113">Add a data model class</span></span>

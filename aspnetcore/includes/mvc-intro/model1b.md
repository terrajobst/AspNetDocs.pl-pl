---
ms.openlocfilehash: 1c342231905775938715280681ea2b4cf6ebc9bc
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57073025"
---
<span data-ttu-id="be074-101">Dodaj następujące właściwości do `Movie` klasy:</span><span class="sxs-lookup"><span data-stu-id="be074-101">Add the following properties to the `Movie` class:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Models/Movie.cs?name=snippet1)]

<span data-ttu-id="be074-102">`Movie` Klasa zawiera:</span><span class="sxs-lookup"><span data-stu-id="be074-102">The `Movie` class contains:</span></span>

* <span data-ttu-id="be074-103">`Id` Pola, które jest wymagane przez bazę danych dla klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="be074-103">The `Id` field which is required by the database for the primary key.</span></span>
* <span data-ttu-id="be074-104">`[DataType(DataType.Date)]`:  [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) atrybut określa typ danych (`Date`).</span><span class="sxs-lookup"><span data-stu-id="be074-104">`[DataType(DataType.Date)]`:  The [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) attribute specifies the type of the data (`Date`).</span></span> <span data-ttu-id="be074-105">Z tego atrybutu:</span><span class="sxs-lookup"><span data-stu-id="be074-105">With this attribute:</span></span>

  * <span data-ttu-id="be074-106">Użytkownik nie musi wprowadzić informacje o czasie w polu daty.</span><span class="sxs-lookup"><span data-stu-id="be074-106">The user is not required to enter time information in the date field.</span></span>
  * <span data-ttu-id="be074-107">Tylko data jest wyświetlany, nie czas informacji.</span><span class="sxs-lookup"><span data-stu-id="be074-107">Only the date is displayed, not time information.</span></span>

<span data-ttu-id="be074-108">[DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) zostały omówione później w samouczku.</span><span class="sxs-lookup"><span data-stu-id="be074-108">[DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) are covered in a later tutorial.</span></span>
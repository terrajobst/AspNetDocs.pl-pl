---
ms.openlocfilehash: 025a244812a50709bfd48de9f47a234a547c53e2
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57072869"
---
<span data-ttu-id="62369-101"><!-- THIS INCLUDE USED BY MVC AND RP --> Dodaj następujące właściwości do `Movie` klasy:</span><span class="sxs-lookup"><span data-stu-id="62369-101"><!-- THIS INCLUDE USED BY MVC AND RP --> Add the following properties to the `Movie` class:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/Movie.cs?name=snippet1)]

<span data-ttu-id="62369-102">`Movie` Klasa zawiera:</span><span class="sxs-lookup"><span data-stu-id="62369-102">The `Movie` class contains:</span></span>

* <span data-ttu-id="62369-103">`ID` Pole jest wymagane przez bazę danych dla klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="62369-103">The `ID` field is required by the database for the primary key.</span></span>
* <span data-ttu-id="62369-104">`[DataType(DataType.Date)]`:  [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) atrybut określa typ danych (Data).</span><span class="sxs-lookup"><span data-stu-id="62369-104">`[DataType(DataType.Date)]`:  The [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) attribute specifies the type of the data (Date).</span></span> <span data-ttu-id="62369-105">Z tego atrybutu:</span><span class="sxs-lookup"><span data-stu-id="62369-105">With this attribute:</span></span>

  * <span data-ttu-id="62369-106">Użytkownik nie musi wprowadzić informacje o czasie w polu daty.</span><span class="sxs-lookup"><span data-stu-id="62369-106">The user is not required to enter time information in the date field.</span></span>
  * <span data-ttu-id="62369-107">Tylko data jest wyświetlany, nie czas informacji.</span><span class="sxs-lookup"><span data-stu-id="62369-107">Only the date is displayed, not time information.</span></span>

<span data-ttu-id="62369-108">[DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) zostały omówione później w samouczku.</span><span class="sxs-lookup"><span data-stu-id="62369-108">[DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) are covered in a later tutorial.</span></span>
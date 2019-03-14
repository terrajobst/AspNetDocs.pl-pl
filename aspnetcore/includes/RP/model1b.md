---
ms.openlocfilehash: 025a244812a50709bfd48de9f47a234a547c53e2
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57072869"
---
<!-- THIS INCLUDE USED BY MVC AND RP --> Dodaj następujące właściwości do `Movie` klasy:

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/Movie.cs?name=snippet1)]

`Movie` Klasa zawiera:

* `ID` Pole jest wymagane przez bazę danych dla klucza podstawowego.
* `[DataType(DataType.Date)]`:  [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) atrybut określa typ danych (Data). Z tego atrybutu:

  * Użytkownik nie musi wprowadzić informacje o czasie w polu daty.
  * Tylko data jest wyświetlany, nie czas informacji.

[DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) zostały omówione później w samouczku.
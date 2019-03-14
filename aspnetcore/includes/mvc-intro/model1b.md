---
ms.openlocfilehash: 1c342231905775938715280681ea2b4cf6ebc9bc
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57073025"
---
Dodaj następujące właściwości do `Movie` klasy:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Models/Movie.cs?name=snippet1)]

`Movie` Klasa zawiera:

* `Id` Pola, które jest wymagane przez bazę danych dla klucza podstawowego.
* `[DataType(DataType.Date)]`:  [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) atrybut określa typ danych (`Date`). Z tego atrybutu:

  * Użytkownik nie musi wprowadzić informacje o czasie w polu daty.
  * Tylko data jest wyświetlany, nie czas informacji.

[DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) zostały omówione później w samouczku.
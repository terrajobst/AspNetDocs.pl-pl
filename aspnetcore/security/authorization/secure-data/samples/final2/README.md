---
ms.openlocfilehash: e40d28e9a7ca12efe45988fabef23dece893d428
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57073376"
---
# <a name="how-to-buildrun-secure-user-data-sample"></a>Jak kompilacji/uruchomić próbki danych bezpiecznego użytkownika

* Ustaw hasło, za pomocą narzędzia menedżera klucz tajny:

  `dotnet user-secrets set SeedUserPW <pw>`

* Aktualizacja bazy danych:

    `dotnet ef database update`

* Włączanie obsługi protokołu HTTPS w projekcie

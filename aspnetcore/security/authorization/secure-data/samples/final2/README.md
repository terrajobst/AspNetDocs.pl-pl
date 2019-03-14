---
ms.openlocfilehash: e40d28e9a7ca12efe45988fabef23dece893d428
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57073376"
---
# <a name="how-to-buildrun-secure-user-data-sample"></a><span data-ttu-id="e7798-101">Jak kompilacji/uruchomić próbki danych bezpiecznego użytkownika</span><span class="sxs-lookup"><span data-stu-id="e7798-101">How to build/run Secure user data sample</span></span>

* <span data-ttu-id="e7798-102">Ustaw hasło, za pomocą narzędzia menedżera klucz tajny:</span><span class="sxs-lookup"><span data-stu-id="e7798-102">Set password with the Secret Manager tool:</span></span>

  `dotnet user-secrets set SeedUserPW <pw>`

* <span data-ttu-id="e7798-103">Aktualizacja bazy danych:</span><span class="sxs-lookup"><span data-stu-id="e7798-103">Update the database:</span></span>

    `dotnet ef database update`

* <span data-ttu-id="e7798-104">Włączanie obsługi protokołu HTTPS w projekcie</span><span class="sxs-lookup"><span data-stu-id="e7798-104">Enable HTTPS in the project</span></span>

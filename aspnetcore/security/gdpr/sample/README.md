---
ms.openlocfilehash: 96cd8c78178b02ad3eb4a471540950b4581af583
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57068633"
---
# <a name="gdpr-sample"></a><span data-ttu-id="f1f77-101">Przykładowe RODO</span><span class="sxs-lookup"><span data-stu-id="f1f77-101">GDPR Sample</span></span>

* <span data-ttu-id="f1f77-102">W *appsettings.json*ustaw `CheckNotConsentNeeded` do `false` wymagające zgody; w przeciwnym razie wartość true lub pominąć.</span><span class="sxs-lookup"><span data-stu-id="f1f77-102">In *appsettings.json*, set `CheckNotConsentNeeded` to `false` to require consent; otherwise set to true or omit.</span></span> <span data-ttu-id="f1f77-103">Testowanie aplikacji w usłudze `CheckNotConsentNeeded` równa `false` i ustaw `true`.</span><span class="sxs-lookup"><span data-stu-id="f1f77-103">Test the app with `CheckNotConsentNeeded` set to `false` and set to `true`.</span></span>
* <span data-ttu-id="f1f77-104">Utwórz podstawowe i inne niż niezbędne pliki cookie z każdej wersji `CheckConsentNeeded` i udzielono zgody.</span><span class="sxs-lookup"><span data-stu-id="f1f77-104">Create essential and non-essential cookies with each variation of `CheckConsentNeeded` and consent granted.</span></span>
* <span data-ttu-id="f1f77-105">Zarejestruj użytkownika.</span><span class="sxs-lookup"><span data-stu-id="f1f77-105">Register a user.</span></span>
* <span data-ttu-id="f1f77-106">Usuń pliki cookie.</span><span class="sxs-lookup"><span data-stu-id="f1f77-106">Delete cookies.</span></span>

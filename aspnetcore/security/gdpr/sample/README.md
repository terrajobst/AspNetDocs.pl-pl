---
ms.openlocfilehash: 96cd8c78178b02ad3eb4a471540950b4581af583
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57068633"
---
# <a name="gdpr-sample"></a>Przykładowe RODO

* W *appsettings.json*ustaw `CheckNotConsentNeeded` do `false` wymagające zgody; w przeciwnym razie wartość true lub pominąć. Testowanie aplikacji w usłudze `CheckNotConsentNeeded` równa `false` i ustaw `true`.
* Utwórz podstawowe i inne niż niezbędne pliki cookie z każdej wersji `CheckConsentNeeded` i udzielono zgody.
* Zarejestruj użytkownika.
* Usuń pliki cookie.

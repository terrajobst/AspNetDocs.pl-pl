---
ms.openlocfilehash: 2cd201e16cd491d0f468e8ef141b522f1694a257
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57165865"
---
**Ostrzeżenie**: Poniższy kod używa `GetTempFileName`, który zgłasza `IOException` Jeśli więcej niż 65535 pliki są tworzone bez usuwania poprzedniego plików tymczasowych. Rzeczywistej aplikacji należy usunąć plików tymczasowych albo użyj `GetTempPath` i `GetRandomFileName` do tworzenia nazwy tymczasowego pliku. Pliki 65535 wynoszący na serwer, więc wszystkie pliki 65535 korzystać z innej aplikacji na serwerze. 

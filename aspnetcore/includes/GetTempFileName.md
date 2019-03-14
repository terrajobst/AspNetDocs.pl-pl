---
ms.openlocfilehash: 2cd201e16cd491d0f468e8ef141b522f1694a257
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57165865"
---
<span data-ttu-id="e6f1f-101">**Ostrzeżenie**: Poniższy kod używa `GetTempFileName`, który zgłasza `IOException` Jeśli więcej niż 65535 pliki są tworzone bez usuwania poprzedniego plików tymczasowych.</span><span class="sxs-lookup"><span data-stu-id="e6f1f-101">**Warning**: The following code uses `GetTempFileName`, which throws an `IOException` if more than 65535 files are created without deleting previous temporary files.</span></span> <span data-ttu-id="e6f1f-102">Rzeczywistej aplikacji należy usunąć plików tymczasowych albo użyj `GetTempPath` i `GetRandomFileName` do tworzenia nazwy tymczasowego pliku.</span><span class="sxs-lookup"><span data-stu-id="e6f1f-102">A real app should either delete temporary files or use `GetTempPath` and `GetRandomFileName` to create temporary file names.</span></span> <span data-ttu-id="e6f1f-103">Pliki 65535 wynoszący na serwer, więc wszystkie pliki 65535 korzystać z innej aplikacji na serwerze.</span><span class="sxs-lookup"><span data-stu-id="e6f1f-103">The 65535 files limit is per server, so another app on the server can use up all 65535 files.</span></span> 

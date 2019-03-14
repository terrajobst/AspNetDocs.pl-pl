---
ms.openlocfilehash: b90e7963c5d9e5ef09fb519b72672c63bdffabee
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57796540"
---
> Niektóre polecenia nie są obsługiwane, jeśli aplikacja korzysta z bazy danych SQLite jako magazynu danych tożsamości. Ze względu na ograniczenia w aparacie bazy danych `Alter` polecenia throw następujący wyjątek:
>
> "System.NotSupportedException: Bazy danych SQLite nie obsługuje tej operacji migracji." 
>
> Jako obejście Uruchom migracje Code First na bazę danych do tabel zmian.

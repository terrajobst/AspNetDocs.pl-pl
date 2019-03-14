---
ms.openlocfilehash: b90e7963c5d9e5ef09fb519b72672c63bdffabee
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57796540"
---
> <span data-ttu-id="30066-101">Niektóre polecenia nie są obsługiwane, jeśli aplikacja korzysta z bazy danych SQLite jako magazynu danych tożsamości.</span><span class="sxs-lookup"><span data-stu-id="30066-101">Some commands aren't supported if the app uses SQLite as its Identity data store.</span></span> <span data-ttu-id="30066-102">Ze względu na ograniczenia w aparacie bazy danych `Alter` polecenia throw następujący wyjątek:</span><span class="sxs-lookup"><span data-stu-id="30066-102">Due to limitations in the database engine, `Alter` commands throw the following exception:</span></span>
>
> <span data-ttu-id="30066-103">"System.NotSupportedException: Bazy danych SQLite nie obsługuje tej operacji migracji."</span><span class="sxs-lookup"><span data-stu-id="30066-103">"System.NotSupportedException: SQLite does not support this migration operation."</span></span> 
>
> <span data-ttu-id="30066-104">Jako obejście Uruchom migracje Code First na bazę danych do tabel zmian.</span><span class="sxs-lookup"><span data-stu-id="30066-104">As a work around, run Code First migrations on the database to change the tables.</span></span>

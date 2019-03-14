---
ms.openlocfilehash: 17ae8088e2e570628883ea5f8d71e093e6ed7cc8
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57071546"
---
# <a name="data-protection-key-folder"></a><span data-ttu-id="c6008-101">Folder klucza ochrony danych</span><span class="sxs-lookup"><span data-stu-id="c6008-101">Data protection key folder</span></span>

<span data-ttu-id="c6008-102">Ten plik jest symbolem zastępczym, aby utworzyć folder udostępniony dla kluczy ochrony danych.</span><span class="sxs-lookup"><span data-stu-id="c6008-102">This file is a placeholder to create a shared folder for data protection keys.</span></span>

<span data-ttu-id="c6008-103">W przypadku wdrożenia produkcyjnego należy umieścić klucze poza głównym rozwoju i nigdy nie ewidencjonowania plików, w tym katalogu w kontroli źródła.</span><span class="sxs-lookup"><span data-stu-id="c6008-103">In a production deployment, place the keys outside of the development root and never check-in files in this directory into source control.</span></span> <span data-ttu-id="c6008-104">Chroń klucze ochrony danych w plikach za pomocą interfejsu DPAPI lub X509Certificate.</span><span class="sxs-lookup"><span data-stu-id="c6008-104">Protect data protection keys in the files with DPAPI or an X509Certificate.</span></span>

<span data-ttu-id="c6008-105">Zobacz [ochrony danych w programie ASP.NET Core: Interfejsy API konsumenta, konfiguracji, interfejsów API rozszerzania i wdrażania](https://docs.microsoft.com/aspnet/core/security/data-protection/) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="c6008-105">See [Data Protection in ASP.NET Core: Consumer APIs, configuration, extensibility APIs and implementation](https://docs.microsoft.com/aspnet/core/security/data-protection/) for more information.</span></span>

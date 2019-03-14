---
ms.openlocfilehash: 568fa161b27e554fd8b474670b8f9a7ec2f39f45
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57072782"
---
<span data-ttu-id="51724-101">W poniższej tabeli przedstawiono parametry generator kodu ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="51724-101">The following table details the ASP.NET Core code generator parameters:</span></span>

| <span data-ttu-id="51724-102">Parametr</span><span class="sxs-lookup"><span data-stu-id="51724-102">Parameter</span></span>               | <span data-ttu-id="51724-103">Opis</span><span class="sxs-lookup"><span data-stu-id="51724-103">Description</span></span>|
| ----------------- | ------------ |
| <span data-ttu-id="51724-104">-m</span><span class="sxs-lookup"><span data-stu-id="51724-104">-m</span></span>  | <span data-ttu-id="51724-105">Nazwa modelu.</span><span class="sxs-lookup"><span data-stu-id="51724-105">The name of the model.</span></span> |
| <span data-ttu-id="51724-106">-dc</span><span class="sxs-lookup"><span data-stu-id="51724-106">-dc</span></span>  | <span data-ttu-id="51724-107">Kontekst danych.</span><span class="sxs-lookup"><span data-stu-id="51724-107">The data context.</span></span> |
| <span data-ttu-id="51724-108">-udl</span><span class="sxs-lookup"><span data-stu-id="51724-108">-udl</span></span> | <span data-ttu-id="51724-109">Użyj domyślnego układu.</span><span class="sxs-lookup"><span data-stu-id="51724-109">Use the default layout.</span></span> |
| <span data-ttu-id="51724-110">--relativeFolderPath</span><span class="sxs-lookup"><span data-stu-id="51724-110">--relativeFolderPath</span></span> | <span data-ttu-id="51724-111">Ścieżka folderu wyjściowego względne do tworzenia widoków.</span><span class="sxs-lookup"><span data-stu-id="51724-111">The relative output folder path to create the views.</span></span> |
| <span data-ttu-id="51724-112">--useDefaultLayout</span><span class="sxs-lookup"><span data-stu-id="51724-112">--useDefaultLayout</span></span> | <span data-ttu-id="51724-113">Układ domyślny, należy używać widoków.</span><span class="sxs-lookup"><span data-stu-id="51724-113">The default layout should be used for the views.</span></span> |
| <span data-ttu-id="51724-114">--referenceScriptLibraries</span><span class="sxs-lookup"><span data-stu-id="51724-114">--referenceScriptLibraries</span></span> | <span data-ttu-id="51724-115">Dodaje `_ValidationScriptsPartial` to edytowanie i tworzenie stron</span><span class="sxs-lookup"><span data-stu-id="51724-115">Adds `_ValidationScriptsPartial` to Edit and Create pages</span></span> |

<span data-ttu-id="51724-116">Użyj `h` przełącznika, aby uzyskać pomoc dotyczącą `aspnet-codegenerator controller` polecenia:</span><span class="sxs-lookup"><span data-stu-id="51724-116">Use the `h` switch to get help on the `aspnet-codegenerator controller` command:</span></span>

```console
dotnet aspnet-codegenerator controller -h
```
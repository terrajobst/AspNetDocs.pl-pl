---
ms.openlocfilehash: 545448e3673b02abc7e685bd987f2cf5f71375b4
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57074237"
---
> [!WARNING]
> Ze względów bezpieczeństwa należy zgody na uczestnictwo w powiązania `GET` żądanie danych na stronie właściwości modelu. Sprawdź dane wejściowe użytkownika przed mapowania ich właściwości. Włączenie w celu `GET` powiązania jest przydatne w przypadku, gdy adresowania scenariusze, które zależą od wartości trasy lub ciągu zapytania.
>
> Można powiązać właściwości `GET` żądania, ustaw [[BindProperty]](/dotnet/api/microsoft.aspnetcore.mvc.bindpropertyattribute) atrybutu `SupportsGet` właściwości `true`: `[BindProperty(SupportsGet = true)]`

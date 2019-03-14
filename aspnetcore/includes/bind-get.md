---
ms.openlocfilehash: 545448e3673b02abc7e685bd987f2cf5f71375b4
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57074237"
---
> [!WARNING]
> <span data-ttu-id="da8a8-101">Ze względów bezpieczeństwa należy zgody na uczestnictwo w powiązania `GET` żądanie danych na stronie właściwości modelu.</span><span class="sxs-lookup"><span data-stu-id="da8a8-101">For security reasons, you must opt in to binding `GET` request data to page model properties.</span></span> <span data-ttu-id="da8a8-102">Sprawdź dane wejściowe użytkownika przed mapowania ich właściwości.</span><span class="sxs-lookup"><span data-stu-id="da8a8-102">Verify user input before mapping it to properties.</span></span> <span data-ttu-id="da8a8-103">Włączenie w celu `GET` powiązania jest przydatne w przypadku, gdy adresowania scenariusze, które zależą od wartości trasy lub ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="da8a8-103">Opting in to `GET` binding is useful when addressing scenarios which rely on query string or route values.</span></span>
>
> <span data-ttu-id="da8a8-104">Można powiązać właściwości `GET` żądania, ustaw [[BindProperty]](/dotnet/api/microsoft.aspnetcore.mvc.bindpropertyattribute) atrybutu `SupportsGet` właściwości `true`: `[BindProperty(SupportsGet = true)]`</span><span class="sxs-lookup"><span data-stu-id="da8a8-104">To bind a property on `GET` requests, set the [[BindProperty]](/dotnet/api/microsoft.aspnetcore.mvc.bindpropertyattribute) attribute's `SupportsGet` property to `true`: `[BindProperty(SupportsGet = true)]`</span></span>

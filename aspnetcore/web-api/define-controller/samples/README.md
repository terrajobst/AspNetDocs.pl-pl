---
ms.openlocfilehash: b6f39dd0dedb38961eb021cde355f7ec83283195
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57066074"
---
# <a name="aspnet-core-web-api-controller-sample"></a><span data-ttu-id="dcbc4-101">Przykładowe Kontroler interfejsu API sieci Web platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="dcbc4-101">ASP.NET Core Web API Controller Sample</span></span>

<span data-ttu-id="dcbc4-102">Ta przykładowa aplikacja składa się z następujących projektów:</span><span class="sxs-lookup"><span data-stu-id="dcbc4-102">This sample app consists of the following projects:</span></span>

- <span data-ttu-id="dcbc4-103">\**WebApiSample.Api.22*: Projekt platformy ASP.NET Core 2.2, przeznaczonych dla platformy .NET Core 2.2.</span><span class="sxs-lookup"><span data-stu-id="dcbc4-103">\**WebApiSample.Api.22*: An ASP.NET Core 2.2 project targeting .NET Core 2.2.</span></span>
- <span data-ttu-id="dcbc4-104">**WebApiSample.Api.21**: Projekt platformy ASP.NET Core 2.1, przeznaczonych dla platformy .NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="dcbc4-104">**WebApiSample.Api.21**: An ASP.NET Core 2.1 project targeting .NET Core 2.1.</span></span>
- <span data-ttu-id="dcbc4-105">**WebApiSample.Api.Pre21**: Projekt platformy ASP.NET Core 2.0, przeznaczonych dla platformy .NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="dcbc4-105">**WebApiSample.Api.Pre21**: An ASP.NET Core 2.0 project targeting .NET Core 2.0.</span></span>
- <span data-ttu-id="dcbc4-106">**WebApiSample.DataAccess**: Biblioteka klas .NET Standard 2.0 służy jako warstwa dostępu do danych 2 projektów interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="dcbc4-106">**WebApiSample.DataAccess**: A .NET Standard 2.0 class library serving as a data access tier for the 2 Web API projects.</span></span>

<span data-ttu-id="dcbc4-107">Ten przykład ilustruje różnice między tworzenia kontrolera interfejsu API sieci Web:</span><span class="sxs-lookup"><span data-stu-id="dcbc4-107">This sample illustrates variations of Web API controller creation:</span></span>

- [<span data-ttu-id="dcbc4-108">Dziedziczyć klasy ControllerBase</span><span class="sxs-lookup"><span data-stu-id="dcbc4-108">Derive class from ControllerBase</span></span>](https://docs.microsoft.com/aspnet/core/web-api#derive-class-from-controllerbase)
- [<span data-ttu-id="dcbc4-109">Dodawanie adnotacji do klasy za pomocą ApiControllerAttribute</span><span class="sxs-lookup"><span data-stu-id="dcbc4-109">Annotate class with ApiControllerAttribute</span></span>](https://docs.microsoft.com/aspnet/core/web-api#annotate-class-with-apicontrollerattribute)

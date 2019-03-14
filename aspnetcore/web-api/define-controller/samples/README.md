---
ms.openlocfilehash: b6f39dd0dedb38961eb021cde355f7ec83283195
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57066074"
---
# <a name="aspnet-core-web-api-controller-sample"></a>Przykładowe Kontroler interfejsu API sieci Web platformy ASP.NET Core

Ta przykładowa aplikacja składa się z następujących projektów:

- **WebApiSample.Api.22*: Projekt platformy ASP.NET Core 2.2, przeznaczonych dla platformy .NET Core 2.2.
- **WebApiSample.Api.21**: Projekt platformy ASP.NET Core 2.1, przeznaczonych dla platformy .NET Core 2.1.
- **WebApiSample.Api.Pre21**: Projekt platformy ASP.NET Core 2.0, przeznaczonych dla platformy .NET Core 2.0.
- **WebApiSample.DataAccess**: Biblioteka klas .NET Standard 2.0 służy jako warstwa dostępu do danych 2 projektów interfejsu API sieci Web.

Ten przykład ilustruje różnice między tworzenia kontrolera interfejsu API sieci Web:

- [Dziedziczyć klasy ControllerBase](https://docs.microsoft.com/aspnet/core/web-api#derive-class-from-controllerbase)
- [Dodawanie adnotacji do klasy za pomocą ApiControllerAttribute](https://docs.microsoft.com/aspnet/core/web-api#annotate-class-with-apicontrollerattribute)

---
ms.openlocfilehash: 6b2bc386ec179e786de205af0ca6dbd610e000d9
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57075608"
---
# <a name="aspnet-core-background-tasks-sample-generic-host"></a>Próbki (Host ogólnego) zadań w tle programu ASP.NET Core

Ten przykład ilustruje użycie [pomocą interfejsu IHostedService](https://docs.microsoft.com/dotnet/api/microsoft.extensions.hosting.ihostedservice). W tym przykładzie przedstawiono funkcje opisane w [zadania za pomocą usług hostowanych w programie ASP.NET Core w tle](https://docs.microsoft.com/aspnet/core/fundamentals/host/hosted-services) tematu.

Podczas uruchamiania przykładu [programu Visual Studio Code](https://code.visualstudio.com/)ustaw **konsoli** wartość konfiguracji konsoli w *.vscode/launch.json* do jednej `externalTerminal` lub `integratedTerminal`. Korzystanie z `internalConsole` jest niezgodna z danymi wejściowymi naciśnięcia klawisza konsoli przez aplikację do elementów roboczych w tle umieścić w kolejce.

---
ms.openlocfilehash: 6b2bc386ec179e786de205af0ca6dbd610e000d9
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57075608"
---
# <a name="aspnet-core-background-tasks-sample-generic-host"></a><span data-ttu-id="ac0d3-101">Próbki (Host ogólnego) zadań w tle programu ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ac0d3-101">ASP.NET Core Background Tasks Sample (Generic Host)</span></span>

<span data-ttu-id="ac0d3-102">Ten przykład ilustruje użycie [pomocą interfejsu IHostedService](https://docs.microsoft.com/dotnet/api/microsoft.extensions.hosting.ihostedservice).</span><span class="sxs-lookup"><span data-stu-id="ac0d3-102">This sample illustrates the use of [IHostedService](https://docs.microsoft.com/dotnet/api/microsoft.extensions.hosting.ihostedservice).</span></span> <span data-ttu-id="ac0d3-103">W tym przykładzie przedstawiono funkcje opisane w [zadania za pomocą usług hostowanych w programie ASP.NET Core w tle](https://docs.microsoft.com/aspnet/core/fundamentals/host/hosted-services) tematu.</span><span class="sxs-lookup"><span data-stu-id="ac0d3-103">This sample demonstrates the features described in the [Background tasks with hosted services in ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/host/hosted-services) topic.</span></span>

<span data-ttu-id="ac0d3-104">Podczas uruchamiania przykładu [programu Visual Studio Code](https://code.visualstudio.com/)ustaw **konsoli** wartość konfiguracji konsoli w *.vscode/launch.json* do jednej `externalTerminal` lub `integratedTerminal`.</span><span class="sxs-lookup"><span data-stu-id="ac0d3-104">When running the sample in [Visual Studio Code](https://code.visualstudio.com/), set the **console** value of the console configuration in *.vscode/launch.json* to either `externalTerminal` or `integratedTerminal`.</span></span> <span data-ttu-id="ac0d3-105">Korzystanie z `internalConsole` jest niezgodna z danymi wejściowymi naciśnięcia klawisza konsoli przez aplikację do elementów roboczych w tle umieścić w kolejce.</span><span class="sxs-lookup"><span data-stu-id="ac0d3-105">Use of the `internalConsole` is incompatible with console keystroke input that the app uses to enqueue background work items.</span></span>

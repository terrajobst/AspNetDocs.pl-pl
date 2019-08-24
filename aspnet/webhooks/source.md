---
uid: webhooks/source
title: Kod źródłowy i pakiety NuGet ASP.NET webhook | Microsoft Docs
author: rick-anderson
description: Linki do kodu źródłowego i pakietów NuGet ASP.NET webhook
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 91a62bfa-ea3a-41f9-a2e1-e90d2c8fc8ca
ms.openlocfilehash: 8d07848754d9efda9c893b8ba54ac6d0c0214a53
ms.sourcegitcommit: b95316530fa51087d6c400ff91814fe37e73f7e8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/23/2019
ms.locfileid: "70000699"
---
# <a name="aspnet-webhooks-source-code-and-nuget-packages"></a><span data-ttu-id="ee1a6-103">Kod źródłowy i pakiety NuGet ASP.NET webhook</span><span class="sxs-lookup"><span data-stu-id="ee1a6-103">ASP.NET WebHooks source code and NuGet packages</span></span>

<span data-ttu-id="ee1a6-104">Microsoft ASP.NET elementy webhook są częścią Microsoft ASP.NETj rodziny modułów i są hostowane jako [projekt typu open source w witrynie GitHub](https://github.com/aspnet/WebHooks).</span><span class="sxs-lookup"><span data-stu-id="ee1a6-104">Microsoft ASP.NET WebHooks is part of the Microsoft ASP.NET family of modules and is hosted as an [Open Source Project on GitHub](https://github.com/aspnet/WebHooks).</span></span> <span data-ttu-id="ee1a6-105">Oznacza to, że akceptujemy wkłady, ale zapoznaj się z [wytycznymi dotyczącymi udziałów](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) przed przesłaniem żądania ściągnięcia.</span><span class="sxs-lookup"><span data-stu-id="ee1a6-105">This means that we accept contributions, but please look at the [Contribution Guidelines](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) before submitting a pull request.</span></span>

<span data-ttu-id="ee1a6-106">Ta dokumentacja online, którą odczytujesz teraz, jest również hostowana jako [Open Source w serwisie GitHub](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) , a także akceptuje wkłady.</span><span class="sxs-lookup"><span data-stu-id="ee1a6-106">This online documentation which you are reading now is also hosted as [Open Source on GitHub](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) and also accepts contributions.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="ee1a6-107">Pakiety NuGet</span><span class="sxs-lookup"><span data-stu-id="ee1a6-107">NuGet packages</span></span>

<span data-ttu-id="ee1a6-108">[Pakiety NuGet](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) są podzielone na trzy części:</span><span class="sxs-lookup"><span data-stu-id="ee1a6-108">The [NuGet packages](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) are divided into three parts:</span></span>

* <span data-ttu-id="ee1a6-109">[Wspólny](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): Wspólny pakiet współużytkowany przez nadawców i odbiorników.</span><span class="sxs-lookup"><span data-stu-id="ee1a6-109">[Common](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): A common package that is shared between senders and receivers.</span></span>

* <span data-ttu-id="ee1a6-110">[Nadawca](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): Zestaw pakietów obsługujący wysyłanie własnych elementów webhook do innych osób.</span><span class="sxs-lookup"><span data-stu-id="ee1a6-110">[Sender](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): A set of packages supporting sending your own WebHooks to others.</span></span> <span data-ttu-id="ee1a6-111">Funkcje wysyłania elementów webhook zostały szczegółowo opisane w temacie Wysyłanie elementów [webhook](sending/senders.md).</span><span class="sxs-lookup"><span data-stu-id="ee1a6-111">The functionality for sending WebHooks is described in more detail in [Sending WebHooks](sending/senders.md).</span></span>

* <span data-ttu-id="ee1a6-112">[Odbiorcy](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): Zestaw pakietów, które obsługują otrzymywanie elementów webhook od innych użytkowników.</span><span class="sxs-lookup"><span data-stu-id="ee1a6-112">[Receivers](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): A set of packages supporting receiving WebHooks from others.</span></span> <span data-ttu-id="ee1a6-113">Funkcja otrzymywania elementów webhook została szczegółowo opisana w temacie [otrzymywanie](receiving/index.md)elementów webhook.</span><span class="sxs-lookup"><span data-stu-id="ee1a6-113">The functionality for receiving WebHooks is described in more detail in [Receiving WebHooks](receiving/index.md).</span></span>

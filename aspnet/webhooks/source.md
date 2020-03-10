---
uid: webhooks/source
title: Kod źródłowy i pakiety NuGet ASP.NET webhook | Microsoft Docs
author: rick-anderson
description: Linki do kodu źródłowego i pakietów NuGet ASP.NET webhook
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 91a62bfa-ea3a-41f9-a2e1-e90d2c8fc8ca
ms.openlocfilehash: 8d07848754d9efda9c893b8ba54ac6d0c0214a53
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78633064"
---
# <a name="aspnet-webhooks-source-code-and-nuget-packages"></a><span data-ttu-id="8e80c-103">Kod źródłowy i pakiety NuGet ASP.NET webhook</span><span class="sxs-lookup"><span data-stu-id="8e80c-103">ASP.NET WebHooks source code and NuGet packages</span></span>

<span data-ttu-id="8e80c-104">Microsoft ASP.NET elementy webhook są częścią Microsoft ASP.NETj rodziny modułów i są hostowane jako [projekt typu open source w witrynie GitHub](https://github.com/aspnet/WebHooks).</span><span class="sxs-lookup"><span data-stu-id="8e80c-104">Microsoft ASP.NET WebHooks is part of the Microsoft ASP.NET family of modules and is hosted as an [Open Source Project on GitHub](https://github.com/aspnet/WebHooks).</span></span> <span data-ttu-id="8e80c-105">Oznacza to, że akceptujemy wkłady, ale zapoznaj się z [wytycznymi dotyczącymi udziałów](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) przed przesłaniem żądania ściągnięcia.</span><span class="sxs-lookup"><span data-stu-id="8e80c-105">This means that we accept contributions, but please look at the [Contribution Guidelines](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) before submitting a pull request.</span></span>

<span data-ttu-id="8e80c-106">Ta dokumentacja online, którą odczytujesz teraz, jest również hostowana jako [Open Source w serwisie GitHub](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) , a także akceptuje wkłady.</span><span class="sxs-lookup"><span data-stu-id="8e80c-106">This online documentation which you are reading now is also hosted as [Open Source on GitHub](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) and also accepts contributions.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="8e80c-107">Pakiety NuGet</span><span class="sxs-lookup"><span data-stu-id="8e80c-107">NuGet packages</span></span>

<span data-ttu-id="8e80c-108">[Pakiety NuGet](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) są podzielone na trzy części:</span><span class="sxs-lookup"><span data-stu-id="8e80c-108">The [NuGet packages](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) are divided into three parts:</span></span>

* <span data-ttu-id="8e80c-109">[Wspólne](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): wspólny pakiet, który jest współużytkowany przez nadawców i odbiorników.</span><span class="sxs-lookup"><span data-stu-id="8e80c-109">[Common](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): A common package that is shared between senders and receivers.</span></span>

* <span data-ttu-id="8e80c-110">[Nadawca](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): zestaw pakietów obsługujący wysyłanie własnych elementów webhook do innych osób.</span><span class="sxs-lookup"><span data-stu-id="8e80c-110">[Sender](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): A set of packages supporting sending your own WebHooks to others.</span></span> <span data-ttu-id="8e80c-111">Funkcje wysyłania elementów webhook zostały szczegółowo opisane w temacie Wysyłanie elementów [webhook](sending/senders.md).</span><span class="sxs-lookup"><span data-stu-id="8e80c-111">The functionality for sending WebHooks is described in more detail in [Sending WebHooks](sending/senders.md).</span></span>

* <span data-ttu-id="8e80c-112">[Odbiorniki](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): zestaw pakietów obsługujący otrzymywanie elementów webhook od innych użytkowników.</span><span class="sxs-lookup"><span data-stu-id="8e80c-112">[Receivers](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): A set of packages supporting receiving WebHooks from others.</span></span> <span data-ttu-id="8e80c-113">Funkcja otrzymywania elementów webhook została szczegółowo opisana w temacie [otrzymywanie elementów webhook](receiving/index.md).</span><span class="sxs-lookup"><span data-stu-id="8e80c-113">The functionality for receiving WebHooks is described in more detail in [Receiving WebHooks](receiving/index.md).</span></span>

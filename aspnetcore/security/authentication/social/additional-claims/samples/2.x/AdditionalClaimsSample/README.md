---
ms.openlocfilehash: 09cd8e639238b998212b813cba5bc1aa85f36a75
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57072317"
---
# <a name="aspnet-core-external-authentication-provider-additional-claims-sample-aspnet-core-2x"></a><span data-ttu-id="6b8b0-101">Dostawcy uwierzytelniania zewnętrznego platformy ASP.NET Core dodatkowe oświadczenia przykładowe (ASP.NET Core 2.x)</span><span class="sxs-lookup"><span data-stu-id="6b8b0-101">ASP.NET Core External Authentication Provider Additional Claims Sample (ASP.NET Core 2.x)</span></span>

<span data-ttu-id="6b8b0-102">W tym przykładzie pokazano, ustanawiania dodatkowych oświadczeń z danych użytkownika Google przy użyciu dostawcy uwierzytelniania serwisu Google.</span><span class="sxs-lookup"><span data-stu-id="6b8b0-102">This sample illustrates establishing additional claims from Google user data using the Google authentication provider.</span></span> <span data-ttu-id="6b8b0-103">W tym przykładzie towarzyszy [zachować dodatkowe oświadczenia i tokeny od dostawców zewnętrznych](https://docs.microsoft.com/aspnet/core/security/authentication/social/additional-claims).</span><span class="sxs-lookup"><span data-stu-id="6b8b0-103">This sample accompanies [Persist additional claims and tokens from external providers](https://docs.microsoft.com/aspnet/core/security/authentication/social/additional-claims).</span></span>

<span data-ttu-id="6b8b0-104">Aby korzystać z przykładu, należy podać prawidłowy identyfikator klienta Google i wpisu tajnego w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="6b8b0-104">To use the sample, provide a valid Google Client ID and Secret in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="6b8b0-105">Aby uzyskać więcej informacji, zobacz [ustawienia logowania zewnętrznego Google](https://docs.microsoft.com/aspnet/core/security/authentication/social/google-logins).</span><span class="sxs-lookup"><span data-stu-id="6b8b0-105">For more information, see [Google external login setup](https://docs.microsoft.com/aspnet/core/security/authentication/social/google-logins).</span></span>

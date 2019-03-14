---
title: Przykłady uwierzytelniania dla platformy ASP.NET Core
author: rick-anderson
description: Zawiera łącza do przykładów uwierzytelniania w repozytorium platformy ASP.NET Core.
ms.author: riande
ms.date: 01/31/2019
uid: security/authentication/samples
ms.openlocfilehash: 7b3c911d60ad4737ebd12ce6f7628ad624b11658
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57076610"
---
# <a name="authentication-samples-for-aspnet-core"></a><span data-ttu-id="aa3d4-103">Przykłady uwierzytelniania dla platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="aa3d4-103">Authentication samples for ASP.NET Core</span></span>

<span data-ttu-id="aa3d4-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="aa3d4-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="aa3d4-105">[Repozytorium platformy ASP.NET Core](https://github.com/aspnet/AspNetCore) zawiera następujące przykłady uwierzytelniania w *AspNetCore/src/Security/samples* folderu:</span><span class="sxs-lookup"><span data-stu-id="aa3d4-105">The [ASP.NET Core repository](https://github.com/aspnet/AspNetCore) contains the following authentication samples in the *AspNetCore/src/Security/samples* folder:</span></span>

* [<span data-ttu-id="aa3d4-106">Przekształcanie oświadczeń</span><span class="sxs-lookup"><span data-stu-id="aa3d4-106">Claims transformation</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/ClaimsTransformation)
* [<span data-ttu-id="aa3d4-107">Plik cookie uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="aa3d4-107">Cookie authentication</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/Cookies)
* [<span data-ttu-id="aa3d4-108">Dostawca zasad niestandardowych — IAuthorizationPolicyProvider</span><span class="sxs-lookup"><span data-stu-id="aa3d4-108">Custom policy provider - IAuthorizationPolicyProvider</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider)
* [<span data-ttu-id="aa3d4-109">Schematy dynamiczne uwierzytelniania i opcje</span><span class="sxs-lookup"><span data-stu-id="aa3d4-109">Dynamic authentication schemes and options</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/DynamicSchemes)
* [<span data-ttu-id="aa3d4-110">Oświadczeń zewnętrznych</span><span class="sxs-lookup"><span data-stu-id="aa3d4-110">External claims</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/Identity.ExternalClaims)
* [<span data-ttu-id="aa3d4-111">Wybieranie między plik cookie i inny schemat uwierzytelniania na podstawie żądania</span><span class="sxs-lookup"><span data-stu-id="aa3d4-111">Selecting between cookie and another authentication scheme based on the request</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/PathSchemeSelection)
* [<span data-ttu-id="aa3d4-112">Ogranicza dostęp do plików statycznych</span><span class="sxs-lookup"><span data-stu-id="aa3d4-112">Restricts access to static files</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/StaticFilesAuth)

## <a name="run-the-samples"></a><span data-ttu-id="aa3d4-113">Uruchamianie przykładów</span><span class="sxs-lookup"><span data-stu-id="aa3d4-113">Run the samples</span></span>

* <span data-ttu-id="aa3d4-114">Wybierz [gałęzi](https://github.com/aspnet/AspNetCore).</span><span class="sxs-lookup"><span data-stu-id="aa3d4-114">Select a [branch](https://github.com/aspnet/AspNetCore).</span></span> <span data-ttu-id="aa3d4-115">Na przykład:`release/2.2`</span><span class="sxs-lookup"><span data-stu-id="aa3d4-115">For example, `release/2.2`</span></span>
* <span data-ttu-id="aa3d4-116">Klonuj lub Pobierz [repozytorium platformy ASP.NET Core](https://github.com/aspnet/AspNetCore).</span><span class="sxs-lookup"><span data-stu-id="aa3d4-116">Clone or download the [ASP.NET Core repository](https://github.com/aspnet/AspNetCore).</span></span>
* <span data-ttu-id="aa3d4-117">Sprawdź zostały zainstalowane [zestawu .NET Core SDK](https://www.microsoft.com/net/download/all) wersji dopasowania klon repozytorium platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="aa3d4-117">Verify you have installed the [.NET Core SDK](https://www.microsoft.com/net/download/all) version matching the clone of the ASP.NET Core repository.</span></span>
* <span data-ttu-id="aa3d4-118">Przejdź do przykładu w *AspNetCore/src/Security/samples* i uruchamianie aplikacji przykładowej z `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="aa3d4-118">Navigate to a sample in *AspNetCore/src/Security/samples* and run the sample with `dotnet run`.</span></span>

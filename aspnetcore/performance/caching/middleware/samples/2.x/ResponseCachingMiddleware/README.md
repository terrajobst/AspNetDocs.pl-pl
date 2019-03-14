---
ms.openlocfilehash: 93bda587eebc438e5da36b07cb7e4a37df8a91eb
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57077477"
---
# <a name="aspnet-core-response-caching-sample"></a><span data-ttu-id="3f237-101">Przykład buforowanie odpowiedzi platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3f237-101">ASP.NET Core Response Caching Sample</span></span>

<span data-ttu-id="3f237-102">W tym przykładzie pokazano sposób użycia platformy ASP.NET Core [oprogramowanie pośredniczące buforowania odpowiedzi](https://docs.microsoft.com/aspnet/core/performance/caching/middleware).</span><span class="sxs-lookup"><span data-stu-id="3f237-102">This sample illustrates the usage of ASP.NET Core [Response Caching Middleware](https://docs.microsoft.com/aspnet/core/performance/caching/middleware).</span></span>

<span data-ttu-id="3f237-103">Aplikacja odpowiada za pomocą jego stronę indeksu w tym `Cache-Control` nagłówek, aby skonfigurować działanie buforowania.</span><span class="sxs-lookup"><span data-stu-id="3f237-103">The app responds with its Index page, including a `Cache-Control` header to configure caching behavior.</span></span> <span data-ttu-id="3f237-104">Aplikacja ustawia również `Vary` nagłówka, aby skonfigurować pamięć podręczną, aby obsługiwać odpowiedź tylko wtedy, gdy `Accept-Encoding` nagłówka kolejne żądania jest zgodna z wersją z oryginalnego żądania.</span><span class="sxs-lookup"><span data-stu-id="3f237-104">The app also sets the `Vary` header to configure the cache to serve the response only if the `Accept-Encoding` header of subsequent requests matches that from the original request.</span></span>

<span data-ttu-id="3f237-105">Podczas uruchamiania przykładu, strony indeksu są dostarczane z pamięci podręcznej, gdy przechowywanych w pamięci podręcznej i maksymalnie 10 sekund.</span><span class="sxs-lookup"><span data-stu-id="3f237-105">When running the sample, the Index page is served from cache when stored and cached for up to 10 seconds.</span></span>

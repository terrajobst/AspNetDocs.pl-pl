---
ms.openlocfilehash: d7ef9b11af8ee11e4ec84404f8cdeb0b89384a3c
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57130453"
---
## <a name="forward-request-information-with-a-proxy-or-load-balancer"></a><span data-ttu-id="c56e6-101">Do przodu żądają informacji z serwerem proxy lub moduł równoważenia obciążenia</span><span class="sxs-lookup"><span data-stu-id="c56e6-101">Forward request information with a proxy or load balancer</span></span>

<span data-ttu-id="c56e6-102">Jeśli aplikacja jest wdrażana za serwerem proxy lub moduł równoważenia obciążenia, niektóre żądania informacji może być przekazywany do aplikacji w nagłówkach żądania.</span><span class="sxs-lookup"><span data-stu-id="c56e6-102">If the app is deployed behind a proxy server or load balancer, some of the original request information might be forwarded to the app in request headers.</span></span> <span data-ttu-id="c56e6-103">Informacje te obejmują zazwyczaj schemat żądania bezpieczne (`https`), hosta i adresu IP klienta.</span><span class="sxs-lookup"><span data-stu-id="c56e6-103">This information usually includes the secure request scheme (`https`), host, and client IP address.</span></span> <span data-ttu-id="c56e6-104">Aplikacje nie automatycznie odczytać te nagłówki żądania do użytku oryginalne informacje o żądaniu.</span><span class="sxs-lookup"><span data-stu-id="c56e6-104">Apps don't automatically read these request headers to discover and use the original request information.</span></span>

<span data-ttu-id="c56e6-105">Schemat jest używana podczas generowania łącza, który wpływa na przepływ uwierzytelniania przy użyciu dostawców zewnętrznych.</span><span class="sxs-lookup"><span data-stu-id="c56e6-105">The scheme is used in link generation that affects the authentication flow with external providers.</span></span> <span data-ttu-id="c56e6-106">Utraty bezpiecznego schematu (`https`) skutkuje aplikacji generowania adresów URL przekierowania niezabezpieczone niepoprawne.</span><span class="sxs-lookup"><span data-stu-id="c56e6-106">Losing the secure scheme (`https`) results in the app generating incorrect insecure redirect URLs.</span></span>

<span data-ttu-id="c56e6-107">Użyj przekazywane nagłówki oprogramowaniu pośredniczącym, aby udostępnić oryginalne informacje o żądaniu aplikacji na potrzeby przetwarzania żądania.</span><span class="sxs-lookup"><span data-stu-id="c56e6-107">Use Forwarded Headers Middleware to make the original request information available to the app for request processing.</span></span>

<span data-ttu-id="c56e6-108">Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="c56e6-108">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

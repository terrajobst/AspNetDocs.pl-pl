---
ms.openlocfilehash: d7ef9b11af8ee11e4ec84404f8cdeb0b89384a3c
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57130453"
---
## <a name="forward-request-information-with-a-proxy-or-load-balancer"></a>Do przodu żądają informacji z serwerem proxy lub moduł równoważenia obciążenia

Jeśli aplikacja jest wdrażana za serwerem proxy lub moduł równoważenia obciążenia, niektóre żądania informacji może być przekazywany do aplikacji w nagłówkach żądania. Informacje te obejmują zazwyczaj schemat żądania bezpieczne (`https`), hosta i adresu IP klienta. Aplikacje nie automatycznie odczytać te nagłówki żądania do użytku oryginalne informacje o żądaniu.

Schemat jest używana podczas generowania łącza, który wpływa na przepływ uwierzytelniania przy użyciu dostawców zewnętrznych. Utraty bezpiecznego schematu (`https`) skutkuje aplikacji generowania adresów URL przekierowania niezabezpieczone niepoprawne.

Użyj przekazywane nagłówki oprogramowaniu pośredniczącym, aby udostępnić oryginalne informacje o żądaniu aplikacji na potrzeby przetwarzania żądania.

Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/proxy-load-balancer>.

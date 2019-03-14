---
title: Szczegóły uwierzytelnionego szyfrowania w programie ASP.NET Core
author: rick-anderson
description: Szczegóły dotyczące wykonania szyfrowania ochronę danych usługi ASP.NET Core uwierzytelniony.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/authenticated-encryption-details
ms.openlocfilehash: ac650e5c32e7eacc4088225e63f56340f95e1913
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57072959"
---
# <a name="authenticated-encryption-details-in-aspnet-core"></a><span data-ttu-id="04942-103">Szczegóły uwierzytelnionego szyfrowania w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="04942-103">Authenticated encryption details in ASP.NET Core</span></span>

<a name="data-protection-implementation-authenticated-encryption-details"></a>

<span data-ttu-id="04942-104">Wywołania IDataProtector.Protect to operacje uwierzytelnione szyfrowanie.</span><span class="sxs-lookup"><span data-stu-id="04942-104">Calls to IDataProtector.Protect are authenticated encryption operations.</span></span> <span data-ttu-id="04942-105">Metoda Chroń oferuje poufności i autentyczności i jest powiązany łańcuchem cel, który został użyty do wyprowadzenia tego konkretnego wystąpienia interfejsu IDataProtector od głównego IDataProtectionProvider.</span><span class="sxs-lookup"><span data-stu-id="04942-105">The Protect method offers both confidentiality and authenticity, and it's tied to the purpose chain that was used to derive this particular IDataProtector instance from its root IDataProtectionProvider.</span></span>

<span data-ttu-id="04942-106">IDataProtector.Protect przyjmuje parametr byte [] zwykłego tekstu i tworzy byte [] chronionych ładunek, którego format został opisany poniżej.</span><span class="sxs-lookup"><span data-stu-id="04942-106">IDataProtector.Protect takes a byte[] plaintext parameter and produces a byte[] protected payload, whose format is described below.</span></span> <span data-ttu-id="04942-107">(Dostępna jest również przeciążenie metody rozszerzenia, która przyjmuje jako parametr ciągu w postaci zwykłego tekstu i zwraca ładunek chronionych ciągu.</span><span class="sxs-lookup"><span data-stu-id="04942-107">(There's also an extension method overload which takes a string plaintext parameter and returns a string protected payload.</span></span> <span data-ttu-id="04942-108">Jeśli jest używany ten interfejs API nadal będzie miał format ładunku chronionych poniżej struktury, ale będzie [zakodowane w formacie base64url](https://tools.ietf.org/html/rfc4648#section-5).)</span><span class="sxs-lookup"><span data-stu-id="04942-108">If this API is used the protected payload format will still have the below structure, but it will be [base64url-encoded](https://tools.ietf.org/html/rfc4648#section-5).)</span></span>

## <a name="protected-payload-format"></a><span data-ttu-id="04942-109">Format ładunku chronionych</span><span class="sxs-lookup"><span data-stu-id="04942-109">Protected payload format</span></span>

<span data-ttu-id="04942-110">Format ładunku chronionych obejmuje trzy główne składniki:</span><span class="sxs-lookup"><span data-stu-id="04942-110">The protected payload format consists of three primary components:</span></span>

* <span data-ttu-id="04942-111">Nagłówek magic 32-bitowy, który identyfikuje wersję system ochrony danych.</span><span class="sxs-lookup"><span data-stu-id="04942-111">A 32-bit magic header that identifies the version of the data protection system.</span></span>

* <span data-ttu-id="04942-112">128-bitowego klucza identyfikatora, który identyfikuje klucz używany do ochrony tego konkretnego ładunku.</span><span class="sxs-lookup"><span data-stu-id="04942-112">A 128-bit key id that identifies the key used to protect this particular payload.</span></span>

* <span data-ttu-id="04942-113">Pozostała część chronionej ładunku jest [specyficzne dla modułu szyfrującego zamknięte przez ten klucz](xref:security/data-protection/implementation/subkeyderivation#data-protection-implementation-subkey-derivation).</span><span class="sxs-lookup"><span data-stu-id="04942-113">The remainder of the protected payload is [specific to the encryptor encapsulated by this key](xref:security/data-protection/implementation/subkeyderivation#data-protection-implementation-subkey-derivation).</span></span> <span data-ttu-id="04942-114">W poniższym przykładzie reprezentuje klucz AES-256-CBC + modułu szyfrującego HMACSHA256 i ładunku dalej jest podzielona następująco: \* 128-bitowego klucza modyfikator.</span><span class="sxs-lookup"><span data-stu-id="04942-114">In the example below the key represents an AES-256-CBC + HMACSHA256 encryptor, and the payload is further subdivided as follows: \* A 128-bit key modifier.</span></span> <span data-ttu-id="04942-115">\* Wektor inicjowania 128-bitowego.</span><span class="sxs-lookup"><span data-stu-id="04942-115">\* A 128-bit initialization vector.</span></span> <span data-ttu-id="04942-116">\* 48 bajtów danych wyjściowych AES-256-CBC.</span><span class="sxs-lookup"><span data-stu-id="04942-116">\* 48 bytes of AES-256-CBC output.</span></span> <span data-ttu-id="04942-117">\* HMACSHA256 tag uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="04942-117">\* An HMACSHA256 authentication tag.</span></span>

<span data-ttu-id="04942-118">Poniżej przedstawiono przykładowy ładunek chronionych.</span><span class="sxs-lookup"><span data-stu-id="04942-118">A sample protected payload is illustrated below.</span></span>

```
09 F0 C9 F0 80 9C 81 0C 19 66 19 40 95 36 53 F8
AA FF EE 57 57 2F 40 4C 3F 7F CC 9D CC D9 32 3E
84 17 99 16 EC BA 1F 4A A1 18 45 1F 2D 13 7A 28
79 6B 86 9C F8 B7 84 F9 26 31 FC B1 86 0A F1 56
61 CF 14 58 D3 51 6F CF 36 50 85 82 08 2D 3F 73
5F B0 AD 9E 1A B2 AE 13 57 90 C8 F5 7C 95 4E 6A
8A AA 06 EF 43 CA 19 62 84 7C 11 B2 C8 71 9D AA
52 19 2E 5B 4C 1E 54 F0 55 BE 88 92 12 C1 4B 5E
52 C9 74 A0
```

<span data-ttu-id="04942-119">Z format ładunku powyżej pierwsze 32 bity lub 4 bajty są magic nagłówka, identyfikowanie wersji (09 F0 C9 F0)</span><span class="sxs-lookup"><span data-stu-id="04942-119">From the payload format above the first 32 bits, or 4 bytes are the magic header identifying the version (09 F0 C9 F0)</span></span>

<span data-ttu-id="04942-120">Następne 128 bitów lub 16-bajtowy jest identyfikator klucza (80 9C 81 0C 19 66 19 40 95 36 53 F8 AA FF EE 57)</span><span class="sxs-lookup"><span data-stu-id="04942-120">The next 128 bits, or 16 bytes is the key identifier (80 9C 81 0C 19 66 19 40 95 36 53 F8 AA FF EE 57)</span></span>

<span data-ttu-id="04942-121">Pozostała zawiera ładunek i zależy od formatu używanego.</span><span class="sxs-lookup"><span data-stu-id="04942-121">The remainder contains the payload and is specific to the format used.</span></span>

>[!WARNING]
> <span data-ttu-id="04942-122">Wszystkich ładunków chronionych przez podany klucz rozpocznie się za pomocą tego samego nagłówka 20-bajtowy (wartość magic, identyfikator klucza).</span><span class="sxs-lookup"><span data-stu-id="04942-122">All payloads protected to a given key will begin with the same 20-byte (magic value, key id) header.</span></span> <span data-ttu-id="04942-123">Administratorzy mogą używać tego faktu w celach diagnostycznych zbliżenie podczas generowania ładunku.</span><span class="sxs-lookup"><span data-stu-id="04942-123">Administrators can use this fact for diagnostic purposes to approximate when a payload was generated.</span></span> <span data-ttu-id="04942-124">Na przykład ładunku powyżej odnosi się do klucza {0c819c80-6619-4019-9536-53f8aaffee57}.</span><span class="sxs-lookup"><span data-stu-id="04942-124">For example, the payload above corresponds to key {0c819c80-6619-4019-9536-53f8aaffee57}.</span></span> <span data-ttu-id="04942-125">Jeśli po sprawdzeniu klucza repozytorium możesz znaleźć, Data aktywacji tego określonego klucza to 2015-01-01 i daty wygaśnięcia został 2015-03-01, a następnie jest uzasadnione założył, że ładunek (Jeśli nie naruszony) został wygenerowany w ramach tego okna, zapewniają lub wykonaj mały współczynnik grę fudge po obu stronach.</span><span class="sxs-lookup"><span data-stu-id="04942-125">If after checking the key repository you find that this specific key's activation date was 2015-01-01 and its expiration date was 2015-03-01, then it's reasonable to assume that the payload (if not tampered with) was generated within that window, give or take a small fudge factor on either side.</span></span>

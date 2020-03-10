---
uid: web-api/overview/security/integrated-windows-authentication
title: Zintegrowane uwierzytelnianie systemu Windows | Microsoft Docs
author: MikeWasson
description: Opisuje używanie zintegrowanego uwierzytelniania systemu Windows w interfejsie Web API ASP.NET.
ms.author: riande
ms.date: 12/18/2012
ms.assetid: 71ee4c78-c500-4d1c-b761-b4e161a291b5
msc.legacyurl: /web-api/overview/security/integrated-windows-authentication
msc.type: authoredcontent
ms.openlocfilehash: e4f31f191f3c0fabff308ea5dadb0f1d9ce7d448
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78621745"
---
# <a name="integrated-windows-authentication"></a><span data-ttu-id="175ee-103">Zintegrowane uwierzytelnianie systemu Windows</span><span class="sxs-lookup"><span data-stu-id="175ee-103">Integrated Windows Authentication</span></span>

<span data-ttu-id="175ee-104">według [Jan Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="175ee-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="175ee-105">Zintegrowane uwierzytelnianie systemu Windows umożliwia użytkownikom logowanie się za pomocą poświadczeń systemu Windows przy użyciu protokołu Kerberos lub NTLM.</span><span class="sxs-lookup"><span data-stu-id="175ee-105">Integrated Windows authentication enables users to log in with their Windows credentials, using Kerberos or NTLM.</span></span> <span data-ttu-id="175ee-106">Klient wysyła poświadczenia w nagłówku autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="175ee-106">The client sends credentials in the Authorization header.</span></span> <span data-ttu-id="175ee-107">Uwierzytelnianie systemu Windows jest najlepiej dostosowane do środowiska intranetowego.</span><span class="sxs-lookup"><span data-stu-id="175ee-107">Windows authentication is best suited for an intranet environment.</span></span> <span data-ttu-id="175ee-108">Aby uzyskać więcej informacji, zobacz [uwierzytelnianie systemu Windows](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication).</span><span class="sxs-lookup"><span data-stu-id="175ee-108">For more information, see [Windows Authentication](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication).</span></span>

| <span data-ttu-id="175ee-109">Zalety</span><span class="sxs-lookup"><span data-stu-id="175ee-109">Advantages</span></span> | <span data-ttu-id="175ee-110">Wady</span><span class="sxs-lookup"><span data-stu-id="175ee-110">Disadvantages</span></span> |
| --- | --- |
| <span data-ttu-id="175ee-111">— Wbudowana w usługi IIS.</span><span class="sxs-lookup"><span data-stu-id="175ee-111">- Built into IIS.</span></span> <span data-ttu-id="175ee-112">-Nie wysyła poświadczeń użytkownika w żądaniu.</span><span class="sxs-lookup"><span data-stu-id="175ee-112">- Does not send the user credentials in the request.</span></span> <span data-ttu-id="175ee-113">— Jeśli komputer kliencki należy do domeny (na przykład aplikacja intranetowa), użytkownik nie musi wprowadzać poświadczeń.</span><span class="sxs-lookup"><span data-stu-id="175ee-113">- If the client computer belongs to the domain (for example, intranet application), the user does not need to enter credentials.</span></span> | <span data-ttu-id="175ee-114">— Nie jest to zalecane w przypadku aplikacji internetowych.</span><span class="sxs-lookup"><span data-stu-id="175ee-114">- Not recommended for Internet applications.</span></span> <span data-ttu-id="175ee-115">-Wymaga obsługi protokołu Kerberos lub NTLM na kliencie.</span><span class="sxs-lookup"><span data-stu-id="175ee-115">- Requires Kerberos or NTLM support in the client.</span></span> <span data-ttu-id="175ee-116">-Klient musi znajdować się w domenie Active Directory.</span><span class="sxs-lookup"><span data-stu-id="175ee-116">- Client must be in the Active Directory domain.</span></span> |

> [!NOTE]
> <span data-ttu-id="175ee-117">Jeśli aplikacja jest hostowana na platformie Azure i masz lokalną domenę Active Directory, rozważ federowanie swojej lokalnej usługi AD z Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="175ee-117">If your application is hosted on Azure and you have an on-premise Active Directory domain, consider federating your on-premise AD with Azure Active Directory.</span></span> <span data-ttu-id="175ee-118">Dzięki temu użytkownicy mogą logować się przy użyciu poświadczeń lokalnych, ale uwierzytelnianie jest wykonywane przez usługę Azure AD.</span><span class="sxs-lookup"><span data-stu-id="175ee-118">That way, users can log in with their on-premise credentials, but the authentication is performed by Azure AD.</span></span> <span data-ttu-id="175ee-119">Aby uzyskać więcej informacji, zobacz [uwierzytelnianie na platformie Azure](../../../visual-studio/overview/2012/windows-azure-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="175ee-119">For more information, see [Azure Authentication](../../../visual-studio/overview/2012/windows-azure-authentication.md).</span></span>

<span data-ttu-id="175ee-120">Aby utworzyć aplikację, która używa zintegrowanego uwierzytelniania systemu Windows, wybierz szablon "aplikacja intranetowa" w Kreatorze projektu MVC 4.</span><span class="sxs-lookup"><span data-stu-id="175ee-120">To create an application that uses Integrated Windows authentication, select the "Intranet Application" template in the MVC 4 project wizard.</span></span> <span data-ttu-id="175ee-121">Ten szablon projektu umieszcza następujące ustawienie w pliku Web. config:</span><span class="sxs-lookup"><span data-stu-id="175ee-121">This project template puts the following setting in the Web.config file:</span></span>

[!code-xml[Main](integrated-windows-authentication/samples/sample1.xml)]

<span data-ttu-id="175ee-122">Po stronie klienta zintegrowane uwierzytelnianie systemu Windows działa z każdą przeglądarką obsługującą schemat uwierzytelniania [negocjowanego](http://www.ietf.org/rfc/rfc4559.txt) , która obejmuje większość najważniejszych przeglądarek.</span><span class="sxs-lookup"><span data-stu-id="175ee-122">On the client side, Integrated Windows authentication works with any browser that supports the [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) authentication scheme, which includes most major browsers.</span></span> <span data-ttu-id="175ee-123">W przypadku aplikacji klienckich platformy .NET Klasa **HttpClient** obsługuje uwierzytelnianie systemu Windows:</span><span class="sxs-lookup"><span data-stu-id="175ee-123">For .NET client applications, the **HttpClient** class supports Windows authentication:</span></span>

[!code-csharp[Main](integrated-windows-authentication/samples/sample2.cs)]

<span data-ttu-id="175ee-124">Uwierzytelnianie systemu Windows jest podatne na ataki w ramach fałszerstwa żądań między lokacjami (CSRF).</span><span class="sxs-lookup"><span data-stu-id="175ee-124">Windows authentication is vulnerable to cross-site request forgery (CSRF) attacks.</span></span> <span data-ttu-id="175ee-125">Zapoznaj się z artykułem [zapobieganie atakom CSRFym między witrynami](preventing-cross-site-request-forgery-csrf-attacks.md).</span><span class="sxs-lookup"><span data-stu-id="175ee-125">See [Preventing Cross-Site Request Forgery (CSRF) Attacks](preventing-cross-site-request-forgery-csrf-attacks.md).</span></span>

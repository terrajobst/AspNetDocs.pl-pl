---
uid: web-api/overview/security/integrated-windows-authentication
title: Zintegrowane uwierzytelnianie Windows | Dokumentacja firmy Microsoft
author: MikeWasson
description: W tym artykule opisano, w interfejsie API sieci Web platformy ASP.NET przy użyciu zintegrowanego uwierzytelniania Windows.
ms.author: riande
ms.date: 12/18/2012
ms.assetid: 71ee4c78-c500-4d1c-b761-b4e161a291b5
msc.legacyurl: /web-api/overview/security/integrated-windows-authentication
msc.type: authoredcontent
ms.openlocfilehash: e4f31f191f3c0fabff308ea5dadb0f1d9ce7d448
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65125201"
---
# <a name="integrated-windows-authentication"></a><span data-ttu-id="99f04-103">Zintegrowane uwierzytelnianie systemu Windows</span><span class="sxs-lookup"><span data-stu-id="99f04-103">Integrated Windows Authentication</span></span>

<span data-ttu-id="99f04-104">przez [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="99f04-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="99f04-105">Zintegrowane uwierzytelnianie Windows umożliwia użytkownikom logowanie się przy użyciu poświadczeń Windows przy użyciu protokołu Kerberos lub NTLM.</span><span class="sxs-lookup"><span data-stu-id="99f04-105">Integrated Windows authentication enables users to log in with their Windows credentials, using Kerberos or NTLM.</span></span> <span data-ttu-id="99f04-106">Klient wysyła poświadczenia w nagłówku autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="99f04-106">The client sends credentials in the Authorization header.</span></span> <span data-ttu-id="99f04-107">Uwierzytelnianie Windows jest najlepszym rozwiązaniem w przypadku środowisku sieci intranet.</span><span class="sxs-lookup"><span data-stu-id="99f04-107">Windows authentication is best suited for an intranet environment.</span></span> <span data-ttu-id="99f04-108">Aby uzyskać więcej informacji, zobacz [uwierzytelniania Windows](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication).</span><span class="sxs-lookup"><span data-stu-id="99f04-108">For more information, see [Windows Authentication](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication).</span></span>

| <span data-ttu-id="99f04-109">Zalety</span><span class="sxs-lookup"><span data-stu-id="99f04-109">Advantages</span></span> | <span data-ttu-id="99f04-110">Wady</span><span class="sxs-lookup"><span data-stu-id="99f04-110">Disadvantages</span></span> |
| --- | --- |
| <span data-ttu-id="99f04-111">-Wbudowane w usługach IIS.</span><span class="sxs-lookup"><span data-stu-id="99f04-111">- Built into IIS.</span></span> <span data-ttu-id="99f04-112">-Nie wysyła poświadczenia użytkownika w żądaniu.</span><span class="sxs-lookup"><span data-stu-id="99f04-112">- Does not send the user credentials in the request.</span></span> <span data-ttu-id="99f04-113">— Jeśli komputer klienta należy do domeny (na przykład aplikacja intranetu), użytkownik musi wprowadzić poświadczenia.</span><span class="sxs-lookup"><span data-stu-id="99f04-113">- If the client computer belongs to the domain (for example, intranet application), the user does not need to enter credentials.</span></span> | <span data-ttu-id="99f04-114">— Nie jest zalecane dla aplikacji internetowych.</span><span class="sxs-lookup"><span data-stu-id="99f04-114">- Not recommended for Internet applications.</span></span> <span data-ttu-id="99f04-115">— Wymagają obsługi protokołu Kerberos lub NTLM w kliencie.</span><span class="sxs-lookup"><span data-stu-id="99f04-115">- Requires Kerberos or NTLM support in the client.</span></span> <span data-ttu-id="99f04-116">-Klient musi mieć w domenie usługi Active Directory.</span><span class="sxs-lookup"><span data-stu-id="99f04-116">- Client must be in the Active Directory domain.</span></span> |

> [!NOTE]
> <span data-ttu-id="99f04-117">Jeśli aplikacja jest hostowana na platformie Azure i masz domenę usługi Active Directory w środowisku lokalnym, należy wziąć pod uwagę federowanie usługi AD w środowisku lokalnym za pomocą usługi Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="99f04-117">If your application is hosted on Azure and you have an on-premise Active Directory domain, consider federating your on-premise AD with Azure Active Directory.</span></span> <span data-ttu-id="99f04-118">Dzięki temu użytkownicy mogą zalogować się przy użyciu swoich poświadczeń w środowisku lokalnym, ale uwierzytelnianie jest wykonywane przez usługę Azure AD.</span><span class="sxs-lookup"><span data-stu-id="99f04-118">That way, users can log in with their on-premise credentials, but the authentication is performed by Azure AD.</span></span> <span data-ttu-id="99f04-119">Aby uzyskać więcej informacji, zobacz [uwierzytelnianie w usłudze Azure](../../../visual-studio/overview/2012/windows-azure-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="99f04-119">For more information, see [Azure Authentication](../../../visual-studio/overview/2012/windows-azure-authentication.md).</span></span>

<span data-ttu-id="99f04-120">Aby utworzyć aplikację, która korzysta z uwierzytelniania zintegrowanego Windows, wybierz szablon "Aplikacja intranetu" w Kreatorze projektu MVC 4.</span><span class="sxs-lookup"><span data-stu-id="99f04-120">To create an application that uses Integrated Windows authentication, select the "Intranet Application" template in the MVC 4 project wizard.</span></span> <span data-ttu-id="99f04-121">Ten szablon projektu umieszcza następujące ustawienie w pliku Web.config:</span><span class="sxs-lookup"><span data-stu-id="99f04-121">This project template puts the following setting in the Web.config file:</span></span>

[!code-xml[Main](integrated-windows-authentication/samples/sample1.xml)]

<span data-ttu-id="99f04-122">Po stronie klienta uwierzytelnianie zintegrowane Windows działa z dowolnej przeglądarki obsługującej [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) schematu uwierzytelniania, która zawiera większości popularnych przeglądarek.</span><span class="sxs-lookup"><span data-stu-id="99f04-122">On the client side, Integrated Windows authentication works with any browser that supports the [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) authentication scheme, which includes most major browsers.</span></span> <span data-ttu-id="99f04-123">Dla aplikacji klienckich, .NET **HttpClient** klasy obsługuje uwierzytelnianie Windows:</span><span class="sxs-lookup"><span data-stu-id="99f04-123">For .NET client applications, the **HttpClient** class supports Windows authentication:</span></span>

[!code-csharp[Main](integrated-windows-authentication/samples/sample2.cs)]

<span data-ttu-id="99f04-124">Uwierzytelnianie Windows jest narażony na fałszerstwo żądania międzywitrynowego (CSRF) ataków.</span><span class="sxs-lookup"><span data-stu-id="99f04-124">Windows authentication is vulnerable to cross-site request forgery (CSRF) attacks.</span></span> <span data-ttu-id="99f04-125">Zobacz [zapobieganie atakom na fałszerstwo żądania Międzywitrynowego (CSRF)](preventing-cross-site-request-forgery-csrf-attacks.md).</span><span class="sxs-lookup"><span data-stu-id="99f04-125">See [Preventing Cross-Site Request Forgery (CSRF) Attacks](preventing-cross-site-request-forgery-csrf-attacks.md).</span></span>

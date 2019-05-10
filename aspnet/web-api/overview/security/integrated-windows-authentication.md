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
# <a name="integrated-windows-authentication"></a>Zintegrowane uwierzytelnianie systemu Windows

przez [Mike Wasson](https://github.com/MikeWasson)

Zintegrowane uwierzytelnianie Windows umożliwia użytkownikom logowanie się przy użyciu poświadczeń Windows przy użyciu protokołu Kerberos lub NTLM. Klient wysyła poświadczenia w nagłówku autoryzacji. Uwierzytelnianie Windows jest najlepszym rozwiązaniem w przypadku środowisku sieci intranet. Aby uzyskać więcej informacji, zobacz [uwierzytelniania Windows](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication).

| Zalety | Wady |
| --- | --- |
| -Wbudowane w usługach IIS. -Nie wysyła poświadczenia użytkownika w żądaniu. — Jeśli komputer klienta należy do domeny (na przykład aplikacja intranetu), użytkownik musi wprowadzić poświadczenia. | — Nie jest zalecane dla aplikacji internetowych. — Wymagają obsługi protokołu Kerberos lub NTLM w kliencie. -Klient musi mieć w domenie usługi Active Directory. |

> [!NOTE]
> Jeśli aplikacja jest hostowana na platformie Azure i masz domenę usługi Active Directory w środowisku lokalnym, należy wziąć pod uwagę federowanie usługi AD w środowisku lokalnym za pomocą usługi Azure Active Directory. Dzięki temu użytkownicy mogą zalogować się przy użyciu swoich poświadczeń w środowisku lokalnym, ale uwierzytelnianie jest wykonywane przez usługę Azure AD. Aby uzyskać więcej informacji, zobacz [uwierzytelnianie w usłudze Azure](../../../visual-studio/overview/2012/windows-azure-authentication.md).

Aby utworzyć aplikację, która korzysta z uwierzytelniania zintegrowanego Windows, wybierz szablon "Aplikacja intranetu" w Kreatorze projektu MVC 4. Ten szablon projektu umieszcza następujące ustawienie w pliku Web.config:

[!code-xml[Main](integrated-windows-authentication/samples/sample1.xml)]

Po stronie klienta uwierzytelnianie zintegrowane Windows działa z dowolnej przeglądarki obsługującej [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) schematu uwierzytelniania, która zawiera większości popularnych przeglądarek. Dla aplikacji klienckich, .NET **HttpClient** klasy obsługuje uwierzytelnianie Windows:

[!code-csharp[Main](integrated-windows-authentication/samples/sample2.cs)]

Uwierzytelnianie Windows jest narażony na fałszerstwo żądania międzywitrynowego (CSRF) ataków. Zobacz [zapobieganie atakom na fałszerstwo żądania Międzywitrynowego (CSRF)](preventing-cross-site-request-forgery-csrf-attacks.md).

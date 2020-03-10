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
# <a name="integrated-windows-authentication"></a>Zintegrowane uwierzytelnianie systemu Windows

według [Jan Wasson](https://github.com/MikeWasson)

Zintegrowane uwierzytelnianie systemu Windows umożliwia użytkownikom logowanie się za pomocą poświadczeń systemu Windows przy użyciu protokołu Kerberos lub NTLM. Klient wysyła poświadczenia w nagłówku autoryzacji. Uwierzytelnianie systemu Windows jest najlepiej dostosowane do środowiska intranetowego. Aby uzyskać więcej informacji, zobacz [uwierzytelnianie systemu Windows](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication).

| Zalety | Wady |
| --- | --- |
| — Wbudowana w usługi IIS. -Nie wysyła poświadczeń użytkownika w żądaniu. — Jeśli komputer kliencki należy do domeny (na przykład aplikacja intranetowa), użytkownik nie musi wprowadzać poświadczeń. | — Nie jest to zalecane w przypadku aplikacji internetowych. -Wymaga obsługi protokołu Kerberos lub NTLM na kliencie. -Klient musi znajdować się w domenie Active Directory. |

> [!NOTE]
> Jeśli aplikacja jest hostowana na platformie Azure i masz lokalną domenę Active Directory, rozważ federowanie swojej lokalnej usługi AD z Azure Active Directory. Dzięki temu użytkownicy mogą logować się przy użyciu poświadczeń lokalnych, ale uwierzytelnianie jest wykonywane przez usługę Azure AD. Aby uzyskać więcej informacji, zobacz [uwierzytelnianie na platformie Azure](../../../visual-studio/overview/2012/windows-azure-authentication.md).

Aby utworzyć aplikację, która używa zintegrowanego uwierzytelniania systemu Windows, wybierz szablon "aplikacja intranetowa" w Kreatorze projektu MVC 4. Ten szablon projektu umieszcza następujące ustawienie w pliku Web. config:

[!code-xml[Main](integrated-windows-authentication/samples/sample1.xml)]

Po stronie klienta zintegrowane uwierzytelnianie systemu Windows działa z każdą przeglądarką obsługującą schemat uwierzytelniania [negocjowanego](http://www.ietf.org/rfc/rfc4559.txt) , która obejmuje większość najważniejszych przeglądarek. W przypadku aplikacji klienckich platformy .NET Klasa **HttpClient** obsługuje uwierzytelnianie systemu Windows:

[!code-csharp[Main](integrated-windows-authentication/samples/sample2.cs)]

Uwierzytelnianie systemu Windows jest podatne na ataki w ramach fałszerstwa żądań między lokacjami (CSRF). Zapoznaj się z artykułem [zapobieganie atakom CSRFym między witrynami](preventing-cross-site-request-forgery-csrf-attacks.md).

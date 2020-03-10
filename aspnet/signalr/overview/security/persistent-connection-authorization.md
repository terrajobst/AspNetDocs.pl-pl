---
uid: signalr/overview/security/persistent-connection-authorization
title: Uwierzytelnianie i autoryzacja dla połączeń trwałych sygnalizujących | Microsoft Docs
author: bradygaster
description: W tym temacie opisano, jak wymusić autoryzację dla trwałego połączenia. Aby uzyskać ogólne informacje na temat integrowania zabezpieczeń z aplikacją sygnalizującą,...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: e264677b-9c01-47ec-94f9-3cd8f08f94af
msc.legacyurl: /signalr/overview/security/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: 7e69d3c1a18f1239142891734ba58cd2b0078f84
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78578877"
---
# <a name="authentication-and-authorization-for-signalr-persistent-connections"></a>Uwierzytelnianie i autoryzacja połączeń trwałych usługi SignalR

[Fletcher Patryk](https://github.com/pfletcher), [Tomasz FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> W tym temacie opisano, jak wymusić autoryzację dla trwałego połączenia. Aby uzyskać ogólne informacje o integrowaniu zabezpieczeń z aplikacją sygnalizującą, zobacz [wprowadzenie do zabezpieczeń](introduction-to-security.md).
>
> ## <a name="software-versions-used-in-this-topic"></a>Wersje oprogramowania używane w tym temacie
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - Sygnalizujący wersja 2
>
>
>
> ## <a name="previous-versions-of-this-topic"></a>Poprzednie wersje tego tematu
>
> Aby uzyskać informacje o wcześniejszych wersjach programu sygnalizującego, zobacz sekcję [sygnalizujące starsze wersje](../older-versions/index.md).
>
> ## <a name="questions-and-comments"></a>Pytania i Komentarze
>
> Prosimy o opinię na temat sposobu, w jaki lubię ten samouczek, i co możemy ulepszyć w komentarzach w dolnej części strony. Jeśli masz pytania, które nie są bezpośrednio związane z samouczkiem, możesz je ogłosić na [forum ASP.NET](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).

## <a name="enforce-authorization"></a>Wymuś autoryzację

Aby wymusić reguły autoryzacji podczas korzystania z [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) , należy zastąpić metodę `AuthorizeRequest`. Nie można użyć atrybutu `Authorize` z połączeniami trwałymi. Metoda `AuthorizeRequest` jest wywoływana przez środowisko sygnalizujące przed każdym żądaniem, aby sprawdzić, czy użytkownik ma uprawnienia do wykonywania żądanych akcji. Metoda `AuthorizeRequest` nie jest wywoływana z klienta; Zamiast tego użytkownik uwierzytelnia użytkownika za pomocą standardowego mechanizmu uwierzytelniania aplikacji.

W poniższym przykładzie pokazano, jak ograniczyć liczbę żądań do uwierzytelnionych użytkowników.

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

Dowolna dostosowana logika autoryzacji można dodać w metodzie AuthorizeRequest. na przykład sprawdzanie, czy użytkownik należy do określonej roli.

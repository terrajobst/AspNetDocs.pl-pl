---
uid: signalr/overview/older-versions/persistent-connection-authorization
title: Uwierzytelnianie i autoryzacja dla połączeń trwałych sygnalizujących (sygnał 1. x) | Microsoft Docs
author: bradygaster
description: W tym temacie opisano, jak wymusić autoryzację dla trwałego połączenia. Aby uzyskać ogólne informacje na temat integrowania zabezpieczeń z aplikacją sygnalizującą,...
ms.author: bradyg
ms.date: 10/21/2013
ms.assetid: c34bc627-41af-4c21-a817-e97a19a7f252
msc.legacyurl: /signalr/overview/older-versions/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: 9ccc59e3ea502daf12ce82382ab30ca73ca0f9b5
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536541"
---
# <a name="authentication-and-authorization-for-signalr-persistent-connections-signalr-1x"></a>Uwierzytelnianie i autoryzacja połączeń trwałych usługi SignalR (SignalR 1.x)

[Fletcher Patryk](https://github.com/pfletcher), [Tomasz FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> W tym temacie opisano, jak wymusić autoryzację dla trwałego połączenia. Aby uzyskać ogólne informacje o integrowaniu zabezpieczeń z aplikacją sygnalizującą, zobacz [wprowadzenie do zabezpieczeń](index.md).

## <a name="enforce-authorization"></a>Wymuś autoryzację

Aby wymusić reguły autoryzacji podczas korzystania z [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) , należy zastąpić metodę `AuthorizeRequest`. Nie można użyć atrybutu `Authorize` z połączeniami trwałymi. Metoda `AuthorizeRequest` jest wywoływana przez środowisko sygnalizujące przed każdym żądaniem, aby sprawdzić, czy użytkownik ma uprawnienia do wykonywania żądanych akcji. Metoda `AuthorizeRequest` nie jest wywoływana z klienta; Zamiast tego użytkownik uwierzytelnia użytkownika za pomocą standardowego mechanizmu uwierzytelniania aplikacji.

W poniższym przykładzie pokazano, jak ograniczyć liczbę żądań do uwierzytelnionych użytkowników.

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

Dowolna dostosowana logika autoryzacji można dodać w metodzie AuthorizeRequest. na przykład sprawdzanie, czy użytkownik należy do określonej roli.

---
uid: aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server
title: Serwer autoryzacji OWIN OAuth 2,0 | Microsoft Docs
author: hongyes
description: Ten samouczek przeprowadzi Cię przez proces wdrażania serwera autoryzacji protokołu OAuth 2,0 przy użyciu oprogramowania pośredniczącego OWIN OAuth. Jest to zaawansowany samouczek, który outlin...
ms.author: riande
ms.date: 01/28/2019
ms.assetid: 20acee16-c70c-41e9-b38f-92bfcf9a4c1c
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server
msc.type: authoredcontent
ms.openlocfilehash: 39c78ee57f791a94af7a5fb66c3ac42d7239760f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78617111"
---
# <a name="owin-oauth-20-authorization-server"></a>Serwer autoryzacji OAuth 2.0 interfejsu OWIN

[Platforma OAuth 2,0](http://tools.ietf.org/html/rfc6749) umożliwia aplikacji innej firmy uzyskanie ograniczonego dostępu do usługi http. Zamiast używać poświadczeń właściciela zasobu do uzyskiwania dostępu do chronionego zasobu, klient uzyskuje token dostępu (który jest ciągiem wskazującym określony zakres, okres istnienia i inne atrybuty dostępu). Tokeny dostępu są wystawiane dla klientów innych firm przez serwer autoryzacji z zatwierdzeniem właściciela zasobu.

OWIN definiuje standardowy interfejs między serwerami sieci Web .NET i aplikacjami sieci Web. Celem interfejsu OWIN jest rozdzielenie serwera i aplikacji, zachęcanie do tworzenia prostych modułów do programowania w sieci Web dla platformy .NET, a przez to jest otwarty standard, pobudzający ekosystem typu open source narzędzi programistycznych sieci Web platformy .NET.

Aby uzyskać więcej informacji, zobacz [Owin](http://owin.org/)

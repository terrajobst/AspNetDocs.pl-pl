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
# <a name="owin-oauth-20-authorization-server"></a><span data-ttu-id="4a08f-104">Serwer autoryzacji OAuth 2.0 interfejsu OWIN</span><span class="sxs-lookup"><span data-stu-id="4a08f-104">OWIN OAuth 2.0 Authorization Server</span></span>

<span data-ttu-id="4a08f-105">[Platforma OAuth 2,0](http://tools.ietf.org/html/rfc6749) umożliwia aplikacji innej firmy uzyskanie ograniczonego dostępu do usługi http.</span><span class="sxs-lookup"><span data-stu-id="4a08f-105">The [OAuth 2.0 framework](http://tools.ietf.org/html/rfc6749) enables a third-party app to obtain limited access to an HTTP service.</span></span> <span data-ttu-id="4a08f-106">Zamiast używać poświadczeń właściciela zasobu do uzyskiwania dostępu do chronionego zasobu, klient uzyskuje token dostępu (który jest ciągiem wskazującym określony zakres, okres istnienia i inne atrybuty dostępu).</span><span class="sxs-lookup"><span data-stu-id="4a08f-106">Instead of using the resource owner's credentials to access a protected resource, the client obtains an access token (which is a string denoting a specific scope, lifetime, and other access attributes).</span></span> <span data-ttu-id="4a08f-107">Tokeny dostępu są wystawiane dla klientów innych firm przez serwer autoryzacji z zatwierdzeniem właściciela zasobu.</span><span class="sxs-lookup"><span data-stu-id="4a08f-107">Access tokens are issued to third-party clients by an authorization server with the approval of the resource owner.</span></span>

<span data-ttu-id="4a08f-108">OWIN definiuje standardowy interfejs między serwerami sieci Web .NET i aplikacjami sieci Web.</span><span class="sxs-lookup"><span data-stu-id="4a08f-108">OWIN defines a standard interface between .NET web servers and web applications.</span></span> <span data-ttu-id="4a08f-109">Celem interfejsu OWIN jest rozdzielenie serwera i aplikacji, zachęcanie do tworzenia prostych modułów do programowania w sieci Web dla platformy .NET, a przez to jest otwarty standard, pobudzający ekosystem typu open source narzędzi programistycznych sieci Web platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="4a08f-109">The goal of the OWIN interface is to decouple server and application, encourage the development of simple modules for .NET web development, and, by being an open standard, stimulate the open source ecosystem of .NET web development tools.</span></span>

<span data-ttu-id="4a08f-110">Aby uzyskać więcej informacji, zobacz [Owin](http://owin.org/)</span><span class="sxs-lookup"><span data-stu-id="4a08f-110">For more information, see [OWIN](http://owin.org/)</span></span>

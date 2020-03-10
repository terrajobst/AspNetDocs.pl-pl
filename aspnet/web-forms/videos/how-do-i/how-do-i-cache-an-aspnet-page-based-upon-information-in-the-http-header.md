---
uid: web-forms/videos/how-do-i/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header
title: '[Jak:]  Buforowanie strony ASP.NET na podstawie informacji z nagłówka HTTP | Microsoft Docs'
author: rick-anderson
description: W tym filmie wideo Krzysztof pikseli pokazuje, jak zachować stronę w wyjściowej pamięci podręcznej ASP.NET na podstawie informacji w nagłówku HTTP strony. Najpierw potencjalny HEA HTTP...
ms.author: riande
ms.date: 02/26/2009
ms.assetid: 0f8df1bd-080a-4eeb-980c-c2fbb05d30c2
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header
msc.type: video
ms.openlocfilehash: 79c27f39793a4a3a94ea412838fb3844579e874d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78624972"
---
# <a name="how-do-i--cache-an-aspnet-page-based-upon-information-in-the-http-header"></a>[Jak:]  Buforowanie strony ASP.NET na podstawie informacji w nagłówku HTTP

[Krzysztof pikseli](https://twitter.com/chrispels)

W tym filmie wideo Krzysztof pikseli pokazuje, jak zachować stronę w wyjściowej pamięci podręcznej ASP.NET na podstawie informacji w nagłówku HTTP strony. Najpierw są analizowane potencjalne wartości nagłówka HTTP. Następnie zostanie utworzona Przykładowa strona, a następnie dyrektywa OutputCache jest używana z atrybutem VaryByHeader, który zawiera wartość "Accept-Language", nagłówka HTTP, aby kontrolować buforowanie na podstawie języka przeglądarki użytkownika. Przykładowa strona jest wyświetlana w programie IE, która jest ustawiona na język angielski, a następnie w przeglądarce FireFox, która jest ustawiona do korzystania z języka francuskiego. Na koniec należy omówić opcję przenoszenia definicji pamięci podręcznej do CacheProfile w pliku Web. config.

[&#9654;Obejrzyj wideo (12 minut)](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header)

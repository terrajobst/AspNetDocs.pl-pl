---
uid: web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
title: '[Jak mogę] Zastąp HTML na stronie ASP.NET za pomocą właściwości Reponse.Filter | Dokumentacja firmy Microsoft'
author: rick-anderson
description: W tym wideo pikseli Chris pokazuje, jak za pomocą właściwości Reponse.Filter przechwycenia i alter HTML są wysyłane do strony. Po pierwsze przykładowa strona jest tworzony w...
ms.author: riande
ms.date: 01/29/2009
ms.assetid: 3e5ae74a-9798-47d8-a2b3-0d8ad42dd4bc
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
msc.type: video
ms.openlocfilehash: 2ebd9162f81f5270c92c6b8d55e2d2dad4660701
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/17/2019
ms.locfileid: "59403431"
---
# <a name="how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page"></a>[Jak mogę] Zastąp HTML na stronie ASP.NET za pomocą właściwości Reponse.Filter

przez [Chris pikseli](https://twitter.com/chrispels)

W tym wideo pikseli Chris pokazuje, jak za pomocą właściwości Reponse.Filter przechwycenia i alter HTML są wysyłane do strony. Przykładowa strona jest utworzenia z tekstem proste. Następnie niestandardowej klasy Stream jest tworzony, który służy jako strumień zastąpienia dla bieżącego strumienia, które są wysyłane do przeglądarki użytkownika. W tej klasie niestandardowe strumienia zawartość strony są pobierane ze strumienia, zmieniona, a następnie zapisywane do strumienia odpowiedzi. W tym niestandardowej klasy Stream metody zapisu jest dostosowany do Zastąp kod HTML w strumienia odpowiedzi podstawowego, a tym samym zmiany, co jest wysyłane do przeglądarki użytkownika. Na koniec nową klasę strumienia jest przypisany do właściwości Response.Filter na stronie\_zdarzeniem ładowania, a tym samym, zapewniając mechanizm zmiany zawartości strony.

[&#9654;Obejrzyj film wideo (minuty 13)](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page)

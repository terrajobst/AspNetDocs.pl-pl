---
uid: web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
title: '[Jak:] Użyj właściwości odpowiedzi. Filter, aby zastąpić kod HTML na stronie ASP.NET | Microsoft Docs'
author: rick-anderson
description: W tym filmie wideo Krzysztof pikseli pokazuje, jak używać właściwości odpowiedzi. Filter, aby przechwycić i zmienić kod HTML wysyłany do strony. Najpierw zostanie utworzona Przykładowa strona w...
ms.author: riande
ms.date: 01/29/2009
ms.assetid: 3e5ae74a-9798-47d8-a2b3-0d8ad42dd4bc
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
msc.type: video
ms.openlocfilehash: 2ebd9162f81f5270c92c6b8d55e2d2dad4660701
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78602817"
---
# <a name="how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page"></a>[Jak:] Użyj właściwości odpowiedzi. Filter, aby zamienić kod HTML na stronie ASP.NET

[Krzysztof pikseli](https://twitter.com/chrispels)

W tym filmie wideo Krzysztof pikseli pokazuje, jak używać właściwości odpowiedzi. Filter, aby przechwycić i zmienić kod HTML wysyłany do strony. Najpierw zostanie utworzona Przykładowa strona z niezwykłym tekstem. Następnie tworzona jest Klasa niestandardowego strumienia, która służy jako strumień zastępczy dla bieżącego strumienia wysyłanego do przeglądarki użytkownika. W tej klasie strumienia niestandardowego zawartość strony jest pobierana ze strumienia, zmieniana, a następnie zapisywana w strumieniu odpowiedzi. W tej niestandardowej klasie strumienia Metoda Write jest dostosowywana, aby zamienić kod HTML w strumieniu podstawowej odpowiedzi, a tym samym zmienić to, co jest wysyłane do przeglądarki użytkownika. Na koniec Nowa Klasa strumienia jest przypisana do właściwości Response. Filter na stronie\_zdarzenia ładowania, w związku z czym zapewnia mechanizm zmieniania zawartości strony.

[&#9654;Obejrzyj wideo (13 minut)](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page)

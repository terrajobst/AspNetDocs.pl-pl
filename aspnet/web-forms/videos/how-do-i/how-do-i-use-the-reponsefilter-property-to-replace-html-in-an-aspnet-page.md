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
# <a name="how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page"></a><span data-ttu-id="396d0-104">[Jak:] Użyj właściwości odpowiedzi. Filter, aby zamienić kod HTML na stronie ASP.NET</span><span class="sxs-lookup"><span data-stu-id="396d0-104">[How Do I:] Use the Reponse.Filter Property to Replace HTML in an ASP.NET Page</span></span>

<span data-ttu-id="396d0-105">[Krzysztof pikseli](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="396d0-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="396d0-106">W tym filmie wideo Krzysztof pikseli pokazuje, jak używać właściwości odpowiedzi. Filter, aby przechwycić i zmienić kod HTML wysyłany do strony.</span><span class="sxs-lookup"><span data-stu-id="396d0-106">In this video Chris Pels shows how to use the Reponse.Filter property to intercept and alter the HTML being sent to a page.</span></span> <span data-ttu-id="396d0-107">Najpierw zostanie utworzona Przykładowa strona z niezwykłym tekstem.</span><span class="sxs-lookup"><span data-stu-id="396d0-107">First, a sample page is created with some simple text.</span></span> <span data-ttu-id="396d0-108">Następnie tworzona jest Klasa niestandardowego strumienia, która służy jako strumień zastępczy dla bieżącego strumienia wysyłanego do przeglądarki użytkownika.</span><span class="sxs-lookup"><span data-stu-id="396d0-108">Then, a custom Stream class is created which serves as the replacement stream for the current stream being sent to the user's browser.</span></span> <span data-ttu-id="396d0-109">W tej klasie strumienia niestandardowego zawartość strony jest pobierana ze strumienia, zmieniana, a następnie zapisywana w strumieniu odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="396d0-109">In that custom stream class the contents of the page are retrieved from the stream, altered, and then written out to the response stream.</span></span> <span data-ttu-id="396d0-110">W tej niestandardowej klasie strumienia Metoda Write jest dostosowywana, aby zamienić kod HTML w strumieniu podstawowej odpowiedzi, a tym samym zmienić to, co jest wysyłane do przeglądarki użytkownika.</span><span class="sxs-lookup"><span data-stu-id="396d0-110">In this custom Stream class the Write method is customized to replace the HTML in the base Response stream, thereby altering what is sent to the user's browser.</span></span> <span data-ttu-id="396d0-111">Na koniec Nowa Klasa strumienia jest przypisana do właściwości Response. Filter na stronie\_zdarzenia ładowania, w związku z czym zapewnia mechanizm zmieniania zawartości strony.</span><span class="sxs-lookup"><span data-stu-id="396d0-111">Finally, the new stream class is assigned to the Response.Filter property in the Page\_Load event, thereby, providing the mechanism for altering the page content.</span></span>

[<span data-ttu-id="396d0-112">&#9654;Obejrzyj wideo (13 minut)</span><span class="sxs-lookup"><span data-stu-id="396d0-112">&#9654; Watch video (13 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page)

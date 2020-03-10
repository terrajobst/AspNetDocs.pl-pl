---
uid: web-forms/videos/how-do-i/how-do-i-persist-the-state-of-a-user-control-during-a-postback
title: '[How Do I]: Persist the State of a User Control During a Postback | Microsoft Docs'
author: rick-anderson
description: W tym filmie wideo Krzysztof pikseli pokazuje, jak utrzymać stan co najmniej jednego obiektu w kontrolce użytkownika. Najpierw zostanie utworzony formant użytkownika, który reprezentuje abilit...
ms.author: riande
ms.date: 04/02/2009
ms.assetid: d1bca4c6-838c-40f7-87ec-80bb67e483e5
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-persist-the-state-of-a-user-control-during-a-postback
msc.type: video
ms.openlocfilehash: c87bd6c5c993a1bde8f8a84f6d53b431e54541d9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78635822"
---
# <a name="how-do-i-persist-the-state-of-a-user-control-during-a-postback"></a>[Jak I]: utrwalanie stanu kontrolki użytkownika podczas ogłaszania zwrotnego
[How Do I]: Persist the State of a User Control During a Postback

<span data-ttu-id="d3dc9-104">[Krzysztof pikseli](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="d3dc9-104">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="d3dc9-105">W tym filmie wideo Krzysztof pikseli pokazuje, jak utrzymać stan co najmniej jednego obiektu w kontrolce użytkownika.</span><span class="sxs-lookup"><span data-stu-id="d3dc9-105">In this video Chris Pels shows how to persist the state of one or more objects in a user control.</span></span> <span data-ttu-id="d3dc9-106">Najpierw tworzony jest formant użytkownika, który reprezentuje możliwość określenia kryteriów filtrowania wyszukiwania przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="d3dc9-106">First, a user control is created that represents the ability for a user to specify filter criteria for a search.</span></span> <span data-ttu-id="d3dc9-107">Ponadto tworzona jest Klasa pomocnika filtru do przechowywania informacji o filtrach.</span><span class="sxs-lookup"><span data-stu-id="d3dc9-107">In addition, a companion Filter class is created to store the filter information.</span></span> <span data-ttu-id="d3dc9-108">Do kontrolki filtru dodawane są kilka elementów interfejsu użytkownika wraz z niektórymi metodami i właściwościami w celu przechowywania bieżących informacji o filtrach w wystąpieniu klasy filtru.</span><span class="sxs-lookup"><span data-stu-id="d3dc9-108">Several user interface elements are added to the filter control along with some methods and properties to store the current filter information in the Filter class instance.</span></span> <span data-ttu-id="d3dc9-109">Następnie trwałość kontrolki użytkownika jest implementowana przy użyciu metody RegisterRequiresControlState i skojarzonych metod Save/Restore.</span><span class="sxs-lookup"><span data-stu-id="d3dc9-109">Next, the user control persistence is implemented using the RegisterRequiresControlState method and associated Save/Restore methods.</span></span> <span data-ttu-id="d3dc9-110">Te metody przechowują wystąpienie klasy Filter i jej danych podczas ogłaszania zwrotnego stron.</span><span class="sxs-lookup"><span data-stu-id="d3dc9-110">These methods store the instance of the filter class and its data during page postbacks.</span></span> <span data-ttu-id="d3dc9-111">Na koniec istnieje omówienie sposobu przechowywania wielu obiektów w implementacji stanu sterowania.</span><span class="sxs-lookup"><span data-stu-id="d3dc9-111">Finally, there is a discussion of how to store multiple objects in control state implementation.</span></span>

[<span data-ttu-id="d3dc9-112">&#9654;Obejrzyj wideo (23 minuty)</span><span class="sxs-lookup"><span data-stu-id="d3dc9-112">&#9654; Watch video (23 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-persist-the-state-of-a-user-control-during-a-postback)

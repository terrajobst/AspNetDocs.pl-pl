---
uid: web-forms/videos/how-do-i/how-do-i-persist-the-state-of-a-user-control-during-a-postback
title: '[How Do I]: Persist the State of a User Control During a Postback | Microsoft Docs'
author: rick-anderson
description: W tym wideo pikseli Chris pokazuje, jak do utrwalenia stanu jeden lub więcej obiektów w kontrolce użytkownika. Najpierw kontrolki użytkownika jest tworzony, który reprezentuje abilit...
ms.author: riande
ms.date: 04/02/2009
ms.assetid: d1bca4c6-838c-40f7-87ec-80bb67e483e5
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-persist-the-state-of-a-user-control-during-a-postback
msc.type: video
ms.openlocfilehash: c87bd6c5c993a1bde8f8a84f6d53b431e54541d9
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/17/2019
ms.locfileid: "59414780"
---
# <a name="how-do-i-persist-the-state-of-a-user-control-during-a-postback"></a>[Jak]: Utrwalanie stanu kontrolki użytkownika podczas ogłaszania zwrotnego
[How Do I]: Persist the State of a User Control During a Postback

<span data-ttu-id="d6ebb-104">przez [Chris pikseli](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="d6ebb-104">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="d6ebb-105">W tym wideo pikseli Chris pokazuje, jak do utrwalenia stanu jeden lub więcej obiektów w kontrolce użytkownika.</span><span class="sxs-lookup"><span data-stu-id="d6ebb-105">In this video Chris Pels shows how to persist the state of one or more objects in a user control.</span></span> <span data-ttu-id="d6ebb-106">Po pierwsze kontrolki użytkownika jest tworzony, który reprezentuje zdolność użytkownikowi określić kryteria filtru wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="d6ebb-106">First, a user control is created that represents the ability for a user to specify filter criteria for a search.</span></span> <span data-ttu-id="d6ebb-107">Ponadto pomocnika klasy filtr zostanie utworzona przechowywać informacje o filtrze.</span><span class="sxs-lookup"><span data-stu-id="d6ebb-107">In addition, a companion Filter class is created to store the filter information.</span></span> <span data-ttu-id="d6ebb-108">Kilka elementów interfejsu użytkownika są dodawane do formantu filtru wraz z niektórych metod i właściwości do przechowywania informacji o filtrach w wystąpieniu klasy filtru.</span><span class="sxs-lookup"><span data-stu-id="d6ebb-108">Several user interface elements are added to the filter control along with some methods and properties to store the current filter information in the Filter class instance.</span></span> <span data-ttu-id="d6ebb-109">Następnie trwałości formantu użytkownika jest implementowany przy użyciu metody RegisterRequiresControlState i skojarzone metody zapisać/przywrócić.</span><span class="sxs-lookup"><span data-stu-id="d6ebb-109">Next, the user control persistence is implemented using the RegisterRequiresControlState method and associated Save/Restore methods.</span></span> <span data-ttu-id="d6ebb-110">Te metody przechowywania wystąpienia klasy filtru i jego danych podczas ogłaszania zwrotnego strony.</span><span class="sxs-lookup"><span data-stu-id="d6ebb-110">These methods store the instance of the filter class and its data during page postbacks.</span></span> <span data-ttu-id="d6ebb-111">Na koniec jest dyskusję na temat sposobu przechowywania wielu obiektów w implementacji stan kontrolki.</span><span class="sxs-lookup"><span data-stu-id="d6ebb-111">Finally, there is a discussion of how to store multiple objects in control state implementation.</span></span>

[<span data-ttu-id="d6ebb-112">&#9654;Obejrzyj film wideo (23 minuty)</span><span class="sxs-lookup"><span data-stu-id="d6ebb-112">&#9654; Watch video (23 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-persist-the-state-of-a-user-control-during-a-postback)

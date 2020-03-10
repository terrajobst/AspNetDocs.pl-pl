---
uid: web-forms/videos/aspnet-ajax/how-do-i-choose-between-methods-of-ajax-page-updates
title: '[Jak:] Czy wybierać między metodami aktualizacji strony AJAX? | Microsoft Docs'
author: JoeStagner
description: W tym wideo Jan Stagner porównuje dwie podstawowe metody wykonywania aktualizacji stron w stylu AJAX w aplikacji ASP.NET. Pierwsza metoda polega na użyciu UPD...
ms.author: riande
ms.date: 07/09/2007
ms.assetid: a5e33a7d-ccb2-483f-a955-3d39f72ba4ec
msc.legacyurl: /web-forms/videos/aspnet-ajax/how-do-i-choose-between-methods-of-ajax-page-updates
msc.type: video
ms.openlocfilehash: 56e3ebfbe0b5af4234791136725de79e38171cc1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78523668"
---
# <a name="how-do-i-choose-between-methods-of-ajax-page-updates"></a><span data-ttu-id="d4d1d-105">[Jak:] Czy wybierać między metodami aktualizacji strony AJAX?</span><span class="sxs-lookup"><span data-stu-id="d4d1d-105">[How Do I:] Choose Between Methods of AJAX Page Updates?</span></span>

<span data-ttu-id="d4d1d-106">Jan [Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="d4d1d-106">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

<span data-ttu-id="d4d1d-107">W tym wideo Jan Stagner porównuje dwie podstawowe metody wykonywania aktualizacji stron w stylu AJAX w aplikacji ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="d4d1d-107">In this video Joe Stagner compares the two primary methods of performing AJAX-style page updates in an ASP.NET application.</span></span> <span data-ttu-id="d4d1d-108">Pierwsza metoda polega na użyciu elementu UpdatePanel, gdzie nie trzeba pisać dodatkowego kodu po stronie klienta lub po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="d4d1d-108">The first method is to use an UpdatePanel, where no additional code needs to be written on either the client side or the server side.</span></span> <span data-ttu-id="d4d1d-109">Zaletą korzystania z elementu UpdatePanel jest to, że wszystko działa automatycznie.</span><span class="sxs-lookup"><span data-stu-id="d4d1d-109">The benefit of using the UpdatePanel is that everything works automatically.</span></span> <span data-ttu-id="d4d1d-110">Kara polega na tym, że klient wymaga dużej ilości danych do uwzględnienia w żądaniu i odpowiedzi AJAX, a na serwerze wymaga pełnego cyklu życia strony.</span><span class="sxs-lookup"><span data-stu-id="d4d1d-110">The penalty is that at the client it requires a lot of data to be included in the AJAX request and response, and at the server it requires a full page lifecycle to be executed.</span></span> <span data-ttu-id="d4d1d-111">Druga metoda polega na użyciu wywołań zwrotnych sieci, gdzie dodatkowy kod musi być zapisany zarówno po stronie klienta, jak i po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="d4d1d-111">The second method is to use network callbacks, where additional code needs to be written on both the client side and the server side.</span></span> <span data-ttu-id="d4d1d-112">Korzystanie z wywołań zwrotnych sieci polega na tym, że klient wymaga, aby w żądaniu i odpowiedzi w technologii AJAX znajdował się niewielką ilość danych, a na serwerze musi być uruchomiona tylko wywołana metoda usługi.</span><span class="sxs-lookup"><span data-stu-id="d4d1d-112">The benefit of using network callbacks is that at the client it requires very little data to be included in the AJAX request and response, and at the server it requires only the called service method to be executed.</span></span> <span data-ttu-id="d4d1d-113">Okresem karnym jest czas i nakład pracy potrzebny do zapisania niezbędnego kodu.</span><span class="sxs-lookup"><span data-stu-id="d4d1d-113">The penality is the time and effort it takes to write the necessary code.</span></span> <span data-ttu-id="d4d1d-114">Jan kończy wideo, omawiając, co należy wziąć pod uwagę podczas wybierania dwóch podstawowych metod aktualizacji strony w stylu AJAX.</span><span class="sxs-lookup"><span data-stu-id="d4d1d-114">Joe concludes the video by discussing what you should consider when choosing between the two primary methods of AJAX-style page updates.</span></span> <span data-ttu-id="d4d1d-115">(Ten film wideo korzysta z kodu z [ASP.NET AJAX](how-do-i-get-started-with-aspnet-ajax.md) wideo i [jak tworzyć wywołania zwrotne sieci po stronie klienta przy użyciu wideo ASP.NET AJAX](how-do-i-make-client-side-network-callbacks-with-aspnet-ajax.md) ).</span><span class="sxs-lookup"><span data-stu-id="d4d1d-115">(This video uses the code from the [How Do I Get Started with ASP.NET AJAX](how-do-i-get-started-with-aspnet-ajax.md) video and the [How Do I Make Client-Side Network Callbacks with ASP.NET AJAX](how-do-i-make-client-side-network-callbacks-with-aspnet-ajax.md) video.)</span></span>

[<span data-ttu-id="d4d1d-116">&#9654;Obejrzyj wideo (11 minut)</span><span class="sxs-lookup"><span data-stu-id="d4d1d-116">&#9654; Watch video (11 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-choose-between-methods-of-ajax-page-updates)

> [!div class="step-by-step"]
> <span data-ttu-id="d4d1d-117">[Poprzednie](how-do-i-update-multiple-regions-of-a-page-with-aspnet-ajax.md)
> [dalej](how-do-i-use-other-javascript-user-interface-libraries-with-aspnet-ajax.md)</span><span class="sxs-lookup"><span data-stu-id="d4d1d-117">[Previous](how-do-i-update-multiple-regions-of-a-page-with-aspnet-ajax.md)
[Next](how-do-i-use-other-javascript-user-interface-libraries-with-aspnet-ajax.md)</span></span>

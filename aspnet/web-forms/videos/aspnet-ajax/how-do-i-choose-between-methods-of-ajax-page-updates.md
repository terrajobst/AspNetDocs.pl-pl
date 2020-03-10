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
# <a name="how-do-i-choose-between-methods-of-ajax-page-updates"></a>[Jak:] Czy wybierać między metodami aktualizacji strony AJAX?

Jan [Stagner](https://github.com/JoeStagner)

W tym wideo Jan Stagner porównuje dwie podstawowe metody wykonywania aktualizacji stron w stylu AJAX w aplikacji ASP.NET. Pierwsza metoda polega na użyciu elementu UpdatePanel, gdzie nie trzeba pisać dodatkowego kodu po stronie klienta lub po stronie serwera. Zaletą korzystania z elementu UpdatePanel jest to, że wszystko działa automatycznie. Kara polega na tym, że klient wymaga dużej ilości danych do uwzględnienia w żądaniu i odpowiedzi AJAX, a na serwerze wymaga pełnego cyklu życia strony. Druga metoda polega na użyciu wywołań zwrotnych sieci, gdzie dodatkowy kod musi być zapisany zarówno po stronie klienta, jak i po stronie serwera. Korzystanie z wywołań zwrotnych sieci polega na tym, że klient wymaga, aby w żądaniu i odpowiedzi w technologii AJAX znajdował się niewielką ilość danych, a na serwerze musi być uruchomiona tylko wywołana metoda usługi. Okresem karnym jest czas i nakład pracy potrzebny do zapisania niezbędnego kodu. Jan kończy wideo, omawiając, co należy wziąć pod uwagę podczas wybierania dwóch podstawowych metod aktualizacji strony w stylu AJAX. (Ten film wideo korzysta z kodu z [ASP.NET AJAX](how-do-i-get-started-with-aspnet-ajax.md) wideo i [jak tworzyć wywołania zwrotne sieci po stronie klienta przy użyciu wideo ASP.NET AJAX](how-do-i-make-client-side-network-callbacks-with-aspnet-ajax.md) ).

[&#9654;Obejrzyj wideo (11 minut)](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-choose-between-methods-of-ajax-page-updates)

> [!div class="step-by-step"]
> [Poprzednie](how-do-i-update-multiple-regions-of-a-page-with-aspnet-ajax.md)
> [dalej](how-do-i-use-other-javascript-user-interface-libraries-with-aspnet-ajax.md)

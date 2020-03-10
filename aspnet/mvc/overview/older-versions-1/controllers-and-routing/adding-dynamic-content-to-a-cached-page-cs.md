---
uid: mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-cs
title: Dodawanie zawartości dynamicznej do buforowanej strony (C#) | Microsoft Docs
author: microsoft
description: Dowiedz się, jak mieszać zawartość dynamiczną i buforowaną na tej samej stronie. Podstawianie po pamięci podręcznej umożliwia wyświetlanie zawartości dynamicznej, na przykład ogłoszeń transparentów o...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: 2ddd4407-d143-4a94-877c-21771bfb97a6
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-cs
msc.type: authoredcontent
ms.openlocfilehash: be43712d3dd5235117558e991d9dd71aa30ec470
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78601585"
---
# <a name="adding-dynamic-content-to-a-cached-page-c"></a>Dodawanie zawartości dynamicznej do buforowanej strony (C#)

przez [firmę Microsoft](https://github.com/microsoft)

> Dowiedz się, jak mieszać zawartość dynamiczną i buforowaną na tej samej stronie. Podstawianie po pamięci podręcznej umożliwia wyświetlanie zawartości dynamicznej, takiej jak anonse transparentu lub elementy wiadomości, w obrębie strony, która została w pamięci podręcznej.

Dzięki wykorzystaniu buforowania danych wyjściowych można znacznie poprawić wydajność aplikacji ASP.NET MVC. Zamiast wygenerowania strony za każdym razem, gdy żądana jest strona, strona może być generowana jednokrotnie i buforowana w pamięci dla wielu użytkowników.

Ale występuje problem. Co zrobić, jeśli chcesz wyświetlić zawartość dynamiczną na stronie? Załóżmy na przykład, że chcesz wyświetlić na stronie anons transparentu. Nie chcesz, aby Anonsy transparentu były przechowywane w pamięci podręcznej, aby każdy użytkownik widział to samo ogłoszenie. W ten sposób nie będziesz mieć żadnych pieniędzy.

Na szczęście istnieje łatwe rozwiązanie. Możesz skorzystać z funkcji platformy ASP.NET Framework zwanej *podstawianiem pamięci podręcznej*. Podstawianie po pamięci podręcznej umożliwia zastępowanie zawartości dynamicznej na stronie, która została zbuforowana w pamięci.

Zwykle podczas wyprowadzania danych wyjściowych ze strony przy użyciu atrybutu [OutputCache] Strona jest buforowana zarówno na serwerze, jak i na kliencie (w przeglądarce sieci Web). Jeśli używasz podstawiania po pamięci podręcznej, strona jest buforowana tylko na serwerze.

#### <a name="using-post-cache-substitution"></a>Using Post-Cache Substitution

Zastosowanie podstawiania po pamięci podręcznej wymaga wykonania dwóch kroków. Najpierw należy zdefiniować metodę zwracającą ciąg, który reprezentuje zawartość dynamiczną, która ma być wyświetlana na stronie buforowanej. Następnie należy wywołać metodę HttpResponse. WriteSubstitution (), aby wstrzyknąć zawartość dynamiczną do strony.

Załóżmy na przykład, że chcesz losowo wyświetlać różne elementy wiadomości w pamięci podręcznej. Klasa na liście 1 uwidacznia pojedynczą metodę o nazwie RenderNews (), która losowo zwraca jeden element News z listy trzech elementów wiadomości.

**Lista 1 – Models\News.cs**

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample1.cs)]

Aby skorzystać z podstawiania po pamięci podręcznej, należy wywołać metodę HttpResponse. WriteSubstitution (). Metoda WriteSubstitution () konfiguruje kod w celu zastąpienia regionu buforowanej strony z zawartością dynamiczną. Metoda WriteSubstitution () służy do wyświetlania losowego elementu wiadomości w widoku na liście 2.

**Lista 2 — Views\Home\Index.aspx**

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample2.aspx)]

Metoda RenderNews jest przenoszona do metody WriteSubstitution (). Zwróć uwagę, że metoda RenderNews nie jest wywoływana (nie ma nawiasów). Zamiast tego odwołanie do metody jest przesyłane do WriteSubstitution ().

Widok indeksu jest buforowany. Widok jest zwracany przez kontroler na liście 3. Zwróć uwagę, że akcja index () jest uzupełniona atrybutem [OutputCache], który powoduje, że widok indeksu jest buforowany przez 60 sekund.

**Lista 3 — Controllers\HomeController.cs**

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample3.cs)]

Mimo że widok indeksu jest buforowany, różne losowe elementy wiadomości są wyświetlane po zażądaniu strony indeksu. Po zażądaniu strony indeksu czas wyświetlany przez stronę nie zmienia się na 60 sekund (patrz rysunek 1). Fakt, że czas nie zmienia się, udowadnia, że strona jest buforowana. Jednak zawartość wprowadzona przez metodę WriteSubstitution () — losowy element wiadomości — zmienia się przy każdym żądaniu.

**Rysunek 1 — Wprowadzanie dynamicznych elementów wiadomości na zbuforowanej stronie**

![clip_image002](adding-dynamic-content-to-a-cached-page-cs/_static/image1.jpg)

#### <a name="using-post-cache-substitution-in-helper-methods"></a>Używanie podstawiania po pamięci podręcznej w metodach pomocniczych

Łatwiejszym sposobem wykorzystania podstawiania po pamięci podręcznej jest hermetyzacja wywołania metody WriteSubstitution () w ramach niestandardowej metody pomocnika. To podejście jest zilustrowane przez metodę pomocnika na liście 4.

**Lista 4 – AdHelper.cs**

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample4.cs)]

Lista 4 zawiera klasę statyczną, która uwidacznia dwie metody: RenderBanner () i RenderBannerInternal (). Metoda RenderBanner () reprezentuje rzeczywistą metodę pomocnika. Ta metoda rozszerza standardową klasę ASP.NET MVC HtmlHelper, aby można było wywołać html. RenderBanner () w widoku podobnie jak jakakolwiek inna metoda pomocnika.

Metoda RenderBanner () wywołuje metodę HttpResponse. WriteSubstitution () przekazującą metodę RenderBannerInternal () do metody WriteSubstitution ().

Metoda RenderBannerInternal () jest metodą prywatną. Ta metoda nie zostanie udostępniona jako metoda pomocnicza. Metoda RenderBannerInternal () losowo zwraca jeden obraz anonsu transparentu z listy trzech obrazów anonsów transparentu.

Widok zmodyfikowany indeks w liście 5 ilustruje sposób używania metody pomocnika RenderBanner (). Zwróć uwagę, że w górnej części widoku znajduje się dodatkowa &lt;% @ import%&gt; dyrektywa, aby zaimportować przestrzeń nazw MvcApplication1. helps. Jeśli nie chcesz zaimportować tej przestrzeni nazw, Metoda RenderBanner () nie będzie wyświetlana jako metoda właściwości HTML.

**Lista 5 – Views\Home\Index.aspx (with RenderBanner () — Metoda**

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample5.aspx)]

Po zażądaniu strony renderowanej przez widok na liście 5 jest wyświetlany inny anons transparentu z każdym żądaniem (patrz rysunek 2). Strona jest buforowana, ale anons transparentu jest wprowadzany dynamicznie przez metodę pomocnika RenderBanner ().

**Rysunek 2 — widok indeksu przedstawiający losowy anons transparentu**

![clip_image004](adding-dynamic-content-to-a-cached-page-cs/_static/image2.jpg)

#### <a name="summary"></a>Podsumowanie

W tym samouczku wyjaśniono sposób dynamicznego aktualizowania zawartości w pamięci podręcznej. Wiesz już, jak używać metody HttpResponse. WriteSubstitution () do włączania zawartości dynamicznej na stronie pamięci podręcznej. Wiesz również, jak hermetyzować wywołanie metody WriteSubstitution () w ramach metody pomocnika HTML.

Korzystaj z pamięci podręcznej, jeśli to możliwe — może mieć znaczący wpływ na wydajność aplikacji sieci Web. Jak wyjaśniono w tym samouczku, możesz skorzystać z buforowania nawet wtedy, gdy musisz wyświetlić zawartość dynamiczną na stronach.

> [!div class="step-by-step"]
> [Poprzednie](improving-performance-with-output-caching-cs.md)
> [dalej](creating-a-controller-cs.md)

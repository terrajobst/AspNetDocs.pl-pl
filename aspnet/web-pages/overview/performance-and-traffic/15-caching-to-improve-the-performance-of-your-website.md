---
uid: web-pages/overview/performance-and-traffic/15-caching-to-improve-the-performance-of-your-website
title: Buforowanie danych w witrynie ASP.NET Web Pages (Razor) w celu zapewnienia lepszej wydajności | Microsoft Docs
author: Rick-Anderson
description: Aby przyspieszyć swoją witrynę sieci Web, możesz ją przechowywać w pamięci podręcznej — czyli na podstawie danych, które zwykle zajmują dużo czasu na pobranie lub przetworzenie...
ms.author: riande
ms.date: 02/14/2014
ms.assetid: 961e525b-7700-469e-8a68-d7010b6fb68c
msc.legacyurl: /web-pages/overview/performance-and-traffic/15-caching-to-improve-the-performance-of-your-website
msc.type: authoredcontent
ms.openlocfilehash: 01796d3ca699a6af5d9162b22a926551435c2040
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78641520"
---
# <a name="caching-data-in-an-aspnet-web-pages-razor-site-for-better-performance"></a>Buforowanie danych w witrynie ASP.NET Web Pages (Razor) w celu zapewnienia lepszej wydajności

Autor [FitzMacken](https://github.com/tfitzmac)

> W tym artykule wyjaśniono, jak używać pomocnika do buforowania informacji w celu zwiększenia wydajności w witrynie internetowej ASP.NET Web Pages (Razor). Aby przyspieszyć swoją witrynę sieci Web, Wystarczy przechowywać &#8212; ją w pamięci podręcznej &#8212; , co oznacza, że dane są zwykle czasochłonne w celu pobrania lub przetworzenia, a które nie zmieniają się często.
> 
> **Dowiesz się:** 
> 
> - Jak używać buforowania, aby zwiększyć czas odpowiedzi witryny sieci Web.
> 
> Są to funkcje ASP.NET wprowadzone w artykule:
> 
> - Pomocnik `WebCache`.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używane w samouczku
> 
> 
> - ASP.NET strony sieci Web (Razor) 3
>   
> 
> Ten samouczek działa również z ASP.NET Web Pages 2.

Za każdym razem, gdy ktoś żąda strony z witryny, serwer sieci Web musi wykonać pewne czynności w celu spełnienia żądania. W przypadku niektórych stron serwer może potrzebować wykonać zadania, które pomogą (w sposób porównawczy), takie jak pobieranie danych z bazy danych. Nawet jeśli te zadania nie zajmują dużo czasu, w przypadku, gdy witryna zajmuje dużo ruchu, cała seria indywidualnych żądań, które powodują, że serwer sieci Web do wykonywania skomplikowanych lub wolnych zadań może dodawać do dużo pracy. Może to ostatecznie mieć wpływ na wydajność lokacji.

Jednym ze sposobów na zwiększenie wydajności witryny sieci Web w takich okolicznościach jest buforowanie danych. Jeśli witryna uzyskuje powtórzone żądania dla tych samych informacji, a informacje nie muszą być modyfikowane dla każdej osoby i nie są zależne od czasu, a nie do ponownego pobrania lub ponownej obliczeń, można pobrać dane jeden raz, a następnie zapisać wyniki. Następnym razem, gdy żądanie dotyczy tych informacji, wystarczy pobrać je z pamięci podręcznej.

Ogólnie rzecz biorąc, informacje o pamięci podręcznej, które nie zmieniają się często. Gdy umieszczasz informacje w pamięci podręcznej, są one przechowywane w pamięci na serwerze sieci Web. Możesz określić, jak długo powinna być buforowana, od sekund do dni. Po upływie okresu buforowania informacje są automatycznie usuwane z pamięci podręcznej.

> [!NOTE]
> Wpisy w pamięci podręcznej mogą zostać usunięte ze względu na to, że wygasły. Na przykład serwer sieci Web może tymczasowo uruchamiać małą ilość pamięci, a jednym ze sposobów odzyskiwania pamięci jest wyrzucanie wpisów z pamięci podręcznej. Jak widać, nawet jeśli informacje są umieszczane w pamięci podręcznej, należy upewnić się, że są nadal dostępne, gdy ich potrzebujesz.

Wyobraź sobie, że witryna sieci Web zawiera stronę wyświetlającą bieżącą temperaturę i prognozę pogody. Aby uzyskać informacje o tym typie, można wysłać żądanie do usługi zewnętrznej. Ponieważ te informacje nie zmieniają się znacznie (na przykład w okresie dwóch godzin), ponieważ wywołania zewnętrzne wymagają czasu i przepustowości, jest to dobry kandydat do buforowania.

## <a name="adding-caching-to-a-page"></a>Dodawanie buforowania do strony

ASP.NET zawiera pomocnika `WebCache`, który ułatwia dodawanie buforowania do lokacji i Dodawanie danych do pamięci podręcznej. W tej procedurze utworzysz stronę, która przechowuje w pamięci podręcznej bieżącą godzinę. To nie jest rzeczywisty przykład, ponieważ bieżący czas to coś, co często zmienia się, a co nie jest skomplikowane do obliczenia. Jednak jest dobrym sposobem zilustrowania buforowania w działaniu.

1. Dodaj nową stronę o nazwie *webcache. cshtml* do witryny sieci Web.
2. Dodaj następujący kod i znaczniki do strony:

    [!code-cshtml[Main](15-caching-to-improve-the-performance-of-your-website/samples/sample1.cshtml)]

    Podczas buforowania danych należy umieścić je w pamięci podręcznej przy użyciu nazwy, która jest unikatowa w całej witrynie sieci Web. W takim przypadku należy użyć wpisu pamięci podręcznej o nazwie `CachedTime`. Jest to `cacheItemKey` pokazano w przykładzie kodu.

    Kod najpierw odczytuje wpis pamięci podręcznej `CachedTime`. Jeśli zwracana jest wartość (to oznacza, że jeśli wpis pamięci podręcznej nie ma wartości null), kod ustawia wartość zmiennej czasowej na dane pamięci podręcznej.

    Jeśli jednak wpis pamięci podręcznej nie istnieje (oznacza to, że ma wartość null), kod ustawia wartość czasu, dodaje ją do pamięci podręcznej i ustawia wartość wygaśnięcia na jedną minutę. Po upływie jednej minuty wpis pamięci podręcznej zostanie odrzucony. (Domyślna wartość wygaśnięcia dla elementu w pamięci podręcznej to 20 minut). Polecenie `WebCache.Set(cacheItemKey, time, 1, false)` pokazuje, jak dodać bieżącą wartość czasu do pamięci podręcznej i ustawić jej wygaśnięcie na 1 minutę. Ustawienie parametru *slidingExpiration* na `false` oznacza, że czas wygaśnięcia nie jest odnawiany przy każdym żądaniu żądania. Po jego pierwotnym dodaniu do pamięci podręcznej wygaśnie dokładnie 1 minutę. Jeśli ta wartość zostanie ustawiona na `true` czas wygaśnięcia 1 minuty zostanie zresetowany za każdym razem, gdy wartość zostanie zażądana z pamięci podręcznej.

    Ten kod ilustruje wzorzec, który powinien być zawsze używany podczas buforowania danych. Przed uzyskaniem czegoś z pamięci podręcznej należy zawsze najpierw sprawdzić, czy metoda `WebCache.Get` zwróciła wartość null. Należy pamiętać, że wpis pamięci podręcznej mógł wygasnąć lub mógł zostać usunięty z jakiegoś innego powodu, więc każdy podaną pozycję nigdy nie będzie w pamięci podręcznej.
3. Uruchom w przeglądarce element *webcache. cshtml* . (Upewnij się, że strona została wybrana w obszarze roboczym **pliki** przed jej uruchomieniem). Podczas pierwszego żądania strony dane czasu nie znajdują się w pamięci podręcznej, a kod musi dodać wartość czasu do pamięci podręcznej.

    ![pamięć podręczna 1](15-caching-to-improve-the-performance-of-your-website/_static/image1.jpg)
4. Odśwież w przeglądarce *webcache. cshtml* . Tym razem dane czasu są w pamięci podręcznej. Zauważ, że czas nie został zmieniony od czasu ostatniego wyświetlenia strony.

    ![pamięć podręczna 2](15-caching-to-improve-the-performance-of-your-website/_static/image2.jpg)
5. Odczekaj jedną minutę, aby opróżnić pamięć podręczną, a następnie Odśwież stronę. Strona ponownie wskazuje, że dane czasu nie zostały odnalezione w pamięci podręcznej, a zaktualizowany czas został dodany do pamięci podręcznej.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Dodatkowe materiały

- [Wyświetlanie danych na wykresie](https://go.microsoft.com/fwlink/?LinkId=202895)
- [Dokumentacja interfejsu API webcache](https://msdn.microsoft.com/library/system.web.helpers.webcache(v=vs.99).aspx) (MSDN)

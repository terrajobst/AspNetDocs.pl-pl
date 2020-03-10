---
uid: web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
title: Tworzenie i używanie pomocnika w witrynie ASP.NET Web Pages (Razor) | Microsoft Docs
author: Rick-Anderson
description: W tym artykule opisano sposób tworzenia pomocnika w witrynie internetowej ASP.NET Web Pages (Razor). Pomocnik to składnik wielokrotnego użytku, który zawiera kod i znaczniki do wydajności...
ms.author: riande
ms.date: 02/17/2014
ms.assetid: 46bff772-01e0-40f0-9ae6-9e18c5442ee6
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 380663951094c9fc7d5f0601e30995fa073a204b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78563512"
---
# <a name="creating-and-using-a-helper-in-an-aspnet-web-pages-razor-site"></a>Tworzenie i używanie pomocnika w witrynie ASP.NET Web Pages (Razor)

Autor [FitzMacken](https://github.com/tfitzmac)

> W tym artykule opisano sposób tworzenia pomocnika w witrynie internetowej ASP.NET Web Pages (Razor). *Pomocnik* to składnik wielokrotnego użytku, który zawiera kod i znaczniki umożliwiające wykonanie zadania, które może być żmudnym lub złożone.
> 
> **Dowiesz się:** 
> 
> - Jak utworzyć i używać prostego pomocnika.
> 
> Są to funkcje ASP.NET wprowadzone w artykule:
> 
> - Składnia `@helper`.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używane w samouczku
> 
> 
> - ASP.NET strony sieci Web (Razor) 3
>   
> 
> Ten samouczek działa również z ASP.NET Web Pages 2.

## <a name="overview-of-helpers"></a>Przegląd pomocników

Jeśli musisz wykonać te same zadania na różnych stronach w witrynie, możesz użyć pomocnika. Strony sieci Web ASP.NET zawierają kilka pomocników i istnieje wiele więcej, które można pobrać i zainstalować. (Lista wbudowanych pomocników na stronach sieci Web ASP.NET jest wymieniona w [szybkim Kompendium interfejsu API ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202907)). Jeśli żaden z istniejących pomocników nie spełnia Twoich potrzeb, możesz utworzyć własnego użytkownika.

Pomocnik umożliwia użycie typowego bloku kodu na wielu stronach. Załóżmy, że na stronie często chcesz utworzyć element notatki, który jest ustawiany poza normalnymi akapitami. Być może notatka jest tworzona jako element `<div>`, który jest stylem jako pole z obramowaniem. Zamiast dodawać te same znaczniki do strony za każdym razem, gdy chcesz wyświetlić notatkę, możesz spakować znacznik jako pomocnika. Następnie możesz wstawić notatkę z jednym wierszem kodu wszędzie tam, gdzie jest to potrzebne.

Użycie pomocnika takiego jak sprawia, że kod na każdej stronie jest prostszy i łatwiejszy do odczytania. Ułatwia ona również obsługę witryny, ponieważ w przypadku konieczności zmiany wyglądu notatek można zmienić adiustację w jednym miejscu.

## <a name="creating-a-helper"></a>Tworzenie pomocnika

Ta procedura pokazuje, jak utworzyć pomocnika, który tworzy notatkę, tak jak to opisano. Jest to prosty przykład, ale pomocnik niestandardowy może zawierać dowolny kod znacznika i ASP.NET, którego potrzebujesz.

1. W folderze głównym witryny sieci Web Utwórz folder o nazwie *App\_Code*. Jest to zarezerwowana nazwa folderu w ASP.NET, w którym można umieścić kod dla składników takich jak pomocniki.
2. W folderze *\_aplikacji* Utwórz nowy plik *. cshtml* i nazwij go *pomocnik. cshtml*.
3. Zastąp istniejącą zawartość następującym:

    [!code-cshtml[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    Kod używa składni `@helper` do deklarowania nowego pomocnika o nazwie `MakeNote`. Ten konkretny pomocnik umożliwia przekazywanie parametru o nazwie `content`, który może zawierać kombinację tekstu i znaczników. Pomocnik wstawia ciąg do treści notatki przy użyciu zmiennej `@content`.

    Zwróć uwagę, że plik ma nazwę *pomocnicy. cshtml*, ale pomocnik ma nazwę `MakeNote`. Można umieścić wiele pomocników niestandardowych w pojedynczym pliku.
4. Zapisz i zamknij plik.

## <a name="using-the-helper-in-a-page"></a>Korzystanie z pomocnika na stronie

1. W folderze głównym Utwórz nowy pusty plik o nazwie *TestHelper. cshtml*.
2. Dodaj następujący kod do pliku:

    [!code-html[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample2.html)]

    Aby wywołać utworzoną przez Ciebie pomocnika, użyj `@`, a następnie nazwę pliku, gdzie pomocnika to, kropka, a następnie nazwę pomocnika. (Jeśli w folderze *\_aplikacji* znajduje się wiele folderów, można użyć składni `@FolderName.FileName.HelperName` do wywołania pomocnika na dowolnym zagnieżdżonym poziomie folderów). Tekst dodany w cudzysłowie w nawiasach jest tekstem, który pomocnik będzie wyświetlał jako część notatki na stronie sieci Web.
3. Zapisz stronę i uruchom ją w przeglądarce. Pomocnik generuje element notatki z prawej strony, gdzie wywołano pomocnika: między dwa akapity.

    ![Zrzut ekranu przedstawiający stronę w przeglądarce oraz sposób wygenerowania znaczników przez pomocnika, które umieszczają pole wokół określonego tekstu.](creating-and-using-a-helper-in-an-aspnet-web-pages-site/_static/image1.png)

## <a name="additional-resources"></a>Dodatkowe materiały

[Menu poziome jako pomocnik Razor](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2341). Ten wpis w blogu według Jan Pope pokazuje, jak utworzyć poziome menu jako pomocnika przy użyciu znaczników, CSS i kodu.

Korzystanie [z HTML5 w ASP.NET stronach sieci Web pomocników dla WebMatrix i ASP.NET MvC3](http://geekswithblogs.net/wildturtle/archive/2010/11/08/html5-in-asp.net-web-pages-helpers-for-webmatrix-and_aspnet_mvc3.aspx). Ten wpis w blogu Abraham sam pokazuje pomocnika, który renderuje element HTML5 `Canvas`.

[Różnica między @Helpers i @Functions w programie WebMatrix](http://www.mikesdotnetting.com/Article/173/The-Difference-Between-@Helpers-and-@Functions-In-WebMatrix). Ten wpis w blogu według Jan solance zawiera opis składni `@helper` i składni `@function` i kiedy należy używać każdego z nich.

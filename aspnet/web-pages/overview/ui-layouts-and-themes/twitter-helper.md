---
uid: web-pages/overview/ui-layouts-and-themes/twitter-helper
title: Pomocnik usługi Twitter ze stronami sieci Web ASP.NET | Microsoft Docs
author: Rick-Anderson
description: W tym temacie i aplikacji pokazano, jak dodać pomocnika usługi Twitter do projektu WebMatrix 3. Zawiera kod pomocnika usługi Twitter i pokazuje, jak wywołać pomocnika...
ms.author: riande
ms.date: 11/26/2018
ms.assetid: c1a1244e-b9c8-42e6-a00b-8456a4ec027c
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/twitter-helper
msc.type: authoredcontent
ms.openlocfilehash: 76e32b7c808467a9a87c70017dac02bdb895e1df
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78638559"
---
# <a name="twitter-helper-with-aspnet-web-pages"></a>Pomocnik usługi Twitter we wzorcu ASP.NET Web Pages

Autor [FitzMacken](https://github.com/tfitzmac)

> [!IMPORTANT]
> Pomocnicy usługi Twitter są przestarzałi. Najnowsze narzędzia do zakontraktowań dla witryn sieci Web w usłudze Twitter można znaleźć w temacie [Omówienie usługi Twitter for websites](https://developer.twitter.com/en/docs/twitter-for-websites/overview)

> W tym temacie i aplikacji pokazano, jak dodać pomocnika usługi Twitter do projektu WebMatrix 3. Zawiera kod pomocnika usługi Twitter i pokazuje, jak wywołać metody pomocnika.
> 
> Ten kod dla pliku Twitter. cshtml został opracowany przez **Tian przesunięcia** firmy Microsoft.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używane w samouczku
> 
> 
> - ASP.NET strony sieci Web (Razor) 3
>   
> 
> Ten samouczek działa również z ASP.NET Web Pages 2.

## <a name="introduction"></a>Wprowadzenie

W tym temacie pokazano, jak dodać pomocnika usługi Twitter do aplikacji i użyć składnia Razor, aby wywołać metody pomocnika. Pomocnik usługi Twitter ułatwia włączanie przycisków i widżetów serwisu Twitter w aplikacji. Aby użyć widżetu usługi Twitter, takiego jak oś czasu użytkownika lub wyniki wyszukiwania dla tego elementu, musisz najpierw utworzyć [widżet w serwisie Twitter](https://twitter.com/settings/widgets). Po utworzeniu widżetu zostanie wyświetlony identyfikator widżetu. Ten identyfikator widżetu jest przekazywany jako parametr podczas wywoływania metod pomocnika, które pokazują widżet.

Ten temat został zapisany w wersji 1,1 interfejsu API usługi Twitter. Bezpośrednio dodając kod pomocnika usługi Twitter do projektu, można zaktualizować kod pomocnika, jeśli interfejs API usługi Twitter ulegnie zmianie.

Aby uzyskać informacje na temat instalowania programu WebMatrix, zobacz [wprowadzenie do stron sieci Web ASP.NET 2 — wprowadzenie](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).

## <a name="add-twitter-helper-to-your-project"></a>Dodawanie pomocnika usługi Twitter do projektu

Aby dodać pomocnika usługi Twitter, najpierw Dodaj folder o nazwie **App\_Code** do projektu. Następnie utwórz plik o nazwie **Twitter. cshtml**.

![App_Code folder](twitter-helper/_static/image1.png)

Zastąp domyślny kod w serwisie Twitter. cshtml poniższym kodem.

[!code-cshtml[Main](twitter-helper/samples/sample1.cshtml)]

## <a name="call-twitter-methods-from-your-web-pages"></a>Wywoływanie metod serwisu Twitter ze stron sieci Web

Poniższy przykład pokazuje, jak używać metod pomocnika usługi Twitter ze strony w projekcie. W projekcie należy zamienić wartości parametrów na wartości, które są istotne dla Twoich potrzeb. Możesz użyć dostarczonych identyfikatorów widżetów, aby dowiedzieć się, jak działają metody, ale chcesz wygenerować własne widżety dla projektu.

Nie wszystkie parametry podane poniżej są wymagane. Parametry opcjonalne służą do dostosowywania sposobu wyświetlania przycisku lub widżetu. Na przykład przycisk Obserwuj wymaga tylko nazwy użytkownika, ale przykład pokazuje, jak uwzględnić liczbę obserwatorów oraz określić rozmiar przycisku i języka.

[!code-html[Main](twitter-helper/samples/sample2.html)]

## <a name="see-the-results"></a>Zobacz wyniki

Powyższy kod tworzy następujące przyciski i widżety. Te przyciski i widżety są w pełni funkcjonalne, a nie zrzuty ekranu. Przycisk Obserwuj jest wyświetlany w języku hiszpańskim, ponieważ parametr Language został ustawiony na **es**.

### <a name="follow-button"></a>Przycisk Obserwuj

[Obserwuj @aspnet)](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`

### <a name="tweet-button"></a>Przycisk Tweet

`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>` [tweetu](https://twitter.com/share)

### <a name="user-timeline-profile"></a>Oś czasu użytkownika (profil)

[Tweety według @aspnet](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`

### <a name="favorites"></a>Ulubione

[Ulubione tweety według @Microsoft](https://twitter.com/Microsoft/favorites)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`

### <a name="list"></a>List

[Tweety z @Microsoft/MS\_\_grup użytkowników](https://twitter.com/microsoft/ms-consumer-brands/)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`

### <a name="search"></a>Wyszukiwanie

[Tweety dotyczące &quot;#asp .net&quot;](https://twitter.com/search?q=%23asp.net)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`

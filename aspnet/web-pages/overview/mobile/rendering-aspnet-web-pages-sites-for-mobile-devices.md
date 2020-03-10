---
uid: web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
title: Renderowanie witryn ASP.NET Web Pages (Razor) dla urządzeń przenośnych | Microsoft Docs
author: Rick-Anderson
description: W tym artykule opisano sposób tworzenia stron w witrynie ASP.NET Web Pages (Razor), która będzie odpowiednio renderowana na urządzeniach przenośnych. Dowiesz się, jak to zrobić...
ms.author: riande
ms.date: 02/17/2014
ms.assetid: f15ab392-c05e-4269-83bf-7c6d2b8c8ec8
msc.legacyurl: /web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
msc.type: authoredcontent
ms.openlocfilehash: c012348d65e48a275cb0e4808fef2a7f31e5fb33
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78563568"
---
# <a name="rendering-aspnet-web-pages-razor-sites-for-mobile-devices"></a>Renderowanie witryn ASP.NET Web Pages (Razor) dla urządzeń przenośnych

Autor [FitzMacken](https://github.com/tfitzmac)

> W tym artykule opisano sposób tworzenia stron w witrynie ASP.NET Web Pages (Razor), która będzie odpowiednio renderowana na urządzeniach przenośnych.
> 
> Zawartość:
> 
> - Jak używać konwencji nazewnictwa, aby określić, że strona jest zaprojektowana specjalnie dla urządzeń przenośnych.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używane w samouczku
> 
> 
> - ASP.NET strony sieci Web (Razor) 3
>   
> 
> Ten samouczek działa również z ASP.NET Web Pages 2.

Strony sieci Web ASP.NET umożliwiają tworzenie niestandardowych ekranów do renderowania zawartości na urządzeniach przenośnych i innych.

Najprostszym sposobem utworzenia strony specyficznej dla urządzenia w witrynie ASP.NET Web Pages jest użycie wzorca nazewnictwa plików, takiego jak: *filename. Mobile. cshtml*. Można utworzyć dwie wersje strony (na przykład jeden nazwany *plik. cshtml* i jeden nazwany *plik. Mobile. cshtml*). W czasie wykonywania, gdy urządzenie przenośne zażąda *pliku. cshtml*, ASP.NET renderuje zawartość z *pliku. Mobile. cshtml*. W przeciwnym razie *plik. cshtml* jest renderowany.

Poniższy przykład pokazuje, jak włączyć renderowanie mobilne przez dodanie strony zawartości dla urządzeń przenośnych. *Strona1. cshtml* zawiera zawartość i pasek boczny nawigacji. *Strona1. Mobile. cshtml* zawiera tę samą zawartość, ale pomija pasek boczny.

1. W witrynie ASP.NET Web Pages Utwórz plik o nazwie *Strona1. cshtml* i Zastąp bieżącą zawartość następującym znacznikiem.

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample1.html)]
2. Utwórz plik o nazwie *Strona1. Mobile. cshtml* i Zastąp istniejącą zawartość następującym znacznikiem. Zwróć uwagę, że wersja aplikacji mobilnej pomija sekcję nawigacji dla lepszego renderowania na mniejszym ekranie.

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample2.html)]
3. Uruchom przeglądarkę pulpitu i przejdź do elementu *Strona1. cshtml*. ![mobilesites-1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)
4. Uruchom przeglądarkę mobilną (lub emulator urządzenia przenośnego) i przejdź do pliku *Strona1. cshtml*. (Należy zauważyć, że nie zawieramy *. Mobile.* w ramach adresu URL). Mimo że żądanie ma wartość *Strona1. cshtml*, ASP.NET renderuje *Strona1. Mobile. cshtml*.

    ![mobilesites-2](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image2.png)

> [!NOTE]
> Aby przetestować strony mobilne, można użyć symulatora urządzenia przenośnego, który działa na komputerze stacjonarnym. To narzędzie umożliwia testowanie stron sieci Web w taki sposób, w jaki będą wyglądały na urządzeniach przenośnych (zazwyczaj z znacznie mniejszym obszarem wyświetlania). Jednym z przykładów symulatora jest [dodatek agent użytkownika](http://addons.mozilla.org/firefox/addon/user-agent-switcher/) dla przeglądarki Mozilla Firefox, który pozwala emulować różne przeglądarki dla urządzeń przenośnych z wersji klasycznej programu Firefox.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Dodatkowe materiały

[Emulator Windows Phone](https://msdn.microsoft.com/library/ff402563(v=VS.92).aspx)

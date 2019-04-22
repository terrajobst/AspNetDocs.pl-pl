---
uid: web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
title: Renderowania ASP.NET Web Pages witryny (Razor) dla urządzeń przenośnych | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: 'W tym artykule opisano sposób tworzenia stron w witrynie ASP.NET Web Pages (Razor), która będzie renderowana odpowiednio na urządzeniach przenośnych. Zawartość: Jak można...'
ms.author: riande
ms.date: 02/17/2014
ms.assetid: f15ab392-c05e-4269-83bf-7c6d2b8c8ec8
msc.legacyurl: /web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
msc.type: authoredcontent
ms.openlocfilehash: dbcd25331387f8606343e551302bc3ed1f9b2c25
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/17/2019
ms.locfileid: "59379511"
---
# <a name="rendering-aspnet-web-pages-razor-sites-for-mobile-devices"></a>Renderowanie witrynach ASP.NET Web Pages (Razor) dla urządzeń przenośnych

przez [Tom FitzMacken](https://github.com/tfitzmac)

> W tym artykule opisano sposób tworzenia stron w witrynie ASP.NET Web Pages (Razor), która będzie renderowana odpowiednio na urządzeniach przenośnych.
> 
> Zawartość:
> 
> - Jak używać konwencji nazewnictwa, aby określić, że strona jest zaprojektowany specjalnie dla urządzeń przenośnych.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używanego w tym samouczku
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> W tym samouczku współpracuje również z wzorca ASP.NET Web Pages 2.


Strony ASP.NET Web Pages umożliwia tworzenie niestandardowych służy do renderowania zawartości na telefon komórkowy lub innych urządzeń.

Najprostszym sposobem utworzenia strony specyficznych dla urządzenia w witrynie ASP.NET Web Pages jest przy użyciu wzorca nazewnictwa plików następująco: *FileName.Mobile.cshtml*. Możesz utworzyć dwie wersje strony (na przykład jeden o nazwie *MyFile.cshtml* i jedną o nazwie *MyFile.Mobile.cshtml*). W czasie, gdy urządzenie przenośne żąda wykonywania *MyFile.cshtml*, platforma ASP.NET renderuje zawartość z *MyFile.Mobile.cshtml*. W przeciwnym razie *MyFile.cshtml* jest renderowany.

Poniższy przykład pokazuje, jak umożliwiające renderowanie mobilnych, dodając strony zawartości dla urządzeń przenośnych. *Page1.cshtml* zawiera zawartość oraz pasek boczny nawigacji. *Page1.Mobile.cshtml* zawiera tę samą zawartość, ale pomija pasku bocznym.

1. W witrynie ASP.NET Web Pages, Utwórz plik o nazwie *Page1.cshtml* i Zastąp zawartość bieżącego następujące znaczniki.

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample1.html)]
2. Utwórz plik o nazwie *Page1.Mobile.cshtml* i zastąpić istniejącą zawartość następującym kodem. Należy zauważyć, że mobilnej wersji strony pomija lepsze renderowanie na mniejsze ekranu można znaleźć w sekcji nawigacji.

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample2.html)]
3. Uruchom przeglądarkę dla komputerów, a następnie przejdź do *Page1.cshtml*. ![mobilesites-1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)
4. Uruchom w przeglądarce dla urządzeń przenośnych (lub w emulatorze urządzenia przenośnego), a następnie przejdź do *Page1.cshtml*. (Zwróć uwagę, że nie zostanie uwzględniony *.mobile.* jako część adresu URL). Nawet jeśli żądanie dotyczy *Page1.cshtml*, platforma ASP.NET renderuje *Page1.Mobile.cshtml*.

    ![mobilesites-2](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image2.png)

> [!NOTE]
> Aby przetestować stron dla urządzeń przenośnych, można użyć symulatora urządzenia przenośnego, uruchomionym na komputerze stacjonarnym. To narzędzie umożliwia testowanie stron sieci web tak, jak będą wyglądały na urządzeniach przenośnych (oznacza to, zwykle za pomocą znacznie mniejszy powoduje wyświetlenie obszaru). Przykład symulatora [przełącznik agenta użytkownika dodatku](http://addons.mozilla.org/firefox/addon/user-agent-switcher/) dla Mozilla Firefox, który umożliwia emulowanie różnych przeglądarek dla urządzeń przenośnych z wersji klasycznej programu Firefox.


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Dodatkowe zasoby


[Emulator Windows Phone](https://msdn.microsoft.com/library/ff402563(v=VS.92).aspx)

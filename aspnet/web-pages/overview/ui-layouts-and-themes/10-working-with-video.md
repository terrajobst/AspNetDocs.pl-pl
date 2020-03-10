---
uid: web-pages/overview/ui-layouts-and-themes/10-working-with-video
title: Wyświetlanie wideo w witrynie ASP.NET Web Pages (Razor) | Microsoft Docs
author: Rick-Anderson
description: W tym rozdziale wyjaśniono, jak wyświetlać wideo na stronach sieci Web ASP.NET za pomocą strony składnia Razor.
ms.author: riande
ms.date: 02/20/2014
ms.assetid: 332fb3da-e2a5-460d-bb90-dd911e1e2c95
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/10-working-with-video
msc.type: authoredcontent
ms.openlocfilehash: 516d46f38ce8910209f4207c474b0404bf012950
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78628948"
---
# <a name="displaying-video-in-an-aspnet-web-pages-razor-site"></a>Wyświetlanie wideo w witrynie ASP.NET Web Pages (Razor)

Autor [FitzMacken](https://github.com/tfitzmac)

> W tym artykule wyjaśniono, jak używać odtwarzacza wideo (multimediów) w witrynie internetowej ASP.NET Web Pages (Razor) w celu umożliwienia użytkownikom wyświetlania filmów wideo przechowywanych w witrynie. ASP.NET Web Pages with składnia Razor umożliwia odtwarzanie filmów Flash (*SWF*), Media Player (*WMV*) i Silverlight (*XAP*).
> 
> Zawartość:
> 
> - Jak wybrać odtwarzacz wideo.
> - Jak dodać wideo do strony sieci Web.
> - Jak ustawić atrybuty odtwarzacza wideo.
> 
> Są to funkcje stron Razor ASP.NET wprowadzone w artykule:
> 
> - Pomocnik `Video`.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używane w samouczku
> 
> 
> - ASP.NET strony sieci Web (Razor) 2
> - WebMatrix 2
>   
> 
> Ten samouczek współpracuje również z programem WebMatrix 3.

## <a name="introduction"></a>Wprowadzenie

Możesz chcieć wyświetlić wideo w swojej witrynie. Jednym ze sposobów jest utworzenie linku do witryny, która zawiera już wideo, takich jak YouTube. Jeśli chcesz osadzić wideo z tych witryn bezpośrednio na własnych stronach, możesz zwykle pobrać znacznik HTML z witryny, a następnie skopiować go na stronę. Na przykład poniższy przykład pokazuje, jak osadzić wideo w serwisie YouTube:

[!code-html[Main](10-working-with-video/samples/sample1.html?highlight=10-14)]

Jeśli chcesz odtworzyć wideo znajdujące się we własnej witrynie sieci Web (nie w publicznej witrynie udostępniania wideo), nie możesz bezpośrednio połączyć się z nią przy użyciu osadzonych znaczników, takich jak ten. Można jednak odtwarzać wideo z witryny przy użyciu pomocnika `Video`, który renderuje odtwarzacz multimedialny bezpośrednio na stronie.

<a id="Choosing_a_Video_Player"></a>
## <a name="choosing-a-video-player"></a>Wybieranie odtwarzacza wideo

Istnieje wiele formatów plików wideo, a każdy format zazwyczaj wymaga innego odtwarzacza i innego sposobu konfigurowania odtwarzacza. Na stronach ASP.NET Razor można odtwarzać wideo na stronie sieci Web przy użyciu pomocnika `Video`. Pomocnik `Video` upraszcza proces osadzania wideo na stronie sieci Web, ponieważ automatycznie generuje `object` i `embed` elementy HTML, które są zwykle używane do dodawania wideo do strony.

Pomocnik `Video` obsługuje następujące odtwarzacze multimedialne:

- Adobe Flash
- MediaPlayer systemu Windows
- Microsoft Silverlight

### <a name="the-flash-player"></a>Program Flash Player

`Flash` Player pomocnika `Video` umożliwia odtwarzanie filmów wideo Flash (plików*SWF* ) na stronie sieci Web. Należy podać co najmniej ścieżkę do pliku wideo. Jeśli określisz Nothing, ale ścieżka, odtwarzacz użyje wartości domyślnych ustawionych przez bieżącą wersję programu Flash. Typowe ustawienia domyślne to:

- Film wideo jest wyświetlany z domyślną szerokością i wysokością i bez koloru tła.
- Film wideo jest odtwarzany automatycznie po załadowaniu strony.
- Film wideo jest ciągle pętli do momentu, gdy zostanie jawnie zatrzymany.
- Film wideo jest skalowany do wyświetlania całego wideo zamiast przycinania wideo w celu dopasowania go do określonego rozmiaru.
- Film wideo jest odtwarzany w oknie.

### <a name="the-mediaplayer-player"></a>Odtwarzacz MediaPlayer

`MediaPlayer` Player pomocnika `Video` umożliwia odtwarzanie filmów wideo w systemie Windows Media (pliki*WMV* ), audio systemu Windows Media (pliki *. WMA* ) oraz plików MP3 (*MP3* ) na stronie sieci Web. Musisz dołączyć ścieżkę pliku multimedialnego do odtwarzania; wszystkie inne parametry są opcjonalne. W przypadku określenia tylko ścieżki odtwarzacz używa ustawień domyślnych ustawionych przez bieżącą wersję MediaPlayer, na przykład:

- Film wideo jest wyświetlany z domyślną szerokością i wysokością.
- Film wideo jest odtwarzany automatycznie po załadowaniu strony.
- Film wideo jest odtwarzany raz (pętla nie działa).
- W odtwarzaczu jest wyświetlany pełen zestaw kontrolek w interfejsie użytkownika.
- Film wideo jest odtwarzany w oknie.

### <a name="the-silverlight-player"></a>Odtwarzacz Silverlight

`Silverlight` Player pomocnika `Video` umożliwia odtwarzanie filmów Windows Media Video (*WMV* ), Windows Media audio (pliki *. WMA* ) oraz plików MP3 (*MP3* ). Należy ustawić parametr path, aby wskazywał pakiet aplikacji oparty na technologii Silverlight (plik *. xap* ). Należy również ustawić parametry width i height. Wszystkie inne parametry są opcjonalne. Jeśli używasz odtwarzacza Silverlight do wideo, jeśli ustawisz tylko wymagane parametry, odtwarzacz Silverlight wyświetli film wideo bez koloru tła.

> [!NOTE]
> Jeśli nie znasz jeszcze Silverlight: plik *XAP* jest skompresowanym plikiem, który zawiera instrukcje układu w pliku *XAML* , kod zarządzany w zestawach i zasoby opcjonalne. Plik *XAP* można utworzyć w programie Visual Studio jako projekt aplikacji Silverlight.

Odtwarzacz wideo `Silverlight` używa zarówno ustawień dostarczonych dla odtwarzacza, jak i ustawień, które są dostępne w pliku *XAP* .

> [!TIP] 
> 
> <a id="SB_MimeTypes"></a>
> ### <a name="mime-types"></a>Typy MIME
> 
> Gdy przeglądarka pobiera plik, przeglądarka upewnia się, że typ pliku pasuje do typu MIME określonego dla renderowanego dokumentu. Typ MIME jest typem zawartości lub typem nośnika pliku. Pomocnik `Video` używa następujących typów MIME:
> 
> - `application/x-shockwave-flash`
> - `application/x-mplayer2`
> - `application/x-silverlight-2`

<a id="Playing_Flash"></a>
## <a name="playing-flash-swf-videos"></a>Odtwarzanie filmów wideo z Flash (SWF)

Ta procedura pokazuje, jak odtworzyć wideo Flash o nazwie *Sample. swf*. W procedurze przyjęto założenie, że w witrynie znajduje się folder o nazwie *multimedia* , a plik *SWF* znajduje się w tym folderze.

1. Dodaj bibliotekę pomocników sieci Web ASP.NET do witryny internetowej zgodnie z opisem w temacie [Instalowanie pomocników w witrynie ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), jeśli jeszcze nie została dodana.
2. W witrynie sieci Web Dodaj stronę i nadaj jej nazwę *FlashVideo. cshtml*.
3. Dodaj do strony następujący znacznik: 

    [!code-cshtml[Main](10-working-with-video/samples/sample2.cshtml)]
4. Uruchom stronę w przeglądarce. (Upewnij się, że strona została wybrana w obszarze roboczym **pliki** przed jej uruchomieniem). Zostanie wyświetlona strona, a film wideo jest odtwarzany automatycznie. 

    ![Image](10-working-with-video/_static/image1.jpg "ch08_video -1. jpg")

Można ustawić `quality` parametru wideo Flash do `low`, `autolow`, `autohigh`, `medium`, `high`i `best`:

[!code-cshtml[Main](10-working-with-video/samples/sample3.cshtml)]

Możesz zmienić wideo Flash, aby odtworzyć o określonym rozmiarze przy użyciu parametru `scale`, który można ustawić w następujący sposób:

- `showall`. To sprawia, że całe wideo jest widoczne przy zachowaniu oryginalnego współczynnika proporcji. Można jednak na każdej stronie zakończyło się obramowaniem.
- `noorder`. To skaluje wideo przy zachowaniu oryginalnego współczynnika proporcji, ale może być przycięty.
- `exactfit`. Dzięki temu całe wideo jest widoczne bez zachowania oryginalnego współczynnika proporcji, ale może wystąpić zniekształcenie.

Jeśli nie określisz parametru `scale`, całe wideo będzie widoczne i oryginalny współczynnik proporcji będzie utrzymywany bez przycinania. Poniższy przykład pokazuje, jak używać `scale` parametru:

[!code-cshtml[Main](10-working-with-video/samples/sample4.cshtml)]

Program Flash Player obsługuje ustawienie trybu wideo o nazwie `windowMode`. Możesz ustawić tę wartość na `window`, `opaque`i `transparent`. Domyślnie `windowMode` jest ustawiona na `window`, co spowoduje wyświetlenie wideo w osobnym oknie na stronie sieci Web. Ustawienie `opaque` ukrywa wszystko za wideo na stronie sieci Web. Ustawienie `transparent` umożliwia tło strony sieci Web wyświetlanej przez wideo, przy założeniu, że jakakolwiek część filmu wideo jest niewidoczna.

<a id="Playing_MediaPlayer"></a>
## <a name="playing-mediaplayer-wmv-videos"></a>Odtwarzanie filmów wideo z MediaPlayer ( *. wmv*)

Poniższa procedura pokazuje, jak odtworzyć film wideo z systemem Windows o nazwie *Sample. wmv* , który znajduje się w folderze *Media* .

1. Dodaj bibliotekę pomocników sieci Web ASP.NET do witryny sieci Web zgodnie z opisem w temacie [Instalowanie pomocników w witrynie ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372)(jeśli jeszcze nie zostało to zrobione).
2. Utwórz nową stronę o nazwie *MediaPlayerVideo. cshtml*.
3. Dodaj do strony następujący znacznik: 

    [!code-cshtml[Main](10-working-with-video/samples/sample5.cshtml)]
4. Uruchom stronę w przeglądarce. Film wideo jest ładowany i odtwarzany automatycznie. 

    ![Image](10-working-with-video/_static/image2.jpg "ch08_video -2. jpg")

Można ustawić `playCount` na liczbę całkowitą, która wskazuje, ile razy ma być odtwarzane wideo:

[!code-cshtml[Main](10-working-with-video/samples/sample6.cshtml)]

Parametr `uiMode` pozwala określić, które kontrolki będą wyświetlane w interfejsie użytkownika. Można ustawić `uiMode` na `invisible`, `none`, `mini`lub `full`. Jeśli nie określisz parametru `uiMode`, film wideo będzie wyświetlany z oknem stanu, paskiem wyszukiwania, przyciskami sterowania i kontrolkami głośności obok okna wideo. Te kontrolki zostaną również wyświetlone, jeśli używasz odtwarzacza do odtwarzania pliku dźwiękowego. Oto przykład sposobu użycia `uiMode` parametru:

[!code-cshtml[Main](10-working-with-video/samples/sample7.cshtml)]

Domyślnie dźwięk jest włączony, gdy wideo jest odtwarzane. Możesz wyciszyć dźwięk, ustawiając parametr `mute` na true:

[!code-cshtml[Main](10-working-with-video/samples/sample8.cshtml)]

Możesz kontrolować poziom audio wideo MediaPlayer, ustawiając parametr `volume` na wartość z zakresu od 0 do 100. Wartość domyślna to 50. Oto przykład:

[!code-cshtml[Main](10-working-with-video/samples/sample9.cshtml)]

<a id="Playing_Silverlight"></a>
## <a name="playing-silverlight-videos"></a>Odtwarzanie filmów wideo Silverlight

Ta procedura pokazuje, jak odtworzyć wideo zawarte na stronie Silverlight *. xap* , która znajduje się w folderze o nazwie *multimedia*.

1. Dodaj bibliotekę pomocników sieci Web ASP.NET do witryny sieci Web zgodnie z opisem w temacie [Instalowanie pomocników w witrynie ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372)(jeśli jeszcze nie zostało to zrobione).
2. Utwórz nową stronę o nazwie *SilverlightVideo. cshtml*.
3. Dodaj do strony następujący znacznik: 

    [!code-cshtml[Main](10-working-with-video/samples/sample10.cshtml)]
4. Uruchom stronę w przeglądarce. 

    ![Image](10-working-with-video/_static/image3.jpg "ch08_video -3. jpg")

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Dodatkowe materiały

[Silverlight — Omówienie](https://msdn.microsoft.com/library/bb404700(VS.95).aspx)

[Atrybuty obiektu Flash i OSADZONEgo znacznika](http://kb2.adobe.com/cps/127/tn_12701.html)

[Tagi PARAM zestawu SDK systemu Windows Media Player 11](https://msdn.microsoft.com/library/aa392321(VS.85).aspx)

---
uid: web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
title: Wyświetlanie map w witrynie ASP.NET Web Pages (Razor) | Microsoft Docs
author: Rick-Anderson
description: W tym artykule wyjaśniono, jak wyświetlać mapy interaktywne na stronach w witrynie internetowej ASP.NET Web Pages (Razor) na podstawie mapowania usług świadczonych przez usługę Bing, Google, ma...
ms.author: riande
ms.date: 02/20/2014
ms.assetid: b5c268dd-ca6a-4562-b94c-a220fcf01f58
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 36f3b753cf312504892872ff54bef49854588990
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78638678"
---
# <a name="displaying-maps-in-an-aspnet-web-pages-razor-site"></a>Wyświetlanie map w witrynie ASP.NET Web Pages (Razor)

Autor [FitzMacken](https://github.com/tfitzmac)

> W tym artykule wyjaśniono, jak wyświetlać mapy interaktywne na stronach w witrynie internetowej ASP.NET Web Pages (Razor) na podstawie mapowania usług świadczonych przez usługę Bing, Google, MapQuest i Yahoo.
> 
> Zawartość:
> 
> - Jak wygenerować mapę na podstawie adresu.
> - Jak wygenerować mapę na podstawie współrzędnych szerokości i długości geograficznej.
> - Jak zarejestrować konto dewelopera map Bing i uzyskać klucz do użycia z mapami Bing.
> 
> Jest to funkcja ASP.NET wprowadzona w artykule:
> 
> - Pomocnik `Maps`.
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

Na stronach sieci Web można wyświetlić mapy na stronie za pomocą pomocnika `Maps`. Mapy można generować na podstawie adresu lub zestawu współrzędnych długości i szerokości geograficznej. Klasa `Maps` umożliwia wywoływanie popularnych aparatów mapy, w tym Bing, Google, MapQuest i Yahoo.

Kroki umożliwiające dodanie mapowania do strony są takie same, niezależnie od tego, które z nich są wywoływane. Wystarczy dodać odwołanie do pliku JavaScript, które udostępnia metody dostępne do wyświetlania mapy, a następnie wywołuje metody pomocnika `Maps`.

Wybierz usługę mapy opartą na tym, która `Maps` używana metoda pomocnika. Można użyć dowolnego z nich:

- `Maps.GetBingHtml`
- `Maps.GetGoogleHtml`
- `Maps.GetYahooHtml`
- `Maps.GetMapQuestHtml`

## <a name="installing-the-pieces-you-need"></a>Instalowanie potrzebnych fragmentów

Aby wyświetlić mapy, potrzebne są następujące elementy:

- Pomocnik `Maps`. Ten pomocnik znajduje się w wersji 2 biblioteki pomocników sieci Web ASP.NET. Jeśli biblioteka nie została jeszcze dodana, można ją zainstalować w swojej lokacji jako pakiet NuGet. Aby uzyskać szczegółowe informacje, zobacz [Instalowanie pomocników w witrynie ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372). (W galerii Wyszukaj pakiet `microsoft-web-helpers`).
- Biblioteka jQuery. Kilka szablonów witryn programu WebMatrix zawiera już biblioteki jQuery w ich folderach *skryptów* . Jeśli nie masz tych bibliotek, możesz pobrać najnowszą bibliotekę jQuery bezpośrednio z witryny [jQuery.org](http://jQuery.org) . Można też utworzyć nową lokację przy użyciu szablonu (na przykład szablonu **witryny startowej** ), a następnie skopiować pliki jQuery z tej lokacji do bieżącej lokacji.

Na koniec Jeśli chcesz korzystać z usługi mapy Bing, musisz najpierw utworzyć konto (bezpłatne) i uzyskać klucz. Aby uzyskać klucz, wykonaj następujące kroki:

1. Utwórz konto na [koncie dewelopera mapy usługi Bing](https://www.microsoft.com/maps/developers/web.aspx). Musisz również mieć konto Microsoft (identyfikator Windows Live).

    Możesz określić, że chcesz użyć klucza do **oceny/testowania**. Jeśli testujesz funkcję mapowania na własnym komputerze przy użyciu programu WebMatrix i IIS Express, przejdź do obszaru roboczego **witryny** i ZANOTUJ adres URL witryny (na przykład `http://localhost:50408`, mimo że numer portu będzie prawdopodobnie inny). Ten adres *localhost* można użyć jako witryny podczas rejestrowania.
2. Po zarejestrowaniu konta przejdź do centrum konta mapy Bing i kliknij pozycję **Utwórz lub Wyświetl klucze**:

    ![Mapowanie — 2](displaying-maps-in-an-aspnet-web-pages-site/_static/image1.png)
3. Zapisz klucz, który tworzy Bing.

## <a name="creating-a-map-based-on-an-address-using-google"></a>Tworzenie mapy na podstawie adresu (przy użyciu usługi Google)

Poniższy przykład pokazuje, jak utworzyć stronę, która renderuje mapę na podstawie adresu. Ten przykład pokazuje, jak używać usługi Google Maps.

1. Utwórz plik o nazwie *mapaddress. cshtml* w folderze głównym witryny. Ta strona generuje mapę na podstawie przekazanego adresu.
2. Skopiuj następujący kod do pliku, zastępując istniejącą zawartość.

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    Zwróć uwagę na następujące funkcje strony:

    - Element `<script>` w elemencie `<head>`. W tym przykładzie element `<script>` odwołuje się do pliku *jQuery-1.6.4. min. js* , który jest wersją zminimalizowanego (skompresowaną) biblioteki jQuery, w wersji 1.6.4. Zwróć uwagę na to, że plik *js* znajduje się w folderze *skryptów* w witrynie. 

        > [!NOTE]
        > Jeśli używasz innej wersji biblioteki jQuery, upewnij się, że jest ona poprawna.
    - Wywołanie `@Maps.GetGoogleHtml` w treści strony. Aby zmapować adres, należy przekazać ciąg adresu. Metody dla innych aparatów mapy działają w podobny sposób (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).
3. Uruchom stronę i wprowadź adres. Na stronie zostanie wyświetlona mapa oparta na usłudze mapy Google, która zawiera określoną lokalizację.

     ![mapowanie — 1](displaying-maps-in-an-aspnet-web-pages-site/_static/image2.png)

## <a name="creating-a-map-based-on-latitude-and-longitude-coordinates-using-bing"></a>Tworzenie mapy na podstawie współrzędnych szerokości i długości geograficznej (przy użyciu usługi Bing)

Ten przykład pokazuje, jak utworzyć mapę na podstawie współrzędnych. Ten przykład pokazuje, jak używać map Bing i jak uwzględnić klucz Bing. (Mapę można utworzyć na podstawie współrzędnych przy użyciu innych aparatów mapy również, bez użycia klucza Bing).

1. Utwórz plik o nazwie *MapCoordinates. cshtml* w folderze głównym lokacji i Zastąp istniejącą zawartość następującym kodem i znacznikiem:

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample2.cshtml)]
2. Zamień `your-key-here` na wygenerowany wcześniej klucz usługi mapy Bing.
3. Uruchom stronę *MapCoordinates. cshtml* , wprowadź współrzędne szerokości geograficznej i długości geograficznej, a następnie kliknij przycisk **Mapa!** Dodaj... (Jeśli nie znasz żadnych współrzędnych, spróbuj wykonać poniższe czynności. Jest to lokalizacja w ramach kampusu Microsoft Redmond.)

   - Szerokość geograficzna: 47.6781005859375
   - Longitude: -122.158317565918

     Strona zostanie wyświetlona przy użyciu określonych współrzędnych.

     ![Mapowanie — 3](displaying-maps-in-an-aspnet-web-pages-site/_static/image3.png)

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Dodatkowe materiały

[Dokumentacja interfejsu API Microsoft. Maps](https://msdn.microsoft.com/library/gg427611.aspx)

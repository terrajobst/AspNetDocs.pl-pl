---
uid: mvc/overview/performance/bundling-and-minification
title: Dzielenie i Minifikacja | Microsoft Docs
author: Rick-Anderson
description: Tworzenie i minifikacja to dwie techniki, których można użyć w ASP.NET 4,5, aby zwiększyć czas ładowania żądania. Przydzielenie i minifikacja zwiększa czas ładowania przez Reducin...
ms.author: riande
ms.date: 08/23/2012
ms.assetid: 5894dc13-5d45-4dad-8096-136499120f1d
msc.legacyurl: /mvc/overview/performance/bundling-and-minification
msc.type: authoredcontent
ms.openlocfilehash: 239980d747c6e0d6be1e9b4fe0371e276e37cf21
ms.sourcegitcommit: 88fc80e3f65aebdf61ec9414810ddbc31c543f04
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/22/2020
ms.locfileid: "76519287"
---
# <a name="bundling-and-minification"></a>Tworzenie pakietów i minimalizowanie

Autor [Rick Anderson]((https://twitter.com/RickAndMSFT))

> Tworzenie i minifikacja to dwie techniki, których można użyć w ASP.NET 4,5, aby zwiększyć czas ładowania żądania. Przydzielenie i minifikacja skraca czas ładowania przez zredukowanie liczby żądań do serwera i zmniejszenie rozmiaru żądanych zasobów (takich jak CSS i JavaScript).

Większość aktualnie poważniejszych przeglądarek ogranicza liczbę [jednoczesnych połączeń](http://www.browserscope.org/?category=network) każdej nazwy hosta do sześciu. Oznacza to, że podczas przetwarzania sześciu żądań dodatkowe żądania dotyczące zasobów na hoście będą umieszczane w kolejce przez przeglądarkę. Na poniższej ilustracji znajdują się karty sieciowe narzędzi deweloperskich programu IE, które są wymagane przez widok informacje aplikacji przykładowej.

![B/M](bundling-and-minification/_static/image1.png)

Szare paski pokazują czas umieszczenia żądania w kolejce przez przeglądarkę, oczekując na sześć limitów połączeń. Żółty pasek to czas żądania do pierwszego bajtu, czyli czas potrzebny na wysłanie żądania i odebranie pierwszej odpowiedzi z serwera. Niebieskie słupki przedstawiają czas potrzebny na odebranie danych odpowiedzi z serwera. Możesz kliknąć dwukrotnie element zawartości, aby uzyskać szczegółowe informacje o chronometrażu. Na przykład na poniższym obrazie przedstawiono szczegóły czasu ładowania pliku */scripts/MyScripts/JavaScript6.js* .

![](bundling-and-minification/_static/image2.png)

Na powyższym obrazie przedstawiono zdarzenie **uruchomienia** , które zawiera czas, w którym żądanie zostało umieszczone w kolejce z powodu ograniczenia liczby jednoczesnych połączeń. W takim przypadku żądanie zostało umieszczone w kolejce przez 46 milisekund, czeka na zakończenie innego żądania.

## <a name="bundling"></a>Tworzenia pakietów

Zgrupowanie to nowa funkcja w programie ASP.NET 4,5, która ułatwia łączenie wielu plików w jeden plik i tworzenie ich. Można tworzyć arkusze CSS i JavaScript oraz inne pakiety. Mniejsza liczba plików oznacza mniejszą liczbę żądań HTTP i może zwiększyć wydajność pierwszej strony.

Na poniższej ilustracji przedstawiono ten sam widok chronometrażu wyświetlanego poprzednio, ale tym razem z włączonym dzieleniem i minifikacja.

![](bundling-and-minification/_static/image3.png)

## <a name="minification"></a>Minifikacja

Minifikacja wykonuje wiele różnych optymalizacji kodu do skryptów lub CSS, takich jak usuwanie zbędnych białych znaków i komentarzy oraz skracanie nazw zmiennych do jednego znaku. Weź pod uwagę następującą funkcję języka JavaScript.

[!code-javascript[Main](bundling-and-minification/samples/sample1.js)]

Po minifikacja funkcja zostaje zredukowana do następujących:

[!code-javascript[Main](bundling-and-minification/samples/sample2.js)]

Oprócz usuwania komentarzy i niepotrzebnych białych znaków, nazwy następujących parametrów i zmiennych zostały zmienione (skrócone) w następujący sposób:

| **Oryginał** | **Zmiany** |
| --- | --- |
| imageTagAndImageID | n |
| imageContext | t |
| imageElement | mogę |

## <a name="impact-of-bundling-and-minification"></a>Wpływ tworzenia i Minifikacja

W poniższej tabeli przedstawiono kilka ważnych różnic między wyświetlaniem wszystkich zasobów osobno i użyciem grupowania i minifikacja (B/M) w programie przykładowym.

|  | **Przy użyciu B/M** | **Bez B/M** | **Zmień** |
| --- | --- | --- | --- |
| **Żądania plików** | 9 | 34 | 256% |
| **KB Sent** | 3.26 | 11.92 | 266% |
| **Odebrano KB** | 388.51 | 530 | 36% |
| **Czas ładowania** | 510 MS | 780 MS | 53% |

Wysłane bajty mają znaczącą redukcję, ponieważ przeglądarki są w pełni pełne, z nagłówkami HTTP, które są stosowane na żądaniach. Redukcja odebranych bajtów nie jest tak duża, ponieważ największe pliki (*skrypty\\jQuery-UI-1.8.11. min. js* i *scripts\\jQuery-1.7.1. min. js*) są już zminimalizowanego. Uwaga: chronometraż dla przykładowego programu użył narzędzia [programu Fiddler](http://www.fiddler2.com/fiddler2/) w celu symulowania wolnej sieci. (Z menu **reguły** programu Fiddler wybierz pozycję **wydajność** , a następnie **Symuluj szybkość modemów**).

## <a name="debugging-bundled-and-minified-javascript"></a>Debugowanie powiązane i zminimalizowanego JavaScript

Debugowanie kodu JavaScript w środowisku deweloperskim (gdzie [element kompilacji](https://msdn.microsoft.com/library/s10awwz0.aspx) w pliku *Web. config* jest ustawiony na `debug="true"`), ponieważ pliki JavaScript nie są powiązane lub zminimalizowanego. Możesz również debugować kompilację wydania, w której pliki JavaScript są powiązane i zminimalizowanego. Korzystając z narzędzi deweloperskich programu IE F12, można debugować funkcję języka JavaScript zawartą w pakiecie zminimalizowanego przy użyciu następującej metody:

1. Wybierz kartę **skrypt** , a następnie wybierz przycisk **Rozpocznij debugowanie** .
2. Wybierz pakiet zawierający funkcję języka JavaScript, która ma być debugowana przy użyciu przycisku elementy zawartości.  
    ![](bundling-and-minification/_static/image4.png)
3. Sformatuj zminimalizowanego JavaScript, wybierając **przycisk konfiguracji** ![](bundling-and-minification/_static/image5.png), a następnie wybierając **Format JavaScript**.
4. W polu wejściowym **skrypt wyszukiwania** wybierz nazwę funkcji, którą chcesz debugować. Na poniższej ilustracji **AddAltToImg** został wprowadzony w polu wejściowym **skryptu wyszukiwania** .  
    ![](bundling-and-minification/_static/image6.png)

Aby uzyskać więcej informacji na temat debugowania za pomocą narzędzi deweloperskich F12, zobacz artykuł MSDN [przy użyciu narzędzia deweloperskie F12, aby debugować błędy JavaScript](https://msdn.microsoft.com/library/ie/gg699336(v=vs.85).aspx).

## <a name="controlling-bundling-and-minification"></a>Kontrolowanie grupowania i Minifikacja

Dzielenie i minifikacja jest włączone lub wyłączone przez ustawienie wartości atrybutu Debug w [elemencie compilation](https://msdn.microsoft.com/library/s10awwz0.aspx) w pliku *Web. config* . W poniższym kodzie XML `debug` ma ustawioną wartość true, więc dzielenie i minifikacja jest wyłączone.

[!code-xml[Main](bundling-and-minification/samples/sample3.xml?highlight=2)]

Aby włączyć grupowania i minifikacja, ustaw wartość `debug` na "false". Ustawienie *Web. config* można zastąpić właściwością `EnableOptimizations` klasy `BundleTable`. Poniższy kod umożliwia zgrupowanie i minifikacja oraz zastępuje wszystkie ustawienia w pliku *Web. config* .

[!code-csharp[Main](bundling-and-minification/samples/sample4.cs?highlight=7)]

> [!NOTE]
> Jeśli `EnableOptimizations` jest `true` lub atrybut Debug w [elemencie compilation](https://msdn.microsoft.com/library/s10awwz0.aspx) w pliku *Web. config* jest ustawiony na `false`, pliki nie będą powiązane ani zminimalizowanego. Ponadto, minimalna wersja plików nie zostanie użyta, zostaną wybrane pełne wersje debugowania. `EnableOptimizations` przesłania atrybut Debug w [elemencie compilation](https://msdn.microsoft.com/library/s10awwz0.aspx) w pliku *Web. config*

## <a name="using-bundling-and-minification-with-aspnet-web-forms-and-web-pages"></a>Korzystanie z funkcji grupowania i Minifikacja z ASP.NET Web Forms i stronami sieci Web

- W przypadku stron sieci Web zapoznaj się z wpisem w blogu [Dodawanie optymalizacji sieci Web do witryny Web Pages](https://docs.microsoft.com/archive/blogs/rickandy/adding-web-optimization-to-a-web-pages-site).
- W przypadku formularzy sieci Web zapoznaj się z wpisem w blogu [Dodawanie grupowania i minifikacja do formularzy sieci Web](https://docs.microsoft.com/archive/blogs/rickandy/adding-bundling-and-minification-to-web-forms).

## <a name="using-bundling-and-minification-with-aspnet-mvc"></a>Korzystanie z grupowania i Minifikacja z ASP.NET MVC

W tej sekcji utworzymy projekt ASP.NET MVC, który będzie badany i minifikacja. Najpierw utwórz nowy projekt internetowy ASP.NET MVC o nazwie **MvcBM** bez zmiany wartości domyślnych.

Otwórz *\\aplikacji \_uruchom\\plik BundleConfig.cs* i Przeanalizuj metodę `RegisterBundles`, która służy do tworzenia, rejestrowania i konfigurowania pakietów. Poniższy kod przedstawia część metody `RegisterBundles`.

[!code-csharp[Main](bundling-and-minification/samples/sample5.cs)]

Poprzedni kod tworzy nowy pakiet JavaScript o nazwie *~/bundles/jQuery* , który zawiera wszystkie odpowiednie (to jest Debug lub zminimalizowanego, ale nie. *vsdoc*) pliki w folderze *skryptów* , które pasują do ciągu symbolu wieloznacznego "~/scripts/jQuery-{Version}.js". W przypadku ASP.NET MVC 4 oznacza to, że z konfiguracją debugowania plik *jQuery-1.7.1. js* zostanie dodany do pakietu. W konfiguracji wydania zostanie dodany *jQuery-1.7.1. min. js* . Struktura tworzenia pakietów jest następująca:

- Wybieraj plik ". min" do wydania, gdy istnieje *FileX. min. js* i *FileX. js* .
- Wybieranie wersji nieujemnej na potrzeby debugowania.
- Ignorowanie plików "-vsdoc" (takich jak *jQuery-1.7.1-vsdoc. js*), które są używane tylko przez funkcję IntelliSense.

Przedstawione powyżej `{version}` wieloznacznego dopasowania służy do automatycznego tworzenia pakietu jQuery z odpowiednią wersją jQuery w folderze *skryptów* . W tym przykładzie korzystanie z symboli wieloznacznych zapewnia następujące korzyści:

- Umożliwia korzystanie z narzędzia NuGet do aktualizowania do nowszej wersji jQuery bez zmiany powyższego kodu grupowania lub odwołań jQuery na stronach widoku.
- Automatycznie wybiera pełną wersję dla konfiguracji debugowania i wersję ". min" dla kompilacji wydań.

## <a name="using-a-cdn"></a>Korzystanie z sieci CDN

 W poniższym kodzie jest zastępowany lokalny pakiet jQuery z pakietem CDN jQuery.

[!code-csharp[Main](bundling-and-minification/samples/sample6.cs)]

W powyższym kodzie usługa jQuery zostanie zażądana z sieci CDN w trybie wydania, a wersja debugowana jQuery zostanie pobrana lokalnie w trybie debugowania. W przypadku korzystania z sieci CDN należy mieć mechanizm rezerwowy w przypadku niepowodzenia żądania sieci CDN. Poniższy fragment znacznika z końca pliku układu pokazuje skrypt dodany do żądania jQuery w przypadku niepowodzenia sieci CDN.

[!code-cshtml[Main](bundling-and-minification/samples/sample7.cshtml?highlight=5-13)]

## <a name="creating-a-bundle"></a>Tworzenie pakietu

Klasa [pakietu](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx) `Include` Metoda przyjmuje tablicę ciągów, gdzie każdy ciąg jest ścieżką wirtualną do zasobu. Poniższy kod z metody `RegisterBundles` w *aplikacji\\\_uruchom\\plik BundleConfig.cs* pokazuje, jak wiele plików jest dodawanych do pakietu:

[!code-csharp[Main](bundling-and-minification/samples/sample8.cs)]

W celu dodania wszystkich plików w katalogu (i opcjonalnie wszystkich podkatalogów), które pasują do wzorca wyszukiwania, podano metodę `IncludeDirectory` klasy [pakietu](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx) . Poniżej przedstawiono klasę [pakietu](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx) `IncludeDirectory` interfejs API:

[!code-csharp[Main](bundling-and-minification/samples/sample9.cs)]

Pakiety są przywoływane w widokach za pomocą metody Render (`Styles.Render` dla CSS i `Scripts.Render` dla języka JavaScript). Następujące znaczniki w *widokach\\udostępnione\\\_Layout. cshtml* pokazują, jak domyślny widok projektu internetowego ASP.NET odwołuje się do pakietów CSS i JavaScript.

[!code-cshtml[Main](bundling-and-minification/samples/sample10.cshtml?highlight=5-6,11)]

Zauważ, że metody renderowania pobierają tablicę ciągów, aby można było dodać wiele pakietów w jednym wierszu kodu. Zwykle chcesz użyć metod renderowania, które tworzą kod HTML wymagany do odwoływania się do elementu zawartości. Możesz użyć metody `Url`, aby wygenerować adres URL dla zasobu bez znaczników wymaganych do odwołania się do elementu zawartości. Załóżmy, że chciałeś użyć nowego atrybutu [ASYNCHRONICZNEGO](http://www.whatwg.org/specs/web-apps/current-work/#attr-script-async) HTML5. Poniższy kod ilustruje sposób odwoływania się do programu modernizacji przy użyciu metody `Url`.

[!code-cshtml[Main](bundling-and-minification/samples/sample11.cshtml?highlight=11)]

## <a name="using-the--wildcard-character-to-select-files"></a>Używanie znaku wieloznacznego "\*" do wybierania plików

Ścieżka wirtualna określona w metodzie `Include` i wzorzec wyszukiwania w metodzie `IncludeDirectory` może akceptować jeden symbol wieloznaczny "\*" jako prefiks lub sufiks do w ostatnim segmencie ścieżki. W ciągu wyszukiwania jest rozróżniana wielkość liter. Metoda `IncludeDirectory` ma opcję wyszukiwania podkatalogów.

Rozważmy projekt z następującymi plikami JavaScript:

- *Scripts\\Common\\AddAltToImg.js*
- *Skrypty\\Common\\ToggleDiv. js*
- *Skrypty\\Common\\ToggleImg. js*
- *Scripts\\Common\\Sub1\\ToggleLinks.js*

![imag katalogu](bundling-and-minification/_static/image7.png)

W poniższej tabeli przedstawiono pliki dodane do pakietu przy użyciu symbolu wieloznacznego, jak pokazano poniżej:

| **Call** | **Dodano pliki lub zgłoszono wyjątek** |
| --- | --- |
| Include ("~/Scripts/Common/\*. js") | *AddAltToImg.js*, *ToggleDiv.js*, *ToggleImg.js* |
| Include("~/Scripts/Common/T\*.js") | Nieprawidłowy wyjątek wzorca. Symbol wieloznaczny jest dozwolony tylko dla prefiksu lub sufiksu. |
| Include ("~/Scripts/Common/\*og.\*") | Nieprawidłowy wyjątek wzorca. Dozwolony jest tylko jeden znak wieloznaczny. |
| Include ("~/Scripts/Common/T\*") | *ToggleDiv.js*, *ToggleImg.js* |
| Include ("~/Scripts/Common/\*") | Nieprawidłowy wyjątek wzorca. Czysty segment wieloznaczny jest nieprawidłowy. |
| IncludeDirectory ("~/Scripts/Common", "T\*") | *ToggleDiv.js*, *ToggleImg.js* |
| IncludeDirectory ("~/Scripts/Common", "T\*", true) | *ToggleDiv.js*, *ToggleImg.js*, *ToggleLinks.js* |

Jawne dodanie każdego pliku do pakietu jest ogólnie preferowane w porównaniu z wieloznacznym ładowaniem plików z następujących powodów:

- Dodawanie skryptów według symboli wieloznacznych domyślnie do ładowania ich w kolejności alfabetycznej, która zwykle nie jest to wymagane. Pliki CSS i JavaScript często należy dodawać w określonym porządku (innym niż alfabetyczny). Możesz złagodzić to ryzyko poprzez dodanie niestandardowej implementacji [IBundleOrderer](https://msdn.microsoft.com/library/system.web.optimization.ibundleorderer(VS.110).aspx) , ale jawne dodanie każdego pliku jest mniej podatne na błędy. Na przykład możesz dodać nowe zasoby do folderu w przyszłości, co może wymagać zmodyfikowania implementacji [IBundleOrderer](https://msdn.microsoft.com/library/system.web.optimization.ibundleorderer(VS.110).aspx) .
- Wyświetlanie określonych plików dodanych do katalogu przy użyciu funkcji ładowania wieloznacznego może być dołączane do wszystkich widoków odwołujących się do tego pakietu. Jeśli do pakietu zostanie dodany konkretny skrypt, może wystąpić błąd języka JavaScript w innych widokach, które odwołują się do tego pakietu.
- Pliki CSS, które zaimportują inne pliki, powodują dwa razy załadowane zaimportowane pliki. Na przykład poniższy kod tworzy pakiet z większością plików CSS motywu interfejsu użytkownika jQuery załadowanych dwa razy. 

    [!code-csharp[Main](bundling-and-minification/samples/sample12.cs)]

  Selektor wieloznaczny "\*. css" umieszcza w każdym pliku CSS w folderze, w tym *motywy\\zawartości\\base\\jQuery. UI. ALL. css* . Plik *jQuery. UI. ALL. css* importuje inne pliki CSS.

## <a name="bundle-caching"></a>Buforowanie zbioru

Zbiory ustawiają nagłówek wygaśnięcia HTTP rok od momentu utworzenia pakietu. Jeśli przejdziesz do wcześniej wyświetlanej strony, programu Fiddler pokazuje, że program IE nie tworzy warunkowego żądania dla tego pakietu, czyli nie ma żądań HTTP GET od IE dla pakietów i nie zwraca odpowiedzi HTTP 304 z serwera. Można wymusić, aby program IE przetworzył żądanie warunkowe dla każdego pakietu z klawiszem F5 (w wyniku odpowiedzi HTTP 304 dla każdego pakietu). Można wymusić pełne odświeżanie za pomocą polecenia ^ F5 (w wyniku odpowiedzi HTTP 200 dla każdego pakietu).

Na poniższej ilustracji przedstawiono kartę **buforowanie** okienka odpowiedzi programu Fiddler:

![obraz pamięci podręcznej programu Fiddler](bundling-and-minification/_static/image8.png)

Żądanie   
`http://localhost/MvcBM_time/bundles/AllMyScripts?v=r0sLDicvP58AIXN_mc3QdyVvVj5euZNzdsa2N1PKvb81`  
 dotyczy zbioru **AllMyScripts** i zawiera parę ciągów zapytania **v = R0sLDicvP58AIXN\\\_mc3QdyVvVj5euZNzdsa2N1PKvb81**. Ciąg zapytania **v** ma token wartości, który jest unikatowym identyfikatorem używanym do buforowania. Dopóki pakiet nie ulegnie zmianie, aplikacja ASP.NET będzie żądać pakietu **AllMyScripts** przy użyciu tego tokenu. Jeśli dowolny plik w pakiecie ulegnie zmianie, platforma optymalizacji ASP.NET wygeneruje nowy token, co gwarantuje, że żądania przeglądarki dla pakietu będą miały najnowszą wersję.

W przypadku uruchamiania narzędzi programistycznych IE9 F12 i przechodzenia do wcześniej załadowanej strony program IE nieprawidłowo pokazuje warunkowe żądania GET do każdego pakietu i zwraca serwer HTTP 304. Dlaczego IE9 ma problemy z ustaleniem, czy żądanie warunkowe zostało wprowadzone w blogu [przy użyciu sieci CDN i wygasa, aby zwiększyć wydajność witryny sieci Web](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx).

## <a name="less-coffeescript-scss-sass-bundling"></a>LESS, CoffeeScript, SCSS i Sass.

Program minifikacja Framework i platforma udostępniają mechanizm przetwarzania języków pośrednich, takich jak [SCSS](http://sass-lang.com/), [Sass](http://sass-lang.com/), [less](http://www.dotlesscss.org/) lub [CoffeeScript](http://coffeescript.org/), i stosują transformacje, takie jak minifikacja, do pakietu, który jest wynikiem. Na przykład, aby dodać pliki [less](http://www.dotlesscss.org/) do projektu MVC 4:

1. Utwórz folder dla MNIEJSZEj zawartości. W poniższym przykładzie zostanie użyta *zawartość folderu\\less* .
2. Dodaj **bezkropkowo** [pakiet NuGet do projektu.](http://www.dotlesscss.org/)  
    ![instalacji z bez](bundling-and-minification/_static/image9.png) NuGet
3. Dodaj klasę, która implementuje interfejs [IBundleTransform](https://msdn.microsoft.com/library/system.web.optimization.ibundletransform(VS.110).aspx) . W przypadku transformacji. less Dodaj poniższy kod do projektu.

    [!code-csharp[Main](bundling-and-minification/samples/sample13.cs)]
4. Utwórz pakiet mniej plików z `LessTransform` i transformacji [CssMinify](https://msdn.microsoft.com/library/system.web.optimization.cssminify(VS.110).aspx) . Dodaj następujący kod do metody `RegisterBundles` w *aplikacji\\_Start\\pliku BundleConfig.cs* .

    [!code-csharp[Main](bundling-and-minification/samples/sample14.cs)]
5. Dodaj następujący kod do wszystkich widoków, które odwołują się do MNIEJSZEgo pakietu.

    [!code-cshtml[Main](bundling-and-minification/samples/sample15.cshtml)]

## <a name="bundle-considerations"></a>Zagadnienia dotyczące zbioru

Dobrym zwyczajem do przestrzegania podczas tworzenia pakietów jest dołączenie "pakietu" jako prefiksu w nazwie pakietu. Uniemożliwi to [konflikt routingu](https://forums.asp.net/post/5012037.aspx).

Po zaktualizowaniu jednego pliku w pakiecie jest generowany nowy token dla parametru ciągu zapytania pakietu, a pełny pakiet musi zostać pobrany przy następnym zażądaniu strony zawierającej pakiet. W tradycyjnym znaczniku, w którym każdy element zawartości jest wymieniany osobno, zostanie pobrany tylko zmieniony plik. Zasoby, które zmieniają się często, mogą nie być dobrymi kandydatami do tworzenia.

Zgrupowanie i minifikacja przede wszystkim zwiększy czas ładowania żądania pierwszej strony. Po zalogowaniu się do strony sieci Web przeglądarka przechowuje w pamięci podręcznej zasoby (JavaScript, CSS i obrazy), więc zgrupowanie i minifikacja nie zapewni zwiększenia wydajności podczas żądania tej samej strony lub stron w tej samej lokacji żądającej tych samych zasobów. Jeśli nie ustawisz prawidłowo nagłówka wygaśnięcia dla elementów zawartości i nie używasz funkcji grupowania i minifikacja, heurystyka Aktualności przeglądarki oznaczy zasoby jako przestarzałe po kilku dniach, a przeglądarka będzie wymagać żądania weryfikacji dla każdego elementu zawartości. W takim przypadku zgrupowanie i minifikacja zapewniają wzrost wydajności po pierwszym żądaniu strony. Aby uzyskać szczegółowe informacje, zapoznaj się z blogiem [przy użyciu sieci CDN i wygasa, aby zwiększyć wydajność witryny sieci Web](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx).

W celu ograniczenia liczby jednoczesnych połączeń między hostami na każdą z nazw hostów można ograniczyć użycie sieci [CDN](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx). Ponieważ sieć CDN będzie miała inną nazwę hosta niż witryna hostingu, żądania zasobów z sieci CDN nie będą wliczane do sześciu jednoczesnych połączeń do środowiska hostingu. Sieć CDN może również zapewniać typowe zalety buforowania pakietów i buforowania krawędzi.

Pakiety powinny być partycjonowane przez strony, które ich potrzebują. Na przykład domyślny szablon ASP.NET MVC dla aplikacji internetowej tworzy pakiet z walidacją jQuery oddzielony od jQuery. Ponieważ utworzone domyślne widoki nie mają danych wejściowych i nie są ogłaszane, nie zawierają one pakietu walidacji.

Przestrzeń nazw `System.Web.Optimization` jest zaimplementowana w *pliku System. Web. Optimization. dll*. Wykorzystuje bibliotekę websmaring (*Websmars. dll*) dla funkcji minifikacja, które z kolei korzystają z *Antlr3. Runtime. dll*.

*Używam usługi Twitter w celu wprowadzania szybkich ogłoszeń i linków do udostępniania. Moje dojście*do usługi Twitter: [@RickAndMSFT](http://twitter.com/RickAndMSFT)

## <a name="additional-resources"></a>Dodatkowe zasoby

- Wideo:[dzielenie i optymalizacja](https://channel9.msdn.com/Events/aspConf/aspConf/Bundling-and-Optimizing) według [Howard Dierking](https://twitter.com/#!/howard_dierking)
- [Dodawanie optymalizacji sieci Web do witryny Web Pages](https://blogs.msdn.com/b/rickandy/archive/2012/08/15/adding-web-optimization-to-a-web-pages-site.aspx).
- [Dodawanie grupowania i minifikacja do formularzy sieci Web](https://blogs.msdn.com/b/rickandy/archive/2012/08/14/adding-bundling-and-minification-to-web-forms.aspx).
- [Wpływ na wydajność tworzenia i minifikacja w trybie przeglądania sieci Web](https://blogs.msdn.com/b/henrikn/archive/2012/06/17/performance-implications-of-bundling-and-minification-on-http.aspx) przez [Henrik F Nielsen](http://en.wikipedia.org/wiki/Henrik_Frystyk_Nielsen) [@frystyk](https://twitter.com/frystyk)
- [Korzystanie z sieci CDN i wygasa w celu poprawy wydajności witryny sieci Web](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx) przez Rick Anderson [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)
- [Minimalizuj czas RTT (czasy rundy)](https://developers.google.com/speed/docs/best-practices/rtt)

## <a name="contributors"></a>Współautorzy

- Hao Kung
- [Howard Dierking](https://twitter.com/#!/howard_dierking)
- Dianę LaRose

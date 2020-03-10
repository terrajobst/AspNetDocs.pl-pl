---
uid: web-forms/overview/getting-started/hands-on-labs/whats-new-in-aspnet-and-web-development-in-visual-studio-2012
title: Co nowego w ASP.NET i programowanie w sieci Web w programie Visual Studio 2012 | Microsoft Docs
author: rick-anderson
description: Nowa wersja programu Visual Studio wprowadza szereg ulepszeń, które koncentrują się na ulepszaniu środowiska i wydajności podczas pracy z technologiami sieci Web...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 6d40d276-1642-4a77-b6c9-02ac914f6805
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/whats-new-in-aspnet-and-web-development-in-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: 80c77ec65ed86b06e417d3f6ba608e404c46768b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78525936"
---
# <a name="whats-new-in-aspnet-and-web-development-in-visual-studio-2012"></a>Co nowego w platformie ASP.NET i w programowaniu dla Internetu w programie Visual Studio 2012

przez [zespół Camp sieci Web](https://twitter.com/webcamps)

> Nowa wersja programu Visual Studio wprowadza szereg ulepszeń, które koncentrują się na ulepszaniu środowiska i wydajności podczas pracy z technologiami sieci Web. Edytory programu Visual Studio dla CSS, JavaScript i HTML zostały całkowicie odnowionyche w celu uwzględnienia wielu najbardziej zapotrzebowań na żądanie, takich jak IntelliSense i automatyczne wcięcia. W odniesieniu do wydajności, grupowania i minifikacja są teraz zintegrowane jako wbudowane funkcje, aby łatwo skrócić czas ładowania strony.
> 
> Program Visual Studio umożliwia korzystanie z najnowszych technologii witryny sieci Web. Można użyć fragmentów kodu CSS3 w przeglądarce, aby upewnić się, że witryna działa niezależnie od platformy klienta, a także korzysta z nowych elementów i funkcji HTML5.
> 
> Pisanie i profilowanie kodu JavaScript powinno być łatwiejsze w tej wersji programu Visual Studio. Listy IntelliSense, zintegrowana dokumentacja XML i funkcje nawigacji są teraz dostępne dla kodu JavaScript. Masz teraz katalog JavaScript na wyręką. Dodatkowo można sprawdzić zgodność ECMAScript5 ze skryptami i wykrywać błędy składni na wczesnym etapie.
> 
> Ostatnia, ale nie najmniej, ta wersja programu Visual Studio implementuje wbudowaną funkcję grupowania i minifikacja. Pliki skryptów i arkusze stylów zostaną spakowane i skompresowane, aby witryna wykonywała szybszy czas.
> 
> To laboratorium przeprowadzi Cię przez ulepszenia i nowe funkcje opisane wcześniej przez zastosowanie drobnych zmian do przykładowej aplikacji sieci Web, która znajduje się w folderze źródłowym.
> 
> Wszystkie przykładowe kod i fragmenty kodu są zawarte w zestawie szkoleń w sieci Web Camp, które są dostępne w [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).

<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>Cele

W tym ćwiczeniu w laboratorium dowiesz się, jak:

- Korzystanie z nowych funkcji i ulepszeń w edytorze CSS
- Korzystanie z nowych funkcji i ulepszeń w edytorze HTML
- Korzystanie z nowych funkcji i ulepszeń w edytorze JavaScript
- Konfigurowanie i używanie grupowania i minifikacja

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Wymagania wstępne

- [Microsoft Visual Studio Express 2012 dla sieci Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) lub nadrzędnych (Przeczytaj [dodatek A,](#AppendixA) Aby uzyskać instrukcje na temat sposobu jego instalacji).
- [Windows PowerShell](https://support.microsoft.com/kb/968930/) (dla skryptów instalacyjnych — już zainstalowane w systemie Windows 8 i windows Server 2008 R2)
- [Internet Explorer 10](https://windows.microsoft.com/internet-explorer/products/ie/home) lub przeglądarka zgodna z HTML5

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Symulacyjn

Ta funkcja w laboratorium obejmuje następujące ćwiczenia:

1. [Ćwiczenia 1: co nowego w edytorze CSS](#Exercise1)
2. [Ćwiczenie 2: co nowego w edytorze HTML](#Exercise2)
3. [Ćwiczenie 3: co nowego w edytorze JavaScript](#Exercise3)
4. [Ćwiczenie 4: dzielenie i Minifikacja](#Exercise4)

Szacowany czas wykonywania tego laboratorium: **60 minut**.

<a id="Exercise1"></a>

<a id="Exercise_1_Whats_New_in_the_CSS_Editor"></a>
### <a name="exercise-1-whats-new-in-the-css-editor"></a>Ćwiczenia 1: co nowego w edytorze CSS

Deweloperzy sieci Web powinni znać wiele problemów związanych z edycją CSS. Jednym z największych problemów dotyczących stylów CSS jest zgodność między przeglądarkami. Często zdarza się to, że po zastosowaniu stylów do witryny Zauważ, że będzie wyglądać inaczej, jeśli otworzysz ją w innej przeglądarce lub urządzeniu. W związku z tym można poświęcać dużo czasu na rozwiązywanie tych problemów wizualnych, aby osiągnąć, że po zakończeniu pracy w jednej przeglądarce zostanie ona zadzielona innym osobom.

Program Visual Studio zawiera teraz funkcje, które pomagają deweloperom efektywnie uzyskiwać dostęp do arkuszy stylów CSS, pracy i ich organizowania. W tym ćwiczeniu będziesz spełniać nowe funkcje efektywnej organizacji i wydania oraz fragmenty kodu CSS3 na potrzeby zgodności między przeglądarkami.

<a id="Ex1Task1"></a>

<a id="Task_1_-_New_Editor_Features"></a>
#### <a name="task-1---new-editor-features"></a>Zadanie 1 — nowe funkcje edytora

W tym zadaniu zostaną wykryte nowe funkcje edytora CSS. Ten nowy edytor pomoże Ci zwiększyć produktywność dzięki wykorzystaniu nowych, inteligentnych wcięć, ulepszonych komentarzy do kodu i udoskonalonej listy funkcji IntelliSense.

1. Uruchom **program Visual Studio** i Otwórz rozwiązanie **WhatsNewASPNET. sln** znajdujące się w folderze **Source\WhatsNewASPNET** w tym laboratorium.
2. W Eksplorator rozwiązań otwórz plik **site. css** znajdujący się w folderze **Style** . Upewnij się, że narzędzia **edytora tekstu** są widoczne na pasku narzędzi. Aby to zrobić, wybierz opcję menu **wyświetl** | **paski narzędzi** i Sprawdź opcje **edytora tekstu** . Zobaczysz, że od tej nowej wersji przycisk **komentarza** (![](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image1.png) przycisk komentarza) i przycisk Usuń **komentarz** (![usuń komentarz-przycisk](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image2.png)) również są włączone dla edytora CSS.

    ![Włączanie edytora i narzędzi CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image3.png "Włączanie edytora i narzędzi CSS")

    *Włączanie edytora i narzędzi CSS*
3. Przewiń kod i wybierz dowolną definicję klasy CSS. Kliknij przycisk **komentarz** (![](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image4.png) przycisk komentarza), aby dodać komentarz do wybranych wierszy. Następnie kliknij przycisk **Usuń komentarz** (![Usuń komentarz-przycisk](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image5.png)), aby cofnąć zmiany.
4. Kliknij przycisk **Zwiń** (![Zwiń](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image6.png)) i **Rozwiń** (![rozwiń](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image7.png)) znajdujący się na lewym marginesie tekstu. Zauważ, że możesz teraz ukryć style, które nie są używane, aby mieć widok bardziej przejrzysty.

    ![Zwijanie klas CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image8.png "Zwijanie klas CSS")

    *Zwijanie klas CSS*
5. Upewnij się, że funkcja inteligentnego wcięcia jest włączona. Wybierz opcję menu **narzędzia** | **Opcje** , a następnie wybierz pozycję **edytor tekstu** | stronie **Formatowanie** | **CSS** w lewym okienku ekranu. Zaznacz opcję **wcięcia hierarchiczne** .

    ![Włączanie wcięcia hierarchicznego](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image9.png "Włączanie wcięcia hierarchicznego")

    *Włączanie wcięcia hierarchicznego*
6. Znajdź definicję klasy głównej (Main) i Dołącz styl do elementów div. Zauważ, że kod jest wyrównywany automatycznie, pomagając użytkownikom w szybkim wyszukiwaniu klas nadrzędnych.

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample1.css)]

    ![Wyrównanie hierarchiczne w CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image10.png "Wyrównanie hierarchiczne w CSS")

    *Wyrównanie hierarchiczne w CSS*
7. Wewnątrz **. głównej klasy DIV** Znajdź kursor na końcu **obramowania: 0px;** i naciśnij klawisz **Enter** , aby wyświetlić listę funkcji IntelliSense. Zacznij wpisywać tekst **u góry** i Zauważ, jak lista jest filtrowana w trakcie pisania. Na liście zostaną wyświetlone elementy, które zawierają **górną** część wyrazu (we wcześniejszych wersjach programu Visual Studio, lista jest filtrowana według elementów *rozpoczynających* się od terminu).

    ![Udoskonalenia funkcji IntelliSense w CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image11.png "Udoskonalenia funkcji IntelliSense w CSS")

    *Udoskonalenia funkcji IntelliSense w CSS*

<a id="Ex1Task2"></a>

<a id="Task_2_-_The_Color_Picker"></a>
#### <a name="task-2---the-color-picker"></a>Zadanie 2 — Wybór koloru

To zadanie umożliwia odnalezienie nowego selektora kolorów CSS zintegrowanego z funkcją IntelliSense programu Visual Studio.

1. W pliku **site. css** Znajdź definicję klasy nagłówka (. Header) i umieść kursor obok atrybutu **background-color** , między &quot;:&quot; i &quot;#&quot; znaków w tym wierszu kodu **.**

    ![Lokalizowanie kursora](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image12.png "Lokalizowanie kursora")

    *Lokalizowanie kursora*
2. Usuń **dwukropek** (:) i napisz ponownie, aby wyświetlić selektor kolorów. Zwróć uwagę, że pierwsze kolory, które zobaczysz, są najczęściej używane w witrynie. Jeśli klikniesz biały kolor, jego kod koloru HTML (#fff) zastąpi bieżący kod koloru w arkuszu stylów.

    ![Selektor kolorów](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image13.png "Selektor kolorów")

    *Selektor kolorów*
3. Naciśnij przycisk **Rozwiń** (![com](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image14.png)) w Próbniku kolorów, aby wyświetlić gradient koloru, a następnie przeciągnij kursor gradientu, aby wybrać inny kolor. Po wybraniu tej opcji kliknij przycisk **Kroplomierz** i wybierz dowolny kolor z ekranu. Zauważ, że wartość koloru tła jest zmieniana dynamicznie podczas przenoszenia kursora.

    ![Gradient selektora kolorów](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image15.png "Gradient selektora kolorów")

    *Gradient selektora kolorów*
4. W suwaku **nieprzezroczystość** przesuń selektor do środka paska, aby zmniejszyć nieprzezroczystość. Zauważ, że wartość koloru Background zmienia teraz jej skalę na RGBA.

    ![Nieprzezroczystość selektora kolorów](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image16.png "Nieprzezroczystość selektora kolorów")

    *Nieprzezroczystość selektora kolorów*

    > [!NOTE]
    > Definicja koloru RGBA (Red, Green, Blue, Alpha) w CSS3 umożliwia zdefiniowanie wartości przezroczystości koloru dla pojedynczego elementu. W przeciwieństwie do **nieprzejrzystości —** podobny atrybut CSS **-** kolory RGBA są również zgodne z najnowszymi przeglądarkami.

<a id="Ex1Task3"></a>

<a id="Task_3_-_CSS_Compatible_Code_Snippets"></a>
#### <a name="task-3---css-compatible-code-snippets"></a>Zadanie 3 — fragmenty kodu zgodne z CSS

W tym zadaniu dowiesz się, jak używać wielostronicowych fragmentów kodu CSS3 w celu zaimplementowania niektórych funkcji w witrynie sieci Web.

1. W pliku **site. css** Znajdź definicję klasy CSS **nagłówka** (. Header) i umieść kursor poniżej **/\*\*granicy promień /** , aby dodać nowy fragment kodu. Naciśnij klawisz **Enter** , aby wyświetlić listę funkcji IntelliSense i wpisz **promień** , aby przefiltrować listę. Wybierz opcję **border-radius** z listy przy użyciu podwójnego kliknięcia, a następnie naciśnij klawisz **Tab** , aby wstawić fragment kodu. Następnie wpisz rozmiar usługi RADIUS w pikselach i naciśnij klawisz **Enter**. Na przykład wpisz **15px**.

    Atrybuty CSS3 dodane przez fragment kodu będą renderować zaokrąglone obramowania w większości przeglądarek zgodności HTML5, w tym przeglądarki Mozilla i WebKit.

    ![Korzystanie z fragmentu kodu usługi RADIUS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image17.png "Korzystanie z fragmentu kodu usługi RADIUS")

    *Korzystanie z fragmentu kodu usługi RADIUS*
2. Zastosuj te same fragmenty kodu **obramowania** w stylu strony (strona).

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample2.css)]
3. Naciśnij klawisz **F5** , aby uruchomić rozwiązanie. Zauważ, że każda strona ma teraz obramowania zaokrąglone.

    ![Zaokrąglone rogi](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image18.png "Zaokrąglone rogi")

    *Zaokrąglone rogi*
4. Zamknij przeglądarkę i wróć do programu Visual Studio.
5. Otwórz **niestandardowy plik CSS** znajdujący się w folderze **Style** i umieść kursor wewnątrz **bloku DIV. obrazy definicja klasy ul li IMG** .
6. Naciśnij klawisz ENTER, aby wyświetlić listę funkcji IntelliSense, wpisz **ramkę box-shadow** i naciśnij klawisz **Tab** dwa razy, aby wstawić domyślny fragment kodu cienia w definicji klasy. Ustaw wartości w tle na **10px 10px 5px #888**. Następnie wpisz **border-radius** i Wstaw fragment kodu. Wpisz **15px** , aby ustawić rozmiar usługi RADIUS, a następnie naciśnij klawisz **Enter**.

    ![Zaokrąglone rogi z cieniem](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image19.png "Zaokrąglone rogi z cieniem")

    *Zaokrąglone rogi z cieniem*

    > [!NOTE]
    > W tej chwili atrybut Shadow zostanie wstawiony przy użyciu odpowiedniego prefiksu (moz, WebKit, o), aby obsługiwał przeglądarki Mozilla i WebKit (Chrome, Safari, Konkeror).
7. Utwórz nową klasę **DIV. images ul li IMG: Umieść** kursor poniżej **bloku DIV. obrazy definicja klasy ul li IMG** i umieść wskaźnik wewnątrz nawiasów **.**

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample3.css)]
8. Wpisz **transformację** i naciśnij klawisz **Tab** dwa razy, aby wstawić fragment kodu przekształcenia. Następnie wprowadź **Obróć (-15deg)** , aby zmienić wartość kąta obrotu, gdy obrazy są przesuwane.

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample4.css)]
9. Naciśnij klawisz **F5** , aby uruchomić rozwiązanie, a następnie przejdź do strony CSS3. Zauważ, że obrazy mają zaokrąglone rogi i cienie okien. Umieść wskaźnik myszy nad obrazami i obejrzyj go.

    ![Przekształcanie fragmentu obrazu na fragmenty](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image20.png "Przekształcanie fragmentu obrazu na fragmenty")

    *Przekształcanie fragmentu obrazu na fragmenty*

    > [!NOTE]
    > Jeśli używasz programu Internet Explorer 10 i nie widzisz cieni, upewnij się, że tryb dokumentu jest ustawiony na programu IE10 standardy. Naciśnij klawisz **F12** , aby otworzyć narzędzia deweloperskie programu Internet Explorer, a następnie kliknij pozycję **Tryb dokumentu** , aby zmienić programu IE10 standardy.

    ![o-US](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image21.png)

<a id="Exercise2"></a>

<a id="Exercise_2_Whats_New_in_the_HTML_Editor"></a>
### <a name="exercise-2-whats-new-in-the-html-editor"></a>Ćwiczenie 2: co nowego w edytorze HTML

Program Visual Studio zawiera udoskonalony Edytor HTML. Niektóre ulepszenia zawarte w tej wersji są inteligentnymi wcięciami w dokumentach HTML, fragmentach HTML5, kodzie HTML i tagów końcowych oraz weryfikacji HTML. W ramach tego ćwiczenia zobaczysz, jak te zmiany ulepszają Fluency podczas pracy z adiustacją witryny sieci Web.

Podobnie jak w przypadku edytora CSS, Edytor HTML został również ulepszony. Większość z tych ulepszeń jest mała, co ułatwia życie dewelopera sieci Web. Elementy, takie jak więcej fragmentów kodu dla języka HTML5, inteligentne wcięcie, dopasowanie tagów początkowych i końcowych podczas edytowania i walidacji dla dokumentu HTML DOCTYPE są niektóre z tych ulepszeń.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Improved_DOCTYPE_Validation"></a>
#### <a name="task-1---improved-doctype-validation"></a>Zadanie 1 — ulepszone Walidacja DOCTYPE

Edytor HTML ma teraz możliwość sprawdzenia deklaracji DOCTYPE strony, mimo że definicja może znajdować się na stronie wzorcowej. W zależności od elementu DOCTYPE strony Edytor HTML sprawdzi poprawność zestawu reguł i odfiltruje listę funkcji IntelliSense uwzględniającą elementy DOCTYPE.

W tym zadaniu zmienisz element DOCTYPE strony, aby zobaczyć, jak zachowanie edytora HTML zmienia się odpowiednio.

1. Jeśli nie zostało to jeszcze zrobione, uruchom **program Visual Studio** i Otwórz rozwiązanie **WhatsNewASPNET. sln** znajdujące się w folderze **Source\WhatsNewASPNET** w tym laboratorium.
2. Otwórz stronę **site. Master** .
3. Zwróć uwagę na schemat docelowy na pasku narzędzi walidacji. Sposób działania edytora HTML (Walidacja, IntelliSense itp.) zostanie odpowiednio zmieniony w celu dopasowania do wybranego typu DOCTYPE.

    ![Użyj DOCTYPE w pasku narzędzi do edycji źródła HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image22.png "Użyj DOCTYPE w pasku narzędzi do edycji źródła HTML")

    *Użyj DOCTYPE w pasku narzędzi do edycji źródła HTML*
4. Zmień schemat docelowy na HTML 4,01.

    ![Zmiana DOCTYPE w pasku narzędzi do edycji źródła HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image23.png "Zmiana DOCTYPE w pasku narzędzi do edycji źródła HTML")

    *Zmiana DOCTYPE w pasku narzędzi do edycji źródła HTML*
5. Umieść kursor w obszarze elementu **Body** i zacznij wpisywać nazwę elementu HTML5 (na przykład **wideo**). Zauważ, że element nie jest dostępny na liście IntelliSense.

    ![Elementy HTML5, które nie znajdują się na liście](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image24.png "Elementy HTML5, które nie znajdują się na liście")

    *Elementy HTML5, które nie znajdują się na liście*
6. Cofnij zmiany w schemacie docelowym dla paska narzędzi walidacji, wybierając DOCTYPE: XHTML5 z listy rozwijanej.

    ![Użyj DOCTYPE w pasku narzędzi do edycji źródła HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image25.png "Użyj DOCTYPE w pasku narzędzi do edycji źródła HTML")

    *Zresetuj DOCTYPE w pasku narzędzi do edycji źródła HTML*
7. Umieść kursor w obszarze elementu **Body** i zacznij ponownie wpisywać element HTML5 (na przykład **wideo**). Zauważ, że elementy HTML5 są teraz dostępne na liście IntelliSense.

    ![Elementy HTML5 są wyświetlane](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image26.png "Elementy HTML5 są wyświetlane")

    *Elementy HTML5 są wyświetlane*

<a id="Ex2Task2"></a>

<a id="Task_2_-_StartEnd_Tags_Automatic_Update"></a>
#### <a name="task-2---startend-tags-automatic-update"></a>Zadanie 2 — automatyczne aktualizowanie tagów Start/End

Program Visual Studio teraz aktualizuje Tagi HTML otwierające lub zamykające element, który jest edytowany w celu dopasowania do siebie nawzajem. Ta nowa funkcja poprawi wydajność podczas edytowania tagów HTML.

1. Na stronie **default. aspx** Dodaj element **H3** z tytułem (na przykład Visual Studio 2012 Rocks!).

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample5.aspx)]
2. Zmień tag **H3** i wpisz **H2** lub **H1.**

    Zauważ, że tag końcowy jest automatycznie aktualizowany. Możesz również zmodyfikować tag końcowy, aby sprawdzić, czy tag początkowy jest odpowiednio aktualizowany.

    ![Automatyczna aktualizacja tagu końcowego](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image27.png "Automatyczna aktualizacja tagu końcowego")

    *Automatyczna aktualizacja tagu końcowego*

<a id="Ex2Task3"></a>

<a id="Task_3_-_New_HTML5_Code_Snippets"></a>
#### <a name="task-3---new-html5-code-snippets"></a>Zadanie 3 — nowe fragmenty kodu HTML5

Program Visual Studio zawiera teraz kilka fragmentów kodu HTML5. W tym zadaniu będziesz używać niektórych z tych fragmentów kodu.

1. Dodaj nowy folder o nazwie **dźwięk** do katalogu głównego folderu witryny sieci Web. Otwórz Eksploratora Windows i skopiuj dowolny plik audio do folderu **audio** rozwiązania **WhatsNewASPNET. sln** .
2. Na stronie **default. aspx** Znajdź kursor w obszarze Web11 Rocks!! Nagłówki. Wpisz **dźwięk** i naciśnij klawisz Tab.

    Nowy edytor HTML zawiera fragmenty kodu dla zawartości HTML5. Należy pamiętać, aby użyć odpowiedniej definicji DOCTYPE do włączenia fragmentów kodu HTML5.

    ![Wstawianie fragmentów kodu HTML5](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image28.png "Wstawianie fragmentów kodu HTML5")

    *Wstawianie fragmentów kodu HTML5*
3. Zaktualizuj źródło audio, aby wskazywało istniejący plik audio.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample6.aspx)]

    > [!NOTE]
    > Należy dodać plik audio do rozwiązania.
4. Naciśnij klawisz **F5** , aby uruchomić lokację i odtworzyć dźwięk.

    ![Uruchamianie kontrolki audio](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image29.png "Uruchamianie kontrolki audio")

    *Uruchamianie kontrolki audio*

    > [!NOTE]
    > Możesz również wypróbować więcej fragmentów kodu zawartych w programie Visual Studio, takich jak wideo, rysunek itp.
5. Teraz spróbuj wstawić formant w pewnej części strony. Na przykład Spróbuj wstawić kontrolkę **GridView** , ale zamiast wpisywać **&lt;SIA,** zacznij pisać **&lt;GV**. Zauważ, że na liście IntelliSense jest wyświetlana kontrolka **ASP: GridView** .

    Funkcja IntelliSense w edytorze HTML zapewnia teraz wyszukiwanie przy użyciu wielkości liter, a także częściowe Dopasowywanie (pobieranie wszystkich elementów, które zawierają termin).

    ![Wstawianie widoku GridView przy użyciu list IntelliSense](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image30.png "Wstawianie widoku GridView przy użyciu list IntelliSense")

    *Wstawianie widoku GridView przy użyciu list IntelliSense*

    W przypadku wpisania **&lt;siatki** zostaną wyświetlone wszystkie elementy zgodne z terminem, ale program Visual Studio zasugeruje kontrolkę **GridView** :

    ![Wstawianie widoku GridView przy użyciu list IntelliSense i częściowego dopasowania](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image31.png "Wstawianie widoku GridView przy użyciu list IntelliSense i częściowego dopasowania")

    *Wstawianie widoku GridView przy użyciu list IntelliSense i częściowego dopasowania*

<a id="Ex2Task4"></a>

<a id="Task_4_-_HTML_Editor_Smart_Tags"></a>
#### <a name="task-4---html-editor-smart-tags"></a>Zadanie 4 — Tagi inteligentne edytora HTML

Innym ulepszeniem edytora HTML jest funkcja tagów inteligentnych. Tagi inteligentne ułatwiają wykonywanie typowych lub powtarzalnych zadań programistycznych na podstawie poszczególnych kontrolek. Ta funkcja była już dostępna w projektancie HTML, ale nie w edytorze HTML.

1. Otwórz plik **site. Master** i Znajdź element **menu ASP:** . Umieść kursor w tagu początkowym i Zauważ, że mały symbol wyświetlany w dolnej części elementu — kliknij go, aby otworzyć menu zadania inteligentne. Zwróć uwagę, że masz szybki dostęp do niektórych zadań związanych z kontrolką menu.

    ![Inteligentne zadania dla kontrolki menu](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image32.png "Inteligentne zadania dla kontrolki menu")

    *Inteligentne zadania dla kontrolki menu*

<a id="Ex2Task5"></a>

<a id="Task_5_-_Smart_Indentation"></a>
#### <a name="task-5---smart-indentation"></a>Zadanie 5 — inteligentne wcięcie

Jednym z najlepszych rozwiązań w języku HTML jest wcięcie elementów zagnieżdżonych w celu zachowania czytelności kodu. W programie Visual Studio 2012 można zauważyć, że edytor automatycznie zwiększa wcięcie elementów podczas pisania kodu.

> [!NOTE]
> W poprzedniej wersji programu Visual Studio inteligentne wcięcia były dostępne w edytorze XML, ale nie w edytorze HTML.

1. Upewnij się, że konfiguracja wcięć w edytorze HTML jest ustawiona na inteligentne wcięcia. Aby to zrobić, wybierz **Narzędzia |** Opcji menu Opcje, a następnie wybierz **Edytor tekstu | HTML | Strona kart** w lewym okienku ekranu. Wybierz opcję inteligentnego wcięcia.

    ![Ustawienia edytora HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image33.png "Ustawienia edytora HTML")

    *Ustawienia edytora HTML*
2. Na stronie **default. aspx** Usuń całą zawartość z elementu audio.
3. Umieść kursor na końcu otwierającego elementu **audio** i naciśnij klawisz **Enter**.

    Zauważ, że nowa pozycja kursora ma dodatkowy poziom wcięcia.

    ![Inteligentne wcięcia w edytorze HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image34.png "Inteligentne wcięcia w edytorze HTML")

    *Inteligentne wcięcia w edytorze HTML*
4. Przywróć tag audio z zawartością, która została usunięta, lub Zamknij **default. aspx** bez zapisywania zmian.

<a id="Ex2Task6"></a>

<a id="Task_6_-_Extract_to_User_Control"></a>
#### <a name="task-6---extract-to-user-control"></a>Zadanie 6 — wyodrębnienie do kontrolki użytkownika

Narzędzia refaktoryzacji zawarte w programie Visual Studio, takie jak wyodrębnianie fragmentu kodu do funkcji, to doskonałe funkcje, które ułatwiają ulepszenie i refaktoryzację istniejącego kodu. Odpowiednikiem stron ASP.NET będzie wyodrębnienie kodu HTML do kontrolki użytkownika. Wykonanie tej czynności ręcznie może wiązać się z kilkoma krokami, takimi jak tworzenie nowej kontrolki użytkownika, przesuwanie sekcji kodu do kontrolki użytkownika, rejestrowanie prefiksu tagu dla kontrolki użytkownika, a wreszcie Tworzenie wystąpienia kontrolki użytkownika na stronach. Teraz nowe narzędzie *Wyodrębnij do kontrolki użytkownika* automatycznie wykonuje wszystkie te kroki.

W tym zadaniu zostanie użyta Nowa operacja Wyodrębnij do kontrolki użytkownika w celu wygenerowania nowej kontrolki użytkownika z wybranego kodu.

1. Na stronie **default. aspx** wybierz elementy **H2** i **audio** .
2. Kliknij prawym przyciskiem myszy i wybierz polecenie **Wyodrębnij do kontrolki użytkownika**.

    ![Opcja menu Wyodrębnij do kontrolki użytkownika](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image35.png "Opcja menu Wyodrębnij do kontrolki użytkownika")

    *Opcja menu Wyodrębnij do kontrolki użytkownika*
3. Wpisz nazwę nowej kontrolki użytkownika. Na przykład, **Jukebox. ascx**, a następnie kliknij przycisk **OK**.

    ![Zapisywanie wyodrębnionej kontrolki użytkownika](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image36.png "Zapisywanie wyodrębnionej kontrolki użytkownika")

    *Zapisywanie wyodrębnionej kontrolki użytkownika*
4. Zauważ, że wybrany kod został wyodrębniony do kontrolki użytkownika, a oryginalna lokalizacja wybranego kodu została zastąpiona wystąpieniem nowej kontrolki użytkownika.

    ![Strona jest automatycznie aktualizowana w celu korzystania z nowej kontrolki użytkownika](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image37.png "Strona jest automatycznie aktualizowana w celu korzystania z nowej kontrolki użytkownika")

    *Strona jest automatycznie aktualizowana w celu korzystania z nowej kontrolki użytkownika*
5. Naciśnij klawisz **F5** , aby uruchomić stronę i sprawdzić, czy formant działa.

<a id="Exercise3"></a>

<a id="Exercise_3_Whats_New_in_the_JavaScript_Editor"></a>
### <a name="exercise-3-whats-new-in-the-javascript-editor"></a>Ćwiczenie 3: co nowego w edytorze JavaScript

Pisanie i edytowanie kodu JavaScript nie jest łatwym zadaniem, szczególnie w przypadku, gdy aplikacja rośnie w miarę wzrostu rozmiaru i można znaleźć informacje o długich plikach i setkach funkcji. Deweloperzy skryptów zazwyczaj muszą wykonać dodatkową czynność, aby zachować czytelność kodu i poruszać się między plikami. Po dołączeniu bibliotek JavaScript, takich jak jQuery, Nawigacja po skrypcie stała się wyzwaniem ze względu na długość kodu.

Program Visual Studio odnowił Edytor JavaScript z obietnicą, aby udostępnić tryb kodu i zorganizować go. Wiele funkcji programu Visual Studio, które już istniały w C# lub EDYTORach VB, zostało teraz zaimplementowane w edytorze JavaScript: Przejdź do definicji, automatycznego wcięcia, dokumentacji i walidacji podczas pisania. Z odnowioną listą IntelliSense masz dostęp do wykazu funkcji JavaScript.

W tym ćwiczeniu przedstawiono niektóre z nowych funkcji i ulepszeń edytora JavaScript. Przeglądasz przykładowe pliki i odkryjemy każdą z nowych cech, które sprawiają, że programowanie JavaScript będzie wydajniejsze w programie Visual Studio 2012.

<a id="Ex3Task1"></a>

<a id="Task_1_-_JavaScript_Editor_New_Features"></a>
#### <a name="task-1---javascript-editor-new-features"></a>Zadanie 1 — nowe funkcje edytora JavaScript

To zadanie spowoduje wprowadzenie do niektórych nowych funkcji edytora JavaScript, które koncentrują się na organizowaniu kodu i ulepszaniu środowiska użytkownika.

1. Jeśli nie zostało to jeszcze zrobione, uruchom **program Visual Studio** i Otwórz rozwiązanie **WhatsNewASPNET. sln** znajdujące się w folderze **Source\WhatsNewASPNET** w tym laboratorium.
2. Naciśnij klawisz **F5** , aby uruchomić aplikację, a następnie kliknij link JavaScript na pasku nawigacyjnym. Odśwież stronę kilka razy i sprawdź, jak licznik rośnie.

    ![Licznik strony](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image38.png "Licznik strony")

    *Licznik strony*
3. Zamknij przeglądarkę i wróć do programu Visual Studio.
4. Otwórz stronę **JavaScript. aspx** i znajdź blok **&lt;skryptu&gt;** (pokazany poniżej).

    Poniższy kod używa lokalnego magazynu HTML5 do przechowywania zmiennej *pageLoadCount* , która przechowuje liczbę odwiedzin strony przez bieżącego użytkownika. Magazyn lokalny to baza danych z kluczową wartością po stronie klienta wprowadzona w standardzie HTML5. Dane są zapisywane na komputerze lokalnym w przeglądarce użytkownika.

    [!code-html[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample7.html)]

    > [!NOTE]
    > Upewnij się, że DOCTYPE jest ustawiony na XHTML5 przed przejściem do następnego kroku.
5. Edytuj kod i zwróć uwagę, że technologia IntelliSense dla języka JavaScript zawiera funkcje HTML5, takie jak magazyn lokalny i ich metody wewnętrzne.

    ![Funkcje JavaScript języka HTML5 w języku JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image39.png "Funkcje JavaScript języka HTML5 w języku JavaScript")

    *Funkcje JavaScript języka HTML5 w języku JavaScript*
6. Kliknij dowolny nawias otwierający ( **{** ) w kodzie skryptów i Zauważ, że nawiasy są wyróżnione.

    ![Nawiasy kwadratowe są wyróżnione](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image40.png "Nawiasy kwadratowe są wyróżnione")

    *Nawiasy kwadratowe są wyróżnione*
7. Usuń komentarz z funkcji **testAutoAlign ()** (wybierz trzy wiersze i możesz użyć **klawisza Ctrl** + **K**; **Naciśnij klawisz CTRL** + **U**) i Znajdź kursor wewnątrz kodu funkcji. Naciśnij klawisz ENTER, aby dołączyć drugi wiersz. Zauważ, że kod jest teraz **wyrównany** i z **wcięciem**.

    ![Kod JavaScript jest wyrównany do Autokorekty](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image41.png "Kod JavaScript jest wyrównany do Autokorekty")

    *Kod JavaScript jest wyrównany do Autokorekty*

<a id="Ex3Task2"></a>

<a id="Task_2_-_Validating_JavaScript"></a>
#### <a name="task-2---validating-javascript"></a>Zadanie 2 — Walidacja kodu JavaScript

To zadanie spowoduje odnalezienie nowej walidacji kodu JavaScript dla standardu ECMAScript5. Ta funkcja pomoże Ci napisać zgodny kod JavaScript, jednocześnie uniemożliwiając problemy ze skryptami przed wdrożeniem lokacji.

> [!NOTE]
> Program Visual Studio 2010 zaimplementował zgodność ECMAStript3, podczas gdy program Visual Studio 2012 zapewnia zgodność ECMAScript5.

1. Otwórz **ECMA5script5. js** znajdujący się w folderze projektu **Scripts\custom** . Teraz sprawdzisz weryfikację standardu ECMAScript5.

    [!code-html[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample8.html)]

    W pierwszym wierszu pliku można wyewidencjonować &quot; **użyć opcji strict** &quot;, co umożliwia **tryb Strict**ECMAScript5. Ten tryb składa się z podzestawu języka, który objaśnia niejasności z ostatniej edycji i dodaje nowe funkcje, takie jak metody pobierające i metody ustawiające, obsługa bibliotek JSON i pełniejsze odbicie we właściwościach obiektu.
2. Otwórz **Lista błędów** , jeśli nie został jeszcze otwarty (menu**Widok** | **Lista błędów**). Zauważ, że deklaracja **funkcji** jest podkreślona. Wynika to z faktu, że w ECMA5 funkcjach standardowych nie mogą być zagnieżdżone wewnątrz struktur języka. Na poniższej liście błędów zobaczysz szczegóły ostrzegawcze.

    ![Komunikat o błędzie weryfikacji kodu JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image42.png "Komunikat o błędzie weryfikacji kodu JavaScript")

    *Komunikat o błędzie weryfikacji kodu JavaScript*
3. Dodaj komentarz do **&quot;Użyj ścisłego&quot;** kierunku i zwróć uwagę na to, że wystąpiły błędy, ale ostrzeżenia pozostają.
4. W ostatnim wierszu pliku Napisz dowolny ciąg, taki jak **&quot;&quot;testu** (Uwzględnij cudzysłowy, aby wskazać, że jest to ciąg znaków). Napisz okres obok ciągu, aby wyświetlić listę funkcji IntelliSense, a następnie wybierz opcję **przycinania** .

    W standardzie ECMAScript5 wartości ciągów i zmienne mają również zdefiniowane metody String, takie jak Trim, wielkie litery, wyszukiwania i zamieniania.

    ![Lista funkcji IntelliSense w języku JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image43.png "Lista funkcji IntelliSense w języku JavaScript")

    *Lista funkcji IntelliSense w języku JavaScript*

<a id="Ex3Task3"></a>

<a id="Task_3_-_XML_Documentation_for_JavaScript"></a>
#### <a name="task-3---xml-documentation-for-javascript"></a>Zadanie 3 — dokumentacja XML dla języka JavaScript

To zadanie zawiera informacje o funkcjach programu Visual Studio dla dokumentacji XML w języku JavaScript. Zostanie wyświetlona lista IntelliSense języka JavaScript, w której jest teraz wyświetlana dokumentacja XML każdej funkcji. Ponadto zostanie wykryta funkcja nawigacji w języku JavaScript.

1. Otwórz plik **XMLDoc. js** znajdujący się w **scripts/Custom** Project folder. Ten plik zawiera dokumentację XML dla każdej funkcji języka JavaScript.

    ![Dokumentacja XML języka JavaScript zintegrowana z technologią IntelliSense](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image44.png "Dokumentacja XML języka JavaScript zintegrowana z technologią IntelliSense")

    *Dokumentacja XML języka JavaScript zintegrowana z technologią IntelliSense*
2. Poniżej **Dodaj** funkcję w pliku **XMLDoc. js** Utwórz nową funkcję o nazwie **test**.
3. W funkcji **testowej** wywołaj funkcję **mnożenia** , która odbiera dwa parametry. Zwróć uwagę na to, że w polu Etykietka narzędzia zostanie wyświetlona dokumentacja funkcji **mnożenia** .

    [!code-javascript[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample9.js)]

    ![Dokumentacja XML dla funkcji języka JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image45.png "Dokumentacja XML dla funkcji języka JavaScript")

    *Dokumentacja XML dla funkcji języka JavaScript*
4. Ukończ instrukcję wywołania funkcji i wpisz *kropkę* , aby otworzyć listę funkcji IntelliSense na zwracanej wartości. Zauważ, że program Visual Studio wykrywa wartość **zwracaną** w dokumentacji, traktując wartość jako liczbę.

    ![Dokumentacja XML dla zwracanych typów](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image46.png "Dokumentacja XML dla zwracanych typów")

    *Dokumentacja XML dla zwracanych typów*
5. Teraz Wstaw wywołanie funkcji Dodaj. Zauważ, że Edytor JavaScript obsługuje teraz przeciążenia funkcji. Po napisaniu nazwy funkcji będzie można wybrać dowolne z dostępnych przeciążeń określonych w dokumentacji.

    ![Dokumentacja XML dla przeciążeń](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image47.png "Dokumentacja XML dla przeciążeń")

    *Dokumentacja XML dla przeciążeń*
6. Otwórz plik **GotoDefinition. js** i Znajdź wywołanie funkcji **$ (). html ()** . Znajdź kursor w **kodzie HTML**.
7. Naciśnij klawisz **F12** i przejdź do definicji. Zauważ, że teraz możesz uzyskiwać dostęp do kodu JavaScript i przeglądać go bez użycia narzędzia **Znajdź** .
8. Znajdź kursor w instrukcji jQuery przed blokiem podpisu w dolnej części pliku kodu. Naciśnij klawisz **F12**. Przejdziesz do pliku biblioteki jQuery. Zwróć uwagę na to, że można także nawigować po plikach jQuery przy użyciu **F12**.

    ![Przechodzenie do definicji jQuery](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image48.png "Przechodzenie do definicji jQuery")

    *Przechodzenie do definicji jQuery*

> [!NOTE]
> Przed zapisaniem pliku upewnij się, że GotoDefinition. js nie ma błędów składniowych.

<a id="Exercise4"></a>

<a id="Exercise_4_Bundling_and_Minification"></a>
### <a name="exercise-4-bundling-and-minification"></a>Ćwiczenie 4: dzielenie i Minifikacja

Ile razy witryna sieci Web zawiera więcej niż jeden plik JavaScript lub CSS? Jest to bardzo typowy scenariusz, w którym zgrupowanie i minifikacja mogą pomóc w zmniejszeniu rozmiaru pliku i przyspieszyć działanie witryny. Nowa funkcja tworzenia pakietów w programie ASP.NET 4,5 pakuje zestaw plików JS lub CSS w jeden element i zmniejsza jego rozmiar, minifikacja zawartość (tj. usunięcie niewymaganych pustych miejsc, usunięcie komentarzy, zmniejszenie identyfikatorów).

Tworzenie i minifikacja w ASP.NET 4,5 jest wykonywane w środowisku uruchomieniowym, dzięki czemu proces może identyfikować agenta użytkownika (na przykład IE, Mozilla itp.), a tym samym poprawić kompresję przez kierowanie do przeglądarki użytkownika (np. usunięcie rzeczy, które są specyficzne dla firmy Mozilla) gdy żądanie pochodzi z programu IE).

W tym ćwiczeniu dowiesz się, jak włączyć i używać różnych typów grupowania i minifikacja w ASP.NET 4,5.

<a id="Ex4Task1"></a>

<a id="Task_1_-_Installing_the_Bundling_and_Minification_Package_from_NuGet"></a>
#### <a name="task-1---installing-the-bundling-and-minification-package-from-nuget"></a>Zadanie 1 — Instalowanie pakietu do tworzenia i tworzenia pakietów Minifikacja z narzędzia NuGet

1. Jeśli nie zostało to jeszcze zrobione, uruchom **program Visual Studio** i Otwórz rozwiązanie **WhatsNewASPNET. sln** znajdujące się w folderze **Source\WhatsNewASPNET** w tym laboratorium.
2. Otwórz konsolę Menedżera pakietów NuGet. W tym celu należy użyć **widoku** menu | innej **konsoli Menedżera pakietów** | **systemu Windows** .

    ![Otwieranie Menedżera pakietów file:///C:/Users/User/AppData/Local/Temp/Marker3744//media/44462/Multiple-Stylesheets-and-JavaScript-files-in-the-application.pngconsole](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image49.png "Otwieranie konsoli Menedżera pakietów")

    *Otwieranie konsoli Menedżera pakietów*
3. W **konsoli Menedżera pakietów wpisz polecenie** **install-package Microsoft. Web. Optimization** i naciśnij klawisz **Enter**.

<a id="Ex4Task2"></a>

<a id="Task_2_-_Default_Bundles"></a>
#### <a name="task-2---default-bundles"></a>Zadanie 2 — pakiety domyślne

Najprostszym sposobem korzystania z grupowania i minifikacja jest włączenie pakietów domyślnych. Ta metoda używa konwencji, aby można było odwołać się do dołączonej i zminimalizowanego wersji dla plików JS i CSS w folderze.

W tym zadaniu dowiesz się, jak włączyć i odwoływać się do powiązanych i zminimalizowanego JS oraz pliki CSS oraz wyświetlać wynikowe dane wyjściowe.

1. Jeśli nie zostało to jeszcze zrobione, uruchom **program Visual Studio** i Otwórz rozwiązanie **WhatsNewASPNET. sln** znajdujące się w folderze **Source\WhatsNewASPNET** w tym laboratorium.
2. W **Eksplorator rozwiązań**rozwiń foldery **Style**, **Scripts\custom** i **Scripts\bundle** .

    Zwróć uwagę, że aplikacja korzysta z więcej niż jednego pliku CSS i JS.

    ![Wiele arkuszy stylów i plików JavaScript w aplikacji](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image50.png "Wiele arkuszy stylów i plików JavaScript w aplikacji")

    *Wiele arkuszy stylów i plików JavaScript w aplikacji*
3. Otwórz plik **Global.asax.cs** .

    Zwróć uwagę, że nowa przestrzeń nazw **Microsoft. Web. Optimization** jest oznaczona na początku pliku. Usuń oznaczenie komentarza dyrektywy using w celu uwzględnienia funkcji tworzenia i minifikacja.

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample10.cs)]
4. Znajdź **\_ową metodę uruchamiania aplikacji** .

    W tej metodzie Usuń komentarz z wywołania EnableDefaultBundles, jak pokazano w poniższym fragmencie kodu. Dzięki temu można odwoływać się do powiązanej kolekcji plików CSS w folderze przy użyciu ścieżki do tego folderu oraz&quot; &quot;CSS lub sufiksu&quot; &quot;JS.

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample11.cs)]
5. Otwórz plik **Optimization. aspx** i Znajdź formant zawartości dla **kontrolka headcontent**.

    Zauważ, że pliki CSS i JS mają jeden tag, do którego się odwołuje.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample12.aspx)]

    > [!NOTE]
    > Ten kod jest przeznaczony do celów demonstracyjnych. Najlepiej odwołują się do pakietów w pliku site. Master. W tym przykładowym kodzie znajdziesz, że niektóre z powiązanych plików są również odwołujące się do pliku site. Master, co oznacza to ostatnie odwołanie.
6. Zwróć uwagę, że linki używają Konwencji łączenia w atrybucie **href** , aby uzyskać wszystkie pliki CSS i JS odpowiednio do folderu style i Scripts\custom.

    Możesz użyć **skryptów Path/Custom/js** , jak pokazano poniżej, aby powiązać i zminifikować wszystkie pliki JS wewnątrz **skryptów/folderów niestandardowych** . Jest to domyślne zachowanie z pakietami domyślnymi.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample13.aspx)]
7. Otwórz plik **Styles\Site.css** .

    Zwróć uwagę, że oryginalny plik CSS zawiera wcięty kod, puste miejsca i komentarze, które powiększają plik. (Ponadto plik JavaScript zawiera spacje i komentarze).

    ![Jeden z oryginalnych plików CSS w folderze skryptów](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image51.png "Jeden z oryginalnych plików CSS w folderze skryptów")

    *Jeden z oryginalnych plików CSS w folderze skryptów*
8. Naciśnij klawisz **F5** , aby uruchomić aplikację, a następnie przejdź do strony **Optymalizacja** .
9. Kliknij link **pakiet CSS** , aby pobrać i otworzyć plik.

    Sprawdź plik z pakietem zminimalizowanego. Zobaczysz, że wszystkie spacje, komentarze i znaki wcięć zostały usunięte, generując mniejszy plik.

    ![Powiązane pliki CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image52.png "Powiązane pliki CSS")

    *Powiązane pliki CSS*
10. Teraz kliknij link **pakietu js** , aby otworzyć plik w języku JavaScript. Możesz bezpiecznie zignorować ostrzeżenie Eksploratora. Zwróć uwagę, że pliki JavaScript w folderze **Custom** są również dołączone i zminimalizowanego.

    ![Powiązane pliki JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image53.png "Powiązane pliki JavaScript")

    *Powiązane pliki JavaScript*

    Włączenie kompresji dla plików CSS lub JS było znacznie bardziej skomplikowane w poprzedniej wersji ASP.NET. Teraz, gdy już się widzisz, wystarczy dodać jeden wiersz w pliku *Global. asax* , aby włączyć funkcję grupowania, a następnie odwołać się do powiązanych plików z witryny.

<a id="Ex4Task3"></a>

<a id="Task_3_-_Static_Bundles"></a>
#### <a name="task-3---static-bundles"></a>Zadanie 3 — pakiety statyczne

Podejście statyczne umożliwia dostosowanie zestawu plików do pakietu, odwołanie i metodę minifikacja, która zostanie użyta.

W tym zadaniu zostanie skonfigurowany zbiór statyczny do definiowania określonego zestawu plików do powiązania i zminifikować.

1. Zamknij okno przeglądarki.
2. Otwórz plik **Global.asax.cs** i znajdź **aplikację\_Start** .
3. Usuń znaczniki komentarza z kodu pakietu statycznego, jak pokazano w poniższym kodzie.

    Definiujesz zestaw statyczny, do którego odwołuje się &quot; **~/StaticBundle**&quot; ścieżką wirtualną i Użyj **JsMinify** dla minifikacja wszystkich określonych plików z metodą **AddFile** . Na koniec dodajesz statyczny pakiet do **pakietu** i włącz go.

    Należy zauważyć, że pliki nie znajdują się w tym samym miejscu; jest to kolejna korzyść w porównaniu z domyślnym przydzieleniem.

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample14.cs)]
4. Otwórz plik **Optimization. aspx** .

    Zwróć uwagę, że połączenie z **pakietem statycznym js** używa ścieżki zadeklarowanej podczas konfigurowania pakietu statycznego w pliku Global.asax.cs: **/StaticBundle**.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample15.aspx)]
5. Naciśnij klawisz **F5** , aby uruchomić aplikację, a następnie przejdź do strony **Optymalizacja** .
6. Kliknij link zestawu **statycznych js** , aby otworzyć plik.

    Należy zauważyć, że plik JavaScript z pakietem zminimalizowanego jest wyjściem dla wszystkich plików JavaScript skonfigurowanych w pliku pakietu statycznego pod ścieżką &quot;/StaticBundle&quot;.

    ![Pakiet statycznych plików JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image54.png "Pakiet statycznych plików JavaScript")

    *Pakiet statycznych plików JavaScript*
7. Zamknij przeglądarkę i wróć do programu Visual Studio.

<a id="Ex4Task4"></a>

<a id="Task_4_-_Dynamic_Folder_Bundles"></a>
#### <a name="task-4---dynamic-folder-bundles"></a>Zadanie 4 — pakiety folderów dynamicznych

W tym zadaniu dowiesz się, jak skonfigurować zbiory folderów dynamicznych. Zaletą dynamicznego grupowania jest możliwość dołączenia statycznego kodu JavaScript, a także innych plików w językach, które kompilują do języka JavaScript, a tym samym wymagają przetwarzania przed wykonaniem tej operacji.

W tym przykładzie dowiesz się, jak za pomocą klasy **DynamicFolderBundle** utworzyć dynamiczny pakiet dla plików pisanych w CofeeScript. CofeeScript to język programowania, który kompiluje się do języka JavaScript i zapewnia prostsze składnie pisania kodu JavaScript, zwiększając zwięzłości i czytelność języka JavaScript.

1. Otwórz plik **Global.asax.cs** i znajdź **aplikację\_Start** .
2. Usuń komentarz z kodu pakietu dynamicznego, jak pokazano w poniższym kodzie.

    Definiujesz pakiet folderu dynamicznego, który będzie korzystał z niestandardowego procesora **CoffeeMinify** minifikacja, który będzie stosowany tylko do plików z rozszerzeniem &quot;**kawy**&quot; (pliki CoffeeScript). Zwróć uwagę, że możesz użyć wzorca wyszukiwania, aby wybrać pliki do powiązania w folderze, np. "\*. kawę".

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample16.cs)]
3. Otwórz konsolę Menedżera pakietów NuGet. W tym celu należy użyć **widoku** menu | innej **konsoli Menedżera pakietów** | **systemu Windows** .
4. W **konsoli Menedżera pakietów wpisz polecenie** **install-package CoffeeSharp** i naciśnij klawisz **Enter**.
5. Kliknij przycisk **Pokaż wszystkie pliki** w oknie **Eksplorator rozwiązań**

    ![Wyświetlanie wszystkich plików](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image55.png "Wyświetlanie wszystkich plików")

    *Wyświetlanie wszystkich plików*
6. Kliknij prawym przyciskiem myszy plik **CoffeeMinify.cs** w **Eksplorator rozwiązań** i wybierz pozycję **Dołącz w projekcie**

    ![Dołącz plik CoffeeMinify.cs do projektu](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image56.png "Dołącz plik CoffeeMinify.cs do projektu")

    *Dołącz plik CoffeeMinify.cs do projektu*
7. Otwórz plik **CoffeeMinify.cs** .

    Ta klasa dziedziczy z JsMinify w celu zminifikować danych wyjściowych języka JavaScript wynikających z kompilacji kodu CoffeeScript. Wywołuje kompilator CoffeeScript, aby najpierw wygenerować kod JavaScript, a następnie wysyła go do metody JsMinify. Process, aby zminifikować otrzymany kod.

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample17.cs)]
8. Otwórz pliki **Script1. kawy** i **Script2. kawy** w folderze **scripts/pakiet** .

    Te pliki będą zawierać kod CoffeScript do skompilowania podczas tworzenia kodu z klasą CoffeeMinify.

    Dla uproszczenia udostępniane pliki CoffeeScript mają tylko kod przykładowy CoffeeScript. Komentarze są wykluczone przez proces JsMinify.

    ![Pliki CoffeeScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image57.png "Pliki CoffeeScript")

    *Pliki CoffeeScript*

    > [!NOTE]
    > [CofeeScript](https://github.com/jashkenas/coffeescript/) zapewnia prostszy sposób pisania kodu JavaScript, zwiększa zwięzłości i czytelność kodu JavaScript, a także dodaje inne funkcje, takie jak zrozumienie i dopasowanie do wzorców.
9. Otwórz plik **Optimization. aspx** i Znajdź linki do pakietu.

    Zwróć uwagę, że połączenie z **pakietem dynamicznego js** odwołuje się do folderu **scripts/Binding** przy użyciu sufiksu **/Coffee** skonfigurowanego dla pakietu folderu dynamicznego.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample18.aspx)]
10. Naciśnij klawisz **F5** , aby uruchomić aplikację, a następnie przejdź do strony **Optymalizacja** .
11. Kliknij link **dynamicznego pakietu js** , aby otworzyć wygenerowany plik.

    Zwróć uwagę, że zawartość dołączona do tego pakietu zawiera tylko pliki **kawy** . Można też zobaczyć, że kod CoffeeScript został skompilowany do języka JavaScript, a wiersze z komentarzem zostały usunięte.

    ![Pakiet dynamicznych plików JS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image58.png "Pakiet dynamicznych plików JS")

    *Pakiet dynamicznych plików JS*

> [!NOTE]
> Ponadto tę aplikację można wdrożyć w witrynach sieci Web systemu Windows Azure, które zostały opisane w [dodatku B: publikowanie aplikacji ASP.NET MVC 4 przy użyciu Web Deploy](#AppendixB).

<a id="Summary"></a>
## <a name="summary"></a>Podsumowanie

To laboratorium pomaga zrozumieć, co nowego w ASP.NET i rozwoju aplikacji sieci Web w programie Visual Studio 2012, oraz jak korzystać z różnych ulepszeń w programie Visual Studio 2012.

Wykonując te praktyczne ćwiczenia, dowiesz się, jak korzystać z nowych funkcji i ulepszeń w edytorach programu Visual Studio 2012 dla CSS, JavaScript i HTML. Ponadto dowiesz się, jak program Visual Studio 2012 implementuje wbudowaną funkcję grupowania i minifikacja.

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Dodatek A: Instalowanie Visual Studio Express 2012 dla sieci Web

Można zainstalować **Microsoft Visual Studio Express 2012 dla sieci Web** lub innej &quot;Express&quot; wersji za pomocą **[Instalator platformy Microsoft Web](https://www.microsoft.com/web/downloads/platform.aspx)** . Poniższe instrukcje przeprowadzą Cię przez kroki wymagane do zainstalowania *programu Visual Studio Express 2012 for Web* przy użyciu *Instalator platformy Microsoft Web*.

1. Przejdź do [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Alternatywnie, jeśli zainstalowano już Instalatora platformy sieci Web, można go otworzyć i wyszukać produkt &quot;<em>Visual Studio Express 2012 for Web przy użyciu zestawu Windows Azure SDK</em>&quot;.
2. Kliknij pozycję **Zainstaluj teraz**. Jeśli nie masz **Instalatora platformy sieci Web** , nastąpi przekierowanie do pobrania i zainstalowania go w pierwszej kolejności.
3. Po otwarciu **Instalatora platformy sieci Web** kliknij przycisk **Zainstaluj** , aby rozpocząć instalację.

    ![Zainstaluj Visual Studio Express](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image59.png "Zainstaluj Visual Studio Express")

    *Zainstaluj Visual Studio Express*
4. Przeczytaj wszystkie licencje i postanowienia produktów, a następnie kliknij przycisk **Akceptuję** , aby kontynuować.

    ![Akceptowanie postanowień licencyjnych](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image60.png)

    *Akceptowanie postanowień licencyjnych*
5. Poczekaj na zakończenie procesu pobierania i instalacji.

    ![Postęp instalacji](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image61.png)

    *Postęp instalacji*
6. Po zakończeniu instalacji kliknij przycisk **Zakończ**.

    ![Instalacja zakończona](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image62.png)

    *Instalacja zakończona*
7. Kliknij przycisk **Zakończ** , aby zamknąć Instalatora platformy sieci Web.
8. Aby otworzyć Visual Studio Express dla sieci Web, przejdź do ekranu **startowego** i zacznij pisać &quot;**vs Express**&quot;, a następnie kliknij kafelek **vs Express for Web** .

    ![Kafelek VS Express for Web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image63.png)

    *Kafelek VS Express for Web*

---

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Dodatek B: publikowanie aplikacji ASP.NET MVC 4 przy użyciu Web Deploy

W tym dodatku pokazano, jak utworzyć nową witrynę sieci Web na podstawie portal zarządzania systemu Windows Azure i opublikować aplikację uzyskaną w wyniku działania laboratorium, korzystając z funkcji publikowania Web Deploy dostępnej w systemie Windows Azure.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Zadanie 1 — Tworzenie nowej witryny sieci Web z poziomu portalu systemu Windows Azure

1. Przejdź do [Portal zarządzania systemu Windows Azure](https://manage.windowsazure.com/) i zaloguj się przy użyciu poświadczeń firmy Microsoft skojarzonych z Twoją subskrypcją.

    > [!NOTE]
    > System Windows Azure umożliwia bezpłatne hostowanie witryn sieci Web 10 ASP.NET, a następnie skalowanie ich w miarę wzrostu ruchu. Możesz utworzyć konto w [tym miejscu](https://aka.ms/aspnet-hol-azure).

    ![Zaloguj się do systemu Windows Azure Portal](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image64.png "Zaloguj się do systemu Windows Azure Portal")

    *Zaloguj się do usługi Windows Azure portal zarządzania*
2. Na pasku poleceń kliknij pozycję **Nowy** .

    ![Tworzenie nowej witryny sieci Web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image65.png "Tworzenie nowej witryny sieci Web")

    *Tworzenie nowej witryny sieci Web*
3. Kliknij pozycję **obliczeniowe** | **witrynie sieci Web**. Następnie wybierz opcję **szybkie tworzenie** . Podaj dostępny adres URL dla nowej witryny sieci Web i kliknij pozycję **Utwórz witrynę sieci Web**.

    > [!NOTE]
    > Witryna sieci Web systemu Windows Azure jest hostem aplikacji sieci Web działającej w chmurze, którą można kontrolować i zarządzać nią. Opcja szybkie tworzenie umożliwia wdrożenie ukończonej aplikacji sieci Web w witrynie sieci Web systemu Windows Azure spoza portalu. Nie obejmuje to kroków związanych z konfigurowaniem bazy danych.

    ![Tworzenie nowej witryny sieci Web przy użyciu funkcji szybkiego tworzenia](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image66.png "Tworzenie nowej witryny sieci Web przy użyciu funkcji szybkiego tworzenia")

    *Tworzenie nowej witryny sieci Web przy użyciu funkcji szybkiego tworzenia*
4. Poczekaj na utworzenie nowej **witryny sieci Web** .
5. Po utworzeniu witryny sieci Web kliknij link pod kolumną **adres URL** . Sprawdź, czy nowa witryna sieci Web działa.

    ![Przeglądanie w nowej witrynie sieci Web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image67.png "Przeglądanie w nowej witrynie sieci Web")

    *Przeglądanie w nowej witrynie sieci Web*

    ![Uruchomiona witryna sieci Web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image68.png "Uruchomiona witryna sieci Web")

    *Uruchomiona witryna sieci Web*
6. Wróć do portalu i kliknij nazwę witryny sieci Web pod kolumną **Nazwa** , aby wyświetlić strony zarządzania.

    ![Otwieranie stron zarządzania witryną sieci Web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image69.png "Otwieranie stron zarządzania witryną sieci Web")

    *Otwieranie stron zarządzania witryną sieci Web*
7. Na stronie **pulpit nawigacyjny** w sekcji **szybki przegląd** kliknij link **Pobierz profil publikowania** .

    > [!NOTE]
    > *Profil publikowania* zawiera wszystkie informacje wymagane do opublikowania aplikacji sieci Web w witrynie sieci Web systemu Windows Azure dla każdej z włączonych metod publikacji. Profil publikowania zawiera adresy URL, poświadczenia użytkownika i ciągi bazy danych wymagane do nawiązania połączenia i uwierzytelniania dla każdego punktu końcowego, dla którego jest włączona metoda publikacji. **Program Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** i **Microsoft Visual Studio 2012** obsługują odczytywanie profilów publikowania w celu zautomatyzowania konfiguracji tych programów do publikowania aplikacji sieci Web w usłudze Microsoft Azure Websites.

    ![Pobieranie profilu publikowania witryny sieci Web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image70.png "Pobieranie profilu publikowania witryny sieci Web")

    *Pobieranie profilu publikowania witryny sieci Web*
8. Pobierz plik profilu publikowania do znanej lokalizacji. W tym ćwiczeniu zobaczysz, jak używać tego pliku do publikowania aplikacji sieci Web w witrynach sieci Web systemu Windows Azure z programu Visual Studio.

    ![Zapisywanie pliku profilu publikowania](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image71.png "Zapisywanie profilu publikowania")

    *Zapisywanie pliku profilu publikowania*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Zadanie 2 — Konfigurowanie serwera bazy danych

Jeśli aplikacja korzysta z baz danych SQL Server, należy utworzyć SQL Database serwer. Jeśli chcesz wdrożyć prostą aplikację, która nie używa SQL Server można pominąć to zadanie.

1. Do przechowywania bazy danych aplikacji potrzebny jest serwer SQL Database. Serwery SQL Database z subskrypcji można wyświetlić w portalu zarządzania systemu Windows Azure w obszarze **bazy danych SQL** | **serwery** | **pulpitu nawigacyjnego serwera**. Jeśli nie masz utworzonego serwera, możesz go utworzyć przy użyciu przycisku **Dodaj** na pasku poleceń. Zanotuj **nazwę serwera i adres URL, nazwę logowania administratora i hasło**, ponieważ zostaną one użyte w następnych zadaniach. Nie Twórz jeszcze bazy danych, ponieważ zostanie ona utworzona na późniejszym etapie.

    ![Pulpit nawigacyjny serwera SQL Database](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image72.png "Pulpit nawigacyjny serwera SQL Database")

    *Pulpit nawigacyjny serwera SQL Database*
2. W następnym zadaniu zostanie przetestowane połączenie z bazą danych z programu Visual Studio. z tego powodu należy uwzględnić lokalny adres IP na liście **dozwolonych adresów IP**serwera. W tym celu kliknij pozycję **Konfiguruj**, wybierz adres IP z **bieżącego adresu IP klienta** i wklej go w polach tekstowych **początkowy adres IP** i **końcowy adres IP** . Wprowadź nazwę reguły, a następnie kliknij przycisk ![Dodaj-Client-IP-Address-OK-przycisk](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image73.png).

    ![Dodawanie adresu IP klienta](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image74.png)

    *Dodawanie adresu IP klienta*
3. Po dodaniu **adresu IP klienta** do listy dozwolone adresy IP kliknij pozycję **Zapisz** , aby potwierdzić zmiany.

    ![Potwierdź zmiany](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image75.png)

    *Potwierdź zmiany*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Zadanie 3 — publikowanie aplikacji ASP.NET MVC 4 przy użyciu Web Deploy

1. Wróć do rozwiązania ASP.NET MVC 4. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt witryny sieci Web i wybierz polecenie **Publikuj**.

    ![Publikowanie aplikacji](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image76.png "Publikowanie aplikacji")

    *Publikowanie witryny sieci Web*
2. Zaimportuj profil publikowania zapisany w pierwszym zadaniu.

    ![Importowanie profilu publikowania](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image77.png "Importowanie profilu publikowania")

    *Importowanie profilu publikowania*
3. Kliknij pozycję **Weryfikuj połączenie**. Po zakończeniu walidacji kliknij przycisk **dalej**.

    > [!NOTE]
    > Walidacja jest ukończona po wyświetleniu zielonego znacznika wyboru obok przycisku Weryfikuj połączenie.

    ![Weryfikowanie połączenia](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image78.png "Weryfikowanie połączenia")

    *Weryfikowanie połączenia*
4. Na stronie **Ustawienia** w sekcji **bazy danych** kliknij przycisk obok pola tekstowego połączenie z bazą danych (np. **DefaultConnection**).

    ![Konfiguracja narzędzia Web Deploy](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image79.png "Konfiguracja narzędzia Web Deploy")

    *Konfiguracja narzędzia Web Deploy*
5. Skonfiguruj połączenie z bazą danych w następujący sposób:

   - W polu **Nazwa serwera** wpisz adres URL serwera SQL Database przy użyciu prefiksu *TCP:* .
   - W polu **Nazwa użytkownika** wpisz nazwę logowania administratora serwera.
   - W polu **hasło** wpisz hasło logowania administratora serwera.
   - Wpisz nową nazwę bazy danych, na przykład: *MVC4SampleDB*.

     ![Konfigurowanie parametrów połączenia docelowego](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image80.png "Konfigurowanie parametrów połączenia docelowego")

     *Konfigurowanie parametrów połączenia docelowego*
6. Następnie kliknij przycisk **OK**. Po wyświetleniu monitu o utworzenie bazy danych kliknij przycisk **tak**.

    ![Tworzenie bazy danych](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image81.png "Tworzenie ciągu bazy danych")

    *Tworzenie bazy danych*
7. Parametry połączenia, które będą używane do nawiązywania połączenia z SQL Database w systemie Windows Azure, są wyświetlane w domyślnym polu tekstowym połączenie. Następnie kliknij przycisk **Next** (Dalej).

    ![Parametry połączenia wskazujące SQL Database](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image82.png "Parametry połączenia wskazujące SQL Database")

    *Parametry połączenia wskazujące SQL Database*
8. Na stronie **Podgląd** kliknij przycisk **Publikuj**.

    ![Publikowanie aplikacji sieci Web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image83.png "Publikowanie aplikacji sieci Web")

    *Publikowanie aplikacji sieci Web*
9. Po zakończeniu procesu publikowania w domyślnej przeglądarce zostanie otwarta opublikowana witryna sieci Web.

    ![Aplikacja opublikowana w systemie Windows Azure](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image84.png "Aplikacja opublikowana w systemie Windows Azure")

    *Aplikacja opublikowana w systemie Windows Azure*

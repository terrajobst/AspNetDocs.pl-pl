---
uid: web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
title: Korzystanie z narzędzia Page Inspector dla programu Visual Studio 2012 w ASP.NET Web Forms | Microsoft Docs
author: rick-anderson
description: Page Inspector for Visual Studio 2012 to narzędzie deweloperskie dla sieci Web, które jest zintegrowane z przeglądarką. Zaznacz dowolny element w zintegrowanej przeglądarce i inspektor strony...
ms.author: riande
ms.date: 08/15/2012
ms.assetid: 2ece0bf4-aae5-4ff4-8f62-28e0819d4f86
msc.legacyurl: /web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: c165bbea505b4cb8eae1312cdd587f4ed36541a0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78575923"
---
# <a name="using-page-inspector-for-visual-studio-2012-in-aspnet-web-forms"></a>Korzystanie z narzędzia Page Inspector dla programu Visual Studio 2012 we wzorcu ASP.NET Web Forms

Autor Tim Ammann

> Page Inspector for Visual Studio 2012 to narzędzie deweloperskie dla sieci Web, które jest zintegrowane z przeglądarką. Zaznacz dowolny element w zintegrowanej przeglądarce, a Inspektor strony natychmiast podświetla źródło i kod CSS elementu. Możesz przeglądać dowolną stronę w aplikacji, szybko znajdować źródła renderowanego znacznika i korzystać z narzędzi przeglądarki bezpośrednio w środowisku programu Visual Studio.
> 
> W tym samouczku przedstawiono sposób włączania trybu inspekcji, a następnie szybkiego lokalizowania i edytowania reguł CSS i tekstu w projekcie sieci Web. W tym samouczku jest używany projekt aplikacji formularzy sieci Web, ale można również użyć Inspektora stron dla projektów witryny sieci Web i aplikacji [MVC](https://go.microsoft.com/?linkid=9802002) .
> 
> Samouczek zawiera następujące sekcje:
> 
> [Wymagania wstępne](#_1_prerequisites)
> 
> [Tworzenie aplikacji sieci Web](#_2_creating_a)
> 
> [Korzystanie z Inspektora stron do wyświetlania aplikacji](#_3_using_page)
> 
> [Włącz tryb inspekcji](#_4_inspection_mode)
> 
> [Używanie inspektora strony do wprowadzania zmian w znacznikach](#_5_using_page)
> 
> [Tryb inspekcji i okno HTML](#_6_inspection_mode)
> 
> [Podgląd zmian CSS w oknie stylów](#_7_previewing_css)
> 
> [Autosynchronizacja CSS](#css_auto_sync)
> 
> [Korzystanie z selektora kolorów CSS](#css_color_picker)

<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a>Wymagania wstępne

- [Program Visual Studio 2012](https://www.microsoft.com/visualstudio/11) lub [Visual Studio Express 2012 dla sieci Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).

> [!NOTE]
> Aby uzyskać najnowszą wersję inspektora stron, Zainstaluj zestaw Azure SDK dla programu .NET 2,0 przy użyciu [Instalatora platformy sieci Web](https://go.microsoft.com/fwlink/?LinkId=255386) .

Narzędzia Page Inspector są powiązane z Microsoft Web Developer Tools. Najnowsza wersja to 1,3. Aby sprawdzić posiadaną wersję, uruchom program Visual Studio i wybierz pozycję **Informacje o Microsoft Visual Studio** z menu **Pomoc** .

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a>Tworzenie aplikacji sieci Web

Najpierw utworzysz aplikację sieci Web, która będzie używana w programie Page Inspector. W programie Visual Studio wybierz kolejno pozycje **plik** &gt; **Nowy projekt**. Po lewej stronie rozwiń pozycję **C#Wizualizacja**, wybierz pozycję **Sieć Web**, a następnie wybierz pozycję **aplikacja formularzy sieci Web ASP.NET**.

![Nowa aplikacja formularzy sieci Web](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image1.png)

Kliknij przycisk **OK**.

Aplikacja zostanie otwarta w widoku **źródła** .

![Nowa aplikacja formularzy sieci Web w widoku źródła](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image2.png)

Teraz, gdy masz aplikację do współpracy z programem, możesz użyć Inspektora stron do jego sprawdzenia i modyfikacji.

<a id="_starting_page_inspector"></a><a id="_3_starting_page"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-view-the-application"></a>Korzystanie z Inspektora stron do wyświetlania aplikacji

Następnie zobaczysz aplikację za pomocą Inspektora stron. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt, a następnie wybierz polecenie **Widok w Inspektorze strony**.

![Wyświetl w Inspektorze strony](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image3.png)

Domyślnie, gdy inspektor strony jest uruchamiany po raz pierwszy, jest zadokowany jako wąskie okno po lewej stronie środowiska programu Visual Studio. Pozostaw to pole zadokowane po lewej stronie i ustaw dla niego szerokość, która jest odpowiednia dla Ciebie, lub Zadokuj ją w jednym z obszarów narzędzi na górze, na dole lub w prawo:

![Położenie dokowania inspektora stron](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image4.png)

Jeśli oddokusz okno Inspektora stron, możesz je umieścić poza programem Visual Studio, a nawet na drugim monitorze, jeśli go masz. Jednak aby można było ALT + TAB między inspektorem strony a programem Visual Studio, gdy okno Inspektora strony jest odzadokowane, przejdź do pozycji **narzędzia** &gt; **opcje** &gt; **środowisko** &gt; **karty i okna** **, a**następnie usuń zaznaczenie pola wyboru nazwanego okna **narzędziowego zawsze na wierzchu okna głównego**:

![Wyczyść pole wyboru przepływającego okna narzędzi, aby ALT + TAB między programem Visual Studio i niezadokowanym oknem inspektora strony](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image5.png)

Górne okienko okna inspektora stron pokazuje bieżącą stronę w oknie przeglądarki. W dolnym okienku zostanie wyświetlona strona w znaczniku HTML po lewej stronie, a niektóre karty po prawej stronie umożliwiają sprawdzenie różnych aspektów strony. Dolne okienko jest podobne do [Narzędzia deweloperskie F12](https://msdn.microsoft.com/ie/aa740478) w programie Internet Explorer. (Jednak w przeciwieństwie do narzędzi deweloperskich możesz użyć Inspektora stron bezpośrednio w programie Visual Studio).

![{1&gt;Inspektor strony&lt;1}](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image6.png)

W tym samouczku użyjesz okienka przeglądarki stron oraz kart **HTML** i **Style** , które ułatwią Ci szybkie nawigowanie i wprowadzanie zmian w aplikacji.

<a id="_4_inspection_mode"></a>
## <a name="enable-inspection-mode"></a>Włącz tryb inspekcji

Następnie zobaczysz, jak działa tryb inspekcji inspektora strony. W oknie Inspektora stron kliknij przycisk **Zbadaj** .

![Zbadaj element](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image7.png)

Aby wyświetlić tryb inspekcji w akcji, Przenieś wskaźnik myszy na różne części strony w oknie przeglądarki strony. Jak to zrobisz, wskaźnik myszy zmieni się na duży znak plus, a element poniżej zostanie wyróżniony:

![Przesuwanie nad blokiem DIV. Content-otoka](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image8.png)

Podczas przesuwania wskaźnika myszy należy zauważyć, że

- Zawartość w widoku **źródła** zmienia się, aby wyświetlić znaczniki odpowiadające wybranemu elementowi na stronie. Odpowiednie znaczniki są wyróżnione. Jeśli źródło znajduje się w innym pliku, plik zostanie otwarty w widoku źródła z wyróżnionym znacznikiem.

- Znaczniki wyświetlane na karcie **HTML** w Inspektorze strony również zmieniają się, aby odpowiadały wybranemu elementowi na stronie. Na karcie **HTML** odpowiednie znaczniki są opisane w opisie.

- Na karcie **Style** są wyświetlane reguły CSS odpowiednie dla bieżącego zaznaczenia.

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a>Używanie inspektora strony do wprowadzania zmian w znacznikach

Teraz zobaczysz, jak można użyć Inspektora strony do znajdowania i wprowadzania zmian w znacznikach lub tekstach, których lokalizacja może nie być od razu oczywista.

Umieść Inspektor strony w trybie inspekcji, a następnie przewiń w dół strony głównej.

Gdy tylko wprowadzisz obszar stopki, inspektor strony otworzy plik *. Master* w widoku **źródła** na karcie tymczasowej z prawej strony innych kart i podświetla wybraną sekcję na stronie wzorcowej. Pokazuje, w jaki sposób Inspektor strony może znaleźć i wyświetlić zawartość na stronie, która może faktycznie pochodzić z innego pliku niż ten, który został pierwotnie otwarty.

![Najważniejsze stopki w trybie inspekcji](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image9.png)

W oknie przeglądarki stron inspektora przesuń wskaźnik myszy nad wiersz z informacją o prawach <a id="a"> </a>autorskich.

Na stronie *site. Master* zostanie wyróżniony odpowiedni wiersz.

![Wyróżniono wiersz Copyright](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image10.png)

Dodaj tekst na końcu wiersza w pliku *site. Master* .

&lt;p&gt;&amp;Copy; &lt;%: DateTime. Now. Year%&gt;-my ASP.NET Application Rocks!&lt;/p&gt;

Teraz naciśnij kombinację klawiszy Ctrl + Alt + Enter lub kliknij pasek aktualizacji, aby wyświetlić wyniki w oknie przeglądarki stron inspektora.

![Moja aplikacja ASP.NET Rocks!](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image11.png)

Być może wiesz, że stopka znajdowała się na stronie *default. aspx* , ale wyłączono ją na stronie układu głównego, a Inspektor strony go wyszukał.

<a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a>Tryb inspekcji i okno HTML

Następnie będziesz mieć szybki podgląd okna HTML i sposobu, w jaki mapuje elementy.

Umieść Inspektor strony w trybie inspekcji.

![Zbadaj element](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image12.png)

Kliknij górną część strony, w której znajduje się tekst "Twoje logo". Badasz konkretny element bardziej szczegółowo, więc wyświetlanie w oknie przeglądarki nie zmienia się po przesunięciu wskaźnika myszy.

Teraz Przenieś wskaźnik myszy do okna **HTML** . Podczas przesuwania wskaźnika myszy, inspektor strony zawiera element w oknie **HTML** i podświetla odpowiadający element w oknie przeglądarki.

![Okno HTML](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image13.png)

Jak wcześniej, inspektor strony otworzy plik *site. Master* dla Ciebie na karcie tymczasowej. Kliknij kartę site. Master, a odpowiednie znaczniki są wyróżnione w sekcji &lt;nagłówek&gt;:

![Wyróżnione adiustacje](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image14.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a>Podgląd zmian CSS w oknie stylów

Następnie zobaczysz, jak można użyć okna **stylów** inspektora strony do podglądu zmian w CSS.

Kliknij przycisk **Sprawdź** , aby umieścić inspektora strony w trybie inspekcji.

W oknie przeglądarki stron inspektora przesuń wskaźnik myszy nad sekcję "Strona główna", aż zostanie wyświetlona etykieta **DIV. Content-otoka** .

![Przesuwanie nad elementami](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image15.png)

Kliknij jeden z sekcji DIV. Content-otoka, a następnie przesuń wskaźnik myszy do okna **Style** . W selektorze klas. Polecane. Content-otoka Usuń zaznaczenie pola wyboru dla właściwości Background-Color.

![Wyczyść kolor tła](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image16.png)

Zwróć uwagę, jak natychmiast przeglądanie zmian w oknie przeglądarki stron.

Zaznacz pole wyboru ponownie, a następnie kliknij dwukrotnie wartość właściwości i zmień ją na `red`. Zmiana zostanie wyświetlona natychmiast:

![Czerwony kolor tła](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image17.png)

Okno **Style** ułatwia testowanie i Podgląd zmian CSS przed zatwierdzeniem zmian w arkuszu stylów.

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a>Autosynchronizacja CSS

> [!NOTE]
> Ta funkcja wymaga wersji 1,3 inspektora strony.

Funkcja autosynchronizacji CSS umożliwia bezpośrednie edytowanie pliku CSS i natychmiastowe wyświetlanie zmian w przeglądarce stron.

Kliknij pozycję **Sprawdź** , aby umieścić Inspektor strony w trybie inspekcji.

W przeglądarce inspektora stron przesuń wskaźnik myszy nad sekcję "Strona główna" do momentu, gdy zostanie wyświetlona etykieta **DIV. Content-otoka** . Kliknij raz, aby zaznaczyć ten element.

W oknie **Style** są wyświetlane wszystkie reguły CSS dla tego elementu. Przewiń w dół, aby znaleźć selektor klasy. Polecane. Content-otoka. Kliknij pozycję ". Polecane. Content-otoka". Inspektor strony otwiera plik CSS, który definiuje ten styl (site. CSS) i podświetla odpowiedni styl CSS.

![Plik CSS](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image18.png)

Teraz zmień wartość dla `background-color` na "Red". Zmiana zostanie wyświetlona natychmiast w przeglądarce inspektora stron.

![Przeglądarka stron inspektora](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image19.png)

<a id="css_color_picker"></a>

## <a name="using-the-css-color-picker"></a>Korzystanie z selektora kolorów CSS

Następnie dowiesz się, jak za pomocą Inspektora stron szybko znajdować i zmieniać arkusz CSS dla wyróżnionego tekstu w aplikacji domyślnej. W tym przykładzie postanowisz, że nie jest to niebieskie wyróżnianie i chcesz zmienić kolor na inny.

Kliknij przycisk **Zbadaj** .

![Zbadaj element](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image20.png)

W oknie przeglądarki stron inspektora przesuń wskaźnik myszy nad wyróżniony tekst "wideo, samouczki i przykłady", aby pojawił się etykieta CSS "Mark".

![Przesuwanie nad elementem znacznika](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image21.png)

Kliknij tekst, aby go zaznaczyć. Odpowiedni selektor znacznika CSS pojawia się u dołu okna **Style** .

![Oznacz selektor w oknie stylów](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image22.png)

Kliknij selektor znacznika. Spowoduje to otwarcie pliku *. css* dla aplikacji sieci Web. Kliknij kartę site. CSS, a odpowiadająca jej CSS selektora zostanie wyróżniona:

![Oznacz selektor w arkuszu stylów](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image23.png)

Zaznacz i Usuń wiersz z właściwością Background-Color.

Teraz będziesz używać nowego selektora kolorów CSS programu Visual Studio 2012, aby wybrać nowy kolor dla właściwości tło **znacznika** .

<a id="_using_the_visual"></a>

### <a name="using-the-visual-studio-2012-css-color-picker"></a>Korzystanie z selektora kolorów CSS programu Visual Studio 2012

Edytor CSS w programie Visual Studio 2012 ma selektor kolorów, dzięki czemu można łatwo wybierać i wstawiać kolory. Ma prosty pasek koloru i selektor "podręczny", który oferuje dokładniejszą kontrolę.

Selektor kolorów zawiera standardową paletę kolorów, obsługuje standardowe nazwy kolorów, kody skrótów, kolory RGB, RGBA, HSL i HSLA, a także zachowuje listę kolorów używanych ostatnio w dokumencie.

W wierszu, w którym zainstalowano Właściwość Background-Color, wpisz "BC" i naciśnij klawisz Strzałka w dół.

Po wpisaniu pierwszego znaku każdego wyrazu w właściwości rozdzielanej łącznikami, takiej jak "Background-Color", IntelliSense filtruje listę, aby wyświetlić tylko pasujące właściwości:

![Przefiltrowane wartości IntelliSense](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image24.png)

Teraz wpisz dwukropek. Gdy to zrobisz, zostanie wstawiona pełna nazwa właściwości Background. Typ **#** lub **RGB (** i zostanie wyświetlony pasek selektora kolorów:

![Pasek selektora kolorów CSS](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image25.png)

Aby zobaczyć, jak działa pasek selektora kolorów, kliknij jego kolory za pomocą wskaźnika myszy lub naciśnij klawisz Strzałka w dół, a następnie użyj klawiszy strzałek w lewo i w prawo, aby przechodzenie między kolorami. Podczas odwiedzania koloru wyświetlana jest odpowiednia wartość właściwości Background-Color:

![Podgląd — wartość właściwości koloru w tle](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image26.png)

W tym momencie można nacisnąć klawisz ENTER, aby wybrać wartość, a następnie średnik (;) Aby ukończyć wprowadzanie wpisów CSS. Na razie przejdź do następnej sekcji, aby zobaczyć, jak działa selektor kolorów.

#### <a name="using-the-color-picker-pop-down"></a>Korzystanie z okna podręcznego selektora kolorów

Gdy pasek koloru nie ma dokładnego koloru, którego szukasz, możesz użyć selektora kolorów.

Aby go otworzyć, kliknij podwójny cudzysłów ostrokątny na prawym końcu paska kolorów lub naciśnij klawisz Strzałka w dół jeden raz lub dwukrotnie na klawiaturze.

![Selektor kolorów CSS — wyskakujące okienko](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image27.png)

Kliknij kolor z pionowego paska po prawej stronie. Spowoduje to wyświetlenie gradientu dla tego koloru w oknie głównym. Wybierz kolor bezpośrednio z paska pionowego, naciskając klawisz ENTER, lub kliknij dowolny punkt w oknie głównym, aby wybrać większą precyzję.

Jeśli na ekranie komputera istnieje kolor, który ma być używany (nie musi znajdować się w interfejsie użytkownika programu Visual Studio), możesz przechwycić jego wartość za pomocą narzędzia Kroplomierz w prawym dolnym rogu.

Możesz również zmienić nieprzezroczystość koloru, przesuwając suwak w dolnej części selektora kolorów. Spowoduje to zmianę wartości koloru na wartości RGBA, ponieważ format RGBA może reprezentować nieprzezroczystość.

Po wybraniu koloru naciśnij klawisz ENTER, a następnie wpisz średnik, aby zakończyć wprowadzanie koloru tła w pliku *site. css* .

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a>Pasek aktualizacji inspektora stron

Inspektor strony natychmiast wykrywa zmiany w pliku *site. css* (lub w dowolnym pliku w aplikacji) i wyświetla alert na pasku aktualizacji.

![Pasek aktualizacji](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image28.png)

Aby zapisać wszystkie pliki i odświeżyć przeglądarkę inspektora stron, naciśnij kombinację klawiszy Ctrl + Alt + Enter lub kliknij pasek aktualizacji. Zmiana w kolorze wyróżnienia zostanie wyświetlona w przeglądarce:

![Zmieniono kolor wyróżnienia](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image29.png)

<a id="_using_page_inspector_1"></a>Należy zauważyć, że wygodnie odświeżono przeglądarkę inspektora stron bezpośrednio z poziomu środowiska programu Visual Studio. Korzystanie z narzędzia Page Inspector zamiast zewnętrznej przeglądarki pozwala pozostać w edytorze podczas opracowywania aplikacji sieci Web.

## <a name="see-also"></a>Zobacz też

[Wprowadzenie do Inspektora stron](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector) (wideo Channel 9)

[Komunikaty o błędach inspektora stron](https://go.microsoft.com/?linkid=9813062) (MSDN)

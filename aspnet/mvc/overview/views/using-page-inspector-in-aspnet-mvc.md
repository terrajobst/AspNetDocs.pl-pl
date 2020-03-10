---
uid: mvc/overview/views/using-page-inspector-in-aspnet-mvc
title: Korzystanie z narzędzia Page Inspector w ASP.NET MVC | Microsoft Docs
author: rick-anderson
description: Narzędzie Page Inspector w programie Visual Studio 2012 jest narzędziem do programowania w sieci Web za pomocą zintegrowanej przeglądarki. Zaznacz dowolny element w zintegrowanej przeglądarce i inspektor strony i...
ms.author: riande
ms.date: 08/15/2012
ms.assetid: c7e4e1ab-4932-4614-9f53-aaf7c706d498
msc.legacyurl: /mvc/overview/views/using-page-inspector-in-aspnet-mvc
msc.type: authoredcontent
ms.openlocfilehash: 5da3e142c52a770f59222c21d9f9a53cbbdbf498
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78538018"
---
# <a name="using-page-inspector-in-aspnet-mvc"></a>Korzystanie z narzędzia Page Inspector we wzorcu ASP.NET MVC

Autor Tim Ammann

> Narzędzie Page Inspector w programie Visual Studio 2012 jest narzędziem do programowania w sieci Web za pomocą zintegrowanej przeglądarki. Zaznacz dowolny element w zintegrowanej przeglądarce, a Inspektor strony natychmiast podświetla źródło i kod CSS elementu. Możesz przeglądać dowolny widok MVC, szybko znajdować źródła renderowanego znacznika i korzystać z narzędzi przeglądarki bezpośrednio w środowisku programu Visual Studio.
> 
> [Obejrzyj wideo](../../videos/mvc-4/using-page-inspector-in-aspnet-mvc.md)
> 
> W tym samouczku pokazano, jak włączyć tryb inspekcji, a następnie szybko zlokalizować i edytować znaczniki i CSS w projekcie sieci Web. Samouczek używa projektu MVC, ale można również użyć Inspektora stron do [formularzy sieci Web](https://go.microsoft.com/?linkid=9802001) i innych aplikacji ASP.NET.
> 
> Samouczek zawiera następujące sekcje:
> 
> - [Wymagania wstępne](#_1_prerequisites)
> - [Tworzenie aplikacji sieci Web](#_2_creating_a)
> - [Użyj Inspektora stron, aby przejść do widoku](#_3_using_page)
> - [Włącz tryb inspekcji](#_4_inspection_mode)
> - [Używanie inspektora strony do wprowadzania zmian w znacznikach](#_5_using_page)
> - [Tryb inspekcji i okno HTML](#_6_inspection_mode)
> - [Podgląd zmian CSS w oknie stylów](#_7_previewing_css)
> - [Autosynchronizacja CSS](#css_auto_sync)
> - [Korzystanie z selektora kolorów CSS](#css_color_picker)
> - [Mapowanie dynamicznych elementów strony na JavaScript](#map_dynamic_elements)

<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a>Wymagania wstępne

- [Program Visual Studio 2012](https://www.microsoft.com/visualstudio/11) lub [Visual Studio Express 2012 dla sieci Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).

> [!NOTE]
> Aby uzyskać najnowszą wersję inspektora stron, użyj [Instalatora platformy sieci Web](https://go.microsoft.com/fwlink/?LinkId=255386) w celu zainstalowania zestawu Windows Azure SDK dla programu .NET 2,0.

Narzędzia Page Inspector są powiązane z Microsoft Web Developer Tools. Najnowsza wersja to 1,3. Aby sprawdzić posiadaną wersję, uruchom program Visual Studio i wybierz pozycję **Informacje o Microsoft Visual Studio** z menu **Pomoc** .

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a>Tworzenie aplikacji sieci Web

Najpierw Utwórz aplikację sieci Web, która będzie używana w programie Page Inspector. W programie Visual Studio wybierz kolejno pozycje **plik** &gt; **Nowy projekt**. Po lewej stronie rozwiń pozycję **C#Wizualizacja**, wybierz pozycję **Sieć Web**, a następnie wybierz pozycję **aplikacja sieci Web ASP.NET MVC4**.

![Nowa aplikacja ASP.NET MVC](using-page-inspector-in-aspnet-mvc/_static/image2.png)

Kliknij przycisk **OK**.

W oknie dialogowym **Nowy projekt ASP.NET MVC 4** wybierz pozycję **aplikacja internetowa**. Pozostaw **Razor** jako domyślny aparat widoku.

![Nowy projekt ASP.NET MVC — aplikacja internetowa](using-page-inspector-in-aspnet-mvc/_static/image4.png)

Aplikacja zostanie otwarta w widoku **źródła** .

![Nowa aplikacja ASP.NET MVC w widoku źródła](using-page-inspector-in-aspnet-mvc/_static/image6.png)

Teraz, gdy masz aplikację do współpracy z programem, możesz użyć Inspektora stron do jego sprawdzenia i modyfikacji.

<a id="_starting_page_inspector"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-browse-to-a-view"></a>Użyj Inspektora stron, aby przejść do widoku

W programie Visual Studio 2012 można kliknąć prawym przyciskiem myszy dowolny widok w projekcie, wybrać pozycję **Widok w Inspektorze strony**, a Inspektor strony wystawia trasę i wyświetlić stronę.

W **Eksplorator rozwiązań**rozwiń folder **widoki** , a następnie folder **macierzysty** . Kliknij prawym przyciskiem myszy plik index. cshtml i wybierz polecenie **Widok w Inspektorze strony**.

![Wyświetl index. cshtml w Inspektorze strony](using-page-inspector-in-aspnet-mvc/_static/image8.png)

Domyślnie Inspektor strony jest zadokowany jako okno po lewej stronie środowiska programu Visual Studio. Jeśli wolisz, możesz zadokować ją w innym miejscu lub oddokować okno. Zobacz [How to: porządkowanie i Dokowanie okien](https://msdn.microsoft.com/library/z4y0hsax.aspx).

Górne okienko okna inspektora stron pokazuje bieżącą stronę w oknie przeglądarki. W dolnym okienku zostanie wyświetlona strona znaczników HTML wraz z niektórymi kartami, które umożliwiają badanie różnych aspektów strony. Dolne okienko jest podobne do [Narzędzia deweloperskie F12](https://msdn.microsoft.com/ie/aa740478) w programie Internet Explorer.

![Aplikacja ASP.NET MVC w Inspektorze strony](using-page-inspector-in-aspnet-mvc/_static/image10.png)

W tym samouczku zostaną użyte karty **HTML** i **Style** umożliwiające szybkie nawigowanie i wprowadzanie zmian w aplikacji.

<a id="_examining_(&quot;decomposing&quot;)_the"></a><a id="_inspection_mode_and"></a><a id="_4_inspection_mode"></a>

## <a name="enableinspection-mode"></a>Tryb EnableInspection

Aby umieścić inspektora stron w trybie inspekcji, kliknij przycisk **Zbadaj** . W trybie inspekcji, gdy przetrzymasz wskaźnik myszy na dowolnej części renderowanej strony, odpowiednie oznakowanie źródłowe lub kod jest wyróżniony.

![Przełącz tryb inspekcji](using-page-inspector-in-aspnet-mvc/_static/image12.png)

Teraz Przenieś wskaźnik myszy na różne części strony w Inspektorze strony. Jak to zrobisz, wskaźnik myszy zmieni się na duży znak plus, a element poniżej zostanie wyróżniony:

![Przesuwanie nad blokiem DIV. Content-otoka](using-page-inspector-in-aspnet-mvc/_static/image14.png)

Podczas przesuwania wskaźnika myszy program Visual Studio podświetla odpowiednie składnia Razor w pliku źródłowym. Jeśli element HTML pochodzi z innego pliku źródłowego, program Visual Studio automatycznie otworzy ten plik.

W Inspektorze strony karta **HTML** zawiera kod HTML, który został wygenerowany na podstawie składnia Razor. Podczas przesuwania wskaźnika myszy elementy HTML są wyróżniane. Na karcie **Style** są wyświetlane reguły CSS dla elementu.

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a>Używanie inspektora strony do wprowadzania zmian w znacznikach

Inspektor strony umożliwia znalezienie znaczników, których lokalizacja może nie być oczywista. Następnie można zmodyfikować znaczniki i zobaczyć wyniki zmian.

Aby to sprawdzić, kliknij pozycję **Inspekcja** , a następnie przewiń w dół strony w oknie Inspektora strony.

Po przesunięciu wskaźnika myszy do obszaru stopka, inspektor strony otwiera plik \_Layout. cshtml i podświetla sekcję strony układu, która została wybrana. Jak widać, stopka jest zdefiniowana w pliku układu, a nie samego widoku.

![Stop](using-page-inspector-in-aspnet-mvc/_static/image16.png)

Teraz Przenieś wskaźnik myszy nad wiersz z informacją o prawach autorskich <a id="a"> </a>. Na stronie \_Layout. cshtml zostanie wyróżniony odpowiedni wiersz.

![Wyróżniono wiersz Copyright](using-page-inspector-in-aspnet-mvc/_static/image18.png)

Dodaj tekst na końcu wiersza w pliku \_Layout. cshtml.

&lt;p&gt;&amp;Copy; @DateTime.Now.Year — moja aplikacja ASP.NET MVC Rocks!&lt;/p&gt;

Teraz naciśnij kombinację klawiszy Ctrl + Alt + Enter lub kliknij pasek aktualizacji, aby wyświetlić wyniki w oknie przeglądarki stron inspektora.

![Moja aplikacja ASP.NET Rocks!](using-page-inspector-in-aspnet-mvc/_static/image20.png)

Być może wiesz, że stopka zdefiniowana w indeksie index. cshtml, ale wyłączono ją w \_Layout. cshtml, a Inspektor strony go wyszukał.

<a id="_inspection_mode_and_1"></a><a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a>Tryb inspekcji i okno HTML

Następnie będziesz mieć szybki podgląd okna HTML i sposobu, w jaki mapuje elementy.

Kliknij pozycję **Sprawdź** , aby umieścić Inspektor strony w trybie inspekcji.

Kliknij górną część strony, w której znajduje się tekst "Twoje logo". Badasz konkretny element bardziej szczegółowo, więc wyświetlanie w oknie przeglądarki nie zmienia się po przesunięciu wskaźnika myszy.

Teraz Przenieś wskaźnik myszy do okna **HTML** . Podczas przesuwania wskaźnika myszy, inspektor strony zawiera element w oknie **HTML** i podświetla odpowiadający element w oknie przeglądarki.

![Okno HTML](using-page-inspector-in-aspnet-mvc/_static/image22.png)

Jak wcześniej, inspektor strony otwiera plik \_Layout. cshtml w karcie tymczasowej. Kliknij kartę tymczasową \_układ. cshtml, a odpowiednie adiustacje zostaną wyróżnione w sekcji&gt; nagłówka &lt;:

![Wyróżnione adiustacje](using-page-inspector-in-aspnet-mvc/_static/image24.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a>Podgląd zmian CSS w oknie stylów

Następnie użyjesz okna **stylów** inspektora strony do podglądu zmian w CSS.

Kliknij pozycję **Sprawdź** , aby umieścić Inspektor strony w trybie inspekcji.

W oknie przeglądarki stron inspektora przesuń wskaźnik myszy nad sekcję "Strona główna", aż zostanie wyświetlona etykieta **DIV. Content-otoka** .

![Przesuwanie nad blokiem DIV. Content-otoka](using-page-inspector-in-aspnet-mvc/_static/image26.png)

Kliknij jeden z sekcji DIV. Content-otoka, a następnie przesuń wskaźnik myszy do okna **Style** . W oknie **Style** są wyświetlane wszystkie reguły CSS dla tego elementu. Przewiń w dół, aby znaleźć selektor klasy. Polecane. Content-otoka. Teraz wyczyść pole wyboru dla właściwości Background-Color.

![Wyczyść kolor tła](using-page-inspector-in-aspnet-mvc/_static/image28.png)

Zwróć uwagę, jak natychmiast przeglądanie zmian w oknie przeglądarki stron.

Zaznacz pole wyboru ponownie, a następnie kliknij dwukrotnie wartość właściwości i zmień ją na czerwony. Zmiana zostanie wyświetlona natychmiast:

![Czerwony kolor tła](using-page-inspector-in-aspnet-mvc/_static/image30.png)

Okno **Style** ułatwia testowanie i Podgląd zmian CSS przed zatwierdzeniem zmian w arkuszu stylów.

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a>Autosynchronizacja CSS

> [!NOTE]
> Ta funkcja wymaga wersji 1,3 inspektora strony.

Funkcja autosynchronizacji CSS umożliwia bezpośrednie edytowanie pliku CSS i natychmiastowe wyświetlanie zmian w przeglądarce stron.

Kliknij pozycję **Sprawdź** , aby umieścić Inspektor strony w trybie inspekcji.

W przeglądarce inspektora stron przesuń wskaźnik myszy nad sekcję "Strona główna" do momentu, gdy zostanie wyświetlona etykieta **DIV. Content-otoka** . Kliknij raz, aby zaznaczyć ten element.

W oknie **Style** są wyświetlane wszystkie reguły CSS dla tego elementu. Przewiń w dół, aby znaleźć selektor klasy. Polecane. Content-otoka. Kliknij pozycję ". Polecane. Content-otoka". Inspektor strony otwiera plik CSS, który definiuje ten styl (site. CSS) i podświetla odpowiedni styl CSS.

![](using-page-inspector-in-aspnet-mvc/_static/image32.png)

Teraz zmień wartość dla `background-color` na "Red". Zmiana zostanie wyświetlona natychmiast w przeglądarce inspektora stron.

![](using-page-inspector-in-aspnet-mvc/_static/image34.png)

<a id="css_color_picker"></a>
## <a name="using-the-css-color-picker"></a>Korzystanie z selektora kolorów CSS

Edytor CSS w programie Visual Studio 2012 ma selektor kolorów, dzięki czemu można łatwo wybierać i wstawiać kolory. Selektor kolorów zawiera standardową paletę kolorów, obsługuje standardowe nazwy kolorów, kody skrótów, kolory RGB, RGBA, HSL i HSLA, a także zachowuje listę kolorów używanych ostatnio w dokumencie.

W poprzedniej sekcji zmieniono wartość właściwości `background-color`. Aby wywołać selektor kolorów, umieść punkt wstawiania po nazwie właściwości i wpisz **#** lub **RGB (** .

![Pasek selektora kolorów CSS](using-page-inspector-in-aspnet-mvc/_static/image36.png)

Kliknij kolor, aby go zaznaczyć, lub naciśnij klawisz Strzałka w dół, a następnie użyj klawiszy strzałek w lewo i w prawo, aby przechodzenie między kolorami. Podczas odwiedzania koloru wyświetlana jest odpowiednia wartość szesnastkowa:

![Podgląd — wartość właściwości koloru w tle](using-page-inspector-in-aspnet-mvc/_static/image38.png)

Jeśli pasek koloru nie ma dokładnego koloru, możesz użyć selektora kolorów. Aby go otworzyć, kliknij podwójny cudzysłów ostrokątny na prawym końcu paska kolorów lub naciśnij klawisz Strzałka w dół jeden raz lub dwukrotnie na klawiaturze.

![Selektor kolorów CSS — wyskakujące okienko](using-page-inspector-in-aspnet-mvc/_static/image40.png)

Kliknij kolor z pionowego paska po prawej stronie. Spowoduje to wyświetlenie gradientu dla tego koloru w oknie głównym. Wybierz kolor bezpośrednio z paska pionowego, naciskając klawisz ENTER, lub kliknij dowolny punkt w oknie głównym, aby wybrać większą precyzję.

Jeśli na ekranie komputera istnieje kolor, który ma być używany (nie musi znajdować się w interfejsie użytkownika programu Visual Studio), możesz przechwycić jego wartość za pomocą narzędzia Kroplomierz w prawym dolnym rogu.

Możesz również zmienić nieprzezroczystość koloru, przesuwając suwak w dolnej części selektora kolorów. Spowoduje to zmianę wartości koloru na wartości RGBA, ponieważ format RGBA może reprezentować nieprzezroczystość.

Po wybraniu koloru naciśnij klawisz ENTER, a następnie wpisz średnik, aby zakończyć wprowadzanie koloru tła w pliku *site. css* .

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a>Pasek aktualizacji inspektora stron

Inspektor strony natychmiast wykrywa zmiany w pliku *site. css* i wyświetla alert na pasku aktualizacji.

![Pasek aktualizacji](using-page-inspector-in-aspnet-mvc/_static/image42.png)

Aby zapisać wszystkie pliki i odświeżyć przeglądarkę inspektora stron, naciśnij kombinację klawiszy Ctrl + Alt + Enter lub kliknij pasek aktualizacji. Zmiana w kolorze wyróżnienia zostanie wyświetlona w przeglądarce.

<a id="map_dynamic_elements"></a>
## <a name="mapping-dynamic-page-elements-to-javascript"></a>Mapowanie dynamicznych elementów strony na JavaScript

W nowoczesnych aplikacjach sieci Web elementy na stronie są często generowane dynamicznie przy użyciu języka JavaScript. Oznacza to, że nie istnieje statyczny znacznik (HTML lub Razor) odpowiadający tym elementom strony.

W wersji 1,3, inspektor strony może teraz mapować elementy, które zostały dynamicznie dodane do strony z powrotem do odpowiedniego kodu JavaScript. Aby zademonstrować tę funkcję, użyjemy [szablonu aplikacji jednostronicowej (Spa)](../../../single-page-application/overview/introduction/knockoutjs-template.md).

> [!NOTE]
> Szablon SPA wymaga aktualizacji [ASP.NET and Web Tools 2012,2](https://go.microsoft.com/fwlink/?LinkId=282650) .

W programie Visual Studio wybierz kolejno pozycje **plik** &gt; **Nowy projekt**. Po lewej stronie rozwiń pozycję **C#Wizualizacja**, wybierz pozycję **Sieć Web**, a następnie wybierz pozycję **aplikacja sieci Web ASP.NET MVC4**. Kliknij przycisk **OK**.

W oknie dialogowym **Nowy projekt ASP.NET MVC 4** wybierz pozycję **aplikacja jednostronicowa**.

W Eksplorator rozwiązań rozwiń folder **widoki** , a następnie folder **macierzysty** . Kliknij prawym przyciskiem myszy plik index. cshtml i wybierz polecenie **Widok w Inspektorze strony**.

Pierwszy element, który jest wyświetlany w przeglądarce stron, jest stroną logowania. Kliknij pozycję "Utwórz konto" i Utwórz nazwę użytkownika i hasło. Po zarejestrowaniu aplikacja zostanie zarejestrowana w usłudze i zostanie utworzona lista czynności do wykonania z niektórymi Przykładowymi elementami.

Kliknij pozycję **Sprawdź** , aby umieścić Inspektor strony w trybie inspekcji. W przeglądarce inspektora stron kliknij jeden z elementów do wykonania. Należy zauważyć, że zamiast wyróżnienia w kolorze niebieskim element jest wyróżniony w pomarańczowym, z "JS" obok nazwy elementu. Oznacza to, że element został utworzony dynamicznie za pomocą skryptu.

![](using-page-inspector-in-aspnet-mvc/_static/image44.png)

Ponadto do karty **stosu wywołań** pojawia się pomarańczowe podkreolenie. Oznacza to, że okienko **stosu wywołań** zawiera więcej informacji na temat elementu.

Kliknij kartę **stos wywołań** . W okienku **stos wywołań** jest wyświetlany stos wywołań dla wywołania JavaScript, które utworzyło element. Wywołania bibliotek zewnętrznych, takich jak jQuery, są zwijane, dzięki czemu można łatwo zobaczyć wywołania skryptu aplikacji.

![](using-page-inspector-in-aspnet-mvc/_static/image46.png)

Aby wyświetlić pełny stos, w tym wywołania bibliotek zewnętrznych, można rozwinąć węzły z etykietą "biblioteki zewnętrzne":

![](using-page-inspector-in-aspnet-mvc/_static/image48.png)

Po kliknięciu elementu w stosie wywołań program Visual Studio otwiera plik kodu i podświetla odpowiedni skrypt.

![](using-page-inspector-in-aspnet-mvc/_static/image50.png)

## <a name="see-also"></a>Zobacz też

[Wprowadzenie do ASP.NET MVC 4 za pomocą programu Visual Studio](../older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md) (ASP.NET Web)

[Wprowadzenie do Inspektora stron](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector/) (wideo Channel 9)

[Komunikaty o błędach inspektora stron](https://go.microsoft.com/?linkid=9813062) (MSDN)

---
uid: visual-studio/overview/2013/visual-studio-2013-web-tools
title: 'Wskazówki dotyczące laboratorium: Visual Studio 2013 narzędzia sieci Web | Microsoft Docs'
author: rick-anderson
description: Program Visual Studio to doskonałe środowisko programistyczne dla programu. Oparte na sieci .NET i projekty sieci Web. Zawiera on zaawansowany edytor tekstu, który może być łatwo używany do...
ms.author: riande
ms.date: 07/16/2014
ms.assetid: 09e82351-816b-402d-acd1-0f9ac6901d16
msc.legacyurl: /visual-studio/overview/2013/visual-studio-2013-web-tools
msc.type: authoredcontent
ms.openlocfilehash: 2fb987dd9b26ad9f0e8a88fd881bde4505ec4148
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622676"
---
# <a name="hands-on-lab-visual-studio-2013-web-tools"></a>Ćwiczenia praktyczne: narzędzia Web Tools dla programu Visual Studio 2013

przez [zespół Camp sieci Web](https://twitter.com/webcamps)

[Pobierz zestaw szkoleniowy dla sieci Web Camp](https://aka.ms/webcamps-training-kit)

> Program Visual Studio to doskonałe środowisko programistyczne dla programu. Oparte na sieci .NET i projekty sieci Web. Zawiera on zaawansowany edytor tekstu, który może być łatwo używany do edytowania plików autonomicznych bez projektu.
> 
> Program Visual Studio utrzymuje w pełni funkcjonalne drzewo analizy podczas edycji każdego pliku. Dzięki temu program Visual Studio może zapewnić niezrównane akcje autouzupełniania i oparte na dokumentach, a jednocześnie sprawiać, że programowanie znacznie szybsze i bardziej przyjemne. Te funkcje są szczególnie zaawansowane w dokumentach HTML i CSS.
> 
> Wszystkie te możliwości są również dostępne dla rozszerzeń, ułatwiając rozszerzanie edytorów o zaawansowane nowe funkcje zgodnie z potrzebami. Web Essentials to zbiór (głównie) ulepszeń związanych z witryną sieci Web w programie Visual Studio. Obejmuje to wiele nowych zaawansowania IntelliSense (szczególnie w przypadku CSS), nowych funkcji linków przeglądarki, automatycznych JSHint dla plików JavaScript, nowych ostrzeżeń dotyczących języka HTML i CSS oraz wielu innych funkcji, które są niezbędne do nowoczesnego programowania w sieci Web.
> 
> Wszystkie przykładowe kod i fragmenty kodu są zawarte w zestawie szkoleń w sieci Web Camp, które są dostępne w [https://aka.ms/webcamps-training-kit](https://aka.ms/webcamps-training-kit).

<a id="Overview"></a>
## <a name="overview"></a>Omówienie

<a id="Objectives"></a>
### <a name="objectives"></a>Cele

W tym ćwiczeniu dowiesz się, jak:

- Korzystaj z nowych funkcji edytora HTML zawartych w programie Web Essentials, takich jak bogate fragmenty kodu HTML5 i kodowanie Zen
- Korzystanie z nowych funkcji edytora CSS zawartych w programie Web Essentials, takich jak selektor kolorów i etykietka narzędzia macierzy przeglądarki
- Korzystanie z nowych funkcji edytora JavaScript zawartych w programie Web Essentials, takich jak Wyodrębnianie do plików i IntelliSense dla wszystkich elementów HTML
- Wymiana danych między przeglądarką i programem Visual Studio przy użyciu linku przeglądarki

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Wymagania wstępne

Następujące czynności są wymagane do wykonania tego laboratorium praktycznego:

- [Microsoft Visual Studio Professional 2013](https://www.microsoft.com/visualstudio/) lub więcej
- [Web Essentials 2013](http://vswebessentials.com/)
- [Google Chrome](https://www.google.com/chrome/)

<a id="Setup"></a>
### <a name="setup"></a>Konfigurowanie

Aby można było uruchomić ćwiczenia w tym ćwiczeniu, należy najpierw skonfigurować środowisko.

1. Otwórz okno Eksploratora Windows i przejdź do folderu **źródłowego** laboratorium.
2. Kliknij prawym przyciskiem myszy **Setup. cmd** i wybierz polecenie **Uruchom jako administrator** , aby uruchomić proces instalacji, który skonfiguruje środowisko i zainstaluje fragmenty kodu programu Visual Studio dla tego laboratorium.
3. Jeśli zostanie wyświetlone okno dialogowe Kontrola konta użytkownika, potwierdź akcję, aby wykonać operację.

> [!NOTE]
> Upewnij się, że wszystkie zależności dla tego laboratorium zostały sprawdzone przed uruchomieniem Instalatora.

<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>Używanie fragmentów kodu

W całym dokumencie laboratoryjnym pojawi się monit o wstawienie bloków kodu. Dla wygody większość tego kodu jest udostępniana jako fragmenty Visual Studio Code, do których można uzyskać dostęp z poziomu Visual Studio 2013, aby uniknąć konieczności ręcznego dodawania go.

> [!NOTE]
> Każdemu z nich towarzyszy rozpoczęcie rozwiązanie znajdujące się w folderze **BEGIN** w ćwiczeniu, który umożliwia wykonywanie poszczególnych czynności niezależnie od innych. Należy pamiętać, że fragmenty kodu dodawane podczas wykonywania nie są dostępne w tych rozwiązaniach i mogą nie działać do czasu ukończenia ćwiczenia. Wewnątrz kodu źródłowego dla ćwiczenia znajdziesz również folder **końcowy** zawierający rozwiązanie programu Visual Studio z kodem, który wynika z wykonania czynności w odpowiednim ćwiczeniu. Te rozwiązania można wykorzystać jako wskazówkę, jeśli potrzebujesz dodatkowej pomocy podczas pracy w ramach tego praktycznego laboratorium.

---

<a id="Exercises"></a>
## <a name="exercises"></a>Symulacyjn

To laboratorium praktyczne obejmuje następujące ćwiczenia:

1. [Praca z linkiem przeglądarki i programem Web Essentials](#Exercise1)
2. [Korzystanie z fragmentów kodu i technologii IntelliSense](#Exercise2)

> [!NOTE]
> Po pierwszym uruchomieniu programu Visual Studio należy wybrać jedną z wstępnie zdefiniowanych kolekcji ustawień. Każda wstępnie zdefiniowana kolekcja jest zaprojektowana tak, aby była zgodna z konkretnym stylem deweloperskim i określa układy okien, zachowanie edytora, fragmenty kodu IntelliSense i opcje okna dialogowego. Procedury przedstawione w tym laboratorium opisują akcje niezbędne do wykonania danego zadania w programie Visual Studio, gdy jest używana **Ogólna kolekcja ustawień deweloperskich** . W przypadku wybrania innej kolekcji ustawień dla środowiska programistycznego mogą wystąpić różnice w czynnościach, które należy wziąć pod uwagę.

<a id="Exercise1"></a>
### <a name="exercise-1-working-with-browser-link-and-web-essentials"></a>Ćwiczenie 1: Praca z linkiem przeglądarki i programem Web Essentials

**Web Essentials** to rozszerzenie programu Visual Studio, które dodaje różne użyteczne funkcje do nowoczesnego programowania w sieci Web, głównie koncentrując się na znacznie szybszym i bardziej przyjemnym środowisku programistycznym dla sieci Web. Program Web Essentials można zainstalować z galerii rozszerzeń w programie Visual Studio.

**Link do przeglądarki** to nowa funkcja dostępna w Visual Studio 2013, która udostępnia kanał między ŚRODOWISKiem IDE programu Visual Studio i dowolną otwartą przeglądarką do wymiany danych między aplikacją sieci Web a programem Visual Studio. Web Essentials rozszerza link przeglądarki za pomocą narzędzi do manipulowania modelem obiektów DOM i stylami CSS stron sieci Web bezpośrednio z przeglądarki.

W tym ćwiczeniu zapoznajesz niektóre funkcje obsługiwane przez program **Web Essentials** i **link przeglądarki** , aby ulepszyć prostą stronę quizu.

<a id="Ex1Task1"></a>
#### <a name="task-1---running-the-project-in-multiple-browsers"></a>Zadanie 1 — uruchamianie projektu w wielu przeglądarkach

W tym zadaniu skonfigurujesz aplikację sieci Web tak, aby była uruchamiana w wielu przeglądarkach jednocześnie, co jest przydatne w przypadku testowania między przeglądarkami.

1. Otwórz **Microsoft Visual Studio**.
2. W menu **plik** wybierz polecenie **Otwórz | Projekt/rozwiązanie..** . i przejdź do **Ex1-WorkingwithBrowserLinkandWebEssentials\Begin** w folderze **źródłowym** laboratorium (C:\WebCampsTK\HOL\VSWebTooling\Source). Wybierz pozycję **Rozpocznij. sln** , a następnie kliknij przycisk **Otwórz**.
3. Na pasku narzędzi programu Visual Studio rozwiń menu przeglądarki i wybierz polecenie **Przeglądaj z...** .

    ![Opcja menu Przeglądaj z](visual-studio-2013-web-tools/_static/image1.png "Przeglądaj za pomocą... w menu przeglądarki")

    *Opcja menu Przeglądaj z*
4. W oknie dialogowym **przeglądanie za pomocą** wybierz opcję **Google Chrome** i **Internet Explorer** , przytrzymując wciśnięty klawisz **Ctrl** , a następnie kliknij pozycję **Ustaw jako domyślną**.

    ![Przeglądanie przy użyciu okna dialogowego](visual-studio-2013-web-tools/_static/image2.png "Przeglądanie przy użyciu okna dialogowego")

    *Wybieranie wielu przeglądarek domyślnych*
5. Programy Google Chrome i Internet Explorer powinny teraz być wyświetlane jako domyślne przeglądarki. Kliknij przycisk **Anuluj** , aby zamknąć okno dialogowe.

    ![Google Chrome i Internet Explorer jako przeglądarki domyślne](visual-studio-2013-web-tools/_static/image3.png "Przeglądarki Google Chrome i przeglądarka Internet Explorer domyślne")

    *Google Chrome i Internet Explorer jako przeglądarki domyślne*

    > [!NOTE]
    > Po skonfigurowaniu domyślnej przeglądarki w menu przeglądarki zostanie wybrana opcja **wiele przeglądarek** .
    > 
    > ![Wiele przeglądarek](visual-studio-2013-web-tools/_static/image4.png "Wiele przeglądarek")
6. Naciśnij klawisz **CTRL** + **F5** , aby uruchomić aplikację bez debugowania.
7. Gdy otwierane są obie okna przeglądarki, umieść je powyżej drugiego, aby wyświetlić aktualizacje obu przeglądarek jednocześnie. W przeglądarkach powinna zostać wyświetlona strona sieci Web z prostokątem jasnoniebieskim.

    ![Umieszczenie jednej przeglądarki powyżej drugiej](visual-studio-2013-web-tools/_static/image5.png "Umieszczenie jednej przeglądarki powyżej drugiej")

    *Umieszczenie jednej przeglądarki powyżej drugiej*
8. Nie zamykaj przeglądarek. Będziesz ich używać w następnym zadaniu.

<a id="Ex1Task2"></a>
#### <a name="task-2---using-zen-coding-to-create-html-elements"></a>Zadanie 2 — używanie kodowania Zen do tworzenia elementów HTML

**Kodowanie Zen** jest wtyczką edytora dla wysokiej szybkości kodu HTML, XML, XSL (lub dowolnego innego formatu kodu strukturalnego) do kodowania i edycji. Rdzeń tej wtyczki to zaawansowany aparat skrótu, który umożliwia rozszerzanie wyrażeń — podobnie jak w przypadku selektorów CSS — do kodu HTML. Kodowanie Zen to szybki sposób pisania kodu HTML przy użyciu składni selektora stylów CSS.

W tym ćwiczeniu użyjesz funkcji kodowania Zen dostarczonej przez program Web Essentials, aby wygenerować przyciski HTML reprezentujące opcje pytania.

1. Przełącz się z powrotem do programu Visual Studio.
2. Otwórz plik **index. cshtml** znajdujący się w **widokach** | folderze **głównym** .
3. Zastąp **&lt;!--do zrobienia: Dodaj tutaj opcje--&gt;** komentarz z poniższym kodem, a następnie naciśnij klawisz **Tab**.

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample1.css)]
4. Kod powinien zostać rozszerzony do formatu HTML.

    ![Rozwinięty kod HTML](visual-studio-2013-web-tools/_static/image6.png "Rozwinięty kod HTML")

    *Rozwinięty kod HTML*

    > [!NOTE]
    > Aby dowiedzieć się więcej o składni kodowania Zen, zapoznaj się z następującym [artykułem](http://www.johnpapa.net/zen-coding-in-visual-studio-2012/).
5. Kliknij przycisk **Odśwież połączone przeglądarki** , aby zaktualizować obie przeglądarki.

    ![Odśwież połączone przeglądarki](visual-studio-2013-web-tools/_static/image7.png "Odśwież połączone przeglądarki")

    *Odśwież połączone przeglądarki*

    ![Internet Explorer — Zaktualizowano na stronie cztery przyciski](visual-studio-2013-web-tools/_static/image8.png "Internet Explorer — Zaktualizowano na stronie cztery przyciski")

    *Internet Explorer — Zaktualizowano na stronie cztery przyciski*

    ![Google Chrome — Zaktualizowano za pomocą czterech przycisków](visual-studio-2013-web-tools/_static/image9.png "Google Chrome — Zaktualizowano za pomocą czterech przycisków")

    *Google Chrome — Zaktualizowano za pomocą czterech przycisków*
6. Przełącz się z powrotem do programu Visual Studio.
7. Dodano przyciski do strony, ale nadal trzeba dodać pytanie dotyczące szablonu. W tym celu będziesz używać nowej funkcji w programie Web Essentials o nazwie **Lorem Ipsum Generator**. Znajdź element **DIV** z **przednim**atrybutem **klasy** .
8. Dodaj następujący kod jako pierwszy element podrzędny elementu **DIV**, a następnie naciśnij klawisz **Tab**.

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample2.css)]
9. Kod powinien zostać rozszerzony do formatu HTML.

    ![Lorem Ipsum](visual-studio-2013-web-tools/_static/image10.png "Lorem Ipsum")

    *Lorem Ipsum*

    > [!NOTE]
    > W ramach kodowania Zen można teraz generować kod Lorem Ipsum bezpośrednio w edytorze HTML. Wystarczy wstawić **Lorem** i **kartę** trafień oraz 30 Word Lorem Ipsum tekstu. Na przykład *lorem10* wstawia 10 Lorem Ipsum wyrazów.
10. W górnej części pytania dodasz logo przy użyciu innej nowej funkcji w programie Web Essentials o nazwie **Generator pikseli Lorem**. Dodaj następujący kod jako pierwszy element podrzędny elementu **DIV** z **kontenerem** jako wartość **klasy** , a następnie naciśnij klawisz **Tab**.

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample3.css)]
11. Kod powinien zostać rozszerzony do formatu HTML.

    ![Wygenerowane piksele Lorem](visual-studio-2013-web-tools/_static/image11.png "Wygenerowane piksele Lorem")

    *Wygenerowane piksele Lorem*

    > [!NOTE]
    > W ramach kodowania Zen można również generować kod pikseli Lorem bezpośrednio w edytorze HTML. Po prostu wpisz **PIX-200x200-zwierzęta** i **kartę** trafień oraz tag **IMG** z obrazem 200x200 zwierzęcia. Aby uzyskać więcej informacji, zobacz [Lorem pikseli](http://www.lorempixel.com).
12. Kliknij przycisk **Odśwież połączone przeglądarki** , aby zaktualizować obie przeglądarki.

    ![Internet Explorer — generowany automatycznie obraz i tekst](visual-studio-2013-web-tools/_static/image12.png "Internet Explorer — generowany automatycznie obraz i tekst")

    *Internet Explorer — generowany automatycznie obraz i tekst*

    ![Google Chrome — automatycznie wygenerowany obraz i tekst](visual-studio-2013-web-tools/_static/image13.png "Google Chrome — automatycznie wygenerowany obraz i tekst")

    *Google Chrome — automatycznie wygenerowany obraz i tekst*

    > [!NOTE]
    > Ponieważ obraz jest wybierany losowo podczas dodawania fragmentu kodu, obraz wyświetlany w przeglądarkach może się różnić.
13. Nie zamykaj przeglądarek. Będziesz ich używać w następnym zadaniu.

<a id="Ex1Task3"></a>
#### <a name="task-3---updating-a-style-property"></a>Zadanie 3 — Aktualizowanie właściwości stylu

W tym zadaniu zostanie użyta funkcja **Tryb inspekcji** linku przeglądarki w celu wykrycia dokładnej lokalizacji, w której jest generowany konkretny element dom, a następnie zaktualizowanie właściwości Color tego elementu przy użyciu selektora kolorów dostarczonego przez program Web Essentials.

1. W przeglądarce Internet Explorer naciśnij klawisz **CTRL** + **Alt** + **I** Włącz tryb inspekcji.
2. Przesuń wskaźnik myszy nad jasnoniebieskim obramowaniem, a następnie kliknij przycisk.

    ![Przesuwanie wskaźnika nad jasnoniebieskim obramowaniem](visual-studio-2013-web-tools/_static/image14.png "Przesuwanie wskaźnika nad jasnoniebieskim obramowaniem")

    *Przesuwanie wskaźnika nad jasnoniebieskim obramowaniem*
3. Przełącz się z powrotem do programu Visual Studio. Zwróć uwagę, jak element HTML wybrany w przeglądarce również jest wybierany w edytorze HTML programu Visual Studio.

    ![Wybrany element HTML w edytorze HTML programu Visual Studio](visual-studio-2013-web-tools/_static/image15.png "Wybrany element HTML w edytorze HTML programu Visual Studio")

    *Wybrany element HTML w edytorze HTML programu Visual Studio*
4. Teraz zaktualizujemy klasy CSS z **przodu** , aby zmienić styl wybranego elementu. Aby to zrobić, naciśnij klawisz **CTRL** +  **,** aby otworzyć pole wyszukiwania **Przejdź do** . Wpisz **site. css** i naciśnij klawisz **Enter** , aby otworzyć plik.

    ![Otwieranie pliku site. CSS](visual-studio-2013-web-tools/_static/image16.png "Otwieranie pliku site. CSS")

    *Otwieranie pliku site. CSS*
5. Naciśnij **kombinację klawiszy CTRL** + **F** i Type **. Przerzuć-Container. Front** , aby znaleźć Selektor CSS.
6. Kliknij jasnoniebieski kwadrat we właściwości Border klasy, aby otworzyć selektora kolorów.

    ![Otwieranie selektora kolorów](visual-studio-2013-web-tools/_static/image17.png "Otwieranie selektora kolorów")

    *Otwieranie selektora kolorów*
7. Rozwiń selektor kolorów, klikając przycisk Pagon i wybierz nowy kolor.

    ![Rozszerzanie selektora kolorów](visual-studio-2013-web-tools/_static/image18.png "Rozszerzanie selektora kolorów")

    *Rozszerzanie selektora kolorów*
8. Naciśnij klawisz **CTRL** + **Alt** + **Enter** , aby odświeżyć połączone przeglądarki.
9. Przejdź do programu Internet Explorer i zwróć uwagę na sposób zmiany koloru obramowania.

    ![Internet Explorer — kolor obramowania zaktualizowany](visual-studio-2013-web-tools/_static/image19.png "Internet Explorer — kolor obramowania zaktualizowany")

    *Internet Explorer — kolor obramowania zaktualizowany*
10. Przejdź do przeglądarki Google Chrome i Zauważ, jak zmieniono kolor obramowania.

    ![Google Chrome — Zaktualizowano kolor obramowania](visual-studio-2013-web-tools/_static/image20.png "Google Chrome — Zaktualizowano kolor obramowania")

    *Google Chrome — Zaktualizowano kolor obramowania*
11. Przełącz się z powrotem do programu Visual Studio.
12. Przejdź na koniec pliku **site. css** i naciśnij klawisz **Ctrl** + **F** , aby zlokalizować selektor **. BTN** .
13. Zauważ, że właściwość **-webkit-border-radius** jest podkreślona w kolorze zielonym.

    ![-webkit-border-właściwość usługi RADIUS selektora BTN](visual-studio-2013-web-tools/_static/image21.png "-webkit-border-właściwość usługi RADIUS selektora BTN")

    *-webkit-border-właściwość usługi RADIUS selektora BTN*
14. Umieść karetkę we właściwości **-webkit-border-radius** . Niebieska linia powinna pojawić się poniżej pierwszej litery pierwszego wyrazu właściwości. Jest to **tag inteligentny**.
15. Naciśnij klawisz **CTRL** +  **.** Aby otworzyć menu sugestie i kliknąć przycisk **Dodaj brakującą Właściwość standardową (border-radius)** .

    ![Dodaj brakującą sugestię właściwości standardowych](visual-studio-2013-web-tools/_static/image22.png "Dodaj brakującą sugestię właściwości standardowych")

    *Dodaj brakującą sugestię właściwości standardowych*
16. Właściwość **border-radius** jest automatycznie dodawana do reguły CSS.

    ![Brak dodanej standardowej właściwości](visual-studio-2013-web-tools/_static/image23.png "Brak dodanej standardowej właściwości")

    *Brak dodanej standardowej właściwości*
17. Przesuń wskaźnik myszy na Właściwość **border-radius** , aby wyświetlić **etykietkę narzędzia macierzy przeglądarki**. **Etykietka narzędzia macierzy przeglądarki** pokazuje dostępność właściwości w każdej przeglądarce.

    ![Etykietka narzędzia macierzy przeglądarki](visual-studio-2013-web-tools/_static/image24.png "Etykietka narzędzia macierzy przeglądarki")

    *Etykietka narzędzia macierzy przeglądarki*
18. Zauważ, że wartość właściwości **border-radius** jest nadal podkreślona. Przesuń wskaźnik myszy nad wartość, aby wyświetlić komunikat ostrzegawczy.

    ![Border — ostrzeżenie wartości właściwości usługi RADIUS](visual-studio-2013-web-tools/_static/image25.png "Border — ostrzeżenie wartości właściwości usługi RADIUS")

    *Border — ostrzeżenie wartości właściwości usługi RADIUS*
19. Usuń jednostkę wartości właściwości **border-radius** zgodnie z sugerowaną przez etykietkę narzędzia.
20. Jako **Border-promień** to standardowa właściwość służąca do definiowania sposobu, w jaki zaokrąglenie rogów obramowania jest, można usunąć Właściwość **-webkit-border-radius** i wartość z reguły CSS.
21. Umieść karetkę we właściwości **zawijania słów** i Zauważ, że tag inteligentny pojawia się również poniżej.
22. Otwórz menu, a następnie kliknij pozycję **Dodaj brakujące specyficzne dla dostawcy**.

    ![Dodaj sugestie dotyczące brakujących specyficznych dla dostawców](visual-studio-2013-web-tools/_static/image26.png "Dodaj sugestie dotyczące brakujących specyficznych dla dostawców")

    *Dodaj sugestie dotyczące brakujących specyficznych dla dostawców*
23. Właściwość **-MS-Word-Otocz** jest automatycznie dodawana do reguły CSS.

    ![Dodana właściwość określona przez dostawcę](visual-studio-2013-web-tools/_static/image27.png "Dodana właściwość określona przez dostawcę")

    *Dodana właściwość określona przez dostawcę*

<a id="Ex1Task4"></a>
#### <a name="task-4---updating-the-html-code-from-the-browser"></a>Zadanie 4 — Aktualizowanie kodu HTML z przeglądarki

W tym zadaniu zostanie użyta funkcja **tryb projektowania** linku przeglądarki do edycji obiektu Dom z przeglądarki i przetransferowania zmian do pliku źródłowego HTML w programie Visual Studio.

1. W przeglądarce Google Chrome naciśnij **klawisze CTRL** + **Alt** + **D** , aby włączyć tryb projektowania.
2. Przesuń wskaźnik myszy na **Lorem ipsum dolor sit amet** , a następnie kliknij przycisk.

    ![Edytowanie pytania](visual-studio-2013-web-tools/_static/image28.png "Edytowanie pytania")

    *Edytowanie pytania*
3. Powinien pojawić się kursor. Zamień oryginalny tekst na *to, co będzie wyglądać, gdy zapisuję dłuższe pytanie?* , a następnie naciśnij klawisz **ESC** , aby wyjść z trybu projektowania.

    ![Edytowane pytanie](visual-studio-2013-web-tools/_static/image29.png "Edytowane pytanie")

    *Edytowane pytanie*
4. Przełącz się z powrotem do programu Visual Studio i Otwórz **index. cshtml**, jeśli nie został jeszcze otwarty. Należy zauważyć, że tekst wewnętrzny elementu **&lt;p&gt;** został zaktualizowany.

    ![Zaktualizowane pytanie na stronie HTML](visual-studio-2013-web-tools/_static/image30.png "Zaktualizowane pytanie na stronie HTML")

    *Zaktualizowane pytanie na stronie HTML*

<a id="Ex1Task5"></a>
#### <a name="task-5---reviewing-seo-related-warnings"></a>Zadanie 5 — przeglądanie ostrzeżeń związanych z programem optymalizacji

**Optymalizacja aparatu wyszukiwania** (wyszukiwarki) to proces zwiększania rangi witryny sieci Web na liście wyników aparatu wyszukiwania. Im większa ranga witryny i im większa jest lista, tym więcej odwiedzanych przez tę lokację będzie można znaleźć w tym aparacie wyszukiwania. W programie Web Essentials znajduje się narzędzie analityczne, które bada kod HTML, raportuje znalezione problemy i zapewnia pomoc w rozwiązaniu tych problemów.

1. Przejdź do menu **Widok** , a następnie kliknij przycisk **Lista błędów** , aby otworzyć okno **Lista błędów** .

    ![Lista błędów w menu Widok](visual-studio-2013-web-tools/_static/image31.png "Lista błędów w menu Widok")

    *Lista błędów w menu Widok*
2. Zwróć uwagę, że istnieje ostrzeżenie dotyczące funkcji optymalizacji informującej o braku **&lt;tagu meta&gt;** dla opisu strony. Kliknij dwukrotnie wpis ostrzeżenia funkcji optymalizacji, aby go usunąć.

    ![Okno Lista błędów](visual-studio-2013-web-tools/_static/image32.png "okno listy błędów")

    *Okno Lista błędów*
3. W oknie dialogowym **Web Essentials** kliknij przycisk **tak** , aby wstawić opis &lt;tagu Meta&gt;.

    ![Web Essentials — okno dialogowe](visual-studio-2013-web-tools/_static/image33.png "Web Essentials — okno dialogowe")

    *Web Essentials — okno dialogowe*
4. Edytor dla **\_Layout. cshtml** zostanie otwarty, a tag **&lt;meta&gt;** zostanie automatycznie dodany do sekcji **nagłówkowej** pliku HTML.

    ![Tag meta został automatycznie dodany na stronie _Layout](visual-studio-2013-web-tools/_static/image34.png "Tag meta został automatycznie dodany na stronie _Layout")

    *Meta tag automatycznie dodawany do strony układu \_*
5. Zmień wartość atrybutu **Content** na *GeekQuiz* i Zapisz plik.

<a id="Exercise2"></a>
### <a name="exercise-2-taking-advantage-of-code-snippets-and-intellisense"></a>Ćwiczenie 2: korzystanie z fragmentów kodu i technologii IntelliSense

W programie Web Essentials Edytor HTML został rozszerzony o dodatkowe funkcje. W tym ćwiczeniu zostaną wyświetlone nowe funkcje, które są przydatne podczas opracowywania aplikacji sieci Web.

<a id="Ex2Task1"></a>
#### <a name="task-1---using-intellisense-in-html-documents"></a>Zadanie 1 — Używanie funkcji IntelliSense w dokumentach HTML

Pierwsza Nowa funkcja, która będzie widoczna w tym zadaniu, nazywa się **dynamiczną**technologią IntelliSense. Dynamiczne IntelliSense odczytuje inne Tagi i atrybuty w celu wywnioskowania możliwych identyfikatorów, które będą używane.

W tym zadaniu utworzysz nowy element formularza HTML, który zawiera etykietę i pole wejściowe. Następnie dodasz atrybut **for** do etykiety, aby powiązać ją z danymi wejściowymi. zobaczysz sugestie IntelliSense na podstawie identyfikatorów wejść w zakres.

1. Otwórz **Visual Studio Express 2013 dla sieci Web** i rozwiązania **BEGIN. sln** znajdującego się w folderze **Source/Ex2-TakingAdvantageofCodeSnippetsandIntelliSense/BEGIN** . Alternatywnie możesz kontynuować z rozwiązaniem uzyskanym w poprzednim ćwiczeniu.
2. W **Eksplorator rozwiązań**Otwórz plik **index. cshtml** znajdujący się w folderze **widoki** | **Home** .
3. Dodaj następujący formularz wewnątrz **sekcji&lt;&gt;** elementu.

    (Fragment kodu — *VisualStudio2013WebTooling* - *Ex2* - *form*)

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample4.html)]
4. Tag wejściowy powinien być poprzedzony etykietą z nieprawidłowym opisem pola. Dodaj następującą etykietę przed tagiem wejściowym.

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample5.html)]
5. Atrybut **for** **&lt;etykiety&gt;** określa, który element formularza jest powiązany z etykietą. Wartość atrybutu powinna być taka sama jak identyfikator powiązanego elementu. Dodaj atrybut **for** do **&lt;&gt;etykieta** elementu. Jak pokazano na poniższej ilustracji, nazwa &quot;&quot; wartość pojawia się w polu IntelliSense na podstawie identyfikatora elementów w ramach tego samego zakresu ( **&lt;&gt;formularz** ).

    ![Wyświetlanie identyfikatora w IntelliSense](visual-studio-2013-web-tools/_static/image35.png "Wyświetlanie identyfikatora w IntelliSense")

    *Wyświetlanie identyfikatora w IntelliSense*
6. Usuń ostatnio dodany element **&lt;formularz&gt;** i jego zawartość.

<a id="Ex2Task2"></a>
#### <a name="task-2---using-html-code-snippets"></a>Zadanie 2 — używanie fragmentów kodu HTML

W języku HTML5 wprowadzono więcej niż 25 nowych tagów semantycznych. Program Visual Studio ma już obsługę technologii IntelliSense dla tych tagów, ale Visual Studio 2013 przyspiesza i ułatwia pisanie znaczników przez dodawanie nowych fragmentów kodu. Chociaż te Tagi nie są skomplikowane, składają się z kilku niewielkich subtleties, takich jak dodanie poprawnych rezerw koderów dla znacznika *audio* . W tym zadaniu zobaczysz fragmenty kodu HTML dla tagu audio.

1. W pliku **index. cshtml** wpisz **&lt;aud** wewnątrz **sekcji&lt;&gt;** elementu, jak pokazano na poniższym rysunku.

    ![Wstawianie elementu audio](visual-studio-2013-web-tools/_static/image36.png "Wstawianie elementu audio")

    *Wstawianie elementu audio*
2. Naciśnij dwukrotnie klawisz **Tab** i zwróć uwagę na to, jak poniższy kod został dodany na stronie, a kursor zostanie umieszczony na atrybucie **src** pierwszego źródła.

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample6.html)]

    > [!NOTE]
    > Naciskając klawisz **Tab** dwa razy, fragment kodu zostanie wstawiony. Fragment kodu audio pokazuje standardowe użycie znacznika *audio* z dwoma plikami źródłowymi w celu zapewnienia lepszej obsługi.
3. Usuń drugi wiersz i zaktualizuj Źródło pierwszego wiersza, korzystając z następującego linku do WebCampsTV Katana show: [http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3](http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3). Kod wynikowy jest przedstawiony poniżej.

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample7.html)]

    > [!NOTE]
    > Plik źródłowy *KatanaProject. mp3* jest używany jako przykład. Jeśli wolisz, możesz użyć innego źródła.
4. Naciśnij klawisz **CTRL** + **S** , aby zapisać plik.
5. Naciśnij klawisz **CTRL** + **F5** , aby uruchomić aplikację.
6. Zwróć uwagę, że odtwarzacz audio został dodany do aplikacji.

    ![Odtwarzacz audio w programie Internet Explorer](visual-studio-2013-web-tools/_static/image37.png "Odtwarzacz audio w programie Internet Explorer")

    *Odtwarzacz audio w programie Internet Explorer*

    ![Odtwarzacz audio w przeglądarce Google Chrome](visual-studio-2013-web-tools/_static/image38.png "Odtwarzacz audio w przeglądarce Google Chrome")

    *Odtwarzacz audio w przeglądarce Google Chrome*
7. Nie zamykaj przeglądarek. Będziesz ich używać w następnym zadaniu.

<a id="Ex2Task3"></a>
#### <a name="task-3---using-intellisense-in-javascript-documents"></a>Zadanie 3 — Używanie funkcji IntelliSense w dokumentach JavaScript

W przypadku programu Web Essentials 2013 arkusze stylów i strony HTML tworzą listę identyfikatorów i nazw klas. W tym zadaniu dowiesz się, jak te listy poprawiają obsługę funkcji IntelliSense języka JavaScript w programie Web Essentials 2013.

1. W pliku **index. cshtml** Dodaj następujący kod, aby zdefiniować tag **skryptu** dla kodu JavaScript.

    [!code-cshtml[Main](visual-studio-2013-web-tools/samples/sample8.cshtml)]
2. Dodaj następujący kod wewnątrz tagu **skryptu** , aby zdefiniować przygotowaną funkcję wywołania zwrotnego.

    (Fragment kodu- *VisualStudio2013WebTooling* - *Ex2* - *ReadyFunction*)

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample9.js)]
3. Umieść karetkę w tagu **skryptu** i naciśnij klawisz **Ctrl** +  **.** , aby otworzyć menu sugestii.
4. Kliknij pozycję **Wyodrębnij do pliku**.

    ![Plik JavaScript Wyodrębnij do sugestii plików](visual-studio-2013-web-tools/_static/image39.png "Plik JavaScript Wyodrębnij do sugestii plików")

    *Plik JavaScript Wyodrębnij do sugestii plików*
5. W oknie **Zapisz jako** wybierz folder **skrypty** , Nazwij plik **init. js** , a następnie kliknij przycisk **Zapisz**.

    ![Zapisz jako okno](visual-studio-2013-web-tools/_static/image40.png "Zapisz jako okno")

    *Zapisz jako okno*

    > [!NOTE]
    > Zostanie utworzony plik **init. js** , a zawartość skryptu zostanie przeniesiona do pliku.
    > 
    > ![Plik init. js utworzony za pomocą dołączonej zawartości](visual-studio-2013-web-tools/_static/image41.png "Plik init. js utworzony za pomocą dołączonej zawartości")
    > 
    > *Plik init. js utworzony za pomocą dołączonej zawartości*
6. Otwórz plik **index. cshtml** i sprawdź, czy tag skryptu został zastąpiony odwołaniem do pliku **init. js** .

    ![Dokumentacja języka HTML init. js](visual-studio-2013-web-tools/_static/image42.png "Dokumentacja języka HTML init. js")

    *Dokumentacja języka HTML init. js*
7. Przejdź do **Eksplorator rozwiązań** i Zauważ, że plik **init. js** został uwzględniony automatycznie w rozwiązaniu.

    ![Plik init. js uwzględniony w rozwiązaniu](visual-studio-2013-web-tools/_static/image43.png "Plik init. js uwzględniony w rozwiązaniu")

    *Plik init. js uwzględniony w rozwiązaniu*
8. Przełącz się z powrotem do pliku **init. js** , aby zaktualizować wywołanie zwrotne **funkcji.**
9. Wewnątrz definicji wywołania zwrotnego funkcji, która jest przenoszona do *gotowe*, Dodaj następujący kod, aby uzyskać wszystkie elementy według określonego atrybutu klasy.

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample10.js)]
10. Naciśnij klawisz **CTRL** + **miejsce** między cudzysłowami w wywołaniu funkcji **getElementsByClassName** .

    ![Wyświetlanie technologii IntelliSense dla funkcji getElementsByClassName](visual-studio-2013-web-tools/_static/image44.png "Wyświetlanie technologii IntelliSense dla funkcji getElementsByClassName")

    *Wyświetlanie technologii IntelliSense dla funkcji getElementsByClassName*

    > [!NOTE]
    > Zauważ, że technologia IntelliSense wyświetla klasy zdefiniowane w arkuszach stylów projektu.
11. Zastąp wiersz utworzony przy użyciu następującego kodu.

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample11.js)]
12. Umieść kursor po elemencie " **au** " w cudzysłowie w funkcji **GetElementsByTagName** i naciśnij klawisz **Ctrl** + **miejsce**.

    ![Wyświetlanie funkcji IntelliSense dla metody getElementByTagName](visual-studio-2013-web-tools/_static/image45.png "Wyświetlanie funkcji IntelliSense dla metody getElementByTagName")

    *Wyświetlanie funkcji IntelliSense dla metody getElementsByTagName*
13. Wybierz pozycję **&quot;audio&quot;** z listy i naciśnij klawisz **Enter**. Wyniki są pokazane na poniższym rysunku.

    ![Pobieranie elementów audio](visual-studio-2013-web-tools/_static/image46.png "Pobieranie elementów audio")

    *Pobieranie elementów audio*
14. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy plik **init. js** w folderze **skrypty** i wybierz pozycję **Zminifikować pliki JavaScript** z menu programu **Web Essentials** .

    ![Zminifikować pliki JavaScript](visual-studio-2013-web-tools/_static/image47.png "Zminifikować pliki JavaScript")

    *Zminifikować pliki JavaScript*
15. Po wyświetleniu monitu o włączenie automatycznej minifikacja po zmianie pliku źródłowego kliknij przycisk **tak**.

    ![Włączanie automatycznego ostrzeżenia minifikacja](visual-studio-2013-web-tools/_static/image48.png "Włączanie automatycznego ostrzeżenia minifikacja")

    *Włączanie automatycznego ostrzeżenia minifikacja*

    > [!NOTE]
    > Element **init. min. js** jest tworzony i dodawany jako zależność pliku **init. js** .
    > 
    > ![Plik init. min. js został utworzony](visual-studio-2013-web-tools/_static/image49.png "Plik init. min. js został utworzony")
    > 
    > *Plik init. min. js został utworzony*
16. Otwórz plik **init. min. js** i zwróć uwagę na to, że plik jest zminimalizowanego.

    ![Zawartość pliku init. min. js](visual-studio-2013-web-tools/_static/image50.png "Zawartość pliku init. min. js")

    *Zawartość pliku init. min. js*
17. W pliku **init. js** Dodaj następujący kod poniżej wywołania funkcji **GetElementsByTagName** , aby odtworzyć wszystkie elementy audio.

    (Fragment kodu- *VisualStudio2013WebTooling* - *Ex2* - *PlayAudioElements*)

    [!code-csharp[Main](visual-studio-2013-web-tools/samples/sample12.cs)]
18. Kliknij przycisk **CTRL** + **S** , aby zapisać plik. Ponieważ plik zminimalizowanego jest już otwarty, zostanie wyświetlone okno dialogowe z informacją o tym, że plik został zmodyfikowany poza edytorem źródła. Kliknij przycisk **Tak**.

    ![Ostrzeżenie Microsoft Visual Studio](visual-studio-2013-web-tools/_static/image51.png "Ostrzeżenie Microsoft Visual Studio")

    *Ostrzeżenie Microsoft Visual Studio*
19. Przełącz się z powrotem do pliku **init. min. js** , aby sprawdzić, czy plik został zaktualizowany przy użyciu nowego kodu.

    ![Zaktualizowano plik init. min. js](visual-studio-2013-web-tools/_static/image52.png "Zaktualizowano plik init. min. js")

    *Zaktualizowano plik init. min. js*
20. Kliknij przycisk **Odśwież link do przeglądarki** .
21. Po odświeżeniu obu przeglądarek odtwarzacze audio, które zostały opisane w poprzednim zadaniu, rozpoczną odtwarzanie automatycznie.

    ![Odtwarzacz audio uwzględniony w widoku](visual-studio-2013-web-tools/_static/image53.png "Odtwarzacz audio uwzględniony w widoku")

    *Odtwarzacz audio uwzględniony w widoku*

---

<a id="Summary"></a>
## <a name="summary"></a>Podsumowanie

Dzięki zakończeniu tego praktycznego laboratorium wiesz, jak:

- Korzystaj z nowych funkcji edytora HTML zawartych w programie Web Essentials, takich jak bogate fragmenty kodu HTML5 i kodowanie Zen
- Korzystanie z nowych funkcji edytora CSS zawartych w programie Web Essentials, takich jak selektor kolorów i etykietka narzędzia macierzy przeglądarki
- Korzystanie z nowych funkcji edytora JavaScript zawartych w programie Web Essentials, takich jak Wyodrębnianie do plików i IntelliSense dla wszystkich elementów HTML
- Wymiana danych między przeglądarką i programem Visual Studio przy użyciu linku przeglądarki

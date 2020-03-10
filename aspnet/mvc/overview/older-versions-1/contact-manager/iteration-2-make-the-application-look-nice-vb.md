---
uid: mvc/overview/older-versions-1/contact-manager/iteration-2-make-the-application-look-nice-vb
title: 'Iteracja #2 — sprawianie, że aplikacja wygląda całkiem (VB) | Microsoft Docs'
author: microsoft
description: W tej iteracji ulepszamy wygląd aplikacji, modyfikując domyślną stronę wzorcową widoku MVC ASP.NET i kaskadowy arkusz stylów.
ms.author: riande
ms.date: 02/20/2009
ms.assetid: f65cb436-e493-46fd-9608-384b27385aa1
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-2-make-the-application-look-nice-vb
msc.type: authoredcontent
ms.openlocfilehash: cd392baaefcfc9eef3551bc534e0b912ccd349cc
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78601984"
---
# <a name="iteration-2--make-the-application-look-nice-vb"></a>Iteracja #2 — Zwiększ wygląd aplikacji (VB)

przez [firmę Microsoft](https://github.com/microsoft)

[Pobierz kod](iteration-2-make-the-application-look-nice-vb/_static/contactmanager_2_vb1.zip)

> W tej iteracji ulepszamy wygląd aplikacji, modyfikując domyślną stronę wzorcową widoku MVC ASP.NET i kaskadowy arkusz stylów.

## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Tworzenie aplikacji Contact Management ASP.NET MVC (VB)

W tej serii samouczków tworzymy całą aplikację do zarządzania kontaktami od początku do końca. Aplikacja Contact Manager umożliwia przechowywanie informacji kontaktowych — nazw, numerów telefonów i adresów e-mail — w celu uzyskania listy osób.

Aplikacja została utworzona przez wiele iteracji. W przypadku każdej iteracji stopniowo ulepszamy aplikację. Celem tej wielu iteracji jest umożliwienie zrozumienia przyczyny każdej zmiany.

- Iteracja #1 — Utwórz aplikację. W pierwszej iteracji tworzymy Menedżera kontaktów w najprostszy sposób. Dodaliśmy obsługę podstawowych operacji bazy danych: Tworzenie, odczytywanie, aktualizowanie i usuwanie (CRUD).

- Iteracja #2 — sprawia, że aplikacja wygląda całkiem. W tej iteracji ulepszamy wygląd aplikacji, modyfikując domyślną stronę wzorcową widoku MVC ASP.NET i kaskadowy arkusz stylów.

- Iteracja #3 — Dodawanie walidacji formularza. W trzeciej iteracji zostanie dodana podstawowa Walidacja formularza. Uniemożliwiamy użytkownikom przesyłanie formularza bez wykonywania wymaganych pól formularza. Sprawdzamy również adresy e-mail i numery telefonów.

- Iteracja #4 — możliwość swobodnego łączenia aplikacji. W tej czwartej iteracji wykorzystujemy kilka wzorców projektowych oprogramowania, aby ułatwić konserwację i modyfikowanie aplikacji Contact Manager. Na przykład Refaktoryzacja naszej aplikacji używa wzorca repozytorium i wzorca iniekcji zależności.

- Iteracja #5 — Tworzenie testów jednostkowych. W piątej iteracji upraszczamy obsługę i modyfikację naszej aplikacji przez dodanie testów jednostkowych. Tworzymy klasy modelu danych i kompilujemy testy jednostkowe dla naszych kontrolerów i logiki walidacji.

- Iteracja #6 — Użyj programowania opartego na testach. W tej szóstej iteracji Dodaliśmy nowe funkcje do naszej aplikacji, pisząc testy jednostkowe jako pierwsze i pisząc kod na testach jednostkowych. W tej iteracji dodamy grupy kontaktów.

- Iteracja #7 — Dodawanie funkcji AJAX. W siódmej iteracji poprawimy czas reakcji i wydajność naszej aplikacji przez dodanie obsługi technologii AJAX.

## <a name="this-iteration"></a>Ta iteracja

Celem tej iteracji jest poprawa wyglądu aplikacji Contact Manager. Obecnie Menedżer kontaktów używa domyślnej strony wzorcowej widoku MVC ASP.NET i kaskadowego arkusza stylów (patrz rysunek 1). Nie ma to nieprawidłowego wyglądu, ale nie chcę, aby Menedżer kontaktów mógł wyglądać podobnie jak każda inna witryna sieci Web ASP.NET MVC. Chcę zamienić te pliki na pliki niestandardowe.

[![okno dialogowe Nowy projekt](iteration-2-make-the-application-look-nice-vb/_static/image1.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image1.png)

**Ilustracja 01**: domyślny wygląd aplikacji ASP.NET MVC ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-2-make-the-application-look-nice-vb/_static/image2.png))

W tej iteracji omówiono dwa podejścia do ulepszania wizualnego projektowania naszej aplikacji. Po pierwsze pokazujemy, jak korzystać z Galerii projektów ASP.NET MVC, aby pobrać bezpłatny szablon projektu ASP.NET MVC. Galeria projektów ASP.NET MVC umożliwia tworzenie profesjonalnych aplikacji sieci Web bez konieczności wykonywania żadnych zadań.

Nie postanowiono używać szablonu z Galerii projektów ASP.NET MVC dla aplikacji Contact Manager. Zamiast tego mam projekt niestandardowy utworzony przez profesjonalne firmy projektowe. W drugiej części tego samouczka wyjaśniono, jak pracowałem z profesjonalnej firmy projektowej w celu utworzenia ASP.NET końcowego projektu MVC.

## <a name="the-aspnet-mvc-design-gallery"></a>Galeria projektów ASP.NET MVC

Galeria projektów ASP.NET MVC jest bezpłatnym zasobem udostępnianym przez firmę Microsoft. Galeria ASP.NET MVC znajduje się pod następującym adresem:

[https://www.asp.net/mvc/gallery](https://www.asp.net/mvc/gallery)

Galeria projektów ASP.NET MVC zawiera kolekcję bezpłatnych projektów witryn sieci Web, które zostały utworzone w celu użycia w projekcie ASP.NET MVC. Projekty są przekazywane przez członków społeczności. Osoby odwiedzające galerię mogą głosować na ulubione projekty (patrz rysunek 2).

[![okno dialogowe Nowy projekt](iteration-2-make-the-application-look-nice-vb/_static/image2.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image3.png)

**Ilustracja 02**. galeria projektów ASP.NET MVC ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-2-make-the-application-look-nice-vb/_static/image4.png))

Gdy napiszę ten samouczek, najpopularniejszym projektem w galerii jest projekt o nazwie Hauser. Tego projektu można użyć dla projektu ASP.NET MVC, wykonując następujące czynności:

1. Kliknij przycisk **Pobierz** , aby pobrać plik. zip z października do komputera.
2. Kliknij prawym przyciskiem myszy pobrany plik w październiku. zip, a następnie kliknij przycisk **odblokowywania** (patrz rysunek 3).
3. Rozpakuj plik do folderu o nazwie październik.
4. Zaznacz wszystkie pliki w folderze DesignTemplate, które znajdują się w folderze październik, kliknij plik prawym przyciskiem myszy, a następnie wybierz opcję menu **Kopiuj**.
5. Kliknij prawym przyciskiem myszy węzeł ContactManager projektu w oknie Eksplorator rozwiązań programu Visual Studio i wybierz opcję menu **Wklej** (zobacz rysunek 4).
6. Wybierz opcję menu programu Visual Studio **Edycja, Znajdź i Zamień, szybkie zastępowanie** i Zastąp *[ProjectName]* elementem *ContactManager* (patrz rysunek 5).

[![okno dialogowe Nowy projekt](iteration-2-make-the-application-look-nice-vb/_static/image3.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image5.png)

**Ilustracja 03**: Odblokowywanie pliku pobranego z sieci Web ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](iteration-2-make-the-application-look-nice-vb/_static/image6.png))

[![okno dialogowe Nowy projekt](iteration-2-make-the-application-look-nice-vb/_static/image4.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image7.png)

**Ilustracja 04**: Zastępowanie plików w Eksplorator rozwiązań ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-2-make-the-application-look-nice-vb/_static/image8.png))

[![okno dialogowe Nowy projekt](iteration-2-make-the-application-look-nice-vb/_static/image5.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image9.png)

**Ilustracja 05**: zamiana [ProjectName] z elementem ContactManager ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](iteration-2-make-the-application-look-nice-vb/_static/image10.png))

Po wykonaniu tych kroków aplikacja sieci Web będzie używać nowego projektu. Strona na rysunku 6 ilustruje wygląd aplikacji Contact Manager w projekcie z października.

[![okno dialogowe Nowy projekt](iteration-2-make-the-application-look-nice-vb/_static/image6.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image11.png)

**Ilustracja 06**: ContactManager z szablonem październik ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](iteration-2-make-the-application-look-nice-vb/_static/image12.png))

## <a name="creating-a-custom-aspnet-mvc-design"></a>Tworzenie niestandardowego projektu ASP.NET MVC

Galeria projektów ASP.NET MVC ma dobry wybór w różnych stylach projektowych. Galeria zawiera bezproblemowego sposób dostosowywania wyglądu aplikacji ASP.NET MVC. Oczywiście Galeria ma duże zalety w pełni bezpłatnej.

Jednak może być konieczne utworzenie całkowicie unikatowego projektu dla witryny sieci Web. W takim przypadku warto skontaktować się z firmą projektu witryny sieci Web. Podjęto decyzję o tym, aby zaprojektować aplikację Contact Manager.

Po przypisaniu Menedżera kontaktów do #1 iteracji i wysłaniu projektu do firmy projektowej. Nie były one własnością programu Visual Studio (Shame na nich!), ale nie wystąpiły problemy. Mogli bezpłatnie pobrać program Microsoft Visual Web Developer z witryny internetowej [https://www.asp.net](https://www.asp.net) i otworzyć aplikację Contact Manager w programie Visual Web Developer. W ciągu kilku dni wyprodukował projekt na rysunku 7.

[![okno dialogowe Nowy projekt](iteration-2-make-the-application-look-nice-vb/_static/image7.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image13.png)

**Ilustracja 07**: projekt programu ASP.NET MVC Contact Manager ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-2-make-the-application-look-nice-vb/_static/image14.png))

Nowy projekt składa się z dwóch głównych plików: nowy plik kaskadowego arkusza stylów i nowy plik strony głównej widoku. Strona wzorcowa widoku zawiera układ i zawartość udostępnioną dla widoków w aplikacji ASP.NET MVC. Na przykład strona wzorcowa widoku zawiera nagłówek, karty nawigacji i stopkę, które są wyświetlane na rysunku 7. Nadpisałem istniejącą stronę wzorcową witryny. Master w folderze Views\Shared z nowym plikiem programu site. Master w firmie projektowej.

Firma projektująca również utworzyła nowy kaskadowy arkusz stylów i zestaw obrazów. Te nowe pliki zostały umieszczone w folderze zawartości i nadpisano istniejący plik. css. Całą zawartość statyczną należy umieścić w folderze zawartości.

Należy zauważyć, że nowy projekt programu Contact Manager zawiera obrazy do edytowania i usuwania kontaktów. Obraz edytowania i usuwania pojawia się obok każdego kontaktu w tabeli HTML kontaktów.

Pierwotnie te linki, które były renderowane przy użyciu kodu HTML. W przypadku tego pomocnika:

[!code-aspx[Main](iteration-2-make-the-application-look-nice-vb/samples/sample1.aspx)]

Metoda html. ActionLink () nie obsługuje obrazów (Metoda HTML koduje tekst łącza ze względów bezpieczeństwa). W związku z tym zamieniono wywołania na HTML. ActionLink () z wywołaniami do adresu URL. akcja () w następujący sposób:

[!code-aspx[Main](iteration-2-make-the-application-look-nice-vb/samples/sample2.aspx)]

Metoda html. ActionLink () renderuje całe hiperłącze HTML. Metoda URL. Action (), z drugiej strony, renderuje tylko adres URL bez tagu &lt;&gt;.

Należy zauważyć, że nowy projekt zawiera zarówno zaznaczone, jak i niezaznaczone karty. Na przykład na rysunku 8 jest zaznaczona karta **Utwórz nowy kontakt** , a karta **Moje kontakty** nie jest zaznaczona.

[![okno dialogowe Nowy projekt](iteration-2-make-the-application-look-nice-vb/_static/image8.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image15.png)

**Ilustracja 08**: wybrane i niewybrane karty ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](iteration-2-make-the-application-look-nice-vb/_static/image16.png))

Aby obsłużyć renderowanie zaznaczonych i niezaznaczonych kart, utworzono niestandardowe pomocnik HTML o nazwie MenuItemHelper. Ta metoda pomocnika renderuje tag &lt;li&gt; lub &lt;li Class = "Selected&gt;", w zależności od tego, czy bieżący kontroler i akcja odpowiadają kontrolerowi i nazwie akcji przesłanej do pomocnika. Kod dla MenuItemHelper znajduje się na liście 1.

**Lista 1 – Helpers\MenuItemHelper.vb**

[!code-vb[Main](iteration-2-make-the-application-look-nice-vb/samples/sample3.vb)]

MenuItemHelper używa klasy TagBuilder wewnętrznie do kompilowania tagu HTML &lt;li&gt;. Klasa TagBuilder jest bardzo użyteczną klasą narzędzi, której można użyć zawsze, gdy trzeba utworzyć nowy tag HTML. Obejmuje ona metody dodawania atrybutów, dodawania klas CSS, generowania identyfikatorów i modyfikowania tagów języka HTML w kodzie wewnętrznym.

## <a name="summary"></a>Podsumowanie

W tej iteracji Ulepszono projekt wizualizacji aplikacji ASP.NET MVC. Najpierw wprowadzono do Galerii projektów ASP.NET MVC. Wiesz już, jak pobrać bezpłatne szablony projektowe z Galerii projektów ASP.NET MVC, których można używać w aplikacjach ASP.NET MVC.

Następnie omawiamy, jak można utworzyć projekt niestandardowy, modyfikując domyślny plik kaskadowego arkusza stylów i plik strony widoku głównego. Aby można było obsługiwać nowy projekt, musiałmy wprowadzić drobne zmiany w naszej aplikacji menedżera kontaktów. Na przykład dodaliśmy nowy pomocnik HTML o nazwie MenuItemHelper, który wyświetla zaznaczone i niezaznaczone karty.

W następnej iteracji zajmiemy się bardzo ważnym tematem weryfikacji. Dodamy kod weryfikacyjny do naszej aplikacji, tak aby użytkownik nie mógł utworzyć nowego kontaktu bez dostarczania wymaganych wartości, takich jak imię i nazwisko osoby.

> [!div class="step-by-step"]
> [Poprzednie](iteration-1-create-the-application-vb.md)
> [dalej](iteration-3-add-form-validation-vb.md)

---
uid: mvc/overview/older-versions-1/contact-manager/iteration-2-make-the-application-look-nice-vb
title: 'Iteracja #2 — wprowadzić wyglądu nieuprzywilejowany (VB) aplikacji | Dokumentacja firmy Microsoft'
author: microsoft
description: W tej iteracji możemy poprawić wygląd aplikacji przez zmodyfikowanie domyślnych strony wzorcowej widoku platformy ASP.NET MVC i kaskadowych arkuszy stylów.
ms.author: riande
ms.date: 02/20/2009
ms.assetid: f65cb436-e493-46fd-9608-384b27385aa1
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-2-make-the-application-look-nice-vb
msc.type: authoredcontent
ms.openlocfilehash: 21f7974fe066543d6db1d17d462398a998d0171e
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/17/2019
ms.locfileid: "59382580"
---
# <a name="iteration-2--make-the-application-look-nice-vb"></a>Iteracja #2 — zastosować Szukaj nieuprzywilejowany (VB)

przez [firmy Microsoft](https://github.com/microsoft)

[Pobierz program Code](iteration-2-make-the-application-look-nice-vb/_static/contactmanager_2_vb1.zip)

> W tej iteracji możemy poprawić wygląd aplikacji przez zmodyfikowanie domyślnych strony wzorcowej widoku platformy ASP.NET MVC i kaskadowych arkuszy stylów.


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Tworzenie aplikacji zarządzania kontaktami platformy ASP.NET MVC (VB)
  

W tej serii samouczków wbudowujemy całej aplikacji zarządzania skontaktuj się z od początku do zakończenia. Aplikacja Contact Manager umożliwia przechowywanie informacji kontaktowych — nazwy, telefonu liczb i ich adresy e-mail — Aby uzyskać listę osób.

Firma Microsoft tworzy aplikację za pośrednictwem wiele iteracji. Z każdą iteracją można stopniowo ulepszyć aplikację. Celem tego wielu podejścia iteracji jest, aby umożliwić Ci zrozumienie przyczyn wprowadzenia poszczególnych zmian.

- Iteracja #1 — Tworzenie aplikacji. W pierwszej iteracji utworzymy Contact Manager w najprostszym sposobem możliwe. Dodano obsługę dla operacji podstawowej bazy danych: Tworzenia, odczytu, aktualizacji i usuwania (CRUD).

- Iteracja #2 — należy wyglądu nieuprzywilejowany aplikacji. W tej iteracji możemy poprawić wygląd aplikacji przez zmodyfikowanie domyślnych strony wzorcowej widoku platformy ASP.NET MVC i kaskadowych arkuszy stylów.

- Iteracja #3 — Dodawanie weryfikacji formularza. W trzecim iteracji dodamy weryfikacji formularza podstawowego. Firma Microsoft ochronić przed przesłaniem formularza nie kończą działania wymaganych pól formularza. Możemy zweryfikować adresy e-mail oraz numerów telefonów.

- Iteracja 4 # — należy luźne sprzężenie aplikacji. W tym czwarty iteracji możemy korzystać z kilku wzorców projektowych oprogramowania, aby ułatwić konserwację i modyfikowanie aplikacji Contact Manager. Na przykład możemy refaktoryzować naszej aplikacji do korzystania z wzorca repozytorium i wzorzec iniekcji zależności.

- Iteracja #5 — Tworzenie testów jednostkowych. W piątej iteracji ułatwiamy naszej aplikacji ułatwia konserwację i modyfikowanie, dodając testów jednostkowych. Firma Microsoft testowanie naszych zajęć modelu danych i tworzenie testów jednostkowych dla naszych kontrolery i logikę weryfikacji.

- Iteracja #6 — korzystanie z projektowania opartego na testach. W tym szóstego iteracji dodamy nowe funkcje do naszej aplikacji, najpierw pisanie testów jednostkowych i pisanie kodu dla testów jednostkowych. W tym iteracji dodamy grup kontaktów.

- Iteracja #7 — dodawanie funkcji Ajax. W siódmej iteracji można ulepszyć czas odpowiedzi i wydajności naszych aplikacji przez dodanie obsługi technologii AJAX.

## <a name="this-iteration"></a>Tej iteracji

Celem tej iteracji jest poprawić wygląd aplikacji Contact Manager. Obecnie Contact Manager używa domyślnej strony wzorcowej widoku platformy ASP.NET MVC i arkusza stylów kaskadowych (patrz rysunek 1). Te don t wygląd zły, ale nie chcę t ma Contact Manager wyglądały podobnie jak każdej innej platformy ASP.NET MVC witryny sieci Web. Chcę zastąpić te pliki niestandardowych plików.


[![Okno dialogowe Nowy projekt](iteration-2-make-the-application-look-nice-vb/_static/image1.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image1.png)

**Rysunek 01**: Domyślny wygląd aplikacji ASP.NET MVC ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-2-make-the-application-look-nice-vb/_static/image2.png))


W tej iteracji I omówiono w nim dwa podejścia do poprawy wyglądu w naszej aplikacji. Po pierwsze I pokazują, jak korzystać z zalet galerii projektu MVC programu ASP.NET, aby pobrać bezpłatny szablon projektu platformy ASP.NET MVC. Galeria projektów MVC ASP.NET umożliwia tworzenie aplikacji sieci professional web bez wykonywania pracy.

Czy mogę zdecydowała się bez użycia szablonu z Galerii projektów MVC ASP.NET dla aplikacji Contact Manager. Zamiast tego miałem projektów niestandardowych utworzonych przez przedsiębiorstwo profesjonalnego projektu. W drugiej części w tym samouczku opisano sposób pracy z firmą profesjonalnego projektu do utworzenia ostatecznego projektu ASP.NET MVC.

## <a name="the-aspnet-mvc-design-gallery"></a>The ASP.NET MVC Design Gallery

Galeria projektów platformy ASP.NET MVC jest bezpłatnym zasobem obsługiwane przez firmę Microsoft. Galeria MVC ASP.NET znajduje się pod następującym adresem:

[https://www.asp.net/mvc/gallery](https://www.asp.net/mvc/gallery)

Galeria projektów platformy ASP.NET MVC obsługuje kolekcję projektów bezpłatne witryny sieci Web, które zostały utworzone specjalnie do korzystanie w projekcie ASP.NET MVC. Projekty są przekazywane przez członków społeczności. Osoby odwiedzające galerii można Zagłosuj na ich ulubionych projekty (patrz rysunek 2).


[![Okno dialogowe Nowy projekt](iteration-2-make-the-application-look-nice-vb/_static/image2.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image3.png)

**Rysunek 02**: Galeria projektów platformy ASP.NET MVC ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-2-make-the-application-look-nice-vb/_static/image4.png))


Pisania tego samouczka, najbardziej popularnych projektów w galerii jest projekt o nazwie października David Hauser. W tym projekcie można użyć w projekcie ASP.NET MVC, wykonując następujące czynności:

1. Kliknij przycisk **Pobierz** przycisk, aby pobrać plik October.zip do komputera.
2. Kliknij prawym przyciskiem myszy pobrany plik October.zip, a następnie kliknij przycisk **odblokowanie** przycisku (zobacz rysunek 3).
3. Rozpakuj plik do folderu o nazwie października.
4. Zaznacz wszystkie pliki w folderze DesignTemplate znajdujące się w folderze października, kliknij prawym przyciskiem myszy pliki i wybierz opcję menu **kopiowania**.
5. Kliknij prawym przyciskiem myszy węzeł projektu ContactManager w oknie Eksploratora rozwiązań w usłudze Visual Studio i wybierz opcję menu **Wklej** (zobacz rysunek 4).
6. Wybierz opcję menu programu Visual Studio **edycji, Znajdź i Zamień, szybkiego zamieniania** i Zastąp *[Nazwamojegoprojektu]* z *ContactManager* (zobacz rysunek 5).


[![Okno dialogowe Nowy projekt](iteration-2-make-the-application-look-nice-vb/_static/image3.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image5.png)

**Rysunek 03**: Odblokowanie pliku pobranego z Internetu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-2-make-the-application-look-nice-vb/_static/image6.png))


[![Okno dialogowe Nowy projekt](iteration-2-make-the-application-look-nice-vb/_static/image4.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image7.png)

**Rysunek 04**: Zastąpienie plików w Eksploratorze rozwiązań ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-2-make-the-application-look-nice-vb/_static/image8.png))


[![Okno dialogowe Nowy projekt](iteration-2-make-the-application-look-nice-vb/_static/image5.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image9.png)

**Rysunek 05**: Zastąpienie [nazwa_projektu] ContactManager ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-2-make-the-application-look-nice-vb/_static/image10.png))


Po wykonaniu tych kroków, aplikacji sieci web użyje nowy projekt. Strona na rysunku 6 przedstawia wygląd aplikacji Contact Manager z projektem października.


[![Okno dialogowe Nowy projekt](iteration-2-make-the-application-look-nice-vb/_static/image6.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image11.png)

**Rysunek 06**: ContactManager za pomocą szablonu października ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-2-make-the-application-look-nice-vb/_static/image12.png))


## <a name="creating-a-custom-aspnet-mvc-design"></a>Tworzenie projektu MVC ASP.NET niestandardowe

Galeria projektów platformy ASP.NET MVC ma dobry wybór style innego projektu. Galeria umożliwia łatwe sposób, aby dostosować wygląd aplikacji ASP.NET MVC. I oczywiście Galeria ma ogromne korzyści z bycia całkowicie bezpłatne.

Jednak może być konieczne tworzenie projektu całkowicie unikatowy dla witryny sieci Web. W takim przypadku warto pracować w firmie projektowej witryny sieci Web. Czy mogę zdecydowała się takiego podejścia do projektowania aplikacji Contact Manager.

Czy mogę Konfigurowanie menedżera kontaktu z iteracji nr 1 i przesłania projektu do firmie projektowej. Nie będzie należał do programu Visual Studio (shame na nich!), ale które nie powodują problemu. Byli w stanie bezpłatnie pobrać z programu Microsoft Visual Web Developer [ https://www.asp.net ](https://www.asp.net) witryny sieci Web i otwórz aplikacji Contact Manager w Visual Web Developer. W kilka dni ich było generowane projektu na rysunku 7.


[![Okno dialogowe Nowy projekt](iteration-2-make-the-application-look-nice-vb/_static/image7.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image13.png)

**Rysunek 07**: Projekt platformy ASP.NET MVC Contact Manager ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-2-make-the-application-look-nice-vb/_static/image14.png))


Nowy projekt składa się z dwóch głównych plików: nowego pliku arkusza stylów kaskadowych i nowy plik strony wzorcowej widoku. Na stronie wzorcowej widoku zawiera układ i zawartość udostępniona w widokach w aplikacji ASP.NET MVC. Na przykład strony wzorcowej widoku zawiera nagłówek, karty nawigacji i stopki, które pojawiają się na rysunku 7. Czy mogę zastąpią istniejące strony wzorcowej widoku w Site.Master w folderze Views\Shared za pomocą nowego pliku Site.Master w firmie projektowej

Firmie projektowej także utworzenie nowego arkusza stylów kaskadowych i zestawu obrazów. Czy mogę umieścić te nowe pliki w folderze, zawartości i zastąpią istniejące pliku Site.css. W folderze zawartości, należy umieścić wszystkie zawartości statycznej.

Zwróć uwagę, że nowy projekt dla Contact Manager obrazów edytowanie i usuwanie kontaktów. Edytowanie i usuwanie obrazu są wyświetlane obok każdego kontaktu w tabeli HTML kontaktów.

Początkowo te linki, które renderowany kod HTML. Pomocnik ActionLink() następująco:

[!code-aspx[Main](iteration-2-make-the-application-look-nice-vb/samples/sample1.aspx)]

Metoda Html.ActionLink() nie obsługuje obrazy (metoda HTML koduje tekst łącza ze względów bezpieczeństwa). W związku z tym czy zastąpione wywołania Html.ActionLink() wywołania Url.Action() następująco:

[!code-aspx[Main](iteration-2-make-the-application-look-nice-vb/samples/sample2.aspx)]

Metoda Html.ActionLink() powoduje wyświetlenie całej hiperłącze HTML. Metoda Url.Action() renderuje z drugiej strony, po prostu adresu URL bez &lt;&gt; tagu.

Zwróć uwagę, co więcej, że nowy projekt zawiera karty zaznaczone i niezaznaczone. Na przykład na rysunku 8 **Utwórz nowy kontakt** wybrana jest karta i **Moje kontakty** karta nie jest zaznaczone.


[![Okno dialogowe Nowy projekt](iteration-2-make-the-application-look-nice-vb/_static/image8.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image15.png)

**Rysunek 08**: Zaznaczony i niezaznaczony karty ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-2-make-the-application-look-nice-vb/_static/image16.png))


Aby zapewnić obsługę renderowania karty zaznaczone i niezaznaczone, utworzony niestandardowego pomocnika HTML o nazwie MenuItemHelper. Ta metoda pomocnika renderuje albo &lt;li&gt; tag lub &lt;klasy li = "wybrane"&gt; tagu w zależności od tego, czy bieżący kontroler i Akcja odnosi się do nazwy akcji i kontrolerów, przekazany do elementu pomocniczego. Kod MenuItemHelper znajduje się w ofercie 1.

**Wyświetlanie listy 1 – Helpers\MenuItemHelper.vb**

[!code-vb[Main](iteration-2-make-the-application-look-nice-vb/samples/sample3.vb)]

MenuItemHelper używa klasa TagBuilder wewnętrznie w celu tworzenia &lt;li&gt; tagu HTML. Klasa TagBuilder jest klasą bardzo przydatne narzędzie, którego można użyć, gdy zajdzie taka potrzeba utworzenia nowego tagu HTML. Zawiera metody dodawania atrybutów, dodawanie klas CSS, generowania identyfikatorów i modyfikowania znaczników s wewnętrznego kodu HTML.

## <a name="summary"></a>Podsumowanie

W tym iteracji poprawiliśmy projektowania wizualnego naszej aplikacji ASP.NET MVC. Po pierwsze zostały wprowadzone do Galerii projektów platformy ASP.NET MVC. Pokazaliśmy ci, jak pobrać szablony projektu bezpłatne z Galerii projektów platformy ASP.NET MVC, używanego w aplikacjach ASP.NET MVC.

Następnie omówiono sposób tworzenia projektów niestandardowych przez zmodyfikowanie pliku arkusza stylów kaskadowych domyślne i plik stronicowania widoku głównego. Aby zapewnić obsługę nowego projektu, musimy wprowadzić pewne drobne zmiany do naszej aplikacji Contact Manager. Na przykład dodaliśmy nowe pomocnika kodu HTML o nazwie MenuItemHelper, który wyświetla karty zaznaczone i niezaznaczone.

W następnej iteracji firma Microsoft czoła bardzo ważne przedmiotem sprawdzania poprawności. Możemy dodać kod sprawdzania poprawności do naszej aplikacji, tak aby użytkownik nie można utworzyć nowego kontaktu bez podawania najpierw wymagane wartości, takie jak osoba s i nazwisko.

> [!div class="step-by-step"]
> [Poprzednie](iteration-1-create-the-application-vb.md)
> [dalej](iteration-3-add-form-validation-vb.md)
